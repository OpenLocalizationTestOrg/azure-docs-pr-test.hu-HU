---
title: "Azure Batch aaaRun végpont feladatok (előzetes verzió) kód írása nélkül |} Microsoft Docs"
description: "Sablon fájlok hello Azure CLI toocreate kötegelt készletek, a feladatok és a feladatok létrehozását."
services: batch
author: mscurrell
manager: timlt
ms.assetid: 
ms.service: batch
ms.devlang: na
ms.topic: article
ms.workload: big-compute
ms.date: 07/20/2017
ms.author: markscu
ms.openlocfilehash: c0434d09766451f6ba516efbad949834711ee819
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-batch-cli-templates-and-file-transfer-preview"></a>Az Azure Batch parancssori felületi sablonjainak és fájlátviteli funkciójának (előzetes verzió) használata

Hello Azure CLI használata is lehetséges toorun kötegelt feladatok kód írása nélkül.

Sablon fájlok hozható létre és hello Azure parancssori felület, amelyek lehetővé teszik a Batch-készletek, a feladatok és a létrehozott feladatok toobe együtt. Feladat bemeneti fájlok könnyen is feltölthetők a hello fiók és a feladat kimenete parancsfájlokat letöltött társított hello tárfiók.

## <a name="overview"></a>Áttekintés

Egy bővítmény toohello Azure parancssori felület lehetővé teszi, hogy a kötegben használt toobe-végpontok felhasználókat, akik nem a fejlesztők által. Egy címkészlet is létrehozható, bemeneti adatokat feltölteni, feladatok és a kapcsolódó feladatok létrehozása és hello eredményül kapott kimeneti letöltött adatok – nincs szükség esetén kód hello CLI közvetlenül használja, vagy parancsfájlok történő integrálását.

