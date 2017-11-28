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
# <a name="machine-learning-apis-text-analytics-for-sentiment-key-phrase-extraction-language-detection-and-topic-detection"></a><span data-ttu-id="9256a-103">Machine Learning APIs: a hangulat szövegalapú elemzése, Kulcsszókeresés, Nyelvfelismerés és Témafelismerés</span><span class="sxs-lookup"><span data-stu-id="9256a-103">Machine Learning APIs: Text Analytics for Sentiment, Key Phrase Extraction, Language Detection and Topic Detection</span></span>
> [!NOTE]
> <span data-ttu-id="9256a-104">Ez az útmutató hello API 1 verziójához is.</span><span class="sxs-lookup"><span data-stu-id="9256a-104">This guide is for version 1 of hello API.</span></span> <span data-ttu-id="9256a-105">2-es, verzió [ **tekintse meg a toothis dokumentum**](../cognitive-services/cognitive-services-text-analytics-quick-start.md).</span><span class="sxs-lookup"><span data-stu-id="9256a-105">For version 2, [**refer toothis document**](../cognitive-services/cognitive-services-text-analytics-quick-start.md).</span></span> <span data-ttu-id="9256a-106">2-es verziója most hello előnyben részesített Ez API.</span><span class="sxs-lookup"><span data-stu-id="9256a-106">Version 2 is now hello preferred version of this API.</span></span>
> 
> 

