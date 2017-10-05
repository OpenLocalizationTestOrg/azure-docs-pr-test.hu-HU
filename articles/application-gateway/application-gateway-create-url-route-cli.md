---
title: "URL-útválasztási szabályok - Azure CLI 2.0 használatával Alkalmazásátjáró létrehozása |} Microsoft Docs"
description: "Ezen a lapon létrehozása, megadni az URL-útválasztási szabályok használata Azure Alkalmazásátjáró utasításokat tartalmazza."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/26/2017
ms.author: gwallace
ms.openlocfilehash: 958049830d6753ec26635f18f8f8b2fabdec0733
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="create-an-application-gateway-using-path-based-routing-with-azure-cli-20"></a>Az Azure CLI 2.0 elérési alapú útválasztás használatával Alkalmazásátjáró létrehozása

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-url-route-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-url-route-arm-ps.md)
> * [Azure CLI 2.0](application-gateway-create-url-route-cli.md)

URL-cím elérési út-alapú útválasztási lehetővé teszi a HTTP-kérések URL-címe alapján útvonalak hozzárendelni. Ellenőrzi, hogy nincs konfigurálva az URL-cím szerepel az Alkalmazásátjáró a háttér-készlet egy útvonalat, és elküldi a hálózati forgalom a meghatározott háttér-készlethez. URL-alapú útválasztási gyakori felhasználási-hoz való különböző háttér-kiszolgálófiók tartalom különböző érkező kérések elosztása.

Az Alkalmazásátjáró URL-alapú útválasztási vezet be egy új szabály típusa. Alkalmazásátjáró két szabály tartozik: alapvető és PathBasedRouting. Alapszintű szabály típusa mellett ciklikus multiplexelés terjesztési PathBasedRouting közben a háttér-készletek ciklikus multiplexelés szolgáltatást biztosít, is a kérelem URL-címének elérési út mintája figyelembe veszi a háttér-készlet kiválasztása során.

## <a name="scenario"></a>Forgatókönyv

A következő példában Application Gateway van szolgáltató contoso.com forgalmát a két háttér-kiszolgálófiók rendelkezik: egy alapértelmezett kiszolgálókészletet és egy kép kiszolgálókészlethez.

Kérések a http://contoso.com/image * legyenek átirányítva kép kiszolgálókészlethez (imagesBackendPool), ha az elérési út mintája nem egyezik, egy alapértelmezett kiszolgálókészletet (appGatewayBackendPool) van kiválasztva.

![URL-cím útvonal](./media/application-gateway-create-url-route-cli/scenario.png)

## <a name="log-in-to-azure"></a>Jelentkezzen be az Azure-ba

Nyissa meg a **Microsoft Azure parancssori**, és jelentkezzen be. 

