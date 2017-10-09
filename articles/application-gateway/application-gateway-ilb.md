---
title: "Azure Application Gateway belső terheléselosztóval aaaUsing |} Microsoft Docs"
description: "Ezen a lapon nyújt útmutatást tooconfigure egy Azure Application Gateway egy belső elosztott terhelésű végponthoz"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 7403d28e-909f-46a2-b282-43a8e942f53c
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: gwallace
ms.openlocfilehash: 272ef84a02f92a8521c35aad6f1d9f9bf1675718
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-gateway-with-an-internal-load-balancer-ilb"></a>Alkalmazásátjáró létrehozása belső Load Balancerrel (ILB)

> [!div class="op_single_selector"]
> * [Klasszikus Azure PowerShell](application-gateway-ilb.md)
> * [Azure Resource Manager PowerShell](application-gateway-ilb-arm.md)

Alkalmazásátjáró Internet felé néző virtuális IP-cím vagy egy belső végpont nem lesz közzétéve toohello kell konfigurálni az internet, más néven belső Load Balancer (ILB) végpont. Belső üzleti alkalmazások közötti kapcsolat nincs felfedve toointernet hello átjáró konfigurálása egy ILB. Is célszerű a szolgáltatások vagy rétegek, egy többrétegű alkalmazást, amely az egy biztonsági határ nem lesz közzétéve toointernet helyezkedik el, de továbbra is szükséges a ciklikus multiplexelés terheléselosztási, a munkamenet tölcsérútvonalak vagy az SSL-lezárást belül. Ez a cikk bemutatja, hogyan hello lépéseket tooconfigure egy ILB az Alkalmazásátjáró.

## <a name="before-you-begin"></a>Előkészületek

