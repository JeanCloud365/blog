---
exerpt: "What's inside the DevBox? A close look at the why and how of Microsoft's developer VDI offering"
tags: azure devbox virtual desktop windows
---
*What's inside the DevBox? A close look at the why and how of Microsoft's developer VDI offering*


Microsoft announced their [DevBox](https://techcommunity.microsoft.com/t5/azure-developer-community-blog/introducing-microsoft-dev-box/ba-p/3412063) product at Build 2022 and since the 15th of August, 2022 we can all try it out as [public preview](https://azure.microsoft.com/en-us/updates/public-preview-microsoft-dev-box/)! What does it mean and what is the unique selling point versus existing, similar Microsoft services? Lets find out!

## The rise, fall, and resurrection of VDI through IT history

VDI (short for Virtual Desktop Infrastructure) always was an interesting technology to me. The premise of having your digital working environment fully cloud-based in a VM, resilient to hardware failures and aging is something I use and love for both my professional as personal use. However, it is also a source of headaches for the IT administrator:

* If it is a shared VDI, making sure everybody gets a piece of the performance cake, and prevent that somebody is eating it all. A solution is to switch to personal VDI, but this increases the costs
* Making sure that all software needed for an employee is available and up-to-date. In some cases even manage different images based on the employee profile
* The Achilles heel: network latency. A delay of as much as 40ms between clicking on something and seeing the VDI perform the click can be a productivity killer.
* The false promise of cost savings, as licenses, operational overhead and infrastructure cost often are higher than a traditional desktop/laptop on every desk.

In this aspect, Microsoft already uses its cloud platforms to tackle some of these issues, by (until now) providing 2 distinct VDI offerings on their platforms:

* [Azure Virtual Desktop](https://azure.microsoft.com/en-us/services/virtual-desktop/), a PaaS-level solution which architecture-wise resembles a lot the traditional [Windows Server RDS setup](https://docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/welcome-to-rds), but with a lot of abstraction on the management components. You indicate the size of the (shared or personal) VDI pools, which users can connect to what pool and Microsoft does the rest. A nice cost saver is the ability to auto-stop and on-demand start VDI's, saving costs if no-one is in need of their VDI.
* [Windows 365](https://www.microsoft.com/en-us/windows-365), a SaaS-level solution integrated with the [Microsoft 365](https://www.microsoft.com/en-us/microsoft-365/what-is-microsoft-365) eco-system. It is basically a 'hands-off' Azure Virtual Desktop platform, where a 1 VDI is a license (cost based on the VDI sizing) per-user. Once you assign the license, Microsoft does the rest and drops a VDI at the user's door step. From this point the VDI is 'always-on' and aside from a reboot or reimage, can't be shut down (hence the fixed license cost).

The COVID pandemic was a source of a huge amount of Azure Virtual Desktop projects. The impact of working from home in the way we connect to our company resources was enormous:

* As employees were not working within the company building perimeter, the traditional 'desktop-attached-to-a-LAN-connection' model became useless
* Getting equipment to employees, who could not pick it up in the IT shop, became a logistics challenge
* Keep data exfiltration in check with devices connecting from untrusted (home internet) locations
* Prevent the VPN infrastructure from exploding in terms of capacity and/or license cost

The combination of cloud as an enabler, and COVID (however morbid) becoming a driver of efficient VDI at scale lowered the bar and proved the feasibility. With upcoming technologies like 5G networking and fiber connectivity becoming broadly available for home internet (at least in Belgium), the way gets further paved for 'My PC is in the cloud' to become a common pattern.

## Developers and VDI, a difficult marriage

Developers are not a difficult people! They just want to have:

* All THE CPU CORES
* ALL THE MEMORY
* Have first place in the build/release queue (not really VDI-related, but want to see impatience? Look at a developer waiting for their build to start.)
* Freedom to install any tool as needed
* If there are multiple projects, receive one handcrafted machine per project to avoid compatibility issues
* No lag as they are typing their 10000th line of code

These types of needs usually are a bombshell to a VDI platform, as usually these platforms are sized for average office use, with a fixed set of software tools.

Often the projects the developers are working on are limited in time and also don't need the machine to be always-on or always-there.

It is still possible to onboard these kinds of requirements on a VDI setup, but requires a big conversation for every project between the project team and the VDI platform team:

* Creating an image with the required tools
* Setting up a dedicated, right-sized pool for the project
* Making sure that costs are being distributed correctly
* Setting up stop- and start schedules (if this is even allowed at  least)
* Making sure everyone gets access to the pool, and joiners/leavers are handled

This re-introduces the traditional overhead that VDI-in-cloud hoped to solve! It is the eternal boundary between IT and Developers causing friction once again!

How can we tackle this? We need to bring the capability to spin up developer VDI capacity to the project teams in a governed, but most importantly **self-service** way. This is where DevBox comes in...

## What's in the DevBox?

![architecture](/assets/images/microsoft-devbox.png)
> NOTE: download this image and open it in draw.io to have the original drawing!

DevBox is actually Windows365, but ripped out of the Microsoft 365 stack and put in the Azure management one with a self-service sauce on top. You still [need](https://azure.microsoft.com/en-us/pricing/details/dev-box/) to have per-user a license that permits the right to use Windows and [Microsoft Endpoint Manager](https://www.microsoft.com/en-ww/security/business/microsoft-endpoint-manager) for management (eg P1,E5,...).

Note that once more, B2B Guest users are (alas, as this would benefit external developer scenario's) not supported.

However, you don't need to buy a separate Windows 365 license. That cost is replaced by a hourly cost for the compute and storage. As you can start and stop a DevBox on-demand, you can control the costs.

### Persona's

As mentioned in the [documentation](https://docs.microsoft.com/en-us/azure/dev-box/overview-what-is-microsoft-dev-box), the DevBox toplogy assumes 3 types of persona:

* **IT Infra Team** can manage the DevBoxes like any other VDI or phycical device, as they are hybrid or cloud joined to [Azure Active Directory](https://azure.microsoft.com/en-us/services/active-directory/). This ensures that policies and conditional access remain functional. They also manage the network connectivity from and to the DevBoxes.
* **DevOps Infra Team** (this team provides the infrastructure to enable developer workflows, sometimes known as the tooling factory. This can also be the same team as the IT one). Can [setup](https://docs.microsoft.com/en-us/azure/dev-box/quickstart-configure-dev-box-service?tabs=AzureADJoin) the hubs (called Dev Centers) and attach them to the correct network and identity stack, as well as define the DevBox sizings and images. They [hand out](https://docs.microsoft.com/en-us/azure/dev-box/how-to-project-admin) capacity to dev teams in the form of 'Projects' which enables delegated consumption of the resources.
* **Project/Development Team Manager** this person manages a project or pool of developers and [makes sure](https://docs.microsoft.com/en-us/azure/dev-box/how-to-dev-box-user) that the capacity can be consumed (and measured in terms of cost) by their team
* **Developer** they can [connect](https://docs.microsoft.com/en-us/azure/dev-box/tutorial-connect-to-dev-box-with-remote-desktop-app) to the developer portal of DevBox and setup their DevBox based on the assigned Projects and linked pools. Just like with Windows365, this spins up a VDI instantly which can be connected to using Azure AD credentials and a web- or RDP-based client.

### Architecture

The architecture of DevBox is quite simple, as most of its running components are embedded in the SaaS-layer of Windows365. It is only the management plane that gets exposed.

* **Dev Center**: this is a type of [hub resource](https://docs.microsoft.com/en-us/azure/dev-box/quickstart-configure-dev-box-service?tabs=AzureADJoin#create-a-dev-center) which brings together all downstream components into a centralized management workspace. It is here that the DevOps infra team can setup definitions, connections and projects. A user-assigned identity can be attached to this resource so it can integrate with other Azure resources (like the image gallery)
  * **Image Gallery**: although DevBox comes with the same Windows 10/11 images (with or without Microsoft 365 apps) out of the box, you can [upload](https://docs.microsoft.com/en-us/azure/dev-box/how-to-configure-azure-compute-gallery) your custom images to an Azure gallery and link it to the Dev Center. This allows you to create images with specific developer tools ready-to-go. The Dev Center requires read-access on the gallery resource for the integration.
  * **DevBox Definition**: here you can [combine](https://docs.microsoft.com/en-us/azure/dev-box/quickstart-configure-dev-box-service?tabs=AzureADJoin#create-a-dev-box-definition) a specific image, and a VM/Storage size. At the time of writing the sizes are 4CPU/16GB RAM and 8CPU/32GB RAM with a separately configurable SSD size of 256, 512 or 1024GB SSD). These definitions can be delegated (using Azure IAM) to allow project admins to consume a specific set of definitions. Although this resource can be updated, it will not update linked DevBoxes. If a change is needed on DevBox level, it must be recreated.
  * **Network Connection**: in order for the DevBoxes to attach to the correct Virtual Network/Subnet as well as perform the correct type of Azure AD join, one or more [network connections](https://docs.microsoft.com/en-us/azure/dev-box/quickstart-configure-dev-box-service?tabs=AzureADJoin#create-a-network-connection) need to be setup. These connections create a managed resource group that contains a network interface per DevBox attached to the target subnet. These connections can be delegated to project admins in the same sense as definitions to allow fine-grained consumption as needed. Although this resource can be updated, it will not update linked DevBoxes. If a change is needed on DevBox level, it must be recreated.
  * **Project**: represents a development team or project. This resource is provided to the project administrator using the 'DevCenter Project Admin' role (this can be deployed in a separate resource group for cohesion and cost centering with other resources of the project). The administrator can then further configure the resource as needed by the project.
    * **Pool**: a pool combines a specific Network Connection and DevBox Definition. It is from this resource that developers can request DevBox creation. The login type for developer can be set to either 'standard user' or 'local administrator' based on what is the company policy. Although this resource can be updated, it will not update linked DevBoxes. If a change is needed on DevBox level, it must be recreated.
    * **Developer Assignment**: the project admin can grant the 'DevCenter Dev Box User' role to their team. They can then go into the [DevBox Portal](https://devbox.microsoft.com/) and create a DevBox based on the pools defined in the project. From this point on, the experience is idential to Windows 365.

### Security & Governance

Because the solution integrates with Microsoft Endpoint Manager + Azure AD  for identity, and the Azure private network stack of your company for connectivity, the security posture of your existing device landscape is fully adopted by default in the DevBox setup. It is only the management of the DevBox delegated admin model and what can be requested by whom that a process should be in place before rolling out this service.

* What sizes and connections are allowed?
* What images will be available?
* Who will operate the service?
* How are projects to be requested, by whom?

### Costs

The [costs](https://azure.microsoft.com/en-us/pricing/details/dev-box/) for DevBox are terminated against the Project-resources for easy rebilling and tagging of consumed resources. As mentioned briefly before, the cost model involves:

* A Windows usage license per DevBox user (either a suite license like P1, E5 etc or individually acquired)
* A per hour running cost of
  * compute (which is suspended if the DevBox is not running)
  * storage (which keeps running until the DevBox is deleted)

### Automation

A [REST API](https://docs.microsoft.com/en-us/rest/api/devcenter/) is available to create the resources. I expect ARM/Bicep support soon to follow based on the API.

## Conclusion: DevBox versus Windows365/Azure Virtual Desktop

Whereas Windows365 requires additional, fixed cost licenses and is managed by the Office 365 administrator, a DevBox can be thrown at a developer by their manager. The developer can then spin up and down the DevBox as needed (I expect auto-start/stop to be a feature quite soon).

Whereas Azure Virtual Desktop required a rollout and management of additional pools via a central IT team, with DevBox a combination of DevOps infra teams and project administrators can set up all the infrastructure and instantly enable developers to consume it.

**The value proposition of DevBox is the fact that you can offload the IT cost & operational overhead** in the spin-up and down of developer VDI's. This by providing a self-service platform operated by a project manager and team on a per-need base, with automatic distribution of costs.



## Want to try it out?

There is a preview discount where you get some DevBox compute and storage capacity for free for some hours per month! So play with it and see for yourself what it can bring to your table! The best place to get started is the [QuickStart](https://docs.microsoft.com/en-us/azure/dev-box/quickstart-configure-dev-box-service) in the documentation.

Enjoy!