SSH, Public Key, Private Key আর Digital Signature প্রথমে confusing লাগে। একটা বাস্তব উদাহরণ দিয়ে দেখি।

ধরো তুমি GitLab server-কে বলতে চাও:

> "আমি Ashraful, আমাকে push করতে দাও।"

কিন্তু GitLab কিভাবে বুঝবে তুমি সত্যিই Ashraful?

---

# Traditional Password Method

```text
Ashraful
   |
   | Username + Password
   v
GitLab Server
   |
   | Password Match?
   v
Access Granted
```

এখানে server তোমার password জানে।

---

# SSH Key Method

এখানে ২টা key থাকে:

```text
Public Key
Private Key
```

এরা mathematically related, কিন্তু Public Key দেখে Private Key বের করা যায় না।

---

# Step 1: Key Pair Generate

তুমি চালাও:

```bash
ssh-keygen -t ed25519
```

তৈরি হয়:

```text
~/.ssh/gitlablocal        <-- Private Key
~/.ssh/gitlablocal.pub    <-- Public Key
```

ASCII:

```text
             Generate
                |
                v

      +-------------------+
      |   Key Generator   |
      +-------------------+
         /            \
        /              \
       v                v

Private Key       Public Key
(Secret)           (Shareable)
```

---

# Step 2: Public Key GitLab-এ Save

তুমি GitLab-কে দাও:

```text
ssh-ed25519 AAAA....
```

GitLab store করে:

```text
GitLab Database

User: Ashraful
Public Key:
AAAA....
```

ASCII:

```text
Your PC                    GitLab

Private Key
     |
     |
Public Key -------------> Store Here
```

---

# Step 3: Push করার সময় কী হয়?

তুমি:

```bash
git push
```

দিলে flow:

```text
Your PC
   |
   | "I am Ashraful"
   |
   v
GitLab
```

GitLab সরাসরি বিশ্বাস করে না।

সে challenge পাঠায়।

---

# Step 4: Challenge

GitLab random number generate করে:

```text
837462918
```

ASCII:

```text
GitLab
   |
   | Challenge
   | 837462918
   |
   v
Your PC
```

---

# Step 5: Digital Signature

এখন Private Key দিয়ে challenge sign করা হয়।

```text
Challenge:
837462918

Private Key
     |
     v

Signature:
XYZ123ABC999
```

ASCII:

```text
           Challenge
           837462918
                |
                v

      +----------------+
      | Private Key    |
      +----------------+
                |
                v

        Digital Signature
           XYZ123ABC
```

এটাই Signature।

Signature মানে:

> "আমি Private Key-এর মালিক, তাই আমি এই challenge sign করতে পারলাম।"

---

# Step 6: Signature GitLab-এ যায়

```text
Your PC
   |
   | Signature
   |
   v
GitLab
```

ASCII:

```text
Your PC
   |
   | XYZ123ABC
   |
   v
GitLab
```

---

# Step 7: Verification

GitLab-এর কাছে Public Key আছে।

সে check করে:

```text
Signature
      +
Public Key
      |
      v

Valid?
```

ASCII:

```text
                Signature
                    |
                    v

             +-------------+
             | Public Key  |
             +-------------+
                    |
                    v

              Verification

                    |
         +----------+----------+
         |                     |
         v                     v

      Valid               Invalid
```

---

# কেন Public Key দিয়ে Verify করা যায়?

কারণ:

```text
Private Key
     |
     | Sign
     v
Signature

Public Key
     |
     | Verify
     v
Result
```

এটাই Public Key Cryptography-এর magic।

---

# সবচেয়ে গুরুত্বপূর্ণ ব্যাপার

GitLab কখনো তোমার Private Key পায় না।

```text
Private Key
    |
    | NEVER LEAVES PC
    |
    v

Your Computer
```

ASCII:

```text
+----------------------+
| Your PC              |
|                      |
| Private Key          |
|                      |
+----------------------+

         X
         X
         X

Private Key Never Sent

         X
         X
         X

+----------------------+
| GitLab Server        |
|                      |
| Public Key Only      |
|                      |
+----------------------+
```

---

# Real Git Push Flow

```text
git push
    |
    v

GitLab
    |
    | Random Challenge
    |
    v

Your PC
    |
    | Sign using Private Key
    |
    v

Digital Signature
    |
    v

GitLab
    |
    | Verify using Public Key
    |
    v

Push Allowed
```

---

তোমার GitLab SSH setup-এর ক্ষেত্রে:

```text
~/.ssh/gitlablocal
```

= Private Key (শুধু তোমার PC-তে থাকবে)

আর

```text
~/.ssh/gitlablocal.pub
```

= Public Key (GitLab-এ add করবে)

আর `~/.ssh/config` ফাইল শুধু SSH-কে বলে দেয়:

```text
When connecting to:

mygitlab

Use:

HostName 202.74.246.122
User git
IdentityFile ~/.ssh/gitlablocal
```

অর্থাৎ config-এর কাজ authentication না, **কোন server-এর জন্য কোন private key ব্যবহার করতে হবে সেটা SSH-কে জানানো**।
