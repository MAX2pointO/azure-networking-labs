# Troubleshooting – Lab 07: Azure Bastion

This document records the issues faced while setting up and connecting to a VM using Azure Bastion.

1. 🔐 SSH Key Permission Error
--------------------------------

**Error Observed:**When attempting to connect using Bastion’s native SSH client, an error was displayed:
```bash
WARNING: UNPROTECTED PRIVATE KEY FILE!
Permissions 0644 for 'id_rsa' are too open.
```
Cause:
SSH requires that private keys have strict file permissions for security reasons. The file was accessible to more than just the owner.

Fix:
- Ran the following command to restrict permissions:
- chmod 600 ~/Downloads/id_rsa
- After this change, the SSH connection proceeded without the permission warning.

2. 🧱 Issue: NSG Blocked AzureBastionSubnet

**Error Message:**
(NetworkSecurityGroupNotCompliantForAzureBastionSubnet)

**Cause:**
- Attempted to attach a custom NSG (`Block443NSG`) that blocked inbound TCP 443 to the AzureBastionSubnet.

**Fix:**
- Azure enforces strict requirements on Bastion. Either use no NSG or one with explicitly allowed TCP 443 and 3389.

3. 🔀 Issue: VM Couldn’t Reach Internet (curl failed)
---------------------------------------------------------

**Error Observed:**
```bash
curl https://www.microsoft.com
# No response / failed DNS or routing
```
Cause:
- Custom route table sent all traffic (0.0.0.0/0) to 0.0.0.0 as next hop — a non-existent appliance.

Fix:

- Remove the faulty route table.
- Or configure a correct next hop (e.g., Firewall, NVA, etc.) if needed.

4. 🔌 Issue: Session Drop or Timeout
----------------------------------------

Cause:
Leaving Bastion tab idle for too long without user interaction.

Fix:
- Manually refresh the session tab.
- If VM or Bastion was deallocated, restart resources from portal.

✅ Notes

- Azure Bastion cannot function with improperly configured NSGs or invalid route tables.
- Always validate Bastion subnet name and compliance (must be AzureBastionSubnet).
- Use az network bastion list or portal to confirm Bastion health.
