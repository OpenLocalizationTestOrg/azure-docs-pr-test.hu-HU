---
title: "Az Azure Cosmos Adatbázisba regionális feladatátvétel |} Microsoft Docs"
description: "Tudnivalók Azure Cosmos DB hogyan manuális és automatikus feladatátvételt együttműködik."
services: cosmos-db
documentationcenter: 
author: arramac
manager: jhubbard
editor: 
ms.assetid: 446e2580-ff49-4485-8e53-ae34e08d997f
ms.service: cosmos-db
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/24/2017
ms.author: arramac
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3d8ba08bc9f99cb77c9f03949fc5db299eb222c8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="automatic-regional-failover-for-business-continuity-in-azure-cosmos-db"></a>Az üzletmenet folytonossága érdekében az Azure Cosmos Adatbázisba automatikus regionális feladatátvétel
Azure Cosmos DB egyszerűbbé teszi a globális adatok terjesztési felajánlásával teljes körűen felügyelt, [több területi adatbázis fiókok](distribute-data-globally.md) biztosító egyértelmű mellékhatásokkal konzisztencia, a rendelkezésre állás és a teljesítmény, az összes megfelelő garanciák között. Cosmos DB fiókok kínálnak a magas rendelkezésre állású, egyetlen számjegy ms késések, [jól meghatározott konzisztenciaszintek](consistency-levels.md), többhelyű API-khoz transzparens regionális feladatátvétel és rugalmasan méretezhető átviteli sebesség és tárterület világszerte képességét. 

Cosmos DB egyaránt explicit támogat, és a feladatátvétel vezérelt házirend, amely lehetővé teszi a végpont rendszerműködést ellenőrzési hibák esetén. Ebben a cikkben úgy tekintünk:

* Hogyan működnek a manuális feladatátvételt az Adatbázisba az Cosmos?
* Hogyan tegye Cosmos DB automatikus feladatátvétel munka és mi történik, ha egy data center álljon le?
* Hogyan használhatók kézi feladatátvételt alkalmazási architektúrákban található?

Is megismerheti az Azure-ban a regionális feladatátvétel videó péntek Scott Hanselman és egyszerű mérnöki Manager Karthik Raman.

>[!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Planet-Scale-NoSQL-with-DocumentDB/player]  

## <a id="ConfigureMultiRegionApplications"></a>Több területi alkalmazások konfigurálása
Ahhoz, hogy alaposabban feladatátvételi mód, úgy tekintünk, hogyan konfigurálhat egy alkalmazás több régiónkénti elérhetőség előnyeinek kihasználása és a regionális feladatátvétel állásuk rugalmasak lehetnek.

