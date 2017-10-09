---
title: "Az Azure Cosmos DB: A DocumentDB Java API, SDK & erőforrások |} Microsoft Docs"
description: "Tudnivalók az hello Java API és az SDK kiadási dátum, használatból való kivonást dátumok és módosítások hello Azure Cosmos DB DocumentDB Java SDK verziói között."
services: cosmos-db
documentationcenter: java
author: rnagpal
manager: jhubbard
editor: cgronlun
ms.assetid: 7861cadf-2a05-471a-9925-0fec0599351b
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 07/11/2017
ms.author: khdang
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8ef43ebeb7ae1bfc55512c4a7489c1b7930122d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-documentdb-java-sdk-release-notes-and-resources"></a>Azure Cosmos DB: A DocumentDB Java SDK kibocsátási megjegyzések és erőforrások
> [!div class="op_single_selector"]
> * [.NET](documentdb-sdk-dotnet.md)
> * [.NET-módosítás adatcsatorna](documentdb-sdk-dotnet-changefeed.md)
> * [.NET Core](documentdb-sdk-dotnet-core.md)
> * [Node.js](documentdb-sdk-node.md)
> * [Java](documentdb-sdk-java.md)
> * [Python](documentdb-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/documentdb/)
> * [REST erőforrás-szolgáltató](https://docs.microsoft.com/rest/api/documentdbresourceprovider/)
> * [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> 
> 

<table>

<tr><td>**SDK letöltése**</td><td>[Maven 3](http://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22com.microsoft.azure%22%20AND%20a%3A%22azure-documentdb%22)</td></tr>

<tr><td>**API-JÁNAK dokumentációja**</td><td>[Java API-referenciadokumentáció](/java/api/com.microsoft.azure.documentdb)</td></tr>

<tr><td>**Közreműködési lehetőségek tooSDK**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-java/)</td></tr>

<tr><td>**Első lépések**</td><td>[Ismerkedés a hello Java SDK](documentdb-java-get-started.md)</td></tr>

<tr><td>**Webes alkalmazás oktatóanyag**</td><td>[Webalkalmazás fejlesztése a Azure Cosmos DB](documentdb-java-application.md)</td></tr>

<tr><td>**Aktuális támogatott futásidejű**</td><td>[JDK 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)</td></tr>
</table></br>

## <a name="release-notes"></a>Kibocsátási megjegyzések

### <a name="a-name11201120"></a><a name="1.12.0"/>1.12.0
* Felosztja a kritikus hibajavítások toorequest során partíció feldolgozása.
* Megtörtént egy probléma javítása hello erős és BoundedStaleness konzisztenciaszintek.

### <a name="a-name11101110"></a><a name="1.11.0"/>1.11.0
* Egy új konzisztenciaszint támogatása ConsistentPrefix nevezik.
* Rögzített munkamenetmód gyűjtemény olvasási hiba.

### <a name="a-name11001100"></a><a name="1.10.0"/>1.10.0
* Particionált gyűjtemény, mint az engedélyezett támogatása 2500 RU/mp, alacsony, a méretezés lépésekben 100 RU/mp.
* A hiba kijavítva a hello natív szerelvény, ami azt eredményezheti NullRef kivétel néhány lekérdezést.

### <a name="a-name196196"></a><a name="1.9.6"/>1.9.6
* Programhiba rögzített hello lekérdezési motor konfigurációját, amelyek kivételek lekérdezések okozhatnak átjáró módban.
* Rögzített néhány hibák hello munkamenet tárolóban, amelyek közvetlenül a webhelycsoport létrehozása után a kéréseket "Tulajdonosa erőforrás nem található" kivétel okozhatnak.

### <a name="a-name195195"></a><a name="1.9.5"/>1.9.5
* Összesítési lekérdezéseket (COUNT, MIN, MAX, SUM és átlagos) támogatása. Lásd: [összesítési támogatási](documentdb-sql-query.md#Aggregates).
* Adatcsatorna módosítás támogatása.
* Gyűjtemény kvótaadatok keresztül RequestOptions.setPopulateQuotaInfo támogatása.
* Tárolt eljárás parancsfájl naplózást RequestOptions.setScriptLoggingEnabled támogatása.
* Rögzített programhiba, ahol DirectHttps módban lekérdezés lefagyhat, amikor adatátviteli hiba észlelése.
* Programhiba rögzített munkamenet-konzisztencia módban.
* Egy hiba miatt előfordulhat, hogy NullReferenceException a HttpContext Ha nagy a kérések aránya rögzített.
* Javítja a teljesítményt a DirectHttps mód.

### <a name="a-name194194"></a><a name="1.9.4"/>1.9.4
* A hozzáadott egyszerű példány-alapú proxyk ügyféltámogatás ConnectionPolicy.setProxy() API-t.
* A hozzáadott DocumentClient.close() API tooproperly leállítási DocumentClient-példány.
* Továbbfejlesztett lekérdezési teljesítmény szerint hello lekérdezésterv származó hello natív szerelvény helyett átjáró hello közvetlen kapcsolatot módban.
* Állítsa be a FAIL_ON_UNKNOWN_PROPERTIES = false, így a felhasználók a saját pojo-vá JsonIgnoreProperties toodefine nem szükséges.
* Átkerült naplózási toouse SLF4J.
* Rögzített, néhány más hibák konzisztencia-olvasó.

### <a name="a-name193193"></a><a name="1.9.3"/>1.9.3
* Programhiba rögzített hello kapcsolat felügyeleti tooprevent kapcsolat kiszivárgásának közvetlen kapcsolatot módban.
* Rögzített programhiba hello legfelső szintű lekérdezésben, ahol NullReferenece kivétel generálhat.
* Javítja a teljesítményt, csökkenti a hálózati hívás hello belső gyorsítótárak esetében hello száma.
* A hozzáadott állapotkód, tevékenységazonosító és a kérelem URI-azonosítója a DocumentClientException hibaelhárítást.

### <a name="a-name192192"></a><a name="1.9.2"/>1.9.2
* Megtörtént egy probléma javítása hello kapcsolat felügyeleti stabilitását.

### <a name="a-name191191"></a><a name="1.9.1"/>1.9.1
* BoundedStaleness konzisztenciaszint támogatása.
* Közvetlen kapcsolat CRUD műveleteihez particionált gyűjtemények támogatása.
* SQL adatbázis lekérdezése hiba kijavítva.
* Programhiba rögzített hello gyorsítótárában, ahol munkamenet-azonosító is lehet helytelenül van beállítva.

### <a name="a-name190190"></a><a name="1.9.0"/>1.9.0
* Támogatja a párhuzamos lekérdezések partíció közötti.
* FELSŐ/ORDER BY lekérdezések a particionált gyűjtemények támogatása.
* Az erős konzisztencia támogatása.
* Alapú kérések közvetlen kapcsolat használatakor támogatása.
* Rögzített toomake ActivityId maradnak konzisztens összes kérelem újrapróbálkozások között.
* Programhiba rögzített során egy gyűjteménybe hello és újbóli létrehozása a munkamenet-gyorsítótár toohello kapcsolatos ugyanazzal a névvel.
* A hozzáadott sokszög és LineString adattípusok indexelő házirend geokerítések térbeli lekérdezéseket a gyűjtemény meg.
* Java Doc Java 1.8 rögzített problémákat.

### <a name="a-name181181"></a><a name="1.8.1"/>1.8.1
* Programhiba rögzített PartitionKeyDefinitionMap toocache az egypartíciós gyűjtemények és extra nem később fetch partíció kulcs kérelmeket.
* Rögzített egy hiba toonot újra, amikor egy helytelen partíciókulcs-értékkel van megadva.

### <a name="a-name180180"></a><a name="1.8.0"/>1.8.0
* Több területi adatbázis fiókok hozzáadott hello támogatása.
* A beállítások toocustomize hello maximális szabályozottan halmozott kérelmek automatikus újrapróbálkozási támogatása ismétlési kísérletek száma és max várakozási idő.  Lásd: RetryOptions és ConnectionPolicy.getRetryOptions().
* Elavult IPartitionResolver egyéni particionálási kód alapján. A particionált gyűjtemények használata magasabb tárolási és átviteli sebességet.

### <a name="a-name171171"></a><a name="1.7.1"/>1.7.1
* A hozzáadott újrapróbálkozási házirend támogatása sávszélesség-szabályozás.  

### <a name="a-name170170"></a><a name="1.7.0"/>1.7.0
* Időigényessé toolive (TTL) dokumentumok támogatása.

### <a name="a-name160160"></a><a name="1.6.0"/>1.6.0
* Megvalósított [particionált gyűjtemények](partition-data.md) és [felhasználói teljesítményszintet](performance-levels.md).

### <a name="a-name151151"></a><a name="1.5.1"/>1.5.1
* Programhiba rögzített HashPartitionResolver toogenerate kivonatértékeket más SDK konzisztens little endian toobe.

### <a name="a-name150150"></a><a name="1.5.0"/>1.5.0
* Kivonatoló & tartomány hozzáadása partíció feloldókat tooassist a horizontális az alkalmazások több partíciót.

### <a name="a-name140140"></a><a name="1.4.0"/>1.4.0
* Upsert megvalósításához. Új upsertXXX módszerek toosupport Upsert szolgáltatás hozzá.
* Azonosító-alapú útválasztás megvalósítása. Nincs nyilvános API-módosítás, belső összes módosítását.

### <a name="a-name130130"></a><a name="1.3.0"/>1.3.0
* Kiadás kihagyva Csomagjától összehangolás toobring verziószám

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0
* Támogatja a földrajzi Index
* Ellenőrzi az összes erőforrás id tulajdonság. Erőforrások azonosító nem tartalmazhat?, /, #, \, karaktereket vagy záró szóközt.
* Hozzáadja az új fejléc "index átalakítása folyamatban" tooResourceResponse.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0
* V2 indexelési házirendet alkalmazza

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* GA SDK

## <a name="release--retirement-dates"></a>Kiadás & használatból való kivonást dátumok
Microsoft legalább értesítést küldenek **12 hónapon keresztül** előre kivonása az SDK-t rendelés toosmooth hello átmenet tooa vagy újabb támogatott verzióra.

Új szolgáltatásait és funkcióit és optimalizálás csak hozzáadott toohello aktuális SDK-t, így célszerű a lehető leghamarabb javasoljuk, hogy Ön mindig frissítési toohello SDK letöltéséhez.

A kérelem tooCosmos DB kivont SDK használatával a program elutasítja hello szolgáltatás.

> [!WARNING]
> Hello DocumentDB SDK-t a Java előzetes tooversion összes verziója **1.0.0** a rendszerből **2016. február 29-én**.
> 
> 

<br/>

| Verzió | Kiadás dátuma | Kivezetési dátum |
| --- | --- | --- |
| [1.12.0](#1.12.0) |2017. július 11. |--- |
| [1.11.0](#1.11.0) |2017. május 10. |--- |
| [1.10.0](#1.10.0) |2017. március 11. |--- |
| [1.9.6](#1.9.6) |2017. február 21. |--- |
| [1.9.5](#1.9.5) |2017. január 31-ig. |--- |
| [1.9.4](#1.9.4) |2016. november 24. |--- |
| [1.9.3](#1.9.3) |2016. október 30. |--- |
| [1.9.2](#1.9.2) |2016. október 28. |--- |
| [1.9.1](#1.9.1) |2016. október 26. |--- |
| [1.9.0](#1.9.0) |2016. október 03. |--- |
| [1.8.1](#1.8.1) |2016. június 30. |--- |
| [1.8.0](#1.8.0) |2016. június 14. |--- |
| [1.7.1](#1.7.1) |2016. április 30. |--- |
| [1.7.0](#1.7.0) |2016. április 27. |--- |
| [1.6.0](#1.6.0) |2016. március 29. |--- |
| [1.5.1](#1.5.1) |2015. december 31. |--- |
| [1.5.0](#1.5.0) |2015. december 04. |--- |
| [1.4.0](#1.4.0) |2015. október 05. |--- |
| [1.3.0](#1.3.0) |2015. október 05. |--- |
| [1.2.0](#1.2.0) |2015. augusztus 05. |--- |
| [1.1.0](#1.1.0) |2015. július 09. |--- |
| [1.0.1](#1.0.1) |2015. május 12. |--- |
| [1.0.0](#1.0.0) |2015. április 07. |--- |
| 0.9.5-prelease |2015. gyel 09. |2016. február 29. |
| 0.9.4-prelease |2015. február 17. |2016. február 29. |
| 0.9.3-prelease |2015. január 13. |2016. február 29. |
| 0.9.2-prelease |2014. decemberi 19 |2016. február 29. |
| 0.9.1-prelease |2014. decemberi 19 |2016. február 29. |
| 0.9.0-prelease |2014. december 10. |2016. február 29. |

## <a name="faq"></a>GYIK
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Lásd még:
toolearn Cosmos DB kapcsolatos további információkért lásd: [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) szolgáltatás lapján.

