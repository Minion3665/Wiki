# Encryption & Internet security

## What is encryption?

Encryption is writing data in a form that only those authorized to read it can
read it. Most encryption methods have a "key" which you need to have to decrypt
the data, which bring us to...

### Symmetric encryption

Symmetric encryption is encryption where both sides have the same key. This is
often quite quick to calculate, however it raises the problem of "how do you
share the key?"

### Asymmetric encryption

Asymmetric encryption is an encryption method where you have a "public" key and
a "private" key. The public key can only encrypt data, whereas the private key
can decrypt data encrypted by the public key. This allows you to share the
public key wherever you want, and nobody in the middle can intercept your
messages... well, unless they replaced your public key of course, then they
could read everything that was meant to be sent to you

### Signing keys
Signing allows you to trust that someone is who they say they are. HTTPS
certificates, for example, need to be signed by a *certificate authority* in
order for your browser to accept them. Your browser/operating system maker
provides a list of these authorities to trust, so that you can be relatively
sure that people are who they say they are. It isn't perfect, but it's generally
good enough.

## What is a firewall?

A firewall sees what traffic is coming into a machine and chooses what to do
with it. It can often decide to let it through, block it, or silently drop it

### What do firewalls act based on

Firewalls can act based on the IP address that sent the traffic and where it's
going to (i.e. what port it's on). For example, a system administrator may block
port `22` (the SSH port) to all machines outside the local network, but allow
port `443` (the HTTPS port) to everyone in order to host a website.

## What is a proxy?

A proxy sits between a client and a server, allowing routing the traffic to
servers that the client may not be able to able to access normally (i.e. for
region blocking reasons or because the server does not want to expose its IP).
Proxies may also block some traffic, for example a school may run a proxy that
attempts to block non-educational websites.

### Forwards proxies

Forwards proxies sit in front of a client for websites that it wants to
visit. They might be deployed by the client themselves (for example a proxy that
the client is using to get around region-locked content on Netflix) or they may
be deployed by an organization in order to stop clients from accessing certain
websites.

### Reverse proxies

<!-- spell-checker:words DDOS,proxying -->

Reverse proxies sit in front of a server, and clients are generally required to
go through it before getting to the server. They are deployed by the server
operator, rather than by someone on the client side. Examples are Cloudflare,
which prevents against DDOS attacks by reverse-proxying traffic, blocking or
challenging suspicious traffic, and providing caching so as not to overwhelm
your server. Similarly, a server might use NGINX to route traffic internally
(e.g. if you want to host multiple websites with only 1 IP address you can use a
reverse proxy to direct traffic based on the headers, or if you want to add
HTTPS in-front of an application that doesn't support it one way to do that is
to send it through a reverse proxy).


