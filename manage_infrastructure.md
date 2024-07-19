# Managing the infrastructure and storage of IBM Storage Fusion HCI

You can manage your appliance infrastructure by up sizing compute nodes and storage disks. In addition, you can administer and configure aspects of storage integration with the IBM Storage Fusion HCI System offering.

## Administering nodes and racks

Administering nodes in the system includes day-to-day operations such as power-on, power-off, placing node in maintenance, upscale, and up size storage disks. You can add new racks to your cluster or edit its details.

### Administering the node

Administering the nodes in the system includes day-to-day operations such as monitoring the health, putting a node into maintenance mode, performing power operations, and doing firmware updates whenever it is available.

#### - Configured and Discovered node tabs
From the IBM Storage Fusion menu, click Infrastructure > Nodes. The Node page includes Configured nodes and Discovered nodes tabs. By default, the Node page opens the Configured nodes tab.

Go to Discovered nodes tab to add discovered nodes in a rack that were not added to the OpenShift cluster earlier.

#### - Node details

| Detail            | Description |
|-------------------|-------------|
| Name              | The name of the node. Naming convention of nodes added by installation or upsize: compute-\<rackid\>-\<rack unit number\>.\<domainname\> For a single rack rackid is always 1. |
| Hardware status   | The node Status column that is shown on the node inventory page is the node hardware monitoring state on the IBM Storage Fusion user interface. A node is monitored by connecting to its remote management module. Hence, the node state is a reflection of its connectivity to the monitoring module and its ability to fetch the monitoring data. |
| Firmware          | The firmware version of the node. |
| Type              | The options are Compute only, Compute storage, AFM, or GPU. |
| S/N               | The hardware serial number. |
| Rack              | The name of the rack. |
| Rack unit         | The rack unit position of the node in the rack. |
| CPU cores         | The amount of CPU cores. |
| Memory (GB)       | The amount of memory in gigabytes (GB). |
                                                                                         
#### - Node power operations
Node actions from the ellipsis overflow menu of a node record:
- Enable maintenance or Disable maintenance When the node is moved to maintenance succesfully, the ellipsis overflow menu option shows Disable maintenance option and the Enable maintenance option is available when the node is not already in maintenance.
- Power operations:
Alternatively, you can click the Manage resources in the node details > Inventory tab page and then do power operations.
Power on and power off operations on a node:
1. Move the node to maintenance.
2. In the ellipsis menu of the node, click Power off node to power off the node.
3. In the confirmation window, click Power off.
4. In the ellipsis menu of the node, click Power on node to power on the node.
5. In the confirmation window, click Power on.
6. After you complete the maintenance operations, from the ellipsis overflow menu of the node, click Power on node to power on the node.
Restart and shutdown operations on a node
1. Move the node to maintenance.
2. In the ellipsis menu of the node, click Restart node or Shutdown node. Alternatively, you can click the Manage resources in the node details > Inventory tab page and then do power operations.

- Enable and disable node maintenance
When you put a node into maintenance mode, it marks the node to Scheduling disabled and also drains workload from it. You can place a node to maintenance from the user interface or through any operation that needs a server reboot.
To run power operations, place a node in maintenance mode.
The following procedure guides you with the steps to enable or disable maintenance mode on a node from the user interface.
Procedure:
1. In the Nodes page, click the ellipsis menu of the node you want to move to maintenance and click Enable maintenance mode. Alternatively, you can click the Manage resources in the node details > Inventory tab page.
2. In the Enable maintenance mode confirmation window, click Enable.
3. Wait for node to go to maintenance mode.
4. After all the required maintenance operations are completed, move the node out of maintenance. From the ellipsis overflow menu of a node that is in maintenance, select Disable maintainance mode option.
Maintenance node operation timeout is configurable now. The default value is 15 minutes and configurable time can go up to 40 minutes. Edit the
MaintenanceWarningTimeout value to configure the maintenance operation.

- Firmware upgrade
If firmware upgrade is available for a node, then click the ellipsis overflow menu of the node record and click Upgrade firmware. You can also select multiple nodes at a time and click Upgrade button.

Failure on a node:
If upgrade fails on a node, then click the ellipsis overflow menu of the node record and click Cancel upgrade. The Retry upgrade option is enabled.
Note: Upon clicking Cancel upgrade, the firmware upgrade failed state changes to upgrade available.
If a node is queued up for upgrade or scheduled for upgrade, then the Cancel upgrade option is available in the menu.
1. If you click Cancel upgrade on a node that is queued up for upgrade, then the node is removed from the upgrade queue.
2. The cancel option for an ongoing firmware upgrade is not allowed.
Failure when multiple nodes are selected:
When you choose multiple nodes for upgrade and a node fails in between, then the rest of the nodes in the sequence changes to Scheduled for the upgrade state.
Fix the issue in the failed node and click Retry upgrade to complete the upgrade on the following nodes:
1. Problematic node
2. Rest of the nodes queued for upgrade, which are in Scheduled state

- Add disks
Click Add disks to upsize disks.

- Add racks
Click Actions and select Add racks to expand your IBM Storage Fusion HCI System system with an additional rack. 

- Upsize nodes
Go to Discovered nodes tab to add discovered nodes in a rack that were not added to the OpenShift cluster earlier.

### Scaling the Cluster

As workloads in the cluster increase, you must add more CPU, memory, GPU, or storage to accommodate them. CPU and memory can be increased by adding additional compute nodes to the cluster. For workloads that require GPUs, you can add specialized GPU nodes to the cluster. Storage can be scaled by adding additional disks to existing storage nodes, or adding additional storage nodes to the cluster. If you want to scale your cluster beyond a single rack, then you can add up to two expansion racks, enabling your cluster to host more compute, storage, and GPU nodes.

#### - Adding nodes to the cluster
After installing new node hardware in an existing rack, or adding an additional rack of hardware, you complete a procedure to configure the nodes so that they are monitored and added to the OpenShift® cluster.
##### - Configuring nodes for management:
How to configure the newly installed nodes so that they are monitored and added to the OpenShift cluster. For nodes that contribute to storage or Scale-related activities, add them to the storage cluster.
Meet the following prerequisites for all node types in the specified sequence:
1. Important: Before you begin the node upsize, make sure that all switches are up and running. Run the vgen script before you start the installation. If any TOR is down, the installation will fail as it is the only switch that supports PXE boot to avoid network looping. The lacp-bypass is not enabled on both the switches.
2. The purchased quantity of nodes are delivered to your location with the following information:
- For Compute-storage nodes, the received new node must have a specific number of NVMe drives. That is, the new node must have the same number of disks as the ones that are already there.
- For the Compute-only nodes, the node must have the same hardware configuration as the ones that are already there.
- All new nodes must have a link local IP address and MAC address from IBM.
Note: Update your Dynamic Host Configuration Protocol (DHCP) and Domain Name System (DNS) Configuration to include reservations for all nodes in the
IBM Storage Fusion System, which includes control, compute, AFM, and GPU. These configurations are based on the MAC addresses shared with you by IBM.
For more information, see Setting up the DNS and DHCP for IBM Storage Fusion appliance.
3. Ensure that the IBM Support Representative inserted the nodes in the appropriate rack slots and connected the network and power cables.
4. The IBM Storage Fusion appliance is up and running.
5. Red Hat® OpenShift® is up and running and the existing nodes are healthy.
Note: It is not applicable for node replacement.
6. When you add a node to Red Hat OpenShift, the operation might get stuck for a long time in the node network configuration policy creation stage. To avoid this issue, do the steps that are mentioned in the troubleshooting section before you add the node. For the workaround steps, see Node issues.
7. Naming convention of nodes that are added:
```
Nodes added by installation
compute-<rackid>-<rack unit number>.<domainname>
Nodes added as upsize nodes on Day 2
compute-<rackid>-<rack unit number>.<domainname>
For a single rack, the rackid is always 1.
For an example of the base system, see host name conventions.
```

Upsizing when backup or restore is in progress
When a backup or restore job is in progress, you must not upsize the nodes, though backup or restore jobs might not cause scale-out or scale-up operations to fail.
- When you want to do a scale-out or scale-up operation, turn off the IBM Storage Fusion backup or restore jobs, if any. Do not back up or restore when an upsize action is in progress.
- Disable the IBM Storage Fusion backup or restore job from the IBM Storage Protect Plus user interface before you proceed with a scale-out or scale-up operation.
- Before you start a scale-out or scale-up operation, back up the components, storage, and workloads for IBM Storage Fusion. For more information about backup, see Backup and Restore.
- When the scale-out or the scale-up operation is complete, back up the components, storage, and workloads again for IBM Storage Fusion.

Upsize nodes to brand-new racks
1. Upsize node must be powered on only on a running IBM Storage Fusion HCI System. If upsize node is installed on a new rack that is not configured with OpenShift Container Platform, then keep the upsized node powered off (remove the power cables from the node).
2. Complete the installation.
3. Plug-in the power and switch on the node.

1. After a node is physically added to a rack, networking is done, and powered on, log in to IBM Storage Fusion HCI System user interface.
2. Go to Infrastructure > Nodes.
3. Check whether the node you added to the physical hardware is available in the Discovered nodes tab. If there are any errors in the configuration, then Error connecting to node message gets displayed in the State column. All of the error conditions have builtin retries. Whenever the event persists for a longer duration, support tickets get raised to check the appliance. If the condition is transient, it recovers automatically. Otherwise, IBM Support checks the appliance.
4. To add a node to the OpenShift cluster, click the add icon. To add more than one node, select the nodes and click Add to cluster.
5. In the Add node window, go through the node name and serial number of the nodes to include in the cluster and click Add to confirm.
6. Go to the Configured nodes tab to see whether the newly added node is in the list with the State as Adding to OpenShift.
7. When all the nodes are ready for storage configuration, add them to the storage cluster: This final step allows workloads on the nodes to access storage, so complete this step before stateful workloads are scheduled on the nodes.

