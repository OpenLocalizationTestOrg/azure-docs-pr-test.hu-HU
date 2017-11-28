---
title: "Az Azure Cosmos DB Automation - felügyelet a PowerShell használatával |} Microsoft Docs"
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
ms.openlocfilehash: 25c543528119410dff0684845a713dcb0d6151d6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-cosmos-db-account-using-powershell"></a><span data-ttu-id="dbb74-103">PowerShell-lel Azure Cosmos DB-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="dbb74-103">Create an Azure Cosmos DB account using PowerShell</span></span>

<span data-ttu-id="dbb74-104">Ez az útmutató ismerteti a parancsok automatizált felügyelete az Azure Powershell Azure Cosmos DB adatbázis fiók.</span><span class="sxs-lookup"><span data-stu-id="dbb74-104">The following guide describes commands to automate management of your Azure Cosmos DB database accounts using Azure Powershell.</span></span> <span data-ttu-id="dbb74-105">Parancsok kulcsait és a feladatátvételi prioritások kezelésére is tartalmaz [több területi adatbázis fiókok][scaling-globally].</span><span class="sxs-lookup"><span data-stu-id="dbb74-105">It also includes commands to manage account keys and failover priorities in [multi-region database accounts][scaling-globally].</span></span> <span data-ttu-id="dbb74-106">Az adatbázisfiók frissítése lehetővé teszi a konzisztencia-házirendek módosíthatók és régiók hozzáadása/eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="dbb74-106">Updating your database account allows you to modify consistency policies and add/remove regions.</span></span> <span data-ttu-id="dbb74-107">A platformok közötti felügyeleti Azure Cosmos DB-fiókja, választhat [Azure CLI](cli-samples.md), a [erőforrás-szolgáltató REST API][rp-rest-api], vagy a [Azure-portálon ](create-documentdb-dotnet.md#create-account).</span><span class="sxs-lookup"><span data-stu-id="dbb74-107">For cross-platform management of your Azure Cosmos DB account, you can use either [Azure CLI](cli-samples.md), the [Resource Provider REST API][rp-rest-api], or the [Azure portal](create-documentdb-dotnet.md#create-account).</span></span>

## <a name="getting-started"></a><span data-ttu-id="dbb74-108">Első lépések</span><span class="sxs-lookup"><span data-stu-id="dbb74-108">Getting Started</span></span>

<span data-ttu-id="dbb74-109">Kövesse az utasításokat a [telepítése és konfigurálása az Azure PowerShell] [ powershell-install-configure] telepítéséhez, és jelentkezzen be az Azure Resource Manager fiókjába a PowerShellben.</span><span class="sxs-lookup"><span data-stu-id="dbb74-109">Follow the instructions in [How to install and configure Azure PowerShell][powershell-install-configure] to install and log in to your Azure Resource Manager account in Powershell.</span></span>

### <a name="notes"></a><span data-ttu-id="dbb74-110">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="dbb74-110">Notes</span></span>

* <span data-ttu-id="dbb74-111">Ha szeretné hajtható végre az alábbi parancsokat a felhasználó jóváhagyásának kérése nélkül, hozzáfűző a `-Force` jelzőjét, hogy a parancs.</span><span class="sxs-lookup"><span data-stu-id="dbb74-111">If you would like to execute the following commands without requiring user confirmation, append the `-Force` flag to the command.</span></span>
* <span data-ttu-id="dbb74-112">A következő parancsok szinkronizáltak.</span><span class="sxs-lookup"><span data-stu-id="dbb74-112">All the following commands are synchronous.</span></span>

## <span data-ttu-id="dbb74-113"><a id="create-documentdb-account-powershell"></a>Az Azure Cosmos DB-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="dbb74-113"><a id="create-documentdb-account-powershell"></a> Create an Azure Cosmos DB Account</span></span>

<span data-ttu-id="dbb74-114">Ez a parancs egy Azure Cosmos DB adatbázisfiók létrehozása teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="dbb74-114">This command allows you to create an Azure Cosmos DB database account.</span></span> <span data-ttu-id="dbb74-115">Az új adatbázis-fiók beállítása vagy egyetlen területi vagy [több területi] [ scaling-globally] az egy bizonyos [konzisztencia házirend](consistency-levels.md).</span><span class="sxs-lookup"><span data-stu-id="dbb74-115">Configure your new database account as either single-region or [multi-region][scaling-globally] with a certain [consistency policy](consistency-levels.md).</span></span>

    $locations = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0}, @{"locationName"="<read-region-location>"; "failoverPriority"=1})
    $iprangefilter = "<ip-range-filter>"
    $consistencyPolicy = @{"defaultConsistencyLevel"="<default-consistency-level>"; "maxIntervalInSeconds"="<max-interval>"; "maxStalenessPrefix"="<max-staleness-prefix>"}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    New-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName <resource-group-name>  -Location "<resource-group-location>" -Name <database-account-name> -Properties $CosmosDBProperties
    
