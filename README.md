# ROCKPro64 Nextcloud setup

A comprehensive, step-by-step journal documenting how to set up a self-hosted Nextcloud server on a **Pine64 ROCKPro64** running Armbian Linux.

This guide walks through:

- Choosing and preparing hardware, including repurposed old PC parts for a DIY NAS
- Installing and configuring Armbian Linux
- Setting up Docker and deploying Nextcloud
- Network setup, security, and SSL configuration
- Troubleshooting and optimization for a privacy-focused, fully functional cloud solution

Perfect for anyone looking to build their own secure, self-hosted cloud storage 
using affordable SBCs and DIY components.

## Motivation

1. **Privacy and Control** – I wanted a cloud solution where my data stays under
     my control, without relying on third-party services.
2. **Repurposing Old Hardware** – I had old PC parts lying around and wanted to 
    give them a second life as a DIY NAS.
3. **Learning and Experimentation** – Setting up Nextcloud on Armbian allowed me
 to deepen my understanding of Linux, Docker, networking, and home server management.
4. **Customizability** – Running my own server gives me the flexibility to tailor
 the system to my needs, from storage structure to security measures.
5. **Cost-Effective Solution** – Using affordable hardware like the Pine64 
ROCKPro64 and existing parts made this a budget-friendly way to host my own cloud.

Building this server has been both a practical project and a learning 
experience, resulting in a secure, private, and fully functional cloud solution 
for personal use.

## Hardware setup

### ROCKPro64

For this project, I chose the Pine64 ROCKPro64 single-board computer as the heart of my self-hosted Nextcloud server.
It consumes far less power than a full PC, making it ideal for 24/7 operation. The ROCKPro64 comes equipped with Gigabit Ethernet, ensuring stable data transfer speeds within the local network, and — crucially — it includes a PCIe slot, offering great flexibility for custom integrations.
In my case, I utilized this slot to add a SATA expansion board for additional storage connectivity.

I opted for the 4 GB RAM version to provide better performance and to accommodate potential future improvements, such as running additional Docker containers or background services alongside Nextcloud.

<br>
<div align="center">
    <img src="/pics/IMG_20251006_165107.jpg" width="60%" >
</div>
<br>

ROCKPro64 doesn’t typically suffer from overheating under light or moderate loads, I decided to mount an active cooler (small fan + heatsink) to ensure stable operation under continuous 24/7 use. Since the board will be running Nextcloud, Docker containers, and other background services, sustained CPU activity can cause temperatures to rise, especially inside an enclosed case or during heavy file transfers.

Adding active cooling helps maintain consistent performance, prevents thermal throttling, and slightly extends the overall lifespan of the board. It’s a small investment for long-term reliability.

<br>
<div align="center">
    <img src="/pics/IMG_20251006_165059.jpg" width="60%" >
</div>
<br>

To make development and troubleshooting easier, I connected a USB-to-UART serial module to the ROCKPro64’s debug header.
This allows direct access to the system’s UART console, which is essential for monitoring boot messages, recovering from misconfigurations, or debugging issues when SSH or network access is unavailable. Detailed description is available [here](https://pine64.org/documentation/ROCKPro64/Getting_started/#setup-a-serial-console-uart2)

<br>
<div align="center">
    <img src="/pics/IMG_20251006_165230.jpg" width="60%" >
</div>
<br>

During the setup process, having serial access proved invaluable, especially when configuring Armbian for the first time and verifying that the system booted correctly from the chosen storage medium (microSD or eMMC). It also provides a simple way to view kernel logs and manage the board before any network configuration is complete.

Although Pine64 has an active community, it doesn’t match the scale of the Raspberry Pi ecosystem. Still, considering its performance-to-price ratio and hardware capabilities, the ROCKPro64 stands out as an excellent alternative for developers seeking more power and flexibility in their self-hosted setups.

### Power supply

I used old ATX power supply thats been lying on shelf waiting to be used again. 
It has plenty of SATA power connectors to power drives and 500 watts of power
which is more than enough to run ROCKPro64, while offering to use it to power 
more devices along the NAS.

<br>
<div align="center">
    <img src="/pics/IMG_20251008_153928.jpg" width="60%" >
</div>
<br>

In order to activate ATX PSU you need to connect green wire to ground wire on 
main power connector.

<br>
<div align="center">
    <img src="/pics/IMG_20251008_161502.jpg" width="40%" >
</div>
<br>

ROCKPro64 needs 12V to power and is equiped with barrel plug so I repurposed 
one CPU power connector and crimped it with barrel jack.

<br>
<div align="center">
    <img src="/pics/IMG_2025100_barrel_jack.jpg" width="40%" >
</div>
<br>