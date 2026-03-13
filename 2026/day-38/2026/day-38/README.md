# Day 38 – YAML Basics

## YAML Files

### person.yml
#################################################################
name: Praveen
role: DevOps Engineer
experience_years: 2
learning: true

tools:
  - git
  - docker
  - kubernetes
  - github-actions
  - aws

hobbies: [learning, cooking, reading, watching movie]

#################################################################
### server.yml
#################################################################
server:
  name: web-server
  ip: 192.168.1.10
  port: 80

database:
  host: localhost
  name: appdb
  credentials:
    user: admin
    password: secret123

startup_script_pipe: |
  echo "Starting server"
  sudo systemctl start nginx
  echo "Server started"

startup_script_folded: >
  echo "Starting server"
  sudo systemctl start nginx
  echo "Server started"
##################################################################

## What I Learned

1. YAML uses spaces for indentation and does not allow tabs.
2. Lists in YAML can be written using block style or inline style.
3. YAML supports nested objects and multi-line strings.
