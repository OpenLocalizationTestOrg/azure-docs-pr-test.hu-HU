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
# <a name="deprecated-cluster-model"></a><span data-ttu-id="8d396-103">(elavult) Fürt modell</span><span class="sxs-lookup"><span data-stu-id="8d396-103">(deprecated) Cluster Model</span></span>

> [!NOTE]
> <span data-ttu-id="8d396-104">a Microsoft DataMarket hello használatból van, és ez az API már elavult.</span><span class="sxs-lookup"><span data-stu-id="8d396-104">hello Microsoft DataMarket is being retired and this API has been deprecated.</span></span> 
> 
> <span data-ttu-id="8d396-105">Sok hasznos példa kísérletek és API-kat az található hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="8d396-105">You can find many useful example experiments and APIs in hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="8d396-106">Gyűjteményelem hello kapcsolatos további információkért lásd: [megosztás és a Cortana Intelligence Gallery hello erőforrások felderítéséhez](machine-learning-gallery-how-to-use-contribute-publish.md).</span><span class="sxs-lookup"><span data-stu-id="8d396-106">For more information about hello Gallery, see [Share and discover resources in hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span></span>

<span data-ttu-id="8d396-107">Hogyan azt előrejelzése jóváírás cardholders viselkedésmódok rendelés tooreduce hello kell fizetni az indító kockázatot hitelkártya kiállítók csoportok?</span><span class="sxs-lookup"><span data-stu-id="8d396-107">How can we predict groups of credit cardholders’ behaviors in order tooreduce hello charge-off risk of credit card issuers?</span></span> <span data-ttu-id="8d396-108">Hogyan azt megadására az alkalmazottak személy jellemzők csoportját a rendelés tooimprove munkahelyi teljesítményének?</span><span class="sxs-lookup"><span data-stu-id="8d396-108">How can we define groups of personality traits of employees in order tooimprove their performance at work?</span></span> <span data-ttu-id="8d396-109">Hogyan lehet sorolni orvosi betegek hello jellemzői a betegségek alapuló?</span><span class="sxs-lookup"><span data-stu-id="8d396-109">How can doctors classify patients into groups based on hello characteristics of their diseases?</span></span> <span data-ttu-id="8d396-110">Alapvetően összes ezeket a kérdéseket is válaszolni fürt elemzése révén.</span><span class="sxs-lookup"><span data-stu-id="8d396-110">In principle, all of these questions can be answered through cluster analysis.</span></span>   

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<span data-ttu-id="8d396-111">Fürt elemzés csoportokba két vagy több egymást kölcsönösen kizáró ismeretlen változók kombinációi megfigyelések készlete osztályozza.</span><span class="sxs-lookup"><span data-stu-id="8d396-111">Cluster analysis classifies a set of observations into two or more mutually exclusive unknown groups based on combinations of variables.</span></span> <span data-ttu-id="8d396-112">hello fürt elemzés célja toodiscover rendszert megfigyelések, általában személyek vagy jellemzőik, rendezése csoportokba, ahol hello csoportok tagjai tulajdonságok megosztása közös.</span><span class="sxs-lookup"><span data-stu-id="8d396-112">hello purpose of cluster analysis is toodiscover a system of organizing observations, usually people or their characteristics, into groups, where members of hello groups share properties in common.</span></span> <span data-ttu-id="8d396-113">Ez [szolgáltatás](https://datamarket.azure.com/dataset/aml_labs/k_cluster_model) használ hello K-közép módszertan használatával, a gyakran használt csoportosítási módszer toocluster tetszőleges adatok csoportokba.</span><span class="sxs-lookup"><span data-stu-id="8d396-113">This [service](https://datamarket.azure.com/dataset/aml_labs/k_cluster_model) uses hello K-Means methodology, a commonly used clustering technique, toocluster arbitrary data into groups.</span></span> <span data-ttu-id="8d396-114">Ez a webszolgáltatás hello adatokat fogad, és bemenetként fürtök, és hozza létre az előrejelzéseket, amelyek hello k csoportok toowhich minden megfigyelések tartozik k hello száma.</span><span class="sxs-lookup"><span data-stu-id="8d396-114">This web service takes hello data and hello number of k clusters as input, and produces predictions of which of hello k groups toowhich each observations belongs.</span></span> 

> <span data-ttu-id="8d396-115">Ez a webszolgáltatás kell fenntartania – potenciálisan végig a mobilalkalmazások a webhelyen keresztül, vagy akár a helyi számítógépen, a felhasználókat például.</span><span class="sxs-lookup"><span data-stu-id="8d396-115">This web service could be consumed by users – potentially through a mobile app, through a website, or even on a local computer, for example.</span></span> <span data-ttu-id="8d396-116">De hello hello webszolgáltatás célja is tooserve példa bemutatja, hogyan Azure Machine Learning webszolgáltatások használt toocreate fölött R-kód is lehet.</span><span class="sxs-lookup"><span data-stu-id="8d396-116">But hello purpose of hello web service is also tooserve as an example of how Azure Machine Learning can be used toocreate web services on top of R code.</span></span> <span data-ttu-id="8d396-117">Az R-kód csupán néhány sornyi és az Azure Machine Learning Studio egy gombját kattint egy kísérlet hozható létre az R-kód és webszolgáltatásként közzé.</span><span class="sxs-lookup"><span data-stu-id="8d396-117">With just a few lines of R code and clicks of a button within Azure Machine Learning Studio, an experiment can be created with R code and published as a web service.</span></span> <span data-ttu-id="8d396-118">hello webszolgáltatás majd lehet közzétett toohello Azure piactéren, és a felhasználók és eszközök által felhasznált között hello world hello Szerző hello webszolgáltatás infrastruktúra beállítás nélkül.</span><span class="sxs-lookup"><span data-stu-id="8d396-118">hello web service can then be published toohello Azure Marketplace and consumed by users and devices across hello world with no infrastructure setup by hello author of hello web service.</span></span>  
> 
> 

## <a name="consumption-of-web-service"></a><span data-ttu-id="8d396-119">Felhasználási webszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="8d396-119">Consumption of web service</span></span>
<span data-ttu-id="8d396-120">Ez a webszolgáltatás hello adatok csoportosítja k csoportok és -kimenetek hello csoport-hozzárendelés minden egyes sorára készlete.</span><span class="sxs-lookup"><span data-stu-id="8d396-120">This web service groups hello data into a set of k groups and outputs hello group assignment for each row.</span></span> <span data-ttu-id="8d396-121">hello webszolgáltatás hello végfelhasználói tooinput adatok vár, ahol sorok egymástól vesszővel (,) válassza el egymástól, és oszlopok pontosvesszővel (;) elválasztott karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="8d396-121">hello web service expects hello end user tooinput data as a string where rows are separated by commas (,) and columns are separated by semicolons (;).</span></span> <span data-ttu-id="8d396-122">hello webszolgáltatás 1 sor egyszerre vár.</span><span class="sxs-lookup"><span data-stu-id="8d396-122">hello web service expects 1 row at a time.</span></span> <span data-ttu-id="8d396-123">Egy példa adatkészlet nézhet ki:</span><span class="sxs-lookup"><span data-stu-id="8d396-123">An example dataset could look like this:</span></span>

![Mintaadatok][1]

<span data-ttu-id="8d396-125">Tegyük fel, hogy hello kívánta felhasználói tooseparate ezek az adatok 3 egymást kölcsönösen kizáró csoportokba.</span><span class="sxs-lookup"><span data-stu-id="8d396-125">Suppose hello user wanted tooseparate this data into 3 mutually exclusive groups.</span></span> <span data-ttu-id="8d396-126">a fenti dataset hello hello következő bemeneti adatokat hello: érték = "10; 5; 2,18; 1; 6,7; 5; 5,22; 3; 4,12; 2; 1,10; 3; 4"; k = "3".</span><span class="sxs-lookup"><span data-stu-id="8d396-126">hello data input for hello above dataset would be hello following: value = “10;5;2,18;1;6,7;5;5,22;3;4,12;2;1,10;3;4”; k=”3”.</span></span> <span data-ttu-id="8d396-127">hello kimeneti van hello előre jelzett csoporttagság az egyes hello sorokat.</span><span class="sxs-lookup"><span data-stu-id="8d396-127">hello output is hello predicted group membership for each of hello rows.</span></span>

> <span data-ttu-id="8d396-128">Ez a szolgáltatás az Azure piactér hello kihelyezett egy OData-szolgáltatás; a POST vagy GET módszerrel elnevezése lehet.</span><span class="sxs-lookup"><span data-stu-id="8d396-128">This service, as hosted on hello Azure Marketplace, is an OData service; these may be called through POST or GET methods.</span></span> 
> 
> 

<span data-ttu-id="8d396-129">Többféleképpen is az automatizált módon hello szolgáltatás fel (egy példa alkalmazás [Itt](http://microsoftazuremachinelearning.azurewebsites.net/ClusterModel.aspx)).</span><span class="sxs-lookup"><span data-stu-id="8d396-129">There are multiple ways of consuming hello service in an automated fashion (an example app is [here](http://microsoftazuremachinelearning.azurewebsites.net/ClusterModel.aspx)).</span></span>

### <a name="starting-c-code-for-web-service-consumption"></a><span data-ttu-id="8d396-130">C#-kódban a webes szolgáltatások felhasználásához megkezdése:</span><span class="sxs-lookup"><span data-stu-id="8d396-130">Starting C# code for web service consumption:</span></span>
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




## <a name="creation-of-web-service"></a><span data-ttu-id="8d396-131">Webes szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="8d396-131">Creation of web service</span></span>
> <span data-ttu-id="8d396-132">Ez a webszolgáltatás Azure Machine Learning segítségével lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="8d396-132">This web service was created using Azure Machine Learning.</span></span> <span data-ttu-id="8d396-133">Az ingyenes próbaverzió, valamint a bevezető videó kísérletek létrehozásával és [közzétételi webes szolgáltatások](machine-learning-publish-a-machine-learning-web-service.md), lásd: [azure.com/ml](http://azure.com/ml).</span><span class="sxs-lookup"><span data-stu-id="8d396-133">For a free trial, as well as introductory videos on creating experiments and [publishing web services](machine-learning-publish-a-machine-learning-web-service.md), please see [azure.com/ml](http://azure.com/ml).</span></span> <span data-ttu-id="8d396-134">Az alábbiakban van egy képernyőfelvétel a hello webes szolgáltatás, és példa kódot az egyes hello modulok hello kísérlet belül létrehozott hello kísérlet.</span><span class="sxs-lookup"><span data-stu-id="8d396-134">Below is a screenshot of hello experiment that created hello web service and example code for each of hello modules within hello experiment.</span></span>
> 
> 

<span data-ttu-id="8d396-135">Azure Machine Learning belül egy új üres kísérlet létrehozásához és két [R-parancsfájl végrehajtása] [ execute-r-script] modulok lekért hello munkaterület-kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="8d396-135">From within Azure Machine Learning, a new blank experiment was created and two [Execute R Script][execute-r-script] modules pulled onto hello workspace.</span></span> <span data-ttu-id="8d396-136">hello adatkulcsokat létrejött egy egyszerű [R-parancsfájl végrehajtása][execute-r-script].</span><span class="sxs-lookup"><span data-stu-id="8d396-136">hello data schema was created with a simple [Execute R Script][execute-r-script].</span></span> <span data-ttu-id="8d396-137">Ezt követően hello adatkulcsokat lett csatolt toohello modell szakaszban, újra létre a fürt egy [R-parancsfájl végrehajtása][execute-r-script].</span><span class="sxs-lookup"><span data-stu-id="8d396-137">Then, hello data schema was linked toohello cluster model section, again created with an [Execute R Script][execute-r-script].</span></span> <span data-ttu-id="8d396-138">A hello [R-parancsfájl végrehajtása] [ execute-r-script] hello fürt modellt használja, hello webszolgáltatás majd használja az "k-közép" hello függvény, amely hello az előre elkészített [R-parancsfájl végrehajtása] [ execute-r-script] az Azure gépi tanulás.</span><span class="sxs-lookup"><span data-stu-id="8d396-138">In hello [Execute R Script][execute-r-script] used for hello cluster model, hello web service then utilizes hello “k-means” function, which is prebuilt into hello [Execute R Script][execute-r-script] of Azure Machine Learning.</span></span>    

![Kísérlet folyamata][3]

#### <a name="module-1"></a><span data-ttu-id="8d396-140">1. modul:</span><span class="sxs-lookup"><span data-stu-id="8d396-140">Module 1:</span></span>
    #Enter hello input data as a string 
    mydata <- data.frame(value = "1; 3; 5; 6; 7; 7, 5; 5; 6; 7; 2; 1, 3; 7; 2; 9; 56; 6, 1; 4; 5; 26; 4; 23, 15; 35; 6; 7; 12; 1, 32; 51; 62; 7; 21; 1", k=5, stringsAsFactors=FALSE)

    maml.mapOutputPort("mydata");     


#### <a name="module-2"></a><span data-ttu-id="8d396-141">A modul 2:</span><span class="sxs-lookup"><span data-stu-id="8d396-141">Module 2:</span></span>
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


## <a name="limitations"></a><span data-ttu-id="8d396-142">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="8d396-142">Limitations</span></span>
<span data-ttu-id="8d396-143">Itt látható egy nagyon egyszerű példa egy csoportosítási webszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="8d396-143">This is a very simple example of a clustering web service.</span></span> <span data-ttu-id="8d396-144">Hello példakódot fent is látható, mert nincs hiba alatt van megvalósítva és hello szolgáltatás feltételezi, hogy minden rendben (ahol nincs kategorikus funkció engedélyezett), a folyamatos változó hello szolgáltatást csak bemenetek numerikus értékként hello létrehozását webhely hello helyreállításkor a szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="8d396-144">As can be seen from hello example code above, no error catching is implemented and hello service assumes everything is a continuous variable (no categorical features allowed), as hello service only inputs numeric values at hello time of hello creation of this web service.</span></span> <span data-ttu-id="8d396-145">Is hello szolgáltatás jelenleg korlátozott adatméret kezeli, miatt toohello kérelem-válasz jellegű hello webszolgáltatás hívás és hello tényt, hogy a modell hello alatt alkalmas minden alkalommal, amikor hello webszolgáltatás nevezik.</span><span class="sxs-lookup"><span data-stu-id="8d396-145">Also, hello service currently handles limited data size, due toohello request/response nature of hello web service call and hello fact that hello model is being fit every time hello web service is called.</span></span> 

## <a name="faq"></a><span data-ttu-id="8d396-146">GYIK</span><span class="sxs-lookup"><span data-stu-id="8d396-146">FAQ</span></span>
<span data-ttu-id="8d396-147">Gyakori kérdések a felhasználás hello webszolgáltatás vagy az Azure piactér közzétételi toohello, lásd: [Itt](machine-learning-marketplace-faq.md).</span><span class="sxs-lookup"><span data-stu-id="8d396-147">For frequently asked questions on consumption of hello web service or publishing toohello Azure Marketplace, see [here](machine-learning-marketplace-faq.md).</span></span>

[1]: ./media/machine-learning-r-csharp-cluster-model/cluster-img1.png
[2]: ./media/machine-learning-r-csharp-cluster-model/cluster-img2.png
[3]: ./media/machine-learning-r-csharp-cluster-model/cluster-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

