---
title: "Azure Application Gateway WebSocket támogatást |} Microsoft Docs"
description: "Ezen a lapon a kérelem átjáró WebSocket támogatási áttekintést nyújt."
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
ms.openlocfilehash: 75b06ddd02da231b7813c609c848c75e42116da5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="overview-of-websocket-support-in-application-gateway"></a><span data-ttu-id="b0ef4-103">Alkalmazásátjáró WebSocket támogatást áttekintése</span><span class="sxs-lookup"><span data-stu-id="b0ef4-103">Overview of WebSocket support in Application Gateway</span></span>

<span data-ttu-id="b0ef4-104">Alkalmazásátjáró közötti átjáró különböző méretű WebSocket natív támogatást biztosít.</span><span class="sxs-lookup"><span data-stu-id="b0ef4-104">Application Gateway provides native support for WebSocket across all gateway sizes.</span></span> <span data-ttu-id="b0ef4-105">Nem található felhasználó által konfigurálható szelektív letiltása és engedélyezése WebSocket támogatása.</span><span class="sxs-lookup"><span data-stu-id="b0ef4-105">There is no user-configurable setting to selectively enable or disable WebSocket support.</span></span> 

<span data-ttu-id="b0ef4-106">A szabványos WebSocket protokoll [RFC6455](https://tools.ietf.org/html/rfc6455) lehetővé teszi, hogy a kiszolgáló és az ügyfél között a teljes kétirányú kommunikációs egy hosszú ideig tartó TCP-kapcsolaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="b0ef4-106">WebSocket protocol standardized in [RFC6455](https://tools.ietf.org/html/rfc6455) enables a full duplex communication between a server and a client over a long running TCP connection.</span></span> <span data-ttu-id="b0ef4-107">Ez a funkció lehetővé teszi, hogy a webkiszolgáló és az ügyfél, amely kétirányú, mint a HTTP-alapú megvalósításokhoz lekérdezési szükségessége nélkül lehet több interaktív kommunikációját.</span><span class="sxs-lookup"><span data-stu-id="b0ef4-107">This feature allows for a more interactive communication between the web server and the client, which can be bidirectional without the need for polling as required in HTTP-based implementations.</span></span> <span data-ttu-id="b0ef4-108">WebSocket alacsony terhelés eltérően a HTTP és a több kérelem/válasz az erőforrások hatékonyabb kihasználását eredményezi azonos TCP-kapcsolatot is felhasználhatja.</span><span class="sxs-lookup"><span data-stu-id="b0ef4-108">WebSocket has low overhead unlike HTTP and can reuse the same TCP connection for multiple request/responses resulting in a more efficient utilization of resources.</span></span> <span data-ttu-id="b0ef4-109">WebSocket protokoll tervezték hagyományos HTTP-port a 80-as és 443-as porton keresztül.</span><span class="sxs-lookup"><span data-stu-id="b0ef4-109">WebSocket protocols are designed to work over traditional HTTP ports of 80 and 443.</span></span>

<span data-ttu-id="b0ef4-110">Tovább a szabványos HTTP-figyelő használ a 80-as vagy 443-as porton WebSocket forgalom fogadására.</span><span class="sxs-lookup"><span data-stu-id="b0ef4-110">You can continue using a standard HTTP listener on port 80 or 443 to receive WebSocket traffic.</span></span> <span data-ttu-id="b0ef4-111">WebSocket forgalmat a rendszer ekkor átirányítja a WebSocket engedélyezett háttérkiszolgáló megfelelő háttérkészlet megadott alkalmazás átjáró szabályok használatával.</span><span class="sxs-lookup"><span data-stu-id="b0ef4-111">WebSocket traffic is then directed to the WebSocket enabled backend server using the appropriate backend pool as specified in application gateway rules.</span></span> <span data-ttu-id="b0ef4-112">A háttérkiszolgáló az alkalmazás átjáró mintavételek menüpontban, amely ismerteti a válaszolnia kell a [állapot-mintavételi áttekintése](application-gateway-probe-overview.md) szakasz.</span><span class="sxs-lookup"><span data-stu-id="b0ef4-112">The backend server must respond to the application gateway probes, which are described in the [health probe overview](application-gateway-probe-overview.md) section.</span></span> <span data-ttu-id="b0ef4-113">Alkalmazás átjáró állapotfigyelő mintavételt csak olyan HTTP/HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b0ef4-113">Application gateway health probes are HTTP/HTTPS only.</span></span> <span data-ttu-id="b0ef4-114">Minden egyes háttérkiszolgáló válaszolnia kell a HTTP-mintavételt az Alkalmazásátjáró WebSocket forgalom irányítására a kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="b0ef4-114">Each backend server must respond to HTTP probes for application gateway to route WebSocket traffic to the server.</span></span>

## <a name="listener-configuration-element"></a><span data-ttu-id="b0ef4-115">Figyelő konfigurációs elem</span><span class="sxs-lookup"><span data-stu-id="b0ef4-115">Listener configuration element</span></span>

<span data-ttu-id="b0ef4-116">Egy meglévő HTTP-figyelő segítségével WebSocket forgalom támogatásához.</span><span class="sxs-lookup"><span data-stu-id="b0ef4-116">An existing HTTP listener can be used to support WebSocket traffic.</span></span> <span data-ttu-id="b0ef4-117">A következő egy minta sablon fájlból httpListeners elem egy részlet.</span><span class="sxs-lookup"><span data-stu-id="b0ef4-117">The following is a snippet of an httpListeners element from a sample template file.</span></span> <span data-ttu-id="b0ef4-118">Támogatja a WebSocket és WebSocket forgalmának biztonságossá tétele a HTTP és HTTPS figyelői kellene.</span><span class="sxs-lookup"><span data-stu-id="b0ef4-118">You would need both HTTP and HTTPS listeners to support WebSocket and secure WebSocket traffic.</span></span> <span data-ttu-id="b0ef4-119">Hasonló módon használhatja a [portal](application-gateway-create-gateway-portal.md) vagy [PowerShell](application-gateway-create-gateway-arm.md) Alkalmazásátjáró létrehozása a figyelők a porton keresztül 80/443-as WebSocket forgalom támogatásához.</span><span class="sxs-lookup"><span data-stu-id="b0ef4-119">Similarly you can use the [portal](application-gateway-create-gateway-portal.md) or [PowerShell](application-gateway-create-gateway-arm.md) to create an application gateway with listeners on port 80/443 to support WebSocket traffic.</span></span>

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

## <a name="backendaddresspool-backendhttpsetting-and-routing-rule-configuration"></a><span data-ttu-id="b0ef4-120">BackendAddressPool BackendHttpSetting és útválasztás szabály konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b0ef4-120">BackendAddressPool, BackendHttpSetting, and Routing rule configuration</span></span>

<span data-ttu-id="b0ef4-121">Egy BackendAddressPool engedélyezett WebSocket-kiszolgálókkal háttérkészlet azonosítására szolgál.</span><span class="sxs-lookup"><span data-stu-id="b0ef4-121">A BackendAddressPool is used to define a backend pool with WebSocket enabled servers.</span></span> <span data-ttu-id="b0ef4-122">A backendHttpSetting a 80-as és 443-as, a háttérportot van meghatározva.</span><span class="sxs-lookup"><span data-stu-id="b0ef4-122">The backendHttpSetting is defined with a backend port 80 and 443.</span></span> <span data-ttu-id="b0ef4-123">Az affinitási cookie-alapú és requestTimeouts tulajdonságait, amelyek nem kapcsolódnak a WebSocket-forgalmat.</span><span class="sxs-lookup"><span data-stu-id="b0ef4-123">The properties for cookie-based affinity and requestTimeouts are not relevant to WebSocket traffic.</span></span> <span data-ttu-id="b0ef4-124">Nincs változás a útválasztási szabály szükséges, a "Basic" szolgál a megfelelő figyelőt, hogy a megfelelő háttér címkészletet kötése.</span><span class="sxs-lookup"><span data-stu-id="b0ef4-124">There is no change required in the routing rule, 'Basic' is used to tie the appropriate listener to the corresponding backend address pool.</span></span> 

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

## <a name="websocket-enabled-backend"></a><span data-ttu-id="b0ef4-125">A WebSocket engedélyezett háttér</span><span class="sxs-lookup"><span data-stu-id="b0ef4-125">WebSocket enabled backend</span></span>

<span data-ttu-id="b0ef4-126">A háttér kell rendelkeznie a konfigurált futó HTTP/HTTPS webkiszolgálón (általában 80/443-as port) WebSocket működéséhez a.</span><span class="sxs-lookup"><span data-stu-id="b0ef4-126">Your backend must have a HTTP/HTTPS web server running on the configured port (usually 80/443) for WebSocket to work.</span></span> <span data-ttu-id="b0ef4-127">Ez a követelmény azért van, mert WebSocket protokoll megköveteli a kezdeti kézfogás HTTP kell egy fejlécmező WebSocket protokoll a frissítést.</span><span class="sxs-lookup"><span data-stu-id="b0ef4-127">This requirement is because WebSocket protocol requires the initial handshake to be HTTP with upgrade to WebSocket protocol as a header field.</span></span> <span data-ttu-id="b0ef4-128">A következő egy példa a fejléc:</span><span class="sxs-lookup"><span data-stu-id="b0ef4-128">The following is an example of a header:</span></span>

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

<span data-ttu-id="b0ef4-129">Egy másik ennek oka, hogy alkalmazás átjáró háttér állapotmintáihoz csak a HTTP és HTTPS protokollok támogatja.</span><span class="sxs-lookup"><span data-stu-id="b0ef4-129">Another reason for this is that application gateway backend health probe supports HTTP and HTTPS protocols only.</span></span> <span data-ttu-id="b0ef4-130">Ha a háttérkiszolgáló nem válaszol a HTTP vagy HTTPS mintavételek menüpontban, az háttérkészlet kívüli lesz végrehajtva.</span><span class="sxs-lookup"><span data-stu-id="b0ef4-130">If the backend server does not respond to HTTP or HTTPS probes, it is taken out of backend pool.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b0ef4-131">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b0ef4-131">Next steps</span></span>

<span data-ttu-id="b0ef4-132">Támogatás a WebSocket megismerését követően navigáljon [Alkalmazásátjáró létrehozása](application-gateway-create-gateway.md) WebSocket használatába engedélyezve van a webes alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="b0ef4-132">After learning about WebSocket support, go to [create an application gateway](application-gateway-create-gateway.md) to get started with a WebSocket enabled web application.</span></span>

