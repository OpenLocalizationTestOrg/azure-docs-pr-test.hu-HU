---
title: a .NET SDK - Azure hdinsight clusters aaaManage Hadoop |} Microsoft Docs
description: "Ismerje meg, hogyan tooperform felügyeleti feladatokat hello HDInsight .NET SDK használatával hdinsight Hadoop-fürtök."
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
ms.openlocfilehash: d8bbf966b7eba3e943dfb2f764d15d8e52b9be71
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-hadoop-clusters-in-hdinsight-by-using-net-sdk"></a><span data-ttu-id="6386b-103">Hdinsight Hadoop-fürtök kezelése .NET SDK használatával</span><span class="sxs-lookup"><span data-stu-id="6386b-103">Manage Hadoop clusters in HDInsight by using .NET SDK</span></span>
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

<span data-ttu-id="6386b-104">Ismerje meg, hogyan toomanage HDInsight clusters használatával [HDInsight.NET SDK](https://msdn.microsoft.com/library/mt271028.aspx).</span><span class="sxs-lookup"><span data-stu-id="6386b-104">Learn how toomanage HDInsight clusters using [HDInsight.NET SDK](https://msdn.microsoft.com/library/mt271028.aspx).</span></span>

<span data-ttu-id="6386b-105">**Előfeltételek**</span><span class="sxs-lookup"><span data-stu-id="6386b-105">**Prerequisites**</span></span>

<span data-ttu-id="6386b-106">Ez a cikk elkezdéséhez hello következő kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="6386b-106">Before you begin this article, you must have hello following:</span></span>

* <span data-ttu-id="6386b-107">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="6386b-107">**An Azure subscription**.</span></span> <span data-ttu-id="6386b-108">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="6386b-108">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>

## <a name="connect-tooazure-hdinsight"></a><span data-ttu-id="6386b-109">Csatlakozzon a HDInsight tooAzure</span><span class="sxs-lookup"><span data-stu-id="6386b-109">Connect tooAzure HDInsight</span></span>

<span data-ttu-id="6386b-110">A következő Nuget-csomagok hello lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="6386b-110">You need hello following Nuget packages:</span></span>

    Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
    Install-Package Microsoft.Azure.Management.ResourceManager -Pre
    Install-Package Microsoft.Azure.Management.HDInsight

<span data-ttu-id="6386b-111">hello következő mintakód bemutatja, hogyan tooconnect tooAzure előtt felügyelheti a HDInsight-fürtök alatt az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="6386b-111">hello following code sample shows you how tooconnect tooAzure before you can administer HDInsight clusters under your Azure subscription.</span></span>

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
            // This is hello GUID for hello PowerShell client. Used for interactive logins in this example.
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

                System.Console.WriteLine("Press ENTER toocontinue");
                System.Console.ReadLine();
            }

            /// <summary>
            /// Authenticate tooan Azure subscription and retrieve an authentication token
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
                // Create a client for hello Resource manager and set hello subscription ID
                var resourceManagementClient = new ResourceManagementClient(new TokenCredentials(authToken.Token));
                resourceManagementClient.SubscriptionId = SubscriptionId;
                // Register hello HDInsight provider
                var rpResult = resourceManagementClient.Providers.Register("Microsoft.HDInsight");
            }
        }
    }

<span data-ttu-id="6386b-112">Kell megjelenik egy üzenet, amikor futtatja a programot.</span><span class="sxs-lookup"><span data-stu-id="6386b-112">You shall see a prompt when you run this program.</span></span>  <span data-ttu-id="6386b-113">Ha nem szeretné toosee hello kérdés, lásd: [.NET HDInsight-alkalmazások létrehozása a nem interaktív hitelesítés](hdinsight-create-non-interactive-authentication-dotnet-applications.md).</span><span class="sxs-lookup"><span data-stu-id="6386b-113">If you don't want toosee hello prompt, see [Create non-interactive authentication .NET HDInsight applications](hdinsight-create-non-interactive-authentication-dotnet-applications.md).</span></span>

