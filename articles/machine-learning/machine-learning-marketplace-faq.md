---
title: "(elavult) Gyakori kérdések – közzétételéhez és használatához a Machine Learning-alkalmazások az Azure piactérről |} Microsoft Docs"
description: "(elavult) Közzétételi Machine Learning-alkalmazások az Azure piactéren kapcsolatos gyakori kérdések"
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
redirect_document_id: TRUE
ms.openlocfilehash: a4631dfeb2f817b3a3c612a8f6af48398e4f2ab9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="deprecated-publishing-and-using-machine-learning-apps-in-the-azure-marketplace-faq"></a><span data-ttu-id="25dd7-103">(elavult) Kiadása és használata a Machine Learning-alkalmazások az Azure piactéren: gyakran ismételt kérdések</span><span class="sxs-lookup"><span data-stu-id="25dd7-103">(deprecated) Publishing and using Machine Learning apps in the Azure Marketplace: FAQ</span></span>

> [!NOTE]
> <span data-ttu-id="25dd7-104">DataMarket és Data Services rendszer hatókörről, és a meglévő előfizetések rendszerből, és elindítása megszakítva 3/31/2017.</span><span class="sxs-lookup"><span data-stu-id="25dd7-104">DataMarket and Data Services are being retired, and existing subscriptions will be retired and cancelled starting 3/31/2017.</span></span> <span data-ttu-id="25dd7-105">Ennek köszönhetően ez a cikk is elavult.</span><span class="sxs-lookup"><span data-stu-id="25dd7-105">As a result, this article is being deprecated.</span></span> 
> 
> <span data-ttu-id="25dd7-106">Alternatív megoldásként közzéteheti a Machine Learning kísérleteket, hogy a [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/) az adatok tudományos közösségi érdekében.</span><span class="sxs-lookup"><span data-stu-id="25dd7-106">As an alternative, you can publish your Machine Learning experiments to the [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/) for the benefit of the data science community.</span></span> <span data-ttu-id="25dd7-107">További információkért lásd: [megosztást, és felderítik a Cortana Intelligence Gallery erőforrások](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-gallery-how-to-use-contribute-publish).</span><span class="sxs-lookup"><span data-stu-id="25dd7-107">For more information, see [Share and discover resources in the Cortana Intelligence Gallery](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-gallery-how-to-use-contribute-publish).</span></span>


## <a name="questions-about-consuming-from-marketplace"></a><span data-ttu-id="25dd7-108">Piactérről fel kapcsolatos kérdések</span><span class="sxs-lookup"><span data-stu-id="25dd7-108">Questions about consuming from Marketplace</span></span>
<span data-ttu-id="25dd7-109">**1. Miért kapok a következő hibaüzenet jelenik meg a webszolgáltatás bemeneti beírása után:**</span><span class="sxs-lookup"><span data-stu-id="25dd7-109">**1. Why do I get the following error message after I enter input for the web service:**</span></span>

<span data-ttu-id="25dd7-110">**A kérés egy háttér-időtúllépés vagy a háttér-hibát eredményezett. A csapata vizsgálja a problémát. Elnézését kérjük a kellemetlenségért. (500)**</span><span class="sxs-lookup"><span data-stu-id="25dd7-110">**The request resulted in a back-end time out or back-end error. The team is investigating the issue. We are sorry for the inconvenience. (500)**</span></span>

<span data-ttu-id="25dd7-111">A bemeneti paraméterek nem felelnek meg az adott webszolgáltatás megkívánt formátumban.</span><span class="sxs-lookup"><span data-stu-id="25dd7-111">Your input parameter(s) may not conform to the required format for the specific web service.</span></span> <span data-ttu-id="25dd7-112">Tekintse meg a megfelelő dokumentáció-hivatkozás található a bemeneti paraméterekben megfelelő formátum és a webszolgáltatás vonatkozó korlátozások.</span><span class="sxs-lookup"><span data-stu-id="25dd7-112">Please refer to the corresponding documentation link to find the correct format for input parameters and the limitations of this web service.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<span data-ttu-id="25dd7-113">**2. Ha a webszolgáltatás, amelyet a "Intéző ehhez az adatkészlethez", és illessze be azt egy másik böngészőben ablakba, milyen hitelesítő adatokat kell látható API hivatkozásra másolása az eredmények eléréséhez használni, és hogyan tekinthető meg őket?**</span><span class="sxs-lookup"><span data-stu-id="25dd7-113">**2. If I copy the API link for the web service that I see on the "Explore this dataset" page and paste it into another browser window, what credentials should I use to access the results, and how do I see them?**</span></span>

