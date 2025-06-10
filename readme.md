## Configure SSL-VPN (Fortigate)
### Configure VPN Client on linux with openfortiVPN
install openfortivpn
```base
sudo apt update
sudo apt install openfortivpn
```

Test connect 
```base
sudo openfortivpn {host}:{port} -u {user} -p {password}
```

Or enter the password later.
```base
sudo openfortivpn {host}:{port} -u {user}
```
> [!NOTE]
Replace `{host}` with the actual hostname or IP address.<br>
Replace `{port}` with the SSL-VPN port (usually 443).<br>
Replace `{user}` `{password}` with your VPN username and password.

If the connection error is due to trusted-cert.
find `ERROR:      --trusted-cert ##########################`
```base
sudo openfortivpn {host}:{port} -u {user} --trusted-cert {cert code} -v
```
> [!NOTE]
Replace `{cert code}` with trusted-cert code or certificate.

### Configure OpenfortiVPN with ini
Go to config openfortivpn at path `/etc/openfortivpn/config`.
```base
sudo nano /etc/openfortivpn/config.ini
```
Edit file `ini`.
```base
host = {host}
port = {Port}
realm = pub-all
username = {user}
password = {password}
trusted-cert = {cert code}
```

Test running openfortivpn via config.
```base
sudo openfortivpn -c /etc/openfortivpn/config -v
```

Provide openfortivpn autostart service.
```base
sudo nano /etc/systemd/system/openfortivpn.service
```

Edit file `Service`.
```base
[Service]
ExecStart=/usr/bin/openfortivpn -c /etc/openfortivpn/config
Restart=always
RestartSec=5
CapabilityBoundingSet=CAP_NET_ADMIN CAP_NET_BIND_SERVICE
```

Enable and start servive.
```base
sudo systemctl enable openfortivpn.service
sudo systemctl start openfortivpn.service
```
Check status service.
```base
sudo systemctl status openfortivpn.service
```

