---
title: "aaaAzure esemény rács áttekintése"
description: "Azure Event rács és a fogalmakat ismerteti."
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 08/18/2017
ms.author: babanisa
ms.openlocfilehash: 95dce22e9335df88e81b134143a6c14994c26b8c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="an-introduction-tooazure-event-grid"></a><span data-ttu-id="2377d-103">Egy bevezető tooAzure esemény rács</span><span class="sxs-lookup"><span data-stu-id="2377d-103">An introduction tooAzure Event Grid</span></span>

<span data-ttu-id="2377d-104">Azure esemény rács tooeasily build alkalmazások esemény-alapú architektúra lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="2377d-104">Azure Event Grid allows you tooeasily build applications with event-based architectures.</span></span> <span data-ttu-id="2377d-105">Azure-erőforrás volna, például a toosubscribe, és adjon meg hello eseménykezelő vagy WebHook végpont toosend hello esemény hello választja.</span><span class="sxs-lookup"><span data-stu-id="2377d-105">You select hello Azure resource you would like toosubscribe to, and give hello event handler or WebHook endpoint toosend hello event to.</span></span> <span data-ttu-id="2377d-106">Esemény rács rendelkezik beépített támogatása az Azure-szolgáltatások, például a tárolási BLOB és -erőforráscsoportok származó események.</span><span class="sxs-lookup"><span data-stu-id="2377d-106">Event Grid has built-in support for events coming from Azure services, like storage blobs and resource groups.</span></span> <span data-ttu-id="2377d-107">Esemény rács is rendelkeznek, alkalmazás és a külső események egyéni támogatása egyéni témakörök és egyéni webhookokkal használatával.</span><span class="sxs-lookup"><span data-stu-id="2377d-107">Event Grid also has custom support for application and third-party events, using custom topics and custom webhooks.</span></span> 

<span data-ttu-id="2377d-108">Szűrők tooroute adott események toodifferent végpontok, csoportos küldésű toomultiple végpontok használatát, és győződjön meg arról, hogy az események megbízhatóan érkeznek.</span><span class="sxs-lookup"><span data-stu-id="2377d-108">You can use filters tooroute specific events toodifferent endpoints, multicast toomultiple endpoints, and make sure your events are reliably delivered.</span></span> <span data-ttu-id="2377d-109">Esemény rács is rendelkezik beépített egyéni és külső események támogatása.</span><span class="sxs-lookup"><span data-stu-id="2377d-109">Event Grid also has built in support for custom and third-party events.</span></span>

<span data-ttu-id="2377d-110">A hello előzetes kiadását, esemény rács támogatja **westus2** és **westcentralus** helyét.</span><span class="sxs-lookup"><span data-stu-id="2377d-110">For hello preview release, Event Grid supports **westus2** and **westcentralus** locations.</span></span> <span data-ttu-id="2377d-111">Más régiókban lesz hozzáadva.</span><span class="sxs-lookup"><span data-stu-id="2377d-111">Other regions will be added.</span></span>

<span data-ttu-id="2377d-112">Ez a cikk áttekintést Azure esemény rács.</span><span class="sxs-lookup"><span data-stu-id="2377d-112">This article provides an overview of Azure Event Grid.</span></span> <span data-ttu-id="2377d-113">Ha azt szeretné, hogy az esemény rács lépései tooget, [Azure esemény rácshoz hozza létre és útvonal egyéni események](custom-event-quickstart.md).</span><span class="sxs-lookup"><span data-stu-id="2377d-113">If you want tooget started with Event Grid, see [Create and route custom events with Azure Event Grid](custom-event-quickstart.md).</span></span>

![Esemény rács működési modell](./media/overview/event-grid-functional-model.png)

<span data-ttu-id="2377d-115">A Blob Storage jelenleg nem nyilvánosan elérhető közzétevő.</span><span class="sxs-lookup"><span data-stu-id="2377d-115">Currently, Blob Storage is not publicly available as a publisher.</span></span>

