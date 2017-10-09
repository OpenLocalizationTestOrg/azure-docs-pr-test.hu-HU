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
# <a name="overview-of-websocket-support-in-application-gateway"></a>Alkalmazásátjáró WebSocket támogatást áttekintése

Alkalmazásátjáró közötti átjáró különböző méretű WebSocket natív támogatást biztosít. Nincs felhasználó által konfigurálható beállítás tooselectively engedélyezése vagy letiltása WebSocket támogatási. 

A szabványos WebSocket protokoll [RFC6455](https://tools.ietf.org/html/rfc6455) lehetővé teszi, hogy a kiszolgáló és az ügyfél között a teljes kétirányú kommunikációs egy hosszú ideig tartó TCP-kapcsolaton keresztül. Ezzel a funkcióval a több interaktív kommunikációhoz hello és hello ügyfél, amely kétirányú anélkül hello a lekérdezéshez, mint a HTTP-alapú megvalósításokhoz lehet. WebSocket alacsony terhelés eltérően a HTTP és a is újbóli hello azonos több kérelem/válasz az erőforrások hatékonyabb kihasználását eredményezi TCP-kapcsolatot. WebSocket protokoll tervezett toowork hagyományos HTTP-port a 80-as és 443-as porton keresztül történik.

A WebSocket-forgalom 80-as vagy 443-as port tooreceive egy szabványos HTTP-figyelő használatának folytatása. WebSocket forgalom nem irányított toohello WebSocket engedélyezett hello megfelelő háttérkészlet használó alkalmazás átjáró szabályok a háttérkiszolgálóra. hello háttérkiszolgáló kell válaszolnia toohello alkalmazás átjáró mintavételek menüpontban, amely ismerteti a hello [állapot-mintavételi áttekintése](application-gateway-probe-overview.md) szakasz. Alkalmazás átjáró állapotfigyelő mintavételt csak olyan HTTP/HTTPS. Minden egyes háttérkiszolgáló alkalmazás tooroute WebSocket forgalom toohello átjárókiszolgáló tooHTTP mintavételt kell válaszolnia.

## <a name="listener-configuration-element"></a>Figyelő konfigurációs elem

Egy meglévő HTTP-figyelő lehet használt toosupport WebSocket-forgalmat. hello az alábbiakban látható egy minta sablon fájlból httpListeners elem részlet. Ehhez a HTTP és HTTPS figyelői toosupport WebSocket kell, és biztosítják a WebSocket-forgalmát. Hasonló módon használhatja a hello [portal](application-gateway-create-gateway-portal.md) vagy [PowerShell](application-gateway-create-gateway-arm.md) toocreate port 80/443-as toosupport WebSocket forgalom figyelőinek az Alkalmazásátjáró.

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

## <a name="backendaddresspool-backendhttpsetting-and-routing-rule-configuration"></a>BackendAddressPool BackendHttpSetting és útválasztás szabály konfigurálása

Egy BackendAddressPool használt toodefine engedélyezett WebSocket-kiszolgálókkal háttérkészlet. hello backendHttpSetting a 80-as és 443-as, a háttérportot van meghatározva. az affinitási cookie-alapú és requestTimeouts hello tulajdonságai nincsenek megfelelő tooWebSocket forgalmat. Nincs változás hello útválasztási szabály szükséges, a "Basic" használt tootie hello megfelelő figyelő toohello megfelelő háttér címkészletet. 

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

## <a name="websocket-enabled-backend"></a>A WebSocket engedélyezett háttér

A háttérrendszer rendelkeznie kell olyan hello konfigurálni a HTTP/HTTPS webkiszolgálóra (általában 80/443-as port) a WebSocket toowork. Ez a követelmény azért van, mert WebSocket protokoll megköveteli hello kezdeti kézfogás toobe HTTP fejléc mezőként frissítési tooWebSocket protokollal. hello az alábbiakban látható egy példa a fejléc:

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

Egy másik ennek oka, hogy alkalmazás átjáró háttér állapotmintáihoz csak a HTTP és HTTPS protokollok támogatja. Ha hello háttérkiszolgáló tooHTTP vagy HTTPS mintavételt nem válaszol, az háttérkészlet kívüli lesz végrehajtva.

## <a name="next-steps"></a>Következő lépések

Támogatás a WebSocket megismerését követően nyissa meg túl[Alkalmazásátjáró létrehozása](application-gateway-create-gateway.md) WebSocket használatába tooget engedélyezve van a webes alkalmazást.

