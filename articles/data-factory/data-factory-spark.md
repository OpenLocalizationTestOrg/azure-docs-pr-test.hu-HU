---
title: az Azure Data Factory programok aaaInvoke Spark |} Microsoft Docs
description: "Ismerje meg, hogyan tooinvoke Spark programok egy az Azure data factory használatával hello MapReduce művelethez."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: fd98931c-cab5-4d66-97cb-4c947861255c
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: spelluru
ms.openlocfilehash: f88943ece7ee3d21dedbd857609f1b2713b62741
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="invoke-spark-programs-from-azure-data-factory-pipelines"></a>Az Azure Data Factory folyamatok Spark programok meghívása

> [!div class="op_single_selector" title1="Transformation Activities"]
> * [Hive-tevékenység](data-factory-hive-activity.md)
> * [A Pig-tevékenység](data-factory-pig-activity.md)
> * [MapReduce művelethez](data-factory-map-reduce.md)
> * [Hadoop Streamelési tevékenységben](data-factory-hadoop-streaming-activity.md)
> * [A Spark-tevékenység](data-factory-spark.md)
> * [Machine Learning kötegelt végrehajtási tevékenység](data-factory-azure-ml-batch-execution-activity.md)
> * [Machine Learning Update-erőforrástevékenység](data-factory-azure-ml-update-resource-activity.md)
> * [Tárolt eljárási tevékenység](data-factory-stored-proc-activity.md)
> * [Data Lake Analytics U-SQL-tevékenység](data-factory-usql-activity.md)
> * [.NET egyéni tevékenység](data-factory-use-custom-activities.md)

## <a name="introduction"></a>Bevezetés
Spark tevékenység egyike hello [adatok átalakítása tevékenységek](data-factory-data-transformation-activities.md) Azure Data Factory által támogatott. Ez a tevékenység fut megadott hello az Apache Spark-fürt Azure hdinsight Spark programot.    

> [!IMPORTANT]
> - Spark tevékenység nem támogatja a HDInsight Spark-fürtjei használja az Azure Data Lake Store elsődleges tárolására.
> - Spark tevékenység támogatja a csak meglévő HDInsight Spark-fürtjei (saját). Egy igény szerinti HDInsight társított szolgáltatás nem támogatja.

## <a name="walkthrough-create-a-pipeline-with-spark-activity"></a>Forgatókönyv: hozzon létre egy folyamatot Spark tevékenység
Az alábbiakban hello jellemzően előforduló lépéseket toocreate a Data Factory-folyamathoz Spark tevékenységgel.  

1. Adat-előállító létrehozása
2. Hozzon létre egy Azure Storage társított szolgáltatás toolink a az Azure storage társított a HDInsight Spark-fürt toohello adat-előállítóban.     
2. Hozzon létre egy Azure HDInsight kapcsolódó szolgáltatás toolink az Apache Spark-fürt Azure HDInsight toohello adat-előállítóban.
3. Hozzon létre egy adatkészlet hivatkozó toohello Azure tárolás társított szolgáltatása. Jelenleg adjon meg egy kimeneti adatkészlet a tevékenység akkor is, ha nincs folyamatban létrehozott kimenet van.  
4. Hozzon létre egy folyamatot toohello HDInsight társított szolgáltatás létrehozása a #2 hivatkozó Spark-tevékenység. hello konfigurálta, egy kimeneti adatkészlet hello előző lépésben létrehozott hello az adatkészlethez. hello kimeneti adatkészlet milyen meghajtók hello ütemezés (óránként, naponta, stb.). Hello kimeneti adatkészletet, ezért meg kell adnia annak ellenére, hogy hello tevékenység készít valóban kimenettel.

