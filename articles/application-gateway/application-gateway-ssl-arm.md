---
title: "aaaConfigure SSL - Azure Application Gateway - PowerShell kiszervezése |} Microsoft Docs"
description: "Ezen a lapon nyújt útmutatást toocreate Alkalmazásátjáró SSL-kiszervezés Azure Resource Manager használatával"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 3c3681e0-f928-4682-9d97-567f8e278e13
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/19/2017
ms.author: gwallace
ms.openlocfilehash: c2855d8d3caaa97ec05475c67ff0f8dce72ef2a7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-an-application-gateway-for-ssl-offload-by-using-azure-resource-manager"></a>Application Gateway konfigurálása SSL-alapú kiszervezéshez az Azure Resource Manager használatával

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-ssl-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-ssl-arm.md)
> * [Klasszikus Azure PowerShell](application-gateway-ssl.md)
> * [Azure CLI 2.0](application-gateway-ssl-cli.md)

Az Azure Application Gateway konfigurált tooterminate hello Secure Sockets Layer (SSL) munkamenet: hello átjáró tooavoid költséges SSL visszafejtési feladatok toohappen: hello webfarm lehet. SSL kiszervezési is egyszerűbbé teszi a hello előtér-kiszolgáló beállítása és felügyelete hello webalkalmazás.

## <a name="before-you-begin"></a>Előkészületek

