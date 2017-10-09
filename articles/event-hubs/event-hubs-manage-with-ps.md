---
title: "aaaUse PowerShell toomanage Azure Event Hubs erőforrások |} Microsoft Docs"
description: "PowerShell modul toocreate használatát és kezelését az Event Hubs"
services: event-hubs
documentationcenter: .NET
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: d79cb307c2b4a031d059ce6ca67117ffc0b4600b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-toomanage-event-hubs-resources"></a>PowerShell toomanage Event Hubs-erőforrások használata

A Microsoft Azure PowerShell egy parancsfájl-kezelési környezet, hogy toocontrol használ, és automatizálni hello telepítése és kezelése az Azure-szolgáltatásokhoz. Ez a cikk ismerteti, hogyan toouse hello [Event Hubs Resource Manager PowerShell-modul](/powershell/module/azurerm.eventhub) tooprovision és kezelheti az Event Hubs entitások (a névterek, a különálló eseményközpontokhoz és a fogyasztói csoportok) helyi Azure PowerShell-konzollal vagy parancsfájl.

Az Event Hubs-erőforrások Azure Resource Manager-sablonok segítségével is kezelheti. További információkért lásd: hello cikk [az Event Hubs-névtér létrehozása Azure Resource Manager-sablonnal event hub és fogyasztói csoport](event-hubs-resource-manager-namespace-event-hub.md).

## <a name="prerequisites"></a>Előfeltételek

Mielőtt elkezdené, hello következőkre lesz szüksége:

* Azure-előfizetés. Előfizetés beszerzésével kapcsolatos további információkért lásd: [beszerzési lehetőségek][purchase options], [ajánlatok][member offers], vagy [ingyenes fiókot][free account].
* Az Azure PowerShell számítógép. Útmutatásért lásd: [Ismerkedés az Azure PowerShell-parancsmagok](/powershell/azure/get-started-azureps).
* A PowerShell-parancsfájlok, a NuGet-csomagok és a .NET-keretrendszer hello általános ismertetése.

## <a name="get-started"></a>Bevezetés

hello első lépése pedig toouse PowerShell toolog tooyour Azure-fiókot az Azure-előfizetés. Hello utasításait követve [Ismerkedés az Azure PowerShell-parancsmagok](/powershell/azure/get-started-azureps) toolog a tooyour Azure-fiókra, majd beolvasása és az Azure-előfizetéshez hello erőforrásaihoz.

## <a name="provision-an-event-hubs-namespace"></a>Az Event Hubs névtér kiépítése

Az Event Hubs névterek használatakor hello használhatja [Get-AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/get-azurermeventhubnamespace), [New-AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/new-azurermeventhubnamespace), [Remove-AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/remove-azurermeventhubnamespace) , és [Set-AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/set-azurermeventhubnamespace) parancsmagok.

Ebben a példában néhány helyi változók hello parancsfájlban; hoz létre. `$Namespace` és `$Location`.

* `$Namespace`hello Event Hubs névteret, amelyhez toowork szeretnénk hello neve van.
* `$Location`azonosítja a hello adatközpont, amelyen kiépíti a Microsoft hello névtér.
* `$CurrentNamespace`hello hivatkozás névtér, amely azt lekérése (vagy hozzon létre) tárolja.

Egy tényleges parancsfájlban `$Namespace` és `$Location` paraméterként kell.

Ez a kijelző hello parancsfájl hello a következő:

1. Az Event Hubs névtér hello kísérletek tooretrieve adta meg.
2. Ha hello névtérben található, mi található jelenti.
3. Ha hello névtér nem található, akkor hello névteret hoz létre, és majd beolvassa az újonnan létrehozott névtér hello.

    ```powershell
    # Query toosee if hello namespace currently exists
    $CurrentNamespace = Get-AzureRMEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace
   
    # Check if hello namespace already exists or needs toobe created
    if ($CurrentNamespace)
    {
        Write-Host "hello namespace $Namespace already exists in hello $Location region:"
        # Report what was found
        Get-AzureRMEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace
    }
    else
    {
        Write-Host "hello $Namespace namespace does not exist."
        Write-Host "Creating hello $Namespace namespace in hello $Location region..."
        New-AzureRmEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace -Location $Location
        $CurrentNamespace = Get-AzureRMEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace
        Write-Host "hello $Namespace namespace in Resource Group $ResGrpName in hello $Location region has been successfully created."
    }
    ```

