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
# <a name="move-a-windows-vm-tooanother-azure-subscription-or-resource-group"></a>Windows virtuális gép tooanother Azure-előfizetéshez vagy erőforráscsoporthoz áthelyezése
Ez a cikk végigvezeti toomove a Windows virtuális gépek erőforrás-csoportok vagy előfizetések között. Az előfizetések közötti áthelyezése lehet hasznos, ha az eredetileg létrehozott virtuális gép személyes előfizetés és most szeretné, hogy toomove azt tooyour vállalati előfizetés toocontinue a munkáját.

> [!IMPORTANT]
>Felügyelt lemezek jelenleg nem helyezhető át. 
>
>Új erőforrás-azonosítók hello áthelyezés részeként jönnek létre. Miután hello virtuális gép át lett helyezve, kell tooupdate az eszközök és parancsfájlok toouse hello új erőforrás-azonosítók. 
> 
> 

[!INCLUDE [virtual-machines-common-move-vm](../../../includes/virtual-machines-common-move-vm.md)]

## <a name="use-powershell-toomove-a-vm"></a>A virtuális gép Powershell toomove használata
toomove egy virtuális gép tooanother erőforráscsoportot, meg kell toomake meg arról, hogy akkor is hello függő erőforrásokat áthelyezni. hello erőforrás nevének és hello típusú erőforrás szüksége toouse hello Move-AzureRMResource parancsmag. Keresés – AzureRMResource hello parancsmag az kérheti le.

    Find-AzureRMResource -ResourceGroupNameContains "<sourceResourceGroupName>"


a virtuális gép több erőforrást kell toomove toomove. Jelenleg csak az egyes erőforrások külön változók létrehozása, és rendezze őket. Ebben a példában egy virtuális Gépet tartalmaz a hello alapvető erőforrások többségét, de igény szerint több is hozzáadhat.

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

toomove hello erőforrások toodifferent előfizetés, közé tartozik a hello **- DestinationSubscriptionId** paraméter. 

    Move-AzureRmResource -DestinationSubscriptionId "<destinationSubscriptionID>" -DestinationResourceGroupName $destinationRG -ResourceId $vm.ResourceId, $storageAccount.ResourceId, $diagStorageAccount.ResourceId, $vNet.ResourceId, $nic.ResourceId, $ip.ResourceId, $nsg.ResourceId



Meg kell adnia, hogy szeretné-e toomove hello tooconfirm megadott erőforrások. Típus **Y** tooconfirm, amelyet az toomove hello erőforrásokat.

## <a name="next-steps"></a>Következő lépések
Számos különböző típusú erőforrások áthelyezheti erőforráscsoport-sablonok és előfizetések között. További információkért lásd: [erőforrások toonew erőforráscsoportba vagy előfizetésbe áthelyezése](../../resource-group-move-resources.md).    

