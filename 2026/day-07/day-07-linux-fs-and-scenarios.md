hi-Praveen Dwivedi 
Scenario 1: Service Not Starting 
Que:- A web application service called 'myapp' failed to start after a server reboot.
What commands would you run to diagnose the issue?
Write at least 4 commands in order.
Ans:- 
# systemctl status myapp  (check myapp service running or not)
# journalctl -u myapp -n 50  (exact error show like config/port/permission issue)
# systemctl is-enabled myapp  ( parmanent on service when restart or boot the server service running)
# systemctl daemon-reload   (when any change to config file so restart  then apply new changess)
# systemctl restart myapp

Scenario 2: High CPU Usage
Que:- Your manager reports that the application server is slow.
You SSH into the server. What commands would you run to identify
which process is using high CPU.?

Ans:-
# top (showing currect cpu usage)
# ps aux --sort=-%cpu | head -10  (show which application or server usage high cpu)

Scenario 3️: Docker Service Logs
Que:-A developer asks: "Where are the logs for the 'docker' service?"
The service is managed by systemd.
What commands would you use?

Ans:- 
# systemctl status docker  (showing server status runing/stop/failed)
# journalctl -u docker -n 50 (last 50 log showing error/faield)
# journalctl -u docker -f  (shoing live log)

Scenario 4: File Permissions Issue
Que:- A script at /home/user/backup.sh is not executing.
When you run it: ./backup.sh
You get: "Permission denied"
What commands would you use to fix this?

Ans:-
# ls -l /home/user/backup.sh (check current permission)
# chmod +x /home/user/backup.sh (given exucute permission)
# ./backup.sh  (run the script file)







