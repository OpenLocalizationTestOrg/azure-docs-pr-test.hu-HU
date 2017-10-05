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
# <a name="deprecated-survival-analysis"></a>(elavult) Túlélési elemzés

> [!NOTE]
> A Microsoft DataMarket használatból van, és ez az API már elavult. 
> 
> Sok hasznos példa kísérletek és API-k a [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com). A gyűjtemény kapcsolatos további információkért lásd: [megosztást, és felderítik a Cortana Intelligence Gallery erőforrások](machine-learning-gallery-how-to-use-contribute-publish.md).

Sok esetben a fő alatt assessment eredménye érdeklő esemény időt. Más szóval a kérdés "Ha ez az esemény történik?" meg kell adnia. Példaként, fontolja meg az olyan helyzetekben, ahol az adatokat ismerteti az eltelt idő (nap, év, távolság, stb.) csak az egyik fontos (elleni relapse, kapott doktori fok, berendezés pad hibája) esemény következik be. Az adatok minden példánya (egy türelmet student, egy autó, stb.) egy adott objektumot jelöli.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Ez [webszolgáltatás](https://datamarket.azure.com/dataset/aml_labs/survivalanalysis) a "Mi az a valószínűsége annak, hogy az eseményt egyik fontos idő n objektum x történik?" kérdésre adott válaszok Egy túlélési elemzési modellt biztosít, a webszolgáltatás lehetővé teszi a felhasználók adatokat a modell betanítását és tesztelik azt. A fő téma a kísérlet során eltelt idő hosszát modellezésére érdeklő az esemény akkor következik be, amíg. 

> Ez a webszolgáltatás kell fenntartania – potenciálisan végig a mobilalkalmazások a webhelyen keresztül, vagy akár a helyi számítógépen, a felhasználókat például. De a webszolgáltatás célja is példa bemutatja, hogyan Azure Machine Learning webszolgáltatások fölött R-kód létrehozásához használható kiszolgálásához. Az R-kód csupán néhány sornyi és az Azure Machine Learning Studio egy gombját kattint egy kísérlet hozható létre az R-kód és webszolgáltatásként közzé. A webszolgáltatás majd közzé az Azure piactéren, és felhasználók és eszközök által felhasznált világszerte a szerző, a webszolgáltatás által infrastruktúra beállítás nélkül.  
> 
> 

## <a name="consumption-of-web-service"></a>Felhasználási webszolgáltatás
A bemeneti adatok séma, a webszolgáltatás a következő táblázatban látható. Hat adatra van szükség a bemeneti: betanítási adatok, az adatok tesztelés, idő érdeklő "idő" indexét dimenzió, "event" dimenzió és a változó típusok indexe (folyamatos vagy tényező). A betanítási adatok képviselt karakterláncot, ahol a sorokat is vesszővel elválasztott, és az oszlopok pontosvesszővel válassza el egymástól. Az adatok szolgáltatások száma kvórummodellje rugalmas. A bemeneti karakterlánc szereplő összes elem számnak kell lennie. A betanítási adatok a "time" dimenzió jelez a kiindulási pont, a vizsgálat (a fogadó kábítószerrel kezelés programok, kezdési doktori tanulmány, egy autó megkezdi a vezeti stb student türelmet.) óta eltelt idő (nap, év, távolság, stb.) meghatározása csak az egyik fontos (a beteg visszaadó kábítószerrel használatra, a student megszerezni a doktori fok, a car rendelkező berendezés pad hiba stb.) az esemény akkor következik be. Az "event" dimenzió azt jelzi, hogy egyik fontos az esemény akkor következik be, a vizsgálat végén. Érték "esemény = 1" azt jelenti, hogy az egyik fontos az esemény akkor következik be, a "time" dimenzió; által jelzett időpontra "esemény = 0" azt jelenti, hogy az eseményt egyik fontos nem történt meg a "time" dimenzió által jelzett időn által.

* trainingdata - karakterlánc. Vesszővel elválasztott sorok és oszlopok pontosvesszővel válassza el egymástól. Minden egyes sorára "Idődimenzió", "event" dimenzió és előrejelzőjének változót tartalmaz.
* testingdata - adatokat tartalmazó előrejelzőjének változók egy adott objektum tartalmaz egy sort.
* time_of_interest - érdeklődési n során eltelt idő.
* index_time – a "time" dimenzió (1-től kezdődő) oszlop indexét.
* index_event - az adatoszlop indexe az "event" dimenzió (1-től kezdődő).
* variable_types - karakterlánc pontosvesszővel válassza el az azt elválasztóként. 0 folyamatos változók és a 1 tényező változók jelenti.

A kimenete által egy adott időpontban bekövetkező eseményhez valószínűségét. 

> Ez a szolgáltatás az Azure piactéren kihelyezett egy OData-szolgáltatás; a POST vagy GET módszerrel elnevezése lehet. 
> 
> 

Többféleképpen is az automatizált módon a szolgáltatás fel (egy példa alkalmazás [Itt](http://microsoftazuremachinelearning.azurewebsites.net/SurvivalAnalysis.aspx)). 

### <a name="starting-c-code-for-web-service-consumption"></a>C#-kódban a webes szolgáltatások felhasználásához megkezdése:
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




Ez a vizsgálat értékelése a következőképpen történik. Feltéve, hogy az adatok célja, hogy eltelt idő modellezhető csak a két kezelés programok kapó betegeknél kábítószerrel használati visszaállításához. A kimenet a webes szolgáltatás olvasások: betegek 35 évnél fiatalabb alatt, hogy előző kábítószer-kezelés 2 többször, hosszú helyi kezelés program véve és heroin és a kokain felhasználású, térjen vissza a kábítószerrel használati valószínűségét 95.64 % 500 nap.

## <a name="creation-of-web-service"></a>Webes szolgáltatás létrehozása
> Ez a webszolgáltatás Azure Machine Learning segítségével lett létrehozva. Az ingyenes próbaverzió, valamint a bevezető videó kísérletek létrehozásával és [közzétételi webes szolgáltatások](machine-learning-publish-a-machine-learning-web-service.md), lásd: [azure.com/ml](http://azure.com/ml). Az alábbiakban van egy Képernyőkép a kísérlet, amely a webes szolgáltatás, és példa kód létre minden egyes belül modulok.
> 
> 

Azure Machine Learning belül egy új üres kísérlet létrehozásához és két [R-parancsfájl végrehajtása] [ execute-r-script] modulok lekért a munkaterület-kiszolgálóra. Az adatok séma, amely egy egyszerű készült [R-parancsfájl végrehajtása][execute-r-script], amely megadja, hogy a bemeneti adatok séma a webszolgáltatáshoz. Ez a modul majd csatolva van a második [R-parancsfájl végrehajtása] [ execute-r-script] modult, amely a fő munka. Ez a modul az adatok előfeldolgozása, a modell létrehozásának és az előrejelzés végzi. Az adatok előfeldolgozási lépésben a bemeneti adatok hosszú karakterlánc által képviselt át legyenek-e, és adatok keret alakítja át. A modell épület lépésben egy külső R csomagot "survival_2.37-7.zip" első telepítésekor túlélési elemzés elvégzéséhez. Majd a "coxph" függvény végrehajtása után egy sorozat adatfeldolgozási feladatok. A "coxph" függvény túlélési elemzéshez részleteit az R dokumentációjában olvasható. Az előrejelzés lépésben egy tesztelési példány van megadva a "surfit" függvény a betanított modell, és a tesztelési példány túlélési görbe "görbe" változóként jön létre. Végezetül fontos idő valószínűségét kapott. 

### <a name="experiment-flow"></a>Kísérlet folyamata:
![Kísérlet folyamata][1]

#### <a name="module-1"></a>1. modul:
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

#### <a name="module-2"></a>A modul 2:
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




## <a name="limitations"></a>Korlátozások
A webszolgáltatás szolgáltatás változók (oszlop) csak numerikus értékeket vehet igénybe. Az "event" oszlop csak 0 vagy 1 értéket vehet igénybe. A "idő" oszlopot kell pozitív egész számnak kell lennie.

## <a name="faq"></a>GYIK
Gyakori kérdések a felhasználás a webszolgáltatás vagy az Azure piactéren közzétételt, lásd: [Itt](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-survival-analysis/survive_img2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

