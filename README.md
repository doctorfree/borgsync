# borgsync

The `borgsync` command is a wrapper to simplify backups with `borgbackup`
and provide command line management of an [rsync.net](https://rsync.net)
and/or [BorgBase](https://www.borgbase.com) off-site storage account.

## Overview

The `borgsync` command can be used to manage cloud storage on
[rsync.net](https://www.rsync.net), [BorgBase](https://www.borgbase.com), or any
remote server with `ssh` access.

`borgsync` supports remote backups using the [borg](https://www.borgbackup.org)
deduplicating archiver with compression and encryption.

Several excellent wrappers and front-ends for `borg` exist. The `borgsync`
command attempts to be simple, easy to setup and use, while still being fully
enough featured to satisfy the off-site storage and backup needs of most users.

Setup, configuration, and use of `borgsync` is easy:

* [Install](#installation): `git clone ...` and `./install`
* [Configure](#configuration): set `REMOTE_USER` and `REMOTE_HOST` in `/etc/borgsync/config`
* Initialize a `borg` backup repository with `borgsync -b init`
* Create the default `borg` backup with `borgsync`

In addition to managing `borg` backups, the `borgsync` command can be used
to upload/manage files and folders on the remote storage service through `ssh`.

For a more complex and thorough approach, see [Borgmatic](https://torsion.org/borgmatic).

In order to use `borgsync` an `rsync.net` or `BorgBase` account is not required
but defaults are configured for use with either `rsync.net` or `BorgBase`.

Pricing for these two popular and well maintained off-site storage services
ranges from $5-$10 per terabyte per month. BorgBase is cheaper, rsync.net
offers some additional features, both are excellent and affordable. BorgBase
offers a free trial plan with 10GB storage and 2 repositories along with
automated extension of your plan based on usage.

The author of `borgsync` is not affiliated with any storage service and does
not receive any compensation for this effort.

## Table of Contents

- [Requirements](#requirements)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [Automated scheduled backups](#automated-scheduled-backups)
  - [Automated backups using cron](#automated-backups-using-cron)
  - [Automated backups using systemd](#automated-backups-using-systemd)
- [Examples](#examples)
- [Borg server preparation](#borg-server-preparation)

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
`borgsync` commands into `/usr/local/bin/`:

```bash
sudo cp bin/borgsync /usr/local/bin/borgsync
sudo chown root:root /usr/local/bin/borgsync
sudo chmod 750 /usr/local/bin/borgsync
```

In addition, the Borg Backup GUI [Vorta]() is installed.

***At this point your system is ready but borgsync is not yet configured***

Follow the steps in the [Configuration section](#configuration) to complete
the installation and configuration of `borgsync`.

## Configuration

By default `borgsync` expects the system-wide configuration to be located at
`/etc/borgsync/config`. Individual users can override these defaults in
`~/.config/borgsync/config`. An example configuration file is included in
[config.example](config.example).

The `install` script copies the example configuration to `/etc/borgsync/config`
but at the remote user and host must be set manually:

```bash
vi /etc/borgsync/config    # customize with your storage service user/host/etc
```

## Usage

Get started with `borgsync` by editing `/etc/borgsync/config` and setting
the `REMOTE_USER` and `REMOTE_HOST` variables to your storage service
user and host. For example, an `rsync.net` account for user `fm1872`
would have the following settings in `/etc/borgsync/config`:

```bash
REMOTE_USER="fm1872"
REMOTE_HOST="fm1872.rsync.net"
```

The defaults in `/etc/borgsync/config` should suffice for most users but
backup paths, exclusions, options, and more can be customized. Once these
are configured:

* Initialize a `borg` backup repository with the command `borgsync -b init`
* Create the default `borg` backup with the command `borgsync`

The `borgsync` usage message can be displayed with `borgsync -u`:

```
Usage: borgsync [-b init|check|create|delete|info|list|mount|umount]
                [-C config] [-c cmd] [-d dir] [-lLn] [-m mnt] [-U user]
                [-H host] [-qQruv] [-t default|full|home|logs] folder
Where:
	-b 'init' initializes a borg backup repository
	-b 'check' verifies the consistency of the borg backup repository
	-b 'create' creates a borg backup on remote storage
	   combine with '-t default|full|home|logs' (default: default)
	-b 'delete' deletes the borg backup repository on remote storage
	-b 'info' displays detailed information about the borg backup repository
	-b 'list' lists all archives in the borg backup repository
	-b 'mount' mounts the borg backup repository on /mnt/borg
	-b 'umount' unmounts the borg backup repository from /mnt/borg
	-C 'config' specifies the config file (default: /etc/borgsync/config)
	-c 'cmd' runs command 'cmd' on the remote storage host
	-d 'dir' specifies a borg backup directory (default: 'backups'
	-l indicates list the contents of the backup folder
	-L indicates recursively list the contents of the backup folder
	-m 'mnt' specifies the mount point for the borg repo (default: /mnt/borg)
	-n indicates perform a dry run, don't make any changes
	-q indicates see how much space your account uses with the quota/df commands
	-Q indicates see how much space your account uses with the quota/df/du commands
	-r indicates remove remote backup
	-v indicates verbose mode
	-U 'user' sets the remote storage user to 'user'
	-H 'host' sets the remote storage host to 'host'
	-u displays this usage message and exits

The 'folder' argument indicates the remote storage folder to sync/list/remove
Without arguments borgsync performs the default borg backup
```

## Automated scheduled backups

Automated periodic backups can be scheduled with `systemd` or `cron`

### Automated backups using cron

To automate daily remote backups with the `borgsync` command, verify that the `root` user can access the `rsync.net` account using `ssh` without being prompted. Copy the `$HOME/.config/borg/` folder to `/root/.config/borg/` and set the `BORG_PASSPHRASE` environment variable for `root`.

Configure a `cron` job for the `root` user with something like the following:

```
# Backup nightly at 2am
0 2 * * * /bin/bash -lc '/usr/local/bin/borgsync'

# Verify the backups once a month
40 17 23 * * /usr/local/bin/borgsync -b check
```

For further details on scheduling Borg backups with `cron`, see
[cron/README.md](cron/README.md) and the [example crontab entry](cron/crontab.in).

### Automated backups using systemd

Copy the example systemd [unit files](systemd/) to `/etc/systemd/system/`.

Issue the following commands to enable and start the systemd timers:

```bash
sudo systemctl enable borgsync-backup.timer
sudo systemctl enable borgsync-verify.timer

sudo systemctl start borgsync-backup.timer
sudo systemctl start borgsync-verify.timer
```

The included systemd files are set up using a daily schedule. If you want to
take backups more often than that you can change the `Timer` parameters
directly in the systemd timer files:

```bash
sudo systemctl edit borgsync-backup.timer
```

Here is an example where you run a backup 4 times
a day (every 6 hours). See the manual for systemd.timer for more information on
the `OnCalendar` format.

```
    [Timer]
    OnCalendar=00/6:00
    RandomizedDelaySec=0
```

The output will land in

```
/etc/systemd/system/borgsync-backup.timer.d/override.conf
```

You can just drop files in the directory directly too, without editing via
systemctl. This is better suited for configuration management systems.

You can view the backup logs with:

```bash
journalctl -xu borgsync-backup
journalctl -xu borgsync-verify
```

If you want to run the tasks manually outside the timers you can just start them like usual
services:

```bash
sudo systemctl start borgsync-backup
sudo systemctl start borgsync-verify
```

## Examples

### Initialize a backup repository

```bash
borgsync -b init
```

### Create a remote backup

```bash
borgsync -b create
```

### Get info about a remote backup repository

```bash
borgsync -b info
borgsync -b list
```

### Mount a remote backup repository on /mnt/borg

```bash
borgsync -b mount
```

### Upload a folder to rsync.net

```bash
borgsync Videos
```

### Verify backups

```bash
    borgsync -b check
```

### Run a command on rsync.net

For example, to execute the `ls` command on your configured `rsync.net` account:

```bash
    borgsync -c 'ls */backups'
```

Any supported command can be run similarly:

```bash
    borgsync -c '<command-name> <arguments>'
```

## Borg server preparation

***Server-side configuration is not necessary if using rsync.net***

Install borg and then:

```bash
sudo adduser --system --group --shell /bin/bash borg
sudo mkdir /srv/borg
sudo chown borg. /srv/borg
sudo chmod 755 /srv/borg
```

Generate the needed passwordless ssh-keys as root (the user you run the backup as) on the client

```bash
sudo ssh-keygen
```

Copy the content of the generated public key from `/root/.ssh/<key>.pub` to `/home/borg/.ssh/authorized_keys` on the server, with
some restrictions so it looks something like this:

```
    command="borg serve --restrict-to-path /srv/borg/<hostname>",no-pty,no-agent-forwarding,no-port-forwarding,no-X11-forwarding,no-user-rc ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDeCInOLjv0hgzI0u1b/p4yYnCEV5n89HIXF1hrLor+ZQ7lSUii21tpn47Aw8RJJAjfDCwCdQ27MXjpzNelBf4KrlAiN1K3FcnGGIiE3XFNoj4LW7oAjzjFgOKC/ea/hXaCI6E8M/Pn5+MhdNN1ZsWNm/9Zp0+jza+l74DQgOE33XhSBjckUchqtBci7BqoCejy2lVvboFA231mSEpPValcKmG2qaNphAkCgAPjtDOx3V6DGQ8e7jfA2McQYxfju6HlpWPUx/li6VJhRa5huczfJ3J/sdfu123s/lgTW4rG5QNng1vt1FOIZ/TkaEsPt2wzD2Qxdwo70qVts3hrd+r root@client
```
