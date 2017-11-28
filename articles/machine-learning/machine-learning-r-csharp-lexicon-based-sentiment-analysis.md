---
title: "aaa(deprecated) Lexicon alapú véleményeket Analysis - Azure |} Microsoft Docs"
description: "(elavult) Lexicon alapú véleményeket elemzés"
services: machine-learning
documentationcenter: 
author: pengxia
manager: jhubbard
editor: cgronlun
ms.assetid: 912f41af-966c-4d79-a413-6f9fc02823df
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: pengxia
ROBOTS: NOINDEX
redirect_url: https://gallery.cortanaintelligence.com/
redirect_document_id: True
ms.openlocfilehash: 1ed7e19441c6a8ad270a0c0f567b4aea588a583e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deprecated-lexicon-based-sentiment-analysis"></a><span data-ttu-id="70875-103">(elavult) Lexicon alapú véleményeket elemzés</span><span class="sxs-lookup"><span data-stu-id="70875-103">(deprecated) Lexicon Based Sentiment Analysis</span></span>

> [!NOTE]
> <span data-ttu-id="70875-104">a Microsoft DataMarket hello használatból van, és ez az API már elavult.</span><span class="sxs-lookup"><span data-stu-id="70875-104">hello Microsoft DataMarket is being retired and this API has been deprecated.</span></span> 
> 
> <span data-ttu-id="70875-105">Sok hasznos példa kísérletek és API-kat az található hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="70875-105">You can find many useful example experiments and APIs in hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="70875-106">Gyűjteményelem hello kapcsolatos további információkért lásd: [megosztás és a Cortana Intelligence Gallery hello erőforrások felderítéséhez](machine-learning-gallery-how-to-use-contribute-publish.md).</span><span class="sxs-lookup"><span data-stu-id="70875-106">For more information about hello Gallery, see [Share and discover resources in hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span></span>

<span data-ttu-id="70875-107">Hogyan lehet mérni a felhasználói vélemények és márkákat vagy online közösségi hálózatokkal, témakörei felé szokások, mint például a visszaküldés Facebook, Twitter-üzeneteket, értékelést, stb.?</span><span class="sxs-lookup"><span data-stu-id="70875-107">How can you measure users’ opinions and attitudes toward brands or topics in online social networks, such as Facebook posts, tweets, reviews, etc.?</span></span> <span data-ttu-id="70875-108">Véleményeket elemzés ilyen kérdések elemzésére szolgáló módszert biztosít.</span><span class="sxs-lookup"><span data-stu-id="70875-108">Sentiment analysis provides a method for analyzing such questions.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<span data-ttu-id="70875-109">Kétféleképpen általában véleményeket elemzés céljából.</span><span class="sxs-lookup"><span data-stu-id="70875-109">There are generally two methods for sentiment analysis.</span></span> <span data-ttu-id="70875-110">Egy felügyelt tanulási algoritmus használ, és más hello felügyeletlen tanulási kell tekinteni.</span><span class="sxs-lookup"><span data-stu-id="70875-110">One is using a supervised learning algorithm, and hello other can be treated as unsupervised learning.</span></span> <span data-ttu-id="70875-111">Felügyelt tanulási algoritmus a besorolási modell általában egy nagy megjegyzésekkel ellátott corpus épít.</span><span class="sxs-lookup"><span data-stu-id="70875-111">A supervised learning algorithm generally builds a classification model on a large annotated corpus.</span></span> <span data-ttu-id="70875-112">A pontosság főként alapul hello jegyzet hello minőségének, és általában hello képzési folyamat hosszú időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="70875-112">Its accuracy is mainly based on hello quality of hello annotation, and usually hello training process will take a long time.</span></span> <span data-ttu-id="70875-113">Amellett, hogy a Microsoft hello algoritmus tooanother tartomány alkalmazásakor hello eredménye általában nem helyes.</span><span class="sxs-lookup"><span data-stu-id="70875-113">Besides that, when we apply hello algorithm tooanother domain, hello result is usually not good.</span></span> <span data-ttu-id="70875-114">Toosupervised tanulási lexicon alapú felügyeletlen tanulási véleményeket szótár, amely nem igényli a tárolása egy nagy méretű adatok corpus használ, és képzési – így az egész folyamat sokkal gyorsabb hello képest.</span><span class="sxs-lookup"><span data-stu-id="70875-114">Compared toosupervised learning, lexicon-based unsupervised learning uses a sentiment dictionary, which doesn’t require storing a large data corpus and training - which makes hello whole process much faster.</span></span> 

<span data-ttu-id="70875-115">A [szolgáltatás](https://datamarket.azure.com/dataset/aml_labs/lexicon_based_sentiment_analysis) hello MPQA szubjektivitás Lexicon (http://mpqa.cs.pitt.edu/lexicons/subj_lexicon/), az egyik leggyakrabban használt hello szubjektivitás lexikonok épül.</span><span class="sxs-lookup"><span data-stu-id="70875-115">Our [service](https://datamarket.azure.com/dataset/aml_labs/lexicon_based_sentiment_analysis) is built on hello MPQA Subjectivity Lexicon (http://mpqa.cs.pitt.edu/lexicons/subj_lexicon/), which is one of hello most commonly used subjectivity lexicons.</span></span> <span data-ttu-id="70875-116">Nincsenek MPQA 5097 negatív és 2533 pozitív szavakat.</span><span class="sxs-lookup"><span data-stu-id="70875-116">There are 5097 negative and 2533 positive words in MPQA.</span></span> <span data-ttu-id="70875-117">És az összes ezeknek a szavaknak vannak leképezésként erős vagy gyenge polaritás.</span><span class="sxs-lookup"><span data-stu-id="70875-117">And all of these words are annotated as strong or weak polarity.</span></span> <span data-ttu-id="70875-118">hello teljes corpus GNU általános nyilvános licenc alatt áll.</span><span class="sxs-lookup"><span data-stu-id="70875-118">hello whole corpus is under GNU General Public License.</span></span> <span data-ttu-id="70875-119">hello webszolgáltatás lehet alkalmazott tooany rövid mondatokat, például a Twitter-üzeneteket és a Facebook-bejegyzéseket.</span><span class="sxs-lookup"><span data-stu-id="70875-119">hello web service can be applied tooany short sentences, such as tweets and Facebook posts.</span></span> 

> <span data-ttu-id="70875-120">Ez a webszolgáltatás kell fenntartania – potenciálisan a mobilalkalmazások a webhelyen keresztül, vagy akár a helyi számítógépen végig a felhasználókat például.</span><span class="sxs-lookup"><span data-stu-id="70875-120">This web service could be consumed by users – potentially through a mobile app, through a website, or even on a local computer for example.</span></span> <span data-ttu-id="70875-121">De hello hello webszolgáltatás célja is tooserve példa bemutatja, hogyan Azure Machine Learning webszolgáltatások használt toocreate fölött R-kód is lehet.</span><span class="sxs-lookup"><span data-stu-id="70875-121">But hello purpose of hello web service is also tooserve as an example of how Azure Machine Learning can be used toocreate web services on top of R code.</span></span> <span data-ttu-id="70875-122">Az R-kód csupán néhány sornyi és az Azure Machine Learning Studio egy gombját kattint egy kísérlet hozható létre az R-kód és webszolgáltatásként közzé.</span><span class="sxs-lookup"><span data-stu-id="70875-122">With just a few lines of R code and clicks of a button within Azure Machine Learning Studio, an experiment can be created with R code and published as a web service.</span></span> <span data-ttu-id="70875-123">hello webszolgáltatás majd lehet közzétett toohello Azure piactéren, és a felhasználók és eszközök által felhasznált között hello world hello Szerző hello webszolgáltatás infrastruktúra beállítás nélkül.</span><span class="sxs-lookup"><span data-stu-id="70875-123">hello web service can then be published toohello Azure Marketplace and consumed by users and devices across hello world with no infrastructure setup by hello author of hello web service.</span></span>
> 
> 

## <a name="consumption-of-web-service"></a><span data-ttu-id="70875-124">Felhasználási webszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="70875-124">Consumption of web service</span></span>
<span data-ttu-id="70875-125">hello bemeneti adatok szöveget, de hello webszolgáltatás jobban rövid mondat működik.</span><span class="sxs-lookup"><span data-stu-id="70875-125">hello input data can be any text, but hello web service works better with short sentences.</span></span> <span data-ttu-id="70875-126">hello kimeneti numerikus értéke -1 és 1 között.</span><span class="sxs-lookup"><span data-stu-id="70875-126">hello output is a numeric value between -1 and 1.</span></span> <span data-ttu-id="70875-127">Bármely alábbi 0 érték azt jelzi, hogy hello véleményeket hello szöveg negatív; Ha a fenti 0 pozitív.</span><span class="sxs-lookup"><span data-stu-id="70875-127">Any value below 0 denotes that hello sentiment of hello text is negative; positive if above 0.</span></span> <span data-ttu-id="70875-128">hello abszolút értékét hello eredmény azt jelzi, hogy hello erősségével hello kapcsolatos véleményeket.</span><span class="sxs-lookup"><span data-stu-id="70875-128">hello absolute value of hello result denotes hello strength of hello associated sentiment.</span></span> 

> <span data-ttu-id="70875-129">Ez a szolgáltatás az Azure piactér hello kihelyezett egy OData-szolgáltatás; a POST vagy GET módszerrel elnevezése lehet.</span><span class="sxs-lookup"><span data-stu-id="70875-129">This service, as hosted on hello Azure Marketplace, is an OData service; these may be called through POST or GET methods.</span></span> 
> 
> 

<span data-ttu-id="70875-130">Többféleképpen is az automatizált módon hello szolgáltatás fel (egy példa alkalmazás [Itt](http://microsoftazuremachinelearning.azurewebsites.net/)).</span><span class="sxs-lookup"><span data-stu-id="70875-130">There are multiple ways of consuming hello service in an automated fashion (an example app is [here](http://microsoftazuremachinelearning.azurewebsites.net/)).</span></span>

### <a name="starting-c-code-for-web-service-consumption"></a><span data-ttu-id="70875-131">C#-kódban a webes szolgáltatások felhasználásához megkezdése:</span><span class="sxs-lookup"><span data-stu-id="70875-131">Starting C# code for web service consumption:</span></span>
    public class ScoreResult
    {
            [DataMember]
            public double result
            {
                get;
                set;
            }
    }

    void main()
    {
            using (var wb = new WebClient())
            {
                var acitionUri = new Uri("PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score");
                DataServiceContext ctx = new DataServiceContext(acitionUri);
                var cred = new NetworkCredential("PutEmailAddressHere", "ChangeToAPIKey");
                var cache = new CredentialCache();

                cache.Add(acitionUri, "Basic", cred);
                ctx.Credentials = cache;
                var query = ctx.Execute<ScoreResult>(acitionUri, "POST", true, new BodyOperationParameter("Text", TextBox1.Text));
                ScoreResult scoreResult = query.ElementAt(0);
                double result = scoreResult.result;
            }
    }



<span data-ttu-id="70875-132">hello bemeneti érték a "Ma egy jó napon."</span><span class="sxs-lookup"><span data-stu-id="70875-132">hello input is “Today is a good day.”</span></span> <span data-ttu-id="70875-133">hello eredménye "1", amely megadja, hogy a bemeneti mondat hello társított pozitív véleményeket.</span><span class="sxs-lookup"><span data-stu-id="70875-133">hello output is “1”, which indicates a positive sentiment associated with hello input sentence.</span></span> 

## <a name="creation-of-web-service"></a><span data-ttu-id="70875-134">Webes szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="70875-134">Creation of web service</span></span>
> <span data-ttu-id="70875-135">Ez a webszolgáltatás Azure Machine Learning segítségével lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="70875-135">This web service was created using Azure Machine Learning.</span></span> <span data-ttu-id="70875-136">Az ingyenes próbaverzió, valamint a bevezető videó kísérletek létrehozásával és [közzétételi webes szolgáltatások](machine-learning-publish-a-machine-learning-web-service.md), lásd: [azure.com/ml](http://azure.com/ml).</span><span class="sxs-lookup"><span data-stu-id="70875-136">For a free trial, as well as introductory videos on creating experiments and [publishing web services](machine-learning-publish-a-machine-learning-web-service.md), please see [azure.com/ml](http://azure.com/ml).</span></span> <span data-ttu-id="70875-137">Az alábbiakban van egy képernyőfelvétel a hello webes szolgáltatás, és példa kódot az egyes hello modulok hello kísérlet belül létrehozott hello kísérlet.</span><span class="sxs-lookup"><span data-stu-id="70875-137">Below is a screenshot of hello experiment that created hello web service and example code for each of hello modules within hello experiment.</span></span>
> 
> 

<span data-ttu-id="70875-138">Azure Machine Learning belül egy új üres kísérlet létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="70875-138">From within Azure Machine Learning, a new blank experiment was created.</span></span> <span data-ttu-id="70875-139">hello az alábbi ábra szemlélteti hello kísérlet lexicon alapú véleményeket elemzési.</span><span class="sxs-lookup"><span data-stu-id="70875-139">hello figure below shows hello experiment flow of lexicon-based sentiment analysis.</span></span> <span data-ttu-id="70875-140">hello "sent_dict.csv" fájl hello MPQA szubjektivitás lexicon, és be van állítva egy hello bevitelének [R-parancsfájl végrehajtása][execute-r-script].</span><span class="sxs-lookup"><span data-stu-id="70875-140">hello “sent_dict.csv” file is hello MPQA subjectivity lexicon, and is set as one of hello inputs of [Execute R Script][execute-r-script].</span></span> <span data-ttu-id="70875-141">Egy másik bemeneti érték a mintában szereplő felülvizsgálati adatkészletből hello Amazon felülvizsgálati vizsgálat, ahol azt végre kijelölés, az oszlop neve módosítását, és ossza fel a műveletek.</span><span class="sxs-lookup"><span data-stu-id="70875-141">Another input is a sampled review from hello Amazon review dataset for test, where we performed selection, column name modification, and split operations.</span></span> <span data-ttu-id="70875-142">Egy kivonatoló csomag toostore hello szubjektivitás lexicon hello memóriában felhasználása és hello pontszám számítási folyamatban érdekében.</span><span class="sxs-lookup"><span data-stu-id="70875-142">We use a hash package toostore hello subjectivity lexicon in hello memory and accelerate hello score computation process.</span></span> <span data-ttu-id="70875-143">hello teljes szöveges a rendszer "tm" csomag tokenekre majd hello word hello véleményeket szótárban képest.</span><span class="sxs-lookup"><span data-stu-id="70875-143">hello whole text will be tokenized by “tm” package and compared with hello word in hello sentiment dictionary.</span></span> <span data-ttu-id="70875-144">Végezetül a pontszám akkor lesz kiszámítva, hello szöveg hello súlyozást az összes szubjektív szó hozzáadásával.</span><span class="sxs-lookup"><span data-stu-id="70875-144">Finally, a score will be calculated by adding hello weight of each subjective word in hello text.</span></span> 

### <a name="experiment-flow"></a><span data-ttu-id="70875-145">Kísérlet folyamata:</span><span class="sxs-lookup"><span data-stu-id="70875-145">Experiment flow:</span></span>
![Kísérlet folyamata][2]

#### <a name="module-1"></a><span data-ttu-id="70875-147">1. modul:</span><span class="sxs-lookup"><span data-stu-id="70875-147">Module 1:</span></span>
    # Map 1-based optional input ports toovariables
    sent_dict_data<- maml.mapInputPort(1) # class: data.frame
    dataset2 <- maml.mapInputPort(2) # class: data.frame

# <a name="install-hash-package"></a><span data-ttu-id="70875-148">Kivonatoló csomag telepítése</span><span class="sxs-lookup"><span data-stu-id="70875-148">Install hash package</span></span>
    install.packages("src/hash_2.2.6.zip", lib = ".", repos = NULL, verbose = TRUE)
    success <- library("hash", lib.loc = ".", logical.return = TRUE, verbose = TRUE)
    library(tm)
    library(stringr)

    #create sentiment dictionary
    negation_word <- c("not","nor", "no")
    result <- c()
    sent_dict <- hash()
    sent_dict <- hash(sent_dict_data$word, sent_dict_data$polarity)

    #  Compute sentiment score for each document
    for (m in 1:nrow(dataset2)){
    polarity_ratio <- 0
    polarity_total <- 0
    not <- 0
    sentence <- tolower(dataset2[m,1])
    if (nchar(sentence) > 0){
        token_array <- scan_tokenizer(sentence)
        for (j in 1:length(token_array)){
            word = str_replace_all(token_array[j], "[^[:alnum:]]", "")
            for (k in 1:length(negation_word)){
              if (word == negation_word[k]){
                not <- (not+1) %% 2

              }
            }
            if (word != ""){
                if (!is.null(sent_dict[[word]])){
                  polarity_ratio <- polarity_ratio + (-2*not+1)*sent_dict[[word]]
                  polarity_total <- polarity_total + abs(sent_dict[[word]])
                }
            }

        }
    }
    if (polarity_total > 0){
        result <- c(result, polarity_ratio/polarity_total)
    }else{
        result<- c(result,0)
    }
    }

    # Sample operation
    data.set <- data.frame(result)

    # Select data.frame toobe sent toohello output Dataset port
    maml.mapOutputPort("data.set")



## <a name="limitations"></a><span data-ttu-id="70875-149">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="70875-149">Limitations</span></span>
<span data-ttu-id="70875-150">Algoritmus szempontból a lexikonban-alapú véleményeket elemzésre egy általános véleményeket elemző eszközt, amely nem adott mezők hello besorolási módszert jobban hajthatja végre.</span><span class="sxs-lookup"><span data-stu-id="70875-150">From an algorithm perspective, lexicon-based sentiment analysis is a general sentiment analysis tool, which may not perform better than hello classification method for specific fields.</span></span> <span data-ttu-id="70875-151">hello tagadásának probléma nem is foglalkozik.</span><span class="sxs-lookup"><span data-stu-id="70875-151">hello negation problem is not well dealt with.</span></span> <span data-ttu-id="70875-152">A Microsoft megoldás több tagadásának jelenti a program, de jobb módja van tagadásának dictionary és néhány szabály hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="70875-152">We hardcode several negation words in our program, but a better way is using a negation dictionary and build some rules.</span></span> <span data-ttu-id="70875-153">hello webszolgáltatás végez jobban a rövid és egyszerű mondatokat, például a Twitter-üzeneteket és a Facebook-bejegyzéseket, mint például az Amazon értékelést hosszú és összetett mondatokat.</span><span class="sxs-lookup"><span data-stu-id="70875-153">hello web service performs better on short and simple sentences, such as tweets and Facebook posts, than on long and complex sentences such as Amazon reviews.</span></span> 

## <a name="faq"></a><span data-ttu-id="70875-154">GYIK</span><span class="sxs-lookup"><span data-stu-id="70875-154">FAQ</span></span>
<span data-ttu-id="70875-155">Gyakori kérdések a felhasználás hello webszolgáltatás vagy az Azure piactér közzétételi toohello, lásd: [Itt](machine-learning-marketplace-faq.md).</span><span class="sxs-lookup"><span data-stu-id="70875-155">For frequently asked questions on consumption of hello web service or publishing toohello Azure Marketplace, see [here](machine-learning-marketplace-faq.md).</span></span>

[1]: ./media/machine-learning-r-csharp-lexicon-based-sentiment-analysis/sentiment_analysis_1.png
[2]: ./media/machine-learning-r-csharp-lexicon-based-sentiment-analysis/sentiment_analysis_2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/