* <span data-ttu-id="dbb74-116">`<write-region-location>`Az adatbázisfiók írási régiója hely neve.</span><span class="sxs-lookup"><span data-stu-id="dbb74-116">`<write-region-location>` The location name of the write region of the database account.</span></span> <span data-ttu-id="dbb74-117">Ezen a helyen szükség van a feladatátvételi prioritási értéke 0.</span><span class="sxs-lookup"><span data-stu-id="dbb74-117">This location is required to have a failover priority value of 0.</span></span> <span data-ttu-id="dbb74-118">Másodpercenkénti adatbázis-fiók pontosan egy írási régióban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="dbb74-118">There must be exactly one write region per database account.</span></span>
* <span data-ttu-id="dbb74-119">`<read-region-location>`Az adatbázisfiók írásvédett régió hely neve.</span><span class="sxs-lookup"><span data-stu-id="dbb74-119">`<read-region-location>` The location name of the read region of the database account.</span></span> <span data-ttu-id="dbb74-120">Ez a hely szükséges feladatátvevő prioritás értéke csak 0-nál nagyobb.</span><span class="sxs-lookup"><span data-stu-id="dbb74-120">This location is required to have a failover priority value of greater than 0.</span></span> <span data-ttu-id="dbb74-121">Másodpercenkénti adatbázis-fiók több mint egy olvasási régiók lehet.</span><span class="sxs-lookup"><span data-stu-id="dbb74-121">There can be more than one read regions per database account.</span></span>
* <span data-ttu-id="dbb74-122">`<ip-range-filter>`Megadja az IP-címek vagy IP-címtartományok része, mint az engedélyezett bővítmények listájához ügyfél IP-címek egy adott adatbázis fiókjához tartozó CIDR formátumban.</span><span class="sxs-lookup"><span data-stu-id="dbb74-122">`<ip-range-filter>` Specifies the set of IP addresses or IP address ranges in CIDR form to be included as the allowed list of client IPs for a given database account.</span></span> <span data-ttu-id="dbb74-123">IP-címeken/tartományokon vesszővel elválasztott, és nem tartalmazhat szóközt kell lennie.</span><span class="sxs-lookup"><span data-stu-id="dbb74-123">IP addresses/ranges must be comma separated and must not contain any spaces.</span></span> <span data-ttu-id="dbb74-124">További információkért lásd: [Azure Cosmos DB-Tűzfaltámogatás](firewall-support.md)</span><span class="sxs-lookup"><span data-stu-id="dbb74-124">For more information, see [Azure Cosmos DB Firewall Support](firewall-support.md)</span></span>
* <span data-ttu-id="dbb74-125">`<default-consistency-level>`A Azure Cosmos DB-fiók alapértelmezett konzisztencia szintjét.</span><span class="sxs-lookup"><span data-stu-id="dbb74-125">`<default-consistency-level>` The default consistency level of the Azure Cosmos DB account.</span></span> <span data-ttu-id="dbb74-126">További információkért lásd: [Azure Cosmos DB-ben Konzisztenciaszintek](consistency-levels.md).</span><span class="sxs-lookup"><span data-stu-id="dbb74-126">For more information, see [Consistency Levels in Azure Cosmos DB](consistency-levels.md).</span></span>
* <span data-ttu-id="dbb74-127">`<max-interval>`Kötött elavulási konzisztencia használata esetén ezt az értéket jelenti idő (másodpercben) elavulási megengedett.</span><span class="sxs-lookup"><span data-stu-id="dbb74-127">`<max-interval>` When used with Bounded Staleness consistency, this value represents the time amount of staleness (in seconds) tolerated.</span></span> <span data-ttu-id="dbb74-128">Ez az érték elfogadható tartománya 1-100.</span><span class="sxs-lookup"><span data-stu-id="dbb74-128">Accepted range for this value is 1 - 100.</span></span>
* <span data-ttu-id="dbb74-129">`<max-staleness-prefix>`Kötött elavulási konzisztencia használata esetén ezt az értéket jelöli megengedett elavult kérelmek számát jelenti.</span><span class="sxs-lookup"><span data-stu-id="dbb74-129">`<max-staleness-prefix>` When used with Bounded Staleness consistency, this value represents the number of stale requests tolerated.</span></span> <span data-ttu-id="dbb74-130">Ez az érték elfogadható tartománya 1 – 2 147 483 647.</span><span class="sxs-lookup"><span data-stu-id="dbb74-130">Accepted range for this value is 1 – 2,147,483,647.</span></span>
* <span data-ttu-id="dbb74-131">`<resource-group-name>`Neve a [Azure erőforráscsoport] [ azure-resource-groups] , amelyhez az új Azure Cosmos DB adatbázisfiók való tartozik.</span><span class="sxs-lookup"><span data-stu-id="dbb74-131">`<resource-group-name>` The name of the [Azure Resource Group][azure-resource-groups] to which the new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="dbb74-132">`<resource-group-location>`Az Azure-erőforráscsoportot, amelyhez az új Azure Cosmos DB adatbázis fiók tartozik helyét.</span><span class="sxs-lookup"><span data-stu-id="dbb74-132">`<resource-group-location>` The location of the Azure Resource Group to which the new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="dbb74-133">`<database-account-name>`A létrehozandó Azure Cosmos DB fiók neve.</span><span class="sxs-lookup"><span data-stu-id="dbb74-133">`<database-account-name>` The name of the Azure Cosmos DB database account to be created.</span></span> <span data-ttu-id="dbb74-134">Csak kisbetűket, számokat, képes használni a "-" karakter, és 3 – 50 karakter közé kell esnie.</span><span class="sxs-lookup"><span data-stu-id="dbb74-134">It can only use lowercase letters, numbers, the '-' character, and must be between 3 and 50 characters.</span></span>

