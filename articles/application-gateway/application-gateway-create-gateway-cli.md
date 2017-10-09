---
title: egy Azure Application Gateway - aaaCreate Azure CLI 2.0 |} Microsoft Docs
description: "Ismerje meg, hogyan toocreate használatával Alkalmazásátjáró hello Azure CLI 2.0 az erőforrás-kezelőben"
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
ms.openlocfilehash: 952065586cd87d253882438bb779b768d9fd59fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-gateway-by-using-hello-azure-cli-20"></a>Alkalmazásátjáró létrehozása hello Azure CLI 2.0 használatával

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-gateway-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-gateway-arm.md)
> * [Klasszikus Azure PowerShell](application-gateway-create-gateway.md)
> * [Azure Resource Manager-sablon](application-gateway-create-gateway-arm-template.md)
> * [Azure CLI 1.0](application-gateway-create-gateway-cli.md)
> * [Azure CLI 2.0](application-gateway-create-gateway-cli.md)

Alkalmazásátjáró egy dedikált virtuális készülék szolgáltatásként, az alkalmazás kézbesítési vezérlő (LÉPETT) biztosító ajánlat különböző réteg 7 betölteni az alkalmazás terheléselosztási képességeit.

## <a name="cli-versions-toocomplete-hello-task"></a>Parancssori felület verziók toocomplete hello feladat

Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:

* [Az Azure CLI 1.0](application-gateway-create-gateway-cli-nodejs.md) -hello klasszikus és resource management üzembe helyezési modellel a parancssori felületen.
* [Az Azure CLI 2.0](application-gateway-create-gateway-cli.md) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell

## <a name="prerequisite-install-hello-azure-cli-20"></a>Előfeltétel: Hello Azure CLI 2.0 telepítése

tooperform hello ebben a cikkben ismertetett visszaállítási lépésekkel, túl kell[hello Azure parancssori felület Mac, Linux és Windows (Azure CLI) telepítése](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).

> [!NOTE]
> Ha az Azure-fiók nem rendelkezik, meg kell egyet. Itt regisztrálhat az [ingyenes próbaverzióra](../active-directory/sign-up-organization.md).

## <a name="scenario"></a>Forgatókönyv

Ebben a forgatókönyvben a megismerheti, hogyan egy alkalmazást átjáró, amely toocreate hello Azure-portálon.

Ez a forgatókönyv tartalma:

* Hozzon létre egy közepes méretű Alkalmazásátjáró két példányt.
* Nevű AdatumAppGatewayVNET egy fenntartott CIDR-blokkja 10.0.0.0/16, a virtuális hálózat létrehozása.
* Létrehoz egy Appgatewaysubnet nevű alhálózatot, amelynek a CIDR-blokkja 10.0.0.0/28 lesz.

> [!NOTE]
> További konfigurációs hello Alkalmazásátjáró, beleértve az egyéni rendszerállapot-vizsgálat, háttér címkészletet címek és a további szabályok hello Alkalmazásátjáró konfigurálása után, és nem a kezdeti telepítése során konfigurált.

## <a name="before-you-begin"></a>Előkészületek

Az Azure Application Gateway saját alhálózatba van szükség. Virtuális hálózat létrehozásakor győződjön meg arról, hogy hagyja elég cím terület toohave több alhálózattal. Miután telepít egy alkalmazást tooa átjáróalhálózatot, csak további alkalmazásátjárót toohello alhálózati lehet hozzáadni.

## <a name="log-in-tooazure"></a>Jelentkezzen be tooAzure

Nyissa meg hello **Microsoft Azure parancssori**, és jelentkezzen be. 

```azurecli-interactive
az login -u "username"
```

> [!NOTE]
> Is `az login` az eszköz bejelentkezéshez írja be a kódot, aka.ms/devicelogin igénylő hello kapcsoló nélkül.

Amennyiben az előző példa hello írja be, a kód valósul meg. Keresse meg a böngésző toocontinue hello bejelentkezési folyamat toohttps://aka.ms/devicelogin.

![cmd megjelenítő eszköz-bejelentkezés][1]

