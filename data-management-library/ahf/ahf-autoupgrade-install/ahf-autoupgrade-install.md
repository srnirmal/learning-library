# Installing Oracle Autonomous Health Framework

## Introduction

In this lab, you will learn how to install AHF either as **root** or as a non-root user as well as how to run AHF on SELinux-enabled systems.

Estimated Time: 30 minutes

### About <Product/Technology> (Optional)


### Objectives

*List objectives for this lab using the format below*

In this lab, you will:
* (Recommended) Install AHF on Linux or UNIX as **root** user in daemon mode
* Install AHF on Linux or UNIX as a non-root user in non-daemon mode
* Run AHF on SELinux-enabled systems

### Prerequisites (Optional)

*List the prerequisites for this lab using the format below. Fill in whatever knowledge, accounts, etc. is needed to complete the lab. Do NOT list each previous lab as a prerequisite.*

This lab assumes you have:
* Perl version 5.10 or later installed.

*Below, is the "fold"--where items are collapsed by default.*

## Task 1: (Recommended) Installing on Linux or Unix as root User in Daemon Mode

To obtain the fullest capabilities of Oracle Autonomous Health Framework, install it as **root**.

Oracle Autonomous Health Framework maintains Access Control Lists (ACLs) to determine which users are allowed access. By default, the **GRID_HOME** owner and **ORACLE_HOME** owners have access to their respective diagnostics. No other users can perform diagnostic collections.

If Oracle Autonomous Health Framework is already installed, then reinstalling performs an upgrade to the existing location.

1. Download and copy the **AHF-LINUX_<*version*>.zip** file to the required machine, and then unzip it.

2. To ensure that the environment has been set correctly, enter the following commands:

  ```
  <copy>umask
  env | more
  </copy>
  ```

3. Verify that the **umask** command displays a value of **22**, **022**, or **0022**.

  ```
	<copy>>>$ umask
  0022
	</copy>
  ```

