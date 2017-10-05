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
# <a name="deprecated-cluster-model"></a><span data-ttu-id="9c4a5-103">(elavult) Fürt modell</span><span class="sxs-lookup"><span data-stu-id="9c4a5-103">(deprecated) Cluster Model</span></span>

> [!NOTE]
> <span data-ttu-id="9c4a5-104">A Microsoft DataMarket használatból van, és ez az API már elavult.</span><span class="sxs-lookup"><span data-stu-id="9c4a5-104">The Microsoft DataMarket is being retired and this API has been deprecated.</span></span> 
> 
> <span data-ttu-id="9c4a5-105">Sok hasznos példa kísérletek és API-k a [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="9c4a5-105">You can find many useful example experiments and APIs in the [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="9c4a5-106">A gyűjtemény kapcsolatos további információkért lásd: [megosztást, és felderítik a Cortana Intelligence Gallery erőforrások](machine-learning-gallery-how-to-use-contribute-publish.md).</span><span class="sxs-lookup"><span data-stu-id="9c4a5-106">For more information about the Gallery, see [Share and discover resources in the Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span></span>

<span data-ttu-id="9c4a5-107">Hogyan azt előrejelzése jóváírás cardholders viselkedésmódok csoportok hitelkártya kiállítók kell fizetni az indító kockázat csökkentése érdekében?</span><span class="sxs-lookup"><span data-stu-id="9c4a5-107">How can we predict groups of credit cardholders’ behaviors in order to reduce the charge-off risk of credit card issuers?</span></span> <span data-ttu-id="9c4a5-108">Hogyan lehet meghatároztuk személy jellemzők alkalmazottak csoportját a munkahelyi teljesítményének javítása érdekében?</span><span class="sxs-lookup"><span data-stu-id="9c4a5-108">How can we define groups of personality traits of employees in order to improve their performance at work?</span></span> <span data-ttu-id="9c4a5-109">Hogyan lehet sorolni orvosi betegek azok betegségek jellemzői alapuló?</span><span class="sxs-lookup"><span data-stu-id="9c4a5-109">How can doctors classify patients into groups based on the characteristics of their diseases?</span></span> <span data-ttu-id="9c4a5-110">Alapvetően összes ezeket a kérdéseket is válaszolni fürt elemzése révén.</span><span class="sxs-lookup"><span data-stu-id="9c4a5-110">In principle, all of these questions can be answered through cluster analysis.</span></span>   

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<span data-ttu-id="9c4a5-111">Fürt elemzés csoportokba két vagy több egymást kölcsönösen kizáró ismeretlen változók kombinációi megfigyelések készlete osztályozza.</span><span class="sxs-lookup"><span data-stu-id="9c4a5-111">Cluster analysis classifies a set of observations into two or more mutually exclusive unknown groups based on combinations of variables.</span></span> <span data-ttu-id="9c4a5-112">A fürt elemzés célja megfigyelések, általában személyek vagy jellemzőik, rendezése csoportokba rendszerének felderítését ahol a csoportok tagjainak tulajdonságok megosztása közös.</span><span class="sxs-lookup"><span data-stu-id="9c4a5-112">The purpose of cluster analysis is to discover a system of organizing observations, usually people or their characteristics, into groups, where members of the groups share properties in common.</span></span> <span data-ttu-id="9c4a5-113">Ez [szolgáltatás](https://datamarket.azure.com/dataset/aml_labs/k_cluster_model) használja a K-közép kiosztásának, gyakran használt csoportosítási módszer fürt tetszőleges adatok csoportokba.</span><span class="sxs-lookup"><span data-stu-id="9c4a5-113">This [service](https://datamarket.azure.com/dataset/aml_labs/k_cluster_model) uses the K-Means methodology, a commonly used clustering technique, to cluster arbitrary data into groups.</span></span> <span data-ttu-id="9c4a5-114">Ez a webszolgáltatás lekéri az adatokat, és bemenetként fürtök, és hozza létre az előrejelzéseket, amelyek a k tartalmazó csoportok minden megfigyelések k száma.</span><span class="sxs-lookup"><span data-stu-id="9c4a5-114">This web service takes the data and the number of k clusters as input, and produces predictions of which of the k groups to which each observations belongs.</span></span> 

> <span data-ttu-id="9c4a5-115">Ez a webszolgáltatás kell fenntartania – potenciálisan végig a mobilalkalmazások a webhelyen keresztül, vagy akár a helyi számítógépen, a felhasználókat például.</span><span class="sxs-lookup"><span data-stu-id="9c4a5-115">This web service could be consumed by users – potentially through a mobile app, through a website, or even on a local computer, for example.</span></span> <span data-ttu-id="9c4a5-116">De a webszolgáltatás célja is példa bemutatja, hogyan Azure Machine Learning webszolgáltatások fölött R-kód létrehozásához használható kiszolgálásához.</span><span class="sxs-lookup"><span data-stu-id="9c4a5-116">But the purpose of the web service is also to serve as an example of how Azure Machine Learning can be used to create web services on top of R code.</span></span> <span data-ttu-id="9c4a5-117">Az R-kód csupán néhány sornyi és az Azure Machine Learning Studio egy gombját kattint egy kísérlet hozható létre az R-kód és webszolgáltatásként közzé.</span><span class="sxs-lookup"><span data-stu-id="9c4a5-117">With just a few lines of R code and clicks of a button within Azure Machine Learning Studio, an experiment can be created with R code and published as a web service.</span></span> <span data-ttu-id="9c4a5-118">A webszolgáltatás majd közzé az Azure piactéren, és felhasználók és eszközök által felhasznált világszerte a szerző, a webszolgáltatás által infrastruktúra beállítás nélkül.</span><span class="sxs-lookup"><span data-stu-id="9c4a5-118">The web service can then be published to the Azure Marketplace and consumed by users and devices across the world with no infrastructure setup by the author of the web service.</span></span>  
> 
> 

## <a name="consumption-of-web-service"></a><span data-ttu-id="9c4a5-119">Felhasználási webszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="9c4a5-119">Consumption of web service</span></span>
<span data-ttu-id="9c4a5-120">Ez a webszolgáltatás az adatok csoportosítja k csoportokat, és kiírja a csoport-hozzárendelés minden egyes sorára.</span><span class="sxs-lookup"><span data-stu-id="9c4a5-120">This web service groups the data into a set of k groups and outputs the group assignment for each row.</span></span> <span data-ttu-id="9c4a5-121">A webszolgáltatás vár a felhasználó a bemeneti adatok karakterláncként, ahol sorok egymástól vesszővel (,) válassza el egymástól, és oszlopok egymástól pontosvesszővel (;).</span><span class="sxs-lookup"><span data-stu-id="9c4a5-121">The web service expects the end user to input data as a string where rows are separated by commas (,) and columns are separated by semicolons (;).</span></span> <span data-ttu-id="9c4a5-122">A webszolgáltatás 1 sor egyszerre vár.</span><span class="sxs-lookup"><span data-stu-id="9c4a5-122">The web service expects 1 row at a time.</span></span> <span data-ttu-id="9c4a5-123">Egy példa adatkészlet nézhet ki:</span><span class="sxs-lookup"><span data-stu-id="9c4a5-123">An example dataset could look like this:</span></span>

![Mintaadatok][1]

<span data-ttu-id="9c4a5-125">Tegyük fel, hogy a felhasználó kívánta csoportokba ezek az adatok 3 kölcsönösen kizárják egymást.</span><span class="sxs-lookup"><span data-stu-id="9c4a5-125">Suppose the user wanted to separate this data into 3 mutually exclusive groups.</span></span> <span data-ttu-id="9c4a5-126">Adjon meg, mert a fenti adatkészlet a következő adatokat: érték = "10; 5; 2,18; 1; 6,7; 5; 5,22; 3; 4,12; 2; 1,10; 3; 4"; k = "3".</span><span class="sxs-lookup"><span data-stu-id="9c4a5-126">The data input for the above dataset would be the following: value = “10;5;2,18;1;6,7;5;5,22;3;4,12;2;1,10;3;4”; k=”3”.</span></span> <span data-ttu-id="9c4a5-127">Az előre jelzett csoporttagság az egyes sorok eredménye.</span><span class="sxs-lookup"><span data-stu-id="9c4a5-127">The output is the predicted group membership for each of the rows.</span></span>

> <span data-ttu-id="9c4a5-128">Ez a szolgáltatás az Azure piactéren kihelyezett egy OData-szolgáltatás; a POST vagy GET módszerrel elnevezése lehet.</span><span class="sxs-lookup"><span data-stu-id="9c4a5-128">This service, as hosted on the Azure Marketplace, is an OData service; these may be called through POST or GET methods.</span></span> 
> 
> 

<span data-ttu-id="9c4a5-129">Többféleképpen is az automatizált módon a szolgáltatás fel (egy példa alkalmazás [Itt](http://microsoftazuremachinelearning.azurewebsites.net/ClusterModel.aspx)).</span><span class="sxs-lookup"><span data-stu-id="9c4a5-129">There are multiple ways of consuming the service in an automated fashion (an example app is [here](http://microsoftazuremachinelearning.azurewebsites.net/ClusterModel.aspx)).</span></span>

### <a name="starting-c-code-for-web-service-consumption"></a><span data-ttu-id="9c4a5-130">C#-kódban a webes szolgáltatások felhasználásához megkezdése:</span><span class="sxs-lookup"><span data-stu-id="9c4a5-130">Starting C# code for web service consumption:</span></span>
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




## <a name="creation-of-web-service"></a><span data-ttu-id="9c4a5-131">Webes szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="9c4a5-131">Creation of web service</span></span>
> <span data-ttu-id="9c4a5-132">Ez a webszolgáltatás Azure Machine Learning segítségével lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="9c4a5-132">This web service was created using Azure Machine Learning.</span></span> <span data-ttu-id="9c4a5-133">Az ingyenes próbaverzió, valamint a bevezető videó kísérletek létrehozásával és [közzétételi webes szolgáltatások](machine-learning-publish-a-machine-learning-web-service.md), lásd: [azure.com/ml](http://azure.com/ml).</span><span class="sxs-lookup"><span data-stu-id="9c4a5-133">For a free trial, as well as introductory videos on creating experiments and [publishing web services](machine-learning-publish-a-machine-learning-web-service.md), please see [azure.com/ml](http://azure.com/ml).</span></span> <span data-ttu-id="9c4a5-134">Az alábbiakban van egy Képernyőkép a kísérlet, amely a webes szolgáltatás, és példa kód létre minden egyes belül modulok.</span><span class="sxs-lookup"><span data-stu-id="9c4a5-134">Below is a screenshot of the experiment that created the web service and example code for each of the modules within the experiment.</span></span>
> 
> 

<span data-ttu-id="9c4a5-135">Azure Machine Learning belül egy új üres kísérlet létrehozásához és két [R-parancsfájl végrehajtása] [ execute-r-script] modulok lekért a munkaterület-kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="9c4a5-135">From within Azure Machine Learning, a new blank experiment was created and two [Execute R Script][execute-r-script] modules pulled onto the workspace.</span></span> <span data-ttu-id="9c4a5-136">Az adatok séma, amely egy egyszerű készült [R-parancsfájl végrehajtása][execute-r-script].</span><span class="sxs-lookup"><span data-stu-id="9c4a5-136">The data schema was created with a simple [Execute R Script][execute-r-script].</span></span> <span data-ttu-id="9c4a5-137">Ezt követően az adatok séma újra létre a fürt modell szakaszban csatolva egy [R-parancsfájl végrehajtása][execute-r-script].</span><span class="sxs-lookup"><span data-stu-id="9c4a5-137">Then, the data schema was linked to the cluster model section, again created with an [Execute R Script][execute-r-script].</span></span> <span data-ttu-id="9c4a5-138">Az a [R-parancsfájl végrehajtása] [ execute-r-script] a fürt modellt használja, a webszolgáltatás majd használja a "k-közép" függvény, amely az előre elkészített a [R-parancsfájl végrehajtása] [ execute-r-script] az Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="9c4a5-138">In the [Execute R Script][execute-r-script] used for the cluster model, the web service then utilizes the “k-means” function, which is prebuilt into the [Execute R Script][execute-r-script] of Azure Machine Learning.</span></span>    

![Kísérlet folyamata][3]

#### <a name="module-1"></a><span data-ttu-id="9c4a5-140">1. modul:</span><span class="sxs-lookup"><span data-stu-id="9c4a5-140">Module 1:</span></span>
    #Enter the input data as a string 
    mydata <- data.frame(value = "1; 3; 5; 6; 7; 7, 5; 5; 6; 7; 2; 1, 3; 7; 2; 9; 56; 6, 1; 4; 5; 26; 4; 23, 15; 35; 6; 7; 12; 1, 32; 51; 62; 7; 21; 1", k=5, stringsAsFactors=FALSE)

    maml.mapOutputPort("mydata");     


#### <a name="module-2"></a><span data-ttu-id="9c4a5-141">A modul 2:</span><span class="sxs-lookup"><span data-stu-id="9c4a5-141">Module 2:</span></span>
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


## <a name="limitations"></a><span data-ttu-id="9c4a5-142">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="9c4a5-142">Limitations</span></span>
<span data-ttu-id="9c4a5-143">Itt látható egy nagyon egyszerű példa egy csoportosítási webszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="9c4a5-143">This is a very simple example of a clustering web service.</span></span> <span data-ttu-id="9c4a5-144">A fenti példa kódot is látható, mert nincs hiba alatt van megvalósítva, és a szolgáltatás azt feltételezi, hogy minden folyamatos változó (ahol nincs kategorikus funkció engedélyezett), a szolgáltatás csak bemenetek numerikus értékként ennek a webszolgáltatásnak létrehozásának időpontjában.</span><span class="sxs-lookup"><span data-stu-id="9c4a5-144">As can be seen from the example code above, no error catching is implemented and the service assumes everything is a continuous variable (no categorical features allowed), as the service only inputs numeric values at the time of the creation of this web service.</span></span> <span data-ttu-id="9c4a5-145">Emellett a szolgáltatás jelenleg kezeli korlátozott adatok mérete, kellő a webszolgáltatás-hívások és azt a tényt, hogy a modell alatt alkalmas a webes szolgáltatás neve minden egyes kérelem/válasz jellegének.</span><span class="sxs-lookup"><span data-stu-id="9c4a5-145">Also, the service currently handles limited data size, due to the request/response nature of the web service call and the fact that the model is being fit every time the web service is called.</span></span> 

## <a name="faq"></a><span data-ttu-id="9c4a5-146">GYIK</span><span class="sxs-lookup"><span data-stu-id="9c4a5-146">FAQ</span></span>
<span data-ttu-id="9c4a5-147">Gyakori kérdések a felhasználás a webszolgáltatás vagy az Azure piactéren közzétételt, lásd: [Itt](machine-learning-marketplace-faq.md).</span><span class="sxs-lookup"><span data-stu-id="9c4a5-147">For frequently asked questions on consumption of the web service or publishing to the Azure Marketplace, see [here](machine-learning-marketplace-faq.md).</span></span>

[1]: ./media/machine-learning-r-csharp-cluster-model/cluster-img1.png
[2]: ./media/machine-learning-r-csharp-cluster-model/cluster-img2.png
[3]: ./media/machine-learning-r-csharp-cluster-model/cluster-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

