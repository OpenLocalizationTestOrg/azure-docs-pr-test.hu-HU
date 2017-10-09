---
title: "a másolási tevékenység aaaMove adatok |} Microsoft Docs"
description: "További tudnivalók az adat-előállító adatcsatornák adatmozgás: adatáttelepítés felhő tárolja, valamint egy helyi tárolóban és a felhőben levő tárolójának között. Használja a másolási tevékenység."
keywords: "másolja az adatokat, adatmozgás, adatáttelepítés, adatátviteli"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 67543a20-b7d5-4d19-8b5e-af4c1fd7bc75
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jingwang
ms.openlocfilehash: 29b74154b9006795ead3b0ee9638a3dbf2c5d831
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-by-using-copy-activity"></a>Adatok áthelyezése a másolási tevékenység használatával
## <a name="overview"></a>Áttekintés
Az Azure Data Factoryben, használhatja a másolási tevékenység toocopy adatok között a helyszíni és felhőalapú adattároló. Miután hello adatok másolását követően képes lehet további át legyenek-e és elemzése. Másolási tevékenység toopublish átalakítását és az elemzés eredményeinek használhatja az üzleti intelligenciával és alkalmazás-fogyasztás.

![Másolási tevékenység szerepe](media/data-factory-data-movement-activities/copy-activity.png)

