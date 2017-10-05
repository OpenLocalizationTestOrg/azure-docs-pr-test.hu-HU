---
title: "Webalkalmazási tűzfal - Azure Application Gateway konfigurálása |} Microsoft Docs"
description: "Ez a cikk útmutatást webalkalmazási tűzfal egy új vagy meglévő Alkalmazásátjáró használatának módjáról."
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
ms.openlocfilehash: ac6c629ceaf1a8036643f593ce3d7ef9ea096ef8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="configure-web-application-firewall-on-a-new-or-existing-application-gateway-with-azure-cli"></a><span data-ttu-id="e1e86-103">Webalkalmazási tűzfal konfigurálása egy új vagy meglévő Alkalmazásátjáró Azure parancssori felülettel</span><span class="sxs-lookup"><span data-stu-id="e1e86-103">Configure web application firewall on a new or existing Application Gateway with Azure CLI</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="e1e86-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="e1e86-104">Azure portal</span></span>](application-gateway-web-application-firewall-portal.md)
> * [<span data-ttu-id="e1e86-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e1e86-105">PowerShell</span></span>](application-gateway-web-application-firewall-powershell.md)
> * [<span data-ttu-id="e1e86-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="e1e86-106">Azure CLI</span></span>](application-gateway-web-application-firewall-cli.md)

<span data-ttu-id="e1e86-107">Útmutató a webes alkalmazás engedélyezve van a tűzfal Alkalmazásátjáró létrehozása vagy meglévő Alkalmazásátjáró webalkalmazási tűzfal hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="e1e86-107">Learn how to create a web application firewall enabled application gateway or add web application firewall to an existing application gateway.</span></span>

<span data-ttu-id="e1e86-108">A webes alkalmazás tűzfalat (WAF) Azure Application Gateway webalkalmazások védje a közös web-alapú támadások, például az SQL-injektálás, a többhelyes parancsfájlok futtatására és a munkamenet kihasználásának.</span><span class="sxs-lookup"><span data-stu-id="e1e86-108">The web application firewall (WAF) in Azure Application Gateway protects web applications from common web-based attacks like SQL injection, cross-site scripting attacks, and session hijacks.</span></span>

<span data-ttu-id="e1e86-109">Az Azure Application Gateway egy 7. rétegbeli terheléselosztó.</span><span class="sxs-lookup"><span data-stu-id="e1e86-109">Azure Application Gateway is a layer-7 load balancer.</span></span> <span data-ttu-id="e1e86-110">Feladatátvételt és teljesítményalapú útválasztást biztosít a HTTP-kérelmek számára különböző kiszolgálók között, függetlenül attól, hogy a felhőben vagy a helyszínen vannak.</span><span class="sxs-lookup"><span data-stu-id="e1e86-110">It provides failover, performance-routing HTTP requests between different servers, whether they are on the cloud or on-premises.</span></span> <span data-ttu-id="e1e86-111">Alkalmazásátjáró biztosít számos alkalmazás kézbesítési vezérlő (LÉPETT) szolgáltatást HTTP beleértve betöltése terheléselosztási, a cookie-alapú munkamenet-kapcsolatot, és a secure sockets layer (SSL)-kiszervezés el egyéni állapotteljesítmény, többhelyes támogatása, és sok más.</span><span class="sxs-lookup"><span data-stu-id="e1e86-111">Application gateway provides many application delivery controller (ADC) features including HTTP load balancing, cookie-based session affinity, secure sockets layer (SSL) offload, custom health probes, support for multi-site, and many others.</span></span> <span data-ttu-id="e1e86-112">Támogatott funkció teljes listájának megkereséséhez látogasson el a: [Alkalmazásátjáró áttekintése](application-gateway-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="e1e86-112">To find a complete list of supported features, visit: [Overview of Application Gateway](application-gateway-introduction.md).</span></span>

<span data-ttu-id="e1e86-113">A következő cikk bemutatja hogyan [webalkalmazási tűzfal hozzáadása egy meglévő Alkalmazásátjáró](#add-web-application-firewall-to-an-existing-application-gateway) és [webalkalmazási tűzfal használó Alkalmazásátjáró létrehozása](#create-an-application-gateway-with-web-application-firewall).</span><span class="sxs-lookup"><span data-stu-id="e1e86-113">The following article shows how to [add web application firewall to an existing application gateway](#add-web-application-firewall-to-an-existing-application-gateway) and [create an application gateway that uses web application firewall](#create-an-application-gateway-with-web-application-firewall).</span></span>

![a forgatókönyv kép][scenario]

## <a name="prerequisite-install-the-azure-cli-20"></a><span data-ttu-id="e1e86-115">Előfeltétel: Az Azure parancssori felület 2.0 telepítése</span><span class="sxs-lookup"><span data-stu-id="e1e86-115">Prerequisite: Install the Azure CLI 2.0</span></span>

<span data-ttu-id="e1e86-116">Ebben a cikkben szereplő lépések végrehajtásához kell [telepítse az Azure parancssori felület Mac, Linux és Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="e1e86-116">To perform the steps in this article, you need to [install the Azure Command-Line Interface for Mac, Linux, and Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span></span>

## <a name="waf-configuration-differences"></a><span data-ttu-id="e1e86-117">WAF konfigurációs különbségek</span><span class="sxs-lookup"><span data-stu-id="e1e86-117">WAF configuration differences</span></span>

<span data-ttu-id="e1e86-118">Ha mindenképpen olvassa el [hozzon létre egy alkalmazást az Azure parancssori felület](application-gateway-create-gateway-cli.md), hogy tudomásul veszi, hogy a Termékváltozat-beállítások konfigurálása az Alkalmazásátjáró létrehozása.</span><span class="sxs-lookup"><span data-stu-id="e1e86-118">If you have read [Create an Application Gateway with Azure CLI](application-gateway-create-gateway-cli.md), you understand the SKU settings to configure when creating an application gateway.</span></span> <span data-ttu-id="e1e86-119">WAF Alkalmazásátjáró a Termékváltozat konfigurálásakor adja meg a további beállításokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="e1e86-119">WAF provides additional settings to define when configuring the SKU on an application gateway.</span></span> <span data-ttu-id="e1e86-120">Nincsenek további módosítások, amely akkor adja meg az Alkalmazásátjáró magát a.</span><span class="sxs-lookup"><span data-stu-id="e1e86-120">There are no additional changes that you make on the application gateway itself.</span></span>

| <span data-ttu-id="e1e86-121">**Beállítás**</span><span class="sxs-lookup"><span data-stu-id="e1e86-121">**Setting**</span></span> | <span data-ttu-id="e1e86-122">**Részletek**</span><span class="sxs-lookup"><span data-stu-id="e1e86-122">**Details**</span></span>
|---|---|
|<span data-ttu-id="e1e86-123">**Termékváltozat**</span><span class="sxs-lookup"><span data-stu-id="e1e86-123">**SKU**</span></span> |<span data-ttu-id="e1e86-124">Egy normál Alkalmazásátjáró nélkül WAF támogatja **szabványos\_kis**, **szabványos\_Közepes**, és **szabványos\_nagy**méretét.</span><span class="sxs-lookup"><span data-stu-id="e1e86-124">A normal application gateway without WAF supports **Standard\_Small**, **Standard\_Medium**, and **Standard\_Large** sizes.</span></span> <span data-ttu-id="e1e86-125">A WAF bevezetése esetén két további SKU, **WAF\_Közepes** és **WAF\_nagy**.</span><span class="sxs-lookup"><span data-stu-id="e1e86-125">With the introduction of WAF, there are two additional SKUs, **WAF\_Medium** and **WAF\_Large**.</span></span> <span data-ttu-id="e1e86-126">WAF kis alkalmazásátjárót nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="e1e86-126">WAF is not supported on small application gateways.</span></span>|
|<span data-ttu-id="e1e86-127">**Mód**</span><span class="sxs-lookup"><span data-stu-id="e1e86-127">**Mode**</span></span> | <span data-ttu-id="e1e86-128">Ez a beállítás akkor WAF módját.</span><span class="sxs-lookup"><span data-stu-id="e1e86-128">This setting is the mode of WAF.</span></span> <span data-ttu-id="e1e86-129">két érték engedélyezett **észlelési** és **megelőzési**.</span><span class="sxs-lookup"><span data-stu-id="e1e86-129">allowed values are **Detection** and **Prevention**.</span></span> <span data-ttu-id="e1e86-130">Ha WAF észlelési mód van beállítva, minden fenyegetések naplófájl vannak tárolva.</span><span class="sxs-lookup"><span data-stu-id="e1e86-130">When WAF is set up in detection mode, all threats are stored in a log file.</span></span> <span data-ttu-id="e1e86-131">Megelőzési módban eseményeket naplózza, de a támadó 403-as jogosulatlan választ kap az Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="e1e86-131">In prevention mode, events are still logged but the attacker receives a 403 unauthorized response from the application gateway.</span></span>|

## <a name="add-web-application-firewall-to-an-existing-application-gateway"></a><span data-ttu-id="e1e86-132">Webalkalmazási tűzfal hozzáadása egy meglévő Alkalmazásátjáró</span><span class="sxs-lookup"><span data-stu-id="e1e86-132">Add web application firewall to an existing application gateway</span></span>

<span data-ttu-id="e1e86-133">A következő parancsot egy meglévő szabványos Alkalmazásátjáró WAF engedélyezett Alkalmazásátjáró változik.</span><span class="sxs-lookup"><span data-stu-id="e1e86-133">The follow command changes an existing standard application gateway to a WAF enabled application gateway.</span></span>

```azurecli-interactive
#!/bin/bash

az network application-gateway waf-config set \
  --enabled true \
  --firewall-mode Prevention \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG"
```

<span data-ttu-id="e1e86-134">Ez a parancs frissíti az Alkalmazásátjáró webalkalmazási tűzfal.</span><span class="sxs-lookup"><span data-stu-id="e1e86-134">This command updates the application gateway with web application firewall.</span></span> <span data-ttu-id="e1e86-135">Látogasson el [átjáró Alkalmazásdiagnosztika](application-gateway-diagnostics.md) annak megértése, hogyan az Alkalmazásátjáró a naplók megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="e1e86-135">Visit [Application Gateway Diagnostics](application-gateway-diagnostics.md) to understand how to view logs for your application gateway.</span></span> <span data-ttu-id="e1e86-136">WAF biztonsági jellege miatt naplók kell tudni, hogy a biztonságot, a webalkalmazások rendszeresen vizsgálni.</span><span class="sxs-lookup"><span data-stu-id="e1e86-136">Due to the security nature of WAF, logs need to be reviewed regularly to understand the security posture of your web applications.</span></span>

## <a name="create-an-application-gateway-with-web-application-firewall"></a><span data-ttu-id="e1e86-137">Webalkalmazási tűzfal Alkalmazásátjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="e1e86-137">Create an Application Gateway with web application firewall</span></span>

<span data-ttu-id="e1e86-138">A következő parancsot egy Application Gateway webalkalmazási tűzfal hoz létre.</span><span class="sxs-lookup"><span data-stu-id="e1e86-138">The following command creates an Application Gateway with web application firewall.</span></span>

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
> <span data-ttu-id="e1e86-139">Az alapszintű webes alkalmazás tűzfal konfigurációját létre alkalmazásátjárót védelmet CRS 3.0-val van állítva.</span><span class="sxs-lookup"><span data-stu-id="e1e86-139">Application gateways created with the basic web application firewall configuration are configured with CRS 3.0 for protections.</span></span>

## <a name="get-application-gateway-dns-name"></a><span data-ttu-id="e1e86-140">Az Application Gateway DNS-nevének beszerzése</span><span class="sxs-lookup"><span data-stu-id="e1e86-140">Get application gateway DNS name</span></span>

<span data-ttu-id="e1e86-141">Az átjáró létrehozása után a következő lépés a kommunikációra szolgáló előtér konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="e1e86-141">Once the gateway is created, the next step is to configure the front end for communication.</span></span> <span data-ttu-id="e1e86-142">Nyilvános IP-cím esetén az Application Gateway használatához dinamikusan hozzárendelt DNS-névre van szükség, amely nem valódi név.</span><span class="sxs-lookup"><span data-stu-id="e1e86-142">When using a public IP, application gateway requires a dynamically assigned DNS name, which is not friendly.</span></span> <span data-ttu-id="e1e86-143">Ha szeretné, hogy a végfelhasználók elérjék az Application Gatewayt, használjon egy Application Gateway nyilvános végpontjára mutató CNAME-rekordot.</span><span class="sxs-lookup"><span data-stu-id="e1e86-143">To ensure end users can hit the application gateway, a CNAME record can be used to point to the public endpoint of the application gateway.</span></span> <span data-ttu-id="e1e86-144">[Egyéni tartománynév konfigurálása az Azure-ban](../cloud-services/cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="e1e86-144">[Configuring a custom domain name for in Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span></span> <span data-ttu-id="e1e86-145">Egy olyan CNAME rekordot konfigurálásához részleteit az Alkalmazásátjáró és a társított IP-/ DNS-nevét, a PublicIPAddress elem csatolva az Alkalmazásátjáró beolvasása.</span><span class="sxs-lookup"><span data-stu-id="e1e86-145">To configure a CNAME record, retrieve details of the application gateway and its associated IP/DNS name using the PublicIPAddress element attached to the application gateway.</span></span> <span data-ttu-id="e1e86-146">Az Application Gateway DNS-nevének használatával létrehozhat egy CNAME rekordot, amely a két webalkalmazást erre a DNS-névre irányítja.</span><span class="sxs-lookup"><span data-stu-id="e1e86-146">The application gateway's DNS name should be used to create a CNAME record, which points the two web applications to this DNS name.</span></span> <span data-ttu-id="e1e86-147">Az A-bejegyzések használata nem javasolt, mivel a virtuális IP-cím változhat az Application Gateway újraindításakor.</span><span class="sxs-lookup"><span data-stu-id="e1e86-147">The use of A-records is not recommended since the VIP may change on restart of application gateway.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="e1e86-148">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e1e86-148">Next steps</span></span>

<span data-ttu-id="e1e86-149">WAF szabályok testreszabása ellátogatva: [testre szabhatja a webes alkalmazás tűzfalszabályok keresztül az Azure CLI 2.0](application-gateway-customize-waf-rules-cli.md).</span><span class="sxs-lookup"><span data-stu-id="e1e86-149">Learn how to customize WAF rules by visiting: [Customize web application firewall rules through the Azure CLI 2.0](application-gateway-customize-waf-rules-cli.md).</span></span>

[scenario]: ./media/application-gateway-web-application-firewall-cli/scenario.png
