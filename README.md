# nft-simple-fw

nftables framework ruleset comparting traffic into different zones.  
Supports private subnets and vpn interfaces and provides a custom template for [nft-blackhole](https://github.com/tomasz-c/nft-blackhole).
  
Uses performant [interface group](https://www.freedesktop.org/software/systemd/man/systemd.network.html#Group=) matching for distinguishing interfaces.

## zone policies

Packets traverse differents zones depending on distinct criteria, mainly the interface group and/or destination address:

- **[public](zones/public.nft)**: Packets coming from public ip ranges, passing through the main interface(s) (group **1** by default).
- **[private](zones/private.nft)**: Packets coming from local networks, passing through the main interface(s) (group **1** by default).
- **[forward](zones/forward.nft)**: Packets destined for other machines or subnets not directly configured on the host but expected to be forwarded.
- **[vpn](zones/vpn.nft)**: Packets passing through VPN interface(s) (group **2** by default).
- **[output](zones/output.nft)**: Packets originating from the host itself.
- **[default](zones/default.nft)**: Packets not matching any of the above interface groupings (group is **unset**).

Without any configuration, all zones but **output** and **vpn** are configured to reject packets by default if not matching some pre-defined criteria. Please see the zone's source comments for details.

### caveats

Packets to **ungrouped** interfaces traverse the *[default](zones/default.nft)* zone and are only subject to mild filtering when originating from inside the local network. Assign an interface group or customize the zone to change this behaviour. Packets to **grouped** interfaces, **excluding** `IIFGROUP_{ETHERNET,VPN}` (see [conf.def](conf.def)), are **rejected** by default.

## Usage

- Customize [conf.def](conf.def) and the filter zones in [zones](zones/) to your liking if you don't agree with the sane defaults.
- Assign interface groups via systemd-networkd (see [conf.def](conf.def)), create/edit `services/{default,forward,private,public,vpn,output}.zone.d/*.nft` files for the services you wish to use.
- Load via `nft -f fw.nft`, or use the supplied systemd service file [systemd/nft-fw.service](systemd/nft-fw.service), which expects the files to reside in `/etc/nftables`.
- If you are using [nft-blackhole](https://github.com/tomasz-c/nft-blackhole), symlink [nft-blackhole/template.nft](nft-blackhole/template.nft) to `/usr/share/nft-blackhole/nft-blackhole.template`.
- If you want to keep your kernel log clean, an example configuration for [ulogd2](https://www.netfilter.org/projects/ulogd/index.html) is provided with [ulogd2.conf](ulogd2/ulogd2.conf). Only `CHAIN_LOG_DISPATCH_{DROP,REJECT}` inside [conf.def](conf.def) need to be set accordingly to utilize the *NFLOG* target.
