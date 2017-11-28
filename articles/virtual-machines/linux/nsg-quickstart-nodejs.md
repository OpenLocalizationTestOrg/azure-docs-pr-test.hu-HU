---
title: "aaaOpen portok tooa Linux virtuális gép az Azure CLI 1.0 |} Microsoft Docs"
description: "Megtudhatja, hogyan tooopen port / hozzon létre egy végpont tooyour Linux virtuális gép hello Azure resource manager telepítési modell és hello Azure CLI 1.0"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 337c37d151f527b43d4852291159b2f70a00accc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="opening-ports-and-endpoints-tooa-linux-vm-in-azure-using-hello-azure-cli-10"></a><span data-ttu-id="ea7fe-103">Nyitó portokat és végpontot tooa Linux virtuális gép az Azure használatával hello Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="ea7fe-103">Opening ports and endpoints tooa Linux VM in Azure using hello Azure CLI 1.0</span></span>
<span data-ttu-id="ea7fe-104">Nyissa meg a portot, vagy hozzon létre egy végpontot, tooa virtuális gép (VM) az Azure-ban egy alhálózatot vagy a virtuális gép hálózati illesztő hálózati szűrő létrehozásával.</span><span class="sxs-lookup"><span data-stu-id="ea7fe-104">You open a port, or create an endpoint, tooa virtual machine (VM) in Azure by creating a network filter on a subnet or VM network interface.</span></span> <span data-ttu-id="ea7fe-105">Ezek a szűrők, amely szabályozza a bejövő és kimenő forgalmat, hello forgalmat fogadó csatlakoztatott hálózati biztonsági csoport toohello erőforrás helyezhető el.</span><span class="sxs-lookup"><span data-stu-id="ea7fe-105">You place these filters, which control both inbound and outbound traffic, on a Network Security Group attached toohello resource that receives hello traffic.</span></span> <span data-ttu-id="ea7fe-106">Ilyenek például a webes forgalom most használja a 80-as porton.</span><span class="sxs-lookup"><span data-stu-id="ea7fe-106">Let's use a common example of web traffic on port 80.</span></span> <span data-ttu-id="ea7fe-107">Ez a cikk bemutatja, hogyan port tooa VM using tooopen hello Azure CLI 1.0.</span><span class="sxs-lookup"><span data-stu-id="ea7fe-107">This article shows you how tooopen a port tooa VM using hello Azure CLI 1.0.</span></span>


## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="ea7fe-108">Parancssori felület verziók toocomplete hello feladat</span><span class="sxs-lookup"><span data-stu-id="ea7fe-108">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="ea7fe-109">Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:</span><span class="sxs-lookup"><span data-stu-id="ea7fe-109">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="ea7fe-110">[Az Azure CLI 1.0](#quick-commands) – a parancssori felületen hello klasszikus és resource management üzembe helyezési modellel (a cikk)</span><span class="sxs-lookup"><span data-stu-id="ea7fe-110">[Azure CLI 1.0](#quick-commands) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="ea7fe-111">[Az Azure CLI 2.0](nsg-quickstart.md) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell</span><span class="sxs-lookup"><span data-stu-id="ea7fe-111">[Azure CLI 2.0](nsg-quickstart.md) - our next generation CLI for hello resource management deployment model</span></span>


## <a name="quick-commands"></a><span data-ttu-id="ea7fe-112">Gyors parancsok</span><span class="sxs-lookup"><span data-stu-id="ea7fe-112">Quick commands</span></span>
<span data-ttu-id="ea7fe-113">Hálózati biztonsági csoport toocreate és a szükséges szabályok [hello Azure CLI 1.0](../../cli-install-nodejs.md) telepített és a Resource Manager módra:</span><span class="sxs-lookup"><span data-stu-id="ea7fe-113">toocreate a Network Security Group and rules you need [hello Azure CLI 1.0](../../cli-install-nodejs.md) installed and using Resource Manager mode:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="ea7fe-114">Hello alábbi példák, cserélje le például paraméterek nevei a saját értékeit.</span><span class="sxs-lookup"><span data-stu-id="ea7fe-114">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="ea7fe-115">Példa paraméter nevekre *myResourceGroup*, *myNetworkSecurityGroup*, és *myVnet*.</span><span class="sxs-lookup"><span data-stu-id="ea7fe-115">Example parameter names included *myResourceGroup*, *myNetworkSecurityGroup*, and *myVnet*.</span></span>

