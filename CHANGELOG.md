# Changelog

## Unreleased

### ⚡️ Added

### 🐞 Fixed

## v1.0.1r3

### ⚡️ Added

- Add `-a archive` arguments to specify a borg repository archive name
- Add `-B` option to indicate update `borgsync` and `borg`

### 🐞 Fixed

- Preserve existing configs during install/update

## v1.0.1r2

### ⚡️ Added

- Initial release version
- Support for `rsync.net` and `BorgBase` accounts
- Support for multiple configuration files and repositories
- Support for `borg` commands `init|check|create|delete|extract|info|list|mount|umount`
- Support for `ssh` execution of remote commands on configured services
- Merged `bu2rsync` upload/management of an `rsync.net` account with `borgwrapper`

### 🐞 Fixed

- Use `BORG_REPO` rather than `user@host:/...` in all `borg` commands

## Prerelease History

* `Ronald Record, Sun Jan 28 09:56:52 2024 -0800 33a86ce`:
    Add release management and version
* `Ronald Record, Sun Jan 28 09:23:13 2024 -0800 eec5e5c`:
    Use BORG_REPO rather than user/host/myhost/bdir with all borg commands
* `Ronald Record, Sun Jan 28 08:46:14 2024 -0800 66a6612`:
    Add option to install borgsync and configs in user home
* `Ronald Record, Sun Jan 28 08:07:28 2024 -0800 3847581`:
    Remove redirects in uninstall
* `Ronald Record, Sun Jan 28 07:56:20 2024 -0800 eafd21a`:
    Merge pull request #1 from doctorfree/develop
* `Ronald Record, Sun Jan 28 07:48:35 2024 -0800 ce773c6`:
    Move base config to /etc/borgsync/base, add config specification readme
* `Ronald Record, Sat Jan 27 20:47:15 2024 -0800 519ea8a`:
    Add custom configurations section to readme
* `Ronald Record, Sat Jan 27 19:35:07 2024 -0800 e5e17af`:
    Restrict backups to a single filesystem and exclude using .nobackup file
* `Ronald Record, Sat Jan 27 19:24:20 2024 -0800 7da7ff6`:
    Add support for BorgBase, multiple configs with overrides for base config
* `Ronald Record, Sat Jan 27 16:35:58 2024 -0800 3096a33`:
    First pass at multiple configs in /etc/borgsync, one for each backup type and extensible
* `Ronald Record, Sat Jan 27 13:33:09 2024 -0800 8afae0e`:
    Extend support for borgbase and ssh, improve readme, add uninstall
* `Ronald Record, Sat Jan 27 11:31:34 2024 -0800 5853612`:
    Add support for macos, install vorta
* `Ronald Record, Sat Jan 27 09:43:17 2024 -0800 bf86abb`:
    Exclude /var/lib/docker/volumes
* `Ronald Record, Fri Jan 26 16:36:01 2024 -0800 297fa6a`:
    Add Configure comments and alternate keywords to borgsync
* `Ronald Record, Fri Jan 26 16:15:47 2024 -0800 8dd37a1`:
    Add crontab.in and expanded README for cron
* `Ronald Record, Fri Jan 26 15:53:58 2024 -0800 0f04740`:
    Rework systemd unit files, update README examples
* `Ronald Record, Fri Jan 26 15:24:19 2024 -0800 622e123`:
    Add /home/*/.local/share/nvim-* `to excludes
* `Ronald Record, Fri Jan 26 15:18:20 2024 -0800 ed68f1a`:
    Add /home/*/.docker/desktop/vms to excludes
* `Ronald Record, Fri Jan 26 13:48:30 2024 -0800 6a0c012`:
    Expand config to cover all backup levels, remove borg-create
* `Ronald Record, Fri Jan 26 12:35:15 2024 -0800 9808a4b`:
    Integrate remaining config arguments into borgsync
* `Ronald Record, Fri Jan 26 11:47:46 2024 -0800 0adf4ad`:
    Integrate config settings in default borgsync backup, move bu2rsync to borgsync, support invocation as normal user
* `Ronald Record, Fri Jan 26 09:19:28 2024 -0800 71e519e`:
    Move Borg passphrase out of config to environment
* `Ronald Record, Fri Jan 26 08:25:01 2024 -0800 19d9d3c`:
    Add bu2rsync to install
* `Ronald Record, Fri Jan 26 08:17:05 2024 -0800 1e62631`:
    Add gitignore
* `Ronald Record, Thu Jan 25 08:36:05 2024 -0800 e991c50`:
    Add docker postgres backup compose.yml
* `Ronald Record, Thu Jan 25 08:31:13 2024 -0800 86e243b`:
    Add Immich backup/restore scripts
* `Ronald Record, Tue Jan 23 09:14:31 2024 -0800 f216fb6`:
    First pass at using config file in borg-create and bu2rsync
