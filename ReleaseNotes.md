## Borgsync Release Notes

The `borgsync` command is a wrapper to simplify backups with `borgbackup`
and provide command line management of an [rsync.net](https://rsync.net)
and/or [BorgBase](https://www.borgbase.com) off-site storage account.

## Requirements

The `borgsync` command can be installed on `Linux` or `macOS`. It is a command
line utility requiring a terminal or console and the `Bash` shell.

## Installation

To install `borgsync` clone this repository and run the `install` script:

```bash
git clone https://github.com/doctorfree/borgsync
cd borgsync
./install
```

The `install` script can be run either as the `root` user or as a user with
`sudo` privilege. The `install` script will check if the `borg` command is in
your `PATH` and, if not, it will download and install the latest release of `borg`.

After ensuring that `borg` is installed the `install` script copies the
`borgsync` command into `/usr/local/bin/`.

In addition, the Borg Backup GUI [Vorta](https://vorta.borgbase.com) is installed.

To install the `borgsync` command and configuration files in the user's home
rather than system-wide, invoke the `install` script with the `user` argument:

```bash
./install user
```

If `borgsync` is installed in the user home, add `$HOME/.local/bin` to your `PATH`.

***At this point your system is ready but borgsync is not yet configured***

Follow the steps in the [Configuration section](#configuration) to complete
the installation and configuration of `borgsync`.

## Configuration

By default `borgsync` expects the base system-wide configuration to be located
at `/etc/borgsync/base`. Individual users can override these defaults in
`~/.config/borgsync/base`.

The `install` script copies the example configuration to `/etc/borgsync/base`
but the remote user and host must be set manually:

```bash
vi /etc/borgsync/base    # customize with your storage service user/host/etc
```
### Custom configurations

Several custom configurations are installed in `/etc/borgsync/`:

* `/etc/borgsync/base`: the default configuration, used as the base for all others
* `/etc/borgsync/full`: perform a full system backup (`borgsync -b create -t full`)
* `/etc/borgsync/home`: backup /home (`borgsync -b create -t home`)
* `/etc/borgsync/logs`: backup /var/log (`borgsync -b create -t logs`)
* `/etc/borgsync/photos`: backup a photo library (`borgsync -b create -t photos`)
* `/etc/borgsync/test`: example backup using BorgBase (`borgsync -b create -t test`)

Additional custom configurations can be created by adding files to `/etc/borgsync` or
`~/.config/borgsync`. Use the `-t <name>` argument to specify the `/etc/borgsync/<name>`
configuration. Custom configurations all use `/etc/borgsync/base` as the base configuration.
Only configuration variables that differ from the base need to be specified in
a custom configuration.

## Usage

Get started with `borgsync` by editing `/etc/borgsync/base` and setting
the `REMOTE_USER` and `REMOTE_HOST` variables to your storage service
user and host. For example, an `rsync.net` account for user `fm1872`
would have the following settings in `/etc/borgsync/base`:

```bash
REMOTE_USER="fm1872"
REMOTE_HOST="fm1872.rsync.net"
```

The defaults in `/etc/borgsync/base` should suffice for most users but
backup paths, exclusions, options, and more can be customized. Once these
are configured:

* Initialize a `borg` backup repository with the command `borgsync -b init`
* Create the default `borg` backup with the command `borgsync`

The `borgsync` usage message can be displayed with `borgsync -u`.

## Automated scheduled backups

Automated periodic backups can be scheduled with `systemd` or `cron`

See the [project README](https://github.com/doctorfree/borgsync#readme)
for additional details.
