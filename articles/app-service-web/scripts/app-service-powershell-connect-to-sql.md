---
title: "Az Azure PowerShell-parancsfájl minta - webes alkalmazás csatlakoztatása SQL-adatbázishoz |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - webes alkalmazás csatlakoztatása SQL-adatbázishoz"
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
ms.openlocfilehash: 5312bf6b81d1cc48490b71c3f77323cca23e1559
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="connect-a-web-app-to-a-sql-database"></a><span data-ttu-id="c2fb6-103">Webes alkalmazás csatlakoztatása SQL-adatbázishoz</span><span class="sxs-lookup"><span data-stu-id="c2fb6-103">Connect a web app to a SQL database</span></span>

<span data-ttu-id="c2fb6-104">Ebben a forgatókönyvben, megtudhatja, hogyan Azure SQL-adatbázis és az Azure-webalkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="c2fb6-104">In this scenario you will learn how to create an Azure SQL database and an Azure web app.</span></span> <span data-ttu-id="c2fb6-105">Az SQL-adatbázis majd összekapcsolja a webes alkalmazás alkalmazás-beállításokkal.</span><span class="sxs-lookup"><span data-stu-id="c2fb6-105">Then you will link the SQL database to the web app using app settings.</span></span>

<span data-ttu-id="c2fb6-106">Szükség esetén telepítse az Azure PowerShell található utasítás használatával a [Azure PowerShell útmutató](/powershell/azure/overview), majd futtassa a `Login-AzureRmAccount` kapcsolat létrehozása az Azure-ral.</span><span class="sxs-lookup"><span data-stu-id="c2fb6-106">If needed, install the Azure PowerShell using the instruction found in the [Azure PowerShell guide](/powershell/azure/overview), and then run `Login-AzureRmAccount` to create a connection with Azure.</span></span>

## <a name="sample-script"></a><span data-ttu-id="c2fb6-107">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="c2fb6-107">Sample script</span></span>

<span data-ttu-id="c2fb6-108">[!code-powershell[fő](../../../powershell_scripts/app-service/connect-to-sql/connect-to-sql.ps1?highlight=13 "webes alkalmazás csatlakoztatása SQL-adatbázishoz")]</span><span class="sxs-lookup"><span data-stu-id="c2fb6-108">[!code-powershell[main](../../../powershell_scripts/app-service/connect-to-sql/connect-to-sql.ps1?highlight=13 "Connect a web app to a SQL database")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="c2fb6-109">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="c2fb6-109">Clean up deployment</span></span> 

<span data-ttu-id="c2fb6-110">A parancsfájl-minta futtatása után a következő parancs segítségével távolítsa el az erőforráscsoportot, a web app és az összes kapcsolódó erőforrások.</span><span class="sxs-lookup"><span data-stu-id="c2fb6-110">After the script sample has been run, the following command can be used to remove the resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="c2fb6-111">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="c2fb6-111">Script explanation</span></span>

<span data-ttu-id="c2fb6-112">A parancsfájl a következő parancsokat.</span><span class="sxs-lookup"><span data-stu-id="c2fb6-112">This script uses the following commands.</span></span> <span data-ttu-id="c2fb6-113">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="c2fb6-113">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="c2fb6-114">Parancs</span><span class="sxs-lookup"><span data-stu-id="c2fb6-114">Command</span></span> | <span data-ttu-id="c2fb6-115">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="c2fb6-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="c2fb6-116">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="c2fb6-116">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="c2fb6-117">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="c2fb6-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="c2fb6-118">Új AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="c2fb6-118">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="c2fb6-119">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="c2fb6-119">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="c2fb6-120">Új AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="c2fb6-120">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="c2fb6-121">Létrehoz egy webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c2fb6-121">Creates a web app.</span></span> |
| [<span data-ttu-id="c2fb6-122">Új AzureRMSQLServer</span><span class="sxs-lookup"><span data-stu-id="c2fb6-122">New-AzureRMSQLServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="c2fb6-123">Létrehoz egy SQL Database-kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="c2fb6-123">Creates a SQL Database server.</span></span> |
| [<span data-ttu-id="c2fb6-124">New-AzureRmSqlServerFirewallRule</span><span class="sxs-lookup"><span data-stu-id="c2fb6-124">New-AzureRmSqlServerFirewallRule</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule) | <span data-ttu-id="c2fb6-125">SQL adatbázis-kiszolgálót egy tűzfalszabályt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="c2fb6-125">Creates a firewall rule for a SQL Database server.</span></span> |
| [<span data-ttu-id="c2fb6-126">Új-AzureRMSQLDatabase</span><span class="sxs-lookup"><span data-stu-id="c2fb6-126">New-AzureRMSQLDatabase</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabase) | <span data-ttu-id="c2fb6-127">Létrehoz egy adatbázist vagy egy olyan rugalmas adatbázis.</span><span class="sxs-lookup"><span data-stu-id="c2fb6-127">Creates a database or an elastic database.</span></span> |
| [<span data-ttu-id="c2fb6-128">Set-AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="c2fb6-128">Set-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/set-azurermwebapp) | <span data-ttu-id="c2fb6-129">A webalkalmazás konfigurációs módosítja.</span><span class="sxs-lookup"><span data-stu-id="c2fb6-129">Modifies a web app's configuration.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="c2fb6-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c2fb6-130">Next steps</span></span>

<span data-ttu-id="c2fb6-131">Az Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="c2fb6-131">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="c2fb6-132">Azure App Service Web Apps további Azure Powershell-példák találhatók a [Azure PowerShell-példák](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="c2fb6-132">Additional Azure Powershell samples for Azure App Service Web Apps can be found in the [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>