4. Run the **ahf_setup** script:

  ```
   <copy>./ahf_setup</copy>

	 If you plan to run only Oracle ORAchk or Oracle EXAchk and do not want to run any Oracle Trace File Analyzer processes, then use the install option of **-extract -notfasetup**.

	 <copy>
	 ahf_setup -extract -notfasetup

	 AHF Installer for Platform Linux Architecture x86_64

	 AHF Installation Log : /tmp/ahf_install_221000_98374_2022_02_02-13_33_27.log

	 Starting Autonomous Health Framework (AHF) Installation

	 AHF Version: 22.1.0 Build Date: 202201302324

	 Default AHF Location : /opt/oracle.ahf

	 Do you want to install AHF at [/opt/oracle.ahf] ? [Y]|N :

	 AHF Location : /opt/oracle.ahf

	 AHF Data Directory : /opt/oracle.ahf/data

	 Extracting AHF to /opt/oracle.ahf

	 AHF is deployed at /opt/oracle.ahf

	 ORAchk is available at /opt/oracle.ahf/bin/orachk

	 AHF binaries are available in /opt/oracle.ahf/bin

	 AHF is successfully installed

	 Moving /tmp/ahf_install_221000_98374_2022_02_02-13_33_27.log to /opt/oracle.ahf/data/den02mwa/diag/ahf/
	 </copy>
   ```

	 The installation prompts you to do a local or cluster installation.

	 Whether the installation is local or cluster-wide, the script configures Oracle Autonomous Health Framework for automatic startup. The implementation of auto-start is platform-dependent. Linux uses **init**, or an **init** replacement, such as **upstart** or **systemd** and Microsoft Windows uses a Windows service.

	 The installer prompts you to specify one or more email addresses of the recipients who can receive diagnostic notifications. Oracle Autonomous Health Framework notifies the recipients with the results of Oracle ORAchk and Oracle EXAchk compliance checking, or when Oracle Autonomous Health Framework detects significant faults.

	 **Local installation:**

   ```
	 <copy>
	 ahf_setup

	 AHF Installer for Platform Linux Architecture x86_64

	 AHF Installation Log : /tmp/ahf_install_221000_103911_2022_02_02-13_38_15.log

	 Starting Autonomous Health Framework (AHF) Installation

	 AHF Version: 22.1.0 Build Date: 202201302324

	 Default AHF Location : /opt/oracle.ahf

	 Do you want to install AHF at [/opt/oracle.ahf] ? [Y]|N :

	 AHF Location : /opt/oracle.ahf

	 AHF Data Directory stores diagnostic collections and metadata.
	 AHF Data Directory requires at least 5GB (Recommended 10GB) of free space.

	 Please Enter AHF Data Directory : /opt/oracle.ahf

	 AHF Data Directory : /opt/oracle.ahf/data

	 Do you want to add AHF Notification Email IDs ? [Y]|N : N

	 Extracting AHF to /opt/oracle.ahf

	 Configuring TFA Services

	 Discovering Nodes and Oracle Resources

	 Successfully generated certificates.

	 Starting TFA Services
	 Created symlink from /etc/systemd/system/multi-user.target.wants/oracle-tfa.service to /etc/systemd/system/oracle-tfa.service.
	 Created symlink from /etc/systemd/system/graphical.target.wants/oracle-tfa.service to /etc/systemd/system/oracle-tfa.service.

	 .-------------------------------------------------------------------------------.
	 | Host     | Status of TFA | PID    | Port  | Version    | Build ID             |
	 +----------+---------------+--------+-------+------------+----------------------+
	 | den02mwa | RUNNING       | 105916 | 59452 | 22.1.0.0.0 | 22100020220130232427 |
	 '----------+---------------+--------+-------+------------+----------------------'

	 Running TFA Inventory...

	 Adding default users to TFA Access list...

	 .------------------------------------------------------.
	 |             Summary of AHF Configuration             |
	 +-----------------+------------------------------------+
	 | Parameter       | Value                              |
	 +-----------------+------------------------------------+
	 | AHF Location    | /opt/oracle.ahf                    |
	 | TFA Location    | /opt/oracle.ahf/tfa                |
	 | Orachk Location | /opt/oracle.ahf/orachk             |
	 | Data Directory  | /opt/oracle.ahf/data               |
	 | Repository      | /opt/oracle.ahf/data/repository    |
	 | Diag Directory  | /opt/oracle.ahf/data/den02mwa/diag |
	 '-----------------+------------------------------------'

	 Starting orachk scheduler from AHF ...

	 AHF binaries are available in /opt/oracle.ahf/bin

	 AHF is successfully installed

	 Do you want AHF to store your My Oracle Support Credentials for Automatic Upload ? Y|[N] : N

	 Moving /tmp/ahf_install_221000_103911_2022_02_02-13_38_15.log to /opt/oracle.ahf/data/den02mwa/diag/ahf/
	 </copy>
   ```

	 **Cluster-wide Installation:**

	 **Note:** Oracle Clusterware does not manage Oracle Autonomous Health Framework because Oracle Autonomous Health Framework must be available if Oracle Clusterware stops working.

	 Cluster installation requires passwordless SSH user equivalency for root to all cluster nodes. If you have not already configured passwordless SSH user equivalency, then the installation optionally sets up passwordless SSH user equivalency and then removes it at the end.

   ```
	 <copy>
	 ahf_setup

	 AHF Installer for Platform Linux Architecture x86_64

	 AHF Installation Log : /tmp/ahf_install_221000_13129_2022_02_03-20_53_26.log

	 Starting Autonomous Health Framework (AHF) Installation

	 AHF Version: 22.1.0 Build Date: 202202030823

	 Default AHF Location : /opt/oracle.ahf

	 Do you want to install AHF at [/opt/oracle.ahf] ? [Y]|N :

	 AHF Location : /opt/oracle.ahf

	 AHF Data Directory stores diagnostic collections and metadata.
	 AHF Data Directory requires at least 5GB (Recommended 10GB) of free space.

	 Choose Data Directory from below options :

	 1. /u01/app/giusr [Free Space : 28917 MB]
	 2. Enter a different Location

	 Choose Option [1 - 2] : 2

	 Please Enter AHF Data Directory : /opt/oracle.ahf

	 AHF Data Directory : /opt/oracle.ahf/data

	 Do you want to add AHF Notification Email IDs ? [Y]|N : N

	 AHF will also be installed/upgraded on these Cluster Nodes :

	 1. stbm000037-vm2

	 The AHF Location and AHF Data Directory must exist on the above nodes
	 AHF Location : /opt/oracle.ahf
	 AHF Data Directory : /opt/oracle.ahf/data

	 Do you want to install/upgrade AHF on Cluster Nodes ? [Y]|N :

	 Extracting AHF to /opt/oracle.ahf

	 Configuring TFA Services

	 Discovering Nodes and Oracle Resources

	 Not generating certificates as GI discovered

	 Starting TFA Services
	 Created symlink /etc/systemd/system/multi-user.target.wants/oracle-tfa.service ¿ /etc/systemd/system/oracle-tfa.service.
	 Created symlink /etc/systemd/system/graphical.target.wants/oracle-tfa.service ¿ /etc/systemd/system/oracle-tfa.service.

	 .-----------------------------------------------------------------------------------.
	 | Host           | Status of TFA | PID   | Port | Version    | Build ID             |
	 +----------------+---------------+-------+------+------------+----------------------+
	 | stbm000037-vm1 | RUNNING       | 17450 | 5000 | 22.1.0.0.0 | 22100020220203082328 |
	 '----------------+---------------+-------+------+------------+----------------------'

	 Running TFA Inventory...

	 Adding default users to TFA Access list...

	 .------------------------------------------------------------.
	 |                Summary of AHF Configuration                |
	 +-----------------+------------------------------------------+
	 | Parameter       | Value                                    |
	 +-----------------+------------------------------------------+
	 | AHF Location    | /opt/oracle.ahf                          |
	 | TFA Location    | /opt/oracle.ahf/tfa                      |
	 | Orachk Location | /opt/oracle.ahf/orachk                   |
	 | Data Directory  | /opt/oracle.ahf/data                     |
	 | Repository      | /opt/oracle.ahf/data/repository          |
	 | Diag Directory  | /opt/oracle.ahf/data/stbm000037-vm1/diag |
	 '-----------------+------------------------------------------'

	 Starting orachk scheduler from AHF ...

	 AHF install completed on stbm000037-vm1

	 Installing AHF on Remote Nodes :

	 AHF will be installed on stbm000037-vm2, Please wait.

	 Please Enter the password for stbm000037-vm2 :

	 Is password same for all the nodes? [Y]|N :

	 Installing AHF on stbm000037-vm2 :

	 [stbm000037-vm2] Copying AHF Installer

	 AHF binaries are available in /opt/oracle.ahf/bin

	 AHF is successfully installed

	 Do you want AHF to store your My Oracle Support Credentials for Automatic Upload ? Y|[N] :

	 Moving /tmp/ahf_install_221000_13129_2022_02_03-20_53_26.log to /opt/oracle.ahf/data/stbm000037-vm1/diag/ahf/
	 </copy>
   ```

	 If you do not wish to use passwordless SSH, then you install Oracle Autonomous Health Framework on each host using a local installation. Run the **tfactl syncnodes** command to generate and deploy relevant SSL certificates.

   ```
	 <copy>
	 tfactl syncnodes

	 TFA has not yet generated any certificates on this Node.

	 Do you want to generate new certificates to synchronize across the nodes? [Y]|N:

	 Generating new TFA Certificates...

	 Successfully generated certificates.

	 Restarting TFA on stbm000037-vm1...
	 Shutting down TFA
	 Removed /etc/systemd/system/graphical.target.wants/oracle-tfa.service.
	 Removed /etc/systemd/system/multi-user.target.wants/oracle-tfa.service.
	 Successfully shutdown TFA..

	 Starting TFA..
	 Created symlink /etc/systemd/system/multi-user.target.wants/oracle-tfa.service -> /etc/systemd/system/oracle-tfa.service.
	 Created symlink /etc/systemd/system/graphical.target.wants/oracle-tfa.service -> /etc/systemd/system/oracle-tfa.service.
	 Waiting up to 100 seconds for TFA to be started..
	 . . . . .
	 . . . . .
	 Successfully started TFA Process..
	 . . . . .
	 TFA Started and listening for commands

	 Current Node List in TFA :
	 1. stbm000037-vm1

	 Node List in Cluster :
	 1. stbm000037-vm1
	 2. stbm000037-vm2

	 Node List to sync TFA Certificates :
     1  stbm000037-vm2

		 Do you want to update this node list? Y|[N]:

		 Syncing TFA Certificates on stbm000037-vm2 :

		 TFA_HOME on stbm000037-vm2 : /opt/oracle.ahf/tfa

		 DATA_DIR on stbm000037-vm2 : /opt/oracle.ahf/data/stbm000037-vm2/tfa

		 Please Enter the password for stbm000037-vm2 :

		 Is password same for all the nodes? [Y]|N:

		 Shutting down TFA on stbm000037-vm2...
		 Copying TFA Certificates to stbm000037-vm2...
		 Copying SSL Properties to stbm000037-vm2...
		 Sleeping for 5 seconds...
		 Sleeping for 5 seconds...
		 Starting TFA on stbm000037-vm2...

		 .------------------------------------------------------------------------------------------------------.
		 | Host           | Status of TFA | PID   | Port | Version    | Build ID             | Inventory Status |
		 +----------------+---------------+-------+------+------------+----------------------+------------------+
		 | stbm000037-vm1 | RUNNING       | 15331 | 5000 | 22.1.0.0.0 | 22100020220203082328 | COMPLETE         |
		 | stbm000037-vm2 | RUNNING       | 11384 | 5000 | 22.1.0.0.0 | 22100020220203082328 | COMPLETE         |
		 '----------------+---------------+-------+------+------------+----------------------+------------------'
	 </copy>
   ```

