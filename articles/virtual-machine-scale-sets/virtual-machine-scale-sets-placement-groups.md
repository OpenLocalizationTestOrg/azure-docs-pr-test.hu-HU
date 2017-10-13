---
title: "a nagy Azure virtuálisgép-méretezési csoportok aaaWorking |} Microsoft Docs"
description: "Mit kell tooknow toouse nagyméretű Azure virtuális gép skálázása beállítása"
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
ms.date: 2/7/2017
ms.author: guybo
ms.openlocfilehash: a39aab25925d7fc50763f0a20148b1f2213b492f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="working-with-large-virtual-machine-scale-sets"></a><span data-ttu-id="97a41-103">Nagyméretű virtuálisgép-méretezési csoportok használata</span><span class="sxs-lookup"><span data-stu-id="97a41-103">Working with large virtual machine scale sets</span></span>
<span data-ttu-id="97a41-104">Mostantól létrehozhat Azure [virtuálisgép-méretezési csoportok](/azure/virtual-machine-scale-sets/) too1, 000 virtuális gép mentése kapacitással.</span><span class="sxs-lookup"><span data-stu-id="97a41-104">You can now create Azure [virtual machine scale sets](/azure/virtual-machine-scale-sets/) with a capacity of up too1,000 VMs.</span></span> <span data-ttu-id="97a41-105">A jelen dokumentum egy _nagy virtuálisgép-méretezési csoport_ egy méretezési készletben képes mint 100 virtuális gépek toogreater skálázás típusúként van definiálva.</span><span class="sxs-lookup"><span data-stu-id="97a41-105">In this document, a _large virtual machine scale set_ is defined as a scale set capable of scaling toogreater than 100 VMs.</span></span> <span data-ttu-id="97a41-106">Ezt a képességet a méretezési csoport egyik tulajdonsága adja meg (_singlePlacementGroup=False_).</span><span class="sxs-lookup"><span data-stu-id="97a41-106">This capability is set by a scale set property (_singlePlacementGroup=False_).</span></span> 

<span data-ttu-id="97a41-107">Nagy méretű bizonyos elemeinek állítja be, például a terhelést és a tartalék tartományok eltérően viselkednek tooa szabványos méretezési készlet.</span><span class="sxs-lookup"><span data-stu-id="97a41-107">Certain aspects of large scale sets, such as load balancing and fault domains behave differently tooa standard scale set.</span></span> <span data-ttu-id="97a41-108">Ez a dokumentum nagy méretű méretezési csoportok hello jellemzőit ismerteti, és leírja, milyen kell tooknow toosuccessfully használhatja azokat az alkalmazásokban.</span><span class="sxs-lookup"><span data-stu-id="97a41-108">This document explains hello characteristics of large scale sets, and describes what you need tooknow toosuccessfully use them in your applications.</span></span> 

<span data-ttu-id="97a41-109">Nagy léptékű felhőalapú infrastruktúra üzembe helyezéséhez egy általánosan használt megközelítés olyan toocreate _skálázási egységek_, például hozzon létre több virtuális gépek méretezési készletek több Vnetek és a storage-fiókok.</span><span class="sxs-lookup"><span data-stu-id="97a41-109">A common approach for deploying cloud infrastructure at large scale is toocreate a set of _scale units_, for example by creating multiple VMs scale sets across multiple VNETs and storage accounts.</span></span> <span data-ttu-id="97a41-110">Ez a megközelítés biztosítja egyszerűbb felügyeleti képest toosingle virtuális gépek, és hasznos a számos olyan alkalmazás, különösképpen más rakatolható összetevők, például a virtuális hálózatok és a végpontok igénylő több méretezési egységeket.</span><span class="sxs-lookup"><span data-stu-id="97a41-110">This approach provides easier management compared toosingle VMs, and multiple scale units are useful for many applications, particularly those that require other stackable components like multiple virtual networks and endpoints.</span></span> <span data-ttu-id="97a41-111">Ha az alkalmazás egy nagy fürtön azonban, célszerű egyetlen méretezési beállítása a too1, 000 virtuális gépek egyszerűbb toodeploy.</span><span class="sxs-lookup"><span data-stu-id="97a41-111">If your application requires a single large cluster however, it can be more straightforward toodeploy a single scale set of up too1,000 VMs.</span></span> <span data-ttu-id="97a41-112">A példaforgatókönyvek központosított big data-alapú üzemelő példányokat, vagy a munkavégző csomópontok nagy készleteinek egyszerű kezelését igénylő számítási grideket tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="97a41-112">Example scenarios include centralized big data deployments, or compute grids requiring simple management of a large pool of worker nodes.</span></span> <span data-ttu-id="97a41-113">Virtuálisgép-méretezési készlet együtt [adatlemezt csatolni](virtual-machine-scale-sets-attached-disks.md), nagy méretű méretezési csoportok lehetővé teszik a toodeploy egy skálázható infrastruktúrája magok ezer és petabájt is, mint egyetlen műveletben.</span><span class="sxs-lookup"><span data-stu-id="97a41-113">Combined with VM scale set [attached data disks](virtual-machine-scale-sets-attached-disks.md), large scale sets enable you toodeploy a scalable infrastructure consisting of thousands of cores and petabytes of storage, as a single operation.</span></span>

