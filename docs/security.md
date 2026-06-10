# Security Guidelines

## Overview

This document describes security best practices when using OpenFortiVPN on GNU/Linux systems.

The goal is to reduce the risk of credential leakage, unauthorized access, and accidental exposure of VPN-related information.

---

## Prefer SAML Authentication

Whenever possible, use SAML authentication.

Benefits:

* No password storage in configuration files
* Centralized identity management
* Multi-factor authentication support
* Improved auditing
* Reduced credential exposure

Example:

```bash
vpn-saml
```

or:

```bash
sudo openfortivpn vpn.example.com:443 --saml-login
```

---

## Avoid Storing Passwords

Avoid configurations like:

```ini
username = myuser
password = mypassword
```

Although supported by OpenFortiVPN, storing passwords in plain text increases risk.

Prefer:

```text
SAML Authentication
```

whenever available.

---

## Never Commit Credentials

Never commit files containing:

```ini
username =
password =
trusted-cert =
```

or any internal VPN configuration.

Examples:

```text
openfortivpn-config
.env
vpn.conf
```

Add sensitive files to:

```text
.gitignore
```

Example:

```gitignore
openfortivpn-config
*.vpn
*.conf.local
.env
```

---

## Protect Configuration Files

If a configuration file must exist locally:

```bash
chmod 600 openfortivpn-config
```

Verify:

```bash
ls -l openfortivpn-config
```

Expected:

```text
-rw-------
```

Only the owner should have access.

---

## Protect Shell History

Commands containing credentials may be stored in shell history.

Avoid:

```bash
openfortivpn vpn.example.com --username user --password password
```

Use:

```text
SAML authentication
```

or configuration files with restricted permissions.

---

## Protect Screenshots and Logs

Logs may contain:

* usernames
* internal hostnames
* internal IP addresses
* routing information

Before sharing logs:

Remove:

```text
Usernames
Hostnames
IP addresses
Domains
Tokens
```

---

## Verify Download Sources

Always download OpenFortiVPN from trusted sources.

Recommended:

```text
Official GitHub repository
Official distribution repositories
```

Avoid:

```text
Unknown mirrors
Untrusted package repositories
Random binaries
```

---

## Verify Installed Version

Check:

```bash
openfortivpn --version
```

Keep the client updated.

New releases may include:

* security fixes
* authentication improvements
* compatibility updates

---

## Use MFA Whenever Available

If supported by the identity provider:

Enable:

```text
Multi-Factor Authentication
```

Examples:

* Microsoft Entra ID
* Azure AD
* Okta
* Google Workspace

MFA significantly reduces account compromise risk.

---

## Disconnect When Not Needed

Terminate VPN sessions when work is complete.

Disconnect:

```text
CTRL + C
```

Avoid leaving VPN sessions active unnecessarily.

---

## Verify Active Connections

Check processes:

```bash
ps aux | grep openfortivpn
```

Check interfaces:

```bash
ip addr
```

Review active connections periodically.

---

## Shared Computers

Avoid VPN usage on:

* public computers
* shared workstations
* unmanaged systems

Use only trusted devices.

---

## Corporate Policies

Always follow your organization's:

* VPN policies
* authentication policies
* security requirements
* endpoint requirements

OpenFortiVPN should not be used to bypass organizational security controls.

---

## Recommended Security Checklist

Before connecting:

```text
✓ OpenFortiVPN updated
✓ SAML authentication enabled
✓ MFA enabled
✓ No passwords stored in Git repositories
✓ Sensitive files protected with chmod 600
✓ Trusted network connection
```

After disconnecting:

```text
✓ VPN session terminated
✓ No sensitive logs shared publicly
✓ No credentials exposed
```

Following these practices significantly reduces security risks while using OpenFortiVPN.
