হ্যাঁ, GitLab access করার মূলত ২টা জনপ্রিয় উপায় আছে:

## Method 1: HTTPS + Personal Access Token (PAT)

Remote URL:

```text
http://202.74.246.122:7000/root/banking_ctb_demo.git
```

Flow:

```text
Git Push
    ↓
GitLab Username
    ↓
Personal Access Token
    ↓
Authentication Success
```

প্রতিবার token দিতে না চাইলে credential manager/store ব্যবহার করা যায়।

উদাহরণ:

```bash
git config --global credential.helper store
```

---

## Method 2: SSH Key Authentication

Flow:

```text
Private Key (Your PC)
        ↓
     Sign
        ↓
GitLab Server
        ↓
Public Key Verify
        ↓
Access Granted
```

এখানে Password/Token লাগে না।

---

## তাহলে ~/.ssh/config কেন?

আসলে `config` ফাইল **অবশ্যই লাগবে না**।

তুমি চাইলে সরাসরি লিখতে পারো:

```bash
git remote set-url origin git@202.74.246.122:root/banking_ctb_demo.git
```

এবং SSH key default location-এ (`~/.ssh/id_ed25519`) থাকলে config-এর দরকারই নেই।

---

### Config ফাইলের সুবিধা

ধরো তোমার ৩টা Git server আছে:

```text
GitHub
Office GitLab
Local GitLab
```

আর ৩টা আলাদা key:

```text
~/.ssh/github
~/.ssh/office
~/.ssh/gitlablocal
```

SSH কীভাবে বুঝবে কোন server-এর জন্য কোন key ব্যবহার করবে?

তাই:

```text
Host mygitlab
    HostName 202.74.246.122
    User git
    IdentityFile ~/.ssh/gitlablocal
```

মানে:

```text
mygitlab
   ↓
202.74.246.122
   ↓
Use ~/.ssh/gitlablocal
```

এখন command:

```bash
ssh git@mygitlab
```

SSH automatically জানে:

```text
HostName = 202.74.246.122
Key = ~/.ssh/gitlablocal
User = git
```

---

### Git Remote-এ সুবিধা

Config ছাড়া:

```bash
git remote set-url origin git@202.74.246.122:root/banking_ctb_demo.git
```

Config সহ:

```bash
git remote set-url origin git@mygitlab:root/banking_ctb_demo.git
```

ছোট, সুন্দর, এবং multiple key manage করা সহজ।

---

## VS Code-এর সাথে config-এর সম্পর্ক?

VS Code আলাদা কিছু করে না।

VS Code → Git Extension → Git CLI → SSH

Flow:

```text
VS Code
   ↓
Git
   ↓
SSH
   ↓
~/.ssh/config
   ↓
Correct Key Selected
```

তাই VS Code-এর জন্য config না, **SSH-এর জন্য config**।

---

## তোমার ক্ষেত্রে

যদি শুধু এই একটাই GitLab server ব্যবহার করো, তাহলে:

```bash
ssh-keygen -t ed25519
```

Default location:

```text
~/.ssh/id_ed25519
~/.ssh/id_ed25519.pub
```

Public key GitLab-এ add করো, তারপর:

```bash
git remote set-url origin git@202.74.246.122:root/banking_ctb_demo.git
```

এতেই কাজ হবে, `~/.ssh/config` লাগবে না।

`config` তখনই বেশি দরকার হয় যখন:

* একাধিক Git server
* একাধিক SSH key
* Custom SSH port
* GitHub + GitLab + Bitbucket একসাথে ব্যবহার করো।
