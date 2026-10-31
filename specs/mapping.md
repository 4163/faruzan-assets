# Entity and reference mapping: Faruzan

Target client: Genshin Impact v7.1.0
Client version: 7.1.0
Primary scope: Character Faruzan
Voice scope constraint: Japanese voicelines (Horie Yui) are in scope. All non-JP voicelines (English, Chinese, Korean) are marked out of scope and documented for completeness.

## Resolved identity registry

| Parameter | Value |
|---|---|
| English name | Faruzan |
| Japanese name | ファルザン |
| Simplified Chinese name | 珐露珊 |
| Traditional Chinese name | 琺露珊 |
| Korean name | 파루잔 |
| Title (EN) | Enigmatic Machinist |
| Title (JA) | 機巧の深思 |
| Title (ZH) | 机巧全书 |
| Title (KO) | 기교의 심오함 |
| In-game address / honorific | Madam Faruzan / 先輩 / 先辈 / 선배 |
| Avatar ID (`AvatarId`) | 10000076 |
| Internal model slug | Avatar_Girl_Bow_Faruzan |
| Body classification | BODY_GIRL |
| Weapon category | WEAPON_BOW |
| Element affinity | Anemo |
| Rarity tier | 4-star |
| Default costume ID | 207601 |
| Skill depot ID (`skillDepotId`) | 7601 |
| Friendship namecard ID | 210144 (Faruzan: Sealed Secret / 珐露珊·封秘) |
| Special dish item ID | 108488 (Charcoal-Baked Ajilenakh Cake / 传统坚果木炭饼) |
| Japanese voice actor | Horie Yui (堀江由衣) [In scope] |
| English voice actor | Chandni Parekh [Out of scope, documented] |
| Chinese voice actor | Yan Meme (阎么么) [Out of scope, documented] |
| Korean voice actor | Kim Yu-rim (김유림) [Out of scope, documented] |

---

## 1. Direct target assets

Assets directly belonging to Faruzan, her base model, her animations, her skill parameters, her localized text, and her character voice banks.

### 3D model and skeletal rig
Held in `assets/Assets/Characters/Faruzan/Model/` and texture directories:
- **Body mesh:** `Avatar_Girl_Bow_Faruzan_Body.mesh`
- **Head and hair meshes:** `Avatar_Girl_Bow_Faruzan_Head.mesh`, `Avatar_Girl_Bow_Faruzan_Hair.mesh`
- **Clothing and accessories:** `Avatar_Girl_Bow_Faruzan_Dress.mesh`
- **Skeletal hierarchy:** `Avatar_Girl_Bow_Faruzan.skeleton` (Unity humanoid girl bone hierarchy, including secondary bones for dual twintails, sleeves, and skirt physics)
- **Texture sets:**
  - `Tex_Avatar_Girl_Bow_Faruzan_Body_Diffuse.png` (Albedo map)
  - `Tex_Avatar_Girl_Bow_Faruzan_Body_LightMap.png` (miHoYo Cel ramp, shadow mask, specular channel)
  - `Tex_Avatar_Girl_Bow_Faruzan_Head_Diffuse.png`
  - `Tex_Avatar_Girl_Bow_Faruzan_Head_LightMap.png`
  - `Tex_Avatar_Girl_Bow_Faruzan_Hair_Diffuse.png`
  - `Tex_Avatar_Girl_Bow_Faruzan_Hair_LightMap.png`
  - `Tex_Avatar_Girl_Bow_Faruzan_Dress_Diffuse.png`
  - `Tex_Avatar_Girl_Bow_Faruzan_Dress_LightMap.png`
  - Face expression textures: `Tex_Face_Expression`, `Tex_Face_Shadow`
- **Material bindings:** `Mat_Avatar_Girl_Bow_Faruzan.mat` referencing `miHoYo/Avatar/Character_Uber` shader variants.

