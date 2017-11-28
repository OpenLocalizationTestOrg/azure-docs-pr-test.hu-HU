---
title: "aaaConvert Azure kezelt lemezek tárolási szabványos toopremium, és ez fordítva is igaz |} Microsoft Docs"
description: "Tooconvert Azure kezelésének lemezek tárolási szabványos toopremium, és ez fordítva is igaz, az Azure parancssori felület használatával."
services: virtual-machines-linux
documentationcenter: 
author: ramankum
manager: kavithag
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: ramankum
ms.openlocfilehash: 261d77202f7fd381085c4e25211a5d0026f43b01
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="convert-azure-managed-disks-storage-from-standard-toopremium-and-vice-versa"></a><span data-ttu-id="ef6ed-103">Azure átalakítása felügyelt lemezek tárolási szabványos toopremium, és ez fordítva is igaz</span><span class="sxs-lookup"><span data-stu-id="ef6ed-103">Convert Azure managed disks storage from standard toopremium, and vice versa</span></span>

<span data-ttu-id="ef6ed-104">Kezelt lemezek két tárolási lehetőségeket kínál: [prémium](../../storage/storage-premium-storage.md) (SSD-alapú) és [szabványos](../../storage/storage-standard-storage.md) (HDD-alapú).</span><span class="sxs-lookup"><span data-stu-id="ef6ed-104">Managed disks offers two storage options: [Premium](../../storage/storage-premium-storage.md) (SSD-based) and [Standard](../../storage/storage-standard-storage.md) (HDD-based).</span></span> <span data-ttu-id="ef6ed-105">Ez lehetővé teszi tooeasily kapcsoló minimális állásidővel, a teljesítmény igények alapján hello két beállításai között.</span><span class="sxs-lookup"><span data-stu-id="ef6ed-105">It allows you tooeasily switch between hello two options with minimal downtime based on your performance needs.</span></span> <span data-ttu-id="ef6ed-106">Ez a funkció nem felügyelt lemezek nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="ef6ed-106">This capability is not available for unmanaged disks.</span></span> <span data-ttu-id="ef6ed-107">Azonban úgy is könnyen [toomanaged lemezek konvertálása](convert-unmanaged-to-managed-disks.md) tooeasily kapcsoló két hello-beállítások között.</span><span class="sxs-lookup"><span data-stu-id="ef6ed-107">But you can easily [convert toomanaged disks](convert-unmanaged-to-managed-disks.md) tooeasily switch between hello two options.</span></span>

