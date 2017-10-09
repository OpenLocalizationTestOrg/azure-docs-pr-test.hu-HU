---
title: "aaaAzure virtuális hálózatok és a Linux virtuális gépek |} Microsoft Docs"
description: "Útmutató – hello Azure CLI Azure virtuális hálózatok és a Linux virtuális gépek kezelése"
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
ms.openlocfilehash: 57e6bd4de16f0e31d53dc67bf50dc5730d43712b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-virtual-networks-and-linux-virtual-machines-with-hello-azure-cli"></a><span data-ttu-id="55c7e-103">Azure virtuális hálózatok és a Linux virtuális gépek kezelése hello Azure parancssori felület</span><span class="sxs-lookup"><span data-stu-id="55c7e-103">Manage Azure Virtual Networks and Linux Virtual Machines with hello Azure CLI</span></span>

<span data-ttu-id="55c7e-104">Az Azure virtuális gépek Azure hálózatkezelés belső és külső hálózati kommunikációhoz használni.</span><span class="sxs-lookup"><span data-stu-id="55c7e-104">Azure virtual machines use Azure networking for internal and external network communication.</span></span> <span data-ttu-id="55c7e-105">Ez az oktatóanyag végigvezeti a két virtuális gép telepítése és konfigurálása ezen a virtuális gépek Azure hálózatkezelés.</span><span class="sxs-lookup"><span data-stu-id="55c7e-105">This tutorial walks through deploying two virtual machines and configuring Azure networking for these VMs.</span></span> <span data-ttu-id="55c7e-106">Ebben az oktatóanyagban hello példák feltételezik, hogy, hogy hello virtuális gépeken üzemeltet egy adatbázis-háttér webalkalmazás azonban egy alkalmazás nem lett telepítve a hello oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="55c7e-106">hello examples in this tutorial assume that hello VMs are hosting a web application with a database back-end, however an application is not deployed in hello tutorial.</span></span> <span data-ttu-id="55c7e-107">Eben az oktatóanyagban az alábbiakkal fog megismerkedni:</span><span class="sxs-lookup"><span data-stu-id="55c7e-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="55c7e-108">Telepíthet egy virtuális hálózatot</span><span class="sxs-lookup"><span data-stu-id="55c7e-108">Deploy a virtual network</span></span>
> * <span data-ttu-id="55c7e-109">Hozzon létre egy alhálózatot a virtuális hálózaton belül</span><span class="sxs-lookup"><span data-stu-id="55c7e-109">Create a subnet within a virtual network</span></span>
> * <span data-ttu-id="55c7e-110">Virtuális gépek tooa alhálózati csatolása</span><span class="sxs-lookup"><span data-stu-id="55c7e-110">Attach virtual machines tooa subnet</span></span>
> * <span data-ttu-id="55c7e-111">Virtuális gép nyilvános IP-címeinek kezelése</span><span class="sxs-lookup"><span data-stu-id="55c7e-111">Manage virtual machine public IP addresses</span></span>
> * <span data-ttu-id="55c7e-112">Bejövő internet forgalmának biztonságossá tétele</span><span class="sxs-lookup"><span data-stu-id="55c7e-112">Secure incoming internet traffic</span></span>
> * <span data-ttu-id="55c7e-113">Virtuális gép tooVM forgalmának biztonságossá tétele</span><span class="sxs-lookup"><span data-stu-id="55c7e-113">Secure VM tooVM traffic</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="55c7e-114">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ez az oktatóanyag van szükség, hogy verzióját hello Azure CLI 2.0.4 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="55c7e-114">If you choose tooinstall and use hello CLI locally, this tutorial requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="55c7e-115">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="55c7e-115">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="55c7e-116">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="55c7e-116">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="vm-networking-overview"></a><span data-ttu-id="55c7e-117">VM-hálózat – áttekintés</span><span class="sxs-lookup"><span data-stu-id="55c7e-117">VM networking overview</span></span>

<span data-ttu-id="55c7e-118">Egy Azure virtuális hálózatot a virtuális gépek között a biztonságos hálózati kapcsolatok engedélyezéséhez hello internetes és az egyéb Azure-szolgáltatások például az Azure SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="55c7e-118">Azure virtual networks enable secure network connections between virtual machines, hello internet, and other Azure services such as Azure SQL database.</span></span> <span data-ttu-id="55c7e-119">Virtuális hálózatok logikai szegmensekben – ún alhálózatok osztható.</span><span class="sxs-lookup"><span data-stu-id="55c7e-119">Virtual networks are broken down into logical segments called subnets.</span></span> <span data-ttu-id="55c7e-120">Alhálózatok használt toocontrol hálózati folyamata, és egy biztonsági határként.</span><span class="sxs-lookup"><span data-stu-id="55c7e-120">Subnets are used toocontrol network flow, and as a security boundary.</span></span> <span data-ttu-id="55c7e-121">A virtuális gép telepítésekor a virtuális hálózati adaptert, amely csatolt tooa alhálózati általában tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="55c7e-121">When deploying a VM, it generally includes a virtual network interface, which is attached tooa subnet.</span></span>

## <a name="deploy-virtual-network"></a><span data-ttu-id="55c7e-122">Virtuális hálózati telepítése</span><span class="sxs-lookup"><span data-stu-id="55c7e-122">Deploy virtual network</span></span>

