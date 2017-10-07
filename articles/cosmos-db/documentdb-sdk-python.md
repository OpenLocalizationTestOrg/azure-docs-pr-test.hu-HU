---
title: "aaaAzure Cosmos DB Python API SDK & erőforrások |} Microsoft Docs"
description: "Tudnivalók az hello Python API és az SDK kiadási dátum, használatból való kivonást dátumok és módosítások hello Azure Cosmos DB Python SDK verziói között."
services: cosmos-db
documentationcenter: python
author: rnagpal
manager: jhubbard
editor: cgronlun
ms.assetid: 3ac344a9-b2fa-4a3f-a4cc-02d287e05469
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 05/24/2017
ms.author: rnagpal
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1a164b72d2bd819de87df0229357b82e2177af2f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-python-sdk-release-notes-and-resources"></a>Az Azure Cosmos DB Python SDK: A kibocsátási megjegyzések és erőforrások
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

<tr><td>**SDK letöltése**</td><td>[PyPI](https://pypi.python.org/pypi/pydocumentdb)</td></tr>

<tr><td>**API-JÁNAK dokumentációja**</td><td>[Python API referenciadokumentációt tartalmaz](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.html)</td></tr>

<tr><td>**SDK telepítési utasításokat**</td><td>[Python SDK telepítési utasításokat](http://azure.github.io/azure-documentdb-python/)</td></tr>

<tr><td>**Közreműködési lehetőségek tooSDK**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-python)</td></tr>

<tr><td>**Első lépések**</td><td>[Ismerkedés a hello Python SDK](documentdb-python-application.md)</td></tr>

<tr><td>**Aktuális támogatott platform**</td><td>[Python 2.7](https://www.python.org/downloads/) és [Python 3.5](https://www.python.org/downloads/)</td></tr>
</table></br>

## <a name="release-notes"></a>Kibocsátási megjegyzések
### <a name="a-name220220"></a><a name="2.2.0"/>2.2.0
* Egy új konzisztenciaszint támogatása ConsistentPrefix nevezik.


### <a name="a-name210210"></a><a name="2.1.0"/>2.1.0
* Összesítési lekérdezéseket (COUNT, MIN, MAX, SUM és átlagos) támogatása.
* A beállítás a letiltás SSL ellenőrzési Cosmos DB emulatorban futtatásakor hozzá.
* Függő kérések modul toobe pontosan 2.10.0 hello korlátozása eltávolítva.
* A particionált gyűjtemények 10,100 RU/mp too2500 RU/mp a minimális átviteli szintűre csökkent.
* Támogatja a tárolt eljárás végrehajtása során parancsfájl naplózásának engedélyezéséről.
* REST API-verzió bumped túl "2017-01-19' ebben a kiadásban.

### <a name="a-name201201"></a><a name="2.0.1"/>2.0.1
* A Szerkesztői módosítások végrehajtott toodocumentation megjegyzések.

### <a name="a-name200200"></a><a name="2.0.0"/>2.0.0
* Python 3.5 támogatása.
* Támogatja a kapcsolatkészletezést kérelmek modul használatával.
* A munkamenet-konzisztencia támogatása.
* FELSŐ/ORDERBY lekérdezések particionált gyűjtemények támogatása.

### <a name="a-name190190"></a><a name="1.9.0"/>1.9.0
* Szabályozottan halmozott kérelmek hozzáadott újrapróbálkozási házirend támogatása. (A szabályozottan halmozott kérelmek egy kérelem aránya túl nagy kivétel, hibakód 429 kapnak.) Alapértelmezés szerint Azure Cosmos DB újrapróbálja kilenc alkalommal az egyes kérelmek 429. Hibakód: a rendszer észlelt, amikor hello retryAfter idő hello válaszfejléc érvényesítenie. A rögzített újrapróbálkozási időköz most már beállítható hello RetryOptions tulajdonság részeként hello ConnectionPolicy objektum Ha azt szeretné, hogy a kiszolgáló által visszaadott hello újrapróbálkozások között tooignore hello retryAfter idő. Azure Cosmos-adatbázis most megvárja, legfeljebb 30 másodpercig minden egyes kérelemhez (függetlenül újrapróbálkozások száma) leszabályozta, és hibakód 429 hello választ ad vissza. Most is lehet a hello RetryOptions tulajdonság ConnectionPolicy objektumon.
* Cosmos DB most adja vissza x-ms-szabályozási--újrapróbálkozások és x-ms-throttle-retry-wait-time-ms, minden kérelem toodenote hello késleltetési hello válaszfejlécek újrapróbálkozás száma és hello cummulative időpontja hello kérelem hello újrapróbálkozások között várt.
* Eltávolított hello RetryPolicy osztály és hello megfelelő tulajdonságával (retry_policy) kikerültek a hello document_client osztály, és ehelyett bevezetett ConnectionPolicy osztály, amely használt toooverride hello RetryOptions tulajdonsága kitettségének RetryOptions osztály Néhány hello alapértelmezett ismételje meg a beállításokat.

### <a name="a-name180180"></a><a name="1.8.0"/>1.8.0
* Több területi adatbázis fiókok hozzáadott hello támogatása.

### <a name="a-name170170"></a><a name="1.7.0"/>1.7.0
* Hozzáadott hello idő tooLive(TTL) szolgáltatás dokumentumok támogatása.

### <a name="a-name161161"></a><a name="1.6.1"/>1.6.1
* Hibajavítások kapcsolódó tooserver ügyféloldali particionálás partitionkey elérési út tooallow különleges karaktereket.

### <a name="a-name160160"></a><a name="1.6.0"/>1.6.0
* Megvalósított [particionált gyűjtemények](partition-data.md) és [felhasználói teljesítményszintet](performance-levels.md). 

### <a name="a-name150150"></a><a name="1.5.0"/>1.5.0
* Kivonatoló & tartomány hozzáadása partíció feloldókat tooassist a horizontális az alkalmazások több partíciót.

### <a name="a-name142142"></a><a name="1.4.2"/>1.4.2
* Upsert megvalósításához. Új UpsertXXX módszerek toosupport Upsert szolgáltatás hozzá.
* Azonosító-alapú útválasztás megvalósítása. Nincs nyilvános API-módosítás, belső összes módosítását.

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0
* Támogatja a földrajzi index.
* Ellenőrzi az összes erőforrás id tulajdonság. Erőforrások azonosító nem tartalmazhat?, /, #, \, karaktereket vagy záró szóközt.
* Hozzáadja az új fejléc "index átalakítása folyamatban" tooResourceResponse.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0
* V2 indexelési házirendet alkalmazza.

### <a name="a-name101101"></a><a name="1.0.1"/>1.0.1
* Proxy kapcsolatot támogat.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* GA SDK.

## <a name="release--retirement-dates"></a>Kiadás & használatból való kivonást dátumok
Microsoft legalább értesítést küldenek **12 hónapon keresztül** előre kivonása az SDK-t rendelés toosmooth hello átmenet tooa vagy újabb támogatott verzióra.

Új szolgáltatásait és funkcióit és optimalizálás csak hozzáadott toohello aktuális SDK-t, így célszerű a lehető leghamarabb javasoljuk, hogy Ön mindig frissítési toohello SDK letöltéséhez. 

A kérelem tooCosmos DB kivont SDK használatával a program elutasítja hello szolgáltatás.

> [!WARNING]
> Hello Azure DocumentDB SDK-t a Python előzetes tooversion összes verziója **1.0.0** a rendszerből **2016. február 29-én**. 
> 
> 

<br/>

| Verzió | Kiadás dátuma | Kivezetési dátum |
| --- | --- | --- |
| [2.2.0](#2.2.0) |2017. május 10. |--- |
| [2.1.0](#2.1.0) |2017. Előfordulhat, hogy 01. |--- |
| [2.0.1](#2.0.1) |2016. október 30. |--- |
| [2.0.0](#2.0.0) |2016. Szeptembertől 29. |--- |
| [1.9.0](#1.9.0) |2016. július 07. |--- |
| [1.8.0](#1.8.0) |2016. június 14. |--- |
| [1.7.0](#1.7.0) |2016. április 26. |--- |
| [1.6.1](#1.6.1) |2016. április 08. |--- |
| [1.6.0](#1.6.0) |2016. március 29. |--- |
| [1.5.0](#1.5.0) |2016. január 03. |--- |
| [1.4.2](#1.4.2) |2015. október 06. |--- |
| [1.4.1](#1.4.1) |2015. október 06. |--- |
| [1.2.0](#1.2.0) |2015. augusztus 06. |--- |
| [1.1.0](#1.1.0) |2015. július 09. |--- |
| [1.0.1](#1.0.1) |2015. május 25. |--- |
| [1.0.0](#1.0.0) |2015. április 07. |--- |
| 0.9.4-prelease |2015. január 14. |2016. február 29. |
| 0.9.3-prelease |2014. decemberi 09 |2016. február 29. |
| 0.9.2-prelease |2014. november 25. |2016. február 29. |
| 0.9.1-prelease |2014. szeptember 23. |2016. február 29. |
| 0.9.0-prelease |2014. augusztus 21. |2016. február 29. |

## <a name="faq"></a>GYIK
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Lásd még:
toolearn Cosmos DB kapcsolatos további információkért lásd: [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) szolgáltatás lapján. 

