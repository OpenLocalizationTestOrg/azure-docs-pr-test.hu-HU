---
title: "(elavult) Bináris osztályozó - Azure |} Microsoft Docs"
description: "(elavult) Bináris osztályozó"
services: machine-learning
documentationcenter: 
author: jaymathe
manager: jhubbard
editor: cgronlun
ms.assetid: 8045038a-9dcf-44b9-a6de-7f1f8e791575
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: jaymathe
ROBOTS: NOINDEX
redirect_url: https://gallery.cortanaintelligence.com/
redirect_document_id: TRUE
ms.openlocfilehash: 1a83392f90bb5a9fb183334c03ccec20dd3f3520
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="deprecated-binary-classifier"></a>(elavult) Bináris osztályozó

> [!NOTE]
> A Microsoft DataMarket használatból van, és ez az API már elavult. 
> 
> Sok hasznos példa kísérletek és API-k a [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com). A gyűjtemény kapcsolatos további információkért lásd: [megosztást, és felderítik a Cortana Intelligence Gallery erőforrások](machine-learning-gallery-how-to-use-contribute-publish.md).

Tegyük fel, hogy egy adatkészlet rendelkezik, és szeretné egy bináris függő változó a független változók alapján előre jelezni. "Logisztikai regresszió" a népszerű statisztikai technika ilyen előrejelzéseket használatos. Itt a függő változó bináris vagy dichotomous, és p az egyik fontos jellemzője jelenléte valószínűségét. 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Egy egyszerű forgatókönyv lehet, ahol egy biztonságkutatói próbál előre jelezni, hogy rendelkezik leendő student várhatóan fogadja el a egyetemi (középiskolai, termékcsalád bevétel, rezidens állapot GPA nemek közötti) adatok alapján történő beléptetésre ajánlatot. Az előre jelzett eredménye a potenciális student a beléptetésre ajánlat elfogadása valószínűségét. Ez [webszolgáltatás](https://datamarket.azure.com/dataset/aml_labs/log_regression) megfelel a logisztikai regresszió modellt az adatokat, és kiírja a valószínűségi érték (y) minden az adatok a megfigyelt.  

> Ez a webszolgáltatás kell fenntartania – potenciálisan végig a mobilalkalmazások a webhelyen keresztül, vagy akár a helyi számítógépen, a felhasználókat például. De a webszolgáltatás célja is példa bemutatja, hogyan Azure Machine Learning webszolgáltatások fölött R-kód létrehozásához használható kiszolgálásához. Az R-kód csupán néhány sornyi és az Azure Machine Learning Studio egy gombját kattint egy kísérlet hozható létre az R-kód és webszolgáltatásként közzé. A webszolgáltatás majd közzé az Azure piactéren, és felhasználók és eszközök által felhasznált világszerte a szerző, a webszolgáltatás által infrastruktúra beállítás nélkül.  
> 
> 

## <a name="consumption-of-web-service"></a>Felhasználási webszolgáltatás
Ennek a webszolgáltatásnak ad az összes megfigyelések független változók alapján függő változó előre jelzett értékek. A webszolgáltatás vár a felhasználó a bemeneti adatok karakterláncként, ahol sorok vesszővel (,) válassza el egymástól, és oszlopok pontosvesszővel (;) válassza el egymástól. A webszolgáltatás egyszerre 1 sor vár, és a függő változó az első oszlopot vár. Egy példa adatkészlet nézhet ki:

![Mintaadatok][1]

Megfigyelések egy függő változó nélkül y kell megadnia, "NA"KARAKTERLÁNCOT. Az adatok, adjon meg, mert a fenti adatkészlet a következő karakterláncot: "1; 5; 2,1; 1; 6,0; 5.3; 2.1,0; 5; 5,0; 3; 4,1; 2; 1., NA; 3; 4". A kimeneti az előre jelzett érték az egyes a sorok, a független változók alapján. 

> Ez a szolgáltatás az Azure piactéren kihelyezett egy OData-szolgáltatás; a POST vagy GET módszerrel elnevezése lehet. 
> 
> 

Többféleképpen is az automatizált módon a szolgáltatás fel (egy példa alkalmazás [Itt](http://microsoftazuremachinelearning.azurewebsites.net/BinaryClassifier.aspx)).

### <a name="starting-c-code-for-web-service-consumption"></a>C#-kódban a webes szolgáltatások felhasználásához megkezdése:
    public class Input
    {
           public string value;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
        byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
        return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
        var input = new Input() { value = TextBox1.Text };
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
> Ez a webszolgáltatás Azure Machine Learning segítségével lett létrehozva. Az ingyenes próbaverzió, valamint a bevezető videó kísérletek létrehozásával és [közzétételi webes szolgáltatások](machine-learning-publish-a-machine-learning-web-service.md), lásd: [azure.com/ml](http://azure.com/ml). Az alábbiakban van egy Képernyőkép a kísérlet, amely a webes szolgáltatás, és példa kód létre minden egyes belül modulok.
> 
> 

Azure Machine Learning belül egy új üres kísérlet létrehozásához és két [R-parancsfájl végrehajtása] [ execute-r-script] modulok lekért a munkaterület-kiszolgálóra. Ez a webszolgáltatás egy Azure Machine Learning kísérlet fut, alapul szolgáló R-parancsfájl. Ehhez a kísérlethez 2 részből áll: schema definíció jelenik meg, és a tanítási modell + pontozási. Az első modul határozza meg a bemeneti adatkészlet, ahol az első változó a függő változó, és a fennmaradó változók függetlenek a várt szerkezetnek. A második modul megfelel a bemeneti adatok általános logisztikai regresszió modellt.    

![Kísérlet folyamata][2]

#### <a name="module-1"></a>1. modul:
    #Schema definition  
    data <- data.frame(value = "1;2;3,1;5;6,0;8;9", stringsAsFactors=FALSE) 
    maml.mapOutputPort("data");  

#### <a name="module-2"></a>A modul 2:
    #GLM modeling   
    data <- maml.mapInputPort(1) # class: data.frame  

    data.split <- strsplit(data[1,1], ",")[[1]] 
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE) 
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE) 
    data.split <- as.data.frame(t(data.split)) data.split <- 
    data.matrix(data.split) 
    data.split <- data.frame(data.split) 

    model <- glm(data.split$V1 ~., family='binomial', data=data.split)  
    out <- data.frame(predict(model,data.split, type="response")) 
    pred1 <- as.data.frame(out) 
    group <- array(1:nrow(pred1)) 
    for (i in 1:nrow(pred1))  
        {
        if(as.numeric(pred1[i,])>0.5) {group[i]=1} 
        else {group[i]=0}
        } 
    pred2 <- as.data.frame(group) 
    maml.mapOutputPort("pred2");  


## <a name="limitations"></a>Korlátozások
Itt látható egy nagyon egyszerű példa egy bináris osztályozási webszolgáltatás. A fenti példa kódot is látható, mert nincs hiba alatt van megvalósítva, és a szolgáltatás azt feltételezi, hogy minden bináris/folyamatos változó (ahol nincs kategorikus funkció engedélyezett), a szolgáltatás csak bemenetek numerikus értékként ennek a webszolgáltatásnak létrehozásának időpontjában . Emellett a szolgáltatás jelenleg kezeli korlátozott adatok mérete, kellő a webszolgáltatás-hívások és azt a tényt, hogy a modell alatt alkalmas a webes szolgáltatás neve minden egyes kérelem/válasz jellegének. 

## <a name="faq"></a>GYIK
Gyakori kérdések a felhasználás a webszolgáltatás vagy az Azure piactéren közzétételt, lásd: [Itt](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-binary-classifier/binary1.png
[2]: ./media/machine-learning-r-csharp-binary-classifier/binary2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

