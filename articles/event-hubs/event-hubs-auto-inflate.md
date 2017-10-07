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
# <a name="automatically-scale-up-azure-event-hubs-throughput-units"></a>Azure Event Hubs átviteli egységek automatikus méretezése

## <a name="overview"></a>Áttekintés

Az Azure Event Hubs egy kiválóan méretezhető adatfolyam platform. Az Event Hubs az ügyfelek gyakran a használat növelése után bevezetési toohello szolgáltatás. Ilyen növekszik növekvő előre meghatározott hello átviteli egységek (TUs) tooscale Event Hubs igényelnek, és nagyobb átviteli sebesség kezelésére. Hello *automatikus megnöveli* az Event hubs szolgáltatás automatikusan rendkívül TUs toomeet igényei hello száma. TUs növelése megakadályozza, hogy a sávszélesség-szabályozás forgatókönyvek, amelyben:

* Adatok érkező haladhatja meg a díjszabás TUs beállítása.
* Kimenő kérelem haladhatja meg a díjszabás TUs beállítása.

## <a name="how-auto-inflate-works"></a>Automatikus megnöveli működése

Event Hubs forgalom átviteli egységek vezérli. Egyetlen SO lehetővé teszi, hogy 1 MB másodpercenkénti bemenő és kimenő forgalom kétszer adott mennyisége. Standard Event Hubs 1 – 20 átviteli egység konfigurálható. Automatikus megnöveli lehetővé teszi toostart kis hello minimálisan szükséges átviteli egységek használatával. hello szolgáltatás automatikusan toohello maximális átviteli egységek van szükség, attól függően, hogy a forgalom hello növekedése majd arányosan. Automatikus megnöveli hello a következő előnyöket biztosítja:

- Egy hatékony méretezési mechanizmus toostart kis és, ha Ön felskálázott nő.
- Automatikus méretezése toohello megadott felső korlát nélkül szabályozási problémákat.
- Több vezérlése skálázás, meghatározhatja, mikor és hogyan sok tooscale.

## <a name="enable-auto-inflate-on-a-namespace"></a>Engedélyezze az automatikus megnöveli a névtér

Engedélyezi, vagy tiltsa le az automatikus megnöveli a névtér hello a következő módszerek valamelyikével:

1. Hello [Azure-portálon](https://portal.azure.com).
2. Az Azure Resource Manager-sablon.

### <a name="enable-auto-inflate-through-hello-portal"></a>Engedélyezze az automatikus megnöveli hello portálon keresztül

Engedélyezheti a hello automatikus megnöveli a szolgáltatást egy névtér, ha az Event Hubs névtér létrehozása:
 
![](./media/event-hubs-auto-inflate/event-hubs-auto-inflate1.png)

A funkció engedélyezését kezdje kis lépésekkel az átviteli egységek, és a használati mutatói igények növekedése méret. hello infláció felső határa nem érinti a díjszabást hello száma óránként használt TUs függ.

Továbbá engedélyezheti az automatikus megnöveli hello segítségével **méretezési** lehetőséget a hello portálon hello-beállítások panelen:
 
![](./media/event-hubs-auto-inflate/event-hubs-auto-inflate2.png)

### <a name="enable-auto-inflate-using-an-azure-resource-manager-template"></a>Engedélyezze az automatikus-megnöveli Azure Resource Manager-sablonnal

Automatikus megnöveli az Azure Resource Manager sablon telepítése során engedélyezheti. Például set hello `isAutoInflateEnabled` tulajdonság túl**igaz** és `maximumThroughputUnits` too10.

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

Hello teljes sablon, lásd: hello [létrehozása az Event Hubs-névteret és engedélyezése megnöveli](https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-inflate) sablon a Githubon.

## <a name="next-steps"></a>Következő lépések

További információ az Event Hubs érhetők el a következő hivatkozások hello:

* [Event Hubs – áttekintés](event-hubs-what-is-event-hubs.md)
* [Event Hub létrehozása](event-hubs-create.md)
