---
title: "aaaAzure Cosmos DB Automation - felügyelet a PowerShell-lel |} Microsoft Docs"
description: "Azure Powershell használata az Azure Cosmos DB fiókok kezelése."
services: cosmos-db
author: dmakwana
manager: jhubbard
editor: 
tags: azure-resource-manager
documentationcenter: 
ms.assetid: 
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/21/2017
ms.author: dimakwan
ms.openlocfilehash: 3239fb815918a0e47bff69fcd1ab6562519e429b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-cosmos-db-account-using-powershell"></a><span data-ttu-id="f6a28-103">PowerShell-lel Azure Cosmos DB-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="f6a28-103">Create an Azure Cosmos DB account using PowerShell</span></span>

<span data-ttu-id="f6a28-104">hello Ez az útmutató ismerteti az Azure Cosmos DB adatbázis fiókok az Azure Powershell parancsok tooautomate kezelését.</span><span class="sxs-lookup"><span data-stu-id="f6a28-104">hello following guide describes commands tooautomate management of your Azure Cosmos DB database accounts using Azure Powershell.</span></span> <span data-ttu-id="f6a28-105">Emellett parancsok toomanage kulcsait és a feladatátvételi prioritások [több területi adatbázis fiókok][scaling-globally].</span><span class="sxs-lookup"><span data-stu-id="f6a28-105">It also includes commands toomanage account keys and failover priorities in [multi-region database accounts][scaling-globally].</span></span> <span data-ttu-id="f6a28-106">Az adatbázisfiók frissítése lehetővé teszi az toomodify konzisztencia házirendek és hozzáadása régiók.</span><span class="sxs-lookup"><span data-stu-id="f6a28-106">Updating your database account allows you toomodify consistency policies and add/remove regions.</span></span> <span data-ttu-id="f6a28-107">A platformok közötti felügyeleti Azure Cosmos DB-fiókja, választhat [Azure CLI](cli-samples.md), hello [erőforrás-szolgáltató REST API][rp-rest-api], vagy hello [Azure portál](create-documentdb-dotnet.md#create-account).</span><span class="sxs-lookup"><span data-stu-id="f6a28-107">For cross-platform management of your Azure Cosmos DB account, you can use either [Azure CLI](cli-samples.md), hello [Resource Provider REST API][rp-rest-api], or hello [Azure portal](create-documentdb-dotnet.md#create-account).</span></span>

## <a name="getting-started"></a><span data-ttu-id="f6a28-108">Első lépések</span><span class="sxs-lookup"><span data-stu-id="f6a28-108">Getting Started</span></span>

<span data-ttu-id="f6a28-109">Hello utasításait követve [hogyan tooinstall és konfigurálja az Azure Powershellt] [ powershell-install-configure] tooinstall és az Azure Resource Manager tooyour bejelentkezési fiók a PowerShellben.</span><span class="sxs-lookup"><span data-stu-id="f6a28-109">Follow hello instructions in [How tooinstall and configure Azure PowerShell][powershell-install-configure] tooinstall and log in tooyour Azure Resource Manager account in Powershell.</span></span>

### <a name="notes"></a><span data-ttu-id="f6a28-110">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="f6a28-110">Notes</span></span>

* <span data-ttu-id="f6a28-111">Ha szeretné, hogy a következő parancsok a felhasználó jóváhagyásának kérése nélkül tooexecute hello, hozzáfűző hello `-Force` toohello parancs jelzőt.</span><span class="sxs-lookup"><span data-stu-id="f6a28-111">If you would like tooexecute hello following commands without requiring user confirmation, append hello `-Force` flag toohello command.</span></span>
* <span data-ttu-id="f6a28-112">A következő parancsokat minden hello szinkronizáltak.</span><span class="sxs-lookup"><span data-stu-id="f6a28-112">All hello following commands are synchronous.</span></span>

## <span data-ttu-id="f6a28-113"><a id="create-documentdb-account-powershell"></a>Az Azure Cosmos DB-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="f6a28-113"><a id="create-documentdb-account-powershell"></a> Create an Azure Cosmos DB Account</span></span>

<span data-ttu-id="f6a28-114">Ez a parancs lehetővé teszi egy Azure Cosmos DB adatbázisfiók toocreate.</span><span class="sxs-lookup"><span data-stu-id="f6a28-114">This command allows you toocreate an Azure Cosmos DB database account.</span></span> <span data-ttu-id="f6a28-115">Az új adatbázis-fiók beállítása vagy egyetlen területi vagy [több területi] [ scaling-globally] az egy bizonyos [konzisztencia házirend](consistency-levels.md).</span><span class="sxs-lookup"><span data-stu-id="f6a28-115">Configure your new database account as either single-region or [multi-region][scaling-globally] with a certain [consistency policy](consistency-levels.md).</span></span>

    $locations = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0}, @{"locationName"="<read-region-location>"; "failoverPriority"=1})
    $iprangefilter = "<ip-range-filter>"
    $consistencyPolicy = @{"defaultConsistencyLevel"="<default-consistency-level>"; "maxIntervalInSeconds"="<max-interval>"; "maxStalenessPrefix"="<max-staleness-prefix>"}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    New-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName <resource-group-name>  -Location "<resource-group-location>" -Name <database-account-name> -Properties $CosmosDBProperties
    
