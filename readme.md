## Configure SSL-VPN (Fortigate)
### Configure VPN Client on linux with openfortiVPN
install openfortivpn
```sh
sudo apt update
sudo apt install openfortivpn
```

Test connect 
```sh
sudo openfortivpn {host}:{port} -u {user} -p {password}
```

Or enter the password later.
```sh
sudo openfortivpn {host}:{port} -u {user}
```
> [!NOTE]
Replace `{host}` with the actual hostname or IP address.<br>
Replace `{port}` with the SSL-VPN port (usually 443).<br>
Replace `{user}` `{password}` with your VPN username and password.

If the connection error is due to trusted-cert.
find `ERROR:      --trusted-cert ##########################`
```sh
sudo openfortivpn {host}:{port} -u {user} --trusted-cert {cert code} -v
```
> [!NOTE]
Replace `{cert code}` with trusted-cert code or certificate.

### Configure OpenfortiVPN with ini
Go to config openfortivpn at path `/etc/openfortivpn/config`.
```sh
sudo nano /etc/openfortivpn/config.ini
```
Edit file `ini`.
```sh
host = {host}
port = {Port}
realm = pub-all
username = {user}
password = {password}
trusted-cert = {cert code}
```

Test running openfortivpn via config.
```sh
sudo openfortivpn -c /etc/openfortivpn/config -v
```

Provide openfortivpn autostart service.
```sh
sudo nano /etc/systemd/system/openfortivpn.service
```

Edit file `Service`.
```sh
[Service]
ExecStart=/usr/bin/openfortivpn -c /etc/openfortivpn/config
Restart=always
RestartSec=5
CapabilityBoundingSet=CAP_NET_ADMIN CAP_NET_BIND_SERVICE
```

Enable and start servive.
```sh
sudo systemctl enable openfortivpn.service
sudo systemctl start openfortivpn.service
```
Check status service.
```sh
sudo systemctl status openfortivpn.service
```

