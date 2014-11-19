fuel-utils
==========

Utilities to make it easier to work &amp; develop with Mirantis Fuel.

Set up your Fuel environment from ISO as described here for example http://samuraiincloud.com/2014/08/13/building-openstack-icehouse-in-virtualbox-in-60-minutes-using-mirantis-fuel-2/

# `bin/fuel` script

Currently the only script in this repo, add it to your `$PATH` to make access easier.

It is assumed that your `fuel-master` machine is under IP `10.20.0.2`. If it's different, specify it in the `--ip`
argument.  Also the script tries to send your SSH public key credentials to the `fuel-master` server to save you from
typing passwords. You can change the location of the SSH key by passing the `--ssh-identity-file` variable in the
script. If can also turn off sending the SSH identity file by passing the `--no-ssh-identity-file` option but you may be
prompted for root password, even couple of times.

Here's a list of supported commands:

---

## Global options

### `--ip`

Specify IP of the Fuel master ISO (default is `10.20.0.2`).

### `--no-ssh-identity-file`

Don't try to send the SSH identity file if it's not present on the Fuel master ISO.

### `--ssh-identity-file`

Specify SSH identity file (default is `$HOME/.ssh/id_rsa.openstack`).

### `--verbose`

Be more verbose.

---

## `info`

Print some info about all Docker containers.

## `send-identity`

Send your public identity key to master server for passwordless authentication.

---

## `ssh`

SSH into `fuel-master`.

---

## `docker {subcommand}`

manipulate Docker containers

###### `--container`, `-c`

Specify which container to manipulate (`nailgun` is the default)

### Subcommands

### `id`

Print ID of the container

### `config`

Print JSON configuration of the container

### `dir`

Print `rootfs` directory of the container

### `log`

Print last 100 lines of logs of a container (by default all logs)

###### `[files list]`

Additionally specify files for which the logs are to be printed

### `restart`

Restart container

### `rsync`

Rsync directory into the container (default: current working dir)

###### `--source`, `-s`

Specify the rsync source directory

### `rsync-static`

Rsync static files into the container (default: current working dir)

###### `--no-grunt`

Don't run Grunt building task (default: False).

Note that by default the minified version is used by fuel-main so if you change the static files and rsync without running `grunt build` you won't see the changes in `fuel-main`..

###### `--source`, `-s`

Specify the rsync source directory

### `shell`

Shell into the container. By default, nailgun container shells into `IPython` environment, while PostgreSQL container runs `psql` with the `nailgun` database.

###### `--shell-command`, `-c`

Specify an optional command to run in the Docker shell. This overrides the default command (IPython for nailgun, `psql` for PostgreSQL, etc). Run it like this:

```
fuel docker shell -c /bin/bash
```

For example, if you want a fresh database (because some new models showed up in the current migration) you can do:

```
fuel docker -c postgres shell
\c postgres
-- kill processes if DROP DATABASE doesn't work:
-- SELECT pg_terminate_backend(pid) FROM pg_stat_activity WHERE pid <> pg_backend_pid() AND datname = 'nailgun'; -- Postgres 9.2
-- SELECT pg_terminate_backend(procpid) FROM pg_stat_activity WHERE procpid <> pg_backend_pid() AND datname = 'nailgun';  -- Postgres 9.1
DROP DATABASE nailgun;
CREATE DATABASE nailgun WITH OWNER nailgun;
\q
fuel docker shell -c /bin/bash
manage.py syncdb
manage.py loaddefault
```

### `start`

Start the container

### `stop`

Stop the container

### `tail`

Continuously inspect (tail) logs of a container (default: all logs).

###### `[files list]`

Restrict the log files to specified list

### `volumes`

Print all volumes of a container.

---

## `puppet {subcommand}`

Manipulate Puppet on Fuel Master

### Subcommands

### `rsync`

Rsync `fuel-libs` Puppet modules into Fuel Master.

###### `--source`, `-s`

Specify the rsync source directory

---

# TODOs

* After ISO rebuild error is thrown about `.ssh/known_hosts` -- detect it and fix automatically to minimize user pain.
* Better error reporting about wrong command, no SSH ID file, etc
