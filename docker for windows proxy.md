

Edit file `%userprofile%\.docker\machine\machines\default\config.json`, and enter proxy address and port. Create the file if it is not existing.


```json
{
"iptables": false,
    "HostOptions": {
        "EngineOptions": {
            "Env": [
                "HTTP_PROXY=http://10.1.1.1:3128", 
                "HTTPS_PROXY=http://10.1.1.1:3128", 
                "NO_PROXY=127.0.0.1"
            ]
        } 
    }
}
```
