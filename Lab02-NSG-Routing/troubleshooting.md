# Troubleshooting - Lab02

### Issue: SSH connection to VM2 timed out
- **Resolution**: NSG rule for port 22 was missing or misconfigured. Added rule to allow SSH from VM1 private IP to VM2 on port 22.

### Issue: PEM file permission denied
- **Resolution**: Ran `chmod 400 <filename>.pem` to fix key file permissions.

### Issue: Couldn't SSH from VM1 to VM2
- **Resolution**: Needed to `scp` the private key of VM2 to VM1.

