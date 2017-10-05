---
title: "A HDInsight - Azure interaktív struktúra |} Microsoft Docs"
description: "Megtudhatja, hogyan interaktív struktúra (Hive LLAP a) a hdinsight eszközben."
keywords: 
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 0957643c-4936-48a3-84a3-5dc83db4ab1a
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: e7874b55fc72f14d8e2c801872359e823cb2ba34
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-interactive-hive-in-hdinsight-preview"></a>Interaktív Hive használata a Hdinsightban (előzetes verzió)
Interaktív (más néven struktúra [Hosszú Live és a folyamat](https://cwiki.apache.org/confluence/display/Hive/LLAP)) az új HDInsight [típusú fürt](hdinsight-hadoop-provision-linux-clusters.md#cluster-types).  Interaktív Hive lehetővé teszi, hogy a memóriába való gyorsítótárazás növeli, amelynek eredményeképpen a Hive-lekérdezéseket, sokkal több interaktív és gyorsabb lett. Ezen új szolgáltatás révén HDInsight a világ legtöbb performant, rugalmas, és nyissa meg a Big Data-megoldások a felhő a memóriában lévő gyorsítótárát (a Hive és a Spark használatával) és advanced analytics átfogóan integrálja az R Services keresztül. 

Az interaktív Hive fürt eltér a Hadoop-fürt. Csak a Hive szolgáltatást tartalmaz. 

> [!NOTE]
> MapReduce, a Pig, a Sqoop, a Oozie és egyéb szolgáltatások törlődik a fürt típusa hamarosan.
> A Hive-szolgáltatás a interaktív Hive fürt csak az az Ambari Hive view Beeline és Hive ODBC keresztül érhető el. Nem érhető el Hive konzol, lépni a Templeton, az Azure parancssori felület és Azure PowerShell használatával. 
> 
> 

## <a name="create-an-interactive-hive-cluster"></a>Az interaktív Hive-fürt létrehozása
Interaktív Hive fürt csak Linux-alapú fürtökön támogatott. A HDInsight-fürtök létrehozásával kapcsolatos további információkért lásd: [hdinsight létrehozása Linux-alapú Hadoop-fürtök](hdinsight-hadoop-provision-linux-clusters.md).

## <a name="execute-hive-from-interactive-hive"></a>Az interaktív Hive Hive végrehajtás
Nincsenek a másik lehetőség, hogyan futtathat Hive-lekérdezéseket:

* Futtassa a Hive használata a Ambari Hive nézete
  
    A Hive nézet használatával kapcsolatos információkért lásd: [Hive nézet használata a hadooppal a Hdinsightban](hdinsight-hadoop-use-hive-ambari-view.md).
* Hive Beeline használatával futtassa
  
    Beeline a HDInsight használatáról információkért lásd: [használja a Beeline a HDInsight Hadoop Hive](hdinsight-hadoop-use-hive-beeline.md).
  
    A headnode vagy egy üres élcsomópontot Beeline használja.  Az egy üres élcsomópontot Beeline használata ajánlott.  Információk a HDInsight-fürtöt hoz létre egy üres edgenode: [üres peremhálózati csomópontok használata a Hdinsightban](hdinsight-apps-use-edge-node.md).
* Hive Hive ODBC használatával futtassa
  
    A Hive ODBC használatával kapcsolatos információkért lásd: [kapcsolódás Excel és a Microsoft Hive ODBC-illesztőprogram Hadoop](hdinsight-connect-excel-hive-odbc-driver.md).

**A JDBC-kapcsolati karakterlánc megkeresése:**

1. Jelentkezzen be a következő URL-cím segítségével Ambari: https://<ClusterName>. AzureHDInsight.net.
2. Kattintson a **Hive** a bal oldali menüből.
3. Kattintson a kijelölt ikonra másolja az URL-címet:
   
   ![HDInsight Hadoop Hive interaktív LLAP JDBC](./media/hdinsight-hadoop-use-interactive-hive/hdinsight-hadoop-use-interactive-hive-jdbc.png)

## <a name="see-also"></a>Lásd még:
* [Linux-alapú Hadoop-fürtök létrehozása a Hdinsightban](hdinsight-hadoop-provision-linux-clusters.md): útmutató hdinsight fürtök interaktív Hive létrehozásához.
* [A Hive használata a hadooppal a Hdinsightban az Beeline](hdinsight-hadoop-use-hive-beeline.md): Beeline elküldeni a Hive-lekérdezések használata.
* [Az Excel és a Hadoop csatlakoztatása a Microsoft Hive ODBC-illesztőprogram a](hdinsight-connect-excel-hive-odbc-driver.md): megtudhatja, hogyan csatlakoztathatja az Excelt struktúra.

