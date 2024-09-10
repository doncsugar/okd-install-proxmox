# okd-install-proxmox
Installation of OKD using Proxmox and pfSense
# Assumptions
- Your Proxmox host has >=69GB of memory
- Your Proxmox host has ~20 cores (feel free to oversubscribe)
- Your Proxmox host has >=538GB of storage for hosts
- Your goal is the smallest OKD cluster w/separate Control Plane
- Your OKD cluster will be entirely on vlan 99 (feel free to choose another)
# Prerequisite ISOs
- Fedora CoreOS
- Rocky Linux (can use any linux)
- pfSense
# Required Hosts
8 Hosts will be used. You can add the ISOs to the CD drive, but do not power them on yet.
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
- okd-bootstrap-01
	- OS: Fedora CoreOS
	- CPU: 4 cores
	- Memory: 16GB
	- Storage: 100GB
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
You will need to allocate IPs for each node. Take the hostname, the MAC address of the host's NIC, and a unique IP address and create a DHCP static mapping. You should have 7, one for each cluster host. Your cluster hosts will inherit the names you choose here.
## DNS
- You will need 4 sets of records:
	- Forward and Reverse records for each host in your cluster
 	- Forward and Reverse records for your cluster's API server virtual IP (provisioned by HAProxy on pfSense)
  	- Forward record for your cluster's apps
You will need to choose a domain name. I suggest `pve.lan` (Proxmox Virtual Environment).
You will need a cluster name. I suggest `okd`.
You will need DNS A and PTR records for each host. Combining your domain name and cluster name with the hostname of `okd-control-01`, you might get `okd-control-01.okd.pve.lan` as the FQDN. 
## Load Balancer (HAProxy)
- You will have 4 HAProxy Frontends on your OKD virtual IP:
	- API server on 6443
 	- machine config server on 22623
  	- ingress router on 443
  	- ingress router on 80
- You will have 4 respective HAProxy Backends:
	- API server on 6443 for your 3 control plane nodes + bootstrap
 	- machine config server on 22623 for your 3 control plane nodes + bootstrap
  	- ingress router on 443 for your 2 workers
  	- ingress router on 80 for 2 workers
