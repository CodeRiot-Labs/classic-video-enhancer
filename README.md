# Classic Video Enhancer

**AI-Powered Restoration & 1080p Remastering Suite for 80s & 90s Music Videos**  
*Developed by [CodeRiot Labs](https://classicvideoenhancer.coderiotlabs.com/)*

![Platform](https://img.shields.io/badge/Platform-Windows%2010%2F11%20(64--bit)-blue)
![Engine](https://img.shields.io/badge/AI%20Engine-PyTorch%20%2B%20Real--ESRGAN-orange)
![Privacy](https://img.shields.io/badge/Operation-100%25%20Offline%20%26%20Private-green)
![Version](https://img.shields.io/badge/Release-v1.0.0-indigo)

**Classic Video Enhancer** is a standalone, 100% offline Windows desktop application engineered to restore, deinterlace, upscale, and reformat classic 4:3 Standard Definition (SD) music videos, DVD `.VOB` rips, and analog tape transfers into crisp, widescreen 1080p HD masters without distorting human proportions or cutting off essential visual details.

---

## Table of Contents

1. [Key Features](#key-features)
2. [Dynamic Hardware Detection & VRAM Protection](#dynamic-hardware-detection--vram-protection)
3. [Installation (Windows 64-Bit)](#installation-windows-64-bit)
4. [Real-World Use Cases](#real-world-use-cases)
5. [Best Practices & Restoration Workflow](#best-practices--restoration-workflow)
6. [Frequently Asked Questions (FAQ)](#frequently-asked-questions-faq)
7. [Building from Source](#building-from-source)
8. [Privacy & Security Guarantee](#privacy--security-guarantee)
9. [Support the Project](#support-the-project)

---

## Key Features

* **Neural Super-Resolution Engine:** Powered by `SRVGGNetCompact` (`realesr-general-x4v3`) running in **FP16 half-precision** on NVIDIA CUDA GPUs (with automatic FP32 fallback on CPU/integrated graphics) to remove compression artifacts, blockiness, and tape softness.
* **Intelligent 4:3 to 16:9 Aspect Ratio Modes:**
  * **1080p — Smart Stretch:** Applies a non-linear cubic polynomial warp that preserves the central 4:3 subject (singers, faces, instruments) in natural proportions while gently stretching the outer edges to fill modern 16:9 TVs with zero black bars.
  * **1080p — Blurred Wings:** Keeps the original 4:3 frame razor-sharp in the center and fills the side pillars with a color-matched, softly blurred ambient backdrop.
  * **1080p — Cinematic Crop:** Scales and crops vertically to deliver a true full-bleed 16:9 cinema frame.
  * **1080p — Pillarbox:** Preserves the exact original 4:3 geometry centered inside a 1920×1080 canvas with pure black sidebars.
  * **2× (`1280×960`) & 4× (`2560×1920`) Native Upscale:** Pure pixel-accurate upscaling maintaining the source aspect ratio.
* **Physical VOB/MPG Packet Scanner:** Automatically scans raw MPEG-2/DVD `.VOB` streams before processing to determine exact physical frame counts and durations, bypassing corrupted DVD cell headers and broadcast TV PTS timestamp offsets.
* **Broadcast-Grade Preprocessing & Encoding:**
  * **Auto YADIF Deinterlacing:** Automatically detects interlaced fields (`tt`, `bb`, `tb`, `bt`) and converts 50i/60i captures into smooth progressive frames.
  * **Pre-Denoise & Post-Sharpen Filters:** Configurable `Off`, `Low`, `Medium`, and `High` stages to clean analog noise before AI upscaling and restore crisp micro-detail afterward.
  * **Archival H.264 + 320 kbps AAC Output:** Encodes `yuv420p` MP4 files with selectable CRF (`16`, `18`, `20`), `+faststart` web/TV streaming flags, and high-bitrate audio muxing.
* **10-Second Instant Preview:** Render a 10-second test clip with your chosen geometry and filter settings before committing to a full-length batch queue.
* **Unattended Batch Queue & System Tray Mode:** Queue multiple videos, monitor live FPS/VRAM telemetry, and minimize to the Windows system tray during overnight remastering runs.

---

## Dynamic Hardware Detection & VRAM Protection

Upon startup, Classic Video Enhancer inspects your system's graphics hardware via CUDA and the Windows Display Registry—without freezing the UI—and automatically configures safe batch limits:

| Detected Hardware Tier | Available VRAM | Default Batch Size | Precision Mode |
| :--- | :--- | :--- | :--- |
| **Enthusiast RTX** (e.g., RTX 4080, 4090, 3090) | `≥ 16.0 GB` | **64 Frames** | CUDA FP16 |
| **Performance RTX** (e.g., RTX 3060 12GB, 4060 Ti, 4070) | `8.0 – 15.9 GB` | **32 Frames** | CUDA FP16 |
| **Entry / Mid-Range GTX & RTX** (e.g., GTX 1650, RTX 3050) | `4.0 – 7.9 GB` | **16 Frames** | CUDA FP16 |
| **Legacy CUDA GPUs** | `< 4.0 GB` | **4 Frames** | CUDA FP16 |
| **Integrated Graphics / CPU Mode** (Intel UHD/Iris, AMD Radeon) | Shared / System RAM | **2 Frames** (1 active) | CPU FP32 |

### Automatic Out-of-Memory (OOM) Recovery
If background applications (browsers, recording tools, games) consume VRAM during a render, the processing pipeline catches `torch.cuda.OutOfMemoryError`, flushes the CUDA cache, automatically halves the active batch size (`16 → 8 → 4 → 2 → 1`), and retries the exact frame slice—ensuring **zero dropped frames and zero crashes**.

---

## Installation (Windows 64-Bit)

Because the installer bundles PyTorch, CUDA runtime libraries, neural network weights, and FFmpeg for **100% offline execution**, it is distributed as a **two-part installer**:

1. Go to the [**Releases**](../../releases) page and download **both** files into the **same folder**:
   * `ClassicVideoEnhancer_Setup.exe` (Installer launcher)
   * `ClassicVideoEnhancer_Setup-1.bin` (Compressed offline engine & AI models)
2. Double-click `ClassicVideoEnhancer_Setup.exe`.
   * *Note on Windows SmartScreen:* Because independent open-source/indie releases do not carry a paid corporate EV certificate, Windows Defender SmartScreen may display *"Windows protected your PC"*. Click **More info → Run anyway** (you can verify file integrity using the SHA-256 hashes published in each Release).
3. Complete the setup wizard. The application installs cleanly into `%LOCALAPPDATA%\ClassicVideoEnhancer` **without requiring Administrator (UAC) privileges**.

---

## Real-World Use Cases

### 1. DVD Music Video Compilation Ripping (`.VOB` / `.MPG`)
* **Challenge:** Classic music video DVDs suffer from 480i interlacing, MPEG-2 macroblocking, and broken timecode headers that cause standard converters to misreport video length or desync audio.
* **Solution:** Load the raw `.VOB` files directly into the queue. Classic Video Enhancer scans the physical stream duration, applies YADIF deinterlacing, cleans MPEG compression artifacts, and outputs a synced 1080p MP4.

### 2. VHS, S-VHS & Hi8 Concert / TV Broadcast Transfers
* **Challenge:** Analog tape captures contain chroma noise, horizontal jitter softness, and 4:3 framing that leaves large black bars on modern widescreen displays.
* **Solution:** Enable **Pre-Denoise: Low/Medium**, select **1080p — Smart Stretch** or **1080p — Blurred Wings**, and generate a **10s Preview** to dial in sharpness before remastering the full tape.

### 3. Retro Music Channel & Smart TV Playback
* **Challenge:** Building a cohesive 80s/90s music video library for Plex, Jellyfin, Kodi, or direct USB Smart TV playback where every clip shares uniform 1080p widescreen framing and normalized AAC audio.
* **Solution:** Queue an entire folder of mixed-resolution clips (`352×240`, `640×480`, `720×480`, `720×576`) and batch-export them with uniform `1080p` geometry and `CRF 18` H.264 encoding.

### 4. Archival Master Preservation
* **Challenge:** Preserving the exact original 4:3 frame geometry without any stretching or cropping while maximizing spatial clarity.
* **Solution:** Use **2× (`1280×960`)** or **4× (`2560×1920`)** with **Maximum (CRF 16)** encoding.

---

## Best Practices & Restoration Workflow

1. **Always Run a 10-Second Preview First:**  
   Click **Generate Preview (10s)** before starting a multi-video queue. Ten seconds is enough to verify deinterlacing smoothness, aspect ratio framing, and denoise/sharpen intensity in under a minute.
2. **Keep Deinterlace on `Auto` Unless Needed:**  
   `Auto (Automatic Detection)` inspects stream field-order metadata and activates YADIF only on interlaced sources, avoiding unnecessary softening on progressive clips.
3. **Avoid Over-Filtering Clean Sources:**  
   * Start with **Pre-Denoise: Off** and **Post-Sharpen: Off**. The Real-ESRGAN `x4v3` model already performs natural denoising and edge reconstruction.
   * Use **Pre-Denoise: Low** only for noisy VHS/TV captures, and **Post-Sharpen: Low** if the source tape was unusually soft. High settings on clean DVDs can create artificial haloing.
4. **Choosing the Right 1080p Geometry:**
   * Use **Smart Stretch** for general music videos with dynamic camera movement.
   * Use **Blurred Wings** when a video features static close-ups, prominent circular logos, or architectural framing where any edge stretch is noticeable.
   * Use **Pillarbox** for purist archival playback.
5. **Recommended Encoding Quality (`CRF`):**
   * **High Quality (CRF 18):** Best balance of visual transparency and file size (recommended for most libraries).
   * **Maximum (CRF 16):** Near-lossless archival master quality.
   * **Balanced (CRF 20):** Smaller file sizes for portable devices or streaming servers.

---

## Frequently Asked Questions (FAQ)

**Q: Why do I need to download both `Setup.exe` and `Setup-1.bin`?**  
**A:** Classic Video Enhancer bundles the full PyTorch CUDA engine, FFmpeg binaries, and AI model weights so it never needs to download gigabytes of dependencies during installation. Windows installers split archives exceeding 2 GB into a launcher (`.exe`) and a data container (`.bin`). Both must sit in the same folder when you run the installer.

**Q: Does this software send my videos or usage data to the cloud?**  
**A:** **No.** Classic Video Enhancer operates 100% locally on your PC. It contains zero telemetry, zero analytics, and zero cloud uploads. Your video files never leave your hard drive.

**Q: Can I run Classic Video Enhancer without an NVIDIA GPU?**  
**A:** **Yes.** If no CUDA-capable NVIDIA GPU is found, the hardware detector identifies your Intel or AMD graphics adapter via the Windows Registry and automatically switches the engine to safe **CPU FP32 mode** (`Batch: 2 Frames`). Rendering will be significantly slower than on an NVIDIA GPU, so using **Generate Preview (10s)** is recommended first.

**Q: Why does the batch size automatically drop during a render?**  
**A:** That is the built-in **VRAM OOM Guard**. If another application spikes GPU memory usage mid-render, Classic Video Enhancer automatically steps down the batch size (e.g., `16 → 8 → 4`) to prevent a crash and finish your video safely.

**Q: Where are my restored videos saved?**  
**A:** By default, outputs are saved to `C:\Users\<YourUsername>\Videos\ClassicVideoEnhancer` with a descriptive suffix (e.g., `MyVideo_SmartStretch_Remaster.mp4`). You can change the destination anytime via the **Output Folder** button.

**Q: How do I update or uninstall the application?**  
**A:** You can uninstall cleanly at any time from **Windows Settings → Apps → Installed apps → Classic Video Enhancer**, or via the Start Menu uninstaller shortcut.

---

## Building from Source

Developers who wish to run or customize the application from source require **Python 3.11 (64-bit)** on Windows:

```cmd
git clone [https://github.com/CodeRiot-Labs/classic-video-enhancer.git](https://github.com/CodeRiot-Labs/classic-video-enhancer.git)
cd classic-video-enhancer

python -m venv .venv
.venv\Scripts\activate

pip install torch torchvision --index-url [https://download.pytorch.org/whl/cu121](https://download.pytorch.org/whl/cu121)
pip install customtkinter numpy pillow pystray pyinstaller
```

Ensure `ffmpeg.exe` and `ffprobe.exe` are placed in the project root and `realesr-general-x4v3.pth` is placed inside `models/`, then run:

```cmd
python main.py
```

To build the standalone executable bundle:

```cmd
pyinstaller --clean --noconfirm ClassicVideoEnhancer.spec
```

---

## Privacy & Security Guarantee

* **Zero External Network Calls:** No internet connection is required or used during video analysis, upscaling, or encoding.
* **No Administrator Elevation Required:** Installs strictly within user space (`%LOCALAPPDATA%`) without modifying system-wide registry keys or system folders.
* **Clean Temporary Storage:** Intermediate raw streams are written to the OS temp directory and automatically purged upon completion or cancellation.

---

## Support the Project

Classic Video Enhancer is developed and maintained by **CodeRiot Labs**. If this tool helped restore your music video collection, consider supporting ongoing development and future updates:

* **Website:** [https://classicvideoenhancer.coderiotlabs.com/](https://classicvideoenhancer.coderiotlabs.com/)
* **Support / Donations:** Visit [classicvideoenhancer.coderiotlabs.com](https://classicvideoenhancer.coderiotlabs.com/) to contribute via PayPal.