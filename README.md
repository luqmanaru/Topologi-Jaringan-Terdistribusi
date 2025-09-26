# Topologi-Jaringan-Terdistribusi

Implementasi infrastruktur jaringan enterprise dengan routing dinamis OSPF

![Cisco](https://img.shields.io/badge/Cisco-Enterprise-blue?style=for-the-badge&logo=cisco&logoColor=white)
![Network](https://img.shields.io/badge/Network-Distributed-green?style=for-the-badge)
![OSPF](https://img.shields.io/badge/OSPF-Dynamic-orange?style=for-the-badge)

---

## 📝 Deskripsi
Proyek ini mengimplementasikan **topologi jaringan terdistribusi** dengan routing dinamis OSPF. Implementasi mencakup:
- Konfigurasi 3 router dengan segmentasi jaringan efisien
- Implementasi routing dinamis OSPF untuk konektivitas end-to-end
- Verifikasi tabel routing dan neighbor adjacency
- Pengujian konektivitas antar segmen jaringan

---

## 🏗️ Topologi Jaringan
| Perangkat | Interface | IP Address | Subnet | Koneksi |
|-----------|-----------|------------|--------|---------|
| **R1** | Gi0/0 | 192.168.1.1 | /24 | Ke PC1, PC2 |
| | Gi0/1 | 10.10.10.1 | /30 | Ke R2 |
| **R2** | Gi0/0 | 10.10.10.2 | /30 | Ke R1 |
| | Gi0/1 | 20.20.20.1 | /30 | Ke R3 |
| | Gi1/0 | 192.168.2.1 | /24 | Ke PC3, PC4 |
| **R3** | Gi0/0 | 20.20.20.2 | /30 | Ke R2 |
| | Gi0/1 | 192.168.3.1 | /24 | Ke PC5, PC6 |
| **PC1** | - | 192.168.1.2 | /24 | Ke R1 |
| **PC2** | - | 192.168.1.3 | /24 | Ke R1 |
| **PC3** | - | 192.168.2.2 | /24 | Ke R2 |
| **PC4** | - | 192.168.2.3 | /24 | Ke R2 |
| **PC5** | - | 192.168.3.2 | /24 | Ke R3 |
| **PC6** | - | 192.168.3.3 | /24 | Ke R3 |

---

## 🖼️ Gambar Topologi
```
    [PC1] [PC2]         [PC3] [PC4]         [PC5] [PC6]
        |   |               |   |               |   |
        +---+---+           +---+---+           +---+---+
            |                   |                   |
          [R1]---------------[R2]---------------[R3]
            |                   |                   |
        10.10.10.0/30       20.20.20.0/30
```
<img width="921" height="465" alt="image" src="https://github.com/user-attachments/assets/f5f463b6-90d5-408e-80d2-51891b376880" />

---

## 📊 Output Verifikasi
### Tabel Routing R1
```
R1# show ip route

Codes: C - connected, S - static, I - IGRP, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
       i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
       * - candidate default, U - per-user static route, o - ODR
       P - periodic downloaded static route

Gateway of last resort is not set

     10.0.0.0/30 is subnetted, 1 subnets
C       10.10.10.0 is directly connected, GigabitEthernet0/1
     20.0.0.0/30 is subnetted, 1 subnets
O       20.20.20.0 [110/2] via 10.10.10.2, 00:00:05, GigabitEthernet0/1
     192.168.1.0/24 is subnetted, 1 subnets
C       192.168.1.0 is directly connected, GigabitEthernet0/0
     192.168.2.0/24 is subnetted, 1 subnets
O       192.168.2.0 [110/2] via 10.10.10.2, 00:00:05, GigabitEthernet0/1
     192.168.3.0/24 is subnetted, 1 subnets
O       192.168.3.0 [110/3] via 10.10.10.2, 00:00:05, GigabitEthernet0/1
```

### OSPF Neighbor R2
```
R2# show ip ospf neighbor

Neighbor ID     Pri   State           Dead Time   Address         Interface
192.168.1.1       1   FULL/BDR        00:00:39    10.10.10.1     GigabitEthernet0/0
192.168.3.1       1   FULL/BDR        00:00:39    20.20.20.2     GigabitEthernet0/1
```

### Konektivitas PC1 ke PC5
```
PC1> ping 192.168.3.2

Pinging 192.168.3.2 with 32 bytes of data:
Reply from 192.168.3.2: bytes=32 time=15ms TTL=253
Reply from 192.168.3.2: bytes=32 time=10ms TTL=253
Reply from 192.168.3.2: bytes=32 time=12ms TTL=253
Reply from 192.168.3.2: bytes=32 time=11ms TTL=253

Ping statistics for 192.168.3.2:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 10ms, Maximum = 15ms, Average = 12ms
```

### OSPF Database R3
```
R3# show ip ospf database

            OSPF Router with ID (192.168.3.1) (Process ID 1)

                Router Link States (Area 0)

Link ID         ADV Router      Age         Seq#       Checksum Link count
192.168.1.1     192.168.1.1     321         0x80000003 0x00C5F3 2
192.168.2.1     192.168.2.1     256         0x80000004 0x00A5B2 3
192.168.3.1     192.168.3.1     198         0x80000003 0x008A91 2

                Net Link States (Area 0)

Link ID         ADV Router      Age         Seq#       Checksum
10.10.10.1      192.168.1.1     321         0x80000001 0x00E7D4
10.10.10.2      192.168.2.1     256         0x80000001 0x00C6A3
20.20.20.1      192.168.2.1     256         0x80000001 0x00A572
```

---
**luqmanaru**  
