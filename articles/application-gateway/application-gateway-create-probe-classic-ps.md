---
title: "egyéni tesztműveleti - Azure Application Gateway - PowerShell klasszikus aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egyéni mintavételi az Alkalmazásátjáró hello klasszikus üzembe helyezési modellel PowerShell használatával"
services: application-gateway
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 338a7be1-835c-48e9-a072-95662dc30f5e
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: gwallace
ms.openlocfilehash: 68332367c99328bd6456b0c339923765637be986
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-custom-probe-for-azure-application-gateway-classic-by-using-powershell"></a>Hozzon létre egy egyéni mintavétel az Azure Application Gateway (klasszikus) PowerShell használatával

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-probe-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-probe-ps.md)
> * [Klasszikus Azure PowerShell](application-gateway-create-probe-classic-ps.md)

Ebben a cikkben egy egyéni mintavételi tooan meglévő Alkalmazásátjáró a PowerShell használatával adja hozzá. Egyéni mintavételt hasznosak, az alkalmazásokat, amelyek egy adott állapotának ellenőrzése lapon vagy az alkalmazásokat, amelyek nem a sikeres válasz hello alapértelmezett webalkalmazáshoz.

> [!IMPORTANT]
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja. Ismerje meg, hogyan túl[hello Resource Manager modellt használja a következő lépésekkel](application-gateway-create-probe-ps.md).

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-an-application-gateway"></a>Application Gateway létrehozása

Alkalmazásátjáró toocreate:

1. Egy Application Gateway erőforrás létrehozása.
2. Hozzon létre egy konfigurációs XML-fájlt vagy konfigurációs objektumot.
3. Az újonnan létrehozott alkalmazás átjáró erőforrás hello konfigurációs toohello véglegesítése.

### <a name="create-an-application-gateway-resource-with-a-custom-probe"></a>Egy egyéni mintavételi alkalmazás átjáró erőforrás létrehozása

toocreate hello átjáró használata hello `New-AzureApplicationGateway` hello értékeket cserélje le a saját, a parancsmag. Számlázási hello átjáró nem indul el ezen a ponton. Számlázási egy későbbi lépésben, akkor kezdődik, amikor hello átjáró sikeresen elindult.

hello alábbi példa használatával hozza létre az Alkalmazásátjáró nevű, "testvnet1" és "alhálózat-1" nevű alhálózat virtuális hálózat.

```powershell
New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")
```

amely átjáró hello toovalidate lett létrehozva, használhatja a hello `Get-AzureApplicationGateway` parancsmag.

```powershell
Get-AzureApplicationGateway AppGwTest
```

> [!NOTE]
> az alapértelmezett érték hello *InstanceCount* 2, maximális értéke 10. az alapértelmezett érték hello *GatewaySize* közepes. Kis, közepes és nagy közül választhat.
> 
> 

*Virtualip értékek* és *DnsName* jelennek meg az üres mert hello átjáró még nem kezdődött meg. Ezeket az értékeket jönnek létre, ha hello átjáró hello futó állapotban van.

### <a name="configure-an-application-gateway-by-using-xml"></a>Alkalmazásátjáró konfigurálása XML használatával

A következő példa hello egy XML-fájl tooconfigure összes alkalmazás átjáró beállításait használja, és véglegesítse azokat toohello alkalmazás átjáró-erőforráshoz.  

A következő szöveg tooNotepad hello másolja.

```xml
<ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
<FrontendIPConfigurations>
    <FrontendIPConfiguration>
        <Name>fip1</Name>
        <Type>Private</Type>
    </FrontendIPConfiguration>
</FrontendIPConfigurations>
<FrontendPorts>
    <FrontendPort>
        <Name>port1</Name>
        <Port>80</Port>
    </FrontendPort>
</FrontendPorts>
<Probes>
    <Probe>
        <Name>Probe01</Name>
        <Protocol>Http</Protocol>
        <Host>contoso.com</Host>
        <Path>/path/custompath.htm</Path>
        <Interval>15</Interval>
        <Timeout>15</Timeout>
        <UnhealthyThreshold>5</UnhealthyThreshold>
    </Probe>
    </Probes>
    <BackendAddressPools>
    <BackendAddressPool>
        <Name>pool1</Name>
        <IPAddresses>
            <IPAddress>1.1.1.1</IPAddress>
            <IPAddress>2.2.2.2</IPAddress>
        </IPAddresses>
    </BackendAddressPool>
</BackendAddressPools>
<BackendHttpSettingsList>
    <BackendHttpSettings>
        <Name>setting1</Name>
        <Port>80</Port>
        <Protocol>Http</Protocol>
        <CookieBasedAffinity>Enabled</CookieBasedAffinity>
        <RequestTimeout>120</RequestTimeout>
        <Probe>Probe01</Probe>
    </BackendHttpSettings>
</BackendHttpSettingsList>
<HttpListeners>
    <HttpListener>
        <Name>listener1</Name>
        <FrontendIP>fip1</FrontendIP>
    <FrontendPort>port1</FrontendPort>
        <Protocol>Http</Protocol>
    </HttpListener>
</HttpListeners>
<HttpLoadBalancingRules>
    <HttpLoadBalancingRule>
        <Name>lbrule1</Name>
        <Type>basic</Type>
        <BackendHttpSettings>setting1</BackendHttpSettings>
        <Listener>listener1</Listener>
        <BackendAddressPool>pool1</BackendAddressPool>
    </HttpLoadBalancingRule>
</HttpLoadBalancingRules>
</ApplicationGatewayConfiguration>
```

