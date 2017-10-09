---
title: "aaa(deprecated) túlélési elemzése Azure Machine Learning segítségével |} Microsoft Docs"
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
redirect_document_id: True
ms.openlocfilehash: af946d8df5ba650a9d74fbabbe3b15d3a07dd508
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deprecated-survival-analysis"></a>(elavult) Túlélési elemzés

> [!NOTE]
> a Microsoft DataMarket hello használatból van, és ez az API már elavult. 
> 
> Sok hasznos példa kísérletek és API-kat az található hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com). Gyűjteményelem hello kapcsolatos további információkért lásd: [megosztás és a Cortana Intelligence Gallery hello erőforrások felderítéséhez](machine-learning-gallery-how-to-use-contribute-publish.md).

Sok esetben a hello fő alatt assessment eredménye hello tooan esemény iránt. Más szóval hello kérdés "Ha ez az esemény történik?" meg kell adnia. Példaként, fontolja meg az olyan helyzetekben, ahol hello adatokat ismerteti hello futása közben eltelt idő (nap, év, távolság, stb.) addig hello iránt (elleni relapse, kapott doktori fok, berendezés pad hibája) esemény akkor következik be. Minden példánya hello adatokat (a türelmet student, egy autó, stb.) egy adott objektumot jelöli.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Ez [webszolgáltatás](https://datamarket.azure.com/dataset/aml_labs/survivalanalysis) megválaszolja hello kérdést a "hello annak valószínűségét, hogy egyik fontos esemény hello Újdonságok történik idő n objektum x?" Egy túlélési elemzési modellt biztosít, a webszolgáltatás lehetővé teszi, hogy a felhasználók toosupply tootrain hello adatmodell, és tesztelik azt. hello fő téma hello kísérlet nem toomodel hello hossza hello eltelt idő, amíg érdeklő hello esemény akkor következik be. 

> Ez a webszolgáltatás kell fenntartania – potenciálisan végig a mobilalkalmazások a webhelyen keresztül, vagy akár a helyi számítógépen, a felhasználókat például. De hello hello webszolgáltatás célja is tooserve példa bemutatja, hogyan Azure Machine Learning webszolgáltatások használt toocreate fölött R-kód is lehet. Az R-kód csupán néhány sornyi és az Azure Machine Learning Studio egy gombját kattint egy kísérlet hozható létre az R-kód és webszolgáltatásként közzé. hello webszolgáltatás majd lehet közzétett toohello Azure piactéren, és a felhasználók és eszközök által felhasznált között hello world hello Szerző hello webszolgáltatás infrastruktúra beállítás nélkül.  
> 
> 

## <a name="consumption-of-web-service"></a>Felhasználási webszolgáltatás
hello bemeneti adatok séma hello webszolgáltatás hello a következő táblázatban látható. Hat adatra van szükség hello bemenetként: betanítási adatok, a vizsgálati adatok, érdeklő idő, a "time" dimenzió hello indexe, hello index "event" dimenzió és hello változó (folyamatos vagy tényező). hello betanítási adatok karakterláncot, ahol hello sorok vesszővel vannak elválasztva, és pontosvesszővel elválasztott hello oszlopok képviseli. Néhány hello szolgáltatás hello adatok kvórummodellje rugalmas. A bemeneti karakterlánc hello összes hello elem számokkal kell megadni. Hello betanítási adatok, a "hello"időbeli dimenzió jelzi hello kiindulópontjaként hello tanulmányozzák (egy türelmet kábítószerrel kezelés programok, student kezdési doktori tanulmány, egy autó toobe kezdési fogadása óta eltelt időegységek (nap, év, távolság, stb.) hello száma hajtott, stb.) amíg hello iránt (hello türelmet visszaadó toodrug használati, hello student beszerzését hello doktori fok, hello car rendelkező berendezés pad hiba stb.) az esemény akkor következik be. hello "event" dimenzió meghatározza, hogy egyik fontos hello esemény hello vizsgálat hello végén. Érték "esemény = 1" azt jelenti, hogy hello érdeklő esemény kerül sor, hello "idő" dimenzió; által jelzett hello időpontban "esemény = 0" hello "idő" dimenzió által jelzett hello idő szerint nem történt, azt jelenti, hogy az egyik fontos esemény hello.

* trainingdata - karakterlánc. Vesszővel elválasztott sorok és oszlopok pontosvesszővel válassza el egymástól. Minden egyes sorára "Idődimenzió", "event" dimenzió és előrejelzőjének változót tartalmaz.
* testingdata - adatokat tartalmazó előrejelzőjének változók egy adott objektum tartalmaz egy sort.
* time_of_interest - érdeklődési n hello eltelt időt.
* index_time - hello oszlopindex hello "idő" dimenzió (1-től kezdődő).
* index_event - hello oszlopindex hello "event" dimenzió (1-től kezdődő).
* variable_types - karakterlánc pontosvesszővel válassza el az azt elválasztóként. 0 folyamatos változók és a 1 tényező változók jelenti.

hello eredménye hello valószínűségértékének inverzét által egy adott időpontban bekövetkező esemény. 

> Ez a szolgáltatás az Azure piactér hello kihelyezett egy OData-szolgáltatás; a POST vagy GET módszerrel elnevezése lehet. 
> 
> 

Többféleképpen is az automatizált módon hello szolgáltatás fel (egy példa alkalmazás [Itt](http://microsoftazuremachinelearning.azurewebsites.net/SurvivalAnalysis.aspx)). 

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




Ez a vizsgálat hello értékelése a következőképpen történik. Feltéve, hogy hello cél hello adatok toomodel hello eltelt idő, amíg hello hello betegeknél hello két kezelés programok kapó toodrug használati vissza. hello kimeneti hello web service olvasások: betegek 35 évnél fiatalabb alatt, hogy előző kábítószer-kezelés 2 többször, véve hello hosszú helyi kezelés program, és heroin és a kokain felhasználású hello valószínűségértékének inverzét toohello kábítószerrel használati visszaadó 95.64 %-a 500 nap.

## <a name="creation-of-web-service"></a>Webes szolgáltatás létrehozása
> Ez a webszolgáltatás Azure Machine Learning segítségével lett létrehozva. Az ingyenes próbaverzió, valamint a bevezető videó kísérletek létrehozásával és [közzétételi webes szolgáltatások](machine-learning-publish-a-machine-learning-web-service.md), lásd: [azure.com/ml](http://azure.com/ml). Az alábbiakban van egy képernyőfelvétel a hello webes szolgáltatás, és példa kódot az egyes hello modulok hello kísérlet belül létrehozott hello kísérlet.
> 
> 

Azure Machine Learning belül egy új üres kísérlet létrehozásához és két [R-parancsfájl végrehajtása] [ execute-r-script] modulok lekért hello munkaterület-kiszolgálóra. hello adatok jött létre séma egy egyszerű [R-parancsfájl végrehajtása][execute-r-script], amely megadja, hogy hello bemeneti adatok séma hello webszolgáltatáshoz. Ez a modul az majd toohello második kapcsolódó [R-parancsfájl végrehajtása] [ execute-r-script] modult, amely a fő munka. Ez a modul az adatok előfeldolgozása, a modell létrehozásának és az előrejelzés végzi. Hello adatok előfeldolgozási lépésben hello bemeneti adatokat egy hosszú karakterlánc által meghatározott át legyenek-e, és adatok keret alakítja át. Hello modell épület lépésben egy külső R csomagot "survival_2.37-7.zip" első telepítésekor túlélési elemzés elvégzéséhez. Majd hello "coxph" függvény végrehajtása után egy sorozat adatfeldolgozási feladatok. hello részletek hello "coxph" függvény túlélési elemzéshez hello R dokumentációjában olvasható. Hello előrejelzés lépésben a tesztelési példánya van megadva a betanított modell hello "surfit" hello funkcióval, és hello túlélési görbe tesztelési példányhoz tartozó hozzák "görbe" változóként. Végezetül hello idő érdeklő hello valószínűségét kapott. 

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

    maml.mapOutputPort("sampleInput"); #send data toooutput port

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

    # Prepare toobuild model
    attach(mydata)

    for (i in 1:n_col){ mydata[,i]=as.numeric(mydata[,i])} 
    d_time=paste("X",index_time,sep = "")
    d_event=paste("X",index_event,sep = "")
    v_time_event <- c(d_time,d_event)
    v_predictors = names(mydata)[!(names(mydata) %in% v_time_event)]

    variable_types = unlist(strsplit(as.character(variable_types),";"))

    len = length(v_predictors)
    c="" # Construct hello execution string
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

    # Fit a Cox proportional hazards model and get hello predicted survival curve for a testing instance 
    fit=eval(parse(text=f))

    testingdata = as.data.frame(matrix(testingdata, ncol=len,byrow = TRUE),stringsAsFactors=FALSE)
    names(testingdata)=v_predictors
    for (i in 1:len){ testingdata[,i]=as.numeric(testingdata[,i])}

    curve=survfit(fit,testingdata)

    # Based on user input, find hello event occurrence probability
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

    #Pull out results toosend tooweb service
    output=paste(round(100*output, 2), "%") 
    maml.mapOutputPort("output"); #output port




## <a name="limitations"></a>Korlátozások
A webszolgáltatás szolgáltatás változók (oszlop) csak numerikus értékeket vehet igénybe. hello "event" oszlop csak 0 vagy 1 értéket vehet igénybe. hello "idő" oszlopot toobe pozitív egész számnak kell.

## <a name="faq"></a>GYIK
Gyakori kérdések a felhasználás hello webszolgáltatás vagy az Azure piactér közzétételi toohello, lásd: [Itt](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-survival-analysis/survive_img2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

