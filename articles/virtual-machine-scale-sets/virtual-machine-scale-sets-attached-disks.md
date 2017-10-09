---
title: "Virtuális gép méretezési készletek csatolt adatlemezek aaaAzure |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse csatolt adatok lemez is van a virtuálisgép-méretezési csoportok"
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 4/25/2017
ms.author: guybo
ms.openlocfilehash: 77b66f80934c0aaf7bb1ad0de00a738052a878ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-vm-scale-sets-and-attached-data-disks"></a><span data-ttu-id="40dea-103">Azure-beli virtuálisgép-méretezési csoportok és csatlakoztatott adatlemezek</span><span class="sxs-lookup"><span data-stu-id="40dea-103">Azure VM scale sets and attached data disks</span></span>
<span data-ttu-id="40dea-104">Az Azure-beli [virtuálisgép-méretezési csoportok](/azure/virtual-machine-scale-sets/) mostantól támogatják a csatlakoztatott adatlemezekkel rendelkező virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="40dea-104">Azure [virtual machine scale sets](/azure/virtual-machine-scale-sets/) now support virtual machines with attached data disks.</span></span> <span data-ttu-id="40dea-105">Az adatlemezek hello tárolási profilban Azure felügyelt lemezekkel létrehozott méretezési csoportok definiálhatók.</span><span class="sxs-lookup"><span data-stu-id="40dea-105">Data disks can be defined in hello storage profile for scale sets that have been created with Azure Managed Disks.</span></span> <span data-ttu-id="40dea-106">Korábban hello érhető el virtuális gépek méretezési csoportok csak közvetlenül csatlakoztatott tárolók beállítások lettek hello operációsrendszer-lemezek vagy ideiglenes meghajtók.</span><span class="sxs-lookup"><span data-stu-id="40dea-106">Previously hello only directly attached storage options available with VMs in scale sets were hello OS drive and temp drives.</span></span>

> [!NOTE]
>  <span data-ttu-id="40dea-107">Amikor létrehoz egy méretezési definiált csatolt adatlemezekkel rendelkező készletben, toomount továbbra is szükséges és formátum hello lemezek a belül egy virtuális gép toouse őket (az önálló Azure virtuális gépek csak hasonló).</span><span class="sxs-lookup"><span data-stu-id="40dea-107">When you create a scale set with attached data disks defined, you still need toomount and format hello disks from within a VM toouse them (just like for standalone Azure VMs).</span></span> <span data-ttu-id="40dea-108">Egy kényelmes módszert arra toodo toouse egy egyéni parancsfájl-kiterjesztésen, amely meghívja a szabványos parancsfájlként toopartition és formázza az összes hello adatok lemezt a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="40dea-108">A convenient way toodo this is toouse a custom script extension which calls a standard script toopartition and format all hello data disks on a VM.</span></span>