* <span data-ttu-id="f6a28-116">`<write-region-location>`hello helyének neve hello írási hello adatbázisfiók területet.</span><span class="sxs-lookup"><span data-stu-id="f6a28-116">`<write-region-location>` hello location name of hello write region of hello database account.</span></span> <span data-ttu-id="f6a28-117">Ez a hely szükség toohave feladatátvételi prioritási érték 0.</span><span class="sxs-lookup"><span data-stu-id="f6a28-117">This location is required toohave a failover priority value of 0.</span></span> <span data-ttu-id="f6a28-118">Másodpercenkénti adatbázis-fiók pontosan egy írási régióban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="f6a28-118">There must be exactly one write region per database account.</span></span>
* <span data-ttu-id="f6a28-119">`<read-region-location>`hello helyének neve hello olvasási hello adatbázisfiók területet.</span><span class="sxs-lookup"><span data-stu-id="f6a28-119">`<read-region-location>` hello location name of hello read region of hello database account.</span></span> <span data-ttu-id="f6a28-120">Ez a hely szükséges toohave 0-nál nagyobb feladatátvételi prioritási értéke.</span><span class="sxs-lookup"><span data-stu-id="f6a28-120">This location is required toohave a failover priority value of greater than 0.</span></span> <span data-ttu-id="f6a28-121">Másodpercenkénti adatbázis-fiók több mint egy olvasási régiók lehet.</span><span class="sxs-lookup"><span data-stu-id="f6a28-121">There can be more than one read regions per database account.</span></span>
* <span data-ttu-id="f6a28-122">`<ip-range-filter>`Hello olyan IP-címek vagy IP-címtartományok engedélyezettek listájához, az ügyfél IP-címek egy adott adatbázis fiókjához tartozó hello néven CIDR formátumban toobe határozza meg.</span><span class="sxs-lookup"><span data-stu-id="f6a28-122">`<ip-range-filter>` Specifies hello set of IP addresses or IP address ranges in CIDR form toobe included as hello allowed list of client IPs for a given database account.</span></span> <span data-ttu-id="f6a28-123">IP-címeken/tartományokon vesszővel elválasztott, és nem tartalmazhat szóközt kell lennie.</span><span class="sxs-lookup"><span data-stu-id="f6a28-123">IP addresses/ranges must be comma separated and must not contain any spaces.</span></span> <span data-ttu-id="f6a28-124">További információkért lásd: [Azure Cosmos DB-Tűzfaltámogatás](firewall-support.md)</span><span class="sxs-lookup"><span data-stu-id="f6a28-124">For more information, see [Azure Cosmos DB Firewall Support](firewall-support.md)</span></span>
* <span data-ttu-id="f6a28-125">`<default-consistency-level>`hello alapértelmezett konzisztencia szintje hello Azure Cosmos DB fiók.</span><span class="sxs-lookup"><span data-stu-id="f6a28-125">`<default-consistency-level>` hello default consistency level of hello Azure Cosmos DB account.</span></span> <span data-ttu-id="f6a28-126">További információkért lásd: [Azure Cosmos DB-ben Konzisztenciaszintek](consistency-levels.md).</span><span class="sxs-lookup"><span data-stu-id="f6a28-126">For more information, see [Consistency Levels in Azure Cosmos DB](consistency-levels.md).</span></span>
* <span data-ttu-id="f6a28-127">`<max-interval>`Kötött elavulási konzisztencia használata esetén ezt az értéket adja hello idő (másodpercben) elavulási megengedett.</span><span class="sxs-lookup"><span data-stu-id="f6a28-127">`<max-interval>` When used with Bounded Staleness consistency, this value represents hello time amount of staleness (in seconds) tolerated.</span></span> <span data-ttu-id="f6a28-128">Ez az érték elfogadható tartománya 1-100.</span><span class="sxs-lookup"><span data-stu-id="f6a28-128">Accepted range for this value is 1 - 100.</span></span>
* <span data-ttu-id="f6a28-129">`<max-staleness-prefix>`Kötött elavulási konzisztencia használata esetén ezt az értéket jelöli hello megengedett elavult kérelmek száma.</span><span class="sxs-lookup"><span data-stu-id="f6a28-129">`<max-staleness-prefix>` When used with Bounded Staleness consistency, this value represents hello number of stale requests tolerated.</span></span> <span data-ttu-id="f6a28-130">Ez az érték elfogadható tartománya 1 – 2 147 483 647.</span><span class="sxs-lookup"><span data-stu-id="f6a28-130">Accepted range for this value is 1 – 2,147,483,647.</span></span>
* <span data-ttu-id="f6a28-131">`<resource-group-name>`hello hello neve [Azure erőforráscsoport] [ azure-resource-groups] toowhich hello új Azure Cosmos DB adatbázis fiók tartozik.</span><span class="sxs-lookup"><span data-stu-id="f6a28-131">`<resource-group-name>` hello name of hello [Azure Resource Group][azure-resource-groups] toowhich hello new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="f6a28-132">`<resource-group-location>`hello Azure erőforráscsoport toowhich hello új Azure Cosmos DB adatbázisfiók hello helyét tartozik.</span><span class="sxs-lookup"><span data-stu-id="f6a28-132">`<resource-group-location>` hello location of hello Azure Resource Group toowhich hello new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="f6a28-133">`<database-account-name>`hello Azure Cosmos DB adatbázis fiók toobe létrehozott hello neve.</span><span class="sxs-lookup"><span data-stu-id="f6a28-133">`<database-account-name>` hello name of hello Azure Cosmos DB database account toobe created.</span></span> <span data-ttu-id="f6a28-134">Csak használhat kisbetűket, számokat, hello "-" karakter, és 3 – 50 karakter közé kell esnie.</span><span class="sxs-lookup"><span data-stu-id="f6a28-134">It can only use lowercase letters, numbers, hello '-' character, and must be between 3 and 50 characters.</span></span>

