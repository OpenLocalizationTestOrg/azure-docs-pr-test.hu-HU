---
title: "storage-fiókok biztonságos átvitele az Azure HDInsight-fürtöt aaaCreate Hadoop |} Microsoft Docs"
description: "Ismerje meg, hogyan engedélyezve van a HDInsight-fürtök toocreate biztonságos átvitele az Azure storage-fiókok."
keywords: "hadoop első lépései,hadoop linux,hadoop gyorsútmutató,biztonságos átvitel,azure-tárfiók"
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/21/2017
ms.author: jgao
ms.openlocfilehash: 0acb8814ad0d5d5b5652d930b2e3da90f9d7978d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-hadoop-cluster-with-secure-transfer-storage-accounts-in-azure-hdinsight"></a>Biztonságos átvitelű tárfiókokkal rendelkező Hadoop-fürt létrehozása az Azure HDInsightban

Hello [szükséges átviteli biztonságos](../storage/common/storage-require-secure-transfer.md) a szolgáltatás továbbfejleszti hello biztonsági az Azure Storage-fiókjának tartat be az összes kérelem tooyour fiók biztonságos kapcsolaton keresztül. Ez a szolgáltatás és hello wasbs séma csak támogatja HDInsight-fürt verziószáma 3,6 vagy újabb. 

## <a name="prerequisites"></a>Előfeltételek
Az oktatóanyag elindításának feltétele:

* **Azure-előfizetés**: egy ingyenes egy hónapos próbafiók, toocreate Tallózás túl[azure.microsoft.com/free](https://azure.microsoft.com/free).
* **Engedélyezett biztonságos átvitellel rendelkező Azure-tárfiók** Hello útmutatásért lásd: [hozzon létre egy tárfiókot](../storage/common/storage-create-storage-account.md#create-a-storage-account) és [biztonságos átvitelét igénylő](../storage/common/storage-require-secure-transfer.md).
* **A Blob-tároló hello tárfiók**. 
## <a name="create-cluster"></a>Fürt létrehozása

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


Ebben a szakaszban egy Hadoop-fürtöt hozhat létre a HDInsightban egy [Azure Resource Manager-sablonnal](../azure-resource-manager/resource-group-template-deploy.md). hello sablon található [Github](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-with-existing-default-storage-account/). Nem kell a Resource Manager-sablonok használatára vonatkozó tapasztalattal rendelkeznie az oktatóanyag követéséhez. Egyéb Fürtlétrehozási módszerekhez és ebben az oktatóanyagban használt hello tulajdonságok ismertetése, tanulmányozza a [HDInsight-fürtök létrehozása](hdinsight-hadoop-provision-linux-clusters.md).

1. Kattintson a következő kép toosign tooAzure a és a nyitott hello Resource Manager sablon hello Azure-portálon hello. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-with-existing-default-storage-account%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hadoop-linux-tutorial-get-started/deploy-to-azure.png" alt="Deploy tooAzure"></a>

2. Hello utasításokat toocreate hello fürt kövesse a hello előírásainak megfelelően: 

    - Adja meg a HDInsight 3.6-os verzióját.  hello alapértelmezett verziója 3.5-ös verzióját. A 3.6-os vagy újabb verzió szükséges.
    - Adjon meg egy biztonságos átvitel használatára képes tárfiókot.
    - Hello tárfiók rövid nevét használja.
    - Hello tárfiók és a blob-tároló hello előre kell létrehozni. 

    Hello útmutatásért lásd: [fürt létrehozása](./hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster). 

Parancsfájl művelet tooprovide saját konfigurációs fájlok használatára, ha a következő beállítások hello wasbs kell használnia:

- fs.defaultFS (core-site)
- spark.eventLog.dir 
- spark.history.fs.logDirectory

## <a name="add-additional-storage-accounts"></a>További tárfiókok hozzáadása

Van több beállítások tooadd engedélyezni biztonságos letöltési storage-fiókok:

- Hello utolsó szakaszában hello Azure Resource Manager sablon módosításához.
- Hozzon létre egy fürtöt hello [Azure-portálon](https://portal.azure.com) , és adja meg a kapcsolt tárfiókra.
- Engedélyezve használata parancsfájl művelet tooadd további biztonságos átviteli tárolási fiókok tooan meglévő HDInsight-fürtre.  További információkért lásd: [adja hozzá a további tárhely fiókok tooHDInsight](hdinsight-hadoop-add-storage.md).

## <a name="next-steps"></a>Következő lépések
Ebben az oktatóprogramban megtanulhatta, hogyan toocreate HDInsight-fürtöt, és biztonságos engedélyezése átvitele toohello storage-fiókok.

További információk a HDInsight az adatok elemzése toolearn lásd: a következő cikkek hello:

* További információk a hdinsight eszközzel, hogyan tooperform Hive lekérdezi a Visual Studio eszközből, beleértve a Hive eszközzel toolearn lásd [használata a HDInsight Hive][hdinsight-use-hive].
* Pig kapcsolatos toolearn, nyelv használt tootransform adatokat, lásd: [a Pig használata a hdinsight eszközzel][hdinsight-use-pig].
* toolearn MapReduce, egy módszer toowrite, Hadoopon adatokat feldolgozó programok kapcsolatban lásd: [és a HDInsight együttes használata MapReduce][hdinsight-use-mapreduce].
* toolearn hello HDInsight Tools for Visual Studio tooanalyze adatok, használatával kapcsolatban lásd: [első lépések a Visual Studio Hadoop tools for HDInsight használatával](hdinsight-hadoop-visual-studio-tools-get-started.md).

További információk a HDInsight adattárolási módszereiről toolearn vagy hogyan tooget adatok HDInsight, tekintse meg a következő cikkek hello:

* További információt az Azure Storage HDInsight általi használatáról [az Azure Storage és a HDInsight együttes használatát](hdinsight-hadoop-use-blob-storage.md) ismertető cikkben talál.
* Információ tooupload adatok tooHDInsight, lásd: [töltse fel az adatok tooHDInsight][hdinsight-upload-data].

toolearn bővebben létrehozása vagy kezelése a HDInsight-fürtöt, tekintse meg a következő cikkek hello:

* a Linux-alapú HDInsight-fürt kezeléséhez toolearn lásd: [kezelése HDInsight-fürtök Ambari használatával](hdinsight-hadoop-manage-ambari.md).
* toolearn kiválasztható egy HDInsight-fürt létrehozásakor hello beállításokról bővebben lásd: [létrehozása HDInsight Linux egyéni beállításokkal](hdinsight-hadoop-provision-linux-clusters.md).
* Ha ismeri a Linux és a Hadoop, de szeretné, hogy a hello HDInsight Hadoop kapcsolatos tooknow rögzítésen, lásd: [a HDInsight használata Linux](hdinsight-hadoop-linux-information.md). Ez a cikk többek között az alábbi információkat tartalmazza:
  
  * Például az Ambari és a WebHCat hello fürtön tárolt szolgáltatások URL-címek
  * a Hadoop-fájlok és példák a helyi fájlrendszerben hello hello helye
  * hello alapértelmezett adattár hello használata az Azure Storage (WASB) HDFS helyett.

[1]: ../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md

[hdinsight-provision]: hdinsight-provision-linux-clusters.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md


