
# Korah COA Hosting

Public home for batch test reports (COAs). Each PDF gets a permanent address:

    https://coa.korahpeptides.com/KP-0001.pdf

A browsable list lives at https://coa.korahpeptides.com/

## What's in this folder

| Item | What it is |
|---|---|
| `public/` | Everything that goes on the internet. COA PDFs go in here. |
| `public/index.html` | The searchable list page. You never need to edit it. |
| `public/batches.json` | The list of batches. You edit this each time you add a COA. |
| `tools/qr.html` | QR code maker for labels. Double-click to open. Not published. |
| `tools/qrcode.js` | Helper for the QR maker. Leave it alone. |
| `wrangler.jsonc` | Tells Cloudflare to publish the `public` folder. Leave it alone. |

---

## One-time setup

Menus on GitHub and Cloudflare change. If a label below doesn't match your screen, look for the closest one.

### Part 1: Put the files on GitHub

1. On github.com, create a **new repository** named `korah-coa` under your korahpeptides account (same place as `korah-peptides-site`). Leave it empty.
2. On the empty repo page, click **uploading an existing file**.
3. Drag in the **contents** of this folder: the `public` folder, the `tools` folder, `wrangler.jsonc`, and `README.md`. Drag the folders themselves, not just the files inside them.
4. Click **Commit changes**.

### Part 2: Publish it on Cloudflare

1. In Cloudflare, go to **Workers & Pages** and click **Create** (or Create application).
2. Choose to import from **GitHub** and pick the `korah-coa` repository.
3. Accept the defaults. The name is `korah-coa`. Leave the build command empty. The deploy command stays at its default (`npx wrangler deploy`).
4. Click **Deploy**.

The `wrangler.jsonc` file already tells Cloudflare to attach `coa.korahpeptides.com` as the project's address, so there is no separate domain step. It also switches off the workers.dev address and preview links, so the only way to reach the files is the korahpeptides.com address. That is why the Wrangler warnings go away.

### Part 3: Check it

1. Wait a few minutes after the deploy finishes (DNS and the security certificate need time).
2. Open https://coa.korahpeptides.com/ and confirm you see the Certificates of Analysis page.

If the deploy log shows a domain or DNS error, open the project's **Settings**, then **Domains & Routes**, and check that `coa.korahpeptides.com` is listed. If it isn't, click **Add**, choose **Custom domain**, and enter it.

**Do not print any label until step 2 works** and a test COA PDF opens from that address on your phone.

---

## Adding a new COA (do this every time)

1. **Check the PDF for private information.** Anything on it is public to anyone with the link. Remove customer names, order numbers, or personal details before uploading.
2. Name the file by its batch number, exactly: `KP-0012.pdf`. Capital letters, a dash, four digits. **Never rename it after labels are printed.**
3. On GitHub, open the `korah-coa` repo, open the `public` folder, click **Add file**, then **Upload files**, and drop the PDF in. Commit.
4. Still on GitHub, open `public/batches.json` and click the pencil icon (Edit). Copy one of the existing blocks and change the values. Blocks go between the square brackets, separated by commas, like this:

```
  {
    "batch": "KP-0012",
    "peptide": "BPC-157",
    "test_date": "2026-10-15",
    "lab": "Name of testing lab",
    "file": "KP-0012.pdf"
  },
```

   The last block in the list has **no comma** after its closing brace. Every block before it does. A missing or extra comma will make the list page show an error.
5. Commit. Cloudflare republishes in about a minute. Open `https://coa.korahpeptides.com/KP-0012.pdf` to confirm.

The first row in `batches.json` (`KP-0001`, "Example Peptide") is a placeholder. Replace it with a real batch or delete the whole block.

---

## Making QR codes for labels

1. Double-click `tools/qr.html`. It opens in your browser. It works with no internet.
2. Leave the base address as `https://coa.korahpeptides.com/` (it must match what you set up above).
3. Type your batch numbers, one per line, or a range such as `KP-0001 to KP-0025`.
4. Click **Make QR codes**. Download **SVG** for print (sharpest at any size) or **PNG** for quick use. **Download all (PNG)** saves the whole set.
5. **Test every code with a phone before printing**: scan it and confirm the right PDF opens.

Print tips: keep the code at least 2 cm (about 3/4 inch) square, black on white, with the white border left in place. The codes use the highest error-correction level, so small scuffs are survivable, but low contrast or tiny sizes are not.

---

## Linking from the tracker app

Link each batch to `https://coa.korahpeptides.com/<batch>.pdf`. It is a plain link and opens in the browser.
