WordPress Demo
==============

This launches an Amazon Linux 2 instance and then deploys a minimal viable install of WordPress. 

Implementation
--------------

The deployment work something like this:

1. First, the user is prompted for base AWS network info, after which, a t3a.micro instance is launched.
2. Then, the play waits until the instance is reachable.
3. Next, the inventory is refreshed to allow ansible to look up the new instance and connect to it.
4. Then, the user is prompted for an domain to enter. That domain is validated shortly afterward to confirm that it is a "valid" domain.
5. Credential secrets are read from file and decrypted if necessary.
6. The database directory is created for the database container.
7. The current forcast for Chicago is retrieved for first post.
8. Afterwards, roles are invoked to install required components and commands.
9. Then, a database container instance is created.
10. Finally, wordpress is deployed with wrapper plays and a post with the current to published.

Requirements
------------

1. The most recent version of Ansible (at least 5 and above).
2. docker and amazon aws collections (should be included with Ansible core).
3. An AWS account with a configured keypair.

Testing
-------

This was developed on a Mac and ran against Amazon Linux 2 in a non-default VPC in us-west-2 with AMI ID `ami-0cea098ed2ac54925` and with the following versions of:

```
ansible                 6.1.0
ansible-core            2.13.2
boto3                   1.24.46
botocore                1.27.46
```

Instructions
------------

1. Change over to the installed directory, as it relys on the `ansible.cfg` for base configuration functionality.
2. Next, if you are launching from a bastion host, configure `inventory/aws_ec2.yml` to use `private-ip-address` instead. Otherwise, you may run it from your local workstation.
3. In addition, update or replace `secrets.yml` with the followings variables:

- `mysql_database`: WordPress database name
- `mysql_password`: WordPress database name
- `mysql_root_password`: mySQL root password

and then encrypt the file with `ansible-vault`

4. Finally, run `ansible-playbook --ask-vault-pass site.yml`. It will ask the following during the run:

- `security group id`: enter security group that allows ingress via ssh and http to your instance
- `subnet id`: enter the subnet that you intend to expose wordpress app to, (for example, public subnet)
- `AWS keypair`: the kay pair you setup
- `domain name`: the domain name that is attached to the wordpress.


The whole output should like this:

