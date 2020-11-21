# ansible-minidlna

Installs a `minidlna` service as docker container managed by systemd.

# System requirements

* Docker
* Systemd

## Role requirements

* (none, so far)

## Role parameters

### Main config

| Variable      | Type | Mandatory? | Default | Description           |
|---------------|------|------------|---------|-----------------------|
| minidlna_alpine_version | version number | no | `latest` | Used alpine linux version |
| minidlna_version        | version number | no | `latest` | Installed minidlna version |
| minidlna_docker_working_directory | absolute path | no | `/opt/minidlna/docker` | Used docker working directory |
| minidlna_config_volume_directory  | absolute path | no | `/srv/minidlna/config` | Directory where your config file will be stored |
| minidlna_database_volume_directory | absolute path | no | `/srv/minidlna/cache` | Directory where the cache database will be stored |
| minidlna_log_volume_directory      | absolute path | no | `/srv/my_minidlna/log` | Directory where the logs will be stored |
| minidlna_media_directories         | array of `media_directory`s | no | `[]`       | Your configured media directories |
| minidlna_friendly_name             | text                        | no | `''`       | The server name |
| minidlna_serial                    | text                        | no | `12345678` | minidlna's serial |
| minidlna_model_number              | number                      | no | `1`        | minidlna's model number |

### `media_directory` definition

| Property      | Type | Mandatory? | Default | Description           |
|---------------|------|------------|---------|-----------------------|
| name          | text | yes        |         | The name of the media directory. The path inside the docker container: `/opt/<name>`
| path          | absolute path | yes |       | The path of the media directory outside the docker container. |
| type          | text          | yes |       | The media directory type, `A`, `V` or `P` |

## Example Playbook

### Add to `requirements.yml`:

```yaml
- name: install-minidlna
  src: https://github.com/borisskert/ansible_minidlna.git
  scm: git
```

### Example `playbook.yml`:

```yaml
- hosts: servers
  roles:
    - role: install-minidlna
      minidlna_alpine_version: '3.12'
      minidlna_version: '1.2.1'
      minidlna_docker_working_directory: /opt/my_minidlna/docker
      minidlna_config_volume_directory: /srv/my_minidlna/config
      minidlna_media_directories:
        - name: Music
          path: /srv/my_minidlna/media/music
          type: A
        - name: Videos
          path: /srv/my_minidlna/media/videos
          type: V
        - name: Pictures
          path: /srv/my_minidlna/media/pictures
          type: P
      minidlna_database_volume_directory: /srv/my_minidlna/cache
      minidlna_log_volume_directory: /srv/my_minidlna/log
      minidlna_friendly_name: My DLNA Server
      minidlna_serial: '75296493'
      minidlna_model_number: 3
```

## Testing

Requirements:

* [Vagrant](https://www.vagrantup.com/)
* [Qemu](https://www.qemu.org/libvirt) and [libvirt](https://libvirt.org/)
* [Ansible](https://docs.ansible.com/)
* [Molecule](https://molecule.readthedocs.io/en/latest/index.html)
* [yamllint](https://yamllint.readthedocs.io/en/stable/#)
* [ansible-lint](https://docs.ansible.com/ansible-lint/)
* [Docker](https://docs.docker.com/)

### Run within docker

```shell script
molecule test -s docker-default && molecule test -s docker-all-parameters
```

### Run within Vagrant

```shell script
molecule test -s vagrant-default && molecule test -s vagrant-all-parameters
```

I recommend to use [pyenv](https://github.com/pyenv/pyenv) for local testing.
Within the Github Actions pipeline I use [my own molecule Docker image](https://github.com/borisskert/docker-molecule).

## License

MIT

## Author Information

* [borisskert](https://github.com/borisskert)
