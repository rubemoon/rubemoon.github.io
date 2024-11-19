---
title: "How to Remove Sensitive Information from Git History"
date: 2024-11-15 08:00:00 -0500
categories: [Git, Version Control, Best Practices]
tags: [Git, Sensitive Information, Security, Version Control]
image:
    path: /assets/img/headers/git_sensitive_info.webp
    lqip: data:image/webp;base64,UklGRkAAAABXRUJQVlA4IDQAAAAQAwCdASoUAAsAPzmGu1OvKSYisAgB4CcJYwAAW+ll18AA/rniDRhs9sq0/4Ip9WfHYHAA

---

# How to Remove Sensitive Information from Git History

Accidentally committing sensitive information to a Git repository can be a serious issue. Whether it's an API key, password, or any other confidential data, it's crucial to remove it from the repository's history to prevent unauthorized access. This post will guide you through the process of removing sensitive information from Git history using the `filter-branch` command and the BFG Repo-Cleaner.

## Why It's Important

Once sensitive information is committed to a Git repository, it becomes part of the repository's history. Even if you remove the file in a later commit, the sensitive data can still be accessed by anyone with access to the repository. Therefore, it's essential to completely remove the file from all commits in the repository's history.

## Using `filter-branch` Command

The `filter-branch` command is a powerful tool that allows you to rewrite Git history. Here's how you can use it to remove a file containing sensitive information:

### Step-by-Step Guide

1. **Backup Your Repository**

   Before making any changes, it's important to create a backup of your repository. This is because the following steps will rewrite the history, and you may need to restore the original state if something goes wrong.

2. **Remove the File from All Commits**

   Use the `filter-branch` command to remove the file from all commits:

   ```bash
   git filter-branch --force --index-filter \
   'git rm --cached --ignore-unmatch backend/database.db' \
   --prune-empty --tag-name-filter cat -- --all
   ```

3. **Clean Up the Backup Refs Created by `filter-branch`**

   After running the `filter-branch` command, clean up the backup refs:

   ```bash
   rm -rf .git/refs/original/
   git reflog expire --expire=now --all
   git gc --prune=now --aggressive
   ```

4. **Push the Changes to the Remote Repository**

   Finally, push the changes to the remote repository:

   ```bash
   git push origin --force --all
   git push origin --force --tags
   ```

   This will remove the `database.db` file from all commits in the repository's history and push the changes to the remote repository. Be aware that this is a destructive operation and will rewrite the commit history, so all collaborators will need to re-clone the repository.

## Using BFG Repo-Cleaner

The BFG Repo-Cleaner is an alternative tool that is simpler and faster than `filter-branch`. It is designed specifically for removing large files and sensitive data from Git repositories.

### Step-by-Step Guide

1. **Download and Install BFG Repo-Cleaner**

   Download the BFG Repo-Cleaner from the [official website](https://rtyley.github.io/bfg-repo-cleaner/).

2. **Run BFG Repo-Cleaner**

   Use the BFG Repo-Cleaner to remove the sensitive file:

   ```bash
   bfg --delete-files database.db
   ```

3. **Clean Up the Repository**

   After running BFG, clean up the repository:

   ```bash
   git reflog expire --expire=now --all
   git gc --prune=now --aggressive
   ```

4. **Push the Changes to the Remote Repository**

   Finally, push the changes to the remote repository:

   ```bash
   git push origin --force --all
   git push origin --force --tags
   ```

## Conclusion

Removing sensitive information from a Git repository is crucial for maintaining security. Whether you use the `filter-branch` command or the BFG Repo-Cleaner, it's important to follow the steps carefully and ensure that all collaborators are aware of the changes. By doing so, you can protect your sensitive data and maintain a secure repository.

## Read This Post on Medium

If you enjoyed this post, you can also read it on Medium at [@kozenetpro](https://medium.com/@kozenetpro).

# Explore a Custom Dashboard for Programmers Free
To help you manage your projects more effectively, I’ve designed a custom dashboard template specifically tailored for programmers. This dashboard brings together all the tools you need to stay organized and productive in one place.

Introducing Full Stack Pro 1.0 - Your Ultimate Notion Organization Template!

This template isn't just about organization—it's about growth. With an integrated Roadmap.sh example, you can set clear, actionable goals for your development journey. Fully customizable, efficient, and polished, Full Stack Pro 1.0 is crafted to unlock your coding potential and help you achieve more in less time.

![Dashboard Preview](/assets/img/dashboard-preview.webp)

Check out the dashboard template here: [Dashboard Template](https://www.kozenetpro.com/l/fullstack).