---
title: Detailed SSH troubleshooting for an Azure VM
description: More detailed SSH troubleshooting steps for issues connecting to an Azure virtual machine
keywords: ssh connection refused,ssh error,azure ssh,SSH connection failed
services: virtual-machines
documentationcenter: ''
author: JarrettRenshaw
manager: dcscontentpm
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.custom: sap:Cannot connect to my VM, linux-related-content
ms.service: azure-virtual-machines

ms.collection: linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux

ms.topic: troubleshooting
ms.date: 07/22/2024
ms.author: jarrettr
---
# Detailed SSH troubleshooting steps for issues connecting to a Linux VM in Azure

**Applies to:** :heavy_check_mark: Linux VMs

There are many possible reasons that the SSH client might not be able to reach the SSH service on the VM. If you have followed through the more [general SSH troubleshooting steps](troubleshoot-ssh-connection.md), you need to further troubleshoot the connection issue. This article guides you through detailed troubleshooting steps to determine where the SSH connection is failing and how to resolve it.

## Take preliminary steps

The following diagram shows the components that are involved.

:::image type="content" source="media/detailed-troubleshoot-ssh-connection/components-of-ssh-service.png" alt-text="Diagram that shows components of SSH service.":::

The following steps help you isolate the source of the failure and figure out solutions or workarounds.

1. Check the status of the VM in the portal.
   In the [Azure portal](https://portal.azure.com), select **Virtual machines** > *VM name*.

   The status pane for the VM should show **Running**. Scroll down to show recent activity for compute, storage, and network resources.

2. Select **Settings** to examine endpoints, IP addresses, network security groups, and other settings.

   The VM should have an endpoint defined for SSH traffic that you can view in **Endpoints** or **[Network security group](/azure/virtual-network/network-security-groups-overview)**. Endpoints in VMs that were created by using Resource Manager are stored in a network security group. Verify that the rules have been applied to the network security group and are referenced in the subnet.

To verify network connectivity, check the configured endpoints and see if you can connect to the VM through another protocol, such as HTTP or another service.

After these steps, try the SSH connection again.

## Find the source of the issue

The SSH client on your computer might fail to connect to the SSH service on the Azure VM due to issues or misconfigurations in the following areas:

* [SSH client computer](#source-1-ssh-client-computer)
* [Organization edge device](#source-2-organization-edge-device)
* [Network security groups](#source-3-network-security-groups)
* [Linux-based Azure VM](#source-4-linux-based-azure-virtual-machine)

## Source 1: SSH client computer

To eliminate your computer as the source of the failure, verify that it can make SSH connections to another on-premises, Linux-based computer.

:::image type="content" source="media/detailed-troubleshoot-ssh-connection/ssh-client-computer-components.png" alt-text="Diagram that highlights SSH client computer components.":::

If the connection fails, check for the following issues on your computer:

* A local firewall setting that is blocking inbound or outbound SSH traffic (TCP 22)
* Locally installed client proxy software that is preventing SSH connections
* Locally installed network monitoring software that is preventing SSH connections
* Other types of security software that either monitor traffic or allow/disallow specific types of traffic

If one of these conditions apply, temporarily disable the software and try an SSH connection to an on-premises computer to find out the reason the connection is being blocked on your computer. Then work with your network administrator to correct the software settings to allow SSH connections.

If you are using certificate authentication, verify that you have these permissions to the .ssh folder in your home directory:

* Chmod 700 ~/.ssh
* Chmod 644 ~/.ssh/\*.pub
* Chmod 600 ~/.ssh/id_rsa (or any other files that have your private keys stored in them)
* Chmod 644 ~/.ssh/known_hosts (contains hosts that you’ve connected to via SSH)

## Source 2: Organization edge device

To eliminate your organization edge device as the source of the failure, verify that a computer directly connected to the Internet can make SSH connections to your Azure VM. If you are accessing the VM over a site-to-site VPN or an Azure ExpressRoute connection, skip to [Source 3: Network security groups](#nsg).

:::image type="content" source="media/detailed-troubleshoot-ssh-connection/organization-edge-device.png" alt-text="Diagram that highlights organization edge device.":::

If you don't have a computer that is directly connected to the Internet, create a new Azure VM in its own resource group or cloud service and use that new VM. For more information, see [Create a virtual machine running Linux in Azure](/azure/virtual-machines/linux/quick-create-cli). Delete the resource group or VM and cloud service when you're done with your testing.

If you can create an SSH connection with a computer that's directly connected to the Internet, check your organization edge device for:

* An internal firewall that's blocking SSH traffic with the Internet
* A proxy server that's preventing SSH connections
* Intrusion detection or network monitoring software running on devices in your edge network that's preventing SSH connections

Work with your network administrator to correct the settings of your organization edge devices to allow SSH traffic with the Internet.

<a id="nsg"></a>
## Source 3: Network security groups

Network security groups enable you to have more granular control of allowed inbound and outbound traffic. You can create rules that span subnets and cloud services in an Azure virtual network. Check your network security group rules to ensure that SSH traffic to and from the Internet is allowed.
For more information, see [About network security groups](/azure/virtual-network/network-security-groups-overview).

You can also use IP Verify to validate the NSG configuration. For more information, see [Azure network monitoring overview](/azure/network-watcher/network-watcher-monitoring-overview).

## Source 4: Linux-based Azure virtual machine

The last source of possible problems is the Azure virtual machine itself.

:::image type="content" source="media/detailed-troubleshoot-ssh-connection/linux-based-azure-virtual-machine.png" alt-text="Diagram that highlights Linux-based Azure virtual machine.":::

If you haven't done so already, follow the instructions [to reset a password Linux-based virtual machines](./reset-password.md).

Try connecting from your computer again. If it still fails, the following are some of the possible issues:

* The SSH service is not running on the target virtual machine.
* The SSH service is not listening on TCP port 22. To test, install a telnet client on your local computer and run "telnet *cloudServiceName*.cloudapp.net 22". This step determines if the virtual machine allows inbound and outbound communication to the SSH endpoint.
* The local firewall on the target virtual machine has rules that are preventing inbound or outbound SSH traffic.
* Intrusion detection or network monitoring software that's running on the Azure virtual machine is preventing SSH connections.

## Additional resources

For more information about troubleshooting application access, see [Troubleshoot access to an application running on an Azure virtual machine](../windows/troubleshoot-app-connection.md).

[!INCLUDE [Azure Help Support](../../../includes/azure-help-support.md)]