## <a name="overview"></a><span data-ttu-id="9256a-107">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="9256a-107">Overview</span></span>
<span data-ttu-id="9256a-108">hello szöveg Analytics API szövegelemzések együttese [webszolgáltatások](https://datamarket.azure.com/dataset/amla/text-analytics) Azure Machine Learning-val készült.</span><span class="sxs-lookup"><span data-stu-id="9256a-108">hello Text Analytics API is a suite of text analytics [web services](https://datamarket.azure.com/dataset/amla/text-analytics) built with Azure Machine Learning.</span></span> <span data-ttu-id="9256a-109">hello API használt tooanalyze lehet olyan feladatokhoz, mint a céggel kapcsolatos véleményeket elemzés, a kulcs kifejezés kinyerési, a nyelvi felderítését és a témakör észlelési strukturálatlan szöveg.</span><span class="sxs-lookup"><span data-stu-id="9256a-109">hello API can be used tooanalyze unstructured text for tasks such as sentiment analysis, key phrase extraction, language detection and topic detection.</span></span> <span data-ttu-id="9256a-110">Nincs adat képzési szükséges toouse az API: csak hozni a szöveges adatokat.</span><span class="sxs-lookup"><span data-stu-id="9256a-110">No training data is needed toouse this API: just bring your text data.</span></span> <span data-ttu-id="9256a-111">Ez az API feldolgozás alatt álló technikák toodeliver legjobb osztály előrejelzéseket speciális természetes nyelvű használja.</span><span class="sxs-lookup"><span data-stu-id="9256a-111">This API uses advanced natural language processing techniques toodeliver best in class predictions.</span></span>

<span data-ttu-id="9256a-112">A művelet szövegelemzések tekintheti meg a [bemutató hely](https://text-analytics-demo.azurewebsites.net/), ahol is talál [minták](https://text-analytics-demo.azurewebsites.net/Home/SampleCode) hogyan tooimplement szövegelemzések C# és Python.</span><span class="sxs-lookup"><span data-stu-id="9256a-112">You can see text analytics in action on our [demo site](https://text-analytics-demo.azurewebsites.net/), where you will also find [samples](https://text-analytics-demo.azurewebsites.net/Home/SampleCode) on how tooimplement text analytics in C# and Python.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

- - -
## <a name="sentiment-analysis"></a><span data-ttu-id="9256a-113">Véleményelemzés</span><span class="sxs-lookup"><span data-stu-id="9256a-113">Sentiment analysis</span></span>
<span data-ttu-id="9256a-114">hello API numerikus pontszám 0 és 1 közötti adja vissza.</span><span class="sxs-lookup"><span data-stu-id="9256a-114">hello API returns a numeric score between 0 & 1.</span></span> <span data-ttu-id="9256a-115">Pontszámok Bezárás too1 pozitív véleményeket azt jelzik, amíg pontszámok Bezárás too0 jelző negatív céggel kapcsolatos véleményeket.</span><span class="sxs-lookup"><span data-stu-id="9256a-115">Scores close too1 indicate positive sentiment, while scores close too0 indicate negative sentiment.</span></span> <span data-ttu-id="9256a-116">A véleménypontszámok generálásáról az osztályozási technikák gondoskodnak.</span><span class="sxs-lookup"><span data-stu-id="9256a-116">Sentiment score is generated using classification techniques.</span></span> <span data-ttu-id="9256a-117">hello bemeneti szolgáltatások toohello osztályozó n-g, része a beszéd címkék és a word beágyazásokat szolgáltatásokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="9256a-117">hello input features toohello classifier include n-grams, features generated from part-of-speech tags, and word embeddings.</span></span> <span data-ttu-id="9256a-118">Angol jelenleg csak a támogatott nyelvi hello.</span><span class="sxs-lookup"><span data-stu-id="9256a-118">Currently, English is hello only supported language.</span></span>

## <a name="key-phrase-extraction"></a><span data-ttu-id="9256a-119">A kulcsfontosságú kifejezések kinyerése</span><span class="sxs-lookup"><span data-stu-id="9256a-119">Key phrase extraction</span></span>
<span data-ttu-id="9256a-120">hello API azt jelöli, beszélő legfontosabb hello hello bemeneti szöveges karakterláncok listáját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="9256a-120">hello API returns a list of strings denoting hello key talking points in hello input text.</span></span> <span data-ttu-id="9256a-121">A Microsoft Office kifinomult természetes nyelvek feldolgozási eszközkészletéből alkalmazunk technikákat.</span><span class="sxs-lookup"><span data-stu-id="9256a-121">We employ techniques from Microsoft Office's sophisticated Natural Language Processing toolkit.</span></span> <span data-ttu-id="9256a-122">Angol jelenleg csak a támogatott nyelvi hello.</span><span class="sxs-lookup"><span data-stu-id="9256a-122">Currently, English is hello only supported language.</span></span>

## <a name="language-detection"></a><span data-ttu-id="9256a-123">Nyelvfelismerés</span><span class="sxs-lookup"><span data-stu-id="9256a-123">Language detection</span></span>
<span data-ttu-id="9256a-124">hello API adja vissza, hello észlelte a nyelvet és a numerikus 0 és 1 közötti pontszámot.</span><span class="sxs-lookup"><span data-stu-id="9256a-124">hello API returns hello detected language and a numeric score between 0 & 1.</span></span> <span data-ttu-id="9256a-125">Pontszámok Bezárás too1 100 %-os valószínűségét, hogy teljesül-e a azonosított hello nyelvi jelzi.</span><span class="sxs-lookup"><span data-stu-id="9256a-125">Scores close too1 indicate 100% certainty that hello identified language is true.</span></span> <span data-ttu-id="9256a-126">Összesen 120 nyelv támogatott.</span><span class="sxs-lookup"><span data-stu-id="9256a-126">A total of 120 languages are supported.</span></span>

## <a name="topic-detection"></a><span data-ttu-id="9256a-127">Téma azonosítása</span><span class="sxs-lookup"><span data-stu-id="9256a-127">Topic detection</span></span>
<span data-ttu-id="9256a-128">Ez az egy újonnan kiadott API-hello felső észlelt témakörök elküldött szöveg rekordok listáját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="9256a-128">This is a newly released API which returns hello top detected topics for a list of submitted text records.</span></span> <span data-ttu-id="9256a-129">A témakörök azonosítása egy kulcskifejezésre alapul, amely egy vagy több kapcsolódó szó lehet.</span><span class="sxs-lookup"><span data-stu-id="9256a-129">A topic is identified with a key phrase, which can be one or more related words.</span></span> <span data-ttu-id="9256a-130">Az API használatához legalább 100 szöveg rögzíti az elküldött toobe, de nem tervezett toodetect témakörök több száz toothousands rekordok.</span><span class="sxs-lookup"><span data-stu-id="9256a-130">This API requires a minimum of 100 text records toobe submitted, but is designed toodetect topics across hundreds toothousands of records.</span></span> <span data-ttu-id="9256a-131">Vegye figyelembe, hogy az API díjszámításában egy beküldött rekord egy tranzakciónak felel meg.</span><span class="sxs-lookup"><span data-stu-id="9256a-131">Note that this API charges 1 transaction per text record submitted.</span></span> <span data-ttu-id="9256a-132">hello API szöveget, például az értékelést, és a felhasználói visszajelzések rövid, emberi esetén tervezett toowork.</span><span class="sxs-lookup"><span data-stu-id="9256a-132">hello API is designed toowork well for short, human written text such as reviews and user feedback.</span></span>

- - -
## <a name="api-definition"></a><span data-ttu-id="9256a-133">API-definíció</span><span class="sxs-lookup"><span data-stu-id="9256a-133">API Definition</span></span>
### <a name="headers"></a><span data-ttu-id="9256a-134">Fejlécek</span><span class="sxs-lookup"><span data-stu-id="9256a-134">Headers</span></span>
<span data-ttu-id="9256a-135">Győződjön meg a megfelelő fejlécek hello szerepeljenek a kérést, amely a következő lesz:</span><span class="sxs-lookup"><span data-stu-id="9256a-135">Ensure that you include hello correct headers in your request, which should be as follows:</span></span>

    Authorization: Basic <creds>
    Accept: application/json

    Where <creds> = ConvertToBase64(“AccountKey:” + yourActualAccountKey);  

<span data-ttu-id="9256a-136">Megtalálja a fiókhoz tartozó fiókkulcs hello [Azure adatok piaci](https://datamarket.azure.com/account/keys).</span><span class="sxs-lookup"><span data-stu-id="9256a-136">You can find your account key from your account in hello [Azure Data Market](https://datamarket.azure.com/account/keys).</span></span> <span data-ttu-id="9256a-137">Vegye figyelembe, hogy jelenleg csak JSON elfogadható bemeneti és kimeneti formátum.</span><span class="sxs-lookup"><span data-stu-id="9256a-137">Note that currently only JSON is accepted for input and output formats.</span></span> <span data-ttu-id="9256a-138">XML nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="9256a-138">XML is not supported.</span></span>

- - -
## <a name="single-response-apis"></a><span data-ttu-id="9256a-139">Egyetlen válasz API-k</span><span class="sxs-lookup"><span data-stu-id="9256a-139">Single Response APIs</span></span>
### <a name="getsentiment"></a><span data-ttu-id="9256a-140">GetSentiment</span><span class="sxs-lookup"><span data-stu-id="9256a-140">GetSentiment</span></span>
<span data-ttu-id="9256a-141">**URL-CÍME**</span><span class="sxs-lookup"><span data-stu-id="9256a-141">**URL**</span></span>    

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentiment

<span data-ttu-id="9256a-142">**Kérelem (példa)**</span><span class="sxs-lookup"><span data-stu-id="9256a-142">**Example request**</span></span>

<span data-ttu-id="9256a-143">Az alábbi hello hívásban azt a kért hello kifejezés "Hello World" véleményeket-elemzés:</span><span class="sxs-lookup"><span data-stu-id="9256a-143">In hello call below, we are requesting sentiment analysis for hello phrase "Hello World":</span></span>

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentiment?Text=hello+world

<span data-ttu-id="9256a-144">Ez visszaadja azt választ az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="9256a-144">This will return a response as follows:</span></span>

    {
      "odata.metadata":"https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/$metadata",
        "Score":1.0
    }

- - -
### <a name="getkeyphrases"></a><span data-ttu-id="9256a-145">GetKeyPhrases</span><span class="sxs-lookup"><span data-stu-id="9256a-145">GetKeyPhrases</span></span>
<span data-ttu-id="9256a-146">**URL-CÍME**</span><span class="sxs-lookup"><span data-stu-id="9256a-146">**URL**</span></span>

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrases

<span data-ttu-id="9256a-147">**Kérelem (példa)**</span><span class="sxs-lookup"><span data-stu-id="9256a-147">**Example request**</span></span>

<span data-ttu-id="9256a-148">Hello hívás az alábbi, az azt kérő hello legfontosabb kifejezések található hello szöveg a "Volt, egyedi decor és rövid személyzettel csodálatos Szálloda toostay":</span><span class="sxs-lookup"><span data-stu-id="9256a-148">In hello call below, we are requesting hello key phrases found in hello text "It was a wonderful hotel toostay at, with unique decor and friendly staff":</span></span>

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrases?
    Text=It+was+a+wonderful+hotel+to+stay+at,+with+unique+decor+and+friendly+staff

<span data-ttu-id="9256a-149">Ez visszaadja azt választ az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="9256a-149">This will return a response as follows:</span></span>

    {
      "odata.metadata":"https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/$metadata",
      "KeyPhrases":[
        "wonderful hotel",
        "unique decor",
        "friendly staff"
      ]
    }

- - -
### <a name="getlanguage"></a><span data-ttu-id="9256a-150">GetLanguage</span><span class="sxs-lookup"><span data-stu-id="9256a-150">GetLanguage</span></span>
<span data-ttu-id="9256a-151">**URL-CÍME**</span><span class="sxs-lookup"><span data-stu-id="9256a-151">**URL**</span></span>

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguage

<span data-ttu-id="9256a-152">**Kérelem (példa)**</span><span class="sxs-lookup"><span data-stu-id="9256a-152">**Example request**</span></span>

<span data-ttu-id="9256a-153">Hello GET hívást az alábbi, az azt kérő hello véleményeket hello legfontosabb kifejezések hello szövegben a az *Hello World*</span><span class="sxs-lookup"><span data-stu-id="9256a-153">In hello GET call below, we are requesting for hello sentiment for hello key phrases in hello text *Hello World*</span></span>

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguages?
    Text=Hello+World

<span data-ttu-id="9256a-154">Ez visszaadja azt választ az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="9256a-154">This will return a response as follows:</span></span>

    {
      "UnknownLanguage": false,
      "DetectedLanguages": [{
        "Name": "English",
        "Iso6391Name": "en",
        "Score": 1.0
      }]
    }

<span data-ttu-id="9256a-155">**Választható paraméterek:**</span><span class="sxs-lookup"><span data-stu-id="9256a-155">**Optional parameters**</span></span>

<span data-ttu-id="9256a-156">`NumberOfLanguagesToDetect`nem kötelező paraméter.</span><span class="sxs-lookup"><span data-stu-id="9256a-156">`NumberOfLanguagesToDetect` is an optional parameter.</span></span> <span data-ttu-id="9256a-157">hello alapértelmezett érték 1.</span><span class="sxs-lookup"><span data-stu-id="9256a-157">hello default is 1.</span></span>

- - -
## <a name="batch-apis"></a><span data-ttu-id="9256a-158">Kötegelt API-k</span><span class="sxs-lookup"><span data-stu-id="9256a-158">Batch APIs</span></span>
<span data-ttu-id="9256a-159">hello Szövegelemzések szolgáltatás lehetővé teszi, hogy toodo véleményeket és a kulcs-kifejezés információkinyerés kötegelt módban.</span><span class="sxs-lookup"><span data-stu-id="9256a-159">hello Text Analytics service allows you toodo sentiment and key-phrase extractions in batch mode.</span></span> <span data-ttu-id="9256a-160">Vegye figyelembe, hogy egyes hello rekordok pontozza a mennyiségeket összesítése egy tranzakcióként.</span><span class="sxs-lookup"><span data-stu-id="9256a-160">Note that each of hello records scored counts as one transaction.</span></span> <span data-ttu-id="9256a-161">Tegyük fel Ha Ön kérte a céggel kapcsolatos véleményeket 1000 rekordok egyetlen hívásban, 1000-tranzakciók le kell vonni.</span><span class="sxs-lookup"><span data-stu-id="9256a-161">As an example, if you request sentiment for 1000 records in a single call, 1000 transactions will be deducted.</span></span>

<span data-ttu-id="9256a-162">Vegye figyelembe, hogy hello azonosítók megadott hello rendszerbe hello rendszer által visszaadott hello azonosítók.</span><span class="sxs-lookup"><span data-stu-id="9256a-162">Note that hello IDs entered into hello system are hello IDs returned by hello system.</span></span> <span data-ttu-id="9256a-163">hello webszolgáltatás nem ellenőrzi, hogy ezek az azonosítók egyediek.</span><span class="sxs-lookup"><span data-stu-id="9256a-163">hello web service does not check that these IDs are unique.</span></span> <span data-ttu-id="9256a-164">Feladata hello hello hívó tooverify egyediségi.</span><span class="sxs-lookup"><span data-stu-id="9256a-164">It is hello responsibility of hello caller tooverify uniqueness.</span></span> 

### <a name="getsentimentbatch"></a><span data-ttu-id="9256a-165">GetSentimentBatch</span><span class="sxs-lookup"><span data-stu-id="9256a-165">GetSentimentBatch</span></span>
<span data-ttu-id="9256a-166">**URL-CÍME**</span><span class="sxs-lookup"><span data-stu-id="9256a-166">**URL**</span></span>    

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentimentBatch

<span data-ttu-id="9256a-167">**Kérelem (példa)**</span><span class="sxs-lookup"><span data-stu-id="9256a-167">**Example request**</span></span>

<span data-ttu-id="9256a-168">A FELADÁS egy vagy több hello hívás alatt, azt a kért a hello hangulati elemek, a "Hello World", a "Hello PEL World" és a "Hello saját World" szövegre hello kérelem törzse hello hello kifejezések:</span><span class="sxs-lookup"><span data-stu-id="9256a-168">In hello POST call below, we are requesting for hello sentiments of hello phrases "Hello World", "Hello Foo World" and "Hello My World" in hello body of hello request:</span></span>

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentimentBatch 

<span data-ttu-id="9256a-169">A kérelem törzse:</span><span class="sxs-lookup"><span data-stu-id="9256a-169">Request body:</span></span>

    {"Inputs":
    [
        {"Id":"1","Text":"hello world"},
        {"Id":"2","Text":"hello foo world"},
        {"Id":"3","Text":"hello my world"},
    ]}

<span data-ttu-id="9256a-170">Hello válaszként az alábbi a szöveg azonosítók társított pontszámok hello listájának beolvasása:</span><span class="sxs-lookup"><span data-stu-id="9256a-170">In hello response below, you get hello list of scores associated with your text Ids:</span></span>

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
### <a name="getkeyphrasesbatch"></a><span data-ttu-id="9256a-171">GetKeyPhrasesBatch</span><span class="sxs-lookup"><span data-stu-id="9256a-171">GetKeyPhrasesBatch</span></span>
<span data-ttu-id="9256a-172">**URL-CÍME**</span><span class="sxs-lookup"><span data-stu-id="9256a-172">**URL**</span></span>

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrasesBatch

<span data-ttu-id="9256a-173">**Kérelem (példa)**</span><span class="sxs-lookup"><span data-stu-id="9256a-173">**Example request**</span></span>

<span data-ttu-id="9256a-174">Ebben a példában azt a kért hangulati elemek hello legfontosabb kifejezések hello a következő szöveget a hello listáját:</span><span class="sxs-lookup"><span data-stu-id="9256a-174">In this example, we are requesting for hello list of sentiments for hello key phrases in hello following texts:</span></span> 

* <span data-ttu-id="9256a-175">"Volt, egyedi decor és rövid személyzettel csodálatos Szálloda toostay"</span><span class="sxs-lookup"><span data-stu-id="9256a-175">"It was a wonderful hotel toostay at, with unique decor and friendly staff"</span></span>
* <span data-ttu-id="9256a-176">"Az volt a nagyon fontos megbeszélések egy elképesztő build konferencia"</span><span class="sxs-lookup"><span data-stu-id="9256a-176">"It was an amazing build conference, with very interesting talks"</span></span>
* <span data-ttu-id="9256a-177">"hello forgalom lett Félelmetes, I töltött toohello repülőtéri is három óra"</span><span class="sxs-lookup"><span data-stu-id="9256a-177">"hello traffic was terrible, I spent three hours going toohello airport"</span></span>

<span data-ttu-id="9256a-178">A kérelem a FELADÁS egy vagy több hívás toohello végpontként:</span><span class="sxs-lookup"><span data-stu-id="9256a-178">This request is made as a POST call toohello endpoint:</span></span>

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrasesBatch

<span data-ttu-id="9256a-179">A kérelem törzse:</span><span class="sxs-lookup"><span data-stu-id="9256a-179">Request body:</span></span>

    {"Inputs":
    [
        {"Id":"1","Text":"It was a wonderful hotel toostay at, with unique decor and friendly staff"},
        {"Id":"2","Text":"It was an amazing build conference, with very interesting talks"},
        {"Id":"3","Text":"hello traffic was terrible, I spent three hours going toohello airport"}
    ]}

<span data-ttu-id="9256a-180">Hello válaszként az alábbi szerezze be a szöveget azonosítók társított kulcs mondatok hello listát:</span><span class="sxs-lookup"><span data-stu-id="9256a-180">In hello response below, you get hello list of key phrases associated with your text Ids:</span></span>

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
### <a name="getlanguagebatch"></a><span data-ttu-id="9256a-181">GetLanguageBatch</span><span class="sxs-lookup"><span data-stu-id="9256a-181">GetLanguageBatch</span></span>

<span data-ttu-id="9256a-182">Hello POST híváson az alábbi, az azt kérő szöveg két bemeneti nyelvi észlelését:</span><span class="sxs-lookup"><span data-stu-id="9256a-182">In hello POST call below, we are requesting language detection for two text inputs:</span></span>

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguageBatch

<span data-ttu-id="9256a-183">A kérelem törzse:</span><span class="sxs-lookup"><span data-stu-id="9256a-183">Request body:</span></span>

    {
      "Inputs": [
        {"Text": "hello world", "Id": "1"},
        {"Text": "c'est la vie", "Id": "2"}
      ]
    }

<span data-ttu-id="9256a-184">Ez visszaadja a következő választ, ahol angol észlelésekor első bemenet hello és francia hello második bemeneti hello:</span><span class="sxs-lookup"><span data-stu-id="9256a-184">This returns hello following response, where English is detected in hello first input and French in hello second input:</span></span>

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
## <a name="topic-detection-apis"></a><span data-ttu-id="9256a-185">A témakör API-k</span><span class="sxs-lookup"><span data-stu-id="9256a-185">Topic Detection APIs</span></span>
<span data-ttu-id="9256a-186">Ez az egy újonnan kiadott API-hello felső észlelt témakörök elküldött szöveg rekordok listáját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="9256a-186">This is a newly released API which returns hello top detected topics for a list of submitted text records.</span></span> <span data-ttu-id="9256a-187">A témakörök azonosítása egy kulcskifejezésre alapul, amely egy vagy több kapcsolódó szó lehet.</span><span class="sxs-lookup"><span data-stu-id="9256a-187">A topic is identified with a key phrase, which can be one or more related words.</span></span> <span data-ttu-id="9256a-188">Vegye figyelembe, hogy az API díjszámításában egy beküldött rekord egy tranzakciónak felel meg.</span><span class="sxs-lookup"><span data-stu-id="9256a-188">Note that this API charges 1 transaction per text record submitted.</span></span>

<span data-ttu-id="9256a-189">Az API használatához legalább 100 szöveg rögzíti az elküldött toobe, de nem tervezett toodetect témakörök több száz toothousands rekordok.</span><span class="sxs-lookup"><span data-stu-id="9256a-189">This API requires a minimum of 100 text records toobe submitted, but is designed toodetect topics across hundreds toothousands of records.</span></span>

### <a name="topics--submit-job"></a><span data-ttu-id="9256a-190">Témakör – küldés feladat</span><span class="sxs-lookup"><span data-stu-id="9256a-190">Topics – Submit job</span></span>
<span data-ttu-id="9256a-191">**URL-CÍME**</span><span class="sxs-lookup"><span data-stu-id="9256a-191">**URL**</span></span>

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/StartTopicDetection

<span data-ttu-id="9256a-192">**Kérelem (példa)**</span><span class="sxs-lookup"><span data-stu-id="9256a-192">**Example request**</span></span>

<span data-ttu-id="9256a-193">Hello POST híváson az alábbi, az azt kérő témakörök 100 cikkeket, ahol hello első és utolsó bemeneti cikkek jelennek meg, és két StopPhrases tartalmazza azon.</span><span class="sxs-lookup"><span data-stu-id="9256a-193">In hello POST call below, we are requesting topics for a set of 100 articles, where hello first and last input articles are shown, and two StopPhrases are included.</span></span>

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/StartTopicDetection HTTP/1.1

<span data-ttu-id="9256a-194">A kérelem törzse:</span><span class="sxs-lookup"><span data-stu-id="9256a-194">Request body:</span></span>

    {"Inputs":[
        {"Id":"1","Text":"I loved hello food at this restaurant"},
        ...,
        {"Id":"100","Text":"I hated hello decor"}
    ],
    "StopPhrases":[
        "restaurant", “visitor"
    ]}

<span data-ttu-id="9256a-195">Hello válaszként az alábbi hello JobId hello elküldött feladat beolvasása:</span><span class="sxs-lookup"><span data-stu-id="9256a-195">In hello response below, you get hello JobId for hello submitted job:</span></span>

    {
        "odata.metadata":"<url>",
        "JobId":"<JobId>"
    }

<span data-ttu-id="9256a-196">Egyetlen szó, vagy több olyan word mondatok, amelyek nem vissza kell adni, témakörök listáját.</span><span class="sxs-lookup"><span data-stu-id="9256a-196">A list of single word or multiple word phrases which should not be returned as topics.</span></span> <span data-ttu-id="9256a-197">Kimenő nagyon általános témaköröket használt toofilter lehet.</span><span class="sxs-lookup"><span data-stu-id="9256a-197">Can be used toofilter out very generic topics.</span></span> <span data-ttu-id="9256a-198">Például egy adatkészlet kapcsolatos Szálloda értékelést, "Szálloda" és "hostel" lehet kijelölve stop kifejezések.</span><span class="sxs-lookup"><span data-stu-id="9256a-198">For example, in a dataset about hotel reviews, "hotel" and "hostel" may be sensible stop phrases.</span></span>  

### <a name="topics--poll-for-job-results"></a><span data-ttu-id="9256a-199">Témakör – a feladat eredményeinek lekérdezési</span><span class="sxs-lookup"><span data-stu-id="9256a-199">Topics – Poll for job results</span></span>
<span data-ttu-id="9256a-200">**URL-CÍME**</span><span class="sxs-lookup"><span data-stu-id="9256a-200">**URL**</span></span>

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetTopicDetectionResult

<span data-ttu-id="9256a-201">**Kérelem (példa)**</span><span class="sxs-lookup"><span data-stu-id="9256a-201">**Example request**</span></span>

<span data-ttu-id="9256a-202">Sikeresen hello JobId hello "Küldési feladat" lépés toofetch hello eredményt adott vissza.</span><span class="sxs-lookup"><span data-stu-id="9256a-202">Pass hello JobId returned from hello ‘Submit job’ step toofetch hello results.</span></span> <span data-ttu-id="9256a-203">Azt javasoljuk, hogy hívjon ehhez a végponthoz minden percben, amíg az állapot = "Complete" hello válaszként.</span><span class="sxs-lookup"><span data-stu-id="9256a-203">We recommend that you call this endpoint every minute until Status=’Complete’ in hello response.</span></span> <span data-ttu-id="9256a-204">Körülbelül 10 perc, a feladat toocomplete vagy a sok ezer rögzíti a feladatok hosszabb ideig eltarthat.</span><span class="sxs-lookup"><span data-stu-id="9256a-204">It will take around 10 mins for a job toocomplete, or longer for jobs with many thousands of records.</span></span>

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetTopicDetectionResult?JobId=<JobId>


<span data-ttu-id="9256a-205">Amíg feldolgozása, hello választ a következők:</span><span class="sxs-lookup"><span data-stu-id="9256a-205">While it is processing, hello response will be as follows:</span></span>

    {
        "odata.metadata":"<url>",
        "Status":"Running",
         "TopicInfo":[],
        "TopicAssignment":[],
        "Errors":[]
    }


<span data-ttu-id="9256a-206">hello API kimeneti formátum a következő hello JSON formátumban adja vissza:</span><span class="sxs-lookup"><span data-stu-id="9256a-206">hello API returns output in JSON format in hello following format:</span></span>

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


<span data-ttu-id="9256a-207">hello válasz részét képező összes hello tulajdonságai a következők:</span><span class="sxs-lookup"><span data-stu-id="9256a-207">hello properties for each part of hello response are as follows:</span></span>

<span data-ttu-id="9256a-208">**TopicInfo tulajdonságai**</span><span class="sxs-lookup"><span data-stu-id="9256a-208">**TopicInfo properties**</span></span>

| <span data-ttu-id="9256a-209">Kulcs</span><span class="sxs-lookup"><span data-stu-id="9256a-209">Key</span></span> | <span data-ttu-id="9256a-210">Leírás</span><span class="sxs-lookup"><span data-stu-id="9256a-210">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="9256a-211">TopicId</span><span class="sxs-lookup"><span data-stu-id="9256a-211">TopicId</span></span> |<span data-ttu-id="9256a-212">Egyes témakörök egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="9256a-212">A unique identifier for each topic.</span></span> |
| <span data-ttu-id="9256a-213">Pontszám</span><span class="sxs-lookup"><span data-stu-id="9256a-213">Score</span></span> |<span data-ttu-id="9256a-214">Bejegyzések száma tootopic rendelve.</span><span class="sxs-lookup"><span data-stu-id="9256a-214">Count of records assigned tootopic.</span></span> |
| <span data-ttu-id="9256a-215">KeyPhrase</span><span class="sxs-lookup"><span data-stu-id="9256a-215">KeyPhrase</span></span> |<span data-ttu-id="9256a-216">Summarizing szót vagy kifejezést hello témakörhöz.</span><span class="sxs-lookup"><span data-stu-id="9256a-216">A summarizing word or phrase for hello topic.</span></span> <span data-ttu-id="9256a-217">1 vagy több szó lehet.</span><span class="sxs-lookup"><span data-stu-id="9256a-217">Can be 1 or multiple words.</span></span> |

<span data-ttu-id="9256a-218">**TopicAssignment tulajdonságai**</span><span class="sxs-lookup"><span data-stu-id="9256a-218">**TopicAssignment properties**</span></span>

| <span data-ttu-id="9256a-219">Kulcs</span><span class="sxs-lookup"><span data-stu-id="9256a-219">Key</span></span> | <span data-ttu-id="9256a-220">Leírás</span><span class="sxs-lookup"><span data-stu-id="9256a-220">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="9256a-221">Azonosító</span><span class="sxs-lookup"><span data-stu-id="9256a-221">Id</span></span> |<span data-ttu-id="9256a-222">Hello rekord azonosítója.</span><span class="sxs-lookup"><span data-stu-id="9256a-222">Identifier for hello record.</span></span> <span data-ttu-id="9256a-223">Csatlakozás hello bemeneti szereplő toohello azonosítója.</span><span class="sxs-lookup"><span data-stu-id="9256a-223">Equates toohello ID included in hello input.</span></span> |
| <span data-ttu-id="9256a-224">TopicId</span><span class="sxs-lookup"><span data-stu-id="9256a-224">TopicId</span></span> |<span data-ttu-id="9256a-225">hello Témaazonosító mely hello rekord hozzá van rendelve.</span><span class="sxs-lookup"><span data-stu-id="9256a-225">hello topic ID which hello record has been assigned to.</span></span> |
| <span data-ttu-id="9256a-226">távolság</span><span class="sxs-lookup"><span data-stu-id="9256a-226">Distance</span></span> |<span data-ttu-id="9256a-227">Várható, rekord hello toohello témakör tartozik.</span><span class="sxs-lookup"><span data-stu-id="9256a-227">Confidence that hello record belongs toohello topic.</span></span> <span data-ttu-id="9256a-228">Távolság szorosabb toozero magasabb abban, hogy jelzi.</span><span class="sxs-lookup"><span data-stu-id="9256a-228">Distance closer toozero indicates higher confidence.</span></span> |

