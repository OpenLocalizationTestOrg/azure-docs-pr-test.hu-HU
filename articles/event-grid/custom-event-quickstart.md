---
title: "az Azure Event rács aaaCustom események |} Microsoft Docs"
description: "Azure Event rács toopublish témakör használja, és feliratkozhat toothat esemény."
services: event-grid
keywords: 
author: djrosanova
ms.author: darosa
ms.date: 08/15/2017
ms.topic: hero-article
ms.service: event-grid
ms.openlocfilehash: 5055df1c970b043cadf06978a076f7f5c83501cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-route-custom-events-with-azure-event-grid"></a><span data-ttu-id="d9b9a-103">Egyéni események létrehozása és átirányítása az Azure Event Griddel</span><span class="sxs-lookup"><span data-stu-id="d9b9a-103">Create and route custom events with Azure Event Grid</span></span>

<span data-ttu-id="d9b9a-104">Az Azure Event rács egy hello felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="d9b9a-104">Azure Event Grid is an eventing service for hello cloud.</span></span> <span data-ttu-id="d9b9a-105">Ebben a cikkben használjon hello Azure CLI toocreate egyéni témakör, előfizetés toohello témakör, és indítás hello esemény tooview hello eredménye.</span><span class="sxs-lookup"><span data-stu-id="d9b9a-105">In this article, you use hello Azure CLI toocreate a custom topic, subscribe toohello topic, and trigger hello event tooview hello result.</span></span> <span data-ttu-id="d9b9a-106">Általában küldött események tooan végpontot, amely válaszol a toohello esemény, például egy webhook vagy az Azure-függvény.</span><span class="sxs-lookup"><span data-stu-id="d9b9a-106">Typically, you send events tooan endpoint that responds toohello event, such as, a webhook or Azure Function.</span></span> <span data-ttu-id="d9b9a-107">Azonban toosimplify Ez cikk, hello események tooa URL-cím köszönőüzenetei csupán összegyűjtő küld.</span><span class="sxs-lookup"><span data-stu-id="d9b9a-107">However, toosimplify this article, you send hello events tooa URL that merely collects hello messages.</span></span> <span data-ttu-id="d9b9a-108">Az URL-címet a [RequestBin](https://requestb.in/) nevű nyílt forráskódú, külső fél által biztosított eszközzel fogjuk létrehozni.</span><span class="sxs-lookup"><span data-stu-id="d9b9a-108">You create this URL by using an open source, third-party tool called [RequestBin](https://requestb.in/).</span></span>

>[!NOTE]
><span data-ttu-id="d9b9a-109">A **RequestBin** egy nyílt forráskódú eszköz, amely nagy teljesítményt megkövetelő használati területekhez nem alkalmas.</span><span class="sxs-lookup"><span data-stu-id="d9b9a-109">**RequestBin** is an open source tool that is not intended for high throughput usage.</span></span> <span data-ttu-id="d9b9a-110">Itt hello eszköz hello használata tisztán demonstrative.</span><span class="sxs-lookup"><span data-stu-id="d9b9a-110">hello use of hello tool here is purely demonstrative.</span></span> <span data-ttu-id="d9b9a-111">Ha egyszerre több esemény leküldéses, nem láthatja az összes hello eszközben az események.</span><span class="sxs-lookup"><span data-stu-id="d9b9a-111">If you push more than one event at a time, you might not see all of your events in hello tool.</span></span>

<span data-ttu-id="d9b9a-112">Ha elkészült, láthatja, hogy hello eseményadatok küldte tooan végpont.</span><span class="sxs-lookup"><span data-stu-id="d9b9a-112">When you are finished, you see that hello event data has been sent tooan endpoint.</span></span>

![Eseményadatok](./media/custom-event-quickstart/request-result.png)

[!INCLUDE [quickstarts-free-trial-note.md](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="d9b9a-114">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ez a cikk van szükség, hogy futnak-e az Azure parancssori felület legújabb verziójának hello (2.0.14 vagy újabb).</span><span class="sxs-lookup"><span data-stu-id="d9b9a-114">If you choose tooinstall and use hello CLI locally, this article requires that you are running hello latest version of Azure CLI (2.0.14 or later).</span></span> <span data-ttu-id="d9b9a-115">toofind hello verzió, futtassa `az --version`.</span><span class="sxs-lookup"><span data-stu-id="d9b9a-115">toofind hello version, run `az --version`.</span></span> <span data-ttu-id="d9b9a-116">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="d9b9a-116">If you need tooinstall or upgrade, see [Install Azure CLI 2.0](/cli/azure/install-azure-cli).</span></span>

## <a name="create-a-resource-group"></a><span data-ttu-id="d9b9a-117">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="d9b9a-117">Create a resource group</span></span>

<span data-ttu-id="d9b9a-118">Az Event Grid-témakörök Azure-erőforrások, amelyeket egy Azure-erőforráscsoportba kell helyezni.</span><span class="sxs-lookup"><span data-stu-id="d9b9a-118">Event Grid topics are Azure resources, and must be placed in an Azure resource group.</span></span> <span data-ttu-id="d9b9a-119">hello erőforráscsoport gyűjteményei logikai mely Azure az erőforrások telepítése és kezelése.</span><span class="sxs-lookup"><span data-stu-id="d9b9a-119">hello resource group is a logical collection into which Azure resources are deployed and managed.</span></span>

<span data-ttu-id="d9b9a-120">Hozzon létre egy erőforráscsoportot hello [az csoport létrehozása](/cli/azure/group#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="d9b9a-120">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span> 

<span data-ttu-id="d9b9a-121">hello alábbi példa létrehoz egy erőforráscsoportot *gridResourceGroup* a hello *westus2* helyét.</span><span class="sxs-lookup"><span data-stu-id="d9b9a-121">hello following example creates a resource group named *gridResourceGroup* in hello *westus2* location.</span></span>

```azurecli-interactive
az group create --name gridResourceGroup --location westus2
```

## <a name="create-a-custom-topic"></a><span data-ttu-id="d9b9a-122">Egyéni témakör létrehozása</span><span class="sxs-lookup"><span data-stu-id="d9b9a-122">Create a custom topic</span></span>

<span data-ttu-id="d9b9a-123">A témakör egy felhasználó által meghatározott végpontot biztosít, amelybe elküldheti az eseményeket.</span><span class="sxs-lookup"><span data-stu-id="d9b9a-123">A topic provides a user-defined endpoint that you post your events to.</span></span> <span data-ttu-id="d9b9a-124">hello alábbi példakód létrehozza hello a témakör az erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="d9b9a-124">hello following example creates hello topic in your resource group.</span></span> <span data-ttu-id="d9b9a-125">A `<topic_name>` elemet a témakör egyedi nevére cserélje le.</span><span class="sxs-lookup"><span data-stu-id="d9b9a-125">Replace `<topic_name>` with a unique name for your topic.</span></span> <span data-ttu-id="d9b9a-126">hello témakör nevének egyedinek kell lennie, mert a DNS-bejegyzés jelképezi.</span><span class="sxs-lookup"><span data-stu-id="d9b9a-126">hello topic name must be unique because it is represented by a DNS entry.</span></span> <span data-ttu-id="d9b9a-127">A hello előzetes kiadását, esemény rács támogatja **westus2** és **westcentralus** helyét.</span><span class="sxs-lookup"><span data-stu-id="d9b9a-127">For hello preview release, Event Grid supports **westus2** and **westcentralus** locations.</span></span>

```azurecli-interactive
az eventgrid topic create --name <topic_name> -l westus2 -g gridResourceGroup
```

## <a name="create-a-message-endpoint"></a><span data-ttu-id="d9b9a-128">Üzenetvégpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="d9b9a-128">Create a message endpoint</span></span>

<span data-ttu-id="d9b9a-129">Mielőtt toohello témakör előfizetés, hozzuk létre hello eseményüzenet hello végpontja.</span><span class="sxs-lookup"><span data-stu-id="d9b9a-129">Before subscribing toohello topic, let's create hello endpoint for hello event message.</span></span> <span data-ttu-id="d9b9a-130">Helyett a kód toorespond toohello esemény írása, hozzon létre egy végpontot, amely köszönőüzenetei gyűjti, így meg lehet tekinteni őket.</span><span class="sxs-lookup"><span data-stu-id="d9b9a-130">Rather than write code toorespond toohello event, let's create an endpoint that collects hello messages so you can view them.</span></span> <span data-ttu-id="d9b9a-131">RequestBin egy nyílt forráskódú, külső eszköz, amely lehetővé teszi a végpont toocreate és tooit küldött kérések megtekintése.</span><span class="sxs-lookup"><span data-stu-id="d9b9a-131">RequestBin is an open source, third-party tool that enables you toocreate an endpoint, and view requests that are sent tooit.</span></span> <span data-ttu-id="d9b9a-132">Nyissa meg túl[RequestBin](https://requestb.in/), és kattintson a **hozzon létre egy RequestBin**.</span><span class="sxs-lookup"><span data-stu-id="d9b9a-132">Go too[RequestBin](https://requestb.in/), and click **Create a RequestBin**.</span></span>  <span data-ttu-id="d9b9a-133">Másolja a hello bin URL-címet, mert amikor toohello témakör előfizetés.</span><span class="sxs-lookup"><span data-stu-id="d9b9a-133">Copy hello bin URL, because you need it when subscribing toohello topic.</span></span>

## <a name="subscribe-tooa-topic"></a><span data-ttu-id="d9b9a-134">Előfizetés tooa témakör</span><span class="sxs-lookup"><span data-stu-id="d9b9a-134">Subscribe tooa topic</span></span>

<span data-ttu-id="d9b9a-135">Előfizetés tooa témakör tootell esemény rács eseményeket szeretné tootrack.</span><span class="sxs-lookup"><span data-stu-id="d9b9a-135">You subscribe tooa topic tootell Event Grid which events you want tootrack.</span></span> <span data-ttu-id="d9b9a-136">hello alábbi példa előfizet hozott létre, és továbbítja a hello URL-címet a RequestBin hello végpontként az eseményértesítés toohello témakör.</span><span class="sxs-lookup"><span data-stu-id="d9b9a-136">hello following example subscribes toohello topic you created, and passes hello URL from RequestBin as hello endpoint for event notification.</span></span> <span data-ttu-id="d9b9a-137">Cserélje le `<event_subscription_name>` az előfizetéshez tartozó egyedi névvel ellátott és `<URL_from_RequestBin>` az előző szakaszban hello hello értékkel.</span><span class="sxs-lookup"><span data-stu-id="d9b9a-137">Replace `<event_subscription_name>` with a unique name for your subscription, and `<URL_from_RequestBin>` with hello value from hello preceding section.</span></span> <span data-ttu-id="d9b9a-138">Szerint adja meg a végpont, ha feliratkozik, esemény rács kezeli a hello útválasztási események toothat végpont.</span><span class="sxs-lookup"><span data-stu-id="d9b9a-138">By specifying an endpoint when subscribing, Event Grid handles hello routing of events toothat endpoint.</span></span> <span data-ttu-id="d9b9a-139">A `<topic_name>`, korábban létrehozott hello értéket használja.</span><span class="sxs-lookup"><span data-stu-id="d9b9a-139">For `<topic_name>`, use hello value you created earlier.</span></span> 

```azurecli-interactive
az eventgrid topic event-subscription create --name <event_subscription_name> \
  --endpoint <URL_from_RequestBin> \
  -g gridResourceGroup \
  --topic-name <topic_name>
```

## <a name="send-an-event-tooyour-topic"></a><span data-ttu-id="d9b9a-140">Egy esemény tooyour témakör küldése</span><span class="sxs-lookup"><span data-stu-id="d9b9a-140">Send an event tooyour topic</span></span>

<span data-ttu-id="d9b9a-141">Most tegyük indul el, az esemény toosee hogyan osztja el a esemény rács hello üzenet tooyour végpont.</span><span class="sxs-lookup"><span data-stu-id="d9b9a-141">Now, let's trigger an event toosee how Event Grid distributes hello message tooyour endpoint.</span></span> <span data-ttu-id="d9b9a-142">Először hozzuk hello URL-cím beszerzése és hello témakör kulcsát.</span><span class="sxs-lookup"><span data-stu-id="d9b9a-142">First, let's get hello URL and key for hello topic.</span></span> <span data-ttu-id="d9b9a-143">A `<topic_name>` helyett használja ismét a témakör nevét.</span><span class="sxs-lookup"><span data-stu-id="d9b9a-143">Again, use your topic name for `<topic_name>`.</span></span>

```azurecli-interactive
endpoint=$(az eventgrid topic show --name <topic_name> -g gridResourceGroup --query "endpoint" --output tsv)
key=$(az eventgrid topic key list --name <topic_name> -g gridResourceGroup --query "key1" --output tsv)
```

<span data-ttu-id="d9b9a-144">toosimplify ebben a cikkben azt állította be a minta esemény adatok toosend toohello témakör.</span><span class="sxs-lookup"><span data-stu-id="d9b9a-144">toosimplify this article, we have set up sample event data toosend toohello topic.</span></span> <span data-ttu-id="d9b9a-145">Általában az alkalmazás vagy az Azure szolgáltatás elküldése hello eseményadatok.</span><span class="sxs-lookup"><span data-stu-id="d9b9a-145">Typically, an application or Azure service would send hello event data.</span></span> <span data-ttu-id="d9b9a-146">a következő példa hello hello események adatainak beolvasása:</span><span class="sxs-lookup"><span data-stu-id="d9b9a-146">hello following example gets hello event data:</span></span>

```azurecli-interactive
body=$(eval echo "'$(curl https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/event-grid/customevent.json)'")
```

<span data-ttu-id="d9b9a-147">Ha Ön `echo "$body"` hello az esemény teljes látható.</span><span class="sxs-lookup"><span data-stu-id="d9b9a-147">If you `echo "$body"` you can see hello full event.</span></span> <span data-ttu-id="d9b9a-148">Hello `data` hello JSON eleme az esemény hello hasznos.</span><span class="sxs-lookup"><span data-stu-id="d9b9a-148">hello `data` element of hello JSON is hello payload of your event.</span></span> <span data-ttu-id="d9b9a-149">Bármilyen, megfelelően formált JSON megadható ebben a mezőben.</span><span class="sxs-lookup"><span data-stu-id="d9b9a-149">Any well-formed JSON can go in this field.</span></span> <span data-ttu-id="d9b9a-150">Hello tulajdonos mezőt az speciális útválasztáshoz és a szűrés is használhatja.</span><span class="sxs-lookup"><span data-stu-id="d9b9a-150">You can also use hello subject field for advanced routing and filtering.</span></span>

<span data-ttu-id="d9b9a-151">A CURL egy olyan segédprogram, amely HTTP-kéréseket hajt végre.</span><span class="sxs-lookup"><span data-stu-id="d9b9a-151">CURL is a utility that performs HTTP requests.</span></span> <span data-ttu-id="d9b9a-152">Ez a cikk a CURL toosend hello esemény tooour témakör használjuk.</span><span class="sxs-lookup"><span data-stu-id="d9b9a-152">In this article, we use CURL toosend hello event tooour topic.</span></span> 

```azurecli-interactive
curl -X POST -H "aeg-sas-key: $key" -d "$body" $endpoint
```

<span data-ttu-id="d9b9a-153">Hello esemény léptetnie, ezért esemény rács küldi hello üzenet toohello végpont előfizetés konfigurált.</span><span class="sxs-lookup"><span data-stu-id="d9b9a-153">You have triggered hello event, and Event Grid sent hello message toohello endpoint you configured when subscribing.</span></span> <span data-ttu-id="d9b9a-154">Keresse meg a korábban létrehozott RequestBin URL toohello.</span><span class="sxs-lookup"><span data-stu-id="d9b9a-154">Browse toohello RequestBin URL that you created earlier.</span></span> <span data-ttu-id="d9b9a-155">vagy kattintson a megnyitott RequestBin-böngésző frissítés elemére.</span><span class="sxs-lookup"><span data-stu-id="d9b9a-155">Or, click refresh in your open RequestBin browser.</span></span> <span data-ttu-id="d9b9a-156">Megjelenik az imént a telefonjára küldött hello esemény.</span><span class="sxs-lookup"><span data-stu-id="d9b9a-156">You see hello event you just sent.</span></span> 

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

## <a name="clean-up-resources"></a><span data-ttu-id="d9b9a-157">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="d9b9a-157">Clean up resources</span></span>
<span data-ttu-id="d9b9a-158">Ha azt tervezi, hogy ez az esemény használata toocontinue, tegye a cikkben létrehozott hello erőforrások nem törlése.</span><span class="sxs-lookup"><span data-stu-id="d9b9a-158">If you plan toocontinue working with this event, do not clean up hello resources created in this article.</span></span> <span data-ttu-id="d9b9a-159">Ha nem tervezi toocontinue, használja a következő parancs toodelete hello erőforrások ebben a cikkben létrehozott hello.</span><span class="sxs-lookup"><span data-stu-id="d9b9a-159">If you do not plan toocontinue, use hello following command toodelete hello resources you created in this article.</span></span>

```azurecli-interactive
az group delete --name gridResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="d9b9a-160">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d9b9a-160">Next steps</span></span>

<span data-ttu-id="d9b9a-161">Most, hogy hogyan toocreate üzenettémák és előfizetések esemény, tudhat meg többet milyen esemény rács is segítséget nyújthat:</span><span class="sxs-lookup"><span data-stu-id="d9b9a-161">Now that you know how toocreate topics and event subscriptions, learn more about what Event Grid can help you do:</span></span>

- [<span data-ttu-id="d9b9a-162">Bevezetés az Event Grid használatába</span><span class="sxs-lookup"><span data-stu-id="d9b9a-162">About Event Grid</span></span>](overview.md)
- [<span data-ttu-id="d9b9a-163">Virtuális gépek módosításainak monitorozása az Azure Event Grid és a Logic Apps segítségével</span><span class="sxs-lookup"><span data-stu-id="d9b9a-163">Monitor virtual machine changes with Azure Event Grid and Logic Apps</span></span>](monitor-virtual-machine-changes-event-grid-logic-app.md)
