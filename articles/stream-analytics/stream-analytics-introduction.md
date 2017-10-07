---
title: aaaIntroduction tooStream Analytics |} Microsoft Docs
description: "További információk a Stream Analytics egy felügyelt szolgáltatást, amelyek segítségével elemezheti hello az eszközök internetes hálózatát (IoT) származó valós időben."
keywords: "szolgáltatásként kínált elemzés, felügyelt szolgáltatások, streamfeldolgozás, streamelemzés, mi a stream analytics"
services: stream-analytics
documentationcenter: 
author: jenniehubbard
manager: jhubbard
editor: cgronlun
ms.assetid: 613c9b01-d103-46e0-b0ca-0839fee94ca8
ms.service: stream-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 08/08/2017
ms.author: jhubbard
ms.openlocfilehash: 6dd7ea1d358bcc94e927a3e699a2771a25104d72
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-stream-analytics"></a>Mi a Stream Analytics?

Az Azure Stream Analytics teljes körűen felügyelt eseményfeldolgozó motor, amellyel valós idejű elemző számítások állíthatók be adatfolyamokra. hello adatok eszközök, érzékelőket, webhelyek, közösségi hírcsatornák, alkalmazások, infrastruktúra-rendszereknek és több származhatnak. 

## <a name="what-can-i-do-with-stream-analytics"></a>Mire használhatom a Stream Analytics szolgáltatást?

Használja a Stream Analytics tooexamine nagy adatmennyiségek áramlanak eszközök és folyamatok, információk kinyerése hello adatfolyamban, és keressen a minták, a trendek és a kapcsolatokat. A hello adatok alapján, majd feladatokat hajthat végre alkalmazás. Például, előfordulhat, hogy riasztást, automation munkafolyamatok indítsa, eszköz, például a Power BI reporting adatokat tooa hírcsatorna vagy későbbi vizsgálat adatainak tárolásához. 

Példák:

* Pénzügyi szolgáltatók által kínált személyre szabott, valós idejű tőzsdeelemzések és riasztások.
* Tranzakciós adatok vizsgálatán alapuló valós idejű csalásészlelés. 
* Adat- és személyazonosság-védelmi szolgáltatások.
* Fizikai objektumokba ágyazott érzékelők és indítószerkezetek (IoT) által generált adatok elemzése.
* Webes kattintássorozat-elemzés.
* Ügyfélkapcsolat-kezelő (CRM-) alkalmazások, amelyek például riasztást küldenek, ha az ügyfélélmény romlik egy időkereten belül.

## <a name="how-does-stream-analytics-work"></a>Hogyan működik a Stream Analytics?

Az ábra azt mutatja, hogyan adatok okozhatnak, elemzése, és elküldheti bemutató vagy művelet megjelenítő hello Stream Analytics folyamat. 

![Stream Analytics folyamatábra](./media/stream-analytics-introduction/stream_analytics_intro_pipeline.png)

A Stream Analytics egy adatfolyam-forrásból indul ki. hello adatokat egy eszközről, IoT-központ vagy az Azure event hubs az Azure meghatározták. hello adatok Azure Blob Storage például adattárat is lekért. 

tooexamine hello stream, létrehozhat egy Stream Analytics *feladat* , amely megadja, ahol hello adat érkezik. hello feladatot is megadja egy *átalakítása*&mdash;hogyan toolook adatokat, a minták vagy a kapcsolatokat. Ebben a feladatban a Stream Analytics támogat egy SQL-szerű lekérdezési nyelvet, amely lehetővé teszi a streamelési adatok adott időtartamon belüli szűrését, rendezését, összesítését és egyesítését.

Végezetül hello feladat megadja egy kimeneti toosend hello átalakított adatokat. Ennek segítségével szabályozhatja, hogy milyen toodo válasz toohello adatot is elemezheti. Például a válasz tooanalysis, akkor előfordulhat, hogy:

* A parancs toochange küldése egy Eszközbeállítások. 
* A folyamat, amely alapján úgy találja, a művelet által figyelt adatok tooa várólista küldése. 
* Adatok tooa Power BI-irányítópulton a jelentési küldése.
* Például a Data Lake Store, az SQL Server-adatbázis vagy az Azure Blob és Table storage adatok toostorage küldése.

Figyelhet egy feladatot a futása közben, és átállíthatja a másodpercenként feldolgozott események számát. Beállíthat feladatokat diagnosztikai naplók előállítására hibaelhárításhoz.

## <a name="key-capabilities-and-benefits"></a>Főbb képességek és előnyök

A Stream Analytics tervezett toobe könnyen toouse, méret és gazdaságos rugalmas, méretezhető tooany feladat.

### <a name="connectivity-toomany-inputs-and-outputs"></a>Kapcsolat toomany bemenetekhez és kimenetekhez

