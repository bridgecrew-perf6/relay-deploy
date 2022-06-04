This repo contains an Ansible role
to install a simple FreeSWITCH SIP relay
on the
[stack-deploy](https://github.com/tessercat/stack-deploy)
stack.


# SIP/RTP iptables rules

Configure
iptables rules
to allow TCP connections
to the inbound profile's port and RTP ports
for both IPv4 and IPv6.

The outbound SIP profile
isn't meant to receive calls,
so don't configure an iptables rule.
Once a gateway establishes an outbound connection
to a trunk provider,
the stack's conntrack `ESTABLISHED` rule
allows inbound traffic on the trunk connection
for five days.


# FreeSWITCH

## Install

Install a very bare bones FreeSWITCH
from the SignalWire repositories.

A SignalWire Personal Access Token
name and token
must be configured in deploy vars.

Install `x86_64` or `armv7l`
based on lsb release.


## Configure

Install the
[relay-conf](https://github.com/tessercat/relay-conf)
FreeSWITCH configuration repo in `/etc/freeswitch/`,
install `vars.xml` and link timezone config.


## TLS certs

Install a Let's Encrypt renewal hook script
that writes new files to `/etc/freeswitch/tls/`
and emails the admin that FreeSWITCH
must be restarted.
