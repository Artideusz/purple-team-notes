# Elastalert2 - Free open-source tool for alerting on anomalies

Elastalert is a standalone tool used for alerting. It can be used as an alternative to the Elastic Alerting feature which exists in the premium version of the stack. It has many alerting options, including:

- Discord
- Email
- Microsoft Teams
- ServiceNow
- HTTP POST requests
- And many more.



## Configuring Elastalert2

You can configure elastalert2 using the `config.yaml` file. Here is an example file:

```yaml
rules_folder: /opt/elastalert/rules
buffer_time:
    minutes: 10
run_every:
    minutes: 10
es_host: elastic.example.local
es_port: 9200
es_username: example
es_password: password
writeback_index: elastalert_status
alert_time_limit:
    days: 2
```

- `rules_folder` is self-explanatory.
- `buffer_time` denotes the time range for events to be processed, from present to `minutes: 10` ago.
- `run_every` is self-explanatory.
- `writeback_index` is the index which elastalert2 will use to save it's state.
- `alert_time_limit` indicates the time in which an alert exists for.

## Rules and alerts

Rules are files which have criteria needed to invoke an alert. A rule is in `yaml` format. Here is an example rule:

```yaml
name: Port scan
index: example-*
type: cardinality

timeframe:
    minutes: 1
    
cardinality_field: "dest_port"
max_cardinality: 100

aggregation_key: "src_ip.keyword"

filter:
    - query:
        query_string:
            query: "tags: firewall"

alert: 
    - email

from_addr: "alerter@example.com"
email: "customer@example2.com"
alert_subject: "Issue {0} occurred at {1}"
alert_subject_args:
- issue.name
- "@timestamp"
```

- `name` - The name of the rule. This field is required.
- `index` - The index which willbe searched.
- `type` - The ![type](https://elastalert.readthedocs.io/en/latest/ruletypes.html#rule-types) of the rule. `cardinality` will group by a selected field and calculate the count of distint values for that field (think of it as `stats dc(dest_port)` in splunk).
- `timeframe` - The timeframe in which to search events and count the unique value count.
- `aggregation_key` - The key in which to group events by (it's like the `by <aggregation_key>` keyword in `stats dc(dest_port) by src_ip`).
- `filter` - The DSL query filter which will be used for filtering results.
- `alert` - The ![alerter type](https://elastalert.readthedocs.io/en/latest/ruletypes.html#alerts) to invoke when the rule has a match.

## Resources

- https://elastalert2.readthedocs.io/en/latest/elastalert.html#overview
- https://elastalert.readthedocs.io/en/latest/ruletypes.html
