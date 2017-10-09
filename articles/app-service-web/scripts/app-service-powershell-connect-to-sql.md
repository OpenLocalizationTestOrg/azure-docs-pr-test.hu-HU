---
title: "aaaAzure PowerShell parancsfájl minta - web app tooa SQL adatbázis csatlakoztatása |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - web app tooa SQL adatbázis csatlakoztatása"
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 055440a9-fff1-49b2-b964-9c95b364e533
ms.service: app-service
ms.devlang: multiple
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 03/20/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 8f80b940378d020cbcaec2c1bbc28bae1a3ef35a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-a-web-app-tooa-sql-database"></a><span data-ttu-id="33724-103">Webes alkalmazás tooa SQL adatbázis csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="33724-103">Connect a web app tooa SQL database</span></span>

<span data-ttu-id="33724-104">Ebben a forgatókönyvben megtudhatja, hogyan toocreate Azure SQL-adatbázis és egy Azure webes alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="33724-104">In this scenario you will learn how toocreate an Azure SQL database and an Azure web app.</span></span> <span data-ttu-id="33724-105">Majd hello SQL adatbázis toohello web app használatával Alkalmazásbeállítások csatolást.</span><span class="sxs-lookup"><span data-stu-id="33724-105">Then you will link hello SQL database toohello web app using app settings.</span></span>

<span data-ttu-id="33724-106">Szükség esetén telepítse az Azure PowerShell használatával hello utasítás található hello hello [Azure PowerShell útmutató](/powershell/azure/overview), majd futtassa a `Login-AzureRmAccount` toocreate Azure kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="33724-106">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](/powershell/azure/overview), and then run `Login-AzureRmAccount` toocreate a connection with Azure.</span></span>

## <a name="sample-script"></a><span data-ttu-id="33724-107">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="33724-107">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/app-service/connect-to-sql/connect-to-sql.ps1?highlight=13 "Connect a web app tooa SQL database")]

## <a name="clean-up-deployment"></a><span data-ttu-id="33724-108">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="33724-108">Clean up deployment</span></span> 

<span data-ttu-id="33724-109">Hello parancsfájl minta futtatása után a következő parancs hello lehet használt tooremove hello erőforráscsoport, a web app és az összes kapcsolódó erőforrások.</span><span class="sxs-lookup"><span data-stu-id="33724-109">After hello script sample has been run, hello following command can be used tooremove hello resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="33724-110">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="33724-110">Script explanation</span></span>

<span data-ttu-id="33724-111">A parancsfájl a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="33724-111">This script uses hello following commands.</span></span> <span data-ttu-id="33724-112">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="33724-112">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="33724-113">Parancs</span><span class="sxs-lookup"><span data-stu-id="33724-113">Command</span></span> | <span data-ttu-id="33724-114">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="33724-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="33724-115">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="33724-115">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="33724-116">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="33724-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="33724-117">Új AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="33724-117">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="33724-118">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="33724-118">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="33724-119">Új AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="33724-119">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="33724-120">Létrehoz egy webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="33724-120">Creates a web app.</span></span> |
| [<span data-ttu-id="33724-121">Új AzureRMSQLServer</span><span class="sxs-lookup"><span data-stu-id="33724-121">New-AzureRMSQLServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="33724-122">Létrehoz egy SQL Database-kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="33724-122">Creates a SQL Database server.</span></span> |
| [<span data-ttu-id="33724-123">New-AzureRmSqlServerFirewallRule</span><span class="sxs-lookup"><span data-stu-id="33724-123">New-AzureRmSqlServerFirewallRule</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule) | <span data-ttu-id="33724-124">SQL adatbázis-kiszolgálót egy tűzfalszabályt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="33724-124">Creates a firewall rule for a SQL Database server.</span></span> |
| [<span data-ttu-id="33724-125">Új-AzureRMSQLDatabase</span><span class="sxs-lookup"><span data-stu-id="33724-125">New-AzureRMSQLDatabase</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabase) | <span data-ttu-id="33724-126">Létrehoz egy adatbázist vagy egy olyan rugalmas adatbázis.</span><span class="sxs-lookup"><span data-stu-id="33724-126">Creates a database or an elastic database.</span></span> |
| [<span data-ttu-id="33724-127">Set-AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="33724-127">Set-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/set-azurermwebapp) | <span data-ttu-id="33724-128">A webalkalmazás konfigurációs módosítja.</span><span class="sxs-lookup"><span data-stu-id="33724-128">Modifies a web app's configuration.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="33724-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="33724-129">Next steps</span></span>

<span data-ttu-id="33724-130">Hello Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="33724-130">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="33724-131">Azure App Service Web Apps további Azure Powershell-példák találhatók hello [Azure PowerShell-példák](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="33724-131">Additional Azure Powershell samples for Azure App Service Web Apps can be found in hello [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>
