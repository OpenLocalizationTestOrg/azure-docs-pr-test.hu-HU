---
title: "aaaUse Livy Spark toosubmit feladatok tooSpark fürt Azure HDInsight |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Apache Spark REST API toosubmit Spark feladatok távolról tooan Azure HDInsight-fürthöz."
keywords: az Apache Spark on rest API-t livy spark
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 2817b779-1594-486b-8759-489379ca907d
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: nitinme
ms.openlocfilehash: 417213b5f1db1522373188002fe05117faea5243
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-apache-spark-rest-api-toosubmit-remote-jobs-tooan-hdinsight-spark-cluster"></a>Apache Spark REST API toosubmit távoli feladatok tooan HDInsight Spark-fürt használatára

Ismerje meg, hogyan toouse Livy, Apache Spark REST API, amely távoli használt toosubmit hello feladatok tooan Azure HDInsight Spark-fürt. További részletes dokumentációt: [Livy](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server).

Livy toorun interaktív Spark ismertetése használhatja, vagy küldje el a kötegelt feladatok toobe Spark futtatnak. Ez a cikk beszél Livy toosubmit kötegelt feladatok használatával. Ebben a cikkben hello kódtöredékek használata cURL toomake REST API-hívások toohello Livy Spark végpont.

**Előfeltételek:**

* A HDInsight az Apache Spark-fürt. Útmutatásért lásd: [létrehozása az Apache Spark on Azure hdinsight clusters](hdinsight-apache-spark-jupyter-spark-sql.md).

