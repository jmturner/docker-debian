# hub.docker.com/tiredofit/debian

# Introduction

Dockerfile to build an [debian](https://www.debian.org/) container image.

* Currently tracking Jessie (8)
* [s6 overlay](https://github.com/just-containers/s6-overlay) enabled for PID 1 Init capabilities
* [zabbix-agent](https://zabbix.org) based on TRUNK compiled for individual container monitoring.
* Cron installed along with other tools (curl, less, logrotate, nano, vim) for easier management.

# Authors

- [Dave Conroy](dave at tiredofit dot ca) [https://github.com/tiredofit]

# Table of Contents

- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Quick Start](#quick-start)
- [Configuration](#configuration)
    - [Data Volumes](#data-volumes)
    - [Environment Variables](#environmentvariables)   
    - [Networking](#networking)
- [Maintenance](#maintenance)
    - [Shell Access](#shell-access)
   - [References](#references)

# Prerequisites

No prequisites required

# Installation

Automated builds of the image are available on [Docker Hub](https://hub.docker.com/tiredofit/debian) and 
is the recommended method of installation.


```bash
docker pull tiredofit/debian:(imagetag)
```

The following image tags are available:

* `latest` - Debian Stretch - 9
* `stretch:latest` - Debian Stretch - 9
* `jessie:latest` - Debian Jessie - 8
* `wheezy:latest` - Debian Wheezy - 7


# Quick Start

Utilize this image as a base for further builds. By default it does not start the S6 Overlay system, but 
Bash. Please visit the [s6 overlay repository](https://github.com/just-containers/s6-overlay) for 
instructions on how to enable the S6 Init system when using this base or look at some of my other images 
which use this as a base.

# Configuration

### Data-Volumes
The following directories are used for configuration and can be mapped for persistent storage.

| Directory                           | Description                 |
|-------------------------------------|-----------------------------|
| `/etc/zabbix/zabbix_agentd.conf.d/` | Root tinc Directory         |
| `/assets/cron-custom`               | Drop Custom Crontabs here |


### Environment Variables

Below is the complete list of available options that can be used to customize your installation.

| Parameter         | Description                                                    |
|-------------------|----------------------------------------------------------------|
| `DEBUG_MODE`      | Enable Debug Mode - Default: `FALSE`                            |
| `DEBUG_SMTP`      | Setup Mail Catch all on port 1025 (SMTP) and 8025 (HTTP) - Default: `FALSE`	 |
| `ENABLE_CRON`     | Enable Cron - Default: `TRUE`                                   |
| `ENABLE_SMTP`     | Enable SMTP services - Default: `TRUE`						|
| `ENABLE_ZABBIX`   | Enable Zabbix Agent - Default: `TRUE`                           |
| `TIMEZONE`        | Set Timezone - Default: `America/Vancouver`                     |

If you wish to have this send mail, set `ENABLE_SMTP=TRUE` and configure the following environment variables. See the [MSMTP Configuration Options](http://msmtp.sourceforge.net/doc/msmtp.html) for further information on options to configure MSMTP

| Parameter         | Description                                                    |
|-------------------|----------------------------------------------------------------|
| `SMTP_HOST`      | Hostname of SMTP Server - Default: `postfix-relay`                            |
| `SMTP_PORT`      | Port of SMTP Server - Default: `25`                            |
| `SMTP_DOMAIN`     | HELO Domain - Default: `docker`                                   |
| `SMTP_MAILDOMAIN`     | Mail Domain From - Default: `example.org`						|
| `SMTP_AUTHENTICATION`     | SMTP Authentication - Default: `none`                                   |
| `SMTP_USER`     | Enable SMTP services - Default: `user`						|
| `SMTP_PASS`   | Enable Zabbix Agent - Default: `password`                           |
| `SMTP_TLS`        | Use TLS - Default: `off`                     |
| `SMTP_STARTTLS`   | Start TLS from within Dession - Default: `off` |
| `SMTP_TLSCERTCHECK` | Check remote certificate - Default: `off` |

See The [Official Zabbix Agent Documentation](https://www.zabbix.com/documentation/2.2/manual/appendix/config/zabbix_agentd) for information about the following Zabbix values

| Zabbix Parameters | Description                                                    |
|-------------------|----------------------------------------------------------------|
| `ZABBIX_LOGFILE` | Logfile Location - Default: `/var/log/zabbix/zabbix_agentd.log` |
| `ZABBIX_LOGFILESIZE` | Logfile Size - Default: `1` |
| `ZABBIX_DEBUGLEVEL` | Debug Level - Default: `1` |
| `ZABBIX_REMOTECOMMANDS` | Enable Remote Commands (0/1) - Default: `1` |
| `ZABBIX_REMOTECOMMANDS_LOG` | Enable Remote Commands Log (0/1)| - Default: `1` |
| `ZABBIX_SERVER` | Allow connections from Zabbix Server IP - Default: `0.0.0.0/0` |
| `ZABBIX_LISTEN_PORT` | Zabbix Agent Listening Port - Default: `10050` |
| `ZABBIX_LISTEN_IP` | Zabbix Agent Listening IP - Default: `0.0.0.0` |
| `ZABBIX_START_AGENTS` | How many Zabbix Agents to Start - Default: `3 | 
| `ZABBIX_SERVER_ACTIVE` | Server for Active Checks - Default: `zabbix-proxy` |
| `ZABBIX_HOSTNAME` | Container hostname to report to server - Default: `docker` |
| `ZABBIX_REFRESH_ACTIVE_CHECKS` | Seconds to refresh Active Checks - Default: `120` |
| `ZABBIX_BUFFER_SEND` | Buffer Send - Default: `5` |
| `ZABBIX_BUFFER_SIZE` | Buffer Size - Default: `100` |
| `ZABBIX_MAXLINES_SECOND` | Max Lines Per Second - Default: `20` |
| `ZABBIX_ALLOW_ROOT` | Allow running as root - Default: `1` |
| `ZABBIX_USER` | Zabbix user to start as - Default: `zabbix` |


### Networking


The following ports are exposed.

| Port      | Description  |
|-----------|--------------|
| `1025`    | `DEBUG_MODE` & `DEBUG_SMTP` SMTP Catcher |
| `8025`    | `DEBUG_MODE` & `DEBUG_SMTP` SMTP HTTP Viewer |
| `10050`   | Zabbix Agent |


# Debug Mode

When using this as a base image, create statements in your startup scripts to check for existence of `DEBUG_MODE=TRUE` and set various parameters in your applications to output more detail, enable debugging modes, and so on. In this base image it does the following:

* Sets zabbix-agent to output logs in verbosity
* Enables MailHog mailcatcher, which replaces `/usr/sbin/sendmail` with it's own catchall executible. It also opens port `1025` for SMTP trapping, and you can view the messages it's trapped at port `8025`

# Maintenance
#### Shell Access

For debugging and maintenance purposes you may want access the containers shell. 

```bash
docker exec -it (whatever your container name is e.g. debian) bash
```

# References

* https://www.debian.org
* https://www.zabbix.org
* https://github.com/just-containers/s6-overlay



