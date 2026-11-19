# Brave URL Submitter

A Manifest V3 browser extension for **Brave** (and other Chromium browsers) that bulk-submits URLs to the [Brave Search submit-URL form](https://search.brave.com/submit-url). Import a list of URLs, set a delay, and let it work through the queue for you.

## Features

- Import URLs from a **CSV**, **TXT**, **TSV** or **sitemap XML** file, or paste them directly
- Automatically picks the first URL in each row, skips headers and removes duplicates
- Fills the form and clicks Submit for each URL, then moves to the next
- Random delay between submissions (configurable min and max)
- **Pause**, **Resume** and **Stop** controls
- Pauses automatically if a **CAPTCHA** appears or the form cannot be found, then retries the same URL on Resume
- Live progress bar and log in the popup
- Progress is saved, so closing the popup does not stop the run
- **Download log** as CSV (url, status, note, time)

## Installation

The extension is not on the Chrome Web Store, so load it as an unpacked extension.

1. Download or clone this repository and unzip it if needed.
2. Open `brave://extensions` (or `chrome://extensions`).
3. Turn on **Developer mode**.
4. Click **Load unpacked** and select the extension folder (the one containing `manifest.json`).
5. Pin the extension to your toolbar.

## Usage

1. Click the extension icon.
2. Choose your file (`.csv`, `.txt`, `.tsv` or sitemap `.xml`), or paste URLs into the text box.
3. Set the **min** and **max** delay in seconds (default 8 to 15).
4. Click **Start**. A tab opens on the Brave submit page and the queue runs.
5. Use **Pause**, **Resume** or **Stop & reset** at any time.
6. When finished, click **Download log** to save the results.

### Input formats

One URL per line in a TXT file:

```
https://example.com/
https://example.com/about/
https://example.com/blog/post-1/
```

CSV (the first URL found in each row is used, header rows are ignored):

```
url
https://example.com/
https://example.com/about/
```

Sitemap XML: every `<loc>` entry is imported.

### Log statuses

| Status | Meaning |
|--------|---------|
| `ok` | A confirmation message appeared or the form was cleared after submit |
| `sent` | The form was submitted but no confirmation was detected |
| `error` | The page showed an error message (note column has details) |
| `captcha` | A CAPTCHA was detected and the run paused |

## Tips

- Keep delays at **8 seconds or more** and submit in small batches (for example 20 to 50 URLs per day). Brave may rate-limit or ignore large batches.
- If a CAPTCHA appears, solve it in the tab and click **Resume**. The same URL is retried.
- Submit your homepage and key pages first. Make sure `robots.txt` does not block Brave's crawler and your sitemap is declared there.

## Limitations

- The Brave submit page is rendered with JavaScript. The extension finds the URL field (a text or URL input) and a button containing "Submit" automatically. If Brave changes the page layout, the selectors in `content.js` may need an update.
- Submitting a URL does not guarantee it will be indexed.
- Use it responsibly and in line with Brave Search's terms. Do not use it to spam.

## Project structure

```
brave-url-submitter/
├── manifest.json     # Extension manifest (MV3)
├── background.js     # Service worker (tab identification)
├── content.js        # Fills and submits the form on the Brave page
├── popup.html        # Popup interface
├── popup.js          # File import, queue control, log
└── icons/            # Extension icons (16, 32, 48, 128)
```

## Permissions

- `storage`: saves the queue and progress locally
- `tabs`: opens and controls the work tab
- Host access to `https://search.brave.com/*` only

No data is sent anywhere except to the Brave submit form itself.

## Author

Created by **Ben Haissoun**, [Clix Agency](https://clix.ma).

- LinkedIn: [linkedin.com/in/benhaissoun](https://www.linkedin.com/in/benhaissoun/)
- Email: [benhaissoun@consultant.com](mailto:benhaissoun@consultant.com)

Questions, bug reports and suggestions are welcome. Open an issue or get in touch.

## Disclaimer

This project is not affiliated with or endorsed by Brave Software. "Brave" is a trademark of Brave Software, Inc.

## License

MIT
