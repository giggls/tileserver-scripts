# Replication and tile expire scripts from German tileserver

This repository contains the scripts which are used on the German
tileservers for database replication using pyosmium-get-changes and
osm2pgsql. These scripts will also trigger the tile expire mechanism from
the output created by osm2pgsql.

The machines are currently running Debian GNU/Linux 10 (buster) and most of
the software running there is available directly from the distribution.

Exceptions are:

* osml10n https://github.com/giggls/mapnik-german-l10n
* tirex https://github.com/openstreetmap/tirex
* libapache2-mod-tile https://github.com/openstreetmap/mod_tile/

## Provided scripts

### whichdiff.pl
A perl script to determine the replication number for the first sequence.
This is needed for bootstraping the replication process.

### expiremeta.pl
A perl script which will mark all meta-tiles read from osm2pgsql tile expire
file for re-rendering.

### expirehrb
A shell script which will mark our special small area Sorbian meta-tiles for re-rendering
based on the ones marked by expiremeta.pl.

### osm-replicate.service
Systemd service-file for running replicate-loop.sh

### osm-replicate.timer
Systemd timer-file for calling osm-replicate.service

### replicate-loop.sh

A shell script which fetches diff-files from OSM Planet server using pyosmium-get-changes
and feed them to osm2pgsql afterwards. It will also call tile expire scripts
if requested. This script is intended to be called from a cronjob or
(recommend) from the provided systemd service and timer files. If you
intend to use this for your own tileserver you will likely need to slightly adapt
this to your own requirements.

### check_osmdb_up2date.py

Monitoring Plugin for Icinga to see if database is up to date.
