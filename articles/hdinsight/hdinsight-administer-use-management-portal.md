---
title: "A HDInsight az Azure portál használatával Windows-alapú Hadoop-fürtök kezelése |} Microsoft Docs"
description: "Megtudhatja, hogyan felügyelheti a HDInsight-szolgáltatás. HDInsight-fürtök létrehozása, nyissa meg az interaktív JavaScript-konzolt, és nyissa meg a Hadoop parancskonzolról."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 9295a988-bd88-453a-8c8b-55fa103bf39c
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: f69fa4f838b22ccbb25186c08cac9744bb31c6d1
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="manage-windows-based-hadoop-clusters-in-hdinsight-by-using-the-azure-portal"></a><span data-ttu-id="14f84-104">A HDInsight Windows-alapú Hadoop-fürtök kezelése az Azure portál használatával</span><span class="sxs-lookup"><span data-stu-id="14f84-104">Manage Windows-based Hadoop clusters in HDInsight by using the Azure portal</span></span>

<span data-ttu-id="14f84-105">Használja a [Azure-portálon][azure-portal], Windows-alapú Hadoop-fürtök létrehozása az Azure HDInsight, Hadoop felhasználói jelszó módosításához, és engedélyezze a Remote Desktop Protocol (RDP), hogy hozzáférhessen a Hadoop parancskonzolról a fürtön.</span><span class="sxs-lookup"><span data-stu-id="14f84-105">Using the [Azure portal][azure-portal], you can create Windows-based Hadoop clusters in Azure HDInsight, change Hadoop user password, and enable Remote Desktop Protocol (RDP) so you can access the Hadoop command console on the cluster.</span></span>

