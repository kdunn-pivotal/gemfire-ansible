# gemfire-ansible

GemFire automation through Ansible

## Requirements

* Pip (installed in setup script if missing)
* Ansible (installed in setup script if missing)

## Optional (Mac Only)

* Homebrew (installed in setup script if missing)

## Local Testing (Mac Only)

This playbook is meant to be self-contained as much as possible. The bootstrap script will install minimal required softwares, e.g. pip, ansible, and brew to the Mac machine.

Download the folowing files and place under gemfire-ansible/files/:
* jdk-8u121-linux-x64.rpm (from Oracle)
* pivotal-gemfire-9.0.2.zip (from Pivotal)

under gemfire-ansible, run the following commands to getup local gemfire cluster (docker):
```
scripts/bootstrap
sudo ansible-galaxy install williamyeh.oracle-java
ansible-playbook setup_gemfire.yml
```

## Verify Local GemFire Docker Containers

The Ansible playbook will create 4 docker containers, gf-locator1, gf-locator2, gf-server1, and gf-server2. To verify the provisioned containers, use the following command to ssh into each container.
```
ssh -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no gemfire@172.18.0.2
ssh -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no gemfire@172.18.0.3
ssh -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no gemfire@172.18.0.4
ssh -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no gemfire@172.18.0.5
```

Use the following command in the terminal first if there is need to access the gemfire containers through docker cli. This step is not needed when accessing the gemfire containers through the ssh:
```
bash -c "clear && DOCKER_HOST=tcp://192.168.99.50:2376 DOCKER_CERT_PATH=~/.docker/machine/machines/gemfire DOCKER_TLS_VERIFY=1 /bin/bash"
```

## Helpful Docker commands

```
docker stop $(docker ps -a -q)
docker rm $(docker ps -a -q)
docker container prune
```

## Uninstall Docker

```
docker-machine rm -f default
brew cask uninstall --force docker-toolbox
brew cask uninstall --force docker
rm -f /usr/local/bin/docker
rm -f /usr/local/bin/docker-compose
rm -f /usr/local/bin/docker-machine
rm -rf ~/.docker
```

### TODO (in no particular order)

- [ ] start GemFire
- [ ] stop GemFire
- [ ] rolling restart
- [ ] config/jar deployment
- [ ] split the playbook (better management)  
- [ ] improve the performance (time need for automation)
- [ ] clean up all the warnings
- [ ] better handle sudo for local connection (localhost)
- [ ] review/enhance variable definitions (default, vars, and etc)
- [ ] clean up/enhance the documentations
- [ ] review and clean up each role
- [ ] others
