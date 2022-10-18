# IP Addresses

## Reserved addresses

Some addresses are reserved:

- Addresses 10.x.x.x, 192.168.x.x are reserved for local networks only
- x.x.x.0 is reserved to refer to an entire network
- 127.x.x.x is a loopback address

<!-- spell-checker:words Netmasks,hostmask -->

## Class identifiers & Netmasks

IP addresses are made of 2 parts

- Network identifier (left-hand part), used to denote the network
- Host identifier (right-hand part), used to identify separate nodes on the
  network

You know how long the network identifier is by looking at the hostmask or the
class identifier:

Let's say your class identifier is "8". This means that 8 bits are the network
identifier, and the rest is the host identifier. You can also represent this as
a netmask by setting the first 8 bits to 1 and the rest to 0, being 255.0.0.0

<!-- spell-checker:words Classful -->

### Classful IP addressing

- Previously, there were classes "Class A" having a prefix of 8, and each
  subsequent class having a prefix of 8 more than the last

