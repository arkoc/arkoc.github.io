---
layout: post
title: Azure, V-Net, Database, VPN, GOOD!


---

The title is Pretty. Much. And explanatory. 

-- Who are we?

-- We are developers!

-- What we need? 

-- Access to production databases!

Ok, our task is pretty simple. Create a virtual network for the production environment. Then create an Azure PostgresSQL Database and join it to v-net. And in the end, create VPN Gateway to this v-net so developers can get access to production databases through it. Sounds simple and straightforward. Let's start:

- [How to Create Virtual Network](https://docs.microsoft.com/th-th/azure/virtual-network/quick-create-portal)

- [How to Create Azure PostgresSQL Database](https://docs.microsoft.com/en-us/azure/postgresql/quickstart-create-server-database-portal)

- [How to Integrate PostgresSQL database with V-Net](https://docs.microsoft.com/en-us/azure/postgresql/howto-manage-vnet-using-portal#create-a-vnet-rule-and-enable-service-endpoints-in-the-azure-portal)

- [How to Create VPN Gateway to V-Net](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal)

Don't you say you were expecting a huge useless novel about all these "how to"-s? Go and hit those links. Microsoft document ninjas did a really good job. Thank you, Microsoft. #mymicrosoft #microsoftloveslinux #microsoftloveslinkedin #microsoftlovesgithub #microsoftlovesnatasha #microsoftlovesyou

So let's go underwater to find those fucking stones.

<!--more-->

First underwater stone is right here. We didn't ask one more right question.

- [How to Create Private-Link to Azure PostgresSQL Database.](https://docs.microsoft.com/en-us/azure/postgresql/howto-configure-privatelink-portal)

Azure PostgresSQL Database is managed service. It is not technically joining your virtual network. It is just with some voodoo magic allowing your azure managed resources to access it. So what is wrong with this. Managed is wrong. It is allowing only managed resources to have access to them through [Service Endpoints (in other words voodoo magic)](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-service-endpoints-overview). But what was our task? To allow VPN access for developers. Are developers managed service? I do not think so. So what can we do? Here is when Private-Link comes into play. Imagine Service Endpoints and Private Links just simple proxy applications. They just get deployed on your v-net and proxy everything to managed services behind. Also, your proxy application receives local IP which can be used to access managed service. So our developers can access the managed database through VPN. So they happy. Once Abraham Lincoln said, Happy developers never ... Nah, just developers should be happy. So let's ask that question once more. [How to Create Private-Link to Azure PostgresSQL Database.](https://docs.microsoft.com/en-us/azure/postgresql/howto-configure-privatelink-portal)

Virtual Networks works pretty stable. If you are clicking, knocking, swearing changing, and trying everything, so better consider that you can break it. In those situations just remove resources from v-net and then attach them back. In most scenarios, this helps to reset to starting position. This means, be CAREFUL when working with production virtual networks. Otherwise, great_business_service customers will see an error page for a couple of minutes. It is a disaster, no?

And yet another one. If you are a poor developer who doesn't want to instantly pay more than 50$ for the database, which is not going to be used at the start that much, you just suck. Yeah. You can't do that. Because [Support for VNet service endpoints is only for General Purpose and Memory Optimized servers.](https://docs.microsoft.com/en-us/azure/postgresql/concepts-limits#vnet-service-endpoints) #microsoftlovesnatashanotyou

If PostgresSQL says something like this: "server is not configured to allow ipv6 connections" check credentials and make sure you have enabled SQL service endpoint enabled for the subnet from which you are connecting to the database.

This one is not an underwater stone, but something that is still confusing for me. If you open V-Net subnets, you will see GatewaySubnet which has some address space for it, like: "10.7.0.0/24". I understand that if I connect to VPN I should receive from one of those addresses. But unfortunately no, I received something like "172.17.0.2". So there is another option which is affecting our IP. And that option is in Virtual Network Gateway resource, under User VPN configuration section, Address Pool. That's it. 

Seems it. Thats what I remmembered.
