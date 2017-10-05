---
title: "A .NET SDK - Azure hdinsight Hadoop-fürtök kezelése |} Microsoft Docs"
description: "Útmutató a HDInsight .NET SDK használatával hdinsight Hadoop-fürtök felügyeleti feladatokat hajthat végre."
services: hdinsight
editor: cgronlun
manager: jhubbard
tags: azure-portal
author: mumian
documentationcenter: 
ms.assetid: fd134765-c2a0-488a-bca6-184d814d78e9
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: c10471425fa1202ddb7fe35d0adf4ef33509f268
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="manage-hadoop-clusters-in-hdinsight-by-using-net-sdk"></a><span data-ttu-id="8073b-103">Hdinsight Hadoop-fürtök kezelése .NET SDK használatával</span><span class="sxs-lookup"><span data-stu-id="8073b-103">Manage Hadoop clusters in HDInsight by using .NET SDK</span></span>
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

<span data-ttu-id="8073b-104">HDInsight-fürtök kezelése [HDInsight.NET SDK](https://msdn.microsoft.com/library/mt271028.aspx).</span><span class="sxs-lookup"><span data-stu-id="8073b-104">Learn how to manage HDInsight clusters using [HDInsight.NET SDK](https://msdn.microsoft.com/library/mt271028.aspx).</span></span>

<span data-ttu-id="8073b-105">**Előfeltételek**</span><span class="sxs-lookup"><span data-stu-id="8073b-105">**Prerequisites**</span></span>

<span data-ttu-id="8073b-106">A cikk elkezdéséhez az alábbiakkal kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="8073b-106">Before you begin this article, you must have the following:</span></span>

* <span data-ttu-id="8073b-107">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="8073b-107">**An Azure subscription**.</span></span> <span data-ttu-id="8073b-108">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="8073b-108">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>

## <a name="connect-to-azure-hdinsight"></a><span data-ttu-id="8073b-109">Csatlakozás az Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="8073b-109">Connect to Azure HDInsight</span></span>

<span data-ttu-id="8073b-110">A következő Nuget-csomagok lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="8073b-110">You need the following Nuget packages:</span></span>

    Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
    Install-Package Microsoft.Azure.Management.ResourceManager -Pre
    Install-Package Microsoft.Azure.Management.HDInsight

<span data-ttu-id="8073b-111">A következő példakód bemutatja, hogyan csatlakozik az Azure HDInsight-fürtök az Azure-előfizetéshez tartozó felügyeletének előfeltétele.</span><span class="sxs-lookup"><span data-stu-id="8073b-111">The following code sample shows you how to connect to Azure before you can administer HDInsight clusters under your Azure subscription.</span></span>

    using System;
    using Microsoft.Azure;
    using Microsoft.Azure.Management.HDInsight;
    using Microsoft.Azure.Management.HDInsight.Models;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    using Microsoft.Rest.Azure.Authentication;

    namespace HDInsightManagement
    {
        class Program
        {
            private static HDInsightManagementClient _hdiManagementClient;
            // Replace with your AAD tenant ID if necessary
            private const string TenantId = UserTokenProvider.CommonTenantId; 
            private const string SubscriptionId = "<Your Azure Subscription ID>";
            // This is the GUID for the PowerShell client. Used for interactive logins in this example.
            private const string ClientId = "1950a258-227b-4e31-a9cf-717495945fc2";

            static void Main(string[] args)
            {
                // Authenticate and get a token
                var authToken = Authenticate(TenantId, ClientId, SubscriptionId);
                // Flag subscription for HDInsight, if it isn't already.
                EnableHDInsight(authToken);
                // Get an HDInsight management client
                _hdiManagementClient = new HDInsightManagementClient(authToken);

                // insert code here

                System.Console.WriteLine("Press ENTER to continue");
                System.Console.ReadLine();
            }

            /// <summary>
            /// Authenticate to an Azure subscription and retrieve an authentication token
            /// </summary>
            static TokenCloudCredentials Authenticate(string TenantId, string ClientId, string SubscriptionId)
            {
                var authContext = new AuthenticationContext("https://login.microsoftonline.com/" + TenantId);
                var tokenAuthResult = authContext.AcquireToken("https://management.core.windows.net/", 
                    ClientId, 
                    new Uri("urn:ietf:wg:oauth:2.0:oob"), 
                    PromptBehavior.Always, 
                    UserIdentifier.AnyUser);
                return new TokenCloudCredentials(SubscriptionId, tokenAuthResult.AccessToken);
            }
            /// <summary>
            /// Marks your subscription as one that can use HDInsight, if it has not already been marked as such.
            /// </summary>
            /// <remarks>This is essentially a one-time action; if you have already done something with HDInsight
            /// on your subscription, then this isn't needed at all and will do nothing.</remarks>
            /// <param name="authToken">An authentication token for your Azure subscription</param>
            static void EnableHDInsight(TokenCloudCredentials authToken)
            {
                // Create a client for the Resource manager and set the subscription ID
                var resourceManagementClient = new ResourceManagementClient(new TokenCredentials(authToken.Token));
                resourceManagementClient.SubscriptionId = SubscriptionId;
                // Register the HDInsight provider
                var rpResult = resourceManagementClient.Providers.Register("Microsoft.HDInsight");
            }
        }
    }

<span data-ttu-id="8073b-112">Kell megjelenik egy üzenet, amikor futtatja a programot.</span><span class="sxs-lookup"><span data-stu-id="8073b-112">You shall see a prompt when you run this program.</span></span>  <span data-ttu-id="8073b-113">Ha nem szeretné kéri, lásd: [.NET HDInsight-alkalmazások létrehozása a nem interaktív hitelesítés](hdinsight-create-non-interactive-authentication-dotnet-applications.md).</span><span class="sxs-lookup"><span data-stu-id="8073b-113">If you don't want to see the prompt, see [Create non-interactive authentication .NET HDInsight applications](hdinsight-create-non-interactive-authentication-dotnet-applications.md).</span></span>

## <a name="create-clusters"></a><span data-ttu-id="8073b-114">Fürtök létrehozása</span><span class="sxs-lookup"><span data-stu-id="8073b-114">Create clusters</span></span>
<span data-ttu-id="8073b-115">Lásd: [fürtök létrehozása Linux-alapú hdinsight .NET SDK használatával](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)</span><span class="sxs-lookup"><span data-stu-id="8073b-115">See [Create Linux-based clusters in HDInsight using the .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)</span></span>

## <a name="list-clusters"></a><span data-ttu-id="8073b-116">Lista fürtök</span><span class="sxs-lookup"><span data-stu-id="8073b-116">List clusters</span></span>
<span data-ttu-id="8073b-117">A következő kódrészletet a fürtök és az egyes tulajdonságok tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="8073b-117">The following code snippet lists clusters and some properties:</span></span>

    var results = _hdiManagementClient.Clusters.List();
    foreach (var name in results.Clusters) {
        Console.WriteLine("Cluster Name: " + name.Name);
        Console.WriteLine("\t Cluster type: " + name.Properties.ClusterDefinition.ClusterType);
        Console.WriteLine("\t Cluster location: " + name.Location);
        Console.WriteLine("\t Cluster version: " + name.Properties.ClusterVersion);
    }

## <a name="delete-clusters"></a><span data-ttu-id="8073b-118">Fürtök törlése</span><span class="sxs-lookup"><span data-stu-id="8073b-118">Delete clusters</span></span>
<span data-ttu-id="8073b-119">A fürtök törlése szinkron vagy aszinkron módon használja a következő kódrészletet:</span><span class="sxs-lookup"><span data-stu-id="8073b-119">Use the following code snippet to delete a cluster synchronously or asynchronously:</span></span> 

    _hdiManagementClient.Clusters.Delete("<Resource Group Name>", "<Cluster Name>");
    _hdiManagementClient.Clusters.DeleteAsync("<Resource Group Name>", "<Cluster Name>");

## <a name="scale-clusters"></a><span data-ttu-id="8073b-120">Fürtök méretezése</span><span class="sxs-lookup"><span data-stu-id="8073b-120">Scale clusters</span></span>
<span data-ttu-id="8073b-121">A fürt skálázás funkciót lehetővé teszi, hogy anélkül, hogy újra létre kell hoznia a fürt fut az Azure HDInsight fürt által használt feldolgozó csomópontok számának módosítása.</span><span class="sxs-lookup"><span data-stu-id="8073b-121">The cluster scaling feature allows you to change the number of worker nodes used by a cluster that is running in Azure HDInsight without having to re-create the cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="8073b-122">Csak verzió 3.1.3 hdinsight clusters vagy annál magasabb támogatottak.</span><span class="sxs-lookup"><span data-stu-id="8073b-122">Only clusters with HDInsight version 3.1.3 or higher are supported.</span></span> <span data-ttu-id="8073b-123">Ha biztos benne, hogy a fürt verzióját, a Tulajdonságok lapján ellenőrizheti.</span><span class="sxs-lookup"><span data-stu-id="8073b-123">If you are unsure of the version of your cluster, you can check the Properties page.</span></span>  <span data-ttu-id="8073b-124">Lásd: [listája és megjelenítése fürtök](hdinsight-administer-use-portal-linux.md#list-and-show-clusters).</span><span class="sxs-lookup"><span data-stu-id="8073b-124">See [List and show clusters](hdinsight-administer-use-portal-linux.md#list-and-show-clusters).</span></span>
> 
> 

<span data-ttu-id="8073b-125">A fürt a HDInsight által támogatott különböző típusú adatok csomópontok számának módosítása következményei:</span><span class="sxs-lookup"><span data-stu-id="8073b-125">The impact of changing the number of data nodes for each type of cluster supported by HDInsight:</span></span>

* <span data-ttu-id="8073b-126">Hadoop</span><span class="sxs-lookup"><span data-stu-id="8073b-126">Hadoop</span></span>
  
    <span data-ttu-id="8073b-127">Zökkenőmentesen növelheti adhatja meg, hogy minden folyamatban lévő vagy a futó feladatok befolyásolása nélkül fut egy Hadoop-fürt feldolgozó csomópontjainak számát.</span><span class="sxs-lookup"><span data-stu-id="8073b-127">You can seamlessly increase the number of worker nodes in a Hadoop cluster that is running without impacting any pending or running jobs.</span></span> <span data-ttu-id="8073b-128">Új feladatokat is küldheti el, amíg a művelet folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="8073b-128">New jobs can also be submitted while the operation is in progress.</span></span> <span data-ttu-id="8073b-129">A méretezési művelet sikertelen szabályosan kezeli, hogy a fürt mindig működőképes állapotban marad.</span><span class="sxs-lookup"><span data-stu-id="8073b-129">Failures in a scaling operation are gracefully handled so that the cluster is always left in a functional state.</span></span>
  
    <span data-ttu-id="8073b-130">A Hadoop fürtök adatok csomópontok számának csökkentésével átméretezi, ha néhány, a fürt a szolgáltatások újraindításáig.</span><span class="sxs-lookup"><span data-stu-id="8073b-130">When a Hadoop cluster is scaled down by reducing the number of data nodes, some of the services in the cluster are restarted.</span></span> <span data-ttu-id="8073b-131">Ennek hatására a összes futó és függőben lévő feladatok meghiúsulhatnak, a méretezési művelet befejezését.</span><span class="sxs-lookup"><span data-stu-id="8073b-131">This causes all running and pending jobs to fail at the completion of the scaling operation.</span></span> <span data-ttu-id="8073b-132">Akkor is, azonban küldje el újra a feladatok a művelet végrehajtása után.</span><span class="sxs-lookup"><span data-stu-id="8073b-132">You can, however, resubmit the jobs once the operation is complete.</span></span>
* <span data-ttu-id="8073b-133">HBase</span><span class="sxs-lookup"><span data-stu-id="8073b-133">HBase</span></span>
  
    <span data-ttu-id="8073b-134">Akkor is zökkenőmentesen csomópontok hozzáadásához és eltávolításához a HBase-fürtöt a futtatása.</span><span class="sxs-lookup"><span data-stu-id="8073b-134">You can seamlessly add or remove nodes to your HBase cluster while it is running.</span></span> <span data-ttu-id="8073b-135">Területi kiszolgálók automatikus elosztását a méretezési művelet befejezését néhány percen belül.</span><span class="sxs-lookup"><span data-stu-id="8073b-135">Regional Servers are automatically balanced within a few minutes of completing the scaling operation.</span></span> <span data-ttu-id="8073b-136">Azonban Ön kézzel is eloszthatja a regionális kiszolgálók fürt headnode való bejelentkezés, és futtatja a következő parancsokat egy parancssori ablakot:</span><span class="sxs-lookup"><span data-stu-id="8073b-136">However, you can also manually balance the regional servers by logging into the headnode of cluster and running the following commands from a command prompt window:</span></span>
  
        >pushd %HBASE_HOME%\bin
        >hbase shell
        >balancer
* <span data-ttu-id="8073b-137">Storm</span><span class="sxs-lookup"><span data-stu-id="8073b-137">Storm</span></span>
  
    <span data-ttu-id="8073b-138">Akkor is zökkenőmentesen csomópontok hozzáadásához és eltávolításához adatok Storm fürthöz való futtatása során.</span><span class="sxs-lookup"><span data-stu-id="8073b-138">You can seamlessly add or remove data nodes to your Storm cluster while it is running.</span></span> <span data-ttu-id="8073b-139">De a méretezési művelet sikeres befejezését követően szüksége lesz a topológia egyensúlyba.</span><span class="sxs-lookup"><span data-stu-id="8073b-139">But after a successful completion of the scaling operation, you will need to rebalance the topology.</span></span>
  
    <span data-ttu-id="8073b-140">Kétféle módon valósítható meg újraelosztás:</span><span class="sxs-lookup"><span data-stu-id="8073b-140">Rebalancing can be accomplished in two ways:</span></span>
  
  * <span data-ttu-id="8073b-141">A Storm webes felhasználói felület</span><span class="sxs-lookup"><span data-stu-id="8073b-141">Storm web UI</span></span>
  * <span data-ttu-id="8073b-142">Parancssori felület (CLI) eszköz</span><span class="sxs-lookup"><span data-stu-id="8073b-142">Command-line interface (CLI) tool</span></span>
    
    <span data-ttu-id="8073b-143">Tekintse meg a [alatt futó Apache Storm-dokumentáció](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) további részleteket.</span><span class="sxs-lookup"><span data-stu-id="8073b-143">Please refer to the [Apache Storm documentation](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) for more details.</span></span>
    
    <span data-ttu-id="8073b-144">A Storm webes felhasználói felület érhető el a HDInsight-fürt:</span><span class="sxs-lookup"><span data-stu-id="8073b-144">The Storm web UI is available on the HDInsight cluster:</span></span>
    
    ![A HDInsight alatt futó Storm méretezési egyensúlyozza ki újra](./media/hdinsight-administer-use-management-portal/hdinsight-portal-scale-cluster-storm-rebalance.png)
    
    <span data-ttu-id="8073b-146">Íme egy példa a CLI parancs használata a Storm-topológia egyensúlyba:</span><span class="sxs-lookup"><span data-stu-id="8073b-146">Here is an example how to use the CLI command to rebalance the Storm topology:</span></span>
    
        ## Reconfigure the topology "mytopology" to use 5 worker processes,
        ## the spout "blue-spout" to use 3 executors, and
        ## the bolt "yellow-bolt" to use 10 executors
        $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10

<span data-ttu-id="8073b-147">A következő kódrészletet bemutatja, hogyan méretezze át a fürt szinkron vagy aszinkron módon:</span><span class="sxs-lookup"><span data-stu-id="8073b-147">The following code snippet shows how to resize a cluster synchronously or asynchronously:</span></span>

    _hdiManagementClient.Clusters.Resize("<Resource Group Name>", "<Cluster Name>", <New Size>);   
    _hdiManagementClient.Clusters.ResizeAsync("<Resource Group Name>", "<Cluster Name>", <New Size>);   


## <a name="grantrevoke-access"></a><span data-ttu-id="8073b-148">Hozzáférés biztosítása/visszavonása</span><span class="sxs-lookup"><span data-stu-id="8073b-148">Grant/revoke access</span></span>
<span data-ttu-id="8073b-149">A HDInsight-fürtök a következő HTTP webszolgáltatásokat (ezen szolgáltatások mindegyikéhez rendelkezik RESTful végpontok) rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="8073b-149">HDInsight clusters have the following HTTP web services (all of these services have RESTful endpoints):</span></span>

* <span data-ttu-id="8073b-150">ODBC</span><span class="sxs-lookup"><span data-stu-id="8073b-150">ODBC</span></span>
* <span data-ttu-id="8073b-151">JDBC</span><span class="sxs-lookup"><span data-stu-id="8073b-151">JDBC</span></span>
* <span data-ttu-id="8073b-152">Ambari</span><span class="sxs-lookup"><span data-stu-id="8073b-152">Ambari</span></span>
* <span data-ttu-id="8073b-153">Oozie</span><span class="sxs-lookup"><span data-stu-id="8073b-153">Oozie</span></span>
* <span data-ttu-id="8073b-154">Lépni a Templeton</span><span class="sxs-lookup"><span data-stu-id="8073b-154">Templeton</span></span>

<span data-ttu-id="8073b-155">Alapértelmezés szerint ezek a szolgáltatások hozzáférés vonatkozóan biztosított.</span><span class="sxs-lookup"><span data-stu-id="8073b-155">By default, these services are granted for access.</span></span> <span data-ttu-id="8073b-156">Meg is visszavonási/engedélyezze a hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="8073b-156">You can revoke/grant the access.</span></span> <span data-ttu-id="8073b-157">Visszavonni:</span><span class="sxs-lookup"><span data-stu-id="8073b-157">To revoke:</span></span>

    var httpParams = new HttpSettingsParameters
    {
        HttpUserEnabled = false,
        HttpUsername = "admin",
        HttpPassword = "*******",
    };
    _hdiManagementClient.Clusters.ConfigureHttpSettings("<Resource Group Name>, <Cluster Name>, httpParams);

<span data-ttu-id="8073b-158">Megadását:</span><span class="sxs-lookup"><span data-stu-id="8073b-158">To grant:</span></span>

    var httpParams = new HttpSettingsParameters
    {
        HttpUserEnabled = enable,
        HttpUsername = "admin",
        HttpPassword = "*******",
    };
    _hdiManagementClient.Clusters.ConfigureHttpSettings("<Resource Group Name>, <Cluster Name>, httpParams);


> [!NOTE]
> <span data-ttu-id="8073b-159">A hozzáférés biztosítása/visszavonása, visszaállítja, a fürt felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="8073b-159">By granting/revoking the access, you will reset the cluster user name and password.</span></span>
> 
> 

<span data-ttu-id="8073b-160">Ez a portálon keresztül is elvégezhető.</span><span class="sxs-lookup"><span data-stu-id="8073b-160">This can also be done via the Portal.</span></span> <span data-ttu-id="8073b-161">Lásd: [felügyelheti a HDInsight az Azure portál használatával][hdinsight-admin-portal].</span><span class="sxs-lookup"><span data-stu-id="8073b-161">See [Administer HDInsight by using the Azure portal][hdinsight-admin-portal].</span></span>

## <a name="update-http-user-credentials"></a><span data-ttu-id="8073b-162">HTTP-felhasználó hitelesítő adatainak frissítése</span><span class="sxs-lookup"><span data-stu-id="8073b-162">Update HTTP user credentials</span></span>
<span data-ttu-id="8073b-163">Ugyanezt az eljárást, mint a [Grant/revoke HTTP access](#grant/revoke-access). Ha a fürt megadták a HTTP-hozzáférést, meg kell először vonja vissza.</span><span class="sxs-lookup"><span data-stu-id="8073b-163">It is the same procedure as [Grant/revoke HTTP access](#grant/revoke-access).If the cluster has been granted the HTTP access, you must first revoke it.</span></span>  <span data-ttu-id="8073b-164">És adja meg a hozzáférés új HTTP felhasználói hitelesítő adatokkal.</span><span class="sxs-lookup"><span data-stu-id="8073b-164">And then grant the access with new HTTP user credentials.</span></span>

## <a name="find-the-default-storage-account"></a><span data-ttu-id="8073b-165">Az alapértelmezett tárfiók keresése</span><span class="sxs-lookup"><span data-stu-id="8073b-165">Find the default storage account</span></span>
<span data-ttu-id="8073b-166">A következő kódrészletet az alapértelmezett tárfiók neve és a alapértelmezett tárfiók hívóbetűjét fürt mutatja be.</span><span class="sxs-lookup"><span data-stu-id="8073b-166">The following code snippet demonstrates how to get the default storage account name and the default storage account key for a cluster.</span></span>

    var results = _hdiManagementClient.Clusters.GetClusterConfigurations(<Resource Group Name>, <Cluster Name>, "core-site");
    foreach (var key in results.Configuration.Keys)
    {
        Console.WriteLine(String.Format("{0} => {1}", key, results.Configuration[key]));
    }


## <a name="submit-jobs"></a><span data-ttu-id="8073b-167">Feladatok elküldéséhez</span><span class="sxs-lookup"><span data-stu-id="8073b-167">Submit jobs</span></span>
<span data-ttu-id="8073b-168">**Elküldeni a MapReduce-feladatok**</span><span class="sxs-lookup"><span data-stu-id="8073b-168">**To submit MapReduce jobs**</span></span>

<span data-ttu-id="8073b-169">Lásd: [hdinsight Hadoop-MapReduce futtatása minták](hdinsight-hadoop-run-samples-linux.md).</span><span class="sxs-lookup"><span data-stu-id="8073b-169">See [Run Hadoop MapReduce samples in HDInsight](hdinsight-hadoop-run-samples-linux.md).</span></span>

<span data-ttu-id="8073b-170">**Elküldeni a Hive-feladatok**</span><span class="sxs-lookup"><span data-stu-id="8073b-170">**To submit Hive jobs**</span></span> 

<span data-ttu-id="8073b-171">Lásd: [.NET SDK használatával futtassa Hive lekérdezések](hdinsight-hadoop-use-hive-dotnet-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="8073b-171">See [Run Hive queries using .NET SDK](hdinsight-hadoop-use-hive-dotnet-sdk.md).</span></span>

<span data-ttu-id="8073b-172">**Elküldeni a Pig-feladatokhoz**</span><span class="sxs-lookup"><span data-stu-id="8073b-172">**To submit Pig jobs**</span></span>

<span data-ttu-id="8073b-173">Lásd: [.NET SDK használatával futtassa Pig-feladatokhoz](hdinsight-hadoop-use-pig-dotnet-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="8073b-173">See [Run Pig jobs using .NET SDK](hdinsight-hadoop-use-pig-dotnet-sdk.md).</span></span>

<span data-ttu-id="8073b-174">**Sqoop feladatok küldéséhez**</span><span class="sxs-lookup"><span data-stu-id="8073b-174">**To submit Sqoop jobs**</span></span>

<span data-ttu-id="8073b-175">Lásd: [Use Sqoop with HDInsight](hdinsight-hadoop-use-sqoop-dotnet-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="8073b-175">See [Use Sqoop with HDInsight](hdinsight-hadoop-use-sqoop-dotnet-sdk.md).</span></span>

<span data-ttu-id="8073b-176">**Oozie feladatok küldéséhez**</span><span class="sxs-lookup"><span data-stu-id="8073b-176">**To submit Oozie jobs**</span></span>

<span data-ttu-id="8073b-177">Lásd: [hadooppal megadásához és a munkafolyamat futtatása hdinsight használata Oozie](hdinsight-use-oozie-linux-mac.md).</span><span class="sxs-lookup"><span data-stu-id="8073b-177">See [Use Oozie with Hadoop to define and run a workflow in HDInsight](hdinsight-use-oozie-linux-mac.md).</span></span>

## <a name="upload-data-to-azure-blob-storage"></a><span data-ttu-id="8073b-178">Adatok feltöltése az Azure Blob storage</span><span class="sxs-lookup"><span data-stu-id="8073b-178">Upload data to Azure Blob storage</span></span>
<span data-ttu-id="8073b-179">Lásd: [Adatok feltöltése a HDInsightba][hdinsight-upload-data].</span><span class="sxs-lookup"><span data-stu-id="8073b-179">See [Upload data to HDInsight][hdinsight-upload-data].</span></span>

## <a name="see-also"></a><span data-ttu-id="8073b-180">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="8073b-180">See Also</span></span>
* [<span data-ttu-id="8073b-181">A HDInsight .NET SDK referenciadokumentációt</span><span class="sxs-lookup"><span data-stu-id="8073b-181">HDInsight .NET SDK reference documentation</span></span>](https://msdn.microsoft.com/library/mt271028.aspx)
* <span data-ttu-id="8073b-182">[HDInsight felügyelete az Azure-portál használatával][hdinsight-admin-portal]</span><span class="sxs-lookup"><span data-stu-id="8073b-182">[Administer HDInsight by using the Azure portal][hdinsight-admin-portal]</span></span>
* <span data-ttu-id="8073b-183">[HDInsight a parancssori felület felügyelete][hdinsight-admin-cli]</span><span class="sxs-lookup"><span data-stu-id="8073b-183">[Administer HDInsight using a command-line interface][hdinsight-admin-cli]</span></span>
* <span data-ttu-id="8073b-184">[A HDInsight-fürtök létrehozása][hdinsight-provision]</span><span class="sxs-lookup"><span data-stu-id="8073b-184">[Create HDInsight clusters][hdinsight-provision]</span></span>
* <span data-ttu-id="8073b-185">[Adatok feltöltése a HDInsightba][hdinsight-upload-data]</span><span class="sxs-lookup"><span data-stu-id="8073b-185">[Upload data to HDInsight][hdinsight-upload-data]</span></span>
* <span data-ttu-id="8073b-186">[Azure HDInsight – első lépések][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="8073b-186">[Get started with Azure HDInsight][hdinsight-get-started]</span></span>

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-provision-custom-options]: hdinsight-hadoop-provision-linux-clusters.md#configuration
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[hdinsight-admin-cli]: hdinsight-administer-use-command-line.md
[hdinsight-admin-portal]: hdinsight-administer-use-portal-linux.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-flight]: hdinsight-analyze-flight-delay-data.md


