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
# <a name="create-and-route-custom-events-with-azure-event-grid"></a>Egyéni események létrehozása és átirányítása az Azure Event Griddel

Az Azure Event rács egy hello felhőalapú szolgáltatás. Ebben a cikkben használjon hello Azure CLI toocreate egyéni témakör, előfizetés toohello témakör, és indítás hello esemény tooview hello eredménye. Általában küldött események tooan végpontot, amely válaszol a toohello esemény, például egy webhook vagy az Azure-függvény. Azonban toosimplify Ez cikk, hello események tooa URL-cím köszönőüzenetei csupán összegyűjtő küld. Az URL-címet a [RequestBin](https://requestb.in/) nevű nyílt forráskódú, külső fél által biztosított eszközzel fogjuk létrehozni.

>[!NOTE]
>A **RequestBin** egy nyílt forráskódú eszköz, amely nagy teljesítményt megkövetelő használati területekhez nem alkalmas. Itt hello eszköz hello használata tisztán demonstrative. Ha egyszerre több esemény leküldéses, nem láthatja az összes hello eszközben az események.

Ha elkészült, láthatja, hogy hello eseményadatok küldte tooan végpont.

![Eseményadatok](./media/custom-event-quickstart/request-result.png)

[!INCLUDE [quickstarts-free-trial-note.md](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Ha Ön tooinstall kiválasztása és hello CLI helyileg, ez a cikk van szükség, hogy futnak-e az Azure parancssori felület legújabb verziójának hello (2.0.14 vagy újabb). toofind hello verzió, futtassa `az --version`. Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése](/cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Hozzon létre egy erőforráscsoportot

Az Event Grid-témakörök Azure-erőforrások, amelyeket egy Azure-erőforráscsoportba kell helyezni. hello erőforráscsoport gyűjteményei logikai mely Azure az erőforrások telepítése és kezelése.

Hozzon létre egy erőforráscsoportot hello [az csoport létrehozása](/cli/azure/group#create) parancsot. 

hello alábbi példa létrehoz egy erőforráscsoportot *gridResourceGroup* a hello *westus2* helyét.

```azurecli-interactive
az group create --name gridResourceGroup --location westus2
```

## <a name="create-a-custom-topic"></a>Egyéni témakör létrehozása

A témakör egy felhasználó által meghatározott végpontot biztosít, amelybe elküldheti az eseményeket. hello alábbi példakód létrehozza hello a témakör az erőforráscsoportban. A `<topic_name>` elemet a témakör egyedi nevére cserélje le. hello témakör nevének egyedinek kell lennie, mert a DNS-bejegyzés jelképezi. A hello előzetes kiadását, esemény rács támogatja **westus2** és **westcentralus** helyét.

```azurecli-interactive
az eventgrid topic create --name <topic_name> -l westus2 -g gridResourceGroup
```

## <a name="create-a-message-endpoint"></a>Üzenetvégpont létrehozása

Mielőtt toohello témakör előfizetés, hozzuk létre hello eseményüzenet hello végpontja. Helyett a kód toorespond toohello esemény írása, hozzon létre egy végpontot, amely köszönőüzenetei gyűjti, így meg lehet tekinteni őket. RequestBin egy nyílt forráskódú, külső eszköz, amely lehetővé teszi a végpont toocreate és tooit küldött kérések megtekintése. Nyissa meg túl[RequestBin](https://requestb.in/), és kattintson a **hozzon létre egy RequestBin**.  Másolja a hello bin URL-címet, mert amikor toohello témakör előfizetés.

## <a name="subscribe-tooa-topic"></a>Előfizetés tooa témakör

Előfizetés tooa témakör tootell esemény rács eseményeket szeretné tootrack. hello alábbi példa előfizet hozott létre, és továbbítja a hello URL-címet a RequestBin hello végpontként az eseményértesítés toohello témakör. Cserélje le `<event_subscription_name>` az előfizetéshez tartozó egyedi névvel ellátott és `<URL_from_RequestBin>` az előző szakaszban hello hello értékkel. Szerint adja meg a végpont, ha feliratkozik, esemény rács kezeli a hello útválasztási események toothat végpont. A `<topic_name>`, korábban létrehozott hello értéket használja. 

```azurecli-interactive
az eventgrid topic event-subscription create --name <event_subscription_name> \
  --endpoint <URL_from_RequestBin> \
  -g gridResourceGroup \
  --topic-name <topic_name>
```

## <a name="send-an-event-tooyour-topic"></a>Egy esemény tooyour témakör küldése

Most tegyük indul el, az esemény toosee hogyan osztja el a esemény rács hello üzenet tooyour végpont. Először hozzuk hello URL-cím beszerzése és hello témakör kulcsát. A `<topic_name>` helyett használja ismét a témakör nevét.

```azurecli-interactive
endpoint=$(az eventgrid topic show --name <topic_name> -g gridResourceGroup --query "endpoint" --output tsv)
key=$(az eventgrid topic key list --name <topic_name> -g gridResourceGroup --query "key1" --output tsv)
```

toosimplify ebben a cikkben azt állította be a minta esemény adatok toosend toohello témakör. Általában az alkalmazás vagy az Azure szolgáltatás elküldése hello eseményadatok. a következő példa hello hello események adatainak beolvasása:

```azurecli-interactive
body=$(eval echo "'$(curl https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/event-grid/customevent.json)'")
```

Ha Ön `echo "$body"` hello az esemény teljes látható. Hello `data` hello JSON eleme az esemény hello hasznos. Bármilyen, megfelelően formált JSON megadható ebben a mezőben. Hello tulajdonos mezőt az speciális útválasztáshoz és a szűrés is használhatja.

A CURL egy olyan segédprogram, amely HTTP-kéréseket hajt végre. Ez a cikk a CURL toosend hello esemény tooour témakör használjuk. 

```azurecli-interactive
curl -X POST -H "aeg-sas-key: $key" -d "$body" $endpoint
```

Hello esemény léptetnie, ezért esemény rács küldi hello üzenet toohello végpont előfizetés konfigurált. Keresse meg a korábban létrehozott RequestBin URL toohello. vagy kattintson a megnyitott RequestBin-böngésző frissítés elemére. Megjelenik az imént a telefonjára küldött hello esemény. 

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

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása
Ha azt tervezi, hogy ez az esemény használata toocontinue, tegye a cikkben létrehozott hello erőforrások nem törlése. Ha nem tervezi toocontinue, használja a következő parancs toodelete hello erőforrások ebben a cikkben létrehozott hello.

```azurecli-interactive
az group delete --name gridResourceGroup
```

## <a name="next-steps"></a>Következő lépések

Most, hogy hogyan toocreate üzenettémák és előfizetések esemény, tudhat meg többet milyen esemény rács is segítséget nyújthat:

- [Bevezetés az Event Grid használatába](overview.md)
- [Virtuális gépek módosításainak monitorozása az Azure Event Grid és a Logic Apps segítségével](monitor-virtual-machine-changes-event-grid-logic-app.md)
