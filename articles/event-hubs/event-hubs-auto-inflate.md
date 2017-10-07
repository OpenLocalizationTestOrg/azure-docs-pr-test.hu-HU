---
title: "Azure Event Hubs átviteli egységek aaaAutomatically felskálázott |} Microsoft Docs"
description: "Átviteli egységek névtér tooautomatically felskálázott automatikus megnöveli engedélyezése"
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/12/2017
ms.author: shvija;sethm
ms.openlocfilehash: 0f5216bcd619ccddc1fd4063a2f0131bfa36c7d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="automatically-scale-up-azure-event-hubs-throughput-units"></a><span data-ttu-id="7e6f6-103">Azure Event Hubs átviteli egységek automatikus méretezése</span><span class="sxs-lookup"><span data-stu-id="7e6f6-103">Automatically scale up Azure Event Hubs throughput units</span></span>

## <a name="overview"></a><span data-ttu-id="7e6f6-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="7e6f6-104">Overview</span></span>

<span data-ttu-id="7e6f6-105">Az Azure Event Hubs egy kiválóan méretezhető adatfolyam platform.</span><span class="sxs-lookup"><span data-stu-id="7e6f6-105">Azure Event Hubs is a highly scalable data streaming platform.</span></span> <span data-ttu-id="7e6f6-106">Az Event Hubs az ügyfelek gyakran a használat növelése után bevezetési toohello szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="7e6f6-106">As such, Event Hubs customers often increase their usage after onboarding toohello service.</span></span> <span data-ttu-id="7e6f6-107">Ilyen növekszik növekvő előre meghatározott hello átviteli egységek (TUs) tooscale Event Hubs igényelnek, és nagyobb átviteli sebesség kezelésére.</span><span class="sxs-lookup"><span data-stu-id="7e6f6-107">Such increases require increasing hello predetermined throughput units (TUs) tooscale Event Hubs and handle larger transfer rates.</span></span> <span data-ttu-id="7e6f6-108">Hello *automatikus megnöveli* az Event hubs szolgáltatás automatikusan rendkívül TUs toomeet igényei hello száma.</span><span class="sxs-lookup"><span data-stu-id="7e6f6-108">hello *Auto-inflate* feature of Event Hubs automatically scales up hello number of TUs toomeet usage needs.</span></span> <span data-ttu-id="7e6f6-109">TUs növelése megakadályozza, hogy a sávszélesség-szabályozás forgatókönyvek, amelyben:</span><span class="sxs-lookup"><span data-stu-id="7e6f6-109">Increasing TUs prevents throttling scenarios, in which:</span></span>

* <span data-ttu-id="7e6f6-110">Adatok érkező haladhatja meg a díjszabás TUs beállítása.</span><span class="sxs-lookup"><span data-stu-id="7e6f6-110">Data ingress rates exceed set TUs.</span></span>
* <span data-ttu-id="7e6f6-111">Kimenő kérelem haladhatja meg a díjszabás TUs beállítása.</span><span class="sxs-lookup"><span data-stu-id="7e6f6-111">Data egress request rates exceed set TUs.</span></span>

## <a name="how-auto-inflate-works"></a><span data-ttu-id="7e6f6-112">Automatikus megnöveli működése</span><span class="sxs-lookup"><span data-stu-id="7e6f6-112">How Auto-inflate works</span></span>

