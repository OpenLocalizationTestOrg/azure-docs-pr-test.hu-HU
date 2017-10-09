---
title: az Apache Hive - Azure HDInsight Twitter-adatok aaaAnalyze |} Microsoft Docs
description: "Ismerje meg, hogyan toouse használatát Hive és a Hadoop HDInsight tootransform nyers TWitter-adatok kereshető Hive táblába."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: e1e249ed-5f57-40d6-b3bc-a1b4d9a871d3
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 02c4d027c7bbf390ac1c3724c14f8d549ea5195e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-twitter-data-using-hive-and-hadoop-on-hdinsight"></a><span data-ttu-id="fb7ad-103">A HDInsight Hive és a Hadoop használatával Twitter-adatok elemzése</span><span class="sxs-lookup"><span data-stu-id="fb7ad-103">Analyze Twitter data using Hive and Hadoop on HDInsight</span></span>

<span data-ttu-id="fb7ad-104">Megtudhatja, hogyan toouse Apache Hive tooprocess Twitter-adatok.</span><span class="sxs-lookup"><span data-stu-id="fb7ad-104">Learn how toouse Apache Hive tooprocess Twitter data.</span></span> <span data-ttu-id="fb7ad-105">hello eredménye küldi hello legtöbb Twitter-üzeneteket, amelyek tartalmaznak egy adott szó Twitter felhasználók listáját.</span><span class="sxs-lookup"><span data-stu-id="fb7ad-105">hello result is a list of Twitter users who sent hello most tweets that contain a certain word.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fb7ad-106">Ebben a dokumentumban hello lépéseket a HDInsight 3.6 teszteltük.</span><span class="sxs-lookup"><span data-stu-id="fb7ad-106">hello steps in this document were tested on HDInsight 3.6.</span></span>
>
> <span data-ttu-id="fb7ad-107">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="fb7ad-107">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="fb7ad-108">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="fb7ad-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="get-hello-data"></a><span data-ttu-id="fb7ad-109">Hello adatok beolvasása</span><span class="sxs-lookup"><span data-stu-id="fb7ad-109">Get hello data</span></span>

