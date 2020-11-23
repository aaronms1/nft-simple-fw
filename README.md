# nft-simple-fw

nftables framework ruleset comparting traffic into different zones.  
Supports private subnets and vpn interfaces and provides a custom template for [nft-blackhole](https://github.com/tomasz-c/nft-blackhole).
  
Uses performant [interface group](https://www.freedesktop.org/software/systemd/man/systemd.network.html#Group=) matching for distinguishing interfaces.

## default zone policies

### filter zones

- public, private, forward: reject
- default: reject
- vpn, output: accept

## Usage

- Customize [conf.def](conf.def) and the filter zones in [zones](zones/) to your liking if you don't agree with the sane defaults.
- Assign interface groups via systemd-networkd (see [conf.def](conf.def)), create/edit `services.d/{forward,private,public,vpn}/*.nft` files for the services you wish to use and load via `nft -f fw.nft`, or use the supplied systemd service file [systemd/nft-fw.service](systemd/nft-fw.service), which expects the files to reside in `/etc/nftables`.
- If you are using nft-blackhole, symlink [nft-blackhole/template.nft](nft-blackhole/template.nft) to `/usr/share/nft-blackhole/nft-blackhole.template`.
