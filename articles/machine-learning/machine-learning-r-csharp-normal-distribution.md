---
title: "(elavult) Normális eloszlás Web Service Suite - Azure |} Microsoft Docs"
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
redirect_document_id: TRUE
ms.openlocfilehash: 79d1621330ad56b0c62ca46cfac424c2306e371f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="deprecated-normal-distribution-suite"></a><span data-ttu-id="1dce1-103">(elavult) Normál terjesztési csomag</span><span class="sxs-lookup"><span data-stu-id="1dce1-103">(deprecated) Normal Distribution Suite</span></span>

> [!NOTE]
> <span data-ttu-id="1dce1-104">A Microsoft DataMarket használatból van, és ez az API már elavult.</span><span class="sxs-lookup"><span data-stu-id="1dce1-104">The Microsoft DataMarket is being retired and this API has been deprecated.</span></span> 
> 
> <span data-ttu-id="1dce1-105">Sok hasznos példa kísérletek és API-k a [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="1dce1-105">You can find many useful example experiments and APIs in the [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="1dce1-106">A gyűjtemény kapcsolatos további információkért lásd: [megosztást, és felderítik a Cortana Intelligence Gallery erőforrások](machine-learning-gallery-how-to-use-contribute-publish.md).</span><span class="sxs-lookup"><span data-stu-id="1dce1-106">For more information about the Gallery, see [Share and discover resources in the Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span></span>

<span data-ttu-id="1dce1-107">A normál terjesztési Suite egy olyan minta webszolgáltatások ([generátor](https://datamarket.azure.com/dataset/aml_labs/ndg7), [ki osztóérték Számológép](https://datamarket.azure.com/dataset/aml_labs/ndq5), [valószínűség Számológép](https://datamarket.azure.com/dataset/aml_labs/ndp5)), amely megkönnyíti a létrehozása és kezelése normál terjesztési.</span><span class="sxs-lookup"><span data-stu-id="1dce1-107">The Normal Distribution Suite is a set of sample web services ([Generator](https://datamarket.azure.com/dataset/aml_labs/ndg7), [Quantile Calculator](https://datamarket.azure.com/dataset/aml_labs/ndq5), [Probability Calculator](https://datamarket.azure.com/dataset/aml_labs/ndp5)) that help in generating and handling normal distributions.</span></span> <span data-ttu-id="1dce1-108">A szolgáltatások lehetővé teszik, egy normál terjesztési feladatütemezési hossza, az adott valószínűséggel quantiles kiszámítása, és az egy adott ki osztóérték valószínűség kiszámítása létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="1dce1-108">The services allow generating a normal distribution sequence of any length, calculating quantiles from a given probability, and calculating probability from a given quantile.</span></span> <span data-ttu-id="1dce1-109">A szolgáltatások bocsát ki a kijelölt szolgáltatás alapján különböző kimenetek (lásd az alábbi leírása).</span><span class="sxs-lookup"><span data-stu-id="1dce1-109">Each of the services emits different outputs based on the selected service (see description below).</span></span> <span data-ttu-id="1dce1-110">A normál terjesztési Suite az R funkciók qnorm, rnorm és pnorm, amely R statisztikák csomagban található alapul.</span><span class="sxs-lookup"><span data-stu-id="1dce1-110">The Normal Distribution Suite is based on the R functions qnorm, rnorm, and pnorm, which are included in R stats package.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

> <span data-ttu-id="1dce1-111">Ez a webszolgáltatás kell fenntartania – potenciálisan végig a mobilalkalmazások a webhelyen keresztül, vagy akár a helyi számítógépen, a felhasználókat például.</span><span class="sxs-lookup"><span data-stu-id="1dce1-111">This web service could be consumed by users – potentially through a mobile app, through a website, or even on a local computer, for example.</span></span> <span data-ttu-id="1dce1-112">De a webszolgáltatás célja is példa bemutatja, hogyan Azure Machine Learning webszolgáltatások fölött R-kód létrehozásához használható kiszolgálásához.</span><span class="sxs-lookup"><span data-stu-id="1dce1-112">But the purpose of the web service is also to serve as an example of how Azure Machine Learning can be used to create web services on top of R code.</span></span> <span data-ttu-id="1dce1-113">Az R-kód csupán néhány sornyi és az Azure Machine Learning Studio egy gombját kattint egy kísérlet hozható létre az R-kód és webszolgáltatásként közzé.</span><span class="sxs-lookup"><span data-stu-id="1dce1-113">With just a few lines of R code and clicks of a button within Azure Machine Learning Studio, an experiment can be created with R code and published as a web service.</span></span> <span data-ttu-id="1dce1-114">A webszolgáltatás majd közzé az Azure piactéren, és felhasználók és eszközök által felhasznált világszerte a szerző, a webszolgáltatás által infrastruktúra beállítás nélkül.</span><span class="sxs-lookup"><span data-stu-id="1dce1-114">The web service can then be published to the Azure Marketplace and consumed by users and devices across the world with no infrastructure setup by the author of the web service.</span></span>  
> 
> 

## <a name="consumption-of-web-service"></a><span data-ttu-id="1dce1-115">Felhasználási webszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="1dce1-115">Consumption of web service</span></span>
<span data-ttu-id="1dce1-116">A normál terjesztési csomag a következő 3 szolgáltatásokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="1dce1-116">The Normal Distribution Suite includes the following 3 services.</span></span>

### <a name="normal-distribution-quantile-calculator"></a><span data-ttu-id="1dce1-117">Normális eloszlás ki osztóérték Számológép</span><span class="sxs-lookup"><span data-stu-id="1dce1-117">Normal Distribution Quantile Calculator</span></span>
<span data-ttu-id="1dce1-118">Ez a szolgáltatás a normális eloszlás 4 argumentumként fogadja, és a társított ki osztóérték számítja ki.</span><span class="sxs-lookup"><span data-stu-id="1dce1-118">This service accepts 4 arguments of a normal distribution and calculates the associated quantile.</span></span>

<span data-ttu-id="1dce1-119">A bemeneti argumentumok a következők:</span><span class="sxs-lookup"><span data-stu-id="1dce1-119">The input arguments are:</span></span>

* <span data-ttu-id="1dce1-120">p - egy egyetlen, a normális eloszlás esemény valószínűségét.</span><span class="sxs-lookup"><span data-stu-id="1dce1-120">p - A single probability of an event with normal distribution.</span></span> 
* <span data-ttu-id="1dce1-121">Közepes – normális eloszlás középértékét.</span><span class="sxs-lookup"><span data-stu-id="1dce1-121">Mean - The normal distribution mean.</span></span>
* <span data-ttu-id="1dce1-122">SD - normális eloszlás szórását.</span><span class="sxs-lookup"><span data-stu-id="1dce1-122">SD - The normal distribution standard deviation.</span></span> 
* <span data-ttu-id="1dce1-123">Ügyféloldali - L alsó részén a telepítési és a terjesztési felső oldalán U.</span><span class="sxs-lookup"><span data-stu-id="1dce1-123">Side - L for the lower side of the distribution and U for the upper side of the distribution.</span></span>

<span data-ttu-id="1dce1-124">A szolgáltatás a számított ki az adott valószínűséggel társított osztóérték eredménye.</span><span class="sxs-lookup"><span data-stu-id="1dce1-124">The output of the service is the calculated quantile that is associated with the given probability.</span></span>

### <a name="normal-distribution-probability-calculator"></a><span data-ttu-id="1dce1-125">Normális eloszlás valószínűség Számológép</span><span class="sxs-lookup"><span data-stu-id="1dce1-125">Normal Distribution Probability Calculator</span></span>
<span data-ttu-id="1dce1-126">Ez a szolgáltatás a normális eloszlás 4 argumentumként fogadja, és kiszámítja a társított valószínűség.</span><span class="sxs-lookup"><span data-stu-id="1dce1-126">This service accepts 4 arguments of a normal distribution and calculates the associated probability.</span></span>

<span data-ttu-id="1dce1-127">A bemeneti argumentumok a következők:</span><span class="sxs-lookup"><span data-stu-id="1dce1-127">The input arguments are:</span></span>

* <span data-ttu-id="1dce1-128">q - a normál terjesztési esemény egy egyetlen ki azt osztóérték.</span><span class="sxs-lookup"><span data-stu-id="1dce1-128">q - A single quantile of an event with normal distribution.</span></span> 
* <span data-ttu-id="1dce1-129">Közepes – normális eloszlás középértékét.</span><span class="sxs-lookup"><span data-stu-id="1dce1-129">Mean - The normal distribution mean.</span></span>
* <span data-ttu-id="1dce1-130">SD - normális eloszlás szórását.</span><span class="sxs-lookup"><span data-stu-id="1dce1-130">SD - The normal distribution standard deviation.</span></span> 
* <span data-ttu-id="1dce1-131">Ügyféloldali - L alsó részén a telepítési és a terjesztési felső oldalán U.</span><span class="sxs-lookup"><span data-stu-id="1dce1-131">Side - L for the lower side of the distribution and U for the upper side of the distribution.</span></span>

<span data-ttu-id="1dce1-132">A szolgáltatás a számított annak valószínűségét, hogy az adott ki osztóérték társított eredménye.</span><span class="sxs-lookup"><span data-stu-id="1dce1-132">The output of the service is the calculated probability that is associated with the given quantile.</span></span>

### <a name="normal-distribution-generator"></a><span data-ttu-id="1dce1-133">Normális eloszlás generátor</span><span class="sxs-lookup"><span data-stu-id="1dce1-133">Normal Distribution Generator</span></span>
<span data-ttu-id="1dce1-134">Ez a szolgáltatás fogadja el a normális eloszlás 3 argumentum, és általában elosztott véletlenszerű számsorozatot állít elő.</span><span class="sxs-lookup"><span data-stu-id="1dce1-134">This service accepts 3 arguments of a normal distribution and generates a random sequence of numbers that are normally distributed.</span></span> <span data-ttu-id="1dce1-135">A következő argumentumok belül a kérelem hozzá kell adni:</span><span class="sxs-lookup"><span data-stu-id="1dce1-135">The following arguments should be provided to it within the request:</span></span>

* <span data-ttu-id="1dce1-136">n - megfigyelések száma.</span><span class="sxs-lookup"><span data-stu-id="1dce1-136">n - The number of observations.</span></span> 
* <span data-ttu-id="1dce1-137">Közepes – normális eloszlás középértékét.</span><span class="sxs-lookup"><span data-stu-id="1dce1-137">mean - The normal distribution mean.</span></span>
* <span data-ttu-id="1dce1-138">SD - normális eloszlás szórását.</span><span class="sxs-lookup"><span data-stu-id="1dce1-138">sd - The normal distribution standard deviation.</span></span> 

<span data-ttu-id="1dce1-139">A szolgáltatás eredménye alapján a középérték és sd argumentumok normális eloszlás a hossza n sorozata.</span><span class="sxs-lookup"><span data-stu-id="1dce1-139">The output of the service is a sequence of length n with a normal distribution based on the mean and sd arguments.</span></span>

> <span data-ttu-id="1dce1-140">Ez a szolgáltatás az Azure piactéren kihelyezett egy OData-szolgáltatás; a POST vagy GET módszerrel elnevezése lehet.</span><span class="sxs-lookup"><span data-stu-id="1dce1-140">This service, as hosted on the Azure Marketplace, is an OData service; these may be called through POST or GET methods.</span></span> 
> 
> 

<span data-ttu-id="1dce1-141">Többféleképpen is az automatizált módon a szolgáltatás fel (például alkalmazások olyan itt: [generátor](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionGenerator.aspx), [valószínűség Számológép](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionProbabilityCalculator.aspx), [ki osztóérték Számológép](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionQuantileCalculator.aspx)).</span><span class="sxs-lookup"><span data-stu-id="1dce1-141">There are multiple ways of consuming the service in an automated fashion (example apps are here: [Generator](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionGenerator.aspx), [Probability Calculator](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionProbabilityCalculator.aspx), [Quantile Calculator](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionQuantileCalculator.aspx)).</span></span>

### <a name="starting-c-code-for-web-service-consumption"></a><span data-ttu-id="1dce1-142">C#-kódban a webes szolgáltatások felhasználásához megkezdése:</span><span class="sxs-lookup"><span data-stu-id="1dce1-142">Starting C# code for web service consumption:</span></span>
### <a name="normal-distribution-quantile-calculator"></a><span data-ttu-id="1dce1-143">Normális eloszlás ki osztóérték Számológép</span><span class="sxs-lookup"><span data-stu-id="1dce1-143">Normal Distribution Quantile Calculator</span></span>
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


### <a name="normal-distribution-probability-calculator"></a><span data-ttu-id="1dce1-144">Normális eloszlás valószínűség Számológép</span><span class="sxs-lookup"><span data-stu-id="1dce1-144">Normal Distribution Probability Calculator</span></span>
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

### <a name="normal-distribution-generator"></a><span data-ttu-id="1dce1-145">Normális eloszlás generátor</span><span class="sxs-lookup"><span data-stu-id="1dce1-145">Normal Distribution Generator</span></span>
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


## <a name="creation-of-web-service"></a><span data-ttu-id="1dce1-146">Webes szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="1dce1-146">Creation of web service</span></span>
> <span data-ttu-id="1dce1-147">Ez a webszolgáltatás Azure Machine Learning segítségével lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="1dce1-147">This web service was created using Azure Machine Learning.</span></span> <span data-ttu-id="1dce1-148">Az ingyenes próbaverzió, valamint a bevezető videó kísérletek létrehozásával és [közzétételi webes szolgáltatások](machine-learning-publish-a-machine-learning-web-service.md), lásd: [azure.com/ml](http://azure.com/ml).</span><span class="sxs-lookup"><span data-stu-id="1dce1-148">For a free trial, as well as introductory videos on creating experiments and [publishing web services](machine-learning-publish-a-machine-learning-web-service.md), please see [azure.com/ml](http://azure.com/ml).</span></span> 
> 
> 

<span data-ttu-id="1dce1-149">Az alábbiakban van egy Képernyőkép a kísérlet, amely a webes szolgáltatás, és példa kód létre minden egyes belül modulok.</span><span class="sxs-lookup"><span data-stu-id="1dce1-149">Below is a screenshot of the experiment that created the web service and example code for each of the modules within the experiment.</span></span>

### <a name="normal-distribution-quantile-calculator"></a><span data-ttu-id="1dce1-150">Normális eloszlás ki osztóérték Számológép</span><span class="sxs-lookup"><span data-stu-id="1dce1-150">Normal Distribution Quantile Calculator</span></span>
<span data-ttu-id="1dce1-151">Kísérlet folyamata:</span><span class="sxs-lookup"><span data-stu-id="1dce1-151">Experiment flow:</span></span>

![Kísérlet folyamata][2]

    #Data schema with example data (replaced with data from web service)
    data.set=data.frame(p=0.1,mean=0,sd=1,side='L');
    maml.mapOutputPort("data.set"); #send data to output port

    # Map 1-based optional input ports to variables
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

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");

### <a name="normal-distribution-probability-calculator"></a><span data-ttu-id="1dce1-153">Normális eloszlás valószínűség Számológép</span><span class="sxs-lookup"><span data-stu-id="1dce1-153">Normal Distribution Probability Calculator</span></span>
<span data-ttu-id="1dce1-154">Kísérlet folyamata:</span><span class="sxs-lookup"><span data-stu-id="1dce1-154">Experiment flow:</span></span>

![Kísérlet folyamata][3]

     #Data schema with example data (replaced with data from web service)
    data.set=data.frame(q=-1,mean=0,sd=1,side='L');
    maml.mapOutputPort("data.set"); #send data to output port

    # Map 1-based optional input ports to variables
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

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");

### <a name="normal-distribution-generator"></a><span data-ttu-id="1dce1-156">Normális eloszlás generátor</span><span class="sxs-lookup"><span data-stu-id="1dce1-156">Normal Distribution Generator</span></span>
<span data-ttu-id="1dce1-157">Kísérlet folyamata:</span><span class="sxs-lookup"><span data-stu-id="1dce1-157">Experiment flow:</span></span>

![Kísérlet folyamata][4]

    #Data schema with example data (replaced with data from web service)
    data.set=data.frame(n=50,mean=0,sd=1);
    maml.mapOutputPort("data.set"); #send data to output port

    # Map 1-based optional input ports to variables
    dataset1 <- maml.mapInputPort(1) # class: data.frame

    param = dataset1
    dist = rnorm(param$n,mean=param$mean,sd=param$sd)

    output = as.data.frame(t(dist))

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");

## <a name="limitations"></a><span data-ttu-id="1dce1-159">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="1dce1-159">Limitations</span></span>
<span data-ttu-id="1dce1-160">Példák nagyon egyszerű körülvevő a normális eloszlás.</span><span class="sxs-lookup"><span data-stu-id="1dce1-160">These are very simple examples surrounding the normal distribution.</span></span> <span data-ttu-id="1dce1-161">A fenti példa kódot is látható, mert kevés hiba alatt van megvalósítva.</span><span class="sxs-lookup"><span data-stu-id="1dce1-161">As can be seen from the example code above, little error catching is implemented.</span></span>

## <a name="faq"></a><span data-ttu-id="1dce1-162">GYIK</span><span class="sxs-lookup"><span data-stu-id="1dce1-162">FAQ</span></span>
<span data-ttu-id="1dce1-163">Gyakori kérdések a felhasználás a webszolgáltatás vagy az Azure piactéren közzétételt, lásd: [Itt](machine-learning-marketplace-faq.md).</span><span class="sxs-lookup"><span data-stu-id="1dce1-163">For frequently asked questions on consumption of the web service or publishing to the Azure Marketplace, see [here](machine-learning-marketplace-faq.md).</span></span>

[1]: ./media/machine-learning-r-csharp-normal-distribution/normal-img1.png
[2]: ./media/machine-learning-r-csharp-normal-distribution/normal-img2.png
[3]: ./media/machine-learning-r-csharp-normal-distribution/normal-img3.png
[4]: ./media/machine-learning-r-csharp-normal-distribution/normal-img4.png

