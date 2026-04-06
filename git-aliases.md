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

## Remotes — directorist project

### Add all team remotes (run once per repo clone)

```sh
git config --global alias.addremotes '!git remote add arman https://github.com/Armanul46/directorist.git \
  && git remote add galib https://github.com/syedgalib/directorist \
  && git remote add mehedi https://github.com/HeyMehedi/directorist \
  && git remote add nazmul https://github.com/nazmulhasan103/directorist.git \
  && git remote add obi https://github.com/obiPlabon/directorist \
  && git remote add origin https://github.com/RabbiIslamRony/directorist.git \
  && git remote add rafiq https://github.com/vairafiq/directorist \
  && git remote add riaz https://github.com/iamibrahimriaz/directorist/ \
  && git remote add shamim https://github.com/devShamim/directorist \
  && git remote add sovware https://github.com/sovware/directorist.git \
  && git remote add tanjim https://github.com/tanjimhasan/directorist \
  && git remote add upstream https://github.com/sovware/directorist.git \
  && git remote add github-desktop-woadudakand https://github.com/woadudakand/directorist.git'
```

Then run:

```sh
git addremotes
```

### Fetch from a specific team member

```sh
git fetch mehedi                           # fetch only
git pull mehedi main                       # fetch + merge their main branch
git fetch upstream && git m upstream/main  # sync with upstream
```

---

## Clone — sovware repos

### Themes

```sh
git clone https://github.com/sovware/wpwax-theme.git \
  && git clone https://github.com/sovware/formgent-theme.git \
  && git clone https://github.com/sovware/dir-theme.git
```

### Listing themes (SSH)

```sh
for repo in dhotels dclassified onelisting dplace drestaurant drealestate dcar dlist directoria ddoctors dlawyers djobs dcourse direo dservice best-listing; do
  git clone git@github.com:sovware/$repo.git
done
```

### Directorist core + extensions (SSH)

```sh
git clone git@github.com:sovware/directorist.git \
  && git clone git@github.com:sovware/directorist-adverts-manager.git \
  && git clone git@github.com:sovware/directorist-booking.git \
  && git clone git@github.com:sovware/directorist-business-hours.git \
  && git clone git@github.com:sovware/directorist-claim-listing.git \
  && git clone git@github.com:sovware/directorist-compare-listing.git \
  && git clone git@github.com:sovware/directorist-coupon.git \
  && git clone git@github.com:sovware/directorist-faqs.git \
  && git clone git@github.com:sovware/directorist-live-chat.git \
  && git clone git@github.com:sovware/directorist-mark-as-sold.git \
  && git clone git@github.com:sovware/directorist-paypal.git \
  && git clone git@github.com:sovware/directorist-post-your-need.git \
  && git clone git@github.com:sovware/directorist-pricing-plans.git \
  && git clone git@github.com:sovware/directorist-rank-featured-listings.git \
  && git clone git@github.com:sovware/directorist-social-login.git \
  && git clone git@github.com:sovware/directorist-stripe.git \
  && git clone git@github.com:sovware/directorist-google-recaptcha.git \
  && git clone git@github.com:sovware/directorist-listings-with-map.git \
  && git clone git@github.com:sovware/directorist-search-popup-block \
  && git clone git@github.com:sovware/directorist-account-block
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

*Maintained by Rony — last updated 2026*
