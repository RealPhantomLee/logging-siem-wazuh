# 🧱 Brick #5 — Build Log (Wazuh SIEM Deployment)

Author: Charles Tucker  
Project: Brick Stack  
Brick: 5 — Logging & SIEM Layer (Wazuh)  
Environment: Kali Live USB (Encrypted Persistence)  
Deployment: Single-node (Manager + Indexer + Dashboard)

---

## 🎯 Objective

Deploy Wazuh and produce portfolio-grade proof of:

- Services installed and running
- Dashboard reachable via HTTPS
- API reachable on port 55000
- Agent enrollment
- Test alert generation
- Evidence captured and sanitized

---

# =========================
# Phase 1 — Service Verification
# =========================

## Verify services

sudo systemctl status wazuh-manager --no-pager -l
sudo systemctl status wazuh-indexer --no-pager -l
sudo systemctl status wazuh-dashboard --no-pager -l

Save proof:

sudo systemctl status wazuh-manager --no-pager -l > logs/wazuh-manager-status.txt
sudo systemctl status wazuh-indexer --no-pager -l > logs/wazuh-indexer-status.txt
sudo systemctl status wazuh-dashboard --no-pager -l > logs/wazuh-dashboard-status.txt

---

# =========================
# Phase 2 — Port Verification
# =========================

sudo ss -tulpn | egrep '(:443|:55000)\b' > logs/ports-listening.txt

Expected:
443 (Dashboard)
55000 (Wazuh API)

---

# =========================
# Phase 3 — API Reachability
# =========================

curl -kI https://127.0.0.1:55000 > logs/api-head.txt

Expected:
405 Method Not Allowed OR 401 Unauthorized
Either confirms API is responding.

---

# =========================
# Phase 4 — Dashboard Access
# =========================

Access:
https://localhost

Capture:
screenshots/03-dashboard-login.png
screenshots/04-wazuh-app-load.png

---

# =========================
# Phase 5 — Current Issue
# =========================

Observed:
401 Unauthorized between Dashboard plugin and Wazuh API.

Root cause:
Dashboard credentials do not match Wazuh API RBAC user.

RBAC database location:
/var/ossec/api/configuration/security/rbac.db

Next:
Align API credentials and update wazuh.yml host block.

---

# =========================
# Phase 6 — Resume Plan
# =========================

1. Verify dashboard login with generated admin credentials.
2. Confirm valid API RBAC user.
3. Update:
   /usr/share/wazuh-dashboard/data/wazuh/config/wazuh.yml
4. Restart dashboard:
   sudo systemctl restart wazuh-dashboard
5. Confirm no 401 errors in:
   sudo journalctl -u wazuh-dashboard -n 100

---

## Current State

Services: Running  
Ports: Listening  
Dashboard: Reachable  
Blocker: API authentication mismatch pending correction  

