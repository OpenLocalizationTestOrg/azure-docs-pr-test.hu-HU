---
title: Twitter-adatok hadooppal a Hdinsightban - Azure aaaAnalyze |} Microsoft Docs
description: "Ismerje meg, hogyan toouse Hive tooanalyze Twitter Hadoopon adatokat a HDInsight toofind hello egy adott word használat gyakorisága."
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
ms.openlocfilehash: 40c0a1afbc1fff10c070d22a99cd9d32d42f230a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-twitter-data-using-hive-in-hdinsight"></a><span data-ttu-id="b0794-103">Hdinsight Hive eszközzel Twitter-adatok elemzése</span><span class="sxs-lookup"><span data-stu-id="b0794-103">Analyze Twitter data using Hive in HDInsight</span></span>
<span data-ttu-id="b0794-104">Közösségi webhelyek rendszer egyik hello fő vezetői kényszeríti a big data alkalmazására vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="b0794-104">Social websites are one of hello major driving forces for big-data adoption.</span></span> <span data-ttu-id="b0794-105">Nyilvános API-k, például a Twitter helyek által biztosított az hasznos adatforrást ismertetése népszerű trendeket és elemzésére.</span><span class="sxs-lookup"><span data-stu-id="b0794-105">Public APIs provided by sites like Twitter are a useful source of data for analyzing and understanding popular trends.</span></span>
<span data-ttu-id="b0794-106">Ebben az oktatóanyagban, lesz egy Twitter-adatfolyam-API segítségével könnyebben nyerhet Twitter-üzeneteket, és használja Apache Hive az Azure HDInsight tooget küldi hello legtöbb Twitter-üzeneteket, amelyek egy adott szó található Twitter felhasználók listáját.</span><span class="sxs-lookup"><span data-stu-id="b0794-106">In this tutorial, you will get tweets by using a Twitter streaming API, and then use Apache Hive on Azure HDInsight tooget a list of Twitter users who sent hello most tweets that contained a certain word.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b0794-107">hello jelen dokumentumban leírt lépések szükséges egy Windows-alapú HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="b0794-107">hello steps in this document require a Windows-based HDInsight cluster.</span></span> <span data-ttu-id="b0794-108">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="b0794-108">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="b0794-109">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="b0794-109">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span> <span data-ttu-id="b0794-110">Lépéseket adott tooa Linux-alapú fürt, lásd: [Hive HDInsight (Linux) használatával elemezheti Twitter adatok](hdinsight-analyze-twitter-data-linux.md).</span><span class="sxs-lookup"><span data-stu-id="b0794-110">For steps specific tooa Linux-based cluster, see [Analyze Twitter data using Hive in HDInsight (Linux)](hdinsight-analyze-twitter-data-linux.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b0794-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b0794-111">Prerequisites</span></span>
<span data-ttu-id="b0794-112">Ez az oktatóanyag elkezdéséhez hello következő kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="b0794-112">Before you begin this tutorial, you must have hello following:</span></span>

* <span data-ttu-id="b0794-113">**A munkaállomás** az Azure PowerShell telepítése és konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="b0794-113">**A workstation** with Azure PowerShell installed and configured.</span></span>

    <span data-ttu-id="b0794-114">Windows PowerShell-parancsfájlok tooexecute, futtassa a Azure PowerShell rendszergazdaként, és hello végrehajtási házirend beállítása túl*RemoteSigned*.</span><span class="sxs-lookup"><span data-stu-id="b0794-114">tooexecute Windows PowerShell scripts, you must run Azure PowerShell as administrator and set hello execution policy too*RemoteSigned*.</span></span> <span data-ttu-id="b0794-115">Lásd: [futtassa a Windows PowerShell-parancsfájlok][powershell-script].</span><span class="sxs-lookup"><span data-stu-id="b0794-115">See [Run Windows PowerShell scripts][powershell-script].</span></span>

    <span data-ttu-id="b0794-116">Windows PowerShell-parancsfájlok futtatásakor, előtt ellenőrizze, rendelkezik csatlakoztatott tooyour Azure-előfizetés hello a következő parancsmag használatával:</span><span class="sxs-lookup"><span data-stu-id="b0794-116">Before running Windows PowerShell scripts, make sure you are connected tooyour Azure subscription by using hello following cmdlet:</span></span>

    ```powershell
    Login-AzureRmAccount
    ```

    <span data-ttu-id="b0794-117">Ha több Azure-előfizetéssel rendelkezik, használja a következő parancsmag tooset hello előfizetésben hello:</span><span class="sxs-lookup"><span data-stu-id="b0794-117">If you have multiple Azure subscriptions, use hello following cmdlet tooset hello current subscription:</span></span>

    ```powershell
    Select-AzureRmSubscription -SubscriptionID <Azure Subscription ID>
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="b0794-118">A HDInsight-erőforrások Azure Service Managerrel történő kezelésének Azure PowerShell-támogatása **elavult**, így 2017. január 1-től megszűnt.</span><span class="sxs-lookup"><span data-stu-id="b0794-118">Azure PowerShell support for managing HDInsight resources using Azure Service Manager is **deprecated**, and was removed on January 1, 2017.</span></span> <span data-ttu-id="b0794-119">a dokumentum használatát hello új HDInsight-parancsmagokat, amelyek működnek az Azure Resource Manager hello szükséges lépések.</span><span class="sxs-lookup"><span data-stu-id="b0794-119">hello steps in this document use hello new HDInsight cmdlets that work with Azure Resource Manager.</span></span>
    >
    > <span data-ttu-id="b0794-120">Kövesse a lépéseket hello [telepítse és konfigurálja az Azure Powershellt](/powershell/azureps-cmdlets-docs) tooinstall hello legújabb Azure PowerShell verziója.</span><span class="sxs-lookup"><span data-stu-id="b0794-120">Please follow hello steps in [Install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) tooinstall hello latest version of Azure PowerShell.</span></span> <span data-ttu-id="b0794-121">Ha vannak olyan parancsprogramjai, hogy szükség toobe módosított toouse hello új parancsmagok használata Azure Resource Managerrel működő című [áttelepítése tooAzure fejlesztési Resource Manager-alapú HDInsight-fürtök eszközei](hdinsight-hadoop-development-using-azure-resource-manager.md) további információt.</span><span class="sxs-lookup"><span data-stu-id="b0794-121">If you have scripts that need toobe modified toouse hello new cmdlets that work with Azure Resource Manager, see [Migrating tooAzure Resource Manager-based development tools for HDInsight clusters](hdinsight-hadoop-development-using-azure-resource-manager.md) for more information.</span></span>

* <span data-ttu-id="b0794-122">**Egy Azure HDInsight fürt**.</span><span class="sxs-lookup"><span data-stu-id="b0794-122">**An Azure HDInsight cluster**.</span></span> <span data-ttu-id="b0794-123">A fürtök kiépítése útmutatásért lásd: [használatának megkezdésében a HDInsight] [ hdinsight-get-started] vagy [Provision HDInsight clusters][hdinsight-provision].</span><span class="sxs-lookup"><span data-stu-id="b0794-123">For instructions on cluster provisioning, see [Get started using HDInsight][hdinsight-get-started] or [Provision HDInsight clusters][hdinsight-provision].</span></span> <span data-ttu-id="b0794-124">Hello oktatóanyag későbbi részében szüksége lesz hello fürt nevét.</span><span class="sxs-lookup"><span data-stu-id="b0794-124">You will need hello cluster name later in hello tutorial.</span></span>

<span data-ttu-id="b0794-125">hello következő táblázat sorolja fel ebben az oktatóanyagban használt hello fájlok:</span><span class="sxs-lookup"><span data-stu-id="b0794-125">hello following table lists hello files used in this tutorial:</span></span>

| <span data-ttu-id="b0794-126">Fájlok</span><span class="sxs-lookup"><span data-stu-id="b0794-126">Files</span></span> | <span data-ttu-id="b0794-127">Leírás</span><span class="sxs-lookup"><span data-stu-id="b0794-127">Description</span></span> |
| --- | --- |
| <span data-ttu-id="b0794-128">/tutorials/Twitter/Data/tweets.txt</span><span class="sxs-lookup"><span data-stu-id="b0794-128">/tutorials/twitter/data/tweets.txt</span></span> |<span data-ttu-id="b0794-129">hello forrásadatok hello Hive feladat.</span><span class="sxs-lookup"><span data-stu-id="b0794-129">hello source data for hello Hive job.</span></span> |
| <span data-ttu-id="b0794-130">/tutorials/Twitter/output</span><span class="sxs-lookup"><span data-stu-id="b0794-130">/tutorials/twitter/output</span></span> |<span data-ttu-id="b0794-131">kimeneti mappa hello hello Hive feladat.</span><span class="sxs-lookup"><span data-stu-id="b0794-131">hello output folder for hello Hive job.</span></span> <span data-ttu-id="b0794-132">hello Hive feladat kimeneti fájl alapértelmezett neve a **000000_0**.</span><span class="sxs-lookup"><span data-stu-id="b0794-132">hello default Hive job output file name is **000000_0**.</span></span> |
| <span data-ttu-id="b0794-133">tutorials/Twitter/Twitter.hql</span><span class="sxs-lookup"><span data-stu-id="b0794-133">tutorials/twitter/twitter.hql</span></span> |<span data-ttu-id="b0794-134">hello HiveQL-parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="b0794-134">hello HiveQL script file.</span></span> |
| <span data-ttu-id="b0794-135">/tutorials/Twitter/JobStatus</span><span class="sxs-lookup"><span data-stu-id="b0794-135">/tutorials/twitter/jobstatus</span></span> |<span data-ttu-id="b0794-136">hello Hadoop-feladat állapotát.</span><span class="sxs-lookup"><span data-stu-id="b0794-136">hello Hadoop job status.</span></span> |

## <a name="get-twitter-feed"></a><span data-ttu-id="b0794-137">Get Twitter-hírcsatorna</span><span class="sxs-lookup"><span data-stu-id="b0794-137">Get Twitter feed</span></span>
<span data-ttu-id="b0794-138">Ebben az oktatóanyagban hello segítségével [streamelési API-k Twitter][twitter-streaming-api].</span><span class="sxs-lookup"><span data-stu-id="b0794-138">In this tutorial, you will use hello [Twitter streaming APIs][twitter-streaming-api].</span></span> <span data-ttu-id="b0794-139">hello használandó API streaming adott Twitter van [állapotok-vagy szűrőkezelő][twitter-statuses-filter].</span><span class="sxs-lookup"><span data-stu-id="b0794-139">hello specific Twitter streaming API you will use is [statuses/filter][twitter-statuses-filter].</span></span>

> [!NOTE]
> <span data-ttu-id="b0794-140">10 000 Twitter-üzeneteket és hello Hive parancsfájl (hello a következő szakaszban ismertetett) tartalmazó fájlt fel lett töltve, egy nyilvános Blob tárolóban.</span><span class="sxs-lookup"><span data-stu-id="b0794-140">A file containing 10,000 tweets and hello Hive script file (covered in hello next section) have been uploaded in a public Blob container.</span></span> <span data-ttu-id="b0794-141">Ez a szakasz kihagyhatja, ha azt szeretné, hogy toouse hello feltöltött fájlokat.</span><span class="sxs-lookup"><span data-stu-id="b0794-141">You can skip this section if you want toouse hello uploaded files.</span></span>

<span data-ttu-id="b0794-142">[Twitter-üzeneteket adatok](https://dev.twitter.com/docs/platform-objects/tweets) , amely tartalmaz egy összetett beágyazott struktúra hello JavaScript Object Notation (JSON) formátumban tárolja.</span><span class="sxs-lookup"><span data-stu-id="b0794-142">[Tweets data](https://dev.twitter.com/docs/platform-objects/tweets) is stored in hello JavaScript Object Notation (JSON) format that contains a complex nested structure.</span></span> <span data-ttu-id="b0794-143">Ahelyett, hogy hány sornyi kód írása a hagyományos programozási nyelv használatával, alakíthatja át a beágyazott struktúra egy Hive táblába úgy, hogy azt egy Structured Query Language (SQL) lekérdezhetők – például a HiveQL nevű nyelv.</span><span class="sxs-lookup"><span data-stu-id="b0794-143">Instead of writing many lines of code by using a conventional programming language, you can transform this nested structure into a Hive table, so that it can be queried by a Structured Query Language (SQL)-like language called HiveQL.</span></span>

<span data-ttu-id="b0794-144">Twitter OAuth tooprovide engedélyezett hozzáférési tooits API-t használja.</span><span class="sxs-lookup"><span data-stu-id="b0794-144">Twitter uses OAuth tooprovide authorized access tooits API.</span></span> <span data-ttu-id="b0794-145">OAuth olyan hitelesítési protokoll, amely lehetővé teszi a felhasználók tooapprove alkalmazások tooact nevükben nélkül megosztása a jelszavát.</span><span class="sxs-lookup"><span data-stu-id="b0794-145">OAuth is an authentication protocol that allows users tooapprove applications tooact on their behalf without sharing their password.</span></span> <span data-ttu-id="b0794-146">További információ található [oauth.net](http://oauth.net/) vagy kiváló hello [Beginner's Guide tooOAuth](http://hueniverse.com/oauth/) a Hueniverse.</span><span class="sxs-lookup"><span data-stu-id="b0794-146">More information can be found at [oauth.net](http://oauth.net/) or in hello excellent [Beginner's Guide tooOAuth](http://hueniverse.com/oauth/) from Hueniverse.</span></span>

<span data-ttu-id="b0794-147">hello első lépés toouse OAuth toocreate egy új alkalmazást hello Twitter fejlesztői helyen.</span><span class="sxs-lookup"><span data-stu-id="b0794-147">hello first step toouse OAuth is toocreate a new application on hello Twitter Developer site.</span></span>

<span data-ttu-id="b0794-148">**toocreate egy Twitter-alkalmazás**</span><span class="sxs-lookup"><span data-stu-id="b0794-148">**toocreate a Twitter application**</span></span>

1. <span data-ttu-id="b0794-149">Jelentkezzen be a túl[https://apps.twitter.com/](https://apps.twitter.com/).</span><span class="sxs-lookup"><span data-stu-id="b0794-149">Sign in too[https://apps.twitter.com/](https://apps.twitter.com/).</span></span> <span data-ttu-id="b0794-150">Kattintson a hello **feliratkozás most** hivatkozásra, ha egy Twitter-fiók nem rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="b0794-150">Click hello **Sign up now** link if you don't have a Twitter account.</span></span>
2. <span data-ttu-id="b0794-151">Kattintson a **új alkalmazás létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="b0794-151">Click **Create New App**.</span></span>
3. <span data-ttu-id="b0794-152">Adja meg **neve**, **leírás**, **webhely**.</span><span class="sxs-lookup"><span data-stu-id="b0794-152">Enter **Name**, **Description**, **Website**.</span></span> <span data-ttu-id="b0794-153">Biztosíthatja, hogy be egy URL-címet hello **webhely** mező.</span><span class="sxs-lookup"><span data-stu-id="b0794-153">You can make up a URL for hello **Website** field.</span></span> <span data-ttu-id="b0794-154">hello a következő táblázatban néhány példa értékek toouse jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="b0794-154">hello following table shows some sample values toouse:</span></span>

   | <span data-ttu-id="b0794-155">Mező</span><span class="sxs-lookup"><span data-stu-id="b0794-155">Field</span></span> | <span data-ttu-id="b0794-156">Érték</span><span class="sxs-lookup"><span data-stu-id="b0794-156">Value</span></span> |
   | --- | --- |
   |  <span data-ttu-id="b0794-157">Név</span><span class="sxs-lookup"><span data-stu-id="b0794-157">Name</span></span> |<span data-ttu-id="b0794-158">MyHDInsightApp</span><span class="sxs-lookup"><span data-stu-id="b0794-158">MyHDInsightApp</span></span> |
   |  <span data-ttu-id="b0794-159">Leírás</span><span class="sxs-lookup"><span data-stu-id="b0794-159">Description</span></span> |<span data-ttu-id="b0794-160">MyHDInsightApp</span><span class="sxs-lookup"><span data-stu-id="b0794-160">MyHDInsightApp</span></span> |
   |  <span data-ttu-id="b0794-161">Honlap</span><span class="sxs-lookup"><span data-stu-id="b0794-161">Website</span></span> |<span data-ttu-id="b0794-162">http://www.myhdinsightapp.com</span><span class="sxs-lookup"><span data-stu-id="b0794-162">http://www.myhdinsightapp.com</span></span> |
4. <span data-ttu-id="b0794-163">Ellenőrizze **Igen, elfogadom**, és kattintson a **az Twitter-alkalmazás létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="b0794-163">Check **Yes, I agree**, and then click **Create your Twitter application**.</span></span>
5. <span data-ttu-id="b0794-164">Kattintson a hello **engedélyek** külön-külön hello alapértelmezett engedély **csak olvasható**.</span><span class="sxs-lookup"><span data-stu-id="b0794-164">Click hello **Permissions** tab. hello default permission is **Read only**.</span></span> <span data-ttu-id="b0794-165">Ez a megfelelő ehhez az oktatóanyaghoz.</span><span class="sxs-lookup"><span data-stu-id="b0794-165">This is sufficient for this tutorial.</span></span>
6. <span data-ttu-id="b0794-166">Kattintson a hello **kulcsok és a hozzáférési jogkivonatok** fülre.</span><span class="sxs-lookup"><span data-stu-id="b0794-166">Click hello **Keys and Access Tokens** tab.</span></span>
7. <span data-ttu-id="b0794-167">Kattintson a **a hozzáférési jogkivonat létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="b0794-167">Click **Create my access token**.</span></span>
8. <span data-ttu-id="b0794-168">Kattintson a **teszt OAuth** hello oldal jobb felső sarkában hello.</span><span class="sxs-lookup"><span data-stu-id="b0794-168">Click **Test OAuth** in hello upper-right corner of hello page.</span></span>
9. <span data-ttu-id="b0794-169">Írja le **kulcsa**, **felhasználói titok**, **hozzáférési jogkivonat**, és **Access token titkos**.</span><span class="sxs-lookup"><span data-stu-id="b0794-169">Write down **consumer key**, **Consumer secret**, **Access token**, and **Access token secret**.</span></span> <span data-ttu-id="b0794-170">Hello oktatóanyag későbbi részében szüksége lesz hello értékekre.</span><span class="sxs-lookup"><span data-stu-id="b0794-170">You will need hello values later in hello tutorial.</span></span>

<span data-ttu-id="b0794-171">Ebben az oktatóanyagban a Windows PowerShell toomake hello webszolgáltatás-hívás fog használni.</span><span class="sxs-lookup"><span data-stu-id="b0794-171">In this tutorial, you will use Windows PowerShell toomake hello web service call.</span></span> <span data-ttu-id="b0794-172">.NET C# mintát, lásd: [elemzése a HBase a hdinsight eszközben valós idejű Twitter sentiment][hdinsight-hbase-twitter-sentiment].</span><span class="sxs-lookup"><span data-stu-id="b0794-172">For a .NET C# sample, see [Analyze real-time Twitter sentiment with HBase in HDInsight][hdinsight-hbase-twitter-sentiment].</span></span> <span data-ttu-id="b0794-173">hello egyéb népszerű eszköz toomake webszolgáltatás-hívás van [ *Curl*][curl].</span><span class="sxs-lookup"><span data-stu-id="b0794-173">hello other popular tool toomake web service calls is [*Curl*][curl].</span></span> <span data-ttu-id="b0794-174">Curl letölthető [Itt][curl-download].</span><span class="sxs-lookup"><span data-stu-id="b0794-174">Curl can be downloaded from [here][curl-download].</span></span>

> [!NOTE]
> <span data-ttu-id="b0794-175">Ha a Windows hello curl-parancsot használja, idézőjelek közé foglalt helyett használjon szimpla idézőjelben az hello beállítás értékét.</span><span class="sxs-lookup"><span data-stu-id="b0794-175">When you use hello curl command in Windows, use double quotes instead of single quotes for hello option values.</span></span>

<span data-ttu-id="b0794-176">**tooget Twitter-üzenetek**</span><span class="sxs-lookup"><span data-stu-id="b0794-176">**tooget tweets**</span></span>

1. <span data-ttu-id="b0794-177">Nyissa meg a Windows PowerShell integrált parancsfájlkezelési környezet (ISE) hello.</span><span class="sxs-lookup"><span data-stu-id="b0794-177">Open hello Windows PowerShell Integrated Scripting Environment (ISE).</span></span> <span data-ttu-id="b0794-178">(Hello Windows 8 kezdőképernyőn írja be a **PowerShell_ISE** majd **Windows PowerShell ISE**.</span><span class="sxs-lookup"><span data-stu-id="b0794-178">(On hello Windows 8 Start screen, type **PowerShell_ISE** and then click **Windows PowerShell ISE**.</span></span> <span data-ttu-id="b0794-179">Lásd: [indítsa el a Windows PowerShell használatával Windows 8 és Windows][powershell-start].)</span><span class="sxs-lookup"><span data-stu-id="b0794-179">See [Start Windows PowerShell on Windows 8 and Windows][powershell-start].)</span></span>
2. <span data-ttu-id="b0794-180">Másolja a következő parancsfájl hello parancsfájl ablaktáblára hello:</span><span class="sxs-lookup"><span data-stu-id="b0794-180">Copy hello following script into hello script pane:</span></span>

    ```powershell
    #region - variables and constants
    $clusterName = "<HDInsightClusterName>" # Enter hello HDInsight cluster name

    # Enter hello OAuth information for your Twitter application
    $oauth_consumer_key = "<TwitterAppConsumerKey>";
    $oauth_consumer_secret = "<TwitterAppConsumerSecret>";
    $oauth_token = "<TwitterAppAccessToken>";
    $oauth_token_secret = "<TwitterAppAccessTokenSecret>";

    $destBlobName = "tutorials/twitter/data/tweets.txt" # This script saves hello tweets into this blob.

    $trackString = "Azure, Cloud, HDInsight" # This script gets hello tweets containing these keywords.
    $track = [System.Uri]::EscapeDataString($trackString);
    $lineMax = 10000  # hello script will get this number of tweets. It is about 3 minutes every 100 lines.
    #endregion

    #region - Connect tooAzure subscription
    Write-Host "`nConnecting tooyour Azure subscription ..." -ForegroundColor Green
    Login-AzureRmAccount
    #endregion

    #region - Create a block blob object for writing tweets into Blob storage
    Write-Host "Get hello default storage account name and Blob container name using hello cluster name ..." -ForegroundColor Green
    $myCluster = Get-AzureRmHDInsightCluster -Name $clusterName
    $resourceGroupName = $myCluster.ResourceGroup
    $storageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
    $containerName = $myCluster.DefaultStorageContainer
    Write-Host "`tThe storage account name is $storageAccountName." -ForegroundColor Yellow
    Write-Host "`tThe blob container name is $containerName." -ForegroundColor Yellow

    Write-Host "Define hello Azure storage connection string ..." -ForegroundColor Green
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

    #region - Write tweets tooBlob storage
    Write-Host "Write toohello destination blob ..." -ForegroundColor Green
    $writeStream.Flush()
    $memStream.Seek(0, "Begin")
    $destBlob.UploadFromStream($memStream)

    $sReader.close()
    #endregion

    Write-Host "Completed!" -ForegroundColor Green
    ```

3. <span data-ttu-id="b0794-181">Hello első öt tooeight változók megadása hello parancsfájlban:</span><span class="sxs-lookup"><span data-stu-id="b0794-181">Set hello first five tooeight variables in hello script:</span></span>

    <span data-ttu-id="b0794-182">Változó</span><span class="sxs-lookup"><span data-stu-id="b0794-182">Variable</span></span>|<span data-ttu-id="b0794-183">Leírás</span><span class="sxs-lookup"><span data-stu-id="b0794-183">Description</span></span>
    ---|---
    <span data-ttu-id="b0794-184">$clusterName</span><span class="sxs-lookup"><span data-stu-id="b0794-184">$clusterName</span></span>|<span data-ttu-id="b0794-185">Ez az hello nevét, ahol szeretné toorun hello alkalmazás hello HDInsight-fürt.</span><span class="sxs-lookup"><span data-stu-id="b0794-185">This is hello name of hello HDInsight cluster where you want toorun hello application.</span></span>
    <span data-ttu-id="b0794-186">$oauth_consumer_key</span><span class="sxs-lookup"><span data-stu-id="b0794-186">$oauth_consumer_key</span></span>|<span data-ttu-id="b0794-187">Ez a hello Twitter alkalmazás **kulcsa** megírt korábbi hello Twitter-alkalmazás létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="b0794-187">This is hello Twitter application **consumer key** you wrote down earlier when you created hello Twitter application.</span></span>
    <span data-ttu-id="b0794-188">$oauth_consumer_secret</span><span class="sxs-lookup"><span data-stu-id="b0794-188">$oauth_consumer_secret</span></span>|<span data-ttu-id="b0794-189">Ez a hello Twitter alkalmazás **felhasználói titok** korábban megírt.</span><span class="sxs-lookup"><span data-stu-id="b0794-189">This is hello Twitter application **consumer secret** you wrote down earlier.</span></span>
    <span data-ttu-id="b0794-190">$oauth_token</span><span class="sxs-lookup"><span data-stu-id="b0794-190">$oauth_token</span></span>|<span data-ttu-id="b0794-191">Ez a hello Twitter alkalmazás **hozzáférési jogkivonat** korábban megírt.</span><span class="sxs-lookup"><span data-stu-id="b0794-191">This is hello Twitter application **access token** you wrote down earlier.</span></span>
    <span data-ttu-id="b0794-192">$oauth_token_secret</span><span class="sxs-lookup"><span data-stu-id="b0794-192">$oauth_token_secret</span></span>|<span data-ttu-id="b0794-193">Ez a hello Twitter alkalmazás **access token titkos** korábban megírt.</span><span class="sxs-lookup"><span data-stu-id="b0794-193">This is hello Twitter application **access token secret** you wrote down earlier.</span></span>
    <span data-ttu-id="b0794-194">$destBlobName</span><span class="sxs-lookup"><span data-stu-id="b0794-194">$destBlobName</span></span>|<span data-ttu-id="b0794-195">Ez az hello kimeneti blob neve.</span><span class="sxs-lookup"><span data-stu-id="b0794-195">This is hello output blob name.</span></span> <span data-ttu-id="b0794-196">hello alapértelmezett értéke **tutorials/twitter/data/tweets.txt**.</span><span class="sxs-lookup"><span data-stu-id="b0794-196">hello default value is **tutorials/twitter/data/tweets.txt**.</span></span> <span data-ttu-id="b0794-197">Ha megváltoztatja hello alapértelmezett értékét, ennek megfelelően kell tooupdate hello Windows PowerShell-parancsfájlok.</span><span class="sxs-lookup"><span data-stu-id="b0794-197">If you change hello default value, you will need tooupdate hello Windows PowerShell scripts accordingly.</span></span>
    <span data-ttu-id="b0794-198">$trackString</span><span class="sxs-lookup"><span data-stu-id="b0794-198">$trackString</span></span>|<span data-ttu-id="b0794-199">hello webszolgáltatás visszaadható kapcsolódó toothese kulcsszavak Twitter-üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="b0794-199">hello web service will return tweets related toothese keywords.</span></span> <span data-ttu-id="b0794-200">hello alapértelmezett értéke **Azure-felhő, HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="b0794-200">hello default value is **Azure, Cloud, HDInsight**.</span></span> <span data-ttu-id="b0794-201">Hello alapértelmezett értékének módosításához a Windows PowerShell-parancsfájlok hello ennek megfelelően frissíti.</span><span class="sxs-lookup"><span data-stu-id="b0794-201">If you change hello default value, you will update hello Windows PowerShell scripts accordingly.</span></span>
    <span data-ttu-id="b0794-202">$lineMax</span><span class="sxs-lookup"><span data-stu-id="b0794-202">$lineMax</span></span>|<span data-ttu-id="b0794-203">hello érték határozza meg, hány Twitter-üzeneteket hello parancsfájl olvasását.</span><span class="sxs-lookup"><span data-stu-id="b0794-203">hello value determines how many tweets hello script will read.</span></span> <span data-ttu-id="b0794-204">Tart körülbelül 3 percig tooread 100 Twitter-üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="b0794-204">It takes about three minutes tooread 100 tweets.</span></span> <span data-ttu-id="b0794-205">Beállíthatja a korábbinál több, de további időt toodownload fog igénybe venni.</span><span class="sxs-lookup"><span data-stu-id="b0794-205">You can set a larger number, but it will take more time toodownload.</span></span>

1. <span data-ttu-id="b0794-206">Nyomja le az **F5** toorun hello parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="b0794-206">Press **F5** toorun hello script.</span></span> <span data-ttu-id="b0794-207">Ha a probléma megoldásához, problémát tapasztal összes hello sort, és nyomja le az **F8**.</span><span class="sxs-lookup"><span data-stu-id="b0794-207">If you run into problems, as a workaround, select all hello lines, and then press **F8**.</span></span>
2. <span data-ttu-id="b0794-208">Ekkor megjelenik a "Complete!"</span><span class="sxs-lookup"><span data-stu-id="b0794-208">You shall see "Complete!"</span></span> <span data-ttu-id="b0794-209">hello kimeneti hello végén.</span><span class="sxs-lookup"><span data-stu-id="b0794-209">at hello end of hello output.</span></span> <span data-ttu-id="b0794-210">Piros hibaüzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b0794-210">Any error messages will be displayed in red.</span></span>

<span data-ttu-id="b0794-211">Egy érvényesítési eljárással ellenőrizheti a hello kimeneti fájl, **/tutorials/twitter/data/tweets.txt**, a Azure Blob-tároló egy Azure Tártallózó vagy az Azure PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="b0794-211">As a validation procedure, you can check hello output file, **/tutorials/twitter/data/tweets.txt**, on your Azure Blob storage by using an Azure storage explorer or Azure PowerShell.</span></span> <span data-ttu-id="b0794-212">A Windows PowerShell-parancsfájlpélda a fájlok listázása, lásd: [és a HDInsight együttes használata a Blob storage][hdinsight-storage-powershell].</span><span class="sxs-lookup"><span data-stu-id="b0794-212">For a sample Windows PowerShell script for listing files, see [Use Blob storage with HDInsight][hdinsight-storage-powershell].</span></span>

## <a name="create-hiveql-script"></a><span data-ttu-id="b0794-213">Hozzon létre HiveQL-parancsfájlt</span><span class="sxs-lookup"><span data-stu-id="b0794-213">Create HiveQL script</span></span>
<span data-ttu-id="b0794-214">Az Azure PowerShell használatával is futtathatja több HiveQL utasítást egy időben, vagy csomag hello HiveQL utasítás egy parancsfájl-fájlba.</span><span class="sxs-lookup"><span data-stu-id="b0794-214">Using Azure PowerShell, you can run multiple HiveQL statements one at a time, or package hello HiveQL statement into a script file.</span></span> <span data-ttu-id="b0794-215">Ebben az oktatóanyagban létrehoz egy HiveQL-parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="b0794-215">In this tutorial, you will create a HiveQL script.</span></span> <span data-ttu-id="b0794-216">hello parancsfájl feltöltött tooAzure Blob Storage tárolóban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="b0794-216">hello script file must be uploaded tooAzure Blob storage.</span></span> <span data-ttu-id="b0794-217">A következő szakaszban hello Azure PowerShell használatával fog futni hello parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="b0794-217">In hello next section, you will run hello script file by using Azure PowerShell.</span></span>

> [!NOTE]
> <span data-ttu-id="b0794-218">hello Hive parancsfájl és 10 000 Twitter-üzeneteket tartalmazó fájl fel lett töltve egy nyilvános Blob tárolóban.</span><span class="sxs-lookup"><span data-stu-id="b0794-218">hello Hive script file and a file containing 10,000 tweets have been uploaded in a public Blob container.</span></span> <span data-ttu-id="b0794-219">Ez a szakasz kihagyhatja, ha azt szeretné, hogy toouse hello feltöltött fájlokat.</span><span class="sxs-lookup"><span data-stu-id="b0794-219">You can skip this section if you want toouse hello uploaded files.</span></span>

<span data-ttu-id="b0794-220">hello HiveQL-parancsfájlt hello következőket hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="b0794-220">hello HiveQL script will perform hello following:</span></span>

1. <span data-ttu-id="b0794-221">**Hello tweets_raw table DROP** hello tábla már létezik.</span><span class="sxs-lookup"><span data-stu-id="b0794-221">**Drop hello tweets_raw table** in case hello table already exists.</span></span>
2. <span data-ttu-id="b0794-222">**Hello tweets_raw Hive tábla létrehozásához**.</span><span class="sxs-lookup"><span data-stu-id="b0794-222">**Create hello tweets_raw Hive table**.</span></span> <span data-ttu-id="b0794-223">Az ideiglenes strukturált Hive tábla további kivonat hello adatokat tároló, átalakítási és betöltési (ETL) feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="b0794-223">This temporary Hive structured table holds hello data for further extract, transform, and load (ETL) processing.</span></span> <span data-ttu-id="b0794-224">A partíciókon információkért lásd: [Hive oktatóanyag][apache-hive-tutorial].</span><span class="sxs-lookup"><span data-stu-id="b0794-224">For information on partitions, see [Hive tutorial][apache-hive-tutorial].</span></span>
3. <span data-ttu-id="b0794-225">**Adatok betöltése** forrásmappából hello, /tutorials/twitter/data.</span><span class="sxs-lookup"><span data-stu-id="b0794-225">**Load data** from hello source folder, /tutorials/twitter/data.</span></span> <span data-ttu-id="b0794-226">ideiglenes Hive tábla struktúrába most átalakítása hello nagy Twitter-üzeneteket adatkészlet beágyazott JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="b0794-226">hello large tweets dataset in nested JSON format has now been transformed into a temporary Hive table structure.</span></span>
4. <span data-ttu-id="b0794-227">**Közvetlen hello Twitter-üzeneteket tábla** hello tábla már létezik.</span><span class="sxs-lookup"><span data-stu-id="b0794-227">**Drop hello tweets table** in case hello table already exists.</span></span>
5. <span data-ttu-id="b0794-228">**Hello Twitter-üzeneteket tábla létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="b0794-228">**Create hello tweets table**.</span></span> <span data-ttu-id="b0794-229">Hive eszközzel kérdezheti hello Twitter-üzeneteket dataset ellen, meg kell toorun egy másik ETL-folyamat.</span><span class="sxs-lookup"><span data-stu-id="b0794-229">Before you can query against hello tweets dataset by using Hive, you need toorun another ETL process.</span></span> <span data-ttu-id="b0794-230">Az ETL-folyamat hello "twitter_raw" táblában tárolt adatok hello részletesebb tábla séma határozza meg.</span><span class="sxs-lookup"><span data-stu-id="b0794-230">This ETL process defines a more detailed table schema for hello data that you have stored in hello "twitter_raw" table.</span></span>
6. <span data-ttu-id="b0794-231">**Felülírás táblázat beszúrása**.</span><span class="sxs-lookup"><span data-stu-id="b0794-231">**Insert overwrite table**.</span></span> <span data-ttu-id="b0794-232">Az összetett Hive parancsfájl hello Hadoop-fürt lesz indítsa hosszú MapReduce-feladatok készlete.</span><span class="sxs-lookup"><span data-stu-id="b0794-232">This complex Hive script will kick off a set of long MapReduce jobs by hello Hadoop cluster.</span></span> <span data-ttu-id="b0794-233">A fürt a DataSet adatkészlet és hello méretétől függően ez eltarthat körülbelül 10 perc.</span><span class="sxs-lookup"><span data-stu-id="b0794-233">Depending on your dataset and hello size of your cluster, this could take about 10 minutes.</span></span>
7. <span data-ttu-id="b0794-234">**INSERT felülírása directory**.</span><span class="sxs-lookup"><span data-stu-id="b0794-234">**Insert overwrite directory**.</span></span> <span data-ttu-id="b0794-235">Az alábbi lekérdezés és a kimeneti hello dataset tooa fájl futtatása.</span><span class="sxs-lookup"><span data-stu-id="b0794-235">Run a query and output hello dataset tooa file.</span></span> <span data-ttu-id="b0794-236">Ez a lekérdezés a Twitter felhasználók, akik a legtöbb Twitter-üzeneteket hello word "Azure" lévő küldött listáját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="b0794-236">This query will return a list of Twitter users who sent most tweets that contained hello word "Azure".</span></span>

<span data-ttu-id="b0794-237">**a Hive toocreate parancsfájlt, és töltse fel tooAzure**</span><span class="sxs-lookup"><span data-stu-id="b0794-237">**toocreate a Hive script and upload it tooAzure**</span></span>

1. <span data-ttu-id="b0794-238">Nyissa meg a Windows PowerShell ISE.</span><span class="sxs-lookup"><span data-stu-id="b0794-238">Open Windows PowerShell ISE.</span></span>
2. <span data-ttu-id="b0794-239">Másolja a következő parancsfájl hello parancsfájl ablaktáblára hello:</span><span class="sxs-lookup"><span data-stu-id="b0794-239">Copy hello following script into hello script pane:</span></span>

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

    #region - Connect tooAzure subscription
    Write-Host "`nConnecting tooyour Azure subscription ..." -ForegroundColor Green

    Try{
        Get-AzureRmSubscription
    }
    Catch{
        Login-AzureRmAccount
    }

    Select-AzureRmSubscription -SubscriptionId $subscriptionID

    #endregion

    #region - Create a block blob object for writing hello Hive script file
    Write-Host "Get hello default storage account name and container name based on hello cluster name ..." -ForegroundColor Green
    $myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $myCluster.ResourceGroup
    $defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
    $defaultBlobContainerName = $myCluster.DefaultStorageContainer
    Write-Host "`tThe storage account name is $defaultStorageAccountName." -ForegroundColor Yellow
    Write-Host "`tThe blob container name is $defaultBlobContainerName." -ForegroundColor Yellow

    Write-Host "Define hello connection string ..." -ForegroundColor Green
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$defaultStorageAccountName;AccountKey=$defaultStorageAccountKey"

    Write-Host "Create block blob objects referencing hello hql script file" -ForegroundColor Green
    $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
    $storageClient = $storageAccount.CreateCloudBlobClient();
    $storageContainer = $storageClient.GetContainerReference($defaultBlobContainerName)
    $hqlScriptBlob = $storageContainer.GetBlockBlobReference($hqlScriptFile)

    Write-Host "Define a MemoryStream and a StreamWriter for writing ... " -ForegroundColor Green
    $memStream = New-Object System.IO.MemoryStream
    $writeStream = New-Object System.IO.StreamWriter $memStream
    $writeStream.Writeline($hqlStatements)
    #endregion

    #region - Write hello Hive script file tooBlob storage
    Write-Host "Write toohello destination blob ... " -ForegroundColor Green
    $writeStream.Flush()
    $memStream.Seek(0, "Begin")
    $hqlScriptBlob.UploadFromStream($memStream)
    #endregion

    Write-Host "Completed!" -ForegroundColor Green
    ```

3. <span data-ttu-id="b0794-240">Hello első két változók megadása hello parancsfájlban:</span><span class="sxs-lookup"><span data-stu-id="b0794-240">Set hello first two variables in hello script:</span></span>

   | <span data-ttu-id="b0794-241">Változó</span><span class="sxs-lookup"><span data-stu-id="b0794-241">Variable</span></span> | <span data-ttu-id="b0794-242">Leírás</span><span class="sxs-lookup"><span data-stu-id="b0794-242">Description</span></span> |
   | --- | --- |
   |  <span data-ttu-id="b0794-243">$clusterName</span><span class="sxs-lookup"><span data-stu-id="b0794-243">$clusterName</span></span> |<span data-ttu-id="b0794-244">Adja meg hello HDInsight-fürt nevét, ahol szeretné toorun hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="b0794-244">Enter hello HDInsight cluster name where you want toorun hello application.</span></span> |
   |  <span data-ttu-id="b0794-245">$subscriptionID</span><span class="sxs-lookup"><span data-stu-id="b0794-245">$subscriptionID</span></span> |<span data-ttu-id="b0794-246">Adja meg az Azure-előfizetés azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="b0794-246">Enter your Azure subscription ID.</span></span> |
   |  <span data-ttu-id="b0794-247">$sourceDataPath</span><span class="sxs-lookup"><span data-stu-id="b0794-247">$sourceDataPath</span></span> |<span data-ttu-id="b0794-248">hello Azure Blob tárolási helye, ahol hello Hive-lekérdezések hello adatokat olvassa.</span><span class="sxs-lookup"><span data-stu-id="b0794-248">hello Azure Blob storage location where hello Hive queries will read hello data from.</span></span> <span data-ttu-id="b0794-249">Ez a változó toochange nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="b0794-249">You don't need toochange this variable.</span></span> |
   |  <span data-ttu-id="b0794-250">$outputPath</span><span class="sxs-lookup"><span data-stu-id="b0794-250">$outputPath</span></span> |<span data-ttu-id="b0794-251">hello hello Hive-lekérdezések lesz a kimeneti eredmények hello Azure Blob tárolási helyét.</span><span class="sxs-lookup"><span data-stu-id="b0794-251">hello Azure Blob storage location where hello Hive queries will output hello results.</span></span> <span data-ttu-id="b0794-252">Ez a változó toochange nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="b0794-252">You don't need toochange this variable.</span></span> |
   |  <span data-ttu-id="b0794-253">$hqlScriptFile</span><span class="sxs-lookup"><span data-stu-id="b0794-253">$hqlScriptFile</span></span> |<span data-ttu-id="b0794-254">hello helye és fájlneve hello hello HiveQL-parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="b0794-254">hello location and hello file name of hello HiveQL script file.</span></span> <span data-ttu-id="b0794-255">Ez a változó toochange nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="b0794-255">You don't need toochange this variable.</span></span> |
4. <span data-ttu-id="b0794-256">Nyomja le az **F5** toorun hello parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="b0794-256">Press **F5** toorun hello script.</span></span> <span data-ttu-id="b0794-257">Ha a probléma megoldásához, problémát tapasztal összes hello sort, és nyomja le az **F8**.</span><span class="sxs-lookup"><span data-stu-id="b0794-257">If you run into problems, as a workaround, select all hello lines, and then press **F8**.</span></span>
5. <span data-ttu-id="b0794-258">Ekkor megjelenik a "Complete!"</span><span class="sxs-lookup"><span data-stu-id="b0794-258">You shall see "Complete!"</span></span> <span data-ttu-id="b0794-259">hello kimeneti hello végén.</span><span class="sxs-lookup"><span data-stu-id="b0794-259">at hello end of hello output.</span></span> <span data-ttu-id="b0794-260">Piros hibaüzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b0794-260">Any error messages will be displayed in red.</span></span>

<span data-ttu-id="b0794-261">Egy érvényesítési eljárással ellenőrizheti a hello kimeneti fájl, **/tutorials/twitter/twitter.hql**, a Azure Blob-tároló egy Azure Tártallózó vagy az Azure PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="b0794-261">As a validation procedure, you can check hello output file, **/tutorials/twitter/twitter.hql**, on your Azure Blob storage by using an Azure storage explorer or Azure PowerShell.</span></span> <span data-ttu-id="b0794-262">A Windows PowerShell-parancsfájlpélda a fájlok listázása, lásd: [és a HDInsight együttes használata a Blob storage][hdinsight-storage-powershell].</span><span class="sxs-lookup"><span data-stu-id="b0794-262">For a sample Windows PowerShell script for listing files, see [Use Blob storage with HDInsight][hdinsight-storage-powershell].</span></span>

## <a name="process-twitter-data-by-using-hive"></a><span data-ttu-id="b0794-263">Twitter-adatok feldolgozása a Hive eszközzel</span><span class="sxs-lookup"><span data-stu-id="b0794-263">Process Twitter data by using Hive</span></span>
<span data-ttu-id="b0794-264">Befejezte az összes hello előkészítő munkát.</span><span class="sxs-lookup"><span data-stu-id="b0794-264">You have finished all hello preparation work.</span></span> <span data-ttu-id="b0794-265">Most hello Hive parancsfájl elindíthat és hello eredményeket.</span><span class="sxs-lookup"><span data-stu-id="b0794-265">Now, you can invoke hello Hive script and check hello results.</span></span>

### <a name="submit-a-hive-job"></a><span data-ttu-id="b0794-266">Hive feladat elküldése</span><span class="sxs-lookup"><span data-stu-id="b0794-266">Submit a Hive job</span></span>
<span data-ttu-id="b0794-267">A következő Windows PowerShell parancsfájl toorun hello Hive parancsfájl hello használata.</span><span class="sxs-lookup"><span data-stu-id="b0794-267">Use hello following Windows PowerShell script toorun hello Hive script.</span></span> <span data-ttu-id="b0794-268">Szüksége lesz tooset hello első változó.</span><span class="sxs-lookup"><span data-stu-id="b0794-268">You will need tooset hello first variable.</span></span>

> [!NOTE]
> <span data-ttu-id="b0794-269">toouse hello Twitter-üzeneteket és a HiveQL-parancsfájlt hello utolsó két szakaszokban set $hqlScriptFile too"/tutorials/twitter/twitter.hql feltöltött hello".</span><span class="sxs-lookup"><span data-stu-id="b0794-269">toouse hello tweets and hello HiveQL script you uploaded in hello last two sections, set $hqlScriptFile too"/tutorials/twitter/twitter.hql".</span></span> <span data-ttu-id="b0794-270">toouse hello néhányat a meglévők közül, amelyek lettek feltöltve tooa nyilvános blob meg, állítsa be a $hqlScriptFile túl"wasb://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql".</span><span class="sxs-lookup"><span data-stu-id="b0794-270">toouse hello ones that have been uploaded tooa public blob for you, set $hqlScriptFile too"wasb://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql".</span></span>

```powershell
#region variables and constants
$clusterName = "<Existing Azure HDInsight Cluster Name>"
$httpUserName = "admin"
$httpUserPassword = "<HDInsight Cluster HTTP User Password>"

#use one of hello following
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

# Create hello HDInsight cluster
$pw = ConvertTo-SecureString -String $httpUserPassword -AsPlainText -Force
$httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)

Use-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName -HttpCredential $httpCredential
$response = Invoke-AzureRmHDInsightHiveJob -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -file $hqlScriptFile -StatusFolder $statusFolder #-OutVariable $outVariable

Write-Host "Display hello standard error log ... " -ForegroundColor Green
$jobID = ($response | Select-String job_ | Select-Object -First 1) -replace ‘\s*$’ -replace ‘.*\s’
Get-AzureRmHDInsightJobOutput -ClusterName $clusterName -JobId $jobID -DefaultContainer $defaultBlobContainerName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -HttpCredential $httpCredential
#endregion
```

### <a name="check-hello-results"></a><span data-ttu-id="b0794-271">Hello ellenőrzésének az eredménye</span><span class="sxs-lookup"><span data-stu-id="b0794-271">Check hello results</span></span>
<span data-ttu-id="b0794-272">A következő Windows PowerShell parancsfájl toocheck hello Hive-feladat kimeneti hello használata.</span><span class="sxs-lookup"><span data-stu-id="b0794-272">Use hello following Windows PowerShell script toocheck hello Hive job output.</span></span> <span data-ttu-id="b0794-273">Tooset hello első két változót kell.</span><span class="sxs-lookup"><span data-stu-id="b0794-273">You will need tooset hello first two variables.</span></span>

```powershell
#region variables and constants
$clusterName = "<Existing Azure HDInsight Cluster Name>"

