VNet Peering # Lab 06: VNet Peering – Azure Networking Labs

This lab demonstrates how to configure and validate **Virtual Network (VNet) Peering** in Azure. The objective is to enable communication between two VMs located in separate virtual networks using private IP addresses through peering.

---

## 🧪 Lab Goals

- Create two VNets: `vnet-east` and `vnet-west`
- Deploy a VM in each VNet: `vm-east` and `vm-west`
- Create VNet peering between the two VNets
- Test connectivity using private IP addresses (via SSH or ping)
- Verify peering status and traffic flow
- Observe restriction in absence of peering

---

## 🏗️ Resources Created

|    Resource Type   |      Name       |    Region    |
|--------------------|-----------------|--------------|
| Resource Group     | rg-vnet-peering | East US      |
| Virtual Network    | vnet-east       | East US      |
| Subnet             | subnet-east     | East US      |
| Virtual Machine    | vm-east         | East US      |
| NIC                | nic-east        | East US      |
| Public IP          | vm-east-ip      | East US      |
| Virtual Network    | vnet-west       | West US      |
| Subnet             | subnet-west     | West US      |
| Virtual Machine    | vm-west         | West US      |
| NIC                | nic-west        | West US      |
| Public IP          | vm-west-ip      | West US      |
| VNet Peering       | east-to-west /  | Bidirectional|
|                    | west-to-east    |              |

---

## 📸 Important Screenshots Captured

- Peering configuration in Azure Portal (both directions)
- Successful ping/SSH from one VM to another using private IP
- `az network vnet peering list` confirmation
- NSG rules allowing ICMP/SSH
- Troubleshooting failed ping before peering

---

## ⚙️ Commands Used

```bash
# Get private IPs
az vm list-ip-addresses --name vm-east --resource-group rg-vnet-peering --query "[].virtualMachine.network.privateIpAddresses[]" --output tsv

# Get public IPs
az vm list-ip-addresses --name vm-west --resource-group rg-vnet-peering --query "[].virtualMachine.network.publicIpAddresses[].ipAddress" --output tsv

# SSH into VMs using public IP
ssh azureuser@<public-ip>

# Ping private IP
ping <private-ip-of-other-vm>

# Create VNet peering
az network vnet peering create \
  --name east-to-west \
  --resource-group rg-vnet-peering \
  --vnet-name vnet-east \
  --remote-vnet /subscriptions/<sub-id>/resourceGroups/rg-vnet-peering/providers/Microsoft.Network/virtualNetworks/vnet-west \
  --allow-vnet-access

# View peering status
az network vnet peering list --resource-group rg-vnet-peering --vnet-name vnet-east --output table
```
---

##🧾 Outcome

VMs in different VNets could not communicate before peering.

After peering and NSG configuration, VMs successfully pinged each other using private IPs.

Public IPs were used only for SSH access.

Peering status was shown as Connected in both directions.

##📂 GitHub Uploads

README.md

troubleshooting.md

Screenshots (named clearly, e.g. peering-config-east.png, ping-success.png, etc.)

##🧠 Key Learnings

VNet peering enables secure communication across regions

NSG must allow ICMP/SSH for testing

DNS resolution and routing still depend on explicit settings (if needed)

Peering is non-transitive, and must be explicitly configured in both directions
