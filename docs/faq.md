# Frequently Asked Questions

## Can I use the version available through apt?

Usually not.

Many Linux distributions provide outdated OpenFortiVPN packages.

Check your version:

```bash
openfortivpn --version
```

For SAML authentication, use OpenFortiVPN 1.24.1 or newer.

---

## How do I check if SAML is supported?

Run:

```bash
openfortivpn --help | grep -i saml
```

If no SAML-related options are displayed, install a newer version.

---

## Can I delete the cloned repository after installation?

Yes.

After:

```bash
sudo make install
```

the binary is installed into the system.

Verify:

```bash
openfortivpn --version
```

Then remove the repository:

```bash
rm -rf openfortivpn
```

---

## Do I need sudo?

Yes.

OpenFortiVPN needs elevated privileges to:

* create tunnel interfaces
* modify routes
* update DNS settings
* manage PPP

---

## How do I disconnect?

Press:

```text
CTRL + C
```

The VPN tunnel will be terminated.

---

## How do I verify the VPN tunnel?

Run:

```bash
ip addr
```

Look for:

```text
ppp0
```

or:

```text
tun0
```

depending on the VPN configuration.

---

## How do I verify routes?

Run:

```bash
ip route
```

You should see VPN-related routes after a successful connection.

---

## Can I use username and password authentication?

Yes.

Use:

```bash
./scripts/vpn-legacy
```

with a valid configuration file.

However, SAML is the recommended approach whenever supported.

---

## Can I store my password inside the configuration file?

Technically yes.

Recommended:

```text
No
```

Prefer SAML authentication whenever available.

Never commit credentials to source control.

---

## Can I use MFA?

Yes.

SAML authentication supports MFA through the configured identity provider.

Examples:

* Microsoft Entra ID
* Azure AD
* Okta
* Google Workspace

---

## How do I update OpenFortiVPN?

Verify the installed version:

```bash
openfortivpn --version
```

Build and install a newer release:

```bash
git checkout <new-version>
./autogen.sh
./configure
make -j"$(nproc)"
sudo make install
```

---

## How do I verify DNS resolution?

Run:

```bash
dig vpn.example.com
```

Expected:

```text
Public IP address
```

---

## How do I verify HTTPS connectivity?

Run:

```bash
curl -vk https://vpn.example.com
```

The gateway should return a valid response.

---

## How do I run diagnostics?

Run:

```bash
vpn-doctor
```

The script validates:

* OpenFortiVPN installation
* SAML support
* DNS resolution
* HTTPS connectivity
* Required dependencies

---

## Where should I report issues?

Include:

```bash
openfortivpn --version
ip addr
ip route
vpn-doctor
```

These commands provide enough information for initial troubleshooting.
