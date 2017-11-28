---
title: "Hdinsight Hadoop Oozie coordinator időalapú használata |} Microsoft Docs"
description: "Időalapú Hadoop Oozie-koordinátor használata a Hdinsightban, big data-szolgáltatása. Megtudhatja, hogyan Oozie munkafolyamatok és a koordinátor és a feladatok elküldéséhez."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 00c3a395-d51a-44ff-af2d-1f116c4b1c83
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: 600a70c74a16e2601a874f804ac2e8382c8bfa90
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="use-time-based-oozie-coordinator-with-hadoop-in-hdinsight-to-define-workflows-and-coordinate-jobs"></a><span data-ttu-id="4cc23-104">A HDInsight Hadoop határozza meg a munkafolyamatok és feladatok időalapú Oozie-koordinátor használata</span><span class="sxs-lookup"><span data-stu-id="4cc23-104">Use time-based Oozie coordinator with Hadoop in HDInsight to define workflows and coordinate jobs</span></span>
<span data-ttu-id="4cc23-105">Ebből a cikkből megtudhatja, hogyan adhat meg a munkafolyamatok és a koordinátor, és hogyan indítható el, a koordinátor feladatok, ideje alapján.</span><span class="sxs-lookup"><span data-stu-id="4cc23-105">In this article, you'll learn how to define workflows and coordinators, and how to trigger the coordinator jobs, based on time.</span></span> <span data-ttu-id="4cc23-106">Hasznos lehet, hogy a [és a HDInsight együttes használata Oozie] [ hdinsight-use-oozie] Ez a cikk olvasása előtt.</span><span class="sxs-lookup"><span data-stu-id="4cc23-106">It is helpful to go through [Use Oozie with HDInsight][hdinsight-use-oozie] before you read this article.</span></span> <span data-ttu-id="4cc23-107">Oozie, mellett is ütemezheti az Azure Data Factory használatával feladatok.</span><span class="sxs-lookup"><span data-stu-id="4cc23-107">In addition to Oozie, you can also schedule jobs using Azure Data Factory.</span></span> <span data-ttu-id="4cc23-108">Azure Data Factory kapcsolatban [használja a Pig és a Data Factory Hive](../data-factory/data-factory-data-transformation-activities.md).</span><span class="sxs-lookup"><span data-stu-id="4cc23-108">To learn Azure Data Factory, see [Use Pig and Hive with Data Factory](../data-factory/data-factory-data-transformation-activities.md).</span></span>

> [!NOTE]
> <span data-ttu-id="4cc23-109">Ez a cikk egy Windows-alapú HDInsight-fürt szükséges.</span><span class="sxs-lookup"><span data-stu-id="4cc23-109">This article requires a Windows-based HDInsight cluster.</span></span> <span data-ttu-id="4cc23-110">Oozie, többek között a feladatok időalapú egy Linux-alapú fürtön használatáról lásd: [hadooppal határozza meg, és egy munkafolyamat futtatása a Linux-alapú HDInsight használata Oozie](hdinsight-use-oozie-linux-mac.md)</span><span class="sxs-lookup"><span data-stu-id="4cc23-110">For information on using Oozie, including time-based jobs, on a Linux-based cluster, see [Use Oozie with Hadoop to define and run a workflow on Linux-based HDInsight](hdinsight-use-oozie-linux-mac.md)</span></span>

## <a name="what-is-oozie"></a><span data-ttu-id="4cc23-111">Mi az az Oozie</span><span class="sxs-lookup"><span data-stu-id="4cc23-111">What is Oozie</span></span>
<span data-ttu-id="4cc23-112">Apache Oozie egy munkafolyamat/koordinációs rendszer, amely a Hadoop-feladatokat kezeli.</span><span class="sxs-lookup"><span data-stu-id="4cc23-112">Apache Oozie is a workflow/coordination system that manages Hadoop jobs.</span></span> <span data-ttu-id="4cc23-113">Integrálva van a Hadoop-veremmel, és támogatja a Hadoop-feladatokat Apache MapReduce, Apache Pig, Apache Hive és Apache Sqoop.</span><span class="sxs-lookup"><span data-stu-id="4cc23-113">It is integrated with the Hadoop stack, and it supports Hadoop jobs for Apache MapReduce, Apache Pig, Apache Hive, and Apache Sqoop.</span></span> <span data-ttu-id="4cc23-114">Is használható, például Java programok vagy héjparancsfájlok ütemezésére rendszerspecifikus feladatok ütemezése.</span><span class="sxs-lookup"><span data-stu-id="4cc23-114">It can also be used to schedule jobs that are specific to a system, such as Java programs or shell scripts.</span></span>

<span data-ttu-id="4cc23-115">A következő kép bemutatja a munkafolyamat fogja végrehajtani:</span><span class="sxs-lookup"><span data-stu-id="4cc23-115">The following image shows the workflow you will implement:</span></span>

![Munkafolyamat diagramja][img-workflow-diagram]

<span data-ttu-id="4cc23-117">A munkafolyamat két műveletet tartalmaz:</span><span class="sxs-lookup"><span data-stu-id="4cc23-117">The workflow contains two actions:</span></span>

1. <span data-ttu-id="4cc23-118">A Hive művelet a log4j naplófájl naplózási szintű típusonkénti előfordulások megszámlálásához HiveQL parancsfájlt futtatja.</span><span class="sxs-lookup"><span data-stu-id="4cc23-118">A Hive action runs a HiveQL script to count the occurrences of each log-level type in a log4j log file.</span></span> <span data-ttu-id="4cc23-119">A mezők típusát és súlyosságát, például megjelenítése [NAPLÓZÁSI szint] mező tartalmazó sor minden log4j napló foglalja magában:</span><span class="sxs-lookup"><span data-stu-id="4cc23-119">Each log4j log consists of a line of fields that contains a [LOG LEVEL] field to show the type and the severity, for example:</span></span>

        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...

    <span data-ttu-id="4cc23-120">A Hive parancsfájl kimenete hasonló:</span><span class="sxs-lookup"><span data-stu-id="4cc23-120">The Hive script output is similar to:</span></span>

        [DEBUG] 434
        [ERROR] 3
        [FATAL] 1
        [INFO]  96
        [TRACE] 816
        [WARN]  4

    <span data-ttu-id="4cc23-121">További információ a Hive-ról: [A Hive használata a HDInsightban][hdinsight-use-hive].</span><span class="sxs-lookup"><span data-stu-id="4cc23-121">For more information about Hive, see [Use Hive with HDInsight][hdinsight-use-hive].</span></span>
2. <span data-ttu-id="4cc23-122">A Sqoop művelet exportálja a HiveQL műveleti kimenet egy Azure SQL adatbázis egyik táblája.</span><span class="sxs-lookup"><span data-stu-id="4cc23-122">A Sqoop action exports the HiveQL action output to a table in an Azure SQL database.</span></span> <span data-ttu-id="4cc23-123">További információ a Sqoop: [és a HDInsight együttes használata Sqoop][hdinsight-use-sqoop].</span><span class="sxs-lookup"><span data-stu-id="4cc23-123">For more information about Sqoop, see [Use Sqoop with HDInsight][hdinsight-use-sqoop].</span></span>

> [!NOTE]
> <span data-ttu-id="4cc23-124">Tekintse meg a HDInsight-fürtökön támogatott Oozie verziók [What's new in HDInsight által biztosított fürt verziók?] [hdinsight-versions].</span><span class="sxs-lookup"><span data-stu-id="4cc23-124">For supported Oozie versions on HDInsight clusters, see [What's new in the cluster versions provided by HDInsight?][hdinsight-versions].</span></span>
>
>

## <a name="prerequisites"></a><span data-ttu-id="4cc23-125">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="4cc23-125">Prerequisites</span></span>
<span data-ttu-id="4cc23-126">Az oktatóanyag elkezdéséhez az alábbiakkal kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="4cc23-126">Before you begin this tutorial, you must have the following:</span></span>

* <span data-ttu-id="4cc23-127">**Munkaállomás Azure PowerShell-lel**.</span><span class="sxs-lookup"><span data-stu-id="4cc23-127">**A workstation with Azure PowerShell**.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="4cc23-128">A HDInsight-erőforrások Azure Service Managerrel történő kezelésének Azure PowerShell-támogatása **elavult**, így 2017. január 1-től megszűnik.</span><span class="sxs-lookup"><span data-stu-id="4cc23-128">Azure PowerShell support for managing HDInsight resources using Azure Service Manager is **deprecated**, and will be removed by January 1, 2017.</span></span> <span data-ttu-id="4cc23-129">A jelen dokumentumban leírt lépések az új HDInsight-parancsmagokat használják, amelyek az Azure Resource Managerrel működnek.</span><span class="sxs-lookup"><span data-stu-id="4cc23-129">The steps in this document use the new HDInsight cmdlets that work with Azure Resource Manager.</span></span>
    >
    > <span data-ttu-id="4cc23-130">Kérjük, kövesse az alábbi cikkben leírt lépéseket az Azure PowerShell legújabb verziójának telepítéséhez: [Install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) (Az Azure PowerShell letöltése és konfigurálása).</span><span class="sxs-lookup"><span data-stu-id="4cc23-130">Please follow the steps in [Install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) to install the latest version of Azure PowerShell.</span></span> <span data-ttu-id="4cc23-131">Ha vannak olyan parancsprogramjai, amelyeket módosítani kell az új, az Azure Resource Managerrel működő parancsmagok használatához, tekintse meg az alábbi cikket: [Migrating to Azure Resource Manager-based development tools for HDInsight clusters](hdinsight-hadoop-development-using-azure-resource-manager.md) (Az Azure Resource Manager-alapú fejlesztési eszközökre való áttérés HDInsight-fürtök esetén).</span><span class="sxs-lookup"><span data-stu-id="4cc23-131">If you have scripts that need to be modified to use the new cmdlets that work with Azure Resource Manager, see [Migrating to Azure Resource Manager-based development tools for HDInsight clusters](hdinsight-hadoop-development-using-azure-resource-manager.md) for more information.</span></span>

* <span data-ttu-id="4cc23-132">**HDInsight-fürtök**.</span><span class="sxs-lookup"><span data-stu-id="4cc23-132">**An HDInsight cluster**.</span></span> <span data-ttu-id="4cc23-133">HDInsight-fürtök létrehozásával kapcsolatos további információkért lásd: [HDInsight-fürtök létrehozása][hdinsight-provision], vagy [első lépései a hdinsight eszközzel][hdinsight-get-started].</span><span class="sxs-lookup"><span data-stu-id="4cc23-133">For information about creating an HDInsight cluster, see [Create HDInsight clusters][hdinsight-provision], or [Get started with HDInsight][hdinsight-get-started].</span></span> <span data-ttu-id="4cc23-134">Szüksége lesz az oktatóanyag végrehajtania az alábbi adatokat:</span><span class="sxs-lookup"><span data-stu-id="4cc23-134">You will need the following data to go through the tutorial:</span></span>

    <table border = "1">
    <tr><th><span data-ttu-id="4cc23-135">Fürt tulajdonság</span><span class="sxs-lookup"><span data-stu-id="4cc23-135">Cluster property</span></span></th><th><span data-ttu-id="4cc23-136">A Windows PowerShell-változó neve</span><span class="sxs-lookup"><span data-stu-id="4cc23-136">Windows PowerShell variable name</span></span></th><th><span data-ttu-id="4cc23-137">Érték</span><span class="sxs-lookup"><span data-stu-id="4cc23-137">Value</span></span></th><th><span data-ttu-id="4cc23-138">Leírás</span><span class="sxs-lookup"><span data-stu-id="4cc23-138">Description</span></span></th></tr>
    <tr><td><span data-ttu-id="4cc23-139">A HDInsight-fürt neve</span><span class="sxs-lookup"><span data-stu-id="4cc23-139">HDInsight cluster name</span></span></td><td><span data-ttu-id="4cc23-140">$clusterName</span><span class="sxs-lookup"><span data-stu-id="4cc23-140">$clusterName</span></span></td><td></td><td><span data-ttu-id="4cc23-141">A HDInsight-fürt amelyre ebben az oktatóanyagban fog futni.</span><span class="sxs-lookup"><span data-stu-id="4cc23-141">The HDInsight cluster on which you will run this tutorial.</span></span></td></tr>
    <tr><td><span data-ttu-id="4cc23-142">A HDInsight fürt felhasználónév</span><span class="sxs-lookup"><span data-stu-id="4cc23-142">HDInsight cluster username</span></span></td><td><span data-ttu-id="4cc23-143">$clusterUsername</span><span class="sxs-lookup"><span data-stu-id="4cc23-143">$clusterUsername</span></span></td><td></td><td><span data-ttu-id="4cc23-144">A HDInsight fürt felhasználó felhasználóneve.</span><span class="sxs-lookup"><span data-stu-id="4cc23-144">The HDInsight cluster user name.</span></span> </td></tr>
    <tr><td><span data-ttu-id="4cc23-145">A HDInsight fürt felhasználói jelszó</span><span class="sxs-lookup"><span data-stu-id="4cc23-145">HDInsight cluster user password</span></span> </td><td><span data-ttu-id="4cc23-146">$clusterPassword</span><span class="sxs-lookup"><span data-stu-id="4cc23-146">$clusterPassword</span></span></td><td></td><td><span data-ttu-id="4cc23-147">A HDInsight fürt felhasználói jelszavát.</span><span class="sxs-lookup"><span data-stu-id="4cc23-147">The HDInsight cluster user password.</span></span></td></tr>
    <tr><td><span data-ttu-id="4cc23-148">Az Azure storage-fiók neve</span><span class="sxs-lookup"><span data-stu-id="4cc23-148">Azure storage account name</span></span></td><td><span data-ttu-id="4cc23-149">$storageAccountName</span><span class="sxs-lookup"><span data-stu-id="4cc23-149">$storageAccountName</span></span></td><td></td><td><span data-ttu-id="4cc23-150">Egy Azure Storage-fiók érhető el a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="4cc23-150">An Azure Storage account available to the HDInsight cluster.</span></span> <span data-ttu-id="4cc23-151">A jelen oktatóanyag esetében használja az alapértelmezett tárfiókot, amely a fürt létesítése során megadott.</span><span class="sxs-lookup"><span data-stu-id="4cc23-151">For this tutorial, use the default storage account that you specified during the cluster provision process.</span></span></td></tr>
    <tr><td><span data-ttu-id="4cc23-152">Az Azure Blob-tároló neve</span><span class="sxs-lookup"><span data-stu-id="4cc23-152">Azure Blob container name</span></span></td><td><span data-ttu-id="4cc23-153">$containerName</span><span class="sxs-lookup"><span data-stu-id="4cc23-153">$containerName</span></span></td><td></td><td><span data-ttu-id="4cc23-154">Ebben a példában az Azure Blob storage tárolót a HDInsight fürt alapértelmezett fájlrendszer használt használja.</span><span class="sxs-lookup"><span data-stu-id="4cc23-154">For this example, use the Azure Blob storage container that is used for the default HDInsight cluster file system.</span></span> <span data-ttu-id="4cc23-155">Alapértelmezés szerint rendelkezik a neve megegyezik a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="4cc23-155">By default, it has the same name as the HDInsight cluster.</span></span></td></tr>
    </table><span data-ttu-id="4cc23-156">
* **Azure SQL-adatbázis**.</span><span class="sxs-lookup"><span data-stu-id="4cc23-156">
* **An Azure SQL database**.</span></span> <span data-ttu-id="4cc23-157">Konfigurálnia kell egy tűzfalszabályt a SQL Database-kiszolgálóhoz való hozzáférést a munkaállomáson.</span><span class="sxs-lookup"><span data-stu-id="4cc23-157">You must configure a firewall rule for the SQL Database server to allow access from your workstation.</span></span> <span data-ttu-id="4cc23-158">Egy Azure SQL-adatbázis létrehozása, és a tűzfalon konfigurálásával kapcsolatos útmutatásért lásd: [használatának első Azure SQL adatbázis] [sqldatabase-get-started].</span><span class="sxs-lookup"><span data-stu-id="4cc23-158">For instructions about creating an Azure SQL database and configuring the firewall, see [Get started using Azure SQL database][sqldatabase-get-started].</span></span> <span data-ttu-id="4cc23-159">A cikkben egy Windows PowerShell-parancsfájlt, amelyekre szüksége van az oktatóanyag az Azure SQL-adatbázistáblában szereplő létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="4cc23-159">This article provides a Windows PowerShell script for creating the Azure SQL database table that you need for this tutorial.</span></span>

    <table border = "1">
    <tr><th><span data-ttu-id="4cc23-160">SQL-adatbázis tulajdonság</span><span class="sxs-lookup"><span data-stu-id="4cc23-160">SQL database property</span></span></th><th><span data-ttu-id="4cc23-161">A Windows PowerShell-változó neve</span><span class="sxs-lookup"><span data-stu-id="4cc23-161">Windows PowerShell variable name</span></span></th><th><span data-ttu-id="4cc23-162">Érték</span><span class="sxs-lookup"><span data-stu-id="4cc23-162">Value</span></span></th><th><span data-ttu-id="4cc23-163">Leírás</span><span class="sxs-lookup"><span data-stu-id="4cc23-163">Description</span></span></th></tr>
    <tr><td><span data-ttu-id="4cc23-164">SQL adatbázis-kiszolgáló neve</span><span class="sxs-lookup"><span data-stu-id="4cc23-164">SQL database server name</span></span></td><td><span data-ttu-id="4cc23-165">$sqlDatabaseServer</span><span class="sxs-lookup"><span data-stu-id="4cc23-165">$sqlDatabaseServer</span></span></td><td></td><td><span data-ttu-id="4cc23-166">Az SQL adatbázis-kiszolgáló, amelyhez Sqoop exportálja az adatokat.</span><span class="sxs-lookup"><span data-stu-id="4cc23-166">The SQL database server to which Sqoop will export data.</span></span> </td></tr>
    <tr><td><span data-ttu-id="4cc23-167">SQL adatbázis-bejelentkezési név</span><span class="sxs-lookup"><span data-stu-id="4cc23-167">SQL database login name</span></span></td><td><span data-ttu-id="4cc23-168">$sqlDatabaseLogin</span><span class="sxs-lookup"><span data-stu-id="4cc23-168">$sqlDatabaseLogin</span></span></td><td></td><td><span data-ttu-id="4cc23-169">SQL adatbázis-bejelentkezési név.</span><span class="sxs-lookup"><span data-stu-id="4cc23-169">SQL Database login name.</span></span></td></tr>
    <tr><td><span data-ttu-id="4cc23-170">SQL-adatbázis a bejelentkezés jelszó</span><span class="sxs-lookup"><span data-stu-id="4cc23-170">SQL database login password</span></span></td><td><span data-ttu-id="4cc23-171">$sqlDatabaseLoginPassword</span><span class="sxs-lookup"><span data-stu-id="4cc23-171">$sqlDatabaseLoginPassword</span></span></td><td></td><td><span data-ttu-id="4cc23-172">SQL-adatbázis bejelentkezési jelszót.</span><span class="sxs-lookup"><span data-stu-id="4cc23-172">SQL Database login password.</span></span></td></tr>
    <tr><td><span data-ttu-id="4cc23-173">SQL-adatbázis neve</span><span class="sxs-lookup"><span data-stu-id="4cc23-173">SQL database name</span></span></td><td><span data-ttu-id="4cc23-174">$sqlDatabaseName</span><span class="sxs-lookup"><span data-stu-id="4cc23-174">$sqlDatabaseName</span></span></td><td></td><td><span data-ttu-id="4cc23-175">Az Azure SQL-adatbázis, amelyhez Sqoop exportálja az adatokat.</span><span class="sxs-lookup"><span data-stu-id="4cc23-175">The Azure SQL database to which Sqoop will export data.</span></span> </td></tr>
    </table>

  > [!NOTE]
  > <span data-ttu-id="4cc23-176">Alapértelmezés szerint az Azure SQL database az Azure-szolgáltatások, például az Azure HDInsight engedélyezi a csatlakozást.</span><span class="sxs-lookup"><span data-stu-id="4cc23-176">By default an Azure SQL database allows connections from Azure Services, such as Azure HDInsight.</span></span> <span data-ttu-id="4cc23-177">Ha a tűzfal beállítás nincs engedélyezve, engedélyeznie kell azt az Azure portálról.</span><span class="sxs-lookup"><span data-stu-id="4cc23-177">If this firewall setting is disabled, you must enable it from the Azure Portal.</span></span> <span data-ttu-id="4cc23-178">Az utasítás SQL-adatbázis létrehozása és a tűzfal-szabályok konfigurálása, lásd: [létrehozása és konfigurálása az SQL-adatbázis][sqldatabase-get-started].</span><span class="sxs-lookup"><span data-stu-id="4cc23-178">For instruction about creating a SQL Database and configuring firewall rules, see [Create and Configure SQL Database][sqldatabase-get-started].</span></span>

> [!NOTE]
> <span data-ttu-id="4cc23-179">Kitöltés a táblában az értékeket.</span><span class="sxs-lookup"><span data-stu-id="4cc23-179">Fill-in the values in the tables.</span></span> <span data-ttu-id="4cc23-180">Hasznos lehet az oktatóanyag lépéseinek lesz.</span><span class="sxs-lookup"><span data-stu-id="4cc23-180">It will be helpful for going through this tutorial.</span></span>

