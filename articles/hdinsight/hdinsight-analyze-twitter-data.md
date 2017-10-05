---
title: "A HDInsight - Azure Hadoop Twitter adatok elemzése |} Microsoft Docs"
description: "Útmutató egy adott word használati gyakoriságának a HDInsight hadoop Twitter-adatok elemzése a Hive segítségével."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 78e4ea33-9714-424d-ac07-3d60ecaebf2e
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: 711d364c36c3aba699326f4a76d42891ba3219fb
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="analyze-twitter-data-using-hive-in-hdinsight"></a><span data-ttu-id="d03e7-103">Hdinsight Hive eszközzel Twitter-adatok elemzése</span><span class="sxs-lookup"><span data-stu-id="d03e7-103">Analyze Twitter data using Hive in HDInsight</span></span>
<span data-ttu-id="d03e7-104">Közösségi webhelyek egyik fő növeli a big data alkalmazására vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="d03e7-104">Social websites are one of the major driving forces for big-data adoption.</span></span> <span data-ttu-id="d03e7-105">Nyilvános API-k, például a Twitter helyek által biztosított az hasznos adatforrást ismertetése népszerű trendeket és elemzésére.</span><span class="sxs-lookup"><span data-stu-id="d03e7-105">Public APIs provided by sites like Twitter are a useful source of data for analyzing and understanding popular trends.</span></span>
<span data-ttu-id="d03e7-106">Ebben az oktatóanyagban meg fog Twitter-üzeneteket beolvasni egy Twitter-adatfolyam-API használatával, és használja Apache Hive on Azure HDInsight a Twitter-felhasználók, akik a legtöbb Twitter-üzeneteket, amelyek egy adott szó található küldött listáját.</span><span class="sxs-lookup"><span data-stu-id="d03e7-106">In this tutorial, you will get tweets by using a Twitter streaming API, and then use Apache Hive on Azure HDInsight to get a list of Twitter users who sent the most tweets that contained a certain word.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d03e7-107">A jelen dokumentumban leírt lépések egy Windows-alapú HDInsight-fürt szükséges.</span><span class="sxs-lookup"><span data-stu-id="d03e7-107">The steps in this document require a Windows-based HDInsight cluster.</span></span> <span data-ttu-id="d03e7-108">A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható.</span><span class="sxs-lookup"><span data-stu-id="d03e7-108">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="d03e7-109">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="d03e7-109">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span> <span data-ttu-id="d03e7-110">Linux-alapú fürthöz adott lépései: [Hive HDInsight (Linux) használatával elemezheti Twitter adatok](hdinsight-analyze-twitter-data-linux.md).</span><span class="sxs-lookup"><span data-stu-id="d03e7-110">For steps specific to a Linux-based cluster, see [Analyze Twitter data using Hive in HDInsight (Linux)](hdinsight-analyze-twitter-data-linux.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d03e7-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d03e7-111">Prerequisites</span></span>
<span data-ttu-id="d03e7-112">Az oktatóanyag elkezdéséhez az alábbiakkal kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="d03e7-112">Before you begin this tutorial, you must have the following:</span></span>

