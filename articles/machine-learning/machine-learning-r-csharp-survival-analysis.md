---
title: "(elavult) Túlélési elemzése Azure Machine Learning segítségével |} Microsoft Docs"
description: "(elavult) Túlélési elemzés esemény előfordulása valószínűség"
services: machine-learning
documentationcenter: 
author: zhangya
manager: jhubbard
editor: cgronlun
ms.assetid: a142fc45-cdfb-4971-910e-05dab8bc699e
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: zhangya
ROBOTS: NOINDEX
redirect_url: https://gallery.cortanaintelligence.com/
redirect_document_id: TRUE
ms.openlocfilehash: 7d4066d5f15a39c428d8035257c4841f9b3cc775
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="deprecated-survival-analysis"></a><span data-ttu-id="41b04-103">(elavult) Túlélési elemzés</span><span class="sxs-lookup"><span data-stu-id="41b04-103">(deprecated) Survival Analysis</span></span>

> [!NOTE]
> <span data-ttu-id="41b04-104">A Microsoft DataMarket használatból van, és ez az API már elavult.</span><span class="sxs-lookup"><span data-stu-id="41b04-104">The Microsoft DataMarket is being retired and this API has been deprecated.</span></span> 
> 
> <span data-ttu-id="41b04-105">Sok hasznos példa kísérletek és API-k a [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="41b04-105">You can find many useful example experiments and APIs in the [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="41b04-106">A gyűjtemény kapcsolatos további információkért lásd: [megosztást, és felderítik a Cortana Intelligence Gallery erőforrások](machine-learning-gallery-how-to-use-contribute-publish.md).</span><span class="sxs-lookup"><span data-stu-id="41b04-106">For more information about the Gallery, see [Share and discover resources in the Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span></span>

<span data-ttu-id="41b04-107">Sok esetben a fő alatt assessment eredménye érdeklő esemény időt.</span><span class="sxs-lookup"><span data-stu-id="41b04-107">Under many scenarios, the main outcome under assessment is the time to an event of interest.</span></span> <span data-ttu-id="41b04-108">Más szóval a kérdés "Ha ez az esemény történik?"</span><span class="sxs-lookup"><span data-stu-id="41b04-108">In other words, the question “when this event will occur?”</span></span> <span data-ttu-id="41b04-109">meg kell adnia.</span><span class="sxs-lookup"><span data-stu-id="41b04-109">is asked.</span></span> <span data-ttu-id="41b04-110">Példaként, fontolja meg az olyan helyzetekben, ahol az adatokat ismerteti az eltelt idő (nap, év, távolság, stb.) csak az egyik fontos (elleni relapse, kapott doktori fok, berendezés pad hibája) esemény következik be.</span><span class="sxs-lookup"><span data-stu-id="41b04-110">As examples, consider situations where the data describes the elapsed time (days, years, mileage, etc.) until the event of interest (disease relapse, PhD degree received, brake pad failure) occurs.</span></span> <span data-ttu-id="41b04-111">Az adatok minden példánya (egy türelmet student, egy autó, stb.) egy adott objektumot jelöli.</span><span class="sxs-lookup"><span data-stu-id="41b04-111">Each instance in the data represents a specific object (a patient, a student, a car, etc.).</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<span data-ttu-id="41b04-112">Ez [webszolgáltatás](https://datamarket.azure.com/dataset/aml_labs/survivalanalysis) a "Mi az a valószínűsége annak, hogy az eseményt egyik fontos idő n objektum x történik?" kérdésre adott válaszok</span><span class="sxs-lookup"><span data-stu-id="41b04-112">This [web service](https://datamarket.azure.com/dataset/aml_labs/survivalanalysis) answers the question “what is the probability that the event of interest will occur by time n for object x?”</span></span> <span data-ttu-id="41b04-113">Egy túlélési elemzési modellt biztosít, a webszolgáltatás lehetővé teszi a felhasználók adatokat a modell betanítását és tesztelik azt.</span><span class="sxs-lookup"><span data-stu-id="41b04-113">By providing a survival analysis model, this web service enables users to supply data to train the model and test it.</span></span> <span data-ttu-id="41b04-114">A fő téma a kísérlet során eltelt idő hosszát modellezésére érdeklő az esemény akkor következik be, amíg.</span><span class="sxs-lookup"><span data-stu-id="41b04-114">The main theme of the experiment is to model the length of the elapsed time until the event of interest occurs.</span></span> 

> <span data-ttu-id="41b04-115">Ez a webszolgáltatás kell fenntartania – potenciálisan végig a mobilalkalmazások a webhelyen keresztül, vagy akár a helyi számítógépen, a felhasználókat például.</span><span class="sxs-lookup"><span data-stu-id="41b04-115">This web service could be consumed by users – potentially through a mobile app, through a website, or even on a local computer, for example.</span></span> <span data-ttu-id="41b04-116">De a webszolgáltatás célja is példa bemutatja, hogyan Azure Machine Learning webszolgáltatások fölött R-kód létrehozásához használható kiszolgálásához.</span><span class="sxs-lookup"><span data-stu-id="41b04-116">But the purpose of the web service is also to serve as an example of how Azure Machine Learning can be used to create web services on top of R code.</span></span> <span data-ttu-id="41b04-117">Az R-kód csupán néhány sornyi és az Azure Machine Learning Studio egy gombját kattint egy kísérlet hozható létre az R-kód és webszolgáltatásként közzé.</span><span class="sxs-lookup"><span data-stu-id="41b04-117">With just a few lines of R code and clicks of a button within Azure Machine Learning Studio, an experiment can be created with R code and published as a web service.</span></span> <span data-ttu-id="41b04-118">A webszolgáltatás majd közzé az Azure piactéren, és felhasználók és eszközök által felhasznált világszerte a szerző, a webszolgáltatás által infrastruktúra beállítás nélkül.</span><span class="sxs-lookup"><span data-stu-id="41b04-118">The web service can then be published to the Azure Marketplace and consumed by users and devices across the world with no infrastructure setup by the author of the web service.</span></span>  
> 
> 

## <a name="consumption-of-web-service"></a><span data-ttu-id="41b04-119">Felhasználási webszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="41b04-119">Consumption of web service</span></span>
<span data-ttu-id="41b04-120">A bemeneti adatok séma, a webszolgáltatás a következő táblázatban látható.</span><span class="sxs-lookup"><span data-stu-id="41b04-120">The input data schema of the web service is shown in the following table.</span></span> <span data-ttu-id="41b04-121">Hat adatra van szükség a bemeneti: betanítási adatok, az adatok tesztelés, idő érdeklő "idő" indexét dimenzió, "event" dimenzió és a változó típusok indexe (folyamatos vagy tényező).</span><span class="sxs-lookup"><span data-stu-id="41b04-121">Six pieces of information are needed as the input: training data, testing data, time of interest, the index of "time" dimension, the index of "event" dimension, and the variable types (continuous or factor).</span></span> <span data-ttu-id="41b04-122">A betanítási adatok képviselt karakterláncot, ahol a sorokat is vesszővel elválasztott, és az oszlopok pontosvesszővel válassza el egymástól.</span><span class="sxs-lookup"><span data-stu-id="41b04-122">The training data is represented with a string, where the rows are separated by comma, and the columns are separated by semicolon.</span></span> <span data-ttu-id="41b04-123">Az adatok szolgáltatások száma kvórummodellje rugalmas.</span><span class="sxs-lookup"><span data-stu-id="41b04-123">The number of features of the data is flexible.</span></span> <span data-ttu-id="41b04-124">A bemeneti karakterlánc szereplő összes elem számnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="41b04-124">All the elements in the input string must be numeric.</span></span> <span data-ttu-id="41b04-125">A betanítási adatok a "time" dimenzió jelez a kiindulási pont, a vizsgálat (a fogadó kábítószerrel kezelés programok, kezdési doktori tanulmány, egy autó megkezdi a vezeti stb student türelmet.) óta eltelt idő (nap, év, távolság, stb.) meghatározása csak az egyik fontos (a beteg visszaadó kábítószerrel használatra, a student megszerezni a doktori fok, a car rendelkező berendezés pad hiba stb.) az esemény akkor következik be.</span><span class="sxs-lookup"><span data-stu-id="41b04-125">In the training data, the “time” dimension indicates the number of time units (days, years, mileage, etc.) elapsed since the starting point of the study (a patient receiving drug treatment programs, a student starting PhD study, a car starting to be driven, etc.) until the event of interest (the patient returning to drug usage, the student obtaining the PhD degree, the car having brake pad failure, etc.) occurs.</span></span> <span data-ttu-id="41b04-126">Az "event" dimenzió azt jelzi, hogy egyik fontos az esemény akkor következik be, a vizsgálat végén.</span><span class="sxs-lookup"><span data-stu-id="41b04-126">The “event” dimension indicates whether the event of interest occurs at the end of the study.</span></span> <span data-ttu-id="41b04-127">Érték "esemény = 1" azt jelenti, hogy az egyik fontos az esemény akkor következik be, a "time" dimenzió; által jelzett időpontra "esemény = 0" azt jelenti, hogy az eseményt egyik fontos nem történt meg a "time" dimenzió által jelzett időn által.</span><span class="sxs-lookup"><span data-stu-id="41b04-127">A value of “event=1” means that the event of interest occurs at the time indicated by the “time” dimension; “event=0” means that the event of interest has not occurred by the time indicated by the “time” dimension.</span></span>

* <span data-ttu-id="41b04-128">trainingdata - karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="41b04-128">trainingdata - A character string.</span></span> <span data-ttu-id="41b04-129">Vesszővel elválasztott sorok és oszlopok pontosvesszővel válassza el egymástól.</span><span class="sxs-lookup"><span data-stu-id="41b04-129">Rows are separated by comma, and columns are separated by semicolon.</span></span> <span data-ttu-id="41b04-130">Minden egyes sorára "Idődimenzió", "event" dimenzió és előrejelzőjének változót tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="41b04-130">Each row includes “time” dimension, “event” dimension, and predictor variables.</span></span>
* <span data-ttu-id="41b04-131">testingdata - adatokat tartalmazó előrejelzőjének változók egy adott objektum tartalmaz egy sort.</span><span class="sxs-lookup"><span data-stu-id="41b04-131">testingdata - One row of data that contains predictor variables for a particular object.</span></span>
* <span data-ttu-id="41b04-132">time_of_interest - érdeklődési n során eltelt idő.</span><span class="sxs-lookup"><span data-stu-id="41b04-132">time_of_interest - The elapsed time of interest n.</span></span>
* <span data-ttu-id="41b04-133">index_time – a "time" dimenzió (1-től kezdődő) oszlop indexét.</span><span class="sxs-lookup"><span data-stu-id="41b04-133">index_time - The column index of the “time” dimension (starting from 1).</span></span>
* <span data-ttu-id="41b04-134">index_event - az adatoszlop indexe az "event" dimenzió (1-től kezdődő).</span><span class="sxs-lookup"><span data-stu-id="41b04-134">index_event - The column index of the “event” dimension (starting from 1).</span></span>
* <span data-ttu-id="41b04-135">variable_types - karakterlánc pontosvesszővel válassza el az azt elválasztóként.</span><span class="sxs-lookup"><span data-stu-id="41b04-135">variable_types - A character string with semicolons as separators in it.</span></span> <span data-ttu-id="41b04-136">0 folyamatos változók és a 1 tényező változók jelenti.</span><span class="sxs-lookup"><span data-stu-id="41b04-136">0 represents continuous variables and 1 represents factor variables.</span></span>

<span data-ttu-id="41b04-137">A kimenete által egy adott időpontban bekövetkező eseményhez valószínűségét.</span><span class="sxs-lookup"><span data-stu-id="41b04-137">The output is the probability of an event occurring by a specific time.</span></span> 

> <span data-ttu-id="41b04-138">Ez a szolgáltatás az Azure piactéren kihelyezett egy OData-szolgáltatás; a POST vagy GET módszerrel elnevezése lehet.</span><span class="sxs-lookup"><span data-stu-id="41b04-138">This service, as hosted on the Azure Marketplace, is an OData service; these may be called through POST or GET methods.</span></span> 
> 
> 

<span data-ttu-id="41b04-139">Többféleképpen is az automatizált módon a szolgáltatás fel (egy példa alkalmazás [Itt](http://microsoftazuremachinelearning.azurewebsites.net/SurvivalAnalysis.aspx)).</span><span class="sxs-lookup"><span data-stu-id="41b04-139">There are multiple ways of consuming the service in an automated fashion (an example app is [here](http://microsoftazuremachinelearning.azurewebsites.net/SurvivalAnalysis.aspx)).</span></span> 

### <a name="starting-c-code-for-web-service-consumption"></a><span data-ttu-id="41b04-140">C#-kódban a webes szolgáltatások felhasználásához megkezdése:</span><span class="sxs-lookup"><span data-stu-id="41b04-140">Starting C# code for web service consumption:</span></span>
    public class Input
    {
            public string trainingdata;
            public string testingdata;
            public string timeofinterest;
            public string indextime;
            public string indexevent;
            public string variabletypes;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { trainingdata = TextBox1.Text, testingdata = TextBox2.Text, timeofinterest = TextBox3.Text, indextime = TextBox4.Text, indexevent = TextBox5.Text, variabletypes = TextBox6.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();

            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));

            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }




<span data-ttu-id="41b04-141">Ez a vizsgálat értékelése a következőképpen történik.</span><span class="sxs-lookup"><span data-stu-id="41b04-141">The interpretation of this test is as follows.</span></span> <span data-ttu-id="41b04-142">Feltéve, hogy az adatok célja, hogy eltelt idő modellezhető csak a két kezelés programok kapó betegeknél kábítószerrel használati visszaállításához.</span><span class="sxs-lookup"><span data-stu-id="41b04-142">Assuming the goal of the data is to model the elapsed time until the return to drug usage for the patients who received one of the two treatment programs.</span></span> <span data-ttu-id="41b04-143">A kimenet a webes szolgáltatás olvasások: betegek 35 évnél fiatalabb alatt, hogy előző kábítószer-kezelés 2 többször, hosszú helyi kezelés program véve és heroin és a kokain felhasználású, térjen vissza a kábítószerrel használati valószínűségét 95.64 % 500 nap.</span><span class="sxs-lookup"><span data-stu-id="41b04-143">The output of the web service reads: for patients being 35 years old, having previous drug treatment 2 times, taking the long residential treatment program, and with both heroin and cocaine usage, the probability of returning to the drug usage is 95.64% by day 500.</span></span>

## <a name="creation-of-web-service"></a><span data-ttu-id="41b04-144">Webes szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="41b04-144">Creation of web service</span></span>
> <span data-ttu-id="41b04-145">Ez a webszolgáltatás Azure Machine Learning segítségével lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="41b04-145">This web service was created using Azure Machine Learning.</span></span> <span data-ttu-id="41b04-146">Az ingyenes próbaverzió, valamint a bevezető videó kísérletek létrehozásával és [közzétételi webes szolgáltatások](machine-learning-publish-a-machine-learning-web-service.md), lásd: [azure.com/ml](http://azure.com/ml).</span><span class="sxs-lookup"><span data-stu-id="41b04-146">For a free trial, as well as introductory videos on creating experiments and [publishing web services](machine-learning-publish-a-machine-learning-web-service.md), please see [azure.com/ml](http://azure.com/ml).</span></span> <span data-ttu-id="41b04-147">Az alábbiakban van egy Képernyőkép a kísérlet, amely a webes szolgáltatás, és példa kód létre minden egyes belül modulok.</span><span class="sxs-lookup"><span data-stu-id="41b04-147">Below is a screenshot of the experiment that created the web service and example code for each of the modules within the experiment.</span></span>
> 
> 

<span data-ttu-id="41b04-148">Azure Machine Learning belül egy új üres kísérlet létrehozásához és két [R-parancsfájl végrehajtása] [ execute-r-script] modulok lekért a munkaterület-kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="41b04-148">From within Azure Machine Learning, a new blank experiment was created and two [Execute R Script][execute-r-script] modules were pulled onto the workspace.</span></span> <span data-ttu-id="41b04-149">Az adatok séma, amely egy egyszerű készült [R-parancsfájl végrehajtása][execute-r-script], amely megadja, hogy a bemeneti adatok séma a webszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="41b04-149">The data schema was created with a simple [Execute R Script][execute-r-script], which defines the input data schema for the web service.</span></span> <span data-ttu-id="41b04-150">Ez a modul majd csatolva van a második [R-parancsfájl végrehajtása] [ execute-r-script] modult, amely a fő munka.</span><span class="sxs-lookup"><span data-stu-id="41b04-150">This module is then linked to the second [Execute R Script][execute-r-script] module, which does major work.</span></span> <span data-ttu-id="41b04-151">Ez a modul az adatok előfeldolgozása, a modell létrehozásának és az előrejelzés végzi.</span><span class="sxs-lookup"><span data-stu-id="41b04-151">This module does data preprocessing, model building, and predictions.</span></span> <span data-ttu-id="41b04-152">Az adatok előfeldolgozási lépésben a bemeneti adatok hosszú karakterlánc által képviselt át legyenek-e, és adatok keret alakítja át.</span><span class="sxs-lookup"><span data-stu-id="41b04-152">In the data preprocessing step, the input data represented by a long string is transformed and converted into a data frame.</span></span> <span data-ttu-id="41b04-153">A modell épület lépésben egy külső R csomagot "survival_2.37-7.zip" első telepítésekor túlélési elemzés elvégzéséhez.</span><span class="sxs-lookup"><span data-stu-id="41b04-153">In the model building step, an external R package “survival_2.37-7.zip” is first installed for conducting survival analysis.</span></span> <span data-ttu-id="41b04-154">Majd a "coxph" függvény végrehajtása után egy sorozat adatfeldolgozási feladatok.</span><span class="sxs-lookup"><span data-stu-id="41b04-154">Then the “coxph” function is executed after a series data processing tasks.</span></span> <span data-ttu-id="41b04-155">A "coxph" függvény túlélési elemzéshez részleteit az R dokumentációjában olvasható.</span><span class="sxs-lookup"><span data-stu-id="41b04-155">The details of the “coxph” function for survival analysis can be read from the R documentation.</span></span> <span data-ttu-id="41b04-156">Az előrejelzés lépésben egy tesztelési példány van megadva a "surfit" függvény a betanított modell, és a tesztelési példány túlélési görbe "görbe" változóként jön létre.</span><span class="sxs-lookup"><span data-stu-id="41b04-156">In the prediction step, a testing instance is supplied into the trained model with the “surfit” function, and the survival curve for this testing instance is produced as “curve” variable.</span></span> <span data-ttu-id="41b04-157">Végezetül fontos idő valószínűségét kapott.</span><span class="sxs-lookup"><span data-stu-id="41b04-157">Finally, the probability of the time of interest is obtained.</span></span> 

### <a name="experiment-flow"></a><span data-ttu-id="41b04-158">Kísérlet folyamata:</span><span class="sxs-lookup"><span data-stu-id="41b04-158">Experiment flow:</span></span>
![Kísérlet folyamata][1]

#### <a name="module-1"></a><span data-ttu-id="41b04-160">1. modul:</span><span class="sxs-lookup"><span data-stu-id="41b04-160">Module 1:</span></span>
    #Data schema with example data (replaced with data from web service)
    trainingdata="53;1;29;0;0;3,79;1;34;0;1;2,45;1;27;0;1;1,37;1;24;0;1;1,122;1;30;0;1;1,655;0;41;0;0;1,166;1;30;0;0;3,227;1;29;0;0;3,805;0;30;0;0;1,104;1;24;0;0;1,90;1;32;0;0;1,373;1;26;0;0;1,70;1;36;0;0;1”
    testingdata="35;2;1;1"
    time_of_interest="500"
    index_time="1"
    index_event="2"

    # 0 - continuous; 1 -  factor
    variable_types="0;0;1;1"

    sampleInput=data.frame(trainingdata,testingdata,time_of_interest,index_time,index_event,variable_types)

    maml.mapOutputPort("sampleInput"); #send data to output port

#### <a name="module-2"></a><span data-ttu-id="41b04-161">A modul 2:</span><span class="sxs-lookup"><span data-stu-id="41b04-161">Module 2:</span></span>
    #Read data from input port
    data <- maml.mapInputPort(1) 
    colnames(data) <- c("trainingdata","testingdata","time_of_interest","index_time","index_event","variable_types")

    # Preprocessing training data
    traindingdata=data$trainingdata
    y=strsplit(as.character(data$trainingdata),",")
    n_row=length(unlist(y))
    z=sapply(unlist(y), strsplit, ";", simplify = TRUE)
    mydata <- data.frame(matrix(unlist(z), nrow=n_row, byrow=T), stringsAsFactors=FALSE)
    n_col=ncol(mydata)

    # Preprocessing testing data
    testingdata=as.character(data$testingdata)
    testingdata=unlist(strsplit(testingdata,";"))

    # Preprocessing other input parameters
    time_of_interest=data$time_of_interest
    time_of_interest=as.numeric(as.character(time_of_interest))
    index_time = data$index_time
    index_event = data$index_event
    variable_types = data$variable_types

    # Necessary R packages
    install.packages("src/packages_survival/survival_2.37-7.zip",lib=".",repos=NULL,verbose=TRUE)
    library(survival)

    # Prepare to build model
    attach(mydata)

    for (i in 1:n_col){ mydata[,i]=as.numeric(mydata[,i])} 
    d_time=paste("X",index_time,sep = "")
    d_event=paste("X",index_event,sep = "")
    v_time_event <- c(d_time,d_event)
    v_predictors = names(mydata)[!(names(mydata) %in% v_time_event)]

    variable_types = unlist(strsplit(as.character(variable_types),";"))

    len = length(v_predictors)
    c="" # Construct the execution string
    for (i in 1:len){
    if(i==len){
    if(variable_types[i]!=0){ c=paste(c, "factor(",v_predictors[i],")",sep="")}
     else{ c=paste(c, v_predictors[i])}
    }else{
    if(variable_types[i]!=0){c=paste(c, "factor(",v_predictors[i],") + ",sep="")}
    else{c=paste(c, v_predictors[i],"+")}
    }
    }
    f=paste("coxph(Surv(",d_time,",",d_event,") ~")
    f=paste(f,c)
    f=paste(f,", data=mydata )")

    # Fit a Cox proportional hazards model and get the predicted survival curve for a testing instance 
    fit=eval(parse(text=f))

    testingdata = as.data.frame(matrix(testingdata, ncol=len,byrow = TRUE),stringsAsFactors=FALSE)
    names(testingdata)=v_predictors
    for (i in 1:len){ testingdata[,i]=as.numeric(testingdata[,i])}

    curve=survfit(fit,testingdata)

    # Based on user input, find the event occurrence probability
    position_closest=which.min(abs(prob_event$time - time_of_interest))

    if(prob_event[position_closest,"time"]==time_of_interest){# exact match
    output=prob_event[position_closest,"prob"]
    }else{# not exact match
    if(time_of_interest>max(prob_event$time)){
    output=prob_event[position_closest,"prob"]
    }else if(time_of_interest<min(prob_event$time)){
    output=prob_event[position_closest,"prob"]
    }else{output=(prob_event[position_closest,"prob"]+prob_event[position_closest+1,"prob"])/2}
    }

    #Pull out results to send to web service
    output=paste(round(100*output, 2), "%") 
    maml.mapOutputPort("output"); #output port




## <a name="limitations"></a><span data-ttu-id="41b04-162">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="41b04-162">Limitations</span></span>
<span data-ttu-id="41b04-163">A webszolgáltatás szolgáltatás változók (oszlop) csak numerikus értékeket vehet igénybe.</span><span class="sxs-lookup"><span data-stu-id="41b04-163">This web service can take only numerical values as feature variables (columns).</span></span> <span data-ttu-id="41b04-164">Az "event" oszlop csak 0 vagy 1 értéket vehet igénybe.</span><span class="sxs-lookup"><span data-stu-id="41b04-164">The “event” column can take only value 0 or 1.</span></span> <span data-ttu-id="41b04-165">A "idő" oszlopot kell pozitív egész számnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="41b04-165">The “time” column needs to be a positive integer.</span></span>

## <a name="faq"></a><span data-ttu-id="41b04-166">GYIK</span><span class="sxs-lookup"><span data-stu-id="41b04-166">FAQ</span></span>
<span data-ttu-id="41b04-167">Gyakori kérdések a felhasználás a webszolgáltatás vagy az Azure piactéren közzétételt, lásd: [Itt](machine-learning-marketplace-faq.md).</span><span class="sxs-lookup"><span data-stu-id="41b04-167">For frequently asked questions on consumption of the web service or publishing to the Azure Marketplace, see [here](machine-learning-marketplace-faq.md).</span></span>

[1]: ./media/machine-learning-r-csharp-survival-analysis/survive_img2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

