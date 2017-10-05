---
title: "Az Ambari nézetek használhatja a Hive hdinsight (Hadoop) - Azure |} Microsoft Docs"
description: "Útmutató a Hive View webböngészőből elküldeni a Hive-lekérdezéseket. A Hive nézet az Ambari webes felhasználói felületén megadott a Linux-alapú HDInsight-fürt része."
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
ms.openlocfilehash: 80df3da4d62feb814ea2dd97c96e57954093c5c5
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="use-the-hive-view-with-hadoop-in-hdinsight"></a>A Hive nézet használata a hadooppal a Hdinsightban

[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

Megtudhatja, hogyan futtathat Hive-lekérdezéseket Ambari Hive nézet használatával. Ambari egy felügyeleti és figyelési segédprogram Linux-alapú HDInsight-fürtökkel. Az Ambari keresztül elérhető szolgáltatások egyik webes felhasználói Felületet, amely segítségével futtathat Hive-lekérdezéseket.

> [!NOTE]
> Ambari rendelkezik számos lényeges képességét, hogy ez a dokumentum nem ismerteti. További információkért lásd: [kezelése HDInsight-fürtök az Ambari webes felhasználói felület használatával](hdinsight-hadoop-manage-ambari.md).

## <a name="prerequisites"></a>Előfeltételek

* A Linux-alapú HDInsight-fürtöt. A fürt létrehozása információkért lásd: [Ismerkedés a Linux-alapú HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).

> [!IMPORTANT]
> A jelen dokumentumban leírt lépések egy HDInsight-fürt által használt Linux igényelnek. A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="open-the-hive-view"></a>Nyissa meg a Hive nézete

Az Ambari nézetek is az Azure portálról; Válassza ki a HDInsight-fürt majd **Ambari nézetek** a a **Gyorshivatkozások** szakasz.

![a portál Gyorshivatkozások szakasz](./media/hdinsight-hadoop-use-hive-ambari-view/quicklinks.png)

Válassza ki a listáról a nézetek, a __Hive View__.

![A kiválasztott Hive nézete](./media/hdinsight-hadoop-use-hive-ambari-view/select-hive-view.png)

> [!NOTE]
> Ambari elérésekor a rendszer kéri, hogy a hely hitelesítést. Adja meg a rendszergazdai (alapértelmezett `admin`) nevét és jelszavát, a fürt létrehozásakor használt fiókot.

Az alábbi képen hasonló lap kell megjelennie:

![A lekérdezés munkalap a Hive nézet képe](./media/hdinsight-hadoop-use-hive-ambari-view/ambari-hive-view.png)

## <a name="hivequery"></a>A lekérdezés futtatása

Hive-lekérdezések futtatásához használja a következő lépéseket a Hive nézetből.

1. Az a __lekérdezés__ lapon, a következő hiveql illessze be a munkalapra:

    ```hiveql
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION '/example/data/';
    SELECT t4 AS sev, COUNT(*) AS cnt FROM log4jLogs WHERE t4 = '[ERROR]' GROUP BY t4;
    ```

    Ezekre az utasításokra hajtsa végre a következő műveleteket:

   * `DROP TABLE`-Törli a táblázat és az adatfájl, abban az esetben, ha a tábla már létezik.

   * `CREATE EXTERNAL TABLE`-Táblát hoz létre egy új "external" struktúra.
   Külső táblák csak a tábladefiníció Hive tárolja. Az adatok marad az eredeti helyen.

   * `ROW FORMAT`-Az adatok formázását. Ebben az esetben a mezőket az egyes naplókon szóközzel elválasztva.

   * `STORED AS TEXTFILE LOCATION`-Az adatok tárolására, és szövegként tárolt.

   * `SELECT`-Kijelöli az adott oszlop t4 értéke [hiba] összes sorok számát.

     > [!NOTE]
     > Külső táblák kell használni, amikor külső forrásból frissítenie kell az alapul szolgáló adatokat várt. Például egy automatizált adatfeltöltés eljárásra, vagy egy másik MapReduce művelethez. A külső tábla eldobása does *nem* törli az adatokat, csak a tábla definíciójában.

    > [!IMPORTANT]
    > Hagyja a __adatbázis__ kiválasztottat __alapértelmezett__. Az ebben a dokumentumban a példákban HDInsight tartozó alapértelmezett adatbázishoz.

2. A lekérdezés indításához használja a **Execute** a munkalap gombra. Narancssárga változik, és a szöveg a következőre változik **leállítása**.

3. Ha a lekérdezés befejeződött, a **eredmények** lap megjeleníti a művelet eredménye. A következő szöveget a lekérdezés eredménye:

        sev       cnt
        [ERROR]   3

    A **naplók** lap segítségével hozta létre a feldolgozás naplózási információk megtekintése.

   > [!TIP]
   > A **-eredményeket menteni** a bal felső legördülő párbeszédpanelen a **lekérdezési folyamat eredményei** szakasz lehetővé teszi, hogy töltse le vagy -eredményeket menteni.

4. Válassza ki a lekérdezés első négy sorokat, majd válasszon **Execute**. Figyelje meg, hogy nincsenek eredmény a feladat befejezése után. Használja a **Execute** gomb, amikor a lekérdezés van kijelölve, csak a kijelölt utasításokat futtat. Ebben az esetben a kijelölés nem tartozik a végső utasítás, kiolvassa a sorokat a táblában. Ha csak a sor választja, és használjon **Execute**, láthatja, hogy a kívánt eredmény elérése érdekében.

5. A munkalap hozzáadásához használja a **új munkalapra lesznek** gomb alján a **Lekérdezésszerkesztő**. Az új munkalapra adja meg a következő hiveql:

    ```hiveql
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]';
    ```

  Ezekre az utasításokra hajtsa végre a következő műveleteket:

   * **Hozzon létre Ha nem létezik táblázat** -táblázatot hoz létre, ha még nem létezik. Mivel a **külső** kulcsszó nem használható, egy belső tábla jön létre. Egy belső tábla a Hive-adatraktárban tárolja, és teljesen kezeli a struktúra. Ellentétben a külső táblákhoz az alapul szolgáló adatokat egy belső tábla eldobása törli.

   * **TÁROLT AS ORC** -tárolja az adatokat optimalizált sor oszlopos (ORC) formátumban. ORC formátuma egy magas optimalizált és hatékony Hive adatainak tárolásához.

   * **ÍRJA FELÜL AZ INSERT... Válassza ki** -sorát kiválasztja a **log4jLogs** tartalmazó `[ERROR]`, majd beilleszti az adatokat a **errorLogs** tábla.

     Használja a **Execute** gombra a lekérdezés futtatására. A **eredmények** lap tartalmaz adatokat, ha a lekérdezés nulla sort adja vissza. A állapota kell **sikeres** a lekérdezés befejeződése után.

### <a name="visual-explain"></a>Visual ismertetik.

A lekérdezésterv a képi megjelenítés megjelenítéséhez jelölje ki a **Visual ismertetik** alatt a munkalap fülre.

A **Visual ismertetik** lehet, hogy a nézet a lekérdezés a folyamatot, az összetett lekérdezések hasznos információkat. A szöveges megfelelőjét e nézet használatával tekintheti a **magyarázat** gombra a lekérdezés-szerkesztő.

### <a name="tez-ui"></a>Tez felhasználói felület

A lekérdezés a Tez felhasználói felület megjelenítéséhez jelölje ki a **Tez** alatt a munkalap fülre.

> [!IMPORTANT]
> Tez nem használt összes lekérdezéseket. Több lekérdezést orvosolni tudja Tez használata nélkül. 

Ha a Tez a lekérdezés feloldásához használt, az irányított aciklikus diagramhoz (DAG) jelenik meg. Ha meg szeretné tekinteni a DAG lekérdezések már futott az elmúlt, vagy a Tez folyamat hibakeresése használja a [Tez nézet](hdinsight-debug-ambari-tez-view.md) helyette.

## <a name="view-job-history"></a>Feladatelőzmények megtekintése

A __feladatok__ lapon Hive-lekérdezések előzményeit jeleníti meg.

![A feladatelőzmények képe](./media/hdinsight-hadoop-use-hive-ambari-view/job-history.png)

## <a name="database-tables"></a>Adatbázistáblák

Használhatja a __táblák__ lapon Hive adatbázisban lévő táblák együttműködni.

![A táblák lap képe](./media/hdinsight-hadoop-use-hive-ambari-view/tables.png)

## <a name="saved-queries"></a>Lekérdezések

A lekérdezés lap, a lekérdezések opcionálisan mentheti. Miután menti, felhasználhatja a lekérdezés a __mentett lekérdezések__ fülre.

![Lekérdezések lap képe](./media/hdinsight-hadoop-use-hive-ambari-view/saved-queries.png)

## <a name="user-defined-functions"></a>Felhasználó által definiált függvények

Hive is kiterjeszthető felhasználói függvény (UDF) keresztül. Egy UDF funkció vagy logika, amely nem egyszerű modellezve megvalósítását a HiveQL teszi lehetővé.

Az UDF lap felső részén a Hive View teszi deklarál, és mentse a felhasználó által megadott függvények készlete. A felhasználó által megadott függvények használhatók a **Lekérdezésszerkesztő**.

![Az UDF lap képe](./media/hdinsight-hadoop-use-hive-ambari-view/user-defined-functions.png)

A Hive nézetet egy UDF hozzáadása után egy **helyezze be a felhasználó által megadott függvények** gomb alján megjelenik a **Lekérdezésszerkesztő**. Látható értesítések valamelyikének kiválasztásakor Ez a bejegyzés a Hive nézetben megadott felhasználó által megadott függvények legördülő listája. Az UDF-ben ahhoz, hogy a lekérdezés egy UDF kiválasztása HiveQL utasítás hozzá.

Például, ha meghatározta a UDF-ben a következő tulajdonságokkal:

* Az erőforrásnév: myudfs

* Erőforrás elérési útja: /myudfs.jar

* Az UDF-name: myawesomeudf

* Az UDF osztálynév: com.myudfs.Awesome

Használja a **helyezze be a felhasználó által megadott függvények** gomb megjelenik egy bejegyzés nevű **myudfs**, egy másik legördülő minden UDF definiálva az adott erőforráshoz. Ebben az esetben **myawesomeudf**. A lekérdezés elejére válassza ezt a bejegyzést ad a következő:

```hiveql
add jar /myudfs.jar;
create temporary function myawesomeudf as 'com.myudfs.Awesome';
```

Az UDF használhatja a lekérdezésben. Például: `SELECT myawesomeudf(name) FROM people;`.

A felhasználó által megadott függvények használata a HDInsight Hive további információkért lásd a következő dokumentumokat:

* [Python használata a Hive és a Pig a Hdinsightban](hdinsight-python.md)
* [HDInsight egy egyéni Hive UDF felvétele](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/14/how-to-add-custom-hive-udfs-to-hdinsight.aspx)

## <a name="hive-settings"></a>Hive-beállítások

Beállítások segítségével különböző Hive-beállítások módosításához. Például megváltoztathatja a-végrehajtó motor a Hive Tez (alapértelmezett) a MapReduce a.

## <a id="nextsteps"></a>Következő lépések

Általános információk a hdinsight Hive:

* [A Hive használata a hdinsight Hadoop](hdinsight-use-hive.md)

Az egyéb lehetőségeiről a HDInsight Hadoop dolgozhat:

* [A Pig használata a HDInsight Hadoop](hdinsight-use-pig.md)
* [A HDInsight Hadoop MapReduce használata](hdinsight-use-mapreduce.md)
