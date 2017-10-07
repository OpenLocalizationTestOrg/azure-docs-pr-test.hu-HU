---
title: "aaa(deprecated) bináris osztályozó - Azure |} Microsoft Docs"
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
redirect_document_id: True
ms.openlocfilehash: 0496fcec9952ca243270caf67f55fe191b2dc9f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deprecated-binary-classifier"></a>(elavult) Bináris osztályozó

> [!NOTE]
> a Microsoft DataMarket hello használatból van, és ez az API már elavult. 
> 
> Sok hasznos példa kísérletek és API-kat az található hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com). Gyűjteményelem hello kapcsolatos további információkért lásd: [megosztás és a Cortana Intelligence Gallery hello erőforrások felderítéséhez](machine-learning-gallery-how-to-use-contribute-publish.md).

Tegyük fel, hogy egy adatkészlet rendelkezik, és szeretné toopredict egy bináris függő változó hello független változók alapján. "Logisztikai regresszió" a népszerű statisztikai technika ilyen előrejelzéseket használatos. Itt hello függő változó bináris vagy dichotomous, pedig p hello jellemző érdeklő jelenléte hello valószínűségét. 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Egy egyszerű forgatókönyv lehet, ahol egy biztonságkutatói próbál toopredict-e a potenciális student valószínűleg tooaccept egy beléptetésre ajánlat tooa egyetemi (középiskolai, termékcsalád bevétel, rezidens állapot GPA nemek közötti) adatok alapján. előre jelzett hello eredménye-hello potenciális student hello beléptetésre ajánlat elfogadása hello valószínűségét. Ez [webszolgáltatás](https://datamarket.azure.com/dataset/aml_labs/log_regression) elfér hello logisztikai regresszió toohello modelladatok és kimenetek hello valószínűségi érték (y) az egyes hello megfigyelések hello adataiban.  

> Ez a webszolgáltatás kell fenntartania – potenciálisan végig a mobilalkalmazások a webhelyen keresztül, vagy akár a helyi számítógépen, a felhasználókat például. De hello hello webszolgáltatás célja is tooserve példa bemutatja, hogyan Azure Machine Learning webszolgáltatások használt toocreate fölött R-kód is lehet. Az R-kód csupán néhány sornyi és az Azure Machine Learning Studio egy gombját kattint egy kísérlet hozható létre az R-kód és webszolgáltatásként közzé. hello webszolgáltatás majd lehet közzétett toohello Azure piactéren, és a felhasználók és eszközök által felhasznált között hello world hello Szerző hello webszolgáltatás infrastruktúra beállítás nélkül.  
> 
> 

## <a name="consumption-of-web-service"></a>Felhasználási webszolgáltatás
A webes szolgáltatás által biztosított hello értékek hello függő változó összes hello megfigyelések hello független változók alapján előre jelezni. hello webszolgáltatás várja hello végfelhasználói tooinput adatok egy karakterláncot, ahol sorok vesszővel (,) válassza el egymástól, és oszlopok pontosvesszővel (;) válassza el egymástól. hello webszolgáltatás egyszerre 1 sor vár, és hello első oszlop toobe hello függő változó vár. Egy példa adatkészlet nézhet ki:

![Mintaadatok][1]

Megfigyelések egy függő változó nélkül y kell megadnia, "NA"KARAKTERLÁNCOT. hello adatokat adjon meg a fenti dataset hello volna kell hello a következő karakterláncot: "1; 5; 2,1; 1; 6,0; 5.3; 2.1,0; 5; 5,0; 3; 4,1; 2; 1., NA; 3; 4". a kimeneti hello van hello előre jelzett érték az egyes hello sorok hello független változók alapján. 

> Ez a szolgáltatás az Azure piactér hello kihelyezett egy OData-szolgáltatás; a POST vagy GET módszerrel elnevezése lehet. 
> 
> 

Többféleképpen is az automatizált módon hello szolgáltatás fel (egy példa alkalmazás [Itt](http://microsoftazuremachinelearning.azurewebsites.net/BinaryClassifier.aspx)).

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
> Ez a webszolgáltatás Azure Machine Learning segítségével lett létrehozva. Az ingyenes próbaverzió, valamint a bevezető videó kísérletek létrehozásával és [közzétételi webes szolgáltatások](machine-learning-publish-a-machine-learning-web-service.md), lásd: [azure.com/ml](http://azure.com/ml). Az alábbiakban van egy képernyőfelvétel a hello webes szolgáltatás, és példa kódot az egyes hello modulok hello kísérlet belül létrehozott hello kísérlet.
> 
> 

Azure Machine Learning belül egy új üres kísérlet létrehozásához és két [R-parancsfájl végrehajtása] [ execute-r-script] modulok lekért hello munkaterület-kiszolgálóra. Ez a webszolgáltatás egy Azure Machine Learning kísérlet fut, alapul szolgáló R-parancsfájl. Nincsenek a 2 részek toothis kísérletezhet: schema definíció jelenik meg, és a tanítási modell + pontozási. hello első modul várt hello struktúrát hello bemeneti adatkészlet, ahol a hello első változó hello függő változó és hello fennmaradó változók független határozza meg. hello második modul megfelelő hello bemeneti adatok általános logisztikai regresszió modellt.    

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
Itt látható egy nagyon egyszerű példa egy bináris osztályozási webszolgáltatás. Hello példakódot fent is látható, mert nincs hiba alatt van megvalósítva és hello szolgáltatás feltételezi, hogy egy bináris/folyamatos változó (ahol nincs kategorikus funkció engedélyezett), hello szolgáltatást csak bemenetek numerikus értékként hello hello létrehozása során webes szolgáltatás. Is hello szolgáltatás jelenleg korlátozott adatméret kezeli, miatt toohello kérelem-válasz jellegű hello webszolgáltatás hívás és hello tényt, hogy a modell hello alatt alkalmas minden alkalommal, amikor hello webszolgáltatás nevezik. 

## <a name="faq"></a>GYIK
Gyakori kérdések a felhasználás hello webszolgáltatás vagy az Azure piactér közzétételi toohello, lásd: [Itt](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-binary-classifier/binary1.png
[2]: ./media/machine-learning-r-csharp-binary-classifier/binary2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

