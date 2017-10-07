---
title: "a belső terheléselosztó - PowerShell Azure Application Gateway aaaUsing |} Microsoft Docs"
description: "Ezen a lapon útmutatás toocreate, konfigurálása, indítsa el, és a belső terheléselosztón (ILB) Azure Alkalmazásátjáró törlése az Azure Resource Manager"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 75cfd5a2-e378-4365-99ee-a2b2abda2e0d
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: gwallace
ms.openlocfilehash: dd0d7e954b1fa219ae6ebe42cb4b479dbcf08653
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-gateway-with-an-internal-load-balancer-ilb-by-using-azure-resource-manager"></a>Belső terheléselosztóval (ILB) rendelkező Application Gateway létrehozása az Azure Resource Manager használatával

> [!div class="op_single_selector"]
> * [Klasszikus Azure PowerShell](application-gateway-ilb.md)
> * [Azure Resource Manager PowerShell](application-gateway-ilb-arm.md)

Azure Application Gateway egy internetre irányuló virtuális IP-címre, vagy egy belső végponttal, amely nincs kitett toohello Internet, más néven belső terheléselosztó (ILB) végpont konfigurálható. Hello átjáró konfigurálása egy ILB akkor hasznos, amelyek nincsenek kitett toohello Internet belső üzleti alkalmazásokhoz. Szintén hasznosak lehetnek a szolgáltatások és rétegek belül egy többrétegű alkalmazást, amely nincs kitett toohello internetes biztonsági határt a elhelyezkedik, de továbbra is a ciklikus multiplexelési igénylő betöltése terjesztési, a munkamenet tölcsérútvonalak vagy a Secure Sockets Layer (SSL) megszüntetése.

Ez a cikk bemutatja, hogyan hello lépéseket tooconfigure egy ILB az Alkalmazásátjáró.

## <a name="before-you-begin"></a>Előkészületek

