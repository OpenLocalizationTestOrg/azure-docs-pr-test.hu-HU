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
# <a name="url-path-based-routing-overview"></a><span data-ttu-id="9331c-103">Az URL-alapú útválasztás áttekintése</span><span class="sxs-lookup"><span data-stu-id="9331c-103">URL Path Based Routing overview</span></span>

<span data-ttu-id="9331c-104">Elérési út URL-cím alapú útválasztási lehetővé teszi, hogy Ön tooroute forgalom tooback-a befejezési kiszolgálókészletek a hello kérelem URL-cím elérési útján alapszik.</span><span class="sxs-lookup"><span data-stu-id="9331c-104">URL Path Based Routing allows you tooroute traffic tooback-end server pools based on URL Paths of hello request.</span></span> 

<span data-ttu-id="9331c-105">Hello forgatókönyvek egyike különböző tartalomtípusok toodifferent háttérkészletek server tooroute kérelmek.</span><span class="sxs-lookup"><span data-stu-id="9331c-105">One of hello scenarios is tooroute requests for different content types toodifferent backend server pools.</span></span>

<span data-ttu-id="9331c-106">A következő példa hello, Alkalmazásátjáró van szolgáltató contoso.com három háttér-kiszolgálófiók készletek az adatforgalmat például: VideoServerPool ImageServerPool és DefaultServerPool.</span><span class="sxs-lookup"><span data-stu-id="9331c-106">In hello following example, Application Gateway is serving traffic for contoso.com from three back-end server pools for example: VideoServerPool, ImageServerPool, and DefaultServerPool.</span></span>

![imageURLroute](./media/application-gateway-url-route-overview/figure1.png)

<span data-ttu-id="9331c-108">Kérések a http://contoso.com/video * irányított tooVideoServerPool, és http://contoso.com/images * irányított tooImageServerPool.</span><span class="sxs-lookup"><span data-stu-id="9331c-108">Requests for http://contoso.com/video* are routed tooVideoServerPool, and http://contoso.com/images* are routed tooImageServerPool.</span></span> <span data-ttu-id="9331c-109">DefaultServerPool van jelölve, ha hello elérési minták egyike.</span><span class="sxs-lookup"><span data-stu-id="9331c-109">DefaultServerPool is selected if none of hello path patterns match.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9331c-110">Szabályok feldolgozása hello sorrendben hello portálon.</span><span class="sxs-lookup"><span data-stu-id="9331c-110">Rules are processed in hello order they are listed in hello portal.</span></span> <span data-ttu-id="9331c-111">Erősen ajánlott tooconfigure többhelyes figyelői első előzetes tooconfiguring egy alapszintű figyelő is.</span><span class="sxs-lookup"><span data-stu-id="9331c-111">It is highly recommended tooconfigure multi-site listeners first prior tooconfiguring a basic listener.</span></span>  <span data-ttu-id="9331c-112">Ez biztosítja, hogy forgalom lekérdezi irányított toohello jobb vissza végén.</span><span class="sxs-lookup"><span data-stu-id="9331c-112">This ensures that traffic gets routed toohello right back end.</span></span> <span data-ttu-id="9331c-113">Ha előbb egy alapszintű figyelő szerepel a listában, és az megfelel egy bejövő kérésnek, a figyelő feldolgozza azt.</span><span class="sxs-lookup"><span data-stu-id="9331c-113">If a basic listener is listed first and matches an incoming request, it gets processed by that listener.</span></span>

## <a name="urlpathmap-configuration-element"></a><span data-ttu-id="9331c-114">Az UrlPathMap konfigurációs elem</span><span class="sxs-lookup"><span data-stu-id="9331c-114">UrlPathMap configuration element</span></span>

<span data-ttu-id="9331c-115">hello urlPathMap elem használt toospecify elérési minták tooback-end kiszolgáló készlet leképezéseket.</span><span class="sxs-lookup"><span data-stu-id="9331c-115">hello urlPathMap element is used toospecify Path patterns tooback-end server pool mappings.</span></span> <span data-ttu-id="9331c-116">hello alábbi Kódpélda az hello részlet urlPathMap elem a sablonfájl.</span><span class="sxs-lookup"><span data-stu-id="9331c-116">hello following code example is hello snippet of urlPathMap element from template file.</span></span>

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
> <span data-ttu-id="9331c-117">PathPattern: Ez a beállítás akkor elérési minták toomatch listáját.</span><span class="sxs-lookup"><span data-stu-id="9331c-117">PathPattern: This setting is a list of path patterns toomatch.</span></span> <span data-ttu-id="9331c-118">Minden egyes kell kezdődnie / és hello egyetlen hely, ahol a "*" van: hello vége a következő engedélyezett a "/."</span><span class="sxs-lookup"><span data-stu-id="9331c-118">Each must start with / and hello only place a "*" is allowed is at hello end following a "/."</span></span> <span data-ttu-id="9331c-119">toohello elérési matcher táplált hello karakterlánc nem tartalmaz sem szöveges hello után először? vagy #, és ezek karakter itt nem engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="9331c-119">hello string fed toohello path matcher does not include any text after hello first? or #, and those chars are not allowed here.</span></span>

<span data-ttu-id="9331c-120">További információért tekintse át az [URL-alapú átirányításhoz készült Resource Manager-sablonokat](https://azure.microsoft.com/documentation/templates/201-application-gateway-url-path-based-routing).</span><span class="sxs-lookup"><span data-stu-id="9331c-120">You can check out a [Resource Manager template using URL-based routing](https://azure.microsoft.com/documentation/templates/201-application-gateway-url-path-based-routing) for more information.</span></span>

## <a name="pathbasedrouting-rule"></a><span data-ttu-id="9331c-121">PathBasedRouting szabály</span><span class="sxs-lookup"><span data-stu-id="9331c-121">PathBasedRouting rule</span></span>

<span data-ttu-id="9331c-122">PathBasedRouting típusú RequestRoutingRule használt toobind egy figyelő tooa urlPathMap.</span><span class="sxs-lookup"><span data-stu-id="9331c-122">RequestRoutingRule of type PathBasedRouting is used toobind a listener tooa urlPathMap.</span></span> <span data-ttu-id="9331c-123">Minden kérés, amely ehhez a figyelőhöz kapcsolódik, az urlPathMap elemben meghatározott irányelvek alapján lesz átirányítva.</span><span class="sxs-lookup"><span data-stu-id="9331c-123">All requests that are received for this listener are routed based on policy specified in urlPathMap.</span></span>
<span data-ttu-id="9331c-124">A PathBasedRouting szabály kódrészlete:</span><span class="sxs-lookup"><span data-stu-id="9331c-124">Snippet of PathBasedRouting rule:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="9331c-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9331c-125">Next steps</span></span>

<span data-ttu-id="9331c-126">Tartalom útválasztási URL-alapú megismerését követően nyissa meg túl[URL-alapú útválasztás használatával Alkalmazásátjáró létrehozása](application-gateway-create-url-route-portal.md) toocreate Alkalmazásátjáró URL-cím útválasztási szabályokat.</span><span class="sxs-lookup"><span data-stu-id="9331c-126">After learning about URL-based content routing, go too[create an application gateway using URL-based routing](application-gateway-create-url-route-portal.md) toocreate an application gateway with URL routing rules.</span></span>
