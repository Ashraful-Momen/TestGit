Here's the **updated and complete note** for configuring Git with SSH on **Parrot OS**, now including the `git clone` command and step-by-step workflow.

---

# 🛠️ Complete Git + SSH Configuration Guide for Parrot OS

This guide will help you:
- Generate an SSH key
- Add it to GitHub / GitLab / Bitbucket
- Configure Git globally
- Clone a repo using SSH
- Switch from HTTPS to SSH remote URLs

---

## 🔐 1. Check for Existing SSH Keys

```bash
ls -al ~/.ssh
```

Look for files like:
- `id_rsa`, `id_rsa.pub`
- `id_ed25519`, `id_ed25519.pub` ← **Recommended**
- `id_ecdsa`, `id_ecdsa.pub`

If nothing exists, generate one.

---

## 🧑‍💻 2. Generate a New SSH Key

For modern systems (recommended):

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

For older systems:

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

When prompted:
- Press `Enter` to accept default file location (`~/.ssh/id_ed25519`)
- Optionally set a passphrase (recommended for security)

---

## 🧪 3. Start SSH Agent and Add Your Key

Start the agent:

```bash
eval "$(ssh-agent bash)"
```

Add your private key:

```bash
ssh-add ~/.ssh/id_ed25519
```

> Use `id_rsa` if you generated an RSA key.

Verify:

```bash
ssh-add -l
```

You should see something like:

```
256 SHA256:abc123xyz... ~/.ssh/id_ed25519 (ED25519)
```

---

## 📋 4. Copy Your Public SSH Key

Show your public key:

```bash
cat ~/.ssh/id_ed25519.pub
```

Copy the entire line that looks like:

```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIMx6JHZtHAAg9g7BCZ7b5vRA1VYgc1fXV6AYlPOScIfa your_email@example.com
```

---

## 🌐 5. Add SSH Key to Git Hosting Service

### 🔗 GitHub:
- https://github.com/settings/keys
- Click **New SSH key**
- Paste your key and save

### 🔗 GitLab:
- https://gitlab.com/-/profile/keys

### 🔗 Bitbucket:
- https://bitbucket.org/account/settings/ssh-keys/

---

## 🛰 6. Test SSH Connection

Test with GitHub:

```bash
ssh -T git@github.com
```

Expected output:

```
Hi username! You've successfully authenticated, but GitHub does not provide shell access.
```

For GitLab:

```bash
ssh -T git@gitlab.com
```

---

## 📦 7. Set Global Git Config (Name & Email)

Set your name and email (used in commits):

```bash
git config --global user.name "Your Full Name"
git config --global user.email "your_email@example.com"
```

Verify:

```bash
git config --list
```

---

## 🧩 8. Clone a Repo Using SSH

To clone a repo:

```bash
git clone git@github.com:Ashraful-Momen/TestGit.git
```

Then enter the directory:

```bash
cd TestGit
```

Now you can work inside the repo.

---

## 🔄 9. Change Remote URL from HTTPS to SSH (if needed)

Check current remote:

```bash
git remote -v
```

Change to SSH:

```bash
git remote set-url origin git@github.com:Ashraful-Momen/TestGit.git
```

Verify again:

```bash
git remote -v
```

---

## 🎯 10. Optional: Auto-start SSH Agent on Login

Edit `.bashrc`:

```bash
nano ~/.bashrc
```

Add at the bottom:

```bash
# Auto-start SSH agent
if [ -z "$SSH_AUTH_SOCK" ]; then
  eval "$(ssh-agent bash)"
  ssh-add ~/.ssh/id_ed25519 &>/dev/null
fi
```

Save and exit (`Ctrl+O`, `Enter`, `Ctrl+X`), then reload:

```bash
source ~/.bashrc
```

---

## 🧼 11. Clean Up (Optional)

If you created multiple keys or want to start fresh:

```bash
rm -v ~/.ssh/id_ed25519*
ssh-keygen -t ed25519 -C "your_email@example.com"
```

---

# ✅ Summary Checklist

| Step | Task |
|------|------|
| ✅   | Generate SSH key |
| ✅   | Add key to SSH agent |
| ✅   | Copy public key |
| ✅   | Paste key into GitHub / GitLab / Bitbucket |
| ✅   | Test SSH connection |
| ✅   | Set global Git name/email |
| ✅   | Clone repo using SSH |
| ✅   | Switch remotes to SSH if needed |
| ✅   | Auto-start SSH agent (optional) |

---

Let me know if you want help:
- Using GUI tools like VS Code, GitKraken
- Managing multiple Git accounts with SSH
- Setting up SSH config for advanced use

Happy hacking! 🐧🔐
