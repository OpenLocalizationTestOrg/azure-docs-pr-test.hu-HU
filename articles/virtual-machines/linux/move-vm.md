---
title: "A Linux virtuális gépek áthelyezése az Azure-ban |} Microsoft Docs"
description: "Linux virtuális gép áthelyezése egy másik Azure-előfizetés vagy az erőforrás-csoportok a Resource Manager üzembe helyezési modellben."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: d635f0a5-4458-4b95-a5f8-eed4f41eb4d4
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: article
ms.date: 03/22/2017
ms.author: cynthn
ms.openlocfilehash: 4695a9c934f97f2b2d448c4990e7ad5533e38e9f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="move-a-linux-vm-to-another-subscription-or-resource-group"></a><span data-ttu-id="912a9-103">Linux virtuális gép áthelyezése egy másik előfizetés vagy az erőforrás-csoport</span><span class="sxs-lookup"><span data-stu-id="912a9-103">Move a Linux VM to another subscription or resource group</span></span>
<span data-ttu-id="912a9-104">Ez a cikk végigvezeti a Linux virtuális gépek áthelyezése erőforráscsoportok vagy előfizetések között.</span><span class="sxs-lookup"><span data-stu-id="912a9-104">This article walks you through how to move a Linux VM between resource groups or subscriptions.</span></span> <span data-ttu-id="912a9-105">A virtuális gépek áthelyezése másik előfizetések lehet hasznos, ha egy virtuális Gépet egy személyes előfizetésben hozott létre, és most szeretné helyezze át a vállalat előfizetés.</span><span class="sxs-lookup"><span data-stu-id="912a9-105">Moving a VM between subscriptions can be handy if you created a VM in a personal subscription and now want to move it to your company's subscription.</span></span>

> [!IMPORTANT]
><span data-ttu-id="912a9-106">Felügyelt lemezek jelenleg nem helyezhető át.</span><span class="sxs-lookup"><span data-stu-id="912a9-106">You cannot move Managed Disks at this time.</span></span> 
>
><span data-ttu-id="912a9-107">Új erőforrás-azonosítók az áthelyezés részeként jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="912a9-107">New resource IDs are created as part of the move.</span></span> <span data-ttu-id="912a9-108">Miután a virtuális gép át lett helyezve, módosítania az eszközök és parancsfájlok használata az új erőforrás-azonosítók.</span><span class="sxs-lookup"><span data-stu-id="912a9-108">Once the VM has been moved, you need to update your tools and scripts to use the new resource IDs.</span></span> 
> 
> 

## <a name="use-the-azure-cli-to-move-a-vm"></a><span data-ttu-id="912a9-109">Helyezze át a virtuális Gépet az Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="912a9-109">Use the Azure CLI to move a VM</span></span>
<span data-ttu-id="912a9-110">Sikeresen helyezi át a virtuális gép, helyezze át a virtuális gép és annak támogató erőforrásokat kell.</span><span class="sxs-lookup"><span data-stu-id="912a9-110">To successfully move a VM, you need to move the VM and all its supporting resources.</span></span> <span data-ttu-id="912a9-111">Használja a **azure-csoportok megjelenítése** paranccsal listát készíthet az erőforráscsoportot és az azonosítók abban tárolt összes erőforrás.</span><span class="sxs-lookup"><span data-stu-id="912a9-111">Use the **azure group show** command to list all the resources in a resource group and their IDs.</span></span> <span data-ttu-id="912a9-112">Ez a parancs kimenetében pipe fájlba, így másolja és illessze be az azonosítók újabb parancsok segíti.</span><span class="sxs-lookup"><span data-stu-id="912a9-112">It helps to pipe the output of this command to a file so you can copy and paste the IDs into later commands.</span></span>

    azure group show <resourceGroupName>