Módosíthatja a hello konfigurációs elemek hello zárójelek között hello értékeket. Bővítmény .xml hello fájl mentése.

hello következő példa bemutatja, hogyan toouse egy konfigurációs fájl tooset hello alkalmazás átjáró tooload be HTTP-forgalom 80-as nyilvános portjához egyensúlyba és a hálózati forgalom elküldése a 80-as port tooback-a befejezési egy egyéni mintavételt használatával két IP-címre.

> [!IMPORTANT]
> hello protokoll Http vagy Https elem kis-és nagybetűket.

Egy új konfigurációelemet \<mintavételi\> tooconfigure egyéni mintavételt kerül.

hello konfigurációs paraméterek a következők:

|Paraméter|Leírás|
|---|---|
|**Name (Név)** |Egyéni mintavétel hivatkozás neve. |
* **Protokoll** | Használt protokoll (a lehetséges értékek: HTTP vagy HTTPS).|
| **Állomás** és **elérési útja** | Végezze el, amelyet hello alkalmazás átjáró toodetermine hello állapotának hello példány URL-címe. Például ha egy webhely http://contoso.com/ majd hello egyéni mintavételi konfigurálhatja a "http://contoso.com/path/custompath.htm" mintavétel toohave sikeres HTTP-válasz ellenőrzi.|
| **Időköz** | Konfigurálja a hello mintavételi időköze ellenőrzések másodpercben.|
| **Időtúllépés** | Határozza meg egy HTTP-válasz ellenőrzést hello mintavételi időkorlátját.|
| **UnhealthyThreshold** | tooflag hello háttér-példány szükséges a sikertelen HTTP-válaszok száma hello *sérült*.|

hello mintavétel nevének hivatkozik hello \<BackendHttpSettings\> konfigurációs tooassign egyéni tesztműveleti beállításokat használja, mely háttér-készlet.

## <a name="add-a-custom-probe-tooan-existing-application-gateway"></a>Egyéni tesztműveleti tooan meglévő Alkalmazásátjáró hozzáadása

Változó hello aktuális konfigurációja Alkalmazásátjáró három lépésből áll: hello aktuális XML konfigurációs fájl beolvasása módosítani egy egyéni mintavételt toohave és hello Alkalmazásátjáró hello új XML-beállításainak konfigurálása.

1. Hello XML-fájl segítségével könnyebben nyerhet `Get-AzureApplicationGatewayConfig`. Ez a parancsmag kivitel hello konfigurációs XML toobe módosítani tooadd mintavételi beállítást.

  ```powershell
  Get-AzureApplicationGatewayConfig -Name "<application gateway name>" -Exporttofile "<path toofile>"
  ```

1. Nyissa meg a hello XML-fájlt egy szövegszerkesztőben. Adja hozzá a `<probe>` után szakasz `<frontendport>`.

  ```xml
<Probes>
    <Probe>
        <Name>Probe01</Name>
        <Protocol>Http</Protocol>
        <Host>contoso.com</Host>
        <Path>/path/custompath.htm</Path>
        <Interval>15</Interval>
        <Timeout>15</Timeout>
        <UnhealthyThreshold>5</UnhealthyThreshold>
    </Probe>
</Probes>
  ```

  Hello XML hello backendhttpsettings beállítások területen hozzáadása hello mintavétel nevének, ahogy az alábbi példa hello:

  ```xml
    <BackendHttpSettings>
        <Name>setting1</Name>
        <Port>80</Port>
        <Protocol>Http</Protocol>
        <CookieBasedAffinity>Enabled</CookieBasedAffinity>
        <RequestTimeout>120</RequestTimeout>
        <Probe>Probe01</Probe>
    </BackendHttpSettings>
  ```

  Hello XML-fájl mentése.

1. Frissítés hello alkalmazás átjáró konfigurációját hello új XML-fájl használatával `Set-AzureApplicationGatewayConfig`. Ez a parancsmag frissíti az Alkalmazásátjáró hello új konfigurációval.

```powershell
Set-AzureApplicationGatewayConfig -Name "<application gateway name>" -Configfile "<path toofile>"
```

## <a name="next-steps"></a>Következő lépések

Ha azt szeretné, hogy Secure Sockets Layer (SSL)-kiszervezés tooconfigure, [konfigurálása az SSL-kiszervezés Alkalmazásátjáró](application-gateway-ssl.md).

Ha azt szeretné, hogy egy alkalmazás átjáró toouse belső terheléselosztással tooconfigure, [hozzon létre egy alkalmazást egy belső terheléselosztón (ILB)](application-gateway-ilb.md).

