## Setup Tailscale and Connect Domain to Enable SSL with Caddy

### Requirements

- A TLD from providers such as porkbun, cloudflare, etc.
- Get account `API_KEY`, `API_SECRET_KEY` and enable API access in the domain-level
- Packages installed: `tailscale`, `dnsmasq`, `caddy`

### Setup

1. Do =tailscale up= and log in
2. Get the tailscale IP of the homelab, i.e. `100.100.49.81`
3. Change the entry in `configuration.nix`

```nix
services.dnsmasq = {
  enable = true;
  address = "/$DOMAIN/$TAILSCALE_IP"
};
```

4. In the Tailscale Web Dashboard, enter Settings and add a new Split DNS nameserver pointing to the same `$TAILSCALE_IP`, set it to a subdomain of the TLD (no need for an A-records)
5. put the `API_KEY` and `API_SECRET_KEY` from the domain provider to `/etc/caddy/envfile` and make sure it has `600` permission by doing

```bash
sudo chmod 600 /etc/caddy/envfile
```
