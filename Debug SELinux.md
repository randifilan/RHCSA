# Question
- A server running on non standart port 82 is having to issues to serve is content. Debug and fix the issue
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
Mar 23 23:15:29 servera.lab.randifilan.id httpd[248801]: (13)Permission denied: AH00072: make_sock: could not bind to address 0.0.0.0:82
Mar 23 23:15:29 servera.lab.randifilan.id httpd[248801]: no listening sockets available, shutting down
Mar 23 23:15:29 servera.lab.randifilan.id httpd[248801]: AH00015: Unable to open logs
Mar 23 23:15:29 servera.lab.randifilan.id systemd[1]: httpd.service: Main process exited, code=exited, status=1/FAILURE
Mar 23 23:15:29 servera.lab.randifilan.id systemd[1]: httpd.service: Failed with result 'exit-code'.
Mar 23 23:15:29 servera.lab.randifilan.id systemd[1]: Failed to start The Apache HTTP Server.
Hint: Some lines were ellipsized, use -l to show in full.
```

Check http_port in SELinux
```
semanage port -l | grep http
http_port_t                    tcp      80, 443, 488, 8008, 8009, 8443
```

Add TCP 82 to http_port_t
```
semanage port -a -t http_port_t -p tcp 82
```

Verify Change
```
semanage port -l | grep http
http_port_t                    tcp      80, 443, 488, 8008, 8009, 8443, 82
```
