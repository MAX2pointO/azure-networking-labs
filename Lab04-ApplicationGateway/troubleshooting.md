## 🧰 `troubleshooting.md`

```markdown
# Troubleshooting - Lab04

## ❌ Issue: VM creation failed due to SKU not available

**Cause**: Default VM size `Standard_DS1_v2` was not available in East US.

**Resolution**:
Used the `--size Standard_B1s` flag to explicitly specify a supported SKU.

---

## ❌ Issue: Application Gateway deployment failed

**Cause**: `AppGatewaySubnet` already had other resources. App Gateway requires an isolated subnet.

**Resolution**:
Created a new subnet `AppGatewaySubnetNew` and used that during deployment.

---

## ❌ Issue: Request routing rule missing priority

**Cause**: Priority parameter (`--priority`) was missing in the CLI command.

**Resolution**:
Added `--priority 100` to `az network application-gateway create`.

---

## ❌ Issue: Traffic not switching between VMs

**Cause**: Cloud-init might not have executed or Apache not installed.

**Resolution**:
- SSH into both VMs
- Checked `/var/www/html/index.html` and Apache status
- Restarted Apache if needed

---

## ✅ Best Practices

- Always validate Application Gateway status with:

```bash
az network application-gateway show \
  --name MyAppGateway \
  --resource-group RG-Lab04 \
  --query "provisioningState" -o tsv

Use cloud-init for automation and reproducibility.
