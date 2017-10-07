---
title: "aaaOpen portok tooa Linux virtuális gép az Azure CLI 2.0 |} Microsoft Docs"
description: "Megtudhatja, hogyan tooopen port / hozzon létre egy végpont tooyour Linux virtuális gép hello Azure resource manager telepítési modell és hello Azure CLI 2.0"
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
ms.openlocfilehash: c79b31206e97558171609cf033bb3cb3370777c7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="open-ports-and-endpoints-tooa-linux-vm-with-hello-azure-cli"></a><span data-ttu-id="fdfc1-103">Nyisson meg portokat és végpontot tooa Linux virtuális gép hello Azure parancssori felület</span><span class="sxs-lookup"><span data-stu-id="fdfc1-103">Open ports and endpoints tooa Linux VM with hello Azure CLI</span></span>
<span data-ttu-id="fdfc1-104">Nyissa meg a portot, vagy hozzon létre egy végpontot, tooa virtuális gép (VM) az Azure-ban egy alhálózatot vagy a virtuális gép hálózati illesztő hálózati szűrő létrehozásával.</span><span class="sxs-lookup"><span data-stu-id="fdfc1-104">You open a port, or create an endpoint, tooa virtual machine (VM) in Azure by creating a network filter on a subnet or VM network interface.</span></span> <span data-ttu-id="fdfc1-105">Ezek a szűrők, amely szabályozza a bejövő és kimenő forgalmat, hello forgalmat fogadó csatlakoztatott hálózati biztonsági csoport toohello erőforrás helyezhető el.</span><span class="sxs-lookup"><span data-stu-id="fdfc1-105">You place these filters, which control both inbound and outbound traffic, on a Network Security Group attached toohello resource that receives hello traffic.</span></span> <span data-ttu-id="fdfc1-106">Ilyenek például a webes forgalom most használja a 80-as porton.</span><span class="sxs-lookup"><span data-stu-id="fdfc1-106">Let's use a common example of web traffic on port 80.</span></span> <span data-ttu-id="fdfc1-107">Ez a cikk bemutatja, hogyan tooopen a hello Azure CLI 2.0-s port tooa virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="fdfc1-107">This article shows you how tooopen a port tooa VM with hello Azure CLI 2.0.</span></span> <span data-ttu-id="fdfc1-108">Is elvégezheti ezeket a lépéseket hello [Azure CLI 1.0](nsg-quickstart-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="fdfc1-108">You can also perform these steps with hello [Azure CLI 1.0](nsg-quickstart-nodejs.md).</span></span>


