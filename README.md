# 🛰️ NMAP Reference Guide

> A sleek, terminal-aesthetic HTML cheatsheet covering OS detection, service version detection, timing options, and output formats — all in one page.

---

## 📸 Preview

![Nmap Reference Guide](https://img.shields.io/badge/style-terminal--dark-00ff6a?style=for-the-badge&logo=gnubash&logoColor=black)
![HTML](https://img.shields.io/badge/HTML-single--file-00e5ff?style=for-the-badge&logo=html5&logoColor=black)
![License](https://img.shields.io/badge/license-MIT-ffb300?style=for-the-badge)

---

## 📋 Contents

The guide is organized into **3 major sections** with **10 subsections**:

### 01 — OS & Service Detection

| # | Topic |
|---|-------|
| 1.1 | Version Detection Overview — flags, intensity levels |
| 1.2 | OS Detection — fingerprinting, `--osscan-guess`, aggressive mode |
| 1.3 | Service Version Detection — `-sV`, probe depth, combining with scripts |

### 02 — Timing Options

| # | Topic |
|---|-------|
| 2.1 | Timing Overview — T0 (paranoid) through T5 (insane) templates |
| 2.2 | Timing Parameters — `--host-timeout`, `--max-retries`, RTT limits |
| 2.3 | Minimum Scan Delay — `--scan-delay` usage and time units |
| 2.4 | Minimum Packet Rate — `--min-rate`, `--min-parallelism` |
| 2.5 | Maximum Scan Delay — `--max-scan-delay`, combining with `-T4` |
| 2.6 | Maximum Packet Rate — `--max-rate`, `--max-parallelism` |

### 03 — Output Options

| # | Topic |
|---|-------|
| 3.1 | Save to Text File — `-oN` normal output |
| 3.2 | Save to XML File — `-oX` for tool pipelines |
| 3.3 | Output All Formats — `-oA` (normal + XML + grepable) |
| 3.4 | Display Scan Statistics — `-v`, `--stats-every`, `--reason` |

---

## 🚀 Usage

No installation needed. Just open the file in any browser:

```bash
# Clone the repo
git clone https://github.com/YOUR_USERNAME/YOUR_REPO.git
cd YOUR_REPO

# Open in browser
open nmap-reference.html        # macOS
xdg-open nmap-reference.html   # Linux
start nmap-reference.html       # Windows
```

Or view it directly on GitHub Pages if enabled:

```
https://YOUR_USERNAME.github.io/YOUR_REPO/nmap-reference.html
```

---

## ⚡ Quick Command Reference

```bash
# OS + Service Detection
nmap -A <target>                          # Aggressive: OS + version + scripts + traceroute
nmap -O --osscan-guess <target>           # OS detection with guessing
nmap -sV --version-intensity 5 <target>  # Service detection, custom depth

# Timing
nmap -T4 <target>                         # Aggressive timing (recommended)
nmap -T1 --scan-delay 1s <target>        # Slow & stealthy
nmap --min-rate 500 --max-rate 1000 <target>  # Controlled packet rate

# Output
nmap -oA full_scan <target>              # Save all formats at once
nmap -oX output.xml <target>             # XML for tool integration
nmap -v --stats-every 10s <target>       # Verbose with progress updates

# Combined example
nmap -sV -O -T4 -oA audit --stats-every 5s <target>
```

---

## 🎨 Design

Built with a **terminal / hacker aesthetic**:

- 🟢 Dark background (`#050a08`) with green phosphor glow
- 🔤 **Orbitron** (headers) + **Share Tech Mono** (body/code)
- ✨ CSS scanline overlay and noise texture
- 📐 Responsive CSS Grid layout
- 💡 Corner bracket decorations on each section panel
- 🎞️ Fade-in animations on load

---

## 📁 File Structure

```
.
├── nmap-reference.html   # Single self-contained HTML file
└── README.md             # This file
```

---

## ⚠️ Disclaimer

This guide is intended for **authorized security testing and educational purposes only**.  
Always obtain proper written permission before scanning any network or host.  
Unauthorized scanning may be illegal in your jurisdiction.

---

## 📄 License

MIT — free to use, share, and modify.

---

<div align="center">
  <sub>Built with ❤️ for the security community</sub>
</div>
