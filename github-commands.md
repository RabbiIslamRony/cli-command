# GitHub CLI & Git Commands Reference

A comprehensive collection of GitHub CLI (`gh`) and Git commands for everyday use.

---

## Table of Contents

- [GitHub CLI Setup](#github-cli-setup)
- [Repository Commands](#repository-commands)
- [Pull Request Commands](#pull-request-commands)
- [Issue Commands](#issue-commands)
- [Branch Commands](#branch-commands)
- [Commit Commands](#commit-commands)
- [Stash Commands](#stash-commands)
- [Remote Commands](#remote-commands)
- [Tag & Release Commands](#tag--release-commands)
- [Diff & Log Commands](#diff--log-commands)
- [GitHub Actions Commands](#github-actions-commands)
- [Gist Commands](#gist-commands)
- [SSH & Authentication](#ssh--authentication)
- [Undo & Fix Commands](#undo--fix-commands)
- [Advanced Commands](#advanced-commands)
- [Common Workflows](#common-workflows)

---

## GitHub CLI Setup

```sh
# Install GitHub CLI (Windows — winget)
winget install --id GitHub.cli

# Install GitHub CLI (macOS)
brew install gh

# Install GitHub CLI (Linux — Debian/Ubuntu)
sudo apt install gh

# Authenticate with GitHub
gh auth login

# Check auth status
gh auth status

# Refresh auth token
gh auth refresh

# Switch between GitHub accounts
gh auth switch
```

---

## Repository Commands

### Create & Clone

```sh
# Clone a repository
git clone https://github.com/user/repo.git

# Clone with SSH
git clone git@github.com:user/repo.git

# Clone a specific branch
git clone -b branch-name https://github.com/user/repo.git

# Shallow clone (latest commit only — faster)
git clone --depth 1 https://github.com/user/repo.git

# Create a new repo (GitHub CLI)
gh repo create repo-name --public
gh repo create repo-name --private
gh repo create repo-name --public --clone   # create + clone locally

# Fork a repository
gh repo fork user/repo
gh repo fork user/repo --clone              # fork + clone locally

# View repo info
gh repo view user/repo
gh repo view user/repo --web                # open in browser

# List your repositories
gh repo list
gh repo list --limit 50
gh repo list --public
gh repo list --private

# Delete a repository
gh repo delete user/repo --yes

# Archive a repository
gh repo archive user/repo
```

### Initialize a Local Repo

```sh
# Initialize a new git repo
git init

# Initialize + set default branch to main
git init -b main

# Add remote origin
git remote add origin https://github.com/user/repo.git

# First push to set upstream
git push -u origin main
```

---

## Pull Request Commands

```sh
# Create a pull request
gh pr create
gh pr create --title "PR title" --body "Description"
gh pr create --draft                        # create as draft
gh pr create --base main                    # target branch
gh pr create --reviewer user1,user2         # add reviewers
gh pr create --assignee @me                 # assign to yourself
gh pr create --label "bug,priority"         # add labels
gh pr create --web                          # open form in browser

# List pull requests
gh pr list
gh pr list --state open
gh pr list --state closed
gh pr list --state merged
gh pr list --author @me
gh pr list --label "bug"
gh pr list --search "is:open review-requested:@me"

# View a pull request
gh pr view 123
gh pr view 123 --web                        # open in browser
gh pr view 123 --comments                   # show comments

# Check out a PR locally
gh pr checkout 123

# Review a pull request
gh pr review 123 --approve
gh pr review 123 --request-changes --body "Please fix X"
gh pr review 123 --comment --body "Looks good overall"

# Merge a pull request
gh pr merge 123
gh pr merge 123 --merge                     # merge commit
gh pr merge 123 --squash                    # squash and merge
gh pr merge 123 --rebase                    # rebase and merge
gh pr merge 123 --auto                      # auto-merge when checks pass
gh pr merge 123 --delete-branch             # delete branch after merge

# Close / reopen
gh pr close 123
gh pr reopen 123

# Mark as ready for review (from draft)
gh pr ready 123

# Add comment to PR
gh pr comment 123 --body "This looks great!"

# View PR diff
gh pr diff 123

# View PR checks/status
gh pr checks 123

# Edit PR
gh pr edit 123 --title "New title"
gh pr edit 123 --add-label "enhancement"
gh pr edit 123 --add-reviewer user1
```

---

## Issue Commands

```sh
# Create an issue
gh issue create
gh issue create --title "Bug report" --body "Description"
gh issue create --label "bug"
gh issue create --assignee @me
gh issue create --web                       # open form in browser

# List issues
gh issue list
gh issue list --state open
gh issue list --state closed
gh issue list --assignee @me
gh issue list --label "bug"
gh issue list --search "is:open sort:created-desc"

# View an issue
gh issue view 45
gh issue view 45 --web                      # open in browser
gh issue view 45 --comments                 # show comments

# Close / reopen
gh issue close 45
gh issue close 45 --reason "completed"
gh issue reopen 45

# Add comment
gh issue comment 45 --body "Working on this"

# Edit issue
gh issue edit 45 --title "Updated title"
gh issue edit 45 --add-label "priority"
gh issue edit 45 --add-assignee user1

# Pin / unpin
gh issue pin 45
gh issue unpin 45

# Transfer issue to another repo
gh issue transfer 45 user/other-repo

# Delete an issue
gh issue delete 45
```

---

## Branch Commands

```sh
# List branches
git branch                                  # local branches
git branch -r                               # remote branches
git branch -a                               # all branches
git branch -v                               # with last commit

# Create a new branch
git branch feature-name
git checkout -b feature-name                # create + switch
git switch -c feature-name                  # create + switch (modern)

# Switch branches
git checkout branch-name
git switch branch-name                      # modern syntax

# Rename a branch
git branch -m old-name new-name
git branch -M main                          # force rename current branch

# Delete a branch
git branch -d branch-name                   # safe delete (merged only)
git branch -D branch-name                   # force delete
git push origin --delete branch-name        # delete remote branch

# Track a remote branch
git checkout --track origin/branch-name

# Set upstream for current branch
git push -u origin branch-name

# Show branches merged into current
git branch --merged
git branch --no-merged

# Clean up stale remote-tracking branches
git remote prune origin
git fetch --prune
```

---

## Commit Commands

```sh
# Stage files
git add file.txt                            # stage specific file
git add .                                   # stage all changes
git add -A                                  # stage all (including deletions)
git add -p                                  # interactive staging (hunks)
git add *.js                                # stage by pattern

# Commit
git commit -m "commit message"
git commit -m "title" -m "description"      # multi-line
git commit -am "message"                    # stage tracked files + commit
git commit --amend                          # amend last commit
git commit --amend -m "new message"         # amend message only
git commit --amend --no-edit                # amend without changing message
git commit --allow-empty -m "empty commit"  # trigger CI/CD

# View commit details
git show                                    # last commit details
git show abc1234                            # specific commit
git show HEAD~2                             # 2 commits ago

# Cherry-pick
git cherry-pick abc1234                     # apply a specific commit
git cherry-pick abc1234..def5678            # range of commits
git cherry-pick --abort                     # cancel cherry-pick
```

---

## Stash Commands

```sh
# Save current changes to stash
git stash
git stash push -m "work in progress"        # with message
git stash push -p                           # interactive (select hunks)
git stash --include-untracked               # include new files

# List stashes
git stash list

# Apply stash
git stash apply                             # apply latest, keep in stash
git stash apply stash@{2}                   # apply specific stash
git stash pop                               # apply latest + remove from stash
git stash pop stash@{2}                     # apply specific + remove

# View stash contents
git stash show                              # summary
git stash show -p                           # full diff
git stash show stash@{1} -p                 # specific stash diff

# Drop / clear stash
git stash drop                              # drop latest
git stash drop stash@{2}                    # drop specific
git stash clear                             # remove all stashes

# Create branch from stash
git stash branch new-branch stash@{0}
```

---

## Remote Commands

```sh
# List remotes
git remote -v

# Add a remote
git remote add origin https://github.com/user/repo.git
git remote add upstream https://github.com/original/repo.git

# Remove a remote
git remote remove remote-name

# Rename a remote
git remote rename old-name new-name

# Change remote URL
git remote set-url origin https://github.com/user/new-repo.git

# Fetch from remote
git fetch                                   # fetch from origin
git fetch --all                             # fetch from all remotes
git fetch upstream                          # fetch from specific remote

# Pull (fetch + merge)
git pull
git pull origin main
git pull --rebase                           # fetch + rebase instead of merge

# Push
git push
git push origin branch-name
git push -u origin branch-name              # set upstream + push
git push --all                              # push all branches
git push --tags                             # push all tags
git push --force-with-lease                 # safer force push

# Show remote details
git remote show origin
```

---

## Tag & Release Commands

### Git Tags

```sh
# List tags
git tag
git tag -l "v1.*"                           # filter tags

# Create tags
git tag v1.0.0                              # lightweight tag
git tag -a v1.0.0 -m "Version 1.0.0"       # annotated tag
git tag -a v1.0.0 abc1234                   # tag a specific commit

# Push tags
git push origin v1.0.0                      # push specific tag
git push origin --tags                      # push all tags

# Delete tags
git tag -d v1.0.0                           # delete local tag
git push origin --delete v1.0.0             # delete remote tag

# View tag details
git show v1.0.0
```

### GitHub Releases

```sh
# Create a release
gh release create v1.0.0
gh release create v1.0.0 --title "Release v1.0.0" --notes "Release notes"
gh release create v1.0.0 --draft            # create as draft
gh release create v1.0.0 --prerelease       # mark as pre-release
gh release create v1.0.0 --generate-notes   # auto-generate notes
gh release create v1.0.0 ./dist/*.zip       # upload assets

# List releases
gh release list

# View a release
gh release view v1.0.0
gh release view v1.0.0 --web                # open in browser

# Download release assets
gh release download v1.0.0
gh release download v1.0.0 --pattern "*.zip"

# Edit a release
gh release edit v1.0.0 --title "New title"
gh release edit v1.0.0 --draft=false         # publish draft

# Delete a release
gh release delete v1.0.0
gh release delete v1.0.0 --cleanup-tag       # also delete the tag
```

---

## Diff & Log Commands

### Diff

```sh
# View changes
git diff                                    # unstaged changes
git diff --staged                           # staged changes
git diff HEAD                               # all changes vs last commit
git diff branch1..branch2                   # between branches
git diff abc1234..def5678                   # between commits
git diff --stat                             # summary only
git diff --name-only                        # file names only
git diff -- path/to/file                    # specific file
```

### Log

```sh
# View commit history
git log
git log --oneline                           # compact view
git log --oneline -10                       # last 10 commits
git log --graph --oneline --all             # visual branch graph
git log --author="username"                 # by author
git log --since="2024-01-01"                # since date
git log --until="2024-12-31"                # until date
git log --grep="keyword"                    # search commit messages
git log -S "code_string"                    # search for code changes
git log --follow -- file.txt                # file history (follows renames)
git log --stat                              # with file change stats
git log --pretty=format:"%h %an %s"         # custom format
git log branch1..branch2                    # commits in branch2 not in branch1

# Who changed what
git blame file.txt                          # line-by-line author info
git blame -L 10,20 file.txt                 # specific line range

# Short log
git shortlog -sn                            # commit count per author
```

---

## GitHub Actions Commands

```sh
# List workflow runs
gh run list
gh run list --workflow=build.yml
gh run list --branch main
gh run list --status failure

# View a specific run
gh run view 123456
gh run view 123456 --web                    # open in browser
gh run view 123456 --log                    # show full logs
gh run view 123456 --log-failed             # show only failed logs

# Watch a run in progress
gh run watch 123456

# Re-run a workflow
gh run rerun 123456
gh run rerun 123456 --failed                # rerun only failed jobs

# Cancel a run
gh run cancel 123456

# Trigger a workflow manually
gh workflow run build.yml
gh workflow run build.yml --ref branch-name
gh workflow run deploy.yml -f environment=staging

# List workflows
gh workflow list

# View workflow details
gh workflow view build.yml

# Enable / disable workflow
gh workflow enable build.yml
gh workflow disable build.yml
```

---

## Gist Commands

```sh
# Create a gist
gh gist create file.txt                     # secret gist
gh gist create file.txt --public            # public gist
gh gist create file.txt --desc "Description"
gh gist create file1.txt file2.txt          # multi-file gist

# List gists
gh gist list
gh gist list --public
gh gist list --secret

# View a gist
gh gist view GIST_ID
gh gist view GIST_ID --web                  # open in browser

# Edit a gist
gh gist edit GIST_ID
gh gist edit GIST_ID --add newfile.txt      # add file to gist
gh gist edit GIST_ID --filename file.txt    # edit specific file

# Clone a gist
gh gist clone GIST_ID

# Delete a gist
gh gist delete GIST_ID
```

---

## SSH & Authentication

```sh
# Generate SSH key
ssh-keygen -t ed25519 -C "your_email@example.com"

# Start SSH agent
eval "$(ssh-agent -s)"

# Add key to agent
ssh-add ~/.ssh/id_ed25519

# Copy public key (macOS)
pbcopy < ~/.ssh/id_ed25519.pub

# Copy public key (Windows Git Bash)
cat ~/.ssh/id_ed25519.pub | clip

# Add SSH key to GitHub
gh ssh-key add ~/.ssh/id_ed25519.pub --title "My Laptop"

# List SSH keys
gh ssh-key list

# Test SSH connection
ssh -T git@github.com

# Add GPG key to GitHub
gh gpg-key add key.pub

# List GPG keys
gh gpg-key list
```

---

## Undo & Fix Commands

```sh
# Unstage files
git restore --staged file.txt               # unstage specific file
git restore --staged .                      # unstage all
git reset HEAD file.txt                     # older syntax

# Discard local changes
git restore file.txt                        # discard changes in file
git checkout -- file.txt                    # older syntax

# Undo last commit (keep changes staged)
git reset --soft HEAD~1

# Undo last commit (keep changes unstaged)
git reset HEAD~1

# Undo last commit (discard changes) ⚠️
git reset --hard HEAD~1

# Revert a commit (creates new commit)
git revert abc1234
git revert HEAD                             # revert last commit

# Recover deleted branch / lost commits
git reflog                                  # view history of HEAD
git checkout -b recovered-branch abc1234    # restore from reflog

# Clean untracked files
git clean -n                                # dry run (show what would delete)
git clean -f                                # delete untracked files
git clean -fd                               # delete untracked files + dirs

# Fix last commit message
git commit --amend -m "corrected message"

# Add forgotten file to last commit
git add forgotten-file.txt
git commit --amend --no-edit
```

---

## Advanced Commands

### Rebase

```sh
# Rebase current branch onto main
git rebase main

# Interactive rebase (last 3 commits)
git rebase -i HEAD~3

# Continue / abort rebase
git rebase --continue
git rebase --abort
git rebase --skip
```

### Submodules

```sh
# Add a submodule
git submodule add https://github.com/user/repo.git path/to/submodule

# Initialize submodules after clone
git submodule init
git submodule update

# Clone with submodules
git clone --recurse-submodules https://github.com/user/repo.git

# Update all submodules
git submodule update --remote
```

### Worktrees

```sh
# Create a worktree
git worktree add ../feature-branch feature-branch

# List worktrees
git worktree list

# Remove a worktree
git worktree remove ../feature-branch

# Prune stale worktrees
git worktree prune
```

### Bisect (Find Bug-Introducing Commit)

```sh
# Start bisect
git bisect start

# Mark current as bad
git bisect bad

# Mark a known good commit
git bisect good abc1234

# Git will checkout middle commits — mark each:
git bisect good                             # this commit is fine
git bisect bad                              # this commit has the bug

# End bisect
git bisect reset
```

### Config

```sh
# Set user info
git config --global user.name "Your Name"
git config --global user.email "you@example.com"

# Set default branch name
git config --global init.defaultBranch main

# Set default editor
git config --global core.editor "code --wait"

# Enable color output
git config --global color.ui auto

# View all config
git config --list
git config --global --list

# Set aliases
git config --global alias.st "status -sb"
git config --global alias.co "checkout"
git config --global alias.br "branch"

# Credential caching
git config --global credential.helper cache
git config --global credential.helper store  # permanent (plain text)
```

---

## Common Workflows

### Start a New Project

```sh
mkdir my-project && cd my-project
git init
echo "# My Project" > README.md
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/user/repo.git
git push -u origin main
```

### Feature Branch Workflow

```sh
git checkout -b feature/my-feature          # create branch
# ... make changes ...
git add .
git commit -m "add my feature"
git push -u origin feature/my-feature       # push branch
gh pr create --title "Add my feature"       # create PR
```

### Sync Fork with Upstream

```sh
git fetch upstream
git checkout main
git merge upstream/main
git push origin main
```

### Squash Commits Before PR

```sh
git rebase -i HEAD~3                        # squash last 3 commits
# In editor: change "pick" to "squash" for commits to combine
git push --force-with-lease
```

### Resolve Merge Conflicts

```sh
git merge feature-branch                    # may cause conflicts
# ... edit conflicted files ...
git add .                                   # mark as resolved
git commit                                  # complete the merge
```

### Undo a Pushed Commit (Safely)

```sh
git revert HEAD                             # creates a new undo commit
git push
```

### Deploy with Tags

```sh
git tag -a v1.0.0 -m "Release v1.0.0"
git push origin v1.0.0
gh release create v1.0.0 --generate-notes
```

---

---

> **Tip:** Run `gh help <command>` for detailed usage info on any GitHub CLI command.
