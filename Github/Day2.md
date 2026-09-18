# Day 02 - Advanced Git & GitHub Management

## 1. Git Reset

`git reset` is used to move the current branch to an earlier commit.

There are two commonly used types:

### Soft Reset

A **soft reset** moves the branch pointer to an earlier commit but keeps the changes in the **staging area**.

```bash
git reset --soft HEAD~1
```

Example:

```text
Before:

A --- B --- C (HEAD)

After git reset --soft HEAD~1:

A --- B (HEAD)

Changes from C are still staged.
```

Use soft reset when you want to undo a commit but keep its changes so that you can modify or recommit them.

### Hard Reset

A **hard reset** moves the branch pointer to an earlier commit and discards the changes from the commits being removed.

```bash
git reset --hard HEAD~1
```

Example:

```text
Before:

A --- B --- C (HEAD)

After git reset --hard HEAD~1:

A --- B (HEAD)
```

The changes from commit `C` are removed from the working directory and staging area.

> **Warning:** Use `git reset --hard` carefully because uncommitted changes can be permanently lost.

---

## 2. Git Revert

`git revert` creates a **new commit** that reverses the changes made by an earlier commit.

It does not remove the original commit from the history.

### Example

First check the commit history:

```bash
git log --oneline
```

Suppose the commit is:

```text
abc1234 Add unwanted change
```

Revert it:

```bash
git revert abc1234
```

Git creates a new commit that undoes the changes from `abc1234`.

The history becomes:

```text
A --- B --- C --- D
              ↑
         revert commit
```

`C` still exists, but `D` reverses its changes.

---

## 3. Reset vs Revert

| Git Reset                                          | Git Revert                                   |
| -------------------------------------------------- | -------------------------------------------- |
| Moves the branch pointer                           | Creates a new commit                         |
| Can change existing history                        | Preserves existing history                   |
| Useful for local/private work                      | Useful for shared/public branches            |
| Can remove commits from the visible branch history | Does not remove the original commit          |
| `git reset --soft` keeps changes staged            | Revert creates a commit containing the undo  |
| `git reset --hard` can discard changes             | Does not normally delete the original commit |

### When to use Reset

Use reset when:

* Working on a local/private branch.
* You want to remove or modify recent commits.
* You have not shared the commits with other developers.

Example:

```bash
git reset --soft HEAD~1
```

### When to use Revert

Use revert when:

* The commit has already been pushed.
* Other developers are working with the branch.
* You want to undo a change without rewriting history.

Example:

```bash
git revert <commit-id>
```

---

## 4. Squash Commit

**Squashing** means combining multiple commits into a single commit.

It is useful for cleaning up commit history before merging changes.

### Example

Suppose the history is:

```text
A --- B --- C --- D
         ↑
     multiple changes
```

We can combine `B`, `C`, and `D` into one commit.

Use interactive rebase:

```bash
git rebase -i HEAD~3
```

An editor will open:

```text
pick abc111 First change
pick abc222 Second change
pick abc333 Third change
```

Change it to:

```text
pick abc111 First change
squash abc222 Second change
squash abc333 Third change
```

Save and exit.

Git combines the three commits into one commit.

The history becomes:

```text
A --- E
      ↑
   combined commit
```

---

## 5. Git Cherry Pick

`git cherry-pick` applies the changes from a **specific commit** to the current branch.

It is useful when you need one particular change from another branch without merging the entire branch.

### Example

Suppose:

```text
main:    A --- B

feature: A --- B --- C --- D
```

If we only need commit `C` in `main`:

```bash
git checkout main
git cherry-pick <commit-id-of-C>
```

Now:

```text
main:    A --- B --- C'
```

The changes from `C` are applied to `main` as a new commit.

---

## 6. Git Rebase

`git rebase` moves or reapplies commits on top of another branch.

It creates a cleaner, linear history.

### Example

Before rebase:

```text
A --- B --- C        main
       \
        D --- E      feature
```

Run:

```bash
git checkout feature
git rebase main
```

After rebase:

```text
A --- B --- C --- D' --- E'    feature
```

The feature commits are reapplied on top of the latest `main`.

### Basic command

```bash
git rebase main
```

Rebase is also used for:

* Cleaning commit history.
* Squashing commits.
* Updating a feature branch with the latest changes from another branch.

---

## 7. Git Merge

`git merge` combines the changes from one branch into another branch.

### Example

Suppose:

```text
A --- B --- C        main
       \
        D --- E      feature
```

To merge `feature` into `main`:

```bash
git checkout main
git merge feature
```

Git combines the changes.

Depending on the history, Git may create a merge commit:

```text
A --- B --- C -------- F    main
       \              /
        D --- E ------      feature
```

Here, `F` is the merge commit.

---

## 8. Rebase vs Merge

| Rebase                                     | Merge                                   |
| ------------------------------------------ | --------------------------------------- |
| Reapplies commits on top of another branch | Combines two branches                   |
| Produces a linear history                  | Can create a merge commit               |
| Rewrites commit history                    | Preserves existing history              |
| Useful for cleaning local feature branches | Useful for integrating shared branches  |
| Can make history easier to read            | Clearly shows that branches were merged |

### When to use Rebase

Use rebase when:

* Working on your own feature branch.
* You want a clean, linear history.
* You have not shared the branch or are sure rewriting history is safe.

Example:

```bash
git checkout feature
git rebase main
```

