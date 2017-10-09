---
title: "az Azure Managed lemez a biztonsági mentést aaaCopy |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy példányát az Azure Managed lemez toouse vissza felfelé vagy hibaelhárítási lemez ad ki."
documentationcenter: 
author: squillace
manager: timlt
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 2/6/2017
ms.author: rasquill
ms.openlocfilehash: 41b91c2d68eb5be9c493a66be5f7d085a70450d0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-copy-of-a-vhd-stored-as-an-azure-managed-disk-by-using-managed-snapshots"></a><span data-ttu-id="83938-103">Felügyelt pillanatképek használatával Azure felügyelt lemezként tárolt virtuális merevlemez másolatának létrehozása</span><span class="sxs-lookup"><span data-stu-id="83938-103">Create a copy of a VHD stored as an Azure Managed Disk by using Managed Snapshots</span></span>
<span data-ttu-id="83938-104">Pillanatkép készítése a biztonsági mentéshez felügyelt lemez vagy hello pillanatképből kezelt lemez létrehozása, és mellékelje tooa teszt virtuális gép tootroubleshoot.</span><span class="sxs-lookup"><span data-stu-id="83938-104">Take a snapshot of a Managed disk for backup or create a Managed Disk from hello snapshot and attach it tooa test virtual machine tootroubleshoot.</span></span> <span data-ttu-id="83938-105">Felügyelt pillanatkép egy virtuális gép kezelt lemez teljes-időpontban másolatát.</span><span class="sxs-lookup"><span data-stu-id="83938-105">A Managed Snapshot is a full point-in-time copy of a VM Managed Disk.</span></span> <span data-ttu-id="83938-106">Azt a virtuális merevlemez csak olvasható másolatát hozza létre, és alapértelmezés szerint tárolja azt, Standard szintű felügyelt lemezes.</span><span class="sxs-lookup"><span data-stu-id="83938-106">It creates a read-only copy of your VHD and, by default, stores it as a Standard Managed Disk.</span></span> 