## <a name="create-clusters"></a><span data-ttu-id="6386b-114">Fürtök létrehozása</span><span class="sxs-lookup"><span data-stu-id="6386b-114">Create clusters</span></span>
<span data-ttu-id="6386b-115">Lásd: [létrehozása Linux-alapú fürtökön a Hdinsightban az hello .NET SDK-val](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)</span><span class="sxs-lookup"><span data-stu-id="6386b-115">See [Create Linux-based clusters in HDInsight using hello .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)</span></span>

## <a name="list-clusters"></a><span data-ttu-id="6386b-116">Lista fürtök</span><span class="sxs-lookup"><span data-stu-id="6386b-116">List clusters</span></span>
<span data-ttu-id="6386b-117">hello következő kódrészletet felsorolja fürtök és az egyes tulajdonságok:</span><span class="sxs-lookup"><span data-stu-id="6386b-117">hello following code snippet lists clusters and some properties:</span></span>

    var results = _hdiManagementClient.Clusters.List();
    foreach (var name in results.Clusters) {
        Console.WriteLine("Cluster Name: " + name.Name);
        Console.WriteLine("\t Cluster type: " + name.Properties.ClusterDefinition.ClusterType);
        Console.WriteLine("\t Cluster location: " + name.Location);
        Console.WriteLine("\t Cluster version: " + name.Properties.ClusterVersion);
    }

## <a name="delete-clusters"></a><span data-ttu-id="6386b-118">Fürtök törlése</span><span class="sxs-lookup"><span data-stu-id="6386b-118">Delete clusters</span></span>
<span data-ttu-id="6386b-119">A következő kód részlet toodelete fürt szinkron vagy aszinkron módon hello használata:</span><span class="sxs-lookup"><span data-stu-id="6386b-119">Use hello following code snippet toodelete a cluster synchronously or asynchronously:</span></span> 

    _hdiManagementClient.Clusters.Delete("<Resource Group Name>", "<Cluster Name>");
    _hdiManagementClient.Clusters.DeleteAsync("<Resource Group Name>", "<Cluster Name>");

