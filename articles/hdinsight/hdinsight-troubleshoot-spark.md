---
title: "aaaTroubleshoot Spark on Azure HDInsight használatával |} Microsoft Docs"
description: "Válaszok az Apache Spark és az Azure HDInsight toocommon kérdésekre."
keywords: "Az Azure HDInsight Spark, gyakran ismételt kérdések, hibaelhárítási útmutató, gyakori problémákat, Alkalmazáskonfiguráció, Ambari"
services: Azure HDInsight
documentationcenter: na
author: arijitt
manager: 
editor: 
ms.assetid: 25D89586-DE5B-4268-B5D5-CC2CE12207ED
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/7/2017
ms.author: arijitt
ms.openlocfilehash: c9f910daf295462238a3143ae2589db6d383097f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-spark-by-using-azure-hdinsight"></a>Hibaelhárítás Spark on Azure HDInsight segítségével

Hello legfőbb problémákat és azok megoldásait ismerje meg az Apache Ambari az Apache Spark Payload van jelen használatakor.

## <a name="how-do-i-configure-a-spark-application-by-using-ambari-on-clusters"></a>Hogyan konfigurálhatom a Spark-alkalmazások fürtök az Ambari használatával

### <a name="resolution-steps"></a>Megoldási lépések

hello konfigurációs értékeket az eljárás végrehajtásához a Hdinsightban korábban beállított. toodetermine mely Spark konfigurációk kell toobe és a toowhat értékek, lásd: [mi okozza a Spark OutofMemoryError Alkalmazáskivétel](#what-causes-a-spark-application-outofmemoryerror-exception). 

1. A fürtök hello listában válassza ki a **Spark2**.

    ![Válassza ki a fürtöt a listából](media/hdinsight-troubleshoot-spark/update-config-1.png)

2. Jelölje be hello **Configs** fülre.

    ![Válassza ki a hello Configs lap](media/hdinsight-troubleshoot-spark/update-config-2.png)

3. A konfigurációk hello listában válassza ki a **egyéni-spark2-alapértelmezett**.

    ![Válassza ki az egyéni-spark-alapértelmezései](media/hdinsight-troubleshoot-spark/update-config-3.png)

4. Hello érték beállítása tooadjust, például a kell keresni **spark.executor.memory**. Ebben az esetben a értékének hello **4608m** túl nagy.

    ![Válassza ki a hello spark.executor.memory mező](media/hdinsight-troubleshoot-spark/update-config-4.png)

5. Set hello érték toohello ajánlott beállítás. érték hello **2048m** ezt a beállítást ajánlott.

    ![Változás érték too2048m](media/hdinsight-troubleshoot-spark/update-config-5.png)

6. Mentse hello értéket, és mentse a hello konfigurációs. Hello eszköztáron válassza **mentése**.

    ![Hello beállítás és konfiguráció mentése](media/hdinsight-troubleshoot-spark/update-config-6a.png)

    Ha a konfigurációkat kezeléséről értesítést kap. Vegye figyelembe a hello elemeket, majd válassza ki **mégis folytatni**. 

    ![Válassza ki mégis folytatni](media/hdinsight-troubleshoot-spark/update-config-6b.png)

    Írjon megjegyzést hello konfigurációs módosításokat, majd válassza ki **mentése**.

    ![Hello módosításokkal kapcsolatos megjegyzés](media/hdinsight-troubleshoot-spark/update-config-6c.png)

7. Amikor egy konfigurációs mentésekor kéri toorestart hello szolgáltatást. Válassza ki **indítsa újra a**.

    ![Válassza ki az újraindítást](media/hdinsight-troubleshoot-spark/update-config-7a.png)

    Erősítse meg a hello újraindítás.

    ![Válassza ki erősítse meg, indítsa újra az összes](media/hdinsight-troubleshoot-spark/update-config-7b.png)

    Futó folyamatok hello tekintheti meg.

    ![Tekintse át a futó folyamatok](media/hdinsight-troubleshoot-spark/update-config-7c.png)

8. Konfigurációk is hozzáadhat. A konfigurációk hello listában válassza ki a **egyéni-spark2-alapértelmezett**, majd válassza ki **tulajdonság hozzáadása**.

    ![Válassza ki a tulajdonság hozzáadása](media/hdinsight-troubleshoot-spark/update-config-8.png)

9. Adja meg az új tulajdonság. Egy-egy tulajdonság az egyes beállítások, például hello adatok típusát egy párbeszédpanel segítségével adhat meg. Vagy több tulajdonságok adhatók soronként egy definíciót használatával. 

    Ebben a példában hello **spark.driver.memory** tulajdonságot definiálták, értékű **4g**.

    ![Új tulajdonság megadása](media/hdinsight-troubleshoot-spark/update-config-9.png)

10. Hello konfiguráció mentéséhez, majd indítsa újra hello szolgáltatást, 6 és 7 lépésben leírtak szerint.

Ezek a változások fürt kiterjedő, de hello Spark feladat elküldése felülbírálható.

### <a name="additional-reading"></a>További olvasnivaló

[Spark feladat elküldése a HDInsight-fürtökön](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="how-do-i-configure-a-spark-application-by-using-a-jupyter-notebook-on-clusters"></a>Hogyan konfigurálhatom a Spark-alkalmazások fürtökön Jupyter notebook használatával

### <a name="resolution-steps"></a>Megoldási lépések

1. toodetermine mely Spark konfigurációk kell toobe és a toowhat értékek, lásd: [mi okozza a Spark OutofMemoryError Alkalmazáskivétel](#what-causes-a-spark-application-outofmemoryerror-exception).

2. Hello első cellájában hello Jupyter notebook hello után **%% konfigurálása** irányelv, adja meg a hello Spark konfigurációk érvényes JSON formátumban. Hello tényleges értékek módosítása szükséges:

    ![A konfiguráció hozzáadása](media/hdinsight-troubleshoot-spark/add-configuration-cell.png)

### <a name="additional-reading"></a>További olvasnivaló

[Spark feladat elküldése a HDInsight-fürtökön](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="how-do-i-configure-a-spark-application-by-using-livy-on-clusters"></a>Hogyan konfigurálhatom a Spark-alkalmazások fürtökön Livy használatával

### <a name="resolution-steps"></a>Megoldási lépések

1. toodetermine mely Spark konfigurációk kell toobe és a toowhat értékek, lásd: [mi okozza a Spark OutofMemoryError Alkalmazáskivétel](#what-causes-a-spark-application-outofmemoryerror-exception). 

2. Hello Spark alkalmazás tooLivy nyújt a REST-ügyfelek, például cURL használatával. Használja a parancs hasonló toohello következő. Hello tényleges értékek módosítása szükséges:

    ```apache
    curl -k --user 'username:password' -v -H 'Content-Type: application/json' -X POST -d '{ "file":"wasb://container@storageaccountname.blob.core.windows.net/example/jars/sparkapplication.jar", "className":"com.microsoft.spark.application", "numExecutors":4, "executorMemory":"4g", "executorCores":2, "driverMemory":"8g", "driverCores":4}'  
    ```

### <a name="additional-reading"></a>További olvasnivaló

[Spark feladat elküldése a HDInsight-fürtökön](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="how-do-i-configure-a-spark-application-by-using-spark-submit-on-clusters"></a>Hogyan konfigurálhatók a alkalmazás használatával spark-elküldeni egy Spark-fürtökön

### <a name="resolution-steps"></a>Megoldási lépések

1. toodetermine mely Spark konfigurációk kell toobe és a toowhat értékek, lásd: [mi okozza a Spark OutofMemoryError Alkalmazáskivétel](#what-causes-a-spark-application-outofmemoryerror-exception).

2. Egy hasonló toohello következő parancs használatával indítsa el a spark-rendszerhéjat. Hello konfigurációk hello aktuális értékét módosítsa szükség szerint: 

    ```apache
    spark-submit --master yarn-cluster --class com.microsoft.spark.application --num-executors 4 --executor-memory 4g --executor-cores 2 --driver-memory 8g --driver-cores 4 /home/user/spark/sparkapplication.jar
    ```

### <a name="additional-reading"></a>További olvasnivaló

[Spark feladat elküldése a HDInsight-fürtökön](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="what-causes-a-spark-application-outofmemoryerror-exception"></a>Mi okozza a Spark OutofMemoryError Alkalmazáskivétel

### <a name="detailed-description"></a>Részletes leírás

hello Spark alkalmazás nem sikerül, a következő típusú nem kezelt kivételek hello:

```apache
ERROR Executor: Exception in task 7.0 in stage 6.0 (TID 439) 

java.lang.OutOfMemoryError 
    at java.io.ByteArrayOutputStream.hugeCapacity(Unknown Source) 
    at java.io.ByteArrayOutputStream.grow(Unknown Source) 
    at java.io.ByteArrayOutputStream.ensureCapacity(Unknown Source) 
    at java.io.ByteArrayOutputStream.write(Unknown Source) 
    at java.io.ObjectOutputStream$BlockDataOutputStream.drain(Unknown Source) 
    at java.io.ObjectOutputStream$BlockDataOutputStream.setBlockDataMode(Unknown Source) 
    at java.io.ObjectOutputStream.writeObject0(Unknown Source) 
    at java.io.ObjectOutputStream.writeObject(Unknown Source) 
    at org.apache.spark.serializer.JavaSerializationStream.writeObject(JavaSerializer.scala:44) 
    at org.apache.spark.serializer.JavaSerializerInstance.serialize(JavaSerializer.scala:101) 
    at org.apache.spark.executor.Executor$TaskRunner.run(Executor.scala:239) 
    at java.util.concurrent.ThreadPoolExecutor.runWorker(Unknown Source) 
    at java.util.concurrent.ThreadPoolExecutor$Worker.run(Unknown Source) 
    at java.lang.Thread.run(Unknown Source) 
```

```apache
ERROR SparkUncaughtExceptionHandler: Uncaught exception in thread Thread[Executor task launch worker-0,5,main] 

java.lang.OutOfMemoryError 
    at java.io.ByteArrayOutputStream.hugeCapacity(Unknown Source) 
    at java.io.ByteArrayOutputStream.grow(Unknown Source) 
    at java.io.ByteArrayOutputStream.ensureCapacity(Unknown Source) 
    at java.io.ByteArrayOutputStream.write(Unknown Source) 
    at java.io.ObjectOutputStream$BlockDataOutputStream.drain(Unknown Source) 
    at java.io.ObjectOutputStream$BlockDataOutputStream.setBlockDataMode(Unknown Source) 
    at java.io.ObjectOutputStream.writeObject0(Unknown Source) 
    at java.io.ObjectOutputStream.writeObject(Unknown Source) 
    at org.apache.spark.serializer.JavaSerializationStream.writeObject(JavaSerializer.scala:44) 
    at org.apache.spark.serializer.JavaSerializerInstance.serialize(JavaSerializer.scala:101) 
    at org.apache.spark.executor.Executor$TaskRunner.run(Executor.scala:239) 
    at java.util.concurrent.ThreadPoolExecutor.runWorker(Unknown Source) 
    at java.util.concurrent.ThreadPoolExecutor$Worker.run(Unknown Source) 
    at java.lang.Thread.run(Unknown Source) 
```

### <a name="probable-cause"></a>Lehetséges ok

Ez a kivétel hello legvalószínűbb oka, hogy nem elég halommemória lefoglalja a memóriát toohello Java virtuális gépek (JVMs). Ezek JVMs hello Spark alkalmazás részeként végrehajtója vagy illesztőprogramok működését. 

### <a name="resolution-steps"></a>Megoldási lépések

1. Határozza meg a maximális méretét hello hello adatok hello Spark alkalmazás kezeli. Egy becslés hello bemeneti adatok hello köztes hello bemeneti adatokat, és átalakításakor hello alkalmazás van további hello köztes adatok előállított hello kimeneti adatok átalakítása által létrehozott hello maximális mérete alapján végezheti el. Ez a folyamat az ismétlődő lehet, ha egy kezdeti formális becslés nem hajtható végre. 

2. Győződjön meg arról, hogy, hogy fog toouse rendelkezik elegendő memóriával és -magok tooaccommodate hello Spark alkalmazás erőforrások hello HDInsight-fürthöz. Ez hello fürt metrikák szakasza hello YARN felhasználói felületen hello értékek megtekintésével meghatározhatja a **használt memória** vs. **Memória összesen**, és **VCores használt** vs. **VCores összesen**.

3. Állítsa be a következő Spark hello konfigurációk tooappropriate értékek, amelyek nem haladhatja meg a rendelkezésre álló memória hello és -magok 90 %-át. hello értékeket is hello memóriaigényének hello Spark alkalmazás belül kell lennie: 

    ```apache
    spark.executor.instances (Example: 8 for 8 executor count) 
    spark.executor.memory (Example: 4g for 4 GB) 
    spark.yarn.executor.memoryOverhead (Example: 384m for 384 MB) 
    spark.executor.cores (Example: 2 for 2 cores per executor) 
    spark.driver.memory (Example: 8g for 8GB) 
    spark.driver.cores (Example: 4 for 4 cores)   
    spark.yarn.driver.memoryOverhead (Example: 384m for 384MB) 
    ```

    tooget hello teljes memóriahasználatát összes végrehajtója, futtassa a következő parancs hello: 
    
    ```apache
    spark.executor.instances * (spark.executor.memory + spark.yarn.executor.memoryOverhead) 
    ```
    tooget hello teljes memóriahasználatát hello illesztőprogram, futtassa a következő parancs hello:
    
    ```apache
    spark.driver.memory + spark.yarn.driver.memoryOverhead
    ```

### <a name="additional-reading"></a>További olvasnivaló

- [Spark memória – áttekintés](http://spark.apache.org/docs/latest/tuning.html#memory-management-overview)
- [A Spark on HDInsight-fürt alkalmazások hibakeresése](https://blogs.msdn.microsoft.com/azuredatalake/2016/12/19/spark-debugging-101/)

