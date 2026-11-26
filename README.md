# Lead Tracker

A lightweight Chrome extension for saving leads — URLs and notes — straight from your browser. Grab the page you're on with one click, or type in a lead manually. Everything is stored locally, so your list is still there the next time you open the popup.

---

## Features

- **Save Tab** – Saves the URL of your current active tab with one click
- **Save Input** – Type any URL or note into the field and save it (empty entries are ignored)
- **Persistent storage** – Leads are kept in `localStorage`, so they survive closing the popup and restarting the browser
- **Clickable links** – Every saved lead renders as a link that opens in a new tab
- **Delete All** – Double-click to clear your entire list (double-click prevents accidental wipes)

---

## Installation

The extension isn't on the Chrome Web Store yet, so you load it manually in Developer Mode.

1. **Clone the repo**
   ```bash
   git clone https://github.com/Akai-Outlaw/Lead-Tracker.git
   ```
2. Open Chrome and go to `chrome://extensions`
3. Turn on **Developer mode** (top-right toggle)
4. Click **Load unpacked** and select the `Lead-Tracker` folder
5. Pin the extension from the puzzle-piece icon in the toolbar for quick access

---

## Usage

| Action | How |
|---|---|
| Save the page you're on | Click **SAVE TAB** |
| Save a custom lead | Type in the input box, then click **SAVE INPUT** |
| Open a saved lead | Click the link in the list |
| Clear everything | **Double-click** **DELETE ALL** |

---

## Project Structure

```
Lead-Tracker/
├── manifest.json   # Extension config (Manifest V3, popup, permissions)
├── index.html      # Popup layout: input, buttons, leads list
├── index.css       # Popup styling (green theme)
├── index.js        # App logic: saving, rendering, storage, deleting
└── icon.png        # Toolbar icon
```

---

## How It Works

1. **On load**, `index.js` reads `myLeads` from `localStorage`. If leads exist, they're parsed from JSON and rendered.
2. **Save Tab** calls `chrome.tabs.query({ active: true, currentWindow: true })` to get the current tab's URL, adds it to the `myLeads` array, writes the array back to `localStorage`, and re-renders.
3. **Save Input** pushes the input field's value into the array, clears the field, saves, and re-renders.
4. **`render()`** builds the `<li><a>` list with template literals and sets it in one `innerHTML` update (one DOM write instead of one per item).
5. **Delete All** listens for `dblclick`, clears `localStorage`, resets the array, and re-renders an empty list.

---

## Tech Stack

- **JavaScript (ES6)** – DOM manipulation, event listeners, template literals, arrays
- **HTML5 / CSS3** – Popup UI and styling
- **Chrome Extensions API (Manifest V3)** – `chrome.tabs` for reading the active tab
- **Web Storage API** – `localStorage` with `JSON.stringify` / `JSON.parse`

### Permissions

| Permission | Why |
|---|---|
| `tabs` | Reads the URL of the active tab for the **SAVE TAB** button |

---

## What I Learned

- How a Chrome extension is structured and configured with `manifest.json`
- Reading browser state with the `chrome.tabs` API
- Storing data between sessions with `localStorage` and JSON serialization
- Rendering lists to the DOM efficiently with template strings
- Using different events (`click` vs. `dblclick`) to make destructive actions safer

---

## Roadmap

- [ ] Delete individual leads instead of only clearing all
- [ ] Switch to `chrome.storage` so leads can sync across devices
- [x] Validate input and ignore empty entries
- [ ] Prevent duplicate leads
- [ ] Export leads to CSV
- [ ] Add a note or tag to each lead
- [ ] Publish to the Chrome Web Store

---

## Author

**Akai Outlaw**
GitHub: [@Akai-Outlaw](https://github.com/Akai-Outlaw)
