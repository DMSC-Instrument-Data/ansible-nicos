# ansible-nicos
Ansible installer for NICOS. Can also start and stop the NICOS services.

## Prerequisites
Install the following software:
 - ansible (2.4.0 or higher)
 - git

## Installing NICOS on a VM
Make sure that the ``hosts`` file points to the IP of the VM where NICOS is to be installed.

The playbook ``install-nicos.yml`` sets up everything including:
 - installing NICOS
 - installing the requirements
 - creating data directories
 - setting up NICOS instruments
 - creating the service files.

All the useful variables are defined in the playbook.

```shell
ansible-playbook -i hosts install-nicos.yml
```

If the playbook fails with the message:

```
> "Failed to connect to the host via ssh: muxserver_listen bind(): Operation not permitted\r\n"
```
add ``-c paramiko`` to the previous command.

### Installing locally
Edit the `install-nicos.yml` file so that the `hosts` setting is `localhost`.
Then run using:

```shell
ansible-playbook install-nicos.yml
```

## Start and stop NICOS services

The playbooks ``start_nicos_services.yml``, ``stop_nicos_services.yml`` and ``restart_nicos_services.yml`` can be used to start, stop and restart the NICOS services.

```shell
ansible-playbook -i hosts start_nicos_services.yml    # Start services
ansible-playbook -i hosts stop_nicos_services.yml    # Stop services
ansible-playbook -i hosts restart_nicos_services.yml    # Restart services
```
