---
title: "Azure Data Factory által támogatott aaaCompute környezetben |} Microsoft Docs"
description: "További információk a számítási környezetekben, amelyek az Azure Data Factory folyamatok (például az Azure HDInsight) tootransform vagy a folyamat."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 6877a7e8-1a58-4cfb-bbd3-252ac72e4145
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 07/25/2017
ms.author: shlo
ms.openlocfilehash: aba7d7de695bc1c7d475f1e741ee3b3e884151c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="compute-environments-supported-by-azure-data-factory"></a>Számítási környezetek Azure Data Factory által támogatott
Ez a cikk ismerteti, hogy tooprocess használhatja, vagy adatok különböző számítási környezeteket. Adat-előállító támogatott, ha a számítási környezetek tooan az Azure data factory linking összekapcsolt szolgáltatások konfigurálása (igény szerinti és kapcsolja a saját) különböző konfigurációkkal kapcsolatos adatokat is tartalmazza.

hello következő táblázat felsorolja a Data Factory és hello tevékenységek ezeken futó által támogatott számítási környezetek. 

| Számítási környezet | tevékenységek |
| --- | --- |
| [Igény szerinti HDInsight-fürt](#azure-hdinsight-on-demand-linked-service) vagy [saját HDInsight-fürt](#azure-hdinsight-linked-service) |[DotNet](data-factory-use-custom-activities.md), [Hive](data-factory-hive-activity.md), [Pig](data-factory-pig-activity.md), [MapReduce](data-factory-map-reduce.md), [Hadoop Streamelési](data-factory-hadoop-streaming-activity.md) |
| [Az Azure Batch](#azure-batch-linked-service) |[DotNet](data-factory-use-custom-activities.md) |
| [Azure Machine Learning](#azure-machine-learning-linked-service) |[Machine Learning-tevékenységek: kötegelt végrehajtás és erőforrás frissítése](data-factory-azure-ml-batch-execution-activity.md) |
| [Az Azure Data Lake Analytics](#azure-data-lake-analytics-linked-service) |[Data Lake Analytics U-SQL](data-factory-usql-activity.md) |
| [Az Azure SQL](#azure-sql-linked-service), [Azure SQL Data Warehouse](#azure-sql-data-warehouse-linked-service), [SQL Server](#sql-server-linked-service) |[Tárolt eljárás](data-factory-stored-proc-activity.md) |

## <a name="supported-hdinsight-versions-in-azure-data-factory"></a>Azure Data Factory-HDInsight támogatott verziói
Az Azure HDInsight Hadoop fürt több verziója, amely bármikor telepíthető támogatja. Minden egyes verzió choice hoz létre, egy adott verziójához hello Hortonworks Data Platform (HDP) telepítési és összetevők belüli, hogy a terjesztési. A Microsoft hello listája HDInsight tooprovide legújabb Hadoop ökoszisztémájának összetevőit és javításokat támogatott verziói tartja a frissítése. a HDInsight 3.2 hello 2017. április 1. az elavult. Részletes információkért lásd: [HDInsight-verziókról támogatott](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions).

Ez hatással van a meglévő Azure adat-előállítók, amelyek rendelkeznek a HDInsight 3.2 fürtök futtatott tevékenységeket. Azt javasoljuk, hogy a következő szakasz tooupdate hello felhasználók toofollow hello irányelveinek hello érintett adat-előállítók:

### <a name="for-linked-services-pointing-tooyour-own-hdinsight-clusters"></a>Az összekapcsolt szolgáltatások mutató tooyour saját HDInsight-fürtök
* **HDInsight összekapcsolt szolgáltatások tooyour mutató saját a HDInsight 3.2-es vagy annak szintje alatt a fürtök:**

  Az Azure Data Factory támogatja a saját HDInsight-fürtök a HDI 3.1 túl küldő feladatok tooyour[legújabb támogatott hello HDInsight-verzió](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions). Azonban már nem után hozhat létre HDInsight 3.2 fürt részletes ismertetését lásd: hello érvénytelenítése házirend alapján 2017. április 1 [HDInsight-verziókról támogatott](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions).  

  **Javaslatok:** 
  * Hajtsa végre a tesztek tooensure hello kompatibilitásának hello túl a társított szolgáltatások hivatkozó tevékenységek[legújabb támogatott hello HDInsight-verzió](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions) dokumentált információkat [érhető el a Hadoop-összetevők különböző HDInsight-verziókról](../hdinsight/hdinsight-component-versioning.md#hadoop-components-available-with-different-hdinsight-versions) és [Hortonworks kibocsátási megjegyzéseket HDInsight-verziókról társított](../hdinsight/hdinsight-component-versioning.md#hortonworks-release-notes-associated-with-hdinsight-versions).
  * A HDInsight 3.2 fürt frissítése túl[legújabb támogatott hello HDInsight-verzió](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions) tooget hello legújabb Hadoop-ökoszisztémával összetevők és javításokat. 

* **HDInsight összekapcsolt szolgáltatások tooyour mutató saját HDInsight, 3.3-as verziójának vagy a fenti fürtök:**

  Az Azure Data Factory támogatja a saját HDInsight-fürtök a HDI 3.1 túl küldő feladatok tooyour[legújabb támogatott hello HDInsight-verzió](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions). 
  
  **Javaslatok:** 
  * Semmilyen műveletet nem kell a Data Factory szempontjából. Azonban ha a HDInsight egy korábbi verziója, mégis azt javasoljuk túl frissítése[legújabb támogatott hello HDInsight-verzió](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions) tooget hello legújabb Hadoop-ökoszisztémával összetevők és javításokat.

### <a name="for-hdinsight-on-demand-linked-services"></a>A HDInsight-igény szerinti kapcsolódó szolgáltatások
* **3.2-es verziója vagy az alábbiakban megadott HDInsight igény társított szolgáltatás JSON-definícióból:**
  
  Az Azure Data Factory fogja támogatni a 3.3-as verziójú vagy annál nagyobb az igény szerinti HDInsight-fürtök létrehozása **05/15/2017** és újabb verziók esetében. És összekapcsolt szolgáltatások túl ki van bővítve meglévő igény szerinti HDInsight 3.2 hello végéhez**07/15/2017**.  

  **Javaslatok:** 
  * Hajtsa végre a tesztek tooensure hello kompatibilitásának hello túl a társított szolgáltatások hivatkozó tevékenységek [legújabb támogatott hello HDInsight-verzió](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions) dokumentált információkat [érhető el a Hadoop-összetevők különböző HDInsight-verziókról](../hdinsight/hdinsight-component-versioning.md#hadoop-components-available-with-different-hdinsight-versions) és [Hortonworks kibocsátási megjegyzéseket HDInsight-verziókról társított](../hdinsight/hdinsight-component-versioning.md#hortonworks-release-notes-associated-with-hdinsight-versions).
  * Mielőtt **07/15/2017**, hello Version tulajdonság a igény szerinti HDI társított szolgáltatás JSON-definícióból túl frissítése[legújabb támogatott hello HDInsight-verzió](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions) tooget hello legújabb Hadoop ökoszisztémájának összetevőit és javítások. Részletes JSON-definícióból, tekintse meg a toohello [Azure HDInsight igény társított szolgáltatás minta](#azure-hdinsight-on-demand-linked-service). 

* **A verzió nincs megadva a igény szerinti HDInsight összekapcsolt szolgáltatások:**
  
  Az Azure Data Factory fogja támogatni a 3.3-as verziójú vagy annál nagyobb az igény szerinti HDInsight-fürtök létrehozása **05/15/2017** és újabb verziók esetében. És támogatási tooexisting igény szerinti HDInsight 3.2 kapcsolódó szolgáltatások hello végét ki van bővítve túl**07/15/2017**. 

  Mielőtt **07/15/2017**, ha üresen hagyja a mezőt, hello alapértelmezett értékei verziót, és osType tulajdonságai vannak: 

  | Tulajdonság | Alapértelmezett érték | Szükséges |
  | --- | --- | --- |
  Verzió   | A Windows-fürt és a Linux-fürt HDI 3.2 HDI 3.1-et.| Nem
  osType | hello alapértelmezett érték Windows | Nem

  Miután **07/15/2017**, ha üresen hagyja a mezőt, hello alapértelmezett értékei verziót, és osType tulajdonságai vannak:

  | Tulajdonság | Alapértelmezett érték | Szükséges |
  | --- | --- | --- |
  Verzió   | A Windows-fürt és a Linux-fürt 3.5 HDI 3.3-as.    | Nem
  osType | hello alapértelmezett érték a Linux   | Nem

  **Javaslatok:** 
  * Előtt **07/15/2017**, hajtsa végre a tesztek tooensure hello kompatibilitásának hello túl a társított szolgáltatások hivatkozó tevékenységek[legújabb támogatott hello HDInsight-verzió](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions) dokumentált információkat a [HDInsight különböző verzióiban Hadoop-összetevők](../hdinsight/hdinsight-component-versioning.md#hadoop-components-available-with-different-hdinsight-versions) és [Hortonworks kibocsátási megjegyzéseket HDInsight-verziókról társított](../hdinsight/hdinsight-component-versioning.md#hortonworks-release-notes-associated-with-hdinsight-versions).  
  * Miután **07/15/2017**, győződjön meg arról, ha azt szeretné, hogy toooverride hello alapértelmezett beállítások kifejezetten megad osType és verzió értékét. 

>[!Note]
>Azure Data Factory jelenleg nem támogatja a HDInsight-fürtök az Azure Data Lake Store elsődleges tárolóként. Azure Storage használják elsődleges tár a HDInsight-fürtök. 
>  
>  

## <a name="on-demand-compute-environment"></a>Igény szerinti számítási környezet
Az ilyen típusú konfigurációs hello számítási környezet teljesen hello Azure Data Factory szolgáltatás kezeli. Ez automatikusan hello Data Factory szolgáltatásnak által létrehozott, mielőtt egy feladatot az elküldött tooprocess adatai és távolítja el, amikor hello feladat befejezését. Csatolt szolgáltatást hello igény szerinti számítási környezetet hozhat létre, konfigurálja és feladat végrehajtási, a kiszolgálófürt-felügyelet és a műveletek rendszerindítása részletes beállításait.

> [!NOTE]
> hello igény konfigurációs jelenleg csak az Azure HDInsight-fürtök támogatott.
> 
> 

## <a name="azure-hdinsight-on-demand-linked-service"></a>Az Azure HDInsight igény társított szolgáltatás
hello Azure Data Factory szolgáltatásnak automatikusan hozhat létre egy Windows/Linux-alapú igény szerinti HDInsight fürt tooprocess adatok. hello fürt hello és hello (hello JSON linkedServiceName tulajdonsága) storage-fiók ugyanabban a régióban társított hello fürt jön létre. hello tárfiók egy általános célú standard Azure-tárfiókot kell lennie. 

Vegye figyelembe a következőket hello **fontos** kapcsolatos igény szerinti HDInsight pontok társított szolgáltatás:

* Nem látja hello igény szerinti HDInsight-fürt létrehozása az Azure-előfizetéshez. Azure Data Factory szolgáltatásnak hello hello igény szerinti HDInsight-fürthöz az Ön nevében kezeli.
* hello naplók az igény szerinti HDInsight fürt vannak a futó feladatok másolással hello HDInsight-fürthöz társított toohello tárfiók. Ezek a naplók elérhető hello Azure portálra az hello **tevékenység futtatása részletei** panelen. Lásd: [figyelő és folyamatok kezelése](data-factory-monitor-manage-pipelines.md) cikkben alább.
* Van szó, csak a hello időpontot, amikor hello HDInsight-fürt fel és futó feladatot.

> [!IMPORTANT]
> Általában tart **20 perc** vagy a további tooprovision egy Azure HDInsight fürt igény szerint.
> 
> 

### <a name="example"></a>Példa
a következő JSON hello igény kapcsolódó HDInsight Linux-alapú szolgáltatás határozza meg. hello Data Factory szolgáltatásnak automatikusan létrehoz egy **Linux-alapú** HDInsight-fürt adatok szelet feldolgozása közben. 

```json
{
    "name": "HDInsightOnDemandLinkedService",
    "properties": {
        "type": "HDInsightOnDemand",
        "typeProperties": {
            "version": "3.5",
            "clusterSize": 1,
            "timeToLive": "00:05:00",
            "osType": "Linux",
            "linkedServiceName": "AzureStorageLinkedService"
        }
    }
}
```

egy Windows-alapú HDInsight-fürt toouse beállítása **osType** túl**windows** , vagy ne használjon hello tulajdonság, mert hello alapértelmezett érték: windows.  

> [!IMPORTANT]
> hello HDInsight-fürtöt hoz létre egy **alapértelmezett tároló** az Ön által megadott hello JSON hello blob storage (**linkedServiceName**). HDInsight nem törli a tárolót hello fürt törlésekor. Ez a működésmód szándékos. Az igény szerinti HDInsight kapcsolódó szolgáltatás használata esetén a HDInsight-fürt létrehozása minden alkalommal, amikor a szelet kell feldolgozni, kivéve, ha egy meglévő élő fürthöz toobe (**timeToLive**), és törlődik, amikor hello feldolgozási hajtja végre. 
> 
> Ahogy egyre több szelet lesz feldolgozva, egyre több tároló jelenik meg az Azure Blob Storage-tárban. Ha nem kell őket hello feladatok hibaelhárítási, érdemes lehet a toodelete őket tooreduce hello tárolási költségeket. ezekhez a tárolókhoz hello nevei, hajtsa végre a minta: `adf**yourdatafactoryname**-**linkedservicename**-datetimestamp`. Használjon például az eszközök [Microsoft Tártallózó](http://storageexplorer.com/) toodelete tárolókat az az Azure blob-tároló.
> 
> 

### <a name="properties"></a>Tulajdonságok
| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| type |hello típusú tulajdonságot kell beállítani, túl**HDInsightOnDemand**. |Igen |
| Nagyobbnak |Hello fürt/adatok csomópontok száma. Ez a tulajdonság a megadott munkavégző csomópontokhoz hello száma együtt 2 átjárócsomópontokkal hello HDInsight-fürt jön létre. hello csomópontra van, amely nem rendelkezik 4 mag, így egy 4 munkavégző csomópontot tartalmazó fürtben veszi 24 mag standard, D3 méretű (4\*a munkavégző csomópontokról, valamint 2 processzormag, 4 = 16\*az átjárócsomópontokkal processzormag, 4 = 8). Lásd: [hdinsight létrehozása Linux-alapú Hadoop-fürtök](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md) hello standard, D3 réteg vonatkozó további információért. |Igen |
| a TimeToLive tulajdonság |hello megengedett üresjárati idő hello igény szerinti HDInsight-fürthöz. Meghatározza, mennyi ideig hello igény szerinti HDInsight-fürt aktív marad a művelet végrehajtása, ha nincsenek más aktív feladatok hello fürt tevékenység befejezését követően.<br/><br/>Például ha egy tevékenységfuttatási fogad 6 percnél és timetolive nem beállítása too5 perc, hello fürt marad a 5 perc után hello életben hello tevékenységfuttatási feldolgozásának 6 percnél. Ha egy másik tevékenységfuttatási hello 6-perc ablak, hello által feldolgozott ugyanabban a fürtben.<br/><br/>Igény szerinti HDInsight-fürtök létrehozása során költséges (igénybe vehet), így használják ezt a beállítást egy adat-előállító szükséges tooimprove teljesítményének újból felhasználja az igény szerinti HDInsight-fürtök által.<br/><br/>Ha a TimeToLive tulajdonság értékének too0, hello-fürtök törlése, amint hello tevékenységfuttatási befejeződik. Mivel a magas értéket ad meg, ha hello fürt felfüggesztheti üresjárati feleslegesen magas költségeket eredményez. Ezért fontos, hogy állítsa a igények alapján hello megfelelő értékre.<br/><br/>Hello timetolive tulajdonság értékének megfelelően van beállítva, több folyamatok hello igény szerinti HDInsight-fürt példányának hello is megoszthatja.  |Igen |
| Verzió |Hello HDInsight-fürt verziószáma. hello alapértelmezett érték: a Windows-fürt 3.1 és a Linux-fürt 3.2-es verzióját. |Nem |
| linkedServiceName | Az Azure Storage társított szolgáltatás toobe történő tárolására és feldolgozására adatok hello igény fürt által használt. hello HDInsight-fürt létrehozása a hello azonos régióban legyen, mint az Azure Storage-fiók.<p>Jelenleg nem hozható létre, amely egy Azure Data Lake Store hello tárolóként használ igény szerinti HDInsight-fürtöt. Ha azt szeretné, hogy toostore hello ezért az adatok a HDInsight-feldolgozás alatt álló egy Azure Data Lake Store-ból, a hello Azure Blob Storage toohello Azure Data Lake Store másolási tevékenység toocopy hello adatait használják. </p>  | Igen |
| additionalLinkedServiceNames |Adja meg a további tárfiókok hello HDInsight a társított szolgáltatás, így hello Data Factory szolgáltatásnak is regisztrálja őket az Ön nevében. Ezekre a tárfiókokra hello kell lennie és hello HDInsight-fürt, amelynek létrehozása hello megegyezik ugyanabban a régióban hello tárfiókkal linkedServiceName által megadott régiót. |Nem |
| osType |Az operációs rendszer típusát. Két érték engedélyezett: (alapértelmezett) Windows és Linux |Nem |
| hcatalogLinkedServiceName |Azure SQL társított hello neve szolgáltatási pont toohello HCatalog adatbázis. hello igény szerinti HDInsight-fürt létrehozása hello metaadattárhoz hello Azure SQL database segítségével. |Nem |

#### <a name="additionallinkedservicenames-json-example"></a>additionalLinkedServiceNames JSON – példa

```json
"additionalLinkedServiceNames": [
    "otherLinkedServiceName1",
    "otherLinkedServiceName2"
  ]
```

### <a name="advanced-properties"></a>A Speciális tulajdonságok
Adja meg a következő részletességi szintű konfigurációs hello hello igény szerinti HDInsight-fürt tulajdonságai hello is.

| Tulajdonság | Leírás | Szükséges |
|:--- |:--- |:--- |
| coreConfiguration |Hello HDInsight fürt toobe létrehozott hello core konfigurációs paramétereket (ahogy a core-site.xml) határozza meg. |Nem |
| hBaseConfiguration |Meghatározza a hello HBase konfigurációs paramétereket (a hbase-site.xml) hello HDInsight-fürthöz. |Nem |
| hdfsConfiguration |Meghatározza a hello HDFS konfigurációs paramétereket (hdfs-site.xml) hello HDInsight-fürthöz. |Nem |
| hiveConfiguration |Meghatározza a hello hive konfigurációs paramétereket (hive-site.xml) hello HDInsight-fürthöz. |Nem |
| mapReduceConfiguration |Meghatározza a hello MapReduce konfigurációs paramétereket (mapred-site.xml) hello HDInsight-fürthöz. |Nem |
| oozieConfiguration |Meghatározza a hello Oozie konfigurációs paramétereket (oozie-site.xml) hello HDInsight-fürthöz. |Nem |
| stormConfiguration |Meghatározza a hello Storm konfigurációs paramétereket (a storm-site.xml) hello HDInsight-fürthöz. |Nem |
| yarnConfiguration |Meghatározza a hello Yarn konfigurációs paramétereket (yarn-site.xml) hello HDInsight-fürthöz. |Nem |

#### <a name="example--on-demand-hdinsight-cluster-configuration-with-advanced-properties"></a>A Speciális tulajdonságok igény szerinti HDInsight-fürt konfiguráció – – példa

```json
{
  "name": " HDInsightOnDemandLinkedService",
  "properties": {
    "type": "HDInsightOnDemand",
    "typeProperties": {
      "clusterSize": 16,
      "timeToLive": "01:30:00",
      "linkedServiceName": "adfods1",
      "coreConfiguration": {
        "templeton.mapper.memory.mb": "5000"
      },
      "hiveConfiguration": {
        "templeton.mapper.memory.mb": "5000"
      },
      "mapReduceConfiguration": {
        "mapreduce.reduce.java.opts": "-Xmx4000m",
        "mapreduce.map.java.opts": "-Xmx4000m",
        "mapreduce.map.memory.mb": "5000",
        "mapreduce.reduce.memory.mb": "5000",
        "mapreduce.job.reduce.slowstart.completedmaps": "0.8"
      },
      "yarnConfiguration": {
        "yarn.app.mapreduce.am.resource.mb": "5000",
        "mapreduce.map.memory.mb": "5000"
      },
      "additionalLinkedServiceNames": [
        "datafeeds",
        "adobedatafeed"
      ]
    }
  }
}
```

### <a name="node-sizes"></a>Csomópont-méretek
Hello méretek head, az adatok és a zookeeper csomópontok következő tulajdonságai hello segítségével adhatja meg: 

| Tulajdonság | Leírás | Szükséges |
|:--- |:--- |:--- |
| headNodeSize |Hello átjárócsomópont hello méretét határozza meg. hello alapértelmezett érték: Standard, D3. Lásd: hello **csomópont méretét megadó** című szakaszban talál információt. |Nem |
| dataNodeSize |Hello adatcsomóponton hello méretét határozza meg. hello alapértelmezett érték: Standard, D3. |Nem |
| zookeeperNodeSize |Hello itt üzemeltetőjének csomópont hello méretét határozza meg. hello alapértelmezett érték: Standard, D3. |Nem |

#### <a name="specifying-node-sizes"></a>Csomópont méret megadása
Lásd: hello [virtuálisgép-méretek](../virtual-machines/linux/sizes.md) karakterlánc-értékek toospecify hello előző szakaszban említett hello tulajdonságok van szüksége a cikkben találhat. hello értékeket kell tooconform toohello **parancsmagok & API-k** hello cikk hivatkozik. Ahogy látja, hello cikkben, hello adatok (alapértelmezett) nagy méretű fürtcsomópont 7 GB memória, amely nem lehet adott esetben elég jó. 

Ha azt szeretné, hogy toocreate D4 méretű átjárócsomópontokkal és feldolgozó csomópontokat, adja meg a **standard szintű, D4** headNodeSize és dataNodeSize tulajdonságok hello értékként. 

```json
"headNodeSize": "Standard_D4",    
"dataNodeSize": "Standard_D4",
```

Ezek a Tulajdonságok helytelen értéket ad meg, ha jelenhet meg hello következő **hiba:** sikertelen toocreate fürt. Kivétel: Nem toocomplete hello fürt létrehozási művelete. A művelet 400-as kóddal meghiúsult. A fürt állapota: „hiba”. Üzenet: "PreClusterCreationValidationFailure". Ha ez a hibaüzenet jelenik meg, győződjön meg arról, hogy hello használ **PARANCSMAG & API-k** nevű hello hello táblából [virtuálisgép-méretek](../virtual-machines/linux/sizes.md) cikk.  

## <a name="bring-your-own-compute-environment"></a>Kapcsolja a saját számítási környezet
Az ilyen típusú konfigurációs a felhasználók regisztrálhatják egy már meglévő informatikai környezete a Data Factory kapcsolt szolgáltatásként. hello számítási környezet hello felhasználó által felügyelt, és a Data Factory szolgáltatásnak hello tooexecute hello tevékenységek használja.

Az ilyen típusú konfigurációs hello következő számítási környezetek esetén támogatott:

* Az Azure HDInsight
* Azure Batch
* Azure Machine Learning
* Azure Data Lake Analytics
* Az Azure SQL-adatbázis, az Azure SQL DW, SQL Server

## <a name="azure-hdinsight-linked-service"></a>Az Azure HDInsight társított szolgáltatás
A Data Factory egy Azure HDInsight kapcsolódó szolgáltatás tooregister saját HDInsight-fürtöt hozhat létre.

### <a name="example"></a>Példa

```json
{
  "name": "HDInsightLinkedService",
  "properties": {
    "type": "HDInsight",
    "typeProperties": {
      "clusterUri": " https://<hdinsightclustername>.azurehdinsight.net/",
      "userName": "admin",
      "password": "<password>",
      "linkedServiceName": "MyHDInsightStoragelinkedService"
    }
  }
}
```

### <a name="properties"></a>Tulajdonságok
| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| type |hello típusú tulajdonságot kell beállítani, túl**HDInsight**. |Igen |
| clusterUri |hello hello HDInsight-fürt URI. |Igen |
| felhasználónév |Adja meg felhasználói toobe hello hello nevét használja tooconnect tooan meglévő HDInsight-fürtre. |Igen |
| jelszó |Adja meg a hello felhasználói fiókhoz tartozó jelszót. |Igen |
| linkedServiceName | Hello toohello Azure blob storage Azure Storage társított szolgáltatás neve hello által használt HDInsight-fürthöz. <p>Jelenleg nem adhat meg egy Azure Data Lake Store társított szolgáltatás ehhez a tulajdonsághoz. Ha hello HDInsight-fürt hozzáférés toohello Data Lake Store, előfordulhat, hogy éri hello Azure Data Lake Store a Hive/Pig-parancsfájlok. </p>  |Igen |

## <a name="azure-batch-linked-service"></a>Az Azure Batch társított szolgáltatás
Létrehozhat egy csatolt Azure Batch szolgáltatás tooregister virtuális gépek (VM) tooa adat-előállító kötegelt készletét. .NET-egyéni tevékenységek Azure Batch vagy Azure HDInsight segítségével is futtathatja.

Lásd az alábbi témakörök, ha új tooAzure Batch szolgáltatás:

* [Azure Batch alapjai](../batch/batch-technical-overview.md) hello Azure Batch szolgáltatás áttekintését.
* [Új AzureBatchAccount](https://msdn.microsoft.com/library/mt125880.aspx) parancsmag toocreate Azure Batch-fiók (vagy) [Azure-portálon](../batch/batch-account-create-portal.md) toocreate hello Azure Batch-fiók Azure-portál használatával. Lásd: [Azure Batch-fiókhoz használó PowerShell toomanage](http://blogs.technet.com/b/windowshpc/archive/2014/10/28/using-azure-powershell-to-manage-azure-batch-account.aspx) témakör részletes útmutatást a hello parancsmag használatával.
* [Új AzureBatchPool](https://msdn.microsoft.com/library/mt125936.aspx) parancsmag toocreate Azure Batch-készlet.

### <a name="example"></a>Példa

```json
{
  "name": "AzureBatchLinkedService",
  "properties": {
    "type": "AzureBatch",
    "typeProperties": {
      "accountName": "<Azure Batch account name>",
      "accessKey": "<Azure Batch account key>",
      "poolName": "<Azure Batch pool name>",
      "linkedServiceName": "<Specify associated storage linked service reference here>"
    }
  }
}
```

Hozzáfűzendő "**.\< régió neve\>**"hello toohello a batch-fiók nevét **accountName** tulajdonság. Példa:

```json
"accountName": "mybatchaccount.eastus"
```

Másik lehetőség is tooprovide hello batchUri végpont, ahogy az a következő minta hello:

```json
"accountName": "adfteam",
"batchUri": "https://eastus.batch.azure.com",
```

### <a name="properties"></a>Tulajdonságok
| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| type |hello típusú tulajdonságot kell beállítani, túl**AzureBatch**. |Igen |
| Fióknév |Hello Azure Batch-fiók neve. |Igen |
| accessKey |Hello Azure Batch-fiók elérési kulcsának. |Igen |
| poolName |A virtuális gépek hello készlet neve. |Igen |
| linkedServiceName |Hello a kapcsolódó Azure Batch-szolgáltatás társított Azure Storage társított szolgáltatás neve. A társított szolgáltatás szolgál átmeneti fájlok toorun hello tevékenység és a hello tevékenység végrehajtási naplók tárolásához szükséges. |Igen |

## <a name="azure-machine-learning-linked-service"></a>A társított szolgáltatásnak Azure gépi tanulás
A Machine Learning kötegelt pontozási végpont tooa adat-előállító létrehozása az Azure Machine Learning kapcsolódó szolgáltatás tooregister.

### <a name="example"></a>Példa

```json
{
  "name": "AzureMLLinkedService",
  "properties": {
    "type": "AzureML",
    "typeProperties": {
      "mlEndpoint": "https://[batch scoring endpoint]/jobs",
      "apiKey": "<apikey>"
    }
  }
}
```

### <a name="properties"></a>Tulajdonságok
| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| Típus |hello tulajdonságra kell megadni: **AzureML**. |Igen |
| mlEndpoint |hello kötegelt pontozás URL-CÍMÉT. |Igen |
| apiKey |hello közzétett munkaterület-modell API. |Igen |

## <a name="azure-data-lake-analytics-linked-service"></a>Az Azure Data Lake Analytics társított szolgáltatás
Létrehozhat egy **Azure Data Lake Analytics** szolgáltatás toolink egy Azure Data Lake Analytics számítási szolgáltatás tooan az Azure data factory kapcsolt. Data Lake Analytics U-SQL-tevékenység hello folyamat hello toothis kapcsolódó szolgáltatás hivatkozik. 

hello következő táblázat ismerteti hello hello JSON-definícióból használt általános tulajdonságok. További választhat egyszerű szolgáltatásnév és felhasználói hitelesítő adatok hitelesítése.

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| **típusa** |hello tulajdonságra kell megadni: **AzureDataLakeAnalytics**. |Igen |
| **Fióknév** |Az Azure Data Lake Analytics-fiók neve. |Igen |
| **datalakeanalyticsuri paraméter** |Az Azure Data Lake Analytics URI. |Nem |
| **előfizetés-azonosító** |Az Azure előfizetés-azonosító |Nem (Ha nincs megadva, az adat-előállító használt hello előfizetés). |
| **erőforráscsoport-név** |Azure erőforráscsoport-név |Nem (Ha nincs megadva, az adat-előállító használt hello erőforráscsoport). |

### <a name="service-principal-authentication-recommended"></a>Szolgáltatás egyszerű hitelesítés (ajánlott)
toouse szolgáltatás egyszerű hitelesítési regisztrálása az Azure Active Directory (Azure AD) és támogatás azt hello alkalmazás entitás tooData Lake áruház eléréséhez. Részletes útmutató: [szolgáltatások közötti hitelesítési](../data-lake-store/data-lake-store-authenticate-using-active-directory.md). Jegyezze fel az értéket, amely a következő hello toodefine hello társított szolgáltatáshoz:
* Alkalmazásazonosító
* Alkalmazás kulcs 
* Bérlőazonosító

Szolgáltatás egyszerű hitelesítés használata a következő tulajdonságok hello megadásával:

| Tulajdonság | Leírás | Szükséges |
|:--- |:--- |:--- |
| **servicePrincipalId** | Adja meg a hello alkalmazás ügyfél-azonosítót. | Igen |
| **servicePrincipalKey** | Adja meg a hello kulcsát. | Igen |
| **Bérlői** | Adja meg a hello bérlői adatokat (tartomány nevét vagy a bérlő azonosító) alatt az alkalmazás található. Ez által rámutató hello egér hello Azure-portál jobb felső sarkában hello kérheti le. | Igen |

**Példa: Szolgáltatás egyszerű hitelesítés**
```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "adftestaccount",
            "dataLakeAnalyticsUri": "datalakeanalyticscompute.net",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "subscriptionId": "<optional, subscription id of ADLA>",
            "resourceGroupName": "<optional, resource group name of ADLA>"
        }
    }
}
```

### <a name="user-credential-authentication"></a>Felhasználói hitelesítő adatok hitelesítése
Alternatív megoldásként használható felhasználói hitelesítő adatok hitelesítése Data Lake Analytics megadásával következő tulajdonságai hello:

| Tulajdonság | Leírás | Szükséges |
|:--- |:--- |:--- |
| **engedélyezési** | Kattintson a hello **engedélyezés** hello Data Factory Editor gombra, és írja be a hitelesítő adat, amelyet hozzárendel hello automatikusan létrehozott engedélyezési URL-cím toothis tulajdonság. | Igen |
| **munkamenet-azonosító** | OAuth munkamenet-azonosító hello OAuth hitelesítési munkamenetből. Minden munkamenet-azonosító egyedi, és csak egyszer használható. Ez a beállítás automatikusan létrejön a Data Factory Editor hello használatakor. | Igen |

**Példa: Felhasználók hitelesítő adatok hitelesítése**
```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "adftestaccount",
            "dataLakeAnalyticsUri": "datalakeanalyticscompute.net",
            "authorization": "<authcode>",
            "sessionId": "<session ID>", 
            "subscriptionId": "<optional, subscription id of ADLA>",
            "resourceGroupName": "<optional, resource group name of ADLA>"
        }
    }
}
```

#### <a name="token-expiration"></a>Jogkivonat lejáratáról
Ön hello segítségével létrehozott engedélyezési kód hello **engedélyezés** gomb némi várakozás után lejár. Tekintse meg a következő táblázat a különböző típusú felhasználói fiókok hello lejárati idejének hello. Hello a következő hibaüzenetet fog látni amikor hello hitelesítési **jogkivonat lejár**: hitelesítőadat-műveleti hiba: invalid_grant - AADSTS70002: Hiba történt a hitelesítő adatok ellenőrzése. AADSTS70008: hello megadott hozzáférés biztosítása lejárt vagy visszavonták. Nyomkövetési azonosító: d18629e8-af88-43c5-88e3-d8419eb1fca1 Korrelációazonosító: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 időbélyegző: 2015-12-15 21:09:31Z

| Felhasználó típusa | Után lejár |
|:--- |:--- |
| Felhasználói fiókok Azure Active Directory által nem kezelt (@hotmail.com, @live.comstb.) |12 óra |
| Felhasználói fiókok felügyelete által Azure Active Directory (AAD) |Futtatás után utolsó szelet hello 14 nap. <br/><br/>90 nap, ha a szelet OAuth-alapú társított szolgáltatás fut legalább 14 naponta. |

tooavoid/hárítsa el a hiba, ismét engedélyezheti a hello segítségével **engedélyezés** gombbal hello **jogkivonat lejár** és telepítse újra a kapcsolódó hello szolgáltatást. Értékek is létrehozhat **sessionId** és **engedélyezési** programozott módon, az alábbiak szerint kód tulajdonságok:

```csharp
if (linkedService.Properties.TypeProperties is AzureDataLakeStoreLinkedService ||
    linkedService.Properties.TypeProperties is AzureDataLakeAnalyticsLinkedService)
{
    AuthorizationSessionGetResponse authorizationSession = this.Client.OAuth.Get(this.ResourceGroupName, this.DataFactoryName, linkedService.Properties.Type);

    WindowsFormsWebAuthenticationDialog authenticationDialog = new WindowsFormsWebAuthenticationDialog(null);
    string authorization = authenticationDialog.AuthenticateAAD(authorizationSession.AuthorizationSession.Endpoint, new Uri("urn:ietf:wg:oauth:2.0:oob"));

    AzureDataLakeStoreLinkedService azureDataLakeStoreProperties = linkedService.Properties.TypeProperties as AzureDataLakeStoreLinkedService;
    if (azureDataLakeStoreProperties != null)
    {
        azureDataLakeStoreProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
        azureDataLakeStoreProperties.Authorization = authorization;
    }

    AzureDataLakeAnalyticsLinkedService azureDataLakeAnalyticsProperties = linkedService.Properties.TypeProperties as AzureDataLakeAnalyticsLinkedService;
    if (azureDataLakeAnalyticsProperties != null)
    {
        azureDataLakeAnalyticsProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
        azureDataLakeAnalyticsProperties.Authorization = authorization;
    }
}
```

Lásd: [AzureDataLakeStoreLinkedService osztály](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [AzureDataLakeAnalyticsLinkedService osztály](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx), és [AuthorizationSessionGetResponse osztály](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) részletes kapcsolatos témakörök kapcsolatos hello adat-előállító osztályok hello kódban használt. Adjon hozzá egy hivatkozást,: Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll hello WindowsFormsWebAuthenticationDialog osztály számára. 

## <a name="azure-sql-linked-service"></a>Az Azure SQL társított szolgáltatásnak
Hozzon létre egy Azure SQL társított szolgáltatást, és használatához a hello [tárolt eljárási tevékenység](data-factory-stored-proc-activity.md) tooinvoke a Data Factory-folyamathoz tárolt eljárást. Lásd: [Azure SQL-összekötő](data-factory-azure-sql-connector.md#linked-service-properties) szóló cikkben olvashat a szolgáltatásnak.

## <a name="azure-sql-data-warehouse-linked-service"></a>Az Azure SQL Data Warehouse-társított szolgáltatás
Azure SQL Data Warehouse társított szolgáltatás létrehozása és használatához a hello [tárolt eljárási tevékenység](data-factory-stored-proc-activity.md) tooinvoke a Data Factory-folyamathoz tárolt eljárást. Lásd: [Azure SQL Data Warehouse-összekötő](data-factory-azure-sql-data-warehouse-connector.md#linked-service-properties) szóló cikkben olvashat a szolgáltatásnak.

## <a name="sql-server-linked-service"></a>SQL Server társított szolgáltatás
Hozzon létre csatolt SQL Server szolgáltatást, és használatához a hello [tárolt eljárási tevékenység](data-factory-stored-proc-activity.md) tooinvoke a Data Factory-folyamathoz tárolt eljárást. Lásd: [SQL Server-összekötő](data-factory-sqlserver-connector.md#linked-service-properties) szóló cikkben olvashat a szolgáltatásnak.