## <a name="create-a-scale-set-with-attached-data-disks"></a><span data-ttu-id="40dea-109">Csatlakoztatott adatlemezekkel rendelkező méretezési csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="40dea-109">Create a scale set with attached data disks</span></span>
<span data-ttu-id="40dea-110">Egy egyszerű módon toocreate egy méretezési csatlakoztatott lemezzel toouse hello [Azure CLI](https://github.com/Azure/azure-cli) _vmss létrehozása_ parancsot.</span><span class="sxs-lookup"><span data-stu-id="40dea-110">A simple way toocreate a scale set with attached disks is toouse hello [Azure CLI](https://github.com/Azure/azure-cli) _vmss create_ command.</span></span> <span data-ttu-id="40dea-111">a következő példa hello hoz létre egy Azure-erőforráscsoportot, és a Virtuálisgép-méretezési készlet 10 Ubuntu virtuális gépek, külön 2 csatolt adatlemezek, 50 GB-os és 100 GB-osztályban.</span><span class="sxs-lookup"><span data-stu-id="40dea-111">hello following example creates an Azure resource group, and a VM scale set of 10 Ubuntu VMs, each with 2 attached data disks, of 50 GB and 100 GB respectively.</span></span>
```bash
az group create -l southcentralus -n dsktest
az vmss create -g dsktest -n dskvmss --image ubuntults --instance-count 10 --data-disk-sizes-gb 50 100
```
<span data-ttu-id="40dea-112">Vegye figyelembe, hogy hello _vmss létrehozása_ parancs bizonyos konfigurációs értékeket alapértelmezés szerint, ha nem adja meg azokat.</span><span class="sxs-lookup"><span data-stu-id="40dea-112">Note that hello _vmss create_ command defaults certain configuration values if you do not specify them.</span></span> <span data-ttu-id="40dea-113">toosee hello elérhető lehetőségek, hogy próbálja meg lehet felülbírálni:</span><span class="sxs-lookup"><span data-stu-id="40dea-113">toosee hello available options that you can override try:</span></span>
```bash
az vmss create --help
```
<span data-ttu-id="40dea-114">Állítsa be a mellékelt adatok lemezek terjedő skálán toodefine egy méretezési Azure Resource Manager-sablon, egy másik módja toocreate közé tartozik egy _dataDisks_ hello szakasz _storageProfile_, és hello telepítése sablon.</span><span class="sxs-lookup"><span data-stu-id="40dea-114">Another way toocreate a scale set with attached data disks is toodefine a scale set in an Azure Resource Manager template, include a _dataDisks_ section in hello _storageProfile_, and deploy hello template.</span></span> <span data-ttu-id="40dea-115">hello 50 GB-os és 100 GB szabad fenti példa hello sablonban ehhez hasonló lenne meghatározott:</span><span class="sxs-lookup"><span data-stu-id="40dea-115">hello 50 GB and 100 GB disk example above would be defined like this in hello template:</span></span>
```json
"dataDisks": [
    {
    "lun": 1,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 50
    },
    {
    "lun": 2,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 100
    }
]
```
<span data-ttu-id="40dea-116">Egy csatlakoztatott lemezzel, az itt megadott egy teljes, készen áll a toodeploy például egy méretezési készlet sablon láthatja: [https://github.com/chagarw/MDPP/tree/master/101-vmss-os-data](https://github.com/chagarw/MDPP/tree/master/101-vmss-os-data).</span><span class="sxs-lookup"><span data-stu-id="40dea-116">You can see a complete, ready toodeploy example of a scale set template with an attached disk defined here: [https://github.com/chagarw/MDPP/tree/master/101-vmss-os-data](https://github.com/chagarw/MDPP/tree/master/101-vmss-os-data).</span></span>

## <a name="adding-a-data-disk-tooan-existing-scale-set"></a><span data-ttu-id="40dea-117">Adatok lemez tooan meglévő méretezési hozzáadása beállítása</span><span class="sxs-lookup"><span data-stu-id="40dea-117">Adding a data disk tooan existing scale set</span></span>
> [!NOTE]
>  <span data-ttu-id="40dea-118">Adatok lemezek tooa méretezési jött létre, amely csak csatolhat [Azure felügyelt lemezek](./virtual-machine-scale-sets-managed-disks.md).</span><span class="sxs-lookup"><span data-stu-id="40dea-118">You can only attach data disks tooa scale set which has been created with [Azure Managed Disks](./virtual-machine-scale-sets-managed-disks.md).</span></span>

<span data-ttu-id="40dea-119">Hozzáadhat egy adatok lemez tooa Virtuálisgép-méretezési beállítása az Azure parancssori felület használatával _az vmss lemez csatolása_ parancsot.</span><span class="sxs-lookup"><span data-stu-id="40dea-119">You can add a data disk tooa VM scale set using Azure CLI _az vmss disk attach_ command.</span></span> <span data-ttu-id="40dea-120">Győződjön meg arról, hogy olyan logikaiegység-számot ad meg, amely még nincs használatban.</span><span class="sxs-lookup"><span data-stu-id="40dea-120">Make sure you specify a lun which is not already in use.</span></span> <span data-ttu-id="40dea-121">a következő példa CLI hello ad hozzá egy 50 GB-os meghajtó toolun 3:</span><span class="sxs-lookup"><span data-stu-id="40dea-121">hello following CLI example adds a 50 GB drive toolun 3:</span></span>
```bash
az vmss disk attach -g dsktest -n dskvmss --size-gb 50 --lun 3
```

<span data-ttu-id="40dea-122">a következő PowerShell-példa hello ad hozzá egy 50 GB-os meghajtó toolun 3:</span><span class="sxs-lookup"><span data-stu-id="40dea-122">hello following PowerShell example adds a 50 GB drive toolun 3:</span></span>
```powershell
$vmss = Get-AzureRmVmss -ResourceGroupName myvmssrg -VMScaleSetName myvmss
$vmss = Add-AzureRmVmssDataDisk -VirtualMachineScaleSet $vmss -Lun 3 -Caching 'ReadWrite' -CreateOption Empty -DiskSizeGB 50 -StorageAccountType StandardLRS
Update-AzureRmVmss -ResourceGroupName myvmssrg -Name myvmss -VirtualMachineScaleSet $vmss
```

> [!NOTE]
> <span data-ttu-id="40dea-123">Másik Virtuálisgép-méretek hello számok csatlakoztatott meghajtók támogatják a különböző korlátokkal rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="40dea-123">Different VM sizes have different limits on hello numbers of attached drives they support.</span></span> <span data-ttu-id="40dea-124">Ellenőrizze a hello [virtuális gép mérete alapján](../virtual-machines/windows/sizes.md) új lemez hozzáadása előtt.</span><span class="sxs-lookup"><span data-stu-id="40dea-124">Check hello [virtual machine size characteristics](../virtual-machines/windows/sizes.md) before adding a new disk.</span></span>

<span data-ttu-id="40dea-125">Egy lemezt hozzáad egy új belépési toohello hozzáadásával _dataDisks_ hello tulajdonság _storageProfile_ a skála állítsa be a definíció- és hello módosítás alkalmazása.</span><span class="sxs-lookup"><span data-stu-id="40dea-125">You can also add a disk by adding a new entry toohello _dataDisks_ property in hello _storageProfile_ of a scale set definition and applying hello change.</span></span> <span data-ttu-id="40dea-126">tootest, a keresés hello egy meglévő méretezési készlet definíciója [Azure erőforrás-kezelő](https://resources.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="40dea-126">tootest this, find an existing scale set definition in hello [Azure Resource Explorer](https://resources.azure.com/).</span></span> <span data-ttu-id="40dea-127">Válassza ki _szerkesztése_ , és adja hozzá egy új lemezt toohello listájának beolvasása.</span><span class="sxs-lookup"><span data-stu-id="40dea-127">Select _Edit_ and add a new disk toohello list of data disks.</span></span> <span data-ttu-id="40dea-128">Például</span><span class="sxs-lookup"><span data-stu-id="40dea-128">E.g.</span></span> <span data-ttu-id="40dea-129">a fenti hello-példáját:</span><span class="sxs-lookup"><span data-stu-id="40dea-129">using hello example above:</span></span>
```json
"dataDisks": [
    {
    "lun": 1,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 50
    },
    {
    "lun": 2,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 100
    },
    {
    "lun": 3,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 20
    }          
]
```

<span data-ttu-id="40dea-130">Válassza ki _PUT_ tooapply hello tooyour méretezési változik.</span><span class="sxs-lookup"><span data-stu-id="40dea-130">Then select _PUT_ tooapply hello changes tooyour scale set.</span></span> <span data-ttu-id="40dea-131">Ez a példa akkor működik, ha a használt virtuális gép mérete kettőnél több csatlakoztatott adatlemezt támogat.</span><span class="sxs-lookup"><span data-stu-id="40dea-131">This example would work as long as you are using a VM size which supports more than two attached data disks.</span></span>

> [!NOTE]
> <span data-ttu-id="40dea-132">Amikor a módosítás tooa méretezési definition például hozzáadásával vagy eltávolításával adatlemezt, tooall az újonnan létrehozott virtuális gépek vonatkozik, de tooexisting virtuális gépek csak akkor, ha hello _upgradePolicy_ tulajdonsága túl "automatikus".</span><span class="sxs-lookup"><span data-stu-id="40dea-132">When you make a change tooa scale set definition such as adding or removing a data disk, it applies tooall newly created VMs, but only applies tooexisting VMs if hello _upgradePolicy_ property is set too"Automatic".</span></span> <span data-ttu-id="40dea-133">Ha túl "Manual" értéke, meg kell-e toomanually hello új modell tooexisting virtuális gépek alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="40dea-133">If it is set too"Manual", you need toomanually apply hello new model tooexisting VMs.</span></span> <span data-ttu-id="40dea-134">Ehhez a hello portál hello segítségével _frissítés-AzureRmVmssInstance_ PowerShell parancsot, vagy a hello segítségével _az vmss frissítés-példányokat_ CLI parancsot.</span><span class="sxs-lookup"><span data-stu-id="40dea-134">You can do this in hello portal, using hello _Update-AzureRmVmssInstance_ PowerShell command, or using hello _az vmss update-instances_ CLI command.</span></span>

## <a name="adding-pre-populated-data-disks-tooan-existent-scale-set"></a><span data-ttu-id="40dea-135">Előre megadott adatok lemezek tooan létező méretezési hozzáadása beállítása</span><span class="sxs-lookup"><span data-stu-id="40dea-135">Adding pre-populated data disks tooan existent scale set</span></span> 
> <span data-ttu-id="40dea-136">Lemezek tooan létező hozzáadásakor méretezési modell úgy lett kialakítva, hello lemez mindig készül üres.</span><span class="sxs-lookup"><span data-stu-id="40dea-136">When you add disks tooan existent scale set model, by design, hello disk will always be created empty.</span></span> <span data-ttu-id="40dea-137">Ebben a forgatókönyvben is hello méretezési készlet által létrehozott új példányok.</span><span class="sxs-lookup"><span data-stu-id="40dea-137">This scenario also includes new instances created by hello scale set.</span></span> <span data-ttu-id="40dea-138">Ez a viselkedés nem, mert hello scaleset van egy üres adatlemez.</span><span class="sxs-lookup"><span data-stu-id="40dea-138">This behaviour is because hello scaleset definition has an empty data disk.</span></span> <span data-ttu-id="40dea-139">Rendelés toocreate előre megadott adatmeghajtókon egy létező méretezési készlet modell, a következő két lehetőség közül választhat:</span><span class="sxs-lookup"><span data-stu-id="40dea-139">In order toocreate pre-populated data drives for an existent scale set model, you can choose either of next two options:</span></span>

* <span data-ttu-id="40dea-140">Adatok másolása az hello példányt 0 VM toohello adatok lemez(ek) hello a többi virtuális gép egy egyéni parancsfájl futtatásával.</span><span class="sxs-lookup"><span data-stu-id="40dea-140">Copy data from hello instance 0 VM toohello data disk(s) in hello other VMs by running a custom script.</span></span>
* <span data-ttu-id="40dea-141">Hozzon létre egy felügyelt képre hello operációsrendszer-lemez és adatlemez (hello szükséges adatokkal), és hozzon létre egy új scaleset hello kép.</span><span class="sxs-lookup"><span data-stu-id="40dea-141">Create a managed image with hello OS disk plus data disk (with hello required data) and create a new scaleset with hello image.</span></span> <span data-ttu-id="40dea-142">Így minden létrehozott új virtuális Gépet fog rendelkezni az adatok lemezre hello scaleset hello definíciójában megadott.</span><span class="sxs-lookup"><span data-stu-id="40dea-142">This way every new VM created will have a data disk that that is provided in hello definition of hello scaleset.</span></span> <span data-ttu-id="40dea-143">Mivel ez a definíció hivatkozhatnak, amely rendelkezik a testreszabott adatlemezt tooan rendszerképének hello scaleset minden virtuális gép lesz automatikusan kapja meg ezeket a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="40dea-143">Since this definition will refer tooan image with a data disk that has customized data, every virtual machine on hello scaleset will automatically come up with these changes.</span></span>

> <span data-ttu-id="40dea-144">hello módon toocreate egyéni lemezkép itt található: [egy felügyelt képre egy általánosított virtuális gép létrehozása az Azure-ban](/azure/virtual-machines/windows/capture-image-resource/)</span><span class="sxs-lookup"><span data-stu-id="40dea-144">hello way toocreate a custom image can be found here: [Create a managed image of a generalized VM in Azure](/azure/virtual-machines/windows/capture-image-resource/)</span></span> 

> <span data-ttu-id="40dea-145">hello felhasználó számára szükséges toocapture hello 0 virtuális gép, amely rendelkezik a szükséges adatok hello példányt, és ezután használja az adott vhd hello kép definíciója.</span><span class="sxs-lookup"><span data-stu-id="40dea-145">hello user needs toocapture hello instance 0 VM which has hello required data, and then use that vhd for hello image definition.</span></span>

## <a name="removing-a-data-disk-from-a-scale-set"></a><span data-ttu-id="40dea-146">Adatlemez eltávolítása méretezési csoportból</span><span class="sxs-lookup"><span data-stu-id="40dea-146">Removing a data disk from a scale set</span></span>
<span data-ttu-id="40dea-147">Adatlemez az Azure parancssori felület _az vmss disk detach_ parancsával távolítható el a virtuálisgép-méretezési csoportokból.</span><span class="sxs-lookup"><span data-stu-id="40dea-147">You can remove a data disk from a VM scale set using Azure CLI _az vmss disk detach_ command.</span></span> <span data-ttu-id="40dea-148">Például hello parancsban hello lemez eltávolítása a lun 2 van megadva:</span><span class="sxs-lookup"><span data-stu-id="40dea-148">For example hello following command removes hello disk defined at lun 2:</span></span>
```bash
az vmss disk detach -g dsktest -n dskvmss --lun 2
```  
<span data-ttu-id="40dea-149">Hasonló módon is eltávolíthat egy lemezt egy bejegyzés eltávolítása hello által beállított méretezési _dataDisks_ hello tulajdonság _storageProfile_ és hello módosítás alkalmazása.</span><span class="sxs-lookup"><span data-stu-id="40dea-149">Similarly you can also remove a disk from a scale set by removing an entry from hello _dataDisks_ property in hello _storageProfile_ and applying hello change.</span></span> 

## <a name="additional-notes"></a><span data-ttu-id="40dea-150">További megjegyzések</span><span class="sxs-lookup"><span data-stu-id="40dea-150">Additional notes</span></span>
<span data-ttu-id="40dea-151">Az Azure által kezelt lemezek és a skála állítsa a mellékelt adatok lemezek is elérhetők az API verzió [ _2016-04-30-előzetes_ ](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-compute/2016-04-30-preview/swagger/compute.json) vagy újabb hello Microsoft.Compute API.</span><span class="sxs-lookup"><span data-stu-id="40dea-151">Support for Azure Managed disks and scale set attached data disks is available in API version [_2016-04-30-preview_](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-compute/2016-04-30-preview/swagger/compute.json) or later of hello Microsoft.Compute API.</span></span>

<span data-ttu-id="40dea-152">Hello kezdeti végrehajtásának méretezési csoportok csatlakoztatott lemezre támogatása nem csatolása vagy leválasztása adatlemezek méretezési csoportban lévő egyes virtuális gépek és a.</span><span class="sxs-lookup"><span data-stu-id="40dea-152">In hello initial implementation of attached disk support for scale sets, you cannot attach or detach data disks to/from individual VMs in a scale set.</span></span>

<span data-ttu-id="40dea-153">Az Azure Portal kezdetben csak korlátozott támogatást biztosít a méretezési csoportok csatlakoztatott adatlemezei számára.</span><span class="sxs-lookup"><span data-stu-id="40dea-153">Azure portal support for attached data disks in scale sets is initially limited.</span></span> <span data-ttu-id="40dea-154">Azure-sablonok is használhatja a követelményeitől függően a parancssori felület, a PowerShell, az SDK-k és a REST API toomanage csatlakoztatott lemezekkel.</span><span class="sxs-lookup"><span data-stu-id="40dea-154">Depending on your requirements you can use Azure templates, CLI, PowerShell, SDKs, and REST API toomanage attached disks.</span></span>


