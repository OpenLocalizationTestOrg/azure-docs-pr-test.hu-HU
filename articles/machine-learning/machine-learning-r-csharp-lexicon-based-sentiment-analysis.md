---
title: "(elavult) Lexicon véleményeket Analysis - alapú Azure |} Microsoft Docs"
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
redirect_document_id: TRUE
ms.openlocfilehash: 7bc80a1e1067296528eca1a843ea30b0c27af616
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="deprecated-lexicon-based-sentiment-analysis"></a><span data-ttu-id="9ca07-103">(elavult) Lexicon alapú véleményeket elemzés</span><span class="sxs-lookup"><span data-stu-id="9ca07-103">(deprecated) Lexicon Based Sentiment Analysis</span></span>

> [!NOTE]
> <span data-ttu-id="9ca07-104">A Microsoft DataMarket használatból van, és ez az API már elavult.</span><span class="sxs-lookup"><span data-stu-id="9ca07-104">The Microsoft DataMarket is being retired and this API has been deprecated.</span></span> 
> 
> <span data-ttu-id="9ca07-105">Sok hasznos példa kísérletek és API-k a [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="9ca07-105">You can find many useful example experiments and APIs in the [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="9ca07-106">A gyűjtemény kapcsolatos további információkért lásd: [megosztást, és felderítik a Cortana Intelligence Gallery erőforrások](machine-learning-gallery-how-to-use-contribute-publish.md).</span><span class="sxs-lookup"><span data-stu-id="9ca07-106">For more information about the Gallery, see [Share and discover resources in the Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span></span>

<span data-ttu-id="9ca07-107">Hogyan lehet mérni a felhasználói vélemények és márkákat vagy online közösségi hálózatokkal, témakörei felé szokások, mint például a visszaküldés Facebook, Twitter-üzeneteket, értékelést, stb.?</span><span class="sxs-lookup"><span data-stu-id="9ca07-107">How can you measure users’ opinions and attitudes toward brands or topics in online social networks, such as Facebook posts, tweets, reviews, etc.?</span></span> <span data-ttu-id="9ca07-108">Véleményeket elemzés ilyen kérdések elemzésére szolgáló módszert biztosít.</span><span class="sxs-lookup"><span data-stu-id="9ca07-108">Sentiment analysis provides a method for analyzing such questions.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<span data-ttu-id="9ca07-109">Kétféleképpen általában véleményeket elemzés céljából.</span><span class="sxs-lookup"><span data-stu-id="9ca07-109">There are generally two methods for sentiment analysis.</span></span> <span data-ttu-id="9ca07-110">Egy felügyelt tanulási algoritmus használ, és a másik felügyeletlen tanulás képes kezelni.</span><span class="sxs-lookup"><span data-stu-id="9ca07-110">One is using a supervised learning algorithm, and the other can be treated as unsupervised learning.</span></span> <span data-ttu-id="9ca07-111">Felügyelt tanulási algoritmus a besorolási modell általában egy nagy megjegyzésekkel ellátott corpus épít.</span><span class="sxs-lookup"><span data-stu-id="9ca07-111">A supervised learning algorithm generally builds a classification model on a large annotated corpus.</span></span> <span data-ttu-id="9ca07-112">Pontosságát főként a jegyzet minőségének alapul, és általában a képzési folyamat hosszú időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="9ca07-112">Its accuracy is mainly based on the quality of the annotation, and usually the training process will take a long time.</span></span> <span data-ttu-id="9ca07-113">Amellett, hogy ha egy másik tartományra, az algoritmus érvénybe lépni eredménye általában nem helyes.</span><span class="sxs-lookup"><span data-stu-id="9ca07-113">Besides that, when we apply the algorithm to another domain, the result is usually not good.</span></span> <span data-ttu-id="9ca07-114">Felügyelt tanulási, lexicon alapú felügyeletlen tanulás által használt véleményeket szótár, amely nem igényli a tárolása egy nagy méretű adatok corpus és képzési képest – így az egész folyamat sokkal gyorsabb.</span><span class="sxs-lookup"><span data-stu-id="9ca07-114">Compared to supervised learning, lexicon-based unsupervised learning uses a sentiment dictionary, which doesn’t require storing a large data corpus and training - which makes the whole process much faster.</span></span> 

<span data-ttu-id="9ca07-115">A [szolgáltatás](https://datamarket.azure.com/dataset/aml_labs/lexicon_based_sentiment_analysis) a MPQA szubjektivitás Lexicon (http://mpqa.cs.pitt.edu/lexicons/subj_lexicon/), amely a leggyakrabban használt szubjektivitás lexikonok egyike épül.</span><span class="sxs-lookup"><span data-stu-id="9ca07-115">Our [service](https://datamarket.azure.com/dataset/aml_labs/lexicon_based_sentiment_analysis) is built on the MPQA Subjectivity Lexicon (http://mpqa.cs.pitt.edu/lexicons/subj_lexicon/), which is one of the most commonly used subjectivity lexicons.</span></span> <span data-ttu-id="9ca07-116">Nincsenek MPQA 5097 negatív és 2533 pozitív szavakat.</span><span class="sxs-lookup"><span data-stu-id="9ca07-116">There are 5097 negative and 2533 positive words in MPQA.</span></span> <span data-ttu-id="9ca07-117">És az összes ezeknek a szavaknak vannak leképezésként erős vagy gyenge polaritás.</span><span class="sxs-lookup"><span data-stu-id="9ca07-117">And all of these words are annotated as strong or weak polarity.</span></span> <span data-ttu-id="9ca07-118">A teljes corpus GNU általános nyilvános licenc alatt áll.</span><span class="sxs-lookup"><span data-stu-id="9ca07-118">The whole corpus is under GNU General Public License.</span></span> <span data-ttu-id="9ca07-119">A webszolgáltatás alkalmazhatók minden rövid mondatokat, például a Twitter-üzeneteket és a Facebook-bejegyzéseket.</span><span class="sxs-lookup"><span data-stu-id="9ca07-119">The web service can be applied to any short sentences, such as tweets and Facebook posts.</span></span> 

> <span data-ttu-id="9ca07-120">Ez a webszolgáltatás kell fenntartania – potenciálisan a mobilalkalmazások a webhelyen keresztül, vagy akár a helyi számítógépen végig a felhasználókat például.</span><span class="sxs-lookup"><span data-stu-id="9ca07-120">This web service could be consumed by users – potentially through a mobile app, through a website, or even on a local computer for example.</span></span> <span data-ttu-id="9ca07-121">De a webszolgáltatás célja is példa bemutatja, hogyan Azure Machine Learning webszolgáltatások fölött R-kód létrehozásához használható kiszolgálásához.</span><span class="sxs-lookup"><span data-stu-id="9ca07-121">But the purpose of the web service is also to serve as an example of how Azure Machine Learning can be used to create web services on top of R code.</span></span> <span data-ttu-id="9ca07-122">Az R-kód csupán néhány sornyi és az Azure Machine Learning Studio egy gombját kattint egy kísérlet hozható létre az R-kód és webszolgáltatásként közzé.</span><span class="sxs-lookup"><span data-stu-id="9ca07-122">With just a few lines of R code and clicks of a button within Azure Machine Learning Studio, an experiment can be created with R code and published as a web service.</span></span> <span data-ttu-id="9ca07-123">A webszolgáltatás majd közzé az Azure piactéren, és felhasználók és eszközök által felhasznált világszerte a szerző, a webszolgáltatás által infrastruktúra beállítás nélkül.</span><span class="sxs-lookup"><span data-stu-id="9ca07-123">The web service can then be published to the Azure Marketplace and consumed by users and devices across the world with no infrastructure setup by the author of the web service.</span></span>
> 
> 

## <a name="consumption-of-web-service"></a><span data-ttu-id="9ca07-124">Felhasználási webszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="9ca07-124">Consumption of web service</span></span>
<span data-ttu-id="9ca07-125">A bemeneti adatok szöveget, de a webszolgáltatás jobban rövid mondat működik.</span><span class="sxs-lookup"><span data-stu-id="9ca07-125">The input data can be any text, but the web service works better with short sentences.</span></span> <span data-ttu-id="9ca07-126">Egy 1 és 1 közötti számértéknek eredménye.</span><span class="sxs-lookup"><span data-stu-id="9ca07-126">The output is a numeric value between -1 and 1.</span></span> <span data-ttu-id="9ca07-127">Bármely alábbi 0 érték azt jelzi, hogy a szöveg a céggel kapcsolatos véleményeket negatív; Ha a fenti 0 pozitív.</span><span class="sxs-lookup"><span data-stu-id="9ca07-127">Any value below 0 denotes that the sentiment of the text is negative; positive if above 0.</span></span> <span data-ttu-id="9ca07-128">Az eredmény abszolút értéke azt jelzi, hogy a kapcsolódó véleményeket erősségével.</span><span class="sxs-lookup"><span data-stu-id="9ca07-128">The absolute value of the result denotes the strength of the associated sentiment.</span></span> 

> <span data-ttu-id="9ca07-129">Ez a szolgáltatás az Azure piactéren kihelyezett egy OData-szolgáltatás; a POST vagy GET módszerrel elnevezése lehet.</span><span class="sxs-lookup"><span data-stu-id="9ca07-129">This service, as hosted on the Azure Marketplace, is an OData service; these may be called through POST or GET methods.</span></span> 
> 
> 

<span data-ttu-id="9ca07-130">Többféleképpen is az automatizált módon a szolgáltatás fel (egy példa alkalmazás [Itt](http://microsoftazuremachinelearning.azurewebsites.net/)).</span><span class="sxs-lookup"><span data-stu-id="9ca07-130">There are multiple ways of consuming the service in an automated fashion (an example app is [here](http://microsoftazuremachinelearning.azurewebsites.net/)).</span></span>

### <a name="starting-c-code-for-web-service-consumption"></a><span data-ttu-id="9ca07-131">C#-kódban a webes szolgáltatások felhasználásához megkezdése:</span><span class="sxs-lookup"><span data-stu-id="9ca07-131">Starting C# code for web service consumption:</span></span>
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



<span data-ttu-id="9ca07-132">A bemeneti érték a "Ma egy jó napon."</span><span class="sxs-lookup"><span data-stu-id="9ca07-132">The input is “Today is a good day.”</span></span> <span data-ttu-id="9ca07-133">A kimeneti értéke "1", amely megadja, hogy a bemeneti mondat társított egy pozitív véleményeket.</span><span class="sxs-lookup"><span data-stu-id="9ca07-133">The output is “1”, which indicates a positive sentiment associated with the input sentence.</span></span> 

## <a name="creation-of-web-service"></a><span data-ttu-id="9ca07-134">Webes szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="9ca07-134">Creation of web service</span></span>
> <span data-ttu-id="9ca07-135">Ez a webszolgáltatás Azure Machine Learning segítségével lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="9ca07-135">This web service was created using Azure Machine Learning.</span></span> <span data-ttu-id="9ca07-136">Az ingyenes próbaverzió, valamint a bevezető videó kísérletek létrehozásával és [közzétételi webes szolgáltatások](machine-learning-publish-a-machine-learning-web-service.md), lásd: [azure.com/ml](http://azure.com/ml).</span><span class="sxs-lookup"><span data-stu-id="9ca07-136">For a free trial, as well as introductory videos on creating experiments and [publishing web services](machine-learning-publish-a-machine-learning-web-service.md), please see [azure.com/ml](http://azure.com/ml).</span></span> <span data-ttu-id="9ca07-137">Az alábbiakban van egy Képernyőkép a kísérlet, amely a webes szolgáltatás, és példa kód létre minden egyes belül modulok.</span><span class="sxs-lookup"><span data-stu-id="9ca07-137">Below is a screenshot of the experiment that created the web service and example code for each of the modules within the experiment.</span></span>
> 
> 

<span data-ttu-id="9ca07-138">Azure Machine Learning belül egy új üres kísérlet létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="9ca07-138">From within Azure Machine Learning, a new blank experiment was created.</span></span> <span data-ttu-id="9ca07-139">Az alábbi ábra szemlélteti a kísérlet lexicon alapú véleményeket elemzési.</span><span class="sxs-lookup"><span data-stu-id="9ca07-139">The figure below shows the experiment flow of lexicon-based sentiment analysis.</span></span> <span data-ttu-id="9ca07-140">A "sent_dict.csv" fájl MPQA szubjektivitás lexikonban, és az adatokat a rendelkezésre álló [R-parancsfájl végrehajtása][execute-r-script].</span><span class="sxs-lookup"><span data-stu-id="9ca07-140">The “sent_dict.csv” file is the MPQA subjectivity lexicon, and is set as one of the inputs of [Execute R Script][execute-r-script].</span></span> <span data-ttu-id="9ca07-141">Egy másik bemeneti érték a mintában szereplő tekintse át a vizsgálat, ahol azt végre kijelölés, az oszlop neve módosítását, és ossza fel a műveletek Amazon felülvizsgálati adatkészletből.</span><span class="sxs-lookup"><span data-stu-id="9ca07-141">Another input is a sampled review from the Amazon review dataset for test, where we performed selection, column name modification, and split operations.</span></span> <span data-ttu-id="9ca07-142">Egy kivonatoló csomag szubjektivitás lexikonban tárolása a memória, valamint a pontszám számítási folyamatban érdekében használjuk.</span><span class="sxs-lookup"><span data-stu-id="9ca07-142">We use a hash package to store the subjectivity lexicon in the memory and accelerate the score computation process.</span></span> <span data-ttu-id="9ca07-143">A teljes szöveges a rendszer "tm" csomag tokenekre, majd a word, a céggel kapcsolatos véleményeket szótárban képest.</span><span class="sxs-lookup"><span data-stu-id="9ca07-143">The whole text will be tokenized by “tm” package and compared with the word in the sentiment dictionary.</span></span> <span data-ttu-id="9ca07-144">Végezetül pontszámot akkor lesz kiszámítva, a szöveg minden szubjektív szó súlya hozzáadásával.</span><span class="sxs-lookup"><span data-stu-id="9ca07-144">Finally, a score will be calculated by adding the weight of each subjective word in the text.</span></span> 

### <a name="experiment-flow"></a><span data-ttu-id="9ca07-145">Kísérlet folyamata:</span><span class="sxs-lookup"><span data-stu-id="9ca07-145">Experiment flow:</span></span>
![Kísérlet folyamata][2]

#### <a name="module-1"></a><span data-ttu-id="9ca07-147">1. modul:</span><span class="sxs-lookup"><span data-stu-id="9ca07-147">Module 1:</span></span>
    # Map 1-based optional input ports to variables
    sent_dict_data<- maml.mapInputPort(1) # class: data.frame
    dataset2 <- maml.mapInputPort(2) # class: data.frame

# <a name="install-hash-package"></a><span data-ttu-id="9ca07-148">Kivonatoló csomag telepítése</span><span class="sxs-lookup"><span data-stu-id="9ca07-148">Install hash package</span></span>
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

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("data.set")



## <a name="limitations"></a><span data-ttu-id="9ca07-149">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="9ca07-149">Limitations</span></span>
<span data-ttu-id="9ca07-150">Algoritmus szempontból a lexikonban-alapú véleményeket elemzésre egy általános véleményeket elemző eszközt, amely nem az adott mezők besorolási módszert jobban hajthatja végre.</span><span class="sxs-lookup"><span data-stu-id="9ca07-150">From an algorithm perspective, lexicon-based sentiment analysis is a general sentiment analysis tool, which may not perform better than the classification method for specific fields.</span></span> <span data-ttu-id="9ca07-151">A negálás problémát nem is foglalkozik.</span><span class="sxs-lookup"><span data-stu-id="9ca07-151">The negation problem is not well dealt with.</span></span> <span data-ttu-id="9ca07-152">A Microsoft megoldás több tagadásának jelenti a program, de jobb módja van tagadásának dictionary és néhány szabály hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="9ca07-152">We hardcode several negation words in our program, but a better way is using a negation dictionary and build some rules.</span></span> <span data-ttu-id="9ca07-153">A webszolgáltatás végez jobban a rövid és egyszerű mondatokat, például a Twitter-üzeneteket és a Facebook-bejegyzéseket, mint például az Amazon értékelést hosszú és összetett mondatokat.</span><span class="sxs-lookup"><span data-stu-id="9ca07-153">The web service performs better on short and simple sentences, such as tweets and Facebook posts, than on long and complex sentences such as Amazon reviews.</span></span> 

## <a name="faq"></a><span data-ttu-id="9ca07-154">GYIK</span><span class="sxs-lookup"><span data-stu-id="9ca07-154">FAQ</span></span>
<span data-ttu-id="9ca07-155">Gyakori kérdések a felhasználás a webszolgáltatás vagy az Azure piactéren közzétételt, lásd: [Itt](machine-learning-marketplace-faq.md).</span><span class="sxs-lookup"><span data-stu-id="9ca07-155">For frequently asked questions on consumption of the web service or publishing to the Azure Marketplace, see [here](machine-learning-marketplace-faq.md).</span></span>

[1]: ./media/machine-learning-r-csharp-lexicon-based-sentiment-analysis/sentiment_analysis_1.png
[2]: ./media/machine-learning-r-csharp-lexicon-based-sentiment-analysis/sentiment_analysis_2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/


