# Lobster Brain — Installation Guide (Claude Code)

## What you need

- [Claude Code](https://claude.ai/code) installed on your Mac
- [Obsidian](https://obsidian.md) with a vault set up
- iCloud (or another sync service) if you want vault access across devices

---

## Step 1 — Update the vault path

Before installing, open `skills/lobster-brain/SKILL.md` and find this line:

```
/Users/michieljanssen/Library/Mobile Documents/iCloud~md~obsidian/Documents/Michiel/
```

Replace it with the path to your own Obsidian vault. You can find your vault path in Obsidian under **Settings → About → Vault path**.

---

## Step 2 — Package the skill

You need to package the skill folder into a `.skill` file before installing it.

The packaging script is included with Claude Code's skill creator. Run this from your terminal:

```bash
cd ~/.claude/skills/skill-creator  # or wherever skill-creator lives
python3 -m scripts.package_skill /path/to/lobster-brain
```

If you get a `No module named 'yaml'` error, install it first:

```bash
pip3 install pyyaml
```

The script will output a `lobster-brain.skill` file in the current directory.

---

## Step 3 — Install the skill

1. Open **Claude Code**
2. Go to **Settings → Skills**
3. Click **Install skill**
4. Select the `lobster-brain.skill` file you just created

The skill is now available globally across all Claude Code sessions and Cowork.

---

## Step 4 — Set up your Obsidian vault

The skill will automatically create `Lobster Brain.md` and all subtopic pages on first use. If you want to pre-seed the starter categories (AI, Health, Tech, Business, Fitness), you can run `/lobsterbrain` and it will build the structure for you.

Alternatively, copy the contents of `skills/lobster-brain/SKILL.md` to see the exact folder structure and create it manually.

---

## Using the skill

Trigger it by typing `/lobsterbrain` in Claude Code, or simply:
- Share a screenshot
- Paste a link
- Describe a podcast moment
- Paste long-form text you want to save

The skill will extract metadata, propose a category and subtopic, show you a confirmation draft, and only write to your vault after you approve.

---

## Folder structure created in Obsidian

```
Lobster Brain.md                          ← overview and routing index
Lobster Brain/
├── AI/
│   ├── Claude Tips and Tricks.md
│   ├── Claude Code.md
│   ├── Claude MD Files.md
│   ├── OpenClaw Tips.md
│   └── Prompting Resources.md
├── Health/
│   ├── Peptides.md
│   └── Supplements.md
├── Tech/
│   ├── Mac.md
│   └── Networks.md
├── Business/
│   ├── Start-up Stories.md
│   ├── Business Ideas.md
│   └── Business Inspiration.md
└── Fitness/
    └── Exercise Ideas.md
```

New categories and subtopics are proposed and added with your approval as your knowledge base grows.

---

## Syncing to iPhone

If your Obsidian vault is stored in iCloud, all captures made on your Mac are automatically available in the Obsidian iPhone app. No additional setup needed.

Mobile capture (sending items directly from your phone to the vault) is not yet supported but is planned for a future version.
