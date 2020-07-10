# DuckDNS

This is an Ansible role for updating DuckDNS when a network interface changes its IP address. It hooks into the DHCP client and updates the given DuckDNS record whenever the IP address of the given device changes.

## Configuration

All of the following variables must be present:

```yaml
duckdns:
  interface:
  domain:
  token:
```

Get `domain` and `token` from [duckdns.org](http://duckdns.org). `interface` is the local device interface of which you'd like to publish its address to DuckDNS, e.g. `wlan0` or `eth0`.

# Debugging

* Show the local IP address with `ip -4 a show dev wlan0`
* Force renewal of IP address with `sudo dhclient wlan0`
* Check the log file in `/var/log/duckdns.log`
* There is a debug script in `/etc/dhcp/dhclient-enter-hooks.d/debug` which produces output like this:

  ```
  Sun Jun 28 11:11:07 CEST 2020: entering /etc/dhcp/dhclient-enter-hooks.d, dumping variables.
  reason='PREINIT'
  interface='wlan0'
  --------------------------
  Sun Jun 28 11:11:07 CEST 2020: entering /etc/dhcp/dhclient-exit-hooks.d, dumping variables.
  reason='PREINIT'
  interface='wlan0'
  --------------------------
  Sun Jun 28 11:11:07 CEST 2020: entering /etc/dhcp/dhclient-enter-hooks.d, dumping variables.
  reason='BOUND'
  interface='wlan0'
  new_ip_address='192.168.8.102'
  new_network_number='192.168.8.0'
  new_subnet_mask='255.255.255.0'
  new_broadcast_address='192.168.8.255'
  new_routers='192.168.8.1'
  new_domain_name_servers='192.168.8.1 192.168.8.1'
  --------------------------
  Sun Jun 28 11:11:07 CEST 2020: entering /etc/dhcp/dhclient-exit-hooks.d, dumping variables.
  reason='BOUND'
  interface='wlan0'
  new_ip_address='192.168.8.102'
  new_network_number='192.168.8.0'
  new_subnet_mask='255.255.255.0'
  new_broadcast_address='192.168.8.255'
  new_routers='192.168.8.1'
  new_domain_name_servers='192.168.8.1 192.168.8.1'
  --------------------------
  ```
