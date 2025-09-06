The perimeter of the network is specifically important to monitoring the external threats that will be coming to attack your SQL server. 

In larger companies, Network Administration and Security would be in charge of network design and lock-down.

In smaller companies, their may not even be a full time DBA or systems administrator.

**Network Firewalls**

We should have a network firewall of some sort at the perimeter of our network. 

This can be

1. A physical network device like a switch.
2. A software component within the main router.

_Firewall capabilities can be emulated with ACLs, with respect to having more with a firewall implemented alongside ACLs._

Firewalls are important because they assist in preventing DDoS attacks, which occurs when a group of computers, usually zombie computers owned by unsuspecting people being controlled by a hacker, send large numbers of requests to a specific website or network in an attempt to bring the network offline.

Typically, a firewall will sit between the public internet and your border router. 

The border router is a device that sits at the edge of a network between the company's network and the ISP network and handles the routing of data between the public and private internet.

This allows the firewall to protect the internal network from not only the internet but also the border router.

The firewall opens and closes access to objects like SQL servers and can be configured through a series of GRANTs and DENYs i.e. Close all ports and open only necessary ports.

**Note: NAT**

**Network Address Translation**

NAT is used to **allow** mapping from a public IP Address to a private IP Address so that the computers don't need to have a public IP Address.

NAT is often used with Network/IP Masquerading, where a series of computers accesses the public network from a single public IP address.

Communications are established from the private IP Network to the public Internet and are controlled via a stateful translation table as the network packets flow through the router that is performing the masquerading.

### Public IP Addresses vs Private IP Addresses

All IP addresses are not the same; some are route able on the public internet and some are not.

IP addresses that are available **for** use on the **public** internet are issued by the ICANN which regulate strict policies against how many IPs can be used by one party, based on the requirements of the person requesting  the IPs.

**Private** IP sub-net can be used any way for any device, as long as those devices are not directly connected to the internet.

# Finding the Instances#
Before you secure Microsoft SQL server instances, you must search for them. You can simply do this: 
`sqlcmd -L`
*This will query the local IP sub-net for available SQL Server instances.*
There is also power-shell variants of this command
`[System.Data.Sql.SqlDataSourceEnumerator]::Instance.GetDataSources()`
Another technique involves using Server Management Objects(SMOs) in power-shell.
```
[System.Reflection.Assembly]::LoadWithPartialName("Microsoft.
SqlServer.Smo") | out-null
[Microsoft.SqlServer.Management.Smo.SmoApplication]::EnumAvai
lableSqlServers() | ft
```

The above power-shell code requires .NET framework or SMO in order to query for the SQL servers.


Query to find DLLs which have been injected into the SQL server process
```select *
   from sys.dm_os_loaded_modules
   where company <> 'Microsoft Corporation';
```