<span data-ttu-id="dbb74-135">Példa:</span><span class="sxs-lookup"><span data-stu-id="dbb74-135">Example:</span></span> 

    $locations = @(@{"locationName"="West US"; "failoverPriority"=0}, @{"locationName"="East US"; "failoverPriority"=1})
    $iprangefilter = ""
    $consistencyPolicy = @{"defaultConsistencyLevel"="BoundedStaleness"; "maxIntervalInSeconds"=5; "maxStalenessPrefix"=100}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    New-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Location "West US" -Name "docdb-test" -Properties $CosmosDBProperties

### <a name="notes"></a><span data-ttu-id="dbb74-136">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="dbb74-136">Notes</span></span>
* <span data-ttu-id="dbb74-137">A fenti példában egy adatbázis-fiók két régió hoz létre.</span><span class="sxs-lookup"><span data-stu-id="dbb74-137">The preceding example creates a database account with two regions.</span></span> <span data-ttu-id="dbb74-138">Akkor is egy régió tartozik (amely a írási régió van kijelölve, és a feladatátvételi prioritási értéke csak 0) vagy a több mint két régiók adatbázis-fiók létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="dbb74-138">It is also possible to create a database account with either one region (which is designated as the write region and have a failover priority value of 0) or more than two regions.</span></span> <span data-ttu-id="dbb74-139">További információkért lásd: [több területi adatbázis fiókok][scaling-globally].</span><span class="sxs-lookup"><span data-stu-id="dbb74-139">For more information, see [multi-region database accounts][scaling-globally].</span></span>
* <span data-ttu-id="dbb74-140">A helyek, amelyben Azure Cosmos DB általánosan elérhető régiók kell lennie.</span><span class="sxs-lookup"><span data-stu-id="dbb74-140">The locations must be regions in which Azure Cosmos DB is generally available.</span></span> <span data-ttu-id="dbb74-141">Az aktuális területek listája a a [Azure-régiókat lap](https://azure.microsoft.com/regions/#services).</span><span class="sxs-lookup"><span data-stu-id="dbb74-141">The current list of regions is provided on the [Azure Regions page](https://azure.microsoft.com/regions/#services).</span></span>

## <span data-ttu-id="dbb74-142"><a id="update-documentdb-account-powershell"></a>A DocumentDB-adatbázisfiók frissítése</span><span class="sxs-lookup"><span data-stu-id="dbb74-142"><a id="update-documentdb-account-powershell"></a> Update a DocumentDB Database Account</span></span>

<span data-ttu-id="dbb74-143">Ez a parancs lehetővé teszi az Azure Cosmos DB adatbázis fiók tulajdonságainak módosítása.</span><span class="sxs-lookup"><span data-stu-id="dbb74-143">This command allows you to update your Azure Cosmos DB database account properties.</span></span> <span data-ttu-id="dbb74-144">Ez magában foglalja a konzisztencia-házirend és a helyek, amely az adatbázis-fiók létezik-e.</span><span class="sxs-lookup"><span data-stu-id="dbb74-144">This includes the consistency policy and the locations which the database account exists in.</span></span>

> [!NOTE]
> <span data-ttu-id="dbb74-145">Ez a parancs lehetővé teszi a hozzáadása és eltávolítása a régiókban, de nem engedélyezi feladatátvételi prioritások módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="dbb74-145">This command allows you to add and remove regions but does not allow you to modify failover priorities.</span></span> <span data-ttu-id="dbb74-146">Feladatátvételi prioritások módosításához lásd [alatt](#modify-failover-priority-powershell).</span><span class="sxs-lookup"><span data-stu-id="dbb74-146">To modify failover priorities, see [below](#modify-failover-priority-powershell).</span></span>

    $locations = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0}, @{"locationName"="<read-region-location>"; "failoverPriority"=1})
    $iprangefilter = "<ip-range-filter>"
    $consistencyPolicy = @{"defaultConsistencyLevel"="<default-consistency-level>"; "maxIntervalInSeconds"="<max-interval>"; "maxStalenessPrefix"="<max-staleness-prefix>"}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    Set-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName <resource-group-name> -Name <database-account-name> -Properties $CosmosDBProperties
    
