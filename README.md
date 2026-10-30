# Faruzan (Genshin Impact)

Extracted game assets, technical specifications, and interactive visual confirmation gallery for Faruzan from Genshin Impact (v7.1.0 client with recovered v3.6.0 event audio).

## Pipeline and reproduction

These assets were cataloged, extracted, categorized, and documented deterministically using the structured asset extraction pipeline:
https://github.com/4163/i-asset-extraction

## Live preview

Interactive web gallery: https://4163.github.io/faruzan-assets

## Asset overview

| Category | File count | Formats | Directory |
|---|---|---|---|
| Audio | 790 | .wem | `assets/audio/` |
| Textures | 185 | .png | `assets/textures/`, `assets/Texture2D/` |
| 3D Meshes and LODs | 107 | .obj, .fbx | `assets/Mesh/`, `assets/Animator/` |
| Materials and Shaders | 12 | .json | `assets/Material/` |
| Physics and Cloth Configs | 45 | .json, .cloth, .state | `assets/TextAsset/`, `assets/MonoBehaviour/` |
| Text and Lore | 4 | .json | `assets/text/` |
| TCG Card Skeletons | 2 | .skel, .atlas | `assets/` |
| Character Animations | 514 | .anim | Distributed via [GitHub Release Attachment](https://github.com/4163/faruzan-assets/releases) |

To keep git clones light (~175 MB), the 514 character animation clips (~1.98 GB uncompressed) are packaged into `faruzan-animations.zip` (104.94 MB) available in the GitHub Release attachments.

## Technical specifications

- **Skeletal hierarchy:** 87-node bone rig mapped to Unity Humanoid standard (`Bip001` -> `Pelvis` -> `Spine` -> `Spine1` -> `Spine2` -> `Neck` -> `Head`).
- **Material parameters:** Dual-pass cel-shading pipeline binding diffuse albedo (`_DiffuseTex`), anisotropic hair highlights (`_Lightmap`), UV shadow ramp lookups (`_RampMap`), specular metallic mask (`_SMBE`), and procedural ink-line vertex extrusion maps (`Textureline`).

Full technical details and cluster bindings are documented in [specs/visualization-spec.md](specs/visualization-spec.md).

## Tools and utilities used

| Tool | Role | Repository / Reference |
|---|---|---|
| AnimeStudio CLI | Unity AssetBundle extraction for geometry, textures, materials, and physics | [Razmoth/AnimeStudio](https://github.com/Razmoth/AnimeStudio) |
| vgmstream | Wwise WEM audio decoding and gallery stream transcoding | [vgmstream/vgmstream](https://github.com/vgmstream/vgmstream) |
| REA | Extraction environment analysis, archive inspection, and pipeline management | [4163/i-asset-extraction](https://github.com/4163/i-asset-extraction) |
| HTTP Range Streamer | Remote CDN zip central directory inspection and uncompressed WEM extraction | [specs/recovery-methodology.md](specs/recovery-methodology.md) |

## Disclaimer

All game assets and character content are property of HoYoverse. This repository is maintained strictly for archival purposes.
