# CCNP ENCOR – Switch Stacking, VSS and StackWise Virtual (Summary)

## Purpose of multi‑switch stacking

* The video explains why modern networks use multi‑switch stacking: linking two or more switches together as a **single logical switch** simplifies management and improves reliability. Without stacking, each physical switch must run protocols like **Spanning Tree** and an **FHRP (HSRP, VRRP etc.)**, and each device must be managed separately. By stacking switches you present a single control and management plane to the rest of the network.
## Traditional switch stacking / StackWise

* **Concept:** Multiple access‑layer switches are connected together with proprietary stack cables, making them behave like one switch. One switch becomes the master; the rest act as members. Users and uplinks can connect to any member and traffic travels across the stack backplane.
* **Platform & architecture:** Cisco’s **StackWise** technology is found on Catalyst 2960, 3650, 3750, 3850 and some Catalyst 9K series switches. It uses special stacking ports and cables, so members must be close together (normally within the same rack).
* **Scaling:** A stack can typically include up to eight or nine switches. It’s easy to add or remove members; the stack automatically elects a new master.
* **Control plane:** There is a single control plane (one switch acts as the master), so the stack is configured and managed as a single switch. There is no dedicated control or management link – control messages use the stack backplane.
* **Use case:** Simple and cost‑effective for access‑layer or small/medium networks that need more ports or local redundancy. Because stacking is easy to modify, it’s ideal for networks expecting frequent upgrades or expansions.
## Virtual Switching System (VSS)

* **Concept:** **VSS** virtualizes two chassis‑based switches (e.g., Catalyst 4500/6500/6800) into one logical switch. Each physical chassis contains its own supervisor engine, but one becomes the **active** supervisor and the other is **standby**. Both chassis present a single MAC address and control plane to the rest of the network.
* **Virtual Switch Link (VSL):** VSS uses high‑bandwidth front‑panel Ethernet ports (typically 10 Gbps) to create the **Virtual Switch Link**, which carries control plane and data plane traffic. Because it uses standard Ethernet ports rather than stacking cables, the two chassis can be located far apart (e.g., in different racks or buildings).
* **Limitations:** VSS supports only **two members**. Converting switches into VSS members requires more complex configuration and planning; after setup, changes are less flexible. However, it delivers high availability and non‑stop forwarding – ideal for core or distribution layers in environments where downtime must be minimized.
* **Use case:** Best suited to large enterprise cores or aggregation layers needing very high throughput and redundancy (e.g., finance or healthcare). Setup and equipment costs are higher and require more advanced expertise.
## StackWise Virtual

* **Evolution of StackWise:** **StackWise Virtual** provides VSS‑like functionality on newer Catalyst 9K switches. Instead of proprietary stacking cables, it uses **10 Gbps or 40 Gbps front‑panel ports** as the **StackWise Virtual Link**, allowing the two switches to be placed further apart (across racks or floors). Each stack has an **active** and **standby** member.
* **Additional links:** Besides the virtual link, StackWise Virtual requires **Dual‑Active Detection (DAD)** links that detect a split‑brain scenario and help the standby take over if the active fails.
* **Licensing/support:** Supported on Catalyst 9500/9600 (16.8.1 or later IOS‑XE) and 9400/9606 with newer firmware. Unlike traditional stacking, there is no special hardware license (base licensing now includes it).
* **Improvements over VSS:** StackWise Virtual is considered the successor to VSS for new platforms. It is built for modern IOS‑XE and includes capabilities like programmability, application visibility and potential MPLS support.
* **Use case:** Recommended for Catalyst 9K deployments where separate chassis need to operate as one logical switch but are not physically co‑located. Provides high availability similar to VSS while simplifying cabling and adding flexibility.
## Comparison and considerations

* **Complexity:** VSS and StackWise Virtual involve more complex setup and planning compared with simple stacking. They require configuration of virtual links and split‑brain detection. Traditional StackWise is easier to deploy and expand.
* **Number of members:** Traditional StackWise supports many switches (up to eight or nine), whereas VSS and StackWise Virtual support only two members.
* **Distance & cabling:** StackWise uses short‑range proprietary cables and is limited to a single rack. VSS and StackWise Virtual use standard 10/40 Gbps Ethernet links so members can be separated by greater distances.
* **Platform support:** Different Cisco platforms support different multi‑chassis technologies; a switch typically supports only one of VSS, StackWise or vPC. StackWise is available on Catalyst 3750/3850/9000; VSS on Catalyst 4500/6500; StackWise Virtual on Catalyst 9K running IOS‑XE.
* **Redundancy & performance:** VSS and StackWise Virtual provide higher availability and are designed for critical core or distribution roles. Traditional stacking offers redundancy, but failure of the master can temporarily disrupt the stack and it may not provide non‑stop forwarding.
* **Choosing a solution:** When selecting among these technologies, consider network size, budget, staff expertise and redundancy requirements. VSS/StackWise Virtual suits large networks demanding minimal downtime and predictable performance; traditional StackWise is sufficient for medium networks where flexibility and ease of expansion are more important.
## Key takeaways

* Stacking technologies consolidate multiple switches into a **single logical switch**, simplifying management and enabling link aggregation across chassis.
* **StackWise** uses proprietary stacking cables; scales to many switches but is limited by distance and is common at the access layer.
* **VSS** combines two high‑end chassis into one logical switch using **Virtual Switch Links**; it’s complex to deploy but provides high availability and is used in core/aggregation networks.
* **StackWise Virtual** is the modern replacement for VSS on Catalyst 9K switches; it uses regular 10/40 Gbps ports, supports long‑distance separation and adds programmability and modern features.

