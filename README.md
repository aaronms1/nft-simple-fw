# nft-simple-firewall

nftables framework ruleset comparting traffic into different zones.  
Supports private subnets and vpn interfaces and provides a custom template for [nft-blackhole](https://github.com/tomasz-c/nft-blackhole), s
  
Uses performant [interface group](https://www.freedesktop.org/software/systemd/man/systemd.network.html#Group=) matching for distinguishing interfaces.

## default zone policies

### filter type

- public, private, forward: `reject`
- vpn, output: `accept`

## Usage

- Customize [conf.def](conf.def) and the filter zones in [zones](zones/) to your liking if you don't agree with the sane defaults.
- Assign interface groups via systemd-networkd (see [conf.def](conf.def)), create/edit `services.d/{forward,private,public,vpn}/*.nft` files for the services you wish to use and load via `nft -f sfw.nft, or use the supplied systemd service file [systemd/nft-sfw.service](systemd/nft-sfw.service).
- If you are using nft-blackhole, symlink [nft-blackhole/template.nft](nft-blackhole/template.nft) to `/usr/share/nft-blackhole/nft-blackhole.template`.