<span data-ttu-id="fb7ad-110">Twitter lehetővé teszi a tooretrieve hello [minden tweetet adatainak](https://dev.twitter.com/docs/platform-objects/tweets) JavaScript Object Notation (JSON)-dokumentumként egy REST API-n keresztül.</span><span class="sxs-lookup"><span data-stu-id="fb7ad-110">Twitter allows you tooretrieve hello [data for each tweet](https://dev.twitter.com/docs/platform-objects/tweets) as a JavaScript Object Notation (JSON) document through a REST API.</span></span> <span data-ttu-id="fb7ad-111">[OAuth](http://oauth.net) hitelesítési toohello API szükséges.</span><span class="sxs-lookup"><span data-stu-id="fb7ad-111">[OAuth](http://oauth.net) is required for authentication toohello API.</span></span>

### <a name="create-a-twitter-application"></a><span data-ttu-id="fb7ad-112">Twitter-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="fb7ad-112">Create a Twitter application</span></span>

1. <span data-ttu-id="fb7ad-113">Egy webböngészőből bejelentkezés túl[https://apps.twitter.com/](https://apps.twitter.com/).</span><span class="sxs-lookup"><span data-stu-id="fb7ad-113">From a web browser, sign in too[https://apps.twitter.com/](https://apps.twitter.com/).</span></span> <span data-ttu-id="fb7ad-114">Kattintson a hello **előfizetési most** hivatkozásra, ha egy Twitter-fiók nem rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="fb7ad-114">Click hello **Sign-up now** link if you don't have a Twitter account.</span></span>

2. <span data-ttu-id="fb7ad-115">Kattintson a **új alkalmazás létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="fb7ad-115">Click **Create New App**.</span></span>

3. <span data-ttu-id="fb7ad-116">Adja meg **neve**, **leírás**, **webhely**.</span><span class="sxs-lookup"><span data-stu-id="fb7ad-116">Enter **Name**, **Description**, **Website**.</span></span> <span data-ttu-id="fb7ad-117">Biztosíthatja, hogy be egy URL-címet hello **webhely** mező.</span><span class="sxs-lookup"><span data-stu-id="fb7ad-117">You can make up a URL for hello **Website** field.</span></span> <span data-ttu-id="fb7ad-118">hello a következő táblázatban néhány példa értékek toouse jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="fb7ad-118">hello following table shows some sample values toouse:</span></span>

   | <span data-ttu-id="fb7ad-119">Mező</span><span class="sxs-lookup"><span data-stu-id="fb7ad-119">Field</span></span> | <span data-ttu-id="fb7ad-120">Érték</span><span class="sxs-lookup"><span data-stu-id="fb7ad-120">Value</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="fb7ad-121">Név</span><span class="sxs-lookup"><span data-stu-id="fb7ad-121">Name</span></span> |<span data-ttu-id="fb7ad-122">MyHDInsightApp</span><span class="sxs-lookup"><span data-stu-id="fb7ad-122">MyHDInsightApp</span></span> |
   | <span data-ttu-id="fb7ad-123">Leírás</span><span class="sxs-lookup"><span data-stu-id="fb7ad-123">Description</span></span> |<span data-ttu-id="fb7ad-124">MyHDInsightApp</span><span class="sxs-lookup"><span data-stu-id="fb7ad-124">MyHDInsightApp</span></span> |
   | <span data-ttu-id="fb7ad-125">Honlap</span><span class="sxs-lookup"><span data-stu-id="fb7ad-125">Website</span></span> |<span data-ttu-id="fb7ad-126">http://www.myhdinsightapp.com</span><span class="sxs-lookup"><span data-stu-id="fb7ad-126">http://www.myhdinsightapp.com</span></span> |

4. <span data-ttu-id="fb7ad-127">Ellenőrizze **Igen, elfogadom**, és kattintson a **az Twitter-alkalmazás létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="fb7ad-127">Check **Yes, I agree**, and then click **Create your Twitter application**.</span></span>

5. <span data-ttu-id="fb7ad-128">Kattintson a hello **engedélyek** külön-külön hello alapértelmezett engedély **csak olvasható**.</span><span class="sxs-lookup"><span data-stu-id="fb7ad-128">Click hello **Permissions** tab. hello default permission is **Read only**.</span></span>

6. <span data-ttu-id="fb7ad-129">Kattintson a hello **kulcsok és a hozzáférési jogkivonatok** fülre.</span><span class="sxs-lookup"><span data-stu-id="fb7ad-129">Click hello **Keys and Access Tokens** tab.</span></span>

7. <span data-ttu-id="fb7ad-130">Kattintson a **a hozzáférési jogkivonat létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="fb7ad-130">Click **Create my access token**.</span></span>

8. <span data-ttu-id="fb7ad-131">Kattintson a **teszt OAuth** hello oldal jobb felső sarkában hello.</span><span class="sxs-lookup"><span data-stu-id="fb7ad-131">Click **Test OAuth** in hello upper-right corner of hello page.</span></span>

9. <span data-ttu-id="fb7ad-132">Írja le **kulcsa**, **felhasználói titok**, **hozzáférési jogkivonat**, és **Access token titkos**.</span><span class="sxs-lookup"><span data-stu-id="fb7ad-132">Write down **consumer key**, **Consumer secret**, **Access token**, and **Access token secret**.</span></span>

### <a name="download-tweets"></a><span data-ttu-id="fb7ad-133">Töltse le a Twitter-üzenetek</span><span class="sxs-lookup"><span data-stu-id="fb7ad-133">Download tweets</span></span>

<span data-ttu-id="fb7ad-134">Python kódját a következő hello 10 000 Twitter-üzeneteket tölti le, a Twitter és mentse őket tooa fájlt **tweets.txt**.</span><span class="sxs-lookup"><span data-stu-id="fb7ad-134">hello following Python code downloads 10,000 tweets from Twitter and save them tooa file named **tweets.txt**.</span></span>

> [!NOTE]
> <span data-ttu-id="fb7ad-135">a lépéseket követve hello hello HDInsight-fürt vannak végrehajtani, mert a Python már telepítve van.</span><span class="sxs-lookup"><span data-stu-id="fb7ad-135">hello following steps are performed on hello HDInsight cluster, since Python is already installed.</span></span>

1. <span data-ttu-id="fb7ad-136">Csatlakozzon az SSH használatával toohello HDInsight-fürt:</span><span class="sxs-lookup"><span data-stu-id="fb7ad-136">Connect toohello HDInsight cluster using SSH:</span></span>

    ```bash
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="fb7ad-137">További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="fb7ad-137">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

3. <span data-ttu-id="fb7ad-138">Használjon hello következő parancsok tooinstall [Tweepy](http://www.tweepy.org/), [Progressbar](https://pypi.python.org/pypi/progressbar/2.2), és más szükséges csomagokat:</span><span class="sxs-lookup"><span data-stu-id="fb7ad-138">Use hello following commands tooinstall [Tweepy](http://www.tweepy.org/), [Progressbar](https://pypi.python.org/pypi/progressbar/2.2), and other required packages:</span></span>

   ```bash
   sudo apt install python-dev libffi-dev libssl-dev
   sudo apt remove python-openssl
   pip install virtualenv
   mkdir gettweets
   cd gettweets
   virtualenv gettweets
   source gettweets/bin/activate
   pip install tweepy progressbar pyOpenSSL requests[security]
   ```

4. <span data-ttu-id="fb7ad-139">Használjon hello következő parancsot a toocreate nevű fájlt **gettweets.py**:</span><span class="sxs-lookup"><span data-stu-id="fb7ad-139">Use hello following command toocreate a file named **gettweets.py**:</span></span>

   ```bash
   nano gettweets.py
   ```

5. <span data-ttu-id="fb7ad-140">Szöveg hello hello tartalmát, a következő használatát hello **gettweets.py** fájlt:</span><span class="sxs-lookup"><span data-stu-id="fb7ad-140">Use hello following text as hello contents of hello **gettweets.py** file:</span></span>

   ```python
   #!/usr/bin/python

   from tweepy import Stream, OAuthHandler
   from tweepy.streaming import StreamListener
   from progressbar import ProgressBar, Percentage, Bar
   import json
   import sys

   #Twitter app information
   consumer_secret='Your consumer secret'
   consumer_key='Your consumer key'
   access_token='Your access token'
   access_token_secret='Your access token secret'

   #hello number of tweets we want tooget
   max_tweets=10000

   #Create hello listener class that receives and saves tweets
   class listener(StreamListener):
       #On init, set hello counter toozero and create a progress bar
       def __init__(self, api=None):
           self.num_tweets = 0
           self.pbar = ProgressBar(widgets=[Percentage(), Bar()], maxval=max_tweets).start()

       #When data is received, do this
       def on_data(self, data):
           #Append hello tweet toohello 'tweets.txt' file
           with open('tweets.txt', 'a') as tweet_file:
               tweet_file.write(data)
               #Increment hello number of tweets
               self.num_tweets += 1
               #Check toosee if we have hit max_tweets and exit if so
               if self.num_tweets >= max_tweets:
                   self.pbar.finish()
                   sys.exit(0)
               else:
                   #increment hello progress bar
                   self.pbar.update(self.num_tweets)
           return True

       #Handle any errors that may occur
       def on_error(self, status):
           print status

   #Get hello OAuth token
   auth = OAuthHandler(consumer_key, consumer_secret)
   auth.set_access_token(access_token, access_token_secret)
   #Use hello listener class for stream processing
   twitterStream = Stream(auth, listener())
   #Filter for these topics
   twitterStream.filter(track=["azure","cloud","hdinsight"])
   ```

    > [!IMPORTANT]
    > <span data-ttu-id="fb7ad-141">Cserélje le a helyőrző szöveg hello hello elemek hello információkkal a twitter-alkalmazás a következő:</span><span class="sxs-lookup"><span data-stu-id="fb7ad-141">Replace hello placeholder text for hello following items with hello information from your twitter application:</span></span>
    >
    > * `consumer_secret`
    > * `consumer_key`
    > * `access_token`
    > * `access_token_secret`

6. <span data-ttu-id="fb7ad-142">Használjon **Ctrl + X**, majd **Y** toosave hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="fb7ad-142">Use **Ctrl + X**, then **Y** toosave hello file.</span></span>

7. <span data-ttu-id="fb7ad-143">Használja a következő parancs toorun hello fájl hello, és töltse le a Twitter-üzeneteket:</span><span class="sxs-lookup"><span data-stu-id="fb7ad-143">Use hello following command toorun hello file and download tweets:</span></span>

    ```bash
    python gettweets.py
    ```

    <span data-ttu-id="fb7ad-144">Egy folyamatjelző jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="fb7ad-144">A progress indicator appears.</span></span> <span data-ttu-id="fb7ad-145">Akkor számít fel too100 % hello Twitter-üzeneteket letöltődnek.</span><span class="sxs-lookup"><span data-stu-id="fb7ad-145">It counts up too100% as hello tweets are downloaded.</span></span>

   > [!NOTE]
   > <span data-ttu-id="fb7ad-146">Ha hello folyamatjelző sáv tooadvance hosszú ideig tart, módosítania kell a hello szűrő tootrack trendekkel témaköröket.</span><span class="sxs-lookup"><span data-stu-id="fb7ad-146">If it is taking a long time for hello progress bar tooadvance, you should change hello filter tootrack trending topics.</span></span> <span data-ttu-id="fb7ad-147">Ha a szűrőben lévő hello témakör számos Twitter-üzeneteket, gyorsan kaphat hello szükséges 10000 Twitter-üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="fb7ad-147">When there are many tweets about hello topic in your filter, you can quickly get hello 10000 tweets needed.</span></span>

### <a name="upload-hello-data"></a><span data-ttu-id="fb7ad-148">Hello adatok feltöltése</span><span class="sxs-lookup"><span data-stu-id="fb7ad-148">Upload hello data</span></span>

<span data-ttu-id="fb7ad-149">tooupload hello tooHDInsight adattárolás, a következő parancsok használata hello:</span><span class="sxs-lookup"><span data-stu-id="fb7ad-149">tooupload hello data tooHDInsight storage, use hello following commands:</span></span>

   ```bash
   hdfs dfs -mkdir -p /tutorials/twitter/data
   hdfs dfs -put tweets.txt /tutorials/twitter/data/tweets.txt
```

<span data-ttu-id="fb7ad-150">Ezek a parancsok hello adatok hello fürt összes csomópontja által elérhető helyen tárolja.</span><span class="sxs-lookup"><span data-stu-id="fb7ad-150">These commands store hello data in a location that all nodes in hello cluster can access.</span></span>

## <a name="run-hello-hiveql-job"></a><span data-ttu-id="fb7ad-151">Hello HiveQL feladat futtatása</span><span class="sxs-lookup"><span data-stu-id="fb7ad-151">Run hello HiveQL job</span></span>

1. <span data-ttu-id="fb7ad-152">A következő parancs toocreate HiveQL utasításokat tartalmazó fájl hello használata:</span><span class="sxs-lookup"><span data-stu-id="fb7ad-152">Use hello following command toocreate a file containing HiveQL statements:</span></span>

   ```bash
   nano twitter.hql
   ```

    <span data-ttu-id="fb7ad-153">Szöveg hello hello fájl tartalmát, a következő hello használata:</span><span class="sxs-lookup"><span data-stu-id="fb7ad-153">Use hello following text as hello contents of hello file:</span></span>

   ```hiveql
   set hive.exec.dynamic.partition = true;
   set hive.exec.dynamic.partition.mode = nonstrict;
   -- Drop table, if it exists
   DROP TABLE tweets_raw;
   -- Create it, pointing toward hello tweets logged from Twitter
   CREATE EXTERNAL TABLE tweets_raw (
       json_response STRING
   )
   STORED AS TEXTFILE LOCATION '/tutorials/twitter/data';
   -- Drop and recreate hello destination table
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
   -- Select tweets from hello imported data, parse hello JSON,
   -- and insert into hello tweets table
   FROM tweets_raw
   INSERT OVERWRITE TABLE tweets
   SELECT
       cast(get_json_object(json_response, '$.id_str') as BIGINT),
       get_json_object(json_response, '$.created_at'),
       concat(substr (get_json_object(json_response, '$.created_at'),1,10),' ',
       substr (get_json_object(json_response, '$.created_at'),27,4)),
       substr (get_json_object(json_response, '$.created_at'),27,4),
       case substr (get_json_object(json_response,    '$.created_at'),5,3)
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
   ```

2. <span data-ttu-id="fb7ad-154">Nyomja le az ENTER **Ctrl + X**, majd nyomja le az **Y** toosave hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="fb7ad-154">Press **Ctrl + X**, then press **Y** toosave hello file.</span></span>
3. <span data-ttu-id="fb7ad-155">A következő parancs toorun hello HiveQL hello fájlban lévő hello használata:</span><span class="sxs-lookup"><span data-stu-id="fb7ad-155">Use hello following command toorun hello HiveQL contained in hello file:</span></span>

   ```bash
   beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http' -i twitter.hql
   ```

    <span data-ttu-id="fb7ad-156">Ez a parancs futtatása hello hello **twitter.hql** fájlt.</span><span class="sxs-lookup"><span data-stu-id="fb7ad-156">This command runs hello hello **twitter.hql** file.</span></span> <span data-ttu-id="fb7ad-157">Miután hello lekérdezés befejeződött, megjelenik egy `jdbc:hive2//localhost:10001/>` kérdés.</span><span class="sxs-lookup"><span data-stu-id="fb7ad-157">Once hello query completes, you see a `jdbc:hive2//localhost:10001/>` prompt.</span></span>

4. <span data-ttu-id="fb7ad-158">Hello beeline parancssorból lekérdezés tooverify adatok importálása a következő hello használata:</span><span class="sxs-lookup"><span data-stu-id="fb7ad-158">From hello beeline prompt, use hello following query tooverify that data was imported:</span></span>

   ```hiveql
   SELECT name, screen_name, count(1) as cc
       FROM tweets
       WHERE text like "%Azure%"
       GROUP BY name,screen_name
       ORDER BY cc DESC LIMIT 10;
   ```

    <span data-ttu-id="fb7ad-159">A lekérdezés által visszaadott legfeljebb 10 Twitter-üzeneteket, amelyek tartalmazzák a hello word **Azure** az üdvözlő üzenet szövegét.</span><span class="sxs-lookup"><span data-stu-id="fb7ad-159">This query returns a maximum of 10 tweets that contain hello word **Azure** in hello message text.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fb7ad-160">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fb7ad-160">Next steps</span></span>

<span data-ttu-id="fb7ad-161">Megtanulta, hogyan tootransform egy strukturálatlan JSON strukturált Hive tábla a DataSet adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="fb7ad-161">You have learned how tootransform an unstructured JSON dataset into a structured Hive table.</span></span> <span data-ttu-id="fb7ad-162">toolearn Hive hdinsight, kapcsolatos további információkért tekintse meg a következő dokumentumok hello:</span><span class="sxs-lookup"><span data-stu-id="fb7ad-162">toolearn more about Hive on HDInsight, see hello following documents:</span></span>

* [<span data-ttu-id="fb7ad-163">Első lépései a hdinsight eszközzel</span><span class="sxs-lookup"><span data-stu-id="fb7ad-163">Get started with HDInsight</span></span>](hdinsight-hadoop-linux-tutorial-get-started.md)
* [<span data-ttu-id="fb7ad-164">HDInsight eszközzel repülési késleltetés adatok elemzése</span><span class="sxs-lookup"><span data-stu-id="fb7ad-164">Analyze flight delay data using HDInsight</span></span>](hdinsight-analyze-flight-delay-data-linux.md)

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[apache-hive-tutorial]: https://cwiki.apache.org/confluence/display/Hive/Tutorial

[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter
