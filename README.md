genestack-airgap-tools
=========

This ansible role is primarily aimed for installed and configuring the tools required to support an airgapped install of genestack. The tools which are used for this purpose include:
- debmirror: debmirror is used to mirror the apt repositories required for airgapped install
- apache2: required for hosting the local ubuntu mirror and for hosting the local pypi package index for pip and other required artifacts for kubespray; these are hosted behind virtual hosts in apache2 which are secured with self-signed certs
- docker: required for running containers for pulp (for local pypi package index) and harbor (offline container image registry and helm chart repository)
- gitea: required for hosting the git repos for genestack, kubespray and other required ansible collections
- pulp: run as a container on the host; docker is installed as a pre-requisite; storage directories are bind mounts from the host in case the container is re-created; data is preserved; this hosts the local pypi package index
- harbor: run as a container with docker-compose; there are a set of containers which are created by running the standard install.sh script provided by the harbor archive; storage is bind mount from the host; this hosts the 
container images required for an offline install and the helm charts themselves

It should be noted that beyond installing these tools; there are other steps which are required for an airgapped install of genestack which include:
- updating the apt sources on the target nodes to point to the local apt repo server
- updating the pip config on the target nodes to point to the local pypi package index
- providing the required overrides for kubespray installs via yaml files
- providing the required overrides for install helm charts via overrides

Requirements
------------

python should be installed on the ansible node and a virtual environment should be created; upgrade pip and install the requests package inside the virtual environment before running this role

Role Variables
--------------

The defaults/main.yml file contains variables for specifying:
- directory path for ubuntu mirror on the target host
- selecting which ubuntu release (i.e jammy, noble etc) should be mirrored along with the sub-sections (main,universe etc)
- providing the fqdn for the virtual host for hosting the local apt mirror and local pypi package index
- specifying the wild-card domain for the self-signed certs which the role generates
- specifying the version of gitea along with the username and passwd for the gitea admin user

consult defaults/main.yml for further details


License
-------

BSD

Author Information
------------------

Author: Punit Shankar Kundal
Email: punitshankar.kundal@rackspace.com
