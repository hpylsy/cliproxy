# CliProxyAPI Deployment Backup

This repository tracks the non-secret deployment state for the lab CliProxyAPI
server.

Tracked here:
- redacted `config.redacted.yaml`
- systemd service snapshots
- upstream version documentation/config examples
- management static assets that are safe to store

Not tracked here:
- real `config.yaml` values
- API keys or provider keys
- logs and error captures
- compiled release binaries

The server also keeps a full pre-upgrade rollback archive under
`/root/pre-upgrade-backups/`. Use that archive for emergency restoration of
secret-bearing runtime state.

