---
title: "az IntelliJ - hibakeresési alkalmazások távolról a HDInsight Spark eszköztára aaaAzure |} Microsoft Docs"
description: "Megtudhatja, hogyan használhat HDInsight eszközöket az Azure-eszközkészlet IntelliJ HDInsight Spark-fürtjei VPN-en keresztül futó tooremotely hibakeresési alkalmazás."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 55fb454f-c7dc-46de-a978-e242e9a94f4c
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: ad67d23bf609d0a7afb38b2acb110062f8b27b39
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-toolkit-for-intellij-toodebug-applications-remotely-on-hdinsight-spark-through-vpn"></a><span data-ttu-id="aed17-103">Az IntelliJ toodebug alkalmazások távolról a VPN-en keresztül a HDInsight Spark Azure eszközkészlet használata</span><span class="sxs-lookup"><span data-stu-id="aed17-103">Use Azure Toolkit for IntelliJ toodebug applications remotely on HDInsight Spark through VPN</span></span>

<span data-ttu-id="aed17-104">Azt javasoljuk, hogy a hibakeresési távolról a spark applicaltion ssh hello módon.</span><span class="sxs-lookup"><span data-stu-id="aed17-104">We recommend hello way of debugging spark applicaltion remotely through ssh.</span></span> <span data-ttu-id="aed17-105">Útmutatásért lásd: [távolról az IntelliJ SSH-n keresztül a Azure eszközkészlet a HDInsight-fürtök a Spark-alkalmazások hibakeresését](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh).</span><span class="sxs-lookup"><span data-stu-id="aed17-105">For instructions, see [Remotely debug Spark applications on an HDInsight cluster with Azure Toolkit for IntelliJ through SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh).</span></span>

<span data-ttu-id="aed17-106">Ez a cikk lépésenként bemutató biztosítanak toouse hello Azure eszközkészlet a HDInsight Tools IntelliJ toosubmit HDInsight Spark-fürt Spark feladatot és majd debug távolról, az asztali számítógépről.</span><span class="sxs-lookup"><span data-stu-id="aed17-106">This article provides step-by-step guidance on how toouse hello HDInsight Tools in Azure Toolkit for IntelliJ toosubmit a Spark job on HDInsight Spark cluster and then debug it remotely from your desktop computer.</span></span> <span data-ttu-id="aed17-107">toodo Igen, hello magas szintű lépéseket kell végrehajtania:</span><span class="sxs-lookup"><span data-stu-id="aed17-107">toodo so, you must perform hello following high-level steps:</span></span>