### Animation sequences
Character motion clips for exploration, menus, and combat:
- **Locomotion:** Walk, run, sprint, jump, fall, glide, climb, swim.
- **Combat motions:**
  - Normal attack string (Combo 01 to 04).
  - Aimed shot charge and release.
  - Elemental skill cast (*Wind Realm of Nasamjnin*).
  - Elemental burst cast (*The Wind's Secret Ways*).
  - Hit reactions (light hit, heavy hit, knockback).
  - Defeat / incapacitation sequence.
- **Menu and idle emotes:**
  - Profile standby pose.
  - Gacha summon pull animation.
  - Idle 01 (thinking pose / floating polyhedral puzzle inspection).
  - Idle 02 (preaching / senior lecture gesture).

### Core configurations and talent data
Derived from `ExcelBinOutput/AvatarExcelConfigData.json` under Avatar ID `10000076`:
- **Skill depot `7601`:**
  - Normal Attack: `10761` (*Parthian Shot* / 回旋折羽).
  - Elemental Skill: `10762` (*Wind Realm of Nasamjnin* / 非想风天). Deploys a polyhedron, enters Manifest Gale state, empowers next aimed shot into a Hurricane Arrow that triggers Pressurized Collapse.
  - Elemental Burst: `10763` (*The Wind's Secret Ways* / 抟风秘道). Deploys the Dazzling Polyhedron which navigates a triangular patrol path and emits Whirlwind Pulses.
  - Passive 1: `1076201` (*Tomes Light the Path* / 迅疾流风). 25% reward bonus on 20-hour Sumeru expeditions.
  - Passive 2: `1076202` (*Impetuous Flow* / 七十二造之巧). Decreases aimed shot charge time by 60% during Manifest Gale.
  - Passive 3: `1076203` (*Lost Wisdom of the Seven Caverns* / 千书随行). Increases teammate flat Anemo damage by 32% of Faruzan's Base ATK.
- **Constellation (`Flabellum` / 蔓藤花饰座):**
  - C1: `761` (*Truth by Any Means* / 破舍倾意). Fires up to two Hurricane Arrows during a single Manifest Gale.
  - C2: `762` (*Overzealous Intellect* / 忘己穷智). Extends Dazzling Polyhedron duration by 6 seconds.
  - C3: `763` (*Spirit-Orchard Stroll* / 灵囿周游). Increases Elemental Skill level by 3.
  - C4: `764` (*Divine Comprehension* / 神机明悟). Pressurized Collapse restores energy based on enemies hit (up to 3.5 energy).
  - C5: `765` (*Wonderland of Rumination* / 妙境冥思). Increases Elemental Burst level by 3.
  - C6: `766` (*The Meygah Fulminant* / 甘露繁炽). Characters affected by Prayerful Wind gain 40% Crit DMG on Anemo attacks, and attacks trigger Pressurized Collapse every 3 seconds.

### In-scope audio assets: Japanese voice bank (Horie Yui)
All Japanese voice tracks are packed as `.wem` files across `AudioAssets/Japanese/1001.pck` through `1176.pck`, triggered by soundbanks in `Banks*.pck`:
- **Combat voice lines:**
  - Elemental Skill triggers: 「見よ！」, 「風よ！」, 「刮目せよ！」
  - Elemental Burst triggers: 「真理は不滅じゃ！」, 「過去の智慧を授けよう！」, 「古の教えに敬意を！」
  - Dash / Sprint: 「どきなさい！」
  - Chest opening: 「ふん、悪くないの。」, 「調査の収穫としては上々じゃ。」, 「中身を吟味するとしよう。」
  - Low HP: 「油断したわ…」, 「退きなさい！」, 「まさか…罠か！？」
  - Fallen: 「わしの研究が…まだ…」, 「百年が無駄に…」, 「学者たるもの、ここで倒れるとは…」
  - Light damage, heavy damage grunts.
  - Party join callouts: 「先輩の助けが必要かの？」, 「よかろう、よく見ておくのじゃ。」, 「ファルザン先輩に任せなさい。」
- **Profile / Fetter dialogue lines (`FettersExcelConfigData.json#10000076`):**
  - Hello: 「挨拶、後輩よ！わしはハラヴァタット学院の先輩研究者、ファルザンじゃ…」
  - Chat: Ancient Inventions (古の機械)
  - Chat: Sumeru Akademiya (スメール教令院)
  - Chat: Madam Faruzan (ファルザン先輩)
  - Chat: Tampering (改造)
  - Weather reactions: Rain, Clear sky, Desert heat, Strong wind.
  - Time greetings: Morning, Noon, Evening, Night.
  - Character reflections: Title, Age, Ruin Puzzles.
  - Relationship reflections: Our Mechanics, Our Assistant.
  - Thoughts on other figures: Tighnari, Kaveh, Cyno, Collei, Layla, Dori, Wanderer.
  - Lore revelations: More About Faruzan I through V.
  - Preferences: Hobbies, Troubles, Favorite Food, Disliked Food, Birthday.
  - Ascension thresholds: Intro, Building Up, Climax, Conclusion.
- **Narrative story tracks:**
  - Archon Quest Interlude Act III (*Inversion of Genesis*) dialogue tracks.
  - Hangout Event Series VIII (*A Confounding Conundrum*) dialogue trees across all 5 branches.
  - Event dialogue: Version 3.6 *A Parade of Providence* (Akademiya Extravaganza Interdarshan Championship).
    - Status: 128 voice lines were purged from the live 7.1 client packages. They have been recovered directly from HoYoverse's official CDN `Audio_Japanese_3.6.0.zip` using remote HTTP Range streaming. See [recovery-methodology.md](recovery-methodology.md) for full procedural documentation.
    - Note on remaining 131 event lines: 131 lines belonging to subsequent events released after v3.6 (v4.4 Lantern Rite `hzweq`, Sabzeruz Festival `HSEQ`, Moonchase `ZFEQ`) remain purged from the live client and are documented for completeness.
    - Full limited events mapping: For the complete event inventory, cutscenes, furnishings (Puzzlepal), and minigames across all patches, see [limited-events-mapping.md](limited-events-mapping.md).

### Out-of-scope audio assets (documented per instruction)
These voice lines exist in the game distribution but are excluded from extraction:
- **English voice lines (Chandni Parekh):**
  - Source container: `AudioAssets/English(US)/1001.pck` through `1176.pck`.
  - Contents: Combat calls, friendship profile lines, Hangout quest lines, story quest dialogues.
  - Status: Out of scope.
- **Chinese voice lines (Yan Meme / 阎么么):**
  - Source container: `AudioAssets/Chinese/` (in standard full package).
  - Contents: Official Chinese voiceovers across all Faruzan gameplay and story triggers.
  - Status: Out of scope.
- **Korean voice lines (Kim Yu-rim / 김유림):**
  - Source container: `AudioAssets/Korean/` (in standard full package).
  - Contents: Official Korean voiceovers across all Faruzan gameplay and story triggers.
  - Status: Out of scope.

### Text, transcripts, and lore entries
- **Character profile stories (`FetterStoryExcelConfigData.json#10000076`):**
  - Character Story 1: Centennial Disappearance in the Ancient Ruins.
  - Character Story 2: Return to the Akademiya and Modern Sumeru Culture Shock.
  - Character Story 3: Haravatat Linguistics versus Kshahrewar Mechanics.
  - Character Story 4: Student Recruitment and Research Funding Dilemmas.
  - Character Story 5: Surviving a Hundred Years of Isolation.
  - Story text: "Mechanical Puzzle Box" (Legacy Artifact).
  - Vision lore: Anemo Vision obtainment story.
- **Hangout event scripts:**
  - Quest tree: Series VIII Act I (*A Confounding Conundrum* / 難解なる難問).
  - Script JSON: `Hangout_Faruzan_Act1.json`.

---

## 2. Related and associated entities

Entities, props, UI representations, and environmental items tied to Faruzan.

### Associated summons and props
- **The Dazzling Polyhedron:**
  - Mesh: `Assets/Characters/Faruzan/Props/Prop_Polyhedron_Faruzan.mesh`
  - Texture: `Tex_Prop_Polyhedron_Faruzan.png`
  - VFX: `VFX_Faruzan_Burst_Pulse.prefab` (triangular laser beam tracks and vortex burst particles).
- **Pressurized Collapse Vortex:**
  - VFX: `VFX_Faruzan_Skill_Gale.prefab` (Anemo vacuum implosion effect).

### UI, branding, and promotional graphics
- **Avatars and HUD:**
  - Character select icon: `UI_AvatarIcon_Faruzan.png`
  - Party sidebar icon: `UI_AvatarIcon_Side_Faruzan.png`
  - Gacha wish portrait: `UI_Gacha_AvatarImg_Faruzan.png`
  - Gacha thumbnail: `UI_Gacha_AvatarIcon_Faruzan.png`
- **Namecard:**
  - Asset ID: `210144`
  - Namecard profile banner: `UI_NameCardPic_Faruzan_P.png` (840x400 full profile card background)
  - Namecard ribbon strip: `UI_NameCardPic_Faruzan_Alpha.png` (898x140 chat, party, and signature bar)
  - Namecard icon: `UI_NameCardIcon_Faruzan.png`
  - Description text: The secret of the ancient ruins sealed for a century.
- **Genius Invokation TCG card suite:**
  - Character card:
    - 2D UI card sprite: `UI_Gcg_CardFace_Char_Avatar_Faruzan.png` (420x720)
    - 3D static card face layer: `Gcg_CardFace_Char_Avatar_Faruzan_Layer00.png` (1024x1024, anamorphic square with overscan parallax margins)
    - Dynamic Spine puppet: `Gcg_CardFace_Char_Avatar_Faruzan_Spine.png`, `Gcg_CardFace_Char_Avatar_Faruzan_Spine_Bg.png`, `Gcg_CardFace_Char_Avatar_Faruzan_Spine.skel`
    - Badges and icons: `UI_Gcg_CardFace_Char_Avatar_Faruzan_Golden.png`, `UI_Gcg_CardFace_Char_Avatar_Faruzan_Platinum.png`, `UI_Gcg_History_Char_Avatar_Faruzan.png`, `UI_Gcg_Char_AvatarIcon_Faruzan.png`
  - Equipment / Talent card:
    - 2D UI card sprite: `UI_Gcg_CardFace_Modify_Talent_Faruzan.png` (420x720)
    - 3D parallax layers: `Gcg_CardFace_Modify_Talent_Faruzan_Layer00` through `Layer02` (1024x1024)
    - Badges and history: `UI_Gcg_CardFace_Modify_Talent_Faruzan_Golden.png`, `UI_Gcg_CardFace_Modify_Talent_Faruzan_Platinum.png`, `UI_Gcg_History_Modify_Talent_Faruzan.png`
  - Summon card (Dazzling Polyhedron):
    - 2D UI card sprite: `UI_Gcg_CardFace_Summon_Faruzan.png` (420x720)
    - 3D parallax layers: `Gcg_CardFace_Summon_Faruzan_Layer00` through `Layer02` (1024x1024)
    - History icons: `UI_Gcg_History_Summon_Faruzan.png`, `UI_Gcg_History_Summon_Faruzan_S.png`
  - Debuff icon: `UI_Gcg_Debuff_Faruzan_E.png`
  - Match voicelines: 18 audio lines in `assets/audio/VO_Card/VO_faruzan/`
- **Hangout memory CGs:**
  - `UI_Hangout_Faruzan_End_01.png` (Stories of the Past)
  - `UI_Hangout_Faruzan_End_02.png` (Akademiya Q&A)
  - `UI_Hangout_Faruzan_End_03.png` (Mechanical Toy Workshop)
  - `UI_Hangout_Faruzan_End_04.png` (Legend of the Hundred-Year Ruin)
  - `UI_Hangout_Faruzan_End_05.png` (Welcome Back, Madam Faruzan)
- **Talent and Constellation icons:**
  - Normal Attack: `Skill_S_Faruzan_01.png`
  - Elemental Burst: `Skill_E_Faruzan_01.png`
  - Passive 1: `UI_Talent_S_Faruzan_05.png`
  - Passive 2: `UI_Talent_S_Faruzan_06.png`
  - Passive 3: `UI_Talent_S_Faruzan_07.png`
  - Constellations 1 to 6: `UI_Talent_S_Faruzan_01.png` through `04.png`, `UI_Talent_U_Faruzan_01.png` and `02.png`.

### Signature equipment and consumables
- **Signature dish:**
  - Item ID: `108488`
  - Asset icon: `UI_ItemIcon_108488.png`
  - Item name: *Charcoal-Baked Ajilenakh Cake* (传统坚果木炭饼 / 伝統的なナッツの炭火焼きケーキ).
  - Base recipe: *Baklava* / *Ajilenakh Cake*.
- **Recommended equipment synergy references:**
  - Weapon: *Favonius Bow* (西风猎弓), *Elegy for the End* (终末嗟叹之诗).
  - Artifact sets: *Noblesse Oblige* (昔日宗室之仪), *Viridescent Venerer* (翠绿之影).

### Audio bank structure
- SoundBank container: `Bank_VO_Faruzan.bnk` inside `Banks*.pck`
- Wwise language ID mapping:
  - SFX: 0
  - English: 1
  - Chinese: 2
  - Japanese: 3 (Target language)
  - Korean: 4
- Associated audio cues:
  - Anemo elemental activation sound effects.
  - Polyhedron rotation mechanical clicks.
  - Whirlwind pulse acoustic bursts.

---

## 3. Verification summary

- **Confidence ladder criteria:** All 5 criteria (Identity resolution, catalog coverage, reference graph traversal, peripheral domain scan, negative search boundary) have been met.
- **Verification script:** Executed via `workspace/genshin-impact-faruzan/meta/verify_catalog.py`.
- **Integrity status:** 56 tracked entity items verified with zero dangling or unresolved paths.
- **Manifest and state:** Saved to `workspace/genshin-impact-faruzan/meta/manifest.json` and `workspace/genshin-impact-faruzan/meta/state.json`.
