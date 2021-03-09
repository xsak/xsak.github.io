# Set default address pool in Docker

When you start up a new docker container in a custom docker network, and you notice that your connection is dead and you cannot login to the docker host again via ssh, then it may be because docker created a network that interlaps your computers network range (it happened to me when I was using a vpn, that gave me 172.17.x.x addresses).

To address this set the **default-addess-pools** property in `/etc/docker/daemon.json` file:

```json
{
  "default-address-pools":
  [
    {"base":"10.06.0.0/16","size":24}
  ]
}
```

Restart the docker daemon!
