# Expert-Password-Generator-by-Kavishka99761
Expert Password Generator by Kavishka99761
# SecurePass: Project Documentation

Complete technical reference for the archive `Password_generator_by_Kavishka.rar`. Every fact below was read directly from the archive contents: source files, Git metadata, and assets.

---

## 1. Overview

| Item | Value |
| --- | --- |
| Project name (UI/README) | **SecurePass** |
| Archive name | `Password_generator_by_Kavishka.rar` (RAR5, 54,832 bytes, unencrypted, single volume) |
| Root folder inside archive | `Password_generator_by Kavishka/` |
| Type | Static client-side web app (password and passphrase generator) |
| Stack | Pure HTML, CSS, vanilla JavaScript. No build step, no dependencies, no tracking |
| Visual style | "Liquid glass" UI (backdrop blur, specular highlights, animated aurora canvas) |
| Themes | Dark (default) and light, auto-detected from system |
| Network use | Only the optional breach check (Have I Been Pwned range API) and Google Fonts |
| Upstream repo | `https://github.com/SalmaHazal/Password_generator_website` |
| Working tree state | Heavily modified fork of upstream (4 files modified, 1 new untracked folder) |

The project started as a tiny 12-character password generator by Salma EL HAZAL and was rewritten into a full-featured generator with passphrase mode, entropy estimation, breach checking, and a glass-style UI.

---

## 2. Archive Contents

Total: 39 files and 21 folders (104,960 bytes uncompressed, 49,264 compressed), including Git internals. File sizes below are uncompressed bytes.

### 2.1 Project files

| Path | Size (bytes) | Lines | Purpose |
| --- | ---: | ---: | --- |
| `index.html` | 13,176 | 187 | Page structure, inline SVG icons, dialog, ARIA roles |
| `style.css` | 17,958 | 361 | Themes, glass effect, animation, responsive layout |
| `script.js` | 20,739 | 394 | Generators, strength meter, breach check, canvas background |
| `README.md` | 4,467 | 78 | Project description (CRLF line endings) |
| `images/copy.png` | 824 | n/a | 74 x 86 px PNG, leftover from upstream, unused by current HTML |
| `images/generate.png` | 4,148 | n/a | 65 x 92 px RGBA PNG, leftover from upstream, unused by current HTML |
| `.vscode/launch.json` | 701 | n/a | VS Code Chrome debug launch configs (untracked in Git) |

### 2.2 Git internals (`.git/`)

`config`, `description`, `HEAD`, `FETCH_HEAD`, `index`, `packed-refs`, `info/exclude`, `logs/HEAD`, `logs/refs/heads/master`, `logs/refs/remotes/origin/HEAD`, `refs/heads/master`, `refs/remotes/origin/HEAD`, `objects/pack/pack-bf2f8b0b...{pack,idx,rev}`, `objects/info/commit-graphs/*`, `gk/config` (GitKraken metadata), and 14 default `hooks/*.sample` files.

### 2.3 Referenced but missing

- `assets/bg.webm` and `assets/bg.mp4`: the HTML and README reference an optional looping video background. The `assets/` folder is **not** in the archive. The code handles this gracefully (video stays hidden if the file does not load).

---

## 3. Git History

### 3.1 Repository facts

- Remote `origin`: `https://github.com/SalmaHazal/Password_generator_website.git`
- Branch: `master`, tracking `origin/master`
- `FETCH_HEAD`: `ae9048555981778d5eb70d658403fca4c79e9f4b` from branch `master`
- Config flags: `filemode = false`, `symlinks = false`, `ignorecase = true` (Windows checkout), LFS section present
- Cloned on 2026-11-04 19:39 (+05:30) by Git user `Kavishka99761`; `remote set-head` executed 2026-11-04 19:39 and again 2026-11-05 06:30 (+05:30)
- GitKraken last accessed: 2026-11-04T15:07:52Z

### 3.2 Upstream commits (all by Salma EL HAZAL)

| Hash | Date (UTC) | Message |
| --- | --- | --- |
| `b8656a5` | 2024-04-11 14:52 | First commit (added `images/copy.png`, `images/generate.png`, `index.html`, `script.js`) |
| `6edffb5` | 2024-04-13 16:09 | Create README.md |
| `8f4e1a6` | 2024-04-13 16:10 | Update index.html |
| `ae90485` | 2024-04-13 16:11 | Update README.md (HEAD) |

