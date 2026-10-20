<p align="center">
  <img src="https://i.postimg.cc/SssbH2Dg/locked-static-page-logo.png" alt="Locked Static Page: encrypt, publish, access" width="260">
</p>

# locked-static-page

Put a static website behind an access code. The site is encrypted in your browser and published as
numbered files. Visitors enter the code, and the site is decrypted and opened in their browser.
Nothing is decrypted on the server.

## Lock a site

1. Open `index.html?setup`
2. Pick the site's folder. It needs an `index.html` at its top level.
3. Untick any files you don't want to include. Hidden files start unticked.
4. Press **Generate**, then **Copy**, and keep the code somewhere safe. It can't be recovered.
5. Set **Maximum file size** (20 MB by default; Cloudflare Pages serves files of up to 25 MB).
6. Press **Encrypt and save**. You get `site-000.enc`, `site-001.enc` and so on, each at most that size.

## Publish it

Put these files together on any static host:

```
index.html
sw.js
site-000.enc
site-001.enc
...
```

Don't publish the original folder.

## Open it

Visitors open `index.html`, enter the code and press **Unlock**. The page downloads the `.enc`
files, decrypts them, and shows the site.

## How it works

- `index.html` encrypts the site with a key made from the code, and splits it into numbered files (20 MB each by default). Unlocking fetches them in order until it
  has the whole site, so any file size works.
- After unlocking, the site runs in a frame at `__site__/`. `sw.js` answers requests there with the
  decrypted files, which stay in the page's memory and are gone when the tab closes.
- Without `sw.js`, the site still opens, but only references written in its `index.html` load.

## Good to know

- Anyone with the code can see and save every file in the site.
- Links that start with `/` (like `/about.html`) don't work. Use relative links (`about.html`).
- A new code means encrypting again and publishing the new `.enc` files.

## Local Decryption Test
To test the decryption locally, you can use the following options: 

#### Option 1: Python
```
python -m http.server 6060
```

#### Option 2: npx
```
npx serve -l 6060
```

Then go to localhost:6060 and paste your code to see the page.