---
title: "Alkalmazásátjáró használatával URL-cím útválasztási szabályok aaaCreate |} Microsoft Docs"
description: "Ezen a lapon nyújt útmutatást toocreate, Azure Alkalmazásátjáró használatával URL-útválasztási szabályok konfigurálása"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: d141cfbb-320a-4fc9-9125-10001c6fa4cf
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/03/2017
ms.author: gwallace
ms.openlocfilehash: 54fcccc39e48a933576968ce3d8160518c0d0b7e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-gateway-using-path-based-routing"></a>Elérési út alapú útválasztás használatával Alkalmazásátjáró létrehozása

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-url-route-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-url-route-arm-ps.md)
> * [Azure CLI 2.0](application-gateway-create-url-route-cli.md)

URL-cím elérési út-alapú útválasztási lehetővé teszi, hogy Ön tooassociate útvonalak HTTP-kérések hello URL-címe alapján. Ellenőrzi, hogy van-e egy útvonal tooa háttér-készlet szerepel a hello Alkalmazásátjáró hello URL-lel konfigurálja. Majd küldi hello hálózati forgalom toohello definiált háttér-készlet. URL-alapú útválasztási gyakori felhasználási tooload-érkező kérések elosztása a különböző típusú tartalmakat toodifferent háttér-kiszolgálófiók készletek.

Új szabály típusa tooapplication átjáró URL-alapú útválasztási vezet be. Alkalmazásátjáró két szabály tartozik: alapvető és PathBasedRouting. Alapszintű szabálytípus biztosít a ciklikus multiplexelési hello háttér-szolgáltatás a készletbe közben PathBasedRouting továbbá tooround multiplexelés terjesztési, a is hello kérelem URL-címének elérési út mintája figyelembe veszi a hello háttérkészlet kiválasztásakor.

## <a name="scenario"></a>Forgatókönyv

A következő példa hello, Application Gateway van szolgáltató contoso.com forgalmát a két háttér-kiszolgálófiók rendelkezik: videó kiszolgálókészlet és lemezkép kiszolgálókészlethez.

Tooimage (pool1), és hogy http://contoso.com/video * legyenek átirányítva a legyenek átirányítva http://contoso.com/image * kérelmek toovideo kiszolgálókészlet (pool2). Ha hello elérési minták egyike, egy alapértelmezett kiszolgálókészletet (pool1) van kiválasztva.

![URL-cím útvonal](./media/application-gateway-create-url-route-arm-ps/figure1.png)

## <a name="before-you-begin"></a>Előkészületek

