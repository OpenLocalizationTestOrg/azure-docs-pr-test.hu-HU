---
title: "a Hortonworks védőfal - Azure HDInsight Visual Studio eszközök aaaData Lake |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello Azure Data Lake tools for Visual Studio a helyi virtuális gépen futó hello Hortonworks védőfal. Ezekkel az eszközökkel hozhat létre, és futtassa a Hive és a Pig feladatot a hello védőfal, és a nézet feladatkiemenetét és a korábbi."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: e3434c45-95d1-4b96-ad4c-fb59870e2ff0
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/11/2017
ms.author: larryfr
ms.openlocfilehash: 7a3406b28d87d953e9b5be387faa86268f47b935
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-azure-data-lake-tools-for-visual-studio-with-hello-hortonworks-sandbox"></a>Hello Azure Data Lake tools for Visual Studio használata hello Hortonworks védőfal

Azure Data Lake az általános Hadoop-fürtök használata eszközöket tartalmazza. Ez a dokumentum lépéseit hello toouse hello Data Lake tools a hello szükséges a helyi virtuális gépen futó Hortonworks védőfal.

Hello Hortonworks védőfal használata lehetővé teszi helyileg a fejlesztési környezetet az hadooppal toowork. Miután olyan megoldást fejlesztett ki, és szeretné, hogy toodeploy azt léptékű, majd helyezze át a tooan HDInsight-fürthöz.

## <a name="prerequisites"></a>Előfeltételek

* hello Hortonworks védőfal, a virtuális gépen futó a fejlesztési környezetet. Ez a dokumentum írása és Oracle VirtualBox futó hello védőfal tesztelték. Hello védőfal beállításával kapcsolatos információkért lásd: hello [hello Hortonworks védőfal az első lépései.](hdinsight-hadoop-emulator-get-started.md) a dokumentum.

* A Visual Studio 2013, Visual Studio 2015-öt vagy a Visual Studio 2017 (minden kiadás).