## <a name="define-oozie-workflow-and-the-related-hiveql-script"></a><span data-ttu-id="4cc23-181">Oozie munkafolyamat és a kapcsolódó HiveQL-parancsfájlt</span><span class="sxs-lookup"><span data-stu-id="4cc23-181">Define Oozie workflow and the related HiveQL script</span></span>
<span data-ttu-id="4cc23-182">Oozie munkafolyamatok definíciók hPDL (az XML folyamat definition language) nyelven íródtak.</span><span class="sxs-lookup"><span data-stu-id="4cc23-182">Oozie workflows definitions are written in hPDL (an XML process definition language).</span></span> <span data-ttu-id="4cc23-183">Az alapértelmezett munkafolyamat Fájlnév *workflow.xml*.</span><span class="sxs-lookup"><span data-stu-id="4cc23-183">The default workflow file name is *workflow.xml*.</span></span>  <span data-ttu-id="4cc23-184">Mentse helyileg a munkafolyamat-fájlt fog, és ezután telepítse a HDInsight-fürthöz az oktatóanyag későbbi részében Azure PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="4cc23-184">You will save the workflow file locally, and then deploy it to the HDInsight cluster by using Azure PowerShell later in this tutorial.</span></span>

<span data-ttu-id="4cc23-185">A munkafolyamat Hive műveletét meghívja a HiveQL-parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="4cc23-185">The Hive action in the workflow calls a HiveQL script file.</span></span> <span data-ttu-id="4cc23-186">A parancsfájl három HiveQL utasításokat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="4cc23-186">This script file contains three HiveQL statements:</span></span>

1. <span data-ttu-id="4cc23-187">**A DROP TABLE utasítás** törli a log4j Hive táblát, ha van ilyen.</span><span class="sxs-lookup"><span data-stu-id="4cc23-187">**The DROP TABLE statement** deletes the log4j Hive table if it exists.</span></span>
2. <span data-ttu-id="4cc23-188">**A CREATE TABLE utasítás** táblázatot hoz létre log4j Hive külső, amelyek a helyet a log4j naplófájl;</span><span class="sxs-lookup"><span data-stu-id="4cc23-188">**The CREATE TABLE statement** creates a log4j Hive external table, which points to the location of the log4j log file;</span></span>
3. <span data-ttu-id="4cc23-189">**A log4j naplófájl helye**.</span><span class="sxs-lookup"><span data-stu-id="4cc23-189">**The location of the log4j log file**.</span></span> <span data-ttu-id="4cc23-190">A mező határoló ",".</span><span class="sxs-lookup"><span data-stu-id="4cc23-190">The field delimiter is ",".</span></span> <span data-ttu-id="4cc23-191">Az alapértelmezett sor elválasztó karaktere "\n".</span><span class="sxs-lookup"><span data-stu-id="4cc23-191">The default line delimiter is "\n".</span></span> <span data-ttu-id="4cc23-192">A Hive külső tábla elkerülése érdekében az adatfájl távolít el az eredeti helyre, abban az esetben, ha szeretné futtatni az Oozie munkafolyamat többször szolgál.</span><span class="sxs-lookup"><span data-stu-id="4cc23-192">A Hive external table is used to avoid the data file being removed from the original location, in case you want to run the Oozie workflow multiple times.</span></span>
4. <span data-ttu-id="4cc23-193">**Az INSERT FELÜLÍRÁSA utasítás** számát a log4j Hive táblát, és a naplózási szint típusonkénti előfordulását menti a kimenetet, egy Azure Blob storage-helyre.</span><span class="sxs-lookup"><span data-stu-id="4cc23-193">**The INSERT OVERWRITE statement** counts the occurrences of each log-level type from the log4j Hive table, and it saves the output to an Azure Blob storage location.</span></span>

> [!NOTE]
> <span data-ttu-id="4cc23-194">Nincs olyan ismert Hive útvonallal kapcsolatos probléma.</span><span class="sxs-lookup"><span data-stu-id="4cc23-194">There is a known Hive path issue.</span></span> <span data-ttu-id="4cc23-195">Ha egy Oozie feladat elküldése elindul ezt a problémát.</span><span class="sxs-lookup"><span data-stu-id="4cc23-195">You will run into this problem when submitting an Oozie job.</span></span> <span data-ttu-id="4cc23-196">A TechNet Wikin található az utasításokat a probléma kijavítása: [HDInsight Hive-hiba: nem lehet átnevezni][technetwiki-hive-error].</span><span class="sxs-lookup"><span data-stu-id="4cc23-196">The instructions for fixing the issue can be found on the TechNet Wiki: [HDInsight Hive error: Unable to rename][technetwiki-hive-error].</span></span>

<span data-ttu-id="4cc23-197">**A HiveQL parancsfájlt kell a munkafolyamat által meghívott meghatározása**</span><span class="sxs-lookup"><span data-stu-id="4cc23-197">**To define the HiveQL script file to be called by the workflow**</span></span>

