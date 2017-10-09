---
title: "aaaHow tootag egy Windows virtuális gép erőforrásához az Azure-ban |} Microsoft Docs"
description: "További tudnivalók az Azure-ban hello Resource Manager üzembe helyezési modellben létrehozott Windows virtuális gépek címkézés"
services: virtual-machines-windows
documentationcenter: 
author: mmccrory
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 56d17f45-e4a7-4d84-8022-b40334ae49d2
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/05/2016
ms.author: memccror
ms.openlocfilehash: 160416ddc35998b3c98c6e579668a6a5eb6de6e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tootag-a-windows-virtual-machine-in-azure"></a>Hogyan tootag Windows virtuális gépként az Azure-ban
Ez a cikk ismerteti a különböző módokon tootag Windows virtuális gépként az Azure-ban keresztül hello Resource Manager üzembe helyezési modellben. Címke található a felhasználó által definiált kulcs/érték párok, amely lehet tenni közvetlenül egy erőforrás vagy egy erőforráscsoportot. Azure jelenleg legfeljebb too15 címkék erőforrás pedig erőforráscsoportban támogat. Címke lehet, hogy helyezni egy erőforrás létrehozása a hello időpontjában, vagy hozzá tooan meglévő erőforrást. Vegye figyelembe, hogy az erőforrások hello Resource Manager üzembe helyezési modellben csak létrehozott címkék használhatók. Ha azt szeretné, hogy a Linux virtuális gépek tootag, [hogyan tootag egy Linux virtuális gép az Azure-ban](../linux/tag.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

[!INCLUDE [virtual-machines-common-tag](../../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-powershell"></a>A PowerShell-címkézés
toocreate, adja hozzá, és a PowerShell segítségével címkék törlése, tooset kell fel a [PowerShell-környezet az Azure Resource Manager][PowerShell environment with Azure Resource Manager]. Hello telepítő befejezése után a számítási, hálózati és tárolási erőforrásokat a létrehozásakor vagy PowerShell hello erőforrás létrehozása után elhelyezheti címkék. Ez a cikk megtekintéséhez vagy szerkesztéséhez címkéket a virtuális gépek összpontosít.

Először keresse meg a virtuális gép tooa keresztül hello `Get-AzureRmVM` parancsmag.

        PS C:\> Get-AzureRmVM -ResourceGroupName "MyResourceGroup" -Name "MyTestVM"

Ha a virtuális gép már tartalmazza a címkék, majd megjelenik minden hello címkék az erőforrás:

        Tags : {
                "Application": "MyApp1",
                "Created By": "MyName",
                "Department": "MyDepartment",
                "Environment": "Production"
               }

Ha szeretné tooadd címkék a PowerShell segítségével, használhatja a hello `Set-AzureRmResource` parancsot. Vegye figyelembe a PowerShell segítségével, címkék frissítésekor címkék egész frissülnek. Így címke tooa egy erőforrást, amely már a címkét ad hozzá, ha szüksége lesz tooinclude toobe hello erőforrás elhelyezni kívánt összes hello címkét. Az alábbiakban példája hogyan tooadd további címkék tooa erőforrás PowerShell parancsmagokon keresztül.

Ez a parancsmag első beállítja hello címkék helyezve *MyTestVM* toohello *$tags* változó hello segítségével `Get-AzureRmResource` és `Tags` tulajdonság.

        PS C:\> $tags = (Get-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM).Tags

hello második parancs megjeleníti az adott változó hello hello címkék.

        PS C:\> $tags

        Name        Value
        ----                           -----
        Value        MyDepartment
        Name        Department
        Value        MyApp1
        Name        Application
        Value        MyName
        Name        Created By
        Value        Production
        Name        Environment

hello harmadik parancs hozzáadja egy további címke toohello *$tags* változó. Vegye figyelembe a hello hello használata  **+=**  tooappend hello új kulcs/érték pár toohello *$tags* listája.

        PS C:\> $tags += @{Name="Location";Value="MyLocation"}

hello negyedik parancs beállítja a hello definiált hello címkék *$tags* változó toohello resource biztosítani. Ebben az esetben is MyTestVM.

        PS C:\> Set-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM -ResourceType "Microsoft.Compute/VirtualMachines" -Tag $tags

hello ötödik parancs megjeleníti hello címkék hello erőforrás. Ahogy látja, *hely* most egy címkét a típusúként van definiálva *MyLocation* hello értékként.

        PS C:\> (Get-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM).Tags

        Name        Value
        ----                           -----
        Value        MyDepartment
        Name        Department
        Value        MyApp1
        Name        Application
        Value        MyName
        Name        Created By
        Value        Production
        Name        Environment
        Value        MyLocation
        Name        Location

További részletek toolearn címkézése a PowerShell segítségével, tekintse meg hello [Azure Resource parancsmagok][Azure Resource Cmdlets].

[!INCLUDE [virtual-machines-common-tag-usage](../../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a>Következő lépések
* toolearn az Azure-erőforrások, címkézéssel kapcsolatos további információkért lásd: [Azure Resource Manager áttekintése] [ Azure Resource Manager Overview] és [tooorganize címkék használata az Azure-erőforrások] [ Using Tags tooorganize your Azure Resources].
* toosee hogyan címkék segítségével kezelése az Azure-erőforrások, lásd: [ismertetése a Azure számlázási] [ Understanding your Azure Bill] és [betekintést nyerhet a Microsoft Azure erőforrás-felhasználás] [Gain insights into your Microsoft Azure resource consumption].

[PowerShell environment with Azure Resource Manager]: ../../azure-resource-manager/powershell-azure-resource-manager.md
[Azure Resource Cmdlets]: https://msdn.microsoft.com/library/azure/dn757692.aspx
[Azure Resource Manager Overview]: ../../azure-resource-manager/resource-group-overview.md
[Using Tags tooorganize your Azure Resources]: ../../azure-resource-manager/resource-group-using-tags.md
[Understanding your Azure Bill]: ../../billing/billing-understand-your-bill.md
[Gain insights into your Microsoft Azure resource consumption]: ../../billing/billing-usage-rate-card-overview.md