<span data-ttu-id="25dd7-114">A jelszót a piactér fiók célszerű használni a felhasználónév és az elsődleges kulcsa.</span><span class="sxs-lookup"><span data-stu-id="25dd7-114">You should use your Marketplace account as the username and the primary account key as the password.</span></span> <span data-ttu-id="25dd7-115">Az elsődleges kulcsa megtalálható a **ehhez az adatkészlethez felfedezés** a webszolgáltatás leírását a lap (kattintson a **megjelenítése** gombra).</span><span class="sxs-lookup"><span data-stu-id="25dd7-115">The primary account key can be found on the **Explore this dataset** page under the description of the web service (click the **show** button).</span></span> <span data-ttu-id="25dd7-116">Az eredmény jeleníthet meg a böngészőben vagy elképzelhető, hogy töltse le, a böngésző használatával.</span><span class="sxs-lookup"><span data-stu-id="25dd7-116">The result may display in the browser or it may be available to  download, depending on which browser you are using.</span></span>

<span data-ttu-id="25dd7-117">**3. Miért jelenik meg hibaüzenet, a bemeneti a webszolgáltatás az "Ez az adatkészlet böngészés" oldalon beírása után:**</span><span class="sxs-lookup"><span data-stu-id="25dd7-117">**3. Why do I get the following error message after I enter the input for the web service on the "Explore this dataset" page:**</span></span> 

<span data-ttu-id="25dd7-118">**Váratlan hiba történt a kérés feldolgozása közben. Próbálkozzon újra.**</span><span class="sxs-lookup"><span data-stu-id="25dd7-118">**An unexpected error occurred while processing your request. Please try again.**</span></span>

<span data-ttu-id="25dd7-119">Egy vagy több bemeneti paraméterek, a webszolgáltatás túllépte a maximális hossz amikor fel a webszolgáltatás a piactéren **ehhez az adatkészlethez felfedezés** lap.</span><span class="sxs-lookup"><span data-stu-id="25dd7-119">One or more input parameters of your web service may have exceeded the length limit when consuming the web service on the marketplace **Explore this dataset** page.</span></span> <span data-ttu-id="25dd7-120">A szolgáltatások egy már bemeneti adat hosszánál HTTP POST metódusok használatával hívható.</span><span class="sxs-lookup"><span data-stu-id="25dd7-120">The services can be called with a longer input length by using HTTP POST methods.</span></span> <span data-ttu-id="25dd7-121">Tekintse meg a [R használata a Machine Learning megoldások mintát, és teszi közzé az piactér](machine-learning-r-csharp-web-service-examples.md).</span><span class="sxs-lookup"><span data-stu-id="25dd7-121">For examples, see [Sample solutions using R on Machine Learning and published to Marketplace](machine-learning-r-csharp-web-service-examples.md).</span></span>

<span data-ttu-id="25dd7-122">**4. Miért nem látom sem a "API EXPLORER" lapon int a tároló, a klasszikus Azure-portálon?**</span><span class="sxs-lookup"><span data-stu-id="25dd7-122">**4. Why do I not see anything in the "API EXPLORER" tab int the Store in the Azure Classic Portal?**</span></span> 

<span data-ttu-id="25dd7-123">Ez az egy ismert probléma az Azure klasszikus portál Piactéri.</span><span class="sxs-lookup"><span data-stu-id="25dd7-123">This is a known issue with the Azure Classic Portal Marketplace.</span></span> <span data-ttu-id="25dd7-124">A csapat dolgozik a probléma megoldásához.</span><span class="sxs-lookup"><span data-stu-id="25dd7-124">The team is working to resolve this issue.</span></span> 

## <a name="questions-about-publishing-from-azure-machine-learning-on-marketplace"></a><span data-ttu-id="25dd7-125">Azure Machine Learning piactéren történő közzétételre vonatkozó kérdések</span><span class="sxs-lookup"><span data-stu-id="25dd7-125">Questions about publishing from Azure Machine Learning on Marketplace</span></span>
<span data-ttu-id="25dd7-126">**1. Miért van a tranzakciók emblémák vagy képek nem frissíti a webszolgáltatáshoz?**</span><span class="sxs-lookup"><span data-stu-id="25dd7-126">**1. Why are my transactions of logos or images not refreshing for my web service?**</span></span> 