* <span data-ttu-id="d03e7-113">**A munkaállomás** az Azure PowerShell telepítése és konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="d03e7-113">**A workstation** with Azure PowerShell installed and configured.</span></span>

    <span data-ttu-id="d03e7-114">Windows PowerShell-parancsfájlok végrehajtása az Azure PowerShell futtassa rendszergazdaként és a végrehajtási házirend beállítása *RemoteSigned*.</span><span class="sxs-lookup"><span data-stu-id="d03e7-114">To execute Windows PowerShell scripts, you must run Azure PowerShell as administrator and set the execution policy to *RemoteSigned*.</span></span> <span data-ttu-id="d03e7-115">Lásd: [futtassa a Windows PowerShell-parancsfájlok][powershell-script].</span><span class="sxs-lookup"><span data-stu-id="d03e7-115">See [Run Windows PowerShell scripts][powershell-script].</span></span>

    <span data-ttu-id="d03e7-116">Windows PowerShell-parancsfájlok futtatásakor, mielőtt győződjön meg arról, hogy az Azure-előfizetéshez a következő parancsmag használatával csatlakozik:</span><span class="sxs-lookup"><span data-stu-id="d03e7-116">Before running Windows PowerShell scripts, make sure you are connected to your Azure subscription by using the following cmdlet:</span></span>

    ```powershell
    Login-AzureRmAccount
    ```

    <span data-ttu-id="d03e7-117">Ha több Azure-előfizetéssel rendelkezik, a jelenlegi előfizetés beállításához használja a következő parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="d03e7-117">If you have multiple Azure subscriptions, use the following cmdlet to set the current subscription:</span></span>

    ```powershell
    Select-AzureRmSubscription -SubscriptionID <Azure Subscription ID>
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="d03e7-118">A HDInsight-erőforrások Azure Service Managerrel történő kezelésének Azure PowerShell-támogatása **elavult**, így 2017. január 1-től megszűnt.</span><span class="sxs-lookup"><span data-stu-id="d03e7-118">Azure PowerShell support for managing HDInsight resources using Azure Service Manager is **deprecated**, and was removed on January 1, 2017.</span></span> <span data-ttu-id="d03e7-119">A jelen dokumentumban leírt lépések az új HDInsight-parancsmagokat használják, amelyek az Azure Resource Managerrel működnek.</span><span class="sxs-lookup"><span data-stu-id="d03e7-119">The steps in this document use the new HDInsight cmdlets that work with Azure Resource Manager.</span></span>
    >
    > <span data-ttu-id="d03e7-120">Kérjük, kövesse az alábbi cikkben leírt lépéseket az Azure PowerShell legújabb verziójának telepítéséhez: [Install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) (Az Azure PowerShell letöltése és konfigurálása).</span><span class="sxs-lookup"><span data-stu-id="d03e7-120">Please follow the steps in [Install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) to install the latest version of Azure PowerShell.</span></span> <span data-ttu-id="d03e7-121">Ha vannak olyan parancsprogramjai, amelyeket módosítani kell az új, az Azure Resource Managerrel működő parancsmagok használatához, tekintse meg az alábbi cikket: [Migrating to Azure Resource Manager-based development tools for HDInsight clusters](hdinsight-hadoop-development-using-azure-resource-manager.md) (Az Azure Resource Manager-alapú fejlesztési eszközökre való áttérés HDInsight-fürtök esetén).</span><span class="sxs-lookup"><span data-stu-id="d03e7-121">If you have scripts that need to be modified to use the new cmdlets that work with Azure Resource Manager, see [Migrating to Azure Resource Manager-based development tools for HDInsight clusters](hdinsight-hadoop-development-using-azure-resource-manager.md) for more information.</span></span>

* <span data-ttu-id="d03e7-122">**Egy Azure HDInsight fürt**.</span><span class="sxs-lookup"><span data-stu-id="d03e7-122">**An Azure HDInsight cluster**.</span></span> <span data-ttu-id="d03e7-123">A fürtök kiépítése útmutatásért lásd: [használatának megkezdésében a HDInsight] [ hdinsight-get-started] vagy [Provision HDInsight clusters][hdinsight-provision].</span><span class="sxs-lookup"><span data-stu-id="d03e7-123">For instructions on cluster provisioning, see [Get started using HDInsight][hdinsight-get-started] or [Provision HDInsight clusters][hdinsight-provision].</span></span> <span data-ttu-id="d03e7-124">Az oktatóanyag későbbi részében szüksége lesz a fürt nevét.</span><span class="sxs-lookup"><span data-stu-id="d03e7-124">You will need the cluster name later in the tutorial.</span></span>

<span data-ttu-id="d03e7-125">Az alábbi táblázat az ebben az oktatóanyagban használt fájlok:</span><span class="sxs-lookup"><span data-stu-id="d03e7-125">The following table lists the files used in this tutorial:</span></span>

| <span data-ttu-id="d03e7-126">Fájlok</span><span class="sxs-lookup"><span data-stu-id="d03e7-126">Files</span></span> | <span data-ttu-id="d03e7-127">Leírás</span><span class="sxs-lookup"><span data-stu-id="d03e7-127">Description</span></span> |
| --- | --- |
| <span data-ttu-id="d03e7-128">/tutorials/Twitter/Data/tweets.txt</span><span class="sxs-lookup"><span data-stu-id="d03e7-128">/tutorials/twitter/data/tweets.txt</span></span> |<span data-ttu-id="d03e7-129">A Hive-feladatokban a forrásadatokban.</span><span class="sxs-lookup"><span data-stu-id="d03e7-129">The source data for the Hive job.</span></span> |
| <span data-ttu-id="d03e7-130">/tutorials/Twitter/output</span><span class="sxs-lookup"><span data-stu-id="d03e7-130">/tutorials/twitter/output</span></span> |<span data-ttu-id="d03e7-131">A Hive-feladat kimeneti mappa.</span><span class="sxs-lookup"><span data-stu-id="d03e7-131">The output folder for the Hive job.</span></span> <span data-ttu-id="d03e7-132">A Hive feladat kimeneti fájl alapértelmezett neve a **000000_0**.</span><span class="sxs-lookup"><span data-stu-id="d03e7-132">The default Hive job output file name is **000000_0**.</span></span> |
| <span data-ttu-id="d03e7-133">tutorials/Twitter/Twitter.hql</span><span class="sxs-lookup"><span data-stu-id="d03e7-133">tutorials/twitter/twitter.hql</span></span> |<span data-ttu-id="d03e7-134">A HiveQL-parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="d03e7-134">The HiveQL script file.</span></span> |
| <span data-ttu-id="d03e7-135">/tutorials/Twitter/JobStatus</span><span class="sxs-lookup"><span data-stu-id="d03e7-135">/tutorials/twitter/jobstatus</span></span> |<span data-ttu-id="d03e7-136">A Hadoop-feladat állapotát.</span><span class="sxs-lookup"><span data-stu-id="d03e7-136">The Hadoop job status.</span></span> |

## <a name="get-twitter-feed"></a><span data-ttu-id="d03e7-137">Get Twitter-hírcsatorna</span><span class="sxs-lookup"><span data-stu-id="d03e7-137">Get Twitter feed</span></span>
<span data-ttu-id="d03e7-138">Ebben az oktatóanyagban fogja használni a [streamelési API-k Twitter][twitter-streaming-api].</span><span class="sxs-lookup"><span data-stu-id="d03e7-138">In this tutorial, you will use the [Twitter streaming APIs][twitter-streaming-api].</span></span> <span data-ttu-id="d03e7-139">Az adott adatfolyam-használandó API Twitter van [állapotok-vagy szűrőkezelő][twitter-statuses-filter].</span><span class="sxs-lookup"><span data-stu-id="d03e7-139">The specific Twitter streaming API you will use is [statuses/filter][twitter-statuses-filter].</span></span>

> [!NOTE]
> <span data-ttu-id="d03e7-140">10 000 Twitter-üzeneteket tartalmazó fájlt, és a Hive parancsfájl (a következő szakaszban ismertetett) fel lett töltve, egy nyilvános Blob tárolóban.</span><span class="sxs-lookup"><span data-stu-id="d03e7-140">A file containing 10,000 tweets and the Hive script file (covered in the next section) have been uploaded in a public Blob container.</span></span> <span data-ttu-id="d03e7-141">Ez a szakasz kihagyhatja, ha szeretné használni a feltöltött fájlok.</span><span class="sxs-lookup"><span data-stu-id="d03e7-141">You can skip this section if you want to use the uploaded files.</span></span>

<span data-ttu-id="d03e7-142">[Twitter-üzeneteket adatok](https://dev.twitter.com/docs/platform-objects/tweets) összetett beágyazott szerkezetet tartalmaz JavaScript Object Notation (JSON) formátumban tárolja.</span><span class="sxs-lookup"><span data-stu-id="d03e7-142">[Tweets data](https://dev.twitter.com/docs/platform-objects/tweets) is stored in the JavaScript Object Notation (JSON) format that contains a complex nested structure.</span></span> <span data-ttu-id="d03e7-143">Ahelyett, hogy hány sornyi kód írása a hagyományos programozási nyelv használatával, alakíthatja át a beágyazott struktúra egy Hive táblába úgy, hogy azt egy Structured Query Language (SQL) lekérdezhetők – például a HiveQL nevű nyelv.</span><span class="sxs-lookup"><span data-stu-id="d03e7-143">Instead of writing many lines of code by using a conventional programming language, you can transform this nested structure into a Hive table, so that it can be queried by a Structured Query Language (SQL)-like language called HiveQL.</span></span>

<span data-ttu-id="d03e7-144">Twitter OAuth használ meghatalmazott hozzáférést biztosítson az API-hoz.</span><span class="sxs-lookup"><span data-stu-id="d03e7-144">Twitter uses OAuth to provide authorized access to its API.</span></span> <span data-ttu-id="d03e7-145">OAuth olyan hitelesítési protokoll, amely lehetővé teszi a felhasználók jóváhagyása nélkül megosztása a jelszavát a nevében eljárni alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="d03e7-145">OAuth is an authentication protocol that allows users to approve applications to act on their behalf without sharing their password.</span></span> <span data-ttu-id="d03e7-146">További információ található [oauth.net](http://oauth.net/) vagy a kiváló [Alapszintű útmutató az OAuth](http://hueniverse.com/oauth/) a Hueniverse.</span><span class="sxs-lookup"><span data-stu-id="d03e7-146">More information can be found at [oauth.net](http://oauth.net/) or in the excellent [Beginner's Guide to OAuth](http://hueniverse.com/oauth/) from Hueniverse.</span></span>

<span data-ttu-id="d03e7-147">Az első lépés lehetővé teszi az OAuth-hoz egy új alkalmazás létrehozása a fejlesztői Twitter-helyen.</span><span class="sxs-lookup"><span data-stu-id="d03e7-147">The first step to use OAuth is to create a new application on the Twitter Developer site.</span></span>

<span data-ttu-id="d03e7-148">**Twitter-alkalmazás létrehozása**</span><span class="sxs-lookup"><span data-stu-id="d03e7-148">**To create a Twitter application**</span></span>

1. <span data-ttu-id="d03e7-149">Jelentkezzen be [https://apps.twitter.com/](https://apps.twitter.com/).</span><span class="sxs-lookup"><span data-stu-id="d03e7-149">Sign in to [https://apps.twitter.com/](https://apps.twitter.com/).</span></span> <span data-ttu-id="d03e7-150">Kattintson a **feliratkozás most** hivatkozásra, ha egy Twitter-fiók nem rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="d03e7-150">Click the **Sign up now** link if you don't have a Twitter account.</span></span>
2. <span data-ttu-id="d03e7-151">Kattintson a **új alkalmazás létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="d03e7-151">Click **Create New App**.</span></span>
3. <span data-ttu-id="d03e7-152">Adja meg **neve**, **leírás**, **webhely**.</span><span class="sxs-lookup"><span data-stu-id="d03e7-152">Enter **Name**, **Description**, **Website**.</span></span> <span data-ttu-id="d03e7-153">Hogy fel egy URL-címet a **webhely** mező.</span><span class="sxs-lookup"><span data-stu-id="d03e7-153">You can make up a URL for the **Website** field.</span></span> <span data-ttu-id="d03e7-154">A következő táblázatban néhány példa értékeket:</span><span class="sxs-lookup"><span data-stu-id="d03e7-154">The following table shows some sample values to use:</span></span>

   | <span data-ttu-id="d03e7-155">Mező</span><span class="sxs-lookup"><span data-stu-id="d03e7-155">Field</span></span> | <span data-ttu-id="d03e7-156">Érték</span><span class="sxs-lookup"><span data-stu-id="d03e7-156">Value</span></span> |
   | --- | --- |
   |  <span data-ttu-id="d03e7-157">Név</span><span class="sxs-lookup"><span data-stu-id="d03e7-157">Name</span></span> |<span data-ttu-id="d03e7-158">MyHDInsightApp</span><span class="sxs-lookup"><span data-stu-id="d03e7-158">MyHDInsightApp</span></span> |
   |  <span data-ttu-id="d03e7-159">Leírás</span><span class="sxs-lookup"><span data-stu-id="d03e7-159">Description</span></span> |<span data-ttu-id="d03e7-160">MyHDInsightApp</span><span class="sxs-lookup"><span data-stu-id="d03e7-160">MyHDInsightApp</span></span> |
   |  <span data-ttu-id="d03e7-161">Honlap</span><span class="sxs-lookup"><span data-stu-id="d03e7-161">Website</span></span> |<span data-ttu-id="d03e7-162">http://www.myhdinsightapp.com</span><span class="sxs-lookup"><span data-stu-id="d03e7-162">http://www.myhdinsightapp.com</span></span> |
4. <span data-ttu-id="d03e7-163">Ellenőrizze **Igen, elfogadom**, és kattintson a **az Twitter-alkalmazás létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="d03e7-163">Check **Yes, I agree**, and then click **Create your Twitter application**.</span></span>
5. <span data-ttu-id="d03e7-164">Kattintson a **engedélyek** fülre.</span><span class="sxs-lookup"><span data-stu-id="d03e7-164">Click the **Permissions** tab.</span></span> <span data-ttu-id="d03e7-165">Az alapértelmezett engedély **csak olvasható**.</span><span class="sxs-lookup"><span data-stu-id="d03e7-165">The default permission is **Read only**.</span></span> <span data-ttu-id="d03e7-166">Ez a megfelelő ehhez az oktatóanyaghoz.</span><span class="sxs-lookup"><span data-stu-id="d03e7-166">This is sufficient for this tutorial.</span></span>
6. <span data-ttu-id="d03e7-167">Kattintson a **kulcsok és a hozzáférési jogkivonatok** fülre.</span><span class="sxs-lookup"><span data-stu-id="d03e7-167">Click the **Keys and Access Tokens** tab.</span></span>
7. <span data-ttu-id="d03e7-168">Kattintson a **a hozzáférési jogkivonat létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="d03e7-168">Click **Create my access token**.</span></span>
8. <span data-ttu-id="d03e7-169">Kattintson a **teszt OAuth** a lap jobb felső sarkában.</span><span class="sxs-lookup"><span data-stu-id="d03e7-169">Click **Test OAuth** in the upper-right corner of the page.</span></span>
9. <span data-ttu-id="d03e7-170">Írja le **kulcsa**, **felhasználói titok**, **hozzáférési jogkivonat**, és **Access token titkos**.</span><span class="sxs-lookup"><span data-stu-id="d03e7-170">Write down **consumer key**, **Consumer secret**, **Access token**, and **Access token secret**.</span></span> <span data-ttu-id="d03e7-171">Az oktatóanyag későbbi részében szüksége lesz az értékeket.</span><span class="sxs-lookup"><span data-stu-id="d03e7-171">You will need the values later in the tutorial.</span></span>

<span data-ttu-id="d03e7-172">Ebben az oktatóanyagban a Windows PowerShell használandó ellenőrizze a webes szolgáltatás hívása.</span><span class="sxs-lookup"><span data-stu-id="d03e7-172">In this tutorial, you will use Windows PowerShell to make the web service call.</span></span> <span data-ttu-id="d03e7-173">.NET C# mintát, lásd: [elemzése a HBase a hdinsight eszközben valós idejű Twitter sentiment][hdinsight-hbase-twitter-sentiment].</span><span class="sxs-lookup"><span data-stu-id="d03e7-173">For a .NET C# sample, see [Analyze real-time Twitter sentiment with HBase in HDInsight][hdinsight-hbase-twitter-sentiment].</span></span> <span data-ttu-id="d03e7-174">A webszolgáltatás-hívások egyéb népszerű eszköz [ *Curl*][curl].</span><span class="sxs-lookup"><span data-stu-id="d03e7-174">The other popular tool to make web service calls is [*Curl*][curl].</span></span> <span data-ttu-id="d03e7-175">Curl letölthető [Itt][curl-download].</span><span class="sxs-lookup"><span data-stu-id="d03e7-175">Curl can be downloaded from [here][curl-download].</span></span>

> [!NOTE]
> <span data-ttu-id="d03e7-176">Használatakor a curl-parancsot a Windows idézőjelek közé foglalt helyett használjon szimpla idézőjelben az beállítási értékeit.</span><span class="sxs-lookup"><span data-stu-id="d03e7-176">When you use the curl command in Windows, use double quotes instead of single quotes for the option values.</span></span>

<span data-ttu-id="d03e7-177">**Twitter-üzeneteket beolvasni**</span><span class="sxs-lookup"><span data-stu-id="d03e7-177">**To get tweets**</span></span>

1. <span data-ttu-id="d03e7-178">Nyissa meg a Windows PowerShell integrált parancsfájlkezelési környezet (ISE).</span><span class="sxs-lookup"><span data-stu-id="d03e7-178">Open the Windows PowerShell Integrated Scripting Environment (ISE).</span></span> <span data-ttu-id="d03e7-179">(A Windows 8 kezdőképernyőn írja be a **PowerShell_ISE** majd **Windows PowerShell ISE**.</span><span class="sxs-lookup"><span data-stu-id="d03e7-179">(On the Windows 8 Start screen, type **PowerShell_ISE** and then click **Windows PowerShell ISE**.</span></span> <span data-ttu-id="d03e7-180">Lásd: [indítsa el a Windows PowerShell használatával Windows 8 és Windows][powershell-start].)</span><span class="sxs-lookup"><span data-stu-id="d03e7-180">See [Start Windows PowerShell on Windows 8 and Windows][powershell-start].)</span></span>
2. <span data-ttu-id="d03e7-181">Másolja a következő a parancsfájl ablaktáblára:</span><span class="sxs-lookup"><span data-stu-id="d03e7-181">Copy the following script into the script pane:</span></span>

    ```powershell
    #region - variables and constants
    $clusterName = "<HDInsightClusterName>" # Enter the HDInsight cluster name

    # Enter the OAuth information for your Twitter application
    $oauth_consumer_key = "<TwitterAppConsumerKey>";
    $oauth_consumer_secret = "<TwitterAppConsumerSecret>";
    $oauth_token = "<TwitterAppAccessToken>";
    $oauth_token_secret = "<TwitterAppAccessTokenSecret>";

    $destBlobName = "tutorials/twitter/data/tweets.txt" # This script saves the tweets into this blob.

    $trackString = "Azure, Cloud, HDInsight" # This script gets the tweets containing these keywords.
    $track = [System.Uri]::EscapeDataString($trackString);
    $lineMax = 10000  # The script will get this number of tweets. It is about 3 minutes every 100 lines.
    #endregion

    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    Login-AzureRmAccount
    #endregion

    #region - Create a block blob object for writing tweets into Blob storage
    Write-Host "Get the default storage account name and Blob container name using the cluster name ..." -ForegroundColor Green
    $myCluster = Get-AzureRmHDInsightCluster -Name $clusterName
    $resourceGroupName = $myCluster.ResourceGroup
    $storageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
    $containerName = $myCluster.DefaultStorageContainer
    Write-Host "`tThe storage account name is $storageAccountName." -ForegroundColor Yellow
    Write-Host "`tThe blob container name is $containerName." -ForegroundColor Yellow

    Write-Host "Define the Azure storage connection string ..." -ForegroundColor Green
    $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
    $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$storageAccountName;AccountKey=$storageAccountKey"
    Write-Host "`tThe connection string is $storageConnectionString." -ForegroundColor Yellow

    Write-Host "Create block blob object ..." -ForegroundColor Green
    $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
    $storageClient = $storageAccount.CreateCloudBlobClient();
    $storageContainer = $storageClient.GetContainerReference($containerName)
    $destBlob = $storageContainer.GetBlockBlobReference($destBlobName)
    #end region

    # region - Format OAuth strings
    Write-Host "Format oauth strings ..." -ForegroundColor Green
    $oauth_nonce = [System.Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes([System.DateTime]::Now.Ticks.ToString()));
    $ts = [System.DateTime]::UtcNow - [System.DateTime]::ParseExact("01/01/1970", "dd/MM/yyyy", $null)
    $oauth_timestamp = [System.Convert]::ToInt64($ts.TotalSeconds).ToString();

    $signature = "POST&";
    $signature += [System.Uri]::EscapeDataString("https://stream.twitter.com/1.1/statuses/filter.json") + "&";
    $signature += [System.Uri]::EscapeDataString("oauth_consumer_key=" + $oauth_consumer_key + "&");
    $signature += [System.Uri]::EscapeDataString("oauth_nonce=" + $oauth_nonce + "&");
    $signature += [System.Uri]::EscapeDataString("oauth_signature_method=HMAC-SHA1&");
    $signature += [System.Uri]::EscapeDataString("oauth_timestamp=" + $oauth_timestamp + "&");
    $signature += [System.Uri]::EscapeDataString("oauth_token=" + $oauth_token + "&");
    $signature += [System.Uri]::EscapeDataString("oauth_version=1.0&");
    $signature += [System.Uri]::EscapeDataString("track=" + $track);

    $signature_key = [System.Uri]::EscapeDataString($oauth_consumer_secret) + "&" + [System.Uri]::EscapeDataString($oauth_token_secret);

    $hmacsha1 = new-object System.Security.Cryptography.HMACSHA1;
    $hmacsha1.Key = [System.Text.Encoding]::ASCII.GetBytes($signature_key);
    $oauth_signature = [System.Convert]::ToBase64String($hmacsha1.ComputeHash([System.Text.Encoding]::ASCII.GetBytes($signature)));

    $oauth_authorization = 'OAuth ';
    $oauth_authorization += 'oauth_consumer_key="' + [System.Uri]::EscapeDataString($oauth_consumer_key) + '",';
    $oauth_authorization += 'oauth_nonce="' + [System.Uri]::EscapeDataString($oauth_nonce) + '",';
    $oauth_authorization += 'oauth_signature="' + [System.Uri]::EscapeDataString($oauth_signature) + '",';
    $oauth_authorization += 'oauth_signature_method="HMAC-SHA1",'
    $oauth_authorization += 'oauth_timestamp="' + [System.Uri]::EscapeDataString($oauth_timestamp) + '",'
    $oauth_authorization += 'oauth_token="' + [System.Uri]::EscapeDataString($oauth_token) + '",';
    $oauth_authorization += 'oauth_version="1.0"';

    $post_body = [System.Text.Encoding]::ASCII.GetBytes("track=" + $track);
    #endregion

    #region - Read tweets
    Write-Host "Create HTTP web request ..." -ForegroundColor Green
    [System.Net.HttpWebRequest] $request = [System.Net.WebRequest]::Create("https://stream.twitter.com/1.1/statuses/filter.json");
    $request.Method = "POST";
    $request.Headers.Add("Authorization", $oauth_authorization);
    $request.ContentType = "application/x-www-form-urlencoded";
    $body = $request.GetRequestStream();

    $body.write($post_body, 0, $post_body.length);
    $body.flush();
    $body.close();
    $response = $request.GetResponse() ;

    Write-Host "Start stream reading ..." -ForegroundColor Green

    Write-Host "Define a MemoryStream and a StreamWriter for writing ..." -ForegroundColor Green
    $memStream = New-Object System.IO.MemoryStream
    $writeStream = New-Object System.IO.StreamWriter $memStream

    $sReader = New-Object System.IO.StreamReader($response.GetResponseStream())

    $inrec = $sReader.ReadLine()
    $count = 0
    while (($inrec -ne $null) -and ($count -le $lineMax))
    {
        if ($inrec -ne "")
        {
            Write-Host "`n`t $count tweets received." -ForegroundColor Yellow

            $writeStream.WriteLine($inrec)
            $count ++
        }

        $inrec=$sReader.ReadLine()
    }
    #endregion

    #region - Write tweets to Blob storage
    Write-Host "Write to the destination blob ..." -ForegroundColor Green
    $writeStream.Flush()
    $memStream.Seek(0, "Begin")
    $destBlob.UploadFromStream($memStream)

    $sReader.close()
    #endregion

    Write-Host "Completed!" -ForegroundColor Green
    ```

3. <span data-ttu-id="d03e7-182">Állítsa be az első 5-8 változókat a parancsfájl:</span><span class="sxs-lookup"><span data-stu-id="d03e7-182">Set the first five to eight variables in the script:</span></span>

    <span data-ttu-id="d03e7-183">Változó</span><span class="sxs-lookup"><span data-stu-id="d03e7-183">Variable</span></span>|<span data-ttu-id="d03e7-184">Leírás</span><span class="sxs-lookup"><span data-stu-id="d03e7-184">Description</span></span>
    ---|---
    <span data-ttu-id="d03e7-185">$clusterName</span><span class="sxs-lookup"><span data-stu-id="d03e7-185">$clusterName</span></span>|<span data-ttu-id="d03e7-186">Azt a nevet a HDInsight-fürt, ahol az alkalmazást futtatni szeretné.</span><span class="sxs-lookup"><span data-stu-id="d03e7-186">This is the name of the HDInsight cluster where you want to run the application.</span></span>
    <span data-ttu-id="d03e7-187">$oauth_consumer_key</span><span class="sxs-lookup"><span data-stu-id="d03e7-187">$oauth_consumer_key</span></span>|<span data-ttu-id="d03e7-188">Ez az a Twitter-alkalmazás **kulcsa** megírt korábban a Twitter-alkalmazás létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="d03e7-188">This is the Twitter application **consumer key** you wrote down earlier when you created the Twitter application.</span></span>
    <span data-ttu-id="d03e7-189">$oauth_consumer_secret</span><span class="sxs-lookup"><span data-stu-id="d03e7-189">$oauth_consumer_secret</span></span>|<span data-ttu-id="d03e7-190">Ez az a Twitter-alkalmazás **felhasználói titok** korábban megírt.</span><span class="sxs-lookup"><span data-stu-id="d03e7-190">This is the Twitter application **consumer secret** you wrote down earlier.</span></span>
    <span data-ttu-id="d03e7-191">$oauth_token</span><span class="sxs-lookup"><span data-stu-id="d03e7-191">$oauth_token</span></span>|<span data-ttu-id="d03e7-192">Ez az a Twitter-alkalmazás **hozzáférési jogkivonat** korábban megírt.</span><span class="sxs-lookup"><span data-stu-id="d03e7-192">This is the Twitter application **access token** you wrote down earlier.</span></span>
    <span data-ttu-id="d03e7-193">$oauth_token_secret</span><span class="sxs-lookup"><span data-stu-id="d03e7-193">$oauth_token_secret</span></span>|<span data-ttu-id="d03e7-194">Ez az a Twitter-alkalmazás **access token titkos** korábban megírt.</span><span class="sxs-lookup"><span data-stu-id="d03e7-194">This is the Twitter application **access token secret** you wrote down earlier.</span></span>
    <span data-ttu-id="d03e7-195">$destBlobName</span><span class="sxs-lookup"><span data-stu-id="d03e7-195">$destBlobName</span></span>|<span data-ttu-id="d03e7-196">Ez az a kimeneti blob neve.</span><span class="sxs-lookup"><span data-stu-id="d03e7-196">This is the output blob name.</span></span> <span data-ttu-id="d03e7-197">Az alapértelmezett érték **tutorials/twitter/data/tweets.txt**.</span><span class="sxs-lookup"><span data-stu-id="d03e7-197">The default value is **tutorials/twitter/data/tweets.txt**.</span></span> <span data-ttu-id="d03e7-198">Ha megváltoztatja az alapértelmezett érték, szüksége lesz a Windows PowerShell-parancsfájlok ennek megfelelően frissülnek.</span><span class="sxs-lookup"><span data-stu-id="d03e7-198">If you change the default value, you will need to update the Windows PowerShell scripts accordingly.</span></span>
    <span data-ttu-id="d03e7-199">$trackString</span><span class="sxs-lookup"><span data-stu-id="d03e7-199">$trackString</span></span>|<span data-ttu-id="d03e7-200">A webszolgáltatás visszatér a következő kulcsszavak kapcsolódó Twitter-üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="d03e7-200">The web service will return tweets related to these keywords.</span></span> <span data-ttu-id="d03e7-201">Az alapértelmezett érték **Azure-felhő, HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="d03e7-201">The default value is **Azure, Cloud, HDInsight**.</span></span> <span data-ttu-id="d03e7-202">Ha megváltoztatja az alapértelmezett érték, ennek megfelelően frissíti a Windows PowerShell-parancsfájlokat.</span><span class="sxs-lookup"><span data-stu-id="d03e7-202">If you change the default value, you will update the Windows PowerShell scripts accordingly.</span></span>
    <span data-ttu-id="d03e7-203">$lineMax</span><span class="sxs-lookup"><span data-stu-id="d03e7-203">$lineMax</span></span>|<span data-ttu-id="d03e7-204">Az érték határozza meg, hány Twitter-üzeneteket, a parancsfájl olvasását.</span><span class="sxs-lookup"><span data-stu-id="d03e7-204">The value determines how many tweets the script will read.</span></span> <span data-ttu-id="d03e7-205">Három perc olvasási 100 Twitter-üzeneteket vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="d03e7-205">It takes about three minutes to read 100 tweets.</span></span> <span data-ttu-id="d03e7-206">Megadhat egy nagyobb számot, de letöltése több ideig tart.</span><span class="sxs-lookup"><span data-stu-id="d03e7-206">You can set a larger number, but it will take more time to download.</span></span>

1. <span data-ttu-id="d03e7-207">A szkript futtatásához nyomja le az **F5** billentyűt.</span><span class="sxs-lookup"><span data-stu-id="d03e7-207">Press **F5** to run the script.</span></span> <span data-ttu-id="d03e7-208">Ha a probléma megoldásához, problémát tapasztal a sorok, és nyomja le az **F8**.</span><span class="sxs-lookup"><span data-stu-id="d03e7-208">If you run into problems, as a workaround, select all the lines, and then press **F8**.</span></span>
2. <span data-ttu-id="d03e7-209">Ekkor megjelenik a "Complete!"</span><span class="sxs-lookup"><span data-stu-id="d03e7-209">You shall see "Complete!"</span></span> <span data-ttu-id="d03e7-210">a kimeneti végén.</span><span class="sxs-lookup"><span data-stu-id="d03e7-210">at the end of the output.</span></span> <span data-ttu-id="d03e7-211">Piros hibaüzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="d03e7-211">Any error messages will be displayed in red.</span></span>

<span data-ttu-id="d03e7-212">Egy érvényesítési eljárással ellenőrizheti a kimeneti fájl **/tutorials/twitter/data/tweets.txt**, a Azure Blob-tároló egy Azure Tártallózó vagy az Azure PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="d03e7-212">As a validation procedure, you can check the output file, **/tutorials/twitter/data/tweets.txt**, on your Azure Blob storage by using an Azure storage explorer or Azure PowerShell.</span></span> <span data-ttu-id="d03e7-213">A Windows PowerShell-parancsfájlpélda a fájlok listázása, lásd: [és a HDInsight együttes használata a Blob storage][hdinsight-storage-powershell].</span><span class="sxs-lookup"><span data-stu-id="d03e7-213">For a sample Windows PowerShell script for listing files, see [Use Blob storage with HDInsight][hdinsight-storage-powershell].</span></span>

## <a name="create-hiveql-script"></a><span data-ttu-id="d03e7-214">Hozzon létre HiveQL-parancsfájlt</span><span class="sxs-lookup"><span data-stu-id="d03e7-214">Create HiveQL script</span></span>
<span data-ttu-id="d03e7-215">Az Azure PowerShell használatával több HiveQL utasítást egy olyan időpontra, vagy egy parancsfájl fájlba a HiveQL utasítás csomag.</span><span class="sxs-lookup"><span data-stu-id="d03e7-215">Using Azure PowerShell, you can run multiple HiveQL statements one at a time, or package the HiveQL statement into a script file.</span></span> <span data-ttu-id="d03e7-216">Ebben az oktatóanyagban létrehoz egy HiveQL-parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="d03e7-216">In this tutorial, you will create a HiveQL script.</span></span> <span data-ttu-id="d03e7-217">A parancsfájl az Azure Blob storage fel kell tölteni.</span><span class="sxs-lookup"><span data-stu-id="d03e7-217">The script file must be uploaded to Azure Blob storage.</span></span> <span data-ttu-id="d03e7-218">A következő szakaszban az Azure PowerShell használatával fog futni a parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="d03e7-218">In the next section, you will run the script file by using Azure PowerShell.</span></span>

> [!NOTE]
> <span data-ttu-id="d03e7-219">A Hive parancsfájl és 10 000 Twitter-üzeneteket tartalmazó fájl fel lett töltve egy nyilvános Blob tárolóban.</span><span class="sxs-lookup"><span data-stu-id="d03e7-219">The Hive script file and a file containing 10,000 tweets have been uploaded in a public Blob container.</span></span> <span data-ttu-id="d03e7-220">Ez a szakasz kihagyhatja, ha szeretné használni a feltöltött fájlok.</span><span class="sxs-lookup"><span data-stu-id="d03e7-220">You can skip this section if you want to use the uploaded files.</span></span>

<span data-ttu-id="d03e7-221">A HiveQL-parancsfájlt kell elvégezni a következőket:</span><span class="sxs-lookup"><span data-stu-id="d03e7-221">The HiveQL script will perform the following:</span></span>

1. <span data-ttu-id="d03e7-222">**Dobja el a tweets_raw táblát** abban az esetben, ha a tábla már létezik.</span><span class="sxs-lookup"><span data-stu-id="d03e7-222">**Drop the tweets_raw table** in case the table already exists.</span></span>
2. <span data-ttu-id="d03e7-223">**A tweets_raw Hive tábla létrehozásához**.</span><span class="sxs-lookup"><span data-stu-id="d03e7-223">**Create the tweets_raw Hive table**.</span></span> <span data-ttu-id="d03e7-224">Adatait tartalmazza a ideiglenes Hive strukturált táblázat további kinyerési, átalakítási és betöltési (ETL) feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="d03e7-224">This temporary Hive structured table holds the data for further extract, transform, and load (ETL) processing.</span></span> <span data-ttu-id="d03e7-225">A partíciókon információkért lásd: [Hive oktatóanyag][apache-hive-tutorial].</span><span class="sxs-lookup"><span data-stu-id="d03e7-225">For information on partitions, see [Hive tutorial][apache-hive-tutorial].</span></span>
3. <span data-ttu-id="d03e7-226">**Adatok betöltése** a forrásmappából, /tutorials/twitter/data.</span><span class="sxs-lookup"><span data-stu-id="d03e7-226">**Load data** from the source folder, /tutorials/twitter/data.</span></span> <span data-ttu-id="d03e7-227">A beágyazott JSON formátumban nagy Twitter-üzeneteket adatkészlet most átalakítása ideiglenes Hive tábla struktúrába.</span><span class="sxs-lookup"><span data-stu-id="d03e7-227">The large tweets dataset in nested JSON format has now been transformed into a temporary Hive table structure.</span></span>
4. <span data-ttu-id="d03e7-228">**A Twitter-üzeneteket table DROP** abban az esetben, ha a tábla már létezik.</span><span class="sxs-lookup"><span data-stu-id="d03e7-228">**Drop the tweets table** in case the table already exists.</span></span>
5. <span data-ttu-id="d03e7-229">**A Twitter-üzeneteket tábla létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="d03e7-229">**Create the tweets table**.</span></span> <span data-ttu-id="d03e7-230">Mielőtt a Hive eszközzel kérdezheti Twitter-üzeneteket adatkészlet ellen, egy másik ETL-folyamat futtatásához szükséges.</span><span class="sxs-lookup"><span data-stu-id="d03e7-230">Before you can query against the tweets dataset by using Hive, you need to run another ETL process.</span></span> <span data-ttu-id="d03e7-231">Az ETL-folyamat a "twitter_raw" táblázatban tárolt adatok részletes táblázat séma határozza meg.</span><span class="sxs-lookup"><span data-stu-id="d03e7-231">This ETL process defines a more detailed table schema for the data that you have stored in the "twitter_raw" table.</span></span>
6. <span data-ttu-id="d03e7-232">**Felülírás táblázat beszúrása**.</span><span class="sxs-lookup"><span data-stu-id="d03e7-232">**Insert overwrite table**.</span></span> <span data-ttu-id="d03e7-233">Az összetett Hive parancsfájl a Hadoop-fürt lesz indítsa hosszú MapReduce-feladatok készlete.</span><span class="sxs-lookup"><span data-stu-id="d03e7-233">This complex Hive script will kick off a set of long MapReduce jobs by the Hadoop cluster.</span></span> <span data-ttu-id="d03e7-234">Attól függően, hogy a DataSet adatkészlet és a fürt méretét Ez körülbelül 10 percig eltarthat.</span><span class="sxs-lookup"><span data-stu-id="d03e7-234">Depending on your dataset and the size of your cluster, this could take about 10 minutes.</span></span>
7. <span data-ttu-id="d03e7-235">**INSERT felülírása directory**.</span><span class="sxs-lookup"><span data-stu-id="d03e7-235">**Insert overwrite directory**.</span></span> <span data-ttu-id="d03e7-236">Lekérdezés futtatása, és a kimeneti fájlba adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="d03e7-236">Run a query and output the dataset to a file.</span></span> <span data-ttu-id="d03e7-237">Ez a lekérdezés listáját adja vissza a Twitter felhasználók, akik a legtöbb Twitter-üzeneteket, amely a word tartalmazott küldött "Azure".</span><span class="sxs-lookup"><span data-stu-id="d03e7-237">This query will return a list of Twitter users who sent most tweets that contained the word "Azure".</span></span>

<span data-ttu-id="d03e7-238">**A Hive parancsprogram létrehozásához, és töltse fel az Azure-bA**</span><span class="sxs-lookup"><span data-stu-id="d03e7-238">**To create a Hive script and upload it to Azure**</span></span>

1. <span data-ttu-id="d03e7-239">Nyissa meg a Windows PowerShell ISE.</span><span class="sxs-lookup"><span data-stu-id="d03e7-239">Open Windows PowerShell ISE.</span></span>
2. <span data-ttu-id="d03e7-240">Másolja a következő a parancsfájl ablaktáblára:</span><span class="sxs-lookup"><span data-stu-id="d03e7-240">Copy the following script into the script pane:</span></span>

    ```powershell
    #region - variables and constants
    $clusterName = "<Existing HDInsight Cluster Name>" # Enter your HDInsight cluster name
    $subscriptionID = "<Azure Subscription ID>"

    $sourceDataPath = "/tutorials/twitter/data"
    $outputPath = "/tutorials/twitter/output"
    $hqlScriptFile = "tutorials/twitter/twitter.hql"

    $hqlStatements = @"
    set hive.exec.dynamic.partition = true;
    set hive.exec.dynamic.partition.mode = nonstrict;

    DROP TABLE tweets_raw;
    CREATE EXTERNAL TABLE tweets_raw (
        json_response STRING
    )
    STORED AS TEXTFILE LOCATION '$sourceDataPath';

    DROP TABLE tweets;
    CREATE TABLE tweets
    (
        id BIGINT,
        created_at STRING,
        created_at_date STRING,
        created_at_year STRING,
        created_at_month STRING,
        created_at_day STRING,
        created_at_time STRING,
        in_reply_to_user_id_str STRING,
        text STRING,
        contributors STRING,
        retweeted STRING,
        truncated STRING,
        coordinates STRING,
        source STRING,
        retweet_count INT,
        url STRING,
        hashtags array<STRING>,
        user_mentions array<STRING>,
        first_hashtag STRING,
        first_user_mention STRING,
        screen_name STRING,
        name STRING,
        followers_count INT,
        listed_count INT,
        friends_count INT,
        lang STRING,
        user_location STRING,
        time_zone STRING,
        profile_image_url STRING,
        json_response STRING
    );

    FROM tweets_raw
    INSERT OVERWRITE TABLE tweets
    SELECT
        cast(get_json_object(json_response, '$.id_str') as BIGINT),
        get_json_object(json_response, '$.created_at'),
        concat(substr (get_json_object(json_response, '$.created_at'),1,10),' ',
        substr (get_json_object(json_response, '$.created_at'),27,4)),
        substr (get_json_object(json_response, '$.created_at'),27,4),
        case substr (get_json_object(json_response, '$.created_at'),5,3)
            when "Jan" then "01"
            when "Feb" then "02"
            when "Mar" then "03"
            when "Apr" then "04"
            when "May" then "05"
            when "Jun" then "06"
            when "Jul" then "07"
            when "Aug" then "08"
            when "Sep" then "09"
            when "Oct" then "10"
            when "Nov" then "11"
            when "Dec" then "12" end,
        substr (get_json_object(json_response, '$.created_at'),9,2),
        substr (get_json_object(json_response, '$.created_at'),12,8),
        get_json_object(json_response, '$.in_reply_to_user_id_str'),
        get_json_object(json_response, '$.text'),
        get_json_object(json_response, '$.contributors'),
        get_json_object(json_response, '$.retweeted'),
        get_json_object(json_response, '$.truncated'),
        get_json_object(json_response, '$.coordinates'),
        get_json_object(json_response, '$.source'),
        cast (get_json_object(json_response, '$.retweet_count') as INT),
        get_json_object(json_response, '$.entities.display_url'),
        array(
            trim(lower(get_json_object(json_response, '$.entities.hashtags[0].text'))),
            trim(lower(get_json_object(json_response, '$.entities.hashtags[1].text'))),
            trim(lower(get_json_object(json_response, '$.entities.hashtags[2].text'))),
            trim(lower(get_json_object(json_response, '$.entities.hashtags[3].text'))),
            trim(lower(get_json_object(json_response, '$.entities.hashtags[4].text')))),
        array(
            trim(lower(get_json_object(json_response, '$.entities.user_mentions[0].screen_name'))),
            trim(lower(get_json_object(json_response, '$.entities.user_mentions[1].screen_name'))),
            trim(lower(get_json_object(json_response, '$.entities.user_mentions[2].screen_name'))),
            trim(lower(get_json_object(json_response, '$.entities.user_mentions[3].screen_name'))),
            trim(lower(get_json_object(json_response, '$.entities.user_mentions[4].screen_name')))),
        trim(lower(get_json_object(json_response, '$.entities.hashtags[0].text'))),
        trim(lower(get_json_object(json_response, '$.entities.user_mentions[0].screen_name'))),
        get_json_object(json_response, '$.user.screen_name'),
        get_json_object(json_response, '$.user.name'),
        cast (get_json_object(json_response, '$.user.followers_count') as INT),
        cast (get_json_object(json_response, '$.user.listed_count') as INT),
        cast (get_json_object(json_response, '$.user.friends_count') as INT),
        get_json_object(json_response, '$.user.lang'),
        get_json_object(json_response, '$.user.location'),
        get_json_object(json_response, '$.user.time_zone'),
        get_json_object(json_response, '$.user.profile_image_url'),
        json_response
    WHERE (length(json_response) > 500);

    INSERT OVERWRITE DIRECTORY '$outputPath'
    SELECT name, screen_name, count(1) as cc
        FROM tweets
        WHERE text like "%Azure%"
        GROUP BY name,screen_name
        ORDER BY cc DESC LIMIT 10;
    "@
    #endregion

    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green

    Try{
        Get-AzureRmSubscription
    }
    Catch{
        Login-AzureRmAccount
    }

    Select-AzureRmSubscription -SubscriptionId $subscriptionID

    #endregion

    #region - Create a block blob object for writing the Hive script file
    Write-Host "Get the default storage account name and container name based on the cluster name ..." -ForegroundColor Green
    $myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $myCluster.ResourceGroup
    $defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
    $defaultBlobContainerName = $myCluster.DefaultStorageContainer
    Write-Host "`tThe storage account name is $defaultStorageAccountName." -ForegroundColor Yellow
    Write-Host "`tThe blob container name is $defaultBlobContainerName." -ForegroundColor Yellow

    Write-Host "Define the connection string ..." -ForegroundColor Green
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$defaultStorageAccountName;AccountKey=$defaultStorageAccountKey"

    Write-Host "Create block blob objects referencing the hql script file" -ForegroundColor Green
    $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
    $storageClient = $storageAccount.CreateCloudBlobClient();
    $storageContainer = $storageClient.GetContainerReference($defaultBlobContainerName)
    $hqlScriptBlob = $storageContainer.GetBlockBlobReference($hqlScriptFile)

    Write-Host "Define a MemoryStream and a StreamWriter for writing ... " -ForegroundColor Green
    $memStream = New-Object System.IO.MemoryStream
    $writeStream = New-Object System.IO.StreamWriter $memStream
    $writeStream.Writeline($hqlStatements)
    #endregion

    #region - Write the Hive script file to Blob storage
    Write-Host "Write to the destination blob ... " -ForegroundColor Green
    $writeStream.Flush()
    $memStream.Seek(0, "Begin")
    $hqlScriptBlob.UploadFromStream($memStream)
    #endregion

    Write-Host "Completed!" -ForegroundColor Green
    ```

3. <span data-ttu-id="d03e7-241">A parancsfájl az első két változók megadása:</span><span class="sxs-lookup"><span data-stu-id="d03e7-241">Set the first two variables in the script:</span></span>

   | <span data-ttu-id="d03e7-242">Változó</span><span class="sxs-lookup"><span data-stu-id="d03e7-242">Variable</span></span> | <span data-ttu-id="d03e7-243">Leírás</span><span class="sxs-lookup"><span data-stu-id="d03e7-243">Description</span></span> |
   | --- | --- |
   |  <span data-ttu-id="d03e7-244">$clusterName</span><span class="sxs-lookup"><span data-stu-id="d03e7-244">$clusterName</span></span> |<span data-ttu-id="d03e7-245">Írja be a HDInsight-fürt nevét, ahol szeretné az alkalmazás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="d03e7-245">Enter the HDInsight cluster name where you want to run the application.</span></span> |
   |  <span data-ttu-id="d03e7-246">$subscriptionID</span><span class="sxs-lookup"><span data-stu-id="d03e7-246">$subscriptionID</span></span> |<span data-ttu-id="d03e7-247">Adja meg az Azure-előfizetés azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="d03e7-247">Enter your Azure subscription ID.</span></span> |
   |  <span data-ttu-id="d03e7-248">$sourceDataPath</span><span class="sxs-lookup"><span data-stu-id="d03e7-248">$sourceDataPath</span></span> |<span data-ttu-id="d03e7-249">Az Azure Blob tárolási helyét, ahol a Hive-lekérdezéseket a adatokat olvassa.</span><span class="sxs-lookup"><span data-stu-id="d03e7-249">The Azure Blob storage location where the Hive queries will read the data from.</span></span> <span data-ttu-id="d03e7-250">Nem kell módosítani a változót.</span><span class="sxs-lookup"><span data-stu-id="d03e7-250">You don't need to change this variable.</span></span> |
   |  <span data-ttu-id="d03e7-251">$outputPath</span><span class="sxs-lookup"><span data-stu-id="d03e7-251">$outputPath</span></span> |<span data-ttu-id="d03e7-252">A Hive-lekérdezések lesz a kimeneti eredmények Azure Blob tárolási helyét.</span><span class="sxs-lookup"><span data-stu-id="d03e7-252">The Azure Blob storage location where the Hive queries will output the results.</span></span> <span data-ttu-id="d03e7-253">Nem kell módosítani a változót.</span><span class="sxs-lookup"><span data-stu-id="d03e7-253">You don't need to change this variable.</span></span> |
   |  <span data-ttu-id="d03e7-254">$hqlScriptFile</span><span class="sxs-lookup"><span data-stu-id="d03e7-254">$hqlScriptFile</span></span> |<span data-ttu-id="d03e7-255">A hely és a HiveQL parancsfájl a fájl nevét.</span><span class="sxs-lookup"><span data-stu-id="d03e7-255">The location and the file name of the HiveQL script file.</span></span> <span data-ttu-id="d03e7-256">Nem kell módosítani a változót.</span><span class="sxs-lookup"><span data-stu-id="d03e7-256">You don't need to change this variable.</span></span> |
4. <span data-ttu-id="d03e7-257">A szkript futtatásához nyomja le az **F5** billentyűt.</span><span class="sxs-lookup"><span data-stu-id="d03e7-257">Press **F5** to run the script.</span></span> <span data-ttu-id="d03e7-258">Ha a probléma megoldásához, problémát tapasztal a sorok, és nyomja le az **F8**.</span><span class="sxs-lookup"><span data-stu-id="d03e7-258">If you run into problems, as a workaround, select all the lines, and then press **F8**.</span></span>
5. <span data-ttu-id="d03e7-259">Ekkor megjelenik a "Complete!"</span><span class="sxs-lookup"><span data-stu-id="d03e7-259">You shall see "Complete!"</span></span> <span data-ttu-id="d03e7-260">a kimeneti végén.</span><span class="sxs-lookup"><span data-stu-id="d03e7-260">at the end of the output.</span></span> <span data-ttu-id="d03e7-261">Piros hibaüzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="d03e7-261">Any error messages will be displayed in red.</span></span>

<span data-ttu-id="d03e7-262">Egy érvényesítési eljárással ellenőrizheti a kimeneti fájl **/tutorials/twitter/twitter.hql**, a Azure Blob-tároló egy Azure Tártallózó vagy az Azure PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="d03e7-262">As a validation procedure, you can check the output file, **/tutorials/twitter/twitter.hql**, on your Azure Blob storage by using an Azure storage explorer or Azure PowerShell.</span></span> <span data-ttu-id="d03e7-263">A Windows PowerShell-parancsfájlpélda a fájlok listázása, lásd: [és a HDInsight együttes használata a Blob storage][hdinsight-storage-powershell].</span><span class="sxs-lookup"><span data-stu-id="d03e7-263">For a sample Windows PowerShell script for listing files, see [Use Blob storage with HDInsight][hdinsight-storage-powershell].</span></span>

## <a name="process-twitter-data-by-using-hive"></a><span data-ttu-id="d03e7-264">Twitter-adatok feldolgozása a Hive eszközzel</span><span class="sxs-lookup"><span data-stu-id="d03e7-264">Process Twitter data by using Hive</span></span>
<span data-ttu-id="d03e7-265">Befejezte a előkészítő munkát.</span><span class="sxs-lookup"><span data-stu-id="d03e7-265">You have finished all the preparation work.</span></span> <span data-ttu-id="d03e7-266">Most a Hive-parancsfájl elindíthat és az eredményeket.</span><span class="sxs-lookup"><span data-stu-id="d03e7-266">Now, you can invoke the Hive script and check the results.</span></span>

### <a name="submit-a-hive-job"></a><span data-ttu-id="d03e7-267">Hive feladat elküldése</span><span class="sxs-lookup"><span data-stu-id="d03e7-267">Submit a Hive job</span></span>
<span data-ttu-id="d03e7-268">A következő Windows PowerShell-parancsfájl segítségével a Hive-parancsfájl futtatása.</span><span class="sxs-lookup"><span data-stu-id="d03e7-268">Use the following Windows PowerShell script to run the Hive script.</span></span> <span data-ttu-id="d03e7-269">Szüksége lesz, ha az első változót.</span><span class="sxs-lookup"><span data-stu-id="d03e7-269">You will need to set the first variable.</span></span>

> [!NOTE]
> <span data-ttu-id="d03e7-270">A Twitter-üzeneteket és a feltöltött HiveQL-parancsfájlt az utolsó két szakaszban, állítsa $hqlScriptFile "/ tutorials/twitter/twitter.hql".</span><span class="sxs-lookup"><span data-stu-id="d03e7-270">To use the tweets and the HiveQL script you uploaded in the last two sections, set $hqlScriptFile to "/tutorials/twitter/twitter.hql".</span></span> <span data-ttu-id="d03e7-271">A gazdarendszerhez meg lettek feltöltve egy nyilvános blob, állítsa $hqlScriptFile "wasb://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql".</span><span class="sxs-lookup"><span data-stu-id="d03e7-271">To use the ones that have been uploaded to a public blob for you, set $hqlScriptFile to "wasb://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql".</span></span>

```powershell
#region variables and constants
$clusterName = "<Existing Azure HDInsight Cluster Name>"
$httpUserName = "admin"
$httpUserPassword = "<HDInsight Cluster HTTP User Password>"

