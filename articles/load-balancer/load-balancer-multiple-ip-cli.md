---
title: "az Azure parancssori felület használatával több IP-konfigurációk terheléselosztási aaaLoad |} Microsoft Docs"
description: "Ismerje meg, hogyan tooassign több IP-címek tooa virtuális gép Azure parancssori felületével |} Erőforrás-kezelő."
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
ms.openlocfilehash: df81e1b8193f274bad435d6b506c7be824117416
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="load-balancing-on-multiple-ip-configurations"></a><span data-ttu-id="b7660-103">Terheléselosztás több IP-konfigurációk</span><span class="sxs-lookup"><span data-stu-id="b7660-103">Load balancing on multiple IP configurations</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="b7660-104">Portál</span><span class="sxs-lookup"><span data-stu-id="b7660-104">Portal</span></span>](load-balancer-multiple-ip.md)
> * [<span data-ttu-id="b7660-105">Parancssori felület</span><span class="sxs-lookup"><span data-stu-id="b7660-105">CLI</span></span>](load-balancer-multiple-ip-cli.md)
> * [<span data-ttu-id="b7660-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b7660-106">PowerShell</span></span>](load-balancer-multiple-ip-powershell.md)

<span data-ttu-id="b7660-107">Ez a cikk ismerteti, hogyan toouse Azure Load Balancer több IP-címek egy másodlagos hálózati adapteren (NIC).</span><span class="sxs-lookup"><span data-stu-id="b7660-107">This article describes how toouse Azure Load Balancer with multiple IP addresses on a secondary network interface (NIC).</span></span> <span data-ttu-id="b7660-108">Ebben a forgatókönyvben két virtuális gépeken futó Windows, az elsődleges és másodlagos hálózati tudunk</span><span class="sxs-lookup"><span data-stu-id="b7660-108">For this scenario, we have two VMs running Windows, each with a primary and a secondary NIC.</span></span> <span data-ttu-id="b7660-109">Egyes hello másodlagos hálózati adapterei két IP-konfigurációk.</span><span class="sxs-lookup"><span data-stu-id="b7660-109">Each of hello secondary NICs have two IP configurations.</span></span> <span data-ttu-id="b7660-110">Minden virtuális gép webhelyeket a contoso.com és fabrikam.com üzemelteti. Minden webhelyre kötött tooone az IP-konfigurációkhoz hello hello másodlagos hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="b7660-110">Each VM hosts both websites contoso.com and fabrikam.com. Each website is bound tooone of hello IP configurations on hello secondary NIC.</span></span> <span data-ttu-id="b7660-111">Azure Load Balancer tooexpose két előtérbeli IP-cím, egy a minden webhelyre, toodistribute forgalom toohello megfelelő IP-konfiguráció hello webhely használjuk.</span><span class="sxs-lookup"><span data-stu-id="b7660-111">We use Azure Load Balancer tooexpose two frontend IP addresses, one for each website, toodistribute traffic toohello respective IP configuration for hello website.</span></span> <span data-ttu-id="b7660-112">Ebben a forgatókönyvben használt hello ugyanazt a portszámot is frontends, valamint mindkét háttér címkészletet IP-címek között.</span><span class="sxs-lookup"><span data-stu-id="b7660-112">This scenario uses hello same port number across both frontends, as well as both backend pool IP addresses.</span></span>

![Terheléselosztó forgatókönyv kép](./media/load-balancer-multiple-ip/lb-multi-ip.PNG)

## <a name="steps-tooload-balance-on-multiple-ip-configurations"></a><span data-ttu-id="b7660-114">Több IP-konfigurációk lépéseket tooload egyenlege</span><span class="sxs-lookup"><span data-stu-id="b7660-114">Steps tooload balance on multiple IP configurations</span></span>

<span data-ttu-id="b7660-115">Kövesse az alábbi cikkben leírt tooachieve hello forgatókönyv hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="b7660-115">Follow hello steps below tooachieve hello scenario outlined in this article:</span></span>