<span data-ttu-id="f6a28-135">Példa:</span><span class="sxs-lookup"><span data-stu-id="f6a28-135">Example:</span></span> 

    $locations = @(@{"locationName"="West US"; "failoverPriority"=0}, @{"locationName"="East US"; "failoverPriority"=1})
    $iprangefilter = ""
    $consistencyPolicy = @{"defaultConsistencyLevel"="BoundedStaleness"; "maxIntervalInSeconds"=5; "maxStalenessPrefix"=100}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    New-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Location "West US" -Name "docdb-test" -Properties $CosmosDBProperties

### <a name="notes"></a><span data-ttu-id="f6a28-136">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="f6a28-136">Notes</span></span>
* <span data-ttu-id="f6a28-137">hello előző példa hoz létre egy adatbázis-fiók két régióban.</span><span class="sxs-lookup"><span data-stu-id="f6a28-137">hello preceding example creates a database account with two regions.</span></span> <span data-ttu-id="f6a28-138">Akkor is lehetséges toocreate egy adatbázis-fiók egy régió tartozik (amely hello írási régió van kijelölve, és a feladatátvételi prioritási értéke csak 0) vagy a több mint két régióban.</span><span class="sxs-lookup"><span data-stu-id="f6a28-138">It is also possible toocreate a database account with either one region (which is designated as hello write region and have a failover priority value of 0) or more than two regions.</span></span> <span data-ttu-id="f6a28-139">További információkért lásd: [több területi adatbázis fiókok][scaling-globally].</span><span class="sxs-lookup"><span data-stu-id="f6a28-139">For more information, see [multi-region database accounts][scaling-globally].</span></span>
* <span data-ttu-id="f6a28-140">hello helyét, amelyben Azure Cosmos DB általánosan elérhető régiók kell lennie.</span><span class="sxs-lookup"><span data-stu-id="f6a28-140">hello locations must be regions in which Azure Cosmos DB is generally available.</span></span> <span data-ttu-id="f6a28-141">hello aktuális területek listája a hello [Azure-régiókat lap](https://azure.microsoft.com/regions/#services).</span><span class="sxs-lookup"><span data-stu-id="f6a28-141">hello current list of regions is provided on hello [Azure Regions page](https://azure.microsoft.com/regions/#services).</span></span>

## <span data-ttu-id="f6a28-142"><a id="update-documentdb-account-powershell"></a>A DocumentDB-adatbázisfiók frissítése</span><span class="sxs-lookup"><span data-stu-id="f6a28-142"><a id="update-documentdb-account-powershell"></a> Update a DocumentDB Database Account</span></span>

<span data-ttu-id="f6a28-143">Ez a parancs tooupdate lehetővé teszi az Azure Cosmos DB adatbázis fiók tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="f6a28-143">This command allows you tooupdate your Azure Cosmos DB database account properties.</span></span> <span data-ttu-id="f6a28-144">Ez magában foglalja a hello konzisztencia házirend és hello helyek, mely hello adatbázis fiók létezik-e.</span><span class="sxs-lookup"><span data-stu-id="f6a28-144">This includes hello consistency policy and hello locations which hello database account exists in.</span></span>

> [!NOTE]
> <span data-ttu-id="f6a28-145">Ez a parancs lehetővé teszi a tooadd és eltávolítása, de nem teszi lehetővé a toomodify feladatátvételi prioritások.</span><span class="sxs-lookup"><span data-stu-id="f6a28-145">This command allows you tooadd and remove regions but does not allow you toomodify failover priorities.</span></span> <span data-ttu-id="f6a28-146">toomodify feladatátvételi prioritás, lásd: [alatt](#modify-failover-priority-powershell).</span><span class="sxs-lookup"><span data-stu-id="f6a28-146">toomodify failover priorities, see [below](#modify-failover-priority-powershell).</span></span>

    $locations = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0}, @{"locationName"="<read-region-location>"; "failoverPriority"=1})
    $iprangefilter = "<ip-range-filter>"
    $consistencyPolicy = @{"defaultConsistencyLevel"="<default-consistency-level>"; "maxIntervalInSeconds"="<max-interval>"; "maxStalenessPrefix"="<max-staleness-prefix>"}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    Set-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName <resource-group-name> -Name <database-account-name> -Properties $CosmosDBProperties
    