<span data-ttu-id="912a9-113">A virtuális gépek és az erőforrások áthelyezése egy másik erőforráscsoportban, használja a **az azure erőforrás-áthelyezés** CLI parancsot.</span><span class="sxs-lookup"><span data-stu-id="912a9-113">To move a VM and its resources to another resource group, use the **azure resource move** CLI command.</span></span> <span data-ttu-id="912a9-114">A következő példa bemutatja, hogyan kívánja áthelyezni a virtuális gép és a leggyakrabban használt erőforrásokat igényel.</span><span class="sxs-lookup"><span data-stu-id="912a9-114">The following example shows how to move a VM and the most common resources it requires.</span></span> <span data-ttu-id="912a9-115">Használjuk a **-i** paraméter és egy vesszővel tagolt (szóközök nélkül) azonosítók listáját az erőforrások áthelyezése adjon át.</span><span class="sxs-lookup"><span data-stu-id="912a9-115">We use the **-i** parameter and pass in a comma-separated list (without spaces) of IDs for the resources to move.</span></span>

    vm=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Compute/virtualMachines/<vmName>
    nic=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/networkInterfaces/<nicName>
    nsg=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/networkSecurityGroups/<nsgName>
    pip=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/publicIPAddresses/<publicIPName>
    vnet=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/virtualNetworks/<vnetName>
    diag=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Storage/storageAccounts/<diagnosticStorageAccountName>
    storage=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Storage/storageAccounts/<storageAcountName>      

    azure resource move --ids $vm,$nic,$nsg,$pip,$vnet,$storage,$diag -d "<destinationResourceGroup>"

<span data-ttu-id="912a9-116">Ha azt szeretné, a virtuális gép és az erőforrások áthelyezése egy másik előfizetést, vegye fel a **– cél-subscriptionId &#60; destinationSubscriptionID &#62;** paraméterrel adhatja meg a célként megadott előfizetés.</span><span class="sxs-lookup"><span data-stu-id="912a9-116">If you want to move the VM and its resources to a different subscription, add the **--destination-subscriptionId &#60;destinationSubscriptionID&#62;** parameter to specify the destination subscription.</span></span>

<span data-ttu-id="912a9-117">Ha Windows-számítógépen a parancssorból dolgozik, akkor hozzon létre egy  **$**  a változó neve, amikor azok deklarálhatja elé.</span><span class="sxs-lookup"><span data-stu-id="912a9-117">If you are working from the Command Prompt on a Windows computer, you need to add a **$** in front of the variable names when you declare them.</span></span> <span data-ttu-id="912a9-118">Ez a Linux nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="912a9-118">This isn't needed in Linux.</span></span>

<span data-ttu-id="912a9-119">Győződjön meg arról, hogy szeretné-e a megadott erőforrás áthelyezése kérni.</span><span class="sxs-lookup"><span data-stu-id="912a9-119">You are asked to confirm that you want to move the specified resource.</span></span> <span data-ttu-id="912a9-120">Típus **Y** annak ellenőrzéséhez, hogy szeretné-e az erőforrások áthelyezése.</span><span class="sxs-lookup"><span data-stu-id="912a9-120">Type **Y** to confirm that you want to move the resources.</span></span>

[!INCLUDE [virtual-machines-common-move-vm](../../../includes/virtual-machines-common-move-vm.md)]

## <a name="next-steps"></a><span data-ttu-id="912a9-121">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="912a9-121">Next steps</span></span>
<span data-ttu-id="912a9-122">Számos különböző típusú erőforrások áthelyezheti erőforráscsoport-sablonok és előfizetések között.</span><span class="sxs-lookup"><span data-stu-id="912a9-122">You can move many different types of resources between resource groups and subscriptions.</span></span> <span data-ttu-id="912a9-123">További információ: [Erőforrások áthelyezése új erőforráscsoportba vagy előfizetésbe](../../resource-group-move-resources.md).</span><span class="sxs-lookup"><span data-stu-id="912a9-123">For more information, see [Move resources to new resource group or subscription](../../resource-group-move-resources.md).</span></span>    