* <span data-ttu-id="dbb74-147">`<write-region-location>`Az adatbázisfiók írási régiója hely neve.</span><span class="sxs-lookup"><span data-stu-id="dbb74-147">`<write-region-location>` The location name of the write region of the database account.</span></span> <span data-ttu-id="dbb74-148">Ezen a helyen szükség van a feladatátvételi prioritási értéke 0.</span><span class="sxs-lookup"><span data-stu-id="dbb74-148">This location is required to have a failover priority value of 0.</span></span> <span data-ttu-id="dbb74-149">Másodpercenkénti adatbázis-fiók pontosan egy írási régióban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="dbb74-149">There must be exactly one write region per database account.</span></span>
* <span data-ttu-id="dbb74-150">`<read-region-location>`Az adatbázisfiók írásvédett régió hely neve.</span><span class="sxs-lookup"><span data-stu-id="dbb74-150">`<read-region-location>` The location name of the read region of the database account.</span></span> <span data-ttu-id="dbb74-151">Ez a hely szükséges feladatátvevő prioritás értéke csak 0-nál nagyobb.</span><span class="sxs-lookup"><span data-stu-id="dbb74-151">This location is required to have a failover priority value of greater than 0.</span></span> <span data-ttu-id="dbb74-152">Másodpercenkénti adatbázis-fiók több mint egy olvasási régiók lehet.</span><span class="sxs-lookup"><span data-stu-id="dbb74-152">There can be more than one read regions per database account.</span></span>
* <span data-ttu-id="dbb74-153">`<default-consistency-level>`A Azure Cosmos DB-fiók alapértelmezett konzisztencia szintjét.</span><span class="sxs-lookup"><span data-stu-id="dbb74-153">`<default-consistency-level>` The default consistency level of the Azure Cosmos DB account.</span></span> <span data-ttu-id="dbb74-154">További információkért lásd: [Azure Cosmos DB-ben Konzisztenciaszintek](consistency-levels.md).</span><span class="sxs-lookup"><span data-stu-id="dbb74-154">For more information, see [Consistency Levels in Azure Cosmos DB](consistency-levels.md).</span></span>
* <span data-ttu-id="dbb74-155">`<ip-range-filter>`Megadja az IP-címek vagy IP-címtartományok része, mint az engedélyezett bővítmények listájához ügyfél IP-címek egy adott adatbázis fiókjához tartozó CIDR formátumban.</span><span class="sxs-lookup"><span data-stu-id="dbb74-155">`<ip-range-filter>` Specifies the set of IP addresses or IP address ranges in CIDR form to be included as the allowed list of client IPs for a given database account.</span></span> <span data-ttu-id="dbb74-156">IP-címeken/tartományokon vesszővel elválasztott, és nem tartalmazhat szóközt kell lennie.</span><span class="sxs-lookup"><span data-stu-id="dbb74-156">IP addresses/ranges must be comma separated and must not contain any spaces.</span></span> <span data-ttu-id="dbb74-157">További információkért lásd: [Azure Cosmos DB-Tűzfaltámogatás](firewall-support.md)</span><span class="sxs-lookup"><span data-stu-id="dbb74-157">For more information, see [Azure Cosmos DB Firewall Support](firewall-support.md)</span></span>
* <span data-ttu-id="dbb74-158">`<max-interval>`Kötött elavulási konzisztencia használata esetén ezt az értéket jelenti idő (másodpercben) elavulási megengedett.</span><span class="sxs-lookup"><span data-stu-id="dbb74-158">`<max-interval>` When used with Bounded Staleness consistency, this value represents the time amount of staleness (in seconds) tolerated.</span></span> <span data-ttu-id="dbb74-159">Ez az érték elfogadható tartománya 1-100.</span><span class="sxs-lookup"><span data-stu-id="dbb74-159">Accepted range for this value is 1 - 100.</span></span>
* <span data-ttu-id="dbb74-160">`<max-staleness-prefix>`Kötött elavulási konzisztencia használata esetén ezt az értéket jelöli megengedett elavult kérelmek számát jelenti.</span><span class="sxs-lookup"><span data-stu-id="dbb74-160">`<max-staleness-prefix>` When used with Bounded Staleness consistency, this value represents the number of stale requests tolerated.</span></span> <span data-ttu-id="dbb74-161">Ez az érték elfogadható tartománya 1 – 2 147 483 647.</span><span class="sxs-lookup"><span data-stu-id="dbb74-161">Accepted range for this value is 1 – 2,147,483,647.</span></span>
* <span data-ttu-id="dbb74-162">`<resource-group-name>`Neve a [Azure erőforráscsoport] [ azure-resource-groups] , amelyhez az új Azure Cosmos DB adatbázisfiók való tartozik.</span><span class="sxs-lookup"><span data-stu-id="dbb74-162">`<resource-group-name>` The name of the [Azure Resource Group][azure-resource-groups] to which the new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="dbb74-163">`<resource-group-location>`Az Azure-erőforráscsoportot, amelyhez az új Azure Cosmos DB adatbázis fiók tartozik helyét.</span><span class="sxs-lookup"><span data-stu-id="dbb74-163">`<resource-group-location>` The location of the Azure Resource Group to which the new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="dbb74-164">`<database-account-name>`Frissíteni kell az Azure Cosmos DB adatbázis fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="dbb74-164">`<database-account-name>` The name of the Azure Cosmos DB database account to be updated.</span></span>

