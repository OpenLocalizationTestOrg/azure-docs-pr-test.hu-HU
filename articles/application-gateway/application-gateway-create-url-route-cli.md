---
title: "Alkalmazásátjáró használatával URL-cím útválasztási szabályok - aaaCreate Azure CLI 2.0 |} Microsoft Docs"
description: "Ezen a lapon nyújt útmutatást toocreate, Azure Alkalmazásátjáró használatával URL-útválasztási szabályok konfigurálása"
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
ms.openlocfilehash: 335b52be258945e1172eb0252b732e0e6ecb2ef0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-gateway-using-path-based-routing-with-azure-cli-20"></a>Az Azure CLI 2.0 elérési alapú útválasztás használatával Alkalmazásátjáró létrehozása

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-url-route-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-url-route-arm-ps.md)
> * [Azure CLI 2.0](application-gateway-create-url-route-cli.md)

URL-cím elérési út-alapú útválasztási lehetővé teszi, hogy Ön tooassociate útvonalak HTTP-kérések hello URL-címe alapján. Ellenőrzi, hogy az útvonal tooa háttér-készlet hello URL-cím szerepel a hello Alkalmazásátjáró konfigurálva van, és elküldi a hello hálózati forgalom toohello definiált háttér-készlet. URL-alapú útválasztási gyakori felhasználási tooload-érkező kérések elosztása a különböző típusú tartalmakat toodifferent háttér-kiszolgálófiók készletek.

Új szabály típusa tooapplication átjáró URL-alapú útválasztási vezet be. Alkalmazásátjáró két szabály tartozik: alapvető és PathBasedRouting. Alapszintű szabálytípus biztosít a ciklikus multiplexelés hello háttér-szolgáltatás a készletbe közben PathBasedRouting továbbá tooround multiplexelés terjesztési, a is hello kérelem URL-címének elérési út mintája figyelembe veszi a hello háttér-készlet kiválasztásakor.

## <a name="scenario"></a>Forgatókönyv

A következő példa hello, Application Gateway van szolgáltató contoso.com forgalmát a két háttér-kiszolgálófiók rendelkezik: egy alapértelmezett kiszolgálókészletet és egy kép kiszolgálókészlethez.

Kérések a http://contoso.com/image * irányított tooimage kiszolgálókészlet (imagesBackendPool), ha hello elérési út mintája nem felel meg, egy alapértelmezett kiszolgálókészletet (appGatewayBackendPool) van kiválasztva.

![URL-cím útvonal](./media/application-gateway-create-url-route-cli/scenario.png)

## <a name="log-in-tooazure"></a>Jelentkezzen be tooAzure

Nyissa meg hello **Microsoft Azure parancssori**, és jelentkezzen be. 

```azurecli
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

## <a name="add-a-path-based-rule-tooan-existing-application-gateway"></a>Adja hozzá az elérési út alapú szabály tooan meglévő Alkalmazásátjáró

Alkalmazásátjáró definiált elérésiút-szabály létrehozása

### <a name="create-a-new-back-end-pool"></a>Háttér-készlet létrehozása

Alkalmazásbeállítás átjáró konfigurálása **imagesBackendPool** hello terhelésű hálózati forgalmára hello háttér-készlet. Ebben a példában beállítások másik háttér címkészletet hello új háttér-készlet. Mindegyik háttérkészlet saját háttérkészlet-beállításokkal rendelkezhet.  Szabályok tooroute forgalom toohello megfelelő háttér a készlet tagjainak háttér HTTP-beállításokat használja. Ez határozza meg, hello protokoll és toohello háttér a készlet tagjainak forgalom küldéséhez használt port. A munkamenetek cookie-alapú is a backendhttpsettings osztályhoz hello határozza meg.  Ha engedélyezve van, a munkamenet cookie-alapú kapcsolat küld-e a forgalom toohello azonos háttér, egyes csomagok előző kérelmekben.

```azurecli-interactive
az network application-gateway address-pool create \
--gateway-name AdatumAppGateway \
--name imagesBackendPool  \
--resource-group myresourcegroup \
--servers 10.0.0.6 10.0.0.7
```

### <a name="create-a-new-front-end-port"></a>Hozzon létre egy új előtér-port

Hello előtér-port konfigurálása az Alkalmazásátjáró. hello előtér-port konfigurációs objektum használja egy figyelő toodefine melyik portot hello Alkalmazásátjáró figyeli a hello figyelő-forgalmat.

```azurecli-interactive
az network application-gateway frontend-port create --port 82 --gateway-name AdatumAppGateway --resource-group myresourcegroup --name port82
```

### <a name="create-a-new-listener"></a>Hozzon létre egy új figyelőt

Hello figyelő konfigurálása. Ez a lépés konfigurál hello figyelő hello nyilvános IP-cím és port használt tooreceive bejövő hálózati forgalom. a következő példa hello hello korábban konfigurált előtér-IP-konfiguráció, előtér-konfiguráció és a protokollt (http vagy https), és konfigurálja a hello figyelő. Ebben a példában hello figyelő tooHTTP adatforgalmat hello nyilvános IP-címet, amely korábban a 82-es porton figyel.

```azurecli-interactive
az network application-gateway http-listener create --name imageListener --frontend-ip appGatewayFrontendIP  --frontend-port port82 --resource-group myresourcegroup --gateway-name AdatumAppGateway
```

### <a name="create-hello-url-path-map"></a>Hello URL-cím elérési út térkép létrehozásához

URL-cím szabály útvonalak hello háttér-címkészletek konfigurálása. Ez a lépés konfigurál hello relatív elérési út átjáró toodefine hello alkalmazástársításhoz közötti URL-címet, és melyik háttér-készlet kap toohandle hello bejövő forgalom által használt.

> [!IMPORTANT]
> Egyes elérési utakat kell kezdődnie / és hello egyetlen hely, ahol a "\*" engedélyezett, a hello végén. Érvényes többek között az /xyz, /xyz* vagy/xyz / *. hello toohello elérési matcher táplált karakterlánc nem tartalmaz sem szöveges hello után először "?" vagy "#", és ezek a karakterek nem engedélyezettek. 

hello alábbi példa létrehoz egy szabály a "/ képek / *" elemzéshez forgalom tooback-end "imagesBackendPool." Ez a szabály biztosítja, hogy az egyes URL-címek forgalom irányított toohello háttér. Például http://adatum.com/images/figure1.jpg túl kerül "imagesBackendPool." Hello elérési út bármely hello előre definiált elérésiút-szabály nem megfelelő, ha a szabály térkép konfiguráció hello is konfigurálja egy háttér címkészletet. Például a http://adatum.com/shoppingcart/test.html toopool1 kerül, mint hello alapértelmezett alkalmazáskészlet páratlan forgalom van definiálva.

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

Ha azt szeretné, hogy Secure Sockets Layer (SSL) kiszervezési kapcsolatos toolearn, [konfigurálása az SSL-kiszervezés Alkalmazásátjáró](application-gateway-ssl-cli.md).


[scenario]: ./media/application-gateway-create-url-route-cli/scenario.png
[1]: ./media/application-gateway-create-url-route-cli/figure1.png
[2]: ./media/application-gateway-create-url-route-cli/figure2.png
[3]: ./media/application-gateway-create-url-route-cli/figure3.png
