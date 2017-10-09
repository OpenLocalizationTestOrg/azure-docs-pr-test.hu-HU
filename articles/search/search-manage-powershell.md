---
title: "a Powershell-parancsfájlok Azure Search aaaManage |} Microsoft Docs"
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
ms.openlocfilehash: fc7fb4b025340c77717601e0aaee938be3e9230f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-your-azure-search-service-with-powershell"></a>A PowerShell segítségével az Azure Search szolgáltatás kezelése
> [!div class="op_single_selector"]
> * [Portál](search-manage.md)
> * [PowerShell](search-manage-powershell.md)
> 
> 

Ez a témakör ismerteti a PowerShell-parancsok tooperform hello hello felügyeleti feladatok az Azure Search szolgáltatások. Végigvezetjük a search szolgáltatás létrehozása, skálázás, és annak API-kulcsok kezelése.
Ezek a parancsok párhuzamos hello hello használható felügyeleti lehetőségekről [Azure Search felügyeleti REST API](http://msdn.microsoft.com/library/dn832684.aspx).

## <a name="prerequisites"></a>Előfeltételek
* Az Azure PowerShell 1.0-ás vagy újabb verzióját kell rendelkeznie. Útmutatásért lásd: [telepítse és konfigurálja az Azure Powershellt](/powershell/azure/overview).
* Az Azure-előfizetés az alább ismertetett PowerShell tooyour kell bejelentkeznie.

Először meg kell bejelentkezési tooAzure ezzel a paranccsal:

    Login-AzureRmAccount

Hello Microsoft Azure bejelentkezési párbeszédpanel hello e-mail címet az Azure-fiókjával, és a hozzá tartozó jelszó megadása

Alternatív megoldásként [egy egyszerű szolgáltatást a nem interaktív bejelentkezési](../azure-resource-manager/resource-group-authenticate-service-principal.md).

Ha több Azure-előfizetéssel rendelkezik, akkor tooset az Azure-előfizetéshez. az aktuális előfizetések listája toosee futtassa ezt a parancsot.

    Get-AzureRmSubscription | sort SubscriptionName | Select SubscriptionName

toospecify hello előfizetés, futtassa a következő parancs hello. A következő példa hello, hello előfizetés neve: `ContosoSubscription`.

    Select-AzureRmSubscription -SubscriptionName ContosoSubscription

## <a name="commands-toohelp-you-get-started"></a>Parancsok toohelp megkezdése
    $serviceName = "your-service-name-lowercase-with-dashes"
    $sku = "free" # or "basic" or "standard" for paid services
    $location = "West US"
    # You can get a list of potential locations with
    # (Get-AzureRmResourceProvider -ListAvailable | Where-Object {$_.ProviderNamespace -eq 'Microsoft.Search'}).Locations
    $resourceGroupName = "YourResourceGroup" 
    # If you don't already have this resource group, you can create it with 
    # New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Register hello ARM provider idempotently. This must be done once per subscription
    Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Search"

    # Create a new search service
    # This command will return once hello service is fully created
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

    # Get hello primary admin API key
    $primaryKey = (Invoke-AzureRmResourceAction `
        -Action listAdminKeys `
        -ResourceId $resource.ResourceId `
        -ApiVersion 2015-08-19).PrimaryKey

    # Regenerate hello secondary admin API Key
    $secondaryKey = (Invoke-AzureRmResourceAction `
        -ResourceType "Microsoft.Search/searchServices/regenerateAdminKey" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19 `
        -Action secondary).SecondaryKey

    # Create a query key for read only access tooyour indexes
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
    # This command will not return until hello operation is finished
    # It can take 15 minutes or more tooprovision hello additional resources
    $resource.Properties.ReplicaCount = 2
    $resource | Set-AzureRmResource

    # Delete your service
    # Deleting your service will delete all indexes and data in hello service
    $resource | Remove-AzureRmResource

## <a name="next-steps"></a>Következő lépések
Most, hogy a szolgáltatás létrehozása eltarthat hello lépések: build egy [index](search-what-is-an-index.md), [lekérdezheti az indexét](search-query-overview.md), és végül létrehozása és kezelése a saját keresési alkalmazás az Azure Search használ.

* [Hozzon létre egy Azure Search-index hello Azure-portálon](search-create-index-portal.md)
* [Keresési ablak használatát hello Azure portál Azure Search-index lekérdezése](search-explorer.md)
* [A telepítő egy indexelő tooload adatok más szolgáltatások](search-indexer-overview.md)
* [Hogyan toouse .NET keresni Azure](search-howto-dotnet-sdk.md)
* [Az Azure Search forgalom elemzése](search-traffic-analytics.md)

