# Installation Instructions

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](http://doctoc.herokuapp.com/)*

- [Requirements](#requirements)
- [Quick and Easy Install Instructions](#quick-and-easy-install-instructions)
  - [Extracting](#extracting)
  - [MySQL DB](#mysql-db)
  - [Webserver Setup](#webserver-setup)
  - [Next Steps](#next-steps)
  - [Notes](#notes)
- [Detailed Install Instructions](#detailed-install-instructions)
  - [Extracting](#extracting-1)
  - [MySQL DB](#mysql-db-1)
  - [Webserver Setup](#webserver-setup-1)
  - [Next Steps](#next-steps-1)
  - [Notes](#notes-1)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

- - -

## Requirements

*	python
*	latest exabgp (by default in /opt/exabgp)
*	webserver w/ PHP and pear
	*	`pear install Image_GraphViz`

## Quick and Easy Install Instructions

These instructions will get you up and running quickly in a closed development environment, but may not be the most secure setup.

### Extracting

GIXLG expects itself to be installed under `/opt/gixlg` and exabgp to be under `/opt/exabgp`.

	mkdir -p /opt
	git clone https://github.com/dpiekacz/gixlg.git /opt/gixlg
	git clone https://github.com/Exa-Networks/exabgp.git /opt/exabgp

### MySQL DB

To get up and running in under 5 seconds, import the `sql/gixlg.sql` script, it will drop/create a database named gixlg, and setup a mysql username/password (gixlg/gixlg) with credentials to work with this database.

### Webserver Setup

GIXLG provides php scripts (in the `htdocs/` directory) which should be run though your webserver of choice (Apache or NGINX is supported). Example configuration files for Apache and NGINX are located in the `webconfigs/`.

You will need to copy the config file from the samples provided.

	cd htdocs
	cp gixlg-cfg_sample.php gixlg-cfg.php

### Next Steps

After you have the files extracted, the MySQL DB created and populated, and the webserver set up, you need to start the backend. The files for this are located in `exabgp/`.

Modify your exabgp.conf to include the routers you'd like to peer with. You will see that our own collector script is called.

To run the backend:

	cd exabgp/
	sh exabgp.sh; tail -n 40 -f log_exabgp

exabgp should fork into the background and then begin tailing the logfile and show you any log messages which may indicate a failure.

### Notes

Exabgp needs to be run as root at the moment.

- - -

## Detailed Install Instructions

These instructions will get you up and running in a custom environment. Each step along the way is described in detail to aid you in making secure choices for your environment.

### Extracting

GIXLG by default expects itself to be installed under /opt/gixlg, and a copy of exabgp to be under /opt/exabgp.

	mkdir -p /opt
	git clone https://github.com/dpiekacz/gixlg.git /opt/gixlg
	git clone https://github.com/Exa-Networks/exabgp.git /opt/exabgp

If you would like to install to another location, you will need to modify `exabgp/exabgp.conf` to specify a different path to to collector.py, as well as the last line in `exabgp/exabgp.sh` to specify a different path to the config file.

### MySQL DB

Begin by examining `sql/gixlg.sql`, if you desire a custom setup, you will need to modify the first few lines of the SQL dump:

	DROP DATABASE IF EXISTS `gixlg`;
	CREATE DATABASE `gixlg`;

	GRANT ALL ON gixlg.* TO 'gixlg'@'localhost' IDENTIFIED BY 'gixlg';
	FLUSH PRIVILEGES;

To set a different password change the IDENTIFIED BY section. If you don't use default credentials, you will also need to modify `exabgp/collector.py` and `htdocs/gixlg-cfg.php` (once you create it from the sample)

### Webserver Setup

GIXLG provides php scripts (in the `htdocs/` directory) which should be run though your webserver of choice (Apache or NGINX is supported). Example configuration files for Apache and NGINX are located in the `webconfigs/`.

You will need to copy the config file from the sample provided. If you changed the mysql credentials, you will need to make the config file here match those.

	cd htdocs
	cp gixlg-cfg_sample.php gixlg-cfg.php

### Next Steps

After you have the files extracted, the MySQL DB created and populated, and the webserver set up, you need to start the backend. The files for this are located in `exabgp/`.

Modify your exabgp.conf to include the routers you'd like to peer with. You will see that our own collector script is called.

To run the backend:

	cd exabgp/
	sh exabgp.sh; tail -n 40 -f log_exabgp

exabgp should fork into the background and then begin tailing the logfile and show you any log messages which may indicate a failure.

### Notes

Exabgp needs to be run as root at the moment.