## Task 2: Enabling or Disabling the Oracle ORAchk or Oracle EXAchk Daemon to Start Automatically

Installing Oracle Autonomous Health Framework as **root** on Linux or Solaris automatically sets up and runs the Oracle ORAchk or Oracle EXAchk daemon.

The daemon restarts at 1 am every day to discover environment changes. The daemon runs a full local Oracle ORAchk check once every week at 3 am, and a partial run of the most impactful checks at 2 am every day through the **oratier1** or **exatier1** profiles. The daemon automatically purges the **oratier1** or **exatier1** profile run that runs daily, after a week. The daemon also automatically purges the full local run after 2 weeks. You can change the daemon settings after enabling **autostart**.

1. To disable auto start:

  ```
  <copy>
	orachk -autostop
	Removing orachk cache discovery....
	Successfully completed orachk cache discovery removal.
	Successfully copied Daemon Store to Remote Nodes
	Removed orachk from inittab
	</copy>
  ```	 

2. To enable auto start:

  ```
	<copy>
	orachk -autostart
	.
	.
	Successfully copied Daemon Store to Remote Nodes
	.  .  .
	orachk is using TFA Scheduler. TFA PID: 11552
	Daemon log file location is : /opt/oracle.ahf/data/den00pkf/orachk/user_root/output/orachk_daemon.log
	</copy>
  ```

