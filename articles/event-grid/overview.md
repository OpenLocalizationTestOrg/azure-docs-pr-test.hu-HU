---
title: "Az Azure Event rács áttekintése"
description: "Azure Event rács és a fogalmakat ismerteti."
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 08/18/2017
ms.author: babanisa
ms.openlocfilehash: 59a834f32793e349d5639baf3c80dbcba274dfa8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="an-introduction-to-azure-event-grid"></a><span data-ttu-id="b644a-103">Azure Event rács bemutatása</span><span class="sxs-lookup"><span data-stu-id="b644a-103">An introduction to Azure Event Grid</span></span>

<span data-ttu-id="b644a-104">Az Azure Event rács lehetővé teszi az architektúrák esemény-alapú alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="b644a-104">Azure Event Grid allows you to easily build applications with event-based architectures.</span></span> <span data-ttu-id="b644a-105">Az Azure-erőforrás szeretne előfizetni, és adja meg a eseménykezelő vagy az esemény küldése a WebHook végpont választja.</span><span class="sxs-lookup"><span data-stu-id="b644a-105">You select the Azure resource you would like to subscribe to, and give the event handler or WebHook endpoint to send the event to.</span></span> <span data-ttu-id="b644a-106">Esemény rács rendelkezik beépített támogatása az Azure-szolgáltatások, például a tárolási BLOB és -erőforráscsoportok származó események.</span><span class="sxs-lookup"><span data-stu-id="b644a-106">Event Grid has built-in support for events coming from Azure services, like storage blobs and resource groups.</span></span> <span data-ttu-id="b644a-107">Esemény rács is rendelkeznek, alkalmazás és a külső események egyéni támogatása egyéni témakörök és egyéni webhookokkal használatával.</span><span class="sxs-lookup"><span data-stu-id="b644a-107">Event Grid also has custom support for application and third-party events, using custom topics and custom webhooks.</span></span> 

<span data-ttu-id="b644a-108">Szűrők segítségével meghatározott események különböző végpontokhoz, több végpontot, csoportos küldéssel történő továbbításához, és győződjön meg arról, hogy az események megbízhatóan érkeznek.</span><span class="sxs-lookup"><span data-stu-id="b644a-108">You can use filters to route specific events to different endpoints, multicast to multiple endpoints, and make sure your events are reliably delivered.</span></span> <span data-ttu-id="b644a-109">Esemény rács is rendelkezik beépített egyéni és külső események támogatása.</span><span class="sxs-lookup"><span data-stu-id="b644a-109">Event Grid also has built in support for custom and third-party events.</span></span>

<span data-ttu-id="b644a-110">Az előzetes verzió esetén az Event Grid a **westus2** és a **westcentralus** helyet támogatja.</span><span class="sxs-lookup"><span data-stu-id="b644a-110">For the preview release, Event Grid supports **westus2** and **westcentralus** locations.</span></span> <span data-ttu-id="b644a-111">Más régiókban lesz hozzáadva.</span><span class="sxs-lookup"><span data-stu-id="b644a-111">Other regions will be added.</span></span>

<span data-ttu-id="b644a-112">Ez a cikk áttekintést Azure esemény rács.</span><span class="sxs-lookup"><span data-stu-id="b644a-112">This article provides an overview of Azure Event Grid.</span></span> <span data-ttu-id="b644a-113">Ha azt szeretné, esemény rács használatába, lásd: [Azure esemény rácshoz hozza létre és útvonal egyéni események](custom-event-quickstart.md).</span><span class="sxs-lookup"><span data-stu-id="b644a-113">If you want to get started with Event Grid, see [Create and route custom events with Azure Event Grid](custom-event-quickstart.md).</span></span>

![Esemény rács működési modell](./media/overview/event-grid-functional-model.png)

<span data-ttu-id="b644a-115">A Blob Storage jelenleg nem nyilvánosan elérhető közzétevő.</span><span class="sxs-lookup"><span data-stu-id="b644a-115">Currently, Blob Storage is not publicly available as a publisher.</span></span>

