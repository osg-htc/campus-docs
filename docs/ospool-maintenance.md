# OSPool Integration Maintenance

In general, once your cluster is contributing to the OSPool,
things should mostly just continue to work.
There are no *required, routine* maintenance tasks specific to maintaining the integration.

## When To Contact Us

If you have or are concerned about **a possible security incident**
that affects your cluster or may affect our integration,
please [email our cybersecurity team](mailto:security@osg-htc.org).

For **all other topics,** please [email our support system](mailto:support@osg-htc.org);
a real human will reply ASAP!

Some reasons to contact us:

*   You have a scheduled or unplanned **downtime**.
    Please [see here](https://osg-htc.org/docs/common/registration/#registering-resource-downtimes)
    for instructions on registering that downtime with us;
    we will stop trying to submit jobs into your cluster until it is over
    (or you tell us explicitly to start again).

*   Your **site changes** in a way that could affect the integration
    (see [the Requirements page](ospool-requirements.md));
    e.g., networking, login node, SSH, cluster configuration, OS distro or major version.

*   You want to change the configuration (“shape” or maximum number)
    of jobs that we submit into your cluster.

## Monitoring

If you want to monitor aspects of the integration, here are a few ideas:

*   Verify that SSH to the login node continues to work.

*   Verify that the batch system on the login node is working;
    for example, can our user run basic query and submit commands?

*   Check whether any of our jobs run, especially when you would expect them to.

*   If your integration is via a Hosted CE (Option 1a),
    check [your OSPool Hosted CE Dashboard](ospool-metrics.md)
    to see if the OSPool is actually getting your contributions and allocating capacity to researchers.

*   If you told us to configure a Squid proxy service,
    perform a periodic functional test on it:
    Can it actually fetch a page from offsite?
    Squid has a strange failure mode in which the service is running
    but will not respond to any attempt to actually use it.
