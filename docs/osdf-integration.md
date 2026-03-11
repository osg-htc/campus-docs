# OSDF Integration Process

This page explains the process for integrating storage with
the OSDF.

## Overall Process

We organize the integration process around two meetings, plus some tasks
in-between and after:

1.  **Start.** [Fill out our interest form](https://docs.google.com/forms/d/e/1FAIpQLSem2Lu-
9nL2DBOXrSzmHTWdBZHsMmVN_pIq5ITSnj4A51BTLw/viewform) or email [support@osg-htc.org](mailto:support@osg-htc.org). 
We aim to reply quickly and get started.

1.  **Kickoff meeting.** We will discuss in more depth your goals for
the project and what you aim to contribute, plus the technical options
(see below) for integration and the remaining process. The project PI or
lead, and participating technical lead and staff, are encouraged to
join; the meeting lasts about 1 hour.

1.  **Preparations.** After the kickoff, both teams will have
preparatory steps to complete; these items will be detailed in a
follow-up email.

1.  **Live integration session.** When everyone is ready, we schedule a
joint, live integration session to verify site readiness, complete the
integration, test data uploads and downloads,
and plan for next steps. Certain technical staff from both teams are
required. 

    Here we are following the lead of the OSPool integration process. 
    This session is live to avoid the inevitable and sometimes lengthy
    delays that can arise when trying to troubleshoot technical issues
    over email. 

1.  **Post-integration follow-up activities.** Once the integration is
working, there is a small list of follow-up activities to complete: 
setting up communication for ongoing maintenance and community, and 
ensuring the service is functioning as expected. 

We work at your pace throughout. While we will send check-in and follow 
up emails, these are just to make sure you have the information you need 
for when you're ready to move forward. If this is one of many projects, 
we understand the necessity of a long time scale; if you're under a tight 
timeline, we will do our best to accommodate. 

## Service Options

Before joining storage to the OSDF, you should identify a) what service you are
offering (an origin or a cache) and b) if an origin, whether you plan to
serve your own data, or share storage with OSG services. If you aren't
sure which is best for your campus, we will discuss in the kickoff
meeting. 

## Integration Mechanisms

To connect storage to the OSDF, an origin or cache service needs to be
running with on a physical or virtual host with access to the backing
store. Pelican services can access storage via Unix mount, S3 endpoint,
or Globus endpoint.

There are two overall paths forward to operate the Pelican service. Many
campus storage contributors use some version of Option 1, as it simplifies the effort
required to keep the service operational. **We will discuss the options and their tradeoffs** during the kickoff meeting (see above),
so consider the information below as a starting point.

!!! tip "Hardware recommendations" See hardware recommendations for each
service in the
[origin](https://osg-htc.org/docs/data/osdf/install-origin-container#before-starting) and
[cache](https://osg-htc.org/docs/data/osdf/install-cache-container#before-starting)
documentation

### Option 1: We (OSDF) operate the service

In this option, OSDF operations staff operate the needed service, 
either on our own hardware or hardware you provide: 

*   **Option 1a: Hosted Origin/Cache - S3 only**

	If your data store has an S3
	endpoint, we can operate the origin without any local hardware
	requirements or configuration. Let us know if this applies to you.

*   **Option 1b: Hosted Origin/Cache - Kubernetes**

	Currently, we can operate the Pelican services on a service node local
	to the institution via Kubernetes. It is conceptually described on our
	[home
	website](https://osg-htc.org/about/osdf/deploying_an_osdf_origin.html)
	for an origin. A cache would be deployed exactly the same way. The 
	service node needs to have access to the storage (typically via a 
	POSIX mount). 
	
	You can provide a node with Kubernetes in one of two ways:
	
	1. Integrate with the National Research Platform (NRP): See the 
	NRP documentation on [Joining a Server to 
	NRP](https://nrp.ai/documentation/admindocs/participating/new-
	contributor-guide/). After your server is integrated with NRP, we will
	deploy the appropriate service. 
	
	1. If you have an existing Kubernetes cluster on campus, and you can
	provide a user account on that cluster, our staff can operate the
	service from there. 

*   **Option 1c: Hosted Origin/Cache - SSH (experimental)**

	We are developing an integration method
	where OSDF operations staff can run the appropriate Pelican service on
	your service node through **SSH access**, without requiring Kubernetes
	or root access. Contact us ([support@osg-htc.org](mailto:support@osg-htc.org)) if you would like to
	explore using this option.

The benefit of this approach is that you don't need to learn anything
about Pelican or be responsible for running the appropriate service. The
trade-off is the work of either running Kubernetes yourself, or
providing to the node so it can be integrated with NRP.

### Option 2: You (the campus/site) operate the service

In this scenario, you deploy the service on either your storage system
or an attached service node as you would any other service, like a web
server. 

There are multiple ways to deploy Pelican yourself, detailed in 
the OSG technical docs: 

1. Use the Pelican container 
	1. [Installing the OSDF Origin by Container](https://osg-htc.org/docs/data/osdf/install-origin-container) 
	1. [Installing the OSDF Cache by Container](https://osg-htc.org/docs/data/osdf/install-cache-container) 
1. Use the RPM
	1. [Installing the OSDF Origin by RPM](https://osg-htc.org/docs/data/osdf/install-origin-rpm) 
	1. [Installing the OSDF Cache by RPM](https://osg-htc.org/docs/data/osdf/install-cache-rpm)

The benefit of this approach is full control of the service - no one
from the OSG team needs access to the service node. The trade-off is the
time and effort to stand up the service, and then to maintain it
(upgrades, configuration, etc.). 

