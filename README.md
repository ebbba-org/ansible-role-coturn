# Set up a TURN/STUN server for [BigBlueButton](https://bigbluebutton.org/)

This is an ansible-role to set up [coturn](https://github.com/coturn/coturn) and largely follows the [official BigBlueButton documentation](https://docs.bigbluebutton.org/admin/setup-turn-server.html).

## Setup

The role works on Ubuntu 20.04 and Centos 8.
You should carefully look through the [defaults](defaults/main.yml) to see which variables you need to set.
For most parts, the default values of the coturn configuration should already be optimal for use with BigBlueButton.
But pay particular attention to the following:

* You need to set `coturn_static_auth_secret` to the same secret you specified in your BigBlueButton setup.
* `coturn_realm`, althoug optional, should be set (often to you domain name).
* You _really should_ configure **TLS**. When you do so, coturn needs access to your certificate files.
This is why you need to specify their directory and the cert- and key-files.
In practice, this means that you usually set up your tls certificates _before_ running the coturn role.
If you have a special tls user group, you can optionally set it as well.

## Variables

| Required  | Variable Name     | Function  | Default value | Comment  |
| --------- | ----------------- | --------- | ------------- | -------- |
| | `coturn_install_state` | [package state](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/package_module.html#parameter-state) | `present` | set `latest` if you wish to update coturn on every run |
| | `coturn_listening_port` | Default coturn listening port | `3478` | |
| | `coturn_min_port` | Start of the port range for coturn connections | `32769` | If you have manny connections you want to set a lower value eg. `4096` |
| | `coturn_max_port` | End of the port range for coturn connections | `65535` | |
| | `coturn_listening_ip` | The IP coturn will listen to | `{{ ansible_host }}` | |
| | `coturn_user_quota` | The quota how many connections one user can open | `0` | `0` meaning unlimited |
| | `coturn_total_quota` | The total quota for the whole coturn server | `0` | `0` meaning unlimited |
| ⚠️ | `coturn_static_auth_secret` The auth_secret also used in BigBlueButton | `1234` | Generate on eg. `openssl rand -hex 16` |
| | `coturn_realm` | This should be your DNS name otherwise it defaults to the host domain name |  | |
| | `coturn_use_tls` | Tell coturn to use TLS | `true` | |
| | `coturn_tls_listening_port` | The coturn TLS listening port | `443` | any other port makes no sense, since then you don't need turn |
| ⚠️ | `coturn_tls_cert_dir` | The TLS cert directory |  | |
| ⚠️ | `coturn_tls_cert` | The coturn TLS cert file location |  | I do something like this with LetsEncrypt and certbot `/etc/letsencrypt/live/{{ inventory_hostname }}/fullchain.pem` |
| ⚠️ | `coturn_tls_key` | The coturn TLS key file location |  | I do something like this with LetsEncrypt and certbot `/etc/letsencrypt/live/{{ inventory_hostname }}/privkey.pem` |
| | `coturn_denied_peer_ips` | List of denied IP ranges | `Bogon filtering` | |
| | `coturn_verbosity` | The coturn verbosity | `0` | 1 for verbose, 2 for Verbose (very verbose) | |
| | `turn_user` | the coturn user | `{{ 'coturn' if ansible_os_family == 'RedHat' else 'turnserver' }}` | |
