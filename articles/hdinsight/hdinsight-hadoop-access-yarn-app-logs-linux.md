---
title: "Linux-alapú HDInsight - Azure bejelentkezik hozzáférést Hadoop YARN alkalmazás |} Microsoft Docs"
description: "Ismerje meg, hogyan férhet hozzá a YARN alkalmazásnaplók a parancssori és egy webes böngésző használata Linux-alapú HDInsight (Hadoop) fürtön."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 3ec08d20-4f19-4a8e-ac86-639c04d2f12e
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: fbbbddc47f24a46eac9bc64d4420ee8429ed4ad1
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="access-yarn-application-logs-on-linux-based-hdinsight"></a><span data-ttu-id="be2f6-103">Linux-alapú HDInsight bejelentkezik hozzáférést YARN alkalmazás</span><span class="sxs-lookup"><span data-stu-id="be2f6-103">Access YARN application logs on Linux-based HDInsight</span></span>

<span data-ttu-id="be2f6-104">Ismerje meg, hogyan férhet hozzá a naplók a YARN (még egy másik erőforrás egyeztető) alkalmazásokhoz az Azure HDInsight Hadoop-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="be2f6-104">Learn how to access the logs for YARN (Yet Another Resource Negotiator) applications on a Hadoop cluster in Azure HDInsight.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="be2f6-105">A jelen dokumentumban leírt lépések egy HDInsight-fürt által használt Linux igényelnek.</span><span class="sxs-lookup"><span data-stu-id="be2f6-105">The steps in this document require an HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="be2f6-106">A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható.</span><span class="sxs-lookup"><span data-stu-id="be2f6-106">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="be2f6-107">További információkért lásd: [HDInsight-összetevők verziószámozása](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="be2f6-107">For more information, see [HDInsight component versioning](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <span data-ttu-id="be2f6-108"><a name="YARNTimelineServer"></a>YARN ütemterv kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="be2f6-108"><a name="YARNTimelineServer"></a>YARN Timeline Server</span></span>

<span data-ttu-id="be2f6-109">A [YARN ütemterv Server](http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html) általános információkat nyújt a kész alkalmazások és a keretrendszer-specifikus alkalmazással kapcsolatos adatok két különböző felületeken keresztül.</span><span class="sxs-lookup"><span data-stu-id="be2f6-109">The [YARN Timeline Server](http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html) provides generic information on completed applications and framework-specific application information through two different interfaces.</span></span> <span data-ttu-id="be2f6-110">Konkrétan:</span><span class="sxs-lookup"><span data-stu-id="be2f6-110">Specifically:</span></span>

* <span data-ttu-id="be2f6-111">Tárolása és lekérése általános alkalmazás információk a HDInsight-fürtökön engedélyezett 3.1.1.374 verziójával vagy magasabb volt.</span><span class="sxs-lookup"><span data-stu-id="be2f6-111">Storage and retrieval of generic application information on HDInsight clusters has been enabled with version 3.1.1.374 or higher.</span></span>
* <span data-ttu-id="be2f6-112">A keretrendszer-specifikus információk alkalmazásösszetevő ütemterv kiszolgáló jelenleg nem áll rendelkezésre a HDInsight-fürtökön.</span><span class="sxs-lookup"><span data-stu-id="be2f6-112">The framework-specific application information component of the Timeline Server is not currently available on HDInsight clusters.</span></span>

<span data-ttu-id="be2f6-113">Alkalmazások általános információkat tartalmaz a következő típusú adatok:</span><span class="sxs-lookup"><span data-stu-id="be2f6-113">Generic information on applications includes the following type of data:</span></span>

* <span data-ttu-id="be2f6-114">Az Alkalmazásazonosító, az alkalmazás egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="be2f6-114">The application ID, a unique identifier of an application</span></span>
* <span data-ttu-id="be2f6-115">A felhasználó az alkalmazást</span><span class="sxs-lookup"><span data-stu-id="be2f6-115">The user who started the application</span></span>
* <span data-ttu-id="be2f6-116">Az alkalmazás befejezéséhez kísérletek információk</span><span class="sxs-lookup"><span data-stu-id="be2f6-116">Information on attempts made to complete the application</span></span>
* <span data-ttu-id="be2f6-117">A tárolók kísérel meg adott alkalmazás által használt</span><span class="sxs-lookup"><span data-stu-id="be2f6-117">The containers used by any given application attempt</span></span>

## <span data-ttu-id="be2f6-118"><a name="YARNAppsAndLogs"></a>YARN alkalmazások és a naplókat</span><span class="sxs-lookup"><span data-stu-id="be2f6-118"><a name="YARNAppsAndLogs"></a>YARN applications and logs</span></span>

<span data-ttu-id="be2f6-119">YARN erőforrás-kezelés az ütemezés/alkalmazásfigyelés leválasztásával több programozási modellek (MapReduce egyik folyamatban) támogatja.</span><span class="sxs-lookup"><span data-stu-id="be2f6-119">YARN supports multiple programming models (MapReduce being one of them) by decoupling resource management from application scheduling/monitoring.</span></span> <span data-ttu-id="be2f6-120">YARN használ egy globális *ResourceManager* (RM) worker-csomópontonként *NodeManagers* (NMs), és alkalmazásonként *ApplicationMasters* (AMs).</span><span class="sxs-lookup"><span data-stu-id="be2f6-120">YARN uses a global *ResourceManager* (RM), per-worker-node *NodeManagers* (NMs), and per-application *ApplicationMasters* (AMs).</span></span> <span data-ttu-id="be2f6-121">Az alkalmazás kor (Processzor, memória, lemez, hálózati) erőforrások egyezteti az alkalmazás futtatásához a RM a</span><span class="sxs-lookup"><span data-stu-id="be2f6-121">The per-application AM negotiates resources (CPU, memory, disk, network) for running your application with the RM.</span></span> <span data-ttu-id="be2f6-122">Az erőforrás-kezelő együttműködve biztosítja ezeket az erőforrásokat, amelyek kiadott megadását NMs *tárolók*.</span><span class="sxs-lookup"><span data-stu-id="be2f6-122">The RM works with NMs to grant these resources, which are granted as *containers*.</span></span> <span data-ttu-id="be2f6-123">A AM felelős a tárolók által a RM rendelt állapotának nyomon követése</span><span class="sxs-lookup"><span data-stu-id="be2f6-123">The AM is responsible for tracking the progress of the containers assigned to it by the RM.</span></span> <span data-ttu-id="be2f6-124">Az alkalmazás az alkalmazás természetétől függően számos tárolók lehet szükség.</span><span class="sxs-lookup"><span data-stu-id="be2f6-124">An application may require many containers depending on the nature of the application.</span></span>

<span data-ttu-id="be2f6-125">Minden alkalmazás több állhat *alkalmazás kísérletek*.</span><span class="sxs-lookup"><span data-stu-id="be2f6-125">Each application may consist of multiple *application attempts*.</span></span> <span data-ttu-id="be2f6-126">Ha egy alkalmazás sikertelen, akkor előfordulhat, hogy újra megkísérli egy új kísérlet.</span><span class="sxs-lookup"><span data-stu-id="be2f6-126">If an application fails, it may be retried as a new attempt.</span></span> <span data-ttu-id="be2f6-127">A tárolóban lévő egyes kísérletek fut.</span><span class="sxs-lookup"><span data-stu-id="be2f6-127">Each attempt runs in a container.</span></span> <span data-ttu-id="be2f6-128">Bizonyos értelemben egy tárolót biztosít a YARN alkalmazások által végzett munka alapvető egysége a környezetben.</span><span class="sxs-lookup"><span data-stu-id="be2f6-128">In a sense, a container provides the context for basic unit of work performed by a YARN application.</span></span> <span data-ttu-id="be2f6-129">Egy tároló keretében végrehajtott összes munka a egyetlen munkavégző csomóponton, amelyen a tároló lefoglalt történik.</span><span class="sxs-lookup"><span data-stu-id="be2f6-129">All work that is done within the context of a container is performed on the single worker node on which the container was allocated.</span></span> <span data-ttu-id="be2f6-130">Lásd: [YARN fogalmak] [ YARN-concepts] további referenciaként.</span><span class="sxs-lookup"><span data-stu-id="be2f6-130">See [YARN Concepts][YARN-concepts] for further reference.</span></span>

<span data-ttu-id="be2f6-131">Alkalmazásnaplók (és a társított tároló naplók) kritikusak megoldani a problémát okozó Hadoop-alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="be2f6-131">Application logs (and the associated container logs) are critical in debugging problematic Hadoop applications.</span></span> <span data-ttu-id="be2f6-132">YARN gyűjtése, összesítése és tárolása az alkalmazásnaplók töltött keretet biztosít a [napló összesítési] [ log-aggregation] szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="be2f6-132">YARN provides a nice framework for collecting, aggregating, and storing application logs with the [Log Aggregation][log-aggregation] feature.</span></span> <span data-ttu-id="be2f6-133">A napló összesítési szolgáltatás révén elérése során alkalmazásnaplók sokkal kiszámíthatóbb.</span><span class="sxs-lookup"><span data-stu-id="be2f6-133">The Log Aggregation feature makes accessing application logs more deterministic.</span></span> <span data-ttu-id="be2f6-134">Naplók von össze az összes tároló, a munkavégző csomópont, és tárolja őket egy összesített naplófájlhoz munkavégző csomópont.</span><span class="sxs-lookup"><span data-stu-id="be2f6-134">It aggregates logs across all containers on a worker node and stores them as one aggregated log file per worker node.</span></span> <span data-ttu-id="be2f6-135">A napló tárolja az alapértelmezett fájlrendszer egy alkalmazás befejeződése után.</span><span class="sxs-lookup"><span data-stu-id="be2f6-135">The log is stored on the default file system after an application finishes.</span></span> <span data-ttu-id="be2f6-136">Az alkalmazás használhat több száz vagy ezer tárolók, de egyetlen munkavégző csomóponton futtassa az összes tároló naplókat mindig összesítése egy fájlba.</span><span class="sxs-lookup"><span data-stu-id="be2f6-136">Your application may use hundreds or thousands of containers, but logs for all containers run on a single worker node are always aggregated to a single file.</span></span> <span data-ttu-id="be2f6-137">Így nincs csak egyes munkavégző csomópontok az alkalmazás által használt 1 napló.</span><span class="sxs-lookup"><span data-stu-id="be2f6-137">So there is only 1 log per worker node used by your application.</span></span> <span data-ttu-id="be2f6-138">Napló összesítési HDInsight fürtök 3.0-s verzió vagy újabb rendszeren alapértelmezés szerint engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="be2f6-138">Log Aggregation is enabled by default on HDInsight clusters version 3.0 and above.</span></span> <span data-ttu-id="be2f6-139">Összesített találhatók a fürt tárolóhelyét alapértelmezett.</span><span class="sxs-lookup"><span data-stu-id="be2f6-139">Aggregated logs are located in default storage for the cluster.</span></span> <span data-ttu-id="be2f6-140">A következő elérési út a naplók a HDFS elérési útja:</span><span class="sxs-lookup"><span data-stu-id="be2f6-140">The following path is the HDFS path to the logs:</span></span>

    /app-logs/<user>/logs/<applicationId>

<span data-ttu-id="be2f6-141">Az elérési út `user` az alkalmazást elindító felhasználó neve.</span><span class="sxs-lookup"><span data-stu-id="be2f6-141">In the path, `user` is the name of the user who started the application.</span></span> <span data-ttu-id="be2f6-142">A `applicationId` egy alkalmazás a YARN RM által hozzárendelt egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="be2f6-142">The `applicationId` is the unique identifier assigned to an application by the YARN RM.</span></span>

<span data-ttu-id="be2f6-143">Az összesített naplók nincsenek közvetlenül is olvasható, mivel az oktatóprogram egy [TFile][T-file], [bináris formátum] [ binary-format] indexelik a tárolóban.</span><span class="sxs-lookup"><span data-stu-id="be2f6-143">The aggregated logs are not directly readable, as they are written in a [TFile][T-file], [binary format][binary-format] indexed by container.</span></span> <span data-ttu-id="be2f6-144">A YARN erőforrás-kezelő naplók vagy a parancssori eszközök segítségével az alkalmazások vagy a tárolókat érdeklő egyszerű szövegként ezek a naplók megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="be2f6-144">Use the YARN ResourceManager logs or CLI tools to view these logs as plain text for applications or containers of interest.</span></span>

## <a name="yarn-cli-tools"></a><span data-ttu-id="be2f6-145">YARN CLI-eszközei</span><span class="sxs-lookup"><span data-stu-id="be2f6-145">YARN CLI tools</span></span>

<span data-ttu-id="be2f6-146">A YARN CLI eszközök használatához először csatlakoznia kell a HDInsight-fürthöz SSH használatával.</span><span class="sxs-lookup"><span data-stu-id="be2f6-146">To use the YARN CLI tools, you must first connect to the HDInsight cluster using SSH.</span></span> <span data-ttu-id="be2f6-147">További információk: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="be2f6-147">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

<span data-ttu-id="be2f6-148">Ezek a naplók egyszerű szövegként tekintheti meg a következő parancsok egyikét futtatja:</span><span class="sxs-lookup"><span data-stu-id="be2f6-148">You can view these logs as plain text by running one of the following commands:</span></span>

    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application>
    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application> -containerId <containerId> -nodeAddress <worker-node-address>

<span data-ttu-id="be2f6-149">Adja meg a &lt;applicationId >, &lt;felhasználói-akik-elindult-az-alkalmazási >, &lt;Tárolóazonosító >, és &lt;worker-csomópont-címe > adatokat, amikor a parancsok futtatásával.</span><span class="sxs-lookup"><span data-stu-id="be2f6-149">Specify the &lt;applicationId>, &lt;user-who-started-the-application>, &lt;containerId>, and &lt;worker-node-address> information when running these commands.</span></span>

## <a name="yarn-resourcemanager-ui"></a><span data-ttu-id="be2f6-150">YARN erőforrás-kezelő felhasználói felületen</span><span class="sxs-lookup"><span data-stu-id="be2f6-150">YARN ResourceManager UI</span></span>

<span data-ttu-id="be2f6-151">A YARN erőforrás-kezelő felhasználói felületén a fürt headnode futtatja.</span><span class="sxs-lookup"><span data-stu-id="be2f6-151">The YARN ResourceManager UI runs on the cluster headnode.</span></span> <span data-ttu-id="be2f6-152">Az Ambari webes felhasználói felületen keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="be2f6-152">It is accessed through the Ambari web UI.</span></span> <span data-ttu-id="be2f6-153">A YARN naplók megtekintéséhez tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="be2f6-153">Use the following steps to view the YARN logs:</span></span>

1. <span data-ttu-id="be2f6-154">A böngészőben navigáljon https://CLUSTERNAME.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="be2f6-154">In your web browser, navigate to https://CLUSTERNAME.azurehdinsight.net.</span></span> <span data-ttu-id="be2f6-155">CLUSTERNAME cserélje le a HDInsight-fürt nevét.</span><span class="sxs-lookup"><span data-stu-id="be2f6-155">Replace CLUSTERNAME with the name of your HDInsight cluster.</span></span>
2. <span data-ttu-id="be2f6-156">Válassza ki a listáról a bal oldali szolgáltatások **YARN**.</span><span class="sxs-lookup"><span data-stu-id="be2f6-156">From the list of services on the left, select **YARN**.</span></span>

    ![Kijelölt yarn szolgáltatás](./media/hdinsight-hadoop-access-yarn-app-logs-linux/yarnservice.png)
3. <span data-ttu-id="be2f6-158">Az a **Gyorshivatkozások** legördülő menüben válasszon ki egy központi csomópont, és válassza ki **ResourceManager napló**.</span><span class="sxs-lookup"><span data-stu-id="be2f6-158">From the **Quick Links** dropdown, select one of the cluster head nodes and then select **ResourceManager Log**.</span></span>

    ![Yarn Gyorshivatkozások](./media/hdinsight-hadoop-access-yarn-app-logs-linux/yarnquicklinks.png)

    <span data-ttu-id="be2f6-160">A YARN naplóit hivatkozáslista jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="be2f6-160">You are presented with a list of links to YARN logs.</span></span>

[YARN-timeline-server]:http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html
[log-aggregation]:http://hortonworks.com/blog/simplifying-user-logs-management-and-access-in-yarn/
[T-file]:https://issues.apache.org/jira/secure/attachment/12396286/TFile%20Specification%2020081217.pdf
[binary-format]:https://issues.apache.org/jira/browse/HADOOP-3315
[YARN-concepts]:http://hortonworks.com/blog/apache-hadoop-yarn-concepts-and-applications/
