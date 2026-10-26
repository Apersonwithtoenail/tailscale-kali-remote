# Tailscale + SSH: Remote Kali Access

**Access your Kali Linux laptop from anywhere in the world using your phone.**

This repo documents how to set up a secure, zero-config VPN using Tailscale and connect to a Kali machine via SSH directly from Termux on Android. 

## The Problem
You are away from home, but you need to access your local Kali machine to push code to GitHub, run scripts, or use tools like `termfetch`. Port forwarding on a home router is insecure and complicated.

## The Solution
1. **Tailscale**: Creates a private, encrypted WireGuard network between your phone and laptop.
2. **OpenSSH**: Runs a secure shell server on Kali.
3. **Termux**: An Android terminal emulator that acts as the SSH client.
4. **Systemd Mask**: Prevents the laptop from going to sleep when the lid is closed.

## Setup Guide

### 1. On the Kali Laptop
Install the SSH server and Tailscale:
```bash
sudo apt update
sudo apt install -y openssh-server tailscale
sudo systemctl enable --now ssh
sudo tailscale up
```
*(Run `tailscale up` and log in via the browser URL).*

**Prevent Sleep (Crucial):**
If you close the laptop lid, it will suspend and drop your SSH connection. Run this to keep it awake:
```bash
sudo systemctl mask sleep.target suspend.target hibernate.target hybrid-sleep.target
```

Get your Tailscale IP:
```bash
tailscale ip -4
```

### 2. On the Android Phone (Termux)
Install Tailscale from the Play Store and log in with the *same account*.

Install the SSH client in Termux:
```bash
pkg install openssh -y
```

Connect to your laptop:
```bash
ssh your_username@100.x.x.x
```

## Pro-Tip: SSH Keys
To avoid typing your password every time, generate an SSH key on Termux and copy it to your Kali machine's `~/.ssh/authorized_keys` file.

## Features
- **Zero-Config**: No router port forwarding required.
- **Encrypted**: Uses WireGuard under the hood.
- **Anywhere**: Works over 4G/5G on your phone.
- **Battery Friendly**: SSH uses almost zero battery on the phone.
