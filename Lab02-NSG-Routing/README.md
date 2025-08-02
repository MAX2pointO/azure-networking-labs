# Lab02 - Network Security Groups (NSGs) and Routing

## Objective
To understand and implement **Network Security Groups (NSGs)** to control inbound traffic between two virtual machines. This lab also demonstrates basic routing and access control in a Virtual Network.

## Key Components
- Two Ubuntu VMs in separate subnets:
  - `10.0.1.0/24` for VM1
  - `10.0.2.0/24` for VM2
- NSG rules allowing:
  - SSH from public IP to VM1
  - SSH from VM1 to VM2 (port 22)
  - HTTP from VM1 to VM2 (port 8080)

## Resources Created
- Virtual Network: `VNet-Lab02`
- Subnets: `Subnet-VM1`, `Subnet-VM2`
- NSG: `Lab02-NSG-VM2`
- VMs: `Lab02-VM1`, `Lab02-VM2`
- Keys: `Lab02-VM1_key.pem`, `Lab02-VM2_key.pem`

## Validation
- SSH into VM1 from local terminal
- From VM1, SSH into VM2 using private IP
- (Optional) HTTP access test on port 8080

## Screenshots
Refer to `/screenshots` folder.

