mysql
=========

A minimal setup of mysql community package. It doesn't do much other than install mySQL and the client packages. It is primarily used to support connecting to a container instance of mySQL as well as provide library to non-container apps.

Requirements
------------

This is to be run on a RHEL-based system.

Role Variables
--------------

- `mysql_repo_url`: URL of mySQL repo.
- `mysql_pkgs`: mySQL packages.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - mysql

License
-------

BSD

Author Information
------------------

Rilindo Foster <rilindo.foster@monzell.com>
