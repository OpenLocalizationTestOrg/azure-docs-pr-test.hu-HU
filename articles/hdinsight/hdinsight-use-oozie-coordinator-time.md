---
title: "aaaUse a hdinsight meg időalapú Hadoop Oozie a koordinátor |} Microsoft Docs"
description: "Időalapú Hadoop Oozie-koordinátor használata a Hdinsightban, big data-szolgáltatása. Ismerje meg, hogyan toodefine Oozie munkafolyamatok és a koordinátor, valamint feladatok elküldéséhez."
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
ms.openlocfilehash: aecbb5ee94a4234d1a7768bdb6de2a33508b1e4c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-time-based-oozie-coordinator-with-hadoop-in-hdinsight-toodefine-workflows-and-coordinate-jobs"></a><span data-ttu-id="f5de8-104">Hadoop HDInsight toodefine munkafolyamatok időalapú Oozie-koordinátor használata, és összehangolják a feladatok</span><span class="sxs-lookup"><span data-stu-id="f5de8-104">Use time-based Oozie coordinator with Hadoop in HDInsight toodefine workflows and coordinate jobs</span></span>
<span data-ttu-id="f5de8-105">Ebből a cikkből megtudhatja, hogyan toodefine munkafolyamatok és a koordinátor, és hogyan tootrigger hello koordinátor-feladatokhoz, ideje alapján.</span><span class="sxs-lookup"><span data-stu-id="f5de8-105">In this article, you'll learn how toodefine workflows and coordinators, and how tootrigger hello coordinator jobs, based on time.</span></span> <span data-ttu-id="f5de8-106">Hasznos toogo keresztül [és a HDInsight együttes használata Oozie] [ hdinsight-use-oozie] Ez a cikk olvasása előtt.</span><span class="sxs-lookup"><span data-stu-id="f5de8-106">It is helpful toogo through [Use Oozie with HDInsight][hdinsight-use-oozie] before you read this article.</span></span> <span data-ttu-id="f5de8-107">Ezenkívül tooOozie, is ütemezheti az Azure Data Factory használatával feladatok.</span><span class="sxs-lookup"><span data-stu-id="f5de8-107">In addition tooOozie, you can also schedule jobs using Azure Data Factory.</span></span> <span data-ttu-id="f5de8-108">Azure Data Factory toolearn lásd [használja a Pig és a Data Factory Hive](../data-factory/data-factory-data-transformation-activities.md).</span><span class="sxs-lookup"><span data-stu-id="f5de8-108">toolearn Azure Data Factory, see [Use Pig and Hive with Data Factory](../data-factory/data-factory-data-transformation-activities.md).</span></span>

> [!NOTE]
> <span data-ttu-id="f5de8-109">Ez a cikk egy Windows-alapú HDInsight-fürt szükséges.</span><span class="sxs-lookup"><span data-stu-id="f5de8-109">This article requires a Windows-based HDInsight cluster.</span></span> <span data-ttu-id="f5de8-110">Oozie, többek között a feladatok időalapú egy Linux-alapú fürtön használatáról lásd: [használata Oozie Hadoop toodefine és a Linux-alapú HDInsight a munkafolyamat futtatása](hdinsight-use-oozie-linux-mac.md)</span><span class="sxs-lookup"><span data-stu-id="f5de8-110">For information on using Oozie, including time-based jobs, on a Linux-based cluster, see [Use Oozie with Hadoop toodefine and run a workflow on Linux-based HDInsight](hdinsight-use-oozie-linux-mac.md)</span></span>

## <a name="what-is-oozie"></a><span data-ttu-id="f5de8-111">Mi az az Oozie</span><span class="sxs-lookup"><span data-stu-id="f5de8-111">What is Oozie</span></span>
<span data-ttu-id="f5de8-112">Apache Oozie egy munkafolyamat/koordinációs rendszer, amely a Hadoop-feladatokat kezeli.</span><span class="sxs-lookup"><span data-stu-id="f5de8-112">Apache Oozie is a workflow/coordination system that manages Hadoop jobs.</span></span> <span data-ttu-id="f5de8-113">Hadoop-veremmel hello integrálva van, és támogatja a Hadoop-feladatokat Apache MapReduce, Apache Pig, Apache Hive és Apache Sqoop.</span><span class="sxs-lookup"><span data-stu-id="f5de8-113">It is integrated with hello Hadoop stack, and it supports Hadoop jobs for Apache MapReduce, Apache Pig, Apache Hive, and Apache Sqoop.</span></span> <span data-ttu-id="f5de8-114">Azt is, amelyek adott tooa rendszerén, például Java programok vagy héjparancsfájlok ütemezésére használt tooschedule feladatok.</span><span class="sxs-lookup"><span data-stu-id="f5de8-114">It can also be used tooschedule jobs that are specific tooa system, such as Java programs or shell scripts.</span></span>

<span data-ttu-id="f5de8-115">hello következő kép bemutatja hello munkafolyamat fogja végrehajtani:</span><span class="sxs-lookup"><span data-stu-id="f5de8-115">hello following image shows hello workflow you will implement:</span></span>

![Munkafolyamat diagramja][img-workflow-diagram]

<span data-ttu-id="f5de8-117">hello munkafolyamat két műveletet tartalmaz:</span><span class="sxs-lookup"><span data-stu-id="f5de8-117">hello workflow contains two actions:</span></span>

1. <span data-ttu-id="f5de8-118">A Hive művelet a futás a HiveQL parancsfájl toocount hello napló szintű típusonkénti előfordulások log4j naplófájlban.</span><span class="sxs-lookup"><span data-stu-id="f5de8-118">A Hive action runs a HiveQL script toocount hello occurrences of each log-level type in a log4j log file.</span></span> <span data-ttu-id="f5de8-119">Minden egyes log4j napló például a [NAPLÓZÁSI szint] mező tooshow hello típusát és hello súlyossága, tartalmazó sor mezők áll:</span><span class="sxs-lookup"><span data-stu-id="f5de8-119">Each log4j log consists of a line of fields that contains a [LOG LEVEL] field tooshow hello type and hello severity, for example:</span></span>

        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...

    <span data-ttu-id="f5de8-120">hello Hive parancsfájl kimenetében hasonlít:</span><span class="sxs-lookup"><span data-stu-id="f5de8-120">hello Hive script output is similar to:</span></span>

        [DEBUG] 434
        [ERROR] 3
        [FATAL] 1
        [INFO]  96
        [TRACE] 816
        [WARN]  4

    <span data-ttu-id="f5de8-121">További információ a Hive-ról: [A Hive használata a HDInsightban][hdinsight-use-hive].</span><span class="sxs-lookup"><span data-stu-id="f5de8-121">For more information about Hive, see [Use Hive with HDInsight][hdinsight-use-hive].</span></span>
2. <span data-ttu-id="f5de8-122">A Sqoop művelet Azure SQL adatbázis hello tooa HiveQL műveleti eredménytábla exportálja.</span><span class="sxs-lookup"><span data-stu-id="f5de8-122">A Sqoop action exports hello HiveQL action output tooa table in an Azure SQL database.</span></span> <span data-ttu-id="f5de8-123">További információ a Sqoop: [és a HDInsight együttes használata Sqoop][hdinsight-use-sqoop].</span><span class="sxs-lookup"><span data-stu-id="f5de8-123">For more information about Sqoop, see [Use Sqoop with HDInsight][hdinsight-use-sqoop].</span></span>

> [!NOTE]
> <span data-ttu-id="f5de8-124">Tekintse meg a HDInsight-fürtökön támogatott Oozie verziók [What's new in HDInsight által biztosított hello fürt verziók?] [hdinsight-versions].</span><span class="sxs-lookup"><span data-stu-id="f5de8-124">For supported Oozie versions on HDInsight clusters, see [What's new in hello cluster versions provided by HDInsight?][hdinsight-versions].</span></span>
>
>

## <a name="prerequisites"></a><span data-ttu-id="f5de8-125">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f5de8-125">Prerequisites</span></span>
<span data-ttu-id="f5de8-126">Ez az oktatóanyag elkezdéséhez hello következő kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="f5de8-126">Before you begin this tutorial, you must have hello following:</span></span>

* <span data-ttu-id="f5de8-127">**Munkaállomás Azure PowerShell-lel**.</span><span class="sxs-lookup"><span data-stu-id="f5de8-127">**A workstation with Azure PowerShell**.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="f5de8-128">A HDInsight-erőforrások Azure Service Managerrel történő kezelésének Azure PowerShell-támogatása **elavult**, így 2017. január 1-től megszűnik.</span><span class="sxs-lookup"><span data-stu-id="f5de8-128">Azure PowerShell support for managing HDInsight resources using Azure Service Manager is **deprecated**, and will be removed by January 1, 2017.</span></span> <span data-ttu-id="f5de8-129">a dokumentum használatát hello új HDInsight-parancsmagokat, amelyek működnek az Azure Resource Manager hello szükséges lépések.</span><span class="sxs-lookup"><span data-stu-id="f5de8-129">hello steps in this document use hello new HDInsight cmdlets that work with Azure Resource Manager.</span></span>
    >
    > <span data-ttu-id="f5de8-130">Kövesse a lépéseket hello [telepítse és konfigurálja az Azure Powershellt](/powershell/azureps-cmdlets-docs) tooinstall hello legújabb Azure PowerShell verziója.</span><span class="sxs-lookup"><span data-stu-id="f5de8-130">Please follow hello steps in [Install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) tooinstall hello latest version of Azure PowerShell.</span></span> <span data-ttu-id="f5de8-131">Ha vannak olyan parancsprogramjai, hogy szükség toobe módosított toouse hello új parancsmagok használata Azure Resource Managerrel működő című [áttelepítése tooAzure fejlesztési Resource Manager-alapú HDInsight-fürtök eszközei](hdinsight-hadoop-development-using-azure-resource-manager.md) további információt.</span><span class="sxs-lookup"><span data-stu-id="f5de8-131">If you have scripts that need toobe modified toouse hello new cmdlets that work with Azure Resource Manager, see [Migrating tooAzure Resource Manager-based development tools for HDInsight clusters](hdinsight-hadoop-development-using-azure-resource-manager.md) for more information.</span></span>

