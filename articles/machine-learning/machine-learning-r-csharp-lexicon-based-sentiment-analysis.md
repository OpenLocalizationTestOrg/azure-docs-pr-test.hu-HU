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
# <a name="deprecated-lexicon-based-sentiment-analysis"></a>(elavult) Lexicon alapú véleményeket elemzés

> [!NOTE]
> A Microsoft DataMarket használatból van, és ez az API már elavult. 
> 
> Sok hasznos példa kísérletek és API-k a [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com). A gyűjtemény kapcsolatos további információkért lásd: [megosztást, és felderítik a Cortana Intelligence Gallery erőforrások](machine-learning-gallery-how-to-use-contribute-publish.md).

Hogyan lehet mérni a felhasználói vélemények és márkákat vagy online közösségi hálózatokkal, témakörei felé szokások, mint például a visszaküldés Facebook, Twitter-üzeneteket, értékelést, stb.? Véleményeket elemzés ilyen kérdések elemzésére szolgáló módszert biztosít.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Kétféleképpen általában véleményeket elemzés céljából. Egy felügyelt tanulási algoritmus használ, és a másik felügyeletlen tanulás képes kezelni. Felügyelt tanulási algoritmus a besorolási modell általában egy nagy megjegyzésekkel ellátott corpus épít. Pontosságát főként a jegyzet minőségének alapul, és általában a képzési folyamat hosszú időt vesz igénybe. Amellett, hogy ha egy másik tartományra, az algoritmus érvénybe lépni eredménye általában nem helyes. Felügyelt tanulási, lexicon alapú felügyeletlen tanulás által használt véleményeket szótár, amely nem igényli a tárolása egy nagy méretű adatok corpus és képzési képest – így az egész folyamat sokkal gyorsabb. 

A [szolgáltatás](https://datamarket.azure.com/dataset/aml_labs/lexicon_based_sentiment_analysis) a MPQA szubjektivitás Lexicon (http://mpqa.cs.pitt.edu/lexicons/subj_lexicon/), amely a leggyakrabban használt szubjektivitás lexikonok egyike épül. Nincsenek MPQA 5097 negatív és 2533 pozitív szavakat. És az összes ezeknek a szavaknak vannak leképezésként erős vagy gyenge polaritás. A teljes corpus GNU általános nyilvános licenc alatt áll. A webszolgáltatás alkalmazhatók minden rövid mondatokat, például a Twitter-üzeneteket és a Facebook-bejegyzéseket. 

> Ez a webszolgáltatás kell fenntartania – potenciálisan a mobilalkalmazások a webhelyen keresztül, vagy akár a helyi számítógépen végig a felhasználókat például. De a webszolgáltatás célja is példa bemutatja, hogyan Azure Machine Learning webszolgáltatások fölött R-kód létrehozásához használható kiszolgálásához. Az R-kód csupán néhány sornyi és az Azure Machine Learning Studio egy gombját kattint egy kísérlet hozható létre az R-kód és webszolgáltatásként közzé. A webszolgáltatás majd közzé az Azure piactéren, és felhasználók és eszközök által felhasznált világszerte a szerző, a webszolgáltatás által infrastruktúra beállítás nélkül.
> 
> 

## <a name="consumption-of-web-service"></a>Felhasználási webszolgáltatás
A bemeneti adatok szöveget, de a webszolgáltatás jobban rövid mondat működik. Egy 1 és 1 közötti számértéknek eredménye. Bármely alábbi 0 érték azt jelzi, hogy a szöveg a céggel kapcsolatos véleményeket negatív; Ha a fenti 0 pozitív. Az eredmény abszolút értéke azt jelzi, hogy a kapcsolódó véleményeket erősségével. 

> Ez a szolgáltatás az Azure piactéren kihelyezett egy OData-szolgáltatás; a POST vagy GET módszerrel elnevezése lehet. 
> 
> 

Többféleképpen is az automatizált módon a szolgáltatás fel (egy példa alkalmazás [Itt](http://microsoftazuremachinelearning.azurewebsites.net/)).

### <a name="starting-c-code-for-web-service-consumption"></a>C#-kódban a webes szolgáltatások felhasználásához megkezdése:
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



A bemeneti érték a "Ma egy jó napon." A kimeneti értéke "1", amely megadja, hogy a bemeneti mondat társított egy pozitív véleményeket. 

## <a name="creation-of-web-service"></a>Webes szolgáltatás létrehozása
> Ez a webszolgáltatás Azure Machine Learning segítségével lett létrehozva. Az ingyenes próbaverzió, valamint a bevezető videó kísérletek létrehozásával és [közzétételi webes szolgáltatások](machine-learning-publish-a-machine-learning-web-service.md), lásd: [azure.com/ml](http://azure.com/ml). Az alábbiakban van egy Képernyőkép a kísérlet, amely a webes szolgáltatás, és példa kód létre minden egyes belül modulok.
> 
> 

Azure Machine Learning belül egy új üres kísérlet létrehozásához. Az alábbi ábra szemlélteti a kísérlet lexicon alapú véleményeket elemzési. A "sent_dict.csv" fájl MPQA szubjektivitás lexikonban, és az adatokat a rendelkezésre álló [R-parancsfájl végrehajtása][execute-r-script]. Egy másik bemeneti érték a mintában szereplő tekintse át a vizsgálat, ahol azt végre kijelölés, az oszlop neve módosítását, és ossza fel a műveletek Amazon felülvizsgálati adatkészletből. Egy kivonatoló csomag szubjektivitás lexikonban tárolása a memória, valamint a pontszám számítási folyamatban érdekében használjuk. A teljes szöveges a rendszer "tm" csomag tokenekre, majd a word, a céggel kapcsolatos véleményeket szótárban képest. Végezetül pontszámot akkor lesz kiszámítva, a szöveg minden szubjektív szó súlya hozzáadásával. 

### <a name="experiment-flow"></a>Kísérlet folyamata:
![Kísérlet folyamata][2]

#### <a name="module-1"></a>1. modul:
    # Map 1-based optional input ports to variables
    sent_dict_data<- maml.mapInputPort(1) # class: data.frame
    dataset2 <- maml.mapInputPort(2) # class: data.frame

# <a name="install-hash-package"></a>Kivonatoló csomag telepítése
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



## <a name="limitations"></a>Korlátozások
Algoritmus szempontból a lexikonban-alapú véleményeket elemzésre egy általános véleményeket elemző eszközt, amely nem az adott mezők besorolási módszert jobban hajthatja végre. A negálás problémát nem is foglalkozik. A Microsoft megoldás több tagadásának jelenti a program, de jobb módja van tagadásának dictionary és néhány szabály hozhat létre. A webszolgáltatás végez jobban a rövid és egyszerű mondatokat, például a Twitter-üzeneteket és a Facebook-bejegyzéseket, mint például az Amazon értékelést hosszú és összetett mondatokat. 

## <a name="faq"></a>GYIK
Gyakori kérdések a felhasználás a webszolgáltatás vagy az Azure piactéren közzétételt, lásd: [Itt](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-lexicon-based-sentiment-analysis/sentiment_analysis_1.png
[2]: ./media/machine-learning-r-csharp-lexicon-based-sentiment-analysis/sentiment_analysis_2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/