<span data-ttu-id="ef6ed-108">Ez a cikk bemutatja, hogyan tooconvert kezeli-e lemezek a standard toopremium, és ez fordítva is igaz Azure parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="ef6ed-108">This article shows you how tooconvert managed disks from standard toopremium, and vice versa by using Azure CLI.</span></span> <span data-ttu-id="ef6ed-109">Ha tooinstall kell, vagy frissíteni, lásd: [Azure CLI 2.0 telepítése](/cli/azure/install-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="ef6ed-109">If you need tooinstall or upgrade it, see [Install Azure CLI 2.0](/cli/azure/install-azure-cli.md).</span></span> 

## <a name="before-you-begin"></a><span data-ttu-id="ef6ed-110">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="ef6ed-110">Before you begin</span></span>

* <span data-ttu-id="ef6ed-111">hello átalakításhoz hello virtuális gép újraindítása szükséges, ezért ütemezése hello áttelepítését a lemezek már meglévő karbantartási időszak alatt történjen.</span><span class="sxs-lookup"><span data-stu-id="ef6ed-111">hello conversion requires a restart of hello VM, so schedule hello migration of your disks storage during a pre-existing maintenance window.</span></span> 
* <span data-ttu-id="ef6ed-112">Használatakor a nem felügyelt lemezek, először [toomanaged lemezek konvertálása](convert-unmanaged-to-managed-disks.md) toouse Ez a cikk tooswitch hello két tárolási lehetőségek között.</span><span class="sxs-lookup"><span data-stu-id="ef6ed-112">If you are using unmanaged disks, first [convert toomanaged disks](convert-unmanaged-to-managed-disks.md) toouse this article tooswitch between hello two storage options.</span></span> 


## <a name="convert-all-hello-managed-disks-of-a-vm-from-standard-toopremium-and-vice-versa"></a><span data-ttu-id="ef6ed-113">Az összes hello Convert által kezelt lemezeken a virtuális gépek a szabványos toopremium, és ez fordítva is igaz</span><span class="sxs-lookup"><span data-stu-id="ef6ed-113">Convert all hello managed disks of a VM from standard toopremium, and vice versa</span></span>

<span data-ttu-id="ef6ed-114">A következő példa hello, megmutatjuk, hogyan tooswitch összes lemezt a virtuális gépek a szabványos toopremium tárolási hello.</span><span class="sxs-lookup"><span data-stu-id="ef6ed-114">In hello following example, we show how tooswitch all hello disks of a VM from standard toopremium storage.</span></span> <span data-ttu-id="ef6ed-115">toouse prémium által kezelt lemezeken, a virtuális Gépet kell használnia egy [Virtuálisgép-méretet](sizes.md) , amely támogatja a prémium szintű storage.</span><span class="sxs-lookup"><span data-stu-id="ef6ed-115">toouse premium managed disks, your VM must use a [VM size](sizes.md) that supports premium storage.</span></span> <span data-ttu-id="ef6ed-116">Ebben a példában is, amely támogatja a prémium szintű storage tooa mérete vált.</span><span class="sxs-lookup"><span data-stu-id="ef6ed-116">This example also switches tooa size that supports premium storage.</span></span>

 ```azurecli

#resource group that contains hello virtual machine
rgName='yourResourceGroup'

#Name of hello virtual machine
vmName='yourVM'

#Premium capable size 
#Required only if converting from standard toopremium
size='Standard_DS2_v2'

#Choose between Standard_LRS and Premium_LRS based on your scenario
sku='Premium_LRS'

#Deallocate hello VM before changing hello size of hello VM
az vm deallocate --name $vmName --resource-group $rgName

#Change hello VM size tooa size that supports premium storage 
#Skip this step if converting storage from premium toostandard
az vm resize --resource-group $rgName --name $vmName --size $size

#Update hello sku of all hello data disks 
az vm show -n $vmName -g $rgName --query storageProfile.dataDisks[*].managedDisk -o tsv \
 | awk -v sku=$sku '{system("az disk update --sku "sku" --ids "$1)}'

#Update hello sku of hello OS disk
az vm show -n $vmName -g $rgName --query storageProfile.osDisk.managedDisk -o tsv \
| awk -v sku=$sku '{system("az disk update --sku "sku" --ids "$1)}'

az vm start --name $vmName --resource-group $rgName

```
## <a name="convert-a-managed-disk-from-standard-toopremium-and-vice-versa"></a><span data-ttu-id="ef6ed-117">Alakítsa át a felügyelt lemezes szabványos toopremium, és ez fordítva is igaz</span><span class="sxs-lookup"><span data-stu-id="ef6ed-117">Convert a managed disk from standard toopremium, and vice versa</span></span>

<span data-ttu-id="ef6ed-118">A fejlesztési és tesztelési célú munkaterheléshez érdemes lehet standard és premium lemezek tooreduce toohave keverékével az költsége.</span><span class="sxs-lookup"><span data-stu-id="ef6ed-118">For your dev/test workload, you may want toohave mixture of standard and premium disks tooreduce your cost.</span></span> <span data-ttu-id="ef6ed-119">Ez csak jobb teljesítményt igénylő hello lemezek toopremium tárolási frissítésével végezheti el.</span><span class="sxs-lookup"><span data-stu-id="ef6ed-119">You can accomplish it by upgrading toopremium storage, only hello disks that require better performance.</span></span> <span data-ttu-id="ef6ed-120">A következő példa hello, megmutatjuk, hogyan tooswitch egyetlen lemezt a virtuális gépek szabványos toopremium tárolóból, és ez fordítva is igaz.</span><span class="sxs-lookup"><span data-stu-id="ef6ed-120">In hello following example, we show how tooswitch a single disk of a VM from standard toopremium storage, and vice versa.</span></span> <span data-ttu-id="ef6ed-121">toouse prémium által kezelt lemezeken, a virtuális Gépet kell használnia egy [Virtuálisgép-méretet](sizes.md) , amely támogatja a prémium szintű storage.</span><span class="sxs-lookup"><span data-stu-id="ef6ed-121">toouse premium managed disks, your VM must use a [VM size](sizes.md) that supports premium storage.</span></span> <span data-ttu-id="ef6ed-122">Ebben a példában is, amely támogatja a prémium szintű storage tooa mérete vált.</span><span class="sxs-lookup"><span data-stu-id="ef6ed-122">This example also switches tooa size that supports premium storage.</span></span>

 ```azurecli

#resource group that contains hello managed disk
rgName='yourResourceGroup'

#Name of your managed disk
diskName='yourManagedDiskName'

#Premium capable size 
#Required only if converting from standard toopremium
size='Standard_DS2_v2'

#Choose between Standard_LRS and Premium_LRS based on your scenario
sku='Premium_LRS'

#Get hello parent VM Id 
vmId=$(az disk show --name $diskName --resource-group $rgName --query managedBy --output tsv)

#Deallocate hello VM before changing hello size of hello VM
az vm deallocate --ids $vmId 

#Change hello VM size tooa size that supports premium storage 
#Skip this step if converting storage from premium toostandard
az vm resize --ids $vmId --size $size

# Update hello sku
az disk update --sku $sku --name $diskName --resource-group $rgName 

az vm start --ids $vmId 
```

## <a name="next-steps"></a><span data-ttu-id="ef6ed-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ef6ed-123">Next steps</span></span>

<span data-ttu-id="ef6ed-124">Tegye meg a virtuális gépek csak olvasható másolatát a használatával [pillanatképek](snapshot-copy-managed-disk.md).</span><span class="sxs-lookup"><span data-stu-id="ef6ed-124">Take a read-only copy of a VM by using [snapshots](snapshot-copy-managed-disk.md).</span></span>