* <span data-ttu-id="f6a28-147">`<write-region-location>`hello helyének neve hello írási hello adatbázisfiók területet.</span><span class="sxs-lookup"><span data-stu-id="f6a28-147">`<write-region-location>` hello location name of hello write region of hello database account.</span></span> <span data-ttu-id="f6a28-148">Ez a hely szükség toohave feladatátvételi prioritási érték 0.</span><span class="sxs-lookup"><span data-stu-id="f6a28-148">This location is required toohave a failover priority value of 0.</span></span> <span data-ttu-id="f6a28-149">Másodpercenkénti adatbázis-fiók pontosan egy írási régióban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="f6a28-149">There must be exactly one write region per database account.</span></span>
* <span data-ttu-id="f6a28-150">`<read-region-location>`hello helyének neve hello olvasási hello adatbázisfiók területet.</span><span class="sxs-lookup"><span data-stu-id="f6a28-150">`<read-region-location>` hello location name of hello read region of hello database account.</span></span> <span data-ttu-id="f6a28-151">Ez a hely szükséges toohave 0-nál nagyobb feladatátvételi prioritási értéke.</span><span class="sxs-lookup"><span data-stu-id="f6a28-151">This location is required toohave a failover priority value of greater than 0.</span></span> <span data-ttu-id="f6a28-152">Másodpercenkénti adatbázis-fiók több mint egy olvasási régiók lehet.</span><span class="sxs-lookup"><span data-stu-id="f6a28-152">There can be more than one read regions per database account.</span></span>
* <span data-ttu-id="f6a28-153">`<default-consistency-level>`hello alapértelmezett konzisztencia szintje hello Azure Cosmos DB fiók.</span><span class="sxs-lookup"><span data-stu-id="f6a28-153">`<default-consistency-level>` hello default consistency level of hello Azure Cosmos DB account.</span></span> <span data-ttu-id="f6a28-154">További információkért lásd: [Azure Cosmos DB-ben Konzisztenciaszintek](consistency-levels.md).</span><span class="sxs-lookup"><span data-stu-id="f6a28-154">For more information, see [Consistency Levels in Azure Cosmos DB](consistency-levels.md).</span></span>
* <span data-ttu-id="f6a28-155">`<ip-range-filter>`Hello olyan IP-címek vagy IP-címtartományok engedélyezettek listájához, az ügyfél IP-címek egy adott adatbázis fiókjához tartozó hello néven CIDR formátumban toobe határozza meg.</span><span class="sxs-lookup"><span data-stu-id="f6a28-155">`<ip-range-filter>` Specifies hello set of IP addresses or IP address ranges in CIDR form toobe included as hello allowed list of client IPs for a given database account.</span></span> <span data-ttu-id="f6a28-156">IP-címeken/tartományokon vesszővel elválasztott, és nem tartalmazhat szóközt kell lennie.</span><span class="sxs-lookup"><span data-stu-id="f6a28-156">IP addresses/ranges must be comma separated and must not contain any spaces.</span></span> <span data-ttu-id="f6a28-157">További információkért lásd: [Azure Cosmos DB-Tűzfaltámogatás](firewall-support.md)</span><span class="sxs-lookup"><span data-stu-id="f6a28-157">For more information, see [Azure Cosmos DB Firewall Support](firewall-support.md)</span></span>
* <span data-ttu-id="f6a28-158">`<max-interval>`Kötött elavulási konzisztencia használata esetén ezt az értéket adja hello idő (másodpercben) elavulási megengedett.</span><span class="sxs-lookup"><span data-stu-id="f6a28-158">`<max-interval>` When used with Bounded Staleness consistency, this value represents hello time amount of staleness (in seconds) tolerated.</span></span> <span data-ttu-id="f6a28-159">Ez az érték elfogadható tartománya 1-100.</span><span class="sxs-lookup"><span data-stu-id="f6a28-159">Accepted range for this value is 1 - 100.</span></span>
* <span data-ttu-id="f6a28-160">`<max-staleness-prefix>`Kötött elavulási konzisztencia használata esetén ezt az értéket jelöli hello megengedett elavult kérelmek száma.</span><span class="sxs-lookup"><span data-stu-id="f6a28-160">`<max-staleness-prefix>` When used with Bounded Staleness consistency, this value represents hello number of stale requests tolerated.</span></span> <span data-ttu-id="f6a28-161">Ez az érték elfogadható tartománya 1 – 2 147 483 647.</span><span class="sxs-lookup"><span data-stu-id="f6a28-161">Accepted range for this value is 1 – 2,147,483,647.</span></span>
* <span data-ttu-id="f6a28-162">`<resource-group-name>`hello hello neve [Azure erőforráscsoport] [ azure-resource-groups] toowhich hello új Azure Cosmos DB adatbázis fiók tartozik.</span><span class="sxs-lookup"><span data-stu-id="f6a28-162">`<resource-group-name>` hello name of hello [Azure Resource Group][azure-resource-groups] toowhich hello new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="f6a28-163">`<resource-group-location>`hello Azure erőforráscsoport toowhich hello új Azure Cosmos DB adatbázisfiók hello helyét tartozik.</span><span class="sxs-lookup"><span data-stu-id="f6a28-163">`<resource-group-location>` hello location of hello Azure Resource Group toowhich hello new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="f6a28-164">`<database-account-name>`hello Azure Cosmos DB adatbázis fiók toobe frissített hello neve.</span><span class="sxs-lookup"><span data-stu-id="f6a28-164">`<database-account-name>` hello name of hello Azure Cosmos DB database account toobe updated.</span></span>

