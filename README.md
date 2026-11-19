# G Cleaner

A small Windows app that frees space in a GitLab container registry. It scans the images in a group (or every project you're a member of), proposes old tags to delete by simple rules, lists them with checkboxes, and deletes only what you confirm.

**[Download GCleaner.exe](https://github.com/NikolaCunovic/gcleaner-releases/releases/latest/download/GCleaner.exe)** (Windows 10 and 11, always the newest version)

## What it does

- Reads every image and tag in a GitLab group and its subgroups, or only the repos you type next to **Scan**.
- Sorts tags into kinds by what follows the version number: releases (`6.11.20261111`), `-rc`, `-final`, `-test` and so on. Tags like `latest`, `main` or a commit id are "other" and kept by default.
- Keeps the newest tag of each kind, either in every version line (6.4, 6.5…) or in total. You set how many with the steppers on the left.
- Everything older is ticked for deleting. Untick anything you want to keep, and it stays unticked in later scans.
- **Export list** saves every tag with what would happen to it and why, as a CSV, to review before deleting.
- Updates itself: when a new version is out, a green **Update to …** button appears at the top.

## What it never deletes

- **Deployed tags.** G Cleaner reads every `image:` line in the deployments repos you've set up in [Deployer](https://github.com/NikolaCunovic/deployer-releases), before every scan. If any of them can't be read, nothing can be deleted until a new scan reads them all.
- **Images on your list.** Settings › Images that always stay: one image per line, as in a manifest, for versions that run somewhere that isn't in a deployments repo.
- **Tags on the same image** as a tag that stays: deleting them would free nothing.
- **Tags you unticked.**
- `latest`.

Deleting asks first, listing every tag by name. One delete removes at most 100 tags (you can change that in Settings). If a tag was pushed again since the scan, it's left alone.

Without Deployer, turn off **Never delete deployed tags** in Settings, and check the list carefully before deleting. Only plain `image:` lines are read. Images set through Kustomize `images:` or Helm values aren't recognised as deployed.

## Install

1. Download **GCleaner.exe**. If your browser says the file "isn't commonly downloaded", choose **Keep**.
2. Put it in a folder where it can stay, for example `C:\Users\<you>\Apps\GCleaner\`.
3. Run it. Windows may show **"Windows protected your PC"**: click **More info**, then **Run anyway**. This appears because the app isn't signed with a paid certificate.
4. In Settings, enter your GitLab address and, optionally, the group to scan. If Deployer is set up on this PC, its GitLab address and token are used, and there's nothing else to enter. Otherwise create a personal access token in GitLab (**Preferences › Access tokens**) with the **api** scope and paste it.
5. Click **Scan**.

GitLab frees the disk space when its registry garbage collection runs, not right away.

## Your data stays on your PC

- Settings: `%APPDATA%\GCleaner\settings.json`.
- Your GitLab token: Windows Credential Manager, never in a file.
- The app talks only to your GitLab, and to GitHub to check for a new version (nothing about you is sent). There's no account, cloud or tracking.

## Updating

G Cleaner checks for a new version when it starts and every few hours. When one is out, click **Update to …** at the top and choose **Update and restart**. Your settings and token are kept. You can also check in **Settings**, bottom left.

If updating doesn't work: close G Cleaner, replace `GCleaner.exe` with the new download, and start it again.

## Requirements

Windows 10 or 11 with the Microsoft Edge WebView2 Runtime, which Windows 11 and up-to-date Windows 10 already include. A GitLab with the container registry.
