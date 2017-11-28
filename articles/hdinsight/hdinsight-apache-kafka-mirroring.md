---
title: "Apache Kafka témakörök - Azure HDInsight tükrözésének |} Microsoft Docs"
description: "Útmutató Apache Kafka tükrözési szolgáltatás használatával egy replikát készít egy HDInsight-fürt Kafka karbantartása másodlagos fürtre témakörök tükrözése révén."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 015d276e-f678-4f2b-9572-75553c56625b
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/13/2017
ms.author: larryfr
ms.openlocfilehash: e418cb01e1a9168e3662e8d6242903e052b6047b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="use-mirrormaker-to-replicate-apache-kafka-topics-with-kafka-on-hdinsight-preview"></a><span data-ttu-id="fe72a-103">MirrorMaker segítségével replikálni, Apache Kafka témakörök Kafka a HDInsight (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="fe72a-103">Use MirrorMaker to replicate Apache Kafka topics with Kafka on HDInsight (preview)</span></span>

<span data-ttu-id="fe72a-104">Útmutató Apache Kafka tükrözési szolgáltatás használatával kapcsolatos témakörök replikálása egy másodlagos fürthöz.</span><span class="sxs-lookup"><span data-stu-id="fe72a-104">Learn how to use Apache Kafka's mirroring feature to replicate topics to a secondary cluster.</span></span> <span data-ttu-id="fe72a-105">Tükrözés folyamatos folyamatként lefutott, és áttelepítése módszerként időnként használt adatokat az egyik fürtről a másikra.</span><span class="sxs-lookup"><span data-stu-id="fe72a-105">Mirroring can be ran as a continuous process, or used intermittently as a method of migrating data from one cluster to another.</span></span>

<span data-ttu-id="fe72a-106">Ebben a példában a Tükrözés történő replikálásához használt témakörök két HDInsight-fürtök között.</span><span class="sxs-lookup"><span data-stu-id="fe72a-106">In this example, mirroring is used to replicate topics between two HDInsight clusters.</span></span> <span data-ttu-id="fe72a-107">Mindkét fürtről egy Azure virtuális hálózat ugyanabban a régióban van.</span><span class="sxs-lookup"><span data-stu-id="fe72a-107">Both clusters are in an Azure Virtual Network in the same region.</span></span>

> [!WARNING]
> <span data-ttu-id="fe72a-108">Tükrözés nem tekinthető hibatűrést eléréséhez eszközként.</span><span class="sxs-lookup"><span data-stu-id="fe72a-108">Mirroring should not be considered as a means to achieve fault-tolerance.</span></span> <span data-ttu-id="fe72a-109">Az eltolás egy témakör elemekre különböznek a forrás és cél fürtök, így nem tudják használni a két azonos értelemben.</span><span class="sxs-lookup"><span data-stu-id="fe72a-109">The offset to items within a topic are different between the source and destination clusters, so clients cannot use the two interchangeably.</span></span>
>
> <span data-ttu-id="fe72a-110">Ha a hibatűrés replikációs a témakörök a fürtön belül kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="fe72a-110">If you are concerned about fault tolerance, you should set replication for the topics within your cluster.</span></span> <span data-ttu-id="fe72a-111">További információkért lásd: [Ismerkedjen meg a HDInsight Kafka](hdinsight-apache-kafka-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="fe72a-111">For more information, see [Get started with Kafka on HDInsight](hdinsight-apache-kafka-get-started.md).</span></span>

## <a name="how-kafka-mirroring-works"></a><span data-ttu-id="fe72a-112">Tükrözés Kafka működése</span><span class="sxs-lookup"><span data-stu-id="fe72a-112">How Kafka mirroring works</span></span>

<span data-ttu-id="fe72a-113">Tükrözés működik a MirrorMaker eszközzel (Apache Kafka része) a témakörök a kiindulási fürt bejegyzéseit használnak, és ezután hozzon létre egy helyi példány a célfürtön.</span><span class="sxs-lookup"><span data-stu-id="fe72a-113">Mirroring works by using the MirrorMaker tool (part of Apache Kafka) to consume records from topics on the source cluster and then create a local copy on the destination cluster.</span></span> <span data-ttu-id="fe72a-114">MirrorMaker használ egy (vagy több) *fogyasztók* , olvassa el a forrás-fürtből, és egy *készítő* , amely a helyi (cél) fürt ír.</span><span class="sxs-lookup"><span data-stu-id="fe72a-114">MirrorMaker uses one (or more) *consumers* that read from the source cluster, and a *producer* that writes to the local (destination) cluster.</span></span>

<span data-ttu-id="fe72a-115">Az alábbi ábra a tükrözés folyamatát mutatja be:</span><span class="sxs-lookup"><span data-stu-id="fe72a-115">The following diagram illustrates the Mirroring process:</span></span>

![A tükrözési folyamat diagramja](./media/hdinsight-apache-kafka-mirroring/kafka-mirroring.png)

<span data-ttu-id="fe72a-117">A HDInsight Apache Kafka nem biztosít hozzáférést a Kafka szolgáltatáshoz a nyilvános interneten keresztül.</span><span class="sxs-lookup"><span data-stu-id="fe72a-117">Apache Kafka on HDInsight does not provide access to the Kafka service over the public internet.</span></span> <span data-ttu-id="fe72a-118">Kafka gyártók vagy a fogyasztók a Kafka fürt csomópontja azonos Azure virtuális hálózaton kell lennie.</span><span class="sxs-lookup"><span data-stu-id="fe72a-118">Kafka producers or consumers must be in the same Azure virtual network as the nodes in the Kafka cluster.</span></span> <span data-ttu-id="fe72a-119">Ehhez a példához mindkét a Kafka forrás és cél fürtök Azure virtuális hálózat található.</span><span class="sxs-lookup"><span data-stu-id="fe72a-119">For this example, both the Kafka source and destination clusters are located in an Azure virtual network.</span></span> <span data-ttu-id="fe72a-120">Az alábbi ábra bemutatja, hogyan kommunikációs a fürtök között zajló kommunikációról:</span><span class="sxs-lookup"><span data-stu-id="fe72a-120">The following diagram shows how communication flows between the clusters:</span></span>

![Egy Azure virtuális hálózatban fürtök forrás és cél Kafka ábrája](./media/hdinsight-apache-kafka-mirroring/spark-kafka-vnet.png)

<span data-ttu-id="fe72a-122">Lehet, hogy a forrás és cél fürtök különböző csomópontok és a partíciók számának, és a témakörök belül eltolások különböző is.</span><span class="sxs-lookup"><span data-stu-id="fe72a-122">The source and destination clusters can be different in the number of nodes and partitions, and offsets within the topics are different also.</span></span> <span data-ttu-id="fe72a-123">A kulcs értékét, particionálás, amellyel tükrözés tart fenn, így sorrendje a kulcs alapú megőrződik.</span><span class="sxs-lookup"><span data-stu-id="fe72a-123">Mirroring maintains the key value that is used for partitioning, so record order is preserved on a per-key basis.</span></span>

### <a name="mirroring-across-network-boundaries"></a><span data-ttu-id="fe72a-124">Hálózati határokon keresztül történő tükrözés</span><span class="sxs-lookup"><span data-stu-id="fe72a-124">Mirroring across network boundaries</span></span>

<span data-ttu-id="fe72a-125">Különböző hálózatokon Kafka fürtök közötti tükrözésének van szüksége, ha van a következő szempontokat:</span><span class="sxs-lookup"><span data-stu-id="fe72a-125">If you need to mirror between Kafka clusters in different networks, there are the following additional considerations:</span></span>

* <span data-ttu-id="fe72a-126">**Átjárók**: A hálózatok tudjanak kommunikálni a TCPIP szinten kell lennie.</span><span class="sxs-lookup"><span data-stu-id="fe72a-126">**Gateways**: The networks must be able to communicate at the TCPIP level.</span></span>

* <span data-ttu-id="fe72a-127">**Nevek feloldása**: Kafka a fürt minden egyes hálózati segítségével hostnames csatlakozni egymáshoz képesnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="fe72a-127">**Name resolution**: The Kafka clusters in each network must be able to connect to each other by using hostnames.</span></span> <span data-ttu-id="fe72a-128">A tartománynévrendszer (DNS) kiszolgáló minden más hálózataihoz kérelmek továbbítása konfigurált hálózati elképzelhető, hogy szükség.</span><span class="sxs-lookup"><span data-stu-id="fe72a-128">This may require a Domain Name System (DNS) server in each network that is configured to forward requests to the other networks.</span></span>

    <span data-ttu-id="fe72a-129">A automatikus DNS, a hálózati megadott helyett egy Azure virtuális hálózat létrehozásakor meg kell adnia egy egyéni DNS-kiszolgáló és a kiszolgáló IP-címét.</span><span class="sxs-lookup"><span data-stu-id="fe72a-129">When creating an Azure Virtual Network, instead of using the automatic DNS provided with the network, you must specify a custom DNS server and the IP address for the server.</span></span> <span data-ttu-id="fe72a-130">A virtuális hálózat létrehozása után, majd hozzon létre egy Azure virtuális gép által használt IP-címet, majd telepítenie és konfigurálnia kell DNS szoftver rajta.</span><span class="sxs-lookup"><span data-stu-id="fe72a-130">After the Virtual Network has been created, you must then create an Azure Virtual Machine that uses that IP address, then install and configure DNS software on it.</span></span>

    > [!WARNING]
    > <span data-ttu-id="fe72a-131">Hozzon létre, és az egyéni DNS-kiszolgáló konfigurálása a HDInsight a virtuális hálózaton történő telepítése előtt.</span><span class="sxs-lookup"><span data-stu-id="fe72a-131">Create and configure the custom DNS server before installing HDInsight into the Virtual Network.</span></span> <span data-ttu-id="fe72a-132">A HDInsight a konfigurált virtuális hálózat DNS-kiszolgáló használatára szükség további beállításokra van.</span><span class="sxs-lookup"><span data-stu-id="fe72a-132">There is no additional configuration required for HDInsight to use the DNS server configured for the Virtual Network.</span></span>

<span data-ttu-id="fe72a-133">A két Azure virtuális hálózatokhoz csatlakozó további információkért lásd: [VNet – VNet-kapcsolatot konfiguráló](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="fe72a-133">For more information on connecting two Azure Virtual Networks, see [Configure a VNet-to-VNet connection](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md).</span></span>

## <a name="create-kafka-clusters"></a><span data-ttu-id="fe72a-134">Kafka fürtök létrehozása</span><span class="sxs-lookup"><span data-stu-id="fe72a-134">Create Kafka clusters</span></span>

<span data-ttu-id="fe72a-135">Létrehozhat egy Azure virtuális hálózatra, és manuálisan fürtök Kafka, célszerűbb Azure Resource Manager sablonnal.</span><span class="sxs-lookup"><span data-stu-id="fe72a-135">While you can create an Azure virtual network and Kafka clusters manually, it's easier to use an Azure Resource Manager template.</span></span> <span data-ttu-id="fe72a-136">Az alábbi lépések segítségével egy Azure virtuális hálózat és két Kafka fürt központi telepítése az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="fe72a-136">Use the following steps to deploy an Azure virtual network and two Kafka clusters to your Azure subscription.</span></span>

1. <span data-ttu-id="fe72a-137">A következő gomb segítségével jelentkezzen be az Azure-ba, és nyissa meg a sablon az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="fe72a-137">Use the following button to sign in to Azure and open the template in the Azure portal.</span></span>
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json" target="_blank"><img src="./media/hdinsight-apache-kafka-mirroring/deploy-to-azure.png" alt="Deploy to Azure"></a>
   
    <span data-ttu-id="fe72a-138">Az Azure Resource Manager sablon itt található: **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json**.</span><span class="sxs-lookup"><span data-stu-id="fe72a-138">The Azure Resource Manager template is located at **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json**.</span></span>

    > [!WARNING]
    > <span data-ttu-id="fe72a-139">A HDInsightban futó Kafka platform rendelkezésre állásának biztosításához fürtjének legalább három feldolgozó csomópontot kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="fe72a-139">To guarantee availability of Kafka on HDInsight, your cluster must contain at least three worker nodes.</span></span> <span data-ttu-id="fe72a-140">Ez a sablon a három munkavégző csomópontokhoz tartalmaz Kafka fürtöt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="fe72a-140">This template creates a Kafka cluster that contains three worker nodes.</span></span>

2. <span data-ttu-id="fe72a-141">A következő információk segítségével a feltöltik a **egyéni telepítési** panel:</span><span class="sxs-lookup"><span data-stu-id="fe72a-141">Use the following information to populate the entries on the **Custom deployment** blade:</span></span>
    
    ![HDInsight egyéni központi telepítés](./media/hdinsight-apache-kafka-mirroring/parameters.png)
    
    * <span data-ttu-id="fe72a-143">**Erőforráscsoport**: hozzon létre egy csoportot, vagy válasszon egy meglévőt.</span><span class="sxs-lookup"><span data-stu-id="fe72a-143">**Resource group**: Create a group or select an existing one.</span></span> <span data-ttu-id="fe72a-144">Ez a csoport tartalmazza a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="fe72a-144">This group contains the HDInsight cluster.</span></span>

    * <span data-ttu-id="fe72a-145">**Hely**: Adjon meg egy földrajzilag Önhöz legközelebb eső helyet.</span><span class="sxs-lookup"><span data-stu-id="fe72a-145">**Location**: Select a location geographically close to you.</span></span>
     
    * <span data-ttu-id="fe72a-146">**Fürt neve kiinduló**: Ez az érték használható alap neveként a Kafka fürtök számára.</span><span class="sxs-lookup"><span data-stu-id="fe72a-146">**Base Cluster Name**: This value is used as the base name for the Kafka clusters.</span></span> <span data-ttu-id="fe72a-147">Ha például **hdi** nevű fürtök létrehozása **forrás-hdi** és **cél-hdi**.</span><span class="sxs-lookup"><span data-stu-id="fe72a-147">For example, entering **hdi** creates clusters named **source-hdi** and **dest-hdi**.</span></span>

    * <span data-ttu-id="fe72a-148">**A fürt bejelentkezési felhasználónevét**: A rendszergazda felhasználóneve a forrás és cél Kafka fürtök.</span><span class="sxs-lookup"><span data-stu-id="fe72a-148">**Cluster Login User Name**: The admin user name for the source and destination Kafka clusters.</span></span>

    * <span data-ttu-id="fe72a-149">**A fürt bejelentkezési jelszó**: a forrás- és a rendszergazdai jelszóval Kafka fürtök.</span><span class="sxs-lookup"><span data-stu-id="fe72a-149">**Cluster Login Password**: The admin user password for the source and destination Kafka clusters.</span></span>

    * <span data-ttu-id="fe72a-150">**SSH-felhasználónév**: az SSH-felhasználó a forrás és cél Kafka fürtök létrehozása.</span><span class="sxs-lookup"><span data-stu-id="fe72a-150">**SSH User Name**: The SSH user to create for the source and destination Kafka clusters.</span></span>

    * <span data-ttu-id="fe72a-151">**SSH-jelszónak**: a forrás- és az SSH-felhasználó jelszavát Kafka fürtök.</span><span class="sxs-lookup"><span data-stu-id="fe72a-151">**SSH Password**: The password for the SSH user for the source and destination Kafka clusters.</span></span>

3. <span data-ttu-id="fe72a-152">Olvassa el a **feltételek és kikötések**, majd válassza ki **elfogadom a feltételeket és a fenti feltételek**.</span><span class="sxs-lookup"><span data-stu-id="fe72a-152">Read the **Terms and Conditions**, and then select **I agree to the terms and conditions stated above**.</span></span>

4. <span data-ttu-id="fe72a-153">Végül ellenőrizze **rögzítés az irányítópulton** majd **beszerzési**.</span><span class="sxs-lookup"><span data-stu-id="fe72a-153">Finally, check **Pin to dashboard** and then select **Purchase**.</span></span> <span data-ttu-id="fe72a-154">A fürt létrehozása nagyjából 20 percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="fe72a-154">It takes about 20 minutes to create the clusters.</span></span>

<span data-ttu-id="fe72a-155">Az erőforrások létrehozása után, ekkor megnyílik egy panel, ahhoz az erőforráscsoporthoz, amely tartalmazza a fürtök és a webes irányítópult.</span><span class="sxs-lookup"><span data-stu-id="fe72a-155">Once the resources have been created, you are redirected to a blade for the resource group that contains the clusters and web dashboard.</span></span>

![A virtuális hálózat és a fürtök erőforráscsoport panel](./media/hdinsight-apache-kafka-mirroring/groupblade.png)

> [!IMPORTANT]
> <span data-ttu-id="fe72a-157">Figyelje meg, hogy a HDInsight-fürtök neve **forrás-BASENAME** és **cél-BASENAME**, ahol BASENAME a sablonhoz megadott név.</span><span class="sxs-lookup"><span data-stu-id="fe72a-157">Notice that the names of the HDInsight clusters are **source-BASENAME** and **dest-BASENAME**, where BASENAME is the name you provided to the template.</span></span> <span data-ttu-id="fe72a-158">Ezeket a neveket a későbbi lépésekben használja, a fürtök történő csatlakozás során.</span><span class="sxs-lookup"><span data-stu-id="fe72a-158">You use these names in later steps when connecting to the clusters.</span></span>

## <a name="create-topics"></a><span data-ttu-id="fe72a-159">Hozzon létre kapcsolatos témakörök</span><span class="sxs-lookup"><span data-stu-id="fe72a-159">Create topics</span></span>

1. <span data-ttu-id="fe72a-160">Csatlakozás a **forrás** fürtön SSH használatával:</span><span class="sxs-lookup"><span data-stu-id="fe72a-160">Connect to the **source** cluster using SSH:</span></span>

    ```bash
    ssh sshuser@source-BASENAME-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="fe72a-161">Cserélje le **sshuser** az a fürt létrehozásakor használt SSH-felhasználónév.</span><span class="sxs-lookup"><span data-stu-id="fe72a-161">Replace **sshuser** with the SSH user name used when creating the cluster.</span></span> <span data-ttu-id="fe72a-162">Cserélje le **BASENAME** a fürt létrehozásakor használt alap névvel.</span><span class="sxs-lookup"><span data-stu-id="fe72a-162">Replace **BASENAME** with the base name used when creating the cluster.</span></span>

    <span data-ttu-id="fe72a-163">További információk: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="fe72a-163">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="fe72a-164">Az alábbi parancsokkal a Zookeeper állomások találhatók a kiindulási fürt:</span><span class="sxs-lookup"><span data-stu-id="fe72a-164">Use the following commands to find the Zookeeper hosts for the source cluster:</span></span>

    ```bash
    # Install jq if it is not installed
    sudo apt -y install jq
    # get the zookeeper hosts for the source cluster
    export SOURCE_ZKHOSTS=`curl -sS -u admin:$PASSWORD -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`
    
    Replace `$PASSWORD` with the password for the cluster.

    Replace `$CLUSTERNAME` with the name of the source cluster.

3. To create a topic named `testtopic`, use the following command:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 2 --partitions 8 --topic testtopic --zookeeper $SOURCE_ZKHOSTS
    ```

3. <span data-ttu-id="fe72a-165">A következő parancs használatával győződjön meg arról, hogy létrejött-e a következő témakörben:</span><span class="sxs-lookup"><span data-stu-id="fe72a-165">Use the following command to verify that the topic was created:</span></span>

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $SOURCE_ZKHOSTS
    ```

    <span data-ttu-id="fe72a-166">A válaszban `testtopic`.</span><span class="sxs-lookup"><span data-stu-id="fe72a-166">The response contains `testtopic`.</span></span>

4. <span data-ttu-id="fe72a-167">A Zookeeper állomás kapcsolatos információ megtekintéséhez használja a következő (a **forrás**) fürt:</span><span class="sxs-lookup"><span data-stu-id="fe72a-167">Use the following to view the Zookeeper host information for this (the **source**) cluster:</span></span>

    ```bash
    echo $SOURCE_ZKHOSTS
    ```

    <span data-ttu-id="fe72a-168">Ez hasonló ad vissza adatokat a következő szöveget:</span><span class="sxs-lookup"><span data-stu-id="fe72a-168">This returns information similar to the following text:</span></span>

    `zk0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:2181,zk1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:2181`

    <span data-ttu-id="fe72a-169">Mentse ezt az információt.</span><span class="sxs-lookup"><span data-stu-id="fe72a-169">Save this information.</span></span> <span data-ttu-id="fe72a-170">A következő szakaszban használatban van.</span><span class="sxs-lookup"><span data-stu-id="fe72a-170">It is used in the next section.</span></span>

## <a name="configure-mirroring"></a><span data-ttu-id="fe72a-171">Konfigurálja a tükrözés</span><span class="sxs-lookup"><span data-stu-id="fe72a-171">Configure mirroring</span></span>

1. <span data-ttu-id="fe72a-172">Csatlakozás a **cél** a fürt egy másik SSH-munkamenetet használatával:</span><span class="sxs-lookup"><span data-stu-id="fe72a-172">Connect to the **destination** cluster using a different SSH session:</span></span>

    ```bash
    ssh sshuser@dest-BASENAME-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="fe72a-173">Cserélje le **sshuser** az a fürt létrehozásakor használt SSH-felhasználónév.</span><span class="sxs-lookup"><span data-stu-id="fe72a-173">Replace **sshuser** with the SSH user name used when creating the cluster.</span></span> <span data-ttu-id="fe72a-174">Cserélje le **BASENAME** a fürt létrehozásakor használt alap névvel.</span><span class="sxs-lookup"><span data-stu-id="fe72a-174">Replace **BASENAME** with the base name used when creating the cluster.</span></span>

    <span data-ttu-id="fe72a-175">További információk: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="fe72a-175">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="fe72a-176">Az alábbi parancs segítségével hozzon létre egy `consumer.properties` fájlt, amely azt ismerteti, hogyan kommunikáljon a **forrás** fürt:</span><span class="sxs-lookup"><span data-stu-id="fe72a-176">Use the following command to create a `consumer.properties` file that describes how to communicate with the **source** cluster:</span></span>

    ```bash
    nano consumer.properties
    ```

    <span data-ttu-id="fe72a-177">Használja a következő szöveget a tartalmát a `consumer.properties` fájlt:</span><span class="sxs-lookup"><span data-stu-id="fe72a-177">Use the following text as the contents of the `consumer.properties` file:</span></span>

    ```yaml
    zookeeper.connect=SOURCE_ZKHOSTS
    group.id=mirrorgroup
    ```

    <span data-ttu-id="fe72a-178">Cserélje le **SOURCE_ZKHOSTS** adataival a Zookeeper állomások a **forrás** fürt.</span><span class="sxs-lookup"><span data-stu-id="fe72a-178">Replace **SOURCE_ZKHOSTS** with the Zookeeper hosts information from the **source** cluster.</span></span>

    <span data-ttu-id="fe72a-179">Ez a fájl olvasásához a forrás Kafka fürt fogyasztói információkat ismerteti.</span><span class="sxs-lookup"><span data-stu-id="fe72a-179">This file describes the consumer information to use when reading from the source Kafka cluster.</span></span> <span data-ttu-id="fe72a-180">További információk a fogyasztói konfigurációhoz, lásd: [fogyasztói Configs](https://kafka.apache.org/documentation#consumerconfigs) kafka.apache.org címen.</span><span class="sxs-lookup"><span data-stu-id="fe72a-180">For more information consumer configuration, see [Consumer Configs](https://kafka.apache.org/documentation#consumerconfigs) at kafka.apache.org.</span></span>

    <span data-ttu-id="fe72a-181">Mentse a fájlt, használja a **Ctrl + X**, **Y**, majd **Enter**.</span><span class="sxs-lookup"><span data-stu-id="fe72a-181">To save the file, use **Ctrl + X**, **Y**, and then **Enter**.</span></span>

3. <span data-ttu-id="fe72a-182">Mielőtt konfigurálná a termelő kommunikáló Miután a célfürtöt, keresse meg az átvitelszervező-állomás a **cél** fürt.</span><span class="sxs-lookup"><span data-stu-id="fe72a-182">Before configuring the producer that communicates with the destination cluster, you must find the broker hosts for the **destination** cluster.</span></span> <span data-ttu-id="fe72a-183">A következő parancsok használatával ezek az információk beolvasása:</span><span class="sxs-lookup"><span data-stu-id="fe72a-183">Use the following commands to retrieve this information:</span></span>

    ```bash
    sudo apt -y install jq
    DEST_BROKERHOSTS=`curl -sS -u admin:$PASSWORD -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`
    echo $DEST_BROKERHOSTS
    ```

    <span data-ttu-id="fe72a-184">Cserélje le `$PASSWORD` a fürt bejelentkezési fiók (felügyeleti) jelszavával.</span><span class="sxs-lookup"><span data-stu-id="fe72a-184">Replace `$PASSWORD` with the login account (admin) password for the cluster.</span></span>

    <span data-ttu-id="fe72a-185">Cserélje le `$CLUSTERNAME` a célfürt nevével.</span><span class="sxs-lookup"><span data-stu-id="fe72a-185">Replace `$CLUSTERNAME` with the name of the destination cluster.</span></span>

    <span data-ttu-id="fe72a-186">Ezek a parancsok adatokat ad vissza a következőhöz hasonló:</span><span class="sxs-lookup"><span data-stu-id="fe72a-186">These commands return information similar to the following:</span></span>

        wn0-dest.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn1-dest.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092

4. <span data-ttu-id="fe72a-187">Használja a következő létrehozásához egy `producer.properties` fájlt, amely azt ismerteti, hogyan kommunikáljon a **cél** fürt:</span><span class="sxs-lookup"><span data-stu-id="fe72a-187">Use the following to create a `producer.properties` file that describes how to communicate with the **destination** cluster:</span></span>

    ```bash
    nano producer.properties
    ```

    <span data-ttu-id="fe72a-188">Használja a következő szöveget a tartalmát a `producer.properties` fájlt:</span><span class="sxs-lookup"><span data-stu-id="fe72a-188">Use the following text as the contents of the `producer.properties` file:</span></span>

    ```yaml
    bootstrap.servers=DEST_BROKERS
    compression.type=none
    ```

    <span data-ttu-id="fe72a-189">Cserélje le **DEST_BROKERS** az előző lépésben broker adataival.</span><span class="sxs-lookup"><span data-stu-id="fe72a-189">Replace **DEST_BROKERS** with the broker information from the previous step.</span></span>

    <span data-ttu-id="fe72a-190">További információk készítő konfigurációs, lásd: [készítő Configs](https://kafka.apache.org/documentation#producerconfigs) kafka.apache.org címen.</span><span class="sxs-lookup"><span data-stu-id="fe72a-190">For more information producer configuration, see [Producer Configs](https://kafka.apache.org/documentation#producerconfigs) at kafka.apache.org.</span></span>

## <a name="start-mirrormaker"></a><span data-ttu-id="fe72a-191">Indítsa el a MirrorMaker</span><span class="sxs-lookup"><span data-stu-id="fe72a-191">Start MirrorMaker</span></span>

1. <span data-ttu-id="fe72a-192">Az SSH-kapcsolatot a a **cél** fürt esetén a MirrorMaker folyamat a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="fe72a-192">From the SSH connection to the **destination** cluster, use the following command to start the MirrorMaker process:</span></span>

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-run-class.sh kafka.tools.MirrorMaker --consumer.config consumer.properties --producer.config producer.properties --whitelist testtopic --num.streams 4
    ```

    <span data-ttu-id="fe72a-193">Ebben a példában használt paraméterek a következők:</span><span class="sxs-lookup"><span data-stu-id="fe72a-193">The parameters used in this example are:</span></span>

    * <span data-ttu-id="fe72a-194">**--consumer.config**: felhasználói tulajdonságokat tartalmazó fájlt határozza meg.</span><span class="sxs-lookup"><span data-stu-id="fe72a-194">**--consumer.config**: Specifies the file that contains consumer properties.</span></span> <span data-ttu-id="fe72a-195">Ezek a tulajdonságok hozhatók létre, amely olvassa be az ügyféllel a *forrás* Kafka fürt.</span><span class="sxs-lookup"><span data-stu-id="fe72a-195">These properties are used to create a consumer that reads from the *source* Kafka cluster.</span></span>

    * <span data-ttu-id="fe72a-196">**--producer.config**: készítő tulajdonságokat tartalmazó fájlt határozza meg.</span><span class="sxs-lookup"><span data-stu-id="fe72a-196">**--producer.config**: Specifies the file that contains producer properties.</span></span> <span data-ttu-id="fe72a-197">Ezek a tulajdonságok hozhatók létre, ír termelő a *cél* Kafka fürt.</span><span class="sxs-lookup"><span data-stu-id="fe72a-197">These properties are used to create a producer that writes to the *destination* Kafka cluster.</span></span>

    * <span data-ttu-id="fe72a-198">**--az engedélyezett**: replikáló MirrorMaker a forrás-fürtről a cél-témakörök listáját.</span><span class="sxs-lookup"><span data-stu-id="fe72a-198">**--whitelist**: A list of topics that MirrorMaker replicates from the source cluster to the destination.</span></span>

    * <span data-ttu-id="fe72a-199">**--num.streams**: létrehozásához fogyasztói szálak száma.</span><span class="sxs-lookup"><span data-stu-id="fe72a-199">**--num.streams**: The number of consumer threads to create.</span></span>

 <span data-ttu-id="fe72a-200">Rendszerindításkor MirrorMaker adatait adja vissza az alábbihoz hasonló:</span><span class="sxs-lookup"><span data-stu-id="fe72a-200">On startup, MirrorMaker returns information similar to the following text:</span></span>

    ```json
    {metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-3, security.protocol=PLAINTEXT}{metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-0, security.protocol=PLAINTEXT}
    metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-kafka.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-2, security.protocol=PLAINTEXT}
    metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-1, security.protocol=PLAINTEXT}
    ```

2. <span data-ttu-id="fe72a-201">Az SSH-kapcsolatot a a **forrás** fürt esetén az alábbi parancs segítségével indítsa el a gyártó és üzenetek küldése a következő témakörben:</span><span class="sxs-lookup"><span data-stu-id="fe72a-201">From the SSH connection to the **source** cluster, use the following command to start a producer and send messages to the topic:</span></span>

    ```bash
    SOURCE_BROKERHOSTS=`curl -sS -u admin:$PASSWORD -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`
    /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $SOURCE_BROKERHOSTS --topic testtopic
    ```

    <span data-ttu-id="fe72a-202">Cserélje le `$PASSWORD` a kiindulási fürt (rendszergazda) bejelentkezési jelszavával.</span><span class="sxs-lookup"><span data-stu-id="fe72a-202">Replace `$PASSWORD` with the login (admin) password for the source cluster.</span></span>

    <span data-ttu-id="fe72a-203">Cserélje le `$CLUSTERNAME` a kiindulási fürt nevével.</span><span class="sxs-lookup"><span data-stu-id="fe72a-203">Replace `$CLUSTERNAME` with the name of the source cluster.</span></span>

     <span data-ttu-id="fe72a-204">Amikor egy üres sort a kurzorral érkeznek, adja meg néhány szöveges üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="fe72a-204">When you arrive at a blank line with a cursor, type in a few text messages.</span></span> <span data-ttu-id="fe72a-205">Ezek a témakör küldött a **forrás** fürt.</span><span class="sxs-lookup"><span data-stu-id="fe72a-205">These are sent to the topic on the **source** cluster.</span></span> <span data-ttu-id="fe72a-206">Amikor végzett, **Ctrl + C** a gyártó folyamat befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="fe72a-206">When done, use **Ctrl + C** to end the producer process.</span></span>

3. <span data-ttu-id="fe72a-207">Az SSH-kapcsolatot a a **cél** fürt esetén használjon **Ctrl + C** a MirrorMaker folyamat befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="fe72a-207">From the SSH connection to the **destination** cluster, use **Ctrl + C** to end the MirrorMaker process.</span></span> <span data-ttu-id="fe72a-208">A következő parancsok segítségével ellenőrizze, hogy a `testtopic` témakör jött létre, és a tükör történő replikálása a tárolt adatokat a következő témakörben:</span><span class="sxs-lookup"><span data-stu-id="fe72a-208">Then use the following commands to verify that the `testtopic` topic was created, and that data in the topic was replicated to this mirror:</span></span>

    ```bash
    DEST_ZKHOSTS=`curl -sS -u admin:$PASSWORD -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $DEST_ZKHOSTS
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --zookeeper $DEST_ZKHOSTS --topic testtopic --from-beginning
    ```

    <span data-ttu-id="fe72a-209">Cserélje le `$PASSWORD` a célfürt (rendszergazda) bejelentkezési jelszavával.</span><span class="sxs-lookup"><span data-stu-id="fe72a-209">Replace `$PASSWORD` with the login (admin) password for the destination cluster.</span></span>

    <span data-ttu-id="fe72a-210">Cserélje le `$CLUSTERNAME` a célfürt nevével.</span><span class="sxs-lookup"><span data-stu-id="fe72a-210">Replace `$CLUSTERNAME` with the name of the destination cluster.</span></span>

    <span data-ttu-id="fe72a-211">Most már tartalmaz témakörök listáját `testtopic`, ami akkor jön létre, amikor MirrorMaster tükrözi a témakör a forrás-fürtről a célhelyre.</span><span class="sxs-lookup"><span data-stu-id="fe72a-211">The list of topics now includes `testtopic`, which is created when MirrorMaster mirrors the topic from the source cluster to the destination.</span></span> <span data-ttu-id="fe72a-212">A témakör sorból beolvasott üzenetekből ugyanazok a kiindulási fürt beírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="fe72a-212">The messages retrieved from the topic are the same as entered on the source cluster.</span></span>

## <a name="delete-the-cluster"></a><span data-ttu-id="fe72a-213">A fürt törlése</span><span class="sxs-lookup"><span data-stu-id="fe72a-213">Delete the cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

<span data-ttu-id="fe72a-214">A jelen dokumentumban leírt lépések az azonos Azure erőforráscsoport mindkét fürtöket létrehozni, mert az erőforráscsoportot az Azure portálon törölheti.</span><span class="sxs-lookup"><span data-stu-id="fe72a-214">Since the steps in this document create both clusters in the same Azure resource group, you can delete the resource group in the Azure portal.</span></span> <span data-ttu-id="fe72a-215">Az erőforráscsoport törlése eltávolítja a következő Ez a dokumentum, az Azure-beli virtuális hálózatra és a fürt által használt tárfiók által létrehozott összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="fe72a-215">Deleting the resource group removes all resources created by following this document, the Azure Virtual Network, and storage account used by the clusters.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fe72a-216">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fe72a-216">Next Steps</span></span>

<span data-ttu-id="fe72a-217">Ebben a dokumentumban megtanulhatta MirrorMaker Kafka fürt másolatának létrehozásához használja.</span><span class="sxs-lookup"><span data-stu-id="fe72a-217">In this document, you learned how to use MirrorMaker to create a replica of a Kafka cluster.</span></span> <span data-ttu-id="fe72a-218">Az alábbi hivatkozások segítségével felderíteni a más módon történő együttműködésre Kafka:</span><span class="sxs-lookup"><span data-stu-id="fe72a-218">Use the following links to discover other ways to work with Kafka:</span></span>

* <span data-ttu-id="fe72a-219">[Apache Kafka MirrorMaker dokumentáció](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) cwiki.apache.org címen.</span><span class="sxs-lookup"><span data-stu-id="fe72a-219">[Apache Kafka MirrorMaker documentation](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) at cwiki.apache.org.</span></span>
* [<span data-ttu-id="fe72a-220">Ismerkedés az Apache Kafka a HDInsight-on</span><span class="sxs-lookup"><span data-stu-id="fe72a-220">Get started with Apache Kafka on HDInsight</span></span>](hdinsight-apache-kafka-get-started.md)
* [<span data-ttu-id="fe72a-221">Az Apache Spark használata a Kafkával a HDInsighton</span><span class="sxs-lookup"><span data-stu-id="fe72a-221">Use Apache Spark with Kafka on HDInsight</span></span>](hdinsight-apache-spark-with-kafka.md)
* [<span data-ttu-id="fe72a-222">Az Apache Storm használata a HDInsighton futó Kafkával</span><span class="sxs-lookup"><span data-stu-id="fe72a-222">Use Apache Storm with Kafka on HDInsight</span></span>](hdinsight-apache-storm-with-kafka.md)
* [<span data-ttu-id="fe72a-223">Csatlakozás a Kafkához Azure Virtual Networkön keresztül</span><span class="sxs-lookup"><span data-stu-id="fe72a-223">Connect to Kafka through an Azure Virtual Network</span></span>](hdinsight-apache-kafka-connect-vpn-gateway.md)