1. <span data-ttu-id="b7660-116">[Telepítse és konfigurálja az Azure parancssori felület hello](../cli-install-nodejs.md) hello Azure CLI lépések hello hello csatolt cikk, majd jelentkezzen be Azure-fiókjába.</span><span class="sxs-lookup"><span data-stu-id="b7660-116">[Install and Configure hello Azure CLI](../cli-install-nodejs.md) hello Azure CLI by following hello steps in hello linked article and log into your Azure account.</span></span>
2. <span data-ttu-id="b7660-117">[Hozzon létre egy erőforráscsoportot](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-resource-group) nevű *contosofabrikam* fent leírt módon.</span><span class="sxs-lookup"><span data-stu-id="b7660-117">[Create a resource group](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-resource-group) called *contosofabrikam* as described above.</span></span>

    ```azurecli
    azure group create contosofabrikam westcentralus
    ```

3. <span data-ttu-id="b7660-118">[Egy rendelkezésre állási csoport létrehozása](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-an-availability-set) toofor hello két virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="b7660-118">[Create an availability set](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-an-availability-set) toofor hello two VMs.</span></span> <span data-ttu-id="b7660-119">Ebben az esetben használja a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="b7660-119">For this scenario, use hello following command:</span></span>

    ```azurecli
    azure availset create --resource-group contosofabrikam --location westcentralus --name myAvailabilitySet
    ```

4. <span data-ttu-id="b7660-120">[Hozzon létre egy virtuális hálózatot](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-virtual-network-and-subnet) nevű *myVNet* és alhálózat nevű *mySubnet*:</span><span class="sxs-lookup"><span data-stu-id="b7660-120">[Create a virtual network](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-virtual-network-and-subnet) called *myVNet* and a subnet called *mySubnet*:</span></span>

    ```azurecli
    azure network vnet create --resource-group contosofabrikam --name myVnet --address-prefixes 10.0.0.0/16  --location westcentralus

    azure network vnet subnet create --resource-group contosofabrikam --vnet-name myVnet --name mySubnet --address-prefix 10.0.0.0/24
    ```

5. <span data-ttu-id="b7660-121">[Hozzon létre hello terheléselosztó](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json) nevű *mylb*:</span><span class="sxs-lookup"><span data-stu-id="b7660-121">[Create hello load balancer](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json) called *mylb*:</span></span>

    ```azurecli
    azure network lb create --resource-group contosofabrikam --location westcentralus --name mylb
    ```

6. <span data-ttu-id="b7660-122">Hozzon létre két dinamikus nyilvános IP-címet hello előtérbeli IP-konfigurációkat a terheléselosztó:</span><span class="sxs-lookup"><span data-stu-id="b7660-122">Create two dynamic public IP addresses for hello frontend IP configurations of your load balancer:</span></span>

    ```azurecli
    azure network public-ip create --resource-group contosofabrikam --location westcentralus --name PublicIp1 --domain-name-label contoso --allocation-method Dynamic

    azure network public-ip create --resource-group contosofabrikam --location westcentralus --name PublicIp2 --domain-name-label fabrikam --allocation-method Dynamic
    ```

7. <span data-ttu-id="b7660-123">Hozzon létre két hello előtérbeli IP-konfigurációk *contosofe* és *fabrikamfe* osztályban:</span><span class="sxs-lookup"><span data-stu-id="b7660-123">Create hello two frontend IP configurations, *contosofe* and *fabrikamfe* respectively:</span></span>

    ```azurecli
    azure network lb frontend-ip create --resource-group contosofabrikam --lb-name mylb --public-ip-name PublicIp1 --name contosofe
    azure network lb frontend-ip create --resource-group contosofabrikam --lb-name mylb --public-ip-name PublicIp2 --name fabrkamfe
    ```

