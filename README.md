# 🐧 Linux Kernel for Acer Switch One 10 SW1-011

Custom Linux kernel builds optimized for Intel x5-Z8300 (Silvermont architecture).

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

- **Target Platform:** Intel x5-Z8300 (Silvermont)
- **Compiler Optimizations:** `-march=silvermont -mtune=silvermont`
- **Build System:** GitHub Actions with ccache
- **Automatic Builds:** Every 5 days (keeps cache warm)

### Build Times

| Build Type | Duration | Cache Hit Rate |
|------------|----------|----------------|
| First build (cold cache) | ~50-60 min | 0% |
| Subsequent builds (warm cache) | ~10-20 min | 75-90% |
| Version change (6.19 → 6.20) | ~15-25 min | 60-80% |
| Major version (6.x → 7.x) | ~30-40 min | 40-60% |

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

### Cache System

The build system uses aggressive caching to speed up compilation:

- **ccache:** Compiler cache (~500MB per version)
- **kernel metadata:** Configuration and generated files
- **build tools:** Pre-compiled build utilities
- **APT packages:** Build dependencies

Cache is automatically managed and refreshed every 5 days.

### Customization

To modify kernel configuration:

1. Edit `.config` in the kernel source
2. Commit changes
3. Run the workflow
4. New release will use your custom config

## 📊 Build Status

[![Build Status](https://img.shields.io/github/actions/workflow/status/CypherNoodle/Releases-Kernel/build-kernel.yaml?branch=main&style=for-the-badge&logo=github-actions&logoColor=white)](https://github.com/CypherNoodle/Releases-Kernel/actions/workflows/build-kernel.yaml)

## 🤝 Contributing

Contributions are welcome! Please feel free to submit issues or pull requests.

---

<div align="center">

**Made with ❤️ for Intel x5-Z8300**

</div>