## Task 3: Installing on Linux or UNIX as Non-root User in Non-Daemon Mode

If you are unable to install as **root**, then install Oracle Autonomous Health Framework as the Oracle Home owner.

**Note:**
- Perl version 5.10 or later is required to install Oracle Autonomous Health Framework.
- You cannot perform the cluster-wide installation as a non-root user.

Oracle Autonomous Health Framework has reduced capabilities when you install it as a non-root user in non-daemon mode. Therefore, you cannot complete the following tasks:
- Automate diagnostic collections
- Collect diagnostics from remote hosts
- Collect files that are not readable by the Oracle Home owner, for example, **/var/log/messages**, or certain Oracle Grid Infrastructure logs

1. To install as the Oracle Home owner, use the **–ahf_loc** option, and optionally specify the **-notfasetup** option to prevent the running of any Oracle Trace File Analyzer processes.

  ```
  <copy>
	ahf_setup -ahf_loc /ahf -notfasetup

	AHF Installer for Platform Linux Architecture x86_64

	AHF Installation Log : /tmp/ahf_install_221000_101841_2022_02_02-13_36_44.log

	Starting Autonomous Health Framework (AHF) Installation

	AHF Version: 22.1.0 Build Date: 202201302324

	AHF Location : /ahf/oracle.ahf

	AHF Data Directory : /ahf/oracle.ahf/data

	Extracting AHF to /ahf/oracle.ahf

	AHF is deployed at /ahf/oracle.ahf

	ORAchk is available at /ahf/oracle.ahf/bin/orachk

	AHF binaries are available in /ahf/oracle.ahf/bin

	AHF is successfully installed

	Moving /tmp/ahf_install_221000_101841_2022_02_02-13_36_44.log to /ahf/oracle.ahf/data/den02mwa/diag/ahf/
	</copy>
  ```

	For more information, run **ahf_setup -h**.

