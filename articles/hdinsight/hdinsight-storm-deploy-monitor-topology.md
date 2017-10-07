---
title: "aaaDeploy és kezelése a HDInsight alatt futó Apache Storm-topológiák |} Microsoft Docs"
description: "Ismerje meg, hogyan toodeploy, figyelheti és kezelheti a Storm irányítópultja hello használata a HDInsight alatt futó Apache Storm-topológiák. Visual Studio Hadoop-eszközök használata."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 5e542072-f014-42aa-82d6-2694a76df520
ms.service: hdinsight
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/01/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: 05f05fe8dd519fe99fb771d36bfc3d28168ca85f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-manage-apache-storm-topologies-on-windows-based-hdinsight"></a>Központi telepítése és kezelése a Windows-alapú HDInsight alatt futó Apache Storm-topológiák

a Storm irányítópultja hello lehetővé teszi a tooeasily telepítésére és alatt futó Apache Storm topológiák tooyour HDInsight-fürt futtatására a webböngésző használatával. Is hello irányítópult toomonitor használja, és kezelheti a futó topológiákat. Visual Studio használata, ha a hello a HDInsight Tools for Visual Studio tartalmaz a Visual Studio hasonló funkciókat.

a Storm irányítópultja hello és hello Storm szolgáltatások hello a HDInsight Tools támaszkodjon hello Storm REST API, amely lehet használt toocreate saját figyelése és felügyeleti megoldásokat.

> [!IMPORTANT]
> hello jelen dokumentumban leírt lépések egy Storm on HDInsight-fürt által használt Windows hello operációs rendszert igényelnek. Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).
>
> Telepítésével és a HDInsight-fürtök által használt Linux Storm-topológiák kezelésével kapcsolatos információkért lásd: [központi telepítése és kezelése a Linux-alapú HDInsight alatt futó Apache Storm-topológiák](hdinsight-storm-deploy-monitor-topology-linux.md)

## <a name="prerequisites"></a>Előfeltételek

* **A HDInsight alatt futó Apache Storm** -lásd [első lépései a HDInsight alatt futó Apache Storm](hdinsight-apache-storm-tutorial-get-started.md) fürt létrehozásának lépései.

* A hello **a Storm irányítópultja**: HTML5-támogatással rendelkező modern webböngésző.

* A **Visual Studio** -2.5.1-es verzióval készült Azure SDK újabb vagy és a HDInsight Tools for Visual Studio hello. Lásd: [első lépései a HDInsight Tools for Visual Studio használatával](hdinsight-hadoop-visual-studio-tools-get-started.md) tooinstall és hello a HDInsight tools for Visual Studio konfigurálása.

    A következő Visual Studio verziójának hello egyikét:

  * A Visual Studio 2012 Update 4

  * Visual Studio 2013 4. frissítéssel vagy Visual Studio 2013 Community

  * A Visual Studio 2015 (minden kiadás)

  * A Visual Studio 2017 (minden kiadás)

## <a name="storm-dashboard"></a>A Storm irányítópultja

a Storm irányítópultja hello kerül, a Storm-fürt érhető el. hello URL-cím **https://&lt;clustername >.azurehdinsight.net/**, ahol **clustername** hello a Storm on HDInsight-fürt neve.

Válassza ki a Storm irányítópultja hello hello felső részén **nyújt topológia**. Kövesse az utasításokat hello hello lap toorun egy minta topológia vagy tooupload, és futtassa a topológia létrehozott.

![hello küldje el a topológia lapon][storm-dashboard-submit]

### <a name="storm-ui"></a>A Storm felhasználói felülete

A Storm irányítópultja hello, válassza ki hello **Storm felhasználói felülete** hivatkozásra. A futó topológiákkal hozzáadása tooany hello fürt, információkat jelenít meg.

![hello storm felhasználói felülete][storm-dashboard-ui]

> [!NOTE]
> Internet Explorer egyes verziói azt tapasztalhatja, hogy hello Storm felhasználói felülete követően először ellátogatott nem frissül. Például akkor lehet, hogy megjelenni hello új topológiák meg, vagy azt előfordulhat, hogy a topológia aktívnak amikor korábban inaktív. Microsoft ismeri a problémát, és a megoldás működik.

#### <a name="main-page"></a>Főoldala

hello fő lapján hello Storm felhasználói felülete hello a következő információkat biztosítja:

* **A fürt összefoglaló**: hello Storm-fürt alapvető adatait.

