# Configure Devices to Send Syslog Data to Orion for EIGRP changes
### Variables
ASE link

EIGRP broadcast address

Access-list details

LOCATION_NAME

**Confirm standard seq. 10 does not already exist in access-list**
```
sh ip access-list
```

**Confirm ASE link**

```
! Filter as desired. I.e., sh int gi 0/0/0 | inc desc

sh int gi0/0/0
```

**Confirm loopback eigrp broadcast doesn't already exist**

```
sh run | section eigrp
```

**If EIGRP broadcast does not exist**

```
conf t

router eigrp 1

!loopback

network 172.16.2.40 0.0.0.0

end

wr
```

**If acces-list standard 10 and 10 permit does not exist**

```
conf t

ip access-list standard 10

10 permit 10.2.2.55

end

wr
```

**Change community BlueVine permissions from RO to RW**

```
conf t

no snmp-server community BlueVine RO

snmp-server community BlueVine RW 10

snmp-server location <LOCATION_NAME>

snmp-server host 10.2.2.55 BlueVine

snmp-server enable traps syslog

end

wr

conf t

!displays hostname rather than ip address

logging origin-id hostname

!source is ASE link

logging source-interface Gi0/0/0

!logging host is Orion
logging host 10.2.2.55

end

wr
```