## <a name="concepts"></a><span data-ttu-id="2377d-116">Alapelvek</span><span class="sxs-lookup"><span data-stu-id="2377d-116">Concepts</span></span>

<span data-ttu-id="2377d-117">Nincsenek Azure esemény rács, amelyek lehetővé teszik az induláshoz öt fogalmak:</span><span class="sxs-lookup"><span data-stu-id="2377d-117">There are five concepts in Azure Event Grid that let you get going:</span></span>

* <span data-ttu-id="2377d-118">**Események** – Mi történt.</span><span class="sxs-lookup"><span data-stu-id="2377d-118">**Events** - What happened.</span></span>
* <span data-ttu-id="2377d-119">**Esemény források-közzétevők** - Ha hello esemény került sor.</span><span class="sxs-lookup"><span data-stu-id="2377d-119">**Event sources/publishers** - Where hello event took place.</span></span>
* <span data-ttu-id="2377d-120">**Témakörök** -hello végpont, ahol közzétevők küldi az eseményeket.</span><span class="sxs-lookup"><span data-stu-id="2377d-120">**Topics** - hello endpoint where publishers send events.</span></span>
* <span data-ttu-id="2377d-121">**Esemény-előfizetések** -hello végpont vagy beépített mechanizmus tooroute események, néha toomultiple kezelők.</span><span class="sxs-lookup"><span data-stu-id="2377d-121">**Event subscriptions** - hello endpoint or built-in mechanism tooroute events, sometimes toomultiple handlers.</span></span> <span data-ttu-id="2377d-122">Előfizetések kezelők toointelligently szűrő bejövő események által is használt.</span><span class="sxs-lookup"><span data-stu-id="2377d-122">Subscriptions are also used by handlers toointelligently filter incoming events.</span></span>
* <span data-ttu-id="2377d-123">**Az eseménykezelők** – hello alkalmazás vagy szolgáltatás reakciójú toohello esemény.</span><span class="sxs-lookup"><span data-stu-id="2377d-123">**Event handlers** - hello app or service reacting toohello event.</span></span>

<span data-ttu-id="2377d-124">Ezek a fogalmak kapcsolatos további információkért lásd: [Azure esemény rácsban fogalmak](concepts.md).</span><span class="sxs-lookup"><span data-stu-id="2377d-124">For more information about these concepts, see [Concepts in Azure Event Grid](concepts.md).</span></span>

## <a name="capabilities"></a><span data-ttu-id="2377d-125">Funkciók</span><span class="sxs-lookup"><span data-stu-id="2377d-125">Capabilities</span></span>

<span data-ttu-id="2377d-126">Az alábbiakban néhány kulcsfontosságú szolgáltatásokat hello Azure esemény rács:</span><span class="sxs-lookup"><span data-stu-id="2377d-126">Here are some of hello key features of Azure Event Grid:</span></span>

