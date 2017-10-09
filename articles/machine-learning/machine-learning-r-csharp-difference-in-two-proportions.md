---
title: "aaa(deprecated) arányok teszt - Azure különbség |} Microsoft Docs"
description: "(elavult) Különbség arányok teszt"
services: machine-learning
documentationcenter: 
author: aniedea
manager: jhubbard
editor: cgronlun
ms.assetid: 9356b821-5345-44f6-8e26-719f2dea5e6d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: aniedea
ROBOTS: NOINDEX
redirect_url: https://gallery.cortanaintelligence.com/
redirect_document_id: True
ms.openlocfilehash: 820aad377f9dec12b0ef455974aaa95f6e8d723a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deprecated-difference-in-proportions-test"></a><span data-ttu-id="ce426-103">(elavult) Különbség arányok teszt</span><span class="sxs-lookup"><span data-stu-id="ce426-103">(deprecated) Difference in Proportions Test</span></span>

> [!NOTE]
> <span data-ttu-id="ce426-104">a Microsoft DataMarket hello használatból van, és ez az API már elavult.</span><span class="sxs-lookup"><span data-stu-id="ce426-104">hello Microsoft DataMarket is being retired and this API has been deprecated.</span></span> 
> 
> <span data-ttu-id="ce426-105">Sok hasznos példa kísérletek és API-kat az található hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="ce426-105">You can find many useful example experiments and APIs in hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="ce426-106">Gyűjteményelem hello kapcsolatos további információkért lásd: [megosztás és a Cortana Intelligence Gallery hello erőforrások felderítéséhez](machine-learning-gallery-how-to-use-contribute-publish.md).</span><span class="sxs-lookup"><span data-stu-id="ce426-106">For more information about hello Gallery, see [Share and discover resources in hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span></span>

<span data-ttu-id="ce426-107">Eltérőek két arányok statisztikailag?</span><span class="sxs-lookup"><span data-stu-id="ce426-107">Are two proportions statistically different?</span></span> <span data-ttu-id="ce426-108">Tegyük fel, hogy egy felhasználó azt szeretné, ha egy movie jelentősen nagyobb arányban toocompare két filmek toodetermine "kedveli" mikor más toohello képest.</span><span class="sxs-lookup"><span data-stu-id="ce426-108">Suppose a user wants toocompare two movies toodetermine if one movie has a significantly higher proportion of ‘likes’ when compared toohello other.</span></span> <span data-ttu-id="ce426-109">Nagy mintával lehet hello arányok 0,50 és 0,51 statisztikailag jelentős különbsége.</span><span class="sxs-lookup"><span data-stu-id="ce426-109">With a large sample, there could be a statistically significant difference between hello proportions 0.50 and 0.51.</span></span> <span data-ttu-id="ce426-110">A mintát nem lehet elég adat toodetermine Ha ezek arányok ténylegesen eltérőek.</span><span class="sxs-lookup"><span data-stu-id="ce426-110">With a small sample, there may not be enough data toodetermine if these proportions are actually different.</span></span> 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<span data-ttu-id="ce426-111">Ez [webszolgáltatás](https://datamarket.azure.com/dataset/aml_labs/prop_test) végez egy t-próba a hello különbség a két arányban felhasználói bevitel sikeres hello száma és hello hello 2 összehasonlítás csoportok kísérletek teljes száma alapján.</span><span class="sxs-lookup"><span data-stu-id="ce426-111">This [web service](https://datamarket.azure.com/dataset/aml_labs/prop_test) conducts a hypothesis test of hello difference in two proportions based on user input of hello number of successes and hello total number of trials for hello 2 comparison groups.</span></span> <span data-ttu-id="ce426-112">Egy lehetséges esetben ez a webszolgáltatás sikerült hívható meg a movie összehasonlítás alkalmazások szólítja fel hello felhasználót, hogy egy hello filmek van valóban "tetszését" gyakrabban hello más, mint a film minősítése alapján.</span><span class="sxs-lookup"><span data-stu-id="ce426-112">In one possible scenario, this web service could be called from within a movie comparison app, telling hello user whether one of hello movies is really ‘liked’ more often than hello other, based on movie ratings.</span></span>

> <span data-ttu-id="ce426-113">Ez a webszolgáltatás kell fenntartania – potenciálisan végig a mobilalkalmazások a webhelyen keresztül, vagy akár a helyi számítógépen, a felhasználókat például.</span><span class="sxs-lookup"><span data-stu-id="ce426-113">This web service could be consumed by users – potentially through a mobile app, through a website, or even on a local computer, for example.</span></span> <span data-ttu-id="ce426-114">De hello hello webszolgáltatás célja is tooserve példa bemutatja, hogyan Azure Machine Learning webszolgáltatások használt toocreate fölött R-kód is lehet.</span><span class="sxs-lookup"><span data-stu-id="ce426-114">But hello purpose of hello web service is also tooserve as an example of how Azure Machine Learning can be used toocreate web services on top of R code.</span></span> <span data-ttu-id="ce426-115">Az R-kód csupán néhány sornyi és az Azure Machine Learning Studio egy gombját kattint egy kísérlet hozható létre az R-kód és webszolgáltatásként közzé.</span><span class="sxs-lookup"><span data-stu-id="ce426-115">With just a few lines of R code and clicks of a button within Azure Machine Learning Studio, an experiment can be created with R code and published as a web service.</span></span> <span data-ttu-id="ce426-116">hello webszolgáltatás majd lehet közzétett toohello Azure piactéren, és a felhasználók és eszközök által felhasznált között hello world hello Szerző hello webszolgáltatás infrastruktúra beállítás nélkül.</span><span class="sxs-lookup"><span data-stu-id="ce426-116">hello web service can then be published toohello Azure Marketplace and consumed by users and devices across hello world with no infrastructure setup by hello author of hello web service.</span></span>
> 
> 

## <a name="consumption-of-web-service"></a><span data-ttu-id="ce426-117">Felhasználási webszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="ce426-117">Consumption of web service</span></span>
<span data-ttu-id="ce426-118">A szolgáltatás fogadja a 4 argumentum, és nem egy alapul tesztelése a arányok.</span><span class="sxs-lookup"><span data-stu-id="ce426-118">This service accepts 4 arguments and does a hypothesis test of proportions.</span></span>

<span data-ttu-id="ce426-119">hello bemeneti argumentumai a következők:</span><span class="sxs-lookup"><span data-stu-id="ce426-119">hello input arguments are:</span></span>

* <span data-ttu-id="ce426-120">Successes1 - minta 1 sikeres események száma.</span><span class="sxs-lookup"><span data-stu-id="ce426-120">Successes1 - Number of success events in sample 1.</span></span>
* <span data-ttu-id="ce426-121">Successes2 - minta 2 sikeres események száma.</span><span class="sxs-lookup"><span data-stu-id="ce426-121">Successes2 - Number of success events in sample 2.</span></span>
* <span data-ttu-id="ce426-122">Total1 - minta 1 mérete.</span><span class="sxs-lookup"><span data-stu-id="ce426-122">Total1 -  Size of sample 1.</span></span>
* <span data-ttu-id="ce426-123">Total2 - mintát 2.</span><span class="sxs-lookup"><span data-stu-id="ce426-123">Total2 - Size of sample 2.</span></span>

<span data-ttu-id="ce426-124">hello hello szolgáltatás eredménye hello feltevése hello eredményét hello chi-square statisztika, df, p-értékkel együtt tesztelése, és része, a minta 1/2 és a megbízhatósági határain.</span><span class="sxs-lookup"><span data-stu-id="ce426-124">hello output of hello service is hello result of hello hypothesis test along with hello chi-square statistic, df, p-value, and proportion in sample 1/2 and confidence interval bounds.</span></span>

> <span data-ttu-id="ce426-125">Ez a szolgáltatás az Azure piactér hello kihelyezett egy OData-szolgáltatás; a POST vagy GET módszerrel elnevezése lehet.</span><span class="sxs-lookup"><span data-stu-id="ce426-125">This service, as hosted on hello Azure Marketplace, is an OData service; these may be called through POST or GET methods.</span></span> 
> 
> 

<span data-ttu-id="ce426-126">Többféleképpen is az automatizált módon hello szolgáltatás fel (egy példa alkalmazás [Itt](http://microsoftazuremachinelearning.azurewebsites.net/DifferenceInProportionsTest.aspx)).</span><span class="sxs-lookup"><span data-stu-id="ce426-126">There are multiple ways of consuming hello service in an automated fashion (an example app is [here](http://microsoftazuremachinelearning.azurewebsites.net/DifferenceInProportionsTest.aspx)).</span></span>

### <a name="starting-c-code-for-web-service-consumption"></a><span data-ttu-id="ce426-127">C#-kódban a webes szolgáltatások felhasználásához megkezdése:</span><span class="sxs-lookup"><span data-stu-id="ce426-127">Starting C# code for web service consumption:</span></span>
    public class Input
    {
            public string successes1;
            public string successes2;
            public string total1;
            public string total2;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { successes1 = TextBox1.Text, successes2 = TextBox2.Text, total1 = TextBox3.Text, total2 = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();

            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));

            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }


## <a name="creation-of-web-service"></a><span data-ttu-id="ce426-128">Webes szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="ce426-128">Creation of web service</span></span>
> <span data-ttu-id="ce426-129">Ez a webszolgáltatás Azure Machine Learning segítségével lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="ce426-129">This web service was created using Azure Machine Learning.</span></span> <span data-ttu-id="ce426-130">Az ingyenes próbaverzió, valamint a bevezető videó kísérletek létrehozásával és [közzétételi webes szolgáltatások](machine-learning-publish-a-machine-learning-web-service.md), lásd: [azure.com/ml](http://azure.com/ml).</span><span class="sxs-lookup"><span data-stu-id="ce426-130">For a free trial, as well as introductory videos on creating experiments and [publishing web services](machine-learning-publish-a-machine-learning-web-service.md), please see [azure.com/ml](http://azure.com/ml).</span></span> <span data-ttu-id="ce426-131">Az alábbiakban van egy képernyőfelvétel a hello webes szolgáltatás, és példa kódot az egyes hello modulok hello kísérlet belül létrehozott hello kísérlet.</span><span class="sxs-lookup"><span data-stu-id="ce426-131">Below is a screenshot of hello experiment that created hello web service and example code for each of hello modules within hello experiment.</span></span>
> 
> 

<span data-ttu-id="ce426-132">Azure Machine Learning belül egy új üres kísérlet létrehozásához és két [R-parancsfájl végrehajtása] [ execute-r-script] modulok.</span><span class="sxs-lookup"><span data-stu-id="ce426-132">From within Azure Machine Learning, a new blank experiment was created with two [Execute R Script][execute-r-script] modules.</span></span> <span data-ttu-id="ce426-133">Hello első modul hello adatok sémát, miközben a hello második modul 2 arányok hello prop.test parancs R tooperform hello a t-próba belül használja.</span><span class="sxs-lookup"><span data-stu-id="ce426-133">In hello first module hello data schema is defined, while hello second module uses hello prop.test command within R tooperform hello hypothesis test for 2 proportions.</span></span> 

### <a name="experiment-flow"></a><span data-ttu-id="ce426-134">Kísérlet folyamata:</span><span class="sxs-lookup"><span data-stu-id="ce426-134">Experiment flow:</span></span>
![Kísérlet folyamata][2]

#### <a name="module-1"></a><span data-ttu-id="ce426-136">1. modul:</span><span class="sxs-lookup"><span data-stu-id="ce426-136">Module 1:</span></span>
    ####Schema definition  
    data.set=data.frame(successes1=50,successes2=60,total1=100,total2=100);
    maml.mapOutputPort("data.set"); #send data toooutput port
    dataset1 = maml.mapInputPort(1) #read data from input port


#### <a name="module-2"></a><span data-ttu-id="ce426-137">A modul 2:</span><span class="sxs-lookup"><span data-stu-id="ce426-137">Module 2:</span></span>
    test=prop.test(c(dataset1$successes1[1],dataset1$successes2[1]),c(dataset1$total1[1],dataset1$total2[1])) #conduct hypothesis test

    result=data.frame(
    result=ifelse(test$p.value<0.05,"hello proportions are different!",
    "hello proportions aren't different statistically."),
    ChiSquarestatistic=round(test$statistic,2),df=test$parameter,
    pvalue=round(test$p.value,4),
    proportion1=round(test$estimate[1],4),
    proportion2=round(test$estimate[2],4),
    confintlow=round(test$conf.int[1],4),
    confinthigh=round(test$conf.int[2],4)) 

    maml.mapOutputPort("result"); #output port


## <a name="limitations"></a><span data-ttu-id="ce426-138">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="ce426-138">Limitations</span></span>
<span data-ttu-id="ce426-139">Ez az eltérés 2 arányban tesztjének egy nagyon egyszerű példa.</span><span class="sxs-lookup"><span data-stu-id="ce426-139">This is a very simple example for a test of difference in 2 proportions.</span></span> <span data-ttu-id="ce426-140">Hello példakódot fent is látható, mert nincs hiba alatt van megvalósítva, és hello szolgáltatás feltételezi, hogy minden hello változók folyamatos.</span><span class="sxs-lookup"><span data-stu-id="ce426-140">As can be seen from hello example code above, no error catching is implemented and hello service assumes that all hello variables are continuous.</span></span>

## <a name="faq"></a><span data-ttu-id="ce426-141">GYIK</span><span class="sxs-lookup"><span data-stu-id="ce426-141">FAQ</span></span>
<span data-ttu-id="ce426-142">Gyakori kérdések a felhasználás hello webszolgáltatás vagy az Azure piactér közzétételi toohello, lásd: [Itt](machine-learning-marketplace-faq.md).</span><span class="sxs-lookup"><span data-stu-id="ce426-142">For frequently asked questions on consumption of hello web service or publishing toohello Azure Marketplace, see [here](machine-learning-marketplace-faq.md).</span></span>

[1]: ./media/machine-learning-r-csharp-difference-in-two-proportions/hyptest-img1.png
[2]: ./media/machine-learning-r-csharp-difference-in-two-proportions/hyptest-img2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