| Option                  | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|-------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Global Data Platform    | To configure node for Global Data Platform, do the following steps: <br> a. Go to the Global Data Platform page. The following notification is displayed: Nodes are ready for storage. The Storage status of the nodes in the Nodes list is Ready for storage and the Type is Storage. <br> b. Click Configure nodes link in the notification. <br> c. In the Configure \<node type\> nodes window, click Install. The Storage status of the nodes in the Nodes list changes to Installing Storage software. After the configuration is complete, a success notification is displayed and the Storage status of the node changes to Ready. <br> d. Repeat steps for the remaining available node types one by one. |
| Fusion Data Foundation  | To configure node for Fusion Data Foundation, do the following steps: <br> a. Go to the Data Foundation page. The following notification is displayed: Nodes are ready for storage. The Storage status of the nodes in the Nodes list is Ready for storage and the Type is Storage. <br> b. Click Configure nodes link in the notification. <br> c. In the Configure \<node type\> nodes window, click Install. The Storage status of the nodes in the Nodes list changes to Installing Storage software. After the configuration is complete, a success notification is displayed and the Storage status of the node changes to Ready. <br> d. Repeat steps for the remaining available node types one by one. |

8. To verify, do the following steps:
    a. Go to the node details page.
    b. In the Inventory tab, go to Disks tab.
    c. Check the new size. Also, check whether the disk information reflects the newly added drive.
    d. Check the total disk count and Raw capacity after you add the disk.
        i. Go to Infrastructure > Dashboard.
        ii. In the Disks tile, check total disk count and Raw capacity.

##### - Adding expansion racks:
You can expand your IBM Storage Fusion HCI System with an additional rack.
1. From the IBM Storage Fusion HCI System menu, click Infrastructure > Nodes.
2. Go to Actions and click Add rack. For Compute-storage nodes, the nodes on the auxiliary rack must have a minimum of two NVMe or same number of NVMes as that of the base rack. The Add rack wizard page is displayed.
3. In the Getting started wizard page, go through the details and click Next to initiate the discovery process.
4. In the Discovery wizard page, wait for the rack discovery to complete. In this page, IBM Storage Fusion HCI System enables expansion of rack, connects to the spine switch, connects to the provisioned node, and discovers rack data. Errors might occur during the discovery step because of the following reasons:
- Unable to connect to the spine switch
- Unable to establish connection to the provisioned node on the new rack. To resolve this issue, revisit your DNS configuration and retry.
- Unsupported rack configuration. For example, the new rack contains six storage nodes, but eleven storage nodes are required as the existing storage configuration is using 8 + 3p erasure coding. Contact your IBM Support to identify the supported configuration. There are Retry and Cancel options on this page. If the discovery fails, it can be retried by clicking retry and if you wish to cancel, click Cancel.
5. Click Next.
6. In the Validation wizard page, check whether all the nodes are validated properly. The IBM Storage Fusion HCI System validates the configuration of each node as a preparatory step so as to join the cluster. A minimum of six nodes are required to add the rack successfully. This page continuously shows the number of nodes that are successfully validated. If the validation does not complete within the stipulated time, the validation operation fails and message corresponding to that gets displayed. If issues exist, then check your connections or fix problematic nodes and retry. You cannot proceed with errors, so click Save & Quit. Click Collect logs to download and view logs. In this page, if rack addition fails with the following error message, then contact IBM Support:
```
Rack import timedout
```
The Retry on this page refreshes the node validation status.
7. If the session is lost during the add rack operation, do the following steps:
a. Log in to IBM Storage Fusion HCI System user interface.
b. Go to Infrastructure > Nodes.
A Scale out operation not completed notification is displayed on the page.
c. Click Resume process link in the Scale out operation not completed notification.
The Resume process allows you to continue the add rack operation from where you left.
For any errors in rack addition due to timedout issues, go to the Infrastructure > Nodes > Discovered nodes tab page to know more details.
8. Click Next. The optional Node selection wizard page is displayed.
9. Optionally, in the Node selection wizard page, you can open the Advanced options and edit node assignments. By default, IBM Storage Fusion HCI System automatically configures all nodes in the rack. The Advanced options shows a table where you can deselect nodes. If you do not deselect, then the nodes get added automatically to the OpenShift® cluster. Your deselection depends on the amount of nodes required for your storage building block. For example, you have twenty nodes and six nodes are required for storage building block. The table displays only the remaining fourteen nodes. Also, a summary section is displayed with the following details:
- Nodes
- CPUs
- Memory (GB)
- Usable capacity (TiB)
The values of the summary section changes dynamically based on your selection and deselection.
10. Click Next. The Finish wizard page is displayed. The IBM Storage Fusion HCI System adds these nodes to the OpenShift cluster.
11. In Finish wizard page, click Finish to close the Add rack wizard. After the add rack operation is complete, the user interface shows auxiliary rack related nodes in the Discovered node tab. To add more racks, click Actions > Add rack again and follow the steps mentioned in this procedure.
12. Select nodes and click Add to cluster. The IBM Storage Fusion HCI System starts to add nodes to the OpenShift cluster. To know the progress of the OpenShift cluster addition, go to Configured nodes tab. If error occurs at this stage, a link is available to view nodes and errors in the error notification. For example, OpenShift experienced a timeout error. You can also click Retry to initiate the process. After the nodes are successfully added to OpenShift, the State changes to Ready for storage.
13. When all the nodes are ready for storage configuration, click Add nodes to the storage cluster link in the notification. If you are not in the Nodes page, then a global notification is displayed and you can click the Configure storage link and return to the Nodes page. The State of the node changes to Installing storage software. After all the nodes are successfully added to storage, the following success notification is displayed and the State of the node changes to Running:
```
Rack expansion complete
All nodes are available for scheduling workloads
```

#### -  Adding storage
You can add additional storage nodes or add disks to existing nodes.

- Adding additional storage nodes
    As the disk slots fill up on existing storage nodes in the cluster, you can add additional storage nodes to increase the amount of available storage. Note that all storage nodes must have an equal number of disks, so new storage nodes must match the disk configuration of the storage nodes that are already in the rack.

- Adding new disks to existing Global Data Platform nodes
    If storage gets added at the hardware level, then the new storage information is available on the IBM Storage Fusion HCI System user interface. You can add storage to the cluster by adding additional disks to the storage nodes in the cluster. Each storage node must have the same number of disks, so make sure to add an equal number of disks to each node in the cluster.

    Ensure that the following conditions are met:
    - No node is in maintenance mode.
    - Nodes do not get rebooted during disk upsize.
    - Do not initiate an upgrade of OpenShift®, IBM Storage Fusion HCI System, or node firmware until the new storage addition is complete.
    - Number of disks is the same for all servers in the storage cluster.
    If you want disk upsize, then add same number of disks to all storage nodes available in the cluster. For example, a cluster has a base minimum of six storage
    nodes, then at least 6*2=12 disks with two disks each must be added. For nodes, if the cluster has six storage nodes with two disks each, you can add one more
    node with two disks. Now, your storage cluster has seven storage nodes with two disks each.
    - Storage nodes must have an even number of disks.
    - During installation or upgrade, the pod may get stuck in container creating state and the crash loop might fail with attach volume. For such error scenarios, do
    the following workaround steps:
        1. Unschedule the node from which you were trying to attach volume.
        2. Delete the pod that got stuck in container creating state.
        3. Run upsize again.
    - For hardware configuration of compute-storage or compute-only nodes, see Hardware overview of a single rack.

    Procedure:
    1. In the IBM Storage Fusion menu, go to Global Data Platform depending on your storage type.
    2. Click Actions > Add disk.
    A window opens with details of the newly added storage at the hardware level.
    If you are not able to see additional disks even after 1 hour, contact IBM Support.
    3. Go through the storage details and click Add.
    Note: It is a best practice to add a bunch of storage nodes at a time and not one-by-one.
    The following message is displayed:
    Request accepted successfully
    All new disks on all nodes get added automatically and disk selection is not required.
    4. To verify, go to the node details page and check the Disks tab. For more information about the procedure, see Node details.

- Adding new disks to existing Fusion Data Foundation nodes
    After you add additional disks to Fusion Data Foundation storage nodes, it auto detects the new disks and auto expands the capacity to Fusion Data Foundation storage cluster. Each storage node must have the same number of disks, so make sure to add an equal number of disks to each node in the cluster.
    Ensure that the following conditions are met:
    - No node is in maintenance mode.
    - Nodes do not get rebooted during disk upsize.
    - Do not initiate an upgrade of OpenShift®, IBM Storage Fusion HCI System, or node firmware until the new storage addition is complete.
    - Number of disks is the same for all servers in the storage cluster.
    If you want disk upsize, then add same number of disks to all storage nodes available in the cluster. For example, a cluster has a base minimum of six storage
    nodes, then at least 6*2=12 disks with two disks each must be added. For nodes, if the cluster has six storage nodes with two disks each, you can add one more
    node with two disks. Now, your storage cluster has seven storage nodes with two disks each.
    - Storage nodes must have an even number of disks.
    D- uring installation or upgrade, the pod may get stuck in container creating state and the crash loop might fail with attach volume. For such error scenarios, do
    the following workaround steps:
        1. Unschedule the node from which you were trying to attach volume.
        2. Delete the pod that got stuck in container creating state.
        3. Run upsize again.
    - For hardware configuration of compute-storage or compute-only nodes, see Hardware overview of a single rack.

    Procedure:
    1. In the IBM Storage Fusion menu, go to Data Foundation.
    Wait for the new capacity to be shown in the Data Foundation page.
    2. To verify, go to the node details page and check the Disks tab. For more information about the procedure, see Node details.

