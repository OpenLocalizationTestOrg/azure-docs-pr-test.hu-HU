---
title: "Klasszikus virtuális hálózatok csatlakoztatása Azure Resource Manager Vnetek: portál |} Microsoft Docs"
description: "Útmutató klasszikus virtuális hálózatokat és erőforrás-kezelő Vnetek VPN-átjáró és a portál használatával közötti VPN-kapcsolat létrehozása"
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 5a90498c-4520-4bd3-a833-ad85924ecaf9
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/21/2017
ms.author: cherylmc
ms.openlocfilehash: 1b7b67ec28986b7c20b3e990e3565265f74c28e6
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="connect-virtual-networks-from-different-deployment-models-using-the-portal"></a>Virtuális hálózatok csatlakoztatása a különböző üzembe helyezési modellel, a portál használatával

Ez a cikk bemutatja, hogyan csatlakozzon a hagyományos Vneteket erőforrás-kezelő Vnetek engedélyezi a különálló üzembe helyezési modellel egymással kommunikálni található erőforrásokhoz. A cikkben ismertetett elsősorban az Azure-portálon, de ez a konfiguráció a cikk jelöl ki a listában a PowerShell használatával is létrehozhat.

> [!div class="op_single_selector"]
> * [Portal](vpn-gateway-connect-different-deployment-models-portal.md)
> * [PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)
> 
> 

