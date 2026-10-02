# My Pokémon Squads

A static site, hosted on GitHub Pages, with one squad per game. It switches between English and Simplified Chinese using the EN / 中文 button, and remembers the choice for each browser.

- `index.html`: the whole site (no build step).
- `games.csv`: one row per game (all 21).
- `data/NN.csv`: one file per game squad, e.g. `data/01.csv` (Blue) and `data/16.csv` (Ultra Sun). A game without a file shows on the home page as "Squad not added yet".
- `data/TEMPLATE.csv`: an example of every feature. **Its numbers are illustrative, not your data.**

To preview locally, open a terminal in this folder and run `python -m http.server`, then go to http://localhost:8000. Opening `index.html` by double-clicking won't work, because browsers block reading CSV files from `file://`.

---

## Numbering

| Thing | Format | Example |
|---|---|---|
| Game | two digits, in the order of `games.csv` | `13` = Pokémon Y |
| Pokémon | `game-slot` (slot 1–6, left→right, top→bottom) | `13-6` = Garchomp |
| Form / alternate card | add a letter, starting at `b` | `13-6b` = Mega Garchomp |

## games.csv

`no, title_en, title_zh, generation, year, platform, region_en, region_zh, player_en, player_zh, hometown_en, hometown_zh, color`

`color` is the game's colour (hex RGB, e.g. `#3B5FC4`), used for the home-page spine and the game banner. These colours are my own picks, so change them freely.

**Please check:** I couldn't verify these Simplified Chinese names against a source, so they come from my own knowledge:
- 13 Vaniville Town = 朝露镇
- 17 Elaine = 小菘
- 18 Postwick = 断离镇
- 20 Akari = 小照
- 20 Jubilife Village = 祝庆村
- 21 Cabo Poco = 小匙岬

21 Violet's player (Juliana / 小青) and hometown (Cabo Poco) are also my assumptions, since your Word file has no Violet table.

## data/NN.csv

Two blocks, each starting with a `#name` line:

### `#pokemon`, one row per Pokémon and per form

`id, form, form_en, form_zh, name_en, name_zh, type1, type2, color, ability_en, ability_zh, item_en, item_zh, location_en, location_zh, bst, hp, atk, def, spa, spd, spe`

- **Base row** (`13-6`): fill everything. `color` is the card background. In the sample files it was taken from your PowerPoint slide backgrounds.
- **Form row** (`13-6b`): fill only what changes. **Any blank cell inherits from the base row** (stats, ability, item, location, colour…). A form that lists only `type1` is treated as mono-type.
- **CP is not stored.** The site adds up the six stats.
- Stats are your final level-100 stats. For Gen 1, enter Special in both `spa` and `spd`.
- `type1`/`type2` use English keys: `normal fire water grass electric ice fighting poison ground flying psychic bug rock ghost dragon dark steel fairy` (plus `stellar` for Tera). The site translates them.
- `form` picks the toggle button:

| `form` | Meaning | Button image |
|---|---|---|
| `mega` | Mega Evolution | `icons/form-mega.png` |
| `z` | Z-Move panel | `icons/form-z.png` |
| `dynamax` | Dynamax | `icons/form-dynamax.png` |
| `gigantamax` | Gigantamax | `icons/form-gigantamax.png` |
| `tera` | Terastal (`type1` = Tera type) | `icons/form-tera.png` |
| `primal` | Primal Reversion | `icons/form-primal.png` |
| `alt` | any other form (Therian, Zen Mode, Blade, Sunshine, Origin…) | `icons/form-alt.png` |

Clicking a button switches the card to that form, and clicking it again goes back to base.

### `#moves`, one row per move

`id, slot, variant, move_en, move_zh, type, damage`

- `slot` 1–4 (5 is allowed, e.g. Partner Eevee's extra move). `damage` is your calculated damage. Leave it blank for status moves (shown as —).
- **Form moves override by slot.** A Mega/Dynamax/Tera row only needs the slots that change. Blank name or type cells reuse the base move, so `13-6b,1,,,,,190` means "Dragon Claw, but 190 damage".
- **Z-Moves:** give the form row only the Z-Move, in the slot of the move it powers up. The card works like the Sun & Moon Z panel: that slot is highlighted (with "from <base move>"), and the other slots are dimmed.
- **`variant`** is for per-move styles that don't change the Pokémon itself:
  - `agile` / `strong` (Legends: Arceus): add rows with the same slot and the new damage. Buttons: `icons/style-agile.png`, `icons/style-strong.png`.
  - `plus` (Legends: Z-A Plus Moves): `icons/style-plus.png`.

  A button appears on the card only when that Pokémon has rows for that variant.

---

## Images you'll supply

Every image is optional. Until one exists, the site shows a clean fallback (coloured block, initials, line icon or text pill). PNG throughout. Box art and portraits also accept `.jpg`.

| Folder | File name | What |
|---|---|---|
| `games/` | `01.png` … `21.png` | box art (square crop looks best) |
| `players/` | `01.png` … | main character portrait |
| `pokemon/` | `01-1.png`, `16-4b.png` … | artwork. A form without its own image falls back to the base image |
| `places/` | `pallet-town.png` | hometown and caught-at locations, named by **slug** of the English name |
| `places/` | `17-pallet-town.png` | optional per-game override (checked first) |
| `items/` | `incinium-z.png`, `ampharosite.png` | held items, by slug |
| `types/` | `water.png` … `fairy.png`, `stellar.png` | square type icons (moves + Pokémon types) |
| `types/` | `tera-water.png` … `tera-stellar.png` | Tera type icons |
| `icons/` | `pokeball.png`, `ability.png`, `item.png`, `location.png` | card UI icons |
| `icons/` | `form-*.png`, `style-*.png` | toggle buttons (tables above) |

**Slug rule:** lower-case, accents removed, apostrophes and full stops dropped, anything else non-alphanumeric becomes `-`. For example `Pokémon Mansion` → `pokemon-mansion`, `Hau'oli Outskirts` → `hauoli-outskirts`, `Thrifty Megamart (Abandoned Site)` → `thrifty-megamart-abandoned-site`.