<span data-ttu-id="83938-107">További információk a díjszabásról: [Azure Storage szolgáltatás díjszabása](https://azure.microsoft.com/pricing/details/managed-disks/).</span><span class="sxs-lookup"><span data-stu-id="83938-107">For information about pricing, see [Azure Storage Pricing](https://azure.microsoft.com/pricing/details/managed-disks/).</span></span> <!--Add link tootopic or blog post that explains managed disks. -->

<span data-ttu-id="83938-108">Használjon vagy hello Azure portál vagy hello Azure CLI 2.0 tootake hello kezelt lemez pillanatképet.</span><span class="sxs-lookup"><span data-stu-id="83938-108">Use either hello Azure portal or hello Azure CLI 2.0 tootake a snapshot of hello Managed Disk.</span></span>

## <a name="use-azure-cli-20-tootake-a-snapshot"></a><span data-ttu-id="83938-109">Azure CLI 2.0 tootake pillanatkép használata</span><span class="sxs-lookup"><span data-stu-id="83938-109">Use Azure CLI 2.0 tootake a snapshot</span></span>

> [!NOTE] 
> <span data-ttu-id="83938-110">hello alábbi példa telepíteni kell hello Azure CLI 2.0 és az Azure-fiókjával bejelentkezik.</span><span class="sxs-lookup"><span data-stu-id="83938-110">hello following example requires hello Azure CLI 2.0 installed and logged into your Azure account.</span></span>

<span data-ttu-id="83938-111">hello következő lépések bemutatják, hogyan tooobtain, és hajtsa végre a megfelelő felügyelt OS pillanatképe lemez hello segítségével `az snapshot create` hello parancsot `--source-disk` paraméter.</span><span class="sxs-lookup"><span data-stu-id="83938-111">hello following steps show how tooobtain and take a snapshot of a managed OS disk using hello `az snapshot create` command with hello `--source-disk` parameter.</span></span> <span data-ttu-id="83938-112">hello alábbi példa feltételezi, hogy van-e a virtuális gépek nevű `myVM` hello a felügyelt operációsrendszer-lemezzel létrehozott `myResourceGroup` erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="83938-112">hello following example assumes that there is a VM called `myVM` created with a managed OS disk in hello `myResourceGroup` resource group.</span></span>

```azure-cli
# take hello disk id with which toocreate a snapshot
osDiskId=$(az vm show -g myResourceGroup -n myVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)
az snapshot create -g myResourceGroup --source "$osDiskId" --name osDisk-backup
```

<span data-ttu-id="83938-113">hello kimeneti hasonlóan kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="83938-113">hello output should look something like:</span></span>

```json
{
  "accountType": "Standard_LRS",
  "creationData": {
    "createOption": "Copy",
    "imageReference": null,
    "sourceResourceId": null,
    "sourceUri": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/disks/osdisk_6NexYgkFQU",
    "storageAccountId": null
  },
  "diskSizeGb": 30,
  "encryptionSettings": null,
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/snapshots/osDisk-backup",
  "location": "westus",
  "name": "osDisk-backup",
  "osType": "Linux",
  "ownerId": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "tags": null,
  "timeCreated": "2017-02-06T21:27:10.172980+00:00",
  "type": "Microsoft.Compute/snapshots"
}
```

## <a name="use-azure-portal-tootake-a-snapshot"></a><span data-ttu-id="83938-114">Az Azure portál tootake pillanatkép használata</span><span class="sxs-lookup"><span data-stu-id="83938-114">Use Azure portal tootake a snapshot</span></span> 

1. <span data-ttu-id="83938-115">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="83938-115">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="83938-116">-Től kezdődően hello bal felső sarokban kattintson **új** keresse meg a **pillanatkép**.</span><span class="sxs-lookup"><span data-stu-id="83938-116">Starting in hello upper-left, click **New** and search for **snapshot**.</span></span>
3. <span data-ttu-id="83938-117">Hello pillanatkép paneljén kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="83938-117">In hello Snapshot blade, click **Create**.</span></span>
4. <span data-ttu-id="83938-118">Adjon meg egy **neve** hello pillanatkép.</span><span class="sxs-lookup"><span data-stu-id="83938-118">Enter a **Name** for hello snapshot.</span></span>
5. <span data-ttu-id="83938-119">Válasszon ki egy létező [erőforráscsoport](../../azure-resource-manager/resource-group-overview.md#resource-groups) vagy egy új hello típusnév.</span><span class="sxs-lookup"><span data-stu-id="83938-119">Select an existing [Resource group](../../azure-resource-manager/resource-group-overview.md#resource-groups) or type hello name for a new one.</span></span> 
6. <span data-ttu-id="83938-120">Válasszon egy Azure-adatközpontban helyét.</span><span class="sxs-lookup"><span data-stu-id="83938-120">Select an Azure datacenter Location.</span></span>  
7. <span data-ttu-id="83938-121">A **forráslemez**, válassza ki a kezelt lemez toosnapshot hello.</span><span class="sxs-lookup"><span data-stu-id="83938-121">For **Source disk**, select hello Managed Disk toosnapshot.</span></span>
8. <span data-ttu-id="83938-122">Jelölje be hello **fiók típusa** toouse toostore hello pillanatkép.</span><span class="sxs-lookup"><span data-stu-id="83938-122">Select hello **Account type** toouse toostore hello snapshot.</span></span> <span data-ttu-id="83938-123">Ajánlott **Standard_LRS** kivéve, ha esetleg szükség lenne rá egy nagy teljesítményű lemezen tárolja.</span><span class="sxs-lookup"><span data-stu-id="83938-123">We recommend **Standard_LRS** unless you need it stored on a high performing disk.</span></span>
9. <span data-ttu-id="83938-124">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="83938-124">Click **Create**.</span></span>

<span data-ttu-id="83938-125">Ha toouse hello pillanatkép toocreate tervezze meg a felügyelt lemez, és csatlakoztassa a virtuális gép, amelyet a toobe magas hajt végre, használja a hello paramétert `--sku Premium_LRS` a hello `az snapshot create` parancsot.</span><span class="sxs-lookup"><span data-stu-id="83938-125">If you plan toouse hello snapshot toocreate a Managed Disk and attach it a VM that needs toobe high performing, use hello parameter `--sku Premium_LRS` with hello `az snapshot create` command.</span></span> <span data-ttu-id="83938-126">Ez létrehoz hello pillanatkép, prémium felügyelt lemezként tárolja.</span><span class="sxs-lookup"><span data-stu-id="83938-126">This creates hello snapshot so that it is stored as a Premium Managed Disk.</span></span> <span data-ttu-id="83938-127">Prémium szintű felügyelt lemez jobb teljesítményt, mert SSD-meghajtót (SSD), de drágább, mint a Standard lemezek (HDD).</span><span class="sxs-lookup"><span data-stu-id="83938-127">Premium Managed Disks perform better because they are solid-state drives (SSDs), but cost more than Standard disks (HDDs).</span></span>


