---
title: "Gyakori kérdések – aaa(deprecated) közzétételéhez és használatához a Machine Learning-alkalmazások az Azure piactérről |} Microsoft Docs"
description: "(elavult) Gépi tanulás alkalmazások közzétételi hello Azure piactér kapcsolatos gyakori kérdések"
services: machine-learning
documentationcenter: 
author: bharaths
manager: jhubbard
editor: cgronlun
ms.assetid: 26b3a1e7-8b9a-4004-98bc-17456d4c25e8
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: bharaths
ROBOTS: NOINDEX
redirect_url: https://gallery.cortanaintelligence.com/
redirect_document_id: True
ms.openlocfilehash: b3ae45dfff211fe9baccaf54faaf360a8309c780
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deprecated-publishing-and-using-machine-learning-apps-in-hello-azure-marketplace-faq"></a><span data-ttu-id="62b9b-103">(elavult) Közzététel és a Machine Learning-alkalmazások használata az Azure piactér hello: gyakran ismételt kérdések</span><span class="sxs-lookup"><span data-stu-id="62b9b-103">(deprecated) Publishing and using Machine Learning apps in hello Azure Marketplace: FAQ</span></span>

> [!NOTE]
> <span data-ttu-id="62b9b-104">DataMarket és Data Services rendszer hatókörről, és a meglévő előfizetések rendszerből, és elindítása megszakítva 3/31/2017.</span><span class="sxs-lookup"><span data-stu-id="62b9b-104">DataMarket and Data Services are being retired, and existing subscriptions will be retired and cancelled starting 3/31/2017.</span></span> <span data-ttu-id="62b9b-105">Ennek köszönhetően ez a cikk is elavult.</span><span class="sxs-lookup"><span data-stu-id="62b9b-105">As a result, this article is being deprecated.</span></span> 
> 
> <span data-ttu-id="62b9b-106">Másik megoldásként, közzéteheti a Machine Learning kísérleteket toohello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/) hello adatok tudományos Közösség hello juttatásra.</span><span class="sxs-lookup"><span data-stu-id="62b9b-106">As an alternative, you can publish your Machine Learning experiments toohello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/) for hello benefit of hello data science community.</span></span> <span data-ttu-id="62b9b-107">További információkért lásd: [megosztás és a Cortana Intelligence Gallery hello erőforrások felderítéséhez](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-gallery-how-to-use-contribute-publish).</span><span class="sxs-lookup"><span data-stu-id="62b9b-107">For more information, see [Share and discover resources in hello Cortana Intelligence Gallery](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-gallery-how-to-use-contribute-publish).</span></span>


## <a name="questions-about-consuming-from-marketplace"></a><span data-ttu-id="62b9b-108">Piactérről fel kapcsolatos kérdések</span><span class="sxs-lookup"><span data-stu-id="62b9b-108">Questions about consuming from Marketplace</span></span>
<span data-ttu-id="62b9b-109">**1. Miért kapok hello bemeneti hello webszolgáltatáshoz beírása után a következő hibaüzenet jelenik meg:**</span><span class="sxs-lookup"><span data-stu-id="62b9b-109">**1. Why do I get hello following error message after I enter input for hello web service:**</span></span>

<span data-ttu-id="62b9b-110">**hello kérés egy háttér-időtúllépés vagy a háttér-hibát eredményezett. hello team hello problémát vizsgál. Hello kellemetlenségért elnézését. (500)**</span><span class="sxs-lookup"><span data-stu-id="62b9b-110">**hello request resulted in a back-end time out or back-end error. hello team is investigating hello issue. We are sorry for hello inconvenience. (500)**</span></span>

<span data-ttu-id="62b9b-111">A bemeneti paraméterek nem felelnek meg az adott webszolgáltatás hello toohello formátum.</span><span class="sxs-lookup"><span data-stu-id="62b9b-111">Your input parameter(s) may not conform toohello required format for hello specific web service.</span></span> <span data-ttu-id="62b9b-112">A bemeneti paraméterek és a webszolgáltatás hello korlátozásai tekintse meg a toohello megfelelő dokumentáció hivatkozás toofind hello formátuma helytelen.</span><span class="sxs-lookup"><span data-stu-id="62b9b-112">Please refer toohello corresponding documentation link toofind hello correct format for input parameters and hello limitations of this web service.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<span data-ttu-id="62b9b-113">**2. Látható hello "Intéző ehhez az adatkészlethez" lapot, és illessze be egy másik böngészőablakban, milyen hitelesítő adatokat kell I tooaccess hello eredmények használja, és hogyan hello webszolgáltatáshoz hello API-hivatkozás másolása látható őket?**</span><span class="sxs-lookup"><span data-stu-id="62b9b-113">**2. If I copy hello API link for hello web service that I see on hello "Explore this dataset" page and paste it into another browser window, what credentials should I use tooaccess hello results, and how do I see them?**</span></span>

