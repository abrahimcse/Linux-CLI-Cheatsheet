# Configure Static IP on Ubuntu

To configure a static IP on Ubuntu, you need to modify the network configuration files using `Netplan`.

**1. Identify the Network Interface**
```
ip a
```
Look for the interface name (e.g., `eth0`, `enp3s0`, `wlp2s0`, etc.). You’ll need this for configuring the static IP.

**2.Edit the Netplan Configuration**

- You can find the `.yaml` file for network configuration in this directory.

```
ls /etc/netplan/
```
- You will see a file like `01-netcfg.yaml` or `50-cloud-init.yaml`

**Modify the YAML File for Static IP**
- `open the file for editing`
```
sudo vim /etc/netplan/01-netcfg.yaml
```
```
network:
  version: 2
  renderer: networkd
  ethernets:
    enp3s0:  # Your network interface name
      dhcp4: no  # Disable DHCP
      addresses:
        - 192.168.1.100/24  # Set your static IP address
      gateway4: 192.168.1.1  # Set your gateway
      nameservers:
        addresses:
          - 8.8.8.8  # Google's DNS
          - 8.8.4.4  # Alternate DNS
```
**3.Apply the Changes**
```
sudo netplan apply
```
**4.Verify the Static IP**

```
ip a
ping 192.168.1.1
```
**5.Revert to DHCP (if needed):** If the static IP configuration doesn't work and you need to revert back to DHCP, just set `dhcp4: yes` in the YAML file, like this:

```
enp3s0:
  dhcp4: yes
```
- Then apply the changes again:

```
sudo netplan apply
```
> Notes

- `ethernets:` The block defines settings for a specific Ethernet interface. Replace `enp3s0` with your actual network interface name.
- `addresses:` The static IP address you want to assign, including the subnet mask (in CIDR notation). For example, `/24` represents a subnet mask of `255.255.255.0`.
- `gateway4:` The IP address of your gateway (usually the IP of your router).
- `nameservers:` The DNS servers to use (Google’s DNS in this example: `8.8.8.8` and `8.8.4.4`).