<span data-ttu-id="dbb74-165">Példa:</span><span class="sxs-lookup"><span data-stu-id="dbb74-165">Example:</span></span> 

    $locations = @(@{"locationName"="West US"; "failoverPriority"=0}, @{"locationName"="East US"; "failoverPriority"=1})
    $iprangefilter = ""
    $consistencyPolicy = @{"defaultConsistencyLevel"="BoundedStaleness"; "maxIntervalInSeconds"=5; "maxStalenessPrefix"=100}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    Set-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -Properties $CosmosDBProperties

## <span data-ttu-id="dbb74-166"><a id="delete-documentdb-account-powershell"></a>Egy DocumentDB-adatbázisfiók törlése</span><span class="sxs-lookup"><span data-stu-id="dbb74-166"><a id="delete-documentdb-account-powershell"></a> Delete a DocumentDB Database Account</span></span>

<span data-ttu-id="dbb74-167">Ez a parancs lehetővé teszi egy meglévő Azure Cosmos DB adatbázisfiók törlése.</span><span class="sxs-lookup"><span data-stu-id="dbb74-167">This command allows you to delete an existing Azure Cosmos DB database account.</span></span>

    Remove-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"
    
* <span data-ttu-id="dbb74-168">`<resource-group-name>`Neve a [Azure erőforráscsoport] [ azure-resource-groups] , amelyhez az új Azure Cosmos DB adatbázisfiók való tartozik.</span><span class="sxs-lookup"><span data-stu-id="dbb74-168">`<resource-group-name>` The name of the [Azure Resource Group][azure-resource-groups] to which the new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="dbb74-169">`<database-account-name>`A törlendő Azure Cosmos DB fiók neve.</span><span class="sxs-lookup"><span data-stu-id="dbb74-169">`<database-account-name>` The name of the Azure Cosmos DB database account to be deleted.</span></span>

<span data-ttu-id="dbb74-170">Példa:</span><span class="sxs-lookup"><span data-stu-id="dbb74-170">Example:</span></span>

    Remove-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <span data-ttu-id="dbb74-171"><a id="get-documentdb-properties-powershell"></a>A DocumentDB adatbázis fiók tulajdonságainak beolvasása</span><span class="sxs-lookup"><span data-stu-id="dbb74-171"><a id="get-documentdb-properties-powershell"></a> Get Properties of a DocumentDB Database Account</span></span>

<span data-ttu-id="dbb74-172">A parancs lehetővé teszi, hogy egy meglévő Azure Cosmos DB adatbázisfiók tulajdonságainak beolvasása.</span><span class="sxs-lookup"><span data-stu-id="dbb74-172">This command allows you to get the properties of an existing Azure Cosmos DB database account.</span></span>

    Get-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* <span data-ttu-id="dbb74-173">`<resource-group-name>`Neve a [Azure erőforráscsoport] [ azure-resource-groups] , amelyhez az új Azure Cosmos DB adatbázisfiók való tartozik.</span><span class="sxs-lookup"><span data-stu-id="dbb74-173">`<resource-group-name>` The name of the [Azure Resource Group][azure-resource-groups] to which the new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="dbb74-174">`<database-account-name>`Az Azure Cosmos adatbázis adatbázis-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="dbb74-174">`<database-account-name>` The name of the Azure Cosmos DB database account.</span></span>

