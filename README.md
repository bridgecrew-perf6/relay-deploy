This repo contains an Ansible role
to install a simple FreeSWITCH SIP relay
on the
[stack-deploy](https://github.com/tessercat/stack-deploy)
stack.

The config has two SIP profiles/interfaces:

- a single SIP/TLS and SRTP-only inbound profile
  that accepts all inbound calls.
- a single outbound profile
  and no-register,
  username/password,
  SIP/TLS and SRTP-only gateway.

Deploy vars define
Inbound/outbound SIP ports,
RTP ports
and gateway credentials.

The outbound gateway
works with VoIP.ms,
which expects RPID caller ID.
I haven't tested other providers.

By default,
the dialplan rejects all inbound calls.
Copy and rename the `diaplan/relay-template.xml` file
to `dialplan/relay.xml.d/`
for each dialed number
to relay through the gateway,
replacing template values for
`dialed_number` and `destination_number`.

Iptables rules
allow all access
to the inbound profile's port
and to the entire RTP port range.

To call through the gateway,
call `<dialed_number>@<relay_domain>:<relay_inbound_sip_port>`
with a SIP/TLS and SRTP-capable client,
and if there's an extension matching `<dialed_number>`,
the call goes through the gateway
to `<destination_number>`.

The inbound profile
forces SIP/TLS and SRTP on inbound legs.

The template and outbound profile/gateway
force SIP/TLS and SRTP on outbound legs.

The outbound profile/gateway
authenticates calls,
but the config provides no directory,
so incoming calls to the profile will fail.
Also, the deploy role
doesn't install iptables rules
to allow access to the outbound profile's port,
though the stack's conntrack `ESTABLISHED` rule
allows inbound traffic from the gateway
for up five days.
