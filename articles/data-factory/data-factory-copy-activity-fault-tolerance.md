---
title: "aaaAdd hibatűrést az Azure Data Factory másolási tevékenység nem kompatibilis sorok átugrásával |} Microsoft Docs"
description: "Ismerje meg, hogyan tooadd hibatűrés az Azure Data Factory másolási tevékenység során példány nem kompatibilis sorok átugrásával"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jingwang
ms.openlocfilehash: e7cf6117655910844b292d340674d8d631450a81
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-fault-tolerance-in-copy-activity-by-skipping-incompatible-rows"></a>A hibatűrés hozzáadása a másolási tevékenység nem kompatibilis sorok kihagyása

Az Azure Data Factory [másolási tevékenység](data-factory-data-movement-activities.md) kínálja kétféleképpen toohandle nem kompatibilis sorokat forrás és a fogadó adattárolók között adatok másolásakor:

- Megszakítja, és sikertelen hello másolási tevékenység nem kompatibilis adat esetén történt (alapértelmezés).
- Folytatás toocopy hello adatok hozzáadásával a hibatűrést, és nem kompatibilis adat sorok kihagyása. Ezenkívül bejelentkezhet hello nem kompatibilis sorok Azure Blob Storage tárolóban. Majd keresse meg a hello napló toolearn hello hello hiba okát, javítsa ki a hello adatok hello az adatforrás és ismételje meg a hello másolási tevékenység.

## <a name="supported-scenarios"></a>Támogatott helyzetek
Másolási tevékenység észlelésekor, kihagyása és naplózási adatok nem kompatibilis három forgatókönyveket teszi lehetővé:

- **Kompatibilitási hello forrás adattípus és hello fogadó natív típusa között**

    Például: adatok másolása CSV-fájlból a Blob storage tooa SQL-adatbázis egy séma-definícióval, amely tartalmazza a három **INT** típusú oszlop. például numerikus adatokat tartalmazó CSV-fájl sorok hello `123,456,789` sikeresen másolta toohello fogadó tároló. Például a nem numerikus értékeket tartalmazó sorok azonban hello `123,456,abc` észlelhetők a nem kompatibilis, és kimarad.

- **Hello hello forrás- és fogadó hello között oszlopok száma nem egyezik**

    Például: adatok másolása CSV-fájlból a Blob storage tooa SQL adatbázis-egy séma-definícióval, amely hat oszlopokat tartalmaz. hello hat oszlopokat tartalmazó sorokat tartalmazó CSV-fájl sikeresen másolta toohello fogadó tároló. hello CSV fájlt tartalmazó sorok több vagy kevesebb, mint hat oszlopok észlelhetők a nem kompatibilis, és kimarad.

- **Elsődleges kulcs megsértése tooa relációs adatbázis írásakor**

    Például: adatok másolása az SQL server tooa SQL-adatbázis. Hello fogadó SQL adatbázis meg van adva egy elsődleges kulcs, de nincs ilyen elsődleges kulcs hello forrás SQL server van definiálva. Duplikált hello azon sorait, amelyek hello forrás szerepel másolt toohello fogadó nem lehet. Másolási tevékenység csak hello adatok első sora hello forrás hello fogadó másolja. hello hello ismétlődő elsődleges kulcs értéke a következő forrás sorokat észlelhetők a nem kompatibilis és kimarad.

## <a name="configuration"></a>Konfiguráció
hello alábbi példa tartalmazza a JSON definition tooconfigure hello másolási tevékenység nem kompatibilis sorok kihagyása:

```json
"typeProperties": {
    "source": {
        "type": "BlobSource"
    },
    "sink": {
        "type": "SqlSink",
    },         
    "enableSkipIncompatibleRow": true,           
    "redirectIncompatibleRowSettings": {
        "linkedServiceName": "BlobStorage",
        "path": "redirectcontainer/erroroutput"
    }
}
```

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| **enableSkipIncompatibleRow** | Vagy nem engedélyezheti a nem kompatibilis sorok kihagyása másolása során. | True (Igaz)<br/>Hamis (alapértelmezés) | Nem |
| **redirectIncompatibleRowSettings** | Egy csoportját, amely tulajdonságok megadott, ha azt szeretné, hogy toolog hello nem kompatibilis sorok. | &nbsp; | Nem |
| **linkedServiceName** | hello társított szolgáltatásnak Azure Storage toostore hello napló kihagyva hello sorokat tartalmaz-e. | hello nevét egy [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service) vagy [AzureStorageSas](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) társított szolgáltatás, amely a toohello tárolási példányt, amelyet az toouse toostore hello naplófájl. | Nem |
| **elérési út** | hello hello tartalmazó hello naplófájl elérési útja kihagyja a sort. | Adja meg a hello Blob. tárolási elérési útja, amelyet az toouse toolog hello inkompatibilis adatokat. Ha nem ad meg egy elérési utat, hello szolgáltatás létrehoz egy tárolót. | Nem |

## <a name="monitoring"></a>Figyelés
Futtatás hello másolási tevékenység befejezése után hello számát kihagyott sorok hello figyelés szakaszban tekintheti meg:

![A figyelő nem kompatibilis sorok kihagyva](./media/data-factory-copy-activity-fault-tolerance/skip-incompatible-rows-monitoring.png)

Ha toolog hello nem kompatibilis sorok konfigurálásához hello naplófájl ezen az elérési úton található: `https://[your-blob-account].blob.core.windows.net/[path-if-configured]/[copy-activity-run-id]/[auto-generated-GUID].csv` hello naplófájlban is hello azon sorait, amelyek ki lettek hagyva és hello hello kompatibilitási okozza.

Hello fájl hello eredeti adatok és a megfelelő hello hiba jelentkezett be. Hello naplótartalmak fájlt például a következőképpen történik:
```
data1, data2, data3, UserErrorInvalidDataValue,Column 'Prop_2' contains an invalid value 'data3'. Cannot convert 'data3' tootype 'DateTime'.,
data4, data5, data6, Violation of PRIMARY KEY constraint 'PK_tblintstrdatetimewithpk'. Cannot insert duplicate key in object 'dbo.tblintstrdatetimewithpk'. hello duplicate key value is (data4).
```

## <a name="next-steps"></a>Következő lépések
További információ az Azure Data Factory másolási tevékenység során toolearn lásd [adatok áthelyezése a másolási tevékenység segítségével](data-factory-data-movement-activities.md).
