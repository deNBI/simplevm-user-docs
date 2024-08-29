## General

### I cannot access my virtual machine?
At first try to restart the machine.
If this does not bring the desired success, the reasons can be manifold.

Make sure that you use the correct SSH key when accessing the machine.
If you have changed your SSH key since creating the machine, it will not be automatically changed on the machines already running. The connection to these VMs still requires the key that was set at the time of creation. 
The private key is always used to connect to the machine.

You can find more about SSH keys [here](./simple_vm/keypairs.md#ssh-keys-and-how-to-share-access).

Maintenance may be performed at the site where your machines are running. Occasionally, malfunctions also occur during regular service.

### What are flavors? Which flavor should I choose for my VM?
Flavor determine the hardware resources available for their virtual machine. During the process of starting the VM, you will see the flavors that are available to you.
Applications or more complex pipelines that require high memory or compute should be run on flavors with more VCPUs and/or RAM. For certain use cases from the fields of artificial intelligence or data mining, GPU flavors can be useful. This is also true if CUDA is part of your pipeline.


## SimpleVM

### My VM is stuck in "build" or "checking connection" status? What does this mean?
An error may have occurred while creating the machine. If the machine is not available even after a longer period of time, feel free to contact us so that we can take a look at the situation.

### My machine is marked as "NOT FOUND". What does this imply?
The machine may no longer exist in OpenStack. If you have any questions regarding your virtual machine, please feel free to contact us.

## Miscellaneous

### I can not build docker images and can not download packages from inside of the container.

In case you use docker containers in your VM it may happen that the building 
process of the containers will fail or you can not download packages. The reason is that docker can not connect to the 
internet. The problem is caused by different mtu values for the host and the 
docker container. To fix the problem you have to add the correct mtu value to 
the following file:

     /lib/systemd/system/docker.service

Add **--mtu=1440** to the end of the **ExecStart** line in the [Service] 
section, like shown below:

    ExecStart=/usr/bin/docker daemon -H fd:// --mtu=1440

Or you can add this to **/etc/docker/daemon.json**

    {
          "mtu": 1440
    }    

In a last step you have to reload the daemon and restart docker:

    sudo systemctl daemon-reload
    sudo systemctl restart docker

Now the docker build process should be successful.

