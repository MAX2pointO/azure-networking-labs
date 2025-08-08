# Lab 07: Azure Bastion – Azure Networking Labs

This lab demonstrates how to securely connect to a VM without exposing public IPs, using Azure Bastion. It also includes optional experiments involving NSG misconfiguration and faulty routing to understand how Bastion connectivity can be disrupted.

---

## 🧪 Lab Goals

- Deploy Azure Bastion in a new virtual network
- Create a VM (Linux) in a subnet inside the same VNet
- Access the VM securely over Azure Bastion using the Azure Portal (no public IP)
- Understand the role of AzureBastionSubnet
- Explore NSG and Route Table misconfigurations to test failure scenarios

---

## 🏗️ Resources Created

|     Resource Type     |       Name        |  Region  |
|-----------------------|-------------------|----------|
| Resource Group        | rg-bastion-lab    | East US  |
| Virtual Network       | BastionVNet       | East US  |
| Subnet (Bastion)      | AzureBastionSubnet| East US  |
| Subnet (Workload)     | WorkloadSubnet    | East US  |
| Virtual Machine       | MyVM              | East US  |
| Azure Bastion Host    | MyBastion         | East US  |
| NSG                   | AllowBastionNSG   | East US  |

---

## 📸 Important Screenshots Captured

- Bastion deployment confirmation
- Successful Bastion connection to VM
- Apache installation and status
- NSG rule misconfiguration result
- Route Table misconfiguration effect (connectivity break)
- Ping/curl failure to public internet (after bad route)

---

## ⚙️ Commands Used

```bash
# Install Apache
sudo apt update && sudo apt install apache2 -y

# Check Apache status
sudo systemctl status apache2

# Test curl or ping to check external connectivity
curl https://www.microsoft.com
ping 8.8.8.8

# Create NSG blocking TCP 443 (FAILED due to Bastion compliance requirement)
az network nsg create --name Block443NSG --resource-group rg-bastion-lab --location eastus

# Attempt to assign it to AzureBastionSubnet (FAILED)
az network vnet subnet update \
  --vnet-name BastionVNet \
  --name AzureBastionSubnet \
  --network-security-group Block443NSG

# Create route table with next hop to 0.0.0.0 (non-existent)
az network route-table create --name BadRouteTable --resource-group rg-bastion-lab --location eastus

az network route-table route create \
  --resource-group rg-bastion-lab \
  --route-table-name BadRouteTable \
  --name bad-default \
  --address-prefix 0.0.0.0/0 \
  --next-hop-type VirtualAppliance \
  --next-hop-ip-address 0.0.0.0

# Associate route table with subnet
az network vnet subnet update \
  --name WorkloadSubnet \
  --vnet-name BastionVNet \
  --resource-group rg-bastion-lab \
  --route-table BadRouteTable
```
---

🧪 Optional Experiments

- ❌ Blocked TCP 443 to AzureBastionSubnet via NSG → Bastion deployment/assignment failed due to compliance checks.
- ⚠️ Route Table misconfiguration → Caused broken internet connectivity from VM (curl/ping failed).
- ⏱️ Session reconnect test → Verified that session resumes or refreshes within Azure Bastion timeout window.

🧾 Outcome

- Bastion allowed secure access to VM without public IP.
- Apache installed and verified over Bastion session.
- Misconfigured NSG failed with compliance warning (expected).
- Faulty routing broke outbound access (proven via curl failure).
- Cleaned up all resources after verification.

📂 GitHub Uploads

- README.md
- troubleshooting.md
- Screenshots:
  bastion-connected.png
  apache-running.png
  nsg-failure.png
  route-break-ping.png

🧠 Key Learnings

- Azure Bastion requires specific NSG rules and subnet naming.
- Public IP is not needed for VM when using Bastion.
- NSG and Route Table misconfigurations can block or break Bastion connectivity.
- Safe network architecture demands caution with default routes and subnet-level security.
