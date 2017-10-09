---
title: "aaaDeploy és kezelése a Linux-alapú HDInsight alatt futó Apache Storm-topológiák |} Microsoft Docs"
description: "Ismerje meg, hogyan toodeploy, figyelheti és kezelheti hello Storm irányítópultjának használata a Linux-alapú HDInsight alatt futó Apache Storm-topológiák. Visual Studio Hadoop-eszközök használata."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 35086e62-d6d8-4ccf-8cae-00073464a1e1
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/16/2017
ms.author: larryfr
ms.openlocfilehash: 3a1edb773089cc596fea423710aa88cf83c7b841
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-manage-apache-storm-topologies-on-hdinsight"></a>Központi telepítése és kezelése a HDInsight alatt futó Apache Storm-topológiák

Ebből a dokumentumból megtudhatja, kezelése és figyelése a Storm a HDInsight-fürtökön futó Storm-topológiák hello alapjait.

> [!IMPORTANT]
> hello cikkben leírt lépéseket igényelnek a Linux-alapú Storm on HDInsight-fürt. Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement). 
>
> A telepítését és megfigyelését futó Windows-alapú HDInsight-topológiák további információkért lásd: [központi telepítése és kezelése a Windows-alapú HDInsight alatt futó Apache Storm-topológiák](hdinsight-storm-deploy-monitor-topology.md)


## <a name="prerequisites"></a>Előfeltételek

* **A Linux-alapú Storm on HDInsight-fürt**: lásd: [első lépései a HDInsight alatt futó Apache Storm](hdinsight-apache-storm-tutorial-get-started-linux.md) fürt létrehozásának lépései

