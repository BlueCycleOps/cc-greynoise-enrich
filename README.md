# cc-greynoise-enrich
----

This pack compliments the `greynoise-redis-docker` solution by Blue Cycle to enrich events in Cribl Stream with a malicious disposition. Due to Cribl's ability to process high event volumes, a REST API cannot handle the volume of queries required to enrich events in real time. Enter redis. This solution keeps a redis instance updated with the latest GreyNoise dataset in order to enrich an IP address from Redis in realtime.

This pack is a collaboration between [Cribl](https://cribl.io), [Blue Cycle](https://www.bluecycle.net) and [GreyNoise](https://www.greynoise.io). 

This pack currently supports a single node redis cluster that is already running and populated with GreyNoise data. More info [here](https://docs.cribl.io/stream/redis-function).

## Requirements Section

Before you begin, ensure that you have met the following requirements:

* You need a GreyNoise API Key with appropriate license that includes approved FEED support, and the associated software package that can populate redis from the GreyNoise API.
* An event with an IP address to enrich.
* The IP address, port, and admin secret for your redis instance.

## WARNING
* When using Redis functions in a pack, if redis is not available, your pipeline will time out and may affect Cribl performance.
* In order to authenticate to your redis instance as described below, you will need to configure a `user` or `admin` secret as a text secret in Stream, then select that secret in the redis function's authentication section. Info on Secrets [here](https://docs.cribl.io/stream/securing-and-monitoring/#accessing-secrets). Info on Redis Authentication in Stream [here](https://docs.cribl.io/stream/redis-function#authentication-method). Authentication is currently disabled in the sample pipelines.


## Redis Setup

For each redis function, set up the following:
* filter
** Lookup: Result field: `ex: malicious` (the field to write intel lookup result), Command: get, Key: event field you are looking up, Args (not required)
* Redis URL: redis://10.10.10.5:6379 (redis://<IP>:<port>)
* Secret: redis admin/user account secret
More info on setting up redis functions can be found in the Cribl Stream docs [here](https://docs.cribl.io/stream/redis-function)


## Using The Pack

### IPv4 Enrichment Method (Using Palo FW Log Samles)

1. With the redis function configured above, enter the event field with an IPv4 address in the `Key` field for your redis lookup.
2. In the `PAN-Traffic-Enrich-Src-v1` example pipeline in this pack, we use a PAN firewall event, and are doing a lookup on the `source_ip` field, and returning our result to a field called `malicious`.

One could use a similar redis function to enrich any IP address in an event and return the result to any field name desired.


## Release Notes

### Version 0.1.5 - 2023-01-24
- Fix findings for pack approval

### Version 0.1.3 - 2022-12-20
- Initial public release for performance and QA testing.



## Contributing to the Pack
To contribute to the Pack, please do the following:

Reach out to @JP Bourget on Slack to discuss.


## Contact
Reach out to '@JP Bourget' on Slack
To contact us please email support@bluecycle.net or support@greynoise.io

## License
This Pack uses the following license: [`Apache 2.0`](https://www.apache.org/licenses/LICENSE-2.0.txt).
