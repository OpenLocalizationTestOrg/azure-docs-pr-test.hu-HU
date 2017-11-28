---
title: "aaaAvailability oktatóanyag beállítja a Linux virtuális gépek Azure-ban |} Microsoft Docs"
description: "További információk a rendelkezésre állási készletek hello Linux virtuális gépek Azure-ban."
documentationcenter: 
services: virtual-machines-linux
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 2a91e4a6057180035ec51410d9fffccaca343758
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-availability-sets"></a><span data-ttu-id="24be4-103">Hogyan toouse rendelkezésre állási beállítása</span><span class="sxs-lookup"><span data-stu-id="24be4-103">How toouse availability sets</span></span>


<span data-ttu-id="24be4-104">Ebből az oktatóanyagból megtudhatja, hogyan tooincrease hello rendelkezésre állása és megbízhatósága szempontjából a virtuális gép megoldások Azure-ban egy képesség nevű rendelkezésre állási készletek.</span><span class="sxs-lookup"><span data-stu-id="24be4-104">In this tutorial, you will learn how tooincrease hello availability and reliability of your Virtual Machine solutions on Azure using a capability called Availability Sets.</span></span> <span data-ttu-id="24be4-105">Rendelkezésre állási készletek győződjön meg arról, hogy több elkülönített hardver fürt központi telepítése az Azure virtuális gépek elosztott hello.</span><span class="sxs-lookup"><span data-stu-id="24be4-105">Availability sets ensure that hello VMs you deploy on Azure are distributed across multiple isolated hardware clusters.</span></span> <span data-ttu-id="24be4-106">Ez biztosítja, hogy ha az Azure hardveres vagy szoftveres hiba akkor fordul elő, csak a virtuális gépek alárendelt meghatározott csökkenhet, és, amely a teljes megoldás elérhető és az azt használó ügyfelek hello szempontjából működési marad.</span><span class="sxs-lookup"><span data-stu-id="24be4-106">Doing this ensures that if a hardware or software failure within Azure happens, only a sub-set of your VMs will be impacted and that your overall solution will remain available and operational from hello perspective of your customers using it.</span></span>

<span data-ttu-id="24be4-107">Eben az oktatóanyagban az alábbiakkal fog megismerkedni:</span><span class="sxs-lookup"><span data-stu-id="24be4-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="24be4-108">Rendelkezésre állási csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="24be4-108">Create an availability set</span></span>
> * <span data-ttu-id="24be4-109">Hozzon létre egy virtuális gép rendelkezésre állási csoportba</span><span class="sxs-lookup"><span data-stu-id="24be4-109">Create a VM in an availability set</span></span>
> * <span data-ttu-id="24be4-110">Ellenőrizze a rendelkezésre álló Virtuálisgép-méretek</span><span class="sxs-lookup"><span data-stu-id="24be4-110">Check available VM sizes</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="24be4-111">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ez az oktatóanyag van szükség, hogy verzióját hello Azure CLI 2.0.4 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="24be4-111">If you choose tooinstall and use hello CLI locally, this tutorial requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="24be4-112">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="24be4-112">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="24be4-113">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="24be4-113">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="availability-set-overview"></a><span data-ttu-id="24be4-114">Rendelkezésre állási csoport – áttekintés</span><span class="sxs-lookup"><span data-stu-id="24be4-114">Availability set overview</span></span>

