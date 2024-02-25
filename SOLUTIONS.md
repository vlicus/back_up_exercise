# Backups exercise

## Backups types

1. Full: The most basic and comprehensive backup method, where all data is sent to another location.

2. Incremental: Backups all files that have changed since the last backup ocurred. Incremental backups are made possible by enabling the server's binary log, which the server uses to record data changes.

3. Differential: Backups only copies of all files that have changed since the last full backup.

## CLI tools to perform backups (full and incremental) and restores for the databases listed below:

### MySQL

Most backup strategies start with a complete (full) backup of the MySQL server, from which you can restore all databases and tables. After you have created a full backup, you might perform incremental backups (which are smaller and faster) for the next several backup tasks. You then make a full backup periodically to begin the cycle again.

1. Full MySQL backup

* 
* 

2. Incremental MySQL backup

* 
* 

### MongoDB

## Automate the backups from section 2 using bash scripting (either MySQL or MongoDB).

## Schedule the scripts from section 3 using cron job to perform a full backup once a week and an incremental backup every day.