# Fieldlight — Public Snapshot (2025‑07‑30)

*Local‑first AI & knowledge‑governance stack by ****Anni McHenry****.*\
This file is the **only** system‑state document meant for the public repo.\
All raw traces, logs, and the full Naming Container live in a separate private repository.

---

## What Fieldlight Does

| Layer                | Purpose                                                                                                  | Live Status                                    |
| -------------------- | -------------------------------------------------------------------------------------------------------- | ---------------------------------------------- |
| **Presence Agent**   | Watches the encrypted Obsidian vault for changes, logs them, and triggers the auto‑indexer.              | **Running** as a user‑level `systemd` service. |
| **Naming Container** | Generates canonical IDs (people / events / artifacts / concepts) + fast SQLite lookup for the local LLM. | **Auto‑rebuilt** on every vault change.        |
| **Encrypted Vault**  | Stores all notes & logs; keeps data sovereign and offline.                                               | **Mounted** at login via Veracrypt.            |
| **Local LLM**        | Answers questions using only offline data + Naming Container context.                                    | **Operational** (model: Llama 3 8‑B Q4).       |

**Data never leaves the machine** unless a scrubbed slice is manually exported.

---

## High‑Level Flow

```mermaid
graph LR
    A[Obsidian edits] -->|sync| B[vault (encrypted)]
    B --> C[Presence Agent]
    C --> D[scan_vault.py]
    D --> E[Naming Container (.index.sqlite)]
    E --> F[Local LLM]
```

1. **Edit** a note (desktop or mobile).
2. Changes sync into the encrypted vault.
3. Presence Agent logs the event and calls the scanner.
4. Scanner stubs new YAML entities & rebuilds the index.
5. Local LLM answers queries using the fresh index.

---

## Current Configuration (sanitised)

- **Vault:** Veracrypt volume mounted at a user‑only path.
- **Python venv:** `~/nc_env` isolates dependencies.
- **Service:** `presence-agent.service` auto‑starts on login, restarts on failure.
- **Helper CLI:** `nc_add` allows scratch entities (rarely needed now that scanning is automatic).

---

## Public vs. Private

| Repo                                 | Visibility | Contents                                                           |
| ------------------------------------ | ---------- | ------------------------------------------------------------------ |
| **fieldlight\_snapshot** (this repo) | Public     | Résumé packet, sanitized system snapshot (this file), demo assets. |
| **fieldlight\_private**              | Private    | Naming Container YAMLs, presence logs, Phase 1–3 system logs.      |

To request deeper technical details, reach out to `anni@fieldlight.com`.

---

*Last updated 2025‑07‑30*

