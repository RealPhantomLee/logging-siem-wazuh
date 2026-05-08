# Logging & SIEM — Wazuh

**Single-node Wazuh SIEM deployed on a Kali Live USB — manager + indexer + dashboard + local agent, with first-login guide and dashboard proof.**

This is **Brick #5** of a portable Kali Live USB security platform. See [Related projects](#related-projects).

---

## What this is

A working, hands-on SIEM lab: Wazuh installed end-to-end on Kali Live USB with encrypted persistence (Brick #1), with the local Kali host enrolled as the first agent. The repo ships the install script, a first-login guide, and a dashboard screenshot proving the stack is up and shipping events.

This brick demonstrates:

- SIEM deployment (manager + indexer + dashboard, single-node)
- Host-based intrusion detection
- Log aggregation
- File integrity monitoring (FIM)
- Vulnerability detection
- Alert generation and tuning
- Agent-based architecture

---

## Quick start

```bash
# 1) On a fresh Kali Live USB with persistence (see Brick #1)
sudo ./wazuh-install.sh -a

# 2) Save the admin password printed at the end of install

# 3) Open the Wazuh dashboard
# https://<live-usb-ip>:443
# Login as user 'admin' with the password from step 2

# 4) Confirm the local Kali agent is enrolled and reporting
```

See [`install/first-login.md`](install/first-login.md) for the post-install checklist (admin password rotation, dashboard tour, agent enrollment verification).

---

## Architecture

### Phase 1 — single-node (delivered)

```
Wazuh Manager + Indexer + Dashboard  (on Kali Live USB)
        ↑
   Local Kali agent
```

Everything runs on the Live USB. The single-node footprint is intentional — it keeps the lab fully portable and lets the operator stand up a SIEM anywhere a Pi cyberdeck can boot.

### Phase 2 — expansion (planned)

- Cyberdeck agent (Brick #2)
- Additional VM agents (e.g., the targets in [Brick #6 vulnerability lab](https://github.com/RealPhantomLee/vulnerability-management-lab))
- Network segmentation between agents and manager

---

## Proof artifacts

The [`screenshots/`](screenshots/) folder includes:

- `wazuh-dashboard-initial.png` — first-login dashboard view confirming manager + indexer + agent are up

---

## Repository layout

| Path | Purpose |
|------|---------|
| `wazuh-install.sh` | Wazuh single-node installer (upstream Wazuh-provided script, vendored for offline use) |
| `install/first-login.md` | Post-install checklist and dashboard tour |
| `build.md` | Build narrative |
| `lessons-learned.md` | Tuning notes and gotchas |
| `screenshots/` | Dashboard proof |
| `assets/` | Reference images |

---

## Related projects

- [live-usb-encrypted-persistence](https://github.com/RealPhantomLee/live-usb-encrypted-persistence) — Brick #1 (host platform)
- [cyberdeck-platform](https://github.com/RealPhantomLee/cyberdeck-platform) — Brick #2 (hardware)
- [interface-hud-operator-controls](https://github.com/RealPhantomLee/interface-hud-operator-controls) — Brick #3 (HUD)
- [toolchain-layer](https://github.com/RealPhantomLee/toolchain-layer) — Brick #4 (security tools)
- **logging-siem-wazuh** (this repo) — Brick #5
- [vulnerability-management-lab](https://github.com/RealPhantomLee/vulnerability-management-lab) — Brick #6 (targets to feed this SIEM)
- [azure-security-monitoring-lab](https://github.com/RealPhantomLee/azure-security-monitoring-lab) — cloud counterpart (Azure Log Analytics + KQL detections)

---

## Author

**Charles Tucker** — [github.com/RealPhantomLee](https://github.com/RealPhantomLee)
