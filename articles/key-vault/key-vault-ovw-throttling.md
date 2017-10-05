---
ms.assetid: 
title: "Sávszélesség-szabályozás útmutató az Azure Key Vault |} Microsoft Docs"
ms.service: key-vault
author: BrucePerlerMS
ms.author: bruceper
manager: mbaldwin
ms.date: 06/21/2017
ms.openlocfilehash: fe700e22c5323c2a0bdc315e349cd119798bcf40
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-key-vault-throttling-guidance"></a><span data-ttu-id="636b3-102">Az Azure Key Vault útmutatást szabályozása</span><span class="sxs-lookup"><span data-stu-id="636b3-102">Azure Key Vault throttling guidance</span></span>

<span data-ttu-id="636b3-103">Sávszélesség-szabályozás elindít egy folyamatot, amely korlátozza az erőforrások túlhasználat megelőzése érdekében az Azure szolgáltatásban az egyidejű hívások száma.</span><span class="sxs-lookup"><span data-stu-id="636b3-103">Throttling is a process you initiate that limits the number of concurrent calls to the Azure service to prevent overuse of resources.</span></span> <span data-ttu-id="636b3-104">Az Azure Key Vault (AKV) nagy mennyiségű kérést kezelésére terveztek.</span><span class="sxs-lookup"><span data-stu-id="636b3-104">Azure Key Vault (AKV) is designed to handle a high volume of requests.</span></span> <span data-ttu-id="636b3-105">Egy túlságosan kérelmek száma akkor fordul elő, ha az ügyfélkérelmek sávszélesség-szabályozás optimális teljesítménye és megbízhatósága a AKV szolgáltatás karbantartása.</span><span class="sxs-lookup"><span data-stu-id="636b3-105">If an overwhelming number of requests occurs, throttling your client's requests helps maintain optimal performance and reliability of the AKV service.</span></span>

<span data-ttu-id="636b3-106">A forgatókönyv függően változhat a sávszélesség-szabályozási korlátok.</span><span class="sxs-lookup"><span data-stu-id="636b3-106">Throttling limits vary based on the scenario.</span></span> <span data-ttu-id="636b3-107">Például ha nagy mennyiségű írási műveleteket hajt végre, szabályozás lehetőség, magasabb, mint ha olvasási műveletek csak végzik.</span><span class="sxs-lookup"><span data-stu-id="636b3-107">For example, if you are performing a large volume of writes, the possibility for throttling is higher than if you are only performing reads.</span></span>

## <a name="how-does-key-vault-handle-its-limits"></a><span data-ttu-id="636b3-108">Hogyan nem kezelik a teljes kapacitásukkal működjenek a Key Vault?</span><span class="sxs-lookup"><span data-stu-id="636b3-108">How does Key Vault handle its limits?</span></span>

<span data-ttu-id="636b3-109">A Key Vault szolgáltatásra vonatkozó korlátozások-e erőforrásokhoz való visszaélés és az összes Key Vault ügyfelek minőségének biztosítása érdekében.</span><span class="sxs-lookup"><span data-stu-id="636b3-109">Service limits in Key Vault are there to prevent misuse of resources and ensure quality of service for all of Key Vault’s clients.</span></span> <span data-ttu-id="636b3-110">A szolgáltatás küszöbérték túllépésekor a Key Vault semmilyen további kérelmeit, hogy az ügyfél korlátozza egy ideig.</span><span class="sxs-lookup"><span data-stu-id="636b3-110">When a service threshold is exceeded, Key Vault limits any further requests from that client for a period of time.</span></span> <span data-ttu-id="636b3-111">Ha ez történik, a Key Vault HTTP-állapotkód 429 adja vissza (túl sok kérelem), és a kérelem sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="636b3-111">When this happens, Key Vault returns HTTP status code 429 (Too many requests), and the requests fail.</span></span> <span data-ttu-id="636b3-112">A Key Vault követik szabályozási korlátok felé 429-es jelű szerepleni kérelmek is, sikertelen.</span><span class="sxs-lookup"><span data-stu-id="636b3-112">Also, failed requests that return a 429 count towards the throttle limits tracked by Key Vault.</span></span> 

<span data-ttu-id="636b3-113">Ha a nagyobb mértékű késleltetési korlátozás érvényes üzleti eset, lépjen kapcsolatba velünk a következő címen.</span><span class="sxs-lookup"><span data-stu-id="636b3-113">If you have a valid business case for higher throttle limits, please contact us.</span></span>


## <a name="how-to-throttle-your-app-in-response-to-service-limits"></a><span data-ttu-id="636b3-114">Hogyan szolgáltatásra vonatkozó korlátozások válaszul Alkalmazáshasználat szabályozása</span><span class="sxs-lookup"><span data-stu-id="636b3-114">How to throttle your app in response to service limits</span></span>

