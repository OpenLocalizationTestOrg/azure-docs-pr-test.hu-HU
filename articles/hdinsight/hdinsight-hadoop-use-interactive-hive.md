---
title: "Interaktív Hive hdinsight - Azure aaaUse |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse interaktív struktúra (Hive LLAP a) a hdinsight eszközben."
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
ms.openlocfilehash: 9e751a08091d18bc1b3d070468feef87f6828c61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-interactive-hive-in-hdinsight-preview"></a>Interaktív Hive használata a Hdinsightban (előzetes verzió)
Interaktív (más néven struktúra [Hosszú Live és a folyamat](https://cwiki.apache.org/confluence/display/Hive/LLAP)) az új HDInsight [típusú fürt](hdinsight-hadoop-provision-linux-clusters.md#cluster-types).  Interaktív Hive lehetővé teszi, hogy a memóriába való gyorsítótárazás növeli, amelynek eredményeképpen a Hive-lekérdezéseket, sokkal több interaktív és gyorsabb lett. Ezen új szolgáltatás révén hello egyik HDInsight világ legtöbb performant, rugalmas, és nyissa meg a Big Data-megoldások hello felhő a memóriában lévő gyorsítótárát (a Hive és a Spark használatával) és advanced analytics átfogóan integrálja az R Services keresztül. 

hello interaktív Hive fürt Hadoop-fürt hello eltér. Csak hello struktúra szolgáltatást tartalmaz. 

> [!NOTE]
> MapReduce, a Pig, a Sqoop, a Oozie és egyéb szolgáltatások törlődik a fürt típusa hamarosan.
> hello Hive szolgáltatás hello interaktív Hive fürt csak az Ambari Hive nézete hello Beeline és Hive ODBC keresztül érhető el. Nem érhető el Hive konzol, lépni a Templeton, az Azure parancssori felület és Azure PowerShell használatával. 
> 
> 

## <a name="create-an-interactive-hive-cluster"></a>Az interaktív Hive-fürt létrehozása
Interaktív Hive fürt csak Linux-alapú fürtökön támogatott. A HDInsight-fürtök létrehozásával kapcsolatos további információkért lásd: [hdinsight létrehozása Linux-alapú Hadoop-fürtök](hdinsight-hadoop-provision-linux-clusters.md).

## <a name="execute-hive-from-interactive-hive"></a>Az interaktív Hive Hive végrehajtás
Nincsenek a másik lehetőség, hogyan futtathat Hive-lekérdezéseket:

* Futtassa a Hive hello Ambari Hive nézete segítségével
  
    Hello hello Hive nézet használatával kapcsolatos információkért lásd: [használata hello a HDInsight Hadoop Hive nézet](hdinsight-hadoop-use-hive-ambari-view.md).
* Hive Beeline használatával futtassa
  
    Hello Beeline a HDInsight használatáról információkért lásd: [használja a Beeline a HDInsight Hadoop Hive](hdinsight-hadoop-use-hive-beeline.md).
  
    Hello headnode vagy egy üres élcsomópontot Beeline használja.  Az egy üres élcsomópontot Beeline használata ajánlott.  Információk a HDInsight-fürtöt hoz létre egy üres edgenode: [üres peremhálózati csomópontok használata a Hdinsightban](hdinsight-apps-use-edge-node.md).
* Hive Hive ODBC használatával futtassa
  
    Hello Hive ODBC használatával kapcsolatos információkért lásd: [kapcsolódás Excel tooHadoop hello Microsoft Hive ODBC-illesztővel rendelkező](hdinsight-connect-excel-hive-odbc-driver.md).

**toofind hello JDBC kapcsolati karakterlánc:**

1. Használja a következő URL-cím hello tooAmbari bejelentkezés: https://<ClusterName>. AzureHDInsight.net.
2. Kattintson a **Hive** hello bal oldali menüből.
3. Kattintson a kijelölt hello ikon toocopy hello URL-címe:
   
   ![HDInsight Hadoop Hive interaktív LLAP JDBC](./media/hdinsight-hadoop-use-interactive-hive/hdinsight-hadoop-use-interactive-hive-jdbc.png)

## <a name="see-also"></a>Lásd még:
* [Linux-alapú Hadoop-fürtök létrehozása a Hdinsightban](hdinsight-hadoop-provision-linux-clusters.md): megtudhatja, hogyan toocreate interaktív Hive hdinsight clusters.
* [A Hive használata a hadooppal a Hdinsightban az Beeline](hdinsight-hadoop-use-hive-beeline.md): megtudhatja, hogyan toouse Beeline toosubmit Hive-lekérdezéseket.
* [Csatlakozás Excel tooHadoop hello Microsoft Hive ODBC-illesztőprogram](hdinsight-connect-excel-hive-odbc-driver.md): megtudhatja, hogyan tooconnect Excel tooHive.

