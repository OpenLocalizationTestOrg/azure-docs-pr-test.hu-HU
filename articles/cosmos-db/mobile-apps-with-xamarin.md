---
title: "a Xamarinnal és Azure Cosmos DB aaaBuild mobilalkalmazások |} Microsoft Docs"
description: "Egy oktatóanyag, amely a Xamarin iOS, Android vagy űrlapok alkalmazás Azure Cosmos DB használatával. Azure Cosmos DB egy gyors, a bolygónk méretezés, a felhő adatbázis mobilalkalmazásokhoz."
services: cosmos-db
documentationcenter: .net
author: arramac
manager: monicar
editor: 
ms.assetid: ff97881a-b41a-499d-b7ab-4f394df0e153
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/03/2017
ms.author: arramac
ms.openlocfilehash: 362a0e239a61e1309e1cf720c23151760952cf69
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="build-mobile-applications-with-xamarin-and-azure-cosmos-db"></a>Mobil a Xamarinnal és Azure Cosmos DB használó alkalmazások
A legtöbb mobileszköz alkalmazások toostore adatokra hello felhőben van szükségük, és Azure Cosmos DB mobilalkalmazások felhő adatbázis. Minden mobileszköz fejlesztőnek rendelkezik. Az igény szerinti méretezi szolgáltatásként is teljes körűen felügyelt adatbázis. Azt is, vonja az adatok tooyour alkalmazás átlátható módon, a felhasználók körül hello földgömb helyétől. Hello segítségével [Azure Cosmos DB .NET Core SDK](documentdb-sdk-dotnet-core.md), engedélyezheti a Xamarin mobilalkalmazások toointeract közvetlenül az Azure Cosmos DB, a középső réteg nélkül.

Ez a cikk nyújt segítséget a Xamarinnal és Azure Cosmos DB mobilalkalmazások létrehozása. Hello oktatóanyag: hello teljes forráskód található [Xamarinnal és Azure Cosmos DB a Githubon](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/xamarin), egyebek között toomanage felhasználókat és engedélyeket.

## <a name="azure-cosmos-db-capabilities-for-mobile-apps"></a>Az Azure Cosmos DB képességet biztosít a mobile apps szolgáltatásban
Azure Cosmos DB főbb funkciók a mobilalkalmazás-fejlesztőknek következő hello biztosítja:

![Az Azure Cosmos DB képességet biztosít a mobile apps szolgáltatásban](media/mobile-apps-with-xamarin/documentdb-for-mobile.png)

