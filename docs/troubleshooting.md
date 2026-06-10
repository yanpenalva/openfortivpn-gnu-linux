# Troubleshooting Guide

## Overview

This guide covers the most common OpenFortiVPN and Fortinet SSL VPN issues on GNU/Linux systems.

Supported environments:

* Ubuntu
* Pop!_OS
* Linux Mint
* Zorin OS
* Debian-based distributions

---

# Verify OpenFortiVPN Installation

Check the installed version:

```bash
openfortivpn --version
```

Expected:

```text
openfortivpn 1.24.1
```

If an older version is installed, upgrade to a newer release.

---

# Verify SAML Support

Check available options:

```bash
openfortivpn --help | grep -i saml
```

Expected:

```text
SAML-related options
```

If nothing is returned:

* the installed version is outdated
* OpenFortiVPN was built incorrectly
* the wrong binary is being executed

Verify the binary:

```bash
which openfortivpn
```

---

# Verify DNS Resolution

Check the VPN hostname:

```bash
dig <YOUR_VPN_HOST>
```

Expected:

```text
200.187.0.106
```

If DNS fails:

```bash
nslookup <YOUR_VPN_HOST>
```

or

```bash
host <YOUR_VPN_HOST>
```

---

# Verify HTTPS Connectivity

Test HTTPS access:

```bash
curl -vk https://<YOUR_VPN_HOST>
```

Expected:

```text
HTTP/1.1 200 OK
```

or a redirect response.

If the request times out:

* firewall issue
* network issue
* ISP restriction
* VPN gateway unavailable

---

# Verify PPP

OpenFortiVPN requires PPP.

Check:

```bash
which pppd
```

Expected:

```text
/usr/sbin/pppd
```

Install if missing:

```bash
sudo apt install ppp
```

---

# Browser Authentication Does Not Start

Verify SAML support:

```bash
openfortivpn --help | grep -i saml
```

Verify version:

```bash
openfortivpn --version
```

Verify browser access:

```bash
xdg-open https://www.microsoft.com
```

---

# Authentication Succeeds but Tunnel Is Not Created

Check interfaces:

```bash
ip addr
```

Expected:

```text
ppp0
```

or:

```text
tun0
```

If no tunnel interface exists:

* PPP failure
* gateway rejection
* authentication callback failure

---

# VPN Connects but Internal Resources Are Unreachable

Check routes:

```bash
ip route
```

Check DNS:

```bash
resolvectl status
```

Verify corporate routes exist.

Example:

```bash
ip route | grep 10.
```

---

# Verify VPN Process

Check active processes:

```bash
ps aux | grep openfortivpn
```

Expected:

```text
openfortivpn
pppd
```

---

# Verify Tunnel Traffic

Check traffic:

```bash
sudo tcpdump -ni any host <YOUR_VPN_HOST>
```

Expected:

```text
Bidirectional traffic
```

Symptoms:

| Symptom                                  | Possible Cause                 |
| ---------------------------------------- | ------------------------------ |
| No packets                               | Network issue                  |
| SYN retransmissions                      | Gateway unreachable            |
| Immediate disconnect                     | Authentication or policy issue |
| Authentication succeeds then disconnects | Server-side policy             |

---

# Verify Tunnel Interface

List interfaces:

```bash
ip link
```

Expected:

```text
ppp0
```

or:

```text
tun0
```

---

# Verify DNS After Connection

Check DNS servers:

```bash
resolvectl dns
```

Check search domains:

```bash
resolvectl domain
```

---

# Verify Corporate Network Reachability

Ping internal resources:

```bash
ping internal-host
```

Check routes:

```bash
ip route get INTERNAL_IP
```

---

# OpenFortiVPN Doctor

Run:

```bash
vpn-doctor
```

Expected checks:

```text
✓ OpenFortiVPN installed
✓ SAML support detected
✓ DNS resolution works
✓ HTTPS connectivity works
✓ PPP installed
```

---

# Common Issues

## SAML Authentication Fails

Verify:

```bash
openfortivpn --version
```

Upgrade to:

```text
1.24.1+
```

---

## Tunnel Does Not Appear

Verify:

```bash
ip addr
```

No PPP/TUN interface usually indicates:

* PPP problem
* authentication callback problem
* server-side rejection

---

## DNS Works but Internal Services Fail

Verify:

```bash
resolvectl status
```

Verify:

```bash
ip route
```

---

## VPN Suddenly Stops Working

Check:

```bash
openfortivpn --version
```

Verify:

```bash
vpn-doctor
```

Verify gateway:

```bash
curl -vk https://<YOUR_VPN_HOST>
```

---

# Useful Commands

```bash
openfortivpn --version
```

```bash
openfortivpn --help | grep -i saml
```

```bash
ip addr
```

```bash
ip route
```

```bash
resolvectl status
```

```bash
curl -vk https://<YOUR_VPN_HOST>
```
```

```bash
dig <YOUR_VPN_HOST>
```

```bash
vpn-doctor
```

---

# Escalation Checklist

Before opening a support request, collect:

```bash
openfortivpn --version
```

```bash
ip addr
```

```bash
ip route
```

```bash
vpn-doctor
```

```bash
curl -vk https://<YOUR_VPN_HOST>
```

This information is usually sufficient to identify most Linux VPN issues.
