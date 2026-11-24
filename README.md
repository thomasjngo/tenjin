# Tenjin

A fast, private reader for comics and books that runs entirely in your browser. Nothing is uploaded anywhere: your files, covers and reading progress stay on your device.

Tenjin (天神) is named after the Japanese deity of learning and scholarship.

**Live version:** https://thomasjngo.github.io/tenjin/

## Supported formats

| Format | Works out of the box? |
| --- | --- |
| **CBZ** (and ZIP archives of images) | Yes |
| **PDF** | Yes. The PDF engine (PDF.js) is downloaded from a CDN the first time you open a PDF, so that needs an internet connection. |
| **EPUB** (comic or manga EPUBs made of page images) | Yes. Pages follow the book's reading order. |
| **CBT** (tar archives of images) | Yes |
| **Loose images** (JPG, PNG, WebP, GIF, AVIF, BMP) | Yes. Select or drop several images and they open as one comic, sorted by name. |
| **CBR** (RAR) and **CB7** (7-Zip) | Optional. Add the three files in the `libarchive/` folder to your site (see Run it). |

Files are detected by their contents as well as their extension, so a mislabelled `.cbr` that is really a ZIP still opens.

## Features

### Reading
- **Page mode** shows one page at a time. **Scroll mode** is a continuous vertical scroll, which suits webtoons and long strips.
- **Two-page spreads** work in both modes. In Page mode, facing pages sit side by side. In Scroll mode, pages are laid out in rows of two. In both, a page that is already landscape is shown on its own. The first page can be shown alone like a cover (Settings).
- **The page gap only affects the space between rows.** In two-page Scroll mode, paired pages touch side by side and the gap setting applies only to the vertical space below each row.
- **Seamless wide pages in single-column Scroll mode.** If a mostly-portrait book has the occasional landscape double-page spread, the spread is split into its two halves, stacked one after the other, so every page keeps the same width and text size. Choose **Keep whole** in Settings to leave spreads intact.
- **Manga mode** reads right to left: arrow keys, click zones, the slider, spreads and split pages all flip.
- **Fit modes:** Width, Fit (the whole page), and Actual size.
- **Zoom** from 25% to 400%. Type a percentage in the toolbar field and press Enter, use **+** and **−** (10% steps), or use `Ctrl` + mouse wheel (zooms toward the cursor) or a trackpad pinch. Double-click the field to reset. Drag a zoomed page to move around it.
- **Center lock.** When you zoom in, a **Center lock** switch appears at the bottom of the page (or press `c`). While it is on, the page stays centered and can't drift sideways; turn it off to pan freely. It is also in Settings.
- **Touch gestures.** Swipe left or right to turn pages, and double-tap to zoom in and out. Both can be turned off in Settings. With double-tap on, a single tap waits a moment to see whether a second tap follows.
- **Bookmarks.** Press `b` (or the star button) to bookmark the page you're on. Bookmarked pages get a star on their thumbnail, and the list button (or `Shift+B`) shows all your bookmarks with previews so you can jump back.
- **Your page is kept** when you switch between one-page and two-page mode, or between Page and Scroll mode.
- **Smart scrolling:** `Space` scrolls down and turns the page when you reach the bottom. `Shift+Space` does the reverse.
- **Page previews** can sit at the **bottom, top, left or right** (Settings). Click a thumbnail to jump there. The current page, or both pages of a spread, is highlighted. Hover over a thumbnail to see an enlarged preview (can be turned off).
- **Page slider and jump box:** drag the slider, or type a page number (press `g`).
- **Page number indicator:** an optional "12 / 40" badge in the bottom-right corner.
- **Motion:** spring-style transitions throughout. Pages slide and fade as you turn them, the segmented controls glide, toolbars slide in, dialogs and menus pop, and library cards cascade in. Everything switches off automatically if your system asks for reduced motion.
- **Themes:** automatic (follows your system), light or dark, in an Apple-inspired style with frosted toolbars, rounded cards and soft shadows.
- **Fullscreen** and a **hide toolbars** option (`h`, or click the middle of the page).
- Drag and drop a file anywhere on the page to open it.

