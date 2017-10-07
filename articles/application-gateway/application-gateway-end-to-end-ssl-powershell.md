---
title: "aaaConfigure end tooend SSL-Titkosítást az Azure Application Gateway |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooconfigure end tooend SSL-Titkosítást az Alkalmazásátjáró Azure Resource Manager PowerShell használatával"
services: application-gateway
documentationcenter: na
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: e6d80a33-4047-4538-8c83-e88876c8834e
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/19/2017
ms.author: gwallace
ms.openlocfilehash: 7c280478e143d309e7665219441cbee8c81d9a80
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-end-tooend-ssl-with-application-gateway-using-powershell"></a>Záró tooend SSL konfigurálása az Alkalmazásátjáró PowerShell használatával

## <a name="overview"></a>Áttekintés

Alkalmazás átjáró támogatja a forgalom titkosítása tooend végén. Alkalmazásátjáró ennek érdekében hello SSL-kapcsolat hello Alkalmazásátjáró leáll. hello átjáró majd hello útválasztási szabályok toohello forgalom újra titkosítja a hello csomagot, majd továbbítja hello csomagok toohello megfelelő háttér hello megadott útválasztási szabályok alapján alkalmazza. Hello webkiszolgáló válaszának végighalad hello hátsó toohello végfelhasználói ugyanezt a folyamatot.

Egy másik szolgáltatás, hogy az Alkalmazásátjáró is támogatja egyéni SSL-beállítások. Alkalmazásátjáró támogatja a következő protokoll verziója; letiltása hello **TLSv1.0**, **TLSv1.1**, és **TLSv1.2** is meghatározása hello, amely cipher suites toouse és hello sorrend figyelembe vételével.  További információ az SSL-beállítások konfigurálhatók, toolearn látogasson el [SSL-házirend áttekintése](application-gateway-SSL-policy-overview.md).

> [!NOTE]
> Az SSL 2.0 és az SSL 3.0 alapértelmezés szerint le vannak tiltva, és nem lehet engedélyezni. Ezek nem biztonságos pedig nem képes toobe Alkalmazásátjáró együtt.

![a forgatókönyv kép][scenario]

## <a name="scenario"></a>Forgatókönyv

Ebben a forgatókönyvben a megismerheti, hogyan egy alkalmazást átjáró, amely toocreate end tooend SSL PowerShell használatával.

Ez a forgatókönyv tartalma:

* Hozzon létre egy erőforráscsoportot nevű **appgw-rg-n**
* Hozzon létre egy virtuális hálózatot nevű **appgwvnet** rendelkező 10.0.0.0/16 a címteret.
* Hozzon létre két alhálózat nevű **appgwsubnet** és **appsubnet**.
* Hozzon létre egy kis alkalmazás átjáró támogató end tooend SSL-titkosítást, adott korlátok SSL protokollok verziójának és titkosító csomagok.

## <a name="before-you-begin"></a>Előkészületek

tooconfigure end tooend SSL az Alkalmazásátjáró, egy tanúsítványra szükség, hello átjáró számára, és hello háttérkiszolgálók tanúsítványok szükségesek. hello átjáró tanúsítvány használt tooencrypt és visszafejtése hello elküldött forgalom tooit SSL használatával. hello átjáró tanúsítványt kell toobe személyes információcsere (pfx) formátumú. Ez lehetővé teszi a hello titkos kulcs toobe exportált hello alkalmazás átjáró tooperform hello titkosítási és visszafejtési forgalom által előírt.

Az end tooend SSL titkosítást hello háttér szerepel az engedélyezési listán alkalmazás átjáróval kell lennie. Nyilvános tanúsítvány hello hello háttérkiszolgálókon toohello Alkalmazásátjáró feltöltése ehhez. Ez biztosítja, hogy hello Alkalmazásátjáró csak ismert háttér példányok kommunikál. Ez biztosítja a hello közötti tooend kommunikáció további.

Ez a folyamat ismertetett lépésekkel hello:

## <a name="create-hello-resource-group"></a>Hello erőforráscsoport létrehozása

Ez a szakasz bemutatja, hogyan hello Alkalmazásátjáró tartalmazó erőforráscsoport létrehozása.

### <a name="step-1"></a>1. lépés