* <span data-ttu-id="2377d-127">**Egyszerűség** -ponton, majd kattintson az Azure-erőforrás tooany eseménykezelő vagy a végpont tooaim események.</span><span class="sxs-lookup"><span data-stu-id="2377d-127">**Simplicity** - Point and click tooaim events from your Azure resource tooany event handler or endpoint.</span></span>
* <span data-ttu-id="2377d-128">**Speciális szűrési** -szűrőt a esemény típusa vagy esemény közzététele elérési út tooensure eseménykezelők csak a kapcsolódó eseményeket fogadni.</span><span class="sxs-lookup"><span data-stu-id="2377d-128">**Advanced filtering** - Filter on event type or event publish path tooensure event handlers only receive relevant events.</span></span>
* <span data-ttu-id="2377d-129">**Szétterítési** -előfizetés több végpontok toohello azonos esemény toosend másolja át a hello esemény tooas sok helyen igény szerint.</span><span class="sxs-lookup"><span data-stu-id="2377d-129">**Fan-out** - Subscribe multiple endpoints toohello same event toosend copies of hello event tooas many places as needed.</span></span>
* <span data-ttu-id="2377d-130">**Megbízhatóság** -24 órás újra exponenciális leállítási tooensure kézbesíti az eseményeket egy használják.</span><span class="sxs-lookup"><span data-stu-id="2377d-130">**Reliability** - Utilize 24-hour retry with exponential backoff tooensure events are delivered.</span></span>
* <span data-ttu-id="2377d-131">**Fizetési / esemény** - kell fizetnie, amennyit csak a hello összeg esemény rács használja.</span><span class="sxs-lookup"><span data-stu-id="2377d-131">**Pay-per-event** - Pay only for hello amount you use Event Grid.</span></span>
* <span data-ttu-id="2377d-132">**Magas teljesítmény** -esemény rács nagy munkaterhelések létrehozásához, támogatja a több millió esemény / másodperc.</span><span class="sxs-lookup"><span data-stu-id="2377d-132">**High throughput** - Build high-volume workloads on Event Grid with support for millions of events per second.</span></span>
* <span data-ttu-id="2377d-133">**Beépített események** - létrehozásához, és gyorsan futtató beépített események erőforrás által definiált.</span><span class="sxs-lookup"><span data-stu-id="2377d-133">**Built-in Events** - Get up and running quickly with resource-defined built-in events.</span></span>
* <span data-ttu-id="2377d-134">**Egyéni események** -esemény rács útvonal, szűrő és megbízhatóan kézbesítése egyéni események használja az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="2377d-134">**Custom Events** - use Event Grid route, filter, and reliably deliver custom events in your app.</span></span>

## <a name="built-in-publisher-and-handler-integration"></a><span data-ttu-id="2377d-135">Beépített közzétevő és kezelő integrációja</span><span class="sxs-lookup"><span data-stu-id="2377d-135">Built-in publisher and handler integration</span></span>

<span data-ttu-id="2377d-136">Azure számos szolgáltatásokkal, beleértve a közzétevők és a kezelők esemény beépített támogatást nyújt.</span><span class="sxs-lookup"><span data-stu-id="2377d-136">Azure offers built-in event support using numerous services, including both publishers and handlers.</span></span>

### <a name="publishers"></a><span data-ttu-id="2377d-137">Kiadók</span><span class="sxs-lookup"><span data-stu-id="2377d-137">Publishers</span></span>

<span data-ttu-id="2377d-138">Jelenleg hello Azure-szolgáltatásokat kell beépített publisher az esemény rács:</span><span class="sxs-lookup"><span data-stu-id="2377d-138">Currently, hello following Azure services have built-in publisher support for event grid:</span></span>

* <span data-ttu-id="2377d-139">Erőforráscsoportok (műveletek)</span><span class="sxs-lookup"><span data-stu-id="2377d-139">Resource Groups (management operations)</span></span>
* <span data-ttu-id="2377d-140">Azure-előfizetések (műveletek)</span><span class="sxs-lookup"><span data-stu-id="2377d-140">Azure Subscriptions (management operations)</span></span>
* <span data-ttu-id="2377d-141">Event Hubs</span><span class="sxs-lookup"><span data-stu-id="2377d-141">Event Hubs</span></span>
* <span data-ttu-id="2377d-142">Egyéni kapcsolatos témakörök</span><span class="sxs-lookup"><span data-stu-id="2377d-142">Custom Topics</span></span>

<span data-ttu-id="2377d-143">Más Azure-szolgáltatásokkal fog bővülni az év.</span><span class="sxs-lookup"><span data-stu-id="2377d-143">Other Azure services will be added this year.</span></span>

### <a name="handlers"></a><span data-ttu-id="2377d-144">Kivételkezelők</span><span class="sxs-lookup"><span data-stu-id="2377d-144">Handlers</span></span>

<span data-ttu-id="2377d-145">Jelenleg hello Azure-szolgáltatásokat kell beépített kezelő az esemény rács:</span><span class="sxs-lookup"><span data-stu-id="2377d-145">Currently, hello following Azure services have built-in handler support for Event Grid:</span></span> 