<span data-ttu-id="62b9b-114">Piactér-fiókját hello felhasználónév és a hello elsődleges fiókkulcs hello jelszó célszerű használni.</span><span class="sxs-lookup"><span data-stu-id="62b9b-114">You should use your Marketplace account as hello username and hello primary account key as hello password.</span></span> <span data-ttu-id="62b9b-115">hello elsődleges fiókkulcs hello található **ehhez az adatkészlethez felfedezés** lapján hello webszolgáltatás hello leírása (hello kattintson **megjelenítése** gombra).</span><span class="sxs-lookup"><span data-stu-id="62b9b-115">hello primary account key can be found on hello **Explore this dataset** page under hello description of hello web service (click hello **show** button).</span></span> <span data-ttu-id="62b9b-116">hello eredménye lehet, hogy hello böngészőben megjelenő, vagy elérhető, vagy túl letöltési, attól függően, hogy mely böngészőt használ.</span><span class="sxs-lookup"><span data-stu-id="62b9b-116">hello result may display in hello browser or it may be available too download, depending on which browser you are using.</span></span>

<span data-ttu-id="62b9b-117">**3. Miért kapok hello hello bemeneti hello webszolgáltatáshoz hello "Intéző ehhez az adatkészlethez" lapon beírása után a következő hibaüzenet jelenik meg:**</span><span class="sxs-lookup"><span data-stu-id="62b9b-117">**3. Why do I get hello following error message after I enter hello input for hello web service on hello "Explore this dataset" page:**</span></span> 

<span data-ttu-id="62b9b-118">**Váratlan hiba történt a kérés feldolgozása közben. Próbálkozzon újra.**</span><span class="sxs-lookup"><span data-stu-id="62b9b-118">**An unexpected error occurred while processing your request. Please try again.**</span></span>

<span data-ttu-id="62b9b-119">Egy vagy több bemeneti paraméterek, a webszolgáltatás túllépte hello korlát amikor hello webszolgáltatás hello piactéren fel **ehhez az adatkészlethez felfedezés** lap.</span><span class="sxs-lookup"><span data-stu-id="62b9b-119">One or more input parameters of your web service may have exceeded hello length limit when consuming hello web service on hello marketplace **Explore this dataset** page.</span></span> <span data-ttu-id="62b9b-120">hello szolgáltatás már bemeneti adat hosszánál HTTP POST metódusok használatával hívható.</span><span class="sxs-lookup"><span data-stu-id="62b9b-120">hello services can be called with a longer input length by using HTTP POST methods.</span></span> <span data-ttu-id="62b9b-121">Tekintse meg a [R használata a Machine Learning és a közzétett tooMarketplace megoldások minta](machine-learning-r-csharp-web-service-examples.md).</span><span class="sxs-lookup"><span data-stu-id="62b9b-121">For examples, see [Sample solutions using R on Machine Learning and published tooMarketplace](machine-learning-r-csharp-web-service-examples.md).</span></span>

<span data-ttu-id="62b9b-122">**4. Miért nem látom semmit hello "API EXPLORER" lapon int hello tároló a klasszikus Azure portál hello?**</span><span class="sxs-lookup"><span data-stu-id="62b9b-122">**4. Why do I not see anything in hello "API EXPLORER" tab int hello Store in hello Azure Classic Portal?**</span></span> 

<span data-ttu-id="62b9b-123">Ez az egy ismert probléma az Azure klasszikus portál piactér hello.</span><span class="sxs-lookup"><span data-stu-id="62b9b-123">This is a known issue with hello Azure Classic Portal Marketplace.</span></span> <span data-ttu-id="62b9b-124">hello csoport dolgozik tooresolve probléma.</span><span class="sxs-lookup"><span data-stu-id="62b9b-124">hello team is working tooresolve this issue.</span></span> 

## <a name="questions-about-publishing-from-azure-machine-learning-on-marketplace"></a><span data-ttu-id="62b9b-125">Azure Machine Learning piactéren történő közzétételre vonatkozó kérdések</span><span class="sxs-lookup"><span data-stu-id="62b9b-125">Questions about publishing from Azure Machine Learning on Marketplace</span></span>
<span data-ttu-id="62b9b-126">**1. Miért van a tranzakciók emblémák vagy képek nem frissíti a webszolgáltatáshoz?**</span><span class="sxs-lookup"><span data-stu-id="62b9b-126">**1. Why are my transactions of logos or images not refreshing for my web service?**</span></span> 

