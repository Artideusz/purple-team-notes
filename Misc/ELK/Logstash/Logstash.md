# Logstash

Logstash is a data processing tool which collects, transforms and sends data to a database of choice. I tis usually used with Elasticsearch for data analysis.

## Structure of a pipeline

A pipeline consists of plugins which the structure can be defined as follows:

```

input {
    <plugin_name> {
        <option_key> => <option_value>
        <option_object> {
            <nested_option_key> => <nested_option_value>
        }
        ...
    }
    
    <plugin_name_2> {
        <option_key> => <option_value>
        <option_object> {
            <nested_option_key> => <nested_option_value>
        }
        ...
    }
}

filter {
    <plugin_name_3> {
        <option_key> => <option_value>
        <option_object> {
            <nested_option_key> => <nested_option_value>
        }
        ...
    }
    
    <plugin_name_4> {
        <option_key> => <option_value>
        <option_object> {
            <nested_option_key> => <nested_option_value>
        }
        ...
    }
    
    <plugin_name_5> {
        <option_key> => <option_value>
        <option_object> {
            <nested_option_key> => <nested_option_value>
        }
        ...
    }
}

output {
    <plugin_name_6> {
        <option_key> => <option_value>
        <option_object> {
            <nested_option_key> => <nested_option_value>
        }
        ...
    }
}
```

There are three types of plugins:

- Input plugins
- Filter plugins
- Output plugins

Each of the sections (`input`,`filter` and `output`) can have multiple plugins defined inside them, which will be sequentially ran upon data being recieved from a source defined in the `input` section.

## Grok multiple different sources

If you log different types of data using the same input, you can `grok` sequentially until a regex returns successfully using the following syntax:

```
filter {
    ...
    grok {
        match => {
            some_field => [
                "regex1",
                "regex2",
                "regex3",
                ...
                "regexN"
            ]
        }
    }
    ...
}
```

## Resources

- https://www.elastic.co/guide/en/logstash/current/configuration.html
- https://stackoverflow.com/questions/31667083/grok-pattern-for-different-types-of-log-in-a-logfile