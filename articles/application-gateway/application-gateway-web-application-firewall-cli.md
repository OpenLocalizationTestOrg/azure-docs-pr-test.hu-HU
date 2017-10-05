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
# <a name="configure-web-application-firewall-on-a-new-or-existing-application-gateway-with-azure-cli"></a>Webalkalmazási tűzfal konfigurálása egy új vagy meglévő Alkalmazásátjáró Azure parancssori felülettel

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-web-application-firewall-portal.md)
> * [PowerShell](application-gateway-web-application-firewall-powershell.md)
> * [Azure CLI](application-gateway-web-application-firewall-cli.md)

Útmutató a webes alkalmazás engedélyezve van a tűzfal Alkalmazásátjáró létrehozása vagy meglévő Alkalmazásátjáró webalkalmazási tűzfal hozzáadása.

A webes alkalmazás tűzfalat (WAF) Azure Application Gateway webalkalmazások védje a közös web-alapú támadások, például az SQL-injektálás, a többhelyes parancsfájlok futtatására és a munkamenet kihasználásának.

Az Azure Application Gateway egy 7. rétegbeli terheléselosztó. Feladatátvételt és teljesítményalapú útválasztást biztosít a HTTP-kérelmek számára különböző kiszolgálók között, függetlenül attól, hogy a felhőben vagy a helyszínen vannak. Alkalmazásátjáró biztosít számos alkalmazás kézbesítési vezérlő (LÉPETT) szolgáltatást HTTP beleértve betöltése terheléselosztási, a cookie-alapú munkamenet-kapcsolatot, és a secure sockets layer (SSL)-kiszervezés el egyéni állapotteljesítmény, többhelyes támogatása, és sok más. Támogatott funkció teljes listájának megkereséséhez látogasson el a: [Alkalmazásátjáró áttekintése](application-gateway-introduction.md).

A következő cikk bemutatja hogyan [webalkalmazási tűzfal hozzáadása egy meglévő Alkalmazásátjáró](#add-web-application-firewall-to-an-existing-application-gateway) és [webalkalmazási tűzfal használó Alkalmazásátjáró létrehozása](#create-an-application-gateway-with-web-application-firewall).

![a forgatókönyv kép][scenario]

## <a name="prerequisite-install-the-azure-cli-20"></a>Előfeltétel: Az Azure parancssori felület 2.0 telepítése

Ebben a cikkben szereplő lépések végrehajtásához kell [telepítse az Azure parancssori felület Mac, Linux és Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).

## <a name="waf-configuration-differences"></a>WAF konfigurációs különbségek

Ha mindenképpen olvassa el [hozzon létre egy alkalmazást az Azure parancssori felület](application-gateway-create-gateway-cli.md), hogy tudomásul veszi, hogy a Termékváltozat-beállítások konfigurálása az Alkalmazásátjáró létrehozása. WAF Alkalmazásátjáró a Termékváltozat konfigurálásakor adja meg a további beállításokat tartalmazza. Nincsenek további módosítások, amely akkor adja meg az Alkalmazásátjáró magát a.

| **Beállítás** | **Részletek**
|---|---|
|**Termékváltozat** |Egy normál Alkalmazásátjáró nélkül WAF támogatja **szabványos\_kis**, **szabványos\_Közepes**, és **szabványos\_nagy**méretét. A WAF bevezetése esetén két további SKU, **WAF\_Közepes** és **WAF\_nagy**. WAF kis alkalmazásátjárót nem támogatott.|
|**Mód** | Ez a beállítás akkor WAF módját. két érték engedélyezett **észlelési** és **megelőzési**. Ha WAF észlelési mód van beállítva, minden fenyegetések naplófájl vannak tárolva. Megelőzési módban eseményeket naplózza, de a támadó 403-as jogosulatlan választ kap az Alkalmazásátjáró.|

## <a name="add-web-application-firewall-to-an-existing-application-gateway"></a>Webalkalmazási tűzfal hozzáadása egy meglévő Alkalmazásátjáró

A következő parancsot egy meglévő szabványos Alkalmazásátjáró WAF engedélyezett Alkalmazásátjáró változik.

```azurecli-interactive
#!/bin/bash

az network application-gateway waf-config set \
  --enabled true \
  --firewall-mode Prevention \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG"
```

Ez a parancs frissíti az Alkalmazásátjáró webalkalmazási tűzfal. Látogasson el [átjáró Alkalmazásdiagnosztika](application-gateway-diagnostics.md) annak megértése, hogyan az Alkalmazásátjáró a naplók megtekintéséhez. WAF biztonsági jellege miatt naplók kell tudni, hogy a biztonságot, a webalkalmazások rendszeresen vizsgálni.

## <a name="create-an-application-gateway-with-web-application-firewall"></a>Webalkalmazási tűzfal Alkalmazásátjáró létrehozása

A következő parancsot egy Application Gateway webalkalmazási tűzfal hoz létre.

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
> Az alapszintű webes alkalmazás tűzfal konfigurációját létre alkalmazásátjárót védelmet CRS 3.0-val van állítva.

## <a name="get-application-gateway-dns-name"></a>Az Application Gateway DNS-nevének beszerzése

Az átjáró létrehozása után a következő lépés a kommunikációra szolgáló előtér konfigurálása. Nyilvános IP-cím esetén az Application Gateway használatához dinamikusan hozzárendelt DNS-névre van szükség, amely nem valódi név. Ha szeretné, hogy a végfelhasználók elérjék az Application Gatewayt, használjon egy Application Gateway nyilvános végpontjára mutató CNAME-rekordot. [Egyéni tartománynév konfigurálása az Azure-ban](../cloud-services/cloud-services-custom-domain-name-portal.md). Egy olyan CNAME rekordot konfigurálásához részleteit az Alkalmazásátjáró és a társított IP-/ DNS-nevét, a PublicIPAddress elem csatolva az Alkalmazásátjáró beolvasása. Az Application Gateway DNS-nevének használatával létrehozhat egy CNAME rekordot, amely a két webalkalmazást erre a DNS-névre irányítja. Az A-bejegyzések használata nem javasolt, mivel a virtuális IP-cím változhat az Application Gateway újraindításakor.

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

## <a name="next-steps"></a>Következő lépések

WAF szabályok testreszabása ellátogatva: [testre szabhatja a webes alkalmazás tűzfalszabályok keresztül az Azure CLI 2.0](application-gateway-customize-waf-rules-cli.md).

[scenario]: ./media/application-gateway-web-application-firewall-cli/scenario.png
