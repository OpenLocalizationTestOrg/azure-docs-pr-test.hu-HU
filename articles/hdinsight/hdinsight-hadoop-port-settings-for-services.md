---
title: "A HDInsight - Azure Hadoop-szolgáltatás által használt portok |} Microsoft Docs"
description: "HDInsight Hadoop szolgáltatás által használt portok listájáról."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: dd14aed9-ec25-4bb3-a20c-e29562735a7d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/23/2017
ms.author: larryfr
ms.openlocfilehash: 1ad536a53d64b0144f6396393830bc0c5cbe4fb1
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="ports-used-by-hadoop-services-on-hdinsight"></a>A HDInsight Hadoop-szolgáltatás által használt portok

Ez a dokumentum a Linux-alapú HDInsight-fürtökön futó Hadoop-szolgáltatás által használt portok listáját jeleníti meg. A fürt SSH használatával való kapcsolódáshoz használt portokon információkat is biztosít.

## <a name="public-ports-vs-non-public-ports"></a>Nyilvános port és a nem nyilvános portok

Linux-alapú HDInsight-fürtök csak teszi közzé a három portok nyilvánosan az interneten; 22, 23, és 443-as. Ezeket a portokat használatával biztonságosan férjenek hozzá a fürthöz az SSH és a biztonságos HTTPS protokollon keresztül elérhetővé tett szolgáltatások használatával.

Belsőleg, HDInsight megvalósítása által több Azure virtuális gépek (a fürt csomópontjainak) rendszert futtató Azure virtuális hálózatban. Az a virtuális hálózaton belül van-e hozzáférési portok nem lesz közzétéve az interneten keresztül. Például ha csatlakozni az SSH használatával átjárócsomópontokkal egyikét, az átjárócsomópont a majd közvetlenül hozzáférhet a fürt csomópontjain futó szolgáltatások.

> [!IMPORTANT]
> Ha nem ad meg egy Azure virtuális hálózatra konfigurációs beállításként hdinsight, egy automatikusan létrejön. Azonban más gépekkel (például egyéb Azure virtuális gépek vagy az ügyfélszámítógép fejlesztési) nem csatlakoztathatók a virtuális hálózat.


További gépeket csatlakoztatni a virtuális hálózat, először hozza létre a virtuális hálózaton, és adja meg azt a HDInsight-fürt létrehozásakor. További információkért lásd: [kiterjesztése HDInsight képességek az Azure virtuális hálózat használatával](hdinsight-extend-hadoop-virtual-network.md)

## <a name="public-ports"></a>Nyilvános portok

HDInsight-fürtök minden csomópontján egy Azure virtuális hálózatban találhatók, és nem tudnak közvetlenül hozzáférni az internetről. Nyilvános átjáró a következő portokat, amely az összes HDInsight-fürttípusok közösen használnak internet-hozzáférést biztosít.

| Szolgáltatás | Port | Protokoll | Leírás |
| --- | --- | --- | --- | --- |
| sshd |22 |SSH |Ügyfelek kapcsolódik az elsődleges headnode a sshd. További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md). |
| sshd |22 |SSH |A peremhálózati csomóponton sshd ügyfelek csatlakozik. További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md). |
| sshd |23 |SSH |A másodlagos headnode a sshd ügyfelek csatlakozik. További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md). |
| Ambari |443 |HTTPS |Ambari webes felhasználói felület. Lásd: [kezelése HDInsight az Ambari webes felhasználói felület használatával](hdinsight-hadoop-manage-ambari.md) |
| Ambari |443 |HTTPS |Ambari REST API-t. Lásd: [kezelése HDInsight az Ambari REST API használatával](hdinsight-hadoop-manage-ambari-rest-api.md) |
| WebHCat |443 |HTTPS |HCatalog REST API-t. Lásd: [a Hive használata a Curl](hdinsight-hadoop-use-pig-curl.md), [a Pig használata Curl](hdinsight-hadoop-use-pig-curl.md), [MapReduce használata Curl](hdinsight-hadoop-use-mapreduce-curl.md) |
| Hiveserver2-n |443 |ODBC |Hive ODBC használatával csatlakozik. Lásd: [az Excel csatlakozzon a HDInsight a Microsoft ODBC-illesztőprogram](hdinsight-connect-excel-hive-odbc-driver.md). |
| Hiveserver2-n |443 |JDBC |A Hive használatával JDBC csatlakozik. Lásd: [Hive hdinsight Hive JDBC-illesztőprogram használatával csatlakozhat](hdinsight-connect-hive-jdbc-driver.md) |

