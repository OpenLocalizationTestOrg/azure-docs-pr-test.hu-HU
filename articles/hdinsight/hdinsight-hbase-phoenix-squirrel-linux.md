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
# <a name="use-apache-phoenix-with-linux-based-hbase-clusters-in-hdinsight"></a><span data-ttu-id="d847f-103">Hdinsight Linux-alapú HBase-fürtökkel Apache Phoenix használata</span><span class="sxs-lookup"><span data-stu-id="d847f-103">Use Apache Phoenix with Linux-based HBase clusters in HDInsight</span></span>
<span data-ttu-id="d847f-104">Ismerje meg, hogyan használható [Apache Phoenix](http://phoenix.apache.org/) , és hogyan SQLLine használható.</span><span class="sxs-lookup"><span data-stu-id="d847f-104">Learn how to use [Apache Phoenix](http://phoenix.apache.org/) in HDInsight, and how to use SQLLine.</span></span> <span data-ttu-id="d847f-105">Phoenix kapcsolatos további információkért lásd: [Phoenix kevesebb mint 15 perc alatt](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html).</span><span class="sxs-lookup"><span data-stu-id="d847f-105">For more information about Phoenix, see [Phoenix in 15 minutes or less](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html).</span></span> <span data-ttu-id="d847f-106">A Phoenix nyelvtan, lásd: [Phoenix nyelvtan](http://phoenix.apache.org/language/index.html).</span><span class="sxs-lookup"><span data-stu-id="d847f-106">For the Phoenix grammar, see [Phoenix Grammar](http://phoenix.apache.org/language/index.html).</span></span>

> [!NOTE]
> <span data-ttu-id="d847f-107">A Phoenix fájlverzió-információkat a Hdinsightban, lásd: [What's new in HDInsight által biztosított Hadoop-fürt verziók?](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="d847f-107">For the Phoenix version information in HDInsight, see [What's new in the Hadoop cluster versions provided by HDInsight?](hdinsight-component-versioning.md).</span></span>
>
>

## <a name="use-sqlline"></a><span data-ttu-id="d847f-108">SQLLine használata</span><span class="sxs-lookup"><span data-stu-id="d847f-108">Use SQLLine</span></span>
<span data-ttu-id="d847f-109">[SQLLine](http://sqlline.sourceforge.net/) egy parancssori segédprogram SQL végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="d847f-109">[SQLLine](http://sqlline.sourceforge.net/) is a command-line utility to execute SQL.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="d847f-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d847f-110">Prerequisites</span></span>
<span data-ttu-id="d847f-111">Mielőtt használhatná SQLLine, az alábbiakkal kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="d847f-111">Before you can use SQLLine, you must have the following:</span></span>

* <span data-ttu-id="d847f-112">**A HDInsight HBase-fürtöt**.</span><span class="sxs-lookup"><span data-stu-id="d847f-112">**An HBase cluster in HDInsight**.</span></span> <span data-ttu-id="d847f-113">HBase kiépítési információk fürt esetén lásd: [Ismerkedés az Apache HBase hdinsightban][hdinsight-hbase-get-started].</span><span class="sxs-lookup"><span data-stu-id="d847f-113">For information on provision HBase cluster, see [Get started with Apache HBase in HDInsight][hdinsight-hbase-get-started].</span></span>
* <span data-ttu-id="d847f-114">**A HBase-fürtöt, a távoli asztal protokollal csatlakozni**.</span><span class="sxs-lookup"><span data-stu-id="d847f-114">**Connect to the HBase cluster via the remote desktop protocol**.</span></span> <span data-ttu-id="d847f-115">Útmutatásért lásd: [kezelése Hadoop-fürtök a HDInsight az Azure portál használatával][hdinsight-manage-portal].</span><span class="sxs-lookup"><span data-stu-id="d847f-115">For instructions, see [Manage Hadoop clusters in HDInsight by using the Azure portal][hdinsight-manage-portal].</span></span>

<span data-ttu-id="d847f-116">Való csatlakozáskor HBase-fürtöt, a Zookeepers csatlakozni szeretne.</span><span class="sxs-lookup"><span data-stu-id="d847f-116">When you connect to an HBase cluster, you need to connect to one of the Zookeepers.</span></span> <span data-ttu-id="d847f-117">Minden egyes HDInsight-fürt három Zookeepers rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="d847f-117">Each HDInsight cluster has three Zookeepers.</span></span>

<span data-ttu-id="d847f-118">**Ha szeretné tudni, Zookeeper állomásneve**</span><span class="sxs-lookup"><span data-stu-id="d847f-118">**To find out the Zookeeper host name**</span></span>

1. <span data-ttu-id="d847f-119">Keresse meg azt az Ambari megnyitásához **https://<ClusterName>. azurehdinsight.net**.</span><span class="sxs-lookup"><span data-stu-id="d847f-119">Open Ambari by browsing to **https://<ClusterName>.azurehdinsight.net**.</span></span>
2. <span data-ttu-id="d847f-120">Adja meg a HTTP (fürt) felhasználónév és jelszó használatát a bejelentkezéshez.</span><span class="sxs-lookup"><span data-stu-id="d847f-120">Enter the HTTP (cluster) username and password to login.</span></span>
3. <span data-ttu-id="d847f-121">Kattintson a **ZooKeeper** a bal oldali menüből.</span><span class="sxs-lookup"><span data-stu-id="d847f-121">Click **ZooKeeper** from the left menu.</span></span> <span data-ttu-id="d847f-122">Lásd: három **ZooKeeper Server** szerepel a listában.</span><span class="sxs-lookup"><span data-stu-id="d847f-122">You see three **ZooKeeper Server** listed.</span></span>
4. <span data-ttu-id="d847f-123">Kattintson az egyik a **ZooKeeper Server** szerepel a listában.</span><span class="sxs-lookup"><span data-stu-id="d847f-123">Click one of the **ZooKeeper Server** listed.</span></span> <span data-ttu-id="d847f-124">Keresse meg a összefoglalás ablaktábla a **állomásnév**.</span><span class="sxs-lookup"><span data-stu-id="d847f-124">On the Summary pane, find the **Hostname**.</span></span> <span data-ttu-id="d847f-125">Hasonló *zk1-jdolehb.3lnng4rcvp5uzokyktxs4a5dhd.bx.internal.cloudapp.net*.</span><span class="sxs-lookup"><span data-stu-id="d847f-125">It is similar to *zk1-jdolehb.3lnng4rcvp5uzokyktxs4a5dhd.bx.internal.cloudapp.net*.</span></span>

<span data-ttu-id="d847f-126">**SQLLine használata**</span><span class="sxs-lookup"><span data-stu-id="d847f-126">**To use SQLLine**</span></span>

1. <span data-ttu-id="d847f-127">Csatlakozzon a fürthöz SSH használatával.</span><span class="sxs-lookup"><span data-stu-id="d847f-127">Connect to the cluster using SSH.</span></span> <span data-ttu-id="d847f-128">További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="d847f-128">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="d847f-129">Az SSH-ból SQLLine futtassa a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="d847f-129">From SSH, run the following commands to run SQLLine:</span></span>

        cd /usr/hdp/2.2.9.1-7/phoenix/bin
        ./sqlline.py <ClusterName>:2181:/hbase-unsecure
3. <span data-ttu-id="d847f-130">A következő parancsokat egy HBase tábla létrehozásához, és néhány adat beviteléhez:</span><span class="sxs-lookup"><span data-stu-id="d847f-130">Run the following commands to create a HBase table, and insert some data:</span></span>

        CREATE TABLE Company (COMPANY_ID INTEGER PRIMARY KEY, NAME VARCHAR(225));

        !tables

        UPSERT INTO Company VALUES(1, 'Microsoft');

        SELECT * FROM Company;

        !quit

<span data-ttu-id="d847f-131">További információkért lásd: [SQLLine manuális](http://sqlline.sourceforge.net/#manual) és [Phoenix nyelvtan](http://phoenix.apache.org/language/index.html).</span><span class="sxs-lookup"><span data-stu-id="d847f-131">For more information, see [SQLLine manual](http://sqlline.sourceforge.net/#manual) and [Phoenix Grammar](http://phoenix.apache.org/language/index.html).</span></span>

## <a name="next-steps"></a><span data-ttu-id="d847f-132">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d847f-132">Next steps</span></span>
<span data-ttu-id="d847f-133">Ebben a cikkben megtanulta rendelkezik Apache Phoenix használata a Hdinsightban.</span><span class="sxs-lookup"><span data-stu-id="d847f-133">In this article, you have learned how to use Apache Phoenix in HDInsight.</span></span>  <span data-ttu-id="d847f-134">További tudnivalókért lásd:</span><span class="sxs-lookup"><span data-stu-id="d847f-134">To learn more, see:</span></span>

* <span data-ttu-id="d847f-135">[HDInsight HBase overview][hdinsight-hbase-overview] (A HDInsight HBase áttekintése): A HBase egy Apache, nyílt forráskódú, a Hadoopra épülő NoSQL-adatbázis, amely véletlenszerű hozzáférést és erős konzisztenciát biztosít a nagy mennyiségű strukturálatlan és félig strukturált adatok számára.</span><span class="sxs-lookup"><span data-stu-id="d847f-135">[HDInsight HBase overview][hdinsight-hbase-overview]: HBase is an Apache, open-source, NoSQL database built on Hadoop that provides random access and strong consistency for large amounts of unstructured and semistructured data.</span></span>
* <span data-ttu-id="d847f-136">[Azure virtuális hálózat HBase-fürtök kiépítése][hdinsight-hbase-provision-vnet]: A virtuális hálózati integráció, HBase-fürtökkel telepíthetők az alkalmazások azonos virtuális hálózaton, hogy az alkalmazások közvetlenül kommunikálhatnak a HBase.</span><span class="sxs-lookup"><span data-stu-id="d847f-136">[Provision HBase clusters on Azure Virtual Network][hdinsight-hbase-provision-vnet]: With virtual network integration, HBase clusters can be deployed to the same virtual network as your applications so that applications can communicate with HBase directly.</span></span>
* <span data-ttu-id="d847f-137">[A HDInsight HBase-replikálás konfigurálása](hdinsight-hbase-replication.md): HBase-replikálás konfigurálása az Azure két adatközpont között.</span><span class="sxs-lookup"><span data-stu-id="d847f-137">[Configure HBase replication in HDInsight](hdinsight-hbase-replication.md): Learn how to configure HBase replication across two Azure datacenters.</span></span>
* <span data-ttu-id="d847f-138">[Elemzése a HBase a hdinsight eszközben Twitter sentiment][hbase-twitter-sentiment]: valós idejű módjáról [véleményeket elemzés](http://en.wikipedia.org/wiki/Sentiment_analysis) a big Data típusú adatok a HBase a hdinsight Hadoop-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="d847f-138">[Analyze Twitter sentiment with HBase in HDInsight][hbase-twitter-sentiment]: Learn how to do real-time [sentiment analysis](http://en.wikipedia.org/wiki/Sentiment_analysis) of big data by using HBase in a Hadoop cluster in HDInsight.</span></span>

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