<span data-ttu-id="dbb74-175">Példa:</span><span class="sxs-lookup"><span data-stu-id="dbb74-175">Example:</span></span>

    Get-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <span data-ttu-id="dbb74-176"><a id="update-tags-powershell"></a>Egy Azure Cosmos DB adatbázis fiók címkék frissítése</span><span class="sxs-lookup"><span data-stu-id="dbb74-176"><a id="update-tags-powershell"></a> Update Tags of an Azure Cosmos DB Database Account</span></span>

<span data-ttu-id="dbb74-177">Az alábbi példa ismerteti, hogyan lehet beállítani [Azure erőforráscímkék] [ azure-resource-tags] az Azure Cosmos DB az adatbázis-fiók.</span><span class="sxs-lookup"><span data-stu-id="dbb74-177">The following example describes how to set [Azure resource tags][azure-resource-tags] for your Azure Cosmos DB database account.</span></span>

> [!NOTE]
> <span data-ttu-id="dbb74-178">Ez a parancs kombinálva, a létrehozás vagy frissítés parancsok hozzáfűzésével a `-Tags` jelzőt mellékel a megfelelő paraméter.</span><span class="sxs-lookup"><span data-stu-id="dbb74-178">This command can be combined with the create or update commands by appending the `-Tags` flag with the corresponding parameter.</span></span>