## <a name="concepts"></a><span data-ttu-id="b644a-116">Alapelvek</span><span class="sxs-lookup"><span data-stu-id="b644a-116">Concepts</span></span>

<span data-ttu-id="b644a-117">Nincsenek Azure esemény rács, amelyek lehetővé teszik az induláshoz öt fogalmak:</span><span class="sxs-lookup"><span data-stu-id="b644a-117">There are five concepts in Azure Event Grid that let you get going:</span></span>

* <span data-ttu-id="b644a-118">**Események** – Mi történt.</span><span class="sxs-lookup"><span data-stu-id="b644a-118">**Events** - What happened.</span></span>
* <span data-ttu-id="b644a-119">**Esemény források-közzétevők** – Ha az esemény történt.</span><span class="sxs-lookup"><span data-stu-id="b644a-119">**Event sources/publishers** - Where the event took place.</span></span>
* <span data-ttu-id="b644a-120">**Témakörök** -a végpont, ahol közzétevők küldi az eseményeket.</span><span class="sxs-lookup"><span data-stu-id="b644a-120">**Topics** - The endpoint where publishers send events.</span></span>
* <span data-ttu-id="b644a-121">**Esemény-előfizetések** -útvonal események, egyes esetekben több kezelők végpont vagy beépített mechanizmusa.</span><span class="sxs-lookup"><span data-stu-id="b644a-121">**Event subscriptions** - The endpoint or built-in mechanism to route events, sometimes to multiple handlers.</span></span> <span data-ttu-id="b644a-122">Előfizetések is használják kezelők ezután a bejövő események szűréséhez.</span><span class="sxs-lookup"><span data-stu-id="b644a-122">Subscriptions are also used by handlers to intelligently filter incoming events.</span></span>
* <span data-ttu-id="b644a-123">**Az eseménykezelők** – az alkalmazás vagy szolgáltatás reagálnak az esemény.</span><span class="sxs-lookup"><span data-stu-id="b644a-123">**Event handlers** - The app or service reacting to the event.</span></span>

<span data-ttu-id="b644a-124">Ezek a fogalmak kapcsolatos további információkért lásd: [Azure esemény rácsban fogalmak](concepts.md).</span><span class="sxs-lookup"><span data-stu-id="b644a-124">For more information about these concepts, see [Concepts in Azure Event Grid](concepts.md).</span></span>

## <a name="capabilities"></a><span data-ttu-id="b644a-125">Funkciók</span><span class="sxs-lookup"><span data-stu-id="b644a-125">Capabilities</span></span>

<span data-ttu-id="b644a-126">Az alábbiakban néhány fő funkciója Azure esemény rács:</span><span class="sxs-lookup"><span data-stu-id="b644a-126">Here are some of the key features of Azure Event Grid:</span></span>

