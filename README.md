# Freeland DNS Balancer Standard Proposal

**DNS Balancer** is a service to deal with connetcivity problems based on MFCoin Blockchain of Freeland project.

## Who may use?

Developers for their Freeland-based projects.

## Notes

Balancer names is not a "standard" DNS names: they don't have any DNS zones, only names like localhost do. Balancer names can not replace system-reserved names (e.g. localhost, loopback etc.)

Balancer names may be used instead of DNS for serving static content from blockchain directly.

## Technical description

There is used NVS (Name Value System) for writing data to the blockhain.

NVS **name** could be `balancer:[name]`, where `[name]` is replaced with its name. For example, `balancer:mfid`.

**Content** for a few nodes:
```
type=nodes
nodes=node1.myproject.mfc,node2.myproject.net,222.333.444.555
probe=/speedprobe
```

Client will do periodic HEAD requests to http://[node]/[probe] for each node and can switch to the fastest (or just working) node any time. Probe url just must return `200` code with `Content-Type: application/node-probe` header.

Node also can return 302 code with next node domain (if it already present in the list) to switch own clients to another node before disappears.

For example above, requests wil be sent to `http://node1.myproject.mfc/speedprobe`, `http://node2.myproject.net` and `http://222.333.444.555/speedprobe`.

**Content** for a static data:
```
type=static
format=tar
zip=gzip
data=...BINARY DATA...
```

Where `format` is an archive format (tar is only supported for now, required) and `zip` (optional) specifies compression algorythm if present.

Note: all the content after the `data=` is interpreted as a binary one. So you need to place all the keys above.
