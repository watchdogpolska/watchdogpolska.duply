Duply Playbook
==============

The Ansible role is responsible for ensuring backup configuration of duply via duplicity.

[Duplicity](http://duplicity.nongnu.org/) is a python based shell application that makes encrypted incremental backups to remote storages in push-model.

[Duply](http://duply.net/) is a frontend for the mighty duplicity magic. duply simplifies running duplicity with cron or on command line by:
* keeping recurring settings in profiles per backup job
* automated import/export of keys between profile and keyring
* enabling batch operations eg. backup_verify_purge
* executing pre/post scripts
* precondition checking for flawless duplicity operation

The role simplifies duply configuration by:

* forcing variables with access data appropriate for the backend
* centralization of duply configuration
* validation of credentials
* manage pre-backup script to execute MySQL / PostgreSQL dump

Requirements
------------

Use of this role requires that you indicate the correct access data to the storage backend. Currently available backends is:
* S3 storage, including s3-compatible services eg [e24files of e24cloud](https://panel.e24cloud.com/referal/GuFfaD31),
* Swift storage eg Oktawave Files
* Microsoft Azure blob storage

The entered access data is verified when performing the role.

Role Variables
--------------

Sporadic backup verification mechanism has been introduced. Set `` duply__frequency_verify`` to specify how often (in days) should be executed. The role will ensure that verification on multiple servers is not performed simultaneously.

A detailed description of the settable variables for this role is defined in ``defaults/main.yml``.

Dependencies
------------

There is no dependencies roles.

``duply__postgresql_backup`` set to ``True`` require PostgreSQL installation.
``duply__mysql_backup`` set to ``True`` require MySQL installation.

Example Playbook
----------------

There is a example playbook:

    - hosts: servers
      roles:
         - name: watchdogpolska.duply
           duply__storage: s3
           duply__pw: FCoog7QM2MnmmEA  # encryption password
           duply__s3_host: e24files.com
           duply__s3_bucket: backup-duply-{{ansible_hostname}}
           duply__s3_path: "{{ansible_hostname}}"
           duply__s3__access_key: 'yoEeeYjj0NX1YcB'
           duply__secret_access: 'Cw4blRY6EOyCdsu'

I recommend use seperate credentials for each hosts, therefore setting variables through ``host_var`` directory. To use other backends, analyze ``defaults/main.yml`` comments.

License
-------

BSD

Author Information
------------------

The role was realized by Karol Breguła on behalf of [Citizens Network Watchdog Poland](http://siecobywatelska.pl/in-brief/?lang=en). The role is actively used (as of July 29, 2017) in Citizens Network Watchdog Poland for months.