* <span data-ttu-id="b644a-127">**Egyszerűség** -ponton, majd kattintson az e eseménykezelő vagy a végpont az Azure-erőforrás származó események célja.</span><span class="sxs-lookup"><span data-stu-id="b644a-127">**Simplicity** - Point and click to aim events from your Azure resource to any event handler or endpoint.</span></span>
* <span data-ttu-id="b644a-128">**Speciális szűrési** -szűrőt a esemény típusa vagy esemény közzététele elérési út annak biztosítása érdekében az eseménykezelők csak megkapják a kapcsolódó eseményeket.</span><span class="sxs-lookup"><span data-stu-id="b644a-128">**Advanced filtering** - Filter on event type or event publish path to ensure event handlers only receive relevant events.</span></span>
* <span data-ttu-id="b644a-129">**Szétterítési** -előfizetés az összes olyan helyre, szükség esetén az esemény példánya küldendő ugyanarra az eseményre több végpontot.</span><span class="sxs-lookup"><span data-stu-id="b644a-129">**Fan-out** - Subscribe multiple endpoints to the same event to send copies of the event to as many places as needed.</span></span>
* <span data-ttu-id="b644a-130">**Megbízhatóság** -24 órás újra exponenciális leállítási biztosításához kézbesíti az eseményeket egy használják.</span><span class="sxs-lookup"><span data-stu-id="b644a-130">**Reliability** - Utilize 24-hour retry with exponential backoff to ensure events are delivered.</span></span>
* <span data-ttu-id="b644a-131">**Fizetési / esemény** - kell fizetnie, amennyit csak az összeg esemény rács használja.</span><span class="sxs-lookup"><span data-stu-id="b644a-131">**Pay-per-event** - Pay only for the amount you use Event Grid.</span></span>
* <span data-ttu-id="b644a-132">**Magas teljesítmény** -esemény rács nagy munkaterhelések létrehozásához, támogatja a több millió esemény / másodperc.</span><span class="sxs-lookup"><span data-stu-id="b644a-132">**High throughput** - Build high-volume workloads on Event Grid with support for millions of events per second.</span></span>
* <span data-ttu-id="b644a-133">**Beépített események** - létrehozásához, és gyorsan futtató beépített események erőforrás által definiált.</span><span class="sxs-lookup"><span data-stu-id="b644a-133">**Built-in Events** - Get up and running quickly with resource-defined built-in events.</span></span>
* <span data-ttu-id="b644a-134">**Egyéni események** -esemény rács útvonal, szűrő és megbízhatóan kézbesítése egyéni események használja az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="b644a-134">**Custom Events** - use Event Grid route, filter, and reliably deliver custom events in your app.</span></span>

## <a name="built-in-publisher-and-handler-integration"></a><span data-ttu-id="b644a-135">Beépített közzétevő és kezelő integrációja</span><span class="sxs-lookup"><span data-stu-id="b644a-135">Built-in publisher and handler integration</span></span>

<span data-ttu-id="b644a-136">Azure számos szolgáltatásokkal, beleértve a közzétevők és a kezelők esemény beépített támogatást nyújt.</span><span class="sxs-lookup"><span data-stu-id="b644a-136">Azure offers built-in event support using numerous services, including both publishers and handlers.</span></span>

### <a name="publishers"></a><span data-ttu-id="b644a-137">Kiadók</span><span class="sxs-lookup"><span data-stu-id="b644a-137">Publishers</span></span>

<span data-ttu-id="b644a-138">Jelenleg az Azure-szolgáltatásokat kell beépített publisher az esemény rács:</span><span class="sxs-lookup"><span data-stu-id="b644a-138">Currently, the following Azure services have built-in publisher support for event grid:</span></span>

* <span data-ttu-id="b644a-139">Erőforráscsoportok (műveletek)</span><span class="sxs-lookup"><span data-stu-id="b644a-139">Resource Groups (management operations)</span></span>
* <span data-ttu-id="b644a-140">Azure-előfizetések (műveletek)</span><span class="sxs-lookup"><span data-stu-id="b644a-140">Azure Subscriptions (management operations)</span></span>
* <span data-ttu-id="b644a-141">Event Hubs</span><span class="sxs-lookup"><span data-stu-id="b644a-141">Event Hubs</span></span>
* <span data-ttu-id="b644a-142">Egyéni kapcsolatos témakörök</span><span class="sxs-lookup"><span data-stu-id="b644a-142">Custom Topics</span></span>

<span data-ttu-id="b644a-143">Más Azure-szolgáltatásokkal fog bővülni az év.</span><span class="sxs-lookup"><span data-stu-id="b644a-143">Other Azure services will be added this year.</span></span>

### <a name="handlers"></a><span data-ttu-id="b644a-144">Kivételkezelők</span><span class="sxs-lookup"><span data-stu-id="b644a-144">Handlers</span></span>

<span data-ttu-id="b644a-145">Jelenleg az Azure-szolgáltatásokat kell beépített kezelő az esemény rács:</span><span class="sxs-lookup"><span data-stu-id="b644a-145">Currently, the following Azure services have built-in handler support for Event Grid:</span></span> 