A Stream Analytics túl csatlakozik közvetlenül[Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) és [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/) streameket, és hello [Azure Blob storage szolgáltatás](https://docs.microsoft.com/azure/storage/storage-introduction#blob-storage-accounts) tooingest előzményadatok. Ha az adatok eseményközpontokból származnak, akkor a Stream Analytics más adatforrásokkal és feldolgozó motorokkal is kombinálható.

A feladat bemenete referenciaadatokat (statikus vagy lassan változó adatokat) is tartalmazhat. Csatlakozhat a streamelési adatok toothis hivatkozás adatok tooperform keresési műveletek hello módon az adatbázis-lekérdezést.

A Stream Analytics-feladat kimenetét különböző célok felé továbbíthatja. Toostorage, például az Azure Storage blobsba vagy táblák, Azure SQL Database, Azure Data Lake-tárolók vagy Azure Cosmos DB írhat. Ott hello adatok mehet kötegelt elemzésekben az Azure HDInsight keresztül. Egy másik folyamat, például az event hubs, Azure Service Bus-üzenettémakörök vagy várólisták el tudja küldeni hello kimeneti tooanother szolgáltatás felhasználásra. A képi megjelenítéshez tartozó hello kimeneti tooPower BI el tudja küldeni.

### <a name="ease-of-use"></a>Könnyű használat

toodefine átalakítások, használhat egy egyszerű, deklaratív [Stream Analytics lekérdezési nyelv](https://msdn.microsoft.com/library/azure/dn834998.aspx) , amely lehetővé teszi a kifinomult elemzést nem programozási hozzon létre. hello lekérdezési nyelv streamadatok fogadja a bemeneti adatként. Ezután a szűrési és rendezési hello adatok értéket összesítő, számításokat, adatokat (belül adatfolyam vagy tooreference adatok) és a földrajzi funkciók. Szerkesztheti lekérdezések hello portál, az IntelliSense és a szintaxis-ellenőrzés segítségével, és tesztelheti a lekérdezéseket is kibonthat egy élő adatfolyam hello mintaadatok használatával.

### <a name="extensible-query-language"></a>Bővíthető lekérdező nyelv

Hello lekérdezési nyelv hello képességeit és további funkciók meghívása terjeszthetők ki. Függvényhívások hello Azure Machine Learning szolgáltatás tootake Azure Machine Learning megoldások előnyeit is meghatározhatja. A Stream Analytics-lekérdezés részeként rendelés tooperform összetett számítások is integrálhatja JavaScript felhasználói függvény (UDF).

### <a name="scalability"></a>Méretezhetőség

A Stream Analytics képes kezelni too1 GB másodpercenként bejövő adatok mentése. Integráció a [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) és [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/) lehetővé teszi, hogy a feladatok tooingest több millió esemény másodpercenként a csatlakoztatott eszközön, kattintássorozatokból és naplófájlok tooname néhány. Hello partíció szolgáltatás az event hubs használatával akkor is partícióazonosító számítások be logikai lépésekre, az hello képes további toobe particionálva tooincrease méretezhetőséget.

### <a name="low-cost"></a>Alacsony költség

Egy felhőalapú szolgáltatás a Stream Analytics optimalizált, alacsony költséggel induláshoz toolet lesz. Menet alapján streaming unit használati és hello összeg hello rendszer által feldolgozott adatmennyiség után kell fizetnie. A használat számítása a feldolgozott események mennyisége hello és hello fürt toohandle belül kiépített számítási teljesítmény mennyisége hello Stream Analytics-feladatok.

### <a name="reliability-quick-recovery-and-repeatability"></a>Megbízhatóság, gyors helyreállítás és ismételhetőség

Hello felhőben felügyelt szolgáltatásként a Stream Analytics segítségével megakadályozza az adatvesztést és biztosítja az üzletmenet folytonosságát. Ha hiba történik, hello szolgáltatást biztosít a beépített helyreállítási képességek. Hello ügyfélgépek toointernally-állapot karbantartásához, hello szolgáltatás tooarchive lehetséges eseményeket, és alkalmazza újra a hello jövőben mindig ugyanazt az eredményt az hello első feldolgozási ismételhető eredményekkel biztosít. Ez lehetővé teszi időben toogo, és vizsgálja meg a számítások, ha a probléma alapvető okát, elemzési és így tovább.

## <a name="next-steps"></a>Következő lépések

* Első lépések: [kísérletezés IoT-eszközöktől származó bemenetekkel és lekérdezésekkel](stream-analytics-get-started-with-azure-stream-analytics-to-process-data-from-iot-devices.md).
* Build egy [végpont Stream Analytics megoldás](stream-analytics-real-time-fraud-detection.md) , amely megvizsgálja a telefon metaadatok toolook csalárd hívások.
* Tudnivalók hello SQL-szerű lekérdezési nyelv a Stream Analytics, illetve például egyedi fogalmak kapcsolatos [ablak Funkciók](stream-analytics-window-functions.md).
* Ismerje meg, hogyan túl[Stream Analytics-feladatok méretezése](stream-analytics-scale-jobs.md). 
* Ismerje meg, hogyan túl[integrálják a Stream Analytics és az Azure Machine Learning](stream-analytics-machine-learning-integration-tutorial.md).
* Válaszok tooyour Stream Analytics kérdéseket a hello [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

