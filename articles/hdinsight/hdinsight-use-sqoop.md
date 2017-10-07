---
title: az Azure HDInsight (Hadoop) feladatok aaaRun Apache Sqoop |} Microsoft Docs
description: "Ismerje meg, hogyan egy munkaállomás toorun Sqoop az Azure PowerShell toouse importálása és exportálása egy Hadoop-fürt és az Azure SQL-adatbázis között."
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
ms.openlocfilehash: bdac507704937d77921c9c13d70aa2434c7e3be4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-sqoop-with-hadoop-in-hdinsight"></a><span data-ttu-id="87327-103">Sqoop használata a hadooppal a Hdinsightban</span><span class="sxs-lookup"><span data-stu-id="87327-103">Use Sqoop with Hadoop in HDInsight</span></span>
[!INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

<span data-ttu-id="87327-104">Megtudhatja, hogyan toouse Sqoop HDInsight tooimport és exportálás a HDInsight-fürt és Azure SQL database vagy az SQL Server-adatbázis között.</span><span class="sxs-lookup"><span data-stu-id="87327-104">Learn how toouse Sqoop in HDInsight tooimport and export between HDInsight cluster and Azure SQL database or SQL Server database.</span></span>

<span data-ttu-id="87327-105">Hadoop természetes téve a feldolgozása a strukturálatlan és félig strukturált adatok, például a naplókat, valamint fájlt, azonban a is lehet a szükséges strukturált tooprocess relációs adatbázisokban tárolt adatokat.</span><span class="sxs-lookup"><span data-stu-id="87327-105">Although Hadoop is a natural choice for processing unstructured and semistructured data, such as logs and files, there may also be a need tooprocess structured data that is stored in relational databases.</span></span>

<span data-ttu-id="87327-106">[Sqoop] [ sqoop-user-guide-1.4.4] van egy eszköz tootransfer adatokat Hadoop-fürtök és a relációs adatbázisok között.</span><span class="sxs-lookup"><span data-stu-id="87327-106">[Sqoop][sqoop-user-guide-1.4.4] is a tool designed tootransfer data between Hadoop clusters and relational databases.</span></span> <span data-ttu-id="87327-107">Használható a relációs adatbázis-kezelő rendszerének (RDBMS) tooimport adatokat például SQL Server, MySQL vagy hello Hadoop elosztott fájlrendszer (HDFS), az Oracle hello adatok a Hadoop MapReduce vagy a Hive, majd exportálja újra hello adatok egy RDBMS.</span><span class="sxs-lookup"><span data-stu-id="87327-107">You can use it tooimport data from a relational database management system (RDBMS) such as SQL Server, MySQL, or Oracle into hello Hadoop distributed file system (HDFS), transform hello data in Hadoop with MapReduce or Hive, and then export hello data back into an RDBMS.</span></span> <span data-ttu-id="87327-108">Ebben az oktatóanyagban egy SQL Server adatbázist használ a relációs adatbázis.</span><span class="sxs-lookup"><span data-stu-id="87327-108">In this tutorial, you are using a SQL Server database for your relational database.</span></span>

<span data-ttu-id="87327-109">A HDInsight-fürtökön támogatott Sqoop verzióiért lásd: [What's new in HDInsight által biztosított hello fürt verziók?][hdinsight-versions]</span><span class="sxs-lookup"><span data-stu-id="87327-109">For Sqoop versions that are supported on HDInsight clusters, see [What's new in hello cluster versions provided by HDInsight?][hdinsight-versions]</span></span>

## <a name="understand-hello-scenario"></a><span data-ttu-id="87327-110">Hello forgatókönyv ismertetése</span><span class="sxs-lookup"><span data-stu-id="87327-110">Understand hello scenario</span></span>

<span data-ttu-id="87327-111">HDInsight-fürtök néhány adatot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="87327-111">HDInsight cluster comes with some sample data.</span></span> <span data-ttu-id="87327-112">A következő két minta hello használhatja:</span><span class="sxs-lookup"><span data-stu-id="87327-112">You use hello following two samples:</span></span>

* <span data-ttu-id="87327-113">A log4j naplófájlt, amely itt található: */example/data/sample.log*.</span><span class="sxs-lookup"><span data-stu-id="87327-113">A log4j log file, which is located at */example/data/sample.log*.</span></span> <span data-ttu-id="87327-114">a következő naplók hello kinyert hello fájlt:</span><span class="sxs-lookup"><span data-stu-id="87327-114">hello following logs are extracted from hello file:</span></span>
  
        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...
* <span data-ttu-id="87327-115">Nevű Hive tábla *hivesampletable*, amely hivatkozások hello adatok fájlba */hive/warehouse/hivesampletable*.</span><span class="sxs-lookup"><span data-stu-id="87327-115">A Hive table named *hivesampletable*, which references hello data file located at */hive/warehouse/hivesampletable*.</span></span> <span data-ttu-id="87327-116">hello táblázat tartalmaz néhány mobileszköz adat.</span><span class="sxs-lookup"><span data-stu-id="87327-116">hello table contains some mobile device data.</span></span> 
  
  | <span data-ttu-id="87327-117">Mező</span><span class="sxs-lookup"><span data-stu-id="87327-117">Field</span></span> | <span data-ttu-id="87327-118">Adattípus</span><span class="sxs-lookup"><span data-stu-id="87327-118">Data type</span></span> |
  | --- | --- |
  | <span data-ttu-id="87327-119">ClientID</span><span class="sxs-lookup"><span data-stu-id="87327-119">clientid</span></span> |<span data-ttu-id="87327-120">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="87327-120">string</span></span> |
  | <span data-ttu-id="87327-121">querytime</span><span class="sxs-lookup"><span data-stu-id="87327-121">querytime</span></span> |<span data-ttu-id="87327-122">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="87327-122">string</span></span> |
  | <span data-ttu-id="87327-123">piaci</span><span class="sxs-lookup"><span data-stu-id="87327-123">market</span></span> |<span data-ttu-id="87327-124">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="87327-124">string</span></span> |
  | <span data-ttu-id="87327-125">deviceplatform</span><span class="sxs-lookup"><span data-stu-id="87327-125">deviceplatform</span></span> |<span data-ttu-id="87327-126">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="87327-126">string</span></span> |
  | <span data-ttu-id="87327-127">devicemake</span><span class="sxs-lookup"><span data-stu-id="87327-127">devicemake</span></span> |<span data-ttu-id="87327-128">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="87327-128">string</span></span> |
  | <span data-ttu-id="87327-129">devicemodel</span><span class="sxs-lookup"><span data-stu-id="87327-129">devicemodel</span></span> |<span data-ttu-id="87327-130">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="87327-130">string</span></span> |
  | <span data-ttu-id="87327-131">state</span><span class="sxs-lookup"><span data-stu-id="87327-131">state</span></span> |<span data-ttu-id="87327-132">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="87327-132">string</span></span> |
  | <span data-ttu-id="87327-133">Ország</span><span class="sxs-lookup"><span data-stu-id="87327-133">country</span></span> |<span data-ttu-id="87327-134">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="87327-134">string</span></span> |
  | <span data-ttu-id="87327-135">querydwelltime</span><span class="sxs-lookup"><span data-stu-id="87327-135">querydwelltime</span></span> |<span data-ttu-id="87327-136">Dupla</span><span class="sxs-lookup"><span data-stu-id="87327-136">double</span></span> |
  | <span data-ttu-id="87327-137">munkamenet-azonosító</span><span class="sxs-lookup"><span data-stu-id="87327-137">sessionid</span></span> |<span data-ttu-id="87327-138">bigint</span><span class="sxs-lookup"><span data-stu-id="87327-138">bigint</span></span> |
  | <span data-ttu-id="87327-139">sessionpagevieworder</span><span class="sxs-lookup"><span data-stu-id="87327-139">sessionpagevieworder</span></span> |<span data-ttu-id="87327-140">bigint</span><span class="sxs-lookup"><span data-stu-id="87327-140">bigint</span></span> |

<span data-ttu-id="87327-141">Először exportálnia *sample.log* és *hivesampletable* toohello Azure SQL-adatbázist vagy kiszolgáló tooSQL, majd hello mobileszköz adat tartalmazó hello tábla importálása biztonsági tooHDInsight hello használatával következő elérési úton:</span><span class="sxs-lookup"><span data-stu-id="87327-141">First, you export *sample.log* and *hivesampletable* toohello Azure SQL database or tooSQL Server, and then import hello table that contains hello mobile device data back tooHDInsight by using hello following path:</span></span>

    /tutorials/usesqoop/importeddata

## <a name="create-cluster-and-sql-database"></a><span data-ttu-id="87327-142">Fürt és az SQL-adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="87327-142">Create cluster and SQL database</span></span>
<span data-ttu-id="87327-143">Ez a szakasz bemutatja, hogyan toocreate fürt SQL-adatbázis, hello SQL adatbázis-sémák és futó hello oktatóanyag használatára vonatkozó hello Azure-portál és az Azure Resource Manager-sablon.</span><span class="sxs-lookup"><span data-stu-id="87327-143">This section shows you how toocreate a cluster, a SQL Database, and hello SQL database schemas for running hello tutorial using hello Azure portal and an Azure Resource Manager template.</span></span> <span data-ttu-id="87327-144">hello sablon található [Azure gyors üzembe helyezési sablonokat](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-with-sql-database/).</span><span class="sxs-lookup"><span data-stu-id="87327-144">hello template can be found in [Azure QuickStart Templates](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-with-sql-database/).</span></span> <span data-ttu-id="87327-145">hello Resource Manager-sablon toodeploy hello tábla sémái tooSQL adatbázis bacpac csomag hívja.</span><span class="sxs-lookup"><span data-stu-id="87327-145">hello Resource Manager template calls a bacpac package toodeploy hello table schemas tooSQL database.</span></span>  <span data-ttu-id="87327-146">a következő nyilvános blobtárolóban https://hditutorialdata.blob.core.windows.net/usesqoop/SqoopTutorial-2016-2-23-11-2.bacpac hello bacpac csomag található.</span><span class="sxs-lookup"><span data-stu-id="87327-146">hello bacpac package is located in a public blob container, https://hditutorialdata.blob.core.windows.net/usesqoop/SqoopTutorial-2016-2-23-11-2.bacpac.</span></span> <span data-ttu-id="87327-147">Egy személyes tárolót toouse hello bacpac-fájlok, használja a következő értékek hello sablonban hello:</span><span class="sxs-lookup"><span data-stu-id="87327-147">If you want toouse a private container for hello bacpac files, use hello following values in hello template:</span></span>
   
        "storageKeyType": "Primary",
        "storageKey": "<TheAzureStorageAccountKey>",

<span data-ttu-id="87327-148">Ha jobban szeret toouse Azure PowerShell toocreate hello fürt és hello SQL-adatbázis, lásd: [függelék](#appendix-a---a-powershell-sample).</span><span class="sxs-lookup"><span data-stu-id="87327-148">If you prefer toouse Azure PowerShell toocreate hello cluster and hello SQL Database, see [Appendix A](#appendix-a---a-powershell-sample).</span></span>

1. <span data-ttu-id="87327-149">Kattintson a következő kép tooopen hello Azure-portálon a Resource Manager sablon hello.</span><span class="sxs-lookup"><span data-stu-id="87327-149">Click hello following image tooopen a Resource Manager template in hello Azure portal.</span></span>         
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-with-sql-database%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-use-sqoop/deploy-to-azure.png" alt="Deploy tooAzure"></a>
   

2. <span data-ttu-id="87327-150">Adja meg a következő tulajdonságai hello:</span><span class="sxs-lookup"><span data-stu-id="87327-150">Enter hello following properties:</span></span>

    - <span data-ttu-id="87327-151">**Előfizetés**: Adja meg az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="87327-151">**Subscription**: Enter your Azure subscription.</span></span>
    - <span data-ttu-id="87327-152">**Erőforráscsoport**: hozzon létre egy új Azure-erőforráscsoportot, vagy válasszon ki egy meglévő erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="87327-152">**Resource Group**: Create a new Azure Resource Group, or select an existing Resource Group.</span></span>  <span data-ttu-id="87327-153">Egy erőforráscsoport egy felügyeleti célból.</span><span class="sxs-lookup"><span data-stu-id="87327-153">A Resource Group is for management purpose.</span></span>  <span data-ttu-id="87327-154">Objektumok tárolója.</span><span class="sxs-lookup"><span data-stu-id="87327-154">It is a container for objects.</span></span>
    - <span data-ttu-id="87327-155">**Hely**: Válasszon ki egy régiót.</span><span class="sxs-lookup"><span data-stu-id="87327-155">**Location**: Select a region.</span></span>
    - <span data-ttu-id="87327-156">**ClusterName**: hello Hadoop-fürt nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="87327-156">**ClusterName**: Enter a name for hello Hadoop cluster.</span></span>
    - <span data-ttu-id="87327-157">**A fürt bejelentkezési nevet és jelszót**: hello alapértelmezett bejelentkezési név az admin.</span><span class="sxs-lookup"><span data-stu-id="87327-157">**Cluster login name and password**: hello default login name is admin.</span></span>
    - <span data-ttu-id="87327-158">**SSH-felhasználónév és -jelszó**.</span><span class="sxs-lookup"><span data-stu-id="87327-158">**SSH user name and password**.</span></span>
    - <span data-ttu-id="87327-159">**SQL adatbázis-bejelentkezési nevet és jelszót**.</span><span class="sxs-lookup"><span data-stu-id="87327-159">**SQL database server login name and password**.</span></span>
    - <span data-ttu-id="87327-160">**hely _artifacts**: hello alapértelmezett értéket használja, kivéve, ha szeretné, hogy toouse saját backpac fájl egy másik helyen.</span><span class="sxs-lookup"><span data-stu-id="87327-160">**_artifacts Location**: Use hello default value unless you want toouse your own backpac file in a different location.</span></span>
    - <span data-ttu-id="87327-161">**hely Sas tokent _artifacts**: hagyja üresen a mezőt.</span><span class="sxs-lookup"><span data-stu-id="87327-161">**_artifacts Location Sas Token**: Leave it blank.</span></span>
    - <span data-ttu-id="87327-162">**Bacpac Fájlnév**: hello alapértelmezett értéket használja, kivéve, ha szeretné, hogy toouse saját backpac fájl.</span><span class="sxs-lookup"><span data-stu-id="87327-162">**Bacpac File Name**: Use hello default value unless you want toouse your own backpac file.</span></span>
     
     <span data-ttu-id="87327-163">a következő értékek hello szoftveresen kötött hello változók szakaszban:</span><span class="sxs-lookup"><span data-stu-id="87327-163">hello following values are hardcoded in hello variables section:</span></span>
     
     | <span data-ttu-id="87327-164">Alapértelmezett tárfiók neve</span><span class="sxs-lookup"><span data-stu-id="87327-164">Default storage account name</span></span> | <span data-ttu-id="87327-165"><CluterName>tároló</span><span class="sxs-lookup"><span data-stu-id="87327-165"><CluterName>store</span></span> |
     | --- | --- |
     | <span data-ttu-id="87327-166">Az Azure SQL adatbázis-kiszolgáló neve</span><span class="sxs-lookup"><span data-stu-id="87327-166">Azure SQL database server name</span></span> |<span data-ttu-id="87327-167"><ClusterName>dbserver</span><span class="sxs-lookup"><span data-stu-id="87327-167"><ClusterName>dbserver</span></span> |
     | <span data-ttu-id="87327-168">Az Azure SQL-adatbázis neve</span><span class="sxs-lookup"><span data-stu-id="87327-168">Azure SQL database name</span></span> |<span data-ttu-id="87327-169"><ClusterName>DB</span><span class="sxs-lookup"><span data-stu-id="87327-169"><ClusterName>db</span></span> |
     
     <span data-ttu-id="87327-170">Jegyezze fel ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="87327-170">Please write down these values.</span></span>  <span data-ttu-id="87327-171">Már szükség hello oktatóanyag későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="87327-171">You need them later in hello tutorial.</span></span>

<span data-ttu-id="87327-172">3. Kattintson **OK** toosave hello paraméterek.</span><span class="sxs-lookup"><span data-stu-id="87327-172">3.Click **OK** toosave hello parameters.</span></span>

<span data-ttu-id="87327-173">4 a hello **egyéni központi telepítés** panelen kattintson **erőforráscsoport** legördülő mezőben, és kattintson **új** toocreate egy új erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="87327-173">4.From hello **Custom deployment** blade, click **Resource group** dropdown box, and then click **New** toocreate a new resource group.</span></span> <span data-ttu-id="87327-174">hello erőforráscsoport egy olyan tároló, amely csoportosítja hello fürtöt, hello függő tárfiókot és egyéb kapcsolt erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="87327-174">hello resource group is a container that groups hello cluster, hello dependent storage account and other linked resource.</span></span>

<span data-ttu-id="87327-175">5. Kattintson a **Legal terms** (Jogi feltételek), majd a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="87327-175">5.Click **Legal terms**, and then click **Create**.</span></span>

<span data-ttu-id="87327-176">6. Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="87327-176">6.Click **Create**.</span></span> <span data-ttu-id="87327-177">Megjelenik egy új csempe jelenik meg Submitting deployment sablon központi telepítéshez.</span><span class="sxs-lookup"><span data-stu-id="87327-177">You see a new tile titled Submitting deployment for Template deployment.</span></span> <span data-ttu-id="87327-178">Körülbelül 20 percet toocreate hello fürt és az SQL-adatbázis vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="87327-178">It takes about around 20 minutes toocreate hello cluster and SQL database.</span></span>

<span data-ttu-id="87327-179">Ha úgy dönt, hogy toouse meglévő Azure SQL adatbázis vagy a Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="87327-179">If you choose toouse existing Azure SQL database or Microsoft SQL Server</span></span>

* <span data-ttu-id="87327-180">**Az Azure SQL adatbázis**: hello Azure SQL adatbázis-kiszolgáló tooallow hozzáférési egy tűzfalszabályt konfigurálnia kell a munkaállomáson.</span><span class="sxs-lookup"><span data-stu-id="87327-180">**Azure SQL database**: You must configure a firewall rule for hello Azure SQL database server tooallow access from your workstation.</span></span> <span data-ttu-id="87327-181">Egy Azure SQL-adatbázis létrehozása, és hello tűzfal konfigurálásával kapcsolatos útmutatásért lásd: [Azure SQL-adatbázis használatának első][sqldatabase-get-started].</span><span class="sxs-lookup"><span data-stu-id="87327-181">For instructions about creating an Azure SQL database and configuring hello firewall, see [Get started using Azure SQL database][sqldatabase-get-started].</span></span> 
  
  > [!NOTE]
  > <span data-ttu-id="87327-182">Alapértelmezés szerint az Azure SQL adatbázis Azure-szolgáltatások, például az Azure HDInsight kapcsolatokat engedélyez.</span><span class="sxs-lookup"><span data-stu-id="87327-182">By default an Azure SQL database allows connections from Azure services, such as Azure HDInsight.</span></span> <span data-ttu-id="87327-183">Ha a tűzfal beállítás le van tiltva, akkor kell-e tooenable azt hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="87327-183">If this firewall setting is disabled, you need tooenable it from hello Azure portal.</span></span> <span data-ttu-id="87327-184">További információk az Azure SQL-adatbázis létrehozása és a tűzfalszabályok konfigurálása, lásd: [létrehozása és konfigurálása az SQL-adatbázis][sqldatabase-create-configue].</span><span class="sxs-lookup"><span data-stu-id="87327-184">For instruction about creating an Azure SQL database and configuring firewall rules, see [Create and Configure SQL Database][sqldatabase-create-configue].</span></span>
  > 
  > 
* <span data-ttu-id="87327-185">**SQL Server**: Ha a HDInsight-fürt hello ugyanazt a virtuális hálózatot az Azure SQL-kiszolgálóként, lépésekkel hello Ez a cikk tooimport és exportálási adatok tooa SQL Server adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="87327-185">**SQL Server**: If your HDInsight cluster is on hello same virtual network in Azure as SQL Server, you can use hello steps in this article tooimport and export data tooa SQL Server database.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="87327-186">HDInsight támogatja a csak helyalapú virtuális hálózatokat, és jelenleg nem működik az affinitáscsoport-alapú virtuális hálózatokon.</span><span class="sxs-lookup"><span data-stu-id="87327-186">HDInsight supports only location-based virtual networks, and it does not currently work with affinity group-based virtual networks.</span></span>
  > 
  > 
  
  * <span data-ttu-id="87327-187">toocreate és virtuális hálózat konfigurálása, lásd: [hello Azure-portál virtuális hálózat létrehozása](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).</span><span class="sxs-lookup"><span data-stu-id="87327-187">toocreate and configure a virtual network, see [Create a virtual network using hello Azure portal](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).</span></span>
    
    * <span data-ttu-id="87327-188">SQL Server használatakor az adatközpontban található konfigurálnia kell a virtuális hálózatban hello *pont-pont* vagy *pont-pont*.</span><span class="sxs-lookup"><span data-stu-id="87327-188">When you are using SQL Server in your datacenter, you must configure hello virtual network as *site-to-site* or *point-to-site*.</span></span>
      
      > [!NOTE]
      > <span data-ttu-id="87327-189">A **pont-pont** virtuális hálózatok, az SQL Server futnia kell az hello VPN-ügyfél konfigurációs alkalmazás, amely elérhető a hello **irányítópult** az Azure-beli virtuális hálózat konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="87327-189">For **point-to-site** virtual networks, SQL Server must be running hello VPN client configuration application, which is available from hello **Dashboard** of your Azure virtual network configuration.</span></span>
      > 
      > 
    * <span data-ttu-id="87327-190">Használatakor az SQL Server Azure virtuális géphez, virtuális hálózati konfigurációt használható, ha hello virtuális gépet üzemeltető SQL Server hello tagja HDInsight megegyező virtuális hálózatban.</span><span class="sxs-lookup"><span data-stu-id="87327-190">When you are using SQL Server on an Azure virtual machine, any virtual network configuration can be used if hello virtual machine hosting SQL Server is a member of hello same virtual network as HDInsight.</span></span>
  * <span data-ttu-id="87327-191">a virtuális hálózaton, HDInsight-fürtök toocreate lásd: [létrehozása Hadoop-fürtök a Hdinsightban egyéni beállításokkal](hdinsight-hadoop-provision-linux-clusters.md)</span><span class="sxs-lookup"><span data-stu-id="87327-191">toocreate an HDInsight cluster on a virtual network, see [Create Hadoop clusters in HDInsight using custom options](hdinsight-hadoop-provision-linux-clusters.md)</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="87327-192">SQL Server hitelesítési is lehetővé kell tenni.</span><span class="sxs-lookup"><span data-stu-id="87327-192">SQL Server must also allow authentication.</span></span> <span data-ttu-id="87327-193">Bejelentkezési toocomplete hello ebben a cikkben ismertetett visszaállítási lépésekkel SQL-kiszolgálót kell használnia.</span><span class="sxs-lookup"><span data-stu-id="87327-193">You must use a SQL Server login toocomplete hello steps in this article.</span></span>
    > 
    > 

## <a name="run-sqoop-jobs"></a><span data-ttu-id="87327-194">Sqoop feladatok futtatása</span><span class="sxs-lookup"><span data-stu-id="87327-194">Run Sqoop jobs</span></span>
<span data-ttu-id="87327-195">HDInsight Sqoop feladatok futtatásához számos módszer használatával.</span><span class="sxs-lookup"><span data-stu-id="87327-195">HDInsight can run Sqoop jobs by using a variety of methods.</span></span> <span data-ttu-id="87327-196">Használja a következő tábla toodecide, melyik módszert részesíti az Ön számára legmegfelelőbb hello, majd kövesse az útmutatást hello hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="87327-196">Use hello following table toodecide which method is right for you, then follow hello link for a walkthrough.</span></span>

| <span data-ttu-id="87327-197">**Ezzel** Ha azt szeretné...</span><span class="sxs-lookup"><span data-stu-id="87327-197">**Use this** if you want...</span></span> | <span data-ttu-id="87327-198">.. .an **interaktív** rendszerhéj</span><span class="sxs-lookup"><span data-stu-id="87327-198">...an **interactive** shell</span></span> | <span data-ttu-id="87327-199">... **kötegelt** feldolgozása</span><span class="sxs-lookup"><span data-stu-id="87327-199">...**batch** processing</span></span> | <span data-ttu-id="87327-200">és mivel ez **fürt operációs rendszer**</span><span class="sxs-lookup"><span data-stu-id="87327-200">...with this **cluster operating system**</span></span> | <span data-ttu-id="87327-201">.. .from ez **ügyfél operációs rendszer**</span><span class="sxs-lookup"><span data-stu-id="87327-201">...from this **client operating system**</span></span> |
|:--- |:---:|:---:|:--- |:--- |
| [<span data-ttu-id="87327-202">SSH</span><span class="sxs-lookup"><span data-stu-id="87327-202">SSH</span></span>](hdinsight-use-sqoop-mac-linux.md) |<span data-ttu-id="87327-203">✔</span><span class="sxs-lookup"><span data-stu-id="87327-203">✔</span></span> |<span data-ttu-id="87327-204">✔</span><span class="sxs-lookup"><span data-stu-id="87327-204">✔</span></span> |<span data-ttu-id="87327-205">Linux</span><span class="sxs-lookup"><span data-stu-id="87327-205">Linux</span></span> |<span data-ttu-id="87327-206">Linux, Unix, Mac OS X vagy Windows</span><span class="sxs-lookup"><span data-stu-id="87327-206">Linux, Unix, Mac OS X, or Windows</span></span> |
| [<span data-ttu-id="87327-207">.NET SDK a Hadoophoz</span><span class="sxs-lookup"><span data-stu-id="87327-207">.NET SDK for Hadoop</span></span>](hdinsight-hadoop-use-sqoop-dotnet-sdk.md) |&nbsp; |<span data-ttu-id="87327-208">✔</span><span class="sxs-lookup"><span data-stu-id="87327-208">✔</span></span> |<span data-ttu-id="87327-209">Linux- vagy Windows</span><span class="sxs-lookup"><span data-stu-id="87327-209">Linux or Windows</span></span> |<span data-ttu-id="87327-210">(Egyelőre) Windows</span><span class="sxs-lookup"><span data-stu-id="87327-210">Windows (for now)</span></span> |
| [<span data-ttu-id="87327-211">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="87327-211">Azure PowerShell</span></span>](hdinsight-hadoop-use-sqoop-powershell.md) |&nbsp; |<span data-ttu-id="87327-212">✔</span><span class="sxs-lookup"><span data-stu-id="87327-212">✔</span></span> |<span data-ttu-id="87327-213">Linux- vagy Windows</span><span class="sxs-lookup"><span data-stu-id="87327-213">Linux or Windows</span></span> |<span data-ttu-id="87327-214">Windows</span><span class="sxs-lookup"><span data-stu-id="87327-214">Windows</span></span> |

## <a name="limitations"></a><span data-ttu-id="87327-215">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="87327-215">Limitations</span></span>
* <span data-ttu-id="87327-216">Tömeges export - a Linux-alapú HDInsight, hello Sqoop használt tooexport adatok tooMicrosoft SQL Server vagy az Azure SQL Database jelenleg nem támogatja a tömeges beszúrások.</span><span class="sxs-lookup"><span data-stu-id="87327-216">Bulk export - With Linux-based HDInsight, hello Sqoop connector used tooexport data tooMicrosoft SQL Server or Azure SQL Database does not currently support bulk inserts.</span></span>
* <span data-ttu-id="87327-217">Kötegelés – a Linux-alapú hdinsight eszközzel, hello használatakor `-batch` Beszúrások végrehajtásakor kapcsoló, a Sqoop hajt végre több Beszúrások hello beszúrási műveletek kötegelése helyett.</span><span class="sxs-lookup"><span data-stu-id="87327-217">Batching - With Linux-based HDInsight, When using hello `-batch` switch when performing inserts, Sqoop performs multiple inserts instead of batching hello insert operations.</span></span>

## <a name="next-steps"></a><span data-ttu-id="87327-218">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="87327-218">Next steps</span></span>
<span data-ttu-id="87327-219">Most megtanulta, hogyan toouse Sqoop.</span><span class="sxs-lookup"><span data-stu-id="87327-219">Now you have learned how toouse Sqoop.</span></span> <span data-ttu-id="87327-220">toolearn több, lásd:</span><span class="sxs-lookup"><span data-stu-id="87327-220">toolearn more, see:</span></span>

* [<span data-ttu-id="87327-221">A Hive használata a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="87327-221">Use Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="87327-222">A Pig használata a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="87327-222">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* <span data-ttu-id="87327-223">[Oozie használata a HDInsight][hdinsight-use-oozie]: egy Oozie munkafolyamat használja Sqoop műveletét.</span><span class="sxs-lookup"><span data-stu-id="87327-223">[Use Oozie with HDInsight][hdinsight-use-oozie]: Use Sqoop action in an Oozie workflow.</span></span>
* <span data-ttu-id="87327-224">[HDInsight eszközzel repülési késleltetés adatok elemzése][hdinsight-analyze-flight-data]: tooanalyze felhőszolgáltató közötti átviteléhez használja Hive késleltetés az adatok, és majd a Sqoop tooexport tooan Azure SQL-adatbázis használata.</span><span class="sxs-lookup"><span data-stu-id="87327-224">[Analyze flight delay data using HDInsight][hdinsight-analyze-flight-data]: Use Hive tooanalyze flight delay data, and then use Sqoop tooexport data tooan Azure SQL database.</span></span>
* <span data-ttu-id="87327-225">[Töltse fel az adatok tooHDInsight][hdinsight-upload-data]: található adatok tooHDInsight vagy az Azure Blob storage feltöltése más módszerrel.</span><span class="sxs-lookup"><span data-stu-id="87327-225">[Upload data tooHDInsight][hdinsight-upload-data]: Find other methods for uploading data tooHDInsight/Azure Blob storage.</span></span>

## <a name="appendix-a---a-powershell-sample"></a><span data-ttu-id="87327-226">A melléklet – PowerShell-példa</span><span class="sxs-lookup"><span data-stu-id="87327-226">Appendix A - a PowerShell sample</span></span>
<span data-ttu-id="87327-227">hello PowerShell-példa hello a következő lépéseket végzi el:</span><span class="sxs-lookup"><span data-stu-id="87327-227">hello PowerShell sample performs hello following steps:</span></span>

1. <span data-ttu-id="87327-228">Csatlakozás tooAzure.</span><span class="sxs-lookup"><span data-stu-id="87327-228">Connect tooAzure.</span></span>
2. <span data-ttu-id="87327-229">Azure-erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="87327-229">Create an Azure resource group.</span></span> <span data-ttu-id="87327-230">További információkért lásd: [az Azure PowerShell használata Azure Resource Managerrel](../powershell-azure-resource-manager.md)</span><span class="sxs-lookup"><span data-stu-id="87327-230">For more information, see [Using Azure PowerShell with Azure Resource Manager](../powershell-azure-resource-manager.md)</span></span>
3. <span data-ttu-id="87327-231">Hozzon létre egy Azure SQL adatbázis-kiszolgáló, az Azure SQL-adatbázis és a két tábla.</span><span class="sxs-lookup"><span data-stu-id="87327-231">Create an Azure SQL Database server, an Azure SQL database, and two tables.</span></span> 
   
    <span data-ttu-id="87327-232">Ha ehelyett SQL Servert használja, használja a következő utasítások toocreate hello táblák hello:</span><span class="sxs-lookup"><span data-stu-id="87327-232">If you use SQL Server instead, use hello following statements toocreate hello tables:</span></span>
   
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
   
    <span data-ttu-id="87327-233">hello legegyszerűbb módja tooexamine hello adatbázis és a táblák nem toouse Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="87327-233">hello easiest way tooexamine hello database and tables is toouse Visual Studio.</span></span> <span data-ttu-id="87327-234">hello adatbázis és adatbázis-kiszolgáló hello megvizsgálhatók hello Azure-portál használatával.</span><span class="sxs-lookup"><span data-stu-id="87327-234">hello database server and hello database can be examined using hello Azure portal.</span></span>
4. <span data-ttu-id="87327-235">HDInsight-fürtök létrehozása.</span><span class="sxs-lookup"><span data-stu-id="87327-235">Create an HDInsight cluster.</span></span>
   
    <span data-ttu-id="87327-236">tooexamine hello fürt, használhatja hello Azure-portálon vagy az Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="87327-236">tooexamine hello cluster, you can use hello Azure portal or Azure PowerShell.</span></span>
5. <span data-ttu-id="87327-237">Előre feldolgozzák a hello forrásadatfájlok.</span><span class="sxs-lookup"><span data-stu-id="87327-237">Pre-process hello source data file.</span></span>
   
    <span data-ttu-id="87327-238">Ebben az oktatóanyagban kell exportálnia a log4j naplófájl (tagolt fájl), és a Hive tábla tooan Azure SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="87327-238">In this tutorial, you export a log4j log file (a delimited file) and a Hive table tooan Azure SQL database.</span></span> <span data-ttu-id="87327-239">hello tagolt fájl neve */example/data/sample.log*.</span><span class="sxs-lookup"><span data-stu-id="87327-239">hello delimited file is called */example/data/sample.log*.</span></span> <span data-ttu-id="87327-240">Hello az oktatóanyagban a korábbi látott néhány minták log4j naplók.</span><span class="sxs-lookup"><span data-stu-id="87327-240">Earlier in hello tutorial, you saw a few samples of log4j logs.</span></span> <span data-ttu-id="87327-241">Hello naplófájlban van néhány üres sorok és az egyes sorok hasonló toothese:</span><span class="sxs-lookup"><span data-stu-id="87327-241">In hello log file, there are some empty lines and some lines similar toothese:</span></span>
   
        java.lang.Exception: 2012-02-03 20:11:35 SampleClass2 [FATAL] unrecoverable system problem at id 609774657
            at com.osa.mocklogger.MockLogger$2.run(MockLogger.java:83)
   
    <span data-ttu-id="87327-242">Ennek megfelelően működik, ezeket az adatokat használó más példák, de azt előtt el kell távolítani ezeket a kivételeket azt importálnia hello Azure SQL database vagy az SQL Server.</span><span class="sxs-lookup"><span data-stu-id="87327-242">This is fine for other examples that use this data, but we must remove these exceptions before we can import into hello Azure SQL database or SQL Server.</span></span> <span data-ttu-id="87327-243">Sqoop exportálás sikertelen lesz, ha van egy üres karakterlánc vagy egy kevesebb sort hello Azure SQL adatbázis táblázatban definiált mezők hello száma elemet.</span><span class="sxs-lookup"><span data-stu-id="87327-243">Sqoop export  fails if there is an empty string or a line with a fewer elements than hello number of fields defined in hello Azure SQL database table.</span></span> <span data-ttu-id="87327-244">hello log4jlogs táblához 7 karakterlánc típusú mezőket.</span><span class="sxs-lookup"><span data-stu-id="87327-244">hello log4jlogs table has 7 string-type fields.</span></span>
   
    <span data-ttu-id="87327-245">Ez az eljárás létrehoz egy új fájlt hello fürt: tutorials/usesqoop/data/sample.log.</span><span class="sxs-lookup"><span data-stu-id="87327-245">This procedure creates a new file on hello cluster: tutorials/usesqoop/data/sample.log.</span></span> <span data-ttu-id="87327-246">tooexamine hello módosított adatfájlt, használhat hello Azure-portálon, az Azure Storage explorer eszköz vagy az Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="87327-246">tooexamine hello modified data file, you can use hello Azure portal, an Azure Storage explorer tool, or Azure PowerShell.</span></span> <span data-ttu-id="87327-247">[Ismerkedés a HDInsight] [ hdinsight-get-started] minta az Azure PowerShell toodownload egy fájl segítségével, és megjeleníti a hello fájl tartalma kódot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="87327-247">[Get started with HDInsight][hdinsight-get-started] has a code sample for using Azure PowerShell toodownload a file and display hello file content.</span></span>
6. <span data-ttu-id="87327-248">Exportálja az adatokat fájl toohello Azure SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="87327-248">Export a data file toohello Azure SQL database.</span></span>
   
    <span data-ttu-id="87327-249">hello forrásfájl tutorials/usesqoop/data/sample.log.</span><span class="sxs-lookup"><span data-stu-id="87327-249">hello source file is tutorials/usesqoop/data/sample.log.</span></span> <span data-ttu-id="87327-250">ahol hello adatok az exportált toois hello tábla log4jlogs nevezik.</span><span class="sxs-lookup"><span data-stu-id="87327-250">hello table where hello data is exported toois called log4jlogs.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="87327-251">Eltérő kapcsolati karakterlánc információt ebben a szakaszban található lépéseket hello Azure SQL-adatbázis, vagy az SQL Server kell működnie.</span><span class="sxs-lookup"><span data-stu-id="87327-251">Other than connection string information, hello steps in this section should work for an Azure SQL database or for SQL Server.</span></span> <span data-ttu-id="87327-252">Ezeket a lépéseket teszteltük hello konfiguráció a következő használatával:</span><span class="sxs-lookup"><span data-stu-id="87327-252">These steps were tested by using hello following configuration:</span></span>
   > 
   > * <span data-ttu-id="87327-253">**Azure-beli virtuális hálózat pont-hely konfigurációs**: egy virtuális hálózathoz csatlakozó hello HDInsight fürt tooa SQL Server egy privát adatközpontban.</span><span class="sxs-lookup"><span data-stu-id="87327-253">**Azure virtual network point-to-site configuration**: A virtual network connected hello HDInsight cluster tooa SQL Server in a private datacenter.</span></span> <span data-ttu-id="87327-254">Lásd: [pont-pont VPN konfigurálásához a kezelési portál hello](../vpn-gateway/vpn-gateway-point-to-site-create.md) további információt.</span><span class="sxs-lookup"><span data-stu-id="87327-254">See [Configure a Point-to-Site VPN in hello Management Portal](../vpn-gateway/vpn-gateway-point-to-site-create.md) for more information.</span></span>
   > * <span data-ttu-id="87327-255">**Az Azure HDInsight 3.1**: lásd: [létrehozása Hadoop-fürtök a Hdinsightban egyéni beállításokkal](hdinsight-hadoop-provision-linux-clusters.md) információ a fürt létrehozása a virtuális hálózaton.</span><span class="sxs-lookup"><span data-stu-id="87327-255">**Azure HDInsight 3.1**: See [Create Hadoop clusters in HDInsight using custom options](hdinsight-hadoop-provision-linux-clusters.md) for information about creating a cluster on a virtual network.</span></span>
   > * <span data-ttu-id="87327-256">**Az SQL Server 2014**: tooallow hitelesítési és futó hello VPN ügyfél konfigurációs csomag tooconnect biztonságosan konfigurált virtuális hálózati toohello.</span><span class="sxs-lookup"><span data-stu-id="87327-256">**SQL Server 2014**: Configured tooallow authentication and running hello VPN client configuration package tooconnect securely toohello virtual network.</span></span>
   > 
   > 
7. <span data-ttu-id="87327-257">Exportálja a Hive tábla toohello Azure SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="87327-257">Export a Hive table toohello Azure SQL database.</span></span>
8. <span data-ttu-id="87327-258">Importálja a hello mobiledata tábla toohello HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="87327-258">Import hello mobiledata table toohello HDInsight cluster.</span></span>
   
    <span data-ttu-id="87327-259">tooexamine hello módosított adatfájlt, használhat hello Azure-portálon, az Azure Storage explorer eszköz vagy az Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="87327-259">tooexamine hello modified data file, you can use hello Azure portal, an Azure Storage explorer tool, or Azure PowerShell.</span></span>  <span data-ttu-id="87327-260">[Ismerkedés a HDInsight] [ hdinsight-get-started] Azure PowerShell toodownload fájl használatáról sample és hello fájl tartalom megjelenítése egy kódot.</span><span class="sxs-lookup"><span data-stu-id="87327-260">[Get started with HDInsight][hdinsight-get-started] has a code sample about using Azure PowerShell toodownload a file and display hello file content.</span></span>

### <a name="hello-powershell-sample"></a><span data-ttu-id="87327-261">hello PowerShell-példa</span><span class="sxs-lookup"><span data-stu-id="87327-261">hello PowerShell sample</span></span>
    # Prepare an Azure SQL database toobe used by hello Sqoop tutorial

    #region - provide hello following values

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

    #region - Connect tooAzure subscription
    Write-Host "`nConnecting tooyour Azure subscription ..." -ForegroundColor Green
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

        #tooallow other Azure services tooaccess hello server add a firewall rule and set both hello StartIpAddress and EndIpAddress too0.0.0.0. 
        #Note that this allows Azure traffic from any Azure subscription tooaccess hello server.
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
    Write-Host "Creating hello log4jlogs table and hello mobiledata table ..." -ForegroundColor Green

    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = $sqlDatabaseConnectionString
    $conn.Open()

    # Create hello log4jlogs table and index
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.Connection = $conn
    $cmd.CommandText = $cmdCreateLog4jTable
    $ret = $cmd.ExecuteNonQuery()
    $cmd.CommandText = $cmdCreateLog4jClusteredIndex
    $cmd.ExecuteNonQuery()

    # Create hello mobiledata table and index
    $cmd.CommandText = $cmdCreateMobileTable
    $cmd.ExecuteNonQuery()
    $cmd.CommandText = $cmdCreateMobileDataClusteredIndex
    $cmd.ExecuteNonQuery()

    $conn.close()

    #endregion


    #region - Create HDInsight cluster

    Write-Host "Creating hello HDInsight cluster and hello dependent services ..." -ForegroundColor Green

    # Create hello default storage account
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Location $location `
        -Type Standard_LRS

    # Create hello default Blob container
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext `
                                        -StorageAccountName $defaultStorageAccountName `
                                        -StorageAccountKey $defaultStorageAccountKey 
    New-AzureStorageContainer `
        -Name $defaultBlobContainerName `
        -Context $defaultStorageAccountContext 

    # Create hello HDInsight cluster
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

    # Validate hello cluster
    Get-AzureRmHDInsightCluster -ClusterName $hdinsightClusterName
    #endregion

    #region - pre-process hello source file

    Write-Host "Preprocessing hello source file ..." -ForegroundColor Green

    # This procedure creates a new file with $destBlobName
    $sourceBlobName = "example/data/sample.log"
    $destBlobName = "tutorials/usesqoop/data/sample.log"

    # Define hello connection string
    $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$defaultStorageAccountName;AccountKey=$defaultStorageAccountKey"

    # Create block blob objects referencing hello source and destination blob.
    $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
    $storageClient = $storageAccount.CreateCloudBlobClient();
    $storageContainer = $storageClient.GetContainerReference($defaultBlobContainerName)
    $sourceBlob = $storageContainer.GetBlockBlobReference($sourceBlobName)
    $destBlob = $storageContainer.GetBlockBlobReference($destBlobName)

    # Define a MemoryStream and a StreamReader for reading from hello source file
    $stream = New-Object System.IO.MemoryStream
    $stream = $sourceBlob.OpenRead()
    $sReader = New-Object System.IO.StreamReader($stream)

    # Define a MemoryStream and a StreamWriter for writing into hello destination file
    $memStream = New-Object System.IO.MemoryStream
    $writeStream = New-Object System.IO.StreamWriter $memStream

    # Pre-process hello source blob
    $exString = "java.lang.Exception:"
    while(-Not $sReader.EndOfStream){
        $line = $sReader.ReadLine()
        $split = $line.Split(" ")

        # remove hello "java.lang.Exception" from hello first element of hello array
        # for example: java.lang.Exception: 2012-02-03 19:11:02 SampleClass8 [WARN] problem finding id 153454612
        if ($split[0] -eq $exString){
            #create a new ArrayList tooremove $split[0]
            $newArray = [System.Collections.ArrayList] $split
            $newArray.Remove($exString)

            # update $split and $line
            $split = $newArray
            $line = $newArray -join(" ")
        }

        # remove hello lines that has less than 7 elements
        if ($split.count -ge 7){
            write-host $line
            $writeStream.WriteLine($line)
        }
    }

    # Write toohello destination blob
    $writeStream.Flush()
    $memStream.Seek(0, "Begin")
    $destBlob.UploadFromStream($memStream)

    #endregion

    #region - export a log file from hello cluster toohello SQL database

    Write-Host "Preprocessing hello source file ..." -ForegroundColor Green

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
