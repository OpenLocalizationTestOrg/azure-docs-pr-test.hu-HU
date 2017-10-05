---
title: "Hozzon létre egy Azure Application Gateway - Azure CLI 2.0 |} Microsoft Docs"
description: "Útmutató Alkalmazásátjáró létrehozása az Azure CLI 2.0 erőforrás-kezelő használatával"
services: application-gateway
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: c2f6516e-3805-49ac-826e-776b909a9104
ms.service: application-gateway
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: gwallace
ms.openlocfilehash: 052410db8c7619c7990dc319951a55663f2c2ba1
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="create-an-application-gateway-by-using-the-azure-cli-20"></a>Alkalmazásátjáró létrehozása az Azure CLI 2.0 használatával

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-gateway-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-gateway-arm.md)
> * [Klasszikus Azure PowerShell](application-gateway-create-gateway.md)
> * [Azure Resource Manager-sablon](application-gateway-create-gateway-arm-template.md)
> * [Azure CLI 1.0](application-gateway-create-gateway-cli.md)
> * [Azure CLI 2.0](application-gateway-create-gateway-cli.md)

Alkalmazásátjáró egy dedikált virtuális készülék szolgáltatásként, az alkalmazás kézbesítési vezérlő (LÉPETT) biztosító ajánlat különböző réteg 7 betölteni az alkalmazás terheléselosztási képességeit.

## <a name="cli-versions-to-complete-the-task"></a>A feladat befejezéséhez használható CLI-verziók

A következő CLI-verziók egyikével elvégezheti a feladatot:

* [Azure CLI 1.0](application-gateway-create-gateway-cli-nodejs.md) – parancssori felületünk a klasszikus és a Resource Management üzemi modellekhez.
* [Azure CLI 2.0](application-gateway-create-gateway-cli.md) – a Resource Management üzemi modellhez tartozó parancssori felületek következő generációját képviseli.

## <a name="prerequisite-install-the-azure-cli-20"></a>Előfeltétel: Az Azure parancssori felület 2.0 telepítése

Ebben a cikkben szereplő lépések végrehajtásához kell [telepítse az Azure parancssori felület Mac, Linux és Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).

> [!NOTE]
> Ha az Azure-fiók nem rendelkezik, meg kell egyet. Itt regisztrálhat az [ingyenes próbaverzióra](../active-directory/sign-up-organization.md).

## <a name="scenario"></a>Forgatókönyv

Ebben a forgatókönyvben ismerje meg az Azure portál használatával Alkalmazásátjáró létrehozása.

Ez a forgatókönyv tartalma:

* Hozzon létre egy közepes méretű Alkalmazásátjáró két példányt.
* Nevű AdatumAppGatewayVNET egy fenntartott CIDR-blokkja 10.0.0.0/16, a virtuális hálózat létrehozása.
* Létrehoz egy Appgatewaysubnet nevű alhálózatot, amelynek a CIDR-blokkja 10.0.0.0/28 lesz.

> [!NOTE]
> Az Alkalmazásátjáró, beleértve az egyéni állapotfigyelő további konfigurációs vizsgálat, háttér címkészletet címeket, és további szabályok is konfigurálva van az Alkalmazásátjáró konfigurálása után, és nem a kezdeti üzembe helyezése során.

## <a name="before-you-begin"></a>Előkészületek

Az Azure Application Gateway saját alhálózatba van szükség. Amikor egy virtuális hálózat létrehozásával, győződjön meg arról, hogy hagyja-e elég hely a cím több alhálózattal rendelkezik. Miután telepít egy alhálózatra Alkalmazásátjáró, csak további alkalmazásátjárót alhálózathoz lehet hozzáadni.

## <a name="log-in-to-azure"></a>Jelentkezzen be az Azure-ba

Nyissa meg a **Microsoft Azure parancssori**, és jelentkezzen be. 

```azurecli-interactive
az login -u "username"
```

> [!NOTE]
> Is `az login` az eszköz bejelentkezéshez írja be a kódot, aka.ms/devicelogin igénylő kapcsoló nélkül.

Miután beírta a fenti példában, a kód valósul meg. Nyissa meg a böngészőben a bejelentkezési folyamat folytatásához https://aka.ms/devicelogin.

![cmd megjelenítő eszköz-bejelentkezés][1]

A böngészőben írja be a kapott kódot. Ekkor megnyílik egy bejelentkezési oldalára.

![Írja be a kódját a böngésző][2]

Ha nincs megadva a kód be van jelentkezve, zárja be a böngészőt, és a forgatókönyv a folytatáshoz.

![sikeres volt][3]

## <a name="create-the-resource-group"></a>Az erőforráscsoport létrehozása

Az Alkalmazásátjáró létrehozása, mielőtt egy erőforráscsoportot hoz létre az Alkalmazásátjáró. Az alábbiakban a parancs látható.

