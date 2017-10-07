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
# <a name="use-apache-phoenix-with-linux-based-hbase-clusters-in-hdinsight"></a><span data-ttu-id="7a911-103">Hdinsight Linux-alapú HBase-fürtökkel Apache Phoenix használata</span><span class="sxs-lookup"><span data-stu-id="7a911-103">Use Apache Phoenix with Linux-based HBase clusters in HDInsight</span></span>
<span data-ttu-id="7a911-104">Megtudhatja, hogyan toouse [Apache Phoenix](http://phoenix.apache.org/) a Hdinsightban, és hogyan toouse SQLLine.</span><span class="sxs-lookup"><span data-stu-id="7a911-104">Learn how toouse [Apache Phoenix](http://phoenix.apache.org/) in HDInsight, and how toouse SQLLine.</span></span> <span data-ttu-id="7a911-105">Phoenix kapcsolatos további információkért lásd: [Phoenix kevesebb mint 15 perc alatt](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html).</span><span class="sxs-lookup"><span data-stu-id="7a911-105">For more information about Phoenix, see [Phoenix in 15 minutes or less](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html).</span></span> <span data-ttu-id="7a911-106">Hello Phoenix nyelvtan, lásd: [Phoenix nyelvtan](http://phoenix.apache.org/language/index.html).</span><span class="sxs-lookup"><span data-stu-id="7a911-106">For hello Phoenix grammar, see [Phoenix Grammar](http://phoenix.apache.org/language/index.html).</span></span>

> [!NOTE]
> <span data-ttu-id="7a911-107">Hello Phoenix fájlverzió-információkat a Hdinsightban, lásd: [What's new in HDInsight által biztosított hello Hadoop-fürt verziók?](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="7a911-107">For hello Phoenix version information in HDInsight, see [What's new in hello Hadoop cluster versions provided by HDInsight?](hdinsight-component-versioning.md).</span></span>
>
>

## <a name="use-sqlline"></a><span data-ttu-id="7a911-108">SQLLine használata</span><span class="sxs-lookup"><span data-stu-id="7a911-108">Use SQLLine</span></span>
<span data-ttu-id="7a911-109">[SQLLine](http://sqlline.sourceforge.net/) egy parancssori segédprogram tooexecute SQL.</span><span class="sxs-lookup"><span data-stu-id="7a911-109">[SQLLine](http://sqlline.sourceforge.net/) is a command-line utility tooexecute SQL.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="7a911-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="7a911-110">Prerequisites</span></span>
<span data-ttu-id="7a911-111">SQLLine használata előtt hello következő kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="7a911-111">Before you can use SQLLine, you must have hello following:</span></span>

* <span data-ttu-id="7a911-112">**A HDInsight HBase-fürtöt**.</span><span class="sxs-lookup"><span data-stu-id="7a911-112">**An HBase cluster in HDInsight**.</span></span> <span data-ttu-id="7a911-113">HBase kiépítési információk fürt esetén lásd: [Ismerkedés az Apache HBase hdinsightban][hdinsight-hbase-get-started].</span><span class="sxs-lookup"><span data-stu-id="7a911-113">For information on provision HBase cluster, see [Get started with Apache HBase in HDInsight][hdinsight-hbase-get-started].</span></span>
* <span data-ttu-id="7a911-114">**Csatlakozás toohello HBase-fürtöt hello távoli asztal protokollal**.</span><span class="sxs-lookup"><span data-stu-id="7a911-114">**Connect toohello HBase cluster via hello remote desktop protocol**.</span></span> <span data-ttu-id="7a911-115">Útmutatásért lásd: [kezelése Hadoop-fürtöket a HDInsight használatával hello Azure-portálon][hdinsight-manage-portal].</span><span class="sxs-lookup"><span data-stu-id="7a911-115">For instructions, see [Manage Hadoop clusters in HDInsight by using hello Azure portal][hdinsight-manage-portal].</span></span>

<span data-ttu-id="7a911-116">Csatlakozás tooan HBase-fürtöt, meg kell a hello Zookeepers tooconnect tooone.</span><span class="sxs-lookup"><span data-stu-id="7a911-116">When you connect tooan HBase cluster, you need tooconnect tooone of hello Zookeepers.</span></span> <span data-ttu-id="7a911-117">Minden egyes HDInsight-fürt három Zookeepers rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="7a911-117">Each HDInsight cluster has three Zookeepers.</span></span>

<span data-ttu-id="7a911-118">**kimenő hello Zookeeper állomásnév toofind**</span><span class="sxs-lookup"><span data-stu-id="7a911-118">**toofind out hello Zookeeper host name**</span></span>

1. <span data-ttu-id="7a911-119">Nyissa meg az Ambari tallózással túl**https://<ClusterName>. azurehdinsight.net**.</span><span class="sxs-lookup"><span data-stu-id="7a911-119">Open Ambari by browsing too**https://<ClusterName>.azurehdinsight.net**.</span></span>
2. <span data-ttu-id="7a911-120">Adja meg a hello HTTP (fürt) felhasználónév és jelszó toologin.</span><span class="sxs-lookup"><span data-stu-id="7a911-120">Enter hello HTTP (cluster) username and password toologin.</span></span>
3. <span data-ttu-id="7a911-121">Kattintson a **ZooKeeper** hello bal oldali menüből.</span><span class="sxs-lookup"><span data-stu-id="7a911-121">Click **ZooKeeper** from hello left menu.</span></span> <span data-ttu-id="7a911-122">Lásd: három **ZooKeeper Server** szerepel a listában.</span><span class="sxs-lookup"><span data-stu-id="7a911-122">You see three **ZooKeeper Server** listed.</span></span>
4. <span data-ttu-id="7a911-123">Kattintson az egyik hello **ZooKeeper Server** szerepel a listában.</span><span class="sxs-lookup"><span data-stu-id="7a911-123">Click one of hello **ZooKeeper Server** listed.</span></span> <span data-ttu-id="7a911-124">Megkeresi a hello összefoglalás ablaktábla hello **állomásnév**.</span><span class="sxs-lookup"><span data-stu-id="7a911-124">On hello Summary pane, find hello **Hostname**.</span></span> <span data-ttu-id="7a911-125">Abban a tekintetben hasonló túl*zk1-jdolehb.3lnng4rcvp5uzokyktxs4a5dhd.bx.internal.cloudapp.net*.</span><span class="sxs-lookup"><span data-stu-id="7a911-125">It is similar too*zk1-jdolehb.3lnng4rcvp5uzokyktxs4a5dhd.bx.internal.cloudapp.net*.</span></span>

<span data-ttu-id="7a911-126">**toouse SQLLine**</span><span class="sxs-lookup"><span data-stu-id="7a911-126">**toouse SQLLine**</span></span>

1. <span data-ttu-id="7a911-127">Csatlakozzon az SSH használatával toohello fürt.</span><span class="sxs-lookup"><span data-stu-id="7a911-127">Connect toohello cluster using SSH.</span></span> <span data-ttu-id="7a911-128">További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="7a911-128">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="7a911-129">Az SSH-ból futtassa a következő parancsok toorun SQLLine hello:</span><span class="sxs-lookup"><span data-stu-id="7a911-129">From SSH, run hello following commands toorun SQLLine:</span></span>

        cd /usr/hdp/2.2.9.1-7/phoenix/bin
        ./sqlline.py <ClusterName>:2181:/hbase-unsecure
3. <span data-ttu-id="7a911-130">Futtassa a következő parancsok toocreate hello a HBase táblában, és néhány adat beviteléhez:</span><span class="sxs-lookup"><span data-stu-id="7a911-130">Run hello following commands toocreate a HBase table, and insert some data:</span></span>

        CREATE TABLE Company (COMPANY_ID INTEGER PRIMARY KEY, NAME VARCHAR(225));

        !tables

        UPSERT INTO Company VALUES(1, 'Microsoft');

        SELECT * FROM Company;

        !quit

<span data-ttu-id="7a911-131">További információkért lásd: [SQLLine manuális](http://sqlline.sourceforge.net/#manual) és [Phoenix nyelvtan](http://phoenix.apache.org/language/index.html).</span><span class="sxs-lookup"><span data-stu-id="7a911-131">For more information, see [SQLLine manual](http://sqlline.sourceforge.net/#manual) and [Phoenix Grammar](http://phoenix.apache.org/language/index.html).</span></span>

## <a name="next-steps"></a><span data-ttu-id="7a911-132">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7a911-132">Next steps</span></span>
<span data-ttu-id="7a911-133">Ebben a cikkben megtanulta, hogyan toouse Apache Phoenix a hdinsight eszközben.</span><span class="sxs-lookup"><span data-stu-id="7a911-133">In this article, you have learned how toouse Apache Phoenix in HDInsight.</span></span>  <span data-ttu-id="7a911-134">toolearn több, lásd:</span><span class="sxs-lookup"><span data-stu-id="7a911-134">toolearn more, see:</span></span>

* <span data-ttu-id="7a911-135">[HDInsight HBase overview][hdinsight-hbase-overview] (A HDInsight HBase áttekintése): A HBase egy Apache, nyílt forráskódú, a Hadoopra épülő NoSQL-adatbázis, amely véletlenszerű hozzáférést és erős konzisztenciát biztosít a nagy mennyiségű strukturálatlan és félig strukturált adatok számára.</span><span class="sxs-lookup"><span data-stu-id="7a911-135">[HDInsight HBase overview][hdinsight-hbase-overview]: HBase is an Apache, open-source, NoSQL database built on Hadoop that provides random access and strong consistency for large amounts of unstructured and semistructured data.</span></span>
* <span data-ttu-id="7a911-136">[Azure virtuális hálózat HBase-fürtök kiépítése][hdinsight-hbase-provision-vnet]: A virtuális hálózati integráció, a HBase-fürtökkel telepíthetők telepített toohello azonos virtuális hálózati alkalmazásaihoz, így alkalmazások kommunikálhatnak A HBase közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="7a911-136">[Provision HBase clusters on Azure Virtual Network][hdinsight-hbase-provision-vnet]: With virtual network integration, HBase clusters can be deployed toohello same virtual network as your applications so that applications can communicate with HBase directly.</span></span>
* <span data-ttu-id="7a911-137">[A HDInsight HBase-replikálás konfigurálása](hdinsight-hbase-replication.md): megtudhatja, hogyan tooconfigure HBase-replikálás két Azure üzemeltetésében.</span><span class="sxs-lookup"><span data-stu-id="7a911-137">[Configure HBase replication in HDInsight](hdinsight-hbase-replication.md): Learn how tooconfigure HBase replication across two Azure datacenters.</span></span>
* <span data-ttu-id="7a911-138">[Elemzése a HBase a hdinsight eszközben Twitter sentiment][hbase-twitter-sentiment]: megtudhatja, hogyan valós idejű toodo [véleményeket elemzés](http://en.wikipedia.org/wiki/Sentiment_analysis) a big Data típusú adatok a HBase a hdinsight Hadoop-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="7a911-138">[Analyze Twitter sentiment with HBase in HDInsight][hbase-twitter-sentiment]: Learn how toodo real-time [sentiment analysis](http://en.wikipedia.org/wiki/Sentiment_analysis) of big data by using HBase in a Hadoop cluster in HDInsight.</span></span>

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
