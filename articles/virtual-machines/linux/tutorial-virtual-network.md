---
title: "Az Azure virtuális hálózatok és a Linux virtuális gépek |} Microsoft Docs"
description: "Az oktatóanyag - kezelése az Azure virtuális hálózatok és a Linux virtuális gépek az Azure parancssori felület"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/10/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 2366905b8160675f77cbc41ba97540af70be8c01
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="manage-azure-virtual-networks-and-linux-virtual-machines-with-the-azure-cli"></a><span data-ttu-id="1d8c3-103">Egy Azure virtuális hálózatot és a Linux virtuális gépek az Azure parancssori felület kezelése</span><span class="sxs-lookup"><span data-stu-id="1d8c3-103">Manage Azure Virtual Networks and Linux Virtual Machines with the Azure CLI</span></span>

<span data-ttu-id="1d8c3-104">Az Azure virtuális gépek Azure hálózatkezelés belső és külső hálózati kommunikációhoz használni.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-104">Azure virtual machines use Azure networking for internal and external network communication.</span></span> <span data-ttu-id="1d8c3-105">Ez az oktatóanyag végigvezeti a két virtuális gép telepítése és konfigurálása ezen a virtuális gépek Azure hálózatkezelés.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-105">This tutorial walks through deploying two virtual machines and configuring Azure networking for these VMs.</span></span> <span data-ttu-id="1d8c3-106">Ebben az oktatóanyagban szereplő példák feltételezik, hogy, hogy a virtuális gépeket üzemeltető egy adatbázis-háttér webalkalmazás azonban egy alkalmazás nincs telepítve az oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-106">The examples in this tutorial assume that the VMs are hosting a web application with a database back-end, however an application is not deployed in the tutorial.</span></span> <span data-ttu-id="1d8c3-107">Eben az oktatóanyagban az alábbiakkal fog megismerkedni:</span><span class="sxs-lookup"><span data-stu-id="1d8c3-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1d8c3-108">Telepíthet egy virtuális hálózatot</span><span class="sxs-lookup"><span data-stu-id="1d8c3-108">Deploy a virtual network</span></span>
> * <span data-ttu-id="1d8c3-109">Hozzon létre egy alhálózatot a virtuális hálózaton belül</span><span class="sxs-lookup"><span data-stu-id="1d8c3-109">Create a subnet within a virtual network</span></span>
> * <span data-ttu-id="1d8c3-110">Csatlakoztassa a virtuális gépek alhálózathoz</span><span class="sxs-lookup"><span data-stu-id="1d8c3-110">Attach virtual machines to a subnet</span></span>
> * <span data-ttu-id="1d8c3-111">Virtuális gép nyilvános IP-címeinek kezelése</span><span class="sxs-lookup"><span data-stu-id="1d8c3-111">Manage virtual machine public IP addresses</span></span>
> * <span data-ttu-id="1d8c3-112">Bejövő internet forgalmának biztonságossá tétele</span><span class="sxs-lookup"><span data-stu-id="1d8c3-112">Secure incoming internet traffic</span></span>
> * <span data-ttu-id="1d8c3-113">Virtuális gép biztonságos a virtuális gépek forgalma</span><span class="sxs-lookup"><span data-stu-id="1d8c3-113">Secure VM to VM traffic</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="1d8c3-114">Telepítése és a parancssori felület helyileg használata mellett dönt, ha ez az oktatóanyag van szükség, hogy futnak-e az Azure parancssori felület 2.0.4 verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-114">If you choose to install and use the CLI locally, this tutorial requires that you are running the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="1d8c3-115">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-115">Run `az --version` to find the version.</span></span> <span data-ttu-id="1d8c3-116">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="1d8c3-116">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="vm-networking-overview"></a><span data-ttu-id="1d8c3-117">VM-hálózat – áttekintés</span><span class="sxs-lookup"><span data-stu-id="1d8c3-117">VM networking overview</span></span>

<span data-ttu-id="1d8c3-118">Egy Azure virtuális hálózatot a virtuális gépek, az internetes és más Azure-szolgáltatások például az Azure SQL-adatbázis között a biztonságos hálózati kapcsolatok engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-118">Azure virtual networks enable secure network connections between virtual machines, the internet, and other Azure services such as Azure SQL database.</span></span> <span data-ttu-id="1d8c3-119">Virtuális hálózatok logikai szegmensekben – ún alhálózatok osztható.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-119">Virtual networks are broken down into logical segments called subnets.</span></span> <span data-ttu-id="1d8c3-120">Alhálózatok folyamatábrán hálózati és biztonsági határaként szolgálnak.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-120">Subnets are used to control network flow, and as a security boundary.</span></span> <span data-ttu-id="1d8c3-121">A virtuális gép telepítésekor általában tartalmazza a virtuális hálózati adaptert, alhálózat van csatolva.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-121">When deploying a VM, it generally includes a virtual network interface, which is attached to a subnet.</span></span>

