# Cribl Splunk UF Internal Reduction
----

Use this Pack to reduce your Splunk Forwarder log volume.

Internal logs do not count against index, but they surely impact your resource utilization. In my experience, 5-7% of enterprise deployments' resources are consumed by UF internal logs. Some of these logs can be useful, but most are not. The pipelines in this Pack will allow you to better control your UF traffic before index time.

See the top comment in each pipeline for detailed description of what the pipeline is doing.

## Requirements Section

* Splunk Forwarders sending logs through LogStream :-)
* We have provided a few component-level entries in the lookup. Add more, or remove, as you see fit

## Using The Pack

To use this Pack, follow these steps:

1. Install the pack
2. Set-up route(s) that match for Splunk inputs *and* source for splunkd, metrics, or introspection; and point them to the pack

## Release Notes

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
