# Docker Compose Deploy Role

This Ansible role deploys Docker Compose stacks. It ensures that the necessary directories exist, copies the required Docker Compose files and source directories, and then deploys the Docker Compose stacks.

## Requirements

- Ansible 2.9 or later
- Docker and Docker Compose installed on the target hosts
- Ansible `community.docker` collection installed

## Role Variables

The role uses a list of dictionaries defined in the `docker_compose_stacks` variable. Each dictionary can have the following keys:

- `name`: (required) The name of the Docker Compose stack.
- `src`: (optional) The source directory containing the Docker Compose files. Defaults to `playbook_dir + '/stacks/' + item.name`.
- `dest`: (optional) The destination directory for the Docker Compose stack on the target host. Defaults to `ansible_env.HOME + '/' + item.name`.

Example:
```yaml
docker_compose_stacks:
  - name: my_app
    src: /path/to/my_app
    dest: /var/docker/my_app
  - name: another_app
    # src and dest are optional
```

## Dependencies

None

## Example Playbook

```yaml
- hosts: all
  roles:
    - role: docker-compose-deploy
      vars:
        docker_compose_stacks:
          - name: my_app
            src: /path/to/my_app
            dest: /var/docker/my_app
          - name: another_app
```

## Tasks

The role performs the following tasks:

1. **Ensure directory for Docker Compose stacks exists**: Creates the destination directory for each stack with the appropriate permissions.
2. **Copy Docker Compose files**: Copies the Docker Compose YAML template file to the destination directory.
3. **Copy source directory**: Copies the entire source directory to the destination directory.
4. **Deploy Docker Compose stack**: Uses the `community.docker.docker_compose` module to deploy the Docker Compose stack.