### Upgrading node firmware

Before you begin:
- Upgrade the IBM Storage Fusion HCI System management software.
- Post the upgrade of IBM Storage Fusion HCI System management software, check whether firmware upgrade exists by using any of the following methods:
Open the Red Hat® OpenShift® Container Platform web console. In the Overview page, go to Activity > Recent events section, see whether you can see the
following message:
```
IBM Fusion firmware upgrade available for nodes.
```
- Open the IBM Storage Fusion HCI System user interface displays notifications in the following locations:
    - The message is displayed in the Quick start page of IBM Storage Fusion user interface:
    ```
    Firmware upgrade available
    An upgrade (version <version_number>) is available for the nodes
    ```
    - In the bell icon, you can see the following messages for the node:
    ```
    Compute firmware upgrade available
    ```
    - In the Infrastructure > Nodes, you can see the following message:
    ```
    Version <version_number> firmware upgrade is available
    ```
- Ensure all the nodes must be in a running state before you start the upgrade.

Procedure:
1. Open the IBM Storage Fusion HCI System user interface.
2. Go to Infrastructure > Nodes.
3. In the Configured nodes tab, select the nodes that you want to upgrade.
    As soon as you select the nodes, the Upgrade button is displayed.
    Note: The Cancel button on the nodes page appears whenever you make any node selection from the node list. The functionality of the Cancel button is to deselect all the selections made earlier.
4. Click Upgrade.
    The Upgrade firmware window gets displayed. The IBM Storage Fusion HCI System runs prechecks on the health of the cluster operator, compute health, compute
    maintenance, DNS resolution, Fusion operator status, machine configuration pool status, and registry accessibility.
5. If there are blocker issues, do the following steps:
    a. To know more about blockers or warnings, click View details link.
    The IBM Storage Fusion HCI System user interface takes you to the Nodes page wherein a Upgrade pre-check report window displays all the Blocking issues
    and Issues (optional). If the upgrade fails, check the updateStatus in the computefirmware CR.
    b. Click on the corresponding BMYxxxxxx code to know more about how to fix the issue.
    If you find error resources in the Resources section of the individual issues, you can click the resource link to know more details about it.
    c. Fix all the blocking issues based on the guidance.
    If the upgrade fails with a message Failed to perform power on or off operation on the node in compute-firmware CR status, then you
    need to restart the node manually from the IBM Storage Fusion HCI System user interface or Openshift > Compute > Bare Metal Hosts. For more information
    about power operation, see Administering the node.
6. After the pre-checks complete without any blocker issues, click Upgrade now.
    To know more details about the upgrade that is progress, click View details in the notification tab of the Services page. A slide out pane displays the following
    details:
    - Upgrade pre-check. As this is complete, a check mark appears and the status shows complete.
    - Firmware upgrade in progress.
    If a firmware update fails for a node in each set of nodes, the remaining nodes are not updated until the node is fixed to a ready state. Though you select multiple
    nodes, they are upgraded one-by-one.
    Before the actual upgrade, the node is automatically moved to maintenance mode. To know more details about the upgrade that is in progress, click View details in the notification tab of the Nodes page.
    The following table lists the various node states that provide information about the firmware upgrade:

    | Firmware State               | Description                                                                                           |
    |------------------------------|-------------------------------------------------------------------------------------------------------|
    | \<version\>                  | Current firmware version                                                                              |
    | \<version\> - Upgrade Available | Indicates that an upgrade is available and displays the current firmware version.                      |
    | \<version\> - Upgrade scheduled | Indicates that an upgrade is scheduled for the next firmware version.                                |
    | \<version\> - Upgrade in progress | Indicates that an upgrade is in progress for the next firmware version.                            |
    | \<version\> - Upgrade failed     | Indicates that an upgrade failed for the next firmware version.                                    |

### Upgrading memory in the nodes of an IBM Storage Fusion HCI System System

Before you begin:
- Installing the memory upgrade requires temporarily shutting down each of the compute nodes that are being upgraded. Ensure all the nodes in the IBM Storage Fusion HCI System system are functioning properly so that the system can tolerate the shutting down of compute nodes without impact on running applications or the storage cluster.
- The system must have sufficient capacity to run all applications with one compute node shutting down. If not, close some applications until the upgrade is complete.
- Before you begin the task of installing the memory upgrade, identify the compute nodes in the rack enclosure to be upgraded. Ensure that each compute node has enough DIMMs for the upgrade. Only 9155-C00, 9155-C01, 9155-C04, 9155-C05, 9155-C10, and 9155-C14 compute nodes are supported by the memory upgrade. The 9155-G01 GPU nodes and the 9155-F01 AFM nodes do not support this memory upgrade. It is likely that more than one compute node in the IBM Storage Fusion HCI System system is being upgraded. Because each compute node that is upgraded must be powered off before the update. Also, it is important that only one compute node is upgraded at a time to maintain the integrity of the IBM Storage Scale ECE storage cluster and the OpenShift® Container Platform control plane. The only exception is when the entire system is powered down to perform upgrades.
- A Philips screwdriver is used to turn the locking screw on the cover latches of the compute-only nodes (9155-C10, 9155-C00, 9155-C04) and storage-compute nodes (9155-C14, 9155-C01, 9155-C05).
- Observe all normal safety precautions. Refer to the safety notices provided in the feature kit.
- Before powering off the compute nodes, move the node to maintenance mode. For the steps to move to maintenance mode, see Administering the node.
- On the node details page and on the Overview tab, view the health of events, disks, and ports. The green (normal) tick mark indicates that the status is healthy. If a red or yellow indicator appears, do not proceed. You must correct the error or warning to safely continue with the upgrade.

Procedure:
After successfully placing a compute node in maintenance mode follow the steps to upgrade.
1. Power down the compute node. For the steps to move to power down, see Administering the node.
2. Move the compute node into position for the upgrade.
    a. Open the rear door of the IBM Storage Fusion HCI System system and identify the compute node that was shut down.
    b. Check that the four optical Ethernet network cables (2x 100GbE and 2x 25GbE), two RJ45 copper Ethernet cables (one in an OCP port, the other in the IMM),
    and the two power cables have correct labels. If the labels are damaged or missing, add replacement labels that indicate the compute node and port where
    the cable is attached. When all cables have the correct labels, disconnect the cables from the compute node. Be careful not to dislodge any cables or power
    cords from any other component in the system during this process.
    Note: It is possible that there are no 25GbE optical network cables attached to the compute node.
    c. Move to the front of the system, open the door and locate the powered down compute node.
    d. Unlatch the catches on the sides of the compute node that hold the rails, and then pull the compute node forward until the rails are fully extended.
    e. Remove the compute node from the rails and then place the compute node securely on the workbench. To remove the compute node from the rails, follow
    the instructions that are shown in the “System Removal” illustration as a guide.
3. Remove the compute node top cover as follows:
    Note: Ensure you wear an ESD wrist strap, while removing the compute node top cover.
    a. Turn the lock screw on the cover latch to the open position.
    b. Press the blue button.
    c. Lift the cover latch.
    d. Slide the cover toward the back of the compute node until it detaches from the chassis, remove it and place the cover in a safe place.
4. Install the Feature Code AHJK or AHJN memory module as follows:
    The FC AHJK or AHJN memory upgrade requires only adding more DIMMs to the compute node. The compute node already has 16 x16GB DIMMs installed. At the
    end of the installation, ensure that all 32 memory slots of the compute node are filled with 16GB DIMMs.
    To open the server and add more memory, six network cables (two Ethernet RJ45 cables, two 25G split cables and two100G fiber cables) and two power cords
    must be removed. After memory chips are added, these eight cables must be rewired:
    - 25G ports- split cables have to be firmly pressed in order for them to be securely seated to the cage. If the connection become loose, then the port does not work.
    - Handle 100G fiber cables with great care as the fiber cables bend and break easily. Take care when you work with existing memory chips as it may get loose and wires inside the server may get disturbed. Some times, one has to use force to install the memory chip that can disturb near by wires as well.
    a. Locate the components to be installed. You need 16 of the 16GB DIMMs for each of the compute nodes to be upgraded.
    b. Verify that 16 DIMMs are already in the compute node and occupy the following slots as follows: 1, 3, 5, 7, 10, 12, 14, 16, 17, 19, 21, 23, 26, 28, 30, 32
    c. Remove the air baffle as shown in figure 5 and 6.
    Note: Ensure you wear an ESD wrist strap, while removing the air baffle.
    d. Add 16 more 16GB DIMMs to the empty slots in the following slot order: 13, 29, 15, 31, 4, 20, 2, 18, 9, 25, 11, 27, 8, 24, 6, 22.
    For each slot, open the retaining clips at each end of the slot and then firmly press the new DIMM at both ends down into the slot, causing the tabs to snap to
    the locked position.
    e. After adding the 16 DIMMs, verify that all DIMM slots are now populated.
    f. Install the air baffle.
