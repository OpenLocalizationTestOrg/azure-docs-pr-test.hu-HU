---
title: "aaaPowerShell példa aktív georedundáns replikáció készletezett Azure SQL adatbázis |} Microsoft Docs"
description: "Az Azure PowerShell példa parancsfájl tooset be aktív georeplikáció készletezett Azure SQL-adatbázis"
services: sql-database
documentationcenter: sql-database
author: CarlRabeler
manager: jhubbard
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: business continuity
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 07/25/2017
ms.author: carlrab
ms.openlocfilehash: 9d183f08dcc07ba864e42fe70a562fef8bd572f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-tooconfigure-active-geo-replication-for-a-pooled-azure-sql-database"></a><span data-ttu-id="405aa-103">PowerShell tooconfigure aktív georeplikáció készletezett Azure SQL-adatbázis használata</span><span class="sxs-lookup"><span data-stu-id="405aa-103">Use PowerShell tooconfigure active geo-replication for a pooled Azure SQL database</span></span>

<span data-ttu-id="405aa-104">A PowerShell-mintaparancsfájl konfigurálja az Azure SQL-adatbázis aktív georeplikáció a rugalmas készletekben található, és átadja azt toohello hello Azure SQL adatbázis másodlagos replikája.</span><span class="sxs-lookup"><span data-stu-id="405aa-104">This PowerShell script example configures active geo-replication for an Azure SQL database in an elastic pool and fails it over toohello secondary replica of hello Azure SQL database.</span></span>

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-scripts"></a><span data-ttu-id="405aa-105">Mintaszkriptek</span><span class="sxs-lookup"><span data-stu-id="405aa-105">Sample scripts</span></span>

[!code-powershell[main](../../../powershell_scripts/sql-database/setup-geodr-and-failover-pool/setup-geodr-and-failover-pool.ps1?highlight=16-19 "Set up active geo-replication for elastic pool")]

## <a name="clean-up-deployment"></a><span data-ttu-id="405aa-106">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="405aa-106">Clean up deployment</span></span>

<span data-ttu-id="405aa-107">Hello parancsfájl minta futtatása után a következő parancs hello lehet használt tooremove hello erőforráscsoport és a vele társított összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="405aa-107">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myPrimaryResourceGroup"
Remove-AzureRmResourceGroup -ResourceGroupName "mySecondaryResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="405aa-108">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="405aa-108">Script explanation</span></span>

<span data-ttu-id="405aa-109">A parancsfájl a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="405aa-109">This script uses hello following commands.</span></span> <span data-ttu-id="405aa-110">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="405aa-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="405aa-111">Parancs</span><span class="sxs-lookup"><span data-stu-id="405aa-111">Command</span></span> | <span data-ttu-id="405aa-112">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="405aa-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="405aa-113">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="405aa-113">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="405aa-114">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="405aa-114">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="405aa-115">Új AzureRmSqlServer</span><span class="sxs-lookup"><span data-stu-id="405aa-115">New-AzureRmSqlServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="405aa-116">Adatbázis vagy a rugalmas készlet tároló logikai kiszolgáló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="405aa-116">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="405aa-117">Új-AzureRmSqlElasticPool</span><span class="sxs-lookup"><span data-stu-id="405aa-117">New-AzureRmSqlElasticPool</span></span>](/powershell/module/azurerm.sql/new-azurermsqlelasticpool) | <span data-ttu-id="405aa-118">Létrehoz egy logikai kiszolgáló belül rugalmas készlethez.</span><span class="sxs-lookup"><span data-stu-id="405aa-118">Creates an elastic pool within a logical server.</span></span> |
| [<span data-ttu-id="405aa-119">New-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="405aa-119">New-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabase) | <span data-ttu-id="405aa-120">Adatbázist hoz létre egy logikai egyetlen vagy egy készletezett adatbázis-kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="405aa-120">Creates a database in a logical server as a single or a pooled database.</span></span> |
| [<span data-ttu-id="405aa-121">Set-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="405aa-121">Set-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/set-azurermsqldatabase) | <span data-ttu-id="405aa-122">Frissíti az adatbázis-tulajdonságai, vagy azokat, kívüli vagy rugalmas készletek között adatbázis helyezi.</span><span class="sxs-lookup"><span data-stu-id="405aa-122">Updates database properties or moves a database into, out of, or between elastic pools.</span></span> |
| [<span data-ttu-id="405aa-123">Új AzureRmSqlDatabaseSecondary</span><span class="sxs-lookup"><span data-stu-id="405aa-123">New-AzureRmSqlDatabaseSecondary</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabasesecondary)| <span data-ttu-id="405aa-124">Létrehoz egy meglévő adatbázis másodlagos adatbázist, és elindítja a adatreplikáció.</span><span class="sxs-lookup"><span data-stu-id="405aa-124">Creates a secondary database for an existing database and starts data replication.</span></span> |
| [<span data-ttu-id="405aa-125">Get-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="405aa-125">Get-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/get-azurermsqldatabase)| <span data-ttu-id="405aa-126">Egy vagy több adatbázis lekérdezi.</span><span class="sxs-lookup"><span data-stu-id="405aa-126">Gets one or more databases.</span></span> |
| [<span data-ttu-id="405aa-127">Set-AzureRmSqlDatabaseSecondary</span><span class="sxs-lookup"><span data-stu-id="405aa-127">Set-AzureRmSqlDatabaseSecondary</span></span>](/powershell/module/azurerm.sql/set-azurermsqldatabasesecondary)| <span data-ttu-id="405aa-128">Egy másodlagos adatbázis toobe elsődleges kapcsolók tooinitiate feladatátvételi sorrendje.</span><span class="sxs-lookup"><span data-stu-id="405aa-128">Switches a secondary database toobe primary in order tooinitiate failover.</span></span>|
| [<span data-ttu-id="405aa-129">Get-AzureRmSqlDatabaseReplicationLink</span><span class="sxs-lookup"><span data-stu-id="405aa-129">Get-AzureRmSqlDatabaseReplicationLink</span></span>](/powershell/module/azurerm.sql/get-azurermsqldatabasereplicationlink) | <span data-ttu-id="405aa-130">Lekérdezi a hello földrajzi-replikációs hivatkozások az Azure SQL Database és egy erőforráscsoport vagy SQL Server között.</span><span class="sxs-lookup"><span data-stu-id="405aa-130">Gets hello geo-replication links between an Azure SQL Database and a resource group or SQL Server.</span></span> |
| [<span data-ttu-id="405aa-131">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="405aa-131">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="405aa-132">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="405aa-132">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="405aa-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="405aa-133">Next steps</span></span>

<span data-ttu-id="405aa-134">Az Azure PowerShell hello további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="405aa-134">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="405aa-135">További SQL Database PowerShell parancsfájl minták hello található [Azure SQL Database PowerShell-parancsfájlok](../sql-database-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="405aa-135">Additional SQL Database PowerShell script samples can be found in hello [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).</span></span>
