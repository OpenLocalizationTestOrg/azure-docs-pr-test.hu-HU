---
title: "aaaMirror Apache Kafka témakörök - Azure HDInsight |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Apache Kafka tükrözési funkció toomaintain egy replikát készít egy HDInsight-fürt Kafka témakörök tooa másodlagos fürt tükrözése révén."
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
ms.openlocfilehash: 5ace0251d7402d4d7d9b28726e253ce7091a87ef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-mirrormaker-tooreplicate-apache-kafka-topics-with-kafka-on-hdinsight-preview"></a><span data-ttu-id="2b4b3-103">A HDInsight (előzetes verzió) Kafka MirrorMaker tooreplicate Apache Kafka témakörök használata</span><span class="sxs-lookup"><span data-stu-id="2b4b3-103">Use MirrorMaker tooreplicate Apache Kafka topics with Kafka on HDInsight (preview)</span></span>

<span data-ttu-id="2b4b3-104">Ismerje meg, hogyan toouse Apache Kafka a tükrözést a szolgáltatás tooreplicate témakörök tooa másodlagos fürt.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-104">Learn how toouse Apache Kafka's mirroring feature tooreplicate topics tooa secondary cluster.</span></span> <span data-ttu-id="2b4b3-105">Tükrözés folyamatos folyamatként lefutott, és áttelepítése módszerként időnként használt adatokat egy fürt tooanother.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-105">Mirroring can be ran as a continuous process, or used intermittently as a method of migrating data from one cluster tooanother.</span></span>

<span data-ttu-id="2b4b3-106">Ebben a példában a tükrözés jelenleg használt tooreplicate témakörök két HDInsight-fürtök között.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-106">In this example, mirroring is used tooreplicate topics between two HDInsight clusters.</span></span> <span data-ttu-id="2b4b3-107">Mindkét fürt szerepelnek a hello egy Azure virtuális hálózat ugyanabban a régióban.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-107">Both clusters are in an Azure Virtual Network in hello same region.</span></span>

> [!WARNING]
> <span data-ttu-id="2b4b3-108">Tükrözés nem kell tekinteni a azt jelenti, hogy tooachieve hibatűrést.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-108">Mirroring should not be considered as a means tooachieve fault-tolerance.</span></span> <span data-ttu-id="2b4b3-109">hello eltolási tooitems egy témakör különböznek hello forrás és cél fürtök, így nem tudják használni hello két azonos értelemben.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-109">hello offset tooitems within a topic are different between hello source and destination clusters, so clients cannot use hello two interchangeably.</span></span>
>
> <span data-ttu-id="2b4b3-110">Ha aggódik hibatűrést, a fürtön belül kell beállítani a replikációs hello témaköröket.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-110">If you are concerned about fault tolerance, you should set replication for hello topics within your cluster.</span></span> <span data-ttu-id="2b4b3-111">További információkért lásd: [Ismerkedjen meg a HDInsight Kafka](hdinsight-apache-kafka-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="2b4b3-111">For more information, see [Get started with Kafka on HDInsight](hdinsight-apache-kafka-get-started.md).</span></span>

## <a name="how-kafka-mirroring-works"></a><span data-ttu-id="2b4b3-112">Tükrözés Kafka működése</span><span class="sxs-lookup"><span data-stu-id="2b4b3-112">How Kafka mirroring works</span></span>

<span data-ttu-id="2b4b3-113">Tükrözési works hello MirrorMaker eszköz (Apache Kafka része) tooconsume használatával rögzíti a kiindulási fürt hello témaköröktől, és létrehozhat egy helyi példány hello cél fürtön.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-113">Mirroring works by using hello MirrorMaker tool (part of Apache Kafka) tooconsume records from topics on hello source cluster and then create a local copy on hello destination cluster.</span></span> <span data-ttu-id="2b4b3-114">MirrorMaker használ egy (vagy több) *fogyasztók* hello forrásfürt, olvasó és egy *készítő* , írja az toohello (cél) helyi fürt.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-114">MirrorMaker uses one (or more) *consumers* that read from hello source cluster, and a *producer* that writes toohello local (destination) cluster.</span></span>

<span data-ttu-id="2b4b3-115">a következő diagram hello hello tükrözés folyamatát mutatja be:</span><span class="sxs-lookup"><span data-stu-id="2b4b3-115">hello following diagram illustrates hello Mirroring process:</span></span>

