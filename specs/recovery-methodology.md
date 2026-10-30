# Purged audio recovery methodology

## Background and problem

During major version updates, game developers routinely purge past limited-time event audio packages (`.pck`) from the live client installation to conserve player disk space. When reverse engineering or extracting assets for a specific character or entity, these event dialogue files appear in internal engine mappings (such as `hk4e.map` or configuration excel sheets) but are absent from the local installation.

For Faruzan (`AvatarId: 10000076`), 259 event voice lines were cataloged in `hk4e.map` but missing from the version 7.1 live client disk.

## Provenance rules and constraints

1. **Strict first-party provenance.** Third-party web scrapers, fan wikis, and re-encoded community rips are prohibited. Assets must originate from the game binary or official content delivery endpoints.
2. **Disk boundaries.** The target archive `Audio_Japanese_3.6.0.zip` is 12,412,394,814 bytes (12.41 GB). Downloading the full 12.4 GB archive for ~25 MB of target voice lines violates workspace storage guidelines and wastes network bandwidth.
3. **Deterministic verification.** Every extracted file must be verified with cryptographic SHA-256 hashes and audio format magic byte inspection (`RIFF`).

---

## Technical methodology: remote HTTP range streaming

Instead of downloading the 12.4 GB archive, the extraction pipeline uses HTTP Range requests (`Range: bytes=start-end`) to stream only the metadata and target file payloads directly across three stages.

### Stage A: Remote Zip64 central directory reading

Standard ZIP and Zip64 archives store their Central Directory at the end of the file.

1. **Locate the Zip64 End of Central Directory (EOCD):**
   Send a range request for the last 1,024 bytes of the archive (`bytes=12412393790-12412394813`).
   Find the Zip64 locator signature `PK\x06\x07` to read the offset of the Zip64 EOCD record (`12412394716`).

2. **Read the Zip64 EOCD record:**
   Fetch the 56-byte record at offset `12412394716` to retrieve:
   - Total archive entries: `113`
   - Central directory size: `17,494` bytes (~17 KB)
   - Central directory offset: `12412377222`

3. **Fetch the entire central directory:**
   A single range request of 17 KB (`bytes=12412377222-12412394715`) yields the file name, compression method, uncompressed size, and local header offset for every file in the 12.4 GB package.

```python
# Central directory entry parsing
sig, flags, comp_method, mtime, mdate, crc, comp_sz, uncomp_sz, fn_len, extra_len, comment_len, disk, int_attr, ext_attr, local_hdr_off = struct.unpack('<HHHHHHIIIHHHHHII', data[pos+4:pos+46])
```

### Stage B: Exploiting uncompressed storage (`comp_method: 0`)

Inspection of the central directory revealed that all `.pck` files inside `Audio_Japanese_3.6.0.zip` are stored with compression method `0` (STORED, uncompressed).

Because the `.pck` containers are uncompressed inside the zip:
- The byte stream inside the zip matches the exact byte sequence of the standalone `.pck` file.
- The absolute zip offset of any file inside a `.pck` container is:
  `abs_zip_offset = local_header_offset + 30 + fn_len + extra_len + pck_file_offset`

### Stage C: Wwise AKPK table of contents inspection

Audiokinetic Wwise package files (`AKPK`) structure their header into dedicated sectors:

1. **Read first 28 bytes of each `.pck`:**
   Fetch `bytes={data_start}-{data_start+27}`:
   ```python
   header_size, flag, lang_size, bnk_size, stream_size, ext_size = struct.unpack('<IIIIII', data[4:28])
   ```
2. **Locate the external streams table:**
   The table of contents is positioned at `table_start = 28 + lang_size + bnk_size + stream_size`.
   Fetch only `4 + ext_size` bytes (`Range: bytes={data_start + table_start}-{data_start + table_start + 4 + ext_size - 1}`).
3. **Parse stream records:**
   Each record is 24 bytes:
   ```python
   fid, block_size, file_size, file_offset, lang_id = struct.unpack('<QIIII', data_table[i*24:(i+1)*24])
   ```
4. Match `fid` against the unresolved target set from `hk4e.map`.

### Stage D: Targeted payload extraction and verification

For each matched audio track:
1. Issue an HTTP Range request for the exact bytes:
   `Range: bytes={abs_zip_offset}-{abs_zip_offset + file_size - 1}`
2. Validate the first 4 bytes match the `RIFF` container signature.
3. Write the payload directly into `assets/audio/VO_EQ/<virtual_path>.wem`.

---

## Network and environment optimization rules

When interacting with HoYoverse CloudFront and AliyunOSS content delivery networks:

1. **Force IPv4 resolution:**
   On Windows environments, default dual-stack socket resolution can cause 10-second DNS timeouts on `autopatchhk.yuanshen.com` if IPv6 routing is inactive. Override `socket.getaddrinfo` to return `AF_INET` addresses only.
2. **Disable persistent keep-alive:**
   Send `Connection: close` in request headers. CloudFront throttles rapid sequential range requests on reused keep-alive connections.
3. **Use realistic User-Agent:**
   Set a browser user-agent string (such as `Mozilla/5.0`) to avoid default urllib blocklists.
4. **Controlled concurrency:**
   Limit parallel threads to 4-6 workers. Excessive concurrency triggers HTTP 429 or read timeouts on CDN edge nodes.

---

## Faruzan recovery audit results

- **Total cataloged missing lines:** 259
- **Event scope breakdown:**
  - **Version 3.6 event ("A Parade of Providence", `XYJEQ`):** 128 lines.
    - `XYJEQ001`: 65 lines
    - `XYJEQ002`: 35 lines
    - `xyjeq002`: 8 lines
    - `XYJEQ005`: 11 lines
    - Free dialogue (`vo_dialog_XYJEQ...`): 9 lines
  - **Post-v3.6 events (v4.4 Lantern Rite `hzweq`, Sabzeruz Festival `HSEQ`, Moonchase `ZFEQ`):** 131 lines. These events did not exist when v3.6.0 was compiled and remain documented as purged from the live client.
- **Recovered lines:** 128 of 128 v3.6 event lines (100% of event scope).
- **Total payload transferred:** 24.79 MB (compared to 12.41 GB full archive download, saving 99.8% network transfer).
- **Storage target:** `workspace/genshin-impact-faruzan/assets/audio/VO_EQ/`.
- **Validation:** 100% passed `RIFF` header validation.

---

## Reference scripts in workspace

The following reusable utility scripts implement this pipeline and remain preserved for cold agents and future project extractions:

- `inspect_remote_zip.py`: Reads the remote Zip64 central directory and exports entry offsets.
- `scan_external_pcks.py`: Scans remote PCK table of contents via byte ranges and maps `fid` to byte offsets.
- `extract_purged_audio.py`: Streams individual `.wem` audio payloads directly into `assets/audio/VO_EQ/` and appends entries to `meta/audio_extracted.json`.
- `meta/purged_audio_locations.json`: JSON mapping of all 128 recovered files with their exact byte offsets in the 3.6.0 zip.
