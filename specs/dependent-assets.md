# Dependent and compound assets specification: Faruzan

Project: [`workspace/genshin-impact-faruzan`](workspace/genshin-impact-faruzan)  
Client target: Genshin Impact v7.1.0  
Entity: Faruzan (`AvatarId: 10000076`)  
Stage: Stage 3 Asset Categorization

---

## 1. Asset categorization summary and accounting

In Stage 3 of the pipeline, all assets extracted into [`assets/`](assets) were audited and partitioned into two categories without duplicating files across separate directories:

- **Category A (Standalone assets organized in-place):** 932 files (151.63 MB) functioning independently without runtime dependencies. These were relocated in-place into structured directories under [`assets/audio/`](assets/audio), [`assets/textures/`](assets/textures), and [`assets/text/`](assets/text).
- **Category B (Dependent and compound assets preserved in original structure):** 728 files (2,007.41 MB) comprising multi-file clusters (3D meshes, skeleton rigs, cel ramps, materials, cloth configs, animation clips, and Spine puppet definitions) that require runtime engine graphs. These remain anchored in their original extracted package hierarchy to preserve relative file linkages.

### Accounting ledger

| Category | Domain | Files | Formats | Location in assets |
|---|---|---|---|---|
| **Category A** | Audio | 790 | `.wem` | [`assets/audio/`](assets/audio) |
| **Category A** | Graphics | 138 | `.png` | [`assets/textures/`](assets/textures) |
| **Category A** | Text and Lore | 4 | `.json` | [`assets/text/`](assets/text) |
| **Category B** | Character Animations | 514 | `.anim` | [`assets/AnimationClip/`](assets/AnimationClip) and hash folders |
| **Category B** | 3D Meshes and LODs | 107 | `.obj`, `.fbx` | [`assets/Mesh/`](assets/Mesh), [`assets/Animator/`](assets/Animator), and hash folders |
| **Category B** | 3D Textures and Ramps | 47 | `.png` | [`assets/Texture2D/`](assets/Texture2D) and hash folders |
| **Category B** | Materials and Shaders | 12 | `.json` | [`assets/Material/`](assets/Material) |
| **Category B** | Physics and Cloth Configs | 45 | `.json`, `.cloth`, `.state` | [`assets/TextAsset/`](assets/TextAsset), [`assets/MonoBehaviour/`](assets/MonoBehaviour), and hash folders |
| **Category B** | 2D Spine Puppets | 2 | `.skel`, `.atlas` | [`assets/`](assets) root |
| **Category B** | State Models | 1 | `.txt` | [`assets/TextAsset/`](assets/TextAsset) |
| **Total Inventory** | | **1,660** | | |

---

## 2. Pre-organization directory trace

To retain complete provenance after in-place reorganization, the state of the initial mirrored extraction was recorded prior to moving standalone files. The full machine-readable audit is preserved in [`meta/pre_cleaning_snapshot.json`](meta/pre_cleaning_snapshot.json).

### Initial extraction metrics

- Total files before reorganization: 1,685 files
- Total directories before reorganization: 383 directories
- Total initial extraction payload: 2,170.30 MB

### Initial package directories

| Directory path | Files | Asset types contained | Post-cleaning disposition |
|---|---|---|---|
| `Audio/Japanese/` | 790 | Wwise `.wem` voice tracks across 12 dialog domains | Moved to `assets/audio/` preserving subcategory structure |
| `AnimationClip/` | 261 | Avatar locomotion, combat, and expression `.anim` tracks | Preserved untouched in `assets/AnimationClip/` |
| `Texture2D/` | 129 | 3D model diffuses, shadow ramps, and limited-event textures | 38 3D textures preserved in `assets/Texture2D/`; 91 UI textures moved to `assets/textures/` |
| `Mesh/` | 79 | Body, hair, prop, and furnishing `.obj` geometry | Preserved untouched in `assets/Mesh/` |
| `Sprite/` | 29 | Standalone UI icons and talent badge textures | Consolidated into `assets/textures/` |
| `MonoBehaviour/` | 13 | Component configuration and cloth attachment tables | Preserved untouched in `assets/MonoBehaviour/` |
| `Material/` | 12 | Cel-shading material parameter JSON graphs | Preserved untouched in `assets/Material/` |
| `Animator/` | 8 | NPC and automaton skeletal rigs and controller mappings | Preserved untouched in `assets/Animator/` |
| `TextAsset/` | 4 | Behavior state model configurations and cloth parameters | Preserved untouched in `assets/TextAsset/` |
| Virtual hash folders (373 dirs) | 360 | Model decimation tiers, variant `.anim` clips, weapon and gadget parts | Compound files preserved; standalone UI sprites moved to `assets/textures/` |

