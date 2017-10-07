---
title: "több hely üzemeltetéséhez Alkalmazásátjáró aaaCreate |} Microsoft Docs"
description: "Ezen a lapon nyújt útmutatást toocreate, konfiguráljon egy Azure-alkalmazásokban átjárót a hello több webalkalmazás üzemeltetéséhez ugyanahhoz az átjáróhoz."
documentationcenter: na
services: application-gateway
author: amsriva
manager: rossort
editor: amsriva
ms.assetid: b107d647-c9be-499f-8b55-809c4310c783
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/12/2016
ms.author: amsriva
ms.openlocfilehash: bad9a76be0a73a7026a770630fa7156f6e5940c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-gateway-for-hosting-multiple-web-applications"></a>Hozzon létre egy alkalmazás több webalkalmazás üzemeltetéséhez

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-multisite-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-multisite-azureresourcemanager-powershell.md)

Több helyet üzemeltető lehetővé teszi egy webalkalmazás több toodeploy a hello ugyanazt az Alkalmazásátjáró. Az állomásfejlécnek hello bejövő HTTP-kérelmek, mely figyelő kapja forgalom toodetermine jelenlétére támaszkodik. hello figyelő majd arra utasítja a forgalom tooappropriate háttérkészlet be hello átjáró hello szabályok meghatározását. Az SSL engedélyezve van a webalkalmazások Alkalmazásátjáró hello kiszolgálónév jelzése (SNI) bővítmény toochoose hello megfelelő figyelő hello webes forgalom támaszkodik. Több hely üzemeltetéséhez használatos tooload-érkező kérések elosztása a különböző webes tartományok toodifferent háttér-kiszolgálófiók készletek. Hasonlóképpen a legfelső szintű tartománynak is futhat a hello több altartományt hello ugyanazt az Alkalmazásátjáró.

## <a name="scenario"></a>Forgatókönyv

A következő példa hello, Alkalmazásátjáró van kiszolgáló a contoso.com és fabrikam.com forgalom a két háttér-kiszolgálófiók rendelkezik: contoso kiszolgálókészlet és a fabrikam kiszolgálókészlethez. Hasonló lehet, például app.contoso.com és blog.contoso.com használt toohost altartományokat.

![imageURLroute](./media/application-gateway-create-multisite-azureresourcemanager-powershell/multisite.png)

## <a name="before-you-begin"></a>Előkészületek

