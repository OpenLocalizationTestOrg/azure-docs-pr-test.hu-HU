---
title: "R aaaUse toocustomize HDInsight - fürtök Azure |} Microsoft Docs"
description: "Megtudhatja, hogyan tooinstall R használata parancsfájl-művelet és a HDInsight-fürtök R történő használatra."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: be851270-afa5-4af0-a69e-2d343a4deeb7
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: bf5adf2e18dc43a743b29fd1567fad731b9c3ab7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-use-r-on-hdinsight-hadoop-clusters"></a>Az R környezet telepítése és használata HDInsight-beli Hadoop-fürtökön

Ismerje meg, hogyan toocustomize Windows-alapú HDInsight-fürt az R parancsfájl műveletével, és hogyan toouse R a HDInsight-fürtök. Hello [HDInsight ajánlat](https://azure.microsoft.com/pricing/details/hdinsight/) tartalmaz R Server a HDInsight-fürt részeként. Ez lehetővé teszi a R parancsfájlok toouse MapReduce és Spark toorun elosztott számítások. További információk: [Get started using R Server on HDInsight](hdinsight-hadoop-r-server-get-started.md) (R Server on HDInsight – első lépések). Az R használatával egy Linux-alapú fürttel információkért lásd: [telepítése és R használata a HDinsight Hadoop-fürtök (Linux)](hdinsight-hadoop-r-scripts-linux.md).

Telepíthető R bármilyen típusú on Azure HDInsight (Hadoop-, Storm, HBase, Spark) fürt segítségével *parancsfájlművelet*. Egy minta parancsfájlt tooinstall R egy HDInsight-fürt érhető el, csak olvasható az Azure storage-blobból [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1).

**Kapcsolódó cikkek**

* [Telepítheti és használhatja R HDinsight Hadoop-fürtök (Linux)](hdinsight-hadoop-r-scripts-linux.md)
* [Hdinsight Hadoop-fürtök létrehozása](hdinsight-hadoop-provision-linux-clusters.md): általános információk a HDInsight-fürtök létrehozása
* [Testre szabhatja a HDInsight-fürtjéhez parancsfájlművelet][hdinsight-cluster-customize]: általános információk a HDInsight-parancsfájlművelet fürtök testreszabása
* [A HDInsight parancsfájlművelet-parancsfájlok fejlesztése](hdinsight-hadoop-script-actions.md)

## <a name="what-is-r"></a>Mi az az R?
Hello <a href="http://www.r-project.org/" target="_blank">R-projektjét, amely statisztikai számításokhoz</a> egy megnyitott adatforrás nyelvi és statisztikai számításokhoz környezet. R biztosít több száz beépített statisztikai függvények és a saját programozási nyelv, amely egyesíti a működési és objektumorientált programozási aspektusait. Nagy mennyiségű grafikus képességeket is tartalmaz. R hello előnyben részesített programozási környezetet legtöbb szakmai statisztikusok és a mezők számos különböző kutatók.

R rendszer kompatibilis Azure Blob Storage (WASB), így az ott tárolt adatok R használata a HDInsight dolgozhatók.  

## <a name="install-r"></a>R telepítéséhez
A [mintaparancsfájl](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1) tooinstall R a HDInsight-fürtök az egy csak olvasható az Azure Storage-blobból érhető el. Ez a szakasz bemutatja, hogyan toouse hello hello Azure portál használatával hello fürt létrehozása során minta parancsfájlt.

> [!NOTE]
> HDInsight-fürt verziószáma 3.1 hello mintaparancsfájl jelent. A HDInsight fürt verzióival kapcsolatos további információkért lásd: [HDInsight-fürt verziókról](hdinsight-component-versioning.md).
>
>

1. Amikor HDInsight-fürtöt hoz létre hello portál, kattintson a **opcionális konfigurációs**, és kattintson a **Parancsfájlműveletek**.
2. A hello **Parancsfájlműveletek** lapján adja meg a következő értékek hello:

    ![Használja a fürt parancsfájlművelet toocustomize](./media/hdinsight-hadoop-r-scripts/hdi-r-script-action.png "használata parancsfájlművelet toocustomize fürt")

    <table border='1'>
        <tr><th>Tulajdonság</th><th>Érték</th></tr>
        <tr><td>Név</td>
            <td>Nevezze el hello parancsfájlművelet, például <b>R telepítéséhez</b>.</td></tr>
        <tr><td>A parancsfájl URI azonosítója</td>
            <td>Adja meg a hello URI toohello parancsfájl, amely meghívott toocustomize hello fürt, például <i>https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1</i></td></tr>
        <tr><td>Csomóponttípus</td>
            <td>Adja meg, amelyen hello testreszabási parancsfájl futtatása hello csomópontok. Választhat <b>csomópontjaihoz</b>, <b>csak Átjárócsomópontokat</b>, vagy <b>munkavégző csomópontokhoz</b> csak.
        <tr><td>Paraméterek</td>
            <td>Adja meg a hello paraméterek hello parancsfájl által szükség esetén. Hello parancsfájl tooinstall R azonban nem szükséges paramétereket, ezért üresen hagyhatja, ez.</td></tr>
    </table>

    Hello fürtön egynél több parancsprogram művelet tooinstall több összetevőből is hozzáadhat. Miután hozzáadta a hello parancsfájlok, kattintson a hello pipa toostart crating hello fürt.

A HDInsight is használhatja hello parancsfájl tooinstall R az Azure PowerShell vagy a hello HDInsight .NET SDK használatával. Ezek az eljárások az utasításokat a cikk későbbi részében.

## <a name="run-r-scripts"></a>R parancsfájlok futtatása
Ez a szakasz ismerteti, hogyan hello Hadoop toorun egy R parancsfájlt a HDInsight fürt.

1. **A távoli asztali kapcsolat toohello fürt létrehozására**: hello portálon, a távoli asztal engedélyezése a telepített r létrehozott hello fürthöz, és csatlakoztassa a toohello fürt. Útmutatásért lásd: [csatlakozás RDP tooHDInsight-fürtök](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).
2. **Nyissa meg hello R konzol**: hello R-telepítés helyezi a hivatkozás toohello R konzol hello központi csomópont hello asztalon. Kattintson rá tooopen hello R konzol.
3. **Hello R-parancsfájl futtatása**: hello R parancsfájlt közvetlenül hello R konzolról illeszti, ha kiválasztja, és nyomja le az ENTER BILLENTYŰT. Ez egy egyszerű példa parancsfájlt, amely hello számok 1 too100 állít elő, majd 2 szorzatát.

        library(rmr2)
        library(rhdfs)
        ints = to.dfs(1:100)
        calc = mapreduce(input = ints, map = function(k, v) cbind(v, 2*v))
        from.dfs(calc)

hello első két sorok hívás hello RHadoop szalagtárak telepített R. hello végső sor megrendelése hello eredmények toohello konzol. hello kimeneti kell kinéznie:

    [1,]  1 2
    [2,]  2 4
    .
    .
    .
    [98,]  98 196
    [99,]  99 198
    [100,] 100 200


## <a name="install-r-using-aure-powershell"></a>Segítségével a következőkre PowerShell R telepítéséhez
Lásd: [testreszabása HDInsight-fürtök használata parancsfájlművelet](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).  hello minta bemutatja, hogyan tooinstall Spark on Azure PowerShell használatával. Toocustomize hello parancsfájl toouse kell [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1).

## <a name="install-r-using-net-sdk"></a>.NET SDK használatával R telepítéséhez
Lásd: [testreszabása HDInsight-fürtök használata parancsfájlművelet](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell). hello minta bemutatja, hogyan tooinstall Spark .NET SDK használatával hello. Toocustomize hello parancsfájl toouse kell [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps11).

## <a name="see-also"></a>Lásd még:
* [Telepítheti és használhatja R HDinsight Hadoop-fürtök (Linux)](hdinsight-hadoop-r-scripts-linux.md)
* [Hdinsight Hadoop-fürtök létrehozása](hdinsight-hadoop-provision-linux-clusters.md): általános információk a HDInsight-fürtök létrehozása
* [Testre szabhatja a HDInsight-fürtjéhez parancsfájlművelet][hdinsight-cluster-customize]: általános információk a HDInsight-parancsfájlművelet fürtök testreszabása
* [A HDInsight parancsfájlművelet-parancsfájlok fejlesztése](hdinsight-hadoop-script-actions.md)
* [Telepítse, és válassza a Spark on HDInsight-fürtök][hdinsight-install-spark]: Spark telepítéséről parancsfájlművelet-minta
* [Giraph telepíthető a HDInsight-fürtök](hdinsight-hadoop-giraph-install.md): parancsfájlművelet minta Giraph telepítéséről
* [Solr telepíthető a HDInsight-fürtök](hdinsight-hadoop-solr-install-linux.md): parancsfájlművelet minta Solr telepítésével kapcsolatban.

[powershell-install-configure]: /powershell/azureps-cmdlets-docs
[hdinsight-provision]: ../hdinsight-provision-clusters/
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
[hdinsight-install-spark]: hdinsight-apache-spark-jupyter-spark-sql.md
