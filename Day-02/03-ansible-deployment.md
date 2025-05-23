# Simple Ansible Deployment

Here’s a step-by-step guide to create a simple Ansible deployment inside GitHub Codespaces, using Docker to simulate a deployment target.

### Goal

We’ll use Ansible to:

- Build a Docker image with a basic web app (like nginx serving an HTML file).
- Use Ansible to deploy it as a container.
- Access the app

### Project Structure

ansible-docker-demo/

├── Dockerfile

├── index.html

├── inventory

├── playbook.yml

└── requirements.yml

### Install Prerequisites:

#### Install Python dependencies
python3 -m pip install --upgrade pip

pip install ansible docker

#### Install Ansible collection
ansible-galaxy collection install -r requirements.yml

### Run the Ansible Playbook
ansible-playbook -i inventory playbook.yml