A klasszikus virtuális hálózat csatlakozik egy erőforrás-kezelő virtuális hálózat hasonlít egy Vnetet csatlakozik egy helyszíni hely. Mindkét kapcsolattípus egy VPN-átjárót használ a biztonságos alagút IPsec/IKE használatával való kialakításához. Létrehozhat, amelyek különböző előfizetésekhez és különböző régiókban Vnetek közötti kapcsolat. A helyszíni hálózatokhoz való csatlakozás már rendelkező Vnetek mindaddig, amíg az átjáró, amely a konfigurálva a dinamikus- vagy útválasztó-alapú is csatlakoztathatja. A virtuális hálózatok közötti kapcsolatokról további információt a cikk végén, a [Virtuális hálózatok közötti kapcsolat – gyakori kérdések](#faq) című részben talál. 

Ha a Vnetek ugyanabban a régióban, érdemes lehet helyette megfontolandó csatlakoztatja őket a Vnetben társviszony-létesítés használatával. A virtuális hálózatok közötti társviszony nem használ VPN-átjárót. További információ: [Társviszony létesítése virtuális hálózatok között](../virtual-network/virtual-network-peering-overview.md). 

### <a name="prerequisites"></a>Előfeltételek

* Ezek a lépések feltételezik, hogy mindkét Vnetek létre lett hozva. Ha egy a gyakorlatban ez a cikk használ, és nem rendelkezik a Vneteket, nincsenek hivatkozásokat hozhat létre, azokat a lépéseket.
* Győződjön meg arról, hogy a Vnetek címtartományát nem lehetnek átfedésben vannak egymással, vagy átfedésben vannak a tartományt az átjárók csatlakoztathatók más kapcsolatok esetében.
* Telepítse a legújabb PowerShell-parancsmagokat a Resource Manager és a Service Management (klasszikus). Ez a cikk használjuk, az Azure portál és a PowerShell. A kapcsolat létrehozása a klasszikus virtuális hálózatot az erőforrás-kezelő virtuális hálózat szükséges PowerShell. További információt [az Azure PowerShell telepítésével és konfigurálásával](/powershell/azure/overview) foglalkozó témakörben talál. 

### <a name="values"></a>Példabeállítások

Ezekkel az értékekkel létrehozhat egy tesztkörnyezetet, vagy a segítségükkel értelmezheti a cikkben szereplő példákat.

**Klasszikus virtuális hálózaton**

Virtuális hálózat neve = ClassicVNet <br>
Címtér = 10.0.0.0/24 <br>
Alhálózat-1 = 10.0.0.0/27 <br>
Erőforráscsoport = ClassicRG <br>
Hely = USA nyugati régiója <br>
A GatewaySubnet = 10.0.0.32/28 <br>
Helyi = RMVNetLocal <br>

**Erőforrás-kezelő virtuális hálózat**

Virtuális hálózat neve = RMVNet <br>
Címtér = 192.168.0.0/16 <br>
Alhálózat-1 = 192.168.1.0/24 <br>
A GatewaySubnet = 192.168.0.0/26 <br>
Erőforráscsoport = RG1 <br>
Hely = USA keleti régiója <br>
Virtuális hálózati átjáró neve = RMGateway <br>
Átjáró típusa = VPN <br>
VPN-típus útválasztó-alapú = <br>
Átjáró nyilvános IP-cím neve = rmgwpip <br>
Helyi hálózati átjáró = ClassicVNetLocal <br>
Kapcsolat neve = RMtoClassic

### <a name="connection-overview"></a>Kapcsolat – áttekintés

Ebben a konfigurációban az a virtuális hálózatok közötti IPsec/IKE VPN-alagúton keresztül hoz létre VPN gateway-kapcsolattal. Győződjön meg arról, hogy a virtuális hálózat tartományok egyike átfedésben vannak egymással, vagy egy helyi hálózaton, amelyhez csatlakoznak.

A következő táblázat a példa Vnetek és helyi helyek definiálásának módját példáját mutatja be:

| Virtual Network | Címterület | Régió | Csatlakozik a helyi hálózati telephely |
|:--- |:--- |:--- |:--- |
| ClassicVNet |(10.0.0.0/24) |USA nyugati régiója | RMVNetLocal (192.168.0.0/16) |
| RMVNet | (192.168.0.0/16) |USA keleti régiója |ClassicVNetLocal (10.0.0.0/24) |

## <a name="classicvnet"></a>1. A klasszikus virtuális hálózat beállításainak konfigurálása

Ebben a szakaszban hoz létre a helyi hálózati (helyi) és a klasszikus virtuális hálózat a virtuális hálózati átjáró. Ha a klasszikus virtuális hálózat nem rendelkezik, és ezeket a lépéseket egy gyakorlatot fussanak, is létrehozhat egy virtuális hálózat használatával [Ez a cikk](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) és a [példa](#values) beállítások értékeket felülről.

Amikor a portál segítségével hozzon létre egy klasszikus virtuális hálózatot, akkor kell megkeresnie és kijelölnie a virtuális hálózat panel az alábbi lépéseket követve, ellenkező esetben a klasszikus virtuális hálózat létrehozása a beállítás nem jelenik meg:

1. Kattintson a "+" a "New" panel megnyitásához.
2. A "Keresése a piactéren" mezőben írja be a "Virtuális hálózat". Ha ehelyett hálózatkezelés virtuális hálózat ->, létrehozhat egy klasszikus virtuális hálózat nem fog.
3. Keresse meg a "Virtuális hálózat" a visszaadott listából, és kattintson arra a virtuális hálózat panel megnyitásához. 
4. A virtuális hálózati panelen válassza ki a "Klasszikus" klasszikus VNet létrehozása. 

Ha már van egy virtuális hálózat VPN-átjáróval, győződjön meg arról, hogy az átjáró dinamikus. Ha statikus, kell először törli a VPN-átjárót, majd a folytatáshoz.

A képernyőképek csak példaként szolgálnak. Ügyeljen arra, hogy cserélje le az értékeket a saját, vagy használja a [példa](#values) értékeket.

### <a name="part-1---configure-the-local-site"></a>1. rész - annak konfigurálása, a helyi webhelyhez

Nyissa meg a [Azure-portálon](https://ms.portal.azure.com) és jelentkezzen be az Azure-fiókjával.

1. Navigáljon a **összes erőforrás** , és keresse meg a **ClassicVNet** a listában.
2. Az a **áttekintése** panelen, a a **VPN-kapcsolatok** területen kattintson a **átjáró** kép átjáró létrehozásához.

    ![Konfigurálja a VPN-átjáró](./media/vpn-gateway-connect-different-deployment-models-portal/gatewaygraphic.png "VPN-átjáró konfigurálása")
3. Az a **új VPN-kapcsolat** panelen a **kapcsolattípus**, jelölje be **pont-pont**.
4. A **helyi**, kattintson a **kötelező beállítások konfigurálása**. Ekkor megnyílik a **helyi** panelen.
5. Az a **helyi** panelen, egy nevet az alkalmazásfüggőségek az erőforrás-kezelő virtuális hálózat létrehozása. Például "RMVNetLocal."
6. Ha a VPN-átjáró a Resource Manager virtuális hálózat már van egy nyilvános IP-címet használja az értékét a **VPN-átjáró IP-címet** mező. Ha ezek a lépések végrehajtása egy gyakorlat szerint, vagy még nem rendelkezik a virtuális hálózati átjáró az erőforrás-kezelő vnet, hogy a helyőrző IP-címeit. Gondoskodjon arról, hogy, hogy a helyőrző IP-cím formátuma érvénytelen. Később a helyőrző IP-cím cserélje a Resource Manager virtuális hálózati átjáró nyilvános IP-címét.
7. A **ügyfél Címterület**, a virtuális hálózati IP-címteret a Resource Manager vnet értékeit használja. Ezzel a beállítással adhatja meg a címterek, az erőforrás-kezelő virtuális hálózati útvonalat.
8. Kattintson a **OK** értékek mentéséhez, és térjen vissza a **új VPN-kapcsolat** panelen.

### <a name="part-2---create-the-virtual-network-gateway"></a>2. rész – a virtuális hálózati átjáró létrehozása

1. Az a **új VPN-kapcsolat** panelen válassza a **átjáró létrehozása azonnal** jelölőnégyzetet és kattintson a **választható átjáró konfigurációs** megnyitásához a **átjáró konfigurációs** panelen. 

    ![Nyissa meg az átjáró konfigurációs panel](./media/vpn-gateway-connect-different-deployment-models-portal/optionalgatewayconfiguration.png "nyitott átjáró konfigurációs panel")
2. Kattintson a **alhálózat - kötelező beállítások konfigurálása** megnyitásához a **alhálózat hozzáadása** panelen. A **neve** már be van állítva a szükséges érték **GatewaySubnet**.
3. A **-címtartományt** az átjáró alhálózat hivatkozik. Bár létrehozhat egy átjáró-alhálózatot a /29-címtartományt (3 címeket), javasoljuk, hogy további IP-címeket létrehozható egy átjáró-alhálózatot, amely tartalmaz. Ez be tudja fogadni a jövőbeli konfigurációk, előfordulhat, hogy több elérhető IP-címeket. Ha lehetséges használjon /27 vagy /28. Ha ezeket a lépéseket egy gyakorlatot használ, olvassa el a [példa](#values) értékeket. Kattintson a **OK** az átjáró alhálózatának létrehozásához.
4. Az a **átjáró konfigurációs** panelen **mérete** az átjáró-Termékváltozat hivatkozik. Válassza ki a VPN-átjáró SKU átjáróján.
5. Ellenőrizze a **útválasztási típus** van **dinamikus**, majd kattintson **OK** való visszatéréshez a **új VPN-kapcsolat** panelen.
6. Az a **új VPN-kapcsolat** panelen kattintson a **OK** VPN-átjárót létrehozásának megkezdéséhez. VPN-átjáró létrehozása akár 45 percet is igénybe vehet.

### <a name="ip"></a>3. rész – a virtuális hálózati átjáró nyilvános IP-cím másolása

A virtuális hálózati átjáró létrehozása után megtekintheti az átjáró IP-cím. 

1. Keresse meg a klasszikus virtuális hálózaton, és kattintson **áttekintése**.
2. Kattintson a **VPN-kapcsolatok** VPN kapcsolatok panel megnyitásához. A VPN-kapcsolatok panelen megtekintheti a nyilvános IP-cím. Ez az a virtuális hálózati átjáró nyilvános IP-cím. 
3. Írja le, vagy másolja át az IP-címet. Használhatja a későbbi lépésekben a Resource Manager helyi hálózati átjáró konfigurációs beállítások használatakor. Az átjáró-kapcsolatok állapotát is megtekintheti. Figyelje meg a helyi hálózati telephely létrehozott "Csatlakozás" szerepel. Az állapot változik, a kapcsolatok létrehozása után.
4. Az átjáró IP-cím másolása után zárja be a panelt.

## <a name="rmvnet"></a>2. Az erőforrás-kezelő hálózatok beállításainak konfigurálása

Ebben a szakaszban hoz létre a virtuális hálózati átjáró és a helyi hálózati átjáró az erőforrás-kezelő virtuális hálózat számára. Ha egy erőforrás-kezelő virtuális hálózat nem rendelkezik, és ezeket a lépéseket egy gyakorlatot fussanak, is létrehozhat egy virtuális hálózat használatával [Ez a cikk](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) és a [példa](#values) beállítások értékeket felülről.

A képernyőképek csak példaként szolgálnak. Ügyeljen arra, hogy cserélje le az értékeket a saját, vagy használja a [példa](#values) értékeket.

### <a name="part-1---create-a-gateway-subnet"></a>1. rész – hozzon létre egy átjáró-alhálózatot

Virtuális hálózati átjáró létrehozása előtt először hozza létre az átjáró-alhálózatot. Hozzon létre egy átjáró-alhálózatot CIDR száma /28 vagy nagyobb. (/ 27- / 26, stb.)

[!INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

### <a name="part-2---create-a-virtual-network-gateway"></a>2. rész – a virtuális hálózati átjáró létrehozása

[!INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

### <a name="createlng"></a>3. rész – a helyi hálózati átjáró létrehozása

A helyi hálózati átjáró határozza meg a címtartományt és a társított a klasszikus virtuális hálózat és a virtuális hálózati átjáró nyilvános IP-cím.

Ha ezeket a lépéseket tennie, mint egy gyakorlatot, olvassa el ezeket a beállításokat:

| Virtual Network | Címterület | Régió | Csatlakozik a helyi hálózati telephely |Átjáró nyilvános IP-címe|
|:--- |:--- |:--- |:--- |:--- |
| ClassicVNet |(10.0.0.0/24) |USA nyugati régiója | RMVNetLocal (192.168.0.0/16) |A nyilvános IP-cím, amely hozzá van rendelve a ClassicVNet átjáró|
| RMVNet | (192.168.0.0/16) |USA keleti régiója |ClassicVNetLocal (10.0.0.0/24) |A nyilvános IP-cím, amely a RMVNet átjáró van hozzárendelve.|

[!INCLUDE [vpn-gateway-add-lng-rm-portal](../../includes/vpn-gateway-add-lng-rm-portal-include.md)]

## <a name="modifylng"></a>3. A klasszikus virtuális hálózat helyi webhely beállításainak módosítása

Ebben a szakaszban cserélje le a helyőrző IP-cím erőforrás-kezelő VPN átjáró IP-címét a helyi webhely beállításainak megadásakor használt. Ez a szakasz a klasszikus (SM) PowerShell-parancsmagokat használ.

1. Az Azure-portálon lépjen a klasszikus virtuális hálózatot.
2. A virtuális hálózata paneljén kattintson **áttekintése**.
3. Az a **VPN-kapcsolatok** területen kattintson az ábra a helyi webhely nevét.

    ![VPN-kapcsolatok](./media/vpn-gateway-connect-different-deployment-models-portal/vpnconnections.png "VPN-kapcsolatok")
4. Az a **telephelyek közötti VPN-kapcsolatok** panelen kattintson a hely nevét.

    ![Site-name](./media/vpn-gateway-connect-different-deployment-models-portal/sitetosite3.png "helyi webhely neve")
5. A helyi kapcsolat paneljén kattintson a helyi webhely megnyitása nevét a **helyi** panelen.

    ![Nyissa meg helyi-webhelyeken](./media/vpn-gateway-connect-different-deployment-models-portal/openlocal.png "helyi webhely megnyitása")
6. Az a **helyi** panelen, cserélje le a **VPN-átjáró IP-címet** az erőforrás-kezelő átjáró IP-címmel.

    ![Ip-átjárócím](./media/vpn-gateway-connect-different-deployment-models-portal/gwipaddress.png "átjáró IP-címe")
7. Kattintson a **OK** az IP-címének frissítése.

## <a name="RMtoclassic"></a>4. Erőforrás-kezelő klasszikus kapcsolat létrehozása

Ezeket a lépéseket konfigurálja a kapcsolat a Resource Manager virtuális hálózat és a klasszikus virtuális hálózatot az Azure portál használatával.

1. A **összes erőforrás**, keresse meg a helyi hálózati átjáró. A fenti példában a helyi hálózati átjáró van **ClassicVNetLocal**.
2. Kattintson a **konfigurációs** , és győződjön meg arról, hogy az IP-cím érték a klasszikus virtuális hálózaton a VPN-átjárót. Szükség esetén frissítse, majd kattintson az **mentése**. Zárja be a panelt.
3. A **összes erőforrás**, kattintson a helyi hálózati átjáró.
4. Kattintson a **kapcsolatok** kapcsolatok panel megnyitásához.
5. Az a **kapcsolatok** panelen kattintson a  **+**  -kapcsolatot.
6. Az a **kapcsolat hozzáadása a** panelen, a kapcsolat neve. Például "RMtoClassic."
7. **Pont-pont** már be van jelölve a panel.
8. Válassza ki a virtuális hálózati átjáró, amely a webhelyhez hozzárendelni kívánt.
9. Hozzon létre egy **megosztott kulcs**. Ez a kulcs csatlakozási folyamat, amely a Resource Manager virtuális hálózatba hoz létre a klasszikus virtuális hálózatot is használatban van. A kulcs létrehozása, vagy egy alkotják. A jelen példában használjuk "abc123", de lehet (és kell) használhat összetettebb.
10. Kattintson a **OK** a VPN-kapcsolat létrehozásához.

##<a name="classictoRM"></a>5. Klasszikus – Resource Manager-kapcsolat létrehozása

Ezeket a lépéseket a klasszikus virtuális hálózatot a kapcsolat és a Resource Manager virtuális hálózaton konfigurálja. Ezeket a lépéseket igényelnek a PowerShell. Ez a kapcsolat nem hozható létre a portálon. Győződjön meg arról, hogy már letöltötte és telepítette a klasszikus (SM) és az erőforrás-kezelő (RM) PowerShell-parancsmagok.

### <a name="1-connect-to-your-azure-account"></a>1. Csatlakozás az Azure-fiókhoz

Nyissa meg a PowerShell-konzolt emelt szintű jogosultságokkal, és jelentkezzen be az Azure-fiókjával. A következő parancsmag kéri a bejelentkezési hitelesítő adatok az Azure-fiók. A bejelentkezés után a fiók beállításait, hogy elérhetők az Azure PowerShell letöltése.

```powershell
Login-AzureRmAccount
```
   
Az Azure-előfizetések listájának lekérdezése, ha egynél több előfizetéssel rendelkezik.

```powershell
Get-AzureRmSubscription
```

Válassza ki a használni kívánt előfizetést. 

```powershell
Select-AzureRmSubscription -SubscriptionName "Name of subscription"
```

Adja hozzá az Azure a fiókot a klasszikus PowerShell-parancsmagokkal (SM). Ehhez használhatja a következő parancsot:

```powershell
Add-AzureAccount
```

### <a name="2-view-the-network-configuration-file-values"></a>2. A hálózati konfigurációs fájl értékeinek megtekintése

VNet létrehozása az Azure portálon, az Azure által használt teljes név esetén nem jelenik meg az Azure-portálon. Például egy VNet neve "ClassicVNet" az Azure portálon megjelenő lehetséges, hogy mennyi hosszabb nevet adni a hálózati konfigurációs fájlban. A név lehet, hogy alábbihoz hasonló: "Csoport ClassicRG ClassicVNet". Ezeket a lépéseket töltse le a hálózati konfigurációs fájlt, és megtekintheti az értékeket.

Hozzon létre egy könyvtárat a számítógépén, majd exportálja a hálózati konfigurációs fájlt a könyvtárba. Ebben a példában a hálózati konfigurációs fájlt a C:\AzureNet helyre exportálja.

```powershell
Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
```

Nyissa meg a fájlt egy szövegszerkesztőben, és megtekintése a klasszikus virtuális hálózat nevét. Amikor a PowerShell-parancsmagok futtatását lévő nevek használata a hálózati konfigurációs fájlt.

- Virtuális hálózat nevének felsorolt **VirtualNetworkSite neve =**
- Helyneveket felsorolt **LocalNetworkSite neve =**

### <a name="3-create-the-connection"></a>3. A kapcsolat létrehozása

A megosztott kulcsot, és hozza létre a kapcsolatot a klasszikus virtuális hálózatot az erőforrás-kezelő virtuális hálózat. Nem állítható be a megosztott kulcsot, a portál használatával. Ellenőrizze, hogy ezeket a lépéseket a klasszikus verzióra, a PowerShell-parancsmagok használatával bejelentkezve futtatja. Ehhez használja **Add-AzureAccount**. Ellenkező esetben csak akkor állíthatja be a "-AzureVNetGatewayKey".

- Ebben a példában **- VNetName** neve a klasszikus virtuális hálózaton található, a hálózati konfigurációs fájlban. 
- A **- LocalNetworkSiteName** határoz meg a helyi webhely neve megtalálható a hálózati konfigurációs fájlban.
- A **- SharedKey** érték, amely Ön hozza létre, és adja meg. Ehhez a példához használtuk *abc123*, de összetettebb hozhat létre. Figyeljen arra, hogy az itt megadott érték a Resource Manager klasszikus kapcsolat létrehozásakor megadott ugyanazt az értéket kell lennie.

```powershell
Set-AzureVNetGatewayKey -VNetName "Group ClassicRG ClassicVNet" `
-LocalNetworkSiteName "172B9E16_RMVNetLocal" -SharedKey abc123
```

##<a name="verify"></a>6. Kapcsolatok ellenőrzése

Az Azure-portálon vagy a PowerShell használatával ellenőrizheti a kapcsolatokat. Amikor ellenőrzése, szükség lehet Várjon egy percet, vagy két, mint a kapcsolat létrehozása folyamatban van. Sikeres kapcsolat esetén a kapcsolat állapota megosztottról "Csatlakozás" a "Csatlakoztatott".

### <a name="to-verify-the-connection-from-your-classic-vnet-to-your-resource-manager-vnet"></a>A kapcsolat a klasszikus virtuális hálózat és az erőforrás-kezelő virtuális hálózat ellenőrzése

[!INCLUDE [vpn-gateway-verify-connection-azureportal-classic](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]

###<a name="to-verify-the-connection-from-your-resource-manager-vnet-to-your-classic-vnet"></a>A kapcsolat az erőforrás-kezelő virtuális hálózat és a klasszikus virtuális hálózat ellenőrzése

[!INCLUDE [vpn-gateway-verify-connection-portal-rm](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <a name="faq"></a>Virtuális hálózatok közötti kapcsolat – gyakori kérdések

[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]
