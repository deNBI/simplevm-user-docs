# Instance detail

The detail page offers more information about a virtual machine and extra actions.

## General Overview

The overview of the instance detail page shows the most important information of the machine.
This section is devided into several parts

![general](../img/instance_detail/detail_general.png)
### General Information
This part shows:
 - The state of the VM, i.e. whether it is active or shut down, for example.
 - The project the VM is running in - you are also able to access the corresponding project overview clicking on the name of the project.
 - OpenStack-ID: the ID of the machine in the corresponding compute center. In case you have any problems with your machine, this identifier is very helpful for the support, when contacted
 - Creation date and user who created the machine
 
 It also enables the user to perform certain actions with the machine:

  - **Stop VM**<br>
      Shuts off the virtual machine. You may resume it afterward, but you can't access it while in "SHUTOFF" state.
  - **Reboot VM**<br>
      [Soft or hard](https://docs.openstack.org/mitaka/user-guide/cli_reboot_an_instance.html) reboot your virtual 
      machine.
  - **Create Snapshot**<br>
      Take a snapshot of the virtual machine. 
      See [here](../snapshots.md) for more information.
  - **Delete VM**<br> 
      Delete the virtual machine. Any attached volume gets detached but not deleted.
  - **Resume/Restart VM**<br> 
      Boot up the "SHUTOFF" virtual machine.

### Connection information

The connection information shows how to connect to the machine via **ssh** and in case a research environment is installed on the machine, the URL for browser access is shown.

### Flavor information

Shows information about the resources assigned to the virtual machine, like the number of VCPUs and the amount RAM.

### Image information

Shows the image the VM runs on. 

## Volumes

![volumes](../img/instance_detail/detail_volume.png)

This tab shows which volumes are attached to the machine and allows actions like detaching, renaming or deleting the volume.
For each volume, the unique OpenStack ID, the storage capacity, and the status is shown. 
For more information on volumes, see the [volume](../volumes.md) wiki page.


## Ports

When enabled for the project your machine is running in, you are able to configure port ranges for the vm, so it allows connections on these ports when sending requests from machines in the same network. 
In the case of SimpleVM, all machines in a project are located in the same network. Machines outside this network cannot access these ports. 
You can open port ranges from port 1024 to 65535.
The Ethernet type, the IP protocol and the start and end of the range can be specified for each range. This setting can be added to a machine with **Add**. Released port ranges can be removed again in the list below with **Remove**. 

Within the same network you are able to reach the machine using the **private IP address** shown.
The IP cannot be used to reach the machine from outside the project's own network.


![ports](../img/instance_detail/detail_ports.png)

???+ warning "Safety-critical"
    As this is potentially a safety-critical feature, these authorisations should be used with caution.

## Research environment

![resenv](../img/instance_detail/detail_resenv.png)

This tab delivers information about the installed browser-based research environment.
For more information on Research Environments, see the [customization](../customization.md#research-environments) wiki page.
This includes the chosen environment itself and the URL to access it via browser.
You are able to copy the link, view the logs of the research environment setup and to renew the backend or delete it.
Deleting the backend of the research environment will make the research environment unaccessible.
When problems with the access occur, a renewal of the backend might fix the problem.

## User Management
The user management enables you to grant and revoke access to the research environment. 
To grant access, a user has to be a member of the project.

[users](../img/instance_detail/user_management.png  )


???+ warning "Concurrent sessions"
    This doesn't automatically enable concurrent sessions, i.e., your session terminates
    once another user logs in with the same credentials.
    For information on concurrent sessions, see the specific 
    section of the [research environment](../customization.md#research-environments).

## Conda

![conda](../img/instance_detail/detail_conda.png)

The conda tab shows which conda packages got installed on the machines during startup.
Below a log of the installation can be viewed and downloaded as a `PDF` or `.txt`-file


## Event logs

It is possible to display all actions that have changed the status of the machine in tabular form.
The event log shows actions performed by users, such as starting and stopping machines, as well as system actions (for example, when the data center shuts down the machines for maintenance work). A date and the person or system responsible for the action is specified.

![events](../img/instance_detail/details_events.png)

## Rescue instance

If your instance is in a state of error so that you can no longer work with the instance correctly, there is the option to rescue the instance. This will boot a new instance with the same image as the original instance, then attach the root disk of the original instance to the new instance for data recovery. (See also [the Openstack documentation](https://docs.openstack.org/nova/latest/user/rescue.html) for more information.)

You can activate the rescue mode by opening the detail page of an instance and clicking on the "Rescue" button. 

![rescue](../img/instance_detail/detail_rescue.jpg)

You can only rescue your own instances. If you wish to rescue your instance but cannot see this button, please contact your project administration for help.

When the instance is in "RESCUED" state, you can access the rescued instance with the same connection information as the original instance, then download any data you wish to save. You can exit the "RESCUED" state by clicking on the "Unrescue" button, which will return the instance to its original state.

![unrescue](../img/instance_detail/detail_unrescue.jpg)

???+ warning "Purpose of rescue mode"
    The rescue mode only serves as a mean to recover data from an instance that doesn't work properly. After you have used the rescue mode and downloaded the necessary data, it is advised to delete the instance and start a new one instead of using the rescue mode for trying to fix it.

### Manual mounting

The root disk of the original instance is usually displayed as /dev/vdb. You can confirm this as well as the mountpoint of the disk by executing the following command:
```
sudo lsblk
```
![lsblk](../img/instance_detail/rescue_vdb.png)

Sometimes this drive is not mounted automatically or only partially mounted.

![vdb1_not_mounted](../img/instance_detail/rescue_vdb1.png)

 In this case, you can first create a directory, then mount the missing drive manually.
```
sudo mkdir /PATH/OF/YOUR/DIRECTORY
sudo mount /dev/DRIVE_TO_MOUNT /PATH/OF/YOUR/DIRECTORY
```

![vdb1_mounted](../img/instance_detail/rescue_vdb1_mounted.png)

After you have successfully mounted the drive, you can access the data by going to the directory you created.

???+ info "Instance with volume"
    You can still access attachted volumes in rescude mode. However, you cannot attach a volume to an already rescued instance. If you are using an instance with ephemeral storage and worry about data loss, we recommend attaching a volume to it when creating the instance.