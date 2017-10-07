---
title: "aaaInstall és Giraph használata a HDInsight (Hadoop) - Azure-on |} Microsoft Docs"
description: "Ismerje meg, hogyan tooinstall Giraph a Linux-alapú HDInsight clusters Parancsfájlműveletek használatával. A Parancsfájlműveletek lehetővé teszik toocustomize hello fürt létrehozásakor fürtkonfiguráció módosításával, vagy a szolgáltatások és segédprogramok telepítése."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 9fcac906-8f06-4002-9fe8-473e42f8fd0f
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: 0f195b65cebf5e24d1808ef33b95b4d362555521
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-giraph-on-hdinsight-hadoop-clusters-and-use-giraph-tooprocess-large-scale-graphs"></a>HDInsight Hadoop-fürtök Giraph telepítse, és Giraph tooprocess nagyméretű diagramjait használata

Megtudhatja, hogyan tooinstall Apache Giraph a HDInsight-fürtöt. hello parancsfájl művelet a HDInsight lehetővé teszi toocustomize a fürt bash parancsfájl futtatásával. Parancsfájlok lehet használt toocustomize fürtök alatt és a fürt létrehozása után.

> [!IMPORTANT]
> hello jelen dokumentumban leírt lépések egy HDInsight-fürt által használt Linux igényelnek. Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="whatis"></a>Mi az a Giraph

