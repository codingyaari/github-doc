![GitHub Actions CI/CD Pipeline](images/github-warkflow/logo.png)

# 🚀 GitHub Actions CI/CD Pipeline Setup Guide

This guide will help you set up automated deployment to your Ubuntu server using GitHub Actions.

## 📄 Documentation PDF


<div align="center">


> **📌 Note:** The PDF document contains **all pages** with complete visual documentation including diagrams, step-by-step instructions, and troubleshooting guides for the CI/CD pipeline setup.

[![View PDF Documentation](https://img.shields.io/badge/📄_View_Complete_PDF_Guide-blue?style=for-the-badge&logo=adobe-acrobat-reader)](images/github-warkflow/Automatin.pdf)
</div>

---




## 📋 Table of Contents

- [Documentation PDF](#-documentation-pdf)
- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [GitHub Secrets Setup](#github-secrets-setup)
- [Configuration](#configuration)
- [How It Works](#how-it-works)
- [Troubleshooting](#troubleshooting)

---

## 🔧 Prerequisites

Before setting up the pipeline, ensure you have:

- ✅ A GitHub repository with your code
- ✅ An Ubuntu server with SSH access
- ✅ Node.js and npm installed on your server
- ✅ PM2 installed on your server (`npm install -g pm2`)
- ✅ Git installed on your server
- ✅ Your server's public SSH key added to GitHub (for git pull)

---

<table>
<tr>
<td width="50%">
<img src="images/github-warkflow/1.jpg" alt="Step 1" style="width: 100%;">
</td>
<td width="50%">
<img src="images/github-warkflow/2.jpg" alt="Step 2" style="width: 100%;">
</td>
</tr>
<tr>
<td width="50%">
<img src="images/github-warkflow/3.jpg" alt="Step 3" style="width: 100%;">
</td>
<td width="50%">
<img src="images/github-warkflow/4.jpg" alt="Step 4" style="width: 100%;">
</td>
</tr>
<tr>
<td width="50%">
<img src="images/github-warkflow/5.jpg" alt="Step 5" style="width: 100%;">
</td>
<td width="50%">
<img src="images/github-warkflow/6.jpg" alt="Step 6" style="width: 100%;">
</td>
</tr>
<tr>
<td width="50%">
<img src="images/github-warkflow/7.jpg" alt="Step 7" style="width: 100%;">
</td>
<td width="50%">
<img src="images/github-warkflow/8.jpg" alt="Step 8" style="width: 100%;">
</td>
</tr>
<tr>
<td width="50%">
<img src="images/github-warkflow/9.jpg" alt="Step 9" style="width: 100%;">
</td>
<td width="50%">
<img src="images/github-warkflow/10.jpg" alt="Step 10" style="width: 100%;">
</td>
</tr>
<tr>
<td width="50%">
<img src="images/github-warkflow/11.jpg" alt="Step 11" style="width: 100%;">
</td>
<td width="50%">
<img src="images/github-warkflow/12.jpg" alt="Step 12" style="width: 100%;">
</td>
</tr>
<tr>
<td width="50%">
<img src="images/github-warkflow/13.jpg" alt="Step 13" style="width: 100%;">
</td>
<td width="50%">
<img src="images/github-warkflow/14.jpg" alt="Step 14" style="width: 100%;">
</td>
</tr>
<tr>
<td width="50%" colspan="2" style="text-align: center;">
<img src="images/github-warkflow/15.jpg" alt="Step 15" style="width: 50%;">
</td>
</tr>
</table>





## 🔑 GitHub Secrets Setup

You need to add **3 secrets** to your GitHub repository for the pipeline to work.

### Step 1: Navigate to Repository Settings

1. Go to your GitHub repository
2. Click on **Settings** (top menu bar)
3. In the left sidebar, click on **Secrets and variables** → **Actions**


### Step 2: Add Secrets

Click the **New repository secret** button to add each secret.



---

### Secret 1: `SERVER_SSH_KEY` 🔐

This is your **private SSH key** used to connect to your Ubuntu server.

#### How to Get Your SSH Private Key:

**On your local machine or server:**

1. Open terminal/command prompt
2. Navigate to your SSH directory:
   ```bash
   cd ~/.ssh
   ```
3. If you don't have an SSH key, generate one:
   ```bash
   ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
   ```
4. Display your private key:
   ```bash
   cat id_rsa
   ```
   (or `cat ~/.ssh/id_rsa` if you're not in the .ssh directory)

5. **Copy the entire output** including:
   ```
   -----BEGIN RSA PRIVATE KEY-----
   [key content]
   -----END RSA PRIVATE KEY-----
   ```

#### Add to GitHub:

1. Name: `SERVER_SSH_KEY`
2. Value: Paste your entire private key
3. Click **Add secret**

* Adding SERVER_SSH_KEY secret*

---

### Secret 2: `SERVER_USER` 👤

This is your **SSH username** for connecting to the server.

#### How to Get Your Server Username:

**Common usernames:**
- `root` (for root access)
- `ubuntu` (default for Ubuntu servers)
- `admin` (custom username)
- Your custom username if you created one

#### Add to GitHub:

1. Name: `SERVER_USER`
2. Value: Your SSH username (e.g., `ubuntu`, `root`, `admin`)
3. Click **Add secret**



---

### Secret 3: `SERVER_HOST` 🌐

This is your **server's IP address or domain name**.

#### How to Get Your Server Host:

**Option 1: IP Address**
- Find your server's public IP address
- Example: `192.168.1.100` or `45.67.89.123`

**Option 2: Domain Name**
- If you have a domain pointing to your server
- Example: `example.com` or `server.example.com`

#### Add to GitHub:

1. Name: `SERVER_HOST`
2. Value: Your server IP or domain (e.g., `45.67.89.123` or `example.com`)
3. Click **Add secret**


---

### ✅ Verify All Secrets Are Added

After adding all three secrets, you should see:
 secrets listed in GitHub Secrets*

**Required Secrets:**
- ✅ `SERVER_SSH_KEY`
- ✅ `SERVER_USER`
- ✅ `SERVER_HOST`

---

## ⚙️ Configuration

### Update Deployment Path

Before using the pipeline, update the deployment path in `.github/workflows/deploy.yml`:

**Find this line (around line 36):**
```yaml
cd /var/www/html/your-root-folder/your-web-folder || {
```

**Replace with your actual path:**
```yaml
cd /var/www/html/portfolio-maker-66/portfolio-maker-web || {
```

**Also update the permission fix line (around line 43):**
```yaml
sudo chown -R $USER:$USER /var/www/html/your-root-folder/your-web-folder || {
```

**To:**
```yaml
sudo chown -R $USER:$USER /var/www/html/portfolio-maker-66/portfolio-maker-web || {
```

---

### Complete Workflow File

Here is the complete `.github/workflows/deploy.yml` file:

```yaml
# GitHub Actions CI/CD Pipeline for Ubuntu Server Deployment
# Simple git-based deployment - pulls code directly on server
name: Deploy to Ubuntu Server

# Trigger: Run this workflow when code is pushed to the main branch
on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      # Step 1: Get your code from GitHub repository
      - name: Checkout code
        uses: actions/checkout@v4

      # Step 2: Setup SSH Agent (loads your SSH key for connecting to server)
      - name: Setup SSH Agent
        uses: webfactory/ssh-agent@v0.9.0
        with:
          ssh-private-key: ${{ secrets.SERVER_SSH_KEY }}

      # Step 3: Deploy on Ubuntu Server
      # Single SSH connection - all commands run in one session
      # If any command fails, the entire step fails and job stops
      - name: Deploy on Ubuntu Server
        run: |
          set -e # Exit immediately if a command exits with a non-zero status
          set -o pipefail # Exit on pipe failures
          ssh -o StrictHostKeyChecking=no ${{ secrets.SERVER_USER }}@${{ secrets.SERVER_HOST }} << 'EOF' || exit 1
          set -e # Exit on error
          set -o pipefail # Exit on pipe failures
          
          cd /var/www/html/your-root-folder/your-web-folder || {
            echo "❌ Cannot change to directory. Exiting..."
            exit 1
          }
          
          # Fix ownership and permissions to avoid permission issues
          echo "🔧 Fixing directory permissions..."
          sudo chown -R $USER:$USER /var/www/html/your-root-folder/your-web-folder || {
            echo "❌ Failed to fix permissions. Exiting..."
            exit 1
          }
          
          # Ensure git remote uses SSH instead of HTTPS
          echo "🔗 Configuring git remote to use SSH..."
          REMOTE_URL=$(git remote get-url origin 2>/dev/null || echo "")
          if [[ "$REMOTE_URL" == https://github.com/* ]]; then
            # Convert HTTPS URL to SSH format
            REPO_PATH=$(echo "$REMOTE_URL" | sed 's|https://github.com/||' | sed 's|\.git$||')
            git remote set-url origin "git@github.com:${REPO_PATH}.git" || {
              echo "⚠️ Failed to update remote URL, but continuing..."
            }
            echo "✅ Updated remote URL to SSH format"
          elif [[ "$REMOTE_URL" == git@github.com:* ]]; then
            echo "✅ Remote already configured for SSH"
          else
            echo "⚠️ Unknown remote format: $REMOTE_URL"
          fi
          
          echo "📥 Pulling latest code from GitHub..."
          git pull origin main || {
            echo "❌ Git pull failed! Exiting..."
            exit 1
          }
          
          echo "📦 Installing dependencies..."
          npm ci || {
            echo "❌ npm ci failed! Exiting..."
            exit 1
          }
          
          echo "🔨 Building Next.js app..."
          npm run build || {
            echo "❌ Build failed! Exiting..."
            exit 1
          }
          
          echo "🔄 Restarting PM2 application..."
          pm2 restart ecosystem.config.js || {
            echo "❌ PM2 restart failed! Exiting..."
            exit 1
          }
          pm2 save || echo "⚠️ PM2 save failed, but continuing..."
          
          echo "✅ Deployment successful! 🚀"
          pm2 status
          EOF
```

> **💡 Tip:** Copy this code and save it as `.github/workflows/deploy.yml` in your repository. Don't forget to update the deployment paths as mentioned above!

---

## 🔄 How It Works

### Pipeline Flow

```
┌─────────────────┐
│  Push to main   │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Checkout Code   │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Setup SSH      │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Connect Server │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Git Pull       │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  npm ci         │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  npm run build  │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  PM2 Restart    │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  ✅ Deployed!   │
└─────────────────┘
```

### What Happens on Each Push

1. **Trigger**: When you push code to the `main` branch
2. **Checkout**: GitHub Actions checks out your repository code
3. **SSH Setup**: Loads your SSH private key
4. **Server Connection**: Connects to your Ubuntu server
5. **Deployment**: Runs all deployment commands on the server
6. **Build & Restart**: Installs dependencies, builds, and restarts your app

---

## 🐛 Troubleshooting

### Common Issues and Solutions

#### ❌ "Permission denied (publickey)"

**Problem**: SSH authentication failed

**Solutions**:
- Verify `SERVER_SSH_KEY` contains the complete private key (including BEGIN/END lines)
- Ensure the corresponding public key is in `~/.ssh/authorized_keys` on your server
- Check that `SERVER_USER` is correct

#### ❌ "Cannot change to directory"

**Problem**: The deployment path doesn't exist

**Solutions**:
- Verify the path in `deploy.yml` is correct
- Ensure the directory exists on your server
- Check folder permissions

#### ❌ "Git pull failed"

**Problem**: Git authentication or network issue

**Solutions**:
- Ensure your server's SSH key is added to GitHub
- Verify git remote is configured correctly
- Check network connectivity from server to GitHub

#### ❌ "npm ci failed"

**Problem**: Dependency installation failed

**Solutions**:
- Check if `package.json` exists
- Verify Node.js version compatibility
- Check npm version on server

#### ❌ "Build failed"

**Problem**: Next.js build error

**Solutions**:
- Check build logs in GitHub Actions
- Verify all environment variables are set
- Test build locally first

#### ❌ "PM2 restart failed"

**Problem**: PM2 process not found

**Solutions**:
- Ensure `ecosystem.config.js` exists
- Check if PM2 is installed: `npm install -g pm2`
- Verify PM2 process name matches config

---

## 📝 Additional Notes

### Security Best Practices

- 🔒 Never commit secrets to your repository
- 🔑 Use SSH keys instead of passwords
- 🛡️ Restrict SSH access to specific IPs if possible
- 🔐 Regularly rotate SSH keys

### Testing the Pipeline

1. Make a small change to your code
2. Commit and push to `main` branch
3. Go to **Actions** tab in GitHub
4. Watch the workflow run in real-time



### Manual Deployment

If you need to deploy manually:

```bash
ssh user@your-server
cd /var/www/html/your-project
git pull origin main
npm ci
npm run build
pm2 restart ecosystem.config.js
```

---

## 📚 Resources

- 📄 [Complete Setup Guide PDF](Automatin.pdf) - Detailed visual documentation with diagrams
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [SSH Key Generation Guide](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)
- [PM2 Documentation](https://pm2.keymetrics.io/docs/usage/quick-start/)

---

## ✅ Checklist

Before your first deployment, ensure:

- [ ] All 3 GitHub secrets are added (`SERVER_SSH_KEY`, `SERVER_USER`, `SERVER_HOST`)
- [ ] Deployment path is updated in `deploy.yml`
- [ ] Server has Node.js, npm, PM2, and Git installed
- [ ] Server's SSH public key is added to GitHub
- [ ] `ecosystem.config.js` exists in your project
- [ ] Tested build locally

---

## 🎉 You're All Set!

Once everything is configured, every push to `main` will automatically deploy your application! 🚀

**Need help?** Check the [Troubleshooting](#troubleshooting) section or review the GitHub Actions logs.

---

*Last updated: 2024*

