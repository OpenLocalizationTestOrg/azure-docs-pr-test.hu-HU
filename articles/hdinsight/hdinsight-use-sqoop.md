---
title: "Apache Sqoop feladatok futtatása az Azure HDInsight (Hadoop) |} Microsoft Docs"
description: "Ismerje meg, hogyan használható az Azure PowerShell munkaállomásról futtatása Sqoop importálása és exportálása egy Hadoop-fürt és az Azure SQL-adatbázis között."
editor: cgronlun
manager: jhubbard
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
ms.assetid: 2fdcc6b7-6ad5-4397-a30b-e7e389b66c7a
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: 8e77153493b6f37f5f48116b86bad6b25a50d1a1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-sqoop-with-hadoop-in-hdinsight"></a><span data-ttu-id="74df7-103">Sqoop használata a hadooppal a Hdinsightban</span><span class="sxs-lookup"><span data-stu-id="74df7-103">Use Sqoop with Hadoop in HDInsight</span></span>
[!INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

<span data-ttu-id="74df7-104">Ismerje meg a Sqoop használata a Hdinsightban történő importálására és exportálására HDInsight-fürt és Azure SQL database vagy az SQL Server-adatbázis között.</span><span class="sxs-lookup"><span data-stu-id="74df7-104">Learn how to use Sqoop in HDInsight to import and export between HDInsight cluster and Azure SQL database or SQL Server database.</span></span>

<span data-ttu-id="74df7-105">Hadoop természetes téve a feldolgozása a strukturálatlan és félig strukturált adatok, például a naplókat, valamint fájlt, azonban a is lehet a relációs adatbázisok tárolt strukturált adatok feldolgozása érdekében.</span><span class="sxs-lookup"><span data-stu-id="74df7-105">Although Hadoop is a natural choice for processing unstructured and semistructured data, such as logs and files, there may also be a need to process structured data that is stored in relational databases.</span></span>

<span data-ttu-id="74df7-106">[Sqoop] [ sqoop-user-guide-1.4.4] az eszköz a Hadoop-fürtök és a relációs adatbázisok közötti adattovábbításra.</span><span class="sxs-lookup"><span data-stu-id="74df7-106">[Sqoop][sqoop-user-guide-1.4.4] is a tool designed to transfer data between Hadoop clusters and relational databases.</span></span> <span data-ttu-id="74df7-107">Adatok importálása egy relációs adatbázis-kezelő rendszerének (RDBMS), például az SQL Server, MySQL, vagy a Hadoop elosztott fájlrendszer (HDFS), az Oracle átalakíthatja az adatokat a Hadoop MapReduce vagy a Hive, majd az adatok exportálása vissza egy RDBMS használhatja.</span><span class="sxs-lookup"><span data-stu-id="74df7-107">You can use it to import data from a relational database management system (RDBMS) such as SQL Server, MySQL, or Oracle into the Hadoop distributed file system (HDFS), transform the data in Hadoop with MapReduce or Hive, and then export the data back into an RDBMS.</span></span> <span data-ttu-id="74df7-108">Ebben az oktatóanyagban egy SQL Server adatbázist használ a relációs adatbázis.</span><span class="sxs-lookup"><span data-stu-id="74df7-108">In this tutorial, you are using a SQL Server database for your relational database.</span></span>

<span data-ttu-id="74df7-109">A HDInsight-fürtökön támogatott Sqoop verzióiért lásd: [What's new in HDInsight által biztosított fürt verziók?][hdinsight-versions]</span><span class="sxs-lookup"><span data-stu-id="74df7-109">For Sqoop versions that are supported on HDInsight clusters, see [What's new in the cluster versions provided by HDInsight?][hdinsight-versions]</span></span>

## <a name="understand-the-scenario"></a><span data-ttu-id="74df7-110">A forgatókönyv ismertetése</span><span class="sxs-lookup"><span data-stu-id="74df7-110">Understand the scenario</span></span>

<span data-ttu-id="74df7-111">HDInsight-fürtök néhány adatot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="74df7-111">HDInsight cluster comes with some sample data.</span></span> <span data-ttu-id="74df7-112">A következő két mintát használ:</span><span class="sxs-lookup"><span data-stu-id="74df7-112">You use the following two samples:</span></span>

* <span data-ttu-id="74df7-113">A log4j naplófájlt, amely itt található: */example/data/sample.log*.</span><span class="sxs-lookup"><span data-stu-id="74df7-113">A log4j log file, which is located at */example/data/sample.log*.</span></span> <span data-ttu-id="74df7-114">A következő fájl kibontása a következő naplók kapcsolódnak:</span><span class="sxs-lookup"><span data-stu-id="74df7-114">The following logs are extracted from the file:</span></span>
  
        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...
* <span data-ttu-id="74df7-115">Nevű Hive tábla *hivesampletable*, amely hivatkozik az adatok fájlba */hive/warehouse/hivesampletable*.</span><span class="sxs-lookup"><span data-stu-id="74df7-115">A Hive table named *hivesampletable*, which references the data file located at */hive/warehouse/hivesampletable*.</span></span> <span data-ttu-id="74df7-116">A táblázat tartalmaz néhány mobileszköz adat.</span><span class="sxs-lookup"><span data-stu-id="74df7-116">The table contains some mobile device data.</span></span> 
  
  | <span data-ttu-id="74df7-117">Mező</span><span class="sxs-lookup"><span data-stu-id="74df7-117">Field</span></span> | <span data-ttu-id="74df7-118">Adattípus</span><span class="sxs-lookup"><span data-stu-id="74df7-118">Data type</span></span> |
  | --- | --- |
  | <span data-ttu-id="74df7-119">ClientID</span><span class="sxs-lookup"><span data-stu-id="74df7-119">clientid</span></span> |<span data-ttu-id="74df7-120">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="74df7-120">string</span></span> |
  | <span data-ttu-id="74df7-121">querytime</span><span class="sxs-lookup"><span data-stu-id="74df7-121">querytime</span></span> |<span data-ttu-id="74df7-122">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="74df7-122">string</span></span> |
  | <span data-ttu-id="74df7-123">piaci</span><span class="sxs-lookup"><span data-stu-id="74df7-123">market</span></span> |<span data-ttu-id="74df7-124">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="74df7-124">string</span></span> |
  | <span data-ttu-id="74df7-125">deviceplatform</span><span class="sxs-lookup"><span data-stu-id="74df7-125">deviceplatform</span></span> |<span data-ttu-id="74df7-126">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="74df7-126">string</span></span> |
  | <span data-ttu-id="74df7-127">devicemake</span><span class="sxs-lookup"><span data-stu-id="74df7-127">devicemake</span></span> |<span data-ttu-id="74df7-128">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="74df7-128">string</span></span> |
  | <span data-ttu-id="74df7-129">devicemodel</span><span class="sxs-lookup"><span data-stu-id="74df7-129">devicemodel</span></span> |<span data-ttu-id="74df7-130">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="74df7-130">string</span></span> |
  | <span data-ttu-id="74df7-131">state</span><span class="sxs-lookup"><span data-stu-id="74df7-131">state</span></span> |<span data-ttu-id="74df7-132">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="74df7-132">string</span></span> |
  | <span data-ttu-id="74df7-133">Ország</span><span class="sxs-lookup"><span data-stu-id="74df7-133">country</span></span> |<span data-ttu-id="74df7-134">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="74df7-134">string</span></span> |
  | <span data-ttu-id="74df7-135">querydwelltime</span><span class="sxs-lookup"><span data-stu-id="74df7-135">querydwelltime</span></span> |<span data-ttu-id="74df7-136">Dupla</span><span class="sxs-lookup"><span data-stu-id="74df7-136">double</span></span> |
  | <span data-ttu-id="74df7-137">munkamenet-azonosító</span><span class="sxs-lookup"><span data-stu-id="74df7-137">sessionid</span></span> |<span data-ttu-id="74df7-138">bigint</span><span class="sxs-lookup"><span data-stu-id="74df7-138">bigint</span></span> |
  | <span data-ttu-id="74df7-139">sessionpagevieworder</span><span class="sxs-lookup"><span data-stu-id="74df7-139">sessionpagevieworder</span></span> |<span data-ttu-id="74df7-140">bigint</span><span class="sxs-lookup"><span data-stu-id="74df7-140">bigint</span></span> |

<span data-ttu-id="74df7-141">Először exportálnia *sample.log* és *hivesampletable* az Azure SQL database vagy az SQL Server és a mobileszközök adatait tartalmazó tábla biztonsági a HDInsight a következő elérési út használatával, majd importálja:</span><span class="sxs-lookup"><span data-stu-id="74df7-141">First, you export *sample.log* and *hivesampletable* to the Azure SQL database or to SQL Server, and then import the table that contains the mobile device data back to HDInsight by using the following path:</span></span>

    /tutorials/usesqoop/importeddata

## <a name="create-cluster-and-sql-database"></a><span data-ttu-id="74df7-142">Fürt és az SQL-adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="74df7-142">Create cluster and SQL database</span></span>
<span data-ttu-id="74df7-143">Ez a szakasz bemutatja, hogyan fürt SQL-adatbázis, az SQL adatbázis sémák és futtatásához az oktatóanyag az Azure-portál és az Azure Resource Manager-sablon létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="74df7-143">This section shows you how to create a cluster, a SQL Database, and the SQL database schemas for running the tutorial using the Azure portal and an Azure Resource Manager template.</span></span> <span data-ttu-id="74df7-144">A sablonban található [Azure gyors üzembe helyezési sablonokat](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-with-sql-database/).</span><span class="sxs-lookup"><span data-stu-id="74df7-144">The template can be found in [Azure QuickStart Templates](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-with-sql-database/).</span></span> <span data-ttu-id="74df7-145">A Resource Manager-sablon meghívja a táblasémákat telepítendő SQL-adatbázis bacpac csomag.</span><span class="sxs-lookup"><span data-stu-id="74df7-145">The Resource Manager template calls a bacpac package to deploy the table schemas to SQL database.</span></span>  <span data-ttu-id="74df7-146">A következő nyilvános blobtárolóban https://hditutorialdata.blob.core.windows.net/usesqoop/SqoopTutorial-2016-2-23-11-2.bacpac a bacpac csomag található.</span><span class="sxs-lookup"><span data-stu-id="74df7-146">The bacpac package is located in a public blob container, https://hditutorialdata.blob.core.windows.net/usesqoop/SqoopTutorial-2016-2-23-11-2.bacpac.</span></span> <span data-ttu-id="74df7-147">Ha azt szeretné, személyes tároló használata a bacpac-fájlok, a sablon a következő értékeket használja:</span><span class="sxs-lookup"><span data-stu-id="74df7-147">If you want to use a private container for the bacpac files, use the following values in the template:</span></span>
   
        "storageKeyType": "Primary",
        "storageKey": "<TheAzureStorageAccountKey>",

<span data-ttu-id="74df7-148">Ha inkább az Azure PowerShell használatával a fürt és az SQL-adatbázis létrehozása című [függelék](#appendix-a---a-powershell-sample).</span><span class="sxs-lookup"><span data-stu-id="74df7-148">If you prefer to use Azure PowerShell to create the cluster and the SQL Database, see [Appendix A](#appendix-a---a-powershell-sample).</span></span>

1. <span data-ttu-id="74df7-149">Kattintson az alábbi képre kattintva nyissa meg a Resource Manager-sablon az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="74df7-149">Click the following image to open a Resource Manager template in the Azure portal.</span></span>         
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-with-sql-database%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-use-sqoop/deploy-to-azure.png" alt="Deploy to Azure"></a>
   

2. <span data-ttu-id="74df7-150">Adja meg a következő tulajdonságokkal:</span><span class="sxs-lookup"><span data-stu-id="74df7-150">Enter the following properties:</span></span>

    - <span data-ttu-id="74df7-151">**Előfizetés**: Adja meg az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="74df7-151">**Subscription**: Enter your Azure subscription.</span></span>
    - <span data-ttu-id="74df7-152">**Erőforráscsoport**: hozzon létre egy új Azure-erőforráscsoportot, vagy válasszon ki egy meglévő erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="74df7-152">**Resource Group**: Create a new Azure Resource Group, or select an existing Resource Group.</span></span>  <span data-ttu-id="74df7-153">Egy erőforráscsoport egy felügyeleti célból.</span><span class="sxs-lookup"><span data-stu-id="74df7-153">A Resource Group is for management purpose.</span></span>  <span data-ttu-id="74df7-154">Objektumok tárolója.</span><span class="sxs-lookup"><span data-stu-id="74df7-154">It is a container for objects.</span></span>
    - <span data-ttu-id="74df7-155">**Hely**: Válasszon ki egy régiót.</span><span class="sxs-lookup"><span data-stu-id="74df7-155">**Location**: Select a region.</span></span>
    - <span data-ttu-id="74df7-156">**ClusterName**: Adja meg a Hadoop-fürt nevét.</span><span class="sxs-lookup"><span data-stu-id="74df7-156">**ClusterName**: Enter a name for the Hadoop cluster.</span></span>
    - <span data-ttu-id="74df7-157">**A fürt bejelentkezési neve és jelszava**: Az alapértelmezett bejelentkezési név az admin.</span><span class="sxs-lookup"><span data-stu-id="74df7-157">**Cluster login name and password**: The default login name is admin.</span></span>
    - <span data-ttu-id="74df7-158">**SSH-felhasználónév és -jelszó**.</span><span class="sxs-lookup"><span data-stu-id="74df7-158">**SSH user name and password**.</span></span>
    - <span data-ttu-id="74df7-159">**SQL adatbázis-bejelentkezési nevet és jelszót**.</span><span class="sxs-lookup"><span data-stu-id="74df7-159">**SQL database server login name and password**.</span></span>
    - <span data-ttu-id="74df7-160">**hely _artifacts**: az alapértelmezett értéket használja, kivéve, ha egy másik helyen található saját backpac fájlt használja.</span><span class="sxs-lookup"><span data-stu-id="74df7-160">**_artifacts Location**: Use the default value unless you want to use your own backpac file in a different location.</span></span>
    - <span data-ttu-id="74df7-161">**hely Sas tokent _artifacts**: hagyja üresen a mezőt.</span><span class="sxs-lookup"><span data-stu-id="74df7-161">**_artifacts Location Sas Token**: Leave it blank.</span></span>
    - <span data-ttu-id="74df7-162">**Bacpac Fájlnév**: az alapértelmezett értéket használja, kivéve, ha szeretné használni a saját backpac fájlt.</span><span class="sxs-lookup"><span data-stu-id="74df7-162">**Bacpac File Name**: Use the default value unless you want to use your own backpac file.</span></span>
     
     <span data-ttu-id="74df7-163">A következő értékek a következők név szoftveresen kötött a sablonváltozók szakaszban:</span><span class="sxs-lookup"><span data-stu-id="74df7-163">The following values are hardcoded in the variables section:</span></span>
     
     | <span data-ttu-id="74df7-164">Alapértelmezett tárfiók neve</span><span class="sxs-lookup"><span data-stu-id="74df7-164">Default storage account name</span></span> | <span data-ttu-id="74df7-165"><CluterName>tároló</span><span class="sxs-lookup"><span data-stu-id="74df7-165"><CluterName>store</span></span> |
     | --- | --- |
     | <span data-ttu-id="74df7-166">Az Azure SQL adatbázis-kiszolgáló neve</span><span class="sxs-lookup"><span data-stu-id="74df7-166">Azure SQL database server name</span></span> |<span data-ttu-id="74df7-167"><ClusterName>dbserver</span><span class="sxs-lookup"><span data-stu-id="74df7-167"><ClusterName>dbserver</span></span> |
     | <span data-ttu-id="74df7-168">Az Azure SQL-adatbázis neve</span><span class="sxs-lookup"><span data-stu-id="74df7-168">Azure SQL database name</span></span> |<span data-ttu-id="74df7-169"><ClusterName>DB</span><span class="sxs-lookup"><span data-stu-id="74df7-169"><ClusterName>db</span></span> |
     
     <span data-ttu-id="74df7-170">Jegyezze fel ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="74df7-170">Please write down these values.</span></span>  <span data-ttu-id="74df7-171">Az oktatóanyag későbbi részében szüksége lesz rájuk.</span><span class="sxs-lookup"><span data-stu-id="74df7-171">You need them later in the tutorial.</span></span>

<span data-ttu-id="74df7-172">3. Kattintson az **OK** gombra a paraméterek mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="74df7-172">3.Click **OK** to save the parameters.</span></span>

<span data-ttu-id="74df7-173">4. A **Custom deployment** (Egyéni üzembe helyezés) panelen kattintson a **Resource group** (Erőforráscsoport) legördülő listára, majd a **New** (Új) lehetőségre egy új erőforráscsoport létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="74df7-173">4.From the **Custom deployment** blade, click **Resource group** dropdown box, and then click **New** to create a new resource group.</span></span> <span data-ttu-id="74df7-174">Az erőforráscsoport egy olyan tároló, amely csoportosítja a fürtöt, a függő tárfiókot és egyéb kapcsolt erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="74df7-174">The resource group is a container that groups the cluster, the dependent storage account and other linked resource.</span></span>

<span data-ttu-id="74df7-175">5. Kattintson a **Legal terms** (Jogi feltételek), majd a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="74df7-175">5.Click **Legal terms**, and then click **Create**.</span></span>

<span data-ttu-id="74df7-176">6. Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="74df7-176">6.Click **Create**.</span></span> <span data-ttu-id="74df7-177">Megjelenik egy új csempe jelenik meg Submitting deployment sablon központi telepítéshez.</span><span class="sxs-lookup"><span data-stu-id="74df7-177">You see a new tile titled Submitting deployment for Template deployment.</span></span> <span data-ttu-id="74df7-178">A fürt és az SQL-adatbázis létrehozása nagyjából 20 percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="74df7-178">It takes about around 20 minutes to create the cluster and SQL database.</span></span>

<span data-ttu-id="74df7-179">Ha úgy dönt, hogy a meglévő Azure SQL adatbázis vagy a Microsoft SQL Server használata</span><span class="sxs-lookup"><span data-stu-id="74df7-179">If you choose to use existing Azure SQL database or Microsoft SQL Server</span></span>

* <span data-ttu-id="74df7-180">**Az Azure SQL adatbázis**: konfigurálnia kell egy tűzfalszabályt az Azure SQL adatbázis-kiszolgáló számára engedélyezi a hozzáférést a munkaállomáson.</span><span class="sxs-lookup"><span data-stu-id="74df7-180">**Azure SQL database**: You must configure a firewall rule for the Azure SQL database server to allow access from your workstation.</span></span> <span data-ttu-id="74df7-181">Egy Azure SQL-adatbázis létrehozása, és a tűzfalon konfigurálásával kapcsolatos útmutatásért lásd: [Azure SQL-adatbázis használatának első][sqldatabase-get-started].</span><span class="sxs-lookup"><span data-stu-id="74df7-181">For instructions about creating an Azure SQL database and configuring the firewall, see [Get started using Azure SQL database][sqldatabase-get-started].</span></span> 
  
  > [!NOTE]
  > <span data-ttu-id="74df7-182">Alapértelmezés szerint az Azure SQL adatbázis Azure-szolgáltatások, például az Azure HDInsight kapcsolatokat engedélyez.</span><span class="sxs-lookup"><span data-stu-id="74df7-182">By default an Azure SQL database allows connections from Azure services, such as Azure HDInsight.</span></span> <span data-ttu-id="74df7-183">Ha a tűzfal beállítás le van tiltva, engedélyezze az Azure portálról szeretné.</span><span class="sxs-lookup"><span data-stu-id="74df7-183">If this firewall setting is disabled, you need to enable it from the Azure portal.</span></span> <span data-ttu-id="74df7-184">További információk az Azure SQL-adatbázis létrehozása és a tűzfalszabályok konfigurálása, lásd: [létrehozása és konfigurálása az SQL-adatbázis][sqldatabase-create-configue].</span><span class="sxs-lookup"><span data-stu-id="74df7-184">For instruction about creating an Azure SQL database and configuring firewall rules, see [Create and Configure SQL Database][sqldatabase-create-configue].</span></span>
  > 
  > 
* <span data-ttu-id="74df7-185">**SQL Server**: Ha a HDInsight-fürt ugyanazt a virtuális hálózatot az Azure SQL-kiszolgálóként, segítségével a lépéseket a cikkben adatok importálása és exportálása az SQL Server-adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="74df7-185">**SQL Server**: If your HDInsight cluster is on the same virtual network in Azure as SQL Server, you can use the steps in this article to import and export data to a SQL Server database.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="74df7-186">HDInsight támogatja a csak helyalapú virtuális hálózatokat, és jelenleg nem működik az affinitáscsoport-alapú virtuális hálózatokon.</span><span class="sxs-lookup"><span data-stu-id="74df7-186">HDInsight supports only location-based virtual networks, and it does not currently work with affinity group-based virtual networks.</span></span>
  > 
  > 
  
  * <span data-ttu-id="74df7-187">Hozzon létre, és egy virtuális hálózat konfigurálására, [hozzon létre egy virtuális hálózatot az Azure portál használatával](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).</span><span class="sxs-lookup"><span data-stu-id="74df7-187">To create and configure a virtual network, see [Create a virtual network using the Azure portal](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).</span></span>
    
    * <span data-ttu-id="74df7-188">SQL Server használatakor az adatközpontban található konfigurálnia kell a virtuális hálózaton: *pont-pont* vagy *pont-pont*.</span><span class="sxs-lookup"><span data-stu-id="74df7-188">When you are using SQL Server in your datacenter, you must configure the virtual network as *site-to-site* or *point-to-site*.</span></span>
      
      > [!NOTE]
      > <span data-ttu-id="74df7-189">A **pont-pont** virtuális hálózatok, az SQL Server futnia kell a VPN-ügyfél konfigurációs alkalmazás, amely elérhető a a **irányítópult** az Azure-beli virtuális hálózat konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="74df7-189">For **point-to-site** virtual networks, SQL Server must be running the VPN client configuration application, which is available from the **Dashboard** of your Azure virtual network configuration.</span></span>
      > 
      > 
    * <span data-ttu-id="74df7-190">Használatakor az SQL Server Azure virtuális géphez, virtuális hálózati konfigurációt is használható, ha a virtuális gépet üzemeltető SQL Server a HDInsight megegyező virtuális hálózatban tagja.</span><span class="sxs-lookup"><span data-stu-id="74df7-190">When you are using SQL Server on an Azure virtual machine, any virtual network configuration can be used if the virtual machine hosting SQL Server is a member of the same virtual network as HDInsight.</span></span>
  * <span data-ttu-id="74df7-191">HDInsight-fürtök létrehozása a virtuális hálózaton: [létrehozása Hadoop-fürtök a Hdinsightban egyéni beállításokkal](hdinsight-hadoop-provision-linux-clusters.md)</span><span class="sxs-lookup"><span data-stu-id="74df7-191">To create an HDInsight cluster on a virtual network, see [Create Hadoop clusters in HDInsight using custom options](hdinsight-hadoop-provision-linux-clusters.md)</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="74df7-192">SQL Server hitelesítési is lehetővé kell tenni.</span><span class="sxs-lookup"><span data-stu-id="74df7-192">SQL Server must also allow authentication.</span></span> <span data-ttu-id="74df7-193">Ebben a cikkben lépéseinek végrehajtásához egy SQL Server bejelentkezési fiókot kell használnia.</span><span class="sxs-lookup"><span data-stu-id="74df7-193">You must use a SQL Server login to complete the steps in this article.</span></span>
    > 
    > 

## <a name="run-sqoop-jobs"></a><span data-ttu-id="74df7-194">Sqoop feladatok futtatása</span><span class="sxs-lookup"><span data-stu-id="74df7-194">Run Sqoop jobs</span></span>
<span data-ttu-id="74df7-195">HDInsight Sqoop feladatok futtatásához számos módszer használatával.</span><span class="sxs-lookup"><span data-stu-id="74df7-195">HDInsight can run Sqoop jobs by using a variety of methods.</span></span> <span data-ttu-id="74df7-196">A következő táblázat segítségével döntse el, melyik módszert részesíti az Ön számára legmegfelelőbb, majd kövesse a hivatkozást útmutatást.</span><span class="sxs-lookup"><span data-stu-id="74df7-196">Use the following table to decide which method is right for you, then follow the link for a walkthrough.</span></span>

| <span data-ttu-id="74df7-197">**Ezzel** Ha azt szeretné...</span><span class="sxs-lookup"><span data-stu-id="74df7-197">**Use this** if you want...</span></span> | <span data-ttu-id="74df7-198">.. .an **interaktív** rendszerhéj</span><span class="sxs-lookup"><span data-stu-id="74df7-198">...an **interactive** shell</span></span> | <span data-ttu-id="74df7-199">... **kötegelt** feldolgozása</span><span class="sxs-lookup"><span data-stu-id="74df7-199">...**batch** processing</span></span> | <span data-ttu-id="74df7-200">és mivel ez **fürt operációs rendszer**</span><span class="sxs-lookup"><span data-stu-id="74df7-200">...with this **cluster operating system**</span></span> | <span data-ttu-id="74df7-201">.. .from ez **ügyfél operációs rendszer**</span><span class="sxs-lookup"><span data-stu-id="74df7-201">...from this **client operating system**</span></span> |
|:--- |:---:|:---:|:--- |:--- |
| [<span data-ttu-id="74df7-202">SSH</span><span class="sxs-lookup"><span data-stu-id="74df7-202">SSH</span></span>](hdinsight-use-sqoop-mac-linux.md) |<span data-ttu-id="74df7-203">✔</span><span class="sxs-lookup"><span data-stu-id="74df7-203">✔</span></span> |<span data-ttu-id="74df7-204">✔</span><span class="sxs-lookup"><span data-stu-id="74df7-204">✔</span></span> |<span data-ttu-id="74df7-205">Linux</span><span class="sxs-lookup"><span data-stu-id="74df7-205">Linux</span></span> |<span data-ttu-id="74df7-206">Linux, Unix, Mac OS X vagy Windows</span><span class="sxs-lookup"><span data-stu-id="74df7-206">Linux, Unix, Mac OS X, or Windows</span></span> |
| [<span data-ttu-id="74df7-207">.NET SDK a Hadoophoz</span><span class="sxs-lookup"><span data-stu-id="74df7-207">.NET SDK for Hadoop</span></span>](hdinsight-hadoop-use-sqoop-dotnet-sdk.md) |&nbsp; |<span data-ttu-id="74df7-208">✔</span><span class="sxs-lookup"><span data-stu-id="74df7-208">✔</span></span> |<span data-ttu-id="74df7-209">Linux- vagy Windows</span><span class="sxs-lookup"><span data-stu-id="74df7-209">Linux or Windows</span></span> |<span data-ttu-id="74df7-210">(Egyelőre) Windows</span><span class="sxs-lookup"><span data-stu-id="74df7-210">Windows (for now)</span></span> |
| [<span data-ttu-id="74df7-211">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="74df7-211">Azure PowerShell</span></span>](hdinsight-hadoop-use-sqoop-powershell.md) |&nbsp; |<span data-ttu-id="74df7-212">✔</span><span class="sxs-lookup"><span data-stu-id="74df7-212">✔</span></span> |<span data-ttu-id="74df7-213">Linux- vagy Windows</span><span class="sxs-lookup"><span data-stu-id="74df7-213">Linux or Windows</span></span> |<span data-ttu-id="74df7-214">Windows</span><span class="sxs-lookup"><span data-stu-id="74df7-214">Windows</span></span> |

## <a name="limitations"></a><span data-ttu-id="74df7-215">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="74df7-215">Limitations</span></span>
* <span data-ttu-id="74df7-216">Tömeges export - a Linux-alapú HDInsight, a Sqoop összekötő használt Microsoft SQL Server vagy az Azure SQL Database adatainak exportálása jelenleg nem támogatja a tömeges beszúrások.</span><span class="sxs-lookup"><span data-stu-id="74df7-216">Bulk export - With Linux-based HDInsight, the Sqoop connector used to export data to Microsoft SQL Server or Azure SQL Database does not currently support bulk inserts.</span></span>
* <span data-ttu-id="74df7-217">Kötegelés - és a Linux-alapú HDInsight együttes használata esetén a `-batch` beszúrása végrehajtásakor kapcsoló, a Sqoop több beszúrás helyett a beszúrási műveletek kötegelése hajt végre.</span><span class="sxs-lookup"><span data-stu-id="74df7-217">Batching - With Linux-based HDInsight, When using the `-batch` switch when performing inserts, Sqoop performs multiple inserts instead of batching the insert operations.</span></span>

## <a name="next-steps"></a><span data-ttu-id="74df7-218">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="74df7-218">Next steps</span></span>
<span data-ttu-id="74df7-219">Most megtanulhatta, hogyan használható a Sqoop.</span><span class="sxs-lookup"><span data-stu-id="74df7-219">Now you have learned how to use Sqoop.</span></span> <span data-ttu-id="74df7-220">További tudnivalókért lásd:</span><span class="sxs-lookup"><span data-stu-id="74df7-220">To learn more, see:</span></span>

* [<span data-ttu-id="74df7-221">A Hive használata a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="74df7-221">Use Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="74df7-222">A Pig használata a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="74df7-222">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* <span data-ttu-id="74df7-223">[Oozie használata a HDInsight][hdinsight-use-oozie]: egy Oozie munkafolyamat használja Sqoop műveletét.</span><span class="sxs-lookup"><span data-stu-id="74df7-223">[Use Oozie with HDInsight][hdinsight-use-oozie]: Use Sqoop action in an Oozie workflow.</span></span>
* <span data-ttu-id="74df7-224">[HDInsight eszközzel repülési késleltetés adatok elemzése][hdinsight-analyze-flight-data]: használja struktúra elemzése repülési késleltetés az adatok, és a Sqoop segítségével exportál adatokat az Azure SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="74df7-224">[Analyze flight delay data using HDInsight][hdinsight-analyze-flight-data]: Use Hive to analyze flight delay data, and then use Sqoop to export data to an Azure SQL database.</span></span>
* <span data-ttu-id="74df7-225">[Adatok feltöltése a HDInsight][hdinsight-upload-data]: található adatok feltöltése a HDInsight vagy az Azure Blob storage más módszerrel.</span><span class="sxs-lookup"><span data-stu-id="74df7-225">[Upload data to HDInsight][hdinsight-upload-data]: Find other methods for uploading data to HDInsight/Azure Blob storage.</span></span>

## <a name="appendix-a---a-powershell-sample"></a><span data-ttu-id="74df7-226">A melléklet – PowerShell-példa</span><span class="sxs-lookup"><span data-stu-id="74df7-226">Appendix A - a PowerShell sample</span></span>
<span data-ttu-id="74df7-227">A PowerShell-példa a következő lépéseket végzi el:</span><span class="sxs-lookup"><span data-stu-id="74df7-227">The PowerShell sample performs the following steps:</span></span>

1. <span data-ttu-id="74df7-228">Csatlakozás az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="74df7-228">Connect to Azure.</span></span>
2. <span data-ttu-id="74df7-229">Azure-erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="74df7-229">Create an Azure resource group.</span></span> <span data-ttu-id="74df7-230">További információkért lásd: [az Azure PowerShell használata Azure Resource Managerrel](../powershell-azure-resource-manager.md)</span><span class="sxs-lookup"><span data-stu-id="74df7-230">For more information, see [Using Azure PowerShell with Azure Resource Manager](../powershell-azure-resource-manager.md)</span></span>
3. <span data-ttu-id="74df7-231">Hozzon létre egy Azure SQL adatbázis-kiszolgáló, az Azure SQL-adatbázis és a két tábla.</span><span class="sxs-lookup"><span data-stu-id="74df7-231">Create an Azure SQL Database server, an Azure SQL database, and two tables.</span></span> 
   
    <span data-ttu-id="74df7-232">Ha ehelyett SQL Servert használja, a táblák létrehozásához használja az alábbi utasításokat:</span><span class="sxs-lookup"><span data-stu-id="74df7-232">If you use SQL Server instead, use the following statements to create the tables:</span></span>
   
        CREATE TABLE [dbo].[log4jlogs](
         [t1] [nvarchar](50),
         [t2] [nvarchar](50),
         [t3] [nvarchar](50),
         [t4] [nvarchar](50),
         [t5] [nvarchar](50),
         [t6] [nvarchar](50),
         [t7] [nvarchar](50))
   
        CREATE TABLE [dbo].[mobiledata](
         [clientid] [nvarchar](50),
         [querytime] [nvarchar](50),
         [market] [nvarchar](50),
         [deviceplatform] [nvarchar](50),
         [devicemake] [nvarchar](50),
         [devicemodel] [nvarchar](50),
         [state] [nvarchar](50),
         [country] [nvarchar](50),
         [querydwelltime] [float],
         [sessionid] [bigint],
         [sessionpagevieworder][bigint])
   
    <span data-ttu-id="74df7-233">A vizsgálata, az adatbázis és a táblák legkönnyebben Visual Studio használata.</span><span class="sxs-lookup"><span data-stu-id="74df7-233">The easiest way to examine the database and tables is to use Visual Studio.</span></span> <span data-ttu-id="74df7-234">Az adatbázis-kiszolgáló és az adatbázis megfelel az Azure portál használatával.</span><span class="sxs-lookup"><span data-stu-id="74df7-234">The database server and the database can be examined using the Azure portal.</span></span>
4. <span data-ttu-id="74df7-235">HDInsight-fürtök létrehozása.</span><span class="sxs-lookup"><span data-stu-id="74df7-235">Create an HDInsight cluster.</span></span>
   
    <span data-ttu-id="74df7-236">Vizsgálja meg a fürt, használhatja az Azure portálon vagy az Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="74df7-236">To examine the cluster, you can use the Azure portal or Azure PowerShell.</span></span>
5. <span data-ttu-id="74df7-237">Előre feldolgozzák a forrásadatfájlok.</span><span class="sxs-lookup"><span data-stu-id="74df7-237">Pre-process the source data file.</span></span>
   
    <span data-ttu-id="74df7-238">Ebben az oktatóanyagban exportálhatja egy log4j naplófájl (tagolt fájl) és a Hive tábla Azure SQL-adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="74df7-238">In this tutorial, you export a log4j log file (a delimited file) and a Hive table to an Azure SQL database.</span></span> <span data-ttu-id="74df7-239">A tagolt fájl neve */example/data/sample.log*.</span><span class="sxs-lookup"><span data-stu-id="74df7-239">The delimited file is called */example/data/sample.log*.</span></span> <span data-ttu-id="74df7-240">Az oktatóanyag korábbi részében látott néhány minták log4j naplók.</span><span class="sxs-lookup"><span data-stu-id="74df7-240">Earlier in the tutorial, you saw a few samples of log4j logs.</span></span> <span data-ttu-id="74df7-241">A naplófájlban van néhány üres sorok és az egyes sorok alábbiakhoz hasonló:</span><span class="sxs-lookup"><span data-stu-id="74df7-241">In the log file, there are some empty lines and some lines similar to these:</span></span>
   
        java.lang.Exception: 2012-02-03 20:11:35 SampleClass2 [FATAL] unrecoverable system problem at id 609774657
            at com.osa.mocklogger.MockLogger$2.run(MockLogger.java:83)
   
    <span data-ttu-id="74df7-242">Ennek megfelelően működik, ezeket az adatokat használó más példák, de azt el kell távolítania az ilyen kivételek ahhoz, hogy importálja az Azure SQL database vagy az SQL Server.</span><span class="sxs-lookup"><span data-stu-id="74df7-242">This is fine for other examples that use this data, but we must remove these exceptions before we can import into the Azure SQL database or SQL Server.</span></span> <span data-ttu-id="74df7-243">Sqoop exportálás sikertelen lesz, ha üres karakterlánc vagy egy kevesebb sort van definiálva az Azure SQL-adatbázistáblában szereplő mezők elemet.</span><span class="sxs-lookup"><span data-stu-id="74df7-243">Sqoop export  fails if there is an empty string or a line with a fewer elements than the number of fields defined in the Azure SQL database table.</span></span> <span data-ttu-id="74df7-244">A log4jlogs tábla 7 karakterlánc típusú mezőket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="74df7-244">The log4jlogs table has 7 string-type fields.</span></span>
   
    <span data-ttu-id="74df7-245">Ez az eljárás létrehoz egy új fájlt a fürt: tutorials/usesqoop/data/sample.log.</span><span class="sxs-lookup"><span data-stu-id="74df7-245">This procedure creates a new file on the cluster: tutorials/usesqoop/data/sample.log.</span></span> <span data-ttu-id="74df7-246">Tekintse meg a módosított adatok fájlt, használhatja az Azure-portálon, az Azure Storage explorer eszköz vagy az Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="74df7-246">To examine the modified data file, you can use the Azure portal, an Azure Storage explorer tool, or Azure PowerShell.</span></span> <span data-ttu-id="74df7-247">[Ismerkedés a HDInsight] [ hdinsight-get-started] rendelkezik egy példakód az Azure PowerShell használatával töltse le a fájlt, és megjeleníti a fájl tartalma.</span><span class="sxs-lookup"><span data-stu-id="74df7-247">[Get started with HDInsight][hdinsight-get-started] has a code sample for using Azure PowerShell to download a file and display the file content.</span></span>
6. <span data-ttu-id="74df7-248">Az Azure SQL-adatbázis egy adatfájlt exportálja.</span><span class="sxs-lookup"><span data-stu-id="74df7-248">Export a data file to the Azure SQL database.</span></span>
   
    <span data-ttu-id="74df7-249">A forrásfájl tutorials/usesqoop/data/sample.log.</span><span class="sxs-lookup"><span data-stu-id="74df7-249">The source file is tutorials/usesqoop/data/sample.log.</span></span> <span data-ttu-id="74df7-250">A tábla, ahol az adatok exportálása az log4jlogs nevezik.</span><span class="sxs-lookup"><span data-stu-id="74df7-250">The table where the data is exported to is called log4jlogs.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="74df7-251">Eltérő kapcsolati karakterlánc információt a jelen szakaszban szereplő lépéseket az Azure SQL-adatbázis, vagy az SQL Server kell működnie.</span><span class="sxs-lookup"><span data-stu-id="74df7-251">Other than connection string information, the steps in this section should work for an Azure SQL database or for SQL Server.</span></span> <span data-ttu-id="74df7-252">Ezeket a lépéseket a következő konfigurációval teszteltük:</span><span class="sxs-lookup"><span data-stu-id="74df7-252">These steps were tested by using the following configuration:</span></span>
   > 
   > * <span data-ttu-id="74df7-253">**Azure-beli virtuális hálózat pont-hely konfigurációs**: virtuális hálózat a HDInsight-fürthöz csatlakozik egy SQL Server egy privát adatközpontban.</span><span class="sxs-lookup"><span data-stu-id="74df7-253">**Azure virtual network point-to-site configuration**: A virtual network connected the HDInsight cluster to a SQL Server in a private datacenter.</span></span> <span data-ttu-id="74df7-254">Lásd: [pont-pont VPN konfigurálásához a kezelési portál](../vpn-gateway/vpn-gateway-point-to-site-create.md) további információt.</span><span class="sxs-lookup"><span data-stu-id="74df7-254">See [Configure a Point-to-Site VPN in the Management Portal](../vpn-gateway/vpn-gateway-point-to-site-create.md) for more information.</span></span>
   > * <span data-ttu-id="74df7-255">**Az Azure HDInsight 3.1**: lásd: [létrehozása Hadoop-fürtök a Hdinsightban egyéni beállításokkal](hdinsight-hadoop-provision-linux-clusters.md) információ a fürt létrehozása a virtuális hálózaton.</span><span class="sxs-lookup"><span data-stu-id="74df7-255">**Azure HDInsight 3.1**: See [Create Hadoop clusters in HDInsight using custom options](hdinsight-hadoop-provision-linux-clusters.md) for information about creating a cluster on a virtual network.</span></span>
   > * <span data-ttu-id="74df7-256">**Az SQL Server 2014**: konfigurált ahhoz, hogy hitelesítést és fut a VPN-ügyfél konfigurációs csomag biztonságosan a virtuális hálózathoz csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="74df7-256">**SQL Server 2014**: Configured to allow authentication and running the VPN client configuration package to connect securely to the virtual network.</span></span>
   > 
   > 
7. <span data-ttu-id="74df7-257">Hive tábla exportálása az Azure SQL adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="74df7-257">Export a Hive table to the Azure SQL database.</span></span>
8. <span data-ttu-id="74df7-258">A mobiledata tábla importálása a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="74df7-258">Import the mobiledata table to the HDInsight cluster.</span></span>
   
    <span data-ttu-id="74df7-259">Tekintse meg a módosított adatok fájlt, használhatja az Azure-portálon, az Azure Storage explorer eszköz vagy az Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="74df7-259">To examine the modified data file, you can use the Azure portal, an Azure Storage explorer tool, or Azure PowerShell.</span></span>  <span data-ttu-id="74df7-260">[Ismerkedés a HDInsight] [ hdinsight-get-started] rendelkezik egy kódminta Azure PowerShell használatával töltse le a fájlt, és megjeleníti a fájl tartalma.</span><span class="sxs-lookup"><span data-stu-id="74df7-260">[Get started with HDInsight][hdinsight-get-started] has a code sample about using Azure PowerShell to download a file and display the file content.</span></span>

### <a name="the-powershell-sample"></a><span data-ttu-id="74df7-261">A PowerShell-példa</span><span class="sxs-lookup"><span data-stu-id="74df7-261">The PowerShell sample</span></span>
    # Prepare an Azure SQL database to be used by the Sqoop tutorial

    #region - provide the following values

    $subscriptionID = "<Enter your Azure Subscription ID>"

    $sqlDatabaseLogin = "<Enter a SQL Database Login name>" #SQL Database server login
    $sqlDatabasePassword = "<Enter a Password>"

    $httpUserName = "admin"  #HDInsight cluster username
    $httpPassword = "<Enter a Password>"

    # used for creating Azure service names
    $nameToken = "<Enter an alias>" 
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")
    #endregion

    #region - variables

    # Resource group variables
    $resourceGroupName = $namePrefix + "rg"
    $location = "East US 2" # used by all Azure services defined in this tutorial

    # SQL database varialbes
    $sqlDatabaseServerName = $namePrefix + "sqldbserver"
    $sqlDatabaseName = $namePrefix + "sqldb"
    $sqlDatabaseConnectionString = "Data Source=$sqlDatabaseServerName.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
    $sqlDatabaseMaxSizeGB = 10

    # Used for retrieving external IP address and creating firewall rules
    $ipAddressRestService = "http://bot.whatismyipaddress.com"
    $fireWallRuleName = "UseSqoop"

    # Used for creating tables and clustered indexes
    $cmdCreateLog4jTable = "CREATE TABLE [dbo].[log4jlogs](
        [t1] [nvarchar](50),
        [t2] [nvarchar](50),
        [t3] [nvarchar](50),
        [t4] [nvarchar](50),
        [t5] [nvarchar](50),
        [t6] [nvarchar](50),
        [t7] [nvarchar](50))"

    $cmdCreateLog4jClusteredIndex = "CREATE CLUSTERED INDEX log4jlogs_clustered_index on log4jlogs(t1)"

    $cmdCreateMobileTable = " CREATE TABLE [dbo].[mobiledata](
    [clientid] [nvarchar](50),
    [querytime] [nvarchar](50),
    [market] [nvarchar](50),
    [deviceplatform] [nvarchar](50),
    [devicemake] [nvarchar](50),
    [devicemodel] [nvarchar](50),
    [state] [nvarchar](50),
    [country] [nvarchar](50),
    [querydwelltime] [float],
    [sessionid] [bigint],
    [sessionpagevieworder][bigint])"

    $cmdCreateMobileDataClusteredIndex = "CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(clientid)"

    # HDInsight variables
    $hdinsightClusterName = $namePrefix + "hdi"
    $defaultStorageAccountName = $namePrefix + "store"
    $defaultBlobContainerName = $hdinsightClusterName
    #endregion

    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"

    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #endregion

    #region - Create Azure resouce group
    Write-Host "`nCreating an Azure resource group ..." -ForegroundColor Green
    try{
        Get-AzureRmResourceGroup -Name $resourceGroupName
    }
    catch{
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    }
    #endregion

    #region - Create Azure SQL database server
    Write-Host "`nCreating an Azure SQL Database server ..." -ForegroundColor Green
    try{
        Get-AzureRmSqlServer -ServerName $sqlDatabaseServerName -ResourceGroupName $resourceGroupName}
    catch{
        Write-Host "`nCreating SQL Database server ..."  -ForegroundColor Green

        $sqlDatabasePW = ConvertTo-SecureString -String $sqlDatabasePassword -AsPlainText -Force
        $credential = New-Object System.Management.Automation.PSCredential($sqlDatabaseLogin,$sqlDatabasePW)

        $sqlDatabaseServerName = (New-AzureRmSqlServer `
                                    -ResourceGroupName $resourceGroupName `
                                    -ServerName $sqlDatabaseServerName `
                                    -SqlAdministratorCredentials $credential `
                                    -Location $location).ServerName
        Write-Host "`tThe new SQL database server name is $sqlDatabaseServerName." -ForegroundColor Cyan

        Write-Host "`nCreating firewall rule, $fireWallRuleName ..." -ForegroundColor Green
        $workstationIPAddress = Invoke-RestMethod $ipAddressRestService
        New-AzureRmSqlServerFirewallRule `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -FirewallRuleName "$fireWallRuleName-workstation" `
            -StartIpAddress $workstationIPAddress `
            -EndIpAddress $workstationIPAddress

        #To allow other Azure services to access the server add a firewall rule and set both the StartIpAddress and EndIpAddress to 0.0.0.0. 
        #Note that this allows Azure traffic from any Azure subscription to access the server.
        New-AzureRmSqlServerFirewallRule `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -FirewallRuleName "$fireWallRuleName-Azureservices" `
            -StartIpAddress "0.0.0.0" `
            -EndIpAddress "0.0.0.0"
    }

    #endregion

    #region - Create and validate Azure SQL database
    Write-Host "`nCreating an Azure SQL database ..." -ForegroundColor Green

    try {
        Get-AzureRmSqlDatabase `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -DatabaseName $sqlDatabaseName
    }
    catch {
        Write-Host "`nCreating SQL Database, $sqlDatabaseName ..."  -ForegroundColor Green
        New-AzureRMSqlDatabase `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -DatabaseName $sqlDatabaseName `
            -Edition "Standard" `
            -RequestedServiceObjectiveName "S1"
    }

    #endregion

    #region - Create tables
    Write-Host "Creating the log4jlogs table and the mobiledata table ..." -ForegroundColor Green

    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = $sqlDatabaseConnectionString
    $conn.Open()

    # Create the log4jlogs table and index
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.Connection = $conn
    $cmd.CommandText = $cmdCreateLog4jTable
    $ret = $cmd.ExecuteNonQuery()
    $cmd.CommandText = $cmdCreateLog4jClusteredIndex
    $cmd.ExecuteNonQuery()

    # Create the mobiledata table and index
    $cmd.CommandText = $cmdCreateMobileTable
    $cmd.ExecuteNonQuery()
    $cmd.CommandText = $cmdCreateMobileDataClusteredIndex
    $cmd.ExecuteNonQuery()

    $conn.close()

    #endregion


    #region - Create HDInsight cluster

    Write-Host "Creating the HDInsight cluster and the dependent services ..." -ForegroundColor Green

    # Create the default storage account
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Location $location `
        -Type Standard_LRS

    # Create the default Blob container
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext `
                                        -StorageAccountName $defaultStorageAccountName `
                                        -StorageAccountKey $defaultStorageAccountKey 
    New-AzureStorageContainer `
        -Name $defaultBlobContainerName `
        -Context $defaultStorageAccountContext 

    # Create the HDInsight cluster
    $pw = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)

    New-AzureRmHDInsightCluster `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $HDInsightClusterName `
        -Location $location `
        -ClusterType Hadoop `
        -OSType Windows `
        -ClusterSizeInNodes 2 `
        -HttpCredential $httpCredential `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $defaultBlobContainerName 

    # Validate the cluster
    Get-AzureRmHDInsightCluster -ClusterName $hdinsightClusterName
    #endregion

    #region - pre-process the source file

    Write-Host "Preprocessing the source file ..." -ForegroundColor Green

    # This procedure creates a new file with $destBlobName
    $sourceBlobName = "example/data/sample.log"
    $destBlobName = "tutorials/usesqoop/data/sample.log"

    # Define the connection string
    $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$defaultStorageAccountName;AccountKey=$defaultStorageAccountKey"

    # Create block blob objects referencing the source and destination blob.
    $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
    $storageClient = $storageAccount.CreateCloudBlobClient();
    $storageContainer = $storageClient.GetContainerReference($defaultBlobContainerName)
    $sourceBlob = $storageContainer.GetBlockBlobReference($sourceBlobName)
    $destBlob = $storageContainer.GetBlockBlobReference($destBlobName)

    # Define a MemoryStream and a StreamReader for reading from the source file
    $stream = New-Object System.IO.MemoryStream
    $stream = $sourceBlob.OpenRead()
    $sReader = New-Object System.IO.StreamReader($stream)

    # Define a MemoryStream and a StreamWriter for writing into the destination file
    $memStream = New-Object System.IO.MemoryStream
    $writeStream = New-Object System.IO.StreamWriter $memStream

    # Pre-process the source blob
    $exString = "java.lang.Exception:"
    while(-Not $sReader.EndOfStream){
        $line = $sReader.ReadLine()
        $split = $line.Split(" ")

        # remove the "java.lang.Exception" from the first element of the array
        # for example: java.lang.Exception: 2012-02-03 19:11:02 SampleClass8 [WARN] problem finding id 153454612
        if ($split[0] -eq $exString){
            #create a new ArrayList to remove $split[0]
            $newArray = [System.Collections.ArrayList] $split
            $newArray.Remove($exString)

            # update $split and $line
            $split = $newArray
            $line = $newArray -join(" ")
        }

        # remove the lines that has less than 7 elements
        if ($split.count -ge 7){
            write-host $line
            $writeStream.WriteLine($line)
        }
    }

    # Write to the destination blob
    $writeStream.Flush()
    $memStream.Seek(0, "Begin")
    $destBlob.UploadFromStream($memStream)

    #endregion

    #region - export a log file from the cluster to the SQL database

    Write-Host "Preprocessing the source file ..." -ForegroundColor Green

    $tableName_log4j = "log4jlogs"

    # Connection string for Azure SQL Database.
    # Comment if using SQL Server
    $connectionString = "jdbc:sqlserver://$sqlDatabaseServerName.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServerName;password=$sqlDatabasePassword;database=$sqlDatabaseName"
    # Connection string for SQL Server.
    # Uncomment if using SQL Server.
    #$connectionString = "jdbc:sqlserver://$sqlDatabaseServerName;user=$sqlDatabaseLogin;password=$sqlDatabasePassword;database=$sqlDatabaseName"

    $exportDir_log4j = "/tutorials/usesqoop/data"

    # Submit a Sqoop job
    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "export --connect $connectionString --table $tableName_log4j --export-dir $exportDir_log4j --input-fields-terminated-by \0x20 -m 1"
    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose
    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId

    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -HttpCredential $httpCredential -JobId $sqoopJob.JobId -DisplayOutputType StandardError
    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -HttpCredential $httpCredential -JobId $sqoopJob.JobId -DisplayOutputType StandardOutput

    #endregion

    #region - export a Hive table

    $tableName_mobile = "mobiledata"
    $exportDir_mobile = "/hive/warehouse/hivesampletable"

    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "export --connect $connectionString --table $tableName_mobile --export-dir $exportDir_mobile --fields-terminated-by \t -m 1"
    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose

    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId

    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardError

    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardOutput

    #endregion

    #region - import a database

    $targetDir_mobile = "/tutorials/usesqoop/importeddata/"

    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "import --connect $connectionString --table $tableName_mobile --target-dir $targetDir_mobile --fields-terminated-by \t --lines-terminated-by \n -m 1"

    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose

    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId

    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardError

    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardOutput

    #endregion



[azure-management-portal]: https://portal.azure.com/

[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[sqldatabase-get-started]: ../sql-database/sql-database-get-started.md
[sqldatabase-create-configue]: ../sql-database/sql-database-get-started.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: /powershell/azureps-cmdlets-docs
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html
