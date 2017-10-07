---
title: Azure - modell aaa(deprecated) |} Microsoft Docs
description: "(elavult) Fürt modell"
services: machine-learning
documentationcenter: 
author: FrancescaLazzeri
manager: jhubbard
editor: cgronlun
ms.assetid: 51b8d012-ed44-4312-920c-9c808dfd4ff6
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: lazzeri
ROBOTS: NOINDEX
redirect_url: https://gallery.cortanaintelligence.com/
redirect_document_id: True
ms.openlocfilehash: 7b2dffb855a8d91114752b579115e97d07210e45
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deprecated-cluster-model"></a>(elavult) Fürt modell

> [!NOTE]
> a Microsoft DataMarket hello használatból van, és ez az API már elavult. 
> 
> Sok hasznos példa kísérletek és API-kat az található hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com). Gyűjteményelem hello kapcsolatos további információkért lásd: [megosztás és a Cortana Intelligence Gallery hello erőforrások felderítéséhez](machine-learning-gallery-how-to-use-contribute-publish.md).

Hogyan azt előrejelzése jóváírás cardholders viselkedésmódok rendelés tooreduce hello kell fizetni az indító kockázatot hitelkártya kiállítók csoportok? Hogyan azt megadására az alkalmazottak személy jellemzők csoportját a rendelés tooimprove munkahelyi teljesítményének? Hogyan lehet sorolni orvosi betegek hello jellemzői a betegségek alapuló? Alapvetően összes ezeket a kérdéseket is válaszolni fürt elemzése révén.   

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Fürt elemzés csoportokba két vagy több egymást kölcsönösen kizáró ismeretlen változók kombinációi megfigyelések készlete osztályozza. hello fürt elemzés célja toodiscover rendszert megfigyelések, általában személyek vagy jellemzőik, rendezése csoportokba, ahol hello csoportok tagjai tulajdonságok megosztása közös. Ez [szolgáltatás](https://datamarket.azure.com/dataset/aml_labs/k_cluster_model) használ hello K-közép módszertan használatával, a gyakran használt csoportosítási módszer toocluster tetszőleges adatok csoportokba. Ez a webszolgáltatás hello adatokat fogad, és bemenetként fürtök, és hozza létre az előrejelzéseket, amelyek hello k csoportok toowhich minden megfigyelések tartozik k hello száma. 

> Ez a webszolgáltatás kell fenntartania – potenciálisan végig a mobilalkalmazások a webhelyen keresztül, vagy akár a helyi számítógépen, a felhasználókat például. De hello hello webszolgáltatás célja is tooserve példa bemutatja, hogyan Azure Machine Learning webszolgáltatások használt toocreate fölött R-kód is lehet. Az R-kód csupán néhány sornyi és az Azure Machine Learning Studio egy gombját kattint egy kísérlet hozható létre az R-kód és webszolgáltatásként közzé. hello webszolgáltatás majd lehet közzétett toohello Azure piactéren, és a felhasználók és eszközök által felhasznált között hello world hello Szerző hello webszolgáltatás infrastruktúra beállítás nélkül.  
> 
> 

## <a name="consumption-of-web-service"></a>Felhasználási webszolgáltatás
Ez a webszolgáltatás hello adatok csoportosítja k csoportok és -kimenetek hello csoport-hozzárendelés minden egyes sorára készlete. hello webszolgáltatás hello végfelhasználói tooinput adatok vár, ahol sorok egymástól vesszővel (,) válassza el egymástól, és oszlopok pontosvesszővel (;) elválasztott karakterlánc. hello webszolgáltatás 1 sor egyszerre vár. Egy példa adatkészlet nézhet ki:

![Mintaadatok][1]

Tegyük fel, hogy hello kívánta felhasználói tooseparate ezek az adatok 3 egymást kölcsönösen kizáró csoportokba. a fenti dataset hello hello következő bemeneti adatokat hello: érték = "10; 5; 2,18; 1; 6,7; 5; 5,22; 3; 4,12; 2; 1,10; 3; 4"; k = "3". hello kimeneti van hello előre jelzett csoporttagság az egyes hello sorokat.

> Ez a szolgáltatás az Azure piactér hello kihelyezett egy OData-szolgáltatás; a POST vagy GET módszerrel elnevezése lehet. 
> 
> 

Többféleképpen is az automatizált módon hello szolgáltatás fel (egy példa alkalmazás [Itt](http://microsoftazuremachinelearning.azurewebsites.net/ClusterModel.aspx)).

### <a name="starting-c-code-for-web-service-consumption"></a>C#-kódban a webes szolgáltatások felhasználásához megkezdése:
    public class Input
    {
            public string value;
            public string k;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { value = TextBox1.Text, k = TextBox2.Text };
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

Azure Machine Learning belül egy új üres kísérlet létrehozásához és két [R-parancsfájl végrehajtása] [ execute-r-script] modulok lekért hello munkaterület-kiszolgálóra. hello adatkulcsokat létrejött egy egyszerű [R-parancsfájl végrehajtása][execute-r-script]. Ezt követően hello adatkulcsokat lett csatolt toohello modell szakaszban, újra létre a fürt egy [R-parancsfájl végrehajtása][execute-r-script]. A hello [R-parancsfájl végrehajtása] [ execute-r-script] hello fürt modellt használja, hello webszolgáltatás majd használja az "k-közép" hello függvény, amely hello az előre elkészített [R-parancsfájl végrehajtása] [ execute-r-script] az Azure gépi tanulás.    

![Kísérlet folyamata][3]

#### <a name="module-1"></a>1. modul:
    #Enter hello input data as a string 
    mydata <- data.frame(value = "1; 3; 5; 6; 7; 7, 5; 5; 6; 7; 2; 1, 3; 7; 2; 9; 56; 6, 1; 4; 5; 26; 4; 23, 15; 35; 6; 7; 12; 1, 32; 51; 62; 7; 21; 1", k=5, stringsAsFactors=FALSE)

    maml.mapOutputPort("mydata");     


#### <a name="module-2"></a>A modul 2:
    # Map 1-based optional input ports toovariables
    mydata <- maml.mapInputPort(1) # class: data.frame

    data.split <- strsplit(mydata[1,1], ",")[[1]]
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)
    data.split <- as.data.frame(t(data.split))

    data.split <- data.matrix(data.split)
    data.split <- data.frame(data.split)

    # K-Means cluster analysis
    fit <- kmeans(data.split, mydata$k) # k-cluster solution

    # Get cluster means 
    aggregate(data.split,by=list(fit$cluster),FUN=mean)
    # Append cluster assignment
    mydatafinal <- data.frame(t(fit$cluster))
    n_col=ncol(mydatafinal)
    colnames(mydatafinal) <- paste("V",1:n_col,sep="")

    # Select data.frame toobe sent toohello output Dataset port
    maml.mapOutputPort("mydatafinal");


## <a name="limitations"></a>Korlátozások
Itt látható egy nagyon egyszerű példa egy csoportosítási webszolgáltatás. Hello példakódot fent is látható, mert nincs hiba alatt van megvalósítva és hello szolgáltatás feltételezi, hogy minden rendben (ahol nincs kategorikus funkció engedélyezett), a folyamatos változó hello szolgáltatást csak bemenetek numerikus értékként hello létrehozását webhely hello helyreállításkor a szolgáltatás. Is hello szolgáltatás jelenleg korlátozott adatméret kezeli, miatt toohello kérelem-válasz jellegű hello webszolgáltatás hívás és hello tényt, hogy a modell hello alatt alkalmas minden alkalommal, amikor hello webszolgáltatás nevezik. 

## <a name="faq"></a>GYIK
Gyakori kérdések a felhasználás hello webszolgáltatás vagy az Azure piactér közzétételi toohello, lásd: [Itt](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-cluster-model/cluster-img1.png
[2]: ./media/machine-learning-r-csharp-cluster-model/cluster-img2.png
[3]: ./media/machine-learning-r-csharp-cluster-model/cluster-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