1. <span data-ttu-id="4cc23-198">Hozzon létre egy szövegfájlt az alábbi tartalommal:</span><span class="sxs-lookup"><span data-stu-id="4cc23-198">Create a text file with the following content:</span></span>

        DROP TABLE ${hiveTableName};
        CREATE EXTERNAL TABLE ${hiveTableName}(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
        INSERT OVERWRITE DIRECTORY '${hiveOutputFolder}' SELECT t4 AS sev, COUNT(*) AS cnt FROM ${hiveTableName} WHERE t4 LIKE '[%' GROUP BY t4;

    <span data-ttu-id="4cc23-199">Három változókat a parancsfájl használatban van:</span><span class="sxs-lookup"><span data-stu-id="4cc23-199">There are three variables used in the script:</span></span>

   * <span data-ttu-id="4cc23-200">${hiveTableName}</span><span class="sxs-lookup"><span data-stu-id="4cc23-200">${hiveTableName}</span></span>
   * <span data-ttu-id="4cc23-201">${hiveDataFolder}</span><span class="sxs-lookup"><span data-stu-id="4cc23-201">${hiveDataFolder}</span></span>
   * <span data-ttu-id="4cc23-202">${hiveOutputFolder}</span><span class="sxs-lookup"><span data-stu-id="4cc23-202">${hiveOutputFolder}</span></span>

     <span data-ttu-id="4cc23-203">A munkafolyamat-definíciós fájl (ebben az oktatóanyagban workflow.xml) továbbítani fogja ezeket az értékeket a HiveQL-parancsfájlt a futási időben.</span><span class="sxs-lookup"><span data-stu-id="4cc23-203">The workflow definition file (workflow.xml in this tutorial) will pass these values to this HiveQL script at run time.</span></span>
2. <span data-ttu-id="4cc23-204">Mentse a fájlt **C:\Tutorials\UseOozie\useooziewf.hql** ANSI (ASCII) kódolással.</span><span class="sxs-lookup"><span data-stu-id="4cc23-204">Save the file as **C:\Tutorials\UseOozie\useooziewf.hql** by using ANSI (ASCII) encoding.</span></span> <span data-ttu-id="4cc23-205">(Használja a Jegyzettömböt, ha a szövegszerkesztőben nem adja meg ezt a lehetőséget.) A parancsfájl az oktatóanyag későbbi részében telepíti a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="4cc23-205">(Use Notepad if your text editor doesn't provide this option.) This script file will be deployed to the HDInsight cluster later in the tutorial.</span></span>

<span data-ttu-id="4cc23-206">**Egy munkafolyamat meghatározása**</span><span class="sxs-lookup"><span data-stu-id="4cc23-206">**To define a workflow**</span></span>

1. <span data-ttu-id="4cc23-207">Hozzon létre egy szövegfájlt az alábbi tartalommal:</span><span class="sxs-lookup"><span data-stu-id="4cc23-207">Create a text file with the following content:</span></span>

    ```xml
    <workflow-app name="useooziewf" xmlns="uri:oozie:workflow:0.2">
        <start to = "RunHiveScript"/>

        <action name="RunHiveScript">
            <hive xmlns="uri:oozie:hive-action:0.2">
                <job-tracker>${jobTracker}</job-tracker>
                <name-node>${nameNode}</name-node>
                <configuration>
                    <property>
                        <name>mapred.job.queue.name</name>
                        <value>${queueName}</value>
                    </property>
                </configuration>
                <script>${hiveScript}</script>
                <param>hiveTableName=${hiveTableName}</param>
                <param>hiveDataFolder=${hiveDataFolder}</param>
                <param>hiveOutputFolder=${hiveOutputFolder}</param>
            </hive>
            <ok to="RunSqoopExport"/>
            <error to="fail"/>
        </action>

        <action name="RunSqoopExport">
            <sqoop xmlns="uri:oozie:sqoop-action:0.2">
                <job-tracker>${jobTracker}</job-tracker>
                <name-node>${nameNode}</name-node>
                <configuration>
                    <property>
                        <name>mapred.compress.map.output</name>
                        <value>true</value>
                    </property>
                </configuration>
            <arg>export</arg>
            <arg>--connect</arg>
            <arg>${sqlDatabaseConnectionString}</arg>
            <arg>--table</arg>
            <arg>${sqlDatabaseTableName}</arg>
            <arg>--export-dir</arg>
            <arg>${hiveOutputFolder}</arg>
            <arg>-m</arg>
            <arg>1</arg>
            <arg>--input-fields-terminated-by</arg>
            <arg>"\001"</arg>
            </sqoop>
            <ok to="end"/>
            <error to="fail"/>
        </action>

        <kill name="fail">
            <message>Job failed, error message[${wf:errorMessage(wf:lastErrorNode())}] </message>
        </kill>

        <end name="end"/>
    </workflow-app>
    ```

    <span data-ttu-id="4cc23-208">Nincsenek a munkafolyamatban meghatározott két műveletet.</span><span class="sxs-lookup"><span data-stu-id="4cc23-208">There are two actions defined in the workflow.</span></span> <span data-ttu-id="4cc23-209">A start művelet *RunHiveScript*.</span><span class="sxs-lookup"><span data-stu-id="4cc23-209">The start-to action is *RunHiveScript*.</span></span> <span data-ttu-id="4cc23-210">Ha a művelet a futás *OK*, a következő művelet *RunSqoopExport*.</span><span class="sxs-lookup"><span data-stu-id="4cc23-210">If the action runs *OK*, the next action is *RunSqoopExport*.</span></span>

    <span data-ttu-id="4cc23-211">A RunHiveScript több változót tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="4cc23-211">The RunHiveScript has several variables.</span></span> <span data-ttu-id="4cc23-212">Az értékeket fogja továbbítani, amikor az Oozie-feladatot a munkaállomáson Azure PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="4cc23-212">You will pass the values when you submit the Oozie job from your workstation by using Azure PowerShell.</span></span>

    <span data-ttu-id="4cc23-213">Munkafolyamat-változók</span><span class="sxs-lookup"><span data-stu-id="4cc23-213">Workflow variables</span></span>

    <table border = "1">
    <tr><th><span data-ttu-id="4cc23-214">Munkafolyamat-változók</span><span class="sxs-lookup"><span data-stu-id="4cc23-214">Workflow variables</span></span></th><th><span data-ttu-id="4cc23-215">Leírás</span><span class="sxs-lookup"><span data-stu-id="4cc23-215">Description</span></span></th></tr>
    <tr><td><span data-ttu-id="4cc23-216">${jobTracker}</span><span class="sxs-lookup"><span data-stu-id="4cc23-216">${jobTracker}</span></span></td><td><span data-ttu-id="4cc23-217">Adja meg a Hadoop-feladat követő URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="4cc23-217">Specify the URL of the Hadoop job tracker.</span></span> <span data-ttu-id="4cc23-218">Használjon <strong>jobtrackerhost:9010</strong> a HDInsight fürt 3.0-s és 2.0-s verzióját.</span><span class="sxs-lookup"><span data-stu-id="4cc23-218">Use <strong>jobtrackerhost:9010</strong> on HDInsight cluster version 3.0 and 2.0.</span></span></td></tr>
    <tr><td><span data-ttu-id="4cc23-219">${nameNode}</span><span class="sxs-lookup"><span data-stu-id="4cc23-219">${nameNode}</span></span></td><td><span data-ttu-id="4cc23-220">Adja meg a Hadoop neve csomópont URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="4cc23-220">Specify the URL of the Hadoop name node.</span></span> <span data-ttu-id="4cc23-221">Használja az alapértelmezett fájl rendszer wasb: / / címmel, például <i>wasb: / /&lt;containerName&gt;@&lt;storageAccountName&gt;. blob.core.windows.net</i>.</span><span class="sxs-lookup"><span data-stu-id="4cc23-221">Use the default file system wasb:// address, for example, <i>wasb://&lt;containerName&gt;@&lt;storageAccountName&gt;.blob.core.windows.net</i>.</span></span></td></tr>
    <tr><td><span data-ttu-id="4cc23-222">${queueName}</span><span class="sxs-lookup"><span data-stu-id="4cc23-222">${queueName}</span></span></td><td><span data-ttu-id="4cc23-223">A feladat elküldve a várólista nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="4cc23-223">Specifies the queue name that the job will be submitted to.</span></span> <span data-ttu-id="4cc23-224">Használjon <strong>alapértelmezett</strong>.</span><span class="sxs-lookup"><span data-stu-id="4cc23-224">Use <strong>default</strong>.</span></span></td></tr>
    </table>

    <span data-ttu-id="4cc23-225">Hive művelet változói</span><span class="sxs-lookup"><span data-stu-id="4cc23-225">Hive action variables</span></span>

    <table border = "1">
    <tr><th><span data-ttu-id="4cc23-226">Műveleti változó struktúra</span><span class="sxs-lookup"><span data-stu-id="4cc23-226">Hive action variable</span></span></th><th><span data-ttu-id="4cc23-227">Leírás</span><span class="sxs-lookup"><span data-stu-id="4cc23-227">Description</span></span></th></tr>
    <tr><td><span data-ttu-id="4cc23-228">${hiveDataFolder}</span><span class="sxs-lookup"><span data-stu-id="4cc23-228">${hiveDataFolder}</span></span></td><td><span data-ttu-id="4cc23-229">A Hive Create Table parancs forráskönyvtárat.</span><span class="sxs-lookup"><span data-stu-id="4cc23-229">The source directory for the Hive Create Table command.</span></span></td></tr>
    <tr><td><span data-ttu-id="4cc23-230">${hiveOutputFolder}</span><span class="sxs-lookup"><span data-stu-id="4cc23-230">${hiveOutputFolder}</span></span></td><td><span data-ttu-id="4cc23-231">A kimeneti mappa az FELÜLÍRÁSA INSERT utasítás.</span><span class="sxs-lookup"><span data-stu-id="4cc23-231">The output folder for the INSERT OVERWRITE statement.</span></span></td></tr>
    <tr><td><span data-ttu-id="4cc23-232">${hiveTableName}</span><span class="sxs-lookup"><span data-stu-id="4cc23-232">${hiveTableName}</span></span></td><td><span data-ttu-id="4cc23-233">A Hive táblát, amely hivatkozik a log4j adatfájlok neve.</span><span class="sxs-lookup"><span data-stu-id="4cc23-233">The name of the Hive table that references the log4j data files.</span></span></td></tr>
    </table>

    <span data-ttu-id="4cc23-234">Sqoop művelet változói</span><span class="sxs-lookup"><span data-stu-id="4cc23-234">Sqoop action variables</span></span>

    <table border = "1">
    <tr><th><span data-ttu-id="4cc23-235">Sqoop művelet változó</span><span class="sxs-lookup"><span data-stu-id="4cc23-235">Sqoop action variable</span></span></th><th><span data-ttu-id="4cc23-236">Leírás</span><span class="sxs-lookup"><span data-stu-id="4cc23-236">Description</span></span></th></tr>
    <tr><td><span data-ttu-id="4cc23-237">${sqlDatabaseConnectionString}</span><span class="sxs-lookup"><span data-stu-id="4cc23-237">${sqlDatabaseConnectionString}</span></span></td><td><span data-ttu-id="4cc23-238">SQL adatbázis-kapcsolati karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="4cc23-238">SQL Database connection string.</span></span></td></tr>
    <tr><td><span data-ttu-id="4cc23-239">${sqlDatabaseTableName}</span><span class="sxs-lookup"><span data-stu-id="4cc23-239">${sqlDatabaseTableName}</span></span></td><td><span data-ttu-id="4cc23-240">Az Azure SQL adatbázis tábla hol lesznek exportálva az adatokat.</span><span class="sxs-lookup"><span data-stu-id="4cc23-240">The Azure SQL database table to where the data will be exported.</span></span></td></tr>
    <tr><td><span data-ttu-id="4cc23-241">${hiveOutputFolder}</span><span class="sxs-lookup"><span data-stu-id="4cc23-241">${hiveOutputFolder}</span></span></td><td><span data-ttu-id="4cc23-242">A kimeneti mappa a Hive BESZÚRÁSA FELÜLÍRÁSA utasítás.</span><span class="sxs-lookup"><span data-stu-id="4cc23-242">The output folder for the Hive INSERT OVERWRITE statement.</span></span> <span data-ttu-id="4cc23-243">Ez a Sqoop export (Exportálás-dir) ugyanabban a mappában.</span><span class="sxs-lookup"><span data-stu-id="4cc23-243">This is the same folder for the Sqoop export (export-dir).</span></span></td></tr>
    </table>

    <span data-ttu-id="4cc23-244">Oozie munkafolyamat és a munkafolyamat-műveleteket használatával kapcsolatos további információkért lásd: [Apache Oozie 4.0 dokumentáció] [ apache-oozie-400] (a HDInsight-fürt verziószáma 3.0) vagy [Apache Oozie 3.3.2 dokumentáció ] [ apache-oozie-332] (a HDInsight-fürt verziószáma 2.1-es).</span><span class="sxs-lookup"><span data-stu-id="4cc23-244">For more information about Oozie workflow and using the workflow actions, see [Apache Oozie 4.0 documentation][apache-oozie-400] (for HDInsight cluster version 3.0) or [Apache Oozie 3.3.2 documentation][apache-oozie-332] (for HDInsight cluster version 2.1).</span></span>

1. <span data-ttu-id="4cc23-245">Mentse a fájlt **C:\Tutorials\UseOozie\workflow.xml** ANSI (ASCII) kódolással.</span><span class="sxs-lookup"><span data-stu-id="4cc23-245">Save the file as **C:\Tutorials\UseOozie\workflow.xml** by using ANSI (ASCII) encoding.</span></span> <span data-ttu-id="4cc23-246">(Használja a Jegyzettömböt, ha a szövegszerkesztőben nem adja meg ezt a lehetőséget.)</span><span class="sxs-lookup"><span data-stu-id="4cc23-246">(Use Notepad if your text editor doesn't provide this option.)</span></span>

<span data-ttu-id="4cc23-247">**A koordinátor meghatározása**</span><span class="sxs-lookup"><span data-stu-id="4cc23-247">**To define coordinator**</span></span>

1. <span data-ttu-id="4cc23-248">Hozzon létre egy szövegfájlt az alábbi tartalommal:</span><span class="sxs-lookup"><span data-stu-id="4cc23-248">Create a text file with the following content:</span></span>

    ```xml
    <coordinator-app name="my_coord_app" frequency="${coordFrequency}" start="${coordStart}" end="${coordEnd}" timezone="${coordTimezone}" xmlns="uri:oozie:coordinator:0.4">
        <action>
            <workflow>
                <app-path>${wfPath}</app-path>
            </workflow>
        </action>
    </coordinator-app>
    ```

    <span data-ttu-id="4cc23-249">Nincsenek a szolgáltatásdefiníciós fájlban használt öt változók:</span><span class="sxs-lookup"><span data-stu-id="4cc23-249">There are five variables used in the definition file:</span></span>

   | <span data-ttu-id="4cc23-250">Változó</span><span class="sxs-lookup"><span data-stu-id="4cc23-250">Variable</span></span> | <span data-ttu-id="4cc23-251">Leírás</span><span class="sxs-lookup"><span data-stu-id="4cc23-251">Description</span></span> |
   | --- | --- |
   | <span data-ttu-id="4cc23-252">${coordFrequency}</span><span class="sxs-lookup"><span data-stu-id="4cc23-252">${coordFrequency}</span></span> |<span data-ttu-id="4cc23-253">A feladat felfüggesztése időpontja.</span><span class="sxs-lookup"><span data-stu-id="4cc23-253">Job pause time.</span></span> <span data-ttu-id="4cc23-254">Gyakoriság mindig kifejezett perc múlva.</span><span class="sxs-lookup"><span data-stu-id="4cc23-254">Frequency is always expressed in minutes.</span></span> |
   | <span data-ttu-id="4cc23-255">${coordStart}</span><span class="sxs-lookup"><span data-stu-id="4cc23-255">${coordStart}</span></span> |<span data-ttu-id="4cc23-256">A feladat kezdési időpontja.</span><span class="sxs-lookup"><span data-stu-id="4cc23-256">Job start time.</span></span> |
   | <span data-ttu-id="4cc23-257">${coordEnd}</span><span class="sxs-lookup"><span data-stu-id="4cc23-257">${coordEnd}</span></span> |<span data-ttu-id="4cc23-258">A feladat befejezési időpontja.</span><span class="sxs-lookup"><span data-stu-id="4cc23-258">Job end time.</span></span> |
   | <span data-ttu-id="4cc23-259">${coordTimezone}</span><span class="sxs-lookup"><span data-stu-id="4cc23-259">${coordTimezone}</span></span> |<span data-ttu-id="4cc23-260">Oozie coordinator feladatok rögzített időzónában dolgozza nem nyári időszámítás (általában képviseli UTC használatával).</span><span class="sxs-lookup"><span data-stu-id="4cc23-260">Oozie processes coordinator jobs in a fixed time zone with no daylight saving time (typically represented by using UTC).</span></span> <span data-ttu-id="4cc23-261">Ez az időzóna kezeli a "Oozie feldolgozási időzónában."</span><span class="sxs-lookup"><span data-stu-id="4cc23-261">This time zone is referred as the "Oozie processing timezone."</span></span> |
   | <span data-ttu-id="4cc23-262">${wfPath}</span><span class="sxs-lookup"><span data-stu-id="4cc23-262">${wfPath}</span></span> |<span data-ttu-id="4cc23-263">A workflow.xml elérési útját.</span><span class="sxs-lookup"><span data-stu-id="4cc23-263">The path for the workflow.xml.</span></span>  <span data-ttu-id="4cc23-264">Ha a munkafolyamat-fájl neve nem az alapértelmezett név (workflow.xml), meg kell adnia.</span><span class="sxs-lookup"><span data-stu-id="4cc23-264">If the workflow file name is not the default file name (workflow.xml), you must specify it.</span></span> |
2. <span data-ttu-id="4cc23-265">Mentse a fájlt **C:\Tutorials\UseOozie\coordinator.xml** a ANSI (ASCII) kódolással.</span><span class="sxs-lookup"><span data-stu-id="4cc23-265">Save the file as **C:\Tutorials\UseOozie\coordinator.xml** by using the ANSI (ASCII) encoding.</span></span> <span data-ttu-id="4cc23-266">(Használja a Jegyzettömböt, ha a szövegszerkesztőben nem adja meg ezt a lehetőséget.)</span><span class="sxs-lookup"><span data-stu-id="4cc23-266">(Use Notepad if your text editor doesn't provide this option.)</span></span>

## <a name="deploy-the-oozie-project-and-prepare-the-tutorial"></a><span data-ttu-id="4cc23-267">Telepítse az Oozie-projektet, és az oktatóanyag előkészítéséhez</span><span class="sxs-lookup"><span data-stu-id="4cc23-267">Deploy the Oozie project and prepare the tutorial</span></span>
<span data-ttu-id="4cc23-268">Egy Azure PowerShell-parancsfájlt, hogy a következő lesz futtatva:</span><span class="sxs-lookup"><span data-stu-id="4cc23-268">You will run an Azure PowerShell script to perform the following:</span></span>

* <span data-ttu-id="4cc23-269">Másolja a HiveQL-parancsfájlt (useoozie.hql) az Azure Blob storage wasb:///tutorials/useoozie/useoozie.hql.</span><span class="sxs-lookup"><span data-stu-id="4cc23-269">Copy the HiveQL script (useoozie.hql) to Azure Blob storage, wasb:///tutorials/useoozie/useoozie.hql.</span></span>
* <span data-ttu-id="4cc23-270">Másolja a workflow.xml wasb:///tutorials/useoozie/workflow.xml.</span><span class="sxs-lookup"><span data-stu-id="4cc23-270">Copy workflow.xml to wasb:///tutorials/useoozie/workflow.xml.</span></span>
* <span data-ttu-id="4cc23-271">Másolja a coordinator.xml wasb:///tutorials/useoozie/coordinator.xml.</span><span class="sxs-lookup"><span data-stu-id="4cc23-271">Copy coordinator.xml to wasb:///tutorials/useoozie/coordinator.xml.</span></span>
* <span data-ttu-id="4cc23-272">Másolja az adatokat (/ example/data/sample.log) wasb:///tutorials/useoozie/data/sample.log számára.</span><span class="sxs-lookup"><span data-stu-id="4cc23-272">Copy the data file (/example/data/sample.log) to wasb:///tutorials/useoozie/data/sample.log.</span></span>
* <span data-ttu-id="4cc23-273">Hozzon létre egy Azure SQL-adatbázistáblában szereplő Sqoop exportálási adatainak tárolásához.</span><span class="sxs-lookup"><span data-stu-id="4cc23-273">Create an Azure SQL database table for storing Sqoop export data.</span></span> <span data-ttu-id="4cc23-274">A táblázat neve *log4jLogCount*.</span><span class="sxs-lookup"><span data-stu-id="4cc23-274">The table name is *log4jLogCount*.</span></span>

<span data-ttu-id="4cc23-275">**HDInsight tároló ismertetése**</span><span class="sxs-lookup"><span data-stu-id="4cc23-275">**Understand HDInsight storage**</span></span>

<span data-ttu-id="4cc23-276">HDInsight Azure Blob storage-tárolására használ.</span><span class="sxs-lookup"><span data-stu-id="4cc23-276">HDInsight uses Azure Blob storage for data storage.</span></span> <span data-ttu-id="4cc23-277">wasb: / / a Hadoop elosztott fájlrendszer (HDFS) az Azure Blob storage a Microsoft általi implementációja.</span><span class="sxs-lookup"><span data-stu-id="4cc23-277">wasb:// is Microsoft's implementation of the Hadoop distributed file system (HDFS) in Azure Blob storage.</span></span> <span data-ttu-id="4cc23-278">További információkért lásd: [használata Azure Blob storage a hdinsight eszközzel][hdinsight-storage].</span><span class="sxs-lookup"><span data-stu-id="4cc23-278">For more information, see [Use Azure Blob storage with HDInsight][hdinsight-storage].</span></span>

<span data-ttu-id="4cc23-279">HDInsight-fürtök kiépítése, amikor egy Azure Blob storage-fiókot és egy adott tárolóhoz ebből a fiókból van kijelölve az alapértelmezett fájlrendszer, például a HDFS-ben.</span><span class="sxs-lookup"><span data-stu-id="4cc23-279">When you provision an HDInsight cluster, an Azure Blob storage account and a specific container from that account is designated as the default file system, like in HDFS.</span></span> <span data-ttu-id="4cc23-280">Ezen tárfiók mellett is hozzáadhat további tárfiókokat az azonos Azure-előfizetés vagy az Azure-előfizetések a telepítési folyamat során.</span><span class="sxs-lookup"><span data-stu-id="4cc23-280">In addition to this storage account, you can add additional storage accounts from the same Azure subscription or from different Azure subscriptions during the provisioning process.</span></span> <span data-ttu-id="4cc23-281">További tárfiókok hozzáadásáról útmutatásért lásd: [Provision HDInsight clusters][hdinsight-provision].</span><span class="sxs-lookup"><span data-stu-id="4cc23-281">For instructions about adding additional storage accounts, see [Provision HDInsight clusters][hdinsight-provision].</span></span> <span data-ttu-id="4cc23-282">Ebben az oktatóanyagban használt Azure PowerShell-parancsfájl leegyszerűsítése összes fájlt tárolja az alapértelmezett fájl rendszer tárolóban található */oktatóanyagok/useoozie*.</span><span class="sxs-lookup"><span data-stu-id="4cc23-282">To simplify the Azure PowerShell script used in this tutorial, all of the files are stored in the default file system container located at */tutorials/useoozie*.</span></span> <span data-ttu-id="4cc23-283">Alapértelmezés szerint ez a tároló neve megegyezik a HDInsight-fürt neve van.</span><span class="sxs-lookup"><span data-stu-id="4cc23-283">By default, this container has the same name as the HDInsight cluster name.</span></span>
<span data-ttu-id="4cc23-284">A szintaxis a következő:</span><span class="sxs-lookup"><span data-stu-id="4cc23-284">The syntax is:</span></span>

    wasb[s]://<ContainerName>@<StorageAccountName>.blob.core.windows.net/<path>/<filename>

> [!NOTE]
> <span data-ttu-id="4cc23-285">Csak a *wasb: / /* szintaxis a HDInsight fürt 3.0-s verziója támogatott.</span><span class="sxs-lookup"><span data-stu-id="4cc23-285">Only the *wasb://* syntax is supported in HDInsight cluster version 3.0.</span></span> <span data-ttu-id="4cc23-286">A régebbi *asv: / /* HDInsight 2.1-es és 1.6 fürtök a szintaxis támogatott, de nem támogatott a HDInsight 3.0 fürtök.</span><span class="sxs-lookup"><span data-stu-id="4cc23-286">The older *asv://* syntax is supported in HDInsight 2.1 and 1.6 clusters, but it is not supported in HDInsight 3.0 clusters.</span></span>
>
> <span data-ttu-id="4cc23-287">A wasb: / / elérési út egy virtuális elérési út.</span><span class="sxs-lookup"><span data-stu-id="4cc23-287">The wasb:// path is a virtual path.</span></span> <span data-ttu-id="4cc23-288">További információ: [használata Azure Blob storage a hdinsight eszközzel][hdinsight-storage].</span><span class="sxs-lookup"><span data-stu-id="4cc23-288">For more information see [Use Azure Blob storage with HDInsight][hdinsight-storage].</span></span>

<span data-ttu-id="4cc23-289">Az alapértelmezett fájl rendszer tárolóban tárolt fájlt érhetők el a HDInsight-ból a következő URI-azonosítók (használok workflow.xml példaként) egyikével sem:</span><span class="sxs-lookup"><span data-stu-id="4cc23-289">A file that is stored in the default file system container can be accessed from HDInsight by using any of the following URIs (I am using workflow.xml as an example):</span></span>

    wasb://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/workflow.xml
    wasb:///tutorials/useoozie/workflow.xml
    /tutorials/useoozie/workflow.xml

<span data-ttu-id="4cc23-290">Ha azt szeretné, a fájlt közvetlenül elérje a tárfiókot, a fájl a blob neve van:</span><span class="sxs-lookup"><span data-stu-id="4cc23-290">If you want to access the file directly from the storage account, the blob name for the file is:</span></span>

    tutorials/useoozie/workflow.xml

<span data-ttu-id="4cc23-291">**Hive belső és külső táblák ismertetése**</span><span class="sxs-lookup"><span data-stu-id="4cc23-291">**Understand Hive internal and external tables**</span></span>

<span data-ttu-id="4cc23-292">Néhány dolgot Hive belső és külső táblák ismernie kell:</span><span class="sxs-lookup"><span data-stu-id="4cc23-292">There are a few things you need to know about Hive internal and external tables:</span></span>

* <span data-ttu-id="4cc23-293">A CREATE TABLE parancs létrehoz egy belső tábla, más néven a felügyelt.</span><span class="sxs-lookup"><span data-stu-id="4cc23-293">The CREATE TABLE command creates an internal table, also known as a managed table.</span></span> <span data-ttu-id="4cc23-294">Az adatfájlban az alapértelmezett tárolóban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="4cc23-294">The data file must be located in the default container.</span></span>
* <span data-ttu-id="4cc23-295">A CREATE TABLE parancs áthelyezi az adatfájl/hive/adatraktár/<TableName> az alapértelmezett tároló mappát.</span><span class="sxs-lookup"><span data-stu-id="4cc23-295">The CREATE TABLE command moves the data file to the /hive/warehouse/<TableName> folder in the default container.</span></span>
* <span data-ttu-id="4cc23-296">A külső tábla létrehozása parancs létrehoz egy külső táblát.</span><span class="sxs-lookup"><span data-stu-id="4cc23-296">The CREATE EXTERNAL TABLE command creates an external table.</span></span> <span data-ttu-id="4cc23-297">Az adatfájlban az alapértelmezett tároló kívül is kell elhelyezni.</span><span class="sxs-lookup"><span data-stu-id="4cc23-297">The data file can be located outside the default container.</span></span>
* <span data-ttu-id="4cc23-298">A külső tábla létrehozása parancs nem helyezi át az adatfájlban.</span><span class="sxs-lookup"><span data-stu-id="4cc23-298">The CREATE EXTERNAL TABLE command does not move the data file.</span></span>
* <span data-ttu-id="4cc23-299">A külső tábla létrehozása parancs a mappában, a hely záradékban megadott almappákban nem engedélyezi.</span><span class="sxs-lookup"><span data-stu-id="4cc23-299">The CREATE EXTERNAL TABLE command doesn't allow any subfolders under the folder that is specified in the LOCATION clause.</span></span> <span data-ttu-id="4cc23-300">Ez az az oka, miért az oktatóanyag a sample.log fájl másolatot készít.</span><span class="sxs-lookup"><span data-stu-id="4cc23-300">This is the reason why the tutorial makes a copy of the sample.log file.</span></span>

<span data-ttu-id="4cc23-301">További információkért lásd: [HDInsight: Hive belső és külső táblák bevezetés][cindygross-hive-tables].</span><span class="sxs-lookup"><span data-stu-id="4cc23-301">For more information, see [HDInsight: Hive Internal and External Tables Intro][cindygross-hive-tables].</span></span>

<span data-ttu-id="4cc23-302">**Az oktatóanyag előkészítéséhez**</span><span class="sxs-lookup"><span data-stu-id="4cc23-302">**To prepare the tutorial**</span></span>

1. <span data-ttu-id="4cc23-303">Nyissa meg a Windows PowerShell ISE (írja be a Windows 8 kezdőképernyőn **PowerShell_ISE**, és kattintson a **Windows PowerShell ISE**.</span><span class="sxs-lookup"><span data-stu-id="4cc23-303">Open the Windows PowerShell ISE (in the Windows 8 Start screen, type **PowerShell_ISE**, and then click **Windows PowerShell ISE**.</span></span> <span data-ttu-id="4cc23-304">További információkért lásd: [Windows PowerShell elindítása a Windows 8 és Windows][powershell-start]).</span><span class="sxs-lookup"><span data-stu-id="4cc23-304">For more information, see [Start Windows PowerShell on Windows 8 and Windows][powershell-start]).</span></span>
2. <span data-ttu-id="4cc23-305">Az alsó ablaktáblában futtassa az Azure-előfizetéssel kapcsolódni a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="4cc23-305">In the bottom pane, run the following command to connect to your Azure subscription:</span></span>

    ```powershell
    Add-AzureAccount
    ```

    <span data-ttu-id="4cc23-306">Az Azure-fiók hitelesítő adatainak megadását kéri.</span><span class="sxs-lookup"><span data-stu-id="4cc23-306">You will be prompted to enter your Azure account credentials.</span></span> <span data-ttu-id="4cc23-307">Ez a módszer az előfizetés-kapcsolat hozzáadása túllépi az időkorlátot, és 12 óra elteltével kell futtassa újra a parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="4cc23-307">This method of adding a subscription connection times out, and after 12 hours, you will have to run the cmdlet again.</span></span>

   > [!NOTE]
   > <span data-ttu-id="4cc23-308">Ha több Azure-előfizetéssel rendelkezik, és az alapértelmezett előfizetés nem szeretné használni, azt a <strong>válasszon-AzureSubscription</strong> parancsmagot, hogy válasszon egy előfizetést.</span><span class="sxs-lookup"><span data-stu-id="4cc23-308">If you have multiple Azure subscriptions and the default subscription is not the one you want to use, use the <strong>Select-AzureSubscription</strong> cmdlet to select a subscription.</span></span>

3. <span data-ttu-id="4cc23-309">Másolja a következő a parancsfájl ablaktáblára, és utána állítsa be az első hat változók:</span><span class="sxs-lookup"><span data-stu-id="4cc23-309">Copy the following script into the script pane, and then set the first six variables:</span></span>

    ```powershell
    # WASB variables
    $storageAccountName = "<StorageAccountName>"
    $containerName = "<BlobStorageContainerName>"

    # SQL database variables
    $sqlDatabaseServer = "<SQLDatabaseServerName>"
    $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
    $sqlDatabaseLoginPassword = "SQLDatabaseLoginPassword>"
    $sqlDatabaseName = "<SQLDatabaseName>"
    $sqlDatabaseTableName = "log4jLogsCount"

    # Oozie files for the tutorial
    $hiveQLScript = "C:\Tutorials\UseOozie\useooziewf.hql"
    $workflowDefinition = "C:\Tutorials\UseOozie\workflow.xml"
    $coordDefinition =  "C:\Tutorials\UseOozie\coordinator.xml"

    # WASB folder for storing the Oozie tutorial files.
    $destFolder = "tutorials/useoozie"  # Do NOT use the long path here
    ```

    <span data-ttu-id="4cc23-310">A változók további leírását lásd: a [Előfeltételek](#prerequisites) ebben az oktatóanyagban szakasz.</span><span class="sxs-lookup"><span data-stu-id="4cc23-310">For more descriptions of the variables, see the [Prerequisites](#prerequisites) section in this tutorial.</span></span>

4. <span data-ttu-id="4cc23-311">A következő hozzáfűzése a parancsfájl a parancsfájl panelen:</span><span class="sxs-lookup"><span data-stu-id="4cc23-311">Append the following to the script in the script pane:</span></span>

    ```powershell
    # Create a storage context object
    $storageaccountkey = get-azurestoragekey $storageAccountName | %{$_.Primary}
    $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey

    function uploadOozieFiles()
    {
        Write-Host "Copy HiveQL script, workflow definition and coordinator definition ..." -ForegroundColor Green
        Set-AzureStorageBlobContent -File $hiveQLScript -Container $containerName -Blob "$destFolder/useooziewf.hql" -Context $destContext
        Set-AzureStorageBlobContent -File $workflowDefinition -Container $containerName -Blob "$destFolder/workflow.xml" -Context $destContext
        Set-AzureStorageBlobContent -File $coordDefinition -Container $containerName -Blob "$destFolder/coordinator.xml" -Context $destContext
    }

    function prepareHiveDataFile()
    {
        Write-Host "Make a copy of the sample.log file ... " -ForegroundColor Green
        Start-CopyAzureStorageBlob -SrcContainer $containerName -SrcBlob "example/data/sample.log" -Context $destContext -DestContainer $containerName -destBlob "$destFolder/data/sample.log" -DestContext $destContext
    }

    function prepareSQLDatabase()
    {
        # SQL query string for creating log4jLogsCount table
        $cmdCreateLog4jCountTable = " CREATE TABLE [dbo].[$sqlDatabaseTableName](
                [Level] [nvarchar](10) NOT NULL,
                [Total] float,
            CONSTRAINT [PK_$sqlDatabaseTableName] PRIMARY KEY CLUSTERED
            (
            [Level] ASC
            )
            )"

        #Create the log4jLogsCount table
        Write-Host "Create Log4jLogsCount table ..." -ForegroundColor Green
        $conn = New-Object System.Data.SqlClient.SqlConnection
        $conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabaseLoginPassword;Encrypt=true;Trusted_Connection=false;"
        $conn.open()
        $cmd = New-Object System.Data.SqlClient.SqlCommand
        $cmd.connection = $conn
        $cmd.commandtext = $cmdCreateLog4jCountTable
        $cmd.executenonquery()

        $conn.close()
    }

    # upload workflow.xml, coordinator.xml, and ooziewf.hql
    uploadOozieFiles;

    # make a copy of example/data/sample.log to example/data/log4j/sample.log
    prepareHiveDataFile;

    # create log4jlogsCount table on SQL database
    prepareSQLDatabase;
    ```

5. <span data-ttu-id="4cc23-312">Kattintson a **-parancsfájl futtatása** vagy nyomja le az ENTER **F5** a parancsfájl futtatásához.</span><span class="sxs-lookup"><span data-stu-id="4cc23-312">Click **Run Script** or press **F5** to run the script.</span></span> <span data-ttu-id="4cc23-313">A kimeneti hasonló lesz:</span><span class="sxs-lookup"><span data-stu-id="4cc23-313">The output will be similar to:</span></span>

    ![Útmutató előkészítési kimeneti][img-preparation-output]

## <a name="run-the-oozie-project"></a><span data-ttu-id="4cc23-315">Futtassa az Oozie-projektet</span><span class="sxs-lookup"><span data-stu-id="4cc23-315">Run the Oozie project</span></span>
<span data-ttu-id="4cc23-316">Az Azure PowerShell jelenleg nem biztosít semmilyen parancsmagok Oozie feladatok meghatározásához.</span><span class="sxs-lookup"><span data-stu-id="4cc23-316">Azure PowerShell currently doesn't provide any cmdlets for defining Oozie jobs.</span></span> <span data-ttu-id="4cc23-317">Használhatja a **Invoke-RestMethod** parancsmag Oozie webszolgáltatás meghívására.</span><span class="sxs-lookup"><span data-stu-id="4cc23-317">You can use the **Invoke-RestMethod** cmdlet to invoke Oozie web services.</span></span> <span data-ttu-id="4cc23-318">Az Oozie webszolgáltatási API-ra egy HTTP REST API-t JSON.</span><span class="sxs-lookup"><span data-stu-id="4cc23-318">The Oozie web services API is a HTTP REST JSON API.</span></span> <span data-ttu-id="4cc23-319">Az Oozie-webszolgáltatások API kapcsolatos további információkért lásd: [Apache Oozie 4.0 dokumentáció] [ apache-oozie-400] (a HDInsight-fürt verziószáma 3.0) vagy [Apache Oozie 3.3.2 dokumentáció] [ apache-oozie-332] (a HDInsight-fürt verziószáma 2.1-es).</span><span class="sxs-lookup"><span data-stu-id="4cc23-319">For more information about the Oozie web services API, see [Apache Oozie 4.0 documentation][apache-oozie-400] (for HDInsight cluster version 3.0) or [Apache Oozie 3.3.2 documentation][apache-oozie-332] (for HDInsight cluster version 2.1).</span></span>

<span data-ttu-id="4cc23-320">**Elküldeni egy Oozie feladatot**</span><span class="sxs-lookup"><span data-stu-id="4cc23-320">**To submit an Oozie job**</span></span>

1. <span data-ttu-id="4cc23-321">Nyissa meg a Windows PowerShell ISE (a Windows 8 kezdőképernyőn írja be **PowerShell_ISE**, és kattintson a **Windows PowerShell ISE**.</span><span class="sxs-lookup"><span data-stu-id="4cc23-321">Open the Windows PowerShell ISE (in Windows 8 Start screen, type **PowerShell_ISE**, and then click **Windows PowerShell ISE**.</span></span> <span data-ttu-id="4cc23-322">További információkért lásd: [Windows PowerShell elindítása a Windows 8 és Windows][powershell-start]).</span><span class="sxs-lookup"><span data-stu-id="4cc23-322">For more information, see [Start Windows PowerShell on Windows 8 and Windows][powershell-start]).</span></span>
2. <span data-ttu-id="4cc23-323">Másolja a következő a parancsfájl ablaktáblára, és utána állítsa be az első 14 változók (azonban kihagyása **$storageUri**).</span><span class="sxs-lookup"><span data-stu-id="4cc23-323">Copy the following script into the script pane, and then set the first fourteen variables (however, skip **$storageUri**).</span></span>

    ```powershell
    #HDInsight cluster variables
    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterUserPassword>"

    #Azure Blob storage (WASB) variables
    $storageAccountName = "<StorageAccountName>"
    $storageContainerName = "<BlobContainerName>"
    $storageUri="wasb://$storageContainerName@$storageAccountName.blob.core.windows.net"

    #Azure SQL database variables
    $sqlDatabaseServer = "<SQLDatabaseServerName>"
    $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
    $sqlDatabaseLoginPassword = "<SQLDatabaseloginPassword>"
    $sqlDatabaseName = "<SQLDatabaseName>"

    #Oozie WF/coordinator variables
    $coordStart = "2014-03-21T13:45Z"
    $coordEnd = "2014-03-21T13:45Z"
    $coordFrequency = "1440"    # in minutes, 24h x 60m = 1440m
    $coordTimezone = "UTC"    #UTC/GMT

    $oozieWFPath="$storageUri/tutorials/useoozie"  # The default name is workflow.xml. And you don't need to specify the file name.
    $waitTimeBetweenOozieJobStatusCheck=10

    #Hive action variables
    $hiveScript = "$storageUri/tutorials/useoozie/useooziewf.hql"
    $hiveTableName = "log4jlogs"
    $hiveDataFolder = "$storageUri/tutorials/useoozie/data"
    $hiveOutputFolder = "$storageUri/tutorials/useoozie/output"

    #Sqoop action variables
    $sqlDatabaseConnectionString = "jdbc:sqlserver://$sqlDatabaseServer.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServer;password=$sqlDatabaseLoginPassword;database=$sqlDatabaseName"
    $sqlDatabaseTableName = "log4jLogsCount"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)
    ```

    <span data-ttu-id="4cc23-324">A változók további leírását lásd: a [Előfeltételek](#prerequisites) ebben az oktatóanyagban szakasz.</span><span class="sxs-lookup"><span data-stu-id="4cc23-324">For more descriptions of the variables, see the [Prerequisites](#prerequisites) section in this tutorial.</span></span>

    <span data-ttu-id="4cc23-325">$coordstart és $coordend a munkafolyamat kezdési és befejezési idő.</span><span class="sxs-lookup"><span data-stu-id="4cc23-325">$coordstart and $coordend are the workflow starting and ending time.</span></span> <span data-ttu-id="4cc23-326">Az UTC/GMT szerinti idő további tudnivalókért keresse "UTC szerinti idő" bing.com. A $coordFrequency milyen gyakran szeretné futtatni a munkafolyamat percben van.</span><span class="sxs-lookup"><span data-stu-id="4cc23-326">To find out the UTC/GMT time, search "utc time" on bing.com. The $coordFrequency is how often in minutes you want to run the workflow.</span></span>
3. <span data-ttu-id="4cc23-327">A következő hozzáfűzése a parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="4cc23-327">Append the following to the script.</span></span> <span data-ttu-id="4cc23-328">Ez a kijelző meghatározza, hogy az Oozie-tartalom:</span><span class="sxs-lookup"><span data-stu-id="4cc23-328">This part defines the Oozie payload:</span></span>

    ```powershell
    #OoziePayload used for Oozie web service submission
    $OoziePayload =  @"
    <?xml version="1.0" encoding="UTF-8"?>
    <configuration>

        <property>
            <name>nameNode</name>
            <value>$storageUrI</value>
        </property>

        <property>
            <name>jobTracker</name>
            <value>jobtrackerhost:9010</value>
        </property>

        <property>
            <name>queueName</name>
            <value>default</value>
        </property>

        <property>
            <name>oozie.use.system.libpath</name>
            <value>true</value>
        </property>

        <property>
            <name>oozie.coord.application.path</name>
            <value>$oozieWFPath</value>
        </property>

        <property>
            <name>wfPath</name>
            <value>$oozieWFPath</value>
        </property>

        <property>
            <name>coordStart</name>
            <value>$coordStart</value>
        </property>

        <property>
            <name>coordEnd</name>
            <value>$coordEnd</value>
        </property>

        <property>
            <name>coordFrequency</name>
            <value>$coordFrequency</value>
        </property>

        <property>
            <name>coordTimezone</name>
            <value>$coordTimezone</value>
        </property>

        <property>
            <name>hiveScript</name>
            <value>$hiveScript</value>
        </property>

        <property>
            <name>hiveTableName</name>
            <value>$hiveTableName</value>
        </property>

        <property>
            <name>hiveDataFolder</name>
            <value>$hiveDataFolder</value>
        </property>

        <property>
            <name>hiveOutputFolder</name>
            <value>$hiveOutputFolder</value>
        </property>

        <property>
            <name>sqlDatabaseConnectionString</name>
            <value>&quot;$sqlDatabaseConnectionString&quot;</value>
        </property>

        <property>
            <name>sqlDatabaseTableName</name>
            <value>$SQLDatabaseTableName</value>
        </property>

        <property>
            <name>user.name</name>
            <value>admin</value>
        </property>

    </configuration>
    "@
    ```

   > [!NOTE]
   > <span data-ttu-id="4cc23-329">A fő különbség az adatfájlban munkafolyamat küldésének képest a változó **oozie.coord.application.path**.</span><span class="sxs-lookup"><span data-stu-id="4cc23-329">The major difference compared to the workflow submission payload file is the variable **oozie.coord.application.path**.</span></span> <span data-ttu-id="4cc23-330">Amikor egy munkafolyamat-feladat elküldéséhez használhatja **oozie.wf.application.path** helyette.</span><span class="sxs-lookup"><span data-stu-id="4cc23-330">When you submit a workflow job, you use **oozie.wf.application.path** instead.</span></span>

4. <span data-ttu-id="4cc23-331">A következő hozzáfűzése a parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="4cc23-331">Append the following to the script.</span></span> <span data-ttu-id="4cc23-332">Ez a kijelző Oozie webes szolgáltatás állapotát ellenőrzi:</span><span class="sxs-lookup"><span data-stu-id="4cc23-332">This part checks the Oozie web service status:</span></span>

    ```powershell
    function checkOozieServerStatus()
    {
        Write-Host "Checking Oozie server status..." -ForegroundColor Green
        $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/admin/status"
        $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds -OutVariable $OozieServerStatus

        $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
        $oozieServerSatus = $jsonResponse[0].("systemMode")
        Write-Host "Oozie server status is $oozieServerSatus..."

        if($oozieServerSatus -notmatch "NORMAL")
        {
            Write-Host "Oozie server status is $oozieServerSatus...cannot submit Oozie jobs. Check the server status and re-run the job."
            exit 1
        }
    }
    ```

5. <span data-ttu-id="4cc23-333">A következő hozzáfűzése a parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="4cc23-333">Append the following to the script.</span></span> <span data-ttu-id="4cc23-334">Ez a kijelző egy Oozie feladat létrehozása:</span><span class="sxs-lookup"><span data-stu-id="4cc23-334">This part creates an Oozie job:</span></span>

    ```powershell
    function createOozieJob()
    {
        # create Oozie job
        Write-Host "Sending the following Payload to the cluster:" -ForegroundColor Green
        Write-Host "`n--------`n$OoziePayload`n--------"
        $clusterUriCreateJob = "https://$clusterName.azurehdinsight.net:443/oozie/v2/jobs"
        $response = Invoke-RestMethod -Method Post -Uri $clusterUriCreateJob -Credential $creds -Body $OoziePayload -ContentType "application/xml" -OutVariable $OozieJobName -debug -Verbose

        $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
        $oozieJobId = $jsonResponse[0].("id")
        Write-Host "Oozie job id is $oozieJobId..."

        return $oozieJobId
    }
    ```

   > [!NOTE]
   > <span data-ttu-id="4cc23-335">Amikor egy munkafolyamat-feladat, meg kell nyitnia egy másik webszolgáltatás-hívás elindítja a feladatot, a feladat létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="4cc23-335">When you submit a workflow job, you must make another web service call to start the job after the job is created.</span></span> <span data-ttu-id="4cc23-336">Ebben az esetben a koordinátor feladat idő váltja ki.</span><span class="sxs-lookup"><span data-stu-id="4cc23-336">In this case, the coordinator job is triggered by time.</span></span> <span data-ttu-id="4cc23-337">A feladat automatikusan elindul.</span><span class="sxs-lookup"><span data-stu-id="4cc23-337">The job will start automatically.</span></span>

6. <span data-ttu-id="4cc23-338">A következő hozzáfűzése a parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="4cc23-338">Append the following to the script.</span></span> <span data-ttu-id="4cc23-339">Ez a kijelző Oozie feladat állapotát ellenőrzi:</span><span class="sxs-lookup"><span data-stu-id="4cc23-339">This part checks the Oozie job status:</span></span>

    ```powershell
    function checkOozieJobStatus($oozieJobId)
    {
        # get job status
        Write-Host "Sleeping for $waitTimeBetweenOozieJobStatusCheck seconds until the job metadata is populated in the Oozie metastore..." -ForegroundColor Green
        Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck

        Write-Host "Getting job status and waiting for the job to complete..." -ForegroundColor Green
        $clusterUriGetJobStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?show=info"
        $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $creds
        $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
        $JobStatus = $jsonResponse[0].("status")

        while($JobStatus -notmatch "SUCCEEDED|KILLED")
        {
            Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state...waiting $waitTimeBetweenOozieJobStatusCheck seconds for the job to complete..."
            Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $creds
            $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
            $JobStatus = $jsonResponse[0].("status")
        }

        Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state!"
        if($JobStatus -notmatch "SUCCEEDED")
        {
            Write-Host "Check logs at http://headnode0:9014/cluster for detais."
            exit -1
        }
    }
    ```

7. <span data-ttu-id="4cc23-340">(Választható) A következő hozzáfűzése a parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="4cc23-340">(Optional) Append the following to the script.</span></span>

    ```powershell
    function listOozieJobs()
    {
        Write-Host "Listing Oozie jobs..." -ForegroundColor Green
        $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/jobs"
        $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds

        write-host "Job ID                                   App Name        Status      Started                         Ended"
        write-host "----------------------------------------------------------------------------------------------------------------------------------"
        foreach($job in $response.workflows)
        {
            Write-Host $job.id "`t" $job.appName "`t" $job.status "`t" $job.startTime "`t" $job.endTime
        }
    }

    function ShowOozieJobLog($oozieJobId)
    {
        Write-Host "Showing Oozie job info..." -ForegroundColor Green
        $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/$oozieJobId" + "?show=log"
        $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds
        write-host $response
    }

    function killOozieJob($oozieJobId)
    {
        Write-Host "Killing the Oozie job $oozieJobId..." -ForegroundColor Green
        $clusterUriStartJob = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?action=kill" #Valid values for the 'action' parameter are 'start', 'suspend', 'resume', 'kill', 'dryrun', 'rerun', and 'change'.
        $response = Invoke-RestMethod -Method Put -Uri $clusterUriStartJob -Credential $creds | Format-Table -HideTableHeaders -debug
    }
    ```

8. <span data-ttu-id="4cc23-341">A következő hozzáfűzése a parancsfájlt:</span><span class="sxs-lookup"><span data-stu-id="4cc23-341">Append the following to the script:</span></span>

    ```powershell
    checkOozieServerStatus
    # listOozieJobs
    $oozieJobId = createOozieJob($oozieJobId)
    checkOozieJobStatus($oozieJobId)
    # ShowOozieJobLog($oozieJobId)
    # killOozieJob($oozieJobId)
    ```

    <span data-ttu-id="4cc23-342">Távolítsa el a # jelet, ha a további funkciók futtatni szeretné.</span><span class="sxs-lookup"><span data-stu-id="4cc23-342">Remove the # signs if you want to run the additional functions.</span></span>
9. <span data-ttu-id="4cc23-343">Ha a HDinsight-fürt 2.1-es verziója, cserélje le a "https://$clusterName.azurehdinsight.net:443/oozie/v2/" a "https://$clusterName.azurehdinsight.net:443/oozie/v1/".</span><span class="sxs-lookup"><span data-stu-id="4cc23-343">If your HDinsight cluster is version 2.1, replace "https://$clusterName.azurehdinsight.net:443/oozie/v2/" with "https://$clusterName.azurehdinsight.net:443/oozie/v1/".</span></span> <span data-ttu-id="4cc23-344">HDInsight-fürt verziószáma 2.1 nem támogatja a 2-es verziójának a webszolgáltatások végzi.</span><span class="sxs-lookup"><span data-stu-id="4cc23-344">HDInsight cluster version 2.1 does not supports version 2 of the web services.</span></span>
10. <span data-ttu-id="4cc23-345">Kattintson a **-parancsfájl futtatása** vagy nyomja le az ENTER **F5** a parancsfájl futtatásához.</span><span class="sxs-lookup"><span data-stu-id="4cc23-345">Click **Run Script** or press **F5** to run the script.</span></span> <span data-ttu-id="4cc23-346">A kimeneti hasonló lesz:</span><span class="sxs-lookup"><span data-stu-id="4cc23-346">The output will be similar to:</span></span>

     ![Útmutató munkafolyamat kimeneti futtatása][img-runworkflow-output]
11. <span data-ttu-id="4cc23-348">Csatlakozás az SQL-adatbázis az exportált adatok megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="4cc23-348">Connect to your SQL Database to see the exported data.</span></span>

<span data-ttu-id="4cc23-349">**A feladat hibanapló ellenőrzése**</span><span class="sxs-lookup"><span data-stu-id="4cc23-349">**To check the job error log**</span></span>

<span data-ttu-id="4cc23-350">Egy munkafolyamat elhárításához a Oozie naplófájl helyen találhatók C:\apps\dist\oozie-3.3.2.1.3.2.0-05\oozie-win-distro\logs\Oozie.log a a fürt headnode.</span><span class="sxs-lookup"><span data-stu-id="4cc23-350">To troubleshoot a workflow, the Oozie log file can be found at C:\apps\dist\oozie-3.3.2.1.3.2.0-05\oozie-win-distro\logs\Oozie.log from the cluster headnode.</span></span> <span data-ttu-id="4cc23-351">Az RDP információkért lásd: [felügyelete HDInsight-fürtök az Azure portál használatával][hdinsight-admin-portal].</span><span class="sxs-lookup"><span data-stu-id="4cc23-351">For information on RDP, see [Administering HDInsight clusters using the Azure portal][hdinsight-admin-portal].</span></span>

<span data-ttu-id="4cc23-352">**Az oktatóanyag újrafuttatása**</span><span class="sxs-lookup"><span data-stu-id="4cc23-352">**To rerun the tutorial**</span></span>

<span data-ttu-id="4cc23-353">A munkafolyamat újrafuttatásához, a következő feladatokat kell elvégeznie:</span><span class="sxs-lookup"><span data-stu-id="4cc23-353">To rerun the workflow, you must perform the following tasks:</span></span>

* <span data-ttu-id="4cc23-354">A Hive parancsfájl kimeneti fájl törlése.</span><span class="sxs-lookup"><span data-stu-id="4cc23-354">Delete the Hive script output file.</span></span>
* <span data-ttu-id="4cc23-355">Törölje a log4jLogsCount tábla.</span><span class="sxs-lookup"><span data-stu-id="4cc23-355">Delete the data in the log4jLogsCount table.</span></span>

<span data-ttu-id="4cc23-356">Íme egy minta Windows PowerShell-parancsfájl használható:</span><span class="sxs-lookup"><span data-stu-id="4cc23-356">Here is a sample Windows PowerShell script that you can use:</span></span>

```powershell
$storageAccountName = "<AzureStorageAccountName>"
$containerName = "<ContainerName>"

#SQL database variables
$sqlDatabaseServer = "<SQLDatabaseServerName>"
$sqlDatabaseLogin = "<SQLDatabaseLoginName>"
$sqlDatabaseLoginPassword = "<SQLDatabaseLoginPassword>"
$sqlDatabaseName = "<SQLDatabaseName>"
$sqlDatabaseTableName = "log4jLogsCount"

Write-host "Delete the Hive script output file ..." -ForegroundColor Green
$storageaccountkey = get-azurestoragekey $storageAccountName | %{$_.Primary}
$destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey
Remove-AzureStorageBlob -Context $destContext -Blob "tutorials/useoozie/output/000000_0" -Container $containerName

Write-host "Delete all the records from the log4jLogsCount table ..." -ForegroundColor Green
$conn = New-Object System.Data.SqlClient.SqlConnection
$conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabaseLoginPassword;Encrypt=true;Trusted_Connection=false;"
$conn.open()
$cmd = New-Object System.Data.SqlClient.SqlCommand
$cmd.connection = $conn
$cmd.commandtext = "delete from $sqlDatabaseTableName"
$cmd.executenonquery()

$conn.close()
```

## <a name="next-steps"></a><span data-ttu-id="4cc23-357">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4cc23-357">Next steps</span></span>
<span data-ttu-id="4cc23-358">Ebben az oktatóanyagban megtanulta, hogyan adhat meg, az Oozie munkafolyamat és az Oozie-koordinátor és az Oozie-koordinátor feladat futtatása az Azure PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="4cc23-358">In this tutorial, you learned how to define an Oozie workflow and an Oozie coordinator, and how to run an Oozie coordinator job by using Azure PowerShell.</span></span> <span data-ttu-id="4cc23-359">További tudnivalókért tekintse meg a következő cikkeket:</span><span class="sxs-lookup"><span data-stu-id="4cc23-359">To learn more, see the following articles:</span></span>

* <span data-ttu-id="4cc23-360">[Első lépései a hdinsight eszközzel][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="4cc23-360">[Get started with HDInsight][hdinsight-get-started]</span></span>
* <span data-ttu-id="4cc23-361">[Használhat Azure Blob Storage tárolót a hdinsight eszközzel][hdinsight-storage]</span><span class="sxs-lookup"><span data-stu-id="4cc23-361">[Use Azure Blob storage with HDInsight][hdinsight-storage]</span></span>
* <span data-ttu-id="4cc23-362">[HDInsight felügyelete az Azure PowerShell használatával][hdinsight-admin-powershell]</span><span class="sxs-lookup"><span data-stu-id="4cc23-362">[Administer HDInsight by using Azure PowerShell][hdinsight-admin-powershell]</span></span>
* <span data-ttu-id="4cc23-363">[Adatok feltöltése a HDInsightba][hdinsight-upload-data]</span><span class="sxs-lookup"><span data-stu-id="4cc23-363">[Upload data to HDInsight][hdinsight-upload-data]</span></span>
* <span data-ttu-id="4cc23-364">[Use Sqoop with HDInsight][hdinsight-use-sqoop]</span><span class="sxs-lookup"><span data-stu-id="4cc23-364">[Use Sqoop with HDInsight][hdinsight-use-sqoop]</span></span>
* <span data-ttu-id="4cc23-365">[A Hive használata a HDInsightban][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="4cc23-365">[Use Hive with HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="4cc23-366">[A Pig használata a HDInsightban][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="4cc23-366">[Use Pig with HDInsight][hdinsight-use-pig]</span></span>
* <span data-ttu-id="4cc23-367">[Java-MapReduce programok fejlesztése a HDInsight][hdinsight-develop-java-mapreduce]</span><span class="sxs-lookup"><span data-stu-id="4cc23-367">[Develop Java MapReduce programs for HDInsight][hdinsight-develop-java-mapreduce]</span></span>

[hdinsight-cmdlets-download]: http://go.microsoft.com/fwlink/?LinkID=325563

[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md

[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-develop-java-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md

[sqldatabase-get-started]: ../sql-database/sql-database-get-started.md

[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
[apache-oozie-400]: http://oozie.apache.org/docs/4.0.0/
[apache-oozie-332]: http://oozie.apache.org/docs/3.3.2/

[powershell-download]: http://azure.microsoft.com/downloads/
[powershell-about-profiles]: http://go.microsoft.com/fwlink/?LinkID=113729
[powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[img-workflow-diagram]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.Workflow.Diagram.png
[img-preparation-output]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.Preparation.Output1.png
[img-runworkflow-output]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.RunCoord.Output.png

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx
