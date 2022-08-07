httpd
=========

A MVP of a role to install apache httpd

Requirements
------------

This is to be run on RHEL-compatible system.

Role Variables
--------------


- `httpd_pkg`: list of httpd packages
- `httpd_svc`: service name of httpd

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - httpd

License
-------

BSD

Author Information
------------------

Rilindo Foster <rilindo.foster@monzell.com>

