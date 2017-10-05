---
title: "Az IntelliJ - hibakeresési alkalmazások távolról a HDInsight Spark az Azure eszköztára |} Microsoft Docs"
description: "Megtudhatja, hogyan használják az Azure-eszközkészlet a HDInsight Tools az IntelliJ HDInsight Spark futtatott távolról hibakeresési alkalmazásokkal fürtök VPN-en keresztül."
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
ms.openlocfilehash: 5ce282aac94d0f22ea587cbe4005819310e23b1f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="use-azure-toolkit-for-intellij-to-debug-applications-remotely-on-hdinsight-spark-through-vpn"></a><span data-ttu-id="4c9a5-103">Az intellij-t Azure eszközkészlet segítségével távolról a VPN-en keresztül a HDInsight Spark-alkalmazások hibakeresését</span><span class="sxs-lookup"><span data-stu-id="4c9a5-103">Use Azure Toolkit for IntelliJ to debug applications remotely on HDInsight Spark through VPN</span></span>

<span data-ttu-id="4c9a5-104">Azt javasoljuk, hogy a hibakeresési távolról a spark applicaltion ssh módja.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-104">We recommend the way of debugging spark applicaltion remotely through ssh.</span></span> <span data-ttu-id="4c9a5-105">Útmutatásért lásd: [távolról az IntelliJ SSH-n keresztül a Azure eszközkészlet a HDInsight-fürtök a Spark-alkalmazások hibakeresését](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh).</span><span class="sxs-lookup"><span data-stu-id="4c9a5-105">For instructions, see [Remotely debug Spark applications on an HDInsight cluster with Azure Toolkit for IntelliJ through SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh).</span></span>

<span data-ttu-id="4c9a5-106">Ez a cikk részletes útmutatást a HDInsight Tools használatával IntelliJ Azure eszköztára a Spark on HDInsight Spark-fürt a feladat elküldése, és ezután az asztali számítógépről távolról hibakeresési azt.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-106">This article provides step-by-step guidance on how to use the HDInsight Tools in Azure Toolkit for IntelliJ to submit a Spark job on HDInsight Spark cluster and then debug it remotely from your desktop computer.</span></span> <span data-ttu-id="4c9a5-107">Ehhez a következő magas szintű lépéseket kell végrehajtani:</span><span class="sxs-lookup"><span data-stu-id="4c9a5-107">To do so, you must perform the following high-level steps:</span></span>

