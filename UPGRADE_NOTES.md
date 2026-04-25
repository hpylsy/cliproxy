# Upgrade Notes

The one-click installer targets `/root/cliproxyapi` when run as root.

Expected installer changes:
- backs up `/root/cliproxyapi/config.yaml` to `/root/cliproxyapi/config_backup/config_*.yaml`
- extracts the latest release to `/root/cliproxyapi/<version>/`
- replaces `/root/cliproxyapi/cli-proxy-api`
- restores `/root/cliproxyapi/config.yaml` from the backup
- regenerates `/root/cliproxyapi/cliproxyapi.service`
- writes `/root/cliproxyapi/version.txt`
- may remove old version directories, keeping the latest two
- creates or updates `/root/.config/systemd/user/cliproxyapi.service`

Important local difference:
- production currently uses `/etc/systemd/system/cliproxyapi.service`
- the installer manages the user service path, so verify the system service
  after upgrading

Post-upgrade checklist:
- confirm `/root/cliproxyapi/config.yaml` still has the Nvidia mappings
- confirm `/etc/systemd/system/cliproxyapi.service` still starts the intended binary
- restart `cliproxyapi.service` if the installer only touched the user service
- disable the root user-level service if it was enabled by the installer:
  `systemctl --user stop cliproxyapi.service && systemctl --user disable cliproxyapi.service`
- confirm `pioneer-vision-preprocess.service` still points to port `8317`
- test `gpt-5.4-mini` and `gpt-5.2` through `http://127.0.0.1:8320/v1/responses`

Observed during the 6.9.34 -> 6.9.38 upgrade:
- the installer created/enabled `/root/.config/systemd/user/cliproxyapi.service`
- the production system service stayed at `/etc/systemd/system/cliproxyapi.service`
- the user service attempted to start and conflicted with the system service on port `8317`
- resolution: stop/disable the user service and keep the system service active
