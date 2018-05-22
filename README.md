# ansible-nicos
Ansible installer for NICOS.

## Prerequisites
Works on RedHat-based and Debian-based Linux versions.

Install the following software:
 - ansible (2.4.0 or higher)
 - git

## Installing the GUI only
This is for setting up a NICOS client machine.

To install the NICOS GUI on the local machine run:

```shell
ansible-playbook install-nicos-client.yml
```

## Installing the server
This is for setting up a NICOS server machine (i.e. an instrument machine).

All the useful variables are defined in the playbook `install-nicos.yml`.

### Setting the facility and instrument
By default the ansible script will set NICOS to use the demo instrument. This can be changed by editing the `install-nicos.yml` file to set a facility and instrument, for example:
```
...
nicos_facility: "nicos_ess"
nicos_instrument: "essiip"
...
```
These settings can be changed post-installation by editing the `nicos.conf` file in `/opt/nicos`.

### Installing locally
The anisble script will install on localhost by default.

To install run:

```shell
sudo ansible-playbook install-nicos.yml
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
ansible-playbook -i hosts start_nicos_services.yml    # Start services
ansible-playbook -i hosts stop_nicos_services.yml    # Stop services
ansible-playbook -i hosts restart_nicos_services.yml    # Restart services
```
Add `sudo` and remove `-i hosts` if running on a local machine.