<span data-ttu-id="24be4-115">A logikai csoportosításhoz képessége, amelyik használható rendelkezésre állási csoport megtalálható, hogy hello Virtuálisgép-erőforrások helyezi-e benne el különítve egymástól belül egy Azure-adatközpontban telepítésekor Azure tooensure.</span><span class="sxs-lookup"><span data-stu-id="24be4-115">An Availability Set is a logical grouping capability that you can use in Azure tooensure that hello VM resources you place within it are isolated from each other when they are deployed within an Azure datacenter.</span></span> <span data-ttu-id="24be4-116">Azure biztosítja, hogy hello helyezi-e belül egy rendelkezésre állási készlet több fizikai kiszolgálókon futó virtuális gépek, a számítási, például rackszekrények, a tárolási egység és a hálózati kapcsolók.</span><span class="sxs-lookup"><span data-stu-id="24be4-116">Azure ensures that hello VMs you place within an Availability Set run across multiple physical servers, compute racks, storage units, and network switches.</span></span> <span data-ttu-id="24be4-117">Ez biztosítja, hogy hello eseményben hardver vagy Azure szoftverhiba lép fel, csökkenhet a virtuális gépek csak egy részét, és az alkalmazás általános mentése marad, és továbbra is toobe elérhető tooyour ügyfelek.</span><span class="sxs-lookup"><span data-stu-id="24be4-117">This ensures that in hello event of a hardware or Azure software failure, only a subset of your VMs will be impacted, and your overall application will stay up and continue toobe available tooyour customers.</span></span> <span data-ttu-id="24be4-118">Az alapvető funkció tooleverage rendelkezésre állási csoportokkal akkor, ha azt szeretné, hogy megbízható toobuild megoldások.</span><span class="sxs-lookup"><span data-stu-id="24be4-118">Using Availability Sets is an essential capability tooleverage when you want toobuild reliable cloud solutions.</span></span>

<span data-ttu-id="24be4-119">Mérlegeljük, ahol például 4 előtér-webkiszolgáló, és 2 háttér-virtuális gép egy adatbázist az tipikus Virtuálisgép-alapú megoldás.</span><span class="sxs-lookup"><span data-stu-id="24be4-119">Let’s consider a typical VM-based solution where you might have 4 front-end web servers and use 2 back-end VMs that host a database.</span></span> <span data-ttu-id="24be4-120">Az Azure-érdemes toodefine két rendelkezésre állási csoportok csak a virtuális gépekre telepítheti: egy rendelkezésre állási készlet hello "web" réteg és a rendelkezésre állási csoporthoz hello "adatbázis" szinthez.</span><span class="sxs-lookup"><span data-stu-id="24be4-120">With Azure, you’d want toodefine two availability sets before you deploy your VMs: one availability set for hello “web” tier and one availability set for hello “database” tier.</span></span> <span data-ttu-id="24be4-121">Egy új virtuális Gépet, majd megadhatja a hello a rendelkezésre állási csoportban paraméter toohello az virtuális gép létrehozása parancs, és az Azure automatikusan biztosítja, hogy hello belül elérhető hello hoz létre virtuális gépek létrehozásakor a készlet több fizikai hardver-erőforrások között munkakönyvtárral.</span><span class="sxs-lookup"><span data-stu-id="24be4-121">When you create a new VM you can then specify hello availability set as a parameter toohello az vm create command, and Azure will automatically ensure that hello VMs you create within hello available set are isolated across multiple physical hardware resources.</span></span> <span data-ttu-id="24be4-122">Ez azt jelenti, hogy hello fizikai hardver, az egyik webkiszolgálón vagy adatbázis-kiszolgáló virtuális gépeken futó hibásan működik, ha tudja, hogy hello a webkiszolgáló és az adatbázis virtuális gépek más példányai marad futó részletes mert másik hardveren.</span><span class="sxs-lookup"><span data-stu-id="24be4-122">This means that if hello physical hardware that one of your Web Server or Database Server VMs is running on has a problem, you know that hello other instances of your Web Server and Database VMs will remain running fine because they are on different hardware.</span></span>

<span data-ttu-id="24be4-123">Rendelkezésre állási készletek mindig használjon, ha azt szeretné, hogy toodeploy megbízható Virtuálisgép-alapú megoldások Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="24be4-123">You should always use Availability Sets when you want toodeploy reliable VM-based solutions within Azure.</span></span>


## <a name="create-an-availability-set"></a><span data-ttu-id="24be4-124">Rendelkezésre állási csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="24be4-124">Create an availability set</span></span>

