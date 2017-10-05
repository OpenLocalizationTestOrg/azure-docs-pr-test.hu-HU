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
# <a name="deprecated-normal-distribution-suite"></a>(elavult) Normál terjesztési csomag

> [!NOTE]
> A Microsoft DataMarket használatból van, és ez az API már elavult. 
> 
> Sok hasznos példa kísérletek és API-k a [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com). A gyűjtemény kapcsolatos további információkért lásd: [megosztást, és felderítik a Cortana Intelligence Gallery erőforrások](machine-learning-gallery-how-to-use-contribute-publish.md).

A normál terjesztési Suite egy olyan minta webszolgáltatások ([generátor](https://datamarket.azure.com/dataset/aml_labs/ndg7), [ki osztóérték Számológép](https://datamarket.azure.com/dataset/aml_labs/ndq5), [valószínűség Számológép](https://datamarket.azure.com/dataset/aml_labs/ndp5)), amely megkönnyíti a létrehozása és kezelése normál terjesztési. A szolgáltatások lehetővé teszik, egy normál terjesztési feladatütemezési hossza, az adott valószínűséggel quantiles kiszámítása, és az egy adott ki osztóérték valószínűség kiszámítása létrehozásakor. A szolgáltatások bocsát ki a kijelölt szolgáltatás alapján különböző kimenetek (lásd az alábbi leírása). A normál terjesztési Suite az R funkciók qnorm, rnorm és pnorm, amely R statisztikák csomagban található alapul.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

> Ez a webszolgáltatás kell fenntartania – potenciálisan végig a mobilalkalmazások a webhelyen keresztül, vagy akár a helyi számítógépen, a felhasználókat például. De a webszolgáltatás célja is példa bemutatja, hogyan Azure Machine Learning webszolgáltatások fölött R-kód létrehozásához használható kiszolgálásához. Az R-kód csupán néhány sornyi és az Azure Machine Learning Studio egy gombját kattint egy kísérlet hozható létre az R-kód és webszolgáltatásként közzé. A webszolgáltatás majd közzé az Azure piactéren, és felhasználók és eszközök által felhasznált világszerte a szerző, a webszolgáltatás által infrastruktúra beállítás nélkül.  
> 
> 

## <a name="consumption-of-web-service"></a>Felhasználási webszolgáltatás
A normál terjesztési csomag a következő 3 szolgáltatásokat tartalmazza.

### <a name="normal-distribution-quantile-calculator"></a>Normális eloszlás ki osztóérték Számológép
Ez a szolgáltatás a normális eloszlás 4 argumentumként fogadja, és a társított ki osztóérték számítja ki.

A bemeneti argumentumok a következők:

* p - egy egyetlen, a normális eloszlás esemény valószínűségét. 
* Közepes – normális eloszlás középértékét.
* SD - normális eloszlás szórását. 
* Ügyféloldali - L alsó részén a telepítési és a terjesztési felső oldalán U.

A szolgáltatás a számított ki az adott valószínűséggel társított osztóérték eredménye.

### <a name="normal-distribution-probability-calculator"></a>Normális eloszlás valószínűség Számológép
Ez a szolgáltatás a normális eloszlás 4 argumentumként fogadja, és kiszámítja a társított valószínűség.

A bemeneti argumentumok a következők:

* q - a normál terjesztési esemény egy egyetlen ki azt osztóérték. 
* Közepes – normális eloszlás középértékét.
* SD - normális eloszlás szórását. 
* Ügyféloldali - L alsó részén a telepítési és a terjesztési felső oldalán U.

A szolgáltatás a számított annak valószínűségét, hogy az adott ki osztóérték társított eredménye.

### <a name="normal-distribution-generator"></a>Normális eloszlás generátor
Ez a szolgáltatás fogadja el a normális eloszlás 3 argumentum, és általában elosztott véletlenszerű számsorozatot állít elő. A következő argumentumok belül a kérelem hozzá kell adni:

* n - megfigyelések száma. 
* Közepes – normális eloszlás középértékét.
* SD - normális eloszlás szórását. 

A szolgáltatás eredménye alapján a középérték és sd argumentumok normális eloszlás a hossza n sorozata.

> Ez a szolgáltatás az Azure piactéren kihelyezett egy OData-szolgáltatás; a POST vagy GET módszerrel elnevezése lehet. 
> 
> 

Többféleképpen is az automatizált módon a szolgáltatás fel (például alkalmazások olyan itt: [generátor](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionGenerator.aspx), [valószínűség Számológép](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionProbabilityCalculator.aspx), [ki osztóérték Számológép](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionQuantileCalculator.aspx)).

### <a name="starting-c-code-for-web-service-consumption"></a>C#-kódban a webes szolgáltatások felhasználásához megkezdése:
### <a name="normal-distribution-quantile-calculator"></a>Normális eloszlás ki osztóérték Számológép
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


### <a name="normal-distribution-probability-calculator"></a>Normális eloszlás valószínűség Számológép
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

### <a name="normal-distribution-generator"></a>Normális eloszlás generátor
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


## <a name="creation-of-web-service"></a>Webes szolgáltatás létrehozása
> Ez a webszolgáltatás Azure Machine Learning segítségével lett létrehozva. Az ingyenes próbaverzió, valamint a bevezető videó kísérletek létrehozásával és [közzétételi webes szolgáltatások](machine-learning-publish-a-machine-learning-web-service.md), lásd: [azure.com/ml](http://azure.com/ml). 
> 
> 

Az alábbiakban van egy Képernyőkép a kísérlet, amely a webes szolgáltatás, és példa kód létre minden egyes belül modulok.

### <a name="normal-distribution-quantile-calculator"></a>Normális eloszlás ki osztóérték Számológép
Kísérlet folyamata:

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

### <a name="normal-distribution-probability-calculator"></a>Normális eloszlás valószínűség Számológép
Kísérlet folyamata:

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

### <a name="normal-distribution-generator"></a>Normális eloszlás generátor
Kísérlet folyamata:

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

## <a name="limitations"></a>Korlátozások
Példák nagyon egyszerű körülvevő a normális eloszlás. A fenti példa kódot is látható, mert kevés hiba alatt van megvalósítva.

## <a name="faq"></a>GYIK
Gyakori kérdések a felhasználás a webszolgáltatás vagy az Azure piactéren közzétételt, lásd: [Itt](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-normal-distribution/normal-img1.png
[2]: ./media/machine-learning-r-csharp-normal-distribution/normal-img2.png
[3]: ./media/machine-learning-r-csharp-normal-distribution/normal-img3.png
[4]: ./media/machine-learning-r-csharp-normal-distribution/normal-img4.png

