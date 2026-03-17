# `systemd`
- [Reference](#reference)
- [Description](#description)
- [Configuration](#configuration)
- [Basic Usage](#basic-usage)
- [`journalctl`](#journalctl)
<hr>

### Reference
- [systemd.io](https://systemd.io/)
<hr>

### Description
`systemd` is the standard system and service manager for modern Linux distributions. It is the first process that starts after the kernel boots (PID 1) and acts as the parent to all other processes. If offers fast boot times through aggressive parallelization and dependency-based service management.

<hr>

### Configuration
Creating a custom `systemd` service file is the standard way to ensure your application starts automatically, restarts on failure, and manages its own logging.

These files are typically stored in `/etc/systemd/system/` and end with the `.service` extension.

Below is an example for a web applcation located at `/opt/myapp`:
```TOML
[Unit]
Description=Custom Web Application
After=network.target mysql.service

[Service]
Type=simple
User=webapp-user
Group=webapp-group
WorkingDirectory=/opt/myapp
ExecStart=/usr/bin/python3 /opt/myapp/app.py
Restart=always
RestartSec=5
Environment=NODE_ENV=production PORT=8080

[Install]
WantedBy=multi-user.target
```
#### Breakdown
1. `[Unit]`: Defines metadata and dependencies
    - `Description`: Human readable name shown in `systemctl status`
    - `After`: Ensures teh app doesn't try to start until the network and db are ready
2. `[Service]`: Defines how the application actually runs
    - `Type=simple`: The most common type; assumes the process started by `ExecStart` is the main process
    - `User/Group`: For security, never run your app as `root`
    - `ExecStart`: The full path to the exectuable and its arguments
    - `Restart=always`: Automatically restarts the app if it crashes or the process is killed
    - `Environment`: Sets environment variables directly within the service context
3. `[Install]`: Defines when the service should be activated
    - `WantedBy=multi-user.target`: Standard "runlevel" for a typical server. It ensures that when you `enable` the service, it starts during the normal boot sequence

#### Deploy
Once you have created your file (e.g. `/etc/systemd/system/myapp.service`):
```bash
# reload the daemon
sudo systemctl daemon-reload

# start and enable
sudeo systemctl enable --now myapp

# check for errors
sudo journalctl -u myapp -f
```

<hr>

### Basic Usage
The primary tool used to interact with `systemd` is `systemctl`. It allows you to manage services (units), check system status, and modify boot behavior.

```bash
systemctl [command] [unit]
```

#### Common Management Commands
- `start`: Starts a service immediately
- `stop`: Stops a running service
- `restart`: Stops then starts a service
- `status`: Shows the current state, runtime data, and recent log lines of a service
- `enable`: Configure a service to start automatically at boot
- `disable`: Prevents a service from starting at boot

#### Common Options & Unit Types
While `service` files are the most common, `systemd` manages verious "unit" types identified by their suffixes:
- `.service`: For daemon applications (e.g., `apache2.service`)
- `.timer`: For scheduling tasks (similar to cron)
- `.mount`: For managing file system mount points
- `.target`: For grouping units

#### Useful Flags:
- `--now`: Used with `enable` or `disable` to start / stop the service at the same time.
    - Example: `systemctl enable --now nginx`
- `--failed`: Lists all units that are in a failed state
- `--type=<type>`: Filters units by type
    - Example: `systemctl list-units --type=service`
- `-H <user@host>`: Allows you to manage `systemd` on a remote machine over SSH
    - Example: `systemctl -H user@host status docker`

<hr>

### `journalctl`
`systemd` includes its own logging system called **Journal**. Unlike traditional text logs in `/var/log`, the journal is binary formatted, making it faster to search and filter.

```bash
# view all logs
journalctl

# view logs for a specific service
journalctl -u nginx.service
```

#### Filtering Logs
- `-f`: "Follow" mode; live updates
- `-b`: Show logs only from the current boot
- `-p <level>`: Filter by priority (e.g. `err`, `warning`, `info`)
- `--since <time window>`: Filter logs by a time window
    - Example: `journalctl -u app.service --since "5 minutes ago"`
