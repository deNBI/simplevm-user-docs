# Workshop management

You can manage your workshops conveniently with SimpleVM.
Create workshops in your SimpleVM project, add users and tutors, and launch virtual machines
for your users with a few clicks.

On this page, you learn:

- How to create and manage a workshop.
- How to start instances for your workshop participants.
- How to inform your participants about their machines.
- Good general practices and tips for workshop and vm management.

## Requirements

Only administrators in a SimpleVM project with workshops activated can use the workshop tools.<br>
If you have no SimpleVM project, you need to apply for a SimpleVM project with `Workshop` marked,
see the [Portal]({{extra.cloud_portal_wiki_link}}portal/allocation/) wiki page for more information about the application process.

![allocation_workshop](./img/workshop/checkbox.png)

If you have a SimpleVM project without workshop activated, but would like to conduct one in the context of your 
project, contact the de.NBI helpdesk at [cloud-helpdesk@denbi.de](mailto:cloud-helpdesk@denbi.de).

## Create a new workshop

![new_workshop_sidebar](./img/workshop/new_workshop_sidebar.png){align=left}

All project administrators can manage workshops within the SimpleVM portal once your SimpleVM project got
approval.<br>


To create a new workshop for your project click `YOUR_PROJECT_NAME` in the sidebar and select `Create a new workshop`.

???+ question "I can't see the tab in the sidebar"
    The workshop section is only visible if:
    
    - You are an administrator of a SimpleVM project.
    - The SimpleVM project has workshop capability activated.
    
    If you satisfy both conditions and still not see the tab, contact the [cloud-helpdesk@denbi.de](mailto:cloud-helpdesk@denbi.de).

This will open a new page with a workshop creation formular:
![workshop-formular](./img/workshop/new_workshop_formular.png)


???+ question "Why the shortname?"
    The shortname of your workshop appears in the workshop vm names and research environment URLs to
    better differentiate between workshops.

???+ info "Workshop resource consumption"
    Workshops share the available SimpleVM project resources. If you have maximum 10 vms available,
    you can't start more than 10 vms across all workshops running in that SimpleVM project.

Upon submitting the workflow form, you will be automatically redirected to the Workshop Overview. Additionally, the Workshop will also be visible in the sidebar for quick access.



## Workshop Overview

![workshop-overview](./img/workshop/workshop_overview.png)

### Participants and Tutors

#### Registration
You can open the registration for the workshop via the `Create Invitation` button.
This will create a link which you can share, where participants can request a registration.
![workshop-invitation](./img/workshop/workshop_invitation_link.png)

The user can then request a registration:
![workshop-registration](./img/workshop/workshop_registration.png)


And a Project Admin can approve/decline the registration:
![workshop-regi_tutor](./img/workshop/workshop_reg_tutor.png)

Additional Workshop Tutors can be set via the Tutors Tab;

![workshop-add-tutor](./img/workshop/add_tutors.png)
???+ info "Tutors"
    Only project members can be set as tutors!


#### Start virtual machines for your participants

At your workshop overview select "Create new instances".

![new_instance_workshop](./img/workshop/workshop_new_instances.png)  

This will open the vm creation formular for the workshop:

![workshop_vms_form](./img/workshop/workshop_vms_form.png)  


Select a flavor.
See [Flavors]({{extra.cloud_portal_wiki_link}}Concept/flavors/) for more information about flavors.<br>
Select an image.
See [Images and Snapshots](snapshots.md) for more information about images and snapshots.<br>
Optionally, select a research environment template.
See [Research environments](customization.md#research-environments) for more information about browser-based
research environments.


??? tip "Base image with template versus research environment image"
    de.NBI Cloud provides images with a research environment installed.
    Virtual machines with pre-build images start faster than base images with a selected template.

Select the participants you want to start vms for.
You may start as many virtual machines for a participant as you have resources.


The machine name, and the research environment URL, have the form 
`<WORKSHOP-SHORTNAME><PARTICIPANT-LASTNAME><PARTICIPANT-FIRSTNAME>`, and have a maximum of 25 characters.<br>
The startup process puts the public SSH key of your workshop tutors and project admins on every workshop vm.
This enables your workshop tutors to connect to a workshop vm if required.<br>
The research-environment is accessible by the participant, and the tutors who started the virtual machine.
<br>
Before starting the workshop vms, you need to confirm your responsibility for the machines.
After starting, a redirect takes you to the workshop instance overview where you find more information about the status
of your machines.

![vm_overview](./img/workshop/workshop_vm_list.png)

#### Inform participants about their instances

Select specific or all vms and click `Send Information Mails` to inform the corresponding participants with a virtual machine about their virtual machines details and 
how to access it.<br>


???+ question "Who sends the E-Mail and who gets it?"
    The [cloud@denbi.de](mailto:cloud@denbi.de) address sends an E-Mail to the preferred E-Mail address of each
    participant. Each user can change their preferred E-Mail address on their [profile page](../simple_vm/keypairs.md#key-handling-on-profile-page).

### Clean up a workshop
Select the workshop you want to end and click `Delete Workshop`.
This deletes the workshop and all instances started for this workshop. Created volumes and snapshots remain.

???+ info "Volume resources"
    Keep in mind that volumes, that remain after cleaning up a workshop, occupy your allocated volume resources.
    If you want to free the resources, you need to 
    [apply for more resources]({{extra.cloud_portal_wiki_link}}portal/modification/#resource-modifications) or 
    delete the remaining [volumes](volumes.md) manually.




## Prepare data for your participants

Often, workshop participants need access to the same data.
SimpleVM projects can't duplicate or snapshot volumes yet.<br>
Because it's important that your participants can access the same data, and creating each volume
is time-consuming, you can contact us at [cloud-helpdesk@denbi.de](mailto:cloud-helpdesk@denbi.de).
We create copies of your prepared volume for you and assign them to your project.

## Best Practices

### Add participants beforehand

You should add participants to your project a couple of days before your workshop starts.
They need an account for the de.NBI Cloud Portal, otherwise you can't add them.
You can send them a link to the [registration wiki]({{extra.cloud_portal_wiki_link}}registration) where they can read about the
registration process.<br>
Sometimes, participants can have trouble registering.
Having them register beforehand, so you can add them, helps for a smoother workshop start.

### Start machines before the workshop starts

You should start machines for your participants before your workshop begins.
Depending on your vm configuration, starting many vms can take a couple of minutes.
In rare cases, starting a vm can fail, or a snapshot can be faulty.
Starting them beforehand, and making sure they run correctly, helps for a smoother workshop start 
and prevents troubleshooting when you actually want to conduct your workshop.

### Explain SSH keys

???+ info
    Please ignore this paragraph if your participants don't need to connect to their machines with SSH.

You should explain to your participants the basics of an SSH keypair.
Many participants don't know what an SSH keypair is.
It may happen that participants lose their private key, mix up public and private keys, or
try to use unrelated private and public keys.
This can lead to your participants losing access to their vm.<br>
Your participants can set their public key or create a keypair on their 
[profile page]({{extra.cloud_portal_wiki_link}}portal/user_information/#ssh-key).
Make sure they download the private key when they create an SSH keypair and save it on their computers,
the de.NBI Cloud Portal only stores the public key.

???+ tip "Have your administrators set their public key"
    Make sure the administrators of your project have their SSH key set.
    Starting virtual machines for your participants requires a public key from every administrator.

## Troubleshooting problems

If you experience troubles, have feedback, or have a special request, 
contact us at [cloud-helpdesk@denbi.de](mailto:cloud-helpdesk@denbi.de).
