# nft-simple-firewall

Basic nft firewall ruleset with a custom template for [nft-blackhole](https://github.com/tomasz-c/nft-blackhole), supports private subnets and vpn interfaces.
  
Uses performant [interface group](https://www.freedesktop.org/software/systemd/man/systemd.network.html#Group=) matching for distinguishing interfaces, `1` for ethernet, `2` for vpn devices.

## default policies

- public, private, forward: `reject` (see `common` chain)
- vpn: `accept`


## Usage

- Assign interface groups via systemd-networkd, create/edit `services.d/{forward,private,public,vpn}/*.nft` files for the services you wish to use and load via `nft -f sfw.nft` or use the supplied systemd service file [systemd/nft-sfw.service](systemd/nft-sfw.service).
- If you are using nft-blackhole, copy or link [nft-blackhole/template.nft](nft-blackhole/template.nft) to `/usr/share/nft-blackhole/nft-blackhole.template`.
