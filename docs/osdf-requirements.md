# OSDF Storage Contribution Requirements

## Hardware / Configuration Requirements

A service node supporting a Pelican cache or origin has the following requirements: 

* File systems: The cache should have a partition of its own for storing data and metadata.
* Network ports: The cache service requires the following open ports:
	* Inbound TCP port 8443 for authenticated file access via the HTTP(S) and XRoot protocols.
	* Inbound TCP port 8444 for access to web endpoints such as origin-based token issuers, federation health checks, and metrics
	* Outbound UDP port 9930 for reporting to xrd-report.osgstorage.org and xrd-mon.osgstorage.org for monitoring
* Service requirements:
	* For an origin (recommended): 
		* 4 cores
    	* 25 Gbps connectivity
    	* 20 GB of RAM
    	* An origin can be run with 1 core, 12GB of RAM and 1Gbps connectivity if needed. 
	* For a cache: 
		* 8 cores
		* 40 Gbps connectivity
		* 24 GB of RAM
		* Disk space for the cache partition: 
			* A regional cache should have at least 50-200 TB of NVMe disk for the cache partition; you may distribute the disk, e.g., by using an NVMe-backed Ceph pool, if you cannot fit that much disk into a single chassis
			* A cache being used to serve data from the OSDF to a single site should have at least 2 TB of NVMe disk for the cache partition

For an origin service, if using a POSIX file system as the backing store, it must 
be mounted to the service node supporting the Pelican origin. 


## Checklist for Your OSDF Cache

If you are setting up
[your own OSDF Cache](https://osg-htc.org/docs/data/osdf/overview/#option-2-you-the-campussite-operate-the-service),
please check these items before enabling access:

*   Backing storage for the Cache uses fast storage (e.g., SSD, NVMe)
*   The service is listening on the expected public IP address and ports
    (see above for defaults, and check local configuration)
*   The service has at least a 40 Gbps connection to the WAN, both up and down
*   The OSDF registry has current, accurate information for the Cache
*   If the Cache is for local use only or for restricted namespaces,
    verify that those are configured correctly


## Feedback

Do you have questions or comments about this page?
What could we do better?
If so, please email us at [support@osg-htc.org](mailto:support@osg-htc.org)!
