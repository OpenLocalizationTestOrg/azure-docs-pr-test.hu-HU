---
title: "a Hadoop-fürt - Azure aaaUse parancsfájlművelet tooinstall Solr |} Microsoft Docs"
description: "Ismerje meg, hogyan toocustomize HDInsight-fürtöt Solr parancsfájlművelet használatával."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: b1e6f338-8ac1-4b38-bbb5-2f7388b9de3b
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/05/2016
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: 022ba56b7499390a91bfe869e5069893e56b6503
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-use-solr-on-windows-based-hdinsight-clusters"></a>Telepítheti és használhatja a Windows-alapú HDInsight-fürtök Solr

Megtudhatja, hogyan toocustomize Windows-alapú HDInsight-fürtöt használó parancsfájlművelet Solr, és hogyan toouse Solr toosearch adatokat.

> [!IMPORTANT]
> Ez a dokumentum csak olyan feladaton végezhető Windows-alapú HDInsight-fürtökkel hello szükséges lépések. HDInsight csak érhető el a Windows korábbi, mint a HDInsight 3.4-es verziójához. Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement). Egy Linux-alapú fürttel Solr használatáról információkért lásd: [telepítése és használata Solr a HDinsight Hadoop-fürtök (Linux)](hdinsight-hadoop-solr-install-linux.md).


Telepíthető Solr bármilyen típusú on Azure HDInsight (Hadoop-, Storm, HBase, Spark) fürt segítségével *parancsfájlművelet*. Egy minta parancsfájlt tooinstall Solr egy HDInsight-fürt érhető el, csak olvasható az Azure storage-blobból [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).

hello mintaparancsfájl csak HDInsight-fürt verziószáma 3.1-es verziójával működik. A HDInsight-fürt verziókról további információkért lásd: [HDInsight-fürt verziókról](hdinsight-component-versioning.md).

a jelen témakörben használt hello mintaparancsfájl egy Windows-alapú Solr fürtöt hoz létre a adott konfigurációval. Ha azt szeretné, hogy tooconfigure hello Solr fürt különböző gyűjteményeket, szilánkok, sémákat, replikákat, stb., módosítania kell hello parancsfájl és Solr bináris ennek megfelelően.

**Kapcsolódó cikkek**

* [Telepítheti és használhatja Solr HDinsight Hadoop-fürtök (Linux)](hdinsight-hadoop-solr-install-linux.md)
* [Hdinsight Hadoop-fürtök létrehozása](hdinsight-provision-clusters.md): általános információk a HDInsight-fürtök létrehozása.
* [Testre szabhatja a HDInsight-fürtjéhez parancsfájlművelet][hdinsight-cluster-customize]: parancsfájlművelet HDInsight-fürtök testreszabása általános tájékoztatást.
* [Parancsfájlművelet-parancsfájlok fejlesztése a HDInsight](hdinsight-hadoop-script-actions.md).

## <a name="what-is-solr"></a>Mi az a Solr?
<a href="http://lucene.apache.org/solr/features.html" target="_blank">Apache Solr</a> egy vállalati keresési platform, amely lehetővé teszi az adatok hatékony teljes szöveges keresés. Hadoop lehetővé teszi a tárolásához és kezeléséhez az adatok óriási mennyiségben, Apache Solr hello keresési képességeket biztosít tooquickly lekérése hello adatokat.

## <a name="install-solr-using-portal"></a>Telepítse a portál használatával Solr
1. Indítsa el a fürt létrehozása hello segítségével **egyéni létrehozás** beállítást, a részben ismertetett módon [Hadoop létrehozása a HDInsight-fürtök](hdinsight-provision-clusters.md).
2. A hello **Parancsfájlműveletek** lap hello varázsló, kattintson a **parancsfájlművelet hozzáadása** tooprovide részleteinek hello parancsfájlművelet, alább látható módon:

    ![Használja a fürt parancsfájlművelet toocustomize](./media/hdinsight-hadoop-solr-install/hdi-script-action-solr.png "használata parancsfájlművelet toocustomize fürt")

    <table border='1'>
        <tr><th>Tulajdonság</th><th>Érték</th></tr>
        <tr><td>Név</td>
            <td>Adja meg a hello parancsfájlművelet nevét. Például <b>telepítése Solr</b>.</td></tr>
        <tr><td>A parancsfájl URI azonosítója</td>
            <td>Hello egységes erőforrás-azonosító (URI) toohello parancsfájl megadásához, amely meghívott toocustomize hello fürt. Például <i>https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1</i></td></tr>
        <tr><td>Csomóponttípus</td>
            <td>Adja meg, amelyen hello testreszabási parancsfájl futtatása hello csomópontok. Választhat <b>csomópontjaihoz</b>, <b>csak Átjárócsomópontokat</b>, vagy <b>csak a feldolgozó csomópontok</b>.
        <tr><td>Paraméterek</td>
            <td>Adja meg a hello paraméterek hello parancsfájl által szükség esetén. hello parancsfájl tooinstall Solr kell paramétert, így ez üresen hagyhatja.</td></tr>
    </table>

    Hello fürtön egynél több parancsprogram művelet tooinstall több összetevőből is hozzáadhat. Miután hozzáadta a hello parancsfájlok, kattintson a hello pipa toostart hello fürtöt hoz létre.

