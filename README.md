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

You may have wondered what the After= directive did. It simply means that your service must be started after the network is ready.

By default, systemd does not restart your service if the program exits for whatever reason. This is usually not what you want for a service that must be always available, so we’re instructing it to always restart on exit:
``
Restart=always
```
You could also use on-failure to only restart if the exit status is not 0.

By default, systemd attempts a restart after 100ms. You can specify the number of seconds to wait before attempting a restart, using:
```
RestartSec=1
```
By default, when you configure Restart=always as we did, systemd gives up restarting your service if it fails to start more than 5 times within a 10 seconds interval. Forever.
There are two [Unit] configuration options responsible for this:
```
StartLimitBurst=5
StartLimitIntervalSec=10
```
The RestartSec directive also has an impact on the outcome: if you set it to restart after 3 seconds, then you can never reach 5 failed retries within 10 seconds.
