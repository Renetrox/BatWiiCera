# BatWiiCera for EmulationStation-X

> A Wii-inspired theme adapted for **[EmulationStation-X (ES-X)](https://github.com/Renetrox/EmulationStation-X)**, with native system Grid navigation, game previews, and selectable 16:9 / 4:3 layouts.

[![ES-X](https://img.shields.io/badge/Frontend-EmulationStation--X-2088D7)](https://github.com/Renetrox/EmulationStation-X)
[![Layouts](https://img.shields.io/badge/Layouts-16%3A9%20%7C%204%3A3-6C757D)](./layouts)
[![License: CC BY-NC-SA 4.0](https://img.shields.io/badge/License-CC%20BY--NC--SA%204.0-lightgrey)](./LICENSE)
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/4c0ff1af-1d8a-43da-aa73-ea173d270951" />

**Original theme:** BatWiiCera v0.1.6 by **Dan Lee (yiddifliddo)**.  
**ES-X adaptation:** **Renetrox**.  
**Status:** Adaptation tested on desktop; additional hardware and 4:3 visual testing are welcome.

This is a community adaptation of the original BatWiiCera theme, not an official Nintendo product. The original Wii-channel-inspired visual identity—including the subtle gray relief on unselected tiles—is intentionally preserved.

## Features

| Feature | ES-X adaptation |
| --- | --- |
| **System selection** | Native `imagegrid` with four-direction navigation and rounded channel tiles |
| **System logos** | SVG/PNG logos, localized variants when available, and configurable missing-logo text |
| **Game list** | Left-side game list and right-side artwork / video preview, with game metadata |
| **Missing media** | Conditional `no-preview.png` fallback; no permanently overlaid placeholder |
| **Video preview** | Thumbnail displayed while the video starts, then video playback via `md_video` |
| **Game Grid** | Optional grid view with artwork and selection highlighting |
| **Bottom bar** | Clock, system information and themed help prompts |
| **Status indicators** | ES-X network and Bluetooth icons, displayed only when the corresponding status/settings allow it; positioned at the top-right by the theme |
| **Side arrows** | Original BatWiiCera left/right SVG decorations in the system view; **not** clickable controls |
| **Aspect ratios** | Widescreen **16:9** and classic **4:3**, selectable in Theme Options |

The round Home and Mail icons and the two side arrows are **visual theme artwork** in this ES-X port; they are not advertised here as clickable actions. Bluetooth and network indicators are different: ES-X renders them conditionally from its own status checks, using the position specified by the theme. Batocera-only extras from the original README (such as Netplay navigation, pointer-hand interaction and save-state / RetroAchievements buttons) have **not** been ported as interactive features.

## Requirements

- **EmulationStation-X** with native system Grid and `defaultSnapshot` support, introduced in [ES-X PR #73](https://github.com/Renetrox/EmulationStation-X/pull/73) and merged into `main` on November 14, 2026.
- A supported desktop or RetroPie installation running ES-X.
- Scraped images, thumbnails and videos for the corresponding previews. The theme does **not** download game media.

**Important:** This repository is the **ES-X adaptation**. The original Batocera version has a different feature set. Do not assume that Batocera-specific UI actions are available here.

## Installation

Install the whole theme directory, not just `theme.xml`.

```bash
mkdir -p ~/.emulationstation/themes
git clone https://github.com/Renetrox/BatWiiCera.git \
  ~/.emulationstation/themes/BatWiiCera
```

Then open **ES-X → UI Settings → Theme Set** and choose **BatWiiCera**.

If you previously installed an older copy, back up custom files before updating it:

```bash
cd ~/.emulationstation/themes/BatWiiCera
git pull --ff-only
```

This updates only a clean checkout of this repository. If you have edited the installed theme, save your modifications before pulling.

## Select 16:9 or 4:3

Go to **ES-X → UI Settings → Theme Configuration / Theme Options → Formato de pantalla**.

| Layout | System Grid | Game Grid | Intended use |
| --- | --- | --- | --- |
| **16:9** | 4 columns × 3 rows | 5 columns × 3 rows | Widescreen displays |
| **4:3** | 3 columns × 3 rows | 4 columns × 3 rows | Traditional 4:3 displays, including 800×600 |

The layout selection is defined in [`theme.ini`](./theme.ini). The actual XML files are:

- [`layouts/16:9/theme.xml`](./layouts/16:9/theme.xml)
- [`layouts/4:3/theme.xml`](./layouts/4:3/theme.xml)

ES-X applies the selected layout to the root `theme.xml` and restarts the interface. **Layout selection is manual**, not automatic aspect-ratio detection. The root `theme.xml` ships with the 16:9 layout as its default.

### Alternative: manual layout selection

If needed, close ES-X and copy the desired layout to the theme root:

```bash
cd ~/.emulationstation/themes/BatWiiCera
cp theme.xml theme.xml.backup
cp "layouts/4:3/theme.xml" theme.xml
```

For 16:9, copy `layouts/16:9/theme.xml` instead.

## Media previews and fallback behavior

The XML separates ES-X's **Detailed** and **Video** gamelist views:

1. **Video available:** a thumbnail can appear briefly during the playback delay, then the video plays.
2. **No video, image available:** the game's image is displayed in the applicable view.
3. **No usable artwork:** `_inc/images/no-preview.png` is displayed as a conditional fallback.

The Video view uses the ES-X-specific `<defaultSnapshot>` element, together with `<showSnapshotNoVideo>true</showSnapshotNoVideo>` and `<showSnapshotDelay>true</showSnapshotDelay>`. The `md_image` component is hidden in the Video view so a separate image does not overlap the player.

> **Compatibility note:** An older ES-X executable that does not recognize `defaultSnapshot` may log `Unknown property type "defaultSnapshot"` and fail to load the theme. Update and rebuild ES-X from a version containing [PR #73](https://github.com/Renetrox/EmulationStation-X/pull/73).

## Theme structure

```text
BatWiiCera/
├── theme.xml                 # Active XML (16:9 by default)
├── theme.ini                 # Layout selector used by ES-X
├── layouts/
│   ├── 16:9/theme.xml        # Widescreen layout
│   └── 4:3/theme.xml         # Classic layout
├── _inc/
│   ├── fonts/                # Varela Round and Nunito
│   ├── images/               # Original Wii-inspired art and fallback
│   └── systems/logos/        # Console and collection logos
├── music/                    # Audio files and notes
├── LICENSE                   # Original license and asset credits
└── README.md
```

The theme keeps the original graphics, including `tile.png` and `tile-selected.png`. Please preserve the bundled [license and attribution](./LICENSE) when modifying or redistributing it.

## Known limitations / testing

- The desktop adaptation has been exercised with system navigation, localized logos, media previews and help prompts.
- **Orange Pi 4A performance and layout behavior** have not yet been independently verified for this repository.
- The proposed 4:3 layout is included, but visual tuning may be required on particular displays.
- Some systems or automatic collections may not have a corresponding logo asset. Missing logo files do not necessarily indicate a theme failure.
- Interactive features of the original Batocera theme are not automatically provided by ES-X.

If you find a layout problem, open a [GitHub issue](https://github.com/Renetrox/BatWiiCera/issues) with your screen resolution, ES-X version and a screenshot or log.

## Credits and license

**Original BatWiiCera theme:** Dan Lee (yiddifliddo), version 0.1.6.  
**ES-X compatibility adaptation:** Renetrox.  
**License:** [Creative Commons Attribution–NonCommercial–ShareAlike 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/) (CC BY-NC-SA 4.0), subject to the separate attributions and licenses noted in [`LICENSE`](./LICENSE).

Additional credits retained from the original distribution:

- **Carbon** system logos: Rookervik, Nils Bonenberger and Fabrice Caruso.
- **Art Book Next** fallback logos and help icons: Anthony Caccese.
- **es-theme-minimal** assets / ideas: lilbud and Fabrice Caruso.
- **PlayStation-X** structural inspiration: pajarorrojo.
- **Varela Round:** Apache License 2.0; **Nunito:** SIL Open Font License 1.1.

This project is a fan-made visual tribute. It is not affiliated with, sponsored by, or endorsed by Nintendo or any other console manufacturer. The authors retain their respective rights, and console/game names and logos may be trademarks of their owners.

---

## Resumen en español

**BatWiiCera para ES-X** adapta la estética de los canales Wii a EmulationStation-X sin abandonar RetroPie. Incluye **Grid nativo de sistemas**, logos localizados, vista de juegos con imágenes y videos, respaldo multimedia `defaultSnapshot`, barra de ayuda y dos diseños: **16:9** y **4:3**.

Instala el repositorio en `~/.emulationstation/themes/BatWiiCera`, selecciona el tema desde ES-X y cambia el formato en **Opciones del tema → Formato de pantalla**.

**Se requiere una versión de ES-X que incluya el [PR #73](https://github.com/Renetrox/EmulationStation-X/pull/73).** El proyecto original pertenece a **Dan Lee**; esta adaptación mantiene la licencia **CC BY-NC-SA 4.0**, sus recursos y créditos.
