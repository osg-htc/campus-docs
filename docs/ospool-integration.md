# OSPool Integration Process

This page explains the process for integrating your compute cluster with the OSPool.


## Overall Process

We organize the integration process around two meetings,
plus some tasks in-between and after:

1.  **Start.** Just email [support@osg-htc.org](mailto:support@osg-htc.org) and
    write that you want to contribute to the OSPool!
    If you want, give an overview of
    who is contributing,
    your goals,
    and your computing system
    (detailed technical specs are not needed).
    We aim to reply quickly and get started.

1.  **Kickoff meeting.**
    We will discuss in more depth your goals for the project and what you aim to contribute,
    plus the technical options (see below) for integration and the remaining process.
    The project PI or lead, and participating technical lead and staff, are encouraged to join;
    the meeting lasts about 1 hour.

1.  **Preparations.**
    After the kickoff, both teams will have preparatory steps to complete;
    these items will be detailed in a follow-up email.

1.  **Live integration session.**
    When everyone is ready,
    we schedule a joint, live integration session to
    verify site readiness,
    complete the integration,
    monitor the first OSPool jobs running via the integration, and
    plan for next steps.
    Certain technical staff from both teams are required,
    and we allocate 2 hours for the session.

    This session is live to avoid the inevitable and sometimes lengthy delays
    that can arise when trying to troubleshoot technical issues over email.
    In our experience, most integrations need just one 2-hour session.

1.  **Post-integration follow-up activities.**
    Once the integration is working,
    there is a small list of follow-up activities to complete:
    verifying the configuration of contributions,
    scaling up,
    accessing views of the contributions and how they are used, and
    setting up communication for ongoing maintenance and community.

Throughout the process, you remain in control of the overall schedule.
If you want to move fast, so do we!
If you are juggling a busy schedule, we will move forward when you have time.


## Integration Mechanisms

We offer two fundamental ways to integrate some of your computing capacity with the OSPool,
and the first way has two flavors for a total of three options.
These are outlined below.

**We will discuss the options and their tradeoffs** during the kickoff meeting (see above),
so consider the information below as a starting point.

*   **Option 1a: OSG Hosted CE**

    If you manage your computing capacity with a batch system (e.g., Slurm, HTCondor),
    then we can provide an automated service (an **HTCondor-CE**)
    to submit jobs into your batch system and manage them.
    In this option, we offer to host and operate your HTCondor-CE service for you
    in one of our data centers.

    The jobs submitted to your batch system are _not_ researcher jobs;
    instead, each one is a **glidein job** that, when run,
    creates an HTCondor runtime environment called an **Execution Point**,
    which will in turn join the OSPool and accept researcher jobs from it.

    The Hosted CE service submits and manages jobs
    via SSH to an unprivileged account on a login node that you provide.
    You can read [detailed requirements for this mechanism](ospool-requirements.md)
    and we will discuss them during the kickoff.

*   **Option 1b: Self-Operated CE**

    In this variant of the HTCondor-CE mechanism,
    you host and operate the HTCondor-CE service yourself.
    Then, you provide access from our OSPool central services to that HTCondor-CE service,
    so that we can send glidein jobs to your CE.
    
    The tradeoffs and requirements for this option differ from the Hosted CE option,
    and we can discuss those during the kickoff.

*   **Option 2: Containers**

    You can also use containers to contribute capacity to the OSPool.
    For this option, we provide a container image, and you run it on one or more hosts in your cluster.
    Each running container creates the same kind of **Execution Point** as above,
    and thus joins the OSPool with the capacity given to the container.
    
    This option may be most suitable if you already manage your computing capacity with containers
    or have reasons to avoid sharing some capacity using a batch system.
