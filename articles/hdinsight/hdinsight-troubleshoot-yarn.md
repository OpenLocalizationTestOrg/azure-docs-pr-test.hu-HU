---
title: "Azure HDInsight segítségével YARN aaaTroubleshoot |} Microsoft Docs"
description: "Válaszok az Apache Hadoop yarn rendszerre és az Azure HDInsight toocommon kérdésekre."
keywords: "Az Azure HDInsight, YARN, gyakori kérdések hibaelhárítási útmutatója, gyakori kérdések"
services: Azure HDInsight
documentationcenter: na
author: arijitt
manager: 
editor: 
ms.assetid: F76786A9-99AB-4B85-9B15-CA03528FC4CD
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/7/2017
ms.author: arijitt
ms.openlocfilehash: 800d9738cb27e05a64db470ee58565af3b85aa99
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-yarn-by-using-azure-hdinsight"></a>Hibaelhárítás YARN Azure HDInsight segítségével

Hello legfőbb problémákat és azok megoldásait ismerje meg az Apache Ambari az Apache Hadoop YARN Payload van jelen használatakor.

## <a name="how-do-i-create-a-new-yarn-queue-on-a-cluster"></a>Hogyan hozzon létre egy új YARN várólistát egy fürtön


### <a name="resolution-steps"></a>Megoldási lépések 

Használja hello alábbi új YARN várólista Ambari toocreate lépéseit, és majd egyenleg közötti összes hello várólisták hello kapacitásának elosztását. 

Ebben a példában két meglévő várólisták (**alapértelmezett** és **thriftsvr**) is megváltoznak kapacitásából 50 % kapacitás too25 %, ami hello új várólista (külső) 50 % kapacitást biztosít.
| Várólista | Kapacitás | Maximális kapacitás |
| --- | --- | --- | --- |
| Alapértelmezett | 25% | 50% |
| thrftsvr | 25% | 50% |
| Spark | 50% | 50% |

1. Jelölje be hello **Ambari nézetek** ikonra, és jelölje ki hello rács mintát. Válassza ki, **YARN várólista-kezelő**.

    ![Válassza ki a hello Ambari nézetek ikon](media/hdinsight-troubleshoot-yarn/create-queue-1.png)
2. Jelölje be hello **alapértelmezett** várólista.

    ![Válassza ki a hello alapértelmezett várólista](media/hdinsight-troubleshoot-yarn/create-queue-2.png)
3. A hello **alapértelmezett** várólista, módosítsa a hello **kapacitás** 50 % too25 %. A hello **thriftsvr** várólista, módosítsa a hello **kapacitás** too25 %.

    ![Hello kapacitás too25 % hello alapértelmezett és thriftsvr várólisták módosítása](media/hdinsight-troubleshoot-yarn/create-queue-3.png)
4. Válasszon egy új sor toocreate **hozzáadása várólista**.

    ![Válassza ki a várólista hozzáadása](media/hdinsight-troubleshoot-yarn/create-queue-4.png)

5. Hello új várólista neve.

    ![Spark hello várólista neve](media/hdinsight-troubleshoot-yarn/create-queue-5.png)  

6. Hagyja hello **kapacitás** érték 50 %-át, és jelölje ki hello **műveletek** gombra.

    ![Válassza ki a hello műveletek gomb](media/hdinsight-troubleshoot-yarn/create-queue-6.png)  
7. Válassza ki **mentse és frissítse a várólisták**.

    ![Válassza ki, mentse, és a frissítést](media/hdinsight-troubleshoot-yarn/create-queue-7.png)  

A módosítások azonnal a YARN Feladatütemező felhasználói felület hello láthatók.

### <a name="additional-reading"></a>További olvasnivaló

- [YARN CapacityScheduler](https://hadoop.apache.org/docs/r2.7.2/hadoop-yarn/hadoop-yarn-site/CapacityScheduler.html)


## <a name="how-do-i-download-yarn-logs-from-a-cluster"></a>Hogyan le a YARN naplóit fürtből


### <a name="resolution-steps"></a>Megoldási lépések 

1. Toohello HDInsight-fürtjéhez Secure Shell (SSH) ügyfél csatlakozhat. További információkért lásd: [További olvasnivaló](#additional-reading-2).

2. toolist összes hello Alkalmazásazonosítók hello YARN alkalmazások futnak, futtassa a következő parancs hello:

    ```apache
    yarn top
    ```
    hello azonosítók a hello **APPLICATIONID** oszlop. Naplók letölthető hello **APPLICATIONID** oszlop.

    ```apache
    YARN top - 18:00:07, up 19d, 0:14, 0 active users, queue(s): root
    NodeManager(s): 4 total, 4 active, 0 unhealthy, 0 decommissioned, 0 lost, 0 rebooted
    Queue(s) Applications: 2 running, 10 submitted, 0 pending, 8 completed, 0 killed, 0 failed
    Queue(s) Mem(GB): 97 available, 3 allocated, 0 pending, 0 reserved
    Queue(s) VCores: 58 available, 2 allocated, 0 pending, 0 reserved
    Queue(s) Containers: 2 allocated, 0 pending, 0 reserved

                      APPLICATIONID USER             TYPE      QUEUE   #CONT  #RCONT  VCORES RVCORES     MEM    RMEM  VCORESECS    MEMSECS %PROGR       TIME NAME
     application_1490377567345_0007 hive            spark  thriftsvr       1       0       1       0      1G      0G    1628407    2442611  10.00   18:20:20 Thrift JDBC/ODBC Server
     application_1490377567345_0006 hive            spark  thriftsvr       1       0       1       0      1G      0G    1628430    2442645  10.00   18:20:20 Thrift JDBC/ODBC Server
    ```

3. toodownload YARN tároló naplók összes alkalmazás főkiszolgálók, a következő parancs hello használata:
   
    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am ALL > amlogs.txt
    ```

    Ez a parancs egy amlogs.txt nevű naplófájlt hoz létre. 

4. toodownload YARN tároló naplók csak hello legfrissebb alkalmazás master, a következő parancs hello használata:

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am -1 > latestamlogs.txt
    ```

    Ez a parancs egy latestamlogs.txt nevű naplófájlt hoz létre. 

4. toodownload YARN tároló naplók hello első két alkalmazás főkiszolgálók, a következő parancs hello használata:

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am 1,2 > first2amlogs.txt 
    ```

    Ez a parancs egy first2amlogs.txt nevű naplófájlt hoz létre. 

5. toodownload tároló összes YARN naplóit, használjon hello a következő parancsot:

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> > logs.txt
    ```

    Ez a parancs egy logs.txt nevű naplófájlt hoz létre. 

6. toodownload hello YARN tároló napló a egy adott tárolóhoz, a következő parancs használata hello:

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -containerId <container_id> > containerlogs.txt 
    ```

    Ez a parancs egy containerlogs.txt nevű naplófájlt hoz létre.

### <a name="additional-reading-2"></a>További olvasnivaló

- [Kapcsolódás az SSH használatával tooHDInsight (Hadoop)](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix)
- [Apache Hadoop YARN fogalmakat és alkalmazások](https://hortonworks.com/blog/apache-hadoop-yarn-concepts-and-applications/)







