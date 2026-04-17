# 🐧 Optimized Linux Kernels

Custom Linux kernel builds optimized for specific hardware targets to prioritize performance, stability, and security.

## 📱 Supported Platforms

- **Acer Switch One 10 (SW1-011)**: Intel Atom x5-Z8300 (Silvermont)
- **Samsung NP300E4C-A0FCL**: Intel Celeron B820 (Sandy Bridge)

## 📊 Build Status

![Build Status](https://img.shields.io/github/actions/workflow/status/CypherNoodle/Releases-Kernel/build-kernel.yaml?branch=main&style=for-the-badge&logo=github-actions&logoColor=white)

## 🚀 Quick Start

### Build Kernel

Click the button below to build and release a new kernel version:

[![Build Kernel](https://img.shields.io/badge/Build-Kernel-blue?style=for-the-badge&logo=github-actions&logoColor=white)](https://github.com/CypherNoodle/Releases-Kernel/actions/workflows/build-kernel.yaml)

Or manually trigger the workflow:

1. Go to [Actions](https://github.com/CypherNoodle/Releases-Kernel/actions/workflows/build-kernel.yaml)
2. Click **"Run workflow"**
3. Select branch `main`
4. Optionally check **"Mark as pre-release"**
5. Click **"Run workflow"** button

## 📦 Download Latest Release

[![Latest Release](https://img.shields.io/github/v/release/CypherNoodle/Releases-Kernel?style=for-the-badge&logo=linux&logoColor=white)](https://github.com/CypherNoodle/Releases-Kernel/releases/latest)

## ⚙️ Build Information

- **Target Platforms:**
  - Acer SW1-011: Silvermont (`-march=silvermont`)
  - Samsung NP300E4C: Celeron B820 (`-march=sandybridge`)
  -
- **Compiler Optimizations:** `-O2 -pipe -fno-strict-aliasing`
- **Build System:** GitHub Actions with advanced caching
- **Linker:** Mold (ultra-fast, 3-5x faster than GNU ld)
- **Automatic Builds:** Every 5 days (maintains warm cache)
- **Debug Info:** Disabled (faster builds, smaller packages)

### Build Times

#### Optimized Performance (with tmpfs + mold linker + ccache)

| Build Type | Duration | Cache Hit Rate | Notes |
|------------|----------|----------------|-------|
| First build (cold cache) | ~40-45 min | 0% | Full compilation in RAM |
| Subsequent builds (warm cache) | ~6-8 min | 85-90% | Reusing most compiled files |
| Minor version change (rc2 → rc3) | ~25-30 min | 50-70% | Partial recompilation |
| Patch version (6.19.0 → 6.19.1) | ~12-18 min | 70-85% | Few changes |
| Major version (6.x → 7.x) | ~35-40 min | 40-60% | Significant changes |

> **Real example:** Build 6.19.0-rc3 with 51% cache (from rc2) took 88 minutes without optimizations. With current optimizations (tmpfs + mold + ccache), the same build takes ~25-30 minutes (65% improvement).

> **tmpfs boost:** Building in RAM eliminates disk I/O bottleneck, providing 15-20% speed improvement on top of other optimizations.

#### Performance Optimizations Applied

- 🚀 **tmpfs Build Directory**: Entire build runs in RAM (6GB tmpfs) - eliminates disk I/O bottleneck
- ⚡ **Mold Linker**: 3-5x faster linking (saves 10-15 min per build)
- 💨 **Ccache**: Intelligent compiler cache (reuses ~85% on rebuilds)
- 🔧 **Parallel Compilation**: Optimized job count for GitHub Actions runners
- 🎯 **Debug Info Disabled**: No debug symbols (30% faster compilation)
- 📦 **Module Stripping**: Smaller packages, faster builds
- 🔄 **Smart Cache Keys**: Reuses cache across kernel versions
- 💾 **Ccache Temp in RAM**: Temporary ccache files in tmpfs for maximum speed

## 📥 Installation

### Download and Install

1. Download the `.deb` packages from the [latest release](https://github.com/CypherNoodle/Releases-Kernel/releases/latest)
2. Install all packages:

```bash
sudo dpkg -i linux-*.deb
```

3. Reboot your system:

```bash
sudo reboot
```

### Verify Installation

After reboot, check your kernel version:

```bash
uname -r
```

## 🔐 Package Verification

All releases include `SHA256SUMS.txt` for package verification:

```bash
# Verify checksums
sha256sum -c SHA256SUMS.txt
```

## 📋 Release Contents

Each release includes:

- `linux-image-*.deb` - Kernel image
- `linux-headers-*.deb` - Kernel headers for module compilation
- `linux-libc-dev-*.deb` - Kernel headers for userspace development
- `SHA256SUMS.txt` - Checksums for verification
- `build-metrics.txt` - Build statistics and cache information

## 🔄 Automatic Updates

The workflow automatically:

- ✅ Detects kernel version from Makefile
- ✅ Creates/updates release with detected version tag
- ✅ Builds every 5 days to maintain warm cache
- ✅ Optimizes for Intel Silvermont architecture
- ✅ Generates changelog from git history

## 🛠️ Development

### Technical Optimizations

The build system includes several performance optimizations:

#### Compilation Speed
- **tmpfs Build**: Entire build in RAM (6GB)
  - Zero disk I/O latency
  - 15-20% faster than disk builds
  - Ccache temp files also in RAM
- **Mold Linker**: Modern linker that's 3-5x faster than GNU ld
  - Parallel symbol resolution
  - Efficient memory usage
  - Saves 10-15 minutes per build
- **Ccache Configuration**: Optimized for kernel builds
  - Fast compression (level 1)
  - Permissive sloppiness settings
  - Hash directory disabled for speed
  - Temporary directory in tmpfs
- **Parallel Jobs**: `nproc + 2` for optimal CPU utilization

#### Package Size & Speed
- **No Debug Symbols**: `CONFIG_DEBUG_INFO=n`
  - 30% faster compilation
  - 60% smaller packages
  - Suitable for production use
- **Module Stripping**: `INSTALL_MOD_STRIP=1`
  - Removes debug symbols from kernel modules
  - Faster packaging
  - Smaller .deb files

#### Build Artifacts
- **XZ Compression**: Balance between speed and size
- **Stripped Binaries**: Faster installation
- **Optimized for x5-Z8300**: Silvermont-specific optimizations

### Cache System

The build system uses aggressive caching to speed up compilation:

- **ccache:** Compiler cache (~500-900MB per version)
  - Compression level 1 (fast)
  - Intelligent sloppiness settings for maximum reuse
  - 85-90% hit rate on rebuilds
- **kernel metadata:** Configuration and generated files
- **build tools:** Pre-compiled build utilities (fixdep, kconfig, etc.)
- **APT packages:** Build dependencies (mold, ccache, gcc, etc.)

>Cache is automatically managed and refreshed every 5 days to prevent expiration.

### Customization
This project supports multiple optimized configurations.

#### Kernel Configuration (Acer SW1-011)

The build uses `acer_sw1_011_defconfig` (stored in the kernel source). Tuned for:
- Intel Atom x5-Z8300 (Silvermont)
- Cherry Trail platform
- Touchscreen and sensor support

#### Kernel Configuration (Samsung NP300E4C)

The build uses `samsung_np300e4c_defconfig` (stored in the root of this repo). Tuned for:
- Intel Celeron B820 (Sandy Bridge)
- Intel HD Graphics 2000
- Atheros Wireless support
- Optimized performance and security (KSPP)

To modify the kernel configuration:

1. Edit the respective defconfig file (`arch/x86/configs/acer_sw1_011_defconfig` in kernel-src or `samsung_np300e4c_defconfig` in this repo)
2. Commit your changes
3. Run the workflow
4. The new release will use your custom configuration

#### Adding/Removing Features

```bash
# In your local kernel source:
make acer_sw1_011_defconfig
make menuconfig  # Make your changes
make savedefconfig
cp defconfig arch/x86/configs/acer_sw1_011_defconfig
git add arch/x86/configs/acer_sw1_011_defconfig
git commit -m "Update kernel config"
git push
```

## 🤝 Contributing

Contributions are welcome! Please feel free to submit issues or pull requests.

---

<div align="center">

**Made with ❤️ for Optimized Linux Performance**

</div>
