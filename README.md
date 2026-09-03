# SJSU Ansible Web Server Deployment

Configure two virtual machines (VM1 and VM2) and use Ansible to deploy a web
server on each, listening on **port 8080**, serving a page that displays:

- **VM1:** `Hello World from SJSU-1`
- **VM2:** `Hello World from SJSU-2`

## Repository layout

```
.
├── Vagrantfile                     # Defines VM1 and VM2 (Ubuntu 24.04, VirtualBox)
└── ansible
    ├── ansible.cfg                 # Ansible configuration
    ├── inventory.ini               # Hosts VM1 / VM2 + per-host web message
    ├── webserver.yml               # Deploy and undeploy plays
    └── templates
        ├── index.html.j2           # Web page template
        └── sjsu-web.conf.j2        # Nginx site config (listens on 8080)
```

## Prerequisites

- [VirtualBox](https://www.virtualbox.org/)
- [Vagrant](https://www.vagrantup.com/)
- [Ansible](https://docs.ansible.com/)

## Bring up the VMs

```bash
vagrant up
```

This creates two VMs:

| VM  | Private IP       | Host port (forwarded) |
|-----|------------------|-----------------------|
| VM1 | 192.168.56.11    | http://127.0.0.1:8081 |
| VM2 | 192.168.56.12    | http://127.0.0.1:8082 |

Each VM serves the web page on guest port **8080**.

## Deploy the web servers

```bash
cd ansible
ansible-playbook webserver.yml --tags deploy
```

Then open the pages in a browser:

- VM1: http://127.0.0.1:8081  → `Hello World from SJSU-1`
- VM2: http://127.0.0.1:8082  → `Hello World from SJSU-2`

Or directly against the private IPs on port 8080:

- http://192.168.56.11:8080
- http://192.168.56.12:8080

## Un-deploy the web servers

```bash
cd ansible
ansible-playbook webserver.yml --tags undeploy
```

This stops and removes Nginx and deletes the deployed configuration and web page.

## How it works

The playbook (`ansible/webserver.yml`) contains two plays:

- **deploy** — installs Nginx, renders the web page from `index.html.j2`,
  configures Nginx to listen on port 8080 via `sjsu-web.conf.j2`, enables the
  site, and starts the service.
- **undeploy** — stops the service and removes all deployed resources.

The per-VM message is driven by the `web_message` variable defined per host in
`inventory.ini`, so the same template produces `SJSU-1` on VM1 and `SJSU-2` on
VM2.