1. Webplatform-telepítő hello segítségével hello hello Azure PowerShell-parancsmagok legújabb verzióját telepítse. Töltse le, és telepítse hello legújabb verziót a hello **Windows PowerShell** hello szakasza [letöltési oldalon](https://azure.microsoft.com/downloads/).
2. Létre kell hozni egy virtuális hálózatot és alhálózatot az Application Gateway számára. Győződjön meg arról, hogy egyetlen virtuális gépek vagy a felhőben történő alkalmazáshoz hello alhálózat használja. Az Application Gateway-nek egyedül kell lennie a virtuális hálózat alhálózatán.
3. hello kiszolgálók toouse hello Alkalmazásátjáró konfigurált léteznie kell, különben a végpontok hello virtuális hálózatban vagy a nyilvános IP-cím/VIP rendelt hozzá.

## <a name="what-is-required-toocreate-an-application-gateway"></a>Mi az a szükséges toocreate olyan átjárót?

* **Háttér-kiszolgálófiók készlet:** hello hello háttér-kiszolgálók IP-címek listáját. hello IP-cím jelenik meg vagy tartozik toohello virtuális hálózat, de az Alkalmazásátjáró hello egy másik alhálózat vagy egy nyilvános IP-cím/VIP kell lennie.
* **Háttér-kiszolgálókészlet beállításai:** Minden készletnek vannak beállításai, például port, protokoll vagy cookie-alapú affinitás. Ezek a beállítások esetén tooa kapcsolt verem és a hello készlet alkalmazott tooall-kiszolgálók.
* **Előtér-port:** Ez a port nem hello nyilvános portot, amelyet a hello Alkalmazásátjáró meg van nyitva. Forgalom találatok ezt a portot, és lekérdezi átirányítja tooone hello háttér-kiszolgálók.
* **Figyelő:** hello figyelő rendelkezik egy előtér-portot, a protokollt (Http vagy Https, amelyek kis-és nagybetűket), és hello SSL tanúsítvány neve (ha az SSL beállításának-kiszervezés).
* **Szabály:** hello szabály hello figyelő és hello háttér-kiszolgálófiók alkalmazáskészlet van kötve, és azt határozza meg, mely háttér-kiszolgálófiók készlet hello forgalom irányított toowhen találatok száma a egy adott figyelő. Jelenleg csak hello *alapvető* szabály használata támogatott. Hello *alapvető* szabály-e időszeletelés terheléselosztási.

## <a name="create-an-application-gateway"></a>Application Gateway létrehozása

hello közötti Azure klasszikus és az Azure Resource Manager használatával különbség hello rendelés hello Alkalmazásátjáró és hello elemeket toobe konfigurálva kell létrehozni.
A Resource Manager az összes olyan átjárót elemeit van konfigurálva külön-külön és helyreállíthatósága majd toocreate hello alkalmazás átjáró-erőforráshoz.

Az alábbiakban a szükséges toocreate Alkalmazásátjáró hello lépéseket:

1. Erőforráscsoport létrehozása a Resource Managerhez
2. Hozzon létre egy virtuális hálózatot és hello Alkalmazásátjáró alhálózatot
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

Hozzon létre egy új erőforráscsoportot (hagyja ki ezt a lépést, ha egy meglévő erőforráscsoportot használ).

```powershell
New-AzureRmResourceGroup -Name appgw-rg -location "West US"
```

Az Azure Resource Manager megköveteli, hogy minden erőforráscsoport megadjon egy helyet. Ez az erőforráscsoport erőforrások hello alapértelmezett helye szolgál. Győződjön meg arról, hogy olyan átjárót használó összes parancsok toocreate hello azonos erőforráscsoportot.

A fenti példa hello létrehozott egy "Appgw-rg" és a hely "USA nyugati régiója" nevű erőforráscsoport.

## <a name="create-a-virtual-network-and-a-subnet-for-hello-application-gateway"></a>Hozzon létre egy virtuális hálózatot és hello Alkalmazásátjáró alhálózatot

a következő példa azt mutatja meg hogyan hello toocreate erőforrás-kezelő használatával egy virtuális hálózathoz:

### <a name="step-1"></a>1. lépés

```powershell
$subnetconfig = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24
```

Ebben a lépésben a virtuális hálózati hello cím tartomány 10.0.0.0/24 tooa alhálózati használt változó toobe toocreate rendeli hozzá.

### <a name="step-2"></a>2. lépés

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnetconfig
```

Ebben a lépésben a virtuális hálózati "appgwvnet" nevű erőforrás csoport "appgw-rg" hello USA nyugati régiójában hello előtag 10.0.0.0/16 használata a 10.0.0.0/24 alhálózat hoz létre.

### <a name="step-3"></a>3. lépés

```powershell
$subnet = $vnet.subnets[0]
```

Ez a lépés hozzárendel hello alhálózati objektum toovariable $subnet hello további lépéseket.

## <a name="create-an-application-gateway-configuration-object"></a>Hozzon létre egy Application Gateway konfigurációs objektumot

### <a name="step-1"></a>1. lépés

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet
```

Ebben a lépésben létrehozza a "gatewayIP01" nevű alkalmazás átjáró IP-konfigurációt. Alkalmazásátjáró indításakor szerzi be hello alhálózati konfigurált IP-címeit, és útvonal-hello háttér IP-készletet a hálózati forgalom toohello IP-címek. Ne feledje, hogy minden példány egy IP-címet vesz fel.

### <a name="step-2"></a>2. lépés

```powershell
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 10.1.1.8,10.1.1.9,10.1.1.10
```

Ez a lépés konfigurál hello háttér IP-címkészlet neve "pool01" IP-címek "10.1.1.8, 10.1.1.9, 10.1.1.10". Most már az hello IP-címek, amelyek megkapják a hello a hálózati forgalom, elérhető lesz a hello előtér-IP-végponton. IP-címek tooadd megelőző hello lecseréli a saját alkalmazás IP-cím végpontokat.

### <a name="step-3"></a>3. lépés

```powershell
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled
```

Ez a lépés konfigurál alkalmazás átjáró beállítás "poolsetting01" hello terhelésre eloszlik hálózati forgalom hello háttér-készletben.

### <a name="step-4"></a>4. lépés

```powershell
$fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80
```

Ez a lépés konfigurál hello "frontendport01" nevű hello ILB az előtér-IP-port.

### <a name="step-5"></a>5. lépés

```powershell
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -Subnet $subnet
```

Ebben a lépésben hello "fipconfig01" nevű előtér-IP-konfiguráció hoz létre, és társítja azt egy privát IP-cím hello jelenlegi virtuális hálózati alhálózat.

### <a name="step-6"></a>6. lépés

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp
```

Ebben a lépésben létrehozza a "listener01" nevű hello figyelő, és hozzárendeli a hello előtér-port toohello előtér-IP-konfiguráció.

### <a name="step-7"></a>7. lépés

```powershell
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
```

Ebben a lépésben létrehoz hello terheléselosztási útválasztási szabály neve "rule01" által konfigurált házirendsablonok hello load balancer viselkedését.

### <a name="step-8"></a>8. lépés

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2
```

Ez a lépés konfigurál hello példányméretének hello Alkalmazásátjáró.

> [!NOTE]
> az alapértelmezett érték hello *InstanceCount* 2, maximális értéke 10. az alapértelmezett érték hello *GatewaySize* közepes. A Standard_Small, Standard_Medium és a Standard_Large lehetőségek közül választhat.

## <a name="create-an-application-gateway-by-using-new-azureapplicationgateway"></a>Application Gateway létrehozása a New-AzureApplicationGateway használatával

Az előző lépésekben hello összes konfigurációs elemet hoz létre a meglévő Alkalmazásátjáró. Ebben a példában a hello Alkalmazásátjáró "appgwtest" nevezik.

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku
```

Ez a lépés Alkalmazásátjáró az előző lépésekben hello összes konfigurációs elemet hoz létre. Hello a példában a hello Alkalmazásátjáró "appgwtest" nevezik.

## <a name="delete-an-application-gateway"></a>Application Gateway törlése

Alkalmazásátjáró toodelete, toodo hello lépések sorrendben kell:

1. Használjon hello `Stop-AzureRmApplicationGateway` parancsmag toostop hello átjáró.
2. Használjon hello `Remove-AzureRmApplicationGateway` parancsmag tooremove hello átjáró.
3. Győződjön meg arról, hogy hello átjáró el lett távolítva a hello `Get-AzureApplicationGateway` parancsmag.

### <a name="step-1"></a>1. lépés

Hello application gateway-objektum, és társítsa azt tooa változó "$getgw".

```powershell
$getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg
```

### <a name="step-2"></a>2. lépés

Használjon `Stop-AzureRmApplicationGateway` toostop hello Alkalmazásátjáró. Ez a példa bemutatja hello `Stop-AzureRmApplicationGateway` hello első sor parancsmag hello kimeneti követ.

```powershell
Stop-AzureRmApplicationGateway -ApplicationGateway $getgw  
```

```
VERBOSE: 9:49:34 PM - Begin Operation: Stop-AzureApplicationGateway
VERBOSE: 10:10:06 PM - Completed Operation: Stop-AzureApplicationGateway
Name       HTTP Status Code     Operation ID                             Error
----       ----------------     ------------                             ----
Successful OK                   ce6c6c95-77b4-2118-9d65-e29defadffb8
```

Ha hello Alkalmazásátjáró leállított állapotban van, a hello `Remove-AzureRmApplicationGateway` parancsmag tooremove hello szolgáltatást.

```powershell
Remove-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Force
```

```
VERBOSE: 10:49:34 PM - Begin Operation: Remove-AzureApplicationGateway
VERBOSE: 10:50:36 PM - Completed Operation: Remove-AzureApplicationGateway
Name       HTTP Status Code     Operation ID                             Error
----       ----------------     ------------                             ----
Successful OK                   055f3a96-8681-2094-a304-8d9a11ad8301
```

> [!NOTE]
> Hello **-force** kapcsoló lehet használt toosuppress hello eltávolítása jóváhagyást kérő üzenet.

tooverify, amely hello szolgáltatás el lett távolítva, használhatja a hello `Get-AzureRmApplicationGateway` parancsmag. Ez a lépés nem kötelező.

```powershell
Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg
```

```
VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

Get-AzureApplicationGateway : ResourceNotFound: hello gateway does not exist.
```

## <a name="next-steps"></a>Következő lépések

Ha azt szeretné, hogy az SSL-kiszervezés tooconfigure, [konfigurálása az SSL-kiszervezés Alkalmazásátjáró](application-gateway-ssl.md).

Ha azt szeretné, hogy egy alkalmazás átjáró toouse egy ILB rendelkező tooconfigure, [hozzon létre egy alkalmazást egy belső terheléselosztón (ILB)](application-gateway-ilb.md).

Ha további általános információra van szüksége a terheléselosztás beállításaival kapcsolatban:

* [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)

