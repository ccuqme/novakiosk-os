# novakiosk-os

[![bluebuild build badge](https://github.com/ccuqme/novakiosk-os/actions/workflows/build.yml/badge.svg)](https://github.com/ccuqme/novakiosk-os/actions/workflows/build.yml)
![License](https://img.shields.io/github/license/ccuqme/novakiosk-os)

`novakiosk-os` is a minimal [BlueBuild](https://blue-build.org) Fedora Atomic image recipe
for kiosk machines managed by [novakiosk](https://github.com/ccemqu/novakiosk) (optionally
with [novakeys](https://github.com/ccemqu/novakeys)).

`novakiosk` communicates with a kiosk agent running on the kiosk machine; 
this image provides the Sway session + browser and the OS-level dependencies that the kiosk agent expects.

## What’s included

- Sway session with auto-login via `greetd` (starts Sway as the `nova` user)
- Firefox as a system Flatpak (`org.mozilla.firefox`) for kiosk display
- `ydotool` for kiosk agent automation (e.g. forcing refresh / sending input)
- `wayvnc` for remote view/control (used by novakiosk’s VNC feature)
- `openssh-server` + `openssl` for direct SSH access and key management
- Build tooling used by kiosk-agent dependencies (e.g. `gcc-c++` for `node-pty`, used for novakiosk's
  built-in web terminal)

## Intended use

This repo is primarily an internal recipe, but it is shared for anyone building kiosks with
`novakiosk`/`novakeys` who wants a small, reproducible Fedora Atomic base.

The defaults are opinionated. If you need a different setup, expect to fork and
adjust the recipe.

Note: this repo does not ship the kiosk agent itself; it only provides the OS image and its
dependencies. The kiosk agent is served when onboarding the kiosk via the novakiosk web interface.

This project was built for [NOVA Spektrum](https://novaspektrum.no) in Norway and is
published with their permission. It is maintained by the author personally and
is not maintained, supported, or warranted by NOVA Spektrum.

NOVA Spektrum is a registered name and is not covered by this license.

## Installation (rebase)

> [!WARNING]  
> [This is an experimental feature](https://www.fedoraproject.org/wiki/Changes/OstreeNativeContainerStable),
> try at your own discretion.

To rebase an existing Fedora Atomic system to the latest build:

- First rebase to the unsigned image, to get the proper signing keys and policies installed:
  ```
  rpm-ostree rebase ostree-unverified-registry:ghcr.io/ccuqme/novakiosk-os:latest
  ```
- Reboot to complete the rebase:
  ```
  systemctl reboot
  ```
- Then rebase to the signed image, like so:
  ```
  rpm-ostree rebase ostree-image-signed:docker://ghcr.io/ccuqme/novakiosk-os:latest
  ```
- Reboot again to complete the installation
  ```
  systemctl reboot
  ```

## ISO

Prebuilt ISO snapshot: `novakiosk-os.iso` (built February 2026)

Checksum (SHA256): 8a7109c62a62cae021691adf88f6028b8ad822dbf0d3a5cb1cf3732c7a974f93

[Download](http://static.nova.onl/novakiosk-os.iso)

## Customization

### Change the kiosk username

The kiosk user is hard-coded to `nova` and is created at image build time (via `sysusers.d`
and `tmpfiles.d`). To change it, fork the repo and update all of the following:

- `files/etc/greetd/config.toml` (the `initial_session.user`)
- `files/system/usr/lib/sysusers.d/90-novakiosk.conf` (the user/group definition)
- `files/system/usr/lib/tmpfiles.d/90-novakiosk.conf` (home directory creation/ownership)

After changing these, build/publish your forked image and rebase to your own image reference.