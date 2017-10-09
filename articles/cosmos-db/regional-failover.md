---
title: "az Azure Cosmos Adatbázisba aaaRegional feladatátvételi |} Microsoft Docs"
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
ms.openlocfilehash: d2fdc7b0e8958d129ab027e4b11193b12961ddae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="automatic-regional-failover-for-business-continuity-in-azure-cosmos-db"></a>Az üzletmenet folytonossága érdekében az Azure Cosmos Adatbázisba automatikus regionális feladatátvétel
Azure Cosmos-adatbázis leegyszerűsíti az adatokat globális eloszlásáról hello felajánlásával teljes körűen felügyelt, [több területi adatbázis fiókok](distribute-data-globally.md) biztosító egyértelmű mellékhatásokkal konzisztencia, a rendelkezésre állás és a teljesítmény, mindezt megfelelő között garantálja. Cosmos DB fiókok kínálnak a magas rendelkezésre állású, egyetlen számjegy ms késések, [jól meghatározott konzisztenciaszintek](consistency-levels.md), többhelyű API-kat, és a hello képességét tooelastically méretezési átviteli sebesség és a tárterület átlátható regionális feladatátvétel különböző hello földgolyó méretét. 

Cosmos DB egyaránt explicit támogat, és vezérelt házirend feladatátvételek, amelyek lehetővé teszik toocontrol hello végpont rendszerműködést hello eseményben hibák. Ebben a cikkben úgy tekintünk:

* Hogyan működnek a manuális feladatátvételt az Adatbázisba az Cosmos?
* Hogyan tegye Cosmos DB automatikus feladatátvétel munka és mi történik, ha egy data center álljon le?
* Hogyan használhatók kézi feladatátvételt alkalmazási architektúrákban található?

Is megismerheti az Azure-ban a regionális feladatátvétel videó péntek Scott Hanselman és egyszerű mérnöki Manager Karthik Raman.

>[!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Planet-Scale-NoSQL-with-DocumentDB/player]  

## <a id="ConfigureMultiRegionApplications"></a>Több területi alkalmazások konfigurálása
Ahhoz, hogy alaposabban feladatátvételi mód, úgy tekintünk, hogyan konfigurálhatja az alkalmazás tootake előnyeit, több régiónkénti elérhetőség, és a regionális feladatátvétel hello felületét rugalmasak lehetnek.

