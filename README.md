# create-a-Systemd-service-in-Linux


At times you create a script and then you want to have the scripts controlled by systemd or in some cases you wish to have the scripts getting restarted by itself when it is killed due to some reason. In such cases systemd in Linux helps to configure services which can be managed.

Redirect to ```cd /etc/systemd/system```
Create a file named your-service.service and include the following:

```

[Unit]
Description=<description about this service>

[Service]
User=<user e.g. root>
WorkingDirectory=<directory_of_script e.g. /root>
ExecStart=<script which needs to be executed>
Restart=always

[Install]
WantedBy=multi-user.target

=======================================================

[Unit]
Description=MyApp
After=syslog.target network.target
StartLimitIntervalSec=0

[Service]
Type=simple
Restart=always
ExecStart=/usr/bin/npm start
Environment=NODE_ENV=development
StandardOutput=syslog
StandardError=syslog
[Install]
WantedBy=multi-user.target

```

Reload the service files to include the new service.
```
sudo systemctl daemon-reload
```

Start your service
```
sudo systemctl start your-service.service
```

To check the status of your service
```
sudo systemctl status example.service
```
To enable your service on every reboot
```
sudo systemctl enable example.service
```
To disable your service on every reboot
```
sudo systemctl disable example.service
```
