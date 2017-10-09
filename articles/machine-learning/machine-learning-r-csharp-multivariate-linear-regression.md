---
title: "aaa(deprecated) Multivariate lineáris regressziós - Azure |} Microsoft Docs"
description: "(elavult) Multivariate lineáris regressziós"
services: machine-learning
documentationcenter: 
author: jaymathe
manager: jhubbard
editor: cgronlun
ms.assetid: 2fb78220-ced9-4564-a439-9e5df6772994
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: jaymathe
ROBOTS: NOINDEX
redirect_url: https://gallery.cortanaintelligence.com/
redirect_document_id: True
ms.openlocfilehash: 0ff7221cd06c0ef059b0c5bf327016588174dcfe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deprecated-multivariate-linear-regression"></a><span data-ttu-id="9b189-103">(elavult) Multivariate lineáris regressziós</span><span class="sxs-lookup"><span data-stu-id="9b189-103">(deprecated) Multivariate Linear Regression</span></span>

> [!NOTE]
> <span data-ttu-id="9b189-104">a Microsoft DataMarket hello használatból van, és ez az API már elavult.</span><span class="sxs-lookup"><span data-stu-id="9b189-104">hello Microsoft DataMarket is being retired and this API has been deprecated.</span></span> 
> 
> <span data-ttu-id="9b189-105">Sok hasznos példa kísérletek és API-kat az található hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="9b189-105">You can find many useful example experiments and APIs in hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="9b189-106">Gyűjteményelem hello kapcsolatos további információkért lásd: [megosztás és a Cortana Intelligence Gallery hello erőforrások felderítéséhez](machine-learning-gallery-how-to-use-contribute-publish.md).</span><span class="sxs-lookup"><span data-stu-id="9b189-106">For more information about hello Gallery, see [Share and discover resources in hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span></span>

<span data-ttu-id="9b189-107">Tegyük fel egy adatkészlet rendelkezik, és például tooquickly előrejelzése lenne a függő változó (y), a független változók alapján minden egyes (i).</span><span class="sxs-lookup"><span data-stu-id="9b189-107">Suppose you have a dataset and would like tooquickly predict a dependent variable (y) for each individual (i) based on independent variables.</span></span> <span data-ttu-id="9b189-108">Lineáris regressziós a népszerű statisztikai technika ilyen előrejelzéseket használatos.</span><span class="sxs-lookup"><span data-stu-id="9b189-108">Linear regression is a popular statistical technique used for such predictions.</span></span> <span data-ttu-id="9b189-109">Itt hello függő változó y toobe folyamatos értéket feltételezi.</span><span class="sxs-lookup"><span data-stu-id="9b189-109">Here hello dependent variable y is assumed toobe a continuous value.</span></span>  

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<span data-ttu-id="9b189-110">Egy egyszerű forgatókönyv lehet, ahol hello biztonságkutatói próbál toopredict hello súlya egy adott (y), a magasságát (x) alapján.</span><span class="sxs-lookup"><span data-stu-id="9b189-110">A simple scenario could be where hello researcher is trying toopredict hello weight of an individual (y) based on their height (x).</span></span> <span data-ttu-id="9b189-111">Ennél összetettebb környezetben oka lehet, ahol a hello biztonságkutatói további információk a hello egyes (például a súly, a nemét, a fajta) rendelkezik, és megpróbál toopredict hello súly hello egyéni.</span><span class="sxs-lookup"><span data-stu-id="9b189-111">A more advanced scenario could be where hello researcher has additional information for hello individual (such as weight, gender, race) and attempts toopredict hello weight of hello individual.</span></span> <span data-ttu-id="9b189-112">Ez [webszolgáltatás](https://datamarket.azure.com/dataset/aml_labs/multivariate_regression) elfér hello lineáris regressziós modell toohello adatai és kimenetek hello előre jelzett érték (y) az egyes hello megfigyelések hello adataiban.</span><span class="sxs-lookup"><span data-stu-id="9b189-112">This [web service](https://datamarket.azure.com/dataset/aml_labs/multivariate_regression) fits hello linear regression model toohello data and outputs hello predicted value (y) for each of hello observations in hello data.</span></span>

> <span data-ttu-id="9b189-113">Ez a webszolgáltatás kell fenntartania – potenciálisan végig a mobilalkalmazások a webhelyen keresztül, vagy akár a helyi számítógépen, a felhasználókat például.</span><span class="sxs-lookup"><span data-stu-id="9b189-113">This web service could be consumed by users – potentially through a mobile app, through a website, or even on a local computer, for example.</span></span> <span data-ttu-id="9b189-114">De hello hello webszolgáltatás célja is tooserve példa bemutatja, hogyan Azure Machine Learning webszolgáltatások használt toocreate fölött R-kód is lehet.</span><span class="sxs-lookup"><span data-stu-id="9b189-114">But hello purpose of hello web service is also tooserve as an example of how Azure Machine Learning can be used toocreate web services on top of R code.</span></span> <span data-ttu-id="9b189-115">Az R-kód csupán néhány sornyi és az Azure Machine Learning Studio egy gombját kattint egy kísérlet hozható létre az R-kód és webszolgáltatásként közzé.</span><span class="sxs-lookup"><span data-stu-id="9b189-115">With just a few lines of R code and clicks of a button within Azure Machine Learning Studio, an experiment can be created with R code and published as a web service.</span></span> <span data-ttu-id="9b189-116">hello webszolgáltatás majd lehet közzétett toohello Azure piactéren, és a felhasználók és eszközök által felhasznált között hello world hello Szerző hello webszolgáltatás infrastruktúra beállítás nélkül.</span><span class="sxs-lookup"><span data-stu-id="9b189-116">hello web service can then be published toohello Azure Marketplace and consumed by users and devices across hello world with no infrastructure setup by hello author of hello web service.</span></span>  
> 
> 

## <a name="consumption-of-web-service"></a><span data-ttu-id="9b189-117">Felhasználási webszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="9b189-117">Consumption of web service</span></span>
<span data-ttu-id="9b189-118">A webes szolgáltatás által biztosított hello értékek hello függő változó összes hello megfigyelések hello független változók alapján előre jelezni.</span><span class="sxs-lookup"><span data-stu-id="9b189-118">This web service gives hello predicted values of hello dependent variable based on hello independent variables for all of hello observations.</span></span> <span data-ttu-id="9b189-119">hello webszolgáltatás hello végfelhasználói tooinput adatok vár, ahol sorok egymástól vesszővel (,) válassza el egymástól, és oszlopok pontosvesszővel (;) elválasztott karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="9b189-119">hello web service expects hello end user tooinput data as a string where rows are separated by commas (,) and columns are separated by semicolons (;).</span></span> <span data-ttu-id="9b189-120">hello webszolgáltatás egyszerre 1 sor vár, és hello első oszlop toobe hello függő változó vár.</span><span class="sxs-lookup"><span data-stu-id="9b189-120">hello web service expects 1 row at a time and expects hello first column toobe hello dependent variable.</span></span> <span data-ttu-id="9b189-121">Egy példa adatkészlet nézhet ki:</span><span class="sxs-lookup"><span data-stu-id="9b189-121">An example dataset could look like this:</span></span>

![Mintaadatok][1]

<span data-ttu-id="9b189-123">Megfigyelések egy függő változó nélkül y kell megadnia, "NA"KARAKTERLÁNCOT.</span><span class="sxs-lookup"><span data-stu-id="9b189-123">Observations without a dependent variable should be input as “NA” for y.</span></span> <span data-ttu-id="9b189-124">hello adatokat adjon meg a fenti dataset hello volna kell hello a következő karakterláncot: "10; 5; 2,18; 1; 6,6; 5.3-as; 2.1,7; 5; 5,22; 3; 4,12; 2; 1, NA; 3; 4".</span><span class="sxs-lookup"><span data-stu-id="9b189-124">hello data input for hello above dataset would be hello following string: “10;5;2,18;1;6,6;5.3;2.1,7;5;5,22;3;4,12;2;1,NA;3;4”.</span></span> <span data-ttu-id="9b189-125">a kimeneti hello van hello előre jelzett érték az egyes hello sorok hello független változók alapján.</span><span class="sxs-lookup"><span data-stu-id="9b189-125">hello output is hello predicted value for each of hello rows based on hello independent variables.</span></span> 

> <span data-ttu-id="9b189-126">Ez a szolgáltatás az Azure piactér hello kihelyezett egy OData-szolgáltatás; a POST vagy GET módszerrel elnevezése lehet.</span><span class="sxs-lookup"><span data-stu-id="9b189-126">This service, as hosted on hello Azure Marketplace, is an OData service; these may be called through POST or GET methods.</span></span> 
> 
> 

<span data-ttu-id="9b189-127">Többféleképpen is az automatizált módon hello szolgáltatás fel (egy példa alkalmazás [Itt](http://microsoftazuremachinelearning.azurewebsites.net/MultipleLinearRegressionService.aspx)).</span><span class="sxs-lookup"><span data-stu-id="9b189-127">There are multiple ways of consuming hello service in an automated fashion (an example app is [here](http://microsoftazuremachinelearning.azurewebsites.net/MultipleLinearRegressionService.aspx)).</span></span>

### <a name="starting-c-code-for-web-service-consumption"></a><span data-ttu-id="9b189-128">C#-kódban a webes szolgáltatások felhasználásához megkezdése:</span><span class="sxs-lookup"><span data-stu-id="9b189-128">Starting C# code for web service consumption:</span></span>
    public class Input
    {
            public string value;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { value = TextBox1.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();

            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));

            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }




## <a name="creation-of-web-service"></a><span data-ttu-id="9b189-129">Webes szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="9b189-129">Creation of web service</span></span>
> <span data-ttu-id="9b189-130">Ez a webszolgáltatás Azure Machine Learning segítségével lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="9b189-130">This web service was created using Azure Machine Learning.</span></span> <span data-ttu-id="9b189-131">Az ingyenes próbaverzió, valamint a bevezető videó kísérletek létrehozásával és [közzétételi webes szolgáltatások](machine-learning-publish-a-machine-learning-web-service.md), lásd: [azure.com/ml](http://azure.com/ml).</span><span class="sxs-lookup"><span data-stu-id="9b189-131">For a free trial, as well as introductory videos on creating experiments and [publishing web services](machine-learning-publish-a-machine-learning-web-service.md), please see [azure.com/ml](http://azure.com/ml).</span></span> <span data-ttu-id="9b189-132">Az alábbiakban van egy képernyőfelvétel a hello webes szolgáltatás, és példa kódot az egyes hello modulok hello kísérlet belül létrehozott hello kísérlet.</span><span class="sxs-lookup"><span data-stu-id="9b189-132">Below is a screenshot of hello experiment that created hello web service and example code for each of hello modules within hello experiment.</span></span>
> 
> 

<span data-ttu-id="9b189-133">Azure Machine Learning belül egy új üres kísérlet létrehozásához és két [R-parancsfájl végrehajtása] [ execute-r-script] modulok lekért hello munkaterület-kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="9b189-133">From within Azure Machine Learning, a new blank experiment was created and two [Execute R Script][execute-r-script] modules were pulled onto hello workspace.</span></span> <span data-ttu-id="9b189-134">Ez a webszolgáltatás egy Azure Machine Learning kísérlet fut, alapul szolgáló R-parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="9b189-134">This web service runs an Azure Machine Learning experiment with an underlying R script.</span></span> <span data-ttu-id="9b189-135">Nincsenek a 2 részek toothis kísérletezhet: schema definíció jelenik meg, és a tanítási modell + pontozási.</span><span class="sxs-lookup"><span data-stu-id="9b189-135">There are 2 parts toothis experiment: schema definition, and training model + scoring.</span></span> <span data-ttu-id="9b189-136">hello első modul várt hello struktúrát hello bemeneti adatkészlet, ahol a hello első változó hello függő változó és hello fennmaradó változók független határozza meg.</span><span class="sxs-lookup"><span data-stu-id="9b189-136">hello first module defines hello expected structure of hello input dataset, where hello first variable is hello dependent variable and hello remaining variables are independent.</span></span> <span data-ttu-id="9b189-137">hello második modul megfelel egy általános lineáris regressziós modellt hello bemeneti adatok.</span><span class="sxs-lookup"><span data-stu-id="9b189-137">hello second module fits a generic linear regression model for hello input data.</span></span>  

![Kísérlet folyamata][3]

#### <a name="module-1"></a><span data-ttu-id="9b189-139">1. modul:</span><span class="sxs-lookup"><span data-stu-id="9b189-139">Module 1:</span></span>
#### <a name="schema-definition"></a><span data-ttu-id="9b189-140">Sémadefiníciót</span><span class="sxs-lookup"><span data-stu-id="9b189-140">Schema definition</span></span>
    data <- data.frame(value = "1;2;3,4;5;6,7;8;9", stringsAsFactors=FALSE) maml.mapOutputPort("data");  

#### <a name="module-2"></a><span data-ttu-id="9b189-141">A modul 2:</span><span class="sxs-lookup"><span data-stu-id="9b189-141">Module 2:</span></span>
#### <a name="lm-modeling"></a><span data-ttu-id="9b189-142">Az LM modellezési</span><span class="sxs-lookup"><span data-stu-id="9b189-142">LM modeling</span></span>
    data <- maml.mapInputPort(1) # class: data.frame  

    data.split <- strsplit(data[1,1], ",")[[1]]  
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)  
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)  
    data.split <- as.data.frame(t(data.split)) 
    data.split <- data.matrix(data.split) 
    data.split <- data.frame(data.split) 
    model <- lm(data.split)  

    out=data.frame(predict(model,data.split))  
    out <- data.frame(t(out))

    maml.mapOutputPort("out");  

## <a name="limitations"></a><span data-ttu-id="9b189-143">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="9b189-143">Limitations</span></span>
<span data-ttu-id="9b189-144">Itt látható egy nagyon egyszerű példa egy lineáris regressziós több webszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="9b189-144">This is a very simple example of a multiple linear regression web service.</span></span> <span data-ttu-id="9b189-145">Hello példakódot fent is látható, mert nincs hiba alatt van megvalósítva és hello szolgáltatás feltételezi, hogy minden rendben (ahol nincs kategorikus funkció engedélyezett), a folyamatos változó hello szolgáltatást csak bemenetek numerikus értékként hello létrehozását webhely hello helyreállításkor a szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="9b189-145">As can be seen from hello example code above, no error catching is implemented and hello service assumes everything is a continuous variable (no categorical features allowed), as hello service only inputs numeric values at hello time of hello creation of this web service.</span></span> <span data-ttu-id="9b189-146">Is hello szolgáltatás jelenleg korlátozott adatméret kezeli, miatt toohello kérelem-válasz jellegű hello webszolgáltatás hívás és hello tényt, hogy a modell hello alatt alkalmas minden alkalommal, amikor hello webszolgáltatás nevezik.</span><span class="sxs-lookup"><span data-stu-id="9b189-146">Also, hello service currently handles limited data size, due toohello request/response nature of hello web service call and hello fact that hello model is being fit every time hello web service is called.</span></span> 

## <a name="faq"></a><span data-ttu-id="9b189-147">GYIK</span><span class="sxs-lookup"><span data-stu-id="9b189-147">FAQ</span></span>
<span data-ttu-id="9b189-148">Gyakori kérdések a felhasználás hello webszolgáltatás vagy az Azure piactér közzétételi toohello, lásd: [Itt](machine-learning-marketplace-faq.md).</span><span class="sxs-lookup"><span data-stu-id="9b189-148">For frequently asked questions on consumption of hello web service or publishing toohello Azure Marketplace, see [here](machine-learning-marketplace-faq.md).</span></span>

[1]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img1.png
[2]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img2.png
[3]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

