---
title: "Az Azure parancssori felület használatával több IP-konfigurációk terheléselosztási |} Microsoft Docs"
description: "Megtudhatja, hogyan több IP-címek hozzárendelése a virtuális gép Azure parancssori felületével |} Erőforrás-kezelő."
services: virtual-network
documentationcenter: na
author: anavinahar
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/10/2017
ms.author: annahar
ms.openlocfilehash: bd15713752ea01ad403d8e3dcfed0c9a7adcc7fa
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="load-balancing-on-multiple-ip-configurations"></a><span data-ttu-id="da8dc-103">Terheléselosztás több IP-konfigurációk</span><span class="sxs-lookup"><span data-stu-id="da8dc-103">Load balancing on multiple IP configurations</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="da8dc-104">Portal</span><span class="sxs-lookup"><span data-stu-id="da8dc-104">Portal</span></span>](load-balancer-multiple-ip.md)
> * [<span data-ttu-id="da8dc-105">Parancssori felület</span><span class="sxs-lookup"><span data-stu-id="da8dc-105">CLI</span></span>](load-balancer-multiple-ip-cli.md)
> * [<span data-ttu-id="da8dc-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="da8dc-106">PowerShell</span></span>](load-balancer-multiple-ip-powershell.md)

<span data-ttu-id="da8dc-107">Ez a cikk ismerteti az Azure Load Balancer használata a másodlagos hálózati adapteren (NIC) több IP-címmel.</span><span class="sxs-lookup"><span data-stu-id="da8dc-107">This article describes how to use Azure Load Balancer with multiple IP addresses on a secondary network interface (NIC).</span></span> <span data-ttu-id="da8dc-108">Ebben a forgatókönyvben két virtuális gépeken futó Windows, az elsődleges és másodlagos hálózati tudunk</span><span class="sxs-lookup"><span data-stu-id="da8dc-108">For this scenario, we have two VMs running Windows, each with a primary and a secondary NIC.</span></span> <span data-ttu-id="da8dc-109">A másodlagos hálózati adapterrel rendelkezhetnek két IP-konfigurációk.</span><span class="sxs-lookup"><span data-stu-id="da8dc-109">Each of the secondary NICs have two IP configurations.</span></span> <span data-ttu-id="da8dc-110">Minden virtuális gép webhelyeket a contoso.com és fabrikam.com üzemelteti.</span><span class="sxs-lookup"><span data-stu-id="da8dc-110">Each VM hosts both websites contoso.com and fabrikam.com.</span></span> <span data-ttu-id="da8dc-111">Minden webhelyre van kötve egy IP-konfigurációk másodlagos hálózati adapteren</span><span class="sxs-lookup"><span data-stu-id="da8dc-111">Each website is bound to one of the IP configurations on the secondary NIC.</span></span> <span data-ttu-id="da8dc-112">Azure Load Balancer használatával teszi közzé a két előtérbeli IP-cím, egy, a megfelelő IP-konfiguráció a webhelyre irányuló forgalom terjeszteni minden webhelyre vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="da8dc-112">We use Azure Load Balancer to expose two frontend IP addresses, one for each website, to distribute traffic to the respective IP configuration for the website.</span></span> <span data-ttu-id="da8dc-113">Ebben a példában ugyanazt a portszámot is frontends, valamint mindkét háttér címkészletet IP-címek között.</span><span class="sxs-lookup"><span data-stu-id="da8dc-113">This scenario uses the same port number across both frontends, as well as both backend pool IP addresses.</span></span>

![Terheléselosztó forgatókönyv kép](./media/load-balancer-multiple-ip/lb-multi-ip.PNG)

## <a name="steps-to-load-balance-on-multiple-ip-configurations"></a><span data-ttu-id="da8dc-115">A több IP-konfigurációk terheléselosztásához lépései</span><span class="sxs-lookup"><span data-stu-id="da8dc-115">Steps to load balance on multiple IP configurations</span></span>

<span data-ttu-id="da8dc-116">A cikkben ismertetett forgatókönyvben eléréséhez az alábbi lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="da8dc-116">Follow the steps below to achieve the scenario outlined in this article:</span></span>

1. <span data-ttu-id="da8dc-117">[Telepítse és konfigurálja az Azure parancssori felület](../cli-install-nodejs.md) az Azure parancssori felület lépésekkel a csatolt cikk, majd jelentkezzen be Azure-fiókjába.</span><span class="sxs-lookup"><span data-stu-id="da8dc-117">[Install and Configure the Azure CLI](../cli-install-nodejs.md) the Azure CLI by following the steps in the linked article and log into your Azure account.</span></span>
2. <span data-ttu-id="da8dc-118">[Hozzon létre egy erőforráscsoportot](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-resource-group) nevű *contosofabrikam* fent leírt módon.</span><span class="sxs-lookup"><span data-stu-id="da8dc-118">[Create a resource group](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-resource-group) called *contosofabrikam* as described above.</span></span>

    ```azurecli
    azure group create contosofabrikam westcentralus
    ```

3. <span data-ttu-id="da8dc-119">[Egy rendelkezésre állási csoport létrehozása](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-an-availability-set) való lévő két virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="da8dc-119">[Create an availability set](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-an-availability-set) to for the two VMs.</span></span> <span data-ttu-id="da8dc-120">A jelen esetben használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="da8dc-120">For this scenario, use the following command:</span></span>

    ```azurecli
    azure availset create --resource-group contosofabrikam --location westcentralus --name myAvailabilitySet
    ```

4. <span data-ttu-id="da8dc-121">[Hozzon létre egy virtuális hálózatot](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-virtual-network-and-subnet) nevű *myVNet* és alhálózat nevű *mySubnet*:</span><span class="sxs-lookup"><span data-stu-id="da8dc-121">[Create a virtual network](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-virtual-network-and-subnet) called *myVNet* and a subnet called *mySubnet*:</span></span>

    ```azurecli
    azure network vnet create --resource-group contosofabrikam --name myVnet --address-prefixes 10.0.0.0/16  --location westcentralus

    azure network vnet subnet create --resource-group contosofabrikam --vnet-name myVnet --name mySubnet --address-prefix 10.0.0.0/24
    ```

5. <span data-ttu-id="da8dc-122">[A terheléselosztó létrehozása](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json) nevű *mylb*:</span><span class="sxs-lookup"><span data-stu-id="da8dc-122">[Create the load balancer](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json) called *mylb*:</span></span>

    ```azurecli
    azure network lb create --resource-group contosofabrikam --location westcentralus --name mylb
    ```

6. <span data-ttu-id="da8dc-123">Hozzon létre két dinamikus nyilvános IP-címet a terheléselosztó előtérbeli IP-konfigurációk:</span><span class="sxs-lookup"><span data-stu-id="da8dc-123">Create two dynamic public IP addresses for the frontend IP configurations of your load balancer:</span></span>

    ```azurecli
    azure network public-ip create --resource-group contosofabrikam --location westcentralus --name PublicIp1 --domain-name-label contoso --allocation-method Dynamic

    azure network public-ip create --resource-group contosofabrikam --location westcentralus --name PublicIp2 --domain-name-label fabrikam --allocation-method Dynamic
    ```

7. <span data-ttu-id="da8dc-124">A két előtérbeli IP-konfigurációk létrehozása *contosofe* és *fabrikamfe* osztályban:</span><span class="sxs-lookup"><span data-stu-id="da8dc-124">Create the two frontend IP configurations, *contosofe* and *fabrikamfe* respectively:</span></span>

    ```azurecli
    azure network lb frontend-ip create --resource-group contosofabrikam --lb-name mylb --public-ip-name PublicIp1 --name contosofe
    azure network lb frontend-ip create --resource-group contosofabrikam --lb-name mylb --public-ip-name PublicIp2 --name fabrkamfe
    ```

8. <span data-ttu-id="da8dc-125">A háttéralkalmazás - címkészletek létrehozása *contosopool* és *fabrikampool*, egy [mintavételi](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json) - *HTTP*, és a terheléselosztási szabályok - *HTTPc* és *HTTPf*:</span><span class="sxs-lookup"><span data-stu-id="da8dc-125">Create your backend address pools - *contosopool* and *fabrikampool*, a [probe](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json) - *HTTP*, and your load balancing rules - *HTTPc* and *HTTPf*:</span></span>

    ```azurecli
    azure network lb address-pool create --resource-group contosofabrikam --lb-name mylb --name contosopool
    azure network lb address-pool create --resource-group contosofabrikam --lb-name mylb --name fabrikampool

    azure network lb probe create --resource-group contosofabrikam --lb-name mylb --name HTTP --protocol "http" --interval 15 --count 2 --path index.html

    azure network lb rule create --resource-group contosofabrikam --lb-name mylb --name HTTPc --protocol tcp --probe-name http--frontend-port 5000 --backend-port 5000 --frontend-ip-name contosofe --backend-address-pool-name contosopool
    azure network lb rule create --resource-group contosofabrikam --lb-name mylb --name HTTPf --protocol tcp --probe-name http --frontend-port 5000 --backend-port 5000 --frontend-ip-name fabrkamfe --backend-address-pool-name fabrikampool
    ```

9. <span data-ttu-id="da8dc-126">A következő parancsot az alábbi, és ellenőrizze a kimenet [ellenőrizze a terheléselosztó](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json) megfelelően lett létrehozva:</span><span class="sxs-lookup"><span data-stu-id="da8dc-126">Run the following command below and then check the output to [verify your load balancer](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json) was created correctly:</span></span>

    ```azurecli
    azure network lb show --resource-group contosofabrikam --name mylb
    ```

10. <span data-ttu-id="da8dc-127">[Hozzon létre egy nyilvános IP-cím](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-public-ip-address), *myPublicIp*, és [tárfiók](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json), *mystorageaccont1* az első virtuális gépen VM1 alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="da8dc-127">[Create a public IP](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-public-ip-address), *myPublicIp*, and [storage account](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json), *mystorageaccont1* for your first virtual machine VM1 as shown below:</span></span>

    ```azurecli
    azure network public-ip create --resource-group contosofabrikam --location westcentralus --name myPublicIP --domain-name-label mypublicdns345 --allocation-method Dynamic

    azure storage account create --location westcentralus --resource-group contosofabrikam --kind Storage --sku-name GRS mystorageaccount1
    ```

11. <span data-ttu-id="da8dc-128">[A hálózati adapterek létrehozása](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-virtual-nic) VM1 számára, és adja hozzá a második IP-konfiguráció *VM1-ipconfig2*, és [a virtuális gép létrehozása](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-the-linux-vms) alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="da8dc-128">[Create the network interfaces](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-virtual-nic) for VM1 and add a second IP configuration, *VM1-ipconfig2*, and [create the VM](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-the-linux-vms) as shown below:</span></span>

    ```azurecli
    azure network nic create --resource-group contosofabrikam --location westcentralus --subnet-vnet-name myVnet --subnet-name mySubnet --name VM1Nic1 --ip-config-name NIC1-ipconfig1
    azure network nic create --resource-group contosofabrikam --location westcentralus --subnet-vnet-name myVnet --subnet-name mySubnet --name VM1Nic2 --ip-config-name VM1-ipconfig1 --public-ip-name myPublicIP --lb-address-pool-ids "/subscriptions/<your subscription ID>/resourceGroups/contosofabrikam/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/contosopool"
    azure network nic ip-config create --resource-group contosofabrikam --nic-name VM1Nic2 --name VM1-ipconfig2 --lb-address-pool-ids "/subscriptions/<your subscription ID>/resourceGroups/contosofabrikam/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/fabrikampool"
    azure vm create --resource-group contosofabrikam --name VM1 --location westcentralus --os-type linux --nic-names VM1Nic1,VM1Nic2  --vnet-name VNet1 --vnet-subnet-name Subnet1 --availset-name myAvailabilitySet --vm-size Standard_DS3_v2 --storage-account-name mystorageaccount1 --image-urn canonical:UbuntuServer:16.04.0-LTS:latest --admin-username <your username>  --admin-password <your password>
    ```

12. <span data-ttu-id="da8dc-129">A második virtuális gép ismételje meg a 10-11:</span><span class="sxs-lookup"><span data-stu-id="da8dc-129">Repeat steps 10-11 for your second VM:</span></span>

    ```azurecli
    azure network public-ip create --resource-group contosofabrikam --location westcentralus --name myPublicIP2 --domain-name-label mypublicdns785 --allocation-method Dynamic
    azure storage account create --location westcentralus --resource-group contosofabrikam --kind Storage --sku-name GRS mystorageaccount2
    azure network nic create --resource-group contosofabrikam --location westcentralus --subnet-vnet-name myVnet --subnet-name mySubnet --name VM2Nic1
    azure network nic create --resource-group contosofabrikam --location westcentralus --subnet-vnet-name myVnet --subnet-name mySubnet --name VM2Nic2 --ip-config-name VM2-ipconfig1 --public-ip-name myPublicIP2 --lb-address-pool-ids "/subscriptions/<your subscription ID>/resourceGroups/contosofabrikam/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/contosopool"
    azure network nic ip-config create --resource-group contosofabrikam --nic-name VM2Nic2 --name VM2-ipconfig2 --lb-address-pool-ids "/subscriptions/<your subscription ID>/resourceGroups/contosofabrikam/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/fabrikampool"
    azure vm create --resource-group contosofabrikam --name VM2 --location westcentralus --os-type linux --nic-names VM2Nic1,VM2Nic2 --vnet-name VNet1 --vnet-subnet-name Subnet1 --availset-name myAvailabilitySet --vm-size Standard_DS3_v2 --storage-account-name mystorageaccount2 --image-urn canonical:UbuntuServer:16.04.0-LTS:latest --admin-username <your username>  --admin-password <your password>
    ```

13. <span data-ttu-id="da8dc-130">Végül konfigurálnia kell DNS-erőforrásrekordok a terheléselosztó megfelelő előtérbeli IP-címére mutasson.</span><span class="sxs-lookup"><span data-stu-id="da8dc-130">Finally, you must configure DNS resource records to point to the respective frontend IP address of the Load Balancer.</span></span> <span data-ttu-id="da8dc-131">A tartományok Azure DNS-ben is tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="da8dc-131">You may host your domains in Azure DNS.</span></span> <span data-ttu-id="da8dc-132">Az Azure DNS-sel terheléselosztással kapcsolatos további információkért lásd: [Azure DNS használata más Azure-szolgáltatásokkal](../dns/dns-for-azure-services.md).</span><span class="sxs-lookup"><span data-stu-id="da8dc-132">For more information about using Azure DNS with Load Balancer, see [Using Azure DNS with other Azure services](../dns/dns-for-azure-services.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="da8dc-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="da8dc-133">Next steps</span></span>
- <span data-ttu-id="da8dc-134">További tudnivalók az Azure a terheléselosztási egyesítése [terheléselosztás szolgáltatások használata az Azure-ban](../traffic-manager/traffic-manager-load-balancing-azure.md).</span><span class="sxs-lookup"><span data-stu-id="da8dc-134">Learn more about how to combine load balancing services in Azure in [Using load-balancing services in Azure](../traffic-manager/traffic-manager-load-balancing-azure.md).</span></span>
- <span data-ttu-id="da8dc-135">Ismerje meg, hogyan használhatja különféle naplók az Azure-ban kezelésére és hibaelhárítására terheléselosztó [analytics keresse meg a Azure terheléselosztó](../load-balancer/load-balancer-monitor-log.md).</span><span class="sxs-lookup"><span data-stu-id="da8dc-135">Learn how you can use different types of logs in Azure to manage and troubleshoot load balancer in [Log analytics for Azure Load Balancer](../load-balancer/load-balancer-monitor-log.md).</span></span>
