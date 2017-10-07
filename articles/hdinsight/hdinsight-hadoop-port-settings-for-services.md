---
title: "a HDInsight - Azure Hadoop-szolgáltatás által használt aaaPorts |} Microsoft Docs"
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
ms.openlocfilehash: 0abc5c1c678aa79816e3e82a74538d2fb6db40ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="ports-used-by-hadoop-services-on-hdinsight"></a>A HDInsight Hadoop-szolgáltatás által használt portok

Ez a dokumentum egy Linux-alapú HDInsight-fürtökön futó Hadoop-szolgáltatás által használt hello portok listáját tartalmazza. Emellett biztosítja a portokat az információknak az SSH használatával tooconnect toohello fürt.

## <a name="public-ports-vs-non-public-ports"></a>Nyilvános port és a nem nyilvános portok

Linux-alapú HDInsight fürtök csak teszi közzé három portok nyilvánosan a hello internet; 22, 23, és 443-as. Ezeket a portokat fürtöket használt toosecurely hozzáférés hello az SSH és hello biztonságos HTTPS protokollon keresztül elérhetővé tett szolgáltatások használatával.

Belsőleg, HDInsight megvalósítása által több Azure virtuális gépek (hello fürt csomópontja hello) rendszert futtató Azure virtuális hálózatban. A hello virtuális hálózaton belül van-e hozzáférési portok hello keresztül nem lesz közzétéve internet. Például ha az SSH használatával hello átjárócsomópontokkal tooone, hello központi csomópontról majd közvetlenül hozzáférhet hello fürtcsomóponton futó szolgáltatásokat.

> [!IMPORTANT]
> Ha nem ad meg egy Azure virtuális hálózatra konfigurációs beállításként hdinsight, egy automatikusan létrejön. Azonban nem csatlakozik más (például egyéb Azure virtuális gépek vagy az ügyfélszámítógép fejlesztési) gépek toothis virtuális hálózaton.


toojoin további gépeket toohello virtuális hálózat, kell először hozzon létre hello virtuális hálózatot, és adja meg azt a HDInsight-fürt létrehozásakor. További információkért lásd: [kiterjesztése HDInsight képességek az Azure virtuális hálózat használatával](hdinsight-extend-hadoop-virtual-network.md)

## <a name="public-ports"></a>Nyilvános portok

HDInsight-fürtök csomópontjai egy Azure virtuális hálózatban találhatók, és nem közvetlenül érhető el az összes hello internet hello. Egy nyilvános átjáró-porttal rendelkezik, amely az összes HDInsight-fürttípusok közösen használnak a következő internet-hozzáférés toohello biztosít.

| Szolgáltatás | Port | Protokoll | Leírás |
| --- | --- | --- | --- | --- |
| sshd |22 |SSH |Ügyfelek toosshd hello elsődleges headnode a kapcsolatot. További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md). |
| sshd |22 |SSH |Ügyfelek toosshd hello peremhálózati csomóponton csatlakozik. További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md). |
| sshd |23 |SSH |A másodlagos headnode hello ügyfelek toosshd csatlakozik. További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md). |
| Ambari |443 |HTTPS |Ambari webes felhasználói felület. Lásd: [kezelése Hdinsightban az Ambari webes felhasználói felületén hello](hdinsight-hadoop-manage-ambari.md) |
| Ambari |443 |HTTPS |Ambari REST API-t. Lásd: [kezelése Hdinsightban az Ambari REST API hello](hdinsight-hadoop-manage-ambari-rest-api.md) |
| WebHCat |443 |HTTPS |HCatalog REST API-t. Lásd: [a Hive használata a Curl](hdinsight-hadoop-use-pig-curl.md), [a Pig használata Curl](hdinsight-hadoop-use-pig-curl.md), [MapReduce használata Curl](hdinsight-hadoop-use-mapreduce-curl.md) |
| Hiveserver2-n |443 |ODBC |ODBC használatával tooHive csatlakozik. Lásd: [kapcsolódás Excel tooHDInsight hello Microsoft ODBC-illesztővel](hdinsight-connect-excel-hive-odbc-driver.md). |
| Hiveserver2-n |443 |JDBC |JDBC használatával tooHive csatlakozik. Lásd: [tooHive hdinsight Hive JDBC-illesztőt hello használata csatlakozáshoz](hdinsight-connect-hive-jdbc-driver.md) |

hello alábbi adott fürt esetében érhetők el:

| Szolgáltatás | Port | Protokoll | Fürttípus | Leírás |
| --- | --- | --- | --- | --- |
| Stargate |443 |HTTPS |HBase |HBase REST API-t. Lásd: [HBase első lépései](hdinsight-hbase-tutorial-get-started-linux.md) |
| Livy |443 |HTTPS |Spark |Spark REST API-t. Lásd: [küldje el külső feladatok távolról a Livy használatával](hdinsight-apache-spark-livy-rest-interface.md) |
| Storm |443 |HTTPS |Storm |A Storm webes felhasználói felület. Lásd: [központi telepítése és kezelése a HDInsight alatt futó Storm-topológiák](hdinsight-storm-deploy-monitor-topology-linux.md) |

### <a name="authentication"></a>Authentication

Minden szolgáltatás a hello internet hitelesíteni kell nyilvánosan elérhetővé:

| Port | Hitelesítő adatok |
| --- | --- |
| 22-es vagy 23 |hello fürt létrehozásakor megadott SSH hitelesítő |
| 443 |hello bejelentkezési nevet (alapértelmezett: admin) és a fürt létrehozása során beállított jelszót |

## <a name="non-public-ports"></a>Nem nyilvános portok

> [!NOTE]
> Egyes szolgáltatások adott fürt típusok csak érhetők el. A HBase például a HBase fürt típusok csak érhető el.