```
rilindo@MacBook-Air ansible_wordpress_demo % ansible-playbook --ask-vault-pass site.yml        
Vault password: 
[WARNING]: provided hosts list is empty, only localhost is available. Note that the implicit localhost does not match 'all'
enter ami id, enter to use default [ami-0cea098ed2ac54925]: 
enter security group id: sg-0590ac9090a5fa772
enter subnet id: subnet-0fd8495487e71b137
enter AWS keypair: TestKeyPair

PLAY [launch EC2 instance] ************************************************************************************************************************

TASK [Gathering Facts] ****************************************************************************************************************************
ok: [localhost]

TASK [launching ec2 instance] *********************************************************************************************************************
[DEPRECATION WARNING]: The purge_tags parameter currently defaults to False. For consistency across the collection, this default value will change
 to True in release 5.0.0. This feature will be removed from amazon.aws in version 5.0.0. Deprecation warnings can be disabled by setting 
deprecation_warnings=False in ansible.cfg.
changed: [localhost]

TASK [wait for ssh to come up] ********************************************************************************************************************
ok: [localhost]

TASK [meta] ***************************************************************************************************************************************
enter domain name: example.org

PLAY [begin wordpress deployment] *****************************************************************************************************************

TASK [Gathering Facts] ****************************************************************************************************************************
[WARNING]: Platform linux on host 50.112.48.95 is using the discovered Python interpreter at /usr/bin/python3.7, but future installation of
another Python interpreter could change the meaning of that path. See https://docs.ansible.com/ansible-
core/2.13/reference_appendices/interpreter_discovery.html for more information.
ok: [50.112.48.95]

TASK [assert] *************************************************************************************************************************************
ok: [50.112.48.95] => {
    "changed": false,
    "msg": "valid domain"
}

TASK [Create directory for mysql container files] *************************************************************************************************
changed: [50.112.48.95]

TASK [get current weather forecast] ***************************************************************************************************************
ok: [50.112.48.95]

TASK [amazon_linux_extras : include_tasks] ********************************************************************************************************
included: /Users/rilindo/src/cleo_assessement/roles/amazon_linux_extras/tasks/enable.yml for 50.112.48.95

TASK [amazon_linux_extras : enable components to be deployed from Amazon Linux Extras] ************************************************************
changed: [50.112.48.95] => (item={'name': 'docker', 'pkgs': 'docker', 'svc': 'docker', 'add_pip_pkgs': ['docker']})
changed: [50.112.48.95] => (item={'name': 'php7.4', 'pkgs': 'php7.4', 'add_yum_pkgs': ['php-cli', 'php-pdo', 'php-fpm', 'php-json', 'php-mysqlnd']})
changed: [50.112.48.95] => (item={'name': 'epel', 'pkgs': 'epel'})

TASK [amazon_linux_extras : include_tasks] ********************************************************************************************************
included: /Users/rilindo/src/cleo_assessement/roles/amazon_linux_extras/tasks/install_main.yml for 50.112.48.95

TASK [amazon_linux_extras : install packages] *****************************************************************************************************
changed: [50.112.48.95] => (item={'name': 'docker', 'pkgs': 'docker', 'svc': 'docker', 'add_pip_pkgs': ['docker']})
changed: [50.112.48.95] => (item={'name': 'php7.4', 'pkgs': 'php7.4', 'add_yum_pkgs': ['php-cli', 'php-pdo', 'php-fpm', 'php-json', 'php-mysqlnd']})
changed: [50.112.48.95] => (item={'name': 'epel', 'pkgs': 'epel'})

TASK [amazon_linux_extras : include_tasks] ********************************************************************************************************
included: /Users/rilindo/src/cleo_assessement/roles/amazon_linux_extras/tasks/install_additional.yml for 50.112.48.95

TASK [amazon_linux_extras : install additional components from yum] *******************************************************************************
skipping: [50.112.48.95] => (item={'name': 'docker', 'pkgs': 'docker', 'svc': 'docker', 'add_pip_pkgs': ['docker']}) 
ok: [50.112.48.95] => (item={'name': 'php7.4', 'pkgs': 'php7.4', 'add_yum_pkgs': ['php-cli', 'php-pdo', 'php-fpm', 'php-json', 'php-mysqlnd']})
skipping: [50.112.48.95] => (item={'name': 'epel', 'pkgs': 'epel'}) 

TASK [amazon_linux_extras : install additional components from pip] *******************************************************************************
changed: [50.112.48.95] => (item={'name': 'docker', 'pkgs': 'docker', 'svc': 'docker', 'add_pip_pkgs': ['docker']})
skipping: [50.112.48.95] => (item={'name': 'php7.4', 'pkgs': 'php7.4', 'add_yum_pkgs': ['php-cli', 'php-pdo', 'php-fpm', 'php-json', 'php-mysqlnd']}) 
skipping: [50.112.48.95] => (item={'name': 'epel', 'pkgs': 'epel'}) 

TASK [amazon_linux_extras : include_tasks] ********************************************************************************************************
included: /Users/rilindo/src/cleo_assessement/roles/amazon_linux_extras/tasks/services.yml for 50.112.48.95

TASK [amazon_linux_extras : enable services] ******************************************************************************************************
changed: [50.112.48.95] => (item={'name': 'docker', 'pkgs': 'docker', 'svc': 'docker', 'add_pip_pkgs': ['docker']})
skipping: [50.112.48.95] => (item={'name': 'php7.4', 'pkgs': 'php7.4', 'add_yum_pkgs': ['php-cli', 'php-pdo', 'php-fpm', 'php-json', 'php-mysqlnd']}) 
skipping: [50.112.48.95] => (item={'name': 'epel', 'pkgs': 'epel'}) 

TASK [httpd : include_tasks] **********************************************************************************************************************
included: /Users/rilindo/src/cleo_assessement/roles/httpd/tasks/install.yml for 50.112.48.95

TASK [httpd : installing httpd] *******************************************************************************************************************
changed: [50.112.48.95]

TASK [httpd : include_tasks] **********************************************************************************************************************
included: /Users/rilindo/src/cleo_assessement/roles/httpd/tasks/service.yml for 50.112.48.95

TASK [httpd : enable httpd service] ***************************************************************************************************************
changed: [50.112.48.95]

TASK [mysql : include_tasks] **********************************************************************************************************************
included: /Users/rilindo/src/cleo_assessement/roles/mysql/tasks/repo.yml for 50.112.48.95

TASK [mysql : install mysql repo] *****************************************************************************************************************
changed: [50.112.48.95]

TASK [mysql : include_tasks] **********************************************************************************************************************
included: /Users/rilindo/src/cleo_assessement/roles/mysql/tasks/install.yml for 50.112.48.95

TASK [mysql : install mysql, so that we can use the client] ***************************************************************************************
changed: [50.112.48.95]

TASK [wp_cli : retrieve wp-cli] *******************************************************************************************************************
changed: [50.112.48.95]

TASK [instantiate  wordpress mysql docker container] **********************************************************************************************
changed: [50.112.48.95]

TASK [wait for mySQL container to finish reconfiguring] *******************************************************************************************
Pausing for 120 seconds
(ctrl+C then 'C' = continue early, ctrl+C then 'A' = abort)
ok: [50.112.48.95]

TASK [download and extract wordpress to target directory] *****************************************************************************************
changed: [50.112.48.95]

TASK [create wordpress configuration] *************************************************************************************************************
changed: [50.112.48.95]

TASK [finalize installation of wordpress] *********************************************************************************************************
changed: [50.112.48.95]

TASK [create first wordpress post] ****************************************************************************************************************
changed: [50.112.48.95]

PLAY RECAP ****************************************************************************************************************************************
50.112.48.95               : ok=28   changed=15   unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
localhost                  : ok=3    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```

Notes
-----

- The roles are created in lieu of using third party roles in Galaxy as support for Amazon Linux 2 is spotty at best
- The roles have just enough functionality to support deploying WordPress as well keep the main playbook from ballooning into a mess. That said, they can be extended if necessary (for example, the `mysql` role).
- We used the `wp-cli` command instead of using native Ansible modules such as `get_url` as  1) `wp-cli` is more purpose built and therefore suitable for deploying WordPress and 2) The role itself can be extend to be Ansible module in future revisions, if necessary.
- We pulled weather forecast from [wttr.in](http://wttr.in) as that does not require an API key, which simplies retrieval and processing.
- This is not meant to be a idempotent deployment. We recommend terminating the instance to re-deploy a new copy of wordpress. That said, re-running the deployment will not cause any issues other than another "first" post.

License
-------

BSD

Author Information
------------------

Rilindo Foster <rilindo.foster@monzell.com>