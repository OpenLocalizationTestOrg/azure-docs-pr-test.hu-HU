---
title: "Nyisson meg portokat a Linux virtuális gép Azure CLI 2.0 |} Microsoft Docs"
description: "Nyisson meg egy portot / hozzon létre egy végpontot a Linux virtuális gép az Azure resource manager üzembe helyezési modellben és az Azure CLI 2.0 útmutató"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: eef9842b-495a-46cf-99a6-74e49807e74e
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 08/21/2017
ms.author: iainfou
ms.openlocfilehash: d176187fe465264b5f433260de5178b48ca9dd4a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="open-ports-and-endpoints-to-a-linux-vm-with-the-azure-cli"></a><span data-ttu-id="cb946-103">Nyisson meg portokat és a Linux virtuális gép végpontokat az Azure parancssori felület</span><span class="sxs-lookup"><span data-stu-id="cb946-103">Open ports and endpoints to a Linux VM with the Azure CLI</span></span>
<span data-ttu-id="cb946-104">Nyissa meg a portot, vagy hozzon létre egy végpontot a virtuális gép (VM), az Azure-ban egy alhálózatot vagy a virtuális gép hálózati illesztő hálózati szűrő létrehozásával.</span><span class="sxs-lookup"><span data-stu-id="cb946-104">You open a port, or create an endpoint, to a virtual machine (VM) in Azure by creating a network filter on a subnet or VM network interface.</span></span> <span data-ttu-id="cb946-105">Ezek a szűrők, amely szabályozza a bejövő és kimenő forgalmat, a hálózati biztonsági csoport az erőforrás a forgalmat fogadó csatolva helyezze el.</span><span class="sxs-lookup"><span data-stu-id="cb946-105">You place these filters, which control both inbound and outbound traffic, on a Network Security Group attached to the resource that receives the traffic.</span></span> <span data-ttu-id="cb946-106">Ilyenek például a webes forgalom most használja a 80-as porton.</span><span class="sxs-lookup"><span data-stu-id="cb946-106">Let's use a common example of web traffic on port 80.</span></span> <span data-ttu-id="cb946-107">Ez a cikk bemutatja, hogyan nyisson meg egy portot a virtuális gépre az Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="cb946-107">This article shows you how to open a port to a VM with the Azure CLI 2.0.</span></span> <span data-ttu-id="cb946-108">Az [Azure CLI 1.0-s](nsg-quickstart-nodejs.md) verziójával is elvégezheti ezeket a lépéseket.</span><span class="sxs-lookup"><span data-stu-id="cb946-108">You can also perform these steps with the [Azure CLI 1.0](nsg-quickstart-nodejs.md).</span></span>


