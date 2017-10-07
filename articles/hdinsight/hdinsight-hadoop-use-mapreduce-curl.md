---
title: "aaaUse MapReduce és a hadooppal a Hdinsightban - Azure Curl |} Microsoft Docs"
description: "Ismerje meg, hogyan tooremotely hibaüzenettel MapReduce-feladatok Hadoop on HDInsight használata Curl használatával."
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
ms.date: 07/12/2017
ms.author: larryfr
ms.openlocfilehash: 16920205bacf9699f88090568099e0508a172b3b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-rest"></a>A HDInsight a többi hadooppal futtatási MapReduce-feladatok

Ismerje meg, hogyan toouse hello WebHCat REST API toorun MapReduce a hadoop on HDInsight-fürt feladatokat. Curl, és együttműködését HDInsight nyers HTTP kérelmeket toorun MapReduce-feladatok használatával használt toodemonstrate.

> [!NOTE]
> Ha már ismeri a Linux-alapú Hadoop-kiszolgálókat használ, de új tooHDInsight áll, tekintse meg a hello [mire van szüksége a HDInsight Linux-alapú Hadooppal kapcsolatos tooknow](hdinsight-hadoop-linux-information.md) dokumentum.


## <a id="prereq"></a>Előfeltételek

* A Hadoop on HDInsight-fürt
* [Curl](http://curl.haxx.se/)
* [jq](http://stedolan.github.io/jq/)

## <a id="curl"></a>MapReduce-feladatok használata Curl használatával futtassa

> [!NOTE]
> Használatakor Curl vagy más REST kommunikációt használ a Webhcattel, hitelesítenie kell hello kérelmek hello HDInsight fürt rendszergazdai felhasználónév és jelszó megadásával. Hello fürtnév hello használt toosend hello kérelmek toohello kiszolgáló URI részeként kell használnia.
>
> Ezen szakasz parancsaiban hello, cserélje le **felhasználónév** hello felhasználói tooauthenticate toohello fürttel, és **jelszó** a hello hello felhasználói fiók jelszavát. Cserélje le **CLUSTERNAME** hello néven a fürt.
>
> hello REST API által védett használatával [alapszintű hitelesítés](http://en.wikipedia.org/wiki/Basic_access_authentication). Mindig ügyeljen kérelmek HTTPS tooensure, hogy a hitelesítő adatait biztonságos módon küldje toohello kiszolgáló használatával.


1. A parancssorból a következő parancs tooverify, hogy csatlakozhasson tooyour HDInsight-fürt hello használata:

    ```bash
    curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
    ```

    A válasz a következő JSON hasonló toohello kell megjelennie:

        {"status":"ok","version":"v1"}

    Ebben a parancsban használt hello paraméterek a következők:

   * **-u**: azt jelzi, hogy hello felhasználónevet és jelszót használt tooauthenticate hello kérelem
   * **-G**: azt jelzi, hogy ez a művelet egy GET kérés

     URI, hello eleje hello **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, van hello azonos minden olyan kérelem esetében.

2. toosubmit MapReduce feladatot, a következő parancs hello használata:

    ```bash
    curl -u USERNAME:PASSWORD -d user.name=USERNAME -d jar=/example/jars/hadoop-mapreduce-examples.jar -d class=wordcount -d arg=/example/data/gutenberg/davinci.txt -d arg=/example/data/CurlOut https://CLUSTERNAME.azurehdinsight.net/templeton/v1/mapreduce/jar
    ```

    hello (vagy mapreduce/jar) URI hello végét közli a WebHCat, hogy a kérelem osztályból jar-fájlra a MapReduce feladatot elindul. Ebben a parancsban használt hello paraméterek a következők:

   * **-d**: `-G` nem használja, így hello kérelem alapértelmezés szerint használt érték toohello POST metódussal. `-d`Megadja a küldött hello adatértékek hello kérelemmel.
    * **User.name**: hello hello parancsot futtató felhasználó
    * **JAR**: hello jar-fájlt tartalmazó osztály toobe hello helyét futott
    * **osztály**: hello hello MapReduce programot tartalmazó osztály
    * **arg**: hello argumentumok toobe átadott toohello MapReduce feladatot. Ebben az esetben hello adjon meg szöveget a fájl- és hello directory hello kimenethez használt

     Ez a parancs visszaadja-e a feladat azonosítója, amely használt toocheck hello hello feladat állapota:

       {"id": "job_1415651640909_0026"}

3. hello feladat, a következő parancs használata hello toocheck hello állapota:

    ```bash
    curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
    ```

    Cserélje le a hello **JOBID** hello előző lépés eredményeképpen visszakapott hello értékkel. Például ha hello visszatérési érték volt `{"id":"job_1415651640909_0026"}`, majd JOBID lenne hello `job_1415651640909_0026`.

    Ha hello folyamat befejeződik, hello visszaküldött állapot `SUCCEEDED`.

   > [!NOTE]
   > A Curl kérelem hello feladat adatait tartalmazó JSON-dokumentumból adja vissza. Jq használt tooretrieve csak hello állapotérték.

4. Ha hello állapota hello feladat túl megváltozott,`SUCCEEDED`, az Azure Blob storage hello feladat eredményeinek hello kérheti le. Hello `statusdir` hello lekérdezéssel átadott paraméter hello kimeneti fájl helye hello tartalmazza. Ebben a példában hello helye `/example/curl`. Ez a cím hello feladat eredményének hello tárol hello fürtök alapértelmezett tárolás `/example/curl`.

Listáról, és ezek a fájlok letöltésére hello [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli). A BLOB az Azure CLI hello munkáról bővebben lásd: hello [Using hello Azure CLI 2.0 az Azure Storage](../storage/common/storage-azure-cli.md#create-and-manage-blobs) dokumentum.

## <a id="nextsteps"></a>Következő lépések

Általános információk a hdinsight MapReduce-feladatok:

* [A HDInsight Hadoop MapReduce használata](hdinsight-use-mapreduce.md)

Más módszerekkel kapcsolatos információk a HDInsight Hadoop dolgozhat:

* [A Hive használata a hdinsight Hadoop](hdinsight-use-hive.md)
* [A Pig használata a HDInsight Hadoop](hdinsight-use-pig.md)

Ebben a cikkben használt hello REST-felület kapcsolatos további információkért lásd: hello [WebHCat hivatkozás](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).
