Global Registration
=========

Register content host to Satelite or Capsule/Load-Balancer using the [Global Registration API](https://access.redhat.com/documentation/en-us/red_hat_satellite/6.13/html/managing_hosts/registering_hosts_to_server_managing-hosts#Registering_Hosts_by_Using_Global_Registration_managing-hosts)

By default call to API is done from content host. So it you're registring directly to Satellite it's using port 443, otherwise if using Capsule it's using deprecated port 8443.

Requirements
------------

Repository Red Hat Satellite Client 6 must be part of the content view to install the katello-host-tools packages.

Role Variables
--------------

**Mandatory variables**

Credentials for user allowed to register host

- global_registration_username
- global_registration_password

User must have the "Regsiter hosts" role. Check https://access.redhat.com/solutions/7034461 if Red Hat Satellite 6.13 and earlier.


FQDN of Capsule/Load-balancer used to register the content host. If Satellite integrated capsule override global_registration_host in your playbook.

- global_registration_capsule

Organization Name

- global_registration_organization

Content Host Location

- global_registration_location

Comma separated list of activation keys

- global_registration_activation_keys

**Optional variables (Not Tested)**

- global_registration_hostgroup_id
- global_registration_operatingsystem_id
- global_registration_repo
- global_registration_repo_gpg_key_url
- global_registration_remote_interface

**Default variables**

Are we using Satellite or Capsule to register host ?

- global_registration_host: "capsule"

Do content host knows Certificate Authority used to sign Satellite/Capsule certificate ?

- global_registration_validate_certs: true

Setup Insights

- global_registration_setup_insights: yes

Setup REX

- global_registration_setup_remote_execution: yes

Token life time

- global_registration_token_life_time: 4

Install Packages during the registration. Red Hat Satellite Client 6 must be part of the content view to install the katello-host-tools packages.

- global_registration_packages: "katello-host-tools katello-host-tools-tracer"

Update content hosts packages

- global_registration_update_packages: yes

REX Pull mode

- global_remote_execution_pull: false

Remove any `katello-ca-consumer` rpms before registration and run subscription-manager with `--force` argument.

- global_registration_force: false

Print role debug information

- global_registration_debug: false

**Main variables**

Port used to generate  registration command using Capsule/Load-Balancer.

- global_registration_port: 8443

Use basic authentication to log to API

- global_registration_force_basic_auth: true

Ignore subscription manager errors

- global_registration_ignore_subman_errors: false

Example Playbook
----------------

```
- name: "Register Host using Capsule/Load-Balancer"
  hosts: server1
  gather_facts: false
  roles:
    - role: global_registration
      vars:
        # Mandatory vars
        - global_registration_username: "reg_user"
        - global_registration_password: "reg_pass"
        - global_registration_capsule: "loadbalancer.gnali.lab"
        - global_registration_organization: "Gnali"
        - global_registration_location: "Pau"
        - global_registration_activation_keys: ak_virtual_rhel8
        # Override default vars
        - global_registration_validate_certs: false
        - global_registration_setup_insights: false
        - global_registration_setup_remote_execution: false
        - global_registration_update_packages: false
```

```
- name: "Register Host using Satellite"
  hosts: server2
  gather_facts: false
  roles:
    - role: global_registration
      vars:
        # Mandatory vars
        - global_registration_username: "reg_user"
        - global_registration_password: "reg_pass"
        - global_registration_capsule: "satellite.gnali.lab"
        - global_registration_organization: "Gnali"
        - global_registration_location: "Paris"
        - global_registration_activation_keys: ak_virtual_rhel8
        # Override default vars
        - global_registration_host: "satellite"
        - global_registration_validate_certs: false
        - global_registration_setup_insights: false
        - global_registration_setup_remote_execution: false
        - global_registration_update_packages: false
```

License
-------

BSD

Author Information
------------------

[Stephane V.](https://www.gnali.org) : tinsjourney@mastodon.top
