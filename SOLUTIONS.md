# Backups exercise

## Backups types

1. Full: The most basic and comprehensive backup method, where all data is sent to another location.

2. Incremental: Backups all files that have changed since the last backup ocurred. Incremental backups are made possible by enabling the server's binary log, which the server uses to record data changes.

3. Differential: Backups only copies of all files that have changed since the last full backup.

## CLI tools to perform backups (full and incremental) and restores for the databases listed below:

### MySQL

Most backup strategies start with a complete (full) backup of the MySQL server, from which you can restore all databases and tables. After you have created a full backup, you might perform incremental backups (which are smaller and faster) for the next several backup tasks. You then make a full backup periodically to begin the cycle again.

1. Full MySQL backup

To perform a full backup of a MySQL database, it can be used `mysqldump` command, which is a MySQL utility for creating logical backups. It will be use as an example a MySQL database named "example"

```
mysqldump -u your_username -p your_password example > example_backup.sql
```

Where:
`your_username` must be your MySQL username.
`your_password` must be your MySQL password (empty if there is no password configured).
`example` is the name of your MySQL database.
`example_backup.sql` would be the name for your backup file.

Once you run this command, it will prompt you for the MySQL user password and after the password has been provided, it will create a SQL dump file `example_backup.sql` containing the complete structure of the specified MySQL database (example).

If you are running this command on the same machine where MySQL is installed, you might not need to provide `-u`and `-p` options and the command would look like this:

```
mysqldump example > example_backup.sql
```

This assumes that your MySQL user has the necessary privileges to access and dump the specified database (example).

A good practice will include the use of different flags that will be mentioned below:

`--single-transaction`: This option ensures that a consisten snapshot of the database is taken, using a single transaction. This is particulary useful for InnoDB tables, as it avoids locking the tables during the backup.

If binary loggin is enabled:

`--flush-logs`: This option causes the server to flush the logs after the backup, ensuring that the binary log files contain the events from the backup.

`--master-data=2`: This option includes the binary log coordinates as a comment at the end of the dump file. The value "2" specifies that it should incluse the file name and position. This information is useful for point-in-time recovery.

The command then would look like this:

```
mysqldump -u your_username -p your_password --single-transaction --flush-logs --master-data=2 example > example_backup.sql
```

Other graphical user interfaces and third-party tools such as phpMyAdmin or MySQL Workbench can be used but in this repository we will only focus on CLI tools.

2. Incremental MySQL backup

* 
* 

### MongoDB

## Automate the backups from section 2 using bash scripting (either MySQL or MongoDB).

## Schedule the scripts from section 3 using cron job to perform a full backup once a week and an incremental backup every day.