# OSPool Metrics and Monitoring

Once you are contributing to the OSPool,
you may be interested in details about those contributions.

## Viewing Metrics About Contributions and Their Usage

After your OSPool integration is working,
we will send you links that can help you see
how your contributions are going and
how they are being allocated to researchers.

In general, we offer:

*   **OSPool (Hosted) CE Dashboard**

    If you are contributing via a Hosted CE (Option 1a), there will be a CE Dashboard view for your CE and site.
    We will send you the link to that page during the post-integration follow-up.

    On the top of the page,
    there are small charts that show resource contributions (top, red line) and
    usage (blue area) for CPUs, memory, and disk (BCUs are a mess right now).
    If you scroll down,
    there is detailed information about the science projects that benefitted from running on your cluster;
    in the table of projects, you can click a project name to get details about it.

*   **OSPool Contributors**

    There is [an overview page for all OSPool contributors](https://osg-htc.org/services/ospool/institutions.html),
    showing data from the past year.
    You can click on your campus name to get more details.
    This may be a good resource for, say, annual reporting.

*   **OSPool Projects**

    In the previous view, projects are not linked to their project details pages.
    Instead, there is [an overview page for all OSPool Projects](https://osg-htc.org/services/ospool/projects.html).
    Click on the name of a project to learn more about it.

*   **Map**

    Visit [our map of OSPool Contributors](https://map.osg-htc.org/map/iframe?view=OSPool);
    click a marker to view details about that site.
    You can do usual map things (moving, zooming in and out, etc.) and
    capture the resulting map with the “Print as PNG” button.

Were you hoping or looking for other kinds of information from us,
or the same kinds of information but presented differently?
[Let us know](mailto:support@osg-htc.org) how we can improve!


## Investigating Live OSPool Jobs

In addition to your usual operating system and batch system tools,
and the OSPool metrics above,
we provide some ways to investigate live OSPool jobs.
You are welcome to use these tools,
and we may ask you to use them when troubleshooting.

### Viewing researcher jobs running within a glidein job

1. Log in to a worker node as the OSPool user (e.g., “osg01” but may be different at your site)

2. Pick a glidein (and its directory):
   
      **Note:** *SCRATCH* is the path of the scratch directory you gave us to put glideins in.

      1. Option 1: List HTCondor processes, pick one, and note its glidein directory:

          ```
          $ ps -u osg01 -f | grep master
          osg01    4122304 [...] SCRATCH/glide_qJDh7z/main/condor/sbin/condor_master [...]
          ```

      2. Option 2: List glidein directories and pick an active one:

         ```
         $ ls -l SCRATCH/glide_*/_GLIDE_LEASE_FILE
         ```

         Pick a “glide\_xxxxxx” directory that has a lease file with a timestamp less than about 5 minutes old.

      **Note:** Let *GLIDEDIR* be the path to the chosen glidein directory, e.g., `SCRATCH/glide_xxxxxx`

   3. Make sure HTCondor is recent enough:

      ```
      $ GLIDEDIR/main/condor/bin/condor_version
      $CondorVersion: 24.6.1 2025-03-20 BuildID: 794846 PackageID: 24.6.1-1 $
      $CondorPlatform: x86_64_AlmaLinux9 $
      ```

      The Condor Version should be 2.7.0 or later; if not, go back to step 2 and pick a different glidein directory.

4. Pick the PID of an HTCondor “startd” process to query:

    1. If you are just exploring, run the following command and pick any “condor\_startd” process — it does not matter if it is associated with the HTCondor instance identified in steps 2–3 above:

           ```
           $ ps -u osg01 -f | grep condor_startd
           ```

       2. If you are looking for the “startd” associated with some other process, start with a process tree:

          ```
          $ ps -u osg01 -f --forest
          ```

          Find the process of interest, then work upward in the process tree to the nearest ancestor “condor\_startd” process.

    3. In either case, note the PID (leftmost number) on the “condor\_startd” line you picked.

5. Run `condor_who` on the PID you picked:

    ```
    $ GLIDEDIR/main/condor/bin/condor_who -pid PID -ospool
    <b>Batch System : SLURM</b>
    <b>Batch Job    : 346675</b>
    <b>Birthdate    : 2025-04-07 14:47:02</b>
    <b>Temp Dir     : /var/lib/condor/execute/osg01/glide_qJDh7z/tmp</b>
    <b>Startd : glidein_4101601_324283152@spark-a220.chtc.wisc.edu has 1 job(s) running:</b>
    PROJECT   USER   AP_HOSTNAME         JOBID        RUNTIME    MEMORY    DISK      CPUs EFCY PID     STARTER
    Inst-Proj jsmith ap20.uc.osg-htc.org 27781234.0   0+00:17:43  512.0 MB  129.0 MB    1 0.00 4124321 4123123
    …
    ```

Definitions of fields in the header (before the PROJECT USER … row):

| Batch System | HTCondor’s name for the type of batch system you are running. |
| :---- | :---- |
| Batch Job | The identifier for this glidein job in your batch system. |
| Birthdate | When HTCondor began running within this glidein; typically, this is a few minutes after the glidein job itself began running. |
| Temp Dir | The path to the glidein job directory (remove the trailing “/tmp”) |
| Startd | HTCondor’s identifier for its “startd” process within the glidein job. |

Definitions of fields in each row of the researcher job table:

| PROJECT | The OSPool project identifier for this researcher job |
| :---- | :---- |
| USER | The OSPool AP’s user identifier for this researcher job |
| AP\_HOSTNAME | The OSPool AP’s hostname |
| JOBID | HTCondor’s identifier for this researcher job on its AP |
| RUNTIME | HTCondor’s value for the runtime of the researcher job |
| MEMORY | The amount of memory (RAM), in MB, that HTCondor **allocated** to this researcher job |
| DISK | The amount of disk, in KB, that HTCondor **allocated** to this researcher job |
| CPUs | The number of CPU cores that HTCondor **allocated** to this researcher job |
| EFCY | An HTCondor measure of the efficiency of the job, roughly calculated as CPU time / wallclock time; a value noticeably greater than the CPUs value may mean the researcher job is using more cores than requested |
| PID | The local PID of the researcher job, or more often, of the root of the process tree for the researcher job (e.g., this could be a wrapper script or even Singularity or Apptainer for a researcher job in a container) |
| STARTER | The local PID of the HTCondor “starter” process that owns this research job |


### Viewing researcher jobs running within all OSPool glidein jobs

Note: Steps 1–3 are the same as above.

1. Log in to a worker node as the OSPool user (e.g., “osg01” but may be different at your site)

2. Pick a glidein (and its directory):

    **Note:** *SCRATCH* is the path of the scratch directory you gave us to put glideins in.

    1. Option 1: List HTCondor processes, pick one, and note its glidein directory:

       ```
       $ ps -u osg01 -f | grep master
       osg01    4122304 [...] SCRATCH/glide_qJDh7z/main/condor/sbin/condor_master [...]
       ```

    2. Option 2: List glidein directories and pick an active one:

       ```
       $ ls -l SCRATCH/glide_*/_GLIDE_LEASE_FILE
       ```

       Pick a “glide\_xxxxxx” directory that has a lease file with a timestamp less than about 5 minutes old.

    **Note:** Let *GLIDEDIR* be the path to the chosen glidein directory, e.g., ```SCRATCH/glide_xxxxxx```

3. Make sure HTCondor is recent enough:

      ```
      $ GLIDEDIR/main/condor/bin/condor_version
      $CondorVersion: 24.6.1 2025-03-20 BuildID: 794846 PackageID: 24.6.1-1 $
      $CondorPlatform: x86_64_AlmaLinux9 $
      ```

      The Condor Version should be 2.7.0 or later; if not, go back to step 2 and pick a different glidein directory.

4. Run `condor_who` on all discoverable glideins on this host running as the current user:

    ```
    $ GLIDEDIR/main/condor/bin/condor_who -allpids -ospool
   
    Batch System : SLURM
    Batch Job    : 346675
    Birthdate    : 2025-04-07 14:47:02
    Temp Dir     : /var/lib/condor/execute/osg01/glide_qJDh7z/tmp
    Startd : glidein_4101601_324283152@spark-a220.chtc.wisc.edu has 1 job(s) running:
    PROJECT   USER   AP_HOSTNAME         JOBID        RUNTIME    MEMORY    DISK      CPUs EFCY PID     STARTER
    Inst-Proj jsmith ap20.uc.osg-htc.org 27781234.0   0+00:17:43  512.0 MB  129.0 MB    1 0.00 4124321 4123123
    …
    ```

For each glidein job, there will be one set of heading lines (“Batch System”, etc.) and a table of researcher jobs, one per line; format and definitions are as above.

OSPool Contribution Requirements

# Contributing via a Hosted CE

-   The cluster and login node are set up for our user account:
    

-   The cluster is operational and generally works
    
-   The user account has a home directory on the login node
    
-   The user account can read, write, and execute files and directories within its home directory
    
-   Our home directory has enough available space and inodes (TBD but not a lot)
    
-   PATH staff know the right partition (and other batch system config) to use
    
-   The batch system is configured to allow the user account to submit jobs to the right partition(s) and for the default job “shape” (e.g., 1 core, 2 GB memory, and 24-hour maximum run time)
    

-   It is possible to SSH from the CE to the login node:
    

-   PATh staff know the current hostname of your login node
    
-   That hostname has a public DNS entry that resolves to the correct IP address
    
-   PATh staff know the user account name (default, “osg01”)
    
-   PATh staff know about SSH configuration details to use (e.g., alternate port, jump host)
    
-   The SSH client on one of our IP addresses can connect to your login node (through firewalls, etc.)
    
-   The provided SSH public key has been installed in the right place and with the right permissions
    
-   The provided SSH public key is sufficient for authentication by your SSH server
    

-   The worker nodes on which our jobs may run are ready:
    

-   Our home directory is shared with each cluster node
    
-   PATh staff know the correct path to scratch space for jobs (ideally on each worker node, but a shared filesystem may work)
    
-   Our user account can create subdirectories and run executables in the scratch directory
    
-   The worker nodes have permissive outbound network connectivity to the Internet (default allow, please note specific restrictions)