<span data-ttu-id="24be4-125">Rendelkezésre állási készlet használatával hozhat létre [az virtuális gép rendelkezésre állási-csoport létrehozása](/cli/azure/vm/availability-set#create).</span><span class="sxs-lookup"><span data-stu-id="24be4-125">You can create an availability set using [az vm availability-set create](/cli/azure/vm/availability-set#create).</span></span> <span data-ttu-id="24be4-126">Ebben a példában a frissítés és a tartalék tartományok mindkét hello száma hivatott *2* a hello rendelkezésre állási csoport elnevezett *myAvailabilitySet* a hello *myResourceGroupAvailability*erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="24be4-126">In this example, we set both hello number of update and fault domains at *2* for hello availability set named *myAvailabilitySet* in hello *myResourceGroupAvailability* resource group.</span></span>

<span data-ttu-id="24be4-127">Hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="24be4-127">Create a resource group.</span></span>

```azurecli-interactive 
az group create --name myResourceGroupAvailability --location eastus
```


```azurecli-interactive 
az vm availability-set create \
    --resource-group myResourceGroupAvailability \
    --name myAvailabilitySet \
    --platform-fault-domain-count 2 \
    --platform-update-domain-count 2
```

<span data-ttu-id="24be4-128">Rendelkezésre állási készletek lehetővé teszik a tooisolate erőforrások "tartalék tartományok" és "tartományok frissítése".</span><span class="sxs-lookup"><span data-stu-id="24be4-128">Availability Sets allow you tooisolate resources across "fault domains" and "update domains".</span></span> <span data-ttu-id="24be4-129">A **tartalék tartomány** kiszolgáló + hálózati, tárolási elkülönített gyűjteményét képviseli erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="24be4-129">A **fault domain** represents an isolated collection of server + network + storage resources.</span></span> <span data-ttu-id="24be4-130">A fenti példa hello azt jelzi, hogy azt szeretné, hogy a rendelkezésre állási csoport toobe elosztva legalább két tartalék tartományok között, ha a virtuális gépek vannak telepítve.</span><span class="sxs-lookup"><span data-stu-id="24be4-130">In hello preceding example, we indicate that we want our availability set toobe distributed across at least two fault domains when our VMs are deployed.</span></span> <span data-ttu-id="24be4-131">Azt is jelzi, hogy azt szeretné, hogy a rendelkezésre állási készlet elosztott két **tartományok frissítése**.</span><span class="sxs-lookup"><span data-stu-id="24be4-131">We also indicate that we want our availability set distributed across two **update domains**.</span></span>  <span data-ttu-id="24be4-132">Két frissítési tartományok gondoskodjon arról, hogy amikor a szoftverfrissítések az Azure-ban a Virtuálisgép-erőforrások elkülönített, akadályozza meg, hogy minden hello szoftver frissítését, hello alatt a virtuális gép fut egyszerre.</span><span class="sxs-lookup"><span data-stu-id="24be4-132">Two update domains ensure that when Azure performs software updates our VM resources are isolated, preventing all hello software running underneath our VM from being updated at hello same time.</span></span>

## <a name="configure-virtual-network"></a><span data-ttu-id="24be4-133">Virtuális hálózat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="24be4-133">Configure virtual network</span></span>
<span data-ttu-id="24be4-134">Mielőtt központilag az egyes virtuális gépek, és tesztelheti a terheléselosztó, hozzon létre virtuális hálózati erőforrások támogató hello.</span><span class="sxs-lookup"><span data-stu-id="24be4-134">Before you deploy some VMs and can test your balancer, create hello supporting virtual network resources.</span></span> <span data-ttu-id="24be4-135">Virtuális hálózatok kapcsolatos további információkért lásd: hello [Azure virtuális hálózatok kezelése](tutorial-virtual-network.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="24be4-135">For more information about virtual networks, see hello [Manage Azure Virtual Networks](tutorial-virtual-network.md) tutorial.</span></span>

### <a name="create-network-resources"></a><span data-ttu-id="24be4-136">Hálózati erőforrások létrehozása</span><span class="sxs-lookup"><span data-stu-id="24be4-136">Create network resources</span></span>
<span data-ttu-id="24be4-137">A virtuális hálózat létrehozása [az hálózati vnet létrehozása](/cli/azure/network/vnet#create).</span><span class="sxs-lookup"><span data-stu-id="24be4-137">Create a virtual network with [az network vnet create](/cli/azure/network/vnet#create).</span></span> <span data-ttu-id="24be4-138">hello alábbi példa létrehoz egy virtuális hálózatot nevű *myVnet* nevű alhálózattal *mySubnet*:</span><span class="sxs-lookup"><span data-stu-id="24be4-138">hello following example creates a virtual network named *myVnet* with a subnet named *mySubnet*:</span></span>

```azurecli-interactive 
az network vnet create \
    --resource-group myResourceGroupAvailability \
    --name myVnet \
    --subnet-name mySubnet
```
<span data-ttu-id="24be4-139">Virtuális hálózati adapter jönnek létre [az hálózat összevont hálózati létrehozása](/cli/azure/network/nic#create).</span><span class="sxs-lookup"><span data-stu-id="24be4-139">Virtual NICs are created with [az network nic create](/cli/azure/network/nic#create).</span></span> <span data-ttu-id="24be4-140">hello alábbi példakód létrehozza három virtuális hálózati adapter.</span><span class="sxs-lookup"><span data-stu-id="24be4-140">hello following example creates three virtual NICs.</span></span> <span data-ttu-id="24be4-141">(Az egyes virtuális gépek virtuális hálózati adapter egy hoz létre a az alkalmazást a következő hello lépések).</span><span class="sxs-lookup"><span data-stu-id="24be4-141">(One virtual NIC for each VM you create for your app in hello following steps).</span></span> <span data-ttu-id="24be4-142">További virtuális hálózati adapterek és virtuális gépek létrehozása tetszőleges időpontban, és azok hozzáadása toohello terheléselosztó:</span><span class="sxs-lookup"><span data-stu-id="24be4-142">You can create additional virtual NICs and VMs at any time and add them toohello load balancer:</span></span>

```bash
for i in `seq 1 3`; do
    az network nic create \
        --resource-group myResourceGroupAvailability \
        --name myNic$i \
        --vnet-name myVnet \
        --subnet mySubnet \
        --lb-name myLoadBalancer \
        --lb-address-pools myBackEndPool
done
```

## <a name="create-vms-inside-an-availability-set"></a><span data-ttu-id="24be4-143">Hozzon létre virtuális gépek rendelkezésre állási csoportok belül</span><span class="sxs-lookup"><span data-stu-id="24be4-143">Create VMs inside an availability set</span></span>

<span data-ttu-id="24be4-144">Virtuális gépek hello rendelkezésre állási készlet meg arról, hogy helyesen hello hardver vannak elosztva toomake belül kell létrehoznia.</span><span class="sxs-lookup"><span data-stu-id="24be4-144">VMs must be created within hello availability set toomake sure they are correctly distributed across hello hardware.</span></span> <span data-ttu-id="24be4-145">Meglévő virtuális gép tooan rendelkezésre állási készlet létrehozása után nem vehető fel.</span><span class="sxs-lookup"><span data-stu-id="24be4-145">You can't add an existing VM tooan availability set after it is created.</span></span> 

<span data-ttu-id="24be4-146">Amikor hoz létre egy virtuális gép használatával [az virtuális gép létrehozása](/cli/azure/vm#create) hello a rendelkezésre állási csoportban hello segítségével megadhat `--availability-set` paraméter toospecify hello hello rendelkezésre állási készlet nevét.</span><span class="sxs-lookup"><span data-stu-id="24be4-146">When you create a VM using [az vm create](/cli/azure/vm#create) you specify hello availability set using hello `--availability-set` parameter toospecify hello name of hello availability set.</span></span>

```azurecli-interactive 
for i in `seq 1 2`; do
   az vm create \
     --resource-group myResourceGroupAvailability \
     --name myVM$i \
     --availability-set myAvailabilitySet \
     --nics myNic$i \
     --size Standard_DS1_v2  \
     --image Canonical:UbuntuServer:14.04.4-LTS:latest \
     --admin-username azureuser \
     --generate-ssh-keys \
     --no-wait
done 
```

<span data-ttu-id="24be4-147">Most már tudunk két virtuális gép belül az újonnan létrehozott rendelkezésre állási csoportot.</span><span class="sxs-lookup"><span data-stu-id="24be4-147">We now have two virtual machines within our newly created availability set.</span></span> <span data-ttu-id="24be4-148">Mivel azokra hello azonos rendelkezésre állási csoport Azure biztosítja az adott hello virtuális gépek és az összes az erőforrások (beleértve a adatlemezek) különítve fizikai hardver különböző pontjain.</span><span class="sxs-lookup"><span data-stu-id="24be4-148">Because they are in hello same availability set, Azure will ensure that hello VMs and all their resources (including data disks) are distributed across isolated physical hardware.</span></span> <span data-ttu-id="24be4-149">Ehhez a terjesztéshez biztosíthatja, hogy mekkora a teljes méretű megoldás magas rendelkezésre állás érdekében.</span><span class="sxs-lookup"><span data-stu-id="24be4-149">This distribution helps ensure much higher availability of our overall VM solution.</span></span>

<span data-ttu-id="24be4-150">Virtuális gépek hozzáadásakor merülhetnek egyetlen művelet, győződjön meg arról, hogy egy adott virtuális gép méretét már nem elérhető toouse a rendelkezésre állási csoport belül.</span><span class="sxs-lookup"><span data-stu-id="24be4-150">One thing you may encounter as you add VMs is that a particular VM size is no longer available toouse within your availability set.</span></span> <span data-ttu-id="24be4-151">A probléma akkor fordulhat elő, ha már nincs elég kapacitás tooadd el hello elkülönítési szabályok hello rendelkezésre állási csoport adatainak megőrzése mellett.</span><span class="sxs-lookup"><span data-stu-id="24be4-151">This issue can happen if there is no longer enough capacity tooadd it while preserving hello isolation rules hello availability set enforces.</span></span> <span data-ttu-id="24be4-152">Ellenőrizheti toosee milyen Virtuálisgép-méretek meglévő rendelkezésre állási készlet használatával hello belül elérhető toouse `--availability-set list-sizes` paraméter.</span><span class="sxs-lookup"><span data-stu-id="24be4-152">You can check toosee what VM sizes are available toouse within an existing availability set using hello `--availability-set list-sizes` parameter.</span></span>

## <a name="check-for-available-vm-sizes"></a><span data-ttu-id="24be4-153">Ellenőrizze az elérhető Virtuálisgép-méretek</span><span class="sxs-lookup"><span data-stu-id="24be4-153">Check for available VM sizes</span></span> 

<span data-ttu-id="24be4-154">További virtuális gépek toohello rendelkezésre állási csoportban később adhat hozzá, de kell tooknow milyen Virtuálisgép-méretek hello hardveren érhetők el.</span><span class="sxs-lookup"><span data-stu-id="24be4-154">You can add more VMs toohello availability set later, but you need tooknow what VM sizes are available on hello hardware.</span></span> <span data-ttu-id="24be4-155">Használjon [az virtuális gép rendelkezésre állási-készlet lista-méretek](/cli/azure/availability-set#list-sizes) toolist hello rendelkezésre állási csoport fürt összes hello elérhető méretek hello hardveren.</span><span class="sxs-lookup"><span data-stu-id="24be4-155">Use [az vm availability-set list-sizes](/cli/azure/availability-set#list-sizes) toolist all hello available sizes on hello hardware cluster for hello availability set.</span></span>

```azurecli-interactive 
az vm availability-set list-sizes \
     --resource-group myResourceGroupAvailability \
     --name myAvailabilitySet \
     --output table  
```

## <a name="next-steps"></a><span data-ttu-id="24be4-156">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="24be4-156">Next steps</span></span>

<span data-ttu-id="24be4-157">Ez az oktatóanyag bemutatta, hogyan végezheti el az alábbi műveleteket:</span><span class="sxs-lookup"><span data-stu-id="24be4-157">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="24be4-158">Rendelkezésre állási csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="24be4-158">Create an availability set</span></span>
> * <span data-ttu-id="24be4-159">Hozzon létre egy virtuális gép rendelkezésre állási csoportba</span><span class="sxs-lookup"><span data-stu-id="24be4-159">Create a VM in an availability set</span></span>
> * <span data-ttu-id="24be4-160">Ellenőrizze a rendelkezésre álló Virtuálisgép-méretek</span><span class="sxs-lookup"><span data-stu-id="24be4-160">Check available VM sizes</span></span>

<span data-ttu-id="24be4-161">Előzetes toohello oktatóanyag következő toolearn kapcsolatos virtuálisgép-méretezési készlet.</span><span class="sxs-lookup"><span data-stu-id="24be4-161">Advance toohello next tutorial toolearn about virtual machine scale sets.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="24be4-162">Virtuálisgép-méretezési csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="24be4-162">Create a VM scale set</span></span>](tutorial-create-vmss.md)