---

## 3. Provenance ledger

Every standalone file relocated out of its initial package directory is recorded in [`meta/provenance_ledger.json`](meta/provenance_ledger.json). The ledger tracks original extraction paths, destination paths, file sizes, and SHA-256 hashes.

### Relocation summary

1. **Audio (790 files):** Moved from `Audio/Japanese/<group>/<file>.wem` to `assets/audio/<group>/<file>.wem`. Group folders (`VO_gameplay`, `VO_friendship`, `VO_COOP`, `VO_EQ`, `vo_aq`, `VO_Card`, `VO_HS`, `VO_teamJoin`, `VO_LQ`, `VO_spice`, `vo_freetalk`, `VO_Tower`) were preserved directly.
2. **Graphics (138 files):** Gathered from `Texture2D/`, `Sprite/`, and virtual hash directories, then organized by functional category into `assets/textures/`.
3. **Text (4 files):** Structured JSON dossiers generated during Stage 3 written to `assets/text/`.

### Provenance sample across categories

| File name | Initial extraction path | Destination path | SHA-256 (first 16 chars) |
|---|---|---|---|
| `vo_xmaq404_26_faruzan_01.wem` | `Audio/Japanese/vo_aq/vo_faruzan/` | `assets/audio/vo_aq/vo_faruzan/` | `da448c93ef859663` |
| `vo_XYJEQ001_3_faruzan_01.wem` | `Audio/Japanese/VO_EQ/VO_faruzan/` | `assets/audio/VO_EQ/VO_faruzan/` | `ef4d4d1f274cb92e` |
| `UI_AvatarIcon_Faruzan.png` | `-4043205/` | `assets/textures/portraits_and_branding/` | `a37ea1a6a6fa58df` |
| `UI_NameCardPic_Faruzan_P.png` | `Texture2D/` | `assets/textures/portraits_and_branding/` | `ee30f813c98ffad9` |
| `UI_CoopCutScene_Faruzan_C01.png` | `Texture2D/` | `assets/textures/hangout_cg/` | `df1cf533bf6ba75c` |
| `Skill_E_Faruzan_01.png` | `Sprite/` | `assets/textures/talents_and_skills/` | `2e043ae74c86fe54` |
| `UI_Gcg_CardFace_Char_Avatar_Faruzan.png` | `-1792302642/` | `assets/textures/tcg_cards/` | `9fa732e6fcb7a421` |
| `UI_Tutorial_AkaFesArchaeology_01.png` | `Texture2D/` | `assets/textures/event_ui_and_guides/` | `2df9f2f8ebf996c1` |
| `UI_Homeworld_Ani_Exterior_Ndkl_Decoration_Prop_Renou_01.png` | `Texture2D/` | `assets/textures/furnishing_icons/` | `3732c53aee24ceb5` |

---

## 4. Category A: Standalone assets organization summary

Category A assets require no runtime bone rigs, shaders, or dependency graphs to play or view.

### 4.1 Audio ([`assets/audio/`](assets/audio))

790 playable Wwise `.wem` clips (104.99 MB) voiced by Horie Yui (堀江由衣), organized into 12 domains:

- [`assets/audio/VO_gameplay/`](assets/audio/VO_gameplay): 146 files. Skill callouts, Burst lines, normal grunts, sprint lines, low HP warnings, and defeat clips.
- [`assets/audio/VO_friendship/`](assets/audio/VO_friendship): 68 files. Profile voiceover lines, character stories, weather comments, and character discussions.
- [`assets/audio/VO_COOP/`](assets/audio/VO_COOP): 247 files. Hangout Event Series VIII dialog lines across all five narrative endings.
- [`assets/audio/VO_EQ/`](assets/audio/VO_EQ): 128 files. Version 3.6 event dialog lines (*A Parade of Providence*) recovered via remote HTTP Range requests.
- [`assets/audio/vo_aq/`](assets/audio/vo_aq): 42 files. Archon Quest Interlude Act III (*Inversion of Genesis*) dialog tracks.
- [`assets/audio/VO_Card/`](assets/audio/VO_Card): 38 files. Genius Invokation TCG match dialog, turn lines, and victory callouts.
- [`assets/audio/VO_HS/`](assets/audio/VO_HS): 35 files. Serenitea Pot interactions and companion housing dialog.
- [`assets/audio/VO_teamJoin/`](assets/audio/VO_teamJoin): 12 files. Party setup and switch-in voice tracks.
- [`assets/audio/VO_LQ/`](assets/audio/VO_LQ): 28 files. Story Quest guest appearance voice lines.
- [`assets/audio/VO_spice/`](assets/audio/VO_spice): 6 files. Food tasting reaction lines.
- [`assets/audio/vo_freetalk/`](assets/audio/vo_freetalk): 38 files. Overworld ambient dialog and NPC interaction voice tracks.
- [`assets/audio/VO_Tower/`](assets/audio/VO_Tower): 2 files. Spiral Abyss floor completion lines.