1. <span data-ttu-id="aed17-108">Pont-pont vagy a pont-pont Azure virtuális hálózat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="aed17-108">Create a site-to-site or point-to-site Azure Virtual Network.</span></span> <span data-ttu-id="aed17-109">Ez a dokumentum hello lépések azt feltételezik, hogy a pont-pont hálózati használható.</span><span class="sxs-lookup"><span data-stu-id="aed17-109">hello steps in this document assume that you use a site-to-site network.</span></span>
2. <span data-ttu-id="aed17-110">Hello-helyek Azure-beli virtuális hálózat részét képező Azure hdinsight Spark-fürt létrehozása.</span><span class="sxs-lookup"><span data-stu-id="aed17-110">Create a Spark cluster in Azure HDInsight that is part of hello site-to-site Azure Virtual Network.</span></span>
3. <span data-ttu-id="aed17-111">Ellenőrizze a hello kapcsolatát hello fürt headnode és az asztal között.</span><span class="sxs-lookup"><span data-stu-id="aed17-111">Verify hello connectivity between hello cluster headnode and your desktop.</span></span>
4. <span data-ttu-id="aed17-112">Az IntelliJ IDEA Scala-alkalmazás létrehozása, és konfigurálja a távoli hibakereséshez.</span><span class="sxs-lookup"><span data-stu-id="aed17-112">Create a Scala application in IntelliJ IDEA and configure it for remote debugging.</span></span>
5. <span data-ttu-id="aed17-113">Futtassa, és hello alkalmazás hibakeresése.</span><span class="sxs-lookup"><span data-stu-id="aed17-113">Run and debug hello application.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="aed17-114">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="aed17-114">Prerequisites</span></span>
* <span data-ttu-id="aed17-115">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="aed17-115">An Azure subscription.</span></span> <span data-ttu-id="aed17-116">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="aed17-116">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="aed17-117">A HDInsight az Apache Spark-fürt.</span><span class="sxs-lookup"><span data-stu-id="aed17-117">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="aed17-118">Útmutatásért lásd: [létrehozása az Apache Spark on Azure hdinsight clusters](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="aed17-118">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>
* <span data-ttu-id="aed17-119">Oracle Java fejlesztői készlet.</span><span class="sxs-lookup"><span data-stu-id="aed17-119">Oracle Java Development kit.</span></span> <span data-ttu-id="aed17-120">A későbbiekben telepítheti az [Itt](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span><span class="sxs-lookup"><span data-stu-id="aed17-120">You can install it from [here](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span></span>
* <span data-ttu-id="aed17-121">IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="aed17-121">IntelliJ IDEA.</span></span> <span data-ttu-id="aed17-122">Ez a cikk 2017.1 verzióját használja.</span><span class="sxs-lookup"><span data-stu-id="aed17-122">This article uses version 2017.1.</span></span> <span data-ttu-id="aed17-123">A későbbiekben telepítheti az [Itt](https://www.jetbrains.com/idea/download/).</span><span class="sxs-lookup"><span data-stu-id="aed17-123">You can install it from [here](https://www.jetbrains.com/idea/download/).</span></span>
* <span data-ttu-id="aed17-124">Az Azure eszköztára IntelliJ HDInsight eszközök.</span><span class="sxs-lookup"><span data-stu-id="aed17-124">HDInsight Tools in Azure Toolkit for IntelliJ.</span></span> <span data-ttu-id="aed17-125">A HDInsight tools for IntelliJ elérhetők hello Azure eszköztára IntelliJ részeként.</span><span class="sxs-lookup"><span data-stu-id="aed17-125">HDInsight tools for IntelliJ are available as part of hello Azure Toolkit for IntelliJ.</span></span> <span data-ttu-id="aed17-126">Hogyan tooinstall hello Azure eszközkészlet, lásd: [telepítése hello Azure eszköztára IntelliJ](../azure-toolkit-for-intellij-installation.md).</span><span class="sxs-lookup"><span data-stu-id="aed17-126">For instructions on how tooinstall hello Azure Toolkit, see [Installing hello Azure Toolkit for IntelliJ](../azure-toolkit-for-intellij-installation.md).</span></span>
* <span data-ttu-id="aed17-127">Jelentkezzen be az Azure-előfizetéshez az IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="aed17-127">Log into your Azure Subscription from IntelliJ IDEA.</span></span> <span data-ttu-id="aed17-128">Útmutatás alapján hello [Itt](hdinsight-apache-spark-intellij-tool-plugin.md).</span><span class="sxs-lookup"><span data-stu-id="aed17-128">Follow hello instructions [here](hdinsight-apache-spark-intellij-tool-plugin.md).</span></span>
* <span data-ttu-id="aed17-129">Futtatásakor a Spark Scala-alkalmazások a távoli hibakereséshez a Windows rendszerű számítógépeken, előfordulhat, hogy beolvasása kivételt, a [SPARK-2356](https://issues.apache.org/jira/browse/SPARK-2356) , amely akkor fordul elő, miatt tooa hiányzó WinUtils.exe Windows rendszeren.</span><span class="sxs-lookup"><span data-stu-id="aed17-129">While running Spark Scala application for remote debugging on a Windows computer, you might get an exception as explained in [SPARK-2356](https://issues.apache.org/jira/browse/SPARK-2356) that occurs due tooa missing WinUtils.exe on Windows.</span></span> <span data-ttu-id="aed17-130">Ezt a hibát körül toowork, kell [végrehajtható hello letölthető innen](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) tooa helyre, például **C:\WinUtils\bin**.</span><span class="sxs-lookup"><span data-stu-id="aed17-130">toowork around this error, you must [download hello executable from here](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) tooa location like **C:\WinUtils\bin**.</span></span> <span data-ttu-id="aed17-131">Majd adjon hozzá egy környezeti változó **HADOOP_HOME** és hello hello változó értékének túl**C\WinUtils**.</span><span class="sxs-lookup"><span data-stu-id="aed17-131">You must then add an environment variable **HADOOP_HOME** and set hello value of hello variable too**C\WinUtils**.</span></span>

## <a name="step-1-create-an-azure-virtual-network"></a><span data-ttu-id="aed17-132">1. lépés: Az Azure virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="aed17-132">Step 1: Create an Azure Virtual Network</span></span>
<span data-ttu-id="aed17-133">Hello utasításokat kövesse az alábbi hivatkozások toocreate egy Azure virtuális hálózatra hello, és ellenőrizze a hello asztal és az Azure Virtual Network hello összekapcsolását.</span><span class="sxs-lookup"><span data-stu-id="aed17-133">Follow hello instructions from hello below links toocreate an Azure Virtual Network and then verify hello connectivity between hello desktop and Azure Virtual Network.</span></span>

* [<span data-ttu-id="aed17-134">VNet létrehozása az Azure portál használatával pont-pont VPN-kapcsolattal</span><span class="sxs-lookup"><span data-stu-id="aed17-134">Create a VNet with a site-to-site VPN connection using Azure Portal</span></span>](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)
* [<span data-ttu-id="aed17-135">VNet létrehozása a PowerShell használatával pont-pont VPN-kapcsolattal</span><span class="sxs-lookup"><span data-stu-id="aed17-135">Create a VNet with a site-to-site VPN connection using PowerShell</span></span>](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)
* [<span data-ttu-id="aed17-136">Egy pont – hely kapcsolat tooa virtuális hálózatnak a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="aed17-136">Configure a point-to-site connection tooa virtual network using PowerShell</span></span>](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

## <a name="step-2-create-an-hdinsight-spark-cluster"></a><span data-ttu-id="aed17-137">2. lépés: Egy HDInsight Spark-fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="aed17-137">Step 2: Create an HDInsight Spark cluster</span></span>
<span data-ttu-id="aed17-138">Apache Spark-fürt hello létrehozott Azure virtuális hálózat részét képező Azure hdinsight is készítsen.</span><span class="sxs-lookup"><span data-stu-id="aed17-138">You should also create an Apache Spark cluster on Azure HDInsight that is part of hello Azure Virtual Network that you created.</span></span> <span data-ttu-id="aed17-139">Használja a rendelkezésre álló hello információ [hdinsight létrehozása Linux-alapú fürtökön](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="aed17-139">Use hello information available at [Create Linux-based clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span></span> <span data-ttu-id="aed17-140">Választható konfiguráció részeként hello hello előző lépésben létrehozott Azure virtuális hálózat kiválasztása</span><span class="sxs-lookup"><span data-stu-id="aed17-140">As part of optional configuration, select hello Azure Virtual Network that you created in hello previous step.</span></span>

## <a name="step-3-verify-hello-connectivity-between-hello-cluster-headnode-and-your-desktop"></a><span data-ttu-id="aed17-141">3. lépés: Hello fürt headnode és az asztal hello összekapcsolását ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="aed17-141">Step 3: Verify hello connectivity between hello cluster headnode and your desktop</span></span>
1. <span data-ttu-id="aed17-142">Első hello headnode hello IP-címét.</span><span class="sxs-lookup"><span data-stu-id="aed17-142">Get hello IP address of hello headnode.</span></span> <span data-ttu-id="aed17-143">Ambari felhasználói felületének megnyitásához hello fürthöz.</span><span class="sxs-lookup"><span data-stu-id="aed17-143">Open Ambari UI for hello cluster.</span></span> <span data-ttu-id="aed17-144">Hello-fürt panelén kattintson **irányítópult**.</span><span class="sxs-lookup"><span data-stu-id="aed17-144">From hello cluster blade, click **Dashboard**.</span></span>

    ![Headnode IP-cím keresése](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/launch-ambari-ui.png)
2. <span data-ttu-id="aed17-146">A hello Ambari felhasználói felületén, a hello jobb felső sarokban, kattintson az **állomások**.</span><span class="sxs-lookup"><span data-stu-id="aed17-146">From hello Ambari UI, from hello top-right corner, click **Hosts**.</span></span>

    ![Headnode IP-cím keresése](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/ambari-hosts.png)
3. <span data-ttu-id="aed17-148">Headnodes, a feldolgozó csomópontok és a zookeeper csomópontok listáját kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="aed17-148">You should see a list of headnodes, worker nodes, and zookeeper nodes.</span></span> <span data-ttu-id="aed17-149">hello headnodes rendelkezik hello **hn*** előtag.</span><span class="sxs-lookup"><span data-stu-id="aed17-149">hello headnodes have hello **hn*** prefix.</span></span> <span data-ttu-id="aed17-150">Kattintson az első headnode hello.</span><span class="sxs-lookup"><span data-stu-id="aed17-150">Click hello first headnode.</span></span>

    ![Headnode IP-cím keresése](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/cluster-headnodes.png)
4. <span data-ttu-id="aed17-152">Hello lap nyílik meg, hello hello alján **összegzés** másolási hello IP-címe hello headnode és hello állomás neve mezőben.</span><span class="sxs-lookup"><span data-stu-id="aed17-152">At hello bottom of hello page that opens, from hello **Summary** box, copy hello IP address of hello headnode and hello host name.</span></span>

    ![Headnode IP-cím keresése](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/headnode-ip-address.png)
5. <span data-ttu-id="aed17-154">Hello IP-cím és hello állomásnevét hello headnode toohello **állomások** hello számítógépen, amelyen szeretné, hogy toorun, és távolról debug hello Spark feladatok a fájlt.</span><span class="sxs-lookup"><span data-stu-id="aed17-154">Include hello IP address and hello host name of hello headnode toohello **hosts** file on hello computer from where you want toorun and remotely debug hello Spark jobs.</span></span> <span data-ttu-id="aed17-155">Ez lehetővé teszi az toocommunicate a hello headnode hello IP-címet, valamint a hello állomásnév használatával.</span><span class="sxs-lookup"><span data-stu-id="aed17-155">This will enable you toocommunicate with hello headnode using hello IP address as well as hello hostname.</span></span>

   1. <span data-ttu-id="aed17-156">Nyissa meg a Jegyzettömbben emelt szintű engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="aed17-156">Open a notepad with elevated permissions.</span></span> <span data-ttu-id="aed17-157">Hello fájl menüben kattintson a **nyitott** , majd lépjen a toohello hello hosts fájl helyét.</span><span class="sxs-lookup"><span data-stu-id="aed17-157">From hello file menu, click **Open** and then navigate toohello location of hello hosts file.</span></span> <span data-ttu-id="aed17-158">A Windows-számítógépen van `C:\Windows\System32\Drivers\etc\hosts`.</span><span class="sxs-lookup"><span data-stu-id="aed17-158">On a Windows computer, it is `C:\Windows\System32\Drivers\etc\hosts`.</span></span>
   2. <span data-ttu-id="aed17-159">Adja hozzá a következő toohello hello **állomások** fájlt.</span><span class="sxs-lookup"><span data-stu-id="aed17-159">Add hello following toohello **hosts** file.</span></span>

           # For headnode0
           192.xxx.xx.xx hn0-nitinp
           192.xxx.xx.xx hn0-nitinp.lhwwghjkpqejawpqbwcdyp3.gx.internal.cloudapp.net

           # For headnode1
           192.xxx.xx.xx hn1-nitinp
           192.xxx.xx.xx hn1-nitinp.lhwwghjkpqejawpqbwcdyp3.gx.internal.cloudapp.net
6. <span data-ttu-id="aed17-160">Számítógépről hello toohello hello HDInsight-fürt által használt Azure-beli virtuális hálózatra csatlakozó győződjön meg arról, hogy pingelhető mindkét hello headnodes hello IP-címet, valamint a hello állomásnév használatával.</span><span class="sxs-lookup"><span data-stu-id="aed17-160">From hello computer that you connected toohello Azure Virtual Network that is used by hello HDInsight cluster, verify that you can ping both hello headnodes using hello IP address as well as hello hostname.</span></span>
7. <span data-ttu-id="aed17-161">SSH-ból: hello utasításokat követve hello fürt headnode [Connect tooan HDInsight-fürtjéhez SSH](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="aed17-161">SSH into hello cluster headnode using hello instructions at [Connect tooan HDInsight cluster using SSH](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span> <span data-ttu-id="aed17-162">Hello fürt headnode, a ping hello asztali számítógép hello IP-címét.</span><span class="sxs-lookup"><span data-stu-id="aed17-162">From hello cluster headnode, ping hello IP address of hello desktop computer.</span></span> <span data-ttu-id="aed17-163">Kapcsolat tooboth hello rendelt IP-címekre toohello számítógép, egy a hello hálózati kapcsolatot tesztelje, és az Azure Virtual Network számítógép hello hello hello csatlakozik-e.</span><span class="sxs-lookup"><span data-stu-id="aed17-163">You should test connectivity tooboth hello IP addresses assigned toohello computer, one for hello network connection and hello other for hello Azure Virtual Network that hello computer is connected to.</span></span>
8. <span data-ttu-id="aed17-164">Ismételje hello hello más headnode is.</span><span class="sxs-lookup"><span data-stu-id="aed17-164">Repeat hello steps for hello other headnode as well.</span></span>

## <a name="step-4-create-a-spark-scala-application-using-hello-hdinsight-tools-in-azure-toolkit-for-intellij-and-configure-it-for-remote-debugging"></a><span data-ttu-id="aed17-165">4. lépés: Hello HDInsight Tools for IntelliJ az Azure-eszközkészlet használatával Spark Scala-alkalmazás létrehozása, és konfigurálja a távoli hibakeresés</span><span class="sxs-lookup"><span data-stu-id="aed17-165">Step 4: Create a Spark Scala application using hello HDInsight Tools in Azure Toolkit for IntelliJ and configure it for remote debugging</span></span>
1. <span data-ttu-id="aed17-166">Indítsa el az IntelliJ IDEA, és hozzon létre egy új projektet.</span><span class="sxs-lookup"><span data-stu-id="aed17-166">Launch IntelliJ IDEA and create a new project.</span></span> <span data-ttu-id="aed17-167">Hello új projekt párbeszédpanel, győződjön meg a következő lehetőségek hello, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="aed17-167">In hello new project dialog box, make hello following choices, and then click **Next**.</span></span>

    ![Spark Scala-alkalmazás létrehozása](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/create-hdi-scala-app.png)

   * <span data-ttu-id="aed17-169">Hello bal oldali ablaktáblában jelölje ki a **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="aed17-169">From hello left pane, select **HDInsight**.</span></span>
   * <span data-ttu-id="aed17-170">Hello jobb oldali ablaktáblában jelölje ki **a Spark on HDInsight (Scala)**.</span><span class="sxs-lookup"><span data-stu-id="aed17-170">From hello right pane, select **Spark on HDInsight (Scala)**.</span></span>
   * <span data-ttu-id="aed17-171">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="aed17-171">Click **Next**.</span></span>
2. <span data-ttu-id="aed17-172">Hello következő ablakban adja meg a projekt részleteit a következő hello, és kattintson a **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="aed17-172">In hello next window, provide hello following project details, and then click **Finish**.</span></span>  
   - <span data-ttu-id="aed17-173">Adja meg a projekt nevét és a projekt helyére.</span><span class="sxs-lookup"><span data-stu-id="aed17-173">Provide a project name and project location.</span></span>
   - <span data-ttu-id="aed17-174">A **projekt SDK**, használja a Java 1.8 spark 2.x fürthöz, a spark-fürt 1.x 1.7 Java.</span><span class="sxs-lookup"><span data-stu-id="aed17-174">For **Project SDK**, Use Java 1.8 for spark 2.x cluster, Java 1.7 for spark 1.x cluster.</span></span>
   - <span data-ttu-id="aed17-175">A **Spark verzió**, Scala-projekt létrehozása varázsló Spark SDK és Scala SDK integrálja a megfelelő verzióját.</span><span class="sxs-lookup"><span data-stu-id="aed17-175">For **Spark Version**, Scala project creation wizard integrates proper version for Spark SDK and Scala SDK.</span></span> <span data-ttu-id="aed17-176">Ha hello spark-fürt verziószáma alacsonyabb 2.0, válassza a spark 1.x.</span><span class="sxs-lookup"><span data-stu-id="aed17-176">If hello spark cluster version is lower 2.0, choose spark 1.x.</span></span> <span data-ttu-id="aed17-177">Ellenkező esetben spark2.x kell választania.</span><span class="sxs-lookup"><span data-stu-id="aed17-177">Otherwise, you should select spark2.x.</span></span> <span data-ttu-id="aed17-178">A példa Spark2.0.2 (Scala 2.11.8).</span><span class="sxs-lookup"><span data-stu-id="aed17-178">This example uses Spark2.0.2(Scala 2.11.8).</span></span>
       <span data-ttu-id="aed17-179">![Spark Scala-alkalmazás létrehozása](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/hdi-scala-project-details.png)</span><span class="sxs-lookup"><span data-stu-id="aed17-179">![Create Spark Scala application](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/hdi-scala-project-details.png)</span></span>
  
3. <span data-ttu-id="aed17-180">hello Spark projekt automatikusan összetevő hozza létre.</span><span class="sxs-lookup"><span data-stu-id="aed17-180">hello Spark project will automatically create an artifact for you.</span></span> <span data-ttu-id="aed17-181">toosee hello összetevő, kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="aed17-181">toosee hello artifact, follow these steps.</span></span>

   1. <span data-ttu-id="aed17-182">A hello **fájl** menüben kattintson a **szerkezetének**.</span><span class="sxs-lookup"><span data-stu-id="aed17-182">From hello **File** menu, click **Project Structure**.</span></span>
   2. <span data-ttu-id="aed17-183">A hello **szerkezetének** párbeszédpanel, kattintson a **összetevők** toosee hello alapértelmezett összetevő, amely jön létre.</span><span class="sxs-lookup"><span data-stu-id="aed17-183">In hello **Project Structure** dialog box, click **Artifacts** toosee hello default artifact that is created.</span></span>
   <span data-ttu-id="aed17-184">![Hozzon létre JAR](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/default-artifact.png)</span><span class="sxs-lookup"><span data-stu-id="aed17-184">![Create JAR](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/default-artifact.png)</span></span>

      <span data-ttu-id="aed17-185">Is létrehozhat saját összetevő hello kattintva bly  **+**  ikonra, a fenti kép hello kiemelve.</span><span class="sxs-lookup"><span data-stu-id="aed17-185">You can also create your own artifact bly clicking on hello **+** icon, highlighted in hello image above.</span></span>

4. <span data-ttu-id="aed17-186">Szalagtárak tooyour projekt hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="aed17-186">Add libraries tooyour project.</span></span> <span data-ttu-id="aed17-187">a szalagtár tooadd kattintson a jobb gombbal a hello projekt neve hello projekt elemére, és kattintson **beállítások modul megnyitása**.</span><span class="sxs-lookup"><span data-stu-id="aed17-187">tooadd a library, right-click hello project name in hello project tree, and then click **Open Module Settings**.</span></span> <span data-ttu-id="aed17-188">A hello **szerkezetének** párbeszédpanelen hello bal oldali ablaktáblában kattintson a **szalagtárak**, kattintson a szimbólum hello (+), majd **a Maven**.</span><span class="sxs-lookup"><span data-stu-id="aed17-188">In hello **Project Structure** dialog box, from hello left pane, click **Libraries**, click hello (+) symbol, and then click **From Maven**.</span></span>

    ![Könyvtár hozzáadása](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/add-library.png)

    <span data-ttu-id="aed17-190">A hello **letöltése könyvtár Maven tárházból** párbeszédpanel mezőben keressen, és adja hozzá a következő könyvtárak hello.</span><span class="sxs-lookup"><span data-stu-id="aed17-190">In hello **Download Library from Maven Repository** dialog box, search and add hello following libraries.</span></span>

   * `org.scalatest:scalatest_2.10:2.2.1`
   * `org.apache.hadoop:hadoop-azure:2.7.1`
5. <span data-ttu-id="aed17-191">Másolás `yarn-site.xml` és `core-site.xml` hello a fürt headnode és toohello projekt hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="aed17-191">Copy `yarn-site.xml` and `core-site.xml` from hello cluster headnode and add it toohello project.</span></span> <span data-ttu-id="aed17-192">Használja a következő parancsok toocopy hello fájlok hello.</span><span class="sxs-lookup"><span data-stu-id="aed17-192">Use hello following commands toocopy hello files.</span></span> <span data-ttu-id="aed17-193">Használhat [Cygwin](https://cygwin.com/install.html) toorun hello következő `scp` toocopy hello fájlok hello fürt headnodes parancsokat.</span><span class="sxs-lookup"><span data-stu-id="aed17-193">You can use [Cygwin](https://cygwin.com/install.html) toorun hello following `scp` commands toocopy hello files from hello cluster headnodes.</span></span>

        scp <ssh user name>@<headnode IP address or host name>://etc/hadoop/conf/core-site.xml .

    <span data-ttu-id="aed17-194">Már hozzáadott hello fürt headnode IP cím és állomásnevekkel fő hello gazdafájl hello asztali, mert hello használhatjuk **scp** hello módon a következő parancsokat.</span><span class="sxs-lookup"><span data-stu-id="aed17-194">Because we already added hello cluster headnode IP address and hostnames fo hello hosts file on hello desktop, we can use hello **scp** commands in hello following manner.</span></span>

        scp sshuser@hn0-nitinp:/etc/hadoop/conf/core-site.xml .
        scp sshuser@hn0-nitinp:/etc/hadoop/conf/yarn-site.xml .

    <span data-ttu-id="aed17-195">Ezen fájlok tooyour projekt hozzáadása a hello másolásával **/src** mappa a projekt csomópontjára, például `<your project directory>\src`.</span><span class="sxs-lookup"><span data-stu-id="aed17-195">Add these files tooyour project by copying them under hello **/src** folder in your project tree, for example `<your project directory>\src`.</span></span>
6. <span data-ttu-id="aed17-196">Frissítés hello `core-site.xml` toomake hello a következő módosításokat.</span><span class="sxs-lookup"><span data-stu-id="aed17-196">Update hello `core-site.xml` toomake hello following changes.</span></span>

   1. <span data-ttu-id="aed17-197">`core-site.xml`hello titkosított kulcs toohello tárfiók hello-fürthöz tartozó tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="aed17-197">`core-site.xml` includes hello encrypted key toohello storage account associated with hello cluster.</span></span> <span data-ttu-id="aed17-198">A hello `core-site.xml` toohello projekt, csere hello titkosított kulcs hello tényleges biztonságitár-kulcs társított hello alapértelmezett tárfiók hozzáadásának.</span><span class="sxs-lookup"><span data-stu-id="aed17-198">In hello `core-site.xml` that you added toohello project, replace hello encrypted key with hello actual storage key associated with hello default storage account.</span></span> <span data-ttu-id="aed17-199">Lásd: [a tárelérési kulcsok kezelése](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span><span class="sxs-lookup"><span data-stu-id="aed17-199">See [Manage your storage access keys](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span>

           <property>
                 <name>fs.azure.account.key.hdistoragecentral.blob.core.windows.net</name>
                 <value>access-key-associated-with-the-account</value>
           </property>
   2. <span data-ttu-id="aed17-200">Távolítsa el a bejegyzések követően – hello hello `core-site.xml`.</span><span class="sxs-lookup"><span data-stu-id="aed17-200">Remove hello following entries from hello `core-site.xml`.</span></span>

           <property>
                 <name>fs.azure.account.keyprovider.hdistoragecentral.blob.core.windows.net</name>
                 <value>org.apache.hadoop.fs.azure.ShellDecryptionKeyProvider</value>
           </property>

           <property>
                 <name>fs.azure.shellkeyprovider.script</name>
                 <value>/usr/lib/python2.7/dist-packages/hdinsight_common/decrypt.sh</value>
           </property>

           <property>
                 <name>net.topology.script.file.name</name>
                 <value>/etc/hadoop/conf/topology_script.py</value>
           </property>
   3. <span data-ttu-id="aed17-201">Hello fájl mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="aed17-201">Save hello file.</span></span>
7. <span data-ttu-id="aed17-202">Adja hozzá az alkalmazás hello fő osztály.</span><span class="sxs-lookup"><span data-stu-id="aed17-202">Add hello Main class for your application.</span></span> <span data-ttu-id="aed17-203">A hello **Project Explorer**, kattintson a jobb gombbal **src**, pont túl**új**, és kattintson a **Scala osztály**.</span><span class="sxs-lookup"><span data-stu-id="aed17-203">From hello **Project Explorer**, right-click **src**, point too**New**, and then click **Scala class**.</span></span>

    ![Forráskód hozzáadása](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/hdi-spark-scala-code.png)
8. <span data-ttu-id="aed17-205">A hello **új Scala osztály létrehozása** párbeszédpanelen adja meg egy nevet, **jellegű** kiválasztása **objektum**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="aed17-205">In hello **Create New Scala Class** dialog box, provide a name, for **Kind** select **Object**, and then click **OK**.</span></span>

    ![Forráskód hozzáadása](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/hdi-spark-scala-code-object.png)
9. <span data-ttu-id="aed17-207">A hello `MyClusterAppMain.scala` fájlt, illessze be a kódját a következő hello.</span><span class="sxs-lookup"><span data-stu-id="aed17-207">In hello `MyClusterAppMain.scala` file, paste hello following code.</span></span> <span data-ttu-id="aed17-208">Ez a kód hello Spark-környezetet hoz létre, és elindítja egy `executeJob` hello metódusnak `SparkSample` objektum.</span><span class="sxs-lookup"><span data-stu-id="aed17-208">This code creates hello Spark context and launches an `executeJob` method from hello `SparkSample` object.</span></span>

        import org.apache.spark.{SparkConf, SparkContext}

        object SparkSampleMain {
          def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("SparkSample")
                                      .set("spark.hadoop.validateOutputSpecs", "false")
            val sc = new SparkContext(conf)

            SparkSample.executeJob(sc,
                                   "wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv",
                                   "wasb:///HVACOut")
          }
        }

10. <span data-ttu-id="aed17-209">Ismételje meg a 8. és 9 fent egy új Scala objektum nevű tooadd `SparkSample`.</span><span class="sxs-lookup"><span data-stu-id="aed17-209">Repeat steps 8 and 9 above tooadd a new Scala object called `SparkSample`.</span></span> <span data-ttu-id="aed17-210">toothis osztály adja hozzá a következő kód hello.</span><span class="sxs-lookup"><span data-stu-id="aed17-210">toothis class add hello following code.</span></span> <span data-ttu-id="aed17-211">Ez a kód hello adatokat olvas hello HVAC.csv (az összes HDInsight Spark-fürtjei elérhető), hello tartalmazó sorok csak egy számjegy hello CSV hetedik oszlopban hello lekéri és hello kimenetet túl írja**/HVACOut** hello alapértelmezés szerint a tároló hello fürthöz.</span><span class="sxs-lookup"><span data-stu-id="aed17-211">This code reads hello data from hello HVAC.csv (available on all HDInsight Spark clusters), retrieves hello rows that only have one digit in hello seventh column in hello CSV, and writes hello output too**/HVACOut** under hello default storage container for hello cluster.</span></span>

        import org.apache.spark.SparkContext

        object SparkSample {
         def executeJob (sc: SparkContext, input: String, output: String): Unit = {
           val rdd = sc.textFile(input)

           //find hello rows which have only one digit in hello 7th column in hello CSV
           val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)

           val s = sc.parallelize(rdd.take(5)).cartesian(rdd).count()
           println(s)

           rdd1.saveAsTextFile(output)
           //rdd1.collect().foreach(println)
         }
        }
11. <span data-ttu-id="aed17-212">Ismételje meg a 8. és 9 fent egy új osztályt nevű tooadd `RemoteClusterDebugging`.</span><span class="sxs-lookup"><span data-stu-id="aed17-212">Repeat steps 8 and 9 above tooadd a new class called `RemoteClusterDebugging`.</span></span> <span data-ttu-id="aed17-213">Ez az osztály hello Spark keretrendszeréhez alkalmazások hibakereséshez használt valósítja meg.</span><span class="sxs-lookup"><span data-stu-id="aed17-213">This class implements hello Spark test framework that is used for debugging applications.</span></span> <span data-ttu-id="aed17-214">Adja hozzá a következő kód toohello hello `RemoteClusterDebugging` osztály.</span><span class="sxs-lookup"><span data-stu-id="aed17-214">Add hello following code toohello `RemoteClusterDebugging` class.</span></span>

        import org.apache.spark.{SparkConf, SparkContext}
        import org.scalatest.FunSuite

        class RemoteClusterDebugging extends FunSuite {

         test("Remote run") {
           val conf = new SparkConf().setAppName("SparkSample")
                                     .setMaster("yarn-client")
                                     .set("spark.yarn.am.extraJavaOptions", "-Dhdp.version=2.4")
                                     .set("spark.yarn.jar", "wasb:///hdp/apps/2.4.2.0-258/spark-assembly-1.6.1.2.4.2.0-258-hadoop2.7.1.2.4.2.0-258.jar")
                                     .setJars(Seq("""C:\workspace\IdeaProjects\MyClusterApp\out\artifacts\MyClusterApp_DefaultArtifact\default_artifact.jar"""))
                                     .set("spark.hadoop.validateOutputSpecs", "false")
           val sc = new SparkContext(conf)

           SparkSample.executeJob(sc,
             "wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv",
             "wasb:///HVACOut")
         }
        }

     <span data-ttu-id="aed17-215">Néhány fontos dolgot toonote itt:</span><span class="sxs-lookup"><span data-stu-id="aed17-215">Couple of important things toonote here:</span></span>

   * <span data-ttu-id="aed17-216">A `.set("spark.yarn.jar", "wasb:///hdp/apps/2.4.2.0-258/spark-assembly-1.6.1.2.4.2.0-258-hadoop2.7.1.2.4.2.0-258.jar")`, győződjön meg arról, Spark szerelvény JAR hello hello fürttároló hello megadott elérési úton található.</span><span class="sxs-lookup"><span data-stu-id="aed17-216">For `.set("spark.yarn.jar", "wasb:///hdp/apps/2.4.2.0-258/spark-assembly-1.6.1.2.4.2.0-258-hadoop2.7.1.2.4.2.0-258.jar")`, make sure hello Spark assembly JAR is available on hello cluster storage at hello specified path.</span></span>
   * <span data-ttu-id="aed17-217">A `setJars`, ahol létrejön hello összetevő jar hello helyének megadása.</span><span class="sxs-lookup"><span data-stu-id="aed17-217">For `setJars`, specify hello location where hello artifact jar will be created.</span></span> <span data-ttu-id="aed17-218">Általában akkor `<Your IntelliJ project directory>\out\<project name>_DefaultArtifact\default_artifact.jar`.</span><span class="sxs-lookup"><span data-stu-id="aed17-218">Typically it is `<Your IntelliJ project directory>\out\<project name>_DefaultArtifact\default_artifact.jar`.</span></span>
12. <span data-ttu-id="aed17-219">A hello `RemoteClusterDebugging` osztály, kattintson a jobb gombbal a hello `test` kulcsszót, és válassza **létrehozása RemoteClusterDebugging konfigurációs**.</span><span class="sxs-lookup"><span data-stu-id="aed17-219">In hello `RemoteClusterDebugging` class, right-click hello `test` keyword and select **Create RemoteClusterDebugging Configuration**.</span></span>

    ![A távoli konfiguráció létrehozása](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/create-remote-config.png)

13. <span data-ttu-id="aed17-221">A hello párbeszédpanelen adjon meg egy nevet hello konfigurációs, és válassza a hello **jellegű tesztelése** , **teszt neve**.</span><span class="sxs-lookup"><span data-stu-id="aed17-221">In hello dialog box, provide a name for hello configuration, and select hello **Test kind** as **Test name**.</span></span> <span data-ttu-id="aed17-222">Minden más értéket alapértelmezett hagyja, kattintson a **alkalmaz**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="aed17-222">Leave all other values as default, click **Apply**, and then click **OK**.</span></span>

    ![A távoli konfiguráció létrehozása](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/provide-config-value.png)
14. <span data-ttu-id="aed17-224">Ekkor megjelenik egy **távoli Futtatás** konfigurációs hello menüsávon legördülő.</span><span class="sxs-lookup"><span data-stu-id="aed17-224">You should now see a **Remote Run** configuration drop-down in hello menu bar.</span></span>

    ![A távoli konfiguráció létrehozása](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/config-run.png)

## <a name="step-5-run-hello-application-in-debug-mode"></a><span data-ttu-id="aed17-226">5. lépés: A hibakeresési módban hello alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="aed17-226">Step 5: Run hello application in debug mode</span></span>
1. <span data-ttu-id="aed17-227">Az IntelliJ IDEA projektben nyissa meg `SparkSample.scala` , és hozzon létre egy töréspontot következő too'val rdd1 ".</span><span class="sxs-lookup"><span data-stu-id="aed17-227">In your IntelliJ IDEA project, open `SparkSample.scala` and create a breakpoint next too\`val rdd1'.</span></span> <span data-ttu-id="aed17-228">Hello előugró menüben töréspont létrehozásához válasszon ki **függvény executeJob sor**.</span><span class="sxs-lookup"><span data-stu-id="aed17-228">In hello pop-up menu for creating a breakpoint, select **line in function executeJob**.</span></span>

    ![Töréspont](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/create-breakpoint.png)
2. <span data-ttu-id="aed17-230">Hello kattintson **Debug futtatása** gomb következő toohello **távoli Futtatás** konfigurációs legördülő toostart hello alkalmazást futtat.</span><span class="sxs-lookup"><span data-stu-id="aed17-230">Click hello **Debug Run** button next toohello **Remote Run** configuration drop-down toostart running hello application.</span></span>

    ![Hello programfuttatást hibakeresési módban](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-run-mode.png)
3. <span data-ttu-id="aed17-232">Hello töréspont hello program végrehajtását elérésekor kell megjelennie a **hibakereső** hello alsó lapját.</span><span class="sxs-lookup"><span data-stu-id="aed17-232">When hello program execution reaches hello breakpoint, you should see a **Debugger** tab in hello bottom pane.</span></span>

    ![Hello programfuttatást hibakeresési módban](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-add-watch.png)
4. <span data-ttu-id="aed17-234">Kattintson a hello (**+**) ikonra tooadd a figyelés, az alábbi hello ábrán látható módon.</span><span class="sxs-lookup"><span data-stu-id="aed17-234">Click hello (**+**) icon tooadd a watch as shown in hello image below.</span></span>

    ![Hello programfuttatást hibakeresési módban](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-add-watch-variable.png)

    <span data-ttu-id="aed17-236">Itt, mert hello alkalmazás túllépte a hello változó előtt `rdd1` lett létrehozva, mi van hello hello változóban első 5 sorok láthatja a figyelési `rdd`.</span><span class="sxs-lookup"><span data-stu-id="aed17-236">Here, because hello application broke before hello variable `rdd1` was created, using this watch we can see what are hello first 5 rows in hello variable `rdd`.</span></span> <span data-ttu-id="aed17-237">Nyomja le az **ENTER** billentyűt.</span><span class="sxs-lookup"><span data-stu-id="aed17-237">Press **ENTER**.</span></span>

    ![Hello programfuttatást hibakeresési módban](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-add-watch-variable-value.png)

    <span data-ttu-id="aed17-239">A fenti kép hello lásd az, hogy futásidőben, akkor lekérdezhet terrabytes adatok és a hibakeresési módját az alkalmazás időtartamára.</span><span class="sxs-lookup"><span data-stu-id="aed17-239">What you see in hello image above is that at runtime, you could query terrabytes of data and debug how your application progresses.</span></span> <span data-ttu-id="aed17-240">Például a hello kimeneti fenti hello ábrán látható, megtekintheti, hogy hello első sorát hello kimeneti fejléc.</span><span class="sxs-lookup"><span data-stu-id="aed17-240">For example, in hello output shown in hello image above, you can see that hello first row of hello output is a header.</span></span> <span data-ttu-id="aed17-241">Ennek alapján módosíthatja az alkalmazás kódja tooskip hello fejlécsor szükség esetén.</span><span class="sxs-lookup"><span data-stu-id="aed17-241">Based on this, you can modify your application code tooskip hello header row if required.</span></span>
5. <span data-ttu-id="aed17-242">Most kattintson hello **folytatása Program** ikon tooproceed futtassa az alkalmazással.</span><span class="sxs-lookup"><span data-stu-id="aed17-242">You can now click hello **Resume Program** icon tooproceed with your application run.</span></span>

    ![Hello programfuttatást hibakeresési módban](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-continue-run.png)
6. <span data-ttu-id="aed17-244">Ha hello alkalmazás sikeresen befejeződött, hello hasonló kimenetnek kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="aed17-244">If hello application completes successfully, you should see an output like hello following.</span></span>

    ![Hello programfuttatást hibakeresési módban](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-complete.png)

## <span data-ttu-id="aed17-246"><a name="seealso"></a>Lásd még:</span><span class="sxs-lookup"><span data-stu-id="aed17-246"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="aed17-247">Overview: Apache Spark on Azure HDInsight (Áttekintés: Apache Spark on Azure HDInsight)</span><span class="sxs-lookup"><span data-stu-id="aed17-247">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="demo"></a><span data-ttu-id="aed17-248">Bemutató</span><span class="sxs-lookup"><span data-stu-id="aed17-248">Demo</span></span>
* <span data-ttu-id="aed17-249">(Videó) Scala-projekt létrehozása: [Spark Scala-alkalmazások létrehozása](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)</span><span class="sxs-lookup"><span data-stu-id="aed17-249">Create Scala Project (Video): [Create Spark Scala Applications](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)</span></span>
* <span data-ttu-id="aed17-250">Távoli hibakeresési (videó): [IntelliJ toodebug Spark-alkalmazások távolról a HDInsight-fürt használata Azure eszköztára](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)</span><span class="sxs-lookup"><span data-stu-id="aed17-250">Remote Debug (Video): [Use Azure Toolkit for IntelliJ toodebug Spark applications remotely on HDInsight Cluster](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)</span></span>

### <a name="scenarios"></a><span data-ttu-id="aed17-251">Forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="aed17-251">Scenarios</span></span>
* [<span data-ttu-id="aed17-252">Spark és BI: Interaktív adatelemzés végrehajtása a Spark on HDInsight használatával, BI-eszközökkel</span><span class="sxs-lookup"><span data-stu-id="aed17-252">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="aed17-253">Spark és Machine Learning: A Spark on HDInsight használata az épület-hőmérséklet elemzésére HVAC-adatok alapján</span><span class="sxs-lookup"><span data-stu-id="aed17-253">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="aed17-254">Spark és Machine Learning: használja a Spark on HDInsight toopredict élelmiszervizsgálati eredmények</span><span class="sxs-lookup"><span data-stu-id="aed17-254">Spark with Machine Learning: Use Spark in HDInsight toopredict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="aed17-255">Spark Streaming: A Spark on HDInsight használata valós idejű streamelési alkalmazások összeállítására</span><span class="sxs-lookup"><span data-stu-id="aed17-255">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="aed17-256">A webhelynapló elemzése a Spark on HDInsight használatával</span><span class="sxs-lookup"><span data-stu-id="aed17-256">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="aed17-257">Alkalmazások létrehozása és futtatása</span><span class="sxs-lookup"><span data-stu-id="aed17-257">Create and run applications</span></span>
* [<span data-ttu-id="aed17-258">Önálló alkalmazás létrehozása a Scala használatával</span><span class="sxs-lookup"><span data-stu-id="aed17-258">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="aed17-259">Feladatok távoli futtatása Spark-fürtön a Livy használatával</span><span class="sxs-lookup"><span data-stu-id="aed17-259">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="aed17-260">Eszközök és bővítmények</span><span class="sxs-lookup"><span data-stu-id="aed17-260">Tools and extensions</span></span>
* [<span data-ttu-id="aed17-261">Használata a HDInsight Tools Azure eszközkészlet IntelliJ toocreate, és küldje el a Spark Scala applicatons</span><span class="sxs-lookup"><span data-stu-id="aed17-261">Use HDInsight Tools in Azure Toolkit for IntelliJ toocreate and submit Spark Scala applicatons</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="aed17-262">Az IntelliJ toodebug Spark-alkalmazások SSH keresztül távolról használható Azure eszköztára</span><span class="sxs-lookup"><span data-stu-id="aed17-262">Use Azure Toolkit for IntelliJ toodebug Spark applications remotely through SSH</span></span>](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [<span data-ttu-id="aed17-263">Használja a HDInsight Tools for IntelliJ a Hortonworks védőfal</span><span class="sxs-lookup"><span data-stu-id="aed17-263">Use HDInsight Tools for IntelliJ with Hortonworks Sandbox</span></span>](hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [<span data-ttu-id="aed17-264">HDInsight-eszközök használata az Eclipse toocreate Spark-alkalmazások Azure eszköztára</span><span class="sxs-lookup"><span data-stu-id="aed17-264">Use HDInsight Tools in Azure Toolkit for Eclipse toocreate Spark applications</span></span>](hdinsight-apache-spark-eclipse-tool-plugin.md)
* [<span data-ttu-id="aed17-265">Zeppelin notebookok használata Spark-fürttel HDInsighton</span><span class="sxs-lookup"><span data-stu-id="aed17-265">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="aed17-266">Jupyter notebookokhoz elérhető kernelek a HDInsight Spark-fürtjében</span><span class="sxs-lookup"><span data-stu-id="aed17-266">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="aed17-267">Külső csomagok használata Jupyter notebookokkal</span><span class="sxs-lookup"><span data-stu-id="aed17-267">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="aed17-268">Jupyter telepítse a számítógépre, és csatlakozzon a HDInsight Spark-fürt tooan</span><span class="sxs-lookup"><span data-stu-id="aed17-268">Install Jupyter on your computer and connect tooan HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="aed17-269">Erőforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="aed17-269">Manage resources</span></span>
* [<span data-ttu-id="aed17-270">Az Azure HDInsight hello Apache Spark-fürt erőforrásainak kezelése</span><span class="sxs-lookup"><span data-stu-id="aed17-270">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="aed17-271">Apache Spark-fürtön futó feladatok nyomon követése és hibakeresése a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="aed17-271">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)