### Library
- Click **Library** (or press `l`) and choose **This device** or **Online**.
- **This device:** choose a folder and Tenjin scans it and all subfolders for CBZ, CBR, CB7, CBT, PDF and EPUB files. In Chrome and Edge the folder is remembered; next visit you may be asked to click **Reconnect** once to re-grant access.
- Each comic gets a **cover preview**. Covers are generated as you scroll and saved so they only need to be made once.
- **Reading progress** is tracked for every comic: a progress bar with "Page 12 of 40", "Not started", or "Finished". Finished comics have a slightly greyed-out cover.
- **Resume where you left off:** opening a comic jumps back to the page you were on. **Continue reading** opens whatever you read most recently.
- **Series and volumes.** Tenjin reads each CBZ's `ComicInfo.xml` (series, volume, title) and falls back to folder and file names such as `Berserk v03.cbz`. Volumes are grouped by series, in volume order, with a "3 of 12 read" count. Switch grouping on or off with the **Series** button.
- **Next volume.** When you reach the end of a book, a sheet offers the next volume in the series. The library's **Continue reading** button also suggests the next unread volume when nothing is in progress.
- **Grid or list view,** and a **cover-size slider** for the grid.
- **Select several comics.** Click **Select** (or Ctrl/Cmd-click a comic), pick as many as you like, or use **Select all** or a series' **Select** button. Then **Mark read**, **Mark unread** or **Download** them in one go. Press `Esc` to leave selection.
- **Search**, **filter** (All, Reading, Unread, Finished) and **sort** (Name, Recently read, Newest file).
- **Right-click a comic** (or long-press on touch, or `Shift+F10` / the Menu key) for **Open**, **Mark as read**, **Mark as unread** and **Regenerate cover**. Online comics also get **Download for offline**, **Remove download** and **Save to device…**.

### Online libraries
In the library, choose **Online**, click **Add…** and paste a public folder link. You can save **several online libraries** and switch between them from the menu next to the buttons; **Edit** changes the current one, and its dialog has a button to remove it. Tenjin works out what kind of link it is, reads the folder structure (including subfolders), and lists every comic with covers and reading progress. **Your link is remembered**, so next time you open Tenjin the library is right there, and the last list stays visible offline.

| Link | What you need | Notes |
| --- | --- | --- |
| **Google Drive folder** shared as "Anyone with the link" | A free Google API key | Subfolders are included. Setup is below. |
| **Web folder** (a page that lists files, as served by Apache, nginx or `python -m http.server`) | The server must allow cross-site reads (CORS) | Follows subfolders up to five levels deep. |
| **Manifest** (a JSON file listing your links) | The server must allow CORS | Works with any host and any folder structure. Format below. |
| A direct link to one `.cbz`, `.pdf`, `.epub` and so on | CORS | Adds a single comic. |

**Manifest format.** `path` becomes the folder structure, `url` can be relative to the manifest, and `size` is optional:

```json
{
  "items": [
    { "path": "Series A/Vol 1.cbz", "url": "https://example.com/files/a1.cbz", "size": 52428800 },
    { "path": "Series A/Vol 2.cbz", "url": "a2.cbz" }
  ]
}
```

**About CORS.** A browser only lets a web page read another site if that site allows it. Self-hosted servers and storage buckets can usually be configured to allow it. Many file hosts do not. If a link opens in a browser tab but Tenjin can't read it, this is why.

**Downloading:**
- Click the download button on a cover, or use the right-click menu, to download one book for offline reading. A ring fills around the button and turns into a check mark when it is saved.
- **Download all** saves the whole folder one book at a time, with an animated progress bar and a Cancel button.
- **Save to folder…** (Chrome and Edge) writes real files, keeping your subfolders, into a folder on your computer.
- Downloaded books open instantly and work without an internet connection.

