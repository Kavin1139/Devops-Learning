# Git – Day 1

## 1. What is Git?

**Git** is a **Distributed Version Control System (DVCS)** used to track changes in files and source code.

### Git helps developers:

- Track the history of changes.
- Save different versions of a project.
- Work on projects with multiple developers.
- Create branches and work on different features.
- Go back to an earlier version when needed.
- Collaborate with other developers using remote repositories such as GitHub.

### Example

Suppose you have a file:

```text
index.html
```

You make changes to it several times. Git can keep track of those changes:

```text
Version 1 → Version 2 → Version 3 → Version 4
```

If something goes wrong in **Version 4**, Git allows you to look at previous versions and restore your work.

---

## 2. Advantages of Git over CVCS

**CVCS** stands for **Centralized Version Control System**.

In a CVCS, there is usually **one central server** that contains the main repository. Developers depend heavily on this central server.

Git is a **Distributed Version Control System (DVCS)**.

In Git, every developer's local repository contains the **project and its history**.

### Advantages of Git

- **Distributed:** Every developer has a complete local copy of the repository.
- **Works offline:** Many Git operations can be performed without an internet connection.
- **Faster:** Most operations happen locally instead of communicating with a central server.
- **Easy branching:** Creating and switching branches is simple.
- **Easy merging:** Changes from different branches can be merged efficiently.
- **Better backup:** Multiple developers may have complete copies of the repository.
- **Good collaboration:** Multiple developers can work on the same project.
- **Complete history:** Git maintains the history of changes made to the project.

### Git vs CVCS

| **Git (DVCS)** | **CVCS** |
|---|---|
| Distributed | Centralized |
| Complete repository exists locally | Main repository is on a central server |
| Many operations work offline | Usually depends on the central server |
| Fast local operations | Operations may require server communication |
| Branching and merging are powerful | Branching can be more restrictive |
| Every clone can contain full history | History is primarily maintained centrally |

---

## 3. Git Workflow

The basic Git workflow is:

```text
Working Directory
       ↓
Staging Area
       ↓
Local Repository
       ↓
Remote Repository
```

### Complete Diagram

```text
┌──────────────────────┐
│   Working Directory  │
│                      │
│  Files you modify    │
└──────────┬───────────┘
           │
           │ git add
           ↓
┌──────────────────────┐
│    Staging Area      │
│                      │
│ Changes selected for │
│ the next commit      │
└──────────┬───────────┘
           │
           │ git commit
           ↓
┌──────────────────────┐
│   Local Repository   │
│                      │
│  Commits stored      │
│     locally          │
└──────────┬───────────┘
           │
           │ git push
           ↓
┌──────────────────────┐
│   Remote Repository  │
│       GitHub         │
│                      │
│  Repository stored   │
│      remotely        │
└──────────────────────┘
```

### 3.1 Working Directory

The **Working Directory** is where you create or modify files in your project.

For example:

```text
Day1.md
README.md
index.html
```

When you edit `Day1.md`, the changes initially exist in the **Working Directory**.

### 3.2 Staging Area

The **Staging Area** contains the changes that you want to include in your next commit.

You move changes from the **Working Directory** to the **Staging Area** using:

```bash
git add
```

Example:

```bash
git add Day1.md
```

### 3.3 Local Repository

The **Local Repository** is the Git repository stored on your computer.

You save staged changes into the **Local Repository** using:

```bash
git commit
```

Example:

```bash
git commit -m "Add Day 1 Git notes"
```

The commit creates a permanent entry in the local Git history.

### 3.4 Remote Repository

A **Remote Repository** is a repository hosted on a remote service such as GitHub.

You can upload your local commits to GitHub using:

```bash
git push
```

Example:

```bash
git push origin main
```

---

## 4. `git add`

### What is `git add`?

The `git add` command is used to move changes from the **Working Directory** to the **Staging Area**.

It tells Git:

> "I want to include these changes in my next commit."

### Syntax

```bash
git add <file-name>
```

### Example

```bash
git add Day1.md
```

This stages only `Day1.md`.

### Stage Multiple Files

```bash
git add Day1.md README.md
```

### Stage All Changed Files

```bash
git add .
```

### Workflow

```text
Working Directory
       │
       │ git add
       ↓
Staging Area
```

> **Important:** `git add` does **not** create a commit.  
> It only prepares the changes for a commit.

---

## 5. `git commit`

### What is `git commit`?

The `git commit` command saves the changes from the **Staging Area** into the **Local Repository**.

A **commit** represents a saved version of your project.

### Syntax

```bash
git commit -m "commit message"
```

### Example

```bash
git commit -m "Add Day 1 Git notes"
```

The message describes what was changed.

### Workflow

```text
Staging Area
      │
      │ git commit
      ↓
Local Repository
```

### Why use commit messages?

A good commit message makes it easy to understand the project history.

**Good example:**

```bash
git commit -m "Add Git workflow notes"
```

**Bad example:**

```bash
git commit -m "changes"
```

---

## 6. `git restore`

### What is `git restore`?

The `git restore` command is used to **discard changes** or restore files to a previous state.

It is commonly used when you have modified a file but decide that you do not want to keep those changes.

### Example

Suppose you modify:

```text
Day1.md
```

You can restore the file:

```bash
git restore Day1.md
```

This removes the **unstaged changes** in `Day1.md` and restores it to the version from the last commit.

### Example Workflow

```text
Last Commit
    ↓
Day1.md
    ↓
You modify the file
    ↓
You don't want the changes
    ↓
git restore Day1.md
    ↓
Changes are discarded
```

> **Important Warning:** Be careful when using `git restore <file>`.  
> Uncommitted changes can be lost.

---

## 7. `git rm --cached`

### What is `git rm --cached`?

The command:

```bash
git rm --cached <file>
```

removes a file from Git's **tracking** while keeping the actual file in your **Working Directory**.

### In simple words

**Git stops tracking the file, but the file remains on your computer.**

### Example

Suppose you accidentally add:

```text
password.txt
```

You can remove it from Git's tracking using:

```bash
git rm --cached password.txt
```

The file will still exist on your computer, but Git will no longer track it.

### Another Example

Suppose you have added a file:

```bash
git add test.txt
```

But you don't want Git to track it anymore:

```bash
git rm --cached test.txt
```

The file remains in your folder.

### Difference between `git rm` and `git rm --cached`

**`git rm`:**

```bash
git rm file.txt
```

Removes the file from **Git and from your Working Directory**.

**`git rm --cached`:**

```bash
git rm --cached file.txt
```

Removes the file from **Git's tracking** but keeps the file in your **Working Directory**.

### Quick Comparison

| Command | Git Tracking | Local File |
|---|---|---|
| `git rm file.txt` | Removed | Removed |
| `git rm --cached file.txt` | Removed | Kept |

---

## 8. `git log`

### What is `git log`?

The `git log` command displays the **commit history** of a Git repository.

It shows information such as:

- **Commit ID (SHA)**
- **Author**
- **Date**
- **Commit message**

### Command

```bash
git log
```

### Example Output

```text
commit 8f32ab12...
Author: John Doe
Date:   Sun Sep 13 2026

    Add Day 1 Git notes

commit 45cd7821...
Author: John Doe
Date:   Sat Sep 12 2026

    Add README file
```

This allows you to see what changes were committed in the project.

### When is `git log` useful?

It is useful when you want to:

- See previous commits.
- Find who made a change.
- Find when a change was made.
- Read previous commit messages.
- Find a commit ID.

---

## 9. `git log --oneline`

### What is `git log --oneline`?

The command:

```bash
git log --oneline
```

shows the commit history in a **short and compact format**.

Instead of showing complete commit information, it displays:

```text
commit-id commit-message
```

### Example Output

```text
8f32ab1 Add Day 1 Git notes
45cd782 Add README file
91ab432 Initial commit
```

The first part is a **shortened commit ID**, followed by the **commit message**.

### Difference between `git log` and `git log --oneline`

| `git log` | `git log --oneline` |
|---|---|
| Shows detailed commit information | Shows compact commit information |
| Shows author and date | Mainly shows commit ID and message |
| More detailed | Easier to read quickly |

### Examples

**Detailed history:**

```bash
git log
```

**Short history:**

```bash
git log --oneline
```

---

## 10. Quick Git Command Summary

| **Command** | **Purpose** |
|---|---|
| `git add` | Moves changes to the Staging Area |
| `git commit` | Saves staged changes to the Local Repository |
| `git restore` | Restores or discards changes in a file |
| `git rm --cached` | Stops tracking a file but keeps it locally |
| `git log` | Shows detailed commit history |
| `git log --oneline` | Shows compact commit history |
| `git push` | Sends local commits to the Remote Repository |

---

## 11. Complete Git Workflow Example

Suppose you create a new file:

```text
Day1.md
```

### Step 1: Create or Modify the File

The file is initially in the:

```text
Working Directory
```

### Step 2: Stage the File

Run:

```bash
git add Day1.md
```

Now:

```text
Working Directory
       ↓
Staging Area
```

### Step 3: Commit the File

Run:

```bash
git commit -m "Add Day 1 Git notes"
```

Now:

```text
Working Directory
       ↓
Staging Area
       ↓
Local Repository
```

### Step 4: Push to GitHub

Run:

```bash
git push origin main
```

Now:

```text
Working Directory
       ↓
Staging Area
       ↓
Local Repository
       ↓
Remote Repository (GitHub)
```

---

## 12. Easy Way to Remember Git Commands

```text
git add
    ↓
"Prepare my changes"

git commit
    ↓
"Save my changes"

git push
    ↓
"Upload my commits to GitHub"

git restore
    ↓
"Discard or restore my changes"

git rm --cached
    ↓
"Stop tracking this file, but keep it"

git log
    ↓
"Show me the commit history"

git log --oneline
    ↓
"Show me the commit history briefly"
```

---

## Conclusion

Git is a **Distributed Version Control System** that helps developers **track, manage, and collaborate** on projects.

### Basic Git Workflow

```text
Working Directory
        ↓
     git add
        ↓
Staging Area
        ↓
   git commit
        ↓
Local Repository
        ↓
    git push
        ↓
Remote Repository (GitHub)
```

Understanding this workflow and the basic Git commands is essential for working effectively with **Git and GitHub**.
