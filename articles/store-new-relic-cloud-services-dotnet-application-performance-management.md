---
title: az Azure-ral New Relic aaaUsing |} Microsoft Docs
description: "Ismerje meg, hogyan toouse New Relic szolgáltatás toomanage hello és figyelése az Azure-alkalmazásában."
services: 
documentationcenter: .net
author: nickfloyd
manager: timlt
editor: 
ms.assetid: b01011be-c344-4e33-987d-c93dac1971fb
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/23/2016
ms.author: nickfloyd@newrelic.com
ms.openlocfilehash: 81e5f1b694264e3586ac75d40edd8afe185493d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="new-relic-application-performance-management-on-azure"></a><span data-ttu-id="09ce9-103">Új New Relic alkalmazások teljesítmény kezelése az Azure-on</span><span class="sxs-lookup"><span data-stu-id="09ce9-103">New Relic Application Performance Management on Azure</span></span>
<span data-ttu-id="09ce9-104">Hozzáadhat új New Relic világszínvonalú teljesítmény tooyour figyelése Azure üzemeltetett alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="09ce9-104">You can add New Relic's world-class performance monitoring tooyour Azure hosted applications.</span></span> <span data-ttu-id="09ce9-105">Figyelés, hibaelhárítási és az Azure alkalmazások hangolása átfogó képességek mellett áll is jogosult a New Relic-termékeket kedvezményes áron Azure használatával.</span><span class="sxs-lookup"><span data-stu-id="09ce9-105">Along with comprehensive capabilities for monitoring, troubleshooting, and tuning your Azure apps, you are also eligible for a discounted price for New Relic products by using Azure.</span></span>