<span data-ttu-id="62b9b-127">Emblémák és lemezképek gyorsítótárba kerüljenek-e hello közzétételi portálon, és új embléma hello vagy kép tooupdate hello portál too10 nap igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="62b9b-127">Logos and images are cached in hello publishing portal, and it may take up too10 days for hello new logo or image tooupdate on hello portal.</span></span>

<span data-ttu-id="62b9b-128">**2. Miért van hello "Részletek" lap piactér hibaüzenetet jelenít meg a webszolgáltatás?**</span><span class="sxs-lookup"><span data-stu-id="62b9b-128">**2. Why is hello “Detail" tab of my web service on Marketplace showing an error message?**</span></span>

<span data-ttu-id="62b9b-129">Egy ismert probléma piactér van tooAzure Machine Learning szolgáltatás részletei a csatlakozáskor.</span><span class="sxs-lookup"><span data-stu-id="62b9b-129">There is a known Marketplace issue when connecting tooAzure Machine Learning for service details.</span></span> <span data-ttu-id="62b9b-130">hello csoport dolgozik tooresolve probléma.</span><span class="sxs-lookup"><span data-stu-id="62b9b-130">hello team is working tooresolve this issue.</span></span>

<span data-ttu-id="62b9b-131">**3. Miért hello R mintakód hello Azure Machine Learning web Services esetében nem működik a piactéren fogyasztó hello webszolgáltatások?**</span><span class="sxs-lookup"><span data-stu-id="62b9b-131">**3. Why does hello R sample code in hello Azure Machine Learning web services not work for consuming hello web services in Marketplace?**</span></span>

<span data-ttu-id="62b9b-132">hello hitelesítési rendszereket különböző tooconnecting toothese webszolgáltatások hello piactéren keresztül közvetlenül csatlakozó tooAzure Machine Learning webszolgáltatások képest.</span><span class="sxs-lookup"><span data-stu-id="62b9b-132">hello authentication systems are different when connecting tooAzure Machine Learning web services directly compared tooconnecting toothese web services through hello Marketplace.</span></span> <span data-ttu-id="62b9b-133">a piactéren hello szolgáltatások OData-szolgáltatásaival, és a GET vagy POST metódusok hívható.</span><span class="sxs-lookup"><span data-stu-id="62b9b-133">hello services in Marketplace are OData services, and they can be called with GET or POST methods.</span></span> 

<span data-ttu-id="62b9b-134">**4. Miért van hello támogatási hivatkozások a webszolgáltatás nem frissül megfelelően a ajánlatok részénél kínál?**</span><span class="sxs-lookup"><span data-stu-id="62b9b-134">**4. Why are hello support links of my web service offers not updating correctly for some of my offers?**</span></span>

<span data-ttu-id="62b9b-135">hello támogatási hivatkozások / publisher, nem az ajánlat / globálisak.</span><span class="sxs-lookup"><span data-stu-id="62b9b-135">hello support links are global per publisher, not per offer.</span></span> 

<span data-ttu-id="62b9b-136">**5. Hogyan tehetők közzé egy webszolgáltatás-bővítmény bemeneti kötegelt módban a piactéren?**</span><span class="sxs-lookup"><span data-stu-id="62b9b-136">**5. How do I publish a web service with batch input mode in Marketplace?**</span></span>

<span data-ttu-id="62b9b-137">hello kötegelt bemeneti mód jelenleg nem támogatott a piactér webszolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="62b9b-137">hello batch input mode is currently not supported in Marketplace web services.</span></span>

<span data-ttu-id="62b9b-138">**6. Ki lehet kapcsolatba lépni tooget súgó Ha kérdéseik vannak a közzétevő váljon, vagy ha közzététele során problémába ütközik??**</span><span class="sxs-lookup"><span data-stu-id="62b9b-138">**6. Who should I contact tooget help if I have questions about becoming a data publisher, or if I have issues during publishing?**</span></span>

<span data-ttu-id="62b9b-139">Lépjen kapcsolatba a hello Azure piactér csapatának < mailto:datamarketbd@microsoft.com > további információt.</span><span class="sxs-lookup"><span data-stu-id="62b9b-139">Please contact hello Azure Marketplace team at <mailto:datamarketbd@microsoft.com> for more information.</span></span>