#use one of the following
$hqlScriptFile = "wasb://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql"
$hqlScriptFile = "/tutorials/twitter/twitter.hql"

$statusFolder = "/tutorials/twitter/jobstatus"
#endregion

$myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
$resourceGroupName = $myCluster.ResourceGroup
$defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
$defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value

$defaultBlobContainerName = $myCluster.DefaultStorageContainer

#region - Invoke Hive
Write-Host "Invoke Hive ... " -ForegroundColor Green

# Create the HDInsight cluster
$pw = ConvertTo-SecureString -String $httpUserPassword -AsPlainText -Force
$httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)

Use-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName -HttpCredential $httpCredential
$response = Invoke-AzureRmHDInsightHiveJob -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -file $hqlScriptFile -StatusFolder $statusFolder #-OutVariable $outVariable

Write-Host "Display the standard error log ... " -ForegroundColor Green
$jobID = ($response | Select-String job_ | Select-Object -First 1) -replace ‘\s*$’ -replace ‘.*\s’
Get-AzureRmHDInsightJobOutput -ClusterName $clusterName -JobId $jobID -DefaultContainer $defaultBlobContainerName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -HttpCredential $httpCredential
#endregion
```

### <a name="check-the-results"></a><span data-ttu-id="d03e7-272">Az eredményeket</span><span class="sxs-lookup"><span data-stu-id="d03e7-272">Check the results</span></span>
<span data-ttu-id="d03e7-273">A következő Windows PowerShell-parancsfájl használatával ellenőrizze a Hive-feladat kimeneti.</span><span class="sxs-lookup"><span data-stu-id="d03e7-273">Use the following Windows PowerShell script to check the Hive job output.</span></span> <span data-ttu-id="d03e7-274">Szüksége lesz az első két változók megadása.</span><span class="sxs-lookup"><span data-stu-id="d03e7-274">You will need to set the first two variables.</span></span>

```powershell
#region variables and constants
$clusterName = "<Existing Azure HDInsight Cluster Name>"

