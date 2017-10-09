---
title: "aaa(deprecated) arányok teszt - Azure különbség |} Microsoft Docs"
description: "(elavult) Különbség arányok teszt"
services: machine-learning
documentationcenter: 
author: aniedea
manager: jhubbard
editor: cgronlun
ms.assetid: 9356b821-5345-44f6-8e26-719f2dea5e6d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: aniedea
ROBOTS: NOINDEX
redirect_url: https://gallery.cortanaintelligence.com/
redirect_document_id: True
ms.openlocfilehash: 820aad377f9dec12b0ef455974aaa95f6e8d723a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deprecated-difference-in-proportions-test"></a>(elavult) Különbség arányok teszt

> [!NOTE]
> a Microsoft DataMarket hello használatból van, és ez az API már elavult. 
> 
> Sok hasznos példa kísérletek és API-kat az található hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com). Gyűjteményelem hello kapcsolatos további információkért lásd: [megosztás és a Cortana Intelligence Gallery hello erőforrások felderítéséhez](machine-learning-gallery-how-to-use-contribute-publish.md).

Eltérőek két arányok statisztikailag? Tegyük fel, hogy egy felhasználó azt szeretné, ha egy movie jelentősen nagyobb arányban toocompare két filmek toodetermine "kedveli" mikor más toohello képest. Nagy mintával lehet hello arányok 0,50 és 0,51 statisztikailag jelentős különbsége. A mintát nem lehet elég adat toodetermine Ha ezek arányok ténylegesen eltérőek. 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Ez [webszolgáltatás](https://datamarket.azure.com/dataset/aml_labs/prop_test) végez egy t-próba a hello különbség a két arányban felhasználói bevitel sikeres hello száma és hello hello 2 összehasonlítás csoportok kísérletek teljes száma alapján. Egy lehetséges esetben ez a webszolgáltatás sikerült hívható meg a movie összehasonlítás alkalmazások szólítja fel hello felhasználót, hogy egy hello filmek van valóban "tetszését" gyakrabban hello más, mint a film minősítése alapján.

> Ez a webszolgáltatás kell fenntartania – potenciálisan végig a mobilalkalmazások a webhelyen keresztül, vagy akár a helyi számítógépen, a felhasználókat például. De hello hello webszolgáltatás célja is tooserve példa bemutatja, hogyan Azure Machine Learning webszolgáltatások használt toocreate fölött R-kód is lehet. Az R-kód csupán néhány sornyi és az Azure Machine Learning Studio egy gombját kattint egy kísérlet hozható létre az R-kód és webszolgáltatásként közzé. hello webszolgáltatás majd lehet közzétett toohello Azure piactéren, és a felhasználók és eszközök által felhasznált között hello world hello Szerző hello webszolgáltatás infrastruktúra beállítás nélkül.
> 
> 

## <a name="consumption-of-web-service"></a>Felhasználási webszolgáltatás
A szolgáltatás fogadja a 4 argumentum, és nem egy alapul tesztelése a arányok.

hello bemeneti argumentumai a következők:

* Successes1 - minta 1 sikeres események száma.
* Successes2 - minta 2 sikeres események száma.
* Total1 - minta 1 mérete.
* Total2 - mintát 2.

hello hello szolgáltatás eredménye hello feltevése hello eredményét hello chi-square statisztika, df, p-értékkel együtt tesztelése, és része, a minta 1/2 és a megbízhatósági határain.

> Ez a szolgáltatás az Azure piactér hello kihelyezett egy OData-szolgáltatás; a POST vagy GET módszerrel elnevezése lehet. 
> 
> 

Többféleképpen is az automatizált módon hello szolgáltatás fel (egy példa alkalmazás [Itt](http://microsoftazuremachinelearning.azurewebsites.net/DifferenceInProportionsTest.aspx)).

### <a name="starting-c-code-for-web-service-consumption"></a>C#-kódban a webes szolgáltatások felhasználásához megkezdése:
    public class Input
    {
            public string successes1;
            public string successes2;
            public string total1;
            public string total2;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { successes1 = TextBox1.Text, successes2 = TextBox2.Text, total1 = TextBox3.Text, total2 = TextBox4.Text };
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
> Ez a webszolgáltatás Azure Machine Learning segítségével lett létrehozva. Az ingyenes próbaverzió, valamint a bevezető videó kísérletek létrehozásával és [közzétételi webes szolgáltatások](machine-learning-publish-a-machine-learning-web-service.md), lásd: [azure.com/ml](http://azure.com/ml). Az alábbiakban van egy képernyőfelvétel a hello webes szolgáltatás, és példa kódot az egyes hello modulok hello kísérlet belül létrehozott hello kísérlet.
> 
> 

Azure Machine Learning belül egy új üres kísérlet létrehozásához és két [R-parancsfájl végrehajtása] [ execute-r-script] modulok. Hello első modul hello adatok sémát, miközben a hello második modul 2 arányok hello prop.test parancs R tooperform hello a t-próba belül használja. 

### <a name="experiment-flow"></a>Kísérlet folyamata:
![Kísérlet folyamata][2]

#### <a name="module-1"></a>1. modul:
    ####Schema definition  
    data.set=data.frame(successes1=50,successes2=60,total1=100,total2=100);
    maml.mapOutputPort("data.set"); #send data toooutput port
    dataset1 = maml.mapInputPort(1) #read data from input port


#### <a name="module-2"></a>A modul 2:
    test=prop.test(c(dataset1$successes1[1],dataset1$successes2[1]),c(dataset1$total1[1],dataset1$total2[1])) #conduct hypothesis test

    result=data.frame(
    result=ifelse(test$p.value<0.05,"hello proportions are different!",
    "hello proportions aren't different statistically."),
    ChiSquarestatistic=round(test$statistic,2),df=test$parameter,
    pvalue=round(test$p.value,4),
    proportion1=round(test$estimate[1],4),
    proportion2=round(test$estimate[2],4),
    confintlow=round(test$conf.int[1],4),
    confinthigh=round(test$conf.int[2],4)) 

    maml.mapOutputPort("result"); #output port


## <a name="limitations"></a>Korlátozások
Ez az eltérés 2 arányban tesztjének egy nagyon egyszerű példa. Hello példakódot fent is látható, mert nincs hiba alatt van megvalósítva, és hello szolgáltatás feltételezi, hogy minden hello változók folyamatos.

## <a name="faq"></a>GYIK
Gyakori kérdések a felhasználás hello webszolgáltatás vagy az Azure piactér közzétételi toohello, lásd: [Itt](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-difference-in-two-proportions/hyptest-img1.png
[2]: ./media/machine-learning-r-csharp-difference-in-two-proportions/hyptest-img2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

