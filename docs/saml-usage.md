# SAML Authentication Usage Guide

## Overview

This document explains how to connect to a Fortinet SSL VPN using OpenFortiVPN and SAML authentication.

This is the recommended authentication method when the VPN is integrated with:

- Microsoft Entra ID
- Azure AD
- Okta
- Google Workspace
- Other SAML Identity Providers

---

## Prerequisites

Verify that OpenFortiVPN is installed:

```bash
openfortivpn --version
```

Expected:

```text
openfortivpn 1.24.1
```

Verify SAML support:

```bash
openfortivpn --help | grep -i saml
```

---

## Starting a VPN Session

Using the provided helper script:

```bash
vpn-saml
```

Or directly:

```bash
sudo openfortivpn <YOUR_VPN_HOST>:<YOUR_VPN_PORT> --saml-login
```

To pin the VPN gateway certificate fingerprint with the helper script:

```bash
OPENFORTIVPN_TRUSTED_CERT="SHA256_FINGERPRINT" vpn-saml
```

Or directly:

```bash
sudo openfortivpn <YOUR_VPN_HOST>:<YOUR_VPN_PORT> --saml-login --trusted-cert SHA256_FINGERPRINT
```

---

## Authentication Flow

When the connection starts:

1. OpenFortiVPN launches the SAML authentication process.
2. A browser window or authentication URL is presented.
3. The user authenticates with Microsoft Entra ID.
4. Multi-factor authentication is completed if required.
5. The SAML response is returned to OpenFortiVPN.
6. The VPN tunnel is established.

Example:

```text
Starting authentication...
Waiting for SAML response...
Authentication successful.
Tunnel established.
```

---

## Verifying Connection Status

Check VPN interfaces:

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

depending on the VPN configuration.

---

## Verify Routing

```bash
ip route
```

Expected:

```text
PPP routes
Corporate network routes
Default route adjustments
```

---

## Verify Internal Access

Example:

```bash
ping internal-server.company.local
```

or:

```bash
curl http://internal-service.company.local
```

---

## Disconnecting

To terminate the VPN session:

```text
CTRL + C
```

The tunnel will be closed and routes removed.

---

## Common Issues

### Browser Opens but Authentication Never Completes

Verify:

```bash
openfortivpn --version
```

Expected:

```text
1.24.1+
```

---

### SAML Option Not Available

Verify:

```bash
openfortivpn --help | grep -i saml
```

If no output is returned, install a newer version.

---

### Tunnel Not Created

Check:

```bash
ip addr
```

No PPP/TUN interface usually indicates:

- authentication failure
- routing issues
- PPP configuration problems

---

### DNS Resolution Issues

Verify:

```bash
dig <YOUR_VPN_HOST>
```

Expected:

```text
200.187.0.106
```

---

### HTTPS Connectivity Issues

Verify:

```bash
curl -vk https://<YOUR_VPN_HOST>
```

The VPN gateway should respond successfully.

---

## Recommended Daily Workflow

Connect:

```bash
vpn-saml
```

Authenticate through Microsoft Entra ID.

Verify:

```bash
ip addr
```

Work normally.

Disconnect:

```text
CTRL + C
```

This is the recommended workflow for Linux users using Fortinet SSL VPN with SAML authentication.
