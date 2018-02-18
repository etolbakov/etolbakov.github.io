---
layout: post
feature-img: "assets/img/sample_feature_img.png"
title: Unit toxiproxy.service not found
tags: [ubuntu, linux, proxy, toxiproxy]
---
Recently for some tests I had to slow down my mysql docker image. For that I used [Toxiproxy](https://github.com/Shopify/toxiproxy) tool.
It should be easy to [set up](https://github.com/Shopify/toxiproxy#1-installing-toxiproxy) it on Ubuntu, but the final step `sudo service toxiproxy start` failed for me:

```
Failed to restart toxiproxy.service: Unit toxiproxy.service not found.
```
To make things work for me I had to
* create `toxiproxy.service` file
* copy it to `/etc/systemd/system/` and reload config
* start the `toxiproxy.service` 

The `toxiproxy.service` file has the following content: 
```
[Unit]
Description=ToxiProxy Server
Wants=network-online.target
After=network-online.target

[Service]
Type=simple
WorkingDirectory=/tmp/
ExecStart=/usr/bin/toxiproxy-server -port 8474 -host 0.0.0.0
Restart=on-failure
RestartSec=10
StartLimitInterval=0
PrivateTmp=true
PrivateDevices=true
[Install]
WantedBy=multi-user.target
```
Then I copied it to `system` folder with 
```
$ sudo cp toxiproxy.service /etc/systemd/system/
```
and reloaded config with 
```
$ sudo systemctl daemon-reload
```
When it's done `toxiproxy.service` is ready to start, but let's check it's status before
```
$ sudo systemctl status toxiproxy.service
● toxiproxy.service - ToxiProxy Server
   Loaded: loaded (/etc/systemd/system/toxiproxy.service; disabled; vendor preset: enabled)
   Active: inactive (dead)
```
Good! Let's start it: 
```
$ sudo systemctl start toxiproxy.service
 ```
and check the status again to make sure our service is running:
```
$ sudo systemctl status toxiproxy.service
```
output:

```
● toxiproxy.service - ToxiProxy Server
   Loaded: loaded (/etc/systemd/system/toxiproxy.service; disabled; vendor preset: enabled)
   Active: active (running) since Fri 2018-02-16 10:47:36 GMT; 1s ago
 Main PID: 8298 (toxiproxy-serve)
    Tasks: 6
   Memory: 904.0K
      CPU: 21ms
   CGroup: /system.slice/toxiproxy.service
           └─8298 /usr/bin/toxiproxy-server -port 8474 -host 0.0.0.0

Feb 16 10:47:36 u16 systemd[1]: Started ToxiProxy Server.
Feb 16 10:47:36 u16 toxiproxy-server[8298]: time="2018-02-16T10:47:36Z" level="info" msg="API HTTP server starting" host="0.0.0.0" port="8474" version="2.1.
```

So service is working !