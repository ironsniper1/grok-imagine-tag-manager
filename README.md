# Grok Imagine Tag Manager

A [Tampermonkey](https://www.tampermonkey.net/) userscript that lets you organize your saved **Grok Imagine** images using Grok's own tags (folders) — from a fast, searchable grid with both **per-image** and **bulk** "add to tag" actions. All tagging is written directly to Grok through its native API, so your tags show up everywhere Grok shows them.

---

## Features

- **Searchable grid** of your saved/liked images with prompts and thumbnails.
- **Tag sidebar** listing your real Grok tags (folders) with live image counts. Click a tag to filter the grid to it.
- **Per-image tagging** — a 🏷 button on every image opens a popover showing which tags it's already in, and lets you add it to any other tag (or create a new one on the spot).
- **Bulk tagging** — select multiple images and add them all to a tag in one action.
- **Create tags** straight from the panel.
- **Untagged view** to find images not in any tag yet.

Because tags are Grok folders, anything you tag here appears under the matching tag chip on Grok's own Saved page, and vice-versa.

---

## Requirements

- A desktop browser with Tampermonkey (Chrome, Firefox, Edge, or Safari).
- A Grok account, signed in, with saved/liked images on [grok.com/imagine](https://grok.com/imagine).
- **The companion "Grok Imagine Favorites Search" script**, run at least once so its image index exists. This Tag Manager reads that local index (read-only) to populate the searchable grid. Your tags come live from Grok regardless, but the grid will be empty until the index is built.

---

## Installation

### Step 1 — Install Tampermonkey

First, install the Tampermonkey browser extension if you haven't already:

- **Chrome** → [Install from Chrome Web Store](https://chrome.google.com/webstore/detail/tampermonkey/dhdgffkkebhmkfjojejmpbldmpobfkfo)
- **Firefox** → [Install from Firefox Add-ons](https://addons.mozilla.org/firefox/addon/tampermonkey/)
- **Edge** → [Install from Edge Add-ons](https://microsoftedge.microsoft.com/addons/detail/tampermonkey/iikmkjmpaadaobahmlepeloendndfphd)
- **Safari** → [Install from App Store](https://www.tampermonkey.net/?browser=safari)

### Step 2 — Configure Tampermonkey (do this *before* installing the script)

After installing Tampermonkey, you need to enable two settings or the script will not work.

1. Go to `chrome://extensions` in your address bar.
2. Find **Tampermonkey** and click **Details**.
3. Scroll down and turn on **"Allow User Scripts"**.

This allows Tampermonkey to run scripts that have not been reviewed by Google. You should only enable this if you trust the script you are installing.

4. On the same page, also turn on **"Allow in Incognito"** if you want the script to work in private/incognito windows.

Without this the script will only run in normal browser windows.

Once both settings are enabled, proceed to installation below.

### Step 3 — Install the script

Click the raw script link to have Tampermonkey prompt you to install:

**[Install Grok Imagine Tag Manager](https://raw.githubusercontent.com/ironsniper1/grok-imagine-tag-manager/main/grok-imagine-tag-manager.user.js)**

Tampermonkey will open an install tab showing the script source — click **Install**. To update later, reopen the same link.

If the link just shows text instead of an install prompt, double-check that **"Allow User Scripts"** from Step 2 is enabled.

---

## Usage

1. Go to [grok.com/imagine](https://grok.com/imagine) (or the Saved page) while signed in.
2. Click the **🏷 Tags** button in the bottom-right corner to open the panel.
3. Use it:
   - **Search** prompts with the box at the top.
   - **Filter by tag** by clicking a tag in the left sidebar. "All images" and "Untagged" are at the top.
   - **Tag one image** — click the 🏷 button on a card. The popover shows its current tags (✓) and lets you add it to any other tag, or type a name + Enter to create-and-add.
   - **Tag many images** — click **Select**, click the images you want (or **Select page**), pick a tag in the bulk bar, and hit **Add to tag**.
   - **Create a tag** — **+ New tag**.
   - **Reload tags from Grok** — the **↻** button refreshes the tag list and counts.
4. Clicking an image's thumbnail (when not in Select mode) opens its Grok saved page in a new tab.

---

## How it works

Grok's "tags" are folders. The script talks to the same REST endpoints Grok's own UI uses:

| Action | Endpoint | Payload |
| --- | --- | --- |
| List tags | `POST /rest/media/folder/list` | `{}` |
| List a tag's images | `POST /rest/media/post/list` | `{ filter: { source: "MEDIA_POST_SOURCE_LIKED", folderId } }` |
| Create a tag | `POST /rest/media/folder/create` | `{ name }` |
| Which tags an image is in | `POST /rest/media/post/folders` | `{ postId }` |
| Add an image to a tag | `POST /rest/media/post/like` | `{ id, folderId }` |

The searchable grid is populated from the local IndexedDB index built by the Favorites Search script; the tag layer is fetched live from Grok.

---

## Limitations

- **Add-only.** There is currently no remove-from-tag, to avoid risking an un-save of an image. Removal may be added once that endpoint is confirmed.
- **Grid depends on the index.** A tag's filter and counts only display images that also exist in your local Favorites Search index. If the index is behind, a freshly-saved image may not appear in the grid until you re-run the indexer — but the tag on Grok's side is correct regardless.
- Desktop browsers only; requires being signed in to Grok.

---

## Disclaimer

This is an unofficial, third-party userscript. It is not affiliated with, endorsed by, or supported by xAI or Grok. It uses Grok's own web API the same way the site does, on your own signed-in account. Use at your own discretion.

---

## License

MIT
