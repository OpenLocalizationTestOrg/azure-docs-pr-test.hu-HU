---
title: "Hadoop Sqoop használata a hdinsight - Azure Curl |} Microsoft Docs"
description: "Megtudhatja, hogyan távolról a Sqoop feladatok HDInsight használata Curl használatával való elküldéséhez."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 39798321-78ca-428c-bcfe-322e49af4059
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: 0975aedf58c6e110726dd3308eae5f9ad3907cc7
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="run-sqoop-jobs-with-hadoop-in-hdinsight-with-curl"></a>Sqoop feladatok futtatása a hadooppal a Hdinsightban a Curl
[!INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Megtudhatja, hogyan használja a Curl hdinsight Hadoop-fürthöz Sqoop feladatok futtatásához.

Curl használatával bemutatják, hogyan kezelheti a HDInsight nyers HTTP-kérelmek futtatásához, a figyelheti és a Sqoop feladatot az eredmények visszakeresésére használatával. Ez a módszer a WebHCat REST API használatával (korábbi nevén lépni a Templeton) a HDInsight-fürt által biztosított.

> [!NOTE]
> Ha ismeri a Linux-alapú Hadoop-kiszolgálókat használ, de még nem használta a HDInsight, lásd: [információ a HDInsight használata Linux](hdinsight-hadoop-linux-information.md).
> 
> 

## <a name="prerequisites"></a>Előfeltételek
Ebben a cikkben szereplő lépések elvégzéséhez a következőkre lesz szüksége:

* A Hadoop on HDInsight-fürt (Linux vagy a Windows-alapú)
* [Curl](http://curl.haxx.se/)
* [jq](http://stedolan.github.io/jq/)

## <a name="submit-sqoop-jobs-by-using-curl"></a>Sqoop feladatok elküldéséhez a Curl használatával
> [!NOTE]
> Amikor a Curl vagy más REST kommunikációt használ a WebHCattel, hitelesítenie kell a kéréseket a HDInsight fürt rendszergazdája felhasználónevének és jelszavának megadásával. A fürtnevet a kérések kiszolgálóhoz küldéséhez használt egységes erőforrás-azonosító (URI) részeként is használnia kell.
> 
> Ezen szakasz parancsaiban cserélje le a **USERNAME** elemet a fürthöz hitelesíteni kívánt felhasználóval, és a **PASSWORD** elemet pedig a felhasználói fiók jelszavával. Cserélje le a **CLUSTERNAME** elemet a fürt nevére.
> 
> A REST API védelméről [alapszintű hitelesítés](http://en.wikipedia.org/wiki/Basic_access_authentication) gondoskodik. Mindig biztonságos HTTP-n (HTTPS-en) keresztül kell kéréseket végeznie, hogy a hitelesítő adatait biztonságos módon küldje a kiszolgálóhoz.
> 
> 

1. Egy parancssorból a következő paranccsal ellenőrizze, hogy tud-e kapcsolódni a HDInsight-fürthöz:
   
        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
   
    A következőhöz hasonló választ kell kapnia:
   
        {"status":"ok","version":"v1"}
   
    Ezen parancs paraméterei a következők:
   
   * **-u** - A kérés hitelesítéséhez használt felhasználónév és jelszó.
   * **-G** - Jelzi, hogy ez egy GET kérés.
     
     Az URL-cím, az elején **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, minden olyan kérelem esetében ugyanaz lesz. Az elérési út **/status**, azt jelzi, hogy a kérelem vissza WebHCat (más néven Templeton) állapota a kiszolgálóhoz. 
2. Használja a következő sqoop feladat elküldése:

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d command="export --connect jdbc:sqlserver://SQLDATABASESERVERNAME.database.windows.net;user=USERNAME@SQLDATABASESERVERNAME;password=PASSWORD;database=SQLDATABASENAME --table log4jlogs --export-dir /tutorials/usesqoop/data --input-fields-terminated-by \0x20 -m 1" -d statusdir="wasb:///example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/sqoop

    Ezen parancs paraméterei a következők:

    * **-d** - óta `-G` nem használ, akkor a kérelmet az alapértelmezett a POST metódussal. `-d`Megadja a küldött adatértékekkel mellékel a kérelemhez.

        * **User.name** – a parancsot futtató felhasználónak.

        * **a parancs** -a Sqoop parancs végrehajtásához.

        * **statusdir** -címtárban, ami a feladat állapotát a rendszer úgy tünteti fel.

    Ez a parancs a Feladatazonosítót a feladat állapotának ellenőrzéséhez használható kell visszaadnia.

        {"id":"job_1415651640909_0026"}

1. A feladat állapotának ellenőrzéséhez használja a következő parancsot. Cserélje le **JOBID** az előző lépésben visszaadott értékkel. Például, ha a visszatérési érték volt `{"id":"job_1415651640909_0026"}`, majd **JOBID** lenne `job_1415651640909_0026`.
   
        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
   
    Ha a feladat befejeződött, az állapot nem **sikeres**.
   
   > [!NOTE]
   > A Curl kérelem egy JavaScript Object Notation (JSON) dokumentumot, a feladat; információkat ad vissza csak a állapotérték beolvasásához használt jq.
   > 
   > 
2. Ha a feladat állapota módosult **sikeres**, a feladat eredményeinek lekérése Azure Blob Storage tárolóban. A `statusdir` a lekérdezés átadott paraméter tartalmazza a hely a kimeneti fájl; ebben az esetben az **wasb: / / / Példa/curl**. Ez a cím tárolja a feladat kimenete a **példa/curl** könyvtárába a HDInsight-fürt által használt alapértelmezett tárolót.
   
    Listáról, és ezek a fájlok letöltésére a [Azure CLI](../cli-install-nodejs.md). Például, hogy a fájlok listázása **példa/curl**, használja a következő parancsot:
   
        azure storage blob list <container-name> example/curl
   
    A fájl letöltésére, az alábbi parancsokat használja:
   
        azure storage blob download <container-name> <blob-name> <destination-file>
   
   > [!NOTE]
   > Meg kell adnia a tárfiók nevét, amely tartalmazza a blob használatával a `-a` és `-k` paramétereket, vagy állítsa a **AZURE\_tárolási\_fiók** és **AZURE \_Tárolási\_hozzáférés\_kulcs** környezeti változókat. Lásd: < a href = "a hdinsight-feltöltési-data.md" target = "_blank" További információ.
   > 
   > 

## <a name="limitations"></a>Korlátozások
* Tömeges export - a Linux-alapú HDInsight, a Sqoop összekötő használt Microsoft SQL Server vagy az Azure SQL Database adatainak exportálása jelenleg nem támogatja a tömeges beszúrások.
* Kötegelés - és a Linux-alapú HDInsight együttes használata esetén a `-batch` beszúrása végrehajtásakor kapcsoló, a Sqoop több beszúrás helyett a beszúrási műveletek kötegelése hajt végre.

## <a name="summary"></a>Összefoglalás
Ahogyan az ebben a dokumentumban, használhatja a nyers HTTP-kérelmek futtatásához, a figyelheti és a Sqoop feladat eredményeinek megtekintéséhez a HDInsight-fürt.

A cikk ezt használja a REST-felület további információkért tekintse meg a <a href="https://sqoop.apache.org/docs/1.99.3/RESTAPI.html" target="_blank">Sqoop REST API útmutató</a>.

## <a name="next-steps"></a>Következő lépések
Általános információk a hdinsight Hive:

* [Sqoop használata a HDInsight Hadoop](hdinsight-use-sqoop.md)

Az egyéb lehetőségeiről a HDInsight Hadoop dolgozhat:

* [A Hive használata a hdinsight Hadoop](hdinsight-use-hive.md)
* [A Pig használata a HDInsight Hadoop](hdinsight-use-pig.md)
* [A HDInsight Hadoop MapReduce használata](hdinsight-use-mapreduce.md)

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md




[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md

[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


