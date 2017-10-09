---
title: a hdinsight - Azure Curl Hadoop Hive aaaUse |} Microsoft Docs
description: "Ismerje meg, hogyan tooremotely submit Pig feladatok tooHDInsight használata Curl használatával."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 6ce18163-63b5-4df6-9bb6-8fcbd4db05d8
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/12/2017
ms.author: larryfr
ms.openlocfilehash: e725829ad2adcf3540f44375e3e87b7cdaebd15e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-hive-queries-with-hadoop-in-hdinsight-using-rest"></a>A többi használatával HDInsight Hadoop Hive-lekérdezések futtatása

[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

Ismerje meg, hogyan toouse hello WebHCat REST API toorun Hive lekérdezi a Hadoop on Azure HDInsight-fürt.

[Curl](http://curl.haxx.se/) használt toodemonstrate, és együttműködését HDInsight nyers HTTP-kérelmek használatával van. Hello [jq](http://stedolan.github.io/jq/) segédprogram használt tooprocess hello JSON-adatokat adott vissza a többi kérelmek.

> [!NOTE]
> Ha ismeri a Linux-alapú Hadoop-kiszolgálókat használ, de új tooHDInsight, tekintse meg a hello [mire van szüksége a Linux-alapú HDInsight Hadoop kapcsolatos tooknow](hdinsight-hadoop-linux-information.md) dokumentum.

## <a id="curl"></a>Hive-lekérdezések futtatása

> [!NOTE]
> Használata cURL vagy más REST kommunikációt használ a Webhcattel, hitelesítenie kell hello kérelmek hello felhasználónevet és jelszót hello HDInsight fürt rendszergazdája által.
>
> Ezen szakasz parancsaiban hello, cserélje le **felhasználónév** hello felhasználói tooauthenticate toohello fürt, és cserélje ki a **jelszó** a hello hello felhasználói fiók jelszavát. Cserélje le **CLUSTERNAME** hello néven a fürt.
>
> hello REST API védelméről [az egyszerű hitelesítés](http://en.wikipedia.org/wiki/Basic_access_authentication). toohelp győződjön meg arról, hogy a hitelesítő adatait biztonságos toohello kiszolgáló küldött, mindig kéréseket HTTP Secure (HTTPS) használatával.

1. A parancssorból a következő parancs tooverify, hogy csatlakozhasson tooyour HDInsight-fürt hello használata:

    ```bash
    curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
    ```

    Egy hasonló toohello választ, a következő szöveg jelenik meg:

        {"status":"ok","version":"v1"}

    Ebben a parancsban használt hello paraméterek a következők:

   * **-u** -hello felhasználónevet és jelszót használt tooauthenticate hello kérelem.
   * **-G** -azt jelzi, hogy a kérelem a GET műveletet.

     hello hello URL-címe, eleje **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, van hello azonos minden olyan kérelem esetében. hello elérési **/status**, mutatja, hogy hello kérés tooreturn WebHCat (más néven Templeton) állapota hello kiszolgáló. Hive hello verzióját is kérheti hello a következő parancs használatával:

    ```bash
    curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/version/hive
    ```

     A kérelem a válasz a következő szöveg hasonló toohello adja vissza:

       {"module": "hive", "version": "0.13.0.2.1.6.0-2103"}

2. A következő nevű tábla toocreate használata hello **log4jLogs**:

    ```bash
    curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="set+hive.execution.engine=tez;DROP+TABLE+log4jLogs;CREATE+EXTERNAL+TABLE+log4jLogs(t1+string,t2+string,t3+string,t4+string,t5+string,t6+string,t7+string)+ROW+FORMAT+DELIMITED+FIELDS+TERMINATED+BY+' '+STORED+AS+TEXTFILE+LOCATION+'/example/data/';SELECT+t4+AS+sev,COUNT(*)+AS+count+FROM+log4jLogs+WHERE+t4+=+'[ERROR]'+AND+INPUT__FILE__NAME+LIKE+'%25.log'+GROUP+BY+t4;" -d statusdir="/example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/hive
    ```

    a következő a kérelem paramétereit hello:

   * **-d** - óta `-G` nem használ, akkor hello kérelem alapértelmezés szerint használt érték toohello POST metódussal. `-d`Megadja a küldött hello adatértékek hello kérelemmel.

     * **User.name** -hello parancsot futtató felhasználónak hello.
     * **végrehajtás** -HiveQL utasítások tooexecute hello.
     * **statusdir** – Ez a feladat állapotát hello hello könyvtár nevével.

     Ezekre az utasításokra hajtsa végre a következő műveletek hello:
   * **DROP TABLE** -Ha hello tábla már létezik, akkor az törlődik.
   * **A külső tábla létrehozása** -táblát hoz létre egy új "külső" struktúra. Külső táblák csak hello tábladefiníció Hive tárolja. hello adatok hello eredeti helyen marad.

     > [!NOTE]
     > Külső táblák kell használni, amikor hello frissíteni az külső forrás alapjául szolgáló adatok toobe várt. Például egy automatizált adatok feltöltési folyamat vagy egy másik MapReduce művelet.
     >
     > A külső tábla eldobása does **nem** törölni hello adatokat, csak a hello tábla definíciójában.

   * **SOR formátum** – hogyan hello adatok formátuma. az egyes naplókon hello mezők vannak szóközzel elválasztva.
   * **AS TEXTFILE helyen tárolt** - Ha hello tárolja (hello példaadatokat/könyvtár) és szövegként tárolt.
   * **Válassza ki** -választja ki az összes sorok számát ahol oszlop **t4** hello értéket tartalmaz **[hiba]**. A jelen nyilatkozat értéket ad vissza, **3** ahány három ezt az értéket tartalmazó sorok.

     > [!NOTE]
     > Figyelje meg, hogy HiveQL utasítások hello szóközt helyébe hello `+` karakter Curl használata esetén. Szóközt, például a hello elválasztó idézőjelek közé zárt értékek nem helyébe kell `+`.

   * **INPUT__FILE__NAME PÉLDÁUL "% 25.log"** - e utasítás korlátok hello keresési tooonly használata fájlok végződése. napló.

     > [!NOTE]
     > Hello `%25` hello URL-kódolású formátum %-os van, így a tényleges feltétele hello `like '%.log'`. hello % toobe URL-címe, többek között kódolhatók, akkor a rendszer egy különleges karakter URL-van.

     Ez a parancs a feladat azonosítója, amely hello feladat állapotának használt toocheck hello kell visszaadnia.

       {"id": "job_1415651640909_0026"}

3. hello feladat, a következő parancs használata hello toocheck hello állapota:

    ```bash
    curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
    ```

    Cserélje le **JOBID** hello előző lépés eredményeképpen visszakapott hello értékkel. Például ha hello visszatérési érték volt `{"id":"job_1415651640909_0026"}`, majd **JOBID** lenne `job_1415651640909_0026`.

    Ha hello feladat befejeződött, hello állapota **sikeres**.

   > [!NOTE]
   > A Curl kérelem hello feladat adatait tartalmazó JavaScript Object Notation (JSON) dokumentum adja vissza. Jq használt tooretrieve csak hello állapotérték.

4. Miután hello feladat hello állapota megváltozott túl**sikeres**, az Azure Blob storage hello feladat eredményeinek hello kérheti le. Hello `statusdir` hello lekérdezéssel átadott paraméter tartalmaz hello hely hello kimeneti fájl; ebben az esetben az **/példa/curl**. Ez a cím hello kimeneti tárol hello **példa/curl** könyvtárat a hello fürtök alapértelmezett tároló.

    Listáról, és ezek a fájlok letöltésére hello [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli). Az Azure Storage hello Azure CLI használatáról további információkért lásd: hello [Azure CLI 2.0 használata Azure Storage](https://docs.microsoft.com/en-us/azure/storage/storage-azure-cli#create-and-manage-blobs) dokumentum.

5. A következő utasítások toocreate nevű új "belső" tábla használata hello **errorLogs**:

    ```bash
    curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="set+hive.execution.engine=tez;CREATE+TABLE+IF+NOT+EXISTS+errorLogs(t1+string,t2+string,t3+string,t4+string,t5+string,t6+string,t7+string)+STORED+AS+ORC;INSERT+OVERWRITE+TABLE+errorLogs+SELECT+t1,t2,t3,t4,t5,t6,t7+FROM+log4jLogs+WHERE+t4+=+'[ERROR]'+AND+INPUT__FILE__NAME+LIKE+'%25.log';SELECT+*+from+errorLogs;" -d statusdir="/example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/hive
    ```

    Ezekre az utasításokra hajtsa végre a következő műveletek hello:

   * **Hozzon létre Ha nem létezik táblázat** -táblázatot hoz létre, ha még nem létezik. A jelen nyilatkozat egy belső tábla, amely hello Hive-adatraktárban tárolja, és teljesen kezeli a Hive hoz létre.

     > [!NOTE]
     > Külső táblák eltérően eldobását egy belső tábla törlése hello, valamint az alapul szolgáló adatokat.

   * **TÁROLT AS ORC** -hello eltárolja optimalizált sor oszlopos (ORC) formátumban. ORC formátuma egy magas optimalizált és hatékony Hive adatainak tárolásához.
   * **ÍRJA FELÜL AZ INSERT... Válassza ki** -sorok kiválaszt hello **log4jLogs** tartalmazó **[hiba]**, majd Beszúrások adatok hello hello be **errorLogs** tábla.
   * **Válassza ki** -kiválaszt minden sor új hello **errorLogs** tábla.

6. Használja a hello Feladatazonosító hello feladat toocheck hello állapotot adott vissza. Ha sikeres, a hello Azure CLI korábban leírt toodownload és hello az eredmények. hello kimeneti tartalmaznia kell a három sort, ezek mindegyike tartalmazhat **[hiba]**.

## <a id="nextsteps"></a>Következő lépések

Általános információk a hdinsight Hive:

* [A Hive használata a hdinsight Hadoop](hdinsight-use-hive.md)

Az egyéb lehetőségeiről a HDInsight Hadoop dolgozhat:

* [A Pig használata a HDInsight Hadoop](hdinsight-use-pig.md)
* [A HDInsight Hadoop MapReduce használata](hdinsight-use-mapreduce.md)

Ha a Hive Tez használ, tekintse meg a hello dokumentumok hibakeresési információkat a következő:

* [A Linux-alapú HDInsight Ambari Tez nézet hello használata](hdinsight-debug-ambari-tez-view.md)

Hello REST API-t Itt további információkért lásd: hello [WebHCat hivatkozás](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference) dokumentum.

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