Kötegelt sablonok létrehozása a kiszolgálón hello [hello Azure CLI meglévő kötegelt támogatást](https://docs.microsoft.com/azure/batch/batch-cli-get-started#json-files-for-resource-creation) , amely lehetővé teszi a JSON-fájlok toospecify tulajdonságértékek készletek, feladatok, feladatok és más elemek hello létrehozásához. Kötegelt sablonokkal hello alábbi képességeket kerülnek mi hello JSON-fájlokat tesz lehetővé az keresztül:

-   Paraméterekkel definiálható. Hello sablon használatakor csak hello paraméter értékei megadott toocreate hello elem, más elem tulajdonság értékekkel meghatározott hello sablon törzsében. A felhasználó, aki együttműködik a kötegelt által futtatott kötegelt és az alkalmazások toobe hozhatnak létre, készlet, feladat és egyéb tevékenység tulajdonságértékeket. Kisebb ismerős kötegelt és/vagy az alkalmazásokat a felhasználó csak a meghatározott hello paraméterek kell toospecify hello értékek.

-   Feladat feladat előállítók hozzon létre egyet, vagy egy feladatot, elkerülve a hello társított további feladatokhoz szükséges sok feladat definíciók toobe létrehozott és jelentősen egyszerűsítő feladat elküldése.


Bemeneti adatfájlok kell feladatok megadott toobe és a kimeneti adatok fájlok gyakran. A storage-fiók hozzá van rendelve, alapértelmezés szerint, az egyes kötegekben fiók és a fájlok könnyen átvitt tooand ezt a tárfiókot nem kódolási a CLI használata az lehet, és tárolás szükséges hitelesítő adatok nem szükséges.

Például [ffmpeg](http://ffmpeg.org/) népszerű alkalmazás, amely feldolgozza a hang- és fájlokat. Azure Batch CLI hello használt tooinvoke ffmpeg tootranscode forrás videofájlok toodifferent megoldások is lehet.

-   Egy készlet sablon jön létre. hello sablonok létrehozásának hello felhasználó tudja, hogyan toocall hello ffmpeg alkalmazás és a követelmények vonatkoznak; hello adnia megfelelő operációs rendszer, a virtuális gép mérete, hogyan ffmpeg telepítve (az alkalmazás csomagot vagy a Csomagkezelő például használata), és más tárolókészlet tulajdonság értékével. Paraméterek jönnek létre, ezért hello sablon használatakor csak a hello alkalmazáskészlet-azonosító és a virtuális gépek száma kell-e a megadott toobe.

-   Egy feladat sablon jön létre. hello felhasználói létrehozni hello sablont tudja, hogyan ffmpeg igényeinek toobe meghívott tootranscode forrás videó tooa különböző feloldásához, és adja meg a feladat parancssori hello; tudják is, hogy van-e a mappát, amely tartalmazza a hello forrás videofájlok lejátszását, a bemeneti fájl szükséges feladatokkal.

-   Videofájlok tootranscode állítja be a felhasználó először létrehoz egy alkalmazáskészlet hello készlet sablonnal, csak hello alkalmazáskészlet-azonosító és egyéb szükséges virtuális gépek száma. Hello forrás fájlok tootranscode majd feltöltheti teheti. Egy feladat majd küldheti el hello feladat sablont használja, csak a hello alkalmazáskészlet-azonosító és a feltöltött hello forrásfájljainak helyét. hello kötegelt hozza létre, egy feladathoz egy bemeneti fájl létrehozása folyamatban. Végezetül hello engedélyezi az átalakítását kimeneti fájlok letöltési lehet.

## <a name="installation"></a>Telepítés

hello sablon és a fájl-átviteli lehetőségeket egy bővítmény toobe telepítése szükséges.

Hogyan tooinstall hello Azure CLI: kapcsolatos utasításokat [Azure CLI 2.0 telepítése](https://docs.microsoft.com/cli/azure/install-azure-cli).

Egyszer hello Azure parancssori felület telepítve van, hello kötegelt bővítmény telepítheti a következő parancssori parancsot:

```azurecli
az component update --add batch-extensions --allow-third-party
```

Hello kötegelt bővítmény kapcsolatos további információkért lásd: [Microsoft Azure Batch CLI Extensions for Windows, Mac és Linux](https://github.com/Azure/azure-batch-cli-extensions#microsoft-azure-batch-cli-extensions-for-windows-mac-and-linux).

## <a name="templates"></a>Sablonok

hello Azure Batch parancssori felület lehetővé teszi, hogy az elemek, például készletek, feladatok és feladatok toobe hozta létre a JSON-fájl nevét és értékeit tartalmazó megadása. Példa:

```azurecli
az batch pool create –-json-file AppPool.json
```

Azure Batch-sablonok olyan hasonló tooAzure Resource Manager-sablonok, funkcióit és szintaxist. Azok az elemek nevét és értékeit tartalmazza, de vegye fel a következő fő alapelv hello JSON-fájlok:

-   **Paraméterek**

    -   A tulajdonság értékek toobe csak megadni, ha a sablont hello toobe kellene paraméterértékekkel egy szervezet szakaszában megadott engedélyezése. Például hello teljes definition készlet sikerült elhelyezni a hello törzs és csak az egyik paraméter definiálva a következő alkalmazáskészlet-azonosító; csak egy alkalmazáskészlet azonosító karakterláncot ezért szükséges megadott toobe toocreate készlet.
        
    -   Kötegelt ismerete rendelkező bármely személy hello sablon törzs hozhat létre, és hello alkalmazások toobe futtathatja kötegelt; csak hello szerző által megadott paraméterek értékét meg kell adni, ha hello sablont használ. A felhasználó nélkül hello részletes kötegelt és/vagy alkalmazás Tudásbázis ezért a sablonok használatával.

-   **Változók**

    -   Engedélyezze az egyszerű vagy összetett paraméter értékek toobe egy helyen vannak megadva, és egy vagy több helyen hello sablon törzsében használja. Változók is egyszerűbbé és hello sablon hello méretének csökkentésére, valamint teszik több fenntarthatóvá azzal, hogy egy hely toochange tulajdonságai, amelynek az értéke módosíthatja.

-   **Magasabb szintű szerkezeteket**

    -   Néhány magasabb szintű szerkezeteket hello sablont, amely még nem állnak rendelkezésre a hello kötegelt API-k érhetők el. Például egy feladat gyári adható meg a feladat-sablont, amely használatával a közös feladatdefiníció hello feladat több feladatot hoz létre. A fentiek elkerülése érdekében hello kell toocode dinamikusan hozzon létre több JSON-fájlt, például egy fájl feladat, valamint parancsfájl fájlok tooinstall alkalmazások létrehozása a Csomagkezelő keresztül például.

    -   Néhány pont, ahol alkalmazható, lehet, hogy a fentiek hozzáadott toothe Batch szolgáltatás és a rendelkezésre álló hello kötegelt API-k, UI, stb.

### <a name="pool-templates"></a>Alkalmazáskészlet-sablonok

Ezenkívül toohello standard sablon képességek paraméterek és változók, a következő magasabb szintű szerkezeteket hello hello készlet sablon támogatja:

-   **Csomaghivatkozásokhoz**

    -   Opcionálisan lehetővé teszi a szoftver másolása toobe toopool csomópontok csomag kezelők. hello Csomagkezelő és csomagazonosító meg van adva. Képes toodeclare éppen egy vagy több csomagot elkerülhető hello kell toocreate olyan parancsfájlt, amely lekérdezi a szükséges hello csomagok hello script telepítése és hello parancsprogrammal minden alkalmazáskészlet-csomóponton.

hello az alábbiakban látható egy példa a sablont, amely ffmpeg hoz létre egy Linux virtuális gépek készletét telepítve, és csak számot kell megadni készlet azonosítója karakterlánc és hello a virtuális gépek megadott toobe toouse:

```json
{
    "parameters": {
        "nodeCount": {
            "type": "int",
            "metadata": {
                "description": "hello number of pool nodes"
            }
        },
        "poolId": {
            "type": "string",
            "metadata": {
                "description": "hello pool id "
            }
        }
    },
    "pool": {
        "type": "Microsoft.Batch/batchAccounts/pools",
        "apiVersion": "2016-12-01",
        "properties": {
            "id": "[parameters('poolId')]",
            "virtualMachineConfiguration": {
                "imageReference": {
                    "publisher": "Canonical",
                    "offer": "UbuntuServer",
                    "sku": "16.04.0-LTS",
                    "version": "latest"
                },
                "nodeAgentSKUId": "batch.node.ubuntu 16.04"
            },
            "vmSize": "STANDARD_D3_V2",
            "targetDedicatedNodes": "[parameters('nodeCount')]",
            "enableAutoScale": false,
            "maxTasksPerNode": 1,
            "packageReferences": [
                {
                    "type": "aptPackage",
                    "id": "ffmpeg"
                }
            ]
        }
    }
}
```

Ha hello sablonfájl nevet kapta _alkalmazáskészlet-ffmpeg.json_, majd hello sablon volna hívható meg az alábbiak szerint:

```azurecli
az batch pool create --template pool-ffmpeg.json
```

### <a name="job-templates"></a>Feladatsablonok

Ezenkívül toohello standard sablon képességek paraméterek és változók, a következő magasabb szintű szerkezeteket hello hello feladatsablonhoz támogatja:

-   **A feladat gyári**

    -   Egy feladat meghatározása a feladat több feladat jön létre. Három típusú feladatot gyári támogatja – paraméteres sweep, feladathoz egy fájlt, és tevékenység-gyűjtemény.

hello az alábbiakban látható egy példa a sablont, amely létrehoz egy feladatot, amely egy feladat minden forrás videó fájlban létrehozott ffmpeg átkódolására MP4 videofájlok tooone a két alsó megoldások, hogy használ:

```json
{
    "parameters": {
        "poolId": {
            "type": "string",
            "metadata": {
                "description": "hello name of Azure Batch pool which runs hello job"
            }
        },
        "jobId": {
            "type": "string",
            "metadata": {
                "description": "hello name of Azure Batch job"
            }
        },
        "resolution": {
            "type": "string",
            "defaultValue": "428x240",
            "allowedValues": [
                "428x240",
                "854x480"
            ],
            "metadata": {
                "description": "Target video resolution"
            }
        }
    },
    "job": {
        "type": "Microsoft.Batch/batchAccounts/jobs",
        "apiVersion": "2016-12-01",
        "properties": {
            "id": "[parameters('jobId')]",
            "constraints": {
                "maxWallClockTime": "PT5H",
                "maxTaskRetryCount": 1
            },
            "poolInfo": {
                "poolId": "[parameters('poolId')]"
            },
            "taskFactory": {
                "type": "taskPerFile",
                "source": { 
                    "fileGroup": "ffmpeg-input"
                },
                "repeatTask": {
                    "commandLine": "ffmpeg -i {fileName} -y -s [parameters('resolution')] -strict -2 {fileNameWithoutExtension}_[parameters('resolution')].mp4",
                    "resourceFiles": [
                        {
                            "blobSource": "{url}",
                            "filePath": "{fileName}"
                        }
                    ],
                    "outputFiles": [
                        {
                            "filePattern": "{fileNameWithoutExtension}_[parameters('resolution')].mp4",
                            "destination": {
                                "autoStorage": {
                                    "path": "{fileNameWithoutExtension}_[parameters('resolution')].mp4",
                                    "fileGroup": "ffmpeg-output"
                                }
                            },
                            "uploadOptions": {
                                "uploadCondition": "TaskSuccess"
                            }
                        }
                    ]
                }
            },
            "onAllTasksComplete": "terminatejob"
        }
    }
}
```

Ha hello sablonfájl nevet kapta _feladat-ffmpeg.json_, majd hello sablon volna hívható meg az alábbiak szerint:

```azurecli
az batch job create --template job-ffmpeg.json
```

## <a name="file-groups-and-file-transfer"></a>Fájlcsoportok és fájlátvitel

A legtöbb feladatot és feladatok esetében a bemeneti fájlokat, és a kimeneti fájlok előállításához. Mind a bemeneti fájlokból, és a kimeneti fájlok általában kell áthelyezéshez hello ügyfél toohello csomópont, vagy hello csomópont toohello ügyfél toobe. hello Azure Batch CLI kiterjesztés kötelező fájlátvitel kivonatolja, és használja a hello storage-fiók, amely alapértelmezés szerint minden Batch-fiók jön létre.

Fájlcsoport csatlakozás az Azure storage-fiók hello létrehozott tooa tároló. hello fájlcsoport almappákat is.

hello kötegelt CLI bővítmény parancsokat biztosít tooa megadott fájl ügyfélcsoportból feltöltése fájlok és a fájlok letöltése hello megadott fájl csoport tooa ügyfélről.

```azurecli
az batch file upload --local-path c:\source_videos\*.mp4 
    --file-group ffmpeg-input

az batch file download --file-group ffmpeg-output --local-path
    c:\output_lowres_videos
```

Készlet és a feladat a sablonokkal fájl csoportok toobe megadott készlet csomópontok alakzatot másolási tárolt fájlok, vagy ki készlet csomópontok biztonsági tooa fájlcsoport. Például a korábban megadott feladat sablon hello fájl csoport "ffmpeg-bevitel" beállítása hello feladat beépített hello hello videó forrásfájljainak helyét az átkódolás; hello csomópont másolható hello fájl csoport "ffmpeg-kimeneti" hello hello engedélyezi az átalakítását kimeneti fájlok esetén helyre másolt toofrom hello csomópont minden feladatot használatos.

## <a name="summary"></a>Összefoglalás

Sablon és a fájl adatátviteli támogatása jelenleg váltak elérhetővé, csak az Azure parancssori felület toohello. hello célja tooexpand hello célközönségnek juttathatja el, használhatja a kötegelt toousers, akik nem szükséges toodevelop kód megadása a hello kötegelt API-k, például a kutatók, az informatikusok stb. Nélkül kódolási, az Azure Batch és hello alkalmazások toobe kötegelt által futtatott ismerete rendelkező felhasználók hozhatnak létre a készlet és a feladat létrehozásához. A sablon paraméterekkel kötegelt és hello alkalmazások részletes ismerete nélkül a felhasználók a hello sablonok.

Próbálja ki hello kötegelt kiterjesztése hello Azure CLI és meg kell adnia a visszajelzést vagy a javaslatok, vagy a hello megjegyzésekkel ebben a cikkben vagy hello segítségével [Azure Batch fórum](https://social.msdn.microsoft.com/forums/azure/home?forum=azurebatch).

## <a name="next-steps"></a>Következő lépések

- Hello kötegelt sablonok blogbejegyzésben található: [használó Azure Batch futó feladatok hello Azure CLI-t – nem szükséges kód](https://azure.microsoft.com/en-us/blog/running-azure-batch-jobs-using-the-azure-cli-no-code-required/).
- Részletes telepítési és használati dokumentációt, példák és forráskód találhatók hello [Azure GitHub-tárházban](https://github.com/Azure/azure-batch-cli-extensions).