* <span data-ttu-id="2377d-146">Azure Functions</span><span class="sxs-lookup"><span data-stu-id="2377d-146">Azure Functions</span></span>
* <span data-ttu-id="2377d-147">Logic Apps</span><span class="sxs-lookup"><span data-stu-id="2377d-147">Logic Apps</span></span>
* <span data-ttu-id="2377d-148">Azure Automation</span><span class="sxs-lookup"><span data-stu-id="2377d-148">Azure Automation</span></span>
* <span data-ttu-id="2377d-149">Webhook</span><span class="sxs-lookup"><span data-stu-id="2377d-149">WebHooks</span></span>

<span data-ttu-id="2377d-150">Más Azure-szolgáltatásokkal fog bővülni az év.</span><span class="sxs-lookup"><span data-stu-id="2377d-150">Other Azure services will be added this year.</span></span>

## <a name="what-can-i-do-with-event-grid"></a><span data-ttu-id="2377d-151">Mi a teendő esemény rácshoz?</span><span class="sxs-lookup"><span data-stu-id="2377d-151">What can I do with Event Grid?</span></span>

<span data-ttu-id="2377d-152">Azure Event rács több képességeket kínál, amelyek jelentősen javítja a kiszolgáló nélküli, ops automatizálási és integrációs munkahelyi biztosít:</span><span class="sxs-lookup"><span data-stu-id="2377d-152">Azure Event Grid provides several capabilities that vastly improve serverless, ops automation, and integration work:</span></span> 

### <a name="serverless-application-architectures"></a><span data-ttu-id="2377d-153">Kiszolgáló nélküli alkalmazás-architektúrák</span><span class="sxs-lookup"><span data-stu-id="2377d-153">Serverless application architectures</span></span>

![Kiszolgáló nélküli alkalmazást](./media/overview/serverless_web_app.png)

<span data-ttu-id="2377d-155">Az Event Grid összeköti az adatforrásokat az eseménykezelőkkel.</span><span class="sxs-lookup"><span data-stu-id="2377d-155">Event Grid connects data sources and event handlers.</span></span> <span data-ttu-id="2377d-156">Egy kiszolgáló nélküli függvény toorun kép elemzés minden alkalommal, amikor egy új fénykép tooa blob storage tárolót fel például, használja a esemény rács tooinstantly eseményindító.</span><span class="sxs-lookup"><span data-stu-id="2377d-156">For example, use Event Grid tooinstantly trigger a serverless function toorun image analysis each time a new photo is added tooa blob storage container.</span></span> 

### <a name="ops-automation"></a><span data-ttu-id="2377d-157">OPS automatizálás</span><span class="sxs-lookup"><span data-stu-id="2377d-157">Ops Automation</span></span>

![Műveletek automatizálása](./media/overview/Ops_automation.png)

<span data-ttu-id="2377d-159">Esemény rács toospeed automatizálás lehetővé teszi, és egyszerűbb lehet a házirend betartatása.</span><span class="sxs-lookup"><span data-stu-id="2377d-159">Event Grid allows you toospeed automation and simplify policy enforcement.</span></span> <span data-ttu-id="2377d-160">Az Event Grid értesítheti például az Azure Automation szolgáltatást, amikor egy virtuális gép létrejön vagy egy SQL Database működésbe lép.</span><span class="sxs-lookup"><span data-stu-id="2377d-160">For example, Event Grid can notify Azure Automation when a virtual machine is created, or a SQL Database is spun up.</span></span> <span data-ttu-id="2377d-161">Ezek az események használható tooautomatically ellenőrizze, hogy az szolgáltatáskonfiguráció megfelelőnek metaadatok kerüljenek a műveletek eszközök, a címke virtuális gépek vagy a fájl munkaelemek.</span><span class="sxs-lookup"><span data-stu-id="2377d-161">These events can be used tooautomatically check that service configurations are compliant, put metadata into operations tools, tag virtual machines, or file work items.</span></span>

