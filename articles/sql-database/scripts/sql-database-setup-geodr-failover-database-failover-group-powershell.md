---
title: "aaaPowerShell példa georeplikációt feladatátvételi csoport-egyetlen Azure SQL Database |} Microsoft Docs"
description: "Az Azure PowerShell példa parancsfájl tooset be aktív georeplikáció egyetlen Azure SQL-adatbázis"
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: business continuity
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 06/23/2017
ms.author: janeng
ms.openlocfilehash: 0ea7afb1125b95370811ef7f80cb9eb7391b2443
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-tooconfigure-an-active-geo-replication-failover-group-for-a-single-azure-sql-database"></a><span data-ttu-id="c5f6b-103">PowerShell tooconfigure egy aktív georeplikáció feladatátvételi csoport egyetlen Azure SQL-adatbázis használata</span><span class="sxs-lookup"><span data-stu-id="c5f6b-103">Use PowerShell tooconfigure an active geo-replication failover group for a single Azure SQL database</span></span>

<span data-ttu-id="c5f6b-104">A PowerShell-mintaparancsfájl egy aktív georeplikáció feladatátvételi csoport egyetlen Azure SQL-adatbázis konfigurálja, és átadja azt tooa hello Azure SQL adatbázis másodlagos replikája.</span><span class="sxs-lookup"><span data-stu-id="c5f6b-104">This PowerShell script example configures an active geo-replication failover group for a single Azure SQL database and fails it over tooa secondary replica of hello Azure SQL database.</span></span>

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-scripts"></a><span data-ttu-id="c5f6b-105">Mintaszkriptek</span><span class="sxs-lookup"><span data-stu-id="c5f6b-105">Sample scripts</span></span>

[!code-powershell[main](../../../powershell_scripts/sql-database/setup-geodr-and-failover-database/setup-geodr-and-failover-database-failover-group.ps1?highlight=19-22 "Set up failover group for single database")]

## <a name="clean-up-deployment"></a><span data-ttu-id="c5f6b-106">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="c5f6b-106">Clean up deployment</span></span>

<span data-ttu-id="c5f6b-107">Hello parancsfájl minta futtatása után a következő parancs hello lehet használt tooremove hello erőforráscsoport és a vele társított összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="c5f6b-107">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myPrimaryResourceGroup"
Remove-AzureRmResourceGroup -ResourceGroupName "mySecondaryResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="c5f6b-108">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="c5f6b-108">Script explanation</span></span>

<span data-ttu-id="c5f6b-109">A parancsfájl a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="c5f6b-109">This script uses hello following commands.</span></span> <span data-ttu-id="c5f6b-110">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="c5f6b-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="c5f6b-111">Parancs</span><span class="sxs-lookup"><span data-stu-id="c5f6b-111">Command</span></span> | <span data-ttu-id="c5f6b-112">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="c5f6b-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="c5f6b-113">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="c5f6b-113">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="c5f6b-114">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="c5f6b-114">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="c5f6b-115">Új AzureRmSqlServer</span><span class="sxs-lookup"><span data-stu-id="c5f6b-115">New-AzureRmSqlServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="c5f6b-116">Adatbázis vagy a rugalmas készlet tároló logikai kiszolgáló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="c5f6b-116">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="c5f6b-117">Új-AzureRmSqlElasticPool</span><span class="sxs-lookup"><span data-stu-id="c5f6b-117">New-AzureRmSqlElasticPool</span></span>](/powershell/module/azurerm.sql/new-azurermsqlelasticpool) | <span data-ttu-id="c5f6b-118">Létrehoz egy logikai kiszolgáló belül rugalmas készlethez.</span><span class="sxs-lookup"><span data-stu-id="c5f6b-118">Creates an elastic pool within a logical server.</span></span> |
| [<span data-ttu-id="c5f6b-119">Set-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="c5f6b-119">Set-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/set-azurermsqldatabase) | <span data-ttu-id="c5f6b-120">Frissíti az adatbázis-tulajdonságai, vagy azokat, kívüli vagy rugalmas készletek között adatbázis helyezi.</span><span class="sxs-lookup"><span data-stu-id="c5f6b-120">Updates database properties or moves a database into, out of, or between elastic pools.</span></span> |
| [<span data-ttu-id="c5f6b-121">Új AzureRmSqlDatabaseSecondary</span><span class="sxs-lookup"><span data-stu-id="c5f6b-121">New-AzureRmSqlDatabaseSecondary</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabasesecondary)| <span data-ttu-id="c5f6b-122">Létrehoz egy meglévő adatbázis másodlagos adatbázist, és elindítja a adatreplikáció.</span><span class="sxs-lookup"><span data-stu-id="c5f6b-122">Creates a secondary database for an existing database and starts data replication.</span></span> |
| [<span data-ttu-id="c5f6b-123">Get-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="c5f6b-123">Get-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/get-azurermsqldatabase)| <span data-ttu-id="c5f6b-124">Egy vagy több adatbázis lekérdezi.</span><span class="sxs-lookup"><span data-stu-id="c5f6b-124">Gets one or more databases.</span></span> |
| [<span data-ttu-id="c5f6b-125">Set-AzureRmSqlDatabaseSecondary</span><span class="sxs-lookup"><span data-stu-id="c5f6b-125">Set-AzureRmSqlDatabaseSecondary</span></span>](/powershell/module/azurerm.sql/set-azurermsqldatabasesecondary)| <span data-ttu-id="c5f6b-126">A másodlagos adatbázis toobe elsődleges tooinitiate feladatátvételt vált.</span><span class="sxs-lookup"><span data-stu-id="c5f6b-126">Switches a secondary database toobe primary tooinitiate failover.</span></span>|
| [<span data-ttu-id="c5f6b-127">Get-AzureRmSqlDatabaseReplicationLink</span><span class="sxs-lookup"><span data-stu-id="c5f6b-127">Get-AzureRmSqlDatabaseReplicationLink</span></span>](/powershell/module/azurerm.sql/get-azurermsqldatabasereplicationlink) | <span data-ttu-id="c5f6b-128">Lekérdezi a hello földrajzi-replikációs hivatkozások az Azure SQL Database és egy erőforráscsoport vagy SQL Server között.</span><span class="sxs-lookup"><span data-stu-id="c5f6b-128">Gets hello geo-replication links between an Azure SQL Database and a resource group or SQL Server.</span></span> |
| [<span data-ttu-id="c5f6b-129">Remove-AzureRmSqlDatabaseSecondary</span><span class="sxs-lookup"><span data-stu-id="c5f6b-129">Remove-AzureRmSqlDatabaseSecondary</span></span>](/powershell/module/azurerm.sql/remove-azurermsqldatabasesecondary) | <span data-ttu-id="c5f6b-130">Egy SQL-adatbázis és a megadott másodlagos adatbázis hello közötti adatreplikáció leáll.</span><span class="sxs-lookup"><span data-stu-id="c5f6b-130">Terminates data replication between a SQL Database and hello specified secondary database.</span></span> |
| [<span data-ttu-id="c5f6b-131">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="c5f6b-131">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="c5f6b-132">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="c5f6b-132">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="c5f6b-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c5f6b-133">Next steps</span></span>

<span data-ttu-id="c5f6b-134">Az Azure PowerShell hello további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="c5f6b-134">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="c5f6b-135">További SQL Database PowerShell parancsfájl minták hello található [Azure SQL Database PowerShell-parancsfájlok](../sql-database-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="c5f6b-135">Additional SQL Database PowerShell script samples can be found in hello [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).</span></span>