## <a name="use-solr"></a>A Solr használata
Az indexelő Solr bizonyos adatok fájlokkal kell elindítani. Solr toorun keresési lekérdezések használhatja, ha az indexelt hello adatokon. Hajtsa végre a következő lépéseket toouse Solr a HDInsight-fürtök hello:

1. **Remote Desktop Protocol (RDP) tooremote hello HDInsight-fürthöz történő használata telepített Solr**. Hello Azure-portálon, a távoli asztal engedélyezése Solr telepített, és be, majd távolról hello fürthöz létrehozott hello fürt számára. Útmutatásért lásd: [csatlakozás RDP tooHDInsight-fürtök](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).
2. **Fájlok feltöltésével index Solr**. Index Solr, amikor a rendszer, amely toosearch szükség lehet a put dokumentumok. tooindex Solr, használja az RDP tooremote hello fürtbe, keresse meg a toohello asztali, hello Hadoop parancssor megnyitásához, és keresse meg a túl**C:\apps\dist\solr-4.7.2\example\exampledocs**. Futtassa a következő parancs hello:

        java -jar post.jar solr.xml monitor.xml

    Hello kimeneti hello konzolban, a következő jelenik meg:

        POSTing file solr.xml
        POSTing file monitor.xml
        2 files indexed.
        COMMITting Solr index changes toohttp://localhost:8983/solr/update..
        Time spent: 0:00:01.624

    hello post.jar segédprogram Solr indexeli a két minta dokumentumok **solr.xml** és **monitor.xml**. hello post.jar segédprogram és hello minta dokumentumok Solr telepítés érhetők el.
3. **Használjon hello Solr irányítópult toosearch belül hello indexelt dokumentumok**. A hello RDP munkamenet toohello HDInsight fürt, nyissa meg az Internet Explorert, indítsa el a következő hello Solr irányítópult **http://headnodehost:8983/solr / #/**. Hello bal oldali ablaktáblán, a hello **Core választó** legördülő listából válassza **collection1**, majd belül, hogy a **lekérdezés**. Egy példa, tooselect és visszatérési összes hello dokumentumok a Solr, adja meg a következő értékek hello:

   * A hello **q** szöveget adja meg a  **\*:**\*. Ez visszaállítja az összes hello dokumentumok indexelt a Solr. Ha azt szeretné, toosearch hello dokumentumok belül egy adott karakterláncot, karakterláncokat Itt adhatja meg.
   * A hello **wt** szövegmezőben, jelölje be hello kimeneti formátum. Alapértelmezett érték a **json**. Kattintson a **lekérdezés végrehajtása**.

     ![Használja a fürt parancsfájlművelet toocustomize](./media/hdinsight-hadoop-solr-install/hdi-solr-dashboard-query.png "Solr irányítópult-lekérdezés futtatható")

     hello kimeneti hello az indexelési Solr használt két docs adja vissza. hello kimeneti hello következőhöz hasonló:

           "response": {
               "numFound": 2,
               "start": 0,
               "maxScore": 1,
               "docs": [
                 {
                   "id": "SOLR1000",
                   "name": "Solr, hello Enterprise Search Server",
                   "manu": "Apache Software Foundation",
                   "cat": [
                     "software",
                     "search"
                   ],
                   "features": [
                     "Advanced Full-Text Search Capabilities using Lucene",
                     "Optimized for High Volume Web Traffic",
                     "Standards Based Open Interfaces - XML and HTTP",
                     "Comprehensive HTML Administration Interfaces",
                     "Scalability - Efficient Replication tooother Solr Search Servers",
                     "Flexible and Adaptable with XML configuration and Schema",
                     "Good unicode support: héllo (hello with an accent over hello e)"
                   ],
                   "price": 0,
                   "price_c": "0,USD",
                   "popularity": 10,
                   "inStock": true,
                   "incubationdate_dt": "2006-01-17T00:00:00Z",
                   "_version_": 1486960636996878300
                 },
                 {
                   "id": "3007WFP",
                   "name": "Dell Widescreen UltraSharp 3007WFP",
                   "manu": "Dell, Inc.",
                   "manu_id_s": "dell",
                   "cat": [
                     "electronics and computer1"
                   ],
                   "features": [
                     "30\" TFT active matrix LCD, 2560 x 1600, .25mm dot pitch, 700:1 contrast"
                   ],
                   "includes": "USB cable",
                   "weight": 401.6,
                   "price": 2199,
                   "price_c": "2199,USD",
                   "popularity": 6,
                   "inStock": true,
                   "store": "43.17614,-90.57341",
                   "_version_": 1486960637584081000
                 }
               ]
             }
