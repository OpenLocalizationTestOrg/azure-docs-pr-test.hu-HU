---
title: "A rugalmas adatbázis-feladatok eszköz eltávolítása"
description: "A rugalmas feladatok eszköz eltávolítása"
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
ms.openlocfilehash: ae7f0bce452a0a86f6f1e4d9b0c93a0fa1727f21
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="uninstall-elastic-database-jobs-components"></a><span data-ttu-id="7a79a-103">A rugalmas adatbázis-feladatok összetevőinek eltávolítása</span><span class="sxs-lookup"><span data-stu-id="7a79a-103">Uninstall Elastic Database jobs components</span></span>
<span data-ttu-id="7a79a-104">**Rugalmas adatbázis-feladatok** összetevőket is el kell távolítani a portál vagy a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="7a79a-104">**Elastic Database jobs** components can be uninstalled using either the Portal or PowerShell.</span></span>

## <a name="uninstall-elastic-database-jobs-components-using-the-azure-portal"></a><span data-ttu-id="7a79a-105">Az Azure portál használatával rugalmas adatbázis-feladatok összetevőinek eltávolítása</span><span class="sxs-lookup"><span data-stu-id="7a79a-105">Uninstall Elastic Database jobs components using the Azure portal</span></span>
1. <span data-ttu-id="7a79a-106">Nyissa meg az [Azure portált](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="7a79a-106">Open the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="7a79a-107">Keresse meg az előfizetés tartalmazó **rugalmas adatbázis-feladatok** összetevők, nevezetesen az előfizetés, mely rugalmas adatbázis feladatok összetevők megtörtént-e.</span><span class="sxs-lookup"><span data-stu-id="7a79a-107">Navigate to the subscription that contains **Elastic Database jobs** components, namely the subscription in which Elastic Database jobs components were installed.</span></span>
3. <span data-ttu-id="7a79a-108">Kattintson a **Tallózás** kattintson **erőforráscsoportok**.</span><span class="sxs-lookup"><span data-stu-id="7a79a-108">Click **Browse** and click **Resource groups**.</span></span>
4. <span data-ttu-id="7a79a-109">Válassza ki a "__ElasticDatabaseJob" nevű erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="7a79a-109">Select the resource group named "__ElasticDatabaseJob".</span></span>
5. <span data-ttu-id="7a79a-110">Törölje a csoportot.</span><span class="sxs-lookup"><span data-stu-id="7a79a-110">Delete the resource group.</span></span>

## <a name="uninstall--elastic-database-jobs-components-using-powershell"></a><span data-ttu-id="7a79a-111">Távolítsa el a PowerShell használatával rugalmas adatbázis-feladatok összetevői</span><span class="sxs-lookup"><span data-stu-id="7a79a-111">Uninstall  Elastic Database jobs components using PowerShell</span></span>
1. <span data-ttu-id="7a79a-112">Indítsa el a Microsoft Azure PowerShell-parancsablakot, és keresse meg az eszközök alkönyvtárát Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x mappában lévő: típus **cd eszközök**.</span><span class="sxs-lookup"><span data-stu-id="7a79a-112">Launch a Microsoft Azure PowerShell command window and navigate to the tools sub-directory under the Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x folder: Type **cd tools**.</span></span>
   
     <span data-ttu-id="7a79a-113">PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x* > cd-eszközök</span><span class="sxs-lookup"><span data-stu-id="7a79a-113">PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*>cd tools</span></span>
2. <span data-ttu-id="7a79a-114">A PowerShell-parancsfájl.\UninstallElasticDatabaseJobs.ps1 hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="7a79a-114">Execute the .\UninstallElasticDatabaseJobs.ps1 PowerShell script.</span></span>
   
     <span data-ttu-id="7a79a-115">PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools > feloldása fájl.\UninstallElasticDatabaseJobs.ps1 PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools >. \ UninstallElasticDatabaseJobs.ps1</span><span class="sxs-lookup"><span data-stu-id="7a79a-115">PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>Unblock-File .\UninstallElasticDatabaseJobs.ps1   PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>.\UninstallElasticDatabaseJobs.ps1</span></span>

<span data-ttu-id="7a79a-116">Vagy egyszerűen, hajtsa végre a következő parancsfájlt, ha az alapértelmezett értékeket, ha az összetevők telepítéséhez használja:</span><span class="sxs-lookup"><span data-stu-id="7a79a-116">Or simply, execute the following script, assuming default values where used on installation of the components:</span></span>

        $ResourceGroupName = "__ElasticDatabaseJob"
        Switch-AzureMode AzureResourceManager

        $resourceGroup = Get-AzureResourceGroup -Name $ResourceGroupName
        if(!$resourceGroup)
        {
            Write-Host "The Azure Resource Group: $ResourceGroupName has already been deleted.  Elastic database job components are uninstalled."
            return
        }

        Write-Host "Removing the Azure Resource Group: $ResourceGroupName.  This may take a few minutes.”
        Remove-AzureResourceGroup -Name $ResourceGroupName -Force
        Write-Host "Completed removing the Azure Resource Group: $ResourceGroupName.  Elastic database job compoennts are now uninstalled."

## <a name="next-steps"></a><span data-ttu-id="7a79a-117">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7a79a-117">Next steps</span></span>
<span data-ttu-id="7a79a-118">Telepítse újra a rugalmas feladatok, lásd: [a rugalmas adatbázis feladat szolgáltatás telepítése](sql-database-elastic-jobs-service-installation.md)</span><span class="sxs-lookup"><span data-stu-id="7a79a-118">To re-install Elastic Database jobs, see [Installing the Elastic Database job service](sql-database-elastic-jobs-service-installation.md)</span></span>

<span data-ttu-id="7a79a-119">A rugalmas feladatok áttekintéséhez lásd: [rugalmas adatbázis-feladatok áttekintése](sql-database-elastic-jobs-overview.md).</span><span class="sxs-lookup"><span data-stu-id="7a79a-119">For an overview of Elastic Database jobs, see [Elastic Database jobs overview](sql-database-elastic-jobs-overview.md).</span></span>

<!--Image references-->


