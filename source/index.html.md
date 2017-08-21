---
title: Some IBM Cloud private Q&As Documentation

language_tabs:
  - javascript: Node
  - java: Java

includes:

search: true
---
# Some Q&As

_The HA functionality was removed from the CE edition of CFC between versions 1.1 and 1.2.  
We were using this functionality with version 1.1 and would like to continue to use it. 
Will the HA functionality be part of the CE edition in the future or will it be made available separately?_

No, that's the difference between the CE and the Enterprise (paid) edition: the EE provides the capability of having multiple Kubernetes masters

___

_Is it possible to reduce the price of a license in version 1.2 by restricting its scope to just the Kubernetes functionality (Calico, Helm, Nginx etc.)?_

The ICp price is being re-evaluated, with different pricing options that will include some license for IBM contents.

For now, the ICp 1.2 pricing for the basic Kubernetes functionality, and the IBM content is priced separately.

___

_Is it possible to reduce the price of IBM Cloud private by using it without Bluemix products and ecosystem?_

Again, the price for the platform, not for IBM contents.

___

_Is it possible with CFC version 1.1 to update referenced products (Ansible, Calico, etcd, Heapster, Helm, Keystone, kube-dns) via Docker Update or do we need version 1.2 for this?_

I don't know about CFC version 1.1

___

_We’re running CFC version 1.1 on a CPU pool for which we have WebSphere ND b for each cluster we’re running or will one VPC license cover any number of clusters?_

I don't know about CFC v1.1

___

_Is there a way to use CFC CE version 1.1 in a production environment without incurring license costs e.g. without S&S?_

I am not familiar with the CFC CE v1.1 capability, but the questions I would have are  

* does it provide the capability of having multiple Kubernetes master?
* does the client need support for the platform?

___

_https://www.ibm.com/support/knowledgecenter/SSBS6K_1.2.0/getting_started/whats_new.html IBM Cloud private-ce provides a free, limited offering that is ideal for test environments. What are the exact limitations of this “limited offering”?  Can it also be used free in a production environment?_

* The CE edition limits the deployment to a single Kubernetes master, incurring to having a Single Point of Failure. If any client needs to run in production, he would want to have multiple masters, which requires the Enterprise Edition

* The CE version doesn't provide *any* support.

___

_Can IBM confirm that the internal Kubernetes network (Master-Node/Worker-Node/Proxy-Node) is not visible to external systems? i.e. The IP addresses in the `service_cluster_ip_range` and `network_cidr` of a Calico network are only visible from within the Kubernetes network._

This is true only if BGP has not been configured

The `service_cluster_ip_range` is always internal only

The `network_cidr`, though, can be accessible from the world if BGP has been configured between the enterprise network and the calico node

___

_We followed the CFC installation instructions to install the HA functionality but we’re missing usage instructions. Could you send us the documentation (or a link to it) detailing exactly how the HA functionality works and how to use it._

TBD

___

_Please provide (a link to) the documentation for the configuration of (nginx) sticky sessions (cookie based) in CFC._

