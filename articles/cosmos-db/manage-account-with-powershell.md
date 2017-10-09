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
# <a name="create-an-azure-cosmos-db-account-using-powershell"></a>PowerShell-lel Azure Cosmos DB-fiók létrehozása

hello Ez az útmutató ismerteti az Azure Cosmos DB adatbázis fiókok az Azure Powershell parancsok tooautomate kezelését. Emellett parancsok toomanage kulcsait és a feladatátvételi prioritások [több területi adatbázis fiókok][scaling-globally]. Az adatbázisfiók frissítése lehetővé teszi az toomodify konzisztencia házirendek és hozzáadása régiók. A platformok közötti felügyeleti Azure Cosmos DB-fiókja, választhat [Azure CLI](cli-samples.md), hello [erőforrás-szolgáltató REST API][rp-rest-api], vagy hello [Azure portál](create-documentdb-dotnet.md#create-account).

## <a name="getting-started"></a>Első lépések

Hello utasításait követve [hogyan tooinstall és konfigurálja az Azure Powershellt] [ powershell-install-configure] tooinstall és az Azure Resource Manager tooyour bejelentkezési fiók a PowerShellben.

### <a name="notes"></a>Megjegyzések

* Ha szeretné, hogy a következő parancsok a felhasználó jóváhagyásának kérése nélkül tooexecute hello, hozzáfűző hello `-Force` toohello parancs jelzőt.
* A következő parancsokat minden hello szinkronizáltak.

## <a id="create-documentdb-account-powershell"></a>Az Azure Cosmos DB-fiók létrehozása

Ez a parancs lehetővé teszi egy Azure Cosmos DB adatbázisfiók toocreate. Az új adatbázis-fiók beállítása vagy egyetlen területi vagy [több területi] [ scaling-globally] az egy bizonyos [konzisztencia házirend](consistency-levels.md).

    $locations = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0}, @{"locationName"="<read-region-location>"; "failoverPriority"=1})
    $iprangefilter = "<ip-range-filter>"
    $consistencyPolicy = @{"defaultConsistencyLevel"="<default-consistency-level>"; "maxIntervalInSeconds"="<max-interval>"; "maxStalenessPrefix"="<max-staleness-prefix>"}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    New-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName <resource-group-name>  -Location "<resource-group-location>" -Name <database-account-name> -Properties $CosmosDBProperties
    
* `<write-region-location>`hello helyének neve hello írási hello adatbázisfiók területet. Ez a hely szükség toohave feladatátvételi prioritási érték 0. Másodpercenkénti adatbázis-fiók pontosan egy írási régióban kell lennie.
* `<read-region-location>`hello helyének neve hello olvasási hello adatbázisfiók területet. Ez a hely szükséges toohave 0-nál nagyobb feladatátvételi prioritási értéke. Másodpercenkénti adatbázis-fiók több mint egy olvasási régiók lehet.
* `<ip-range-filter>`Hello olyan IP-címek vagy IP-címtartományok engedélyezettek listájához, az ügyfél IP-címek egy adott adatbázis fiókjához tartozó hello néven CIDR formátumban toobe határozza meg. IP-címeken/tartományokon vesszővel elválasztott, és nem tartalmazhat szóközt kell lennie. További információkért lásd: [Azure Cosmos DB-Tűzfaltámogatás](firewall-support.md)
* `<default-consistency-level>`hello alapértelmezett konzisztencia szintje hello Azure Cosmos DB fiók. További információkért lásd: [Azure Cosmos DB-ben Konzisztenciaszintek](consistency-levels.md).
* `<max-interval>`Kötött elavulási konzisztencia használata esetén ezt az értéket adja hello idő (másodpercben) elavulási megengedett. Ez az érték elfogadható tartománya 1-100.
* `<max-staleness-prefix>`Kötött elavulási konzisztencia használata esetén ezt az értéket jelöli hello megengedett elavult kérelmek száma. Ez az érték elfogadható tartománya 1 – 2 147 483 647.
* `<resource-group-name>`hello hello neve [Azure erőforráscsoport] [ azure-resource-groups] toowhich hello új Azure Cosmos DB adatbázis fiók tartozik.
* `<resource-group-location>`hello Azure erőforráscsoport toowhich hello új Azure Cosmos DB adatbázisfiók hello helyét tartozik.
* `<database-account-name>`hello Azure Cosmos DB adatbázis fiók toobe létrehozott hello neve. Csak használhat kisbetűket, számokat, hello "-" karakter, és 3 – 50 karakter közé kell esnie.