1. <span data-ttu-id="4c9a5-108">Pont-pont vagy a pont-pont Azure virtuális hálózat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-108">Create a site-to-site or point-to-site Azure Virtual Network.</span></span> <span data-ttu-id="4c9a5-109">A jelen dokumentumban leírt lépések azt feltételezik, hogy a pont-pont hálózatot használ.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-109">The steps in this document assume that you use a site-to-site network.</span></span>
2. <span data-ttu-id="4c9a5-110">A pont-pont Azure virtuális hálózat részét képező Azure hdinsight Spark-fürt létrehozása.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-110">Create a Spark cluster in Azure HDInsight that is part of the site-to-site Azure Virtual Network.</span></span>
3. <span data-ttu-id="4c9a5-111">Ellenőrizze a fürt headnode és az asztal közötti kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-111">Verify the connectivity between the cluster headnode and your desktop.</span></span>
4. <span data-ttu-id="4c9a5-112">Az IntelliJ IDEA Scala-alkalmazás létrehozása, és konfigurálja a távoli hibakereséshez.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-112">Create a Scala application in IntelliJ IDEA and configure it for remote debugging.</span></span>
5. <span data-ttu-id="4c9a5-113">Futtassa, és az alkalmazás hibakeresése.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-113">Run and debug the application.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4c9a5-114">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="4c9a5-114">Prerequisites</span></span>
* <span data-ttu-id="4c9a5-115">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-115">An Azure subscription.</span></span> <span data-ttu-id="4c9a5-116">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="4c9a5-116">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="4c9a5-117">A HDInsight az Apache Spark-fürt.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-117">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="4c9a5-118">Útmutatásért lásd: [létrehozása az Apache Spark on Azure hdinsight clusters](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="4c9a5-118">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>
* <span data-ttu-id="4c9a5-119">Oracle Java fejlesztői készlet.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-119">Oracle Java Development kit.</span></span> <span data-ttu-id="4c9a5-120">A későbbiekben telepítheti az [Itt](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span><span class="sxs-lookup"><span data-stu-id="4c9a5-120">You can install it from [here](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span></span>
* <span data-ttu-id="4c9a5-121">IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-121">IntelliJ IDEA.</span></span> <span data-ttu-id="4c9a5-122">Ez a cikk 2017.1 verzióját használja.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-122">This article uses version 2017.1.</span></span> <span data-ttu-id="4c9a5-123">A későbbiekben telepítheti az [Itt](https://www.jetbrains.com/idea/download/).</span><span class="sxs-lookup"><span data-stu-id="4c9a5-123">You can install it from [here](https://www.jetbrains.com/idea/download/).</span></span>
* <span data-ttu-id="4c9a5-124">Az Azure eszköztára IntelliJ HDInsight eszközök.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-124">HDInsight Tools in Azure Toolkit for IntelliJ.</span></span> <span data-ttu-id="4c9a5-125">A HDInsight tools for IntelliJ érhetők el az intellij-t Azure eszköztára részeként.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-125">HDInsight tools for IntelliJ are available as part of the Azure Toolkit for IntelliJ.</span></span> <span data-ttu-id="4c9a5-126">Az Azure-eszközkészlet telepítése, lásd: [az intellij-t az Azure eszközkészlet telepítésével](../azure-toolkit-for-intellij-installation.md).</span><span class="sxs-lookup"><span data-stu-id="4c9a5-126">For instructions on how to install the Azure Toolkit, see [Installing the Azure Toolkit for IntelliJ](../azure-toolkit-for-intellij-installation.md).</span></span>
* <span data-ttu-id="4c9a5-127">Jelentkezzen be az Azure-előfizetéshez az IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-127">Log into your Azure Subscription from IntelliJ IDEA.</span></span> <span data-ttu-id="4c9a5-128">Kövesse az utasításokat [Itt](hdinsight-apache-spark-intellij-tool-plugin.md).</span><span class="sxs-lookup"><span data-stu-id="4c9a5-128">Follow the instructions [here](hdinsight-apache-spark-intellij-tool-plugin.md).</span></span>
* <span data-ttu-id="4c9a5-129">A távoli hibakereséshez a Windows rendszerű számítógépeken Spark Scala alkalmazás futtatásakor kaphat kivételt a [SPARK-2356](https://issues.apache.org/jira/browse/SPARK-2356) , amelyek miatt a Windows egy hiányzó WinUtils.exe következik be.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-129">While running Spark Scala application for remote debugging on a Windows computer, you might get an exception as explained in [SPARK-2356](https://issues.apache.org/jira/browse/SPARK-2356) that occurs due to a missing WinUtils.exe on Windows.</span></span> <span data-ttu-id="4c9a5-130">Ez a hiba megoldása érdekében kell [töltse le a végrehajtható fájl itt](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) egy olyan helyre, például a **C:\WinUtils\bin**.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-130">To work around this error, you must [download the executable from here](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) to a location like **C:\WinUtils\bin**.</span></span> <span data-ttu-id="4c9a5-131">Majd adjon hozzá egy környezeti változó **HADOOP_HOME** és a változó értékét állíthatja be **C\WinUtils**.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-131">You must then add an environment variable **HADOOP_HOME** and set the value of the variable to **C\WinUtils**.</span></span>

## <a name="step-1-create-an-azure-virtual-network"></a><span data-ttu-id="4c9a5-132">1. lépés: Az Azure virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="4c9a5-132">Step 1: Create an Azure Virtual Network</span></span>
<span data-ttu-id="4c9a5-133">Kövesse az utasításokat az alábbi hozzon létre egy Azure virtuális hálózatra, és ellenőrizze az asztal és az Azure virtuális hálózat közötti kapcsolat mutató hivatkozásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-133">Follow the instructions from the below links to create an Azure Virtual Network and then verify the connectivity between the desktop and Azure Virtual Network.</span></span>

* [<span data-ttu-id="4c9a5-134">VNet létrehozása az Azure portál használatával pont-pont VPN-kapcsolattal</span><span class="sxs-lookup"><span data-stu-id="4c9a5-134">Create a VNet with a site-to-site VPN connection using Azure Portal</span></span>](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)
* [<span data-ttu-id="4c9a5-135">VNet létrehozása a PowerShell használatával pont-pont VPN-kapcsolattal</span><span class="sxs-lookup"><span data-stu-id="4c9a5-135">Create a VNet with a site-to-site VPN connection using PowerShell</span></span>](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)
* [<span data-ttu-id="4c9a5-136">PowerShell virtuális hálózat egy pont – hely kapcsolat beállítása</span><span class="sxs-lookup"><span data-stu-id="4c9a5-136">Configure a point-to-site connection to a virtual network using PowerShell</span></span>](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

## <a name="step-2-create-an-hdinsight-spark-cluster"></a><span data-ttu-id="4c9a5-137">2. lépés: Egy HDInsight Spark-fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="4c9a5-137">Step 2: Create an HDInsight Spark cluster</span></span>
<span data-ttu-id="4c9a5-138">A létrehozott Azure virtuális hálózat részét képező Azure HDInsight is Apache Spark-fürt kell létrehoznia.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-138">You should also create an Apache Spark cluster on Azure HDInsight that is part of the Azure Virtual Network that you created.</span></span> <span data-ttu-id="4c9a5-139">Az információk rendelkezésre [hdinsight létrehozása Linux-alapú fürtökön](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="4c9a5-139">Use the information available at [Create Linux-based clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span></span> <span data-ttu-id="4c9a5-140">Választható konfiguráció részeként válassza ki az előző lépésben létrehozott Azure virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-140">As part of optional configuration, select the Azure Virtual Network that you created in the previous step.</span></span>

## <a name="step-3-verify-the-connectivity-between-the-cluster-headnode-and-your-desktop"></a><span data-ttu-id="4c9a5-141">3. lépés: Ellenőrizze a fürt headnode és az asztal közötti kapcsolat</span><span class="sxs-lookup"><span data-stu-id="4c9a5-141">Step 3: Verify the connectivity between the cluster headnode and your desktop</span></span>
1. <span data-ttu-id="4c9a5-142">Az IP-címét a headnode beolvasása.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-142">Get the IP address of the headnode.</span></span> <span data-ttu-id="4c9a5-143">Ambari felhasználói felületének megnyitásához a fürthöz.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-143">Open Ambari UI for the cluster.</span></span> <span data-ttu-id="4c9a5-144">A fürt paneljén kattintson **irányítópult**.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-144">From the cluster blade, click **Dashboard**.</span></span>

    ![Headnode IP-cím keresése](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/launch-ambari-ui.png)
2. <span data-ttu-id="4c9a5-146">Az Ambari felhasználói felülete, a jobb felső sarokban kattintson **állomások**.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-146">From the Ambari UI, from the top-right corner, click **Hosts**.</span></span>

    ![Headnode IP-cím keresése](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/ambari-hosts.png)
3. <span data-ttu-id="4c9a5-148">Headnodes, a feldolgozó csomópontok és a zookeeper csomópontok listáját kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-148">You should see a list of headnodes, worker nodes, and zookeeper nodes.</span></span> <span data-ttu-id="4c9a5-149">A headnodes rendelkezik a **hn*** előtag.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-149">The headnodes have the **hn*** prefix.</span></span> <span data-ttu-id="4c9a5-150">Kattintson az első headnode.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-150">Click the first headnode.</span></span>

    ![Headnode IP-cím keresése](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/cluster-headnodes.png)
4. <span data-ttu-id="4c9a5-152">Megnyílik, az oldal alján a a **összegzés** mezőbe másolja át az IP-címe a headnode és az állomás neve.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-152">At the bottom of the page that opens, from the **Summary** box, copy the IP address of the headnode and the host name.</span></span>

    ![Headnode IP-cím keresése](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/headnode-ip-address.png)
5. <span data-ttu-id="4c9a5-154">Az IP-cím és a headnode állomásneve a **állomások** fájlt a számítógépen, ahonnan szeretné futtatni, és távolról a a Spark feladatok hibakeresési.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-154">Include the IP address and the host name of the headnode to the **hosts** file on the computer from where you want to run and remotely debug the Spark jobs.</span></span> <span data-ttu-id="4c9a5-155">Ez lehetővé teszi az IP-címet, valamint az állomásnevet használja headnode folytatott kommunikációhoz.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-155">This will enable you to communicate with the headnode using the IP address as well as the hostname.</span></span>

   1. <span data-ttu-id="4c9a5-156">Nyissa meg a Jegyzettömbben emelt szintű engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-156">Open a notepad with elevated permissions.</span></span> <span data-ttu-id="4c9a5-157">A Fájl menüben kattintson a **nyitott** , majd lépjen a hosts fájl helyét.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-157">From the file menu, click **Open** and then navigate to the location of the hosts file.</span></span> <span data-ttu-id="4c9a5-158">A Windows-számítógépen van `C:\Windows\System32\Drivers\etc\hosts`.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-158">On a Windows computer, it is `C:\Windows\System32\Drivers\etc\hosts`.</span></span>
   2. <span data-ttu-id="4c9a5-159">Adja hozzá a következőt a **állomások** fájlt.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-159">Add the following to the **hosts** file.</span></span>

           # For headnode0
           192.xxx.xx.xx hn0-nitinp
           192.xxx.xx.xx hn0-nitinp.lhwwghjkpqejawpqbwcdyp3.gx.internal.cloudapp.net

           # For headnode1
           192.xxx.xx.xx hn1-nitinp
           192.xxx.xx.xx hn1-nitinp.lhwwghjkpqejawpqbwcdyp3.gx.internal.cloudapp.net
6. <span data-ttu-id="4c9a5-160">A számítógépre, hogy csatlakozott az Azure-beli virtuális hálózathoz, a HDInsight-fürt által használt győződjön meg arról, hogy pingelhető-e mind a headnodes IP-címét, valamint az állomásnevet használja.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-160">From the computer that you connected to the Azure Virtual Network that is used by the HDInsight cluster, verify that you can ping both the headnodes using the IP address as well as the hostname.</span></span>
7. <span data-ttu-id="4c9a5-161">SSH-ból a következő utasításokat követve fürt headnode [egy HDInsight-fürthöz SSH használatával csatlakozhat](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="4c9a5-161">SSH into the cluster headnode using the instructions at [Connect to an HDInsight cluster using SSH](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span> <span data-ttu-id="4c9a5-162">A fürt headnode Pingelje meg az asztali számítógép IP-címét.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-162">From the cluster headnode, ping the IP address of the desktop computer.</span></span> <span data-ttu-id="4c9a5-163">Az IP-címe. a számítógép, a hálózati kapcsolat, a másik az Azure virtuális hálózat, amely a számítógép csatlakozik egy rendelt kapcsolat kell tesztelni.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-163">You should test connectivity to both the IP addresses assigned to the computer, one for the network connection and the other for the Azure Virtual Network that the computer is connected to.</span></span>
8. <span data-ttu-id="4c9a5-164">Ismételje meg a lépést a többi headnode is.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-164">Repeat the steps for the other headnode as well.</span></span>

## <a name="step-4-create-a-spark-scala-application-using-the-hdinsight-tools-in-azure-toolkit-for-intellij-and-configure-it-for-remote-debugging"></a><span data-ttu-id="4c9a5-165">4. lépés: A HDInsight Tools használatával az Azure-eszközkészlet az IntelliJ Spark Scala-alkalmazás létrehozása, és konfigurálja a távoli hibakeresés</span><span class="sxs-lookup"><span data-stu-id="4c9a5-165">Step 4: Create a Spark Scala application using the HDInsight Tools in Azure Toolkit for IntelliJ and configure it for remote debugging</span></span>
1. <span data-ttu-id="4c9a5-166">Indítsa el az IntelliJ IDEA, és hozzon létre egy új projektet.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-166">Launch IntelliJ IDEA and create a new project.</span></span> <span data-ttu-id="4c9a5-167">Az új projekt párbeszédpanel a következők közül választhat, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-167">In the new project dialog box, make the following choices, and then click **Next**.</span></span>

    ![Spark Scala-alkalmazás létrehozása](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/create-hdi-scala-app.png)

   * <span data-ttu-id="4c9a5-169">A bal oldali ablaktáblán válassza ki a **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-169">From the left pane, select **HDInsight**.</span></span>
   * <span data-ttu-id="4c9a5-170">A jobb oldali ablaktáblában jelölje ki **a Spark on HDInsight (Scala)**.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-170">From the right pane, select **Spark on HDInsight (Scala)**.</span></span>
   * <span data-ttu-id="4c9a5-171">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-171">Click **Next**.</span></span>
2. <span data-ttu-id="4c9a5-172">A következő ablakban adja meg a következő projekt részleteit, és kattintson **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-172">In the next window, provide the following project details, and then click **Finish**.</span></span>  
   - <span data-ttu-id="4c9a5-173">Adja meg a projekt nevét és a projekt helyére.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-173">Provide a project name and project location.</span></span>
   - <span data-ttu-id="4c9a5-174">A **projekt SDK**, használja a Java 1.8 spark 2.x fürthöz, a spark-fürt 1.x 1.7 Java.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-174">For **Project SDK**, Use Java 1.8 for spark 2.x cluster, Java 1.7 for spark 1.x cluster.</span></span>
   - <span data-ttu-id="4c9a5-175">A **Spark verzió**, Scala-projekt létrehozása varázsló Spark SDK és Scala SDK integrálja a megfelelő verzióját.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-175">For **Spark Version**, Scala project creation wizard integrates proper version for Spark SDK and Scala SDK.</span></span> <span data-ttu-id="4c9a5-176">Ha a spark-fürt verziószáma alacsonyabb 2.0, válassza a spark 1.x.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-176">If the spark cluster version is lower 2.0, choose spark 1.x.</span></span> <span data-ttu-id="4c9a5-177">Ellenkező esetben spark2.x kell választania.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-177">Otherwise, you should select spark2.x.</span></span> <span data-ttu-id="4c9a5-178">A példa Spark2.0.2 (Scala 2.11.8).</span><span class="sxs-lookup"><span data-stu-id="4c9a5-178">This example uses Spark2.0.2(Scala 2.11.8).</span></span>
       <span data-ttu-id="4c9a5-179">![Spark Scala-alkalmazás létrehozása](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/hdi-scala-project-details.png)</span><span class="sxs-lookup"><span data-stu-id="4c9a5-179">![Create Spark Scala application](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/hdi-scala-project-details.png)</span></span>
  
3. <span data-ttu-id="4c9a5-180">A Spark-projekt automatikusan összetevő hozza létre.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-180">The Spark project will automatically create an artifact for you.</span></span> <span data-ttu-id="4c9a5-181">Az összetevő megtekintéséhez kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-181">To see the artifact, follow these steps.</span></span>

   1. <span data-ttu-id="4c9a5-182">Az a **fájl** menüben kattintson a **szerkezetének**.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-182">From the **File** menu, click **Project Structure**.</span></span>
   2. <span data-ttu-id="4c9a5-183">Az a **szerkezetének** párbeszédpanel, kattintson a **összetevők** létrehozott alapértelmezett összetevő megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-183">In the **Project Structure** dialog box, click **Artifacts** to see the default artifact that is created.</span></span>
   <span data-ttu-id="4c9a5-184">![Hozzon létre JAR](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/default-artifact.png)</span><span class="sxs-lookup"><span data-stu-id="4c9a5-184">![Create JAR](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/default-artifact.png)</span></span>

      <span data-ttu-id="4c9a5-185">Is létrehozhat saját összetevő bly kattint a  **+**  ikonra, a fenti kép kiemelve.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-185">You can also create your own artifact bly clicking on the **+** icon, highlighted in the image above.</span></span>

4. <span data-ttu-id="4c9a5-186">Szalagtárak hozzáadása a projekthez.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-186">Add libraries to your project.</span></span> <span data-ttu-id="4c9a5-187">A szalagtár hozzáadásához kattintson a jobb gombbal a projekt nevét a projekt csomópontjára, és kattintson **beállítások modul megnyitása**.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-187">To add a library, right-click the project name in the project tree, and then click **Open Module Settings**.</span></span> <span data-ttu-id="4c9a5-188">Az a **szerkezetének** párbeszédpanel bal oldali ablaktáblában kattintson a **szalagtárak**, kattintson a (+) szimbólum, és kattintson a **a Maven**.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-188">In the **Project Structure** dialog box, from the left pane, click **Libraries**, click the (+) symbol, and then click **From Maven**.</span></span>

    ![Könyvtár hozzáadása](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/add-library.png)

    <span data-ttu-id="4c9a5-190">Az a **letöltése könyvtár Maven tárházból** párbeszédpanel mezőben keressen, és adja hozzá az alábbi kódtárak.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-190">In the **Download Library from Maven Repository** dialog box, search and add the following libraries.</span></span>

   * `org.scalatest:scalatest_2.10:2.2.1`
   * `org.apache.hadoop:hadoop-azure:2.7.1`
5. <span data-ttu-id="4c9a5-191">Másolás `yarn-site.xml` és `core-site.xml` a a fürt headnode és adja hozzá a projekthez.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-191">Copy `yarn-site.xml` and `core-site.xml` from the cluster headnode and add it to the project.</span></span> <span data-ttu-id="4c9a5-192">A következő parancsok segítségével másolja a fájlokat.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-192">Use the following commands to copy the files.</span></span> <span data-ttu-id="4c9a5-193">Használhat [Cygwin](https://cygwin.com/install.html) futtatásához a következő `scp` a fájlok másolását a fürt headnodes parancsok.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-193">You can use [Cygwin](https://cygwin.com/install.html) to run the following `scp` commands to copy the files from the cluster headnodes.</span></span>

        scp <ssh user name>@<headnode IP address or host name>://etc/hadoop/conf/core-site.xml .

    <span data-ttu-id="4c9a5-194">Mivel azt már adja meg a fürt headnode IP-cím és állomásnevekkel fő az állomások fájlba az asztalon, használhatjuk a **scp** parancsok a következő módon.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-194">Because we already added the cluster headnode IP address and hostnames fo the hosts file on the desktop, we can use the **scp** commands in the following manner.</span></span>

        scp sshuser@hn0-nitinp:/etc/hadoop/conf/core-site.xml .
        scp sshuser@hn0-nitinp:/etc/hadoop/conf/yarn-site.xml .

    <span data-ttu-id="4c9a5-195">Ezek a fájlok hozzáadása a projekthez a másolásával a **/src** mappa a projekt csomópontjára, például `<your project directory>\src`.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-195">Add these files to your project by copying them under the **/src** folder in your project tree, for example `<your project directory>\src`.</span></span>
6. <span data-ttu-id="4c9a5-196">Frissítés a `core-site.xml` a következő módosításokat.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-196">Update the `core-site.xml` to make the following changes.</span></span>

   1. <span data-ttu-id="4c9a5-197">`core-site.xml`a tárfiókhoz a fürthöz tartozó titkosított kulcsot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-197">`core-site.xml` includes the encrypted key to the storage account associated with the cluster.</span></span> <span data-ttu-id="4c9a5-198">Az a `core-site.xml` , hogy hozzáadta a projekthez, a titkosított kulcs cserélje le a tényleges kulcsot társított az alapértelmezett tárfiók.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-198">In the `core-site.xml` that you added to the project, replace the encrypted key with the actual storage key associated with the default storage account.</span></span> <span data-ttu-id="4c9a5-199">Lásd: [a tárelérési kulcsok kezelése](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span><span class="sxs-lookup"><span data-stu-id="4c9a5-199">See [Manage your storage access keys](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span>

           <property>
                 <name>fs.azure.account.key.hdistoragecentral.blob.core.windows.net</name>
                 <value>access-key-associated-with-the-account</value>
           </property>
   2. <span data-ttu-id="4c9a5-200">Távolítsa el a következő bejegyzéseket a `core-site.xml`.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-200">Remove the following entries from the `core-site.xml`.</span></span>

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
   3. <span data-ttu-id="4c9a5-201">Mentse a fájlt.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-201">Save the file.</span></span>
7. <span data-ttu-id="4c9a5-202">Vegye fel a fő osztály az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-202">Add the Main class for your application.</span></span> <span data-ttu-id="4c9a5-203">Az a **Project Explorer**, kattintson a jobb gombbal **src**, mutasson a **új**, és kattintson a **Scala osztály**.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-203">From the **Project Explorer**, right-click **src**, point to **New**, and then click **Scala class**.</span></span>

    ![Forráskód hozzáadása](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/hdi-spark-scala-code.png)
8. <span data-ttu-id="4c9a5-205">Az a **új Scala osztály létrehozása** párbeszédpanelen adja meg egy nevet, **jellegű** kiválasztása **objektum**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-205">In the **Create New Scala Class** dialog box, provide a name, for **Kind** select **Object**, and then click **OK**.</span></span>

    ![Forráskód hozzáadása](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/hdi-spark-scala-code-object.png)
9. <span data-ttu-id="4c9a5-207">Az a `MyClusterAppMain.scala` fájlt, az alábbi kódot.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-207">In the `MyClusterAppMain.scala` file, paste the following code.</span></span> <span data-ttu-id="4c9a5-208">Ezt a kódot hoz létre a Spark, a környezetben, és elindítja egy `executeJob` metódust a `SparkSample` objektum.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-208">This code creates the Spark context and launches an `executeJob` method from the `SparkSample` object.</span></span>

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

10. <span data-ttu-id="4c9a5-209">Ismételje meg a 8. és 9 nevű új Scala objektum a fenti `SparkSample`.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-209">Repeat steps 8 and 9 above to add a new Scala object called `SparkSample`.</span></span> <span data-ttu-id="4c9a5-210">Ez az osztály fel a következő kódot.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-210">To this class add the following code.</span></span> <span data-ttu-id="4c9a5-211">Ezt a kódot az adatokat olvas a HVAC.csv (az összes HDInsight Spark-fürtjei elérhető), lekéri a sor csak egy számot a fürt megosztott kötetei szolgáltatás hetedik oszlopban, és ír a kimeneti **/HVACOut** alatt az alapértelmezett tároló a fürthöz.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-211">This code reads the data from the HVAC.csv (available on all HDInsight Spark clusters), retrieves the rows that only have one digit in the seventh column in the CSV, and writes the output to **/HVACOut** under the default storage container for the cluster.</span></span>

        import org.apache.spark.SparkContext

        object SparkSample {
         def executeJob (sc: SparkContext, input: String, output: String): Unit = {
           val rdd = sc.textFile(input)

           //find the rows which have only one digit in the 7th column in the CSV
           val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)

           val s = sc.parallelize(rdd.take(5)).cartesian(rdd).count()
           println(s)

           rdd1.saveAsTextFile(output)
           //rdd1.collect().foreach(println)
         }
        }
11. <span data-ttu-id="4c9a5-212">Ismételje meg a 8. és 9 a fenti adjon hozzá egy új osztályt `RemoteClusterDebugging`.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-212">Repeat steps 8 and 9 above to add a new class called `RemoteClusterDebugging`.</span></span> <span data-ttu-id="4c9a5-213">Ez az osztály a Spark keretrendszeréhez alkalmazások hibakereséshez használt valósítja meg.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-213">This class implements the Spark test framework that is used for debugging applications.</span></span> <span data-ttu-id="4c9a5-214">Adja hozzá a következő kódot a `RemoteClusterDebugging` osztály.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-214">Add the following code to the `RemoteClusterDebugging` class.</span></span>

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

     <span data-ttu-id="4c9a5-215">Néhány fontos itt ügyeljen a következőkre:</span><span class="sxs-lookup"><span data-stu-id="4c9a5-215">Couple of important things to note here:</span></span>

   * <span data-ttu-id="4c9a5-216">A `.set("spark.yarn.jar", "wasb:///hdp/apps/2.4.2.0-258/spark-assembly-1.6.1.2.4.2.0-258-hadoop2.7.1.2.4.2.0-258.jar")`, ellenőrizze, hogy a külső szerelvény JAR érhető el a fürttároló, a megadott elérési úton.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-216">For `.set("spark.yarn.jar", "wasb:///hdp/apps/2.4.2.0-258/spark-assembly-1.6.1.2.4.2.0-258-hadoop2.7.1.2.4.2.0-258.jar")`, make sure the Spark assembly JAR is available on the cluster storage at the specified path.</span></span>
   * <span data-ttu-id="4c9a5-217">A `setJars`, adja meg a helyet, ahol a összetevő jar létrejön.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-217">For `setJars`, specify the location where the artifact jar will be created.</span></span> <span data-ttu-id="4c9a5-218">Általában akkor `<Your IntelliJ project directory>\out\<project name>_DefaultArtifact\default_artifact.jar`.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-218">Typically it is `<Your IntelliJ project directory>\out\<project name>_DefaultArtifact\default_artifact.jar`.</span></span>
12. <span data-ttu-id="4c9a5-219">Az a `RemoteClusterDebugging` osztály, kattintson a jobb gombbal a `test` kulcsszót, és válassza **létrehozása RemoteClusterDebugging konfigurációs**.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-219">In the `RemoteClusterDebugging` class, right-click the `test` keyword and select **Create RemoteClusterDebugging Configuration**.</span></span>

    ![A távoli konfiguráció létrehozása](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/create-remote-config.png)

13. <span data-ttu-id="4c9a5-221">A párbeszédpanelen adja meg a konfiguráció nevét, és válassza a **jellegű tesztelése** , **teszt neve**.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-221">In the dialog box, provide a name for the configuration, and select the **Test kind** as **Test name**.</span></span> <span data-ttu-id="4c9a5-222">Minden más értéket alapértelmezett hagyja, kattintson a **alkalmaz**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-222">Leave all other values as default, click **Apply**, and then click **OK**.</span></span>

    ![A távoli konfiguráció létrehozása](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/provide-config-value.png)
14. <span data-ttu-id="4c9a5-224">Ekkor megjelenik egy **távoli Futtatás** konfigurációs legördülő a menüsávon.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-224">You should now see a **Remote Run** configuration drop-down in the menu bar.</span></span>

    ![A távoli konfiguráció létrehozása](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/config-run.png)

## <a name="step-5-run-the-application-in-debug-mode"></a><span data-ttu-id="4c9a5-226">5. lépés: Futtassa az alkalmazást hibakeresési módban</span><span class="sxs-lookup"><span data-stu-id="4c9a5-226">Step 5: Run the application in debug mode</span></span>
1. <span data-ttu-id="4c9a5-227">Az IntelliJ IDEA projektben nyissa meg `SparkSample.scala` , és hozzon létre egy töréspontot mellett látható "val rdd1".</span><span class="sxs-lookup"><span data-stu-id="4c9a5-227">In your IntelliJ IDEA project, open `SparkSample.scala` and create a breakpoint next to \`val rdd1'.</span></span> <span data-ttu-id="4c9a5-228">A töréspont létrehozásához, a megjelenő menüben válassza ki a **függvény executeJob sor**.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-228">In the pop-up menu for creating a breakpoint, select **line in function executeJob**.</span></span>

    ![Töréspont](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/create-breakpoint.png)
2. <span data-ttu-id="4c9a5-230">Kattintson a **Debug futtassa** megjelenítő gombra a **távoli Futtatás** konfigurációs legördülő futtatni az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-230">Click the **Debug Run** button next to the **Remote Run** configuration drop-down to start running the application.</span></span>

    ![A program futtatása a hibakeresési módban](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-run-mode.png)
3. <span data-ttu-id="4c9a5-232">Amikor a program végrehajtását eléri a töréspont, megjelenik egy **hibakereső** fülre az alsó ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-232">When the program execution reaches the breakpoint, you should see a **Debugger** tab in the bottom pane.</span></span>

    ![A program futtatása a hibakeresési módban](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-add-watch.png)
4. <span data-ttu-id="4c9a5-234">Kattintson a (**+**) ikonra kattintva vegye fel a figyelés, az alábbi ábrán látható módon.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-234">Click the (**+**) icon to add a watch as shown in the image below.</span></span>

    ![A program futtatása a hibakeresési módban](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-add-watch-variable.png)

    <span data-ttu-id="4c9a5-236">Itt, mert a kérelem túllépte a változó előtt `rdd1` lett létrehozva, Mik a változó első 5 sora láthatja a figyelési `rdd`.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-236">Here, because the application broke before the variable `rdd1` was created, using this watch we can see what are the first 5 rows in the variable `rdd`.</span></span> <span data-ttu-id="4c9a5-237">Nyomja le az **ENTER** billentyűt.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-237">Press **ENTER**.</span></span>

    ![A program futtatása a hibakeresési módban](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-add-watch-variable-value.png)

    <span data-ttu-id="4c9a5-239">A fenti kép láthatók az, hogy futásidőben, akkor lekérdezhet terrabytes adatok és a hibakeresési módját az alkalmazás időtartamára.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-239">What you see in the image above is that at runtime, you could query terrabytes of data and debug how your application progresses.</span></span> <span data-ttu-id="4c9a5-240">Például a kimenetben, az ábrán látható, láthatja, hogy a kimeneti első sora-e egy fejlécet.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-240">For example, in the output shown in the image above, you can see that the first row of the output is a header.</span></span> <span data-ttu-id="4c9a5-241">Ennek alapján módosíthatja az alkalmazás kódjában a fejlécsor kihagyhatja, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-241">Based on this, you can modify your application code to skip the header row if required.</span></span>
5. <span data-ttu-id="4c9a5-242">Most kattintson a **folytatása Program** ikon gombra az alkalmazás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-242">You can now click the **Resume Program** icon to proceed with your application run.</span></span>

    ![A program futtatása a hibakeresési módban](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-continue-run.png)
6. <span data-ttu-id="4c9a5-244">Ha az alkalmazás sikeresen befejeződött, a következőhöz hasonló kimenetnek kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="4c9a5-244">If the application completes successfully, you should see an output like the following.</span></span>

    ![A program futtatása a hibakeresési módban](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-complete.png)

## <span data-ttu-id="4c9a5-246"><a name="seealso"></a>Lásd még:</span><span class="sxs-lookup"><span data-stu-id="4c9a5-246"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="4c9a5-247">Overview: Apache Spark on Azure HDInsight (Áttekintés: Apache Spark on Azure HDInsight)</span><span class="sxs-lookup"><span data-stu-id="4c9a5-247">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="demo"></a><span data-ttu-id="4c9a5-248">Bemutató</span><span class="sxs-lookup"><span data-stu-id="4c9a5-248">Demo</span></span>
* <span data-ttu-id="4c9a5-249">(Videó) Scala-projekt létrehozása: [Spark Scala-alkalmazások létrehozása](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)</span><span class="sxs-lookup"><span data-stu-id="4c9a5-249">Create Scala Project (Video): [Create Spark Scala Applications](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)</span></span>
* <span data-ttu-id="4c9a5-250">Távoli hibakeresési (videó): [IntelliJ a Spark-alkalmazások távolról a HDInsight-fürt használata Azure eszköztára](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)</span><span class="sxs-lookup"><span data-stu-id="4c9a5-250">Remote Debug (Video): [Use Azure Toolkit for IntelliJ to debug Spark applications remotely on HDInsight Cluster](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)</span></span>

### <a name="scenarios"></a><span data-ttu-id="4c9a5-251">Forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="4c9a5-251">Scenarios</span></span>
* [<span data-ttu-id="4c9a5-252">Spark és BI: Interaktív adatelemzés végrehajtása a Spark on HDInsight használatával, BI-eszközökkel</span><span class="sxs-lookup"><span data-stu-id="4c9a5-252">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="4c9a5-253">Spark és Machine Learning: A Spark on HDInsight használata az épület-hőmérséklet elemzésére HVAC-adatok alapján</span><span class="sxs-lookup"><span data-stu-id="4c9a5-253">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="4c9a5-254">Spark és Machine Learning: A Spark on HDInsight használata az élelmiszervizsgálati eredmények előrejelzésére</span><span class="sxs-lookup"><span data-stu-id="4c9a5-254">Spark with Machine Learning: Use Spark in HDInsight to predict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="4c9a5-255">Spark Streaming: A Spark on HDInsight használata valós idejű streamelési alkalmazások összeállítására</span><span class="sxs-lookup"><span data-stu-id="4c9a5-255">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="4c9a5-256">A webhelynapló elemzése a Spark on HDInsight használatával</span><span class="sxs-lookup"><span data-stu-id="4c9a5-256">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="4c9a5-257">Alkalmazások létrehozása és futtatása</span><span class="sxs-lookup"><span data-stu-id="4c9a5-257">Create and run applications</span></span>
* [<span data-ttu-id="4c9a5-258">Önálló alkalmazás létrehozása a Scala használatával</span><span class="sxs-lookup"><span data-stu-id="4c9a5-258">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="4c9a5-259">Feladatok távoli futtatása Spark-fürtön a Livy használatával</span><span class="sxs-lookup"><span data-stu-id="4c9a5-259">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="4c9a5-260">Eszközök és bővítmények</span><span class="sxs-lookup"><span data-stu-id="4c9a5-260">Tools and extensions</span></span>
* [<span data-ttu-id="4c9a5-261">Az intellij-t Azure eszköztára a HDInsight Tools használatával hozzon létre, és küldje el a Spark Scala applicatons</span><span class="sxs-lookup"><span data-stu-id="4c9a5-261">Use HDInsight Tools in Azure Toolkit for IntelliJ to create and submit Spark Scala applicatons</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="4c9a5-262">Az intellij-t Azure eszközkészlet segítségével SSH keresztül távolról Spark-alkalmazások</span><span class="sxs-lookup"><span data-stu-id="4c9a5-262">Use Azure Toolkit for IntelliJ to debug Spark applications remotely through SSH</span></span>](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [<span data-ttu-id="4c9a5-263">Használja a HDInsight Tools for IntelliJ a Hortonworks védőfal</span><span class="sxs-lookup"><span data-stu-id="4c9a5-263">Use HDInsight Tools for IntelliJ with Hortonworks Sandbox</span></span>](hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [<span data-ttu-id="4c9a5-264">Az Eclipse Azure eszközkészlet a HDInsight Tools használatával Spark-alkalmazások létrehozása</span><span class="sxs-lookup"><span data-stu-id="4c9a5-264">Use HDInsight Tools in Azure Toolkit for Eclipse to create Spark applications</span></span>](hdinsight-apache-spark-eclipse-tool-plugin.md)
* [<span data-ttu-id="4c9a5-265">Zeppelin notebookok használata Spark-fürttel HDInsighton</span><span class="sxs-lookup"><span data-stu-id="4c9a5-265">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="4c9a5-266">Jupyter notebookokhoz elérhető kernelek a HDInsight Spark-fürtjében</span><span class="sxs-lookup"><span data-stu-id="4c9a5-266">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="4c9a5-267">Külső csomagok használata Jupyter notebookokkal</span><span class="sxs-lookup"><span data-stu-id="4c9a5-267">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="4c9a5-268">A Jupyter telepítése a számítógépre, majd csatlakozás egy HDInsight Spark-fürthöz</span><span class="sxs-lookup"><span data-stu-id="4c9a5-268">Install Jupyter on your computer and connect to an HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="4c9a5-269">Erőforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="4c9a5-269">Manage resources</span></span>
* [<span data-ttu-id="4c9a5-270">Apache Spark-fürt erőforrásainak kezelése az Azure HDInsightban</span><span class="sxs-lookup"><span data-stu-id="4c9a5-270">Manage resources for the Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="4c9a5-271">Apache Spark-fürtön futó feladatok nyomon követése és hibakeresése a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="4c9a5-271">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)
