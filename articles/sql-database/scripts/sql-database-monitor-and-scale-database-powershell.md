---
title: "PowerShell-példa-figyelő-skálázási-egyetlen Azure SQL-adatbázis |} Microsoft Docs"
description: "Az Azure PowerShell-példa parancsfájl figyelését, valamint egy Azure SQL-adatbázis méretezése"
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
ms.openlocfilehash: 5d08335f4b1d6c5c6a120cbfb86ab2c62b79bb9f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="use-powershell-to-monitor-and-scale-a-single-sql-database"></a>PowerShell segítségével figyelheti, és egy SQL-adatbázis méretezése

A PowerShell-mintaparancsfájl figyeli a metrikák egy adatbázis méretezi a magasabb teljesítményszintre és egy riasztási szabályt hoz létre a Teljesítményelemzési értékek egyike. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Mintaparancsfájl

[!code-powershell[fő](../../../powershell_scripts/sql-database/monitor-and-scale-database/monitor-and-scale-database.ps1?highlight=13-14 "figyelő és a skála egyetlen SQL-adatbázis")]

## <a name="clean-up-deployment"></a>Az üzemelő példány eltávolítása

A parancsfájl-minta futtatása után a következő parancs segítségével távolítsa el az erőforráscsoportot és a vele társított összes erőforrást.

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsokat. Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.

| Parancs | Megjegyzések |
|---|---|
 [Új-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Az összes erőforrás tároló erőforrás csoportot hoz létre. |
| [Új AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver) | Adatbázis vagy a rugalmas készlet tároló logikai kiszolgáló létrehozása. |
| [Get-AzureRmMetric](/powershell/module/azurerm.insights/get-azurermmetric) | Az az adatbázis méretének használati információit jeleníti meg.|
| [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase) | Frissíti az adatbázis-tulajdonságai, vagy azokat, kívüli vagy rugalmas készletek között adatbázis helyezi. |
| [Adja hozzá AzureRMMetricAlertRule](/powershell/module/azurerm.insights/add-azurermmetricalertrule) | Beállítja az automatikusan figyelése a jövőben a dtu-k riasztási szabályt. |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése. |
|||

## <a name="next-steps"></a>Következő lépések

Az Azure PowerShell további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).

További SQL Database PowerShell parancsfájl minták megtalálhatók a [Azure SQL Database PowerShell-parancsfájlok](../sql-database-powershell-samples.md).
