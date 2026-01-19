# GitHub Actions CI/CD - Staging & Production Deployment

[![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?style=for-the-badge&logo=github-actions&logoColor=white)](https://github.com/features/actions)
[![GitHub Pages](https://img.shields.io/badge/GitHub_Pages-222222?style=for-the-badge&logo=github&logoColor=white)](https://pages.github.com/)

Automated CI/CD pipeline for deploying static websites to GitHub Pages with separate staging and production environments.

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Architecture](#architecture)
- [Quick Start](#quick-start)
- [Workflow](#workflow)
- [Setup Guide](#setup-guide)
- [Project Structure](#project-structure)
- [Contributing](#contributing)

## 🎯 Overview

This project demonstrates a professional CI/CD setup using GitHub Actions with **2-repository architecture**:

- **Source/Staging Repository**: Where you edit code, auto-deploy staging to the same repo
- **Production Repository**: Separate repository for production deployment (manual)
- **Separate URLs**: Staging and production have different URLs for isolated testing

## ✨ Features

- ✅ **Automated Staging**: Auto-deploy on push/PR to main/master
- ✅ **Manual Production**: Controlled production deployment
- ✅ **Separate Environments**: Staging and production with different URLs
- ✅ **HTML Validation**: Validates HTML before deployment
- ✅ **Error Handling**: Proper error checking and reporting
- ✅ **Documentation**: Comprehensive setup and troubleshooting guides

## 🏗️ Architecture

```
┌──────────────────────────┐
│  Source/Staging Repo     │
│  (Edit code + Staging)   │
└──────────┬───────────────┘
           │
      Push/PR Code
           │
           ▼
┌─────────────────────┐
│  GitHub Actions     │
│  Workflow           │
└──────────┬──────────┘
           │
      ┌────┴────┐
      │         │
      ▼         ▼
┌──────────┐ ┌──────────────┐
│  Staging │ │  Production  │
│ (Same    │ │    Repo      │
│  Repo)   │ │  (External)  │
└────┬─────┘ └──────┬───────┘
     │              │
     ▼              ▼
┌──────────┐ ┌──────────────┐
│  Staging │ │  Production  │
│   URL    │ │     URL      │
└──────────┘ └──────────────┘
```

## 🚀 Quick Start

### Prerequisites

- GitHub account
- Source repository with GitHub Actions enabled
- Personal Access Token (PAT) for deployment

### Installation

1. **Clone the source repository**
   ```bash
   git clone <your-source-repo-url>
   cd <repo-name>
   ```

2. **Enable GitHub Pages for Staging (Current Repo)**
   - Settings → Pages → Source: GitHub Actions
   - This repo will serve as staging environment

3. **Setup Production Repository**
   - Create a new repository: `[repo-name]-production`
   - Enable GitHub Pages: Settings → Pages → Deploy from branch `gh-pages`

4. **Configure Secrets**
   - Go to: Settings → Secrets and variables → Actions
   - Add `PRODUCTION_REPO_TOKEN`: Your Personal Access Token
   - Add `PRODUCTION_REPO` (optional): `username/repo-production`

5. **Deploy**
   ```bash
   git add .
   git commit -m "Initial commit"
   git push origin main
   ```

See [SETUP.md](./SETUP.md) for detailed setup instructions.

## 🔄 Workflow

### Staging Deployment (Automatic)

**Trigger:** Push to `main`/`master` or Pull Request

**Process:**
1. Checkout code from source repository
2. Validate HTML file
3. Deploy to the same repository (GitHub Pages)
4. Website available at: `https://[username].github.io/[repo-name]/`

### Production Deployment (Manual)

**Trigger:** Manual workflow dispatch

**Process:**
1. Checkout code from source repository
2. Validate HTML file
3. Deploy to production repository (external)
4. Website available at: `https://[username].github.io/[repo-production]/`

## 📁 Project Structure

```
.
├── .github/
│   └── workflows/
│       └── deploy.yml          # Main CI/CD workflow
├── frontend/
│   └── index.html              # Static website file
├── .gitignore                  # Git ignore rules
├── LICENSE                     # MIT License
├── README.md                   # This file
└── SETUP.md                    # Detailed setup guide
```

## 🛠️ Development

### Adding New Files

1. Add HTML/CSS/JS files to `frontend/` directory
2. Update `index.html` to reference new files
3. Commit and push - staging will auto-deploy

### Testing

1. Push changes to trigger staging deployment
2. Test on staging URL
3. If OK, manually trigger production deployment

## 📚 Documentation

- [Setup Guide](./SETUP.md) - Detailed setup instructions for 3-repository architecture
- [GitHub Actions Docs](https://docs.github.com/en/actions)
- [GitHub Pages Docs](https://docs.github.com/en/pages)

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📝 License

This project is open source and available under the [MIT License](LICENSE).

## 🙏 Acknowledgments

- [GitHub Actions](https://github.com/features/actions)
- [GitHub Pages](https://pages.github.com/)
- [peaceiris/actions-gh-pages](https://github.com/peaceiris/actions-gh-pages)

---

**Made with ❤️ using GitHub Actions**
