title = "IPv6 prefix delegation with static address and dhclient on Ubuntu / Debian"
datetime = "2015-05-09 10:47"
tags = ["IPv6", "DHCPv6"]
------------
Some hosting providers, like [Online](http://online.net) in France provide IPv6 access via prefix delegation. You get a DUID that has to be sent to their DHCP server. The IP address is configured by the client.

The recommended method is using Dibbler, but this solution is pretty buggy.

When using `dhclient` on Ubuntu or Debian (other systems might have a similar behaviour) with a static address, the interface doesn't end up with the IP set. The solution is to pass the `-S` option to dhclient, to only request stateless configuration.

Below are samples of `dhclient6.conf` and `interfaces`. Note that in this configuration everything is configured by the client, but you could request a name server from the DHCP server.

`/etc/dhcp/dhclient6.conf`:

    interface "em1" {
        send dhcp6.client-id your_duid;
        request;
    }

`/etc/network/interfaces`:

    auto em1
    iface em1 inet6 static
        address your_ip_v6_address
        netmask your_nemask
        accept_ra 1
        post-up /sbin/dhclient -cf /etc/dhcp/dhclient6.conf -6 -S -P em1
