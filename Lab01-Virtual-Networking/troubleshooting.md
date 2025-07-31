# Troubleshooting Notes – Lab 01

## Scenario: Connectivity between two VMs

- ✅ No issues encountered.
- Tested with `ping 10.0.1.5` from VM1.
- Result: Success, <1ms latency.
- NSG allowed ICMP traffic within the subnet by default.

## Notes
- SSH requires correct PEM permissions: `chmod 400 key.pem`
- To delete resources after lab: delete the resource group

