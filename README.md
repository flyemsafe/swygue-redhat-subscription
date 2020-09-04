swygue-redhat-subscription
==========================

This role will accomplish the following base on the variables pass:

 - register to the Red Hat Customer Portal or your Satellite server
 - unregister the system from either of the above
 - ensure repos you want enable, stay enable
 - ensure system is attach to the subscription pools you specify
 - when registered to Satellite, ensure system is attach to the content view you specify
 - when rebuilding a system, reattach to the system idenity you specify
 - when registered to Satellite, ensure katello and related client tools are installed and running
 - ensure the Red Hat Insights package is installed, running and registered
 - ensure the system is in the org you specify

Requirements
------------

This role depends on [jfenal/ansible-modules-jfenal redhat_repositories.py](https://raw.githubusercontent.com/jfenal/ansible-modules-jfenal/master/packaging/os/redhat_repositories.py) which is included.


Role Variables
--------------
| Variable        | Required | Default  | Description                                                                                                                                                                                                                                     |
| --------------- | -------- | -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|rhsm_unregister     |:heavy_check_mark: |false |Force a system to unregister if it's already registered. Also will cause a system not to be registered.|
|rhsm_is_satellite   |:heavy_check_mark: |```false```|Set to true to register system to satellite server|
|rhsm_hostname |:heavy_check_mark: |```subscription.rhsm.redhat.com```|This should be your Satellite server if you are using that instead of RHSM. check_rhsm_hostname.yml will force the system to re-register if this does not match what's in /etc/rhsm/rhsm.conf|
|rhsm_user|:x:|null|User name for Satellite or RHSM. This is use to register systems when not using activation keys or when using existing idenitiy|
|rhsm_password|:x:|null|Password for rhsm_user|
|rhsm_activationkey|:heavy_check_mark: |null|this will cause the system to register using an activation key|
|rhsm_org_id|:x:|null|This is required when using activation keys to register a system.|
|rhsm_identity|:heavy_check_mark: |null|register system using the system identity returned by ```subscription-manager identity``` |
|rhsm_content_view|:x:|null|set the content view to force system to re-register system when content view does not match|
|rhsm_katello_ca_consumer_rpm|:heavy_check_mark: |```katello-ca-consumer-latest.noarch.rpm```|Satellite katello RPM|
|rhsm_satellite_client_pkgs|:x:|```see defaults/main.yml``` |Only the katello-agent is required for best experince with Satellite. Required when setting up Satellite server.|
|rhsm_pool_ids|:x:|null|"Refer to the ansible docs for redhat_subscription. When set| this will ensure a registered system is attach to the pools specifiied."|
|rhsm_repos|:heavy_check_mark: |```see defaults/main.yml``` |List all the repo id's the system should be subscribe to. This will remove all existing repos not in this list.|
|rhsm_repos_to_disable|:x:|| |
|rhsm_setup_insights_client|:heavy_check_mark: |```true```|Installs and setup the insights client. You should be using the offcial [role](https://github.com/RedHatInsights/insights-client-role) for more configuration options.|
|rhsm_insights_client_pkgs|:x:|```see defaults/main.yml```|Required when setting insights|
|rhsm_fix_registration|:x:|:heavy_check_mark:|Set this to force a re-registration of the system|


Dependencies
------------

RHEL with subscription-manager

Example Playbook
----------------

**Unregister a system**
```
- name: PLAY| unregister system
  hosts: vm01
  remote_user: root
  become: false
  gather_facts: true
  vars:
    rhsm_unregister: true

  tasks:
    - name: run subscription role
      include_role:
        name: swygue-redhat-subscription
```

**Register to RHSM with username and password**
```
- name: PLAY| register system
  hosts: vm01
  remote_user: root
  become: false
  gather_facts: true
  vars:
    rhsm_user: user
    rhsm_pass: password

  tasks:
    - name: run subscription role
      include_role:
        name: swygue-redhat-subscription
```

**Register a system using an activation key**

```
- name: PLAY| register system using activaiton key
  hosts: vm01
  remote_user: root
  become: false
  gather_facts: true
  vars:
    rhsm_activationkey: rhel-server
    rhsm_org_id: ACME
```

License
-------

BSD

Author Information
------------------
[flyemsafe/swygue-redhat-subscription](https://github.com/flyemsafe/swygue-redhat-subscription)
