---
title: "Azure Application Gateway aaaWebSocket támogatást |} Microsoft Docs"
description: "Ezen a lapon hello alkalmazás átjáró WebSocket támogatási áttekintést nyújt."
documentationcenter: na
services: application-gateway
author: amsriva
manager: rossort
editor: amsriva
ms.assetid: 8968dac1-e9bc-4fa1-8415-96decacab83f
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/08/2017
ms.author: amsriva
ms.openlocfilehash: 3776117803e8559ad243c2d4c3dd661199c1e48a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-websocket-support-in-application-gateway"></a><span data-ttu-id="a0ded-103">Alkalmazásátjáró WebSocket támogatást áttekintése</span><span class="sxs-lookup"><span data-stu-id="a0ded-103">Overview of WebSocket support in Application Gateway</span></span>

<span data-ttu-id="a0ded-104">Alkalmazásátjáró közötti átjáró különböző méretű WebSocket natív támogatást biztosít.</span><span class="sxs-lookup"><span data-stu-id="a0ded-104">Application Gateway provides native support for WebSocket across all gateway sizes.</span></span> <span data-ttu-id="a0ded-105">Nincs felhasználó által konfigurálható beállítás tooselectively engedélyezése vagy letiltása WebSocket támogatási.</span><span class="sxs-lookup"><span data-stu-id="a0ded-105">There is no user-configurable setting tooselectively enable or disable WebSocket support.</span></span> 

