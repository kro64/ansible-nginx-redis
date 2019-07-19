# ansible-nginx-redis
This repository stores ansible playbook for deploying Nginx + Syslog-ng + Redis system, which captures User-Agent and stores it's request count into Redis database.


# Requirements

To be able to launch this playbook successfully, ansible remote_user must be able to connect to the host without any prompts.

- SSH fingerprint of the remote host must be already trusted

- Connecting to the host via SSH with ansible remote_user, must not require a password. (Installed ssh key.)

- Ansible remote_user on the remote host, must have passwordless become option.

Example:
*sudo echo "ansible ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/ansible*


# Installation

This playbook was created using Ansible 2.8.1.

To launch the playbook, from the root directory, launch like this:

**ansible-playbook -i hosts playbooks/launch_hw.yml**

# Example usage

This is an example of usage using curl. Let's say we want to manipulate our user-agent header, providing a custom value to the server.


Example query:

**curl -H "User-Agent: This is my agent." 127.0.0.1**



Example agent count check:

Check listed keys:

[karbir@ nicepc]# _redis-cli_
127.0.0.1:6379> _KEYS *_
1) "Mozilla/5.0 (X11; Linux x86_64; rv:60.0) Gecko/20100101 Firefox/60.0"
2) "This is my agent."
3) "hasta"

Check user-agent key count on Redis:

127.0.0.1:6379> _GET "This is my agent."_
"1"


*Tip: You can also set values into Redis by visiting the configured nginx webserver.

In this case:

http://127.0.0.1 
