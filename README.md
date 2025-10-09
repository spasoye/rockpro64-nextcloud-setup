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

### Power Supply

For the power supply, I decided to reuse an old ATX PSU that’s been sitting on a shelf for years, waiting for a second life. It’s a 500W unit, which is complete overkill for the ROCKPro64, but it has plenty of SATA connectors — perfect for powering multiple drives in my setup. Plus, it’s reliable and already tested, so why not put it back to work?
<br>
<div align="center">
    <img src="/pics/IMG_20251008_153928.jpg" width="60%" >
</div>
<br>

To get an ATX PSU to power on without a motherboard, you need to short the green wire (PS_ON) to any ground wire (black) on the main 24-pin connector. A simple jumper wire does the trick.

<br>
<div align="center">
    <img src="/pics/IMG_20251008_161502.jpg" width="40%" >
</div>
<br>

Since the ROCKPro64 runs on 12V DC through a barrel jack, I repurposed one of the CPU power connectors from the PSU.
After checking the pinout a few times (just to be safe!), I crimped it to a barrel jack connector — and it worked perfectly.
Now the board powers directly from the ATX supply, keeping everything neat and powered from a single source.

<br>
<div align="center">
    <img src="/pics/IMG_2025100_barrel_jack.jpg" width="40%" >
</div>
<br>

### 💽 Disks and Assembly

For storage, I wanted to take full advantage of the four SATA ports on the PCIe expansion board.
I connected three HDDs and one SSD, mixing parts I already had lying around. The plan was to have a clean split between data, backups, and container storage:

- 🟥 2× WD Red 1TB HDDs — main data storage
- 🟩 1× WD Green 3TB HDD — dedicated to Borg backups (more on this later)
- 🟥 1× WD Red SSD — for containers and system stuff

<br> <div align="center"> <img src="/pics/IMG_20251005_161620.jpg" width="60%"> </div> <br>

I used disk mounting bracket i salvaged from an old PC to securely fix disk. I also screwed on plexiglass panel with to use it as holder for ROCKpro.

<br>
<p align="center">
    <img src="/pics/bracket_with_rockpro_front.jpg " width="37.5%" >
    <img src="/pics/IMG_20251009_141147.jpg " width="50%" >
</p>
<br>

Every component is mounted on an aluminium sheet that serves as a sturdy base and helps with heat dissipation.
It also keeps the build modular — I can easily detach or rearrange parts if I decide to upgrade or rewire anything later.

<br>
<p align="center">
    <img src="/pics/IMG_20251009_141155.jpg" width="40%" >
    <img src="/pics/back_side.jpg" width="40%" >
</p>
<p align="center">
    <img src="/pics/nas_assembled.jpg" width="40%" >
</p>
<p align="center">
<img src="/pics/nas_in_action.jpg" width="60%" >
</p>
<br>

In the end it’s a bit of a Frankenstein build — old drives, reused PSU, random cables - but that’s part of the fun.
The goal wasn’t to build a shiny commercial NAS, but something modular, hackable, and entirely mine.

