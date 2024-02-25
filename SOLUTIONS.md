# Backups exercise

## Backups types

1. Full: The most basic and comprehensive backup method, where all data is sent to another location.

2. Incremental: Backups all files that have changed since the last backup ocurred. Incremental backups are made possible by enabling the server's binary log, which the server uses to record data changes.

3. Differential: Backups only copies of all files that have changed since the last full backup.

## CLI tools to perform backups (full and incremental) and restores for the databases listed below:

### MySQL

Most backup strategies start with a complete (full) backup of the MySQL server, from which you can restore all databases and tables. After you have created a full backup, you might perform incremental backups (which are smaller and faster) for the next several backup tasks. You then make a full backup periodically to begin the cycle again.

#### Full MySQL backup

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

#### Incremental MySQL backup

As mentioned before, incremental MySQL backups can NOT be done if we have not performed any other backup previously and it needs the use of binary logging records.

In order to enable binary logging, we need to modify the MySQL default configuration file, often located at `/etc/mysql/my.cnf or /etc/my.cnf`.

With your favourite text editor we will modify or add the following lines, in this case we will use `nano` text editor:

```
log_bin = /var/log/mysql/mysql-bin.log
```

> Replace `/var/log/mysql/` with the desired path for your binary log.

Then we should restart MySQL to apply the changes with the following command:

```
service mysql restart
```

Now we can perform an incremental backup.

First we need to find the `--start-position` of the binlog position from the last backup. In order to do so, we need to run this command:

```
SHOW MASTER STATUS;
```

It will give us information about the current binlog file name and its position.

Secondly we need to run the following command:

```
mysqlbinlog --start-position=xxx /var/log/mysql/mysql-bin.00000x >> example_backup.sql
```

Replacing "xxx" with the actual position of the binlog and "x" in "00000x" with the sequence number of the same file that you want to work with. An example would be (position:2 / sequence number:2):

```
mysqlbinlog --start-position=2 /var/log/mysql/mysql-bin.000002 >> example_backup.sql
```

> `>>` Is also for output redirection, but it appends the output to the  end of an existing file or creates a new file that does not exists.

In the case scenario that the binary logging file has been enabled before the full backup performance and the `mysqldump` command has been ran with the flag `--flush-logs` to close the current logs (mysql-bin.000001) and create a new one (mysql-bin.000002), we can just simply take an incremental backup by flushing the binary logs and saving the binary logs created from the last full backup by running the following command:

```
mysqladmin -uroot -p flush-logs
```

This will close the `mysql-bin.000002` and create a new one (`mysql-bin.000003`).

It can be checked by running `ls -l /var/log/mysql/`


### MongoDB

## Automate the backups from section 2 using bash scripting (either MySQL or MongoDB).

## Schedule the scripts from section 3 using cron job to perform a full backup once a week and an incremental backup every day.