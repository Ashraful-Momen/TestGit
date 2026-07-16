# Git Team Collaboration Cheat Sheet

## 📌 Core Concepts

| Concept              | Description                                          |
| -------------------- | ---------------------------------------------------- |
| **Commit**           | Save point for your code (like a game save)          |
| **Snapshot**         | Picture of your entire code at a specific time       |
| **Branch**           | Separate line of development (like a tree branch)    |
| **Incoming Changes** | Changes from the branch you're merging INTO          |
| **Current Changes**  | Changes already in your current branch               |

### 🧪 Real Scenario: Your First Day on a Team Project

**Situation:** You joined a team building an e-commerce app. The project is on GitHub and you need to add a "Contact Us" page.

```
Current state of the project (main branch):
shop/
├── index.html          (homepage — commit #a1b2c3)
├── products.html       (product list — commit #d4e5f6)
└── style.css           (shared styles — commit #g7h8i9)

What you'll do:
  1. Clone the repo          → get the snapshot at commit #g7h8i9
  2. Create a branch         → feature/contact-page (separate line of work)
  3. Add contact.html        → stage & commit (new save point)
  4. Push & create PR        → merge your branch back into main
```

**How the concepts connect:**
- The repo is a chain of **commits** (snapshots). Each commit is a full picture of every file.
- Your `feature/contact-page` **branch** is an independent timeline. You can break things without affecting main.
- When your PR merges, **incoming changes** (your branch) join **current changes** (main).

---

## 🚀 Setup & Essential Commands

### One-Time Setup

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --list                  # Verify settings
```

### Daily Workflow (The 4-Step Dance)

```bash
git status                         # What's changed?
git add .                          # Stage all changes
git commit -m "Description"        # Save changes
git push origin branch-name        # Upload to GitHub
```

### 🧪 Real Scenario: First Commit on a New Machine

**Situation:** You just got a new laptop. You need to configure Git and make your first contribution to the team's project.

```bash
# STEP 1: Tell Git who you are (one-time only)
$ git config --global user.name "Ashraful Islam"
$ git config --global user.email "ashraful@company.com"

# STEP 2: Clone the team repo
$ git clone https://github.com/team/ecommerce-app.git
$ cd ecommerce-app

# STEP 3: Create your feature branch
$ git checkout -b feature/add-footer

# STEP 4: Make changes (you edit index.html, add footer)
$ echo "<footer>© 2026 MyShop</footer>" >> index.html

# STEP 5: The 4-step dance
$ git status
On branch feature/add-footer
Changes not staged for commit:
  modified:   index.html

$ git add .
$ git commit -m "Add footer to homepage"
[feature/add-footer a1b2c3d] Add footer to homepage
 1 file changed, 1 insertion(+)

$ git push origin feature/add-footer
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Writing objects: 100% (3/3), 312 bytes | 312.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0)
remote: Create a pull request for 'feature/add-footer' on GitHub
To https://github.com/team/ecommerce-app.git
 * [new branch]      feature/add-footer -> feature/add-footer
```

**What just happened:** Your change is now on GitHub. The team can see it, review it, and merge it. You followed the 4-step dance: status → add → commit → push.

---

## 🌿 Branch Workflow (Team Essential)

### Branch Commands

```bash
git branch                         # See local branches
git branch -a                      # See all branches (including remote)
git checkout -b new-branch         # Create & switch to new branch
git checkout existing-branch       # Switch to another branch
git branch -d branch-name          # Delete a branch
git branch --show-current          # See which branch you're on
```

### The Golden Rule

> **NEVER commit directly to main/master**
>
> 1. Create feature branch
> 2. Make changes
> 3. Push branch
> 4. Create Pull Request (PR)
> 5. Get review
> 6. Merge via PR

### Standard Development Flow

```bash
# 1. Always start from latest main
git checkout main
git pull origin main

# 2. Create feature branch with meaningful name
git checkout -b feature/login-page
# OR: bugfix/header-error, enhancement/dashboard-ui

# 3. Work on your changes — commit frequently
git add .
git commit -m "Add login form validation"

# 4. Push branch to GitHub
git push origin feature/login-page

# 5. Create Pull Request (base: main, compare: your branch)
# 6. Get team review & merge
```

### 🧪 Real Scenario: Building a Login Page While a Coworker Fixes a Bug

**Situation:** You're building a login page. Your teammate Rima is fixing a broken header on the same project. You both work on separate branches so you never block each other.

```bash
# ── YOUR MORNING (Ashraful) ──
$ git checkout main
$ git pull origin main
Already up to date.

