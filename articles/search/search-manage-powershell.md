---
title: "Azure Search kezelése a Powershell-parancsfájlok |} Microsoft Docs"
description: "Az Azure Search szolgáltatás PowerShell-parancsfájlokkal kezelheti. Hozzon létre vagy frissíteni az Azure Search szolgáltatást, és Azure Search adminisztrációs kulcsok kezelése"
services: search
documentationcenter: 
author: seansaleh
manager: mblythe
editor: 
tags: azure-resource-manager
ms.assetid: 9b3dc1f2-3619-4235-ba1f-d2d6f5c45dd5
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: powershell
ms.date: 08/15/2016
ms.author: seasa
ms.openlocfilehash: aa51c846efef12461ec382274199bc049c42aaa3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="manage-your-azure-search-service-with-powershell"></a><span data-ttu-id="1cb31-104">A PowerShell segítségével az Azure Search szolgáltatás kezelése</span><span class="sxs-lookup"><span data-stu-id="1cb31-104">Manage your Azure Search service with PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="1cb31-105">Portal</span><span class="sxs-lookup"><span data-stu-id="1cb31-105">Portal</span></span>](search-manage.md)
> * [<span data-ttu-id="1cb31-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="1cb31-106">PowerShell</span></span>](search-manage-powershell.md)
> 
> 

<span data-ttu-id="1cb31-107">Ez a témakör ismerteti a PowerShell-parancsokat az Azure Search szolgáltatások felügyeleti feladatok elvégzéséhez.</span><span class="sxs-lookup"><span data-stu-id="1cb31-107">This topic describes the PowerShell commands to perform many of the management tasks for Azure Search services.</span></span> <span data-ttu-id="1cb31-108">Végigvezetjük a search szolgáltatás létrehozása, skálázás, és annak API-kulcsok kezelése.</span><span class="sxs-lookup"><span data-stu-id="1cb31-108">We will walk through creating a search service, scaling it, and managing its API keys.</span></span>
<span data-ttu-id="1cb31-109">Ezek a parancsok párhuzamosan használható felügyeleti lehetőségekről a [Azure Search felügyeleti REST API](http://msdn.microsoft.com/library/dn832684.aspx).</span><span class="sxs-lookup"><span data-stu-id="1cb31-109">These commands parallel the management options available in the [Azure Search Management REST API](http://msdn.microsoft.com/library/dn832684.aspx).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1cb31-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1cb31-110">Prerequisites</span></span>
* <span data-ttu-id="1cb31-111">Az Azure PowerShell 1.0-ás vagy újabb verzióját kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="1cb31-111">You must have Azure PowerShell 1.0 or greater.</span></span> <span data-ttu-id="1cb31-112">Útmutatásért lásd: [telepítse és konfigurálja az Azure Powershellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="1cb31-112">For instructions, see [Install and configure Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="1cb31-113">Az Azure-előfizetéshez az alább ismertetett PowerShell kell bejelentkeznie.</span><span class="sxs-lookup"><span data-stu-id="1cb31-113">You must be logged in to your Azure subscription in PowerShell as described below.</span></span>

<span data-ttu-id="1cb31-114">Először meg kell ezzel a paranccsal az Azure bejelentkezési azonosító:</span><span class="sxs-lookup"><span data-stu-id="1cb31-114">First, you must login to Azure with this command:</span></span>

    Login-AzureRmAccount

<span data-ttu-id="1cb31-115">A Microsoft Azure bejelentkezési párbeszédpanelen adja meg az e-mail cím, az Azure-fiókjával, és a hozzá tartozó jelszó.</span><span class="sxs-lookup"><span data-stu-id="1cb31-115">Specify the email address of your Azure account and its password in the Microsoft Azure login dialog.</span></span>

<span data-ttu-id="1cb31-116">Alternatív megoldásként [egy egyszerű szolgáltatást a nem interaktív bejelentkezési](../azure-resource-manager/resource-group-authenticate-service-principal.md).</span><span class="sxs-lookup"><span data-stu-id="1cb31-116">Alternatively you can [login non-interactively with a service principal](../azure-resource-manager/resource-group-authenticate-service-principal.md).</span></span>

<span data-ttu-id="1cb31-117">Ha több Azure-előfizetéssel rendelkezik, az Azure-előfizetéshez be kell.</span><span class="sxs-lookup"><span data-stu-id="1cb31-117">If you have multiple Azure subscriptions, you need to set your Azure subscription.</span></span> <span data-ttu-id="1cb31-118">Az aktuális előfizetések listájának megtekintéséhez futtassa ezt a parancsot.</span><span class="sxs-lookup"><span data-stu-id="1cb31-118">To see a list of your current subscriptions, run this command.</span></span>

    Get-AzureRmSubscription | sort SubscriptionName | Select SubscriptionName

<span data-ttu-id="1cb31-119">Adja meg az előfizetést, futtassa a következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="1cb31-119">To specify the subscription, run the following command.</span></span> <span data-ttu-id="1cb31-120">A következő példában az előfizetés neve: `ContosoSubscription`.</span><span class="sxs-lookup"><span data-stu-id="1cb31-120">In the following example, the subscription name is `ContosoSubscription`.</span></span>

    Select-AzureRmSubscription -SubscriptionName ContosoSubscription

## <a name="commands-to-help-you-get-started"></a><span data-ttu-id="1cb31-121">Parancsok segítségével első lépései</span><span class="sxs-lookup"><span data-stu-id="1cb31-121">Commands to help you get started</span></span>
    $serviceName = "your-service-name-lowercase-with-dashes"
    $sku = "free" # or "basic" or "standard" for paid services
    $location = "West US"
    # You can get a list of potential locations with
    # (Get-AzureRmResourceProvider -ListAvailable | Where-Object {$_.ProviderNamespace -eq 'Microsoft.Search'}).Locations
    $resourceGroupName = "YourResourceGroup" 
    # If you don't already have this resource group, you can create it with 
    # New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Register the ARM provider idempotently. This must be done once per subscription
    Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Search"

    # Create a new search service
    # This command will return once the service is fully created
    New-AzureRmResourceGroupDeployment `
        -ResourceGroupName $resourceGroupName `
        -TemplateUri "https://gallery.azure.com/artifact/20151001/Microsoft.Search.1.0.9/DeploymentTemplates/searchServiceDefaultTemplate.json" `
        -NameFromTemplate $serviceName `
        -Sku $sku `
        -Location $location `
        -PartitionCount 1 `
        -ReplicaCount 1

    # Get information about your new service and store it in $resource
    $resource = Get-AzureRmResource `
        -ResourceType "Microsoft.Search/searchServices" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19

    # View your resource
    $resource

    # Get the primary admin API key
    $primaryKey = (Invoke-AzureRmResourceAction `
        -Action listAdminKeys `
        -ResourceId $resource.ResourceId `
        -ApiVersion 2015-08-19).PrimaryKey

    # Regenerate the secondary admin API Key
    $secondaryKey = (Invoke-AzureRmResourceAction `
        -ResourceType "Microsoft.Search/searchServices/regenerateAdminKey" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19 `
        -Action secondary).SecondaryKey

    # Create a query key for read only access to your indexes
    $queryKeyDescription = "query-key-created-from-powershell"
    $queryKey = (Invoke-AzureRmResourceAction `
        -ResourceType "Microsoft.Search/searchServices/createQueryKey" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19 `
        -Action $queryKeyDescription).Key

    # View your query key
    $queryKey

    # Delete query key
    Remove-AzureRmResource `
        -ResourceType "Microsoft.Search/searchServices/deleteQueryKey/$($queryKey)" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19

    # Scale your service up
    # Note that this will only work if you made a non "free" service
    # This command will not return until the operation is finished
    # It can take 15 minutes or more to provision the additional resources
    $resource.Properties.ReplicaCount = 2
    $resource | Set-AzureRmResource

    # Delete your service
    # Deleting your service will delete all indexes and data in the service
    $resource | Remove-AzureRmResource

## <a name="next-steps"></a><span data-ttu-id="1cb31-122">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1cb31-122">Next Steps</span></span>
<span data-ttu-id="1cb31-123">Most, hogy a szolgáltatás létrehozása a következő lépésekre is: build egy [index](search-what-is-an-index.md), [lekérdezheti az indexét](search-query-overview.md), és végül létrehozása és kezelése a saját keresési alkalmazás az Azure Search használ.</span><span class="sxs-lookup"><span data-stu-id="1cb31-123">Now that your service is created, you can take the next steps: build an [index](search-what-is-an-index.md), [query an index](search-query-overview.md), and finally create and manage your own search application that uses Azure Search.</span></span>

* [<span data-ttu-id="1cb31-124">Az Azure Search-index létrehozása az Azure portálon</span><span class="sxs-lookup"><span data-stu-id="1cb31-124">Create an Azure Search index in the Azure portal</span></span>](search-create-index-portal.md)
* [<span data-ttu-id="1cb31-125">Keresési ablak használata az Azure-portálon az Azure Search-index lekérdezése</span><span class="sxs-lookup"><span data-stu-id="1cb31-125">Query an Azure Search index using Search Explorer in the Azure portal</span></span>](search-explorer.md)
* [<span data-ttu-id="1cb31-126">Adatok betöltése a többitől indexelő beállítása</span><span class="sxs-lookup"><span data-stu-id="1cb31-126">Setup an indexer to load data from other services</span></span>](search-indexer-overview.md)
* [<span data-ttu-id="1cb31-127">Azure Search .NET használata</span><span class="sxs-lookup"><span data-stu-id="1cb31-127">How to use Azure Search in .NET</span></span>](search-howto-dotnet-sdk.md)
* [<span data-ttu-id="1cb31-128">Az Azure Search forgalom elemzése</span><span class="sxs-lookup"><span data-stu-id="1cb31-128">Analyze your Azure Search traffic</span></span>](search-traffic-analytics.md)

