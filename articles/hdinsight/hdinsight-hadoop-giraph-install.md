---
title: "aaaInstall és -felhasználási Giraph a Hadoop fürtök a HDInsight - Azure |} Microsoft Docs"
description: "Megtudhatja, hogyan Giraph toocustomize HDInsight-fürtöt, és hogyan toouse Giraph."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 77a1d0e0-55de-4e61-98a0-060914fb7ca0
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/05/2016
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: bd473faca9d3c87c29d7566a18fc94211c50f059
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-use-giraph-on-windows-based-hdinsight-clusters"></a>Telepítheti és használhatja a Windows-alapú HDInsight-fürtök Giraph

Megtudhatja, hogyan toocustomize Windows alapú HDInsight-fürt a parancsfájl műveletével Giraph, és hogyan toouse Giraph tooprocess nagyméretű diagramjait. Egy Linux-alapú fürttel Giraph használatáról információkért lásd: [Giraph telepítése a HDInsight Hadoop-fürtök (Linux)](hdinsight-hadoop-giraph-install-linux.md).

> [!IMPORTANT]
> Ez a dokumentum csak olyan feladaton végezhető Windows-alapú HDInsight-fürtökkel hello szükséges lépések. HDInsight csak érhető el a Windows korábbi, mint a HDInsight 3.4-es verziójához. Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement). Kapcsolatos információk a Linux-alapú HDInsight-fürt Giraph tooinstall lásd [Giraph telepítése a HDInsight Hadoop-fürtök (Linux)](hdinsight-hadoop-giraph-install-linux.md).


Telepíthető Giraph bármilyen típusú on Azure HDInsight (Hadoop-, Storm, HBase, Spark) fürt segítségével *parancsfájlművelet*. Egy minta parancsfájlt tooinstall Giraph egy HDInsight-fürt érhető el, csak olvasható az Azure storage-blobból [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1). hello mintaparancsfájl csak HDInsight-fürt verziószáma 3.1-es verziójával működik. A HDInsight-fürt verziókról további információkért lásd: [HDInsight-fürt verziókról](hdinsight-component-versioning.md).

**Kapcsolódó cikkek**

* [Giraph telepítse a HDInsight Hadoop-fürtök (Linux)](hdinsight-hadoop-giraph-install-linux.md)
* [Hdinsight Hadoop-fürtök létrehozása](hdinsight-provision-clusters.md): általános információk a HDInsight-fürtök létrehozása.
* [Testre szabhatja a HDInsight-fürtjéhez parancsfájlművelet][hdinsight-cluster-customize]: parancsfájlművelet HDInsight-fürtök testreszabása általános tájékoztatást.
* [Parancsfájlművelet-parancsfájlok fejlesztése a HDInsight](hdinsight-hadoop-script-actions.md).

## <a name="what-is-giraph"></a>Mi az a Giraph?
<a href="http://giraph.apache.org/" target="_blank">Apache Giraph</a> lehetővé teszi a Hadoop használatával tooperform diagramfeldolgozás, és az Azure HDInsight használható. Diagramokat modellhez tartozó objektumok, például hello kapcsolatainak hello Internet, például a nagy hálózaton útválasztók közötti kapcsolatokat, vagy a közösségi hálózatokon (néha hivatkozott tooas közösségi grafikon) személyek közötti kapcsolatok. Graph feldolgozás lehetővé teszi a tooreason kapcsolatos hello kapcsolatai egy grafikonon objektumok, mint:

* A jelenlegi kapcsolatok alapján lehetséges ismerősök azonosítása.
* Azonosító hello legrövidebb útvonal hálózatban két számítógép között.
* A weblapok hello lap indulva számított rangját kiszámítása.

## <a name="install-giraph-using-portal"></a>Telepítse a portál használatával Giraph
1. Indítsa el a fürt létrehozása hello segítségével **egyéni létrehozás** beállítást, a részben ismertetett módon [Hadoop létrehozása a HDInsight-fürtök](hdinsight-provision-clusters.md).
2. A hello **Parancsfájlműveletek** lap hello varázsló, kattintson a **parancsfájlművelet hozzáadása** tooprovide részleteinek hello parancsfájlművelet, alább látható módon:

    ![Használja a fürt parancsfájlművelet toocustomize](./media/hdinsight-hadoop-giraph-install/hdi-script-action-giraph.png "használata parancsfájlművelet toocustomize fürt")

    <table border='1'>
        <tr><th>Tulajdonság</th><th>Érték</th></tr>
        <tr><td>Név</td>
            <td>Adja meg a hello parancsfájlművelet nevét. Például <b>telepítése Giraph</b>.</td></tr>
        <tr><td>A parancsfájl URI azonosítója</td>
            <td>Hello egységes erőforrás-azonosító (URI) toohello parancsfájl megadásához, amely meghívott toocustomize hello fürt. Például <i>https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1</i></td></tr>
        <tr><td>Csomóponttípus</td>
            <td>Adja meg, amelyen hello testreszabási parancsfájl futtatása hello csomópontok. Választhat <b>csomópontjaihoz</b>, <b>csak Átjárócsomópontokat</b>, vagy <b>csak a feldolgozó csomópontok</b>.
        <tr><td>Paraméterek</td>
            <td>Adja meg a hello paraméterek hello parancsfájl által szükség esetén. hello parancsfájl tooinstall Giraph kell paramétert, így ez üresen hagyhatja.</td></tr>
    </table>

    Hello fürtön egynél több parancsprogram művelet tooinstall több összetevőből is hozzáadhat. Miután hozzáadta a hello parancsfájlok, kattintson a hello pipa toostart hello fürtöt hoz létre.