$blob = "tutorials/twitter/output/000000_0" # The name of the blob to be downloaded.
#endregion

#region - Create an Azure storage context object
Write-Host "Get the default storage account name and container name based on the cluster name ..." -ForegroundColor Green
$myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
$resourceGroupName = $myCluster.ResourceGroup
$defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
$defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
$defaultBlobContainerName = $myCluster.DefaultStorageContainer

Write-Host "`tThe storage account name is $defaultStorageAccountName." -ForegroundColor Yellow
Write-Host "`tThe blob container name is $defaultBlobContainerName." -ForegroundColor Yellow

Write-Host "Create a context object ... " -ForegroundColor Green
$storageContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey
#endregion

#region - Download blob and display blob
Write-Host "Download the blob ..." -ForegroundColor Green
cd $HOME
Get-AzureStorageBlobContent -Container $defaultBlobContainerName -Blob $blob -Context $storageContext -Force

Write-Host "Display the output ..." -ForegroundColor Green
Write-Host "==================================" -ForegroundColor Green
cat "./$blob"
Write-Host "==================================" -ForegroundColor Green
#end region
```

> [!NOTE]
> A Hive tábla \001 használja, mint a mezőhatárolóval. <span data-ttu-id="d03e7-276">Az elválasztó karaktere nem látható, a kimenetben.</span><span class="sxs-lookup"><span data-stu-id="d03e7-276">The delimiter is not visible in the output.</span></span>

<span data-ttu-id="d03e7-277">Miután az elemzés eredményeinek az Azure Blob storage lettek helyezve, exportálja az adatokat az Azure SQL adatbázis vagy SQL server, az adatok exportálása az Excel Power Query használatával vagy a Hive ODBC-illesztő segítségével csatlakozzon az alkalmazás számára az adatok.</span><span class="sxs-lookup"><span data-stu-id="d03e7-277">After the analysis results have been placed in Azure Blob storage, you can export the data to an Azure SQL database/SQL server, export the data to Excel by using Power Query, or connect your application to the data by using the Hive ODBC Driver.</span></span> <span data-ttu-id="d03e7-278">További információkért lásd: [és a HDInsight együttes használata Sqoop][hdinsight-use-sqoop], [adatelemzés repülési késleltetés HDInsight eszközzel][hdinsight-analyze-flight-delay-data], [HDInsight a Power Query az Excel csatlakozás][hdinsight-power-query], és [csatlakozzon az Excel a Microsoft Hive ODBC-illesztőprogram HDInsight][hdinsight-hive-odbc].</span><span class="sxs-lookup"><span data-stu-id="d03e7-278">For more information, see [Use Sqoop with HDInsight][hdinsight-use-sqoop], [Analyze flight delay data using HDInsight][hdinsight-analyze-flight-delay-data], [Connect Excel to HDInsight with Power Query][hdinsight-power-query], and [Connect Excel to HDInsight with the Microsoft Hive ODBC Driver][hdinsight-hive-odbc].</span></span>

## <a name="next-steps"></a><span data-ttu-id="d03e7-279">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d03e7-279">Next steps</span></span>
<span data-ttu-id="d03e7-280">Ebben az oktatóanyagban úgy találtuk, hogyan kell egy strukturálatlan JSON adatkészlet átalakítása strukturált Hive tábla lekérdezésére, vizsgálatát, és a Twitter adatok elemzése az Azure-on HDInsight használatával.</span><span class="sxs-lookup"><span data-stu-id="d03e7-280">In this tutorial we have seen how to transform an unstructured JSON dataset into a structured Hive table to query, explore, and analyze data from Twitter by using HDInsight on Azure.</span></span> <span data-ttu-id="d03e7-281">További tudnivalókért lásd:</span><span class="sxs-lookup"><span data-stu-id="d03e7-281">To learn more, see:</span></span>

* <span data-ttu-id="d03e7-282">[Első lépései a hdinsight eszközzel][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="d03e7-282">[Get started with HDInsight][hdinsight-get-started]</span></span>
* <span data-ttu-id="d03e7-283">[A HBase a hdinsight eszközben valós idejű Twitter sentiment elemzése][hdinsight-hbase-twitter-sentiment]</span><span class="sxs-lookup"><span data-stu-id="d03e7-283">[Analyze real-time Twitter sentiment with HBase in HDInsight][hdinsight-hbase-twitter-sentiment]</span></span>
* <span data-ttu-id="d03e7-284">[HDInsight eszközzel repülési késleltetés adatok elemzése][hdinsight-analyze-flight-delay-data]</span><span class="sxs-lookup"><span data-stu-id="d03e7-284">[Analyze flight delay data using HDInsight][hdinsight-analyze-flight-delay-data]</span></span>
* <span data-ttu-id="d03e7-285">[Csatlakoztathatja az Excelt a HDInsight a Power Query][hdinsight-power-query]</span><span class="sxs-lookup"><span data-stu-id="d03e7-285">[Connect Excel to HDInsight with Power Query][hdinsight-power-query]</span></span>
* <span data-ttu-id="d03e7-286">[Excel csatlakoztatása a Microsoft Hive ODBC-illesztőprogram HDInsight][hdinsight-hive-odbc]</span><span class="sxs-lookup"><span data-stu-id="d03e7-286">[Connect Excel to HDInsight with the Microsoft Hive ODBC Driver][hdinsight-hive-odbc]</span></span>
* <span data-ttu-id="d03e7-287">[Use Sqoop with HDInsight][hdinsight-use-sqoop]</span><span class="sxs-lookup"><span data-stu-id="d03e7-287">[Use Sqoop with HDInsight][hdinsight-use-sqoop]</span></span>

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[apache-hive-tutorial]: https://cwiki.apache.org/confluence/display/Hive/Tutorial

[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: /powershell/azureps-cmdlets-docs
[powershell-script]: http://technet.microsoft.com/library/ee176961.aspx

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage-powershell]:hdinsight-hadoop-use-blob-storage.md#access-blobs-using-azure-powershell
[hdinsight-analyze-flight-delay-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-hive-odbc]: hdinsight-connect-excel-hive-odbc-driver.md
[hdinsight-hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