Példa: 

    $locations = @(@{"locationName"="West US"; "failoverPriority"=0}, @{"locationName"="East US"; "failoverPriority"=1})
    $iprangefilter = ""
    $consistencyPolicy = @{"defaultConsistencyLevel"="BoundedStaleness"; "maxIntervalInSeconds"=5; "maxStalenessPrefix"=100}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    New-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Location "West US" -Name "docdb-test" -Properties $CosmosDBProperties

### <a name="notes"></a>Megjegyzések
* hello előző példa hoz létre egy adatbázis-fiók két régióban. Akkor is lehetséges toocreate egy adatbázis-fiók egy régió tartozik (amely hello írási régió van kijelölve, és a feladatátvételi prioritási értéke csak 0) vagy a több mint két régióban. További információkért lásd: [több területi adatbázis fiókok][scaling-globally].
* hello helyét, amelyben Azure Cosmos DB általánosan elérhető régiók kell lennie. hello aktuális területek listája a hello [Azure-régiókat lap](https://azure.microsoft.com/regions/#services).

## <a id="update-documentdb-account-powershell"></a>A DocumentDB-adatbázisfiók frissítése

Ez a parancs tooupdate lehetővé teszi az Azure Cosmos DB adatbázis fiók tulajdonságait. Ez magában foglalja a hello konzisztencia házirend és hello helyek, mely hello adatbázis fiók létezik-e.

> [!NOTE]
> Ez a parancs lehetővé teszi a tooadd és eltávolítása, de nem teszi lehetővé a toomodify feladatátvételi prioritások. toomodify feladatátvételi prioritás, lásd: [alatt](#modify-failover-priority-powershell).

    $locations = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0}, @{"locationName"="<read-region-location>"; "failoverPriority"=1})
    $iprangefilter = "<ip-range-filter>"
    $consistencyPolicy = @{"defaultConsistencyLevel"="<default-consistency-level>"; "maxIntervalInSeconds"="<max-interval>"; "maxStalenessPrefix"="<max-staleness-prefix>"}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    Set-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName <resource-group-name> -Name <database-account-name> -Properties $CosmosDBProperties
    
* `<write-region-location>`hello helyének neve hello írási hello adatbázisfiók területet. Ez a hely szükség toohave feladatátvételi prioritási érték 0. Másodpercenkénti adatbázis-fiók pontosan egy írási régióban kell lennie.
* `<read-region-location>`hello helyének neve hello olvasási hello adatbázisfiók területet. Ez a hely szükséges toohave 0-nál nagyobb feladatátvételi prioritási értéke. Másodpercenkénti adatbázis-fiók több mint egy olvasási régiók lehet.
* `<default-consistency-level>`hello alapértelmezett konzisztencia szintje hello Azure Cosmos DB fiók. További információkért lásd: [Azure Cosmos DB-ben Konzisztenciaszintek](consistency-levels.md).
* `<ip-range-filter>`Hello olyan IP-címek vagy IP-címtartományok engedélyezettek listájához, az ügyfél IP-címek egy adott adatbázis fiókjához tartozó hello néven CIDR formátumban toobe határozza meg. IP-címeken/tartományokon vesszővel elválasztott, és nem tartalmazhat szóközt kell lennie. További információkért lásd: [Azure Cosmos DB-Tűzfaltámogatás](firewall-support.md)
* `<max-interval>`Kötött elavulási konzisztencia használata esetén ezt az értéket adja hello idő (másodpercben) elavulási megengedett. Ez az érték elfogadható tartománya 1-100.
* `<max-staleness-prefix>`Kötött elavulási konzisztencia használata esetén ezt az értéket jelöli hello megengedett elavult kérelmek száma. Ez az érték elfogadható tartománya 1 – 2 147 483 647.
* `<resource-group-name>`hello hello neve [Azure erőforráscsoport] [ azure-resource-groups] toowhich hello új Azure Cosmos DB adatbázis fiók tartozik.
* `<resource-group-location>`hello Azure erőforráscsoport toowhich hello új Azure Cosmos DB adatbázisfiók hello helyét tartozik.
* `<database-account-name>`hello Azure Cosmos DB adatbázis fiók toobe frissített hello neve.

