# GitHub for Vibe Coders

### A Hands-On Lab

> **Format:** Part homework (solo), part real-time collaboration (in class)
> **Group size:** 4 people
> **Goal:** Get comfortable using GitHub the way real teams do - including when things go wrong. Things will go wrong.

Before you start, assign roles within your group. The roles just determine who drives during the in-class exercises - everyone does the same amount of work.

| Role     | Who |
| -------- | --- |
| Person A |     |
| Person B |     |
| Person C |     |
| Person D |     |

---

## Before You Start - Setup and Survival Kit

### How to Read the Commands in This Lab

Throughout this lab you'll see terminal commands that look like this:

```bash
git clone <paste-your-url-here>
```

**The angle brackets `< >` mean "replace this with your own value."** Don't type the angle brackets themselves. For example, the clone command for this class is:

```bash
git clone https://github.com/nickdegiacomo/bard-mba-spring-2026.git
```

Same idea with square brackets in commit messages:

```bash
git commit -m "Add [your name] intro profile"
```

If your name is Kenji, you'd type:

```bash
git commit -m "Add Kenji intro profile"
```

Some words in commands are *not* placeholders even though they might look like it. The word `origin` is one of these - it's a real Git keyword that means "the version of this project on GitHub." You always type it literally. When in doubt, this lab will tell you.

---

### Step 0 - Install Git and Set Up VS Code's Terminal

You all have VS Code installed. Good. You'll run every Git command from VS Code's built-in terminal. No separate app needed.

**A quick note on terminals:** The terminal is the text panel at the bottom of VS Code where you type commands. It's not the same as the main editing area. You type a command, press Enter, and the computer does something. If it works, it often says nothing at all, which feels unsettling at first but is actually the system telling you everything is fine.

**Windows users:**

Git doesn't ship with Windows. You need to install it.