<span data-ttu-id="14f84-106">A cikkben szereplő információk csak a Windows-alapú HDInsight-fürtök vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="14f84-106">The information in this article only applies to Window-based HDInsight clusters.</span></span> <span data-ttu-id="14f84-107">Linux-alapú fürtökön kezeléséről további információért lásd: [kezelése Hadoop-fürtök a HDInsight az Azure portál használatával](hdinsight-administer-use-portal-linux.md).</span><span class="sxs-lookup"><span data-stu-id="14f84-107">For information on managing Linux-based clusters, see [Manage Hadoop clusters in HDInsight by using the Azure portal](hdinsight-administer-use-portal-linux.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="14f84-108">A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható.</span><span class="sxs-lookup"><span data-stu-id="14f84-108">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="14f84-109">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="14f84-109">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="14f84-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="14f84-110">Prerequisites</span></span>

<span data-ttu-id="14f84-111">A cikk elkezdéséhez az alábbiakkal kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="14f84-111">Before you begin this article, you must have the following:</span></span>

* <span data-ttu-id="14f84-112">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="14f84-112">**An Azure subscription**.</span></span> <span data-ttu-id="14f84-113">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="14f84-113">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="14f84-114">**Az Azure Storage-fiók** -egy HDInsight-fürthöz az Azure Blob storage tárolót használja, mint az alapértelmezett fájlrendszert.</span><span class="sxs-lookup"><span data-stu-id="14f84-114">**Azure Storage account** - An HDInsight cluster uses an Azure Blob storage container as the default file system.</span></span> <span data-ttu-id="14f84-115">Hogyan nyújt az Azure Blob storage a HDInsight-fürtökkel zökkenőmentes élményt kapcsolatos további információkért lásd: [az Azure Blob Storage a hdinsight eszközzel](hdinsight-hadoop-use-blob-storage.md).</span><span class="sxs-lookup"><span data-stu-id="14f84-115">For more information about how Azure Blob storage provides a seamless experience with HDInsight clusters, see [Use Azure Blob Storage with HDInsight](hdinsight-hadoop-use-blob-storage.md).</span></span> <span data-ttu-id="14f84-116">Egy Azure Storage-fiók létrehozásával kapcsolatos részletekért lásd: [a Storage-fiók létrehozása](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="14f84-116">For details on creating an Azure Storage account, see [How to Create a Storage Account](../storage/common/storage-create-storage-account.md).</span></span>

## <a name="open-the-portal"></a><span data-ttu-id="14f84-117">A portál megnyitása</span><span class="sxs-lookup"><span data-stu-id="14f84-117">Open the Portal</span></span>
1. <span data-ttu-id="14f84-118">Jelentkezzen be [https://portal.azure.com](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="14f84-118">Sign in to [https://portal.azure.com](https://portal.azure.com).</span></span>
2. <span data-ttu-id="14f84-119">A portál megnyitása után végezhetők el:</span><span class="sxs-lookup"><span data-stu-id="14f84-119">After you open the portal, you can:</span></span>

   * <span data-ttu-id="14f84-120">Kattintson a **új** új fürt létrehozása a bal oldali menüből:</span><span class="sxs-lookup"><span data-stu-id="14f84-120">Click **New** from the left menu to create a new cluster:</span></span>

       ![a HDInsight-fürt új gomb](./media/hdinsight-administer-use-management-portal/azure-portal-new-button.png)
   * <span data-ttu-id="14f84-122">Kattintson a **a HDInsight-fürtök** a bal oldali menüből.</span><span class="sxs-lookup"><span data-stu-id="14f84-122">Click **HDInsight Clusters** from the left menu.</span></span>

       ![Az Azure portál HDInsight fürt gomb](./media/hdinsight-administer-use-management-portal/azure-portal-hdinsight-button.png)

     <span data-ttu-id="14f84-124">Ha **HDInsight** nem jelenik meg a bal oldali menüben, kattintson a **Tallózás**.</span><span class="sxs-lookup"><span data-stu-id="14f84-124">If **HDInsight** doesn't appear in the left menu, click **Browse**.</span></span>

     ![Az Azure portál fürt gomb](./media/hdinsight-administer-use-management-portal/azure-portal-browse-button.png)

## <a name="create-clusters"></a><span data-ttu-id="14f84-126">Fürtök létrehozása</span><span class="sxs-lookup"><span data-stu-id="14f84-126">Create clusters</span></span>
<span data-ttu-id="14f84-127">A létrehozása a portál használatával, lásd: [HDInsight-fürtök létrehozása](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="14f84-127">For the creation instructions using the Portal, see [Create HDInsight clusters](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

<span data-ttu-id="14f84-128">HDInsight Hadoop széles tartomány-összetevők működik.</span><span class="sxs-lookup"><span data-stu-id="14f84-128">HDInsight works with a wide range of Hadoop components.</span></span> <span data-ttu-id="14f84-129">A ellenőrizni és a támogatott összetevők listáját lásd: [Azure HDInsight Hadoop verziójának van](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="14f84-129">For the list of the components that have been verified and supported, see [What version of Hadoop is in Azure HDInsight](hdinsight-component-versioning.md).</span></span> <span data-ttu-id="14f84-130">Testre szabhatja a HDInsight az alábbi lehetőségek egyikének használatával:</span><span class="sxs-lookup"><span data-stu-id="14f84-130">You can customize HDInsight by using one of the following options:</span></span>

* <span data-ttu-id="14f84-131">Parancsfájl művelet segítségével szabhatja testre a fürt konfigurációjának módosítása, vagy egyéni összetevők, például Giraph vagy Solr telepítése egy fürt egyéni parancsfájlok.</span><span class="sxs-lookup"><span data-stu-id="14f84-131">Use Script Action to run custom scripts that can customize a cluster to either change cluster configuration or install custom components such as Giraph or Solr.</span></span> <span data-ttu-id="14f84-132">További információkért lásd: [testreszabása HDInsight-fürtjéhez parancsfájlművelet](hdinsight-hadoop-customize-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="14f84-132">For more information, see [Customize HDInsight cluster using Script Action](hdinsight-hadoop-customize-cluster.md).</span></span>
* <span data-ttu-id="14f84-133">A testreszabási Fürtparaméterek a HDInsight .NET SDK vagy az Azure PowerShell használata a fürt létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="14f84-133">Use the cluster customization parameters in the HDInsight .NET SDK or Azure PowerShell during cluster creation.</span></span> <span data-ttu-id="14f84-134">Ezek a konfigurációs módosítások majd megmaradnak a fürt élettartama keresztül, és nem érinti a fürt csomópont reimages Azure platformon rendszeres karbantartás hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="14f84-134">These configuration changes are then preserved through the lifetime of the cluster and are not affected by cluster node reimages that Azure platform periodically performs for maintenance.</span></span> <span data-ttu-id="14f84-135">A fürt testreszabási paraméterek használatával további információkért lásd: [HDInsight-fürtök létrehozása](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="14f84-135">For more information on using the cluster customization parameters, see [Create HDInsight clusters](hdinsight-hadoop-provision-linux-clusters.md).</span></span>
* <span data-ttu-id="14f84-136">Néhány natív Java-összetevők, például Mahout és kaszkádolás, JAR-fájlok formájában futtathatja a fürtön.</span><span class="sxs-lookup"><span data-stu-id="14f84-136">Some native Java components, like Mahout and Cascading, can be run on the cluster as JAR files.</span></span> <span data-ttu-id="14f84-137">A JAR-fájlok az Azure Blob storage terjeszt, és elküldi a HDInsight-fürtök Hadoop-feladat elküldése mechanizmusokon keresztül.</span><span class="sxs-lookup"><span data-stu-id="14f84-137">These JAR files can be distributed to Azure Blob storage, and submitted to HDInsight clusters through Hadoop job submission mechanisms.</span></span> <span data-ttu-id="14f84-138">További információkért lásd: [nyújt Hadoop feladatok programozott módon](hdinsight-submit-hadoop-jobs-programmatically.md).</span><span class="sxs-lookup"><span data-stu-id="14f84-138">For more information, see [Submit Hadoop jobs programmatically](hdinsight-submit-hadoop-jobs-programmatically.md).</span></span>

  > [!NOTE]
  > <span data-ttu-id="14f84-139">Ha problémába ütközik JAR-fájlok telepítése a HDInsight-fürtök, vagy hívja a JAR-fájlok a HDInsight-fürtökön, forduljon a [Microsoft Support](https://azure.microsoft.com/support/options/).</span><span class="sxs-lookup"><span data-stu-id="14f84-139">If you have issues deploying JAR files to HDInsight clusters or calling JAR files on HDInsight clusters, contact [Microsoft Support](https://azure.microsoft.com/support/options/).</span></span>
  >
  > <span data-ttu-id="14f84-140">Egymásra épülő HDInsight nem támogatja, és nincs for Microsoft Support jogosult.</span><span class="sxs-lookup"><span data-stu-id="14f84-140">Cascading is not supported by HDInsight, and is not eligible for Microsoft Support.</span></span> <span data-ttu-id="14f84-141">Támogatott összetevők listáját lásd: [What's new in HDInsight által biztosított fürt verziók](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="14f84-141">For lists of supported components, see [What's new in the cluster versions provided by HDInsight](hdinsight-component-versioning.md).</span></span>
  >
  >

<span data-ttu-id="14f84-142">A fürt távoli asztali kapcsolat segítségével egyéni szoftver telepítése nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="14f84-142">Installation of custom software on the cluster by using Remote Desktop Connection is not supported.</span></span> <span data-ttu-id="14f84-143">Kerülje a meghajtókat, az átjárócsomópont lévő fájlok tárolásához, ezek elvesznek kell újból létrehozni a fürtöket.</span><span class="sxs-lookup"><span data-stu-id="14f84-143">You should avoid storing any files on the drives of the head node, as they will be lost if you need to re-create the clusters.</span></span> <span data-ttu-id="14f84-144">Javasoljuk, hogy az Azure Blob Storage tárolóban lévő fájlok tárolásához.</span><span class="sxs-lookup"><span data-stu-id="14f84-144">We recommend storing files on Azure Blob storage.</span></span> <span data-ttu-id="14f84-145">A BLOB storage egy állandó.</span><span class="sxs-lookup"><span data-stu-id="14f84-145">Blob storage is persistent.</span></span>

## <a name="list-and-show-clusters"></a><span data-ttu-id="14f84-146">Listában, és a fürt megjelenítése</span><span class="sxs-lookup"><span data-stu-id="14f84-146">List and show clusters</span></span>
1. <span data-ttu-id="14f84-147">Jelentkezzen be [https://portal.azure.com](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="14f84-147">Sign in to [https://portal.azure.com](https://portal.azure.com).</span></span>
2. <span data-ttu-id="14f84-148">Kattintson a **a HDInsight-fürtök** a bal oldali menüből.</span><span class="sxs-lookup"><span data-stu-id="14f84-148">Click **HDInsight Clusters** from the left menu.</span></span>
3. <span data-ttu-id="14f84-149">Kattintson a fürt nevére.</span><span class="sxs-lookup"><span data-stu-id="14f84-149">Click the cluster name.</span></span> <span data-ttu-id="14f84-150">Ha a fürt lista hosszú, az oldal tetején a szűrő is használhatja.</span><span class="sxs-lookup"><span data-stu-id="14f84-150">If the cluster list is long, you can use filter on the top of the page.</span></span>
4. <span data-ttu-id="14f84-151">Kattintson duplán a fürt részleteinek megjelenítése a listáról.</span><span class="sxs-lookup"><span data-stu-id="14f84-151">Double-click a cluster from the list to show the details.</span></span>

    <span data-ttu-id="14f84-152">**Menü és essentials**:</span><span class="sxs-lookup"><span data-stu-id="14f84-152">**Menu and essentials**:</span></span>

    ![Az Azure portál HDInsight fürt alapjai](./media/hdinsight-administer-use-management-portal/hdinsight-essentials.png)

   * <span data-ttu-id="14f84-154">A menü testreszabásához kattintson a jobb gombbal a menüt, és kattintson **Testreszabás**.</span><span class="sxs-lookup"><span data-stu-id="14f84-154">To customize the menu, right-click anywhere on the menu, and then click **Customize**.</span></span>
   * <span data-ttu-id="14f84-155">**Beállítások** és **összes beállítás**: Megjeleníti a **beállítások** panel a fürt, amely lehetővé teszi, hogy a fürt részletes konfigurációs adatok eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="14f84-155">**Settings** and **All Settings**: Displays the **Settings** blade for the cluster, which allows you to access detailed configuration information for the cluster.</span></span>
   * <span data-ttu-id="14f84-156">**Irányítópult**, **fürt irányítópult** és **URL-címe: a következő összes eszközökkel, a fürt irányítópult, amely Ambari Web Linux-alapú fürtök eléréséhez. -**Biztonságos rendszerhéj **: A fürtjét Secure Shell (SSH) kapcsolaton keresztül csatlakozni utasításokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="14f84-156">**Dashboard**, **Cluster Dashboard** and **URL: These are all ways to access the cluster dashboard, which is Ambari Web for Linux-based clusters. -**Secure Shell**: Shows the instructions to connect to the cluster using Secure Shell (SSH) connection.</span></span>
   * <span data-ttu-id="14f84-157">**Fürt méretezése**: lehetővé teszi a fürt feldolgozó csomópontok számának megváltoztatására.</span><span class="sxs-lookup"><span data-stu-id="14f84-157">**Scale Cluster**: Allows you to change the number of worker nodes for this cluster.</span></span>
   * <span data-ttu-id="14f84-158">**Törlés**: törli a fürtöt.</span><span class="sxs-lookup"><span data-stu-id="14f84-158">**Delete**: Deletes the cluster.</span></span>
   * <span data-ttu-id="14f84-159">**Gyors üzembe helyezés**: információit jeleníti meg, amelyek segítségével a HDInsight használatának megkezdésében.</span><span class="sxs-lookup"><span data-stu-id="14f84-159">**Quickstart**: Displays information that will help you get started using HDInsight.</span></span>
   * <span data-ttu-id="14f84-160">** Felhasználók: Lehetővé teszi a engedélyeinek beállítása *portál felügyeleti* a fürt más felhasználók számára az Azure-előfizetése.</span><span class="sxs-lookup"><span data-stu-id="14f84-160">**Users: Allows you to set permissions for *portal management* of this cluster for other users on your Azure subscription.</span></span>

     > [!IMPORTANT]
     > <span data-ttu-id="14f84-161">Ez *csak* hatással van a hozzáférést és engedélyeket ehhez a fürthöz az Azure portálon, és nincs hatása, akik csatlakozni vagy a HDInsight-fürt feladatok elküldéséhez.</span><span class="sxs-lookup"><span data-stu-id="14f84-161">This *only* affects access and permissions to this cluster in the Azure portal, and has no effect on who can connect to or submit jobs to the HDInsight cluster.</span></span>
     >
     >
   * <span data-ttu-id="14f84-162">**Címkék**: címkék lehetővé teszik a felhőalapú szolgáltatások egyéni besorolás meghatározásához kulcs/érték párok.</span><span class="sxs-lookup"><span data-stu-id="14f84-162">**Tags**: Tags allow you to set key/value pairs to define a custom taxonomy of your cloud services.</span></span> <span data-ttu-id="14f84-163">Például létrehozhat nevű kulcs **projekt**, majd használja az adott projekthez tartozó összes szolgáltatás közös értéket.</span><span class="sxs-lookup"><span data-stu-id="14f84-163">For example, you may create a key named **project**, and then use a common value for all services associated with a specific project.</span></span>
   * <span data-ttu-id="14f84-164">**Az Ambari nézetek**: Ambari Web mutató hivatkozásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="14f84-164">**Ambari Views**: Links to Ambari Web.</span></span>

     > [!IMPORTANT]
     > <span data-ttu-id="14f84-165">A HDInsight-fürt által nyújtott szolgáltatások kezelésére, az Ambari Web vagy az Ambari REST API-t kell használnia.</span><span class="sxs-lookup"><span data-stu-id="14f84-165">To manage the services provided by the HDInsight cluster, you must use Ambari Web or the Ambari REST API.</span></span> <span data-ttu-id="14f84-166">További információ az Ambari használatával, lásd: [kezelése HDInsight-fürtök Ambari használatával](hdinsight-hadoop-manage-ambari.md).</span><span class="sxs-lookup"><span data-stu-id="14f84-166">For more information on using Ambari, see [Manage HDInsight clusters using Ambari](hdinsight-hadoop-manage-ambari.md).</span></span>
     >
     >

     <span data-ttu-id="14f84-167">**Használati**:</span><span class="sxs-lookup"><span data-stu-id="14f84-167">**Usage**:</span></span>

     ![Az Azure portál HDInsight fürt használata](./media/hdinsight-administer-use-management-portal/hdinsight-portal-cluster-usage.png)
5. <span data-ttu-id="14f84-169">Kattintson a **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="14f84-169">Click **Settings**.</span></span>

    ![Az Azure portál HDInsight fürt használata](./media/hdinsight-administer-use-management-portal/hdinsight.portal.cluster.settings.png)

   * <span data-ttu-id="14f84-171">**Tulajdonságok**: a fürt tulajdonságainak megtekintése.</span><span class="sxs-lookup"><span data-stu-id="14f84-171">**Properties**: View the cluster properties.</span></span>
   * <span data-ttu-id="14f84-172">**A fürt AAD-identitása**:</span><span class="sxs-lookup"><span data-stu-id="14f84-172">**Cluster AAD Identity**:</span></span>
   * <span data-ttu-id="14f84-173">**Az Azure Storage-kulcsok**: az alapértelmezett tárfiók és a kulcsok megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="14f84-173">**Azure Storage Keys**: View the default storage account and its key.</span></span> <span data-ttu-id="14f84-174">A beállítás a tárfiók a fürt létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="14f84-174">The storage account is configuration during the cluster creation process.</span></span>
   * <span data-ttu-id="14f84-175">**A fürt bejelentkezési**: módosítsa a fürt HTTP felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="14f84-175">**Cluster Login**: Change the cluster HTTP user name and password.</span></span>
   * <span data-ttu-id="14f84-176">**Külső Metaadattárakat**: a Hive és az Oozie metastores megtekintése.</span><span class="sxs-lookup"><span data-stu-id="14f84-176">**External Metastores**: View the Hive and Oozie metastores.</span></span> <span data-ttu-id="14f84-177">A metaadattárakat csak konfigurálható úgy, hogy a fürt létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="14f84-177">The metastores can only be configured during the cluster creation process.</span></span>
   * <span data-ttu-id="14f84-178">**Fürt méretezése**: növelése és a fürt feldolgozó csomópontok száma csökken.</span><span class="sxs-lookup"><span data-stu-id="14f84-178">**Scale Cluster**: Increase and decrease the number of cluster worker nodes.</span></span>
   * <span data-ttu-id="14f84-179">**A távoli asztal**: engedélyezze és tiltsa le a távoli asztal (RDP) eléréséhez, és konfigurálja az RDP-felhasználónév.</span><span class="sxs-lookup"><span data-stu-id="14f84-179">**Remote Desktop**: Enable and disable remote desktop (RDP) access, and configure the RDP username.</span></span>  <span data-ttu-id="14f84-180">A távoli asztalhoz tartozó felhasználónév a HTTP felhasználónévben különbözőnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="14f84-180">The RDP user name must be different from the HTTP user name.</span></span>
   * <span data-ttu-id="14f84-181">**Bejegyzett partner**:</span><span class="sxs-lookup"><span data-stu-id="14f84-181">**Partner of Record**:</span></span>

     > [!NOTE]
     > <span data-ttu-id="14f84-182">Ez az egy általános rendelkezésre álló beállítások listája; nem minden tárolódjanak minden fürt esetében.</span><span class="sxs-lookup"><span data-stu-id="14f84-182">This is a generic list of available settings; not all of them will be present for all cluster types.</span></span>
     >
     >
6. <span data-ttu-id="14f84-183">Kattintson a **tulajdonságok**:</span><span class="sxs-lookup"><span data-stu-id="14f84-183">Click **Properties**:</span></span>

    <span data-ttu-id="14f84-184">A Tulajdonságok szakaszának sorolja fel a következő:</span><span class="sxs-lookup"><span data-stu-id="14f84-184">The properties section lists the following:</span></span>

   * <span data-ttu-id="14f84-185">**Állomásnév**: fürt nevét.</span><span class="sxs-lookup"><span data-stu-id="14f84-185">**Hostname**: Cluster name.</span></span>
   * <span data-ttu-id="14f84-186">**A fürt URL-cím**.</span><span class="sxs-lookup"><span data-stu-id="14f84-186">**Cluster URL**.</span></span>
   * <span data-ttu-id="14f84-187">**Állapot**: tartalmaznak megszakadt, fogadja el, ClusterStorageProvisioned, AzureVMConfiguration, HDInsightConfiguration, működési, fut, a hiba, törlése, törlése, időtúllépésbe került, DeleteQueued, DeleteTimedout, DeleteError, PatchQueued, CertRolloverQueued, ResizeQueued, ClusterCustomization</span><span class="sxs-lookup"><span data-stu-id="14f84-187">**Status**: Include Aborted, Accepted, ClusterStorageProvisioned, AzureVMConfiguration, HDInsightConfiguration, Operational, Running, Error, Deleting, Deleted, Timedout, DeleteQueued, DeleteTimedout, DeleteError, PatchQueued, CertRolloverQueued, ResizeQueued, ClusterCustomization</span></span>
   * <span data-ttu-id="14f84-188">**A régióban**: Azure-beli hely.</span><span class="sxs-lookup"><span data-stu-id="14f84-188">**Region**: Azure location.</span></span> <span data-ttu-id="14f84-189">Támogatott Azure helyek listáját lásd: a **régió** lévő legördülő lista [HDInsight árképzési](https://azure.microsoft.com/pricing/details/hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="14f84-189">For a list of supported Azure locations, see the **Region** dropdown list box on [HDInsight pricing](https://azure.microsoft.com/pricing/details/hdinsight/).</span></span>
   * <span data-ttu-id="14f84-190">**Létrehozott adatokat**.</span><span class="sxs-lookup"><span data-stu-id="14f84-190">**Data created**.</span></span>
   * <span data-ttu-id="14f84-191">**Operációs rendszer**: vagy **Windows** vagy **Linux**.</span><span class="sxs-lookup"><span data-stu-id="14f84-191">**Operating system**: Either **Windows** or **Linux**.</span></span>
   * <span data-ttu-id="14f84-192">**Típus**: Hadoop, HBase, Storm, Spark.</span><span class="sxs-lookup"><span data-stu-id="14f84-192">**Type**: Hadoop, HBase, Storm, Spark.</span></span>
   * <span data-ttu-id="14f84-193">**Verzió**.</span><span class="sxs-lookup"><span data-stu-id="14f84-193">**Version**.</span></span> <span data-ttu-id="14f84-194">Lásd: [HDInsight-verziókról](hdinsight-component-versioning.md)</span><span class="sxs-lookup"><span data-stu-id="14f84-194">See [HDInsight versions](hdinsight-component-versioning.md)</span></span>
   * <span data-ttu-id="14f84-195">**Előfizetés**: előfizetés nevét.</span><span class="sxs-lookup"><span data-stu-id="14f84-195">**Subscription**: Subscription name.</span></span>
   * <span data-ttu-id="14f84-196">**Előfizetés-azonosító**.</span><span class="sxs-lookup"><span data-stu-id="14f84-196">**Subscription ID**.</span></span>
   * <span data-ttu-id="14f84-197">**Elsődleges adatforrás**.</span><span class="sxs-lookup"><span data-stu-id="14f84-197">**Primary data source**.</span></span> <span data-ttu-id="14f84-198">Az Azure Blob storage-fiók alapértelmezés szerint használt Hadoop-fájlrendszer.</span><span class="sxs-lookup"><span data-stu-id="14f84-198">The Azure Blob storage account used as the default Hadoop file system.</span></span>
   * <span data-ttu-id="14f84-199">**IP-címek munkavégző csomópontokhoz**.</span><span class="sxs-lookup"><span data-stu-id="14f84-199">**Worker nodes pricing tier**.</span></span>
   * <span data-ttu-id="14f84-200">**Átjárócsomópont tarifacsomag**.</span><span class="sxs-lookup"><span data-stu-id="14f84-200">**Head node pricing tier**.</span></span>

## <a name="delete-clusters"></a><span data-ttu-id="14f84-201">Fürtök törlése</span><span class="sxs-lookup"><span data-stu-id="14f84-201">Delete clusters</span></span>
<span data-ttu-id="14f84-202">Törli a fürt nem törli az alapértelmezett tárfiók vagy az összes kapcsolt tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="14f84-202">Delete a cluster will not delete the default storage account or any linked storage accounts.</span></span> <span data-ttu-id="14f84-203">Újra létrehozhatja a fürtöt azonos tárfiókok és ugyanazon a metaadattárakat használatával.</span><span class="sxs-lookup"><span data-stu-id="14f84-203">You can re-create the cluster by using the same storage accounts and the same metastores.</span></span>

1. <span data-ttu-id="14f84-204">Jelentkezzen be a [Portal][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="14f84-204">Sign in to the [Portal][azure-portal].</span></span>
2. <span data-ttu-id="14f84-205">Kattintson a **összes tallózása** a bal oldali menüben kattintson a **a HDInsight-fürtök**, kattintson a fürt nevére.</span><span class="sxs-lookup"><span data-stu-id="14f84-205">Click **Browse All** from the left menu, click **HDInsight Clusters**, click your cluster name.</span></span>
3. <span data-ttu-id="14f84-206">Kattintson a **törlése** a felső menüben, majd kövesse az utasításokat.</span><span class="sxs-lookup"><span data-stu-id="14f84-206">Click **Delete** from the top menu, and then follow the instructions.</span></span>

<span data-ttu-id="14f84-207">Lásd még: [fürtök szünet/Leállítás](#pauseshut-down-clusters).</span><span class="sxs-lookup"><span data-stu-id="14f84-207">See also [Pause/shut down clusters](#pauseshut-down-clusters).</span></span>

## <a name="scale-clusters"></a><span data-ttu-id="14f84-208">Fürtök méretezése</span><span class="sxs-lookup"><span data-stu-id="14f84-208">Scale clusters</span></span>
<span data-ttu-id="14f84-209">A fürt skálázás funkciót lehetővé teszi, hogy anélkül, hogy újra létre kell hoznia a fürt fut az Azure HDInsight fürt által használt feldolgozó csomópontok számának módosítása.</span><span class="sxs-lookup"><span data-stu-id="14f84-209">The cluster scaling feature allows you to change the number of worker nodes used by a cluster that is running in Azure HDInsight without having to re-create the cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="14f84-210">Csak verzió 3.1.3 hdinsight clusters vagy annál magasabb támogatottak.</span><span class="sxs-lookup"><span data-stu-id="14f84-210">Only clusters with HDInsight version 3.1.3 or higher are supported.</span></span> <span data-ttu-id="14f84-211">Ha biztos benne, hogy a fürt verzióját, a Tulajdonságok lapján ellenőrizheti.</span><span class="sxs-lookup"><span data-stu-id="14f84-211">If you are unsure of the version of your cluster, you can check the Properties page.</span></span>  <span data-ttu-id="14f84-212">Lásd: [listája és megjelenítése fürtök](#list-and-show-clusters).</span><span class="sxs-lookup"><span data-stu-id="14f84-212">See [List and show clusters](#list-and-show-clusters).</span></span>
>
>

<span data-ttu-id="14f84-213">A fürt a HDInsight által támogatott különböző típusú adatok csomópontok számának módosítása következményei:</span><span class="sxs-lookup"><span data-stu-id="14f84-213">The impact of changing the number of data nodes for each type of cluster supported by HDInsight:</span></span>

* <span data-ttu-id="14f84-214">Hadoop</span><span class="sxs-lookup"><span data-stu-id="14f84-214">Hadoop</span></span>

    <span data-ttu-id="14f84-215">Zökkenőmentesen növelheti adhatja meg, hogy minden folyamatban lévő vagy a futó feladatok befolyásolása nélkül fut egy Hadoop-fürt feldolgozó csomópontjainak számát.</span><span class="sxs-lookup"><span data-stu-id="14f84-215">You can seamlessly increase the number of worker nodes in a Hadoop cluster that is running without impacting any pending or running jobs.</span></span> <span data-ttu-id="14f84-216">Új feladatokat is küldheti el, amíg a művelet folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="14f84-216">New jobs can also be submitted while the operation is in progress.</span></span> <span data-ttu-id="14f84-217">A méretezési művelet sikertelen szabályosan kezeli, hogy a fürt mindig működőképes állapotban marad.</span><span class="sxs-lookup"><span data-stu-id="14f84-217">Failures in a scaling operation are gracefully handled so that the cluster is always left in a functional state.</span></span>

    <span data-ttu-id="14f84-218">A Hadoop fürtök adatok csomópontok számának csökkentésével átméretezi, ha néhány, a fürt a szolgáltatások újraindításáig.</span><span class="sxs-lookup"><span data-stu-id="14f84-218">When a Hadoop cluster is scaled down by reducing the number of data nodes, some of the services in the cluster are restarted.</span></span> <span data-ttu-id="14f84-219">Ennek hatására a összes futó és függőben lévő feladatok meghiúsulhatnak, a méretezési művelet befejezését.</span><span class="sxs-lookup"><span data-stu-id="14f84-219">This causes all running and pending jobs to fail at the completion of the scaling operation.</span></span> <span data-ttu-id="14f84-220">Akkor is, azonban küldje el újra a feladatok a művelet végrehajtása után.</span><span class="sxs-lookup"><span data-stu-id="14f84-220">You can, however, resubmit the jobs once the operation is complete.</span></span>
* <span data-ttu-id="14f84-221">HBase</span><span class="sxs-lookup"><span data-stu-id="14f84-221">HBase</span></span>

    <span data-ttu-id="14f84-222">Akkor is zökkenőmentesen csomópontok hozzáadásához és eltávolításához a HBase-fürtöt a futtatása.</span><span class="sxs-lookup"><span data-stu-id="14f84-222">You can seamlessly add or remove nodes to your HBase cluster while it is running.</span></span> <span data-ttu-id="14f84-223">Területi kiszolgálók automatikus elosztását a méretezési művelet befejezését néhány percen belül.</span><span class="sxs-lookup"><span data-stu-id="14f84-223">Regional Servers are automatically balanced within a few minutes of completing the scaling operation.</span></span> <span data-ttu-id="14f84-224">Azonban Ön kézzel is eloszthatja a regionális kiszolgálók fürt headnode való bejelentkezés, és futtatja a következő parancsokat egy parancssori ablakot:</span><span class="sxs-lookup"><span data-stu-id="14f84-224">However, you can also manually balance the regional servers by logging into the headnode of cluster and running the following commands from a command prompt window:</span></span>

        >pushd %HBASE_HOME%\bin
        >hbase shell
        >balancer

    <span data-ttu-id="14f84-225">A HBase rendszerhéjjal további információkért lásd:]</span><span class="sxs-lookup"><span data-stu-id="14f84-225">For more information on using the HBase shell, see []</span></span>
* <span data-ttu-id="14f84-226">Storm</span><span class="sxs-lookup"><span data-stu-id="14f84-226">Storm</span></span>

    <span data-ttu-id="14f84-227">Akkor is zökkenőmentesen csomópontok hozzáadásához és eltávolításához adatok Storm fürthöz való futtatása során.</span><span class="sxs-lookup"><span data-stu-id="14f84-227">You can seamlessly add or remove data nodes to your Storm cluster while it is running.</span></span> <span data-ttu-id="14f84-228">De a méretezési művelet sikeres befejezését követően szüksége lesz a topológia egyensúlyba.</span><span class="sxs-lookup"><span data-stu-id="14f84-228">But after a successful completion of the scaling operation, you will need to rebalance the topology.</span></span>

    <span data-ttu-id="14f84-229">Kétféle módon valósítható meg újraelosztás:</span><span class="sxs-lookup"><span data-stu-id="14f84-229">Rebalancing can be accomplished in two ways:</span></span>

  * <span data-ttu-id="14f84-230">A Storm webes felhasználói felület</span><span class="sxs-lookup"><span data-stu-id="14f84-230">Storm web UI</span></span>
  * <span data-ttu-id="14f84-231">Parancssori felület (CLI) eszköz</span><span class="sxs-lookup"><span data-stu-id="14f84-231">Command-line interface (CLI) tool</span></span>

    <span data-ttu-id="14f84-232">Tekintse meg a [alatt futó Apache Storm-dokumentáció](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) további részleteket.</span><span class="sxs-lookup"><span data-stu-id="14f84-232">Please refer to the [Apache Storm documentation](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) for more details.</span></span>

    <span data-ttu-id="14f84-233">A Storm webes felhasználói felület érhető el a HDInsight-fürt:</span><span class="sxs-lookup"><span data-stu-id="14f84-233">The Storm web UI is available on the HDInsight cluster:</span></span>

    ![A HDInsight alatt futó storm méretezési egyensúlyozza ki újra](./media/hdinsight-administer-use-management-portal/hdinsight-portal-scale-cluster-storm-rebalance.png)

    <span data-ttu-id="14f84-235">Íme egy példa a CLI parancs használata a Storm-topológia egyensúlyba:</span><span class="sxs-lookup"><span data-stu-id="14f84-235">Here is an example how to use the CLI command to rebalance the Storm topology:</span></span>

        ## Reconfigure the topology "mytopology" to use 5 worker processes,
        ## the spout "blue-spout" to use 3 executors, and
        ## the bolt "yellow-bolt" to use 10 executors
        $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10

<span data-ttu-id="14f84-236">**Fürtök méretezése**</span><span class="sxs-lookup"><span data-stu-id="14f84-236">**To scale clusters**</span></span>

1. <span data-ttu-id="14f84-237">Jelentkezzen be a [Portal][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="14f84-237">Sign in to the [Portal][azure-portal].</span></span>
2. <span data-ttu-id="14f84-238">Kattintson a **összes tallózása** a bal oldali menüben kattintson a **a HDInsight-fürtök**, kattintson a fürt nevére.</span><span class="sxs-lookup"><span data-stu-id="14f84-238">Click **Browse All** from the left menu, click **HDInsight Clusters**, click your cluster name.</span></span>
3. <span data-ttu-id="14f84-239">Kattintson a **beállítások** a felső menüben, majd kattintson a **méretezési fürt**.</span><span class="sxs-lookup"><span data-stu-id="14f84-239">Click **Settings** from the top menu, and then click **Scale Cluster**.</span></span>
4. <span data-ttu-id="14f84-240">Adja meg **számát a feldolgozó csomópontok**.</span><span class="sxs-lookup"><span data-stu-id="14f84-240">Enter **Number of Worker nodes**.</span></span> <span data-ttu-id="14f84-241">A csomópont számára vonatkozó korlátozást Azure-előfizetések függően változik.</span><span class="sxs-lookup"><span data-stu-id="14f84-241">The limit on the number of cluster node varies among Azure subscriptions.</span></span> <span data-ttu-id="14f84-242">A korlát növeléséhez Számlázási támogatást kérhetnek.</span><span class="sxs-lookup"><span data-stu-id="14f84-242">You can contact billing support to increase the limit.</span></span>  <span data-ttu-id="14f84-243">A Költséginformációk változik meg a módosítások a csomópontok számát.</span><span class="sxs-lookup"><span data-stu-id="14f84-243">The cost information will reflect the changes you have made to the number of nodes.</span></span>

    ![HDInsight Hadoop HBase Storm Spark méretezési](./media/hdinsight-administer-use-management-portal/hdinsight.portal.scale.cluster.png)

## <a name="pauseshut-down-clusters"></a><span data-ttu-id="14f84-245">Fürtök szünet/leállítása</span><span class="sxs-lookup"><span data-stu-id="14f84-245">Pause/shut down clusters</span></span>
<span data-ttu-id="14f84-246">A legtöbb Hadoop-feladat kötegelt feladatok, amelyek csak esetenként futott.</span><span class="sxs-lookup"><span data-stu-id="14f84-246">Most of Hadoop jobs are batch jobs that are only ran occasionally.</span></span> <span data-ttu-id="14f84-247">A legtöbb Hadoop-fürtök vonatkoznak-e nagy időszakokra, amely a fürt nem használják a feldolgozáshoz.</span><span class="sxs-lookup"><span data-stu-id="14f84-247">For most Hadoop clusters, there are large periods of time that the cluster is not being used for processing.</span></span> <span data-ttu-id="14f84-248">A HDInsight az Azure Storage szolgáltatásban tárolja az adatokat, így biztonságosan törölhet olyan fürtöket, amelyek nincsenek használatban.</span><span class="sxs-lookup"><span data-stu-id="14f84-248">With HDInsight, your data is stored in Azure Storage, so you can safely delete a cluster when it is not in use.</span></span>
<span data-ttu-id="14f84-249">Ráadásul a HDInsight-fürtök akkor is díjkötelesek, amikor éppen nincsenek használatban.</span><span class="sxs-lookup"><span data-stu-id="14f84-249">You are also charged for an HDInsight cluster, even when it is not in use.</span></span> <span data-ttu-id="14f84-250">Mivel a fürt költsége a sokszorosa a tároló költségeinek, gazdaságossági szempontból is ésszerű törölni a használaton kívüli fürtöket.</span><span class="sxs-lookup"><span data-stu-id="14f84-250">Since the charges for the cluster are many times more than the charges for storage, it makes economic sense to delete clusters when they are not in use.</span></span>

<span data-ttu-id="14f84-251">Meg a program a folyamat számos módja van:</span><span class="sxs-lookup"><span data-stu-id="14f84-251">There are many ways you can program the process:</span></span>

* <span data-ttu-id="14f84-252">Az Azure Data Factory felhasználó.</span><span class="sxs-lookup"><span data-stu-id="14f84-252">User Azure Data Factory.</span></span> <span data-ttu-id="14f84-253">Lásd: [Azure HDInsight társított szolgáltatás](../data-factory/data-factory-compute-linked-services.md) és [alakít át és elemez az Azure Data Factory használatával](../data-factory/data-factory-data-transformation-activities.md) igény szerinti és önálló definiált hdinsight összekapcsolt szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="14f84-253">See [Azure HDInsight Linked Service](../data-factory/data-factory-compute-linked-services.md) and [Transform and analyze using Azure Data Factory](../data-factory/data-factory-data-transformation-activities.md) for on-demand and self-defined HDInsight linked services.</span></span>
* <span data-ttu-id="14f84-254">Azure PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="14f84-254">Use Azure PowerShell.</span></span>  <span data-ttu-id="14f84-255">Lásd: [repülési késleltetés adatok elemzése](hdinsight-analyze-flight-delay-data.md).</span><span class="sxs-lookup"><span data-stu-id="14f84-255">See [Analyze flight delay data](hdinsight-analyze-flight-delay-data.md).</span></span>
* <span data-ttu-id="14f84-256">Az Azure CLI használata.</span><span class="sxs-lookup"><span data-stu-id="14f84-256">Use Azure CLI.</span></span> <span data-ttu-id="14f84-257">Lásd: [kezelése HDInsight-fürtök Azure parancssori felület használatával](hdinsight-administer-use-command-line.md).</span><span class="sxs-lookup"><span data-stu-id="14f84-257">See [Manage HDInsight clusters using Azure CLI](hdinsight-administer-use-command-line.md).</span></span>
* <span data-ttu-id="14f84-258">A HDInsight .NET SDK használata.</span><span class="sxs-lookup"><span data-stu-id="14f84-258">Use HDInsight .NET SDK.</span></span> <span data-ttu-id="14f84-259">Lásd: [nyújt Hadoop-feladatokat](hdinsight-submit-hadoop-jobs-programmatically.md).</span><span class="sxs-lookup"><span data-stu-id="14f84-259">See [Submit Hadoop jobs](hdinsight-submit-hadoop-jobs-programmatically.md).</span></span>

<span data-ttu-id="14f84-260">Díjszabási információkért lásd: [HDInsight árképzési](https://azure.microsoft.com/pricing/details/hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="14f84-260">For the pricing information, see [HDInsight pricing](https://azure.microsoft.com/pricing/details/hdinsight/).</span></span> <span data-ttu-id="14f84-261">Törölni a fürtöt a portálról, lásd: [fürtök törlése](#delete-clusters)</span><span class="sxs-lookup"><span data-stu-id="14f84-261">To delete a cluster from the Portal, see [Delete clusters](#delete-clusters)</span></span>

## <a name="change-cluster-username"></a><span data-ttu-id="14f84-262">Változás fürt felhasználónév</span><span class="sxs-lookup"><span data-stu-id="14f84-262">Change cluster username</span></span>
<span data-ttu-id="14f84-263">HDInsight-fürtök lehet két felhasználói fiókot.</span><span class="sxs-lookup"><span data-stu-id="14f84-263">An HDInsight cluster can have two user accounts.</span></span> <span data-ttu-id="14f84-264">A HDInsight fürt felhasználói fiók jön létre a létrehozási folyamat során.</span><span class="sxs-lookup"><span data-stu-id="14f84-264">The HDInsight cluster user account is created during the creation process.</span></span> <span data-ttu-id="14f84-265">RDP-kapcsolaton keresztül a fürt eléréséhez egy RDP-felhasználói fiókot is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="14f84-265">You can also create an RDP user account for accessing the cluster via RDP.</span></span> <span data-ttu-id="14f84-266">Lásd: [engedélyezése a távoli asztal](#connect-to-hdinsight-clusters-by-using-rdp).</span><span class="sxs-lookup"><span data-stu-id="14f84-266">See [Enable remote desktop](#connect-to-hdinsight-clusters-by-using-rdp).</span></span>

<span data-ttu-id="14f84-267">**A HDInsight fürt felhasználónév és jelszó módosítása**</span><span class="sxs-lookup"><span data-stu-id="14f84-267">**To change the HDInsight cluster user name and password**</span></span>

1. <span data-ttu-id="14f84-268">Jelentkezzen be a [Portal][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="14f84-268">Sign in to the [Portal][azure-portal].</span></span>
2. <span data-ttu-id="14f84-269">Kattintson a **összes tallózása** a bal oldali menüben kattintson a **a HDInsight-fürtök**, kattintson a fürt nevére.</span><span class="sxs-lookup"><span data-stu-id="14f84-269">Click **Browse All** from the left menu, click **HDInsight Clusters**, click your cluster name.</span></span>
3. <span data-ttu-id="14f84-270">Kattintson a **beállítások** a felső menüben, majd kattintson a **fürt bejelentkezési**.</span><span class="sxs-lookup"><span data-stu-id="14f84-270">Click **Settings** from the top menu, and then click **Cluster Login**.</span></span>
4. <span data-ttu-id="14f84-271">Ha **fürt bejelentkezési** lett engedélyezve van, meg kell nyomnia **letiltása**, és kattintson a **engedélyezése** a felhasználónév és jelszó módosítása előtt...</span><span class="sxs-lookup"><span data-stu-id="14f84-271">If **Cluster login** has been enabled, you must click **Disable**, and then click **Enable** before you can change the username and password..</span></span>
5. <span data-ttu-id="14f84-272">Módosítsa a **fürt bejelentkezési neve** és/vagy a **fürt bejelentkezési jelszó**, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="14f84-272">Change the **Cluster Login Name** and/or the **Cluster Login Password**, and then click **Save**.</span></span>

    ![A HDInsight fürt felhasználói felhasználónév jelszó http felhasználó módosítása](./media/hdinsight-administer-use-management-portal/hdinsight.portal.change.username.password.png)

## <a name="grantrevoke-access"></a><span data-ttu-id="14f84-274">Hozzáférés biztosítása/visszavonása</span><span class="sxs-lookup"><span data-stu-id="14f84-274">Grant/revoke access</span></span>
<span data-ttu-id="14f84-275">A HDInsight-fürtök a következő HTTP webszolgáltatásokat (ezen szolgáltatások mindegyikéhez rendelkezik RESTful végpontok) rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="14f84-275">HDInsight clusters have the following HTTP web services (all of these services have RESTful endpoints):</span></span>

* <span data-ttu-id="14f84-276">ODBC</span><span class="sxs-lookup"><span data-stu-id="14f84-276">ODBC</span></span>
* <span data-ttu-id="14f84-277">JDBC</span><span class="sxs-lookup"><span data-stu-id="14f84-277">JDBC</span></span>
* <span data-ttu-id="14f84-278">Ambari</span><span class="sxs-lookup"><span data-stu-id="14f84-278">Ambari</span></span>
* <span data-ttu-id="14f84-279">Oozie</span><span class="sxs-lookup"><span data-stu-id="14f84-279">Oozie</span></span>
* <span data-ttu-id="14f84-280">Lépni a Templeton</span><span class="sxs-lookup"><span data-stu-id="14f84-280">Templeton</span></span>

<span data-ttu-id="14f84-281">Alapértelmezés szerint ezek a szolgáltatások hozzáférés vonatkozóan biztosított.</span><span class="sxs-lookup"><span data-stu-id="14f84-281">By default, these services are granted for access.</span></span> <span data-ttu-id="14f84-282">Akkor is a visszavonási/grant a hozzáférés az Azure portálról.</span><span class="sxs-lookup"><span data-stu-id="14f84-282">You can revoke/grant the access from the Azure portal.</span></span>

> [!NOTE]
> <span data-ttu-id="14f84-283">A hozzáférés biztosítása/visszavonása, visszaállítja, a fürt felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="14f84-283">By granting/revoking the access, you will reset the cluster user name and password.</span></span>
>
>

<span data-ttu-id="14f84-284">**Az engedélyezéshez/visszavonáshoz HTTP webes szolgáltatásokhoz férhetnek hozzá**</span><span class="sxs-lookup"><span data-stu-id="14f84-284">**To grant/revoke HTTP web services access**</span></span>

1. <span data-ttu-id="14f84-285">Jelentkezzen be a [Portal][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="14f84-285">Sign in to the [Portal][azure-portal].</span></span>
2. <span data-ttu-id="14f84-286">Kattintson a **összes tallózása** a bal oldali menüben kattintson a **a HDInsight-fürtök**, kattintson a fürt nevére.</span><span class="sxs-lookup"><span data-stu-id="14f84-286">Click **Browse All** from the left menu, click **HDInsight Clusters**, click your cluster name.</span></span>
3. <span data-ttu-id="14f84-287">Kattintson a **beállítások** a felső menüben, majd kattintson a **fürt bejelentkezési**.</span><span class="sxs-lookup"><span data-stu-id="14f84-287">Click **Settings** from the top menu, and then click **Cluster Login**.</span></span>
4. <span data-ttu-id="14f84-288">Ha **fürt bejelentkezési** lett engedélyezve van, meg kell nyomnia **letiltása**, és kattintson a **engedélyezése** a felhasználónév és jelszó módosítása előtt...</span><span class="sxs-lookup"><span data-stu-id="14f84-288">If **Cluster login** has been enabled, you must click **Disable**, and then click **Enable** before you can change the username and password..</span></span>
5. <span data-ttu-id="14f84-289">A **fürt bejelentkezési felhasználónevének** és **fürt bejelentkezési jelszó**, adja meg az új felhasználónevet és jelszót (illetve) a fürthöz.</span><span class="sxs-lookup"><span data-stu-id="14f84-289">For **Cluster Login Username** and **Cluster Login Password**, enter the new user name and password (respectively) for the cluster.</span></span>
6. <span data-ttu-id="14f84-290">Kattintson a **SAVE** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="14f84-290">Click **SAVE**.</span></span>

    ![HDInsight teljes http webes hozzáférés eltávolítása](./media/hdinsight-administer-use-management-portal/hdinsight.portal.change.username.password.png)

## <a name="find-the-default-storage-account"></a><span data-ttu-id="14f84-292">Az alapértelmezett tárfiók keresése</span><span class="sxs-lookup"><span data-stu-id="14f84-292">Find the default storage account</span></span>
<span data-ttu-id="14f84-293">Minden egyes HDInsight-fürt rendelkezik egy alapértelmezett tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="14f84-293">Each HDInsight cluster has a default storage account.</span></span> <span data-ttu-id="14f84-294">Megjelenik az alapértelmezett tárfiók és a fürt kulcsait **beállítások**/**tulajdonságok**/**Azure Storage kulcsok**.</span><span class="sxs-lookup"><span data-stu-id="14f84-294">The default storage account and its keys for a cluster appears under **Settings**/**Properties**/**Azure Storage Keys**.</span></span> <span data-ttu-id="14f84-295">Lásd: [listája és megjelenítése fürtök](#list-and-show-clusters).</span><span class="sxs-lookup"><span data-stu-id="14f84-295">See [List and show clusters](#list-and-show-clusters).</span></span>

## <a name="find-the-resource-group"></a><span data-ttu-id="14f84-296">Az erőforráscsoport keresése</span><span class="sxs-lookup"><span data-stu-id="14f84-296">Find the resource group</span></span>
<span data-ttu-id="14f84-297">Az Azure Resource Manager módban mindegyik HDInsight-fürt létrehozása egy Azure-erőforrás csoporttal.</span><span class="sxs-lookup"><span data-stu-id="14f84-297">In the Azure Resource Manager mode, each HDInsight cluster is created with an Azure resource group.</span></span> <span data-ttu-id="14f84-298">A fürthöz tartozó Azure erőforráscsoport jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="14f84-298">The Azure resource group that a cluster belongs to appears in:</span></span>

* <span data-ttu-id="14f84-299">A fürt listája tartalmaz egy **erőforráscsoport** oszlop.</span><span class="sxs-lookup"><span data-stu-id="14f84-299">The cluster list has a **Resource Group** column.</span></span>
* <span data-ttu-id="14f84-300">Fürt **alapvető** csempére.</span><span class="sxs-lookup"><span data-stu-id="14f84-300">Cluster **Essential** tile.</span></span>  

<span data-ttu-id="14f84-301">Lásd: [listája és megjelenítése fürtök](#list-and-show-clusters).</span><span class="sxs-lookup"><span data-stu-id="14f84-301">See [List and show clusters](#list-and-show-clusters).</span></span>

## <a name="open-hdinsight-query-console"></a><span data-ttu-id="14f84-302">Nyissa meg a HDInsight lekérdezés konzolt</span><span class="sxs-lookup"><span data-stu-id="14f84-302">Open HDInsight Query console</span></span>
<span data-ttu-id="14f84-303">A HDInsight lekérdezés konzol az alábbi szolgáltatásokat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="14f84-303">The HDInsight Query console includes the following features:</span></span>

* <span data-ttu-id="14f84-304">**Hive szerkesztő**: webes felülete A grafikus felhasználói Felülettel Hive-feladatok elküldésekor.</span><span class="sxs-lookup"><span data-stu-id="14f84-304">**Hive Editor**: A GUI web interface for submitting Hive jobs.</span></span>  <span data-ttu-id="14f84-305">Lásd: [a lekérdezés konzollal Hive futtatása lekérdezések](hdinsight-hadoop-use-hive-query-console.md).</span><span class="sxs-lookup"><span data-stu-id="14f84-305">See [Run Hive queries using the Query Console](hdinsight-hadoop-use-hive-query-console.md).</span></span>

    ![HDInsight portál hive szerkesztő](./media/hdinsight-administer-use-management-portal/hdinsight-hive-editor.png)
* <span data-ttu-id="14f84-307">**Feladatelőzmények**: Monitor Hadoop-feladatokat.</span><span class="sxs-lookup"><span data-stu-id="14f84-307">**Job history**: Monitor Hadoop jobs.</span></span>  

    ![HDInsight portál feladatelőzmények](./media/hdinsight-administer-use-management-portal/hdinsight-job-history.png)

    <span data-ttu-id="14f84-309">Kattintson a **lekérdezésnév** feladattulajdonság, beleértve a részletek megjelenítéséhez **feladat lekérdezés**, és ** Job Output.</span><span class="sxs-lookup"><span data-stu-id="14f84-309">Click **Query Name** to show the details including Job properties, **Job Query**, and **Job Output.</span></span> <span data-ttu-id="14f84-310">A lekérdezés, mind a kimeneti is letöltheti a munkaállomáson.</span><span class="sxs-lookup"><span data-stu-id="14f84-310">You can also download both the query and the output to your workstation.</span></span>
* <span data-ttu-id="14f84-311">**Fájl a böngésző**: tekintse át az alapértelmezett tárfiókot, és a társított tárfiókokat.</span><span class="sxs-lookup"><span data-stu-id="14f84-311">**File Browser**: Browse the default storage account and the linked storage accounts.</span></span>

    ![HDInsight portál fájl böngésző Tallózás](./media/hdinsight-administer-use-management-portal/hdinsight-file-browser.png)

    <span data-ttu-id="14f84-313">A képernyőképen látható a  **<Account>**  típus azt jelzi, hogy a cikk az Azure storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="14f84-313">On the screenshot, the **<Account>** type indicates the item is an Azure storage account.</span></span>  <span data-ttu-id="14f84-314">Kattintson a fiók nevére, a fájlok tallózásához.</span><span class="sxs-lookup"><span data-stu-id="14f84-314">Click the account name to browse the files.</span></span>
* <span data-ttu-id="14f84-315">**Hadoop-UI**.</span><span class="sxs-lookup"><span data-stu-id="14f84-315">**Hadoop UI**.</span></span>

    ![HDInsight portal Hadoop felhasználói felület](./media/hdinsight-administer-use-management-portal/hdinsight-hadoop-ui.png)

    <span data-ttu-id="14f84-317">A **Hadoop felhasználói felület*, keresse meg a fájlokat, és ellenőrizze a naplókat.</span><span class="sxs-lookup"><span data-stu-id="14f84-317">From **Hadoop UI*, you can browse files, and check logs.</span></span>
* <span data-ttu-id="14f84-318">**A yarn felhasználói felület**.</span><span class="sxs-lookup"><span data-stu-id="14f84-318">**Yarn UI**.</span></span>

    ![HDInsight portal YARN felhasználói felületen](./media/hdinsight-administer-use-management-portal/hdinsight-yarn-ui.png)

## <a name="run-hive-queries"></a><span data-ttu-id="14f84-320">Hive-lekérdezések futtatása</span><span class="sxs-lookup"><span data-stu-id="14f84-320">Run Hive queries</span></span>
<span data-ttu-id="14f84-321">Hive-feladatok futtatása a portálról, kattintson a **Hive szerkesztő** a HDInsight lekérdezés konzolon.</span><span class="sxs-lookup"><span data-stu-id="14f84-321">To ran Hive jobs from the Portal, click **Hive Editor** in the HDInsight Query console.</span></span> <span data-ttu-id="14f84-322">Lásd: [nyissa meg a HDInsight-lekérdezés konzol](#open-hdinsight-query-console).</span><span class="sxs-lookup"><span data-stu-id="14f84-322">See [Open HDInsight Query console](#open-hdinsight-query-console).</span></span>

## <a name="monitor-jobs"></a><span data-ttu-id="14f84-323">Feladatok figyelése</span><span class="sxs-lookup"><span data-stu-id="14f84-323">Monitor jobs</span></span>
<span data-ttu-id="14f84-324">A portálról feladat figyeléséhez kattintson **Feladatelőzményekben** a HDInsight lekérdezés konzolon.</span><span class="sxs-lookup"><span data-stu-id="14f84-324">To monitor jobs from the Portal, click **Job History** in the HDInsight Query console.</span></span> <span data-ttu-id="14f84-325">Lásd: [nyissa meg a HDInsight-lekérdezés konzol](#open-hdinsight-query-console).</span><span class="sxs-lookup"><span data-stu-id="14f84-325">See [Open HDInsight Query console](#open-hdinsight-query-console).</span></span>

## <a name="browse-files"></a><span data-ttu-id="14f84-326">Fájlok tallózása</span><span class="sxs-lookup"><span data-stu-id="14f84-326">Browse files</span></span>
<span data-ttu-id="14f84-327">Keresse meg az alapértelmezett tárfiók és a társított tárfiókokat tárolt fájlok, kattintson a **fájl böngésző** a HDInsight lekérdezés konzolon.</span><span class="sxs-lookup"><span data-stu-id="14f84-327">To browse files stored in the default storage account and the linked storage accounts, click **File Browser** in the HDInsight Query console.</span></span> <span data-ttu-id="14f84-328">Lásd: [nyissa meg a HDInsight-lekérdezés konzol](#open-hdinsight-query-console).</span><span class="sxs-lookup"><span data-stu-id="14f84-328">See [Open HDInsight Query console](#open-hdinsight-query-console).</span></span>

<span data-ttu-id="14f84-329">Használhatja a **keresse meg a fájlrendszer** az eszközt a **Hadoop felhasználói felület** a HDInsight-konzolon.</span><span class="sxs-lookup"><span data-stu-id="14f84-329">You can also use the **Browse the file system** utility from the **Hadoop UI** in the HDInsight console.</span></span>  <span data-ttu-id="14f84-330">Lásd: [nyissa meg a HDInsight-lekérdezés konzol](#open-hdinsight-query-console).</span><span class="sxs-lookup"><span data-stu-id="14f84-330">See [Open HDInsight Query console](#open-hdinsight-query-console).</span></span>

## <a name="monitor-cluster-usage"></a><span data-ttu-id="14f84-331">A figyelő fürt használata</span><span class="sxs-lookup"><span data-stu-id="14f84-331">Monitor cluster usage</span></span>
<span data-ttu-id="14f84-332">A **használati** szakasz a HDInsight-fürt panelről az elérhető és a HDInsight együttes használata az előfizetés magok száma, valamint az ehhez a fürthöz, és azok elosztását vezérli a fürtben levő csomópontok számára lefoglalt magok száma információkat jelenít meg.</span><span class="sxs-lookup"><span data-stu-id="14f84-332">The **Usage** section of the HDInsight cluster blade displays information about the number of cores available to your subscription for use with HDInsight, as well as the number of cores allocated to this cluster and how they are allocated for the nodes within this cluster.</span></span> <span data-ttu-id="14f84-333">Lásd: [listája és megjelenítése fürtök](#list-and-show-clusters).</span><span class="sxs-lookup"><span data-stu-id="14f84-333">See [List and show clusters](#list-and-show-clusters).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="14f84-334">A HDInsight-fürt által nyújtott szolgáltatások figyeléséhez, az Ambari Web vagy az Ambari REST API-t kell használnia.</span><span class="sxs-lookup"><span data-stu-id="14f84-334">To monitor the services provided by the HDInsight cluster, you must use Ambari Web or the Ambari REST API.</span></span> <span data-ttu-id="14f84-335">További információ az Ambari használatával, lásd: [kezelése HDInsight-fürtök Ambari használatával](hdinsight-hadoop-manage-ambari.md)</span><span class="sxs-lookup"><span data-stu-id="14f84-335">For more information on using Ambari, see [Manage HDInsight clusters using Ambari](hdinsight-hadoop-manage-ambari.md)</span></span>
>
>

## <a name="open-hadoop-ui"></a><span data-ttu-id="14f84-336">Nyissa meg a Hadoop felhasználói felület</span><span class="sxs-lookup"><span data-stu-id="14f84-336">Open Hadoop UI</span></span>
<span data-ttu-id="14f84-337">A fürt figyelésére, keresse meg a fájlrendszer, és ellenőrizze a naplókat, kattintson az **Hadoop felhasználói felület** a HDInsight lekérdezés konzolon.</span><span class="sxs-lookup"><span data-stu-id="14f84-337">To monitor the cluster, browse the file system, and check logs, click **Hadoop UI** in the HDInsight Query console.</span></span> <span data-ttu-id="14f84-338">Lásd: [nyissa meg a HDInsight-lekérdezés konzol](#open-hdinsight-query-console).</span><span class="sxs-lookup"><span data-stu-id="14f84-338">See [Open HDInsight Query console](#open-hdinsight-query-console).</span></span>

## <a name="open-yarn-ui"></a><span data-ttu-id="14f84-339">Nyissa meg a Yarn felhasználói felületen</span><span class="sxs-lookup"><span data-stu-id="14f84-339">Open Yarn UI</span></span>
<span data-ttu-id="14f84-340">A Yarn felhasználói felületen használatához kattintson **Yarn felhasználói felületen** a HDInsight lekérdezés konzolon.</span><span class="sxs-lookup"><span data-stu-id="14f84-340">To use Yarn user interface, click **Yarn UI** in the HDInsight Query console.</span></span> <span data-ttu-id="14f84-341">Lásd: [nyissa meg a HDInsight-lekérdezés konzol](#open-hdinsight-query-console).</span><span class="sxs-lookup"><span data-stu-id="14f84-341">See [Open HDInsight Query console](#open-hdinsight-query-console).</span></span>

## <a name="connect-to-clusters-using-rdp"></a><span data-ttu-id="14f84-342">Csatlakozás RDP-fürtök</span><span class="sxs-lookup"><span data-stu-id="14f84-342">Connect to clusters using RDP</span></span>
<span data-ttu-id="14f84-343">A fürt a létrehozása a megadott hitelesítő adatai hozzáférést a fürtön a szolgáltatásokat, de nem maga a fürt távoli asztalon keresztül.</span><span class="sxs-lookup"><span data-stu-id="14f84-343">The credentials for the cluster that you provided at its creation give access to the services on the cluster, but not to the cluster itself through Remote Desktop.</span></span> <span data-ttu-id="14f84-344">Ha a fürt, vagy a fürt létesítése után kapcsolhatja be a távoli asztal eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="14f84-344">You can turn on Remote Desktop access when you provision a cluster or after a cluster is provisioned.</span></span> <span data-ttu-id="14f84-345">A távoli asztal engedélyezése a létrehozásakor vonatkozó utasításokért lásd: [hozzon létre HDInsight-fürt](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="14f84-345">For the instructions about enabling Remote Desktop at creation, see [Create HDInsight cluster](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

<span data-ttu-id="14f84-346">**Távoli asztal engedélyezése**</span><span class="sxs-lookup"><span data-stu-id="14f84-346">**To enable Remote Desktop**</span></span>

1. <span data-ttu-id="14f84-347">Jelentkezzen be a [Portal][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="14f84-347">Sign in to the [Portal][azure-portal].</span></span>
2. <span data-ttu-id="14f84-348">Kattintson a **összes tallózása** a bal oldali menüben kattintson a **a HDInsight-fürtök**, kattintson a fürt nevére.</span><span class="sxs-lookup"><span data-stu-id="14f84-348">Click **Browse All** from the left menu, click **HDInsight Clusters**, click your cluster name.</span></span>
3. <span data-ttu-id="14f84-349">Kattintson a **beállítások** a felső menüben, majd kattintson a **távoli asztal**.</span><span class="sxs-lookup"><span data-stu-id="14f84-349">Click **Settings** from the top menu, and then click **Remote Desktop**.</span></span>
4. <span data-ttu-id="14f84-350">Adja meg **lejárati dátuma**, **távoli asztali felhasználónévnek** és **távoli asztal jelszó**, és kattintson a **engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="14f84-350">Enter **Expires On**, **Remote Desktop Username** and **Remote Desktop Password**, and then click **Enable**.</span></span>

    ![HDInsight engedélyezi letiltja a távoli asztal konfigurálása](./media/hdinsight-administer-use-management-portal/hdinsight.portal.remote.desktop.png)

    <span data-ttu-id="14f84-352">Az alapértelmezett értékeit lejárati dátuma a hét.</span><span class="sxs-lookup"><span data-stu-id="14f84-352">The default values for Expires On is a week.</span></span>

   > [!NOTE]
   > <span data-ttu-id="14f84-353">A HDInsight .NET SDK használatával a távoli asztal engedélyezése a fürtön.</span><span class="sxs-lookup"><span data-stu-id="14f84-353">You can also use the HDInsight .NET SDK to enable Remote Desktop on a cluster.</span></span> <span data-ttu-id="14f84-354">Használja a **EnableRdp** módszer a HDInsight ügyfél objektum a következő módon: **ügyfél. EnableRdp (fürtnév, hely, "rdpuser", "rdppassword" DateTime.Now.AddDays(6))**.</span><span class="sxs-lookup"><span data-stu-id="14f84-354">Use the **EnableRdp** method on the HDInsight client object in the following manner: **client.EnableRdp(clustername, location, "rdpuser", "rdppassword", DateTime.Now.AddDays(6))**.</span></span> <span data-ttu-id="14f84-355">Hasonlóképpen, a távoli asztal letiltása a fürtön, segítségével **ügyfél. (A fürtnév, a hely) DisableRdp**.</span><span class="sxs-lookup"><span data-stu-id="14f84-355">Similarly, to disable Remote Desktop on the cluster, you can use **client.DisableRdp(clustername, location)**.</span></span> <span data-ttu-id="14f84-356">E módszerekkel kapcsolatos további információkért lásd: [HDInsight .NET SDK-dokumentáció](http://go.microsoft.com/fwlink/?LinkId=529017).</span><span class="sxs-lookup"><span data-stu-id="14f84-356">For more information on these methods, see [HDInsight .NET SDK Reference](http://go.microsoft.com/fwlink/?LinkId=529017).</span></span> <span data-ttu-id="14f84-357">Ez az csak a HDInsight-fürtökön futó Windows alkalmazható.</span><span class="sxs-lookup"><span data-stu-id="14f84-357">This is applicable only for HDInsight clusters running on Windows.</span></span>
   >
   >

<span data-ttu-id="14f84-358">**RDP használatával csatlakozni egy fürthöz**</span><span class="sxs-lookup"><span data-stu-id="14f84-358">**To connect to a cluster by using RDP**</span></span>

1. <span data-ttu-id="14f84-359">Jelentkezzen be a [Portal][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="14f84-359">Sign in to the [Portal][azure-portal].</span></span>
2. <span data-ttu-id="14f84-360">Kattintson a **összes tallózása** a bal oldali menüben kattintson a **a HDInsight-fürtök**, kattintson a fürt nevére.</span><span class="sxs-lookup"><span data-stu-id="14f84-360">Click **Browse All** from the left menu, click **HDInsight Clusters**, click your cluster name.</span></span>
3. <span data-ttu-id="14f84-361">Kattintson a **beállítások** a felső menüben, majd kattintson a **távoli asztal**.</span><span class="sxs-lookup"><span data-stu-id="14f84-361">Click **Settings** from the top menu, and then click **Remote Desktop**.</span></span>
4. <span data-ttu-id="14f84-362">Kattintson a **Connect** és kövesse az utasításokat.</span><span class="sxs-lookup"><span data-stu-id="14f84-362">Click **Connect** and follow the instructions.</span></span> <span data-ttu-id="14f84-363">Ha Connect tiltsa le, engedélyeznie kell azt először.</span><span class="sxs-lookup"><span data-stu-id="14f84-363">If Connect is disable, you must enable it first.</span></span> <span data-ttu-id="14f84-364">Győződjön meg arról, hogy a távoli asztali felhasználói felhasználónévvel és jelszóval.</span><span class="sxs-lookup"><span data-stu-id="14f84-364">Make sure using the Remote Desktop user username and password.</span></span>  <span data-ttu-id="14f84-365">A fürt felhasználói hitelesítő adatok nem használhatók.</span><span class="sxs-lookup"><span data-stu-id="14f84-365">You can't use the Cluster user credentials.</span></span>

## <a name="open-hadoop-command-line"></a><span data-ttu-id="14f84-366">Nyissa meg a Hadoop parancssor</span><span class="sxs-lookup"><span data-stu-id="14f84-366">Open Hadoop command line</span></span>
<span data-ttu-id="14f84-367">Távoli asztal használatával csatlakozzon a fürthöz, és a Hadoop parancsot használja, kell először engedélyezte a fürt távoli asztal eléréséhez az előző szakaszban leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="14f84-367">To connect to the cluster by using Remote Desktop and use the Hadoop command line, you must first have enabled Remote Desktop access to the cluster as described in the previous section.</span></span>

<span data-ttu-id="14f84-368">**A Hadoop parancssor megnyitása**</span><span class="sxs-lookup"><span data-stu-id="14f84-368">**To open a Hadoop command line**</span></span>

1. <span data-ttu-id="14f84-369">Csatlakozzon a fürthöz, a távoli asztal használatával.</span><span class="sxs-lookup"><span data-stu-id="14f84-369">Connect to the cluster using Remote Desktop.</span></span>
2. <span data-ttu-id="14f84-370">Kattintson duplán az asztalon **Hadoop parancssori**.</span><span class="sxs-lookup"><span data-stu-id="14f84-370">From the desktop, double-click **Hadoop Command Line**.</span></span>

    <span data-ttu-id="14f84-371">![HDI. HadoopCommandLine][image-hadoopcommandline]</span><span class="sxs-lookup"><span data-stu-id="14f84-371">![HDI.HadoopCommandLine][image-hadoopcommandline]</span></span>

    <span data-ttu-id="14f84-372">További információk a Hadoop-parancsok: [Hadoop-parancsok hivatkozás](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/CommandsManual.html).</span><span class="sxs-lookup"><span data-stu-id="14f84-372">For more information on Hadoop commands, see [Hadoop commands reference](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/CommandsManual.html).</span></span>

<span data-ttu-id="14f84-373">Előző képernyőképen látható a mappa neve verziószáma a Hadoop beágyazott.</span><span class="sxs-lookup"><span data-stu-id="14f84-373">In the previous screenshot, the folder name has the Hadoop version number embedded.</span></span> <span data-ttu-id="14f84-374">A verziószám módosítható a fürtön telepíteni a Hadoop-összetevők verziója alapján.</span><span class="sxs-lookup"><span data-stu-id="14f84-374">The version number can changed based on the version of the Hadoop components installed on the cluster.</span></span> <span data-ttu-id="14f84-375">Hadoop környezeti változók segítségével tekintse meg a mappákra.</span><span class="sxs-lookup"><span data-stu-id="14f84-375">You can use Hadoop environment variables to refer to those folders.</span></span> <span data-ttu-id="14f84-376">Példa:</span><span class="sxs-lookup"><span data-stu-id="14f84-376">For example:</span></span>

    cd %hadoop_home%
    cd %hive_home%
    cd %hbase_home%
    cd %pig_home%
    cd %sqoop_home%
    cd %hcatalog_home%

## <a name="next-steps"></a><span data-ttu-id="14f84-377">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="14f84-377">Next steps</span></span>
<span data-ttu-id="14f84-378">Ebben a cikkben megtanulta rendelkezik a HDInsight-fürtök létrehozása a portál használatával, és nyissa meg a parancssori eszköz a Hadoop.</span><span class="sxs-lookup"><span data-stu-id="14f84-378">In this article, you have learned how to create an HDInsight cluster by using the Portal, and how to open the Hadoop command-line tool.</span></span> <span data-ttu-id="14f84-379">További tudnivalókért tekintse meg a következő cikkeket:</span><span class="sxs-lookup"><span data-stu-id="14f84-379">To learn more, see the following articles:</span></span>

* [<span data-ttu-id="14f84-380">Felügyelheti a HDInsight az Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="14f84-380">Administer HDInsight Using Azure PowerShell</span></span>](hdinsight-administer-use-powershell.md)
* [<span data-ttu-id="14f84-381">Felügyelheti a HDInsight az Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="14f84-381">Administer HDInsight Using Azure CLI</span></span>](hdinsight-administer-use-command-line.md)
* [<span data-ttu-id="14f84-382">A HDInsight-fürtök létrehozása</span><span class="sxs-lookup"><span data-stu-id="14f84-382">Create HDInsight clusters</span></span>](hdinsight-hadoop-provision-linux-clusters.md)
* [<span data-ttu-id="14f84-383">Hadoop-feladatokat programozott módon küldhet</span><span class="sxs-lookup"><span data-stu-id="14f84-383">Submit Hadoop jobs programmatically</span></span>](hdinsight-submit-hadoop-jobs-programmatically.md)
* [<span data-ttu-id="14f84-384">Ismerkedés az Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="14f84-384">Get Started with Azure HDInsight</span></span>](hdinsight-hadoop-linux-tutorial-get-started.md)
* [<span data-ttu-id="14f84-385">Azure HDInsight Hadoop verziójának van?</span><span class="sxs-lookup"><span data-stu-id="14f84-385">What version of Hadoop is in Azure HDInsight?</span></span>](hdinsight-component-versioning.md)

[azure-portal]: https://portal.azure.com
[image-hadoopcommandline]: ./media/hdinsight-administer-use-management-portal/hdinsight-hadoop-command-line.png "Hadoop parancssor"