## <a name="use-giraph"></a>A Giraph használata
Hello SimpleShortestPathsComputation példa toodemonstrate hello basic használjuk <a href = "http://people.apache.org/~edwardyoon/documents/pregel.pdf">Pregel</a> megvalósítása a hello legrövidebb elérési út egy grafikonon objektumok közötti kereséshez. A következő lépéseket tooupload hello minta adatok és hello minta jar, egy feladat futtatása hello SimpleShortestPathsComputation példa, majd az eredmények megtekintése hello segítségével hello használata.

1. Töltsön fel egy minta adatok fájl tooAzure Blob Storage tárolóban. A helyi munkaállomáson, hozzon létre egy új fájlt **tiny_graph.txt**. A következő sorokat hello kell tartalmaznia:

        [0,0,[[1,1],[3,3]]]
        [1,0,[[0,1],[2,2],[3,1]]]
        [2,0,[[1,2],[4,4]]]
        [3,0,[[0,3],[1,1],[4,4]]]
        [4,0,[[3,4],[2,4]]]

    Töltse fel a hello tiny_graph.txt toohello elsődleges-tároló a HDInsight-fürthöz. Útmutatást tooupload adatokat, lásd: [feltölteni az adatokat a HDInsight Hadoop-feladatok](hdinsight-upload-data.md).

    Ezek az adatok egy irányított gráf hello formátum használatával az objektumok közötti kapcsolatot ismerteti [forrás\_azonosító, a forrás\_érték, [[cél\_azonosítója], [peremhálózati\_érték],...]]. Minden egyes sorban közötti kapcsolatot jelent a **forrás\_azonosító** objektum és egy vagy több **cél\_azonosító** objektumok. Hello **peremhálózati\_érték** (vagy súly)-re hello erőssége vagy hello kapcsolat közötti távolság **source_id** és **cél\_azonosító**.

    Jelenik meg, és objektumok közötti távolság hello hello érték (vagy súly) használ, fenti adatok hello nézhet ki:

    ![különböző távolságát sornyi kör Megrajzolás tiny_graph.txt](./media/hdinsight-hadoop-giraph-install/giraph-graph.png)
2. Hello SimpleShortestPathsComputation példa futtatásához. Hello Azure PowerShell parancsmagok toorun hello példa következő hello tiny_graph.txt fájllal bemenetként használja.

    > [!IMPORTANT]
    > A HDInsight-erőforrások Azure Service Managerrel történő kezelésének Azure PowerShell-támogatása **elavult**, így 2017. január 1-től megszűnt. a dokumentum használatát hello új HDInsight-parancsmagokat, amelyek működnek az Azure Resource Manager hello szükséges lépések.
    >
    > Kövesse a lépéseket hello [telepítse és konfigurálja az Azure Powershellt](/powershell/azureps-cmdlets-docs) tooinstall hello legújabb Azure PowerShell verziója. Ha vannak olyan parancsprogramjai, hogy szükség toobe módosított toouse hello új parancsmagok használata Azure Resource Managerrel működő című [áttelepítése tooAzure fejlesztési Resource Manager-alapú HDInsight-fürtök eszközei](hdinsight-hadoop-development-using-azure-resource-manager.md) további információt.

    ```powershell
    $clusterName = "clustername"
    # Giraph examples jar
    $jarFile = "wasb:///example/jars/giraph-examples.jar"
    # Arguments for this job
    $jobArguments = "org.apache.giraph.examples.SimpleShortestPathsComputation",
                    "-ca", "mapred.job.tracker=headnodehost:9010",
                    "-vif", "org.apache.giraph.io.formats.JsonLongDoubleFloatDoubleVertexInputFormat",
                    "-vip", "wasb:///example/data/tiny_graph.txt",
                    "-vof", "org.apache.giraph.io.formats.IdWithValueTextOutputFormat",
                    "-op",  "wasb:///example/output/shortestpaths",
                    "-w", "2"
    # Create hello definition
    $jobDefinition = New-AzureHDInsightMapReduceJobDefinition
        -JarFile $jarFile
        -ClassName "org.apache.giraph.GiraphRunner"
        -Arguments $jobArguments

    # Run hello job, write output toohello Azure PowerShell window
    $job = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $jobDefinition
    Write-Host "Wait for hello job toocomplete ..." -ForegroundColor Green
    Wait-AzureHDInsightJob -Job $job
    Write-Host "STDERR"
    Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $job.JobId -StandardError
    Write-Host "Display hello standard output ..." -ForegroundColor Green
    Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $job.JobId -StandardOutput
    ```

    A fenti példa hello, cserélje le a **clustername** a HDInsight-fürt, amelyen telepítve Giraph hello nevű.
