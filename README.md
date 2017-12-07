Ansible Container for Confluence
================================

[![Travis](https://img.shields.io/travis/alvistack/ansible-container-confluence.svg)](https://travis-ci.org/alvistack/ansible-container-confluence)
[![GitHub release](https://img.shields.io/github/release/alvistack/ansible-container-confluence.svg)](https://github.com/alvistack/ansible-container-confluence/releases)
[![GitHub license](https://img.shields.io/github/license/alvistack/ansible-container-confluence.svg)](https://github.com/alvistack/ansible-container-confluence/blob/master/LICENSE)
[![Ansible Role](https://img.shields.io/badge/galaxy-alvistack.container--confluence-blue.svg)](https://galaxy.ansible.com/alvistack/container-confluence)
[![Docker Pulls](https://img.shields.io/docker/pulls/alvistack/ansible-container-confluence.svg)](https://hub.docker.com/r/alvistack/ansible-container-confluence/)

Ansible Container for Atlassian Confluence.

Confluence is where you create, organize and discuss work with your team.

Learn more about Confluence: <https://www.atlassian.com/software/confluence>

Overview
--------

This Docker container makes it easy to get an instance of Confluence up and running.

### Quick Start

For the `CONFLUENCE_HOME` directory that is used to store the repository data (amongst other things) we recommend mounting a host directory as a [data volume](https://docs.docker.com/engine/tutorials/dockervolumes/#/data-volumes), or via a named volume if using a docker version &gt;= 1.9.

Volume permission is managed by entry scripts. To get started you can use a data volume, or named volumes.

Start Atlassian Confluence Server:

    # Pull latest image
    docker pull alvistack/ansible-container-confluence

    # Run as detach
    docker run \
        -itd \
        --name confluence \
        --publish 8090:8090 \
        --volume /var/atlassian/application-data/confluence:/var/atlassian/application-data/confluence \
        alvistack/ansible-container-confluence

**Success**. Confluence is now available on <http://localhost:8090>

Please ensure your container has the necessary resources allocated to it. We recommend 2GiB of memory allocated to accommodate both the application server and the git processes. See [Supported Platforms](https://confluence.atlassian.com/display/DOC/Supported+platforms) for further information.

### Configuration

We don't provide any dynamic configuration by using environment variable; by the way, since this Docker container is created by using Ansible Container with [Ansible Role for Confluence](https://github.com/alvistack/ansible-role-confluence), you install all required Ansible Roles, retouch the running Docker instance with `ansible-container` and `--extra-vars`, then restart it:

    # Install required Ansible Roles
    ansible-galaxy install -r requirements.yml

    # Configuration
    ansible-playbook --verbose --diff \
        --inventory confluence, \
        --connection docker \
        --extra-vars '{"confluence_scheme":"https","confluence_proxy_name":"example.com","confluence_context_path":"confluence"}' \
        tests/config.yml

    # Restart container
    docker restart confluence

Upgrade
-------

To upgrade to a more recent version of Confluence Server you can simply stop the `confluence` container and start a new one based on a more recent image:

    docker stop confluence
    docker rm confluence
    docker pull alvistack/ansible-container-confluence
    docker run ... (See above)

As your data is stored in the data volume directory on the host it will still be available after the upgrade.

*Note: Please make sure that you **don't** accidentally remove the `confluence` container and its volumes using the `-v` option.*

Backup
------

For evaluations you can use the built-in database that will store its files in the Confluence Server home directory. In that case it is sufficient to create a backup archive of the directory on the host that is used as a volume (`/var/atlassian/application-data/confluence` in the example above).

Versioning
----------

The `latest` tag matches the most recent version of this repository. Thus using `alvistack/ansible-container-confluence:latest` or `alvistack/ansible-container-confluence` will ensure you are running the most up to date version of this image.

Dependencies
------------

[requirements.yml](requirements.yml)

License
-------

-   Code released under [Apache License 2.0](LICENSE)
-   Docs released under [CC BY 4.0](http://creativecommons.org/licenses/by/4.0/)

Author Information
------------------

-   Wong Hoi Sing Edison
    -   <https://twitter.com/hswong3i>
    -   <https://github.com/hswong3i>

