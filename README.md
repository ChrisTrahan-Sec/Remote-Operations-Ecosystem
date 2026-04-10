# Remote-Operations-Ecosystem

**Status:** Fully Operational

## 1. Project Objective

The primary goal of this project was to architect a complete ecosystem that enables full, high-performance remote management of a powerful home lab from a low-power, portable, and off-grid mobile client. The architecture is explicitly designed to leverage the raw hardware performance of a central workstation, minimizing the computational and power load on the mobile client to maximize battery longevity and overall system efficiency.

## 2. System Architecture: A Hub-and-Spoke Model

The system operates on a three-tiered model, using a powerful central workstation as a secure "jump box" or "bastion host." This allows the lightweight laptop in the backpack to function as a highly efficient thin client, offloading all intensive tasks to the main workstation.

**`Backpack Client -> Secure Overlay Network -> Primary Workstation -> Internal Homelab`**

## 3. The Mobile Client: Tactical Uplink Backpack

This is the physical mobile platform for remote operations, custom-fabricated for this specific use case.

*   **Chassis & Fabrication:** A ruggedized Westward tool backpack serves as the chassis, heavily modified with structurally integrated control surfaces. This includes a flush-mounted Eaton breaker bank with a custom faceplate and a swing-out component panel fabricated from repurposed materials for at-a-glance access.
*   **Power System:** A custom-designed **320Wh LiFePO4 parallel battery system** provides the core power. The system includes:
    *   A 1200W Pure Sine Wave Inverter for clean AC power.
    *   An integrated MPPT controller with a retractable 50W solar panel input.
    *   A custom-engineered external monitoring window, repurposing a clear ID holder to allow at-a-glance status checks of the MPPT controller's display without opening the main compartment.
*   **Payload:**
    *   **Computer:** Dell Latitude 7420 (16GB RAM, Core i7, 1TB M.2 SSD)
    *   **Uplink:** Netgear Nighthawk M6 Pro (AT&T Business Plan) with 12 dBi high-gain antennas.
    *   **I/O:** Dell WD22 Thunderbolt 4 Dock for single-cable connectivity to all peripherals.

## 4. The Secure Network Fabric: Tailscale

The critical link between the mobile client and the home lab is a **Tailscale secure overlay network.**

*   **Function:** Tailscale creates a zero-configuration, point-to-point WireGuard mesh network. This makes the primary workstation securely accessible from anywhere in the world on any network, without requiring complex firewall rules, port forwarding, or static IP addresses.
*   **Implementation:** Tailscale is installed on the Dell Latitude laptop and the primary Windows workstation, creating a persistent and secure virtual private network that authenticates via a private Google Workspace account.

## 5. The Remote Access Stack: Sunshine & Moonlight

To achieve a low-latency, "like-local" desktop experience, a performance-tuned streaming stack is used.

*   **Host Server (Sunshine):** Installed on the primary Windows 11 Pro workstation (`build-001`). Sunshine is an open-source, self-hosted implementation of NVIDIA's GameStream protocol, chosen for its high performance, extensive configuration options, and hardware-accelerated encoding capabilities.
*   **Remote Client (Moonlight):** Installed on the Dell Latitude laptop. Moonlight is an ultra-lightweight client that is hardware-accelerated to decode the incoming stream from Sunshine with minimal CPU/GPU impact.
*   **Performance Tuning:** Both Sunshine and Moonlight have been extensively tweaked for this application. Settings for bitrate, resolution, codec, and frame rate are optimized to deliver a smooth, responsive desktop experience while minimizing bandwidth consumption and client-side power draw.

## 6. The Homelab Infrastructure

The mobile system provides secure access to a complete homelab environment.

*   **Primary Workstation (`build-001`):** A custom-built, high-performance PC (Windows 11 Pro) that serves as the Sunshine host and the primary management "jump box."
*   **Network Attached Storage (NAS):** A multi-drive server running Ubuntu Server, providing centralized file storage and hosting for other services like Docker containers.

## 7. Management Workflow

The architecture enables a highly efficient workflow. From the backpack, I initiate a single Moonlight session to the `build-001` workstation. From that remote desktop, I can open separate SSH sessions, remote desktop clients, or other management tools to control every other device in the homelab. This allows me to seamlessly `alt+tab` between controlling the Ubuntu NAS, the Windows workstation, and other virtual machines as if I were sitting directly at the server rack, all while leveraging the main build's raw hardware performance.