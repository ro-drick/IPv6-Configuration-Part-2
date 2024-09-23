# Packet Tracer Lab: IPv6 Network Configuration with EUI-64 and Static Routing

## Lab Overview
In this lab, you will configure IPv6 addressing and static routes on a network that already has IPv4 configurations. The focus will be on using the **EUI-64** format to generate interface IDs, enabling IPv6 on router interfaces, and setting up static routes to ensure connectivity between PCs.

### Topology Details:
- **Router 1 (R1):** Cisco 2911
- **Router 2 (R2):** Cisco 2911
- **Switches:** 2 x Cisco 2960-24TT (SW1, SW2)
- **PCs:** PC1, PC2

### Objectives:

1. **Use EUI-64 to Configure IPv6 on G0/1 of R1 and R2**
   - You will first calculate the **EUI-64 interface ID** that will be generated for each interface before configuring the IPv6 address.
   - **Steps:**
     - Obtain the MAC address of G0/1 on both routers:
       ```bash
       R1# show interface g0/1
       R2# show interface g0/1
       ```
     - Use the MAC address to calculate the **EUI-64 interface ID** (insert "FFFE" in the middle of the MAC address and toggle the 7th bit).

   - **Configure the IPv6 address using EUI-64**:
     ```bash
     R1(config)# interface g0/1
     R1(config-if)# ipv6 address 2001:db8::/64 eui-64

     R2(config)# interface g0/1
     R2(config-if)# ipv6 address 2001:db8:0:1::/64 eui-64
     ```

2. **Configure IPv6 Addresses and Default Gateways on PC1 and PC2**
   - Assign appropriate **IPv6 addresses** and **default gateways** for the PCs:
   
   **PC1 (IPv6 Config):**
   - **IPv6 Address:** `2001:db8::2/64`
   - **Gateway:** `2001:db8::` (R1's G0/1 EUI-64 address)

   **PC2 (IPv6 Config):**
   - **IPv6 Address:** `2001:db8:0:1::2/64`
   - **Gateway:** `2001:db8:0:1::` (R2's G0/1 EUI-64 address)

   - Configure these addresses under the **IP Configuration** tab of each PC.

3. **Enable IPv6 on G0/0 of R1 and R2 Without Explicit IPv6 Address**
   - Enable IPv6 on **G0/0** of both routers to support IPv6 routing without manually configuring an IPv6 address:
   ```bash
   R1(config)# interface g0/0
   R1(config-if)# ipv6 enable

   R2(config)# interface g0/0
   R2(config-if)# ipv6 enable
   ```

4. **Configure Static Routes on R1 and R2 to Enable Connectivity Between PC1 and PC2**
   - Configure **static routes** on both R1 and R2 to allow PC1 to ping PC2 and vice versa.
   - Use the `ipv6 route` command with the **help option ('?')** to learn more about its usage.

   **R1 (Static Route to PC2’s Network):**
   ```bash
   R1(config)# ipv6 route 2001:db8:0:1::/64 g0/0
   ```

   **R2 (Static Route to PC1’s Network):**
   ```bash
   R2(config)# ipv6 route 2001:db8::/64 g0/0
   ```

5. **Test Connectivity Between PC1 and PC2**
   - Use the **ping** command to test both **IPv6** connectivity between PC1 and PC2.
     ```bash
     PC1> ping 2001:db8:0:1::2
     PC2> ping 2001:db8::2
     ```

## Key Findings:
- **EUI-64 Interface IDs** were successfully generated based on the router’s MAC address.
- IPv6 routing was enabled on G0/0 of both routers without explicitly assigning an IPv6 address, confirming connectivity.
- **Static routes** were configured correctly on R1 and R2, allowing for successful IPv6 communication between the PCs.

## Commands Summary:

- **Enable IPv6 Routing:**
  ```bash
  R1(config)# ipv6 unicast-routing
  ```

- **Configure IPv6 Address with EUI-64:**
  ```bash
  R1(config-if)# ipv6 address 2001:db8::/64 eui-64
  ```

- **Enable IPv6 Without Address:**
  ```bash
  R1(config-if)# ipv6 enable
  ```

- **Configure IPv6 Static Routes:**
  ```bash
  R1(config)# ipv6 route 2001:db8:0:1::/64 g0/0
  ```

---

This lab demonstrates configuring IPv6 addresses using EUI-64, enabling IPv6 routing on routers, and using static routes to enable communication between two separate IPv6 networks.


## Acknowledgements


Special thanks to **Jeremy's IT Lab** for providing valuable resources and tutorials that greatly contributed to the completion of this exercise. His in-depth explanations and practical demonstrations have been instrumental in enhancing my understanding of Cisco networking concepts and the effective use of Packet Tracer.

For more information and additional resources, visit [Jeremy's IT Lab](https://jeremysitlab.com/) and check out his YouTube for the full course, [Jeremy's IT Lab Free CCNA 200-301 | Complete Course](https://www.youtube.com/playlist?list=PLxbwE86jKRgMpuZuLBivzlM8s2Dk5lXBQ)