5. Install the RPQ 8S1881 memory module as follows:
    The RPQ 8S1881 memory upgrade removes all 16 of the 16GB DIMMs from the compute node and replace them with 16 x 64GB DIMMs in the same slots where
    the 16GB DIMMs removed.
    a. Locate the components to be installed. You need 16 of the 64GB DIMMs for each of the compute nodes to be upgraded.
    b. Verify that 16 DIMMs are already in the compute node and occupy the following slots as follows: 1, 3, 5, 7, 10, 12, 14, 16, 17, 19, 21, 23, 26, 28, 30, 32.
    c. Remove the air baffle as shown in figure 8 and 9.
    Note: Ensure you wear an ESD wrist strap, while removing the air baffle.
    d. Remove the 16 x 16GB DIMMs that are already in the compute node and should occupy the following slots: 1, 3, 5, 7, 10, 12, 14, 16, 17, 19, 21, 23, 26, 28,
    30, 32.
    Open the retaining clips at each end of the DIMM slot and then lift the DIMM straight up while holding both ends of the DIMM.
    To remove the memory module, see https://www.youtube.com/watch?v=XhMSuvGtEqw.
    e. Add the 16 x 64GB DIMMs to the empty DIMM slots in the following slot order: 14, 30,16, 32, 3, 19, 1 ,17, 10, 26, 12, 28, 7, 23, 5, 21. For each slot, open the
    retaining clips (if not already open) at each end of the slot and then firmly press the new DIMM at both ends down into the slot, causing the tabs to snap to
    the locked position.
    To install the memory module, see https://www.youtube.com/watch?v=_N2LLsHk7lI.
    f. Refer to the DIMM installation order label to ensure all the DIMMs are installed in the dedicated slots as per the order.
    g. After adding the 16 DIMMs, verify that the following DIMM slots are now populated: 1, 3, 5, 7, 10, 12, 14, 16, 17, 19, 21, 23, 26, 28, 30, 32.
6. Replace the compute node top cover as follows:
    Note: Ensure you wear an ESD wrist strap, while replacing the compute node top cover.
    a. Ensure that the cover latch is in the open position.
    b. Position the cover over the chassis and then slide the cover forward.
    c. Press the cover latch down until it is closed.
    d. Turn the lock screw on the cover latch to the closed position.
    To replace the compute node top cover, see https://www.youtube.com/watch?v=7SVNkf6FIfU.
7. Place the compute node back into its operating position.
    If the compute node is removed from the rack, place the compute node back on the rails using the “System Installation” illustration shown on the label on top of the compute node.
    a. Slide the compute node back into position in the rack enclosure and reconnect the power cables. Labels on the power cables indicate the rack position of the compute node where the cables should be installed.
    b. If required, verify the memory using the rack-mounted console.
    c. Connect the video, keyboard, and mouse of the rack-mounted console to the compute node.
    d. Press F1 while powering up the compute node to go into the system configuration utility.
    e. Select System Summary from the left menu and scroll down to view the DIMM information and verify that the DIMM Total Count and DIMM Total Capacity have the expected values.
    f. Power down the compute node.
    g. Reconnect the network cables. Ensure that compute nodes are installed on the dedicated racks using labels. Labels on the network cables indicate the rack position of the compute node where the cables should be installed.
8. Power up the compute node as follows:
    After a compute node has been successfully upgraded, power up using these steps:
    a. In the Manage resource window, select the Power up option from the Resource action list. (This may take a few minutes.)
    b. Take the compute node out of maintenance mode. Ensure that the success notification is displayed.
9. Repeat the steps for the other compute nodes in the rack enclosure.
    Repeat from the step “Before powering off the compute node” for each of the compute nodes in the IBM Storage Fusion HCI System system that is being upgraded.
10. Verify the new configuration.
    Check the amount of memory shown in the management GUI for each compute node and compare it to the estimated amount, which should be 512GB (FC AHJK),
    1024GB (RPQ 8S1881) or 2048GB (AHJN).

### Converting compute node to AFM node

Convert a compute node to a AFM node through backend CR.

Before you begin:
- Compute only node must be a part of the storage cluster. To upsize the node to the storage cluster, see Configuring nodes for management.
- The Global Data Platform service must be healthy before you begin the node conversion process.

Procedure:
1. Run the following command to provide a name of the node in the spec section > nodeToBeDesignatedAsAfm field of the storagemanager custom resource.
    ```
    oc patch Scale storagemanager -n ibm-spectrum-fusion-ns --type='json' -p='[{"op": "add", "path":
    "/spec/nodeToBeDesignatedAsAfm", "value": "<compute-only_node_name>"}]'
    ```
2. Run the following command to verify whether the storagemanager custom resource field is updated with the provided node name:
    ```
    oc get Scale storagemanager -n ibm-spectrum-fusion-ns -oyaml | grep nodeToBeDesignatedAsAfm
    ```
    The progress can be tracked through the status field userDesignatedAfmNodeStatus on the storagemanager custom resource.
    ```
    oc get Scale storagemanager -n ibm-spectrum-fusion-ns -oyaml | grep userDesignatedAfmNodeStatus
    ```
    Example output:
    ```
    userDesignatedAfmNodeStatus: IN-PROGRESS
    ```
    After you successfully convert the node from Compute to AFM, the status of the userDesignatedAfmNodeStatus in the storagemanager custom resource
    displays as COMPLETED.
    ```
    oc get Scale storagemanager -n ibm-spectrum-fusion-ns -oyaml | grep userDesignatedAfmNodeStatus
    ```
    Example output:
    ```
    userDesignatedAfmNodeStatus: COMPLETED
    ```
    After you complete the node conversion process, the node name gets added to the designatedAfmNodes field in the status section.
3. Run the following command to obtain a list of all the converted nodes:
    ```
    oc get Scale storagemanager -n ibm-spectrum-fusion-ns -o=jsonpath='{.status.designatedAfmNodes[*]}'
    ```
4. Run the following command to verify the role of the node:
    ```
    oc get node <node_name> -oyaml | grep 'scale.spectrum.ibm.com/role'
    ```
    Example command:
    ```
    oc get node compute-1-ru10.rackae1.mydomain.com -oyaml | grep 'scale.spectrum.ibm.com/role'
    ```
    Example output:
    ```
    scale.spectrum.ibm.com/role: afm
    ```
5. Run the following command to verify whether the required taints are applied on the node.
    ```
    oc get node <node_name> -o=jsonpath='{.spec.taints}'
    ```
    Example:
    ```
    oc get node compute-1-ru10.rackae1.mydomain.com -o=jsonpath='{.spec.taints}'
    ```
6. Go to the Global Data Platform page on the IBM Storage Fusion HCI System user interface and ensure that the node type displays AFM correctly.

### Node issues:
1. Node addition to the Red Hat® OpenShift® Container Platform cluster fails.
    Explanation:
    Node addition to the Red Hat OpenShift Container Platform cluster reports a failure. As a part of the node addition process, IBM Storage Fusion HCI System validates and runs a few steps. If any of these validations or steps fail, a node addition failure gets reported.

    Some of the reported failures can auto resolve after a few minutes because the IBM Storage Fusion HCI System retries the same validation and steps in every reconciliation cycle. You can view the actual failure reason in the status of the computeprovisionworker CR.

    In the following example, the message field indicates a node DNS validation failure. Determine the failure message from the computeprovisionworker CR status, and follow the steps in the Recommended actions section.
    ```
    oc -n ibm-spectrum-fusion-ns get cpw provisionworker-compute-1-ru5 -o yaml
    apiVersion: install.isf.ibm.com/v1
    kind: ComputeProvisionWorker
    metadata:
    creationTimestamp: "2024-05-01T21:39:53Z"
    generation: 1
    name: provisionworker-compute-1-ru5
    namespace: ibm-spectrum-fusion-ns
    resourceVersion: "8733225"
    uid: 3aa59cb6-c032-4b31-98ec-26284ed4588a
    spec:
    location: RU5
    rackSerial: 8Y2DXXX
    status:
    hostname: compute-1-ru5.mycluster.mydomain.com
    installStatus: Installing
    ipAddress: XX.YY.ZZ.15
    location: RU5
    macAddress: 08:xx:eb:ff:yy:zz
    machineSetScaled: true
    message: DNS validation for node failed.
    messageCode: IOCPW0011
    name: provisionworker-compute-1-ru5
    nodeType: storage
    ocpRole: compute-1-ru5
    oneTimeNetworkBoot: Completed
    rackName: mycluster
    startTimeStamp: "2024-05-01T21:39:54Z"
    ```
    Recommended actions:
    - DNS validation for node failed
      The error indicates a node DNS validation failure, which can be intermittent because of an unsuccessful DNS call. It eventually succeeds in the next cycle. However, if the issue persists for more than 10 minutes, check the DHCP and DNS configurations to confirm that the correct FQDN to IP exists for the DNS lookup and reverse lookup. After you fix the issue, the node addition in IBM Storage Fusion HCI System proceeds automatically to the next step.
    - Unable to add a node and failed to set boot order
      Usually, this failure gets resolved in the subsequent reconcile cycles. If it persists more than 10 minutes, then a Baseboard Management Controller (BMC) restart can resolve the issue. Sometimes, the IMM does not respond to ipmi or redfish commands. Contact IBM support to restart the BMC.
      After your restart the BMC, the error gets resolved in the subsequent reconciliation cycles.
    - Failed to read userconfig secret object
      The error indicates that an intermittent API error exists in reading the OpenShift Container Platform secret. It can occur due to a delay in the API response or some other network interruption. This error self-resolves automatically in the successive reconciliation cycles.
    - Failed to read kickstart configmap data
      The error indicates that an intermittent API error exists in reading the OpenShift Container Platform configmap. It can occur due to a delay in the API response or some other network interruption. It self-resolves automatically in the successive reconciliation cycles.
    - Unable to add a node and the Red Hat OpenShift node did not transition to Ready state
      The CSRs pending for approval is the most common reason for this error. The IBM Storage Fusion HCI System operator approves these CSRs as a part of the node addition process. If it does not occur as a rare case scenario, manually approve them to resolve the issue. Run the following commands to check and approve the pending CSRs:
      1. Run the following command to look for pending CSRs:
         ```
         oc get csr | grep -i pending
         ```
      2. Run the following command to approve pending CSRs:
         ```
         oc get csr -o go-template='{{range .items}}{{if not .status}}{{.metadata.name}}{{"\n"}}{{end}}{{end}}' | xargs oc adm certificate approve
         ```
    - Failed to scale machineset during new node addition
      As a part of the Red Hat OpenShift cluster expansion process, a Bare Metal host object gets created for every node that is added to the cluster. This Bare Metal host object go through the following states: registering > inspecting > available > provisioning > provisioned.
      After the Bare Metal host becomes available, the replica of the Bare Metal machineset CR in the OpenShift Container Platform cluster increases by one to start the machine creation process of that host. If that step fails, then you can see a reported error in the computeprovisionworker status. The Fusion operator tries  to increase it in the next reconcile cycle again and that eventually succeeds. If it does not succeed in a rare case scenario, do the following steps to manually increase the replica count of the machineset CR.
      1. Run the following command to get the machineset name:
        ```
        oc get machinesets -n openshift-machine-api | tail -1 | awk '{print $1}'
        ```
        2. Run the following command to get the machineset current replica count:
         ```
         oc get machinesets -n openshift-machine-api | grep worker | awk '{print $2}'
         ```
        3. Run the following command to increase the replica by one:
         ```
         oc scale --replicas=<old replica count from step2> + 1 machineset <machineset name from step 1> -n openshift-machine-api
         ```
    - Node might contain an invalid hostname (localhost)
      The error indicates that the added host did not get the expected fully qualified hostname assigned from DHCP. A common reason for this error is that the DHCP server does not get configured correctly. Check the following content in the DHCP server:
      1. Hostname option is set for this host entry
      2. No mistakes exist in the mac address that is used for this host entry
      3. DHCP service is running and it is reachable from the host
      After you fix the issue on the DHCP side, restart the node by using the Bare Metal host object from the Red Hat OpenShift console. To restart the node, do the following steps from the OpenShift Container Platform web console:
      1. Go to Home > Search.
      2. Search for CRs with ComputePowerOp. It lists CRs and nodes in the cluster.
      3. Go to the CR of the node to restart and then go to the YAML tab.
      4. Add the following line to Spec to power off the node.
    - Unable to add one or more nodes as they contain an invalid hostname (localhost)
      This error indicates that the host you want to add did not get the expected valid fully qualified hostname assigned from DHCP. A common reason for this error is that the DHCP server is not configured correctly. In a DHCP server, check whether the following conditions exist:
      - Hostname option is set for this host entry
      - No mistake in the mac address used for this host entry
      - DHCP service is running and DHCP is reachable from the host
      After you fix the DHCP issues, restart the node to proceed with the node addition. To restart node, do the following steps from the Red Hat OpenShift Container Platform web console:
      1. Go to Home > Search.
      2. Search for CRs with ComputePowerOp.
      3. Go to the CR of the node to restart and then go to the YAML tab.
      4. Add the following line to Spec to power off the node.

