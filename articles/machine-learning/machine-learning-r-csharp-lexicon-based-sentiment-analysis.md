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
# <a name="deprecated-lexicon-based-sentiment-analysis"></a>(elavult) Lexicon alapú véleményeket elemzés

> [!NOTE]
> a Microsoft DataMarket hello használatból van, és ez az API már elavult. 
> 
> Sok hasznos példa kísérletek és API-kat az található hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com). Gyűjteményelem hello kapcsolatos további információkért lásd: [megosztás és a Cortana Intelligence Gallery hello erőforrások felderítéséhez](machine-learning-gallery-how-to-use-contribute-publish.md).

Hogyan lehet mérni a felhasználói vélemények és márkákat vagy online közösségi hálózatokkal, témakörei felé szokások, mint például a visszaküldés Facebook, Twitter-üzeneteket, értékelést, stb.? Véleményeket elemzés ilyen kérdések elemzésére szolgáló módszert biztosít.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Kétféleképpen általában véleményeket elemzés céljából. Egy felügyelt tanulási algoritmus használ, és más hello felügyeletlen tanulási kell tekinteni. Felügyelt tanulási algoritmus a besorolási modell általában egy nagy megjegyzésekkel ellátott corpus épít. A pontosság főként alapul hello jegyzet hello minőségének, és általában hello képzési folyamat hosszú időt vesz igénybe. Amellett, hogy a Microsoft hello algoritmus tooanother tartomány alkalmazásakor hello eredménye általában nem helyes. Toosupervised tanulási lexicon alapú felügyeletlen tanulási véleményeket szótár, amely nem igényli a tárolása egy nagy méretű adatok corpus használ, és képzési – így az egész folyamat sokkal gyorsabb hello képest. 

A [szolgáltatás](https://datamarket.azure.com/dataset/aml_labs/lexicon_based_sentiment_analysis) hello MPQA szubjektivitás Lexicon (http://mpqa.cs.pitt.edu/lexicons/subj_lexicon/), az egyik leggyakrabban használt hello szubjektivitás lexikonok épül. Nincsenek MPQA 5097 negatív és 2533 pozitív szavakat. És az összes ezeknek a szavaknak vannak leképezésként erős vagy gyenge polaritás. hello teljes corpus GNU általános nyilvános licenc alatt áll. hello webszolgáltatás lehet alkalmazott tooany rövid mondatokat, például a Twitter-üzeneteket és a Facebook-bejegyzéseket. 

> Ez a webszolgáltatás kell fenntartania – potenciálisan a mobilalkalmazások a webhelyen keresztül, vagy akár a helyi számítógépen végig a felhasználókat például. De hello hello webszolgáltatás célja is tooserve példa bemutatja, hogyan Azure Machine Learning webszolgáltatások használt toocreate fölött R-kód is lehet. Az R-kód csupán néhány sornyi és az Azure Machine Learning Studio egy gombját kattint egy kísérlet hozható létre az R-kód és webszolgáltatásként közzé. hello webszolgáltatás majd lehet közzétett toohello Azure piactéren, és a felhasználók és eszközök által felhasznált között hello world hello Szerző hello webszolgáltatás infrastruktúra beállítás nélkül.
> 
> 

## <a name="consumption-of-web-service"></a>Felhasználási webszolgáltatás
hello bemeneti adatok szöveget, de hello webszolgáltatás jobban rövid mondat működik. hello kimeneti numerikus értéke -1 és 1 között. Bármely alábbi 0 érték azt jelzi, hogy hello véleményeket hello szöveg negatív; Ha a fenti 0 pozitív. hello abszolút értékét hello eredmény azt jelzi, hogy hello erősségével hello kapcsolatos véleményeket. 

> Ez a szolgáltatás az Azure piactér hello kihelyezett egy OData-szolgáltatás; a POST vagy GET módszerrel elnevezése lehet. 
> 
> 

Többféleképpen is az automatizált módon hello szolgáltatás fel (egy példa alkalmazás [Itt](http://microsoftazuremachinelearning.azurewebsites.net/)).

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



hello bemeneti érték a "Ma egy jó napon." hello eredménye "1", amely megadja, hogy a bemeneti mondat hello társított pozitív véleményeket. 

## <a name="creation-of-web-service"></a>Webes szolgáltatás létrehozása
> Ez a webszolgáltatás Azure Machine Learning segítségével lett létrehozva. Az ingyenes próbaverzió, valamint a bevezető videó kísérletek létrehozásával és [közzétételi webes szolgáltatások](machine-learning-publish-a-machine-learning-web-service.md), lásd: [azure.com/ml](http://azure.com/ml). Az alábbiakban van egy képernyőfelvétel a hello webes szolgáltatás, és példa kódot az egyes hello modulok hello kísérlet belül létrehozott hello kísérlet.
> 
> 

Azure Machine Learning belül egy új üres kísérlet létrehozásához. hello az alábbi ábra szemlélteti hello kísérlet lexicon alapú véleményeket elemzési. hello "sent_dict.csv" fájl hello MPQA szubjektivitás lexicon, és be van állítva egy hello bevitelének [R-parancsfájl végrehajtása][execute-r-script]. Egy másik bemeneti érték a mintában szereplő felülvizsgálati adatkészletből hello Amazon felülvizsgálati vizsgálat, ahol azt végre kijelölés, az oszlop neve módosítását, és ossza fel a műveletek. Egy kivonatoló csomag toostore hello szubjektivitás lexicon hello memóriában felhasználása és hello pontszám számítási folyamatban érdekében. hello teljes szöveges a rendszer "tm" csomag tokenekre majd hello word hello véleményeket szótárban képest. Végezetül a pontszám akkor lesz kiszámítva, hello szöveg hello súlyozást az összes szubjektív szó hozzáadásával. 

### <a name="experiment-flow"></a>Kísérlet folyamata:
![Kísérlet folyamata][2]

#### <a name="module-1"></a>1. modul:
    # Map 1-based optional input ports toovariables
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

    # Select data.frame toobe sent toohello output Dataset port
    maml.mapOutputPort("data.set")



## <a name="limitations"></a>Korlátozások
Algoritmus szempontból a lexikonban-alapú véleményeket elemzésre egy általános véleményeket elemző eszközt, amely nem adott mezők hello besorolási módszert jobban hajthatja végre. hello tagadásának probléma nem is foglalkozik. A Microsoft megoldás több tagadásának jelenti a program, de jobb módja van tagadásának dictionary és néhány szabály hozhat létre. hello webszolgáltatás végez jobban a rövid és egyszerű mondatokat, például a Twitter-üzeneteket és a Facebook-bejegyzéseket, mint például az Amazon értékelést hosszú és összetett mondatokat. 

## <a name="faq"></a>GYIK
Gyakori kérdések a felhasználás hello webszolgáltatás vagy az Azure piactér közzétételi toohello, lásd: [Itt](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-lexicon-based-sentiment-analysis/sentiment_analysis_1.png
[2]: ./media/machine-learning-r-csharp-lexicon-based-sentiment-analysis/sentiment_analysis_2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/