## <a name="deploy-virtual-network"></a><span data-ttu-id="1d8c3-122">Virtuális hálózati telepítése</span><span class="sxs-lookup"><span data-stu-id="1d8c3-122">Deploy virtual network</span></span>

<span data-ttu-id="1d8c3-123">Ebben az oktatóanyagban egy virtuális hálózaton, két alhálózattal jön létre.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-123">For this tutorial, a single virtual network is created with two subnets.</span></span> <span data-ttu-id="1d8c3-124">A webalkalmazás üzemeltetéséhez előtér-alhálózatot, és egy adatbázis-kiszolgálót futtató háttér-alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-124">A front-end subnet for hosting a web application, and a back-end subnet for hosting a database server.</span></span>

<span data-ttu-id="1d8c3-125">Virtuális hálózat létrehozása előtt hozzon létre egy erőforráscsoportot, a [az csoport létrehozása](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="1d8c3-125">Before you can create a virtual network, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="1d8c3-126">Az alábbi példa létrehoz egy erőforráscsoportot *myRGNetwork* eastus a helyen.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-126">The following example creates a resource group named *myRGNetwork* in the eastus location.</span></span>

```azurecli-interactive 
az group create --name myRGNetwork --location eastus
```

### <a name="create-virtual-network"></a><span data-ttu-id="1d8c3-127">Virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="1d8c3-127">Create virtual network</span></span>

<span data-ttu-id="1d8c3-128">Nekünk a [az hálózati vnet létrehozása](/cli/azure/network/vnet#create) parancs futtatásával hozzon létre egy virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-128">Us the [az network vnet create](/cli/azure/network/vnet#create) command to create a virtual network.</span></span> <span data-ttu-id="1d8c3-129">Ebben a példában a hálózati neve *mvVnet* , és egy címelőtagot *10.0.0.0/16*.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-129">In this example, the network is named *mvVnet* and is given an address prefix of *10.0.0.0/16*.</span></span> <span data-ttu-id="1d8c3-130">Egy alhálózat is létrejön, melynek a neve *mySubnetFrontEnd* és a előtaggal *10.0.1.0/24*.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-130">A subnet is also created with a name of *mySubnetFrontEnd* and a prefix of *10.0.1.0/24*.</span></span> <span data-ttu-id="1d8c3-131">Az oktatóanyag későbbi részében egy előtér-VM a alhálózathoz csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-131">Later in this tutorial a front-end VM is connected to this subnet.</span></span> 

```azurecli-interactive 
az network vnet create \
  --resource-group myRGNetwork \
  --name myVnet \
  --address-prefix 10.0.0.0/16 \
  --subnet-name mySubnetFrontEnd \
  --subnet-prefix 10.0.1.0/24
```

### <a name="create-subnet"></a><span data-ttu-id="1d8c3-132">Hozzon létre az alhálózatot</span><span class="sxs-lookup"><span data-stu-id="1d8c3-132">Create subnet</span></span>

<span data-ttu-id="1d8c3-133">Egy új alhálózatot hozzá lett adva a virtuális hálózat a [az alhálózaton virtuális hálózat létrehozása](/cli/azure/network/vnet/subnet#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-133">A new subnet is added to the virtual network using the [az network vnet subnet create](/cli/azure/network/vnet/subnet#create) command.</span></span> <span data-ttu-id="1d8c3-134">Ebben a példában az alhálózat neve *mySubnetBackEnd* , és egy címelőtagot *10.0.2.0/24*.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-134">In this example, the subnet is named *mySubnetBackEnd* and is given an address prefix of *10.0.2.0/24*.</span></span> <span data-ttu-id="1d8c3-135">Ez az alhálózat összes háttérszolgáltatások használatos.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-135">This subnet is used with all back-end services.</span></span>

```azurecli-interactive 
az network vnet subnet create \
  --resource-group myRGNetwork \
  --vnet-name myVnet \
  --name mySubnetBackEnd \
  --address-prefix 10.0.2.0/24
```

<span data-ttu-id="1d8c3-136">Ezen a ponton a hálózati létrehozott és szegmentált két alhálózat, egy az előtér-szolgáltatásokat, és egy másikat a háttér-szolgáltatásaihoz.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-136">At this point, a network has been created and segmented into two subnets, one for front-end services, and another for back-end services.</span></span> <span data-ttu-id="1d8c3-137">A következő szakaszban virtuális gépek létrehozása és ezek alhálózatok csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-137">In the next section, virtual machines are created and connected to these subnets.</span></span>

## <a name="understand-public-ip-address"></a><span data-ttu-id="1d8c3-138">Nyilvános IP-cím ismertetése</span><span class="sxs-lookup"><span data-stu-id="1d8c3-138">Understand public IP address</span></span>

<span data-ttu-id="1d8c3-139">A nyilvános IP-cím lehetővé teszi, hogy az Azure-erőforrások elérhetővé az interneten.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-139">A public IP address allows Azure resources to be accessible on the internet.</span></span> <span data-ttu-id="1d8c3-140">Ebben a szakaszban az oktatóanyag egy virtuális gép létrehozása bemutatásához nyilvános IP-címek használata.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-140">In this section of the tutorial, a VM is created to demonstrate how to work with public IP addresses.</span></span>

### <a name="allocation-method"></a><span data-ttu-id="1d8c3-141">Lefoglalási módszer</span><span class="sxs-lookup"><span data-stu-id="1d8c3-141">Allocation method</span></span>

<span data-ttu-id="1d8c3-142">Egy nyilvános IP-címet, dinamikus vagy statikus oszthat ki.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-142">A public IP address can be allocated as either dynamic or static.</span></span> <span data-ttu-id="1d8c3-143">Alapértelmezés szerint dinamikusan lefoglalt nyilvános IP-cím.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-143">By default, public IP address dynamically allocated.</span></span> <span data-ttu-id="1d8c3-144">Dinamikus IP-címek van kiadva, ha egy virtuális gép felszabadítása.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-144">Dynamic IP addresses are released when a VM is deallocated.</span></span> <span data-ttu-id="1d8c3-145">Ennek következtében az IP-cím módosítása bármely, amely tartalmazza a virtuális gép felszabadítás művelet során.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-145">This behavior causes the IP address to change during any operation that includes a VM deallocation.</span></span>

<span data-ttu-id="1d8c3-146">A kiosztási módszer állítható be a statikus, amely biztosítja, hogy az IP cím maradnak hozzárendelve a virtuális gép, egy megszüntetett állapot esetén.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-146">The allocation method can be set to static, which ensures that the IP address remain assigned to a VM, even during a deallocated state.</span></span> <span data-ttu-id="1d8c3-147">A statikusan lefoglalt IP-cím használata esetén nem adható meg az IP-címet saját magát.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-147">When using a statically allocated IP address, the IP address itself cannot be specified.</span></span> <span data-ttu-id="1d8c3-148">Ehelyett az történik a rendelkezésre álló címek készletét.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-148">Instead, it is allocated from a pool of available addresses.</span></span>

### <a name="dynamic-allocation"></a><span data-ttu-id="1d8c3-149">Dinamikus lefoglalását</span><span class="sxs-lookup"><span data-stu-id="1d8c3-149">Dynamic allocation</span></span>

<span data-ttu-id="1d8c3-150">A virtuális gép létrehozásakor a [az virtuális gép létrehozása](/cli/azure/vm#create) parancs, az alapértelmezett nyilvános IP-cím címkiosztási módszerét dinamikus.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-150">When creating a VM with the [az vm create](/cli/azure/vm#create) command, the default public IP address allocation method is dynamic.</span></span> <span data-ttu-id="1d8c3-151">A következő példában egy virtuális gép létrehozása egy dinamikus IP-címmel.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-151">In the following example, a VM is created with a dynamic IP address.</span></span> 

```azurecli-interactive 
az vm create \
  --resource-group myRGNetwork \
  --name myFrontEndVM \
  --vnet-name myVnet \
  --subnet mySubnetFrontEnd \
  --nsg myNSGFrontEnd \
  --public-ip-address myFrontEndIP \
  --image UbuntuLTS \
  --generate-ssh-keys
```

### <a name="static-allocation"></a><span data-ttu-id="1d8c3-152">Statikus foglalási</span><span class="sxs-lookup"><span data-stu-id="1d8c3-152">Static allocation</span></span>

<span data-ttu-id="1d8c3-153">Használatával egy virtuális gép létrehozásakor a [az virtuális gép létrehozása](/cli/azure/vm#create) paranccsal, tartalmazza a `--public-ip-address-allocation static` argumentum egy statikus nyilvános IP-címet hozzárendelni.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-153">When creating a virtual machine using the [az vm create](/cli/azure/vm#create) command, include the `--public-ip-address-allocation static` argument to assign a static public IP address.</span></span> <span data-ttu-id="1d8c3-154">Ez a művelet nem mutatja be ebben az oktatóanyagban azonban a következő szakaszban a dinamikusan kiosztott IP-címet egy statikusan lefoglalt címet változik.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-154">This operation is not demonstrated in this tutorial, however in the next section a dynamically allocated IP address is changed to a statically allocated address.</span></span> 

### <a name="change-allocation-method"></a><span data-ttu-id="1d8c3-155">Elosztási módszer váltása</span><span class="sxs-lookup"><span data-stu-id="1d8c3-155">Change allocation method</span></span>

<span data-ttu-id="1d8c3-156">Az IP-cím címkiosztási módszerét használatával módosíthatók a [az hálózati nyilvános ip-frissítés](/cli/azure/network/public-ip#update) parancsot.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-156">The IP address allocation method can be changed using the [az network public-ip update](/cli/azure/network/public-ip#update) command.</span></span> <span data-ttu-id="1d8c3-157">Ebben a példában az IP-cím címkiosztási módszerét az előtér virtuális gép statikus értékre változott.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-157">In this example, the IP address allocation method of the front-end VM is changed to static.</span></span>

<span data-ttu-id="1d8c3-158">Első lépésként felszabadítani a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-158">First, deallocate the VM.</span></span>

```azurecli-interactive 
az vm deallocate --resource-group myRGNetwork --name myFrontEndVM
```

<span data-ttu-id="1d8c3-159">Használja a [az hálózati nyilvános ip-frissítés](/cli/azure/network/public-ip#update) parancs foglalási módjának frissítésére.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-159">Use the [az network public-ip update](/cli/azure/network/public-ip#update) command to update the allocation method.</span></span> <span data-ttu-id="1d8c3-160">Ebben az esetben a `--allocation-method` állítják be *statikus*.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-160">In this case, the `--allocation-method` is being set to *static*.</span></span>

```azurecli-interactive 
az network public-ip update --resource-group myRGNetwork --name myFrontEndIP --allocation-method static
```

<span data-ttu-id="1d8c3-161">Indítsa el a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-161">Start the VM.</span></span>

```azurecli-interactive 
az vm start --resource-group myRGNetwork --name myFrontEndVM --no-wait
```

### <a name="no-public-ip-address"></a><span data-ttu-id="1d8c3-162">Nyilvános IP-cím</span><span class="sxs-lookup"><span data-stu-id="1d8c3-162">No public IP address</span></span>

<span data-ttu-id="1d8c3-163">Gyakran egy virtuális Gépet nem kell elérhetőnek kell lennie az interneten keresztül.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-163">Often, a VM does not need to be accessible over the internet.</span></span> <span data-ttu-id="1d8c3-164">Egy virtuális gép, egy nyilvános IP-cím nélküli létrehozásához használja a `--public-ip-address ""` dupla idézőjelek között egy üres számú argumentum.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-164">To create a VM without a public IP address, use the `--public-ip-address ""` argument with an empty set of double quotes.</span></span> <span data-ttu-id="1d8c3-165">Ez a konfiguráció az oktatóanyag későbbi részében bemutatott</span><span class="sxs-lookup"><span data-stu-id="1d8c3-165">This configuration is demonstrated later in this tutorial</span></span>

## <a name="secure-network-traffic"></a><span data-ttu-id="1d8c3-166">Biztonságos hálózati adatforgalom</span><span class="sxs-lookup"><span data-stu-id="1d8c3-166">Secure network traffic</span></span>

<span data-ttu-id="1d8c3-167">A hálózati biztonsági csoport (NSG) egy biztonsági szabályokból álló listát tartalmaz, amelyek engedélyezik vagy megtagadják a hálózati forgalmat az Azure-alapú virtuális hálózatokhoz (VNet-ekhez) csatlakozó erőforrásoknak.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-167">A network security group (NSG) contains a list of security rules that allow or deny network traffic to resources connected to Azure Virtual Networks (VNet).</span></span> <span data-ttu-id="1d8c3-168">Lehet, hogy az NSG-k társított alhálózat vagy az egyes hálózati adapterek.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-168">NSGs can be associated to subnets or individual network interfaces.</span></span> <span data-ttu-id="1d8c3-169">Amikor egy NSG-t egy adott hálózati csatoló kapcsolódik, csak a kapcsolódó virtuális gép vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-169">When an NSG is associated with a network interface, it applies only the associated VM.</span></span> <span data-ttu-id="1d8c3-170">Ha az NSG-t hozzárendelik egy alhálózathoz, a szabályok érvényesek lesznek az alhálózathoz csatlakozó összes erőforrásra.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-170">When an NSG is associated to a subnet, the rules apply to all resources connected to the subnet.</span></span> 

### <a name="network-security-group-rules"></a><span data-ttu-id="1d8c3-171">Hálózati biztonsági csoportszabályok</span><span class="sxs-lookup"><span data-stu-id="1d8c3-171">Network security group rules</span></span>

<span data-ttu-id="1d8c3-172">NSG-szabályok határozza meg, amelyben forgalom engedélyezett vagy megtagadott hálózati portok.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-172">NSG rules define networking ports over which traffic is allowed or denied.</span></span> <span data-ttu-id="1d8c3-173">A szabályok lehetnek forrás és cél IP-címtartományok, hogy az egyes rendszerek vagy az alhálózatok közötti forgalmat szabályozott.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-173">The rules can include source and destination IP address ranges so that traffic is controlled between specific systems or subnets.</span></span> <span data-ttu-id="1d8c3-174">NSG-szabályok terjednie a prioritással (1 – és 4096).</span><span class="sxs-lookup"><span data-stu-id="1d8c3-174">NSG rules also include a priority (between 1—and 4096).</span></span> <span data-ttu-id="1d8c3-175">Szabályok prioritási sorrendben értékeli ki a rendszer.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-175">Rules are evaluated in the order of priority.</span></span> <span data-ttu-id="1d8c3-176">A 100 prioritással kiértékeli előtt szabály 200 prioritással.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-176">A rule with a priority of 100 is evaluated before a rule with priority 200.</span></span>

<span data-ttu-id="1d8c3-177">Minden NSG tartalmaz egy alapértelmezett szabálykészletet.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-177">All NSGs contain a set of default rules.</span></span> <span data-ttu-id="1d8c3-178">Az alapértelmezett szabályokat nem lehet törölni, de mivel a legalacsonyabb prioritást rendelték hozzájuk, a létrehozott szabályok felülbírálhatják azokat.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-178">The default rules cannot be deleted, but because they are assigned the lowest priority, they can be overridden by the rules that you create.</span></span>

- <span data-ttu-id="1d8c3-179">**Virtuális hálózati** - származó forgalmat, és a befejezési egy virtuális hálózat bejövő és kimenő irányban is.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-179">**Virtual network** - Traffic originating and ending in a virtual network is allowed both in inbound and outbound directions.</span></span>
- <span data-ttu-id="1d8c3-180">**Internet** – a kimenő forgalom engedélyezve van, de a bejövő forgalmat blokkol.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-180">**Internet** - Outbound traffic is allowed, but inbound traffic is blocked.</span></span>
- <span data-ttu-id="1d8c3-181">**Terheléselosztó** -engedélyezése Azure terheléselosztó számára, hogy megvizsgálja a virtuális gépek és a szerepkörpéldányok állapotát.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-181">**Load balancer** - Allow Azure’s load balancer to probe the health of your VMs and role instances.</span></span> <span data-ttu-id="1d8c3-182">Ha nem használ elosztott terhelésű készlet, ez a szabály lehet felülbírálni.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-182">If you are not using a load balanced set, you can override this rule.</span></span>

### <a name="create-network-security-groups"></a><span data-ttu-id="1d8c3-183">Hálózati biztonsági csoportok létrehozása</span><span class="sxs-lookup"><span data-stu-id="1d8c3-183">Create network security groups</span></span>

<span data-ttu-id="1d8c3-184">Hálózati biztonsági csoport is létrehozható az azonos időpontban egy virtuális gép használata a [az virtuális gép létrehozása](/cli/azure/vm#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-184">A network security group can be created at the same time as a VM using the [az vm create](/cli/azure/vm#create) command.</span></span> <span data-ttu-id="1d8c3-185">Ennek során, amikor az NSG tartozik a virtuális gépek hálózati illesztő és az NSG-szabályok automatikusan létrehozott forgalmat engedélyezi a port *22* forrásból.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-185">When doing so, the NSG is associated with the VMs network interface and an NSG rule is auto created to allow traffic on port *22* from any source.</span></span> <span data-ttu-id="1d8c3-186">Ez az oktatóanyag során korábban küldje el az előtér-NSG lett automatikus létrehozása a az előtér virtuális Géphez.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-186">Earlier in this tutorial, the front-end NSG was auto-created with the front-end VM.</span></span> <span data-ttu-id="1d8c3-187">Az NSG-szabályok is 22-es port számára létrehozott automatikus volt.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-187">An NSG rule was also auto created for port 22.</span></span> 

<span data-ttu-id="1d8c3-188">Bizonyos esetekben hasznos lehet egy NSG-t, például amikor alapértelmezett SSH szabályokat nem lehet létrehozni, vagy amikor az NSG-t kell csatlakoztatni egy alhálózat előre létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-188">In some cases, it may be helpful to pre-create an NSG, such as when default SSH rules should not be created, or when the NSG should be attached to a subnet.</span></span> 

<span data-ttu-id="1d8c3-189">Használja a [az hálózati nsg létrehozása](/cli/azure/network/nsg#create) parancsot a hálózati biztonsági csoport létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-189">Use the [az network nsg create](/cli/azure/network/nsg#create) command to create a network security group.</span></span>

```azurecli-interactive 
az network nsg create --resource-group myRGNetwork --name myNSGBackEnd
```

<span data-ttu-id="1d8c3-190">Helyett a hálózati illesztő NSG társítása, hozzárendelnek egy alhálózathoz.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-190">Instead of associating the NSG to a network interface, it is associated with a subnet.</span></span> <span data-ttu-id="1d8c3-191">Ebben a konfigurációban a virtuális gép alhálózathoz csatolt örökli az NSG-szabályok.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-191">In this configuration, any VM that is attached to the subnet inherits the NSG rules.</span></span>

<span data-ttu-id="1d8c3-192">Frissítse a meglévő nevű alhálózat *mySubnetBackEnd* rendelkező új NSG.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-192">Update the existing subnet named *mySubnetBackEnd* with the new NSG.</span></span>

```azurecli-interactive 
az network vnet subnet update \
  --resource-group myRGNetwork \
  --vnet-name myVnet \
  --name mySubnetBackEnd \
  --network-security-group myNSGBackEnd
```

<span data-ttu-id="1d8c3-193">Most hozzon létre egy virtuális gép, amely csatolva van a *mySubnetBackEnd*.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-193">Now create a virtual machine, which is attached to the *mySubnetBackEnd*.</span></span> <span data-ttu-id="1d8c3-194">Figyelje meg, hogy a `--nsg` argumentum értéke üres dupla idézőjelek között.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-194">Notice that the `--nsg` argument has a value of empty double quotes.</span></span> <span data-ttu-id="1d8c3-195">Az NSG nem kell létrehozni a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-195">An NSG does not need to be created with the VM.</span></span> <span data-ttu-id="1d8c3-196">A háttér-alhálózathoz, az előre létrehozott háttér-NSG védi a virtuális Géphez van csatolva.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-196">The VM is attached to the back-end subnet, which is protected with the pre-created back-end NSG.</span></span> <span data-ttu-id="1d8c3-197">Az NSG-t a virtuális gép vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-197">This NSG applies to the VM.</span></span> <span data-ttu-id="1d8c3-198">Emellett az információkban, amely a `--public-ip-address` argumentum értéke üres dupla idézőjelek között.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-198">Also, notice here that the `--public-ip-address` argument has a value of empty double quotes.</span></span> <span data-ttu-id="1d8c3-199">Ez a konfiguráció nélkül egy nyilvános IP-cím egy virtuális Gépet hoz létre.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-199">This configuration creates a VM without a public IP address.</span></span> 

```azurecli-interactive 
az vm create \
  --resource-group myRGNetwork \
  --name myBackEndVM \
  --vnet-name myVnet \
  --subnet mySubnetBackEnd \
  --public-ip-address "" \
  --nsg "" \
  --image UbuntuLTS \
  --generate-ssh-keys
```

### <a name="secure-incoming-traffic"></a><span data-ttu-id="1d8c3-200">Biztonságos bejövő forgalom</span><span class="sxs-lookup"><span data-stu-id="1d8c3-200">Secure incoming traffic</span></span>

<span data-ttu-id="1d8c3-201">Az előtér virtuális gép létrehozása után az NSG-szabály létrehozási 22-es portot a bejövő adatforgalom engedélyezésére.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-201">When the front-end VM was created, an NSG rule was created to allow incoming traffic on port 22.</span></span> <span data-ttu-id="1d8c3-202">Ez a szabály lehetővé teszi, hogy a virtuális gép SSH-kapcsolatok.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-202">This rule allows SSH connections to the VM.</span></span> <span data-ttu-id="1d8c3-203">Ebben a példában a forgalmat is engedélyezni kell a porton *80*.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-203">For this example, traffic should also be allowed on port *80*.</span></span> <span data-ttu-id="1d8c3-204">Ez a konfiguráció lehetővé teszi, hogy egy webes alkalmazás érhető el a virtuális Gépen.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-204">This configuration allows a web application to be accessed on the VM.</span></span>

<span data-ttu-id="1d8c3-205">Használja a [az hálózati nsg-szabály létrehozása](/cli/azure/network/nsg/rule#create) parancs futtatásával hozzon létre egy szabályt port *80*.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-205">Use the [az network nsg rule create](/cli/azure/network/nsg/rule#create) command to create a rule for port *80*.</span></span>

```azurecli-interactive 
az network nsg rule create \
  --resource-group myRGNetwork \
  --nsg-name myNSGFrontEnd \
  --name http \
  --access allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 200 \
  --source-address-prefix "*" \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range 80
```

<span data-ttu-id="1d8c3-206">Az előtér virtuális gép jelenleg csak elérhető porton *22* és port *80*.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-206">The front-end VM is now only accessible on port *22* and port *80*.</span></span> <span data-ttu-id="1d8c3-207">A hálózati biztonsági csoport összes egyéb bejövő forgalom blokkolva van.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-207">All other incoming traffic is blocked at the network security group.</span></span> <span data-ttu-id="1d8c3-208">Az NSG-szabályok konfigurációi megjelenítéséhez hasznos lehet.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-208">It may be helpful to visualize the NSG rule configurations.</span></span> <span data-ttu-id="1d8c3-209">Térjen vissza a az NSG-konfiguráció a [az hálózati szabálylistában](/cli/azure/network/nsg/rule#list) parancsot.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-209">Return the NSG rule configuration with the [az network rule list](/cli/azure/network/nsg/rule#list) command.</span></span> 

```azurecli-interactive 
az network nsg rule list --resource-group myRGNetwork --nsg-name myNSGFrontEnd --output table
```

<span data-ttu-id="1d8c3-210">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="1d8c3-210">Output:</span></span>

```azurecli-interactive 
Access    DestinationAddressPrefix      DestinationPortRange  Direction    Name                 Priority  Protocol    ProvisioningState    ResourceGroup    SourceAddressPrefix    SourcePortRange
--------  --------------------------  ----------------------  -----------  -----------------  ----------  ----------  -------------------  ---------------  ---------------------  -----------------
Allow     *                                               22  Inbound      default-allow-ssh        1000  Tcp         Succeeded            myRGNetwork      *                      *
Allow     *                                               80  Inbound      http                      200  Tcp         Succeeded            myRGNetwork      *                      *
```

### <a name="secure-vm-to-vm-traffic"></a><span data-ttu-id="1d8c3-211">Virtuális gép biztonságos a virtuális gépek forgalma</span><span class="sxs-lookup"><span data-stu-id="1d8c3-211">Secure VM to VM traffic</span></span>

<span data-ttu-id="1d8c3-212">Virtuális gépek közötti hálózati biztonsági csoportszabályok is alkalmazhat.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-212">Network security group rules can also apply between VMs.</span></span> <span data-ttu-id="1d8c3-213">Az ebben a példában az előtér-VM a háttér-VM porton kommunikálnia kell *22* és *3306*.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-213">For this example, the front-end VM needs to communicate with the back-end VM on port *22* and *3306*.</span></span> <span data-ttu-id="1d8c3-214">Ez a konfiguráció lehetővé teszi, hogy az SSH-kapcsolatok az előtér virtuális gépről, és is lehetővé teszi egy alkalmazás az előtér virtuális gépen egy háttér-beli MySQL database kommunikálni.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-214">This configuration allows SSH connections from the front-end VM, and also allow an application on the front-end VM to communicate with a back-end MySQL database.</span></span> <span data-ttu-id="1d8c3-215">Az összes többi forgalom le kell tiltani az előtér- és virtuális gépek között.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-215">All other traffic should be blocked between the front-end and back-end virtual machines.</span></span>

<span data-ttu-id="1d8c3-216">Használja a [az hálózati nsg-szabály létrehozása](/cli/azure/network/nsg/rule#create) parancs futtatásával hozzon létre egy szabályt a 22-es portot.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-216">Use the [az network nsg rule create](/cli/azure/network/nsg/rule#create) command to create a rule for port 22.</span></span> <span data-ttu-id="1d8c3-217">Figyelje meg, hogy a `--source-address-prefix` argumentum meghatározza a érték *10.0.1.0/24*.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-217">Notice that the `--source-address-prefix` argument specifies a value of *10.0.1.0/24*.</span></span> <span data-ttu-id="1d8c3-218">Ez a konfiguráció biztosítja, hogy az csak az előtér-alhálózatból forgalom engedélyezve van-e az NSG keresztül.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-218">This configuration ensures that only traffic from the front-end subnet is allowed through the NSG.</span></span>

```azurecli-interactive 
az network nsg rule create \
  --resource-group myRGNetwork \
  --nsg-name myNSGBackEnd \
  --name SSH \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 100 \
  --source-address-prefix 10.0.1.0/24 \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range "22"
```

<span data-ttu-id="1d8c3-219">Most adja hozzá egy forgalomra vonatkozó szabály MySQL 3306 porton.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-219">Now add a rule for MySQL traffic on port 3306.</span></span>

```azurecli-interactive 
az network nsg rule create \
  --resource-group myRGNetwork \
  --nsg-name myNSGBackEnd \
  --name MySQL \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 200 \
  --source-address-prefix 10.0.1.0/24 \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range "3306"
```

<span data-ttu-id="1d8c3-220">Végül mivel az NSG-k egy alapértelmezett szabály, amely lehetővé teszi az ugyanazon virtuális gépek közötti összes forgalom, egy szabály hozható létre a háttér-NSG-ket minden forgalom blokkolása.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-220">Finally, because NSGs have a default rule allowing all traffic between VMs in the same VNet, a rule can be created for the back-end NSGs to block all traffic.</span></span> <span data-ttu-id="1d8c3-221">Figyelje meg, itt, amely a `--priority` értéket kap *300*, amely alacsonyabb, hogy az NSG-t és a MySQL szabályok.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-221">Notice here that the `--priority` is given a value of *300*, which is lower that both the NSG and MySQL rules.</span></span> <span data-ttu-id="1d8c3-222">Ez a konfiguráció biztosítja, hogy SSH és a MySQL-forgalom továbbra is engedélyezett az NSG keresztül.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-222">This configuration ensures that SSH and MySQL traffic is still allowed through the NSG.</span></span>

```azurecli-interactive 
az network nsg rule create \
  --resource-group myRGNetwork \
  --nsg-name myNSGBackEnd \
  --name denyAll \
  --access Deny \
  --protocol Tcp \
  --direction Inbound \
  --priority 300 \
  --source-address-prefix "*" \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range "*"
```

<span data-ttu-id="1d8c3-223">A háttér-virtuális gép jelenleg csak elérhető porton *22* és port *3306* az előtér-alhálózatból.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-223">The back-end VM is now only accessible on port *22* and port *3306* from the front-end subnet.</span></span> <span data-ttu-id="1d8c3-224">A hálózati biztonsági csoport összes egyéb bejövő forgalom blokkolva van.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-224">All other incoming traffic is blocked at the network security group.</span></span> <span data-ttu-id="1d8c3-225">Az NSG-szabályok konfigurációi megjelenítéséhez hasznos lehet.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-225">It may be helpful to visualize the NSG rule configurations.</span></span> <span data-ttu-id="1d8c3-226">Térjen vissza a az NSG-konfiguráció a [az hálózati szabálylistában](/cli/azure/network/nsg/rule#list) parancsot.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-226">Return the NSG rule configuration with the [az network rule list](/cli/azure/network/nsg/rule#list) command.</span></span> 

```azurecli-interactive 
az network nsg rule list --resource-group myRGNetwork --nsg-name myNSGBackEnd --output table
```

<span data-ttu-id="1d8c3-227">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="1d8c3-227">Output:</span></span>

```azurecli-interactive 
Access    DestinationAddressPrefix    DestinationPortRange    Direction    Name       Priority  Protocol    ProvisioningState    ResourceGroup    SourceAddressPrefix    SourcePortRange
--------  --------------------------  ----------------------  -----------  -------  ----------  ----------  -------------------  ---------------  ---------------------  -----------------
Allow     *                           22                      Inbound      SSH             100  Tcp         Succeeded            myRGNetwork      10.0.1.0/24            *
Allow     *                           3306                    Inbound      MySQL           200  Tcp         Succeeded            myRGNetwork      10.0.1.0/24            *
Deny      *                           *                       Inbound      denyAll         300  Tcp         Succeeded            myRGNetwork      *                      *
```

## <a name="next-steps"></a><span data-ttu-id="1d8c3-228">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1d8c3-228">Next steps</span></span>

<span data-ttu-id="1d8c3-229">Ebben az oktatóanyagban létre és a védett virtuális gépek kapcsolatos Azure-hálózatok.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-229">In this tutorial, you created and secured Azure networks as related to virtual machines.</span></span> <span data-ttu-id="1d8c3-230">Megtudta, hogyan, hogy:</span><span class="sxs-lookup"><span data-stu-id="1d8c3-230">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1d8c3-231">Telepíthet egy virtuális hálózatot</span><span class="sxs-lookup"><span data-stu-id="1d8c3-231">Deploy a virtual network</span></span>
> * <span data-ttu-id="1d8c3-232">Hozzon létre egy alhálózatot a virtuális hálózaton belül</span><span class="sxs-lookup"><span data-stu-id="1d8c3-232">Create a subnet within a virtual network</span></span>
> * <span data-ttu-id="1d8c3-233">Csatlakoztassa a virtuális gépek alhálózathoz</span><span class="sxs-lookup"><span data-stu-id="1d8c3-233">Attach virtual machines to a subnet</span></span>
> * <span data-ttu-id="1d8c3-234">Virtuális gép nyilvános IP-címeinek kezelése</span><span class="sxs-lookup"><span data-stu-id="1d8c3-234">Manage virtual machine public IP addresses</span></span>
> * <span data-ttu-id="1d8c3-235">Bejövő internet forgalmának biztonságossá tétele</span><span class="sxs-lookup"><span data-stu-id="1d8c3-235">Secure incoming internet traffic</span></span>
> * <span data-ttu-id="1d8c3-236">Virtuális gép biztonságos a virtuális gépek forgalma</span><span class="sxs-lookup"><span data-stu-id="1d8c3-236">Secure VM to VM traffic</span></span>

<span data-ttu-id="1d8c3-237">Előzetes tájékozódhat az adatvédelem az Azure backup segítségével virtuális gépeket a következő oktatóanyagot.</span><span class="sxs-lookup"><span data-stu-id="1d8c3-237">Advance to the next tutorial to learn about securing data on virtual machines using Azure backup.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="1d8c3-238">Készítsen biztonsági másolatot a Linux virtuális gépek Azure-ban</span><span class="sxs-lookup"><span data-stu-id="1d8c3-238">Back up Linux virtual machines in Azure</span></span>](./tutorial-backup-vms.md)