3. Hello eredményeinek megtekintése. Miután hello feladat befejeződött, hello eredményeket fogja tárolni hello két a kimeneti fájlok **wasb: / / / Példa/out/shotestpaths** mappa. hello fájlok nevezzük **rész-m-00001** és **rész-m-00002**. Hajtsa végre a következő lépéseket toodownload, és tekintse meg a hello kimeneti hello:

    ```powershell
    $subscriptionName = "<SubscriptionName>"       # Azure subscription name
    $storageAccountName = "<StorageAccountName>"   # Azure Storage account name
    $containerName = "<ContainerName>"             # Blob storage container name

    # Select hello current subscription
    Select-AzureSubscription $subscriptionName

    # Create hello Storage account context object
    $storageAccountKey = Get-AzureStorageKey $storageAccountName | %{ $_.Primary }
    $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

    # Download hello job output toohello workstation
    Get-AzureStorageBlobContent -Container $containerName -Blob example/output/shortestpaths/part-m-00001 -Context $storageContext -Force
    Get-AzureStorageBlobContent -Container $containerName -Blob example/output/shortestpaths/part-m-00002 -Context $storageContext -Force
    ```

    Ezzel létrehoz hello **példa/kimeneti/shortestpaths** könyvtárstruktúrát hello aktuális könyvtárhoz a munkaállomáson, a két hello kimeneti fájlok toothat céljából.

    Használjon hello **Cat** parancsmag toodisplay hello tartalmát hello fájlok:

        Cat example/output/shortestpaths/part*

    hello kimeneti hasonló toohello következő kell megjelennie:

        0    1.0
        4    5.0
        2    2.0
        1    0.0
        3    1.0

    hello SimpleShortestPathComputation példa nél nagyobb toostart objektum azonosítója 1 és hello legrövidebb elérési tooother objektumok keresése. Így hello kimeneti értelmezhető `destination_id distance`, ahol a távolság hello érték (vagy súly) amennyi hello szegélye között objektum azonosítója 1 és hello cél azonosítót.

    Ez megjelenítése, ellenőrizheti hello eredmények hello legrövidebb elérési utak utazás azonosítója 1 és egyéb objektumok között. Vegye figyelembe, hogy hello azonosítója 1 és 4 azonosító közötti legrövidebb elérési út pedig az 5. Ez a hello teljes közötti távolság szerint <span style="color:orange">azonosítója 1. és 3</span>, majd <span style="color:red">azonosító 3. és 4</span>.

    ![Az objektumok rajzolási körök mint a legrövidebb elérési utak között](./media/hdinsight-hadoop-giraph-install/giraph-graph-out.png)

## <a name="install-giraph-using-aure-powershell"></a>Giraph segítségével a következőkre PowerShell telepítése
Lásd: [testreszabása HDInsight-fürtök használata parancsfájlművelet](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).  hello minta bemutatja, hogyan tooinstall Spark on Azure PowerShell használatával. Toocustomize hello parancsfájl toouse kell [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1).

## <a name="install-giraph-using-net-sdk"></a>Telepítse a .NET SDK használatával Giraph
Lásd: [testreszabása HDInsight-fürtök használata parancsfájlművelet](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell). hello minta bemutatja, hogyan tooinstall Spark .NET SDK használatával hello. Toocustomize hello parancsfájl toouse kell [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1).

## <a name="see-also"></a>Lásd még:
* [Giraph telepítse a HDInsight Hadoop-fürtök (Linux)](hdinsight-hadoop-giraph-install-linux.md)
* [Hdinsight Hadoop-fürtök létrehozása](hdinsight-provision-clusters.md): általános információk a HDInsight-fürtök létrehozása.
* [Testre szabhatja a HDInsight-fürtjéhez parancsfájlművelet][hdinsight-cluster-customize]: parancsfájlművelet HDInsight-fürtök testreszabása általános tájékoztatást.
* [Parancsfájlművelet-parancsfájlok fejlesztése a HDInsight](hdinsight-hadoop-script-actions.md).
* [Telepítse, és válassza a Spark on HDInsight-fürtök][hdinsight-install-spark]: Spark telepítéséről parancsfájlművelet-mintát.
* [A HDInsight-fürtökön R telepítéséhez][hdinsight-install-r]: parancsfájlművelet minta R. telepítéséről
* [Solr telepíthető a HDInsight-fürtök](hdinsight-hadoop-solr-install.md): parancsfájlművelet minta Solr telepítésével kapcsolatban.

[tools]: https://github.com/Blackmist/hdinsight-tools
[aps]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/

[powershell-install]: /powershell/azureps-cmdlets-docs
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
