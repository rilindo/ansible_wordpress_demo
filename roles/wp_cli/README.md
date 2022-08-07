wp_cli
=========

Installs wp-cli, for us to deploy Wordpress

Requirements
------------

A proper install of most recent version of PHP

Role Variables
--------------

- `wp_cli_name`: wp command name
- `wp_cli_url`: wp-cli url
- `wp_cli_dest_path`: destination path
- `wp_cli_user`: wp cli user
- `wp_cli_group`: wp cli group that the wp cli will become to

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - wp_cli

License
-------

BSD

Author Information
------------------

Rilindo Foster <rilindo.foster@monzell.com>
