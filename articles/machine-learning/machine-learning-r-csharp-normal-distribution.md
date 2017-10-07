---
title: "aaa(deprecated) normális eloszlás Web Service Suite - Azure |} Microsoft Docs"
description: "(elavult) Normális eloszlás Web Service-csomag"
services: machine-learning
documentationcenter: 
author: ireiter
manager: jhubbard
editor: cgronlun
ms.assetid: aab7b92e-953b-43d8-b0af-031394406bfe
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: ireiter
ROBOTS: NOINDEX
redirect_url: https://gallery.cortanaintelligence.com/
redirect_document_id: True
ms.openlocfilehash: 8bdb5afd9fee88587f548d7c5299480f64289bbe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deprecated-normal-distribution-suite"></a><span data-ttu-id="2d8e3-103">(elavult) Normál terjesztési csomag</span><span class="sxs-lookup"><span data-stu-id="2d8e3-103">(deprecated) Normal Distribution Suite</span></span>

> [!NOTE]
> <span data-ttu-id="2d8e3-104">a Microsoft DataMarket hello használatból van, és ez az API már elavult.</span><span class="sxs-lookup"><span data-stu-id="2d8e3-104">hello Microsoft DataMarket is being retired and this API has been deprecated.</span></span> 
> 
> <span data-ttu-id="2d8e3-105">Sok hasznos példa kísérletek és API-kat az található hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="2d8e3-105">You can find many useful example experiments and APIs in hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="2d8e3-106">Gyűjteményelem hello kapcsolatos további információkért lásd: [megosztás és a Cortana Intelligence Gallery hello erőforrások felderítéséhez](machine-learning-gallery-how-to-use-contribute-publish.md).</span><span class="sxs-lookup"><span data-stu-id="2d8e3-106">For more information about hello Gallery, see [Share and discover resources in hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span></span>

<span data-ttu-id="2d8e3-107">hello normális eloszlás Suite egy olyan minta webszolgáltatásokat ([generátor](https://datamarket.azure.com/dataset/aml_labs/ndg7), [ki osztóérték Számológép](https://datamarket.azure.com/dataset/aml_labs/ndq5), [valószínűség Számológép](https://datamarket.azure.com/dataset/aml_labs/ndp5)), amelyekkel létrehozása és kezelése normál terjesztési.</span><span class="sxs-lookup"><span data-stu-id="2d8e3-107">hello Normal Distribution Suite is a set of sample web services ([Generator](https://datamarket.azure.com/dataset/aml_labs/ndg7), [Quantile Calculator](https://datamarket.azure.com/dataset/aml_labs/ndq5), [Probability Calculator](https://datamarket.azure.com/dataset/aml_labs/ndp5)) that help in generating and handling normal distributions.</span></span> <span data-ttu-id="2d8e3-108">hello szolgáltatások lehetővé teszik, hogy egy normál terjesztési feladatütemezési hossza, az adott valószínűséggel quantiles kiszámítása, és az egy adott ki osztóérték valószínűség kiszámítása létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="2d8e3-108">hello services allow generating a normal distribution sequence of any length, calculating quantiles from a given probability, and calculating probability from a given quantile.</span></span> <span data-ttu-id="2d8e3-109">Hello szolgáltatások bocsát ki a kijelölt hello szolgáltatás alapján különböző kimenetek (lásd az alábbi leírása).</span><span class="sxs-lookup"><span data-stu-id="2d8e3-109">Each of hello services emits different outputs based on hello selected service (see description below).</span></span> <span data-ttu-id="2d8e3-110">hello normális eloszlás Suite hello R funkciók qnorm rnorm és pnorm, amely R statisztikák csomagban található alapul.</span><span class="sxs-lookup"><span data-stu-id="2d8e3-110">hello Normal Distribution Suite is based on hello R functions qnorm, rnorm, and pnorm, which are included in R stats package.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

> <span data-ttu-id="2d8e3-111">Ez a webszolgáltatás kell fenntartania – potenciálisan végig a mobilalkalmazások a webhelyen keresztül, vagy akár a helyi számítógépen, a felhasználókat például.</span><span class="sxs-lookup"><span data-stu-id="2d8e3-111">This web service could be consumed by users – potentially through a mobile app, through a website, or even on a local computer, for example.</span></span> <span data-ttu-id="2d8e3-112">De hello hello webszolgáltatás célja is tooserve példa bemutatja, hogyan Azure Machine Learning webszolgáltatások használt toocreate fölött R-kód is lehet.</span><span class="sxs-lookup"><span data-stu-id="2d8e3-112">But hello purpose of hello web service is also tooserve as an example of how Azure Machine Learning can be used toocreate web services on top of R code.</span></span> <span data-ttu-id="2d8e3-113">Az R-kód csupán néhány sornyi és az Azure Machine Learning Studio egy gombját kattint egy kísérlet hozható létre az R-kód és webszolgáltatásként közzé.</span><span class="sxs-lookup"><span data-stu-id="2d8e3-113">With just a few lines of R code and clicks of a button within Azure Machine Learning Studio, an experiment can be created with R code and published as a web service.</span></span> <span data-ttu-id="2d8e3-114">hello webszolgáltatás majd lehet közzétett toohello Azure piactéren, és a felhasználók és eszközök által felhasznált között hello world hello Szerző hello webszolgáltatás infrastruktúra beállítás nélkül.</span><span class="sxs-lookup"><span data-stu-id="2d8e3-114">hello web service can then be published toohello Azure Marketplace and consumed by users and devices across hello world with no infrastructure setup by hello author of hello web service.</span></span>  
> 
> 

## <a name="consumption-of-web-service"></a><span data-ttu-id="2d8e3-115">Felhasználási webszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="2d8e3-115">Consumption of web service</span></span>
<span data-ttu-id="2d8e3-116">hello normális eloszlás csomag magában foglalja a következő 3 szolgáltatások hello.</span><span class="sxs-lookup"><span data-stu-id="2d8e3-116">hello Normal Distribution Suite includes hello following 3 services.</span></span>

### <a name="normal-distribution-quantile-calculator"></a><span data-ttu-id="2d8e3-117">Normális eloszlás ki osztóérték Számológép</span><span class="sxs-lookup"><span data-stu-id="2d8e3-117">Normal Distribution Quantile Calculator</span></span>
<span data-ttu-id="2d8e3-118">Ez a szolgáltatás a normális eloszlás 4 argumentumként fogadja, és társított hello ki osztóérték számítja ki.</span><span class="sxs-lookup"><span data-stu-id="2d8e3-118">This service accepts 4 arguments of a normal distribution and calculates hello associated quantile.</span></span>

<span data-ttu-id="2d8e3-119">hello bemeneti argumentumai a következők:</span><span class="sxs-lookup"><span data-stu-id="2d8e3-119">hello input arguments are:</span></span>

* <span data-ttu-id="2d8e3-120">p - egy egyetlen, a normális eloszlás esemény valószínűségét.</span><span class="sxs-lookup"><span data-stu-id="2d8e3-120">p - A single probability of an event with normal distribution.</span></span> 
* <span data-ttu-id="2d8e3-121">Közepes – hello normális eloszlás értékeinek középértéke.</span><span class="sxs-lookup"><span data-stu-id="2d8e3-121">Mean - hello normal distribution mean.</span></span>
* <span data-ttu-id="2d8e3-122">SD - hello normális eloszlás szórás.</span><span class="sxs-lookup"><span data-stu-id="2d8e3-122">SD - hello normal distribution standard deviation.</span></span> 
* <span data-ttu-id="2d8e3-123">Ügyféloldali - L hello alsó részén hello terjesztési és U hello hello terjesztési felső oldalán.</span><span class="sxs-lookup"><span data-stu-id="2d8e3-123">Side - L for hello lower side of hello distribution and U for hello upper side of hello distribution.</span></span>

<span data-ttu-id="2d8e3-124">hello hello szolgáltatás eredménye hello számított ki, amely kapcsolódik az adott valószínűség hello osztóérték.</span><span class="sxs-lookup"><span data-stu-id="2d8e3-124">hello output of hello service is hello calculated quantile that is associated with hello given probability.</span></span>

### <a name="normal-distribution-probability-calculator"></a><span data-ttu-id="2d8e3-125">Normális eloszlás valószínűség Számológép</span><span class="sxs-lookup"><span data-stu-id="2d8e3-125">Normal Distribution Probability Calculator</span></span>
<span data-ttu-id="2d8e3-126">Ez a szolgáltatás a normális eloszlás 4 argumentumként fogadja, és kiszámítja az hello rendelt valószínűség.</span><span class="sxs-lookup"><span data-stu-id="2d8e3-126">This service accepts 4 arguments of a normal distribution and calculates hello associated probability.</span></span>

<span data-ttu-id="2d8e3-127">hello bemeneti argumentumai a következők:</span><span class="sxs-lookup"><span data-stu-id="2d8e3-127">hello input arguments are:</span></span>

* <span data-ttu-id="2d8e3-128">q - a normál terjesztési esemény egy egyetlen ki azt osztóérték.</span><span class="sxs-lookup"><span data-stu-id="2d8e3-128">q - A single quantile of an event with normal distribution.</span></span> 
* <span data-ttu-id="2d8e3-129">Közepes – hello normális eloszlás értékeinek középértéke.</span><span class="sxs-lookup"><span data-stu-id="2d8e3-129">Mean - hello normal distribution mean.</span></span>
* <span data-ttu-id="2d8e3-130">SD - hello normális eloszlás szórás.</span><span class="sxs-lookup"><span data-stu-id="2d8e3-130">SD - hello normal distribution standard deviation.</span></span> 
* <span data-ttu-id="2d8e3-131">Ügyféloldali - L hello alsó részén hello terjesztési és U hello hello terjesztési felső oldalán.</span><span class="sxs-lookup"><span data-stu-id="2d8e3-131">Side - L for hello lower side of hello distribution and U for hello upper side of hello distribution.</span></span>

<span data-ttu-id="2d8e3-132">hello hello szolgáltatás eredménye számított hello annak valószínűségét, hogy a megadott ki osztóérték hello van társítva.</span><span class="sxs-lookup"><span data-stu-id="2d8e3-132">hello output of hello service is hello calculated probability that is associated with hello given quantile.</span></span>

### <a name="normal-distribution-generator"></a><span data-ttu-id="2d8e3-133">Normális eloszlás generátor</span><span class="sxs-lookup"><span data-stu-id="2d8e3-133">Normal Distribution Generator</span></span>
<span data-ttu-id="2d8e3-134">Ez a szolgáltatás fogadja el a normális eloszlás 3 argumentum, és általában elosztott véletlenszerű számsorozatot állít elő.</span><span class="sxs-lookup"><span data-stu-id="2d8e3-134">This service accepts 3 arguments of a normal distribution and generates a random sequence of numbers that are normally distributed.</span></span> <span data-ttu-id="2d8e3-135">hello következő szabad megadni típusargumentumot tooit hello kérelem belül:</span><span class="sxs-lookup"><span data-stu-id="2d8e3-135">hello following arguments should be provided tooit within hello request:</span></span>

* <span data-ttu-id="2d8e3-136">n - megfigyelések hello száma.</span><span class="sxs-lookup"><span data-stu-id="2d8e3-136">n - hello number of observations.</span></span> 
* <span data-ttu-id="2d8e3-137">Közepes – hello normális eloszlás értékeinek középértéke.</span><span class="sxs-lookup"><span data-stu-id="2d8e3-137">mean - hello normal distribution mean.</span></span>
* <span data-ttu-id="2d8e3-138">SD - hello normális eloszlás szórás.</span><span class="sxs-lookup"><span data-stu-id="2d8e3-138">sd - hello normal distribution standard deviation.</span></span> 

<span data-ttu-id="2d8e3-139">hello hello szolgáltatás eredménye alapján hello középérték és sd argumentumok normális eloszlás a hossza n sorozata.</span><span class="sxs-lookup"><span data-stu-id="2d8e3-139">hello output of hello service is a sequence of length n with a normal distribution based on hello mean and sd arguments.</span></span>

> <span data-ttu-id="2d8e3-140">Ez a szolgáltatás az Azure piactér hello kihelyezett egy OData-szolgáltatás; a POST vagy GET módszerrel elnevezése lehet.</span><span class="sxs-lookup"><span data-stu-id="2d8e3-140">This service, as hosted on hello Azure Marketplace, is an OData service; these may be called through POST or GET methods.</span></span> 
> 
> 

<span data-ttu-id="2d8e3-141">Többféleképpen is az automatizált módon hello szolgáltatás fel (például alkalmazások olyan itt: [generátor](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionGenerator.aspx), [valószínűség Számológép](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionProbabilityCalculator.aspx), [ki osztóérték Számológép](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionQuantileCalculator.aspx)).</span><span class="sxs-lookup"><span data-stu-id="2d8e3-141">There are multiple ways of consuming hello service in an automated fashion (example apps are here: [Generator](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionGenerator.aspx), [Probability Calculator](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionProbabilityCalculator.aspx), [Quantile Calculator](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionQuantileCalculator.aspx)).</span></span>

### <a name="starting-c-code-for-web-service-consumption"></a><span data-ttu-id="2d8e3-142">C#-kódban a webes szolgáltatások felhasználásához megkezdése:</span><span class="sxs-lookup"><span data-stu-id="2d8e3-142">Starting C# code for web service consumption:</span></span>
### <a name="normal-distribution-quantile-calculator"></a><span data-ttu-id="2d8e3-143">Normális eloszlás ki osztóérték Számológép</span><span class="sxs-lookup"><span data-stu-id="2d8e3-143">Normal Distribution Quantile Calculator</span></span>
    public class Input
    {
            public string p;
            public string mean;
            public string sd;
            public string side;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { p = TextBox1.Text, mean = TextBox2.Text, sd = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();

            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));

            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }


### <a name="normal-distribution-probability-calculator"></a><span data-ttu-id="2d8e3-144">Normális eloszlás valószínűség Számológép</span><span class="sxs-lookup"><span data-stu-id="2d8e3-144">Normal Distribution Probability Calculator</span></span>
    public class Input
    {
            public string q;
            public string mean;
            public string sd;
            public string side;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { q = TextBox1.Text, mean = TextBox2.Text, sd = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();

            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));

            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }

### <a name="normal-distribution-generator"></a><span data-ttu-id="2d8e3-145">Normális eloszlás generátor</span><span class="sxs-lookup"><span data-stu-id="2d8e3-145">Normal Distribution Generator</span></span>
    public class Input
    {
            public string n;
            public string mean;
            public string sd;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { n = TextBox1.Text, mean = TextBox2.Text, sd = TextBox3.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();

            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));

            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }


## <a name="creation-of-web-service"></a><span data-ttu-id="2d8e3-146">Webes szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="2d8e3-146">Creation of web service</span></span>
> <span data-ttu-id="2d8e3-147">Ez a webszolgáltatás Azure Machine Learning segítségével lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="2d8e3-147">This web service was created using Azure Machine Learning.</span></span> <span data-ttu-id="2d8e3-148">Az ingyenes próbaverzió, valamint a bevezető videó kísérletek létrehozásával és [közzétételi webes szolgáltatások](machine-learning-publish-a-machine-learning-web-service.md), lásd: [azure.com/ml](http://azure.com/ml).</span><span class="sxs-lookup"><span data-stu-id="2d8e3-148">For a free trial, as well as introductory videos on creating experiments and [publishing web services](machine-learning-publish-a-machine-learning-web-service.md), please see [azure.com/ml](http://azure.com/ml).</span></span> 
> 
> 

<span data-ttu-id="2d8e3-149">Az alábbiakban van egy képernyőfelvétel a hello webes szolgáltatás, és példa kódot az egyes hello modulok hello kísérlet belül létrehozott hello kísérlet.</span><span class="sxs-lookup"><span data-stu-id="2d8e3-149">Below is a screenshot of hello experiment that created hello web service and example code for each of hello modules within hello experiment.</span></span>

### <a name="normal-distribution-quantile-calculator"></a><span data-ttu-id="2d8e3-150">Normális eloszlás ki osztóérték Számológép</span><span class="sxs-lookup"><span data-stu-id="2d8e3-150">Normal Distribution Quantile Calculator</span></span>
<span data-ttu-id="2d8e3-151">Kísérlet folyamata:</span><span class="sxs-lookup"><span data-stu-id="2d8e3-151">Experiment flow:</span></span>

![Kísérlet folyamata][2]

    #Data schema with example data (replaced with data from web service)
    data.set=data.frame(p=0.1,mean=0,sd=1,side='L');
    maml.mapOutputPort("data.set"); #send data toooutput port

    # Map 1-based optional input ports toovariables
    dataset1 <- maml.mapInputPort(1) # class: data.frame

    param = dataset1
    if (param$p < 0 ) {
    print('Bad input: p must be between 0 and 1')
    param$p = 0
    } else if (param$p > 1) {
    print('Bad input: p must be between 0 and 1')
    param$p = 1
    }
    q = qnorm(param$p,mean=param$mean,sd=param$sd)

    if (param$side == 'U'){
    q = 2* param$mean - q
    } else if (param$side =='L') {
    q = q
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(q)

    # Select data.frame toobe sent toohello output Dataset port
    maml.mapOutputPort("output");

### <a name="normal-distribution-probability-calculator"></a><span data-ttu-id="2d8e3-153">Normális eloszlás valószínűség Számológép</span><span class="sxs-lookup"><span data-stu-id="2d8e3-153">Normal Distribution Probability Calculator</span></span>
<span data-ttu-id="2d8e3-154">Kísérlet folyamata:</span><span class="sxs-lookup"><span data-stu-id="2d8e3-154">Experiment flow:</span></span>

![Kísérlet folyamata][3]

     #Data schema with example data (replaced with data from web service)
    data.set=data.frame(q=-1,mean=0,sd=1,side='L');
    maml.mapOutputPort("data.set"); #send data toooutput port

    # Map 1-based optional input ports toovariables
    dataset1 <- maml.mapInputPort(1) # class: data.frame

    param = dataset1
    prob = pnorm(param$q,mean=param$mean,sd=param$sd)

    if (param$side == 'U'){
    prob = 1 - prob
    } else if (param$side =='B') {
    prob = ifelse(prob<=0.5,prob * 2, 1)
    } else if (param$side =='L') {
    prob = prob
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(prob)

    # Select data.frame toobe sent toohello output Dataset port
    maml.mapOutputPort("output");

### <a name="normal-distribution-generator"></a><span data-ttu-id="2d8e3-156">Normális eloszlás generátor</span><span class="sxs-lookup"><span data-stu-id="2d8e3-156">Normal Distribution Generator</span></span>
<span data-ttu-id="2d8e3-157">Kísérlet folyamata:</span><span class="sxs-lookup"><span data-stu-id="2d8e3-157">Experiment flow:</span></span>

![Kísérlet folyamata][4]

    #Data schema with example data (replaced with data from web service)
    data.set=data.frame(n=50,mean=0,sd=1);
    maml.mapOutputPort("data.set"); #send data toooutput port

    # Map 1-based optional input ports toovariables
    dataset1 <- maml.mapInputPort(1) # class: data.frame

    param = dataset1
    dist = rnorm(param$n,mean=param$mean,sd=param$sd)

    output = as.data.frame(t(dist))

    # Select data.frame toobe sent toohello output Dataset port
    maml.mapOutputPort("output");

## <a name="limitations"></a><span data-ttu-id="2d8e3-159">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="2d8e3-159">Limitations</span></span>
<span data-ttu-id="2d8e3-160">Példák nagyon egyszerű körülvevő hello normális eloszlás.</span><span class="sxs-lookup"><span data-stu-id="2d8e3-160">These are very simple examples surrounding hello normal distribution.</span></span> <span data-ttu-id="2d8e3-161">Hello példakódot fent is látható, mert kevés hiba alatt van megvalósítva.</span><span class="sxs-lookup"><span data-stu-id="2d8e3-161">As can be seen from hello example code above, little error catching is implemented.</span></span>

## <a name="faq"></a><span data-ttu-id="2d8e3-162">GYIK</span><span class="sxs-lookup"><span data-stu-id="2d8e3-162">FAQ</span></span>
<span data-ttu-id="2d8e3-163">Gyakori kérdések a felhasználás hello webszolgáltatás vagy az Azure piactér közzétételi toohello, lásd: [Itt](machine-learning-marketplace-faq.md).</span><span class="sxs-lookup"><span data-stu-id="2d8e3-163">For frequently asked questions on consumption of hello web service or publishing toohello Azure Marketplace, see [here](machine-learning-marketplace-faq.md).</span></span>

[1]: ./media/machine-learning-r-csharp-normal-distribution/normal-img1.png
[2]: ./media/machine-learning-r-csharp-normal-distribution/normal-img2.png
[3]: ./media/machine-learning-r-csharp-normal-distribution/normal-img3.png
[4]: ./media/machine-learning-r-csharp-normal-distribution/normal-img4.png