[Apache Giraph](http://giraph.apache.org/) lehetővé teszi a Hadoop használatával tooperform diagramfeldolgozás, és az Azure HDInsight használható. Diagramokat objektumok közötti kapcsolatok modellezésére. Például hello kapcsolatok nagy hálózati útválasztók között, mint például hello Internet, vagy a közösségi hálózatokkal személyek közötti kapcsolatok. Graph feldolgozás lehetővé teszi a tooreason kapcsolatos hello kapcsolatai egy grafikonon objektumok, mint:

* A jelenlegi kapcsolatok alapján lehetséges ismerősök azonosítása.

* Azonosító hello legrövidebb útvonal hálózatban két számítógép között.

* A weblapok hello lap indulva számított rangját kiszámítása.

> [!WARNING]
> Hello HDInsight-fürt összetevői teljes mértékben támogatottak, mert a Microsoft Support tooisolate segítségével, és hárítsa el a problémákat kapcsolódó toothese összetevőket.
>
> Egyéni összetevők, például Giraph, minden üzleti szempontból ésszerű támogatási toohelp fogadni, toofurther hello problémával kapcsolatos hibaelhárítás elősegítéséhez. Microsoft Support képes tooresolving hello probléma lehet. Ha nem, ahol részletes segítséget, hogy a technológiát található nyílt forráskódú Közösségek kell tájékozódnia. Például nincsenek sok közösségi webhelyek használható, például: [MSDN fórum hdinsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Is Apache projektek rendelkezik projekt helyek [http://apache.org](http://apache.org), például: [Hadoop](http://hadoop.apache.org/).


## <a name="what-hello-script-does"></a>Milyen hello parancsprogram

Ez a parancsfájl hello a következő műveleteket hajtja végre:

* Giraph túl telepíti`/usr/hdp/current/giraph`

* Másolatot hello `giraph-examples.jar` fájltárolás toodefault (WASB) a fürt:`/example/jars/giraph-examples.jar`

## <a name="install"></a>A Parancsfájlműveletek segítségével Giraph telepítése

Egy minta parancsfájlt tooinstall Giraph egy HDInsight-fürt hello a következő helyen érhető el:

    https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh

Ez a szakasz útmutatás hogyan toouse hello hello Azure-portál használatával hello fürt létrehozásakor minta parancsfájlt.

> [!NOTE]
> A parancsfájlművelet hello a következő módszerek bármelyikével alkalmazhatók:
> * Azure PowerShell
> * hello Azure parancssori felület
> * hello HDInsight .NET SDK
> * Azure Resource Manager-sablonok
> 
> A futó fürtök parancsfájl műveletek tooalready is alkalmazhat. További információkért lásd: [testreszabása HDInsight-fürtök parancsfájlműveletekkel](hdinsight-hadoop-customize-cluster-linux.md).

1. Indítsa el a fürt létrehozása a hello lépések segítségével [létrehozása Linux-alapú HDInsight-fürtök](hdinsight-hadoop-create-linux-clusters-portal.md), de nem hajtja végre létrehozása.

2. A hello **opcionális konfigurációs** panelen válassza **Parancsfájlműveletek**, és adja meg a következő információ hello:

   * **NÉV**: Adjon meg egy rövid nevet hello parancsfájlművelet.

   * **PARANCSFÁJL URI azonosítója**: https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh

   * **HEAD**: Ez a bejegyzés ellenőrzése

   * **MUNKAVÉGZŐ**: hagyja bejelölve ezt a bejegyzést

   * **ZOOKEEPER**: hagyja bejelölve ezt a bejegyzést

   * **PARAMÉTEREK**: ezt a mezőt hagyja üresen

3. Hello hello alján **Parancsfájlműveletek**, hello használata **válasszon** gombok toosave hello beállítása. Végezetül a hello használata **kiválasztása** hello hello alján gomb **opcionális konfigurációs** panel toosave hello opcionális konfigurációs információkat.

4. Hello fürtöt hoz létre, a folytatáshoz [létrehozása Linux-alapú HDInsight-fürtök](hdinsight-hadoop-create-linux-clusters-portal.md).

## <a name="usegiraph"></a>Giraph használata a Hdinsightban

Hello fürt létrehozása után használja a következő lépéseket toorun hello SimpleShortestPathsComputation példa Giraph mellékelt hello. Ez a példa hello basic [Pregel](http://people.apache.org/~edwardyoon/documents/pregel.pdf) megvalósítása a hello legrövidebb elérési út egy grafikonon objektumok közötti kereséshez.

1. Csatlakozzon az SSH használatával toohello HDInsight-fürt:

    ```bash
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    További információk: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Használjon hello következő parancsot a toocreate nevű fájlt **tiny_graph.txt**:

    ```bash
    nano tiny_graph.txt
    ```

    Szöveg hello a fájl tartalmát, a következő hello használata:

    ```text
    [0,0,[[1,1],[3,3]]]
    [1,0,[[0,1],[2,2],[3,1]]]
    [2,0,[[1,2],[4,4]]]
    [3,0,[[0,3],[1,1],[4,4]]]
    [4,0,[[3,4],[2,4]]]
    ```

    Ezek az adatok egy irányított gráf hello formátum használatával az objektumok közötti kapcsolatot ismerteti `[source_id, source_value,[[dest_id], [edge_value],...]]`. Minden egyes sorban közötti kapcsolatot jelent a `source_id` objektum és egy vagy több `dest_id` objektumok. Hello `edge_value` -re hello erőssége vagy hello kapcsolat közötti távolság `source_id` és `dest\_id`.

    Jelenik meg, és objektumok közötti távolság hello hello érték (vagy súly) használ, hello adatok nézhet ki például a következő diagram hello:

    ![különböző távolságát sornyi kör Megrajzolás tiny_graph.txt](./media/hdinsight-hadoop-giraph-install-linux/giraph-graph.png)

3. toosave hello fájl használata **Ctrl + X**, majd **Y**, és végül **Enter** tooaccept hello fájl nevét.

4. A HDInsight-fürthöz elsődleges tárba toostore hello adatokat követő hello használata:

    ```bash
    hdfs dfs -put tiny_graph.txt /example/data/tiny_graph.txt
    ```

5. Futtassa a hello SimpleShortestPathsComputation példa hello a következő parancs használatával:

    ```bash
    yarn jar /usr/hdp/current/giraph/giraph-examples.jar org.apache.giraph.GiraphRunner org.apache.giraph.examples.SimpleShortestPathsComputation -ca mapred.job.tracker=headnodehost:9010 -vif org.apache.giraph.io.formats.JsonLongDoubleFloatDoubleVertexInputFormat -vip /example/data/tiny_graph.txt -vof org.apache.giraph.io.formats.IdWithValueTextOutputFormat -op /example/output/shortestpaths -w 2
    ```

    Ez a parancs paramétereit hello hello a következő táblázat ismerteti:

   | Paraméter | Funkció |
   | --- | --- |
   | `jar` |hello jar-fájlt tartalmazó hello példák. |
   | `org.apache.giraph.GiraphRunner` |hello osztályt használja a toostart hello példák. |
   | `org.apache.giraph.examples.SimpleShortestPathsCoputation` |hello példára. Ebben a példában kiszámítja hello legrövidebb elérési út azonosítója 1 és más hello Graph azonosítók között. |
   | `-ca mapred.job.tracker` |hello headnode hello fürthöz. |
   | `-vif` |hello bemeneti formátum toouse hello bemeneti adatok. |
   | `-vip` |hello bemeneti adatfájlt. |
   | `-vof` |hello kimeneti formátum. A példa, azonosítója és egyszerű szövegként érték. |
   | `-op` |hello kimeneti helyen. |
   | `-w 2` |feldolgozók toouse hello száma. Ebben a példában, 2. |

    Ezeket és más Giraph minták használt paraméterek további információkért lásd: hello [Giraph gyors üzembe helyezés](http://giraph.apache.org/quick_start.html).

6. Miután hello feladat befejeződött, hello eredmények hello vannak tárolva **/example/out/shotestpaths** könyvtár. hello kimeneti fájl kezdődő **rész-m -** és végződhet azonosítószámát hello először, másrészt fájl stb. A következő parancs tooview hello kimeneti hello használata:

    ```bash
    hdfs dfs -text /example/output/shortestpaths/*
    ```

    hello kimeneti megjelenjen-e a következő szöveg hasonló toohello:

        0    1.0
        4    5.0
        2    2.0
        1    0.0
        3    1.0

    hello SimpleShortestPathComputation példa nél nagyobb toostart objektum azonosítója 1 és hello legrövidebb elérési tooother objektumok keresése. hello kimeneti hello formátumban van `destination_id` és `distance`. Hello `distance` , de hello érték (súly) amennyi hello szegélye között objektum azonosítója 1 és hello cél azonosítót.

    Ezek az adatok megjelenítése, ellenőrizheti hello eredmények hello legrövidebb elérési utak utazás azonosítója 1 és egyéb objektumok között. hello legrövidebb azonosítója 1 és 4 azonosító közötti elérési út 5. Az értéket nem hello teljes közötti távolság szerint <span style="color:orange">azonosítója 1. és 3</span>, majd <span style="color:red">azonosító 3. és 4</span>.

    ![Az objektumok rajzolási körök mint a legrövidebb elérési utak között](./media/hdinsight-hadoop-giraph-install-linux/giraph-graph-out.png)

## <a name="next-steps"></a>Következő lépések

* [Telepítheti és használhatja a HDInsight-fürtök Hue](hdinsight-hadoop-hue-linux.md).

* [Solr telepíthető a HDInsight-fürtök](hdinsight-hadoop-solr-install-linux.md).