<span data-ttu-id="a0ded-106">A szabványos WebSocket protokoll [RFC6455](https://tools.ietf.org/html/rfc6455) lehetővé teszi, hogy a kiszolgáló és az ügyfél között a teljes kétirányú kommunikációs egy hosszú ideig tartó TCP-kapcsolaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="a0ded-106">WebSocket protocol standardized in [RFC6455](https://tools.ietf.org/html/rfc6455) enables a full duplex communication between a server and a client over a long running TCP connection.</span></span> <span data-ttu-id="a0ded-107">Ezzel a funkcióval a több interaktív kommunikációhoz hello és hello ügyfél, amely kétirányú anélkül hello a lekérdezéshez, mint a HTTP-alapú megvalósításokhoz lehet.</span><span class="sxs-lookup"><span data-stu-id="a0ded-107">This feature allows for a more interactive communication between hello web server and hello client, which can be bidirectional without hello need for polling as required in HTTP-based implementations.</span></span> <span data-ttu-id="a0ded-108">WebSocket alacsony terhelés eltérően a HTTP és a is újbóli hello azonos több kérelem/válasz az erőforrások hatékonyabb kihasználását eredményezi TCP-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="a0ded-108">WebSocket has low overhead unlike HTTP and can reuse hello same TCP connection for multiple request/responses resulting in a more efficient utilization of resources.</span></span> <span data-ttu-id="a0ded-109">WebSocket protokoll tervezett toowork hagyományos HTTP-port a 80-as és 443-as porton keresztül történik.</span><span class="sxs-lookup"><span data-stu-id="a0ded-109">WebSocket protocols are designed toowork over traditional HTTP ports of 80 and 443.</span></span>

<span data-ttu-id="a0ded-110">A WebSocket-forgalom 80-as vagy 443-as port tooreceive egy szabványos HTTP-figyelő használatának folytatása.</span><span class="sxs-lookup"><span data-stu-id="a0ded-110">You can continue using a standard HTTP listener on port 80 or 443 tooreceive WebSocket traffic.</span></span> <span data-ttu-id="a0ded-111">WebSocket forgalom nem irányított toohello WebSocket engedélyezett hello megfelelő háttérkészlet használó alkalmazás átjáró szabályok a háttérkiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="a0ded-111">WebSocket traffic is then directed toohello WebSocket enabled backend server using hello appropriate backend pool as specified in application gateway rules.</span></span> <span data-ttu-id="a0ded-112">hello háttérkiszolgáló kell válaszolnia toohello alkalmazás átjáró mintavételek menüpontban, amely ismerteti a hello [állapot-mintavételi áttekintése](application-gateway-probe-overview.md) szakasz.</span><span class="sxs-lookup"><span data-stu-id="a0ded-112">hello backend server must respond toohello application gateway probes, which are described in hello [health probe overview](application-gateway-probe-overview.md) section.</span></span> <span data-ttu-id="a0ded-113">Alkalmazás átjáró állapotfigyelő mintavételt csak olyan HTTP/HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a0ded-113">Application gateway health probes are HTTP/HTTPS only.</span></span> <span data-ttu-id="a0ded-114">Minden egyes háttérkiszolgáló alkalmazás tooroute WebSocket forgalom toohello átjárókiszolgáló tooHTTP mintavételt kell válaszolnia.</span><span class="sxs-lookup"><span data-stu-id="a0ded-114">Each backend server must respond tooHTTP probes for application gateway tooroute WebSocket traffic toohello server.</span></span>

## <a name="listener-configuration-element"></a><span data-ttu-id="a0ded-115">Figyelő konfigurációs elem</span><span class="sxs-lookup"><span data-stu-id="a0ded-115">Listener configuration element</span></span>

<span data-ttu-id="a0ded-116">Egy meglévő HTTP-figyelő lehet használt toosupport WebSocket-forgalmat.</span><span class="sxs-lookup"><span data-stu-id="a0ded-116">An existing HTTP listener can be used toosupport WebSocket traffic.</span></span> <span data-ttu-id="a0ded-117">hello az alábbiakban látható egy minta sablon fájlból httpListeners elem részlet.</span><span class="sxs-lookup"><span data-stu-id="a0ded-117">hello following is a snippet of an httpListeners element from a sample template file.</span></span> <span data-ttu-id="a0ded-118">Ehhez a HTTP és HTTPS figyelői toosupport WebSocket kell, és biztosítják a WebSocket-forgalmát.</span><span class="sxs-lookup"><span data-stu-id="a0ded-118">You would need both HTTP and HTTPS listeners toosupport WebSocket and secure WebSocket traffic.</span></span> <span data-ttu-id="a0ded-119">Hasonló módon használhatja a hello [portal](application-gateway-create-gateway-portal.md) vagy [PowerShell](application-gateway-create-gateway-arm.md) toocreate port 80/443-as toosupport WebSocket forgalom figyelőinek az Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="a0ded-119">Similarly you can use hello [portal](application-gateway-create-gateway-portal.md) or [PowerShell](application-gateway-create-gateway-arm.md) toocreate an application gateway with listeners on port 80/443 toosupport WebSocket traffic.</span></span>

```json
"httpListeners": [
        {
            "name": "appGatewayHttpsListener",
            "properties": {
                "FrontendIPConfiguration": {
                    "Id": "/subscriptions/{subscriptionId/resourceGroups/{resourceGroupName/providers/Microsoft.Network/applicationGateways/{applicationGatewayName/frontendIPConfigurations/DefaultFrontendPublicIP"
                },
                "FrontendPort": {
                    "Id": "/subscriptions/{subscriptionId/resourceGroups/{resourceGroupName/providers/Microsoft.Network/applicationGateways/{applicationGatewayName/frontendPorts/appGatewayFrontendPort443'"
                },
                "Protocol": "Https",
                "SslCertificate": {
                    "Id": "/subscriptions/{subscriptionId/resourceGroups/{resourceGroupName/providers/Microsoft.Network/applicationGateways/{applicationGatewayName/sslCertificates/appGatewaySslCert1'"
                },
            }
        },
        {
            "name": "appGatewayHttpListener",
            "properties": {
                "FrontendIPConfiguration": {
                    "Id": "/subscriptions/{subscriptionId/resourceGroups/{resourceGroupName/providers/Microsoft.Network/applicationGateways/{applicationGatewayName/frontendIPConfigurations/appGatewayFrontendIP'"
                },
                "FrontendPort": {
                    "Id": "/subscriptions/{subscriptionId/resourceGroups/{resourceGroupName/providers/Microsoft.Network/applicationGateways/{applicationGatewayName/frontendPorts/appGatewayFrontendPort80'"
                },
                "Protocol": "Http",
            }
        }
    ],
```

## <a name="backendaddresspool-backendhttpsetting-and-routing-rule-configuration"></a><span data-ttu-id="a0ded-120">BackendAddressPool BackendHttpSetting és útválasztás szabály konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a0ded-120">BackendAddressPool, BackendHttpSetting, and Routing rule configuration</span></span>

<span data-ttu-id="a0ded-121">Egy BackendAddressPool használt toodefine engedélyezett WebSocket-kiszolgálókkal háttérkészlet.</span><span class="sxs-lookup"><span data-stu-id="a0ded-121">A BackendAddressPool is used toodefine a backend pool with WebSocket enabled servers.</span></span> <span data-ttu-id="a0ded-122">hello backendHttpSetting a 80-as és 443-as, a háttérportot van meghatározva.</span><span class="sxs-lookup"><span data-stu-id="a0ded-122">hello backendHttpSetting is defined with a backend port 80 and 443.</span></span> <span data-ttu-id="a0ded-123">az affinitási cookie-alapú és requestTimeouts hello tulajdonságai nincsenek megfelelő tooWebSocket forgalmat.</span><span class="sxs-lookup"><span data-stu-id="a0ded-123">hello properties for cookie-based affinity and requestTimeouts are not relevant tooWebSocket traffic.</span></span> <span data-ttu-id="a0ded-124">Nincs változás hello útválasztási szabály szükséges, a "Basic" használt tootie hello megfelelő figyelő toohello megfelelő háttér címkészletet.</span><span class="sxs-lookup"><span data-stu-id="a0ded-124">There is no change required in hello routing rule, 'Basic' is used tootie hello appropriate listener toohello corresponding backend address pool.</span></span> 

```json
"requestRoutingRules": [{
    "name": "<ruleName1>",
    "properties": {
        "RuleType": "Basic",
        "httpListener": {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/httpListeners/appGatewayHttpsListener')]"
        },
        "backendAddressPool": {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendAddressPools/ContosoServerPool')]"
        },
        "backendHttpSettings": {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
        }
    }

}, {
    "name": "<ruleName2>",
    "properties": {
        "RuleType": "Basic",
        "httpListener": {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/httpListeners/appGatewayHttpListener')]"
        },
        "backendAddressPool": {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendAddressPools/ContosoServerPool')]"
        },
        "backendHttpSettings": {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
        }

    }
}]
```

## <a name="websocket-enabled-backend"></a><span data-ttu-id="a0ded-125">A WebSocket engedélyezett háttér</span><span class="sxs-lookup"><span data-stu-id="a0ded-125">WebSocket enabled backend</span></span>

<span data-ttu-id="a0ded-126">A háttérrendszer rendelkeznie kell olyan hello konfigurálni a HTTP/HTTPS webkiszolgálóra (általában 80/443-as port) a WebSocket toowork.</span><span class="sxs-lookup"><span data-stu-id="a0ded-126">Your backend must have a HTTP/HTTPS web server running on hello configured port (usually 80/443) for WebSocket toowork.</span></span> <span data-ttu-id="a0ded-127">Ez a követelmény azért van, mert WebSocket protokoll megköveteli hello kezdeti kézfogás toobe HTTP fejléc mezőként frissítési tooWebSocket protokollal.</span><span class="sxs-lookup"><span data-stu-id="a0ded-127">This requirement is because WebSocket protocol requires hello initial handshake toobe HTTP with upgrade tooWebSocket protocol as a header field.</span></span> <span data-ttu-id="a0ded-128">hello az alábbiakban látható egy példa a fejléc:</span><span class="sxs-lookup"><span data-stu-id="a0ded-128">hello following is an example of a header:</span></span>

```
    GET /chat HTTP/1.1
    Host: server.example.com
    Upgrade: websocket
    Connection: Upgrade
    Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
    Origin: http://example.com
    Sec-WebSocket-Protocol: chat, superchat
    Sec-WebSocket-Version: 13
```

<span data-ttu-id="a0ded-129">Egy másik ennek oka, hogy alkalmazás átjáró háttér állapotmintáihoz csak a HTTP és HTTPS protokollok támogatja.</span><span class="sxs-lookup"><span data-stu-id="a0ded-129">Another reason for this is that application gateway backend health probe supports HTTP and HTTPS protocols only.</span></span> <span data-ttu-id="a0ded-130">Ha hello háttérkiszolgáló tooHTTP vagy HTTPS mintavételt nem válaszol, az háttérkészlet kívüli lesz végrehajtva.</span><span class="sxs-lookup"><span data-stu-id="a0ded-130">If hello backend server does not respond tooHTTP or HTTPS probes, it is taken out of backend pool.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a0ded-131">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a0ded-131">Next steps</span></span>

<span data-ttu-id="a0ded-132">Támogatás a WebSocket megismerését követően nyissa meg túl[Alkalmazásátjáró létrehozása](application-gateway-create-gateway.md) WebSocket használatába tooget engedélyezve van a webes alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="a0ded-132">After learning about WebSocket support, go too[create an application gateway](application-gateway-create-gateway.md) tooget started with a WebSocket enabled web application.</span></span>

