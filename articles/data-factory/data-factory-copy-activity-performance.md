---
title: "Tevékenység aaaCopy teljesítmény- és hangolási útmutató |} Microsoft Docs"
description: "További információk a másolási tevékenység használatakor az Azure Data Factory adatmozgás hello teljesítményét befolyásoló legfontosabb tényezők."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 4b9a6a4f-8cf5-4e0a-a06f-8133a2b7bc58
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: jingwang
ms.openlocfilehash: b0fb5a76c34752d07e8ddfffbb799a05fb5d6be6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="copy-activity-performance-and-tuning-guide"></a>Másolja a tevékenység teljesítmény- és hangolási útmutató
Az Azure Data Factory másolási tevékenység egy első osztályú biztonságos, megbízható és nagy teljesítményű Adatbetöltési megoldást nyújt. Ez lehetővé teszi toocopy több terabájtos adatkészleteket naponta felhő gazdag számos és a helyszíni adattárolókhoz. Blazing-gyors Adatbetöltési teljesítmény hello core "big data" probléma összpontosíthat kulcs tooensure: speciális elemzési megoldások kialakításához, és lekérése mélyebben elemezheti az adatokat.

Azure számos vállalati szintű adatok tárolási és adatok adatraktár megoldások, és a másolási tevékenység során magas szinten optimalizált Adatbetöltési könnyen tooconfigure, és állítsa be élményt nyújt. Csak egyetlen példány tevékenységgel érhet el:

* Az adatok betöltése **Azure SQL Data Warehouse** : **1,2 GB/s**. A használati esetek bemutatóért lásd: [1 TB-os betöltése az Azure SQL Data Warehouse a 15 perc Azure Data Factory](data-factory-load-sql-data-warehouse.md).
* Az adatok betöltése **Azure Blob Storage tárolóban** : **1,0 GB/s**
* Az adatok betöltése **Azure Data Lake Store** : **1,0 GB/s**

Ez a cikk ismerteti:

* [Hivatkozás számok](#performance-reference) a támogatott forrás és a fogadó adatokat tároló toohelp, tervezze meg a projekt;
* Képes javítani funkciókra hello különböző helyzetekben, például másolás átviteli [adatátviteli adategységek cloud](#cloud-data-movement-units), [másolási párhuzamos](#parallel-copy), és [másolási előkészített](#staged-copy);
* [Teljesítményhangolás útmutatást](#performance-tuning-steps) a hogyan másolja át a tootune hello teljesítmény- és hello tényezők befolyásolhatja a teljesítményt.

> [!NOTE]
> Ha nem ismeri a másolási tevékenység során általában, lásd: [adatok áthelyezése a másolási tevékenység segítségével](data-factory-data-movement-activities.md) a cikk elolvasása előtt.
>

## <a name="performance-reference"></a>Teljesítmény-hivatkozás

Referenciaként táblázat alatti hello másolási átviteli számát mutatja MB/s mértékegységben a belső tesztekre alapozva, a forrás és a fogadó párok megadott hello. Az összehasonlításhoz, azt is bemutatja, különböző beállításait [adatátviteli adategységek cloud](#cloud-data-movement-units) vagy [az adatkezelési átjáró méretezhetőség](data-factory-data-management-gateway-high-availability-scalability.md) a fájlmásolás (több átjárócsomópontok) segítségével.

![Teljesítmény mátrix](./media/data-factory-copy-activity-performance/CopyPerfRef.png)


**Pontok toonote:**
* Átviteli sebesség számítja ki a következő képlet hello: [forrás olvasható adatok mérete] / [a másolási tevékenység időtartama futtatása].
* hello hivatkozás számok hello tábla volt mérni [TPC-H](http://www.tpc.org/tpch/) adathalmaz egy egyetlen másolási tevékenység során futtassa.
* Az Azure data áruházakból hello forrás és a fogadó hello szerepelnek azonos Azure-régiót.
* A hibrid másolás között a helyszíni és felhőalapú adattároló, minden átjárócsomópont működő állapotban volt, de a adattárból hello helyszíni a meghatározás alatt külön gépen. Ha egyetlen tevékenység átjáró kiszolgálón már futott, hello másolási művelet felhasználta csak kis részét hello teszt gép CPU, a memória vagy a hálózati sávszélesség. A további [szempont az adatkezelési átjáró](#considerations-for-data-management-gateway).
    <table>
    <tr>
        <td>CPU</td>
        <td>32 processzormag 2,20 GHz -es Intel Xeon E5-2660 v2</td>
    </tr>
    <tr>
        <td>Memory (Memória)</td>
        <td>128 GB</td>
    </tr>
    <tr>
        <td>Network (Hálózat)</td>
        <td>Internetes adapter: 10 GB/s; intranetes adapterének: 40 GB/s</td>
    </tr>
    </table>


> [!TIP]
> Használatával a további adatok adatátviteli egység (DMUs) hello-nál nagyobb átviteli sebesség érhető el az alapértelmezett maximális DMUs, amely egy felhő-felhőbe történő másolás tevékenységfuttatási 32. Például a 100 DMUs érhet el az adatok másolását az Azure Blob az Azure Data Lake Store: **1.0GBps**. Lásd: hello [Cloud data adatátviteli egység](#cloud-data-movement-units) szakasz Ez a szolgáltatás és a hello támogatott forgatókönyvet. Ügyfél [az Azure támogatási](https://azure.microsoft.com/support/) toorequest további DMUs.

## <a name="parallel-copy"></a>Párhuzamos másolása
Hello adatainak forrás-, vagy írjon a adatok toohello cél olvasható **belül a másolási tevékenység során futtassa párhuzamosan**. Ez a funkció növeli a másolási műveletek hello átviteli, és csökkenti a hello időt toomove adatok.

Ez a beállítás különbözik a hello **egyidejűségi** hello tevékenységdefinícióban tulajdonság. Hello **egyidejűségi** tulajdonság határozza meg, hogy hello **egyidejű másolási tevékenység fut** különböző tevékenység windows tooprocess adatait (1 óra too2 VAGYOK, Reggel 2 too3 VAGYOK, 3 AM too4, és így tovább). Ez a funkció akkor hasznos, ha egy korábbi terheléselosztási elvégezhető. hello párhuzamos másolási képesség vonatkozik tooa **tevékenységfuttatási egyetlen**.

Egy mintaforgatókönyv vizsgáljuk meg. A következő példa hello az elmúlt hello több szelet feldolgozása toobe kell. Adat-előállító fut az egyes szeletek másolási tevékenység (egy tevékenység futott) példánya:

* hello adatszelet hello első tevékenység ablakban (1 óra too2 VAGYOK) == > tevékenység fut 1
* hello adatszelet hello második tevékenység ablakában (2 óra too3 VAGYOK) == > tevékenység fut 2
* hello adatszelet hello második tevékenység ablakában (3 AM too4 VAGYOK) == > tevékenység fut 3

És így tovább.

Ebben a példában, amikor hello **egyidejűségi** értéke too2, **tevékenység fut 1** és **tevékenység fut 2** adatokat másolni két tevékenység windows **egyidejűleg**  tooimprove adatok adatátviteli teljesítményét. Azonban ha több fájl Tevékenységfuttatási 1 társul, hello adatátviteli szolgáltatás fájlokat másolja forrásfájlból hello toohello cél egy egyszerre.

### <a name="cloud-data-movement-units"></a>A mozgás adategységek felhő
A **felhő adatok adatátviteli egység (DMU)** egy mérték, amely jelöli hello (a Processzor, memória és a hálózatierőforrás-lefoglalás kombinációja) adat-előállítóban egyetlen egységben. Egy DMU egy felhő-felhőbe történő másolás művelet, de nem egy hibrid másolás használhatók.

Alapértelmezés szerint a Data Factory használja egy egyetlen felhő DMU tooperform futtatása egyetlen másolási tevékenység. toooverride Ez az alapértelmezett értéket hello **cloudDataMovementUnits** tulajdonság az alábbiak szerint. Egy adott másolási forrását, és a fogadó további egységek konfigurálásakor kaphat jobb teljesítménye hello szintű kapcsolatos információk: hello [teljesítményfigyelési](#performance-reference).

```json
"activities":[  
    {
        "name": "Sample copy activity",
        "description": "",
        "type": "Copy",
        "inputs": [{ "name": "InputDataset" }],
        "outputs": [{ "name": "OutputDataset" }],
        "typeProperties": {
            "source": {
                "type": "BlobSource",
            },
            "sink": {
                "type": "AzureDataLakeStoreSink"
            },
            "cloudDataMovementUnits": 32
        }
    }
]
```
Hello **engedélyezett értékek** a hello **cloudDataMovementUnits** tulajdonság (alapértelmezett) 1, 2, 4, 8, 16, 32. Hello **felhő DMUs tényleges száma** hello konfigurált érték, attól függően, hogy a adatminta-nál kisebb egyenlő tooor, hogy használja-e hello másolási művelet futásidőben.

> [!NOTE]
> Ha további felhőalapú DMUs magasabb átviteli van szüksége, forduljon a [az Azure támogatási](https://azure.microsoft.com/support/). 8 beállítása, a fenti jelenleg működik csak akkor, ha Ön **több fájlok másolását a Blob storage/Data Lake Store/Amazon S3/felhő FTP/felhő SFTP tooBlob tárolási vagy Data Lake Store vagy az Azure SQL-adatbázis**.
>

### <a name="parallelcopies"></a>parallelCopies
Használhatja a hello **parallelCopies** tulajdonság tooindicate hello párhuzamossági, amelyet a másolási tevékenység toouse. Az eltolásokat tekintheti ezt a tulajdonságot, hello belül, amely képes a forrás olvasását tooyour fogadó adattárolókhoz párhuzamosan másolási tevékenység szálak maximális számát.

Minden egyes futtatása másolási tevékenységhez adat-előállító meghatározza, hogy hello száma párhuzamos toouse toocopy adatok hello forrás adattár és toohello adatok céltár másolja. hello alapértelmezett száma párhuzamos, amelyet használ a forrás és a fogadó által használt hello típusától függ.  

| Forrás és a fogadó | Alapértelmezett párhuzamos példányszám szolgáltatás határozza meg |
| --- | --- |
| Adatok másolása közötti fájlalapú tárolók (Blob-tároló; Data Lake Store; Amazon S3; a helyszíni fájlrendszer; egy helyszíni HDFS) |1 és 32. Két felhő adattárolók között használt toocopy adatok hello fájlok és a felhő adatok adatátviteli egység (DMUs) hello száma hello méretétől függ, vagy fizikai konfigurációját hello hello egy hibrid másolás (toocopy adatok tooor ki egy a helyszíni adatok használt átjáró géppel ). |
| Az adatok másolása **forrás adatok tárolásához tooAzure a Table storage** |4 |
| Minden más forrás és a fogadó pár |1 |

Általában hello alapértelmezett viselkedés biztosítani fogja hello legjobb teljesítményt. Azonban toocontrol hello betöltése gépeken üzemeltető az adattároló, vagy tootune másolási teljesítményét, és előfordulhat, hogy válasszon toooverride hello alapértelmezett értéket, és adjon meg egy értéket hello **parallelCopies** tulajdonság. hello érték 1 és 32 (mind a két szélsőértéket beleértve) között kell lennie. Futási időben hello legjobb teljesítmény érdekében másolási tevékenység értéket használ, amely kisebb vagy egyenlő, mint a beállított toohello értékeket.

```json
"activities":[  
    {
        "name": "Sample copy activity",
        "description": "",
        "type": "Copy",
        "inputs": [{ "name": "InputDataset" }],
        "outputs": [{ "name": "OutputDataset" }],
        "typeProperties": {
            "source": {
                "type": "BlobSource",
            },
            "sink": {
                "type": "AzureDataLakeStoreSink"
            },
            "parallelCopies": 8
        }
    }
]
```
Pontok toonote:

* Fájlalapú tárolók közötti adatok másolásakor hello **parallelCopies** hello párhuzamossági hello fájlszintű határozza meg. egyetlen fájlba adattömbösítő hello történne alá, automatikusan és transzparens módon, és úgy van kialakítva, toouse hello legjobb megfelelő adatrészletméretnek egy adott forrás adattároló párhuzamos és merőleges tooparallelCopies tooload adatok írja be. hello tényleges számú hello adatok adatátviteli szolgáltatás által hello másolási művelet futásidőben legfeljebb rendelkezik fájlok hello száma párhuzamos másolatát. Ha hello másolási viselkedése **mergeFile**, másolási tevékenység fájlszintű párhuzamossági tudják kihasználni.
* Hello értékének megadása **parallelCopies** tulajdonság, fontolja meg hello terhelést növelni a forrás és a fogadó adattárolókhoz, toogateway, ha az egy hibrid másolás. Ez akkor fordul elő, különösen ha van több tevékenységek vagy egyidejű frissítési kísérletei során hello azonos elleni futtató tevékenységek hello ugyanazt az adattárat. Ha azt észleli, hogy hello adattárban vagy az átjáró a hello terhelés túlterhelik, csökkentse a hello **parallelCopies** érték toorelieve hello betöltése.
* Amikor adatokat másolni, amelyek nem fájl alapú toostores, amelyek a fájlalapú tárolók, hello adatátviteli szolgáltatás figyelmen kívül hagyja a hello **parallelCopies** tulajdonság. Akkor is, ha a párhuzamos végrehajtás meg van adva, akkor nem lesz alkalmazva ebben az esetben.

> [!NOTE]
> Az adatkezelési átjáró verziója 1.11 vagy újabb toouse hello kell használnia **parallelCopies** a beállítást, ha így tesz, hibrid másolatát.
>
>

toobetter használata e két tulajdonság, és tooenhance a mozgás adatátvitelt, hogy hello [használati esetek minta](#case-study-use-parallel-copy). Nem kell tooconfigure **parallelCopies** hello alapértelmezett viselkedés tootake előnyeit. Ha konfigurálja és **parallelCopies** túl kicsi, több felhőalapú DMUs nem teljes kihasználását.  

### <a name="billing-impact"></a>Számlázási gyakorolt hatás
Rendelkezik **fontos** tooremember van szó, amely alapján a teljes ideje hello hello másolási művelet. Ha egy másolási feladat használt tootake egy órával egy felhőalapú egységhez, és most négy felhő egységek 15 percet vesz igénybe, hello általános számlázási marad szinte hello azonos. Például használhatja négy felhő egység. hello első felhő egység fordít 10 percig, hello másikat, 10 perc hello harmadik egy, 5 perc és hello negyedik egy, 5 perc, mind a futtatásához egy másolási tevékenység. 10 + 10 + 5 + 5 = 30 perc hello teljes másolása (adatátvitel) alkalommal van szó. Használatával **parallelCopies** számlázás nincs hatással.

## <a name="staged-copy"></a>Előkészített másolása
Egy forrás adatokat tároló tooa fogadó adattárból adatok másolásakor ideiglenes átmeneti tárolóként történő toouse Blob-tároló választása. Átmeneti különösen fontos olyan hello a következő esetekben:

1. **Különböző adattárolókhoz tooingest adatait szeretné az SQL Data Warehouse polybase**. Az SQL Data Warehouse PolyBase használja, mint az SQL Data Warehouse nagy átviteli mechanizmus tooload nagy mennyiségű adatot. Azonban hello forrásadatok a Blob Storage tárolóban kell lennie, és további feltételeknek kell megfelelnie. Adatok betöltése a eltérő a Blob storage tárolóban, amikor adatok másolása ideiglenes átmeneti Blob-tároló keresztül aktiválhatja. Ebben az esetben a Data Factory hello szükséges adatok átalakítások tooensure, hogy megfelel-e a PolyBase hello követelményeinek hajt végre. A PolyBase tooload adatok majd az SQL Data Warehouse használ. További részletekért lásd: [használja a PolyBase tooload adatokat az Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse). A használati esetek bemutatóért lásd: [1 TB-os betöltése az Azure SQL Data Warehouse a 15 perc Azure Data Factory](data-factory-load-sql-data-warehouse.md).
2. **Néha egy kis ideig tart egy hibrid adatátvitelt jelölik a (Ez azt jelenti, hogy egy helyszíni adattároló és felhőalapú adattároló közötti toocopy) a lassú hálózati kapcsolaton keresztül tooperform**. tooimprove teljesítmény tömörítheti hello adatok helyben, hogy toomove adatok toohello átmeneti adatok hello felhőben tárolt kevesebb időt vesz igénybe. Majd lehet kibontani a hello adatok hello átmeneti tárolási hello céltár adatok betöltése előtt.
3. **Nem szeretné, hogy tooopen port a 80-as port és a tűzfalon a 443-as porton vállalati informatikai házirendeknek miatt**. Például amikor adatokat másolni egy a helyszíni adatokat tároló tooan Azure SQL Database fogadó vagy egy Azure SQL Data Warehouse fogadó, kell tooactivate kimenő TCP kommunikáció 1433-as portot hello Windows tűzfal és a vállalati tűzfalon. Ebben a forgatókönyvben előnyeit hello toofirst másolási adatok tooa Blob storage átmeneti átjárópéldány HTTP vagy HTTPS a 443-as porton. Ezt követően hello adatok betöltése az SQL Database vagy az SQL Data Warehouse Blob storage átmeneti. Ez a folyamat az 1433-as port tooenable nem szükséges.

### <a name="how-staged-copy-works"></a>Hogyan előkészített másolási működik
A szolgáltatás átmeneti hello aktiválásakor első hello adatok másolásakor hello forrás adatokat tároló toohello átmeneti adattár (kapcsolja a saját). A következő hello adatok másolásakor adatokat tároló toohello fogadó adatokat tároló átmeneti hello. Adat-előállító automatikusan kezeli a hello két szakaszból álló folyamat kidolgozásához. Adat-előállító is törli a szükségtelenné átmeneti tárolási hello adatmozgás befejezése után hello ideiglenes adatait.

Hello felhőbe másolásának esetéhez (a forrás- és fogadó adatok hello felhőben vannak áruházak), az átjáró nem használatos. hello Data Factory szolgáltatásnak hello másolási műveleteket hajtja végre.

![Másolás előkészített: felhős alkalmazás esetében](media/data-factory-copy-activity-performance/staged-copy-cloud-scenario.png)

A hello hibrid másolásának esetéhez (forrása a helyszíni és a fogadó hello felhő), hello átjáró áthelyezi hello forrásadatok adatait tooa adattár átmeneti tárolására. Data Factory szolgáltatásnak helyez át, az adatok átmeneti hello adattároló toohello fogadó adattár. Az adatok másolása a felhőalapú adatokat tároló tooan a helyi átmeneti tárolással végzett adattár is támogatott, fordított irányú hello folyamata.

![Másolás előkészített: hibrid forgatókönyvek](media/data-factory-copy-activity-performance/staged-copy-hybrid-scenario.png)

Aktiválásakor adatmozgás egy átmeneti tároló használatával, megadhatja, hogy kívánja-e adatok tooan ideiglenes vagy adattár átmeneti tárolásához, és majd kibontása előtt az adatok áthelyezése egy ideiglenes hello forrásból származó adatok áthelyezése előtt tömörített hello adatok toobe vagy átmeneti adattárolási toohello fogadó adattár.

Jelenleg nem lehet másolni az adatok között egy átmeneti tárolási használatával két helyszíni adattárolókhoz. Ez a beállítás toobe elérhető várhatóan hamarosan.

### <a name="configuration"></a>Konfiguráció
Hello konfigurálása **enableStaging** a másolási tevékenység toospecify beállítása előtt töltse be a cél-tárolóban a Blob Storage tárolóban előkészített hello adatok toobe kíván-e. Ha **enableStaging** tooTRUE, adja meg a hello hello következő táblázatban felsorolt további tulajdonságok. Ha még nincs fiókja, emellett szükség van egy Azure Storage toocreate, vagy a megosztott aláírás-társított szolgáltatást az átmeneti tárolási.

| Tulajdonság | Leírás | Alapértelmezett érték | Szükséges |
| --- | --- | --- | --- |
| **enableStaging** |Adja meg, hogy átmeneti tárolási ideiglenes toocopy adatokat. |False (Hamis) |Nem |
| **linkedServiceName** |Hello nevét adja meg egy [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service) vagy [AzureStorageSas](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) társított szolgáltatás, amely hivatkozik egy ideiglenes átmeneti tárolóként használó tárolási toohello példányát. <br/><br/> Egy megosztott hozzáférési aláírást tooload adatokat tároló az SQL Data Warehouse polybase nem használható. Más esetekben használható. |N/A |Igen, mikor **enableStaging** tooTRUE van beállítva. |
| **elérési út** |Adja meg a hello Blob. tárolási elérési útja, amelyet az toocontain hello előkészített adatok. Ha nem ad meg egy elérési utat, a hello szolgáltatás a tároló toostore ideiglenes adatokat hoz létre. <br/><br/> Adjon meg egy elérési utat, csak ha tárhelyet használ egy közös hozzáférésű jogosultságkódot, vagy egy adott helyen ideiglenes adatok toobe van szüksége. |N/A |Nem |
| **enableCompression** |Meghatározza, hogy a adatok tömörített másolt toohello cél ahhoz, hogy kell-e. Ez a beállítás hello átvitt adatok mennyiségét csökkenti. |False (Hamis) |Nem |

Hello megelőző táblázatban leírt tulajdonságokkal hello minta másolási tevékenység definíciójának itt található:

```json
"activities":[  
{
    "name": "Sample copy activity",
    "type": "Copy",
    "inputs": [{ "name": "OnpremisesSQLServerInput" }],
    "outputs": [{ "name": "AzureSQLDBOutput" }],
    "typeProperties": {
        "source": {
            "type": "SqlSource",
        },
        "sink": {
            "type": "SqlSink"
        },
        "enableStaging": true,
        "stagingSettings": {
            "linkedServiceName": "MyStagingBlob",
            "path": "stagingcontainer/path",
            "enableCompression": true
        }
    }
}
]
```

### <a name="billing-impact"></a>Számlázási gyakorolt hatás
Van szó, a két lépésből áll: duration másolja, majd másolja át a típust.

* Használatakor a felhőbe történő másolás (az adatok másolása a felhőalapú adatokat tároló tooanother felhőalapú adattároló), közben átmeneti van szó, hello [másolási időtartam 1 és 2. lépést a összege] x [felhő másolási Egységár].
* Használatakor során (az adatok másolása egy a helyszíni adatokat tároló tooa felhőalapú adattároló) hibrid másolatát átmeneti van szó, a [hibrid másolás időtartam] x [hibrid másolás Egységár] + [cloud másolási időtartam] [felhő másolási Egységár] x.

## <a name="performance-tuning-steps"></a>Teljesítmény hangolási lépései
Javasoljuk, hogy ezen lépések tootune hello teljesítményét a Data Factory szolgáltatásnak a másolási tevékenység során:

1. **Meghatározásához**. Hello fejlesztési fázisban tesztelje a feldolgozási sor másolási tevékenység segítségével egy reprezentatív minta alapján. Használhatja a Data Factory hello [modell felosztás](data-factory-scheduling-and-execution.md) toolimit hello adatmennyiség használata.

   Végrehajtási idő és a teljesítményt gyűjtése hello segítségével **figyelés és a felügyeleti alkalmazás**. Válasszon **figyelő & kezelése** a Data Factory kezdőlapon. Hello fanézetben, válassza ki a hello **kimeneti adatkészlet**. A hello **tevékenység Windows** menüben válassza ki a hello másolási tevékenység során futtassa. **Tevékenység Windows** hello másolási tevékenység időtartama és másolt hello adatok hello mérete sorolja fel. hello átviteli szerepel **tevékenység ablak Explorer**. toolearn hello app bővebben lásd: [figyelése és kezelése Azure Data Factory folyamatok használatával hello figyelés és a felügyeleti alkalmazás](data-factory-monitor-manage-app.md).

   ![Tevékenységfuttatás részletei](./media/data-factory-copy-activity-performance/mmapp-activity-run-details.png)

   Hello cikk későbbi részében összehasonlíthatja a hello teljesítmény és a forgatókönyv tooCopy tevékenység által konfigurációjának [teljesítményfigyelési](#performance-reference) a tesztelés.
2. **Diagnosztizálhatja és teljesítményének optimalizálásához**. Ha azt láthatja hello teljesítmény nem felel meg az igényeinek, meg kell tooidentify szűk keresztmetszetek. Ezt követően tooremove teljesítmény optimalizálása, vagy csökkentse a szűk keresztmetszetek hello hatását. A teljesítmény megállapítása teljes leírása, ez a cikk hello terjed, de az alábbiakban néhány gyakori szempontok:

   * Teljesítménnyel kapcsolatos szolgáltatások:
     * [Párhuzamos másolása](#parallel-copy)
     * [A mozgás adategységek felhő](#cloud-data-movement-units)
     * [Előkészített másolása](#staged-copy)
     * [Adatok adatkezelési átjáró méretezhetőség](data-factory-data-management-gateway-high-availability-scalability.md)
   * [Adatkezelési átjáró](#considerations-for-data-management-gateway)
   * [Forrás](#considerations-for-the-source)
   * [A fogadó](#considerations-for-the-sink)
   * [Szerializálás és a deszerializálás](#considerations-for-serialization-and-deserialization)
   * [Tömörítés](#considerations-for-compression)
   * [Oszlop leképezése](#considerations-for-column-mapping)
   * [Egyéb szempontok](#other-considerations)
3. **Bontsa ki a hello konfigurációs tooyour teljes adatkészlet**. Ha elégedett hello végrehajtási eredményt és teljesítményt, bontsa ki a hello definition, és -feldolgozási folyamat aktív időszak toocover a teljes adatkészletet.

## <a name="considerations-for-data-management-gateway"></a>Az adatkezelési átjáró szempontjai
**Átjáró telepítési**: javasoljuk, hogy használjon egy dedikált gép toohost az adatkezelési átjáró. Lásd: [az adatkezelési átjáró használatának szempontjai](data-factory-data-management-gateway.md#considerations-for-using-gateway).  

**Átjáró felügyeleti és felfelé vagy kibővített**: egy vagy több átjáró csomópontokkal egyetlen logikai átjáró ki tud szolgálni több másolási tevékenység fut, hello azonos idő egyidejűleg. Megtekintheti a közel valós idejű pillanatképe erőforrás-használat (Processzor, memória, network(in/out), stb.) egy átjárót működtető gépen, valamint korlát hello Azure-portálon, és fut egyidejűleg futó feladatainak számát hello lásd: a [hello portálonfigyelőátjáró](data-factory-data-management-gateway.md#monitor-gateway-in-the-portal). Ha hibrid adatátvitel nagy számú párhuzamos másolási tevékenység fut, vagy a nagy mennyiségű adatok toocopy nehéz szükség van, érdemes lehet túl[növelheti vagy horizontális felskálázás átjáró](data-factory-data-management-gateway-high-availability-scalability.md#scale-considerations) így toobetter használják az erőforrás vagy másolja a több erőforrás tooempower tooprovision. 

## <a name="considerations-for-hello-source"></a>Hello forrás szempontjai
### <a name="general"></a>Általános kérdések
Győződjön meg arról, hogy az alapul szolgáló adattár hello nem túlterhelik az egyéb munkaterhelések vagy rajta.

Microsoft adattárolókhoz, lásd: [figyelése és beállítása a témakörök](#performance-reference) , amelyek adott toodata tárolja, illetve segítséget megismerte az adatok tárolásához teljesítményt nyújt, a válaszhoz szükséges idő minimalizálása és átviteli sebesség maximalizálása érdekében.

Ha az adatokat másolni a Blob storage tooSQL Data warehouse-ba, fontolja meg **PolyBase** tooboost teljesítményét. Lásd: [használja a PolyBase tooload adatokat az Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) részleteiről. A használati esetek bemutatóért lásd: [1 TB-os betöltése az Azure SQL Data Warehouse a 15 perc Azure Data Factory](data-factory-load-sql-data-warehouse.md).

### <a name="file-based-data-stores"></a>Fájl alapú adattároló
*(A Blob storage, Data Lake Store, Amazon S3, a helyi fájlrendszer és a helyszíni HDFS tartalmazza)*

* **Átlagos méretét és a fájlok száma**: másolási tevékenység egyszerre visz át egy adatfájlt. Az adatok toobe olyan mértékű áthelyezése hello hello teljes átviteli verziója alacsonyabb, ha hello tartalmaz néhány nagy fájlok miatt toohello bootstrap fázis a fájl helyett a sok kisméretű fájlok. Ezért ha lehetséges, kis fájlok egységgé kombinálják nagyobb fájlok toogain nagyobb átviteli sebességgel.
* **Fájl formátuma és tömörítést**: több módon tooimprove teljesítmény, lásd: hello [szempontok a szerializálás és a deszerializálás](#considerations-for-serialization-and-deserialization) és [tömörítés szempontjai](#considerations-for-compression) szakaszok.
* A hello **helyszíni fájlrendszer** forgatókönyv, amelyben **az adatkezelési átjáró** van szükség esetén lásd: hello [az adatkezelési átjáró szempontjai](#considerations-for-data-management-gateway) szakasz.

### <a name="relational-data-stores"></a>Relációs adattároló.
*(Tartalmazza az SQL-adatbázis; Az SQL Data Warehouse; Amazon Redshift; SQL Server-adatbázisok; és Oracle, MySQL, DB2, Teradata, Sybase és PostgreSQL adatbázisok stb.)*

* **Adatminta**: A következő tábla sémáját hatással van a Másolás átviteli sebességet. Nagy sorméret egy kis sorméret jobb teljesítményt biztosít, toocopy hello azonos adatmennyiséget. hello oka, hogy hello az adatbázis hatékonyabban le adatokat, amelyek kevesebb sort tartalmaznak kevesebb kötegekben.
* **Lekérdezés vagy tárolt eljárás**: hello logika hello lekérdezés vagy tárolt eljárás hatékonyabban meg hello másolási tevékenység forrásadatok toofetch optimalizálása.
* A **helyszíni relációs adatbázisok**, például az SQL Server és Oracle, amely kötelező hello használata **az adatkezelési átjáró**, lásd: hello [az adatkezelési átjárószempontjai](#considerations-on-data-management-gateway) szakasz.

## <a name="considerations-for-hello-sink"></a>Hello fogadó szempontjai
### <a name="general"></a>Általános kérdések
Győződjön meg arról, hogy az alapul szolgáló adattár hello nem túlterhelik az egyéb munkaterhelések vagy rajta.

A Microsoft adatokat tárolja, tekintse meg túl[figyelése és beállítása a témakörök](#performance-reference) , amelyek adott toodata tárolja. Ezek a témakörök segítséget az adatok tárolási teljesítményt nyújt, és hogyan toominimize válasz alkalommal fordult elő, és átviteli sebesség maximalizálása érdekében.

Adatok másolása **Blob-tároló** túl**SQL Data Warehouse**, érdemes lehet **PolyBase** tooboost teljesítményét. Lásd: [használja a PolyBase tooload adatokat az Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) részleteiről. A használati esetek bemutatóért lásd: [1 TB-os betöltése az Azure SQL Data Warehouse a 15 perc Azure Data Factory](data-factory-load-sql-data-warehouse.md).

### <a name="file-based-data-stores"></a>Fájl alapú adattároló
*(A Blob storage, Data Lake Store, Amazon S3, a helyi fájlrendszer és a helyszíni HDFS tartalmazza)*

* **Másolja a viselkedés**: adatok másolása egy másik fájlalapú tároló, ha a másolási tevékenység keresztül hello három pontot tartalmaz **copyBehavior** tulajdonság. Megőrzi a hierarchia, hierarchia simítja, illetve egyesíti a fájlokat. Hierarchia egybesimítását vagy megőrzi az rendelkezik kevéssé vagy egyáltalán ne teljesítményigény, de a teljesítmény általános tooincrease fájlok egyesítése okoz.
* **Fájl formátuma és tömörítést**: lásd: hello [szempontok a szerializálás és a deszerializálás](#considerations-for-serialization-and-deserialization) és [tömörítés szempontjai](#considerations-for-compression) szakaszok további módszereket tooimprove teljesítmény .
* **BLOB-tároló**: jelenleg Blob storage támogatja csak blokkblobokat optimalizált adatátvitel és átviteli sebességet.
* A **helyszíni fájlrendszerek** hello használatát igénylő forgatókönyvek **az adatkezelési átjáró**, lásd: hello [az adatkezelési átjáró szempontjai](#considerations-for-data-management-gateway) szakasz.

### <a name="relational-data-stores"></a>Relációs adattároló.
*(Tartalmazza az SQL-adatbázis, az SQL Data Warehouse, az SQL Server-adatbázisok és Oracle-adatbázis)*

* **Másolja a viselkedés**: attól függően, hogy a beállított hello tulajdonságokat **sqlSink**, másolási tevékenység különböző módon írja az adatokat toohello céladatbázisban.
  * Alapértelmezés szerint hello adatok adatátviteli szolgáltatás által hello tömeges másolási API tooinsert adatok hozzáfűzése módját, amely biztosítja a legjobb teljesítmény érdekében hello.
  * Ha konfigurál egy tárolt eljárás hello fogadó, hello adatbázis hello egy adatsorba egyidejűleg ahelyett, hogy a tömeges betöltés vonatkozik. Teljesítmény jelentősen csökken. Ha az adatkészlet túl nagy, ha lehetséges, fontolja meg az áttérést toousing hello **sqlWriterCleanupScript** tulajdonság.
  * Ha konfigurálja hello **sqlWriterCleanupScript** minden másolási tevékenységhez tulajdonság futtatásához, hello szolgáltatás eseményindítók hello parancsfájl és hello tömeges másolási API tooinsert hello adatokat használja majd. Például toooverwrite hello teljes hello legújabb-adatokat tartalmazó táblát, megadhat egy parancsfájl toofirst hello új adatok tömeges-betöltés előtt az összes bejegyzés törlése hello forrásból.
* **Minta és kötegelt adatméret**:
  * A következő tábla sémáját hatással van a Másolás átviteli sebességet. toocopy hello azonos adatmennyiséget, nagy sorméret lehetővé teszi egy kis sorméret jobb teljesítményt, mert hello adatbázis hatékonyabban véglegesítheti az adatok kevesebb kötegek.
  * Másolási tevékenység adatokat egy sorozat része kötegek beszúrása Beállíthatja a sorok számát hello kötegben hello segítségével **writeBatchSize** tulajdonság. Ha az adatok kis sora van, beállíthatja azt a hello **writeBatchSize** tulajdonság egy magasabb érték toobenefit alacsonyabb kötegelt terheléssel jár és nagyobb átviteli sebességgel. Ha az adatok hello sor mérete nagy, ügyeljen arra, amikor növeli **writeBatchSize**. A nagy értékű tooa másolása nem sikerült végrehajtani a túl van terhelve hello adatbázis vezethet.
* A **helyszíni relációs adatbázisok** , például az SQL Server és Oracle, amely kötelező hello használata **az adatkezelési átjáró**, lásd: hello [az adatkezelési átjárószempontjai](#considerations-for-data-management-gateway) szakasz.

### <a name="nosql-stores"></a>NoSQL-tárolókon
*(Tartalmazza a Table storage és Azure Cosmos DB)*

* A **Table storage**:
  * **Partíció**: adatok toointerleaved partíciók jelentősen írása rontja a teljesítményt. Rendezze a forrásadatok partíciós kulcs, így hello adatok bekerülnek hatékonyan egy partíciót egymás után, vagy módosíthatja a hello logika toowrite hello adatok tooa egyetlen partícióra.
* A **Azure Cosmos DB**:
  * **Kötegméret**: hello **writeBatchSize** tulajdonság beállítása hello száma párhuzamos kérelmek toohello Azure Cosmos DB toocreate dokumentumok. Jobb teljesítmény várható növelésével **writeBatchSize** , mert több párhuzamos kérések érkeznek tooAzure Cosmos DB. Azonban nézze meg a sávszélesség-szabályozás tooAzure Cosmos-adatbázis írásakor (hello hibaüzenet a következő: "Ez nagy lekérő"). Számos tényező okozhat a szabályozás, beleértve a dokumentum mérete hello dokumentumok hello számát és hello a célgyűjtemény ügyfélszámítógépein indexelési házirendet. magasabb másolási átviteli tooachieve, érdemes lehet jobban gyűjteménye, például S3.

## <a name="considerations-for-serialization-and-deserialization"></a>Szempontok a szerializálás és a deszerializálás
Szerializálás és a deszerializálás akkor fordulhat elő, ha a bemeneti adatkészlet vagy kimeneti adatkészlet egy fájlt. Lásd: [fájl- és tömörítési formátum támogatott](data-factory-supported-file-and-compression-formats.md) a másolási tevékenység által támogatott fájlformátumok adatokkal.

**Másolja a viselkedés**:

* A fájlok másolása a fájl alapú adattárolók között:
  * Ha a bemeneti és kimeneti adatkészletek egyaránt hello azonos vagy nem fájl formátuma beállításai, hello adatátviteli szolgáltatás hajtja végre a szerializálás vagy deszerializálás nélkül bináris másolatát. Megjelenik egy nagyobb átviteli sebesség képest toohello forgatókönyv, mely hello a forrás és a fogadó fájl formátuma beállítások eltérőek, egymástól.
  * Ha a bemeneti és kimeneti adatkészletek mindkét szöveges formátumú, és csak hello kódolás különböző típusú, csak a teszi kódolási átalakítás hello adatok adatátviteli szolgáltatás. Bármely szerializálási nem, és bizonyos teljesítményt a munkaterhelés deszerializálás tooa bináris másolási képest.
  * Ha bemeneti és kimeneti adatkészletet is rendelkezik különböző fájlformátumok vagy különböző konfigurációkat, például egyes elválasztó karakterek, hello adatátviteli szolgáltatás deserializes forrás adatok toostream, átalakítási és majd szerializálni, jelzett hello kimeneti formátumba. Ez egy sokkal jelentős teljesítménybeli terhet művelet eredményez tooother forgatókönyvek képest.
* A tárolóban, amely nem fájl alapú (például a fájlalapú tároló tooa relációs tároló) vagy a fájlok másolásához hello szerializálás vagy deszerializálás lépés szükség. Ez a lépés jelentős teljesítménybeli terhelést eredményez.

**Fájlformátum**: hello fájlformátum választja hatással lehetnek a fájlmásolás. Például az Avro adatokkal metaadatokat tárol kompakt bináris formátumot. Hello Hadoop ökoszisztémájának feldolgozását, és lekérdezi-e a széles körű támogatást tartalmaz. Azonban az Avro drágább a szerializálás és a deszerializálás, mely eredmények alsó másolási átviteli sebességének képest tootext formátumban. A választott fájlformátum hello holistically feldolgozási folyamat során. Milyen űrlap hello adat indításához adattárolókhoz vagy toobe kibontani a külső rendszerek; hello legjobb formátumát tárolási elemzésfeldolgozási és lekérdezése; és milyen formátumban hello adatok exportálja a jelentéskészítés és a képi megjelenítés eszközök adatpiacait. Egyes esetekben az optimálisnál gyengébb fájl formátuma írási és olvasási teljesítményt lehet hasznos, amikor érdemes hello átfogó analitikai folyamat.

## <a name="considerations-for-compression"></a>Tömörítés szempontjai
Ha a bemeneti vagy kimeneti adatkészlet egy olyan fájl, állíthat másolási tevékenység tooperform tömörítést vagy kitömörítés írja az adatokat toohello cél. Ha úgy dönt, hogy a tömörítés, ellenőrizze-e egy bemeneti/kimeneti (I/O) közötti kompromisszumot és a CPU. Hello az adatok tömörítése a felesleges költségek számítási erőforrásokat. De ismét csökkenti a hálózati i/o- és tárolási. Attól függően, hogy az adatok egy teljes másolatot átviteli sebességének program jelenhet meg.

**Kodek**: másolási tevékenység gzip, bzip2 és Deflate tömörítést típusokat támogatja. Az Azure HDInsight feldolgozásra összes háromféle is felhasználhatnak. Minden egyes tömörítési kodek előnye is van. Például bzip2 hello legalacsonyabb másolási átviteli rendelkezik, de hello legjobb Hive lekérdezés teljesítményt bzip2 beolvasni, mert feldolgozásra felosztása. A gzip legtöbb kiegyensúlyozott hello megoldás, valamint a rendszer általában hello. Válassza ki a végpont forgatókönyvéhez leginkább illő hello kodek.

**Szint**: minden tömörítési kodek két lehetőség közül választhat: leggyorsabb tömörített és optimális tömörített. hello leggyorsabb tömörített tömörítése hello adatok lehető leggyorsabban tegye, még akkor is, ha nem optimális tömörített hello eredményül kapott fájlt. hello optimális tömörített beállítás több időt tölt a tömörítést és adatok mennyisége minimális adja eredményül. Mindkét beállítások toosee abban az esetben, ha jobb összesített teljesítményt biztosító tesztelheti.

**Szempont**: toocopy egy nagy mennyiségű adatot egy helyszíni tárolási és hello felhő között, fontolja meg ideiglenes blob storage használatával, tömörítve történjen. Átmeneti tárolás használatával akkor hasznos, ha a vállalati hálózat és az Azure-szolgáltatások hello sávszélesség korlátozó tényezővé hello, és hello adatkészlet bemeneti és kimeneti adatkészlet mindkét toobe tömörítetlenül. Pontosabban a egyetlen másolási tevékenység során is felosztása két másolási tevékenység. hello első másolási tevékenység másolja át hello forrás tooan ideiglenes vagy átmeneti blob tömörített formátumban. hello második másolási tevékenység tömörített hello adatainak másolása átmeneti, és amíg toohello fogadó ír majd kibontja.

## <a name="considerations-for-column-mapping"></a>Oszlopleképezés szempontjai
Beállíthatja a hello **columnMappings** hello egy részhalmazát vagy a másolási tevékenység toomap összes tulajdonsága bemeneti oszlopok toohello kimeneti oszlopok. Hello adatátviteli szolgáltatás hello forrásból olvassa be az hello adatokat, miután kell tooperform oszlopleképezés hello adatokon előtt hello adatok toohello fogadó ír. A felesleges feldolgozási másolási átviteli csökkenti.

Ha a forrás adattár lekérdezhető, például, ha például az SQL-adatbázis vagy SQL Server relációs áruházbeli, vagy ha például a Table storage vagy Azure Cosmos DB, egy NoSQL-tároló érdemes hello oszlop szűrést és a logikai toohello átrendezése küldését **lekérdezés**  tulajdonság oszlopleképezés használata helyett. Ezzel a módszerrel hello leképezése akkor következik be, amíg hello adatátviteli szolgáltatás rendszer hello forrásadatok adatait tárolja, ahol használata sokkal hatékonyabb.

## <a name="other-considerations"></a>Egyéb szempontok
Ha hello méretezés kívánt toocopy nagy, módosíthatja az üzleti logika toofurther partíció hello adatok hello mechanizmus felosztás adat-előállítóban. Ezt követően ütemezni a másolási tevékenység toorun gyakrabban tooreduce hello adatméret minden másolási tevékenységhez futtassa.

Kell adatkészletek és a Data Factory tooconnector toohello igénylő másolási tevékenység hello száma óvatos ugyanazokat az adatokat tárolja: hello azonos idő. Sok egyidejű másolási feladatokat előfordulhat, hogy szabályozni a tárolóban és toodegraded teljesítmény vezethet, másolja át a feladat belső újrapróbálkozások, és néhány esetben végrehajtása sikertelen.

## <a name="sample-scenario-copy-from-an-on-premises-sql-server-tooblob-storage"></a>Mintaforgatókönyv: egy a helyszíni SQL Server tooBlob tárolási másolását.
**A forgatókönyv**: egy folyamatot egy a helyszíni SQL Server tooBlob tárolási, CSV formátumú toocopy adatait épül. toomake gyorsabban hello másolási feladat, a CSV-fájlok hello bzip2 formátumra kell tömöríthetők.

**Vizsgálati és elemzési**: hello sebességét, a másolási tevékenység érték kisebb, mint 2 MB/s, amely sokkal lassabb, mint hello teljesítmény teljesítményteszt.

**Teljesítményelemzés és hangolása**: tootroubleshoot hello teljesítményprobléma, hogyan hello adatok feldolgozása és áthelyezése vizsgáljuk meg.

1. **Olvassa el az adatok**: átjáró megnyílik egy kapcsolat tooSQL kiszolgáló, és küld hello lekérdezés. SQL Server hello adatok adatfolyam tooGateway hello intraneten keresztül válaszol.
2. **Szerializálható és az adatok tömörítése**: átjáró hello adatformátum adatfolyam tooCSV rendezi sorba, és tömöríti hello tooa bzip2 adatfolyamban.
3. **Adatírás**: átjáró feltölt hello bzip2 adatfolyam tooBlob tárolási hello interneten keresztül.

Ahogy látja, folyamatban van-e a hello adatok feldolgozása és adatfolyam-továbbítási soros módon áthelyezése: SQL Server > LAN > átjáró > WAN > Blob-tároló. **hello összteljesítmény engedi át hello minimális átviteli hello feldolgozási folyamat különböző**.

![Adatfolyam](./media/data-factory-copy-activity-performance/case-study-pic-1.png)

Legalább egy, a következő tényezőket hello okozhat hello teljesítménybeli szűk keresztmetszetek:

* **Forrás**: az SQL Server maga rendelkezik alacsony átviteli túl nagy terhelés miatt.
* **Az adatkezelési átjáró**:
  * **LAN**: átjáró messze hello SQL Server-számítógépen található, és kis sávszélességű kapcsolattal rendelkezik.
  * **Átjáró**: átjáró elérte a terhelés korlátozások tooperform hello a következő műveleteket:
    * **Szerializálási**: hello adatok adatfolyam tooCSV szerializálása formátumhoz lassú átviteli sebességet.
    * **Tömörítés**: úgy döntött, hogy a lassú tömörítési kodek (például bzip2, amely az alapvető i7 2,8 MB/s).
  * **WAN**: hello hello vállalati hálózat és az Azure-szolgáltatások között sávszélessége alacsony (például T1 = 1,544 kbps; T2 = 6,312 kbit/s).
* **Gyűjtése**: a Blob storage alacsony átviteli rendelkezik. (Ez a forgatókönyv nem valószínű, mert az SLA garantálja 60 MB/s legalább.)

Ebben az esetben bzip2 adattömörítés előfordulhat, hogy lehet lelassult hello teljes folyamat. Váltás tooa gzip tömörítési kodek, előfordulhat, hogy a szűk keresztmetszetet megkönnyítése érdekében.

## <a name="sample-scenarios-use-parallel-copy"></a>Lehetséges eset: párhuzamos példányát használhatja az
**A forgatókönyv I:** hello a helyszíni rendszer tooBlob fájltároló 1000 1 MB méretű fájlokat másolni.

**Elemzés és teljesítményhangolás**: példa, ha egy darab négyportos core számítógépen telepített átjárót adat-előállító használja 16 párhuzamos toomove fájlokat másol hello fájltároló rendszer tooBlob egyidejűleg. A párhuzamos végrehajtás nagyobb teljesítményt eredményez. Emellett közvetlenül megadhatja hello párhuzamos másolatok száma. Sok kisméretű fájlok másolásakor párhuzamos másolatok jelentősen segíthet az átviteli sebesség erőforrások hatékonyabb használata.

![1. forgatókönyv](./media/data-factory-copy-activity-performance/scenario-1.png)

**A forgatókönyv II**: 500 MB-os 20 blobok Blob storage tooData Lake Store Analytics másolja, és majd a teljesítmény hangolására.

**Elemzés és teljesítményhangolás**: Ebben a forgatókönyvben a Data Factory hello adatainak másolása a Blob storage tooData Lake Store-ból single-példány használatával (**parallelCopies** too1 beállítása) és egyetlen-felhőbeli adatát adatátviteli egység. hello átviteli erőforrásigények hello ismertetett Bezárás toothat kell lesz [teljesítmény útmutató szakaszban](#performance-reference).   

![2. forgatókönyv](./media/data-factory-copy-activity-performance/scenario-2.png)

**A forgatókönyv III**: egyes fájl mérete nagyobb, mint MB több tucatnyi és a teljes kötet mérete nagy.

**Elemzés és a teljesítmény bekapcsolásáról**: növelése **parallelCopies** nem egy egyetlen-felhő DMU hello erőforrás korlátozásai miatt másolási jobb teljesítményt eredményez. Ehelyett adjon meg további felhőalapú DMUs tooget további erőforrások tooperform hello adatátvitelt jelölik. Ne adjon meg egy értéket hello **parallelCopies** tulajdonság. Adat-előállító hello párhuzamossági az Ön kezeli. Ebben az esetben, ha **cloudDataMovementUnits** too4, hamarosan átviteli négy alkalommal következik be.

![3. forgatókönyv](./media/data-factory-copy-activity-performance/scenario-3.png)

## <a name="reference"></a>Referencia
Az alábbiakban Teljesítményfigyelő és hivatkozások hangolása hello támogatott adattárolókhoz egy részénél:

* Az Azure Storage (beleértve a Blob storage és a Table storage): [Azure Storage méretezhetőségi célok](../storage/common/storage-scalability-targets.md) és [Azure Storage teljesítményére és méretezhetőségére ellenőrzőlista](../storage/common/storage-performance-checklist.md)
* Az Azure SQL Database: Is [hello teljesítmény figyeléséhez](../sql-database/sql-database-single-database-monitor.md) , és ellenőrizze a hello database transaction unit (DTU) százalékos aránya
* Az SQL Data Warehouse: Alkalmasságát mérik adattárházegységek (dwu-k); Lásd: [kezelése számítási teljesítményt az Azure SQL Data Warehouse (áttekintés)](../sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md)
* Az Azure Cosmos DB: [teljesítményszintek az Azure Cosmos-Adatbázisba](../documentdb/documentdb-performance-levels.md)
* A helyszíni SQL Server: [figyelő és a teljesítmény hangolni?](https://msdn.microsoft.com/library/ms189081.aspx)
* A helyi fájlkiszolgáló: [teljesítményhangolás fájlkiszolgálók](https://msdn.microsoft.com/library/dn567661.aspx)
