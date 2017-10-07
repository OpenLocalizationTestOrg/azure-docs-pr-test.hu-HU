---
title: "a Hive hdinsight (Hadoop) - Azure aaaUse Ambari nézetek toowork |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello a webes böngésző toosubmit Hive-lekérdezéseket a Hive nézet. hello Hive View hello Ambari webes felhasználói felületén megadott a Linux-alapú HDInsight-fürt része."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 1abe9104-f4b2-41b9-9161-abbc43de8294
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: f9a77b652e70d34a0ff9165fbb8c2e16d3401ae0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-hive-view-with-hadoop-in-hdinsight"></a>A HDInsight Hadoop Hive View hello használata

[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

Ismerje meg, hogyan toorun Hive lekérdezi az Ambari Hive nézet használatával. Ambari egy felügyeleti és figyelési segédprogram Linux-alapú HDInsight-fürtökkel. Ambari keresztül elérhető hello szolgáltatások egyik webes felhasználói Felületet, amely használt toorun Hive-lekérdezéseket.

> [!NOTE]
> Ambari rendelkezik számos lényeges képességét, hogy ez a dokumentum nem ismerteti. További információkért lásd: [kezelése HDInsight-fürtök hello Ambari webes felhasználói felület használatával](hdinsight-hadoop-manage-ambari.md).

## <a name="prerequisites"></a>Előfeltételek

* A Linux-alapú HDInsight-fürtöt. A fürt létrehozása információkért lásd: [Ismerkedés a Linux-alapú HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).

> [!IMPORTANT]
> hello jelen dokumentumban leírt lépések egy HDInsight-fürt által használt Linux igényelnek. Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="open-hello-hive-view"></a>Nyissa meg a hello Hive nézete

Az Ambari nézetek teheti a hello Azure-portálon; Válassza ki a HDInsight-fürt majd **Ambari nézetek** a hello **Gyorshivatkozások** szakasz.

![Gyorshivatkozások szakasz hello portál](./media/hdinsight-hadoop-use-hive-ambari-view/quicklinks.png)

Nézetek hello listáról válassza ki a hello __Hive View__.

![hello kijelölt Hive nézete](./media/hdinsight-hadoop-use-hive-ambari-view/select-hive-view.png)

> [!NOTE]
> Ambari elérésekor felszólító tooauthenticate toohello hely áll. Adja meg a Üdvözöljük a rendszergazdákat (alapértelmezett `admin`) fiók nevét és jelszavát létrehozásakor használt hello fürt.

A kép a következő lap hasonló toohello kell megjelennie:

![Hello lekérdezés munkalap hello Hive nézet képe](./media/hdinsight-hadoop-use-hive-ambari-view/ambari-hive-view.png)

## <a name="hivequery"></a>A lekérdezés futtatása

toorun hive-lekérdezések használata hello hello Hive nézete a következő lépéseket.

1. A hello __lekérdezés__ lapra, illessze be a következő HiveQL utasítások hello munkalapra hello:

    ```hiveql
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION '/example/data/';
    SELECT t4 AS sev, COUNT(*) AS cnt FROM log4jLogs WHERE t4 = '[ERROR]' GROUP BY t4;
    ```

    Ezekre az utasításokra hajtsa végre a következő műveletek hello:

   * `DROP TABLE`-Hello tábla és törlése hello adatfájlt, abban az esetben hello tábla már létezik.

   * `CREATE EXTERNAL TABLE`-Táblát hoz létre egy új "external" struktúra.
   Külső táblák csak hello tábladefiníció Hive tárolja. hello adatok hello eredeti helyen marad.

   * `ROW FORMAT`-Hello adatok formázását. Ebben az esetben az egyes naplókon hello mezők szóközzel elválasztva.

   * `STORED AS TEXTFILE LOCATION`-Hello adatok tárolására, és szövegként tárolt.

   * `SELECT`-Kijelöli az adott oszlop t4 hello értéket [hiba] tartalmaz minden sorok számát.

     > [!NOTE]
     > Külső táblák kell használni, amikor hello frissíteni az külső forrás alapjául szolgáló adatok toobe várt. Például egy automatizált adatfeltöltés eljárásra, vagy egy másik MapReduce művelethez. A külső tábla eldobása does *nem* törölni hello adatokat, csak a hello tábla definíciójában.

    > [!IMPORTANT]
    > Hagyja hello __adatbázis__ kiválasztottat __alapértelmezett__. e dokumentumban szereplő példák hello hello alapértelmezett adatbázis tartalmazza a hdinsight eszközzel használható.

2. toostart hello lekérdezés használata hello **Execute** hello munkalap gombra. Narancssárga és hello szövegének változásait túl változik**leállítása**.

3. Miután hello lekérdezés befejeződött, hello **eredmények** lap hello művelet hello eredményeit jeleníti meg. a következő szöveg hello hello hello lekérdezés eredményében:

        sev       cnt
        [ERROR]   3

    Hello **naplók** lapon használt tooview hello naplózási információk hello feladat által létrehozott lehet.

   > [!TIP]
   > Hello **-eredményeket menteni** hello felső legördülő párbeszédpanelje hello a bal oldali **lekérdezési folyamat eredményei** szakasz lehetővé teszi a toodownload vagy -eredményeket menteni.

4. Válassza ki a négy hello a lekérdezés, majd válassza ki **Execute**. Figyelje meg, hogy nincsenek eredmény hello feladat befejezésekor. Hello segítségével **Execute** hello lekérdezés részeként gomb kiválasztott csak futtatása hello kijelölt utasításokat. Ebben az esetben a hello kijelölés nem tartozik hello végső utasítás, amely sorok hello táblából. Ha csak a sor választja, és használjon **Execute**, hello várt eredményeket kell megjelennie.

5. a munkalap tooadd hello használata **új munkalapra lesznek** hello hello alján gomb **Lekérdezésszerkesztő**. Adja meg a következő HiveQL utasítások hello hello új munkalapon:

    ```hiveql
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]';
    ```

  Ezekre az utasításokra hajtsa végre a következő műveletek hello:

   * **Hozzon létre Ha nem létezik táblázat** -táblázatot hoz létre, ha még nem létezik. Hello óta **külső** kulcsszó nem használható, egy belső tábla jön létre. Egy belső tábla hello Hive-adatraktárban tárolja, és Hive teljesen kezeli. Külső táblák eltérően eldobását egy belső tábla törlése hello, valamint az alapul szolgáló adatokat.

   * **TÁROLT AS ORC** -hello eltárolja optimalizált sor oszlopos (ORC) formátumban. ORC formátuma egy magas optimalizált és hatékony Hive adatainak tárolásához.

   * **ÍRJA FELÜL AZ INSERT... Válassza ki** -sorok kiválaszt hello **log4jLogs** tartalmazó `[ERROR]`, majd beszúrása hello hello az adatok és **errorLogs** tábla.

     Használjon hello **Execute** toorun gombra a lekérdezés. Hello **eredmények** lap tartalmaz adatokat, amikor hello lekérdezés nulla sort adja vissza. hello kell állapota **sikeres** hello lekérdezés befejeződése után.

### <a name="visual-explain"></a>Visual ismertetik.

hello lekérdezésterv, jelölje be hello a képi megjelenítés toodisplay **Visual ismertetik** alatt hello munkalap fülre.

Hello **Visual ismertetik** hello lekérdezés lehet, az összetett lekérdezések hello folyamata hasznos információkat. Ez a nézet a szöveges megfelelőjét hello segítségével tekintheti **magyarázat** hello Lekérdezésszerkesztő gombjára.

### <a name="tez-ui"></a>Tez felhasználói felület

toodisplay Tez felhasználói felület hello hello lekérdezés, jelölje be hello **Tez** alatt hello munkalap fülre.

> [!IMPORTANT]
> Tez, ezért nem használható tooresolve összes. Több lekérdezést orvosolni tudja Tez használata nélkül. 

Ha a Tez nem használt tooresolve hello lekérdezés, hello irányított aciklikus diagramhoz (DAG) jelenik meg. Ha a lekérdezések már futott az elmúlt hello, vagy a hibakeresési hello Tez folyamat kívánja tooview hello DAG, használja hello [Tez nézet](hdinsight-debug-ambari-tez-view.md) helyette.

## <a name="view-job-history"></a>Feladatelőzmények megtekintése

Hello __feladatok__ lapon Hive-lekérdezések előzményeit jeleníti meg.

![Hello feladatelőzményekben képe](./media/hdinsight-hadoop-use-hive-ambari-view/job-history.png)

## <a name="database-tables"></a>Adatbázistáblák

Használhatja a hello __táblák__ Hive adatbázisban lévő táblák toowork fülre.

![Hello táblák lap képe](./media/hdinsight-hadoop-use-hive-ambari-view/tables.png)

## <a name="saved-queries"></a>Lekérdezések

Hello lekérdezés lap, a lekérdezések opcionálisan mentheti. Miután menti, felhasználhatja hello hello lekérdezést __mentett lekérdezések__ fülre.

![Lekérdezések lap képe](./media/hdinsight-hadoop-use-hive-ambari-view/saved-queries.png)

## <a name="user-defined-functions"></a>Felhasználó által definiált függvények

Hive is kiterjeszthető felhasználói függvény (UDF) keresztül. Egy UDF lehetővé teszi tooimplement funkcionalitással kapcsolatban, vagy nem könnyen modellezve a HiveQL logika.

hello UDF lapon hello Hive View hello tetején toodeclare lehetővé teszi, és mentse a felhasználó által megadott függvények készlete. A felhasználó által megadott függvények használhatók hello **Lekérdezésszerkesztő**.

![Az UDF lap képe](./media/hdinsight-hadoop-use-hive-ambari-view/user-defined-functions.png)

Egy UDF toohello Hive nézet hozzáadása után egy **helyezze be a felhasználó által megadott függvények** hello hello alján megjelenik gomb **Lekérdezésszerkesztő**. Látható értesítések valamelyikének kiválasztásakor Ez a bejegyzés a legördülő listából válassza ki a felhasználó által megadott függvények hello Hive View definiált hello. Egy UDF választva a rendszer hozzáadja HiveQL utasítások tooyour lekérdezés tooenable hello UDF.

Ha például egy UDF definiált a következő tulajdonságai hello:

* Az erőforrásnév: myudfs

* Erőforrás elérési útja: /myudfs.jar

* Az UDF-name: myawesomeudf

* Az UDF osztálynév: com.myudfs.Awesome

Hello segítségével **helyezze be a felhasználó által megadott függvények** gomb megjelenik egy bejegyzés nevű **myudfs**, egy másik legördülő minden UDF definiálva az adott erőforráshoz. Ebben az esetben **myawesomeudf**. Ez a bejegyzés választva a rendszer hozzáadja a következő hello lekérdezés toohello elejére hello:

```hiveql
add jar /myudfs.jar;
create temporary function myawesomeudf as 'com.myudfs.Awesome';
```

A lekérdezés hello UDF használhatja. Például: `SELECT myawesomeudf(name) FROM people;`.

A felhasználó által megadott függvények használata a HDInsight Hive további információkért tekintse meg a következő dokumentumok hello:

* [Python használata a Hive és a Pig a Hdinsightban](hdinsight-python.md)
* [Hogyan tooadd egy egyéni Hive UDF tooHDInsight](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/14/how-to-add-custom-hive-udfs-to-hdinsight.aspx)

## <a name="hive-settings"></a>Hive-beállítások

Beállítások is lehet használt toochange különböző Hive-beállításokat. Például módosítása hello végrehajtó motorja a Hive Tez (hello alapértelmezett) tooMapReduce.

## <a id="nextsteps"></a>Következő lépések

Általános információk a hdinsight Hive:

* [A Hive használata a hdinsight Hadoop](hdinsight-use-hive.md)

Az egyéb lehetőségeiről a HDInsight Hadoop dolgozhat:

* [A Pig használata a HDInsight Hadoop](hdinsight-use-pig.md)
* [A HDInsight Hadoop MapReduce használata](hdinsight-use-mapreduce.md)