1. Webplatform-telepítő hello segítségével hello hello Azure PowerShell-parancsmagok legújabb verzióját telepítse. Töltse le, és telepítse hello legújabb verziót a hello **Windows PowerShell** hello szakasza [letöltési oldalon](https://azure.microsoft.com/downloads/).
2. Az Alkalmazásátjáró hoz létre egy virtuális hálózatot és az alhálózatot. Győződjön meg arról, hogy egyetlen virtuális gépek vagy a felhőben történő alkalmazáshoz hello alhálózat használja. a virtuális hálózati alhálózat hello Alkalmazásátjáró önmagában kell lennie.
3. hello kiszolgálók toohello háttér-készlet toouse hello Alkalmazásátjáró kell lennie, különben a végpontokat hozott létre hello virtuális hálózatban vagy egy nyilvános IP-cím/VIP rendelt hozzá.

## <a name="what-is-required-toocreate-an-application-gateway"></a>Mi az a szükséges toocreate olyan átjárót?

* **Háttér-kiszolgálófiók készlet:** hello hello háttér-kiszolgálók IP-címek listáját. hello IP-címek felsorolt toohello virtuális hálózati alhálózat vagy kell tartoznia, vagy egy nyilvános IP-cím/VIP kell lennie.
* **Háttér-kiszolgálókészlet beállításai:** Minden készletnek vannak beállításai, például port, protokoll vagy cookie-alapú affinitás. Ezek a beállítások esetén tooa kapcsolt verem és a hello készlet alkalmazott tooall-kiszolgálók.
* **Előtér-port:** Ez a port nem hello nyilvános portot, amelyet a hello Alkalmazásátjáró meg van nyitva. Forgalom találatok ezt a portot, és lekérdezi átirányítja tooone hello háttér-kiszolgálók.
* **Figyelő:** hello figyelő rendelkezik egy előtér-portot, a protokollt (Http vagy Https, ezek az értékek kis-és nagybetűket), és hello SSL tanúsítvány neve (ha az SSL beállításának-kiszervezés).
* **Szabály:** hello szabály hello figyelő, hello háttér-kiszolgálófiók alkalmazáskészlet van kötve, és azt határozza meg, mely háttér-kiszolgálófiók készlet hello forgalom irányított toowhen találatok száma a egy adott figyelő.

## <a name="create-an-application-gateway"></a>Application Gateway létrehozása

hello közötti Azure klasszikus és az Azure Resource Manager használatával különbség hello rendelés hello Alkalmazásátjáró és hello elemeket toobe konfigurálva kell létrehozni.

A Resource Manager az összes olyan átjárót elemeit egyedien vannak konfigurálva, majd tegye együtt toocreate hello alkalmazás átjáró-erőforráshoz.

Az alábbiakban a szükséges toocreate Alkalmazásátjáró hello lépéseket:

1. Egy erőforráscsoport létrehozása a Resource Manager számára.
2. Hozzon létre egy virtuális hálózati alhálózat és hello alkalmazás átjáró nyilvános IP-cím.
3. Egy Application Gateway konfigurációs objektum létrehozása.
4. Egy Application Gateway erőforrás létrehozása.

## <a name="create-a-resource-group-for-resource-manager"></a>Erőforráscsoport létrehozása a Resource Managerhez

Győződjön meg arról, hogy azok be hello Azure PowerShell legújabb verzióját. További információ: [A Windows PowerShell használata a Resource Managerrel](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>1. lépés

Jelentkezzen be tooAzure

```powershell
Login-AzureRmAccount
```

A hitelesítő adataival felszólító tooauthenticate áll.<BR>

### <a name="step-2"></a>2. lépés

Hello előfizetések hello fiók ellenőrzése.

```powershell
Get-AzureRmSubscription
```

### <a name="step-3"></a>3. lépés

Válassza ki, amely az Azure-előfizetések toouse. <BR>

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="step-4"></a>4. lépés

Hozzon létre egy erőforráscsoportot (hagyja ki ezt a lépést, ha egy meglévő erőforráscsoportot használ).

```powershell
$resourceGroup = New-AzureRmResourceGroup -Name appgw-RG -Location "West US"
```

Másik lehetőségként az Alkalmazásátjáró erőforráscsoport címkéket is létrehozhat:

```powershell
$resourceGroup = New-AzureRmResourceGroup -Name appgw-RG -Location "West US" -Tags @{Name = "testtag"; Value = "Application Gateway URL routing"} 
```

Az Azure Resource Manager megköveteli, hogy minden erőforráscsoport megadjon egy helyet. Ez az erőforráscsoport hello alapértelmezett helyeként szolgál az erőforráscsoport erőforrások. Győződjön meg arról, hogy az összes parancsok toocreate egy alkalmazás átjáró használata hello ugyanabban az erőforráscsoportban.

A hello a fenti példában létrehozott egy erőforráscsoport neve "appgw-RG" és "Velünk nyugati" helyen.

> [!NOTE]
> Ha egy egyéni mintavételt tooconfigure az Alkalmazásátjáró van szüksége, tekintse meg [PowerShell használatával hozzon létre olyan átjárót egyéni mintavételt](application-gateway-create-probe-ps.md). További információért lásd: [egyéni minták és állapotfigyelés](application-gateway-probe-overview.md).
> 
> 

## <a name="create-a-virtual-network-and-a-subnet-for-hello-application-gateway"></a>Hozzon létre egy virtuális hálózatot és hello Alkalmazásátjáró alhálózatot

a következő példa azt mutatja meg hogyan hello toocreate erőforrás-kezelő használatával egy virtuális hálózatot. Ez a példa egy VNETET az Alkalmazásátjáró hello hoz létre. Alkalmazásátjáró saját alhálózatba hello Alkalmazásátjáró készült hello alhálózati értéke kisebb a VNET címterület hello ezért van szükség. Ez lehetővé teszi a további erőforrások, többek között, de nem korlátozott tooweb kiszolgálók toobe konfigurált hello azonos virtuális hálózaton.

### <a name="step-1"></a>1. lépés

Rendelje hozzá a hello cím tartomány 10.0.0.0/24 toohello alhálózati használt változó toobe toocreate egy virtuális hálózatot.  Hello alhálózati konfigurációs objektum hello alkalmazás átjárón, amely a következő példában hello létrejön.

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24
```

### <a name="step-2"></a>2. lépés

Hozzon létre egy virtuális hálózatot nevű **appgwvnet** erőforráscsoportban **appgw-rg** hello USA nyugati régiójában hello előtag 10.0.0.0/16 használata a 10.0.0.0/24 alhálózat. Ezzel befejezte a hello virtuális Hálózatot egyetlen alhálózattal a hello Alkalmazásátjáró tooreside hello konfigurációját.

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet
```

### <a name="step-3"></a>3. lépés

Rendelje hozzá a hello alhálózati változója hello lépések, ez toohello átadása `New-AzureRMApplicationGateway` parancsmag egy későbbi lépésben.

```powershell
$subnet=$vnet.Subnets[0]
```

## <a name="create-a-public-ip-address-for-hello-front-end-configuration"></a>A nyilvános IP-cím hello előtér-konfiguráció létrehozása

Hozzon létre egy nyilvános IP-erőforrás **publicIP01** erőforráscsoportban **appgw-rg** hello USA nyugati régiójában. Alkalmazásátjáró használhatja egy nyilvános IP-címet, a belső IP-címet vagy mindkét tooreceive kérelmek a(z) terheléselosztást.  Ebben a példában csak nyilvános IP-címet használunk. A következő példa hello DNS-neve nem úgy van konfigurálva, a hello nyilvános IP-cím létrehozásához.  Az Application Gateway nem támogatja az egyéni DNS-neveket a nyilvános IP-címeken.  Ha egy egyéni nevet hello nyilvános végpontot, egy CNAME rekordot kell létrehozni a toopoint automatikusan generált toohello DNS-név hello nyilvános IP-cím.

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

IP-cím hozzá van rendelve toohello Alkalmazásátjáró hello szolgáltatás elindulásakor.

## <a name="create-application-gateway-configuration"></a>Alkalmazás átjáró-konfiguráció létrehozása

Az összes konfigurációs elemek hello Alkalmazásátjáró létrehozása előtt kell beállítani. hello lépések hello konfigurációs elemek létrehozását, amelyek szükségesek az alkalmazás átjáró-erőforráshoz.

### <a name="step-1"></a>1. lépés

Hozzon létre egy **gatewayIP01** nevű Application Gateway IP-konfigurációt. Alkalmazásátjáró indításakor szerzi be hello alhálózati konfigurált IP-címeit, és útvonal-hello háttér IP-készletet a hálózati forgalom toohello IP-címek. Ne feledje, hogy minden példány egy IP-címet vesz fel.

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet
```

### <a name="step-2"></a>2. lépés

Konfigurálja hello háttér IP-címkészletet nevű **pool01** és **pool2** az IP-címekkel rendelkező **pool1** és **pool2**. A következő IP-címek azokat hello IP-címeket hello erőforrások üzemeltető hello webes alkalmazás toobe hello Alkalmazásátjáró védi. Ezek háttér készlettagra megfelelő mintavételt által érvényesített toobe, hogy-e mintavételt alapszintű vagy egyéni mintavételt.  Majd továbbítódik toothem, amikor hello Alkalmazásátjáró kérelmek kerülnek. Háttérkészlet hello Alkalmazásátjáró, ami azt jelenti, egy háttér címkészletet használható több olyan webes alkalmazásokhoz, amelyek megtalálhatók a hello azonos belül több szabály által használható állomás.

```powershell
$pool1 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221, 134.170.185.50

$pool2 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool02 -BackendIPAddresses 134.170.186.47, 134.170.189.222, 134.170.186.51
```

Ebben a példában a rendszer két háttér-készletek tooroute hálózati forgalom hello URL-címe alapján. Az egyik készlet a „/video” URL-útvonalról, a másik az „/image” útvonalról fogadja a forgalmat. Cserélje le a fenti IP-címek tooadd saját alkalmazás IP-cím végpontok hello. 

### <a name="step-3"></a>3. lépés

Alkalmazásbeállítás átjáró konfigurálása **poolsetting01** és **poolsetting02** hello terhelésű hálózati forgalmára hello háttér-készlet. Ebben a példában ehhez beállításokat lehet megadni másik háttér címkészletet hello háttér-készletek. Mindegyik háttérkészlet saját háttérkészlet-beállításokkal rendelkezhet.  Szabályok tooroute forgalom toohello megfelelő háttér a készlet tagjainak háttér HTTP-beállításokat használja. Ez határozza meg, hello protokoll és toohello háttér a készlet tagjainak forgalom küldéséhez használt port. A munkamenetek cookie-alapú is a backendhttpsettings osztályhoz hello határozza meg.  Ha engedélyezve van, a munkamenet cookie-alapú kapcsolat küld-e a forgalom toohello azonos háttér, egyes csomagok előző kérelmekben.

```powershell
$poolSetting01 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled -RequestTimeout 120

$poolSetting02 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting02" -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 240
```

### <a name="step-4"></a>4. lépés

Nyilvános IP-végponton hello előtér-IP konfigurálja. hello előtér-IP-konfigurációs objektum használja egy figyelő toorelate hello kifelé irányuló hello figyelő IP-címmel.

```powershell
$fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip
```

### <a name="step-5"></a>5. lépés

Hello előtér-port konfigurálása az Alkalmazásátjáró. hello előtér-port konfigurációs objektum használja egy figyelő toodefine melyik portot hello Alkalmazásátjáró figyeli a hello figyelő-forgalmat.

```powershell
$fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "fep01" -Port 80
```

### <a name="step-6"></a>6. lépés

Hello figyelő konfigurálása. Ez a lépés konfigurál hello figyelő hello nyilvános IP-cím és port használt tooreceive bejövő hálózati forgalom. a következő példa hello hello korábban konfigurált előtér-IP-konfiguráció, előtér-konfiguráció és a protokollt (http vagy https), és konfigurálja a hello figyelő. Ebben a példában hello figyelő tooHTTP forgalom hello nyilvános IP-címet, amely korábban a 80-as porton figyel.

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name "listener01" -Protocol Http -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01
```

### <a name="step-7"></a>7. lépés

URL-cím szabály útvonalak hello háttér-címkészletek konfigurálása. Ebben a lépésben hello relatív elérési út alkalmazás-átjáró által használt konfigurálja, és határozza meg a hello leképezése hello URL-címe és hello háttér-készlet, amely hozzá van rendelve toohandle hello bejövő forgalmat.

> [!IMPORTANT]
> Egyes elérési utakat kell kezdődnie / és hello egyetlen hely, ahol a "\*" engedélyezett, a hello végén. Érvényes többek között az /xyz, /xyz* vagy/xyz / *. hello toohello elérési matcher táplált karakterlánc nem tartalmaz sem szöveges hello után először "?" vagy "#", és ezek a karakterek nem engedélyezettek. 

hello alábbi példa létrehoz két szabályt: egy a "/ kép /" elérési út útválasztási forgalom tooback-end "pool1" és a "/ videó /" elérési útválasztási forgalom tooback-end "pool2." Ezek a szabályok arról, hogy az egyes URL-címek forgalmát irányított toohello háttér. Például http://contoso.com/image/figure1.jpg toopool1 kerül, és http://contoso.com/video/example.mp4 toopool2 kerül.

```powershell
$imagePathRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "pathrule1" -Paths "/image/*" -BackendAddressPool $pool1 -BackendHttpSettings $poolSetting01

$videoPathRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "pathrule2" -Paths "/video/*" -BackendAddressPool $pool2 -BackendHttpSettings $poolSetting02
```

Hello elérési út bármely hello előre definiált elérésiút-szabály nem megfelelő, ha a szabály térkép konfiguráció hello is konfigurálja egy háttér címkészletet. Például a http://contoso.com/shoppingcart/test.html toopool1 kerül, mint hello alapértelmezett alkalmazáskészlet páratlan forgalom van definiálva.

```powershell
$urlPathMap = New-AzureRmApplicationGatewayUrlPathMapConfig -Name "urlpathmap" -PathRules $videoPathRule, $imagePathRule -DefaultBackendAddressPool $pool1 -DefaultBackendHttpSettings $poolSetting02
```

### <a name="step-8"></a>8. lépés

Hozzon létre egy szabályt beállítást. Ez a lépés konfigurál hello alkalmazás átjáró toouse URL-cím elérési út-alapú útválasztási. Hello `$urlPathMap` definiált változó hello a korábbi. lépés: most használt toocreate hello elérési alapuló szabály. Ebben a lépésben azt társítsa hello szabály egy figyelő és a korábban létrehozott hello URL-cím elérési útjának leképezése.

```powershell
$rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule1" -RuleType PathBasedRouting -HttpListener $listener -UrlPathMap $urlPathMap
```

### <a name="step-9"></a>9. lépés

Példányok és hello Alkalmazásátjáró mérete hello számának konfigurálása.

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name "Standard_Small" -Tier Standard -Capacity 2
```

## <a name="create-application-gateway"></a>Alkalmazásátjáró létrehozása

Hozzon létre egy alkalmazást az előző lépésekben hello összes konfigurációs objektumok.

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-RG -Location "West US" -BackendAddressPools $pool1,$pool2 -BackendHttpSettingsCollection $poolSetting01, $poolSetting02 -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener -UrlPathMaps $urlPathMap -RequestRoutingRules $rule01 -Sku $sku
```

## <a name="get-application-gateway-dns-name"></a>Az Application Gateway DNS-nevének beszerzése

Hello átjáró létrehozása után hello következő lépésre tooconfigure hello előtér-kommunikációhoz. Nyilvános IP-cím esetén az Application Gateway használatához dinamikusan hozzárendelt DNS-névre van szükség, amely nem valódi név. tooensure a végfelhasználók is találati hello Alkalmazásátjáró, egy olyan CNAME rekordot is használt toopoint toohello nyilvános végpontot hello Alkalmazásátjáró. [Egyéni tartománynév konfigurálása az Azure-ban](../cloud-services/cloud-services-custom-domain-name-portal.md). tooconfigure hello előtérbeli IP-CNAME rekord részleteit hello Alkalmazásátjáró és a társított IP-/ DNS-nevét, hello PublicIPAddress elem csatolt toohello Alkalmazásátjáró beolvasása. hello alkalmazás átjáró DNS-név használt toocreate egy olyan CNAME rekordot kell lennie. A-rekordok hello használata nem javasolt, mert hello VIP módosíthatja az Alkalmazásátjáró újra kell indítani.

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -Name publicIP01
```

```
Name                     : publicIP01
ResourceGroupName        : appgw-RG
Location                 : westus
Id                       : /subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/publicIPAddresses/publicIP01
Etag                     : W/"00000d5b-54ed-4907-bae8-99bd5766d0e5"
ResourceGuid             : 00000000-0000-0000-0000-000000000000
ProvisioningState        : Succeeded
Tags                     : 
PublicIpAllocationMethod : Dynamic
IpAddress                : xx.xx.xxx.xx
PublicIpAddressVersion   : IPv4
IdleTimeoutInMinutes     : 4
IpConfiguration          : {
                                "Id": "/subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/applicationGateways/appgwtest/frontendIP
                            Configurations/frontend1"
                            }
DnsSettings              : {
                                "Fqdn": "00000000-0000-xxxx-xxxx-xxxxxxxxxxxx.cloudapp.net"
                            }
```

## <a name="next-steps"></a>Következő lépések

Ha azt szeretné, hogy Secure Sockets Layer (SSL)-kiszervezés toolearn, [konfigurálása az SSL-kiszervezés Alkalmazásátjáró](application-gateway-ssl-arm.md).