![Hello tükrözés folyamat diagramja](./media/hdinsight-apache-kafka-mirroring/kafka-mirroring.png)

<span data-ttu-id="2b4b3-117">A HDInsight Apache Kafka nem biztosít hozzáférést toohello Kafka szolgáltatás képest hello nyilvános internethez.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-117">Apache Kafka on HDInsight does not provide access toohello Kafka service over hello public internet.</span></span> <span data-ttu-id="2b4b3-118">Kafka gyártók vagy fogyasztók kell hello hello Kafka fürtben csomópontként hello azonos Azure virtuális hálózatban.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-118">Kafka producers or consumers must be in hello same Azure virtual network as hello nodes in hello Kafka cluster.</span></span> <span data-ttu-id="2b4b3-119">Ehhez a példához hello Kafka forrás és a cél fürtök találhatók egy Azure virtuális hálózatra.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-119">For this example, both hello Kafka source and destination clusters are located in an Azure virtual network.</span></span> <span data-ttu-id="2b4b3-120">a következő ábra azt mutatja be, hogyan kommunikációs hello fürtök között zajló kommunikációról hello:</span><span class="sxs-lookup"><span data-stu-id="2b4b3-120">hello following diagram shows how communication flows between hello clusters:</span></span>

![Egy Azure virtuális hálózatban fürtök forrás és cél Kafka ábrája](./media/hdinsight-apache-kafka-mirroring/spark-kafka-vnet.png)

<span data-ttu-id="2b4b3-122">csomópontok és a partíciók száma hello hello forrás és cél fürtök eltérhet, és eltolások hello témakörök belül különböző is.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-122">hello source and destination clusters can be different in hello number of nodes and partitions, and offsets within hello topics are different also.</span></span> <span data-ttu-id="2b4b3-123">Particionálás, amellyel kulcsérték hello tükrözés tart fenn, így sorrendje megőrződik kulcs alapú.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-123">Mirroring maintains hello key value that is used for partitioning, so record order is preserved on a per-key basis.</span></span>

### <a name="mirroring-across-network-boundaries"></a><span data-ttu-id="2b4b3-124">Hálózati határokon keresztül történő tükrözés</span><span class="sxs-lookup"><span data-stu-id="2b4b3-124">Mirroring across network boundaries</span></span>

<span data-ttu-id="2b4b3-125">Különböző hálózatokon Kafka fürtök közötti toomirror van szüksége, ha nincsenek további szempontokról a következő hello:</span><span class="sxs-lookup"><span data-stu-id="2b4b3-125">If you need toomirror between Kafka clusters in different networks, there are hello following additional considerations:</span></span>

* <span data-ttu-id="2b4b3-126">**Átjárók**: hello hálózatok: hello TCPIP szint képes toocommunicate kell lennie.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-126">**Gateways**: hello networks must be able toocommunicate at hello TCPIP level.</span></span>

* <span data-ttu-id="2b4b3-127">**Nevek feloldása**: az egyes hálózati fürtök Kafka hello segítségével hostnames kell lennie más képes tooconnect tooeach.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-127">**Name resolution**: hello Kafka clusters in each network must be able tooconnect tooeach other by using hostnames.</span></span> <span data-ttu-id="2b4b3-128">Ez lehet szükség, a tartománynévrendszer (DNS) kiszolgáló, amely minden egyes hálózati beállítva tooforward kérelmek toohello más hálózatokkal.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-128">This may require a Domain Name System (DNS) server in each network that is configured tooforward requests toohello other networks.</span></span>

    <span data-ttu-id="2b4b3-129">Hello hálózattal hello automatikus DNS megadott helyett egy Azure virtuális hálózat létrehozásakor meg kell adnia egy egyéni DNS-kiszolgáló és a hello kiszolgáló IP-címe hello.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-129">When creating an Azure Virtual Network, instead of using hello automatic DNS provided with hello network, you must specify a custom DNS server and hello IP address for hello server.</span></span> <span data-ttu-id="2b4b3-130">Után a rendszer létrehozta a virtuális hálózati hello, majd hozzon létre egy Azure virtuális gép által használt IP-címet, majd telepítenie és konfigurálnia kell DNS szoftver rajta.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-130">After hello Virtual Network has been created, you must then create an Azure Virtual Machine that uses that IP address, then install and configure DNS software on it.</span></span>

    > [!WARNING]
    > <span data-ttu-id="2b4b3-131">Hozzon létre és hello egyéni DNS-kiszolgáló konfigurálása a virtuális hálózati hello HDInsight telepítése előtt.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-131">Create and configure hello custom DNS server before installing HDInsight into hello Virtual Network.</span></span> <span data-ttu-id="2b4b3-132">A HDInsight toouse hello DNS-kiszolgálóhoz konfigurált virtuális hálózati hello szükség további beállításokra van.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-132">There is no additional configuration required for HDInsight toouse hello DNS server configured for hello Virtual Network.</span></span>

