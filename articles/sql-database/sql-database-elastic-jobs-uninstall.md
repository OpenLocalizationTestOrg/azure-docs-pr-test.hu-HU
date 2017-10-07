---
title: "aaaHow toouninstall rugalmas feladatok eszköz"
description: "Hogyan toouninstall hello rugalmas adatbázis-feladatok eszköze"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: bfc9d820-edbd-4fca-bfbf-1f339cfcc448
ms.service: sql-database
ms.workload: sql-database
ms.custom: scale out apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: 3bc1e889d5042bc3eaa9fd9da89816737e0b8df8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="uninstall-elastic-database-jobs-components"></a>A rugalmas adatbázis-feladatok összetevőinek eltávolítása
**Rugalmas adatbázis-feladatok** összetevők hello portál vagy a PowerShell használatával is eltávolíthatók.

## <a name="uninstall-elastic-database-jobs-components-using-hello-azure-portal"></a>Távolítsa el a rugalmas adatbázis-feladatok összetevőket hello Azure-portál használatával
1. Nyissa meg hello [Azure-portálon](https://portal.azure.com/).
2. Keresse meg a toohello előfizetés tartalmazó **rugalmas adatbázis-feladatok** összetevők, azaz hello előfizetés mely rugalmas adatbázis feladatok összetevők megtörtént-e.
3. Kattintson a **Tallózás** kattintson **erőforráscsoportok**.
4. Jelölje be hello "__ElasticDatabaseJob" nevű erőforráscsoport.
5. Hello csoport törléséhez.

## <a name="uninstall--elastic-database-jobs-components-using-powershell"></a>Távolítsa el a PowerShell használatával rugalmas adatbázis-feladatok összetevői
1. Indítsa el a Microsoft Azure PowerShell-parancsablakot, és keresse meg a toohello eszközök alkönyvtárát hello Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x mappában: típus **cd eszközök**.
   
     PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x* > cd-eszközök
2. Hajtsa végre hello.\UninstallElasticDatabaseJobs.ps1 PowerShell parancsfájlt.
   
     PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools > feloldása fájl.\UninstallElasticDatabaseJobs.ps1 PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools >. \ UninstallElasticDatabaseJobs.ps1

Vagy egyszerűen, hajtsa végre a következő parancsfájlt, feltéve, hogy alapértelmezett hello adott hello összetevők telepítéséhez használt értékek:

        $ResourceGroupName = "__ElasticDatabaseJob"
        Switch-AzureMode AzureResourceManager

        $resourceGroup = Get-AzureResourceGroup -Name $ResourceGroupName
        if(!$resourceGroup)
        {
            Write-Host "hello Azure Resource Group: $ResourceGroupName has already been deleted.  Elastic database job components are uninstalled."
            return
        }

        Write-Host "Removing hello Azure Resource Group: $ResourceGroupName.  This may take a few minutes.”
        Remove-AzureResourceGroup -Name $ResourceGroupName -Force
        Write-Host "Completed removing hello Azure Resource Group: $ResourceGroupName.  Elastic database job compoennts are now uninstalled."

## <a name="next-steps"></a>Következő lépések
toore-telepítés rugalmas feladatok, lásd: [hello rugalmas feladat szolgáltatás telepítése](sql-database-elastic-jobs-service-installation.md)

A rugalmas feladatok áttekintéséhez lásd: [rugalmas adatbázis-feladatok áttekintése](sql-database-elastic-jobs-overview.md).

<!--Image references-->


