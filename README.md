fuel-utils
==========

Utilities to make it easier to work &amp; develop with Mirantis Fuel.

Set up your Fuel environment from ISO as described here for example http://samuraiincloud.com/2014/08/13/building-openstack-icehouse-in-virtualbox-in-60-minutes-using-mirantis-fuel-2/

# `bin/fuel` script

Currently the only script in this repo, add it to your `$PATH` to make access easier.

It is assumed that your `fuel-master` machine is under IP `10.20.0.2`. If it's different, change the `IP` variable in the script.
Also the script tries to send your SSH public key credentials to the `fuel-master` server to save you from typing passwords. You can change the location of the SSH key by changing the `IDENTITY_FILE` variable in the script.

Here's a list of supported commands:

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

## `send-identity`

SSH into `fuel-master` (currently using `pexpect`, the shell is interactive but lacks prompt)

# TODOs

* After ISO rebuild error is thrown about `.ssh/known_hosts` -- detect it and fix automatically to minimize user pain.
* Better error reporting about wrong command, no SSH ID file, etc
