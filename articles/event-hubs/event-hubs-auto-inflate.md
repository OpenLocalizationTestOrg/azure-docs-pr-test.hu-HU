---
title: "Automatikus méretezése az Azure Event Hubs átviteli egységek |} Microsoft Docs"
description: "Automatikus megnöveli a névtér automatikus méretezése az átviteli egységek engedélyezése"
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
ms.openlocfilehash: b085091ea7bfd601efb0eee84144ddd091422d6e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="automatically-scale-up-azure-event-hubs-throughput-units"></a><span data-ttu-id="f6f86-103">Azure Event Hubs átviteli egységek automatikus méretezése</span><span class="sxs-lookup"><span data-stu-id="f6f86-103">Automatically scale up Azure Event Hubs throughput units</span></span>

## <a name="overview"></a><span data-ttu-id="f6f86-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="f6f86-104">Overview</span></span>

<span data-ttu-id="f6f86-105">Az Azure Event Hubs egy kiválóan méretezhető adatfolyam platform.</span><span class="sxs-lookup"><span data-stu-id="f6f86-105">Azure Event Hubs is a highly scalable data streaming platform.</span></span> <span data-ttu-id="f6f86-106">Az Event Hubs az ügyfelek gyakran a használat növelése után a szolgáltatás bevezetése.</span><span class="sxs-lookup"><span data-stu-id="f6f86-106">As such, Event Hubs customers often increase their usage after onboarding to the service.</span></span> <span data-ttu-id="f6f86-107">Ilyen növekszik szükséges növelése az előre meghatározott átviteli egységek (TUs) az Event Hubs skálázása, és nagyobb átviteli sebesség kezelésére.</span><span class="sxs-lookup"><span data-stu-id="f6f86-107">Such increases require increasing the predetermined throughput units (TUs) to scale Event Hubs and handle larger transfer rates.</span></span> <span data-ttu-id="f6f86-108">A *automatikus megnöveli* az Event hubs szolgáltatás automatikusan méretezi használati igényeinek TUs számát.</span><span class="sxs-lookup"><span data-stu-id="f6f86-108">The *Auto-inflate* feature of Event Hubs automatically scales up the number of TUs to meet usage needs.</span></span> <span data-ttu-id="f6f86-109">TUs növelése megakadályozza, hogy a sávszélesség-szabályozás forgatókönyvek, amelyben:</span><span class="sxs-lookup"><span data-stu-id="f6f86-109">Increasing TUs prevents throttling scenarios, in which:</span></span>

* <span data-ttu-id="f6f86-110">Adatok érkező haladhatja meg a díjszabás TUs beállítása.</span><span class="sxs-lookup"><span data-stu-id="f6f86-110">Data ingress rates exceed set TUs.</span></span>
* <span data-ttu-id="f6f86-111">Kimenő kérelem haladhatja meg a díjszabás TUs beállítása.</span><span class="sxs-lookup"><span data-stu-id="f6f86-111">Data egress request rates exceed set TUs.</span></span>

## <a name="how-auto-inflate-works"></a><span data-ttu-id="f6f86-112">Automatikus megnöveli működése</span><span class="sxs-lookup"><span data-stu-id="f6f86-112">How Auto-inflate works</span></span>

<span data-ttu-id="f6f86-113">Event Hubs forgalom átviteli egységek vezérli.</span><span class="sxs-lookup"><span data-stu-id="f6f86-113">Event Hubs traffic is controlled by throughput units.</span></span> <span data-ttu-id="f6f86-114">Egyetlen SO lehetővé teszi, hogy 1 MB másodpercenkénti bemenő és kimenő forgalom kétszer adott mennyisége.</span><span class="sxs-lookup"><span data-stu-id="f6f86-114">A single TU allows 1 MB per second of ingress and twice that amount of egress.</span></span> <span data-ttu-id="f6f86-115">Standard Event Hubs 1 – 20 átviteli egység konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="f6f86-115">Standard Event Hubs can be configured with 1-20 throughput units.</span></span> <span data-ttu-id="f6f86-116">Automatikus megnöveli lehetővé teszi a kisebb a minimálisan szükséges átviteli egységek használatával.</span><span class="sxs-lookup"><span data-stu-id="f6f86-116">Auto-inflate enables you to start small with the minimum required throughput units.</span></span> <span data-ttu-id="f6f86-117">A szolgáltatás majd alkalmazkodnak automatikusan van szükség, attól függően, hogy az adatforgalom növekedésének átviteli egységek maximális száma.</span><span class="sxs-lookup"><span data-stu-id="f6f86-117">The feature then scales automatically to the maximum limit of throughput units you need, depending on the increase in your traffic.</span></span> <span data-ttu-id="f6f86-118">Automatikus megnöveli a következő előnyökkel jár:</span><span class="sxs-lookup"><span data-stu-id="f6f86-118">Auto-inflate provides the following benefits:</span></span>

