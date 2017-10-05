---
title: "(elavult) Bináris osztályozó - Azure |} Microsoft Docs"
description: "(elavult) Bináris osztályozó"
services: machine-learning
documentationcenter: 
author: jaymathe
manager: jhubbard
editor: cgronlun
ms.assetid: 8045038a-9dcf-44b9-a6de-7f1f8e791575
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
ms.openlocfilehash: 1a83392f90bb5a9fb183334c03ccec20dd3f3520
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="deprecated-binary-classifier"></a><span data-ttu-id="6fa6f-103">(elavult) Bináris osztályozó</span><span class="sxs-lookup"><span data-stu-id="6fa6f-103">(deprecated) Binary Classifier</span></span>

> [!NOTE]
> <span data-ttu-id="6fa6f-104">A Microsoft DataMarket használatból van, és ez az API már elavult.</span><span class="sxs-lookup"><span data-stu-id="6fa6f-104">The Microsoft DataMarket is being retired and this API has been deprecated.</span></span> 
> 
> <span data-ttu-id="6fa6f-105">Sok hasznos példa kísérletek és API-k a [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="6fa6f-105">You can find many useful example experiments and APIs in the [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="6fa6f-106">A gyűjtemény kapcsolatos további információkért lásd: [megosztást, és felderítik a Cortana Intelligence Gallery erőforrások](machine-learning-gallery-how-to-use-contribute-publish.md).</span><span class="sxs-lookup"><span data-stu-id="6fa6f-106">For more information about the Gallery, see [Share and discover resources in the Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span></span>

<span data-ttu-id="6fa6f-107">Tegyük fel, hogy egy adatkészlet rendelkezik, és szeretné egy bináris függő változó a független változók alapján előre jelezni.</span><span class="sxs-lookup"><span data-stu-id="6fa6f-107">Suppose you have a dataset and would like to predict a binary dependent variable based on the independent variables.</span></span> <span data-ttu-id="6fa6f-108">"Logisztikai regresszió" a népszerű statisztikai technika ilyen előrejelzéseket használatos.</span><span class="sxs-lookup"><span data-stu-id="6fa6f-108">‘Logistic Regression’ is a popular statistical technique used for such predictions.</span></span> <span data-ttu-id="6fa6f-109">Itt a függő változó bináris vagy dichotomous, és p az egyik fontos jellemzője jelenléte valószínűségét.</span><span class="sxs-lookup"><span data-stu-id="6fa6f-109">Here the dependent variable is binary or dichotomous, and p is the probability of presence of the characteristic of interest.</span></span> 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<span data-ttu-id="6fa6f-110">Egy egyszerű forgatókönyv lehet, ahol egy biztonságkutatói próbál előre jelezni, hogy rendelkezik leendő student várhatóan fogadja el a egyetemi (középiskolai, termékcsalád bevétel, rezidens állapot GPA nemek közötti) adatok alapján történő beléptetésre ajánlatot.</span><span class="sxs-lookup"><span data-stu-id="6fa6f-110">A simple scenario could be where a researcher is trying to predict whether a prospective student is likely to accept an admission offer to a university based on information (GPA in high school, family income, resident state, gender).</span></span> <span data-ttu-id="6fa6f-111">Az előre jelzett eredménye a potenciális student a beléptetésre ajánlat elfogadása valószínűségét.</span><span class="sxs-lookup"><span data-stu-id="6fa6f-111">The predicted outcome is the probability of the prospective student accepting the admission offer.</span></span> <span data-ttu-id="6fa6f-112">Ez [webszolgáltatás](https://datamarket.azure.com/dataset/aml_labs/log_regression) megfelel a logisztikai regresszió modellt az adatokat, és kiírja a valószínűségi érték (y) minden az adatok a megfigyelt.</span><span class="sxs-lookup"><span data-stu-id="6fa6f-112">This [web service](https://datamarket.azure.com/dataset/aml_labs/log_regression) fits the logistic regression model to the data and outputs the probability value (y) for each of the observations in the data.</span></span>  

> <span data-ttu-id="6fa6f-113">Ez a webszolgáltatás kell fenntartania – potenciálisan végig a mobilalkalmazások a webhelyen keresztül, vagy akár a helyi számítógépen, a felhasználókat például.</span><span class="sxs-lookup"><span data-stu-id="6fa6f-113">This web service could be consumed by users – potentially through a mobile app, through a website, or even on a local computer, for example.</span></span> <span data-ttu-id="6fa6f-114">De a webszolgáltatás célja is példa bemutatja, hogyan Azure Machine Learning webszolgáltatások fölött R-kód létrehozásához használható kiszolgálásához.</span><span class="sxs-lookup"><span data-stu-id="6fa6f-114">But the purpose of the web service is also to serve as an example of how Azure Machine Learning can be used to create web services on top of R code.</span></span> <span data-ttu-id="6fa6f-115">Az R-kód csupán néhány sornyi és az Azure Machine Learning Studio egy gombját kattint egy kísérlet hozható létre az R-kód és webszolgáltatásként közzé.</span><span class="sxs-lookup"><span data-stu-id="6fa6f-115">With just a few lines of R code and clicks of a button within Azure Machine Learning Studio, an experiment can be created with R code and published as a web service.</span></span> <span data-ttu-id="6fa6f-116">A webszolgáltatás majd közzé az Azure piactéren, és felhasználók és eszközök által felhasznált világszerte a szerző, a webszolgáltatás által infrastruktúra beállítás nélkül.</span><span class="sxs-lookup"><span data-stu-id="6fa6f-116">The web service can then be published to the Azure Marketplace and consumed by users and devices across the world with no infrastructure setup by the author of the web service.</span></span>  
> 
> 

## <a name="consumption-of-web-service"></a><span data-ttu-id="6fa6f-117">Felhasználási webszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="6fa6f-117">Consumption of web service</span></span>
<span data-ttu-id="6fa6f-118">Ennek a webszolgáltatásnak ad az összes megfigyelések független változók alapján függő változó előre jelzett értékek.</span><span class="sxs-lookup"><span data-stu-id="6fa6f-118">This web service gives the predicted values of the dependent variable based on the independent variables for all of the observations.</span></span> <span data-ttu-id="6fa6f-119">A webszolgáltatás vár a felhasználó a bemeneti adatok karakterláncként, ahol sorok vesszővel (,) válassza el egymástól, és oszlopok pontosvesszővel (;) válassza el egymástól.</span><span class="sxs-lookup"><span data-stu-id="6fa6f-119">The web service expects the end user to input data as a string where rows are separated by comma (,) and columns are separated by semicolon (;).</span></span> <span data-ttu-id="6fa6f-120">A webszolgáltatás egyszerre 1 sor vár, és a függő változó az első oszlopot vár.</span><span class="sxs-lookup"><span data-stu-id="6fa6f-120">The web service expects 1 row at a time and expects the first column to be the dependent variable.</span></span> <span data-ttu-id="6fa6f-121">Egy példa adatkészlet nézhet ki:</span><span class="sxs-lookup"><span data-stu-id="6fa6f-121">An example dataset could look like this:</span></span>

![Mintaadatok][1]

<span data-ttu-id="6fa6f-123">Megfigyelések egy függő változó nélkül y kell megadnia, "NA"KARAKTERLÁNCOT.</span><span class="sxs-lookup"><span data-stu-id="6fa6f-123">Observations without a dependent variable should be input as “NA” for y.</span></span> <span data-ttu-id="6fa6f-124">Az adatok, adjon meg, mert a fenti adatkészlet a következő karakterláncot: "1; 5; 2,1; 1; 6,0; 5.3; 2.1,0; 5; 5,0; 3; 4,1; 2; 1., NA; 3; 4".</span><span class="sxs-lookup"><span data-stu-id="6fa6f-124">The data input for the above dataset would be the following string: “1;5;2,1;1;6,0;5.3;2.1,0;5;5,0;3;4,1;2;1,NA;3;4”.</span></span> <span data-ttu-id="6fa6f-125">A kimeneti az előre jelzett érték az egyes a sorok, a független változók alapján.</span><span class="sxs-lookup"><span data-stu-id="6fa6f-125">The output is the predicted value for each of the rows based on the independent variables.</span></span> 

> <span data-ttu-id="6fa6f-126">Ez a szolgáltatás az Azure piactéren kihelyezett egy OData-szolgáltatás; a POST vagy GET módszerrel elnevezése lehet.</span><span class="sxs-lookup"><span data-stu-id="6fa6f-126">This service, as hosted on the Azure Marketplace, is an OData service; these may be called through POST or GET methods.</span></span> 
> 
> 

<span data-ttu-id="6fa6f-127">Többféleképpen is az automatizált módon a szolgáltatás fel (egy példa alkalmazás [Itt](http://microsoftazuremachinelearning.azurewebsites.net/BinaryClassifier.aspx)).</span><span class="sxs-lookup"><span data-stu-id="6fa6f-127">There are multiple ways of consuming the service in an automated fashion (an example app is [here](http://microsoftazuremachinelearning.azurewebsites.net/BinaryClassifier.aspx)).</span></span>

### <a name="starting-c-code-for-web-service-consumption"></a><span data-ttu-id="6fa6f-128">C#-kódban a webes szolgáltatások felhasználásához megkezdése:</span><span class="sxs-lookup"><span data-stu-id="6fa6f-128">Starting C# code for web service consumption:</span></span>
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


## <a name="creation-of-web-service"></a><span data-ttu-id="6fa6f-129">Webes szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="6fa6f-129">Creation of web service</span></span>
> <span data-ttu-id="6fa6f-130">Ez a webszolgáltatás Azure Machine Learning segítségével lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="6fa6f-130">This web service was created using Azure Machine Learning.</span></span> <span data-ttu-id="6fa6f-131">Az ingyenes próbaverzió, valamint a bevezető videó kísérletek létrehozásával és [közzétételi webes szolgáltatások](machine-learning-publish-a-machine-learning-web-service.md), lásd: [azure.com/ml](http://azure.com/ml).</span><span class="sxs-lookup"><span data-stu-id="6fa6f-131">For a free trial, as well as introductory videos on creating experiments and [publishing web services](machine-learning-publish-a-machine-learning-web-service.md), please see [azure.com/ml](http://azure.com/ml).</span></span> <span data-ttu-id="6fa6f-132">Az alábbiakban van egy Képernyőkép a kísérlet, amely a webes szolgáltatás, és példa kód létre minden egyes belül modulok.</span><span class="sxs-lookup"><span data-stu-id="6fa6f-132">Below is a screenshot of the experiment that created the web service and example code for each of the modules within the experiment.</span></span>
> 
> 

<span data-ttu-id="6fa6f-133">Azure Machine Learning belül egy új üres kísérlet létrehozásához és két [R-parancsfájl végrehajtása] [ execute-r-script] modulok lekért a munkaterület-kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="6fa6f-133">From within Azure Machine Learning, a new blank experiment was created and two [Execute R Script][execute-r-script] modules pulled onto the workspace.</span></span> <span data-ttu-id="6fa6f-134">Ez a webszolgáltatás egy Azure Machine Learning kísérlet fut, alapul szolgáló R-parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="6fa6f-134">This web service runs an Azure Machine Learning experiment with an underlying R script.</span></span> <span data-ttu-id="6fa6f-135">Ehhez a kísérlethez 2 részből áll: schema definíció jelenik meg, és a tanítási modell + pontozási.</span><span class="sxs-lookup"><span data-stu-id="6fa6f-135">There are 2 parts to this experiment: schema definition, and training model + scoring.</span></span> <span data-ttu-id="6fa6f-136">Az első modul határozza meg a bemeneti adatkészlet, ahol az első változó a függő változó, és a fennmaradó változók függetlenek a várt szerkezetnek.</span><span class="sxs-lookup"><span data-stu-id="6fa6f-136">The first module defines the expected structure of the input dataset, where the first variable is the dependent variable and the remaining variables are independent.</span></span> <span data-ttu-id="6fa6f-137">A második modul megfelel a bemeneti adatok általános logisztikai regresszió modellt.</span><span class="sxs-lookup"><span data-stu-id="6fa6f-137">The second module fits a generic logistic regression model for the input data.</span></span>    

![Kísérlet folyamata][2]

#### <a name="module-1"></a><span data-ttu-id="6fa6f-139">1. modul:</span><span class="sxs-lookup"><span data-stu-id="6fa6f-139">Module 1:</span></span>
    #Schema definition  
    data <- data.frame(value = "1;2;3,1;5;6,0;8;9", stringsAsFactors=FALSE) 
    maml.mapOutputPort("data");  

#### <a name="module-2"></a><span data-ttu-id="6fa6f-140">A modul 2:</span><span class="sxs-lookup"><span data-stu-id="6fa6f-140">Module 2:</span></span>
    #GLM modeling   
    data <- maml.mapInputPort(1) # class: data.frame  

    data.split <- strsplit(data[1,1], ",")[[1]] 
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE) 
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE) 
    data.split <- as.data.frame(t(data.split)) data.split <- 
    data.matrix(data.split) 
    data.split <- data.frame(data.split) 

    model <- glm(data.split$V1 ~., family='binomial', data=data.split)  
    out <- data.frame(predict(model,data.split, type="response")) 
    pred1 <- as.data.frame(out) 
    group <- array(1:nrow(pred1)) 
    for (i in 1:nrow(pred1))  
        {
        if(as.numeric(pred1[i,])>0.5) {group[i]=1} 
        else {group[i]=0}
        } 
    pred2 <- as.data.frame(group) 
    maml.mapOutputPort("pred2");  


## <a name="limitations"></a><span data-ttu-id="6fa6f-141">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="6fa6f-141">Limitations</span></span>
<span data-ttu-id="6fa6f-142">Itt látható egy nagyon egyszerű példa egy bináris osztályozási webszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="6fa6f-142">This is a very simple example of a binary classification web service.</span></span> <span data-ttu-id="6fa6f-143">A fenti példa kódot is látható, mert nincs hiba alatt van megvalósítva, és a szolgáltatás azt feltételezi, hogy minden bináris/folyamatos változó (ahol nincs kategorikus funkció engedélyezett), a szolgáltatás csak bemenetek numerikus értékként ennek a webszolgáltatásnak létrehozásának időpontjában .</span><span class="sxs-lookup"><span data-stu-id="6fa6f-143">As can be seen from the example code above, no error catching is implemented and the service assumes everything is a binary/continuous variable (no categorical features allowed), as the service only inputs numeric values at the time of the creation of this web service.</span></span> <span data-ttu-id="6fa6f-144">Emellett a szolgáltatás jelenleg kezeli korlátozott adatok mérete, kellő a webszolgáltatás-hívások és azt a tényt, hogy a modell alatt alkalmas a webes szolgáltatás neve minden egyes kérelem/válasz jellegének.</span><span class="sxs-lookup"><span data-stu-id="6fa6f-144">Also, the service currently handles limited data size, due to the request/response nature of the web service call and the fact that the model is being fit every time the web service is called.</span></span> 

## <a name="faq"></a><span data-ttu-id="6fa6f-145">GYIK</span><span class="sxs-lookup"><span data-stu-id="6fa6f-145">FAQ</span></span>
<span data-ttu-id="6fa6f-146">Gyakori kérdések a felhasználás a webszolgáltatás vagy az Azure piactéren közzétételt, lásd: [Itt](machine-learning-marketplace-faq.md).</span><span class="sxs-lookup"><span data-stu-id="6fa6f-146">For frequently asked questions on consumption of the web service or publishing to the Azure Marketplace, see [here](machine-learning-marketplace-faq.md).</span></span>

[1]: ./media/machine-learning-r-csharp-binary-classifier/binary1.png
[2]: ./media/machine-learning-r-csharp-binary-classifier/binary2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

