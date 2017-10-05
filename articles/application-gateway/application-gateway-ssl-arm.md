---
title: "Konfigurálja az SSL kiszervezési - Azure Application Gateway - PowerShell |} Microsoft Docs"
description: "Ez az oldal utasításokat tartalmaz egy SSL-alapú kiszervezéssel rendelkező Application Gateway létrehozásához az Azure Resource Manager használatával"
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
ms.openlocfilehash: ededabc7c665d6bb05b91e4d21d01fb1379add32
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="configure-an-application-gateway-for-ssl-offload-by-using-azure-resource-manager"></a>Application Gateway konfigurálása SSL-alapú kiszervezéshez az Azure Resource Manager használatával

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-ssl-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-ssl-arm.md)
> * [Klasszikus Azure PowerShell](application-gateway-ssl.md)
> * [Azure CLI 2.0](application-gateway-ssl-cli.md)

Az Azure Application Gateway konfigurálható úgy, hogy leállítsa a Secure Sockets Layer (SSL) munkamenetét az átjárónál, így elkerülhetők a költséges SSL visszafejtési feladatok a webfarmon. Az SSL-alapú kiszervezés emellett leegyszerűsíti az előtér-kiszolgáló számára webalkalmazás telepítését és kezelését.

## <a name="before-you-begin"></a>Előkészületek