* A lekérdezések gazdag séma nélküli adatokat. Azure Cosmos-adatbázis a JSON-dokumentumokként séma nélküli heterogén gyűjteményekben tárolja az adatokat. Kínál [gazdag és gyors lekérdezéseket](documentdb-sql-query.md) hello nélkül kell tooworry sémák és indexek.
* Gyors teljesítmény. Csak néhány ezredmásodperc Azure Cosmos DB tooread és a write-dokumentumok szükséges. A fejlesztők megadhatja a hello átviteli van szükségük, és Azure Cosmos DB eleget tegyen, akkor a 99,99 százalékos SLA-k.
* Korlátlan skála. Az Azure Cosmos DB gyűjtemények [nő, ahogy az alkalmazás forgalmához igazítható](partition-data.md). Kisebb adatméret és átviteli kérések száma másodpercenként több száz indítható. A gyűjtemények toopetabytes tetszőlegesen nagy átviteli sebesség és a kérések száma másodpercenként több millió száz növelhető.
* Globálisan elosztott. Felhasználók vannak a hello mobilalkalmazás lépjen, gyakran hello world között. Az Azure Cosmos DB van egy [globálisan elosztott adatbázis](distribute-data-globally.md). Kattintson a hello térkép toomake elérhető tooyour felhasználóit.
* Beépített gazdag engedélyezési. Az Azure Cosmos DB, könnyen Megvalósíthat például népszerű minták [felhasználói adatok](https://aka.ms/documentdb-xamarin-todouser) vagy többfelhasználós megosztott adatok összetett egyéni engedélyezési kód nélkül.
* A földrajzi lekérdezések. Számos ma ajánlatot földrajzi környezetfüggő lép. A kiváló támogatást nyújt az [földrajzi típusok](geospatial.md), ezek lép könnyen tooaccomplish létrehozása Azure Cosmos DB elérhetővé válnak.
* Bináris mellékleteket. Az alkalmazásadatok gyakran bináris blobot tartalmaz. Mellékletek natív támogatását teszi könnyebb toouse Azure Cosmos DB, egy lehetőségekkel az alkalmazás adatok.

## <a name="azure-cosmos-db-and-xamarin-tutorial"></a>Azure Cosmos DB és Xamarin útmutató
a következő oktatóanyag azt mutatja be hogyan hello toobuild mobilalkalmazás Xamarinnal és Azure Cosmos DB használatával. Hello oktatóanyag: hello teljes forráskód található [Xamarinnal és Azure Cosmos DB a Githubon](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/xamarin).

### <a name="get-started"></a>Bevezetés
Fontos lépések az Azure Cosmos DB könnyen tooget. Nyissa meg az Azure portál toohello, és hozzon létre egy új Azure Cosmos DB fiókot. Kattintson a hello **gyors üzembe helyezési** fülre. Letöltési hello Xamarin Forms tennivalók listája mintát, amely már csatlakoztatott tooyour Azure Cosmos DB fiók. 

![A mobilalkalmazások Azure Cosmos DB – első lépések](media/mobile-apps-with-xamarin/cosmos-db-quickstart.png)

Ha egy meglévő Xamarin-alkalmazás, hello adhat hozzá vagy [Azure Cosmos DB NuGet-csomag](documentdb-sdk-dotnet-core.md). Azure Cosmos DB Xamarin.IOS, Xamarin.Android, támogatja, és Xamarin Forms megosztott szalagtárak.

### <a name="work-with-data"></a>Adatok használata
A rekordok Azure Cosmos DB JSON-dokumentumokként séma nélküli heterogén gyűjteményekben vannak tárolva. Különböző struktúrák dokumentumok tárolhat hello ugyanahhoz a gyűjteményhez:

```cs
    var result = await client.CreateDocumentAsync(collectionLink, todoItem);
```

A Xamarin projektekben nyelvintegrált lekérdezéseket séma nélküli adatok használhatja:

```cs
    var query = await client.CreateDocumentQuery<ToDoItem>(collectionLink)
                    .Where(todoItem => todoItem.Complete == false)
                    .AsDocumentQuery();

    Items = new List<TodoItem>();
    while (query.HasMoreResults) {
        Items.AddRange(await query.ExecuteNextAsync<TodoItem>());
    }
```
### <a name="add-users"></a>Felhasználók hozzáadása
Például számos első lépések minták, a hello Azure Cosmos DB minta letöltött toohello szolgáltatás hitelesíti a főkulcs szoftveresen kötött hello app kódban számára. Az alapértelmezett érték nem célszerű toorun bárhol, kivéve a helyi emulátorának kívánt alkalmazás. Ha egy jogosulatlan felhasználó hello főkulcs kapott, minden hello adatok így vannak elrendezve a Azure Cosmos DB fiók sérülhet. Ehelyett azt szeretné, az alkalmazás tooaccess csak hello rekordok hello bejelentkezett felhasználó számára. Az Azure Cosmos DB lehetővé teszi a fejlesztők toogrant alkalmazás olvasása vagy olvasási/írási engedéllyel tooa gyűjtemény és dokumentumok a partíciókulcs szerint csoportosítva, vagy egy adott dokumentumot. 

Kövesse ezeket lépéseket toomodify hello tennivalók listája app tooa többfelhasználós Tennivaló lista alkalmazást: 

  1. Bejelentkezési tooyour alkalmazás hozzáadása a Facebook, az Active Directory vagy bármely egyéb szolgáltató használatával.

  2. Hozzon létre egy Azure Cosmos DB UserItems gyűjteményt a **/UserId** hello partíció kulcsként. Hello partíciókulcs a gyűjtemény lehetővé teszi, hogy Azure Cosmos DB tooscale végtelenül hello a felhasználók számának megadásával növekszik, miközben továbbra toooffer gyors lekérdezéseket.

  3. Adja hozzá az Azure Cosmos DB erőforrás Token Broker. Az egyszerű webes API hitelesíti a felhasználókat, és rövid élettartamú jogkivonatok toosigned felhasználók problémáinak hozzáférés csak toohello dokumentumok a partíción belül. Ebben a példában erőforrás Token Broker az App Service szolgáltatásban tárolja.

  4. Hello app tooauthenticate tooResource Token Broker Facebook módosítása, és igényelhetnek hello erőforrás jogkivonatok hello bejelentkezés Facebook-felhasználók számára. Az adatok hello UserItems gyűjtemény érheti el.  

Ebben a mintában a teljes kód mintát található [erőforrás Token Broker a Githubon](http://aka.ms/documentdb-xamarin-todouser). Ez a diagram azt ábrázolja, hello megoldást:

![Az Azure Cosmos DB felhasználókat és engedélyeket broker](media/mobile-apps-with-xamarin/documentdb-resource-token-broker.png)

Ha azt szeretné, hogy két felhasználók toohave hozzáférés toohello azonos feladatlista erőforrás Token Broker adhat hozzá további engedélyek toohello hozzáférési jogkivonat.

### <a name="scale-on-demand"></a>Az igény szerinti méretezése
Azure Cosmos-adatbázis egy olyan felügyelt adatbázis szolgáltatásként. A felhasználói bázis növekedésével nincs szükség a virtuális gépek kiépítése, vagy növekvő magok kapcsolatos tooworry. Azure Cosmos DB tootell szüksége (teljesítmény) másodpercenként hány művelet a kell. Megadhatja a hello átviteli hello keresztül **méretezési** lapon átviteli kérelem egységek (RUs) másodpercenként nevű mérték használatával. Például egy 1 KB-os dokumentum olvasási művelet szükséges 1 RU. Azt is megteheti riasztások toohello **átviteli** metrika toomonitor hello forgalom növekedés és hello átviteli, riasztást küld tűz módosításához.

![Az igény szerinti Azure Cosmos DB méretezési átviteli](media/mobile-apps-with-xamarin/cosmos-db-xamarin-scale.png)

### <a name="go-planet-scale"></a>Nyissa meg bolygónk méretezési
Az alkalmazás növekedését időben népszerűvé vált, akkor előfordulhat, hogy szerezhet felhasználók hello földgolyó méretét. Vagy például toobe előkészítve a előre nem látható eseményeket. Nyissa meg toohello Azure-portálon, és nyissa meg az Azure Cosmos DB fiókját. Az adatok folyamatos replikálása tooany több régióban az hello térkép toomake kattintson hello world között. Ez a funkció elérhetővé teszi az adatok bárhol a felhasználók is. Azt is megteheti feladatátvételi házirendek toobe esetben előkészítve.

![Az Azure Cosmos DB méretezési földrajzi régiók között](media/mobile-apps-with-xamarin/cosmos-db-xamarin-replicate.png)

Gratulálunk! Hello megoldás befejeződött, és rendelkezik a mobilalkalmazások a Xamarinnal és Azure Cosmos DB. Hasonló lépéseket toobuild Cordova-alkalmazásokkal kövesse a hello Azure Cosmos DB JavaScript SDK-t és a natív iOS vagy Android-alkalmazások Azure Cosmos DB REST API-k használatával.

## <a name="next-steps"></a>Következő lépések
* Hello forráskódja megtekintése [Xamarinnal és Azure Cosmos DB a Githubon](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/xamarin).
* Töltse le a hello [Azure Cosmos DB .NET Core SDK](documentdb-sdk-dotnet-core.md).
* A további mintakódok található [.NET-alkalmazások](documentdb-dotnet-samples.md).
* További tudnivalók [Azure Cosmos DB gazdag lekérdezési képességek](documentdb-sql-query.md).
* További tudnivalók [földrajzi támogatása az Azure Cosmos DB](geospatial.md).