1. Webplatform-telepítő hello segítségével hello hello Azure PowerShell-parancsmagok legújabb verzióját telepítse. Töltse le, és telepítse hello legújabb verziót a hello **Windows PowerShell** hello szakasza [letöltési oldalon](https://azure.microsoft.com/downloads/).
2. hello kiszolgálók toohello háttér-készlet toouse hello Alkalmazásátjáró kell lennie, különben a végpontokat hozott létre vagy hello virtuális hálózatban egy külön alhálózatot, vagy egy nyilvános IP-cím/VIP rendelt hozzá.

## <a name="requirements"></a>Követelmények

* **Háttér-kiszolgálófiók készlet:** hello hello háttér-kiszolgálók IP-címek listáját. hello IP-címek felsorolt toohello virtuális hálózati alhálózat vagy kell tartoznia, vagy egy nyilvános IP-cím/VIP kell lennie. Teljes Tartománynevét is használható.
* **Háttér-kiszolgálókészlet beállításai:** Minden készletnek vannak beállításai, például port, protokoll vagy cookie-alapú affinitás. Ezek a beállítások esetén tooa kapcsolt verem és a hello készlet alkalmazott tooall-kiszolgálók.
* **Előtér-port:** Ez a port nem hello nyilvános portot, amelyet a hello Alkalmazásátjáró meg van nyitva. Forgalom találatok ezt a portot, és lekérdezi átirányítja tooone hello háttér-kiszolgálók.
* **Figyelő:** hello figyelő rendelkezik egy előtér-portot, a protokollt (Http vagy Https, ezek az értékek kis-és nagybetűket), és hello SSL tanúsítvány neve (ha az SSL beállításának-kiszervezés). A többhelyes engedélyezett alkalmazásátjárót, állomásnév és SNI mutatók is bekerülnek.
* **Szabály:** hello szabály van kötve hello figyelő, hello háttér-kiszolgálófiók vermet, és azt határozza meg, mely háttér-kiszolgálófiók készlet hello forgalom irányított toowhen találatok száma a egy adott figyelő. Szabályok feldolgozása hello sorrendben, és a forgalom hello első egyező szabály függetlenül sajátlagossága figyelembe vétele keresztül jutnak. Például ha a szabályt egy alapszintű figyelő és az azonos port, hello szabály hello többhelyes figyelő mindkét használó szabály hello többhelyes figyelő szerepelnie kell a hello szabály előtt hello alapvető figyelőjével ahhoz, hogy hello többhelyes szabály toofunction várt.

## <a name="create-an-application-gateway"></a>Application Gateway létrehozása

hello az alábbiakban hello lépéseket toocreate Alkalmazásátjáró szükséges:

1. Egy erőforráscsoport létrehozása a Resource Manager számára.
2. Hozzon létre egy virtuális hálózatot, alhálózatok és hello alkalmazás átjáró nyilvános IP-cím.
3. Egy Application Gateway konfigurációs objektum létrehozása.
4. Egy Application Gateway erőforrás létrehozása.

## <a name="create-a-resource-group-for-resource-manager"></a>Erőforráscsoport létrehozása a Resource Managerhez

Győződjön meg arról, hogy azok be hello Azure PowerShell legújabb verzióját. További információt [Windows PowerShell használata a Resource Manager](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>1. lépés

Jelentkezzen be tooAzure

```powershell
Login-AzureRmAccount
```
A hitelesítő adataival felszólító tooauthenticate áll.

### <a name="step-2"></a>2. lépés

Hello előfizetések hello fiók ellenőrzése.

```powershell
Get-AzureRmSubscription
```

### <a name="step-3"></a>3. lépés

Válassza ki, amely az Azure-előfizetések toouse.

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="step-4"></a>4. lépés

Hozzon létre egy erőforráscsoportot (hagyja ki ezt a lépést, ha egy meglévő erőforráscsoportot használ).

```powershell
New-AzureRmResourceGroup -Name appgw-RG -location "West US"
```

Másik lehetőségként az Alkalmazásátjáró erőforráscsoport címkéket is létrehozhat:

```powershell
$resourceGroup = New-AzureRmResourceGroup -Name appgw-RG -Location "West US" -Tags @{Name = "testtag"; Value = "Application Gateway multiple site"}
```

Az Azure Resource Manager megköveteli, hogy minden erőforráscsoport megadjon egy helyet. Ezen a helyen az erőforráscsoport erőforrások lesz hello alapértelmezett helye. Győződjön meg arról, hogy az összes parancsok toocreate egy alkalmazás átjáró használata hello ugyanabban az erőforráscsoportban.

A hello a fenti példában létrehozott nevű erőforráscsoport **appgw-RG** a hellyel rendelkező **USA nyugati régiója**.

> [!NOTE]
> Ha egy egyéni mintavételt tooconfigure az Alkalmazásátjáró van szüksége, tekintse meg [PowerShell használatával hozzon létre olyan átjárót egyéni mintavételt](application-gateway-create-probe-ps.md). Látogasson el [egyéni mintavételt és az állapotfigyelés](application-gateway-probe-overview.md) további információt.

## <a name="create-a-virtual-network-and-subnets"></a>Hozzon létre egy virtuális hálózatot és alhálózatok

a következő példa azt mutatja meg hogyan hello toocreate erőforrás-kezelő használatával egy virtuális hálózatot. Két alhálózat ebben a lépésben jönnek létre. hello első alhálózat van az hello Alkalmazásátjáró magát. Alkalmazásátjáró saját alhálózati toohold a példányok van szükség. Csak más alkalmazásátjárót alhálózat is telepíthető. hello második alhálózat használt toohold hello alkalmazás háttérkiszolgálók.

### <a name="step-1"></a>1. lépés

Rendelje hozzá a hello cím tartomány 10.0.0.0/24 toohello alhálózati változó toobe használt toohold hello Alkalmazásátjáró.

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name appgatewaysubnet -AddressPrefix 10.0.0.0/24
```
### <a name="step-2"></a>2. lépés

Rendelje hozzá a hello cím tartomány 10.0.1.0/24 toohello Alhalozat_2 változó toobe hello háttérkészletek használatos.

```powershell
$subnet2 = New-AzureRmVirtualNetworkSubnetConfig -Name backendsubnet -AddressPrefix 10.0.1.0/24
```

### <a name="step-3"></a>3. lépés

Hozzon létre egy virtuális hálózatot nevű **appgwvnet** erőforráscsoportban **appgw-rg** hello USA nyugati régiójában hello előtag 10.0.0.0/16 használata a 10.0.0.0/24 alhálózat, és 10.0.1.0/24.

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet,$subnet2
```

### <a name="step-4"></a>4. lépés

Rendeljen hozzá egy alhálózati változóval hello további lépéseket, ami olyan átjárót hoz létre.

```powershell
$appgatewaysubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name appgatewaysubnet -VirtualNetwork $vnet
$backendsubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name backendsubnet -VirtualNetwork $vnet
```

## <a name="create-a-public-ip-address-for-hello-front-end-configuration"></a>A nyilvános IP-cím hello előtér-konfiguráció létrehozása

Hozzon létre egy nyilvános IP-erőforrás **publicIP01** erőforráscsoportban **appgw-rg** hello USA nyugati régiójában.

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

IP-cím hozzá van rendelve toohello Alkalmazásátjáró hello szolgáltatás elindulásakor.

## <a name="create-application-gateway-configuration"></a>Alkalmazás átjáró-konfiguráció létrehozása

Összes konfigurációs elemek mentése tooset hello Alkalmazásátjáró létrehozása előtt van. hello lépések hello konfigurációs elemek létrehozását, amelyek szükségesek az alkalmazás átjáró-erőforráshoz.

### <a name="step-1"></a>1. lépés

Hozzon létre egy **gatewayIP01** nevű Application Gateway IP-konfigurációt. Alkalmazásátjáró indításakor szerzi be hello alhálózati konfigurált IP-címeit, és útvonal-hello háttér IP-készletet a hálózati forgalom toohello IP-címek. Ne feledje, hogy minden példány egy IP-címet vesz fel.

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $appgatewaysubnet
```

### <a name="step-2"></a>2. lépés

Konfigurálja hello háttér IP-címkészletet nevű **pool01** és **pool2** IP-címekkel rendelkező **134.170.185.46**, **134.170.188.221**, **134.170.185.50** a **pool1** és **134.170.186.46**, **134.170.189.221**, **134.170.186.50**  a **pool2**.

```powershell
$pool1 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 10.0.1.100, 10.0.1.101, 10.0.1.102
$pool2 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool02 -BackendIPAddresses 10.0.1.103, 10.0.1.104, 10.0.1.105
```

Ebben a példában a rendszer két háttér-készletek tooroute hálózati forgalom hello kért hely alapján. Egy készlet a "contoso.com" hely származó forgalmat megkapja és egyéb készlet hely "fabrikam.com" származó forgalmat megkapja. IP-címek tooadd saját alkalmazás IP-cím végpontok megelőző tooreplace hello rendelkezik. Belső IP-címek, helyett is használhatja nyilvános IP-címek, FQDN vagy a virtuális gép hálózati háttér-példányok. PowerShell használt teljes tartománynevek helyett IP-címek toospecify "-BackendFQDNs" paraméter.

### <a name="step-3"></a>3. lépés

Alkalmazásbeállítás átjáró konfigurálása **poolsetting01** és **poolsetting02** hello terhelésű hálózati forgalmára hello háttér-készlet. Ebben a példában ehhez beállításokat lehet megadni másik háttér címkészletet hello háttér-készletek. Mindegyik háttérkészlet saját háttérkészlet-beállításokkal rendelkezhet.

```powershell
$poolSetting01 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled -RequestTimeout 120
$poolSetting02 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting02" -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 240
```

### <a name="step-4"></a>4. lépés

Nyilvános IP-végponton hello előtér-IP konfigurálja.

```powershell
$fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip
```

### <a name="step-5"></a>5. lépés

Hello előtér-port konfigurálása az Alkalmazásátjáró.

```powershell
$fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "fep01" -Port 443
```

### <a name="step-6"></a>6. lépés

Két SSL-tanúsítványok konfigurálása ebben a példában toosupport fogjuk hello két webhelyekhez. Egy tanúsítvány a contoso.com forgalmat pedig más hello fabrikam.com forgalom. Ezeket a tanúsítványokat a webhelyeknek a tanúsítványokat kiadó hitelesítésszolgáltató kell lennie. Önaláírt tanúsítványok támogatottak, de nem javasolt éles forgalom.

```powershell
$cert01 = New-AzureRmApplicationGatewaySslCertificate -Name contosocert -CertificateFile <file path> -Password <password>
$cert02 = New-AzureRmApplicationGatewaySslCertificate -Name fabrikamcert -CertificateFile <file path> -Password <password>
```

### <a name="step-7"></a>7. lépés

Ebben a példában két figyelői hello két webhelyek konfigurálása Ez a lépés konfigurál hello figyelői nyilvános IP-cím, port és gazdagép használt tooreceive bejövő forgalmat. Állomásnév paraméter több webhely támogatásához szükséges, és legyen beállítva toohello megfelelő webhely mely hello a forgalom érkezik. RequireServerNameIndication paraméter tootrue az webhely SSL-t, a több gazdagépen forgatókönyvek kell támogatnia kell állítani. Ha az SSL támogatására szükség, akkor is szükség toospecify hello SSL tanúsítvány, amely az adott webes alkalmazáshoz használt toosecure forgalmat. FrontendIPConfiguration FrontendPort és állomásnév hello kombinációjának egyedi tooa figyelő kell lennie. Minden egyes figyelő több tanúsítvány is támogatja.

```powershell
$listener01 = New-AzureRmApplicationGatewayHttpListener -Name "listener01" -Protocol Https -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -HostName "contoso11.com" -RequireServerNameIndication true  -SslCertificate $cert01
$listener02 = New-AzureRmApplicationGatewayHttpListener -Name "listener02" -Protocol Https -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -HostName "fabrikam11.com" -RequireServerNameIndication true -SslCertificate $cert02
```

### <a name="step-8"></a>8. lépés

Hozzon létre beállítása hello két webes alkalmazások ebben a példában két szabályt. Egy szabály kötelékek együtt, figyelők, a háttérkészlet és a HTTP-beállításait. Ez a lépés hello alkalmazás átjáró toouse alapvető útválasztási szabályokat konfigurálja, egy minden webhelyre vonatkozóan. Forgalom tooeach webhelyet a beállított figyelő fogad, és a rendszer ekkor átirányítja tooits konfigurált háttérkészlet, a backendhttpsettings beállítások hello megadott hello tulajdonságokkal.

```powershell
$rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule01" -RuleType Basic -HttpListener $listener01 -BackendHttpSettings $poolSetting01 -BackendAddressPool $pool1
$rule02 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule02" -RuleType Basic -HttpListener $listener02 -BackendHttpSettings $poolSetting02 -BackendAddressPool $pool2
```

### <a name="step-9"></a>9. lépés

Példányok és hello Alkalmazásátjáró mérete hello számának konfigurálása.

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name "Standard_Medium" -Tier Standard -Capacity 2
```

## <a name="create-application-gateway"></a>Alkalmazásátjáró létrehozása

Hozzon létre egy alkalmazást az előző lépésekben hello összes konfigurációs objektumok.

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-RG -Location "West US" -BackendAddressPools $pool1,$pool2 -BackendHttpSettingsCollection $poolSetting01, $poolSetting02 -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener01, $listener02 -RequestRoutingRules $rule01, $rule02 -Sku $sku -SslCertificates $cert01, $cert02
```

> [!IMPORTANT]
> Alkalmazás átjáró kiépítése hosszú ideig futó művelet, és némi időt toocomplete is igénybe vehet.
> 
> 

## <a name="get-application-gateway-dns-name"></a>Az Application Gateway DNS-nevének beszerzése

Hello átjáró létrehozása után hello következő lépésre tooconfigure hello előtér-kommunikációhoz. Nyilvános IP-cím esetén az Application Gateway használatához dinamikusan hozzárendelt DNS-névre van szükség, amely nem valódi név. tooensure a végfelhasználók is találati hello Alkalmazásátjáró, egy olyan CNAME rekordot is használt toopoint toohello nyilvános végpontot hello Alkalmazásátjáró. [Egyéni tartománynév konfigurálása az Azure-ban](../cloud-services/cloud-services-custom-domain-name-portal.md). toodo a, hello Alkalmazásátjáró és a társított IP-/ DNS-nevét, hello PublicIPAddress elem csatolt toohello Alkalmazásátjáró beolvasása részleteit. hello alkalmazás átjáró DNS-névnek kell lennie a használt toocreate egy olyan CNAME rekordot pontok hello két webes alkalmazások toothis DNS-név. A-rekordok hello használata nem javasolt, mert hello VIP módosíthatja az Alkalmazásátjáró újra kell indítani.

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

Megtudhatja, hogyan tooprotect a webhelyek [Application Gateway - webalkalmazási tűzfal](application-gateway-webapplicationfirewall-overview.md)

