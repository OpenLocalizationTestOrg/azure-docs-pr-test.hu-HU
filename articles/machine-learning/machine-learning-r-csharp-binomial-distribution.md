---
title: "(elavult) Kudarc_szám Suite - Azure |} Microsoft Docs"
description: "(elavult) Kudarc_szám csomag"
services: machine-learning
documentationcenter: 
author: ireiter
manager: jhubbard
editor: cgronlun
ms.assetid: 6d102d57-8f20-4ab3-be31-02fcfe4d15ed
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
ms.openlocfilehash: 6f0a6d06e7401c8360a92a707a0552f41ff3657c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="deprecated-binomial-distribution-suite"></a><span data-ttu-id="73a72-103">(elavult) Kudarc_szám csomag</span><span class="sxs-lookup"><span data-stu-id="73a72-103">(deprecated) Binomial Distribution Suite</span></span>

> [!NOTE]
> <span data-ttu-id="73a72-104">A Microsoft DataMarket használatból van, és ez az API már elavult.</span><span class="sxs-lookup"><span data-stu-id="73a72-104">The Microsoft DataMarket is being retired and this API has been deprecated.</span></span> 
> 
> <span data-ttu-id="73a72-105">Sok hasznos példa kísérletek és API-k a [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="73a72-105">You can find many useful example experiments and APIs in the [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="73a72-106">A gyűjtemény kapcsolatos további információkért lásd: [megosztást, és felderítik a Cortana Intelligence Gallery erőforrások](machine-learning-gallery-how-to-use-contribute-publish.md).</span><span class="sxs-lookup"><span data-stu-id="73a72-106">For more information about the Gallery, see [Share and discover resources in the Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span></span>

<span data-ttu-id="73a72-107">A kudarc_szám Suite egy olyan minta webszolgáltatásokat ([binomiális generátor](https://datamarket.azure.com/dataset/aml_labs/bdg5), [valószínűség Számológép](https://datamarket.azure.com/dataset/aml_labs/bdp4), [ki osztóérték Számológép](https://datamarket.azure.com/dataset/aml_labs/bdq5)), amelyekkel generálásához és binomiális terjesztéseket foglalkozik.</span><span class="sxs-lookup"><span data-stu-id="73a72-107">The Binomial Distribution Suite is a set of sample web services ([Binomial Generator](https://datamarket.azure.com/dataset/aml_labs/bdg5), [Probability Calculator](https://datamarket.azure.com/dataset/aml_labs/bdp4), [Quantile Calculator](https://datamarket.azure.com/dataset/aml_labs/bdq5)) that help in generating and dealing with binomial distributions.</span></span> <span data-ttu-id="73a72-108">A szolgáltatások lehetővé teszik, hogy bármely hossza quantiles kiszámítása kudarc_szám sorozatát generálása kívüli adott valószínűség és számítási valószínűség az egy adott ki osztóérték.</span><span class="sxs-lookup"><span data-stu-id="73a72-108">The services allow generating a binomial distribution sequence of any length, calculating quantiles out of given probability and calculating probability from a given quantile.</span></span> <span data-ttu-id="73a72-109">A szolgáltatások bocsát ki a kijelölt szolgáltatás alapján különböző kimenetek (lásd az alábbi leírása).</span><span class="sxs-lookup"><span data-stu-id="73a72-109">Each of the services emits different outputs based on the selected service (see description below).</span></span> <span data-ttu-id="73a72-110">A kudarc_szám Suite az R funkciók qbinom, rbinom és pbinom, amelyek szerepelnek az R-statisztikák csomag alapul.</span><span class="sxs-lookup"><span data-stu-id="73a72-110">The Binomial Distribution Suite is based on the R functions qbinom, rbinom, and pbinom, which are included in the R stats package.</span></span> 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

> <span data-ttu-id="73a72-111">Ezek a webszolgáltatások kell fenntartania felhasználók – potenciálisan közvetlenül a a piactéren keresztül a mobilalkalmazások a webhelyen keresztül, vagy akár a helyi számítógépen, például.</span><span class="sxs-lookup"><span data-stu-id="73a72-111">These web services could be consumed by users – potentially directly on the marketplace, through a mobile app, through a website, or even on a local computer, for example.</span></span> <span data-ttu-id="73a72-112">De a webszolgáltatás célja is példa bemutatja, hogyan Azure Machine Learning webszolgáltatások fölött R-kód létrehozásához használható kiszolgálásához.</span><span class="sxs-lookup"><span data-stu-id="73a72-112">But the purpose of the web service is also to serve as an example of how Azure Machine Learning can be used to create web services on top of R code.</span></span> <span data-ttu-id="73a72-113">Az R-kód csupán néhány sornyi és az Azure Machine Learning Studio egy gombját kattint egy kísérlet hozható létre az R-kód és webszolgáltatásként közzé.</span><span class="sxs-lookup"><span data-stu-id="73a72-113">With just a few lines of R code and clicks of a button within Azure Machine Learning Studio, an experiment can be created with R code and published as a web service.</span></span> <span data-ttu-id="73a72-114">A webszolgáltatás majd is az Azure piactéren közzétett és a világ különböző felhasználók és eszközök által felhasznált – kell nincs infrastruktúra beállítani a webszolgáltatás készítője által.</span><span class="sxs-lookup"><span data-stu-id="73a72-114">The web service can then be published to the Azure Marketplace and consumed by users and devices across the world – no infrastructure setup by the author of the web service is required.</span></span>
> 
> 

## <a name="consumption-of-web-service"></a><span data-ttu-id="73a72-115">Felhasználási webszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="73a72-115">Consumption of web service</span></span>
<span data-ttu-id="73a72-116">A kudarc_szám csomag a következő 3 szolgáltatásokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="73a72-116">The Binomial Distribution Suite includes the following 3 services.</span></span>

### <a name="binomial-distribution-quantile-calculator"></a><span data-ttu-id="73a72-117">Kudarc_szám ki osztóérték Számológép</span><span class="sxs-lookup"><span data-stu-id="73a72-117">Binomial Distribution Quantile Calculator</span></span>
<span data-ttu-id="73a72-118">Ez a szolgáltatás a normális eloszlás 4 argumentumként fogadja, és a társított ki osztóérték számítja ki.</span><span class="sxs-lookup"><span data-stu-id="73a72-118">This service accepts 4 arguments of a normal distribution and calculates the associated quantile.</span></span>
<span data-ttu-id="73a72-119">A bemeneti argumentumok a következők:</span><span class="sxs-lookup"><span data-stu-id="73a72-119">The input arguments are:</span></span>

* <span data-ttu-id="73a72-120">p - egyetlen összevont több kísérletek valószínűségét.</span><span class="sxs-lookup"><span data-stu-id="73a72-120">p - A single aggregated probability of multiple trials.</span></span>  
* <span data-ttu-id="73a72-121">méret - kísérletek száma.</span><span class="sxs-lookup"><span data-stu-id="73a72-121">size - The number of trials.</span></span>
* <span data-ttu-id="73a72-122">valószínűség - próbaverzió sikeres valószínűségét.</span><span class="sxs-lookup"><span data-stu-id="73a72-122">prob - The probability of success in a trial.</span></span>
* <span data-ttu-id="73a72-123">Az oldal - L az alsó részén a terjesztési U eloszlását felső oldalán.</span><span class="sxs-lookup"><span data-stu-id="73a72-123">Side - L for the lower side of the distribution, U for the upper side of the distribution.</span></span> 

<span data-ttu-id="73a72-124">A szolgáltatás a számított ki az adott valószínűséggel társított osztóérték eredménye.</span><span class="sxs-lookup"><span data-stu-id="73a72-124">The output of the service is the calculated quantile that is associated with the given probability.</span></span>

### <a name="binomial-distribution-probability-calculator"></a><span data-ttu-id="73a72-125">Kudarc_szám valószínűség Számológép</span><span class="sxs-lookup"><span data-stu-id="73a72-125">Binomial Distribution Probability Calculator</span></span>
<span data-ttu-id="73a72-126">Ez a szolgáltatás egy binomiális eloszlás 4 argumentumként fogadja, és kiszámítja a társított valószínűség.</span><span class="sxs-lookup"><span data-stu-id="73a72-126">This service accepts 4 arguments of a binomial distribution and calculates the associated probability.</span></span>
<span data-ttu-id="73a72-127">A bemeneti argumentumok a következők:</span><span class="sxs-lookup"><span data-stu-id="73a72-127">The input arguments are:</span></span>

* <span data-ttu-id="73a72-128">a kérdések - egy egyetlen ki osztóérték kudarc_szám az esemény.</span><span class="sxs-lookup"><span data-stu-id="73a72-128">q - A single quantile of an event with binomial distribution.</span></span> 
* <span data-ttu-id="73a72-129">méret - kísérletek száma.</span><span class="sxs-lookup"><span data-stu-id="73a72-129">size - The number of trials.</span></span>
* <span data-ttu-id="73a72-130">valószínűség - próbaverzió sikeres valószínűségét.</span><span class="sxs-lookup"><span data-stu-id="73a72-130">prob - The probability of success in a trial.</span></span>
* <span data-ttu-id="73a72-131">az oldal - L az alsó részén a terjesztési U a terjesztési vagy egy sikeres egyetlen száma egyenlő E felső oldalán.</span><span class="sxs-lookup"><span data-stu-id="73a72-131">side - L for the lower side of the distribution, U for the upper side of the distribution, or E that is equal to a single number of successes.</span></span>

<span data-ttu-id="73a72-132">A szolgáltatás a számított annak valószínűségét, hogy az adott ki osztóérték társított eredménye.</span><span class="sxs-lookup"><span data-stu-id="73a72-132">The output of the service is the calculated probability that is associated with the given quantile.</span></span>

### <a name="binomial-distribution-generator"></a><span data-ttu-id="73a72-133">Kudarc_szám generátor</span><span class="sxs-lookup"><span data-stu-id="73a72-133">Binomial Distribution Generator</span></span>
<span data-ttu-id="73a72-134">Ez a szolgáltatás egy binomiális eloszlás 3 argumentumként fogadja, és létrehoz egy véletlenszerű számsorozatot binomially elosztott.</span><span class="sxs-lookup"><span data-stu-id="73a72-134">This service accepts 3 arguments of a binomial distribution and generates a random sequence of numbers that are binomially distributed.</span></span> <span data-ttu-id="73a72-135">A következő argumentumok belül a kérelem hozzá kell adni:</span><span class="sxs-lookup"><span data-stu-id="73a72-135">The following arguments should be provided to it within the request:</span></span>

* <span data-ttu-id="73a72-136">n - megfigyelések száma.</span><span class="sxs-lookup"><span data-stu-id="73a72-136">n - Number of observations.</span></span> 
* <span data-ttu-id="73a72-137">méret - kísérletek száma.</span><span class="sxs-lookup"><span data-stu-id="73a72-137">size - Number of trials.</span></span>
* <span data-ttu-id="73a72-138">valószínűség - sikeres valószínűségét.</span><span class="sxs-lookup"><span data-stu-id="73a72-138">prob - Probability of success.</span></span>

<span data-ttu-id="73a72-139">A szolgáltatás eredménye egy binomiális elosztás alapján az mérete és a valószínűség argumentum hossza n sorozata.</span><span class="sxs-lookup"><span data-stu-id="73a72-139">The output of the service is a sequence of length n with a binomial distribution based on the size and prob arguments.</span></span>

> <span data-ttu-id="73a72-140">Ez a szolgáltatás az Azure piactéren kihelyezett egy OData-szolgáltatás; a POST vagy GET módszerrel elnevezése lehet.</span><span class="sxs-lookup"><span data-stu-id="73a72-140">This service, as hosted on the Azure Marketplace, is an OData service; these may be called through POST or GET methods.</span></span> 
> 
> 

<span data-ttu-id="73a72-141">Többféleképpen is az automatizált módon a szolgáltatás fel (például alkalmazások olyan itt: [generátor](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionGenerator.aspx), [valószínűség Számológép](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionProbabilityCalculator.aspx), [ki osztóérték Számológép](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionQuantileCalculator)).</span><span class="sxs-lookup"><span data-stu-id="73a72-141">There are multiple ways of consuming the service in an automated fashion (example apps are here: [Generator](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionGenerator.aspx), [Probability Calculator](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionProbabilityCalculator.aspx), [Quantile Calculator](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionQuantileCalculator)).</span></span> 

### <a name="starting-c-code-for-web-service-consumption"></a><span data-ttu-id="73a72-142">C#-kódban a webes szolgáltatások felhasználásához megkezdése:</span><span class="sxs-lookup"><span data-stu-id="73a72-142">Starting C# code for web service consumption:</span></span>
### <a name="binomial-distribution-quantile-calculator"></a><span data-ttu-id="73a72-143">Kudarc_szám ki osztóérték Számológép</span><span class="sxs-lookup"><span data-stu-id="73a72-143">Binomial Distribution Quantile Calculator</span></span>
    public class Input
    {
            public string p;
            public string size;
            public string prob;
            public string side;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void main()
    {
            var input = new Input() { p = TextBox1.Text, size = TextBox2.Text, prob = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();

            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));

            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }

### <a name="binomial-distribution-probability-calculator"></a><span data-ttu-id="73a72-144">Kudarc_szám valószínűség Számológép</span><span class="sxs-lookup"><span data-stu-id="73a72-144">Binomial Distribution Probability Calculator</span></span>
    public class Input
    {
            public string q;
            public string size;
            public string prob;
            public string side;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { q = TextBox1.Text, size = TextBox2.Text, prob = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = " PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();

            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));

            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }


### <a name="binomial-distribution-generator"></a><span data-ttu-id="73a72-145">Kudarc_szám generátor</span><span class="sxs-lookup"><span data-stu-id="73a72-145">Binomial Distribution Generator</span></span>
    public class Input
    {
            public string n;
            public string size;
            public string p;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { n = TextBox1.Text, size = TextBox2.Text, p = TextBox3.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();

            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));

            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }





## <a name="creation-of-web-service"></a><span data-ttu-id="73a72-146">Webes szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="73a72-146">Creation of web service</span></span>
> <span data-ttu-id="73a72-147">Ez a webszolgáltatás Azure Machine Learning segítségével lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="73a72-147">This web service was created using Azure Machine Learning.</span></span> <span data-ttu-id="73a72-148">Az ingyenes próbaverzió, valamint a bevezető videó kísérletek létrehozásával és [közzétételi webes szolgáltatások](machine-learning-publish-a-machine-learning-web-service.md), lásd: [azure.com/ml](http://azure.com/ml).</span><span class="sxs-lookup"><span data-stu-id="73a72-148">For a free trial, as well as introductory videos on creating experiments and [publishing web services](machine-learning-publish-a-machine-learning-web-service.md), please see [azure.com/ml](http://azure.com/ml).</span></span> <span data-ttu-id="73a72-149">Az alábbiakban van egy Képernyőkép a kísérlet, amely a webes szolgáltatás, és példa kód létre minden egyes belül modulok.</span><span class="sxs-lookup"><span data-stu-id="73a72-149">Below is a screenshot of the experiment that created the web service and example code for each of the modules within the experiment.</span></span>
> 
> 

### <a name="binomial-distribution-quantile-calculator"></a><span data-ttu-id="73a72-150">Kudarc_szám ki osztóérték Számológép</span><span class="sxs-lookup"><span data-stu-id="73a72-150">Binomial Distribution Quantile Calculator</span></span>
![Munkaterület létrehozása][4]

#### <a name="module-1"></a><span data-ttu-id="73a72-152">1. modul:</span><span class="sxs-lookup"><span data-stu-id="73a72-152">Module 1:</span></span>
    #data schema with example data (replaced with data from web service)
    data.set=data.frame(p=0.1,size=10,prob=.5,side='L');
    maml.mapOutputPort("data.set"); #send data to output port
#### <a name="module-2"></a><span data-ttu-id="73a72-153">A modul 2:</span><span class="sxs-lookup"><span data-stu-id="73a72-153">Module 2:</span></span>
    dataset1 <- maml.mapInputPort(1) # class: data.frame
    param = dataset1
    if (param$p < 0 ) {
    print('Bad input: p must be between 0 and 1')
    param$p = 0
    } else if (param$p > 1) {
    print('Bad input: p must be between 0 and 1')
    param$p = 1
    }

    if (param$prob < 0 ) {
    print('Bad input: prob must be between 0 and 1')
    param$prob = 0
    } else if (param$prob > 1) {
    print('Bad input: prob must be between 0 and 1')
    param$prob = 1
    }

    quantile = qbinom(param$p,size=param$size,prob=param$prob)
    df = data.frame(x=1:param$size, prob=dbinom(1:param$size, param$size, prob=param$prob))
    quantile

    if (param$side == 'U'){
    quantile = qbinom(param$p,size=param$size,prob=param$prob,lower.tail = F)
    band=subset(df,x>quantile)
    } else if (param$side =='L') {
    quantile = qbinom(param$p,size=param$size,prob=param$prob,lower.tail = T)
    band=subset(df,x<=quantile)
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(quantile)

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");


### <a name="binomial-distribution-probability-calculator"></a><span data-ttu-id="73a72-154">Kudarc_szám valószínűség Számológép</span><span class="sxs-lookup"><span data-stu-id="73a72-154">Binomial Distribution Probability Calculator</span></span>
![Munkaterület létrehozása][5]

#### <a name="module-1"></a><span data-ttu-id="73a72-156">1. modul:</span><span class="sxs-lookup"><span data-stu-id="73a72-156">Module 1:</span></span>
    #data schema with example data (replaced with data from web service)
    data.set=data.frame(q=5,size=10,prob=.5,side='L');
    maml.mapOutputPort("data.set"); #send data to output port


#### <a name="module-2"></a><span data-ttu-id="73a72-157">A modul 2:</span><span class="sxs-lookup"><span data-stu-id="73a72-157">Module 2:</span></span>
    dataset1 <- maml.mapInputPort(1) # class: data.frame
    param = dataset1
    prob = pbinom(param$q,size=param$size,prob=param$prob)
    prob.eq = dbinom(param$q,size=param$size,prob=param$prob)
    df = data.frame(x=1:param$size, prob=dbinom(1:param$size, param$size, prob=param$prob))
    prob

    if (param$side == 'U'){
    prob = 1 - prob
    band=subset(df,x>param$q)
    } else if (param$side =='E') {
    prob = prob.eq
    band=subset(df,x==param$q)
    } else if (param$side =='L') {
    prob = prob
    band=subset(df,x<=param$q)
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(prob)

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");

### <a name="binomial-distribution-generator"></a><span data-ttu-id="73a72-158">Kudarc_szám generátor</span><span class="sxs-lookup"><span data-stu-id="73a72-158">Binomial Distribution Generator</span></span>
![Munkaterület létrehozása][6]

#### <a name="module-1"></a><span data-ttu-id="73a72-160">1. modul:</span><span class="sxs-lookup"><span data-stu-id="73a72-160">Module 1:</span></span>
    #data schema with example data (replaced with data from web service)
    data.set=data.frame(n=50,size=10,p=.5);
    maml.mapOutputPort("data.set"); #send data to output port

#### <a name="module-2"></a><span data-ttu-id="73a72-161">A modul 2:</span><span class="sxs-lookup"><span data-stu-id="73a72-161">Module 2:</span></span>
    dataset1 <- maml.mapInputPort(1) # class: data.frame
    param = dataset1
    dist = rbinom(param$n,param$size,param$p)

    output = as.data.frame(t(dist))

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");

## <a name="limitations"></a><span data-ttu-id="73a72-162">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="73a72-162">Limitations</span></span>
<span data-ttu-id="73a72-163">Példák nagyon egyszerű kudarc_szám körülvevő.</span><span class="sxs-lookup"><span data-stu-id="73a72-163">These are very simple examples surrounding the binomial distribution.</span></span> <span data-ttu-id="73a72-164">A fenti példa kódot is látható, mert kevés hiba alatt van megvalósítva.</span><span class="sxs-lookup"><span data-stu-id="73a72-164">As can be seen from the example code above, little error catching is implemented.</span></span>

## <a name="faq"></a><span data-ttu-id="73a72-165">GYIK</span><span class="sxs-lookup"><span data-stu-id="73a72-165">FAQ</span></span>
<span data-ttu-id="73a72-166">Gyakori kérdések a felhasználás a webszolgáltatás vagy az Azure piactéren közzétételt, lásd: [Itt](machine-learning-marketplace-faq.md).</span><span class="sxs-lookup"><span data-stu-id="73a72-166">For frequently asked questions on consumption of the web service or publishing to the Azure Marketplace, see [here](machine-learning-marketplace-faq.md).</span></span>

[1]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_1.png

[2]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_2.png

[3]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_3.png

[4]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_4.png

[5]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_5.png

[6]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_6.png

