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
# <a name="analyze-twitter-data-using-hive-in-hdinsight"></a>Hdinsight Hive eszközzel Twitter-adatok elemzése
Közösségi webhelyek rendszer egyik hello fő vezetői kényszeríti a big data alkalmazására vonatkozóan. Nyilvános API-k, például a Twitter helyek által biztosított az hasznos adatforrást ismertetése népszerű trendeket és elemzésére.
Ebben az oktatóanyagban, lesz egy Twitter-adatfolyam-API segítségével könnyebben nyerhet Twitter-üzeneteket, és használja Apache Hive az Azure HDInsight tooget küldi hello legtöbb Twitter-üzeneteket, amelyek egy adott szó található Twitter felhasználók listáját.

> [!IMPORTANT]
> hello jelen dokumentumban leírt lépések szükséges egy Windows-alapú HDInsight-fürtöt. Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement). Lépéseket adott tooa Linux-alapú fürt, lásd: [Hive HDInsight (Linux) használatával elemezheti Twitter adatok](hdinsight-analyze-twitter-data-linux.md).

## <a name="prerequisites"></a>Előfeltételek
Ez az oktatóanyag elkezdéséhez hello következő kell rendelkeznie:

* **A munkaállomás** az Azure PowerShell telepítése és konfigurálása.

    Windows PowerShell-parancsfájlok tooexecute, futtassa a Azure PowerShell rendszergazdaként, és hello végrehajtási házirend beállítása túl*RemoteSigned*. Lásd: [futtassa a Windows PowerShell-parancsfájlok][powershell-script].

    Windows PowerShell-parancsfájlok futtatásakor, előtt ellenőrizze, rendelkezik csatlakoztatott tooyour Azure-előfizetés hello a következő parancsmag használatával:

    ```powershell
    Login-AzureRmAccount
    ```

    Ha több Azure-előfizetéssel rendelkezik, használja a következő parancsmag tooset hello előfizetésben hello:

    ```powershell
    Select-AzureRmSubscription -SubscriptionID <Azure Subscription ID>
    ```

    > [!IMPORTANT]
    > A HDInsight-erőforrások Azure Service Managerrel történő kezelésének Azure PowerShell-támogatása **elavult**, így 2017. január 1-től megszűnt. a dokumentum használatát hello új HDInsight-parancsmagokat, amelyek működnek az Azure Resource Manager hello szükséges lépések.
    >
    > Kövesse a lépéseket hello [telepítse és konfigurálja az Azure Powershellt](/powershell/azureps-cmdlets-docs) tooinstall hello legújabb Azure PowerShell verziója. Ha vannak olyan parancsprogramjai, hogy szükség toobe módosított toouse hello új parancsmagok használata Azure Resource Managerrel működő című [áttelepítése tooAzure fejlesztési Resource Manager-alapú HDInsight-fürtök eszközei](hdinsight-hadoop-development-using-azure-resource-manager.md) további információt.

* **Egy Azure HDInsight fürt**. A fürtök kiépítése útmutatásért lásd: [használatának megkezdésében a HDInsight] [ hdinsight-get-started] vagy [Provision HDInsight clusters][hdinsight-provision]. Hello oktatóanyag későbbi részében szüksége lesz hello fürt nevét.

hello következő táblázat sorolja fel ebben az oktatóanyagban használt hello fájlok:

| Fájlok | Leírás |
| --- | --- |
| /tutorials/Twitter/Data/tweets.txt |hello forrásadatok hello Hive feladat. |
| /tutorials/Twitter/output |kimeneti mappa hello hello Hive feladat. hello Hive feladat kimeneti fájl alapértelmezett neve a **000000_0**. |
| tutorials/Twitter/Twitter.hql |hello HiveQL-parancsfájlt. |
| /tutorials/Twitter/JobStatus |hello Hadoop-feladat állapotát. |

## <a name="get-twitter-feed"></a>Get Twitter-hírcsatorna
Ebben az oktatóanyagban hello segítségével [streamelési API-k Twitter][twitter-streaming-api]. hello használandó API streaming adott Twitter van [állapotok-vagy szűrőkezelő][twitter-statuses-filter].

> [!NOTE]
> 10 000 Twitter-üzeneteket és hello Hive parancsfájl (hello a következő szakaszban ismertetett) tartalmazó fájlt fel lett töltve, egy nyilvános Blob tárolóban. Ez a szakasz kihagyhatja, ha azt szeretné, hogy toouse hello feltöltött fájlokat.

