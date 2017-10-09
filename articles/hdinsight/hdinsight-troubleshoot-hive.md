---
title: "Hive aaaTroubleshoot Azure HDInsight használatával |} Microsoft Docs"
description: "Válaszok az Apache Hive és a Azure HDInsight toocommon kérdésekre."
keywords: "Az Azure HDInsight Hive, gyakori kérdések hibaelhárítási útmutatója, gyakori kérdések"
services: Azure HDInsight
documentationcenter: na
author: dharmeshkakadia
manager: 
editor: 
ms.assetid: 15B8D0F3-F2D3-4746-BDCB-C72944AA9252
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/7/2017
ms.author: dharmeshkakadia
ms.openlocfilehash: ac459316e658d0b29eb66f5685f0bc7e693bb277
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-hive-by-using-azure-hdinsight"></a>Hive hibaelhárítása az Azure HDInsight használatával

Hello felső kérdések és azok megoldásait ismerje meg az Apache Ambari az Apache Hive Payload használatakor.


## <a name="how-do-i-export-a-hive-metastore-and-import-it-on-another-cluster"></a>Hogyan egy Hive metaadattárhoz exportálhatja és importálhatja azokat a fürt egy másik


### <a name="resolution-steps"></a>Megoldási lépések

1. Toohello HDInsight-fürtjéhez Secure Shell (SSH) ügyfél csatlakozhat. További információkért lásd: [További olvasnivaló](#additional-reading-end).

2. Futtassa a parancsot követő hello HDInsight-fürtre, amelyből el kívánja tooexport hello metaadattárhoz hello:

    ```apache
    for d in `hive -e "show databases"`; do echo "create database $d; use $d;" >> alltables.sql ; for t in `hive --database $d -e "show tables"` ; do ddl=`hive --database $d -e "show create table $t"`; echo "$ddl ;" >> alltables.sql ; echo "$ddl" | grep -q "PARTITIONED\s*BY" && echo "MSCK REPAIR TABLE $t ;" >> alltables.sql ; done; done
    ```

  A parancs létrehoz egy allatables.sql nevű fájlt.

3. Hello fájl alltables.sql toohello új HDInsight-fürt másolja, és futtassa a parancsot a következő hello:

  ```apache
  hive -f alltables.sql
  ```

hello kód hello megoldási lépések a azt feltételezi, hogy hello új fürtön az elérési utakat adatok hello azonos hello adatelérési utak hello régi fürtön. Ha hello adatelérési utak eltérőek, manuálisan módosíthatja generált hello alltables.sql fájl tooreflect módosításokat.

### <a name="additional-reading"></a>További olvasnivaló

- [Csatlakozás tooan HDInsight-fürthöz SSH segítségével](hdinsight-hadoop-linux-use-ssh-unix.md)


## <a name="how-do-i-locate-hive-logs-on-a-cluster"></a>Hogyan keresik meg Hive naplók fürt

### <a name="resolution-steps"></a>Megoldási lépések

1. Csatlakozás toohello HDInsight-fürthöz SSH használatával. További információkért lásd: **További olvasnivaló**.

2. tooview Hive ügyfeleinek naplói, a következő parancs hello használata:

  ```apache
  /tmp/<username>/hive.log 
  ```

3. tooview Hive metaadattárhoz naplókat, a következő parancs hello használata:

  ```apache
  /var/log/hive/hivemetastore.log 
  ```

4. tooview Hiveserver naplókat, a következő parancs hello használata:

  ```apache
  /var/log/hive/hiveserver2.log 
  ```

### <a name="additional-reading"></a>További olvasnivaló

- [Csatlakozás tooan HDInsight-fürthöz SSH segítségével](hdinsight-hadoop-linux-use-ssh-unix.md)


## <a name="how-do-i-launch-hello-hive-shell-with-specific-configurations-on-a-cluster"></a>Hogyan indítsa el a Hive rendszerhéjat egy fürtön adott konfigurációjú hello

### <a name="resolution-steps"></a>Megoldási lépések

1. Adjon meg egy konfigurációs kulcs-érték párt, hello Hive rendszerhéjat indításakor. További információkért lásd: [További olvasnivaló](#additional-reading-end).

  ```apache
  hive -hiveconf a=b 
  ```

2. toolist összes hatékony konfiguráció a Hive rendszerhéjat, használjon hello a következő parancsot:

  ```apache
  hive> set;
  ```

  Például használhatja hello toostart Hive parancshéj hibakeresési naplózással hello konzol engedélyezve a következő:

  ```apache
  hive -hiveconf hive.root.logger=ALL,console 
  ```

### <a name="additional-reading"></a>További olvasnivaló

- [Hive konfiguráció tulajdonságai](https://cwiki.apache.org/confluence/display/Hive/Configuration+Properties)


## <a name="how-do-i-analyze-tez-dag-data-on-a-cluster-critical-path"></a>Hogyan elemezheti a fürt kritikus útvonalra Tez DAG adatokat


### <a name="resolution-steps"></a>Megoldási lépések
 
1. az Apache Tez tooanalyze irányított aciklikus diagramhoz (DAG) egy fürt kritikus grafikonon, toohello HDInsight-fürt csatlakozzon az SSH használatával. További információkért lásd: [További olvasnivaló](#additional-reading-end).

2. Parancsot egy parancssorba futtassa a következő parancs hello:
   
  ```apache
  hadoop jar /usr/hdp/current/tez-client/tez-job-analyzer-*.jar CriticalPath --saveResults --dagId <DagId> --eventFileName <DagData.zip> 
  ```

3. toolist más lehet foglalt tooanalyze Tez DAG elemzőkkel hello a következő parancsot használja:

  ```apache
  hadoop jar /usr/hdp/current/tez-client/tez-job-analyzer-*.jar
  ```

  Meg kell adnia egy példa program hello első argumentumként.

  A program érvényes nevek a következők:
    - **ContainerReuseAnalyzer**: tároló újbóli részleteit egy DAG nyomtatása
    - **CriticalPath**: keresés hello kritikus elérési útja egy dag-csoport
    - **LocalityAnalyzer**: egy DAG helység részleteket nyomtatása
    - **ShuffleTimeAnalyzer**: hello sorrendű idő – részletek a egy DAG elemzése
    - **SkewAnalyzer**: hello eltérésére részleteket a egy dag-csoport
    - **SlowNodeAnalyzer**: csomópont részleteit egy DAG nyomtatása
    - **SlowTaskIdentifier**: egy dag-csoport a nyomtatás lassú feladat részletei
    - **SlowestVertexAnalyzer**: egy DAG leglassabb csúcspont részleteket nyomtatása
    - **SpillAnalyzer**: nyomtatás spill részleteit egy dag-csoport
    - **TaskConcurrencyAnalyzer**: hello feladat egyidejű részleteit egy DAG nyomtatása
    - **VertexLevelCriticalPathAnalyzer**: Find hello kritikus elérési út egy DAG csúcspont szinten


### <a name="additional-reading"></a>További olvasnivaló

- [Csatlakozás tooan HDInsight-fürthöz SSH segítségével](hdinsight-hadoop-linux-use-ssh-unix.md)


## <a name="how-do-i-download-tez-dag-data-from-a-cluster"></a>Hogyan le Tez DAG adatok fürtből


#### <a name="resolution-steps"></a>Megoldási lépések

Két módon toocollect hello Tez DAG adatokat:

- Hello parancssorból:
 
    Csatlakozás toohello HDInsight-fürthöz SSH használatával. Hello parancssorban futtassa a következő parancs hello:

  ```apache
  hadoop jar /usr/hdp/current/tez-client/tez-history-parser-*.jar org.apache.tez.history.ATSImportTool -downloadDir . -dagId <DagId> 
  ```

- Ambari Tez nézet hello használata:
   
  1. Nyissa meg tooAmbari. 
  2. (A hello csempék ikon hello jobb felső sarkában található) lépjen tooTez nézet. 
  3. Válassza ki a kívánt tooview DAG hello.
  4. Válassza ki **adatletöltéshez**.

### <a name="additional-reading-end"></a>További olvasnivaló

[Csatlakozás tooan HDInsight-fürthöz SSH segítségével](hdinsight-hadoop-linux-use-ssh-unix.md)






