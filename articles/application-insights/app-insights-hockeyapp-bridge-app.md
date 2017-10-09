---
title: Azure Application Insights adatainak Hockeyappra aaaExploring |} Microsoft Docs
description: "Elemezze a használati és teljesítményadatokat az Azure-alkalmazás az Application insights szolgáltatással."
services: application-insights
documentationcenter: windows
author: CFreemanwa
manager: carmonm
ms.assetid: 97783cc6-67d6-465f-9926-cb9821f4176e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: bwren
ms.openlocfilehash: ed7cf99b48f5ec78d6b401bb954cfcd014b9d1f4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="exploring-hockeyapp-data-in-application-insights"></a><span data-ttu-id="3105a-103">Az Application Insightsban Hockeyappra adatok</span><span class="sxs-lookup"><span data-stu-id="3105a-103">Exploring HockeyApp data in Application Insights</span></span>
<span data-ttu-id="3105a-104">[Hockeyappra](https://azure.microsoft.com/services/hockeyapp/) ajánlott hello platform élő az asztali és mobil alkalmazások figyeléséhez.</span><span class="sxs-lookup"><span data-stu-id="3105a-104">[HockeyApp](https://azure.microsoft.com/services/hockeyapp/) is hello recommended platform for monitoring live desktop and mobile apps.</span></span> <span data-ttu-id="3105a-105">A Hockeyappra küldhet egyéni és nyomkövetési telemetria toomonitor használati és diagnosztikai (a hozzáadása toogetting összeomlási adatait) támogatása.</span><span class="sxs-lookup"><span data-stu-id="3105a-105">From HockeyApp, you can send custom and trace telemetry toomonitor usage and assist in diagnosis (in addition toogetting crash data).</span></span> <span data-ttu-id="3105a-106">Ez az adatfolyam telemetriai adatot lekérdezhetők, hello hatékony használata [Analytics](app-insights-analytics.md) szolgáltatása [Azure Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3105a-106">This stream of telemetry can be queried using hello powerful [Analytics](app-insights-analytics.md) feature of [Azure Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="3105a-107">Emellett képes [exportálja az egyéni hello és nyomkövetési telemetria](app-insights-export-telemetry.md).</span><span class="sxs-lookup"><span data-stu-id="3105a-107">In addition, you can [export hello custom and trace telemetry](app-insights-export-telemetry.md).</span></span> <span data-ttu-id="3105a-108">Ezek a szolgáltatások tooenable, állít be, amely továbbítja a Hockeyappra egyéni adatok tooApplication Insights hidat.</span><span class="sxs-lookup"><span data-stu-id="3105a-108">tooenable these features, you set up a bridge that relays HockeyApp custom data tooApplication Insights.</span></span>

## <a name="hello-hockeyapp-bridge-app"></a><span data-ttu-id="3105a-109">hello Hockeyappra híd alkalmazás</span><span class="sxs-lookup"><span data-stu-id="3105a-109">hello HockeyApp Bridge app</span></span>
<span data-ttu-id="3105a-110">hello Hockeyappra híd alkalmazás, amely lehetővé teszi a Hockeyappra egyéni és hello Analytics keresztül az Application Insights – nyomkövetési telemetria tooaccess hello core szolgáltatás és a szolgáltatások folyamatos exportálása.</span><span class="sxs-lookup"><span data-stu-id="3105a-110">hello HockeyApp Bridge App is hello core feature that enables you tooaccess your HockeyApp custom and trace telemetry in Application Insights through hello Analytics and Continuous Export features.</span></span> <span data-ttu-id="3105a-111">Egyéni és a nyomkövetési események hello Hockeyappra híd App hello létrehozása után a Hockeyappra által gyűjtött fog érhető el ezeket a szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="3105a-111">Custom and trace events collected by HockeyApp after hello creation of hello HockeyApp Bridge App will be accessible from these features.</span></span> <span data-ttu-id="3105a-112">Nézzük meg, hogyan tooset híd alkalmazások közül.</span><span class="sxs-lookup"><span data-stu-id="3105a-112">Let’s see how tooset up one of these Bridge Apps.</span></span>

<span data-ttu-id="3105a-113">A Hockeyappra, nyissa meg a fiók beállításait, [API jogkivonatok](https://rink.hockeyapp.net/manage/auth_tokens).</span><span class="sxs-lookup"><span data-stu-id="3105a-113">In HockeyApp, open Account Settings, [API Tokens](https://rink.hockeyapp.net/manage/auth_tokens).</span></span> <span data-ttu-id="3105a-114">Hozzon létre egy új jogkivonatot, vagy egy meglévő használja fel.</span><span class="sxs-lookup"><span data-stu-id="3105a-114">Either create a new token or reuse an existing one.</span></span> <span data-ttu-id="3105a-115">hello minimális jogosultságokkal szükséges, amelyek "csak olvasható".</span><span class="sxs-lookup"><span data-stu-id="3105a-115">hello minimum rights required are "read only".</span></span> <span data-ttu-id="3105a-116">Hello API másolatát jogkivonat érvénybe.</span><span class="sxs-lookup"><span data-stu-id="3105a-116">Take a copy of hello API token.</span></span>

![A Hockeyappra API-token beszerzése](./media/app-insights-hockeyapp-bridge-app/01.png)

<span data-ttu-id="3105a-118">Nyissa meg hello Microsoft Azure-portálon és [Application Insights-erőforrás létrehozása](app-insights-create-new-resource.md).</span><span class="sxs-lookup"><span data-stu-id="3105a-118">Open hello Microsoft Azure portal and [create an Application Insights resource](app-insights-create-new-resource.md).</span></span> <span data-ttu-id="3105a-119">Alkalmazás típusa túl beállítása "Hockeyappra híd alkalmazás":</span><span class="sxs-lookup"><span data-stu-id="3105a-119">Set Application Type too“HockeyApp bridge application”:</span></span>

![Új Application Insights-erőforrás](./media/app-insights-hockeyapp-bridge-app/02.png)

<span data-ttu-id="3105a-121">Nem kell tooset nevét – ez a program automatikusan állítja hello Hockeyappra neve alapján.</span><span class="sxs-lookup"><span data-stu-id="3105a-121">You don't need tooset a name - this will automatically be set from hello HockeyApp name.</span></span>

<span data-ttu-id="3105a-122">hello Hockeyappra híd mezők jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="3105a-122">hello HockeyApp bridge fields appear.</span></span> 

![Adja meg a híd mezők](./media/app-insights-hockeyapp-bridge-app/03.png)

<span data-ttu-id="3105a-124">Adja meg a korábban feljegyzett hello Hockeyappra jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="3105a-124">Enter hello HockeyApp token you noted earlier.</span></span> <span data-ttu-id="3105a-125">Ez a művelet feltölti hello "Hockeyappra alkalmazás" legördülő menüjében a Hockeyappra alkalmazásokkal.</span><span class="sxs-lookup"><span data-stu-id="3105a-125">This action populates hello “HockeyApp Application” dropdown menu with all your HockeyApp applications.</span></span> <span data-ttu-id="3105a-126">Válassza ki a kívánt toouse és hello mezők teljes hello további része egy hello.</span><span class="sxs-lookup"><span data-stu-id="3105a-126">Select hello one you want toouse, and complete hello remainder of hello fields.</span></span> 

<span data-ttu-id="3105a-127">Nyissa meg a hello új erőforrást.</span><span class="sxs-lookup"><span data-stu-id="3105a-127">Open hello new resource.</span></span> 

<span data-ttu-id="3105a-128">Vegye figyelembe, hogy hello adatok eltart egy ideig toostart továbbítására.</span><span class="sxs-lookup"><span data-stu-id="3105a-128">Note that hello data takes a while toostart flowing.</span></span>

![Várakozás az adatok Application Insights-erőforrás](./media/app-insights-hockeyapp-bridge-app/04.png)

<span data-ttu-id="3105a-130">Készen is van.</span><span class="sxs-lookup"><span data-stu-id="3105a-130">That’s it!</span></span> <span data-ttu-id="3105a-131">Ettől kezdve a Hockeyappra tagolva alkalmazásban gyűjtött egyéni és nyomkövetési adatok mostantól is hello Analytics a rendelkezésre álló tooyou és az Application Insights a folyamatos exportálás funkcióit.</span><span class="sxs-lookup"><span data-stu-id="3105a-131">Custom and trace data collected in your HockeyApp-instrumented app from this point forward is now also available tooyou in hello Analytics and Continuous Export features of Application Insights.</span></span>

<span data-ttu-id="3105a-132">Most rövid időre tekintse át az összes ezen szolgáltatások most már hozzáférhető tooyou.</span><span class="sxs-lookup"><span data-stu-id="3105a-132">Let’s briefly review each of these features now available tooyou.</span></span>

## <a name="analytics"></a><span data-ttu-id="3105a-133">Elemzés</span><span class="sxs-lookup"><span data-stu-id="3105a-133">Analytics</span></span>
<span data-ttu-id="3105a-134">Elemzés alkalmi az adatok lekérdezése, hogy lehetővé teszi a toodiagnose hatékony eszköz és a telemetriai adatok elemzése és gyorsan Fedezze fel az alapvető okok és a minták.</span><span class="sxs-lookup"><span data-stu-id="3105a-134">Analytics is a powerful tool for ad-hoc querying of your data, allowing you toodiagnose and analyze your telemetry and quickly discover root causes and patterns.</span></span>

![Elemzés](./media/app-insights-hockeyapp-bridge-app/05.png)

* [<span data-ttu-id="3105a-136">További információ az elemzés</span><span class="sxs-lookup"><span data-stu-id="3105a-136">Learn more about Analytics</span></span>](app-insights-analytics-tour.md)

## <a name="continuous-export"></a><span data-ttu-id="3105a-137">Folyamatos exportálás</span><span class="sxs-lookup"><span data-stu-id="3105a-137">Continuous export</span></span>
<span data-ttu-id="3105a-138">A folyamatos exportálás tooexport lehetővé teszi az adatok az Azure Blob Storage tárolóba.</span><span class="sxs-lookup"><span data-stu-id="3105a-138">Continuous Export allows you tooexport your data into an Azure Blob Storage container.</span></span> <span data-ttu-id="3105a-139">Nagyon hasznos, ha szüksége tookeep az adatok hosszabb, mint jelenleg az Application Insights által kínált hello megőrzési időtartam azt.</span><span class="sxs-lookup"><span data-stu-id="3105a-139">This is very useful if you need tookeep your data for longer than hello retention period currently offered by Application Insights.</span></span> <span data-ttu-id="3105a-140">A blob Storage tárolóban hello adatok megőrzése, SQL-adatbázis, vagy az elsődleges adatok tekinthetünk feldolgozni azt.</span><span class="sxs-lookup"><span data-stu-id="3105a-140">You can keep hello data in blob storage, process it into a SQL Database, or your preferred data warehousing solution.</span></span>

[<span data-ttu-id="3105a-141">További információ a folyamatos exportálás</span><span class="sxs-lookup"><span data-stu-id="3105a-141">Learn more about Continuous Export</span></span>](app-insights-export-telemetry.md)

## <a name="next-steps"></a><span data-ttu-id="3105a-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3105a-142">Next steps</span></span>
* [<span data-ttu-id="3105a-143">Elemzés tooyour adatok alkalmazásához</span><span class="sxs-lookup"><span data-stu-id="3105a-143">Apply Analytics tooyour data</span></span>](app-insights-analytics-tour.md)