## <a name="placement-groups"></a><span data-ttu-id="97a41-114">Elhelyezési csoportok</span><span class="sxs-lookup"><span data-stu-id="97a41-114">Placement groups</span></span> 
<span data-ttu-id="97a41-115">Milyen részekből egy _nagy_ méretezési készletben különleges nincs hello virtuális gépeinek számával, hanem hello száma _elhelyezési csoportok_ tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="97a41-115">What makes a _large_ scale set special is not hello number of VMs, but hello number of _placement groups_ it contains.</span></span> <span data-ttu-id="97a41-116">Elhelyezési csoport olyan szerkezet hasonló tooan Azure rendelkezésre állási, a saját tartalék tartományok és a frissítési tartományok.</span><span class="sxs-lookup"><span data-stu-id="97a41-116">A placement group is a construct similar tooan Azure availability set, with its own fault domains and upgrade domains.</span></span> <span data-ttu-id="97a41-117">A méretezési csoport alapértelmezés szerint egy legfeljebb 100 virtuális gép méretű elhelyezési csoportból áll.</span><span class="sxs-lookup"><span data-stu-id="97a41-117">By default, a scale set consists of a single placement group with a maximum size of 100 VMs.</span></span> <span data-ttu-id="97a41-118">Ha egy méretezési nevű _singlePlacementGroup_ értéke too_false_, hello méretezési több elhelyezési csoport állhat, és 0 – 1000 tartománnyal rendelkező virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="97a41-118">If a scale set property called _singlePlacementGroup_ is set too_false_, hello scale set can be composed of multiple placement groups and has a range of 0-1,000 VMs.</span></span> <span data-ttu-id="97a41-119">Ha az alapértelmezett értéke toohello beállítása _igaz_, a méretezési egyetlen elhelyezési csoport áll, és 0 – 100 tartománnyal rendelkező virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="97a41-119">When set toohello default value of _true_, a scale set is composed of a single placement group, and has a range of 0-100 VMs.</span></span>

## <a name="checklist-for-using-large-scale-sets"></a><span data-ttu-id="97a41-120">Ellenőrzőlista a nagyméretű méretezési csoportok használatához</span><span class="sxs-lookup"><span data-stu-id="97a41-120">Checklist for using large scale sets</span></span>
<span data-ttu-id="97a41-121">toodecide, hogy az alkalmazás képes hatékony felhasználása nagy méretű méretezési csoportok, fontolja meg a követelményeknek hello:</span><span class="sxs-lookup"><span data-stu-id="97a41-121">toodecide whether your application can make effective use of large scale sets, consider hello following requirements:</span></span>

