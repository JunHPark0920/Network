# Subnetting

The main purpose of subnetting is to use IP address space more efficiently and divide a larger network into smaller, manageable networks.

This concept was introduced in `5-1 Network_Layer` together with **Classless Addressing / CIDR**.

Classless addressing allows networks to use variable-length prefixes instead of being restricted to the traditional Class A, B, and C boundaries.

Subnetting uses this flexibility to divide an allocated network into smaller subnets according to network requirements.

## Two Types of Subnetting

### 1. FLSM (Fixed Length Subnet Mask)

In this subnetting type, every subnet has:

- the same size
- the same subnet mask
- the same number of host bits
- the same number of usable hosts

**Example**

In the network `150.16.0.0/16`, `/16` means that 16 bits are used for the network portion.

Since IPv4 contains 32 bits:

`32 - 16 = 16 host bits`

Therefore:

`Network bits = 16`

`Host bits = 16`

`Subnet mask = 255.255.0.0`

This is the original network before additional subnetting is performed.

### Designing a Subnet for a Required Number of Hosts

If we only need 30,000 hosts, we need to determine how many host bits are necessary.

`2^15 - 2 = 32,766`

This is enough for 30,000 hosts.

However:

`2^14 - 2 = 16,382`

would not be enough.

Therefore, we need **15 host bits**.

Since the original network has 16 host bits, one host bit can be changed into a network bit.

**Example**

- Original: Network = 16 bits, Host = 16 bits
- Changed: Network = 17 bits, Host = 15 bits

We took 1 bit from the host portion and added it to the network portion.

This bit is called a **borrowed bit**.

### Calculating Number of Subnets

**Formula**:

`2^(borrowed bits)`

**Example**

If we borrow 3 bits:

`2^3 = 8`

Therefore, we create **8 subnets**.

### Finding the Next Subnet

There are two simple approaches.

#### Borrowed Bits

We can change the combination of borrowed bits to identify each subnet.

**Example**

If only 1 bit is borrowed, it has two possible values:

- `Subnet 1 → borrowed bit = 0`
- `Subnet 2 → borrowed bit = 1`

Therefore, borrowing 1 bit creates 2 possible subnets.

#### Adding 1 to Previous Broadcast

The next subnet can also be found using:

`Next Network Address = Previous Broadcast Address + 1`

When the last octet reaches `255`, adding 1 carries over into the next octet.

For example:

`192.168.0.255 + 1 → 192.168.1.0`

### 2. VLSM (Variable Length Subnet Mask)

In this subnetting type, different subnets can have:

- different sizes
- different subnet masks
- different numbers of host bits
- different numbers of usable hosts

**Example**

Unlike FLSM, subnet sizes do not need to be equal.

If Network A requires 30,000 hosts and Network B requires 4,000 hosts, we can allocate a larger subnet to Network A and a smaller subnet to Network B.

Start with Network A because it requires more hosts.

The minimum number of host bits required for 30,000 hosts is 15:

`2^15 - 2 = 32,766`

Therefore, Network A needs 15 host bits:

`32 - 15 = /17`

For Network B, 12 host bits are required:

`2^12 - 2 = 4,094`

Therefore:

`32 - 12 = /20`

Network B requires a `/20` subnet.

#### Most Important VLSM Rule

The general rule is:

**"Allocate the network requiring the most hosts first."**

**Example**

`Largest network → Second largest network → ... → Smallest network`

This is because VLSM allocates different-sized blocks from the available address space.

If smaller subnets are allocated without careful planning first, the remaining address space may become fragmented, making it difficult to allocate a sufficiently large contiguous block for a larger network.