### <a name="application-integration"></a><span data-ttu-id="2377d-162">Alkalmazásintegrálás</span><span class="sxs-lookup"><span data-stu-id="2377d-162">Application integration</span></span>

![Alkalmazásintegrálás](./media/overview/app_integration.png)

<span data-ttu-id="2377d-164">Az Event Grids más szolgáltatásokkal kapcsolja össze alkalmazását.</span><span class="sxs-lookup"><span data-stu-id="2377d-164">Event Grid connects your app with other services.</span></span> <span data-ttu-id="2377d-165">Például hozzon létre egy egyéni témakör toosend az alkalmazás esemény adatok tooEvent rács, és kihasználhatják a megbízható szállítási, speciális útválasztási, és az Azure-integráció közvetlen.</span><span class="sxs-lookup"><span data-stu-id="2377d-165">For example, create a custom topic toosend your app's event data tooEvent Grid, and take advantage of its reliable delivery, advanced routing, and direct integration with Azure.</span></span> <span data-ttu-id="2377d-166">Alternatív megoldásként használhatja esemény rács adatokkal Logic Apps tooprocess bárhol, kód írása nélkül.</span><span class="sxs-lookup"><span data-stu-id="2377d-166">Alternatively, you can use Event Grid with Logic Apps tooprocess data anywhere, without writing code.</span></span> 

## <a name="how-is-event-grid-different-from-other-azure-integration-services"></a><span data-ttu-id="2377d-167">Miben különbözik az esemény rács más Azure integrációs szolgáltatások?</span><span class="sxs-lookup"><span data-stu-id="2377d-167">How is Event Grid different from other Azure integration services?</span></span>

<span data-ttu-id="2377d-168">Esemény rács egy esemény csatlakozópanel, amely lehetővé teszi a eseményvezérelt, reaktív programozási.</span><span class="sxs-lookup"><span data-stu-id="2377d-168">Event Grid is an eventing backplane that enables event-driven, reactive programming.</span></span> <span data-ttu-id="2377d-169">Szorosan integrálódik az Azure-szolgáltatásokkal, és harmadik féltől származó szolgáltatással integrálható.</span><span class="sxs-lookup"><span data-stu-id="2377d-169">It is deeply integrated with Azure services and can be integrated with third-party services.</span></span> <span data-ttu-id="2377d-170">hello eseményüzenetben a szolgáltatások és alkalmazások tooreact toochanges szükséges hello információkat.</span><span class="sxs-lookup"><span data-stu-id="2377d-170">hello event message contains hello information you need tooreact toochanges in services and applications.</span></span> <span data-ttu-id="2377d-171">Esemény rács nincs adatok folyamat, és nem kézbesíti a tényleges objektum hello frissítették.</span><span class="sxs-lookup"><span data-stu-id="2377d-171">Event Grid is not a data pipeline, and does not deliver hello actual object that was updated.</span></span>

<span data-ttu-id="2377d-172">A Service Bus kiválóan alkalmas tranzakciók, rendezés, kettős észlelés és azonnali konzisztencia igénylő hagyományos vállalati alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="2377d-172">Service Bus is well suited for traditional enterprise applications that require transactions, ordering, duplicate detection, and instantaneous consistency.</span></span> <span data-ttu-id="2377d-173">Esemény rács sebesség, a méretezés, a körének tervezték, és az alacsony költségű reaktív modellben.</span><span class="sxs-lookup"><span data-stu-id="2377d-173">Event Grid is designed for speed, scale, breadth, and low cost in a reactive model.</span></span> <span data-ttu-id="2377d-174">Kiválóan alkalmas tooserverless architektúra.</span><span class="sxs-lookup"><span data-stu-id="2377d-174">It is well suited tooserverless architecture.</span></span>

