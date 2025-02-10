# ansible-coroot-node-agent

Ansible role to install and manage the coroot node agent.

## Description

This role installs, configures, and manages the coroot node agent. It will:

- Check if the agent is already installed and determine its version.
- Download the agent binary from GitHub if needed.
- Create required systemd service and environment files from templates.
- Start and (if necessary) restart the coroot node agent.
- Optionally uninstall the agent by stopping its service and removing deployed files.

## Role Variables

The following variables can be overridden in your playbook or inventory (see [defaults/main.yml](ansible-coroot-node-agent/defaults/main.yml)):

- **coroot_node_agent_download_url**  
  URL to download the agent binary.

- **coroot_node_agent_version**  
  Target version of the coroot node agent.

- **coroot_node_agent_arch**  
  Architecture of the binary (e.g., `amd64`).

- **coroot_node_agent_sha256sum**  
  SHA256 checksum for validating the binary.

- **coroot_node_agent_bin_dir**  
  Directory where the binary will be installed (default: `/usr/bin`).

- **coroot_node_agent_system_name**  
  Service name (default: `coroot-node-agent`).

- **coroot_node_agent_bin_dest**  
  Full path for the installed binary.

- **coroot_node_agent_systemd_dir**  
  Directory for installed systemd files (differentiates by `ansible_os_family`).

- **coroot_node_agent_systemd_service**  
  Service file name (e.g., `coroot-node-agent.service`).

- **coroot_node_agent_file_service**  
  Full path to the systemd service file.

- **coroot_node_agent_file_env**  
  Full path to the default environment variables file.

- **coroot_node_agent_env_vars**  
  A mapping of environment variables to be inserted in the env file.
  Varialbes could be found in official [DOCUMENTATION](https://docs.coroot.com/configuration/configuration)

## Templates

- [systemd_service.j2](ansible-coroot-node-agent/systemd_service.j2)  
  Template for the systemd unit file.

- [default.j2](ansible-coroot-node-agent/default.j2)  
  Template for the default environment file.

## Tasks

The role is organized into several task files:

- [uninstall.yml](ansible-coroot-node-agent/tasks/uninstall.yml)  
  Contains tasks to uninstall the agent.

- [main.yml](ansible-coroot-node-agent/tasks/main.yml)
  Import and conditionally execute installation or uninstallation tasks.

## Usage Example

Include the role in your playbook as follows:

```yaml
- hosts: all
  become: true
  roles:
    - role: ansible-coroot-node-agent
      vars:
        coroot_node_agent_version: "1.23.8"
        coroot_node_agent_arch: "amd64"
        # Optionally override other variables as needed.
```

To uninstall the node agent, set the variable coroot_node_agent_uninstall to true:
```yaml
- hosts: all
  become: true
  roles:
    - role: ansible-coroot-node-agent
      vars:
        coroot_node_agent_uninstall: true
```
