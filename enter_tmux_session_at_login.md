# Enter tmux session at login

If you add this snippet to `~/.bashrc`, on the next login to the shell it checks if there is a **tmux** session named *"rekettye"*. If it exists, it logs into it, when it doesn't then it creates it.

```bash
tmux has -t rekettye &> /dev/null

if [ $? != 0 ]; then
  tmux -2 new-session -s rekettye -D -d
fi

if [ -z "$TMUX" ]; then
  tmux attach -t rekettye -d
fi
```
