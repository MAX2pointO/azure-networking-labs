# Troubleshooting – Site-to-Site VPN

This guide helps you identify and fix issues with your Site-to-Site VPN setup in Lab 08.

---

## 1️⃣ Tunnel Down

**Symptoms:**
- Azure Portal shows "Not Connected"
- No response to ping between networks

**Possible Causes & Fixes:**
- **Public IP Mismatch**
  - Verify both gateways are configured with each other’s *correct* public IP
- **Shared Key Mismatch**
  - Both sides must use the same IPsec/IKE pre-shared key
  - Reset shared key on both ends if unsure
- **Gateway Not Provisioned**
  - Ensure both Virtual Network Gateways are in `Succeeded` provisioning state

---

## 2️⃣ Shared Key Mismatches

**Symptoms:**
- Tunnel never reaches "Connected" state
- Event logs mention authentication failure

**How to Fix:**
- Compare the shared key in Azure and on-prem
- Update one side so they match exactly (case-sensitive)
- Restart the VPN connection after fixing

---

## 3️⃣ Route Problems

**Symptoms:**
- Tunnel shows "Connected" but no traffic passes
- Ping fails between VMs, but VMs can ping within their own subnet

**How to Fix:**
- **Address Space Overlap**:
  - Ensure Azure VNet address space and on-prem VNet address space do **not** overlap
- **Missing Routes**:
  - Check the *Connection* configuration to ensure correct address prefixes are set
- **Network Security Group (NSG) Rules**:
  - Verify NSGs allow inbound/outbound traffic between subnets
- **Firewall on VM**:
  - Ensure VM-level firewall rules allow ICMP/SSH/RDP

---

## 4️⃣ Verification Commands

**From Azure VM:**
```bash
ping <onprem_vm_private_ip>
traceroute <onprem_vm_private_ip>
```
From On-Prem VM:
```bash
ping <azure_vm_private_ip>
traceroute <azure_vm_private_ip>
```
Azure Portal Check:
Go to Virtual Network Gateway → Connections → See "Connected" status

5️⃣ Useful Logs

-Azure Activity Log: Check provisioning & connection attempts
-On-Prem VPN Device Logs: Look for IPsec/IKE errors
-syslog on Linux VMs for connectivity issues