<span data-ttu-id="ea7fe-116">Hozzon létre a hálózati biztonsági csoport, írja be megfelelően a saját nevét és helyét.</span><span class="sxs-lookup"><span data-stu-id="ea7fe-116">Create your Network Security Group, entering your own names and location appropriately.</span></span> <span data-ttu-id="ea7fe-117">hello alábbi példakód létrehozza a hálózati biztonsági csoport nevű *myNetworkSecurityGroup* a hello *eastus* helye:</span><span class="sxs-lookup"><span data-stu-id="ea7fe-117">hello following example creates a Network Security Group named *myNetworkSecurityGroup* in hello *eastus* location:</span></span>

```azurecli
azure network nsg create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNetworkSecurityGroup
```

<span data-ttu-id="ea7fe-118">A szabály tooallow HTTP-forgalom tooyour webkiszolgáló hozzáadása (vagy a saját forgatókönyvben például SSH hozzáférés vagy az adatbázis-kapcsolat beállítása).</span><span class="sxs-lookup"><span data-stu-id="ea7fe-118">Add a rule tooallow HTTP traffic tooyour webserver (or adjust for your own scenario, such as SSH access or database connectivity).</span></span> <span data-ttu-id="ea7fe-119">hello alábbi példa létrehoz egy nevű szabályt *myNetworkSecurityGroupRule* tooallow TCP-forgalom 80-as porton:</span><span class="sxs-lookup"><span data-stu-id="ea7fe-119">hello following example creates a rule named *myNetworkSecurityGroupRule* tooallow TCP traffic on port 80:</span></span>

```azurecli
azure network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRule \
    --protocol tcp \
    --direction inbound \
    --priority 1000 \
    --destination-port-range 80 \
    --access allow
```

<span data-ttu-id="ea7fe-120">Hálózati biztonsági csoport hello társítani a virtuális gép hálózati illesztőt (NIC).</span><span class="sxs-lookup"><span data-stu-id="ea7fe-120">Associate hello Network Security Group with your VM's network interface (NIC).</span></span> <span data-ttu-id="ea7fe-121">hello alábbi példa társít egy olyan meglévő hálózati adapter nevű *myNic* a hálózati biztonsági csoport nevű hello *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="ea7fe-121">hello following example associates an existing NIC named *myNic* with hello Network Security Group named *myNetworkSecurityGroup*:</span></span>

```azurecli
azure network nic set \
    --resource-group myResourceGroup \
    --network-security-group-name myNetworkSecurityGroup \
    --name myNic
```

<span data-ttu-id="ea7fe-122">Másik lehetőségként is társíthat a hálózati biztonsági csoport egy virtuális csak toohello hálózati adapternek helyett egy virtuális hálózati alhálózat.</span><span class="sxs-lookup"><span data-stu-id="ea7fe-122">Alternatively, you can associate your Network Security Group with a virtual network subnet rather than just toohello network interface on a single VM.</span></span> <span data-ttu-id="ea7fe-123">hello alábbi példa hozzárendeli egy létező alhálózatot nevű *mySubnet* a hello *myVnet* hello nevű hálózati biztonsági csoportot a virtuális hálózati *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="ea7fe-123">hello following example associates an existing subnet named *mySubnet* in hello *myVnet* virtual network with hello Network Security Group named *myNetworkSecurityGroup*:</span></span>

```azurecli
azure network vnet subnet set \
    --resource-group myResourceGroup \
    --network-security-group-name myNetworkSecurityGroup \
    --vnet-name myVnet --name mySubnet
```

