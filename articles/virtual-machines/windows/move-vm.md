---
title: "a Windows virtuális gép erőforrásához az Azure-ban aaaMove |} Microsoft Docs"
description: "Helyezze át egy Windows virtuális gép tooanother Azure-előfizetéshez vagy erőforráscsoporthoz hello Resource Manager üzembe helyezési modellben."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 4e383427-4aff-4bf3-a0f4-dbff5c6f0c81
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/22/2017
ms.author: cynthn
ms.openlocfilehash: 859e78dce9acf1168780d4ee8e9f6dac0e3c11cf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-a-windows-vm-tooanother-azure-subscription-or-resource-group"></a><span data-ttu-id="ffac7-103">Windows virtuális gép tooanother Azure-előfizetéshez vagy erőforráscsoporthoz áthelyezése</span><span class="sxs-lookup"><span data-stu-id="ffac7-103">Move a Windows VM tooanother Azure subscription or resource group</span></span>
<span data-ttu-id="ffac7-104">Ez a cikk végigvezeti toomove a Windows virtuális gépek erőforrás-csoportok vagy előfizetések között.</span><span class="sxs-lookup"><span data-stu-id="ffac7-104">This article walks you through how toomove a Windows VM between resource groups or subscriptions.</span></span> <span data-ttu-id="ffac7-105">Az előfizetések közötti áthelyezése lehet hasznos, ha az eredetileg létrehozott virtuális gép személyes előfizetés és most szeretné, hogy toomove azt tooyour vállalati előfizetés toocontinue a munkáját.</span><span class="sxs-lookup"><span data-stu-id="ffac7-105">Moving between subscriptions can be handy if you originally created a VM in a personal subscription and now want toomove it tooyour company's subscription toocontinue your work.</span></span>

> [!IMPORTANT]
><span data-ttu-id="ffac7-106">Felügyelt lemezek jelenleg nem helyezhető át.</span><span class="sxs-lookup"><span data-stu-id="ffac7-106">You cannot move Managed Disks at this time.</span></span> 
>
><span data-ttu-id="ffac7-107">Új erőforrás-azonosítók hello áthelyezés részeként jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="ffac7-107">New resource IDs are created as part of hello move.</span></span> <span data-ttu-id="ffac7-108">Miután hello virtuális gép át lett helyezve, kell tooupdate az eszközök és parancsfájlok toouse hello új erőforrás-azonosítók.</span><span class="sxs-lookup"><span data-stu-id="ffac7-108">Once hello VM has been moved, you need tooupdate your tools and scripts toouse hello new resource IDs.</span></span> 
> 
> 

[!INCLUDE [virtual-machines-common-move-vm](../../../includes/virtual-machines-common-move-vm.md)]

## <a name="use-powershell-toomove-a-vm"></a><span data-ttu-id="ffac7-109">A virtuális gép Powershell toomove használata</span><span class="sxs-lookup"><span data-stu-id="ffac7-109">Use Powershell toomove a VM</span></span>
<span data-ttu-id="ffac7-110">toomove egy virtuális gép tooanother erőforráscsoportot, meg kell toomake meg arról, hogy akkor is hello függő erőforrásokat áthelyezni.</span><span class="sxs-lookup"><span data-stu-id="ffac7-110">toomove a virtual machine tooanother resource group, you need toomake sure that you also move all of hello dependent resources.</span></span> <span data-ttu-id="ffac7-111">hello erőforrás nevének és hello típusú erőforrás szüksége toouse hello Move-AzureRMResource parancsmag.</span><span class="sxs-lookup"><span data-stu-id="ffac7-111">toouse hello Move-AzureRMResource cmdlet, you need hello resource name and hello type of resource.</span></span> <span data-ttu-id="ffac7-112">Keresés – AzureRMResource hello parancsmag az kérheti le.</span><span class="sxs-lookup"><span data-stu-id="ffac7-112">You can get both from hello Find-AzureRMResource cmdlet.</span></span>

    Find-AzureRMResource -ResourceGroupNameContains "<sourceResourceGroupName>"


<span data-ttu-id="ffac7-113">a virtuális gép több erőforrást kell toomove toomove.</span><span class="sxs-lookup"><span data-stu-id="ffac7-113">toomove a VM we need toomove multiple resources.</span></span> <span data-ttu-id="ffac7-114">Jelenleg csak az egyes erőforrások külön változók létrehozása, és rendezze őket.</span><span class="sxs-lookup"><span data-stu-id="ffac7-114">We can just create separate variables for each resource and then list them.</span></span> <span data-ttu-id="ffac7-115">Ebben a példában egy virtuális Gépet tartalmaz a hello alapvető erőforrások többségét, de igény szerint több is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="ffac7-115">This example includes most of hello basic resources for a VM, but you can add more as needed.</span></span>

    $sourceRG = "<sourceResourceGroupName>"
    $destinationRG = "<destinationResourceGroupName>"

    $vm = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Compute/virtualMachines" -ResourceName "<vmName>"
    $storageAccount = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Storage/storageAccounts" -ResourceName "<storageAccountName>"
    $diagStorageAccount = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Storage/storageAccounts" -ResourceName "<diagnosticStorageAccountName>"
    $vNet = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/virtualNetworks" -ResourceName "<vNetName>"
    $nic = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/networkInterfaces" -ResourceName "<nicName>"
    $ip = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/publicIPAddresses" -ResourceName "<ipName>"
    $nsg = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/networkSecurityGroups" -ResourceName "<nsgName>"

    Move-AzureRmResource -DestinationResourceGroupName $destinationRG -ResourceId $vm.ResourceId, $storageAccount.ResourceId, $diagStorageAccount.ResourceId, $vNet.ResourceId, $nic.ResourceId, $ip.ResourceId, $nsg.ResourceId

<span data-ttu-id="ffac7-116">toomove hello erőforrások toodifferent előfizetés, közé tartozik a hello **- DestinationSubscriptionId** paraméter.</span><span class="sxs-lookup"><span data-stu-id="ffac7-116">toomove hello resources toodifferent subscription, include hello **-DestinationSubscriptionId** parameter.</span></span> 

    Move-AzureRmResource -DestinationSubscriptionId "<destinationSubscriptionID>" -DestinationResourceGroupName $destinationRG -ResourceId $vm.ResourceId, $storageAccount.ResourceId, $diagStorageAccount.ResourceId, $vNet.ResourceId, $nic.ResourceId, $ip.ResourceId, $nsg.ResourceId



<span data-ttu-id="ffac7-117">Meg kell adnia, hogy szeretné-e toomove hello tooconfirm megadott erőforrások.</span><span class="sxs-lookup"><span data-stu-id="ffac7-117">You will be asked tooconfirm that you want toomove hello specified resources.</span></span> <span data-ttu-id="ffac7-118">Típus **Y** tooconfirm, amelyet az toomove hello erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="ffac7-118">Type **Y** tooconfirm that you want toomove hello resources.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ffac7-119">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ffac7-119">Next steps</span></span>
<span data-ttu-id="ffac7-120">Számos különböző típusú erőforrások áthelyezheti erőforráscsoport-sablonok és előfizetések között.</span><span class="sxs-lookup"><span data-stu-id="ffac7-120">You can move many different types of resources between resource groups and subscriptions.</span></span> <span data-ttu-id="ffac7-121">További információkért lásd: [erőforrások toonew erőforráscsoportba vagy előfizetésbe áthelyezése](../../resource-group-move-resources.md).</span><span class="sxs-lookup"><span data-stu-id="ffac7-121">For more information, see [Move resources toonew resource group or subscription](../../resource-group-move-resources.md).</span></span>    

