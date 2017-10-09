---
title: "a Linux virtuális gépek Azure-ban aaaMove |} Microsoft Docs"
description: "Helyezze át egy Linux virtuális gép tooanother Azure-előfizetéshez vagy erőforráscsoporthoz hello Resource Manager üzembe helyezési modellben."
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
ms.openlocfilehash: 938d04234059111912f03e72d14dabd338bc0678
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-a-linux-vm-tooanother-subscription-or-resource-group"></a><span data-ttu-id="6c02a-103">Linux virtuális gép tooanother előfizetés vagy az erőforrás csoport áthelyezése</span><span class="sxs-lookup"><span data-stu-id="6c02a-103">Move a Linux VM tooanother subscription or resource group</span></span>
<span data-ttu-id="6c02a-104">Ez a cikk végigvezeti toomove Linux virtuális gép erőforráscsoportok vagy előfizetések között.</span><span class="sxs-lookup"><span data-stu-id="6c02a-104">This article walks you through how toomove a Linux VM between resource groups or subscriptions.</span></span> <span data-ttu-id="6c02a-105">A virtuális gépek áthelyezése másik előfizetések lehet hasznos, ha létrehozott egy virtuális Gépet egy személyes előfizetésben és most szeretné, hogy toomove azt tooyour vállalat előfizetésének.</span><span class="sxs-lookup"><span data-stu-id="6c02a-105">Moving a VM between subscriptions can be handy if you created a VM in a personal subscription and now want toomove it tooyour company's subscription.</span></span>

> [!IMPORTANT]
><span data-ttu-id="6c02a-106">Felügyelt lemezek jelenleg nem helyezhető át.</span><span class="sxs-lookup"><span data-stu-id="6c02a-106">You cannot move Managed Disks at this time.</span></span> 
>
><span data-ttu-id="6c02a-107">Új erőforrás-azonosítók hello áthelyezés részeként jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="6c02a-107">New resource IDs are created as part of hello move.</span></span> <span data-ttu-id="6c02a-108">Miután hello virtuális gép át lett helyezve, kell tooupdate az eszközök és parancsfájlok toouse hello új erőforrás-azonosítók.</span><span class="sxs-lookup"><span data-stu-id="6c02a-108">Once hello VM has been moved, you need tooupdate your tools and scripts toouse hello new resource IDs.</span></span> 
> 
> 

## <a name="use-hello-azure-cli-toomove-a-vm"></a><span data-ttu-id="6c02a-109">Hello Azure CLI toomove a virtuális gépek használata</span><span class="sxs-lookup"><span data-stu-id="6c02a-109">Use hello Azure CLI toomove a VM</span></span>
<span data-ttu-id="6c02a-110">toosuccessfully helyezze át a virtuális Gépet, toomove hello virtuális gép és annak támogató forrásokat van szükség.</span><span class="sxs-lookup"><span data-stu-id="6c02a-110">toosuccessfully move a VM, you need toomove hello VM and all its supporting resources.</span></span> <span data-ttu-id="6c02a-111">Használjon hello **azure-csoportok megjelenítése** toolist az erőforráscsoportot és az azonosítók hello erőforrásainak parancsot.</span><span class="sxs-lookup"><span data-stu-id="6c02a-111">Use hello **azure group show** command toolist all hello resources in a resource group and their IDs.</span></span> <span data-ttu-id="6c02a-112">Így másolja és illessze be a hello azonosítók a későbbi parancsok toopipe hello kimeneti parancs tooa fájl segítségével.</span><span class="sxs-lookup"><span data-stu-id="6c02a-112">It helps toopipe hello output of this command tooa file so you can copy and paste hello IDs into later commands.</span></span>

    azure group show <resourceGroupName>

