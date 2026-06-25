# 🚀 Sake Clips Store — Setup Guide

Follow these steps in order. Takes about 10 minutes.

---

## STEP 1 — Create Supabase Project (Free Database)

1. Go to **https://supabase.com** → click **Start your project**
2. Sign up with GitHub or Google (free)
3. Click **New Project**
4. Fill in:
   - **Name:** sake-clips-store
   - **Database Password:** choose a strong password (save it)
   - **Region:** pick the closest to India (e.g. Southeast Asia)
5. Click **Create new project** → wait ~2 minutes

---

## STEP 2 — Create the Products Table

1. In your Supabase project, click **SQL Editor** (left sidebar)
2. Click **New Query**
3. Paste this SQL and click **Run**:

```sql
-- Create products table
create table products (
  id          uuid default gen_random_uuid() primary key,
  name        text not null,
  price       text,
  image       text,
  link        text not null,
  badge       text default '',
  created_at  timestamptz default now()
);

-- Allow anyone to READ products (visitors can see them)
alter table products enable row level security;

create policy "Anyone can view products"
  on products for select
  using (true);

create policy "Service role can insert"
  on products for insert
  with check (true);

create policy "Service role can delete"
  on products for delete
  using (true);
```

4. You should see **"Success. No rows returned"** — the table is created ✅

---

## STEP 3 — Get Your Supabase Keys

1. In Supabase, click **Settings** (gear icon) → **API**
2. Copy these two values:
   - **Project URL** → looks like `https://abcdefgh.supabase.co`
   - **anon public** key → long string starting with `eyJ...`

---

## STEP 4 — Edit config.js

Open `config.js` in the project folder and fill in your values:

```js
const SUPABASE_URL      = "https://YOUR_PROJECT.supabase.co";
const SUPABASE_ANON_KEY = "eyJhbGciOiJIUzI1NiIsInR5cCI6...";
const ADMIN_PASSWORD    = "MySecretPassword123";  // only you know this!
```

> ⚠️ Keep `config.js` private — don't share it publicly.
> The anon key is safe to expose (it's read-only by default).

---

## STEP 5 — Deploy to Vercel (Free Hosting)

### Option A — Deploy via Vercel Dashboard (easiest)

1. Go to **https://vercel.com** → Sign up free
2. Click **Add New Project**
3. Choose **Upload** (drag and drop your folder)
4. Drag the entire `sake-clips-store` folder into Vercel
5. Click **Deploy** → done!

### Option B — Deploy via GitHub (recommended for updates)

1. Create a free account at **https://github.com**
2. Create a new repository called `sake-clips-store`
3. Upload your files to GitHub
4. Go to **https://vercel.com** → New Project → Import from GitHub
5. Select your repository → click **Deploy**

Every time you update files and push to GitHub, Vercel auto-redeploys ✅

---

## STEP 6 — Test Your Store

1. Open your Vercel URL (e.g. `https://sake-clips-store.vercel.app`)
2. Click **Admin** in the top right
3. Enter your password from `config.js`
4. Click **Add Product** → paste an Amazon affiliate link
5. Fill in the product name and price → click **Publish to Store**
6. Open the same URL in an **incognito window** — you'll see the product!

---

## How It Works

| Who          | What they can do                     |
|-------------|--------------------------------------|
| **You (Admin)** | Login → Add products → Delete products |
| **Visitors**    | View products → Click to Amazon link  |

- Products are stored in **Supabase** (PostgreSQL database)
- When you add a product, it appears **instantly** for all visitors
- **Real-time** updates — no page refresh needed
- Your store URL works globally on Vercel

---

## How to Add a Product

1. Login as Admin
2. Click **Add Product**
3. Paste Amazon affiliate link (e.g. `https://amzn.to/xxxxx` or `https://www.amazon.in/dp/B09XXXXX/`)
4. Wait for ASIN detection
5. Type the **product name** (copy from Amazon)
6. Add **price** (e.g. ₹1,299)
7. Choose a **badge** (optional: New, Popular, Top Rated, Sale)
8. Click **Publish to Store** ✅

---

## Troubleshooting

**"Supabase not configured"** → Fill in `config.js` with your actual keys

**Products not showing** → Check the SQL was run correctly in Supabase

**Wrong password** → Check the `ADMIN_PASSWORD` in `config.js`

**Images not loading** → Amazon image URLs may expire; paste a custom image URL

---

## File Structure

```
sake-clips-store/
├── index.html    ← Main website (don't edit)
├── config.js     ← Your Supabase keys + admin password (EDIT THIS)
└── SETUP.md      ← This guide
```

---

Built with ❤️ for Sake Clips Store
