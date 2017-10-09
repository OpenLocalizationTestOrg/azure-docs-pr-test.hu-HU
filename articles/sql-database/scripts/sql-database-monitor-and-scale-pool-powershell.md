---
title: "aaaPowerShell példa-figyelő-skálázási-SQL rugalmas készlet-Azure SQL Database |} Microsoft Docs"
description: "Az Azure PowerShell-példa toomonitor parancsfájlt, és a rugalmas SQL-készletet, az Azure SQL-adatbázis méretezése"
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 06/23/2017
ms.author: janeng
ms.openlocfilehash: 149a45174ccb8072ea21753364196c7f98fd4101
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-toomonitor-and-scale-a-sql-elastic-pool-in-azure-sql-database"></a>PowerShell toomonitor használja, és a rugalmas SQL-készletet, az Azure SQL-adatbázis méretezése

A PowerShell-mintaparancsfájl figyelők hello teljesítménymutatók egy rugalmas készlet tooa magasabb teljesítményszintre méretezi azt, és hello teljesítménymutatók egyik egy riasztási szabályt hoz létre. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Mintaparancsfájl

[!code-powershell[main](../../../powershell_scripts/sql-database/monitor-and-scale-pool/monitor-and-scale-pool.ps1?highlight=16-17 "Monitor and scale single SQL Database")]

## <a name="clean-up-deployment"></a>Az üzemelő példány eltávolítása

Hello parancsfájl minta futtatása után a következő parancs hello lehet használt tooremove hello erőforráscsoport és a vele társított összes erőforrást.

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsok hello. Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.

| Parancs | Megjegyzések |
|---|---|
 [Új-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Az összes erőforrás tároló erőforrás csoportot hoz létre. |
| [Új AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver) | Adatbázis vagy a rugalmas készlet tároló logikai kiszolgáló létrehozása. |
| [Új-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/new-azurermsqlelasticpool) | Létrehoz egy logikai kiszolgáló belül rugalmas készlethez. |
| [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) | Adatbázist hoz létre egy logikai egyetlen vagy egy készletezett adatbázis-kiszolgálót. |
| [Get-AzureRmMetric](/powershell/module/azurerm.insights/get-azurermmetric) | Hello hello adatbázis mérete használati információit jeleníti meg.|
| [Adja hozzá AzureRMMetricAlertRule](/powershell/module/azurerm.insights/add-azurermmetricalertrule) | A metrika-alapú riasztási szabály frissítése vagy hozzáadása. |
| [Set-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/set-azurermsqlelasticpool) | Frissítés a rugalmas készlet tulajdonságai |
| [Adja hozzá AzureRMMetricAlertRule](/powershell/module/azurerm.insights/add-azurermmetricalertrule) | Beállítja egy riasztási szabály tooautomatically figyelő dtu-k hello jövőbeli. |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése. |
|||

## <a name="next-steps"></a>Következő lépések

Az Azure PowerShell hello további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).

További SQL Database PowerShell parancsfájl minták hello található [Azure SQL Database PowerShell-parancsfájlok](../sql-database-powershell-samples.md).
