swygue-redhat-subscription
==========================

The role primary purpose is to register a system to the Red Hat Customer Portal using an activation key. It will attempt to re-register the system if one of the following conditions exist:

 1. The system is already registered but ```subscription-manager status``` output does not match **Overall Status: Current**.
 2. The ORG ID returned by  ```subscription-manager identity``` does not match the value defined in **rhsm_org_id**.

It can also re-register a system using the **system identity** that's returned by ```subscription-manager identity``` or taken from the Customer Portal. This assumes the system UUID still exist on RHSM. Useful for rebuilding a server via kickstart and you want to preserve it's RHSM history.

If you pass in the rhsm_pool id it will also ensure the system is attached to this pool. Currently this does not support multiple pool ids.

Finaly, it will ensure the system is only subscribe to the repos in the redhat_repos list. All other repos will be disabled.

Requirements
------------

This role depends on [jfenal/ansible-modules-jfenal redhat_repositories.py](https://raw.githubusercontent.com/jfenal/ansible-modules-jfenal/master/packaging/os/redhat_repositories.py). This needs to be placed in your ansible.cfg library path.

**Example**
```
grep library ~/.ansible.cfg
library        = /home/me/.ansible/modules/
cd /home/me/.ansible/modules/
wget https://raw.githubusercontent.com/jfenal/ansible-modules-jfenal/master/packaging/os/redhat_repositories.py
```
TODO: Switch to https://github.com/giovannisciortino/ansible/blob/5632c6c113bdedeae2bf21a4441d0847914f632a/lib/ansible/modules/packaging/os/rhsm_repository.py

Role Variables
--------------
| Variable        | Required | Default  | Description                                                                                                                                                                                                                                     |
| --------------- | -------- | -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|rhsm_unregister     |:heavy_check_mark: |false |Force a system to unregister if it's already registered. Also will cause a system not to be registered.|
|rhsm_is_satellite   |:heavy_check_mark: |```false```|Set to true to register system to satellite server|

Dependencies
------------

RHEL with subscription-manager

Example Playbook
----------------

**Unregister**
```
    - hosts: servers
      roles:
         - { role: swygue-redhat-subscription, rhsm_unregister: true }
```

**Register**
```
    - hosts: servers
      vars:
        - rhsm_org_id: ''
        - rhsm_activationkey: ''
        - redhat_repos:
            - rhel-7-server-rpms
            - rhel-7-server-optional-rpms
      roles:
         - { role: swygue-redhat-subscription }
```

License
-------

BSD

Author Information
------------------
[flyemsafe/swygue-redhat-subscription](https://github.com/flyemsafe/swygue-redhat-subscription)
