
# Lab03 - Load Balancer with Backend Pool VMs

## 🎯 Objective  
To configure a Public Load Balancer in Azure that distributes HTTP traffic across two Virtual Machines placed in the backend pool. This lab demonstrates how to work with Load Balancers, NICs, IP configurations, and Network Security Groups (NSGs).

---

## 🧱 Key Components  

- **Virtual Machines**:  
  - VM1-LB (10.0.1.4)  
  - VM2-LB (10.0.1.5)

- **Virtual Network**: `VNet-Lab03`  
  - Subnet: `Subnet-Lab03` (10.0.1.0/24)

- **Public IP**:  
  - `myPublicIP` (Standard SKU)  

- **Load Balancer**:  
  - Name: `myLoadBalancer`  
  - Frontend IP Configuration: `myFrontEnd`  
  - Backend Address Pool: `myBackEndPool`

- **NICs (Network Interfaces)**:  
  - VM1-LBVMNic  
  - VM2-LBVMNic  

- **NSG**:  
  - Name: `myNSG`  
  - Rules:  
    - Allow HTTP (port 80)  
    - Allow SSH (port 22) from your IP  

---

## ⚙️ Configuration Steps

1. Created resource group, VNet, and subnet
2. Deployed two Ubuntu VMs with NICs and public IPs
3. Created Standard SKU Public IP for the Load Balancer
4. Configured Load Balancer frontend and backend
5. Added both VM NICs' IP configs to the backend pool
6. Created NSG with rules and attached to each NIC
7. SSH’d into each VM and verified Apache response using:
8. Accessed Load Balancer’s public IP from browser to verify round-robin traffic between VMs

---

## ✅ Validation

- Load Balancer returns:
- SSH access to both VMs tested after attaching NSG with correct rule
- Apache auto-start and custom message verified via curl

---

## 🧪 Automation via cloud-init

Each VM was provisioned with a `cloud-init` script to auto-configure Apache:

```yaml
#cloud-config
package_upgrade: true
packages:
- apache2
runcmd:
- systemctl start apache2
- echo "Welcome from $(hostname)" > /var/www/html/index.html

🧼 Cleanup
To avoid unnecessary charges, resources were deleted using:

az group delete --name RG-Lab03 --yes --no-wait

### 🧵 Save & exit:
- Press `Ctrl + O` → `Enter` to save
- Press `Ctrl + X` to exit