> [!IMPORTANT]
> Egyes szolgáltatások csak futtatható egy headnode egyszerre. Ha tooconnect toohello szolgáltatást az elsődleges headnode hello kísérlet, és a 404-es hibaüzenetet kap, próbálkozzon a hello másodlagos headnode használatával.

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
| NameNode webes felhasználói felület |HEAD csomópontok |30070 |HTTPS |Webes felhasználói felület tooview állapota |
| NameNode metaadat-szolgáltatás |HEAD csomópontok |8020 |IPC |Fájl rendszer metaadatai |
| DataNode |Az összes munkavégző csomópontokhoz |30075 |HTTPS |Webes felhasználói felület tooview állapota, naplói, stb. |
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
| Erőforrás-kezelő felületen |HEAD csomópontok |8050 |HTTP |Hello alkalmazások manager felületén címe |
| NodeManager |Az összes munkavégző csomópontokhoz |30050 |&nbsp; |hello tároló manager hello címe |
| NodeManager webes felhasználói felület |Az összes munkavégző csomópontokhoz |30060 |HTTP |Erőforrás-kezelő felület |
| Az idősor cím |HEAD csomópontok |10200 |RPC |hello ütemterv szolgáltatás RPC szolgáltatás. |
| Az idősor webes felhasználói felület |HEAD csomópontok |8181 |HTTP |hello ütemterv szolgáltatás webes felhasználói felület |

### <a name="hive-ports"></a>Hive-portok

| Szolgáltatás | Csomópontok | Port | Protokoll | Leírás |
| --- | --- | --- | --- | --- |
| Hiveserver2-n |HEAD csomópontok |10001 |Thrift |Kapcsolódás tooHive (Thrift/JDBC) szolgáltatás |
| Hive Metaadattárhoz |HEAD csomópontok |9083 |Thrift |A kapcsolódó tooHive metaadatok (Thrift/JDBC) szolgáltatás |

### <a name="webhcat-ports"></a>WebHCat-portok

| Szolgáltatás | Csomópontok | Port | Protokoll | Leírás |
| --- | --- | --- | --- | --- |
| WebHCat-kiszolgáló |HEAD csomópontok |30111 |HTTP |Webes API-k fölött HCatalog és egyéb Hadoop-szolgáltatás |

### <a name="mapreduce-ports"></a>MapReduce portok

| Szolgáltatás | Csomópontok | Port | Protokoll | Leírás |
| --- | --- | --- | --- | --- |
| JobHistory |HEAD csomópontok |19888 |HTTP |MapReduce JobHistory webes felhasználói felület |
| JobHistory |HEAD csomópontok |10020 |&nbsp; |MapReduce JobHistory kiszolgáló |
| ShuffleHandler |&nbsp; |13562 |&nbsp; |Átvitel köztes térkép toorequesting szűkítő kimenete |

### <a name="oozie"></a>Oozie

| Szolgáltatás | Csomópontok | Port | Protokoll | Leírás |
| --- | --- | --- | --- | --- |
| Oozie kiszolgáló |HEAD csomópontok |11000 |HTTP |Oozie szolgáltatás URL-címe |
| Oozie kiszolgáló |HEAD csomópontok |11001 |HTTP |A Oozie rendszergazdai port |

### <a name="ambari-metrics"></a>Ambari metrikák

| Szolgáltatás | Csomópontok | Port | Protokoll | Leírás |
| --- | --- | --- | --- | --- |
| Az idősor (alkalmazás Előzmények) |HEAD csomópontok |6188 |HTTP |hello ütemterv szolgáltatás webes felhasználói felület |
| Az idősor (alkalmazás Előzmények) |HEAD csomópontok |30200 |RPC |hello ütemterv szolgáltatás webes felhasználói felület |

### <a name="hbase-ports"></a>A HBase-portok

| Szolgáltatás | Csomópontok | Port | Protokoll | Leírás |
| --- | --- | --- | --- | --- |
| HMaster |HEAD csomópontok |16000 |&nbsp; |&nbsp; |
| Webes felhasználói felületén HMaster adatai |HEAD csomópontok |16010 |HTTP |hello port hello HBase fő webes felhasználói felület |
| A régióban kiszolgáló |Az összes munkavégző csomópontokhoz |16020 |&nbsp; |&nbsp; |
| &nbsp; |&nbsp; |2181 |&nbsp; |ügyfelek használó tooconnect tooZooKeeper hello port |

### <a name="kafka-ports"></a>Kafka portok

| Szolgáltatás | Csomópontok | Port | Protokoll | Leírás |
| --- | --- | --- | --- | --- |
| Broker |Munkavégző csomópontokhoz |9092 |[Kafka protokoll](http://kafka.apache.org/protocol.html) |Az ügyfél-kommunikációhoz használt |
| &nbsp; |Zookeeper csomópontok |2181 |&nbsp; |ügyfelek használó tooconnect tooZookeeper hello port |

### <a name="spark-ports"></a>A Spark-portok

| Szolgáltatás | Csomópontok | Port | Protokoll | URL-címe | Leírás |
| --- | --- | --- | --- | --- | --- |
| A Spark Thrift-kiszolgálók |HEAD csomópontok |10002 |Thrift | &nbsp; | Csatlakozás SQL (Thrift/JDBC) tooSpark szolgáltatás |
| Livy kiszolgáló | HEAD csomópontok | 8998 | HTTP | /batches | Szolgáltatás utasítások, feladatok és alkalmazások |

Példák:

* Livy: `curl "http://10.0.0.11:8998/batches"`. Ebben a példában `10.0.0.11` hello headnode hello Livy szolgáltatást üzemeltető hello IP-címe.
