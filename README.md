# Splunk> Cheat Sheet

Basic Search Commands:
----------------------
| Table Command:      | Description:                            |
|----------------------|----------------------------------------|
| `index=`            | Search data in a specific index.        |
| `source=`           | Search data from a specific source.     |
| `sourcetype=`       | Search data of a specific sourcetype.  |
| `host=`             | Search data from a specific host.       |
| `search`            | Start a search.                         |
| `|` (pipe)          | Pass results from one command to another.|

Time Range:
-----------
| Time Range Command: | Description:                            |
|----------------------|----------------------------------------|
| `earliest=`         | Specify the start of the time range.   |
| `latest=`           | Specify the end of the time range.     |
| `timechart`         | Create a time-based chart.             |

Filtering and Fields:
----------------------
| Filtering and Fields Commands: | Description:                        |
|-------------------------------|------------------------------------|
| `search term`                  | Search for specific terms.         |
| `NOT term`                     | Exclude results containing `term`. |
| `field=value`                  | Filter by a specific field value.  |
| `field!=value`                 | Exclude specific field values.     |
| `field>value`                  | Filter by values greater than.     |
| `field<value`                  | Filter by values less than.        |
| `field IN (val1, val2)`        | Filter by multiple values.         |
| `field IS NULL`                | Filter for empty fields.           |
| `| stats`                      | Aggregate data using statistics.   |
| `| sort`                       | Sort results based on a field.     |
| `| eval`                       | Create calculated fields.          |

Advanced Commands:
------------------
| Advanced Commands: | Description:                          |
|--------------------|--------------------------------------|
| `rex`              | Extract fields using regular expressions. |
| `transaction`      | Group events into transactions.      |
| `dedup`            | Remove duplicate events.              |
| `lookup`           | Enrich data with external sources.    |
| `join`             | Combine data from multiple sources.   |
| `eval`             | Create calculated fields.            |

Reporting and Visualization:
---------------------------
| Reporting and Visualization Commands: | Description:               |
|--------------------------------------|---------------------------|
| `| table`                            | Display results in a table. |
| `| stats`                            | Generate statistics.        |
| `| chart`                            | Create charts and graphs.  |
| `| timechart`                        | Create time-based charts.  |

Note: Replace placeholders like `index`, `source`, `sourcetype`, `host`, `term`, `field`, `value`, etc., with actual values relevant to your search.

Examples:
---------
- `index=your_index sourcetype=your_sourcetype | search "error"`
- `index=your_index sourcetype=your_sourcetype earliest=-24h latest=now | stats count by field`
- `index=your_index sourcetype=your_sourcetype | timechart count by field`
- `index=your_index sourcetype=your_sourcetype | table field1, field2 | sort field1`

Feel free to customize and expand this cheat sheet to include specific commands and examples relevant to your use case and data.


### Basic Search Commands:
- `index=your_index sourcetype=your_sourcetype` - Filter data based on index and sourcetype.
- `source="your_source"` - Filter data based on the source of the logs.
- `sourcetype="your_sourcetype"` - Filter data based on sourcetype.

### Time Range:
- `earliest="mm/dd/yyyy:hh:mm:ss"` - Specify the earliest time for the search.
- `latest="mm/dd/yyyy:hh:mm:ss"` - Specify the latest time for the search.
- `timechart span=1h` - Aggregate data into 1-hour time intervals.

### Field Extraction:
- `| rex field=_raw "regex_pattern"` - Extract fields using regular expressions.
- `| table field1, field2` - Display specific fields in the search results.
- `| stats count by field` - Count occurrences of a specific field value.

### Filtering:
- `| search "search_term"` - Filter results based on a specific search term.
- `| where field="value"` - Filter results using the WHERE clause.
- `| eval new_field=expression` - Create a new field based on an expression.

### Aggregation and Statistics:
- `| stats count` - Count the number of events in the results.
- `| stats sum(field)` - Calculate the sum of a numeric field.
- `| stats avg(field)` - Calculate the average of a numeric field.

### Visualization:
- `| timechart span=1h count` - Create a timechart visualization.
- `| chart count over field` - Create a bar chart based on a field.
- `| geom geo_us_states` - Create a geographical map visualization.

### Event Correlation:
- `| transaction startswith="event_start" endswith="event_end"` - Group events into transactions.
- `| streamstats dc(field) as count` - Calculate a running count of unique values.

### Advanced Threat Hunting:
- `| tstats count from datamodel=your_datamodel` - Use the tstats command to query data models.
- `| pivot` - Create pivot tables to explore data relationships.
- `| makeresults` - Generate synthetic data for testing and analysis.

### Threat Indicators:
- `| lookup threat_intel.csv` - Enrich data with threat intelligence.
- `| iplocation field=ip` - Resolve IP addresses to geolocation information.

### Alerts and Notifications:
- `| sendalert` - Trigger custom alerts and notifications.
- `| outputlookup` - Save search results to a CSV file for reporting.

### Splunk Apps:
- Utilize Splunk apps like Splunk Enterprise Security (ES) for advanced threat hunting capabilities.

### Detect unusual access patterns
```
index=your_index sourcetype=your_sourcetype 
| stats count by user 
| sort -count 
| where count < 5
```

### Outbound anomolies in traffic
```
index=your_index sourcetype=your_sourcetype 
| stats sum(bytes_out) as total_bytes by src_ip 
| eventstats avg(total_bytes) as avg_bytes 
| where total_bytes > avg_bytes*2
```

### Failed, followed by a successful login
```
index=your_index sourcetype=your_sourcetype 
| transaction maxspan=1h startswith="Failed Login" endswith="Successful Login" 
| search Failed AND Successful
```

### Rapidly changing firewall rules
```
index=your_index sourcetype=your_sourcetype 
| timechart span=1h count by firewall_rule 
| streamstats window=3 sum(count) as total_count by firewall_rule 
| where total_count > 10
```

### Spikes to uncommon ports
```
index=your_index sourcetype=your_sourcetype 
| stats count by dest_port 
| sort -count 
| where count > 100 AND NOT dest_port IN (80, 443, 22)
```

### Identify large file downloads
```
index=your_index sourcetype=your_sourcetype 
| rex field=_raw "GET\s(.*)\sHTTP/1.1\"\s\d+\s(\d+)" 
| where tonumber($2) > 1000000
```

### Analyze DNS activity on subdomains
```
index=your_index sourcetype=your_sourcetype 
| rex field=query "\.(?<subdomain>[^.]+)\." 
| stats count by subdomain 
| sort -count 
| where count > 10
```

### Documentation and Community:
- [Splunk Documentation](https://docs.splunk.com/)
- [Splunk Community](https://community.splunk.com/)