## Task 4: Running AHF on SELinux Enabled Systems

To run AHF on SELinux-enabled systems, use this procedure.

SELinux Modes:
- **Disabled:** SElinux is disabled.
- **Permissive:** SELinux prints warnings instead of enforcing.
- **Enforcing:** SELinux security policy is enforced.

You can enable or disable SELinux. When enabled, SELinux can run either in **enforcing** or **permissive** modes. To check the status of SELinux, run the **getenforce** or **sestatus** commands. The **getenforce** command returns **Enforcing**, **Permissive**, or **Disabled**.
```
<copy>
/usr/sbin/getenforce
Permissive
</copy>
```

The **sestatus** command returns the SELinux status and the SELinux policy being used:

```
<copy>
/usr/sbin/sestatus
SELinux status: enabled
SELinuxfs mount: /sys/fs/selinux
SELinux root directory: /etc/selinux
Loaded policy name: targeted
Current mode: permissive
Mode from config file: permissive
Policy MLS status: enabled
Policy deny_unknown status: allowed
Memory protection checking: actual (secure)
Max kernel policy version: 31
</copy>
```

**Installing AHF in Permissive or Enforcing Mode**

AHF installer loads the policy and sets relevant contexts.

**Installing AHF in Disabled Mode**

AHF is installed successfully. However, later if you switch the mode to **Permissive** or **Enforcing**, then SELinux starts blocking the AHF processes.

1. To run AHF, load the SELinux policy:

    ```
	  <copy>
		ahfctl loadpolicy
		Checking if policy exists
		Please wait while the policy is being loaded, it might take couple of minutes.
		Successfully loaded SELinux policy
		Restarting TFA...
		</copy>
    ```
2. To check if the policy is loaded successfully:

    ```
	  <copy>
		/usr/sbin/semodule -l | grep inittfa-policy
		</copy>	 
    ```

3. To unload the SELinux policy:

    ```
	  <copy>
		ahfctl unloadpolicy
		Please wait while the policy is being removed, it might take couple of minutes.
		Successfully removed Contexts and Policy
		</copy>
    ```

## Learn More

*(optional - include links to docs, white papers, blogs, etc)*

* [Installing and Upgrading Oracle Autonomous Health Framework](https://docs.oracle.com/en/engineered-systems/health-diagnostics/autonomous-health-framework/ahfug/install-upgrade-ahf.html#GUID-663F0836-A2A2-4EFB-B19E-EABF303739A9)
* [ahfctl setupgrade](https://docs-uat.us.oracle.com/en/engineered-systems/health-diagnostics/autonomous-health-framework/ahfug/ahfctl-setupgrade.html#GUID-0AA4D7BE-781D-4345-BC77-A38AF10826BB)
* [ahfctl unsetupgrade](https://docs-uat.us.oracle.com/en/engineered-systems/health-diagnostics/autonomous-health-framework/ahfug/ahfctl-unsetupgrade.html#GUID-7757592D-7E68-44EB-9ED0-14731146CFF6)
* [ahfctl getupgrade](https://docs-uat.us.oracle.com/en/engineered-systems/health-diagnostics/autonomous-health-framework/ahfug/ahfctl-getupgrade.html#GUID-436F6822-FA11-4BE7-B28A-B8F0D9C01F97)
* [ahfctl upgrade](https://docs-uat.us.oracle.com/en/engineered-systems/health-diagnostics/autonomous-health-framework/ahfug/ahfctl-upgrade.html#GUID-7EB170D6-DC9F-4EE3-9DD8-B5374B856179)
* [Oracle Autonomous Health Framework Installation Command-Line Options](https://docs.oracle.com/en/engineered-systems/health-diagnostics/autonomous-health-framework/ahfug/install-ahf.html#GUID-F57C15E1-B82A-42A1-B064-B6C86639799F)

## Acknowledgements
* **Author** - Nirmal Kumar
* **Contributors** -  Sarahi Partida, Girdhari Ghantiyala, Anuradha Chepuri
* **Last Updated By/Date** - Nirmal Kumar, February 2022