<span data-ttu-id="f6a28-165">Példa:</span><span class="sxs-lookup"><span data-stu-id="f6a28-165">Example:</span></span> 

    $locations = @(@{"locationName"="West US"; "failoverPriority"=0}, @{"locationName"="East US"; "failoverPriority"=1})
    $iprangefilter = ""
    $consistencyPolicy = @{"defaultConsistencyLevel"="BoundedStaleness"; "maxIntervalInSeconds"=5; "maxStalenessPrefix"=100}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    Set-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -Properties $CosmosDBProperties

## <span data-ttu-id="f6a28-166"><a id="delete-documentdb-account-powershell"></a>Egy DocumentDB-adatbázisfiók törlése</span><span class="sxs-lookup"><span data-stu-id="f6a28-166"><a id="delete-documentdb-account-powershell"></a> Delete a DocumentDB Database Account</span></span>

<span data-ttu-id="f6a28-167">Ez a parancs lehetővé teszi egy meglévő Azure Cosmos DB adatbázisfiók toodelete.</span><span class="sxs-lookup"><span data-stu-id="f6a28-167">This command allows you toodelete an existing Azure Cosmos DB database account.</span></span>

    Remove-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"
    
* <span data-ttu-id="f6a28-168">`<resource-group-name>`hello hello neve [Azure erőforráscsoport] [ azure-resource-groups] toowhich hello új Azure Cosmos DB adatbázis fiók tartozik.</span><span class="sxs-lookup"><span data-stu-id="f6a28-168">`<resource-group-name>` hello name of hello [Azure Resource Group][azure-resource-groups] toowhich hello new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="f6a28-169">`<database-account-name>`hello Azure Cosmos DB adatbázis fiók toobe törölt hello neve.</span><span class="sxs-lookup"><span data-stu-id="f6a28-169">`<database-account-name>` hello name of hello Azure Cosmos DB database account toobe deleted.</span></span>

<span data-ttu-id="f6a28-170">Példa:</span><span class="sxs-lookup"><span data-stu-id="f6a28-170">Example:</span></span>

    Remove-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <span data-ttu-id="f6a28-171"><a id="get-documentdb-properties-powershell"></a>A DocumentDB adatbázis fiók tulajdonságainak beolvasása</span><span class="sxs-lookup"><span data-stu-id="f6a28-171"><a id="get-documentdb-properties-powershell"></a> Get Properties of a DocumentDB Database Account</span></span>

<span data-ttu-id="f6a28-172">Ez a parancs lehetővé teszi egy meglévő Azure Cosmos DB adatbázis fiók tooget hello tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="f6a28-172">This command allows you tooget hello properties of an existing Azure Cosmos DB database account.</span></span>

    Get-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* <span data-ttu-id="f6a28-173">`<resource-group-name>`hello hello neve [Azure erőforráscsoport] [ azure-resource-groups] toowhich hello új Azure Cosmos DB adatbázis fiók tartozik.</span><span class="sxs-lookup"><span data-stu-id="f6a28-173">`<resource-group-name>` hello name of hello [Azure Resource Group][azure-resource-groups] toowhich hello new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="f6a28-174">`<database-account-name>`hello Azure Cosmos DB adatbázisfiók hello neve.</span><span class="sxs-lookup"><span data-stu-id="f6a28-174">`<database-account-name>` hello name of hello Azure Cosmos DB database account.</span></span>

