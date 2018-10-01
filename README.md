# ansible-nicos
Ansible installer for NICOS.

## Prerequisites
Works on RedHat-based and Debian-based Linux versions.

Install the following software:
 - ansible (2.4.0 or higher)
 - git

Note: sometimes the Ansible scripts exit with a segmentation fault - this can be ignored.

## Installing the GUI only
This is for setting up a NICOS client machine.

To install the NICOS GUI on the local machine run:

```shell
ansible-playbook install-nicos-client.yml
```

## Installing the server
This is for setting up a NICOS server machine (i.e. an instrument machine).

### Installing locally
The anisble script will install on localhost by default.

To install run:

```shell
sudo ansible-playbook install-nicos.yml
```

By default the ansible script will set NICOS to use the demo instrument.
This and other install details can be changed by supplying additional command line arguments:

```shell
facilty - specify the overall facility (e.g. ess, sinq, demo etc.)
instrument - specify the instrument at the nicos_facility (e.g. amor, cspec etc.)
username - the un*x user to install nicos as
usergroup - the un*x group to install nicos as
variable_host - the machine to install on (default is localhost). Must be defined in the hosts file
download - 'yes' will download NICOS from the ESS staging area rather than cloning it from Gerrit
```

#### Examples

Install as a specific instrument:

```shell
sudo ansible-playbook install-nicos.yml -e facility=ess -e instrument=v20
```

Install as a specific instrument and user:

```shell
sudo ansible-playbook install-nicos.yml -e facility=ess -e instrument=essiip -e username=nicos_user -e usergroup=nicos_user
```

Note: facility and instrument settings can be changed post-installation by editing the `nicos.conf` file in `/opt/nicos`.

Install from the ESS staging area:

```shell
sudo ansible-playbook install-nicos.yml -e facility=ess -e instrument=essiip -e download=yes
```

### Installing NICOS on a different machine (not localhost)
Make sure that the ``hosts`` file points to the IP of the machine where NICOS is to be installed.

To install on that machine:
```shell
ansible-playbook -i hosts install-nicos.yml -e host=NICOS
```

### Trouble-shooting
If the playbook fails with the message:

```
> "Failed to connect to the host via ssh: muxserver_listen bind(): Operation not permitted\r\n"
```
add ``-c paramiko`` to the previous command.

## Start and stop NICOS services (server only)

The playbooks ``start_nicos_services.yml``, ``stop_nicos_services.yml`` and ``restart_nicos_services.yml`` can be used to start, stop and restart the NICOS services.

```shell
sudo ansible-playbook start_nicos_services.yml    # Start services
sudo ansible-playbook stop_nicos_services.yml    # Stop services
sudo ansible-playbook restart_nicos_services.yml    # Restart services
```
Add `-i hosts` if running on a different machine, for example:

```shell
sudo ansible-playbook -i hosts start_nicos_services.yml
```
