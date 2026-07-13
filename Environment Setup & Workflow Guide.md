---
description: Environment Setup & Workflow Guide
---

# Environment Setup & Workflow Guide

This document serves as the master reference guide for maintaining and rebuilding the environment setup for this Applied Information Science Master's Thesis.

---

## 1. Where to Store: OneDrive & GitHub Configuration

To maintain real-time passive cloud backup alongside explicit version history, follow this architecture exactly to prevent synchronization file locks.

### Root Folder Layout
```text
OneDrive - University/
└── Thesis_Project/
    ├── document/        # LaTeX template & written sections (pulled from Overleaf if needed)
    ├── docs/            # Environment and documentation guides
    └── src/             # Coding scripts, data loaders, and model setups

## 2. GitHub Repository & Configuration

git init
git add .
git commit -m "Initial commit: Thesis environment setup"

git remote add origin [https://github.com/walter-telsnig/Master-s-Thesis-WS26-](https://github.com/walter-telsnig/Master-s-Thesis-WS26-)
git branch -M main
git push -u origin main

## 3. Python Dependency & Virtual Environment Management
# Windows
python -m venv .venv

# Create a clean starting requirements list
echo -e "pandas\nnumpy\npython-dotenv" > requirements.txt

# Run installation mapping
pip install -r requirements.txt

# Snap precise version configurations directly back to disk
pip freeze > requirements.txt

### 3.1 Daily Pipeline for Adding Libraries
Run local environment installation: pip install <package-name>
Overwrite the deployment blueprint tracker: pip freeze > requirements.txt
Commit and push requirements.txt straight to GitHub.

## 4. Bibliography Automation (Zotero & Overleaf Premium)
graph LR
    Browser["Browser<br>(Google Scholar / IEEE)"] -->|1-Click Capture| Zotero["Zotero Desktop<br>(Local Database)"]
    Zotero -->|Automatic Sync| ZoteroCloud["Zotero Cloud<br>(zotero.org)"]
    ZoteroCloud -->|Direct API Link| Overleaf["Overleaf Premium<br>(Cloud LaTeX)"]

    subgraph Desktop Environment
        Zotero
    end

    subgraph Cloud Infrastructure
        ZoteroCloud
        Overleaf
    end