- <span data-ttu-id="f6f86-119">Egy hatékony méretezési mechanizmus kezdje kis lépésekkel, és növelhető növelheti.</span><span class="sxs-lookup"><span data-stu-id="f6f86-119">An efficient scaling mechanism to start small and scale up as you grow.</span></span>
- <span data-ttu-id="f6f86-120">Automatikus méretezése a megadott felső határérték nélkül szabályozási problémákat.</span><span class="sxs-lookup"><span data-stu-id="f6f86-120">Automatically scale to the specified upper limit without throttling issues.</span></span>
- <span data-ttu-id="f6f86-121">Több vezérlése skálázás, meghatározhatja, mikor és milyen mértékű skála.</span><span class="sxs-lookup"><span data-stu-id="f6f86-121">More control over scaling, as you control when and how much to scale.</span></span>

## <a name="enable-auto-inflate-on-a-namespace"></a><span data-ttu-id="f6f86-122">Engedélyezze az automatikus megnöveli a névtér</span><span class="sxs-lookup"><span data-stu-id="f6f86-122">Enable Auto-inflate on a namespace</span></span>

<span data-ttu-id="f6f86-123">Engedélyezze, vagy tiltsa le az automatikus megnöveli a névtér a következő módszerek valamelyikével:</span><span class="sxs-lookup"><span data-stu-id="f6f86-123">You can enable or disable Auto-inflate on a namespace using either of the following methods:</span></span>

1. <span data-ttu-id="f6f86-124">A [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f6f86-124">The [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="f6f86-125">Az Azure Resource Manager-sablon.</span><span class="sxs-lookup"><span data-stu-id="f6f86-125">An Azure Resource Manager template.</span></span>

### <a name="enable-auto-inflate-through-the-portal"></a><span data-ttu-id="f6f86-126">Engedélyezze az automatikus megnöveli a portálon keresztül</span><span class="sxs-lookup"><span data-stu-id="f6f86-126">Enable Auto-inflate through the portal</span></span>

<span data-ttu-id="f6f86-127">Engedélyezheti az automatikus megnöveli funkciót a névtér egy Event Hubs névtér létrehozása során:</span><span class="sxs-lookup"><span data-stu-id="f6f86-127">You can enable the Auto-inflate feature on a namespace when creating an Event Hubs namespace:</span></span>
 
![](./media/event-hubs-auto-inflate/event-hubs-auto-inflate1.png)

<span data-ttu-id="f6f86-128">A funkció engedélyezését kezdje kis lépésekkel az átviteli egységek, és a használati mutatói igények növekedése méret.</span><span class="sxs-lookup"><span data-stu-id="f6f86-128">With this option enabled, you can start small on your throughput units and scale up as your usage needs increase.</span></span> <span data-ttu-id="f6f86-129">Inflációs felső határa nem érinti a díjszabás, óránként használt TUs száma függ.</span><span class="sxs-lookup"><span data-stu-id="f6f86-129">The upper limit for inflation does not affect pricing, which depends on the number of TUs used per hour.</span></span>

<span data-ttu-id="f6f86-130">Automatikus-megnöveli használatával is engedélyezheti a **méretezési** lehetőséget a beállítások panelen a portálon:</span><span class="sxs-lookup"><span data-stu-id="f6f86-130">You can also enable Auto-inflate using the **Scale** option on the settings blade in the portal:</span></span>
 
![](./media/event-hubs-auto-inflate/event-hubs-auto-inflate2.png)

### <a name="enable-auto-inflate-using-an-azure-resource-manager-template"></a><span data-ttu-id="f6f86-131">Engedélyezze az automatikus-megnöveli Azure Resource Manager-sablonnal</span><span class="sxs-lookup"><span data-stu-id="f6f86-131">Enable Auto-Inflate using an Azure Resource Manager template</span></span>

<span data-ttu-id="f6f86-132">Automatikus megnöveli az Azure Resource Manager sablon telepítése során engedélyezheti.</span><span class="sxs-lookup"><span data-stu-id="f6f86-132">You can enable Auto-inflate during an Azure Resource Manager template deployment.</span></span> <span data-ttu-id="f6f86-133">Például a `isAutoInflateEnabled` tulajdonságot **igaz** és `maximumThroughputUnits` 10-re.</span><span class="sxs-lookup"><span data-stu-id="f6f86-133">For example, set the `isAutoInflateEnabled` property to **true** and set `maximumThroughputUnits` to 10.</span></span>

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

<span data-ttu-id="f6f86-134">A teljes sablon, tekintse meg a [létrehozása az Event Hubs-névteret és engedélyezése megnöveli](https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-inflate) sablon a Githubon.</span><span class="sxs-lookup"><span data-stu-id="f6f86-134">For the complete template, see the [Create Event Hubs namespace and enable inflate](https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-inflate) template on GitHub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f6f86-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f6f86-135">Next steps</span></span>

<span data-ttu-id="f6f86-136">Az alábbi webhelyeken további információt talál az Event Hubsról:</span><span class="sxs-lookup"><span data-stu-id="f6f86-136">You can learn more about Event Hubs by visiting the following links:</span></span>

* [<span data-ttu-id="f6f86-137">Event Hubs – áttekintés</span><span class="sxs-lookup"><span data-stu-id="f6f86-137">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="f6f86-138">Event Hub létrehozása</span><span class="sxs-lookup"><span data-stu-id="f6f86-138">Create an Event Hub</span></span>](event-hubs-create.md)