* <span data-ttu-id="f5de8-132">**HDInsight-fürtök**.</span><span class="sxs-lookup"><span data-stu-id="f5de8-132">**An HDInsight cluster**.</span></span> <span data-ttu-id="f5de8-133">HDInsight-fürtök létrehozásával kapcsolatos további információkért lásd: [HDInsight-fürtök létrehozása][hdinsight-provision], vagy [első lépései a hdinsight eszközzel][hdinsight-get-started].</span><span class="sxs-lookup"><span data-stu-id="f5de8-133">For information about creating an HDInsight cluster, see [Create HDInsight clusters][hdinsight-provision], or [Get started with HDInsight][hdinsight-get-started].</span></span> <span data-ttu-id="f5de8-134">A következő adatok toogo hello oktatóanyag hello lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="f5de8-134">You will need hello following data toogo through hello tutorial:</span></span>

    <table border = "1">
    <tr><th><span data-ttu-id="f5de8-135">Fürt tulajdonság</span><span class="sxs-lookup"><span data-stu-id="f5de8-135">Cluster property</span></span></th><th><span data-ttu-id="f5de8-136">A Windows PowerShell-változó neve</span><span class="sxs-lookup"><span data-stu-id="f5de8-136">Windows PowerShell variable name</span></span></th><th><span data-ttu-id="f5de8-137">Érték</span><span class="sxs-lookup"><span data-stu-id="f5de8-137">Value</span></span></th><th><span data-ttu-id="f5de8-138">Leírás</span><span class="sxs-lookup"><span data-stu-id="f5de8-138">Description</span></span></th></tr>
    <tr><td><span data-ttu-id="f5de8-139">A HDInsight-fürt neve</span><span class="sxs-lookup"><span data-stu-id="f5de8-139">HDInsight cluster name</span></span></td><td><span data-ttu-id="f5de8-140">$clusterName</span><span class="sxs-lookup"><span data-stu-id="f5de8-140">$clusterName</span></span></td><td></td><td><span data-ttu-id="f5de8-141">hello HDInsight fürt, amelyen futtatja az oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="f5de8-141">hello HDInsight cluster on which you will run this tutorial.</span></span></td></tr>
    <tr><td><span data-ttu-id="f5de8-142">A HDInsight fürt felhasználónév</span><span class="sxs-lookup"><span data-stu-id="f5de8-142">HDInsight cluster username</span></span></td><td><span data-ttu-id="f5de8-143">$clusterUsername</span><span class="sxs-lookup"><span data-stu-id="f5de8-143">$clusterUsername</span></span></td><td></td><td><span data-ttu-id="f5de8-144">hello HDInsight fürt felhasználó felhasználóneve.</span><span class="sxs-lookup"><span data-stu-id="f5de8-144">hello HDInsight cluster user name.</span></span> </td></tr>
    <tr><td><span data-ttu-id="f5de8-145">A HDInsight fürt felhasználói jelszó</span><span class="sxs-lookup"><span data-stu-id="f5de8-145">HDInsight cluster user password</span></span> </td><td><span data-ttu-id="f5de8-146">$clusterPassword</span><span class="sxs-lookup"><span data-stu-id="f5de8-146">$clusterPassword</span></span></td><td></td><td><span data-ttu-id="f5de8-147">hello HDInsight fürt felhasználói jelszavát.</span><span class="sxs-lookup"><span data-stu-id="f5de8-147">hello HDInsight cluster user password.</span></span></td></tr>
    <tr><td><span data-ttu-id="f5de8-148">Az Azure storage-fiók neve</span><span class="sxs-lookup"><span data-stu-id="f5de8-148">Azure storage account name</span></span></td><td><span data-ttu-id="f5de8-149">$storageAccountName</span><span class="sxs-lookup"><span data-stu-id="f5de8-149">$storageAccountName</span></span></td><td></td><td><span data-ttu-id="f5de8-150">Azure Storage fiók elérhető toohello HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="f5de8-150">An Azure Storage account available toohello HDInsight cluster.</span></span> <span data-ttu-id="f5de8-151">Ebben az oktatóanyagban hello alapértelmezett tárfiókot, hello fürt kiépítési folyamat során adott meg használni.</span><span class="sxs-lookup"><span data-stu-id="f5de8-151">For this tutorial, use hello default storage account that you specified during hello cluster provision process.</span></span></td></tr>
    <tr><td><span data-ttu-id="f5de8-152">Az Azure Blob-tároló neve</span><span class="sxs-lookup"><span data-stu-id="f5de8-152">Azure Blob container name</span></span></td><td><span data-ttu-id="f5de8-153">$containerName</span><span class="sxs-lookup"><span data-stu-id="f5de8-153">$containerName</span></span></td><td></td><td><span data-ttu-id="f5de8-154">Ebben a példában hello Azure Blob storage tárolók használt hello alapértelmezett HDInsight-fürt fájlrendszert használja.</span><span class="sxs-lookup"><span data-stu-id="f5de8-154">For this example, use hello Azure Blob storage container that is used for hello default HDInsight cluster file system.</span></span> <span data-ttu-id="f5de8-155">Alapértelmezés szerint hello azonos nevet hello HDInsight-fürttel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="f5de8-155">By default, it has hello same name as hello HDInsight cluster.</span></span></td></tr>
    </table><span data-ttu-id="f5de8-156">
* **Azure SQL-adatbázis**.</span><span class="sxs-lookup"><span data-stu-id="f5de8-156">
* **An Azure SQL database**.</span></span> <span data-ttu-id="f5de8-157">Konfigurálnia kell egy tűzfalszabályt hello SQL adatbázis-kiszolgáló tooallow hozzáférés a munkaállomáson.</span><span class="sxs-lookup"><span data-stu-id="f5de8-157">You must configure a firewall rule for hello SQL Database server tooallow access from your workstation.</span></span> <span data-ttu-id="f5de8-158">Egy Azure SQL-adatbázis létrehozása, és hello tűzfal konfigurálásával kapcsolatos útmutatásért lásd: [használatának első Azure SQL adatbázis] [sqldatabase-get-started].</span><span class="sxs-lookup"><span data-stu-id="f5de8-158">For instructions about creating an Azure SQL database and configuring hello firewall, see [Get started using Azure SQL database][sqldatabase-get-started].</span></span> <span data-ttu-id="f5de8-159">A cikkben egy Windows PowerShell-parancsfájl létrehozásához hello Azure SQL-adatbázistáblában szereplő, amelyekre szüksége van ehhez az oktatóanyaghoz.</span><span class="sxs-lookup"><span data-stu-id="f5de8-159">This article provides a Windows PowerShell script for creating hello Azure SQL database table that you need for this tutorial.</span></span>

    <table border = "1">
    <tr><th><span data-ttu-id="f5de8-160">SQL-adatbázis tulajdonság</span><span class="sxs-lookup"><span data-stu-id="f5de8-160">SQL database property</span></span></th><th><span data-ttu-id="f5de8-161">A Windows PowerShell-változó neve</span><span class="sxs-lookup"><span data-stu-id="f5de8-161">Windows PowerShell variable name</span></span></th><th><span data-ttu-id="f5de8-162">Érték</span><span class="sxs-lookup"><span data-stu-id="f5de8-162">Value</span></span></th><th><span data-ttu-id="f5de8-163">Leírás</span><span class="sxs-lookup"><span data-stu-id="f5de8-163">Description</span></span></th></tr>
    <tr><td><span data-ttu-id="f5de8-164">SQL adatbázis-kiszolgáló neve</span><span class="sxs-lookup"><span data-stu-id="f5de8-164">SQL database server name</span></span></td><td><span data-ttu-id="f5de8-165">$sqlDatabaseServer</span><span class="sxs-lookup"><span data-stu-id="f5de8-165">$sqlDatabaseServer</span></span></td><td></td><td><span data-ttu-id="f5de8-166">hello SQL database kiszolgáló toowhich Sqoop exportálja az adatokat.</span><span class="sxs-lookup"><span data-stu-id="f5de8-166">hello SQL database server toowhich Sqoop will export data.</span></span> </td></tr>
    <tr><td><span data-ttu-id="f5de8-167">SQL adatbázis-bejelentkezési név</span><span class="sxs-lookup"><span data-stu-id="f5de8-167">SQL database login name</span></span></td><td><span data-ttu-id="f5de8-168">$sqlDatabaseLogin</span><span class="sxs-lookup"><span data-stu-id="f5de8-168">$sqlDatabaseLogin</span></span></td><td></td><td><span data-ttu-id="f5de8-169">SQL adatbázis-bejelentkezési név.</span><span class="sxs-lookup"><span data-stu-id="f5de8-169">SQL Database login name.</span></span></td></tr>
    <tr><td><span data-ttu-id="f5de8-170">SQL-adatbázis a bejelentkezés jelszó</span><span class="sxs-lookup"><span data-stu-id="f5de8-170">SQL database login password</span></span></td><td><span data-ttu-id="f5de8-171">$sqlDatabaseLoginPassword</span><span class="sxs-lookup"><span data-stu-id="f5de8-171">$sqlDatabaseLoginPassword</span></span></td><td></td><td><span data-ttu-id="f5de8-172">SQL-adatbázis bejelentkezési jelszót.</span><span class="sxs-lookup"><span data-stu-id="f5de8-172">SQL Database login password.</span></span></td></tr>
    <tr><td><span data-ttu-id="f5de8-173">SQL-adatbázis neve</span><span class="sxs-lookup"><span data-stu-id="f5de8-173">SQL database name</span></span></td><td><span data-ttu-id="f5de8-174">$sqlDatabaseName</span><span class="sxs-lookup"><span data-stu-id="f5de8-174">$sqlDatabaseName</span></span></td><td></td><td><span data-ttu-id="f5de8-175">hello Azure SQL adatbázis toowhich Sqoop exportálja az adatokat.</span><span class="sxs-lookup"><span data-stu-id="f5de8-175">hello Azure SQL database toowhich Sqoop will export data.</span></span> </td></tr>
    </table>

  > [!NOTE]
  > <span data-ttu-id="f5de8-176">Alapértelmezés szerint az Azure SQL database az Azure-szolgáltatások, például az Azure HDInsight engedélyezi a csatlakozást.</span><span class="sxs-lookup"><span data-stu-id="f5de8-176">By default an Azure SQL database allows connections from Azure Services, such as Azure HDInsight.</span></span> <span data-ttu-id="f5de8-177">Ha a tűzfal beállítás nincs engedélyezve, engedélyeznie kell azt a hello Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="f5de8-177">If this firewall setting is disabled, you must enable it from hello Azure Portal.</span></span> <span data-ttu-id="f5de8-178">Az utasítás SQL-adatbázis létrehozása és a tűzfal-szabályok konfigurálása, lásd: [létrehozása és konfigurálása az SQL-adatbázis][sqldatabase-get-started].</span><span class="sxs-lookup"><span data-stu-id="f5de8-178">For instruction about creating a SQL Database and configuring firewall rules, see [Create and Configure SQL Database][sqldatabase-get-started].</span></span>

> [!NOTE]
> <span data-ttu-id="f5de8-179">Kitöltési hello értékek hello táblában.</span><span class="sxs-lookup"><span data-stu-id="f5de8-179">Fill-in hello values in hello tables.</span></span> <span data-ttu-id="f5de8-180">Hasznos lehet az oktatóanyag lépéseinek lesz.</span><span class="sxs-lookup"><span data-stu-id="f5de8-180">It will be helpful for going through this tutorial.</span></span>

## <a name="define-oozie-workflow-and-hello-related-hiveql-script"></a><span data-ttu-id="f5de8-181">Adja meg az Oozie munkafolyamat és a hello kapcsolódó HiveQL-parancsfájlt</span><span class="sxs-lookup"><span data-stu-id="f5de8-181">Define Oozie workflow and hello related HiveQL script</span></span>
<span data-ttu-id="f5de8-182">Oozie munkafolyamatok definíciók hPDL (az XML folyamat definition language) nyelven íródtak.</span><span class="sxs-lookup"><span data-stu-id="f5de8-182">Oozie workflows definitions are written in hPDL (an XML process definition language).</span></span> <span data-ttu-id="f5de8-183">hello alapértelmezett munkafolyamat Fájlnév *workflow.xml*.</span><span class="sxs-lookup"><span data-stu-id="f5de8-183">hello default workflow file name is *workflow.xml*.</span></span>  <span data-ttu-id="f5de8-184">Mentse helyileg a hello munkafolyamat fájlt fog, és majd telepítenie kell azt toohello HDInsight-fürt Azure PowerShell használatával az oktatóanyag későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="f5de8-184">You will save hello workflow file locally, and then deploy it toohello HDInsight cluster by using Azure PowerShell later in this tutorial.</span></span>

<span data-ttu-id="f5de8-185">hello Hive művelet hello munkafolyamat meghívja a HiveQL-parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="f5de8-185">hello Hive action in hello workflow calls a HiveQL script file.</span></span> <span data-ttu-id="f5de8-186">A parancsfájl három HiveQL utasításokat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="f5de8-186">This script file contains three HiveQL statements:</span></span>

1. <span data-ttu-id="f5de8-187">**hello DROP TABLE utasítás** törlések hello log4j Hive táblát, ha van ilyen.</span><span class="sxs-lookup"><span data-stu-id="f5de8-187">**hello DROP TABLE statement** deletes hello log4j Hive table if it exists.</span></span>
2. <span data-ttu-id="f5de8-188">**CREATE TABLE utasítás hello** log4j Hive külső tábla létrehozása, amely hello log4j naplófájl; toohello helyre mutat</span><span class="sxs-lookup"><span data-stu-id="f5de8-188">**hello CREATE TABLE statement** creates a log4j Hive external table, which points toohello location of hello log4j log file;</span></span>
3. <span data-ttu-id="f5de8-189">**hello log4j naplófájl helye hello**.</span><span class="sxs-lookup"><span data-stu-id="f5de8-189">**hello location of hello log4j log file**.</span></span> <span data-ttu-id="f5de8-190">hello mező határoló ",".</span><span class="sxs-lookup"><span data-stu-id="f5de8-190">hello field delimiter is ",".</span></span> <span data-ttu-id="f5de8-191">hello alapértelmezett sor elválasztó karaktere "\n".</span><span class="sxs-lookup"><span data-stu-id="f5de8-191">hello default line delimiter is "\n".</span></span> <span data-ttu-id="f5de8-192">A Hive külső tábla használt tooavoid hello adatfájl távolít el hello eredeti helyére,, abban az esetben, ha azt szeretné toorun hello Oozie munkafolyamat több alkalommal.</span><span class="sxs-lookup"><span data-stu-id="f5de8-192">A Hive external table is used tooavoid hello data file being removed from hello original location, in case you want toorun hello Oozie workflow multiple times.</span></span>
4. <span data-ttu-id="f5de8-193">**hello FELÜLÍRÁSA INSERT utasítás** a napló szintű típusonkénti hello előfordulások száma hello log4j Hive táblát, és hello kimeneti tooan Azure Blob storage helyre menti.</span><span class="sxs-lookup"><span data-stu-id="f5de8-193">**hello INSERT OVERWRITE statement** counts hello occurrences of each log-level type from hello log4j Hive table, and it saves hello output tooan Azure Blob storage location.</span></span>

> [!NOTE]
> <span data-ttu-id="f5de8-194">Nincs olyan ismert Hive útvonallal kapcsolatos probléma.</span><span class="sxs-lookup"><span data-stu-id="f5de8-194">There is a known Hive path issue.</span></span> <span data-ttu-id="f5de8-195">Ha egy Oozie feladat elküldése elindul ezt a problémát.</span><span class="sxs-lookup"><span data-stu-id="f5de8-195">You will run into this problem when submitting an Oozie job.</span></span> <span data-ttu-id="f5de8-196">hello utasításokat a hello a probléma kijavítása találhatók a TechNet Wiki hello: [HDInsight Hive-hiba: nem toorename][technetwiki-hive-error].</span><span class="sxs-lookup"><span data-stu-id="f5de8-196">hello instructions for fixing hello issue can be found on hello TechNet Wiki: [HDInsight Hive error: Unable toorename][technetwiki-hive-error].</span></span>

<span data-ttu-id="f5de8-197">**toodefine hello HiveQL parancsfájl fájl toobe hello munkafolyamat által meghívott**</span><span class="sxs-lookup"><span data-stu-id="f5de8-197">**toodefine hello HiveQL script file toobe called by hello workflow**</span></span>