```azurecli-interactive
az group create --name myresourcegroup --location "eastus"
```

## <a name="create-the-application-gateway"></a>Application Gateway létrehozása

A háttérkiszolgáló az IP-címeit az IP-címet a háttérkiszolgálón is. Ezek az értékek lehetnek privát IP-címek a virtuális hálózat, nyilvános IP-cím vagy teljes tartománynevét a háttérkiszolgálókhoz. A következő példa olyan átjárót hoz létre a http-beállítások, a portok és a szabályok további konfigurációs beállításait.

```azurecli-interactive
az network application-gateway create \
--name "AdatumAppGateway" \
--location "eastus" \
--resource-group "myresourcegroup" \
--vnet-name "AdatumAppGatewayVNET" \
--vnet-address-prefix "10.0.0.0/16" \
--subnet "Appgatewaysubnet" \
--subnet-address-prefix "10.0.0.0/28" \
--servers 10.0.0.4 10.0.0.5 \
--capacity 2 \
--sku Standard_Small \
--http-settings-cookie-based-affinity Enabled \
--http-settings-protocol Http \
--frontend-port 80 \
--routing-rule-type Basic \
--http-settings-port 80 \
--public-ip-address "pip2" \
--public-ip-address-allocation "dynamic" \

```

Az előző példában látható sok tulajdonságok esetében nem szükséges Alkalmazásátjáró létrehozása közben. A következő kódrészlet példa olyan átjárót hoz létre a szükséges adatokat.

```azurecli-interactive
az network application-gateway create \
--name "AdatumAppGateway" \
--location "eastus" \
--resource-group "myresourcegroup" \
--vnet-name "AdatumAppGatewayVNET" \
--vnet-address-prefix "10.0.0.0/16" \
--subnet "Appgatewaysubnet \
--subnet-address-prefix "10.0.0.0/28" \
--servers "10.0.0.5"  \
--public-ip-address pip
```
 
> [!NOTE]
> A következő parancs létrehozása során megadható paramétereket listáját: `az network application-gateway create --help`.

Ez a példa egy alapszintű application gateway alapértelmezett beállításait a figyelő, a háttérkészlet, a backendhttpsettings osztályhoz és a szabályok hoz létre. Ezeket a beállításokat a központi telepítés megfelelően, ha a kiépítés sikeres módosíthatja.
Ha már van definiálva az előző lépésekben, a létrehozását követően, a háttérkészletben webalkalmazásokat terheléselosztás kezdődik.

## <a name="get-application-gateway-dns-name"></a>Az Application Gateway DNS-nevének beszerzése

Az átjáró létrehozása után a következő lépés a kommunikációra szolgáló előtér konfigurálása. Nyilvános IP-cím esetén az Application Gateway használatához dinamikusan hozzárendelt DNS-névre van szükség, amely nem valódi név. Ha szeretné, hogy a végfelhasználók elérjék az Application Gatewayt, használjon egy Application Gateway nyilvános végpontjára mutató CNAME-rekordot. [Egyéni tartománynév konfigurálása az Azure-ban](../dns/dns-custom-domain.md). Alias konfigurálásához az Alkalmazásátjáró és a társított IP-/ DNS-nevét, a PublicIPAddress elem csatolva az Alkalmazásátjáró beolvasása. Az Application Gateway DNS-nevének használatával létrehozhat egy CNAME rekordot, amely a két webalkalmazást erre a DNS-névre irányítja. Az A-bejegyzések használata nem javasolt, mivel a virtuális IP-cím változhat az Application Gateway újraindításakor.


```azurecli-interactive
az network public-ip show --name "pip" --resource-group "AdatumAppGatewayRG"
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

## <a name="delete-all-resources"></a>Az összes erőforrás törlése

A jelen cikkben létrehozott összes erőforrás törléséhez hajtsa végre az alábbi lépéseket:

```azurecli-interactive
az group delete --name AdatumAppGatewayRG
```
 
## <a name="next-steps"></a>Következő lépések

Megtudhatja, hogyan hozzon létre egyéni állapotteljesítmény ellátogatva [hozzon létre egy egyéni állapotmintáihoz](application-gateway-create-probe-portal.md)

Megtudhatja, hogyan konfigurálja az SSL-Feladatkiszervezést, és tegye meg a költséges SSL visszafejtési ki a webkiszolgálók ellátogatva [SSL-kiszervezés konfigurálása](application-gateway-ssl-arm.md)

<!--Image references-->

[scenario]: ./media/application-gateway-create-gateway-cli/scenario.png
[1]: ./media/application-gateway-create-gateway-cli/figure1.png
[2]: ./media/application-gateway-create-gateway-cli/figure2.png
[3]: ./media/application-gateway-create-gateway-cli/figure3.png
