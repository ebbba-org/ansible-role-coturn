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
