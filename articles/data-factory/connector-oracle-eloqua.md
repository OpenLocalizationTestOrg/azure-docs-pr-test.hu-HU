---
title: "Adatok másolása az Azure Data Factory (béta) használatával Oracle Eloqua |} Microsoft Docs"
description: "Útmutató: adatok másolása az Oracle Eloqua támogatott fogadó adattárolókhoz egy Azure Data Factory-folyamat a másolási tevékenység használatával."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/30/2017
ms.author: jingwang
ms.openlocfilehash: a389f4be625dd301b7210000555d71018b4cdec8
ms.sourcegitcommit: c4cc4d76932b059f8c2657081577412e8f405478
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/11/2018
---
# <a name="copy-data-from-oracle-eloqua-using-azure-data-factory-beta"></a>Adatok másolása az Azure Data Factory (béta) használatával Oracle Eloqua

Ez a cikk ismerteti, hogyan használható a másolási tevékenység során az Azure Data Factory adatok másolása az Oracle Eloqua. Buildekről nyújtanak a [másolása tevékenység áttekintése](copy-activity-overview.md) cikket, amely megadja a másolási tevékenység általános áttekintést.

> [!NOTE]
> Ez a cikk a Data Factory 2. verziójára vonatkozik, amely jelenleg előzetes verzióban érhető el. A Data Factory szolgáltatásnak, amely általánosan elérhető (GA), 1 verziójának használatakor lásd [másolási tevékenység során a V1](v1/data-factory-data-movement-activities.md).

> [!IMPORTANT]
> Ez az összekötő jelenleg bétaverziójú. Próbálja ki, és visszajelzést. Ne használja éles környezetben.

## <a name="supported-capabilities"></a>Támogatott képességei

Oracle Eloqua adatok bármely támogatott fogadó adattárolóhoz másolhatja. Adattároló források/mosdók, a másolási tevékenység által támogatott listájáért lásd: a [adattárolókhoz támogatott](copy-activity-overview.md#supported-data-stores-and-formats) tábla.

Az Azure Data Factory kapcsolódásának engedélyezése beépített illesztőprogramot tartalmaz, ezért nem szükséges manuálisan kell telepítenie minden olyan illesztőprogram ezt az összekötőt használja.

## <a name="getting-started"></a>Első lépések

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

A következő szakaszok részletesen bemutatják való Oracle Eloqua összekötő adat-előállító tartozó entitások meghatározásához használt tulajdonságokat.

## <a name="linked-service-properties"></a>A kapcsolódószolgáltatás-tulajdonságok

Oracle Eloqua kapcsolódó szolgáltatás támogatott a következő tulajdonságokkal:

| Tulajdonság | Leírás | Szükséges |
|:--- |:--- |:--- |
| type | A type tulajdonságot kell beállítani: **Eloqua** | Igen |
| endpoint | A végpont a Eloqua kiszolgáló. (Ez azt jelenti, hogy eloqua.example.com)  | Igen |
| felhasználónév | A hely nevét és a képernyőn a Eloqua fiók felhasználóneve: hely neve vagy felhasználó neve. (Ez azt jelenti, hogy Eloqua/Anna)  | Igen |
| jelszó | A jelszó, a felhasználónév megfelelő. Ez a mező megjelölése a SecureString tárolja biztonságos helyen az ADF, vagy a jelszó tárolása az Azure Key Vault választhat, és lehetővé teszik a másolási tevékenység lekéréses ott adatmásolás végrehajtása során – további információhoz [hitelesítő adatok tárolása a Key Vault](store-credentials-in-key-vault.md). | Igen |
| useEncryptedEndpoints | Meghatározza, hogy a data source végpontok titkosítása HTTPS használatával. Az alapértelmezett érték: igaz.  | Nem |
| useHostVerification | Meghatározza, hogy az a számítógép neve a egyeznie kell a gazdagép nevével a kiszolgáló SSL-en keresztüli kapcsolódás esetén a kiszolgáló tanúsítványát. Az alapértelmezett érték: igaz.  | Nem |
| usePeerVerification | Megadja, hogy a kiszolgáló identitásának ellenőrzése SSL-en keresztüli kapcsolódás esetén. Az alapértelmezett érték: igaz.  | Nem |

**Példa**

```json
{
    "name": "EloquaLinkedService",
    "properties": {
        "type": "Eloqua",
        "typeProperties": {
            "endpoint" : "eloqua.example.com",
            "username" : "Eloqua/Alice",
            "password": {
                 "type": "SecureString",
                 "value": "<password>"
            }
        }
    }
}
```

## <a name="dataset-properties"></a>Adatkészlet tulajdonságai

Szakaszok és meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: a [adatkészletek](concepts-datasets-linked-services.md) cikk. Ez a témakör az Oracle Eloqua dataset által támogatott tulajdonságokról.

Adatok másolása az Oracle Eloqua, állítsa be a type tulajdonságot az adathalmaz **EloquaObject**. Nincs ilyen típusú dataset további típusra vonatkozó tulajdonság.

**Példa**

```json
{
    "name": "EloquaDataset",
    "properties": {
        "type": "EloquaObject",
        "linkedServiceName": {
            "referenceName": "<Eloqua linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Másolási tevékenység tulajdonságai

Szakaszok és a rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: a [folyamatok](concepts-pipelines-activities.md) cikk. Ez a témakör az Oracle Eloqua forrás által támogatott tulajdonságokról.

### <a name="eloquasource-as-source"></a>EloquaSource forrásaként

Adatok másolása Oracle Eloqua, állítsa be a forrás típusa a másolási tevékenység **EloquaSource**. A következő tulajdonságok támogatottak a másolási tevékenység **forrás** szakasz:

| Tulajdonság | Leírás | Szükséges |
|:--- |:--- |:--- |
| type | A type tulajdonságot a másolási tevékenység forrás értékre kell állítani: **EloquaSource** | Igen |
| lekérdezés | Az egyéni SQL-lekérdezés segítségével adatokat olvasni. Például: `"SELECT * FROM Accounts"`. | Igen |

**Példa**

```json
"activities":[
    {
        "name": "CopyFromEloqua",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Eloqua input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "EloquaSource",
                "query": "SELECT * FROM Accounts"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="next-steps"></a>További lépések
Támogatott Azure Data Factory által tárolt adatok listájáért lásd: [adattárolókhoz támogatott](copy-activity-overview.md#supported-data-stores-and-formats).
