---
title: "Alkalmazás és szolgáltatás elérhetőségével kapcsolatos problémákat a Microsoft Azure Cloud Services – gyakori kérdések |} Microsoft Docs"
description: "Ez a cikk a Microsoft Azure-szolgáltatásokhoz alkalmazás és szolgáltatás rendelkezésre állása kapcsolatos gyakori kérdésekre. sorolja fel."
services: cloud-services
documentationcenter: 
author: genlin
manager: cshepard
editor: 
tags: top-support-issue
ms.assetid: 84985660-2cfd-483a-8378-50eef6a0151d
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/10/2017
ms.author: genli
ms.openlocfilehash: a893a6d009dd018ad440964419e0a5a1963084b4
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="application-and-service-availability-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a><span data-ttu-id="dd30a-103">Alkalmazások és szolgáltatások elérhetőségi problémáinak Azure-szolgáltatásokhoz: gyakran ismételt kérdések (GYIK)</span><span class="sxs-lookup"><span data-stu-id="dd30a-103">Application and service availability issues for Azure Cloud Services: Frequently asked questions (FAQs)</span></span>

<span data-ttu-id="dd30a-104">Ez a cikk tartalmazza az alkalmazás és szolgáltatás elérhetőségével kapcsolatos problémákat a kapcsolatos gyakran ismételt kérdések [Microsoft Azure Cloud Services](https://azure.microsoft.com/services/cloud-services).</span><span class="sxs-lookup"><span data-stu-id="dd30a-104">This article includes frequently asked questions about application and service availability issues for [Microsoft Azure Cloud Services](https://azure.microsoft.com/services/cloud-services).</span></span> <span data-ttu-id="dd30a-105">Is tud kezelni a [felhőalapú szolgáltatások Virtuálisgép-méretet lap](cloud-services-sizes-specs.md) mérete információt.</span><span class="sxs-lookup"><span data-stu-id="dd30a-105">You can also consult the [Cloud Services VM Size page](cloud-services-sizes-specs.md) for size information.</span></span>

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="my-role-got-recycled-was-there-any-update-rolled-out-for-my-cloud-service"></a><span data-ttu-id="dd30a-106">A szerepkör lett hasznosítva.</span><span class="sxs-lookup"><span data-stu-id="dd30a-106">My role got recycled.</span></span> <span data-ttu-id="dd30a-107">A felhőszolgáltatás megkezdődött módosításairól történt?</span><span class="sxs-lookup"><span data-stu-id="dd30a-107">Was there any update rolled out for my cloud service?</span></span>
<span data-ttu-id="dd30a-108">Nagyjából havonta, Microsoft kiadását egy új vendég operációs rendszer verziója a Windows Azure PaaS virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="dd30a-108">Roughly once a month, Microsoft releases a new Guest OS version for Windows Azure PaaS VMs.</span></span><span data-ttu-id="dd30a-109"> A vendég operációs rendszer csak egy ilyen frissítés folyamatban.</span><span class="sxs-lookup"><span data-stu-id="dd30a-109"> The Guest OS is only one such update in the pipeline.</span></span> <span data-ttu-id="dd30a-110">Egy kiadási sok más tényezőktől is érinti.</span><span class="sxs-lookup"><span data-stu-id="dd30a-110">A release can be affected by many other factors.</span></span> <span data-ttu-id="dd30a-111">Emellett Azure több ezer gép több száz futtatja.</span><span class="sxs-lookup"><span data-stu-id="dd30a-111">In addition, Azure runs on hundreds of thousands of machines.</span></span> <span data-ttu-id="dd30a-112">Ezért lehetetlen előre jelezni a pontos dátum és idő, amikor a szerepkörök újraindul.</span><span class="sxs-lookup"><span data-stu-id="dd30a-112">Therefore, it's impossible to predict the exact date and time when your roles will reboot.</span></span> <span data-ttu-id="dd30a-113">A legújabb információkkal kell frissítjük a vendég operációs rendszer frissítése RSS-hírcsatorna, de vegye figyelembe, hogy jelenteni idő becsült érték.</span><span class="sxs-lookup"><span data-stu-id="dd30a-113">We update the Guest OS Update RSS Feed with the latest information that we have, but you should consider that reported time to be an approximate value.</span></span> <span data-ttu-id="dd30a-114">Azt észleli, hogy ez a problematikus, az ügyfelek, és a terv korlátot vagy a pontos idő újraindítások dolgoznak.</span><span class="sxs-lookup"><span data-stu-id="dd30a-114">We are aware that this is problematic for customers and are working on a plan to limit or precisely time reboots.</span></span>

<span data-ttu-id="dd30a-115">Teljes legutóbbi vendég operációs rendszer frissítése, lásd: [Azure vendég operációs rendszereinek kiadásait és SDK-kompatibilitási mátrixát](cloud-services-guestos-update-matrix.md).</span><span class="sxs-lookup"><span data-stu-id="dd30a-115">For complete details about recent Guest OS updates, see [Azure Guest OS releases and SDK compatibility matrix](cloud-services-guestos-update-matrix.md).</span></span>

<span data-ttu-id="dd30a-116">Újraindítás és a Vendég és a gazda operációs Rendszerével frissítések technikai részleteket mutató hivatkozások hasznos információkat lásd az MSDN blogbejegyzésben [szerepkör példány újraindítása miatt az operációs rendszer frissítései](http://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx).</span><span class="sxs-lookup"><span data-stu-id="dd30a-116">For helpful information on restarts and pointers to technical details of Guest and Host OS updates, see the MSDN blog post [Role Instance Restarts Due to OS Upgrades](http://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx).</span></span>

## <a name="why-does-the-first-request-to-my-cloud-service-after-the-service-has-been-idle-for-some-time-take-longer-than-usual"></a><span data-ttu-id="dd30a-117">Miért nem az első kérésre a felhőalapú szolgáltatáshoz után a szolgáltatás inaktív egy ideig tarthat a szokásosnál hosszabb ideig?</span><span class="sxs-lookup"><span data-stu-id="dd30a-117">Why does the first request to my cloud service after the service has been idle for some time take longer than usual?</span></span>
<span data-ttu-id="dd30a-118">Amikor a webkiszolgáló az első kérelmet kap, először újrafordítja a kódot, és majd feldolgozza a kérést.</span><span class="sxs-lookup"><span data-stu-id="dd30a-118">When the Web Server receives the first request, it first recompiles the code and then processes the request.</span></span> <span data-ttu-id="dd30a-119">Ezért az első kérésre tovább tart, mint a többi.</span><span class="sxs-lookup"><span data-stu-id="dd30a-119">That's why the first request takes longer than the others.</span></span> <span data-ttu-id="dd30a-120">Alapértelmezés szerint az alkalmazáskészlet felhasználói tétlen esetekben lekérdezi leállítása.</span><span class="sxs-lookup"><span data-stu-id="dd30a-120">By default, the app pool gets shut down in cases of user inactivity.</span></span> <span data-ttu-id="dd30a-121">Az alkalmazáskészlet is indul újra alapértelmezés szerint minden 1,740 perc (29 óra).</span><span class="sxs-lookup"><span data-stu-id="dd30a-121">The app pool will also recycle by default every 1,740 minutes (29 hours).</span></span>

<span data-ttu-id="dd30a-122">Internet Information Services (IIS) lehet, hogy rendszeres időközönként újrahasznosít alkalmazás vezethet instabil állapotok elkerülése érdekében készletek omlik össze, lefagy, vagy memóriavesztések.</span><span class="sxs-lookup"><span data-stu-id="dd30a-122">Internet Information Services (IIS) application pools can be periodically recycled to avoid unstable states that can lead to application crashes, hangs, or memory leaks.</span></span>

<span data-ttu-id="dd30a-123">A következő dokumentumok segít megérteni, és a probléma elhárítása érdekében:</span><span class="sxs-lookup"><span data-stu-id="dd30a-123">The following documents will help you understand and mitigate this issue:</span></span>
* [<span data-ttu-id="dd30a-124">Az IIS lassú kezdeti betöltés kijavítása</span><span class="sxs-lookup"><span data-stu-id="dd30a-124">Fixing slow initial load for IIS</span></span>](http://stackoverflow.com/questions/13386471/fixing-slow-initial-load-for-iis)
* [<span data-ttu-id="dd30a-125">Az IIS 7.5 webes alkalmazás első kérelem után-alkalmazáskészlet újrahasznosítási nagyon lelassul</span><span class="sxs-lookup"><span data-stu-id="dd30a-125">IIS 7.5 web application first request after app-pool recycle very slow</span></span>](http://stackoverflow.com/questions/13917205/iis-7-5-web-application-first-request-after-app-pool-recycle-very-slow)

<span data-ttu-id="dd30a-126">Ha az IIS alapértelmezett konfigurációját módosítani kívánja, szüksége lesz indítási feladatok használatával, mert ha manuálisan módosította a webes szerepkör-példányokban kívánja, a módosítások végül elvesznek.</span><span class="sxs-lookup"><span data-stu-id="dd30a-126">If you want to change the default behavior of IIS, you will need to use startup tasks, because if you manually apply changes to the Web Role instances, the changes will eventually be lost.</span></span>

<span data-ttu-id="dd30a-127">További információkért lásd: [hogyan lehet konfigurálni és futtatni egy felhőalapú szolgáltatás indítási feladatokat](cloud-services-startup-tasks.md).</span><span class="sxs-lookup"><span data-stu-id="dd30a-127">For more information, see [How to configure and run startup tasks for a cloud service](cloud-services-startup-tasks.md).</span></span>