* Először is telepítse az alkalmazás több régióba
* Ahhoz, hogy kis késésű hozzáférést minden régióban, az alkalmazás központi telepítése, konfigurálása a megfelelő [előnyben részesített régiók listáját](https://msdn.microsoft.com/library/microsoft.azure.documents.client.connectionpolicy.preferredlocations.aspx#P:Microsoft.Azure.Documents.Client.ConnectionPolicy.PreferredLocations) keresztül valamelyik támogatott SDK-k mindegyik régióhoz.

Az alábbi részlet egy több területi alkalmazás inicializálása mutatja be. Itt, az Azure Cosmos DB fiók `contoso.documents.azure.com` két régió - USA nyugati régiója, Észak-Európa, és van konfigurálva. 

* Az alkalmazás központi telepítése (például az Azure App Services) régióban USA nyugati régiója 
* A konfigurált `West US` beolvassa az alacsony késleltetés első preferált régió szerint
* A konfigurált `North Europe` , a második preferált régió (magas rendelkezésre állásúként regionális esetén)

A DocumentDB az API-ban Ez a konfiguráció néz ki a következő kódrészletet:

```cs
ConnectionPolicy usConnectionPolicy = new ConnectionPolicy 
{ 
    ConnectionMode = ConnectionMode.Direct,
    ConnectionProtocol = Protocol.Tcp
};

usConnectionPolicy.PreferredLocations.Add(LocationNames.WestUS);
usConnectionPolicy.PreferredLocations.Add(LocationNames.NorthEurope);

DocumentClient usClient = new DocumentClient(
    new Uri("https://contosodb.documents.azure.com"),
    "memf7qfF89n6KL9vcb7rIQl6tfgZsRt5gY5dh3BIjesarJanYIcg2Edn9uPOUIVwgkAugOb2zUdCR2h0PTtMrA==",
    usConnectionPolicy);
```

Észak-Európa régióban előnyben részesített régiók fordított sorrendben is telepítik az alkalmazást. Ez azt jelenti, hogy az Észak-Európa régió van megadva először kis késleltetésű olvasása. Ezt követően az USA nyugati régiója régió van megadva a magas rendelkezésre állású második preferált régió regionális esetén.

Az alábbi architektúra ábrán látható a több területi alkalmazástelepítést, ahol Cosmos DB és az alkalmazás vannak konfigurálva a négy Azure földrajzi régióban elérhető legyen.  

![Azure Cosmos DB globálisan elosztott alkalmazás központi telepítése](./media/regional-failover/app-deployment.png)

Most hogyan kezeli a Cosmos DB szolgáltatás a regionális meghibásodásokkal keresztül automatikus feladatátvétel vizsgáljuk meg. 

## <a id="AutomaticFailovers"></a>Automatikus feladatátvétel
Egy Azure regionális kimaradás vagy az Adatközpont-meghibásodás után a ritka esetben Cosmos DB automatikusan elindítja a feladatátvételeket a Cosmos DB fiókoknak minden érintett régióban jelenlét. 

**Mi történik, ha egy olvasási régió kimaradás?**

Az érintett régiók egyikéhez sem olvasható régió cosmos DB fiókok automatikusan bontotta a kapcsolatot az írási régió és offline megjelölve. A Cosmos DB SDK-k egy regionális felderítési protokoll, amellyel automatikusan észlelés, ha egy régióban elérhetővé válik, és átirányítási, olvassa el a következő rendelkezésre álló terület a preferált régió listában hívásainak valósítja meg. Ha az elsődleges régióban listában régiók nincs ilyen, hívások automatikusan térhet vissza az aktuális írási területen. Nincs szükség módosításra az alkalmazás kódjában regionális feladatátvétel kezelésére. Ez a folyamat konzisztencia biztosítja továbbra is Cosmos DB kell ezt a funkciót.

![Olvassa el az Azure Cosmos DB régió hibái](./media/regional-failover/read-region-failures.png)

Az érintett régió helyreállít a a leállás, miután a rendszer automatikusan visszaállítja az érintett Cosmos DB fiókokhoz régióban a szolgáltatás. Cosmos DB fiókok, amelyekről egy olvasási területet az érintett területen, majd automatikusan aktuális írási terület szinkronizálhat és online kapcsolja. A Cosmos DB SDK-k Fedezze fel az új régió rendelkezésre állását, és mérlegelje, hogy a régióban kell kiválasztani, mint a jelenlegi olvasási terület, az alkalmazás által konfigurált preferált régió listája. További olvasási az alkalmazás kódjának módosítása nélkül a helyreállított régió van átirányítva.

**Mi történik, ha egy írási régió kimaradás?**

Ha az érintett régió Cosmos DB partner az aktuális írási területen, majd a régió jelzést kapnak, automatikusan offline állapotúként. Ezt követően egy másik régióban kell magát, a írási régió minden érintett Cosmos DB fiók. Teljes mértékben irányíthatja a régió kijelölés ahhoz, hogy a Cosmos DB fiókok az Azure-portálon vagy [programozott módon](https://docs.microsoft.com/rest/api/documentdbresourceprovider/databaseaccounts#DatabaseAccounts_FailoverPriorityChange). 

![Azure Cosmos DB feladatátvételi prioritásait](./media/regional-failover/failover-priorities.png)

Automatikus feladatátvételek során Cosmos DB automatikusan kiválasztja azt a következő írás Cosmos DB alapuló a megadott prioritás partner régióban. 

![Írja be Azure Cosmos DB régió hibák](./media/regional-failover/write-region-failures.png)

Az érintett régió helyreállít a a leállás, miután a rendszer automatikusan visszaállítja az érintett Cosmos DB fiókokhoz régióban a szolgáltatás. 

* Az előző írási régió érintett régióban cosmos DB fiókok marad offline módban a olvasási rendelkezésre állását a régió a helyreállítás után is. 
* Ebben a régióban számítási bármely árvává írási műveletek során a szolgáltatáskimaradás összehasonlítva az elérhető az aktuális adatok írása régió kérdezheti le. Annak megfelelően kell elvégezni az alkalmazás, egyesítési és/vagy az ütközés feloldása végrehajtása és a módosítások végső készletének írhat vissza az aktuális írási területen. 
* Miután végrehajtotta a módosítások, az érintett régió újra online helyezheti eltávolításával és olvasása a következő a régió Cosmos DB fiókjába. A régió adnak vissza, ha segítségével konfigurálhatja vissza az írási régió, egy manuális feladatátvételt az Azure-portálon elvégzésével vagy [programozott módon](https://docs.microsoft.com/rest/api/documentdbresourceprovider/databaseaccounts#DatabaseAccounts_CreateOrUpdate).

## <a id="ManualFailovers"></a>Manuális feladatátvétel

Automatikus feladatátvétel mellett az aktuális írási terület a megadott Cosmos DB fiók manuálisan módosítható dinamikusan a meglévő olvasási régiók egyikéhez sem. Kézi feladatátvételt is kezdeményezhető az Azure-portálon vagy [programozott módon](https://docs.microsoft.com/rest/api/documentdbresourceprovider/databaseaccounts#DatabaseAccounts_CreateOrUpdate). 

Manuális feladatátvétel biztosítására **adatvesztés nulla** és **nulla rendelkezésre állási** adatvesztéssel és szabályosan átviteli írási állapotát a régi régió írni a megadott Cosmos adatbázis fiók egy. Például az Automatikus feladatátvétel, a Cosmos DB SDK automatikusan kezeli az írási régió módosítások manuális feladatátvételek során és biztosítja, hogy a rendszer automatikusan átirányítja hívások az új írási terület. Az alkalmazása kezelje a feladatátvételek kód vagy a konfigurációs módosítások nélküli kell megadni. 

![Kézi feladatátvételt az Azure Cosmos-Adatbázisba](./media/regional-failover/manual-failovers.png)

A gyakori forgatókönyvek, ahol a kézi feladatátvételre hasznos lehet a következők:

**Hajtsa végre az óra modell**: Ha az alkalmazások kiszámítható forgalmi minták alapján a időpontját, rendszeres időközönként a írási állapotát módosíthatja a legtöbb aktív terület a mai időpont alapján.

**Szolgáltatásfrissítés**: bizonyos globálisan elosztott alkalmazás központi telepítésének magukban foglalhatják a forgalom átirányítását a traffic manager segítségével különböző régióban a tervezett szolgáltatás frissítése közben. Ilyen alkalmazás központi telepítése most kézi feladatátvételre használhatja arra, hogy az írási állapota a régió, ahol nem lesz aktív forgalom a szolgáltatás frissítési időszakban.

**Az üzletmenet folytonosságának és a vész-helyreállítási (BCDR) és a magas rendelkezésre állású és vész-helyreállítási (HADR) csukja**: a legtöbb vállalati alkalmazások tartalmazzák a üzleti folytonossági a fejlesztési és kiadás folyamat részeként. Gyakran a megfelelőségi tanúsítványokról és garanciavállaló szolgáltatás rendelkezésre állása regionális kimaradások esetén fontos lépés BCDR és a HADR tesztelése. Az alkalmazások, amelyek Cosmos DB használjanak tárhelyként Cosmos DB fiókja manuális feladatátvétel kiváltása és/vagy hozzáadása és eltávolítása dinamikusan terület által BCDR készültségét tesztelheti.

Ez a cikk azt felül Cosmos DB hogyan manuális és automatikus feladatátvételt munka, és hogyan konfigurálhatja a Cosmos DB fiókok és az alkalmazások globálisan elérhető legyen. Globális replikációs támogatási Cosmos DB használatával végpontok közötti késés javítása, és, hogy azok magas rendelkezésre állású még régió hibák esetén. 

## <a id="NextSteps"></a>Következő lépések
* További tudnivalók arról, hogyan támogatja a Cosmos DB [globális terjesztési](distribute-data-globally.md)
* További tudnivalók [az Azure Cosmos DB globális egységesítése](consistency-levels.md)
* Azure Cosmos DB használatával több régióba fejlesztést [DocumentDB API](../cosmos-db/tutorial-global-distribution-documentdb.md)
* Ismerje meg, hogyan hozhat létre [több területi író architektúrák](multi-region-writers.md) az Azure documentdb használatával

