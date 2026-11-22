# Publishing This Repo from Visual Studio Code to GitHub

## One-time setup

1. Install [Git](https://git-scm.com/downloads) and [Visual Studio Code](https://code.visualstudio.com/).
2. Create a free [GitHub](https://github.com) account if you don't have one.
3. Tell Git who you are (once per laptop):

   ```bash
   git config --global user.name  "Your Name"
   git config --global user.email "you@example.com"
   ```
4. In VS Code, install the recommended extensions when prompted (PowerShell, Markdown Preview Mermaid Support, Markdown All in One).

## Option A — Publish with the VS Code UI (easiest)

1. Unzip the kit and open the folder: **File → Open Folder…** → `azure-network-watcher-troubleshooting-kit`.
2. Open the **Source Control** panel (Ctrl+Shift+G) and click **Initialize Repository**.
3. Review the files, type a message such as `Initial commit: Network Watcher troubleshooting kit v1`, and click **Commit**.
4. Click **Publish Branch**. VS Code will ask you to sign in to GitHub, then choose **public** or **private**.
5. Open the new repo on github.com and check that the README renders (including the Mermaid diagrams).

## Option B — Publish with the terminal

Create an empty repo on github.com first (no README, no license), then from the project folder:

```bash
git init -b main
git add .
git commit -m "Initial commit: Network Watcher troubleshooting kit v1"
git remote add origin https://github.com/<your-username>/azure-network-watcher-troubleshooting-kit.git
git push -u origin main
```

## Day-to-day workflow (one scenario per week)

```bash
git checkout -b scenario-03-update        # optional branch
# edit the scenario, paste real lab output, add screenshots
git add scenarios/Scenario-03-VM-To-VM-Spoke.md
git commit -m "Scenario 03: add real lab output and screenshots"
git push
```

Committing one scenario at a time keeps the history clean and pairs naturally with a weekly LinkedIn post.

## Before you make it public — checklist

- [ ] Replace `<your name>` in `README.md` and in `LICENSE`.
- [ ] Replace every *illustrative* output block with output from your own lab.
- [ ] Search the repo for subscription IDs, tenant IDs, real public IPs, storage account names, or any client names, and remove them.
- [ ] Never commit credentials, `.env` files, publish profiles, or `.cap` capture files (the included `.gitignore` blocks common ones).
- [ ] Add screenshots to `diagrams/` or a `docs/images/` folder and link them from the scenarios.
- [ ] Verify the items in the "Things to verify" section of [`NetworkWatcherOverview.md`](NetworkWatcherOverview.md).

## Optional: turn it into a GitHub Pages site

In the GitHub repo go to **Settings → Pages**, choose **Deploy from a branch**, select `main` and the `/ (root)` or `/docs` folder. For a nicer portfolio look, add a Jekyll theme later. The markdown files already work as the content.