* [cURL](http://curl.haxx.se/). Ez a cikk a cURL toodemonstrate hogyan toomake REST API meghívja a HDInsight Spark-fürt elleni használja.

## <a name="submit-a-livy-spark-batch-job"></a>Livy Spark kötegelt feladat elküldése
Kötegelt mentése előtt hello alkalmazás jar a hello-fürthöz tartozó hello fürttároló kell feltöltenie. Használhat [ **AzCopy**](../storage/common/storage-use-azcopy.md), a parancssori segédprogram, toodo így. Nincsenek más ügyfelek különböző tooupload adatokat használhatja. A rájuk vonatkozó további található [feltölteni az adatokat a HDInsight Hadoop-feladatok](hdinsight-upload-data.md).

    curl -k --user "<hdinsight user>:<user password>" -v -H <content-type> -X POST -d '{ "file":"<path tooapplication jar>", "className":"<classname in jar>" }' 'https://<spark_cluster_name>.azurehdinsight.net/livy/batches'

**Példák**:

* Ha hello jar-fájlra hello fürt storage (WASB)
  
        curl -k --user "admin:mypassword1!" -v -H 'Content-Type: application/json' -X POST -d '{ "file":"wasb://mycontainer@mystorageaccount.blob.core.windows.net/data/SparkSimpleTest.jar", "className":"com.microsoft.spark.test.SimpleFile" }' "https://mysparkcluster.azurehdinsight.net/livy/batches"
* Ha azt szeretné, hogy toopass hello jar fájlnév és a bemeneti fájl részeként osztálynév hello (ebben a példában input.txt)
  
        curl -k  --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

## <a name="get-information-on-livy-spark-batches-running-on-hello-cluster"></a>Információ a hello fürtben futó Livy Spark kötegek
    curl -k --user "<hdinsight user>:<user password>" -v -X GET "https://<spark_cluster_name>.azurehdinsight.net/livy/batches"

**Példák**:

* Ha azt szeretné, tooretrieve hello fürtben futó összes hello Livy Spark kötegek:
  
        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches"
* Ha azt szeretné, hogy egy adott kötegazonosító adott kötegben tooretrieve
  
        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches/{batchId}"

## <a name="delete-a-livy-spark-batch-job"></a>A Livy Spark kötegelt törlése
    curl -k --user "<hdinsight user>:<user password>" -v -X DELETE "https://<spark_cluster_name>.azurehdinsight.net/livy/batches/{batchId}"

**Példa**:

    curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/{batchId}"

## <a name="livy-spark-and-high-availability"></a>Livy Spark és magas rendelkezésre állású
Livy hello fürtben futó Spark feladatok biztosít a magas rendelkezésre állású. Íme néhány példa.

* Ha hello Livy szolgáltatás nem működik, miután egy feladat távolról elküldte tooa Spark fürtben hello feladat toorun hello háttérben folytatódik. Ha Livy készítsen biztonsági másolatot, hello feladat és a jelentések vissza hello állapotát állítja vissza.
* Jupyter notebookok a HDInsight a hello háttérbeli Livy szerint vannak kapcsolva. Ha a notebook egy Spark feladat fut, és hello Livy szolgáltatás újraindítása lekérdezi, hello notebook toorun hello kód cellák továbbra is. 

## <a name="show-me-an-example"></a>Példa megjelenítése
Ebben a részben azt példák toouse Livy Spark toosubmit kötegelt tekintse meg, hello hello feladat előrehaladásának figyeléséhez, és törölje azt. hello ebben a példában használjuk alkalmazás egy fejlesztett hello cikkben hello [egy önálló Scala alkalmazás és toorun létrehozása a HDInsight Spark-fürt](hdinsight-apache-spark-create-standalone-application.md). Itt a hello lépések azt feltételezik, hogy:

* Már felülírt hello alkalmazás jar toohello tárfiók hello-fürthöz tartozó.
* A CuRL ezeket a lépéseket megkísérlése hello számítógépen telepítve van.

Hajtsa végre az alábbi lépésekkel hello:

1. Ossza meg velünk először győződjön meg arról, hogy fut-e Livy Spark hello fürtön. Olvasson be kötegek futó azt is megteheti. Ha egy feladat Livy használatával hello az első alkalommal futtatja, hello kimeneti nulla kell visszaadnia.
   
        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches"
   
    Az alábbi részlet kimeneti hasonló toohello szerezheti be:
   
        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:47:53 GMT
        < Content-Length: 34
        <
        {"from":0,"total":0,"sessions":[]}* Connection #0 toohost mysparkcluster.azurehdinsight.net left intact
   
    Figyelje meg, hogyan hello utolsó hello kimenet sorban a **összesen: 0**, amely nem futó kötegek javasol.

2. Ossza meg velünk most kötegelt feladat elküldése. a következő kódrészletet hello paraméterként egy bemeneti fájl (input.txt) toopass hello jar és hello osztály nevét használja. Ha ezeket a lépéseket egy Windows-számítógépen futtatja, az ajánlott megközelítést alkalmazva hello bemeneti fájl segítségével.
   
        curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"
   
    hello fájlban paraméterek hello **input.txt** az alábbiak szerint definiáltuk:
   
        { "file":"wasb:///example/jars/SparkSimpleApp.jar", "className":"com.microsoft.spark.example.WasbIOTest" }
   
    Az alábbi részlet kimeneti hasonló toohello kell megjelennie:
   
        < HTTP/1.1 201 Created
        < Content-Type: application/json; charset=UTF-8
        < Location: /0
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:51:30 GMT
        < Content-Length: 36
        <
        {"id":0,"state":"starting","log":[]}* Connection #0 toohost mysparkcluster.azurehdinsight.net left intact
   
    Figyelje meg, hogyan hello hello kimenet utolsó sora szerint **állapota: indítása**. Azt is jelzi, **azonosító: 0**. Itt **0** hello kötegelt azonosító.

3. Most le hello állapota az adott kötegelt hello kötegelt azonosítójával.
   
        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches/0"
   
    Az alábbi részlet kimeneti hasonló toohello kell megjelennie:
   
        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:54:42 GMT
        < Content-Length: 509
        <
        {"id":0,"state":"success","log":["\t diagnostics: N/A","\t ApplicationMaster host: 10.0.0.4","\t ApplicationMaster RPC port: 0","\t queue: default","\t start time: 1448063505350","\t final status: SUCCEEDED","\t tracking URL: http://hn0-myspar.lpel1gnnvxne3gwzqkfq5u5uzh.jx.internal.cloudapp.net:8088/proxy/application_1447984474852_0002/","\t user: root","15/11/20 23:52:47 INFO Utils: Shutdown hook called","15/11/20 23:52:47 INFO Utils: Deleting directory /tmp/spark-b72cd2bf-280b-4c57-8ceb-9e3e69ac7d0c"]}* Connection #0 toohost mysparkcluster.azurehdinsight.net left intact
   
    hello most látható kimeneti **állapota: sikeres**, amely javasol hello feladat sikeresen befejeződött.

4. Ha azt szeretné, most törölheti hello kötegelt.
   
        curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/0"
   
    Az alábbi részlet kimeneti hasonló toohello kell megjelennie:
   
        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Sat, 21 Nov 2015 18:51:54 GMT
        < Content-Length: 17
        <
        {"msg":"deleted"}* Connection #0 toohost mysparkcluster.azurehdinsight.net left intact
   
    hello hello kimenet utolsó sora jeleníti meg, hogy hello kötegelt törlése sikeresen megtörtént. Egy feladat törlése futtatása, is hello feladat használhatatlanná teszi. Ha törli egy feladatot, amely már befejeződött, sikeres, vagy ellenkező esetben teljesen törli hello feladatok adatait.

## <a name="using-livy-spark-on-hdinsight-35-clusters"></a>Livy Spark használatával HDInsight 3.5-fürtökön

HDInsight 3.5 fürt, alapértelmezés szerint letiltja a helyi fájl elérési utak tooaccess mintaadatfájlok vagy a JAR-fájlok kivételével. Javasoljuk, toouse hello `wasb://` elérési út helyett tooaccess JAR-fájlok kivételével vagy mintaadatok fájlok hello fürtből. Ha szeretné, hogy toouse helyi elérési utat, hello Ambari konfigurációját ennek megfelelően kell frissíteni. toodo így:

1. Nyissa meg toohello Ambari portal hello fürthöz. hello Ambari webes felhasználói felület érhető el a HDInsight-fürthöz: https://**CLUSTERNAME**. azurehdidnsight.net, ahol CLUSTERNAME-e a fürt hello neve.

2. A bal oldali navigációs hello, kattintson az **Livy**, és kattintson a **Configs**.

3. A **livy alapértelmezett** hello tulajdonságnév hozzáadása `livy.file.local-dir-whitelist` és állítsa be az értéket túl**"/"** tooallow teljes körű hozzáférési toofile rendszer. Ha azt szeretné, hogy tooallow hozzáférés csak tooa adott directory, ad hello elérési toothat directory hello értékként.

## <a name="submitting-livy-jobs-for-a-cluster-within-an-azure-virtual-network"></a>Egy Azure virtuális hálózaton belül fürt Livy feladatok elküldése

Ha egy Azure virtuális hálózaton belül a HDInsight Spark-fürt tooan, közvetlenül kapcsolódhatnak tooLivy hello fürtön. Ebben az esetben hello URL-címet a Livy végpont `http://<IP address of hello headnode>:8998/batches`. Itt **8998** hello port, amelyen Livy futó hello fürt headnode. A nem nyilvános portokon szolgáltatások eléréséhez szükséges további információkért lásd: [a HDInsight Hadoop-szolgáltatás által használt portok](hdinsight-hadoop-port-settings-for-services.md).

## <a name="troubleshooting"></a>Hibaelhárítás

Az alábbiakban néhány problémát, távoli feladat elküldése tooSpark fürtök Livy használatakor mutatjuk be.

### <a name="using-an-external-jar-from-hello-additional-storage-is-not-supported"></a>Egy külső jar a további tárhely hello használata nem támogatott

**Probléma:** Ha a Livy Spark feladat hivatkozik egy külső jar tárfiókból hello további hello-fürthöz tartozó, hello feladat sikertelen lesz.

**Megoldás:** győződjön meg arról, hogy azt szeretné, hogy a toouse érhető el hello alapértelmezett tárolási hello HDInsight-fürthöz társított hello jar.





## <a name="next-step"></a>Következő lépés

* [Az Azure HDInsight hello Apache Spark-fürt erőforrásainak kezelése](hdinsight-apache-spark-resource-manager.md)
* [Apache Spark-fürtön futó feladatok nyomon követése és hibakeresése a HDInsightban](hdinsight-apache-spark-job-debugging.md)

