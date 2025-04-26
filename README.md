# Configuring-Static-Routes

![Screenshot 2025-04-18 213048](https://github.com/user-attachments/assets/3d23f7ed-cb46-4da5-9803-e877fe6a6f13)

## Lab Overview
- **Lab Purpose:**  
  To understand and implement static routing in a multi-router network using Cisco Packet Tracer. This lab simulates real-world routing configurations where administrators manually define routes for packet delivery.

- **Lab Objective:**  
  - Configure IP addresses on router interfaces.  
  - Establish connectivity between three routers (R1, R2, R3) using static routes.  
  - Verify end-to-end communication between devices in different networks.  

---

## 📚 What is Static Routing?

**Static routing** is a type of network routing technique where routes are manually entered into a router’s routing table by a network administrator. Unlike dynamic routing protocols (such as OSPF or EIGRP), static routes do not change unless manually updated.

### 🔍 Why is Static Routing Important?

- **Simplicity**: Great for small networks where routes don’t change often.
- **Security**: Static routes do not advertise routes over the network, reducing exposure to potential threats.
- **Resource Efficiency**: Uses fewer CPU and memory resources compared to dynamic routing protocols.
- **Predictability**: Administrators have complete control over how packets are routed.

-----

## Router Configuration
You can view the full CLI configuration for the Cisco routers in this [Gist on GitHub](https://gist.github.com/cybererik/671312dbf8936f64d9468071aa3612da).

---
## 📊 Routing Table Snapshots
Routing tables help visualize the paths routers use to forward traffic. After configuring static routes, each router’s routing table updates to include the manually added paths.


## 🖥️ R1 Routing Table

![R1 Screenshot 2025-04-18 214628](https://github.com/user-attachments/assets/572b28b1-913f-4621-b66e-b1f055958e0e)

This static route is added so that **R1** knows how to reach the network `192.168.3.0/24` (where **PC2** is located).
- **R1** is directly connected to **R2** through the `192.168.12.0/24` network.
- **R1** forwards traffic for `192.168.3.0/24` out of its `g0/0` interface.
- Since **R1** doesn’t directly connect to **R3**, it relies on **R2** to handle the next hop.

---

## 🖥️ R2 Routing Table

![R2 creenshot 2025-04-18 214702](https://github.com/user-attachments/assets/dbcf2477-e3fa-4f6f-b45c-7de9b4bf3110)

### Route 1:
```bash
ip route 192.168.1.0 255.255.255.0 g0/0
```
- **Explanation**: This allows R2 to send traffic back to PC1’s network through R1, which connects via the `g0/0` interface.

### Route 2:
```bash
ip route 192.168.3.0 255.255.255.0 g0/1
```
- **Explanation**: This tells R2 how to reach PC2’s network through R3, which is connected via the `g0/1` interface.

### Context:
R2 acts as the central bridge for traffic flowing between R1 and R3. Without these two routes, R2 wouldn’t know where to forward traffic unless it came from directly connected networks.

---

## 🖥️ R3 Routing Table

![R3 Screenshot 2025-04-18 214831](https://github.com/user-attachments/assets/c78d4a2f-14d9-4d28-bdb3-7143726558a4)

### Route Explanation:
This route is necessary for R3 to know how to send packets destined for PC1's network (`192.168.1.0/24`).  
The `g0/0` interface connects to R2, which knows how to reach R1.

### Context:
Even though R3 is only directly connected to R2 and PC2’s network, the static route ensures that return traffic from PC2 to PC1 can be sent back through R2 to R1.

------

## ✅ Verification
Once static routes are configured, test the connectivity using the ping command. If PC1 can successfully ping PC2, it means that the static routing is functioning correctly and traffic is being forwarded as expected.

![Screenshot 2025-04-18 213829](https://github.com/user-attachments/assets/14a628ed-cca0-4486-a23e-a41db820d1e3)