### 4.2 Graphics ([`assets/textures/`](assets/textures))

138 standalone UI textures (46.63 MB) organized into six categories:

- [`assets/textures/portraits_and_branding/`](assets/textures/portraits_and_branding): 17 files. Character avatar icons, Gacha splash art, namecards, and constellation chapter crests.
- [`assets/textures/hangout_cg/`](assets/textures/hangout_cg): 21 files. Full-screen memory CGs and preview cards for Faruzan's Hangout Event.
- [`assets/textures/talents_and_skills/`](assets/textures/talents_and_skills): 19 files. Combat skill button textures, passive talent badges, and constellation icons.
- [`assets/textures/tcg_cards/`](assets/textures/tcg_cards): 17 files. Genius Invokation TCG card faces, modification card faces, and status overlays.
- [`assets/textures/event_ui_and_guides/`](assets/textures/event_ui_and_guides): 62 files. Archaeology tutorial slides, minigame cards, and radar map pointers.
- [`assets/textures/furnishing_icons/`](assets/textures/furnishing_icons): 2 files. Serenitea Pot furnishing item icon and companion HUD sprite.

### 4.3 Structured text ([`assets/text/`](assets/text))

4 structured JSON files documenting biographical data, audio registries, and limited events:

- [`assets/text/faruzan_profile_and_lore.json`](assets/text/faruzan_profile_and_lore.json): Biographical profile, voice actors, Character Stories 1 through 5, and Vision lore.
- [`assets/text/faruzan_voiceline_transcripts.json`](assets/text/faruzan_voiceline_transcripts.json): Registry indexing all 790 Japanese audio clips with trigger categories and transcription context.
- [`assets/text/faruzan_limited_events_dialogue.json`](assets/text/faruzan_limited_events_dialogue.json): Event scripts across Versions 3.6, 4.4, 4.7, and 5.1.
- [`assets/text/faruzan_furnishings_lore.json`](assets/text/faruzan_furnishings_lore.json): In-game lore, load stats, and adeptal energy numbers for Faruzan furnishings and gadgets.

---

## 5. Category B: Dependent and compound assets inventory

Category B assets require runtime engine linkages, skeleton graphs, or shader pipelines. They remain in their original relative paths under [`assets/`](assets).

### Cluster 1: Faruzan 3D character skeletal rig, meshes, and LODs

Primary playable character model assemblies:

- **Meshes:** 77 geometry files located in `assets/Mesh/`, `assets/Animator/`, and virtual hash directories:
  - Base body: `Avatar_Girl_Bow_Faruzan_Mesh` (torso, limbs, boots).
  - Head and hair: `Avatar_Girl_Bow_Faruzan_Hair_Mesh`, `Avatar_Girl_Bow_Faruzan_Face_Mesh`.
  - Clothing: `Avatar_Girl_Bow_Faruzan_Dress_Mesh` and accessory attachments.
  - LOD tiers: LOD0, LOD1, and LOD2 decimated polygon geometry.
  - Colliders: `Avatar_Girl_Bow_Faruzan_Col` capsules.
- **Dependencies:** Skinning requires the bone hierarchy in the Unity skeleton container; surfaces bind to textures in Cluster 2 and materials in Cluster 4.

### Cluster 2: Character 3D textures, cel ramps, and lightmaps

36 image files providing surface responses for Faruzan's model, located in `assets/Texture2D/`:

- **Diffuse maps:**
  - `Avatar_Girl_Bow_Faruzan_Tex_Body_Diffuse.png`
  - `Avatar_Girl_Bow_Faruzan_Tex_Hair_Diffuse.png`
  - `Avatar_Girl_Bow_Faruzan_Tex_Face_Diffuse.png`
- **Cel ramps:**
  - `Avatar_Girl_Bow_Faruzan_Tex_Body_Shadow_Ramp.png`: Sunlight to shadow falloff ramp.
  - `Avatar_Girl_Bow_Faruzan_Tex_Hair_Shadow_Ramp.png`: Hair sheen falloff ramp.