## <a name="quick-commands"></a><span data-ttu-id="fdfc1-109">Gyors parancsok</span><span class="sxs-lookup"><span data-stu-id="fdfc1-109">Quick commands</span></span>
<span data-ttu-id="fdfc1-110">toocreate egy hálózati biztonsági csoport és a szükséges szabályok hello legújabb [Azure CLI 2.0](/cli/azure/install-az-cli2) telepítve, és bejelentkezett tooan Azure-fiók használatával [az bejelentkezési](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="fdfc1-110">toocreate a Network Security Group and rules you need hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="fdfc1-111">Hello alábbi példák, cserélje le például paraméterek nevei a saját értékeit.</span><span class="sxs-lookup"><span data-stu-id="fdfc1-111">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="fdfc1-112">Példa paraméter nevek a következők *myResourceGroup*, *myNetworkSecurityGroup*, és *myVnet*.</span><span class="sxs-lookup"><span data-stu-id="fdfc1-112">Example parameter names include *myResourceGroup*, *myNetworkSecurityGroup*, and *myVnet*.</span></span>

<span data-ttu-id="fdfc1-113">Hello hálózati biztonsági csoport létrehozása [az hálózati nsg létrehozása](/cli/azure/network/nsg#create).</span><span class="sxs-lookup"><span data-stu-id="fdfc1-113">Create hello network security group with [az network nsg create](/cli/azure/network/nsg#create).</span></span> <span data-ttu-id="fdfc1-114">hello alábbi példakód létrehozza a hálózati biztonsági csoport nevű *myNetworkSecurityGroup* a hello *eastus* helye:</span><span class="sxs-lookup"><span data-stu-id="fdfc1-114">hello following example creates a network security group named *myNetworkSecurityGroup* in hello *eastus* location:</span></span>

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNetworkSecurityGroup
```

<span data-ttu-id="fdfc1-115">Vegye fel a szabályt [az hálózati nsg-szabály létrehozása](/cli/azure/network/nsg/rule#create) tooallow HTTP tooyour webkiszolgáló traffic (vagy a saját forgatókönyvben például SSH hozzáférés vagy az adatbázis-kapcsolat beállítása).</span><span class="sxs-lookup"><span data-stu-id="fdfc1-115">Add a rule with [az network nsg rule create](/cli/azure/network/nsg/rule#create) tooallow HTTP traffic tooyour webserver (or adjust for your own scenario, such as SSH access or database connectivity).</span></span> <span data-ttu-id="fdfc1-116">hello alábbi példa létrehoz egy nevű szabályt *myNetworkSecurityGroupRule* tooallow TCP-forgalom 80-as porton:</span><span class="sxs-lookup"><span data-stu-id="fdfc1-116">hello following example creates a rule named *myNetworkSecurityGroupRule* tooallow TCP traffic on port 80:</span></span>

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRule \
    --protocol tcp \
    --priority 1000 \
    --destination-port-range 80
```

<span data-ttu-id="fdfc1-117">A virtuális gép hálózati illesztőt (NIC) a hello hálózati biztonsági csoporthoz társítandó [az hálózati nic frissítés](/cli/azure/network/nic#update).</span><span class="sxs-lookup"><span data-stu-id="fdfc1-117">Associate hello Network Security Group with your VM's network interface (NIC) with [az network nic update](/cli/azure/network/nic#update).</span></span> <span data-ttu-id="fdfc1-118">hello alábbi példa társít egy olyan meglévő hálózati adapter nevű *myNic* a hálózati biztonsági csoport nevű hello *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="fdfc1-118">hello following example associates an existing NIC named *myNic* with hello Network Security Group named *myNetworkSecurityGroup*:</span></span>

```azurecli
az network nic update \
    --resource-group myResourceGroup \
    --name myNic \
    --network-security-group myNetworkSecurityGroup
```

<span data-ttu-id="fdfc1-119">Azt is megteheti, hogy társíthasson a hálózati biztonsági csoport virtuális hálózati alhálózat [az hálózati vnet alhálózati frissítés](/cli/azure/network/vnet/subnet#update) ahelyett, hogy egy virtuális csak toohello hálózati adapternek.</span><span class="sxs-lookup"><span data-stu-id="fdfc1-119">Alternatively, you can associate your Network Security Group with a virtual network subnet with [az network vnet subnet update](/cli/azure/network/vnet/subnet#update) rather than just toohello network interface on a single VM.</span></span> <span data-ttu-id="fdfc1-120">hello alábbi példa hozzárendeli egy létező alhálózatot nevű *mySubnet* a hello *myVnet* hello nevű hálózati biztonsági csoportot a virtuális hálózati *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="fdfc1-120">hello following example associates an existing subnet named *mySubnet* in hello *myVnet* virtual network with hello Network Security Group named *myNetworkSecurityGroup*:</span></span>

```azurecli
az network vnet subnet update \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnet \
    --network-security-group myNetworkSecurityGroup
```

## <a name="more-information-on-network-security-groups"></a><span data-ttu-id="fdfc1-121">További információ a hálózati biztonsági csoportok</span><span class="sxs-lookup"><span data-stu-id="fdfc1-121">More information on Network Security Groups</span></span>
<span data-ttu-id="fdfc1-122">hello itt gyors parancsok lehetővé teszik tooget be és a forgalom szereplő tooyour virtuális gép futtatása.</span><span class="sxs-lookup"><span data-stu-id="fdfc1-122">hello quick commands here allow you tooget up and running with traffic flowing tooyour VM.</span></span> <span data-ttu-id="fdfc1-123">Hálózati biztonsági csoportok adja meg, hány különleges szolgáltatásait és részletességgel ellenőrző tooyour erőforrások eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="fdfc1-123">Network Security Groups provide many great features and granularity for controlling access tooyour resources.</span></span> <span data-ttu-id="fdfc1-124">További tudnivalók [itt szabályok létrehozása a hálózati biztonsági csoport és a hozzáférés-vezérlési lista](tutorial-virtual-network.md#secure-network-traffic).</span><span class="sxs-lookup"><span data-stu-id="fdfc1-124">You can read more about [creating a Network Security Group and ACL rules here](tutorial-virtual-network.md#secure-network-traffic).</span></span>

<span data-ttu-id="fdfc1-125">Magas rendelkezésre állású webes alkalmazásokhoz helyezze a virtuális gépek az Azure terheléselosztó mögött.</span><span class="sxs-lookup"><span data-stu-id="fdfc1-125">For highly available web applications, you should place your VMs behind an Azure Load Balancer.</span></span> <span data-ttu-id="fdfc1-126">hello terheléselosztó osztja el a forgalmat tooVMs forgalomszűrést végez biztosító hálózati biztonsági csoport.</span><span class="sxs-lookup"><span data-stu-id="fdfc1-126">hello load balancer distributes traffic tooVMs, with a Network Security Group that provides traffic filtering.</span></span> <span data-ttu-id="fdfc1-127">További információkért lásd: [hogyan tooload egyenleg Linux virtuális gépek az Azure toocreate a magas rendelkezésre állású alkalmazások](tutorial-load-balancer.md).</span><span class="sxs-lookup"><span data-stu-id="fdfc1-127">For more information, see [How tooload balance Linux virtual machines in Azure toocreate a highly available application](tutorial-load-balancer.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="fdfc1-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fdfc1-128">Next steps</span></span>
<span data-ttu-id="fdfc1-129">Ebben a példában létrehozott egy egyszerű szabályt tooallow HTTP-forgalmat.</span><span class="sxs-lookup"><span data-stu-id="fdfc1-129">In this example, you created a simple rule tooallow HTTP traffic.</span></span> <span data-ttu-id="fdfc1-130">További részletes környezetek létrehozásáról a következő cikkek hello információt talál:</span><span class="sxs-lookup"><span data-stu-id="fdfc1-130">You can find information on creating more detailed environments in hello following articles:</span></span>

* [<span data-ttu-id="fdfc1-131">Az Azure Resource Manager áttekintése</span><span class="sxs-lookup"><span data-stu-id="fdfc1-131">Azure Resource Manager overview</span></span>](../../azure-resource-manager/resource-group-overview.md)
* [<span data-ttu-id="fdfc1-132">Mi az a hálózati biztonsági csoport (NSG)?</span><span class="sxs-lookup"><span data-stu-id="fdfc1-132">What is a Network Security Group (NSG)?</span></span>](../../virtual-network/virtual-networks-nsg.md)