## <a name="scale-clusters"></a><span data-ttu-id="6386b-120">Fürtök méretezése</span><span class="sxs-lookup"><span data-stu-id="6386b-120">Scale clusters</span></span>
<span data-ttu-id="6386b-121">hello fürt skálázás funkció lehetővé teszi, hogy anélkül, hogy toore fut az Azure HDInsight fürt által használt feldolgozó csomópontok száma toochange hello-hello fürt létrehozása.</span><span class="sxs-lookup"><span data-stu-id="6386b-121">hello cluster scaling feature allows you toochange hello number of worker nodes used by a cluster that is running in Azure HDInsight without having toore-create hello cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="6386b-122">Csak verzió 3.1.3 hdinsight clusters vagy annál magasabb támogatottak.</span><span class="sxs-lookup"><span data-stu-id="6386b-122">Only clusters with HDInsight version 3.1.3 or higher are supported.</span></span> <span data-ttu-id="6386b-123">Ha biztos benne, hogy a fürt hello verziója, ellenőrizheti a hello tulajdonságlapján.</span><span class="sxs-lookup"><span data-stu-id="6386b-123">If you are unsure of hello version of your cluster, you can check hello Properties page.</span></span>  <span data-ttu-id="6386b-124">Lásd: [listája és megjelenítése fürtök](hdinsight-administer-use-portal-linux.md#list-and-show-clusters).</span><span class="sxs-lookup"><span data-stu-id="6386b-124">See [List and show clusters](hdinsight-administer-use-portal-linux.md#list-and-show-clusters).</span></span>
> 
> 

<span data-ttu-id="6386b-125">a fürt a HDInsight által támogatott különböző típusú adatok csomópontok hello számának módosítása hello következményei:</span><span class="sxs-lookup"><span data-stu-id="6386b-125">hello impact of changing hello number of data nodes for each type of cluster supported by HDInsight:</span></span>

* <span data-ttu-id="6386b-126">Hadoop</span><span class="sxs-lookup"><span data-stu-id="6386b-126">Hadoop</span></span>
  
    <span data-ttu-id="6386b-127">Zökkenőmentesen növelheti hello adhatja meg, hogy minden folyamatban lévő vagy a futó feladatok befolyásolása nélkül fut egy Hadoop-fürt feldolgozó csomópontjainak számát.</span><span class="sxs-lookup"><span data-stu-id="6386b-127">You can seamlessly increase hello number of worker nodes in a Hadoop cluster that is running without impacting any pending or running jobs.</span></span> <span data-ttu-id="6386b-128">Új feladatokat is küldheti el, amíg hello művelet van folyamatban.</span><span class="sxs-lookup"><span data-stu-id="6386b-128">New jobs can also be submitted while hello operation is in progress.</span></span> <span data-ttu-id="6386b-129">A méretezési művelet sikertelen szabályosan kezeli, így hello fürt mindig marad működőképes állapotban.</span><span class="sxs-lookup"><span data-stu-id="6386b-129">Failures in a scaling operation are gracefully handled so that hello cluster is always left in a functional state.</span></span>
  
    <span data-ttu-id="6386b-130">A Hadoop fürtök adatok csomópontok száma hello csökkentésével átméretezi, ha néhány hello fürt hello szolgáltatás újraindul.</span><span class="sxs-lookup"><span data-stu-id="6386b-130">When a Hadoop cluster is scaled down by reducing hello number of data nodes, some of hello services in hello cluster are restarted.</span></span> <span data-ttu-id="6386b-131">Ennek hatására az összes futó és függőben lévő feladatok toofail művelet skálázás hello hello megvalósításának következő.</span><span class="sxs-lookup"><span data-stu-id="6386b-131">This causes all running and pending jobs toofail at hello completion of hello scaling operation.</span></span> <span data-ttu-id="6386b-132">Akkor is, azonban küldje el újra hello feladatok hello művelet végrehajtása után.</span><span class="sxs-lookup"><span data-stu-id="6386b-132">You can, however, resubmit hello jobs once hello operation is complete.</span></span>
* <span data-ttu-id="6386b-133">HBase</span><span class="sxs-lookup"><span data-stu-id="6386b-133">HBase</span></span>
  
    <span data-ttu-id="6386b-134">Zökkenőmentesen hozzáadása vagy eltávolítása a csomópontok tooyour HBase-fürtöt futtatása.</span><span class="sxs-lookup"><span data-stu-id="6386b-134">You can seamlessly add or remove nodes tooyour HBase cluster while it is running.</span></span> <span data-ttu-id="6386b-135">A területi kiszolgálók hello skálázás művelet befejezése néhány percen belül automatikusan elosztását.</span><span class="sxs-lookup"><span data-stu-id="6386b-135">Regional Servers are automatically balanced within a few minutes of completing hello scaling operation.</span></span> <span data-ttu-id="6386b-136">Azonban Ön kézzel is eloszthatja hello területi kiszolgálók jelentkezzen be a fürt és a következő parancsok parancssori ablakból futó hello hello headnode:</span><span class="sxs-lookup"><span data-stu-id="6386b-136">However, you can also manually balance hello regional servers by logging into hello headnode of cluster and running hello following commands from a command prompt window:</span></span>
  
        >pushd %HBASE_HOME%\bin
        >hbase shell
        >balancer
* <span data-ttu-id="6386b-137">Storm</span><span class="sxs-lookup"><span data-stu-id="6386b-137">Storm</span></span>
  
    <span data-ttu-id="6386b-138">Zökkenőmentesen hozzáadása vagy eltávolítása adatok csomópontok tooyour Storm-fürt futása közben is.</span><span class="sxs-lookup"><span data-stu-id="6386b-138">You can seamlessly add or remove data nodes tooyour Storm cluster while it is running.</span></span> <span data-ttu-id="6386b-139">De hello skálázás művelet sikeres befejezése után kell toorebalance hello topológia.</span><span class="sxs-lookup"><span data-stu-id="6386b-139">But after a successful completion of hello scaling operation, you will need toorebalance hello topology.</span></span>
  
    <span data-ttu-id="6386b-140">Kétféle módon valósítható meg újraelosztás:</span><span class="sxs-lookup"><span data-stu-id="6386b-140">Rebalancing can be accomplished in two ways:</span></span>
  
  * <span data-ttu-id="6386b-141">A Storm webes felhasználói felület</span><span class="sxs-lookup"><span data-stu-id="6386b-141">Storm web UI</span></span>
  * <span data-ttu-id="6386b-142">Parancssori felület (CLI) eszköz</span><span class="sxs-lookup"><span data-stu-id="6386b-142">Command-line interface (CLI) tool</span></span>
    
    <span data-ttu-id="6386b-143">Tekintse meg a toohello [alatt futó Apache Storm-dokumentáció](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) további részleteket.</span><span class="sxs-lookup"><span data-stu-id="6386b-143">Please refer toohello [Apache Storm documentation](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) for more details.</span></span>
    
    <span data-ttu-id="6386b-144">HDInsight-fürt hello hello Storm webes felhasználói felület érhető el:</span><span class="sxs-lookup"><span data-stu-id="6386b-144">hello Storm web UI is available on hello HDInsight cluster:</span></span>
    
    ![A HDInsight alatt futó Storm méretezési egyensúlyozza ki újra](./media/hdinsight-administer-use-management-portal/hdinsight-portal-scale-cluster-storm-rebalance.png)
    
    <span data-ttu-id="6386b-146">Például hogyan toouse hello CLI parancssori toorebalance hello Storm-topológia:</span><span class="sxs-lookup"><span data-stu-id="6386b-146">Here is an example how toouse hello CLI command toorebalance hello Storm topology:</span></span>
    
        ## Reconfigure hello topology "mytopology" toouse 5 worker processes,
        ## hello spout "blue-spout" toouse 3 executors, and
        ## hello bolt "yellow-bolt" toouse 10 executors
        $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10

<span data-ttu-id="6386b-147">a következő kódrészletben látható kód hogyan hello tooresize fürt szinkron vagy aszinkron módon:</span><span class="sxs-lookup"><span data-stu-id="6386b-147">hello following code snippet shows how tooresize a cluster synchronously or asynchronously:</span></span>

    _hdiManagementClient.Clusters.Resize("<Resource Group Name>", "<Cluster Name>", <New Size>);   
    _hdiManagementClient.Clusters.ResizeAsync("<Resource Group Name>", "<Cluster Name>", <New Size>);   


## <a name="grantrevoke-access"></a><span data-ttu-id="6386b-148">Hozzáférés biztosítása/visszavonása</span><span class="sxs-lookup"><span data-stu-id="6386b-148">Grant/revoke access</span></span>
<span data-ttu-id="6386b-149">A HDInsight-fürtök (ezen szolgáltatások mindegyikéhez rendelkezik RESTful végpontok) HTTP-webszolgáltatások a következő hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="6386b-149">HDInsight clusters have hello following HTTP web services (all of these services have RESTful endpoints):</span></span>

* <span data-ttu-id="6386b-150">ODBC</span><span class="sxs-lookup"><span data-stu-id="6386b-150">ODBC</span></span>
* <span data-ttu-id="6386b-151">JDBC</span><span class="sxs-lookup"><span data-stu-id="6386b-151">JDBC</span></span>
* <span data-ttu-id="6386b-152">Ambari</span><span class="sxs-lookup"><span data-stu-id="6386b-152">Ambari</span></span>
* <span data-ttu-id="6386b-153">Oozie</span><span class="sxs-lookup"><span data-stu-id="6386b-153">Oozie</span></span>
* <span data-ttu-id="6386b-154">Lépni a Templeton</span><span class="sxs-lookup"><span data-stu-id="6386b-154">Templeton</span></span>

<span data-ttu-id="6386b-155">Alapértelmezés szerint ezek a szolgáltatások hozzáférés vonatkozóan biztosított.</span><span class="sxs-lookup"><span data-stu-id="6386b-155">By default, these services are granted for access.</span></span> <span data-ttu-id="6386b-156">Akkor is a visszavonási/grant hello hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="6386b-156">You can revoke/grant hello access.</span></span> <span data-ttu-id="6386b-157">toorevoke:</span><span class="sxs-lookup"><span data-stu-id="6386b-157">toorevoke:</span></span>

    var httpParams = new HttpSettingsParameters
    {
        HttpUserEnabled = false,
        HttpUsername = "admin",
        HttpPassword = "*******",
    };
    _hdiManagementClient.Clusters.ConfigureHttpSettings("<Resource Group Name>, <Cluster Name>, httpParams);

<span data-ttu-id="6386b-158">toogrant:</span><span class="sxs-lookup"><span data-stu-id="6386b-158">toogrant:</span></span>

    var httpParams = new HttpSettingsParameters
    {
        HttpUserEnabled = enable,
        HttpUsername = "admin",
        HttpPassword = "*******",
    };
    _hdiManagementClient.Clusters.ConfigureHttpSettings("<Resource Group Name>, <Cluster Name>, httpParams);


> [!NOTE]
> <span data-ttu-id="6386b-159">Hello hozzáférés biztosítása/visszavonása, visszaállítja, hello fürt felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="6386b-159">By granting/revoking hello access, you will reset hello cluster user name and password.</span></span>
> 
> 

<span data-ttu-id="6386b-160">Ez hello portálon keresztül is elvégezhető.</span><span class="sxs-lookup"><span data-stu-id="6386b-160">This can also be done via hello Portal.</span></span> <span data-ttu-id="6386b-161">Lásd: [felügyeletéhez HDInsight használatával hello Azure-portálon][hdinsight-admin-portal].</span><span class="sxs-lookup"><span data-stu-id="6386b-161">See [Administer HDInsight by using hello Azure portal][hdinsight-admin-portal].</span></span>

## <a name="update-http-user-credentials"></a><span data-ttu-id="6386b-162">HTTP-felhasználó hitelesítő adatainak frissítése</span><span class="sxs-lookup"><span data-stu-id="6386b-162">Update HTTP user credentials</span></span>
<span data-ttu-id="6386b-163">Hello azonos művelet [Grant/revoke HTTP access](#grant/revoke-access). Ha hello fürt hello HTTP hozzáféréssel rendelkezik, akkor kell először vonja vissza.</span><span class="sxs-lookup"><span data-stu-id="6386b-163">It is hello same procedure as [Grant/revoke HTTP access](#grant/revoke-access).If hello cluster has been granted hello HTTP access, you must first revoke it.</span></span>  <span data-ttu-id="6386b-164">És új HTTP felhasználói hitelesítő adatokkal majd hello hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="6386b-164">And then grant hello access with new HTTP user credentials.</span></span>

## <a name="find-hello-default-storage-account"></a><span data-ttu-id="6386b-165">Hello alapértelmezett tárfiók keresése</span><span class="sxs-lookup"><span data-stu-id="6386b-165">Find hello default storage account</span></span>
<span data-ttu-id="6386b-166">a következő kódrészletet hello bemutatja, hogyan tooget hello alapértelmezett tárfiók neve és hello alapértelmezett tárfiók kulcsa a fürthöz.</span><span class="sxs-lookup"><span data-stu-id="6386b-166">hello following code snippet demonstrates how tooget hello default storage account name and hello default storage account key for a cluster.</span></span>

    var results = _hdiManagementClient.Clusters.GetClusterConfigurations(<Resource Group Name>, <Cluster Name>, "core-site");
    foreach (var key in results.Configuration.Keys)
    {
        Console.WriteLine(String.Format("{0} => {1}", key, results.Configuration[key]));
    }


## <a name="submit-jobs"></a><span data-ttu-id="6386b-167">Feladatok elküldéséhez</span><span class="sxs-lookup"><span data-stu-id="6386b-167">Submit jobs</span></span>
<span data-ttu-id="6386b-168">**toosubmit MapReduce-feladatok**</span><span class="sxs-lookup"><span data-stu-id="6386b-168">**toosubmit MapReduce jobs**</span></span>

<span data-ttu-id="6386b-169">Lásd: [hdinsight Hadoop-MapReduce futtatása minták](hdinsight-hadoop-run-samples-linux.md).</span><span class="sxs-lookup"><span data-stu-id="6386b-169">See [Run Hadoop MapReduce samples in HDInsight](hdinsight-hadoop-run-samples-linux.md).</span></span>

<span data-ttu-id="6386b-170">**toosubmit Hive-feladatok**</span><span class="sxs-lookup"><span data-stu-id="6386b-170">**toosubmit Hive jobs**</span></span> 

<span data-ttu-id="6386b-171">Lásd: [.NET SDK használatával futtassa Hive lekérdezések](hdinsight-hadoop-use-hive-dotnet-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="6386b-171">See [Run Hive queries using .NET SDK](hdinsight-hadoop-use-hive-dotnet-sdk.md).</span></span>

<span data-ttu-id="6386b-172">**toosubmit Pig-feladatokhoz**</span><span class="sxs-lookup"><span data-stu-id="6386b-172">**toosubmit Pig jobs**</span></span>

<span data-ttu-id="6386b-173">Lásd: [.NET SDK használatával futtassa Pig-feladatokhoz](hdinsight-hadoop-use-pig-dotnet-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="6386b-173">See [Run Pig jobs using .NET SDK](hdinsight-hadoop-use-pig-dotnet-sdk.md).</span></span>

<span data-ttu-id="6386b-174">**toosubmit Sqoop feladatok**</span><span class="sxs-lookup"><span data-stu-id="6386b-174">**toosubmit Sqoop jobs**</span></span>

<span data-ttu-id="6386b-175">Lásd: [Use Sqoop with HDInsight](hdinsight-hadoop-use-sqoop-dotnet-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="6386b-175">See [Use Sqoop with HDInsight](hdinsight-hadoop-use-sqoop-dotnet-sdk.md).</span></span>

<span data-ttu-id="6386b-176">**toosubmit Oozie feladatok**</span><span class="sxs-lookup"><span data-stu-id="6386b-176">**toosubmit Oozie jobs**</span></span>

<span data-ttu-id="6386b-177">Lásd: [Hadoop toodefine és futtatása egy munkafolyamat hdinsight használata Oozie](hdinsight-use-oozie-linux-mac.md).</span><span class="sxs-lookup"><span data-stu-id="6386b-177">See [Use Oozie with Hadoop toodefine and run a workflow in HDInsight](hdinsight-use-oozie-linux-mac.md).</span></span>

## <a name="upload-data-tooazure-blob-storage"></a><span data-ttu-id="6386b-178">Töltse fel az adatok tooAzure Blob-tároló</span><span class="sxs-lookup"><span data-stu-id="6386b-178">Upload data tooAzure Blob storage</span></span>
<span data-ttu-id="6386b-179">Lásd: [töltse fel az adatok tooHDInsight][hdinsight-upload-data].</span><span class="sxs-lookup"><span data-stu-id="6386b-179">See [Upload data tooHDInsight][hdinsight-upload-data].</span></span>

## <a name="see-also"></a><span data-ttu-id="6386b-180">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="6386b-180">See Also</span></span>
* [<span data-ttu-id="6386b-181">A HDInsight .NET SDK referenciadokumentációt</span><span class="sxs-lookup"><span data-stu-id="6386b-181">HDInsight .NET SDK reference documentation</span></span>](https://msdn.microsoft.com/library/mt271028.aspx)
* <span data-ttu-id="6386b-182">[HDInsight felügyelete hello Azure-portál használatával][hdinsight-admin-portal]</span><span class="sxs-lookup"><span data-stu-id="6386b-182">[Administer HDInsight by using hello Azure portal][hdinsight-admin-portal]</span></span>
* <span data-ttu-id="6386b-183">[HDInsight a parancssori felület felügyelete][hdinsight-admin-cli]</span><span class="sxs-lookup"><span data-stu-id="6386b-183">[Administer HDInsight using a command-line interface][hdinsight-admin-cli]</span></span>
* <span data-ttu-id="6386b-184">[A HDInsight-fürtök létrehozása][hdinsight-provision]</span><span class="sxs-lookup"><span data-stu-id="6386b-184">[Create HDInsight clusters][hdinsight-provision]</span></span>
* <span data-ttu-id="6386b-185">[Adatok tooHDInsight feltöltése][hdinsight-upload-data]</span><span class="sxs-lookup"><span data-stu-id="6386b-185">[Upload data tooHDInsight][hdinsight-upload-data]</span></span>
* <span data-ttu-id="6386b-186">[Azure HDInsight – első lépések][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="6386b-186">[Get started with Azure HDInsight][hdinsight-get-started]</span></span>

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