* <span data-ttu-id="b644a-146">Azure Functions</span><span class="sxs-lookup"><span data-stu-id="b644a-146">Azure Functions</span></span>
* <span data-ttu-id="b644a-147">Logic Apps</span><span class="sxs-lookup"><span data-stu-id="b644a-147">Logic Apps</span></span>
* <span data-ttu-id="b644a-148">Azure Automation</span><span class="sxs-lookup"><span data-stu-id="b644a-148">Azure Automation</span></span>
* <span data-ttu-id="b644a-149">Webhook</span><span class="sxs-lookup"><span data-stu-id="b644a-149">WebHooks</span></span>

<span data-ttu-id="b644a-150">Más Azure-szolgáltatásokkal fog bővülni az év.</span><span class="sxs-lookup"><span data-stu-id="b644a-150">Other Azure services will be added this year.</span></span>

## <a name="what-can-i-do-with-event-grid"></a><span data-ttu-id="b644a-151">Mi a teendő esemény rácshoz?</span><span class="sxs-lookup"><span data-stu-id="b644a-151">What can I do with Event Grid?</span></span>

<span data-ttu-id="b644a-152">Azure Event rács több képességeket kínál, amelyek jelentősen javítja a kiszolgáló nélküli, ops automatizálási és integrációs munkahelyi biztosít:</span><span class="sxs-lookup"><span data-stu-id="b644a-152">Azure Event Grid provides several capabilities that vastly improve serverless, ops automation, and integration work:</span></span> 

### <a name="serverless-application-architectures"></a><span data-ttu-id="b644a-153">Kiszolgáló nélküli alkalmazás-architektúrák</span><span class="sxs-lookup"><span data-stu-id="b644a-153">Serverless application architectures</span></span>

![Kiszolgáló nélküli alkalmazást](./media/overview/serverless_web_app.png)

<span data-ttu-id="b644a-155">Az Event Grid összeköti az adatforrásokat az eseménykezelőkkel.</span><span class="sxs-lookup"><span data-stu-id="b644a-155">Event Grid connects data sources and event handlers.</span></span> <span data-ttu-id="b644a-156">Az Event Grid használatával például azonnal aktiválható egy képelemzést futtató, kiszolgáló nélküli funkció, valahányszor új kép kerül egy Blob Storage-tárolóba.</span><span class="sxs-lookup"><span data-stu-id="b644a-156">For example, use Event Grid to instantly trigger a serverless function to run image analysis each time a new photo is added to a blob storage container.</span></span> 

### <a name="ops-automation"></a><span data-ttu-id="b644a-157">OPS automatizálás</span><span class="sxs-lookup"><span data-stu-id="b644a-157">Ops Automation</span></span>

![Műveletek automatizálása](./media/overview/Ops_automation.png)

<span data-ttu-id="b644a-159">Az Event Grid lehetővé teszi az automatizálás felgyorsítását és a szabályzatok érvényesítésének egyszerűsítését.</span><span class="sxs-lookup"><span data-stu-id="b644a-159">Event Grid allows you to speed automation and simplify policy enforcement.</span></span> <span data-ttu-id="b644a-160">Az Event Grid értesítheti például az Azure Automation szolgáltatást, amikor egy virtuális gép létrejön vagy egy SQL Database működésbe lép.</span><span class="sxs-lookup"><span data-stu-id="b644a-160">For example, Event Grid can notify Azure Automation when a virtual machine is created, or a SQL Database is spun up.</span></span> <span data-ttu-id="b644a-161">Ezek az események felhasználhatók a szolgáltatáskonfigurációk megfelelőségének automatikus ellenőrzésére, metaadatok műveleti eszközöknek való átadására, és virtuális gépek vagy munkatétel-fájlok címkézésére.</span><span class="sxs-lookup"><span data-stu-id="b644a-161">These events can be used to automatically check that service configurations are compliant, put metadata into operations tools, tag virtual machines, or file work items.</span></span>

