A bunch of handy windows commands.

## Http-Requests
Requesting an endpoint and getting a string as result.
```powershell
(New-Object System.Net.WebClient).DownloadString(url)
```

## Moving files with ssh
Windows provides a handy command for doing exactly that.

```powershell
scp path_on_client user@address:~/path_on_server

# IPv6
scp -6 path_on_client user@[address]:~/path_on_server
```