2. Cleaning up localhost node from the IBM Storage Fusion HCI System cluster
   The localhost node must not be added to the OpenShift® cluster, as it creates an issue at a later stage. A couple of OpenShift objects are created when the localhost node is added to the IBM Storage Fusion HCI System cluster.
   Procedure:
   Follow the steps to clean up the localhost node so that the node addition can retry after you resolve the DHCP or DNS misconfiguration issues.
    1. Run the following command to edit the ComputeProvisionWorker CR of the compute node that indicated an error message This node might contain an invalid hostname (localhost).
    ```
    # oc edit cpw provisionworker-compute-1-ru5
    ```
    Edit the respective CR to update the location information to an empty string to ensure that the ComputeProvisionWorker controller is not involved when you clean different objects of the compute node.
    ```
    # oc edit cpw provisionworker-compute-1-ru5
    ...
    spec:
    location: ""
    ```
    2. Delete the respective machine object of the compute node.
    You need to identify the machine object first and then mark it with an annotation. Then, scale down the machineset object to delete the machine object.

    a. Run the following command to get the machine object name from the BMH object.
    ```
    # oc -n openshift-machine-api get bmh,machine
    ```
    Example output:
    ```
    # oc -n openshift-machine-api get bmh,machine
    NAME                                    STATE                    CONSUMER                           ONLINE   ERROR   AGE
    baremetalhost.metal3.io/compute-1-ru5   provisioned              isf-rackae6-42ps4-worker-0-r6fmw   true             22h
    baremetalhost.metal3.io/compute-1-ru6   provisioned              isf-rackae6-42ps4-worker-0-t922n   true             22h
    baremetalhost.metal3.io/compute-1-ru7   provisioned              isf-rackae6-42ps4-worker-0-g842m   true             22h
    baremetalhost.metal3.io/control-1-ru2   externally provisioned   isf-rackae6-42ps4-master-0         true             44h
    baremetalhost.metal3.io/control-1-ru3   externally provisioned   isf-rackae6-42ps4-master-1         true             44h
    baremetalhost.metal3.io/control-1-ru4   externally provisioned   isf-rackae6-42ps4-master-2         true             44h

    NAME                                                            PHASE     TYPE   REGION   ZONE   AGE
    machine.machine.openshift.io/isf-rackae6-42ps4-master-0         Running                          44h
    machine.machine.openshift.io/isf-rackae6-42ps4-master-1         Running                          44h
    machine.machine.openshift.io/isf-rackae6-42ps4-master-2         Running                          44h
    machine.machine.openshift.io/isf-rackae6-42ps4-worker-0-g842m   Running                          22h
    machine.machine.openshift.io/isf-rackae6-42ps4-worker-0-r6fmw   Running                          22h
    machine.machine.openshift.io/isf-rackae6-42ps4-worker-0-t922n   Running                          22h
    ```
    The BMH object of compute-1-ru5 maps to the isf-rackae6-42ps4-worker-0-r6fmw of the machine object.
    b. Mark the machine object to delete by a special annotation machine.openshift.io/cluster-api-delete-machine: delete-me. The special marking helps to override the machine deletion policy rule.
    ```
    # oc -n openshift-machine-api edit machine isf-rackae6-42ps4-worker-0-r6fmw

    apiVersion: machine.openshift.io/v1beta1
    kind: Machine
    metadata:
    annotations:
        machine.openshift.io/cluster-api-delete-machine: delete-me
    ...
    ```
    c. Now you need to scale down the machineset object so that the deletion of the machine object gets initiated.
    Note: After the machineset scale down is performed, the machine objects corresponding to compute-1-ru5 are cleaned up, and the status of the BMH object corresponding to compute-1-ru5 changes to deprovisioning.
    ```
    # oc -n openshift-machine-api get machineset
    NAME                         DESIRED   CURRENT   READY   AVAILABLE   AGE
    isf-rackae6-cltp4-worker-0   3         3         3       3           8h

    # oc -n openshift-machine-api get machineset -oyaml | grep replicas
    replicas: 3

    # oc -n openshift-machine-api scale --replicas=<old replica value - 1> machineset <machine set name>
    machineset.machine.openshift.io/isf-rackae6-42ps4-worker-0 scaled
    ```
    d. After the machineset scale down is performed successfully, the status of the BMH object corresponding to compute-1-ru5 changes to ready.
    Important: This activity might take few minutes, and wait for it to be reflected against the BMH object before proceed with the further steps.
    3. Delete the compute nodes in the node object from the OpenShift Container Platform cluster if anything present.
    ```
    # oc get nodes

    NAME                                            STATUS   ROLES           AGE   VERSION
    compute-1-ru6.isf-rackae6.rtp.raleigh.ibm.com   Ready    worker          21h   v1.23.17+16bcd69
    compute-1-ru7.isf-rackae6.rtp.raleigh.ibm.com   Ready    worker          21h   v1.23.17+16bcd69
    control-1-ru2.isf-rackae6.rtp.raleigh.ibm.com   Ready    master,worker   43h   v1.23.17+16bcd69
    control-1-ru3.isf-rackae6.rtp.raleigh.ibm.com   Ready    master,worker   43h   v1.23.17+16bcd69
    control-1-ru4.isf-rackae6.rtp.raleigh.ibm.com   Ready    master,worker   43h   v1.23.17+16bcd69
    localhost                                       Ready    worker          20h   v1.23.17+16bcd69

    # oc delete node localhost 
    ```

    4. Delete the BMH object of the compute node.
    ```
    # oc -n openshift-machine-api get bmh 

    NAME            STATE                    CONSUMER                           ONLINE   ERROR   AGE
    compute-1-ru5   available                                                   false            24h
    compute-1-ru6   provisioned              isf-rackae6-42ps4-worker-0-t922n   true             24h
    compute-1-ru7   provisioned              isf-rackae6-42ps4-worker-0-g842m   true             24h
    control-1-ru2   externally provisioned   isf-rackae6-42ps4-master-0         true             46h
    control-1-ru3   externally provisioned   isf-rackae6-42ps4-master-1         true             46h
    control-1-ru4   externally provisioned   isf-rackae6-42ps4-master-2         true             46h

    # oc -n openshift-machine-api delete bmh compute-1-ru5
    baremetalhost.metal3.io "compute-1-ru5" deleted
    ```

    5. Delete all pending CertificateSigningRequests corresponding to the localhost node.
    ```
    # for i in `oc get csr --no-headers | grep -i system:node:localhost | grep -i pending | awk '{ print $1 }'`;do oc delete csr $i; done
    ```
    6. Fix the DNS or DHCP issue to get the correct hostname for the corresponding compute node instead of the local host.
    Delete the ComputeProvisionWorker object of the compute node.
    ```
    # oc -n ibm-spectrum-fusion-ns get cpw 
    NAME                            AGE
    provisionworker-compute-1-ru5   24h
    provisionworker-compute-1-ru6   24h
    provisionworker-compute-1-ru7   24h

    # oc -n ibm-spectrum-fusion-ns delete cpw provisionworker-compute-1-ru5
    computeprovisionworker.install.isf.ibm.com "provisionworker-compute-1-ru5" deleted
    ```

    7. If the issue occurs during installation at the time of OpenShift configuration, the node conversion resumes automatically. If the issue occurs during the node upsize, then retry the node addition.

