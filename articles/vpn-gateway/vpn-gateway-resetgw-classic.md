---
title: "Az Azure VPN gateway tooreestablish IPsec bújtatja alaphelyzetbe állítása |} Microsoft Docs"
description: "Ez a cikk végigvezeti az Azure VPN Gateway tooreestablish IPsec-alagutak alaphelyzetbe állítását. hello cikk tooVPN átjárók klasszikus hello és hello Resource Manager üzembe helyezési modellel vonatkozik."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 79d77cb8-d175-4273-93ac-712d7d45b1fe
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/24/2017
ms.author: cherylmc
ms.openlocfilehash: 84dd741f0bebd6b18cb235216a68a88da5fe17b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="reset-a-vpn-gateway"></a>VPN-átjáró alaphelyzetbe állítása

Az Azure VPN Gateway alaphelyzetbe állítása akkor hasznos, ha egy vagy több helyek közötti VPN-alagúton elveszíti a létesítmények közötti VPN-kapcsolatot. Ebben a helyzetben a helyszíni VPN-eszközök minden megfelelően működik-e, de nem tudta tooestablish hello Azure VPN gatewayek az IPsec-alagutak. Ez a cikk segítséget nyújt a VPN-átjárót alaphelyzetbe.

### <a name="what-happens-during-a-reset"></a>Mi történik, amikor egy alaphelyzetbe áll?

VPN-átjáró két Virtuálisgép-példány fut egy aktív-készenléti állapotban lévő konfigurációs tevődik össze. Hello átjáró alaphelyzetbe állításakor újraindítja hello átjáró, és ezután újra alkalmazza hello létesítmények közötti konfigurációk tooit. hello átjáró tartja hello már rendelkezik nyilvános IP-cím. Ez azt jelenti, hogy nincs szükség Azure VPN Gateway tooupdate hello VPN-útválasztó konfigurációja egy új nyilvános IP-címmel.

Hello parancs tooreset hello átjáró elküldésekor hello jelenlegi aktív példány hello Azure VPN-átjáró azonnali újraindítása után. Lesz rövid hiány hello aktív példányból (újraindítása folyamatban), toohello készenléti állapotban lévő példány hello feladatátvétel során. hello gap egy percnél kevesebb kell lennie.

Hello kapcsolat hello első újraindítás után nem állítja vissza, ha a probléma hello azonos újra tooreboot hello második Virtuálisgép-példány (hello új aktív átjáró) parancsot. Ha két újraindításra hello kért hátsó tooback, nem lesznek némileg hosszabb ideig ahol mindkét Virtuálisgép-példányok (aktív és készenléti) újraindítása folyamatban van. Ennek hatására hosszabb hiány hello VPN-kapcsolatot, too2 too4 percig, virtuális gépek toocomplete hello vesznek fel a.

Után két újraindításra Ha olyan továbbra is problémákat tapasztal, létesítmények közötti kapcsolatot, nyisson egy támogatási kérést hello Azure-portálon a.

## <a name="before"></a>Előkészületek

Az átjáró visszaállítása előtt ellenőrizze a minden IPsec-webhelyek (közötti S2S) VPN-alagút az alább felsorolt hello kulcsfontosságú tételekről. Bármely eltért elemek hello jár S2S VPN-alagutat hello bontja a kapcsolatot. Ellenőrzése és javítása a helyszíni és az Azure VPN gatewayek hello konfigurációi menti, szükségtelen újraindítások, és a szolgáltatások hello más működő kapcsolatok hello átjárókon.

Ellenőrizze a következő elemeket az átjárót alaphelyzetbe állítás előtt hello:

* hello Internet IP-címek (VIP) mindkét hello Azure VPN Gateway és hello a helyszíni VPN-átjáró helyesen vannak-e konfigurálva mindkét hello Azure és a hello a helyszíni VPN-szabályzatok.
* hello előre megosztott kulcs ugyanazt az Azure és a helyszíni VPN-átjáróként kell hello.
* Ha IPsec/IKE konfigurációs, például a titkosításhoz, a kivonatoló algoritmusok és a PFS (teljes továbbítási titkosítása), győződjön meg arról, mindkettő hello Azure és a helyszíni VPN-átjárók rendelkeznek hello ugyanazokat a konfigurációkat.

