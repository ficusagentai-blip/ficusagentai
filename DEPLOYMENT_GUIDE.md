# 🚀 FicusAgentAI — GitHub + Supabase Deployment Guide

## Complete Step-by-Step Setup

---

## PHASE 1 — SUPABASE SETUP

### Step 1: Supabase Account बनवा
1. जा: https://supabase.com
2. **"Start your project"** click करा
3. GitHub account ने signup करा (recommended)

### Step 2: New Project Create करा
1. Dashboard वर **"New Project"** click करा
2. Fill करा:
   - **Project Name:** `ficusagentai`
   - **Database Password:** एक strong password टाका (लिहून ठेवा!)
   - **Region:** `Southeast Asia (Singapore)` — India साठी closest
3. **"Create new project"** click करा
4. ⏳ 2-3 minutes wait करा (database setup होतो)

### Step 3: API Keys मिळवा
1. Left sidebar → **Settings** (⚙️)
2. **API** section click करा
3. Copy करा आणि कुठेतरी save करा:
   - **Project URL** → `https://xxxxx.supabase.co`
   - **anon public key** → `eyJhbGci...` (लांब string)

### Step 4: Database Tables Create करा
1. Left sidebar → **SQL Editor** click करा
2. **"New query"** click करा
3. खालील SQL paste करा आणि **Run** (▶) click करा:

```sql
-- STEP 1: Users Table
CREATE TABLE IF NOT EXISTS users (
  id TEXT PRIMARY KEY,
  name TEXT NOT NULL,
  role TEXT NOT NULL DEFAULT 'employee',
  title TEXT DEFAULT 'Team Member',
  username TEXT UNIQUE NOT NULL,
  password TEXT NOT NULL,
  bg TEXT DEFAULT 'linear-gradient(135deg,#52a855,#2d6a2f)',
  created_at TIMESTAMPTZ DEFAULT now()
);

-- STEP 2: Tasks Table
CREATE TABLE IF NOT EXISTS tasks (
  id TEXT PRIMARY KEY,
  name TEXT NOT NULL,
  description TEXT DEFAULT '',
  assign_to TEXT DEFAULT '',
  priority TEXT DEFAULT 'medium',
  status TEXT DEFAULT 'pending',
  deploy TEXT DEFAULT 'no',
  due_date TEXT DEFAULT '',
  project TEXT DEFAULT '',
  created_at TIMESTAMPTZ DEFAULT now()
);

-- STEP 3: Projects Table
CREATE TABLE IF NOT EXISTS projects (
  id TEXT PRIMARY KEY,
  name TEXT NOT NULL,
  description TEXT DEFAULT '',
  type TEXT DEFAULT 'internal',
  status TEXT DEFAULT 'active',
  progress INT DEFAULT 0,
  created_at TIMESTAMPTZ DEFAULT now()
);

-- STEP 4: Activity Table
CREATE TABLE IF NOT EXISTS activity (
  id SERIAL PRIMARY KEY,
  text TEXT NOT NULL,
  sub TEXT DEFAULT '',
  icon TEXT DEFAULT '📌',
  time TIMESTAMPTZ DEFAULT now()
);

-- STEP 5: Notifications Table
CREATE TABLE IF NOT EXISTS notifs (
  id SERIAL PRIMARY KEY,
  message TEXT NOT NULL,
  created_at TIMESTAMPTZ DEFAULT now()
);

-- STEP 6: Row Level Security Enable (important!)
ALTER TABLE users    ENABLE ROW LEVEL SECURITY;
ALTER TABLE tasks    ENABLE ROW LEVEL SECURITY;
ALTER TABLE projects ENABLE ROW LEVEL SECURITY;
ALTER TABLE activity ENABLE ROW LEVEL SECURITY;
ALTER TABLE notifs   ENABLE ROW LEVEL SECURITY;

-- STEP 7: Public Access Policies (anon key साठी)
CREATE POLICY "Allow all for anon" ON users    FOR ALL TO anon USING (true) WITH CHECK (true);
CREATE POLICY "Allow all for anon" ON tasks    FOR ALL TO anon USING (true) WITH CHECK (true);
CREATE POLICY "Allow all for anon" ON projects FOR ALL TO anon USING (true) WITH CHECK (true);
CREATE POLICY "Allow all for anon" ON activity FOR ALL TO anon USING (true) WITH CHECK (true);
CREATE POLICY "Allow all for anon" ON notifs   FOR ALL TO anon USING (true) WITH CHECK (true);

-- STEP 8: Default Data Insert करा
INSERT INTO users (id, name, role, title, username, password, bg) VALUES
  ('u1', 'Suraj', 'founder', 'Founder & CEO', 'suraj', 'founder123', 'linear-gradient(135deg,#f0c040,#d4a017)'),
  ('u2', 'Piyush', 'employee', 'Developer', 'piyush', 'piyush123', 'linear-gradient(135deg,#52a855,#2d6a2f)')
ON CONFLICT (id) DO NOTHING;

INSERT INTO projects (id, name, description, type, status, progress) VALUES
  ('p1', 'FicusAgentAI Platform', 'Internal team management hub', 'internal', 'active', 45),
  ('p2', 'AI Chatbot — Client X', 'Conversational AI for e-commerce', 'client', 'active', 70)
ON CONFLICT (id) DO NOTHING;
```

4. ✅ "Success" message दिसला की tables ready आहेत!

---

## PHASE 2 — GITHUB SETUP

### Step 5: GitHub Account + Repository
1. जा: https://github.com
2. Account नाही तर signup करा
3. **"New Repository"** click करा (+ icon → New repository)
4. Fill करा:
   - **Repository name:** `ficusagentai`
   - **Description:** `AI Team Workspace Manager`
   - **Public** select करा (GitHub Pages साठी free मध्ये)
   - ✅ **Add a README file** check करा
5. **"Create repository"** click करा

### Step 6: Files Upload करा
1. Repository page वर **"Add file"** → **"Upload files"** click करा
2. Upload करा:
   - `ficusagentai_supabase.html` (rename to `index.html` before upload!)
   - `README.md`
3. Commit message: `Initial release - FicusAgentAI v2`
4. **"Commit changes"** click करा

### Step 7: GitHub Pages Enable करा
1. Repository → **Settings** tab
2. Left sidebar → **Pages**
3. Source section मध्ये:
   - Branch: **main**
   - Folder: **/ (root)**
4. **Save** click करा
5. ⏳ 2-3 minutes wait करा
6. URL दिसेल: `https://yourusername.github.io/ficusagentai`

---

## PHASE 3 — VERIFY

### Step 8: Test करा
1. Supabase → **Table Editor** → tasks table उघडा
2. App मध्ये login करा आणि एक task बनवा
3. Supabase table editor refresh करा — task दिसला पाहिजे ✅
4. दुसऱ्या device/browser मध्ये open करा — same data दिसला पाहिजे ✅

---

## ⚠️ Important Notes

- **Password Security:** हे demo version आहे, plain text passwords वापरतो. Production साठी Supabase Auth वापरा.
- **API Key:** anon key public होतो — ते OK आहे. Service key कधीही frontend मध्ये टाकू नका!
- **Data:** Supabase free tier मध्ये 500MB storage आणि 50,000 requests/month मिळतात.

---

*FicusAgentAI — Built with ❤️*