### <a name="application-integration"></a><span data-ttu-id="b644a-162">Alkalmazásintegrálás</span><span class="sxs-lookup"><span data-stu-id="b644a-162">Application integration</span></span>

![Alkalmazásintegrálás](./media/overview/app_integration.png)

<span data-ttu-id="b644a-164">Az Event Grids más szolgáltatásokkal kapcsolja össze alkalmazását.</span><span class="sxs-lookup"><span data-stu-id="b644a-164">Event Grid connects your app with other services.</span></span> <span data-ttu-id="b644a-165">Például hozzon létre egy egyéni témakör az alkalmazás esemény adatokat küldeni a rács esemény, és kihasználhatják a megbízható szállítási, speciális útválasztási, és közvetlen integráció az Azure-ral.</span><span class="sxs-lookup"><span data-stu-id="b644a-165">For example, create a custom topic to send your app's event data to Event Grid, and take advantage of its reliable delivery, advanced routing, and direct integration with Azure.</span></span> <span data-ttu-id="b644a-166">Úgy is dönthet, hogy az Event Gridet a Logic Apps-szel együtt használ adatfeldolgozásra tetszőleges helyen, kód írása nélkül.</span><span class="sxs-lookup"><span data-stu-id="b644a-166">Alternatively, you can use Event Grid with Logic Apps to process data anywhere, without writing code.</span></span> 

## <a name="how-is-event-grid-different-from-other-azure-integration-services"></a><span data-ttu-id="b644a-167">Miben különbözik az esemény rács más Azure integrációs szolgáltatások?</span><span class="sxs-lookup"><span data-stu-id="b644a-167">How is Event Grid different from other Azure integration services?</span></span>

<span data-ttu-id="b644a-168">Esemény rács egy esemény csatlakozópanel, amely lehetővé teszi a eseményvezérelt, reaktív programozási.</span><span class="sxs-lookup"><span data-stu-id="b644a-168">Event Grid is an eventing backplane that enables event-driven, reactive programming.</span></span> <span data-ttu-id="b644a-169">Szorosan integrálódik az Azure-szolgáltatásokkal, és harmadik féltől származó szolgáltatással integrálható.</span><span class="sxs-lookup"><span data-stu-id="b644a-169">It is deeply integrated with Azure services and can be integrated with third-party services.</span></span> <span data-ttu-id="b644a-170">Az eseményüzenetben a szolgáltatások és alkalmazások változásairól reagálni szükséges információkat.</span><span class="sxs-lookup"><span data-stu-id="b644a-170">The event message contains the information you need to react to changes in services and applications.</span></span> <span data-ttu-id="b644a-171">Esemény rács nincs adatok folyamat, és nem kézbesíti a tényleges objektum, amely frissítve lett.</span><span class="sxs-lookup"><span data-stu-id="b644a-171">Event Grid is not a data pipeline, and does not deliver the actual object that was updated.</span></span>

<span data-ttu-id="b644a-172">A Service Bus kiválóan alkalmas tranzakciók, rendezés, kettős észlelés és azonnali konzisztencia igénylő hagyományos vállalati alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="b644a-172">Service Bus is well suited for traditional enterprise applications that require transactions, ordering, duplicate detection, and instantaneous consistency.</span></span> <span data-ttu-id="b644a-173">Esemény rács sebesség, a méretezés, a körének tervezték, és az alacsony költségű reaktív modellben.</span><span class="sxs-lookup"><span data-stu-id="b644a-173">Event Grid is designed for speed, scale, breadth, and low cost in a reactive model.</span></span> <span data-ttu-id="b644a-174">Kiválóan alkalmas a kiszolgáló nélküli architektúra.</span><span class="sxs-lookup"><span data-stu-id="b644a-174">It is well suited to serverless architecture.</span></span>