<span data-ttu-id="f6a28-175">Példa:</span><span class="sxs-lookup"><span data-stu-id="f6a28-175">Example:</span></span>

    Get-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <span data-ttu-id="f6a28-176"><a id="update-tags-powershell"></a>Egy Azure Cosmos DB adatbázis fiók címkék frissítése</span><span class="sxs-lookup"><span data-stu-id="f6a28-176"><a id="update-tags-powershell"></a> Update Tags of an Azure Cosmos DB Database Account</span></span>

<span data-ttu-id="f6a28-177">hello alábbi példa bemutatja hogyan tooset [Azure erőforráscímkék] [ azure-resource-tags] az Azure Cosmos DB az adatbázis-fiók.</span><span class="sxs-lookup"><span data-stu-id="f6a28-177">hello following example describes how tooset [Azure resource tags][azure-resource-tags] for your Azure Cosmos DB database account.</span></span>

> [!NOTE]
> <span data-ttu-id="f6a28-178">Ez a parancs kombinálva hello létrehozni vagy frissíteni a parancsok hello hozzáfűzésével `-Tags` jelzőt mellékel hello ennek megfelelő paraméter.</span><span class="sxs-lookup"><span data-stu-id="f6a28-178">This command can be combined with hello create or update commands by appending hello `-Tags` flag with hello corresponding parameter.</span></span>

