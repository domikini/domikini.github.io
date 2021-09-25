## The /var/log/ folder usually will become filled up with old log files from logrotate. Here is a way with find command to clean and delete those.

Delete all files:

`find /var/log -type f -delete`

Delete all .gz and rotated file

`find /var/log -type f -regex ".*\.gz$"`
`find /var/log -type f -regex ".*\.[0-9]$"`

Try run command without "-delete", to test it.
