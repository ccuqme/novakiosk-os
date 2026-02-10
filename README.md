If you for some reason used this image, make sure you rebase to the image published on [github.com/novakiosk/os](https://github.com/novakiosk/os). This repo was only used for testing, and will not be updated any more.

To rebase an existing from this image to the official novakiosk-os image:
```
rpm-ostree rebase ostree-image-signed:docker://ghcr.io/novakiosk/os:latest
```
Then reboot.