# Using SSH tunneling to access a service in a private AWS vpc

### Scenario

Writing tests on the local computer and run it on a server that is not accessible directly, only through a jumphost.

### Overview

Creating ssh tunnel(s) allows us to reach almost anything in a network where our jumphost has access to.

- Enter the remote server's name with `127.0.0.1` as IP into the local computer's `hosts` file.
- Add the remote server's name to the proxy exception list.
- SSH into the jumphost specifying the appropriate tunnels. Keep this SSH connection alive until you want to access the target.
- Access the target with the remote name.

### Example

#### Example scenario

```text
                              jumphost                                 
                           +-----------+                               
                           |           |                               
                           |           |                               
                           |           |                               
                           |           |                               
    my laptop              |           |              remoteserver     
+------------------+       +-----+--+--+       +----------------------+
|                  |             ^  ^          |                      |
|                  |             |  |          |                      |
|                  |             |  +--------->+ port 8080            |
|        port 8080 +<------------+             |                      |
|                  |                           |                      |
+------------------+                           +----------------------+
 ```

I want to access `remoteserver` on port **8080**. You have  **ssh** access to `jumphost` in the middle which has access to `remoteserver`:8080.

#### 1. Adding `remoteserver` to `hosts` file

Add a line to your `hosts` file:

```text
127.0.0.1 remoteserver
```

The `hosts` file on Linux is **`/etc/hosts`** while on Windows **`%WINDIR%\System32\drivers\etc\hosts`**

#### 2. Add `remoteserver` to proxy exception list

This is application dependent. Browsers have their own settings page where exception can be set. Shell based apps may prefer `no_proxy` variable.

#### 3. SSH into `jumphost`

You have to specify the local port you want to "map" the target server's port, the target server name (which is accessible from the jumphost) and the tartget port. You may have many such tunnels in the same ssh connection.

```bash
ssh -L 8080:remoteserver:8080 {user}@jumphost
```

where:

- Option `-L` creates a "local port forward" (there are other types).
- The next part: `{localport}:{remoteserver}:{remoteport}` tells that if you connect to your local machine's {localport}, it will be "tunneled" to {remoteserver}'s {remoteport}.
- At the end of the command you must specify the jumphost, just like if you would connect to it normally.

Be aware:

- The ssh connection should be available all the time while you want to access `remoteserver`
- Some ssh servers are configured to have a timeout. If there is no activity on the ssh terminal it will disconnect. To avoid this problem, either:
  - Specify `ClientAliveInterval` and `ClientAliveCountMax` options in your ssh config file (`$HOME/.ssh/config`)
  - or in **PuTTY** you have such option in the settings at: "**Connection**" group "**Seconds between keepalives (0 to turn off)**" option.
  - or you can start a program which runs continuously, like `top`
-_ `{localport}` should not be used by any other program on your local machine.
- `{remoteserver}:{remoteport}` should be accessible from the `{jumphost}`

#### 4. Connect to the target server

Now you should be able to connect to the remote target server "seamlessly", as the remote name resolves to `127.0.0.1` which is where target port mapped on the local machine. A command like this should work:

```bash
curl http://remoteserver:8080
```

## Tips

- If you don't want to have a shell for the `{jumphost}`, you may specify the **`-N`** option to the `ssh` command which will not execute any remote command, like start a shell.
- For the ssh connection you may use these options to have a minimal experience: `-f` forks to background, `-q` make it more quiet.
- If you reach your target via a web browser, try to use an extention like **[Proxy SwitchyOmega](https://chamibuddhika.wordpress.com/2012/03/21/ssh-tunnelling-explained/)** which enables you to make profiles where you can fine tune which hosts to use with or without proxy. 
- In the `hosts` file you can add secondary names to hosts like:<br>`127.0.0.1 www.example.com webserver` where you can reach this server via both names: www.example.com and webserver.
- Some protocols allocate ports dynamically so be aware that your request may reach the target server but the response may not come back. In these cases you can use `-D` option and use the jumphost as a SOCKS proxy. This is powerful, it may be used to wicked things.
- If you suspect that your DNS cache is making jokes with you, clean it with `ipconfig /flushdns` command on windows.

## Links

- Theory:
  - <https://chamibuddhika.wordpress.com/2012/03/21/ssh-tunnelling-explained/>
  - <https://zaiste.net/ssh_port_forwarding/>
  - <https://myopswork.com/ssh-tunnel-for-rds-via-bastion-host-6659a48edc>
  - <https://patrickmn.com/aside/how-to-keep-alive-ssh-sessions/>
  - <https://superuser.com/questions/1469999/how-to-create-ssh-tunnel-through-https-or-other-method>
- Reverse Proxying:
  - <https://toic.org/blog/2009/reverse-ssh-port-forwarding/>
- Dynamic proxying:
  - <https://ma.ttias.be/socks-proxy-linux-ssh-bypass-content-filters/>
  - <https://debian-administration.org/article/449/SSH_dynamic_port_forwarding_with_SOCKS>
  - <https://dev.to/__namc/ssh-tunneling---local-remote--dynamic-34fa>
- Browser extension:
  - <https://github.com/FelisCatus/SwitchyOmega/>
