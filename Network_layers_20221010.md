# Network layers

## Application layer

- Uses high-level protocols that set a standard between endpoints
  - e.g. SSH, SMTP, HTTPS
- Doesn't determine how the data is transmitted, only what should be sent
  - Means that the same application layer will work with ethernet/a modem/wifi
    etc.

## Transport layer

- Uses TCP/UDP/etc. to establish a connection with the recipient
- Splits data into packets

## Network layer

- Uses the internet protocol (IP) to address packets with the source and
  destination IP address
- A router forwards each packet towards an endpoint (called a socket) defined by
  an IP and port number
- Routers use a routing table to instruct the next hop

## Link layer

- Operates across a physical connection
- This is what you'd swap out for Avian Carriers in IPoAC
- Adds the mac address of the NIC that packets should be sent to
- The mac address changes with each hop

