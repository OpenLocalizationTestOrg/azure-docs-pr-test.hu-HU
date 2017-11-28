---
title: "aaaPowerShell példa-Azure SQL-adatbázis létrehozása |} Microsoft Docs"
description: "Az Azure PowerShell például parancsfájl-toocreate Azure SQL-adatbázis"
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 06/23/2017
ms.author: janeng
ms.openlocfilehash: ae57b2018f4a550bf2c6da688d6e49bdadf3d3ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-toocreate-a-single-azure-sql-database-and-configure-a-firewall-rule"></a><span data-ttu-id="8668c-103">Használjon PowerShell toocreate egy Azure SQL-adatbázis és a tűzfalszabályok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8668c-103">Use PowerShell toocreate a single Azure SQL database and configure a firewall rule</span></span>

<span data-ttu-id="8668c-104">A PowerShell-mintaparancsfájl egy Azure SQL Database adatbázist hoz létre, és konfigurálja egy kiszolgálószintű tűzfalszabályt.</span><span class="sxs-lookup"><span data-stu-id="8668c-104">This PowerShell script example creates an Azure SQL database and configures a server-level firewall rule.</span></span> <span data-ttu-id="8668c-105">Hello parancsfájl sikeres futtatása után hello elérhető SQL-adatbázis az összes Azure-szolgáltatások és hello konfigurált IP-címét.</span><span class="sxs-lookup"><span data-stu-id="8668c-105">Once hello script has been successfully run, hello SQL Database can be accessed from all Azure services and hello configured IP address.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="8668c-106">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="8668c-106">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/sql-database/create-and-configure-database/create-and-configure-database.ps1?highlight=13-14 "Create SQL Database")]

## <a name="clean-up-deployment"></a><span data-ttu-id="8668c-107">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="8668c-107">Clean up deployment</span></span>

<span data-ttu-id="8668c-108">Hello parancsfájl minta futtatása után a következő parancs hello lehet használt tooremove hello erőforráscsoport és a vele társított összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="8668c-108">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="8668c-109">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="8668c-109">Script explanation</span></span>

<span data-ttu-id="8668c-110">A parancsfájl a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="8668c-110">This script uses hello following commands.</span></span> <span data-ttu-id="8668c-111">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="8668c-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="8668c-112">Parancs</span><span class="sxs-lookup"><span data-stu-id="8668c-112">Command</span></span> | <span data-ttu-id="8668c-113">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="8668c-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="8668c-114">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="8668c-114">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="8668c-115">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="8668c-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="8668c-116">Új AzureRmSqlServer</span><span class="sxs-lookup"><span data-stu-id="8668c-116">New-AzureRmSqlServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="8668c-117">Adatbázis vagy a rugalmas készlet tároló logikai kiszolgáló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="8668c-117">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="8668c-118">New-AzureRmSqlServerFirewallRule</span><span class="sxs-lookup"><span data-stu-id="8668c-118">New-AzureRmSqlServerFirewallRule</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule) | <span data-ttu-id="8668c-119">Létrehoz egy tűzfal szabály tooallow hozzáférés tooall SQL-adatbázisok hello megadott IP-címtartomány a hello kiszolgálóján.</span><span class="sxs-lookup"><span data-stu-id="8668c-119">Creates a firewall rule tooallow access tooall SQL Databases on hello server from hello entered IP address range.</span></span> |
| [<span data-ttu-id="8668c-120">New-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="8668c-120">New-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabase) | <span data-ttu-id="8668c-121">Adatbázist hoz létre egy logikai egyetlen vagy egy készletezett adatbázis-kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="8668c-121">Creates a database in a logical server as a single or a pooled database.</span></span> |
| [<span data-ttu-id="8668c-122">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="8668c-122">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="8668c-123">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="8668c-123">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="8668c-124">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8668c-124">Next steps</span></span>

<span data-ttu-id="8668c-125">Az Azure PowerShell hello további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="8668c-125">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="8668c-126">További SQL Database PowerShell parancsfájl minták hello található [Azure SQL Database PowerShell-parancsfájlok](../sql-database-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="8668c-126">Additional SQL Database PowerShell script samples can be found in hello [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).</span></span>