Példa: 

    $locations = @(@{"locationName"="West US"; "failoverPriority"=0}, @{"locationName"="East US"; "failoverPriority"=1})
    $iprangefilter = ""
    $consistencyPolicy = @{"defaultConsistencyLevel"="BoundedStaleness"; "maxIntervalInSeconds"=5; "maxStalenessPrefix"=100}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    Set-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -Properties $CosmosDBProperties

## <a id="delete-documentdb-account-powershell"></a>Egy DocumentDB-adatbázisfiók törlése

Ez a parancs lehetővé teszi egy meglévő Azure Cosmos DB adatbázisfiók toodelete.

    Remove-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"
    
* `<resource-group-name>`hello hello neve [Azure erőforráscsoport] [ azure-resource-groups] toowhich hello új Azure Cosmos DB adatbázis fiók tartozik.
* `<database-account-name>`hello Azure Cosmos DB adatbázis fiók toobe törölt hello neve.

Példa:

    Remove-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <a id="get-documentdb-properties-powershell"></a>A DocumentDB adatbázis fiók tulajdonságainak beolvasása

Ez a parancs lehetővé teszi egy meglévő Azure Cosmos DB adatbázis fiók tooget hello tulajdonságait.

    Get-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* `<resource-group-name>`hello hello neve [Azure erőforráscsoport] [ azure-resource-groups] toowhich hello új Azure Cosmos DB adatbázis fiók tartozik.
* `<database-account-name>`hello Azure Cosmos DB adatbázisfiók hello neve.

Példa:

    Get-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <a id="update-tags-powershell"></a>Egy Azure Cosmos DB adatbázis fiók címkék frissítése

hello alábbi példa bemutatja hogyan tooset [Azure erőforráscímkék] [ azure-resource-tags] az Azure Cosmos DB az adatbázis-fiók.

> [!NOTE]
> Ez a parancs kombinálva hello létrehozni vagy frissíteni a parancsok hello hozzáfűzésével `-Tags` jelzőt mellékel hello ennek megfelelő paraméter.

Példa:

    $tags = @{"dept" = "Finance”; environment = “Production”}
    Set-AzureRmResource -ResourceType “Microsoft.DocumentDB/databaseAccounts”  -ResourceGroupName "rg-test" -Name "docdb-test" -Tags $tags

## <a id="list-account-keys-powershell"></a>Fiók listázása

Egy Azure Cosmos DB fiók létrehozásakor hello szolgáltatás hello Azure Cosmos DB fiók használatakor a hitelesítéshez használt két fő tárelérési kulcsokat hoz létre. Két tárelérési kulcsok megadásával Azure Cosmos DB lehetővé teszi tooregenerate hello kulcsok nem megszakítás tooyour Azure Cosmos DB fiók. Az olvasási műveletek csak olvasható kulcsokat is elérhetők. Nincsenek két írható-olvasható (elsődleges és másodlagos) és két írásvédett kulcsokat (elsődleges és másodlagos).

    $keys = Invoke-AzureRmResourceAction -Action listKeys -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* `<resource-group-name>`hello hello neve [Azure erőforráscsoport] [ azure-resource-groups] toowhich hello új Azure Cosmos DB adatbázis fiók tartozik.
* `<database-account-name>`hello Azure Cosmos DB adatbázisfiók hello neve.

Példa:

    $keys = Invoke-AzureRmResourceAction -Action listKeys -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <a id="list-connection-strings-powershell"></a>Lista kapcsolati karakterláncok

A MongoDB fiókok hello kapcsolati karakterlánc tooconnect a MongoDB app toohello adatbázisfiók hello a következő parancs használatával lehet beolvasni.

    $keys = Invoke-AzureRmResourceAction -Action listConnectionStrings -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* `<resource-group-name>`hello hello neve [Azure erőforráscsoport] [ azure-resource-groups] toowhich hello új Azure Cosmos DB adatbázis fiók tartozik.
* `<database-account-name>`hello Azure Cosmos DB adatbázisfiók hello neve.

Példa:

    $keys = Invoke-AzureRmResourceAction -Action listConnectionStrings -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <a id="regenerate-account-key-powershell"></a>Fiók kulcs újragenerálása

Meg kell változtatni hello hozzáférési kulcsok tooyour Azure Cosmos DB fiók rendszeresen toohelp a kapcsolatok nagyobb biztonságban. Két tárelérési kulcsok rendelt tooenable meg toomaintain kapcsolatok toohello Azure Cosmos DB fiók az egyik kulcs, amíg a újragenerálja hello más hozzáférési kulcsot.

    Invoke-AzureRmResourceAction -Action regenerateKey -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>" -Parameters @{"keyKind"="<key-kind>"}

* `<resource-group-name>`hello hello neve [Azure erőforráscsoport] [ azure-resource-groups] toowhich hello új Azure Cosmos DB adatbázis fiók tartozik.
* `<database-account-name>`hello Azure Cosmos DB adatbázisfiók hello neve.
* `<key-kind>`Hello négy típusú kulcsok: ["Elsődleges" |} " Másodlagos "|}" PrimaryReadonly "|}" SecondaryReadonly"], hogy szeretné-e tooregenerate.

