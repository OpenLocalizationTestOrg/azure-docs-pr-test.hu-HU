---
title: "Az Azure PowerShell parancsfájl-Azure Cosmos DB feladatátvételi szabályzat létrehozása |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - Azure Cosmos DB feladatátvételi házirend létrehozása"
services: cosmos-db
documentationcenter: cosmosdb
author: mimig1
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: cosmos-db
ms.custom: mvc
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: cosmosdb
ms.workload: database
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: 16da3cd543ccbb7fe346261f91d2e9a3ceaf3a8b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-cosmos-db-failover-policy-for-high-availability-using-powershell"></a>A PowerShell használatával magas rendelkezésre állású Azure Cosmos DB feladatátvételi házirend létrehozása

A PowerShell-parancsfájlpélda a magas rendelkezésre állású egy feladatátvételi házirendhez tartozó Azure Cosmos DB hoz létre. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Mintaparancsfájl

[!code-powershell[fő](../../../powershell_scripts/cosmosdb/modify-failover-priority/modify-failover-priority.ps1?highlight=36-39,42-47 "Azure Cosmos DB DocumentDB API-fiók létrehozása")]

## <a name="clean-up-deployment"></a>Az üzemelő példány eltávolítása

A parancsfájl-minta futtatása után a következő parancs segítségével távolítsa el az erőforráscsoportot és a vele társított összes erőforrást.

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsokat. Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.

| Parancs | Megjegyzések |
|---|---|
| [Új-AzureRmResourceGroup](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/new-azurermresourcegroup) | Az összes erőforrás tároló erőforrás csoportot hoz létre. |
| [Új AzureRmResource](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresource?view=azurermps-3.8.0) | Adatbázis vagy a rugalmas készlet tároló logikai kiszolgáló létrehozása. |
| [Invoke-AzureRmResourceAction](https://docs.microsoft.com/powershell/module/azurerm.resources/invoke-azurermresourceaction?view=azurermps-3.8.0) | Meghívja az Azure CosmosDB fiók egy műveletet. |
| [Remove-AzureRmResourceGroup](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/remove-azurermresourcegroup) | Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése. |
|||

## <a name="next-steps"></a>Következő lépések

Az Azure PowerShell további információkért lásd: [Azure PowerShell dokumentációs](https://docs.microsoft.com/powershell/).

További Azure Cosmos DB PowerShell-parancsfájl példák találhatók a [Azure Cosmos DB PowerShell-parancsfájlok](../powershell-samples.md).