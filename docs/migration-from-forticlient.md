# Migrating from FortiClient to OpenFortiVPN

## Overview

This guide explains how to migrate from FortiClient to OpenFortiVPN on GNU/Linux systems.

The goal is to provide a lightweight, scriptable, and reliable alternative for SSL VPN connections, especially in environments using SAML authentication.

---

## Why Consider Migration?

FortiClient is the official Fortinet client and may be required in some environments.

However, Linux users may experience issues such as:

* Infinite "Connecting" state
* Browser authentication loops
* SAML callback failures
* Unexpected authentication responses
* GUI-related issues
* Intermittent connection failures

OpenFortiVPN provides a CLI-based alternative that works well for SSL VPN access.

---

## Benefits of OpenFortiVPN

### Lightweight

OpenFortiVPN runs entirely from the command line.

No desktop application is required.

---

### Open Source

The project is open source and actively maintained.

Source code can be reviewed, audited, and customized.

---

### Automation Friendly

OpenFortiVPN integrates well with:

* Shell scripts
* CI/CD environments
* Remote development workstations
* Headless Linux systems

---

### Native Linux Experience

OpenFortiVPN follows standard Linux conventions and integrates naturally with:

* Ubuntu
* Debian
* Pop!_OS
* Linux Mint
* Zorin OS

---

### Modern Authentication Support

Recent versions support SAML authentication.

Examples:

* Microsoft Entra ID
* Azure AD
* Okta
* Google Workspace
* Other SAML Identity Providers

---

## When Should You Keep FortiClient?

FortiClient may still be required when your organization enforces:

* Endpoint compliance checks
* Zero Trust Network Access
* EMS integration
* Endpoint posture validation
* Security policies tied to FortiClient

In these situations, OpenFortiVPN may not be sufficient.

---

## Migration Prerequisites

Verify the currently installed FortiClient version:

```bash
which forticlient
```

Verify OpenFortiVPN version:

```bash
openfortivpn --version
```

Recommended:

```text
OpenFortiVPN 1.24.1+
```

---

## Install OpenFortiVPN

Follow:

```text
docs/install-openfortivpn-1.24.md
```

Verify installation:

```bash
openfortivpn --version
```

Expected:

```text
1.24.1+
```

---

## Verify SAML Support

Run:

```bash
openfortivpn --help | grep -i saml
```

Expected:

```text
SAML-related options
```

If no output is returned, install a newer version.

---

## First Connection

Using the helper script:

```bash
vpn-saml
```

Or directly:

```bash
sudo openfortivpn vpn.example.com:443 --saml-login
```

---

## Authentication Flow

The typical workflow is:

1. Start OpenFortiVPN.
2. Open browser authentication.
3. Complete identity provider login.
4. Complete MFA if required.
5. Receive SAML response.
6. Establish VPN tunnel.
7. Access internal resources.

---

## Verifying a Successful Migration

Verify VPN interfaces:

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

depending on the VPN implementation.

---

Verify routes:

```bash
ip route
```

Confirm that corporate routes have been added.

---

Verify connectivity:

```bash
ping INTERNAL_RESOURCE
```

or:

```bash
curl INTERNAL_SERVICE
```

---

## Recommended Configuration

Prefer:

```text
SAML authentication
```

Avoid:

```text
Plain-text passwords
```

Avoid:

```text
Credentials stored in Git repositories
```

Prefer helper scripts for daily usage.

---

## Troubleshooting

If authentication succeeds but the tunnel is not created:

Check:

```bash
ip addr
```

Check:

```bash
ip route
```

Check:

```bash
vpn-doctor
```

Review:

```text
docs/troubleshooting.md
```

---

## Rollback Strategy

Migration does not require removing FortiClient.

Both tools can coexist on the same machine.

If OpenFortiVPN does not meet organizational requirements:

1. Stop OpenFortiVPN.
2. Launch FortiClient.
3. Connect using the official client.

This allows gradual adoption without impacting users.

---

## Summary

OpenFortiVPN is a strong alternative for Linux users who require SSL VPN access and modern SAML authentication.

Benefits include:

* Lightweight architecture
* Open source implementation
* Better automation support
* Native Linux workflow
* SAML compatibility

Organizations requiring endpoint compliance or EMS integration may still need FortiClient.

For SSL VPN-only scenarios, OpenFortiVPN is often a simpler and more reliable solution.
