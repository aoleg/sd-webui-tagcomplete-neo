<div align="center">

<img src=".github/banner.png" alt="TagComplete Neo"/>

[![Forge Neo](https://img.shields.io/badge/Forge-Neo-blue)](https://github.com/Haoming02/sd-webui-forge-classic/tree/neo)
[![Gradio](https://img.shields.io/badge/Gradio-4.39.0+-orange)](https://gradio.app/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

> **Extension for [Stable Diffusion WebUI Forge - Neo](https://github.com/Haoming02/sd-webui-forge-classic/tree/neo)**

</div>

# 🏷️ TagComplete Neo

Tag autocompletion for Forge Neo — suggests Danbooru/e621 tags, LoRA/embedding names, wildcards, and chants as you type, with automatic trigger word injection and CivitAI lookup support.

Fork of [a1111-sd-webui-tagcomplete](https://github.com/DominikDoom/a1111-sd-webui-tagcomplete) by [DominikDoom](https://github.com/DominikDoom), maintained here exclusively for Forge Neo. If you are not running Forge Neo, use the [original extension](https://github.com/DominikDoom/a1111-sd-webui-tagcomplete) instead.

---

## 📋 Table of Contents

- [What's New](#-whats-new)
- [Changelog](#-changelog)
- [Roadmap](#️-roadmap)
- [Features](#-features)
- [Installation](#-installation)
- [Tag Lists](#-tag-lists)
- [Credits](#-credits)

---

## 🆕 What's New

### v0.2.5: LoRA architecture filter and recent-first sorting

- **LoRAs that match your model**: LoRA suggestions follow the architecture of Forge Neo's UI Preset. LoRAs made for another model are hidden, and LoRAs whose architecture cannot be determined are listed last, marked `unknown`. Choose "Show matching first" to list the others last instead of hiding them ⭐
- **Recent, then frequent**: a new frequency sorting choice lists the last few tags or LoRAs you used first, then the ones you use often, then the usual order ⭐
- **Alphabetical across folders**: the new "Name (ignore folders)" model sort order sorts LoRAs and wildcards by file name, so subfolders no longer group together ⭐
- **`.pt` and `.ckpt` LoRAs** are no longer suggested; type them by hand

### v0.2.4: Hidden-folder LoRAs and numbered checkpoints

- **LoRAs in hidden folders**: LoRA/LyCORIS files in folders whose names start with `.` are listed when Forge Neo's "List the models/files under hidden directories" option is on (the default)
- **Numbered checkpoints keep their names**: when several checkpoints share one alias (`lora1-0000001000`, `lora1-0000002000`), each is inserted by its filename; a single checkpoint still uses its alias

### v0.2.3: Wildcard contents fix

- **Wildcard files show their contents again**: after choosing a wildcard file, the popup lists the lines inside it instead of listing the files again ([#4](https://github.com/eduardoabreu81/sd-webui-tagcomplete-neo/issues/4))

### v0.2.2 — ANIMA artist prefix & embedding fixes

- **ANIMA artist prefix** — new setting that automatically adds `@` before Danbooru artist tags when an ANIMA-based checkpoint is loaded ⭐
- **No more "Old webui version" warning** — models without embedding support (Anima, Chroma, Qwen, Lumina, Wan…) no longer print a misleading warning on every load
- **`.safetensors` embeddings listed** — embedding suggestions now include `.safetensors` files on every model
- **Suggestions stay in sync while deleting** — the dropdown updates immediately when you erase text

### v0.2.1 — Mobile backspace fix & mid-prompt LoRA keywords

- **Backspace no longer lags on mobile** — holding the delete key no longer triggers heavy translation handlers, making deletion smooth even on older phones
- **Mid-prompt LoRA keywords fixed** — when using "After LORA/LyCO" insertion, trigger words now insert correctly even if the LoRA token is in the middle of the prompt (no more overwritten text)
- **Multi-word tag search** — type any word from a tag with underscores and it will be found (e.g. `towards` → `walking_towards_viewer`)

### v0.2.0 — Smoother typing & mobile support

- **Faster tag suggestions** — the autocomplete list now appears more quickly, even with large tag databases like the merged Danbooru/e621 list
- **Much smoother typing on mobile** — typing in the prompt box no longer lags or stutters on phones and tablets
- **Lower input delay** — reduced waiting time between keystrokes and results appearing on screen
- **Indexed search toggle** — new setting to switch between fast prefix-indexed mode and legacy full-scan mode ⭐
- **Status indicator** — a small colored dot in the toolbar shows when the extension is loading (orange), ready (green), or encountered an error (red) ⭐

---

---

## 📖 Changelog

### v0.2.5: LoRA architecture filter and recent-first sorting
- New setting "Filter Loras by the architecture of the UI preset": Off, Hide other architectures (default), Show matching first. The filter turns itself off when the UI Preset is missing or is not an architecture it knows
- Each LoRA's architecture is read from its safetensors header (tensor names, shapes, metadata), never its weights, and cached in `tags/temp/lora_arch_cache.json`. An "sd version" set in Forge's Extra Networks metadata editor takes priority. Recognised: SD1, SDXL, Flux.1 (including Chroma), Flux.2 Klein, Qwen-Image, Qwen-Image-2.1, Lumina 2, Z-Image, Wan, Anima, Ernie-Image, PiD, Krea 2, MOSS-Image, and Fibo 2, Flux 3, MiniMax H3, Ideogram 4 and YuE2, which have no built-in preset and are hidden under every preset unless an extension adds one. LoRAs that only change top-level layers (many sliders) are recognised too
- A LoRA whose header fits several architectures is offered under each of them
- `.pt` and `.ckpt` LoRAs are no longer suggested; they still count for the alias collision rule
- New frequency sorting choice "Recent, then frequent", with the setting "Number of recently used results listed first" (default 3). Recently used results skip the minimum use count. The other sorting choices are unchanged
- New model sort order "Name (ignore folders)": sorts by file name instead of the path inside the model folder

### v0.2.4: Hidden-folder LoRAs and numbered checkpoints
- LoRA/LyCORIS files in hidden folders are scanned, and their previews, info, trigger words and hashes are found, when Forge Neo's `list_hidden_files` option is on. Only folders inside the LoRA directory count as hidden
- An alias shared by more than one file (as a filename or an alias, case-insensitive) falls back to the filename, as Forge Neo does. Training tools write the same `ss_output_name` into every epoch/step checkpoint, which made them all insert as one name
- The "Filename" preference for LoRA names is honoured before a model is loaded

### v0.2.3: Wildcard contents fix
- Choosing a wildcard file now shows the lines inside it. Autocomplete triggered by the extension's own inserted text runs immediately again, while typing stays debounced (#4)

### v0.2.2 — ANIMA artist prefix & embedding fixes
- New `Auto-prefix Danbooru artist tags with '@' for ANIMA-based checkpoints` setting (Off/Auto); detects the checkpoint's CivitAI base model by hash, cached locally
- Models whose text encoder has no textual inversion support no longer trigger the "Old webui version or unrecognized model shape" warning; embeddings are listed from the folder silently
- SDXL refiner embeddings are now read from `text_processing_engine_g`
- Folder-scan fallback includes `.safetensors` and matches extensions case-insensitively
- Autocomplete runs immediately on delete so the dropdown stays in sync with the prompt

### v0.2.1 — Mobile backspace fix & mid-prompt LoRA keywords
- Backspace no longer triggers heavy translation handlers, eliminating lag when holding delete on mobile
- Fixed "After LORA/LyCO" trigger word insertion corrupting text when the LoRA token is in the middle of the prompt
- Multi-word tag search — type any word from a tag with underscores (e.g. `towards` → `walking_towards_viewer`)

### v0.2.0 — Smoother typing & mobile support
- Tag search significantly faster thanks to an internal prefix index built on load
- Input debounce tightened to keep up with fast typists without causing frame drops
- Reduced DOM and memory churn during dropdown rendering
- Fixed a mouseover listener leak that could accumulate over long sessions
- Startup no longer blocks typing — the tag index is built in small chunks so the prompt box stays responsive while the extension initializes
- Extension parsers now load in parallel, cutting startup time from ~43s to ~7s on slower machines
- Status indicator dot shows loading / ready / error state directly in the Forge Neo toolbar
- Indexed search can be toggled on/off in Settings → Tag Autocomplete
- Fixed crash when inserting tags containing apostrophes
- Fixed broken bold highlight on tags with special characters

### v0.1.2 — Bug fix batch
- Embedding manual refresh (`tac_forceRefreshEmbeddings`) no longer throws `TypeError` on Forge Neo
- LoRA list always populated from filesystem scan; `lora.available_loras` used only for alias enrichment
- `get_embeddings()` resolves symlinks before `relative_to()` — no more crash with linked embedding folders
- `transaction()` in frequency DB initialises `conn = None` before `try` block — prevents `NameError` on failed DB creation

### v0.1.1 — LoRA alias fix
- LoRA/LyCORIS completion now inserts the alias Forge Neo expects, eliminating token blink
- Respects the "Alias from file" / "Filename" setting in Extra Networks

### v0.1.0 — Forge Neo Baseline
- Full Forge Neo / Gradio 4 compatibility
- Booru tags, initialization, and embedding reload fixed
- CivitAI trigger word lookup with SHA256 cache
- "After LoRA/LyCO" insertion option

---

## 🗺️ Roadmap

### v0.1.2 — Bug fix batch ✅
- Embedding manual refresh fixed (Forge Neo removed `force_reload` kwarg) ✅
- LoRA list always populated at startup ✅
- Symlinked embeddings no longer crash on model load ✅
- Frequency database `NameError` on first run fixed ✅

### v0.1.1 — LoRA alias fix ✅
- LoRA/LyCORIS token matches Forge Neo's expected alias ✅

### v0.1.0 — Forge Neo Baseline ✅
- Forge Neo / Gradio 4 compatibility ✅
- Booru tag display fixed (Gradio 4 selectors) ✅
- Reliable re-initialization after WebUI reconnect ✅
- Embedding reload hardened ✅
- Extension resilience after Forge updates ✅
- CivitAI trigger word lookup with SHA256 cache ✅
- "After LoRA/LyCO" insertion option ✅

### v0.2.5: LoRA architecture filter and recent-first sorting ✅
- LoRA suggestions follow the UI Preset's architecture ✅
- "Recent, then frequent" frequency sorting ✅
- "Name (ignore folders)" model sort order ✅

### v0.2.4: Hidden-folder LoRAs and numbered checkpoints ✅
- LoRAs in hidden folders follow `list_hidden_files` ✅
- Numbered checkpoints that share an alias insert by filename ✅

### v0.2.3: Wildcard contents fix ✅
- Wildcard file contents shown after choosing a file ✅

### v0.2.2 — ANIMA artist prefix & embedding fixes ✅
- `@` prefix for Danbooru artist tags on ANIMA checkpoints ✅
- Misleading embedding fallback warning removed ✅
- `.safetensors` embeddings in folder-scan fallback ✅
- Dropdown sync on delete ✅

### v0.2.1 — Mobile backspace fix & mid-prompt LoRA keywords ✅
- Backspace lag eliminated (updateRuby skipped on delete) ✅
- Mid-prompt "After LORA/LyCO" trigger word insertion fixed ✅
- Multi-word tag search (any word of a multi-word tag) ✅

### v0.2.0 — Smoother typing & mobile support ✅
- Prefix-indexed tag search (toggleable) ✅
- Tighter debounce and delete-event skipping ✅
- DOM batching + deferred rendering ✅
- Parallel extension loading (43s → ~7s startup) ✅
- Status indicator dot in toolbar ✅
- Apostrophe crash and highlight fixes ✅
- Mobile typing no longer lags or stutters ✅

### v0.3.0 — Tag data & relevance (planned)
- Update Danbooru and e621 tag lists with current data
- Better tag coverage for Pony / NoobAI / Illustrious models
- Sort suggestions by relevance to tags already in the prompt
- Use multiple tag list files simultaneously

### v0.4.0 — Smart matching (planned)
- Fuzzy matching (e.g. `detco` → `detached_collar`)
- Auto-switch tag list based on the loaded model

### v1.0.0 — Stable (planned)
- All known issues resolved

---

## 🎯 Features

> ⭐ = added or fixed in this Neo fork · everything else is original work by [DominikDoom](https://github.com/DominikDoom)

### 🏷️ Tag Autocompletion

- **Instant suggestions** as you type, sourced from Danbooru, e621, or merged lists
- **Indexed search** — built-in prefix index for near-instant filtering on large tag lists; can be disabled in settings if needed ⭐
- **Keyboard navigation** — arrow keys, Tab, Enter, Escape, all configurable
- **Tag color coding** by category, with post count for relevance
- **Alias and translation search** — find tags by their alternate names or translated terms
- **Frequency sorting** — remembers your most-used tags and promotes them to the top; "Recent, then frequent" lists the last few you used first ⭐
- **Status indicator** — colored dot in the toolbar shows when the extension is loading, ready, or in error state ⭐

### ➕ Extra Networks

- **LoRA and LyCORIS autocomplete** triggered by `<`
- **Architecture filter** — suggests only LoRAs made for the UI Preset's model, read from each file's header ⭐
- **Embedding autocomplete** triggered by `<e:`
- **Thumbnail preview** in the completion popup
- **Correct alias insertion** — uses the same identifier Forge Neo expects (`ss_output_name` or filename, respecting your Extra Networks setting) so tokens never blink ⭐
- **Trigger word injection** on LoRA selection — inserts activation keywords automatically
  - Fetches from CivitAI if not set locally ⭐
  - Cached by SHA256 — only re-fetched when the model file changes ⭐
  - Configurable position: Start of prompt, End of prompt, Before or After the LoRA token ⭐

### ✳️ Wildcards

- **Wildcard file autocomplete** triggered by `__`
- **Nested folder support**
- **YAML wildcard format** (UMI-compatible)

### 🪄 Chants

- **Prompt preset completion** for longer phrase templates stored in JSON files
- Triggered by `<c:` or `<chant:`

---

## 📦 Installation

1. Open Forge Neo WebUI
2. Go to **Extensions** → **Install from URL**
3. Paste: `https://github.com/eduardoabreu81/sd-webui-tagcomplete-neo`
4. Click **Install** and reload the WebUI

> ⚠️ This extension requires **Forge Neo**. It will not work on Automatic1111 or Forge Classic.

---

## 🗂️ Tag Lists

| File | Source | Best for |
|---|---|---|
| `danbooru.csv` | Danbooru top-100k | Anime models (SD 1.5, SDXL) |
| `danbooru_2025.csv` | Danbooru updated 2025 | Anime models (SD 1.5, SDXL) |
| `e621.csv` | e621 top-100k | Furry / anthro models |
| `e621_sfw.csv` | e621 SFW subset | Furry / anthro models (safe) |
| `danbooru_e621_merged.csv` | Merged + unified categories | Pony, NoobAI, Illustrious |
| `derpibooru.csv` | Derpibooru tags | MLP / cartoon models |
| `extra-quality-tags.csv` | Curated set | Quality booster tags |
| `EnglishDictionary.csv` | English dictionary | Photorealistic / non-booru models |
| `demo-chants.json` | Demo presets | Prompt templates |
| `noob_characters-chants.json` | NoobAI character presets | Character-based prompts |

To switch lists, change **Tag filename** in **Settings → Tag Autocomplete**.

---

## 📄 Credits

- **[a1111-sd-webui-tagcomplete](https://github.com/DominikDoom/a1111-sd-webui-tagcomplete)** by DominikDoom — original project, all core functionality
- **[sd-webui-tagcomplete-neo](https://github.com/eduardoabreu81/sd-webui-tagcomplete-neo)** by [Eduardo Abreu](https://github.com/eduardoabreu81) — Forge Neo fork
- **[Forge Neo](https://github.com/Haoming02/sd-webui-forge-classic/tree/neo)** by Haoming02

---

## 📜 License

MIT — see [LICENSE](LICENSE)

---

<div align="center">

Made with ❤️ for the Stable Diffusion community

**[Report Bug](https://github.com/eduardoabreu81/sd-webui-tagcomplete-neo/issues)** • **[Request Feature](https://github.com/eduardoabreu81/sd-webui-tagcomplete-neo/issues)** • **[☕ Ko-fi](https://ko-fi.com/eduardoabreu81)**

</div>