$ git checkout -b feature/login-page
Switched to a new branch 'feature/login-page'

# Create login.html and login.css...
$ git add login.html login.css
$ git commit -m "Add login page with email/password fields"
[feature/login-page f1a2b3c] Add login page with email/password fields
 2 files changed, 85 insertions(+)

$ git push origin feature/login-page

# ── RIMA'S MORNING (simultaneously) ──
$ git checkout main
$ git pull origin main
Already up to date.

$ git checkout -b bugfix/header-error
Switched to a new branch 'bugfix/header-error'

# Fix header.html...
$ git add header.html
$ git commit -m "Fix broken header navigation links"
[bugfix/header-error d4e5f6g] Fix broken header navigation links
 1 file changed, 3 insertions(+), 3 deletions(-)

$ git push origin bugfix/header-error
```

```bash
# List all branches — see both yours and Rima's
$ git branch -a
* feature/login-page
  main
  remotes/origin/main
  remotes/origin/feature/login-page
  remotes/origin/bugfix/header-error
```

**Key takeaway:** You both worked on the same project, at the same time, touching different files. Zero conflicts. Zero waiting. Branches make parallel work possible.

---

## 🔀 Merging Strategies

### Fast-Forward Merge (No Conflicts)

```bash
git checkout main
git merge feature-branch
```

### Merge with Conflicts

```bash
git add .                          # Stage resolved files
git commit -m "Merge complete"     # Commit merge
```

### PR Merge (Best Practice — Recommended)

1. Create Pull Request in GitHub
2. Review changes
3. Click "Merge Pull Request"
4. Delete feature branch

### 🧪 Real Scenario: Merging Your Completed Feature

**Situation:** Your login page PR was approved. Now you merge it into main. Meanwhile, main hasn't changed since you branched off, so it's a clean fast-forward.

```bash
# Your branch is ready, PR is approved. Rima (team lead) merges it:

$ git checkout main
Switched to branch 'main'

$ git pull origin main
Already up to date.

$ git merge feature/login-page
Updating a1b2c3d..f1a2b3c
Fast-forward                           # ← No conflicts!
 login.html | 65 +++++++++++++++++++++++++++++++++++
 login.css  | 20 +++++++++++
 2 files changed, 85 insertions(+)

$ git push origin main

# Clean up the merged branch
$ git branch -d feature/login-page
Deleted branch feature/login-page (was f1a2b3c).

$ git push origin --delete feature/login-page
```

**What "Fast-forward" means:** main simply moved its pointer forward to include your commits. No merge commit needed — the history stays linear and clean.

---

## 🔴 Conflict Resolution — Deep Dive

### Why Conflicts Happen

```
Team Member A              Team Member B
    |                           |
    |-- main (v1) --------------|
    |                           |
    | create branch A           | create branch B
    | edit line 5-10            | edit line 5-10
    | commit                    | commit
    | push                      | push
    |                           |
    |---- MERGE CONFLICT -------|
    |    Both changed same lines!
```

### Types of Conflicts

| Conflict Type        | Description                 | When It Happens            |
| -------------------- | --------------------------- | -------------------------- |
| **Content Conflict** | Same lines modified         | Most common                |
| **Delete/Modify**    | One deletes, other modifies | File removed in one branch |
| **Rename/Edit**      | One renames, other edits    | File renamed in one branch |

### VS Code Resolution (Recommended)

VS Code highlights conflicts with markers:

```
<<<<<<< HEAD          # Current changes (your branch)
   console.log("Hello World");
=======                 # Incoming changes (other branch)
   console.log("Hello Team");
>>>>>>> main
```

| Option                        | Action         | When to Use             |
| ----------------------------- | -------------- | ----------------------- |
| **Accept Current Change**     | Keep your code | Your change is correct  |
| **Accept Incoming Change**    | Use their code | Their change is better  |
| **Accept Both Changes**       | Keep both      | Both changes are needed |
| **Compare Changes**           | See side-by-side | When unsure           |

### Command Line Resolution

```bash
git status                        # See which files have conflicts
git checkout --ours file.js       # Keep current changes
git checkout --theirs file.js     # Keep incoming changes
git mergetool                     # 3-way diff tool
git diff --name-only --diff-filter=U  # List all conflicted files
```

### Decision Tree

```
Is there a conflict?
    ↓
YES → Open VS Code
    ↓
Can you see both changes?
    ↓