Jelentkezzen be Azure-fiók tooyour.

```powershell
Login-AzureRmAccount
```

### <a name="step-2"></a>2. lépés

Válassza ki a hello előfizetés toouse ehhez a forgatókönyvhöz.

```powershell
Select-AzureRmsubscription -SubscriptionName "<Subscription name>"
```

### <a name="step-3"></a>3. lépés

Hozzon létre egy erőforráscsoportot (hagyja ki ezt a lépést, ha egy meglévő erőforráscsoportot használ).

```powershell
New-AzureRmResourceGroup -Name appgw-rg -Location "West US"
```

## <a name="create-a-virtual-network-and-a-subnet-for-hello-application-gateway"></a>Hozzon létre egy virtuális hálózatot és hello Alkalmazásátjáró alhálózatot

hello alábbi példa létrehoz egy virtuális hálózat és két alhálózat. Egy alhálózat használt toohold hello Alkalmazásátjáró. hello más alhálózati használható hello háttérkiszolgálókon üzemeltetési hello webes alkalmazást.

### <a name="step-1"></a>1. lépés

Rendelje hozzá a címtartomány hello alhálózati hello Alkalmazásátjáró magát a használható.

```powershell
$gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24
```

> [!NOTE]
> Az Alkalmazásátjáró konfigurált alhálózatok megfelelő méretű lehet. Alkalmazásátjáró konfigurálhatja a too10 példány mentése. Minden példány hello alhálózat egy IP-címeit vesz igénybe. Túl kicsi alhálózaton kedvezőtlen hatással lehet, olyan átjárót kiterjesztése.
> 
> 

### <a name="step-2"></a>2. lépés

Rendeljen hozzá egy háttér címkészletet hello használt cím tartomány toobe.

```powershell
$nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24
```

### <a name="step-3"></a>3. lépés

Virtuális hálózat létrehozása az előző lépésekben hello definiált hello alhálózatokon.

```powershell
$vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet
```

### <a name="step-4"></a>4. lépés

Lekéri az hello virtuális hálózati erőforrás és alhálózati erőforrások toobe használt hello a következő lépéseket:

```powershell
$vnet = Get-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg
$gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -VirtualNetwork $vnet
$nicSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appsubnet' -VirtualNetwork $vnet
```

## <a name="create-a-public-ip-address-for-hello-front-end-configuration"></a>A nyilvános IP-cím hello előtér-konfiguráció létrehozása

Hozzon létre egy nyilvános IP erőforrás toobe hello használt. A nyilvános IP-címet használja a következő lépéssel.

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -Name 'publicIP01' -Location "West US" -AllocationMethod Dynamic
```

> [!IMPORTANT]
> Alkalmazásátjáró nem támogatja a nyilvános IP-cím, tartomány címkével rendelkező létrehozott hello használatát. Csak nyilvános IP-címnek a dinamikusan létrehozott tartományi címkére esetén támogatott. Ha egy rövid DNS-nevet az Alkalmazásátjáró hello van szüksége, ajánlott aliasként rögzítse toouse egy olyan CNAME REKORDOT.

## <a name="create-an-application-gateway-configuration-object"></a>Hozzon létre egy Application Gateway konfigurációs objektumot

Az összes konfigurációs elemek hello Alkalmazásátjáró létrehozása előtt vannak állítva. hello lépések hello konfigurációs elemek létrehozását, amelyek szükségesek az alkalmazás átjáró-erőforráshoz.

### <a name="step-1"></a>1. lépés

Hozzon létre egy alkalmazás átjáró IP-konfiguráció, ezzel a beállítással milyen alhálózati hello alkalmazás átjárót használja. Alkalmazásátjáró indításakor szerzi be hello alhálózati konfigurált IP-címeit, és továbbítja a hálózati forgalom toohello IP-címek hello háttér IP-címkészletben. Ne feledje, hogy minden példány egy IP-címet vesz fel.

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet
```

### <a name="step-2"></a>2. lépés

Hozzon létre egy előtér-IP-konfiguráció, ezzel a beállítással egy privát vagy nyilvános IP-cím toohello előtér hello Alkalmazásátjáró képezi le. a következő lépés hello társítja hello előtér-IP-konfigurációval lépést megelőző hello hello nyilvános IP-címet.

