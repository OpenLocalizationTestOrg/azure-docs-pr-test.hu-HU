---
title: "keresztül hello JDBC-illesztőt - Azure HDInsight Hive aaaQuery |} Microsoft Docs"
description: "Hello JDBC-illesztőt a egy Java application toosubmit Hive-lekérdezések tooHadoop használata a HDInsight. Csatlakoztassa a szoftveres és hello SQuirrel SQL ügyfélről."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 928f8d2a-684d-48cb-894c-11c59a5599ae
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/14/2017
ms.author: larryfr
ms.openlocfilehash: 93178da3b8d497faa4c788e91dba89c4e45d3fff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="query-hive-through-hello-jdbc-driver-in-hdinsight"></a>Keresztül hello JDBC-illesztőt hdinsight Hive lekérdezés

[!INCLUDE [ODBC-JDBC-selector](../../includes/hdinsight-selector-odbc-jdbc.md)]

Ismerje meg, hogyan toouse hello JDBC-illesztőt a egy Java-alkalmazás toosubmit Hive lekérdezi az Azure HDInsight tooHadoop. a dokumentumban szereplő információk hello bemutatja, hogyan tooconnect programozott módon, és a hello SQuirrel SQL ügyfél.

A Hive JDBC felület hello további információkért lásd: [HiveJDBCInterface](https://cwiki.apache.org/confluence/display/Hive/HiveJDBCInterface).

## <a name="prerequisites"></a>Előfeltételek

* A Hadoop on HDInsight-fürt. Linux-alapú vagy a Windows-alapú fürtök működik.

  > [!IMPORTANT]
  > Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További információkért lásd: [HDInsight 3.3 használatból való kivonást](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* [SQuirreL SQL](http://squirrel-sql.sourceforge.net/). SQuirreL JDBC ügyfélalkalmazás.

* Hello [Java fejlesztői készlet (JDK) 7-es verzió](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) vagy újabb verzióját.

* [Apache Maven](https://maven.apache.org). Maven, a projekt felépítéséhez rendszer Java-projektek esetében ez a cikk társított hello projekt által használt.

## <a name="jdbc-connection-string"></a>JDBC-kapcsolati karakterlánc

JDBC kapcsolatok tooan HDInsight-fürt Azure válnak, mint a 443-as és hello forgalmát az SSL használatával lett biztonságossá. hello nyilvános átjáró mögötti elhelyezkedik hello fürtök hello forgalom toohello port, amelyet a hiveserver2-n figyel ténylegesen irányítja át. hello az alábbiakban látható egy példa kapcsolati karakterlánc:

    jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;transportMode=http;ssl=true;httpPath=/hive2

Cserélje le `CLUSTERNAME` hello nevet, a HDInsight-fürthöz.

## <a name="authentication"></a>Authentication

Hello-kapcsolat létesítéséhez hello HDInsight fürt admin nevét és jelszavát tooauthenticate toohello fürt átjáró kell használnia. JDBC-ügyfelek például SQuirreL SQL kapcsolódáskor meg kell adnia hello admin nevét és jelszavát az ügyfélbeállításokban.

Java-alkalmazás kell használnia hello nevet és jelszót egy kapcsolat. Például hello következő Java-kóddal megnyit egy új kapcsolatot hello kapcsolati karakterláncot, admin név és jelszó használatával:

```java
DriverManager.getConnection(connectionString,clusterAdmin,clusterPassword);
```

## <a name="connect-with-squirrel-sql-client"></a>Csatlakozás SQuirreL SQL-ügyféllel

SQuirreL SQL, amely a HDInsight-fürthöz Hive-lekérdezések futtatásához használt tooremotely JDBC ügyfél. hello következő lépések azt feltételezik, hogy még telepítette SQuirreL SQL.

1. Hello Hive JDBC illesztőprogramok másolása a HDInsight-fürthöz.

    * A **Linux-alapú HDInsight**, használjon hello alábbi lépéseit toodownload szükséges hello jar-fájlok.

        1. Hozzon létre egy hello fájlokat tartalmazó könyvtárat. Például: `mkdir hivedriver`.

        2. A parancssorból használjon hello következő parancsok toocopy hello fájlok hello HDInsight-fürt:

            ```bash
            scp USERNAME@CLUSTERNAME:/usr/hdp/current/hive-client/lib/hive-jdbc*standalone.jar .
            scp USERNAME@CLUSTERNAME:/usr/hdp/current/hadoop-client/hadoop-common.jar .
            scp USERNAME@CLUSTERNAME:/usr/hdp/current/hadoop-client/hadoop-auth.jar .
            ```

            Cserélje le `USERNAME` nevű hello SSH felhasználói fiók hello fürthöz. Cserélje le `CLUSTERNAME` hello HDInsight fürt névvel.

    * A **Windows-alapú HDInsight**, használjon hello alábbi lépéseit toodownload hello jar-fájlok.

        1. A hello Azure-portálon, a HDInsight-fürt, majd válassza ki és hello **távoli asztal** ikonra.

            ![Távoli asztali ikon](./media/hdinsight-connect-hive-jdbc-driver/remotedesktopicon.png)

        2. A távoli asztal panelen hello használata hello **Connect** gomb tooconnect toohello fürt. Hello távoli asztal nincs engedélyezve, ha hello űrlap tooprovide egy felhasználónevet és jelszót használják, és válasszon **engedélyezése** tooenable távoli asztal hello fürthöz.

            ![Távoli asztali panel](./media/hdinsight-connect-hive-jdbc-driver/remotedesktopblade.png)

            Miután kiválasztott **Connect**, egy .rdp le. A fájl toolaunch hello távoli asztali ügyfél használja. Amikor a rendszer kéri, hello felhasználónév és a távoli asztal eléréséhez megadott jelszó használata.

        3. A csatlakozás után másolja a következő fájlok a hello távoli asztali munkamenet tooyour helyi számítógépről hello. Nevű helyi könyvtárba helyezze őket `hivedriver`.

            * C:\apps\dist\hive-0.14.0.2.2.9.1-7\lib\hive-JDBC-0.14.0.2.2.9.1-7-Standalone.JAR
            * C:\apps\dist\hadoop-2.6.0.2.2.9.1-7\share\hadoop\common\hadoop-Common-2.6.0.2.2.9.1-7.JAR
            * C:\apps\dist\hadoop-2.6.0.2.2.9.1-7\share\hadoop\common\lib\hadoop-auth-2.6.0.2.2.9.1-7.JAR

            > [!NOTE]
            > hello verzió hello elérési útját és fájlnevét szereplő számok lehetnek a fürt számára.

        4. Válassza le a hello távoli asztali munkamenet befejezése hello fájlok másolása után.

2. Hello SQuirreL SQL alkalmazás indításához. Hello ablak bal oldalán hello, válassza ki **illesztőprogramok**.

    ![Illesztőprogramok lap hello bal oldali hello ablak](./media/hdinsight-connect-hive-jdbc-driver/squirreldrivers.png)

3. A hello ikonok hello hello tetején **illesztőprogramok** párbeszédpanelen jelölje be hello  **+**  ikon toocreate illesztőprogramot.

    ![Illesztőprogramok ikonok](./media/hdinsight-connect-hive-jdbc-driver/driversicons.png)

4. Hello illesztőprogram hozzáadása párbeszédpanelen adja hozzá a következő információ hello:

    * **Név**: struktúra
    * **Példa URL-cím**:`jdbc:hive2://localhost:443/default;transportMode=http;ssl=true;httpPath=/hive2`
    * **Extra osztály elérési útja**: hello Hozzáadás gomb tooadd hello jar fájlok korábban letöltött
    * **Osztálynév**: org.apache.hive.jdbc.HiveDriver

   ![illesztőprogram párbeszédpanel hozzáadása](./media/hdinsight-connect-hive-jdbc-driver/adddriver.png)

   Kattintson a **OK** toosave ezeket a beállításokat.

5. Hello hello SQuirreL SQL ablak bal oldali, válassza ki a **aliasok**. Kattintson a hello  **+**  ikon toocreate egy kapcsolat aliast.

    ![Új alias hozzáadása](./media/hdinsight-connect-hive-jdbc-driver/aliases.png)

6. Hello használata hello következő értékei **hozzáadása Alias** párbeszédpanel.

    * **Név**: hdinsight Hive

    * **Illesztőprogram**: használata hello legördülő tooselect hello **Hive** illesztőprogram

    * **URL-cím**: jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;transportMode=http;ssl=true;httpPath=/hive2

        Cserélje le **CLUSTERNAME** hello nevet, a HDInsight-fürthöz.

    * **Felhasználónév**: hello fürt bejelentkezési fiók nevét a HDInsight-fürthöz. hello alapértelmezett érték a `admin`.

    * **Jelszó**: hello hello fürt bejelentkezési fiók jelszavát.

 ![Adja hozzá a alias párbeszédpanel](./media/hdinsight-connect-hive-jdbc-driver/addalias.png)

    Használjon hello **teszt** gomb tooverify, amely hello kapcsolat működik. Ha **csatlakozni: hdinsight Hive** kiválasztása párbeszédpanel jelenik meg, **Connect** tooperform hello tesztelése. Ha hello teszt sikeres, megjelenik egy **sikeres kapcsolat** párbeszédpanel.

    toosave hello kapcsolat alias, használjon hello **Ok** hello hello alján gomb **hozzáadása Alias** párbeszédpanel.

7. A hello **csatlakozni** SQuirreL SQL hello felső legördülő menüből válassza **hdinsight Hive**. Amikor a rendszer kéri, válassza ki a **Connect**.

    ![kapcsolódási párbeszédpanel](./media/hdinsight-connect-hive-jdbc-driver/connect.png)

8. A csatlakozás után adja meg a következő lekérdezés be hello SQL lekérdezési párbeszédpanel megnyitása, és válassza a hello hello **futtatása** ikonra. hello eredmények terület hello lekérdezési eredmények hello kell megjelennie.

        select * from hivesampletable limit 10;

    ![SQL lekérdezési párbeszédpanel megnyitása, beleértve az eredmények](./media/hdinsight-connect-hive-jdbc-driver/sqlquery.png)

## <a name="connect-from-an-example-java-application"></a>Csatlakoztatja a Java-alkalmazást például a

Példa egy Java-ügyfél tooquery Hive használata a HDInsight érhető el: [https://github.com/Azure-Samples/hdinsight-java-hive-jdbc](https://github.com/Azure-Samples/hdinsight-java-hive-jdbc). Hello tárház toobuild hello utasításait követve, és futtasson hello mintát.

## <a name="troubleshooting"></a>Hibaelhárítás

### <a name="unexpected-error-occurred-attempting-tooopen-an-sql-connection"></a>Váratlan hiba történt a(z) tooopen egy SQL-kapcsolatot.

**A jelenség**: tooan HDInsight-fürt 3.3-as vagy 3.4-es verziójú kapcsolódáskor jelenhet meg, amely a váratlan hiba történt hiba. Ezt a hibát hello Veremkivonat alábbi hello kezdődik:

```java
java.util.concurrent.ExecutionException: java.lang.RuntimeException: java.lang.NoSuchMethodError: org.apache.commons.codec.binary.Base64.<init>(I)V
at java.util.concurrent.FutureTas...(FutureTask.java:122)
at java.util.concurrent.FutureTask.get(FutureTask.java:206)
```

**OK**: Ez a hiba oka egy hello hello commons-codec.jar fájl SQuirreL és hello egy kötelező hello Hive JDBC-összetevők által használt verziója nem egyezik.

**Megoldási**: Ez a hiba, a következő használatát hello lépések toofix:

1. Hello commons-kodek jar-fájlra letölthető a HDInsight-fürthöz.

        scp USERNAME@CLUSTERNAME:/usr/hdp/current/hive-client/lib/commons-codec*.jar ./commons-codec.jar

2. SQuirreL kilép, és keresse toohello directory ahol a SQuirreL telepítve van a rendszeren. A hello SquirreL könyvtárat, hello `lib` directory, a név felülírandó hello meglévő commons-codec.jar hello hello HDInsight-fürt egyik letölti a.

3. Indítsa újra az SQuirreL. hello hiba már nem kell végrehajtani, a HDInsight tooHive kapcsolódáskor.

## <a name="next-steps"></a>Következő lépések

Most, hogy megtanulta, hogyan toouse JDBC toowork a Hive, a következő használatát hello hivatkozásait tooexplore más módokon toowork Azure hdinsightban.

* [Adatok tooHDInsight feltöltése](hdinsight-upload-data.md)
* [A Hive használata a HDInsightban](hdinsight-use-hive.md)
* [A Pig használata a HDInsightban](hdinsight-use-pig.md)
* [MapReduce-feladatok használata a HDInsightban](hdinsight-use-mapreduce.md)
