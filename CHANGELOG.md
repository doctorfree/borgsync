# Changelog

## Unreleased

### ⚡️ Added

### 🐞 Fixed

## v1.0.1r2

### ⚡️ Added

- Initial release version
- Support for `rsync.net` and `BorgBase` accounts
- Support for multiple configuration files and repositories
- Support for `borg` commands `init|check|create|delete|extract|info|list|mount|umount`
- Support for `ssh` execution of remote commands on configured services

### 🐞 Fixed

- Use `BORG_REPO` rather than `user@host:/...` in all `borg` commands
