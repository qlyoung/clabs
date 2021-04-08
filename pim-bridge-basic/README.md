# pim-bridge-basic

Simple bridge topology with an RP and two PIM routers, each with an alpine box
attached.

## Notes

This uses a node of kind `bridge`, which containerlab does not create, so it
needs to be created manually like so:

```
ip link add pimbridge type bridge
```

Ensure `net.ipv4.ip_forward` is enabled and `iptables -P FORWARD ACCEPT` is
set.

To run:

```
containerlab deploy --topo lab.yaml
```

## Topology

```
                       ┌──────────────────────┐
                       │                      │
                       │          rp          │
                       │                      │
                       └───────────┬──────────┘
                      10.10.1.1/24 │ eth1
                                   │
                                   │
                ┌──────────────────┴────────────────┐
   10.10.1.3/24 │ eth1                         eth1 │ 10.10.1.2/24
       ┌────────┴─────────┐                ┌────────┴────────┐
       │                  │                │                 │
       │       lhr        │                │       fhr       │
       │                  │                │                 │
       └────────┬─────────┘                └────────┬────────┘
           eth2 │                                   │ eth2
                │                                   │
                │ eth1                         eth1 │
       ┌────────┴─────────┐                ┌────────┴────────┐
       │    consumer      │                │      source     │
       └──────────────────┘                └─────────────────┘
```
