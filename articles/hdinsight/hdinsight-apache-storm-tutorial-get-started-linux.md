---
title: "aaaStorm-kezdőpéldák a HDInsight - Azure alatt futó Apache Storm |} Microsoft Docs"
description: "Megtudhatja, hogyan toodo big data-elemzések és a valós idejű adatfeldolgozásra Apache Storm használatának és a HDInsight alatt futó storm-starter-példák hello."
keywords: "storm starter, apache storm-példa"
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: d710dcac-35d1-4c27-a8d6-acaf8146b485
ms.service: hdinsight
ms.devlang: java
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/15/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive,hdiseo17may2017
ms.openlocfilehash: bb6d6549e67ca5b557f0692f98c89692a87267b0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
#<a name="get-started-with-apache-storm-on-hdinsight-using-hello-storm-starter-examples"></a>Ismerkedés az Apache Storm on HDInsight hello storm-kezdőpéldák a

Ismerje meg, hogyan toouse alatt futó Apache Storm a Hdinsightban az hello storm-kezdőpéldák.

Az Apache Storm egy skálázható, hibatűrő, elosztott, valós idejű számítási rendszer az adatstreamek feldolgozására. A Storm on Azure HDInsight segítségével olyan felhőalapú Storm-fürtöket hozhat létre, amelyek valós időben végeznek big data elemzést.

> [!IMPORTANT]
> Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="prerequisites"></a>Előfeltételek

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

* **Azure-előfizetés**. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* **SSH- és SCP-ismeretek**. További információk: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="create-a-storm-cluster"></a>Storm-fürt létrehozása

Következő lépések toocreate Storm on HDInsight-fürt hello használata:

