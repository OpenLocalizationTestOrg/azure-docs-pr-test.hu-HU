---
title: "aaaConfigure webalkalmazási tűzfal - Azure Application Gateway |} Microsoft Docs"
description: "Ez a cikk hogyan toostart használatával webalkalmazási tűzfal a meglévő vagy új Alkalmazásátjáró nyújt útmutatást."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 670b9732-874b-43e6-843b-d2585c160982
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/20/2017
ms.author: gwallace
ms.openlocfilehash: d5354984760ceab12ed49efa9e18836e9f1d3c96
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-web-application-firewall-on-a-new-or-existing-application-gateway-with-azure-cli"></a><span data-ttu-id="aed90-103">Webalkalmazási tűzfal konfigurálása egy új vagy meglévő Alkalmazásátjáró Azure parancssori felülettel</span><span class="sxs-lookup"><span data-stu-id="aed90-103">Configure web application firewall on a new or existing Application Gateway with Azure CLI</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="aed90-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="aed90-104">Azure portal</span></span>](application-gateway-web-application-firewall-portal.md)
> * [<span data-ttu-id="aed90-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="aed90-105">PowerShell</span></span>](application-gateway-web-application-firewall-powershell.md)
> * [<span data-ttu-id="aed90-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="aed90-106">Azure CLI</span></span>](application-gateway-web-application-firewall-cli.md)

<span data-ttu-id="aed90-107">Ismerje meg, hogyan toocreate webalkalmazási tűzfal engedélyezve van az Alkalmazásátjáró, vagy adja hozzá a webes alkalmazás tűzfal tooan meglévő Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="aed90-107">Learn how toocreate a web application firewall enabled application gateway or add web application firewall tooan existing application gateway.</span></span>

<span data-ttu-id="aed90-108">hello webalkalmazási tűzfal (waf-ot) az Azure alkalmazás átjáró webalkalmazások védje a közös web-alapú támadások, például az SQL-injektálás, a többhelyes parancsfájlok futtatására és a munkamenet kihasználásának.</span><span class="sxs-lookup"><span data-stu-id="aed90-108">hello web application firewall (WAF) in Azure Application Gateway protects web applications from common web-based attacks like SQL injection, cross-site scripting attacks, and session hijacks.</span></span>

<span data-ttu-id="aed90-109">Az Azure Application Gateway egy 7. rétegbeli terheléselosztó.</span><span class="sxs-lookup"><span data-stu-id="aed90-109">Azure Application Gateway is a layer-7 load balancer.</span></span> <span data-ttu-id="aed90-110">Akár hello felhőalapú vagy helyszíni biztosít feladatátvételt, HTTP-kérelmek teljesítmény-útválasztási különböző kiszolgálók között.</span><span class="sxs-lookup"><span data-stu-id="aed90-110">It provides failover, performance-routing HTTP requests between different servers, whether they are on hello cloud or on-premises.</span></span> <span data-ttu-id="aed90-111">Alkalmazásátjáró biztosít számos alkalmazás kézbesítési vezérlő (LÉPETT) szolgáltatást HTTP beleértve betöltése terheléselosztási, a cookie-alapú munkamenet-kapcsolatot, és a secure sockets layer (SSL)-kiszervezés el egyéni állapotteljesítmény, többhelyes támogatása, és sok más.</span><span class="sxs-lookup"><span data-stu-id="aed90-111">Application gateway provides many application delivery controller (ADC) features including HTTP load balancing, cookie-based session affinity, secure sockets layer (SSL) offload, custom health probes, support for multi-site, and many others.</span></span> <span data-ttu-id="aed90-112">támogatott szolgáltatások teljes listáját toofind látogassa meg: [Alkalmazásátjáró áttekintése](application-gateway-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="aed90-112">toofind a complete list of supported features, visit: [Overview of Application Gateway](application-gateway-introduction.md).</span></span>

<span data-ttu-id="aed90-113">hello következő a cikk bemutatja, hogyan túl[hozzáadása a webes alkalmazás tűzfal tooan meglévő Alkalmazásátjáró](#add-web-application-firewall-to-an-existing-application-gateway) és [webalkalmazási tűzfal használó Alkalmazásátjáró létrehozása](#create-an-application-gateway-with-web-application-firewall).</span><span class="sxs-lookup"><span data-stu-id="aed90-113">hello following article shows how too[add web application firewall tooan existing application gateway](#add-web-application-firewall-to-an-existing-application-gateway) and [create an application gateway that uses web application firewall](#create-an-application-gateway-with-web-application-firewall).</span></span>

![a forgatókönyv kép][scenario]

## <a name="prerequisite-install-hello-azure-cli-20"></a><span data-ttu-id="aed90-115">Előfeltétel: Hello Azure CLI 2.0 telepítése</span><span class="sxs-lookup"><span data-stu-id="aed90-115">Prerequisite: Install hello Azure CLI 2.0</span></span>

<span data-ttu-id="aed90-116">tooperform hello ebben a cikkben ismertetett visszaállítási lépésekkel, túl kell[hello Azure parancssori felület Mac, Linux és Windows (Azure CLI) telepítése](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="aed90-116">tooperform hello steps in this article, you need too[install hello Azure Command-Line Interface for Mac, Linux, and Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span></span>

## <a name="waf-configuration-differences"></a><span data-ttu-id="aed90-117">WAF konfigurációs különbségek</span><span class="sxs-lookup"><span data-stu-id="aed90-117">WAF configuration differences</span></span>

<span data-ttu-id="aed90-118">Ha mindenképpen olvassa el [hozzon létre egy alkalmazást az Azure parancssori felület](application-gateway-create-gateway-cli.md), tisztában hello SKU beállítások tooconfigure Alkalmazásátjáró létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="aed90-118">If you have read [Create an Application Gateway with Azure CLI](application-gateway-create-gateway-cli.md), you understand hello SKU settings tooconfigure when creating an application gateway.</span></span> <span data-ttu-id="aed90-119">WAF biztosít a további beállítások toodefine Alkalmazásátjáró hello SKU konfigurálásakor.</span><span class="sxs-lookup"><span data-stu-id="aed90-119">WAF provides additional settings toodefine when configuring hello SKU on an application gateway.</span></span> <span data-ttu-id="aed90-120">Nincsenek további módosítások, amely akkor adja meg a hello Alkalmazásátjáró magát.</span><span class="sxs-lookup"><span data-stu-id="aed90-120">There are no additional changes that you make on hello application gateway itself.</span></span>

| <span data-ttu-id="aed90-121">**Beállítás**</span><span class="sxs-lookup"><span data-stu-id="aed90-121">**Setting**</span></span> | <span data-ttu-id="aed90-122">**Részletek**</span><span class="sxs-lookup"><span data-stu-id="aed90-122">**Details**</span></span>
|---|---|
|<span data-ttu-id="aed90-123">**Termékváltozat**</span><span class="sxs-lookup"><span data-stu-id="aed90-123">**SKU**</span></span> |<span data-ttu-id="aed90-124">Egy normál Alkalmazásátjáró nélkül WAF támogatja **szabványos\_kis**, **szabványos\_Közepes**, és **szabványos\_nagy**méretét.</span><span class="sxs-lookup"><span data-stu-id="aed90-124">A normal application gateway without WAF supports **Standard\_Small**, **Standard\_Medium**, and **Standard\_Large** sizes.</span></span> <span data-ttu-id="aed90-125">WAF hello bevezetését, a esetén két további SKU, **WAF\_Közepes** és **WAF\_nagy**.</span><span class="sxs-lookup"><span data-stu-id="aed90-125">With hello introduction of WAF, there are two additional SKUs, **WAF\_Medium** and **WAF\_Large**.</span></span> <span data-ttu-id="aed90-126">WAF kis alkalmazásátjárót nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="aed90-126">WAF is not supported on small application gateways.</span></span>|
|<span data-ttu-id="aed90-127">**Mód**</span><span class="sxs-lookup"><span data-stu-id="aed90-127">**Mode**</span></span> | <span data-ttu-id="aed90-128">Ez a beállítás akkor WAF hello módját.</span><span class="sxs-lookup"><span data-stu-id="aed90-128">This setting is hello mode of WAF.</span></span> <span data-ttu-id="aed90-129">két érték engedélyezett **észlelési** és **megelőzési**.</span><span class="sxs-lookup"><span data-stu-id="aed90-129">allowed values are **Detection** and **Prevention**.</span></span> <span data-ttu-id="aed90-130">Ha WAF észlelési mód van beállítva, minden fenyegetések naplófájl vannak tárolva.</span><span class="sxs-lookup"><span data-stu-id="aed90-130">When WAF is set up in detection mode, all threats are stored in a log file.</span></span> <span data-ttu-id="aed90-131">Megelőzési módban eseményeket naplózza, de hello támadó 403-as jogosulatlan választ kap hello Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="aed90-131">In prevention mode, events are still logged but hello attacker receives a 403 unauthorized response from hello application gateway.</span></span>|

## <a name="add-web-application-firewall-tooan-existing-application-gateway"></a><span data-ttu-id="aed90-132">Webes alkalmazás tűzfal tooan meglévő Alkalmazásátjáró hozzáadása</span><span class="sxs-lookup"><span data-stu-id="aed90-132">Add web application firewall tooan existing application gateway</span></span>

<span data-ttu-id="aed90-133">hello egy meglévő szabványos alkalmazás tooa WAF engedélyezett alkalmazás átjárók kövesse a parancs végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="aed90-133">hello follow command changes an existing standard application gateway tooa WAF enabled application gateway.</span></span>

```azurecli-interactive
#!/bin/bash

az network application-gateway waf-config set \
  --enabled true \
  --firewall-mode Prevention \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG"
```

<span data-ttu-id="aed90-134">Ez a parancs webalkalmazási tűzfal hello Alkalmazásátjáró frissítése.</span><span class="sxs-lookup"><span data-stu-id="aed90-134">This command updates hello application gateway with web application firewall.</span></span> <span data-ttu-id="aed90-135">Látogasson el [átjáró Alkalmazásdiagnosztika](application-gateway-diagnostics.md) toounderstand, hogy miként naplózza az tooview az alkalmazás átjáró.</span><span class="sxs-lookup"><span data-stu-id="aed90-135">Visit [Application Gateway Diagnostics](application-gateway-diagnostics.md) toounderstand how tooview logs for your application gateway.</span></span> <span data-ttu-id="aed90-136">WAF toohello biztonsági jellege miatt naplók kell toobe rendszeresen ellenőrizni a webalkalmazások toounderstand hello biztonsági állapotát.</span><span class="sxs-lookup"><span data-stu-id="aed90-136">Due toohello security nature of WAF, logs need toobe reviewed regularly toounderstand hello security posture of your web applications.</span></span>

## <a name="create-an-application-gateway-with-web-application-firewall"></a><span data-ttu-id="aed90-137">Webalkalmazási tűzfal Alkalmazásátjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="aed90-137">Create an Application Gateway with web application firewall</span></span>

<span data-ttu-id="aed90-138">a következő parancs hello Alkalmazásátjáró webalkalmazási tűzfal hoz létre.</span><span class="sxs-lookup"><span data-stu-id="aed90-138">hello following command creates an Application Gateway with web application firewall.</span></span>

```azurecli-interactive
#!/bin/bash

az network application-gateway create \
  --name "AdatumAppGateway2" \
  --location "eastus" \
  --resource-group "AdatumAppGatewayRG" \
  --vnet-name "AdatumAppGatewayVNET2" \
  --vnet-address-prefix "10.0.0.0/16" \
  --subnet "Appgatewaysubnet2" \
  --subnet-address-prefix "10.0.0.0/28" \
 --servers "10.0.0.5 10.0.0.4" \
  --capacity 2 
  --sku "WAF_Medium" \
  --http-settings-cookie-based-affinity "Enabled" \
  --http-settings-protocol "Http" \
  --frontend-port "80" \
  --routing-rule-type "Basic" \
  --http-settings-port "80" \
  --public-ip-address "pip2" \
  --public-ip-address-allocation "dynamic" \
  --tags "cli[2] owner[administrator]"
```

> [!NOTE]
> <span data-ttu-id="aed90-139">Hello alapvető webalkalmazás tűzfal konfigurációjának létre alkalmazásátjárót védelmet CRS 3.0-val van állítva.</span><span class="sxs-lookup"><span data-stu-id="aed90-139">Application gateways created with hello basic web application firewall configuration are configured with CRS 3.0 for protections.</span></span>

## <a name="get-application-gateway-dns-name"></a><span data-ttu-id="aed90-140">Az Application Gateway DNS-nevének beszerzése</span><span class="sxs-lookup"><span data-stu-id="aed90-140">Get application gateway DNS name</span></span>

<span data-ttu-id="aed90-141">Hello átjáró létrehozása után hello következő lépésre tooconfigure hello előtér-kommunikációhoz.</span><span class="sxs-lookup"><span data-stu-id="aed90-141">Once hello gateway is created, hello next step is tooconfigure hello front end for communication.</span></span> <span data-ttu-id="aed90-142">Nyilvános IP-cím esetén az Application Gateway használatához dinamikusan hozzárendelt DNS-névre van szükség, amely nem valódi név.</span><span class="sxs-lookup"><span data-stu-id="aed90-142">When using a public IP, application gateway requires a dynamically assigned DNS name, which is not friendly.</span></span> <span data-ttu-id="aed90-143">tooensure a végfelhasználók is találati hello Alkalmazásátjáró, egy olyan CNAME rekordot is használt toopoint toohello nyilvános végpontot hello Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="aed90-143">tooensure end users can hit hello application gateway, a CNAME record can be used toopoint toohello public endpoint of hello application gateway.</span></span> <span data-ttu-id="aed90-144">[Egyéni tartománynév konfigurálása az Azure-ban](../cloud-services/cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="aed90-144">[Configuring a custom domain name for in Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span></span> <span data-ttu-id="aed90-145">egy olyan CNAME rekordot tooconfigure hello Alkalmazásátjáró és a társított IP-/ DNS-nevét, hello PublicIPAddress elem csatolt toohello Alkalmazásátjáró részleteit beolvasása.</span><span class="sxs-lookup"><span data-stu-id="aed90-145">tooconfigure a CNAME record, retrieve details of hello application gateway and its associated IP/DNS name using hello PublicIPAddress element attached toohello application gateway.</span></span> <span data-ttu-id="aed90-146">hello alkalmazás átjáró DNS-névnek kell lennie a használt toocreate egy olyan CNAME rekordot pontok hello két webes alkalmazások toothis DNS-név.</span><span class="sxs-lookup"><span data-stu-id="aed90-146">hello application gateway's DNS name should be used toocreate a CNAME record, which points hello two web applications toothis DNS name.</span></span> <span data-ttu-id="aed90-147">A-rekordok hello használata nem javasolt, mert hello VIP módosíthatja az Alkalmazásátjáró újra kell indítani.</span><span class="sxs-lookup"><span data-stu-id="aed90-147">hello use of A-records is not recommended since hello VIP may change on restart of application gateway.</span></span>

```azurecli-interactive
#!/bin/bash

az network public-ip show \
  --name pip2 \
  --resource-group "AdatumAppGatewayRG"
```

```
{
  "dnsSettings": {
    "domainNameLabel": null,
    "fqdn": "8c786058-96d4-4f3e-bb41-660860ceae4c.cloudapp.net",
    "reverseFqdn": null
  },
  "etag": "W/\"3b0ac031-01f0-4860-b572-e3c25e0c57ad\"",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/AdatumAppGatewayRG/providers/Microsoft.Network/publicIPAddresses/pip2",
  "idleTimeoutInMinutes": 4,
  "ipAddress": "40.121.167.250",
  "ipConfiguration": {
    "etag": null,
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/AdatumAppGatewayRG/providers/Microsoft.Network/applicationGateways/AdatumAppGateway2/frontendIPConfigurations/appGatewayFrontendIP",
    "name": null,
    "privateIpAddress": null,
    "privateIpAllocationMethod": null,
    "provisioningState": null,
    "publicIpAddress": null,
    "resourceGroup": "AdatumAppGatewayRG",
    "subnet": null
  },
  "location": "eastus",
  "name": "pip2",
  "provisioningState": "Succeeded",
  "publicIpAddressVersion": "IPv4",
  "publicIpAllocationMethod": "Dynamic",
  "resourceGroup": "AdatumAppGatewayRG",
  "resourceGuid": "3c30d310-c543-4e9d-9c72-bbacd7fe9b05",
  "tags": {
    "cli[2] owner[administrator]": ""
  },
  "type": "Microsoft.Network/publicIPAddresses"
}
```

## <a name="next-steps"></a><span data-ttu-id="aed90-148">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="aed90-148">Next steps</span></span>

<span data-ttu-id="aed90-149">Ismerje meg, hogyan toocustomize WAF szabályok ellátogatva: [testre szabhatja a webes alkalmazás tűzfalszabályok keresztül hello Azure CLI 2.0](application-gateway-customize-waf-rules-cli.md).</span><span class="sxs-lookup"><span data-stu-id="aed90-149">Learn how toocustomize WAF rules by visiting: [Customize web application firewall rules through hello Azure CLI 2.0](application-gateway-customize-waf-rules-cli.md).</span></span>

[scenario]: ./media/application-gateway-web-application-firewall-cli/scenario.png
