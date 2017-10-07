---
title: "a HDInsight - Azure többi Hadoop Pig aaaUse |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse REST toorun Pig Latin feladatok egy hadoop Azure hdinsight fürt."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: ed5e10d1-4f47-459c-a0d6-7ff967b468c4
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: 760139e3caad9103d8c9d34e7f548d476014b5ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-pig-jobs-with-hadoop-on-hdinsight-by-using-rest"></a>Pig-feladatokhoz a Hadoop on HDInsight használatával futtassa REST

[!INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Ismerje meg, hogyan toorun Pig Latin feladatok azáltal, hogy a többi kérelmek tooan Azure HDInsight-fürthöz. Curl használt toodemonstrate, és együttműködését HDInsight hello WebHCat REST API-t a rendszer.

> [!NOTE]
> Ha ismeri a Linux-alapú Hadoop-kiszolgálókat használ, de új tooHDInsight, tekintse meg a [Linux-alapú HDInsight tippek](hdinsight-hadoop-linux-information.md).

## <a id="prereq"></a>Előfeltételek

* (A HDInsight Hadoop) Azure HDInsight-fürtök (Linux- vagy Windows-alapú)

  > [!IMPORTANT]
  > Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* [Curl](http://curl.haxx.se/)

* [jq](http://stedolan.github.io/jq/)

## <a id="curl"></a>Pig-feladatokhoz Curl használatával futtassa

> [!NOTE]
> hello REST API védelméről [alapszintű hitelesítés](http://en.wikipedia.org/wiki/Basic_access_authentication). Mindig végezzen kérelmeket, hogy a hitelesítő adatait biztonságos módon küldje toohello kiszolgáló biztonságos HTTP (HTTPS) tooensure.
>
> Ha ez a szakasz hello parancsokkal, cserélje le `USERNAME` hello felhasználói tooauthenticate toohello fürt, és cserélje ki a `PASSWORD` hello jelszóval hello felhasználói fiók. Cserélje le `CLUSTERNAME` hello néven a fürt.
>


1. A parancssorból a következő parancs tooverify, hogy csatlakozhasson tooyour HDInsight-fürt hello használata:

    ```bash
    curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
    ```

    A következő JSON-válasz hello kell megjelennie:

        {"status":"ok","version":"v1"}

    Ebben a parancsban használt hello paraméterek a következők:

    * **-u**: hello felhasználónevet és jelszót használt tooauthenticate hello kérelem
    * **-G**: azt jelzi, hogy a kérelem a GET kérés

     hello hello URL-címe, eleje **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, van hello azonos minden olyan kérelem esetében. hello elérési **/status**, mutatja, hogy hello kérés WebHCat (más néven Templeton) állapotának tooreturn hello hello kiszolgáló.

2. A következő kód toosubmit Pig Latin feladat toohello fürt hello használata:

    ```bash
    curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="LOGS=LOAD+'/example/data/sample.log';LEVELS=foreach+LOGS+generate+REGEX_EXTRACT($0,'(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)',1)+as+LOGLEVEL;FILTEREDLEVELS=FILTER+LEVELS+by+LOGLEVEL+is+not+null;GROUPEDLEVELS=GROUP+FILTEREDLEVELS+by+LOGLEVEL;FREQUENCIES=foreach+GROUPEDLEVELS+generate+group+as+LOGLEVEL,COUNT(FILTEREDLEVELS.LOGLEVEL)+as+count;RESULT=order+FREQUENCIES+by+COUNT+desc;DUMP+RESULT;" -d statusdir="/example/pigcurl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/pig
    ```

    Ebben a parancsban használt hello paraméterek a következők:

    * **-d**: mivel `-G` nem használ, akkor hello kérelem alapértelmezés szerint használt érték toohello POST metódussal. `-d`Megadja a küldött hello adatértékek hello kérelemmel.

    * **User.name**: hello hello parancsot futtató felhasználó
    * **végrehajtás**: hello Pig Latin utasítások tooexecute
    * **statusdir**: Ez a feladat állapotát hello hello könyvtár nevével

    > [!NOTE]
    > Figyelje meg, hogy a Pig Latin utasításokban hello szóközöket helyébe hello `+` karakter Curl használata esetén.

    Ez a parancs visszaadja-e a feladat azonosítója, amely lehet például hello feladat, használt toocheck hello állapota:

        {"id":"job_1415651640909_0026"}

3. hello feladat, a következő parancs használata hello toocheck hello állapota

     ```bash
    curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
    ```

     Cserélje le `JOBID` hello előző lépés eredményeképpen visszakapott hello értékkel. Például ha hello visszatérési érték volt `{"id":"job_1415651640909_0026"}`, majd `JOBID` van `job_1415651640909_0026`.

    Ha hello feladat befejeződött, hello állapota **sikeres**.

    > [!NOTE]
    > A Curl kérelmeket értéket ad vissza a JavaScript Object Notation (JSON)-dokumentum található információkat hello feladat, illetve jq használt tooretrieve csak hello állapotérték.

## <a id="results"></a>Az eredmények megtekintése

Ha hello állapota hello feladat túl megváltozott,**sikeres**, hello feladat eredményeinek hello kérheti le. Hello `statusdir` hello lekérdezéssel átadott paraméter tartalmaz hello hely hello kimeneti fájl; ebben az esetben az `/example/pigcurl`.

HDInsight a hello alapértelmezett adatokat tároló Azure Storage vagy az Azure Data Lake Store is használhatja. Nincsenek különböző módokon tooget hello adatait attól függően, melyiket használja. További információ a hello hello tárolási című szakaszában talál [Linux-alapú HDInsight-információk](hdinsight-hadoop-linux-information.md#hdfs-azure-storage-and-data-lake-store) dokumentum.

## <a id="summary"></a>Summary (Összefoglalás)

Ahogyan az a dokumentum, egy nyers HTTP-kérelem toorun, a figyelő és hello eredmények megtekintése a Pig feladatot használhatja a HDInsight-fürt.

A cikk ezt használja a hello REST-felület kapcsolatos további információkért lásd: hello [WebHCat hivatkozás](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).

## <a id="nextsteps"></a>Következő lépések

A Pig általános információk a HDInsight:

* [A Pig használata a HDInsight Hadoop](hdinsight-use-pig.md)

Más módszerekkel kapcsolatos információk a HDInsight Hadoop dolgozhat:

* [A Hive használata a hdinsight Hadoop](hdinsight-use-hive.md)
* [A HDInsight Hadoop MapReduce használata](hdinsight-use-mapreduce.md)
