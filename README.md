# Network Design – ACCOUNTS & DELIVERY Departments
## 1. Project Overview

This project demonstrates the design and implementation of a small office network connecting two departments: ACCOUNTS and DELIVERY. The network allows PCs in both departments to communicate with each other, with proper IP addressing, subnetting, and routing.

## 2. Network Requirements
| Requirement          | Implementation                                      |
|---------------------|----------------------------------------------------|
| Departments          | ACCOUNTS & DELIVERY                                |
| PCs per department   | Minimum 2 PCs per department                        |
| Network devices      | Switches and routers to ensure connectivity        |
| Network address      | 192.168.40.0/24                                    |
| Connectivity         | PCs in DELIVERY can ping PCs in ACCOUNTS           |
| Cabling              | Appropriate copper straight-through and crossover cables |


## 3. Network Topology

**Devices Used:**

- 2 Routers (for inter-department communication)  
- 2 Switches (one for each department)  
- 4 PCs (2 per department)  

**Connections:**

1. PCs in each department connect to their respective switch using **Copper Straight-Through cables**.  
2. Switches connect to a **Router interface** using **Copper Straight-Through cables**.  
3. Router interfaces are configured to route traffic between **ACCOUNTS** and **DELIVERY**.
```bash
         [Router]
         /      \
   [Switch1]  [Switch2]
     |           |
[PC1,PC2]    [PC3,PC4]
```

## 4. IP Addressing Scheme
| Device        | Interface           | IP Address       | Subnet Mask       | Default Gateway    |
|---------------|------------------|----------------|-----------------|-----------------|
| Router G0/0   | Router to ACCOUNTS | 192.168.40.1    | 255.255.255.0    | N/A               |
| Router G0/1   | Router to DELIVERY | 192.168.40.129  | 255.255.255.128  | N/A               |
| ACCOUNTS-PC1  | NIC                | 192.168.40.2    | 255.255.255.0    | 192.168.40.1     |
| ACCOUNTS-PC2  | NIC                | 192.168.40.3    | 255.255.255.0    | 192.168.40.1     |
| DELIVERY-PC1  | NIC                | 192.168.40.130  | 255.255.255.128  | 192.168.40.129   |
| DELIVERY-PC2  | NIC                | 192.168.40.131  | 255.255.255.128  | 192.168.40.129   |


**Note:** The network 192.168.40.0/24 is divided into two subnets for the departments:

- **ACCOUNTS:** 192.168.40.0/25  
  - Range: 192.168.40.1 – 192.168.40.126  
  - Broadcast: 192.168.40.127  

- **DELIVERY:** 192.168.40.128/25  
  - Range: 192.168.40.129 – 192.168.40.254  
  - Broadcast: 192.168.40.255


### 5. Router Configuration

Example commands for Router (Cisco IOS CLI):
```bash
enable
configure terminal

# Configure ACCOUNTS interface
interface GigabitEthernet0/0
ip address 192.168.40.1 255.255.255.128
no shutdown

# Configure DELIVERY interface
interface GigabitEthernet0/1
ip address 192.168.40.129 255.255.255.128
no shutdown

# Enable routing (if using static routes, add below)
ip route 192.168.40.0 255.255.255.128 G0/0
ip route 192.168.40.128 255.255.255.128 G0/1

end
write memory
```
## 6. Switch Configuration

No special configuration is required for basic connectivity (default VLAN 1 is used). Just ensure ports connecting to PCs and routers are active.

## 7. Connectivity Testing

**Steps:**

1. Open the **Command Prompt** on DELIVERY-PC1.  
2. Ping ACCOUNTS-PC1:  
   ```bash
   ping 192.168.40.2
   ```

**Repeat for all PCs to ensure full connectivity:**

- DELIVERY-PC1 → ACCOUNTS-PC2
- DELIVERY-PC2 → ACCOUNTS-PC1
- DELIVERY-PC2 → ACCOUNTS-PC2


Expected Result: All pings should succeed with replies confirming network connectivity.

## 8. Cables Used

| Connection           | Cable Type                          |
|---------------------|------------------------------------|
| PC → Switch         | Copper Straight-Through            |
| Switch → Router     | Copper Straight-Through            |
| Switch → Switch (if used) | Copper Crossover (optional if direct connection) |


## 9. Notes & Observations

- Ensure all devices are **powered on** in Packet Tracer.  
- Verify that **IP addresses and subnet masks** are correctly configured.  
- Pinging across subnets requires proper **routing configuration** on the router.  
- VLANs can be added in the future for **departmental isolation** if needed.