1. A hello [Azure-portálon](https://portal.azure.com), jelölje be **+ új**, **Eszközintelligencia + analitika**, majd válassza ki **HDInsight**.

    ![HDInsight-fürt létrehozása](./media/hdinsight-apache-storm-tutorial-get-started-linux/create-hdinsight.png)

2. A hello **alapjai** panelen adja meg a következő információ hello:

    * **A fürt neve**: hello hello HDInsight-fürt nevét.
    * **Előfizetés**: hello előfizetés toouse kiválasztása.
    * **A fürt bejelentkezési felhasználónevének** és **fürt bejelentkezési jelszó**: hello bejelentkezési hello fürt eléréséhez a HTTPS-KAPCSOLATON keresztül. A hitelesítő adatok tooaccess szolgáltatások például hello Ambari webes felhasználói felületén vagy a REST API-t használja.
    * **Secure Shell (SSH) felhasználónév**: hello bejelentkezési SSH-n keresztül hello fürt eléréséhez használt. Alapértelmezés szerint hello jelszó hello ugyanaz, mint a hello fürt bejelentkezési jelszót.
    * **Erőforráscsoport**: hello erőforrás csoport toocreate hello fürtöt.
    * **Hely**: hello Azure-régiót toocreate hello fürtöt.

    ![Előfizetés kiválasztása](./media/hdinsight-apache-storm-tutorial-get-started-linux/hdinsight-basic-configuration.png)

3. Válassza ki **típusú fürt**, és a set hello majd értékek következő hello **fürtkonfiguráció** panel:

    * **Fürt típusa**: Storm

    * **Operációs rendszer**: Linux

    * **Verzió**: Storm 1.1.0 (HDI 3.6)

    * **Fürt szintje**: Standard

    Végül, használja a hello **válasszon** toosave beállítások gombra.

    ![Fürttípus kiválasztása](./media/hdinsight-apache-storm-tutorial-get-started-linux/set-hdinsight-cluster-type.png)

4. Hello fürt típusának kiválasztása után használja a hello __válasszon__ gomb tooset hello fürt típusa. Következő lépésként az hello __következő__ gomb toofinish alapszintű konfigurálása.

5. A hello **tárolási** panelen válassza ki vagy hozzon létre egy tárfiókot. A jelen dokumentumban leírt lépések hello, hello többi mezőt hagyja a panel hello alapértelmezett értéken. Használjon hello __következő__ gomb toosave tárolási konfigurációt.

    ![Állítsa be a HDInsight-hello tárolási fiók beállításai](./media/hdinsight-apache-storm-tutorial-get-started-linux/set-hdinsight-storage-account.png)

6. A hello **összegzés** panelen hello fürt hello konfiguráció áttekintése. Használjon hello __szerkesztése__ hivatkozásait toochange életbe léptetett beállítások helytelenek. Végezetül the__Create__ gomb toocreate hello-fürt használatára.

    ![A fürtkonfiguráció összegzése](./media/hdinsight-apache-storm-tutorial-get-started-linux/hdinsight-configuration-summary.png)

    > [!NOTE]
    > Too20 perc toocreate hello fürt is eltarthat.

## <a name="run-a-storm-starter-sample-on-hdinsight"></a>Storm Starter-példa futtatása a HDInsightban

1. Csatlakozzon az SSH használatával toohello HDInsight-fürt:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    Ha a jelszó toosecure az SSH-felhasználói fiókhoz,-e rákérdezéses tooenter azt. A nyilvános kulcs használatakor kell segítségével hello `-i` paraméter toospecify hello kapcsolódó titkos kulcs. Például: `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`.

    További információk: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

2. A következő parancs toostart példatopológia hello használata:

        storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-*.jar org.apache.storm.starter.WordCountTopology wordcount

    > [!NOTE]
    > A HDInsight korábbi verzióiban hello topológia hello osztály neve nem `storm.starter.WordCountTopology` helyett `org.apache.storm.starter.WordCountTopology`.

    Indítja el a hello példa WordCount topológia hello fürtön, a "wordcount" rövid nevet. Véletlenszerűen létrehozott mondatokat és count hello előfordulásainak kívánt számát, az összes szó hello mondat a.

    > [!NOTE]
    > A saját topológiák toohello fürtre történő elküldésekor, át kell másolnia hello használata előtt hello fürtöt tartalmazó jar-fájlra hello `storm` parancsot. Használjon hello `scp` toocopy hello parancsfájlt. Például: `scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`
    >
    > hello WordCount-példa és egyéb storm-starter-példák megtalálhatóak a fürthöz `/usr/hdp/current/storm-client/contrib/storm-starter/`.

Ha érdekli a hello storm-kezdőpéldák hello forrás megtekintése, hello kódot található [https://github.com/apache/storm/tree/1.1.x-branch/examples/storm-starter](https://github.com/apache/storm/tree/1.1.x-branch/examples/storm-starter). A hivatkozás a Storm 1.1.x-es verzió példáira mutat, amely a HDInsight 3.6-ban érhető el. A Storm egyéb verziói esetén használja a hello __fiókirodai__ hello lap tooselect hello tetején gombra egy másik Storm verzióját.

## <a name="monitor-hello-topology"></a>A figyelő hello topológia

hello Storm felhasználói felülete webes felületet biztosít a futó topológiákkal dolgozik, és szerepel-e a HDInsight-fürthöz.

A következő lépéseket toomonitor hello topológiájának megtekintését a Storm felhasználói felülete hello hello használata:

1. toodisplay hello Storm felhasználói felületén, nyissa meg egy webes böngésző toohttps://CLUSTERNAME.azurehdinsight.net/stormui. Cserélje le **CLUSTERNAME** hello néven a fürt.

    > [!NOTE]
    > Ha tooprovide kéri a felhasználónevet és jelszót, adja meg a hello Fürtrendszergazda (rendszergazda) és a használt jelszó létrehozása hello fürt.

2. A **topológia összegzése**, jelölje be hello **wordcount** hello bejegyzést **neve** oszlop. Hello topológiára vonatkozó információk jelennek meg.

    ![A Storm irányítópultja a Storm Starter WordCount-topológiára vonatkozó információkkal.](./media/hdinsight-apache-storm-tutorial-get-started-linux/topology-summary.png)

    Ezen a lapon hello a következő információkat biztosítja:

    * **Topológiastatisztikák** – alapszintű információkat tartalmaz hello topológia teljesítményével kapcsolatban, időtartományokba.

        > [!NOTE]
        > Ha a egy adott időpont ablak módosítások hello időkerete hello lap más szakaszaiban talál.

    * **Spoutok** – alapszintű információkat tartalmaz a spoutokkal kapcsolatban, beleértve az egyes spoutok által visszaadott legutóbbi hibaüzenetet hello.

    * **Boltok** – Alapszintű információkat tartalmaz a boltokkal kapcsolatban.

    * **Topológiakonfiguráció** -részletes hello topológia konfigurációjával kapcsolatos információkat.

    Ezen a lapon is biztosít, amelyen átvihető hello topológia műveletek:

    * **Aktiválás** – Folytatja az inaktivált topológia feldolgozását.

    * **Inaktiválás** – Megszakítja a futó topológiát.

    * **Visszaegyensúlyozás** -hello topológia párhuzamosságát hello módosíthatja. Futó topológiákat kell egyensúlyba, miután módosította hello fürtben található csomópontok számának hello. Párhuzamossági toocompensate hello növekedése/csökkenése számaként hello fürtben található csomópontok újraelosztás módosítja. További információkért lásd: [ismertetése a Storm-topológia párhuzamosságát hello](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html).

    * **Kill** – a Storm-topológia leállítása után hello megadott időkorlát.

3. Ezen a lapon válassza ki egy bejegyzést hello **Spoutok** vagy **boltok** szakasz. Kiválasztott összetevő hello információkat jelenít meg.

    ![A Storm irányítópultja a kiválasztott összetevőkkel kapcsolatos információkkal.](./media/hdinsight-apache-storm-tutorial-get-started-linux/component-summary.png)

    Ez a lap megjeleníti a következő információ hello:

    * **Spout vagy Bolt statisztikák** – alapszintű információkat tartalmaz hello összetevők teljesítményével kapcsolatban, időtartományokba.

        > [!NOTE]
        > Ha a egy adott időpont ablak módosítások hello időkerete hello lap más szakaszaiban talál.

    * **Beviteli statisztikák** (csak boltok esetében) - információk hello bolt által feldolgozott adatokat előállító összetevőkről.

    * **Kimeneti statisztikák** – Információkat tartalmaz a bolt által kibocsátott adatokról.

    * **Végrehajtók** – Információkat tartalmaz a jelen összetevő példányairól.

    * **Hibák** – A jelen összetevő által visszaadott hibaüzenetek.

4. Ha adott spout vagy bolt hello részleteit jeleníti meg, jelöljön ki egy bejegyzést a hello **Port** hello oszlopa **végrehajtója** hello összetevők adott példánya részletes tooview szakasz.

        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: split default ["with"]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: split default ["nature"]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [snow]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [snow, 747293]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [white]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [white, 747293]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [seven]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [seven, 1493957]

    Ebben a példában a word hello **hét** 1493957 hányszor történt. A számláló értéke hányszor hello word történt. Ez a topológia indítása óta.

## <a name="stop-hello-topology"></a>Hello topológia leállítása

Térjen vissza a toohello **topológia összegzése** hello word-count topológiához lapján, majd válassza ki a hello **Kill** hello a gomb **topológia műveletek** szakasz. Amikor a rendszer kéri, adja meg 10 másodperc toowait hello hello topológia leállítása előtt. Hello időkorlát, után hello topológia nem jelenik meg az hello felkeresésekor **Storm felhasználói felülete** hello irányítópult szakasza.

## <a name="delete-hello-cluster"></a>Hello fürt törlése

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Ha problémába ütközik a HDInsight-fürtök létrehozása során, tekintse meg [a hozzáférés-vezérlésre vonatkozó követelményeket](hdinsight-administer-use-portal-linux.md#create-clusters).

## <a id="next"></a>Következő lépések

Az Apache Storm oktatóanyag megismerte hello használata a HDInsight alatt futó Storm alapjait. A következő megtudhatja, hogyan túl[Maven használatával fejlesztése Java-alapú topológiák](hdinsight-storm-develop-java-topology.md).

Ha már ismeri a Java-alapú topológiák fejlesztése és egy meglévő topológiát tooHDInsight toodeploy, lásd: [központi telepítése és kezelése a HDInsight alatt futó Apache Storm-topológiák](hdinsight-storm-deploy-monitor-topology-linux.md).

A.NET-fejlesztők C#- vagy hibrid C#/Java-topológiákat hozhatnak létre a Visual Studio használatával. További információk: [C#-topológiák fejlesztése HDInsight alatt futó Apache Stormra a Visual Studio Hadoop-eszközeinek használatával](hdinsight-storm-develop-csharp-visual-studio-topology.md).

Példa topológiákat, amely a HDInsight alatt futó Storm használható tekintse meg a következő példák hello:

* [HDInsight alatt futó Storm példatopológiái](hdinsight-storm-example-topology.md)

[apachestorm]: https://storm.incubator.apache.org
[stormdocs]: http://storm.incubator.apache.org/documentation/Documentation.html
[stormstarter]: https://github.com/apache/storm/tree/master/examples/storm-starter
[stormjavadocs]: https://storm.incubator.apache.org/apidocs/
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[preview-portal]: https://portal.azure.com/
