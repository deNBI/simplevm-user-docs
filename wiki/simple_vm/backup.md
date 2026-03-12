# Best practices to minimize risk of loss of data

When a virtual machine gets deleted, the data stored on the machine get deleted too.
This might happen because you deleted the machine, someone else deleted it by error,
or a software or hardware error leads to it.<br>
The de.NBI Cloud can't guarantee that you can always access your virtual machine, and the data on it.
Therefore, you get some best practices to minimize data loss when using the de.NBI Cloud.

## Remote backup

Machines, volumes and snapshots - although rare, can get corrupted.
Therefore, the absolute best practice to back up your data, is to diversify back up storage.
Copy it to a remote hard drive, disk, or another storage solution outside the de.NBI Cloud.<br>
You may find a tutorial on how to copy data from your machine to a local or remote storage at the
[File Transfer from/to de.NBI Cloud](../Tutorials/File_Transfer_Methods/index.md) wiki page.
If you have access to an object storage, please refer to the documentation of your object storage.

## Persist data on volumes

??? info "How to attach and mount volumes"
    You may find more information on how to create, attach, and mount volumes on the [Volume](./volumes.md) wiki page.

Volumes persist data independently of your virtual machines. In other words, a mounted volume 
doesn't get deleted when your virtual machine gets deleted.<br>
Your requested project volume storage limits the size of volumes in your project.
If you hit the volume storage limit, you can request more resources.
See [Resource modifications]({{extra.cloud_portal_wiki_link}}portal/modification/#resource-modifications) for more information.

???+ info "What to consider with volumes volumes"
    Using volumes for data suits you when you want:
    - To back up data often.
    - To back up data from different virtual machines.
    - To restore data without booting a new virtual machine.
    
    Data saved on a volume is somewhat independend of the state of your compute system, i.e., it is stored safely even if 
    you corrupt your virtual machine.

You have different ways to save data on a volume:  

  * Save data manually.<br>
    Mount the volume to the virtual machine you want persist data from and either copy it with:
    
    ```shell
    cp -a /path/to/files/to/backup /path/to/backupDirectory/on/mounted/volume
    ```
    or use a program like rsync to sync data between two directories.<br>
    Another good practice is to archive the files you want to back up:
    
    ```bash
    tar -zcf /path/to/backupDirectory/on/mounted/volume/persistent_01.tgz /path/to/files/to/archive
    ```

  * Save data automatically.<br>
    Mount the volume to the virtual machine you want to persist data from and create a cron job to automatically 
    transfer files to your mounted directory:
    
    ```shell
    0 0 * * * cp -a /path/to/files/to/save /path/to/peristent/directory/on/mounted/volume 
    ```
    or
    
    ```shell
    0 0 * * * tar -zcf /path/to/persistent/directory/on/mounted/volume/peristent_01.tgz /path/to/files/to/archive
    ```
    This cron jobs archive up your files every day at midnight.<br>
    For a more detailed guide on how to create and use cron jobs, please refer to one of the many guides you may find 
    on the internet.  

To transfer data to another machine B, you need to mount the volume on machine B. Detach and unmount 
it first if still attached to machine A.
<br>
<br>
You can attach and mount a volume only to one virtual machine at a time. That means, you can't utilize volumes to save data from
many machines in parallel on one volume.<br>
You can set up a volume as a NFSv4 Share.
For an exemplary guide on NFSv4 Share, see [SettingUpNFSHowTo](https://help.ubuntu.com/community/SettingUpNFSHowTo).

## Persisting data with snapshots

??? info "How to create and use snapshots"
    You may find more information on how to create and start from snapshots at [Images and snapshots](./snapshots.md).

Snapshots preserve the files on the root disk and the full state of your RAM. You may use a snapshot to boot a new 
virtual machine.
You can create a snapshot only manually.

???+ info "When to persist data with snapshots"
    Utilize snapshots when you want:

    - To preserve the full state of your virtual machine, for example, before deleting your vm.
    - To boot up at least one vm with your data already present.
    
???+ danger "Drawbacks to consider"
    A snapshot preserves the full state of your virtual machine.
    Therefore, you can only snapshot a virtual machine with up to a maximum of 256 GB RAM.<br>
    You can't restore data on a running machine, you can use a snapshot only when starting a new vm.<br>
    Data on ephemeral disks exist only as long the vm exists, data on ephemeral disks get lost when not saved on 
    a different storage solution, for example, a volume.<br>
    Data on attached and mounted volumes aren't saved in a Snapshot, but are still available on your volume.