Másolási tevékenység működteti a biztonságos, megbízható, méretezhető, és [világszerte elérhető szolgáltatás](#global). Ez a cikk részletesen az adatátvitelt jelölik a Data Factory és a másolási tevékenység.

Első lépésként nézzük meg az adatok áttelepítési módját két felhő adattárolók között, valamint egy helyszíni adattároló és a felhő-tárolóban.

> [!NOTE]
> toolearn kapcsolatos általános, lásd: [ismertetése folyamatok és a tevékenységek](data-factory-create-pipelines.md).
>
>

### <a name="copy-data-between-two-cloud-data-stores"></a>Másolja az adatokat két felhő adattárolók között
Ha a forrás- és fogadó adattárolókhoz hello felhő, a másolási tevékenység végighalad szakaszból toocopy adatok követően – hello forrás toohello fogadó hello. hello szolgáltatás, amely a másolási tevékenység megoldásaira épül:

1. Forrás adattárból hello olvassa be az adatokat.
2. Szerializálással/deszerializálással, tömörítés/kibontás, oszlopleképezés hajt végre, és az adattípus átalakítása. Ezek a műveletek hello konfigurációk hello bemeneti adatkészletet, a kimeneti adatkészlet és a másolási tevékenység alapján végez el.
3. Írja az adatokat toohello céltár adatokat.

hello szolgáltatás automatikusan kiválasztja azt a hello optimális régió tooperform hello adatátvitelt jelölik. Ebben a régióban általában hello egy legközelebbi toohello fogadó adattár.

![Felhő-felhőbe történő másolás](./media/data-factory-data-movement-activities/cloud-to-cloud.png)

### <a name="copy-data-between-an-on-premises-data-store-and-a-cloud-data-store"></a>Egy helyszíni adattároló és felhőalapú adattároló közötti adatok másolása
toosecurely adatait áthelyezni egy helyszíni adattároló és egy felhőalapú adattároló közötti az adatkezelési átjáró telepítéséhez a helyi számítógépen. Az adatkezelési átjáró olyan ügynök, amely lehetővé teszi a hibrid adatmozgatást és feldolgozását. Ugyanaz a gép, hello adattároló magát hello, vagy egy másik számítógépre, amely rendelkezik hozzáférési toohello adatok tárolására telepítheti.

Ebben a forgatókönyvben az adatkezelési átjáró hello szerializálással/deszerializálással, tömörítés/kibontás, oszlopleképezés hajt végre, és adattípus átalakítása. Nem adatfolyam keresztül hello Azure Data Factory szolgáltatásnak. Az adatkezelési átjáró ehelyett közvetlenül hello adatok toohello céltár írási műveletek.

![A helyszíni-felhő másolása](./media/data-factory-data-movement-activities/onprem-to-cloud.png)

Lásd: [adatok áthelyezése között a helyszíni és felhőalapú adattároló](data-factory-move-data-between-onprem-and-cloud.md) bemutatása és forgatókönyv. Lásd: [az adatkezelési átjáró](data-factory-data-management-gateway.md) az ügynök kapcsolatos részletes információkat.

Adatait is áthelyezheti / toosupported adatokat tárolja, amely az adatkezelési átjáró használatával Azure IaaS virtuális gépek (VM) üzemelnek. Ebben az esetben telepíthető az adatkezelési átjáró ugyanazon virtuális gép hello hello adattároló magát, vagy egy külön virtuális gépre, amely rendelkezik hozzáférési toohello adatok tárolásához.

## <a name="supported-data-stores-and-formats"></a>Támogatott adatokat tárolja, és formázza
A Data Factory másolási tevékenység egy forrás adatokat tároló tooa fogadó adatokat tároló másol adatokat. Adat-előállítót a következő adatokat tárolja hello támogatja. Bármely forrásból származó adatok csak írható tooany fogadó. Hogyan kattintson egy adatokat tároló toolearn toocopy adatok tooand adott áruházból.

> [!NOTE] 
> Ha toomove adatok/egy adattárból, amely nem támogatja a másolási tevékenység van szüksége, használja a **egyéni tevékenység** az adatok másolását vagy áthelyezését a saját logikával adat-előállítóban. További információ az egyéni tevékenységek létrehozásával és használatával kapcsolatban: [Egyéni tevékenységek használata Azure Data Factory-folyamatban](data-factory-use-custom-activities.md).

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

> [!NOTE]
> Adatokat tárolja, a * lehet helyszíni vagy Azure IaaS és használatba tooinstall [az adatkezelési átjáró](data-factory-data-management-gateway.md) a-hez vagy az Azure infrastruktúra-szolgáltatási gépen.

### <a name="supported-file-formats"></a>Támogatott fájlformátumok
Másolási tevékenység használhatja túl**másolja a fájlokat-van** között két fájl alapú adattárolókhoz, kihagyhatja a hello [szakasz formázása](data-factory-create-datasets.md) a bemeneti hello mind a dataset definíciókat. hello adatokat hatékonyan másolja a szerializálással/deszerializálással nélkül.

Másolási tevékenység során is beolvassa és toofiles ír megadott formátumok: **szöveg, JSON, Avro, ORC és Parquet**, és a tömörítési kodek **GZip, Deflate, BZip2 és ZipDeflate** támogatottak. Lásd: [fájl- és tömörítési formátum támogatott](data-factory-supported-file-and-compression-formats.md) adatokkal.

Megteheti például hello következő tevékenységek másolása:

* Másolja az adatokat a helyszíni SQL Server, és írási tooAzure Data Lake Store ORC formátumban.
* Szöveges (CSV) formátumú fájlok másolását a helyszíni fájlrendszer, és az Avro formátum tooAzure Blob írja.
* Tömörített fájlok másolását a helyszíni fájlrendszer, és kibontani, majd tooAzure Data Lake Store megnyílik.
* Adatok másolása GZip tömörített szöveges (CSV) formátumú az Azure Blob, és írási tooAzure SQL-adatbázis.

## <a name="global"></a>Globálisan elérhető adatátvitel
Az Azure Data Factory csak hello USA keleti régiója, Észak-Európa, és az USA nyugati régiója, régiók érhető el. Hello szolgáltatást, amely a rendszert működtet a másolási tevékenység azonban hello globálisan elérhető régiók és földrajzi. hello világszerte elérhető topológia hatékony adatmozgás, amely általában a kereszt-régió ugrások nem biztosítja. Lásd: [régiói](https://azure.microsoft.com/regions/#services) a Data Factory és az adatátvitelt jelölik a régióban rendelkezésre állását.

### <a name="copy-data-between-cloud-data-stores"></a>Adatok másolása felhő adattárolók között
Ha a forrás- és fogadó adattárolókhoz hello felhő, adat-előállító szolgáltatástelepítésben használja, amely a hello legközelebbi toohello fogadó hello régióban ugyanazon a földrajzi toomove hello adatokat. Tekintse meg a következő táblázat a leképezés toohello:

| Az adatok céltárolót hello geográfiai | Hello adatok céltár régiója | Adatátvitel használt terület |
|:--- |:--- |:--- |
| Egyesült Államok | USA keleti régiója | USA keleti régiója |
| &nbsp; | USA 2. keleti régiója | USA 2. keleti régiója |
| &nbsp; | USA középső régiója | USA középső régiója |
| &nbsp; | USA északi középső régiója | USA északi középső régiója |
| &nbsp; | USA déli középső régiója | USA déli középső régiója |
| &nbsp; | USA nyugati középső régiója | USA nyugati középső régiója |
| &nbsp; | USA nyugati régiója | USA nyugati régiója |
| &nbsp; | USA nyugati régiója, 2. | USA nyugati régiója |
| Kanada | Kelet-Kanada | Közép-Kanada |
| &nbsp; | Közép-Kanada | Közép-Kanada |
| Brazília | Dél-Brazília | Dél-Brazília |
| Európa | Észak-Európa | Észak-Európa |
| &nbsp; | Nyugat-Európa | Nyugat-Európa |
| Egyesült Királyság | Az Egyesült Királyság nyugati régiója | Az Egyesült Királyság déli régiója |
| &nbsp; | Az Egyesült Királyság déli régiója | Az Egyesült Királyság déli régiója |
| Ázsia és a Csendes-óceáni térség | Délkelet-Ázsia | Délkelet-Ázsia |
| &nbsp; | Kelet-Ázsia | Délkelet-Ázsia |
| Ausztrália | Kelet-Ausztrália | Kelet-Ausztrália |
| &nbsp; | Délkelet-Ausztrália | Délkelet-Ausztrália |
| Japán | Kelet-Japán | Kelet-Japán |
| &nbsp; | Nyugat-Japán | Kelet-Japán |
| India | Közép-India | Közép-India |
| &nbsp; | Nyugat-India | Közép-India |
| &nbsp; | Dél-India | Közép-India |

Azt is megteheti, explicit módon jelezheti adat-előállító szolgáltatás toobe hello területet tooperform hello másolási használt megadásával `executionLocation` tulajdonság a másolási tevékenység `typeProperties`. Ez a tulajdonság támogatott értékek fent felsorolt **régió használt adatmozgás** oszlop. Vegye figyelembe, hogy az adatok végighalad az adott régióban hello hálózaton keresztül másolása során. Például toocopy Azure között tárolja, koreai megadhatja `"executionLocation": "Japan East"` tooroute japán régió keresztül (lásd: [JSON minta](#by-using-json-scripts) hivatkozásként).

> [!NOTE]
> Ha hello régió hello cél adattár nincs a fenti listában, vagy ismeretlen, alapértelmezés szerint másolási tevékenység nem sikerül egy másik régióban tett lépéseket megkerülve kivéve, ha `executionLocation` van megadva. a program hello támogatott régióban lista adott idő alatt bontja ki.
>

### <a name="copy-data-between-an-on-premises-data-store-and-a-cloud-data-store"></a>Egy helyszíni adattároló és felhőalapú adattároló közötti adatok másolása
Amikor adatokat a helyszíni (vagy Azure virtuális gépek/IaaS) közötti másolással és felhőalapú tárolók, [az adatkezelési átjáró](data-factory-data-management-gateway.md) végez adatátvitelt jelölik a helyi számítógépen vagy virtuális gép. hello nem adatfolyam hello szolgáltatás hello felhőben hello használata [másolási előkészített](data-factory-copy-activity-performance.md#staged-copy) funkció. Ebben az esetben adatáramlás keresztül hello Azure Blob Storage tárolóban átmeneti hello fogadó adattárba írás előtt.

## <a name="create-a-pipeline-with-copy-activity"></a>Hozzon létre egy folyamatot másolási tevékenység
A másolási tevékenység több módon is létrehozhat folyamat:

### <a name="by-using-hello-copy-wizard"></a>Hello másolása varázsló használatával
hello Data Factory másolása varázsló segít toocreate egy folyamatot, a másolási tevékenység. Ez az adatcsatorna lehetővé teszi a toocopy adatok támogatott forrásokból toodestinations *JSON írása nélkül* társított szolgáltatások, adathalmazok és adatcsatornák definícióit. Lásd: [Data Factory másolása varázsló](data-factory-copy-wizard.md) hello varázsló vonatkozó további információért.  

### <a name="by-using-json-scripts"></a>JSON-parancsfájlok használatával
Használható Data Factory Editor hello Azure-portálon, a Visual Studio vagy az Azure PowerShell toocreate egy JSON-definícióból egy folyamat (a másolási tevékenység használatával). Ezután telepítheti azt toocreate hello csővezeték adat-előállítóban. Lásd: [oktatóanyag: másolási tevékenység során használja az Azure Data Factory-feldolgozási folyamat](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) oktatóanyagért részletes utasításokat tartalmaz.    

JSON-tulajdonságok (például a nevét, leírását, valamint bemeneti és kimeneti táblák és házirendek) minden típusú tevékenységek érhetők el. Hello elérhető tulajdonságok `typeProperties` hello tevékenység szakasza tevékenységek minden típusának függenek.

A másolási tevékenység során hello `typeProperties` szakasz hello típusú források függ, és a fogadók esetében. Kattintson a forrás/fogadó hello a [támogatott források és mosdók](#supported-data-stores-and-formats) szakasz toolearn kapcsolatban, amely támogatja a másolási tevékenység során, hogy adattároló tulajdonságokat.

Íme egy minta JSON-definícióból:

```json
{
  "name": "ADFTutorialPipeline",
  "properties": {
    "description": "Copy data from Azure blob tooAzure SQL table",
    "activities": [
      {
        "name": "CopyFromBlobToSQL",
        "type": "Copy",
        "inputs": [
          {
            "name": "InputBlobTable"
          }
        ],
        "outputs": [
          {
            "name": "OutputSQLTable"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "SqlSink"
          },
          "executionLocation": "Japan East"          
        },
        "Policy": {
          "concurrency": 1,
          "executionPriorityOrder": "NewestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
    ],
    "start": "2016-07-12T00:00:00Z",
    "end": "2016-07-13T00:00:00Z"
  }
}
```
hello ütemezés hello meghatározott kimeneti adatkészlet meghatározza, hogy mikor fusson a hello tevékenység (például: **napi**, a frekvenciát **nap**, és intervallumhoz **1**). hello tevékenység másol adatokat egy bemeneti adatkészlet (**forrás**) tooan kimeneti adatkészlet (**fogadó**).

Megadhatja, hogy csak egy bemeneti adatkészlet tooCopy tevékenység. Hello tevékenység futása előtt használt tooverify hello függőségei. Azonban csak hello hello első adatkészletből adata másolt toohello céladatkészletből. További információkért lásd: [ütemezés és a végrehajtás](data-factory-scheduling-and-execution.md).  

## <a name="performance-and-tuning"></a>Teljesítmény és finomhangolás
Lásd: hello [másolási tevékenység teljesítmény- és hangolási útmutató](data-factory-copy-activity-performance.md), amely ismerteti az adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory hello teljesítményét befolyásoló legfontosabb tényezők. Belső tesztelése során megfigyelt hello listája és a másolási tevékenység különböző módokon toooptimize hello teljesítményének ismerteti.

## <a name="fault-tolerance"></a>Hibatűrés
Alapértelmezés szerint a másolási tevékenység során leáll másolja az adatokat, és térjen vissza a hiba nem kompatibilis adatok között a forrás és a fogadó; Ha előforduló amíg explicit módon konfigurálhatja tooskip és napló hello nem kompatibilis sort, hogy csak adott kompatibilis toomake hello adatmásolás sikeresen befejeződött. Lásd: hello [másolási tevékenység hibatűrést](data-factory-copy-activity-fault-tolerance.md) a további részletek.

## <a name="security-considerations"></a>Biztonsági szempontok
Lásd: hello [biztonsági szempontok](data-factory-data-movement-security-considerations.md), amely ismerteti, hogy az Azure Data Factory adatátviteli szolgáltatások használjon toosecure az adatok biztonsági infrastruktúra.

## <a name="scheduling-and-sequential-copy"></a>Ütemezési és soros másolása
Lásd: [ütemezés és a végrehajtás](data-factory-scheduling-and-execution.md) adat-előállítóban ütemezés és a végrehajtási működésével kapcsolatos részletes információk. Azt az lehetséges toorun több másolási műveletek egymás után szekvenciális/rendezett módon. Lásd: hello [egymás után másolja](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) szakasz.

## <a name="type-conversions"></a>Típuskonverziók
Különböző adattárolókhoz natív típusa különböző rendszerek rendelkezik. Másolási tevékenység típusok toosink típusát Automatikus típusú konverzió a következő kétlépcsős megközelítést hello hajtja végre:

1. Az átalakítás natív típusok tooa .NET forrástípus.
2. A .NET típusú tooa natív a fogadó típusa konvertálni.

natív típusa rendszer tooa .NET-típus számára egy adattárból hello-leképezés van hello megfelelő adatokat tároló cikkben. (Hello adott hivatkozásra hello [adattárolókhoz támogatott](#supported-data-stores) tábla). A hozzárendelések toodetermine megfelelő típusok használhatók a táblák létrehozása során, úgy, hogy a másolási tevékenység hello jobb átalakítást hajt végre.

## <a name="next-steps"></a>Következő lépések
* toolearn hello másolási tevékenység több, kapcsolatban lásd: [adatok másolása az Azure Blob storage tooAzure SQL-adatbázis](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
* adatok áthelyezése egy a helyszíni adatokat tároló tooa felhő adattárból, toolearn lásd: [tárolt adatok mozgatása a helyszíni toocloud adattárolókhoz](data-factory-move-data-between-onprem-and-cloud.md).
