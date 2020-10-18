# Get the latest version (tag) number from a Github repository

For example the `docker-compose` repository:

```shell
curl -s https://api.github.com/repos/docker/compose/releases/latest | awk -F\: '/tag_name/ { print gensub(/.*"([^"]+)".*/,"\\1","g",$2) }'
```