## <a name="quick-commands"></a><span data-ttu-id="cb946-109">Gyors parancsok</span><span class="sxs-lookup"><span data-stu-id="cb946-109">Quick commands</span></span>
<span data-ttu-id="cb946-110">Hálózati biztonsági csoport és a legújabb szükséges szabályok létrehozásához [Azure CLI 2.0](/cli/azure/install-az-cli2) telepítve, és bejelentkezett az Azure-fiók használatával [az bejelentkezési](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="cb946-110">To create a Network Security Group and rules you need the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in to an Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="cb946-111">A következő példákban cserélje le a saját értékeit példa paraméterek nevei.</span><span class="sxs-lookup"><span data-stu-id="cb946-111">In the following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="cb946-112">Példa paraméter nevek a következők *myResourceGroup*, *myNetworkSecurityGroup*, és *myVnet*.</span><span class="sxs-lookup"><span data-stu-id="cb946-112">Example parameter names include *myResourceGroup*, *myNetworkSecurityGroup*, and *myVnet*.</span></span>

<span data-ttu-id="cb946-113">A hálózati biztonsági csoport létrehozása [az hálózati nsg létrehozása](/cli/azure/network/nsg#create).</span><span class="sxs-lookup"><span data-stu-id="cb946-113">Create the network security group with [az network nsg create](/cli/azure/network/nsg#create).</span></span> <span data-ttu-id="cb946-114">Az alábbi példakód létrehozza a hálózati biztonsági csoport nevű *myNetworkSecurityGroup* a a *eastus* helye:</span><span class="sxs-lookup"><span data-stu-id="cb946-114">The following example creates a network security group named *myNetworkSecurityGroup* in the *eastus* location:</span></span>

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNetworkSecurityGroup
```

<span data-ttu-id="cb946-115">Vegye fel a szabályt [az hálózati nsg-szabály létrehozása](/cli/azure/network/nsg/rule#create) engedélyezi a HTTP-forgalom számára a webkiszolgáló (vagy a saját forgatókönyvben például SSH hozzáférés vagy az adatbázis-kapcsolat beállítása).</span><span class="sxs-lookup"><span data-stu-id="cb946-115">Add a rule with [az network nsg rule create](/cli/azure/network/nsg/rule#create) to allow HTTP traffic to your webserver (or adjust for your own scenario, such as SSH access or database connectivity).</span></span> <span data-ttu-id="cb946-116">Az alábbi példa létrehoz egy nevű szabályt *myNetworkSecurityGroupRule* a TCP-forgalmat engedélyezi a 80-as port:</span><span class="sxs-lookup"><span data-stu-id="cb946-116">The following example creates a rule named *myNetworkSecurityGroupRule* to allow TCP traffic on port 80:</span></span>

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRule \
    --protocol tcp \
    --priority 1000 \
    --destination-port-range 80
```

<span data-ttu-id="cb946-117">A hálózati biztonsági csoport társítani a virtuális gép hálózati illesztőt (NIC) a [az hálózati nic frissítés](/cli/azure/network/nic#update).</span><span class="sxs-lookup"><span data-stu-id="cb946-117">Associate the Network Security Group with your VM's network interface (NIC) with [az network nic update](/cli/azure/network/nic#update).</span></span> <span data-ttu-id="cb946-118">A következő példa egy olyan meglévő hálózati adapter nevű társítja *myNic* együtt a hálózati biztonsági csoport nevű *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="cb946-118">The following example associates an existing NIC named *myNic* with the Network Security Group named *myNetworkSecurityGroup*:</span></span>

```azurecli
az network nic update \
    --resource-group myResourceGroup \
    --name myNic \
    --network-security-group myNetworkSecurityGroup
```

<span data-ttu-id="cb946-119">Azt is megteheti, hogy társíthasson a hálózati biztonsági csoport virtuális hálózati alhálózat [az hálózati vnet alhálózati frissítés](/cli/azure/network/vnet/subnet#update) helyett csak egy virtuális hálózati adapteréhez.</span><span class="sxs-lookup"><span data-stu-id="cb946-119">Alternatively, you can associate your Network Security Group with a virtual network subnet with [az network vnet subnet update](/cli/azure/network/vnet/subnet#update) rather than just to the network interface on a single VM.</span></span> <span data-ttu-id="cb946-120">A következő példa egy létező alhálózatot nevű társítja *mySubnet* a a *myVnet* a hálózati biztonsági csoport nevű virtuális hálózat *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="cb946-120">The following example associates an existing subnet named *mySubnet* in the *myVnet* virtual network with the Network Security Group named *myNetworkSecurityGroup*:</span></span>

```azurecli
az network vnet subnet update \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnet \
    --network-security-group myNetworkSecurityGroup
```

## <a name="more-information-on-network-security-groups"></a><span data-ttu-id="cb946-121">További információ a hálózati biztonsági csoportok</span><span class="sxs-lookup"><span data-stu-id="cb946-121">More information on Network Security Groups</span></span>
<span data-ttu-id="cb946-122">A gyors parancsok lehetővé teszik, amelyekből megismerheti a forgalom halad a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="cb946-122">The quick commands here allow you to get up and running with traffic flowing to your VM.</span></span> <span data-ttu-id="cb946-123">Hálózati biztonsági csoportok számos különleges szolgáltatásait és az erőforrásokhoz való hozzáférés szabályozása részletességgel adja meg.</span><span class="sxs-lookup"><span data-stu-id="cb946-123">Network Security Groups provide many great features and granularity for controlling access to your resources.</span></span> <span data-ttu-id="cb946-124">További tudnivalók [itt szabályok létrehozása a hálózati biztonsági csoport és a hozzáférés-vezérlési lista](tutorial-virtual-network.md#secure-network-traffic).</span><span class="sxs-lookup"><span data-stu-id="cb946-124">You can read more about [creating a Network Security Group and ACL rules here](tutorial-virtual-network.md#secure-network-traffic).</span></span>

<span data-ttu-id="cb946-125">Magas rendelkezésre állású webes alkalmazásokhoz helyezze a virtuális gépek az Azure terheléselosztó mögött.</span><span class="sxs-lookup"><span data-stu-id="cb946-125">For highly available web applications, you should place your VMs behind an Azure Load Balancer.</span></span> <span data-ttu-id="cb946-126">A load balancer osztja el a forgalmat a virtuális gépekhez, a hálózati biztonsági csoport, amely biztosítja a forgalomszűrést végez.</span><span class="sxs-lookup"><span data-stu-id="cb946-126">The load balancer distributes traffic to VMs, with a Network Security Group that provides traffic filtering.</span></span> <span data-ttu-id="cb946-127">További információkért lásd: [betöltése Linux virtuális gépek magas rendelkezésre állású alkalmazás létrehozása az Azure-ban egyenleg](tutorial-load-balancer.md).</span><span class="sxs-lookup"><span data-stu-id="cb946-127">For more information, see [How to load balance Linux virtual machines in Azure to create a highly available application](tutorial-load-balancer.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="cb946-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cb946-128">Next steps</span></span>
<span data-ttu-id="cb946-129">Ebben a példában létrehozott egy egyszerű szabályt, amely engedélyezi a HTTP-forgalmat.</span><span class="sxs-lookup"><span data-stu-id="cb946-129">In this example, you created a simple rule to allow HTTP traffic.</span></span> <span data-ttu-id="cb946-130">További részletes környezetek létrehozásáról a következő cikkekben találhat:</span><span class="sxs-lookup"><span data-stu-id="cb946-130">You can find information on creating more detailed environments in the following articles:</span></span>

* [<span data-ttu-id="cb946-131">Az Azure Resource Manager áttekintése</span><span class="sxs-lookup"><span data-stu-id="cb946-131">Azure Resource Manager overview</span></span>](../../azure-resource-manager/resource-group-overview.md)
* [<span data-ttu-id="cb946-132">Mi az a hálózati biztonsági csoport (NSG)?</span><span class="sxs-lookup"><span data-stu-id="cb946-132">What is a Network Security Group (NSG)?</span></span>](../../virtual-network/virtual-networks-nsg.md)
