# Lab 05 – Private Endpoint for Azure Storage

## 📌 Objective

This lab demonstrates how to securely access Azure Storage (Blob) using **Private Endpoints** within a Virtual Network (VNet), and how to properly configure DNS resolution using **Private DNS Zones**. It simulates real-world Microsoft support scenarios involving secure access, broken DNS, and permission issues.

---

## 🛠️ What We Did

### 1. **Storage Account & Container**
- Created a storage account: `mystorageankitlab05`
- Created a container inside it: `testcontainer`
- Uploaded a blob file: `index.html` with sample content

### 2. **Virtual Network & VM**
- Created a VNet with a subnet
- Deployed a Linux VM (`VM-Lab05`) inside the subnet
- SSH access verified and Azure CLI used from within VM

### 3. **Private Endpoint**
- Configured a Private Endpoint for the Storage Account in the same subnet
- Automatically associated a Private DNS Zone: `privatelink.blob.core.windows.net`
- Verified private IP DNS resolution inside the VM

### 4. **DNS Verification**
- Used `nslookup` and `curl` to verify that blob requests resolve privately
- Accessed the blob (`index.html`) privately inside the VM without using public endpoint

---

## ✅ Verification Done

- `curl` from the VM to private blob endpoint worked
- `az storage blob list` showed blob only **after** assigning `Storage Blob Data Reader` role
- Verified DNS resolution using `nslookup`

---

## 🧹 Cleanup Performed

- Deleted:
  - Private Endpoint
  - Private DNS Zone
  - VM and NICs
  - Storage Account and Blob
  - Virtual Network and Resource Group

Everything related to the lab was removed to avoid unnecessary charges.

---

## 💡 Key Learnings

- Private Endpoints override public access by routing traffic within the Azure backbone.
- DNS configuration is critical for private access — especially when testing in VMs.
- Azure RBAC roles like `Storage Blob Data Reader` are required when using `az storage blob` commands with `--auth-mode login`.

---
