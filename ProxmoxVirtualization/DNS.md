Building a custom DNS server for a Proxmox-based ethical hacking lab requires creating an isolated network for your virtual machines (VMs) and containers

. A dedicated DNS server within this isolated environment gives you full control over name resolution, allowing you to simulate specific attacks and network configurations safely. 

The best tools for the job

For a secure, isolated lab environment, you should avoid exposing your DNS server to your home network or the public internet. The most common and recommended tools for this setup are open-source software installed on a lightweight Linux Container (LXC) or a virtual machine (VM) inside Proxmox. 

- **BIND9:** The standard and most powerful DNS software. It's a professional-grade nameserver that is ideal for learning about advanced DNS attacks like zone transfers and cache poisoning.
- **Dnsmasq:** A lightweight and easy-to-configure tool that combines DNS and DHCP services. It's perfect for setting up a simple lab network quickly and efficiently.
- **Pi-hole:** A network-wide ad-blocker that can also be used as a custom DNS server. It's user-friendly and offers a web interface for managing records, making it a great option for those who want a visual configuration tool. 

How to set up a DNS server on Proxmox

This guide will focus on setting up a BIND9 server in an Ubuntu LXC container, as it offers the most educational value for ethical hacking.

Step 1: Create an isolated network in Proxmox

First, create a virtual network bridge that is not connected to your internet-facing network.

1. Navigate to **Datacenter > your-node > Network**.
2. Click **Create > Linux Bridge**.
3. Fill in the details:
    - **Name:** A descriptive name, such as `vmbr1`.
    - **IPv4/CIDR:** An unused internal subnet, e.g., `192.168.200.1/24`. This IP will serve as the gateway for your lab VMs.
    - **Autostart:** Enable this option.
4. Click **Create**. 

Step 2: Create a Linux container for your DNS server

Next, set up a minimal Ubuntu LXC to run your DNS software.

1. Download an Ubuntu container template by navigating to **Datacenter > your-node > Local (your storage) > Container Templates**. Click **Templates** and search for `ubuntu-22.04`.
2. Click **Create CT** and configure the following settings:
    - **Hostname:** `dns-server`
    - **Password:** Set a strong password.
    - **Network:** Assign a static IP address within your isolated network (`vmbr1`), e.g., `192.168.200.10/24`. Set the gateway to your network bridge's IP: `192.168.200.1`.
    - **DNS:** Point this container to itself by setting the DNS server to its own static IP, `192.168.200.10`.
3. Leave other settings at their default. When finished, start the new container. 

Step 3: Install and configure BIND9

Inside the newly created Ubuntu container, install and configure BIND9.

1. Access the container's shell in Proxmox.
2. Update the system packages:
    
    sh
    

- ```
    apt update && apt upgrade -y
    ```
    
- Install BIND9:
    
    sh
    
- ```
    apt install bind9 -y
    ```
    
- Edit the primary configuration file, `named.conf.options`:
    
    sh
    
- ```
    nano /etc/bind/named.conf.options
    ```
    
- Add the following lines to configure forwarders and specify the dump file. This allows your lab network to resolve external domains while still being isolated. Use a public DNS server like Cloudflare or Google as an upstream forwarder.
    
    sh
    
- ```
    options {
        directory "/var/cache/bind";
        // If there is a firewall between you and servers, you want
        // to use, you have to use a forwarders block.
        forwarders {
            1.1.1.1;
            1.0.0.1;
        };
        dump-file "/var/cache/bind/dump.db";
        recursion yes;
        allow-recursion { localnets; };
    };
    ```
    
- Edit the main configuration file, `named.conf.local`, to create a custom zone for your lab:
    
    sh
    
- ```
    nano /etc/bind/named.conf.local
    ```
    
- Add a new forward zone for your custom domain (e.g., `lab.local`) and a reverse zone for your subnet:
    
    sh
    
- ```
    zone "lab.local" {
        type master;
        file "/etc/bind/db.lab.local";
    };
    
    zone "200.168.192.in-addr.arpa" {
        type master;
        file "/etc/bind/db.192.168.200";
    };
    ```
    
- Create the forward zone file `/etc/bind/db.lab.local` using the template `db.empty` as a base. Define the records for your DNS server and any other lab hosts.
    
    sh
    

```
cp /etc/bind/db.empty /etc/bind/db.lab.local
nano /etc/bind/db.lab.local
```

Populate the file with your records:

sh

- ```
    $TTL    604800
    @       IN      SOA     dns-server.lab.local. root.lab.local. (
                             2025090401      ; Serial
                                  604800      ; Refresh
                                   86400      ; Retry
                                 2419200      ; Expire
                                  604800 )    ; Negative Cache TTL
    ;
    @       IN      NS      dns-server.lab.local.
    dns-server    IN      A       192.168.200.10
    ```
    
- Create the reverse zone file `/etc/bind/db.192.168.200`:
    
    sh
    

```
cp /etc/bind/db.empty /etc/bind/db.192.168.200
nano /etc/bind/db.192.168.200
```

Populate the file with your records:

sh

- ```
    $TTL    604800
    @       IN      SOA     dns-server.lab.local. root.lab.local. (
                             2025090401      ; Serial
                                  604800      ; Refresh
                                   86400      ; Retry
                                 2419200      ; Expire
                                  604800 )    ; Negative Cache TTL
    ;
    @       IN      NS      dns-server.lab.local.
    10      IN      PTR     dns-server.lab.local.
    ```
    
- Check your configuration for errors with `named-checkconf` and `named-checkzone`, then restart BIND9:
    
    sh
    

```
named-checkconf && named-checkzone lab.local db.lab.local && named-checkzone 200.168.192.in-addr.arpa db.192.168.200
systemctl restart bind9
```

 

Step 4: Add other hosts and test

1. **Create other VMs:** Create additional VMs for your lab (e.g., a vulnerable web server or an attack machine). Connect them to the `vmbr1` network and give them static IPs within your `192.168.200.0/24` subnet.
2. **Point VMs to the custom DNS:** For each new VM, set its DNS server to `192.168.200.10`.
3. **Add records:** Update the `db.lab.local` and `db.192.168.200` zone files on your DNS server with the new VM's hostname and IP address. Remember to increment the serial number.
4. **Test resolution:** From one of the lab VMs, use `dig` or `nslookup` to test that you can resolve both your lab hosts and external domains.
    
    sh
    

```
dig dns-server.lab.local
dig google.com
```

 

Ethical hacking considerations

Having your own DNS server in an isolated lab is beneficial for ethical hacking and penetration testing for several reasons:

- **Simulating real-world scenarios:** A custom DNS server allows for the creation of realistic network environments within your lab, which is crucial for practicing and understanding various cybersecurity concepts and attack vectors.
- **Testing without impacting others:** All network traffic and configurations are contained within your isolated lab, ensuring that any testing or simulation does not affect your home network or external systems.
- **Understanding DNS functionality:** By setting up and managing your own DNS server, you gain a deeper understanding of how DNS works, which is fundamental in cybersecurity.
- **Experimenting with network configurations:** A custom DNS server provides the flexibility to experiment with different network setups and observe how they affect communication and security.
- **Developing defensive skills:** Understanding how DNS can be manipulated in an isolated lab environment can help in developing strategies to defend against such attacks in real-world scenarios.