## Shutting down and restarting IBM Storage Fusion HCI System rack

Procedure to gracefully restart the IBM Storage Fusion rack.

Before you begin:
Install OpenShift® Command Line Interface (CLI):
1. Log in to OpenShift Container Platform web console.
2. Click ? in the title bar, and click Command Line Tools.
    The Command Line Tools page is displayed.
3. In the Command Line Tools, click Download oc for ```<your platform>```.
4. Save the file.
5. Unpack the downloaded archive file.
6. Move the oc binary to a directory on your path.
7. Run the file to install the OpenShift CLI.

Procedure:
1. Capture health check report before bringing down the rack. It helps to check for any preexisting issues post power-on.
    Note: The health report must be saved to a different system.
    Global Data Platform
    For Global Data Platform storage, run the following OC commands to get the system health status before shutdown:
    a. Run the following commands to list the pods, cluster operators, and nodes.
    ```
    oc get po -A | grep -v Running | grep -v Completed
    oc get co
    oc get nodes
    ```
    b. Change to ibm-spectrum-scale namespace:
    ```
    oc project ibm-spectrum-scale
    ```
    c. Log in to a running pod. For example, compute-1-ru5 pod:
    ```
    oc rsh compute-1-ru5
    ```
    d. Run the following command to get the state of the GPFS daemon on one or more nodes.
    ```
    mmgetstate -a
    ```
    e. Run the following command to display the current configuration information for a GPFS cluster.
    ```
    mmlscluster
    ```
    Fusion Data Foundation
    For Fusion Data Foundation storage, run the following OC commands to get the system health status before shutdown:
    a. Check whether all cluster operators are Available and not in Degraded state. To verify, run the following commands:
    ```
    oc get co
    ```
    b. Check whether any updates are Progressing on the cluster. To verify, run the following command:
    ```
    oc get clusterversion
    ```
    c. Check that the Fusion Data Foundation cluster is in a healthy state.
    i. In the Red Hat® OpenShift web console, click Storage > Data Foundation.
    ii. In the Status card of the Overview tab, clickStorage system.
    iii. In the notification window, click the Storage system link.
    iv. In the Status card of theBlock and File tab, verify that the storage cluster is in a healthy state.
    v. In the Details card, verify the cluster information.
2. Check whether there exists any active Backup & Restore jobs. If Backup &  Restore or application synch is in progress, then wait for them to complete. Wait for in progress workload operations to complete. Before you proceed with the shutdown of the storage cluster, ensure that no data is in progress for any job or application.
3. Run the following steps based on whether your rack is a stand alone or is in a disaster recovery setup (Metro-DR or Regional DR):
    Stand-alone
    a. Run the following command to shut down:
    ```
    mmshutdown -a
    ```
    b. Run the following command to verify whether all nodes are down:
    ```
    mmgetstate -a
    ```
    c. Exit from the pod
    ```
    exit
    ```
    Metro-DR
    If you plan to shut down a site, ensure that you failover your applications to the other site.
    a. Shutdown scale pods on affected site by using the mmshutdown directly in the pod Terminal.
    b. Run exit to exit from the pod
    Regional DR
    If you plan to shut down a site, ensure that you failover your applications to the other site.
    a. Run the following command to shut down:
    ```
    mmshutdown -a
    ```
    b. Run the following command to verify whether all nodes are down:
    ```
    mmgetstate -a
    ```
    c. Exit from the pod
    ```
    exit
    ```
4. Run the following commands to shut down storage cluster.
    Global Data Platform cluster
    a. Switch the project to ibm-spectrum-scale-operator.
    ```
    oc project ibm-spectrum-scale-operator
    ```
    b. Set the replicas in the deployment configuration:
    ```
    oc scale --replicas=0 deployment ibm-spectrum-scale-controller-manager
    ```
    c. Switch the project to ibm-spectrum-scale.
    ```
    oc project ibm-spectrum-scale
    ```
    d. Log in to compute-1-ru<x>:
    ```
    oc rsh compute-1-ru<x>
    ```
    Fusion Data Foundation
    For Fusion Data Foundation nodes with status Running, stop the applications that consume storage from the Fusion Data Foundation to stop the I/O. Hence,
    the shutdown sequence is important to avoid any data corruption and pod failures. The shutdown of the application components must to be done before you shut down the nodes. Scale down the application deployments so that the application pods do not get started on other nodes in case node selectors are not set. Also, this must stop storage I/O from the applications.
5. Place the Data Cataloging service in an idle state on the Red Hat OpenShift environment. For more information about the procedure, see Graceful shutdown.
6. Shut down the Red Hat OpenShift Container Platform cluster.
    a. If the cluster-wide proxy is enabled, be sure to export the NO_PROXY, HTTP_PROXY, and HTTPS_PROXY environment variables. To check if the proxy is
    enabled, run 
    ```
    oc get proxy cluster -o yaml
    ```
    b. Take etcd backup.
    ```
    oc debug node/<node_name> (any one control node)
    sh-4.15# /usr/local/bin/cluster-backup.sh /home/core/assets/backup
    ```
    c. Copy the etcd backup to external system.
    ```
    snapshot_.db and static_kuberesources_.tar.gz
    ```
    You can use the oc rsync command to copy the files to an external system. You need two terminals for this operation.
    i. Open terminal one.
    ii. Run the following commands for etcd backup:
    ```
    oc debug node/<node_name>
    sh-4.15# /usr/local/bin/cluster-backup.sh /home/core/assets/backup
    ```
    In oc debug, node/<node_name> command, use any one control node.
    iii. Run the following command and record the new pod name:
    It is the source pod, and the backup files reside inside the pod.
    ```
    oc debug
    ```
    Do not close the terminal 1.
    iv. Open terminal two and run the following command to copy the file to the local folder:
    ```
    oc -n <namespace_of_debug_pod> rsync <source_podname_in_above_step>:/home/core/assets/backup/snapshot_.db <local_folder_path>
    ```
    If required, add the namespace of the debug node pod location.
    v. Repeat the step ii to copy another backup file to the external system.
    vi. Close the terminal windows after all the files are copied.
    For more information about the procedure, see Copy local files to or from a remote directory.
    d. Ensure that you take off the workloads before you shut down the nodes.
    e. Run the following commands to shut down the nodes:
    The node with OpenShift Container Platform user interface and IBM Storage Fusion user interface must be the last node you must power off.
    For Fusion Data Foundation, you must first shut down the application nodes and then shutdown the Fusion Data Foundation nodes. Finally, shutdown the
    OpenShift control plane nodes.
    ```
    for node in $(oc get nodes -o jsonpath='{.items[*].metadata.name}'); do oc debug node/${node} -- chroot /host shutdown -h 1;
    done
    ```
    After 3 to 5 minutes, the Red Hat OpenShift Container Platform becomes inaccessible.
    This step brings down all the software on the rack. The rack is ready to be powered off.
7. Physically press the power off button of the nodes and the rack.
    Note: The switches do not have the option to shutdown, and they can only be rebooted. When you power off the entire rack (unplugged), the switches shut down
    automatically. Similarly, when the power is restored to the rack, the switches comes up automatically.
8. Power on the rack.
    a. Power on the rack.
    b. Go to the physical node and click the power button to power on all the nodes. Power on all control nodes. After all control nodes are up, power on compute nodes.
    For Fusion Data Foundation, power on the nodes in this sequence:
    i. Start the Control Plane nodes.
    ii. Start the Fusion Data Foundation nodes.
    iii. Verify that these pods are in a running state:
    ```
    oc get pods -n openshift-storage
    ```
    It is possible that all Fusion Data Foundation pods may not be Running by now, but it is expected to have rook-ceph-{mds-*,mon-*,mgr-*,osd-
    *} pods to be running to serve storage. If the Ceph cluster is not in a good shape, it results in a FailedMounts with infra and application pods.
    iv. Start the infra nodes.
    v. Start the application nodes.
    c. After all the nodes are up and cluster operators are up (except image registry), run the following commands to ensure that the OpenShift cluster is up along with the IBM Storage Fusion operators.
    ```
    oc get po -A | grep -v Running | grep -v Completed
    oc get co
    oc get nodes
    ```
    d. For Global Data Platform, bring back the Scale.
    ```
    oc project ibm-spectrum-scale-operator
    oc scale --replicas=1 deployment ibm-spectrum-scale-controller-manager
    ```
    Give it a few minutes and check the cluster or storage dashboard.
    For Fusion Data Foundation, scale up the application deployments.
    e. Run the following commands to ensure that the storage pods are up:
    Global Data Platform
    i. To run commands on a node, run the following rsh command:
    ```
    oc rsh compute-t-ru<x>
    ```
    ii. Switch namespace to ibm-spectrum-scale:
    ```
    oc project ibm-spectrum-scale
    ```
    iii. Verify whether all pods are in running state in the ibm-spectrum-scale project:
    ```
    oc get pods
    ```
    iv. Run the following command to get the state of the GPFS daemon on one or more nodes.
    ```
    mmgetstate -a
    ```
    v. Switch project to ibm-spectrum-scale-csi:
    ```
    oc project ibm-spectrum-scale-csi
    ```
    vi. Verify whether all pods are in running state in the ibm-spectrum-scale-csi project. This may take sometime.
    ```
    oc get pods
    ```
    Fusion Data Foundation
    Run the following command to validate all pods are up in the openshift-{storage,monitoring,logging,image-registry} and the application namespaces:
    ```
    oc get pods -n <NAMESPACE>
    ```
