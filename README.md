# Classic Video Enhancer

![Status](https://img.shields.io/badge/Status-Pre--Release%20%2F%20In%20Development-0ea5e9?style=flat-square)
![Platform](https://img.shields.io/badge/Platform-Windows%2010%2F11%20(x64)-1e293b?style=flat-square)
![Execution](https://img.shields.io/badge/Execution-100%25%20Offline-10b981?style=flat-square)
![Telemetry](https://img.shields.io/badge/Telemetry-Zero-64748b?style=flat-square)

**Autonomous, offline AI video super-resolution engineered for legacy footage and analog archives.**

---

### Release Status

> **Current Stage:** Active Pre-Release Refinement (`v1.0.0-dev`)  
> The standalone Windows installer is currently undergoing final optimizations and feature adjustments before public distribution. Once finalized, the compiled binary and its cryptographic checksums will be published directly in the [**Releases**](https://github.com/CodeRiot-Labs/classic-video-enhancer/releases) section and on the official [**CodeRiot Labs**](https://coderiotlabs.com) portal.

---

### Overview

**Classic Video Enhancer** is a native Windows desktop utility built to restore, sharpen, and upscale low-resolution videos directly on local hardware. Powered by deep neural networks and accelerated half-precision inference, it executes frame-by-frame enhancement pipelines without external cloud subscriptions, upload queues, ads, or telemetry.

### Core Capabilities

* **Real-ESRGAN & PyTorch FP16 Inference:** Leverages deep neural networks trained to eliminate compression artifacts, reconstruct high-frequency textures, and sharpen edges on degraded video signals.
* **100% Offline & Local-First Privacy:** Video files never leave your local storage. All tensor processing runs directly on your machine with zero remote server dependencies.
* **Frame-Accurate FFmpeg Pipeline:** Automated stream demuxing and master audio remuxing preserve original audio tracks, multi-channel layouts, and exact timestamps without manual synchronization.
* **VRAM Memory Guard:** Tile-based frame rendering enables 4x upscaling workflows on consumer systems and mid-tier GPUs without memory allocation crashes.

### Target Workflows

* **Analog Cassette & VHS Restoration:** Reduce tape degradation, color bleed, and compression noise from digitized family tapes and vintage broadcasts.
* **Classic Animation & Line Art:** Upscale vintage hand-drawn animation, cartoons, and anime while keeping line work crisp and free of ringing halos.
* **Legacy Gaming & Early Digital Video:** Enhance standard-definition 480p and 720p captures, retro console recordings, and early digital camera footage.

### System Requirements (Target)

| Component | Minimum / Recommended |
| :--- | :--- |
| **OS** | Windows 10 or Windows 11 (64-bit) |
| **Processor** | Multi-core x64 CPU (Intel Core i5 / AMD Ryzen 5 or newer) |
| **Memory** | 8 GB RAM minimum (16 GB recommended) |
| **Graphics** | NVIDIA GPU with CUDA support recommended for FP16 acceleration (CPU fallback supported) |
| **Storage** | ~2.5 GB free disk space for application runtime and temporary frame buffers |

### Security & Binary Integrity Policy

When `v1.0.0` is published, every official release distributed through this repository will include:
1. **SHA-256 Checksum Digest** published alongside the installer and on `coderiotlabs.com`.
2. **VirusTotal Security Report** link for transparent binary inspection.
3. **Unsigned Build Notice:** As an independent experimental release without a commercial code-signing certificate, Windows SmartScreen may display an *"Unknown publisher"* prompt on first launch. Always verify the SHA-256 hash prior to execution.

---

### Support Independent R&D

Classic Video Enhancer is distributed free of charge with zero subscriptions or telemetry. If you would like to support ongoing optimization and funding for an official Windows code-signing certificate:

* **Web Portal:** [coderiotlabs.com](https://coderiotlabs.com)
* **Support on Ko-fi:** [ko-fi.com/coderiotlabs](https://ko-fi.com/coderiotlabs)

---

### Experimental Software Disclaimer

*Classic Video Enhancer is an experimental, non-commercial software project developed by **CodeRiot Labs** for research, personal media preservation, and technical exploration. It is provided "as-is" without warranties of any kind, either expressed or implied, and is not licensed for commercial resale or SaaS redistribution.*
