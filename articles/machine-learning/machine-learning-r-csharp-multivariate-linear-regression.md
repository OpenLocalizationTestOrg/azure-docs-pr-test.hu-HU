---
title: "(elavult) Multivariate lineáris regressziós - Azure |} Microsoft Docs"
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
redirect_document_id: TRUE
ms.openlocfilehash: 65a8005139e920cd19593e954fc1bf836354bdf3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="deprecated-multivariate-linear-regression"></a><span data-ttu-id="66ce9-103">(elavult) Multivariate lineáris regressziós</span><span class="sxs-lookup"><span data-stu-id="66ce9-103">(deprecated) Multivariate Linear Regression</span></span>

> [!NOTE]
> <span data-ttu-id="66ce9-104">A Microsoft DataMarket használatból van, és ez az API már elavult.</span><span class="sxs-lookup"><span data-stu-id="66ce9-104">The Microsoft DataMarket is being retired and this API has been deprecated.</span></span> 
> 
> <span data-ttu-id="66ce9-105">Sok hasznos példa kísérletek és API-k a [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="66ce9-105">You can find many useful example experiments and APIs in the [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="66ce9-106">A gyűjtemény kapcsolatos további információkért lásd: [megosztást, és felderítik a Cortana Intelligence Gallery erőforrások](machine-learning-gallery-how-to-use-contribute-publish.md).</span><span class="sxs-lookup"><span data-stu-id="66ce9-106">For more information about the Gallery, see [Share and discover resources in the Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span></span>

<span data-ttu-id="66ce9-107">Tegyük fel egy adatkészlet rendelkezik, és szeretné gyorsan előrejelzése egy függő változó (y), a független változók alapján minden egyes (i).</span><span class="sxs-lookup"><span data-stu-id="66ce9-107">Suppose you have a dataset and would like to quickly predict a dependent variable (y) for each individual (i) based on independent variables.</span></span> <span data-ttu-id="66ce9-108">Lineáris regressziós a népszerű statisztikai technika ilyen előrejelzéseket használatos.</span><span class="sxs-lookup"><span data-stu-id="66ce9-108">Linear regression is a popular statistical technique used for such predictions.</span></span> <span data-ttu-id="66ce9-109">Itt a függő változó y érték folytonos érték.</span><span class="sxs-lookup"><span data-stu-id="66ce9-109">Here the dependent variable y is assumed to be a continuous value.</span></span>  

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<span data-ttu-id="66ce9-110">Egy egyszerű forgatókönyv lehet, ahol a biztonságkutatói próbál a súlyozási egy adott (y), a magasságát (x) alapján előre jelezni.</span><span class="sxs-lookup"><span data-stu-id="66ce9-110">A simple scenario could be where the researcher is trying to predict the weight of an individual (y) based on their height (x).</span></span> <span data-ttu-id="66ce9-111">Ennél összetettebb környezetben oka lehet, ahol a biztonságkutatói további információ az egyes (például a súly, a nemét, a fajta) rendelkezik, és előre jelezni az egyes súlya megkísérli.</span><span class="sxs-lookup"><span data-stu-id="66ce9-111">A more advanced scenario could be where the researcher has additional information for the individual (such as weight, gender, race) and attempts to predict the weight of the individual.</span></span> <span data-ttu-id="66ce9-112">Ez [webszolgáltatás](https://datamarket.azure.com/dataset/aml_labs/multivariate_regression) megfelel a lineáris regressziós modellt az adatokat, és kiírja az előre jelzett érték (y) minden az adatok a megfigyelt.</span><span class="sxs-lookup"><span data-stu-id="66ce9-112">This [web service](https://datamarket.azure.com/dataset/aml_labs/multivariate_regression) fits the linear regression model to the data and outputs the predicted value (y) for each of the observations in the data.</span></span>

> <span data-ttu-id="66ce9-113">Ez a webszolgáltatás kell fenntartania – potenciálisan végig a mobilalkalmazások a webhelyen keresztül, vagy akár a helyi számítógépen, a felhasználókat például.</span><span class="sxs-lookup"><span data-stu-id="66ce9-113">This web service could be consumed by users – potentially through a mobile app, through a website, or even on a local computer, for example.</span></span> <span data-ttu-id="66ce9-114">De a webszolgáltatás célja is példa bemutatja, hogyan Azure Machine Learning webszolgáltatások fölött R-kód létrehozásához használható kiszolgálásához.</span><span class="sxs-lookup"><span data-stu-id="66ce9-114">But the purpose of the web service is also to serve as an example of how Azure Machine Learning can be used to create web services on top of R code.</span></span> <span data-ttu-id="66ce9-115">Az R-kód csupán néhány sornyi és az Azure Machine Learning Studio egy gombját kattint egy kísérlet hozható létre az R-kód és webszolgáltatásként közzé.</span><span class="sxs-lookup"><span data-stu-id="66ce9-115">With just a few lines of R code and clicks of a button within Azure Machine Learning Studio, an experiment can be created with R code and published as a web service.</span></span> <span data-ttu-id="66ce9-116">A webszolgáltatás majd közzé az Azure piactéren, és felhasználók és eszközök által felhasznált világszerte a szerző, a webszolgáltatás által infrastruktúra beállítás nélkül.</span><span class="sxs-lookup"><span data-stu-id="66ce9-116">The web service can then be published to the Azure Marketplace and consumed by users and devices across the world with no infrastructure setup by the author of the web service.</span></span>  
> 
> 

## <a name="consumption-of-web-service"></a><span data-ttu-id="66ce9-117">Felhasználási webszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="66ce9-117">Consumption of web service</span></span>
<span data-ttu-id="66ce9-118">Ennek a webszolgáltatásnak ad az összes megfigyelések független változók alapján függő változó előre jelzett értékek.</span><span class="sxs-lookup"><span data-stu-id="66ce9-118">This web service gives the predicted values of the dependent variable based on the independent variables for all of the observations.</span></span> <span data-ttu-id="66ce9-119">A webszolgáltatás vár a felhasználó a bemeneti adatok karakterláncként, ahol sorok egymástól vesszővel (,) válassza el egymástól, és oszlopok egymástól pontosvesszővel (;).</span><span class="sxs-lookup"><span data-stu-id="66ce9-119">The web service expects the end user to input data as a string where rows are separated by commas (,) and columns are separated by semicolons (;).</span></span> <span data-ttu-id="66ce9-120">A webszolgáltatás egyszerre 1 sor vár, és a függő változó az első oszlopot vár.</span><span class="sxs-lookup"><span data-stu-id="66ce9-120">The web service expects 1 row at a time and expects the first column to be the dependent variable.</span></span> <span data-ttu-id="66ce9-121">Egy példa adatkészlet nézhet ki:</span><span class="sxs-lookup"><span data-stu-id="66ce9-121">An example dataset could look like this:</span></span>

![Mintaadatok][1]

<span data-ttu-id="66ce9-123">Megfigyelések egy függő változó nélkül y kell megadnia, "NA"KARAKTERLÁNCOT.</span><span class="sxs-lookup"><span data-stu-id="66ce9-123">Observations without a dependent variable should be input as “NA” for y.</span></span> <span data-ttu-id="66ce9-124">Az adatok, adjon meg, mert a fenti adatkészlet a következő karakterláncot: "10; 5; 2,18; 1; 6,6; 5.3-as; 2.1,7; 5; 5,22; 3; 4,12; 2; 1, NA; 3, 4".</span><span class="sxs-lookup"><span data-stu-id="66ce9-124">The data input for the above dataset would be the following string: “10;5;2,18;1;6,6;5.3;2.1,7;5;5,22;3;4,12;2;1,NA;3;4”.</span></span> <span data-ttu-id="66ce9-125">A kimeneti az előre jelzett érték az egyes a sorok, a független változók alapján.</span><span class="sxs-lookup"><span data-stu-id="66ce9-125">The output is the predicted value for each of the rows based on the independent variables.</span></span> 

> <span data-ttu-id="66ce9-126">Ez a szolgáltatás az Azure piactéren kihelyezett egy OData-szolgáltatás; a POST vagy GET módszerrel elnevezése lehet.</span><span class="sxs-lookup"><span data-stu-id="66ce9-126">This service, as hosted on the Azure Marketplace, is an OData service; these may be called through POST or GET methods.</span></span> 
> 
> 

<span data-ttu-id="66ce9-127">Többféleképpen is az automatizált módon a szolgáltatás fel (egy példa alkalmazás [Itt](http://microsoftazuremachinelearning.azurewebsites.net/MultipleLinearRegressionService.aspx)).</span><span class="sxs-lookup"><span data-stu-id="66ce9-127">There are multiple ways of consuming the service in an automated fashion (an example app is [here](http://microsoftazuremachinelearning.azurewebsites.net/MultipleLinearRegressionService.aspx)).</span></span>

### <a name="starting-c-code-for-web-service-consumption"></a><span data-ttu-id="66ce9-128">C#-kódban a webes szolgáltatások felhasználásához megkezdése:</span><span class="sxs-lookup"><span data-stu-id="66ce9-128">Starting C# code for web service consumption:</span></span>
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




## <a name="creation-of-web-service"></a><span data-ttu-id="66ce9-129">Webes szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="66ce9-129">Creation of web service</span></span>
> <span data-ttu-id="66ce9-130">Ez a webszolgáltatás Azure Machine Learning segítségével lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="66ce9-130">This web service was created using Azure Machine Learning.</span></span> <span data-ttu-id="66ce9-131">Az ingyenes próbaverzió, valamint a bevezető videó kísérletek létrehozásával és [közzétételi webes szolgáltatások](machine-learning-publish-a-machine-learning-web-service.md), lásd: [azure.com/ml](http://azure.com/ml).</span><span class="sxs-lookup"><span data-stu-id="66ce9-131">For a free trial, as well as introductory videos on creating experiments and [publishing web services](machine-learning-publish-a-machine-learning-web-service.md), please see [azure.com/ml](http://azure.com/ml).</span></span> <span data-ttu-id="66ce9-132">Az alábbiakban van egy Képernyőkép a kísérlet, amely a webes szolgáltatás, és példa kód létre minden egyes belül modulok.</span><span class="sxs-lookup"><span data-stu-id="66ce9-132">Below is a screenshot of the experiment that created the web service and example code for each of the modules within the experiment.</span></span>
> 
> 

<span data-ttu-id="66ce9-133">Azure Machine Learning belül egy új üres kísérlet létrehozásához és két [R-parancsfájl végrehajtása] [ execute-r-script] modulok lekért a munkaterület-kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="66ce9-133">From within Azure Machine Learning, a new blank experiment was created and two [Execute R Script][execute-r-script] modules were pulled onto the workspace.</span></span> <span data-ttu-id="66ce9-134">Ez a webszolgáltatás egy Azure Machine Learning kísérlet fut, alapul szolgáló R-parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="66ce9-134">This web service runs an Azure Machine Learning experiment with an underlying R script.</span></span> <span data-ttu-id="66ce9-135">Ehhez a kísérlethez 2 részből áll: schema definíció jelenik meg, és a tanítási modell + pontozási.</span><span class="sxs-lookup"><span data-stu-id="66ce9-135">There are 2 parts to this experiment: schema definition, and training model + scoring.</span></span> <span data-ttu-id="66ce9-136">Az első modul határozza meg a bemeneti adatkészlet, ahol az első változó a függő változó, és a fennmaradó változók függetlenek a várt szerkezetnek.</span><span class="sxs-lookup"><span data-stu-id="66ce9-136">The first module defines the expected structure of the input dataset, where the first variable is the dependent variable and the remaining variables are independent.</span></span> <span data-ttu-id="66ce9-137">A második modul megfelel a bemeneti adatok általános lineáris regressziós modellt.</span><span class="sxs-lookup"><span data-stu-id="66ce9-137">The second module fits a generic linear regression model for the input data.</span></span>  

![Kísérlet folyamata][3]

#### <a name="module-1"></a><span data-ttu-id="66ce9-139">1. modul:</span><span class="sxs-lookup"><span data-stu-id="66ce9-139">Module 1:</span></span>
#### <a name="schema-definition"></a><span data-ttu-id="66ce9-140">Sémadefiníciót</span><span class="sxs-lookup"><span data-stu-id="66ce9-140">Schema definition</span></span>
    data <- data.frame(value = "1;2;3,4;5;6,7;8;9", stringsAsFactors=FALSE) maml.mapOutputPort("data");  

#### <a name="module-2"></a><span data-ttu-id="66ce9-141">A modul 2:</span><span class="sxs-lookup"><span data-stu-id="66ce9-141">Module 2:</span></span>
#### <a name="lm-modeling"></a><span data-ttu-id="66ce9-142">Az LM modellezési</span><span class="sxs-lookup"><span data-stu-id="66ce9-142">LM modeling</span></span>
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

## <a name="limitations"></a><span data-ttu-id="66ce9-143">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="66ce9-143">Limitations</span></span>
<span data-ttu-id="66ce9-144">Itt látható egy nagyon egyszerű példa egy lineáris regressziós több webszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="66ce9-144">This is a very simple example of a multiple linear regression web service.</span></span> <span data-ttu-id="66ce9-145">A fenti példa kódot is látható, mert nincs hiba alatt van megvalósítva, és a szolgáltatás azt feltételezi, hogy minden folyamatos változó (ahol nincs kategorikus funkció engedélyezett), a szolgáltatás csak bemenetek numerikus értékként ennek a webszolgáltatásnak létrehozásának időpontjában.</span><span class="sxs-lookup"><span data-stu-id="66ce9-145">As can be seen from the example code above, no error catching is implemented and the service assumes everything is a continuous variable (no categorical features allowed), as the service only inputs numeric values at the time of the creation of this web service.</span></span> <span data-ttu-id="66ce9-146">Emellett a szolgáltatás jelenleg kezeli korlátozott adatok mérete, kellő a webszolgáltatás-hívások és azt a tényt, hogy a modell alatt alkalmas a webes szolgáltatás neve minden egyes kérelem/válasz jellegének.</span><span class="sxs-lookup"><span data-stu-id="66ce9-146">Also, the service currently handles limited data size, due to the request/response nature of the web service call and the fact that the model is being fit every time the web service is called.</span></span> 

## <a name="faq"></a><span data-ttu-id="66ce9-147">GYIK</span><span class="sxs-lookup"><span data-stu-id="66ce9-147">FAQ</span></span>
<span data-ttu-id="66ce9-148">Gyakori kérdések a felhasználás a webszolgáltatás vagy az Azure piactéren közzétételt, lásd: [Itt](machine-learning-marketplace-faq.md).</span><span class="sxs-lookup"><span data-stu-id="66ce9-148">For frequently asked questions on consumption of the web service or publishing to the Azure Marketplace, see [here](machine-learning-marketplace-faq.md).</span></span>

[1]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img1.png
[2]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img2.png
[3]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