9. Bring back Data Cataloging to a running state.

## OpenShift virtualization on IBM Storage Fusion HCI System

How to deploy the Red Hat® OpenShift® Virtualization Operator within IBM Storage Fusion HCI System.
OpenShift Virtualization provides a platform to bring virtual machines (VMs) into containerized workflows by running a virtual machine within a container where you develop, manage, and deploy virtual machines side-by-side with containers in one platform. For more information about how to create and manage VMs, see Red Hat OpenShift Virtualization and What you can do with OpenShift Virtualization.
A list of example VM features that the Red Hat OpenShift Virtualization supports:
- Create and manage Linux and Windows virtual machines
- Connect to virtual machines through a variety of consoles and CLI tools
- Import and clone existing virtual machines
- Manage network interface controllers and storage disks attached to virtual machines

Procedure:
1. Go to the Red Hat OpenShift Container Platform Operator Hub.
2. Search for OpenShift Virtualization.
3. Install the OpenShift Virtualization Operator.
4. Configure the installation parameters to either custom or recommended namespace.
5. Wait for the installation to complete.
6. Go to OpenShift Virtualization in Installed Operators page and click Create Hyperconverged to configure the Hyperconverged resource.
7. When the Operator deployment is complete go to Installed Operators and look for OpenShift Virtualization within the openshift-cnv or your namespace.
8. Check whether the status is successful.
    A new Virtualization menu appears. It indicates that your cluster is now ready to deploy virtual machines within a project /or namespace of your choice.

## Networking

The Network page in the IBM Storage Fusion user interface provides a view of the networking details in your appliance. You can view information about switches, VLANs, and links.

Basic concepts:
- VLAN:
    A virtual LAN (VLAN) is an isolated broadcast domain that is created within a switch. Each VLAN created within a switch is isolated from other VLANs. The network traffic can pass from one VLAN to another by adding a routing device. The routing functions must be provided at the data center core network.
- MLAG:
    A Multi-Chassis Link Aggregation Group (MLAG) allows for multi-system link aggregation and facilitates active-active uplinks of access layer switches. An MLAG with no Spanning Tree configured avoids the wasted bandwidth that is associated with links that are blocked by the spanning tree. To ensure high availability for the system, IBM Storage Fusion requires MLAG and link aggregation for the switches.
- Switch LAG ID
    This Switch LAG ID is used by high-speed switches inside the IBM Storage Fusion rack, which is not related to the LACP ID on (external) switch. It is unique for each IBM Storage Fusion rack and can be less than 250. The high-speed switch (MLAG PAIR) that gets connected to customer switch must have a unique system MAC address (MLAG ID). This MLAG ID is used as source
    MAC address for traffic that is sourced from MLAG PAIR, such as STP BPDUs. The MLAG ID is internally derived from the Switch LAG ID. The specific Switch LAG ID is used as the last octet for "44:38:39:ff:00:xx".
- Layer 2 connections
    The connections from all the IBM Storage Fusion high-speed switches to the data center core network and customer management network are all layer 2
    connections. IBM Storage Fusion supports the Link Aggregate Control Protocol (LACP) type of layer 2 aggregation.
    The following characteristics describe layer 2 connections:
    - Layer 2 is considered switching and is done at the hardware layer.
    - Layer 2 is in the same broadcast domain or local network.
    - Layer 2 finds adjacent partners by MAC address.
- Layer 3 connections
    IBM Storage Fusion does not participate in any layer 3 routing or firewall functions. These functions are done in the data center core network.
    The following characteristics describe layer 3 connections:
    - Layer 3 is considered routing and is done at the software layer.
    - Layer 3 knows how to traverse multiple networks (hops).
    - Layer 3 finds adjacent partners by IP address.
- Spanning tree
    In the networking stages of the installation for IBM Storage Fusion, an option to enable or disable the Spanning Tree is available. When you enable it, the spanning tree is enabled on the high-speed switches. By default, the option is disabled on these switches.The high-speed switches in the IBM Storage Fusion rack support only rapid spanning-tree (RSTP) modes. It is also compatible with PVST and PVST+. Customer data networks that are running older spanning-tree methods (MST) are not supported.

Aggregation methods:
IBM Storage Fusion supports the following aggregation methods for the connections on the customer data network:
- LACP:
    The standard-based negotiation protocol, which is known as IEEE 802.1ax Link Aggregation Control Protocol (LACP), is a way to dynamically build an Etherchannel. The "active" end of the LACP group sends out special frames, which advertise the ability to form an Etherchannel. Typically, both ends are set to an "active" state. After these frames are exchanged, and if the ports on both sides agree that they support the requirements, LACP forms an Etherchannel. LACP is implemented in the switch by way of Linux bonding. Linux bonding provides a method for aggregating multiple network interfaces (members) into a single logical bonded interface(bond). Link aggregation is useful for linear scaling of bandwidth, load balancing, and failover protection.
- No aggregation:
    If aggregation is not possible, this method is available, but not suggested, to provide high availability with no aggregation. With this method, it is suggested to enable Spanning Tree to avoid loops. When this method is used and Spanning Tree is enabled, one of the links is deactivated by STP, while the other link is enabled. his method is also called Aggregation None.

Port types:
- Access:
    An access port provides access to a single VLAN only. Typically, the packets on an access port are raw Ethernet frames (untagged packets).
- Trunk:
    A trunk connection is used to pass traffic from multiple VLANs between two switches. All trunks use the IEEE 802.1Q standard. This link can be a single wire or one of the aggregations methods.

Supported Toplogies:
To review examples of topologies that support high availability, see Network planning.
- Viewing network details
    The Network page in the IBM Storage Fusion user interface provides information about switches, VLANs, and links in the system.
- Configuring network connections
    To configure and manage network connections of your IBM Storage Fusion appliance, add VLANs and them link to the system.
- Upgrading switch firmware
    Upgrade switch firmware.
- Provisioning additional networking
    Provision additional networking for direct network access to a container or to a virtual machine.

## Customer Replacement Units (CRU)

List of Customer Replacement units for your reference.

| Name                               | Machine type          | Model  |
|------------------------------------|-----------------------|--------|
| IBM Storage Fusion HCI System      | Compute/Storage Server| 9155 C01|
| IBM Storage Fusion HCI System      | Compute-storage Server| 9155 C05|
| IBM Storage Fusion HCI System      | Compute-only Server   | 9155 C00|
| IBM Storage Fusion HCI System      | Compute Server        | 9155 C04|
| IBM Storage Fusion HCI System      | Compute-storage Server| 9155 C10|
| IBM Storage Fusion HCI System      | Compute-storage Server| 9155 C14|
| IBM Storage Fusion HCI System      | 100GbE High-Speed Switch| 9155 S01|
| IBM Storage Fusion HCI System      | 1GbE Switch           | 9155 S02|
| IBM Storage Fusion HCI System      | AFM Server            | 9155 F01|
| IBM Storage Fusion HCI System      | GPU Server Gen 02     | 9155 G02|
| IBM Storage Fusion HCI System      | GPU Server Gen 03     | 9155 G03|
| IBM Storage Fusion HCI System      | Rack                  | 9155 R42|
| IBM Storage Fusion HCI System      | KVM                   | 9155 TF5|

### Removing and replacing Customer Replacement Units (CRU)

Removing and replacing CRU includes the guidelines and instructions that need to take care when you remove the CRU.
- Removing and replacing the power supply unit
    Remove the existing power supply unit of the server and replace it with a new one.
    Before you begin:
    Prerequisites for removing the power supply unit:
    - The server is shipped with two power supply by default. In this case, the power supply is hot pluggable and redundant.
    - Ensure that you power off the server and disconnect all the power cords and network cables before you remove the power supply unit.
    - Ensure that you put the node into maintenance mode. For more about node maintenance, see Administering the node.
    - Ensure that you install an additional hot-swap power supply to support redundancy.
    - Ensure that you wear an ESD wrist strap when you remove the power supply unit.
    - Ensure that you handle the component carefully and include more than one qualified person to perform the task if the component weight is more than 18 kg(40 lb).
    Prerequisites for replacing the power supply unit:
    - The server is shipped with only one power supply by default. In this case, the power supply is non-hot swap.
    - Ensure that you power off the server and disconnect all the power cords and network cables before you replace the power supply unit.
    - Ensure that you put the node into maintenance mode. For more about node maintenance, see Administering the node.
    - Ensure that you install an additional hot-swap power supply to support redundancy.
    - Ensure that you wear an ESD wrist strap when you replace the power supply unit.
    - Ensure that you handle the component carefully and include more than one qualified person to perform the task if the component weight is more than 18 kg(40 lb).
    Procedure:
    1. Remove the power supply unit as follows:
        a. Disconnect the power cord from the hot-swap power supply and the electrical outlet.
        b. Slide the release tab toward the handle and carefully pull the handle backwards at the same time to remove the hot-swap power supply out of the chassis.
    2. Replace the power supply unit as follows:
        a. Take the new part out of the package and place it on a static-protective surface.
        b. Remove the power supply filler if it is installed.
        c. Place the new hot-swap power supply into the bay and push until it gets into position.
        d. Connect the power supply unit to a properly grounded electrical outlet.
    What to do next:
    1. Install a power supply filler or a new power supply to cover the slot.
    2. If you are instructed to return an old hot-swap power supply, follow all packaging instructions and use any packaging materials provided.
    3. If the server is turned off, turn on the server. Ensure that both the power input LED and the power output LED on the power supply are lit, indicating that the power supply is operating properly.

