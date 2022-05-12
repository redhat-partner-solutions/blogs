# Deploying RHCS using minimum hardware resources and efficiently making it work for multiple clusters via ODF

Having a software defined storage fabric such as Red Hat Ceph Storage (RHCS) is advantageous to provide file, block and object storage services for workloads deployed on top of Openshift. In many cases Openshift clusters are required to have a storage solution in place that is highly available for multiple clusters to leverage and which can be easily deployed using a minimum hardware footprint. The approach described below can help use the hardware resources efficiently.

This blog post highlights how using RHCS in External Mode and Openshift Data Fabric (ODF) together can achieve the desired flexibility for our storage needs which are required by multiple clusters running on distinct physical servers. We will demonstrate this by deploying multiple Openshift clusters and RHCS on top of 3 physical distinct servers with available NVMe drives. To fully achieve this we will make use of the Crucible automation project (which is a set of ansible playbooks used to deploy a Openshift cluster and all its prerequisites), RHCS 5 and ODF 4.9.6. The goal for this article is to underline all important steps required to set up a minimum hardware environment for RHCS and ODF to work together.

# Setting up the Lab Environment
In order to run trails on a minimum hardware footprint, the lab environment is set up on top of 4 bare metal server nodes. One of the nodes will be the bastion/ansible host node that will be used to set up the OCP and RHCS clusters on the other remaining 3 bare metal nodes.

Using the crucible automation (which is mentioned later in the documentation), RHEL KVM will be deployed on 3 bare metal server nodes and for both OCP clusters, and separate individual VMs will be created on top of each bare metal node. Separate VLANs and networks will be created for both the OCP clusters and the RHCS cluster. The BM hardware specification as well the VM hardware specification are listed below.

The RHCS cluster will also be deployed on top of the 3 BM nodes using the installation guide that this documentation covers. Similar to the OCP clusters, the RHCS cluster will also be deployed on a separate VLAN and network in order to ensure and validate that the RHCS cluster can run in “external mode” efficiently. Each of the BM nodes consist of 3 x NVMe free drives as mentioned in the hardware specification and those will be leveraged inside our RHCS cluster, so in total we will have 9 OSDs configured.

![Sync Background](rhcs5_odf_external/images/IMAGE1.png)