### 3.3 Local uncommitted changes versus HEAD

| File | Change |
| --- | --- |
| `README.md` | Modified (about 126 lines touched) |
| `index.html` | Modified (about 212 lines touched) |
| `script.js` | Modified (about 416 lines touched) |
| `style.css` | Modified (about 432 lines touched) |
| `.vscode/` | Untracked |

Net: 1,022 insertions, 164 deletions across the four modified files. None of this work is committed.

### 3.4 Original upstream version (for comparison)

The original app was a minimal 21-line `script.js`:

- Fixed length 12 (loop `while (password.length <= 12)` actually produced **13** characters)
- Character pool: lowercase, uppercase, digits, and `!@#$%^&*()-_=+[]{}|:',.<>?~`
- Used `Math.random()`, which is **not** cryptographically secure
- Copy via `navigator.clipboard.writeText`, wired with inline `onclick` attributes
- Font Awesome 4.1.0 and Poppins font; single text input and one button

The rewrite replaces all of this with a CSPRNG-based generator and a much larger feature set.

---

## 4. Features

### 4.1 Generation modes

**Random characters mode**
- Length slider 8 to 64 (default 20)
- Toggles: uppercase, lowercase, numbers, symbols (all on by default)
- Optional "skip look-alikes" removes `I l 1 O 0 o` (and `| ` ` ' "`, which are not in the symbol set so have no effect)
- At least one character of each selected type is guaranteed, then the whole result is shuffled
- If every type is switched off, lowercase is force-enabled

**Passphrase mode**
- 3 to 10 words (default 5)
- Separator: hyphen (default), dot, underscore, space, or none
- Optional capitalization (default on)
- Optional random number 0 to 99 appended (default on)

### 4.2 Security and privacy features

- Cryptographic randomness via `crypto.getRandomValues` with **rejection sampling** (no modulo bias)
- Fisher-Yates shuffle driven by the same secure generator
- `Math.random()` is never used for secrets (it is used only for the decorative background animation)
- Breach check using HIBP k-anonymity: SHA-1 hash computed locally with `crypto.subtle.digest`, only the first 5 hex characters sent, `Add-Padding: true` header set
- Optional clipboard auto-clear 30 seconds after copy (only clears if the clipboard still holds the copied value)
- Passwords are never stored. Only UI preferences are saved in `localStorage` under key `securepass:prefs`
- History is memory-only (wiped on tab close) and shown masked

### 4.3 Strength and entropy

- Live entropy in bits
- Five-level meter
- Offline crack-time estimate assuming 1 trillion guesses per second, average case (half the keyspace)

| Level | Entropy range (bits) | Label |
| ---: | --- | --- |
| 0 | below 40 | Very weak |
| 1 | 40 to below 60 | Weak |
| 2 | 60 to below 80 | Good |
| 3 | 80 to below 100 | Strong |
| 4 | 100 and above | Excellent |

Crack-time output scale: instantly, seconds, minutes, hours, days, months, years, "x million years", "x billion years", and "longer than the age of the universe" (above 1e12 years).

### 4.4 Usability

- Copy button with check-mark confirm animation and toast message
- Show/hide (blur) toggle for the generated value
- "Make 10" bulk dialog with per-item click-to-copy, Regenerate, and Copy all (newline-joined)
- History of the last 6 generated values (masked: first 3 chars, dots, last 2 chars, with timestamp and tap to copy)
- Decode-style "scramble" reveal animation as characters settle left to right (22 frames)
- Character coloring: letters, digits, and symbols each get distinct colors

### 4.5 Keyboard shortcuts

| Key | Action |
| --- | --- |
| `G` | Generate new |
| `C` | Copy current |
| Left / Right arrow (on tab list) | Switch between Random and Passphrase tabs |

Shortcuts are ignored when focus is in an input, select, or textarea, when a modifier key (Ctrl, Meta, Alt) is held, or while the bulk dialog is open.

### 4.6 Accessibility