<span data-ttu-id="25dd7-127">Emblémák és lemezképek gyorsítótárba kerüljenek-e a közzétételi portálon, és új embléma vagy a portál frissítése kép legfeljebb 10 napig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="25dd7-127">Logos and images are cached in the publishing portal, and it may take up to 10 days for the new logo or image to update on the portal.</span></span>

<span data-ttu-id="25dd7-128">**2. Miért van a "Részletek" lap piactér hibaüzenetet jelenít meg a webszolgáltatás?**</span><span class="sxs-lookup"><span data-stu-id="25dd7-128">**2. Why is the “Detail" tab of my web service on Marketplace showing an error message?**</span></span>

<span data-ttu-id="25dd7-129">Egy ismert piactér probléma van a szolgáltatás részletes adatai az Azure Machine Learning való csatlakozáshoz.</span><span class="sxs-lookup"><span data-stu-id="25dd7-129">There is a known Marketplace issue when connecting to Azure Machine Learning for service details.</span></span> <span data-ttu-id="25dd7-130">A csapat dolgozik a probléma megoldásához.</span><span class="sxs-lookup"><span data-stu-id="25dd7-130">The team is working to resolve this issue.</span></span>

<span data-ttu-id="25dd7-131">**3. Miért az Azure Machine Learning webszolgáltatások R példakód nem működik a piactér webszolgáltatások felhasználása?**</span><span class="sxs-lookup"><span data-stu-id="25dd7-131">**3. Why does the R sample code in the Azure Machine Learning web services not work for consuming the web services in Marketplace?**</span></span>

<span data-ttu-id="25dd7-132">A hitelesítési rendszerekkel eltérőek, ezek a webszolgáltatások a piactéren keresztül csatlakozik közvetlenül képest Azure Machine Learning webszolgáltatások történő csatlakozás során.</span><span class="sxs-lookup"><span data-stu-id="25dd7-132">The authentication systems are different when connecting to Azure Machine Learning web services directly compared to connecting to these web services through the Marketplace.</span></span> <span data-ttu-id="25dd7-133">A szolgáltatásoknak a piactér OData-szolgáltatásaival, és a GET vagy POST metódusok hívható.</span><span class="sxs-lookup"><span data-stu-id="25dd7-133">The services in Marketplace are OData services, and they can be called with GET or POST methods.</span></span> 

<span data-ttu-id="25dd7-134">**4. Miért van a támogatási hivatkozások a webszolgáltatás nem frissül megfelelően a ajánlatok részénél kínál?**</span><span class="sxs-lookup"><span data-stu-id="25dd7-134">**4. Why are the support links of my web service offers not updating correctly for some of my offers?**</span></span>

<span data-ttu-id="25dd7-135">A támogatási hivatkozások / publisher, nem az ajánlat / globálisak.</span><span class="sxs-lookup"><span data-stu-id="25dd7-135">The support links are global per publisher, not per offer.</span></span> 

<span data-ttu-id="25dd7-136">**5. Hogyan tehetők közzé egy webszolgáltatás-bővítmény bemeneti kötegelt módban a piactéren?**</span><span class="sxs-lookup"><span data-stu-id="25dd7-136">**5. How do I publish a web service with batch input mode in Marketplace?**</span></span>

<span data-ttu-id="25dd7-137">A bemeneti kötegelt módban jelenleg nem támogatott a piactér webszolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="25dd7-137">The batch input mode is currently not supported in Marketplace web services.</span></span>

<span data-ttu-id="25dd7-138">**6. Ki kell kapcsolatba Ha segítséget szeretne kérni, ha kérdéseik vannak a közzétevő váljon, vagy ha közzététele során problémába ütközik??**</span><span class="sxs-lookup"><span data-stu-id="25dd7-138">**6. Who should I contact to get help if I have questions about becoming a data publisher, or if I have issues during publishing?**</span></span>

<span data-ttu-id="25dd7-139">Lépjen kapcsolatba az Azure piactér csapatának < mailto:datamarketbd@microsoft.com > további információt.</span><span class="sxs-lookup"><span data-stu-id="25dd7-139">Please contact the Azure Marketplace team at <mailto:datamarketbd@microsoft.com> for more information.</span></span>

