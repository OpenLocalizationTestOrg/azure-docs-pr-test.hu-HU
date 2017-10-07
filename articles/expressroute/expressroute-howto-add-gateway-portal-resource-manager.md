---
title: "Vegyen fel egy virtuális hálózati átjáró tooa VNet ExpressRoute: Portal: Azure |} Microsoft Docs"
description: "Ez a cikk végigvezeti a már létrehozott erőforrás-kezelő virtuális hálózaton, ExpressRoute a virtuális hálózati átjáró tooan hozzáadása."
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/17/2017
ms.author: cherylmc
ms.openlocfilehash: 9e922af1f3676eeebc569b57c3ae3a22d4e0b395
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-virtual-network-gateway-for-expressroute-using-hello-azure-portal"></a>A virtuális hálózati átjáró konfigurálása ExpressRoute hello Azure-portál használatával
> [!div class="op_single_selector"]
> * [Resource Manager – Azure Portal](expressroute-howto-add-gateway-portal-resource-manager.md)
> * [Resource Manager – PowerShell](expressroute-howto-add-gateway-resource-manager.md)
> * [Klasszikus - PowerShell](expressroute-howto-add-gateway-classic.md)
> * [Video - Azure-portálon](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network)
> 
> 

Ez a cikk bemutatja, hogyan hello lépéseket tooadd a virtuális hálózati átjáró egy már meglévő vnet. Ez a cikk bemutatja, hogyan hello lépéseket tooadd, méretezze át, és távolítsa el a virtuális hálózathoz (VNet) átjáró egy már meglévő vnet. hello ehhez a konfigurációhoz lépésekre kifejezetten a Vnetek hello Resource Manager telepítési modell ExpressRoute konfigurációban használt használatával létrehozott. További információ a virtuális hálózati átjárók és az átjáró konfigurációs beállításainak ExpressRoute: [kapcsolatos az ExpressRoute virtuális hálózati átjárók](expressroute-about-virtual-network-gateways.md). 


## <a name="before-beginning"></a>Mielőtt hozzálát

Ez a feladat használható egy Vnetet hello lépéseket a következő konfigurációs hivatkozáslista hello hello értékei alapján. Ebben a listában a példa lépései használjuk. Másolhatja hello lista toouse referenciaként hello értékeket cserélje le a saját.

**Konfigurációs hivatkozáslista**

* Virtuális hálózati név = "TestVNet"
* Virtuális hálózati címterület = 192.168.0.0/16
* Alhálózati név = "Előtér" 
    * Alhálózati címtartományt = "192.168.1.0/24"
* Erőforráscsoport = "TestRG"
* Hely = "USA keleti régiója"
* Átjáró alhálózati név: "GatewaySubnet" mindig neve egy átjáró-alhálózatot kell *GatewaySubnet*.
    * Átjáró alhálózati címtartományt = "192.168.200.0/26"
* Átjáró Name = "ERGW"
* Átjáró IP-név = "MyERGWVIP"
* Átjáró típusa = "ExpressRoute" Ez a típus egy ExpressRoute-konfiguráció szükséges.
* Átjáró nyilvános IP-név = "MyERGWVIP"

Megtekintheti a [videó](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network) , mielőtt megkezdené a konfigurációs lépéseket.

## <a name="create-hello-gateway-subnet"></a>Hozzon létre hello átjáró-alhálózatot