<span data-ttu-id="b644a-175">Esemény rács kiegészíti a Logic Apps és az Event Hubs például más Azure-szolgáltatásokkal.</span><span class="sxs-lookup"><span data-stu-id="b644a-175">Event Grid complements other Azure services like Logic Apps and Event Hubs.</span></span> <span data-ttu-id="b644a-176">Esemény rács váltja ki a logikai alkalmazást a munkafolyamat megkezdéséhez.</span><span class="sxs-lookup"><span data-stu-id="b644a-176">Event Grid triggers the logic app to begin its workflow.</span></span> <span data-ttu-id="b644a-177">Az Event Hubs azáltal, hogy engedélyezi az események reagálnak az Event Hubs rögzítése, és állítsa be adatok bemenő és átalakítási folyamatok esemény rács működik.</span><span class="sxs-lookup"><span data-stu-id="b644a-177">Event Hubs works with Event Grid by enabling you to react to events from Event Hubs Capture, and build data ingress and transformation pipelines.</span></span>

## <a name="how-much-does-event-grid-cost"></a><span data-ttu-id="b644a-178">Milyen mértékű nem költség esemény rács?</span><span class="sxs-lookup"><span data-stu-id="b644a-178">How much does Event Grid cost?</span></span>

<span data-ttu-id="b644a-179">Azure esemény rács fizetési / esemény árképzési modellt használ, így csak a valóban használt funkciókért fizet.</span><span class="sxs-lookup"><span data-stu-id="b644a-179">Azure Event Grid uses a pay-per-event pricing model, so you only pay for what you use.</span></span>

<span data-ttu-id="b644a-180">Esemény rács költségek 0,60 $ millió műveleteket ($0,30 előzetes), és szabadon havonta az első 100 000 műveletet.</span><span class="sxs-lookup"><span data-stu-id="b644a-180">Event Grid costs $0.60 per million operations ($0.30 during preview) and the first 100,000 operation per month are free.</span></span> <span data-ttu-id="b644a-181">Esemény érkező egyeznek, a kézbesítési kísérletek és a felügyeleti hívások speciális műveleteket is meg van adva.</span><span class="sxs-lookup"><span data-stu-id="b644a-181">Operations are defined as event ingress, advanced match, delivery attempt, and management calls.</span></span>  <span data-ttu-id="b644a-182">További részletek találhatók a [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/event-grid/).</span><span class="sxs-lookup"><span data-stu-id="b644a-182">More details can be found on the [pricing page](https://azure.microsoft.com/pricing/details/event-grid/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="b644a-183">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b644a-183">Next steps</span></span>

* <span data-ttu-id="b644a-184">[Hozzon létre és egyéni események előfizetni](custom-event-quickstart.md) ugrani, és indítsa el a saját egyéni események küldése az Azure Event rács gyors üzembe helyezés bármely végpont.</span><span class="sxs-lookup"><span data-stu-id="b644a-184">[Create and subscribe to custom events](custom-event-quickstart.md) Jump right in and start sending your own custom events to any endpoint using the Azure Event Grid quickstart.</span></span>
* <span data-ttu-id="b644a-185">[A Logic Apps használatával eseménykezelő](monitor-virtual-machine-changes-event-grid-logic-app.md) Logic Apps segítségével események reagálni az alkalmazás elkészítése az oktatóanyag leküldött esemény rács által.</span><span class="sxs-lookup"><span data-stu-id="b644a-185">[Using Logic Apps as an Event Handler](monitor-virtual-machine-changes-event-grid-logic-app.md) A tutorial on building an app using Logic Apps to react to events pushed by Event Grid.</span></span>
* [<span data-ttu-id="b644a-186">Esemény rács REST API-referencia</span><span class="sxs-lookup"><span data-stu-id="b644a-186">Event Grid REST API reference</span></span>](/rest/api/eventgrid)  
  <span data-ttu-id="b644a-187">Esemény-előfizetések kezelése az Azure Event rács, valamint egy hivatkozást kapcsolatos további műszaki információkat biztosít az Útválasztás és a szűrés.</span><span class="sxs-lookup"><span data-stu-id="b644a-187">Provides more technical information about the Azure Event Grid, and a reference for managing Event Subscriptions, routing, and filtering.</span></span>