## <a name="more-information-on-network-security-groups"></a><span data-ttu-id="ea7fe-124">További információ a hálózati biztonsági csoportok</span><span class="sxs-lookup"><span data-stu-id="ea7fe-124">More information on Network Security Groups</span></span>
<span data-ttu-id="ea7fe-125">hello itt gyors parancsok lehetővé teszik tooget be és a forgalom szereplő tooyour virtuális gép futtatása.</span><span class="sxs-lookup"><span data-stu-id="ea7fe-125">hello quick commands here allow you tooget up and running with traffic flowing tooyour VM.</span></span> <span data-ttu-id="ea7fe-126">Hálózati biztonsági csoportok adja meg, hány különleges szolgáltatásait és részletességgel ellenőrző tooyour erőforrások eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="ea7fe-126">Network Security Groups provide many great features and granularity for controlling access tooyour resources.</span></span> <span data-ttu-id="ea7fe-127">További tudnivalók [itt szabályok létrehozása a hálózati biztonsági csoport és a hozzáférés-vezérlési lista](../../virtual-network/virtual-networks-create-nsg-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="ea7fe-127">You can read more about [creating a Network Security Group and ACL rules here](../../virtual-network/virtual-networks-create-nsg-arm-cli.md).</span></span>

<span data-ttu-id="ea7fe-128">Hálózati biztonsági csoportok és ACL-szabályok Azure Resource Manager sablonokban definiálhat.</span><span class="sxs-lookup"><span data-stu-id="ea7fe-128">You can define Network Security Groups and ACL rules as part of Azure Resource Manager templates.</span></span> <span data-ttu-id="ea7fe-129">Tudjon meg többet az [hálózati biztonsági csoportok létrehozása a sablonok](../../virtual-network/virtual-networks-create-nsg-arm-template.md).</span><span class="sxs-lookup"><span data-stu-id="ea7fe-129">Read more about [creating Network Security Groups with templates](../../virtual-network/virtual-networks-create-nsg-arm-template.md).</span></span>

<span data-ttu-id="ea7fe-130">Ha a virtuális gépen kell toouse port-továbbítási toomap egyedi külső port tooan belső port, használja a terheléselosztó és a hálózati címfordítás (NAT) szabályok.</span><span class="sxs-lookup"><span data-stu-id="ea7fe-130">If you need toouse port-forwarding toomap a unique external port tooan internal port on your VM, use a load balancer and Network Address Translation (NAT) rules.</span></span> <span data-ttu-id="ea7fe-131">Például előfordulhat, hogy szeretné, hogy tooexpose TCP 8080-as porton külsőleg és irányított forgalom tooTCP 80-as portot a virtuális gép rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="ea7fe-131">For example, you may want tooexpose TCP port 8080 externally and have traffic directed tooTCP port 80 on a VM.</span></span> <span data-ttu-id="ea7fe-132">Többet is megtudhat [egy internetre irányuló terheléselosztói](../../load-balancer/load-balancer-get-started-internet-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="ea7fe-132">You can learn about [creating an Internet-facing load balancer](../../load-balancer/load-balancer-get-started-internet-arm-cli.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="ea7fe-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ea7fe-133">Next steps</span></span>
<span data-ttu-id="ea7fe-134">Ebben a példában létrehozott egy egyszerű szabályt tooallow HTTP-forgalmat.</span><span class="sxs-lookup"><span data-stu-id="ea7fe-134">In this example, you created a simple rule tooallow HTTP traffic.</span></span> <span data-ttu-id="ea7fe-135">További részletes környezetek létrehozásáról a következő cikkek hello információt talál:</span><span class="sxs-lookup"><span data-stu-id="ea7fe-135">You can find information on creating more detailed environments in hello following articles:</span></span>

* [<span data-ttu-id="ea7fe-136">Az Azure Resource Manager áttekintése</span><span class="sxs-lookup"><span data-stu-id="ea7fe-136">Azure Resource Manager overview</span></span>](../../azure-resource-manager/resource-group-overview.md)
* [<span data-ttu-id="ea7fe-137">Mi az a hálózati biztonsági csoport (NSG)?</span><span class="sxs-lookup"><span data-stu-id="ea7fe-137">What is a Network Security Group (NSG)?</span></span>](../../virtual-network/virtual-networks-nsg.md)
* [<span data-ttu-id="ea7fe-138">Terheléselosztók Azure Resource Manager áttekintése</span><span class="sxs-lookup"><span data-stu-id="ea7fe-138">Azure Resource Manager Overview for Load Balancers</span></span>](../../load-balancer/load-balancer-arm.md)