<span data-ttu-id="7e6f6-113">Event Hubs forgalom átviteli egységek vezérli.</span><span class="sxs-lookup"><span data-stu-id="7e6f6-113">Event Hubs traffic is controlled by throughput units.</span></span> <span data-ttu-id="7e6f6-114">Egyetlen SO lehetővé teszi, hogy 1 MB másodpercenkénti bemenő és kimenő forgalom kétszer adott mennyisége.</span><span class="sxs-lookup"><span data-stu-id="7e6f6-114">A single TU allows 1 MB per second of ingress and twice that amount of egress.</span></span> <span data-ttu-id="7e6f6-115">Standard Event Hubs 1 – 20 átviteli egység konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="7e6f6-115">Standard Event Hubs can be configured with 1-20 throughput units.</span></span> <span data-ttu-id="7e6f6-116">Automatikus megnöveli lehetővé teszi toostart kis hello minimálisan szükséges átviteli egységek használatával.</span><span class="sxs-lookup"><span data-stu-id="7e6f6-116">Auto-inflate enables you toostart small with hello minimum required throughput units.</span></span> <span data-ttu-id="7e6f6-117">hello szolgáltatás automatikusan toohello maximális átviteli egységek van szükség, attól függően, hogy a forgalom hello növekedése majd arányosan.</span><span class="sxs-lookup"><span data-stu-id="7e6f6-117">hello feature then scales automatically toohello maximum limit of throughput units you need, depending on hello increase in your traffic.</span></span> <span data-ttu-id="7e6f6-118">Automatikus megnöveli hello a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="7e6f6-118">Auto-inflate provides hello following benefits:</span></span>

- <span data-ttu-id="7e6f6-119">Egy hatékony méretezési mechanizmus toostart kis és, ha Ön felskálázott nő.</span><span class="sxs-lookup"><span data-stu-id="7e6f6-119">An efficient scaling mechanism toostart small and scale up as you grow.</span></span>
- <span data-ttu-id="7e6f6-120">Automatikus méretezése toohello megadott felső korlát nélkül szabályozási problémákat.</span><span class="sxs-lookup"><span data-stu-id="7e6f6-120">Automatically scale toohello specified upper limit without throttling issues.</span></span>
- <span data-ttu-id="7e6f6-121">Több vezérlése skálázás, meghatározhatja, mikor és hogyan sok tooscale.</span><span class="sxs-lookup"><span data-stu-id="7e6f6-121">More control over scaling, as you control when and how much tooscale.</span></span>

## <a name="enable-auto-inflate-on-a-namespace"></a><span data-ttu-id="7e6f6-122">Engedélyezze az automatikus megnöveli a névtér</span><span class="sxs-lookup"><span data-stu-id="7e6f6-122">Enable Auto-inflate on a namespace</span></span>

<span data-ttu-id="7e6f6-123">Engedélyezi, vagy tiltsa le az automatikus megnöveli a névtér hello a következő módszerek valamelyikével:</span><span class="sxs-lookup"><span data-stu-id="7e6f6-123">You can enable or disable Auto-inflate on a namespace using either of hello following methods:</span></span>

1. <span data-ttu-id="7e6f6-124">Hello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="7e6f6-124">hello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="7e6f6-125">Az Azure Resource Manager-sablon.</span><span class="sxs-lookup"><span data-stu-id="7e6f6-125">An Azure Resource Manager template.</span></span>

### <a name="enable-auto-inflate-through-hello-portal"></a><span data-ttu-id="7e6f6-126">Engedélyezze az automatikus megnöveli hello portálon keresztül</span><span class="sxs-lookup"><span data-stu-id="7e6f6-126">Enable Auto-inflate through hello portal</span></span>

<span data-ttu-id="7e6f6-127">Engedélyezheti a hello automatikus megnöveli a szolgáltatást egy névtér, ha az Event Hubs névtér létrehozása:</span><span class="sxs-lookup"><span data-stu-id="7e6f6-127">You can enable hello Auto-inflate feature on a namespace when creating an Event Hubs namespace:</span></span>
 
![](./media/event-hubs-auto-inflate/event-hubs-auto-inflate1.png)

<span data-ttu-id="7e6f6-128">A funkció engedélyezését kezdje kis lépésekkel az átviteli egységek, és a használati mutatói igények növekedése méret.</span><span class="sxs-lookup"><span data-stu-id="7e6f6-128">With this option enabled, you can start small on your throughput units and scale up as your usage needs increase.</span></span> <span data-ttu-id="7e6f6-129">hello infláció felső határa nem érinti a díjszabást hello száma óránként használt TUs függ.</span><span class="sxs-lookup"><span data-stu-id="7e6f6-129">hello upper limit for inflation does not affect pricing, which depends on hello number of TUs used per hour.</span></span>

