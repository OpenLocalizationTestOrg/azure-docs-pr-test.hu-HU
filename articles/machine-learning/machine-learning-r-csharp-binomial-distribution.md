---
title: "Azure kudarc_szám Suite - aaa(deprecated) |} Microsoft Docs"
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
redirect_document_id: True
ms.openlocfilehash: 6f94436cd19abeb518d179f340c8d4f43fcf4520
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deprecated-binomial-distribution-suite"></a><span data-ttu-id="090a5-103">(elavult) Kudarc_szám csomag</span><span class="sxs-lookup"><span data-stu-id="090a5-103">(deprecated) Binomial Distribution Suite</span></span>

> [!NOTE]
> <span data-ttu-id="090a5-104">a Microsoft DataMarket hello használatból van, és ez az API már elavult.</span><span class="sxs-lookup"><span data-stu-id="090a5-104">hello Microsoft DataMarket is being retired and this API has been deprecated.</span></span> 
> 
> <span data-ttu-id="090a5-105">Sok hasznos példa kísérletek és API-kat az található hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="090a5-105">You can find many useful example experiments and APIs in hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="090a5-106">Gyűjteményelem hello kapcsolatos további információkért lásd: [megosztás és a Cortana Intelligence Gallery hello erőforrások felderítéséhez](machine-learning-gallery-how-to-use-contribute-publish.md).</span><span class="sxs-lookup"><span data-stu-id="090a5-106">For more information about hello Gallery, see [Share and discover resources in hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span></span>

<span data-ttu-id="090a5-107">hello kudarc_szám Suite egy olyan minta webszolgáltatásokat ([binomiális generátor](https://datamarket.azure.com/dataset/aml_labs/bdg5), [valószínűség Számológép](https://datamarket.azure.com/dataset/aml_labs/bdp4), [ki osztóérték Számológép](https://datamarket.azure.com/dataset/aml_labs/bdq5)), amelyekkel generálásához és binomiális terjesztéseket foglalkozik.</span><span class="sxs-lookup"><span data-stu-id="090a5-107">hello Binomial Distribution Suite is a set of sample web services ([Binomial Generator](https://datamarket.azure.com/dataset/aml_labs/bdg5), [Probability Calculator](https://datamarket.azure.com/dataset/aml_labs/bdp4), [Quantile Calculator](https://datamarket.azure.com/dataset/aml_labs/bdq5)) that help in generating and dealing with binomial distributions.</span></span> <span data-ttu-id="090a5-108">hello szolgáltatások engedélyezése bármely hossza quantiles kiszámítása kudarc_szám sorozatát generálása kívüli adott valószínűség és számítási valószínűség az egy adott ki osztóérték.</span><span class="sxs-lookup"><span data-stu-id="090a5-108">hello services allow generating a binomial distribution sequence of any length, calculating quantiles out of given probability and calculating probability from a given quantile.</span></span> <span data-ttu-id="090a5-109">Hello szolgáltatások bocsát ki a kijelölt hello szolgáltatás alapján különböző kimenetek (lásd az alábbi leírása).</span><span class="sxs-lookup"><span data-stu-id="090a5-109">Each of hello services emits different outputs based on hello selected service (see description below).</span></span> <span data-ttu-id="090a5-110">hello kudarc_szám Suite hello R funkciók qbinom rbinom és pbinom, amely hello R statisztikák csomagban található alapul.</span><span class="sxs-lookup"><span data-stu-id="090a5-110">hello Binomial Distribution Suite is based on hello R functions qbinom, rbinom, and pbinom, which are included in hello R stats package.</span></span> 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

> <span data-ttu-id="090a5-111">Ezek a webszolgáltatások kell fenntartania felhasználók – potenciálisan közvetlenül a hello piactéren keresztül a mobilalkalmazások a webhelyen keresztül, vagy akár a helyi számítógépen, például.</span><span class="sxs-lookup"><span data-stu-id="090a5-111">These web services could be consumed by users – potentially directly on hello marketplace, through a mobile app, through a website, or even on a local computer, for example.</span></span> <span data-ttu-id="090a5-112">De hello hello webszolgáltatás célja is tooserve példa bemutatja, hogyan Azure Machine Learning webszolgáltatások használt toocreate fölött R-kód is lehet.</span><span class="sxs-lookup"><span data-stu-id="090a5-112">But hello purpose of hello web service is also tooserve as an example of how Azure Machine Learning can be used toocreate web services on top of R code.</span></span> <span data-ttu-id="090a5-113">Az R-kód csupán néhány sornyi és az Azure Machine Learning Studio egy gombját kattint egy kísérlet hozható létre az R-kód és webszolgáltatásként közzé.</span><span class="sxs-lookup"><span data-stu-id="090a5-113">With just a few lines of R code and clicks of a button within Azure Machine Learning Studio, an experiment can be created with R code and published as a web service.</span></span> <span data-ttu-id="090a5-114">hello webszolgáltatás majd lehet közzétett toohello Azure piactéren, és különböző hello world – felhasználók és eszközök által felhasznált nincs infrastruktúra hello Szerző hello webszolgáltatás kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="090a5-114">hello web service can then be published toohello Azure Marketplace and consumed by users and devices across hello world – no infrastructure setup by hello author of hello web service is required.</span></span>
> 
> 

## <a name="consumption-of-web-service"></a><span data-ttu-id="090a5-115">Felhasználási webszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="090a5-115">Consumption of web service</span></span>
<span data-ttu-id="090a5-116">hello kudarc_szám csomag magában foglalja a következő 3 szolgáltatások hello.</span><span class="sxs-lookup"><span data-stu-id="090a5-116">hello Binomial Distribution Suite includes hello following 3 services.</span></span>

### <a name="binomial-distribution-quantile-calculator"></a><span data-ttu-id="090a5-117">Kudarc_szám ki osztóérték Számológép</span><span class="sxs-lookup"><span data-stu-id="090a5-117">Binomial Distribution Quantile Calculator</span></span>
<span data-ttu-id="090a5-118">Ez a szolgáltatás a normális eloszlás 4 argumentumként fogadja, és társított hello ki osztóérték számítja ki.</span><span class="sxs-lookup"><span data-stu-id="090a5-118">This service accepts 4 arguments of a normal distribution and calculates hello associated quantile.</span></span>
<span data-ttu-id="090a5-119">hello bemeneti argumentumai a következők:</span><span class="sxs-lookup"><span data-stu-id="090a5-119">hello input arguments are:</span></span>

* <span data-ttu-id="090a5-120">p - egyetlen összevont több kísérletek valószínűségét.</span><span class="sxs-lookup"><span data-stu-id="090a5-120">p - A single aggregated probability of multiple trials.</span></span>  
* <span data-ttu-id="090a5-121">méret - hello kísérletek száma.</span><span class="sxs-lookup"><span data-stu-id="090a5-121">size - hello number of trials.</span></span>
* <span data-ttu-id="090a5-122">valószínűség - próbaverzió sikeres hello valószínűségét.</span><span class="sxs-lookup"><span data-stu-id="090a5-122">prob - hello probability of success in a trial.</span></span>
* <span data-ttu-id="090a5-123">Az oldal - L hello alsó részén hello terjesztési, U hello hello terjesztési felső oldalán.</span><span class="sxs-lookup"><span data-stu-id="090a5-123">Side - L for hello lower side of hello distribution, U for hello upper side of hello distribution.</span></span> 

<span data-ttu-id="090a5-124">hello hello szolgáltatás eredménye hello számított ki, amely kapcsolódik az adott valószínűség hello osztóérték.</span><span class="sxs-lookup"><span data-stu-id="090a5-124">hello output of hello service is hello calculated quantile that is associated with hello given probability.</span></span>

### <a name="binomial-distribution-probability-calculator"></a><span data-ttu-id="090a5-125">Kudarc_szám valószínűség Számológép</span><span class="sxs-lookup"><span data-stu-id="090a5-125">Binomial Distribution Probability Calculator</span></span>
<span data-ttu-id="090a5-126">Ez a szolgáltatás egy binomiális eloszlás 4 argumentumként fogadja, és kiszámítja az hello rendelt valószínűség.</span><span class="sxs-lookup"><span data-stu-id="090a5-126">This service accepts 4 arguments of a binomial distribution and calculates hello associated probability.</span></span>
<span data-ttu-id="090a5-127">hello bemeneti argumentumai a következők:</span><span class="sxs-lookup"><span data-stu-id="090a5-127">hello input arguments are:</span></span>

* <span data-ttu-id="090a5-128">a kérdések - egy egyetlen ki osztóérték kudarc_szám az esemény.</span><span class="sxs-lookup"><span data-stu-id="090a5-128">q - A single quantile of an event with binomial distribution.</span></span> 
* <span data-ttu-id="090a5-129">méret - hello kísérletek száma.</span><span class="sxs-lookup"><span data-stu-id="090a5-129">size - hello number of trials.</span></span>
* <span data-ttu-id="090a5-130">valószínűség - próbaverzió sikeres hello valószínűségét.</span><span class="sxs-lookup"><span data-stu-id="090a5-130">prob - hello probability of success in a trial.</span></span>
* <span data-ttu-id="090a5-131">az oldal - L hello alsó részén hello terjesztési U hello terjesztési felső oldalán hello vagy, amely sikeres egyetlen száma egyenlő tooa E.</span><span class="sxs-lookup"><span data-stu-id="090a5-131">side - L for hello lower side of hello distribution, U for hello upper side of hello distribution, or E that is equal tooa single number of successes.</span></span>

<span data-ttu-id="090a5-132">hello hello szolgáltatás eredménye számított hello annak valószínűségét, hogy a megadott ki osztóérték hello van társítva.</span><span class="sxs-lookup"><span data-stu-id="090a5-132">hello output of hello service is hello calculated probability that is associated with hello given quantile.</span></span>

### <a name="binomial-distribution-generator"></a><span data-ttu-id="090a5-133">Kudarc_szám generátor</span><span class="sxs-lookup"><span data-stu-id="090a5-133">Binomial Distribution Generator</span></span>
<span data-ttu-id="090a5-134">Ez a szolgáltatás egy binomiális eloszlás 3 argumentumként fogadja, és létrehoz egy véletlenszerű számsorozatot binomially elosztott.</span><span class="sxs-lookup"><span data-stu-id="090a5-134">This service accepts 3 arguments of a binomial distribution and generates a random sequence of numbers that are binomially distributed.</span></span> <span data-ttu-id="090a5-135">hello következő szabad megadni típusargumentumot tooit hello kérelem belül:</span><span class="sxs-lookup"><span data-stu-id="090a5-135">hello following arguments should be provided tooit within hello request:</span></span>

* <span data-ttu-id="090a5-136">n - megfigyelések száma.</span><span class="sxs-lookup"><span data-stu-id="090a5-136">n - Number of observations.</span></span> 
* <span data-ttu-id="090a5-137">méret - kísérletek száma.</span><span class="sxs-lookup"><span data-stu-id="090a5-137">size - Number of trials.</span></span>
* <span data-ttu-id="090a5-138">valószínűség - sikeres valószínűségét.</span><span class="sxs-lookup"><span data-stu-id="090a5-138">prob - Probability of success.</span></span>

<span data-ttu-id="090a5-139">hello hello szolgáltatás eredménye egy binomiális elosztás alapján hello mérete és a valószínűség argumentum hossza n sorozata.</span><span class="sxs-lookup"><span data-stu-id="090a5-139">hello output of hello service is a sequence of length n with a binomial distribution based on hello size and prob arguments.</span></span>

> <span data-ttu-id="090a5-140">Ez a szolgáltatás az Azure piactér hello kihelyezett egy OData-szolgáltatás; a POST vagy GET módszerrel elnevezése lehet.</span><span class="sxs-lookup"><span data-stu-id="090a5-140">This service, as hosted on hello Azure Marketplace, is an OData service; these may be called through POST or GET methods.</span></span> 
> 
> 

<span data-ttu-id="090a5-141">Többféleképpen is az automatizált módon hello szolgáltatás fel (például alkalmazások olyan itt: [generátor](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionGenerator.aspx), [valószínűség Számológép](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionProbabilityCalculator.aspx), [ki osztóérték Számológép](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionQuantileCalculator)).</span><span class="sxs-lookup"><span data-stu-id="090a5-141">There are multiple ways of consuming hello service in an automated fashion (example apps are here: [Generator](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionGenerator.aspx), [Probability Calculator](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionProbabilityCalculator.aspx), [Quantile Calculator](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionQuantileCalculator)).</span></span> 

### <a name="starting-c-code-for-web-service-consumption"></a><span data-ttu-id="090a5-142">C#-kódban a webes szolgáltatások felhasználásához megkezdése:</span><span class="sxs-lookup"><span data-stu-id="090a5-142">Starting C# code for web service consumption:</span></span>
### <a name="binomial-distribution-quantile-calculator"></a><span data-ttu-id="090a5-143">Kudarc_szám ki osztóérték Számológép</span><span class="sxs-lookup"><span data-stu-id="090a5-143">Binomial Distribution Quantile Calculator</span></span>
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

### <a name="binomial-distribution-probability-calculator"></a><span data-ttu-id="090a5-144">Kudarc_szám valószínűség Számológép</span><span class="sxs-lookup"><span data-stu-id="090a5-144">Binomial Distribution Probability Calculator</span></span>
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


### <a name="binomial-distribution-generator"></a><span data-ttu-id="090a5-145">Kudarc_szám generátor</span><span class="sxs-lookup"><span data-stu-id="090a5-145">Binomial Distribution Generator</span></span>
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





## <a name="creation-of-web-service"></a><span data-ttu-id="090a5-146">Webes szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="090a5-146">Creation of web service</span></span>
> <span data-ttu-id="090a5-147">Ez a webszolgáltatás Azure Machine Learning segítségével lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="090a5-147">This web service was created using Azure Machine Learning.</span></span> <span data-ttu-id="090a5-148">Az ingyenes próbaverzió, valamint a bevezető videó kísérletek létrehozásával és [közzétételi webes szolgáltatások](machine-learning-publish-a-machine-learning-web-service.md), lásd: [azure.com/ml](http://azure.com/ml).</span><span class="sxs-lookup"><span data-stu-id="090a5-148">For a free trial, as well as introductory videos on creating experiments and [publishing web services](machine-learning-publish-a-machine-learning-web-service.md), please see [azure.com/ml](http://azure.com/ml).</span></span> <span data-ttu-id="090a5-149">Az alábbiakban van egy képernyőfelvétel a hello webes szolgáltatás, és példa kódot az egyes hello modulok hello kísérlet belül létrehozott hello kísérlet.</span><span class="sxs-lookup"><span data-stu-id="090a5-149">Below is a screenshot of hello experiment that created hello web service and example code for each of hello modules within hello experiment.</span></span>
> 
> 

### <a name="binomial-distribution-quantile-calculator"></a><span data-ttu-id="090a5-150">Kudarc_szám ki osztóérték Számológép</span><span class="sxs-lookup"><span data-stu-id="090a5-150">Binomial Distribution Quantile Calculator</span></span>
![Munkaterület létrehozása][4]

#### <a name="module-1"></a><span data-ttu-id="090a5-152">1. modul:</span><span class="sxs-lookup"><span data-stu-id="090a5-152">Module 1:</span></span>
    #data schema with example data (replaced with data from web service)
    data.set=data.frame(p=0.1,size=10,prob=.5,side='L');
    maml.mapOutputPort("data.set"); #send data toooutput port
#### <a name="module-2"></a><span data-ttu-id="090a5-153">A modul 2:</span><span class="sxs-lookup"><span data-stu-id="090a5-153">Module 2:</span></span>
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

    # Select data.frame toobe sent toohello output Dataset port
    maml.mapOutputPort("output");


### <a name="binomial-distribution-probability-calculator"></a><span data-ttu-id="090a5-154">Kudarc_szám valószínűség Számológép</span><span class="sxs-lookup"><span data-stu-id="090a5-154">Binomial Distribution Probability Calculator</span></span>
![Munkaterület létrehozása][5]

#### <a name="module-1"></a><span data-ttu-id="090a5-156">1. modul:</span><span class="sxs-lookup"><span data-stu-id="090a5-156">Module 1:</span></span>
    #data schema with example data (replaced with data from web service)
    data.set=data.frame(q=5,size=10,prob=.5,side='L');
    maml.mapOutputPort("data.set"); #send data toooutput port


#### <a name="module-2"></a><span data-ttu-id="090a5-157">A modul 2:</span><span class="sxs-lookup"><span data-stu-id="090a5-157">Module 2:</span></span>
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

    # Select data.frame toobe sent toohello output Dataset port
    maml.mapOutputPort("output");

### <a name="binomial-distribution-generator"></a><span data-ttu-id="090a5-158">Kudarc_szám generátor</span><span class="sxs-lookup"><span data-stu-id="090a5-158">Binomial Distribution Generator</span></span>
![Munkaterület létrehozása][6]

#### <a name="module-1"></a><span data-ttu-id="090a5-160">1. modul:</span><span class="sxs-lookup"><span data-stu-id="090a5-160">Module 1:</span></span>
    #data schema with example data (replaced with data from web service)
    data.set=data.frame(n=50,size=10,p=.5);
    maml.mapOutputPort("data.set"); #send data toooutput port

#### <a name="module-2"></a><span data-ttu-id="090a5-161">A modul 2:</span><span class="sxs-lookup"><span data-stu-id="090a5-161">Module 2:</span></span>
    dataset1 <- maml.mapInputPort(1) # class: data.frame
    param = dataset1
    dist = rbinom(param$n,param$size,param$p)

    output = as.data.frame(t(dist))

    # Select data.frame toobe sent toohello output Dataset port
    maml.mapOutputPort("output");

## <a name="limitations"></a><span data-ttu-id="090a5-162">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="090a5-162">Limitations</span></span>
<span data-ttu-id="090a5-163">Példák nagyon egyszerű hello kudarc_szám körülvevő.</span><span class="sxs-lookup"><span data-stu-id="090a5-163">These are very simple examples surrounding hello binomial distribution.</span></span> <span data-ttu-id="090a5-164">Hello példakódot fent is látható, mert kevés hiba alatt van megvalósítva.</span><span class="sxs-lookup"><span data-stu-id="090a5-164">As can be seen from hello example code above, little error catching is implemented.</span></span>

## <a name="faq"></a><span data-ttu-id="090a5-165">GYIK</span><span class="sxs-lookup"><span data-stu-id="090a5-165">FAQ</span></span>
<span data-ttu-id="090a5-166">Gyakori kérdések a felhasználás hello webszolgáltatás vagy az Azure piactér közzétételi toohello, lásd: [Itt](machine-learning-marketplace-faq.md).</span><span class="sxs-lookup"><span data-stu-id="090a5-166">For frequently asked questions on consumption of hello web service or publishing toohello Azure Marketplace, see [here](machine-learning-marketplace-faq.md).</span></span>

[1]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_1.png

[2]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_2.png

[3]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_3.png

[4]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_4.png

[5]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_5.png

[6]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_6.png

