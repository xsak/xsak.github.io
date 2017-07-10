If defining http_proxy in `/etc/default/docker` or in shell does not work:

```shell
$ mkdir /etc/systemd/system/docker.service.d/
$ cat >/etc/systemd/system/docker.service.d/proxy.conf << EOF
[Service]
Environment=HTTP_PROXY=http://myproxy:3128/
EOF
$ systemctl daemon-reload
$ systemctl restart docker.service
```