- `lang="en"`, semantic landmarks (`main`, `section`, `header`, `article`)
- Tab roles (`tablist`, `tab`, `tabpanel`) with `aria-selected`, `aria-controls`, `aria-labelledby`
- `aria-live="polite"` on the output, breach message, and toast
- `aria-pressed` on show/hide button, `aria-busy` on breach button while checking
- `prefers-reduced-motion` support (skips scramble, ripple, view transition, background animation, hides video)
- `prefers-contrast: more` support (opaque panels)
- `:focus-visible` outlines
- Decorative elements marked `aria-hidden`

### 4.7 Theming and visuals

- Light and dark themes, auto-selected from `prefers-color-scheme`, user choice saved
- Theme toggle uses the View Transitions API with a circular reveal from the button (instant switch fallback)
- Animated canvas background: 4 to 5 drifting radial-gradient "orbs" plus 22 to 46 rising particles, with pointer parallax. Pauses when the tab is hidden
- Film-grain overlay (inline SVG `feTurbulence`)
- Pointer-following specular highlight on glass surfaces
- Button ripple effect, spring-physics easing, animated tip icons (stroke-draw)
- Fallbacks: solid panel where `backdrop-filter` is unsupported

---

## 5. Algorithms and Formulas

### 5.1 Unbiased random integer

```js
function randInt(max) {
  const buf = new Uint32Array(1);
  const limit = Math.floor(0x100000000 / max) * max;
  do { crypto.getRandomValues(buf); } while (buf[0] >= limit);
  return buf[0] % max;
}
```

Values at or above `limit` are discarded so every outcome in `[0, max)` is equally likely.

### 5.2 Random password construction (`genRandom`)

1. Build one pool per enabled type, removing ambiguous characters if requested.
2. Take one random character from each pool (guarantee).
3. Fill remaining positions from the combined pool.
4. Shuffle with Fisher-Yates.
5. Length used is `max(state.length, number of pools)`.

Character sets:

| Set | Characters |
| --- | --- |
| lower | `abcdefghijklmnopqrstuvwxyz` (26) |
| upper | `ABCDEFGHIJKLMNOPQRSTUVWXYZ` (26) |
| digits | `0123456789` (10) |
| symbols | `!@#$%^&*()-_=+[]{};:,.<>?~` (26) |

Full pool with all four types: 88 characters, about 6.46 bits per character. Default 20-character password: about 129.2 bits.

### 5.3 Passphrase construction (`genPhrase`)

- Picks `words` random entries from `WORDS` (with repetition possible)
- Optional capitalization of first letter
- Joins with the chosen separator
- Optionally appends the separator and `randInt(100)` (0 to 99)

Entropy formula: `words x log2(wordlist size) + log2(100)` if a number is added.

### 5.4 Entropy displayed

- Random: `length x log2(pool size)`
- Passphrase: as above

### 5.5 Crack time

```
seconds = 2^(bits - 1) / 1e12
```

### 5.6 Breach check flow

1. Encode password as UTF-8, compute SHA-1, uppercase hex.
2. Send `GET https://api.pwnedpasswords.com/range/<first 5 hex chars>` with `Add-Padding: true`.
3. Parse the response lines `SUFFIX:COUNT`, find the remaining 35-character suffix locally.
4. Show "Seen N times in known breaches" (red) or "Not found in known breaches" (green); on error show a connection message.

---

## 6. Word List (Passphrase Mode)

Defined as `WORDS` in `script.js` (inline template string, split on whitespace, de-duplicated with `Set`).

| Property | Value |
| --- | --- |
| Distinct words | **258** |
| Length range | 3 to 8 letters |
| Bits per word | log2(258) = **8.01** |
| Examples | acorn, amber, anchor, banjo, cactus, delta, falcon, glacier, harbor, igloo, jaguar, kayak, lantern, mango, nomad, oasis, panda, quartz, rocket, saffron, tundra, umbrella, velvet, walrus, yonder, zebra, zenith |

Passphrase strength with defaults (5 words, number on): 5 x 8.01 + 6.64 = about **46.7 bits**, which the meter labels **Weak** (40 to 60). Reaching the "Good" level (60+ bits) needs 7 words; "Strong" (80+) needs 10 words.

Reference table (number appended):

