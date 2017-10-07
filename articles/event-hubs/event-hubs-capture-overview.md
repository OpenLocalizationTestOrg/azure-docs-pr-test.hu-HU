---
title: "az Azure Event Hubs rögzítése aaaOverview |} Microsoft Docs"
description: "Az Event Hubs rögzítése telemetrikus adatokat rögzítése"
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: e53cdeea-8a6a-474e-9f96-59d43c0e8562
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: sethm;darosa
ms.openlocfilehash: 0238cae712a0ed7bdf3e87ee93a069a553cb65df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-event-hubs-capture"></a>Az Azure Event Hubs rögzítése

Az Azure Event Hubs rögzítése lehetővé teszi a streamelési adatok az Event Hubs tooan tooautomatically kézbesítése hello [Azure Blob Storage tárolóban](https://azure.microsoft.com/services/storage/blobs/) vagy [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/) fiók az Ön által választott, a hello további rugalmasságot egy alkalommal vagy méret időszakban megadására. Rögzítési beállítása gyors, nem azt, és azt méretezi automatikusan az Event Hubs adminisztrációs költségek toorun van [átviteli egységek](event-hubs-features.md#capacity). Event Hubs rögzítése hello streamelési adatok az Azure legegyszerűbb módja tooload, és lehetővé teszi az adatok feldolgozása és nem a rögzítés toofocus.

Event Hubs rögzítése lehetővé teszi a valós idejű tooprocess, és a batch-alapú folyamatok hello ugyanaz az adatfolyam. Ez azt jelenti, hogy idővel igényeknek nő megoldásokat hozhat létre. Batch-alapú rendszerek ma felé jövőbeli valós idejű feldolgozással követheti most felépítése, kívánó tooadd hatékony cold elérési tooan meglévő valós idejű megoldás, Event Hubs rögzítése révén könnyebben adatfolyam használata.

## <a name="how-event-hubs-capture-works"></a>Event Hubs rögzítése működése

Az Event Hubs egy idő-megőrzési telemetriai érkező, hasonló tooa elosztott napló tartós puffer. hello kulcs tooscaling az Event Hubs hello [particionált felhasználói modell](event-hubs-features.md#partitions). Mindegyik partíció az adatok egy független szegmense, és egymástól függetlenül felhasznált. Ezek az adatok elavulnak ki idővel hello konfigurálható megőrzési időtartam alapján. Emiatt egy adott eseményközpont soha nem megtelik "túl."

Event Hubs rögzítése lehetővé teszi, hogy Ön toospecify a saját Azure Blob storage-fiók és a tároló vagy az Azure Data Lake Store-fiók, amelyek használt toostore hello rögzített adatok. Ezek a fiókok hello lehet ugyanabban a régióban, az eseményközpont vagy egy másik régióban, toohello rugalmasságot hello Event Hubs rögzítése funkció hozzáadása.

A rögzített adatok [Apache Avro] [ Apache Avro] formátum: kompakt, gyors és bináris formátum, amely a beágyazott sémák gazdag adatstruktúrák biztosít. Ebben a formátumban széles körben használt hello Hadoop ökoszisztémájának, a Stream Analytics és az Azure Data Factory. Az Avro kezelésével kapcsolatos további információk a cikk későbbi részében érhető el.

### <a name="capture-windowing"></a>Leképezési rögzítése

Event Hubs rögzítése lehetővé teszi, hogy a tooset fel egy ablak toocontrol rögzítése. Ebben az ablakban egy minimális és időpontjának beállítása "első wins házirendnek,", amely hello első észlelt eseményindító okok rögzítési művelet jelentését. Ha egy 15 perc, 100 MB Rögzítés ablak, és 1 MB protokollüzenetek másodpercenkénti, hello mérete ablak eseményindítók hello időszak előtt. Mindegyik partíció egymástól függetlenül rögzíti, és befejezett blokkblob írja a rögzítést, hello időpontjában az nevű hello ideje, mely hello rögzítési időköz történt. hello tároló elnevezési egyezmény a következőképpen történik:

```
[namespace]/[event hub]/[partition]/[YYYY]/[MM]/[DD]/[HH]/[mm]/[ss]
```

### <a name="scaling-toothroughput-units"></a>Toothroughput méretezési egységek

Event Hubs forgalom által szabályozott [átviteli egységek](event-hubs-features.md#capacity). Egy átviteli egység lehetővé teszi, hogy 1 MB másodpercenkénti bemenő és kimenő forgalom kétszer adott mennyiségű második vagy 1000 esemény. Standard Event Hubs 1 – 20 átviteli egység konfigurálhatók, és vásárolhat további, a kvóta növeléséhez [támogatási kérelem][support request]. Használat meghaladja a megvásárolt átviteli egységek folyamatban van. Event Hubs rögzítése másol adatokat közvetlenül hello belső Event Hubs tároló átviteli egység kilépő kvóták kihagyásával, és a kimenő forgalom mentése más feldolgozási olvasók, például a Stream Analytics vagy Spark.

Beállítása után Event Hubs rögzítése automatikusan fut, amikor az első esemény küldi el, és továbbra is futnak. toomake könnyebb az alárendelt feldolgozási tooknow hello folyamat működik, az Event Hubs a ír, üres fájlok amikor nincsenek adatok. Ez a folyamat előre jelezhető ütemben történik és jelző, amely képes a kötegelt processzor biztosít.

## <a name="setting-up-event-hubs-capture"></a>Event Hubs rögzítése beállítása

Létrehozáskor hello event hub hello segítségével konfigurálhatja a rögzítési [Azure-portálon](https://portal.azure.com), vagy az Azure Resource Manager-sablonok használatával. További információkért tekintse meg a következő cikkek hello:

- [Engedélyezze az Event Hubs rögzítése hello Azure-portál használatával](event-hubs-capture-enable-through-portal.md)
- [Hozzon létre egy Event Hubs névtér eseményközpontban, és engedélyezze a rögzítést az Azure Resource Manager-sablonok](event-hubs-resource-manager-namespace-event-hub-enable-capture.md)

## <a name="exploring-hello-captured-files-and-working-with-avro"></a>Hello rögzített fájlok fel, és az Avro használata

Event Hubs rögzítése fájlokat hozza létre az Avro formátum megadott hello beállított időszak. Megtekintheti ezeket a fájlokat, mint bármely eszköz [Azure Tártallózó][Azure Storage Explorer]. Letöltheti a hello fájlok helyi toowork rajtuk.

Event Hubs rögzítése által előállított hello fájlok az Avro-séma a következő hello rendelkezik:

![][3]

Egy egyszerű módot tooexplore az Avro-fájlok segítségével hello el [Avro eszközök] [ Avro Tools] az Apache jar. A jar a letöltés után megtekintheti egy konkrét Avro fájl hello séma hello a következő parancs futtatásával:

```
java -jar avro-tools-1.8.2.jar getschema <name of capture file>
```

Ez a parancs visszaadja

```
{

    "type":"record",
    "name":"EventData",
    "namespace":"Microsoft.ServiceBus.Messaging",
    "fields":[
                 {"name":"SequenceNumber","type":"long"},
                 {"name":"Offset","type":"string"},
                 {"name":"EnqueuedTimeUtc","type":"string"},
                 {"name":"SystemProperties","type":{"type":"map","values":["long","double","string","bytes"]}},
                 {"name":"Properties","type":{"type":"map","values":["long","double","string","bytes"]}},
                 {"name":"Body","type":["null","bytes"]}
             ]
}
```

Is Avro eszközök tooconvert hello tooJSON formátumát használja, és hajtsa végre a más feldolgozása.

Speciális feldolgozása, letöltése és telepítése Avro a választott platform tooperform. Hello írásának időpontjában, megvalósítások rendelkezésre állnak olyan C, C++, C\#, Java, NodeJS, Perl, PHP, Python vagy Ruby.

Apache Avro rendelkezik teljes bevezetés útmutatói [Java] [ Java] és [Python][Python]. Hello is olvasható [Bevezetés az Event Hubs rögzítése](event-hubs-capture-python.md) cikk.

## <a name="how-event-hubs-capture-is-charged"></a>Hogyan Event Hubs rögzítése feladata

Event Hubs rögzítése forgalmi díjas hasonlóképpen toothroughput egységek: mint egy óránkénti kell fizetni. hello kell fizetni közvetlenül arányos toohello hello névtér megvásárolt átviteli egységek száma. Átviteli egységek növelhető és csökkenthető, Event Hubs rögzítése mérőszámok növelheti és csökkentheti a megfelelő teljesítmény tooprovide. hello mérőszámok párhuzamosan történik. Díjszabása, lásd: [Event Hubs-díjszabás](https://azure.microsoft.com/pricing/details/event-hubs/). 

## <a name="next-steps"></a>Következő lépések

Hello legegyszerűbb módja tooget adatokat az Azure Event Hubs rögzítése funkció. Az Azure Data Lake, az Azure Data Factory és az Azure HDInsight, kötegfeldolgozási hajthat végre, és más jól ismert eszközökkel és a platformok elemzés, bármilyen léptékben van szüksége.

További információ az Event Hubs érhetők el a következő hivatkozások hello:

* [Küldés és fogadás események első lépései](event-hubs-dotnet-framework-getstarted-send.md)
* [Az Event Hubsot használó teljes mintaalkalmazás][sample application that uses Event Hubs]
* [Event Hubs – áttekintés][Event Hubs overview]

[Apache Avro]: http://avro.apache.org/
[support request]: https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[Azure Storage Explorer]: http://azurestorageexplorer.codeplex.com/
[3]: ./media/event-hubs-capture-overview/event-hubs-capture3.png
[Avro Tools]: http://www-us.apache.org/dist/avro/avro-1.8.2/java/avro-tools-1.8.2.jar
[Java]: http://avro.apache.org/docs/current/gettingstartedjava.html
[Python]: http://avro.apache.org/docs/current/gettingstartedpython.html
[Event Hubs overview]: event-hubs-what-is-event-hubs.md
[sample application that uses Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Scale out Event Processing with Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