- Replacing an NVMe disk
    Follow the instructions to replace an NVMe disk.
    Procedure:
    1. Open the IBM Storage Fusion HCI System user interface.
    2. Click the App Switcher icon in title bar and click Storage outbound arrow from the list.
    As soon as you click the Storage outbound arrow, the IBM Storage Scale user interface is displayed.
    3. Click Sign In to login to the IBM Storage Scale user interface.
    4. Click the Events icon and check whether there is any error event showing the bad disk.
    5. Click Run Fix Procedure and follow the instructions to replace the bad disk.
    As soon as you click the Run Fix Procedure, the Fix Procedure: Replace Disks page is displayed.
    6. Click Next.
    7. Replace the disk physically and select the The disk has been replaced check box.
    8. Click Finish. The message is displayed in the Fix Procedure: Replace Disks page:
    Successfully replaced the disks
    9. Click Close.
    What to do next:
    1. Ensure that the error event of a bad disk disappears from the Events page.
    2. Ensure that the new disk shows up with the different serial number in the Physical Disks page.
    3. Check the file system mount and physical disks in a core scale pod.
    Run the OC command to check Scale core pod.
    ```
    oc get pod
    NAME READY STATUS RESTARTS AGE
    compute-0 2/2 Running 0 17d
    compute-1 2/2 Running 0 17d
    compute-2 2/2 Running 0 17d
    control-0 2/2 Running 0 17d
    control-1 2/2 Running 0 17d
    control-2 2/2 Running 0 17d
    ```
    Run the OC command to see all the containers in this pod.
    ```
    oc describe pod/compute-2 -n ibm-spectrum-scale
    ```
    Run the following command to check the file system mount.
    ```
    mmlsmount all
    ```
    Run the following commands to check the current physical disks health.
    ```
    mmvdisk pdisk list --rg all --not-ok
    mmvdisk: All pdisks are ok.
    mmlsrecoverygroup rg1 -L --pdisk
    declustered current allowable
    recovery group arrays vdisks pdisks format version format version
    ----------------- ----------- ------ ------ -------------- --------------
    rg1 1 25 12 5.1.5.0 5.1.5.0
    declustered needs replace scrub background activity
    array service vdisks pdisks spares threshold BER trim free space duration task progress priority
    ----------- ------- ------ ------ ------ --------- ------- ---- ---------- -------- -------------------------
    DA1 no 25 12 1,3 1 enable no 326 GiB 14 days rebalance 2% low
    n. active, declustered state,
    pdisk total paths array free space remarks
    ----------------- ----------- ----------- ---------- -------
    n001p001 1, 1 DA1 212 GiB ok
    n001p002 1, 1 DA1 208 GiB ok
    n002p001 1, 1 DA1 300 GiB ok
    n002p002 1, 1 DA1 252 GiB ok
    n003p001 1, 1 DA1 236 GiB ok
    n003p002 1, 1 DA1 234 GiB ok
    n004p001 1, 1 DA1 4552 GiB ok
    n004p002 1, 1 DA1 512 GiB ok
    n005p001 1, 1 DA1 232 GiB ok
    n005p002 1, 1 DA1 266 GiB ok
    n006p001 1, 1 DA1 250 GiB ok
    n006p002 1, 1 DA1 222 GiB ok
    ```

- Disconnecting and connecting power cords
    Steps to disconnect and connect existing power cords to the server.
    Before you begin:
    Prerequisites for removing the power supply unit
    - The server is shipped with two power supply by default. In this case, the power supply is hot pluggable and redundant.
    - Ensure that you power off the server and disconnect all the power cords and network cables before you remove the power supply unit.
    - Ensure that you put the node into maintenance mode. For more about node maintenance, see Administering the node.
    - Ensure that you install an additional hot-swap power supply to support redundancy.
    - Ensure that you wear an ESD wrist strap when you remove the power supply unit.
    - Ensure that you handle the component carefully and include more than one qualified person to perform the task if the component weight is more than 18 kg(40 lb).
    Prerequisites for replacing the power supply unit
    - The server is shipped with only one power supply by default. In this case, the power supply is non-hot swap.
    - Ensure that you power off the server and disconnect all the power cords and network cables before you replace the power supply unit.
    - Ensure that you put the node into maintenance mode. For more about node maintenance, see Administering the node.
    - Ensure that you install an additional hot-swap power supply to support redundancy.
    - Ensure that you wear an ESD wrist strap when you replace the power supply unit.
    - Ensure that you handle the component carefully and include more than one qualified person to perform the task if the component weight is more than 18 kg(40 lb).
    Procedure:
    1. Important:
    - Ensure that all power cables are properly connected when connecting power to the product. Connect all power cords to a properly wired and grounded electrical outlet for the racks with AC power. Ensure that the outlet supplies proper voltage and phase rotation according to the PDU or system rating plate.
    - If IBM supplied the power cords, then connect power to this unit only with the IBM provided power cord.
    - The product might be equipped with multiple power cords.
    Disconnect and connect the power cords in the specified order.
        a. Ensure that the mating connector of the power supply is free of any dirt or obstacles.
        b. For AC power, remove the power cords from the outlets.
        c. Unplug the power cord from the power supply.
    2. Connect the power cord as follows:
        a. Connect the power cord to the power supply.
        b. For AC power, attach the power cords to the outlets.

## Monitoring IBM Storage Fusion HCI System

IBM Storage Fusion HCI System provides multiple ways to monitor the health of the hardware and software.

- Monitoring hardware from IBM Storage Fusion HCI System user interface
    The IBM Storage Fusion HCI System user interface provides an enhanced hardware monitoring experience. You can quickly determine a hardware component in the IBM Storage Fusion HCI System that needs your attention.

    The Resource summary section in the Overview page includes the summary of the total number of nodes and switches along with their health statuses. If there are any critical hardware component failure, it provides the number of hardware components that need attention. The page also provides a graphical view of the IBM Storage Fusion HCI System rack and the color of the hardware component represents the health statuses. The nodes and switches are shown as per their appropriate locations in the actual physical rack.
    You can identify the node that has a hardware issue based on the legend displayed in the IBM Storage Fusion HCI System user interface:
    - The color green indicates that the status of the hardware is in normal and healthy state.
    - The color red indicates critical errors in the hardware and needs attention.
    - The color gray indicates that the hardware is in inactive and power off state.
    - The color blue with diagonal stripes indicates that an action is in progress on the hardware, such as power up, power down, or a firmware upgrade.
    - The color yellow denotes warning.
    Important: The nodes and switch rack unit locations highlighted with the colors red and yellow have an issue and need attention.
    - Monitoring nodes and switches
    Procedure to monitor nodes and switches from the Overview dashboard page.
    Procedure:
    1. In the IBM Storage Fusion HCI System user interface, go to Infrastructure > Overview.
    The Overview dashboard page is displayed. For details about the overview page, see Monitoring hardware from IBM Storage Fusion HCI System user interface.
    2. Go through the Resource summary section to monitor the health of the nodes and switches.
    3. Hover over a graphical view of the hardware to identify units in the rack. For node, it shows the name of the hardware, the health status, the type of node, and the
    rack unit. For switch, it shows the name of the hardware, the health status, the type of switch, and the rack unit.
    4. If one or more nodes or switches are in an error, degraded, or disabled state, take appropriate action on the hardware component.
    - Node details
    The Nodes page in the IBM Storage Fusion HCI System user interface provides information about nodes and their health status. From the IBM Storage Fusion HCI System menu, click Infrastructure > Nodes. The Nodes page provides the details of the nodes in a table format. The column headers that are displayed by default in the table are Name, Hardware status, Type, Firmware level, Serial number, Rack name, Rack Unit number, CPU cores, and Memory (GB) details. Use the Search field to filter and find records. The settings icon helps you to customize the column headings of these tables. Decide the number of records per
    page by using the Items per page drop-down list of the table. You can traverse to the previous or next page views. Select a number from the drop-down list of the page numbers to jump to a specific page view.
    Click the node name link that you want to manage. It opens Nodes page that includes the front and rear graphical view, recent events, and other details of the node and its internal components. Select Front and Back in the Components section to see front and back views of the node and hover over a graphical view to check internal components and their status. To debug further, click the internal component that is in red color. It opens a new slide out pane and provides more details about the drives and adapters.

    - Monitoring and logging by using any monitoring tool
    IBM Storage Fusion provides multiple ways to monitor the health of the hardware and software.
    - Events are fed into the Red Hat® OpenShift® event manager
    The following example IBM Storage Fusion events for your reference:
    - Hardware events for IBM Storage Fusion HCI System, including switches and nodes
    - Events that are related to the data services of IBM Storage Fusion, such as backup and restore
    As IBM Storage Fusion feeds events into Red Hat OpenShift event manager, they show up in the integration that you use to monitor OpenShift events.
    - Metrics are fed into Prometheus
    IBM Storage Fusion feeds metrics into a Red Hat OpenShift Prometheus instance.
    The advantage is that they are available in any monitoring tool you are using with Prometheus, such as Grafana. IBM Storage Fusion provides a set of default Grafana dashboards and an access link to the interface.
    - IBM Storage Fusion provides logs for its services
    - IBM Storage Fusion provides a user interface that shows the status of the system
    - IBM Storage Fusion provides CRs that reflect the status of the system
    Events and metrics provide observability that can be fed into other monitoring systems, such as Grafana and ELK.
    Important:
    - IBM Storage Fusion does not install Grafana and ELK as part of its installation.
    - If you already have monitoring tools that you use in your environment, then you can feed IBM Storage Fusion observability into these existing tools. For example, if you use Grafana to monitor your OpenShift environment, then you can import metrics and dashboards of IBM Storage Fusion into its instance.
