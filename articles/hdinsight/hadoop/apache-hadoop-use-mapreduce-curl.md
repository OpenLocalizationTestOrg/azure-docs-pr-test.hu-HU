---
title: "Használjon MapReduce és a hadooppal a Hdinsightban - Azure Curl |} Microsoft Docs"
description: "Megtudhatja, hogyan távolról ugyanúgy futtathatják a MapReduce-feladatok a Hadoop on HDInsight használata Curl használatával."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: bc6daf37-fcdc-467a-a8a8-6fb2f0f773d1
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/04/2017
ms.author: larryfr
ms.openlocfilehash: dd3e5904ee21ee74da5adaa65abd7865a82c8b36
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/05/2017
---
# <a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-rest"></a>A HDInsight a többi hadooppal futtatási MapReduce-feladatok

Útmutató a WebHCat REST API-t használja a HDInsight-fürt Hadoop MapReduce-feladatok futtatásához. Curl használatával bemutatják, hogyan kezelheti a HDInsight nyers HTTP-kérelmek használatával MapReduce-feladatok futtatásához.

> [!NOTE]
> Ha már ismeri a Linux-alapú Hadoop-kiszolgálókat használ, de még nem ismeri a HDInsight, tekintse meg a [mit kell tudnia a Linux-based Hadoop on HDInsight](../hdinsight-hadoop-linux-information.md) dokumentum.


## <a id="prereq"></a>Előfeltételek

* A Hadoop on HDInsight-fürt
* [Curl](http://curl.haxx.se/)
* [jq](http://stedolan.github.io/jq/)

## <a id="curl"></a>MapReduce-feladatok használata Curl használatával futtassa

> [!NOTE]
> Használatakor Curl vagy más REST kommunikációt használ a Webhcattel, hitelesítenie kell a kéréseket a HDInsight fürt rendszergazdai felhasználónév és jelszó megadásával. A fürt nevét, amellyel a kérést küld a kiszolgáló URI azonosítójának részeként kell használnia.
>
> Ezen szakasz parancsaiban cserélje le **admin** a felhasználó a fürtön. Cserélje le a **CLUSTERNAME** elemet a fürt nevére. Amikor a rendszer kéri, írja be a felhasználói fiók jelszavát.
>
> A REST API használatával lett biztonságossá [alapszintű hitelesítés](http://en.wikipedia.org/wiki/Basic_access_authentication). Győződjön meg arról, hogy a hitelesítő adatait biztonságos módon küldje a kiszolgáló a HTTPS használatával mindig készítsen kérelmeket.


1. Egy parancssorból a következő paranccsal ellenőrizze, hogy tud-e kapcsolódni a HDInsight-fürthöz:

    ```bash
    curl -u admin -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
    ```

    A következő JSON hasonló választ kap:

        {"status":"ok","version":"v1"}

    Ezen parancs paraméterei a következők:

   * **-u**: azt jelzi, a felhasználónév és a kérés hitelesítéséhez használt jelszó
   * **-G**: azt jelzi, hogy ez a művelet egy GET kérés

   Az URI elejére **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, minden olyan kérelem esetében megegyezik.

2. Elküldeni a MapReduce feladatot, a következő paranccsal:

    ```bash
    curl -u admin -d user.name=admin -d jar=/example/jars/hadoop-mapreduce-examples.jar -d class=wordcount -d arg=/example/data/gutenberg/davinci.txt -d arg=/example/data/CurlOut https://CLUSTERNAME.azurehdinsight.net/templeton/v1/mapreduce/jar
    ```

    Az URI (/ mapreduce/jar) végén közli a WebHCat, hogy a kérelem osztályból jar-fájlra a MapReduce feladatot elindul. Ezen parancs paraméterei a következők:

   * **-d**: `-G` nem használja, így a kérelem az alapértelmezett a POST metódussal. `-d`Megadja a küldött adatértékekkel mellékel a kérelemhez.
    * **User.name**: a parancsot futtató felhasználó
    * **JAR**: osztályt kell tartalmazó jar-fájl helyét futott
    * **osztály**: az osztály, amely a MapReduce logikát tartalmaz
    * **arg**: az argumentumok a MapReduce feladatot kell átadni. Ebben az esetben, a bemeneti szöveg fájl és a kimenethez használt könyvtár

   Ez a parancs visszaadja-e a Feladatazonosítót a feladat állapotának ellenőrzéséhez használható:

       {"id":"job_1415651640909_0026"}

3. A feladat állapotának ellenőrzéséhez használja a következő parancsot:

    ```bash
    curl -G -u admin -d user.name=admin https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
    ```

    Cserélje le a **JOBID** az előző lépésben visszaadott értékkel. Például, ha a visszatérési érték volt `{"id":"job_1415651640909_0026"}`, akkor a JOBID lenne `job_1415651640909_0026`.

    Ha a feladat befejeződött, a visszaküldött állapot `SUCCEEDED`.

   > [!NOTE]
   > A Curl kérelem adja vissza a feladat adatait tartalmazó JSON-dokumentumból. Csak a állapotérték beolvasásához használt Jq.

4. Ha a feladat állapota módosult `SUCCEEDED`, a feladat eredményeinek lekérése Azure Blob Storage tárolóban. A `statusdir` a lekérdezés átadott paraméter tartalmazza a kimeneti fájl helyét. Ebben a példában a hely a következő `/example/curl`. Ez a cím tárolja a feladat kimenete a fürtök alapértelmezett tárolás `/example/curl`.

Listáról, és ezek a fájlok letöltésére a [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli). A BLOB az Azure parancssori felületen munkáról bővebben lásd: a [az Azure CLI 2.0 használatával az Azure Storage](../../storage/common/storage-azure-cli.md#create-and-manage-blobs) dokumentum.

## <a id="nextsteps"></a>Következő lépések

Általános információk a hdinsight MapReduce-feladatok:

* [A HDInsight Hadoop MapReduce használata](hdinsight-use-mapreduce.md)

Más módszerekkel kapcsolatos információk a HDInsight Hadoop dolgozhat:

* [A Hive használata a hdinsight Hadoop](hdinsight-use-hive.md)
* [A Pig használata a HDInsight Hadoop](hdinsight-use-pig.md)

Ebben a cikkben használt a REST-felület kapcsolatos további információkért tekintse meg a [WebHCat hivatkozás](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).