1. Webplatform-telepítő hello segítségével hello hello Azure PowerShell-parancsmagok legújabb verzióját telepítse. Töltse le, és telepítse hello legújabb verziót a hello **Windows PowerShell** hello szakasza [letöltési oldalon](https://azure.microsoft.com/downloads/).
2. Létrehozhat egy virtuális hálózatot és egy hello alkalmazás átjáró-alhálózatot. Győződjön meg arról, hogy egyetlen virtuális gépek vagy a felhőben történő alkalmazáshoz hello alhálózat használja. Az Application Gateway-nek egyedül kell lennie a virtuális hálózat alhálózatán.
3. hello kiszolgálók toouse hello Alkalmazásátjáró konfigurálnia kell lennie, különben a végpontokat hello virtuális hálózatban vagy a nyilvános IP/virtuális IP-címhez hozzárendelt létrehozása.

## <a name="what-is-required-toocreate-an-application-gateway"></a>Mi az a szükséges toocreate olyan átjárót?

* **Háttér-kiszolgálófiók készlet:** hello hello háttér-kiszolgálók IP-címek listáját. hello IP-címek felsorolt toohello virtuális hálózati alhálózat vagy kell tartoznia, vagy egy nyilvános IP-cím/VIP kell lennie.
* **Háttér-kiszolgálókészlet beállításai:** Minden készletnek vannak beállításai, például port, protokoll vagy cookie-alapú affinitás. Ezek a beállítások esetén tooa kapcsolt verem és a hello készlet alkalmazott tooall-kiszolgálók.
* **Előtér-port:** Ez a port nem hello nyilvános portot, amelyet a hello Alkalmazásátjáró meg van nyitva. Forgalom találatok ezt a portot, és lekérdezi átirányítja tooone hello háttér-kiszolgálók.
* **Figyelő:** hello figyelő rendelkezik egy előtér-portot, a protokollt (Http vagy Https, ezek a beállítások-és nagybetűk), és hello SSL tanúsítvány neve (ha az SSL beállításának-kiszervezés).
* **Szabály:** hello szabály hello figyelő és hello háttér-kiszolgálófiók alkalmazáskészlet van kötve, és azt határozza meg, mely háttér-kiszolgálófiók készlet hello forgalom irányított toowhen találatok száma a egy adott figyelő. Jelenleg csak hello *alapvető* szabály használata támogatott. Hello *alapvető* szabály-e időszeletelés terheléselosztási.

**További konfigurációs megjegyzések**

SSL-tanúsítványok beállítása, a hello protokoll **HttpListener** túl kell módosítani*Https* (kis-és nagybetűket). Hello **SslCertificate** elem túl kerül**HttpListener** hello változó értékével hello SSL-tanúsítvány konfigurálva. hello előtér-port frissített too443 lehet.

**tooenable cookie-alapú kapcsolat**: Alkalmazásátjáró lehet győződjön meg arról, hogy egy kérelem egy ügyfél-munkamenetből mindig irányított toohello konfigurált tooensure hello webfarm azonos virtuális gép. Ebben a forgatókönyvben történik, hogy az egy munkamenetcookie-t, amely lehetővé teszi annak megfelelően hello átjáró toodirect forgalom. cookie-alapú tooenable affinitás beállítása **CookieBasedAffinity** túl*engedélyezve* a hello **BackendHttpSettings** elemet.

## <a name="create-an-application-gateway"></a>Application Gateway létrehozása

hello közötti hello Azure klasszikus telepítési modell és az Azure Resource Manager használatával különbség toobe konfigurálni kell egy alkalmazás átjáró és hello elemek létrehozását hello sorrendjében.

A Resource Manager összes összetevőjének olyan átjárót konfigurált külön-külön és majd együtt toocreate egy alkalmazás átjáró-erőforráshoz.

Az alábbiakban hello szükséges lépéseket toocreate Alkalmazásátjáró:

1. Erőforráscsoport létrehozása a Resource Managerhez
2. Virtuális hálózati alhálózat és hello alkalmazás átjáró nyilvános IP-cím létrehozása
3. Hozzon létre egy Application Gateway konfigurációs objektumot
4. Application Gateway erőforrás létrehozása

## <a name="create-a-resource-group-for-resource-manager"></a>Erőforráscsoport létrehozása a Resource Managerhez

Győződjön meg arról, hogy a PowerShell mód toouse hello Azure Resource Manager parancsmagok váltani. További információ: [A Windows PowerShell használata a Resource Managerrel](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>1. lépés

```powershell
Login-AzureRmAccount
```

### <a name="step-2"></a>2. lépés

Hello előfizetések hello fiók ellenőrzése.

```powershell
Get-AzureRmSubscription
```

A hitelesítő adataival felszólító tooauthenticate áll.

### <a name="step-3"></a>3. lépés

Válassza ki, amely az Azure-előfizetések toouse.

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="step-4"></a>4. lépés

Hozzon létre egy erőforráscsoportot (hagyja ki ezt a lépést, ha egy meglévő erőforráscsoportot használ).

```powershell
New-AzureRmResourceGroup -Name appgw-rg -Location "West US"
```

Az Azure Resource Manager megköveteli, hogy minden erőforráscsoport megadjon egy helyet. Ezzel a beállítással hello alapértelmezett helye az erőforráscsoport erőforrások. Győződjön meg arról, hogy olyan átjárót használó összes parancsok toocreate hello azonos erőforráscsoportot.

A hello a fenti példában létrehozott nevű erőforráscsoport **appgw-RG** és a hely **USA nyugati régiója**.

## <a name="create-a-virtual-network-and-a-subnet-for-hello-application-gateway"></a>Hozzon létre egy virtuális hálózatot és hello Alkalmazásátjáró alhálózatot

a következő példa azt mutatja meg hogyan hello toocreate erőforrás-kezelő használatával egy virtuális hálózathoz:

### <a name="step-1"></a>1. lépés

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24
```

Ez a minta egy virtuális hálózati hello cím tartomány 10.0.0.0/24 tooa alhálózati használt változó toobe toocreate rendeli hozzá.

### <a name="step-2"></a>2. lépés

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet
```

Ez a minta létrehoz egy virtuális hálózatot nevű **appgwvnet** erőforráscsoportban **appgw-rg** hello USA nyugati régiójában hello előtag 10.0.0.0/16 használata a 10.0.0.0/24 alhálózat.

### <a name="step-3"></a>3. lépés

```powershell
$subnet = $vnet.Subnets[0]
```

Ez a minta hozzárendel hello alhálózati objektum toovariable $subnet hello további lépéseket.

## <a name="create-a-public-ip-address-for-hello-front-end-configuration"></a>A nyilvános IP-cím hello előtér-konfiguráció létrehozása

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

Ez a minta létrehoz egy nyilvános IP-erőforrás **publicIP01** erőforráscsoportban **appgw-rg** hello USA nyugati régiójában.

## <a name="create-an-application-gateway-configuration-object"></a>Hozzon létre egy Application Gateway konfigurációs objektumot

### <a name="step-1"></a>1. lépés

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet
```

Ez a minta létrehoz egy alkalmazás átjáró IP-konfiguráció nevű **gatewayIP01**. Alkalmazásátjáró indításakor szerzi be hello alhálózati konfigurált IP-címeit, és útvonal-hello háttér IP-készletet a hálózati forgalom toohello IP-címek. Ne feledje, hogy minden példány egy IP-címet vesz fel.

### <a name="step-2"></a>2. lépés

```powershell
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50
```

Ez a minta konfigurálja hello háttér IP-címkészlet nevű **pool01** IP-címekkel rendelkező **134.170.185.46**, **134.170.188.221**, **134.170.185.50** . Ezeket az értékeket azokat hello IP-címeket, amelyek hello előtér-IP-végponton származik hello hálózati forgalom fogadására. Cserélje le a webes alkalmazás végpontok hello IP-címekkel rendelkező példa megelőző hello hello IP-címek.

### <a name="step-3"></a>3. lépés

```powershell
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Enabled
```

Ez a minta konfigurálja az átjáró Alkalmazásbeállítás **poolsetting01** tooload kiegyensúlyozott hálózati forgalom hello háttér-készletben.

### <a name="step-4"></a>4. lépés

```powershell
$fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 443
```

Ez a minta hello nevű előtér-IP-port konfigurálása **frontendport01** a hello nyilvános IP-végponton.

### <a name="step-5"></a>5. lépés

```powershell
$cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path for certificate file> -Password "<password>"
```

Ez a minta hello tanúsítvány használt SSL-kapcsolat konfigurálása. hello tanúsítványt kell toobe .pfx formátumú, és hello jelszó 4 too12 karakter közötti lehet.

### <a name="step-6"></a>6. lépés

```powershell
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip
```

Ez a minta létrehoz hello előtér-IP-konfiguráció **fipconfig01** és társult hello hello előtér-IP-konfigurációja nyilvános IP-címmel.

### <a name="step-7"></a>7. lépés

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert
```

Ez a minta hello figyelő nevét hozza létre **listener01** és társult hello előtér-port toohello előtér-IP-konfiguráció és a tanúsítvány.

### <a name="step-8"></a>8. lépés

```powershell
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
```

Ez a minta létrehoz hello terheléselosztási útválasztási szabály nevű **rule01** , konfigurálja az hello load balancer viselkedését.

### <a name="step-9"></a>9. lépés

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2
```

Ez a minta hello példányméretének hello Alkalmazásátjáró konfigurálja.

> [!NOTE]
> az alapértelmezett érték hello *InstanceCount* 2, maximális értéke 10. az alapértelmezett érték hello *GatewaySize* közepes. A Standard_Small, Standard_Medium és a Standard_Large lehetőségek közül választhat.

### <a name="step-10"></a>10. lépés

```powershell
$policy = New-AzureRmApplicationGatewaySslPolicy -PolicyType Predefined -PolicyName AppGwSslPolicy20170401S
```

Ez a lépés hello SSL házirend toouse hello Alkalmazásátjáró határozza meg. Látogasson el [SSL konfigurálása házirend verziója és az Application Gateway titkosító csomagok](application-gateway-configure-ssl-policy-powershell.md) további toolearn.

## <a name="create-an-application-gateway-by-using-new-azureapplicationgateway"></a>Application Gateway létrehozása a New-AzureApplicationGateway használatával

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SslCertificates $cert -SslPolicy $policy
```

Ez a minta Alkalmazásátjáró az előző lépésekben hello összes konfigurációs elemet hoz létre. Hello példában hello Alkalmazásátjáró nevezik **appgwtest**.

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

Ha azt szeretné, hogy egy alkalmazás átjáró toouse egy belső terheléselosztón (ILB) rendelkező tooconfigure, [hozzon létre egy alkalmazást egy belső terheléselosztón (ILB)](application-gateway-ilb.md).

Ha további általános információra van szüksége a terheléselosztás beállításaival kapcsolatban:

* [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)

