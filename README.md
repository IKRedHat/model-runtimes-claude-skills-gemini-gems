# model-runtimes-claude-skills-gemini-gems

A repository of custom AI instructions — Claude Skills and Gemini Gems — for generating strategic testing proposals for open-source model serving runtimes integrated with **Red Hat OpenShift AI (RHOAI)**.

Each skill/gem acts as a specialized research analyst: given a runtime name (e.g., vLLM, Hugging Face TGI, Seldon MLServer), it produces a full strategic testing proposal plus a draft customer-facing KCS article scoped to Red Hat's "Tested & Verified" support boundary.

---

## Repository Structure

```
model-runtimes-claude-skills-gemini-gems/
├── claude-skills/
│   └── serving-runtime-strategy/
│       └── SKILL.md          # Claude skill definition
├── gemini-gems/
│   └── serving-runtime-strategy/
│       └── GEM_CONFIG.md     # Gemini Gem system prompt
└── README.md
```

---

## Prerequisites

| Requirement | Notes |
|---|---|
| **Git** | Required to clone this repository |
| **Claude CLI** | Install via `npm install -g @anthropic-ai/claude-code` |
| **Claude Pro / Max subscription** | Required to use custom skills in Claude |
| **Google Gemini Advanced subscription** | Required to create and use custom Gems |

---

## Claude Setup

### 1. Clone the repository

```bash
git clone https://github.com/<your-org>/model-runtimes-claude-skills-gemini-gems.git
cd model-runtimes-claude-skills-gemini-gems
```

### 2. Install the skill into your global Claude skills directory

Claude loads custom skills from `~/.claude/skills/`. You have two options:

**Option A — Symlink (recommended):** Changes you pull from this repo are instantly reflected without re-copying.

```bash
# Create the skills directory if it doesn't already exist
mkdir -p ~/.claude/skills

# Symlink the skill folder into your global skills directory
ln -s "$(pwd)/claude-skills/serving-runtime-strategy" ~/.claude/skills/serving-runtime-strategy
```

**Option B — Copy:** Use this if you want an independent local copy.

```bash
mkdir -p ~/.claude/skills
cp -r claude-skills/serving-runtime-strategy ~/.claude/skills/serving-runtime-strategy
```

### 3. Verify the installation

Confirm the skill is visible to Claude:

```bash
ls ~/.claude/skills/
# Expected output:
# serving-runtime-strategy
```

### 4. Test the skill

Start Claude Code and invoke the skill with a runtime name as the argument:

```bash
claude
```

Then at the Claude prompt, run:

```
/serving-runtime-strategy vLLM
```

Claude will research vLLM and render the full strategic proposal and KCS draft inline.

**No argument? No problem.** If you invoke the skill without a runtime name:

```
/serving-runtime-strategy
```

Claude will prompt you: *"Which model serving runtime would you like me to research and build a strategic testing proposal for in the context of Red Hat OpenShift AI?"*

### 5. Updating the skill

If you used the symlink approach, simply `git pull` from the repository root — no re-linking required:

```bash
git pull origin main
```

If you used the copy approach, re-run the `cp` command after pulling to overwrite the old copy.

---

## Gemini Gem Setup

Gemini Gems are custom AI personas created inside Google Gemini's web interface. The `GEM_CONFIG.md` file contains the complete system prompt to paste in.

### 1. Open the Gem configuration file

Open `gemini-gems/serving-runtime-strategy/GEM_CONFIG.md` in any text editor and **copy the entire file contents to your clipboard**.

```bash
# macOS — copies the full file to clipboard automatically
pbcopy < gemini-gems/serving-runtime-strategy/GEM_CONFIG.md
```

### 2. Navigate to Gemini Gem Manager

1. Go to [https://gemini.google.com](https://gemini.google.com) and sign in with your Google account that has **Gemini Advanced** active.
2. In the left sidebar, click **"Gem manager"** (or navigate directly to [https://gemini.google.com/gems/view](https://gemini.google.com/gems/view)).
3. Click **"New Gem"** (the `+` button).

### 3. Configure the Gem

| Field | Value |
|---|---|
| **Name** | `Serving Runtime Strategy` |
| **Instructions** | Paste the full contents of `GEM_CONFIG.md` here |

Click **Save**.

### 4. Test the Gem

Open the Gem you just saved. It will immediately ask:

> *"Which model serving runtime would you like me to research and build a strategic testing proposal for in the context of Red Hat OpenShift AI (e.g., Seldon MLServer, Hugging Face TGI, Ray Serve, vLLM)?"*

Type a runtime name (e.g., `Triton Inference Server`) and Gemini will render the full structured proposal.

### 5. Updating the Gem

Gems do not auto-sync with this repository. When `GEM_CONFIG.md` is updated:

1. `git pull origin main` to get the latest version.
2. Re-open the Gem in Gem Manager.
3. Clear the Instructions field, re-copy the updated `GEM_CONFIG.md`, and paste it in.
4. Click **Save**.

---

## Output Structure

Every invocation — whether via Claude or Gemini — produces a two-part document:

| Section | Description |
|---|---|
| **Strategic Testing Proposal** | 7-section document covering executive summary, T&V definition, versioning, tiered testing strategy, recommended models, trend analysis, and estimated AWS infrastructure costs |
| **Draft KCS Article** | Customer-facing support scope article scoped to Red Hat's "Tested & Verified" boundary, ready for internal review before publishing to the Red Hat Customer Portal |

---

## Related Resources

* [Red Hat OpenShift AI Documentation](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed)
* [Tested and Verified Runtimes — Article #7089743](https://access.redhat.com/articles/7089743)
* [KServe Documentation](https://kserve.github.io/website/)
* [Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
* [Gemini Gems Help](https://support.google.com/gemini/answer/14932015)

---

## Contributing

To add a new skill or gem (e.g., for a different workflow like cost analysis or CVE triage):

1. Create a new folder under `claude-skills/<skill-name>/` with a `SKILL.md`.
2. Mirror it under `gemini-gems/<skill-name>/` with a `GEM_CONFIG.md` (Markdown-only format, no YAML frontmatter or XML tags).
3. Update this README with setup instructions for the new asset.
4. Open a pull request.
