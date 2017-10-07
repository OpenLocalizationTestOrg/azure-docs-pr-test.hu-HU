---
title: "aaaHosting Azure Application Gateway több helyén |} Microsoft Docs"
description: "Ezen a lapon hello Alkalmazásátjáró többhelyes támogatás áttekintést nyújt."
documentationcenter: na
services: application-gateway
author: amsriva
manager: rossort
editor: 
ms.assetid: 49993fd2-87e5-4a66-b386-8d22056a616d
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/09/2017
ms.author: amsriva
ms.openlocfilehash: 4ab6faa97f1891d7525affdaa36463681bf99e9f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="application-gateway-multiple-site-hosting"></a>Application Gateway – több hely üzemeltetése

Több helyet üzemeltető lehetővé teszi egy webalkalmazás hello nagyobb tooconfigure ugyanazon alkalmazás átjárópéldányt. Ez a funkció lehetővé teszi egy hatékonyabb topológiát tárhelyigényét tooconfigure too20 webhelyek tooone Alkalmazásátjáró össze. Minden webhelyre irányítható tooits saját háttérkészlet. A következő példa hello Alkalmazásátjáró van kiszolgáló a contoso.com és fabrikam.com ContosoServerPool és FabrikamServerPool nevezett két háttér-kiszolgálófiók rendelkezik a forgalmat.

![imageURLroute](./media/application-gateway-multi-site-overview/multisite.png)

> [!IMPORTANT]
> Szabályok feldolgozása hello sorrendben hello portálon. Erősen ajánlott tooconfigure többhelyes figyelői első előzetes tooconfiguring egy alapszintű figyelő is.  Ezzel biztosíthatja, hogy a forgalom lekérdezi irányított toohello jobb vissza befejezéséhez. Ha előbb egy alapszintű figyelő szerepel a listában, és az megfelel egy bejövő kérésnek, a figyelő feldolgozza azt.

Http://contoso.com kérelmek irányított tooContosoServerPool, és http://fabrikam.com irányított tooFabrikamServerPool.

Hasonlóképpen a szülő ugyanabban a tartományban az alábbiakon tárolható hello két altartományok hello ugyanazon alkalmazás átjáró telepítéséhez. Az altartományok használatának példái között lehet az egyetlen alkalmazásátjáró-telepítésen üzemeltetett http://blog.contoso.com és http://app.contoso.com.

## <a name="host-headers-and-server-name-indication-sni"></a>Állomásfejléc és kiszolgálónév jelzése (SNI)

Három közös mechanizmussal üzemeltetésének hello azonos több hely engedélyezése az infrastruktúra.

1. Több webalkalmazás üzemeltetése mindegyikhez rendelt egyedi IP-címmel.
2. Használjon gazdagép nevét toohost több webalkalmazás a hello azonos IP-címet.
3. Használjon más portok toohost több webalkalmazás a hello azonos IP-címet.

Jelenleg az alkalmazásátjáró egyetlen nyilvános IP-címet kap, amelyen figyeli a forgalmat. Éppen ezért több, saját IP-címmel rendelkező alkalmazás üzemeltetése jelenleg nem támogatott. Alkalmazásátjáró támogatja, több alkalmazást futtató minden más porton figyel, de ez a forgatókönyv igényelnének hello alkalmazások tooaccept forgalom nem szabványos porton, és nincs gyakran egy szükséges konfiguráció. Alkalmazásátjáró támaszkodik a HTTP 1.1 egy webhely több állomás fejlécek toohost hello azonos nyilvános IP-cím és port. Alkalmazásátjáró hello webhelyeket is SSL kiszervezési támogatás a kiszolgálónév jelzése (SNI) TLS-bővítménnyel. Ebben a forgatókönyvben azt jelenti, hogy hello ügyfél böngésző és a háttérkiszolgáló webfarm támogatnia kell a HTTP/1.1 és TLS kiterjesztése az RFC 6066.

## <a name="listener-configuration-element"></a>Figyelő konfigurációs elem

Konfigurációs elem meglévő HTTPListener fokozott toosupport állomás nevét és a kiszolgáló neve arra utal, hogy elemek, amellyel alkalmazás átjáró tooroute forgalom tooappropriate háttérkészlet által. hello alábbi Kódpélda az hello részlet HttpListeners elem a sablonfájl.

```json
"httpListeners": [
    {
        "name": "appGatewayHttpsListener1",
        "properties": {
            "FrontendIPConfiguration": {
                "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendIPConfigurations/DefaultFrontendPublicIP"
            },
            "FrontendPort": {
                "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendPorts/appGatewayFrontendPort443'"
            },
            "Protocol": "Https",
            "SslCertificate": {
                "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/sslCertificates/appGatewaySslCert1'"
            },
            "HostName": "contoso.com",
            "RequireServerNameIndication": "true"
        }
    },
    {
        "name": "appGatewayHttpListener2",
        "properties": {
            "FrontendIPConfiguration": {
                "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendIPConfigurations/appGatewayFrontendIP'"
            },
            "FrontendPort": {
                "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendPorts/appGatewayFrontendPort80'"
            },
            "Protocol": "Http",
            "HostName": "fabrikam.com",
            "RequireServerNameIndication": "false"
        }
    }
],
```

Ellátogathat [Resource Manager-sablon használatával több hely](https://github.com/Azure/azure-quickstart-templates/blob/master/201-application-gateway-multihosting) end tooend sablon-alapú központi telepítésre.

## <a name="routing-rule"></a>Útválasztási szabály

Nincs szükség hello útválasztási szabály változás. hello "Basic" továbbra is a kiválasztott toobe tootie hello megfelelő hely figyelő toohello megfelelő háttér címkészletet útválasztási szabály.

```json
"requestRoutingRules": [
{
    "name": "<ruleName1>",
    "properties": {
        "RuleType": "Basic",
        "httpListener": {
            "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/httpListeners/appGatewayHttpsListener1')]"
        },
        "backendAddressPool": {
            "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/ContosoServerPool')]"
        },
        "backendHttpSettings": {
            "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
        }
    }

},
{
    "name": "<ruleName2>",
    "properties": {
        "RuleType": "Basic",
        "httpListener": {
            "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/httpListeners/appGatewayHttpListener2')]"
        },
        "backendAddressPool": {
            "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/FabrikamServerPool')]"
        },
        "backendHttpSettings": {
            "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
        }
    }

}
]
```

## <a name="next-steps"></a>Következő lépések

Több helyet üzemeltető megismerését követően nyissa meg túl[használatával több hely Alkalmazásátjáró létrehozása](application-gateway-create-multisite-azureresourcemanager-powershell.md) toocreate egy webalkalmazás-nál több lehetősége toosupport Alkalmazásátjáró.

