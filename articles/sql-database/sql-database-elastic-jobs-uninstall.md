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
# <a name="uninstall-elastic-database-jobs-components"></a><span data-ttu-id="d0757-103">A rugalmas adatbázis-feladatok összetevőinek eltávolítása</span><span class="sxs-lookup"><span data-stu-id="d0757-103">Uninstall Elastic Database jobs components</span></span>
<span data-ttu-id="d0757-104">**Rugalmas adatbázis-feladatok** összetevők hello portál vagy a PowerShell használatával is eltávolíthatók.</span><span class="sxs-lookup"><span data-stu-id="d0757-104">**Elastic Database jobs** components can be uninstalled using either hello Portal or PowerShell.</span></span>

## <a name="uninstall-elastic-database-jobs-components-using-hello-azure-portal"></a><span data-ttu-id="d0757-105">Távolítsa el a rugalmas adatbázis-feladatok összetevőket hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="d0757-105">Uninstall Elastic Database jobs components using hello Azure portal</span></span>
1. <span data-ttu-id="d0757-106">Nyissa meg hello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="d0757-106">Open hello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="d0757-107">Keresse meg a toohello előfizetés tartalmazó **rugalmas adatbázis-feladatok** összetevők, azaz hello előfizetés mely rugalmas adatbázis feladatok összetevők megtörtént-e.</span><span class="sxs-lookup"><span data-stu-id="d0757-107">Navigate toohello subscription that contains **Elastic Database jobs** components, namely hello subscription in which Elastic Database jobs components were installed.</span></span>
3. <span data-ttu-id="d0757-108">Kattintson a **Tallózás** kattintson **erőforráscsoportok**.</span><span class="sxs-lookup"><span data-stu-id="d0757-108">Click **Browse** and click **Resource groups**.</span></span>
4. <span data-ttu-id="d0757-109">Jelölje be hello "__ElasticDatabaseJob" nevű erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="d0757-109">Select hello resource group named "__ElasticDatabaseJob".</span></span>
5. <span data-ttu-id="d0757-110">Hello csoport törléséhez.</span><span class="sxs-lookup"><span data-stu-id="d0757-110">Delete hello resource group.</span></span>

## <a name="uninstall--elastic-database-jobs-components-using-powershell"></a><span data-ttu-id="d0757-111">Távolítsa el a PowerShell használatával rugalmas adatbázis-feladatok összetevői</span><span class="sxs-lookup"><span data-stu-id="d0757-111">Uninstall  Elastic Database jobs components using PowerShell</span></span>
1. <span data-ttu-id="d0757-112">Indítsa el a Microsoft Azure PowerShell-parancsablakot, és keresse meg a toohello eszközök alkönyvtárát hello Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x mappában: típus **cd eszközök**.</span><span class="sxs-lookup"><span data-stu-id="d0757-112">Launch a Microsoft Azure PowerShell command window and navigate toohello tools sub-directory under hello Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x folder: Type **cd tools**.</span></span>
   
     <span data-ttu-id="d0757-113">PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x* > cd-eszközök</span><span class="sxs-lookup"><span data-stu-id="d0757-113">PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*>cd tools</span></span>
2. <span data-ttu-id="d0757-114">Hajtsa végre hello.\UninstallElasticDatabaseJobs.ps1 PowerShell parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="d0757-114">Execute hello .\UninstallElasticDatabaseJobs.ps1 PowerShell script.</span></span>
   
     <span data-ttu-id="d0757-115">PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools > feloldása fájl.\UninstallElasticDatabaseJobs.ps1 PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools >. \ UninstallElasticDatabaseJobs.ps1</span><span class="sxs-lookup"><span data-stu-id="d0757-115">PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>Unblock-File .\UninstallElasticDatabaseJobs.ps1   PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>.\UninstallElasticDatabaseJobs.ps1</span></span>

<span data-ttu-id="d0757-116">Vagy egyszerűen, hajtsa végre a következő parancsfájlt, feltéve, hogy alapértelmezett hello adott hello összetevők telepítéséhez használt értékek:</span><span class="sxs-lookup"><span data-stu-id="d0757-116">Or simply, execute hello following script, assuming default values where used on installation of hello components:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="d0757-117">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d0757-117">Next steps</span></span>
<span data-ttu-id="d0757-118">toore-telepítés rugalmas feladatok, lásd: [hello rugalmas feladat szolgáltatás telepítése](sql-database-elastic-jobs-service-installation.md)</span><span class="sxs-lookup"><span data-stu-id="d0757-118">toore-install Elastic Database jobs, see [Installing hello Elastic Database job service](sql-database-elastic-jobs-service-installation.md)</span></span>

<span data-ttu-id="d0757-119">A rugalmas feladatok áttekintéséhez lásd: [rugalmas adatbázis-feladatok áttekintése](sql-database-elastic-jobs-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d0757-119">For an overview of Elastic Database jobs, see [Elastic Database jobs overview](sql-database-elastic-jobs-overview.md).</span></span>

<!--Image references-->


