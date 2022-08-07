amazon_linux_extras

=========

A minimal wrapper role around the amazon-linux-extras command. This to be used used with Amazon Linux 2 and above.

Role Variables
--------------

The following data structure can be define to allow enablement:

```
amazon_linux_extras_components:
  - name: app
    pkgs: app_pkg
    svc: app-svc
    add_pip_pkgs:
      - app_pip
    add_yum_pkgs:
      - app_lib_pkg

```

- `name`: name of app to be enable
- `pkgs` : name of app package to pass to amazon-linux-extras
- `svc`: service attached to package that needs to be started. This is optional.
- `app_pip_pkgs`: additional packages via pip used to support the app. This is optional.
- `app_pip_yum`: additional packages via yum used to support the app. This is optional.


Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - amazon_linux_extras

License
-------

BSD

Author Information
------------------

Rilindo Foster <rilindo.foster@monzell.com>