### <a name="prerequisites"></a>Előfeltételek
1. Hozzon létre egy **általános célú Azure Storage-fiók** hello forgatókönyv a következő utasítással: [hozzon létre egy tárfiókot](../storage/common/storage-create-storage-account.md#create-a-storage-account).  
2. Hozzon létre egy **Azure HDInsight az Apache Spark-fürt** hello oktatóanyag utasításai a következő szerint: [Azure HDInsight létrehozása Apache Spark-fürt](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md). Ezzel a fürttel #1. lépésben létrehozott hello Azure storage-fiók társítása.  
3. Töltse le, és tekintse át a python-parancsfájl hello **test.py** helyen található: [https://adftutorialfiles.blob.core.windows.net/sparktutorial/test.py](https://adftutorialfiles.blob.core.windows.net/sparktutorial/test.py).  
3.  Töltse fel **test.py** toohello **pyFiles** hello mappájában **adfspark** az Azure Blob Storage tárolóban. Hello tároló és hello mappa létrehozása, ha azok nem léteznek.

### <a name="create-data-factory"></a>Data factory létrehozása
Kezdjük, az ebben a lépésben hello adat-előállító létrehozása.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).
2. Kattintson a **új** hello bal oldali menüben kattintson **adatok + analitika**, és kattintson a **adat-előállító**.
3. A hello **új adat-előállító** panelen adja meg **SparkDF** a hello nevét.

   > [!IMPORTANT]
   > az Azure data factory hello hello nevének kell lennie **globálisan egyedi**. Ha hello hibát látja: **nem érhető el adat-előállító "SparkDF"**. Hello adat-előállítóban (például yournameSparkDFdate, és próbálja meg újra létrehozni hello nevének módosítása. A Data Factory-összetevők elnevezési szabályait a [Data Factory - Naming Rules](data-factory-naming-rules.md) (Data Factory – Elnevezési szabályok) című témakörben találhatja.   
4. Jelölje be hello **Azure-előfizetés** hello data factory toobe létrehozni kívánt.
5. Válasszon ki egy létező **erőforráscsoport** , vagy hozzon létre egy Azure-erőforráscsoportot.
6. Válassza ki **PIN-kód toodashboard** lehetőséget.  
6. Kattintson a **létrehozása** a hello **új adat-előállító** panelen.

   > [!IMPORTANT]
   > toocreate adat-előállító példányok hello tagjának kell lennie [Data Factory közreműködői](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) szerepkör hello előfizetés-erőforráscsoport szintjén.
7. Hello adat-előállító létrehozása hello látja **irányítópult** a hello Azure-portálon az alábbiak szerint:   
8. Miután hello adat-előállító létrehozása sikerült, oldal akkor jelenik meg hello adatok gyári, amely jelzi, hogy hello hello adat-előállító tartalmát. Ha hello data factory oldalon nem látja, kattintson a hello csempe az irányítópulton hello a data factory.

    ![A Data Factory panel](./media/data-factory-spark/data-factory-blade.png)

### <a name="create-linked-services"></a>Társított szolgáltatások létrehozása
Ebben a lépésben hozzon létre két csatolt szolgáltatást, egy toolink a Spark-fürt tooyour adat-előállító és a hello más toolink az Azure storage tooyour adat-előállítóban.  

#### <a name="create-azure-storage-linked-service"></a>Azure Storage társított szolgáltatás létrehozása
Ebben a lépésben az Azure Storage-fiók tooyour adat-előállító hivatkozásra. A DataSet adatkészlet hoz létre, később a forgatókönyvben egy lépésben toothis kapcsolódó szolgáltatás hivatkozik. hello hello következő lépésben meghatározó kapcsolódó HDInsight-szolgáltatás túl toothis kapcsolódó szolgáltatás hivatkozik.  

1. Kattintson a **Szerző és központi telepítése** a hello **adat-előállító** a data factory paneljét. Hello Data Factory Editor kell megjelennie.
2. Kattintson a **New data store** (Új adattár) elemre, és válassza az **Azure Storage** elemet.

   ![Új adattár – Azure Storage – menü](./media/data-factory-spark/new-data-store-azure-storage-menu.png)
3. Megtekintheti az hello **JSON-parancsfájl** létrehozásához egy Azure Storage társított hello szerkesztőben szolgáltatás.

   ![Azure Storage társított szolgáltatás](./media/data-factory-build-your-first-pipeline-using-editor/azure-storage-linked-service.png)
4. Cserélje le **fióknév** és **fiókkulcs** a hello nevét és hívóbetűjét az Azure-tárfiókot. toolearn hogyan tooget tárhelyét a hozzáférési kulcs, hogyan tooview, másolása és újragenerálása tárolási hívóbetűk a hello adatainak megjelenítéséhez [a tárfiók kezelése](../storage/common/storage-create-storage-account.md#manage-your-storage-account).
5. toodeploy hello társított szolgáltatás, kattintson a **telepítés** hello parancssávon. Hello társított szolgáltatás telepítése után sikeresen hello **vázlat-1** ablak kell eltűnnek, és látni **AzureStorageLinkedService** a hello bal oldali fanézetben hello.

#### <a name="create-hdinsight-linked-service"></a>A HDInsight társított szolgáltatás létrehozása
Ebben a lépésben a HDInsight Spark-fürt toohello data factory kapcsolt Azure HDInsight szolgáltatás toolink hoz létre. hello HDInsight-fürthöz használt toorun hello Spark program hello Spark tevékenység hello folyamatának Ez a példa megadott.  

1. Ha nem látja ezt a gombot, kattintson a három pontot  **További** hello eszköztáron kattintson **új számítási**, és kattintson a **HDInsight-fürt**.

    ![A HDInsight társított szolgáltatás létrehozása](media/data-factory-spark/new-hdinsight-linked-service.png)
2. Másolja és illessze be a következő kódrészletet toohello hello **vázlat-1** ablak. Az hello hello JSON-szerkesztőben, a következő lépéseket:
    1. Adja meg a hello **URI** hello HDInsight Spark fürt. Például: `https://<sparkclustername>.azurehdinsight.net/`.
    2. Adja meg hello hello **felhasználói** kik hozzáférés toohello Spark-fürt.
    3. Adja meg a hello **jelszó** felhasználó számára.
    4. Adja meg a hello **Azure Storage társított szolgáltatásnak** társított hello HDInsight Spark-fürt. Az ebben a példában is: **AzureStorageLinkedService**.

    ```json
    {
        "name": "HDInsightLinkedService",
        "properties": {
            "type": "HDInsight",
            "typeProperties": {
                "clusterUri": "https://<sparkclustername>.azurehdinsight.net/",
                "userName": "admin",
                "password": "**********",
                "linkedServiceName": "AzureStorageLinkedService"
            }
        }
    }
    ```

    > [!IMPORTANT]
    > - Spark tevékenység nem támogatja a HDInsight Spark-fürtjei használja az Azure Data Lake Store elsődleges tárolására.
    > - Spark tevékenység támogatja a csak meglévő HDInsight Spark-fürt (saját). Egy igény szerinti HDInsight társított szolgáltatás nem támogatja.

    Lásd: [HDInsight társított szolgáltatás](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) hello vonatkozó további információért HDInsight társított szolgáltatás.
3.  toodeploy hello társított szolgáltatás, kattintson a **telepítés** hello parancssávon.  

### <a name="create-output-dataset"></a>Kimeneti adatkészlet létrehozása
hello kimeneti adatkészlet milyen meghajtók hello ütemezés (óránként, naponta, stb.). Ezért meg kell adnia egy kimeneti adatkészlet hello spark tevékenység hello folyamat annak ellenére, hogy hello tevékenység valóban nem ad kimenetet. Egy bemeneti adatkészlet hello tevékenység megadása nem kötelező megadni.

1. A hello **Data Factory Editor**, kattintson a **... További** hello parancssávon kattintson **új adatkészlet**, és válassza ki **Azure Blob Storage tárolóban**.  
2. Másolja és illessze be a következő kódrészletet toohello vázlat-1 ablak hello. hello JSON részlet nevű adatkészlet meghatározása **OutputDataset**. Emellett megadhatja, hogy hello eredmények nevű hello blob tárolóban kell tárolni **adfspark** és nevű hello mappát **pyFiles/kimeneti**. Amint azt korábban említettük, ez az adatkészlet egy üres adatkészlet. Ebben a példában hello Spark program nem ad kimenetet. Hello **rendelkezésre állási** szakaszban határozza meg, hogy hello kimeneti adatkészlet naponta jön létre.  

    ```json
    {
        "name": "OutputDataset",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "fileName": "sparkoutput.txt",
                "folderPath": "adfspark/pyFiles/output",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": "\t"
                }
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }
    ```
3. toodeploy hello dataset, kattintson a **telepítés** hello parancssávon.


### <a name="create-pipeline"></a>Folyamat létrehozása
Ebben a lépésben a folyamat létrehoz egy **HDInsightSpark** tevékenység. Kimeneti adatkészlet jelenleg milyen meghajtók hello ütemezését, így még akkor is, ha hello tevékenység nem ad kimenetet kell létrehoznia egy kimeneti adatkészletet. Ha hello tevékenység egyetlen bemeneti nem veszi, kihagyhatja létrehozása hello bemeneti adatkészletet. Ezért bemeneti adatkészlet ebben a példában van megadva.

1. A hello **Data Factory Editor**, kattintson a **... További** hello parancssávon, és kattintson a **új adatcsatorna**.
2. Cserélje le a következő parancsfájl hello hello parancsfájl hello vázlat-1 ablakban:

    ```json
    {
        "name": "SparkPipeline",
        "properties": {
            "activities": [
                {
                    "type": "HDInsightSpark",
                    "typeProperties": {
                        "rootPath": "adfspark\\pyFiles",
                        "entryFilePath": "test.py",
                        "getDebugInfo": "Always"
                    },
                    "outputs": [
                        {
                            "name": "OutputDataset"
                        }
                    ],
                    "name": "MySparkActivity",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2017-02-05T00:00:00Z",
            "end": "2017-02-06T00:00:00Z"
        }
    }
    ```
    Vegye figyelembe a következő pontok hello:
    - Hello **típus** tulajdonsága túl**HDInsightSpark**.
    - Hello **rootPath** értéke túl**adfspark\\pyFiles** ahol adfspark hello Azure Blob-tároló és pyFiles pedig az adott tároló finom mappát. Ebben a példában a hello Azure Blob Storage egy Spark-fürt hello társított hello. Feltöltheti a hello fájl tooa különböző Azure Storage. Ha így tesz, hozzon létre egy Azure Storage társított szolgáltatás toolink adott tárolási fiók toohello adat-előállítóban. Ezt követően adja a hello társított szolgáltatás neve hello hello értékeként **sparkJobLinkedService** tulajdonság. Lásd: [Spark-tevékenység tulajdonságai](#spark-activity-properties) Ez a tulajdonság és az egyéb hello Spark tevékenység által támogatott tulajdonságokról.  
    - Hello **entryFilePath** értéke toohello **test.py**, vagyis hello python-fájl.
    - Hello **getDebugInfo** tulajdonsága túl**mindig**, ami azt jelenti, hogy hello naplófájlok helyét a rendszer mindig generált (sikeres vagy sikertelen).

        > [!IMPORTANT]
        > Azt javasoljuk, hogy nincs megadva ez a tulajdonság túl`Always` éles környezetben kivéve, ha a probléma hibaelhárítást.
    - Hello **kimenete** szakasz tartalmaz egy kimeneti adatkészletet. Meg kell adnia egy kimeneti adatkészletet, még akkor is, ha hello spark program nem ad kimenetet. hello kimeneti adatkészlet meghajtók hello ütemezés hello adatcsatorna (óránként, naponta, stb.).  

        Spark tevékenység által támogatott hello tulajdonságok kapcsolatos részletekért lásd: [tevékenység tulajdonságai Spark](#spark-activity-properties) szakasz.
3. toodeploy hello sorban, kattintson a **telepítés** hello parancssávon.

### <a name="monitor-pipeline"></a>Folyamat figyelése
1. Kattintson a **X** tooclose Data Factory Editor paneleken és toonavigate biztonsági toohello adat-előállító kezdőlapján. Kattintson a **figyelése és kezelése** toolaunch hello alkalmazás egy másik lapon.

    ![Megfigyelés és kezelés csempe](media/data-factory-spark/monitor-and-manage-tile.png)
2. Módosítás hello **kezdési időpont** szűrése hello felső túl**2/1/2017**, és kattintson a **alkalmaz**.
3. Csak egy tevékenység ablakban, csak egy nap közötti hello kezdési (2017-02-01) és befejezési időpontja (2017-02-02) hello folyamatának kell megjelennie. Győződjön meg arról, hogy hello adatszelet a **készen** állapotát.

    ![A figyelő hello folyamat](media/data-factory-spark/monitor-and-manage-app.png)    
4. Jelölje be hello **tevékenység ablakban** hello tevékenységfuttatási toosee részleteit. Ha nem sikerül, akkor vonatkozó részletes azt hello jobb oldali ablaktáblán.

### <a name="verify-hello-results"></a>Hello eredmények ellenőrzése

1. Indítsa el **Jupyter notebook** navigáljon a HDInsight Spark-fürt esetében: https://CLUSTERNAME.azurehdinsight.net/jupyter. Is indítsa el a fürt-irányítópult a HDInsight Spark-fürt számára, majd indítsa el **Jupyter Notebook**.
2. Kattintson a **új** -> **PySpark** toostart egy újat.

    ![Új Jupyter notebook](media/data-factory-spark/jupyter-new-book.png)
3. Futtatási hello parancs következő másolás/beillesztés hello szöveget, majd nyomja le az **SHIFT + ENTER** hello második utasítás hello végén.  

    ```sql
    %%sql

    SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"
    ```
4. Győződjön meg arról, hogy megjelenik-e hello hvac tábla hello adatait:  

    ![Jupyter lekérdezés eredményei](media/data-factory-spark/jupyter-notebook-results.png)

Lásd: [Spark SQL-lekérdezés futtatható](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md#run-a-hive-query-using-spark-sql) szakaszban részletes információkra van szüksége. 

### <a name="troubleshooting"></a>Hibaelhárítás
Óta beállított **getDebugInfo** túl**mindig**, megjelenik egy **napló** hello almappájában **pyFiles** mappa az Azure Blob-tárolóban. hello naplófájl hello napló mappában található további részleteket biztosít. Ez a naplófájl akkor különösen akkor hasznos, ha nem sikerül. Termelési környezetben érdemes lehet a tooset azt túl**hiba**.

Az hello további hibaelhárítási, a következő lépéseket:


1. Keresse meg a túl`https://<CLUSTERNAME>.azurehdinsight.net/yarnui/hn/cluster`.

    ![Alkalmazás a YARN felhasználói felületen](media/data-factory-spark/yarnui-application.png)  
2. Kattintson a **naplók** egy hello futtassa a kísérletet.

    ![Alkalmazások lap](media/data-factory-spark/yarn-applications.png)
3. Meg kell jelennie a hibával kapcsolatos további információkat a hello oldalon.

    ![Naplózási hiba](media/data-factory-spark/yarnui-application-error.png)

a következő szakaszok hello adat-előállító entitások toouse Apache Spark-fürt és a data factory Spark tevékenység kapcsolatos adatok megadása.

## <a name="spark-activity-properties"></a>A Spark-tevékenység tulajdonságai
Hello minta JSON-definícióból Spark tevékenységet adatcsatorna itt található:    

```json
{
    "name": "SparkPipeline",
    "properties": {
        "activities": [
            {
                "type": "HDInsightSpark",
                "typeProperties": {
                    "rootPath": "adfspark\\pyFiles",
                    "entryFilePath": "test.py",
                    "arguments": [ "arg1", "arg2" ],
                    "sparkConfig": {
                        "spark.python.worker.memory": "512m"
                    }
                    "getDebugInfo": "Always"
                },
                "outputs": [
                    {
                        "name": "OutputDataset"
                    }
                ],
                "name": "MySparkActivity",
                "description": "This activity invokes hello Spark program",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2017-02-01T00:00:00Z",
        "end": "2017-02-02T00:00:00Z"
    }
}
```

hello következő táblázat hello JSON-definícióból használt hello JSON-tulajdonságok:

| Tulajdonság | Leírás | Szükséges |
| -------- | ----------- | -------- |
| név | Hello tevékenység hello folyamat nevét. | Igen |
| leírás | Milyen hello tevékenységet leíró szöveg-e. | Nem |
| type | Ez a tulajdonság tooHDInsightSpark kell beállítani. | Igen |
| linkedServiceName | Hello HDInsight neve társított szolgáltatás mely hello Spark a program futtatása. | Igen |
| rootPath | hello Azure blobtároló és hello Spark fájlt tartalmazó mappát. hello fájlnév a kis-és nagybetűket. | Igen |
| entryFilePath | Relatív elérési út toohello gyökérmappájában hello Spark kódcsomag. | Igen |
| Osztálynév | Az alkalmazás Java/Spark fő osztály | Nem |
| Argumentumok | Parancssori argumentumok toohello Spark program listáját. | Nem |
| proxyUser | hello felhasználói fiók tooimpersonate tooexecute hello Spark program | Nem |
| sparkConfig | Adja meg a Spark konfigurációs tulajdonságának hello témakörben felsorolt: [Spark konfigurációs - alkalmazás tulajdonságainak](https://spark.apache.org/docs/latest/configuration.html#available-properties). | Nem |
| getDebugInfo | Itt adhatja meg, amikor hello Spark naplófájlok másolt toohello az Azure storage HDInsight-fürt által használt (vagy) leírt módon sparkJobLinkedService. Megengedett értékek: None, mindig, vagy sikertelen. Alapértelmezett érték: nincs. | Nem |
| sparkJobLinkedService | hello Azure tárolás társított szolgáltatása, amely tárolja a hello Spark feladat fájl, a függőségeket és a naplókat.  Ha nem ad meg egy értéket ehhez a tulajdonsághoz, HDInsight-fürthöz társított hello tárolót használja a rendszer. | Nem |

## <a name="folder-structure"></a>Mappaszerkezet
hello Spark tevékenység nem támogatja a Pig-egy sor parancsprogramot, és Hive tevékenységek tegye. Spark feladatok akkor is több bővíthető, mint a Pig vagy Hive-feladatokat. A Spark-feladatok biztosíthat több függőség például jar (hello java CLASSPATH helyezett) csomagok, python-fájlok (hello PYTHONPATH helyezve) és egyéb fájlokat.

A következő gyökérmappa-szerkezetében lévő hello Azure Blob Storage tárolóban hello kapcsolódó HDInsight-szolgáltatás által hivatkozott hello létrehozása. Ezt követően töltse fel a függő fájlokat toohello megfelelő almappák által képviselt hello gyökérmappában **entryFilePath**. Például feltöltése python fájlok toohello pyFiles almappát, és jar fájlok toohello JAR-fájlok kivételével hello legfelső szintű mappa almappája. Futásidőben a Data Factory szolgáltatásnak vár hello hello Azure Blob Storage tárolót a mappastruktúra a következő:     

| Elérési út | Leírás | Szükséges | Típus |
| ---- | ----------- | -------- | ---- |
| . | hello gyökérútvonalát tekintve hello tárolás társított szolgáltatásának hello Spark feladat    | Igen | Mappa |
| &lt;felhasználó által definiált&gt; | toohello bejegyzés fájl hello Spark feladat rendszerfájlokra hello elérési út | Igen | Fájl |
| . / jars | Ebben a mappában található összes fájl feltöltése és hello java classpath hello fürt helyezve | Nem | Mappa |
| . / pyFiles | Ebben a mappában található összes fájl feltöltése és hello hello fürt PYTHONPATH helyezve | Nem | Mappa |
| . / fájlok | Ebben a mappában található összes fájl feltöltése és végrehajtó munkakönyvtár helyezve | Nem | Mappa |
| . / archiválja | Ebben a mappában található összes fájl tömörítetlen. | Nem | Mappa |
| . / naplói | hello származó Spark-fürt hello naplók tároló mappa.| Nem | Mappa |

Íme egy példa egy tárolási hello Azure Blob Storage HDInsight kapcsolódó szolgáltatás hello által hivatkozott két Spark feladat fájlt tartalmazza.

```
SparkJob1
    main.jar
    files
        input1.txt
        input2.txt
    jars
        package1.jar
        package2.jar
    logs

SparkJob2
    main.py
    pyFiles
        scrip1.py
        script2.py
    logs
```
