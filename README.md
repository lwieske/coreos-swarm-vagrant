# CoreOS Cluster Prepared for Swarm Provisioning

```
vagrant up --provider virtualbox
```

```                                                                       
┌─────────────────────────────────────────────────────────────────────────────────────────┐
│                                  10.10.10                                               │
└─────────┬────────────┬────────────┬──────────────┬────────────┬────────────┬────────────┤
          │            │            │              │            │            │            │
       .11│         .12│        .13 │          .101│        .102│        .103│        .104│
          │            │            │              │            │            │            │
     ┌────┤       ┌────┤       ┌────┤         ┌────┤       ┌────┤       ┌────┤       ┌────┤
     │eth1│       │eth1│       │eth1│         │eth1│       │eth1│       │eth1│       │eth1│
┌────┴────┤  ┌────┴────┤  ┌────┴────┤    ┌────┴────┤  ┌────┴────┤  ┌────┴────┤  ┌────┴────┤
│         │  │         │  │         │    │         │  │         │  │         │  │         │
│ etcd-01 │  │ etcd-02 │  │ etcd-03 │    │ node-01 │  │ node-02 │  │ node-03 │  │ node-04 │
│         │  │         │  │         │    │         │  │         │  │         │  │         │
│         │  │         │  │         │    │ swarm   │  │ swarm   │  │ swarm   │  │ swarm   │
│         │  │         │  │         │    │         │  │         │  │         │  │         │
│         │  │         │  │         │    │ master  │  │         │  │         │  │         │
│         │  │         │  │         │    │         │  │         │  │         │  │         │
│         │  │         │  │         │    │         │  │ agent   │  │ agent   │  │ agent   │
│         │  │         │  │         │    │         │  │         │  │         │  │         │
│ etcd    │  │ etcd    │  │ etcd    │    │         │  │         │  │         │  │         │
│         │  │         │  │         │    │         │  │         │  │         │  │         │
└─────────┘  └─────────┘  └─────────┘    └─────────┘  └─────────┘  └─────────┘  └─────────┘
```

```
$ docker -H 10.10.10.100:3375 info
```

```
Containers: 5
 Running: 5
 Paused: 0
 Stopped: 0
Images: 4
Role: primary
Strategy: spread
Filters: health, port, dependency, affinity, constraint
Nodes: 4
 node-01: 10.10.10.100:2375
  └ Status: Healthy
  └ Containers: 2
  └ Reserved CPUs: 0 / 2
  └ Reserved Memory: 0 B / 2.056 GiB
  └ Labels: executiondriver=native-0.2, kernelversion=4.4.1-coreos, operatingsystem=CoreOS 955.0.0, storagedriver=overlay
  └ Error: (none)
  └ UpdatedAt: 2016-02-16T22:29:07Z
 node-02: 10.10.10.101:2375
  └ Status: Healthy
  └ Containers: 1
  └ Reserved CPUs: 0 / 2
  └ Reserved Memory: 0 B / 2.056 GiB
  └ Labels: executiondriver=native-0.2, kernelversion=4.4.1-coreos, operatingsystem=CoreOS 955.0.0, storagedriver=overlay
  └ Error: (none)
  └ UpdatedAt: 2016-02-16T22:29:19Z
 node-03: 10.10.10.102:2375
  └ Status: Healthy
  └ Containers: 1
  └ Reserved CPUs: 0 / 2
  └ Reserved Memory: 0 B / 2.056 GiB
  └ Labels: executiondriver=native-0.2, kernelversion=4.4.1-coreos, operatingsystem=CoreOS 955.0.0, storagedriver=overlay
  └ Error: (none)
  └ UpdatedAt: 2016-02-16T22:28:58Z
 node-04: 10.10.10.103:2375
  └ Status: Healthy
  └ Containers: 1
  └ Reserved CPUs: 0 / 2
  └ Reserved Memory: 0 B / 2.056 GiB
  └ Labels: executiondriver=native-0.2, kernelversion=4.4.1-coreos, operatingsystem=CoreOS 955.0.0, storagedriver=overlay
  └ Error: (none)
  └ UpdatedAt: 2016-02-16T22:29:09Z
Plugins:
 Volume:
 Network:
Kernel Version: 4.4.1-coreos
Operating System: linux
Architecture: amd64
CPUs: 8
Total Memory: 8.224 GiB
Name: node-01
$
```

### Configuration (Head of Vagrantfile)

```
members = {
  #  Name     #, CPU, RAM, 1ST_IP
  'etcd' => [ 3,   1,  256,     11 ],
  'node' => [ 4,   2, 2048,    100 ],
}
PREFIX   = "10.10.10"
```