It is the same as any nginx session persistence configuration, documented at [nginx Session Persistence](https://www.nginx.com/products/session-persistence/)

___

_Is CF going to be available? When?_
Cloud Foundry will be available in an un-tethered (unmanaged), product form factor in our Oct release. 

___

_Will the CF versions be aligned with the dedicated/Public ones or will have a complete separate life?_
The components of the CF installation will be aligned with dedicated and public, as they are today. The installation, update, service monitoring will be the responsibility of the client (aligning with other market competitors).

___
_What is the difference between NSX and NSX-V?_

NSX-V is the NSX version for vSphere that directly integrates with vCenter. 

___
Is ICp going to be compatible with NSX (that is, can we install it on top of VMWare/NSX?)? When?

ICp 1.2 can be installed on top of NSX-V today (toleration). There is an ongoing collaboration with VMWare to support deeper integration with the upcoming NSX-T (focused on flat networks for containers and VMs). That support will be available shortly after the release of the NSX-T product. 

Presumably the above implies that VXLANs in NSX are tolerated by ICP.

___
_Is CF within ICP going to be compatible with NSX (that is can we install it on top VMWare/NSX)? Will CF tolerate the use of VXLANs, we have heard rumours that this could cause issues._

VXLANs are invisible at ICP level. Calico works at the layer 3 while VXLAN works on layer 2. How is this going to affect the platform, presumably it does not?

___
_Do we need to provision just one big VM, several?-if one, can we make use of different IP’s on different networks at ICp level (assuming yes)?_

___
_What is the recommended size of a VM underpinning an ICP worker node?_
According to this, the minimum is 1 virtual CPU. https://www.ibm.com/support/knowledgecenter/en/SSBS6K_1.2.0/supported_system_config/hardware_reqs.html "Choose a modern processor with multiple cores. Common clusters use 2 - 8 core machines. When it comes to choosing between faster CPUs or more cores, it is recommended to choose hosts with more cores."

According to this, the minimum is 4 GB RAM. https://www.ibm.com/support/knowledgecenter/en/SSBS6K_1.2.0/supported_system_config/hardware_reqs.html Should we not have more RAM when more CPUs are associated with the worker node VMs?

___
_If many, could those VMs be on a different network (externally facing VXLAN)_
Presumably yes, we need a diagram before we can answer this.

___
_If yes, does calico allows communication between them while VXLANs are not allowed to talk between them (assuming yes)?_

Question needs clarification.

___
_Is ICp going to levarage NSX for network segmentation in CF? When?_
There are no plans today to take advantage of NSX to support micro-segmentation of application workloads.
___
_Can we still provide segmentation using calico at the OS level for CF?_
No, segmentation is done through this: https://docs.cloudfoundry.org/adminguide/routing-is.html

___
_Can we provide network segmentation for CF through other means?_

Josh Packer confirmed that this is supported through the use of port groups. We need a diagram to clarify exactly how this works.

___
_Can we have (is it supported) multiple CF instances in one single ICp instance (installation)?_

___
_Is ICp going to leverage NSX-T for network segmentation in K8? When?_
Collaboration is underway to integrate directly with NSX-T to manage the underlying networking layer from k8. That support will be available shortly after the release of the NSX-T product.

This is good news, as confirmed on today's call this is in plan for delivery in our  Oct release.

___
_Is Mesos going to be available in the future? When?_
No, we have removed support for the Mesos orchestration layer from the product in order to focus completely on k8 as the distributed system orchestration. 

___
_Can we guarantee that all CF and K8 APIs will be accessible from day1 now and in the future? (This is extremely important since we've been using the fact that OpenShift does not do it as one of the main reasons to step away from it)._
Yes. We already expose all CF API, and we are following the same open strategy with kubernetes API as well. We will likely have additional API around services like user on boarding, certificate management, etc that appends additional new function; but we are not constricting the kubernetes API in anyway.

This is again good news!

___
_What out of the box services will be available in October? The ones in the right hand side of the below picture? Does it mean they come with the platform? Node-Red not suppported?_
The list of content planned for Oct includes: 

* DSX
* Db2 Warehouse (formerly dashDB)
* Message Hub•API Connect  
* Redis 
* Qradar Deployment / Integration 
* IBM Integration Broker (IIB)
* Node.js 
* GoLang
* Liberty
* API access to Object Store 
* Cloudant
* Redis
* MongoDB
* Elasticsearch
* Cloud Foundry

Open source runtimes will be included and supported on a best-effort basis. IBM products will still be licensed, but there is work under way to provide usage based pricing. These workloads will be entitled to full enterprise support as they always have. 

___
_What level of scalability will the platform be able to support for K8? (1K containers? 10K? 100K?). Same for CF._
We have tested 2K pods (which equated to 2K containers) running across 300 nodes (on x86_64) and also 60 nodes on Power. Kubernetes officially supports up to 1K nodes (3x larger than our tested range).
I believe on today's call, you suggested that we intend to test with 1K nodes to run 10K containers for our Oct release, we need confirmation however.
We should keep in mind that Santander claims to have 12k containers running on their OpenStack/OpenShift platform today, so it would be great to be able to point to a number > 2K.

___
_Will we support Network Policies in ICP right as in https://kubernetes.io/docs/concepts/services-networking/network-policies/?_
Yes, Network Policies are supported today: https://www.ibm.com/support/knowledgecenter/en/SSBS6K_1.2.0/manage_network/set_network_policy.html

___

_Will the RBAC (role-based access control) solution in the Oct release be enhanced compared to the two roles (admin and developer) available in ICP 1.2 today_

___
_We believe that ICP today (1.2) supports multi-tenancy, however there is only support for a single admin user. This limits multi-tenancy for clients in the field, can you confirm that this is the case. And what should we expect fog ICP 2.1 (Oct)?_

___
_Is user management in ICP separate from CF, or do we use the same authentication manager/keystone?_

___
_Assuming we have consolidated user management for ICP and CF, how do we support multi-tenancy in CF? Presumably this is done through CF organisations._

___
_Does ICP support the use of multiple LDAPs within a single ICP instance (cluster)? This would be required for some multi-tenancy scenarios with service providers. And if not, do we provide explicit support for integration with Tivoli Directory Integrator?_

___
_Can we have multiple VIP addresses for the proxy nodes on different VLAN / IP ranges?_
We support multiple VIP addresses by having multiple proxy nodes, each with its own VIP. This would require manual configuration though since the default installation does not facilitate this. We need confirmation as well as documentation for this.

In addition there is an outstanding question whether a single proxy node can support multiple VIPs.

___

_Can I have the same boot node for multiple ICp installations? This might simplify the deployment in the field._