| Words | Entropy (bits) | Meter label |
| ---: | ---: | --- |
| 3 | 30.7 | Very weak |
| 4 | 38.7 | Very weak |
| 5 | 46.7 | Weak |
| 6 | 54.7 | Weak |
| 7 | 62.7 | Good |
| 8 | 70.7 | Good |
| 9 | 78.7 | Good |
| 10 | 86.8 | Strong |

---

## 7. Code Architecture

### 7.1 `index.html`

- `<html lang="en" data-theme="dark">`
- Meta: description, viewport (`viewport-fit=cover`), `color-scheme`, `theme-color` `#070b1a`
- Inline SVG favicon (gradient rounded square with lock)
- Google Fonts: **Sora** (300, 400, 600, 700) and **JetBrains Mono** (400, 600)
- Background layers: `<video id="bgVideo">`, `<canvas id="bg">`, `.grain`
- Main card sections: header with brand and theme button; tab list; output with eye and copy buttons; strength meter; action buttons (Generate new, Make 10, Check for breaches); random panel; passphrase panel; clipboard auto-clear switch; history `<details>`
- Tips section "What makes a password strong" with four cards: **Go long**, **Be truly random**, **One account, one password**, **Keep yourself out**
- Footer note recommending a password manager and two-factor authentication
- Bulk `<dialog id="bulkDlg">` and toast `<div id="toast">`
- Contains 44 elements with IDs; single script tag `script.js`

### 7.2 `script.js` (IIFE, strict mode)

| Section | Responsibility |
| --- | --- |
| Crypto-secure randomness | `randInt`, `shuffle` |
| Data | `SETS`, `AMBIGUOUS`, `WORDS` |
| State | `defaults`, `state`, `savePrefs`, `current`, `history`, timers |
| Generators | `genRandom`, `genPhrase`, `generate` |
| Strength | `LEVELS`, `crackTime`, `updateMeter` |
| Rendering | `render`, `scramble`, `refresh` |
| Clipboard | `writeClip` (async API with `execCommand` fallback), `copy` with auto-clear |
| Toast | `toast` |
| History | `pushHistory`, `renderHistory` |
| Breach check | `breachCheck` |
| Bulk dialog | `fillBulk` |
| UI wiring | `setMode`, `syncControls`, `applyTheme`, `init` |
| Background | `bgPalette`, `resizeBg`, `drawBg`, `loop` |

Default state:

```js
{ mode: 'random', length: 20, upper: true, lower: true, digits: true, symbols: true, ambig: false,
  words: 5, sep: '-', cap: true, num: true, autoclear: true, theme: null }
```

Notes on behavior:
- History is added only by explicit generation (Generate button, `G` key, mode switch), not by slider or toggle changes.
- Clipboard auto-clear needs clipboard read permission; if denied, the clipboard is left alone.
- The preference object (including theme) is stored in `localStorage`; failures are silently ignored.

### 7.3 `style.css` (361 lines)

Design tokens (`:root`):

| Token | Dark | Light |
| --- | --- | --- |
| `--ink` (background) | `#070b1a` | `#e9efff` |
| `--text` | `#eef2ff` | `#0e1633` |
| `--muted` | `#a1aed0` | `#4b5a80` |
| `--accent` | `#5ef2c3` | `#0ea37a` |
| `--accent-2` | `#7c8cff` | `#5566ff` |
| `--accent-3` | `#ff7ad9` | `#e0489f` |
| `--on-accent` | `#04251c` | `#ffffff` |
| `--c-letter` / `--c-digit` / `--c-symbol` | `#eef2ff` / `#7fd4ff` / `#ff9bd9` | `#0e1633` / `#0a6fb0` / `#c02a86` |

Strength colors: `--lv0 #ff5d6c`, `--lv1 #ff9f43`, `--lv2 #ffd75e`, `--lv3 #7be495`, `--lv4 #5ef2c3`.

Fonts: `--font` Sora with system fallbacks; `--mono` JetBrains Mono with system mono fallbacks. Easing: `--ease-spring: cubic-bezier(.34,1.56,.64,1)`, `--ease-out: cubic-bezier(.16,1,.3,1)`.

Keyframes: `rise`, `ripple`, `swap`, `draw`, `pop`, `fade`, `reveal`.

Breakpoints and media queries: `max-width:560px` (actions grid), `max-width:480px` (single-column switches), `min-width:520px` (output actions row), `prefers-reduced-motion`, `prefers-contrast: more`, `@supports not (backdrop-filter)`.

