---
title: "Machine Learning API-k: Szövegelemzések |} Microsoft Docs"
description: "A Microsoft Machine Learning szöveg Analytics API-k lehet használt tooanalyze véleményeket elemzés, a kulcs kifejezés kinyerési, a nyelvi észlelés és a témakör észlelési strukturálatlan szöveg."
services: machine-learning
documentationcenter: 
author: onewth
manager: jhubbard
editor: cgronlun
ms.assetid: 5b60694e-5521-4e4d-bf6a-1a92fdf94b65
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: onewth
ROBOTS: NOINDEX
redirect_url: ../cognitive-services/cognitive-services-text-analytics-quick-start
redirect_document_id: True
ms.openlocfilehash: 49380c83849c5d5fdd8dce4f3899ebcb3d6870f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="machine-learning-apis-text-analytics-for-sentiment-key-phrase-extraction-language-detection-and-topic-detection"></a>Machine Learning APIs: a hangulat szövegalapú elemzése, Kulcsszókeresés, Nyelvfelismerés és Témafelismerés
> [!NOTE]
> Ez az útmutató hello API 1 verziójához is. 2-es, verzió [ **tekintse meg a toothis dokumentum**](../cognitive-services/cognitive-services-text-analytics-quick-start.md). 2-es verziója most hello előnyben részesített Ez API.
> 
> 