## <a name="create-an-event-hub"></a>Eseményközpont létrehozása

az eseményközpontok toocreate ellenőriznie a névtér hello parancsfájllal hello előző szakaszban. Ezután használja az hello [New-AzureRmEventHub](/powershell/module/azurerm.eventhub/new-azurermeventhub) parancsmag toocreate hello eseményközpont:

```powershell
# Check if event hub already exists
$CurrentEH = Get-AzureRMEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName

if($CurrentEH)
{
    Write-Host "hello event hub $EventHubName already exists in hello $Location region:"
    # Report what was found
    Get-AzureRmEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
}
else
{
    Write-Host "hello $EventHubName event hub does not exist."
    Write-Host "Creating hello $EventHubName event hub in hello $Location region..."
    New-AzureRmEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -Location $Location -MessageRetentionInDays 3
    $CurrentEH = Get-AzureRmEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
    Write-Host "hello $EventHubName event hub in Resource Group $ResGrpName in hello $Location region has been successfully created."
}
```

### <a name="create-a-consumer-group"></a>Egy felhasználói csoport létrehozása

egy fogyasztói csoporton belül az eseményközpontok toocreate hello névtér és az esemény hub ellenőrzések elvégzéséhez hello parancsfájlokkal hello előző szakaszban. Ezután használja az hello [New-AzureRmEventHubConsumerGroup](/powershell/module/azurerm.eventhub/new-azurermeventhubconsumergroup) parancsmag toocreate hello fogyasztói csoporton belül hello eseményközpontot. Példa:

```powershell
# Check if consumer group already exists
$CurrentCG = Get-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName -ErrorAction Ignore

if($CurrentCG)
{
    Write-Host "hello consumer group $ConsumerGroupName in event hub $EventHubName already exists in hello $Location region:"
    # Report what was found
    Get-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
}
else
{
    Write-Host "hello $ConsumerGroupName consumer group does not exist."
    Write-Host "Creating hello $ConsumerGroupName consumer group in hello $Location region..."
    New-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName
    $CurrentCG = Get-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
    Write-Host "hello $ConsumerGroupName consumer group in event hub $EventHubName in Resource Group $ResGrpName in hello $Location region has been successfully created."
}
```

#### <a name="set-user-metadata"></a>Készlet felhasználói metaadatok

Után hello parancsfájlok végrehajtása az előző szakaszokban hello, használhatja a hello [Set-AzureRmEventHubConsumerGroup](/powershell/module/azurerm.eventhub/set-azurermeventhubconsumergroup) parancsmag tooupdate hello tulajdonságait egy felhasználói csoport, mint például a következő hello:

```powershell
# Set some user metadata on hello CG
Write-Host "Setting hello UserMetadata field too'Testing'"
Set-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName -UserMetadata "Testing"
# Show result
Get-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName
```

## <a name="remove-event-hub"></a>Távolítsa el az event hubs

tooremove hello event hubs létrehozott hello használható `Remove-*` parancsmagok, mint például a következő hello:

```powershell
# Clean up
Remove-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName
Remove-AzureRmEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
Remove-AzureRmEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace
```

## <a name="next-steps"></a>Következő lépések

- A dokumentációban hello teljes Event Hubs Resource Manager PowerShell modul [Itt](/powershell/module/azurerm.eventhub). Ezen a lapon az összes elérhető parancsmagok sorolja fel.
- Azure Resource Manager-sablonok használatával kapcsolatos információkért lásd: hello cikk [az Event Hubs-névtér létrehozása Azure Resource Manager-sablonnal event hub és fogyasztói csoport](event-hubs-resource-manager-namespace-event-hub.md).
- Információ a [Event Hubs .NET kezelési kódtárakat](event-hubs-management-libraries.md).

[purchase options]: http://azure.microsoft.com/pricing/purchase-options/
[member offers]: http://azure.microsoft.com/pricing/member-offers/
[free account]: http://azure.microsoft.com/pricing/free-trial/