* `Ronald Record, Sun Jan 21 10:55:36 2024 -0800 d23f754`:
    Begin borgsync/bu2rsync integration
* `Ronald Record, Sat Jan 20 09:05:55 2024 -0800 2103ee8`:
    Add borg remote install location to config
* `Ronald Record, Fri Jan 19 07:46:58 2024 -0800 0a4743b`:
    Simplify install process with install script
* `Ronald Record, Thu Jan 18 16:47:17 2024 -0800 4fdd071`:
    Restructure, add bu2rsync and borg-create, customize config
* `Ronald Record, Thu Jan 18 15:42:47 2024 -0800 e2d495e`:
    Initial conversion from borgwrapper to borgsync
* `hkbakke, Sat Jan 11 18:11:25 2020 +0100 a14117c`:
    Update README.md
* `hk, Sat Jan 11 12:40:55 2020 +0100 2fd83ac`:
    Add support for hourly backups
* `hk, Tue Sep 24 14:46:39 2019 +0200 9d2c564`:
    Fix dry-run not working with --stats
* `hk, Tue Sep 17 11:30:32 2019 +0200 8035d70`:
    Update version
* `hk, Tue Sep 17 11:27:47 2019 +0200 6cc0476`:
    Add encryption parameter to init
* `hk, Sat Mar 17 16:02:17 2018 +0100 3417e60`:
    Remove checkpont pruning option as this is done automatically by borg in 1.1.0 and higher
* `hk, Wed Jan 31 19:46:38 2018 +0100 66379d3`:
    Bump version
* `hk, Wed Jan 31 19:45:46 2018 +0100 1304280`:
    Minor refactor
* `hk, Wed Jan 31 19:37:34 2018 +0100 67db0ab`:
    Add config option to set BORG_RSH
* `hk, Tue Jan 30 23:51:46 2018 +0100 6e3f74e`:
    Remove .sh from config and borgwrapper
* `hk, Fri Sep 29 14:55:05 2017 +0200 48de6e8`:
    Move niceness configuration from borgwrapper to systemd unit file to reduce complexity
* `hk, Mon Sep 18 20:39:32 2017 +0200 b1fee78`:
    Remove STATUSDIR from config.sh as it is no longer in use
* `hk, Mon Sep 18 14:10:00 2017 +0200 2e64570`:
    Bump version
* `hk, Mon Sep 18 14:08:22 2017 +0200 1e5c912`:
    Remove builtin status keeping and dedicated nagios monitoring script
* `hk, Sat Sep 16 15:58:55 2017 +0200 cfa1fa8`:
    Update systemd description and bump version
* `hk, Sat Sep 16 15:32:21 2017 +0200 ec691eb`:
    Add systemd configuration
* `hk, Sat Sep 16 14:03:35 2017 +0200 57049ac`:
    Autodetect nice path
* `hk, Sat Sep 16 13:57:18 2017 +0200 53f91f0`:
    Minor code refactor
* `hk, Sat Sep 16 12:58:20 2017 +0200 03c40c7`:
    Change typo in config.sh comment
* `hk, Sat Sep 16 12:57:25 2017 +0200 2f77b64`:
    Bump version
* `hk, Sat Sep 16 12:56:55 2017 +0200 45dee2b`:
    Implement nice support
* `hk, Sat Sep 16 11:47:14 2017 +0200 740621b`:
    Add dry-run support for delete-checkpoints
* `hk, Sat Sep 16 11:45:10 2017 +0200 04fdf1f`:
    Remove some leftovers from the pre and post hooks.
* `hkbakke, Fri Sep 15 14:17:45 2017 +0200 73d38c1`:
    Update version
* `hkbakke, Fri Sep 15 14:00:03 2017 +0200 eabe2ce`:
    Add nagios compatible check plugin
* `hkbakke, Thu Sep 7 13:54:19 2017 +0200 72f722e`:
    Create status file for external monitoring.
* `hk, Mon Jun 26 18:35:45 2017 +0200 5fd3dce`:
    Fix for silently ignore arguments after mode broke exec mode
* `hk, Mon Jun 26 18:17:54 2017 +0200 ffc3706`:
    Do not silently ignore arguments specified after mode
* `hk, Fri Jun 9 17:45:13 2017 +0200 88e10a9`:
    Remove unlock. It is dangerous and should not be easily available
* `hk, Fri Jun 9 13:42:22 2017 +0200 e87c0e5`:
    Rename BORG_BACKUP_ARGS to BORG_CREATE_ARGS for consistency
* `hk, Fri Jun 9 13:40:06 2017 +0200 a1b1528`:
    Add dry-run argument (-d)
* `hk, Fri Jun 9 12:59:45 2017 +0200 b2da1e1`:
    Add -V to usage
* `hk, Fri Jun 9 12:57:56 2017 +0200 8db895b`:
    Add delete-checkpoints
* `hk, Fri Jun 9 12:54:59 2017 +0200 e0fe671`:
    Output version with -V