1. Download Git for Windows: https://git-scm.com/download/win
2. Run the installer. Keep clicking Next and accept all defaults. This is one of the rare times in life where the defaults are fine.
3. Open VS Code.
4. Open the terminal: click **Terminal** in the menu bar, then **New Terminal** (or press Ctrl+\`).
5. Click the dropdown arrow on the right side of the terminal panel and select **Git Bash**.
6. Type `git --version` and press Enter. You should see something like `git version 2.x.x`.

If you see `'git' is not recognized`, you're in PowerShell, not Git Bash. Use the dropdown to switch.

If your machine won't let you run the installer (no admin rights), try the "Portable" edition from the same download page - it doesn't require admin. If that also fails, ask me before class and we'll sort it out. There is always a way.

**Mac users:**

1. Open VS Code.
2. Open the terminal: click **Terminal** in the menu bar, then **New Terminal** (or press Ctrl+\`).
3. Type `git --version` and press Enter.
4. If Git isn't installed yet, macOS will pop up a dialog offering to install developer tools. Say yes. Let it finish. Re-run the command.

---

### VS Code Tips for This Lab

VS Code has visual Git tools built in. Use them as a safety net when the terminal feels like reading Heidegger in the original German:

- **Source Control panel** (branch icon on the left sidebar) - shows your changed files. You can stage and commit by clicking instead of typing commands.
- **Merge conflict editor** - when you hit a conflict, VS Code highlights the problem with color-coded buttons: Accept Current Change, Accept Incoming Change, Accept Both Changes. Much friendlier than raw conflict markers.
- **Explorer panel** - right-click to create new files and folders if you'd rather not use the terminal for that.

Try the terminal commands first. But know the visual tools are there when you need them.

---

### AI Tools for This Lab

You all have Claude Code or the OpenAI Codex extension in VS Code. Use them. That's the whole point of this class - you're vibe coding, not memorizing syntax. When a step says "use AI to generate the contents," use whichever tool you have installed. When a Git command blows up in your face, paste the error into your AI tool and ask what happened. It will almost certainly know.

---

### GitHub Survival Kit

You'll use these commands throughout the lab. Come back to this section whenever you forget something, which will be often.

```bash
git clone <url>              # Download a repo to your computer
git status                   # Check what's changed (run this constantly)
git add .                    # Stage ALL your changes (the dot means "everything")
git commit -m "message"      # Save a snapshot with a description
git push                     # Upload your changes to GitHub
git pull                     # Download the latest changes from GitHub
git checkout -b <branch>     # Create a new branch and switch to it
git checkout <branch>        # Switch to an existing branch
git branch                   # See all branches (asterisk = where you are)
git fetch                    # Check for new branches on GitHub
```

A note on `git add .` - the dot is not a typo. It means "stage everything I changed." There are ways to be more selective, but for this lab, the dot is all you need.

**What's a branch?** Think of it like a parallel version of the project where you can experiment without breaking the main version. When you're done, you merge it back in. Like a Borges garden of forking paths, except you actually want the paths to converge again.

**What's a PR?** A Pull Request is a formal request to merge your branch into `main`. It lets teammates review your work before it goes live. It's a quality gate, not a suggestion box.

---

## Part 1 - Solo Homework (Do Before Class)

**Estimated time:** 45-60 min
**Do this on your own, before the in-class session.**

---

### Step 1 - Configure Git and Authenticate with GitHub

Open VS Code, then open the terminal (Ctrl+\`). Windows users - confirm the terminal shows Git Bash, not PowerShell.

Tell Git who you are. Use the same email address as your GitHub account:

```bash
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
```

Replace `Your Name` and `your@email.com` with your actual name and email. Keep the quotes.

Now run these two housekeeping commands. They prevent problems later that are annoying to debug:

**Windows users:**

```bash
git config --global core.autocrlf true
git config --global core.editor "code --wait"
```

**Mac users:**

```bash
git config --global core.autocrlf input
git config --global core.editor "code --wait"
```

The first command fixes a line-ending mismatch between Windows and Mac that would otherwise make Git think entire files changed when only one line did. The second tells Git to use VS Code as its text editor instead of a terminal-based editor called Vim, which is notoriously difficult to exit. People have mass-posted about this online. We're skipping that particular rite of passage.

**Set up authentication** so GitHub accepts your pushes. The easiest way is to let VS Code handle it:

1. In VS Code, open the Command Palette: press Ctrl+Shift+P (Windows) or Cmd+Shift+P (Mac).
2. Type `GitHub: Sign In` and select it.
3. VS Code will open your browser. Log in to GitHub and authorize VS Code when prompted.
4. Once you see a success message, you're done. VS Code will handle authentication from here.

If the browser sign-in doesn't work for some reason, there's a manual fallback:

1. On GitHub.com, click your profile photo (top right), then **Settings**.
2. Scroll down the left sidebar to **Developer settings**, then **Personal access tokens**, then **Tokens (classic)**.
3. Click **Generate new token (classic)**.
4. Name it `VS Code lab`. Set expiration to 90 days. Check the **repo** checkbox (this gives the token permission to push code).
5. Click **Generate token**. Copy it immediately. GitHub shows it to you exactly once, like a Pavement lyric you overhear at a party and can never quite remember again.
6. Store the token somewhere safe. When you run `git push` later and it asks for a password, paste this token.

---

### Step 2 - Clone the Class Repo

You've been added as a collaborator on the class repo. You should have received a GitHub email invitation - accept it first.

1. Go to the class repo: https://github.com/nickdegiacomo/bard-mba-spring-2026

2. Click the green **Code** button and copy the **HTTPS** URL.

3. In your VS Code terminal, first navigate to your Desktop so the repo ends up somewhere you can find it:

```bash
cd ~/Desktop
```

4. Now clone the repo:

```bash
git clone https://github.com/nickdegiacomo/bard-mba-spring-2026.git
```

5. Move into the repo folder:

```bash
cd bard-mba-spring-2026
```

6. Now open this folder in VS Code properly: click **File > Open Folder**, navigate to **Desktop > bard-mba-spring-2026**, and select it. This is important - it makes VS Code's Explorer panel and Source Control panel actually show your project files. If you skip this step, you'll be working blind.

**Check that it worked:** Open a new terminal in VS Code (Terminal > New Terminal). Type `git status`. You should see something like `On branch main / Your branch is up to date`. If you see `fatal: not a git repository`, you're in the wrong folder - re-do step 6.

---

### Step 3 - Create Your Own Branch

Never work directly on `main`. This is the one commandment. Create a branch named after yourself:

```bash
git checkout -b yourname/intro
```

Replace `yourname` with your actual name. No spaces. For example, if your name is Maria Santos:

```bash
git checkout -b maria/intro
```

Some more examples so the format is clear:

```bash
git checkout -b kenji/intro
git checkout -b priya/intro
git checkout -b alex/intro
```

**Check that it worked:** Run `git branch`. You should see two branches - `main` and your new branch. The asterisk marks the one you're on:

```
  main
* maria/intro
```

If the asterisk is next to `main`, run the checkout command again.

---

### Step 4 - Add Your Intro File

In the VS Code Explorer panel (left sidebar, top icon), open the `students/` folder. Right-click inside it and select **New File**. Name it `yourname.md` - for example, `maria.md` or `kenji.md`.

Use your AI tool (Claude Code or Codex) to write the content. Try a prompt like this:

> "Write a short markdown profile for an MBA student named [your name]. Include their background, one data or coding goal for this class, and a fun fact. Keep it under 150 words."

Paste the result into your file. Tweak anything that sounds wrong or too earnest. Save the file (Ctrl+S or Cmd+S).

---

### Step 5 - Commit and Push

Back in the terminal. First, check what Git sees:

```bash
git status
```

You should see your new file listed in red under "Untracked files." That's normal - it means Git knows the file exists but hasn't saved it yet.

Now stage, commit, and push. Replace the branch name and commit message with your own:

```bash
git add .
git commit -m "Add Maria intro profile"
git push origin maria/intro
```

The word `origin` is not a placeholder. Type it literally. It means "send this to GitHub." The branch name after it *is* a placeholder - use your actual branch name from Step 3.

If this is your first push and VS Code didn't handle authentication earlier, GitHub will ask you to log in. Use your GitHub username and paste the personal access token (from Step 1) as the password. The password field won't show any characters as you paste - that's a security feature, not a bug.

**Check that it worked:** Go to GitHub.com, open the class repo, and click the **branches** dropdown near the top left. You should see your branch listed. Click on it and confirm your file is there.

If `git push` gave you an error instead, run `git status` and read what it says. If that doesn't help, paste the full error message into your AI tool. Debugging is not a personal failing - it's the job.

---

### Step 6 - Open a Pull Request

1. Go to the class repo on GitHub.com.
2. You'll likely see a yellow banner near the top that says something like "maria/intro had recent pushes - Compare & pull request." Click it. (If you don't see the banner, click the **Pull Requests** tab, then **New pull request**, and set the "compare" dropdown to your branch.)
3. Make sure the base branch says `main`.
4. Title your PR: `Add [Your Name] intro profile` (for example, `Add Maria intro profile`).
5. Write a sentence or two in the description about what you added.
6. Click **Create pull request**.

**Homework complete.** Don't merge it yet. You'll review each other's PRs in class.

---

### If Something Went Wrong

Some common problems and their fixes:

**"I committed with the wrong message."** Don't worry about it. The message is just a label. Move on. (If it really bothers you, ask your AI tool about `git commit --amend` - but it's not worth stressing over for this lab.)

**"I made changes on `main` instead of my branch."** If you haven't committed yet, run `git stash`, then `git checkout -b yourname/intro`, then `git stash pop`. This moves your uncommitted changes to the correct branch. If you already committed to main, ask your AI tool for help - it's fixable but the steps depend on your exact situation. Also, don't worry - main is protected, so you won't be able to push to it directly anyway. GitHub will reject the push and tell you to use a branch.

**"My terminal says I'm not in a git repository."** You're in the wrong folder. Run `pwd` to see where you are, then `cd` into the repo folder. Or just re-do Step 2, item 6 (File > Open Folder).

**"I closed VS Code and now everything looks different."** Re-open the repo folder: File > Open Folder > navigate to Desktop > bard-mba-spring-2026. Then open a new terminal. Windows users, switch to Git Bash.

**"A weird text editor opened in my terminal and I can't get out."** That's Vim. Type `:wq` and press Enter to escape. If you ran the config commands in Step 1, this shouldn't happen, but Vim is like a stray cat - it shows up when you least expect it.

---

## Part 2 - In-Class Collaboration

**Do this live, together.**
One person drives at a time. Rotate who shares their screen.

---

### Exercise A - Review a Teammate's PR

Each person reviews one other person's PR. Assign it round-robin:

| You      | Review             |
| -------- | ------------------ |
| Person A | Person B's PR      |
| Person B | Person C's PR      |
| Person C | Person D's PR      |
| Person D | Person A's PR      |

How to do a review:

1. Go to the class repo on GitHub and click the **Pull Requests** tab.
2. Open your assigned PR.
3. Click **Files changed** to see what they added.
4. Leave at least one comment. A question, a suggestion, a compliment, a Sonic Youth reference - anything that shows you read it.
5. Click **Review changes** (green button, top right of the diff view), choose **Approve**, and click **Submit review**.

After everyone has been approved, each person merges their own PR. Click **Merge pull request**, then **Confirm merge**.

The goal here is learning the workflow, not critiquing the prose.

---

### Exercise B - Create a Shared Branch

Now everyone works on the same branch. This is where collaboration gets real.

First, everyone needs to get the latest version of `main` (which now includes all four intro profiles from Exercise A):

**Person A** drives (share your screen):

```bash
git checkout main
git pull
```

`git pull` downloads everyone's merged changes. Your teammates' intro files should now appear in the `students/` folder.

Now create the shared branch:

```bash
git checkout -b team/data-analysis
git push origin team/data-analysis
```

**Everyone else** runs:

```bash
git checkout main
git pull
git fetch
git checkout team/data-analysis
```

`git fetch` tells your local Git about new branches on GitHub. `git checkout team/data-analysis` switches you onto the shared branch. You're all on the same track now.

**Check that it worked:** Everyone runs `git branch`. You should all see the asterisk next to `team/data-analysis`.

---

### Exercise C - Each Person Adds a File

There's an `analysis/` folder in the repo. Each person creates one file in it.

Use your AI tool to generate the contents. Decide among yourselves who takes which file:

| File                          | Prompt to use |
| ----------------------------- | ------------- |
| `analysis/top_customers.sql`  | "Write a SQL query that finds the top 5 customers by total revenue from a table called `orders` with columns: customer_id, amount, date." |
| `analysis/revenue_growth.sql` | "Write a SQL query that calculates month-over-month revenue growth using a `sales` table with columns: month, revenue." |
| `analysis/sales_summary.py`   | "Write a Python script using pandas that loads a CSV called sales_data.csv and prints a summary: total revenue, average order value, and best month." |
| `analysis/sales_chart.py`     | "Write a Python script that generates a bar chart of monthly sales using matplotlib. Use sample data." |

Create your file in the `analysis/` folder (right-click in Explorer > New File), paste in the AI-generated code, and save.

Then commit and push. Here's what it looks like if you picked the customers query:

```bash
git add .
git commit -m "Add top customers SQL query"
git push origin team/data-analysis
```

**Go one at a time.** If two people push simultaneously, the second person will get a rejection. That's not a disaster - just run `git pull` first, then try pushing again - but we're saving the real conflict experience for the next exercise.

**Check that it worked:** After you push, go to GitHub.com, navigate to the repo, switch to the `team/data-analysis` branch using the dropdown, and confirm your file is in the `analysis/` folder.

---

### Exercise D - Survive a Merge Conflict

Time to break something on purpose.

**Setup - Person A** creates a file called `analysis/README.md` with this content:

```markdown
# Analysis Scripts

A collection of SQL queries and Python scripts for sales data analysis.
```

Commit and push it:

```bash
git add .
git commit -m "Add analysis README"
git push origin team/data-analysis
```

Everyone else pulls to get the new file:

```bash
git pull origin team/data-analysis
```

**Now the fun part.** Person C and Person D both open `analysis/README.md` and each add a line at the end describing their script. Don't coordinate. Don't look at what the other person writes. This is the Git equivalent of two people trying to edit the same cell in a spreadsheet at the same time, except Git handles it with more drama.

**Person C** saves, then pushes first:

```bash
git add .
git commit -m "Add script description to README"
git push origin team/data-analysis
```

**Person D** saves, then tries to push:

```bash
git add .
git commit -m "Add script description to README"
git push origin team/data-analysis
```

Person D's push gets rejected. This is expected. This is fine. This is the moment Camus was talking about - we must imagine Sisyphus happy.

**This next part is the whole point of the exercise. Don't skip any of it.**

Person D needs to pull the latest version from GitHub so Git can show where the conflict is:

```bash
git pull origin team/data-analysis
```

Git will mark the conflict inside the file. It looks like this:

```
<<<<<<< HEAD
Person D's line here
=======
Person C's line here
>>>>>>> origin/team/data-analysis
```

This looks alarming. It's not. The `<<<<<<<` and `>>>>>>>` markers are just Git's way of saying "here are the two versions - you decide what to keep." Everything between `<<<<<<< HEAD` and `=======` is your version. Everything between `=======` and `>>>>>>>` is what's on GitHub.

In VS Code, this looks much better - you'll see colored highlights with buttons. Click **Accept Both Changes** to keep both lines. That's the easiest option and the right one here.

Or do it manually: delete the `<<<<<<<`, `=======`, and `>>>>>>>` markers, keep both people's text, and save.

Now you need to tell Git you've fixed the conflict. This part is important - the conflict isn't resolved until you do all three of these:

```bash
git add .
git commit -m "Resolve merge conflict - keep both README descriptions"
git push origin team/data-analysis
```

If you only do `git add` and `git commit` but forget `git push`, the fix is only on your computer. If you only do `git add` but forget `git commit`, Git is still in a conflicted state. Do all three.

**Check that it worked:** Go to GitHub.com, look at `analysis/README.md` on the `team/data-analysis` branch. Both descriptions should be there.

Conflict resolved. This happens to every developer, every week. It's not a crisis. It's just Tuesday.

---

### Exercise E - Read a Teammate's Code with AI

Pick a teammate's file from the `analysis/` folder - ideally one in a language you don't know well. Open it in VS Code and use your AI tool to understand it.

Try prompts like:

> "Explain what this code does, line by line."

> "What would I need to change to make this work with a different table name?"

> "Are there any bugs or edge cases in this code?"

Leave a comment on the file as a GitHub code review comment, or just discuss it with your team. The point here is that reading code is a skill, and AI makes it accessible even if you've never written SQL or Python before. Like having a patient translator who never judges your pronunciation.

---

### Exercise F - Open the Final PR and Merge

**Person B** opens the PR (share your screen):

1. Go to the class repo. Click **Pull Requests**, then **New pull request**.
2. Set base to `main` and compare to `team/data-analysis`.
3. Title it: `Team data analysis - SQL and Python scripts`
4. Add all four teammates as **Reviewers**.
5. Everyone clicks **Approve**.
6. **Person A** clicks **Merge pull request**, then **Confirm merge**.

The team's work is now in `main`. That's the whole workflow. Everything else is variations on this theme.

---

## What You Just Did

| Skill                          | Where           |
| ------------------------------ | --------------- |
| Open the VS Code terminal      | Step 0          |
| Configure Git                  | Step 1          |
| Clone a repo                   | Step 2          |
| Create a branch                | Step 3          |
| Commit and push                | Step 5          |
| Open a pull request            | Step 6          |
| Review a PR                    | Exercise A      |
| Collaborate on a shared branch | Exercises B - C |
| Resolve a merge conflict       | Exercise D      |
| Read code with AI              | Exercise E      |
| Merge a PR as a team           | Exercise F      |

---

## Quick Reference Card

```bash
git clone <url>              # Download a repo (<url> = your actual URL)
git checkout -b <branch>     # Create + switch to new branch (<branch> = your name)
git status                   # What's changed? (run this when confused)
git add .                    # Stage all changes (the dot means "everything")
git commit -m "message"      # Save a snapshot
git push origin <branch>     # Upload to GitHub (origin = GitHub, not a placeholder)
git pull                     # Download latest from GitHub
git fetch                    # Check for new branches on GitHub
git branch                   # List all branches (* = where you are)
```

**When confused:** Run `git status` first. It almost always tells you what to do next.

**When really confused:** Copy the full error message and paste it into Claude Code or Codex. It will tell you exactly what to run. That's not cheating.
