---
title: "aaaPricing & számlázási - Azure Logic Apps |} Microsoft Docs"
description: "Ismerje meg, az Azure Logic Apps árak és számlázás működése."
author: kevinlam1
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: f8f528f5-51c5-4006-b571-54ef74532f32
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: LADocs; klam
ms.openlocfilehash: fa10cbbf7657afba7fadb7c76817b7a5e4af7b42
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="logic-apps-pricing-model"></a><span data-ttu-id="1bb55-103">Logic Apps díjszabási modell</span><span class="sxs-lookup"><span data-stu-id="1bb55-103">Logic Apps pricing model</span></span>
<span data-ttu-id="1bb55-104">Az Azure Logic Apps tooscale lehetővé teszi, majd hajtsa végre az integráció munkafolyamatok hello felhőben.</span><span class="sxs-lookup"><span data-stu-id="1bb55-104">Azure Logic Apps allows you tooscale and execute integration workflows in hello cloud.</span></span>  <span data-ttu-id="1bb55-105">Az alábbiakban a számlázási és a Logic Apps díjszabások hello részletei.</span><span class="sxs-lookup"><span data-stu-id="1bb55-105">Following are details on hello billing and pricing plans for Logic Apps.</span></span>
## <a name="consumption-pricing"></a><span data-ttu-id="1bb55-106">Felhasználás díjszabása</span><span class="sxs-lookup"><span data-stu-id="1bb55-106">Consumption pricing</span></span>
<span data-ttu-id="1bb55-107">Az újonnan létrehozott Logic Apps a fogyasztás csomag használata.</span><span class="sxs-lookup"><span data-stu-id="1bb55-107">Newly created Logic Apps use a consumption plan.</span></span> <span data-ttu-id="1bb55-108">Hello Logic Apps fogyasztás árképzési modellt csak kell fizetnie a valóban használt funkciókért.</span><span class="sxs-lookup"><span data-stu-id="1bb55-108">With hello Logic Apps consumption pricing model, you only pay for what you use.</span></span>  <span data-ttu-id="1bb55-109">A Logic Apps nem szabályozott fogyasztás terv használatakor.</span><span class="sxs-lookup"><span data-stu-id="1bb55-109">Logic Apps are not throttled when using a consumption plan.</span></span>
<span data-ttu-id="1bb55-110">A logic app-példány futását végrehajtott összes műveletek mérése.</span><span class="sxs-lookup"><span data-stu-id="1bb55-110">All actions executed in a run of a logic app instance are metered.</span></span>
### <a name="what-are-action-executions"></a><span data-ttu-id="1bb55-111">Mik azok a műveleti végrehajtások?</span><span class="sxs-lookup"><span data-stu-id="1bb55-111">What are action executions?</span></span>
<span data-ttu-id="1bb55-112">A logic app-definíciót minden lépése egy művelet, beleértve eseményindítók, ellenőrzési folyamata lépéseket mint állapotától függően a hatókört, minden egyes hurkok hurkok, amíg meghívja tooconnectors és hívások toonative műveletek.</span><span class="sxs-lookup"><span data-stu-id="1bb55-112">Every step in a logic app definition is an action, which includes triggers, control flow steps like conditions, scopes, for each loops, do until loops, calls tooconnectors and calls toonative actions.</span></span>
<span data-ttu-id="1bb55-113">Eseményindítók olyan speciális művelet, a rendszer tervezett tooinstantiate logikai alkalmazás új példánya, egy adott esemény bekövetkezésekor.</span><span class="sxs-lookup"><span data-stu-id="1bb55-113">Triggers are special actions that are designed tooinstantiate a new instance of a logic app when a particular event occurs.</span></span>  <span data-ttu-id="1bb55-114">Nincsenek eseményindítók, amely befolyásolhatja, hogyan hello logikai alkalmazás forgalmi díjas számos különböző beállításokat.</span><span class="sxs-lookup"><span data-stu-id="1bb55-114">There are several different behaviors for triggers, which could affect how hello logic app is metered.</span></span>
* <span data-ttu-id="1bb55-115">**Lekérdezési eseményindító** – ehhez az eseményindítóhoz folyamatosan lekérdezi a végpont, amíg nem kap egy üzenetet, amely eleget tesz a hello feltételek megadása a logikai alkalmazás példányának létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="1bb55-115">**Polling trigger** – this trigger continually polls an endpoint until it receives a message that satisfies hello criteria for creating an instance of a logic app.</span></span>  <span data-ttu-id="1bb55-116">a Logic App Designer hello hello eseményindító hello lekérdezési időközt konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="1bb55-116">hello polling interval can be configured in hello trigger in hello Logic App Designer.</span></span>  <span data-ttu-id="1bb55-117">Egyes lekérdezési kérelmek akkor is, ha nem hoz létre egy logikai alkalmazás egy példánya számít egy művelet végrehajtását.</span><span class="sxs-lookup"><span data-stu-id="1bb55-117">Each polling request, even if it doesn’t create an instance of a logic app, counts as an action execution.</span></span>
* <span data-ttu-id="1bb55-118">**Webhook eseményindító** – ehhez az eseményindítóhoz vár egy ügyfél toosend azt egy adott végpont a kérelmet.</span><span class="sxs-lookup"><span data-stu-id="1bb55-118">**Webhook trigger** – this trigger waits for a client toosend it a request on a particular endpoint.</span></span>  <span data-ttu-id="1bb55-119">Minden kérelmet küldött toohello webhook végpont számít egy művelet végrehajtását.</span><span class="sxs-lookup"><span data-stu-id="1bb55-119">Each request sent toohello webhook endpoint counts as an action execution.</span></span> <span data-ttu-id="1bb55-120">hello kérelem és a HTTP Webhook eseményindító hello is mindkét webhook eseményindítók.</span><span class="sxs-lookup"><span data-stu-id="1bb55-120">hello Request and hello HTTP Webhook trigger are both webhook triggers.</span></span>
* <span data-ttu-id="1bb55-121">**Ismétlődés eseményindító** – ehhez az eseményindítóhoz létrehoz egy új hello logikai alkalmazás hello ismétlési időköz hello eseményindító konfigurált alapján.</span><span class="sxs-lookup"><span data-stu-id="1bb55-121">**Recurrence trigger** – this trigger creates an instance of hello logic app based on hello recurrence interval configured in hello trigger.</span></span>  <span data-ttu-id="1bb55-122">Például egy ismétlődési eseményindító lehet konfigurált toorun három naponta vagy akár percenként.</span><span class="sxs-lookup"><span data-stu-id="1bb55-122">For example, a recurrence trigger can be configured toorun every three days or even every minute.</span></span>

