
<div align="center">

# ⚡ Linux Adaptive Optimization Engine

### LAO | Intelligent Linux Optimization Platform

**Understand your hardware. Analyze your system. Optimize with evidence.**

[![Version](https://img.shields.io/badge/version-1.4.0--alpha1-orange.svg)](CHANGELOG.md)
[![Status](https://img.shields.io/badge/status-ALPHA-orange.svg)](docs/IMPLEMENTATION-STATUS-1.4.0-alpha1.md)
[![Language](https://img.shields.io/badge/language-GNU%20Bash-4EAA25.svg?logo=gnubash&logoColor=white)](https://www.gnu.org/software/bash/)
[![Platform](https://img.shields.io/badge/platform-Linux-FCC624.svg?logo=linux&logoColor=black)](https://kernel.org/)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

**A hardware-aware, modular and reversible Linux optimization engine.**

[Overview](#-overview) •
[Features](#-features) •
[Installation](#-installation) •
[Usage](#-quick-start) •
[Architecture](#-architecture) •
[Roadmap](#-roadmap)

</div>

---

> [!WARNING]
> **LAO 1.4.0-alpha1 is an experimental development release.**
>
> The project is under active engineering. Several optimization engines remain diagnostic-only, partially implemented, or pending.
>
> This release does not provide every feature described in the long-term project vision. It does not guarantee performance improvements or universal hardware compatibility.

# ⚡ Overview

The **Linux Adaptive Optimization Engine (LAO)** is an open-source project designed to evolve into a comprehensive Linux system optimization platform.

Its objective is to analyze the actual hardware, kernel, drivers, distribution, storage devices, memory subsystem, network interfaces, system services and desktop environment before selecting appropriate optimization strategies.

Unlike collections of generic Linux tweaks, LAO is designed around capability detection, hardware-specific decisions, controlled modifications, verification and rollback.

The fundamental principle is:

**Never assume a capability exists just because a hardware vendor or device family is recognized. Inspect the actual interfaces exposed by the running system.**

LAO uses information from:

- `/proc`
- `/sys`
- `/etc/os-release`
- CPUFreq interfaces
- Block device interfaces
- Kernel network interfaces
- PCI and DRM device information
- systemd
- Optional diagnostic utilities

The long-term goal is to build an adaptive engine capable of identifying genuine optimization opportunities instead of applying arbitrary parameters to every machine.

# 🎯 Project Mission

LAO aims to provide a unified optimization framework capable of:

- Understanding hardware architecture and capabilities.
- Detecting Linux distributions and kernel configurations.
- Identifying available drivers and kernel interfaces.
- Evaluating memory pressure and reclaim behavior.
- Optimizing supported storage devices.
- Applying GPU-specific policies when supported.
- Evaluating network performance and latency.
- Managing services according to actual system requirements.
- Improving desktop responsiveness without unnecessarily sacrificing visual quality.
- Analyzing boot performance.
- Preparing hardware-aware kernel configurations.
- Building and eventually deploying independent kernels safely.
- Measuring performance before and after modifications.
- Maintaining detailed transaction records.
- Reversing supported changes when necessary.

# 🧠 Engineering Principles

| Principle | Implementation philosophy |
|---|---|
| Hardware awareness | Decisions must reflect the actual hardware and exposed capabilities. |
| Capability detection | Never assume a kernel interface exists. |
| Adaptive optimization | Avoid universal parameters and arbitrary fixed tweaks. |
| Safety first | Preserve system integrity and existing configurations. |
| Reversibility | Track supported modifications and restore previous values conditionally. |
| Measurable results | Distinguish configuration changes from proven performance improvements. |
| Transparency | Explain why a modification is applicable, unavailable, or unnecessary. |
| Modularity | Keep optimization engines independent and maintainable. |
| Distribution awareness | Account for differences between Linux distributions and derivatives. |
| Reliability | Validate inputs, operations and resulting system state. |

# ✨ Features

## Current Capabilities

The following table describes the actual implementation status of version 1.4.0-alpha1.

| Subsystem | Implementation | Current capabilities |
|---|---|---|
| System Detection | Implemented | OS, distribution family, kernel and system inventory. |
| CPU Detection | Implemented | CPU model, topology, features and available interfaces. |
| CPU Optimization | Conditional | CPUFreq governor and EPP when supported. |
| Memory Engine | Partial | PSI, THP, ZRAM, ZSWAP, NUMA and VM diagnostics. |
| Memory Controls | Partial | Manual runtime VM parameter adjustments. |
| Storage Engine | Partial | Device, queue, discard and storage interface diagnostics. |
| I/O Scheduler | Implemented | Manual scheduler selection for an explicitly selected device. |
| GPU Engine | Diagnostic | PCI, DRM and available graphics information. |
| Network Engine | Partial | Network diagnostics and manual TCP congestion control. |
| Service Management | Diagnostic | Selected systemd service, socket and timer inspection. |
| Desktop Engine | Diagnostic | Selected desktop and session information. |
| Boot Engine | Diagnostic | Selected boot and system information. |
| Kernel Builder | Partial | Local kernel preparation and isolated compilation. |
| Transaction System | Implemented for supported writes | Pre-write journal and conditional rollback. |
| CPU Benchmark | Conditional | Repeated CPU measurements through sysbench. |

**Important:** A diagnostic capability is not an implemented optimization.

## ⚙️ CPU Optimization Engine

Current CPU profile support:

- Balanced
- Performance
- Maximum
- Gaming
- Low Latency
- Power Saving
- Custom

Profiles currently target CPUFreq governor and EPP only.

LAO checks the values advertised by the running kernel before attempting a change.

Unsupported values are rejected.

Recognized competing power-management controllers are checked to avoid unnecessary conflicts.

Example:

```bash
./lao --dry-run --optimize --profile gaming
```

Apply after reviewing:

```bash
sudo ./lao --optimize --profile gaming
```

The `performance` governor does not guarantee that every CPU core will remain at maximum frequency.

EPP is not available on every CPU or driver.

## 🧠 Memory Optimization Engine

Current diagnostic coverage includes:

- Memory utilization.
- Pressure Stall Information (PSI).
- Transparent Huge Pages (THP).
- ZRAM.
- ZSWAP.
- NUMA topology.
- Swap information.
- VM parameters.
- Memory-related kernel interfaces.

Current manual runtime controls:

- `vm.swappiness`
- `vm.vfs_cache_pressure`
- `vm.dirty_ratio`
- `vm.dirty_background_ratio`

Example:

```bash
sudo ./lao --dry-run --optimize --vm-swappiness 80
```

The value above is an example of command syntax, not a universal recommendation.

Adaptive ZRAM sizing, ZSWAP management, THP policies, NUMA optimization and reclaim automation remain pending.

## 💾 Storage Optimization Engine

Current diagnostic coverage includes:

- Block device inventory.
- I/O schedulers.
- Queue attributes.
- Read-ahead.
- Discard capabilities.
- Runtime power-related attributes.
- NVMe APST information when exposed.

Manual scheduler selection is supported for explicitly selected devices.

Example:

```bash
sudo ./lao --dry-run --optimize \
  --device sda \
  --io-scheduler mq-deadline
```

Automatic NVMe power management, filesystem tuning and adaptive storage policies remain pending.

## 🎮 GPU Optimization Engine

The long-term GPU engine is designed around separate hardware-specific implementations.

Planned architectures:

- Intel graphics.
- AMD Radeon.
- NVIDIA.

Planned capabilities include:

- Driver identification.
- Graphics acceleration detection.
- Power-state inspection.
- Frequency and temperature monitoring.
- Video acceleration.
- Driver-specific capability analysis.
- Supported performance policies.

**Current release:** diagnostic functionality only. No automatic GPU clock, voltage, overclock or power-profile modification.

## 🌐 Network Optimization Engine

Current diagnostic coverage includes:

- Network interfaces.
- Drivers.
- Offloading capabilities.
- Queue information.
- Ring parameters.
- Interrupt coalescing.
- TCP congestion-control availability.

Manual TCP congestion-control selection is supported when the running kernel advertises the requested algorithm.

Example:

```bash
sudo ./lao --dry-run --optimize \
  --tcp-congestion-control cubic
```

TCP congestion-control changes affect new connections, not existing sessions.

Adaptive MTU, qdisc, buffers, RSS, RPS, XPS and offload policies remain pending.

## 🔧 Kernel Builder

The current Kernel Builder supports:

- Local kernel source trees.
- SHA-256 source-tree fingerprinting.
- Baseline configuration preservation.
- Separate output directories.
- `O=` build workflow.
- `olddefconfig`.
- Build resource checks.
- Explicit compilation confirmation.

Example preparation:

```bash
./lao --kernel-builder \
  --kernel-source "$HOME/src/linux"
```

Example compilation:

```bash
./lao --kernel-build \
  --kernel-source "$HOME/src/linux" \
  --confirm-kernel-build \
  --jobs 4
```

The current builder does not install kernels, generate initramfs, install modules, sign artifacts or modify bootloaders.

# 📦 Installation

## Requirements

- Linux operating system.
- GNU Bash 4.4 or newer.
- Standard GNU utilities.
- `/proc` and `/sys` mounted.

Optional utilities are detected when available.

Examples:

| Category | Optional utilities |
|---|---|
| Hardware | lscpu, lspci, lsusb, dmidecode |
| Sensors | sensors, smartctl, nvme |
| Memory | free, swapon, zramctl |
| Storage | lsblk, findmnt, fstrim |
| Graphics | glxinfo, vulkaninfo, vainfo, nvidia-smi |
| Network | ip, iw, tc, ethtool |
| System | systemctl, systemd-analyze |
| Benchmark | sysbench |
| Kernel build | make, GCC or Clang, bc, flex, bison, perl, python3 |

LAO does not automatically install missing dependencies.

## Installation from release archive

```bash
unzip linux-adaptive-optimization-1.4.0-alpha1.zip

cd linux-adaptive-optimization-1.4.0-alpha1

chmod +x lao tests/run.sh
```

No system-wide installation is required by the current release.

# 🚀 Quick Start

## 1. Display help

```bash
./lao --help
```

## 2. Analyze the system

```bash
./lao --analyze
```

## 3. Generate a report

```bash
./lao --report
```

## 4. List modules

```bash
./lao --list-modules
```

## 5. Inspect individual modules

```bash
./lao --module cpu
./lao --module memory
./lao --module storage
./lao --module gpu
./lao --module network
```

## 6. Preview an optimization

```bash
./lao --dry-run --optimize --profile gaming
```

## 7. Apply an optimization

```bash
sudo ./lao --optimize --profile gaming
```

Always inspect the proposed changes before applying them.

# ↩️ Rollback System

LAO implements a conditional rollback mechanism for its supported runtime modifications.

Global rollback:

```bash
sudo ./lao --rollback
```

Module-specific rollback:

```bash
sudo ./lao --rollback-module cpu
sudo ./lao --rollback-module memory
sudo ./lao --rollback-module storage
sudo ./lao --rollback-module network
```

The transaction system records previous and requested values before supported modifications.

Rollback checks the current value before restoring the original one.

If another application or administrator has changed the value afterward, LAO preserves the external change instead of blindly overwriting it.

Current rollback support is limited to the implemented allowlisted runtime interfaces. It is not a general-purpose system snapshot or filesystem recovery mechanism.

# 🛡️ Safety Architecture

LAO follows a conservative operational model:

1. Detect the target hardware and operating system.
2. Inspect actual kernel capabilities.
3. Validate the requested operation.
4. Read the original value.
5. Record the transaction intent.
6. Synchronize the journal.
7. Apply the supported runtime modification.
8. Read the result again.
9. Verify the resulting state.
10. Preserve enough information for conditional rollback.

Additional safeguards include:

- Input validation.
- Restricted writable interfaces.
- Explicit user intent.
- Conflict-aware rollback.
- Root-owned transaction state.
- No automatic package installation.
- No silent kernel replacement.
- No automatic bootloader modification.

# 🧪 Testing

Run the test suite:

```bash
bash -n lao lib/*.sh tests/*.sh

bash tests/run.sh
```

The 1.4.0-alpha1 test report records 21 passing automated test groups in an isolated Debian 13 container.

Tests cover:

- CLI parsing.
- Profile validation.
- Distribution detection fixtures.
- CPUFreq and EPP logic.
- Transaction journaling.
- Manual VM controls.
- Storage scheduler validation.
- TCP congestion-control validation.
- Rollback.
- Conflict preservation.
- Kernel source fingerprinting.
- Kernel Builder preparation.
- Report generation.

Some tests use simulated sysfs and procfs files.

Passing simulated tests does not prove that a feature works on every physical machine.

## Validation limitations

The recorded test environment did not validate:

- Real hardware CPUFreq writes.
- Real GPU optimization.
- Real NVMe power management.
- Real storage performance improvements.
- Real network performance improvements.
- Real kernel installation.
- Bootloader integration.
- Physical hardware stability across distributions.

No performance gains are claimed without comparable measurements.

# 🗺️ Roadmap

The complete LAO engineering vision consists of nine optimization engines.

| Engine | Long-term objective | Current status |
|---|---|---|
| Memory Optimization | ZRAM, ZSWAP, THP, NUMA, reclaim and adaptive memory policies | Partial |
| Storage Optimization | NVMe, SATA, HDD, TRIM, power and filesystem policies | Partial |
| GPU Optimization | Intel, AMD and NVIDIA hardware-specific tuning | Diagnostic |
| Network Optimization | Latency, throughput, buffers, qdisc and offloading | Partial |
| Intelligent Services | Dependency-aware service classification and management | Diagnostic |
| Desktop Optimization | KDE, GNOME, XFCE and other desktop-specific policies | Diagnostic |
| Boot Optimization | Boot analysis, initramfs and safe boot configuration | Diagnostic |
| Kconfig Engine | Microarchitecture-aware kernel configuration | Pending |
| Kernel Deployment | Parallel installation, initramfs, DKMS and bootloader integration | Pending |

## Planned intelligent profiles

- Gaming
- Desktop
- Workstation
- Low Latency
- Power Saving
- Multimedia
- Compilation
- General Purpose

These are long-term profile goals. They must adapt to actual hardware and workload instead of applying fixed collections of tweaks.

# 🐧 Distribution Support

Current distribution-family detection recognizes:

| Family | Examples |
|---|---|
| Arch-based | Arch Linux, Manjaro, BigLinux, CachyOS and derivatives |
| Debian-based | Debian, Ubuntu and derivatives |
| Fedora-based | Fedora and RHEL-related derivatives |

Detection does not imply that every derivative has been independently tested.

Package-manager recognition is informational. LAO does not install packages or modify repositories.

# 🏗️ Architecture

```text
Linux Adaptive Optimization Engine
│
├── lao
│   └── Command-line interface
│
├── lib/
│   ├── common.sh
│   │   └── Shared utilities and transaction preparation
│   │
│   ├── detect.sh
│   │   └── Hardware and system detection
│   │
│   ├── modules.sh
│   │   └── Diagnostics and supported runtime controls
│   │
│   ├── state.sh
│   │   └── Transaction journal and rollback
│   │
│   └── kernel_builder.sh
│       └── Isolated kernel preparation and compilation
│
├── config/
│   └── default.conf
│
├── docs/
│   ├── ARCHITECTURE.md
│   ├── AUDIT-1.3.0.md
│   ├── COMPATIBILITY.md
│   ├── IMPLEMENTATION-STATUS-1.4.0-alpha1.md
│   ├── FILE-CHANGES-1.4.0-alpha1.md
│   ├── KNOWN-LIMITATIONS.md
│   ├── RESEARCH.md
│   └── TEST-REPORT.md
│
└── tests/
    └── run.sh
```

# 📚 Documentation

Detailed project documentation:

- [Architecture](docs/ARCHITECTURE.md)
- [1.3.0 Baseline Audit](docs/AUDIT-1.3.0.md)
- [Implementation Status](docs/IMPLEMENTATION-STATUS-1.4.0-alpha1.md)
- [File Changes](docs/FILE-CHANGES-1.4.0-alpha1.md)
- [Compatibility Matrix](docs/COMPATIBILITY.md)
- [Research Notes](docs/RESEARCH.md)
- [Known Limitations](docs/KNOWN-LIMITATIONS.md)
- [Test Report](docs/TEST-REPORT.md)
- [Changelog](CHANGELOG.md)

# 🤝 Contributing

Contributions are welcome.

Before submitting changes:

1. Inspect the existing architecture.
2. Preserve working functionality.
3. Avoid duplicating existing implementations.
4. Detect real kernel capabilities instead of assuming them.
5. Validate all user-provided values.
6. Add tests for successful and failed operations.
7. Test rollback and external-conflict handling.
8. Document hardware and distribution limitations.
9. Separate simulated tests from physical-hardware validation.
10. Never claim performance gains without comparable benchmarks.

New optimization engines must integrate with the existing architecture, transaction system and validation methodology.

# 🐛 Bug Reports

When reporting a problem, provide:

- LAO version.
- Linux distribution.
- Kernel version.
- CPU and GPU models.
- Exact command executed.
- Complete error output.
- Relevant diagnostic report.
- Whether the system is physical, virtualized or containerized.

Do not publish private serial numbers, MAC addresses, public IP addresses or other sensitive system information.

# 📜 License

This project is distributed under the MIT License.

See [LICENSE](LICENSE).

# ⚖️ Disclaimer

LAO is experimental system software.

Kernel and system-level changes can affect:

- Stability.
- Performance.
- Power consumption.
- Thermal behavior.
- Storage latency.
- Network behavior.
- Boot reliability.

Always inspect proposed changes, preserve backups and maintain a known-good recovery path.

No universal performance improvement or compatibility with every Linux system is guaranteed.

---

<div align="center">

### Linux Adaptive Optimization Engine

**Understand the hardware. Optimize with evidence. Preserve control.**

</div>
