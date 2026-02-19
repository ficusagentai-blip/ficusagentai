# 🌿 FicusAgentAI — Team Workspace

> AI Automation & App Building — Complete Team Management System

[![Supabase](https://img.shields.io/badge/Backend-Supabase-3ECF8E?logo=supabase)](https://supabase.com)
[![HTML](https://img.shields.io/badge/Frontend-HTML%2FJS-orange)](https://github.com)
[![GitHub Pages](https://img.shields.io/badge/Deploy-GitHub%20Pages-blue?logo=github)](https://pages.github.com)

---

## ✨ Features

| Feature | Description |
|---|---|
| 🎯 Task Management | Assign, track, prioritize tasks with deploy status |
| ⏱ Work Time Tracking | Employees go online/offline — sessions auto-saved |
| 📊 Productivity Reports | Full analysis with PDF download + signature |
| ↩ Carryforward Tasks | Overdue tasks highlighted and prioritized next day |
| 🔔 Notifications | Real-time task assignment notifications |
| 📁 Project Tracker | Client & internal projects with progress |
| 👥 Team Profiles | Professional member profiles with contact info |
| ☁️ Real-time Sync | Supabase real-time database subscriptions |
| 🔐 Role-based Access | Founder (admin) + Employee (team member) |
| 📱 Responsive | Works on desktop, tablet & mobile |

---

## 🚀 Quick Setup

### Step 1: Supabase Setup

1. Go to [supabase.com](https://supabase.com) → Create account
2. Click **"New Project"** → Choose a name, password, region
3. Wait for project to be ready (~2 min)
4. Go to **SQL Editor** → Run the SQL schema below

### Step 2: Run SQL Schema

Copy and run this in **Supabase → SQL Editor → New Query**:

```sql
-- =====================================================
-- FicusAgentAI — Supabase Database Schema
-- =====================================================

-- USERS TABLE
CREATE TABLE IF NOT EXISTS users (
  id TEXT PRIMARY KEY,
  name TEXT NOT NULL,
  role TEXT NOT NULL DEFAULT 'employee',  -- 'founder' or 'employee'
  title TEXT DEFAULT 'Team Member',
  username TEXT UNIQUE NOT NULL,
  password TEXT NOT NULL,
  phone TEXT DEFAULT '',
  email TEXT DEFAULT '',
  bg TEXT DEFAULT 'linear-gradient(135deg,#52a855,#2d6a2f)',
  created_at TIMESTAMPTZ DEFAULT NOW()
);

-- TASKS TABLE
CREATE TABLE IF NOT EXISTS tasks (
  id TEXT PRIMARY KEY,
  name TEXT NOT NULL,
  description TEXT DEFAULT '',
  assign_to TEXT NOT NULL,
  priority TEXT DEFAULT 'medium',   -- 'high', 'medium', 'low'
  status TEXT DEFAULT 'pending',    -- 'pending', 'inprogress', 'done', 'blocked'
  deploy TEXT DEFAULT 'no',         -- 'yes', 'no', 'na'
  due_date DATE,
  project TEXT DEFAULT '',
  created_at TIMESTAMPTZ DEFAULT NOW()
);

-- PROJECTS TABLE
CREATE TABLE IF NOT EXISTS projects (
  id TEXT PRIMARY KEY,
  name TEXT NOT NULL,
  description TEXT DEFAULT '',
  type TEXT DEFAULT 'internal',   -- 'client' or 'internal'
  status TEXT DEFAULT 'active',   -- 'active', 'planning', 'paused', 'done'
  progress INTEGER DEFAULT 0,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

-- ACTIVITY TABLE
CREATE TABLE IF NOT EXISTS activity (
  id SERIAL PRIMARY KEY,
  text TEXT NOT NULL,
  sub TEXT DEFAULT '',
  icon TEXT DEFAULT '📌',
  time TIMESTAMPTZ DEFAULT NOW()
);

-- NOTIFICATIONS TABLE
CREATE TABLE IF NOT EXISTS notifications (
  id TEXT PRIMARY KEY,
  message TEXT NOT NULL,
  for_user_id TEXT NOT NULL,
  type TEXT DEFAULT 'info',   -- 'task_assigned', 'info'
  read BOOLEAN DEFAULT FALSE,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

-- WORK SESSIONS TABLE
CREATE TABLE IF NOT EXISTS work_sessions (
  id TEXT PRIMARY KEY,
  user_id TEXT NOT NULL,
  user_name TEXT NOT NULL,
  date DATE NOT NULL,
  start_time TIMESTAMPTZ,
  end_time TIMESTAMPTZ,
  duration_minutes INTEGER DEFAULT 0,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

-- =====================================================
-- ENABLE ROW LEVEL SECURITY (RLS) — Allow all for anon
-- =====================================================
ALTER TABLE users ENABLE ROW LEVEL SECURITY;
ALTER TABLE tasks ENABLE ROW LEVEL SECURITY;
ALTER TABLE projects ENABLE ROW LEVEL SECURITY;
ALTER TABLE activity ENABLE ROW LEVEL SECURITY;
ALTER TABLE notifications ENABLE ROW LEVEL SECURITY;
ALTER TABLE work_sessions ENABLE ROW LEVEL SECURITY;

-- Allow all operations for anon key (app handles auth internally)
CREATE POLICY "Allow all for anon" ON users FOR ALL TO anon USING (true) WITH CHECK (true);
CREATE POLICY "Allow all for anon" ON tasks FOR ALL TO anon USING (true) WITH CHECK (true);
CREATE POLICY "Allow all for anon" ON projects FOR ALL TO anon USING (true) WITH CHECK (true);
CREATE POLICY "Allow all for anon" ON activity FOR ALL TO anon USING (true) WITH CHECK (true);
CREATE POLICY "Allow all for anon" ON notifications FOR ALL TO anon USING (true) WITH CHECK (true);
CREATE POLICY "Allow all for anon" ON work_sessions FOR ALL TO anon USING (true) WITH CHECK (true);

-- =====================================================
-- REALTIME — Enable for all tables
-- =====================================================
ALTER PUBLICATION supabase_realtime ADD TABLE users;
ALTER PUBLICATION supabase_realtime ADD TABLE tasks;
ALTER PUBLICATION supabase_realtime ADD TABLE projects;
ALTER PUBLICATION supabase_realtime ADD TABLE notifications;
ALTER PUBLICATION supabase_realtime ADD TABLE work_sessions;

-- =====================================================
-- INSERT YOUR FOUNDER ACCOUNT
-- =====================================================
-- ⚠️ Change name, username, password before running!
INSERT INTO users (id, name, role, title, username, password, bg)
VALUES (
  'founder-001',
  'Suraj',           -- ← Your name
  'founder',
  'Founder & CEO',
  'suraj',           -- ← Your username
  'yourpassword',    -- ← Your password (change this!)
  'linear-gradient(135deg,#f0c040,#d4a017)'
) ON CONFLICT (id) DO NOTHING;
```

### Step 3: Get API Keys

1. Supabase Dashboard → **Settings** → **API**
2. Copy:
   - **Project URL** → `https://xxxxx.supabase.co`
   - **anon / public** key → `eyJhbGci...`
3. ⚠️ **NEVER copy the service_role key!**

---

## 🌐 Deploy on GitHub Pages

### Method 1: Direct Upload

1. Create a GitHub repository (e.g., `ficusagentai-workspace`)
2. Upload these files:
   ```
   index.html
   logo.png          ← Your logo file
   README.md
   ```
3. Go to **Settings → Pages**
4. Source: **main branch → / (root)**
5. Save → Your app will be live at:
   ```
   https://yourusername.github.io/ficusagentai-workspace
   ```

### Method 2: Git CLI

```bash
git init
git add .
git commit -m "🌿 FicusAgentAI workspace v2.0"
git branch -M main
git remote add origin https://github.com/yourusername/ficusagentai-workspace.git
git push -u origin main
```

Then enable GitHub Pages in repository settings.

---

## 📁 File Structure

```
ficusagentai-workspace/
├── index.html          # Main application (all-in-one)
├── logo.png            # Your FicusAgentAI logo
└── README.md           # This file
```

---

## 👤 Login Credentials

After setup, login with:
- **Role:** Founder
- **Username:** `suraj` (or whatever you set in SQL)
- **Password:** `yourpassword` (what you set in SQL)

For employees — add them via **Team Members → + Add Member**.

---

## 🔧 Features Explained

### ⏱ Work Time Tracking
- Employees click **"Go Online"** button (top bar) when starting work
- Timer starts and shows live duration
- Click again to stop — session saved to database
- View in **My Report** or **Productivity Report**

### ↩ Carryforward Tasks
- Tasks with past due dates that are not completed show as **↩ Carryforward**
- Orange banner appears warning team members
- In **Task Manager → Carryforward** filter shows all overdue tasks
- These are automatically prioritized at top of task lists

### 🔔 Notifications
- When Founder assigns a task, the employee gets a notification popup instantly
- Bell icon (🔔) shows unread count
- Click bell to see notification panel

### 📊 Productivity Reports
- **Founder Report:** Full team analysis, work time, completion rates
- **Employee Report:** Personal performance + work sessions
- **Download PDF** with signature pad (draw your signature → Download)

### 👥 Profile Cards
- Click any team member name in sidebar or Team page
- Shows: Contact info, tasks, work time, sessions, projects

---

## 🗄️ Database Tables

| Table | Purpose |
|---|---|
| `users` | Founder + employee accounts |
| `tasks` | All tasks with status & deploy |
| `projects` | Client & internal projects |
| `activity` | Full activity log |
| `notifications` | Real-time task notifications |
| `work_sessions` | Employee online time tracking |

---

## ⚠️ Security Notes

- This app uses **Supabase Row Level Security (RLS)** with anon key
- App handles authentication internally (username/password stored in DB)
- **Do NOT use service_role key** in frontend — anon key only
- For production: Add Supabase Auth for stronger security
- Passwords are stored as plaintext in this version — for internal team use

---

## 🔄 Real-time Sync

All changes sync automatically across all open browsers:
- New task assigned → notification appears
- Task status changed → updates everywhere
- Team member goes online → status updates

---

## 📞 Support

Built by **FicusAgentAI** — AI Automation & App Building

---

*Last updated: 2026*