<span data-ttu-id="1bb55-123">Eseményindító végrehajtások hello Logic Apps erőforráspanelen hello eseményindító előzmények része látható.</span><span class="sxs-lookup"><span data-stu-id="1bb55-123">Trigger executions can be seen in hello Logic Apps resource blade in hello Trigger History part.</span></span>

<span data-ttu-id="1bb55-124">Minden műveletek végrehajtódtak, egy művelet végrehajtását, hogy sikeres vagy sikertelen volt-e azok mérése.</span><span class="sxs-lookup"><span data-stu-id="1bb55-124">All actions that were executed, whether they were successful or failed are metered as an action execution.</span></span>  <span data-ttu-id="1bb55-125">A feltétel nem teljesül tooa miatt kihagyta vagy műveletekről, amelyek nem hajtható végre, mert még a befejeződése előtt megszakadt hello logikai alkalmazás nem számítanak művelet végrehajtások.</span><span class="sxs-lookup"><span data-stu-id="1bb55-125">Actions that were skipped due tooa condition not being met or actions that didn’t execute because hello logic app terminated before completion are not counted as action executions.</span></span>

<span data-ttu-id="1bb55-126">Hurkok belül végrehajtható műveletek / hello hurok iterációs bájtjai számítanak.</span><span class="sxs-lookup"><span data-stu-id="1bb55-126">Actions executed within loops are counted per iteration of hello loop.</span></span>  <span data-ttu-id="1bb55-127">Például az egyetlen művelettel egy for-each ciklus 10 elemek listája iteráció számítanak hello száma (10) hello lista elemeinek hello hurok (1) a műveletek hello számát szorozva plusz egy hello hurok hello kezdeményezése a , amely, ebben a példában lenne (10 * 1) + 1 = 11 művelet végrehajtások.</span><span class="sxs-lookup"><span data-stu-id="1bb55-127">For example, a single action in a for each loop iterating through a list of 10 items are counted as hello number of items in hello list (10) multiplied by hello number of actions in hello loop (1) plus one for hello initiation of hello loop, which, in this example, would be (10 * 1) + 1 = 11 action executions.</span></span>
<span data-ttu-id="1bb55-128">Letiltott Logic Apps nem lehet létrehozni új példányok, és ezért le van tiltva, miközben nem számolnak.</span><span class="sxs-lookup"><span data-stu-id="1bb55-128">Disabled Logic Apps cannot have new instances instantiated and therefore, while disabled, are not charged.</span></span>  <span data-ttu-id="1bb55-129">Kell szem előtt tartva, hogy a logikai alkalmazás letiltását követően is eltarthat egy kis időt hello példányok tooquiesce előtt teljesen le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="1bb55-129">Be mindful that after disabling a logic app it may take a little time for hello instances tooquiesce before being completely disabled.</span></span>
### <a name="integration-account-usage"></a><span data-ttu-id="1bb55-130">Integráció fiók használata</span><span class="sxs-lookup"><span data-stu-id="1bb55-130">Integration Account Usage</span></span>
<span data-ttu-id="1bb55-131">Használati fogyasztás alapján szereplő van egy [integrációs fiók](logic-apps-enterprise-integration-create-integration-account.md) feltárása, fejlesztési és tesztelési célra, hogy lehetővé teszi a toouse hello [B2B/EDI](logic-apps-enterprise-integration-b2b.md) és [XML-feldolgozás](logic-apps-enterprise-integration-xml.md)funkciók a Logic Apps minden további költség nélkül.</span><span class="sxs-lookup"><span data-stu-id="1bb55-131">Included in consumption-based usage is an [integration account](logic-apps-enterprise-integration-create-integration-account.md) for exploration, development, and testing purposes allowing you toouse hello [B2B/EDI](logic-apps-enterprise-integration-b2b.md) and [XML processing](logic-apps-enterprise-integration-xml.md) features of Logic Apps at no additional cost.</span></span> <span data-ttu-id="1bb55-132">Képes toocreate legfeljebb egy fiók régiónként és too10 szerződések és 25 maps tárolja.</span><span class="sxs-lookup"><span data-stu-id="1bb55-132">You are able toocreate a maximum of one account per region and store up too10 Agreements and 25 maps.</span></span> <span data-ttu-id="1bb55-133">Sémákat, a tanúsítványok és a partnerek is nem határoz meg, és feltöltheti a lehető legtöbb, szükség szerint.</span><span class="sxs-lookup"><span data-stu-id="1bb55-133">Schemas, certificates, and partners have no limits and you can upload as many as you need.</span></span>

