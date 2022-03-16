# Sending Sample Data to Orion for VoIP traffic

### Variables
```
ASE_IP
ASE_Uplink_Interface
source_ip
interface_ip
Orion_IP
community_name
```
**Configuration**
```
ip sla 10
udp-jitter <ASE_IP> 13684 source-ip <source_ip> codec g711ulaw codec-numpackets 100 codec-size 80
tos 184
threshold 500
timeout 500
frequency 10
history enhanced interval 60 buckets 60
ip sla schedule 10 life forever start-time now
logging history size 400
logging origin-id hostname
logging source-interface <ASE_Uplink_Interface>
logging host <interface_ip> transport udp port 516
logging host <Orion_IP>
ipv6 ioam timestamp
!
!
snmp-server community <community_name>
snmp-server location <location_name>
snmp-server host <Orion-IP> <community_name>
```
