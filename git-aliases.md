# Git Aliases & Commands Reference

A collection of useful Git aliases and commands for everyday use.  
Run the **Setup** block once, then use the short commands anywhere.

---

## Setup — run once

Paste this entire block into your terminal to register all aliases globally.

```sh
git config --global alias.st "status -sb"
git config --global alias.a "add ."
git config --global alias.cm "commit -m"
git config --global alias.cmc "commit -m 'commit changes'"
git config --global alias.pl "pull"
git config --global alias.ps "push"
git config --global alias.co "checkout"
git config --global alias.br "branch"
git config --global alias.m "merge"
git config --global alias.rv "remote -v"
git config --global alias.gl "config --global -l"
git config --global alias.fu "fetch upstream"
git config --global alias.fetchll "fetch --all"
git config --global alias.fetchAll '!git remote update && git status -uno'
git config --global alias.fetchPull '!git fetch && git pull $(git config --get branch.$(git rev-parse --abbrev-ref HEAD).remote) $(git rev-parse --abbrev-ref HEAD)'
git config --global alias.latest '!git log -1 --oneline && git branch --show-current && git describe --tags --abbrev=0 && git describe --tags'
git config --global alias.last-commit "log -1 --pretty=format:'%C(yellow)%h%Creset %C(bold)%s%Creset%n%C(cyan)Branch:%Creset %D%n%C(green)Author:%Creset %an <%ae>%n%C(magenta)Date:%Creset %cd' --date=format:'%Y-%m-%d %H:%M:%S'"
```

---

## Everyday commands

| Command | What it does |
|---|---|
| `git st` | Short status — branch + changed files |
| `git a` | Stage all changed files |
| `git cm "message"` | Commit with a custom message |
| `git cmc` | Commit with generic "commit changes" message |
| `git pl` | Pull from current branch's remote |
| `git ps` | Push to current branch's remote |
| `git co <branch>` | Checkout a branch |
| `git br <name>` | Create a new branch |
| `git m <branch>` | Merge a branch into current |
| `git rv` | List all remotes and their URLs |
| `git gl` | List all global git config values |

---

## Fetch & sync

| Command | What it does |
|---|---|
| `git fetchll` | Fetch from every remote (no merge) |
| `git fetchAll` | Update all remotes + show ahead/behind status |
| `git fetchPull` | Fetch all remotes + pull current branch from its correct remote |
| `git fu` | Fetch from upstream (for forked repos) |

### Details

```sh
git fetchll       # same as: git fetch --all

git fetchAll      # same as: git remote update && git status -uno

git fetchPull     # auto-detects the remote for your current branch,
                  # works even when remotes have different names

git fu            # same as: git fetch upstream
```

---

## Log & history

```sh
git latest        # last commit hash + current branch + latest tag/version

git last-commit   # formatted view:
                  #   hash, message, branch, author, date
```

---

## Team remotes (example)

If your team works on forks, add all team remotes at once with a single alias:

```sh
git config --global alias.addremotes '!git remote add alice https://github.com/alice/project.git \
  && git remote add bob https://github.com/bob/project.git \
  && git remote add upstream https://github.com/org/project.git'
```

Then run:

```sh
git addremotes
```

### Fetch from a specific team member

```sh
git fetch alice                            # fetch only
git pull alice main                        # fetch + merge their main branch
git fetch upstream && git m upstream/main  # sync with upstream
```

---

## Common workflows

### Stage — commit — push

```sh
git a && git cm "your message here" && git ps
```

### Sync with upstream then push

```sh
git fu               # fetch upstream
git m upstream/main  # merge upstream into current branch
git ps               # push your branch
```

### Check status before committing

```sh
git st               # see what's changed
git a                # stage everything
git cm "your message"
git ps
```

### Create a new branch, work, then push

```sh
git co -b feature/my-feature   # create + switch to new branch
git a && git cm "add feature"
git ps
```

---

---

> **Tip:** Customize these aliases to match your workflow. Run `git gl` anytime to review your current aliases.
