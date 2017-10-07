---
title: "Adatkapcsolat: adatfolyam-bemenet egy esemény adatfolyamból |} Microsoft Docs"
description: "További tudnivalók a adatok kapcsolat tooStream \"bemenetek\" nevű Analytics beállítása. Bemenetek adatok adatfolyamot az események, és is adatokra hivatkoztak."
keywords: "az adatfolyam, adatkapcsolat, eseményfelhasználó"
services: stream-analytics
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 8155823c-9dd8-4a6b-8393-34452d299b68
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 07/05/2017
ms.author: samacha
ms.openlocfilehash: be2008f159061c5c9be9d0314c27fa67193e3269
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="data-connection-learn-about-data-stream-inputs-from-events-toostream-analytics"></a>Adatkapcsolat: adatfolyam-bemenet származó események tooStream Analytics adatok megismerése
hello adatok kapcsolat tooa Stream Analytics-feladat olyan adatfolyamot kell megadni az adatforrás, amely hivatkozott tooas hello feladat események *bemeneti*. A Stream Analytics rendelkezik első osztályú integráció az Azure data stream móddal, többek között a [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/), [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/), és [Azure Blob Storage tárolóban](https://azure.microsoft.com/services/storage/blobs/). A bemeneti forrásokból származhatnak hello azonos Azure-előfizetés az analytics-feladat, vagy egy másik előfizetésből.

## <a name="data-input-types-data-stream-and-reference-data"></a>Adatok bemeneti típussal: Data stream és hivatkozási adatok
Adatok fejlesztőre tooa adatforrás, mivel hello Stream Analytics-feladat által használt, és valós időben feldolgozni. Bemeneti adatok meg vannak osztva kétféle: adatok adatfolyam-bemenet, és az adatok bemenetek hivatkozik.

### <a name="data-stream-inputs"></a>Adatfolyam-bemenet
Adatfolyam események unbounded sorozatát adott idő alatt. Stream Analytics-feladatok tartalmaznia kell legalább egy adatfolyam-bemenetre. Az Event Hubs, IoT Hub és a Blob storage adatforrások adatfolyam bemeneti is támogatottak. Az Event hubs használt toocollect eseményfolyamokat több eszközök és szolgáltatások. Ezekbe az adatfolyamokba közé tartozik a közösségi média tevékenység hírcsatornák, a készlet kereskedelmi információ vagy az érzékelők. IoT-központok optimalizált toocollect adatait az eszközök internetes hálózatát (IoT) forgatókönyvekben csatlakoztatott eszközön.  A BLOB storage tömeges eseményközpontokból adatfolyamként, például a naplófájlok bemeneti forrása is használható.  

### <a name="reference-data"></a>Referenciaadatok
A Stream Analytics is támogatja a bemeneti néven *referenciaadatok*. Ez az kiegészítő adatokat, amelyek statikus vagy lassan módosítja. A korrelációs és keresések végrehajtásához általában szolgál. Például előfordulhat, hogy csatlakozik a adatok a hello adatok adatfolyam bemeneti toodata a hello referenciaadatok, mint egy SQL-csatlakozási toolook statikus értékeket kell elvégeznie. Az Azure Blob storage jelenleg csak a támogatott hello bemeneti forrás a referenciaadatoknál. Hivatkozási adatforrás a BLOB-adatobjektumok korlátozódnak too100 MB-nál.

Hogyan toocreate hivatkozás adatok bemenet: toolearn [használata referenciaadatok](stream-analytics-use-reference-data.md).  

## <a name="create-data-stream-input-from-event-hubs"></a>Az Event Hubs adatfolyam-bemenetre létrehozása

Az Azure Event Hubs biztosít magas szinten méretezhető esemény ingestors közzétételi-feliratkozási. Az eseményközpontok több millió esemény / másodperc, képes összegyűjteni, így egyszerűen feldolgozhatja és elemezheti az adatokat a csatlakoztatott eszközök és alkalmazások által létrehozott nagy mennyiségű hello. Az Event Hubs és a Stream Analytics együttesen biztosítja egy végpont megoldás az valós idejű elemzési – az Event Hubs lehetővé teszik, hogy az adatcsatorna-eseményeket az Azure valós időben, és a Stream Analytics-feladatok képes a valós idejű ezeket az eseményeket. Például webes kattint, érzékelő szivattyútelepek érzékelőinek adatai vagy online naplózási események tooEvent hubok is elküldheti. A Stream Analytics-feladatok toouse Event Hubs is létrehozhat, hello bemeneti adatfolyamok valós idejű szűrés, összesítése és megfelelési.

a Stream Analytics Eseményközpontokból származó események összefogásával hello alapértelmezett időbélyegzője, amely esemény hello hello időbélyeg érkező hello eseményközpont, amely `EventEnqueuedUtcTime`. tooprocess adatok hello időbélyeg használatával hello eseménytartalom adatfolyamként, hello kell használnia [TIMESTAMP BY](https://msdn.microsoft.com/library/azure/dn834998.aspx) kulcsszó.

### <a name="consumer-groups"></a>Felhasználói csoportok
Úgy kell konfigurálni minden Stream Analytics event hub bemeneti toohave a saját felhasználói csoportban. Ha egy feladat tartalmaz önillesztés, vagy ha több adatbevitele, néhány bemeneti után a több olvasót olvashatják. Ez a helyzet hatással van az olvasók egyetlen felhasználói csoportokban hello számát. tooavoid meghaladó hello Event Hubs legfeljebb öt olvasók / partíciónként fogyasztói csoportot, akkor a bevált gyakorlat toodesignate egy fogyasztói csoportot minden egyes Stream Analytics-feladat. Maximális száma az eseményközpont / 20 felhasználói csoportot is van. További információkért lásd: [Event Hubs programozási útmutató](../event-hubs/event-hubs-programming-guide.md).

### <a name="configure-an-event-hub-as-a-data-stream-input"></a>Az eseményközpontok beállítása a bemeneti adatfolyam
hello következő táblázat ismerteti az egyes tulajdonságai hello **új bemeneti** panel az Azure portált eseményközpont bemenetként konfigurálásakor hello.

| Tulajdonság | Leírás |
| --- | --- |
| **A bemeneti alias** |Egy rövid nevet használt hello feladat lekérdezés tooreference ez adjon meg. |
| **Service bus-névtér** |Az Azure Service Bus-névtér, amely az üzenetküldési entitások készletének tárolója. Egy új eseményközpont létrehozásakor egy Szolgáltatásbusz-névtér is létrehozhat. |
| **Eseményközpont neve** |hello event hub toouse bemenetként hello neve. |
| **Eseményközpont házirend neve** |hello toohello eseményközpont hozzáférést biztosító megosztott elérési házirendet. Minden megosztott elérési házirend rendelkezik egy nevet, hogy Ön meghatározott engedélyekkel és hozzáférési kulcsokkal. |
| **Event hub fogyasztói csoportot** (nem kötelező) |hello fogyasztói csoportot toouse tooingest adatait hello eseményközpontot. Ha nincs fogyasztói csoportot ad meg, a hello Stream Analytics-feladat használ hello alapértelmezett felhasználói csoport. Azt javasoljuk, hogy használja-e egy külön felhasználói csoport minden egyes Stream Analytics-feladathoz. |
| **Esemény szerializálási formátum** |hello szerializálási formátum (JSON, CSV vagy Avro) az hello bejövő adatfolyam. |
| **Kódolás** | Az UTF-8 jelenleg hello csak támogatott kódolási formátum. |

Ha az adatok az eseményközpontban lévő, a Stream Analytics-lekérdezés metaadatmezőket a következő hozzáférési toohello közül választhat:

| Tulajdonság | Leírás |
| --- | --- |
| **EventProcessedUtcTime** |hello dátum és idő, hogy hello esemény Stream Analytics lett feldolgozva. |
| **EventEnqueuedUtcTime** |hello dátum és idő, hogy hello esemény érkezett az Event Hubs. |
| **PartitionId** |hello nulla alapú Partícióazonosító hello bemeneti adapterhez. |

Például használja ezeket a mezőket, írhat egy lekérdezést, például a következő példa hello:

````
SELECT
    EventProcessedUtcTime,
    EventEnqueuedUtcTime,
    PartitionId
FROM Input
````

## <a name="create-data-stream-input-from-iot-hub"></a>Adatfolyam-bemenetre IoT Hub létrehozása
Az Azure Iot Hub egy kiválóan méretezhető közzététele előfizetés IoT-forgatókönyvek esetén az optimalizált eseménybetöltőnek.

az IoT-központ a Stream Analytics származó események összefogásával hello alapértelmezett időbélyegzője, amely esemény hello hello időbélyeg érkező hello IoT-központ, amely `EventEnqueuedUtcTime`. tooprocess adatok hello időbélyeg használatával hello eseménytartalom adatfolyamként, hello kell használnia [TIMESTAMP BY](https://msdn.microsoft.com/library/azure/dn834998.aspx) kulcsszó.

> [!NOTE]
> Csak a küldött üzenetek egy `DeviceClient` tulajdonság dolgozhatók fel.
> 
> 

### <a name="consumer-groups"></a>Felhasználói csoportok
Úgy kell konfigurálni minden Stream Analytics IoT hub bemeneti toohave a saját felhasználói csoportban. Ha egy feladat tartalmaz önillesztés, vagy ha több adatbevitele, néhány bemeneti után a több olvasót olvashatják. Ez a helyzet hatással van az olvasók egyetlen felhasználói csoportokban hello számát. tooavoid meghaladó hello Azure IoT Hub legfeljebb öt olvasók / partíciónként fogyasztói csoportot, akkor a bevált gyakorlat toodesignate egy fogyasztói csoportot minden egyes Stream Analytics-feladat.

### <a name="configure-an-iot-hub-as-a-data-stream-input"></a>A bemeneti adatfolyamban az IoT-központ konfigurálása
hello következő táblázat ismerteti az egyes tulajdonságai hello **új bemeneti** panel az Azure-portálon az IoT-központ bemenetként konfigurálásakor hello.

| Tulajdonság | Leírás |
| --- | --- |
| **A bemeneti alias** |Egy rövid nevet használt hello feladat lekérdezés tooreference ez adjon meg.|
| **Az IoT-központ** |hello IoT hub toouse bemenetként hello neve. |
| **Végpont** |az IoT-központ hello hello végpontja.|
| **Megosztott elérési házirend neve** |hello megosztott hozzáférési házirend, amely toohello IoT-központ hozzáférést biztosít. Minden megosztott elérési házirend rendelkezik egy nevet, hogy Ön meghatározott engedélyekkel és hozzáférési kulcsokkal. |
| **Megosztott elérési házirend kulcs** |hello megosztott elérési kulcsot tooauthorize hozzáférés toohello IoT-központ használja. |
| **Felhasználói csoport** (nem kötelező) |hello fogyasztói csoportot toouse tooingest adatait hello IoT-központot. Ha nincs felhasználói csoportban van megadva, a Stream Analytics-feladat használ hello alapértelmezett felhasználói csoport. Azt javasoljuk, hogy használja-e egy másik felhasználói csoport minden egyes Stream Analytics-feladathoz. |
| **Esemény szerializálási formátum** |hello szerializálási formátum (JSON, CSV vagy Avro) az hello bejövő adatfolyam. |
| **Kódolás** |Az UTF-8 jelenleg hello csak támogatott kódolási formátum. |

Amikor az adatokat az IoT-központ származik, a Stream Analytics-lekérdezés metaadatmezőket a következő hozzáférési toohello közül választhat:

| Tulajdonság | Leírás |
| --- | --- |
| **EventProcessedUtcTime** |hello dátum és idő, hogy hello esemény feldolgozása. |
| **EventEnqueuedUtcTime** |hello dátum és idő, hogy hello esemény érkezett hello IoT-központot. |
| **PartitionId** |hello nulla alapú Partícióazonosító hello bemeneti adapterhez. |
| **IoTHub.MessageId** | Az IoT hubon toocorrelate kétirányú kommunikációt használt azonosító. |
| **IoTHub.CorrelationId** |A üzenet válaszok és az IoT hubon visszajelzés használt azonosító. |
| **IoTHub.ConnectionDeviceId** |hello hitelesítési Azonosítót használja toosend ezt az üzenetet. Ez az érték servicebound üzenetek nyomtatva hello IoT hubból. |
| **IoTHub.ConnectionDeviceGenerationId** |hello Létrehozás-azonosítója a hello hitelesített használt toosend eszköz ezt az üzenetet. Ez az érték servicebound üzenetek nyomtatva hello IoT hubból. |
| **IoTHub.EnqueuedTime** |hello idő, amikor hello IoT-központ hello üzenetet kapott. |
| **IoTHub.StreamId** |Hello küldő eszköz által hozzáadott egyéni esemény tulajdonság. |


## <a name="create-data-stream-input-from-blob-storage"></a>Adatfolyam-bemenetre Blob-tároló létrehozása
Nagy mennyiségű strukturálatlan adatok toostore hello felhőben forgatókönyvek esetén az Azure Blob storage egy költséghatékony, méretezhető megoldást kínál. Blob Storage tárolóban lévő adatok általában számít inaktív adatok. Azonban dolgozható adatok adatfolyamként Stream Analytics. A Blob storage bemenetek Stream Analytics a jellemző forgatókönyv napló feldolgozásával. Ebben a forgatókönyvben telemetriai adatokat a rendszerből rögzített, és igényeinek toobe elemzése és feldolgozott tooextract lekérdezhetik a fontos adatok.

hello Stream Analytics a Blob storage-események alapértelmezett időbélyegzője, amely hello blob hello időbélyeg voltak utoljára módosítva, amely `BlobLastModifiedUtcTime`. tooprocess adatok hello időbélyeg használatával hello eseménytartalom adatfolyamként, hello kell használnia [TIMESTAMP BY](https://msdn.microsoft.com/library/azure/dn834998.aspx) kulcsszó.

CSV-formátumú bemeneti *szükséges* egy fejléc sor toodefine mezők hello adatkészletnél. Minden sor fejlécmezők emellett egyedinek kell lennie.

> [!NOTE]
> A Stream Analytics nem támogatja a meglévő tartalom tooan blob hozzáadását. A Stream Analytics csak egyszer tekinthetik blob, és az esetleges változásokat hello BLOB után hello feladat rendelkezik-e olvasási hello adatok nincsenek feldolgozva. Az ajánlott eljárás tooupload összes hello adatok egyszer, és nem adja hozzá események toothat blob tároló.
> 

### <a name="configure-blob-storage-as-a-data-stream-input"></a>A bemeneti adatfolyamban Blob-tároló konfigurálása

hello következő táblázat ismerteti az egyes tulajdonságai hello **új bemeneti** panel az Azure-portálon bemeneti Blob-tároló konfigurálásakor hello.

| Tulajdonság | Leírás |
| --- | --- |
| **A bemeneti alias** | Egy rövid nevet használt hello feladat lekérdezés tooreference ez adjon meg. |
| **Storage-fiók** | hello neve hello tárfiókot, ahol hello blob fájlok találhatók. |
| **Tárfiók kulcsa** | hello tartozó titkos kulcsot hello tárfiók. |
| **Tároló** | hello blob bemeneti hello tárolója. Tárolók adja meg a Microsoft Azure Blob szolgáltatás hello tárolt blobok logikai csoportosítása. Ha feltölt egy blobot toohello Azure Blob storage szolgáltatásban, meg kell adnia, hogy a blob tárolója. |
| **Elérési út mintája** (nem kötelező) | hello fájl elérési útját használt toolocate hello blobok hello megadott tárolóban. Hello elérési útban, adja meg a következő három változók hello egy vagy több példányát: `{date}`, `{time}`, vagy`{partition}`<br/><br/>1. példa:`cluster1/logs/{date}/{time}/{partition}`<br/><br/>2. példa:`cluster1/logs/{date}`<br/><br/>Hello `*` karakter nincs hello elérési előtag engedélyezett érték. Csak érvényes <a HREF="https://msdn.microsoft.com/library/azure/dd135715.aspx">Azure blob karakterek</a> engedélyezettek. |
| **Dátumformátum** (nem kötelező) | Hello dátum változó hello elérési utat használja, ha hello dátumformátum mely hello a fájlok vannak rendezve. Példa:`YYYY/MM/DD` |
| **Időformátum** (nem kötelező) |  Hello idő változó hello elérési utat használja, ha hello időformátum mely hello a fájlok vannak rendezve. Csak a támogatott hello értéke jelenleg `HH`. |
| **Esemény szerializálási formátum** | hello szerializálási formátum (JSON, CSV vagy Avro) a bejövő adatfolyamokhoz. |
| **Kódolás** | A fürt megosztott kötetei szolgáltatás és a JSON UTF-8 jelenleg hello csak támogatott kódolási formátum. |

Ha az adatok egy Blob storage forrásból, metaadatmezőket, a Stream Analytics-lekérdezés a következő hozzáférési toohello közül választhat:

| Tulajdonság | Leírás |
| --- | --- |
| **Blobnév** |hello neve hello bemeneti blob hello esemény származik. |
| **EventProcessedUtcTime** |hello dátum és idő, hogy hello esemény Stream Analytics lett feldolgozva. |
| **BlobLastModifiedUtcTime** |hello dátuma és időpontja, utolsó módosításának, hogy a hello blob. |
| **PartitionId** |hello nulla alapú Partícióazonosító hello bemeneti adapterhez. |

Például használja ezeket a mezőket, írhat egy lekérdezést, például a következő példa hello:

````
SELECT
    BlobName,
    EventProcessedUtcTime,
    BlobLastModifiedUtcTime
FROM Input
````

## <a name="get-help"></a>Segítségkérés
Ha további segítségre van szüksége, próbálkozzon a [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Következő lépések
Hogy megismerkedett adatok kapcsolat beállításai az Azure-ban a Stream Analytics-feladatok. toolearn Stream Analytics kapcsolatos további információkért lásd:

* [Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)
* [Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)
* [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)
* [Az Azure Stream Analytics felügyeleti REST API referenciája](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