YES → Make decision:
    • Keep yours?   → Accept Current
    • Keep theirs?  → Accept Incoming
    • Keep both?    → Accept Both
    • Combine?      → Manual Edit
    ↓
Save file → git add . → git commit
```

### 🧪 Real Scenario: Two Developers Change the Same Button

**Situation:** You and Rima both edited the homepage button in `index.html`. You changed its color, she changed its text. Rima's PR got merged first. Now your branch has a conflict.

**The setup:**

```
index.html before anyone touched it (in main):
┌─────────────────────────────────────────────┐
│ 40 │ <button class="btn-primary">Buy Now</button> │
└─────────────────────────────────────────────┘

Your branch (feature/red-button):
┌─────────────────────────────────────────────┐
│ 40 │ <button class="btn-danger">Buy Now</button>  │  ← You changed class
└─────────────────────────────────────────────┘

Rima's branch (bugfix/button-text) — MERGED FIRST:
┌─────────────────────────────────────────────┐
│ 40 │ <button class="btn-primary">Shop Now</button> │  ← She changed text
└─────────────────────────────────────────────┘
```

**You hit the conflict:**

```bash
$ git checkout feature/red-button
$ git merge main
Auto-merging index.html
CONFLICT (content): Merge conflict in index.html   # ← Git can't decide!
Automatic merge failed; fix conflicts and then commit the result.

$ git status
On branch feature/red-button
You have unmerged paths.
  (fix conflicts and run "git commit")
  Unmerged paths:
    both modified:   index.html
