# Namespace

```bash
# create the pair of veth interfaces named, veth0 and veth1
ip link add veth0 type veth peer name veth1

```


<img src="Images/1.png" />

```bash

# confirm that veth0 is created
ip link show veth0

```

<img src="Images/2.png" />

```bash

# create the vnet0 network namespace
ip netns add vnet0

# assign the veth0 interface to the vnet0 network namespace
ip link set veth0 netns vnet0

# assign the 10.0.1.0/24 IP address range to the veth0 interface
ip -n vnet0 addr add 10.0.1.0/24 dev veth0

# bring up the veth0 interface
ip -n vnet0 link set veth0 up

# bring up the lo interface, because packets destined for 10.0.1.0/24
# (like ping) goes through the "local" route table
ip -n vnet0 link set lo up 

# confirm that the interfaces are up
ip -n vnet0 addr show

```
<img src="Images/3.png" />

```bash

```


```bash

```


```bash

```

```bash

```

Container runtime uses the namespace kernel feature to partition system resources to achieve a form of process isolation, such that changes to the resources in one namespace do not affect that in other namespaces. Example of such resources include process IDs, hostnames, user IDs, file names, and network interfaces.

Network namespace, in particular, virtualizes the network stack. Each network namespace has its own set of resources like network interfaces, IP addresses, routing tables, tunnels, firewalls etc. For example, iptables rules added to a network namespace will only affect traffic entering and leaving that namespace.

# Configure the 1st Network Namespace

Our first task is to create a new pair of veth interfaces, veth0 and veth1, by using the ip link add command:

```bash
# create the pair of veth interfaces named, veth0 and veth1
ip link add veth0 type veth peer name veth1

```
<img src="Images/3.png" width="500" height="300" />

The veth interfaces are usually created as interconnected pairs, where data transmitted on one end is immediately received on the other end. This type of interfaces is commonly used in container runtime to transfer packets between different network namespaces.

Let’s create our first network namespace, vnet0. Then we can assign the veth0 interface to this network namespace,

```bash
# create the vnet0 network namespace
ip netns add vnet0

# assign the veth0 interface to the vnet0 network namespace
ip link set veth0 netns vnet0

```

# Configure the 2nd Network Namespace

We will reuse the above commands to create our second network namespace, vnet1. Then we assign the veth1 interface to this network namespace, 

```bash
# create the vnet1 network namespace
ip netns add vnet1

# assign the veth1 interface to the vnet1 network namespace
ip link set veth1 netns vnet1
```