<span data-ttu-id="1bb55-134">Ezenkívül toohello felvétel integrációs fiókok felhasználás, is létrehozhat szabványos integrációs fiókok ezek a korlátozások nélkül és a szabványos Logic Apps SLA-t.</span><span class="sxs-lookup"><span data-stu-id="1bb55-134">In addition toohello inclusion of integration accounts with consumption, you can also create standard integration accounts without these limits and with our standard Logic Apps SLA.</span></span> <span data-ttu-id="1bb55-135">További információkért lásd: [Azure-beli árakról](https://azure.microsoft.com/pricing/details/logic-apps).</span><span class="sxs-lookup"><span data-stu-id="1bb55-135">For more information, see [Azure pricing](https://azure.microsoft.com/pricing/details/logic-apps).</span></span>

## <a name="app-service-plans"></a><span data-ttu-id="1bb55-136">App Service-csomagok</span><span class="sxs-lookup"><span data-stu-id="1bb55-136">App Service plans</span></span>
<span data-ttu-id="1bb55-137">A Logic apps korábban létrehozott egy App Service-csomag hivatkozó toobehave, mielőtt folytatódik.</span><span class="sxs-lookup"><span data-stu-id="1bb55-137">Logic apps previously created referencing an App Service Plan continues toobehave as before.</span></span> <span data-ttu-id="1bb55-138">Attól függően, hogy a kiválasztott hello terv szabályozott után hello előírt napi végrehajtások túllépése, de hello művelet végrehajtási mérő vannak számlázva.</span><span class="sxs-lookup"><span data-stu-id="1bb55-138">Depending on hello plan chosen, are throttled after hello prescribed daily executions are exceeded but are billed using hello action execution meter.</span></span>
<span data-ttu-id="1bb55-139">Nagyvállalati ügyfelek, amelyeknek az előfizetését, amelyek nem rendelkeznek explicit módon társított logikai alkalmazás hello toobe, az App Service-csomag része hello mennyiségek juttatás beolvasása.</span><span class="sxs-lookup"><span data-stu-id="1bb55-139">EA customers that have an App Service Plan in their subscription, which does not have toobe explicitly associated with hello Logic App, get hello included quantities benefit.</span></span>  <span data-ttu-id="1bb55-140">Például ha egy Standard App Service-csomag EA előfizetése és a logikai alkalmazás hello ugyanahhoz az előfizetéshez, majd meg nem szó, a 10 000 művelet végrehajtások napi (lásd a következő táblázat).</span><span class="sxs-lookup"><span data-stu-id="1bb55-140">For example, if you have a Standard App Service Plan in your EA subscription and a Logic App in hello same subscription then you aren't charged for 10,000 action executions per day (see following table).</span></span> 

<span data-ttu-id="1bb55-141">App Service-csomagok és a napi megengedett művelet végrehajtások:</span><span class="sxs-lookup"><span data-stu-id="1bb55-141">App Service Plans and their daily allowed action executions:</span></span>
|  | <span data-ttu-id="1bb55-142">Ingyenes/megosztott/egyszerű</span><span class="sxs-lookup"><span data-stu-id="1bb55-142">Free/Shared/Basic</span></span> | <span data-ttu-id="1bb55-143">Standard</span><span class="sxs-lookup"><span data-stu-id="1bb55-143">Standard</span></span> | <span data-ttu-id="1bb55-144">Prémium</span><span class="sxs-lookup"><span data-stu-id="1bb55-144">Premium</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="1bb55-145">A művelet végrehajtások száma naponta</span><span class="sxs-lookup"><span data-stu-id="1bb55-145">Action executions per day</span></span> |<span data-ttu-id="1bb55-146">200</span><span class="sxs-lookup"><span data-stu-id="1bb55-146">200</span></span> |<span data-ttu-id="1bb55-147">10,000</span><span class="sxs-lookup"><span data-stu-id="1bb55-147">10,000</span></span> |<span data-ttu-id="1bb55-148">50,000</span><span class="sxs-lookup"><span data-stu-id="1bb55-148">50,000</span></span> |
### <a name="convert-from-app-service-plan-pricing-tooconsumption"></a><span data-ttu-id="1bb55-149">App Service-csomag tooConsumption árképzési konvertálása</span><span class="sxs-lookup"><span data-stu-id="1bb55-149">Convert from App Service Plan pricing tooConsumption</span></span>
<span data-ttu-id="1bb55-150">toochange, amely rendelkezik az App Service-csomag társított logikai alkalmazás tooa fogyasztás modell eltávolítása hello hivatkozás toohello App Service-csomag a hello Logic App-definíciót.</span><span class="sxs-lookup"><span data-stu-id="1bb55-150">toochange a Logic App that has an App Service Plan associated with it tooa consumption model, remove hello reference toohello App Service Plan in hello Logic App definition.</span></span>  <span data-ttu-id="1bb55-151">Ez a módosítás nem végezhető hívás tooa PowerShell-parancsmagot:`Set-AzureRmLogicApp -ResourceGroupName ‘rgname’ -Name ‘wfname’ –UseConsumptionModel -Force`</span><span class="sxs-lookup"><span data-stu-id="1bb55-151">This change can be done with a call tooa PowerShell cmdlet: `Set-AzureRmLogicApp -ResourceGroupName ‘rgname’ -Name ‘wfname’ –UseConsumptionModel -Force`</span></span>
## <a name="pricing"></a><span data-ttu-id="1bb55-152">Díjszabás</span><span class="sxs-lookup"><span data-stu-id="1bb55-152">Pricing</span></span>
<span data-ttu-id="1bb55-153">Díjszabása, lásd: [Logic Apps árképzési](https://azure.microsoft.com/pricing/details/logic-apps).</span><span class="sxs-lookup"><span data-stu-id="1bb55-153">For pricing details, see [Logic Apps Pricing](https://azure.microsoft.com/pricing/details/logic-apps).</span></span>

## <a name="next-steps"></a><span data-ttu-id="1bb55-154">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1bb55-154">Next steps</span></span>
* <span data-ttu-id="1bb55-155">[A Logic Apps áttekintése][whatis]</span><span class="sxs-lookup"><span data-stu-id="1bb55-155">[An overview of Logic Apps][whatis]</span></span>
* <span data-ttu-id="1bb55-156">[Az első logikai alkalmazás létrehozása][create]</span><span class="sxs-lookup"><span data-stu-id="1bb55-156">[Create your first logic app][create]</span></span>

[pricing]: https://azure.microsoft.com/pricing/details/logic-apps/
[whatis]: logic-apps-what-are-logic-apps.md
[create]: logic-apps-create-a-logic-app.md