<span data-ttu-id="dbb74-179">Példa:</span><span class="sxs-lookup"><span data-stu-id="dbb74-179">Example:</span></span>

    $tags = @{"dept" = "Finance”; environment = “Production”}
    Set-AzureRmResource -ResourceType “Microsoft.DocumentDB/databaseAccounts”  -ResourceGroupName "rg-test" -Name "docdb-test" -Tags $tags

## <span data-ttu-id="dbb74-180"><a id="list-account-keys-powershell"></a>Fiók listázása</span><span class="sxs-lookup"><span data-stu-id="dbb74-180"><a id="list-account-keys-powershell"></a> List Account Keys</span></span>

<span data-ttu-id="dbb74-181">Egy Azure Cosmos DB fiók létrehozásakor a szolgáltatás két fő kulcsot, amely az Azure Cosmos DB fiók elérésekor hitelesítéshez használt állít elő.</span><span class="sxs-lookup"><span data-stu-id="dbb74-181">When you create an Azure Cosmos DB account, the service generates two master access keys that can be used for authentication when the Azure Cosmos DB account is accessed.</span></span> <span data-ttu-id="dbb74-182">Két tárelérési kulcsok megadásával Azure Cosmos DB teszi Azure Cosmos DB fiókjába megszakítás nélkül a kulcsok újragenerálását.</span><span class="sxs-lookup"><span data-stu-id="dbb74-182">By providing two access keys, Azure Cosmos DB enables you to regenerate the keys with no interruption to your Azure Cosmos DB account.</span></span> <span data-ttu-id="dbb74-183">Az olvasási műveletek csak olvasható kulcsokat is elérhetők.</span><span class="sxs-lookup"><span data-stu-id="dbb74-183">Read-only keys for authenticating read-only operations are also available.</span></span> <span data-ttu-id="dbb74-184">Nincsenek két írható-olvasható (elsődleges és másodlagos) és két írásvédett kulcsokat (elsődleges és másodlagos).</span><span class="sxs-lookup"><span data-stu-id="dbb74-184">There are two read-write keys (primary and secondary) and two read-only keys (primary and secondary).</span></span>

    $keys = Invoke-AzureRmResourceAction -Action listKeys -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* <span data-ttu-id="dbb74-185">`<resource-group-name>`Neve a [Azure erőforráscsoport] [ azure-resource-groups] , amelyhez az új Azure Cosmos DB adatbázisfiók való tartozik.</span><span class="sxs-lookup"><span data-stu-id="dbb74-185">`<resource-group-name>` The name of the [Azure Resource Group][azure-resource-groups] to which the new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="dbb74-186">`<database-account-name>`Az Azure Cosmos adatbázis adatbázis-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="dbb74-186">`<database-account-name>` The name of the Azure Cosmos DB database account.</span></span>

<span data-ttu-id="dbb74-187">Példa:</span><span class="sxs-lookup"><span data-stu-id="dbb74-187">Example:</span></span>

    $keys = Invoke-AzureRmResourceAction -Action listKeys -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <span data-ttu-id="dbb74-188"><a id="list-connection-strings-powershell"></a>Lista kapcsolati karakterláncok</span><span class="sxs-lookup"><span data-stu-id="dbb74-188"><a id="list-connection-strings-powershell"></a> List Connection Strings</span></span>

<span data-ttu-id="dbb74-189">A MongoDB-fiókok a kapcsolati karakterláncot a MongoDB-alkalmazás csatlakoztatása a következő adatbázisfiókot lehet beolvasni a következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="dbb74-189">For MongoDB accounts, the connection string to connect your MongoDB app to the database account can be retrieved using the following command.</span></span>

    $keys = Invoke-AzureRmResourceAction -Action listConnectionStrings -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* <span data-ttu-id="dbb74-190">`<resource-group-name>`Neve a [Azure erőforráscsoport] [ azure-resource-groups] , amelyhez az új Azure Cosmos DB adatbázisfiók való tartozik.</span><span class="sxs-lookup"><span data-stu-id="dbb74-190">`<resource-group-name>` The name of the [Azure Resource Group][azure-resource-groups] to which the new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="dbb74-191">`<database-account-name>`Az Azure Cosmos adatbázis adatbázis-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="dbb74-191">`<database-account-name>` The name of the Azure Cosmos DB database account.</span></span>

<span data-ttu-id="dbb74-192">Példa:</span><span class="sxs-lookup"><span data-stu-id="dbb74-192">Example:</span></span>

    $keys = Invoke-AzureRmResourceAction -Action listConnectionStrings -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <span data-ttu-id="dbb74-193"><a id="regenerate-account-key-powershell"></a>Fiók kulcs újragenerálása</span><span class="sxs-lookup"><span data-stu-id="dbb74-193"><a id="regenerate-account-key-powershell"></a> Regenerate Account Key</span></span>

<span data-ttu-id="dbb74-194">Módosítsa a tárelérési kulcsok rendszeres időközönként a Azure Cosmos DB fiókját biztonságosabbá a kapcsolatokat.</span><span class="sxs-lookup"><span data-stu-id="dbb74-194">You should change the access keys to your Azure Cosmos DB account periodically to help keep your connections more secure.</span></span> <span data-ttu-id="dbb74-195">Engedélyezi, hogy az egyik kulcs, amíg a többi hozzáférési kulcs újragenerálása Azure Cosmos DB fiókhoz kapcsolatok karbantartása két tárelérési kulcsok vannak hozzárendelve.</span><span class="sxs-lookup"><span data-stu-id="dbb74-195">Two access keys are assigned to enable you to maintain connections to the Azure Cosmos DB account using one access key while you regenerate the other access key.</span></span>

    Invoke-AzureRmResourceAction -Action regenerateKey -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>" -Parameters @{"keyKind"="<key-kind>"}

* <span data-ttu-id="dbb74-196">`<resource-group-name>`Neve a [Azure erőforráscsoport] [ azure-resource-groups] , amelyhez az új Azure Cosmos DB adatbázisfiók való tartozik.</span><span class="sxs-lookup"><span data-stu-id="dbb74-196">`<resource-group-name>` The name of the [Azure Resource Group][azure-resource-groups] to which the new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="dbb74-197">`<database-account-name>`Az Azure Cosmos adatbázis adatbázis-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="dbb74-197">`<database-account-name>` The name of the Azure Cosmos DB database account.</span></span>
* <span data-ttu-id="dbb74-198">`<key-kind>`A négy típusú kulcsok: ["Elsődleges" |} " Másodlagos "|}" PrimaryReadonly "|}" SecondaryReadonly"], amely szeretné generálni.</span><span class="sxs-lookup"><span data-stu-id="dbb74-198">`<key-kind>` One of the four types of keys: ["Primary"|"Secondary"|"PrimaryReadonly"|"SecondaryReadonly"] that you would like to regenerate.</span></span>

<span data-ttu-id="dbb74-199">Példa:</span><span class="sxs-lookup"><span data-stu-id="dbb74-199">Example:</span></span>

    Invoke-AzureRmResourceAction -Action regenerateKey -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -Parameters @{"keyKind"="Primary"}

## <span data-ttu-id="dbb74-200"><a id="modify-failover-priority-powershell"></a>Egy Azure Cosmos DB adatbázisfiók feladatátvételi prioritásának módosítása</span><span class="sxs-lookup"><span data-stu-id="dbb74-200"><a id="modify-failover-priority-powershell"></a> Modify Failover Priority of an Azure Cosmos DB Database Account</span></span>

