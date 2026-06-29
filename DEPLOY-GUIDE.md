# Deploying stribor.us to GitHub Pages — Step-by-Step

## What You'll End Up With

- Your site hosted **free** on GitHub Pages
- `stribor.us` pointing to it via GoDaddy DNS changes
- No WordPress, no hosting fees — just a Git repository

---

## Step 1 — Download Your Images

Before anything else, save these 5 images from your current WordPress site and put them in an `images/` folder next to your HTML files:

| Save as | Source URL |
|---|---|
| `images/backMain.png` | https://stribor.us/wp-content/uploads/2024/06/backMain.png |
| `images/But1.png` | https://stribor.us/wp-content/uploads/2024/06/But1.png |
| `images/But2.png` | https://stribor.us/wp-content/uploads/2024/06/But2.png |
| `images/But4.png` | https://stribor.us/wp-content/uploads/2024/06/But4.png |
| `images/favicon.png` | https://stribor.us/wp-content/uploads/2024/06/cropped-stribor6-1.png |

Download each one by opening the URL in your browser and doing File → Save Image As.

---

## Step 2 — Fix the Email Address

Your WordPress site uses Cloudflare's email obfuscation, so I couldn't read your actual email. All 4 HTML files currently have a placeholder:

```
info@stribor.us
```

Search for `info@stribor.us` in each file and replace it with your real email address. It appears in both the `<a href="mailto:...">` and the visible link text.

---

## Step 3 — Create a GitHub Repository

1. Go to [github.com](https://github.com) and sign in (or create a free account)
2. Click **New repository** (the `+` icon, top right)
3. Name it exactly: **`stribor.us`**
4. Set it to **Public**
5. Leave everything else unchecked — click **Create repository**

---

## Step 4 — Upload Your Files

In your new repository, click **uploading an existing file** (the link in the quick setup box).

Drag and drop all of these:
```
index.html
integration.html
branding.html
consulting.html
images/
  backMain.png
  But1.png
  But2.png
  But4.png
  favicon.png
```

Click **Commit changes**.

> **Alternative (if you're comfortable with Git CLI):**
> ```bash
> git init
> git add .
> git commit -m "Initial static site"
> git remote add origin https://github.com/YOUR-USERNAME/stribor.us.git
> git push -u origin main
> ```

---

## Step 5 — Enable GitHub Pages

1. In your repository, go to **Settings** → **Pages** (left sidebar)
2. Under **Source**, select **Deploy from a branch**
3. Choose branch: **main**, folder: **/ (root)**
4. Click **Save**

GitHub will show you a URL like `https://your-username.github.io/stribor.us/` — the site is now live there. Test it before changing DNS.

---

## Step 6 — Add a Custom Domain in GitHub

Still in **Settings → Pages**:

1. Under **Custom domain**, type: `stribor.us`
2. Click **Save**

This creates a file called `CNAME` in your repository automatically. Leave it there.

---

## Step 7 — Update GoDaddy DNS

This is the key step. Log in to GoDaddy → **My Products** → find `stribor.us` → click **DNS**.

### Delete existing A records

Find any existing **A records** pointing to your old hosting IP and delete them.

### Add 4 new A records (GitHub Pages IPs)

Add these one at a time — all with Host `@` and TTL `1 hour` (or `3600`):

| Type | Host | Points to | TTL |
|---|---|---|---|
| A | @ | 185.199.108.153 | 1 hour |
| A | @ | 185.199.109.153 | 1 hour |
| A | @ | 185.199.110.153 | 1 hour |
| A | @ | 185.199.111.153 | 1 hour |

> **Note:** These are GitHub's current Pages IP addresses as of mid-2025. Verify them at https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site if you're doing this well after that date.

### Add a CNAME for www (optional but recommended)

| Type | Host | Points to | TTL |
|---|---|---|---|
| CNAME | www | your-username.github.io | 1 hour |

Replace `your-username` with your actual GitHub username.

---

## Step 8 — Enable HTTPS

Back in **GitHub → Settings → Pages**, after DNS propagates (can take a few minutes to a few hours), you'll see a checkbox:

☑ **Enforce HTTPS**

Check it. GitHub handles the SSL certificate automatically for free.

---

## Propagation Time

DNS changes can take anywhere from a few minutes to 48 hours to fully propagate worldwide. Your site may show as unavailable or still show WordPress during that window — this is normal.

---

## After Your Site Is Working — Cancel WordPress Hosting

Only cancel WordPress once you've confirmed `stribor.us` is loading your new static site correctly. Keep your GoDaddy domain registration — you still need that. You're only cancelling the *hosting*, not the domain.

---

## File Structure Reference

```
stribor.us/              ← GitHub repository root
├── index.html           ← Home page
├── integration.html     ← Integration service page
├── branding.html        ← Branding service page
├── consulting.html      ← Consulting & Training page
├── CNAME                ← Created automatically by GitHub (don't edit)
└── images/
    ├── backMain.png
    ├── But1.png
    ├── But2.png
    ├── But4.png
    └── favicon.png
```

---

## Making Future Changes

To update content: edit the HTML file directly on GitHub (click the file → pencil icon → edit → Commit changes). Your site updates within ~30 seconds automatically.