* Először is telepítse az alkalmazás több régióba
* tooensure kis késésű hozzáférést minden régióban, az alkalmazás van telepítve, konfigurálja a megfelelő hello [előnyben részesített régiók listáját](https://msdn.microsoft.com/library/microsoft.azure.documents.client.connectionpolicy.preferredlocations.aspx#P:Microsoft.Azure.Documents.Client.ConnectionPolicy.PreferredLocations) minden egyes régió keresztül hello támogatott SDK-k.

a következő kódrészletet mutat be hogyan hello tooinitialize egy több területi alkalmazást. Itt hello Azure Cosmos DB fiók `contoso.documents.azure.com` két régió - USA nyugati régiója, Észak-Európa, és van konfigurálva. 

* hello alkalmazás van telepítve (például az Azure App Services) hello USA nyugati régiója régió 
* A konfigurált `West US` , az alacsony késleltetés preferált régió első hello olvassa be
* A konfigurált `North Europe` , hello második preferált régió (magas rendelkezésre állásúként regionális esetén)

A DocumentDB API hello Ez a konfiguráció a következő kódrészletet hello néz:

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

hello alkalmazás is van telepítve hello Észak-Európa régió hello sorrendjében, az előnyben részesített régiók fordított irányú. Ez azt jelenti, hogy hello Észak-Európa régió van megadva először kis késleltetésű olvasása. Majd hello USA nyugati régiója régió van megadva a magas rendelkezésre állású preferált régió második hello regionális esetén.

hello következő architektúrája mutat be, a több területi alkalmazástelepítést amelyben Cosmos DB és hello alkalmazás négy Azure földrajzi régióban elérhető konfigurált toobe.  

![Azure Cosmos DB globálisan elosztott alkalmazás központi telepítése](./media/regional-failover/app-deployment.png)

Most hogyan kezeli a hello Cosmos DB szolgáltatás a regionális meghibásodásokkal keresztül automatikus feladatátvétel vizsgáljuk meg. 

## <a id="AutomaticFailovers"></a>Automatikus feladatátvétel
Hello ritka esetben egy Azure regionális kimaradás vagy az Adatközpont-meghibásodás után Cosmos DB automatikusan elindítja az érintett hello régióban jelenlét fiókokhoz Cosmos DB feladatátvétele. 

**Mi történik, ha egy olvasási régió kimaradás?**

Az érintett hello régiók egyikéhez sem olvasható terület cosmos DB fiókok automatikusan bontotta a kapcsolatot az írási régió és offline megjelölve. hello Cosmos DB SDK-k megvalósítása egy regionális felderítési protokoll, amely lehetővé tenné tooautomatically észlelés, ha egy régióban elérhetővé válik, és átirányítási hívások toohello következő rendelkezésre álló terület hello preferált régió listában olvasható. Hello régiók hello az egyik elsődleges régió lista érhető el, ha hívások automatikusan térhet vissza toohello aktuális írási terület. Nem kell módosítania az alkalmazás kódja toohandle regionális feladatátvétel. Ez a folyamat konzisztencia biztosítja ezt a funkciót Cosmos DB toobe továbbra is.

![Olvassa el az Azure Cosmos DB régió hibái](./media/regional-failover/read-region-failures.png)

Érintett hello régió helyreállít a hello kimaradás, miután a rendszer automatikusan visszaállítja az összes érintett hello Cosmos DB fiókok hello régióban hello szolgáltatás. Olvasási régió volt hatással hello régióban cosmos DB fiókokat, majd automatikusan aktuális írási terület szinkronizálni és online kapcsolja. hello Cosmos DB SDK-k felderítése hello új régió hello rendelkezésre állását, és mérlegelje, hogy hello régió szerint hello aktuális olvasási terület hello alkalmazás által konfigurált hello preferált régió lista alapján kell kiválasztani. További olvasási átirányított toohello helyreállított régió anélkül, hogy a módosítások tooyour alkalmazáskód.

**Mi történik, ha egy írási régió kimaradás?**

Ha hello érintett régió hello aktuális írási terület Cosmos DB partner, majd hello régió jelzést kapnak, automatikusan offline állapotúként. Ezt követően egy másik régióban kell magát, hello írási régió minden érintett Cosmos DB felhasználói fiókhoz. Teljes mértékben irányíthatja hello régió kijelölés ahhoz, hogy a Cosmos DB fiókok hello Azure-portálon keresztül vagy [programozott módon](https://docs.microsoft.com/rest/api/documentdbresourceprovider/databaseaccounts#DatabaseAccounts_FailoverPriorityChange). 

![Azure Cosmos DB feladatátvételi prioritásait](./media/regional-failover/failover-priorities.png)

Automatikus feladatátvételek során Cosmos DB automatikusan kiválasztja azt hello következő írási terület a megadott Cosmos-DB hello-fiók megadva a prioritásuk szerinti sorrendben történik. 

![Írja be Azure Cosmos DB régió hibák](./media/regional-failover/write-region-failures.png)

Érintett hello régió helyreállít a hello kimaradás, miután a rendszer automatikusan visszaállítja az összes érintett hello Cosmos DB fiókok hello régióban hello szolgáltatás. 

* Az előző írási régió érintett hello régióban cosmos DB fiókok marad offline módban a olvasási rendelkezésre állását hello helyreállítási hello régió után is. 
* Lekérheti a régió toocompute bármilyen árvává írás hello kimaradás során összehasonlításával hello aktuális írási régióban elérhető hello adatokat. Az alkalmazás hello igények alapján, hajtsa végre az egyesítési és/vagy az ütközés feloldása, és a módosítások vissza toohello aktuális írási terület hello végső készletének írási. 
* Miután végrehajtotta a módosítások, helyezheti hatással hello régió vissza online eltávolításával és olvasása a következő hello régió tooyour Cosmos DB fiók. Hello régió adnak vissza, ha beállíthat azt vissza hello írási régió hello Azure-portálon keresztül kézi feladatátvételt hajt végre vagy [programozott módon](https://docs.microsoft.com/rest/api/documentdbresourceprovider/databaseaccounts#DatabaseAccounts_CreateOrUpdate).

## <a id="ManualFailovers"></a>Manuális feladatátvétel

Ezenkívül tooautomatic feladatátvételek, aktuális hello írási régió egy adott Cosmos DB fiók manuálisan módosítható dinamikusan hello meglévő olvasási régiók tooone. Kézi feladatátvételt is kezdeményezhető hello Azure-portálon keresztül vagy [programozott módon](https://docs.microsoft.com/rest/api/documentdbresourceprovider/databaseaccounts#DatabaseAccounts_CreateOrUpdate). 

Kézi feladatátvételt győződjön meg arról **adatvesztés nulla** és **rendelkezésre állási nulla** adatvesztés és szabályosan átvitel írási állapota régi hello írási régió toohello újat hello a megadott Cosmos DB fiók. Például az Automatikus feladatátvétel hello Cosmos DB SDK automatikusan kezeli az írási régió manuális feladatátvételek során módosítja, és gondoskodik arról, hogy hívások automatikusan átirányított toohello új írási terület. Kód vagy a konfigurációs módosítások nélküli szükségesek az alkalmazás toomanage feladatátvétele. 

![Kézi feladatátvételt az Azure Cosmos-Adatbázisba](./media/regional-failover/manual-failovers.png)

Néhány hello gyakori forgatókönyv, ahol kézi feladatátvételre hasznos lehet a következők:

**Hajtsa végre a hello óra modell**: Ha az alkalmazások kiszámítható forgalmi minták alapján hello hello időpontot, rendszeres időközönként módosíthatja hello írási állapot toohello legaktívabb földrajzi régió alapján hello időpontot.

**Szolgáltatásfrissítés**: bizonyos globálisan elosztott alkalmazás központi telepítésének magukban foglalhatják keresztül a traffic manager forgalom toodifferent régió átirányításához a tervezett szolgáltatás frissítése közben. Ilyen alkalmazás központi telepítésének most már használhatja kézi feladatátvételre tookeep hello írási állapot toohello régió ahol nincs érintetlen toobe aktív forgalom hello szolgáltatás frissítés időszak alatt.

**Az üzletmenet folytonosságának és a vész-helyreállítási (BCDR) és a magas rendelkezésre állású és vész-helyreállítási (HADR) csukja**: a legtöbb vállalati alkalmazások tartalmazzák a üzleti folytonossági a fejlesztési és kiadás folyamat részeként. BCDR és a HADR tesztelése legtöbbször megfelelőségi minősítései közül fontos lépés és garanciavállaló szolgáltatás rendelkezésre állása regionális kimaradások hello esetében. Az alkalmazások, amelyek Cosmos DB használjanak tárhelyként Cosmos DB fiókja manuális feladatátvétel kiváltása és/vagy hozzáadása és eltávolítása dinamikusan terület által hello BCDR készültségi tesztelheti.

Ez a cikk azt felül Cosmos DB hogyan manuális és automatikus feladatátvételt munka, és hogyan konfigurálhatja a Cosmos DB fiókok és alkalmazások toobe világszerte elérhető. Cosmos DB globális replikációs támogatásának segítségével, végpontok közötti késés javítása, és győződjön meg arról, hogy azok még hello esemény régió hibák a magas rendelkezésre állású. 

## <a id="NextSteps"></a>Következő lépések
* További tudnivalók arról, hogyan támogatja a Cosmos DB [globális terjesztési](distribute-data-globally.md)
* További tudnivalók [az Azure Cosmos DB globális egységesítése](consistency-levels.md)
* Azure Cosmos DB használatával több régióba fejlesztést [DocumentDB API](../cosmos-db/tutorial-global-distribution-documentdb.md)
* Megtudhatja, hogyan toobuild [több területi író architektúrák](multi-region-writers.md) az Azure documentdb használatával

