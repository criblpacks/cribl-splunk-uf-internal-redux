# Cribl Splunk UF Internal Reduction
----

Use this Pack to reduce your Splunk Forwarder log volume.

Internal logs do not count against index, but they surely impact your resource utilization. In my experience, 5-7% of enterprise deployments' resources are consumed by UF internal logs. Some of these logs can be useful, but most are not.

In this case we are:

* Dropping any splunkd logs at DEBUG or TRACE level
* Aggregating repetitive splunkd.logs based on component-level ID in a lookup file
    * A repeat count will be added to the logs allowed through.
* Dropping any metrics logs that are not either name and group =thruput, or group=per_*_thruput
    * Optionally aggregating these based on a lookup value for farm
    * Optionally:
        * Aggregation logs will still end up in _internal, with sourcetype and source == 'metrics:agg'
        * - OR - Aggregated stats will be sent to a metrics store (metrics index is `metrics`, adjust as needed in the Aggregation functions)
* Dropping any introspection logs
* For splunkd and metrics logs that do make it through, optionally trim the timestamp off _raw
* Trim source to just the file name (eg, `./splunkd.log`)

## Requirements Section

* Splunk Forwarders sending logs through LogStream :-)
* We have provided a few component-level entries in the lookup. Add more, or remove, as you see fit

## Using The Pack

To use this Pack, follow these steps:

1. Install the pack
2. Set-up route(s) for splunkd, metrics, and introspection that point to the pack

## Release Notes

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
