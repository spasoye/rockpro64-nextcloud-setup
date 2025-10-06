# ROCKPro64 Nextcloud setup

A comprehensive, step-by-step journal documenting how to set up a self-hosted Nextcloud server on a **Pine64 ROCKPro64** running Armbian Linux.

This guide walks through:

- Choosing and preparing hardware, including repurposed old PC parts for a DIY NAS
- Installing and configuring Armbian Linux
- Setting up Docker and deploying Nextcloud
- Network setup, security, and SSL configuration
- Troubleshooting and optimization for a privacy-focused, fully functional cloud solution

Perfect for anyone looking to build their own secure, self-hosted cloud storage using affordable SBCs and DIY components.

## Motivation

1. **Privacy and Control** – I wanted a cloud solution where my data stays under my control, without relying on third-party services.
2. **Repurposing Old Hardware** – I had old PC parts lying around and wanted to give them a second life as a DIY NAS.
3. **Learning and Experimentation** – Setting up Nextcloud on Armbian allowed me to deepen my understanding of Linux, Docker, networking, and home server management.
4. **Customizability** – Running my own server gives me the flexibility to tailor the system to my needs, from storage structure to security measures.
5. **Cost-Effective Solution** – Using affordable hardware like the Pine64 ROCKPro64 and existing parts made this a budget-friendly way to host my own cloud.

Building this server has been both a practical project and a learning experience, resulting in a secure, private, and fully functional cloud solution for personal use.

## Hardware setup

For this project, I chose the Pine64 ROCKPro64 single-board computer as the heart of my self-hosted Nextcloud server.
It consumes far less power than a full PC, making it ideal for 24/7 operation. The ROCKPro64 comes equipped with Gigabit Ethernet, ensuring stable data transfer speeds within the local network, and — crucially — it includes a PCIe slot, offering great flexibility for custom integrations.
In my case, I utilized this slot to add a SATA expansion board for additional storage connectivity.

I opted for the 4 GB RAM version to provide better performance and to accommodate potential future improvements, such as running additional Docker containers or background services alongside Nextcloud.

Although Pine64 has an active community, it doesn’t match the scale of the Raspberry Pi ecosystem. Still, considering its performance-to-price ratio and hardware capabilities, the ROCKPro64 stands out as an excellent alternative for developers seeking more power and flexibility in their self-hosted setups.
