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
# <a name="application-gateway-multiple-site-hosting"></a><span data-ttu-id="d8563-103">Application Gateway – több hely üzemeltetése</span><span class="sxs-lookup"><span data-stu-id="d8563-103">Application Gateway multiple site hosting</span></span>

<span data-ttu-id="d8563-104">Több helyet üzemeltető lehetővé teszi egy webalkalmazás hello nagyobb tooconfigure ugyanazon alkalmazás átjárópéldányt.</span><span class="sxs-lookup"><span data-stu-id="d8563-104">Multiple site hosting enables you tooconfigure more than one web application on hello same application gateway instance.</span></span> <span data-ttu-id="d8563-105">Ez a funkció lehetővé teszi egy hatékonyabb topológiát tárhelyigényét tooconfigure too20 webhelyek tooone Alkalmazásátjáró össze.</span><span class="sxs-lookup"><span data-stu-id="d8563-105">This feature allows you tooconfigure a more efficient topology for your deployments by adding up too20 websites tooone application gateway.</span></span> <span data-ttu-id="d8563-106">Minden webhelyre irányítható tooits saját háttérkészlet.</span><span class="sxs-lookup"><span data-stu-id="d8563-106">Each website can be directed tooits own backend pool.</span></span> <span data-ttu-id="d8563-107">A következő példa hello Alkalmazásátjáró van kiszolgáló a contoso.com és fabrikam.com ContosoServerPool és FabrikamServerPool nevezett két háttér-kiszolgálófiók rendelkezik a forgalmat.</span><span class="sxs-lookup"><span data-stu-id="d8563-107">In hello following example, application gateway is serving traffic for contoso.com and fabrikam.com from two back-end server pools called ContosoServerPool and FabrikamServerPool.</span></span>

![imageURLroute](./media/application-gateway-multi-site-overview/multisite.png)

> [!IMPORTANT]
> <span data-ttu-id="d8563-109">Szabályok feldolgozása hello sorrendben hello portálon.</span><span class="sxs-lookup"><span data-stu-id="d8563-109">Rules are processed in hello order they are listed in hello portal.</span></span> <span data-ttu-id="d8563-110">Erősen ajánlott tooconfigure többhelyes figyelői első előzetes tooconfiguring egy alapszintű figyelő is.</span><span class="sxs-lookup"><span data-stu-id="d8563-110">It is highly recommended tooconfigure multi-site listeners first prior tooconfiguring a basic listener.</span></span>  <span data-ttu-id="d8563-111">Ezzel biztosíthatja, hogy a forgalom lekérdezi irányított toohello jobb vissza befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="d8563-111">This will ensure that traffic gets routed toohello right back end.</span></span> <span data-ttu-id="d8563-112">Ha előbb egy alapszintű figyelő szerepel a listában, és az megfelel egy bejövő kérésnek, a figyelő feldolgozza azt.</span><span class="sxs-lookup"><span data-stu-id="d8563-112">If a basic listener is listed first and matches an incoming request, it gets processed by that listener.</span></span>

<span data-ttu-id="d8563-113">Http://contoso.com kérelmek irányított tooContosoServerPool, és http://fabrikam.com irányított tooFabrikamServerPool.</span><span class="sxs-lookup"><span data-stu-id="d8563-113">Requests for http://contoso.com are routed tooContosoServerPool, and http://fabrikam.com are routed tooFabrikamServerPool.</span></span>

<span data-ttu-id="d8563-114">Hasonlóképpen a szülő ugyanabban a tartományban az alábbiakon tárolható hello két altartományok hello ugyanazon alkalmazás átjáró telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="d8563-114">Similarly two subdomains of hello same parent domain can be hosted on hello same application gateway deployment.</span></span> <span data-ttu-id="d8563-115">Az altartományok használatának példái között lehet az egyetlen alkalmazásátjáró-telepítésen üzemeltetett http://blog.contoso.com és http://app.contoso.com.</span><span class="sxs-lookup"><span data-stu-id="d8563-115">Examples of using subdomains could include http://blog.contoso.com and http://app.contoso.com hosted on a single application gateway deployment.</span></span>

## <a name="host-headers-and-server-name-indication-sni"></a><span data-ttu-id="d8563-116">Állomásfejléc és kiszolgálónév jelzése (SNI)</span><span class="sxs-lookup"><span data-stu-id="d8563-116">Host headers and Server Name Indication (SNI)</span></span>

<span data-ttu-id="d8563-117">Három közös mechanizmussal üzemeltetésének hello azonos több hely engedélyezése az infrastruktúra.</span><span class="sxs-lookup"><span data-stu-id="d8563-117">There are three common mechanisms for enabling multiple site hosting on hello same infrastructure.</span></span>

1. <span data-ttu-id="d8563-118">Több webalkalmazás üzemeltetése mindegyikhez rendelt egyedi IP-címmel.</span><span class="sxs-lookup"><span data-stu-id="d8563-118">Host multiple web applications each on a unique IP address.</span></span>
2. <span data-ttu-id="d8563-119">Használjon gazdagép nevét toohost több webalkalmazás a hello azonos IP-címet.</span><span class="sxs-lookup"><span data-stu-id="d8563-119">Use host name toohost multiple web applications on hello same IP address.</span></span>
3. <span data-ttu-id="d8563-120">Használjon más portok toohost több webalkalmazás a hello azonos IP-címet.</span><span class="sxs-lookup"><span data-stu-id="d8563-120">Use different ports toohost multiple web applications on hello same IP address.</span></span>

