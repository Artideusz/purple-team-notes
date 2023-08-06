# Collecting logs from PfSense syslogs

1. Configure firewall rules to monitor.

![](./img/pfsense_firewall.png)

2. Enable remote logging, configure the contents and point to the logstash instance (for this example, we will only enable firewall events).

![](./img/remote_logging.png)

3. Download the grok patterns from https://github.com/patrickjennings/logstash-pfsense/blob/master/patterns/pfsense2-4.grok and save it in a folder where logstash can access it.

![](./img/grok_file.png)

4. Configure the logstash pipeline.

![](./img/logstash_pipeline.png)

5. Restart logstash.

6. Add required permissions for the logstash user.

![](./img/permissions.png)

7. Create a data view to analyze the data.

![](./img/data_view.png)