<span data-ttu-id="636b3-115">A következő **ajánlott eljárások** szabályozás az alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="636b3-115">The following are **best practices** for throttling your app:</span></span>
- <span data-ttu-id="636b3-116">Csökkentse a kérelmenként műveletek számát.</span><span class="sxs-lookup"><span data-stu-id="636b3-116">Reduce the number of operations per request.</span></span>
- <span data-ttu-id="636b3-117">Csökkentse a kérelmeket.</span><span class="sxs-lookup"><span data-stu-id="636b3-117">Reduce the frequency of requests.</span></span>
- <span data-ttu-id="636b3-118">Ne használjon közvetlen újrapróbálkozások.</span><span class="sxs-lookup"><span data-stu-id="636b3-118">Avoid immediate retries.</span></span> 
    - <span data-ttu-id="636b3-119">Összes kérelem keletkeznek a használati korlátozások.</span><span class="sxs-lookup"><span data-stu-id="636b3-119">All requests accrue against your usage limits.</span></span>

<span data-ttu-id="636b3-120">Az alkalmazás hibakezelési bevezetésekor használja a HTTP-hibakódot 429 észleli a ügyféloldali szabályozás.</span><span class="sxs-lookup"><span data-stu-id="636b3-120">When you implement your app's error handling, use the HTTP error code 429 to detect the need for client-side throttling.</span></span> <span data-ttu-id="636b3-121">Ha a kérelem végrehajtása ismét sikertelen hibakódú HTTP 429, továbbra is találkozik egy Azure-szolgáltatások korlátot.</span><span class="sxs-lookup"><span data-stu-id="636b3-121">If the request fails again with an HTTP 429 error code, you are still encountering an Azure service limit.</span></span> <span data-ttu-id="636b3-122">Továbbra is használhatják az ajánlott ügyféloldali szabályozás metódus, a kérelem ismételt próbálkozásra, amíg azt nem jár sikerrel.</span><span class="sxs-lookup"><span data-stu-id="636b3-122">Continue to use the recommended client-side throttling method, retrying the request until it succeeds.</span></span>

### <a name="recommended-client-side-throttling-method"></a><span data-ttu-id="636b3-123">Ajánlott az ügyféloldali szabályozási módszer</span><span class="sxs-lookup"><span data-stu-id="636b3-123">Recommended client-side throttling method</span></span>

<span data-ttu-id="636b3-124">A HTTP-hibakódot 429 kezdje a sávszélesség-szabályozás exponenciális leállítási módszer használatát az ügyfél el:</span><span class="sxs-lookup"><span data-stu-id="636b3-124">On HTTP error code 429, begin throttling your client using an exponential backoff approach:</span></span>

1. <span data-ttu-id="636b3-125">Várjon 1 másodperc, ismételje meg a kérést</span><span class="sxs-lookup"><span data-stu-id="636b3-125">Wait 1 second, retry request</span></span>
2. <span data-ttu-id="636b3-126">Ha továbbra is halmozódni várjon 2 másodperc, próbálkozzon újra a kéréssel</span><span class="sxs-lookup"><span data-stu-id="636b3-126">If still throttled wait 2 seconds, retry request</span></span>
3. <span data-ttu-id="636b3-127">Ha továbbra is halmozódni várakozási 4 másodperc, próbálkozzon újra a kéréssel</span><span class="sxs-lookup"><span data-stu-id="636b3-127">If still throttled wait 4 seconds, retry request</span></span>
4. <span data-ttu-id="636b3-128">Ha továbbra is halmozódni várakozási 8 másodperc, próbálkozzon újra a kéréssel</span><span class="sxs-lookup"><span data-stu-id="636b3-128">If still throttled wait 8 seconds, retry request</span></span>
5. <span data-ttu-id="636b3-129">Ha továbbra is halmozódni várakozási 16 másodperc, próbálkozzon újra a kéréssel</span><span class="sxs-lookup"><span data-stu-id="636b3-129">If still throttled wait 16 seconds, retry request</span></span>

<span data-ttu-id="636b3-130">Ekkor meg kell jut HTTP 429 válaszkódot.</span><span class="sxs-lookup"><span data-stu-id="636b3-130">At this point, you should not be getting HTTP 429 response codes.</span></span>

## <a name="see-also"></a><span data-ttu-id="636b3-131">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="636b3-131">See also</span></span>

<span data-ttu-id="636b3-132">A sávszélesség-szabályozás a Microsoft Cloud a mélyebb tájolását, lásd: [sávszélesség-szabályozás mintát](https://docs.microsoft.com/azure/architecture/patterns/throttling).</span><span class="sxs-lookup"><span data-stu-id="636b3-132">For a deeper orientation of throttling on the Microsoft Cloud, see [Throttling Pattern](https://docs.microsoft.com/azure/architecture/patterns/throttling).</span></span>