* **Összegző topológia**: futó topológiákat. Ez a szakasz tooview a hello hivatkozások használata az további információt adott topológiák kapcsolatos.

* **Felügyelő összefoglaló**: hello Storm felügyelő kapcsolatos információkat.

* **Nimbus konfigurációs**: hello fürt Nimbus konfigurációjában.

#### <a name="topology-summary"></a>Topológia összegzése

Egy hivatkozási kiválasztása a hello **topológia összegzése** szakasz hello topológiára vonatkozó adatokat a következő hello jeleníti meg:

* **Összegző topológia**: hello topológia alapvető adatait.

* **Topológia műveletek**: hello topológia végrehajtható felügyeleti műveleteket.

  * **Aktiválása**: folytatása inaktivált topológia feldolgozását.

  * **Inaktiválása**: megszakítja a futó topológiát.

  * **Visszaegyensúlyozás**: hello topológia párhuzamosságát hello módosíthatja. Futó topológiákat kell egyensúlyba, miután módosította hello fürtben található csomópontok számának hello. Ez lehetővé teszi, hogy hello topológia tooadjust párhuzamossági toocompensate hello növelhető vagy csökkenthető, hello fürtben található csomópontok számát.

      További információkért lásd: [ismertetése a Storm-topológia (http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) hello párhuzamosságát](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html).

  * **Kill**: a Storm-topológia leállítása után hello megadott időkorlát.

* **Topológiastatisztikák**: hello topológia statisztikája. Hello hello hivatkozásokkal **ablak** oszlop tooset hello időkeretre hello fennmaradó bejegyzések hello oldalon.

* **Spoutok**: hello hello topológia által használt spoutokkal kapcsolatban. Ez a szakasz tooview a hello hivatkozások használata az adott spoutokkal kapcsolatban további információt.

* **Boltok**: hello boltokhoz hello topológia használják. Ez a szakasz tooview a hello hivatkozások használata az adott boltokhoz további információt.

* **Topológiakonfiguráció**: hello hello kiválasztott topológia beállításától.

#### <a name="spout-and-bolt-summary"></a>Spout és Bolt összegzése

Egy spout kijelölésével hello **Spoutok** vagy **boltok** szakaszok hello kijelölt elemre vonatkozó információkat a következő hello jeleníti meg:

* **Az összetevő összegzés**: hello spout vagy bolt alapvető adatait.

* **Spout vagy Bolt statisztikák**: hello statisztikája spout vagy boltok esetében. Hello hello hivatkozásokkal **ablak** oszlop tooset hello időkeretre hello fennmaradó bejegyzések hello oldalon.

* **Beviteli statisztikák** (csak boltok esetében): hello információ bemenetét hello bolt által használt.

* **Kimeneti statisztikák**: hello adatfolyamok által kibocsátott információ spout vagy boltok esetében.

* **Végrehajtója**: hello spout vagy bolt hello példányaira vonatkozó információkat. Jelölje be hello **Port** egy adott végrehajtó tooview bejegyzést a diagnosztikai adatokat tartalmazó naplófájlt előállított erre a példányra.

* **Hibák**: bármilyen hiba adatokat a spout vagy boltok esetében.

## <a name="hdinsight-tools-for-visual-studio"></a>HDInsight Tools for Visual Studio

a HDInsight Tools hello lehet használt toosubmit C# vagy hibrid topológiák tooyour Storm-fürt. a lépéseket követve hello mintaalkalmazás használja. A saját topológiák hello a HDInsight Tools használatával létrehozásával kapcsolatos további információkért lásd: [fejlesztésére C#-topológiák hello HDInsight Tools for Visual Studio használatával](hdinsight-storm-develop-csharp-visual-studio-topology.md).

Hello követő lépéseket toodeploy egy minta tooyour Storm on HDInsight-fürt használatára, majd megtekintheti, és hello topológia kezelése.

1. Ha még nem telepítette a HDInsight Tools hello hello legújabb verzióját a Visual Studio, lásd: [első lépései a HDInsight Tools for Visual Studio használatával](hdinsight-hadoop-visual-studio-tools-get-started.md).

2. Nyissa meg a Visual Studio, válassza ki **fájl** > **új** > **projekt**.

3. A hello **új projekt** párbeszédpanelen bontsa ki **telepített** > **sablonok**, majd válassza ki **HDInsight**. Válassza ki a sablonok hello listáról **Storm minta**. Hello hello párbeszédpanel alsó részén írja be a hello alkalmazás nevét.

    ![Kép](./media/hdinsight-storm-deploy-monitor-topology/sample.png)

