# SLAM Skills

**Shared AI skills for the Supply Chain and Advanced Manufacturing (SLAM) vertical at Plug and Play.**

Every skill here follows the open [Agent Skills](https://agentskills.io) standard, so the same folder works in every tool listed below. Write it once, use it everywhere.

> **Internal use only.** This repo is private to Plug and Play. Do not fork it to a personal account, copy it to a public repo or share skill files outside the company.

---

## Contents

1. [What's in this repo](#whats-in-this-repo)
2. [Install the skills](#install-the-skills)
   - [Claude (desktop and web)](#claude-desktop-and-web)
   - [ChatGPT (desktop)](#chatgpt-desktop)
   - [Gemini Enterprise](#gemini-enterprise)
   - [Coding agents (Claude Code, Codex, Gemini CLI, Cursor and others)](#coding-agents)
3. [Keep your skills up to date](#keep-your-skills-up-to-date)
4. [Contribute a change](#contribute-a-change)
   - [Option A: in your browser, no Git needed](#option-a-in-your-browser-no-git-needed)
   - [Option B: from the command line](#option-b-from-the-command-line)
   - [What happens after you open a pull request](#what-happens-after-you-open-a-pull-request)
5. [How to write a good skill](#how-to-write-a-good-skill)
6. [What never goes in this repo](#what-never-goes-in-this-repo)
7. [Troubleshooting](#troubleshooting)
8. [Questions and owner](#questions-and-owner)

---

## What's in this repo

```
slam-skills/
├── README.md                  ← you are here
├── CONTRIBUTING.md            ← short version of the contribution rules
├── .github/
│   ├── CODEOWNERS             ← routes every pull request to the repo owner
│   └── pull_request_template.md
├── templates/
│   └── SKILL_TEMPLATE.md      ← copy this to start a new skill
└── skills/
    ├── plug-and-play-deck/
    │   ├── SKILL.md           ← required: name, description and instructions
    │   ├── assets/            ← optional: templates, logos, images
    │   ├── references/        ← optional: longer docs the skill can read
    │   └── scripts/           ← optional: code the skill can run
    ├── plug-and-play-report/
    └── ...

---

## Install the skills

Pick the tool you use. You can install into more than one.

Every chat app installs skills from a **.zip file**, one zip per skill. The easiest way to get them is the **Releases** page of this repo, where an up-to-date zip of every skill is attached. If there is no release yet, use the commands under [Build the zips yourself](#build-the-zips-yourself).

> ⚠️ **Claude and Gemini Enterprise expect different zip layouts.** Claude wants the skill folder *inside* the zip. Gemini Enterprise wants `SKILL.md` at the *top level* of the zip. Grab the right file for your tool (`skill-name.zip` for Claude and ChatGPT, `skill-name-gemini.zip` for Gemini Enterprise).

### Claude (desktop and web)

Skills you add on the web or in the desktop app show up in both, and they also sync into Claude Code when you sign in with the same account.

1. Make sure skills are turned on. On a Team or Enterprise plan your org owner controls this. On an individual plan, check **Settings > Capabilities** and confirm code execution and file creation is enabled.
2. Go to **Customize > Skills**.
3. Click **+**, then **Create skill**, then **Upload a skill**.
4. Choose the skill's zip (for example `plug-and-play-deck.zip`).
5. Make sure the toggle next to the new skill is on.

Repeat for each skill you want. Uploaded skills are private to you until you share them.

### ChatGPT (desktop)

Standalone skills run in the ChatGPT desktop app, Codex CLI and the Codex IDE extension.

1. Open the ChatGPT desktop app.
2. Open **Skills** in the sidebar.
3. Choose the option to add or upload a skill and pick the skill's zip.
4. To use a skill on purpose, type `@` in a chat and select it. ChatGPT will also pick it automatically when your request matches the skill's description.

### Gemini Enterprise

Skills have to be turned on by a Gemini Enterprise admin first. If you don't see **Skills** in the menu, ask IT. Frontline licenses can use shared skills but can't upload new ones.

1. Sign in to the Gemini Enterprise web app.
2. In the navigation menu, click **Skills**.
3. Click the **+** icon, then **Upload skill**.
4. Upload the `-gemini.zip` version of the skill (with `SKILL.md` at the root of the zip). For a skill that is only a `SKILL.md` and nothing else, you can upload the `SKILL.md` file directly.
5. Gemini turns new skills on by default. Open a new chat from the skill or just ask for the task.

### Coding agents

If you use Claude Code, OpenAI Codex CLI, Gemini CLI, Cursor, GitHub Copilot or another agent that supports Agent Skills, you can install every skill in one command. You need [Node.js](https://nodejs.org) installed and GitHub access to this repo (run `gh auth login` once if you haven't).

```bash
# See what's available
npx skills add plug-and-play/slam-skills --list

# Install everything into every agent on your machine
npx skills add plug-and-play/slam-skills --agent '*' -g -y

# Or install one skill into one agent
npx skills add plug-and-play/slam-skills --skill plug-and-play-deck -a claude-code -g
```

Gemini CLI can also install straight from the repo:

```bash
gemini skills install https://github.com/plug-and-play/slam-skills.git --path skills
```

> Replace `plug-and-play/slam-skills` with this repo's actual `owner/name` if it differs.

### Build the zips yourself

From the root of the repo on a Mac:

```bash
mkdir -p dist

# Claude and ChatGPT format (folder inside the zip)
cd skills
for s in */; do s="${s%/}"; zip -rq "../dist/$s.zip" "$s" -x '*.DS_Store'; done
cd ..

# Gemini Enterprise format (SKILL.md at the zip root)
for s in skills/*/; do s="$(basename "$s")"; (cd "skills/$s" && zip -rq "../../dist/$s-gemini.zip" . -x '*.DS_Store'); done
```

Your zips will be in the `dist/` folder. Don't commit `dist/`, it's in `.gitignore`.

---

## Keep your skills up to date

Skills change as we improve them. Check the repo's **Releases** page or the commit history every couple of weeks.

- **Chat apps (Claude, ChatGPT, Gemini Enterprise):** download the new zip and upload it again. In Gemini Enterprise, use the skill's **Update** option instead of adding a duplicate. In Claude and ChatGPT, delete or turn off the old copy so you don't end up with two versions.
- **Coding agents:** run `npx skills update`.

---

## Contribute a change

Anyone on the SLAM team can suggest a change, whether it's fixing a typo, improving an existing skill or adding a new one. **Nobody pushes directly to `main`.** Every change goes through a pull request (PR), and the repo owner reviews and approves it before it's merged.

If you've never made a pull request, that's fine. Option A is all clicks.

### Before you start

- You need a GitHub account that has been added to this repo. Ask the repo owner if you can't see it.
- Check the open [Pull requests](../../pulls) and [Issues](../../issues) to make sure nobody is already working on the same thing.
- For a big change (a brand-new skill or rewriting most of one), open an **Issue** first describing what you want to do. It saves rework.

### Option A: in your browser, no Git needed

Best for small edits to one or two files.

**To edit an existing file:**

1. Open the file on GitHub (for example `skills/plug-and-play-deck/SKILL.md`).
2. Click the **pencil icon** (Edit this file) in the top right.
3. Make your changes.
4. Click **Commit changes...**.
5. In the dialog:
   - Write a short title, like `Fix chart color rule in deck skill`.
   - Add a sentence or two in the description about *why*.
   - Select **Create a new branch for this commit and start a pull request**. Name the branch something like `yourname/fix-deck-colors`.
6. Click **Propose changes**.
7. On the next screen, fill in the pull request template and click **Create pull request**.

**To add a new skill or upload files:**

1. Go to the `skills/` folder on GitHub.
2. Click **Add file > Create new file**.
3. In the name box, type `your-skill-name/SKILL.md`. Typing the `/` creates the folder for you.
4. Paste in the contents of [`templates/SKILL_TEMPLATE.md`](templates/SKILL_TEMPLATE.md) and fill it in.
5. Commit to a **new branch** and open a pull request, same as steps 4 to 7 above.
6. To add template files or images, open your new folder *on your branch*, click **Add file > Upload files**, drag them in and commit to the same branch. They'll be added to your open pull request automatically.

### Option B: from the command line

Best for bigger changes, scripts or several files at once.

**One-time setup:**

```bash
# Install Git and the GitHub CLI if you don't have them (Mac)
brew install git gh

# Log in to GitHub
gh auth login

# Download the repo
gh repo clone plug-and-play/slam-skills
cd slam-skills
```

**Every time you make a change:**

```bash
# 1. Start from the latest version of main
git checkout main
git pull

# 2. Make a branch for your change
git checkout -b yourname/short-description

# 3. Edit files, add a skill folder, etc.

# 4. Review what you changed
git status
git diff

# 5. Save your changes
git add skills/your-skill-name
git commit -m "Add startup scouting brief skill"

# 6. Upload your branch and open a pull request
git push -u origin yourname/short-description
gh pr create --fill
```

`gh pr create` will open the PR and assign it for review automatically. You can also open it from the yellow banner that shows up on the repo page after you push.

**If the reviewer asks for changes:** edit the files on the same branch, then `git add`, `git commit` and `git push` again. The PR updates itself. Don't open a new one.

### What happens after you open a pull request

1. The repo owner is automatically assigned as reviewer.
2. They'll review it, usually within a few business days, and either **approve**, **comment** or **request changes**.
3. Once approved, the owner merges it into `main`. Please don't merge your own PR even if GitHub lets you.
4. The new version shows up in the next release and anyone running `npx skills update` gets it.

### Pull request checklist

Your PR is much more likely to be approved quickly if:

- [ ] It changes one skill, or one clearly related set of things
- [ ] The skill folder name is lowercase with hyphens and matches the `name:` in `SKILL.md`
- [ ] The `description` says what the skill does **and** when to use it
- [ ] You tested it in at least one tool (say which in the PR)
- [ ] It contains no confidential partner, startup or deal information (see below)
- [ ] You updated the **Skill catalog** table in this README if you added or renamed a skill

---

## How to write a good skill

Start from [`templates/SKILL_TEMPLATE.md`](templates/SKILL_TEMPLATE.md). A minimal skill looks like this:

```markdown
---
name: startup-scouting-brief
description: Write a one-page startup scouting brief for a SLAM corporate partner. Use when asked to profile, vet, or summarize a startup for a partner, or to prep for a startup intro call.
---

# Startup scouting brief

## Inputs
- Startup name and website
- The corporate partner and the problem they want solved

## Steps
1. ...
2. ...

## Output format
...

## Final checks
- ...
```

**Rules that keep skills working in every tool:**

- **Name:** lowercase letters, numbers and hyphens only (`plug-and-play-deck`). The folder name must match exactly.
- **Description:** this is how the AI decides whether to use the skill, so lead with what it does and the words people will actually type. Keep it under 1,024 characters.
- **Keep the frontmatter simple.** `name` and `description` are the only fields every tool understands. Extra fields may be ignored or rejected.
- **Use relative paths.** Write `assets/cover-template.pptx`, never a path from your own computer.
- **Don't name tools that only exist in one product.** Say "create a slide deck," not "use the Artifact tool" or "use Canvas." Each assistant will use whatever it has.
- **Keep `SKILL.md` focused.** Put long reference material in `references/` and tell the skill when to read it. Shorter skills load faster and get followed more closely.
- **Scripts are optional and not supported everywhere.** If a skill depends on a script in `scripts/`, say so in the catalog table so people know which tools it works in.
- **Tool-specific extras are fine** as long as they're optional. For example, an `agents/openai.yaml` file adds display settings in ChatGPT and other tools simply ignore it.

---

## What never goes in this repo

Skills may get copied into several AI tools and onto everyone's machines, so treat everything here as visible to the whole SLAM team and to every AI vendor we use.

**Never commit:**

- Names, financials or deal terms of startups or corporate partners that are under NDA or not public
- Partner problem statements, internal memos, meeting notes or pipeline data
- Passwords, API keys, tokens or login links
- Personal information about anyone
- Files you don't have the rights to share (for example a vendor's paid template)

Skills should describe **how** we do the work, not contain the confidential work itself. If you need an example, make up a fictional company.

If you accidentally commit something sensitive, **tell the repo owner right away**. Deleting it in a new commit is not enough, because it stays in the Git history.

---

## Troubleshooting

**The upload says it can't find SKILL.md.** You probably used the wrong zip layout. Claude needs the folder inside the zip. Gemini Enterprise needs `SKILL.md` at the root. Also check the file is named exactly `SKILL.md`.

**The skill uploaded but the AI never uses it.** Make sure it's toggled on. Then try asking for it by name. If that works, the `description` needs better trigger words.

**I don't see a Skills option at all.** Your workspace admin probably hasn't turned skills on yet. That's true for Claude Team/Enterprise, ChatGPT business workspaces and Gemini Enterprise.

**`npx skills add` says repository not found.** It's a private repo, so you need to be logged in. Run `gh auth login` and try again.

**My PR has a merge conflict.** Someone else changed the same lines. Click **Resolve conflicts** on the PR page, or ask the repo owner for help.

---

## Questions and owner

**Repo owner and approver:** Dylan + James

For anything not covered here, open an [Issue](../../issues) or message Dylan or James directly.
