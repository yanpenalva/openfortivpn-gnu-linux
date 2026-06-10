# Developer Guide

## Overview

This document explains how to contribute to the project, update dependencies, test changes, and publish improvements.

---

## Repository Structure

```text
openfortivpn-gnu-linux/
├── README.md
├── scripts/
│   ├── vpn-saml
│   ├── vpn-legacy
│   └── vpn-doctor
├── examples/
│   └── openfortivpn-config.example
└── docs/
    ├── install-openfortivpn-1.24.md
    ├── saml-usage.md
    ├── troubleshooting.md
    ├── migration-from-forticlient.md
    ├── faq.md
    ├── security.md
    └── developer-guide.md
```

---

## Project Goals

The project focuses on:

* GNU/Linux compatibility
* Fortinet SSL VPN support
* SAML authentication support
* Simple user experience
* Minimal dependencies

The project is intentionally lightweight.

---

## Development Environment

Recommended:

* Ubuntu 22.04+
* Pop!_OS 22.04+
* OpenFortiVPN 1.24.1+

Required packages:

```bash
sudo apt install \
    git \
    shellcheck \
    ppp \
    curl \
    dnsutils
```

---

## Shell Script Standards

All scripts should follow:

```bash
#!/usr/bin/env bash

set -euo pipefail
```

Avoid:

```bash
set +e
```

Avoid unnecessary complexity.

Prefer:

* simple functions
* clear variable names
* predictable behavior

---

## Linting

Use:

```bash
shellcheck scripts/*
```

All scripts should pass ShellCheck validation.

---

## Testing Scripts

Validate:

```bash
scripts/vpn-doctor
```

Validate:

```bash
scripts/vpn-saml
```

Validate:

```bash
scripts/vpn-legacy
```

Confirm:

* expected output
* expected error handling
* expected exit codes

---

## Updating OpenFortiVPN Documentation

When a new OpenFortiVPN release becomes available:

Verify:

```bash
openfortivpn --version
```

Review:

```text
Release notes
Breaking changes
Authentication changes
SAML changes
```

Update:

```text
README.md
docs/install-openfortivpn-1.24.md
docs/faq.md
```

when required.

---

## Documentation Standards

Documentation should:

* use Markdown
* use English
* remain vendor-neutral
* avoid company-specific references

Avoid:

```text
Internal hostnames
Corporate domains
Private IPs
Internal authentication details
```

The repository should remain reusable across organizations.

---

## Security Requirements

Never commit:

```text
Passwords
Tokens
VPN credentials
Private certificates
Internal configurations
```

Examples:

```text
openfortivpn-config
.env
vpn.conf
```

Use:

```gitignore
*.env
*.local
openfortivpn-config
```

where appropriate.

---

## Pull Request Guidelines

Before opening a pull request:

Run:

```bash
shellcheck scripts/*
```

Validate:

```bash
scripts/vpn-doctor
```

Review:

```text
Documentation
Examples
Installation instructions
```

Ensure no credentials are included.

---

## Issue Reporting

Include:

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
scripts/vpn-doctor
```

Do not include:

```text
Passwords
Authentication tokens
Sensitive logs
Internal addresses
```

---

## Release Process

Recommended versioning:

```text
v1.0.0
v1.1.0
v1.2.0
```

Use:

```text
MAJOR.MINOR.PATCH
```

Examples:

```text
1.0.0 Initial release
1.1.0 New feature
1.1.1 Documentation fix
```

---

## Long-Term Roadmap

Potential future improvements:

* Automated installer
* Multi-distribution support
* Containerized execution
* Network diagnostics improvements
* Additional VPN validation checks
* CI/CD validation
* GitHub Actions integration

The project should remain simple, maintainable, and focused on Linux VPN usability.
