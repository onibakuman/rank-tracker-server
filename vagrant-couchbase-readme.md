# Vagrant Couchbase

Vagrantfile and supporting bash bootstrap scripts to build a multi-node cluster.  Cluster
size and bucket size can be defined when provisioned.

## Use

The standard `vagrant up` will default to a 2 node clustser with 512mb ram each. Number of nodes and node
size can be overriden by env variables:

```
NODES=4 SIZE=1000 vagrant up
```

Will provision 4 nodes with 1gb ram each.

