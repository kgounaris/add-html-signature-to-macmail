# Add an HTML Signature to Apple Mail on macOS

This guide shows you how to replace an Apple Mail signature's contents with your own **HTML** by editing Mail's signature file on disk.

> Apple Mail stores signatures as files under your user Library. When you edit the right file, Mail will load your updated HTML into the signature you created.

---

## 1) Create a new signature in Apple Mail

1. Open **Mail**.
2. Go to **Mail → Settings (or Preferences) → Signatures**.
3. Click **+** to create a new signature.
4. Type something obvious so you can recognize it later (example: `THIS IS MY HTML SIGNATURE`).
5. Close Settings.

This "placeholder" text is how you'll identify the correct signature file.

---

## 2) Give Terminal Full Disk Access (required)

Without this, Terminal may not be able to access the Mail signature folders.

1. Open **System Settings**.
2. Go to **Privacy & Security → Full Disk Access**.
3. Enable **Terminal**.
   - If Terminal isn't listed, click **+** and add it from:
     - **Applications → Utilities → Terminal**
4. Quit and reopen **Terminal** (important so the permission takes effect).

---

## 3) Open Terminal and navigate to the Signatures folder

Open **Terminal**, then run:

```bash
cd ~/Library/Mail/
ls
```

You'll see a folder like V10, V11, etc. That V# is your Mail storage version.

Now go into the signatures folder (replace V# with your version folder):

```bash
cd ~/Library/Mail/V*/MailData/Signatures
ls
```

> **Tip:** Using `V*` helps you avoid guessing the exact version number.

---

## 4) Find the signature file you just created

List the files:

```bash
ls -la
```

Signature files commonly look like random IDs and may end with `.mailsignature`.

To inspect a file's contents and find your placeholder text, use `cat`:

```bash
cat <signaturefile>
```

Repeat with different files until you find the one containing your unique placeholder text (e.g., `THIS IS MY HTML SIGNATURE`).

---

## 5) Unlock the signature file (remove immutable flag)

Apple Mail often marks signature files as "locked" (immutable). Remove that flag:

```bash
chflags nouchg <signaturefile>
```

---

## 6) Edit the signature file in TextEdit

Open the file in TextEdit:

```bash
open -a TextEdit <signaturefile>
```

Inside the file, locate the HTML body (it will look like an HTML document). Replace the body content with your HTML signature.

**Notes:**
- Keep the file format as plain text (TextEdit usually preserves it, but avoid saving as rich text).
- Paste valid HTML. Inline CSS is recommended for email signatures.

Save the file.

---

## 7) Lock the file again (recommended)

Re-apply the immutable flag so Apple Mail doesn't overwrite your HTML:

```bash
chflags uchg <signaturefile>
```

---

## 8) Restart Apple Mail to load the updated signature

1. Quit Mail completely.
2. Reopen Mail.

Your edited HTML should now appear in the signature you created.

---

## Troubleshooting

### Terminal still can't access the folder

- Re-check **System Settings → Privacy & Security → Full Disk Access → Terminal = ON**
- Quit and reopen Terminal after enabling Full Disk Access.

### My signature reverted back

- Ensure you re-locked the file:
  ```bash
  chflags uchg <signaturefile>
  ```
- Make sure Mail was closed while you edited the file.

### I can't find the signature file

- Confirm you created a new signature and typed a unique placeholder.
- Search for the placeholder text inside the directory:
  ```bash
  grep -R "THIS IS MY HTML SIGNATURE" .
  ```

---

## Example minimal HTML signature

```html
<div style="font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Arial, sans-serif; font-size: 13px; color: #222;">
  <div style="font-weight: 600;">Your Name</div>
  <div>Title · Company</div>
  <div style="margin-top: 6px;">
    <a href="mailto:you@example.com" style="color: #0a66c2; text-decoration: none;">you@example.com</a>
    ·
    <a href="https://example.com" style="color: #0a66c2; text-decoration: none;">example.com</a>
  </div>
</div>
```
