---
ms.assetid: 
title: "Key Vault szabályozási útmutatást aaaAzure |} Microsoft Docs"
ms.service: key-vault
author: BrucePerlerMS
ms.author: bruceper
manager: mbaldwin
ms.date: 06/21/2017
ms.openlocfilehash: a75cf96bc6503e51f14378bee598bad57e85be82
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-key-vault-throttling-guidance"></a><span data-ttu-id="4ff54-102">Az Azure Key Vault útmutatást szabályozása</span><span class="sxs-lookup"><span data-stu-id="4ff54-102">Azure Key Vault throttling guidance</span></span>

<span data-ttu-id="4ff54-103">Sávszélesség-szabályozás rendkívül elindította folyamat, amely korlátozza a hello száma párhuzamos hívások toohello Azure szolgáltatás tooprevent túlhasználat erőforrások.</span><span class="sxs-lookup"><span data-stu-id="4ff54-103">Throttling is a process you initiate that limits hello number of concurrent calls toohello Azure service tooprevent overuse of resources.</span></span> <span data-ttu-id="4ff54-104">Az Azure Key Vault (AKV) kialakított toohandle nagy mennyiségű kérést.</span><span class="sxs-lookup"><span data-stu-id="4ff54-104">Azure Key Vault (AKV) is designed toohandle a high volume of requests.</span></span> <span data-ttu-id="4ff54-105">Egy túlságosan kérelmek száma akkor fordul elő, ha az ügyfélkérelmek sávszélesség-szabályozás optimális teljesítménye és megbízhatósága hello AKV szolgáltatás karbantartása.</span><span class="sxs-lookup"><span data-stu-id="4ff54-105">If an overwhelming number of requests occurs, throttling your client's requests helps maintain optimal performance and reliability of hello AKV service.</span></span>

<span data-ttu-id="4ff54-106">Sávszélesség-szabályozási korlátok hello forgatókönyv függően változhat.</span><span class="sxs-lookup"><span data-stu-id="4ff54-106">Throttling limits vary based on hello scenario.</span></span> <span data-ttu-id="4ff54-107">Például ha nagy mennyiségű írási műveleteket hajt végre, hello lehetősége, hogy a sávszélesség-szabályozás értéke magasabb, mint ha olvasási műveletek csak végzik.</span><span class="sxs-lookup"><span data-stu-id="4ff54-107">For example, if you are performing a large volume of writes, hello possibility for throttling is higher than if you are only performing reads.</span></span>

## <a name="how-does-key-vault-handle-its-limits"></a><span data-ttu-id="4ff54-108">Hogyan nem kezelik a teljes kapacitásukkal működjenek a Key Vault?</span><span class="sxs-lookup"><span data-stu-id="4ff54-108">How does Key Vault handle its limits?</span></span>

<span data-ttu-id="4ff54-109">A Key Vault szolgáltatásra vonatkozó korlátozások-e az erőforrások tooprevent visszaélés, és összes Key Vault ügyfelek minőségének biztosítása.</span><span class="sxs-lookup"><span data-stu-id="4ff54-109">Service limits in Key Vault are there tooprevent misuse of resources and ensure quality of service for all of Key Vault’s clients.</span></span> <span data-ttu-id="4ff54-110">A szolgáltatás küszöbérték túllépésekor a Key Vault semmilyen további kérelmeit, hogy az ügyfél korlátozza egy ideig.</span><span class="sxs-lookup"><span data-stu-id="4ff54-110">When a service threshold is exceeded, Key Vault limits any further requests from that client for a period of time.</span></span> <span data-ttu-id="4ff54-111">Ha ez történik, a Key Vault HTTP-állapotkód 429 adja vissza (túl sok kérelem), és hello kérelmek sikertelenek.</span><span class="sxs-lookup"><span data-stu-id="4ff54-111">When this happens, Key Vault returns HTTP status code 429 (Too many requests), and hello requests fail.</span></span> <span data-ttu-id="4ff54-112">Is sikertelen kérelmek 429-es jelű szerepleni hello szabályozási korlátok követik a Key Vault felé.</span><span class="sxs-lookup"><span data-stu-id="4ff54-112">Also, failed requests that return a 429 count towards hello throttle limits tracked by Key Vault.</span></span> 

<span data-ttu-id="4ff54-113">Ha a nagyobb mértékű késleltetési korlátozás érvényes üzleti eset, lépjen kapcsolatba velünk a következő címen.</span><span class="sxs-lookup"><span data-stu-id="4ff54-113">If you have a valid business case for higher throttle limits, please contact us.</span></span>


## <a name="how-toothrottle-your-app-in-response-tooservice-limits"></a><span data-ttu-id="4ff54-114">Hogyan toothrottle az alkalmazást a válasz tooservice korlátozza</span><span class="sxs-lookup"><span data-stu-id="4ff54-114">How toothrottle your app in response tooservice limits</span></span>

