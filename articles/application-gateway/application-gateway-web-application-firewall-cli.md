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
# <a name="configure-web-application-firewall-on-a-new-or-existing-application-gateway-with-azure-cli"></a>Webalkalmazási tűzfal konfigurálása egy új vagy meglévő Alkalmazásátjáró Azure parancssori felülettel

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-web-application-firewall-portal.md)
> * [PowerShell](application-gateway-web-application-firewall-powershell.md)
> * [Azure CLI](application-gateway-web-application-firewall-cli.md)

Ismerje meg, hogyan toocreate webalkalmazási tűzfal engedélyezve van az Alkalmazásátjáró, vagy adja hozzá a webes alkalmazás tűzfal tooan meglévő Alkalmazásátjáró.

hello webalkalmazási tűzfal (waf-ot) az Azure alkalmazás átjáró webalkalmazások védje a közös web-alapú támadások, például az SQL-injektálás, a többhelyes parancsfájlok futtatására és a munkamenet kihasználásának.

Az Azure Application Gateway egy 7. rétegbeli terheléselosztó. Akár hello felhőalapú vagy helyszíni biztosít feladatátvételt, HTTP-kérelmek teljesítmény-útválasztási különböző kiszolgálók között. Alkalmazásátjáró biztosít számos alkalmazás kézbesítési vezérlő (LÉPETT) szolgáltatást HTTP beleértve betöltése terheléselosztási, a cookie-alapú munkamenet-kapcsolatot, és a secure sockets layer (SSL)-kiszervezés el egyéni állapotteljesítmény, többhelyes támogatása, és sok más. támogatott szolgáltatások teljes listáját toofind látogassa meg: [Alkalmazásátjáró áttekintése](application-gateway-introduction.md).

hello következő a cikk bemutatja, hogyan túl[hozzáadása a webes alkalmazás tűzfal tooan meglévő Alkalmazásátjáró](#add-web-application-firewall-to-an-existing-application-gateway) és [webalkalmazási tűzfal használó Alkalmazásátjáró létrehozása](#create-an-application-gateway-with-web-application-firewall).

![a forgatókönyv kép][scenario]

## <a name="prerequisite-install-hello-azure-cli-20"></a>Előfeltétel: Hello Azure CLI 2.0 telepítése

tooperform hello ebben a cikkben ismertetett visszaállítási lépésekkel, túl kell[hello Azure parancssori felület Mac, Linux és Windows (Azure CLI) telepítése](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).

## <a name="waf-configuration-differences"></a>WAF konfigurációs különbségek

Ha mindenképpen olvassa el [hozzon létre egy alkalmazást az Azure parancssori felület](application-gateway-create-gateway-cli.md), tisztában hello SKU beállítások tooconfigure Alkalmazásátjáró létrehozása során. WAF biztosít a további beállítások toodefine Alkalmazásátjáró hello SKU konfigurálásakor. Nincsenek további módosítások, amely akkor adja meg a hello Alkalmazásátjáró magát.

| **Beállítás** | **Részletek**
|---|---|
|**Termékváltozat** |Egy normál Alkalmazásátjáró nélkül WAF támogatja **szabványos\_kis**, **szabványos\_Közepes**, és **szabványos\_nagy**méretét. WAF hello bevezetését, a esetén két további SKU, **WAF\_Közepes** és **WAF\_nagy**. WAF kis alkalmazásátjárót nem támogatott.|
|**Mód** | Ez a beállítás akkor WAF hello módját. két érték engedélyezett **észlelési** és **megelőzési**. Ha WAF észlelési mód van beállítva, minden fenyegetések naplófájl vannak tárolva. Megelőzési módban eseményeket naplózza, de hello támadó 403-as jogosulatlan választ kap hello Alkalmazásátjáró.|

## <a name="add-web-application-firewall-tooan-existing-application-gateway"></a>Webes alkalmazás tűzfal tooan meglévő Alkalmazásátjáró hozzáadása

hello egy meglévő szabványos alkalmazás tooa WAF engedélyezett alkalmazás átjárók kövesse a parancs végrehajtása.

```azurecli-interactive
#!/bin/bash

az network application-gateway waf-config set \
  --enabled true \
  --firewall-mode Prevention \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG"
```

Ez a parancs webalkalmazási tűzfal hello Alkalmazásátjáró frissítése. Látogasson el [átjáró Alkalmazásdiagnosztika](application-gateway-diagnostics.md) toounderstand, hogy miként naplózza az tooview az alkalmazás átjáró. WAF toohello biztonsági jellege miatt naplók kell toobe rendszeresen ellenőrizni a webalkalmazások toounderstand hello biztonsági állapotát.

## <a name="create-an-application-gateway-with-web-application-firewall"></a>Webalkalmazási tűzfal Alkalmazásátjáró létrehozása

a következő parancs hello Alkalmazásátjáró webalkalmazási tűzfal hoz létre.

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
> Hello alapvető webalkalmazás tűzfal konfigurációjának létre alkalmazásátjárót védelmet CRS 3.0-val van állítva.

## <a name="get-application-gateway-dns-name"></a>Az Application Gateway DNS-nevének beszerzése

Hello átjáró létrehozása után hello következő lépésre tooconfigure hello előtér-kommunikációhoz. Nyilvános IP-cím esetén az Application Gateway használatához dinamikusan hozzárendelt DNS-névre van szükség, amely nem valódi név. tooensure a végfelhasználók is találati hello Alkalmazásátjáró, egy olyan CNAME rekordot is használt toopoint toohello nyilvános végpontot hello Alkalmazásátjáró. [Egyéni tartománynév konfigurálása az Azure-ban](../cloud-services/cloud-services-custom-domain-name-portal.md). egy olyan CNAME rekordot tooconfigure hello Alkalmazásátjáró és a társított IP-/ DNS-nevét, hello PublicIPAddress elem csatolt toohello Alkalmazásátjáró részleteit beolvasása. hello alkalmazás átjáró DNS-névnek kell lennie a használt toocreate egy olyan CNAME rekordot pontok hello két webes alkalmazások toothis DNS-név. A-rekordok hello használata nem javasolt, mert hello VIP módosíthatja az Alkalmazásátjáró újra kell indítani.

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

Ismerje meg, hogyan toocustomize WAF szabályok ellátogatva: [testre szabhatja a webes alkalmazás tűzfalszabályok keresztül hello Azure CLI 2.0](application-gateway-customize-waf-rules-cli.md).

[scenario]: ./media/application-gateway-web-application-firewall-cli/scenario.png
