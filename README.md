# okd-install-proxmox
Installation of OKD using Proxmox and pfSense
# Assumptions
- Your Proxmox host has >=69GB of memory
- Your Proxmox host has ~20 cores (feel free to oversubscribe)
- Your Proxmox host has >=538GB of storage for hosts
- Your goal is the smallest OpenShift cluster w/separate Control Plane
# Prerequisite ISOs
- Fedora CoreOS
- Rocky Linux (can use any linux)
- pfSense
# Required Hosts
7 Hosts will be used.
- pfSense
	- OS: pfSense
	- CPU: >=1 core
	- RAM: 1GB
	- Storage: 8GB
	- NICs:
		- net0 (external to cluster)
		- net1 (internal to cluster) vlan 99
- Linux Jumpbox
	- OS: Rocky Linux
	- CPU: >=1 core
	- RAM: 4GB
	- Storage: 30GB
	- NIC:
		- net0 vlan 99
- okd-control-plane-01
	- OS: Fedora CoreOS
	- CPU: 4 cores
	- Memory: 16GB
	- Storage: 100GB
	- NIC:
		- net0 vlan 99
- okd-control-plane-02
	- OS: Fedora CoreOS
	- CPU: 4 cores
	- Memory: 16GB
	- Storage: 100GB
	- NIC:
		- net0 vlan 99
- okd-control-plane-03
	- OS: Fedora CoreOS
	- CPU: 4 cores
	- Memory: 16GB
	- Storage: 100GB
	- NIC:
		- net0 vlan 99
- okd-worker-01
	- OS: Fedora CoreOS
	- CPU: 4 cores
	- Memory: >=8GB
	- Storage: 100GB
	- NIC:
		- net0 vlan 99
- okd-worker-02
	- OS: Fedora CoreOS
	- CPU: 4 cores
	- Memory: >=8GB
	- Storage: 100GB
	- NIC:
		- net0 vlan 99
# pfSense Configuration
## DHCP
## DNS
## Load Balancer (HAProxy)
