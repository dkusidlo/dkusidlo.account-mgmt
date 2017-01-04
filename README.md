Role Name
=========

Role for managing user and groups.

Requirements
------------

* Ubuntu Xenial
* Ansible 2

Role Variables
--------------
```
# where to store ssh keys. username gets appended to key_location
key_location: '/var/lib/authorized_keys/'

ssh_PasswordAuthentication: [false]
ssh_PermitRootLogin: [false]
ssh_UsePAM: [false]
ssh_ClientAliveInterval: [600]
ssh_ClientAliveCountMax: [3]
ssh_MaxAuthTries: [4]

user_groups:  
  - name:  
    PasswordAuthentication:  
    AllowTcpForwarding:  
    AllowAgentForwarding:  
    X11Forwarding:  
    ForceCommand:  

user:
  - name:  
    state:  
    groups:  
    shell:  
    password:  # python -c 'import crypt; print crypt.crypt("mypassword","$1$somesalt!")'
    key:
```
Example Playbook
----------------

```
  - hosts: all
    roles:
      - role: dkusidlo.account-mgmt
        user_groups:
          - name: testgroup1
            PasswordAuthentication: 'yes'
            AllowTcpForwarding: 'yes'
            AllowAgentForwarding: 'yes'
            X11Forwarding: 'yes'
            ForceCommand: 'yes'
          - name: testgroup2
            PasswordAuthentication: 'no'

        user:
          - name: testuser1
            state: present
            groups: 'testgroup1'
            shell: /bin/zsh
            password: '$yourhashedpw'
            key: 'rsa-ssh ... x@y.z'
          - name: testuser2
            groups: 'testgroup2'
            shell: /bin/false
```


 Development & Testing
 ---------------------

 This project uses [Molecule](http://molecule.readthedocs.io/) to aid in the
 development and testing; the role is unit tested using
 [Testinfra](http://testinfra.readthedocs.io/) and
 [pytest](http://docs.pytest.org/).

 To develop or test you'll need to have installed the following:

 * Linux (e.g. [Ubuntu](http://www.ubuntu.com/))
 * [Docker](https://www.docker.com/)
 * [Python](https://www.python.org/) (including python-pip)
 * [Ansible](https://www.ansible.com/)
 * [Molecule](http://molecule.readthedocs.io/)

 To run the role (i.e. the `tests/test.yml` playbook), and test the results
 (`tests/test_role.py`), execute the following command from the project root
 (i.e. the directory with `molecule.yml` in it):

 ```bash
 molecule test
 ```

License
-------

BSD

Author Information
------------------

Dennis Kusidlo  
https://github.com/dkusidlo