<span data-ttu-id="f6a28-179">Példa:</span><span class="sxs-lookup"><span data-stu-id="f6a28-179">Example:</span></span>

    $tags = @{"dept" = "Finance”; environment = “Production”}
    Set-AzureRmResource -ResourceType “Microsoft.DocumentDB/databaseAccounts”  -ResourceGroupName "rg-test" -Name "docdb-test" -Tags $tags

## <span data-ttu-id="f6a28-180"><a id="list-account-keys-powershell"></a>Fiók listázása</span><span class="sxs-lookup"><span data-stu-id="f6a28-180"><a id="list-account-keys-powershell"></a> List Account Keys</span></span>

<span data-ttu-id="f6a28-181">Egy Azure Cosmos DB fiók létrehozásakor hello szolgáltatás hello Azure Cosmos DB fiók használatakor a hitelesítéshez használt két fő tárelérési kulcsokat hoz létre.</span><span class="sxs-lookup"><span data-stu-id="f6a28-181">When you create an Azure Cosmos DB account, hello service generates two master access keys that can be used for authentication when hello Azure Cosmos DB account is accessed.</span></span> <span data-ttu-id="f6a28-182">Két tárelérési kulcsok megadásával Azure Cosmos DB lehetővé teszi tooregenerate hello kulcsok nem megszakítás tooyour Azure Cosmos DB fiók.</span><span class="sxs-lookup"><span data-stu-id="f6a28-182">By providing two access keys, Azure Cosmos DB enables you tooregenerate hello keys with no interruption tooyour Azure Cosmos DB account.</span></span> <span data-ttu-id="f6a28-183">Az olvasási műveletek csak olvasható kulcsokat is elérhetők.</span><span class="sxs-lookup"><span data-stu-id="f6a28-183">Read-only keys for authenticating read-only operations are also available.</span></span> <span data-ttu-id="f6a28-184">Nincsenek két írható-olvasható (elsődleges és másodlagos) és két írásvédett kulcsokat (elsődleges és másodlagos).</span><span class="sxs-lookup"><span data-stu-id="f6a28-184">There are two read-write keys (primary and secondary) and two read-only keys (primary and secondary).</span></span>

    $keys = Invoke-AzureRmResourceAction -Action listKeys -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* <span data-ttu-id="f6a28-185">`<resource-group-name>`hello hello neve [Azure erőforráscsoport] [ azure-resource-groups] toowhich hello új Azure Cosmos DB adatbázis fiók tartozik.</span><span class="sxs-lookup"><span data-stu-id="f6a28-185">`<resource-group-name>` hello name of hello [Azure Resource Group][azure-resource-groups] toowhich hello new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="f6a28-186">`<database-account-name>`hello Azure Cosmos DB adatbázisfiók hello neve.</span><span class="sxs-lookup"><span data-stu-id="f6a28-186">`<database-account-name>` hello name of hello Azure Cosmos DB database account.</span></span>

<span data-ttu-id="f6a28-187">Példa:</span><span class="sxs-lookup"><span data-stu-id="f6a28-187">Example:</span></span>

    $keys = Invoke-AzureRmResourceAction -Action listKeys -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <span data-ttu-id="f6a28-188"><a id="list-connection-strings-powershell"></a>Lista kapcsolati karakterláncok</span><span class="sxs-lookup"><span data-stu-id="f6a28-188"><a id="list-connection-strings-powershell"></a> List Connection Strings</span></span>

<span data-ttu-id="f6a28-189">A MongoDB fiókok hello kapcsolati karakterlánc tooconnect a MongoDB app toohello adatbázisfiók hello a következő parancs használatával lehet beolvasni.</span><span class="sxs-lookup"><span data-stu-id="f6a28-189">For MongoDB accounts, hello connection string tooconnect your MongoDB app toohello database account can be retrieved using hello following command.</span></span>

    $keys = Invoke-AzureRmResourceAction -Action listConnectionStrings -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* <span data-ttu-id="f6a28-190">`<resource-group-name>`hello hello neve [Azure erőforráscsoport] [ azure-resource-groups] toowhich hello új Azure Cosmos DB adatbázis fiók tartozik.</span><span class="sxs-lookup"><span data-stu-id="f6a28-190">`<resource-group-name>` hello name of hello [Azure Resource Group][azure-resource-groups] toowhich hello new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="f6a28-191">`<database-account-name>`hello Azure Cosmos DB adatbázisfiók hello neve.</span><span class="sxs-lookup"><span data-stu-id="f6a28-191">`<database-account-name>` hello name of hello Azure Cosmos DB database account.</span></span>

<span data-ttu-id="f6a28-192">Példa:</span><span class="sxs-lookup"><span data-stu-id="f6a28-192">Example:</span></span>

    $keys = Invoke-AzureRmResourceAction -Action listConnectionStrings -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <span data-ttu-id="f6a28-193"><a id="regenerate-account-key-powershell"></a>Fiók kulcs újragenerálása</span><span class="sxs-lookup"><span data-stu-id="f6a28-193"><a id="regenerate-account-key-powershell"></a> Regenerate Account Key</span></span>

<span data-ttu-id="f6a28-194">Meg kell változtatni hello hozzáférési kulcsok tooyour Azure Cosmos DB fiók rendszeresen toohelp a kapcsolatok nagyobb biztonságban.</span><span class="sxs-lookup"><span data-stu-id="f6a28-194">You should change hello access keys tooyour Azure Cosmos DB account periodically toohelp keep your connections more secure.</span></span> <span data-ttu-id="f6a28-195">Két tárelérési kulcsok rendelt tooenable meg toomaintain kapcsolatok toohello Azure Cosmos DB fiók az egyik kulcs, amíg a újragenerálja hello más hozzáférési kulcsot.</span><span class="sxs-lookup"><span data-stu-id="f6a28-195">Two access keys are assigned tooenable you toomaintain connections toohello Azure Cosmos DB account using one access key while you regenerate hello other access key.</span></span>

    Invoke-AzureRmResourceAction -Action regenerateKey -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>" -Parameters @{"keyKind"="<key-kind>"}

* <span data-ttu-id="f6a28-196">`<resource-group-name>`hello hello neve [Azure erőforráscsoport] [ azure-resource-groups] toowhich hello új Azure Cosmos DB adatbázis fiók tartozik.</span><span class="sxs-lookup"><span data-stu-id="f6a28-196">`<resource-group-name>` hello name of hello [Azure Resource Group][azure-resource-groups] toowhich hello new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="f6a28-197">`<database-account-name>`hello Azure Cosmos DB adatbázisfiók hello neve.</span><span class="sxs-lookup"><span data-stu-id="f6a28-197">`<database-account-name>` hello name of hello Azure Cosmos DB database account.</span></span>
* <span data-ttu-id="f6a28-198">`<key-kind>`Hello négy típusú kulcsok: ["Elsődleges" |} " Másodlagos "|}" PrimaryReadonly "|}" SecondaryReadonly"], hogy szeretné-e tooregenerate.</span><span class="sxs-lookup"><span data-stu-id="f6a28-198">`<key-kind>` One of hello four types of keys: ["Primary"|"Secondary"|"PrimaryReadonly"|"SecondaryReadonly"] that you would like tooregenerate.</span></span>

<span data-ttu-id="f6a28-199">Példa:</span><span class="sxs-lookup"><span data-stu-id="f6a28-199">Example:</span></span>

    Invoke-AzureRmResourceAction -Action regenerateKey -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -Parameters @{"keyKind"="Primary"}

## <span data-ttu-id="f6a28-200"><a id="modify-failover-priority-powershell"></a>Egy Azure Cosmos DB adatbázisfiók feladatátvételi prioritásának módosítása</span><span class="sxs-lookup"><span data-stu-id="f6a28-200"><a id="modify-failover-priority-powershell"></a> Modify Failover Priority of an Azure Cosmos DB Database Account</span></span>