### When to use Merge

Use merge when:

* Working with shared branches.
* You want to preserve the branch history.
* You want to combine completed work into another branch.

Example:

```bash
git checkout main
git merge feature
```

> Avoid rebasing shared branches unless the team has agreed to it, because rebase changes commit history.

---

# GitHub Repo Management

## 1. Repository Creation

A GitHub repository is a remote location used to store and collaborate on Git projects.

Basic steps:

1. Sign in to GitHub.
2. Select **New repository**.
3. Enter the repository name.
4. Choose repository visibility.
5. Create the repository.
6. Connect the local Git repository to GitHub.

Example:

```bash
git remote add origin git@github.com:USERNAME/REPOSITORY.git
```

Push the branch:

```bash
git push -u origin main
```

---

## 2. Teams Access and Permission Management

GitHub allows repository owners to give access to other users and teams.

Common repository permissions include:

* **Read** - View and clone the repository.
* **Triage** - Manage issues and pull requests without pushing code.
* **Write** - Push code and make changes.
* **Maintain** - Manage repository settings and maintenance tasks without full administrative access.
* **Admin** - Full repository administration.

Teams can be created in a GitHub organization and given access to repositories.

This makes it easier to manage permissions for multiple developers.

---

## 3. Branch Rules / Branch Protection

Branch protection rules help prevent unwanted changes to important branches such as `main`.

Common rules include:

* Require pull requests before merging.
* Require approvals.
* Require conversation resolution.
* Require status checks.
* Restrict direct pushes.
* Require CODEOWNERS review.
* Prevent force pushes.

Example workflow:

```text
Developer
    |
    v
Feature Branch
    |
    v
Pull Request
    |
    v
Review + Checks
    |
    v
Main Branch
```

These rules help maintain code quality and controlled changes.

---

## 4. CODEOWNERS

A `CODEOWNERS` file defines which users or teams are responsible for reviewing specific parts of a repository.

The file is commonly placed in:

```text
.github/CODEOWNERS
```

Example:

```text
*.js @developer-team
/docs/ @documentation-team
```

This means:

* JavaScript files require review from `@developer-team`.
* Files under `/docs/` require review from `@documentation-team`.

CODEOWNERS can be used with branch protection to require approval from the appropriate owners.

---

# SSH Key Setup

SSH allows Git to authenticate with GitHub using an SSH key pair.

The key pair contains:

```text
Private Key  → Stored on your computer
Public Key   → Added to GitHub
```

The private key should never be shared.

## Step 1: Generate an SSH Key

Use:

```bash
ssh-keygen -t ed25519 -C "your-email@example.com"
```

Press Enter to accept the default file location.

Example:

```text
~/.ssh/id_ed25519
~/.ssh/id_ed25519.pub
```

## Step 2: Start SSH Agent

```bash
eval "$(ssh-agent -s)"
```

## Step 3: Add the Private Key

```bash
ssh-add ~/.ssh/id_ed25519
```

## Step 4: Copy the Public Key

```bash
cat ~/.ssh/id_ed25519.pub
```

Copy the complete output.

## Step 5: Add Key to GitHub

In GitHub:

```text
Settings
   ↓
SSH and GPG keys
   ↓
New SSH key
   ↓
Paste public key
   ↓
Add SSH key
```

## Step 6: Test SSH Connection

```bash
ssh -T git@github.com
```

A successful authentication message confirms that SSH authentication is working.

## Clone a Repository Using SSH

```bash
git clone git@github.com:USERNAME/REPOSITORY.git
```

Example:

```bash
git clone git@github.com:Kavin1139/Learn_Linux.git
```

---

# Webhooks

A **webhook** is an HTTP request sent automatically when a specific event happens in a system.

For example:

```text
GitHub
   |
   | Push Event
   v
Webhook
   |
   | HTTP Request
   v
Jenkins / External System
   |
   v
CI/CD Pipeline
```

## How Webhooks Work

1. An event happens in GitHub.
2. GitHub sends an HTTP request to the configured webhook URL.
3. The receiving application gets the event information.
4. The receiving application performs an action based on that event.

Example:

```text
Developer
    |
    | git push
    v
GitHub
    |
    | Webhook
    v
Jenkins
    |
    | Build + Test + Deploy
    v
Application
```

## Uses of Webhooks

### CI/CD

A GitHub push can trigger a Jenkins pipeline or another CI/CD system.

### Notifications

Webhooks can send notifications to external applications when events occur.

### Integrations

Webhooks can connect GitHub with external systems and automation tools.

Examples include:

* Jenkins
* Chat/notification systems
* Deployment platforms
* Monitoring systems
* Other external applications

> A webhook sends the event notification. The receiving system decides what action to perform.

---

# Summary

The important Git commands covered in Day 02 are:

```bash
# Reset
git reset --soft HEAD~1
git reset --hard HEAD~1

# Revert
git revert <commit-id>

# Squash
git rebase -i HEAD~3

# Cherry-pick
git cherry-pick <commit-id>

# Rebase
git rebase main

# Merge
git merge feature

# SSH
ssh-keygen -t ed25519 -C "your-email@example.com"
ssh-add ~/.ssh/id_ed25519
ssh -T git@github.com

# Clone using SSH
git clone git@github.com:USERNAME/REPOSITORY.git
```

These commands and GitHub features help developers manage code history, collaborate safely, control repository access, and automate development workflows.