1. Telepítse az Azure PowerShell-parancsmagok legújabb verzióját a Webplatform-telepítővel. A [Letöltések lap](https://azure.microsoft.com/downloads/) **Windows PowerShell** szakaszából letöltheti és telepítheti a legújabb verziót.
2. Létre kell hozni egy virtuális hálózatot és alhálózatot az Application Gateway számára. Győződjön meg arról, hogy egy virtuális gép vagy felhőalapú telepítés sem használja az alhálózatot. Az Application Gateway-nek egyedül kell lennie a virtuális hálózat alhálózatán.
3. A kiszolgálóknak, amelyeket az Application Gateway használatára konfigurál, már létezniük kell, illetve a virtuális hálózatban vagy hozzárendelt nyilvános/virtuális IP-címmel létrehozott végpontokkal kell rendelkezniük.

## <a name="what-is-required-to-create-an-application-gateway"></a>Mire van szükség egy Application Gateway létrehozásához?

* **Háttér-kiszolgálókészlet:** A háttérkiszolgálók IP-címeinek listája. A listán szereplő IP-címeknek a virtuális hálózat alhálózatához kell tartozniuk, vagy nyilvános/virtuális IP-címnek kell lenniük.
* **Háttér-kiszolgálókészlet beállításai:** Minden készletnek vannak beállításai, például port, protokoll vagy cookie-alapú affinitás. Ezek a beállítások egy adott készlethez kapcsolódnak, és a készlet minden kiszolgálójára érvényesek.
* **Előtérbeli port:** Az Application Gateway-en megnyitott nyilvános port. Amikor a forgalom eléri ezt a portot, a port átirányítja az egyik háttérkiszolgálóra.
* **Figyelő:** A figyelő egy előtérbeli porttal, egy protokollal (Http vagy Https, ezek kis- és nagybetűt megkülönböztető beállítások) és SSL tanúsítványnévvel rendelkezik.
* **Szabály:** A szabály összeköti a figyelőt és a háttérkiszolgáló-készletet, és meghatározza, hogy mely háttérkiszolgáló-készletre legyen átirányítva a forgalom, ha elér egy adott figyelőt. Jelenleg csak a *basic* szabály támogatott. A *basic* szabály a ciklikus időszeleteléses terheléselosztás.

**További konfigurációs megjegyzések**

Az SSL-tanúsítványok konfigurálásához *Https*-re kell módosítani a **HttpListener** protokollját (megkülönböztetve a kis- és nagybetűket). Az **SslCertificate** elemet hozzá kell adni a **HttpListener** elemhez az SSL-tanúsítvány számára konfigurált változóértékkel. Az előtérbeli portot frissíteni kell 443-ra.

**A cookie-alapú affinitás engedélyezése**: Egy Application Gateway konfigurálható úgy, hogy egy ügyfélmunkamenetből érkező kérelmet mindig a webfarm ugyanazon virtuális gépére irányítsa. Ez a forgatókönyv egy munkameneti cookie beszúrásával lesz végrehajtva, amely lehetővé teszi az átjáró számára a forgalom megfelelő irányítását. A cookie-alapú affinitás engedélyezéséhez a **CookieBasedAffinity** paraméter beállítása legyen *Enabled* a **BackendHttpSettings** elemen belül.

## <a name="create-an-application-gateway"></a>Application Gateway létrehozása

A klasszikus Azure üzembe helyezési modell és az Azure Resource Manager használata abban tér el egymástól, hogy más sorrendben kell létrehoznia az Application Gateway-t és a konfigurálást igénylő elemeket.

A Resource Managerrel az Application Gateway összes összetevőjét külön kell konfigurálni, és csak utána kell őket összeállítani az Application Gateway-erőforrás létrehozásához.

Egy Application Gateway létrehozásához a következő lépéseket kell végrehajtania:

1. Erőforráscsoport létrehozása a Resource Managerhez
2. Virtuális hálózat, alhálózat és nyilvános IP-cím létrehozása az Application Gateway számára
3. Hozzon létre egy Application Gateway konfigurációs objektumot
4. Application Gateway erőforrás létrehozása

## <a name="create-a-resource-group-for-resource-manager"></a>Erőforráscsoport létrehozása a Resource Managerhez

Az Azure Resource Manager parancsmagjainak használatához váltson át PowerShell módba. További információ: [A Windows PowerShell használata a Resource Managerrel](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>1. lépés

```powershell
Login-AzureRmAccount
```

### <a name="step-2"></a>2. lépés

Keresse meg a fiókot az előfizetésekben.

```powershell
Get-AzureRmSubscription
```

A rendszer kérni fogja a hitelesítő adatokkal történő hitelesítést.

### <a name="step-3"></a>3. lépés

Válassza ki, hogy melyik Azure előfizetést fogja használni.

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="step-4"></a>4. lépés

Hozzon létre egy erőforráscsoportot (hagyja ki ezt a lépést, ha egy meglévő erőforráscsoportot használ).

```powershell
New-AzureRmResourceGroup -Name appgw-rg -Location "West US"
```

Az Azure Resource Manager megköveteli, hogy minden erőforráscsoport megadjon egy helyet. Ez a beállítás szolgál az erőforráscsoport erőforrásainak alapértelmezett helyeként. Győződjön meg arról, hogy az Application Gateway létrehozására irányuló összes parancs ugyanazt az erőforráscsoportot használja.

A fenti példában létrehoztunk egy **appgw-RG** nevű erőforráscsoportot, amelynek a helye az USA nyugati régiója (**West US**).

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Virtuális hálózat és alhálózat létrehozása az Application Gateway számára

Az alábbi példa bemutatja, hogyan hozhat létre egy virtuális hálózatot a Resource Manager használatával:

### <a name="step-1"></a>1. lépés

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24
```

Ez a példa hozzárendeli a 10.0.0.0/24 címtartományt a virtuális hálózat létrehozásához használni kívánt egyik alhálózati változóhoz.

### <a name="step-2"></a>2. lépés

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet
```

Ez a minta létrehoz egy virtuális hálózatot nevű **appgwvnet** erőforráscsoportban **appgw-rg** az USA nyugati régiója régió a előtag 10.0.0.0/16 használata a 10.0.0.0/24 alhálózat.

### <a name="step-3"></a>3. lépés

```powershell
$subnet = $vnet.Subnets[0]
```

Ez a példa hozzárendeli az alhálózati objektumot a $subnet változóhoz a következő lépésekhez.

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Nyilvános IP-cím létrehozása az előtérbeli konfigurációhoz

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

Ez a minta létrehoz egy nyilvános IP-erőforrás **publicIP01** erőforráscsoportban **appgw-rg** az USA nyugati régiójában.

## <a name="create-an-application-gateway-configuration-object"></a>Hozzon létre egy Application Gateway konfigurációs objektumot

### <a name="step-1"></a>1. lépés

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet
```

Ez a minta létrehoz egy alkalmazás átjáró IP-konfiguráció nevű **gatewayIP01**. Amikor az Application Gateway elindul, a konfigurált alhálózatból felvesz egy IP-címet, és a hálózati forgalmat a háttérbeli IP-készlet IP-címeihez irányítja. Ne feledje, hogy minden példány egy IP-címet vesz fel.

### <a name="step-2"></a>2. lépés

```powershell
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50
```

Ez a minta beállítja a háttér IP-címkészlet nevű **pool01** IP-címekkel rendelkező **134.170.185.46**, **134.170.188.221**, **134.170.185.50**. Ezek az értékek azok az IP-címek, amelyek fogadják majd az előtérbeli IP-végpontból érkező hálózati forgalmat. Cserélje le az előző példában szereplő IP-címeket a webalkalmazás végpontjainak IP-címeire.

### <a name="step-3"></a>3. lépés

```powershell
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Enabled
```

Ez a minta konfigurálja az átjáró Alkalmazásbeállítás **poolsetting01** terhelésű hálózati forgalmat a háttér-készletben.

### <a name="step-4"></a>4. lépés

```powershell
$fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 443
```

Ez a minta konfigurálja az előtér-IP-port nevű **frontendport01** a nyilvános IP-végponton.

### <a name="step-5"></a>5. lépés

```powershell
$cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path for certificate file> -Password "<password>"
```

Ez a példa az SSL-kapcsolathoz használt tanúsítványt konfigurálja. A tanúsítványnak .pfx formátumúnak, a jelszónak pedig 4 és 12 karakter közötti hosszúságúnak kell lennie.

### <a name="step-6"></a>6. lépés

```powershell
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip
```

Ez a minta hoz létre az előtér-IP-konfiguráció **fipconfig01** és a nyilvános IP-címet társít az előtér-IP-konfigurációt.

### <a name="step-7"></a>7. lépés

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert
```

Ez a minta hozza létre a figyelő nevét **listener01** , és hozzárendeli az előtér-IP-konfiguráció és a tanúsítvány az előtér-port.

### <a name="step-8"></a>8. lépés

```powershell
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
```

Ez a minta hoz létre a terheléselosztó útválasztási szabály nevű **rule01** , konfigurálja a terheléselosztó terheléselosztási viselkedését.

### <a name="step-9"></a>9. lépés

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2
```

Ez az Application Gateway példányméretét konfigurálja.

> [!NOTE]
> Az *InstanceCount* alapértelmezett értéke 2, a maximális értéke pedig 10. A *GatewaySize* alapértelmezett értéke Közepes. A Standard_Small, Standard_Medium és a Standard_Large lehetőségek közül választhat.

### <a name="step-10"></a>10. lépés

```powershell
$policy = New-AzureRmApplicationGatewaySslPolicy -PolicyType Predefined -PolicyName AppGwSslPolicy20170401S
```

Ez a lépés határozza meg az SSL-házirend az Alkalmazásátjáró használatára. Látogasson el [SSL konfigurálása házirend verziója és az Application Gateway titkosító csomagok](application-gateway-configure-ssl-policy-powershell.md) további.

## <a name="create-an-application-gateway-by-using-new-azureapplicationgateway"></a>Application Gateway létrehozása a New-AzureApplicationGateway használatával

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SslCertificates $cert -SslPolicy $policy
```

Ez létrehoz egy Application Gateway-t az előző lépések konfigurációs elemeivel. A példában az Alkalmazásátjáró nevezik **appgwtest**.

## <a name="get-application-gateway-dns-name"></a>Az Application Gateway DNS-nevének beszerzése

Az átjáró létrehozása után a következő lépés a kommunikációra szolgáló előtér konfigurálása. Nyilvános IP-cím esetén az Application Gateway használatához dinamikusan hozzárendelt DNS-névre van szükség, amely nem valódi név. Ha szeretné, hogy a végfelhasználók elérjék az Application Gatewayt, használjon egy Application Gateway nyilvános végpontjára mutató CNAME-rekordot. [Egyéni tartománynév konfigurálása az Azure-ban](../cloud-services/cloud-services-custom-domain-name-portal.md). A művelet végrehajtásához az Application Gateway részleteinek beszerzésére és a kapcsolódó IP/DNS-név lekérésére van szükség az Application Gatewayhez csatolt PublicIPAddress használatával. Az Application Gateway DNS-nevének használatával létrehozhat egy CNAME rekordot, amely a két webalkalmazást erre a DNS-névre irányítja. Az A-bejegyzések használata nem javasolt, mivel a virtuális IP-cím változhat az Application Gateway újraindításakor.


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

Ha konfigurálni szeretne egy ILB-vel használni kívánt Application Gateway-t: [Application Gateway létrehozása belső terheléselosztóval (ILB)](application-gateway-ilb.md).

Ha további általános információra van szüksége a terheléselosztás beállításaival kapcsolatban:

* [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)

