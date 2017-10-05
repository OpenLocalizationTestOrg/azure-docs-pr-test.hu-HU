---
title: "(elavult) Azure - ETS + STL - előrejelzés |} Microsoft Docs"
description: "(elavult) Az előrejelzés - ETS + STL"
services: machine-learning
documentationcenter: 
author: xueshanz
manager: jhubbard
editor: cgronlun
ms.assetid: 153eab4d-6293-45e1-9871-ec339e810dd9
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: yijichen
ROBOTS: NOINDEX
redirect_url: https://gallery.cortanaintelligence.com/
redirect_document_id: TRUE
ms.openlocfilehash: a575af931a41b7a55eb2102f3553640a16099146
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="deprecated-forecasting---ets--stl"></a>(elavult) Az előrejelzés - ETS + STL

> [!NOTE]
> A Microsoft DataMarket használatból van, és ez az API már elavult. 
> 
> Sok hasznos példa kísérletek és API-k a [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com). A gyűjtemény kapcsolatos további információkért lásd: [megosztást, és felderítik a Cortana Intelligence Gallery erőforrások](machine-learning-gallery-how-to-use-contribute-publish.md).

Ez [webszolgáltatás](https://datamarket.azure.com/dataset/aml_labs/demand_forecast) megvalósítja határozza Trend felbontás ellen (STL) és exponenciális simítás (ETS) modellek előrejelzéseket, a felhasználó által megadott korábbi adatok alapján történő létrehozásához. A termék igény szerinti növeli az év? Is szeretnék előre jelezni az értékesítés a Karácsony szezon az, hogy I hatékonyan megtervezheti a készlet? Előrejelzési modellek alkalmasak ilyen kérdések megoldására. A régebbi adatokat adott meg, ezek a modellek vizsgálja meg, rejtett trendek és a szezonalitás értékének előre jelezni jövőbeli trendeket. 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

> Ez a webszolgáltatás kell fenntartania – potenciálisan végig a mobilalkalmazások a webhelyen keresztül, vagy akár a helyi számítógépen, a felhasználókat például. De a webszolgáltatás célja is példa bemutatja, hogyan Azure Machine Learning webszolgáltatások fölött R-kód létrehozásához használható kiszolgálásához. Az R-kód csupán néhány sornyi és az Azure Machine Learning Studio egy gombját kattint egy kísérlet hozható létre az R-kód és webszolgáltatásként közzé. A webszolgáltatás majd közzé az Azure piactéren, és felhasználók és eszközök által felhasznált világszerte a szerző, a webszolgáltatás által infrastruktúra beállítás nélkül.  
> 
> 

## <a name="consumption-of-web-service"></a>Felhasználási webszolgáltatás
Ez a szolgáltatás 4 argumentumként fogadja, és kiszámítja az előrejelzések.
A bemeneti argumentumok a következők:

* Gyakoriság - azt jelzi, hogy a nyers adatokat (napi vagy heti vagy havi/negyedéves/évente) gyakoriságát.
* Horizon - jövőbeli előrejelzési időkereten belül.
* Dátum - idő adja hozzá az új idő adatsorozat adatainak.
* Érték – adja hozzá az új idő adatok sorozatértékek.

A szolgáltatás a számított előre jelzett értékek eredménye.

A minta bemeneti lehet: 

* Gyakorisága – 12
* Horizon - 12
* Dátum - 1/15/2012; 2/15/2012; 3 15/2012; 4/15/2012; 2012-5-15; 6/15/2012; 15/7/2012; 8 / 15/2012; a 9/15/2012; a 10/15/2012; a 11/15/2012; a 12/15/2012; 1/15/2013; 2/15/2013; 3/15/2013; 4/15/2013; 5 vagy 15/2013; 6/15/2013; 7/15/2013; 8 / 15/2013; 9/15/2013; 10/15/2013; 11/15/2013; 12/15/2013; 1/15/2014; 2/15/2014; 3/15/2014; 4/15/2014; 5 vagy 15/2014; 6/15/2014; 7/15/2014; 8 / 15/2014; 9/15/2014
* Érték - 3.479; 3.68; 3.832; 3.941; 3.797; 3.586; 3.508; 3.731; 3.915; 3.844; 3.634; 3.549; 3.557; 3.785; 3.782; 3.601; 3.544; 3.556; 3.65; 3.709; 3.682; 3.511; 3.429; 3.51; 3.523; 3.525; 3.626; 3.695; 3.711; 3.711; 3.693; 3.571; 3.509

> Ez a szolgáltatás az Azure piactéren kihelyezett egy OData-szolgáltatás; a POST vagy GET módszerrel elnevezése lehet. 
> 
> 

Többféleképpen is az automatizált módon a szolgáltatás fel (egy példa alkalmazás [Itt](http://microsoftazuremachinelearning.azurewebsites.net/StlEtsForecasting.aspx)).

### <a name="starting-c-code-for-web-service-consumption"></a>C#-kódban a webes szolgáltatások felhasználásához megkezdése:
    public class Input
    {
            public string frequency;
            public string horizon;
            public string date;
            public string value;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { frequency = TextBox1.Text, horizon = TextBox2.Text, date = TextBox3.Text, value = TextBox4.Text };         var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();

            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));

            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }


## <a name="creation-of-web-service"></a>Webes szolgáltatás létrehozása
> Ez a webszolgáltatás Azure Machine Learning segítségével lett létrehozva. Az ingyenes próbaverzió, valamint a bevezető videó kísérletek létrehozásával és [közzétételi webes szolgáltatások](machine-learning-publish-a-machine-learning-web-service.md), lásd: [azure.com/ml](http://azure.com/ml). Az alábbiakban van egy Képernyőkép a kísérlet, amely a webes szolgáltatás, és példa kód létre minden egyes belül modulok.
> 
> 

Azure Machine Learning belül egy új üres kísérlet létrehozásához. A minta bemeneti adatok feltöltése egy előre meghatározott adatkapcsolatú sémával. Az adatok kapcsolódó sémája egy [R-parancsfájl végrehajtása] [ execute-r-script] modul, amely STL és ETS előrejelzési modellek "stl", "ets" használatával hoz létre, és az r "előrejelzési" működik 

### <a name="experiment-flow"></a>Kísérlet folyamata:
![Kísérlet folyamata][2]

#### <a name="module-1"></a>1. modul:
    # Add in the CSV file with the data in the format shown below 
![Mintaadatok][3]    

#### <a name="module-2"></a>A modul 2:
    # Data input
    data <- maml.mapInputPort(1) # class: data.frame
    library(forecast)

    # Preprocessing
    colnames(data) <- c("frequency", "horizon", "dates", "values")
    dates <- strsplit(data$dates, ";")[[1]]
    values <- strsplit(data$values, ";")[[1]]

    dates <- as.Date(dates, format = '%m/%d/%Y')
    values <- as.numeric(values)

    # Fit a time series model
    train_ts<- ts(values, frequency=data$frequency)
    fit1 <- stl(train_ts,  s.window="periodic")
    train_model <- forecast(fit1, h = data$horizon, method = 'ets')
    plot(train_model)

    # Produce forcasting
    train_pred <- round(train_model$mean,2)
    data.forecast <- as.data.frame(t(train_pred))
    colnames(data.forecast) <- paste("Forecast", 1:data$horizon, sep="")

    # Data output
    maml.mapOutputPort("data.forecast");

## <a name="limitations"></a>Korlátozások
Ez egy nagyon egyszerű példa ETS + STL az előrejelzéshez. A fenti példa kódot is látható, nincs hiba alatt van megvalósítva, és a szolgáltatás feltételezi, hogy a változók értékei folyamatos/pozitív és a gyakoriság 1-nél nagyobb egész számnak kell lennie. A dátum és az érték vektorok hosszának meg kell egyeznie, és a idősorozat hosszabbnak kell lennie mint 2 * gyakoriságát. A dátum változó "hh/nn/éééé" formátumban kell felelnie.

## <a name="faq"></a>GYIK
Gyakori kérdések a felhasználás a webszolgáltatás vagy az Azure piactéren közzétételt, lásd: [Itt](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-retail-demand-forecasting/retail-img1.png
[2]: ./media/machine-learning-r-csharp-retail-demand-forecasting/retail-img2.png
[3]: ./media/machine-learning-r-csharp-retail-demand-forecasting/retail-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

