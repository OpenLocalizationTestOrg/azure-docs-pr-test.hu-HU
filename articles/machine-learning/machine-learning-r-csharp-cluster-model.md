---
title: "(elavult) A fürt modell - Azure |} Microsoft Docs"
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
redirect_document_id: TRUE
ms.openlocfilehash: 7cbbbd6d4236dab638eb3051595a584557480841
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="deprecated-cluster-model"></a>(elavult) Fürt modell

> [!NOTE]
> A Microsoft DataMarket használatból van, és ez az API már elavult. 
> 
> Sok hasznos példa kísérletek és API-k a [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com). A gyűjtemény kapcsolatos további információkért lásd: [megosztást, és felderítik a Cortana Intelligence Gallery erőforrások](machine-learning-gallery-how-to-use-contribute-publish.md).

Hogyan azt előrejelzése jóváírás cardholders viselkedésmódok csoportok hitelkártya kiállítók kell fizetni az indító kockázat csökkentése érdekében? Hogyan lehet meghatároztuk személy jellemzők alkalmazottak csoportját a munkahelyi teljesítményének javítása érdekében? Hogyan lehet sorolni orvosi betegek azok betegségek jellemzői alapuló? Alapvetően összes ezeket a kérdéseket is válaszolni fürt elemzése révén.   

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Fürt elemzés csoportokba két vagy több egymást kölcsönösen kizáró ismeretlen változók kombinációi megfigyelések készlete osztályozza. A fürt elemzés célja megfigyelések, általában személyek vagy jellemzőik, rendezése csoportokba rendszerének felderítését ahol a csoportok tagjainak tulajdonságok megosztása közös. Ez [szolgáltatás](https://datamarket.azure.com/dataset/aml_labs/k_cluster_model) használja a K-közép kiosztásának, gyakran használt csoportosítási módszer fürt tetszőleges adatok csoportokba. Ez a webszolgáltatás lekéri az adatokat, és bemenetként fürtök, és hozza létre az előrejelzéseket, amelyek a k tartalmazó csoportok minden megfigyelések k száma. 

> Ez a webszolgáltatás kell fenntartania – potenciálisan végig a mobilalkalmazások a webhelyen keresztül, vagy akár a helyi számítógépen, a felhasználókat például. De a webszolgáltatás célja is példa bemutatja, hogyan Azure Machine Learning webszolgáltatások fölött R-kód létrehozásához használható kiszolgálásához. Az R-kód csupán néhány sornyi és az Azure Machine Learning Studio egy gombját kattint egy kísérlet hozható létre az R-kód és webszolgáltatásként közzé. A webszolgáltatás majd közzé az Azure piactéren, és felhasználók és eszközök által felhasznált világszerte a szerző, a webszolgáltatás által infrastruktúra beállítás nélkül.  
> 
> 

## <a name="consumption-of-web-service"></a>Felhasználási webszolgáltatás
Ez a webszolgáltatás az adatok csoportosítja k csoportokat, és kiírja a csoport-hozzárendelés minden egyes sorára. A webszolgáltatás vár a felhasználó a bemeneti adatok karakterláncként, ahol sorok egymástól vesszővel (,) válassza el egymástól, és oszlopok egymástól pontosvesszővel (;). A webszolgáltatás 1 sor egyszerre vár. Egy példa adatkészlet nézhet ki:

![Mintaadatok][1]

Tegyük fel, hogy a felhasználó kívánta csoportokba ezek az adatok 3 kölcsönösen kizárják egymást. Adjon meg, mert a fenti adatkészlet a következő adatokat: érték = "10; 5; 2,18; 1; 6,7; 5; 5,22; 3; 4,12; 2; 1,10; 3; 4"; k = "3". Az előre jelzett csoporttagság az egyes sorok eredménye.

> Ez a szolgáltatás az Azure piactéren kihelyezett egy OData-szolgáltatás; a POST vagy GET módszerrel elnevezése lehet. 
> 
> 

Többféleképpen is az automatizált módon a szolgáltatás fel (egy példa alkalmazás [Itt](http://microsoftazuremachinelearning.azurewebsites.net/ClusterModel.aspx)).

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
> Ez a webszolgáltatás Azure Machine Learning segítségével lett létrehozva. Az ingyenes próbaverzió, valamint a bevezető videó kísérletek létrehozásával és [közzétételi webes szolgáltatások](machine-learning-publish-a-machine-learning-web-service.md), lásd: [azure.com/ml](http://azure.com/ml). Az alábbiakban van egy Képernyőkép a kísérlet, amely a webes szolgáltatás, és példa kód létre minden egyes belül modulok.
> 
> 

Azure Machine Learning belül egy új üres kísérlet létrehozásához és két [R-parancsfájl végrehajtása] [ execute-r-script] modulok lekért a munkaterület-kiszolgálóra. Az adatok séma, amely egy egyszerű készült [R-parancsfájl végrehajtása][execute-r-script]. Ezt követően az adatok séma újra létre a fürt modell szakaszban csatolva egy [R-parancsfájl végrehajtása][execute-r-script]. Az a [R-parancsfájl végrehajtása] [ execute-r-script] a fürt modellt használja, a webszolgáltatás majd használja a "k-közép" függvény, amely az előre elkészített a [R-parancsfájl végrehajtása] [ execute-r-script] az Azure Machine Learning.    

![Kísérlet folyamata][3]

#### <a name="module-1"></a>1. modul:
    #Enter the input data as a string 
    mydata <- data.frame(value = "1; 3; 5; 6; 7; 7, 5; 5; 6; 7; 2; 1, 3; 7; 2; 9; 56; 6, 1; 4; 5; 26; 4; 23, 15; 35; 6; 7; 12; 1, 32; 51; 62; 7; 21; 1", k=5, stringsAsFactors=FALSE)

    maml.mapOutputPort("mydata");     


#### <a name="module-2"></a>A modul 2:
    # Map 1-based optional input ports to variables
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

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("mydatafinal");


## <a name="limitations"></a>Korlátozások
Itt látható egy nagyon egyszerű példa egy csoportosítási webszolgáltatás. A fenti példa kódot is látható, mert nincs hiba alatt van megvalósítva, és a szolgáltatás azt feltételezi, hogy minden folyamatos változó (ahol nincs kategorikus funkció engedélyezett), a szolgáltatás csak bemenetek numerikus értékként ennek a webszolgáltatásnak létrehozásának időpontjában. Emellett a szolgáltatás jelenleg kezeli korlátozott adatok mérete, kellő a webszolgáltatás-hívások és azt a tényt, hogy a modell alatt alkalmas a webes szolgáltatás neve minden egyes kérelem/válasz jellegének. 

## <a name="faq"></a>GYIK
Gyakori kérdések a felhasználás a webszolgáltatás vagy az Azure piactéren közzétételt, lásd: [Itt](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-cluster-model/cluster-img1.png
[2]: ./media/machine-learning-r-csharp-cluster-model/cluster-img2.png
[3]: ./media/machine-learning-r-csharp-cluster-model/cluster-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

