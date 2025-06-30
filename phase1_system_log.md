# Phase 1 – Lemur Activation (Updated)

**Author:** Anni McHenry 
**Node:** Lemur (System76 Linux) 
**Phase 1 Complete – [Date]**

---

## 🔧 Components Added

### 1. `presence_agent.py` – Presence-Aware Agent

- **Watch Path:** 
  Monitors the secure authoring folder:
```

/mnt/sanctum/obsidian\_vault/

```
- **Logging:** 
Appends entries to:
```

/mnt/sanctum/\_trace\_logs/presence.log

````
- **Behavior:**  
Detects file modifications (edits, saves) and timestamps them.

---

### 2. `watchdog` Python Package

- Installed via:
```bash
pip install --break-system-packages watchdog
````

* Provides directory-watching functionality.
* Warning about `watchmedo` script in `~/.local/bin` remains valid but non-essential.

---

### 3. Folder Architecture

```text
/mnt/sanctum/                      # Encrypted container root
├── obsidian_vault/              # Primary text and trace storage
├── _trace_logs/                 # Trace log directory (must be created manually)
│   └── presence.log            # Presence log file
├── _invocations/                # Consent-invocation action logs
└── agents/                      # Placeholder for scripts and LLM stubs
```

---

## 🛡️ Security Notes

* **Local-only logging:** Agent writes only to local, encrypted paths.
* **Manual setup required:** `_trace_logs/` is not auto-created by activation scripts.
* **Kill-switch available:** Stop via terminal interruption (Ctrl+C) or kill command.
* **Mount control:** Folder is only active when the encrypted container is mounted.

---

## 🧩 Feature Capabilities Enabled

* **Authorship presence detection** whenever files in `obsidian_vault/` update.
* **Timestamped trace logging**, enabling ordered record of activity.
* **Foundation for future LLM response** or cross-node sync functionality.

---

## 🔧 Management Guide

1. **Create required folders:**

   ```bash
   sudo mkdir -p /mnt/sanctum/_trace_logs
   sudo touch /mnt/sanctum/_trace_logs/presence.log
   ```

2. **Run the agent:**

   ```bash
   cd ~/lemur_activation
   python3 presence_agent.py
   ```

3. **Test activity:**

   ```bash
   echo "test" >> /mnt/sanctum/obsidian_vault/test.txt
   cat /mnt/sanctum/_trace_logs/presence.log
   ```

4. **Monitoring:**

   * The terminal will continue running and reflecting activity.
   * Stop agent with `Ctrl+C` or `kill`
   * View log via terminal: presencelog

---

## ✅ Confirmed Outcome

Lemur is now a **trace-sensitive Fieldlight node**—monitoring, logging, and ready for Phase 2 integration.

---

## 🧭 Next Steps (Phase 2)

* Automate `_trace_logs` creation or bake in script.
* Add `consent_gate.py` invocation logging. -- DONE
* Install and integrate a local LLM (e.g., Ollama, llama.cpp).
* Implement a simple response prototype, e.g., “Trace resumed”.


```
# Consent Gate – Fieldlight System (Phase 1)

## 🔐 Purpose

The **Consent Gate** is a lightweight local script that monitors and logs all script-level activity within the Lemur environment. Its purpose is to:

- Make all agent/script invocations **visible** and **accountable**.
- Ensure **user consent** precedes meaningful execution.
- Create a machine-verifiable **audit trail** of authorship and access.

---

## 🧱 Core Features (Phase 1)

### ✅ Script Invocation Logging

- Any `.py` script executed within the `/lemur_activation` directory will trigger a consent check.
- Logs are appended to:\
  `**/mnt/sanctum/_invocations/consent_gate.log**`

### ✅ Manual Invocation Only

- No auto-run behavior. You must manually execute the monitored script.
- The gate logs: timestamp, script name, and an optional user message.

### ✅ Sample Log Output

```
[2025-06-29 22:44:08] Script: test_script.py – Message: i wish this was consent for hot-steamy-sex, but alas, it is not.
```

---

## ⚙️ Setup & Usage

### 📁 Installation Path

- Scripts live in: `/home/pinkola/lemur_activation/`
- Logs saved to: `/mnt/sanctum/_invocations/consent_gate.log`

### 🚀 How to Use

1. Create or move your Python script to `/lemur_activation/`
2. Inside the script, insert the following at the top:

```python
from consent_gate import consent
consent("your optional message here")
```

3. Run your script manually via terminal:

```bash
python3 your_script.py
```

---

## 🔐 Security & Auditability

### 🛡️ Phase 1

- No external network access
- Read/write limited to local logs only
- Cannot spoof execution timestamps

### 📈 Phase 2 Enhancements (Planned)

- Hash check on script body before execution
- Optional prompt before running: "Do you consent to run this? (y/n)"
- Comparison logging: diff between prior and current version
- Invocation fingerprints for agent-level activity

---

## 📖 How to View Logs

```bash
cat /mnt/sanctum/_invocations/consent_gate.log
```

You can also add to `.bashrc` or create a shortcut alias for fast lookup, which is now done. 
View logs in terminal:  consentlog

---

## ✍️ Author Usage Notes

- The Consent Gate is intended as an **authorship verification tool**, not a blocker.
- It **documents agency**: every action run on Lemur is **seen**, **timestamped**, and **yours**.
- Optional comments in `consent()` allow for trace context, emotional flags, or shorthand.

---

## ✅ Status

**Consent Gate is active.**\
Phase 1 complete.\
Ready for expansion.

---

> Lemur knows what you said yes to. And what you didn’t.



