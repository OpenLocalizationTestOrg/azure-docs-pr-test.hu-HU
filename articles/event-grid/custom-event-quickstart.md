---
title: "Egyéni események az Azure Event Gridhez | Microsoft Docs"
description: "Az Azure Event Griddel közzétehet egy témakört, és feliratkozhat a kapcsolódó eseményre."
services: event-grid
keywords: 
author: djrosanova
ms.author: darosa
ms.date: 08/15/2017
ms.topic: hero-article
ms.service: event-grid
ms.openlocfilehash: 0290836bebadb20085a3ce84dddc088c3af385da
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-and-route-custom-events-with-azure-event-grid"></a><span data-ttu-id="d45b8-103">Egyéni események létrehozása és átirányítása az Azure Event Griddel</span><span class="sxs-lookup"><span data-stu-id="d45b8-103">Create and route custom events with Azure Event Grid</span></span>

<span data-ttu-id="d45b8-104">Az Azure Event Grid egy felhőalapú eseménykezelési szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="d45b8-104">Azure Event Grid is an eventing service for the cloud.</span></span> <span data-ttu-id="d45b8-105">Ebben a cikkben létrehozunk egy egyéni témakört az Azure CLI-vel, feliratkozunk a témakörre, majd elindítjuk az eseményt az eredmény megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="d45b8-105">In this article, you use the Azure CLI to create a custom topic, subscribe to the topic, and trigger the event to view the result.</span></span> <span data-ttu-id="d45b8-106">Általában olyan végpontoknak szoktunk eseményeket küldeni, amelyek reagálnak az eseményre, például egy webhooknak vagy egy Azure-függvénynek.</span><span class="sxs-lookup"><span data-stu-id="d45b8-106">Typically, you send events to an endpoint that responds to the event, such as, a webhook or Azure Function.</span></span> <span data-ttu-id="d45b8-107">A cikk egyszerűsítése érdekében azonban az eseményeket egy olyan URL-címnek küldjük el, amely pusztán üzenetek gyűjtésével foglalkozik.</span><span class="sxs-lookup"><span data-stu-id="d45b8-107">However, to simplify this article, you send the events to a URL that merely collects the messages.</span></span> <span data-ttu-id="d45b8-108">Az URL-címet a [RequestBin](https://requestb.in/) nevű nyílt forráskódú, külső fél által biztosított eszközzel fogjuk létrehozni.</span><span class="sxs-lookup"><span data-stu-id="d45b8-108">You create this URL by using an open source, third-party tool called [RequestBin](https://requestb.in/).</span></span>

>[!NOTE]
><span data-ttu-id="d45b8-109">A **RequestBin** egy nyílt forráskódú eszköz, amely nagy teljesítményt megkövetelő használati területekhez nem alkalmas.</span><span class="sxs-lookup"><span data-stu-id="d45b8-109">**RequestBin** is an open source tool that is not intended for high throughput usage.</span></span> <span data-ttu-id="d45b8-110">Az eszköz használata ebben az esetben kizárólag bemutató célt szolgál.</span><span class="sxs-lookup"><span data-stu-id="d45b8-110">The use of the tool here is purely demonstrative.</span></span> <span data-ttu-id="d45b8-111">Ha egyszerre több eseményt továbbít, lehetséges, hogy az eszközben nem fog megjelenni az összes esemény.</span><span class="sxs-lookup"><span data-stu-id="d45b8-111">If you push more than one event at a time, you might not see all of your events in the tool.</span></span>

<span data-ttu-id="d45b8-112">A folyamat végén látni fogja, hogy az eseményadatokat egy végpontnak küldte el a rendszer.</span><span class="sxs-lookup"><span data-stu-id="d45b8-112">When you are finished, you see that the event data has been sent to an endpoint.</span></span>

![Eseményadatok](./media/custom-event-quickstart/request-result.png)

[!INCLUDE [quickstarts-free-trial-note.md](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="d45b8-114">Ha a CLI helyi telepítését és használatát választja, akkor ehhez a cikkhez az Azure CLI legújabb verzióját (2.0.14 vagy újabb) kell futtatnia.</span><span class="sxs-lookup"><span data-stu-id="d45b8-114">If you choose to install and use the CLI locally, this article requires that you are running the latest version of Azure CLI (2.0.14 or later).</span></span> <span data-ttu-id="d45b8-115">A verzió megkereséséhez futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="d45b8-115">To find the version, run `az --version`.</span></span> <span data-ttu-id="d45b8-116">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="d45b8-116">If you need to install or upgrade, see [Install Azure CLI 2.0](/cli/azure/install-azure-cli).</span></span>

## <a name="create-a-resource-group"></a><span data-ttu-id="d45b8-117">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="d45b8-117">Create a resource group</span></span>

<span data-ttu-id="d45b8-118">Az Event Grid-témakörök Azure-erőforrások, amelyeket egy Azure-erőforráscsoportba kell helyezni.</span><span class="sxs-lookup"><span data-stu-id="d45b8-118">Event Grid topics are Azure resources, and must be placed in an Azure resource group.</span></span> <span data-ttu-id="d45b8-119">Az erőforráscsoport egy olyan logikai gyűjtemény, amelyben a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="d45b8-119">The resource group is a logical collection into which Azure resources are deployed and managed.</span></span>

<span data-ttu-id="d45b8-120">Hozzon létre egy erőforráscsoportot az [az group create](/cli/azure/group#create) paranccsal.</span><span class="sxs-lookup"><span data-stu-id="d45b8-120">Create a resource group with the [az group create](/cli/azure/group#create) command.</span></span> 

<span data-ttu-id="d45b8-121">A következő példában létrehozunk egy *gridResourceGroup* nevű erőforráscsoportot a *westus2* helyen.</span><span class="sxs-lookup"><span data-stu-id="d45b8-121">The following example creates a resource group named *gridResourceGroup* in the *westus2* location.</span></span>

```azurecli-interactive
az group create --name gridResourceGroup --location westus2
```

## <a name="create-a-custom-topic"></a><span data-ttu-id="d45b8-122">Egyéni témakör létrehozása</span><span class="sxs-lookup"><span data-stu-id="d45b8-122">Create a custom topic</span></span>

<span data-ttu-id="d45b8-123">A témakör egy felhasználó által meghatározott végpontot biztosít, amelybe elküldheti az eseményeket.</span><span class="sxs-lookup"><span data-stu-id="d45b8-123">A topic provides a user-defined endpoint that you post your events to.</span></span> <span data-ttu-id="d45b8-124">Az alábbi példa az erőforráscsoportban hozza létre a témakört.</span><span class="sxs-lookup"><span data-stu-id="d45b8-124">The following example creates the topic in your resource group.</span></span> <span data-ttu-id="d45b8-125">A `<topic_name>` elemet a témakör egyedi nevére cserélje le.</span><span class="sxs-lookup"><span data-stu-id="d45b8-125">Replace `<topic_name>` with a unique name for your topic.</span></span> <span data-ttu-id="d45b8-126">A témakör nevének egyedinek kell lennie, mert a nevet egy DNS-bejegyzés képviseli.</span><span class="sxs-lookup"><span data-stu-id="d45b8-126">The topic name must be unique because it is represented by a DNS entry.</span></span> <span data-ttu-id="d45b8-127">Az előzetes verzió esetén az Event Grid a **westus2** és a **westcentralus** helyet támogatja.</span><span class="sxs-lookup"><span data-stu-id="d45b8-127">For the preview release, Event Grid supports **westus2** and **westcentralus** locations.</span></span>

```azurecli-interactive
az eventgrid topic create --name <topic_name> -l westus2 -g gridResourceGroup
```

## <a name="create-a-message-endpoint"></a><span data-ttu-id="d45b8-128">Üzenetvégpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="d45b8-128">Create a message endpoint</span></span>

<span data-ttu-id="d45b8-129">A témakörre való feliratkozás előtt hozzuk létre az eseményüzenet végpontját.</span><span class="sxs-lookup"><span data-stu-id="d45b8-129">Before subscribing to the topic, let's create the endpoint for the event message.</span></span> <span data-ttu-id="d45b8-130">Az eseményre reagáló kód írása helyett egy olyan végpontot hozzunk létre, amely gyűjti az üzeneteket, hogy meg tudja őket tekinteni.</span><span class="sxs-lookup"><span data-stu-id="d45b8-130">Rather than write code to respond to the event, let's create an endpoint that collects the messages so you can view them.</span></span> <span data-ttu-id="d45b8-131">A RequestBin egy nyílt forráskódú, külső gyártótól származó eszköz, amely lehetővé teszi egy végpont létrehozását és a neki küldött kérések megtekintését.</span><span class="sxs-lookup"><span data-stu-id="d45b8-131">RequestBin is an open source, third-party tool that enables you to create an endpoint, and view requests that are sent to it.</span></span> <span data-ttu-id="d45b8-132">Nyissa meg a [RequestBin](https://requestb.in/) eszközt, és kattintson a **Create a RequestBin** (Kéréstár létrehozása) elemre.</span><span class="sxs-lookup"><span data-stu-id="d45b8-132">Go to [RequestBin](https://requestb.in/), and click **Create a RequestBin**.</span></span>  <span data-ttu-id="d45b8-133">Másolja ki a tár URL-címét, mert szüksége lesz rá, amikor feliratkozik a témakörre.</span><span class="sxs-lookup"><span data-stu-id="d45b8-133">Copy the bin URL, because you need it when subscribing to the topic.</span></span>

## <a name="subscribe-to-a-topic"></a><span data-ttu-id="d45b8-134">Feliratkozás témakörre</span><span class="sxs-lookup"><span data-stu-id="d45b8-134">Subscribe to a topic</span></span>

<span data-ttu-id="d45b8-135">A témakörre való feliratkozással lehet tudatni az Event Griddel, hogy mely eseményeket kívánja nyomon követni. Az alábbi példa feliratkozik a létrehozott témakörre, és az eseményértesítés végpontjaként adja át a RequestBinből átemelt URL-címet.</span><span class="sxs-lookup"><span data-stu-id="d45b8-135">You subscribe to a topic to tell Event Grid which events you want to track. The following example subscribes to the topic you created, and passes the URL from RequestBin as the endpoint for event notification.</span></span> <span data-ttu-id="d45b8-136">Az `<event_subscription_name>` elemet a feliratkozás egyedi nevére cserélje le, az `<URL_from_RequestBin>` elemet pedig az előző szakaszból származó értékre.</span><span class="sxs-lookup"><span data-stu-id="d45b8-136">Replace `<event_subscription_name>` with a unique name for your subscription, and `<URL_from_RequestBin>` with the value from the preceding section.</span></span> <span data-ttu-id="d45b8-137">Ha megadja a végpontot a feliratkozáskor, az Event Grid az adott végpontra irányítja az eseményeket.</span><span class="sxs-lookup"><span data-stu-id="d45b8-137">By specifying an endpoint when subscribing, Event Grid handles the routing of events to that endpoint.</span></span> <span data-ttu-id="d45b8-138">A `<topic_name>` elemnél a korábban létrehozott értéket adja meg.</span><span class="sxs-lookup"><span data-stu-id="d45b8-138">For `<topic_name>`, use the value you created earlier.</span></span> 

```azurecli-interactive
az eventgrid topic event-subscription create --name <event_subscription_name> \
  --endpoint <URL_from_RequestBin> \
  -g gridResourceGroup \
  --topic-name <topic_name>
```

## <a name="send-an-event-to-your-topic"></a><span data-ttu-id="d45b8-139">Esemény elküldése a témakörbe</span><span class="sxs-lookup"><span data-stu-id="d45b8-139">Send an event to your topic</span></span>

<span data-ttu-id="d45b8-140">Most aktiváljunk egy eseményt, és lássuk, hogyan küldi el az üzenetet az Event Grid a végpontnak.</span><span class="sxs-lookup"><span data-stu-id="d45b8-140">Now, let's trigger an event to see how Event Grid distributes the message to your endpoint.</span></span> <span data-ttu-id="d45b8-141">Először szükségünk lesz a témakör URL-címére és kulcsára.</span><span class="sxs-lookup"><span data-stu-id="d45b8-141">First, let's get the URL and key for the topic.</span></span> <span data-ttu-id="d45b8-142">A `<topic_name>` helyett használja ismét a témakör nevét.</span><span class="sxs-lookup"><span data-stu-id="d45b8-142">Again, use your topic name for `<topic_name>`.</span></span>

```azurecli-interactive
endpoint=$(az eventgrid topic show --name <topic_name> -g gridResourceGroup --query "endpoint" --output tsv)
key=$(az eventgrid topic key list --name <topic_name> -g gridResourceGroup --query "key1" --output tsv)
```

<span data-ttu-id="d45b8-143">A folyamat leegyszerűsítése érdekében mintául szolgáló eseményadatokat hoztunk létre, amelyeket elküldhet a témakörbe.</span><span class="sxs-lookup"><span data-stu-id="d45b8-143">To simplify this article, we have set up sample event data to send to the topic.</span></span> <span data-ttu-id="d45b8-144">Egy alkalmazás vagy Azure-szolgáltatás általában eseményadatokat küld el.</span><span class="sxs-lookup"><span data-stu-id="d45b8-144">Typically, an application or Azure service would send the event data.</span></span> <span data-ttu-id="d45b8-145">Az alábbi példa lekéri az eseményadatokat:</span><span class="sxs-lookup"><span data-stu-id="d45b8-145">The following example gets the event data:</span></span>

```azurecli-interactive
body=$(eval echo "'$(curl https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/event-grid/customevent.json)'")
```

<span data-ttu-id="d45b8-146">Ha kiadja az `echo "$body"` parancsot, a teljes eseményt megtekintheti.</span><span class="sxs-lookup"><span data-stu-id="d45b8-146">If you `echo "$body"` you can see the full event.</span></span> <span data-ttu-id="d45b8-147">A JSON `data` eleme az esemény hasznos adata.</span><span class="sxs-lookup"><span data-stu-id="d45b8-147">The `data` element of the JSON is the payload of your event.</span></span> <span data-ttu-id="d45b8-148">Bármilyen, megfelelően formált JSON megadható ebben a mezőben.</span><span class="sxs-lookup"><span data-stu-id="d45b8-148">Any well-formed JSON can go in this field.</span></span> <span data-ttu-id="d45b8-149">A speciális útválasztáshoz és szűréshez használhatja a tárgy mezőt is.</span><span class="sxs-lookup"><span data-stu-id="d45b8-149">You can also use the subject field for advanced routing and filtering.</span></span>

<span data-ttu-id="d45b8-150">A CURL egy olyan segédprogram, amely HTTP-kéréseket hajt végre.</span><span class="sxs-lookup"><span data-stu-id="d45b8-150">CURL is a utility that performs HTTP requests.</span></span> <span data-ttu-id="d45b8-151">Ebben a cikkben a CURL használatával küldjük el az eseményt a témakörbe.</span><span class="sxs-lookup"><span data-stu-id="d45b8-151">In this article, we use CURL to send the event to our topic.</span></span> 

```azurecli-interactive
curl -X POST -H "aeg-sas-key: $key" -d "$body" $endpoint
```

<span data-ttu-id="d45b8-152">Ön kiváltotta az eseményt, az Event Grid pedig elküldte az üzenetet a feliratkozáskor konfigurált végpontnak.</span><span class="sxs-lookup"><span data-stu-id="d45b8-152">You have triggered the event, and Event Grid sent the message to the endpoint you configured when subscribing.</span></span> <span data-ttu-id="d45b8-153">Lépjen a RequestBin eszközben korábban létrehozott URL-címre,</span><span class="sxs-lookup"><span data-stu-id="d45b8-153">Browse to the RequestBin URL that you created earlier.</span></span> <span data-ttu-id="d45b8-154">vagy kattintson a megnyitott RequestBin-böngésző frissítés elemére.</span><span class="sxs-lookup"><span data-stu-id="d45b8-154">Or, click refresh in your open RequestBin browser.</span></span> <span data-ttu-id="d45b8-155">Megjelenik az imént elküldött esemény.</span><span class="sxs-lookup"><span data-stu-id="d45b8-155">You see the event you just sent.</span></span> 

```json
[{
  "id": "1807",
  "eventType": "recordInserted",
  "subject": "myapp/vehicles/motorcycles",
  "eventTime": "2017-08-10T21:03:07+00:00",
  "data": {
    "make": "Ducati",
    "model": "Monster"
  },
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventGrid/topics/{topic}"
}]
```

## <a name="clean-up-resources"></a><span data-ttu-id="d45b8-156">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="d45b8-156">Clean up resources</span></span>
<span data-ttu-id="d45b8-157">Ha azt tervezi, hogy folytatja az esemény használatát, akkor ne törölje a cikkben létrehozott erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="d45b8-157">If you plan to continue working with this event, do not clean up the resources created in this article.</span></span> <span data-ttu-id="d45b8-158">Ha nem folytatja a munkát, akkor a következő paranccsal törölheti a cikkben létrehozott erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="d45b8-158">If you do not plan to continue, use the following command to delete the resources you created in this article.</span></span>

```azurecli-interactive
az group delete --name gridResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="d45b8-159">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d45b8-159">Next steps</span></span>

<span data-ttu-id="d45b8-160">Most, hogy megismerkedett vele, hogyan hozhat létre témaköröket és eseményfeliratkozásokat, bővebben is tájékozódhat arról, hogy miben nyújthat segítséget az Event Grid:</span><span class="sxs-lookup"><span data-stu-id="d45b8-160">Now that you know how to create topics and event subscriptions, learn more about what Event Grid can help you do:</span></span>

- [<span data-ttu-id="d45b8-161">Bevezetés az Event Grid használatába</span><span class="sxs-lookup"><span data-stu-id="d45b8-161">About Event Grid</span></span>](overview.md)
- [<span data-ttu-id="d45b8-162">Virtuális gépek módosításainak monitorozása az Azure Event Grid és a Logic Apps segítségével</span><span class="sxs-lookup"><span data-stu-id="d45b8-162">Monitor virtual machine changes with Azure Event Grid and Logic Apps</span></span>](monitor-virtual-machine-changes-event-grid-logic-app.md)