<span data-ttu-id="2b4b3-133">A két Azure virtuális hálózatokhoz csatlakozó további információkért lásd: [VNet – VNet-kapcsolatot konfiguráló](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="2b4b3-133">For more information on connecting two Azure Virtual Networks, see [Configure a VNet-to-VNet connection](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md).</span></span>

## <a name="create-kafka-clusters"></a><span data-ttu-id="2b4b3-134">Kafka fürtök létrehozása</span><span class="sxs-lookup"><span data-stu-id="2b4b3-134">Create Kafka clusters</span></span>

<span data-ttu-id="2b4b3-135">Létrehozhat egy Azure virtuális hálózatra, és manuálisan fürtök Kafka, akkor könnyebb toouse Azure Resource Manager-sablonok.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-135">While you can create an Azure virtual network and Kafka clusters manually, it's easier toouse an Azure Resource Manager template.</span></span> <span data-ttu-id="2b4b3-136">Használja a következő lépéseket toodeploy hello Azure virtuális hálózat és két Kafka fürtök tooyour Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-136">Use hello following steps toodeploy an Azure virtual network and two Kafka clusters tooyour Azure subscription.</span></span>

1. <span data-ttu-id="2b4b3-137">A tooAzure gomb toosign és hello Azure-portál megnyitása hello sablon hello használata.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-137">Use hello following button toosign in tooAzure and open hello template in hello Azure portal.</span></span>
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json" target="_blank"><img src="./media/hdinsight-apache-kafka-mirroring/deploy-to-azure.png" alt="Deploy tooAzure"></a>
   
    <span data-ttu-id="2b4b3-138">hello Azure Resource Manager sablon itt található: **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json**.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-138">hello Azure Resource Manager template is located at **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json**.</span></span>

    > [!WARNING]
    > <span data-ttu-id="2b4b3-139">a HDInsight Kafka tooguarantee rendelkezésre állását, a fürt tartalmaznia kell legalább három munkavégző csomópontokhoz.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-139">tooguarantee availability of Kafka on HDInsight, your cluster must contain at least three worker nodes.</span></span> <span data-ttu-id="2b4b3-140">Ez a sablon a három munkavégző csomópontokhoz tartalmaz Kafka fürtöt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-140">This template creates a Kafka cluster that contains three worker nodes.</span></span>

2. <span data-ttu-id="2b4b3-141">Információk toopopulate hello tételek követően hello használata hello **egyéni telepítési** panel:</span><span class="sxs-lookup"><span data-stu-id="2b4b3-141">Use hello following information toopopulate hello entries on hello **Custom deployment** blade:</span></span>
    
    ![HDInsight egyéni központi telepítés](./media/hdinsight-apache-kafka-mirroring/parameters.png)
    
    * <span data-ttu-id="2b4b3-143">**Erőforráscsoport**: hozzon létre egy csoportot, vagy válasszon egy meglévőt.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-143">**Resource group**: Create a group or select an existing one.</span></span> <span data-ttu-id="2b4b3-144">Ez a csoport hello HDInsight-fürtöt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-144">This group contains hello HDInsight cluster.</span></span>

    * <span data-ttu-id="2b4b3-145">**Hely**: Válasszon egy helyet a földrajzi elhelyezkedés alapján Bezárás tooyou.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-145">**Location**: Select a location geographically close tooyou.</span></span>
     
    * <span data-ttu-id="2b4b3-146">**Fürt neve kiinduló**: Ez az érték neveként hello alapszintű hello Kafka fürtök használható.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-146">**Base Cluster Name**: This value is used as hello base name for hello Kafka clusters.</span></span> <span data-ttu-id="2b4b3-147">Ha például **hdi** nevű fürtök létrehozása **forrás-hdi** és **cél-hdi**.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-147">For example, entering **hdi** creates clusters named **source-hdi** and **dest-hdi**.</span></span>

    * <span data-ttu-id="2b4b3-148">**A fürt bejelentkezési felhasználónevét**: hello rendszergazda felhasználóneve hello forrás és cél Kafka fürtök.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-148">**Cluster Login User Name**: hello admin user name for hello source and destination Kafka clusters.</span></span>

    * <span data-ttu-id="2b4b3-149">**A fürt bejelentkezési jelszó**: hello rendszergazdai jelszóval hello forrás és cél Kafka fürtök.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-149">**Cluster Login Password**: hello admin user password for hello source and destination Kafka clusters.</span></span>

    * <span data-ttu-id="2b4b3-150">**SSH-felhasználónév**: hello SSH felhasználói toocreate hello forrás és cél Kafka fürtök.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-150">**SSH User Name**: hello SSH user toocreate for hello source and destination Kafka clusters.</span></span>

    * <span data-ttu-id="2b4b3-151">**SSH-jelszónak**: hello jelszó hello SSH hello forrás és cél Kafka fürtök.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-151">**SSH Password**: hello password for hello SSH user for hello source and destination Kafka clusters.</span></span>

3. <span data-ttu-id="2b4b3-152">Olvasási hello **feltételek és kikötések**, majd válassza ki **toohello feltételek és kikötések fenti elfogadom**.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-152">Read hello **Terms and Conditions**, and then select **I agree toohello terms and conditions stated above**.</span></span>

4. <span data-ttu-id="2b4b3-153">Végül ellenőrizze **PIN-kód toodashboard** majd **beszerzési**.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-153">Finally, check **Pin toodashboard** and then select **Purchase**.</span></span> <span data-ttu-id="2b4b3-154">Körülbelül 20 percet toocreate hello fürtök szükséges.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-154">It takes about 20 minutes toocreate hello clusters.</span></span>

<span data-ttu-id="2b4b3-155">Létrehozása után a hello erőforrásokat, átirányított tooa panel hello fürtök és a webes irányítópult tartalmazó erőforráscsoport hello áll.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-155">Once hello resources have been created, you are redirected tooa blade for hello resource group that contains hello clusters and web dashboard.</span></span>

![Erőforráscsoport panel hello vnet és fürtök](./media/hdinsight-apache-kafka-mirroring/groupblade.png)

> [!IMPORTANT]
> <span data-ttu-id="2b4b3-157">Figyelje meg, hogy vannak-e a HDInsight-fürtök hello hello nevei **forrás-BASENAME** és **cél-BASENAME**, ahol BASENAME toohello sablon megadott hello nevét.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-157">Notice that hello names of hello HDInsight clusters are **source-BASENAME** and **dest-BASENAME**, where BASENAME is hello name you provided toohello template.</span></span> <span data-ttu-id="2b4b3-158">Ezeket a neveket használja a későbbi lépésekben toohello fürtök kapcsolódáskor.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-158">You use these names in later steps when connecting toohello clusters.</span></span>

## <a name="create-topics"></a><span data-ttu-id="2b4b3-159">Hozzon létre kapcsolatos témakörök</span><span class="sxs-lookup"><span data-stu-id="2b4b3-159">Create topics</span></span>

1. <span data-ttu-id="2b4b3-160">Csatlakozás toohello **forrás** fürtön SSH használatával:</span><span class="sxs-lookup"><span data-stu-id="2b4b3-160">Connect toohello **source** cluster using SSH:</span></span>

    ```bash
    ssh sshuser@source-BASENAME-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="2b4b3-161">Cserélje le **sshuser** a hello hello fürt létrehozásakor használt SSH-felhasználónév.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-161">Replace **sshuser** with hello SSH user name used when creating hello cluster.</span></span> <span data-ttu-id="2b4b3-162">Cserélje le **BASENAME** hello fürt létrehozásakor használt hello Alap nevű.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-162">Replace **BASENAME** with hello base name used when creating hello cluster.</span></span>

    <span data-ttu-id="2b4b3-163">További információk: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="2b4b3-163">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="2b4b3-164">Használjon hello következő toofind hello Zookeeper állomások hello forrásfürt parancsokat:</span><span class="sxs-lookup"><span data-stu-id="2b4b3-164">Use hello following commands toofind hello Zookeeper hosts for hello source cluster:</span></span>

    ```bash
    # Install jq if it is not installed
    sudo apt -y install jq
    # get hello zookeeper hosts for hello source cluster
    export SOURCE_ZKHOSTS=`curl -sS -u admin:$PASSWORD -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`
    
    Replace `$PASSWORD` with hello password for hello cluster.

    Replace `$CLUSTERNAME` with hello name of hello source cluster.

3. toocreate a topic named `testtopic`, use hello following command:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 2 --partitions 8 --topic testtopic --zookeeper $SOURCE_ZKHOSTS
    ```

3. <span data-ttu-id="2b4b3-165">A következő parancs tooverify, amely a témakör hello használata hello jött létre:</span><span class="sxs-lookup"><span data-stu-id="2b4b3-165">Use hello following command tooverify that hello topic was created:</span></span>

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $SOURCE_ZKHOSTS
    ```

    <span data-ttu-id="2b4b3-166">hello válaszban `testtopic`.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-166">hello response contains `testtopic`.</span></span>

4. <span data-ttu-id="2b4b3-167">Használjon hello tooview hello Zookeeper állomás adatokat a következő (hello **forrás**) fürt:</span><span class="sxs-lookup"><span data-stu-id="2b4b3-167">Use hello following tooview hello Zookeeper host information for this (hello **source**) cluster:</span></span>

    ```bash
    echo $SOURCE_ZKHOSTS
    ```

    <span data-ttu-id="2b4b3-168">Ez visszaadja a szöveg a következő információk hasonló toohello:</span><span class="sxs-lookup"><span data-stu-id="2b4b3-168">This returns information similar toohello following text:</span></span>

    `zk0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:2181,zk1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:2181`

    <span data-ttu-id="2b4b3-169">Mentse ezt az információt.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-169">Save this information.</span></span> <span data-ttu-id="2b4b3-170">A következő szakaszban hello szolgál.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-170">It is used in hello next section.</span></span>

## <a name="configure-mirroring"></a><span data-ttu-id="2b4b3-171">Konfigurálja a tükrözés</span><span class="sxs-lookup"><span data-stu-id="2b4b3-171">Configure mirroring</span></span>

1. <span data-ttu-id="2b4b3-172">Csatlakozás toohello **cél** a fürt egy másik SSH-munkamenetet használatával:</span><span class="sxs-lookup"><span data-stu-id="2b4b3-172">Connect toohello **destination** cluster using a different SSH session:</span></span>

    ```bash
    ssh sshuser@dest-BASENAME-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="2b4b3-173">Cserélje le **sshuser** a hello hello fürt létrehozásakor használt SSH-felhasználónév.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-173">Replace **sshuser** with hello SSH user name used when creating hello cluster.</span></span> <span data-ttu-id="2b4b3-174">Cserélje le **BASENAME** hello fürt létrehozásakor használt hello Alap nevű.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-174">Replace **BASENAME** with hello base name used when creating hello cluster.</span></span>

    <span data-ttu-id="2b4b3-175">További információk: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="2b4b3-175">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="2b4b3-176">Használjon hello következő parancsot a toocreate egy `consumer.properties` fájlt, amely ismerteti, hogyan toocommunicate a hello **forrás** fürt:</span><span class="sxs-lookup"><span data-stu-id="2b4b3-176">Use hello following command toocreate a `consumer.properties` file that describes how toocommunicate with hello **source** cluster:</span></span>

    ```bash
    nano consumer.properties
    ```

    <span data-ttu-id="2b4b3-177">Szöveg hello hello tartalmát, a következő használatát hello `consumer.properties` fájlt:</span><span class="sxs-lookup"><span data-stu-id="2b4b3-177">Use hello following text as hello contents of hello `consumer.properties` file:</span></span>

    ```yaml
    zookeeper.connect=SOURCE_ZKHOSTS
    group.id=mirrorgroup
    ```

    <span data-ttu-id="2b4b3-178">Cserélje le **SOURCE_ZKHOSTS** a hello Zookeeper hello adatait tároló **forrás** fürt.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-178">Replace **SOURCE_ZKHOSTS** with hello Zookeeper hosts information from hello **source** cluster.</span></span>

    <span data-ttu-id="2b4b3-179">Ez a fájl hello fogyasztói információk toouse írja le, Kafka fürt hello forrásból olvasásakor.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-179">This file describes hello consumer information toouse when reading from hello source Kafka cluster.</span></span> <span data-ttu-id="2b4b3-180">További információk a fogyasztói konfigurációhoz, lásd: [fogyasztói Configs](https://kafka.apache.org/documentation#consumerconfigs) kafka.apache.org címen.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-180">For more information consumer configuration, see [Consumer Configs](https://kafka.apache.org/documentation#consumerconfigs) at kafka.apache.org.</span></span>

    <span data-ttu-id="2b4b3-181">toosave hello fájl használata **Ctrl + X**, **Y**, majd **Enter**.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-181">toosave hello file, use **Ctrl + X**, **Y**, and then **Enter**.</span></span>

3. <span data-ttu-id="2b4b3-182">Mielőtt konfigurálná a kommunikáló hello készítő hello cél fürttel, keresse meg hello broker állomások a hello **cél** fürt.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-182">Before configuring hello producer that communicates with hello destination cluster, you must find hello broker hosts for hello **destination** cluster.</span></span> <span data-ttu-id="2b4b3-183">A következő parancsok tooretrieve hello ezeket az információkat használhatja:</span><span class="sxs-lookup"><span data-stu-id="2b4b3-183">Use hello following commands tooretrieve this information:</span></span>

    ```bash
    sudo apt -y install jq
    DEST_BROKERHOSTS=`curl -sS -u admin:$PASSWORD -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`
    echo $DEST_BROKERHOSTS
    ```

    <span data-ttu-id="2b4b3-184">Cserélje le `$PASSWORD` hello bejelentkezési fiók (felügyeleti) jelszóval hello fürthöz.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-184">Replace `$PASSWORD` with hello login account (admin) password for hello cluster.</span></span>

    <span data-ttu-id="2b4b3-185">Cserélje le `$CLUSTERNAME` hello célfürtöt hello nevére.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-185">Replace `$CLUSTERNAME` with hello name of hello destination cluster.</span></span>

    <span data-ttu-id="2b4b3-186">Ezek a parancsok információk hasonló toohello következő vissza:</span><span class="sxs-lookup"><span data-stu-id="2b4b3-186">These commands return information similar toohello following:</span></span>

        wn0-dest.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn1-dest.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092

4. <span data-ttu-id="2b4b3-187">A következő toocreate használata hello egy `producer.properties` fájlt, amely ismerteti, hogyan toocommunicate hello a **cél** fürt:</span><span class="sxs-lookup"><span data-stu-id="2b4b3-187">Use hello following toocreate a `producer.properties` file that describes how toocommunicate with hello **destination** cluster:</span></span>

    ```bash
    nano producer.properties
    ```

    <span data-ttu-id="2b4b3-188">Szöveg hello hello tartalmát, a következő használatát hello `producer.properties` fájlt:</span><span class="sxs-lookup"><span data-stu-id="2b4b3-188">Use hello following text as hello contents of hello `producer.properties` file:</span></span>

    ```yaml
    bootstrap.servers=DEST_BROKERS
    compression.type=none
    ```

    <span data-ttu-id="2b4b3-189">Cserélje le **DEST_BROKERS** hello előző lépésben hello broker adatokkal.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-189">Replace **DEST_BROKERS** with hello broker information from hello previous step.</span></span>

    <span data-ttu-id="2b4b3-190">További információk készítő konfigurációs, lásd: [készítő Configs](https://kafka.apache.org/documentation#producerconfigs) kafka.apache.org címen.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-190">For more information producer configuration, see [Producer Configs](https://kafka.apache.org/documentation#producerconfigs) at kafka.apache.org.</span></span>

## <a name="start-mirrormaker"></a><span data-ttu-id="2b4b3-191">Indítsa el a MirrorMaker</span><span class="sxs-lookup"><span data-stu-id="2b4b3-191">Start MirrorMaker</span></span>

1. <span data-ttu-id="2b4b3-192">Az SSH-kapcsolat toohello hello **cél** fürt esetén a következő parancs toostart hello MirrorMaker folyamat hello használata:</span><span class="sxs-lookup"><span data-stu-id="2b4b3-192">From hello SSH connection toohello **destination** cluster, use hello following command toostart hello MirrorMaker process:</span></span>

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-run-class.sh kafka.tools.MirrorMaker --consumer.config consumer.properties --producer.config producer.properties --whitelist testtopic --num.streams 4
    ```

    <span data-ttu-id="2b4b3-193">Ebben a példában használt hello paraméterek a következők:</span><span class="sxs-lookup"><span data-stu-id="2b4b3-193">hello parameters used in this example are:</span></span>

    * <span data-ttu-id="2b4b3-194">**--consumer.config**: hello fájlt, amelyben felhasználói tulajdonságok adja meg.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-194">**--consumer.config**: Specifies hello file that contains consumer properties.</span></span> <span data-ttu-id="2b4b3-195">Ezek a tulajdonságok, amelyek hello olvassa be az ügyféllel használt toocreate *forrás* Kafka fürt.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-195">These properties are used toocreate a consumer that reads from hello *source* Kafka cluster.</span></span>

    * <span data-ttu-id="2b4b3-196">**--producer.config**: készítő tulajdonságokat tartalmazó hello fájlt adja meg.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-196">**--producer.config**: Specifies hello file that contains producer properties.</span></span> <span data-ttu-id="2b4b3-197">Ezek a Tulajdonságok használt toocreate írja az toohello termelő *cél* Kafka fürt.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-197">These properties are used toocreate a producer that writes toohello *destination* Kafka cluster.</span></span>

    * <span data-ttu-id="2b4b3-198">**--az engedélyezett**: hello forrás fürt toohello cél a replikáló MirrorMaker témakörök listáját.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-198">**--whitelist**: A list of topics that MirrorMaker replicates from hello source cluster toohello destination.</span></span>

    * <span data-ttu-id="2b4b3-199">**--num.streams**: hello fogyasztói szálak toocreate száma.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-199">**--num.streams**: hello number of consumer threads toocreate.</span></span>

 <span data-ttu-id="2b4b3-200">Rendszerindításkor MirrorMaker információkat a következő szöveg hasonló toohello adja vissza:</span><span class="sxs-lookup"><span data-stu-id="2b4b3-200">On startup, MirrorMaker returns information similar toohello following text:</span></span>

    ```json
    {metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-3, security.protocol=PLAINTEXT}{metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-0, security.protocol=PLAINTEXT}
    metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-kafka.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-2, security.protocol=PLAINTEXT}
    metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-1, security.protocol=PLAINTEXT}
    ```

2. <span data-ttu-id="2b4b3-201">Az SSH-kapcsolat toohello hello **forrás** fürt, használja a következő parancs toostart termelő hello és üzenetek toohello témakör küldeni:</span><span class="sxs-lookup"><span data-stu-id="2b4b3-201">From hello SSH connection toohello **source** cluster, use hello following command toostart a producer and send messages toohello topic:</span></span>

    ```bash
    SOURCE_BROKERHOSTS=`curl -sS -u admin:$PASSWORD -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`
    /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $SOURCE_BROKERHOSTS --topic testtopic
    ```

    <span data-ttu-id="2b4b3-202">Cserélje le `$PASSWORD` hello forrás fürt hello (rendszergazda) bejelentkezési jelszóval.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-202">Replace `$PASSWORD` with hello login (admin) password for hello source cluster.</span></span>

    <span data-ttu-id="2b4b3-203">Cserélje le `$CLUSTERNAME` hello forrásfürt hello nevére.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-203">Replace `$CLUSTERNAME` with hello name of hello source cluster.</span></span>

     <span data-ttu-id="2b4b3-204">Amikor egy üres sort a kurzorral érkeznek, adja meg néhány szöveges üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-204">When you arrive at a blank line with a cursor, type in a few text messages.</span></span> <span data-ttu-id="2b4b3-205">Ezek toohello témakör továbbküldené hello **forrás** fürt.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-205">These are sent toohello topic on hello **source** cluster.</span></span> <span data-ttu-id="2b4b3-206">Amikor végzett, **Ctrl + C** tooend hello készítő folyamat.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-206">When done, use **Ctrl + C** tooend hello producer process.</span></span>

3. <span data-ttu-id="2b4b3-207">Az SSH-kapcsolat toohello hello **cél** fürt esetén használjon **Ctrl + C** tooend hello MirrorMaker folyamat.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-207">From hello SSH connection toohello **destination** cluster, use **Ctrl + C** tooend hello MirrorMaker process.</span></span> <span data-ttu-id="2b4b3-208">Akkor használja hello következő parancsok tooverify adott hello `testtopic` témakör lett létrehozva, és tárolt adatokat hello témakör replikált toothis tükrözött:</span><span class="sxs-lookup"><span data-stu-id="2b4b3-208">Then use hello following commands tooverify that hello `testtopic` topic was created, and that data in hello topic was replicated toothis mirror:</span></span>

    ```bash
    DEST_ZKHOSTS=`curl -sS -u admin:$PASSWORD -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $DEST_ZKHOSTS
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --zookeeper $DEST_ZKHOSTS --topic testtopic --from-beginning
    ```

    <span data-ttu-id="2b4b3-209">Cserélje le `$PASSWORD` hello cél fürt hello (rendszergazda) bejelentkezési jelszóval.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-209">Replace `$PASSWORD` with hello login (admin) password for hello destination cluster.</span></span>

    <span data-ttu-id="2b4b3-210">Cserélje le `$CLUSTERNAME` hello célfürtöt hello nevére.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-210">Replace `$CLUSTERNAME` with hello name of hello destination cluster.</span></span>

    <span data-ttu-id="2b4b3-211">hello témakörlistáját most már tartalmaz `testtopic`, ami akkor jön létre, amikor MirrorMaster tükrözi hello forrás fürt toohello cél hello témakört.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-211">hello list of topics now includes `testtopic`, which is created when MirrorMaster mirrors hello topic from hello source cluster toohello destination.</span></span> <span data-ttu-id="2b4b3-212">hello témakör lekért köszönőüzenetei hello megegyeznek a megadott hello kiindulási fürt.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-212">hello messages retrieved from hello topic are hello same as entered on hello source cluster.</span></span>

## <a name="delete-hello-cluster"></a><span data-ttu-id="2b4b3-213">Hello fürt törlése</span><span class="sxs-lookup"><span data-stu-id="2b4b3-213">Delete hello cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

<span data-ttu-id="2b4b3-214">Mivel a jelen dokumentumban leírt lépések hello létrehozása a fürtök hello azonos Azure-erőforráscsoportot, törölheti a hello erőforráscsoportja hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-214">Since hello steps in this document create both clusters in hello same Azure resource group, you can delete hello resource group in hello Azure portal.</span></span> <span data-ttu-id="2b4b3-215">Hello erőforráscsoport törlése eltávolítja a hozta létre a következő a dokumentum, hello Azure virtuális hálózat és tárfiók hello fürtök által használt összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-215">Deleting hello resource group removes all resources created by following this document, hello Azure Virtual Network, and storage account used by hello clusters.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2b4b3-216">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2b4b3-216">Next Steps</span></span>

<span data-ttu-id="2b4b3-217">Ebből a dokumentumból megtanulta, hogyan toouse MirrorMaker toocreate egy replikát készít egy Kafka fürt.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-217">In this document, you learned how toouse MirrorMaker toocreate a replica of a Kafka cluster.</span></span> <span data-ttu-id="2b4b3-218">Használja a következő hivatkozások toodiscover hello más módokon toowork Kafka:</span><span class="sxs-lookup"><span data-stu-id="2b4b3-218">Use hello following links toodiscover other ways toowork with Kafka:</span></span>

* <span data-ttu-id="2b4b3-219">[Apache Kafka MirrorMaker dokumentáció](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) cwiki.apache.org címen.</span><span class="sxs-lookup"><span data-stu-id="2b4b3-219">[Apache Kafka MirrorMaker documentation](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) at cwiki.apache.org.</span></span>
* [<span data-ttu-id="2b4b3-220">Ismerkedés az Apache Kafka a HDInsight-on</span><span class="sxs-lookup"><span data-stu-id="2b4b3-220">Get started with Apache Kafka on HDInsight</span></span>](hdinsight-apache-kafka-get-started.md)
* [<span data-ttu-id="2b4b3-221">Az Apache Spark használata a Kafkával a HDInsighton</span><span class="sxs-lookup"><span data-stu-id="2b4b3-221">Use Apache Spark with Kafka on HDInsight</span></span>](hdinsight-apache-spark-with-kafka.md)
* [<span data-ttu-id="2b4b3-222">Az Apache Storm használata a HDInsighton futó Kafkával</span><span class="sxs-lookup"><span data-stu-id="2b4b3-222">Use Apache Storm with Kafka on HDInsight</span></span>](hdinsight-apache-storm-with-kafka.md)
* [<span data-ttu-id="2b4b3-223">Csatlakozás tooKafka egy Azure virtuális hálózaton keresztül</span><span class="sxs-lookup"><span data-stu-id="2b4b3-223">Connect tooKafka through an Azure Virtual Network</span></span>](hdinsight-apache-kafka-connect-vpn-gateway.md)