8. <span data-ttu-id="b7660-124">A háttéralkalmazás - címkészletek létrehozása *contosopool* és *fabrikampool*, egy [mintavételi](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json) - *HTTP*, és a terheléselosztási szabályok - *HTTPc* és *HTTPf*:</span><span class="sxs-lookup"><span data-stu-id="b7660-124">Create your backend address pools - *contosopool* and *fabrikampool*, a [probe](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json) - *HTTP*, and your load balancing rules - *HTTPc* and *HTTPf*:</span></span>

    ```azurecli
    azure network lb address-pool create --resource-group contosofabrikam --lb-name mylb --name contosopool
    azure network lb address-pool create --resource-group contosofabrikam --lb-name mylb --name fabrikampool

    azure network lb probe create --resource-group contosofabrikam --lb-name mylb --name HTTP --protocol "http" --interval 15 --count 2 --path index.html

    azure network lb rule create --resource-group contosofabrikam --lb-name mylb --name HTTPc --protocol tcp --probe-name http--frontend-port 5000 --backend-port 5000 --frontend-ip-name contosofe --backend-address-pool-name contosopool
    azure network lb rule create --resource-group contosofabrikam --lb-name mylb --name HTTPf --protocol tcp --probe-name http --frontend-port 5000 --backend-port 5000 --frontend-ip-name fabrkamfe --backend-address-pool-name fabrikampool
    ```

9. <span data-ttu-id="b7660-125">Futtatási hello következő alatt parancsot, és ezután ellenőrizze túl hello kimeneti[ellenőrizze a terheléselosztó](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json) megfelelően lett létrehozva:</span><span class="sxs-lookup"><span data-stu-id="b7660-125">Run hello following command below and then check hello output too[verify your load balancer](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json) was created correctly:</span></span>

    ```azurecli
    azure network lb show --resource-group contosofabrikam --name mylb
    ```

10. <span data-ttu-id="b7660-126">[Hozzon létre egy nyilvános IP-cím](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-public-ip-address), *myPublicIp*, és [tárfiók](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json), *mystorageaccont1* az első virtuális gépen VM1 alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="b7660-126">[Create a public IP](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-public-ip-address), *myPublicIp*, and [storage account](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json), *mystorageaccont1* for your first virtual machine VM1 as shown below:</span></span>

    ```azurecli
    azure network public-ip create --resource-group contosofabrikam --location westcentralus --name myPublicIP --domain-name-label mypublicdns345 --allocation-method Dynamic

    azure storage account create --location westcentralus --resource-group contosofabrikam --kind Storage --sku-name GRS mystorageaccount1
    ```

11. <span data-ttu-id="b7660-127">[Hálózati adapterek létrehozása a hello](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-virtual-nic) a VM1, és adja hozzá a második IP-konfiguráció *VM1-ipconfig2*, és [hello virtuális gép létrehozása](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-the-linux-vms) alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="b7660-127">[Create hello network interfaces](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-virtual-nic) for VM1 and add a second IP configuration, *VM1-ipconfig2*, and [create hello VM](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-the-linux-vms) as shown below:</span></span>

    ```azurecli
    azure network nic create --resource-group contosofabrikam --location westcentralus --subnet-vnet-name myVnet --subnet-name mySubnet --name VM1Nic1 --ip-config-name NIC1-ipconfig1
    azure network nic create --resource-group contosofabrikam --location westcentralus --subnet-vnet-name myVnet --subnet-name mySubnet --name VM1Nic2 --ip-config-name VM1-ipconfig1 --public-ip-name myPublicIP --lb-address-pool-ids "/subscriptions/<your subscription ID>/resourceGroups/contosofabrikam/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/contosopool"
    azure network nic ip-config create --resource-group contosofabrikam --nic-name VM1Nic2 --name VM1-ipconfig2 --lb-address-pool-ids "/subscriptions/<your subscription ID>/resourceGroups/contosofabrikam/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/fabrikampool"
    azure vm create --resource-group contosofabrikam --name VM1 --location westcentralus --os-type linux --nic-names VM1Nic1,VM1Nic2  --vnet-name VNet1 --vnet-subnet-name Subnet1 --availset-name myAvailabilitySet --vm-size Standard_DS3_v2 --storage-account-name mystorageaccount1 --image-urn canonical:UbuntuServer:16.04.0-LTS:latest --admin-username <your username>  --admin-password <your password>
    ```