```powershell
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip
```

### <a name="step-3"></a>3. lépés

Konfigurálja a hello háttér IP-címkészletet a hello webes háttérkiszolgálók hello IP-címmel. Az IP-címeket azokat hello IP-címeket, amelyek hello előtér-IP-végponton származik hello hálózati forgalom fogadására. A következő IP-címek tooadd hello lecseréli a saját alkalmazás IP-cím végpontokat.

```powershell
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3
```

> [!NOTE]
> Egy teljesen minősített tartománynevét (FQDN) érték is érvényes hello háttérkiszolgálókhoz IP-cím helyett hello - BackendFqdns kapcsoló használatával. 

### <a name="step-4"></a>4. lépés

Konfigurálja az előtér-IP-port hello hello nyilvános IP-végponton. Ez a port nem hello port, amelyet a végfelhasználók csatlakozni.

```powershell
$fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443
```

### <a name="step-5"></a>5. lépés

Az Alkalmazásátjáró hello hello tanúsítvány konfigurálása. Ez a tanúsítvány használt toodecrypt, és a hello Alkalmazásátjáró hello-forgalom újratitkosítása.

```powershell
$cert = New-AzureRmApplicationGatewaySSLCertificate -Name cert01 -CertificateFile <full path too.pfx file> -Password <password for certificate file>
```

> [!NOTE]
> Ez a minta hello tanúsítvány használt SSL-kapcsolat konfigurálása. hello tanúsítványt kell toobe .pfx formátumú, és hello jelszó 4 too12 karakter közötti lehet.

### <a name="step-6"></a>6. lépés

Az Alkalmazásátjáró hello hello HTTP-figyelő létrehozása. Rendelje hozzá a hello előtér-IP-konfiguráció, a port és az SSL-tanúsítvány toouse.

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SSLCertificate $cert
```

### <a name="step-7"></a>7. lépés

Hello SSL használt toobe engedélyezve van a háttér címkészletet erőforrások hello-tanúsítvány feltöltése.

> [!NOTE]
> hello alapértelmezett mintavétel hello nyilvános kulcs lekérése hello **alapértelmezett** hello háttér-tartozó IP-cím és a szolgáltatás összehasonlítja hello nyilvános kulcs értéke toohello nyilvános kulcs értéke itt kap SSL-kötést. hello lekérdezése nyilvános kulcs nem feltétlenül hello szánt hely toowhich adatforgalmakat **Ha** állomásfejléc és használ az SNI hello háttér-a. Ha kétségei vannak, a keresse fel a https://127.0.0.1/ hello vissza-végpontok tooconfirm melyik tanúsítványt használják hello **alapértelmezett** SSL-kötést. Hello nyilvános kulcs használata ebben a szakaszban a kérelemből. Ha a HTTPS-kötések használunk állomásfejléc és SNI, és nem kapott választ, és a tanúsítvány egy manuális böngésző kérelem toohttps://127.0.0.1/ hello vissza-végén, be kell állítania egy alapértelmezett SSL-kötés hello vissza-végén. Ha ezt nem teszi meg, mintavételt sikertelen és hello háttér-nem szerepel az engedélyezési listán.

```powershell
$authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile C:\users\gwallace\Desktop\cert.cer
```

> [!NOTE]
> Ebben a lépésben megadott hello tanúsítvány hello hello háttér megtalálható hello pfx-tanúsítvány nyilvános kulcsának kell lennie. Hello háttérkiszolgálón a telepített hello tanúsítvány (nem hello főtanúsítvány) exportálja. CER formátumú, és használhatja ezt a lépést. Ez a lépés whitelists hello hello Alkalmazásátjáró háttérrendszeréhez.

### <a name="step-8"></a>8. lépés

Hello Alkalmazásátjáró háttér http beállításainak konfigurálása. Az előző lépés toohello http-beállítások hello feltöltött hello tanúsítvány hozzárendelése.

```powershell
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert
```

### <a name="step-9"></a>9. lépés

Hozzon létre egy terheléselosztási útválasztási szabály által konfigurált házirendsablonok hello load balancer viselkedését. Ebben a példában egy alapszintű ciklikus multiplexelés szabály jön létre.

```powershell
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
```

### <a name="step-10"></a>10. lépés

Hello Alkalmazásátjáró hello példány méretének beállítása.  hello elérhető értékek **szabványos\_kis**, **szabványos\_Közepes**, és **szabványos\_nagy**.  A kapacitás hello elérhető értékei 1 és 10 közötti.

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2
```

