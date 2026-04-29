# 🧱 Brick #5 — Logging & SIEM Layer (Wazuh)

Author: Charles Tucker  
Project: Brick Stack  
Environment: Kali Live USB (Encrypted Persistence)  

---

## 🎯 Objective

Implement centralized security monitoring using Wazuh.

This brick demonstrates:

- SIEM deployment
- Host-based intrusion detection
- Log aggregation
- File integrity monitoring
- Vulnerability detection
- Alert generation and tuning
- Agent-based architecture

---

## 🏗 Initial Architecture (Phase 1)

Single-node deployment on Live USB:

Wazuh Manager + Indexer + Dashboard  
↓  
Local Agent (Kali)

Phase 2 will expand to:
- Cyberdeck agent
- Additional VM agents
- Network segmentation
