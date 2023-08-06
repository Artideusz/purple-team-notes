# Logging in elasticsearch

Elasticsearch uses Log4j 2 for logging. Logs are saved in the `/var/log/elasticsearch` folder or in the `$ES_HOME/logs` folder by default. The only data logged out-of-the-box is the garbage collector data. You can configure web application logs by using a `log4j2.properties` file.

## Resources
- https://www.elastic.co/guide/en/elasticsearch/reference/current/logging.html
- https://www.elastic.co/guide/en/elasticsearch/reference/7.17/important-settings.html#gc-logging