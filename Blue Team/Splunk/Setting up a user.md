# Setting up a user in Splunk

1. Go to `$SPLUNK_HOME/etc/system/local/`.
2. Create a file called `user-seed.conf`.
3. Generate a password using `splunk hash-passwd "<your-password>"`.
4. Create the following fields and fill out the values in `user-seed.conf`:
5. Change the ownership of the file to `splunk:splunk`.

*user-seed.conf*
```
[user_info]
USERNAME=<username>
HASHED_PASSWORD=<password from splunk hash-passwd>
```

5. Restart splunk by using the command `splunk restart`.

#### Links
- https://docs.splunk.com/Documentation/Splunk/latest/Admin/User-seedconf