**Getting a Google API key (about two minutes):**
1. Go to the [Google Cloud Console](https://console.cloud.google.com/) and create a project.
2. Open **APIs & Services → Library**, search for **Google Drive API** and click **Enable**.
3. Open **APIs & Services → Credentials → Create credentials → API key**.
4. Recommended: edit the key and, under **Application restrictions**, choose **Websites** and add `https://thomasjngo.github.io/*`. Under **API restrictions**, allow only the Google Drive API.
5. In Google Drive, share your folder as **Anyone with the link**, then paste the folder link and the key into Tenjin.

**What can't be read from a web page:** Proton Drive links are end-to-end encrypted, so only Proton's own app can decrypt them. Dropbox, OneDrive and SharePoint, Mega, MediaFire and iCloud links don't offer a folder listing that a web page can read. For these, download the folder with their app and use **This device → Choose folder**, or host the files on a web server you control.

### Backup and restore
In **Settings**, **Export backup** saves a small file with your settings, reading progress, bookmarks and online library links (API keys only if you tick the box). **Import backup…** merges a backup into this browser: the most recent progress wins, bookmarks are combined, and your settings are replaced by the backup's. Use it to move to a new device or to keep a safety copy, since clearing your browser data erases everything Tenjin stores locally.

### Settings
Open **Settings** with the gear button or `s`:
- **Gap between pages in Scroll mode**, from 0 to 60 px (default 10).
- **Wide pages in Scroll mode:** split into two pages, or keep whole.
- **Page previews:** bottom, top, left or right.
- **Enlarge previews when you hover over them**, on or off.
- **Soft shadow** around and between pages, on or off.
- **Lock zoomed pages to the center**, **Swipe to turn pages** and **Double-tap to zoom**, each on or off.
- **Page number** in the bottom-right corner, on or off.
- **First page alone** in two-page mode, on or off.
- **Reset all settings** to the defaults.

Your view choices are **remembered automatically** and applied every time you open a book: reading mode, fit mode, zoom level, two-page mode, Manga direction, theme, previews and everything in Settings.

## Controls

| Action | Key |
| --- | --- |
| Previous / next page | `←` `→` (flipped in Manga mode), or click the left / right edge of the page |
| Scroll down / up, turning the page at the edge | `Space` / `Shift+Space`, `PageDown` / `PageUp` |
| Scroll a little | `↑` `↓` |
| First / last page | `Home` / `End` |
| Zoom in / out / reset | `+` / `-` / `0` (10% steps) |
| Fit width / fit page / actual size | `1` / `2` / `3` |
| Center lock | `c` |
| Bookmark this page / show bookmarks | `b` / `Shift+B` |
| Two-page spreads | `d` |
| Page / Scroll mode | `v` |
| Page previews | `t` |
| Manga (right-to-left) mode | `r` |
| Hide or show toolbars | `h`, or click the middle of the page |
| Jump to a page number | `g` |
| Library | `l` (`Esc` returns to the book you were reading) |
| Settings | `s` |
| Fullscreen | `f` |
| Open a file | `Ctrl/Cmd + O` |
| Help | `?` |

## Where your data is stored

Everything is stored in your browser on your device.

- **Settings, reading progress, bookmarks, your online links and any Google API key** are kept in the browser's local storage. Progress is matched by file name and size, so renaming or editing a file resets its progress.
- **Covers, the remembered folder and downloaded books** are kept in the browser's IndexedDB. Downloads count against your browser's storage quota.
- Data is **per browser and per device** and does not sync. Clearing your browser's site data removes it.
- Because browser storage is shared across everything on `thomasjngo.github.io`, settings and progress from the old `cbz_reader` address carry over to Tenjin automatically.
- The only network requests Tenjin makes are the ones you ask for: your online library (if you connect one), the PDF engine (when you open a PDF) and the Inter font.

## Browser support

- Works in current versions of Chrome, Edge, Firefox and Safari.
- Remembering your library folder and **Save to folder…** use the File System Access API, available in **Chrome and Edge**. In **Firefox and Safari** you pick the folder each visit; progress is still saved.

## Run it

Keep these files together in the same folder: `index.html` and `jszip.min.js`. To add CBR and CB7 support, also add the `libarchive/` folder (three files: `libarchive.js`, `worker-bundle.js` and `libarchive.wasm`).

**In a browser:** serve the folder (`npx serve .`) and open it. Opening `index.html` straight from disk works for CBZ, but CBR/CB7 and online libraries need a web server.

**On GitHub Pages:**
1. Name the repository `tenjin`, push it to GitHub and make it public.
2. Go to **Settings → Pages** and set **Source** to **GitHub Actions**.
3. The workflow in `.github/workflows/pages.yml` publishes the site on every push to `main`.

**As a desktop app (optional):** the repo can include a small Electron wrapper (`main.js` and `package.json`). Set `"name": "tenjin"` and `"productName": "Tenjin"` in `package.json`, and make sure `main.js` loads the `index.html` next to it.

```bash
npm install
npm start
```

Use `npm run dist` to build an installer for your operating system.

## Limits worth knowing

- ZIP and CBZ files larger than about 150 MB are read with a streaming reader (zip.js, loaded on demand from a CDN), so Tenjin only reads the pages you open instead of loading the whole file. CBT files are read the same way. PDF, EPUB and RAR/7-Zip files still load the whole file into memory, so very large ones can be slow, and generating covers for a folder of them takes time. Covers are made one at a time.
- Online comics get a cover only after you download them, because a cover needs the file itself.
- PDFs are drawn page by page as images, so text is not selectable.
- RAR and 7-Zip support is an optional add-on built on libarchive.js. Password-protected archives are not supported.
- In Scroll mode, pages load as you approach them and unload when far away. Spreads are detected as they load, so the scrollbar can shift slightly while page sizes are still being measured.
- Your Google API key is stored in your browser and sent only to Google. Restricting it to your website, as described above, limits what anyone could do with it if it were ever exposed.
- The interface uses the system font on Apple devices (San Francisco) and Inter elsewhere. Inter is loaded from Google Fonts, so offline the app falls back to your system font.

## Credits and license

Uses [JSZip](https://stuk.github.io/jszip/) (MIT or GPLv3), [zip.js](https://gildas-lormeau.github.io/zip.js/) (BSD-3-Clause, loaded on demand for large files), [PDF.js](https://mozilla.github.io/pdf.js/) (Apache 2.0, loaded on demand) and, optionally, [libarchive.js](https://github.com/nika-begiashvili/libarchivejs) (MIT).

This project is released under the MIT License. See `LICENSE`.