Példa:

    Invoke-AzureRmResourceAction -Action regenerateKey -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -Parameters @{"keyKind"="Primary"}

## <a id="modify-failover-priority-powershell"></a>Egy Azure Cosmos DB adatbázisfiók feladatátvételi prioritásának módosítása

Több területi adatbázis fiókok a különböző régiókban, amelyek hello Azure Cosmos-adatbázis adatbázis-fiók létezik a hello hello feladatátvételi prioritásának módosításához. Feladatátvétel az Azure Cosmos DB adatbázis fiókban további információkért lásd: [adatok globálisan Azure Cosmos DB terjesztése][distribute-data-globally].

    $failoverPolicies = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0},@{"locationName"="<read-region-location>"; "failoverPriority"=1})
    Invoke-AzureRmResourceAction -Action failoverPriorityChange -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>" -Parameters @{"failoverPolicies"=$failoverPolicies}

* `<write-region-location>`hello helyének neve hello írási hello adatbázisfiók területet. Ez a hely szükség toohave feladatátvételi prioritási érték 0. Másodpercenkénti adatbázis-fiók pontosan egy írási régióban kell lennie.
* `<read-region-location>`hello helyének neve hello olvasási hello adatbázisfiók területet. Ez a hely szükséges toohave 0-nál nagyobb feladatátvételi prioritási értéke. Másodpercenkénti adatbázis-fiók több mint egy olvasási régiók lehet.
* `<resource-group-name>`hello hello neve [Azure erőforráscsoport] [ azure-resource-groups] toowhich hello új Azure Cosmos DB adatbázis fiók tartozik.
* `<database-account-name>`hello Azure Cosmos DB adatbázisfiók hello neve.

Példa:

    $failoverPolicies = @(@{"locationName"="East US"; "failoverPriority"=0},@{"locationName"="West US"; "failoverPriority"=1})
    Invoke-AzureRmResourceAction -Action failoverPriorityChange -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -Parameters @{"failoverPolicies"=$failoverPolicies}

## <a name="next-steps"></a>Következő lépések

* tooconnect használata a .NET, lásd: [kapcsolódás és lekérdezés .NET](create-documentdb-dotnet.md).
* használatával a .NET Core tooconnect lásd [kapcsolódás és lekérdezés a .NET Core platformmal](create-documentdb-dotnet-core.md).
* tooconnect használata Node.js-t, tekintse meg [kapcsolódás és lekérdezés a Node.js és a MongoDB-alkalmazás](create-mongodb-nodejs.md).

<!--Reference style links - using these makes hello source content way more readable than using inline links-->
[powershell-install-configure]: https://docs.microsoft.com/azure/powershell-install-configure
[scaling-globally]: distribute-data-globally.md#EnableGlobalDistribution
[distribute-data-globally]: distribute-data-globally.md
[azure-resource-groups]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups
[azure-resource-tags]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags
[rp-rest-api]: https://docs.microsoft.com/rest/api/documentdbresourceprovider/