<span data-ttu-id="d8563-121">Jelenleg az alkalmazásátjáró egyetlen nyilvános IP-címet kap, amelyen figyeli a forgalmat.</span><span class="sxs-lookup"><span data-stu-id="d8563-121">Currently an application gateway gets a single public IP address on which it listens for traffic.</span></span> <span data-ttu-id="d8563-122">Éppen ezért több, saját IP-címmel rendelkező alkalmazás üzemeltetése jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="d8563-122">Therefore supporting multiple applications, each with its own IP address, is currently not supported.</span></span> <span data-ttu-id="d8563-123">Alkalmazásátjáró támogatja, több alkalmazást futtató minden más porton figyel, de ez a forgatókönyv igényelnének hello alkalmazások tooaccept forgalom nem szabványos porton, és nincs gyakran egy szükséges konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="d8563-123">Application Gateway supports hosting multiple applications each listening on different ports but this scenario would require hello applications tooaccept traffic on non-standard ports and is often not a desired configuration.</span></span> <span data-ttu-id="d8563-124">Alkalmazásátjáró támaszkodik a HTTP 1.1 egy webhely több állomás fejlécek toohost hello azonos nyilvános IP-cím és port.</span><span class="sxs-lookup"><span data-stu-id="d8563-124">Application Gateway relies on HTTP 1.1 host headers toohost more than one website on hello same public IP address and port.</span></span> <span data-ttu-id="d8563-125">Alkalmazásátjáró hello webhelyeket is SSL kiszervezési támogatás a kiszolgálónév jelzése (SNI) TLS-bővítménnyel.</span><span class="sxs-lookup"><span data-stu-id="d8563-125">hello sites hosted on application gateway can also support SSL offload with Server Name Indication (SNI) TLS extension.</span></span> <span data-ttu-id="d8563-126">Ebben a forgatókönyvben azt jelenti, hogy hello ügyfél böngésző és a háttérkiszolgáló webfarm támogatnia kell a HTTP/1.1 és TLS kiterjesztése az RFC 6066.</span><span class="sxs-lookup"><span data-stu-id="d8563-126">This scenario means that hello client browser and backend web farm must support HTTP/1.1 and TLS extension as defined in RFC 6066.</span></span>

## <a name="listener-configuration-element"></a><span data-ttu-id="d8563-127">Figyelő konfigurációs elem</span><span class="sxs-lookup"><span data-stu-id="d8563-127">Listener configuration element</span></span>

<span data-ttu-id="d8563-128">Konfigurációs elem meglévő HTTPListener fokozott toosupport állomás nevét és a kiszolgáló neve arra utal, hogy elemek, amellyel alkalmazás átjáró tooroute forgalom tooappropriate háttérkészlet által.</span><span class="sxs-lookup"><span data-stu-id="d8563-128">Existing HTTPListener configuration element is enhanced toosupport host name and server name indication elements, which is used by application gateway tooroute traffic tooappropriate backend pool.</span></span> <span data-ttu-id="d8563-129">hello alábbi Kódpélda az hello részlet HttpListeners elem a sablonfájl.</span><span class="sxs-lookup"><span data-stu-id="d8563-129">hello following code example is hello snippet of HttpListeners element from template file.</span></span>

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

<span data-ttu-id="d8563-130">Ellátogathat [Resource Manager-sablon használatával több hely](https://github.com/Azure/azure-quickstart-templates/blob/master/201-application-gateway-multihosting) end tooend sablon-alapú központi telepítésre.</span><span class="sxs-lookup"><span data-stu-id="d8563-130">You can visit [Resource Manager template using multiple site hosting](https://github.com/Azure/azure-quickstart-templates/blob/master/201-application-gateway-multihosting) for an end tooend template-based deployment.</span></span>

## <a name="routing-rule"></a><span data-ttu-id="d8563-131">Útválasztási szabály</span><span class="sxs-lookup"><span data-stu-id="d8563-131">Routing rule</span></span>

<span data-ttu-id="d8563-132">Nincs szükség hello útválasztási szabály változás.</span><span class="sxs-lookup"><span data-stu-id="d8563-132">There is no change required in hello routing rule.</span></span> <span data-ttu-id="d8563-133">hello "Basic" továbbra is a kiválasztott toobe tootie hello megfelelő hely figyelő toohello megfelelő háttér címkészletet útválasztási szabály.</span><span class="sxs-lookup"><span data-stu-id="d8563-133">hello routing rule 'Basic' should continue toobe chosen tootie hello appropriate site listener toohello corresponding backend address pool.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="d8563-134">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d8563-134">Next steps</span></span>

<span data-ttu-id="d8563-135">Több helyet üzemeltető megismerését követően nyissa meg túl[használatával több hely Alkalmazásátjáró létrehozása](application-gateway-create-multisite-azureresourcemanager-powershell.md) toocreate egy webalkalmazás-nál több lehetősége toosupport Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="d8563-135">After learning about multiple site hosting, go too[create an application gateway using multiple site hosting](application-gateway-create-multisite-azureresourcemanager-powershell.md) toocreate an application gateway with ability toosupport more than one web application.</span></span>