1. <span data-ttu-id="f5de8-198">Hozzon létre egy szövegfájlt az alábbi hello tartalom:</span><span class="sxs-lookup"><span data-stu-id="f5de8-198">Create a text file with hello following content:</span></span>

        DROP TABLE ${hiveTableName};
        CREATE EXTERNAL TABLE ${hiveTableName}(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
        INSERT OVERWRITE DIRECTORY '${hiveOutputFolder}' SELECT t4 AS sev, COUNT(*) AS cnt FROM ${hiveTableName} WHERE t4 LIKE '[%' GROUP BY t4;

    <span data-ttu-id="f5de8-199">Három változók hello parancsfájl használatban van:</span><span class="sxs-lookup"><span data-stu-id="f5de8-199">There are three variables used in hello script:</span></span>

   * <span data-ttu-id="f5de8-200">${hiveTableName}</span><span class="sxs-lookup"><span data-stu-id="f5de8-200">${hiveTableName}</span></span>
   * <span data-ttu-id="f5de8-201">${hiveDataFolder}</span><span class="sxs-lookup"><span data-stu-id="f5de8-201">${hiveDataFolder}</span></span>
   * <span data-ttu-id="f5de8-202">${hiveOutputFolder}</span><span class="sxs-lookup"><span data-stu-id="f5de8-202">${hiveOutputFolder}</span></span>

     <span data-ttu-id="f5de8-203">hello munkafolyamat csomagdefiníciós fájl (ebben az oktatóanyagban workflow.xml) futási időben ezen értékek toothis HiveQL-parancsfájlt sikeres lesz.</span><span class="sxs-lookup"><span data-stu-id="f5de8-203">hello workflow definition file (workflow.xml in this tutorial) will pass these values toothis HiveQL script at run time.</span></span>
2. <span data-ttu-id="f5de8-204">Hello fájl mentése másként **C:\Tutorials\UseOozie\useooziewf.hql** ANSI (ASCII) kódolással.</span><span class="sxs-lookup"><span data-stu-id="f5de8-204">Save hello file as **C:\Tutorials\UseOozie\useooziewf.hql** by using ANSI (ASCII) encoding.</span></span> <span data-ttu-id="f5de8-205">(Használja a Jegyzettömböt, ha a szövegszerkesztőben nem adja meg ezt a lehetőséget.) A parancsfájl telepített toohello HDInsight-fürt lesz hello oktatóanyag későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="f5de8-205">(Use Notepad if your text editor doesn't provide this option.) This script file will be deployed toohello HDInsight cluster later in hello tutorial.</span></span>

<span data-ttu-id="f5de8-206">**egy munkafolyamat toodefine**</span><span class="sxs-lookup"><span data-stu-id="f5de8-206">**toodefine a workflow**</span></span>

1. <span data-ttu-id="f5de8-207">Hozzon létre egy szövegfájlt az alábbi hello tartalom:</span><span class="sxs-lookup"><span data-stu-id="f5de8-207">Create a text file with hello following content:</span></span>

    ```xml
    <workflow-app name="useooziewf" xmlns="uri:oozie:workflow:0.2">
        <start too= "RunHiveScript"/>

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

    <span data-ttu-id="f5de8-208">Nincsenek definiált hello munkafolyamat két műveleteket.</span><span class="sxs-lookup"><span data-stu-id="f5de8-208">There are two actions defined in hello workflow.</span></span> <span data-ttu-id="f5de8-209">hello start-tooaction van *RunHiveScript*.</span><span class="sxs-lookup"><span data-stu-id="f5de8-209">hello start-tooaction is *RunHiveScript*.</span></span> <span data-ttu-id="f5de8-210">Ha hello művelet a futás *OK*, hello a következő művelet *RunSqoopExport*.</span><span class="sxs-lookup"><span data-stu-id="f5de8-210">If hello action runs *OK*, hello next action is *RunSqoopExport*.</span></span>

    <span data-ttu-id="f5de8-211">hello RunHiveScript több változót tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="f5de8-211">hello RunHiveScript has several variables.</span></span> <span data-ttu-id="f5de8-212">Hello értékeket fogja továbbítani, amikor hello Oozie feladat munkaállomáson Azure PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="f5de8-212">You will pass hello values when you submit hello Oozie job from your workstation by using Azure PowerShell.</span></span>

    <span data-ttu-id="f5de8-213">Munkafolyamat-változók</span><span class="sxs-lookup"><span data-stu-id="f5de8-213">Workflow variables</span></span>

    <table border = "1">
    <tr><th><span data-ttu-id="f5de8-214">Munkafolyamat-változók</span><span class="sxs-lookup"><span data-stu-id="f5de8-214">Workflow variables</span></span></th><th><span data-ttu-id="f5de8-215">Leírás</span><span class="sxs-lookup"><span data-stu-id="f5de8-215">Description</span></span></th></tr>
    <tr><td><span data-ttu-id="f5de8-216">${jobTracker}</span><span class="sxs-lookup"><span data-stu-id="f5de8-216">${jobTracker}</span></span></td><td><span data-ttu-id="f5de8-217">Adja meg a Hadoop-feladat követő hello hello URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="f5de8-217">Specify hello URL of hello Hadoop job tracker.</span></span> <span data-ttu-id="f5de8-218">Használjon <strong>jobtrackerhost:9010</strong> a HDInsight fürt 3.0-s és 2.0-s verzióját.</span><span class="sxs-lookup"><span data-stu-id="f5de8-218">Use <strong>jobtrackerhost:9010</strong> on HDInsight cluster version 3.0 and 2.0.</span></span></td></tr>
    <tr><td><span data-ttu-id="f5de8-219">${nameNode}</span><span class="sxs-lookup"><span data-stu-id="f5de8-219">${nameNode}</span></span></td><td><span data-ttu-id="f5de8-220">Adja meg a hello Hadoop neve csomópont hello URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="f5de8-220">Specify hello URL of hello Hadoop name node.</span></span> <span data-ttu-id="f5de8-221">Használja a hello alapértelmezett fájl rendszer wasb: / / címmel, például <i>wasb: / /&lt;containerName&gt;@&lt;storageAccountName&gt;. blob.core.windows.net</i>.</span><span class="sxs-lookup"><span data-stu-id="f5de8-221">Use hello default file system wasb:// address, for example, <i>wasb://&lt;containerName&gt;@&lt;storageAccountName&gt;.blob.core.windows.net</i>.</span></span></td></tr>
    <tr><td><span data-ttu-id="f5de8-222">${queueName}</span><span class="sxs-lookup"><span data-stu-id="f5de8-222">${queueName}</span></span></td><td><span data-ttu-id="f5de8-223">Adja meg a feladat hello hello várólistacímke elküldve.</span><span class="sxs-lookup"><span data-stu-id="f5de8-223">Specifies hello queue name that hello job will be submitted to.</span></span> <span data-ttu-id="f5de8-224">Használjon <strong>alapértelmezett</strong>.</span><span class="sxs-lookup"><span data-stu-id="f5de8-224">Use <strong>default</strong>.</span></span></td></tr>
    </table>

    <span data-ttu-id="f5de8-225">Hive művelet változói</span><span class="sxs-lookup"><span data-stu-id="f5de8-225">Hive action variables</span></span>

    <table border = "1">
    <tr><th><span data-ttu-id="f5de8-226">Műveleti változó struktúra</span><span class="sxs-lookup"><span data-stu-id="f5de8-226">Hive action variable</span></span></th><th><span data-ttu-id="f5de8-227">Leírás</span><span class="sxs-lookup"><span data-stu-id="f5de8-227">Description</span></span></th></tr>
    <tr><td><span data-ttu-id="f5de8-228">${hiveDataFolder}</span><span class="sxs-lookup"><span data-stu-id="f5de8-228">${hiveDataFolder}</span></span></td><td><span data-ttu-id="f5de8-229">Hive Create Table parancs hello hello forráskönyvtárat.</span><span class="sxs-lookup"><span data-stu-id="f5de8-229">hello source directory for hello Hive Create Table command.</span></span></td></tr>
    <tr><td><span data-ttu-id="f5de8-230">${hiveOutputFolder}</span><span class="sxs-lookup"><span data-stu-id="f5de8-230">${hiveOutputFolder}</span></span></td><td><span data-ttu-id="f5de8-231">kimeneti mappa hello hello FELÜLÍRÁSA INSERT utasítás.</span><span class="sxs-lookup"><span data-stu-id="f5de8-231">hello output folder for hello INSERT OVERWRITE statement.</span></span></td></tr>
    <tr><td><span data-ttu-id="f5de8-232">${hiveTableName}</span><span class="sxs-lookup"><span data-stu-id="f5de8-232">${hiveTableName}</span></span></td><td><span data-ttu-id="f5de8-233">hello Hive tábla által hivatkozott hello log4j adatfájlok hello neve.</span><span class="sxs-lookup"><span data-stu-id="f5de8-233">hello name of hello Hive table that references hello log4j data files.</span></span></td></tr>
    </table>

    <span data-ttu-id="f5de8-234">Sqoop művelet változói</span><span class="sxs-lookup"><span data-stu-id="f5de8-234">Sqoop action variables</span></span>

    <table border = "1">
    <tr><th><span data-ttu-id="f5de8-235">Sqoop művelet változó</span><span class="sxs-lookup"><span data-stu-id="f5de8-235">Sqoop action variable</span></span></th><th><span data-ttu-id="f5de8-236">Leírás</span><span class="sxs-lookup"><span data-stu-id="f5de8-236">Description</span></span></th></tr>
    <tr><td><span data-ttu-id="f5de8-237">${sqlDatabaseConnectionString}</span><span class="sxs-lookup"><span data-stu-id="f5de8-237">${sqlDatabaseConnectionString}</span></span></td><td><span data-ttu-id="f5de8-238">SQL adatbázis-kapcsolati karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="f5de8-238">SQL Database connection string.</span></span></td></tr>
    <tr><td><span data-ttu-id="f5de8-239">${sqlDatabaseTableName}</span><span class="sxs-lookup"><span data-stu-id="f5de8-239">${sqlDatabaseTableName}</span></span></td><td><span data-ttu-id="f5de8-240">hello Azure SQL adatbázis toowhere hello adatait exportálja.</span><span class="sxs-lookup"><span data-stu-id="f5de8-240">hello Azure SQL database table toowhere hello data will be exported.</span></span></td></tr>
    <tr><td><span data-ttu-id="f5de8-241">${hiveOutputFolder}</span><span class="sxs-lookup"><span data-stu-id="f5de8-241">${hiveOutputFolder}</span></span></td><td><span data-ttu-id="f5de8-242">a Hive BESZÚRÁSA FELÜLÍRÁSA utasítás hello hello a kimeneti mappa.</span><span class="sxs-lookup"><span data-stu-id="f5de8-242">hello output folder for hello Hive INSERT OVERWRITE statement.</span></span> <span data-ttu-id="f5de8-243">Ez a hello ugyanabban a mappában hello Sqoop export (Exportálás-dir).</span><span class="sxs-lookup"><span data-stu-id="f5de8-243">This is hello same folder for hello Sqoop export (export-dir).</span></span></td></tr>
    </table>

    <span data-ttu-id="f5de8-244">Oozie munkafolyamat és a munkafolyamat-műveleteket hello használatával kapcsolatos további információkért lásd: [Apache Oozie 4.0 dokumentáció] [ apache-oozie-400] (a HDInsight-fürt verziószáma 3.0) vagy [Apache Oozie 3.3.2 dokumentáció] [ apache-oozie-332] (a HDInsight-fürt verziószáma 2.1-es).</span><span class="sxs-lookup"><span data-stu-id="f5de8-244">For more information about Oozie workflow and using hello workflow actions, see [Apache Oozie 4.0 documentation][apache-oozie-400] (for HDInsight cluster version 3.0) or [Apache Oozie 3.3.2 documentation][apache-oozie-332] (for HDInsight cluster version 2.1).</span></span>

1. <span data-ttu-id="f5de8-245">Hello fájl mentése másként **C:\Tutorials\UseOozie\workflow.xml** ANSI (ASCII) kódolással.</span><span class="sxs-lookup"><span data-stu-id="f5de8-245">Save hello file as **C:\Tutorials\UseOozie\workflow.xml** by using ANSI (ASCII) encoding.</span></span> <span data-ttu-id="f5de8-246">(Használja a Jegyzettömböt, ha a szövegszerkesztőben nem adja meg ezt a lehetőséget.)</span><span class="sxs-lookup"><span data-stu-id="f5de8-246">(Use Notepad if your text editor doesn't provide this option.)</span></span>

<span data-ttu-id="f5de8-247">**toodefine koordinátora**</span><span class="sxs-lookup"><span data-stu-id="f5de8-247">**toodefine coordinator**</span></span>

1. <span data-ttu-id="f5de8-248">Hozzon létre egy szövegfájlt az alábbi hello tartalom:</span><span class="sxs-lookup"><span data-stu-id="f5de8-248">Create a text file with hello following content:</span></span>

    ```xml
    <coordinator-app name="my_coord_app" frequency="${coordFrequency}" start="${coordStart}" end="${coordEnd}" timezone="${coordTimezone}" xmlns="uri:oozie:coordinator:0.4">
        <action>
            <workflow>
                <app-path>${wfPath}</app-path>
            </workflow>
        </action>
    </coordinator-app>
    ```

    <span data-ttu-id="f5de8-249">Öt változók hello csomagdefiníciós fájl használatban van:</span><span class="sxs-lookup"><span data-stu-id="f5de8-249">There are five variables used in hello definition file:</span></span>

   | <span data-ttu-id="f5de8-250">Változó</span><span class="sxs-lookup"><span data-stu-id="f5de8-250">Variable</span></span> | <span data-ttu-id="f5de8-251">Leírás</span><span class="sxs-lookup"><span data-stu-id="f5de8-251">Description</span></span> |
   | --- | --- |
   | <span data-ttu-id="f5de8-252">${coordFrequency}</span><span class="sxs-lookup"><span data-stu-id="f5de8-252">${coordFrequency}</span></span> |<span data-ttu-id="f5de8-253">A feladat felfüggesztése időpontja.</span><span class="sxs-lookup"><span data-stu-id="f5de8-253">Job pause time.</span></span> <span data-ttu-id="f5de8-254">Gyakoriság mindig kifejezett perc múlva.</span><span class="sxs-lookup"><span data-stu-id="f5de8-254">Frequency is always expressed in minutes.</span></span> |
   | <span data-ttu-id="f5de8-255">${coordStart}</span><span class="sxs-lookup"><span data-stu-id="f5de8-255">${coordStart}</span></span> |<span data-ttu-id="f5de8-256">A feladat kezdési időpontja.</span><span class="sxs-lookup"><span data-stu-id="f5de8-256">Job start time.</span></span> |
   | <span data-ttu-id="f5de8-257">${coordEnd}</span><span class="sxs-lookup"><span data-stu-id="f5de8-257">${coordEnd}</span></span> |<span data-ttu-id="f5de8-258">A feladat befejezési időpontja.</span><span class="sxs-lookup"><span data-stu-id="f5de8-258">Job end time.</span></span> |
   | <span data-ttu-id="f5de8-259">${coordTimezone}</span><span class="sxs-lookup"><span data-stu-id="f5de8-259">${coordTimezone}</span></span> |<span data-ttu-id="f5de8-260">Oozie coordinator feladatok rögzített időzónában dolgozza nem nyári időszámítás (általában képviseli UTC használatával).</span><span class="sxs-lookup"><span data-stu-id="f5de8-260">Oozie processes coordinator jobs in a fixed time zone with no daylight saving time (typically represented by using UTC).</span></span> <span data-ttu-id="f5de8-261">Ez az időzóna kezeli hello "Oozie feldolgozási időzóna."</span><span class="sxs-lookup"><span data-stu-id="f5de8-261">This time zone is referred as hello "Oozie processing timezone."</span></span> |
   | <span data-ttu-id="f5de8-262">${wfPath}</span><span class="sxs-lookup"><span data-stu-id="f5de8-262">${wfPath}</span></span> |<span data-ttu-id="f5de8-263">hello workflow.xml hello elérési útját.</span><span class="sxs-lookup"><span data-stu-id="f5de8-263">hello path for hello workflow.xml.</span></span>  <span data-ttu-id="f5de8-264">Ha hello munkafolyamat fájlnév nem hello alapértelmezett neve (workflow.xml), meg kell adnia.</span><span class="sxs-lookup"><span data-stu-id="f5de8-264">If hello workflow file name is not hello default file name (workflow.xml), you must specify it.</span></span> |
2. <span data-ttu-id="f5de8-265">Hello fájl mentése másként **C:\Tutorials\UseOozie\coordinator.xml** hello ANSI (ASCII) kódolással.</span><span class="sxs-lookup"><span data-stu-id="f5de8-265">Save hello file as **C:\Tutorials\UseOozie\coordinator.xml** by using hello ANSI (ASCII) encoding.</span></span> <span data-ttu-id="f5de8-266">(Használja a Jegyzettömböt, ha a szövegszerkesztőben nem adja meg ezt a lehetőséget.)</span><span class="sxs-lookup"><span data-stu-id="f5de8-266">(Use Notepad if your text editor doesn't provide this option.)</span></span>

## <a name="deploy-hello-oozie-project-and-prepare-hello-tutorial"></a><span data-ttu-id="f5de8-267">Hello Oozie projekt telepítése és előkészítése hello oktatóanyag</span><span class="sxs-lookup"><span data-stu-id="f5de8-267">Deploy hello Oozie project and prepare hello tutorial</span></span>
<span data-ttu-id="f5de8-268">Az Azure PowerShell parancsfájl tooperform hello következő futtatja:</span><span class="sxs-lookup"><span data-stu-id="f5de8-268">You will run an Azure PowerShell script tooperform hello following:</span></span>

* <span data-ttu-id="f5de8-269">Másolás hello HiveQL parancsfájl (useoozie.hql) tooAzure Blob-tároló, wasb:///tutorials/useoozie/useoozie.hql.</span><span class="sxs-lookup"><span data-stu-id="f5de8-269">Copy hello HiveQL script (useoozie.hql) tooAzure Blob storage, wasb:///tutorials/useoozie/useoozie.hql.</span></span>
* <span data-ttu-id="f5de8-270">Workflow.xml toowasb:///tutorials/useoozie/workflow.xml másolja.</span><span class="sxs-lookup"><span data-stu-id="f5de8-270">Copy workflow.xml toowasb:///tutorials/useoozie/workflow.xml.</span></span>
* <span data-ttu-id="f5de8-271">Coordinator.xml toowasb:///tutorials/useoozie/coordinator.xml másolja.</span><span class="sxs-lookup"><span data-stu-id="f5de8-271">Copy coordinator.xml toowasb:///tutorials/useoozie/coordinator.xml.</span></span>
* <span data-ttu-id="f5de8-272">Hello adatok fájl másolása (/ example/data/sample.log) toowasb:///tutorials/useoozie/data/sample.log.</span><span class="sxs-lookup"><span data-stu-id="f5de8-272">Copy hello data file (/example/data/sample.log) toowasb:///tutorials/useoozie/data/sample.log.</span></span>
* <span data-ttu-id="f5de8-273">Hozzon létre egy Azure SQL-adatbázistáblában szereplő Sqoop exportálási adatainak tárolásához.</span><span class="sxs-lookup"><span data-stu-id="f5de8-273">Create an Azure SQL database table for storing Sqoop export data.</span></span> <span data-ttu-id="f5de8-274">hello táblázat neve *log4jLogCount*.</span><span class="sxs-lookup"><span data-stu-id="f5de8-274">hello table name is *log4jLogCount*.</span></span>

<span data-ttu-id="f5de8-275">**HDInsight tároló ismertetése**</span><span class="sxs-lookup"><span data-stu-id="f5de8-275">**Understand HDInsight storage**</span></span>

<span data-ttu-id="f5de8-276">HDInsight Azure Blob storage-tárolására használ.</span><span class="sxs-lookup"><span data-stu-id="f5de8-276">HDInsight uses Azure Blob storage for data storage.</span></span> <span data-ttu-id="f5de8-277">wasb: / / hello Hadoop elosztott fájlrendszer (HDFS) az Azure Blob storage a Microsoft általi implementációja.</span><span class="sxs-lookup"><span data-stu-id="f5de8-277">wasb:// is Microsoft's implementation of hello Hadoop distributed file system (HDFS) in Azure Blob storage.</span></span> <span data-ttu-id="f5de8-278">További információkért lásd: [használata Azure Blob storage a hdinsight eszközzel][hdinsight-storage].</span><span class="sxs-lookup"><span data-stu-id="f5de8-278">For more information, see [Use Azure Blob storage with HDInsight][hdinsight-storage].</span></span>

<span data-ttu-id="f5de8-279">HDInsight-fürtök kiépítése, amikor egy Azure Blob storage-fiókot és egy adott tárolóhoz ebből a fiókból kijelölt hello alapértelmezett fájlrendszerben, például a HDFS-ben.</span><span class="sxs-lookup"><span data-stu-id="f5de8-279">When you provision an HDInsight cluster, an Azure Blob storage account and a specific container from that account is designated as hello default file system, like in HDFS.</span></span> <span data-ttu-id="f5de8-280">Ezenkívül toothis tárfiókot, hozzáadhat további tárfiókok a hello azonos Azure-előfizetés vagy az Azure-előfizetések hello kiépítési folyamat során.</span><span class="sxs-lookup"><span data-stu-id="f5de8-280">In addition toothis storage account, you can add additional storage accounts from hello same Azure subscription or from different Azure subscriptions during hello provisioning process.</span></span> <span data-ttu-id="f5de8-281">További tárfiókok hozzáadásáról útmutatásért lásd: [Provision HDInsight clusters][hdinsight-provision].</span><span class="sxs-lookup"><span data-stu-id="f5de8-281">For instructions about adding additional storage accounts, see [Provision HDInsight clusters][hdinsight-provision].</span></span> <span data-ttu-id="f5de8-282">toosimplify hello Azure PowerShell-parancsprogram ebben az oktatóanyagban használt összes fájljai hello alapértelmezett fájl rendszertárolóhoz hello helyen */oktatóanyagok/useoozie*.</span><span class="sxs-lookup"><span data-stu-id="f5de8-282">toosimplify hello Azure PowerShell script used in this tutorial, all of hello files are stored in hello default file system container located at */tutorials/useoozie*.</span></span> <span data-ttu-id="f5de8-283">Alapértelmezés szerint ebben a tárolóban van hello azonos nevet hello HDInsight fürt neveként.</span><span class="sxs-lookup"><span data-stu-id="f5de8-283">By default, this container has hello same name as hello HDInsight cluster name.</span></span>
<span data-ttu-id="f5de8-284">hello szintaxisa:</span><span class="sxs-lookup"><span data-stu-id="f5de8-284">hello syntax is:</span></span>

    wasb[s]://<ContainerName>@<StorageAccountName>.blob.core.windows.net/<path>/<filename>

> [!NOTE]
> <span data-ttu-id="f5de8-285">Csak hello *wasb: / /* szintaxis a HDInsight fürt 3.0-s verziója támogatott.</span><span class="sxs-lookup"><span data-stu-id="f5de8-285">Only hello *wasb://* syntax is supported in HDInsight cluster version 3.0.</span></span> <span data-ttu-id="f5de8-286">hello régebbi *asv: / /* HDInsight 2.1-es és 1.6 fürtök a szintaxis támogatott, de nem támogatott a HDInsight 3.0 fürtök.</span><span class="sxs-lookup"><span data-stu-id="f5de8-286">hello older *asv://* syntax is supported in HDInsight 2.1 and 1.6 clusters, but it is not supported in HDInsight 3.0 clusters.</span></span>
>
> <span data-ttu-id="f5de8-287">hello wasb: / / elérési út egy virtuális elérési út.</span><span class="sxs-lookup"><span data-stu-id="f5de8-287">hello wasb:// path is a virtual path.</span></span> <span data-ttu-id="f5de8-288">További információ: [használata Azure Blob storage a hdinsight eszközzel][hdinsight-storage].</span><span class="sxs-lookup"><span data-stu-id="f5de8-288">For more information see [Use Azure Blob storage with HDInsight][hdinsight-storage].</span></span>

<span data-ttu-id="f5de8-289">Hello alapértelmezett fájl rendszer tárolóban tárolt fájlt érhetők el a HDInsight-ból a következő URI-azonosítók (használok workflow.xml példaként) hello egyikével sem:</span><span class="sxs-lookup"><span data-stu-id="f5de8-289">A file that is stored in hello default file system container can be accessed from HDInsight by using any of hello following URIs (I am using workflow.xml as an example):</span></span>

    wasb://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/workflow.xml
    wasb:///tutorials/useoozie/workflow.xml
    /tutorials/useoozie/workflow.xml

<span data-ttu-id="f5de8-290">Ha azt szeretné, hogy közvetlenül a hello tárfiók tooaccess hello fájl, hello blob neve hello fájl van:</span><span class="sxs-lookup"><span data-stu-id="f5de8-290">If you want tooaccess hello file directly from hello storage account, hello blob name for hello file is:</span></span>

    tutorials/useoozie/workflow.xml

<span data-ttu-id="f5de8-291">**Hive belső és külső táblák ismertetése**</span><span class="sxs-lookup"><span data-stu-id="f5de8-291">**Understand Hive internal and external tables**</span></span>

<span data-ttu-id="f5de8-292">Néhány dolgot Hive belső és külső táblák tooknow van szüksége:</span><span class="sxs-lookup"><span data-stu-id="f5de8-292">There are a few things you need tooknow about Hive internal and external tables:</span></span>

* <span data-ttu-id="f5de8-293">hello CREATE TABLE parancs létrehoz egy belső tábla, más néven a felügyelt.</span><span class="sxs-lookup"><span data-stu-id="f5de8-293">hello CREATE TABLE command creates an internal table, also known as a managed table.</span></span> <span data-ttu-id="f5de8-294">hello adatfájl hello alapértelmezett tárolóban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="f5de8-294">hello data file must be located in hello default container.</span></span>
* <span data-ttu-id="f5de8-295">CREATE TABLE parancs hello áthelyezi hello adatok toohello fájl, / / adatraktár/hive<TableName> hello alapértelmezett tároló mappa.</span><span class="sxs-lookup"><span data-stu-id="f5de8-295">hello CREATE TABLE command moves hello data file toohello /hive/warehouse/<TableName> folder in hello default container.</span></span>
* <span data-ttu-id="f5de8-296">hello külső tábla létrehozása parancs létrehoz egy külső táblát.</span><span class="sxs-lookup"><span data-stu-id="f5de8-296">hello CREATE EXTERNAL TABLE command creates an external table.</span></span> <span data-ttu-id="f5de8-297">hello adatfájl hello alapértelmezett tároló kívül is kell elhelyezni.</span><span class="sxs-lookup"><span data-stu-id="f5de8-297">hello data file can be located outside hello default container.</span></span>
* <span data-ttu-id="f5de8-298">a külső tábla létrehozása parancs hello nem helyezi át a hello adatfájlt.</span><span class="sxs-lookup"><span data-stu-id="f5de8-298">hello CREATE EXTERNAL TABLE command does not move hello data file.</span></span>
* <span data-ttu-id="f5de8-299">hello külső tábla létrehozása parancs almappákban hello hely záradékban megadott hello mappában nem engedélyezi.</span><span class="sxs-lookup"><span data-stu-id="f5de8-299">hello CREATE EXTERNAL TABLE command doesn't allow any subfolders under hello folder that is specified in hello LOCATION clause.</span></span> <span data-ttu-id="f5de8-300">Ez oka hello miért hello oktatóanyag hello sample.log fájl másolatot készít.</span><span class="sxs-lookup"><span data-stu-id="f5de8-300">This is hello reason why hello tutorial makes a copy of hello sample.log file.</span></span>

<span data-ttu-id="f5de8-301">További információkért lásd: [HDInsight: Hive belső és külső táblák bevezetés][cindygross-hive-tables].</span><span class="sxs-lookup"><span data-stu-id="f5de8-301">For more information, see [HDInsight: Hive Internal and External Tables Intro][cindygross-hive-tables].</span></span>

<span data-ttu-id="f5de8-302">**tooprepare hello oktatóanyag**</span><span class="sxs-lookup"><span data-stu-id="f5de8-302">**tooprepare hello tutorial**</span></span>

1. <span data-ttu-id="f5de8-303">Nyissa meg hello Windows PowerShell ISE (hello Windows 8 kezdőképernyőn írja be **PowerShell_ISE**, és kattintson a **Windows PowerShell ISE**.</span><span class="sxs-lookup"><span data-stu-id="f5de8-303">Open hello Windows PowerShell ISE (in hello Windows 8 Start screen, type **PowerShell_ISE**, and then click **Windows PowerShell ISE**.</span></span> <span data-ttu-id="f5de8-304">További információkért lásd: [Windows PowerShell elindítása a Windows 8 és Windows][powershell-start]).</span><span class="sxs-lookup"><span data-stu-id="f5de8-304">For more information, see [Start Windows PowerShell on Windows 8 and Windows][powershell-start]).</span></span>
2. <span data-ttu-id="f5de8-305">Hello alsó ablaktáblában futtassa a következő parancs tooconnect tooyour Azure-előfizetés hello:</span><span class="sxs-lookup"><span data-stu-id="f5de8-305">In hello bottom pane, run hello following command tooconnect tooyour Azure subscription:</span></span>

    ```powershell
    Add-AzureAccount
    ```

    <span data-ttu-id="f5de8-306">Akkor lesz felszólító tooenter a Azure-fiók hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="f5de8-306">You will be prompted tooenter your Azure account credentials.</span></span> <span data-ttu-id="f5de8-307">Ez a módszer az előfizetés-kapcsolat hozzáadása túllépi az időkorlátot, és 12 óra elteltével fog toorun hello parancsmag újra.</span><span class="sxs-lookup"><span data-stu-id="f5de8-307">This method of adding a subscription connection times out, and after 12 hours, you will have toorun hello cmdlet again.</span></span>

   > [!NOTE]
   > <span data-ttu-id="f5de8-308">Ha több Azure-előfizetéssel rendelkezik, és hello alapértelmezett előfizetés nincs hello kívánt toouse, használja a hello <strong>válasszon-AzureSubscription</strong> parancsmag tooselect előfizetés.</span><span class="sxs-lookup"><span data-stu-id="f5de8-308">If you have multiple Azure subscriptions and hello default subscription is not hello one you want toouse, use hello <strong>Select-AzureSubscription</strong> cmdlet tooselect a subscription.</span></span>

3. <span data-ttu-id="f5de8-309">Másolja a következő parancsfájl hello parancsfájl ablaktáblára hello, és utána állítsa be a hello első hat változók:</span><span class="sxs-lookup"><span data-stu-id="f5de8-309">Copy hello following script into hello script pane, and then set hello first six variables:</span></span>

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

    # Oozie files for hello tutorial
    $hiveQLScript = "C:\Tutorials\UseOozie\useooziewf.hql"
    $workflowDefinition = "C:\Tutorials\UseOozie\workflow.xml"
    $coordDefinition =  "C:\Tutorials\UseOozie\coordinator.xml"

    # WASB folder for storing hello Oozie tutorial files.
    $destFolder = "tutorials/useoozie"  # Do NOT use hello long path here
    ```

    <span data-ttu-id="f5de8-310">Hello változók további leírását lásd: hello [Előfeltételek](#prerequisites) ebben az oktatóanyagban szakasz.</span><span class="sxs-lookup"><span data-stu-id="f5de8-310">For more descriptions of hello variables, see hello [Prerequisites](#prerequisites) section in this tutorial.</span></span>

4. <span data-ttu-id="f5de8-311">A következő toohello parancsfájl hello parancsfájl ablaktáblán hello hozzáfűzése:</span><span class="sxs-lookup"><span data-stu-id="f5de8-311">Append hello following toohello script in hello script pane:</span></span>

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
        Write-Host "Make a copy of hello sample.log file ... " -ForegroundColor Green
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

        #Create hello log4jLogsCount table
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

    # make a copy of example/data/sample.log tooexample/data/log4j/sample.log
    prepareHiveDataFile;

    # create log4jlogsCount table on SQL database
    prepareSQLDatabase;
    ```

5. <span data-ttu-id="f5de8-312">Kattintson a **-parancsfájl futtatása** vagy nyomja le az ENTER **F5** toorun hello parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="f5de8-312">Click **Run Script** or press **F5** toorun hello script.</span></span> <span data-ttu-id="f5de8-313">hello kimeneti hasonló lesz:</span><span class="sxs-lookup"><span data-stu-id="f5de8-313">hello output will be similar to:</span></span>

    ![Útmutató előkészítési kimeneti][img-preparation-output]

## <a name="run-hello-oozie-project"></a><span data-ttu-id="f5de8-315">Hello Oozie-projekt futtatása</span><span class="sxs-lookup"><span data-stu-id="f5de8-315">Run hello Oozie project</span></span>
<span data-ttu-id="f5de8-316">Az Azure PowerShell jelenleg nem biztosít semmilyen parancsmagok Oozie feladatok meghatározásához.</span><span class="sxs-lookup"><span data-stu-id="f5de8-316">Azure PowerShell currently doesn't provide any cmdlets for defining Oozie jobs.</span></span> <span data-ttu-id="f5de8-317">Használhatja a hello **Invoke-RestMethod** parancsmag tooinvoke Oozie webszolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="f5de8-317">You can use hello **Invoke-RestMethod** cmdlet tooinvoke Oozie web services.</span></span> <span data-ttu-id="f5de8-318">hello Oozie webszolgáltatások API egy HTTP REST API-t JSON.</span><span class="sxs-lookup"><span data-stu-id="f5de8-318">hello Oozie web services API is a HTTP REST JSON API.</span></span> <span data-ttu-id="f5de8-319">Hello Oozie webszolgáltatások API kapcsolatos további információkért lásd: [Apache Oozie 4.0 dokumentáció] [ apache-oozie-400] (a HDInsight-fürt verziószáma 3.0) vagy [Apache Oozie 3.3.2 dokumentáció] [ apache-oozie-332] (a HDInsight-fürt verziószáma 2.1-es).</span><span class="sxs-lookup"><span data-stu-id="f5de8-319">For more information about hello Oozie web services API, see [Apache Oozie 4.0 documentation][apache-oozie-400] (for HDInsight cluster version 3.0) or [Apache Oozie 3.3.2 documentation][apache-oozie-332] (for HDInsight cluster version 2.1).</span></span>

<span data-ttu-id="f5de8-320">**toosubmit egy Oozie feladat**</span><span class="sxs-lookup"><span data-stu-id="f5de8-320">**toosubmit an Oozie job**</span></span>

1. <span data-ttu-id="f5de8-321">Nyissa meg hello Windows PowerShell ISE (a Windows 8 kezdőképernyőn írja be **PowerShell_ISE**, és kattintson a **Windows PowerShell ISE**.</span><span class="sxs-lookup"><span data-stu-id="f5de8-321">Open hello Windows PowerShell ISE (in Windows 8 Start screen, type **PowerShell_ISE**, and then click **Windows PowerShell ISE**.</span></span> <span data-ttu-id="f5de8-322">További információkért lásd: [Windows PowerShell elindítása a Windows 8 és Windows][powershell-start]).</span><span class="sxs-lookup"><span data-stu-id="f5de8-322">For more information, see [Start Windows PowerShell on Windows 8 and Windows][powershell-start]).</span></span>
2. <span data-ttu-id="f5de8-323">Másolás hello alábbi parancsfájl hello parancsfájl ablaktáblára, és majd a készlet első 14 változók hello (azonban kihagyása **$storageUri**).</span><span class="sxs-lookup"><span data-stu-id="f5de8-323">Copy hello following script into hello script pane, and then set hello first fourteen variables (however, skip **$storageUri**).</span></span>

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

    $oozieWFPath="$storageUri/tutorials/useoozie"  # hello default name is workflow.xml. And you don't need toospecify hello file name.
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

    <span data-ttu-id="f5de8-324">Hello változók további leírását lásd: hello [Előfeltételek](#prerequisites) ebben az oktatóanyagban szakasz.</span><span class="sxs-lookup"><span data-stu-id="f5de8-324">For more descriptions of hello variables, see hello [Prerequisites](#prerequisites) section in this tutorial.</span></span>

    <span data-ttu-id="f5de8-325">$coordstart és $coordend hello munkafolyamat kezdési és befejezési idő.</span><span class="sxs-lookup"><span data-stu-id="f5de8-325">$coordstart and $coordend are hello workflow starting and ending time.</span></span> <span data-ttu-id="f5de8-326">toofind kimenő hello UTC/GMT szerinti idő, keressen a "UTC szerinti idő" bing.com. hello $coordFrequency milyen gyakran toorun hello munkafolyamat percben van.</span><span class="sxs-lookup"><span data-stu-id="f5de8-326">toofind out hello UTC/GMT time, search "utc time" on bing.com. hello $coordFrequency is how often in minutes you want toorun hello workflow.</span></span>
3. <span data-ttu-id="f5de8-327">A következő toohello parancsfájl hello hozzáfűzése.</span><span class="sxs-lookup"><span data-stu-id="f5de8-327">Append hello following toohello script.</span></span> <span data-ttu-id="f5de8-328">Meghatározza, hogy ez a kijelző hello Oozie hasznos:</span><span class="sxs-lookup"><span data-stu-id="f5de8-328">This part defines hello Oozie payload:</span></span>

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
   > <span data-ttu-id="f5de8-329">hello képest jelentős különbség toohello munkafolyamat küldésének hasznos fájl hello változó **oozie.coord.application.path**.</span><span class="sxs-lookup"><span data-stu-id="f5de8-329">hello major difference compared toohello workflow submission payload file is hello variable **oozie.coord.application.path**.</span></span> <span data-ttu-id="f5de8-330">Amikor egy munkafolyamat-feladat elküldéséhez használhatja **oozie.wf.application.path** helyette.</span><span class="sxs-lookup"><span data-stu-id="f5de8-330">When you submit a workflow job, you use **oozie.wf.application.path** instead.</span></span>

4. <span data-ttu-id="f5de8-331">A következő toohello parancsfájl hello hozzáfűzése.</span><span class="sxs-lookup"><span data-stu-id="f5de8-331">Append hello following toohello script.</span></span> <span data-ttu-id="f5de8-332">Ez a kijelző hello Oozie webes szolgáltatás állapotát ellenőrzi:</span><span class="sxs-lookup"><span data-stu-id="f5de8-332">This part checks hello Oozie web service status:</span></span>

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
            Write-Host "Oozie server status is $oozieServerSatus...cannot submit Oozie jobs. Check hello server status and re-run hello job."
            exit 1
        }
    }
    ```

5. <span data-ttu-id="f5de8-333">A következő toohello parancsfájl hello hozzáfűzése.</span><span class="sxs-lookup"><span data-stu-id="f5de8-333">Append hello following toohello script.</span></span> <span data-ttu-id="f5de8-334">Ez a kijelző egy Oozie feladat létrehozása:</span><span class="sxs-lookup"><span data-stu-id="f5de8-334">This part creates an Oozie job:</span></span>

    ```powershell
    function createOozieJob()
    {
        # create Oozie job
        Write-Host "Sending hello following Payload toohello cluster:" -ForegroundColor Green
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
   > <span data-ttu-id="f5de8-335">Amikor egy munkafolyamat-feladat, meg kell nyitnia egy másik webes szolgáltatás hívása toostart hello feladat hello feladat létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="f5de8-335">When you submit a workflow job, you must make another web service call toostart hello job after hello job is created.</span></span> <span data-ttu-id="f5de8-336">Ebben az esetben hello koordinátor feladat idő váltja ki.</span><span class="sxs-lookup"><span data-stu-id="f5de8-336">In this case, hello coordinator job is triggered by time.</span></span> <span data-ttu-id="f5de8-337">hello feladat automatikusan elindul.</span><span class="sxs-lookup"><span data-stu-id="f5de8-337">hello job will start automatically.</span></span>

6. <span data-ttu-id="f5de8-338">A következő toohello parancsfájl hello hozzáfűzése.</span><span class="sxs-lookup"><span data-stu-id="f5de8-338">Append hello following toohello script.</span></span> <span data-ttu-id="f5de8-339">Ez a kijelző hello Oozie feladat állapotát ellenőrzi:</span><span class="sxs-lookup"><span data-stu-id="f5de8-339">This part checks hello Oozie job status:</span></span>

    ```powershell
    function checkOozieJobStatus($oozieJobId)
    {
        # get job status
        Write-Host "Sleeping for $waitTimeBetweenOozieJobStatusCheck seconds until hello job metadata is populated in hello Oozie metastore..." -ForegroundColor Green
        Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck

        Write-Host "Getting job status and waiting for hello job toocomplete..." -ForegroundColor Green
        $clusterUriGetJobStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?show=info"
        $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $creds
        $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
        $JobStatus = $jsonResponse[0].("status")

        while($JobStatus -notmatch "SUCCEEDED|KILLED")
        {
            Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state...waiting $waitTimeBetweenOozieJobStatusCheck seconds for hello job toocomplete..."
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

7. <span data-ttu-id="f5de8-340">(Választható) A következő toohello parancsfájl hello hozzáfűzése.</span><span class="sxs-lookup"><span data-stu-id="f5de8-340">(Optional) Append hello following toohello script.</span></span>

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
        Write-Host "Killing hello Oozie job $oozieJobId..." -ForegroundColor Green
        $clusterUriStartJob = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?action=kill" #Valid values for hello 'action' parameter are 'start', 'suspend', 'resume', 'kill', 'dryrun', 'rerun', and 'change'.
        $response = Invoke-RestMethod -Method Put -Uri $clusterUriStartJob -Credential $creds | Format-Table -HideTableHeaders -debug
    }
    ```

8. <span data-ttu-id="f5de8-341">A következő toohello parancsfájl hello hozzáfűzése:</span><span class="sxs-lookup"><span data-stu-id="f5de8-341">Append hello following toohello script:</span></span>

    ```powershell
    checkOozieServerStatus
    # listOozieJobs
    $oozieJobId = createOozieJob($oozieJobId)
    checkOozieJobStatus($oozieJobId)
    # ShowOozieJobLog($oozieJobId)
    # killOozieJob($oozieJobId)
    ```

    <span data-ttu-id="f5de8-342">Távolítsa el a hello # jelet, ha azt szeretné, hogy toorun hello további funkciók.</span><span class="sxs-lookup"><span data-stu-id="f5de8-342">Remove hello # signs if you want toorun hello additional functions.</span></span>
9. <span data-ttu-id="f5de8-343">Ha a HDinsight-fürt 2.1-es verziója, cserélje le a "https://$clusterName.azurehdinsight.net:443/oozie/v2/" a "https://$clusterName.azurehdinsight.net:443/oozie/v1/".</span><span class="sxs-lookup"><span data-stu-id="f5de8-343">If your HDinsight cluster is version 2.1, replace "https://$clusterName.azurehdinsight.net:443/oozie/v2/" with "https://$clusterName.azurehdinsight.net:443/oozie/v1/".</span></span> <span data-ttu-id="f5de8-344">HDInsight-fürt verziószáma 2.1 nem támogatja a 2-es verziójának hello webszolgáltatások végzi.</span><span class="sxs-lookup"><span data-stu-id="f5de8-344">HDInsight cluster version 2.1 does not supports version 2 of hello web services.</span></span>
10. <span data-ttu-id="f5de8-345">Kattintson a **-parancsfájl futtatása** vagy nyomja le az ENTER **F5** toorun hello parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="f5de8-345">Click **Run Script** or press **F5** toorun hello script.</span></span> <span data-ttu-id="f5de8-346">hello kimeneti hasonló lesz:</span><span class="sxs-lookup"><span data-stu-id="f5de8-346">hello output will be similar to:</span></span>

     ![Útmutató munkafolyamat kimeneti futtatása][img-runworkflow-output]
11. <span data-ttu-id="f5de8-348">Csatlakozás SQL Database toosee hello exportált adatok tooyour.</span><span class="sxs-lookup"><span data-stu-id="f5de8-348">Connect tooyour SQL Database toosee hello exported data.</span></span>

<span data-ttu-id="f5de8-349">**toocheck hello feladat hibanapló**</span><span class="sxs-lookup"><span data-stu-id="f5de8-349">**toocheck hello job error log**</span></span>

<span data-ttu-id="f5de8-350">egy munkafolyamat tootroubleshoot, hello Oozie naplófájl helyen találhatók C:\apps\dist\oozie-3.3.2.1.3.2.0-05\oozie-win-distro\logs\Oozie.log a hello fürt headnode.</span><span class="sxs-lookup"><span data-stu-id="f5de8-350">tootroubleshoot a workflow, hello Oozie log file can be found at C:\apps\dist\oozie-3.3.2.1.3.2.0-05\oozie-win-distro\logs\Oozie.log from hello cluster headnode.</span></span> <span data-ttu-id="f5de8-351">Az RDP információkért lásd: [felügyelete HDInsight-fürtök hello Azure-portálon][hdinsight-admin-portal].</span><span class="sxs-lookup"><span data-stu-id="f5de8-351">For information on RDP, see [Administering HDInsight clusters using hello Azure portal][hdinsight-admin-portal].</span></span>

<span data-ttu-id="f5de8-352">**toorerun hello oktatóanyag**</span><span class="sxs-lookup"><span data-stu-id="f5de8-352">**toorerun hello tutorial**</span></span>

<span data-ttu-id="f5de8-353">toorerun hello munkafolyamat, végezze el a következő feladatok hello:</span><span class="sxs-lookup"><span data-stu-id="f5de8-353">toorerun hello workflow, you must perform hello following tasks:</span></span>

* <span data-ttu-id="f5de8-354">Hello Hive parancsfájl kimeneti fájl törlése.</span><span class="sxs-lookup"><span data-stu-id="f5de8-354">Delete hello Hive script output file.</span></span>
* <span data-ttu-id="f5de8-355">Hello log4jLogsCount tábla hello adatok törlése.</span><span class="sxs-lookup"><span data-stu-id="f5de8-355">Delete hello data in hello log4jLogsCount table.</span></span>

<span data-ttu-id="f5de8-356">Íme egy minta Windows PowerShell-parancsfájl használható:</span><span class="sxs-lookup"><span data-stu-id="f5de8-356">Here is a sample Windows PowerShell script that you can use:</span></span>

```powershell
$storageAccountName = "<AzureStorageAccountName>"
$containerName = "<ContainerName>"

#SQL database variables
$sqlDatabaseServer = "<SQLDatabaseServerName>"
$sqlDatabaseLogin = "<SQLDatabaseLoginName>"
$sqlDatabaseLoginPassword = "<SQLDatabaseLoginPassword>"
$sqlDatabaseName = "<SQLDatabaseName>"
$sqlDatabaseTableName = "log4jLogsCount"

Write-host "Delete hello Hive script output file ..." -ForegroundColor Green
$storageaccountkey = get-azurestoragekey $storageAccountName | %{$_.Primary}
$destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey
Remove-AzureStorageBlob -Context $destContext -Blob "tutorials/useoozie/output/000000_0" -Container $containerName

Write-host "Delete all hello records from hello log4jLogsCount table ..." -ForegroundColor Green
$conn = New-Object System.Data.SqlClient.SqlConnection
$conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabaseLoginPassword;Encrypt=true;Trusted_Connection=false;"
$conn.open()
$cmd = New-Object System.Data.SqlClient.SqlCommand
$cmd.connection = $conn
$cmd.commandtext = "delete from $sqlDatabaseTableName"
$cmd.executenonquery()

$conn.close()
```

## <a name="next-steps"></a><span data-ttu-id="f5de8-357">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f5de8-357">Next steps</span></span>
<span data-ttu-id="f5de8-358">Ebben az oktatóanyagban, megtudta, hogyan toodefine egy Oozie munkafolyamat- és az Oozie-koordinátor, és hogyan toorun az Oozie-koordinátor feladat Azure PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="f5de8-358">In this tutorial, you learned how toodefine an Oozie workflow and an Oozie coordinator, and how toorun an Oozie coordinator job by using Azure PowerShell.</span></span> <span data-ttu-id="f5de8-359">toolearn több, tekintse meg a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="f5de8-359">toolearn more, see hello following articles:</span></span>

* <span data-ttu-id="f5de8-360">[Első lépései a hdinsight eszközzel][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="f5de8-360">[Get started with HDInsight][hdinsight-get-started]</span></span>
* <span data-ttu-id="f5de8-361">[Használhat Azure Blob Storage tárolót a hdinsight eszközzel][hdinsight-storage]</span><span class="sxs-lookup"><span data-stu-id="f5de8-361">[Use Azure Blob storage with HDInsight][hdinsight-storage]</span></span>
* <span data-ttu-id="f5de8-362">[HDInsight felügyelete az Azure PowerShell használatával][hdinsight-admin-powershell]</span><span class="sxs-lookup"><span data-stu-id="f5de8-362">[Administer HDInsight by using Azure PowerShell][hdinsight-admin-powershell]</span></span>
* <span data-ttu-id="f5de8-363">[Adatok tooHDInsight feltöltése][hdinsight-upload-data]</span><span class="sxs-lookup"><span data-stu-id="f5de8-363">[Upload data tooHDInsight][hdinsight-upload-data]</span></span>
* <span data-ttu-id="f5de8-364">[Use Sqoop with HDInsight][hdinsight-use-sqoop]</span><span class="sxs-lookup"><span data-stu-id="f5de8-364">[Use Sqoop with HDInsight][hdinsight-use-sqoop]</span></span>
* <span data-ttu-id="f5de8-365">[A Hive használata a HDInsightban][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="f5de8-365">[Use Hive with HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="f5de8-366">[A Pig használata a HDInsightban][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="f5de8-366">[Use Pig with HDInsight][hdinsight-use-pig]</span></span>
* <span data-ttu-id="f5de8-367">[Java-MapReduce programok fejlesztése a HDInsight][hdinsight-develop-java-mapreduce]</span><span class="sxs-lookup"><span data-stu-id="f5de8-367">[Develop Java MapReduce programs for HDInsight][hdinsight-develop-java-mapreduce]</span></span>

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
