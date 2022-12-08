# Final Exam
##### Part 1: How to update ubuntu
to check for updates use: `sudo apt update`
to upgrade the packages use: `sudo apt upgrade`
![update and upgrade](/Images/update.png)

##### Part 2: Fix codeblock
1. to replace all the `eco` to `echo` use: type `:%s/eco/echo/g` then press enter
2. to replace `V` to be `C` and also the `-eq 1` to `-eq 0`: move cursor to letter you want to replace press `r` then type the replacement
3. to replace the `nums` to `:digits:`: move cursor to the word you want to replace press `cw` then type the replacement
![codeblock](/Images/editedcode.png)

##### Part 3: journalctl
1. searching journalctl with `man journalctl`  
![manjournalctl](/Images/manjournalctl.png)
2. Using `/logs` to search for logs, I found -b is the appropriate command to use for printing logs for the current boot
![journalctlb](/Images/journalctlb.png)
3. Using `/priority` to search for priority, I found -p is the appropriate command to use for printing logs with priority
![journalctlp](/Images/journalctlp.png)
4. Using `/json` to search for outputting in pretty json format, I found -o is the appropriate command with the option `json-pretty` to use for printing logs in pretty json format
![journalctlo](/Images/journalctlo.png)
5. The final command will be: `journalctl -b -p 6 -o json-pretty`  
![finaljournalctl](/Images/finaljournalctl.png)

##### Part 4: finding users
1. create new user 
`sudo useradd -m -s /bin/bash jack`
2. set password with 
`passwd jack`
3. create a script file in `/etc` with vim 
`sudo vim /opt/allusers/allusers`
4. add the following code to the file
```
#!/bin/bash

grep -E *:[1-5][0-9][0-9][0-9]: /etc/passwd > /etc/motd

echo "Users currently logged in are:" >> /etc/motd

who >> /etc/motd
```
5. Modify the permissions of the file with 
`sudo chmod 744 /etc/allusers` 
as well as the motd file with 
`sudo chmod 744 /etc/motd`

##### Part 5: unit files
1. Create a new unit file with 
`sudo vim /etc/systemd/system/allusers.service`
2. Put in the following
```
[Unit]
Description="Prints all users and their shells as well as th currently logged in users"

[Service]
Type="oneshot"
ExecStart="/opt/allusers/allusers"

[Install]
WantedBy=multi-user.target
```
3. Daemon-reload, start, and enable the service with  
`sudo systemctl daemon-reload`  
`sudo systemctl start allusers.service`  
`sudo systemctl enable allusers.service`  
4. Check the status of the service with 
`systemctl status allusers.service`  
![savestartenable](/Images/savestartenable.png)
This will only run once due to oneshot type. Service was run and exited successfully. 
![allusersstatus](/Images/allusersstatus.png)
Motd file was created and populated with the correct information.
![catmotd](/Images/catmotd.png)

##### Part 6: create a timer
1. Create a new timer file with `sudo vim /etc/systemd/system/allusers.timer`
2. Put in the following
```
[Unit]
Description="A timer to start allusers.service"

[Timer]
OnBootSec=60
Persistent=true

[Install]
WantedBy=timers.target
```
3. Daemon-reload, start, and enable the timer with  
`sudo systemctl daemon-reload`  
`sudo systemctl start allusers.timer`  
`sudo systemctl enable allusers.timer`  
4. Check the status of the timer with
`systemctl status allusers.timer`  
![timerstatus](/Images/alluserstimer.png)
