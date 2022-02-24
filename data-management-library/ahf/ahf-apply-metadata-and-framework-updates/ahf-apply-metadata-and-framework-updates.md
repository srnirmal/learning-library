# Apply AHF Metadata and Framework Updates

## Introduction

In this lab, you will learn how to apply AHF metadata and framework updates, and rollback to the previous timestamp when needed.

Estimated Time: 30 minutes

### Objectives

In this lab, you will:
* Apply AHF framework and metadata updates
* Query AHF framework and metadata updates
* Rollback AHF framework and metadata updates
* Cleanup AHF metadata backup directories

### Prerequisites

This lab assumes you have:
* AHF install user privileges to run the **applyupdate**, **queryupdate**, **rollbackupdate**, and **deletebackup** commands.

## Task 1: Apply AHF Framework and Metadata Updates

You must apply metadata and framework updates to all cluster nodes.

1. To update metadata and framework files on the local node from the zip file provided:

    ```
    <copy>
    ahfctl applyupdate -updatefile /tmp/ahf_data_20220203.zip
    Updated file /opt/oracle.ahf/exachk/.cgrep/collections.dat
    Updated file /opt/oracle.ahf/exachk/rules.dat
    Updated file /opt/oracle.ahf/exachk/.cgrep/versions.dat
    Updated file /opt/oracle.ahf/exachk/messages/check_messages.json
    Data files updated to 20220203 from 20211220
    </copy>
    ```

## Task 2: Query AHF Framework and Metadata Updates

Query the metadata updates using the **-all** option and the framework updates using **-updateid**.

To verify if the metadata and framework updates were applied to all nodes in a cluster, run the **ahfctl queryupdate** command as the AHF install user on each cluster node.

1. To check if an update was applied on the local node:

    ```
    <copy>
    ahfctl queryupdate -all
    AHF Metadata Update: 20220203
    Status: Applied
    Applied on: Fri Feb 4 00:47:00 2022
    </copy>
    ```

## Task 3: Rollback AHF Framework and Metadata Updates

Use the **ahfctl rollbackupdate** command to rollback the updates with a specific update ID applied to the local node. If you do not specify the update ID, then AHF rolls back to the previous state by default.

To rollback the metadata and framework updates applied to all nodes in a cluster, you must run the **ahfctl rollbackupdate** command as the AHF install user on each cluster node.

1. To rollback the metadata and framework updates:

    ```
    <copy>
    ahfctl rollbackupdate -updateid 20220203
    Data files with timestamp 20220203 identified. Rolling back the files to Production version 20211220
    Rolled back the data files 20220203 to Production version 20211220
	  </copy>
    ```

## Task 4: Cleanup AHF Metadata Backup Directories

To delete the backup directories on all nodes in a cluster, you must run the **ahfctl deletebackup** command as the AHF install user on each cluster node.

You must not delete the backup directories randomly. Oracle recommends deleting the backup directories in the same order the updates were applied. If you delete the backup directories associated with a specific timestamp, then you will not be able to roll back to the state before the updates with that specific timestamp were applied.

Upgrading AHF using the **ahf_setup script** automatically deletes the backup directories of the previous AHF versions.

1. To delete the backup directories:

    ```
    <copy>
    ahfctl deletebackup -timestamp 20220130
    Deleted metadata backup directory for: /opt/oracle.ahf/data/work/.exachk_patch_directory/.20220130_metadata_bkp
    </copy>
    ```

## Learn More

* [ahfctl applyupdate](https://docs.oracle.com/en/engineered-systems/health-diagnostics/autonomous-health-framework/ahfug/ahfctl-applyupdate.html#GUID-1C582851-0138-419D-8CBC-D9F83B97A6AC)
* [ahfctl queryupdate](https://docs-uat.us.oracle.com/en/engineered-systems/health-diagnostics/autonomous-health-framework/ahfug/ahfctl-queryupdate.html#GUID-C02F4087-184F-4EF7-B94F-8987F9E192B2)
* [ahfctl rollbackupdate](https://docs.oracle.com/en/engineered-systems/health-diagnostics/autonomous-health-framework/ahfug/ahfctl-rollbackupdate.html#GUID-63CC64FF-3D4D-425B-9484-6237D3AC3FD0)
* [ahfctl deletebackup](https://docs-uat.us.oracle.com/en/engineered-systems/health-diagnostics/autonomous-health-framework/ahfug/ahfctl-deletebackup.html#GUID-154BA5AA-40EF-45BF-8154-B4000718A35D)

## Acknowledgements
* **Author** - Nirmal Kumar
* **Contributors** -  Sarahi Partida, Girdhari Ghantiyala, Anuradha Chepuri
* **Last Updated By/Date** - Nirmal Kumar, February 2022
