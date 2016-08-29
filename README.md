# pypureomapi feature path

this is a patch for [pypureomapip library](https://github.com/CygnusNetworks/pypureomapi) to create Foreman-Like record in dhcp.leases file.

It is needed in case you are migration the network to support Foreman-DHCP Smart Proxy, but you already have hosts on the network.
So you can add them to leases file with this library.

the typical DHCP entry, created by Foreman-Proxy, is this one:

```
host test.host.com {
  dynamic;
  hardware ethernet 00:01:02:03:04:05;
  fixed-address 1.2.3.4;
        supersede server.filename = "pxelinux.0";
        supersede server.next-server = 8d:8d:8d:dd;
        supersede host-name = "test.host.com";
}

```

## Usage

```python
#!/usr/bin/env python

import pypureomapi
from pypureomapi import OmapiMessage
import sys
import struct

keyname = "omapi_key"
secret  = "UbT4vLcXShHPvOSB6.....rR/TVX65DkuucPORIg=="
server  = "0.0.0.0"
port    = 7911

try:
    oma = pypureomapi.Omapi(server, port, keyname, secret)
except pypureomapi.OmapiError, err:
    print "OMAPI error: %s" % (err,)
    sys.exit(1)


# add host
oma.add_host_like_foreman("1.2.3.4", "00:01:02:03:04:05", "test.host.com", "pxelinux.0", "8d:8d:8d:dd")

# delete host
oma.del_host("00:01:02:03:04:05")
```
