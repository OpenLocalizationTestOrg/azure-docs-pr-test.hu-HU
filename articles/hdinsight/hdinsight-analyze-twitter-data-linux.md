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
# <a name="analyze-twitter-data-using-hive-and-hadoop-on-hdinsight"></a>A HDInsight Hive és a Hadoop használatával Twitter-adatok elemzése

Megtudhatja, hogyan toouse Apache Hive tooprocess Twitter-adatok. hello eredménye küldi hello legtöbb Twitter-üzeneteket, amelyek tartalmaznak egy adott szó Twitter felhasználók listáját.

> [!IMPORTANT]
> Ebben a dokumentumban hello lépéseket a HDInsight 3.6 teszteltük.
>
> Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="get-hello-data"></a>Hello adatok beolvasása

Twitter lehetővé teszi a tooretrieve hello [minden tweetet adatainak](https://dev.twitter.com/docs/platform-objects/tweets) JavaScript Object Notation (JSON)-dokumentumként egy REST API-n keresztül. [OAuth](http://oauth.net) hitelesítési toohello API szükséges.

### <a name="create-a-twitter-application"></a>Twitter-alkalmazás létrehozása

1. Egy webböngészőből bejelentkezés túl[https://apps.twitter.com/](https://apps.twitter.com/). Kattintson a hello **előfizetési most** hivatkozásra, ha egy Twitter-fiók nem rendelkezik.

2. Kattintson a **új alkalmazás létrehozása**.

3. Adja meg **neve**, **leírás**, **webhely**. Biztosíthatja, hogy be egy URL-címet hello **webhely** mező. hello a következő táblázatban néhány példa értékek toouse jeleníti meg:

   | Mező | Érték |
   |:--- |:--- |
   | Név |MyHDInsightApp |
   | Leírás |MyHDInsightApp |
   | Honlap |http://www.myhdinsightapp.com |

4. Ellenőrizze **Igen, elfogadom**, és kattintson a **az Twitter-alkalmazás létrehozása**.

5. Kattintson a hello **engedélyek** külön-külön hello alapértelmezett engedély **csak olvasható**.

6. Kattintson a hello **kulcsok és a hozzáférési jogkivonatok** fülre.

7. Kattintson a **a hozzáférési jogkivonat létrehozása**.

8. Kattintson a **teszt OAuth** hello oldal jobb felső sarkában hello.

9. Írja le **kulcsa**, **felhasználói titok**, **hozzáférési jogkivonat**, és **Access token titkos**.

### <a name="download-tweets"></a>Töltse le a Twitter-üzenetek

Python kódját a következő hello 10 000 Twitter-üzeneteket tölti le, a Twitter és mentse őket tooa fájlt **tweets.txt**.

> [!NOTE]
> a lépéseket követve hello hello HDInsight-fürt vannak végrehajtani, mert a Python már telepítve van.

1. Csatlakozzon az SSH használatával toohello HDInsight-fürt:

    ```bash
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

3. Használjon hello következő parancsok tooinstall [Tweepy](http://www.tweepy.org/), [Progressbar](https://pypi.python.org/pypi/progressbar/2.2), és más szükséges csomagokat:

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

4. Használjon hello következő parancsot a toocreate nevű fájlt **gettweets.py**:

   ```bash
   nano gettweets.py
   ```

5. Szöveg hello hello tartalmát, a következő használatát hello **gettweets.py** fájlt:

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
    > Cserélje le a helyőrző szöveg hello hello elemek hello információkkal a twitter-alkalmazás a következő:
    >
    > * `consumer_secret`
    > * `consumer_key`
    > * `access_token`
    > * `access_token_secret`

6. Használjon **Ctrl + X**, majd **Y** toosave hello fájlt.

7. Használja a következő parancs toorun hello fájl hello, és töltse le a Twitter-üzeneteket:

    ```bash
    python gettweets.py
    ```

    Egy folyamatjelző jelenik meg. Akkor számít fel too100 % hello Twitter-üzeneteket letöltődnek.

   > [!NOTE]
   > Ha hello folyamatjelző sáv tooadvance hosszú ideig tart, módosítania kell a hello szűrő tootrack trendekkel témaköröket. Ha a szűrőben lévő hello témakör számos Twitter-üzeneteket, gyorsan kaphat hello szükséges 10000 Twitter-üzeneteket.

### <a name="upload-hello-data"></a>Hello adatok feltöltése

tooupload hello tooHDInsight adattárolás, a következő parancsok használata hello:

   ```bash
   hdfs dfs -mkdir -p /tutorials/twitter/data
   hdfs dfs -put tweets.txt /tutorials/twitter/data/tweets.txt
```

Ezek a parancsok hello adatok hello fürt összes csomópontja által elérhető helyen tárolja.

## <a name="run-hello-hiveql-job"></a>Hello HiveQL feladat futtatása

1. A következő parancs toocreate HiveQL utasításokat tartalmazó fájl hello használata:

   ```bash
   nano twitter.hql
   ```

    Szöveg hello hello fájl tartalmát, a következő hello használata:

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

2. Nyomja le az ENTER **Ctrl + X**, majd nyomja le az **Y** toosave hello fájlt.
3. A következő parancs toorun hello HiveQL hello fájlban lévő hello használata:

   ```bash
   beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http' -i twitter.hql
   ```

    Ez a parancs futtatása hello hello **twitter.hql** fájlt. Miután hello lekérdezés befejeződött, megjelenik egy `jdbc:hive2//localhost:10001/>` kérdés.

4. Hello beeline parancssorból lekérdezés tooverify adatok importálása a következő hello használata:

   ```hiveql
   SELECT name, screen_name, count(1) as cc
       FROM tweets
       WHERE text like "%Azure%"
       GROUP BY name,screen_name
       ORDER BY cc DESC LIMIT 10;
   ```

    A lekérdezés által visszaadott legfeljebb 10 Twitter-üzeneteket, amelyek tartalmazzák a hello word **Azure** az üdvözlő üzenet szövegét.

## <a name="next-steps"></a>Következő lépések

Megtanulta, hogyan tootransform egy strukturálatlan JSON strukturált Hive tábla a DataSet adatkészlet. toolearn Hive hdinsight, kapcsolatos további információkért tekintse meg a következő dokumentumok hello:

* [Első lépései a hdinsight eszközzel](hdinsight-hadoop-linux-tutorial-get-started.md)
* [HDInsight eszközzel repülési késleltetés adatok elemzése](hdinsight-analyze-flight-delay-data-linux.md)

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[apache-hive-tutorial]: https://cwiki.apache.org/confluence/display/Hive/Tutorial

[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter
