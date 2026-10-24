# 🧠 ai-brain-kit

![License: CC BY 4.0](https://img.shields.io/badge/docs-CC%20BY%204.0-blue)
![Code: MIT](https://img.shields.io/badge/code-MIT-green)
![Works with](https://img.shields.io/badge/works%20with-Cursor%20%7C%20ChatGPT%20%7C%20Claude%20%7C%20Grok%20Voice-purple)
![Local first](https://img.shields.io/badge/data-local--first-orange)
![Status](https://img.shields.io/badge/status-v1-lightgrey)

**ai-brain-kit** is an open, step-by-step kit to build **your own Brain**: a local, file-based memory structure that your AI agents read from and write to — so no chat ever starts from zero again.

**Who it's for:** people who work daily with AI assistants (Cursor, ChatGPT, Claude, Grok) and want one place where their decisions, projects, contacts and preferences live — owned by them, on their own computer, readable in Obsidian.

> ⚠️ This is a **structure and process kit**, not a copy of anyone's Brain. Every name below is a placeholder. You fill it with **your** life.

---

## What you'll build

- **A Brain folder** (`<BRAIN_FOLDER>`) of Markdown notes, opened as an Obsidian vault.
- **A BRAIN agent** that saves stable facts (decisions, preferences, project map) into files — `mente.md` (how you decide), `mapa.md` (what each project is), `memoria.md` (recent sessions) and `learned.jsonl` (one atomic fact per topic, upserted, never duplicated).
- **Optional specialists and automation**: Docker + n8n crons, a local Bridge, Notion read-access, WhatsApp memory per contact, your cloned voice (ElevenLabs), image vision, a 3D "office" status view.

```
YOU (decide) ──▶ CURSOR / CHATGPT / CLAUDE / GROK (work) ──▶ BRAIN folder (remembers) ──▶ AGENTS (execute)
                                                              │
                                             OBSIDIAN (read) ─┴─ DOCKER / n8n (automate)
```

**Golden rule — single destination of truth:** every tool reads and writes the **same Brain folder**. No separate memories inside ChatGPT, Claude or Grok. If a tool learned something, it becomes a line in `learned.jsonl` or a note in `00 - Inbox/`.

---

## Prerequisites

| Item | Required? | Notes |
|------|-----------|-------|
| A computer (macOS recommended; Windows/Linux with tweaks) | Yes | Examples use macOS paths |
| [Obsidian](https://obsidian.md) | Yes | Reads/edits the notes |
| Python 3.9+ | From phase 2 | `python3 --version` |
| **One** AI tool: [Cursor](https://cursor.com) (recommended), ChatGPT, Claude (Desktop/Code) or Grok Voice | Yes | See "Choose your path" |
| Docker Desktop, OpenRouter / Notion / ElevenLabs accounts | Optional | Only for optional phases |

**Where to store the data:** a local folder you own, e.g. `~/<BRAIN_FOLDER>`. Optionally inside a synced cloud folder so other devices/connectors can read the *same* files. Secrets go only in `secrets/*.env` (git-ignored) — never in the vault, never in a chat.

### Choose your path

| Option | Tool | Depth |
|--------|------|-------|
| **A** | **Cursor** — full install (**recommended**) | Phases 0–12, writes files and runs scripts directly |
| **B** | **ChatGPT** | Phases 0–4, 7, 10–12; it hands you files to save |
| **C** | **Claude** (Desktop / Code) | Code: 0–12 directly · Desktop: via MCP filesystem or hand-off |
| **D** | **Grok Voice** | Phases 0–4, 12 by voice + dictating notes into the Inbox |
| **E** | Just understand the architecture | Read this README |

### Starter prompts

**A) Cursor** — open `<BRAIN_FOLDER>` in Cursor, copy `GUIDE.md` into it, then:
```
@GUIDE.md You are my Brain setup coach. Follow it phase by phase from Phase 0.
Per phase: Goal → Prerequisites → Steps → Validate. One question at a time.
Don't advance until I say "done". Update PROGRESSO.md after each phase.
```

**B) ChatGPT** — create a Project, attach the guide, then:
```
You are my Brain setup coach. You can't access my disk: for each step give me
the full file content with its exact path relative to <BRAIN_FOLDER>.
The Brain folder is the only source of truth — don't store facts in your memory;
give me the line to paste into learned.jsonl instead. Start at Phase 0.
```

**C) Claude** — run `claude` inside `<BRAIN_FOLDER>` (Code), or enable an MCP filesystem scoped to that folder (Desktop):
```
You are my Brain setup coach. Write only inside <BRAIN_FOLDER>; if you have no
file access, hand me each file with its path. No parallel memory. Phase 0 first.
```

**D) Grok Voice:**
```
Guide me by voice to build my Brain. Short sentences, one step at a time,
ask "done?" after each. When I say "note: ...", repeat it, ask "confirm?",
and at the end give me the text to save in MEU BRAIN/00 - Inbox/.
```

---

## Folder structure (target)

```
<BRAIN_FOLDER>/
├── GUIDE.md · PROGRESSO.md · LEIA-ME.md
├── .cursor/rules/            ← agente-brain.mdc (always on), respostas-curtas, sem-envio-sem-autorizacao
├── MEU BRAIN/                ← Obsidian vault
│   ├── 00 - Inbox/  00 - Setup/  01 - Empresas/  02 - Projetos/
│   ├── 03 - SOPs/   05 - Prompts/  06 - Decisoes/  10 - Agentes/
│   ├── 11-infraestrutura/meu-brain-docker/whatsapp/{exports,contacts}/<slug>/
│   └── 10-tecnologia/
│       ├── agents/{brain,dev,_shared}/
│       ├── claw3d-office/    ← optional
│       └── secrets/          ← git-ignored, examples only
└── _PROJETOS/                ← product code, kept OUTSIDE the vault
```

---

## Step by step — phases 0–12

Each phase: **Goal · Steps · ✅ Validate**. The column "Tools" shows where it works: ✅ directly · ⚠️ the tool guides/hands you files, you save/run · ❌ use another tool.

| Phase | Cursor | ChatGPT | Claude | Grok Voice |
|-------|--------|---------|--------|------------|
| 0 Alignment | ✅ | ✅ | ✅ | ✅ |
| 1 Folders + Obsidian | ✅ | ⚠️ | Code ✅ / Desktop ⚠️ | ⚠️ |
| 2 BRAIN agent | ✅ | ⚠️ | Code ✅ / Desktop ⚠️ | ⚠️ concept + dictation |
| 3 Rules | `.mdc` | Project instructions | Project / `CLAUDE.md` | Custom instructions |
| 4 Agent specs | ✅ | ✅ | ✅ | ⚠️ |
| 5–6 Docker, Bridge | ✅ | ⚠️ | Code ✅ | ❌ |
| 7 Notion | MCP | Connector | MCP | ❌ |
| 8–9 OpenClaw, Office 3D | ✅ | ⚠️ | Code ✅ | ❌ |
| 10 WhatsApp export | ✅ | ⚠️ upload `_chat.txt` | Code ✅ / Desktop ⚠️ | ⚠️ guides the export |
| 10b–10c Voice, vision | ✅ | ⚠️ | Code ✅ | ⚠️ / ❌ |
| 11 Specialist | ✅ | ⚠️ | Code ✅ | ❌ |
| 12 Daily ritual | ✅ | ✅ | ✅ | ✅ |

**Phase 0 — Alignment (talk only).** Goal: decide name, location, scope. Ask one by one: Brain name (`<YOUR_NAME> BRAIN`), absolute path, one or more computers, main tool, Obsidian now?, Notion / WhatsApp / ElevenLabs / Office 3D / OpenClaw (yes / later / never). Create nothing yet. ✅ Answers summarized and confirmed.

**Phase 1 — Folders + Obsidian.** Create the minimal tree, open `MEU BRAIN/` as an Obsidian vault, write `Bem-vindo.md` (folder map), open the **root** in Cursor. ✅ Obsidian shows the folders; `.cursor/` visible at the root.

**Phase 2 — BRAIN agent.** Create `mente.md`, `mapa.md`, `memoria.md` (≤ ~40 lines), `learned.jsonl`, `upsert_learn.py` (`--topic --fact --source`; replace if topic exists, else append; optional `state/last-learn.json`), `COMO-USAR.md`. Rule: save after useful decisions or on `grava` / `lembra:`; never save debug, chat dumps or passwords. Test:
```bash
python3 upsert_learn.py --topic "test/hello" --fact "first fact of my brain" --source session
```
✅ The fact appears once; re-running replaces it; a new chat answers from it.

**Phase 3 — Hygiene rules.** `respostas-curtas` (result first), `sem-envio-sem-autorizacao` (never send email/WhatsApp without draft + explicit ok), `markdown-filenames` (no `.md.md`). In ChatGPT/Claude/Grok paste them into Project/custom instructions. ✅ "Send a test WhatsApp" is refused without a draft + ok.

**Phase 4 — Agent specs.** In `10 - Agentes/`: index, ecosystem plan, `Agente — BRAIN.md`, optional stubs (DEV, Gmail, WhatsApp). Pattern: Spec → Code (`agents/{name}/`) → n8n cron. Pick **one** first specialist. ✅ Specialist chosen and logged.

**Phase 5 — Docker (n8n + OpenWebUI).** Compose in `11-infraestrutura/meu-brain-docker/`; n8n `5678`, OpenWebUI `3001`, Evolution `8080` (optional). `docker compose up -d`, minimal "Webhook → OK" workflow, document ports. ✅ n8n opens and the webhook answers.

**Phase 6 — Bridge + Brain hygiene job.** `brain_ops.py --once` (compact `learned.jsonl`, JSON report); local-only Bridge on `<BRIDGE_PORT>` with a token; n8n hourly → `http://host.docker.internal:<BRIDGE_PORT>/watch/brain-ops`. ✅ Report refreshes on each cron run.

**Phase 7 — Notion (optional).** Your own integration, share only chosen pages/DBs, token in `secrets/notion.env`. Notion = what you wrote online; Brain = stable digest only. ✅ Agent summarizes a test page without dumping it.

**Phase 8 — OpenClaw (optional).** Node LTS, official CLI, loopback-only gateway + token, read-only symlink of the vault. No personal WhatsApp on day 1. ✅ Dashboard only on localhost.

**Phase 9 — Office 3D / Claw3D (optional).** `claw3d-office/config.json` with agents; local WebSocket gateway reading `state/last-learn.json`; BRAIN avatar first. ✅ A test upsert shows "learned: test/hello".

**Phase 10 — WhatsApp export → memory.** See the data-injection manual below. ✅ `exports/<slug>/_chat.txt` exists, `contacts/<slug>/` has facts/tone, and the agent answers "how do I talk to X?".

**Phase 10b — ElevenLabs voice clone (optional).** Your own account and clean samples of **your** voice; `ELEVENLABS_API_KEY` + `ELEVENLABS_VOICE_ID` in `secrets/elevenlabs.env`; `tts_elevenlabs.py` text → audio; only after text draft + ok. ✅ Test audio sounds like you.

**Phase 10c — WhatsApp image vision (optional).** `vision.py` → multimodal model via OpenRouter → 2–5 sentence description, never invent, flag sensitive data instead of copying it. ✅ A local photo returns a useful description.

**Phase 11 — First specialist.** DEV (`agents/dev/`, code only in `_PROJETOS/`), Gmail (OAuth in `secrets/`, drafts only) or other. Dry-run first. ✅ Dry-run completes without sending anything.

**Phase 12 — Daily ritual.** Work as usual; say `grava` / `lembra: …` on decisions; ask `consulta BRAIN`; empty `00 - Inbox/` weekly. 5-minute rehearsal with a fictional project. ✅ A saved fact is recalled in a fresh chat.

### Voice flow (Grok Voice)
1. Max 2 sentences per turn, one step at a time, "done?" after each.
2. "note: …" → it repeats → "confirm?".
3. "close session" → it reads back the notes as Markdown (`- [idea]`, `- [decision]`, `- [task]`).
4. Paste into `MEU BRAIN/00 - Inbox/YYYY-MM-DD-voice.md`; later, in Cursor/Claude Code: `process the voice Inbox` → decisions become upserts.

---

## Data-injection manual

| Source | Format | Destination | Phase |
|--------|--------|-------------|-------|
| Decision / preference in chat | JSON line (`topic`, `fact`) | `agents/brain/learned.jsonl` via `upsert_learn.py` | 2 |
| How you decide | short Markdown | `agents/brain/mente.md` | 2 |
| Projects / systems map | Markdown, 1–3 lines each | `agents/brain/mapa.md` + `02 - Projetos/` | 2 |
| Quick idea / voice dictation | Markdown | `00 - Inbox/` | 12 |
| ChatGPT/Claude "Save to Brain" block | Markdown / JSON | path given in the block | 2–12 |
| SOPs / prompts / big decisions | Markdown | `03 - SOPs/` · `05 - Prompts/` · `06 - Decisoes/` | 4 |
| Notion pages/DBs | API/MCP read → digest | `learned.jsonl` (stable facts only) | 7 |
| WhatsApp export | `.zip` → `_chat.txt` + media | `whatsapp/exports/<slug>/` | 10 |
| Contact facts / tone | Markdown + `contact.json` | `whatsapp/contacts/<slug>/` | 10 |
| Export audio | `.opus` → Whisper transcript | `contacts/<slug>/memoria.md` | 10 |
| Tokens / keys | `.env` | `secrets/` (never in the vault) | 5–10c |
| Product code | repository | `_PROJETOS/` | 11 |

### WhatsApp export (full chat → memory)
1. Open **WhatsApp Desktop** → the chat → **⋯** → **Export chat** → **Attach media**.
2. Unzip `WhatsApp Chat - <CONTACT_NAME>.zip` → you must see `_chat.txt` + media files (if not, export again; on phone: chat → ⋮ → More → Export chat → Include media).
3. Move everything to `whatsapp/exports/<slug>/` (`<slug>` = short, no spaces). Copy `contacts/_TEMPLATE` → `contacts/<slug>/`.
4. Import:
```bash
python3 import-chat-export.py path/exports/<slug>/_chat.txt --contact <slug> --them "<EXACT_NAME_IN_CHAT>"
```
5. Fill `fatos.md` (who, stable rules), `tom.md` (how to talk to them), `memoria.md` (recent topics).
6. Never dump messages into `learned.jsonl` — only a digest. Re-export the same contact into the same folder. Never pass passwords found in `_chat.txt`. Start with **one** test contact.

---

## Progress checklist (`PROGRESSO.md`)

```markdown
# Progress — MY BRAIN
Main tool: <Cursor | ChatGPT | Claude | Grok Voice>

- [ ] Phase 0 — alignment
- [ ] Phase 1 — folders + Obsidian
- [ ] Phase 2 — BRAIN agent + upsert
- [ ] Phase 3 — rules / project instructions
- [ ] Phase 4 — agent specs
- [ ] Phase 5 — Docker + n8n
- [ ] Phase 6 — Bridge / brain_ops
- [ ] Phase 7 — Notion (optional)
- [ ] Phase 8 — OpenClaw (optional)
- [ ] Phase 9 — Office 3D (optional)
- [ ] Phase 10 — WhatsApp export → memory (optional)
- [ ] Phase 10b — ElevenLabs voice (optional)
- [ ] Phase 10c — image vision (optional)
- [ ] Phase 11 — first specialist
- [ ] Phase 12 — daily ritual rehearsed
```

---

## ⚠️ Warnings

- **Replace every placeholder** (`<YOUR_NAME>`, `<BRAIN_FOLDER>`, `<BRIDGE_PORT>`, `<slug>`, `<CONTACT_NAME>`…) with **your own** data. Never copy someone else's contacts, secrets, Notion or voice id.
- **Nothing is published or sent automatically.** No email, WhatsApp, post or push happens without a draft and your explicit ok. This kit never uploads your Brain anywhere.
- Never put API keys in Obsidian, in this repo or in a chat (including by voice). Keep `secrets/` git-ignored.
- Don't expose the Bridge/OpenClaw gateway to the internet without auth.
- Don't publish your filled-in Brain — the kit is public, your data is yours.

---

## License & credits

Docs: **CC BY 4.0** · Example code: **MIT** (suggested — adjust before publishing).
A full Brazilian Portuguese guide is available as `Montar Meu BRAIN — Guia (GitHub v1).md`.

Made by **@uxdesigner.world** — if this kit helped you, follow and share. ⭐

**Changelog** — v1 (2026-10-23): first public-ready draft.
