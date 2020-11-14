# nft-simple-firewall

Basic nft firewall ruleset with a custom template for [nft-blackhole](https://github.com/tomasz-c/nft-blackhole), supports private subnets and vpn interfaces.
  
Uses performant [interface group](https://www.freedesktop.org/software/systemd/man/systemd.network.html#Group=) matching for distinguishing interfaces.

## default policies

- public, private, forward: `reject` (see `common` chain)
- vpn: `accept`

## Usage

- Customize [conf.def](conf.def) and the filter zones in [zones](zones/) to your liking if you don't agree with the sane defaults.
- Assign interface groups via systemd-networkd (see [conf.def](conf.def)), create/edit `services.d/{forward,private,public,vpn}/*.nft` files for the services you wish to use and load via `nft -f sfw.nft` or use the supplied systemd service file [systemd/nft-sfw.service](systemd/nft-sfw.service).
- If you are using nft-blackhole, copy or link [nft-blackhole/template.nft](nft-blackhole/template.nft) to `/usr/share/nft-blackhole/nft-blackhole.template`.