<span data-ttu-id="f6a28-201">Több területi adatbázis fiókok a különböző régiókban, amelyek hello Azure Cosmos-adatbázis adatbázis-fiók létezik a hello hello feladatátvételi prioritásának módosításához.</span><span class="sxs-lookup"><span data-stu-id="f6a28-201">For multi-region database accounts, you can change hello failover priority of hello various regions which hello Azure Cosmos DB database account exists in.</span></span> <span data-ttu-id="f6a28-202">Feladatátvétel az Azure Cosmos DB adatbázis fiókban további információkért lásd: [adatok globálisan Azure Cosmos DB terjesztése][distribute-data-globally].</span><span class="sxs-lookup"><span data-stu-id="f6a28-202">For more information on failover in your Azure Cosmos DB database account, see [Distribute data globally with Azure Cosmos DB][distribute-data-globally].</span></span>

    $failoverPolicies = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0},@{"locationName"="<read-region-location>"; "failoverPriority"=1})
    Invoke-AzureRmResourceAction -Action failoverPriorityChange -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>" -Parameters @{"failoverPolicies"=$failoverPolicies}

* <span data-ttu-id="f6a28-203">`<write-region-location>`hello helyének neve hello írási hello adatbázisfiók területet.</span><span class="sxs-lookup"><span data-stu-id="f6a28-203">`<write-region-location>` hello location name of hello write region of hello database account.</span></span> <span data-ttu-id="f6a28-204">Ez a hely szükség toohave feladatátvételi prioritási érték 0.</span><span class="sxs-lookup"><span data-stu-id="f6a28-204">This location is required toohave a failover priority value of 0.</span></span> <span data-ttu-id="f6a28-205">Másodpercenkénti adatbázis-fiók pontosan egy írási régióban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="f6a28-205">There must be exactly one write region per database account.</span></span>
* <span data-ttu-id="f6a28-206">`<read-region-location>`hello helyének neve hello olvasási hello adatbázisfiók területet.</span><span class="sxs-lookup"><span data-stu-id="f6a28-206">`<read-region-location>` hello location name of hello read region of hello database account.</span></span> <span data-ttu-id="f6a28-207">Ez a hely szükséges toohave 0-nál nagyobb feladatátvételi prioritási értéke.</span><span class="sxs-lookup"><span data-stu-id="f6a28-207">This location is required toohave a failover priority value of greater than 0.</span></span> <span data-ttu-id="f6a28-208">Másodpercenkénti adatbázis-fiók több mint egy olvasási régiók lehet.</span><span class="sxs-lookup"><span data-stu-id="f6a28-208">There can be more than one read regions per database account.</span></span>
* <span data-ttu-id="f6a28-209">`<resource-group-name>`hello hello neve [Azure erőforráscsoport] [ azure-resource-groups] toowhich hello új Azure Cosmos DB adatbázis fiók tartozik.</span><span class="sxs-lookup"><span data-stu-id="f6a28-209">`<resource-group-name>` hello name of hello [Azure Resource Group][azure-resource-groups] toowhich hello new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="f6a28-210">`<database-account-name>`hello Azure Cosmos DB adatbázisfiók hello neve.</span><span class="sxs-lookup"><span data-stu-id="f6a28-210">`<database-account-name>` hello name of hello Azure Cosmos DB database account.</span></span>

<span data-ttu-id="f6a28-211">Példa:</span><span class="sxs-lookup"><span data-stu-id="f6a28-211">Example:</span></span>

    $failoverPolicies = @(@{"locationName"="East US"; "failoverPriority"=0},@{"locationName"="West US"; "failoverPriority"=1})
    Invoke-AzureRmResourceAction -Action failoverPriorityChange -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -Parameters @{"failoverPolicies"=$failoverPolicies}

## <a name="next-steps"></a><span data-ttu-id="f6a28-212">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f6a28-212">Next steps</span></span>

* <span data-ttu-id="f6a28-213">tooconnect használata a .NET, lásd: [kapcsolódás és lekérdezés .NET](create-documentdb-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="f6a28-213">tooconnect using .NET, see [Connect and query with .NET](create-documentdb-dotnet.md).</span></span>
* <span data-ttu-id="f6a28-214">használatával a .NET Core tooconnect lásd [kapcsolódás és lekérdezés a .NET Core platformmal](create-documentdb-dotnet-core.md).</span><span class="sxs-lookup"><span data-stu-id="f6a28-214">tooconnect using .NET Core, see [Connect and query with .NET Core](create-documentdb-dotnet-core.md).</span></span>
* <span data-ttu-id="f6a28-215">tooconnect használata Node.js-t, tekintse meg [kapcsolódás és lekérdezés a Node.js és a MongoDB-alkalmazás](create-mongodb-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="f6a28-215">tooconnect using Node.js, see [Connect and query with Node.js and a MongoDB app](create-mongodb-nodejs.md).</span></span>

<!--Reference style links - using these makes hello source content way more readable than using inline links-->
[powershell-install-configure]: https://docs.microsoft.com/azure/powershell-install-configure
[scaling-globally]: distribute-data-globally.md#EnableGlobalDistribution
[distribute-data-globally]: distribute-data-globally.md
[azure-resource-groups]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups
[azure-resource-tags]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags
[rp-rest-api]: https://docs.microsoft.com/rest/api/documentdbresourceprovider/
