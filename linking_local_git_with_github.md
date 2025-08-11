# Link a Local Git Repository to GitHub

## Steps:
1. Create a repo on GitHub (or get its clone URL).  
2. On your machine: generate **SSH** key or prepare a Personal Access Token **(HTTPS)**.  
3. Initialize or open your local repo, add remote (`origin`), and push (`git push -u origin main`).  
4. Verify and use the normal fetch/pull/push workflow.  

---

## 1) Prepare your identity locally (git user config):

Replace your username & email with your actual username and email ID:

```bash
  git config --global user.name "Your User_Name"
  git config --global user.email "your@email.com"
```

---

## 2) Authentication options

### A. SSH key (Preferred for developer machines)

1. Generate an ed25519 key — replace your email with your actual email ID:
   
```bash
  ssh-keygen -t ed25519 -C "your@email.com"

  #Accept default (~/.ssh/id_ed25519) or give a custom name if you already have keys
```

2. Start the SSH agent and add the key:
   
```bash
# Start agent
  eval "$(ssh-agent -s)"

# Add key to agent
  ssh-add ~/.ssh/id_ed25519
```

3. Add SSH config(Optional):
   
```
cd ~/.ssh/config
Host github.com
  HostName github.com
  User git
  AddKeysToAgent yes
  IdentityFile ~/.ssh/id_ed25519
  ServerAliveInterval 60
  ServerAliveCountMax 5
  TCPKeepAlive yes
```

4. Copy the public key and add it to your GitHub account:
   
```bash
  cat ~/.ssh/id_ed25519.pub
```

5. Test the connection:
   
```bash
  ssh -T git@github.com
```

### B. HTTPS using Personal Access Token (PAT)
- GitHub no longer accepts passwords for HTTPS Git — use a PAT.
- Create PAT in GitHub: Settings → Developer settings → Personal access tokens.
- Example:
  
```bash
  git clone https://github.com/OWNER/REPO.git
```

---

## 3) Create or connect to a GitHub repo

### A. Remote repo exists

    Find repo clone URL on GitHub → Code button.

### B. New local project

    On GitHub: New repository → follow commands to push existing repo.

---

## 4) Full workflows for Local repo commands

### Init local repo and push first time

```bash
  cd ~/my-project
  git init
  git add .
  git commit -m "Initial commit"
  git branch -M main
  git remote add origin git@github.com:OWNER/REPO.git
  git push -u origin main
```

### Add/change remote

```bash
  git remote add origin git@github.com:OWNER/REPO.git
  git remote -v
  git remote set-url origin git@github.com:OWNER/NEWREPO.git
  git remote remove origin
```

---

## 5) Daily workflow (fetch, pull, push)

```bash
  git fetch origin
  git pull origin main
  git config pull.rebase true
  git push origin main
```
---

## 6) Upstream/tracking branches

```bash
  git branch --set-upstream-to=origin/main main
  git push --set-upstream origin feature/x
  git branch -vv
```
---

## 7) Common Problems and Solutions
- Permission denied (publickey) → Check SSH key, agent, GitHub settings.
- Host key verification failed → Update known_hosts.
- Password authentication removed → Use PAT.
- Stale remote-tracking branches → git remote prune origin.
---

## 8) Best practices
- Prefer ed25519 keys.
- Use --force-with-lease instead of --force.
- Never commit secrets.
---

## 9) Complete Example:

```bash
  ssh-keygen -t ed25519 -C "your@email.com"
  eval "$(ssh-agent -s)"
  ssh-add ~/.ssh/id_ed25519
  cat ~/.ssh/id_ed25519.pub                                        #Copy the public key and add it to your GitHub account
  ssh -T git@github.com

  git init
  git add .
  git commit -m "Initial commit"
  git branch -M main
  git remote add origin git@github.com:OWNER/REPO.git
  git push -u origin main

  git remote set-url origin git@github.com:OWNER/NEWREPO.git
  git remote remove origin

  git fetch origin
  git pull origin main
  git push
```
