# Tailscale on LibreELEC
Tailscale static x86_64 client binary on LibreELEC

LibreELEC has empheremeal root read only filesystem, only /storage is persistent. systemd service file from this repo reflects that and keeps the state config in /storage/.config/tailscale/state

## TODO : Install notes
