# Lab04 - Application Gateway

## 🎯 Objective

To deploy and configure an Azure Application Gateway that distributes HTTP traffic across two backend VMs. This lab demonstrates how to use Azure Application Gateway for Layer 7 (HTTP/HTTPS) load balancing.

---

## 🧩 Key Concepts Covered

- Azure Application Gateway setup with HTTP rule
- Backend pool with two Ubuntu VMs
- Custom `cloud-init` script to configure Apache with hostname welcome message
- Frontend listener, backend HTTP settings, and routing rule configuration
- Health probes to monitor backend availability

---

## 🛠️ Resources Created

- **Resource Group**: `RG-Lab04`
- **Virtual Network**: `VNet-Lab04`
- **Subnets**:
  - `AppGatewaySubnetNew` (for Application Gateway)
  - `BackendSubnet` (for VMs)
- **Virtual Machines**:
  - `VM1-AppGW` (10.0.0.4)
  - `VM2-AppGW` (10.0.0.5)
- **Public IP**: `AppGWPublicIP` (40.121.183.156)
- **Application Gateway**: `MyAppGateway`

---

## ⚙️ Validation

After successful deployment, hitting the public IP in a browser (`http://40.121.183.156`) returns:

Welcome from VM1-AppGW

Or:

Welcome from VM2-AppGW

This verifies:
- Backend pool is functioning
- Request routing rule is working
- Health probes are detecting healthy instances
- Traffic is being distributed among both VMs

---

## 🖼️ Screenshots

See `/screenshots` folder for:

- Resource group creation
- VNet and subnet setup
- VM deployment
- `cloud-init` usage
- Application Gateway configuration
- Portal view of routing rules, health probes, backend pool
- Output from browser & terminal

---

## 📦 Cleanup

To avoid incurring charges, the entire lab environment can be deleted using:

```bash
az group delete --name RG-Lab04 --yes --no-wait
