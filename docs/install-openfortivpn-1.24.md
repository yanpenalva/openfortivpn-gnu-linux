# Install OpenFortiVPN 1.24.1 with SAML Support

## Overview

This guide explains how to install OpenFortiVPN 1.24.1 with SAML support on Ubuntu-based distributions such as:

* Ubuntu 22.04
* Ubuntu 24.04
* Pop!_OS 22.04
* Linux Mint
* Zorin OS

## Why Not Use the Repository Version?

Most Ubuntu-based distributions ship an outdated OpenFortiVPN package.

Example:

```bash
sudo apt install openfortivpn
```

Ubuntu 22.04 installs:

```text
openfortivpn 1.17.1
```

This version does not support SAML authentication.

Modern Fortinet VPN deployments commonly use:

* Microsoft Entra ID
* Azure AD
* Okta
* Google Workspace
* SAML Identity Providers

For these environments, OpenFortiVPN 1.24.1 or newer is required.

---

## Remove Existing Installation

Check the installed version:

```bash
openfortivpn --version
```

Remove the old package:

```bash
sudo apt remove -y openfortivpn
```

---

## Install Build Dependencies

```bash
sudo apt update

sudo apt install -y \
    git \
    autoconf \
    automake \
    libtool \
    pkg-config \
    build-essential \
    libssl-dev \
    libxml2-dev \
    ppp
```

---

## Download Source Code

```bash
git clone https://github.com/adrienverge/openfortivpn.git
cd openfortivpn
```

---

## Checkout Version 1.24.1

```bash
git checkout v1.24.1
```

Validate:

```bash
git describe --tags
```

Expected output:

```text
v1.24.1
```

---

## Build

Generate build files:

```bash
./autogen.sh
```

Configure:

```bash
./configure
```

Compile:

```bash
make -j"$(nproc)"
```

Install:

```bash
sudo make install
```

---

## Verify Installation

Check the installed version:

```bash
openfortivpn --version
```

Expected output:

```text
openfortivpn 1.24.1
```

---

## Verify SAML Support

Check available options:

```bash
openfortivpn --help | grep -i saml
```

A SAML-capable build should display SAML-related options.

If nothing is returned, verify that version 1.24.1 or newer is installed.

---

## Test the VPN Connection

Example:

```bash
sudo openfortivpn <YOUR_VPN_HOST>:YOUR_VPN_PORT--saml-login
```

Expected flow:

1. OpenFortiVPN starts.
2. Browser-based authentication begins.
3. User authenticates through Microsoft Entra ID.
4. VPN tunnel is established.
5. Network routes are applied.

---

## Verify Tunnel Creation

Check interfaces:

```bash
ip addr
```

Look for interfaces such as:

```text
ppp0
tun0
```

Check routes:

```bash
ip route
```

---

## Troubleshooting

### Verify DNS Resolution

```bash
dig <YOUR_VPN_HOST>
```

Expected:

```text
200.187.0.106
```

---

### Verify HTTPS Connectivity

```bash
curl -vk https://<YOUR_VPN_HOST>
```

The gateway should return an HTTP response.

---

### Verify PPP Installation

```bash
which pppd
```

Expected:

```text
/usr/sbin/pppd
```

---

### Verify SAML Support

```bash
openfortivpn --help | grep -i saml
```

No output generally indicates an outdated installation.

---

### Verify OpenFortiVPN Version

```bash
openfortivpn --version
```

Expected:

```text
1.24.1
```

---

## Cleanup

After installation, the source repository is no longer required.

Verify installation:

```bash
openfortivpn --version
```

Remove the cloned repository:

```bash
cd ~
rm -rf openfortivpn
```

The installed binary remains available because it was installed into the system during:

```bash
sudo make install
```

---

## Summary

Install dependencies:

```bash
sudo apt install git autoconf automake libtool pkg-config build-essential libssl-dev libxml2-dev ppp
```

Build:

```bash
git clone https://github.com/adrienverge/openfortivpn.git
cd openfortivpn
git checkout v1.24.1
./autogen.sh
./configure
make -j"$(nproc)"
sudo make install
```

Connect:

```bash
sudo openfortivpn <YOUR_VPN_HOST>:YOUR_VPN_PORT --saml-login
```
