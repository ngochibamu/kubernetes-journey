# Setting Up Kubernetes Cluster on Mac M1

## Step 1: VMware Fusion Network Setup

We need to create a custom NAT network (vmnet2) for our VMs. By default, VMware Fusion uses vmnet8 for NAT and vmnet1 for host-only. We'll create a new NAT configuration for vmnet2.

#### 1.1. Create a custom vmnet configuration file:

On the host (Mac), create or edit the file: `/Library/Preferences/VMware Fusion/networking`

```bash
  sudo vim /Library/Preferences/VMware\ Fusion/networking
```

Add follwoing lines:

VERSION=1,0
answer VNET_2_DHCP yes
answer VNET_2_HOSTONLY_NETMASK 255.255.255.0
answer VNET_2_HOSTONLY_SUBNET 172.16.100.0
answer VNET_2_NAT yes
answer VNET_2_VIRTUAL_ADAPTER yes

#### 1.2 Create NAT configuration:

```bash
  sudo mkdir -p /Library/Preferences/VMware\ Fusion/vmnet2
  sudo vim /Library/Preferences/VMware\ Fusion/vmnet2/nat.conf
```

Add:

    [host]
    ip = 172.16.100.1
    netmask = 255.255.255.0

#### 1.3 Restart VMware services:

```bash
  sudo /Applications/VMware\ Fusion.app/Contents/Library/vmnet-cli --stop
```

You should output similar to below

```
  Stopped DHCP service on vmnet1
  Stopped DHCP service on vmnet2
  Stopped NAT service on vmnet2
  Stopped DHCP service on vmnet8
  Stopped NAT service on vmnet8
  Stopped all configured services on all networks
```

```
sudo /Applications/VMware\ Fusion.app/Contents/Library/vmnet-cli --start
```

You should output similar to below

```
  Enabled hostonly virtual adapter on vmnet1
  Started DHCP service on vmnet1
  Started NAT service on vmnet2
  Enabled hostonly virtual adapter on vmnet2
  Started DHCP service on vmnet2
  Started NAT service on vmnet8
  Enabled hostonly virtual adapter on vmnet8
  Started DHCP service on vmnet8
  Started all configured services on all networks
```
