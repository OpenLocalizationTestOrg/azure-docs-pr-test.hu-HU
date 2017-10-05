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
# <a name="deprecated-forecasting---ets--stl"></a><span data-ttu-id="9a2a3-103">(elavult) Az előrejelzés - ETS + STL</span><span class="sxs-lookup"><span data-stu-id="9a2a3-103">(deprecated) Forecasting - ETS + STL</span></span>

> [!NOTE]
> <span data-ttu-id="9a2a3-104">A Microsoft DataMarket használatból van, és ez az API már elavult.</span><span class="sxs-lookup"><span data-stu-id="9a2a3-104">The Microsoft DataMarket is being retired and this API has been deprecated.</span></span> 
> 
> <span data-ttu-id="9a2a3-105">Sok hasznos példa kísérletek és API-k a [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="9a2a3-105">You can find many useful example experiments and APIs in the [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="9a2a3-106">A gyűjtemény kapcsolatos további információkért lásd: [megosztást, és felderítik a Cortana Intelligence Gallery erőforrások](machine-learning-gallery-how-to-use-contribute-publish.md).</span><span class="sxs-lookup"><span data-stu-id="9a2a3-106">For more information about the Gallery, see [Share and discover resources in the Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span></span>

<span data-ttu-id="9a2a3-107">Ez [webszolgáltatás](https://datamarket.azure.com/dataset/aml_labs/demand_forecast) megvalósítja határozza Trend felbontás ellen (STL) és exponenciális simítás (ETS) modellek előrejelzéseket, a felhasználó által megadott korábbi adatok alapján történő létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="9a2a3-107">This [web service](https://datamarket.azure.com/dataset/aml_labs/demand_forecast) implements Seasonal Trend Decomposition (STL) and Exponential Smoothing (ETS) models to produce predictions based on the historical data provided by the user.</span></span> <span data-ttu-id="9a2a3-108">A termék igény szerinti növeli az év?</span><span class="sxs-lookup"><span data-stu-id="9a2a3-108">Will the demand for a specific product increase this year?</span></span> <span data-ttu-id="9a2a3-109">Is szeretnék előre jelezni az értékesítés a Karácsony szezon az, hogy I hatékonyan megtervezheti a készlet?</span><span class="sxs-lookup"><span data-stu-id="9a2a3-109">Can I predict my product sales for the Christmas season, so that I can effectively plan my inventory?</span></span> <span data-ttu-id="9a2a3-110">Előrejelzési modellek alkalmasak ilyen kérdések megoldására.</span><span class="sxs-lookup"><span data-stu-id="9a2a3-110">Forecasting models are apt to address such questions.</span></span> <span data-ttu-id="9a2a3-111">A régebbi adatokat adott meg, ezek a modellek vizsgálja meg, rejtett trendek és a szezonalitás értékének előre jelezni jövőbeli trendeket.</span><span class="sxs-lookup"><span data-stu-id="9a2a3-111">Given the past data, these models examine hidden trends and seasonality to predict future trends.</span></span> 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

> <span data-ttu-id="9a2a3-112">Ez a webszolgáltatás kell fenntartania – potenciálisan végig a mobilalkalmazások a webhelyen keresztül, vagy akár a helyi számítógépen, a felhasználókat például.</span><span class="sxs-lookup"><span data-stu-id="9a2a3-112">This web service could be consumed by users – potentially through a mobile app, through a website, or even on a local computer, for example.</span></span> <span data-ttu-id="9a2a3-113">De a webszolgáltatás célja is példa bemutatja, hogyan Azure Machine Learning webszolgáltatások fölött R-kód létrehozásához használható kiszolgálásához.</span><span class="sxs-lookup"><span data-stu-id="9a2a3-113">But the purpose of the web service is also to serve as an example of how Azure Machine Learning can be used to create web services on top of R code.</span></span> <span data-ttu-id="9a2a3-114">Az R-kód csupán néhány sornyi és az Azure Machine Learning Studio egy gombját kattint egy kísérlet hozható létre az R-kód és webszolgáltatásként közzé.</span><span class="sxs-lookup"><span data-stu-id="9a2a3-114">With just a few lines of R code and clicks of a button within Azure Machine Learning Studio, an experiment can be created with R code and published as a web service.</span></span> <span data-ttu-id="9a2a3-115">A webszolgáltatás majd közzé az Azure piactéren, és felhasználók és eszközök által felhasznált világszerte a szerző, a webszolgáltatás által infrastruktúra beállítás nélkül.</span><span class="sxs-lookup"><span data-stu-id="9a2a3-115">The web service can then be published to the Azure Marketplace and consumed by users and devices across the world with no infrastructure setup by the author of the web service.</span></span>  
> 
> 

## <a name="consumption-of-web-service"></a><span data-ttu-id="9a2a3-116">Felhasználási webszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="9a2a3-116">Consumption of web service</span></span>
<span data-ttu-id="9a2a3-117">Ez a szolgáltatás 4 argumentumként fogadja, és kiszámítja az előrejelzések.</span><span class="sxs-lookup"><span data-stu-id="9a2a3-117">This service accepts 4 arguments and calculates the forecasts.</span></span>
<span data-ttu-id="9a2a3-118">A bemeneti argumentumok a következők:</span><span class="sxs-lookup"><span data-stu-id="9a2a3-118">The input arguments are:</span></span>

* <span data-ttu-id="9a2a3-119">Gyakoriság - azt jelzi, hogy a nyers adatokat (napi vagy heti vagy havi/negyedéves/évente) gyakoriságát.</span><span class="sxs-lookup"><span data-stu-id="9a2a3-119">Frequency - Indicates the frequency of the raw data (daily/weekly/monthly/quarterly/yearly).</span></span>
* <span data-ttu-id="9a2a3-120">Horizon - jövőbeli előrejelzési időkereten belül.</span><span class="sxs-lookup"><span data-stu-id="9a2a3-120">Horizon - Future forecast time-frame.</span></span>
* <span data-ttu-id="9a2a3-121">Dátum - idő adja hozzá az új idő adatsorozat adatainak.</span><span class="sxs-lookup"><span data-stu-id="9a2a3-121">Date - Add in the new time series data for time.</span></span>
* <span data-ttu-id="9a2a3-122">Érték – adja hozzá az új idő adatok sorozatértékek.</span><span class="sxs-lookup"><span data-stu-id="9a2a3-122">Value - Add in the new time series data values.</span></span>

<span data-ttu-id="9a2a3-123">A szolgáltatás a számított előre jelzett értékek eredménye.</span><span class="sxs-lookup"><span data-stu-id="9a2a3-123">The output of the service is the calculated forecast values.</span></span>

<span data-ttu-id="9a2a3-124">A minta bemeneti lehet:</span><span class="sxs-lookup"><span data-stu-id="9a2a3-124">Sample input could be:</span></span> 

* <span data-ttu-id="9a2a3-125">Gyakorisága – 12</span><span class="sxs-lookup"><span data-stu-id="9a2a3-125">Frequency - 12</span></span>
* <span data-ttu-id="9a2a3-126">Horizon - 12</span><span class="sxs-lookup"><span data-stu-id="9a2a3-126">Horizon - 12</span></span>
* <span data-ttu-id="9a2a3-127">Dátum - 1/15/2012; 2/15/2012; 3 15/2012; 4/15/2012; 2012-5-15; 6/15/2012; 15/7/2012; 8 / 15/2012; a 9/15/2012; a 10/15/2012; a 11/15/2012; a 12/15/2012; 1/15/2013; 2/15/2013; 3/15/2013; 4/15/2013; 5 vagy 15/2013; 6/15/2013; 7/15/2013; 8 / 15/2013; 9/15/2013; 10/15/2013; 11/15/2013; 12/15/2013; 1/15/2014; 2/15/2014; 3/15/2014; 4/15/2014; 5 vagy 15/2014; 6/15/2014; 7/15/2014; 8 / 15/2014; 9/15/2014</span><span class="sxs-lookup"><span data-stu-id="9a2a3-127">Date - 1/15/2012;2/15/2012;3/15/2012;4/15/2012;5/15/2012;6/15/2012;7/15/2012;8/15/2012;9/15/2012;10/15/2012;11/15/2012;12/15/2012; 1/15/2013;2/15/2013;3/15/2013;4/15/2013;5/15/2013;6/15/2013;7/15/2013;8/15/2013;9/15/2013;10/15/2013;11/15/2013;12/15/2013; 1/15/2014;2/15/2014;3/15/2014;4/15/2014;5/15/2014;6/15/2014;7/15/2014;8/15/2014;9/15/2014</span></span>
* <span data-ttu-id="9a2a3-128">Érték - 3.479; 3.68; 3.832; 3.941; 3.797; 3.586; 3.508; 3.731; 3.915; 3.844; 3.634; 3.549; 3.557; 3.785; 3.782; 3.601; 3.544; 3.556; 3.65; 3.709; 3.682; 3.511; 3.429; 3.51; 3.523; 3.525; 3.626; 3.695; 3.711; 3.711; 3.693; 3.571; 3.509</span><span class="sxs-lookup"><span data-stu-id="9a2a3-128">Value - 3.479;3.68;3.832;3.941;3.797;3.586;3.508;3.731;3.915;3.844;3.634;3.549;3.557;3.785;3.782;3.601;3.544;3.556;3.65;3.709;3.682;3.511; 3.429;3.51;3.523;3.525;3.626;3.695;3.711;3.711;3.693;3.571;3.509</span></span>

> <span data-ttu-id="9a2a3-129">Ez a szolgáltatás az Azure piactéren kihelyezett egy OData-szolgáltatás; a POST vagy GET módszerrel elnevezése lehet.</span><span class="sxs-lookup"><span data-stu-id="9a2a3-129">This service, as hosted on the Azure Marketplace, is an OData service; these may be called through POST or GET methods.</span></span> 
> 
> 

<span data-ttu-id="9a2a3-130">Többféleképpen is az automatizált módon a szolgáltatás fel (egy példa alkalmazás [Itt](http://microsoftazuremachinelearning.azurewebsites.net/StlEtsForecasting.aspx)).</span><span class="sxs-lookup"><span data-stu-id="9a2a3-130">There are multiple ways of consuming the service in an automated fashion (an example app is [here](http://microsoftazuremachinelearning.azurewebsites.net/StlEtsForecasting.aspx)).</span></span>

### <a name="starting-c-code-for-web-service-consumption"></a><span data-ttu-id="9a2a3-131">C#-kódban a webes szolgáltatások felhasználásához megkezdése:</span><span class="sxs-lookup"><span data-stu-id="9a2a3-131">Starting C# code for web service consumption:</span></span>
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


## <a name="creation-of-web-service"></a><span data-ttu-id="9a2a3-132">Webes szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="9a2a3-132">Creation of web service</span></span>
> <span data-ttu-id="9a2a3-133">Ez a webszolgáltatás Azure Machine Learning segítségével lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="9a2a3-133">This web service was created using Azure Machine Learning.</span></span> <span data-ttu-id="9a2a3-134">Az ingyenes próbaverzió, valamint a bevezető videó kísérletek létrehozásával és [közzétételi webes szolgáltatások](machine-learning-publish-a-machine-learning-web-service.md), lásd: [azure.com/ml](http://azure.com/ml).</span><span class="sxs-lookup"><span data-stu-id="9a2a3-134">For a free trial, as well as introductory videos on creating experiments and [publishing web services](machine-learning-publish-a-machine-learning-web-service.md), please see [azure.com/ml](http://azure.com/ml).</span></span> <span data-ttu-id="9a2a3-135">Az alábbiakban van egy Képernyőkép a kísérlet, amely a webes szolgáltatás, és példa kód létre minden egyes belül modulok.</span><span class="sxs-lookup"><span data-stu-id="9a2a3-135">Below is a screenshot of the experiment that created the web service and example code for each of the modules within the experiment.</span></span>
> 
> 

<span data-ttu-id="9a2a3-136">Azure Machine Learning belül egy új üres kísérlet létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="9a2a3-136">From within Azure Machine Learning, a new blank experiment was created.</span></span> <span data-ttu-id="9a2a3-137">A minta bemeneti adatok feltöltése egy előre meghatározott adatkapcsolatú sémával.</span><span class="sxs-lookup"><span data-stu-id="9a2a3-137">Sample input data was uploaded with a predefined data schema.</span></span> <span data-ttu-id="9a2a3-138">Az adatok kapcsolódó sémája egy [R-parancsfájl végrehajtása] [ execute-r-script] modul, amely STL és ETS előrejelzési modellek "stl", "ets" használatával hoz létre, és az r "előrejelzési" működik</span><span class="sxs-lookup"><span data-stu-id="9a2a3-138">Linked to the data schema is an [Execute R Script][execute-r-script] module, which generates STL and ETS forecasting models by using ‘stl’, ‘ets’, and ‘forecast’ functions from R.</span></span> 

### <a name="experiment-flow"></a><span data-ttu-id="9a2a3-139">Kísérlet folyamata:</span><span class="sxs-lookup"><span data-stu-id="9a2a3-139">Experiment flow:</span></span>
![Kísérlet folyamata][2]

#### <a name="module-1"></a><span data-ttu-id="9a2a3-141">1. modul:</span><span class="sxs-lookup"><span data-stu-id="9a2a3-141">Module 1:</span></span>
    # Add in the CSV file with the data in the format shown below 
![Mintaadatok][3]    

#### <a name="module-2"></a><span data-ttu-id="9a2a3-143">A modul 2:</span><span class="sxs-lookup"><span data-stu-id="9a2a3-143">Module 2:</span></span>
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

## <a name="limitations"></a><span data-ttu-id="9a2a3-144">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="9a2a3-144">Limitations</span></span>
<span data-ttu-id="9a2a3-145">Ez egy nagyon egyszerű példa ETS + STL az előrejelzéshez.</span><span class="sxs-lookup"><span data-stu-id="9a2a3-145">This is a very simple example for ETS + STL forecasting.</span></span> <span data-ttu-id="9a2a3-146">A fenti példa kódot is látható, nincs hiba alatt van megvalósítva, és a szolgáltatás feltételezi, hogy a változók értékei folyamatos/pozitív és a gyakoriság 1-nél nagyobb egész számnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="9a2a3-146">As can be seen from the example code above, no error catching is implemented, and the service assumes that all the variables are continuous/positive values and the frequency should be an integer greater than 1.</span></span> <span data-ttu-id="9a2a3-147">A dátum és az érték vektorok hosszának meg kell egyeznie, és a idősorozat hosszabbnak kell lennie mint 2 * gyakoriságát.</span><span class="sxs-lookup"><span data-stu-id="9a2a3-147">The length of the date and value vectors should be the same, and the length of the time series should be greater than 2*frequency.</span></span> <span data-ttu-id="9a2a3-148">A dátum változó "hh/nn/éééé" formátumban kell felelnie.</span><span class="sxs-lookup"><span data-stu-id="9a2a3-148">The date variable should adhere to the format ‘mm/dd/yyyy’.</span></span>

## <a name="faq"></a><span data-ttu-id="9a2a3-149">GYIK</span><span class="sxs-lookup"><span data-stu-id="9a2a3-149">FAQ</span></span>
<span data-ttu-id="9a2a3-150">Gyakori kérdések a felhasználás a webszolgáltatás vagy az Azure piactéren közzétételt, lásd: [Itt](machine-learning-marketplace-faq.md).</span><span class="sxs-lookup"><span data-stu-id="9a2a3-150">For frequently asked questions on consumption of the web service or publishing to the Azure Marketplace, see [here](machine-learning-marketplace-faq.md).</span></span>

[1]: ./media/machine-learning-r-csharp-retail-demand-forecasting/retail-img1.png
[2]: ./media/machine-learning-r-csharp-retail-demand-forecasting/retail-img2.png
[3]: ./media/machine-learning-r-csharp-retail-demand-forecasting/retail-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

