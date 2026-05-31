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

### 5. Attachment Viewer & Password-Protected ZIP Exporter
View file metadata and extract attachments instantly. Inspect inline previews of text files (with search capabilities) or bundle all attachments into a **secure, password-protected `.zip` archive** (AES-256 encrypted, defaulting to the malware-analyst standard password `"infected"`). This protects your local system by preventing accidental double-clicks or automatic antivirus scans from deleting sensitive payloads.

> [!TIP]
> **Why `"infected"`?** The password `infected` is the standard convention for sharing malware or suspicious payloads among security teams. It prevents antivirus software on your host system from automatically scanning and deleting the attachments, and prevents you from accidentally executing an attachment by double-clicking it.

#### How to Extract the Secure Archive:
* **Windows**: The built-in Windows Explorer extraction utility may fail or throw errors on modern AES-256 encrypted ZIPs. We recommend using **7-Zip** or **WinRAR**. Right-click the archive, select *7-Zip -> Extract Here*, and enter `infected` when prompted.
* **macOS & Linux**: Use the native terminal `unzip` utility or `7z`:
  ```bash
  # Using the standard unzip command
  unzip -P infected your_attachments.zip
  
  # Using the 7-Zip CLI tool
  7z x -pinfected your_attachments.zip
  ```


---

## How it Works Under the Hood

The app is built entirely using HTML5, modern vanilla CSS variables, and modular ES6 JavaScript. Since standard CORS rules block local JS modules when run over `file://` on some browsers, ZMail imports trusted open-source parsers directly from absolute HTTPS CDNs:

1. **EML Parser**: Powered by `postal-mime` loaded as a browser-native ESM module.
2. **MSG Parser**: Powered by `@kenjiuno/msgreader` loaded via a UMD bundle, which handles binary decoding of Microsoft OLE compound files.
3. **Zip Utility**: Powered by `zip.js` to handle client-side attachment archiving, compression, and AES-256 password protection directly in browser memory without Web Workers (CORS-safe).
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

Your secure diagnostics bench is now live at [zrnge.github.io/zmail](https://zrnge.github.io/zmail).

---

## Author

Developed and maintained by **[zrnge](https://github.com/zrnge)**.

---

## License

This project is licensed under the MIT License - feel free to use, modify, and distribute it as you see fit. No telemetry, no catch.