Az alábbiakban adott fürt esetében érhető el:

| Szolgáltatás | Port | Protokoll | Fürttípus | Leírás |
| --- | --- | --- | --- | --- |
| Stargate |443 |HTTPS |HBase |HBase REST API-t. Lásd: [HBase első lépései](hdinsight-hbase-tutorial-get-started-linux.md) |
| Livy |443 |HTTPS |Spark |Spark REST API-t. Lásd: [küldje el külső feladatok távolról a Livy használatával](hdinsight-apache-spark-livy-rest-interface.md) |
| Storm |443 |HTTPS |Storm |A Storm webes felhasználói felület. Lásd: [központi telepítése és kezelése a HDInsight alatt futó Storm-topológiák](hdinsight-storm-deploy-monitor-topology-linux.md) |

### <a name="authentication"></a>Authentication

Minden szolgáltatás nyilvánosan elérhetővé az interneten hitelesíteni kell:

| Port | Hitelesítő adatok |
| --- | --- |
| 22-es vagy 23 |A fürt létrehozásakor megadott SSH hitelesítő |
| 443 |A bejelentkezési név (alapértelmezett: admin) és a fürt létrehozása során beállított jelszót |

## <a name="non-public-ports"></a>Nem nyilvános portok

> [!NOTE]
> Egyes szolgáltatások adott fürt típusok csak érhetők el. A HBase például a HBase fürt típusok csak érhető el.

> [!IMPORTANT]
> Egyes szolgáltatások csak futtatható egy headnode egyszerre. Ha próbál csatlakozni a szolgáltatáshoz, az elsődleges headnode a és a 404-es hibaüzenetet kap, próbálja meg újból, a másodlagos headnode használatával.

### <a name="ambari"></a>Ambari

| Szolgáltatás | Csomópontok | Port | URL-címe | Protokoll | 
| --- | --- | --- | --- | --- |
| Ambari webes felhasználói felület | HEAD csomópontok | 8080 | / | HTTP |
| Ambari REST API-n | HEAD csomópontok | 8080 | / api/v1 | HTTP |

Példák:

* Ambari REST API:`curl -u admin "http://10.0.0.11:8080/api/v1/clusters"`

### <a name="hdfs-ports"></a>HDFS portok

| Szolgáltatás | Csomópontok | Port | Protokoll | Leírás |
| --- | --- | --- | --- | --- |
| NameNode webes felhasználói felület |HEAD csomópontok |30070 |HTTPS |Webes felhasználói felület állapotának megtekintése |
| NameNode metaadat-szolgáltatás |HEAD csomópontok |8020 |IPC |Fájl rendszer metaadatai |
| DataNode |Az összes munkavégző csomópontokhoz |30075 |HTTPS |Webes felhasználói felület állapot megtekintése, naplói, stb. |
| DataNode |Az összes munkavégző csomópontokhoz |30010 |&nbsp; |Adatátvitel |
| DataNode |Az összes munkavégző csomópontokhoz |30020 |IPC |Metaadat-művelet |
| Másodlagos NameNode |HEAD csomópontok |50090 |HTTP |Ellenőrzőpont NameNode metaadatok |

### <a name="yarn-ports"></a>YARN portok

| Szolgáltatás | Csomópontok | Port | Protokoll | Leírás |
| --- | --- | --- | --- | --- |
| Erőforrás-kezelő webes felhasználói felület |HEAD csomópontok |8088 |HTTP |Webes felhasználói felület az erőforrás-kezelő |
| Erőforrás-kezelő webes felhasználói felület |HEAD csomópontok |8090 |HTTPS |Webes felhasználói felület az erőforrás-kezelő |
| Erőforrás-kezelő felügyeleti felülete |HEAD csomópontok |8141 |IPC |Az alkalmazás jelentések (Hive, server Hive, Pig, stb.) |
| Erőforrás-kezelő Feladatütemező |HEAD csomópontok |8030 |HTTP |Rendszergazdai felület |
| Erőforrás-kezelő felületen |HEAD csomópontok |8050 |HTTP |Az alkalmazások manager felületén címe |
| NodeManager |Az összes munkavégző csomópontokhoz |30050 |&nbsp; |A tároló manager címe |
| NodeManager webes felhasználói felület |Az összes munkavégző csomópontokhoz |30060 |HTTP |Erőforrás-kezelő felület |
| Az idősor cím |HEAD csomópontok |10200 |RPC |Az ütemterv RPC szolgáltatás. |
| Az idősor webes felhasználói felület |HEAD csomópontok |8181 |HTTP |Az ütemterv szolgáltatás webes felhasználói felület |

