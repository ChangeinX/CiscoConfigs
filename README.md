# EPC for CISCO IOS and IOS-XE

**Note**

"The capture can be performed on physical interfaces, sub-interfaces, and tunnel interfaces."
-cisco.com


## Setting Up EPC Configuration CISCO IOS


**Enter Global Configuration Mode**
```
configure terminal

! Access List Filter Goes to Buffer

ip access-list extended PACKET_FILTER

	! sourced from (PDC_ASE-Link)
  
	permit ip host 192.168.66.1 any
  
	! heading to (PDC_ASE-Uplink to Branch)
  
	permit ip any host <ip>
  
! Return to privileged exec mode

exit
!

! Note CISCO can't keep up with where to define bytes 

! and where to define KB in reference to size 

! this example is 512 kilobytes 

! linear goes up to and circular drops oldest packet and adds to

! capture is stored in DRAM

monitor capture buffer CAPTURE size 512 max-size 512 linear

monitor capture buffer CAPTURE filter access-list PACKET_FILTER

! Capture point is ASE-Link interface

monitor capture point ip cef CAP-POINT Gi 0/0 both

! Attaches buffer to capture point

monitor capture point associate CAP-POINT CAPTURE
```

**Start, Stop, Exporting EPC**
```
! Start
monitor capture point start CAP-POINT
! Stop
monitor capture point stop CAP-POINT
! Export
monitor capture buffer CAPTURE export tftp://10.1.10.101/CAPTURE.pcap
```

## Setting Up EPC Configuration CISCO IOS-XE

```
! Privelege Exec Mode 

! interface is PDC_ASE Uplink
  
monitor capture CAP interface GigabitEthernet0/0/0 both
  
! Can be setup inline or an ACL or class-map
  
monitor capture CAP match ipv4 protocol tcp any any
```
**Start, Stop, Exporting EPC**
```
! Start Capture
  
monitor capture CAP start

! Stop Capture
  
monitor capture CAP stop

! Export
  
monitor capture CAP export tftp://10.1.10.101/CAP.pcap
```

## Setting Up TFTP On Remote (Branch) Router
 ```
 configure terminal
 !
 ip tftp source-interface gi 0/0/0
 ```
## Cleaning Up EPC Configuration and Data


**CISCO IOS**
```
no monitor capture point ip cef CAP-POINT Gi 0/0 both
no monitor capture buffer CAPTURE
```
**CISCO IOS-XE**
```
no monitor capture CAP
```
