# SSH escape sequences

Normal keys are forwarded over the ssh session, so none of those will work. Instead, use the escape sequences. To kill the current session hit subsequently Enter `↵`, `~`, `.`.

(Have in mind that in international keyboards were ~ is set to be a composing character you have to hit it twice: Enter `↵`, `~`, `~`, `.`

More of these escape sequences can be listed with Enter `↵`, `~`, `?`:

```
Supported escape sequences:
     ~.   - terminate connection (and any multiplexed sessions)
     ~B   - send a BREAK to the remote system
     ~C   - open a command line
     ~R   - request rekey
     ~V/v - decrease/increase verbosity (LogLevel)
     ~^Z  - suspend ssh
     ~#   - list forwarded connections
     ~&   - background ssh (when waiting for connections to terminate)
     ~?   - this message
     ~~   - send the escape character by typing it twice
```

(Note that escapes are only recognized immediately after newline.)
You can close the list of Escape sequences by hitting `enter`.

Notice that because hitting `~~` causes ssh to send the *~* instead of intercepting it, you can address N nested ssh connections by hitting `~` N times. (This only applies to `~`s that directly follow an `enter`.) That is to say that `enter`&nbsp;`~`&nbsp;`~`&nbsp;`~`&nbsp;`~`&nbsp;`~`. terminates an ssh session 5 layers deep and keeps the other 4 intact.

source: <https://askubuntu.com/questions/29942/how-can-i-break-out-of-ssh-when-it-locks>
