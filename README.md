# nft-simple-blackhole

Basic nft firewall ruleset with a custom template for [nft-blackhole](https://github.com/tomasz-c/nft-blackhole), supports private subnets and vpn interfaces.
  
Uses performant [interface group](https://www.freedesktop.org/software/systemd/man/systemd.network.html#Group=) matching for distinguishing interfaces, `1` for ethernet, `2` for vpn devices.

## Usage

- Assign interface groups via systemd-networkd, edit `*-svc.nft` files and load via `nft -f fw.nft` or use the supplied systemd service file.
- If you are using nft-blackhole, copy `nft-blackhole/nft-blackhole.template.nft` to `/usr/share/nft-blackhole/nft-blackhole.template`, dropping the nft extension and overwriting the default template.
