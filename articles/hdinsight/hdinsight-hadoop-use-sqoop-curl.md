---
title: a hdinsight - Azure Curl Hadoop Sqoop aaaUse |} Microsoft Docs
description: "Ismerje meg, hogyan nyújt az tooremotely a Sqoop feladatok tooHDInsight használata Curl használatával."
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
ms.openlocfilehash: d9c09a6704ab6c5f48be50ed6d6314ec406df8ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-sqoop-jobs-with-hadoop-in-hdinsight-with-curl"></a>Sqoop feladatok futtatása a hadooppal a Hdinsightban a Curl
[!INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Ismerje meg, hogyan toouse Curl toorun Sqoop feladatok egy hadoop a Hdinsightban fürt.

Curl, és együttműködését HDInsight nyers HTTP-kérelmek toorun, a figyelő és a Sqoop feladatok eredményeinek lekérése hello segítségével használt toodemonstrate. Ez a módszer hello WebHCat REST API-t (korábbi nevén lépni a Templeton) a HDInsight-fürt által biztosított használatával.

> [!NOTE]
> Ha ismeri a Linux-alapú Hadoop-kiszolgálókat használ, de új tooHDInsight, tekintse meg a [információ a HDInsight használata Linux](hdinsight-hadoop-linux-information.md).
> 
> 

## <a name="prerequisites"></a>Előfeltételek
toocomplete hello cikkben leírt lépéseket, szüksége lesz a következő hello:

* A Hadoop on HDInsight-fürt (Linux vagy a Windows-alapú)
* [Curl](http://curl.haxx.se/)
* [jq](http://stedolan.github.io/jq/)

## <a name="submit-sqoop-jobs-by-using-curl"></a>Sqoop feladatok elküldéséhez a Curl használatával
> [!NOTE]
> Használata Curl vagy más REST kommunikációt használ a Webhcattel, hitelesítenie kell hello kérelmek hello felhasználónevet és jelszót hello HDInsight fürt rendszergazdája által. Hello egységes erőforrás-azonosító (URI) részeként használt toosend hello kérelmek toohello server hello fürtnév is kell használnia.
> 
> Ezen szakasz parancsaiban hello, cserélje le **felhasználónév** hello felhasználói tooauthenticate toohello fürt, és cserélje ki a **jelszó** a hello hello felhasználói fiók jelszavát. Cserélje le **CLUSTERNAME** hello néven a fürt.
> 
> hello REST API védelméről [az egyszerű hitelesítés](http://en.wikipedia.org/wiki/Basic_access_authentication). Mindig ügyeljen kérelmek biztonságos HTTP (HTTPS) toohelp győződjön meg arról, hogy a hitelesítő adatait biztonságos módon küldje toohello kiszolgáló használatával.
> 
> 

1. A parancssorból a következő parancs tooverify, hogy csatlakozhasson tooyour HDInsight-fürt hello használata:
   
        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
   
    Egy hasonló toohello következő választ kell kapnia:
   
        {"status":"ok","version":"v1"}
   
    Ebben a parancsban használt hello paraméterek a következők:
   
   * **-u** -hello felhasználónevet és jelszót használt tooauthenticate hello kérelem.
   * **-G** - Jelzi, hogy ez egy GET kérés.
     
     hello hello URL-címe, eleje **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, minden olyan kérelem esetében hello azonos lesz. hello elérési **/status**, mutatja, hogy hello kérés tooreturn WebHCat (más néven Templeton) állapota hello kiszolgáló. 
2. A következő toosubmit a sqoop feladat hello használata:

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d command="export --connect jdbc:sqlserver://SQLDATABASESERVERNAME.database.windows.net;user=USERNAME@SQLDATABASESERVERNAME;password=PASSWORD;database=SQLDATABASENAME --table log4jlogs --export-dir /tutorials/usesqoop/data --input-fields-terminated-by \0x20 -m 1" -d statusdir="wasb:///example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/sqoop

    Ebben a parancsban használt hello paraméterek a következők:

    * **-d** - óta `-G` nem használ, akkor hello kérelem alapértelmezés szerint használt érték toohello POST metódussal. `-d`Megadja a küldött hello adatértékek hello kérelemmel.

        * **User.name** -hello parancsot futtató felhasználónak hello.

        * **a parancs** -Sqoop parancs tooexecute hello.

        * **statusdir** – Ez a feladat állapotát hello hello könyvtár lesz írva.

    Ez a parancs a feladat azonosítója, amely hello feladat állapotának használt toocheck hello kell visszaadnia.

        {"id":"job_1415651640909_0026"}

1. hello feladat, a következő parancs használata hello toocheck hello állapota. Cserélje le **JOBID** hello előző lépés eredményeképpen visszakapott hello értékkel. Például ha hello visszatérési érték volt `{"id":"job_1415651640909_0026"}`, majd **JOBID** lenne `job_1415651640909_0026`.
   
        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
   
    Ha hello feladat befejeződött, hello állapotban lesz **sikeres**.
   
   > [!NOTE]
   > A Curl kérelem egy JavaScript Object Notation (JSON) dokumentumot hello feladat; információkat ad vissza jq használt tooretrieve csak hello állapotérték.
   > 
   > 
2. Miután hello feladat hello állapota megváltozott túl**sikeres**, az Azure Blob storage hello feladat eredményeinek hello kérheti le. Hello `statusdir` hello lekérdezéssel átadott paraméter tartalmaz hello hely hello kimeneti fájl; ebben az esetben az **wasb: / / / Példa/curl**. Ez a cím hello feladat eredményének hello tárol hello **példa/curl** hello alapértelmezett tárolót a HDInsight-fürt által használt könyvtárába.
   
    Listáról, és ezek a fájlok letöltésére hello [Azure CLI](../cli-install-nodejs.md). Például adatbázisfájlok toolist **példa/curl**, a következő parancs hello használata:
   
        azure storage blob list <container-name> example/curl
   
    egy fájl toodownload hello következő használja:
   
        azure storage blob download <container-name> <blob-name> <destination-file>
   
   > [!NOTE]
   > Meg kell adnia hello tárfiók neve, amely tartalmazza a hello blob hello segítségével `-a` és `-k` paramétereket, vagy a set hello **AZURE\_tárolási\_fiók** és **AZURE\_tárolási\_hozzáférés\_kulcs** környezeti változókat. Lásd: < a href = "a hdinsight-feltöltési-data.md" target = "_blank" További információ.
   > 
   > 

## <a name="limitations"></a>Korlátozások
* Tömeges export - a Linux-alapú HDInsight, hello Sqoop használt tooexport adatok tooMicrosoft SQL Server vagy az Azure SQL Database jelenleg nem támogatja a tömeges beszúrások.
* Kötegelés – a Linux-alapú hdinsight eszközzel, hello használatakor `-batch` Beszúrások végrehajtásakor kapcsoló, a Sqoop hajt végre több Beszúrások hello beszúrási műveletek kötegelése helyett.

## <a name="summary"></a>Összefoglalás
Ahogyan az a dokumentum, egy nyers HTTP-kérelem toorun, a figyelő és hello eredmények megtekintése a Sqoop feladatok használhatja a HDInsight-fürtre.

A cikk ezt használja a hello REST-felület további információkért lásd: hello <a href="https://sqoop.apache.org/docs/1.99.3/RESTAPI.html" target="_blank">Sqoop REST API útmutató</a>.

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