Hello böngészőben írja be a kapott hello kódot. Biztosan átirányított tooa bejelentkezési oldalára.

![böngésző tooenter kódot][2]

Ha nincs megadva hello kód be van jelentkezve, Bezárás hello böngésző toocontinue hello forgatókönyv.

![sikeres volt][3]

## <a name="create-hello-resource-group"></a>Hello erőforráscsoport létrehozása

Mielőtt létrehozna hello Alkalmazásátjáró, egy erőforráscsoport toocontain hello Alkalmazásátjáró jön létre. hello következő hello parancs jeleníti meg.

```azurecli-interactive
az group create --name myresourcegroup --location "eastus"
```

## <a name="create-hello-application-gateway"></a>Hello Alkalmazásátjáró létrehozása

hello IP-címekre hello háttérkiszolgáló hello IP-címet a háttérkiszolgálón is. Ezek az értékek lehetnek vagy privát IP-címek hello virtuális hálózat, nyilvános IP-cím vagy teljes tartománynevét a háttérkiszolgálókhoz. hello alábbi példa hoz létre a meglévő Alkalmazásátjáró http-beállítások, a portok és a szabályok további konfigurációs beállításait.

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

hello előző példa sok tulajdonságok láthatók, amelyek hello Alkalmazásátjáró létrehozása során nem szükséges. a következő példakód hello Alkalmazásátjáró hoz létre a hello szükséges információkat.

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
> A következő parancs futtatása létrehozása hello során megadható paramétereket listáját: `az network application-gateway create --help`.

Ez a példa egy alapszintű application gateway hello figyelő, a háttérkészlet, a backendhttpsettings osztályhoz és a szabályok az alapértelmezett beállításokat hoz létre. Módosíthatja a beállítások toosuit a központi telepítés után hello kiépítés sikeres.
Ha már van definiálva az előző lépésekben létrehozását követően hello hello háttérkészlet webalkalmazásokat terheléselosztás kezdődik.

## <a name="get-application-gateway-dns-name"></a>Az Application Gateway DNS-nevének beszerzése

Hello átjáró létrehozása után hello következő lépésre tooconfigure hello előtér-kommunikációhoz. Nyilvános IP-cím esetén az Application Gateway használatához dinamikusan hozzárendelt DNS-névre van szükség, amely nem valódi név. tooensure a végfelhasználók is találati hello Alkalmazásátjáró, egy olyan CNAME rekordot is használt toopoint toohello nyilvános végpontot hello Alkalmazásátjáró. [Egyéni tartománynév konfigurálása az Azure-ban](../dns/dns-custom-domain.md). tooconfigure alias, hello Alkalmazásátjáró és a társított IP-/ DNS-nevét, hello PublicIPAddress elem csatolt toohello Alkalmazásátjáró beolvasása. hello alkalmazás átjáró DNS-névnek kell lennie a használt toocreate egy olyan CNAME rekordot pontok hello két webes alkalmazások toothis DNS-név. A-rekordok hello használata nem javasolt, mert hello VIP módosíthatja az Alkalmazásátjáró újra kell indítani.


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

toodelete összes erőforrás létrehozása ebben a cikkben teljes hello a következő lépéseket:

```azurecli-interactive
az group delete --name AdatumAppGatewayRG
```
 
## <a name="next-steps"></a>Következő lépések

Ismerje meg, hogyan toocreate egyéni állapot-mintavételi csomagjai ellátogatva [hozzon létre egy egyéni állapotmintáihoz](application-gateway-create-probe-portal.md)

Ismerje meg, hogyan SSL-Feladatkiszervezést tooconfigure és hajtsa végre a megfelelő hello költséges SSL visszafejtési ki a webkiszolgálók ellátogatva [SSL-kiszervezés konfigurálása](application-gateway-ssl-arm.md)

<!--Image references-->

[scenario]: ./media/application-gateway-create-gateway-cli/scenario.png
[1]: ./media/application-gateway-create-gateway-cli/figure1.png
[2]: ./media/application-gateway-create-gateway-cli/figure2.png
[3]: ./media/application-gateway-create-gateway-cli/figure3.png
