# Goplesk

**A server control panel and app platform in one small binary.** Goplesk runs DNS, routing with automatic HTTPS, zero-downtime app releases, backups and multi-server clusters, and it's built for VPS machines with 2 GB of RAM or less.

This repository holds **downloads, the install script and the public issue tracker**. Goplesk is commercial software: the source code isn't published here.

🌐 **Website, pricing and customer portal:** https://goplesk.caaqit.com

---

## Install

On a fresh Linux server (x86_64 or arm64, systemd), as root:

```bash
curl -fsSL https://goplesk.caaqit.com/install.sh | sudo sh
```

The script downloads the binary for your CPU, **checks its SHA-256** against the release's `checksums.txt`, installs it to `/usr/local/bin/goplesk`, shows the license agreement and sets the server up. Pass options to the installer after `-s --`:

```bash
# Control plane + DNS on one server, accepting the EULA without a prompt
curl -fsSL https://goplesk.caaqit.com/install.sh | sudo sh -s -- --accept-eula --role=dns,control-plane

# Add a worker node to an existing cluster
curl -fsSL https://goplesk.caaqit.com/install.sh | sudo sh -s -- --accept-eula --role=worker --token=<cluster key>
```

The installer prints the admin password and cluster key **once**. Open `https://<server-ip>:8443` to sign in.

| Variable | Effect |
|---|---|
| `GOPLESK_VERSION=v1.5.0` | Install a specific release instead of the latest |
| `GOPLESK_BINARY_ONLY=1` | Only install the binary; don't set anything up |
| `GOPLESK_ACCEPT_EULA=yes` | Accept the license agreement without a prompt |

Prefer to check the script first? Download it, read it, then run it:

```bash
curl -fsSLO https://github.com/hmmdlthf/goplesk-releases/releases/latest/download/install.sh
less install.sh
sudo sh install.sh
```

## Upgrade

Run the same install command. On a server that's already set up, it replaces the binary and restarts the `goplesk` service. Your sites keep their configuration.

## License

Goplesk needs a license key to make changes. Without one the panel is read-only, but **your sites keep serving**.

```bash
goplesk license trial you@example.com          # free 14-day trial
goplesk license activate GPLK-XXXX-XXXX-XXXX   # activation code from the customer portal
goplesk license install ./license.key          # offline license (no internet needed)
goplesk license info                           # edition, limits, install ID
```

Plans and prices are at https://goplesk.caaqit.com. By installing you agree to the [end user license agreement](https://github.com/hmmdlthf/goplesk-releases/releases/latest/download/EULA.txt) (`goplesk eula` prints it). Open-source components and their licenses are listed in [`NOTICES.txt`](https://github.com/hmmdlthf/goplesk-releases/releases/latest/download/NOTICES.txt) (`goplesk notices`).

## What's included

* **Authoritative DNS** for your domains, with guided domain onboarding.
* **Reverse proxy with automatic HTTPS**: Let's Encrypt certificates are issued and renewed for every routed domain.
* **Zero-downtime releases**: upload a binary or static site with `goplesk-cli push`. The new release must pass a health check before traffic moves to it, and a failed release rolls back on its own.
* **Multi-server clusters**: add worker nodes, and apps are placed and moved automatically.
* **Encrypted backups** of server state, kept locally or sent to any S3-compatible storage.
* **Brute-force protection**: repeated failed logins are blocked at the firewall.
* **Database tunnels**: connect local tools to production databases without opening public ports.
* **Low memory use**: around 20 MB at idle.

## Downloads

Every [release](https://github.com/hmmdlthf/goplesk-releases/releases) contains:

| File | For |
|---|---|
| `goplesk-linux-amd64`, `goplesk-linux-arm64` | The server binary |
| `goplesk-cli-<os>-<arch>` | The developer CLI (`push`, `tunnel`) for Linux, macOS and Windows |
| `install.sh` | The installer above |
| `checksums.txt` | SHA-256 of every file |
| `EULA.txt`, `NOTICES.txt` | License agreement and open-source notices |

Verify a manual download:

```bash
sha256sum --ignore-missing -c checksums.txt
```

## Support

* **Bugs and feature requests:** [open an issue](https://github.com/hmmdlthf/goplesk-releases/issues/new/choose).
* **Billing and licenses:** use the customer portal at https://goplesk.caaqit.com.
* **Security problems:** see [SECURITY.md](SECURITY.md). Please don't open a public issue.
