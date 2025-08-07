# Troubleshooting – Lab 05: Private Endpoint for Azure Storage

This file documents all the issues encountered during the lab and their resolutions.

---

## ❌ Issue 1: Blob not accessible via public URL

**Symptoms:**
- After uploading `index.html` to the container, trying to access it via:
https://mystorageankitlab05.blob.core.windows.net/testcontainer/index.html

gave **404** or **access denied** errors.

**Root Cause:**
- **Public network access** was disabled due to private endpoint creation.
- Blob container permissions were **private** by default.

**Resolution:**
- Verified access via VM using private DNS.
- Ensured that DNS resolution from VM points to the private IP of the blob endpoint.

---

## ❌ Issue 2: `az storage blob list` – Permission Error

**Command:**
```bash
az storage blob list \
--account-name mystorageankitlab05 \
--container-name testcontainer \
--auth-mode login \
--output table

Error:

You do not have the required permissions needed to perform this operation.
Root Cause:

Even with "Owner" access at the resource group level, data plane operations like blob list require explicit roles.

Resolution:

Assigned the Storage Blob Data Reader role at the storage account scope:

az role assignment create \
  --assignee "<userPrincipalId>" \
  --role "Storage Blob Data Reader" \
  --scope "/subscriptions/<sub-id>/resourceGroups/<rg-name>/providers/Microsoft.Storage/storageAccounts/mystorageankitlab05"

---

❌ Issue 3: DNS name of blob not resolving inside VM

Symptoms:

curl https://mystorageankitlab05.blob.core.windows.net gave DNS resolution errors.
nslookup showed SERVFAIL or public IPs.

Root Cause:

Private endpoint was created but DNS zone was not linked correctly to the VNet.
Resolution:

Checked the Private DNS Zone privatelink.blob.core.windows.net:

Linked it manually to the correct VNet.
Verified that an A record was present for mystorageankitlab05.

Ran:

nslookup mystorageankitlab05.blob.core.windows.net
and confirmed it resolved to a private IP.


✅ Takeaways

DNS troubleshooting is critical for private endpoints.

Use nslookup inside the VM to verify private IP resolution.

Azure RBAC for Storage requires data plane roles.

Blob containers are private by default — access through public URLs may not work after private endpoint is enabled.