4. **Ajánlott: Biztonságimásolat-hello indexelt Solr tooAzure hello HDInsight-fürthöz társított Blob-tároló adatait**. Jó gyakorlat készítsen biztonsági másolatot indexelt hello adatok hello Solr fürt csomópontjának alakzatot Azure Blob Storage tárolóban. Hajtsa végre a következő lépéseket toodo így hello:

   1. Hello RDP-munkamenetet nyissa meg az Internet Explorer és a következő URL-cím pont toohello:

           http://localhost:8983/solr/replication?command=backup

       A következőhöz hasonló választ kell megjelennie:

           <?xml version="1.0" encoding="UTF-8"?>
           <response>
             <lst name="responseHeader">
               <int name="status">0</int>
               <int name="QTime">9</int>
             </lst>
             <str name="status">OK</str>
           </response>
   2. Hello távoli munkamenet, lépjen a túl {SOLR_HOME}\{gyűjtemény} \data. Hello fürthöz létrehozott hello minta parancsfájlt, amely legyen **C:\apps\dist\solr-4.7.2\example\solr\collection1\data**. Ezen a helyen kell megjelennie a pillanatkép mappája létrejön, melynek a neve hasonló túl**pillanatkép.* Timestamp típusú***.
   3. A ZIP-hello pillanatképmappáját, és töltse fel tooAzure Blob-tároló. Hello Hadoop parancssorban keresse meg a hello pillanatkép mappája helyének toohello hello a következő parancs használatával:

             hadoop fs -CopyFromLocal snapshot._timestamp_.zip /example/data

       Ez a parancs másolatok túl/példa/pillanatképadatok hello/hello tárolóban belül hello alapértelmezett tárolási hello-fürthöz tartozó fiókot.

## <a name="install-solr-using-aure-powershell"></a>Telepítse a következőkre PowerShell-lel Solr
Lásd: [testreszabása HDInsight-fürtök használata parancsfájlművelet](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).  hello minta bemutatja, hogyan tooinstall Spark on Azure PowerShell használatával. Toocustomize hello parancsfájl toouse kell [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).

## <a name="install-solr-using-net-sdk"></a>Telepítse a .NET SDK használatával Solr
Lásd: [testreszabása HDInsight-fürtök használata parancsfájlművelet](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell). hello minta bemutatja, hogyan tooinstall Spark .NET SDK használatával hello. Toocustomize hello parancsfájl toouse kell [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).

## <a name="see-also"></a>Lásd még:
* [Telepítheti és használhatja Solr HDinsight Hadoop-fürtök (Linux)](hdinsight-hadoop-solr-install-linux.md)
* [Hdinsight Hadoop-fürtök létrehozása](hdinsight-provision-clusters.md): általános információk a HDInsight-fürtök létrehozása.
* [Testre szabhatja a HDInsight-fürtjéhez parancsfájlművelet][hdinsight-cluster-customize]: parancsfájlművelet HDInsight-fürtök testreszabása általános tájékoztatást.
* [Parancsfájlművelet-parancsfájlok fejlesztése a HDInsight](hdinsight-hadoop-script-actions.md).
* [Telepítse, és válassza a Spark on HDInsight-fürtök][hdinsight-install-spark]: Spark telepítéséről parancsfájlművelet-mintát.
* [A HDInsight-fürtökön R telepítéséhez][hdinsight-install-r]: parancsfájlművelet minta R. telepítéséről
* [Giraph telepíthető a HDInsight-fürtök](hdinsight-hadoop-giraph-install.md): parancsfájlművelet minta Giraph telepítésével kapcsolatban.

[powershell-install-configure]: /powershell/azureps-cmdlets-docs
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
