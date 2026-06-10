# OpenFortiVPN GNU/Linux

Scripts and documentation to connect to Fortinet SSL VPN on GNU/Linux using OpenFortiVPN.

This repository supports two modes:

* SAML authentication
* Legacy username/password authentication

## Configuration

Before using any script, replace the placeholder values below with your VPN information:

```text
YOUR_VPN_HOST
YOUR_VPN_PORT
```

Example:

```text
YOUR_VPN_HOST = vpn.company.com
YOUR_VPN_PORT = 443
```

The following files may contain placeholders that must be updated before use:

```text
scripts/vpn-saml
scripts/vpn-doctor
examples/openfortivpn-config.example
```

Search for:

```text
YOUR_VPN_HOST
YOUR_VPN_PORT
```

and replace them with the values provided by your organization.

Example:

```bash
VPN_HOST="vpn.company.com"
VPN_PORT="443"
```

Failure to replace these values will prevent the VPN connection from working correctly.

## Recommended mode: SAML

Use this mode when your VPN uses browser-based authentication, Microsoft Entra ID, Azure AD, Okta, Google Workspace, or another SAML provider.

### Requirement

SAML requires OpenFortiVPN `1.24.1` or newer.

Check your version:

```bash
openfortivpn --version
```

Check SAML support:

```bash
openfortivpn --help | grep -i saml
```

If no SAML option is shown, install a newer version.

### Usage

```bash
./scripts/vpn-saml
```

Expected gateway format:

```text
YOUR_VPN_HOST:YOUR_VPN_PORT
```

Example:

```text
vpn.company.com:443
```

To override without editing the script:

```bash
VPN_HOST="vpn.company.com" VPN_PORT="443" ./scripts/vpn-saml
```

## Legacy mode

Use this mode only when your VPN uses direct username/password authentication.

Copy the example config:

```bash
cp examples/openfortivpn-config.example openfortivpn-config
```

Edit:

```bash
nano openfortivpn-config
```

Replace:

```text
YOUR_VPN_HOST
YOUR_VPN_PORT
YOUR_USERNAME
YOUR_PASSWORD
```

Run:

```bash
OPENFORTIVPN_CONFIG="./openfortivpn-config" ./scripts/vpn-legacy
```

## Doctor

Run environment checks:

```bash
./scripts/vpn-doctor
```

It checks:

* OpenFortiVPN installation
* SAML support
* DNS resolution
* HTTPS connectivity
* Required system commands

Before running it, make sure `YOUR_VPN_HOST` and `YOUR_VPN_PORT` were replaced inside `scripts/vpn-doctor`, or override them:

```bash
VPN_HOST="vpn.company.com" VPN_PORT="443" ./scripts/vpn-doctor
```

## Install scripts globally

Optional:

```bash
mkdir -p ~/.local/bin

cp scripts/vpn-saml ~/.local/bin/vpn-saml
cp scripts/vpn-legacy ~/.local/bin/vpn-legacy
cp scripts/vpn-doctor ~/.local/bin/vpn-doctor

chmod +x ~/.local/bin/vpn-saml ~/.local/bin/vpn-legacy ~/.local/bin/vpn-doctor
```

Make sure `~/.local/bin` is in your `PATH`.

For ZSH:

```bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

For Bash:

```bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

Then use:

```bash
vpn-saml
```

or:

```bash
vpn-legacy
```

or:

```bash
vpn-doctor
```

## Notes

Ubuntu 22.04 and Pop!_OS 22.04 usually provide an old OpenFortiVPN version through `apt`.

That version may not support SAML.

If SAML is required, install OpenFortiVPN `1.24.1` or newer from source.

## Security

Prefer SAML authentication when available.

Avoid storing passwords in plain text.

Never commit real VPN configuration files, credentials, tokens, internal hosts, or private certificates.