<span data-ttu-id="2377d-175">Esemény rács kiegészíti a Logic Apps és az Event Hubs például más Azure-szolgáltatásokkal.</span><span class="sxs-lookup"><span data-stu-id="2377d-175">Event Grid complements other Azure services like Logic Apps and Event Hubs.</span></span> <span data-ttu-id="2377d-176">Rács eseményindítók hello logic app toobegin munkafolyamat.</span><span class="sxs-lookup"><span data-stu-id="2377d-176">Event Grid triggers hello logic app toobegin its workflow.</span></span> <span data-ttu-id="2377d-177">Az Event Hubs azáltal, hogy engedélyezi az Event Hubs rögzítése és build bemenő és átalakítási folyamatok tooreact tooevents esemény rács működik.</span><span class="sxs-lookup"><span data-stu-id="2377d-177">Event Hubs works with Event Grid by enabling you tooreact tooevents from Event Hubs Capture, and build data ingress and transformation pipelines.</span></span>

## <a name="how-much-does-event-grid-cost"></a><span data-ttu-id="2377d-178">Milyen mértékű nem költség esemény rács?</span><span class="sxs-lookup"><span data-stu-id="2377d-178">How much does Event Grid cost?</span></span>

<span data-ttu-id="2377d-179">Azure esemény rács fizetési / esemény árképzési modellt használ, így csak a valóban használt funkciókért fizet.</span><span class="sxs-lookup"><span data-stu-id="2377d-179">Azure Event Grid uses a pay-per-event pricing model, so you only pay for what you use.</span></span>

<span data-ttu-id="2377d-180">Esemény rács költségek 0,60 $ millió műveleteket ($0,30 előzetes), és szabadon hello havonta az első 100 000 műveletet.</span><span class="sxs-lookup"><span data-stu-id="2377d-180">Event Grid costs $0.60 per million operations ($0.30 during preview) and hello first 100,000 operation per month are free.</span></span> <span data-ttu-id="2377d-181">Esemény érkező egyeznek, a kézbesítési kísérletek és a felügyeleti hívások speciális műveleteket is meg van adva.</span><span class="sxs-lookup"><span data-stu-id="2377d-181">Operations are defined as event ingress, advanced match, delivery attempt, and management calls.</span></span>  <span data-ttu-id="2377d-182">További részletek megtalálhatók a hello [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/event-grid/).</span><span class="sxs-lookup"><span data-stu-id="2377d-182">More details can be found on hello [pricing page](https://azure.microsoft.com/pricing/details/event-grid/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="2377d-183">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2377d-183">Next steps</span></span>

* <span data-ttu-id="2377d-184">[Hozzon létre, és feliratkozhat toocustom események](custom-event-quickstart.md) ugrani, és a saját egyéni események tooany végpont hello Azure esemény rács gyors üzembe helyezés küldésének megkezdése.</span><span class="sxs-lookup"><span data-stu-id="2377d-184">[Create and subscribe toocustom events](custom-event-quickstart.md) Jump right in and start sending your own custom events tooany endpoint using hello Azure Event Grid quickstart.</span></span>
* <span data-ttu-id="2377d-185">[A Logic Apps használatával eseménykezelő](monitor-virtual-machine-changes-event-grid-logic-app.md) használatával Logic Apps tooreact tooevents az alkalmazás elkészítése az oktatóanyag leküldött esemény rács által.</span><span class="sxs-lookup"><span data-stu-id="2377d-185">[Using Logic Apps as an Event Handler](monitor-virtual-machine-changes-event-grid-logic-app.md) A tutorial on building an app using Logic Apps tooreact tooevents pushed by Event Grid.</span></span>
* [<span data-ttu-id="2377d-186">Esemény rács REST API-referencia</span><span class="sxs-lookup"><span data-stu-id="2377d-186">Event Grid REST API reference</span></span>](/rest/api/eventgrid)  
  <span data-ttu-id="2377d-187">Hello Azure esemény rács kapcsolatos további műszaki információkat, valamint egy hivatkozást biztosít esemény-előfizetések kezeléséhez az Útválasztás és a szűrés.</span><span class="sxs-lookup"><span data-stu-id="2377d-187">Provides more technical information about hello Azure Event Grid, and a reference for managing Event Subscriptions, routing, and filtering.</span></span>
