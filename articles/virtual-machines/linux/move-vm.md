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
# <a name="move-a-linux-vm-tooanother-subscription-or-resource-group"></a>Linux virtuális gép tooanother előfizetés vagy az erőforrás csoport áthelyezése
Ez a cikk végigvezeti toomove Linux virtuális gép erőforráscsoportok vagy előfizetések között. A virtuális gépek áthelyezése másik előfizetések lehet hasznos, ha létrehozott egy virtuális Gépet egy személyes előfizetésben és most szeretné, hogy toomove azt tooyour vállalat előfizetésének.

> [!IMPORTANT]
>Felügyelt lemezek jelenleg nem helyezhető át. 
>
>Új erőforrás-azonosítók hello áthelyezés részeként jönnek létre. Miután hello virtuális gép át lett helyezve, kell tooupdate az eszközök és parancsfájlok toouse hello új erőforrás-azonosítók. 
> 
> 

## <a name="use-hello-azure-cli-toomove-a-vm"></a>Hello Azure CLI toomove a virtuális gépek használata
toosuccessfully helyezze át a virtuális Gépet, toomove hello virtuális gép és annak támogató forrásokat van szükség. Használjon hello **azure-csoportok megjelenítése** toolist az erőforráscsoportot és az azonosítók hello erőforrásainak parancsot. Így másolja és illessze be a hello azonosítók a későbbi parancsok toopipe hello kimeneti parancs tooa fájl segítségével.

    azure group show <resourceGroupName>

toomove a virtuális gépek és az erőforrások tooanother erőforráscsoport használata hello **az azure erőforrás-áthelyezés** CLI parancsot. hello a következő példa bemutatja, hogyan toomove a virtuális gépek és hello leggyakrabban használt erőforrásokat igényel. Hello használjuk **-i** paraméter és egy vesszővel tagolt listán (szóközök nélkül) azonosítókat hello erőforrások toomove fázisban.

    vm=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Compute/virtualMachines/<vmName>
    nic=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/networkInterfaces/<nicName>
    nsg=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/networkSecurityGroups/<nsgName>
    pip=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/publicIPAddresses/<publicIPName>
    vnet=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/virtualNetworks/<vnetName>
    diag=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Storage/storageAccounts/<diagnosticStorageAccountName>
    storage=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Storage/storageAccounts/<storageAcountName>      

    azure resource move --ids $vm,$nic,$nsg,$pip,$vnet,$storage,$diag -d "<destinationResourceGroup>"

Ha azt szeretné, hogy toomove hello a virtuális gép és az erőforrások tooa másik előfizetést, vegye fel a hello **– cél-subscriptionId &#60; destinationSubscriptionID &#62;** paraméter toospecify hello célelőfizetés.

Hello parancssort a Windows-számítógépen dolgozik, ha szüksége van-e tooadd egy  **$**  hello változók nevében, ha azok deklarálhatja elé. Ez a Linux nem szükséges.

A rendszer felkéri, hogy szeretné-e toomove hello tooconfirm megadott erőforrás. Típus **Y** tooconfirm, amelyet az toomove hello erőforrásokat.

[!INCLUDE [virtual-machines-common-move-vm](../../../includes/virtual-machines-common-move-vm.md)]

## <a name="next-steps"></a>Következő lépések
Számos különböző típusú erőforrások áthelyezheti erőforráscsoport-sablonok és előfizetések között. További információkért lásd: [erőforrások toonew erőforráscsoportba vagy előfizetésbe áthelyezése](../../resource-group-move-resources.md).    

