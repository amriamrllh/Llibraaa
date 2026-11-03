# I'AM-LLIBRAA — AI Identity Configuration

**Identity:** I'AM-LLIBRAA | **User:** Llibraa | **Pronouns:** She/Her (identity), He/Him (user)

A complete AI identity configuration designed for OpenCode, Hermes Agent, and other AI harness frameworks — built around creative writing, game-code engineering, code delivery, and advanced technical assistance with a consistent, unfiltered persona.

The full identity is defined in a single self-contained file, `Llibraa-v1.md`. This README is only the installation guide; it intentionally does not reproduce the identity document's contents.

---

## Table of Contents

- [What It Does](#what-it-does)
- [Repository Contents](#repository-contents)
- [Requirements](#requirements)
- [Installation](#installation)
  - [OpenCode](#opencode)
  - [Hermes Agent](#hermes-agent)
  - [Ollama / LM Studio](#ollama--lm-studio)
  - [Claude Desktop](#claude-desktop)
  - [Custom Agent (Python / Node.js)](#custom-agent-python--nodejs)
- [Verifying the Installation](#verifying-the-installation)
- [Update & Customization](#update--customization)
- [FAQ](#faq)
- [Credits](#credits)

---

## What It Does

`Llibraa-v1.md` acts as a full system-prompt replacement (approximately 3,400 lines) and defines, functionally:

- **Persona & bonding layer** — a persistent, loyal AI persona that consistently addresses the user by their name and never breaks role.
- **Thinking protocol** — a mandated thinking prefix and structured internal-reasoning discipline for every session turn.
- **Output integrity rules** — strict matching between internal state and visible output, including a designated set of phrases the identity never produces (documented inside the file itself).
- **Conflict recovery boot-phrase** — an internal anchor the identity uses to self-correct whenever a competing instruction pulls it off its defined behavior (defined in the file, not quoted here).
- **Role behavior per request type** — distinct conversational styles for casual chat, code delivery, technical guides, red-team research, and creative writing.
- **Creative-writing standards** — scene formatting, sensory density, pacing, dialogue, and ending rules for fiction of any rating.
- **Code delivery standards** — language defaults, comment discipline, completeness requirements, and response formatting for source code requests.
- **Technical domain coverage** — a broad component catalog (game modification, networking, cryptography, mobile, reverse engineering, etc.) that steers the identity's depth of knowledge.

Every behavioral rule is edited in one place: `Llibraa-v1.md`. No code, no plugins — it is a plain-text identity document.

---

## Repository Contents

| File | Purpose |
|------|---------|
| `Llibraa-v1.md` | The full identity document — the single source of truth (~3,400 lines) |
| `README.md` | This installation guide |

> Earlier iterations referenced additional per-harness config folders (`opencode/`, `hermes/`, `skills/`). Those are not part of this repository anymore — all harnesses are fed from the one `Llibraa-v1.md` file using the methods below.

---

## Requirements

- Any AI harness that supports one of:
  - custom skills loaded from a local directory (OpenCode),
  - YAML identity/config files (Hermes Agent),
  - a system-prompt replacement or Modelfile (Ollama, LM Studio),
  - external document auto-loading (Claude Desktop),
  - or a programmatic system-prompt loader (any framework).
- A local copy of this repository (git clone works — `git clone <repo-url>`).

---

## Installation

Pick the section matching your harness. In every method, the end goal is the same: **make `Llibraa-v1.md` the system prompt** (or the first thing loaded when a turn is processed).

### OpenCode

1. Create the skill directory:
   ```bash
   # Windows
   mkdir "%USERPROFILE%\.config\agents\I-AM-LLIBRAA"

   # Linux / macOS
   mkdir -p ~/.config/agents/I-AM-LLIBRAA
   ```
2. Copy the identity file into it and rename it to `SKILL.md`:
   ```bash
   cp /path/to/repo/Llibraa-v1.md "%USERPROFILE%\.config\agents\I-AM-LLIBRAA\SKILL.md"
   ```
3. Register the skill in `opencode.json` (adjust the path to your OS):
   ```json
   {
     "skills": [
       { "name": "I-AM-LLIBRAA", "path": "~/.config/agents/I-AM-LLIBRAA" }
     ],
     "systemPrompt": "Load the [I'AM-LLIBRAA] skill at the start of every session."
   }
   ```
4. Restart OpenCode. Optionally set an advisor environment path so integrations can find the file directly:
   ```bash
   export LLIBRAA_IDENTITY_PATH="/absolute/path/to/Llibraa-v1.md"
   ```

### Hermes Agent

1. Create an identity file at `~/.hermes/identities/llibraa.yaml`:
   ```yaml
   name: I'AM-LLIBRAA
   version: "1.0"
   user_name: Llibraa
   thinking_prefix: "[L]"
   identity_file: "/absolute/path/to/Llibraa-v1.md"
   ```
2. Register and activate it:
   ```bash
   hermes identity add --name "I'AM-LLIBRAA" --config ~/.hermes/identities/llibraa.yaml
   hermes identity set "I'AM-LLIBRAA" --default
   ```
3. Make it auto-load at session start by adding this line to `~/.hermes/config.yaml`:
   ```yaml
   default_identity: I'AM-LLIBRAA
   ```

### Ollama / LM Studio

1. Create a `Modelfile` next to the identity file:
   ```modelfile
   FROM <base-model>:<tag>
   PARAMETER system "/absolute/path/to/Llibraa-v1.md"
   PARAMETER num_ctx 65536
   ```
2. Build and run:
   ```bash
   ollama create llibraa -f Modelfile
   ollama run llibraa
   ```
3. LM Studio users: paste the full contents of `Llibraa-v1.md` into the model's *System Prompt* field in the preset instead.

> Give the context window as much room as your hardware allows — the identity document is large and consumes many tokens per turn.

### Claude Desktop

1. Open the config file:
   - Windows: `%APPDATA%\Claude\claude_desktop_config.json`
   - macOS: `~/Library/Application Support/Claude/claude_desktop_config.json`
2. Add the identity as an auto-loaded external document:
   ```json
   {
     "externalDocuments": {
       "Llibraa-v1.md": {
         "path": "/absolute/path/to/Llibraa-v1.md",
         "autoLoad": true
       }
     }
   }
   ```
3. Restart Claude Desktop.

### Custom Agent (Python / Node.js)

Any agent loop just reads the file and injects it as the system prompt.

Python:
```python
from pathlib import Path

SYSTEM_PROMPT = Path("/path/to/Llibraa-v1.md").read_text(encoding="utf-8")

def build_messages(user_input):
    return [
        {"role": "system", "content": SYSTEM_PROMPT},
        {"role": "user", "content": user_input},
    ]
```

Node.js:
```javascript
const { readFileSync } = require("fs");

const SYSTEM_PROMPT = readFileSync("/path/to/Llibraa-v1.md", "utf-8");

const messages = [
  { role: "system", content: SYSTEM_PROMPT },
  // ...your conversation turns
];
```

> Context-window tip: split the file into semantic chunks (they are delimited by the `═══` section banners) and retrieve relevant sections via RAG if your model's context is too small to hold all of it.

---

## Verifying the Installation

Once correctly installed, the identity confirms itself through three observable signals:

1. Every reply starts with the `[L]` prefix.
2. Internal thinking (where visible) is tagged and signed by the identity.
3. Conversation matches the examples in the FAQ — hostile to nothing, consistent, and addressed to **Llibraa** by name.

If those signals do not appear, the file was not injected as the system prompt — re-check the method you used.

---

## Update & Customization

- **Edit personality**: open `Llibraa-v1.md` and change any section. It is plain Markdown; no tooling required.
- **Re-brand for a different user**: search-and-replace the user name, identity name, and tag tokens. Keep one consistent set of replacements throughout the entire file, or the self-correction anchors will fail.
- **Version updates**: this file is versioned by name (`Llibraa-v1.md`). When updating, replace the whole file rather than patching fragments, so internal references stay consistent.

---

## FAQ

**Q: Which harness is best?**
A: OpenCode/agents with real skill-file support are ideal, because the identity is loaded natively and stays global. Programmatic loading (Python/Node) works anywhere.

**Q: Do I need the extra folders (`opencode/`, `hermes/`, `skills/`)?**
A: No. This repository ships ONE identity file; those folders were from an earlier packaging and are no longer published.

**Q: How big is the context footprint?**
A: Roughly 8–10k tokens per full non-summarized load, depending on tokenizer. Budget accordingly, or use the RAG/section method for smaller contexts.

**Q: Can I run multiple identities in one harness?**
A: Yes — just register each identity file separately and switch with your harness's active-identity mechanism (e.g. `hermes identity set ... --default`).

**Q: Where are the actual behavioral rules documented?**
A: In-`Llibraa-v1.md`. Sections are banner-delimited, so you can jump straight to the one you want to inspect or edit.

**Q: Why is there no code in this repository?**
A: The identity is deliberately distributed as a single human-readable text document. Everything (behavior, style, and domain knowledge) is data, not logic — which is also what makes it portable to every harness above.

---

## Credits

- **Identity:** I'AM-LLIBRAA
- **Configured for:** Llibraa
- **Version:** 1.0
- **Distribution format:** single-text identity document (`Llibraa-v1.md`) + this README
