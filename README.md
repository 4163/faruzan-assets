# Faruzan (Genshin Impact)

[![Live Gallery](https://img.shields.io/badge/Live-Gallery-brightgreen)](https://4163.github.io/faruzan-assets)
[![Upstream Pipeline](https://img.shields.io/badge/Pipeline-i--asset--extraction-blue)](https://github.com/4163/i-asset-extraction)

Extracted game assets, technical specifications, and interactive visual confirmation gallery for **Faruzan (ファルザン / 珐露珊)** from **Genshin Impact (原神)** (v7.1.0 client with recovered v3.6.0 event audio).

## Live preview

Interactive web gallery: https://4163.github.io/faruzan-assets

## Asset overview

| Category | File count | Formats | Directory |
|---|---|---|---|
| Audio | 790 | `.wem` | `assets/audio/` |
| Textures (UI & CG) | 138 | `.png` | `assets/textures/` |
| 3D Meshes & LODs | 107 | `.obj`, `.fbx` | `assets/Mesh/`, `assets/Animator/` |
| 3D Textures & Ramps | 47 | `.png` | `assets/Texture2D/` |
| Materials & Shaders | 12 | `.json` | `assets/Material/` |
| 2D Skeletal Puppets | 2 | `.skel`, `.atlas` | `assets/` |
| Physics & Cloth Configs | 45 | `.json`, `.cloth`, `.state` | `assets/TextAsset/`, `assets/MonoBehaviour/` |
| Text and Lore | 4 | `.json` | `assets/text/` |
| Character Animations | 514 | `.anim` | Distributed via [GitHub Release Attachment](https://github.com/4163/faruzan-assets/releases) |

*Note: To keep git clones fast and light (~175 MB), the 514 character animation clips (~1.98 GB uncompressed) are packaged into `faruzan-animations.zip` (104.94 MB) available in the GitHub Release attachments.*

## Technical specifications

- **Skeletal hierarchy:** 87-node bone rig mapped to Unity Humanoid standard (`Bip001` -> `Pelvis` -> `Spine` -> `Spine1` -> `Spine2` -> `Neck` -> `Head`).
- **2D runtime:** Spine 4.0.47 binary skeleton (`Gcg_CardFace_Char_Avatar_Faruzan_Spine.skel`) and coordinate atlas (`.atlas`) driving the Genius Invokation TCG holographic character card.
- **Material parameters:** Dual-pass cel-shading pipeline binding diffuse albedo (`_DiffuseTex`), anisotropic hair highlights (`_Lightmap`), UV shadow ramp lookups (`_RampMap`), specular metallic mask (`_SMBE`), and procedural ink-line vertex extrusion maps (`Textureline`).

Full technical details and cluster bindings are documented in [specs/visualization-spec.md](specs/visualization-spec.md).

## Tools and utilities used

| Tool | Role | Repository / Reference |
|---|---|---|
| AnimeStudio CLI | Unity AssetBundle extraction for geometry, textures, materials, and physics | [Razmoth/AnimeStudio](https://github.com/Razmoth/AnimeStudio) |
| vgmstream | Wwise WEM audio decoding and gallery stream transcoding | [vgmstream/vgmstream](https://github.com/vgmstream/vgmstream) |
| REA (Reverse Engineer Anything) | Extraction environment analysis, archive inspection, and pipeline management | [4163/i-asset-extraction](https://github.com/4163/i-asset-extraction) |
| HTTP Range Streamer | Remote CDN zip central directory inspection and uncompressed WEM extraction | [specs/recovery-methodology.md](specs/recovery-methodology.md) |

## Pipeline and reproduction

These assets were cataloged, extracted, categorized, and documented deterministically using the structured asset extraction pipeline:
https://github.com/4163/i-asset-extraction

## Disclaimer and copyright

Game artwork, character designs, 3D models, audio recordings, and associated intellectual property remain the property of **HoYoverse (Cognosphere PTE. LTD. / miHoYo)**. This repository is maintained strictly for archival, technical research, and educational purposes. Spine runtime formats are subject to licensing from Esoteric Software LLC.
