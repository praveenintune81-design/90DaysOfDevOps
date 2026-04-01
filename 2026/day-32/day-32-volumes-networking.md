# Day 32 – Docker Volumes & Networking

##  Objective

To understand data persistence and container communication.

##  Task 1 – Data Loss

* Created MySQL container
* Inserted data into database
* Removed container and recreated it

 Data was lost
**Reason:** Containers are ephemeral


## 💾 Task 2 – Named Volume

* Created volume using `docker volume create`
* Attached volume to MySQL container

 Data persisted after container removal

## Task 3 – Bind Mount

* Mounted local folder to Nginx container
* Changes reflected instantly in browser

**Difference:**

* Volume: Docker managed storage
* Bind Mount: Host-based storage


##  Task 4 – Default Network

* Containers could not communicate using names
* Communication worked using IP addresses


##  Task 5 – Custom Network

* Created custom network
* Containers communicated using names

**Reason:** Built-in DNS resolution


##  Task 6 – Full Setup

* Created custom network + volume
* Ran MySQL and app container
* App successfully connected to MySQL using container name


##  Conclusion

* Volumes solve data persistence
* Bind mounts help in development
* Custom networks enable container communication
