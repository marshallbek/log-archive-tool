# log-archive

A simple Bash tool that compresses a log directory into a timestamped `.tar.gz` archive and optionally syncs it to a remote server via `rsync` over SSH.

## Features

- Compresses any log directory into a timestamped `.tar.gz`
- Stores archives locally under `~/log_archives/`
- Logs every archive operation (success or failure) to `~/log_archives/archive.log`
- Optional: syncs the archive to a remote server over SSH

## Installation

```bash
chmod +x log-archive
sudo mv log-archive /usr/local/bin/log-archive
```

## Usage

```bash
log-archive <log-directory>
```

**Example:**

```bash
log-archive /var/log
```

Some system log directories (e.g. `/var/log`) require root access:

```bash
sudo log-archive /var/log
```

## Output

Archives are saved to `~/log_archives/` with the naming format:

```
logs_archive_YYYYMMDD_HHMMSS.tar.gz
```

Each run also appends a record to `~/log_archives/archive.log`:

```
[2026-04-09 14:32:01] SUCCESS | source=/var/log | archive=logs_archive_20260409_143201.tar.gz | size=4.2M
```

## Remote Sync (Optional)

To enable syncing the archive to a remote server, edit the constants at the top of the script:

```bash
REMOTE_USER="logarchiver"                 # remote user
REMOTE_HOST="192.168.1.100"              # remote host IP or hostname
REMOTE_DIR="/backups/log_archives"       # destination directory on remote
SSH_KEY="~/.ssh/id_ed25519_log-archiver" # SSH key for authentication
```

Then set `REMOTE_ENABLED=true` in the script to activate the sync step.

Generate a dedicated SSH key if needed:

```bash
ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519_log-archiver
```

## Requirements

- `bash` 4+
- `tar` with gzip support
- `rsync` 3.2.3+ (only required for remote sync)
- `ssh` (only required for remote sync)