```

**What VS Code shows you:**

```html
<<<<<<< HEAD (Current Change — YOUR code)
<button class="btn-danger">Buy Now</button>
======= (Incoming Change — main, includes Rima's code)
<button class="btn-primary">Shop Now</button>
>>>>>>> main
```

**Your thought process:**
- Rima's text change ("Shop Now") is better — keep that.
- Your color change (`btn-danger` / red) is what the design team asked for — keep that.
- Neither "Accept Current" nor "Accept Incoming" works alone. You need a **manual edit**.

**Resolution — manual edit:**

```html
<button class="btn-danger">Shop Now</button>
<!--     ^ your change        ^ Rima's change -->
```

```bash
# After saving the file in VS Code:
$ git add index.html
$ git commit -m "Merge main: keep red button style + Rima's new text"
[feature/red-button c3d4e5f] Merge main: keep red button style + Rima's new text

$ git push origin feature/red-button
```

**What you learned:** Conflicts aren't "you vs. them." They're "which combination of changes is correct?" Often the right answer is a mix of both.

---

## 🤝 Common Team Scenarios

### Scenario 1: Get Latest Code from Team

```bash
git pull
```

### Scenario 2: Your Branch Is Behind Main

```bash
git checkout main
git pull
git checkout your-branch
git merge main
```

### Scenario 3: Switch Branches with Unsaved Work

```bash
git stash                         # Save work temporarily
git checkout other-branch
# ... do work ...
git checkout original-branch
git stash pop                     # Restore work
```

### Scenario 4: Resolve a Real Conflict

```bash
# Two developers changed the same function differently
git checkout main && git pull
git checkout your-branch
git merge main

# VS Code shows conflict — choose resolution, then:
git add .
git commit -m "Resolved merge conflict"
git push
```

### 🧪 Real Scenario: The Urgent Hotfix Interruption

**Situation:** You're halfway through building a new dashboard page. Suddenly, your team lead says: "The checkout page is broken in production — drop everything and fix it NOW." But you have 2 hours of uncommitted work.

```bash
# You're in the middle of dashboard work — files are half-done, NOT ready to commit:
$ git status
On branch feature/dashboard
Changes not staged for commit:
  modified:   dashboard.html
  modified:   dashboard.css
  modified:   dashboard.js
  new file:   chart-component.js

# The emergency arrives. Save your work in a stash:
$ git stash
Saved working directory and index state WIP on feature/dashboard: e7f8g9h WIP: dashboard charts

# Now your working directory is CLEAN — back to the last commit.
# Switch to main and create the hotfix branch:
$ git checkout main
$ git pull origin main
$ git checkout -b hotfix/checkout-crash
Switched to a new branch 'hotfix/checkout-crash'

# Fix the bug (it was a null reference):
# Edit checkout.js — add a null check...
$ git add checkout.js
$ git commit -m "Fix null reference crash on checkout page"
[hotfix/checkout-crash a9b0c1d] Fix null reference crash on checkout page
 1 file changed, 3 insertions(+)

$ git push origin hotfix/checkout-crash
# Create PR → get it reviewed FAST → merge → deploy. Crisis averted.

# Now go back to your feature and restore work:
$ git checkout feature/dashboard
Switched to branch 'feature/dashboard'

$ git stash pop
On branch feature/dashboard
Changes not staged for commit:
  modified:   dashboard.html
  modified:   dashboard.css
  modified:   dashboard.js
  new file:   chart-component.js
Dropped refs/stash@{0} (a1b2c3d4e5f6...)

# All your work is back exactly as you left it. Resume building.
```

**Key takeaway:** `git stash` is your "pause button." It shelves your work instantly, gives you a clean working directory, and lets you restore everything later. You handled a production emergency without losing a single line of your dashboard code.

---

## 📂 .gitignore — Complete Guide

### What Is .gitignore?

A file that tells Git which files/folders to **ignore** and **never track** in your repository — your first line of defense against committing sensitive data and unnecessary files.

### Create & Use

```bash
touch .gitignore
```

### Essential Patterns

```bash
# Sensitive files
.env
.env.local
*.pem
*.key

# Dependencies
node_modules/
vendor/
venv/
__pycache__/

# Build output
dist/
build/
*.min.js
*.min.css

# Logs & temp
*.log
*.tmp
*.bak

# IDE & OS
.vscode/
.idea/
.DS_Store
Thumbs.db
Desktop.ini

# Negation (exclude from ignore)
!important.log
```

### Project Templates

**Node.js / React:**
```bash
node_modules/
dist/
build/
.env
.env.local
*.log
coverage/
.DS_Store
```

**Python:**
```bash
__pycache__/
*.py[cod]
venv/
env/
.env
dist/
*.sqlite3
```

**PHP:**
```bash
vendor/
node_modules/
.env
*.log
*.sqlite
.idea/
```

### Cache Clearing — Full Workflow

> Files already tracked by Git won't be ignored just because you added them to .gitignore. You must clear Git's index first.

```bash
# Step 1: Update .gitignore with new patterns
# Step 2: Clear all cache
git rm -r --cached .

# Step 3: Re-add everything (respecting .gitignore)
git add .

# Step 4: Commit
git commit -m "Update .gitignore and clear cache"

# Step 5: Push
git push origin main
```

### Common Cache Scenarios

```bash
# Accidentally committed .env
git rm --cached .env
echo ".env" >> .gitignore
git add .gitignore && git commit -m "Remove .env from repo"

# Remove node_modules from repo
git rm -r --cached node_modules/
echo "node_modules/" >> .gitignore
git add .gitignore && git commit -m "Remove node_modules from version control"

# After team updates .gitignore
git pull origin main
git rm -r --cached .
git add .
git commit -m "Apply updated .gitignore rules"
```

### Check What's Ignored

```bash
git check-ignore -v .env           # Check specific file
git ls-files --others --ignored --exclude-standard  # List all ignored
git status --ignored               # Show status including ignored files
```

### 🧪 Real Scenario: The Leaked API Key Panic

**Situation:** You accidentally committed and pushed your `.env` file containing the production database password and Stripe secret key. This is a **security incident** — anyone with repo access can now see your secrets.

```bash
# The horror moment — you see this in your PR:
$ git show HEAD
+DB_PASSWORD=MyProdDBP@ssword123!
+STRIPE_SECRET_KEY=sk_live_abc123xyz789
+AWS_ACCESS_KEY=AKIAIOSFODNN7EXAMPLE
# ^^^ THIS IS NOW IN YOUR GIT HISTORY FOREVER (unless you act)

# ⚠️  STEP 1 (IMMEDIATE): Rotate the exposed credentials FIRST.
#    Go to Stripe dashboard → regenerate secret key.
#    Go to AWS IAM → deactivate the exposed key, create new one.
#    Go to database → change the password.
#    You cannot undo what's already pushed — revoke the secrets NOW.

# STEP 2: Remove the file from Git (but keep it locally)
$ git rm --cached .env
rm '.env'

# STEP 3: Ensure .gitignore blocks it going forward
$ echo ".env" >> .gitignore
$ echo ".env.local" >> .gitignore
$ echo ".env.*" >> .gitignore

# STEP 4: Also create a safe template for the team
$ cat > .env.example << 'EOF'
# Copy this file to .env and fill in real values
DB_HOST=localhost
DB_PASSWORD=your_password_here
STRIPE_SECRET_KEY=sk_test_your_key_here
AWS_ACCESS_KEY=your_access_key_here
EOF

# STEP 5: Commit the fix
$ git add .gitignore .env.example
$ git commit -m "Remove .env from repo; add .env.example template"
[main f9e8d7c] Remove .env from repo; add .env.example template
 3 files changed, 8 insertions(+), 1 deletion(-)

$ git push origin main

# STEP 6 (IMPORTANT): Rewrite history to scrub the secret from old commits
$ git log --oneline
f9e8d7c Remove .env from repo; add .env.example template
b2c3d4e Add login feature
a1b2c3d Initial commit with .env   # ← This old commit still has the secret!

# Use git filter-branch or BFG Repo-Cleaner to scrub it from history.
# Then force-push (with team coordination):
$ git push --force-with-lease origin main
```

**Prevention for next time:**
```bash
# Create .gitignore BEFORE the first commit — always:
$ cat > .gitignore << 'EOF'
.env
.env.*
*.pem
*.key
credentials.json
EOF

$ git add .gitignore
$ git commit -m "Initial commit with .gitignore"
# Now .env will never be tracked.
```

**Key takeaway:** `.gitignore` is not optional — it's your security boundary. Set it up before your first commit. And if you do leak a secret, **rotate the credential first** (the internet already has it), then clean up Git.

---

## 🆘 Emergency Commands

| Problem                                 | Solution                                          |
| --------------------------------------- | ------------------------------------------------- |
| Wrong commit message                    | `git commit --amend -m "New message"`             |
| Undo last commit (keep changes)         | `git reset --soft HEAD~1`                         |
| Undo last commit (discard changes)      | `git reset --hard HEAD~1`                         |
| Can't push (unrelated histories)        | `git pull origin main --allow-unrelated-histories`|
| Branch is behind remote                 | `git pull origin main` then `git push`            |
| Untracked files would be overwritten    | `git stash && git pull && git stash pop`          |
| Abort a messy merge                     | `git merge --abort`                               |
| Permission denied                       | Check GitHub permissions or SSH keys              |
| Force push (last resort!)               | `git push --force-with-lease origin your-branch`  |

### 🧪 Real Scenario: "Oh No, I Committed to main!"

**Situation:** You were in a rush and committed directly to `main` instead of creating a feature branch. Your team's golden rule says "never commit to main." You need to undo this without losing your work.

```bash
# The mistake:
$ git checkout main         # Forgot to create a branch first!
$ echo "new feature code" >> app.js
$ git add app.js
$ git commit -m "Add search feature"
[main d3a4b5c] Add search feature        # ← Committed to main! 😱

# The fix — undo the commit but keep your changes:
$ git reset --soft HEAD~1
# Your commit is gone from main's history, but app.js still has your changes.

# Now do it properly:
$ git checkout -b feature/search
Switched to a new branch 'feature/search'

$ git add app.js
$ git commit -m "Add search feature"
[feature/search e6f7g8h] Add search feature
# Now your commit is on a feature branch where it belongs.

$ git push origin feature/search
# Create PR → get review → merge properly. Crisis averted.
```

**What if you also need to undo a pushed commit?**

```bash
# If you already pushed the bad commit to main:
$ git log --oneline
d3a4b5c Add search feature              # ← this is on GitHub's main now
c2d3e4f Previous good commit

# Create a REVERT commit (safe — doesn't rewrite history):
$ git revert d3a4b5c
[main f9g0h1i] Revert "Add search feature"
# This creates a NEW commit that undoes the bad one. Safe to push.

$ git push origin main
```

**Key difference:**
| Command | What it does | Safe for pushed commits? |
|---------|-------------|--------------------------|
| `git reset --soft HEAD~1` | Erases commit, keeps changes | ❌ No — rewrites history |
| `git reset --hard HEAD~1` | Erases commit AND changes | ❌ No — destroys work |
| `git revert <commit>` | Creates undo commit | ✅ Yes — adds to history |

---

## 📋 GitHub Pull Request Process

### Via GitHub UI

1. Push: `git push origin feature-branch`
2. Go to GitHub → "Pull Requests" → "New Pull Request"
3. Base = main, Compare = your branch
4. Add title & description
5. Click "Create Pull Request"
6. Wait for review & approval

### Via Terminal (GitHub CLI)

```bash
gh pr create --base main --head feature-branch --title "Title" --body "Description"
```

### 🧪 Real Scenario: Your First PR — From Push to Merge

**Situation:** You've finished the "Contact Us" page. Now you need to get it reviewed and merged through a Pull Request.

```bash
# STEP 1: Push your branch
$ git push origin feature/contact-page
remote: Create a pull request for 'feature/contact-page' on GitHub
To https://github.com/team/ecommerce-app.git
 * [new branch]      feature/contact-page -> feature/contact-page

# STEP 2: Create the PR (via terminal)
$ gh pr create \
    --base main \
    --head feature/contact-page \
    --title "Add Contact Us page with form validation" \
    --body "## What changed
- Added contact.html with name, email, and message fields
- Added client-side form validation
- Styled to match existing design system

## How to test
1. Go to /contact.html
2. Submit the form with empty fields → should see validation errors
3. Submit with valid data → should see success message

## Screenshot
(attached in PR)"

Creating pull request for feature/contact-page into main in team/ecommerce-app
https://github.com/team/ecommerce-app/pull/42

# STEP 3: The review process
# Rima reviews your PR and leaves a comment:
# "Looks great! One small thing — can we add a phone number field?"

# You address the feedback:
$ echo '<input type="tel" name="phone">' >> contact.html
$ git add contact.html
$ git commit -m "Add phone number field per review feedback"
$ git push origin feature/contact-page
# The PR updates automatically — no need to create a new one.

# STEP 4: Rima approves. You merge:
$ gh pr merge 42 --squash --delete-branch
✓ Squashed and merged PR #42
✓ Deleted branch feature/contact-page

# Done! Your code is now in main.
$ git checkout main
$ git pull origin main
$ git log --oneline -3
a9b0c1d Add Contact Us page with form validation (#42)
e7f8g9h Fix header navigation links (#41)
d6e5f4c Add product search (#40)
```

**What the team sees in the PR:**

```
Title:    Add Contact Us page with form validation
Author:   ashraful
Status:   Merged
Reviewer: rima (approved)

Files changed: 2
  contact.html  | +65 lines
  contact.css   | +30 lines

Conversation:
  ashraful: "Ready for review! Let me know if anything needs changing."
  rima:     "Looks great! One small thing — can we add a phone number field?"
  ashraful: "Done! Added in the latest commit."
  rima:     "Perfect, approved! ✅"
```

---

## 🏗️ Full Project Setup (Team Workflow)

### Project Owner: Create Repository

```bash
mkdir my-team-project && cd my-team-project
git init
echo "# My Team Project" >> README.md
echo "node_modules/" >> .gitignore
echo ".env" >> .gitignore
git add .
git commit -m "Initial project setup"
git branch -M main
git remote add origin https://github.com/your-org/my-team-project.git
git push -u origin main
```

### Set Up Branch Protection (GitHub)

Settings → Branches → Add rule for `main`:
- ✅ Require pull request reviews
- ✅ Require status checks
- ❌ Disable force pushes

### Team Members: Clone & Setup

```bash
git clone https://github.com/your-org/my-team-project.git
cd my-team-project
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

### 🧪 Real Scenario: Starting a New Team Project from Scratch

**Situation:** Your team of 4 is starting a new React + Node.js e-commerce app. You're the project lead — you set up the repo, configure branch protection, and onboard everyone.

**A typical team structure:**

```
my-ecommerce-app/
├── .gitignore
├── .env.example
├── README.md
├── frontend/              ← Alice owns this
│   ├── components/
│   └── pages/
├── backend/               ← Bob owns this
│   ├── routes/
│   ├── controllers/
│   └── models/
└── tests/                 ← Diana (QA) owns this

Team members:
  👤 Alice  — Frontend developer (feature/alice-*)
  👤 Bob    — Backend developer  (feature/bob-*)
  👤 Carol  — Full-stack         (feature/carol-*)
  👤 Diana  — QA / testing       (feature/diana-*)
```

**Day 1 — Project lead (you) sets up:**

```bash
$ mkdir my-ecommerce-app && cd my-ecommerce-app
$ git init

# Create .gitignore FIRST
$ cat > .gitignore << 'EOF'
node_modules/
dist/
build/
.env
.env.local
*.log
.DS_Store
.vscode/
coverage/
EOF

# Create .env.example (safe template — never .env!)
$ cat > .env.example << 'EOF'
DB_HOST=localhost
DB_PASSWORD=your_password_here
JWT_SECRET=your_secret_here
STRIPE_KEY=sk_test_your_key_here
EOF

$ echo "# MyEcommerce - Team Project" > README.md

$ git add .
$ git commit -m "Initial project setup with .gitignore and .env.example"
$ git branch -M main
$ git remote add origin https://github.com/team/my-ecommerce-app.git
$ git push -u origin main
```

**Day 1 — Team members join:**

```bash
# Everyone runs this:
$ git clone https://github.com/team/my-ecommerce-app.git
$ cd my-ecommerce-app

# Alice starts her first feature:
$ git checkout -b feature/alice-navbar
$ echo "<nav>MyEcommerce</nav>" > frontend/components/NavBar.js
$ git add .
$ git commit -m "Add NavBar component"
$ git push origin feature/alice-navbar

# Bob starts his first feature:
$ git checkout -b feature/bob-auth-api
$ mkdir -p backend/routes
$ echo "const auth = require('express').Router();" > backend/routes/auth.js
$ git add .
$ git commit -m "Add auth route scaffold"
$ git push origin feature/bob-auth-api

# No conflicts — they're working in different directories.
```

**What branch protection prevents:**

```
Without protection:
  Alice: git checkout main && git commit -m "oops" && git push
  → main is now polluted. Everyone's work is at risk.

With protection:
  Alice: git push origin main
  → remote: error: GH006: Protected branch update failed
  → She CANNOT push to main. Must use a PR. Everyone is safe.
```

---

## 🛡️ Conflict Prevention Strategies

1. **Communicate** — Daily standup: "I'm working on the login page"
2. **Small PRs** — 2-3 files, not 50 files / 10,000 lines
3. **Frequent pulls** — Pull main at least twice a day, and before creating a PR
4. **Clear file ownership** — Assign directories to specific developers
5. **Break up large files** — Split monolithic components into smaller ones

### 🧪 Real Scenario: How a Team Avoids Conflicts for a Week

**Situation:** A 4-person team builds a dashboard feature over one week. Here's how they organize to avoid stepping on each other.

**Monday standup — task breakdown:**

```
Dashboard feature breakdown:
  ├── DashboardLayout.js     → Alice
  ├── StatsWidget.js         → Bob
  ├── ChartWidget.js         → Carol
  └── dashboard.test.js      → Diana (starts Thursday, after components done)
```

**Bad approach (what they AVOID):**
```bash
# ❌ One giant branch with all dashboard work
git checkout -b feature/dashboard
# Alice, Bob, Carol all commit to this branch
# Result: constant merge conflicts, can't review separately, 50-file PR nightmare
```

**Good approach (what they DO):**
```bash
# ✅ Each person has their own branch, small and focused

# Alice:
$ git checkout -b feature/dashboard-layout
# Creates DashboardLayout.js (1 file, 80 lines)
# PR #55 — reviewed and merged in 2 hours

# Bob (same day, no waiting):
$ git checkout -b feature/stats-widget
# Creates StatsWidget.js (1 file, 120 lines)
# PR #56 — reviewed and merged in 3 hours

# Carol (same day, no waiting):
$ git checkout -b feature/chart-widget
# Creates ChartWidget.js (1 file, 150 lines)
# PR #57 — reviewed and merged in 4 hours
```

**What made this work:**
- Each person touched **different files**. No overlap = no conflicts.
- Small PRs got **fast reviews** (reviewing 80 lines takes minutes; reviewing 2000 lines takes hours — or never happens).
- Everyone pulled main before starting each morning:

```bash
# Bob's Tuesday morning routine:
$ git checkout main
$ git pull origin main         # Gets Alice's merged layout
$ git checkout feature/stats-widget
$ git merge main               # Brings layout into his branch
# Now StatsWidget can import DashboardLayout — Bob can test integration locally.
```

**The result after one week:**
```
Merged PRs:
  #55  DashboardLayout     (Mon, Alice)
  #56  StatsWidget         (Mon, Bob)
  #57  ChartWidget         (Tue, Carol)
  #58  Dashboard styles    (Wed, Alice)
  #59  Dashboard tests     (Thu, Diana)
  #60  Wire up API data    (Fri, Bob)

Total: 6 PRs, ~600 lines of code, ZERO merge conflicts.
```

---

## 🏆 Best Practices

### ✅ Do

- Pull before you push
- Commit small, commit often
- Write clear commit messages (explain WHAT and WHY)
- Use feature branches
- Create PRs for code review
- Review your own code before a PR
- Communicate with your team
- Use `.gitignore` properly — and clear cache when updating it

### ❌ Don't

- Force push to main
- Commit directly to main
- Ignore conflicts
- Push without pulling first
- Make giant commits
- Skip code review
- Commit sensitive data (.env, secrets, keys)
- Merge without testing

### 🧪 Real Scenario: Bad Commit vs. Good Commit

**Situation:** You fixed a bug where the logout button didn't clear the user session. Compare the two commit messages.

**❌ Bad commit message:**
```
$ git log --oneline
a1b2c3d fix
d4e5f6g update stuff
g7h8i9j changes
h0i1j2k more fixes
```

```bash
$ git show a1b2c3d
commit a1b2c3d
Author: ashraful <ashraful@company.com>
Date:   Wed Jul 16 2026 14:32:00

    fix                    # ← What does "fix" mean? Which bug? Why?
```

Three months later, a teammate finds a regression. They run `git blame` and see your commit. "fix" tells them nothing — they have to read the entire diff and guess your intent.

**✅ Good commit message:**
```
$ git log --oneline
a1b2c3d Clear session cookie on logout to prevent replay attacks
d4e5f6g Add rate limiting to login endpoint (5 req/min)
g7h8i9j Refactor cart total calculation — extract to CartUtils.js
h0i1j2k Update product card to show sale badge when discount > 0
```

```bash
$ git show a1b2c3d
commit a1b2c3d
Author: ashraful <ashraful@company.com>
Date:   Wed Jul 16 2026 14:32:00

    Clear session cookie on logout to prevent replay attacks

    Problem: After logout, the session cookie was not cleared.
    An attacker could reuse the old cookie to access the account
    without re-authenticating.

    Fix: Set cookie maxAge to 0 and call session.destroy()
    in the /logout route handler.

    Tested: Manual test — logged in, logged out, verified cookie
    is removed and old cookie is rejected by auth middleware.
```

**The difference:**
| Aspect | Bad | Good |
|--------|-----|------|
| Title | "fix" | "Clear session cookie on logout to prevent replay attacks" |
| WHAT changed? | Unknown | Clear the cookie, destroy session |
| WHY? | Unknown | Prevent attackers from reusing old cookies |
| How to verify? | Unknown | Manual test steps included |
| Can a teammate understand it 6 months later? | No | Yes, instantly |

**The commit message formula:**
```
Short (50-char) summary in imperative mood

(blank line)

- What was the problem?
- Why does this fix it?
- Any side effects or things to watch for?
```

---

## 🎯 Team Agreements

| Convention        | Format                                      |
| ----------------- | ------------------------------------------- |
| **Branch names**  | `feature/name`, `bugfix/name`, `enhancement/name` |
| **Commit messages** | "Add login validation" — not "Update stuff" |
| **PR descriptions** | What changed, why, how to test             |
| **Review SLA**    | Within 24 hours                             |

---

## 📝 Quick Reference Card

```bash
# 80% of what you'll use daily:
git status                         # What's changed?
git add .                          # Stage all changes
git commit -m "message"            # Save changes
git push origin branch-name        # Upload to GitHub
git pull                           # Download latest
git checkout -b new-branch         # Create & switch branch
git checkout existing-branch       # Switch branch
git merge branch-name              # Merge branch
git stash / git stash pop          # Temporarily save/restore work
git log --oneline                  # See commit history
git branch --show-current          # Which branch am I on?
```

### Conflict Resolution Quick Commands

```bash
git merge --abort                  # Stop merge
git checkout --ours file           # Keep your version
git checkout --theirs file         # Keep their version
```

---

## 🎯 30-Minute Training Plan

| Time        | Topic                | Activity                              |
| ----------- | -------------------- | ------------------------------------- |
| 0-5 min     | Core Concepts        | Commit → branch → merge flow          |
| 5-10 min    | Setup & Daily Flow   | git init, add, commit, push demo      |
| 10-15 min   | Branch Workflow      | Create branch, switch, merge          |
| 15-20 min   | Pull Requests        | PR creation & review process          |
| 20-25 min   | Conflict Resolution  | Demo merge conflict fix in VS Code    |
| 25-30 min   | Q&A + Practice       | Team members try commands             |

---

## 📚 Recommended Tools

- **Git Graph** (VS Code Extension) — See commit history visually
- **GitLens** (VS Code Extension) — See who wrote each line
- **GitHub Desktop** — Simple GUI for Git operations

---

**Remember:** Conflicts are normal — they mean your team is collaborating. The goal is to work together smoothly, not to be a Git expert! 😊