Layout: card width `min(100% - 2rem, 760px)`, card radius 34px, uses `100dvh`, `env(safe-area-inset-*)`, and `color-mix()`.

---

## 8. Running the Project

```bash
# Option 1: open index.html directly in a browser

# Option 2: serve the folder
npx serve .
# or
python3 -m http.server 8080
```

Use `https://` or `localhost` in production because the Clipboard API and `crypto.subtle` require a secure context.

`.vscode/launch.json` provides two Chrome launch configurations:

1. **Launch Chrome**: opens `http://localhost:8080` with `webRoot` set to the workspace folder.
2. **Open index.html**: opens a hard-coded Windows path under the author's `Downloads\Password_generator_website\index.html`. This path is machine-specific and should be changed or removed before sharing.

---

## 9. Browser Support

Current Chrome, Edge, Safari, and Firefox. Glass blur falls back to a solid panel without `backdrop-filter`. View Transitions fall back to an instant theme switch. Range-input styling includes both WebKit and Firefox thumb rules.

---

## 10. README Claims Versus Actual Code

| Topic | README says | Code shows |
| --- | --- | --- |
| Word list size | about 330 words, about 8.4 bits per word | **258** words, **8.01** bits per word |
| `assets/` folder | Lists `assets/` with optional video | Folder not present in the archive |
| Ambiguous characters | `I l 1 O 0 o` | Also defines `| ` ` ' "`, which are not in the symbol set (no effect) |
| Entropy for random mode | `length x log2(alphabet size)` | Matches code |
| Guaranteed one of each type | Yes | Matches code (this slightly lowers true entropy versus the displayed figure) |

---

## 11. Observations and Recommendations

**Strengths**
- Correct use of CSPRNG with rejection sampling and unbiased shuffle
- Privacy-respecting breach check (k-anonymity plus padding)
- Passwords never persisted, history in memory only
- Good accessibility and reduced-motion handling
- Zero dependencies, small footprint (about 52 KB of source)

**Issues and improvement ideas**
1. **Fix README word-list numbers** (258 words / 8.01 bits) or enlarge the list. The default 5-word passphrase scores only about 46.7 bits ("Weak"). Swapping in the EFF large wordlist (7,776 words, 12.9 bits per word) would give about 71 bits for 5 words.
2. **Entropy is slightly overstated** in random mode because the "one of each type" guarantee removes some possible outputs. The effect is small at length 20 but larger at length 8.
3. **Default passphrase is rated Weak**, which conflicts with the UI hint "5 or more random words is strong".
4. **Add the missing `assets/` folder** or remove the references from the README/HTML.
5. **Remove unused files**: `images/copy.png` and `images/generate.png` are no longer referenced.
6. **Commit work**: all rewrite changes are uncommitted; consider a new branch or fork under the author's own repository and update the remote.
7. **Clean `.vscode/launch.json`** of the hard-coded personal path before publishing.
8. **Add a license file** (none present) and attribute the upstream author, since this is derived from `SalmaHazal/Password_generator_website`.
9. **Self-host fonts** to remove the Google Fonts network call if full offline privacy is the goal.
10. **Clipboard auto-clear** depends on read permission; document that it may silently do nothing in some browsers.

---

## 12. Quick Reference

- **Name:** SecurePass
- **Files:** `index.html`, `style.css`, `script.js`, `README.md`, `images/` (2 PNGs), `.vscode/launch.json`, `.git/`
- **Source size:** HTML 13,176 B, CSS 17,958 B, JS 20,739 B, README 4,467 B
- **Default password:** 20 chars, all four types, about 129.2 bits, "Excellent"
- **Default passphrase:** 5 capitalized words, hyphen separator, plus number, about 46.7 bits, "Weak"
- **Storage key:** `localStorage["securepass:prefs"]`
- **External calls:** `fonts.googleapis.com`, `fonts.gstatic.com`, `api.pwnedpasswords.com/range/<5-char-prefix>`
- **Shortcuts:** `G` generate, `C` copy, arrow keys switch tabs
- **Upstream credit:** Salma EL HAZAL (`SalmaHazal/Password_generator_website`)
- **Local author:** Kavishka (Git user `Kavishka99761`, clone dated 2026-11-04)