- <span data-ttu-id="97a41-122">A nagyméretű méretezési csoportokhoz az Azure Managed Disks szükséges.</span><span class="sxs-lookup"><span data-stu-id="97a41-122">Large scale sets require Azure Managed Disks.</span></span> <span data-ttu-id="97a41-123">Azokhoz a méretezési csoportokhoz, amelyeket nem a Managed Disksszel hoztak létre, több tárfiókra van szükség (egyre minden 20 virtuális géphez).</span><span class="sxs-lookup"><span data-stu-id="97a41-123">Scale sets that are not created with Managed Disks require multiple storage accounts (one for every 20 VMs).</span></span> <span data-ttu-id="97a41-124">Nagy méretű méretezési csoportok kizárólag a storage-fiókok korlátozza a storage management terhelés, és tooavoid hello kockázatát előfizetés rendszert futtató felügyelt lemezek tooreduce tervezett toowork.</span><span class="sxs-lookup"><span data-stu-id="97a41-124">Large scale sets are designed toowork exclusively with Managed Disks tooreduce your storage management overhead, and tooavoid hello risk of running into subscription limits for storage accounts.</span></span> <span data-ttu-id="97a41-125">Nem kezelt lemezek használja, a méretezési esetén korlátozott too100 virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="97a41-125">If you do not use Managed Disks, your scale set is limited too100 VMs.</span></span>
- <span data-ttu-id="97a41-126">Méretezési készlet létrehozása az Azure piactéren elérhető rendszerkép too1, 000 virtuális gépek költenie.</span><span class="sxs-lookup"><span data-stu-id="97a41-126">Scale sets created from Azure Marketplace images can scale up too1,000 VMs.</span></span>
- <span data-ttu-id="97a41-127">Egyéni képek (VM-lemezképekkel hoz létre, és töltse fel saját maga) alapján létrehozott méretezési csoportok jelenleg legfeljebb too100 virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="97a41-127">Scale sets created from custom images (VM images you create and upload yourself) can currently scale up too100 VMs.</span></span>
- <span data-ttu-id="97a41-128">Réteg-4 terheléselosztás hello Azure terheléselosztó és még nem támogatott a méretezési készlet több elhelyezési csoport alkotja.</span><span class="sxs-lookup"><span data-stu-id="97a41-128">Layer-4 load balancing with hello Azure Load Balancer is not yet supported for scale sets composed of multiple placement groups.</span></span> <span data-ttu-id="97a41-129">Azure Load Balancer győződjön meg arról, hogy hello méretezési készletben toouse hello szüksége van konfigurált toouse egyetlen elhelyezési csoport, amely hello alapértelmezett beállítás.</span><span class="sxs-lookup"><span data-stu-id="97a41-129">If you need toouse hello Azure Load Balancer make sure hello scale set is configured toouse a single placement group, which is hello default setting.</span></span>
- <span data-ttu-id="97a41-130">Az összes méretezési csoportok réteg-7 terheléselosztás és hello Azure Application Gateway esetén támogatott.</span><span class="sxs-lookup"><span data-stu-id="97a41-130">Layer-7 load balancing with hello Azure Application Gateway is supported for all scale sets.</span></span>
- <span data-ttu-id="97a41-131">A méretezési van definiálva, egyetlen alhálózattal – ellenőrizze, hogy az alhálózat rendelkezik egy megfelelő méretű címtartománnyal kell hello virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="97a41-131">A scale set is defined with a single subnet - make sure your subnet has an address space large enough for all hello VMs you need.</span></span> <span data-ttu-id="97a41-132">Alapértelmezés szerint olyan méretezési overprovisions (hoz létre további virtuális gépek központi telepítéskor vagy kiterjesztése, amely nem a van szó) tooimprove telepítési megbízhatóságát és teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="97a41-132">By default a scale set overprovisions (creates extra VMs at deployment time or when scaling out, which you are not charged for) tooimprove deployment reliability and performance.</span></span> <span data-ttu-id="97a41-133">Lehetővé teszi egy cím lemezterület 20 % nagyobb, mint a virtuális gépek azt tervezi, hogy tooscale hello száma.</span><span class="sxs-lookup"><span data-stu-id="97a41-133">Allow for an address space 20% greater than hello number of VMs you plan tooscale to.</span></span>
- <span data-ttu-id="97a41-134">Ha azt tervezi, toodeploy sok virtuális gép, a számítási core kvótakorlát esetleg toobe nőtt.</span><span class="sxs-lookup"><span data-stu-id="97a41-134">If you are planning toodeploy many VMs, your Compute core quota limits may need toobe increased.</span></span>
- <span data-ttu-id="97a41-135">A tartalék tartományok és a frissítési tartományok csak az elhelyezési csoporton belül konzisztensek.</span><span class="sxs-lookup"><span data-stu-id="97a41-135">Fault domains and upgrade domains are only consistent within a placement group.</span></span> <span data-ttu-id="97a41-136">Ez az architektúra nem változtatja meg a hello teljes méretű rendelkezésre állási csoportban, virtuális gépek különböző fizikai hardver egyenlően elosztott, akkor viszont azt jelenti, hogy ha két virtuális gépek vannak a másik hardverekhez, tooguarantee kell gondoskodjon arról, hogy azok különböző hiba a tartományok hello elhelyezési ugyanabban a csoportban.</span><span class="sxs-lookup"><span data-stu-id="97a41-136">This architecture does not change hello overall availability of a scale set, as VMs are evenly distributed across distinct physical hardware, but it does means that if you need tooguarantee two VMs are on different hardware, make sure they are in different fault domains in hello same placement group.</span></span> <span data-ttu-id="97a41-137">Tartalék tartomány és elhelyezési Csoportazonosító látható hello _nézet példány_ a skála beállítása a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="97a41-137">Fault domain and placement group ID are shown in hello _instance view_ of a scale set VM.</span></span> <span data-ttu-id="97a41-138">A Virtuálisgép-méretezési csoport példányait tartalmazó nézetet hello megtekintheti a hello [Azure erőforrás-kezelő](https://resources.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="97a41-138">You can view hello instance view of a scale set VM in hello [Azure Resource Explorer](https://resources.azure.com/).</span></span>


## <a name="creating-a-large-scale-set"></a><span data-ttu-id="97a41-139">Nagyméretű méretezési csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="97a41-139">Creating a large scale set</span></span>
<span data-ttu-id="97a41-140">Amikor létrehoz egy méretezési hello Azure-portálon a készletben, engedélyezheti annak tooscale toomultiple elhelyezési csoportok által hello beállítása _korlát tooa egyetlen elhelyezési csoport_ beállítás too_False_ a hello _alapjai_ panelen.</span><span class="sxs-lookup"><span data-stu-id="97a41-140">When you create a scale set in hello Azure portal, you can allow it tooscale toomultiple placement groups by setting hello _Limit tooa single placement group_ option too_False_ in hello _Basics_ blade.</span></span> <span data-ttu-id="97a41-141">Ez a beállítás set too_False_ megadhatja egy _száma példány_ too1, 000 magasabb érték esetén a.</span><span class="sxs-lookup"><span data-stu-id="97a41-141">With this option set too_False_, you can specify an _Instance count_ value of up too1,000.</span></span>

![](./media/virtual-machine-scale-sets-placement-groups/portal-large-scale.png)

<span data-ttu-id="97a41-142">Létrehozhat egy nagy Virtuálisgép-méretezési hello segítségével [Azure CLI](https://github.com/Azure/azure-cli) _az vmss létrehozása_ parancsot.</span><span class="sxs-lookup"><span data-stu-id="97a41-142">You can create a large VM scale set using hello [Azure CLI](https://github.com/Azure/azure-cli) _az vmss create_ command.</span></span> <span data-ttu-id="97a41-143">Ez a parancs beállítja az intelligens alapértelmezett beállításokat, például az alhálózat méretét hello alapján _példányszám_ argumentum:</span><span class="sxs-lookup"><span data-stu-id="97a41-143">This command sets intelligent defaults such as subnet size based on hello _instance-count_ argument:</span></span>

```bash
az group create -l southcentralus -n biginfra
az vmss create -g biginfra -n bigvmss --image ubuntults --instance-count 1000
```
<span data-ttu-id="97a41-144">Vegye figyelembe, hogy hello _vmss létrehozása_ parancs bizonyos konfigurációs értékeket alapértelmezés szerint, ha nem adja meg azokat.</span><span class="sxs-lookup"><span data-stu-id="97a41-144">Note that hello _vmss create_ command defaults certain configuration values if you do not specify them.</span></span> <span data-ttu-id="97a41-145">toosee hello elérhető beállításokat lehet felülbírálni, próbálja meg:</span><span class="sxs-lookup"><span data-stu-id="97a41-145">toosee hello available options that you can override, try:</span></span>
```bash
az vmss create --help
```

<span data-ttu-id="97a41-146">Ha állítja be az Azure Resource Manager-sablon létrehozása nagy méretű hoz létre, győződjön meg arról hello sablon Azure felügyelt lemezek alapuló méretezési készletet hoz létre.</span><span class="sxs-lookup"><span data-stu-id="97a41-146">If you are creating a large scale set by composing an Azure Resource Manager template, make sure hello template creates a scale set based on Azure Managed Disks.</span></span> <span data-ttu-id="97a41-147">Beállíthatja a hello _singlePlacementGroup_ tulajdonság too_false_ a hello _tulajdonságok_ hello szakasza _Microsoft.Compute/virtualMAchineScaleSets_ erőforrás.</span><span class="sxs-lookup"><span data-stu-id="97a41-147">You can set hello _singlePlacementGroup_ property too_false_ in hello _properties_ section of hello _Microsoft.Compute/virtualMAchineScaleSets_ resource.</span></span> <span data-ttu-id="97a41-148">hello következő JSON-töredéket mutatja egy méretezési sablon beállítása, beleértve a hello 1000 Virtuálisgép-kapacitást és hello hello elejére _"singlePlacementGroup": hamis_ beállítást:</span><span class="sxs-lookup"><span data-stu-id="97a41-148">hello following JSON fragment shows hello beginning of a scale set template, including hello 1,000 VM capacity and hello _"singlePlacementGroup" : false_ setting:</span></span>
```json
{
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  "location": "australiaeast",
  "name": "bigvmss",
  "sku": {
    "name": "Standard_DS1_v2",
    "tier": "Standard",
    "capacity": 1000
  },
  "properties": {
    "singlePlacementGroup": false,
    "upgradePolicy": {
      "mode": "Automatic"
    }
```
<span data-ttu-id="97a41-149">Túl nagy méretű átfogó példát a sablon beállítása tudnivalókat[https://github.com/gbowerman/azure-myriad/blob/master/bigtest/bigbottle.json](https://github.com/gbowerman/azure-myriad/blob/master/bigtest/bigbottle.json).</span><span class="sxs-lookup"><span data-stu-id="97a41-149">For a complete example of a large scale set template, refer too[https://github.com/gbowerman/azure-myriad/blob/master/bigtest/bigbottle.json](https://github.com/gbowerman/azure-myriad/blob/master/bigtest/bigbottle.json).</span></span>

## <a name="converting-an-existing-scale-set-toospan-multiple-placement-groups"></a><span data-ttu-id="97a41-150">Egy meglévő méretezési konvertálása beállítása toospan több elhelyezési csoportok</span><span class="sxs-lookup"><span data-stu-id="97a41-150">Converting an existing scale set toospan multiple placement groups</span></span>
<span data-ttu-id="97a41-151">egy meglévő Virtuálisgép-méretezési készlet képes mint 100 virtuális gépek toomore skálázás toomake, kell toochange hello _singplePlacementGroup_ tulajdonság too_false_ hello méretezési modell beállítása.</span><span class="sxs-lookup"><span data-stu-id="97a41-151">toomake an existing VM scale set capable of scaling toomore than 100 VMs, you need toochange hello _singplePlacementGroup_ property too_false_ in hello scale set model.</span></span> <span data-ttu-id="97a41-152">Tesztelheti a hello e tulajdonság módosítása [Azure erőforrás-kezelő](https://resources.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="97a41-152">You can test changing this property with hello [Azure Resource Explorer](https://resources.azure.com/).</span></span> <span data-ttu-id="97a41-153">Méretezési készlet, jelölje be található _szerkesztése_ , és módosítsa a hello _singlePlacementGroup_ tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="97a41-153">Find an existing scale set, select _Edit_ and change hello _singlePlacementGroup_ property.</span></span> <span data-ttu-id="97a41-154">Ha nem látja ezt a tulajdonságot, akkor lehetséges, hogy látja hello méretezési készletben hello Microsoft.Compute API egy régebbi verziója.</span><span class="sxs-lookup"><span data-stu-id="97a41-154">If you do not see this property, you may be viewing hello scale set with an older version of hello Microsoft.Compute API.</span></span>

>[!NOTE] 
<span data-ttu-id="97a41-155">Módosíthatja a skála állítható be egy egyetlen elhelyezési csoport egyetlen (hello alapértelmezett viselkedés) tooa több elhelyezési csoportok támogató támogató, de hello megfordítva már nem lehet konvertálni.</span><span class="sxs-lookup"><span data-stu-id="97a41-155">You can change a scale set from supporting a single placement group only (hello default behavior) tooa supporting multiple placement groups, but you cannot convert hello other way around.</span></span> <span data-ttu-id="97a41-156">Ezért tudja, hogy hello tulajdonságok nagy méretű készlet átalakítás előtt.</span><span class="sxs-lookup"><span data-stu-id="97a41-156">Therefore make sure you understand hello properties of large scale sets before converting.</span></span> <span data-ttu-id="97a41-157">Különösen ellenőrizze, hogy nem kell hello Azure terheléselosztó a terheléselosztási réteg-4.</span><span class="sxs-lookup"><span data-stu-id="97a41-157">In particular, make sure you do not need layer-4 load balancing with hello Azure Load Balancer.</span></span>

