---
title: "aaaURL-alapú tartalom útválasztási áttekintése |} Microsoft Docs"
description: "Ezen a lapon hello alkalmazás átjáró URL-alapú tartalom útválasztási, UrlPathMap konfigurációs és PathBasedRouting szabály áttekintést nyújt."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: 
ms.assetid: 4409159b-e22d-4c9a-a103-f5d32465d163
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/09/2017
ms.author: gwallace
ms.openlocfilehash: 5094b42625baffeb395beace68db0d269e46080c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="url-path-based-routing-overview"></a>Az URL-alapú útválasztás áttekintése

Elérési út URL-cím alapú útválasztási lehetővé teszi, hogy Ön tooroute forgalom tooback-a befejezési kiszolgálókészletek a hello kérelem URL-cím elérési útján alapszik. 

Hello forgatókönyvek egyike különböző tartalomtípusok toodifferent háttérkészletek server tooroute kérelmek.

A következő példa hello, Alkalmazásátjáró van szolgáltató contoso.com három háttér-kiszolgálófiók készletek az adatforgalmat például: VideoServerPool ImageServerPool és DefaultServerPool.

![imageURLroute](./media/application-gateway-url-route-overview/figure1.png)

Kérések a http://contoso.com/video * irányított tooVideoServerPool, és http://contoso.com/images * irányított tooImageServerPool. DefaultServerPool van jelölve, ha hello elérési minták egyike.

> [!IMPORTANT]
> Szabályok feldolgozása hello sorrendben hello portálon. Erősen ajánlott tooconfigure többhelyes figyelői első előzetes tooconfiguring egy alapszintű figyelő is.  Ez biztosítja, hogy forgalom lekérdezi irányított toohello jobb vissza végén. Ha előbb egy alapszintű figyelő szerepel a listában, és az megfelel egy bejövő kérésnek, a figyelő feldolgozza azt.

## <a name="urlpathmap-configuration-element"></a>Az UrlPathMap konfigurációs elem

hello urlPathMap elem használt toospecify elérési minták tooback-end kiszolgáló készlet leképezéseket. hello alábbi Kódpélda az hello részlet urlPathMap elem a sablonfájl.

```json
"urlPathMaps": [{
    "name": "{urlpathMapName}",
    "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/urlPathMaps/{urlpathMapName}",
    "properties": {
        "defaultBackendAddressPool": {
            "id": "/subscriptions/    {subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/backendAddressPools/{poolName1}"
        },
        "defaultBackendHttpSettings": {
            "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/backendHttpSettingsList/{settingname1}"
        },
        "pathRules": [{
            "name": "{pathRuleName}",
            "properties": {
                "paths": [
                    "{pathPattern}"
                ],
                "backendAddressPool": {
                    "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/backendAddressPools/{poolName2}"
                },
                "backendHttpsettings": {
                    "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/backendHttpsettingsList/{settingName2}"
                }
            }
        }]
    }
}]
```

> [!NOTE]
> PathPattern: Ez a beállítás akkor elérési minták toomatch listáját. Minden egyes kell kezdődnie / és hello egyetlen hely, ahol a "*" van: hello vége a következő engedélyezett a "/." toohello elérési matcher táplált hello karakterlánc nem tartalmaz sem szöveges hello után először? vagy #, és ezek karakter itt nem engedélyezett.

További információért tekintse át az [URL-alapú átirányításhoz készült Resource Manager-sablonokat](https://azure.microsoft.com/documentation/templates/201-application-gateway-url-path-based-routing).

## <a name="pathbasedrouting-rule"></a>PathBasedRouting szabály

PathBasedRouting típusú RequestRoutingRule használt toobind egy figyelő tooa urlPathMap. Minden kérés, amely ehhez a figyelőhöz kapcsolódik, az urlPathMap elemben meghatározott irányelvek alapján lesz átirányítva.
A PathBasedRouting szabály kódrészlete:

```json
"requestRoutingRules": [
    {

"name": "{ruleName}",
"id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/requestRoutingRules/{ruleName}",
"properties": {
    "ruleType": "PathBasedRouting",
    "httpListener": {
        "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/httpListeners/<listenerName>"
    },
    "urlPathMap": {
        "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/ urlPathMaps/{urlpathMapName}"
    },

}
    }
]
```

## <a name="next-steps"></a>Következő lépések

Tartalom útválasztási URL-alapú megismerését követően nyissa meg túl[URL-alapú útválasztás használatával Alkalmazásátjáró létrehozása](application-gateway-create-url-route-portal.md) toocreate Alkalmazásátjáró URL-cím útválasztási szabályokat.
