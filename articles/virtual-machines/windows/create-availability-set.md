---
title: "a virtuális gép rendelkezésre állási aaaCreate beállítása az Azure-ban |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy felügyelt rendelkezésre állási beállítása, vagy nem felügyelt rendelkezésre állási csoportot a virtuális gépek Azure PowerShell használatával, vagy hello portal hello Resource Manager üzembe helyezési modellben."
keywords: "A rendelkezésre állási csoport"
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: a3db8659-ace8-4e78-8b8c-1e75c04c042c
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: eadcdfcd28bb2fa21a4647f207b390c33e022ef1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="increase-vm-availability-by-creating-an-azure-availability-set"></a><span data-ttu-id="867c6-104">Növelje virtuális gép rendelkezésre állása Azure rendelkezésre állási csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="867c6-104">Increase VM availability by creating an Azure availability set</span></span> 
<span data-ttu-id="867c6-105">Rendelkezésre állási készletek biztosítanak redundanciát tooyour alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="867c6-105">Availability sets provide redundancy tooyour application.</span></span> <span data-ttu-id="867c6-106">Javasoljuk, hogy legalább két virtuális gép rendelkezésre állási csoportba.</span><span class="sxs-lookup"><span data-stu-id="867c6-106">We recommend that you group two or more virtual machines in an availability set.</span></span> <span data-ttu-id="867c6-107">Ez a konfiguráció biztosítja, hogy vagy a tervezett vagy nem tervezett karbantartási események esetén legalább egy virtuális gép lesz elérhető és igazodhat hello 99,95 % Azure SLA-t.</span><span class="sxs-lookup"><span data-stu-id="867c6-107">This configuration ensures that during either a planned or unplanned maintenance event, at least one virtual machine will be available and meet hello 99.95% Azure SLA.</span></span> <span data-ttu-id="867c6-108">További információkért lásd: hello [SLA-t a virtuális gépek](https://azure.microsoft.com/support/legal/sla/virtual-machines/).</span><span class="sxs-lookup"><span data-stu-id="867c6-108">For more information, see hello [SLA for Virtual Machines](https://azure.microsoft.com/support/legal/sla/virtual-machines/).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="867c6-109">Virtuális gépek kell létrehozni hello azonos erőforráscsoporthoz tartozik, mint hello rendelkezésre állási csoportot.</span><span class="sxs-lookup"><span data-stu-id="867c6-109">VMs must be created in hello same resource group as hello availability set.</span></span>
> 

<span data-ttu-id="867c6-110">Ha azt szeretné, hogy a virtuális gép toobe részei egy rendelkezésre állási csoportnak, szükség van-e toocreate hello rendelkezésre állási beállítása első, vagy ha létre az első virtuális gép hello készletében.</span><span class="sxs-lookup"><span data-stu-id="867c6-110">If you want your VM toobe part of an availability set, you need toocreate hello availability set first or while you are creating your first VM in hello set.</span></span> <span data-ttu-id="867c6-111">A virtuális Gépet felügyelt lemezek fog használni, ha egy felügyelt rendelkezésre állási csoportot, hello rendelkezésre állási csoportot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="867c6-111">If your VM will be using Managed Disks, hello availability set must be created as a managed availability set.</span></span>

<span data-ttu-id="867c6-112">Létrehozásával és a rendelkezésre állási csoportokkal kapcsolatos további információkért lásd: [hello virtuális gépek rendelkezésre állásának kezelése](manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="867c6-112">For more information about creating and using availability sets, see [Manage hello availability of virtual machines](manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

## <a name="use-hello-portal-toocreate-an-availability-set-before-creating-your-vms"></a><span data-ttu-id="867c6-113">Hello portál toocreate rendelkezésre állási készlet létrehozása a virtuális gépek előtt használata</span><span class="sxs-lookup"><span data-stu-id="867c6-113">Use hello portal toocreate an availability set before creating your VMs</span></span>
1. <span data-ttu-id="867c6-114">Hello menüben kattintson **Tallózás** válassza **rendelkezésre állási készletek**.</span><span class="sxs-lookup"><span data-stu-id="867c6-114">In hello hub menu, click **Browse** and select **Availability sets**.</span></span>
2. <span data-ttu-id="867c6-115">A hello **rendelkezésre állási készletek panel**, kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="867c6-115">On hello **Availability sets blade**, click **Add**.</span></span>
   
    ![Képernyőkép a hello hozzáadása egy új rendelkezésre állási csoport létrehozása gombra.](./media/create-availability-set/add-availability-set.png)
3. <span data-ttu-id="867c6-117">A hello **rendelkezésre állási csoport létrehozása** panelen, a készlet teljes hello adatait.</span><span class="sxs-lookup"><span data-stu-id="867c6-117">On hello **Create availability set** blade, complete hello information for your set.</span></span>
   
    ![Képernyőfelvétel, hogy látható hello tooenter toocreate hello rendelkezésre állási szükséges információk beállítása.](./media/create-availability-set/create-availability-set.png)
   
   * <span data-ttu-id="867c6-119">**Név** -hello név számokat, betűket, pontokat, aláhúzásjeleket és kötőjeleket áll 1 – 80 karakter lehet.</span><span class="sxs-lookup"><span data-stu-id="867c6-119">**Name** - hello name should be 1-80 characters made up of numbers, letters, periods, underscores and dashes.</span></span> <span data-ttu-id="867c6-120">hello első karakterének betűnek vagy számnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="867c6-120">hello first character must be a letter or number.</span></span> <span data-ttu-id="867c6-121">hello utolsó karakter betű, szám vagy aláhúzásjel kell lennie.</span><span class="sxs-lookup"><span data-stu-id="867c6-121">hello last character must be a letter, number or underscore.</span></span>
   * <span data-ttu-id="867c6-122">**Tartományok fault** -tartalék tartományok definiálása hello csoport a virtuális gépek, amelyek egy közös power forrás- és a hálózati kapcsolóhoz.</span><span class="sxs-lookup"><span data-stu-id="867c6-122">**Fault domains** - fault domains define hello group of virtual machines that share a common power source and network switch.</span></span> <span data-ttu-id="867c6-123">Alapértelmezés szerint a hello virtuális gépek mentése toothree tartalék tartományok között egymástól, és a módosított toobetween 1. és 3 lehet.</span><span class="sxs-lookup"><span data-stu-id="867c6-123">By default, hello VMs  are separated across up toothree fault domains and can be changed toobetween 1 and 3.</span></span>
   * <span data-ttu-id="867c6-124">**Tartományok frissítése** - öt frissítési tartományok alapértelmezés szerint tartoznak, és ez 1 és 20 toobetween állítható.</span><span class="sxs-lookup"><span data-stu-id="867c6-124">**Update domains** -  five update domains are assigned by default and this can be set toobetween 1 and 20.</span></span> <span data-ttu-id="867c6-125">Frissítési tartományok jelzi a virtuális gépek és a mögöttes fizikai hardver, hogy újra kell indítani a hello csoportok ugyanannyi időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="867c6-125">Update domains indicate groups of virtual machines and underlying physical hardware that can be rebooted at hello same time.</span></span> <span data-ttu-id="867c6-126">Például ha azt adja meg a tartományokat, több mint öt virtuális gépek belül egy egyetlen rendelkezésre állási csoportban, hello hatodik virtuális gép konfigurálásakor bekerülnek öt frissítés hello hello első virtuális gépen, azonos frissítési tartományban hello hetedik a hello azonos UD hello második virtuális gép, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="867c6-126">For example, if we specify five update domains, when more than five virtual machines are configured within a single Availability Set, hello sixth virtual machine will be placed into hello same update domain as hello first virtual machine, hello seventh in hello same UD as hello second virtual machine, and so on.</span></span> <span data-ttu-id="867c6-127">hello újraindítások hello sorrendje nem lehet egymást követő, de csak egy frissítési tartományt újraindításáról egyszerre.</span><span class="sxs-lookup"><span data-stu-id="867c6-127">hello order of hello reboots may not be sequential, but only one update domain will be rebooted at a time.</span></span>
   * <span data-ttu-id="867c6-128">**Előfizetés** -válassza hello előfizetés toouse, ha több van.</span><span class="sxs-lookup"><span data-stu-id="867c6-128">**Subscription** - select hello subscription toouse if you have more than one.</span></span>
   * <span data-ttu-id="867c6-129">**Erőforráscsoport** -válasszon egy meglévő erőforráscsoportot hello mutató nyílra, majd válassza az erőforráscsoport hello a legördülő listán.</span><span class="sxs-lookup"><span data-stu-id="867c6-129">**Resource group** - select an existing resource group by clicking hello arrow and selecting a resource group from hello drop down.</span></span> <span data-ttu-id="867c6-130">Írja be a nevet is létrehozhat egy új erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="867c6-130">You can also create a new resource group by typing in a name.</span></span> <span data-ttu-id="867c6-131">hello neve is tartalmazhatja hello a következő karaktereket: betűket, számokat, pontokat, kötőjeleket, aláhúzásjeleket és nyitó vagy záró zárójel.</span><span class="sxs-lookup"><span data-stu-id="867c6-131">hello name can contain any of hello following characters: letters, numbers, periods, dashes, underscores and opening or closing parenthesis.</span></span> <span data-ttu-id="867c6-132">hello neve nem végződhet ponttal.</span><span class="sxs-lookup"><span data-stu-id="867c6-132">hello name cannot end in a period.</span></span> <span data-ttu-id="867c6-133">Összes hello rendelkezésre állási csoportban lévő virtuális gépek hello kell hello létrehozott toobe erőforráscsoportjában hello rendelkezésre állási csoportot.</span><span class="sxs-lookup"><span data-stu-id="867c6-133">All of hello VMs in hello availability group need toobe created in hello same resource group as hello availability set.</span></span>
   * <span data-ttu-id="867c6-134">**Hely** -a hello legördülő listán jelöljön ki egy helyet.</span><span class="sxs-lookup"><span data-stu-id="867c6-134">**Location** - select a location from hello drop-down.</span></span>
   * <span data-ttu-id="867c6-135">**Felügyelt** – Itt adhatja meg *Igen* toocreate egy felügyelt rendelkezésre állási beállítása toouse virtuális gépek, amelyek a felügyelt lemezeket használjanak tárhelyként.</span><span class="sxs-lookup"><span data-stu-id="867c6-135">**Managed** - select *Yes* toocreate a managed availability set toouse with VMs that use Managed Disks for storage.</span></span> <span data-ttu-id="867c6-136">Válassza ki **nem** Ha hello hello beállítása lévő virtuális gépek a tárfiók nem felügyelt lemezek használhatók.</span><span class="sxs-lookup"><span data-stu-id="867c6-136">Select **No** if hello VMs that will be in hello set use unmanaged disks in a storage account.</span></span>
   
4. <span data-ttu-id="867c6-137">Ha befejezte az adatok hello megadásával, kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="867c6-137">When you are done entering hello information, click **Create**.</span></span> 

## <a name="use-hello-portal-toocreate-a-virtual-machine-and-an-availability-set-at-hello-same-time"></a><span data-ttu-id="867c6-138">Használja a virtuális gépek és a rendelkezésre állási beállított hello azonos hello portál toocreate idő</span><span class="sxs-lookup"><span data-stu-id="867c6-138">Use hello portal toocreate a virtual machine and an availability set at hello same time</span></span>
<span data-ttu-id="867c6-139">Hello portál segítségével új virtuális gép létrehozásakor is létrehozhat egy új rendelkezésre állási hello létrehozása során a virtuális gép hello beállítása hello beállítása az első virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="867c6-139">If you are creating a new VM using hello portal, you can also create a new availability set for hello VM while you create hello first VM in hello set.</span></span> <span data-ttu-id="867c6-140">Ha a virtuális gép toouse felügyelt lemezeket, egy felügyelt rendelkezésre állási csoport jön létre.</span><span class="sxs-lookup"><span data-stu-id="867c6-140">If you choose toouse Managed Disks for your VM, a managed availability set will be created.</span></span>

![Képernyőkép a hello folyamat létrehozásához egy új rendelkezésre állási beállítása hello virtuális gép létrehozása során.](./media/create-availability-set/new-vm-avail-set.png)

## <a name="add-a-new-vm-tooan-existing-availability-set-in-hello-portal"></a><span data-ttu-id="867c6-142">Adja hozzá egy új virtuális gép tooan meglévő rendelkezésre állási csoport hello portál</span><span class="sxs-lookup"><span data-stu-id="867c6-142">Add a new VM tooan existing availability set in hello portal</span></span>
<span data-ttu-id="867c6-143">Az egyes további virtuális gépek létrehozásakor kell hello készlet tartozik, győződjön meg arról, hogy létrehozta a hello azonos **erőforráscsoport** és majd a válassza ki a 3. lépésben beállítása rendelkezésre állási hello.</span><span class="sxs-lookup"><span data-stu-id="867c6-143">For each additional VM that you create that should belong in hello set, make sure that you create it in hello same **resource group** and then select hello existing availability set in Step 3.</span></span> 

![Képernyőfelvétel: hogyan tooselect rendelkezésre állási toouse beállítani a virtuális Gépet.](./media/create-availability-set/add-vm-to-set.png)

## <a name="use-powershell-toocreate-an-availability-set"></a><span data-ttu-id="867c6-145">Használjon PowerShell toocreate rendelkezésre állási beállítása</span><span class="sxs-lookup"><span data-stu-id="867c6-145">Use PowerShell toocreate an availability set</span></span>
<span data-ttu-id="867c6-146">Ebben a példában hoz létre a rendelkezésre állási készlet elnevezett **myAvailabilitySet** a hello **myResourceGroup** hello erőforráscsoportja **USA nyugati régiója** helyét.</span><span class="sxs-lookup"><span data-stu-id="867c6-146">This example creates an availability set named **myAvailabilitySet** in hello **myResourceGroup** resource group in hello **West US** location.</span></span> <span data-ttu-id="867c6-147">Toobe hello létrehozása előtt meg kell első virtuális gép, amely hello készlet lesz.</span><span class="sxs-lookup"><span data-stu-id="867c6-147">This needs toobe done before you create hello first VM that will be in hello set.</span></span>

<span data-ttu-id="867c6-148">Mielőtt elkezdené, győződjön meg arról, hogy rendelkezik-e hello hello AzureRM.Compute PowerShell modul legújabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="867c6-148">Before you begin, make sure that you have hello latest version of hello AzureRM.Compute PowerShell module.</span></span> <span data-ttu-id="867c6-149">Futtatás hello parancs tooinstall követően.</span><span class="sxs-lookup"><span data-stu-id="867c6-149">Run hello following command tooinstall it.</span></span>

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
<span data-ttu-id="867c6-150">További információkért lásd: [Azure PowerShell Versioning](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="867c6-150">For more information, see [Azure PowerShell Versioning](/powershell/azure/overview).</span></span>


<span data-ttu-id="867c6-151">Ha a virtuális gépek felügyelt lemezt használ, írja be:</span><span class="sxs-lookup"><span data-stu-id="867c6-151">If you are using managed disks for your VMs, type:</span></span>

```powershell
    New-AzureRmAvailabilitySet -ResourceGroupName "myResourceGroup" '
    -Name "myAvailabilitySet" -Location "West US" -managed
```

<span data-ttu-id="867c6-152">Ha a virtuális gépek használ a saját storage-fiókok, írja be:</span><span class="sxs-lookup"><span data-stu-id="867c6-152">If you are using your own storage accounts for your VMs, type:</span></span>

```powershell
    New-AzureRmAvailabilitySet -ResourceGroupName "myResourceGroup" '
    -Name "myAvailabilitySet" -Location "West US" 
```

<span data-ttu-id="867c6-153">További információkért lásd: [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset).</span><span class="sxs-lookup"><span data-stu-id="867c6-153">For more information, see [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="867c6-154">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="867c6-154">Troubleshooting</span></span>
* <span data-ttu-id="867c6-155">Amikor létrehoz egy virtuális Gépet, ha nem kívánt hello rendelkezésre állási csoport hello portálon hello legördülő listában, létrehozott azt egy másik erőforráscsoportban található.</span><span class="sxs-lookup"><span data-stu-id="867c6-155">When you create a VM, if hello availability set you want isn't in hello drop-down list in hello portal you may have created it in a different resource group.</span></span> <span data-ttu-id="867c6-156">Ha nem tudja hello erőforráscsoportot a rendelkezésre állási beállítása, nyissa meg toohello központ menü, és kattintson a Tallózás > rendelkezésre állási készletek toosee beállítja a rendelkezésre állási listáját, és mely erőforrás-csoportok tartoznak.</span><span class="sxs-lookup"><span data-stu-id="867c6-156">If you don't know hello resource group for your availability set, go toohello hub menu and click Browse > Availability sets toosee a list of your availability sets and which resource groups they belong to.</span></span>

## <a name="next-steps"></a><span data-ttu-id="867c6-157">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="867c6-157">Next steps</span></span>
<span data-ttu-id="867c6-158">Adja hozzá a további tárhely tooyour VM további hozzáadásával [adatlemez](attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="867c6-158">Add additional storage tooyour VM by adding an additional [data disk](attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

