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
# <a name="automatically-scale-up-azure-event-hubs-throughput-units"></a>Azure Event Hubs átviteli egységek automatikus méretezése

## <a name="overview"></a>Áttekintés

Az Azure Event Hubs egy kiválóan méretezhető adatfolyam platform. Az Event Hubs az ügyfelek gyakran a használat növelése után a szolgáltatás bevezetése. Ilyen növekszik szükséges növelése az előre meghatározott átviteli egységek (TUs) az Event Hubs skálázása, és nagyobb átviteli sebesség kezelésére. A *automatikus megnöveli* az Event hubs szolgáltatás automatikusan méretezi használati igényeinek TUs számát. TUs növelése megakadályozza, hogy a sávszélesség-szabályozás forgatókönyvek, amelyben:

* Adatok érkező haladhatja meg a díjszabás TUs beállítása.
* Kimenő kérelem haladhatja meg a díjszabás TUs beállítása.

## <a name="how-auto-inflate-works"></a>Automatikus megnöveli működése

Event Hubs forgalom átviteli egységek vezérli. Egyetlen SO lehetővé teszi, hogy 1 MB másodpercenkénti bemenő és kimenő forgalom kétszer adott mennyisége. Standard Event Hubs 1 – 20 átviteli egység konfigurálható. Automatikus megnöveli lehetővé teszi a kisebb a minimálisan szükséges átviteli egységek használatával. A szolgáltatás majd alkalmazkodnak automatikusan van szükség, attól függően, hogy az adatforgalom növekedésének átviteli egységek maximális száma. Automatikus megnöveli a következő előnyökkel jár:

- Egy hatékony méretezési mechanizmus kezdje kis lépésekkel, és növelhető növelheti.
- Automatikus méretezése a megadott felső határérték nélkül szabályozási problémákat.
- Több vezérlése skálázás, meghatározhatja, mikor és milyen mértékű skála.

## <a name="enable-auto-inflate-on-a-namespace"></a>Engedélyezze az automatikus megnöveli a névtér

Engedélyezze, vagy tiltsa le az automatikus megnöveli a névtér a következő módszerek valamelyikével:

1. A [Azure-portálon](https://portal.azure.com).
2. Az Azure Resource Manager-sablon.

### <a name="enable-auto-inflate-through-the-portal"></a>Engedélyezze az automatikus megnöveli a portálon keresztül

Engedélyezheti az automatikus megnöveli funkciót a névtér egy Event Hubs névtér létrehozása során:
 
![](./media/event-hubs-auto-inflate/event-hubs-auto-inflate1.png)

A funkció engedélyezését kezdje kis lépésekkel az átviteli egységek, és a használati mutatói igények növekedése méret. Inflációs felső határa nem érinti a díjszabás, óránként használt TUs száma függ.

Automatikus-megnöveli használatával is engedélyezheti a **méretezési** lehetőséget a beállítások panelen a portálon:
 
![](./media/event-hubs-auto-inflate/event-hubs-auto-inflate2.png)

### <a name="enable-auto-inflate-using-an-azure-resource-manager-template"></a>Engedélyezze az automatikus-megnöveli Azure Resource Manager-sablonnal

Automatikus megnöveli az Azure Resource Manager sablon telepítése során engedélyezheti. Például a `isAutoInflateEnabled` tulajdonságot **igaz** és `maximumThroughputUnits` 10-re.

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

A teljes sablon, tekintse meg a [létrehozása az Event Hubs-névteret és engedélyezése megnöveli](https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-inflate) sablon a Githubon.

## <a name="next-steps"></a>Következő lépések

Az alábbi webhelyeken további információt talál az Event Hubsról:

* [Event Hubs – áttekintés](event-hubs-what-is-event-hubs.md)
* [Event Hub létrehozása](event-hubs-create.md)