- **Lightmaps and line maps:**
  - `Avatar_Girl_Bow_Faruzan_Tex_Body_Lightmap.png`: Ambient occlusion and metallic mask.
  - `Avatar_Girl_Bow_Faruzan_Tex_Hair_Lightmap.png`: Directional anisotropic highlight mask.
  - `Avatar_Girl_Bow_Faruzan_Textureline.png`: Ink line outline width map.
  - `Avatar_Girl_Bow_Faruzan_Heightmap.png`: Micro-depth displacement map.

### Cluster 3: Character animation sequences

514 animation clip files (`.anim`) totaling 1,986.61 MB in `assets/AnimationClip/` and virtual hash paths:

- **Locomotion:** Walk, jog, sprint, turn, jump, roll, swim, and climb cycles.
- **Combat:** Bow aim stance, arrow draw cycle, 4-stage normal attack combo, charged arrow release, and plunge dive.
- **Skills:** Elemental Skill cast animation (*Wind Realm of Nasamjnin*), Elemental Burst deployment (*The Wind's Secret Ways*), and burst camera tracks.
- **Expressions:** Blinking, eyebrow movements, speech visemes, smile and irritation morphs.
- **Dependencies:** Requires the `Avatar_Girl_Bow_Faruzan` bone rig and matching controller bone channel bindings.

### Cluster 4: Material graphs and shader configurations

12 material definition files in `assets/Material/`:

- **Character materials:** `Avatar_Girl_Bow_Faruzan_Body_Mat.json`, `Avatar_Girl_Bow_Faruzan_Hair_Mat.json`, `Avatar_Girl_Bow_Faruzan_Face_Mat.json`.
- **Property mapping:** Binds diffuse textures to `_DiffuseTex`, normal maps to `_BumpMap`, shadow ramps to `_RampMap`, and outline widths to `_OutlineWidth`.
- **Shader flags:** Contains cull modes, depth offset biases, and MiHoYo cel-shading parameters (`_RGStrength`, `_BlackenAlbedo`).

### Cluster 5: Skeletal physics, dynamic bones, and cloth simulation

45 configuration files (`.cloth`, `.state`, and JSON definitions) in `assets/TextAsset/`, `assets/MonoBehaviour/`, and hash folders:

- **Cloth simulation:** Dynamic physics driving skirts, sleeves, and dual twintail ribbons during movement.
- **Collider capsules:** Bone colliders preventing skirts from clipping through leg meshes.
- **Spring states:** Hair spring stiffness, damping ratios, and inertia constants.

### Cluster 6: Combat VFX polyhedron meshes

18 assets for Faruzan's Elemental Burst (*The Wind's Secret Ways*):

- **Geometry:** `SkillObj_Faruzan_Pyramid_Mesh` (geometric polyhedron core and orbiting prisms).
- **VFX textures:** `SkillObj_Faruzan_Pyramid_Tex_Eff.png`, emission gradients, and distortion planes.
- **Animation:** Orbiting rotation loops and pulse expansion animation clips.

### Cluster 7: Prop: Faruzan Chibi Fungus Doll Furnishing (Puzzlepal, ID 374636)

5 compound assets for the Faruzan Fungus doll:

- **Visible mesh:** [`Beyd_Object_Prop_RenOu_01_Vo.obj`](assets/Mesh/Beyd_Object_Prop_RenOu_01_Vo.obj) (61,256 bytes).
- **Collision mesh:** [`Beyd_Object_Prop_RenOu_01_Col.obj`](assets/Mesh/Beyd_Object_Prop_RenOu_01_Col.obj) (10,569 bytes).
- **Texture suite:**
  - [`Beyd_Object_Prop_RenOu_01_Diffuse.png`](assets/Texture2D/Beyd_Object_Prop_RenOu_01_Diffuse.png)
  - [`Beyd_Object_Prop_RenOu_01_Normal.png`](assets/Texture2D/Beyd_Object_Prop_RenOu_01_Normal.png)
  - [`Beyd_Object_Prop_RenOu_01_SMBE.png`](assets/Texture2D/Beyd_Object_Prop_RenOu_01_SMBE.png)
- **Material configuration:** [`Beyd_Object_Prop_RenOu_01.json`](assets/Material/Beyd_Object_Prop_RenOu_01.json)

### Cluster 8: Prop: Version 3.6 Omni-Directional Astrolabe Furnishing

3 geometry files for the Akademiya Extravaganza astrolabe:

