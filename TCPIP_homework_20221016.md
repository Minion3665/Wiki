# TCP/IP homework

## 1

### a

FTP

### b

OSI Layers:

- Application layer
- Transport layer
- Network layer
- Link layer

### c

Each packet has a header containing the MAC address of the next device it is
being sent to. When a packet reaches a router, the MAC address needs to change.
This is done by stripping the old MAC address (layer 4) and looking back at the
IP address (layer 3) so that layer 4 can add the new MAC address

## 2

### a

993- IMAP

(Even though the mark scheme says this is wrong, it's wrong, lots of different
ports are used for IMAP)

### b

The accountant must be using POP instead of IMAP. POP downloads emails to your
machine and deletes them from the server, whereas IMAP keeps them on the server.

### c

#### i

22

#### ii

84.134.4.128

#### iii

84.134.4.128:22

### d

SSH is encrypted, whereas HTTP sends data over the internet in plaintext. SSH
can create a 'tunnel', allowing one side to access the network from the
perspective of the other side through the tunnel. This means that HTTP traffic
could be sent through an encrypted SSH tunnel and no unencrypted data would be
sent over the internet.
