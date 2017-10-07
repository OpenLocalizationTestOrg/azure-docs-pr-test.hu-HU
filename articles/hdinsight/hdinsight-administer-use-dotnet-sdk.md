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
# <a name="manage-hadoop-clusters-in-hdinsight-by-using-net-sdk"></a>Hdinsight Hadoop-fürtök kezelése .NET SDK használatával
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Ismerje meg, hogyan toomanage HDInsight clusters használatával [HDInsight.NET SDK](https://msdn.microsoft.com/library/mt271028.aspx).

**Előfeltételek**

Ez a cikk elkezdéséhez hello következő kell rendelkeznie:

* **Azure-előfizetés**. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

## <a name="connect-tooazure-hdinsight"></a>Csatlakozzon a HDInsight tooAzure

A következő Nuget-csomagok hello lesz szüksége:

    Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
    Install-Package Microsoft.Azure.Management.ResourceManager -Pre
    Install-Package Microsoft.Azure.Management.HDInsight

hello következő mintakód bemutatja, hogyan tooconnect tooAzure előtt felügyelheti a HDInsight-fürtök alatt az Azure-előfizetéshez.

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

Kell megjelenik egy üzenet, amikor futtatja a programot.  Ha nem szeretné toosee hello kérdés, lásd: [.NET HDInsight-alkalmazások létrehozása a nem interaktív hitelesítés](hdinsight-create-non-interactive-authentication-dotnet-applications.md).

## <a name="create-clusters"></a>Fürtök létrehozása
Lásd: [létrehozása Linux-alapú fürtökön a Hdinsightban az hello .NET SDK-val](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)

## <a name="list-clusters"></a>Lista fürtök
hello következő kódrészletet felsorolja fürtök és az egyes tulajdonságok:

    var results = _hdiManagementClient.Clusters.List();
    foreach (var name in results.Clusters) {
        Console.WriteLine("Cluster Name: " + name.Name);
        Console.WriteLine("\t Cluster type: " + name.Properties.ClusterDefinition.ClusterType);
        Console.WriteLine("\t Cluster location: " + name.Location);
        Console.WriteLine("\t Cluster version: " + name.Properties.ClusterVersion);
    }

## <a name="delete-clusters"></a>Fürtök törlése
A következő kód részlet toodelete fürt szinkron vagy aszinkron módon hello használata: 

    _hdiManagementClient.Clusters.Delete("<Resource Group Name>", "<Cluster Name>");
    _hdiManagementClient.Clusters.DeleteAsync("<Resource Group Name>", "<Cluster Name>");

## <a name="scale-clusters"></a>Fürtök méretezése
hello fürt skálázás funkció lehetővé teszi, hogy anélkül, hogy toore fut az Azure HDInsight fürt által használt feldolgozó csomópontok száma toochange hello-hello fürt létrehozása.

> [!NOTE]
> Csak verzió 3.1.3 hdinsight clusters vagy annál magasabb támogatottak. Ha biztos benne, hogy a fürt hello verziója, ellenőrizheti a hello tulajdonságlapján.  Lásd: [listája és megjelenítése fürtök](hdinsight-administer-use-portal-linux.md#list-and-show-clusters).
> 
> 

a fürt a HDInsight által támogatott különböző típusú adatok csomópontok hello számának módosítása hello következményei:

* Hadoop
  
    Zökkenőmentesen növelheti hello adhatja meg, hogy minden folyamatban lévő vagy a futó feladatok befolyásolása nélkül fut egy Hadoop-fürt feldolgozó csomópontjainak számát. Új feladatokat is küldheti el, amíg hello művelet van folyamatban. A méretezési művelet sikertelen szabályosan kezeli, így hello fürt mindig marad működőképes állapotban.
  
    A Hadoop fürtök adatok csomópontok száma hello csökkentésével átméretezi, ha néhány hello fürt hello szolgáltatás újraindul. Ennek hatására az összes futó és függőben lévő feladatok toofail művelet skálázás hello hello megvalósításának következő. Akkor is, azonban küldje el újra hello feladatok hello művelet végrehajtása után.
* HBase
  
    Zökkenőmentesen hozzáadása vagy eltávolítása a csomópontok tooyour HBase-fürtöt futtatása. A területi kiszolgálók hello skálázás művelet befejezése néhány percen belül automatikusan elosztását. Azonban Ön kézzel is eloszthatja hello területi kiszolgálók jelentkezzen be a fürt és a következő parancsok parancssori ablakból futó hello hello headnode:
  
        >pushd %HBASE_HOME%\bin
        >hbase shell
        >balancer
* Storm
  
    Zökkenőmentesen hozzáadása vagy eltávolítása adatok csomópontok tooyour Storm-fürt futása közben is. De hello skálázás művelet sikeres befejezése után kell toorebalance hello topológia.
  
    Kétféle módon valósítható meg újraelosztás:
  
  * A Storm webes felhasználói felület
  * Parancssori felület (CLI) eszköz
    
    Tekintse meg a toohello [alatt futó Apache Storm-dokumentáció](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) további részleteket.
    
    HDInsight-fürt hello hello Storm webes felhasználói felület érhető el:
    
    ![A HDInsight alatt futó Storm méretezési egyensúlyozza ki újra](./media/hdinsight-administer-use-management-portal/hdinsight-portal-scale-cluster-storm-rebalance.png)
    
    Például hogyan toouse hello CLI parancssori toorebalance hello Storm-topológia:
    
        ## Reconfigure hello topology "mytopology" toouse 5 worker processes,
        ## hello spout "blue-spout" toouse 3 executors, and
        ## hello bolt "yellow-bolt" toouse 10 executors
        $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10

a következő kódrészletben látható kód hogyan hello tooresize fürt szinkron vagy aszinkron módon:

    _hdiManagementClient.Clusters.Resize("<Resource Group Name>", "<Cluster Name>", <New Size>);   
    _hdiManagementClient.Clusters.ResizeAsync("<Resource Group Name>", "<Cluster Name>", <New Size>);   


## <a name="grantrevoke-access"></a>Hozzáférés biztosítása/visszavonása
A HDInsight-fürtök (ezen szolgáltatások mindegyikéhez rendelkezik RESTful végpontok) HTTP-webszolgáltatások a következő hello rendelkezik:

* ODBC
* JDBC
* Ambari
* Oozie
* Lépni a Templeton

Alapértelmezés szerint ezek a szolgáltatások hozzáférés vonatkozóan biztosított. Akkor is a visszavonási/grant hello hozzáférést. toorevoke:

    var httpParams = new HttpSettingsParameters
    {
        HttpUserEnabled = false,
        HttpUsername = "admin",
        HttpPassword = "*******",
    };
    _hdiManagementClient.Clusters.ConfigureHttpSettings("<Resource Group Name>, <Cluster Name>, httpParams);

toogrant:

    var httpParams = new HttpSettingsParameters
    {
        HttpUserEnabled = enable,
        HttpUsername = "admin",
        HttpPassword = "*******",
    };
    _hdiManagementClient.Clusters.ConfigureHttpSettings("<Resource Group Name>, <Cluster Name>, httpParams);


> [!NOTE]
> Hello hozzáférés biztosítása/visszavonása, visszaállítja, hello fürt felhasználónevet és jelszót.
> 
> 

Ez hello portálon keresztül is elvégezhető. Lásd: [felügyeletéhez HDInsight használatával hello Azure-portálon][hdinsight-admin-portal].

## <a name="update-http-user-credentials"></a>HTTP-felhasználó hitelesítő adatainak frissítése
Hello azonos művelet [Grant/revoke HTTP access](#grant/revoke-access). Ha hello fürt hello HTTP hozzáféréssel rendelkezik, akkor kell először vonja vissza.  És új HTTP felhasználói hitelesítő adatokkal majd hello hozzáférést.

## <a name="find-hello-default-storage-account"></a>Hello alapértelmezett tárfiók keresése
a következő kódrészletet hello bemutatja, hogyan tooget hello alapértelmezett tárfiók neve és hello alapértelmezett tárfiók kulcsa a fürthöz.

    var results = _hdiManagementClient.Clusters.GetClusterConfigurations(<Resource Group Name>, <Cluster Name>, "core-site");
    foreach (var key in results.Configuration.Keys)
    {
        Console.WriteLine(String.Format("{0} => {1}", key, results.Configuration[key]));
    }


## <a name="submit-jobs"></a>Feladatok elküldéséhez
**toosubmit MapReduce-feladatok**

Lásd: [hdinsight Hadoop-MapReduce futtatása minták](hdinsight-hadoop-run-samples-linux.md).

**toosubmit Hive-feladatok** 

Lásd: [.NET SDK használatával futtassa Hive lekérdezések](hdinsight-hadoop-use-hive-dotnet-sdk.md).

**toosubmit Pig-feladatokhoz**

Lásd: [.NET SDK használatával futtassa Pig-feladatokhoz](hdinsight-hadoop-use-pig-dotnet-sdk.md).

**toosubmit Sqoop feladatok**

Lásd: [Use Sqoop with HDInsight](hdinsight-hadoop-use-sqoop-dotnet-sdk.md).

**toosubmit Oozie feladatok**

Lásd: [Hadoop toodefine és futtatása egy munkafolyamat hdinsight használata Oozie](hdinsight-use-oozie-linux-mac.md).

## <a name="upload-data-tooazure-blob-storage"></a>Töltse fel az adatok tooAzure Blob-tároló
Lásd: [töltse fel az adatok tooHDInsight][hdinsight-upload-data].

## <a name="see-also"></a>Lásd még:
* [A HDInsight .NET SDK referenciadokumentációt](https://msdn.microsoft.com/library/mt271028.aspx)
* [HDInsight felügyelete hello Azure-portál használatával][hdinsight-admin-portal]
* [HDInsight a parancssori felület felügyelete][hdinsight-admin-cli]
* [A HDInsight-fürtök létrehozása][hdinsight-provision]
* [Adatok tooHDInsight feltöltése][hdinsight-upload-data]
* [Azure HDInsight – első lépések][hdinsight-get-started]

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