1. Hello Azure PowerShell-parancsmaggal hello Webplatform-telepítő legújabb verziójának telepítéséhez. Töltse le, és telepítse hello legújabb verziót a hello **Windows PowerShell** hello szakasza [letöltési oldala](https://azure.microsoft.com/downloads/).
2. Győződjön meg arról, hogy rendelkezik-e egy működő virtuális alhálózattal rendelkező hálózatot érvényes.
3. Ellenőrizze, rendelkezik háttérkiszolgálók hello virtuális hálózatban, vagy egy nyilvános IP-cím/VIP hozzárendelve.

toocreate Alkalmazásátjáró, hajtsa végre a következő lépéseket a hello sorrendben hello. 

1. [Alkalmazásátjáró létrehozása](#create-a-new-application-gateway)
2. [Hello átjáró konfigurálása](#configure-the-gateway)
3. [Hello átjáró konfigurációjának beállítása](#set-the-gateway-configuration)
4. [Indítsa el a hello átjáró](#start-the-gateway)
5. [Hello átjáró ellenőrzése](#verify-the-gateway-status)

## <a name="create-an-application-gateway"></a>Alkalmazásátjáró létrehozása:

**toocreate hello átjáró**, használja a hello `New-AzureApplicationGateway` hello értékeket cserélje le a saját, a parancsmag. Vegye figyelembe, hogy számlázással hello átjáró nem indul el ezen a ponton. Számlázási egy későbbi lépésben, akkor kezdődik, amikor hello átjáró sikeresen elindult.

```powershell
New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")
```

```
VERBOSE: 4:31:35 PM - Begin Operation: New-AzureApplicationGateway 
VERBOSE: 4:32:37 PM - Completed Operation: New-AzureApplicationGateway
Name       HTTP Status Code     Operation ID                             Error 
----       ----------------     ------------                             ----
Successful OK                   55ef0460-825d-2981-ad20-b9a8af41b399
```

**toovalidate** , hogy hello átjáró lett létrehozva, használhatja a hello `Get-AzureApplicationGateway` parancsmag. 

Hello mintában *leírás*, *InstanceCount*, és *GatewaySize* opcionális paraméterek. az alapértelmezett érték hello *InstanceCount* 2, maximális értéke 10. az alapértelmezett érték hello *GatewaySize* közepes. Kis és nagy más elérhető értékek. *VIP* és *DnsName* jelennek meg az üres mert hello átjáró még nem kezdődött meg. Miután hello átjáró fut. hello jön létre. 

```powershell
Get-AzureApplicationGateway AppGwTest
```

```
VERBOSE: 4:39:39 PM - Begin Operation:
Get-AzureApplicationGateway VERBOSE: 4:39:40 PM - Completed 
Operation: Get-AzureApplicationGateway
Name: AppGwTest    
Description: 
VnetName: testvnet1 
Subnets: {Subnet-1} 
InstanceCount: 2 
GatewaySize: Medium 
State: Stopped 
VirtualIPs: 
DnsName:
```

## <a name="configure-hello-gateway"></a>Hello átjáró konfigurálása
Egy alkalmazás átjáró konfigurálása több érték áll. hello értékeket is kötődik együtt tooconstruct hello konfigurációs.

hello értékek a következők:

* **Kiszolgáló háttérkészlet:** IP-címek, a háttérkiszolgálók hello hello listáját. hello IP-címek felsorolt vagy kell tartoznia, mint toohello VNet subnet, vagy egy nyilvános IP-cím/VIP kell lennie. 
* **Háttérkiszolgáló-készlet beállításai:** Minden készlet rendelkezik olyan beállításokkal, mint a port, a protokoll vagy a cookie-alapú affinitás. Ezek a beállítások esetén tooa kapcsolt verem és a hello készlet alkalmazott tooall-kiszolgálók.
* **Az elülső rétegbeli portot:** Ez a port nem hello nyilvános portot nyit meg hello Alkalmazásátjáró. Forgalom találatok ezt a portot, és ezután lekérdezi a hello háttérkiszolgálók átirányított tooone.
* **Figyelő:** hello figyelő rendelkezik egy elülső rétegbeli portot, a protokollt (Http vagy Https, amelyek kis-és nagybetűket), és hello SSL tanúsítvány neve (ha az SSL beállításának-kiszervezés). 
* **Szabály:** hello szabály hello figyelő és a kiszolgáló háttérkészlet hello van kötve, és azt határozza meg, melyik háttér címkészletet hello forgalom irányított toowhen találatok száma a egy adott figyelő. Jelenleg csak hello *alapvető* szabály használata támogatott. Hello *alapvető* szabály-e időszeletelés terheléselosztási.

A konfigurációs hogyan hozhat létre, vagy hozzon létre egy konfigurációs objektumot, vagy egy konfigurációs XML-fájl használatával. tooconstruct egy konfigurációs XML-fájl, használjon hello segítségével a konfigurációs minta alatt.

Vegye figyelembe a következőket hello:

* Hello *frontendipconfiguration osztálya lehet* elem hello ILB részletek vonatkozó Alkalmazásátjáró konfigurálható egy ILB írja le. 
* hello előtér-IP- *típus* too'Private állítható be "
* Hello *StaticIPAddress* kell beállítani kívánt toohello belső IP-mely hello az átjáró fogadja a forgalmat. Vegye figyelembe, hogy hello *StaticIPAddress* elem nem kötelező megadni. Ha nincs olyan elérhető belső IP-címet a telepített hello alhálózatból be van állítva, van kiválasztva. 
* hello értékének hello *neve* elemben megadott *FrontendIPConfiguration* hello HTTPListener használandó *FrontendIP* elem toorefer toohello FrontendIPConfiguration.
  
  **Konfigurációs XML-minta**
```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
    <FrontendIPConfigurations>
        <FrontendIPConfiguration>
            <Name>fip1</Name> 
            <Type>Private</Type> 
            <StaticIPAddress>10.0.0.10</StaticIPAddress> 
        </FrontendIPConfiguration>
    </FrontendIPConfigurations>
    <FrontendPorts>
        <FrontendPort>
            <Name>FrontendPort1</Name>
            <Port>80</Port>
        </FrontendPort>
    </FrontendPorts>
    <BackendAddressPools>
        <BackendAddressPool>
            <Name>BackendPool1</Name>
            <IPAddresses>
                <IPAddress>10.0.0.1</IPAddress>
                <IPAddress>10.0.0.2</IPAddress>
            </IPAddresses>
        </BackendAddressPool>
    </BackendAddressPools>
    <BackendHttpSettingsList>
        <BackendHttpSettings>
            <Name>BackendSetting1</Name>
            <Port>80</Port>
            <Protocol>Http</Protocol>
            <CookieBasedAffinity>Enabled</CookieBasedAffinity>
        </BackendHttpSettings>
    </BackendHttpSettingsList>
    <HttpListeners>
        <HttpListener>
            <Name>HTTPListener1</Name>
            <FrontendIP>fip1</FrontendIP>
            <FrontendPort>FrontendPort1</FrontendPort>
            <Protocol>Http</Protocol>
        </HttpListener>
    </HttpListeners>
    <HttpLoadBalancingRules>
        <HttpLoadBalancingRule>
            <Name>HttpLBRule1</Name>
            <Type>basic</Type>
            <BackendHttpSettings>BackendSetting1</BackendHttpSettings>
            <Listener>HTTPListener1</Listener>
            <BackendAddressPool>BackendPool1</BackendAddressPool>
        </HttpLoadBalancingRule>
    </HttpLoadBalancingRules>
</ApplicationGatewayConfiguration>
```


## <a name="set-hello-gateway-configuration"></a>Hello átjáró konfigurációjának beállítása
A következő beállításokat a hello Alkalmazásátjáró. Használhatja a hello `Set-AzureApplicationGatewayConfig` parancsmag egy konfigurációs objektumot, vagy egy konfigurációs XML-fájlt. 

```powershell
Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile D:\config.xml
```

```
VERBOSE: 7:54:59 PM - Begin Operation: Set-AzureApplicationGatewayConfig 
VERBOSE: 7:55:32 PM - Completed Operation: Set-AzureApplicationGatewayConfig
Name       HTTP Status Code     Operation ID                             Error 
----       ----------------     ------------                             ----
Successful OK                   9b995a09-66fe-2944-8b67-9bb04fcccb9d
```

## <a name="start-hello-gateway"></a>Indítsa el a hello átjáró

Ha hello átjáró van konfigurálva, a hello `Start-AzureApplicationGateway` parancsmag toostart hello átjáró. Alkalmazásátjáró számlázás megkezdése után hello átjáró sikeresen elindult. 

> [!NOTE]
> Hello `Start-AzureApplicationGateway` parancsmag is igénybe vehet fel toocomplete too15-20 perc. 
> 
> 

```powershell
Start-AzureApplicationGateway AppGwTest 
```

```
VERBOSE: 7:59:16 PM - Begin Operation: Start-AzureApplicationGateway 
VERBOSE: 8:05:52 PM - Completed Operation: Start-AzureApplicationGateway
Name       HTTP Status Code     Operation ID                             Error 
----       ----------------     ------------                             ----
Successful OK                   fc592db8-4c58-2c8e-9a1d-1c97880f0b9b
```

## <a name="verify-hello-gateway-status"></a>Hello az átjáró állapotának megerősítése

Használjon hello `Get-AzureApplicationGateway` parancsmag toocheck hello átjáró állapotának. Ha `Start-AzureApplicationGateway` hello állapotban kell lennie sikeres hello előző lépésben, *futtató*, és a hello Vip DnsName kell állnia, és érvényes bejegyzések. Ez a példa bemutatja hello parancsmag hello első sor hello kimeneti követi. Ez a példa hello átjáró fut, és készen áll a tootake forgalom. 

> [!NOTE]
> hello Alkalmazásátjáró van konfigurálva tooaccept-forgalom zárása a hello 10.0.0.10 ILB végpontja konfigurált ebben a példában.

```powershell
Get-AzureApplicationGateway AppGwTest 
```

```
VERBOSE: 8:09:28 PM - Begin Operation: Get-AzureApplicationGateway 
VERBOSE: 8:09:30 PM - Completed Operation: Get-AzureApplicationGateway
Name          : AppGwTest
Description   : 
VnetName      : testvnet1
Subnets       : {Subnet-1}
InstanceCount : 2
GatewaySize   : Medium
State         : Running
VirtualIPs    : {10.0.0.10}
DnsName       : appgw-b2a11563-2b3a-4172-a4aa-226ee4c23eed.cloudapp.net
```

## <a name="next-steps"></a>Következő lépések
Ha további általános információra van szüksége a terheléselosztás beállításaival kapcsolatban:

* [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)