<span data-ttu-id="6c02a-113">toomove a virtuális gépek és az erőforrások tooanother erőforráscsoport használata hello **az azure erőforrás-áthelyezés** CLI parancsot.</span><span class="sxs-lookup"><span data-stu-id="6c02a-113">toomove a VM and its resources tooanother resource group, use hello **azure resource move** CLI command.</span></span> <span data-ttu-id="6c02a-114">hello a következő példa bemutatja, hogyan toomove a virtuális gépek és hello leggyakrabban használt erőforrásokat igényel.</span><span class="sxs-lookup"><span data-stu-id="6c02a-114">hello following example shows how toomove a VM and hello most common resources it requires.</span></span> <span data-ttu-id="6c02a-115">Hello használjuk **-i** paraméter és egy vesszővel tagolt listán (szóközök nélkül) azonosítókat hello erőforrások toomove fázisban.</span><span class="sxs-lookup"><span data-stu-id="6c02a-115">We use hello **-i** parameter and pass in a comma-separated list (without spaces) of IDs for hello resources toomove.</span></span>

    vm=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Compute/virtualMachines/<vmName>
    nic=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/networkInterfaces/<nicName>
    nsg=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/networkSecurityGroups/<nsgName>
    pip=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/publicIPAddresses/<publicIPName>
    vnet=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/virtualNetworks/<vnetName>
    diag=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Storage/storageAccounts/<diagnosticStorageAccountName>
    storage=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Storage/storageAccounts/<storageAcountName>      

    azure resource move --ids $vm,$nic,$nsg,$pip,$vnet,$storage,$diag -d "<destinationResourceGroup>"

<span data-ttu-id="6c02a-116">Ha azt szeretné, hogy toomove hello a virtuális gép és az erőforrások tooa másik előfizetést, vegye fel a hello **– cél-subscriptionId &#60; destinationSubscriptionID &#62;** paraméter toospecify hello célelőfizetés.</span><span class="sxs-lookup"><span data-stu-id="6c02a-116">If you want toomove hello VM and its resources tooa different subscription, add hello **--destination-subscriptionId &#60;destinationSubscriptionID&#62;** parameter toospecify hello destination subscription.</span></span>

<span data-ttu-id="6c02a-117">Hello parancssort a Windows-számítógépen dolgozik, ha szüksége van-e tooadd egy  **$**  hello változók nevében, ha azok deklarálhatja elé.</span><span class="sxs-lookup"><span data-stu-id="6c02a-117">If you are working from hello Command Prompt on a Windows computer, you need tooadd a **$** in front of hello variable names when you declare them.</span></span> <span data-ttu-id="6c02a-118">Ez a Linux nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="6c02a-118">This isn't needed in Linux.</span></span>

<span data-ttu-id="6c02a-119">A rendszer felkéri, hogy szeretné-e toomove hello tooconfirm megadott erőforrás.</span><span class="sxs-lookup"><span data-stu-id="6c02a-119">You are asked tooconfirm that you want toomove hello specified resource.</span></span> <span data-ttu-id="6c02a-120">Típus **Y** tooconfirm, amelyet az toomove hello erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="6c02a-120">Type **Y** tooconfirm that you want toomove hello resources.</span></span>

[!INCLUDE [virtual-machines-common-move-vm](../../../includes/virtual-machines-common-move-vm.md)]

## <a name="next-steps"></a><span data-ttu-id="6c02a-121">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6c02a-121">Next steps</span></span>
<span data-ttu-id="6c02a-122">Számos különböző típusú erőforrások áthelyezheti erőforráscsoport-sablonok és előfizetések között.</span><span class="sxs-lookup"><span data-stu-id="6c02a-122">You can move many different types of resources between resource groups and subscriptions.</span></span> <span data-ttu-id="6c02a-123">További információkért lásd: [erőforrások toonew erőforráscsoportba vagy előfizetésbe áthelyezése](../../resource-group-move-resources.md).</span><span class="sxs-lookup"><span data-stu-id="6c02a-123">For more information, see [Move resources toonew resource group or subscription](../../resource-group-move-resources.md).</span></span>    