[Twitter-üzeneteket adatok](https://dev.twitter.com/docs/platform-objects/tweets) , amely tartalmaz egy összetett beágyazott struktúra hello JavaScript Object Notation (JSON) formátumban tárolja. Ahelyett, hogy hány sornyi kód írása a hagyományos programozási nyelv használatával, alakíthatja át a beágyazott struktúra egy Hive táblába úgy, hogy azt egy Structured Query Language (SQL) lekérdezhetők – például a HiveQL nevű nyelv.

Twitter OAuth tooprovide engedélyezett hozzáférési tooits API-t használja. OAuth olyan hitelesítési protokoll, amely lehetővé teszi a felhasználók tooapprove alkalmazások tooact nevükben nélkül megosztása a jelszavát. További információ található [oauth.net](http://oauth.net/) vagy kiváló hello [Beginner's Guide tooOAuth](http://hueniverse.com/oauth/) a Hueniverse.

hello első lépés toouse OAuth toocreate egy új alkalmazást hello Twitter fejlesztői helyen.

**toocreate egy Twitter-alkalmazás**

1. Jelentkezzen be a túl[https://apps.twitter.com/](https://apps.twitter.com/). Kattintson a hello **feliratkozás most** hivatkozásra, ha egy Twitter-fiók nem rendelkezik.
2. Kattintson a **új alkalmazás létrehozása**.
3. Adja meg **neve**, **leírás**, **webhely**. Biztosíthatja, hogy be egy URL-címet hello **webhely** mező. hello a következő táblázatban néhány példa értékek toouse jeleníti meg:

   | Mező | Érték |
   | --- | --- |
   |  Név |MyHDInsightApp |
   |  Leírás |MyHDInsightApp |
   |  Honlap |http://www.myhdinsightapp.com |
4. Ellenőrizze **Igen, elfogadom**, és kattintson a **az Twitter-alkalmazás létrehozása**.
5. Kattintson a hello **engedélyek** külön-külön hello alapértelmezett engedély **csak olvasható**. Ez a megfelelő ehhez az oktatóanyaghoz.
6. Kattintson a hello **kulcsok és a hozzáférési jogkivonatok** fülre.
7. Kattintson a **a hozzáférési jogkivonat létrehozása**.
8. Kattintson a **teszt OAuth** hello oldal jobb felső sarkában hello.
9. Írja le **kulcsa**, **felhasználói titok**, **hozzáférési jogkivonat**, és **Access token titkos**. Hello oktatóanyag későbbi részében szüksége lesz hello értékekre.

Ebben az oktatóanyagban a Windows PowerShell toomake hello webszolgáltatás-hívás fog használni. .NET C# mintát, lásd: [elemzése a HBase a hdinsight eszközben valós idejű Twitter sentiment][hdinsight-hbase-twitter-sentiment]. hello egyéb népszerű eszköz toomake webszolgáltatás-hívás van [ *Curl*][curl]. Curl letölthető [Itt][curl-download].

> [!NOTE]
> Ha a Windows hello curl-parancsot használja, idézőjelek közé foglalt helyett használjon szimpla idézőjelben az hello beállítás értékét.

**tooget Twitter-üzenetek**

1. Nyissa meg a Windows PowerShell integrált parancsfájlkezelési környezet (ISE) hello. (Hello Windows 8 kezdőképernyőn írja be a **PowerShell_ISE** majd **Windows PowerShell ISE**. Lásd: [indítsa el a Windows PowerShell használatával Windows 8 és Windows][powershell-start].)
2. Másolja a következő parancsfájl hello parancsfájl ablaktáblára hello:

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

3. Hello első öt tooeight változók megadása hello parancsfájlban:

    Változó|Leírás
    ---|---
    $clusterName|Ez az hello nevét, ahol szeretné toorun hello alkalmazás hello HDInsight-fürt.
    $oauth_consumer_key|Ez a hello Twitter alkalmazás **kulcsa** megírt korábbi hello Twitter-alkalmazás létrehozásakor.
    $oauth_consumer_secret|Ez a hello Twitter alkalmazás **felhasználói titok** korábban megírt.
    $oauth_token|Ez a hello Twitter alkalmazás **hozzáférési jogkivonat** korábban megírt.
    $oauth_token_secret|Ez a hello Twitter alkalmazás **access token titkos** korábban megírt.
    $destBlobName|Ez az hello kimeneti blob neve. hello alapértelmezett értéke **tutorials/twitter/data/tweets.txt**. Ha megváltoztatja hello alapértelmezett értékét, ennek megfelelően kell tooupdate hello Windows PowerShell-parancsfájlok.
    $trackString|hello webszolgáltatás visszaadható kapcsolódó toothese kulcsszavak Twitter-üzeneteket. hello alapértelmezett értéke **Azure-felhő, HDInsight**. Hello alapértelmezett értékének módosításához a Windows PowerShell-parancsfájlok hello ennek megfelelően frissíti.
    $lineMax|hello érték határozza meg, hány Twitter-üzeneteket hello parancsfájl olvasását. Tart körülbelül 3 percig tooread 100 Twitter-üzeneteket. Beállíthatja a korábbinál több, de további időt toodownload fog igénybe venni.

1. Nyomja le az **F5** toorun hello parancsfájl. Ha a probléma megoldásához, problémát tapasztal összes hello sort, és nyomja le az **F8**.
2. Ekkor megjelenik a "Complete!" hello kimeneti hello végén. Piros hibaüzenet jelenik meg.

Egy érvényesítési eljárással ellenőrizheti a hello kimeneti fájl, **/tutorials/twitter/data/tweets.txt**, a Azure Blob-tároló egy Azure Tártallózó vagy az Azure PowerShell használatával. A Windows PowerShell-parancsfájlpélda a fájlok listázása, lásd: [és a HDInsight együttes használata a Blob storage][hdinsight-storage-powershell].

## <a name="create-hiveql-script"></a>Hozzon létre HiveQL-parancsfájlt
Az Azure PowerShell használatával is futtathatja több HiveQL utasítást egy időben, vagy csomag hello HiveQL utasítás egy parancsfájl-fájlba. Ebben az oktatóanyagban létrehoz egy HiveQL-parancsfájlt. hello parancsfájl feltöltött tooAzure Blob Storage tárolóban kell lennie. A következő szakaszban hello Azure PowerShell használatával fog futni hello parancsfájlt.

> [!NOTE]
> hello Hive parancsfájl és 10 000 Twitter-üzeneteket tartalmazó fájl fel lett töltve egy nyilvános Blob tárolóban. Ez a szakasz kihagyhatja, ha azt szeretné, hogy toouse hello feltöltött fájlokat.

hello HiveQL-parancsfájlt hello következőket hajtja végre:

1. **Hello tweets_raw table DROP** hello tábla már létezik.
2. **Hello tweets_raw Hive tábla létrehozásához**. Az ideiglenes strukturált Hive tábla további kivonat hello adatokat tároló, átalakítási és betöltési (ETL) feldolgozása. A partíciókon információkért lásd: [Hive oktatóanyag][apache-hive-tutorial].
3. **Adatok betöltése** forrásmappából hello, /tutorials/twitter/data. ideiglenes Hive tábla struktúrába most átalakítása hello nagy Twitter-üzeneteket adatkészlet beágyazott JSON formátumban.
4. **Közvetlen hello Twitter-üzeneteket tábla** hello tábla már létezik.
5. **Hello Twitter-üzeneteket tábla létrehozása**. Hive eszközzel kérdezheti hello Twitter-üzeneteket dataset ellen, meg kell toorun egy másik ETL-folyamat. Az ETL-folyamat hello "twitter_raw" táblában tárolt adatok hello részletesebb tábla séma határozza meg.
6. **Felülírás táblázat beszúrása**. Az összetett Hive parancsfájl hello Hadoop-fürt lesz indítsa hosszú MapReduce-feladatok készlete. A fürt a DataSet adatkészlet és hello méretétől függően ez eltarthat körülbelül 10 perc.
7. **INSERT felülírása directory**. Az alábbi lekérdezés és a kimeneti hello dataset tooa fájl futtatása. Ez a lekérdezés a Twitter felhasználók, akik a legtöbb Twitter-üzeneteket hello word "Azure" lévő küldött listáját adja vissza.

**a Hive toocreate parancsfájlt, és töltse fel tooAzure**

1. Nyissa meg a Windows PowerShell ISE.
2. Másolja a következő parancsfájl hello parancsfájl ablaktáblára hello:

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

3. Hello első két változók megadása hello parancsfájlban:

   | Változó | Leírás |
   | --- | --- |
   |  $clusterName |Adja meg hello HDInsight-fürt nevét, ahol szeretné toorun hello alkalmazás. |
   |  $subscriptionID |Adja meg az Azure-előfizetés azonosítóját. |
   |  $sourceDataPath |hello Azure Blob tárolási helye, ahol hello Hive-lekérdezések hello adatokat olvassa. Ez a változó toochange nem szükséges. |
   |  $outputPath |hello hello Hive-lekérdezések lesz a kimeneti eredmények hello Azure Blob tárolási helyét. Ez a változó toochange nem szükséges. |
   |  $hqlScriptFile |hello helye és fájlneve hello hello HiveQL-parancsfájlt. Ez a változó toochange nem szükséges. |
4. Nyomja le az **F5** toorun hello parancsfájl. Ha a probléma megoldásához, problémát tapasztal összes hello sort, és nyomja le az **F8**.
5. Ekkor megjelenik a "Complete!" hello kimeneti hello végén. Piros hibaüzenet jelenik meg.

Egy érvényesítési eljárással ellenőrizheti a hello kimeneti fájl, **/tutorials/twitter/twitter.hql**, a Azure Blob-tároló egy Azure Tártallózó vagy az Azure PowerShell használatával. A Windows PowerShell-parancsfájlpélda a fájlok listázása, lásd: [és a HDInsight együttes használata a Blob storage][hdinsight-storage-powershell].

## <a name="process-twitter-data-by-using-hive"></a>Twitter-adatok feldolgozása a Hive eszközzel
Befejezte az összes hello előkészítő munkát. Most hello Hive parancsfájl elindíthat és hello eredményeket.

### <a name="submit-a-hive-job"></a>Hive feladat elküldése
A következő Windows PowerShell parancsfájl toorun hello Hive parancsfájl hello használata. Szüksége lesz tooset hello első változó.

> [!NOTE]
> toouse hello Twitter-üzeneteket és a HiveQL-parancsfájlt hello utolsó két szakaszokban set $hqlScriptFile too"/tutorials/twitter/twitter.hql feltöltött hello". toouse hello néhányat a meglévők közül, amelyek lettek feltöltve tooa nyilvános blob meg, állítsa be a $hqlScriptFile túl"wasb://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql".

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

### <a name="check-hello-results"></a>Hello ellenőrzésének az eredménye
A következő Windows PowerShell parancsfájl toocheck hello Hive-feladat kimeneti hello használata. Tooset hello első két változót kell.

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
> hello Hive tábla mezőhatároló hello \001 használja. hello elválasztó karaktere nem hello kimenet látható.

Hello elemzési adatokat az Azure Blob storage lettek helyezve, miután hello adatok tooan Azure SQL adatbázis vagy SQL server exportálása, hello adatok tooExcel exportálja a Power Query használatával vagy az alkalmazásadatok toohello csatlakozás hello Hive ODBC-illesztő segítségével. További információkért lásd: [és a HDInsight együttes használata Sqoop][hdinsight-use-sqoop], [adatelemzés repülési késleltetés HDInsight eszközzel][hdinsight-analyze-flight-delay-data], [ Power Query az Excel tooHDInsight csatlakozás][hdinsight-power-query], és [kapcsolódás Excel tooHDInsight a Microsoft Hive ODBC-illesztő hello][hdinsight-hive-odbc].

## <a name="next-steps"></a>Következő lépések
Ebben az oktatóanyagban úgy találtuk, hogyan tootransform egy strukturálatlan JSON adatkészlet egy strukturált Hive tábla tooquery be, adatok vizsgálatát és elemzését Twitter az Azure-on HDInsight használatával. toolearn több, lásd:

* [Első lépései a hdinsight eszközzel][hdinsight-get-started]
* [A HBase a hdinsight eszközben valós idejű Twitter sentiment elemzése][hdinsight-hbase-twitter-sentiment]
* [HDInsight eszközzel repülési késleltetés adatok elemzése][hdinsight-analyze-flight-delay-data]
* [Power Query az Excel tooHDInsight csatlakozás][hdinsight-power-query]
* [Csatlakozás a Microsoft Hive ODBC-illesztő hello Excel tooHDInsight][hdinsight-hive-odbc]
* [Use Sqoop with HDInsight][hdinsight-use-sqoop]

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