### <a name="hive-ports"></a>Hive-portok

| Szolgáltatás | Csomópontok | Port | Protokoll | Leírás |
| --- | --- | --- | --- | --- |
| Hiveserver2-n |HEAD csomópontok |10001 |Thrift |Szolgáltatás struktúra (Thrift/JDBC) való kapcsolódáshoz |
| Hive Metaadattárhoz |HEAD csomópontok |9083 |Thrift |Hive-metaadatok (Thrift/JDBC) való kapcsolódáshoz szolgáltatás |

### <a name="webhcat-ports"></a>WebHCat-portok

| Szolgáltatás | Csomópontok | Port | Protokoll | Leírás |
| --- | --- | --- | --- | --- |
| WebHCat-kiszolgáló |HEAD csomópontok |30111 |HTTP |Webes API-k fölött HCatalog és egyéb Hadoop-szolgáltatás |

### <a name="mapreduce-ports"></a>MapReduce portok

| Szolgáltatás | Csomópontok | Port | Protokoll | Leírás |
| --- | --- | --- | --- | --- |
| JobHistory |HEAD csomópontok |19888 |HTTP |MapReduce JobHistory webes felhasználói felület |
| JobHistory |HEAD csomópontok |10020 |&nbsp; |MapReduce JobHistory kiszolgáló |
| ShuffleHandler |&nbsp; |13562 |&nbsp; |A kért szűkítő kimenete átvitelek köztes térkép |

### <a name="oozie"></a>Oozie

| Szolgáltatás | Csomópontok | Port | Protokoll | Leírás |
| --- | --- | --- | --- | --- |
| Oozie kiszolgáló |HEAD csomópontok |11000 |HTTP |Oozie szolgáltatás URL-címe |
| Oozie kiszolgáló |HEAD csomópontok |11001 |HTTP |A Oozie rendszergazdai port |

### <a name="ambari-metrics"></a>Ambari metrikák

| Szolgáltatás | Csomópontok | Port | Protokoll | Leírás |
| --- | --- | --- | --- | --- |
| Az idősor (alkalmazás Előzmények) |HEAD csomópontok |6188 |HTTP |Az ütemterv szolgáltatás webes felhasználói felület |
| Az idősor (alkalmazás Előzmények) |HEAD csomópontok |30200 |RPC |Az ütemterv szolgáltatás webes felhasználói felület |

### <a name="hbase-ports"></a>A HBase-portok

| Szolgáltatás | Csomópontok | Port | Protokoll | Leírás |
| --- | --- | --- | --- | --- |
| HMaster |HEAD csomópontok |16000 |&nbsp; |&nbsp; |
| Webes felhasználói felületén HMaster adatai |HEAD csomópontok |16010 |HTTP |A HBase fő webes felhasználói felület port |
| A régióban kiszolgáló |Az összes munkavégző csomópontokhoz |16020 |&nbsp; |&nbsp; |
| &nbsp; |&nbsp; |2181 |&nbsp; |A port, amelyet az ügyfelek csatlakozhatnak ZooKeeper |

### <a name="kafka-ports"></a>Kafka portok

| Szolgáltatás | Csomópontok | Port | Protokoll | Leírás |
| --- | --- | --- | --- | --- |
| Broker |Munkavégző csomópontokhoz |9092 |[Kafka protokoll](http://kafka.apache.org/protocol.html) |Az ügyfél-kommunikációhoz használt |
| &nbsp; |Zookeeper csomópontok |2181 |&nbsp; |A port, amelyet az ügyfelek csatlakozhatnak Zookeeper |

### <a name="spark-ports"></a>A Spark-portok

| Szolgáltatás | Csomópontok | Port | Protokoll | URL-címe | Leírás |
| --- | --- | --- | --- | --- | --- |
| A Spark Thrift-kiszolgálók |HEAD csomópontok |10002 |Thrift | &nbsp; | Szolgáltatás Spark SQL (Thrift/JDBC) való kapcsolódáshoz |
| Livy kiszolgáló | HEAD csomópontok | 8998 | HTTP | /batches | Szolgáltatás utasítások, feladatok és alkalmazások |

Példák:

* Livy: `curl "http://10.0.0.11:8998/batches"`. Ebben a példában `10.0.0.11` a headnode a Livy szolgáltatást üzemeltető IP-címét.