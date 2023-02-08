# Forward logs from pfSense

1. Login to pfSense
2. Go to `Status -> System Logs`.
3. Go to `Settings`.
4. Scroll down and enable "`Send log messages to remote syslog server`".
5. Set the relevant information.
6. Open Splunk.
7. Click on `Settings -> Data inputs`.
8. Click on `Add new` in the `UDP` category.
9. Set the port specified in pfSense and click `next`.
10. Set the relevant information and hit `next`.
11. Click `submit`.

Logs should now come into Splunk from pfSense.