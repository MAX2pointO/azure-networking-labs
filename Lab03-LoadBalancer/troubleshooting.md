Troubleshooting - Lab03
Issue: Public IP creation failed with Basic SKU
Resolution: Azure free trial accounts have limitations on the number of Basic SKU Public IPs. Switched to --sku Standard during public IP creation to resolve the error.

Issue: Load Balancer backend pool couldn’t be attached to NIC
Resolution: The NIC IP configuration name was incorrect. Used the command
az network nic show --name <NIC_NAME> --resource-group <RG_NAME> --query "ipConfigurations[].name"
to find the exact IP config name (ipconfigVM1-LB, ipconfigVM2-LB) before rerunning the address-pool add command.

Issue: Load Balancer public IP not resolving in browser
Resolution: No NSG was attached to NICs initially, causing all incoming traffic to be blocked. Created an NSG (myNSG), added inbound rule to allow HTTP (port 80), and attached it to both NICs using az network nic update.

Issue: Couldn’t SSH into VMs
Resolution: After attaching the NSG, also added an inbound rule to allow SSH (port 22) from the local public IP. Confirmed access via ssh azureuser@<VM_PUBLIC_IP>.

Issue: Couldn’t verify Load Balancer HTTP response
Resolution: Logged into each VM, verified Apache was installed via cloud-init, and ran curl http://localhost to confirm response contained hostname (e.g., Welcome from VM1-LB). This ensured LB traffic would correctly be routed and load-balanced.

Issue: Tried to delete RG from inside VM
Resolution: az CLI is not available by default inside the VM. Returned to the local shell and ran:
az group delete --name RG-Lab03 --yes --no-wait


