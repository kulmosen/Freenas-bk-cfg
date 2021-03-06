<h1>FreeNAS Backup conf</h1>

Python script to backup FreeNAS configuration file Created by Eric Bright Copyright (C) 2013
Expanded by adding mail, and the use of zpool location of the backup, by Dennis Juhler Aagaard (C) 2015

<b>Tested only on 9.2.1.7</b>

This script will copy the FreeNAS configuration file (/data/freenas-v1.db) to the location /mnt/zpool/.system/cores. The script copies the configuration only when a change has been made to the FreeNAS Configuration file. The scripts makes an encrypted version of the .db file with the provided password in the mailaccount and mail it to a email account of your choosing. Previous backups will be archived when a new backup is created, but encrypted files on FreeNAS will be cleaned after mail action.

Arguments to the script has been made to make it flexible to which mail server should be used and also introduced a System name independent of hostname og the system in case you have multiple FreeNAS systems you need to have backups from.

<code>Usage: backup.py [options]</code>

<code>Options:</code>

<code>-h, --help            show this help message and exit</code>

<code>-n NAME, --name=NAME    Name of the system</code>

<code>-p PASSWD, --passwd=PASSWD</code>
                        <code>Password for your mail account. Beaware that this is
                        in cleartext. This password will also be used for encryption.</code>

<code>-t EMAILTO, --emailto=EMAILTO
                        Mail you want to send the file to</code>

<code>-f EMAILFROM, --emailfrom=EMAILFROM
                        Naming of the email adress sent from. Mostly the same
                        as your mailaccount</code>

Added default arguments inside script. Remember to change the default args for your liking.
Changed the script from using SMTPlib in Python to send the mail via sendmail. If you dont recieve any mail, check your spam folder. Particular gmail.com.


<h2>Install Instructions</h2>

Copy backup.py to /usr/local/sbin/ by first mounting the system as writeable with mount -uw /
<br>Change permissions to add execute to backup.py (ex. chmod +x /tmp/backup.py)
<br>Change the system back to read-only with mount -ur /

Log into the FreeNAS Web Interface
<br>Expand System, expand Cron Jobs
<br>Select Add Cron Job

User = root 

Command = <code>backup.py -n '[name]' -p [password] -t [emailto] -f [emailfrom]</code>

or if you have define default arguments inside script

Command = <code>backup.py -n '[name]'</code>

Set it to run each hour to catch your FreeNAS changes. If there has not been any changes, nothing will happen.

Uncheck Redirect Stdout to receive email* updates for the backup operation
<br>Recommended is having this checked, because you will recieve an email with the .db files attached.

Uncheck Redirect Stderr to receive email* updates for errors processing the script
<br>Recommended is to have this unchecked to recieve any errors that might happen.

Check Enabled to enable this job
<br>*emails sent to root account's email address

<h2>Clientside decryption script</h2>

Copy the decrypt.py to your prefered location.
<br>Execute by running in terminal
<code>./decrypt.py -p 'password' -f 'filepath'</code>

password can also be typed in in a non-cleartext form if not provided in the script argument.

<code>./decrypt.py -f 'filepath'</code>
<br><code>enter aes-256-cbc decryption password:</code>