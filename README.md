# lesson5
## 1. Network Topologies

## 1. Network Topologies

### 1.1 Physical Topology
This diagram illustrates the physical distribution of network hardware across functional zones within the apartment. 
* **Core Infrastructure:** The **Storage Room** serves as the Main Distribution Frame (MDF), safely housing the primary ISP Giga Hub (Router/Modem combo).
* **Wired Backbone:** The **Den** acts as the primary workstation area. To ensure maximum throughput and low latency, both **Laptop A and Laptop B** are hardwired directly to the router using **Cat6 Ethernet** cables.
* **Wireless Distribution:** The network utilizes dual-band Wi-Fi strategically. The **Living Room** relies on the **2.4GHz band** to provide stable, wall-penetrating coverage for IoT devices (Security Camera and Robot Vacuum). Meanwhile, the **5GHz band** provides high-bandwidth wireless access for the **Wireless Printer** in the Den and mobile **Smartphones**.

![Physical Topology](physical-topology.png)



### 1.2 Logical Topology
This diagram details the logical architecture, data flow, and Layer 3 boundaries of the Local Area Network (LAN).
* **Addressing Schema:** The network operates on a flat IPv4 architecture utilizing the private **192.168.1.0/24** subnet (Subnet Mask: 255.255.255.0).
* **Gateway & Routing:** The primary router acts as the default gateway (`192.168.1.1`), handling all NAT (Network Address Translation) routing to the public Internet cloud.
* **IP Allocation Strategy:** The topology visually distinguishes the IP assignment methods. Infrastructure and shared devices (like the Security Camera at `.80` and Printer at `.50`) use **Static IPs or DHCP Reservations** for consistent access, while mobile and user endpoints dynamically obtain addresses from the router's **DHCP Pool**.

```mermaid
flowchart LR
    %% 定义颜色样式，完美匹配你物理图里的区域颜色
    classDef routerZone fill:#fff3e0,stroke:#f57c00,stroke-width:2px
    classDef blueZone fill:#e1f5fe,stroke:#0288d1,stroke-width:2px
    classDef greenZone fill:#e8f5e9,stroke:#388e3c,stroke-width:2px
    classDef grayZone fill:#f5f5f5,stroke:#9e9e9e,stroke-width:2px
    classDef internet fill:#eceff1,stroke:#607d8b,stroke-width:2px

    %% 核心网络层
    Internet(("Internet Cloud")):::internet --> Modem["ISP Modem<br>Bridge Mode"]:::internet
    Modem --> Router{"Giga Hub Router<br>Gateway: 192.168.1.1<br>Subnet: /24"}:::routerZone

    %% 蓝色区域设备 (Den / Workstation)
    Router -->|Eth0: .10| PCA["Laptop A<br>IP: 192.168.1.10<br>DHCP Res"]:::blueZone
    Router -->|Eth0: .11| PCB["Laptop B<br>IP: 192.168.1.11<br>DHCP Res"]:::blueZone
    Router -.->|"Wi-Fi 5GHz: .50"| Print["Wireless Printer<br>IP: 192.168.1.50<br>DHCP Res"]:::blueZone

    %% 绿色区域设备 (Living Room / IoT)
    Router -.->|"Wi-Fi 2.4GHz: .80"| Cam["Security Camera<br>IP: 192.168.1.80<br>Static"]:::greenZone
    Router -.->|"Wi-Fi 2.4GHz: .100"| Vac["Robot Vacuum<br>IP: 192.168.1.100<br>DHCP"]:::greenZone

    %% 灰色区域设备 (Mobile)
    Router -.->|"Wi-Fi 5GHz: .21-24"| Phone["4x Smartphones<br>IP: 192.168.1.21-24<br>DHCP Pool"]:::grayZone
```

## 3. Addressing Documentation
*Note: For privacy, MAC addresses are partially masked, and standard private IP ranges are used for this documentation.*

| Device Name | Location | Interface | IP Address | Assignment Type |
| :--- | :--- | :--- | :--- | :--- |
| **Main Router** | Storage Room | WAN / LAN | 192.168.1.1 | Static |
| **Laptop A** | Den | Ethernet (Eth0) | 192.168.1.10 | DHCP Reservation |
| **Laptop B** | Den | Ethernet (Eth0) | 192.168.1.11 | DHCP Reservation |
| **Security Camera**| Living Room | Wi-Fi (2.4GHz) | 192.168.1.80 | Static |
| **Wireless Printer**| Den | Wi-Fi (5GHz) | 192.168.1.50 | DHCP Reservation |
| **Smartphones (x4)**| Mobile | Wi-Fi (5GHz) | 192.168.1.21-24| Dynamic DHCP |
| **Robot Vacuum** | Living Room | Wi-Fi (2.4GHz) | 192.168.1.100| Dynamic DHCP |

## 4. Network Configuration Details
* **Core Gateway**: ISP-provided Giga Hub acting as the primary DHCP server and NAT Firewall.
* **DNS Strategy**: Configured to use **Google DNS (8.8.8.8 / 8.8.4.4)** for improved reliability and resolution speed.
* **DHCP Pool**: Configured from **.20 to .254**. This range supports the dynamic allocation for Smartphones (.21-24) and the Robot Vacuum (.100), while leaving room for future guest devices.
* **Wireless Security**: Industry-standard **WPA2-AES (CCMP)** encryption is active on both 2.4GHz and 5GHz SSIDs to ensure data privacy.

## 5. Services & Security
* **Surveillance**: The Security Camera (.80) is set with a Static IP to ensure a consistent RTSP stream for monitoring.
* **Credential Management**: I use **Bitwarden**, an industry-standard encrypted password manager, to secure all administrative credentials. Access is protected by a strong Master Password and TOTP-based Two-Factor Authentication (2FA).

## 6. Secure Credential Management
I use **Bitwarden**, an industry-standard encrypted password manager, to store and manage all administrative credentials for the network devices. 
* **Security Measures**: Access is protected by a strong Master Password and Time-based One-Time Password (TOTP) 2FA.
* **Policy**: No plain-text passwords or sensitive configuration keys are included in this public documentation.
