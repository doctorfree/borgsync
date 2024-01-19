# borgsync

Wrapper to simplify backups with `borgbackup` and management of an `rsync.net` account.

## Installation

### Install borg

A convenience script, [install_borg](install_borg), is provided to install `borg`.

### Install borgsync commands

Copy the [borgsync command script](bin/borgsync) and
the [borg backup creation script](bin/borg-create)
somewhere in your execution path:

```bash
sudo cp bin/borgsync /usr/local/bin/borgsync
sudo cp bin/borg-create /usr/local/bin/borg-create
sudo chown root:root /usr/local/bin/borgsync /usr/local/bin/borg-create
sudo chmod 750 /usr/local/bin/borgsync /usr/local/bin/borg-create
```

## Configuration

By default `borgsync` expects the configuration to be located at `/etc/borgsync/config`.
An example configuration file is included in [config.example](config.example).
Ensure restrictive permissions on this file as it exposes the passphrase.

```bash
cp config.example config
vi config    # customize with your rsync.net user/host, passphrase, etc
chmod 600 config
sudo cp config /etc/borgsync/config
sudo chown root:root /etc/borgsync/config
sudo chmod 600 /etc/borgsync/config
```

## Scheduling
### systemd

Copy the example systemd [unit files](systemd/) to `/etc/systemd/system/`. Then for each
configuration file in `/etc/borgsync/<config_name>` do:

```bash
sudo systemctl enable borgsync-backup@<config_name>.timer
sudo systemctl enable borgsync-verify@<config_name>.timer

sudo systemctl start borgsync-backup@<config_name>.timer
sudo systemctl start borgsync-verify@<config_name>.timer
```

The included systemd files are set up using a daily schedule. If you want to
take backups more often than that you can either change the `Timer` parameters
directly in the systemd timer files, or if you only want to override them for
some of the backups you can add per-config overrides by using

```bash
sudo systemctl edit borgsync-backup@<config>.timer
```

and add the wanted overrides. Here is an example where you run a backup 4 times
a day (every 6 hours). See the manual for systemd.timer for more information on
the `OnCalendar` format.

```
    [Timer]
    OnCalendar=00/6:00
    RandomizedDelaySec=0
```

The output will land in

```
/etc/systemd/system/borgsync-backup@<config>.timer.d/override.conf
```

You can just drop files in the directory directly too, without editing via
systemctl. This is better suited for configuration management systems.

You can view the backup logs with:

```bash
journalctl -xu borgsync-backup@<config_name>
journalctl -xu borgsync-verify@<config_name>
```

If you want to run the tasks manually outside the timers you can just start them like usual
services:

```bash
sudo systemctl start borgsync-backup@<config_name>
sudo systemctl start borgsync-verify@<config_name>
```

### Cron (use only if systemd is not available)

```
    # Run the backup daily
    23 1 * * * /usr/local/bin/borgsync backup

    # Verify the backups once a month
    40 17 23 * * /usr/local/bin/borgsync verify
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

## Usage
### Initialize backup repo

```bash
borgsync init
```

### Backup

```bash
    borgsync backup
```

### Verify backups

```bash
    borgsync verify
```

### Run other borg commands
#### Wrapped and easy

Use `exec <borg arguments>`. `BORG_REPO` is exported to the environment so use `::` when the repo
argument is required.

Examples:

```bash
    borgsync exec mount :: /mnt
    borgsync exec list
```

#### Borg directly

Run in subshell if you do not want the passphrase stored in the current shell after the command have exited.

Examples:

```bash
    (. /etc/borgsync/config; export BORG_PASSPHRASE; borg mount "$BORG_REPO" /mnt)
    (. /etc/borgsync/config; export BORG_PASSPHRASE; borg list "$BORG_REPO")
```
