# ZMail

![Offline Only](https://img.shields.io/badge/Privacy-100%25%20Offline-success?style=flat-square&color=2e7d32)
![No Server Needed](https://img.shields.io/badge/Server-None%20Required-blue?style=flat-square&color=0277bd)
![Supports EML & MSG](https://img.shields.io/badge/Supports-EML%20%26%20MSG-orange?style=flat-square&color=e65100)
![GitHub Pages Ready](https://img.shields.io/badge/Deploy-GitHub%20Pages-blueviolet?style=flat-square&color=6a1b9a)
![License](https://img.shields.io/badge/License-MIT-gray?style=flat-square)

We've all been there: you need to inspect a raw email file to verify a sender's IP, check if SPF/DKIM aligned, or grab an attachment. But uploading sensitive company emails to random third-party online parsers is a security nightmare.

**ZMail** is a single-page diagnostics utility built specifically to solve this. It parses standard RFC822 `.eml` files and Outlook binary `.msg` files entirely in your browser.

**No data leaves your machine fully offline.** No backend servers, no analytics, no external APIs.

---

## Why Use ZMail?

- **Zero Server Setup**: It is a single, self-contained `index.html` file. Double-click it to run it locally on the `file://` protocol, or throw it onto any static hosting like GitHub Pages.
- **Support for Outlook `.msg` & `.eml`**: Decodes standard EML text files and binary Outlook MSG files directly in browser memory.
- **Deep Diagnostics**: Includes security header auditing, URL inspection, and server hop analysis.
- **Obsidian-Dark Aesthetics**: A visually rich, glassmorphic layout tailored for security engineers and system administrators who live in dark mode.

---

## Core Features

### 1. Security Header & Authentication Audit
ZMail automatically parses the raw email headers to look for SPF, DKIM, and DMARC results. It verifies standard authentication blocks and displays clear trust seals so you can identify spoofing attempts in seconds.

### 2. Hop-by-Hop Server Timeline
It extracts the `Received` header chain, reverses it to chronological order, and maps out the exact server route the email took. The analyzer calculates the propagation delay between each relay hop to pinpoint where a message was held up.

### 3. Redaction Mode (Anonymizer)
Need to share a screenshot or code snippet of a parsed email without leaking sensitive data? Toggle the anonymization switch. It dynamically masks:
- Subject lines
- Sender and recipient names
- Email addresses and domains
- Plain text and HTML body content

### 4. Link & URL Inspections
Detects every hyperlink in the email body. It flags:
- Mismatched links (where the anchor text says one domain but the `href` points to another)
- Shortened URLs (e.g., bit.ly, tinyurl)
- Tracking parameters

### 5. Attachment Viewer & Exporter
View file metadata and extract attachments instantly. You can inspect inline previews of text files (with search capabilities) or bundle all attachments into a single, clean `.zip` archive.

---

## How it Works Under the Hood

The app is built entirely using HTML5, modern vanilla CSS variables, and modular ES6 JavaScript. Since standard CORS rules block local JS modules when run over `file://` on some browsers, ZMail imports trusted open-source parsers directly from absolute HTTPS CDNs:

1. **EML Parser**: Powered by `postal-mime` loaded as a browser-native ESM module.
2. **MSG Parser**: Powered by `@kenjiuno/msgreader` loaded via a UMD bundle, which handles binary decoding of Microsoft OLE compound files.
3. **Zip Utility**: Powered by `JSZip` to handle client-side attachment archiving in browser memory.
4. **Icons**: Powered by `lucide-icons` for lightweight vector assets.

---

## Getting Started

### Run Locally (Offline)
1. Download [index.html](index.html) to your machine.
2. Double-click `index.html` to open it in Chrome, Firefox, Edge, or Safari.
3. Drag and drop any `.eml` or `.msg` file into the dashboard.

### Deploy to GitHub Pages (Static Hosting)
Because ZMail has zero build steps or server-side dependencies, hosting it on GitHub Pages takes three clicks:
1. Push this folder to your GitHub repository.
2. Go to repository **Settings** -> **Pages**.
3. Under **Build and deployment**, select your main branch as the source and click **Save**.

Your secure diagnostics bench is now live at `https://<username>.github.io/<repository>/`.

---

## License

This project is licensed under the MIT License - feel free to use, modify, and distribute it as you see fit. No telemetry, no catch.