> [!NOTE]
> Tesztelési célokra példányszámának 1 választható ki. Fontos, hogy bármely példányok száma a két példányt tooknow hello SLA nem vonatkozik, és ezért nem ajánlott. Kis átjárók jellemzően toobe fejlesztés tesztelés és nem üzemi célokra szolgál.

### <a name="step-11"></a>11. lépés

Hello SSL házirend toobe használt hello Application Gateway konfigurálásához. Alkalmazásátjáró hello képességét tooset legalább támogatja az SSL protokoll verziója.

hello következő értékei, amelyek az protokoll-verziók listáját.

* **TLSv1_0**
* **TLSv1_1**
* **TLSv1_2**

Készlet túl hello minimális protokollverzió**TLSv1_2** és lehetővé teszi, hogy **TLS\_ECDHE\_ECDSA\_WITH\_AES\_128\_GCM\_ SHA-256**, **TLS\_ECDHE\_ECDSA\_WITH\_AES\_256\_GCM\_SHA384**, és **TLS\_RSA\_WITH\_AES\_128\_GCM\_SHA256** csak.

```powershell
$SSLPolicy = New-AzureRmApplicationGatewaySSLPolicy -MinProtocolVersion TLSv1_2 -CipherSuite "TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256", "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384", "TLS_RSA_WITH_AES_128_GCM_SHA256"
```

## <a name="create-hello-application-gateway"></a>Hello Alkalmazásátjáró létrehozása

Alkalmazásátjáró hello előző lépésekben összes hello segítségével hozzon létre. hello átjáró létrehozása hello rendkívül hosszú ideig futó folyamat.

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgateway -SSLCertificates $cert -ResourceGroupName "appgw-rg" -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SSLPolicy $SSLPolicy -AuthenticationCertificates $authcert -Verbose
```

## <a name="limit-ssl-protocol-versions-on-an-existing-application-gateway"></a>Az SSL protokoll verziói a meglévő Alkalmazásátjáró korlátozása

hello előző lépések befejezési tooend SSL alkalmazás létrehozása és bizonyos SSL protokollverziója letiltása. hello következő példa letiltja az egyes meglévő Alkalmazásátjáró SSL-házirendeket.

### <a name="step-1"></a>1. lépés

Hello alkalmazás átjáró tooupdate beolvasása.

```powershell
$gw = Get-AzureRmApplicationGateway -Name AdatumAppGateway -ResourceGroupName AdatumAppGatewayRG
```

### <a name="step-2"></a>2. lépés

Egy SSL-házirend meghatározásához. A következő példa hello, TLSv1.0 és TLSv1.1 le vannak tiltva, és titkosító csomagok hello **TLS\_ECDHE\_ECDSA\_WITH\_AES\_128\_GCM\_SHA256** , **TLS\_ECDHE\_ECDSA\_WITH\_AES\_256\_GCM\_SHA384**, és  **A TLS\_RSA\_WITH\_AES\_128\_GCM\_SHA256** hello csak ők engedélyezve vannak.

```powershell
Set-AzureRmApplicationGatewaySSLPolicy -MinProtocolVersion -PolicyType Custom -CipherSuite "TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256", "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384", "TLS_RSA_WITH_AES_128_GCM_SHA256" -ApplicationGateway $gw

```

### <a name="step-3"></a>3. lépés

Végül frissítse hello átjáró. Fontos, hogy ezen utolsó lépésében-e a hosszú ideig futó feladatok toonote. Ha elkészült, záró tooend SSL hello Alkalmazásátjáró van konfigurálva.

```powershell
$gw | Set-AzureRmApplicationGateway
```

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

További tudnivalók a webalkalmazások a webalkalmazási tűzfal alkalmazás átjárón keresztül hello biztonsági korlátozások ellátogatva [webalkalmazás tűzfal áttekintése](application-gateway-webapplicationfirewall-overview.md)

[scenario]: ./media/application-gateway-end-to-end-SSL-powershell/scenario.png