12. <span data-ttu-id="b7660-128">A második virtuális gép ismételje meg a 10-11:</span><span class="sxs-lookup"><span data-stu-id="b7660-128">Repeat steps 10-11 for your second VM:</span></span>

    ```azurecli
    azure network public-ip create --resource-group contosofabrikam --location westcentralus --name myPublicIP2 --domain-name-label mypublicdns785 --allocation-method Dynamic
    azure storage account create --location westcentralus --resource-group contosofabrikam --kind Storage --sku-name GRS mystorageaccount2
    azure network nic create --resource-group contosofabrikam --location westcentralus --subnet-vnet-name myVnet --subnet-name mySubnet --name VM2Nic1
    azure network nic create --resource-group contosofabrikam --location westcentralus --subnet-vnet-name myVnet --subnet-name mySubnet --name VM2Nic2 --ip-config-name VM2-ipconfig1 --public-ip-name myPublicIP2 --lb-address-pool-ids "/subscriptions/<your subscription ID>/resourceGroups/contosofabrikam/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/contosopool"
    azure network nic ip-config create --resource-group contosofabrikam --nic-name VM2Nic2 --name VM2-ipconfig2 --lb-address-pool-ids "/subscriptions/<your subscription ID>/resourceGroups/contosofabrikam/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/fabrikampool"
    azure vm create --resource-group contosofabrikam --name VM2 --location westcentralus --os-type linux --nic-names VM2Nic1,VM2Nic2 --vnet-name VNet1 --vnet-subnet-name Subnet1 --availset-name myAvailabilitySet --vm-size Standard_DS3_v2 --storage-account-name mystorageaccount2 --image-urn canonical:UbuntuServer:16.04.0-LTS:latest --admin-username <your username>  --admin-password <your password>
    ```

13. <span data-ttu-id="b7660-129">Végül konfigurálnia kell DNS erőforrás rekordok toopoint toohello megfelelő előtérbeli IP-címe hello terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="b7660-129">Finally, you must configure DNS resource records toopoint toohello respective frontend IP address of hello Load Balancer.</span></span> <span data-ttu-id="b7660-130">A tartományok Azure DNS-ben is tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="b7660-130">You may host your domains in Azure DNS.</span></span> <span data-ttu-id="b7660-131">Az Azure DNS-sel terheléselosztással kapcsolatos további információkért lásd: [Azure DNS használata más Azure-szolgáltatásokkal](../dns/dns-for-azure-services.md).</span><span class="sxs-lookup"><span data-stu-id="b7660-131">For more information about using Azure DNS with Load Balancer, see [Using Azure DNS with other Azure services](../dns/dns-for-azure-services.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="b7660-132">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b7660-132">Next steps</span></span>
- <span data-ttu-id="b7660-133">További tudnivalók az Azure-ban hogyan toocombine terheléselosztás szolgáltatások [terheléselosztás szolgáltatások használata az Azure-ban](../traffic-manager/traffic-manager-load-balancing-azure.md).</span><span class="sxs-lookup"><span data-stu-id="b7660-133">Learn more about how toocombine load balancing services in Azure in [Using load-balancing services in Azure](../traffic-manager/traffic-manager-load-balancing-azure.md).</span></span>
- <span data-ttu-id="b7660-134">Megtudhatja, hogyan naplók különböző típusait használják az Azure toomanage és hibaelhárítása a terheléselosztó [analytics keresse meg a Azure terheléselosztó](../load-balancer/load-balancer-monitor-log.md).</span><span class="sxs-lookup"><span data-stu-id="b7660-134">Learn how you can use different types of logs in Azure toomanage and troubleshoot load balancer in [Log analytics for Azure Load Balancer](../load-balancer/load-balancer-monitor-log.md).</span></span>