## <a name="overview"></a>Áttekintés
hello szöveg Analytics API szövegelemzések együttese [webszolgáltatások](https://datamarket.azure.com/dataset/amla/text-analytics) Azure Machine Learning-val készült. hello API használt tooanalyze lehet olyan feladatokhoz, mint a céggel kapcsolatos véleményeket elemzés, a kulcs kifejezés kinyerési, a nyelvi felderítését és a témakör észlelési strukturálatlan szöveg. Nincs adat képzési szükséges toouse az API: csak hozni a szöveges adatokat. Ez az API feldolgozás alatt álló technikák toodeliver legjobb osztály előrejelzéseket speciális természetes nyelvű használja.

A művelet szövegelemzések tekintheti meg a [bemutató hely](https://text-analytics-demo.azurewebsites.net/), ahol is talál [minták](https://text-analytics-demo.azurewebsites.net/Home/SampleCode) hogyan tooimplement szövegelemzések C# és Python.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

- - -
## <a name="sentiment-analysis"></a>Véleményelemzés
hello API numerikus pontszám 0 és 1 közötti adja vissza. Pontszámok Bezárás too1 pozitív véleményeket azt jelzik, amíg pontszámok Bezárás too0 jelző negatív céggel kapcsolatos véleményeket. A véleménypontszámok generálásáról az osztályozási technikák gondoskodnak. hello bemeneti szolgáltatások toohello osztályozó n-g, része a beszéd címkék és a word beágyazásokat szolgáltatásokat tartalmazza. Angol jelenleg csak a támogatott nyelvi hello.

## <a name="key-phrase-extraction"></a>A kulcsfontosságú kifejezések kinyerése
hello API azt jelöli, beszélő legfontosabb hello hello bemeneti szöveges karakterláncok listáját adja vissza. A Microsoft Office kifinomult természetes nyelvek feldolgozási eszközkészletéből alkalmazunk technikákat. Angol jelenleg csak a támogatott nyelvi hello.

## <a name="language-detection"></a>Nyelvfelismerés
hello API adja vissza, hello észlelte a nyelvet és a numerikus 0 és 1 közötti pontszámot. Pontszámok Bezárás too1 100 %-os valószínűségét, hogy teljesül-e a azonosított hello nyelvi jelzi. Összesen 120 nyelv támogatott.

## <a name="topic-detection"></a>Téma azonosítása
Ez az egy újonnan kiadott API-hello felső észlelt témakörök elküldött szöveg rekordok listáját adja vissza. A témakörök azonosítása egy kulcskifejezésre alapul, amely egy vagy több kapcsolódó szó lehet. Az API használatához legalább 100 szöveg rögzíti az elküldött toobe, de nem tervezett toodetect témakörök több száz toothousands rekordok. Vegye figyelembe, hogy az API díjszámításában egy beküldött rekord egy tranzakciónak felel meg. hello API szöveget, például az értékelést, és a felhasználói visszajelzések rövid, emberi esetén tervezett toowork.

- - -
## <a name="api-definition"></a>API-definíció
### <a name="headers"></a>Fejlécek
Győződjön meg a megfelelő fejlécek hello szerepeljenek a kérést, amely a következő lesz:

    Authorization: Basic <creds>
    Accept: application/json

    Where <creds> = ConvertToBase64(“AccountKey:” + yourActualAccountKey);  

Megtalálja a fiókhoz tartozó fiókkulcs hello [Azure adatok piaci](https://datamarket.azure.com/account/keys). Vegye figyelembe, hogy jelenleg csak JSON elfogadható bemeneti és kimeneti formátum. XML nem támogatott.

- - -
## <a name="single-response-apis"></a>Egyetlen válasz API-k
### <a name="getsentiment"></a>GetSentiment
**URL-CÍME**    

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentiment

**Kérelem (példa)**

Az alábbi hello hívásban azt a kért hello kifejezés "Hello World" véleményeket-elemzés:

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentiment?Text=hello+world

Ez visszaadja azt választ az alábbiak szerint:

    {
      "odata.metadata":"https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/$metadata",
        "Score":1.0
    }

- - -
### <a name="getkeyphrases"></a>GetKeyPhrases
**URL-CÍME**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrases

**Kérelem (példa)**

Hello hívás az alábbi, az azt kérő hello legfontosabb kifejezések található hello szöveg a "Volt, egyedi decor és rövid személyzettel csodálatos Szálloda toostay":

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrases?
    Text=It+was+a+wonderful+hotel+to+stay+at,+with+unique+decor+and+friendly+staff

Ez visszaadja azt választ az alábbiak szerint:

    {
      "odata.metadata":"https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/$metadata",
      "KeyPhrases":[
        "wonderful hotel",
        "unique decor",
        "friendly staff"
      ]
    }

- - -
### <a name="getlanguage"></a>GetLanguage
**URL-CÍME**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguage

**Kérelem (példa)**

Hello GET hívást az alábbi, az azt kérő hello véleményeket hello legfontosabb kifejezések hello szövegben a az *Hello World*

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguages?
    Text=Hello+World

Ez visszaadja azt választ az alábbiak szerint:

    {
      "UnknownLanguage": false,
      "DetectedLanguages": [{
        "Name": "English",
        "Iso6391Name": "en",
        "Score": 1.0
      }]
    }

**Választható paraméterek:**

`NumberOfLanguagesToDetect`nem kötelező paraméter. hello alapértelmezett érték 1.

- - -
## <a name="batch-apis"></a>Kötegelt API-k
hello Szövegelemzések szolgáltatás lehetővé teszi, hogy toodo véleményeket és a kulcs-kifejezés információkinyerés kötegelt módban. Vegye figyelembe, hogy egyes hello rekordok pontozza a mennyiségeket összesítése egy tranzakcióként. Tegyük fel Ha Ön kérte a céggel kapcsolatos véleményeket 1000 rekordok egyetlen hívásban, 1000-tranzakciók le kell vonni.

Vegye figyelembe, hogy hello azonosítók megadott hello rendszerbe hello rendszer által visszaadott hello azonosítók. hello webszolgáltatás nem ellenőrzi, hogy ezek az azonosítók egyediek. Feladata hello hello hívó tooverify egyediségi. 

### <a name="getsentimentbatch"></a>GetSentimentBatch
**URL-CÍME**    

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentimentBatch

**Kérelem (példa)**

A FELADÁS egy vagy több hello hívás alatt, azt a kért a hello hangulati elemek, a "Hello World", a "Hello PEL World" és a "Hello saját World" szövegre hello kérelem törzse hello hello kifejezések:

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentimentBatch 

A kérelem törzse:

    {"Inputs":
    [
        {"Id":"1","Text":"hello world"},
        {"Id":"2","Text":"hello foo world"},
        {"Id":"3","Text":"hello my world"},
    ]}

Hello válaszként az alábbi a szöveg azonosítók társított pontszámok hello listájának beolvasása:

    {
      "odata.metadata":"<url>", 
      "SentimentBatch":
      [
        {"Score":0.9549767,"Id":"1"},
        {"Score":0.7767222,"Id":"2"},
        {"Score":0.8988889,"Id":"3"}
      ],  
      "Errors":[]
    }


- - -
### <a name="getkeyphrasesbatch"></a>GetKeyPhrasesBatch
**URL-CÍME**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrasesBatch

**Kérelem (példa)**

Ebben a példában azt a kért hangulati elemek hello legfontosabb kifejezések hello a következő szöveget a hello listáját: 

* "Volt, egyedi decor és rövid személyzettel csodálatos Szálloda toostay"
* "Az volt a nagyon fontos megbeszélések egy elképesztő build konferencia"
* "hello forgalom lett Félelmetes, I töltött toohello repülőtéri is három óra"

A kérelem a FELADÁS egy vagy több hívás toohello végpontként:

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrasesBatch

A kérelem törzse:

    {"Inputs":
    [
        {"Id":"1","Text":"It was a wonderful hotel toostay at, with unique decor and friendly staff"},
        {"Id":"2","Text":"It was an amazing build conference, with very interesting talks"},
        {"Id":"3","Text":"hello traffic was terrible, I spent three hours going toohello airport"}
    ]}

Hello válaszként az alábbi szerezze be a szöveget azonosítók társított kulcs mondatok hello listát:

    { "odata.metadata":"<url>",
         "KeyPhrasesBatch":
        [
           {"KeyPhrases":["unique decor","friendly staff","wonderful hotel"],"Id":"1"},
           {"KeyPhrases":["amazing build conference","interesting talks"],"Id":"2"},
           {"KeyPhrases":["hours","traffic","airport"],"Id":"3" }
        ],
        "Errors":[]
    }

- - -
### <a name="getlanguagebatch"></a>GetLanguageBatch

Hello POST híváson az alábbi, az azt kérő szöveg két bemeneti nyelvi észlelését:

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguageBatch

A kérelem törzse:

    {
      "Inputs": [
        {"Text": "hello world", "Id": "1"},
        {"Text": "c'est la vie", "Id": "2"}
      ]
    }

Ez visszaadja a következő választ, ahol angol észlelésekor első bemenet hello és francia hello második bemeneti hello:

    {
       "LanguageBatch": [{
         "Id": "1",
         "DetectedLanguages": [{
            "Name": "English",
            "Iso6391Name": "en",
            "Score": 1.0
         }],
         "UnknownLanguage": false
       },
       {
         "Id": "2",
         "DetectedLanguages": [{
            "Name": "French",
            "Iso6391Name": "fr",
            "Score": 1.0
         }],
         "UnknownLanguage": false
       }],
       "Errors": []
    }

- - -
## <a name="topic-detection-apis"></a>A témakör API-k
Ez az egy újonnan kiadott API-hello felső észlelt témakörök elküldött szöveg rekordok listáját adja vissza. A témakörök azonosítása egy kulcskifejezésre alapul, amely egy vagy több kapcsolódó szó lehet. Vegye figyelembe, hogy az API díjszámításában egy beküldött rekord egy tranzakciónak felel meg.

Az API használatához legalább 100 szöveg rögzíti az elküldött toobe, de nem tervezett toodetect témakörök több száz toothousands rekordok.

### <a name="topics--submit-job"></a>Témakör – küldés feladat
**URL-CÍME**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/StartTopicDetection

**Kérelem (példa)**

Hello POST híváson az alábbi, az azt kérő témakörök 100 cikkeket, ahol hello első és utolsó bemeneti cikkek jelennek meg, és két StopPhrases tartalmazza azon.

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/StartTopicDetection HTTP/1.1

A kérelem törzse:

    {"Inputs":[
        {"Id":"1","Text":"I loved hello food at this restaurant"},
        ...,
        {"Id":"100","Text":"I hated hello decor"}
    ],
    "StopPhrases":[
        "restaurant", “visitor"
    ]}

Hello válaszként az alábbi hello JobId hello elküldött feladat beolvasása:

    {
        "odata.metadata":"<url>",
        "JobId":"<JobId>"
    }

Egyetlen szó, vagy több olyan word mondatok, amelyek nem vissza kell adni, témakörök listáját. Kimenő nagyon általános témaköröket használt toofilter lehet. Például egy adatkészlet kapcsolatos Szálloda értékelést, "Szálloda" és "hostel" lehet kijelölve stop kifejezések.  

### <a name="topics--poll-for-job-results"></a>Témakör – a feladat eredményeinek lekérdezési
**URL-CÍME**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetTopicDetectionResult

**Kérelem (példa)**

Sikeresen hello JobId hello "Küldési feladat" lépés toofetch hello eredményt adott vissza. Azt javasoljuk, hogy hívjon ehhez a végponthoz minden percben, amíg az állapot = "Complete" hello válaszként. Körülbelül 10 perc, a feladat toocomplete vagy a sok ezer rögzíti a feladatok hosszabb ideig eltarthat.

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetTopicDetectionResult?JobId=<JobId>


Amíg feldolgozása, hello választ a következők:

    {
        "odata.metadata":"<url>",
        "Status":"Running",
         "TopicInfo":[],
        "TopicAssignment":[],
        "Errors":[]
    }


hello API kimeneti formátum a következő hello JSON formátumban adja vissza:

    {
        "odata.metadata":"<url>",
        "Status":"Finished",
        "TopicInfo":[
        {
            "TopicId":"ed00480e-f0a0-41b3-8fe4-07c1593f4afd",
            "Score":8.0,
            "KeyPhrase":"food"
        },
        ...
        {
            "TopicId":"a5ca3f1a-fdb1-4f02-8f1b-89f2f626d692",
            "Score":6.0,
            "KeyPhrase":"decor"
            }
          ],
        "TopicAssignment":[
        {
            "Id":"1",
            "TopicId":"ed00480e-f0a0-41b3-8fe4-07c1593f4afd",
            "Distance":0.7809
        },
        ...
        {
            "Id":"100",
            "TopicId":"a5ca3f1a-fdb1-4f02-8f1b-89f2f626d692",
            "Distance":0.8034
        }
        ],
        "Errors":[]


hello válasz részét képező összes hello tulajdonságai a következők:

**TopicInfo tulajdonságai**

| Kulcs | Leírás |
|:--- |:--- |
| TopicId |Egyes témakörök egyedi azonosítója. |
| Pontszám |Bejegyzések száma tootopic rendelve. |
| KeyPhrase |Summarizing szót vagy kifejezést hello témakörhöz. 1 vagy több szó lehet. |

**TopicAssignment tulajdonságai**

| Kulcs | Leírás |
|:--- |:--- |
| Azonosító |Hello rekord azonosítója. Csatlakozás hello bemeneti szereplő toohello azonosítója. |
| TopicId |hello Témaazonosító mely hello rekord hozzá van rendelve. |
| távolság |Várható, rekord hello toohello témakör tartozik. Távolság szorosabb toozero magasabb abban, hogy jelzi. |

