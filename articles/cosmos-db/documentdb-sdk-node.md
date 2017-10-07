---
title: "aaaAzure Cosmos DB Node.js API SDK & erőforrások |} Microsoft Docs"
description: "Tudnivalók az hello Node.js API és az SDK kiadási dátum, használatból való kivonást dátumok és módosítások hello Azure Cosmos DB Node.js SDK verziói között."
services: cosmos-db
documentationcenter: nodejs
author: rnagpal
manager: jhubbard
editor: cgronlun
ms.assetid: 9d5621fa-0e11-4619-a28b-a19d872bcf37
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/14/2017
ms.author: rnagpal
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d450b9a9ea7b0f4717ddae8940121fc458ea3744
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-nodejs-sdk-release-notes-and-resources"></a>Azure Cosmos DB Node.js SDK: A kibocsátási megjegyzések és erőforrások
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

<tr><td>**SDK letöltése**</td><td>[NPM](https://www.npmjs.com/package/documentdb)</td></tr>

<tr><td>**API-JÁNAK dokumentációja**</td><td>[NODE.js API referenciadokumentációt](http://azure.github.io/azure-documentdb-node/DocumentClient.html)</td></tr>

<tr><td>**SDK telepítési utasításokat**</td><td>[Telepítési utasításokat](http://azure.github.io/azure-documentdb-node/)</td></tr>

<tr><td>**Közreműködési lehetőségek tooSDK**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-node/tree/master/source)</td></tr>

<tr><td>**Példák**</td><td>[NODE.js-Kódminták](documentdb-nodejs-samples.md)</td></tr>

<tr><td>**Első lépéseket ismertető oktatóanyag**</td><td>[Ismerkedés a Node.js SDK hello](documentdb-nodejs-get-started.md)</td></tr>

<tr><td>**Webes alkalmazás oktatóanyag**</td><td>[Azure Cosmos DB használata Node.js-webalkalmazás létrehozása](documentdb-nodejs-application.md)</td></tr>

<tr><td>**Aktuális támogatott platform**</td><td> 
[NODE.js 6.x](https://nodejs.org/en/blog/release/v6.10.3/)<br/> 
[NODE.js v4.2.0](https://nodejs.org/en/blog/release/v4.2.0/)<br/> 
[NODE.js v0.12](https://nodejs.org/en/blog/release/v0.12.0/)<br/> 
[NODE.js v0.10](https://nodejs.org/en/blog/release/v0.10.0/) 
</td></tr>
</table></br>

## <a name="release-notes"></a>Kibocsátási megjegyzések

### <a name="1.12.2"/>1.12.2</a>
*   rögzített npm dokumentációját.

### <a name="1.12.1"/>1.12.1</a>
* Rögzített programhiba executeStoredProcedure ahol érintett dokumentumok kellett speciális Unicode-karaktereket (LS, PS).
* Unicode-karaktereket a hello partíciókulcs-dokumentumok kezelése hiba kijavítva.
* Gyűjtemények létrehozása hello neve adathordozó rögzített támogatása. Github probléma #114.
* Engedély engedélyezési jogkivonat rögzített támogatása. Github probléma #178.

### <a name="1.12.0"/>1.12.0</a>
* Támogatása egy új [konzisztenciaszint](consistency-levels.md) ConsistentPrefix nevezik.
* UriFactory támogatása.
* Egy Unicode támogatási hiba kijavítva. GitHub probléma #171.

### <a name="1.11.0"/>1.11.0</a>
* Összesítési lekérdezéseket (COUNT, MIN, MAX, SUM és átlagos) hozzáadott hello támogatása.
* A hozzáadott hello beállítás párhuzamossági a partíció lekérdezések közötti mértékű vezérlése.
* Hozzáadott hello beállítás SSL ellenőrzési letiltását Azure Cosmos DB emulatorban futtatásakor.
* A particionált gyűjtemények 10,100 RU/mp too2500 RU/mp a minimális átviteli szintűre csökkent.
* Rögzített hello folytatási token hiba egypartíciós gyűjtemény. Github probléma #107.
* Rögzített hello executeStoredProcedure hiba egyetlen param 0 foglalkoznak. Github probléma #155.

### <a name="1.10.2"/>1.10.2</a>
* Rögzített felhasználói ügynök fejléc tooinclude hello SDK-verzió.
* Kisebb kód karbantartása.

### <a name="1.10.1"/>1.10.1</a>
* SSL-ellenőrzés letiltása, hello SDK tootarget hello emulator(hostname=localhost) használatakor.
* Támogatja a tárolt eljárás végrehajtása során parancsfájl naplózásának engedélyezéséről.

### <a name="1.10.0"/>1.10.0</a>
* Támogatja a párhuzamos lekérdezések partíció közötti.
* FELSŐ/ORDER BY lekérdezések a particionált gyűjtemények támogatása.

### <a name="1.9.0"/>1.9.0</a>
* Szabályozottan halmozott kérelmek hozzáadott újrapróbálkozási házirend támogatása. (A szabályozottan halmozott kérelmek egy kérelem aránya túl nagy kivétel, hibakód 429 kapnak.) Alapértelmezés szerint Azure Cosmos DB újrapróbálja kilenc alkalommal az egyes kérelmek 429. Hibakód: a rendszer észlelt, amikor hello retryAfter idő hello válaszfejléc érvényesítenie. A rögzített újrapróbálkozási időköz most már beállítható hello RetryOptions tulajdonság részeként hello ConnectionPolicy objektum Ha azt szeretné, hogy a kiszolgáló által visszaadott hello újrapróbálkozások között tooignore hello retryAfter idő. Azure Cosmos-adatbázis most megvárja, legfeljebb 30 másodpercig minden egyes kérelemhez (függetlenül újrapróbálkozások száma) leszabályozta, és hibakód 429 hello választ ad vissza. Most is a hello RetryOptions tulajdonság azon objektumhoz, ConnectionPolicy felülbírálható.
* Cosmos DB most adja vissza x-ms-szabályozási--újrapróbálkozások és x-ms-throttle-retry-wait-time-ms, hello válaszfejlécek minden kérelem toodenote hello késleltetési száma és hello összesített ideje hello kérelem hello újrapróbálkozások között várt próbálkozzon újra.
* hello RetryOptions osztály adott, az ilyen hello RetryOptions tulajdonság hello ConnectionPolicy osztály, amely használt toooverride néhány hello alapértelmezett újrapróbálkozási beállítások.

### <a name="1.8.0"/>1.8.0</a>
* Több területi adatbázis fiókok hozzáadott hello támogatása.

### <a name="1.7.0"/>1.7.0</a>
* Hozzáadott hello idő tooLive(TTL) szolgáltatás dokumentumok támogatása.

### <a name="1.6.0"/>1.6.0</a>
* Megvalósított [particionált gyűjtemények](partition-data.md) és [felhasználói teljesítményszintet](performance-levels.md).

### <a name="1.5.6"/>1.5.6</a>
* Rögzített RangePartitionResolver.resolveForRead hiba, ha azt lett nem ad vissza hivatkozások miatt hibás concat tooa eredmények.

### <a name="1.5.5"/>1.5.5</a>
* Rögzített hashParitionResolver resolveForRead(): Ha nincs megadva partíciókulcs kivétel, helyett sorolja fel az összes regisztrált hivatkozások kiváltása volt.

### <a name="1.5.4"/>1.5.4</a>
* Kijavítja a hibát [#100](https://github.com/Azure/azure-documentdb-node/issues/100) -HTTPS-ügynök dedikált: elkerülése hello globális ügynök Azure Cosmos DB célokra módosítása. Használja a dedikált ügynököt az összes hello lib kérelmek.

### <a name="1.5.3"/>1.5.3</a>
* Kijavítja a hibát [#81](https://github.com/Azure/azure-documentdb-node/issues/81) - megfelelően kötőjelek az adathordozó-azonosítók kezelésére.

### <a name="1.5.2"/>1.5.2</a>
* Kijavítja a hibát [#95](https://github.com/Azure/azure-documentdb-node/issues/95) -EventEmitter figyelő szivárgás lépett fel, figyelmeztetés.

### <a name="1.5.1"/>1.5.1</a>
* Kijavítja a hibát [#92](https://github.com/Azure/azure-documentdb-node/issues/90) -nevezze át a mappát kivonatoló toohash rendszerek kis-és nagybetűket.

### <a name="1.5.0"/>1.5.0</a>
* Horizontális segítséget nyújt kivonatoló & tartomány partíció feloldókat hozzáadásával.

### <a name="1.4.0"/>1.4.0</a>
* Upsert megvalósításához. Új upsertXXX metódusai documentClient.

### <a name="1.3.0"/>1.3.0</a>
* Kihagyott toobring verziószámok Csomagjától az igazítását.

### <a name="1.2.2"/>1.2.2</a>
* Vegyes Q kihasználásának köszönhetően akár még a burkoló toonew tárházba.
* Frissítse az npm beállításjegyzék toopackage fájlt.

### <a name="1.2.1"/>1.2.1</a>
* Megvalósítja-alapú útválasztási azonosítója.
* Kijavítja a hibát [#49](https://github.com/Azure/azure-documentdb-node/issues/49) -aktuális tulajdonság metódus current() ütközik.

### <a name="1.2.0"/>1.2.0</a>
* A földrajzi index támogatása.
* Ellenőrzi az összes erőforrás id tulajdonság. Erőforrások azonosító nem tartalmazhat?, /, #, & #47; & #47; karaktereket vagy záró szóközt.
* Hozzáadja az új fejléc "index átalakítása folyamatban" tooResourceResponse.

### <a name="1.1.0"/>1.1.0</a>
* V2 indexelési házirendet alkalmazza.

### <a name="1.0.3"/>1.0.3</a>
* A probléma [#40](https://github.com/Azure/azure-documentdb-node/issues/40) - eslint megvalósítása és grunt hello core-konfigurációja és SDK készletéről.

### <a name="1.0.2"/>1.0.2</a>
* A probléma [#45](https://github.com/Azure/azure-documentdb-node/issues/45) -tett burkoló nem tartalmazza a hiba: a fejlécben.

### <a name="1.0.1"/>1.0.1</a>
* Az ütközések kezelésére képes tooquery megvalósított readConflicts, readConflictAsync és queryConflicts hozzáadásával.
* Frissített API dokumentációját.
* A probléma [#41](https://github.com/Azure/azure-documentdb-node/issues/41) -client.createDocumentAsync hiba.

### <a name="1.0.0"/>1.0.0</a>
* GA SDK.

## <a name="release--retirement-dates"></a>Kiadás & használatból való kivonást dátumok
A Microsoft biztosít értesítési legalább **12 hónapon keresztül** előre kivonása az SDK-t rendelés toosmooth hello átmenet tooa vagy újabb támogatott verzióra.

Új szolgáltatásait és funkcióit és optimalizálás csak hozzáadott toohello aktuális SDK-t, így javasolt, hogy Ön mindig frissítési toohello SDK legújabb lehető leghamarabb.

TooCosmos DB kivont SDK használatával van kérelmet vissza kell utasítani, hello szolgáltatás.

<br/>

| Verzió | Kiadás dátuma | Kivezetési dátum |
| --- | --- | --- |
| [1.12.2](#1.12.2) |2017. augusztus 10. |--- |
| [1.12.1](#1.12.1) |2017. augusztus 10. |--- |
| [1.12.0](#1.12.0) |2017. május 10. |--- |
| [1.11.0](#1.11.0) |2017. március 16. |--- |
| [1.10.2](#1.10.2) |2017. január 27. |--- |
| [1.10.1](#1.10.1) |2016. december 22. |--- |
| [1.10.0](#1.10.0) |2016. október 03. |--- |
| [1.9.0](#1.9.0) |2016. július 07. |--- |
| [1.8.0](#1.8.0) |2016. június 14. |--- |
| [1.7.0](#1.7.0) |2016. április 26. |--- |
| [1.6.0](#1.6.0) |2016. március 29. |--- |
| [1.5.6](#1.5.6) |2016. március 08. |--- |
| [1.5.5](#1.5.5) |2016. február 02. |--- |
| [1.5.4](#1.5.4) |2016. február 01. |--- |
| [1.5.2](#1.5.2) |2016. január 26. |--- |
| [1.5.2](#1.5.2) |2016. január 22. |--- |
| [1.5.1](#1.5.1) |2016. január 4. |--- |
| [1.5.0](#1.5.0) |2015. december 31. |--- |
| [1.4.0](#1.4.0) |2015. október 06. |--- |
| [1.3.0](#1.3.0) |2015. október 06. |--- |
| [1.2.2](#1.2.2) |2015. szeptember 10. |--- |
| [1.2.1](#1.2.1) |2015. augusztus 15. |--- |
| [1.2.0](#1.2.0) |2015. augusztus 05. |--- |
| [1.1.0](#1.1.0) |2015. július 09. |--- |
| [1.0.3](#1.0.3) |2015. június 04. |--- |
| [1.0.2](#1.0.2) |2015. május 23. |--- |
| [1.0.1](#1.0.1) |2015. május 15. |--- |
| [1.0.0](#1.0.0) |2015. áprilisi 08 |--- |

## <a name="faq"></a>GYIK
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Lásd még:
toolearn Cosmos DB kapcsolatos további információkért lásd: [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) szolgáltatás lapján.

