# Start ssh-agent

Add it to `~/.bashrc` because I _always_ forget it...

```bash
function ssha() {
        echo Starting ssh agent...
        eval `ssh-agent -s`
        ssh-add ~/.ssh/id_rsa
        trap 'test -n "$SSH_AGENT_PID" && eval `/usr/bin/ssh-agent -k`' 0
}
```
