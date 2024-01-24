# Collectd - Ubuntu setup for Splunk
Collectd functions as an open-source Unix daemon, proficient in gathering a variety of performance metrics from servers and network equipment, providing valuable insights into system performance and health. This documentation aims to guide the process of forwarding Collectd logs from Ubuntu to Splunk and analyzing these logs within Splunk for comprehensive analytics.

## Intallation
```bash
apt-get install collectd 
```
## Explanation
This document is designed for educational purposes, outlining my exploration and practical application of collecting Collectd metrics data. I have experimented with the implementation of the write_splunk.so/processmon.so module to transmit data to Splunk via UDP port.

## Details
Within one specific section, I have provided details about Plugins that are not included in the default installation of Collectd.