<span data-ttu-id="55c7e-123">Ebben az oktatóanyagban egy virtuális hálózaton, két alhálózattal jön létre.</span><span class="sxs-lookup"><span data-stu-id="55c7e-123">For this tutorial, a single virtual network is created with two subnets.</span></span> <span data-ttu-id="55c7e-124">A webalkalmazás üzemeltetéséhez előtér-alhálózatot, és egy adatbázis-kiszolgálót futtató háttér-alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="55c7e-124">A front-end subnet for hosting a web application, and a back-end subnet for hosting a database server.</span></span>

<span data-ttu-id="55c7e-125">Virtuális hálózat létrehozása előtt hozzon létre egy erőforráscsoportot, a [az csoport létrehozása](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="55c7e-125">Before you can create a virtual network, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="55c7e-126">hello alábbi példa létrehoz egy erőforráscsoportot *myRGNetwork* hello eastus helyen.</span><span class="sxs-lookup"><span data-stu-id="55c7e-126">hello following example creates a resource group named *myRGNetwork* in hello eastus location.</span></span>

```azurecli-interactive 
az group create --name myRGNetwork --location eastus
```

### <a name="create-virtual-network"></a><span data-ttu-id="55c7e-127">Virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="55c7e-127">Create virtual network</span></span>

<span data-ttu-id="55c7e-128">Velünk hello [az hálózati vnet létrehozása](/cli/azure/network/vnet#create) parancs toocreate egy virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="55c7e-128">Us hello [az network vnet create](/cli/azure/network/vnet#create) command toocreate a virtual network.</span></span> <span data-ttu-id="55c7e-129">Ebben a példában hello hálózati nevű *mvVnet* , és egy címelőtagot *10.0.0.0/16*.</span><span class="sxs-lookup"><span data-stu-id="55c7e-129">In this example, hello network is named *mvVnet* and is given an address prefix of *10.0.0.0/16*.</span></span> <span data-ttu-id="55c7e-130">Egy alhálózat is létrejön, melynek a neve *mySubnetFrontEnd* és a előtaggal *10.0.1.0/24*.</span><span class="sxs-lookup"><span data-stu-id="55c7e-130">A subnet is also created with a name of *mySubnetFrontEnd* and a prefix of *10.0.1.0/24*.</span></span> <span data-ttu-id="55c7e-131">Az oktatóanyag későbbi részében egy előtér-virtuális Géphez csatlakoztatott toothis alhálózat.</span><span class="sxs-lookup"><span data-stu-id="55c7e-131">Later in this tutorial a front-end VM is connected toothis subnet.</span></span> 

```azurecli-interactive 
az network vnet create \
  --resource-group myRGNetwork \
  --name myVnet \
  --address-prefix 10.0.0.0/16 \
  --subnet-name mySubnetFrontEnd \
  --subnet-prefix 10.0.1.0/24
```

### <a name="create-subnet"></a><span data-ttu-id="55c7e-132">Hozzon létre az alhálózatot</span><span class="sxs-lookup"><span data-stu-id="55c7e-132">Create subnet</span></span>

<span data-ttu-id="55c7e-133">Egy új alhálózatot kerül toohello virtuális hálózat hello [az alhálózaton virtuális hálózat létrehozása](/cli/azure/network/vnet/subnet#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="55c7e-133">A new subnet is added toohello virtual network using hello [az network vnet subnet create](/cli/azure/network/vnet/subnet#create) command.</span></span> <span data-ttu-id="55c7e-134">Ebben a példában hello alhálózati nevű *mySubnetBackEnd* , és egy címelőtagot *10.0.2.0/24*.</span><span class="sxs-lookup"><span data-stu-id="55c7e-134">In this example, hello subnet is named *mySubnetBackEnd* and is given an address prefix of *10.0.2.0/24*.</span></span> <span data-ttu-id="55c7e-135">Ez az alhálózat összes háttérszolgáltatások használatos.</span><span class="sxs-lookup"><span data-stu-id="55c7e-135">This subnet is used with all back-end services.</span></span>

```azurecli-interactive 
az network vnet subnet create \
  --resource-group myRGNetwork \
  --vnet-name myVnet \
  --name mySubnetBackEnd \
  --address-prefix 10.0.2.0/24
```

<span data-ttu-id="55c7e-136">Ezen a ponton a hálózati létrehozott és szegmentált két alhálózat, egy az előtér-szolgáltatásokat, és egy másikat a háttér-szolgáltatásaihoz.</span><span class="sxs-lookup"><span data-stu-id="55c7e-136">At this point, a network has been created and segmented into two subnets, one for front-end services, and another for back-end services.</span></span> <span data-ttu-id="55c7e-137">Hello a következő szakaszban a virtuális gépek jönnek létre, és toothese alhálózatok csatlakoztatva.</span><span class="sxs-lookup"><span data-stu-id="55c7e-137">In hello next section, virtual machines are created and connected toothese subnets.</span></span>

## <a name="understand-public-ip-address"></a><span data-ttu-id="55c7e-138">Nyilvános IP-cím ismertetése</span><span class="sxs-lookup"><span data-stu-id="55c7e-138">Understand public IP address</span></span>

<span data-ttu-id="55c7e-139">A nyilvános IP-cím lehetővé teszi, hogy Azure-erőforrások toobe elérhető-e hello internet.</span><span class="sxs-lookup"><span data-stu-id="55c7e-139">A public IP address allows Azure resources toobe accessible on hello internet.</span></span> <span data-ttu-id="55c7e-140">Ebben a szakaszban hello oktatóanyag egy virtuális gép létrehozása toodemonstrate hogyan toowork nyilvános IP-címek.</span><span class="sxs-lookup"><span data-stu-id="55c7e-140">In this section of hello tutorial, a VM is created toodemonstrate how toowork with public IP addresses.</span></span>

### <a name="allocation-method"></a><span data-ttu-id="55c7e-141">Lefoglalási módszer</span><span class="sxs-lookup"><span data-stu-id="55c7e-141">Allocation method</span></span>

<span data-ttu-id="55c7e-142">Egy nyilvános IP-címet, dinamikus vagy statikus oszthat ki.</span><span class="sxs-lookup"><span data-stu-id="55c7e-142">A public IP address can be allocated as either dynamic or static.</span></span> <span data-ttu-id="55c7e-143">Alapértelmezés szerint dinamikusan lefoglalt nyilvános IP-cím.</span><span class="sxs-lookup"><span data-stu-id="55c7e-143">By default, public IP address dynamically allocated.</span></span> <span data-ttu-id="55c7e-144">Dinamikus IP-címek van kiadva, ha egy virtuális gép felszabadítása.</span><span class="sxs-lookup"><span data-stu-id="55c7e-144">Dynamic IP addresses are released when a VM is deallocated.</span></span> <span data-ttu-id="55c7e-145">Ennek következtében hello IP-cím toochange bármely, amely tartalmazza a virtuális gép felszabadítás művelet során.</span><span class="sxs-lookup"><span data-stu-id="55c7e-145">This behavior causes hello IP address toochange during any operation that includes a VM deallocation.</span></span>

<span data-ttu-id="55c7e-146">hello elosztási módszert beállítható toostatic, amely biztosítja, hogy hello IP-címet meg kell maradnia hozzárendelt tooa VM, megszüntetett állapot esetén.</span><span class="sxs-lookup"><span data-stu-id="55c7e-146">hello allocation method can be set toostatic, which ensures that hello IP address remain assigned tooa VM, even during a deallocated state.</span></span> <span data-ttu-id="55c7e-147">A statikusan lefoglalt IP-cím használata esetén nem adható meg hello IP-címet saját magát.</span><span class="sxs-lookup"><span data-stu-id="55c7e-147">When using a statically allocated IP address, hello IP address itself cannot be specified.</span></span> <span data-ttu-id="55c7e-148">Ehelyett az történik a rendelkezésre álló címek készletét.</span><span class="sxs-lookup"><span data-stu-id="55c7e-148">Instead, it is allocated from a pool of available addresses.</span></span>

### <a name="dynamic-allocation"></a><span data-ttu-id="55c7e-149">Dinamikus lefoglalását</span><span class="sxs-lookup"><span data-stu-id="55c7e-149">Dynamic allocation</span></span>

<span data-ttu-id="55c7e-150">Virtuális gép létrehozásakor a hello [az virtuális gép létrehozása](/cli/azure/vm#create) parancs hello alapértelmezett nyilvános IP-cím címkiosztási módszerét dinamikus.</span><span class="sxs-lookup"><span data-stu-id="55c7e-150">When creating a VM with hello [az vm create](/cli/azure/vm#create) command, hello default public IP address allocation method is dynamic.</span></span> <span data-ttu-id="55c7e-151">A következő példa hello a virtuális gép létrehozása egy dinamikus IP-címmel.</span><span class="sxs-lookup"><span data-stu-id="55c7e-151">In hello following example, a VM is created with a dynamic IP address.</span></span> 

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

### <a name="static-allocation"></a><span data-ttu-id="55c7e-152">Statikus foglalási</span><span class="sxs-lookup"><span data-stu-id="55c7e-152">Static allocation</span></span>

<span data-ttu-id="55c7e-153">A virtuális gépek hello létrehozásakor [az virtuális gép létrehozása](/cli/azure/vm#create) paranccsal, hello tartalmazza `--public-ip-address-allocation static` argumentum tooassign egy statikus nyilvános IP-cím.</span><span class="sxs-lookup"><span data-stu-id="55c7e-153">When creating a virtual machine using hello [az vm create](/cli/azure/vm#create) command, include hello `--public-ip-address-allocation static` argument tooassign a static public IP address.</span></span> <span data-ttu-id="55c7e-154">Ez a művelet nem mutatja be ebben az oktatóanyagban azonban hello a következő szakaszban a dinamikusan kiosztott IP-cím módosított tooa statikusan lefoglalt címet.</span><span class="sxs-lookup"><span data-stu-id="55c7e-154">This operation is not demonstrated in this tutorial, however in hello next section a dynamically allocated IP address is changed tooa statically allocated address.</span></span> 

### <a name="change-allocation-method"></a><span data-ttu-id="55c7e-155">Elosztási módszer váltása</span><span class="sxs-lookup"><span data-stu-id="55c7e-155">Change allocation method</span></span>

<span data-ttu-id="55c7e-156">hello IP-cím címkiosztási módszerét hello módosítható [az hálózati nyilvános ip-frissítés](/cli/azure/network/public-ip#update) parancsot.</span><span class="sxs-lookup"><span data-stu-id="55c7e-156">hello IP address allocation method can be changed using hello [az network public-ip update](/cli/azure/network/public-ip#update) command.</span></span> <span data-ttu-id="55c7e-157">Ebben a példában hello IP-cím címkiosztási módszerét az előtér-VM megváltozott hello toostatic.</span><span class="sxs-lookup"><span data-stu-id="55c7e-157">In this example, hello IP address allocation method of hello front-end VM is changed toostatic.</span></span>

<span data-ttu-id="55c7e-158">Első lépésként felszabadítani hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="55c7e-158">First, deallocate hello VM.</span></span>

```azurecli-interactive 
az vm deallocate --resource-group myRGNetwork --name myFrontEndVM
```

<span data-ttu-id="55c7e-159">Használjon hello [az hálózati nyilvános ip-frissítés](/cli/azure/network/public-ip#update) tooupdate hello elosztási módszert parancsot.</span><span class="sxs-lookup"><span data-stu-id="55c7e-159">Use hello [az network public-ip update](/cli/azure/network/public-ip#update) command tooupdate hello allocation method.</span></span> <span data-ttu-id="55c7e-160">Ebben az esetben hello `--allocation-method` túl beállítása*statikus*.</span><span class="sxs-lookup"><span data-stu-id="55c7e-160">In this case, hello `--allocation-method` is being set too*static*.</span></span>

```azurecli-interactive 
az network public-ip update --resource-group myRGNetwork --name myFrontEndIP --allocation-method static
```

<span data-ttu-id="55c7e-161">Indítsa el a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="55c7e-161">Start hello VM.</span></span>

```azurecli-interactive 
az vm start --resource-group myRGNetwork --name myFrontEndVM --no-wait
```

### <a name="no-public-ip-address"></a><span data-ttu-id="55c7e-162">Nyilvános IP-cím</span><span class="sxs-lookup"><span data-stu-id="55c7e-162">No public IP address</span></span>

<span data-ttu-id="55c7e-163">Gyakran, egy virtuális Gépet nem kell toobe keresztül elérhető hello internet.</span><span class="sxs-lookup"><span data-stu-id="55c7e-163">Often, a VM does not need toobe accessible over hello internet.</span></span> <span data-ttu-id="55c7e-164">a virtuális gép, egy nyilvános IP-címet, hello használata nélkül toocreate `--public-ip-address ""` dupla idézőjelek között egy üres számú argumentum.</span><span class="sxs-lookup"><span data-stu-id="55c7e-164">toocreate a VM without a public IP address, use hello `--public-ip-address ""` argument with an empty set of double quotes.</span></span> <span data-ttu-id="55c7e-165">Ez a konfiguráció az oktatóanyag későbbi részében bemutatott</span><span class="sxs-lookup"><span data-stu-id="55c7e-165">This configuration is demonstrated later in this tutorial</span></span>

## <a name="secure-network-traffic"></a><span data-ttu-id="55c7e-166">Biztonságos hálózati adatforgalom</span><span class="sxs-lookup"><span data-stu-id="55c7e-166">Secure network traffic</span></span>

<span data-ttu-id="55c7e-167">A hálózati biztonsági csoport (NSG) felsorolja azokat a szabályokat, amelyek engedélyezik vagy megtagadják a hálózati forgalom tooresources csatlakoztatott tooAzure virtuális hálózatot (VNet).</span><span class="sxs-lookup"><span data-stu-id="55c7e-167">A network security group (NSG) contains a list of security rules that allow or deny network traffic tooresources connected tooAzure Virtual Networks (VNet).</span></span> <span data-ttu-id="55c7e-168">Az NSG-k társított toosubnets vagy az egyes hálózati adapterek lehetnek.</span><span class="sxs-lookup"><span data-stu-id="55c7e-168">NSGs can be associated toosubnets or individual network interfaces.</span></span> <span data-ttu-id="55c7e-169">Ha egy NSG-t egy adott hálózati csatoló kapcsolódik, érvényes hello társított virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="55c7e-169">When an NSG is associated with a network interface, it applies only hello associated VM.</span></span> <span data-ttu-id="55c7e-170">Amikor egy NSG társított tooa alhálózati, hello szabályokat alkalmazni tooall erőforrások csatlakoztatott toohello alhálózat.</span><span class="sxs-lookup"><span data-stu-id="55c7e-170">When an NSG is associated tooa subnet, hello rules apply tooall resources connected toohello subnet.</span></span> 

### <a name="network-security-group-rules"></a><span data-ttu-id="55c7e-171">Hálózati biztonsági csoportszabályok</span><span class="sxs-lookup"><span data-stu-id="55c7e-171">Network security group rules</span></span>

<span data-ttu-id="55c7e-172">NSG-szabályok határozza meg, amelyben forgalom engedélyezett vagy megtagadott hálózati portok.</span><span class="sxs-lookup"><span data-stu-id="55c7e-172">NSG rules define networking ports over which traffic is allowed or denied.</span></span> <span data-ttu-id="55c7e-173">hello szabályok is tartalmazza a forrás és cél IP-címtartományok, így egyes rendszerek vagy az alhálózatok közötti forgalmat szabályozott.</span><span class="sxs-lookup"><span data-stu-id="55c7e-173">hello rules can include source and destination IP address ranges so that traffic is controlled between specific systems or subnets.</span></span> <span data-ttu-id="55c7e-174">NSG-szabályok terjednie a prioritással (1 – és 4096).</span><span class="sxs-lookup"><span data-stu-id="55c7e-174">NSG rules also include a priority (between 1—and 4096).</span></span> <span data-ttu-id="55c7e-175">Szabályok a prioritásuk szerinti sorrendben hello értékeli ki a rendszer.</span><span class="sxs-lookup"><span data-stu-id="55c7e-175">Rules are evaluated in hello order of priority.</span></span> <span data-ttu-id="55c7e-176">A 100 prioritással kiértékeli előtt szabály 200 prioritással.</span><span class="sxs-lookup"><span data-stu-id="55c7e-176">A rule with a priority of 100 is evaluated before a rule with priority 200.</span></span>

<span data-ttu-id="55c7e-177">Minden NSG tartalmaz egy alapértelmezett szabálykészletet.</span><span class="sxs-lookup"><span data-stu-id="55c7e-177">All NSGs contain a set of default rules.</span></span> <span data-ttu-id="55c7e-178">hello alapértelmezett szabályokat nem lehet törölni, de a legalacsonyabb prioritású hello hozzárendeli őket, mert azok felülbírálhatja hello létrehozott szabályok.</span><span class="sxs-lookup"><span data-stu-id="55c7e-178">hello default rules cannot be deleted, but because they are assigned hello lowest priority, they can be overridden by hello rules that you create.</span></span>

- <span data-ttu-id="55c7e-179">**Virtuális hálózati** - származó forgalmat, és a befejezési egy virtuális hálózat bejövő és kimenő irányban is.</span><span class="sxs-lookup"><span data-stu-id="55c7e-179">**Virtual network** - Traffic originating and ending in a virtual network is allowed both in inbound and outbound directions.</span></span>
- <span data-ttu-id="55c7e-180">**Internet** – a kimenő forgalom engedélyezve van, de a bejövő forgalmat blokkol.</span><span class="sxs-lookup"><span data-stu-id="55c7e-180">**Internet** - Outbound traffic is allowed, but inbound traffic is blocked.</span></span>
- <span data-ttu-id="55c7e-181">**Terheléselosztó** -engedélyezése Azure load terheléselosztó tooprobe hello állapotát a virtuális gépek és a szerepkörpéldányok.</span><span class="sxs-lookup"><span data-stu-id="55c7e-181">**Load balancer** - Allow Azure’s load balancer tooprobe hello health of your VMs and role instances.</span></span> <span data-ttu-id="55c7e-182">Ha nem használ elosztott terhelésű készlet, ez a szabály lehet felülbírálni.</span><span class="sxs-lookup"><span data-stu-id="55c7e-182">If you are not using a load balanced set, you can override this rule.</span></span>

### <a name="create-network-security-groups"></a><span data-ttu-id="55c7e-183">Hálózati biztonsági csoportok létrehozása</span><span class="sxs-lookup"><span data-stu-id="55c7e-183">Create network security groups</span></span>

<span data-ttu-id="55c7e-184">Hálózati biztonsági csoport is létrehozható, hello azonos időben is a virtuális gépek hello [az virtuális gép létrehozása](/cli/azure/vm#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="55c7e-184">A network security group can be created at hello same time as a VM using hello [az vm create](/cli/azure/vm#create) command.</span></span> <span data-ttu-id="55c7e-185">Annak során, hello NSG hello virtuális gépek hálózati illesztő társítva, és egy NSG-szabály automatikusan létrehozott tooallow adatforgalmat porton *22* forrásból.</span><span class="sxs-lookup"><span data-stu-id="55c7e-185">When doing so, hello NSG is associated with hello VMs network interface and an NSG rule is auto created tooallow traffic on port *22* from any source.</span></span> <span data-ttu-id="55c7e-186">Ebben az oktatóanyagban a korábbi előtér-NSG volt az automatikusan létrehozott hello hello előtér-virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="55c7e-186">Earlier in this tutorial, hello front-end NSG was auto-created with hello front-end VM.</span></span> <span data-ttu-id="55c7e-187">Az NSG-szabályok is 22-es port számára létrehozott automatikus volt.</span><span class="sxs-lookup"><span data-stu-id="55c7e-187">An NSG rule was also auto created for port 22.</span></span> 

<span data-ttu-id="55c7e-188">Bizonyos esetekben lehet hasznos toopre-hozzon létre egy NSG-t, például amikor alapértelmezett SSH szabályokat nem lehet létrehozni, vagy ha hello NSG csatolt tooa alhálózaton kell lennie.</span><span class="sxs-lookup"><span data-stu-id="55c7e-188">In some cases, it may be helpful toopre-create an NSG, such as when default SSH rules should not be created, or when hello NSG should be attached tooa subnet.</span></span> 

<span data-ttu-id="55c7e-189">Használjon hello [az hálózati nsg létrehozása](/cli/azure/network/nsg#create) parancs toocreate hálózati biztonsági csoport.</span><span class="sxs-lookup"><span data-stu-id="55c7e-189">Use hello [az network nsg create](/cli/azure/network/nsg#create) command toocreate a network security group.</span></span>

```azurecli-interactive 
az network nsg create --resource-group myRGNetwork --name myNSGBackEnd
```

<span data-ttu-id="55c7e-190">Hello NSG tooa hálózati adapter közötti társítás, helyett hozzárendelnek egy alhálózathoz.</span><span class="sxs-lookup"><span data-stu-id="55c7e-190">Instead of associating hello NSG tooa network interface, it is associated with a subnet.</span></span> <span data-ttu-id="55c7e-191">Ebben a konfigurációban a virtuális gép, amely csatolt toohello örökli hello NSG-szabályok.</span><span class="sxs-lookup"><span data-stu-id="55c7e-191">In this configuration, any VM that is attached toohello subnet inherits hello NSG rules.</span></span>

<span data-ttu-id="55c7e-192">Hello meglévő nevű alhálózat frissítése *mySubnetBackEnd* rendelkező új NSG hello.</span><span class="sxs-lookup"><span data-stu-id="55c7e-192">Update hello existing subnet named *mySubnetBackEnd* with hello new NSG.</span></span>

```azurecli-interactive 
az network vnet subnet update \
  --resource-group myRGNetwork \
  --vnet-name myVnet \
  --name mySubnetBackEnd \
  --network-security-group myNSGBackEnd
```

<span data-ttu-id="55c7e-193">Most létrehoz egy virtuális gépet, amely csatolt toohello *mySubnetBackEnd*.</span><span class="sxs-lookup"><span data-stu-id="55c7e-193">Now create a virtual machine, which is attached toohello *mySubnetBackEnd*.</span></span> <span data-ttu-id="55c7e-194">Figyelje meg, hogy hello `--nsg` argumentum értéke üres dupla idézőjelek között.</span><span class="sxs-lookup"><span data-stu-id="55c7e-194">Notice that hello `--nsg` argument has a value of empty double quotes.</span></span> <span data-ttu-id="55c7e-195">Az NSG nem kell létrehozni hello VM toobe.</span><span class="sxs-lookup"><span data-stu-id="55c7e-195">An NSG does not need toobe created with hello VM.</span></span> <span data-ttu-id="55c7e-196">virtuális gép hello csatolt toohello háttér-alhálózathoz, előre létrehozott, a háttér NSG hello védi.</span><span class="sxs-lookup"><span data-stu-id="55c7e-196">hello VM is attached toohello back-end subnet, which is protected with hello pre-created back-end NSG.</span></span> <span data-ttu-id="55c7e-197">Az NSG toohello VM vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="55c7e-197">This NSG applies toohello VM.</span></span> <span data-ttu-id="55c7e-198">Emellett az információkban a hello `--public-ip-address` argumentum értéke üres dupla idézőjelek között.</span><span class="sxs-lookup"><span data-stu-id="55c7e-198">Also, notice here that hello `--public-ip-address` argument has a value of empty double quotes.</span></span> <span data-ttu-id="55c7e-199">Ez a konfiguráció nélkül egy nyilvános IP-cím egy virtuális Gépet hoz létre.</span><span class="sxs-lookup"><span data-stu-id="55c7e-199">This configuration creates a VM without a public IP address.</span></span> 

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

### <a name="secure-incoming-traffic"></a><span data-ttu-id="55c7e-200">Biztonságos bejövő forgalom</span><span class="sxs-lookup"><span data-stu-id="55c7e-200">Secure incoming traffic</span></span>

<span data-ttu-id="55c7e-201">Hello előtér-virtuális gép létrehozása után az NSG-szabályok létrehozásának tooallow bejövő forgalom 22-es porton.</span><span class="sxs-lookup"><span data-stu-id="55c7e-201">When hello front-end VM was created, an NSG rule was created tooallow incoming traffic on port 22.</span></span> <span data-ttu-id="55c7e-202">Ez a szabály lehetővé teszi, hogy az SSH-kapcsolatok toohello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="55c7e-202">This rule allows SSH connections toohello VM.</span></span> <span data-ttu-id="55c7e-203">Ebben a példában a forgalmat is engedélyezni kell a porton *80*.</span><span class="sxs-lookup"><span data-stu-id="55c7e-203">For this example, traffic should also be allowed on port *80*.</span></span> <span data-ttu-id="55c7e-204">Ez a konfiguráció lehetővé teszi, hogy egy webes alkalmazás toobe hello VM elérhetők.</span><span class="sxs-lookup"><span data-stu-id="55c7e-204">This configuration allows a web application toobe accessed on hello VM.</span></span>

<span data-ttu-id="55c7e-205">Használjon hello [az hálózati nsg-szabály létrehozása](/cli/azure/network/nsg/rule#create) parancs toocreate port szabály *80*.</span><span class="sxs-lookup"><span data-stu-id="55c7e-205">Use hello [az network nsg rule create](/cli/azure/network/nsg/rule#create) command toocreate a rule for port *80*.</span></span>

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

<span data-ttu-id="55c7e-206">hello előtér-virtuális gép jelenleg csak elérhető porton *22* és port *80*.</span><span class="sxs-lookup"><span data-stu-id="55c7e-206">hello front-end VM is now only accessible on port *22* and port *80*.</span></span> <span data-ttu-id="55c7e-207">Minden bejövő forgalom hello hálózati biztonsági csoport blokkolva van.</span><span class="sxs-lookup"><span data-stu-id="55c7e-207">All other incoming traffic is blocked at hello network security group.</span></span> <span data-ttu-id="55c7e-208">Akkor lehet hasznos toovisualize hello NSG-szabályok konfigurációi.</span><span class="sxs-lookup"><span data-stu-id="55c7e-208">It may be helpful toovisualize hello NSG rule configurations.</span></span> <span data-ttu-id="55c7e-209">Visszatérési hello NSG-szabály konfiguráció hello [az hálózati szabálylistában](/cli/azure/network/nsg/rule#list) parancsot.</span><span class="sxs-lookup"><span data-stu-id="55c7e-209">Return hello NSG rule configuration with hello [az network rule list](/cli/azure/network/nsg/rule#list) command.</span></span> 

```azurecli-interactive 
az network nsg rule list --resource-group myRGNetwork --nsg-name myNSGFrontEnd --output table
```

<span data-ttu-id="55c7e-210">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="55c7e-210">Output:</span></span>

```azurecli-interactive 
Access    DestinationAddressPrefix      DestinationPortRange  Direction    Name                 Priority  Protocol    ProvisioningState    ResourceGroup    SourceAddressPrefix    SourcePortRange
--------  --------------------------  ----------------------  -----------  -----------------  ----------  ----------  -------------------  ---------------  ---------------------  -----------------
Allow     *                                               22  Inbound      default-allow-ssh        1000  Tcp         Succeeded            myRGNetwork      *                      *
Allow     *                                               80  Inbound      http                      200  Tcp         Succeeded            myRGNetwork      *                      *
```

### <a name="secure-vm-toovm-traffic"></a><span data-ttu-id="55c7e-211">Virtuális gép tooVM forgalmának biztonságossá tétele</span><span class="sxs-lookup"><span data-stu-id="55c7e-211">Secure VM tooVM traffic</span></span>

<span data-ttu-id="55c7e-212">Virtuális gépek közötti hálózati biztonsági csoportszabályok is alkalmazhat.</span><span class="sxs-lookup"><span data-stu-id="55c7e-212">Network security group rules can also apply between VMs.</span></span> <span data-ttu-id="55c7e-213">Ebben a példában az előtér-virtuális gép van szüksége a toocommunicate hello hello háttér-VM porton *22* és *3306*.</span><span class="sxs-lookup"><span data-stu-id="55c7e-213">For this example, hello front-end VM needs toocommunicate with hello back-end VM on port *22* and *3306*.</span></span> <span data-ttu-id="55c7e-214">Ez a konfiguráció lehetővé teszi, hogy az SSH-kapcsolatok az előtér-VM hello és is annak engedélyezése, hogy egy alkalmazás hello előtér-VM toocommunicate a háttér-MySQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="55c7e-214">This configuration allows SSH connections from hello front-end VM, and also allow an application on hello front-end VM toocommunicate with a back-end MySQL database.</span></span> <span data-ttu-id="55c7e-215">Az összes többi forgalom hello előtér- és a virtuális gépek között le kell tiltani.</span><span class="sxs-lookup"><span data-stu-id="55c7e-215">All other traffic should be blocked between hello front-end and back-end virtual machines.</span></span>

<span data-ttu-id="55c7e-216">Használjon hello [az hálózati nsg-szabály létrehozása](/cli/azure/network/nsg/rule#create) parancs toocreate egy szabály a 22-es portot.</span><span class="sxs-lookup"><span data-stu-id="55c7e-216">Use hello [az network nsg rule create](/cli/azure/network/nsg/rule#create) command toocreate a rule for port 22.</span></span> <span data-ttu-id="55c7e-217">Figyelje meg, hogy hello `--source-address-prefix` argumentum meghatározza a érték *10.0.1.0/24*.</span><span class="sxs-lookup"><span data-stu-id="55c7e-217">Notice that hello `--source-address-prefix` argument specifies a value of *10.0.1.0/24*.</span></span> <span data-ttu-id="55c7e-218">Ez a konfiguráció biztosítja, hogy az csak a hello előtér-alhálózatból forgalom engedélyezve van-e keresztül hello NSG.</span><span class="sxs-lookup"><span data-stu-id="55c7e-218">This configuration ensures that only traffic from hello front-end subnet is allowed through hello NSG.</span></span>

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

<span data-ttu-id="55c7e-219">Most adja hozzá egy forgalomra vonatkozó szabály MySQL 3306 porton.</span><span class="sxs-lookup"><span data-stu-id="55c7e-219">Now add a rule for MySQL traffic on port 3306.</span></span>

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

<span data-ttu-id="55c7e-220">Végül, mert NSG-ket egy alapértelmezett szabály engedélyezése minden forgalomnak hello a virtuális gépek közötti azonos VNet szabály hozható létre hello háttér-NSG-k tooblock minden forgalmat.</span><span class="sxs-lookup"><span data-stu-id="55c7e-220">Finally, because NSGs have a default rule allowing all traffic between VMs in hello same VNet, a rule can be created for hello back-end NSGs tooblock all traffic.</span></span> <span data-ttu-id="55c7e-221">Itt láthatja, hogy hello `--priority` értéket kap *300*, amelyek verziója alacsonyabb, hogy mindkét hello NSG-t és a MySQL szabályokat.</span><span class="sxs-lookup"><span data-stu-id="55c7e-221">Notice here that hello `--priority` is given a value of *300*, which is lower that both hello NSG and MySQL rules.</span></span> <span data-ttu-id="55c7e-222">Ez a konfiguráció biztosítja, hogy SSH és a MySQL-forgalmat a rendszer nem tagadja hello NSG keresztül.</span><span class="sxs-lookup"><span data-stu-id="55c7e-222">This configuration ensures that SSH and MySQL traffic is still allowed through hello NSG.</span></span>

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

<span data-ttu-id="55c7e-223">hello háttér-virtuális gép jelenleg csak elérhető porton *22* és port *3306* hello előtér-alhálózatról.</span><span class="sxs-lookup"><span data-stu-id="55c7e-223">hello back-end VM is now only accessible on port *22* and port *3306* from hello front-end subnet.</span></span> <span data-ttu-id="55c7e-224">Minden bejövő forgalom hello hálózati biztonsági csoport blokkolva van.</span><span class="sxs-lookup"><span data-stu-id="55c7e-224">All other incoming traffic is blocked at hello network security group.</span></span> <span data-ttu-id="55c7e-225">Akkor lehet hasznos toovisualize hello NSG-szabályok konfigurációi.</span><span class="sxs-lookup"><span data-stu-id="55c7e-225">It may be helpful toovisualize hello NSG rule configurations.</span></span> <span data-ttu-id="55c7e-226">Visszatérési hello NSG-szabály konfiguráció hello [az hálózati szabálylistában](/cli/azure/network/nsg/rule#list) parancsot.</span><span class="sxs-lookup"><span data-stu-id="55c7e-226">Return hello NSG rule configuration with hello [az network rule list](/cli/azure/network/nsg/rule#list) command.</span></span> 

```azurecli-interactive 
az network nsg rule list --resource-group myRGNetwork --nsg-name myNSGBackEnd --output table
```

<span data-ttu-id="55c7e-227">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="55c7e-227">Output:</span></span>

```azurecli-interactive 
Access    DestinationAddressPrefix    DestinationPortRange    Direction    Name       Priority  Protocol    ProvisioningState    ResourceGroup    SourceAddressPrefix    SourcePortRange
--------  --------------------------  ----------------------  -----------  -------  ----------  ----------  -------------------  ---------------  ---------------------  -----------------
Allow     *                           22                      Inbound      SSH             100  Tcp         Succeeded            myRGNetwork      10.0.1.0/24            *
Allow     *                           3306                    Inbound      MySQL           200  Tcp         Succeeded            myRGNetwork      10.0.1.0/24            *
Deny      *                           *                       Inbound      denyAll         300  Tcp         Succeeded            myRGNetwork      *                      *
```

## <a name="next-steps"></a><span data-ttu-id="55c7e-228">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="55c7e-228">Next steps</span></span>

<span data-ttu-id="55c7e-229">Ebben az oktatóanyagban létre, és biztonságos kapcsolódó toovirtual gépként Azure hálózatokhoz.</span><span class="sxs-lookup"><span data-stu-id="55c7e-229">In this tutorial, you created and secured Azure networks as related toovirtual machines.</span></span> <span data-ttu-id="55c7e-230">Megismerte, hogyan végezheti el az alábbi műveleteket:</span><span class="sxs-lookup"><span data-stu-id="55c7e-230">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="55c7e-231">Telepíthet egy virtuális hálózatot</span><span class="sxs-lookup"><span data-stu-id="55c7e-231">Deploy a virtual network</span></span>
> * <span data-ttu-id="55c7e-232">Hozzon létre egy alhálózatot a virtuális hálózaton belül</span><span class="sxs-lookup"><span data-stu-id="55c7e-232">Create a subnet within a virtual network</span></span>
> * <span data-ttu-id="55c7e-233">Virtuális gépek tooa alhálózati csatolása</span><span class="sxs-lookup"><span data-stu-id="55c7e-233">Attach virtual machines tooa subnet</span></span>
> * <span data-ttu-id="55c7e-234">Virtuális gép nyilvános IP-címeinek kezelése</span><span class="sxs-lookup"><span data-stu-id="55c7e-234">Manage virtual machine public IP addresses</span></span>
> * <span data-ttu-id="55c7e-235">Bejövő internet forgalmának biztonságossá tétele</span><span class="sxs-lookup"><span data-stu-id="55c7e-235">Secure incoming internet traffic</span></span>
> * <span data-ttu-id="55c7e-236">Virtuális gép tooVM forgalmának biztonságossá tétele</span><span class="sxs-lookup"><span data-stu-id="55c7e-236">Secure VM tooVM traffic</span></span>

<span data-ttu-id="55c7e-237">Előzetes toohello oktatóanyag következő toolearn kapcsolatos adatok az Azure backup használatával virtuális gépek védelme.</span><span class="sxs-lookup"><span data-stu-id="55c7e-237">Advance toohello next tutorial toolearn about securing data on virtual machines using Azure backup.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="55c7e-238">Készítsen biztonsági másolatot a Linux virtuális gépek Azure-ban</span><span class="sxs-lookup"><span data-stu-id="55c7e-238">Back up Linux virtual machines in Azure</span></span>](./tutorial-backup-vms.md)
