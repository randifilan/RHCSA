# Question
- A server running on non standart port 81 is having to issues to serve is content. Debug and fix the issue
- The web server home directory is */home/www/html*. Do not make any change to this file
- Web service is automaticaly restart on boot
---

Check httpd status 
```
systemctl status httpd --no-pager 
× httpd.service - The Apache HTTP Server
     Loaded: loaded (/usr/lib/systemd/system/httpd.service; enabled; preset: disabled)
    Drop-In: /usr/lib/systemd/system/httpd.service.d
             └─php-fpm.conf
     Active: failed (Result: exit-code) since Sat 2024-03-23 23:15:29 WIB; 10s ago
   Duration: 2d 2h 18min 9.900s
       Docs: man:httpd.service(8)
    Process: 248801 ExecStart=/usr/sbin/httpd $OPTIONS -DFOREGROUND (code=exited, status=1/FAILURE)
   Main PID: 248801 (code=exited, status=1/FAILURE)
     Status: "Reading configuration..."
        CPU: 90ms

Mar 23 23:15:29 servera.lab.randifilan.id systemd[1]: Starting The Apache HTTP Server...
Mar 23 23:15:29 servera.lab.randifilan.id httpd[248801]: (13)Permission denied: AH00072: make_sock: could not bind to address 0.0.0.0:81
Mar 23 23:15:29 servera.lab.randifilan.id httpd[248801]: no listening sockets available, shutting down
Mar 23 23:15:29 servera.lab.randifilan.id httpd[248801]: AH00015: Unable to open logs
Mar 23 23:15:29 servera.lab.randifilan.id systemd[1]: httpd.service: Main process exited, code=exited, status=1/FAILURE
Mar 23 23:15:29 servera.lab.randifilan.id systemd[1]: httpd.service: Failed with result 'exit-code'.
Mar 23 23:15:29 servera.lab.randifilan.id systemd[1]: Failed to start The Apache HTTP Server.
Hint: Some lines were ellipsized, use -l to show in full.
```

Check in sealert
```
sealert -l "*" | grep -i preventing
SELinux is preventing /usr/sbin/httpd from name_bind access on the tcp_socket port 81.
SELinux is preventing /usr/sbin/httpd from name_bind access on the tcp_socket port 81.
```

Check http_port in SELinux
```
semanage port -l | grep http
http_port_t                    tcp      80, 443, 488, 8008, 8009, 8443
```

Add TCP 81 to http_port_t
```
semanage port -a -t http_port_t -p tcp 81
```

Verify Change
```
semanage port -l | grep http
http_port_t                    tcp      80, 443, 488, 8008, 8009, 8443, 81
```

Restart httpd
```
systemctl restart httpd
systemctl enable --now httpd
```

Check httpd status
```
systemctl status httpd
● httpd.service - The Apache HTTP Server
     Loaded: loaded (/usr/lib/systemd/system/httpd.service; enabled; preset: disabled)
    Drop-In: /usr/lib/systemd/system/httpd.service.d
             └─php-fpm.conf
     Active: active (running) since Sat 2024-03-23 23:31:33 WIB; 1min 24s ago
       Docs: man:httpd.service(8)
   Main PID: 7149 (httpd)
     Status: "Total requests: 0; Idle/Busy workers 100/0;Requests/sec: 0; Bytes served/sec:   0 B/sec"
      Tasks: 213 (limit: 23167)
     Memory: 37.1M
        CPU: 805ms
     CGroup: /system.slice/httpd.service
             ├─7149 /usr/sbin/httpd -DFOREGROUND
             ├─7150 /usr/sbin/httpd -DFOREGROUND
             ├─7151 /usr/sbin/httpd -DFOREGROUND
             ├─7152 /usr/sbin/httpd -DFOREGROUND
             └─7153 /usr/sbin/httpd -DFOREGROUND

Mar 23 23:31:23 servera.lab.randifilan.id systemd[1]: Starting The Apache HTTP Server...
Mar 23 23:31:33 servera.lab.randifilan.id systemd[1]: Started The Apache HTTP Server.
Mar 23 23:31:33 servera.lab.randifilan.id httpd[7149]: Server configured, listening on: port 81
```

Check and Modify Firewalld
- Check Firewalld
```
firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: ens3
  sources: 
  services: cockpit dhcpv6-client http ssh
  ports: 82/tcp 2021/tcp 2022/tcp 2023/tcp 2024/tcp 2025/tcp 2026/tcp 2027/tcp 2028/tcp 2029/tcp 2031/tcp 2032/tcp 2033/tcp 2034/tcp 2035/tcp 2036/tcp 2037/tcp 2038/tcp 2039/tcp 8888/tcp 8080/tcp
```

- Add port 81 to Allow in firewall
```
firewall-cmd --add-port=81/tcp --permanent 
success
```

- Verify Firewalld again
```
firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: ens3
  sources: 
  services: cockpit dhcpv6-client http ssh
  ports: 82/tcp 2021/tcp 2022/tcp 2023/tcp 2024/tcp 2025/tcp 2026/tcp 2027/tcp 2028/tcp 2029/tcp 2031/tcp 2032/tcp 2033/tcp 2034/tcp 2035/tcp 2036/tcp 2037/tcp 2038/tcp 2039/tcp 8888/tcp 8080/tcp **81/tcp**
```

```
firewall-cmd --reload
```

Test
```
curl localhost:81
Its works !!!
```