- **Meshes:** `Homeworld_Interior_Xm_Decoration_Prop_XyjLuopan_01_Lod0.obj`, `Lod1.obj`, `Col.obj`.
- **Dependencies:** Concentric multi-axis ring hierarchy requiring shared Sumeru brass shader parameters.

### Cluster 9: Prop: Version 3.6 Archaeological Radar Detector Probes

6 files comprising the Haravatat archaeological minigame probe device:

- **Meshes:** `Property_Ani_Prop_AkaFesArchaeologyDetector_01_01_Vo.obj`, `02_Vo.obj`, `03_Vo.obj`.
- **Textures:** Detector diffuse map, normal map, and signal emissive pulse mask.

### Cluster 10: Prop: Haravatat Darshan Uniform Hat and Scarf Models

5 files comprising scholastic attire:

- **Meshes:** `NPC_ItemDynamic_HaravatatHat001_Model.obj`, `Body008_Standard_HaravatatScarf001.obj`.
- **Physics and materials:** Hat brim rigid collider JSON, scarf dynamic cloth `.cloth` and `.state` configurations.

### Cluster 11: Prop: Ruin Machine Cleaner Automaton (TMR-0001)

4 files for the companion ruin machine:

- **Meshes:** `AvatarObj_Ani_Quest_FaruzanRobot_01_Mesh`.
- **Textures:** `AvatarObj_Ani_Quest_FaruzanRobot_01_Tex_Diffuse.png`, `AvatarObj_Ani_Quest_FaruzanRobot_01_Tex_Lightmap.png`.

### Cluster 12: 2D Spine Skeletal Puppet: Genius Invokation TCG Dynamic Card

2 files providing skeletal animation for the Faruzan TCG character card:

- **Skeleton:** `Gcg_CardFace_Char_Avatar_Faruzan_Spine.skel` (Spine 3.8 binary skeleton).
- **Atlas sheet:** `Gcg_CardFace_Char_Avatar_Faruzan_Spine.atlas` (bone piece coordinates and texture region bindings).
- **Dependencies:** Requires the Spine Web runtime and references `Gcg_CardFace_Char_Avatar_Faruzan_Spine.png`.

### Cluster 13: Animator controllers and minigame state models

Unity Animator controllers and level logic files:

- Unity runtime animator state machines governing avatar animation transitions.
- Minigame state definitions (`Level_ActivityPacMan_01_BSM_*.txt`).

---

## 6. Dual-use asset registry

Dual-use assets stand alone as visual media, but also serve as direct components or decals within compound assemblies. They are organized in `assets/textures/` for immediate access, with their compound roles documented below:

| Asset file | Organized location | Compound context | Role in assembly |
|---|---|---|---|
| `UI_Homeworld_Ani_Exterior_Ndkl_Decoration_Prop_Renou_01.png` | `assets/textures/furnishing_icons/` | Cluster 7: Puzzlepal Furnishing | Furnishing blueprint, shop preview, and placement indicator icon. |
| `UI_Beyd_Gadget_Prop_RenOu_01.png` | `assets/textures/furnishing_icons/` | Cluster 7: Puzzlepal Furnishing | Minigame exploration companion HUD activation sprite. |
| `Gcg_CardFace_Char_Avatar_Faruzan_Layer00.png` | `assets/textures/tcg_cards/` | Cluster 12: 2D Spine Puppet | Base illustration layer behind dynamic Spine skeletal cutouts. |
| `UI_CoopCutScene_Faruzan_C01` through `C05` | `assets/textures/hangout_cg/` | Cluster 3: Hangout Cutscenes | Full-screen story climax stills rendered in narrative dialogue trees. |
| `UI_Tutorial_AkaFesArchaeology_01` through `05` | `assets/textures/event_ui_and_guides/` | Cluster 9: Archaeological Detector | Tutorial banner billboard textures displayed alongside probe HUD. |

---

## 7. Stage 3 verification and next steps

- **Integrity verification:** All 1,660 project assets are verified with SHA-256 hashes recorded in `meta/manifest.json`.
- **Pre-cleaning history preserved:** Complete pre-organization file tree and hashes are stored in `meta/pre_cleaning_snapshot.json` and `meta/provenance_ledger.json`.
- **Zero duplication:** The redundant duplicate directory was eliminated, reducing workspace footprint by 151.63 MB while preserving compound asset linkages.
- **Stage 4 readiness:** This specification establishes the asset clusters, material linkages, and runtime requirements needed for `specs/visualization-spec.md`.
