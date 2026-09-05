# SJSU Ansible Web Server Deployment

Two VMs (VM1 and VM2) provisioned with Vagrant, with Ansible deploying an Nginx
web server on each. Each server listens on port 8080 and serves:

- VM1: `Hello World from SJSU-1`
- VM2: `Hello World from SJSU-2`

## Requirements

- VirtualBox
- Vagrant
- Ansible

## Usage

Start the VMs:

```bash
vagrant up
```

Deploy the web servers:

```bash
cd ansible
ansible-playbook webserver.yml --tags deploy
```

Pages are then reachable at:

- VM1: http://127.0.0.1:8081
- VM2: http://127.0.0.1:8082

Un-deploy the web servers:

```bash
ansible-playbook webserver.yml --tags undeploy
```

## Files

- `Vagrantfile` — defines VM1 and VM2
- `ansible/webserver.yml` — deploy and undeploy plays
- `ansible/inventory.ini` — hosts and per-VM message
- `ansible/templates/` — web page and Nginx config templates