## <a name="what-is-new-relic"></a><span data-ttu-id="09ce9-106">Mi az a New Relic?</span><span class="sxs-lookup"><span data-stu-id="09ce9-106">What is New Relic?</span></span>
<span data-ttu-id="09ce9-107">A [New Relic termékek](https://newrelic.com/products), oldja meg app hibák, lehetséges problémák szolgáltatásokban, és a teljes környezet hello teljesítményének megértése.</span><span class="sxs-lookup"><span data-stu-id="09ce9-107">With [New Relic products](https://newrelic.com/products), you can solve app errors, stay ahead of potential issues, and understand hello performance of your entire environment.</span></span> <span data-ttu-id="09ce9-108">Tervezett toosave azonosítására és teljesítménnyel kapcsolatos problémák diagnosztizálása idő, és hello szükséges információk toosolve ezeket a problémákat ismerhet helyezi el.</span><span class="sxs-lookup"><span data-stu-id="09ce9-108">It is designed toosave you time when identifying and diagnosing performance issues, and it puts hello information you need toosolve these issues at your fingertips.</span></span>

<span data-ttu-id="09ce9-109">Új New Relic számok hello idő és a webes tranzakciók, mind a hello kiszolgáló és a felhasználók böngészőjének átviteli betöltése.</span><span class="sxs-lookup"><span data-stu-id="09ce9-109">New Relic tracks hello load time and throughput for your web transactions, both from hello server and your users' browsers.</span></span> <span data-ttu-id="09ce9-110">Hello adatbázisban, mennyi időt lassú lekérdezések elemzi, és a webes kérelmeket, biztosít hasznos üzemidő figyelési és riasztási, nyomon követi az alkalmazáskivételek és a teljes sok további mutatja.</span><span class="sxs-lookup"><span data-stu-id="09ce9-110">It shows how much time you spend in hello database, analyzes slow queries and web requests, provides uptime monitoring and alerting, tracks application exceptions, and a whole lot more.</span></span> 

## <a name="special-pricing"></a><span data-ttu-id="09ce9-111">Különleges díjszabása</span><span class="sxs-lookup"><span data-stu-id="09ce9-111">Special pricing</span></span>
<span data-ttu-id="09ce9-112">Új New Relic szabványos szabad tooAzure felhasználók.</span><span class="sxs-lookup"><span data-stu-id="09ce9-112">New Relic Standard is free tooAzure users.</span></span> <span data-ttu-id="09ce9-113">Új New Relic Pro tartományregisztráció példány mérete alapján Azure-szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="09ce9-113">New Relic Pro is offered based on instance size for Azure Cloud Services.</span></span> <span data-ttu-id="09ce9-114">Díjszabási információkért lásd: hello [New Relic lap](https://azure.microsoft.com/marketplace/partners/newrelic/newrelic/) az Azure piactér hello, vagy lépjen kapcsolatba a New Relic (sales@newrelic.com) vállalati szintű díjszabási.</span><span class="sxs-lookup"><span data-stu-id="09ce9-114">For pricing information, see hello [New Relic page](https://azure.microsoft.com/marketplace/partners/newrelic/newrelic/) in hello Azure Marketplace or contact New Relic (sales@newrelic.com) for enterprise-level pricing.</span></span>

<span data-ttu-id="09ce9-115">Azure-ügyfél két hét próba-előfizetése új New Relic Pro kapni, hello New Relic ügynök telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="09ce9-115">Azure customers receive a two week trial subscription of New Relic Pro when they deploy hello New Relic agent.</span></span>

## <a name="sign-up-for-new-relic-using-hello-azure-store"></a><span data-ttu-id="09ce9-116">Feliratkozás a New Relic hello Azure tároló használata</span><span class="sxs-lookup"><span data-stu-id="09ce9-116">Sign up for New Relic using hello Azure Store</span></span>
<span data-ttu-id="09ce9-117">Új New Relic integrálható Azure webes és feldolgozói szerepköröket.</span><span class="sxs-lookup"><span data-stu-id="09ce9-117">New Relic integrates seamlessly with Azure Web Roles and Worker roles.</span></span> <span data-ttu-id="09ce9-118">Gyorsan és egyszerűen regisztrálhat New Relic hello Azure tároló-ről.</span><span class="sxs-lookup"><span data-stu-id="09ce9-118">You can quickly and easily sign up for New Relic directly from hello Azure Store.</span></span> <span data-ttu-id="09ce9-119">Útmutatásért lásd: hello [Azure-előfizetési utasításokat tárolására](https://docs.newrelic.com/docs/agents/net-agent/azure-installation/azure-cloud-services#signup) a New Relic.</span><span class="sxs-lookup"><span data-stu-id="09ce9-119">For instructions, see hello [Azure store sign-up instructions](https://docs.newrelic.com/docs/agents/net-agent/azure-installation/azure-cloud-services#signup) from New Relic.</span></span>

## <a name="view-your-data"></a><span data-ttu-id="09ce9-120">Az adatok megtekintése</span><span class="sxs-lookup"><span data-stu-id="09ce9-120">View your data</span></span>
<span data-ttu-id="09ce9-121">Most jelentkezett, miután új New Relic elképesztő alkalmazás figyelése és az adatvezérelt analysis is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="09ce9-121">Once you've signed up, you can take advantage of New Relic's amazing application monitoring and data-driven analysis.</span></span> <span data-ttu-id="09ce9-122">toocheck az alkalmazás teljesítményének a New Relic:</span><span class="sxs-lookup"><span data-stu-id="09ce9-122">toocheck your app's performance in New Relic:</span></span>

1. <span data-ttu-id="09ce9-123">Hello Azure portált válassza ki kezelése.</span><span class="sxs-lookup"><span data-stu-id="09ce9-123">From hello Azure Portal, select Manage.</span></span>
2. <span data-ttu-id="09ce9-124">Jelentkezzen be a New Relic fiók e-mailek és a jelszót.</span><span class="sxs-lookup"><span data-stu-id="09ce9-124">Sign in with your New Relic account email and password.</span></span>
3. <span data-ttu-id="09ce9-125">Válassza ki az alkalmazást a hello alkalmazás index tooview az alkalmazás adatokat a hello [APM – áttekintés oldalra](https://docs.newrelic.com/docs/apm/applications-menu/monitoring/apm-overview-page).</span><span class="sxs-lookup"><span data-stu-id="09ce9-125">Select your app from hello Application index tooview all your app's data in hello [APM overview page](https://docs.newrelic.com/docs/apm/applications-menu/monitoring/apm-overview-page).</span></span>

## <a name="using-new-relic-with-azure"></a><span data-ttu-id="09ce9-126">Új New Relic használata az Azure-ral</span><span class="sxs-lookup"><span data-stu-id="09ce9-126">Using New Relic with Azure</span></span>
<span data-ttu-id="09ce9-127">New Relic és az Azure használatával kapcsolatos további információkért lásd: [New Relic-dokumentációs oldalát](https://docs.newrelic.com/docs/agents/net-agent/azure-installation), többek között a következőket:</span><span class="sxs-lookup"><span data-stu-id="09ce9-127">For more information about using New Relic and Azure, see [New Relic's Documentation site](https://docs.newrelic.com/docs/agents/net-agent/azure-installation), including:</span></span> 

* [<span data-ttu-id="09ce9-128">Új New Relic .NET-keretrendszerhez készült</span><span class="sxs-lookup"><span data-stu-id="09ce9-128">New Relic for .NET</span></span>](https://docs.newrelic.com/docs/agents/net-agent/getting-started/new-relic-net)
* [<span data-ttu-id="09ce9-129">Azure Cloud Service .NET feldolgozói szerepkörök állíthatnak</span><span class="sxs-lookup"><span data-stu-id="09ce9-129">Instrument a .NET Worker Role on Azure Cloud service</span></span>](https://docs.newrelic.com/docs/agents/net-agent/azure-installation/instrument-net-worker-role-azure-cloud-service)
* [<span data-ttu-id="09ce9-130">Új New Relic hello Microsoft Azure Cloud platform</span><span class="sxs-lookup"><span data-stu-id="09ce9-130">New Relic for hello Microsoft Azure Cloud platform</span></span>](https://docs.newrelic.com/docs/agents/net-agent/azure-installation/azure-cloud-services)
* [<span data-ttu-id="09ce9-131">Új New Relic a hello Microsoft Azure App Service szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="09ce9-131">New Relic for hello Microsoft Azure App Services</span></span>](https://docs.newrelic.com/docs/agents/net-agent/azure-installation/azure-portal)
* [<span data-ttu-id="09ce9-132">Új New Relic vagy az Azure hibaelhárítási</span><span class="sxs-lookup"><span data-stu-id="09ce9-132">New Relic/Azure troubleshooting</span></span>](https://docs.newrelic.com/docs/agents/net-agent/azure-troubleshooting)

