---
title: "Használja az Apache Phoenix & SQuirreL a HBase - az Azure HDInsight |} Microsoft Docs"
description: "Útmutató Apache Phoenix használata a Hdinsightban, és telepítésének és konfigurálásának SQuirreL való kapcsolódáshoz a hdinsight HBase-fürtöt a munkaállomáson."
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
ms.openlocfilehash: 13d17083bbe26fa9745ce4c5fef9f56859243c2e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-apache-phoenix-with-linux-based-hbase-clusters-in-hdinsight"></a>Hdinsight Linux-alapú HBase-fürtökkel Apache Phoenix használata
Ismerje meg, hogyan használható [Apache Phoenix](http://phoenix.apache.org/) , és hogyan SQLLine használható. Phoenix kapcsolatos további információkért lásd: [Phoenix kevesebb mint 15 perc alatt](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html). A Phoenix nyelvtan, lásd: [Phoenix nyelvtan](http://phoenix.apache.org/language/index.html).

> [!NOTE]
> A Phoenix fájlverzió-információkat a Hdinsightban, lásd: [What's new in HDInsight által biztosított Hadoop-fürt verziók?](hdinsight-component-versioning.md).
>
>

## <a name="use-sqlline"></a>SQLLine használata
[SQLLine](http://sqlline.sourceforge.net/) egy parancssori segédprogram SQL végrehajtása.

### <a name="prerequisites"></a>Előfeltételek
Mielőtt használhatná SQLLine, az alábbiakkal kell rendelkeznie:

* **A HDInsight HBase-fürtöt**. HBase kiépítési információk fürt esetén lásd: [Ismerkedés az Apache HBase hdinsightban][hdinsight-hbase-get-started].
* **A HBase-fürtöt, a távoli asztal protokollal csatlakozni**. Útmutatásért lásd: [kezelése Hadoop-fürtök a HDInsight az Azure portál használatával][hdinsight-manage-portal].

Való csatlakozáskor HBase-fürtöt, a Zookeepers csatlakozni szeretne. Minden egyes HDInsight-fürt három Zookeepers rendelkezik.

**Ha szeretné tudni, Zookeeper állomásneve**

1. Keresse meg azt az Ambari megnyitásához **https://<ClusterName>. azurehdinsight.net**.
2. Adja meg a HTTP (fürt) felhasználónév és jelszó használatát a bejelentkezéshez.
3. Kattintson a **ZooKeeper** a bal oldali menüből. Lásd: három **ZooKeeper Server** szerepel a listában.
4. Kattintson az egyik a **ZooKeeper Server** szerepel a listában. Keresse meg a összefoglalás ablaktábla a **állomásnév**. Hasonló *zk1-jdolehb.3lnng4rcvp5uzokyktxs4a5dhd.bx.internal.cloudapp.net*.

**SQLLine használata**

1. Csatlakozzon a fürthöz SSH használatával. További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Az SSH-ból SQLLine futtassa a következő parancsok futtatásával:

        cd /usr/hdp/2.2.9.1-7/phoenix/bin
        ./sqlline.py <ClusterName>:2181:/hbase-unsecure
3. A következő parancsokat egy HBase tábla létrehozásához, és néhány adat beviteléhez:

        CREATE TABLE Company (COMPANY_ID INTEGER PRIMARY KEY, NAME VARCHAR(225));

        !tables

        UPSERT INTO Company VALUES(1, 'Microsoft');

        SELECT * FROM Company;

        !quit

További információkért lásd: [SQLLine manuális](http://sqlline.sourceforge.net/#manual) és [Phoenix nyelvtan](http://phoenix.apache.org/language/index.html).

## <a name="next-steps"></a>Következő lépések
Ebben a cikkben megtanulta rendelkezik Apache Phoenix használata a Hdinsightban.  További tudnivalókért lásd:

* [HDInsight HBase overview][hdinsight-hbase-overview] (A HDInsight HBase áttekintése): A HBase egy Apache, nyílt forráskódú, a Hadoopra épülő NoSQL-adatbázis, amely véletlenszerű hozzáférést és erős konzisztenciát biztosít a nagy mennyiségű strukturálatlan és félig strukturált adatok számára.
* [Azure virtuális hálózat HBase-fürtök kiépítése][hdinsight-hbase-provision-vnet]: A virtuális hálózati integráció, HBase-fürtökkel telepíthetők az alkalmazások azonos virtuális hálózaton, hogy az alkalmazások közvetlenül kommunikálhatnak a HBase.
* [A HDInsight HBase-replikálás konfigurálása](hdinsight-hbase-replication.md): HBase-replikálás konfigurálása az Azure két adatközpont között.
* [Elemzése a HBase a hdinsight eszközben Twitter sentiment][hbase-twitter-sentiment]: valós idejű módjáról [véleményeket elemzés](http://en.wikipedia.org/wiki/Sentiment_analysis) a big Data típusú adatok a HBase a hdinsight Hadoop-fürthöz.

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
