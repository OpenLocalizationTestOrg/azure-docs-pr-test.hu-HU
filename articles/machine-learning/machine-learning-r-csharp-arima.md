---
title: "(elavult) Az előrejelzés: Autoregressive integrált áthelyezése átlagos (ARIMA) - Azure |} Microsoft Docs"
description: "(elavult) Az előrejelzés - Autoregressive integrált áthelyezése átlagos (ARIMA)"
services: machine-learning
documentationcenter: 
author: yijichen
manager: jhubbard
editor: cgronlun
ms.assetid: 1e0d525f-8a9e-4b42-87e0-c9423f059f8c
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: yijichen
ROBOTS: NOINDEX
redirect_url: https://gallery.cortanaintelligence.com/
redirect_document_id: True
ms.openlocfilehash: 4f3af41fd8873fdf75c6426fd395351cb41db190
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deprecated-forecasting---autoregressive-integrated-moving-average-arima"></a><span data-ttu-id="80961-103">(elavult) Az előrejelzés - Autoregressive integrált áthelyezése átlagos (ARIMA)</span><span class="sxs-lookup"><span data-stu-id="80961-103">(deprecated) Forecasting - Autoregressive Integrated Moving Average (ARIMA)</span></span>

> [!NOTE]
> <span data-ttu-id="80961-104">a Microsoft DataMarket hello használatból van, és ez az API már elavult.</span><span class="sxs-lookup"><span data-stu-id="80961-104">hello Microsoft DataMarket is being retired and this API has been deprecated.</span></span> 
> 
> <span data-ttu-id="80961-105">Sok hasznos példa kísérletek és API-kat az található hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="80961-105">You can find many useful example experiments and APIs in hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="80961-106">Gyűjteményelem hello kapcsolatos további információkért lásd: [megosztás és a Cortana Intelligence Gallery hello erőforrások felderítéséhez](machine-learning-gallery-how-to-use-contribute-publish.md).</span><span class="sxs-lookup"><span data-stu-id="80961-106">For more information about hello Gallery, see [Share and discover resources in hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span></span>


<span data-ttu-id="80961-107">Ez [szolgáltatás](https://datamarket.azure.com/dataset/aml_labs/arima) megvalósítja az Autoregressive integrált áthelyezése átlagos (ARIMA) tooproduce előrejelzéseket hello hello felhasználó által megadott korábbi adatok alapján.</span><span class="sxs-lookup"><span data-stu-id="80961-107">This [service](https://datamarket.azure.com/dataset/aml_labs/arima) implements Autoregressive Integrated Moving Average (ARIMA) tooproduce predictions based on hello historical data provided by hello user.</span></span> <span data-ttu-id="80961-108">Az év adott termék növekedést igény szerint lesz hello?</span><span class="sxs-lookup"><span data-stu-id="80961-108">Will hello demand for a specific product increase this year?</span></span> <span data-ttu-id="80961-109">Is szeretnék előre jelezni a hello az értékesítés Karácsony szezon, így I hatékonyan megtervezheti a készlet?</span><span class="sxs-lookup"><span data-stu-id="80961-109">Can I predict my product sales for hello Christmas season, so that I can effectively plan my inventory?</span></span> <span data-ttu-id="80961-110">Előrejelzési modellek apt tooaddress vannak ilyen kérdéseket.</span><span class="sxs-lookup"><span data-stu-id="80961-110">Forecasting models are apt tooaddress such questions.</span></span> <span data-ttu-id="80961-111">Hello megadott múltbeli adatok, ezek a modellek vizsgálja meg, rejtett trendeket és a szezonalitás értékének toopredict jövőbeli trendeket.</span><span class="sxs-lookup"><span data-stu-id="80961-111">Given hello past data, these models examine hidden trends and seasonality toopredict future trends.</span></span> 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

> <span data-ttu-id="80961-112">Ez a webszolgáltatás kell fenntartania – potenciálisan végig a mobilalkalmazások a webhelyen keresztül, vagy akár a helyi számítógépen, a felhasználókat például.</span><span class="sxs-lookup"><span data-stu-id="80961-112">This web service could be consumed by users – potentially through a mobile app, through a website, or even on a local computer, for example.</span></span> <span data-ttu-id="80961-113">De hello hello webszolgáltatás célja is tooserve példa bemutatja, hogyan Azure Machine Learning webszolgáltatások használt toocreate fölött R-kód is lehet.</span><span class="sxs-lookup"><span data-stu-id="80961-113">But hello purpose of hello web service is also tooserve as an example of how Azure Machine Learning can be used toocreate web services on top of R code.</span></span> <span data-ttu-id="80961-114">Az R-kód csupán néhány sornyi és az Azure Machine Learning Studio egy gombját kattint egy kísérlet hozható létre az R-kód és webszolgáltatásként közzé.</span><span class="sxs-lookup"><span data-stu-id="80961-114">With just a few lines of R code and clicks of a button within Azure Machine Learning Studio, an experiment can be created with R code and published as a web service.</span></span> <span data-ttu-id="80961-115">hello webszolgáltatás majd lehet közzétett toohello Azure piactéren, és a felhasználók és eszközök által felhasznált között hello world hello Szerző hello webszolgáltatás infrastruktúra beállítás nélkül.</span><span class="sxs-lookup"><span data-stu-id="80961-115">hello web service can then be published toohello Azure Marketplace and consumed by users and devices across hello world with no infrastructure setup by hello author of hello web service.</span></span>
> 
> 

## <a name="consumption-of-web-service"></a><span data-ttu-id="80961-116">Felhasználási webszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="80961-116">Consumption of web service</span></span>
<span data-ttu-id="80961-117">Ez a szolgáltatás 4 argumentumként fogadja, és hello elemhez tartozó ARIMA előrejelzések számítja ki.</span><span class="sxs-lookup"><span data-stu-id="80961-117">This service accepts 4 arguments and calculates hello ARIMA forecasts.</span></span>
<span data-ttu-id="80961-118">hello bemeneti argumentumai a következők:</span><span class="sxs-lookup"><span data-stu-id="80961-118">hello input arguments are:</span></span>

* <span data-ttu-id="80961-119">Gyakoriság - hello nyers adatok (napi vagy heti vagy havi/negyedéves/évente) hello gyakoriságát jelzi.</span><span class="sxs-lookup"><span data-stu-id="80961-119">Frequency - Indicates hello frequency of hello raw data (daily/weekly/monthly/quarterly/yearly).</span></span>
* <span data-ttu-id="80961-120">Horizon - jövőbeli előrejelzési időkereten belül.</span><span class="sxs-lookup"><span data-stu-id="80961-120">Horizon - Future forecast time-frame.</span></span>
* <span data-ttu-id="80961-121">Dátum - adatok hozzáadása a hello új idő sorozat ideje.</span><span class="sxs-lookup"><span data-stu-id="80961-121">Date - Add in hello new time series data for time.</span></span>
* <span data-ttu-id="80961-122">Érték – hello új idő adatok sorozatértékek adja hozzá.</span><span class="sxs-lookup"><span data-stu-id="80961-122">Value - Add in hello new time series data values.</span></span>

<span data-ttu-id="80961-123">hello hello szolgáltatás eredménye hello előre jelzett értékek kiszámítása.</span><span class="sxs-lookup"><span data-stu-id="80961-123">hello output of hello service is hello calculated forecast values.</span></span> 

<span data-ttu-id="80961-124">A minta bemeneti lehet:</span><span class="sxs-lookup"><span data-stu-id="80961-124">Sample input could be:</span></span> 

* <span data-ttu-id="80961-125">Gyakorisága – 12</span><span class="sxs-lookup"><span data-stu-id="80961-125">Frequency - 12</span></span>
* <span data-ttu-id="80961-126">Horizon - 12</span><span class="sxs-lookup"><span data-stu-id="80961-126">Horizon - 12</span></span>
* <span data-ttu-id="80961-127">Dátum - 1/15/2012; 2/15/2012; 3 15/2012; 4/15/2012; 2012-5-15; 6/15/2012; 15/7/2012; 8 / 15/2012; a 9/15/2012; a 10/15/2012; a 11/15/2012; a 12/15/2012; 1/15/2013; 2/15/2013; 3/15/2013; 4/15/2013; 5 vagy 15/2013; 6/15/2013; 7/15/2013; 8 / 15/2013; 9/15/2013; 10/15/2013; 11/15/2013; 12/15/2013; 1/15/2014; 2/15/2014; 3/15/2014; 4/15/2014; 5 vagy 15/2014; 6/15/2014; 7/15/2014; 8 / 15/2014; 9/15/2014</span><span class="sxs-lookup"><span data-stu-id="80961-127">Date - 1/15/2012;2/15/2012;3/15/2012;4/15/2012;5/15/2012;6/15/2012;7/15/2012;8/15/2012;9/15/2012;10/15/2012;11/15/2012;12/15/2012; 1/15/2013;2/15/2013;3/15/2013;4/15/2013;5/15/2013;6/15/2013;7/15/2013;8/15/2013;9/15/2013;10/15/2013;11/15/2013;12/15/2013; 1/15/2014;2/15/2014;3/15/2014;4/15/2014;5/15/2014;6/15/2014;7/15/2014;8/15/2014;9/15/2014</span></span>
* <span data-ttu-id="80961-128">Érték - 3.479; 3.68; 3.832; 3.941; 3.797; 3.586; 3.508; 3.731; 3.915; 3.844; 3.634; 3.549; 3.557; 3.785; 3.782; 3.601; 3.544; 3.556; 3.65; 3.709; 3.682; 3.511; 3.429; 3.51; 3.523; 3.525; 3.626; 3.695; 3.711; 3.711; 3.693; 3.571; 3.509</span><span class="sxs-lookup"><span data-stu-id="80961-128">Value - 3.479;3.68;3.832;3.941;3.797;3.586;3.508;3.731;3.915;3.844;3.634;3.549;3.557;3.785;3.782;3.601;3.544;3.556;3.65;3.709;3.682;3.511; 3.429;3.51;3.523;3.525;3.626;3.695;3.711;3.711;3.693;3.571;3.509</span></span>

> <span data-ttu-id="80961-129">Ez a szolgáltatás az Azure piactér hello kihelyezett egy OData-szolgáltatás; a POST vagy GET módszerrel elnevezése lehet.</span><span class="sxs-lookup"><span data-stu-id="80961-129">This service, as hosted on hello Azure Marketplace, is an OData service; these may be called through POST or GET methods.</span></span> 
> 
> 

<span data-ttu-id="80961-130">Többféleképpen is az automatizált módon hello szolgáltatás fel (egy példa alkalmazás [Itt](http://microsoftazuremachinelearning.azurewebsites.net/ArimaForecasting.aspx)).</span><span class="sxs-lookup"><span data-stu-id="80961-130">There are multiple ways of consuming hello service in an automated fashion (an example app is [here](http://microsoftazuremachinelearning.azurewebsites.net/ArimaForecasting.aspx)).</span></span>

### <a name="starting-c-code-for-web-service-consumption"></a><span data-ttu-id="80961-131">C#-kódban a webes szolgáltatások felhasználásához megkezdése:</span><span class="sxs-lookup"><span data-stu-id="80961-131">Starting C# code for web service consumption:</span></span>
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
          var input = new Input() { frequency = TextBox1.Text, horizon = TextBox2.Text, date = TextBox3.Text, value = TextBox4.Text };
        var json = JsonConvert.SerializeObject(input);
        var acitionUri =  "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";

          var httpClient = new HttpClient();
           httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere","ChangeToAPIKey");
           httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
          var query = httpClient.PostAsync(acitionUri,new StringContent(json));
          var result = query.Result.Content;
          var scoreResult = result.ReadAsStringAsync().Result;
      }

## <a name="creation-of-web-service"></a><span data-ttu-id="80961-132">Webes szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="80961-132">Creation of web service</span></span>
> <span data-ttu-id="80961-133">Ez a webszolgáltatás Azure Machine Learning segítségével lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="80961-133">This web service was created using Azure Machine Learning.</span></span> <span data-ttu-id="80961-134">Az ingyenes próbaverzió, valamint a bevezető videó kísérletek létrehozásával és [közzétételi webes szolgáltatások](machine-learning-publish-a-machine-learning-web-service.md), lásd: [azure.com/ml](http://azure.com/ml).</span><span class="sxs-lookup"><span data-stu-id="80961-134">For a free trial, as well as introductory videos on creating experiments and [publishing web services](machine-learning-publish-a-machine-learning-web-service.md), please see [azure.com/ml](http://azure.com/ml).</span></span> <span data-ttu-id="80961-135">Az alábbiakban van egy képernyőfelvétel a hello webes szolgáltatás, és példa kódot az egyes hello modulok hello kísérlet belül létrehozott hello kísérlet.</span><span class="sxs-lookup"><span data-stu-id="80961-135">Below is a screenshot of hello experiment that created hello web service and example code for each of hello modules within hello experiment.</span></span>
> 
> 

<span data-ttu-id="80961-136">Azure Machine Learning belül egy új üres kísérlet létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="80961-136">From within Azure Machine Learning, a new blank experiment was created.</span></span> <span data-ttu-id="80961-137">A minta bemeneti adatok feltöltése egy előre meghatározott adatkapcsolatú sémával.</span><span class="sxs-lookup"><span data-stu-id="80961-137">Sample input data was uploaded with a predefined data schema.</span></span> <span data-ttu-id="80961-138">Csatolt toohello adatok sémája egy [R-parancsfájl végrehajtása] [ execute-r-script] modult, amely hello elemhez tartozó ARIMA előrejelzési modell állít elő, az r "auto.arima" és "előrejelzés" függvény használatával</span><span class="sxs-lookup"><span data-stu-id="80961-138">Linked toohello data schema is an [Execute R Script][execute-r-script] module, which generates hello ARIMA forecasting model by using ‘auto.arima’ and ‘forecast’ functions from R.</span></span> 

### <a name="experiment-flow"></a><span data-ttu-id="80961-139">Kísérlet folyamata:</span><span class="sxs-lookup"><span data-stu-id="80961-139">Experiment flow:</span></span>
![Munkaterület létrehozása][2]

#### <a name="module-1"></a><span data-ttu-id="80961-141">1. modul:</span><span class="sxs-lookup"><span data-stu-id="80961-141">Module 1:</span></span>
    # Add in hello CSV file with hello data in hello format shown below 
![Munkaterület létrehozása][3]    

#### <a name="module-2"></a><span data-ttu-id="80961-143">A modul 2:</span><span class="sxs-lookup"><span data-stu-id="80961-143">Module 2:</span></span>
    # data input
    data <- maml.mapInputPort(1) # class: data.frame
    library(forecast)

    # preprocessing
    colnames(data) <- c("frequency", "horizon", "dates", "values")
    dates <- strsplit(data$dates, ";")[[1]]
    values <- strsplit(data$values, ";")[[1]]

    dates <- as.Date(dates, format = '%m/%d/%Y')
    values <- as.numeric(values)

    # fit a time-series model
    train_ts<- ts(values, frequency=data$frequency)
    fit1 <- auto.arima(train_ts)
    train_model <- forecast(fit1, h = data$horizon)
    plot(train_model)

    # produce forecasting
    train_pred <- round(train_model$mean,2)
    data.forecast <- as.data.frame(t(train_pred))
    colnames(data.forecast) <- paste("Forecast", 1:data$horizon, sep="")

    # data output
    maml.mapOutputPort("data.forecast");


## <a name="limitations"></a><span data-ttu-id="80961-144">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="80961-144">Limitations</span></span>
<span data-ttu-id="80961-145">Ez az elemhez tartozó ARIMA előrejelzési egy nagyon egyszerű példa.</span><span class="sxs-lookup"><span data-stu-id="80961-145">This is a very simple example for ARIMA forecasting.</span></span> <span data-ttu-id="80961-146">A fenti példa kódot hello is látható, nincs hiba alatt van megvalósítva, és hello szolgáltatás feltételezi, hogy minden hello változók értékei folyamatos/pozitív és hello gyakoriság 1-nél nagyobb egész számnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="80961-146">As can be seen from hello example code above, no error catching is implemented, and hello service assumes that all hello variables are continuous/positive values and hello frequency should be an integer greater than 1.</span></span> <span data-ttu-id="80961-147">hello dátum és az érték vektorok hosszának hello azonos kell hello.</span><span class="sxs-lookup"><span data-stu-id="80961-147">hello length of hello date and value vectors should be hello same.</span></span> <span data-ttu-id="80961-148">hello dátum változó toohello formátum "hh/nn/éééé" kell igazodnia.</span><span class="sxs-lookup"><span data-stu-id="80961-148">hello date variable should adhere toohello format ‘mm/dd/yyyy’.</span></span>

## <a name="faq"></a><span data-ttu-id="80961-149">GYIK</span><span class="sxs-lookup"><span data-stu-id="80961-149">FAQ</span></span>
<span data-ttu-id="80961-150">Gyakori kérdések a felhasználási webszolgáltatás hello vagy közzétételi toomarketplace, lásd: [Itt](machine-learning-marketplace-faq.md).</span><span class="sxs-lookup"><span data-stu-id="80961-150">For frequently asked questions on consumption of hello web service or publishing toomarketplace, see [here](machine-learning-marketplace-faq.md).</span></span>

[1]: ./media/machine-learning-r-csharp-arima/arima-img1.png
[2]: ./media/machine-learning-r-csharp-arima/arima-img2.png
[3]: ./media/machine-learning-r-csharp-arima/arima-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