4. A **Megoldáskezelőben**, kattintson a jobb gombbal a hello projektet, és válassza ki **elküldeni a HDInsight tooStorm**.

   > [!NOTE]
   > Ha a rendszer kéri, adja meg hello bejelentkezési hitelesítő adatait az Azure-előfizetéshez. Ha egynél több előfizetéssel rendelkezik, jelentkezzen be a Storm on HDInsight-fürt tartalmazó toohello.

5. Válassza ki a Storm on HDInsight-fürt hello **Storm-fürt** legördülő listából válassza ki, és válassza **Submit**. Figyelheti, hogy hello elküldése sikeres hello segítségével **kimeneti** ablak.

6. Amikor hello topológia sikeresen elküldte, hello **Storm-topológiák** a hello fürt megjelenjen-e. Válassza ki a hello topológiát hello lista tooview információt a futtató topológia hello.

    ![a Visual studio-figyelő](./media/hdinsight-storm-deploy-monitor-topology/vsmonitor.png)

   > [!NOTE]
   > Megtekintheti továbbá **Storm-topológiák** a **Server Explorer** kibontásával **Azure** > **HDInsight**, és, majd kattintson a jobb gombbal a Storm on HDInsight-fürt, majd válassza **nézet Storm-topológiák**.

    Válassza ki hello alakzat hello spoutok vagy boltok tooview információ ezeket az összetevőket. Minden elem kijelölve egy új ablakban nyílik meg.

   > [!NOTE]
   > hello topológia hello neve nem hello topológia hello osztály neve (ebben az esetben `HelloWord`,) az időbélyegzőnek lesz hozzáfűzve.

7. A hello **topológia összegzése** nézetben jelölje ki **Kill** toostop hello topológia.

   > [!NOTE]
   > Storm-topológiák továbbra is fut le vannak állítva, vagy hello fürt törlésekor.


## <a name="rest-api"></a>REST API

hello Storm felhasználói felülete hello REST API-t épül, ezért hasonló felügyeleti és figyelési funkcióit, hello REST API használatával végezheti el. Felügyelheti és figyelheti a Storm-topológiák hello REST API toocreate egyéni eszközöket is használhatja.

További információkért lásd: [Storm UI REST API](https://github.com/apache/storm/blob/0.9.3-branch/STORM-UI-REST-API.md). hello következő információkat az adott toousing hello REST API-t a HDInsight alatt futó Apache Storm.

### <a name="base-uri"></a>Alap URI

a HDInsight-fürtök REST API hello alap URI hello **https://&lt;clustername >.azurehdinsight.net/stormui/api/v1/**, ahol **clustername** hello alatt futó Storm-neve megtalálható HDInsight-fürthöz.

### <a name="authentication"></a>Authentication

REST API-t kell használnia kérelmek toohello **az egyszerű hitelesítés**, így a hello HDInsight fürt rendszergazdája nevét és jelszavát használja.

> [!NOTE]
> Egyszerű hitelesítést a rendszer a tiszta szöveges küldi el, mert akkor **mindig** hello fürt toosecure HTTPS-kommunikációhoz használni.

### <a name="return-values"></a>Visszatérési érték

REST API-t csak lehet hello fürtön belül használható, vagy a virtuális gépek azonos Azure Virtual Network hello fürtként hello hello visszaadott adatokat. Például hello teljesen minősített tartománynevét (FQDN) érkezett Zookeeper kiszolgálók a rendszer nem érhető el hello Internet.

## <a name="next-steps"></a>Következő lépések

Most, hogy megismerte a hogyan toodeploy és a figyelő topológiák használatával hello a Storm irányítópultja, megtudhatja, hogyan:

* [Fejlesztésére C#-topológiák hello HDInsight Tools for Visual Studio használatával](hdinsight-storm-develop-csharp-visual-studio-topology.md)

* [Maven használatával Java-alapú topológiák fejlesztése](hdinsight-storm-develop-java-topology.md)

További példa topológiák listájáért lásd: [a HDInsight alatt futó Storm](hdinsight-storm-example-topology.md).

[hdinsight-dashboard]: ./media/hdinsight-storm-deploy-monitor-topology/dashboard-link.png
[storm-dashboard-submit]: ./media/hdinsight-storm-deploy-monitor-topology/submit.png
[storm-dashboard-ui]: ./media/hdinsight-storm-deploy-monitor-topology/storm-ui-summary.png
