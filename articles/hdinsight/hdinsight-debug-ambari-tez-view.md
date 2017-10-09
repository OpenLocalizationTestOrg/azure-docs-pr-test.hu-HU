---
title: "a HDInsight - Azure Ambari Tez nézet aaaUse |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello Ambari Tez megtekintése a HDInsight toodebug Tez feladatokhoz."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 9c39ea56-670b-4699-aba0-0f64c261e411
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/12/2017
ms.author: larryfr
ms.openlocfilehash: 5d61bd0403c98284c86982073af91468ae62ac60
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-ambari-views-toodebug-tez-jobs-on-hdinsight"></a>Az Ambari nézetek toodebug Tez feladatokhoz a HDInsight használata

hello Ambari webes felhasználói felületén a HDInsight a Tez nézetet tartalmaz, amely lehet Tez használó használt toounderstand és hibakeresési feladatokat. hello Tez nézet lehetővé teszi toovisualize hello feladat csatlakoztatott elemek grafikon elemezze az egyes elemek, valamint statisztikák és naplózási információk beolvasása.

> [!IMPORTANT]
> hello jelen dokumentumban leírt lépések egy HDInsight-fürt által használt Linux igényelnek. Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További információkért lásd: [HDInsight-összetevők verziószámozása](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="prerequisites"></a>Előfeltételek

* A Linux-alapú HDInsight-fürtöt. A fürt létrehozásának lépései: [Ismerkedés a Linux-alapú HDInsight eszközzel](hdinsight-hadoop-linux-tutorial-get-started.md).
* Egy HTML5-támogatással rendelkező modern webböngésző.

## <a name="understanding-tez"></a>Tez ismertetése

Tez egy bővíthető keretrendszer, amely a hagyományos MapReduce feldolgozási-nál nagyobb sebesség biztosítja az adatok feldolgozásához. Linux-alapú HDInsight-fürtök a Hive hello alapértelmezett motor esetén.

Tez egy irányított aciklikus diagramhoz (DAG), amely leírja a feladatok által szükséges műveletek sorrendjét hello hoz létre. Egyes műveletek csúcsban nevezik, és hajtható végre egy hello adat általános feladat. hello tényleges csúcspont szerint hello munkaelem végrehajtásának feladat neve és hello fürt több csomópontja között szétoszthatók.

### <a name="understanding-hello-tez-view"></a>Understanding hello Tez megtekintése

hello Tez nézet futó folyamatok előzményadatok és a információkat biztosít. Ezeket az információkat jeleníti meg, hogyan oszlik meg a feladat más fürtökre. Feladatok és a csúcsban által használt számlálókat is megjeleníti, és a hibainformációk kapcsolódó toohello feladat. Felajánlhatja, hogy a következő forgatókönyvek hello hasznos információkat:

* Hosszan futó folyamatok figyelése, megtekintése hello térkép előrehaladását, és csökkentheti a feladatokat.
* Sikeres vagy sikertelen folyamatok toolearn előzményadatainak elemzése, hogyan lehet javítani, feldolgozás, vagy okát.

## <a name="generate-a-dag"></a>Egy dag-csoport létrehozása

hello nézet csak olyan adatokat, ha egy feladat által használt hello Tez motor Tez jelenleg is fut, vagy a korábban lefutott. Egyszerű Hive-lekérdezéseket is feloldható Tez használata nélkül. Összetettebb lekérdezi, hogy tegye szűrés csoportosítás, rendezés, illesztéseket, stb. hello Tez motort használja.

A következő lépéseket toorun által használt Tez Hive-lekérdezések hello használata:

1. Egy böngészőben nyissa meg a toohttps://CLUSTERNAME.azurehdinsight.net, ahol **CLUSTERNAME** hello neve, a HDInsight-fürthöz.

2. Hello oldal hello tetején hello menüben válassza ki a hello **nézetek** ikonra. Ez az ikon négyzetes több tűnik. A megjelenő hello legördülő menüben válassza ki **Hive view**.

    ![Hive-nézetek kijelölése](./media/hdinsight-debug-ambari-tez-view/selecthive.png)

3. Ha hello Hive nézete betölti, Beillesztés hello következő hello Lekérdezésszerkesztő történő lekérdezése, és kattintson **hajtható végre**.

        select market, state, country from hivesampletable where deviceplatform='Android' group by market, country, state;

    Hello feladat befejezése után jelennek meg hello hello kimenetet kell látnia **lekérdezési folyamat eredményei** szakasz. hello eredmények hasonló toohello szöveg a következő legyen:

        market  state       country
        en-GB   Hessen      Germany
        en-GB   Kingston    Jamaica

4. Jelölje be hello **napló** fülre. Információk a következő szöveg hasonló toohello jelenik meg:

        INFO : Session is already open
        INFO :

        INFO : Status: Running (Executing on YARN cluster with App id application_1454546500517_0063)

    Mentse a hello **alkalmazásazonosító** érték, mivel ezt az értéket használja a következő szakaszban hello.

## <a name="use-hello-tez-view"></a>Tez nézet hello használata

1. Hello oldal hello tetején hello menüben válassza ki a hello **nézetek** ikonra. A megjelenő hello legördülő menüben válassza ki **Tez nézet**.

    ![Tez nézet kijelölése](./media/hdinsight-debug-ambari-tez-view/selecttez.png)

2. Hello Tez nézet betöltésekor hello fürt futott, amely jelenleg fut, vagy hive-lekérdezések listáját láthatja.

    ![Minden DAG](./media/hdinsight-debug-ambari-tez-view/tez-view-home.png)

3. Ha csak egy bejegyzést, az előző szakaszban hello futtatott hello lekérdezés is. Ha több bejegyzés, kereshet hello mezőkkel hello oldal hello tetején.

4. Jelölje be hello **Lekérdezésazonosítóval** a Hive-lekérdezések. Hello lekérdezés információkat jelenít meg.

    ![DAG-részletek](./media/hdinsight-debug-ambari-tez-view/query-details.png)

5. hello lapok ezen a lapon a következő információk tooview hello engedélyezése:

    * **Részletei lekérdezni**: hello Hive-lekérdezések adatait.
    * **Az idősor**: mennyi ideig tartott minden szakaszhoz feldolgozási információt.
    * **Konfigurációk**: hello konfigurálása ehhez a lekérdezéshez használt.

    A __lekérdezés részletei__ használhatja hello hivatkozások toofind információ hello __alkalmazás__ vagy hello __DAG__ ehhez a lekérdezéshez.
    
    * Hello __alkalmazás__ hivatkozás hello YARN alkalmazás ehhez a lekérdezéshez információit jeleníti meg. Itt hello YARN alkalmazásnaplók végezheti el.
    * Hello __DAG__ hivatkozás hello irányított aciklikus diagramhoz ehhez a lekérdezéshez információit jeleníti meg. Itt megtekintheti a hello DAG grafikus ábrázolása. Hello csúcsban belül hello DAG információk is megtalálhatók.

## <a name="next-steps"></a>Következő lépések

Most, hogy megtanulta, hogyan toouse hello Tez meg, további információ [használata a HDInsight Hive](hdinsight-use-hive.md).

Részletesebb műszaki információkat Tez, lásd: hello [Hortonworks Tez lapon](http://hortonworks.com/hadoop/tez/).

Az Ambari és a HDInsight együttes használatával további információkért lásd: [hello Ambari webes felhasználói felület használatával kezelheti a HDInsight-fürtök](hdinsight-hadoop-manage-ambari.md)