<span data-ttu-id="4ff54-115">hello az alábbiakban **ajánlott eljárások** szabályozás az alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="4ff54-115">hello following are **best practices** for throttling your app:</span></span>
- <span data-ttu-id="4ff54-116">Csökkentse a kérelmenként műveletek hello számát.</span><span class="sxs-lookup"><span data-stu-id="4ff54-116">Reduce hello number of operations per request.</span></span>
- <span data-ttu-id="4ff54-117">Csökkentse a kérelmek hello gyakoriságát.</span><span class="sxs-lookup"><span data-stu-id="4ff54-117">Reduce hello frequency of requests.</span></span>
- <span data-ttu-id="4ff54-118">Ne használjon közvetlen újrapróbálkozások.</span><span class="sxs-lookup"><span data-stu-id="4ff54-118">Avoid immediate retries.</span></span> 
    - <span data-ttu-id="4ff54-119">Összes kérelem keletkeznek a használati korlátozások.</span><span class="sxs-lookup"><span data-stu-id="4ff54-119">All requests accrue against your usage limits.</span></span>

<span data-ttu-id="4ff54-120">Az alkalmazás hibakezelési bevezetésekor használata hello HTTP hiba kód 429-es jelű toodetect hello ügyféloldali szabályozás kell.</span><span class="sxs-lookup"><span data-stu-id="4ff54-120">When you implement your app's error handling, use hello HTTP error code 429 toodetect hello need for client-side throttling.</span></span> <span data-ttu-id="4ff54-121">Ha hello kérelem végrehajtása ismét sikertelen hibakódú HTTP 429, továbbra is találkozik egy Azure-szolgáltatások korlátot.</span><span class="sxs-lookup"><span data-stu-id="4ff54-121">If hello request fails again with an HTTP 429 error code, you are still encountering an Azure service limit.</span></span> <span data-ttu-id="4ff54-122">Továbbra is toouse hello ajánlott az ügyféloldali szabályozási metódus hello kérelem újrapróbálkozás, amíg azt nem jár sikerrel.</span><span class="sxs-lookup"><span data-stu-id="4ff54-122">Continue toouse hello recommended client-side throttling method, retrying hello request until it succeeds.</span></span>

### <a name="recommended-client-side-throttling-method"></a><span data-ttu-id="4ff54-123">Ajánlott az ügyféloldali szabályozási módszer</span><span class="sxs-lookup"><span data-stu-id="4ff54-123">Recommended client-side throttling method</span></span>

<span data-ttu-id="4ff54-124">A HTTP-hibakódot 429 kezdje a sávszélesség-szabályozás exponenciális leállítási módszer használatát az ügyfél el:</span><span class="sxs-lookup"><span data-stu-id="4ff54-124">On HTTP error code 429, begin throttling your client using an exponential backoff approach:</span></span>

1. <span data-ttu-id="4ff54-125">Várjon 1 másodperc, ismételje meg a kérést</span><span class="sxs-lookup"><span data-stu-id="4ff54-125">Wait 1 second, retry request</span></span>
2. <span data-ttu-id="4ff54-126">Ha továbbra is halmozódni várjon 2 másodperc, próbálkozzon újra a kéréssel</span><span class="sxs-lookup"><span data-stu-id="4ff54-126">If still throttled wait 2 seconds, retry request</span></span>
3. <span data-ttu-id="4ff54-127">Ha továbbra is halmozódni várakozási 4 másodperc, próbálkozzon újra a kéréssel</span><span class="sxs-lookup"><span data-stu-id="4ff54-127">If still throttled wait 4 seconds, retry request</span></span>
4. <span data-ttu-id="4ff54-128">Ha továbbra is halmozódni várakozási 8 másodperc, próbálkozzon újra a kéréssel</span><span class="sxs-lookup"><span data-stu-id="4ff54-128">If still throttled wait 8 seconds, retry request</span></span>
5. <span data-ttu-id="4ff54-129">Ha továbbra is halmozódni várakozási 16 másodperc, próbálkozzon újra a kéréssel</span><span class="sxs-lookup"><span data-stu-id="4ff54-129">If still throttled wait 16 seconds, retry request</span></span>

<span data-ttu-id="4ff54-130">Ekkor meg kell jut HTTP 429 válaszkódot.</span><span class="sxs-lookup"><span data-stu-id="4ff54-130">At this point, you should not be getting HTTP 429 response codes.</span></span>

## <a name="see-also"></a><span data-ttu-id="4ff54-131">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="4ff54-131">See also</span></span>

<span data-ttu-id="4ff54-132">A sávszélesség-szabályozás a Microsoft Cloud hello mélyebb tájolását, lásd: [sávszélesség-szabályozás mintát](https://docs.microsoft.com/azure/architecture/patterns/throttling).</span><span class="sxs-lookup"><span data-stu-id="4ff54-132">For a deeper orientation of throttling on hello Microsoft Cloud, see [Throttling Pattern](https://docs.microsoft.com/azure/architecture/patterns/throttling).</span></span>

