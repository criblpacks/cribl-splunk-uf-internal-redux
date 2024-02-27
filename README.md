# Cribl Splunk UF Internal Reduction
----

Use this Pack to reduce your Splunk Forwarder log volume.

Internal logs do not count against index, but they surely impact your resource utilization. In my experience, 5-7% of enterprise deployments' resources are consumed by UF internal logs. Some of these logs can be useful, but most are not. The pipelines in this Pack will allow you to better control your UF traffic before index time.

## Metrics Pipeline Explained

Intended for `$SPLUNK/var/log/splunk/metrics.log` files

1. Start by dropping any events that are **NOT** thruput events
    * Majority of reduction from this step
2. Optionally aggregate the events into Metrics events
    * There are 2 Aggregate functions: 1 for `per_*_thruput` and one for `thruput`
    * There is also the option to lookup the host to get a farm or pod to group by
    * Metrics will be delivered to _metrics by default; modify if needed but be aware of license impacy
3. Use the Trim function to remove the timestamp text from _raw (we already have _time)
4. Rewrite source field to just the file name, eg `./metrics.log`

## Splunkd Pipeline Explained

Intended for `$SPLUNK/var/log/splunk/splunkd.log` files

1. Extract the basic layout of the event
2. Drop DEBUG and TRACE level events with prejudice
3. Suppress messages from *some* components in 30 second windows based on host-punct-component-level key
    * Component-level combos that will be suppressed are listed in the components_suppression.csv lookup
    * A `repeated=` counter will be added to the end of the surviving event
        - Instead, optionally leave `suppressCount` as an index time field
4. Random clean up:
    * Drop the text time string since we already have _time
    * Replace the path to Splunk if shown in the _raw event with `$SPLUNK`
    * Replace the path to `$SPLUNK/var/log/splunk/` in `source` with `./`


## Requirements Section

* Splunk Forwarders sending logs through LogStream :-)
* We have provided a few component-level entries in the lookup. Add more, or remove, as you see fit

## Using The Pack

To use this Pack, follow these steps:

1. Install the pack
2. Set-up route(s) that match for Splunk inputs *and* source for splunkd or metrics and point them to the pack
    * Example: `__inputId.startsWith('splunk') && /splunkd.log|metrics.log/.test(source)`
    
*Note: I do not recommend using this as a pre-processor pipeline unless you have a source defined that solely receives internal logs*

## Release Notes

### Version 1.0.2 Feb 26, 2024

* Updated this README. Same as previous release.

### Version 1.0.1 Dec 9, 2021

* Updated docs to reference _metrics as default metrics destination index

### Version 1.0.0 Dec 7, 2021

* Replaced the Agg function with a Suppression function with an Eval to add the `repeated` count 
* Clarified this doc

### Version 0.3.4 Aug 13 2021

* Re-wrote Agg function to be cleaner
* Moved regex extract to the top
* Used __level from regex extract for drop function instead of full _raw

### Version 0.3.3 July 27 2021

* Aggregation of stats sent to metrics store (optional)
* Comments clean-up

### Version 0.3.1 July 20 2021

* Aggregation of stats sent to metrics store (optional)
* Comments clean-up

### Version 0.2 July 13 2021

* re-wrote splunkd log suppression

### Version 0.1 July 12 2021

* initial release

## Contributing to the Pack
To contribute to the Pack, please do the following:

Contact Jon Rust <jrust@cribl.io>


## Contact
To contact us please email <jrust@cribl.io>.


## License
This Pack uses the following license: [`Apache 2.0`](https://github.com/criblio/appscope/blob/master/LICENSE).
