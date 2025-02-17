Ansible Provisioning for DevOps/SRE Test
Overview
This repository contains an Ansible playbook (main.yaml) to automate system provisioning, user management, Docker installation, and Nginx reverse proxy setup. It also includes an Nginx configuration template (nginx.conf.j2) to forward traffic to a Docker container.

Features
🔹 System Configuration
Sets open file limit for root to 65536.
Ensures required user groups exist (developers, it, infra, docker).
🔹 User Management
Creates user accounts from YAML data.
Assigns users to specified groups.
Configures SSH keys for authentication.
Generates /home/USER/info files containing user details.
🔹 Docker Installation & Setup
Installs Docker CE using the latest official repository method.
Installs:
docker-ce
docker-ce-cli
containerd.io
docker-buildx-plugin
docker-compose-plugin
Ensures Docker service is enabled and running.
Pulls the Ubuntu image for testing.
Deploys default Docker containers.
🔹 File Management
Downloads prrtprrt.txt and:
Copies it to all user home directories.
Saves a local copy under $PLAYBOOK_DIR/files/prrtprrt.txt.
🔹 Nginx Configuration
Installs Nginx.
Configures it as a reverse proxy to route traffic to the happy_roentgen container using nginx.conf.j2.
Restarts Nginx to apply changes.
Installation & Usage
Prerequisites
Ansible installed (ansible --version to check).
Target machines running Ubuntu with SSH access.
A properly configured inventory file.
 Running the Playbook
Clone the repository:

git clone https://github.com/elabed-dhahbi/besedoAnsible.git
cd besedoAnsible
Ensure your inventory file (hosts) is set up with target machines.
Run the playbook:

ansible-playbook -i hosts main.yaml
Verifying the Setup
Check if users were created:

cat /home/jane.doe/info
Verify Docker installation:

docker ps
Test Nginx reverse proxy:

curl http://localhost
File Structure

.
├── main.yaml             # Ansible playbook
├── templates/
│   ├── nginx.conf.j2     # Nginx configuration template
├── README.md             # Documentation
└── files/                # Local storage for files (prrtprrt.txt)
Customization
Modify user accounts and SSH keys in main.yaml.
Update nginx.conf.j2 to change Nginx proxy settings.
Adjust container count and images in main.yaml.
Troubleshooting
1. APT Lock Issue

E: Could not get lock /var/lib/dpkg/lock-frontend
Fix: The playbook waits for APT locks, but if an issue persists, manually check:


sudo lsof /var/lib/dpkg/lock
Kill the process and retry.

 2. Docker Service Not Starting

A dependency job for docker.service failed
Fix: Ensure the docker group exists and restart:


sudo systemctl restart docker
3. Nginx Not Proxying Requests
Check Nginx configuration:

nginx -t
Restart the service:


sudo systemctl restart nginx