* (Választható) **SSH és az SCP ismeretét**: további információkért lásd: [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

* (Választható) **Visual Studio**: Azure SDK 2.5.1-es vagy újabb és hello Data Lake Tools for Visual Studio. További információkért lásd: [Ismerkedés a Data Lake Tools for Visual Studio használatával](hdinsight-hadoop-visual-studio-tools-get-started.md).

    A következő Visual Studio verziójának hello egyikét:

  * A Visual Studio 2012 [4. frissítés](http://www.microsoft.com/download/details.aspx?id=39305)

  * A Visual Studio 2013-as verziójának [4. frissítés](http://www.microsoft.com/download/details.aspx?id=44921) vagy [Visual Studio 2013 Community](http://go.microsoft.com/fwlink/?LinkId=517284)
  * [A Visual Studio 2015](https://www.visualstudio.com/downloads/)

  * A Visual Studio 2015 (minden kiadás)

  * A Visual Studio 2017 (minden kiadás). A Data Lake Tools for Visual Studio 2017 hello Azure munkaterhelés részeként telepített.

## <a name="submit-a-topology-visual-studio"></a>Küldje el a topológia: Visual Studio

a HDInsight Tools hello lehet használt toosubmit C# vagy hibrid topológiák tooyour Storm-fürt. a lépéseket követve hello mintaalkalmazás használja. A saját topológiák hello a HDInsight Tools használatával létrehozásával kapcsolatos további információkért lásd: [fejlesztésére C#-topológiák hello HDInsight Tools for Visual Studio használatával](hdinsight-storm-develop-csharp-visual-studio-topology.md).

1. Ha még nem telepítette hello legújabb verziójának hello Data Lake tools for Visual Studio, lásd: [Ismerkedés a Data Lake Tools for Visual Studio használatával](hdinsight-hadoop-visual-studio-tools-get-started.md).

    > [!NOTE]
    > a Data Lake Tools for Visual Studio hello volt korábbi nevén hello a HDInsight Tools for Visual Studio.
    >
    > A Data Lake Tools for Visual Studio szerepelnek hello __Azure munkaterhelés__ a Visual Studio 2017.

2. Nyissa meg a Visual Studio, válassza ki **fájl** > **új** > **projekt**.

3. A hello **új projekt** párbeszédpanelen bontsa ki **telepített** > **sablonok**, majd válassza ki **HDInsight**. Válassza ki a sablonok hello listáról **Storm minta**. Hello hello párbeszédpanel alsó részén írja be a hello alkalmazás nevét.

    ![Kép](./media/hdinsight-storm-deploy-monitor-topology-linux/sample.png)

4. A **Megoldáskezelőben**, kattintson a jobb gombbal a hello projektet, és válassza ki **elküldeni a HDInsight tooStorm**.

   > [!NOTE]
   > Ha a rendszer kéri, adja meg hello bejelentkezési hitelesítő adatait az Azure-előfizetéshez. Ha egynél több előfizetéssel rendelkezik, jelentkezzen be a Storm on HDInsight-fürt tartalmazó toohello.

5. Válassza ki a Storm on HDInsight-fürt hello **Storm-fürt** legördülő listából válassza ki, és válassza **Submit**. Figyelheti, hogy hello elküldése sikeres hello segítségével **kimeneti** ablak.

## <a name="submit-a-topology-ssh-and-hello-storm-command"></a>Küldje el a topológia: SSH és hello Storm parancs

1. SSH tooconnect toohello HDInsight-fürt használatára. Cserélje le **felhasználónév** az SSH-bejelentkezéskor hello nevét. Cserélje le **CLUSTERNAME** a HDInsight fürt nevű:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    További információ az SSH tooconnect tooyour HDInsight használatával fürt című [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

2. A következő parancs toostart példatopológia hello használata:

        storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-*.jar org.apache.storm.starter.WordCountTopology WordCount

    Indítja el a hello példa WordCount topológia hello fürtön. Ez a topológia véletlenszerű előállításához mondatokat és count hello előfordulásainak kívánt számát, az összes szó a hello mondatokat.

   > [!NOTE]
   > Topológia toohello fürtre történő elküldésekor, át kell másolnia hello használata előtt hello fürtöt tartalmazó jar-fájlra hello `storm` parancsot. toocopy hello toohello fájlfürt, hello használható `scp` parancsot. Például: `scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`
   >
   > hello WordCount-példa és egyéb storm starter-példák megtalálhatóak a fürthöz `/usr/hdp/current/storm-client/contrib/storm-starter/`.

## <a name="submit-a-topology-programmatically"></a>Küldje el a topológia: programozott módon

A HDInsight a topológia tooStorm kommunikál a fürtön tárolt Nimbus szolgáltatás hello programozott módon telepítheti. [https://github.com/Azure-Samples/hdinsight-Java-Deploy-Storm-topology](https://github.com/Azure-Samples/hdinsight-java-deploy-storm-topology) példával Java-alkalmazás, amely bemutatja, hogyan toodeploy és a topológia keresztül hello Nimbus szolgáltatás elindításához.

## <a name="monitor-and-manage-visual-studio"></a>Megfigyelés és kezelés: Visual Studio

Ha egy topológia sikeresen elküldte a Visual Studio használatával, hello **Storm-topológiák** nézetére hello fürt megjelenik-e. Válassza ki a hello topológiát hello lista tooview információt a futtató topológia hello.

![a Visual studio-figyelő](./media/hdinsight-storm-deploy-monitor-topology/vsmonitor.png)

> [!NOTE]
> Megtekintheti továbbá **Storm-topológiák** a **Server Explorer** kibontásával **Azure** > **HDInsight**, és, majd kattintson a jobb gombbal a Storm on HDInsight-fürt, majd válassza **nézet Storm-topológiák**.

Válassza ki hello alakzat hello spoutok vagy boltok tooview információ ezeket az összetevőket. Minden elem kijelölve egy új ablakban nyílik meg.

### <a name="deactivate-and-reactivate"></a>Inaktiválása és újraaktiválása

A topológia inaktiválása felfüggeszti, amíg következtében leállt, vagy újra aktiválni. tooperform ezeket a műveleteket, használja a hello __Deactivate__ és __újraaktiválása__ hello hello tetején gombok __topológia összegzése__.

### <a name="rebalance"></a>Visszaegyensúlyozás

A topológia újraelosztás lehetővé teszi, hogy a hello rendszer toorevise hello párhuzamosságát hello topológia. Például ha hello fürt tooadd átméretezte további megjegyzéseket, újraelosztás lehetővé teszi a topológia toosee hello új csomópontok.

a topológia toorebalance hello használata __Visszaegyensúlyozás__ hello hello tetején gomb __topológia összegzése__.

> [!WARNING]
> A topológia újraelosztás először inaktiválja hello topológia, munkavállalók egyenletesen újraterjeszti hello fürtön, majd végül adja vissza a hello topológia toohello állapota megelőző újraelosztás történt. Ezért ha hello topológia már aktív volt, akkor ismét aktívvá válik. Inaktiváltuk, ha a leválasztást inaktív.

### <a name="kill-a-topology"></a>A topológia leállítása

Storm-topológiák továbbra is fut le vannak állítva, vagy hello fürt törlésekor. a topológia toostop hello használata __Kill__ hello hello tetején gomb __topológia összegzése__.

## <a name="monitor-and-manage-ssh-and-hello-storm-command"></a>Megfigyelés és kezelés: SSH és hello Storm parancs

Hello `storm` segédprogram lehetővé teszi a futó topológiákkal hello parancssorból toowork. Használjon `storm -h` parancsok teljes listáját.

### <a name="list-topologies"></a>Lista topológiák

A következő parancs toolist hello minden futó topológiákat használhatja:

    storm list

Ez a parancs visszaadja a szöveg a következő információk hasonló toohello:

    Topology_name        Status     Num_tasks  Num_workers  Uptime_secs
    -------------------------------------------------------------------
    WordCount            ACTIVE     29         2            263

### <a name="deactivate-and-reactivate"></a>Inaktiválása és újraaktiválása

A topológia inaktiválása felfüggeszti, amíg következtében leállt, vagy újra aktiválni. A következő parancs toodeactivate hello és és aktiválja újra:

    storm Deactivate TOPOLOGYNAME

    storm Activate TOPOLOGYNAME

### <a name="kill-a-running-topology"></a>A futó topológiát leállítása

Storm-topológiák elindításakor, akkor folytassa csak a futó. a topológia toostop hello a következő parancsot használja:

    storm kill TOPOLOGYNAME

### <a name="rebalance"></a>Visszaegyensúlyozás

A topológia újraelosztás lehetővé teszi, hogy a hello rendszer toorevise hello párhuzamosságát hello topológia. Például ha hello fürt tooadd átméretezte további megjegyzéseket, újraelosztás lehetővé teszi a topológia toosee hello új csomópontok.

> [!WARNING]
> A topológia újraelosztás először inaktiválja hello topológia, munkavállalók egyenletesen újraterjeszti hello fürtön, majd végül adja vissza a hello topológia toohello állapota megelőző újraelosztás történt. Ezért ha hello topológia már aktív volt, akkor ismét aktívvá válik. Inaktiváltuk, ha a leválasztást inaktív.

    storm rebalance TOPOLOGYNAME

## <a name="monitor-and-manage-storm-ui"></a>Megfigyelés és kezelés: Storm felhasználói felülete

hello Storm felhasználói felülete webes felületet biztosít a futó topológiákkal dolgozik, és szerepel-e a HDInsight-fürthöz. tooview hello Storm felhasználói felülete, használjon egy webes böngésző tooopen **https://CLUSTERNAME.azurehdinsight.net/stormui**, ahol **CLUSTERNAME** hello a fürt neve van.

> [!NOTE]
> Ha tooprovide kéri a felhasználónevet és jelszót, adja meg a hello Fürtrendszergazda (rendszergazda) és a használt jelszó létrehozása hello fürt.

### <a name="main-page"></a>Főoldala

hello fő lapján hello Storm felhasználói felülete hello a következő információkat biztosítja:

* **A fürt összefoglaló**: hello Storm-fürt alapvető adatait.
* **Összegző topológia**: futó topológiákat. Ez a szakasz tooview a hello hivatkozások használata az további információt adott topológiák kapcsolatos.
* **Felügyelő összefoglaló**: hello Storm felügyelő kapcsolatos információkat.
* **Nimbus konfigurációs**: hello fürt Nimbus konfigurációjában.

### <a name="topology-summary"></a>Topológia összegzése

Egy hivatkozási kiválasztása a hello **topológia összegzése** szakasz hello topológiára vonatkozó adatokat a következő hello jeleníti meg:

* **Összegző topológia**: hello topológia alapvető adatait.
* **Topológia műveletek**: hello topológia végrehajtható felügyeleti műveleteket.

  * **Aktiválása**: folytatása inaktivált topológia feldolgozását.
  * **Inaktiválása**: megszakítja a futó topológiát.
  * **Visszaegyensúlyozás**: hello topológia párhuzamosságát hello módosíthatja. Futó topológiákat kell egyensúlyba, miután módosította hello fürtben található csomópontok számának hello. Ez a művelet lehetővé teszi, hogy hello topológia tooadjust párhuzamossági toocompensate hello növelhető vagy csökkenthető, hello fürtben található csomópontok számát.

    További információkért lásd: <a href="http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html" target="_blank">ismertetése a Storm-topológia párhuzamosságát hello</a>.
  * **Kill**: a Storm-topológia leállítása után hello megadott időkorlát.
* **Topológiastatisztikák**: hello topológia statisztikája. tooset hello hello oldalon bejegyzések fennmaradó hello határideje, hello hello hivatkozásokkal **ablak** oszlop.
* **Spoutok**: hello hello topológia által használt spoutokkal kapcsolatban. Ez a szakasz tooview a hello hivatkozások használata az adott spoutokkal kapcsolatban további információt.
* **Boltok**: hello boltokhoz hello topológia használják. Ez a szakasz tooview a hello hivatkozások használata az adott boltokhoz további információt.
* **Topológiakonfiguráció**: hello hello kiválasztott topológia beállításától.

### <a name="spout-and-bolt-summary"></a>Spout és Bolt összegzése

Egy spout kijelölésével hello **Spoutok** vagy **boltok** szakaszok hello kijelölt elemre vonatkozó információkat a következő hello jeleníti meg:

* **Az összetevő összegzés**: hello spout vagy bolt alapvető adatait.
* **Spout vagy Bolt statisztikák**: hello statisztikája spout vagy boltok esetében. tooset hello hello oldalon bejegyzések fennmaradó hello határideje, hello hello hivatkozásokkal **ablak** oszlop.
* **Beviteli statisztikák** (csak boltok esetében): hello információ bemenetét hello bolt által használt.
* **Kimeneti statisztikák**: hello adatfolyamok által kibocsátott információ spout vagy boltok esetében.
* **Végrehajtója**: hello spout vagy bolt hello példányaira vonatkozó információkat. Jelölje be hello **Port** egy adott végrehajtó tooview bejegyzést a diagnosztikai adatokat tartalmazó naplófájlt előállított erre a példányra.
* **Hibák**: bármilyen hiba adatokat a spout vagy boltok esetében.

## <a name="monitor-and-manage-rest-api"></a>Megfigyelés és kezelés: REST API-n

hello Storm felhasználói felülete hello REST API-t épül, ezért hasonló felügyeleti és figyelési funkcióit, hello REST API használatával végezheti el. Felügyelheti és figyelheti a Storm-topológiák hello REST API toocreate egyéni eszközöket is használhatja.

További információkért lásd: [Storm UI REST API](http://storm.apache.org/releases/0.9.6/STORM-UI-REST-API.html). hello következő információkat az adott toousing hello REST API-t a HDInsight alatt futó Apache Storm.

> [!IMPORTANT]
> nincs nyilvánosan elérhető hello Storm REST API-t több mint hello internet, és egy SSH alagút toohello HDInsight fürt átjárócsomópontjába használatával kell elérni. Létrehozásával és az SSH-alagút használatával további információkért lásd: [használata SSH Tunneling tooaccess Ambari web UI, ResourceManager, JobHistory, NameNode, Oozie és egyéb web UI](hdinsight-linux-ambari-ssh-tunnel.md).

### <a name="base-uri"></a>Alap URI

hello alap URI-Azonosítójának a Linux-alapú HDInsight-fürtök REST API hello érhető el: hello átjárócsomópont **https://HEADNODEFQDN:8744/api/v1/**. hello átjárócsomópont hello tartománynevet jön létre a fürt létrehozása során, és nincs statikus.

A különféle módokon található hello teljesen minősített tartománynevét (FQDN) hello átjárócsomóponthoz:

* **Az SSH-munkamenetet**: hello paranccsal `headnode -f` egy SSH-munkamenet toohello fürtből.
* **Az Ambari webes**: válasszon **szolgáltatások** hello hello oldal tetejére, majd válassza ki **Storm**. A hello **összegzés** lapon jelölje be **Storm felhasználói felület Server**. hello hello Storm felhasználói felülete és a REST API-t futtató hello csomópont teljes Tartományneve van hello oldal hello tetején.
* **Az Ambari REST API**: hello paranccsal `curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/STORM/components/STORM_UI_SERVER"` tooretrieve információ a Storm felhasználói felület és a REST API hello hello csomópontot futtat. Cserélje le **jelszó** hello fürt hello rendszergazdai jelszóval. Cserélje le **CLUSTERNAME** hello fürt névvel. Hello válaszul hello "gazdaszámítógép_neve" bejegyzés tartalmazza hello hello csomópont teljesen minősített Tartománynevét.

### <a name="authentication"></a>Authentication

REST API-t kell használnia kérelmek toohello **az egyszerű hitelesítés**, így a hello HDInsight fürt rendszergazdája nevét és jelszavát használja.

> [!NOTE]
> Egyszerű hitelesítést a rendszer a tiszta szöveges küldi el, mert akkor **mindig** hello fürt toosecure HTTPS-kommunikációhoz használni.

### <a name="return-values"></a>Visszatérési érték

REST API-t csak lehet hello fürtön belül használható, vagy a virtuális gépek azonos Azure Virtual Network hello fürtként hello hello visszaadott adatokat. Például hello teljesen minősített tartománynevét (FQDN) Zookeeper kiszolgálók visszaadott értéke nem érhető el hello Internet.

## <a name="next-steps"></a>Következő lépések

Most, hogy megismerte a hogyan toodeploy és a figyelő topológiák használatával hello a Storm irányítópultja, megtudhatja, hogyan túl[Maven használatával fejlesztése Java-alapú topológiák](hdinsight-storm-develop-java-topology.md).

További példa topológiák listájáért lásd: [a HDInsight alatt futó Storm](hdinsight-storm-example-topology.md).