$blob = "tutorials/twitter/output/000000_0" # hello name of hello blob toobe downloaded.
#endregion

#region - Create an Azure storage context object
Write-Host "Get hello default storage account name and container name based on hello cluster name ..." -ForegroundColor Green
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
Write-Host "Download hello blob ..." -ForegroundColor Green
cd $HOME
Get-AzureStorageBlobContent -Container $defaultBlobContainerName -Blob $blob -Context $storageContext -Force

Write-Host "Display hello output ..." -ForegroundColor Green
Write-Host "==================================" -ForegroundColor Green
cat "./$blob"
Write-Host "==================================" -ForegroundColor Green
#end region
```

> [!NOTE]
> hello Hive tábla mezőhatároló hello \001 használja. <span data-ttu-id="b0794-275">hello elválasztó karaktere nem hello kimenet látható.</span><span class="sxs-lookup"><span data-stu-id="b0794-275">hello delimiter is not visible in hello output.</span></span>

<span data-ttu-id="b0794-276">Hello elemzési adatokat az Azure Blob storage lettek helyezve, miután hello adatok tooan Azure SQL adatbázis vagy SQL server exportálása, hello adatok tooExcel exportálja a Power Query használatával vagy az alkalmazásadatok toohello csatlakozás hello Hive ODBC-illesztő segítségével.</span><span class="sxs-lookup"><span data-stu-id="b0794-276">After hello analysis results have been placed in Azure Blob storage, you can export hello data tooan Azure SQL database/SQL server, export hello data tooExcel by using Power Query, or connect your application toohello data by using hello Hive ODBC Driver.</span></span> <span data-ttu-id="b0794-277">További információkért lásd: [és a HDInsight együttes használata Sqoop][hdinsight-use-sqoop], [adatelemzés repülési késleltetés HDInsight eszközzel][hdinsight-analyze-flight-delay-data], [ Power Query az Excel tooHDInsight csatlakozás][hdinsight-power-query], és [kapcsolódás Excel tooHDInsight a Microsoft Hive ODBC-illesztő hello][hdinsight-hive-odbc].</span><span class="sxs-lookup"><span data-stu-id="b0794-277">For more information, see [Use Sqoop with HDInsight][hdinsight-use-sqoop], [Analyze flight delay data using HDInsight][hdinsight-analyze-flight-delay-data], [Connect Excel tooHDInsight with Power Query][hdinsight-power-query], and [Connect Excel tooHDInsight with hello Microsoft Hive ODBC Driver][hdinsight-hive-odbc].</span></span>

## <a name="next-steps"></a><span data-ttu-id="b0794-278">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b0794-278">Next steps</span></span>
<span data-ttu-id="b0794-279">Ebben az oktatóanyagban úgy találtuk, hogyan tootransform egy strukturálatlan JSON adatkészlet egy strukturált Hive tábla tooquery be, adatok vizsgálatát és elemzését Twitter az Azure-on HDInsight használatával.</span><span class="sxs-lookup"><span data-stu-id="b0794-279">In this tutorial we have seen how tootransform an unstructured JSON dataset into a structured Hive table tooquery, explore, and analyze data from Twitter by using HDInsight on Azure.</span></span> <span data-ttu-id="b0794-280">toolearn több, lásd:</span><span class="sxs-lookup"><span data-stu-id="b0794-280">toolearn more, see:</span></span>

* <span data-ttu-id="b0794-281">[Első lépései a hdinsight eszközzel][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="b0794-281">[Get started with HDInsight][hdinsight-get-started]</span></span>
* <span data-ttu-id="b0794-282">[A HBase a hdinsight eszközben valós idejű Twitter sentiment elemzése][hdinsight-hbase-twitter-sentiment]</span><span class="sxs-lookup"><span data-stu-id="b0794-282">[Analyze real-time Twitter sentiment with HBase in HDInsight][hdinsight-hbase-twitter-sentiment]</span></span>
* <span data-ttu-id="b0794-283">[HDInsight eszközzel repülési késleltetés adatok elemzése][hdinsight-analyze-flight-delay-data]</span><span class="sxs-lookup"><span data-stu-id="b0794-283">[Analyze flight delay data using HDInsight][hdinsight-analyze-flight-delay-data]</span></span>
* <span data-ttu-id="b0794-284">[Power Query az Excel tooHDInsight csatlakozás][hdinsight-power-query]</span><span class="sxs-lookup"><span data-stu-id="b0794-284">[Connect Excel tooHDInsight with Power Query][hdinsight-power-query]</span></span>
* <span data-ttu-id="b0794-285">[Csatlakozás a Microsoft Hive ODBC-illesztő hello Excel tooHDInsight][hdinsight-hive-odbc]</span><span class="sxs-lookup"><span data-stu-id="b0794-285">[Connect Excel tooHDInsight with hello Microsoft Hive ODBC Driver][hdinsight-hive-odbc]</span></span>
* <span data-ttu-id="b0794-286">[Use Sqoop with HDInsight][hdinsight-use-sqoop]</span><span class="sxs-lookup"><span data-stu-id="b0794-286">[Use Sqoop with HDInsight][hdinsight-use-sqoop]</span></span>

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