* `hk, Fri Jun 9 12:50:33 2017 +0200 e627b2b`:
    Add delete-checkpoints mode until borg v1.1.0 is released with automatic checkpoint pruning
* `hk, Wed Jun 7 12:25:55 2017 +0200 353dfd0`:
    Fix typo
* `hk, Wed Jun 7 11:02:01 2017 +0200 af82ccf`:
    Cleanup lockfile on exit
* `hk, Wed Jun 7 10:56:51 2017 +0200 fe2cda0`:
    Autogenerate lockfiles based on BORG_REPO
* `hk, Wed Jun 7 10:23:37 2017 +0200 c6c34e3`:
    Allow binary prefixes in BWLIMIT
* `hk, Wed Jun 7 01:46:36 2017 +0200 8dbc167`:
    Minor syntax change
* `hk, Wed Jun 7 01:34:35 2017 +0200 8bb2a0e`:
    Add support for bandwith limitation
* `hk, Wed Jun 7 00:40:03 2017 +0200 ffdf749`:
    Add comment to LOCKFILE parameter
* `hk, Wed Jun 7 00:19:04 2017 +0200 88390f4`:
    Add note about non-standard SSH port
* `hk, Tue Jun 6 20:47:18 2017 +0200 10ba96f`:
    Remove --numeric-owner from create
* `hk, Mon Jun 5 15:23:44 2017 +0200 52b766e`:
    Remove stray comment
* `hk, Mon Jun 5 15:02:45 2017 +0200 d735f14`:
    Rework error handling and locking interaction
* `hk, Mon Jun 5 02:44:00 2017 +0200 213c235`:
    Fix behaviour when hooks are not set
* `hk, Mon Jun 5 02:29:38 2017 +0200 a48212d`:
    Add pre and post backup hooks. Enforce only one running instance.
* `hk, Sat May 20 15:13:28 2017 +0200 40883bc`:
    Added missing space (visual change only)
* `hk, Sat May 20 15:09:20 2017 +0200 c40dab7`:
    Make borg optional args configurable
* `hkbakke, Sat May 20 14:03:13 2017 +0200 a2d7964`:
    Output added and modified files when doing backups
* `hk, Mon Apr 3 08:42:31 2017 +0200 645e14c`:
    Add config file option. Fix #2.
* `hkbakke, Sat Mar 18 15:27:48 2017 +0100 bf91e51`:
    Update README
* `hkbakke, Sat Mar 18 15:22:23 2017 +0100 72ae920`:
    Output error message to stderr
* `hkbakke, Sat Mar 18 15:15:25 2017 +0100 d8f89ca`:
    Fix typo
* `hkbakke, Sat Mar 18 15:12:16 2017 +0100 33dca23`:
    Include link to configuration example (and script)
* `hkbakke, Sat Mar 18 13:23:57 2017 +0100 0f732f2`:
    Print current script name in usage
* `hkbakke, Sat Mar 18 13:22:00 2017 +0100 61e25f4`:
    Add new modes to usage
* `hkbakke, Sat Mar 18 13:20:41 2017 +0100 eed54f1`:
    Minor visual change
* `hkbakke, Sat Mar 18 13:06:52 2017 +0100 287ffce`:
    Add a couple more examples
* `hkbakke, Sat Mar 18 13:04:20 2017 +0100 ef23ea1`:
    Fix a space too many
* `hkbakke, Sat Mar 18 12:53:07 2017 +0100 92b56a2`:
    Fix syntax
* `hk, Sat Mar 18 12:49:26 2017 +0100 967a4bb`:
    Add init and exec modes
* `hkbakke, Sat Mar 18 11:23:06 2017 +0100 fbf8b16`:
    Update README.md
* `hkbakke, Sat Mar 18 11:22:11 2017 +0100 751b02d`:
    Insert hostname automatically
* `root, Fri Jan 20 14:02:48 2017 +0100 2b17229`:
    Fix some potential issues with wildcard
* `root, Fri Jan 20 11:57:43 2017 +0100 ffd4d55`:
    Minor visual cleanup
* `hkbakke, Sat Dec 24 13:40:39 2016 +0100 7798ccd`:
    Use borgs builtin hostname placeholder
* `hkbakke, Thu Dec 22 23:56:38 2016 +0100 d8153d4`:
    Clean up some comments
* `hkbakke, Thu Dec 22 20:07:24 2016 +0100 f3f4b4f`:
    Increase logging for borg verify
* `hkbakke, Thu Dec 22 18:10:50 2016 +0100 2ef03d7`:
    Replace tabs with spaces
* `hkbakke, Thu Dec 22 17:59:19 2016 +0100 e0ca054`:
    Add initial source
* `hkbakke, Thu Dec 22 17:54:59 2016 +0100 f34788e`:
    Update README.md
* `hkbakke, Thu Dec 22 17:45:24 2016 +0100 f6a3720`:
    Initial commit
