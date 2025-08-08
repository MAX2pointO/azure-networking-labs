## 🛠️ Troubleshooting – Lab 06: VNet Peering

This file documents the challenges encountered during the lab and the solutions applied to resolve them.

---

## 🚫 Issue 1: Unable to ping private IP before peering

- **Symptom**: Ping to the private IP of the other VM failed (`Destination host unreachable`)
- **Root Cause**: VNet peering was not yet established
- **Fix**: Created bidirectional VNet peering (east-to-west and west-to-east)

---

## 🔒 Issue 2: Ping failed after peering was created

- **Symptom**: Even after peering, `ping` command resulted in timeout
- **Root Cause**: Network Security Groups (NSGs) attached to the NICs or subnets did not allow ICMP traffic
- **Fix**: Added inbound rule in both NSGs to allow ICMP

```bash
az network nsg rule create \
  --resource-group rg-vnet-peering \
  --nsg-name nsg-east \
  --name allow-icmp \
  --protocol Icmp \
  --direction Inbound \
  --priority 1001 \
  --source-address-prefixes '*' \
  --destination-address-prefixes '*' \
  --access Allow \
  --description "Allow ICMP traffic"

🔑 Issue 3: SSH access failed intermittently

- **Symptom**: ssh: connect to host <public-ip> port 22: Connection timed out
- **Root Cause**: NSG might have missed port 22 inbound rule
- **Fix**: Ensured port 22 is allowed from 0.0.0.0/0 in both NSGs

❓ Issue 4: No public IP shown for VM

-Symptom: az vm list-ip-addresses returned empty publicIpAddresses[]
-Root Cause: Public IP not assigned during VM creation
-Fix: Added public IP through Portal or recreated VM with public IP config

🧾 Issue 5: VNet Peering shows "Initiated" instead of "Connected"

-Symptom: Peering from one side showed "Initiated"
-Root Cause: Peering was only configured from one VNet
-Fix: Added reverse peering from the other VNet to complete connection

🔁 Validation Summary

-Used az vm list-ip-addresses, az network vnet peering list and ping to validate setup
-Confirmed peering is bidirectional and NSGs are correctly configured
-Captured screenshots at each verification step

✅ Recommendations

-Always verify NSG rules when testing connectivity
-Ensure both directions of peering are created
-Use --output table to clearly see peering and IP information
-Use tags to group related resources for cleanup


---

