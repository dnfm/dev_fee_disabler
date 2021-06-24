# dev_fee_disabler
Disable Ethereum miner dev fees at the network level.

# Usage

First, firewall out the SSL ethereum mining ports, as most miners have SSL hosts hard-coded in them, and we obviously can't affect those:

If the app is being run on the system that's doing the mining:

`iptables -A OUTPUT -p tcp --dport 5555 -j REJECT`
`iptables -A OUTPUT -p tcp --dport 9999 -j REJECT`

If the app is being run on your firewall, but system(s) on your network is/are mining:

`iptables -A FORWARD -p tcp --dport 5555 -j REJECT`
`iptables -A FORWARD -p tcp --dport 9999 -j REJECT`

If you use something other than iptables, you'll have to set it up to either block outgoing ports 5555 and 9999 on the mining system, or forwarding ports 5555 and 9999 on the firewall.  You may want to add more parameters to make it more specific.

Then, forward the non-SSL mining ports to NFQUEUE:

`iptables -A OUTPUT -p tcp --dport 4444 -j NFQUEUE`
`iptables -A OUTPUT -p tcp --dport 14444 -j NFQUEUE`

or

`iptables -A FORWARD -p tcp --dport 4444 -j NFQUEUE`
`iptables -A FORWARD -p tcp --dport 14444 -j NFQUEUE`

Then, edit this program with your parameters as you see fit (your Ethereum wallet address, verbosity, etc), and run it.

I've included an openrc run script and a logrotate config file you can use if you see fit.

This comes with no warranties, no guarantees, etc.  Use at your own risk.

# Dependencies

Requirements for this app to run are:

* Perl 5 (I'm not sure which specific version, but pretty much any modern-ish version should be ok.)
* Perl's NetPacket module (available on [CPAN](https://metacpan.org/pod/NetPacket) and probably in your distribution.)
* [nfqueue-bindings](https://github.com/chifflier/nfqueue-bindings)