<span data-ttu-id="7e6f6-130">Továbbá engedélyezheti az automatikus megnöveli hello segítségével **méretezési** lehetőséget a hello portálon hello-beállítások panelen:</span><span class="sxs-lookup"><span data-stu-id="7e6f6-130">You can also enable Auto-inflate using hello **Scale** option on hello settings blade in hello portal:</span></span>
 
![](./media/event-hubs-auto-inflate/event-hubs-auto-inflate2.png)

### <a name="enable-auto-inflate-using-an-azure-resource-manager-template"></a><span data-ttu-id="7e6f6-131">Engedélyezze az automatikus-megnöveli Azure Resource Manager-sablonnal</span><span class="sxs-lookup"><span data-stu-id="7e6f6-131">Enable Auto-Inflate using an Azure Resource Manager template</span></span>

<span data-ttu-id="7e6f6-132">Automatikus megnöveli az Azure Resource Manager sablon telepítése során engedélyezheti.</span><span class="sxs-lookup"><span data-stu-id="7e6f6-132">You can enable Auto-inflate during an Azure Resource Manager template deployment.</span></span> <span data-ttu-id="7e6f6-133">Például set hello `isAutoInflateEnabled` tulajdonság túl**igaz** és `maximumThroughputUnits` too10.</span><span class="sxs-lookup"><span data-stu-id="7e6f6-133">For example, set hello `isAutoInflateEnabled` property too**true** and set `maximumThroughputUnits` too10.</span></span>

```json
"resources": [
        {
            "apiVersion": "2017-04-01",
            "name": "[parameters('namespaceName')]",
            "type": "Microsoft.EventHub/Namespaces",
            "location": "[variables('location')]",
            "sku": {
                "name": "Standard",
                "tier": "Standard"
            },
            "properties": {
                "isAutoInflateEnabled": true,
                "maximumThroughputUnits": 10
            },
            "resources": [
                {
                    "apiVersion": "2017-04-01",
                    "name": "[parameters('eventHubName')]",
                    "type": "EventHubs",
                    "dependsOn": [
                        "[concat('Microsoft.EventHub/namespaces/', parameters('namespaceName'))]"
                    ],
                    "properties": {},
                    "resources": [
                        {
                            "apiVersion": "2017-04-01",
                            "name": "[parameters('consumerGroupName')]",
                            "type": "ConsumerGroups",
                            "dependsOn": [
                                "[parameters('eventHubName')]"
                            ],
                            "properties": {}
                        }
                    ]
                }
            ]
        }
    ]
```

<span data-ttu-id="7e6f6-134">Hello teljes sablon, lásd: hello [létrehozása az Event Hubs-névteret és engedélyezése megnöveli](https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-inflate) sablon a Githubon.</span><span class="sxs-lookup"><span data-stu-id="7e6f6-134">For hello complete template, see hello [Create Event Hubs namespace and enable inflate](https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-inflate) template on GitHub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7e6f6-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7e6f6-135">Next steps</span></span>

<span data-ttu-id="7e6f6-136">További információ az Event Hubs érhetők el a következő hivatkozások hello:</span><span class="sxs-lookup"><span data-stu-id="7e6f6-136">You can learn more about Event Hubs by visiting hello following links:</span></span>

* [<span data-ttu-id="7e6f6-137">Event Hubs – áttekintés</span><span class="sxs-lookup"><span data-stu-id="7e6f6-137">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="7e6f6-138">Event Hub létrehozása</span><span class="sxs-lookup"><span data-stu-id="7e6f6-138">Create an Event Hub</span></span>](event-hubs-create.md)