```azurecli
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

## <a name="add-a-path-based-rule-to-an-existing-application-gateway"></a>Elérési út alapú szabályokat adhat hozzá egy meglévő Alkalmazásátjáró

Alkalmazásátjáró definiált elérésiút-szabály létrehozása

### <a name="create-a-new-back-end-pool"></a>Háttér-készlet létrehozása

Alkalmazásbeállítás átjáró konfigurálása **imagesBackendPool** az elosztott terhelésű hálózati forgalmat a háttér-készletben. Ebben a példában beállítások különböző háttér-tárolókészlet az új háttér-készlet. Mindegyik háttérkészlet saját háttérkészlet-beállításokkal rendelkezhet.  A szabályok a háttérbeli HTTP-beállítások használatával irányítják a forgalmat a háttérkészlet megfelelő tagjaira. Ez határozza meg a protokoll és a forgalom a háttérrendszer a készlet tagjainak való küldés során használt port. A cookie-alapú munkameneteket szintén a háttérbeli HTTP-beállítások határozzák meg.  Ha engedélyezve van, a cookie-alapú munkamenet-affinitás az egyes csomagokhoz tartozó korábbi kéréseknek megfelelő háttérrendszerekre irányítja a forgalmat.

```azurecli-interactive
az network application-gateway address-pool create \
--gateway-name AdatumAppGateway \
--name imagesBackendPool  \
--resource-group myresourcegroup \
--servers 10.0.0.6 10.0.0.7
```

### <a name="create-a-new-front-end-port"></a>Hozzon létre egy új előtér-port

Konfigurálja az előtérbeli portot egy Application Gatewayhez. Az előtérbeli port konfigurációs objektumával egy figyelő meghatározza, melyik porton figyeli a forgalmat az Application Gateway a figyelőn.

```azurecli-interactive
az network application-gateway frontend-port create --port 82 --gateway-name AdatumAppGateway --resource-group myresourcegroup --name port82
```

### <a name="create-a-new-listener"></a>Hozzon létre egy új figyelőt

Konfigurálja a figyelőt. Ebben a lépésben konfiguráljuk a figyelőt a bejövő hálózati forgalmat fogadó nyilvános IP-címhez és porthoz. Az alábbi példában a korábban konfigurált előtér-IP-konfiguráció, előtér-konfiguráció és a protokollt (http vagy https), és konfigurálja a figyelő. Ebben a példában a figyelő a HTTP-forgalom a nyilvános IP-címet, amely korábban a 82-es porton figyel.

```azurecli-interactive
az network application-gateway http-listener create --name imageListener --frontend-ip appGatewayFrontendIP  --frontend-port port82 --resource-group myresourcegroup --gateway-name AdatumAppGateway
```

### <a name="create-the-url-path-map"></a>Az URL-cím elérési út térkép létrehozásához

A háttér-készletek az URL-cím szabály elérési útvonalainak konfigurálása. Ez a lépés konfigurál a relatív elérési utat határozza meg az URL-címet, és melyik háttér-készlet hozzá van rendelve, a bejövő forgalom kezeléséhez közötti leképezést Alkalmazásátjáró segítségével.

> [!IMPORTANT]
> Egyes elérési utakat kell kezdődnie, /, és csak egy "\*" engedélyezett, a végén. Érvényes többek között az /xyz, /xyz* vagy/xyz / *. Az az elérési út matcher táplált a karakterlánc nem tartalmaz sem szöveges az első után "?" vagy "#", és ezek a karakterek nem engedélyezettek. 

Az alábbi példa létrehoz egy szabály a "/ képek / *" Háttér "imagesBackendPool." elérési út adatforgalom Ez a szabály biztosítja, hogy az egyes URL-címek háttérkiszolgálóra továbbítódik. Például http://adatum.com/images/figure1.jpg kerül "imagesBackendPool." Ha az elérési út nem felel meg az előre definiált elérésiút-szabály, a szabály térkép konfiguráció is konfigurálja egy háttér címkészletet. Például http://adatum.com/shoppingcart/test.html kerül pool1, mint az alapértelmezett alkalmazáskészlet páratlan forgalom van definiálva.

```azurecli-interactive
az network application-gateway url-path-map create \
--gateway-name AdatumAppGateway \
--name imagespathmap \
--paths /images/* \
--resource-group myresourcegroup2 \
--address-pool imagesBackendPool \
--default-address-pool appGatewayBackendPool \
--default-http-settings appGatewayBackendHttpSettings \
--http-settings appGatewayBackendHttpSettings \
--rule-name images
```

## <a name="next-steps"></a>Következő lépések

Ha azt szeretné, további információt a Secure Sockets Layer (SSL) kiszervezési, lásd: [konfigurálása az SSL-kiszervezés Alkalmazásátjáró](application-gateway-ssl-cli.md).


[scenario]: ./media/application-gateway-create-url-route-cli/scenario.png
[1]: ./media/application-gateway-create-url-route-cli/figure1.png
[2]: ./media/application-gateway-create-url-route-cli/figure2.png
[3]: ./media/application-gateway-create-url-route-cli/figure3.png