* Hello [Azure SDK for .NET](https://azure.microsoft.com/downloads/) 2.7.1-es vagy újabb.

* Hello [Azure Data Lake tools for Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504).

## <a name="configure-passwords-for-hello-sandbox"></a>Hello védőfal jelszavak beállítása

Győződjön meg arról, hogy fut-e Hortonworks védőfal hello. Kövesse a lépéseket hello hello [megismerheti a hello Hortonworks védőfal](hdinsight-hadoop-emulator-get-started.md#set-sandbox-passwords) dokumentum. Ezeket a lépéseket konfigurálja hello SSH hello jelszavát `root` fiókot, és az hello Ambari `admin` fiók. Ezek a jelszavak toohello védőfal Visual studióból történő csatlakozáskor használt.

## <a name="connect-hello-tools-toohello-sandbox"></a>Csatlakozás hello eszközök toohello védőfal

1. Nyissa meg a Visual Studio, válassza ki **nézet**, majd válassza ki **Server Explorer**.

2. A **Server Explorer**, kattintson a jobb gombbal hello **HDInsight** bejegyzést, és válassza **tooHDInsight emulátor csatlakozás**.

    ![Képernyőfelvétel a Server Explorer eszközben való csatlakozás tooHDInsight emulátor kiemelve](./media/hdinsight-hadoop-emulator-visual-studio/connect-emulator.png)

3. A hello **tooHDInsight emulátor csatlakozás** párbeszédpanelen adja meg az Ambari beállított hello jelszó.

    ![A kijelölt mezőbe a párbeszédpanel képernyőképe](./media/hdinsight-hadoop-emulator-visual-studio/enter-ambari-password.png)

    Válassza ki **következő** toocontinue.

4. Használjon hello **jelszó** mező tooenter hello jelszó hello konfigurálta `root` fiók. Hello hello alapértelmezett értéke a többi mezőt hagyja.

    ![A kijelölt mezőbe a párbeszédpanel képernyőképe](./media/hdinsight-hadoop-emulator-visual-studio/enter-root-password.png)

    Válassza ki **következő** toocontinue.

5. Várjon, amíg hello szolgáltatások toofinish érvényesítése. Bizonyos esetekben a érvényesítése sikertelen, és tooupdate hello konfigurációs kéri. Ha az érvényesítés meghiúsul, válassza ki a **frissítés**, és várja meg, hello konfigurációs és hello szolgáltatás toofinish ellenőrzést.

    ![A kijelölt frissítés gombra a párbeszédpanel képernyőképe](./media/hdinsight-hadoop-emulator-visual-studio/fail-and-update.png)

    > [!NOTE]
    > hello frissítési folyamat Ambari toomodify hello Hortonworks védőfal konfigurációs toowhat várt által hello Data Lake tools for Visual Studio használja.

6. Érvényesítési befejeződését követően válassza ki a **Befejezés** toocomplete konfigurációs.
    ![A kijelölt a Befejezés gombra a párbeszédpanel képernyőképe](./media/hdinsight-hadoop-emulator-visual-studio/finished-connect.png)

     >[!NOTE]
     > A fejlesztési környezet és a virtuális gép toohello kiosztott memória mennyisége hello hello sebességétől függően ez eltarthat néhány percig tooconfigure és hello szolgáltatások ellenőrzése.

Ezek a lépések után most már rendelkezik egy **helyi HDInsight-fürt** bejegyzésre a Server Explorer eszközben az hello **HDInsight** szakasz.

## <a name="write-a-hive-query"></a>Hive-lekérdezések írása

Hive SQL-szerű lekérdezésnyelvet (HiveQL) biztosít a strukturált adatok használata. A következő lépéseket toolearn hogyan igény toorun lekérdezi hello helyi fürtön hajtották hello használata.

1. A **Server Explorer**, kattintson a jobb gombbal a hello helyi fürthöz, amelyeket korábban hozzáadott hello bejegyzést, és válassza **Hive lekérdezés írása**.

    ![Képernyőfelvétel a Server Explorer, a kiemelt Hive lekérdezés írása](./media/hdinsight-hadoop-emulator-visual-studio/write-hive-query.png)

    Egy új lekérdezési ablak. Itt is gyorsan írása, és küldje el a lekérdezés toohello helyi fürt.

2. Adja meg a következő parancs hello hello új lekérdezési ablakba:

        select count(*) from sample_08;

    toorun hello lekérdezés, jelölje be **Submit** hello ablak hello tetején. Hagyja hello más értékek (**kötegelt** és a kiszolgáló neve): hello alapértelmezett értékeket.

    ![Képernyőkép a lekérdezési ablakban, a hello Küldés gomb](./media/hdinsight-hadoop-emulator-visual-studio/submit-hive.png)

    Is használhatja hello legördülő menü mellett túl**Submit** tooselect **speciális**. Speciális beállítások lehetővé teszik tooprovide további beállítások hello feladat elküldésekor.

    ![Parancsfájl elküldése képernyőkép párbeszédpanel](./media/hdinsight-hadoop-emulator-visual-studio/advanced-hive.png)

3. Hello lekérdezés elküldése után hello feladat állapota akkor jelenik meg. hello feladatállapot hello feladat információit jeleníti meg, azt Hadoop dolgozza fel. **Feladat állapota** hello feladat állapotának hello tartalmazza. hello állapot rendszeresen frissül, vagy manuálisan hello frissítési ikon toorefresh hello állapot használhatja.

    ![Képernyőfelvétel a feladat megtekintése párbeszédpanelen, a kijelölt feladat állapota](./media/hdinsight-hadoop-emulator-visual-studio/job-state.png)

    Hello után **feladatállapotot** túl változik**befejezett**, egy irányított aciklikus diagramhoz (DAG) jelenik meg. Ez az ábra ismerteti hello végrehajtási elérési utat, amely határozták meg Tez hello Hive lekérdezés feldolgozása közben. Tez hello alapértelmezett végrehajtó motorja a Hive hello helyi fürtön.

    > [!NOTE]
    > Tez egyben hello alapértelmezett Linux-alapú HDInsight-fürtök használata esetén. A Windows-alapú HDInsight hello alapértelmezett nincs. toouse azt, hozzá kell adnia hello sor `set hive.execution.engine = tez;` toohello elejéhez a Hive-lekérdezést.

    Használjon hello **Job Output** tooview hello kimeneti hivatkozásra. Ebben az esetben is 823, hello hello sample_08 tábla sorainak száma. Diagnosztikai információ hello feladat hello segítségével tekintheti **Job Log** és **YARN naplók letöltése** hivatkozásokat.

4. Futtathat Hive-feladatok interaktív hello módosításával **kötegelt** túl mezőben**interaktív**. Válassza ki **Execute**.

    ![A kijelölt képernyőkép interaktív és végrehajtási gombok](./media/hdinsight-hadoop-emulator-visual-studio/interactive-query.png)

    Az interaktív lekérdezések adatfolyamok hello feldolgozási toohello során előállított kimeneti naplót **hiveserver2-n kimeneti** ablak.

    > [!NOTE]
    > hello információ van hello azonos hello elérhető **Job Log** hivatkozás egy feladat befejezése után.

    ![Képernyőkép a kimeneti naplót](./media/hdinsight-hadoop-emulator-visual-studio/hiveserver2-output.png)

## <a name="create-a-hive-project"></a>Hive-projekt létrehozása

A projekt több Hive parancsfájl tartalmazó is létrehozhat. A projekt használhatók lehetnek kapcsolódó parancsfájlokat vagy toostore parancsfájlok szeretne egy verziókezelő rendszert.

1. A Visual Studio válassza **fájl**, **új**, majd **projekt**.

2. A projektek hello listájában bontsa ki **sablonok**, bontsa ki a **Azure Data Lake**, majd válassza ki **struktúra (HDInsight)**. Válassza ki a sablonok hello listáról **minta Hive**. Adja meg a nevét és helyét, majd válassza ki **OK**.

    ![Képernyőfelvétel az új projekt ablakról, Azure Data Lake, a HIVE, a minta Hive és a OK kiemelve](./media/hdinsight-hadoop-emulator-visual-studio/new-hive-project.png)

Hello **minta Hive** -projekt tartalmazza két parancsfájlok **WebLogAnalysis.hql** és **SensorDataAnalysis.hql**. Ezen parancsfájlok használatával hello azonos elküldheti **Submit** hello ablak hello tetején gombra.

## <a name="create-a-pig-project"></a>A Pig-projekt létrehozása

Hive egy SQL-szerű nyelv biztosít a strukturált adatok használata, amíg a Pig adatokon átalakítások elvégzésével működik. A Pig biztosít egy nyelv (a Pig latin betűs), amely lehetővé teszi a toodevelop átalakítások folyamat. a Pig toouse hello helyi fürthöz, kövesse az alábbi lépéseket:

1. Nyissa meg a Visual Studio, és válassza ki **fájl**, **új**, majd **projekt**. A projektek hello listájában bontsa ki **sablonok**, bontsa ki a **Azure Data Lake**, majd válassza ki **Pig (HDInsight)**. Válassza ki a sablonok hello listáról **Pig alkalmazás**. Adjon meg egy nevet, a helyre, és válassza ki **OK**.

    ![Képernyőfelvétel az új projekt ablakról, az Azure Data Lake, a Pig, a Pig alkalmazás és az OK gombra a kijelölt](./media/hdinsight-hadoop-emulator-visual-studio/new-pig.png)

2. Adja meg a szöveg másként hello hello tartalmát a következő hello **script.pig** projekthez létrehozott fájlt.

        a = LOAD '/demo/data/Website/Website-Logs' AS (
            log_id:int,
            ip_address:chararray,
            date:chararray,
            time:chararray,
            landing_page:chararray,
            source:chararray);
        b = FILTER a BY (log_id > 100);
        c = GROUP b BY ip_address;
        DUMP c;

    Pig más nyelvű Hive-nál, amíg a hello feladatok futtatásának módját mindkét nyelven, keresztül hello konzisztensek **Submit** gombra. Kiválasztásával hello melletti legördülő **Submit** egy speciális Küldés párbeszédpanel megjeleníti a Pig-feladatokkal.

    ![Parancsfájl elküldése képernyőkép párbeszédpanel](./media/hdinsight-hadoop-emulator-visual-studio/advanced-pig.png)

3. hello feladat állapotát és a kimeneti is látható, hello ugyanaz, mint a Hive-lekérdezések.

    ![Képernyőkép a Pig projekt](./media/hdinsight-hadoop-emulator-visual-studio/completed-pig.png)

## <a name="view-jobs"></a>Feladatok megtekintése

A Data Lake tools is lehetővé teszik a Hadoop futó feladatok adatainak tooeasily megtekintése. A következő lépéseket toosee hello feladatok hello helyi fürtön futó hello használata.

1. A **Server Explorer**, kattintson a jobb gombbal a hello helyi fürthöz, és válassza **feladatok megtekintése**. Lett elküldve toohello fürt feladatok listája jelenik meg.

    ![Képernyőfelvétel a Server Explorer, a kiemelt feladatok megtekintése](./media/hdinsight-hadoop-emulator-visual-studio/view-jobs.png)

2. A feladatok hello listából válasszon egyet tooview hello feladat részleteit.

    ![Képernyőfelvétel a feladat böngésző, egy kiemelt hello feladatok](./media/hdinsight-hadoop-emulator-visual-studio/view-job-details.png)

    megjelenő hello információ a hasonló toowhat párbeszédeket hivatkozások tooview hello kimeneti és naplózási Hive vagy Pig lekérdezés futtatása után megjelenik.

3. Módosítja, és küldje el újra a hello feladat itt.

## <a name="view-hive-databases"></a>Hive adatbázisok megtekintése

1. A **Server Explorer**, bontsa ki a hello **helyi HDInsight-fürt** bejegyzést, majd bontsa ki a **Hive adatbázisokat**. Hello **alapértelmezett** és **xademo** hello helyi fürtön lévő adatbázisok jelennek meg. Hello táblák hello adatbázison belül adatbázis bővülő jeleníti meg.

    ![Képernyőfelvétel a Server Explorer eszközben kibontva adatbázisok](./media/hdinsight-hadoop-emulator-visual-studio/expanded-databases.png)

2. A tábla oszlopainak hello táblázat kibontása jeleníti meg. tooquickly nézet hello, kattintson a jobb gombbal egy táblát, és válasszon **View Top 100 sor**.

    ![Képernyőfelvétel a Server Explorer eszközben kibontva tábla és nézet kijelölt Top 100 sorok](./media/hdinsight-hadoop-emulator-visual-studio/view-100.png)

### <a name="database-and-table-properties"></a>Adatbázis és tábla tulajdonságai

Egy adatbázis vagy táblázat hello tulajdonságait tekintheti meg. Kiválasztása **tulajdonságok** hello tulajdonságai ablakban hello kijelölt elem adatokat tartalmazza. Például tekintse meg a következő képernyőkép hello hello információkat:

![Képernyőfelvétel a Tulajdonságok ablak](./media/hdinsight-hadoop-emulator-visual-studio/properties.png)

### <a name="create-a-table"></a>Tábla létrehozása

toocreate egy táblázat frissítéséhez kattintson a jobb gombbal egy adatbázist, majd válassza ki **Create Table**.

![Képernyőfelvétel a Server Explorer eszközben a Create Table kiemelve](./media/hdinsight-hadoop-emulator-visual-studio/create-table.png)

Hello tábla űrlap használatával is létrehozhat. A következő képernyőkép hello hello alján megjelenik, amely használt toocreate hello tábla nyers HiveQL hello.

![Képernyőfelvétel a hello űrlap használt toocreate tábla](./media/hdinsight-hadoop-emulator-visual-studio/create-table-form.png)

## <a name="next-steps"></a>Következő lépések

* [Learning hello drótkötelek a hello Hortonworks védőfal](http://hortonworks.com/hadoop-tutorial/learning-the-ropes-of-the-hortonworks-sandbox/)
* [Hadoop oktatóanyag – első lépések HDP](http://hortonworks.com/hadoop-tutorial/hello-world-an-introduction-to-hadoop-hcatalog-hive-and-pig/)
