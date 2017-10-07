---
title: a Stream Analytics aaaJSON kimeneti |} Microsoft Docs
description: "Ismerje meg, hogyan Stream Analytics egy célcsoport kijelölésével az Azure Cosmos DB JSON kimeneti adatok archiválása és alacsony késésű lekérdezései strukturálatlan JSON-adatokat."
keywords: JSON kimeneti
documentationcenter: 
services: stream-analytics,documentdb
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 5d2a61a6-0dbf-4f1b-80af-60a80eb25dd1
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: fa717818c839ecd7a60fcee33d22011990fd5878
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="target-azure-cosmos-db-for-json-output-from-stream-analytics"></a>Cél Azure Cosmos DB JSON-kimenetét a Stream Analytics
A Stream Analytics célba [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) JSON-kimenetét, engedélyezi az archiválási és alacsony késésű kérelmek strukturálatlan JSON-adatokat. Ez a dokumentum ismertet néhány gyakorlati tanácsok a konfiguráció alkalmazásához.

Azok számára, akiknek nem ismeri az Cosmos DB, vessen egy pillantást [Azure Cosmos DB képzési terv](https://azure.microsoft.com/documentation/learning-paths/documentdb/) tooget elindult. 

Megjegyzés: A Mongo DB API alapú Cosmos DB gyűjtemények jelenleg nem támogatott. 

## <a name="basics-of-cosmos-db-as-an-output-target"></a>Egy kimeneti célként Cosmos DB alapjai
hello Azure Cosmos DB kimenetet a Stream Analytics lehetővé teszi, hogy írása a streamfeldolgozási eredmények JSON kimenetként a Cosmos DB következő gyűjtemény(ek) készleteit szinkronizálja azokat. A Stream Analytics nem hoz létre gyűjteményeket az adatbázis ehelyett nincs szükség toocreate őket előzetes megfizetése esetén. Ez azért, hogy hello számlázási költségek Cosmos DB gyűjtemények átlátszó tooyou, és így hello teljesítmény hangolás konzisztencia és a gyűjtemények közvetlenül kapacitásának hello [Cosmos DB API-k](https://msdn.microsoft.com/library/azure/dn781481.aspx). Azt javasoljuk, egy Cosmos DB adatbázishoz folyamatos átviteli feladat toologically külön a folyamatos átviteli feladat-gyűjteményekben.

Hello Cosmos DB adatgyűjtési beállítások némelyike részleteket lejjebb olvashatja.

## <a name="tune-consistency-availability-and-latency"></a>Konzisztencia, a rendelkezésre állás és a késleltetés hangolása
toomatch alkalmazás igényeinek, Cosmos DB lehetővé teszi toofine hangolási hello adatbázis és a gyűjtemények és a ellenőrizze kompromisszumot konzisztencia, a rendelkezésre állás és a késleltetés között. Attól függően, hogy milyen szintű olvasási konzisztenciát elleni forgatókönyv igényeinek és olvasási késés, kiválaszthatja a konzisztenciaszint adatbázis fiókja. Alapértelmezés szerint is Cosmos DB engedélyezi szinkron indexelő minden CRUD művelet tooyour gyűjteményen. Ez az egy másik kapcsolót toocontrol hello írási/olvasási Cosmos DB teljesítményét. A témakörrel kapcsolatos további információkért tekintse át a hello [módosítsa az adatbázis és a lekérdezés konzisztenciaszintek](../documentdb/documentdb-consistency-levels.md) cikk.

## <a name="upserts-from-stream-analytics"></a>A Stream Analytics Upserts
Cosmos DB Stream Analytics integrációja lehetővé teszi tooinsert vagy frissítés rögzíti a Cosmos DB gyűjtemény egy adott dokumentum azonosító oszlop alapján. Ez a hivatkozott tooas is egy *Upsert*.

A Stream Analytics használja az optimista Upsert módszert használja, ha frissítések csak végzett amikor insert tooa Dokumentumazonosítója ütközés miatt sikertelen. A frissítés végzi el a Stream Analytics, a javítás, lehetővé teszi, hogy a részleges frissítési toohello dokumentum, azaz hozzáadása új tulajdonságok vagy cseréje a meglévő tulajdonságot Növekményesen történik. Vegye figyelembe, hogy hello értékek tömb-tulajdonságokat a JSON-dokumentum változásait hello teljes tömb első felül eredményez, azaz nem egyesített hello tömb.

## <a name="data-partitioning-in-cosmos-db"></a>A Cosmos DB adatparticionálás
A cosmos DB [particionált gyűjtemények](../cosmos-db/partition-data.md) ajánlott módszer az adatok felosztásának hello. 

Egyetlen Cosmos DB gyűjtemények Stream Analytics továbbra is lehetővé teszi az adatok alapján hello lekérdezési mintáknak és a teljesítménye az alkalmazás igényeinek toopartition. Minden lehetséges, hogy too10GB adatok (maximum), és jelenleg nincs módja tooscale fel van másolatot tartalmaz (vagy túlcsordulás) gyűjtemény. Kiterjesztése, a Stream Analytics lehetővé teszi egy adott előtaggal rendelkező toowrite toomultiple gyűjtemények (használati részleteit lásd alább). A Stream Analytics használ konzisztens hello [kivonatoló partíció feloldó](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.partitioning.hashpartitionresolver.aspx) hello felhasználó alapján stratégia megadott PartitionKey oszlop toopartition a kimeneti rögzíti. hello előtag megadott folyamatos átviteli feladat kezdési ideje hello hello gyűjtemények száma vettük hello kimeneti partíciószám toowhich hello feladat ír tooin párhuzamos (Cosmos DB gyűjtemények = kimeneti partíciók). A lusta indexelési ezzel együtt egyetlen gyűjtemény csak szúrja be, kapcsolatos 0,4 MB/s teljesítménye várhatók. Több gyűjteményt használ lehetővé tehetik, hogy tooachieve nagyobb átviteli teljesítményt és nagyobb kapacitást.

Ha a jövőben hello tooincrease hello partíciók száma, szükség lehet toostop a feladatot, a meglévő gyűjtemények új gyűjtemények, majd indítsa újra hello Stream Analytics-feladat újraparticionálása hello adatait. További részleteket a PartitionResolver használatával, és újra particionálás mintakód, valamint követő post fognak szerepelni. hello cikk [particionálás és Cosmos DB skálázás](../documentdb/documentdb-partition-data.md) is részletesen itt.

## <a name="cosmos-db-settings-for-json-output"></a>JSON kimeneti cosmos DB beállításai
Az alább látható információt adatkérést Cosmos-adatbázis létrehozása a Stream Analytics kimenetként állít elő. Ez a szakasz hello tulajdonságok definíció magyarázattal szolgál.

Particionált gyűjtemény | Több "Egypartíciós" gyűjtemény
---|---
![a documentdb stream analytics kimeneti képernyő](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output-1.png) |  ![a documentdb stream analytics kimeneti képernyő](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output-2.png)


  
> [!NOTE]
> Hello **több "Egypartíciós" gyűjtemény** forgatókönyv partíciós kulcs használatát igényli, és a támogatott konfiguráció. 

* **A kimeneti Alias** – egy alias toorefer ezt a kimenetet a ASA lekérdezés  
* **A fiók neve** – hello nevét vagy a végpont URI-jának hello Cosmos DB fiók.  
* **Kulcs fiók** – Cosmos DB fiók hello hello megosztott hozzáférési kulcsot.  
* **Adatbázis** – hello Cosmos DB adatbázis neve.  
* **Gyűjteménynévmintája** – hello gyűjtemény neve vagy a használt hello gyűjtemények toobe mintát. hello gyűjteménynév-formátum használatával hello opcionális {partition} token, ahol a partíciók 0-tól kezdődnek lehet létrehozni. Minta érvényes bemenetei a következők:  
  1\) MyCollection – egy gyűjteményt a következő "MyCollection" néven már léteznie kell.  
  2\) MyCollection {partition} – ilyen gyűjteményeknek létezniük kell – "MyCollection0", "MyCollection1", "MyCollection2" és így tovább.  
* **Kulcs partícióazonosító** – nem kötelező. Ez csak akkor van szükség, ha a gyűjteménynévmintája {particionáló} jogkivonatot használ. a kimeneti használt események toospecify hello kulcsban a kimenet gyűjtemények közötti particionálására hello mező hello nevét. Egyetlen gyűjtemény kimeneti bármilyen tetszőleges kimeneti oszlop lehet például PartitionId használt.  
* **Dokumentálja azonosító** – nem kötelező. a kimeneti eseményekben a hello mező hello nevét használja toospecify hello elsődleges kulcs mely Beszúrás vagy frissítés műveletek alapulnak.  
