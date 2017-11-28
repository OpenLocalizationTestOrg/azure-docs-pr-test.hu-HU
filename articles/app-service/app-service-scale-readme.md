---
title: "Az Azure App Service: Az App Service alkalmazások méretezése"
description: "Ismerje meg, hogy hello modulok és elemek a méretezés alkalmazás az App Service-ben."
keywords: "app service, azure app service, méret, méretezhető, app service-csomag, app service ára"
services: app-service
documentationcenter: 
author: btardif
manager: erikre
editor: 
ms.assetid: f403c971-4450-432b-8cea-3eeb426c0147
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/07/2016
ms.author: byvinyal
ms.openlocfilehash: d8cd12f73915a916a75d46da2f751a40d567b189
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-scaling-app-service-applications"></a><span data-ttu-id="3dd3d-104">Az Azure App Service: Az App Service alkalmazások méretezése</span><span class="sxs-lookup"><span data-stu-id="3dd3d-104">Azure App Service: Scaling App Service Applications</span></span>
<span data-ttu-id="3dd3d-105">Az Azure App Service-ben üzemeltetett alkalmazások is [jelentős mértékű elérése](https://azure.microsoft.com/blog/canadian-broadcasting-corporation-radio-canada-leverage-azure-for-smooth-election-coverage/).</span><span class="sxs-lookup"><span data-stu-id="3dd3d-105">Applications hosted in Azure App Service can [achieve massive scale](https://azure.microsoft.com/blog/canadian-broadcasting-corporation-radio-canada-leverage-azure-for-smooth-election-coverage/).</span></span>
<span data-ttu-id="3dd3d-106">Azonban az alkalmazás skálázás "egy mérete megfelel az összes" megoldás nem rendelkező összetett probléma.</span><span class="sxs-lookup"><span data-stu-id="3dd3d-106">However, scaling an application is a complex problem that does not have a "one size fits all" solution.</span></span> <span data-ttu-id="3dd3d-107">toocorrectly az alkalmazás vertikális 3 főbb területet, ez is hozzájárul a tooyour alkalmazások sikeres:</span><span class="sxs-lookup"><span data-stu-id="3dd3d-107">toocorrectly scale your application there are 3 key areas that will contribute tooyour applications success:</span></span>

1. <span data-ttu-id="3dd3d-108">Az alkalmazás-architektúra és a gyenge ismertetése.</span><span class="sxs-lookup"><span data-stu-id="3dd3d-108">Understanding your application architecture and its weaknesses.</span></span>
   * <span data-ttu-id="3dd3d-109">Az az alkalmazás állapotalapú alkalmazások és szolgáltatások?</span><span class="sxs-lookup"><span data-stu-id="3dd3d-109">Is your Application Stateful?</span></span> <span data-ttu-id="3dd3d-110">Állapot nélküli?</span><span class="sxs-lookup"><span data-stu-id="3dd3d-110">Stateless?</span></span>
   * <span data-ttu-id="3dd3d-111">Mik az alkalmazás összes hello összetevőket?</span><span class="sxs-lookup"><span data-stu-id="3dd3d-111">What are all hello components of your application?</span></span>
     * <span data-ttu-id="3dd3d-112">Hol vannak hello keresztmetszetei hello alkalmazást?</span><span class="sxs-lookup"><span data-stu-id="3dd3d-112">Where are hello bottlenecks in hello application?</span></span>
   * <span data-ttu-id="3dd3d-113">Alkalmazott tooyour alkalmazás terhelés esetén milyen törés első?</span><span class="sxs-lookup"><span data-stu-id="3dd3d-113">When load is applied tooyour app, what will break first?</span></span>
2. <span data-ttu-id="3dd3d-114">Understanding hello várható terhelést és teljesítménybeli követelményeit.</span><span class="sxs-lookup"><span data-stu-id="3dd3d-114">Understanding hello expected load and performance requirements.</span></span>
   * <span data-ttu-id="3dd3d-115">Szükséges tooserve egy ezer felhasználók hello alkalmazás? vagy egymillió?</span><span class="sxs-lookup"><span data-stu-id="3dd3d-115">Does hello application need tooserve one thousand users? or one million?</span></span>
   * <span data-ttu-id="3dd3d-116">Egy földrajzi helyről vagy globálisan határozza meg forgalom?</span><span class="sxs-lookup"><span data-stu-id="3dd3d-116">Will traffic come from a single geographic location or globally?</span></span>
   * <span data-ttu-id="3dd3d-117">Vannak-e határozza változata? forgalom csúcsait?</span><span class="sxs-lookup"><span data-stu-id="3dd3d-117">Are there seasonal variations? traffic peaks?</span></span>
   * <span data-ttu-id="3dd3d-118">Milyen gyorsan kell válaszolnia a hello alkalmazást?</span><span class="sxs-lookup"><span data-stu-id="3dd3d-118">How fast should hello app respond?</span></span> <span data-ttu-id="3dd3d-119">1 másodperc?</span><span class="sxs-lookup"><span data-stu-id="3dd3d-119">1 second?</span></span> <span data-ttu-id="3dd3d-120">1 ezredmásodpercre?</span><span class="sxs-lookup"><span data-stu-id="3dd3d-120">1 millisecond?</span></span>
3. <span data-ttu-id="3dd3d-121">Ismertetése és megfelelően emelés hello platform Alkalmazáshasználat üzemeltetési.</span><span class="sxs-lookup"><span data-stu-id="3dd3d-121">Understanding and correctly leverage hello platform hosting your app.</span></span>
   * <span data-ttu-id="3dd3d-122">Szolgáltatások kell szeretnék használni tooachieve saját méretezési célok?</span><span class="sxs-lookup"><span data-stu-id="3dd3d-122">What features should I leverage tooachieve my scale goals?</span></span>

<span data-ttu-id="3dd3d-123">Ez a szakasz segítséget nyújt a hello tényezők és megtervezi a stratégiát, amely szükséges az App Service szolgáltatások tooachieve hello kihasználja a méretezhetőségi célok súgó.</span><span class="sxs-lookup"><span data-stu-id="3dd3d-123">This section will help you understand all hello factors and help you devise a strategy that takes advantage of hello necessary App Service features tooachieve your scalability goals.</span></span>

[!INCLUDE [app-service-blueprint-scaling-app-service-applications](../../includes/app-service-blueprint-scaling-app-service-applications.md)]

