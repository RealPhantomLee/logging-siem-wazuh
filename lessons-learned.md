# 🧠 Brick #5 — Lessons Learned (Wazuh SIEM)

## What Worked

- The all-in-one Wazuh stack (Manager + Indexer + Dashboard) runs successfully on Kali Live USB with persistence.
- `systemctl` verification is the quickest truth source:
  - wazuh-manager.service
  - wazuh-indexer.service
  - wazuh-dashboard.service
- The Wazuh API can be validated directly by checking the listening port:
  - Port 55000 listening confirms API service is up.

## What Broke / Friction Points

- **Credential confusion** is the main trap:
  - Dashboard login credentials (OpenSearch Dashboards / Wazuh UI)
  - Wazuh API credentials (RBAC / Wazuh API auth)
  These are not always the same, and mismatch causes misleading failures.

- The Wazuh dashboard may appear "up" (serving https://localhost)
  while still failing inside the app due to API auth issues (401/403 errors).

- Installing `wazuh-agent` can trigger unexpected package changes (e.g., attempts to remove manager).
  This is risky on a single-node setup unless you confirm what APT plans to remove.

## Root Cause (So Far)

- Wazuh Dashboard was configured to talk to the Wazuh API at:
  - https://localhost:55000
- API auth attempts returned:
  - 401 Unauthorized (Invalid credentials)
- RBAC database exists here:
  - /var/ossec/api/configuration/security/rbac.db
This indicates RBAC is active and credentials must match what the API expects.

## Key Evidence Collected

- Services status shows stack is running.
- Port 55000 is listening.
- Dashboard logs show API auth failures and SSL/certificate warnings.
- Installer output later revealed generated Dashboard credentials:
  - User: admin
  - Password: (generated)

## What I Would Do Differently Next Time

- Immediately capture and store generated credentials (admin password) into a protected local note.
- Before changing anything with APT, run:
  - apt -s install <package>
  to preview removals/upgrades.
- Treat “Dashboard login” and “API login” as separate systems until proven aligned.

## Next Steps (Continuation Plan)

1) Confirm dashboard login works:
   - Visit: https://localhost
   - Log in with admin + generated password

2) Confirm API credentials used by the dashboard:
   - Validate API auth with curl once correct credentials are known

3) Align Wazuh dashboard API config with working credentials:
   - /usr/share/wazuh-dashboard/data/wazuh/config/wazuh.yml

4) Only after access is stable:
   - Enroll local Kali as an agent (to generate real alerts)
   - Trigger test detections (FIM change, auth failure, etc.)
   - Capture screenshots + logs into /screenshots and /logs

