---
title: aaaUse Apache Phoenix & SQuirreL a HBase - Azure HDInsight |} Microsoft Docs
description: "Megtudhatja, hogyan toouse Apache Phoenix a Hdinsightban, és hogyan tooinstall és a munkaállomás tooconnect tooan hdinsight HBase fürt SQuirreL konfigurálása."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: cda0f33b-a2e8-494c-972f-ae0bb482b818
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/26/2017
ms.author: jgao
ms.openlocfilehash: a63e8c8212b7a992453ec94fa638ec3863a0ede3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-apache-phoenix-with-linux-based-hbase-clusters-in-hdinsight"></a>Hdinsight Linux-alapú HBase-fürtökkel Apache Phoenix használata
Megtudhatja, hogyan toouse [Apache Phoenix](http://phoenix.apache.org/) a Hdinsightban, és hogyan toouse SQLLine. Phoenix kapcsolatos további információkért lásd: [Phoenix kevesebb mint 15 perc alatt](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html). Hello Phoenix nyelvtan, lásd: [Phoenix nyelvtan](http://phoenix.apache.org/language/index.html).

> [!NOTE]
> Hello Phoenix fájlverzió-információkat a Hdinsightban, lásd: [What's new in HDInsight által biztosított hello Hadoop-fürt verziók?](hdinsight-component-versioning.md).
>
>

## <a name="use-sqlline"></a>SQLLine használata
[SQLLine](http://sqlline.sourceforge.net/) egy parancssori segédprogram tooexecute SQL.

### <a name="prerequisites"></a>Előfeltételek
SQLLine használata előtt hello következő kell rendelkeznie:

* **A HDInsight HBase-fürtöt**. HBase kiépítési információk fürt esetén lásd: [Ismerkedés az Apache HBase hdinsightban][hdinsight-hbase-get-started].
* **Csatlakozás toohello HBase-fürtöt hello távoli asztal protokollal**. Útmutatásért lásd: [kezelése Hadoop-fürtöket a HDInsight használatával hello Azure-portálon][hdinsight-manage-portal].

Csatlakozás tooan HBase-fürtöt, meg kell a hello Zookeepers tooconnect tooone. Minden egyes HDInsight-fürt három Zookeepers rendelkezik.

**kimenő hello Zookeeper állomásnév toofind**

1. Nyissa meg az Ambari tallózással túl**https://<ClusterName>. azurehdinsight.net**.
2. Adja meg a hello HTTP (fürt) felhasználónév és jelszó toologin.
3. Kattintson a **ZooKeeper** hello bal oldali menüből. Lásd: három **ZooKeeper Server** szerepel a listában.
4. Kattintson az egyik hello **ZooKeeper Server** szerepel a listában. Megkeresi a hello összefoglalás ablaktábla hello **állomásnév**. Abban a tekintetben hasonló túl*zk1-jdolehb.3lnng4rcvp5uzokyktxs4a5dhd.bx.internal.cloudapp.net*.

**toouse SQLLine**

1. Csatlakozzon az SSH használatával toohello fürt. További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Az SSH-ból futtassa a következő parancsok toorun SQLLine hello:

        cd /usr/hdp/2.2.9.1-7/phoenix/bin
        ./sqlline.py <ClusterName>:2181:/hbase-unsecure
3. Futtassa a következő parancsok toocreate hello a HBase táblában, és néhány adat beviteléhez:

        CREATE TABLE Company (COMPANY_ID INTEGER PRIMARY KEY, NAME VARCHAR(225));

        !tables

        UPSERT INTO Company VALUES(1, 'Microsoft');

        SELECT * FROM Company;

        !quit

További információkért lásd: [SQLLine manuális](http://sqlline.sourceforge.net/#manual) és [Phoenix nyelvtan](http://phoenix.apache.org/language/index.html).

## <a name="next-steps"></a>Következő lépések
Ebben a cikkben megtanulta, hogyan toouse Apache Phoenix a hdinsight eszközben.  toolearn több, lásd:

* [HDInsight HBase overview][hdinsight-hbase-overview] (A HDInsight HBase áttekintése): A HBase egy Apache, nyílt forráskódú, a Hadoopra épülő NoSQL-adatbázis, amely véletlenszerű hozzáférést és erős konzisztenciát biztosít a nagy mennyiségű strukturálatlan és félig strukturált adatok számára.
* [Azure virtuális hálózat HBase-fürtök kiépítése][hdinsight-hbase-provision-vnet]: A virtuális hálózati integráció, a HBase-fürtökkel telepíthetők telepített toohello azonos virtuális hálózati alkalmazásaihoz, így alkalmazások kommunikálhatnak A HBase közvetlenül.
* [A HDInsight HBase-replikálás konfigurálása](hdinsight-hbase-replication.md): megtudhatja, hogyan tooconfigure HBase-replikálás két Azure üzemeltetésében.
* [Elemzése a HBase a hdinsight eszközben Twitter sentiment][hbase-twitter-sentiment]: megtudhatja, hogyan valós idejű toodo [véleményeket elemzés](http://en.wikipedia.org/wiki/Sentiment_analysis) a big Data típusú adatok a HBase a hdinsight Hadoop-fürthöz.

[azure-portal]: https://portal.azure.com
[vnet-point-to-site-connectivity]: https://msdn.microsoft.com/library/azure/09926218-92ab-4f43-aa99-83ab4d355555#BKMK_VNETPT

[hdinsight-hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md

[hdinsight-hbase-phoenix-sqlline]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-phoenix-sqlline.png
[img-certificate]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vpn-certificate.png
[img-vnet-diagram]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vnet-point-to-site.png
[img-squirrel-driver]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-driver.png
[img-squirrel-alias]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-alias.png
[img-squirrel]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel.png
[img-squirrel-sql]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-sql.png
