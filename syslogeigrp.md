# Configure Devices to Send Syslog Data to Orion for EIGRP Changes
### Variables
ASE link
community_name
EIGRP broadcast address
ip access-list standard 10
ip broadcast
LOCATION_NAME
Orion IP

**Confirm standard seq. 10 does not already exist in access-list**
```
sh ip access-list
```

**Confirm ASE link**

```
! Filter as desired. I.e., sh int gi 0/0/0 | inc desc

sh int gi0/0/0
```

**Confirm loopback EIGRP broadcast doesn't already exist**

```
sh run | section eigrp
```

**If EIGRP broadcast does not exist**

```
conf t

router eigrp 1

!loopback
! local network ip to broadcast
network <ip broadcast> 0.0.0.0

end

wr
```

**If acces-list standard 10 and 10 permit does not exist**

```
conf t

ip access-list standard 10
! To ORION ip
10 permit <ip>

end

wr
```

**Change community BlueVine permissions from RO to RW**

```
conf t

no snmp-server community <community_name> RO

snmp-server community <community_name> RW 10

snmp-server location <LOCATION_NAME>

snmp-server host <Orion IP> <community name>

snmp-server enable traps syslog

end

wr

conf t

! displays hostname rather than ip address

logging origin-id hostname

! source is ASE link

logging source-interface Gi0/0/0

!logging host is Orion
logging host <Orion IP>

end

wr
```