1. A hello [portal](http://portal.azure.com), keresse meg a toohello erőforrás-kezelő virtuális hálózati legyen a virtuális hálózati átjáró toocreate.
2. A hello **beállítások** szakasz a hálózatok paneljén kattintson **alhálózatok** tooexpand hello (alhálózatok) panel.
3. A hello **alhálózatok** panelen kattintson a **+ átjáróalhálózatot** tooopen hello **alhálózat hozzáadása** panelen. 
   
    ![Hello átjáró alhálózatának hozzáadása](./media/expressroute-howto-add-gateway-portal-resource-manager/addgwsubnet.png "hello átjáró alhálózatának hozzáadása")


4. Hello **neve** az alhálózat hello automatikusan felveszi a "GatewaySubnet" értéket. Ezt az értéket ahhoz, hogy az Azure toorecognize hello alhálózati hello átjáró alhálózatának minimális. Állítsa be az automatikusan kitöltött hello **-címtartományt** értékek toomatch a konfigurációs követelmények. Javasoljuk, hogy egy átjáróalhálózat létrehozásához, egy /27 vagy nagyobb (26, / / 25, stb.). Kattintson a **OK** toosave hello értékeket, és hozzon létre hello átjáró-alhálózatot.

    ![Hello alhálózat hozzáadása](./media/expressroute-howto-add-gateway-portal-resource-manager/addsubnetgw.png "hello alhálózat hozzáadása")

## <a name="create-hello-virtual-network-gateway"></a>Hello virtuális hálózati átjáró létrehozása

1. Hello portálon hello bal oldalon kattintson ** + ** , és írja be a "Virtuális hálózati átjáró" keresésben. Keresse meg **virtuális hálózati átjáró** hello keresés térjen vissza, és kattintson a hello bejegyzés. A hello **virtuális hálózati átjáró** panelen kattintson a **létrehozása** hello hello panel alsó részén. Ekkor megnyílik a hello **virtuális hálózati átjáró létrehozása** panelen.
2. A hello **virtuális hálózati átjáró létrehozása** panelen adja meg a hello értékek a virtuális hálózati átjáró.

    ![Virtuális hálózati átjáró létrehozása panel mezői](./media/expressroute-howto-add-gateway-portal-resource-manager/gw.png "Virtuális hálózati átjáró létrehozása panel mezői")
3. **Név**: adjon nevet az átjárónak. A rendszer nem hello ugyanaz, mint egy átjáró alhálózatának elnevezése. Azt a hello átjáró objektum létrehozásakor hello nevét.
4. **Átjáró típusa**: válasszon **ExpressRoute**.
5. **SKU**: Select hello gateway SKU hello legördülő menüből.
6. **Hely**: hello beállítása **hely** mezőben toopoint toohello helyet, ahol a virtuális hálózat található. Hello helye nem mutat toohello régió, ahol a virtuális hálózaton található, ha a virtuális hálózati hello nem jelenik meg a hello "Válasszon egy virtuális hálózati" legördülő lista.
7. Válassza ki a virtuális hálózati toowhich hello szeretné tooadd ezt az átjárót. Kattintson a **virtuális hálózati** tooopen hello **virtuális hálózatot választ** panelen. Válassza ki a virtuális hálózat hello. Ha nem látja a virtuális hálózat, győződjön meg arról, hogy hello **hely** mező mutat toohello régió, ahol a virtuális hálózat is található.
9. Válasszon egy nyilvános IP-címet. Kattintson a **nyilvános IP-cím** tooopen hello **nyilvános IP-cím kiválasztása** panelen. Kattintson a **+ új** tooopen hello **létrehozás nyilvános IP-cím panel**. Adjon egy nevet a nyilvános IP-címnek. Ezen a panelen hoz létre egy nyilvános IP cím objektum toowhich egy nyilvános IP-cím dinamikusan rendeli. Kattintson a **OK** toosave a módosítások toothis panelen.
10. **Előfizetés**: Győződjön meg arról, hogy hello megfelelő előfizetés van kijelölve.
11. **Erőforráscsoport**: Ezzel a beállítással hello virtuális hálózat határozza meg.
12. Ne állítsa be a hello **hely** követően hello korábbi beállításokat.
13. Ellenőrizze a hello beállításait. Ha azt szeretné, hogy az átjáró tooappear hello irányítópultra, válassza **PIN-kód toodashboard** hello hello panel alsó részén.
14. Kattintson a **létrehozása** toobegin hello átjáró létrehozásához. hello-beállítások érvényesítése és hello átjáró telepíti. Virtuális hálózati átjáró létrehozása akár is igénybe vehet too45 perc toocomplete.

## <a name="next-steps"></a>Következő lépések
Hello hálózatok átjáró létrehozása után a virtuális hálózat tooan ExpressRoute-kapcsolatcsoportot társíthatja. Lásd: [csatolni a virtuális hálózati tooan ExpressRoute-kapcsolatcsoportot](expressroute-howto-linkvnet-portal-resource-manager.md).