<span data-ttu-id="dbb74-201">Több területi adatbázis fiókok módosíthatja a különböző régiókban, amelyek az Azure Cosmos DB adatbázis fiók létezik-e a feladatátvételi prioritását.</span><span class="sxs-lookup"><span data-stu-id="dbb74-201">For multi-region database accounts, you can change the failover priority of the various regions which the Azure Cosmos DB database account exists in.</span></span> <span data-ttu-id="dbb74-202">Feladatátvétel az Azure Cosmos DB adatbázis fiókban további információkért lásd: [adatok globálisan Azure Cosmos DB terjesztése][distribute-data-globally].</span><span class="sxs-lookup"><span data-stu-id="dbb74-202">For more information on failover in your Azure Cosmos DB database account, see [Distribute data globally with Azure Cosmos DB][distribute-data-globally].</span></span>

    $failoverPolicies = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0},@{"locationName"="<read-region-location>"; "failoverPriority"=1})
    Invoke-AzureRmResourceAction -Action failoverPriorityChange -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>" -Parameters @{"failoverPolicies"=$failoverPolicies}

* <span data-ttu-id="dbb74-203">`<write-region-location>`Az adatbázisfiók írási régiója hely neve.</span><span class="sxs-lookup"><span data-stu-id="dbb74-203">`<write-region-location>` The location name of the write region of the database account.</span></span> <span data-ttu-id="dbb74-204">Ezen a helyen szükség van a feladatátvételi prioritási értéke 0.</span><span class="sxs-lookup"><span data-stu-id="dbb74-204">This location is required to have a failover priority value of 0.</span></span> <span data-ttu-id="dbb74-205">Másodpercenkénti adatbázis-fiók pontosan egy írási régióban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="dbb74-205">There must be exactly one write region per database account.</span></span>
* <span data-ttu-id="dbb74-206">`<read-region-location>`Az adatbázisfiók írásvédett régió hely neve.</span><span class="sxs-lookup"><span data-stu-id="dbb74-206">`<read-region-location>` The location name of the read region of the database account.</span></span> <span data-ttu-id="dbb74-207">Ez a hely szükséges feladatátvevő prioritás értéke csak 0-nál nagyobb.</span><span class="sxs-lookup"><span data-stu-id="dbb74-207">This location is required to have a failover priority value of greater than 0.</span></span> <span data-ttu-id="dbb74-208">Másodpercenkénti adatbázis-fiók több mint egy olvasási régiók lehet.</span><span class="sxs-lookup"><span data-stu-id="dbb74-208">There can be more than one read regions per database account.</span></span>
* <span data-ttu-id="dbb74-209">`<resource-group-name>`Neve a [Azure erőforráscsoport] [ azure-resource-groups] , amelyhez az új Azure Cosmos DB adatbázisfiók való tartozik.</span><span class="sxs-lookup"><span data-stu-id="dbb74-209">`<resource-group-name>` The name of the [Azure Resource Group][azure-resource-groups] to which the new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="dbb74-210">`<database-account-name>`Az Azure Cosmos adatbázis adatbázis-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="dbb74-210">`<database-account-name>` The name of the Azure Cosmos DB database account.</span></span>

<span data-ttu-id="dbb74-211">Példa:</span><span class="sxs-lookup"><span data-stu-id="dbb74-211">Example:</span></span>

    $failoverPolicies = @(@{"locationName"="East US"; "failoverPriority"=0},@{"locationName"="West US"; "failoverPriority"=1})
    Invoke-AzureRmResourceAction -Action failoverPriorityChange -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -Parameters @{"failoverPolicies"=$failoverPolicies}

## <a name="next-steps"></a><span data-ttu-id="dbb74-212">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="dbb74-212">Next steps</span></span>

* <span data-ttu-id="dbb74-213">A csatlakozás, .NET használatával, lásd: [kapcsolódás és lekérdezés .NET](create-documentdb-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="dbb74-213">To connect using .NET, see [Connect and query with .NET](create-documentdb-dotnet.md).</span></span>
* <span data-ttu-id="dbb74-214">Csatlakozni a .NET Core segítségével, lásd: [kapcsolódás és lekérdezés a .NET Core platformmal](create-documentdb-dotnet-core.md).</span><span class="sxs-lookup"><span data-stu-id="dbb74-214">To connect using .NET Core, see [Connect and query with .NET Core](create-documentdb-dotnet-core.md).</span></span>
* <span data-ttu-id="dbb74-215">A csatlakozás, Node.js használatával, lásd: [kapcsolódás és lekérdezés a Node.js és a MongoDB-alkalmazás](create-mongodb-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="dbb74-215">To connect using Node.js, see [Connect and query with Node.js and a MongoDB app](create-mongodb-nodejs.md).</span></span>

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[powershell-install-configure]: https://docs.microsoft.com/azure/powershell-install-configure
[scaling-globally]: distribute-data-globally.md#EnableGlobalDistribution
[distribute-data-globally]: distribute-data-globally.md
[azure-resource-groups]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups
[azure-resource-tags]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags
[rp-rest-api]: https://docs.microsoft.com/rest/api/documentdbresourceprovider/
