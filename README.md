# novakiosk-os &nbsp; [![bluebuild build badge](https://github.com/ccuqme/novakiosk-os/actions/workflows/build.yml/badge.svg)](https://github.com/ccuqme/novakiosk-os/actions/workflows/build.yml)

novakiosk-os is a minimal [BlueBuild](https://blue-build.org) OS image recipe for
[novakiosk](https://github.com/ccemqu/novakiosk) kiosks. It provides a
Sway-based auto-login session for a dedicated kiosk user, with Firefox as the
default browser, plus the dependencies required by both
[novakiosk](https://github.com/ccemqu/novakiosk) and
[novakeys](https://github.com/ccemqu/novakeys).

This image is intentionally small and focused. It includes packages used for:
- Remote control via terminal access (`node-pty`; requires build-time tooling such as `gcc-c++`)
- Remote control via VNC (`wayvnc`)
- Virtual keyboard automation (`ydotool` for novakeys)
- Secure access and management (`openssl` and `openssh-server`)

This project was built for [NOVA Spektrum](https://novaspektrum.no) in Norway and is
published with their permission. It is maintained by the author personally and
is not maintained, supported, or warranted by NOVA Spektrum.

NOVA Spektrum is a registered name and is not covered by this license.

## Installation

> [!WARNING]  
> [This is an experimental feature](https://www.fedoraproject.org/wiki/Changes/OstreeNativeContainerStable), try at your own discretion.

To rebase an existing atomic Fedora installation to the latest build:

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
Download link to be added at a later date.
