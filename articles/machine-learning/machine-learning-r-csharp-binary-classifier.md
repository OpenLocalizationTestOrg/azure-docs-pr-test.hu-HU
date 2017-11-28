---
title: "aaa(deprecated) bináris osztályozó - Azure |} Microsoft Docs"
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
redirect_document_id: True
ms.openlocfilehash: 0496fcec9952ca243270caf67f55fe191b2dc9f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deprecated-binary-classifier"></a><span data-ttu-id="b16f0-103">(elavult) Bináris osztályozó</span><span class="sxs-lookup"><span data-stu-id="b16f0-103">(deprecated) Binary Classifier</span></span>

> [!NOTE]
> <span data-ttu-id="b16f0-104">a Microsoft DataMarket hello használatból van, és ez az API már elavult.</span><span class="sxs-lookup"><span data-stu-id="b16f0-104">hello Microsoft DataMarket is being retired and this API has been deprecated.</span></span> 
> 
> <span data-ttu-id="b16f0-105">Sok hasznos példa kísérletek és API-kat az található hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="b16f0-105">You can find many useful example experiments and APIs in hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="b16f0-106">Gyűjteményelem hello kapcsolatos további információkért lásd: [megosztás és a Cortana Intelligence Gallery hello erőforrások felderítéséhez](machine-learning-gallery-how-to-use-contribute-publish.md).</span><span class="sxs-lookup"><span data-stu-id="b16f0-106">For more information about hello Gallery, see [Share and discover resources in hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span></span>

<span data-ttu-id="b16f0-107">Tegyük fel, hogy egy adatkészlet rendelkezik, és szeretné toopredict egy bináris függő változó hello független változók alapján.</span><span class="sxs-lookup"><span data-stu-id="b16f0-107">Suppose you have a dataset and would like toopredict a binary dependent variable based on hello independent variables.</span></span> <span data-ttu-id="b16f0-108">"Logisztikai regresszió" a népszerű statisztikai technika ilyen előrejelzéseket használatos.</span><span class="sxs-lookup"><span data-stu-id="b16f0-108">‘Logistic Regression’ is a popular statistical technique used for such predictions.</span></span> <span data-ttu-id="b16f0-109">Itt hello függő változó bináris vagy dichotomous, pedig p hello jellemző érdeklő jelenléte hello valószínűségét.</span><span class="sxs-lookup"><span data-stu-id="b16f0-109">Here hello dependent variable is binary or dichotomous, and p is hello probability of presence of hello characteristic of interest.</span></span> 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<span data-ttu-id="b16f0-110">Egy egyszerű forgatókönyv lehet, ahol egy biztonságkutatói próbál toopredict-e a potenciális student valószínűleg tooaccept egy beléptetésre ajánlat tooa egyetemi (középiskolai, termékcsalád bevétel, rezidens állapot GPA nemek közötti) adatok alapján.</span><span class="sxs-lookup"><span data-stu-id="b16f0-110">A simple scenario could be where a researcher is trying toopredict whether a prospective student is likely tooaccept an admission offer tooa university based on information (GPA in high school, family income, resident state, gender).</span></span> <span data-ttu-id="b16f0-111">előre jelzett hello eredménye-hello potenciális student hello beléptetésre ajánlat elfogadása hello valószínűségét.</span><span class="sxs-lookup"><span data-stu-id="b16f0-111">hello predicted outcome is hello probability of hello prospective student accepting hello admission offer.</span></span> <span data-ttu-id="b16f0-112">Ez [webszolgáltatás](https://datamarket.azure.com/dataset/aml_labs/log_regression) elfér hello logisztikai regresszió toohello modelladatok és kimenetek hello valószínűségi érték (y) az egyes hello megfigyelések hello adataiban.</span><span class="sxs-lookup"><span data-stu-id="b16f0-112">This [web service](https://datamarket.azure.com/dataset/aml_labs/log_regression) fits hello logistic regression model toohello data and outputs hello probability value (y) for each of hello observations in hello data.</span></span>  

> <span data-ttu-id="b16f0-113">Ez a webszolgáltatás kell fenntartania – potenciálisan végig a mobilalkalmazások a webhelyen keresztül, vagy akár a helyi számítógépen, a felhasználókat például.</span><span class="sxs-lookup"><span data-stu-id="b16f0-113">This web service could be consumed by users – potentially through a mobile app, through a website, or even on a local computer, for example.</span></span> <span data-ttu-id="b16f0-114">De hello hello webszolgáltatás célja is tooserve példa bemutatja, hogyan Azure Machine Learning webszolgáltatások használt toocreate fölött R-kód is lehet.</span><span class="sxs-lookup"><span data-stu-id="b16f0-114">But hello purpose of hello web service is also tooserve as an example of how Azure Machine Learning can be used toocreate web services on top of R code.</span></span> <span data-ttu-id="b16f0-115">Az R-kód csupán néhány sornyi és az Azure Machine Learning Studio egy gombját kattint egy kísérlet hozható létre az R-kód és webszolgáltatásként közzé.</span><span class="sxs-lookup"><span data-stu-id="b16f0-115">With just a few lines of R code and clicks of a button within Azure Machine Learning Studio, an experiment can be created with R code and published as a web service.</span></span> <span data-ttu-id="b16f0-116">hello webszolgáltatás majd lehet közzétett toohello Azure piactéren, és a felhasználók és eszközök által felhasznált között hello world hello Szerző hello webszolgáltatás infrastruktúra beállítás nélkül.</span><span class="sxs-lookup"><span data-stu-id="b16f0-116">hello web service can then be published toohello Azure Marketplace and consumed by users and devices across hello world with no infrastructure setup by hello author of hello web service.</span></span>  
> 
> 

## <a name="consumption-of-web-service"></a><span data-ttu-id="b16f0-117">Felhasználási webszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="b16f0-117">Consumption of web service</span></span>
<span data-ttu-id="b16f0-118">A webes szolgáltatás által biztosított hello értékek hello függő változó összes hello megfigyelések hello független változók alapján előre jelezni.</span><span class="sxs-lookup"><span data-stu-id="b16f0-118">This web service gives hello predicted values of hello dependent variable based on hello independent variables for all of hello observations.</span></span> <span data-ttu-id="b16f0-119">hello webszolgáltatás várja hello végfelhasználói tooinput adatok egy karakterláncot, ahol sorok vesszővel (,) válassza el egymástól, és oszlopok pontosvesszővel (;) válassza el egymástól.</span><span class="sxs-lookup"><span data-stu-id="b16f0-119">hello web service expects hello end user tooinput data as a string where rows are separated by comma (,) and columns are separated by semicolon (;).</span></span> <span data-ttu-id="b16f0-120">hello webszolgáltatás egyszerre 1 sor vár, és hello első oszlop toobe hello függő változó vár.</span><span class="sxs-lookup"><span data-stu-id="b16f0-120">hello web service expects 1 row at a time and expects hello first column toobe hello dependent variable.</span></span> <span data-ttu-id="b16f0-121">Egy példa adatkészlet nézhet ki:</span><span class="sxs-lookup"><span data-stu-id="b16f0-121">An example dataset could look like this:</span></span>

![Mintaadatok][1]

<span data-ttu-id="b16f0-123">Megfigyelések egy függő változó nélkül y kell megadnia, "NA"KARAKTERLÁNCOT.</span><span class="sxs-lookup"><span data-stu-id="b16f0-123">Observations without a dependent variable should be input as “NA” for y.</span></span> <span data-ttu-id="b16f0-124">hello adatokat adjon meg a fenti dataset hello volna kell hello a következő karakterláncot: "1; 5; 2,1; 1; 6,0; 5.3; 2.1,0; 5; 5,0; 3; 4,1; 2; 1., NA; 3; 4".</span><span class="sxs-lookup"><span data-stu-id="b16f0-124">hello data input for hello above dataset would be hello following string: “1;5;2,1;1;6,0;5.3;2.1,0;5;5,0;3;4,1;2;1,NA;3;4”.</span></span> <span data-ttu-id="b16f0-125">a kimeneti hello van hello előre jelzett érték az egyes hello sorok hello független változók alapján.</span><span class="sxs-lookup"><span data-stu-id="b16f0-125">hello output is hello predicted value for each of hello rows based on hello independent variables.</span></span> 

> <span data-ttu-id="b16f0-126">Ez a szolgáltatás az Azure piactér hello kihelyezett egy OData-szolgáltatás; a POST vagy GET módszerrel elnevezése lehet.</span><span class="sxs-lookup"><span data-stu-id="b16f0-126">This service, as hosted on hello Azure Marketplace, is an OData service; these may be called through POST or GET methods.</span></span> 
> 
> 

<span data-ttu-id="b16f0-127">Többféleképpen is az automatizált módon hello szolgáltatás fel (egy példa alkalmazás [Itt](http://microsoftazuremachinelearning.azurewebsites.net/BinaryClassifier.aspx)).</span><span class="sxs-lookup"><span data-stu-id="b16f0-127">There are multiple ways of consuming hello service in an automated fashion (an example app is [here](http://microsoftazuremachinelearning.azurewebsites.net/BinaryClassifier.aspx)).</span></span>

### <a name="starting-c-code-for-web-service-consumption"></a><span data-ttu-id="b16f0-128">C#-kódban a webes szolgáltatások felhasználásához megkezdése:</span><span class="sxs-lookup"><span data-stu-id="b16f0-128">Starting C# code for web service consumption:</span></span>
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


## <a name="creation-of-web-service"></a><span data-ttu-id="b16f0-129">Webes szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="b16f0-129">Creation of web service</span></span>
> <span data-ttu-id="b16f0-130">Ez a webszolgáltatás Azure Machine Learning segítségével lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="b16f0-130">This web service was created using Azure Machine Learning.</span></span> <span data-ttu-id="b16f0-131">Az ingyenes próbaverzió, valamint a bevezető videó kísérletek létrehozásával és [közzétételi webes szolgáltatások](machine-learning-publish-a-machine-learning-web-service.md), lásd: [azure.com/ml](http://azure.com/ml).</span><span class="sxs-lookup"><span data-stu-id="b16f0-131">For a free trial, as well as introductory videos on creating experiments and [publishing web services](machine-learning-publish-a-machine-learning-web-service.md), please see [azure.com/ml](http://azure.com/ml).</span></span> <span data-ttu-id="b16f0-132">Az alábbiakban van egy képernyőfelvétel a hello webes szolgáltatás, és példa kódot az egyes hello modulok hello kísérlet belül létrehozott hello kísérlet.</span><span class="sxs-lookup"><span data-stu-id="b16f0-132">Below is a screenshot of hello experiment that created hello web service and example code for each of hello modules within hello experiment.</span></span>
> 
> 

<span data-ttu-id="b16f0-133">Azure Machine Learning belül egy új üres kísérlet létrehozásához és két [R-parancsfájl végrehajtása] [ execute-r-script] modulok lekért hello munkaterület-kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="b16f0-133">From within Azure Machine Learning, a new blank experiment was created and two [Execute R Script][execute-r-script] modules pulled onto hello workspace.</span></span> <span data-ttu-id="b16f0-134">Ez a webszolgáltatás egy Azure Machine Learning kísérlet fut, alapul szolgáló R-parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="b16f0-134">This web service runs an Azure Machine Learning experiment with an underlying R script.</span></span> <span data-ttu-id="b16f0-135">Nincsenek a 2 részek toothis kísérletezhet: schema definíció jelenik meg, és a tanítási modell + pontozási.</span><span class="sxs-lookup"><span data-stu-id="b16f0-135">There are 2 parts toothis experiment: schema definition, and training model + scoring.</span></span> <span data-ttu-id="b16f0-136">hello első modul várt hello struktúrát hello bemeneti adatkészlet, ahol a hello első változó hello függő változó és hello fennmaradó változók független határozza meg.</span><span class="sxs-lookup"><span data-stu-id="b16f0-136">hello first module defines hello expected structure of hello input dataset, where hello first variable is hello dependent variable and hello remaining variables are independent.</span></span> <span data-ttu-id="b16f0-137">hello második modul megfelelő hello bemeneti adatok általános logisztikai regresszió modellt.</span><span class="sxs-lookup"><span data-stu-id="b16f0-137">hello second module fits a generic logistic regression model for hello input data.</span></span>    

![Kísérlet folyamata][2]

#### <a name="module-1"></a><span data-ttu-id="b16f0-139">1. modul:</span><span class="sxs-lookup"><span data-stu-id="b16f0-139">Module 1:</span></span>
    #Schema definition  
    data <- data.frame(value = "1;2;3,1;5;6,0;8;9", stringsAsFactors=FALSE) 
    maml.mapOutputPort("data");  

#### <a name="module-2"></a><span data-ttu-id="b16f0-140">A modul 2:</span><span class="sxs-lookup"><span data-stu-id="b16f0-140">Module 2:</span></span>
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


## <a name="limitations"></a><span data-ttu-id="b16f0-141">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="b16f0-141">Limitations</span></span>
<span data-ttu-id="b16f0-142">Itt látható egy nagyon egyszerű példa egy bináris osztályozási webszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="b16f0-142">This is a very simple example of a binary classification web service.</span></span> <span data-ttu-id="b16f0-143">Hello példakódot fent is látható, mert nincs hiba alatt van megvalósítva és hello szolgáltatás feltételezi, hogy egy bináris/folyamatos változó (ahol nincs kategorikus funkció engedélyezett), hello szolgáltatást csak bemenetek numerikus értékként hello hello létrehozása során webes szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="b16f0-143">As can be seen from hello example code above, no error catching is implemented and hello service assumes everything is a binary/continuous variable (no categorical features allowed), as hello service only inputs numeric values at hello time of hello creation of this web service.</span></span> <span data-ttu-id="b16f0-144">Is hello szolgáltatás jelenleg korlátozott adatméret kezeli, miatt toohello kérelem-válasz jellegű hello webszolgáltatás hívás és hello tényt, hogy a modell hello alatt alkalmas minden alkalommal, amikor hello webszolgáltatás nevezik.</span><span class="sxs-lookup"><span data-stu-id="b16f0-144">Also, hello service currently handles limited data size, due toohello request/response nature of hello web service call and hello fact that hello model is being fit every time hello web service is called.</span></span> 

## <a name="faq"></a><span data-ttu-id="b16f0-145">GYIK</span><span class="sxs-lookup"><span data-stu-id="b16f0-145">FAQ</span></span>
<span data-ttu-id="b16f0-146">Gyakori kérdések a felhasználás hello webszolgáltatás vagy az Azure piactér közzétételi toohello, lásd: [Itt](machine-learning-marketplace-faq.md).</span><span class="sxs-lookup"><span data-stu-id="b16f0-146">For frequently asked questions on consumption of hello web service or publishing toohello Azure Marketplace, see [here](machine-learning-marketplace-faq.md).</span></span>

[1]: ./media/machine-learning-r-csharp-binary-classifier/binary1.png
[2]: ./media/machine-learning-r-csharp-binary-classifier/binary2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