## <a name="portal"></a>Azure-portálon

Alaphelyzetbe állíthatja a Resource Manager VPN-átjáró hello Azure-portál használatával. Ha azt szeretné, hogy a klasszikus átjáró tooreset, tekintse meg a hello [PowerShell](#resetclassic) lépéseket.

### <a name="resource-manager-deployment-model"></a>Resource Manager-alapú üzemi modell

1. Nyissa meg hello [Azure-portálon](https://portal.azure.com) , és keresse meg, hogy szeretné-e tooreset toohello erőforrás-kezelő virtuális hálózati átjáró.
2. Hello virtuális hálózati átjáró hello paneljén kattintson az "Új".

  ![Alaphelyzetbe állítja a VPN-átjáró panel](./media/vpn-gateway-howto-reset-gateway/reset-vpn-gateway-portal.png)
3. A hello panel visszaállítása parancsra hello **alaphelyzetbe** gombra.

## <a name="ps"></a>PowerShell

### <a name="resource-manager-deployment-model"></a>Resource Manager-alapú üzemi modell

hello parancsmag, az átjárót alaphelyzetbe állítása **alaphelyzetbe állítása-AzureRmVirtualNetworkGateway**. A beállítások visszaállítása elvégzése előtt győződjön meg arról, hogy a legújabb verziójának hello hello [Resource Manager PowerShell-parancsmagok](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.0.0). hello alábbi példa alaphelyzetbe állítja a virtuális hálózati átjáró VNet1GW nevű hello TestRG1 erőforráscsoportban:

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name VNet1GW -ResourceGroup TestRG1
Reset-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw
```

Eredmény:

Amikor megjelenik a visszaadandó eredmény, akkor feltételezheti, hello átjáró alaphelyzetbe állítása sikeres volt. Azonban nincs semmi hello visszaadott eredmény, amely explicit módon, hogy hello alaphelyzetbe állítása sikeres volt. Ha azt szeretné, hogy szorosan: hello előzmények toosee toolook pontosan amikor hello átjáró alaphelyzetbe lépett fel, megtekintheti ezt az információt a hello [Azure-portálon](https://portal.azure.com). Hello portálon lépjen túl**(GatewayName) -> erőforrás állapotának**.

### <a name="resetclassic"></a>Klasszikus telepítési modell

hello parancsmag, az átjárót alaphelyzetbe állítása **alaphelyzetbe állítása-AzureVNetGateway**. A beállítások visszaállítása elvégzése előtt győződjön meg arról, hogy a legújabb verziójának hello hello [Service Management (SM) PowerShell-parancsmagok](https://docs.microsoft.com/powershell/azure/install-azure-ps?view=azuresmps-3.7.0). hello alábbi példa visszaállítja egy "ContosoVNet" nevű virtuális hálózati átjáró hello:

```powershell
Reset-AzureVNetGateway –VnetName “ContosoVNet”
```

Eredmény:

```powershell
Error          :
HttpStatusCode : OK
Id             : f1600632-c819-4b2f-ac0e-f4126bec1ff8
Status         : Successful
RequestId      : 9ca273de2c4d01e986480ce1ffa4d6d9
StatusCode     : OK
```

## <a name="cli"></a>Az Azure parancssori felület

tooreset hello átjáró használata hello [az hálózati vnet-átjáró alaphelyzetbe](https://docs.microsoft.com/cli/azure/network/vnet-gateway#reset) parancsot. hello alábbi példa alaphelyzetbe állítja a virtuális hálózati átjáró VNet5GW nevű hello TestRG5 erőforráscsoportban:

```azurecli
az network vnet-gateway reset -n VNet5GW -g TestRG5
```

Eredmény:

Amikor megjelenik a visszaadandó eredmény, akkor feltételezheti, hello átjáró alaphelyzetbe állítása sikeres volt. Azonban nincs semmi hello visszaadott eredmény, amely explicit módon, hogy hello alaphelyzetbe állítása sikeres volt. Ha azt szeretné, hogy szorosan: hello előzmények toosee toolook pontosan amikor hello átjáró alaphelyzetbe lépett fel, megtekintheti ezt az információt a hello [Azure-portálon](https://portal.azure.com). Hello portálon lépjen túl**(GatewayName) -> erőforrás állapotának**.
