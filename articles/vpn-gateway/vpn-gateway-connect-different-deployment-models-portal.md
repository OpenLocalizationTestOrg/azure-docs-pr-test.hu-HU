---
title: "Csatlakoztassa a klasszikus virtuális hálózatok tooAzure erőforrás-kezelő Vnetek: portál |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate klasszikus virtuális hálózatokat és erőforrás-kezelő Vnetek VPN Gateway és hello portál közötti VPN-kapcsolat"
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
ms.openlocfilehash: bef63b4e829335b2e1a9434a35ebfe33b4fd7373
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-virtual-networks-from-different-deployment-models-using-hello-portal"></a>Virtuális hálózatok csatlakoztatása a különböző üzembe helyezési modellel hello portál használatával

Ez a cikk bemutatja, hogyan tooconnect hagyományos Vneteket tooResource Manager Vnetek tooallow hello hello külön telepítési modellek toocommunicate egymás mellett található erőforrásokhoz. a cikkben ismertetett hello elsősorban az hello Azure-portálon, de ez a konfiguráció hello cikk jelöl ki a listában hello PowerShell használatával is létrehozhat.

> [!div class="op_single_selector"]
> * [Portál](vpn-gateway-connect-different-deployment-models-portal.md)
> * [PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)
> 
> 

Csatlakozás egy klasszikus virtuális hálózat tooa erőforrás-kezelő virtuális hálózat hasonló tooconnecting egy VNet tooan helyszíni hely. Mindkét csatlakozási típusok használja a VPN-átjáró tooprovide IPsec/IKE használatával biztonságos alagutat. Létrehozhat, amelyek különböző előfizetésekhez és különböző régiókban Vnetek közötti kapcsolat. Kapcsolatok tooon helyszíni hálózatokban, már rendelkező Vnetek mindaddig, amíg hello átjáró, amely a konfigurálva a dinamikus- vagy útválasztó-alapú is csatlakoztathatja. VNet – VNet kapcsolatokhoz kapcsolatos további információkért lásd: hello [VNet – VNet – gyakori kérdések](#faq) hello Ez a cikk végén. 

Ha a Vnetek hello ugyanabban a régióban, érdemes lehet tooinstead vegye figyelembe, hogy csatlakoztatja őket a Vnetben társviszony-létesítés használatával. A virtuális hálózatok közötti társviszony nem használ VPN-átjárót. További információ: [Társviszony létesítése virtuális hálózatok között](../virtual-network/virtual-network-peering-overview.md). 

### <a name="prerequisites"></a>Előfeltételek

* Ezek a lépések feltételezik, hogy mindkét Vnetek létre lett hozva. Ha egy a gyakorlatban ez a cikk használ, és nem rendelkezik a Vneteket, hivatkozások olyan hello lépéseket toohelp hoz létre őket.
* Győződjön meg arról, hogy hello címtartományát hello Vnetek nincsenek átfedésben vannak egymással, vagy más kapcsolatok címtartományok hello bármelyikét átfedi adott hello átjárók lehet, hogy kapcsolódik.
* Hello legújabb PowerShell-parancsmagjainak telepítése a Resource Manager és a Service Management (klasszikus). Ez a cikk használjuk, mind a hello Azure-portálon, és a PowerShell. PowerShell hello klasszikus virtuális hálózat toohello erőforrás-kezelő virtuális hálózat szükséges toocreate hello kapcsolat. További információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview). 

### <a name="values"></a>Példabeállítások

Ezen értékek toocreate egy tesztkörnyezetben használhatja, vagy tekintse meg a toothem toobetter hello jelen cikk példái a megismeréséhez.

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

Ebben a konfigurációban létrehoz egy VPN gateway-kapcsolatot egy hello virtuális hálózatok közötti IPsec/IKE VPN-alagúton keresztül. Győződjön meg arról, hogy a virtuális hálózat tartományok egyike átfedésben vannak egymással, vagy az összes hello helyi hálózatokat, amelyekhez csatlakoznak.

hello következő táblázatban példa hello például Vnetek és a helyi telephely definiálásának módját:

| Virtual Network | Címterület | Régió | Toolocal hálózati hellyel összeköti |
|:--- |:--- |:--- |:--- |
| ClassicVNet |(10.0.0.0/24) |USA nyugati régiója | RMVNetLocal (192.168.0.0/16) |
| RMVNet | (192.168.0.0/16) |USA keleti régiója |ClassicVNetLocal (10.0.0.0/24) |

## <a name="classicvnet"></a>1. Hello klasszikus virtuális hálózat beállításainak konfigurálása

Ebben a szakaszban hoz létre hello helyi hálózati (helyi) és a klasszikus virtuális hálózat virtuális hálózati átjáró hello. Ha a klasszikus virtuális hálózat nem rendelkezik, és ezeket a lépéseket egy gyakorlatot fussanak, is létrehozhat egy virtuális hálózat használatával [Ez a cikk](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) és hello [példa](#values) beállítások értékeket felülről.

Hello portál toocreate a klasszikus virtuális hálózatot használ, amikor az alábbi lépések, ellenkező esetben hello beállítás toocreate a klasszikus virtuális hálózatot nem jelenik meg hello segítségével keresse meg toohello virtuális hálózat panel:

1. Kattintson a hello "+" tooopen hello "New" panelen.
2. Hello a "Hello a piactéren" mezőben írja be a "Virtuális hálózat". Ha ehelyett hálózatkezelés virtuális hálózat ->, nem fog hello beállítás toocreate egy klasszikus virtuális hálózatot.
3. Keresse meg a "Virtuális hálózat" hello-listát adott vissza a, és kattintson rá a tooopen hello virtuális hálózat panel. 
4. A virtuális hálózat panel hello válassza a "Klasszikus" toocreate egy klasszikus virtuális hálózatot. 

Ha már van egy virtuális hálózat VPN-átjáróval, győződjön meg arról, hogy hello átjáró dinamikus. Ha statikus, kell először hello VPN-átjáró törlése, majd a folytatáshoz.

A képernyőképek csak példaként szolgálnak. Lehet, hogy tooreplace hello értékeket a saját, vagy használjon hello [példa](#values) értékeket.

### <a name="part-1---configure-hello-local-site"></a>1. rész – hello helyi webhely konfigurálása

Nyissa meg hello [Azure-portálon](https://ms.portal.azure.com) és jelentkezzen be az Azure-fiókjával.

1. Keresse meg a túl**összes erőforrás** , és keresse meg a hello **ClassicVNet** hello listában.
2. A hello **áttekintése** paneljén, hello **VPN-kapcsolatok** területen kattintson a hello **átjáró** grafikus toocreate átjáró.

    ![Konfigurálja a VPN-átjáró](./media/vpn-gateway-connect-different-deployment-models-portal/gatewaygraphic.png "VPN-átjáró konfigurálása")
3. A hello **új VPN-kapcsolat** panelen a **kapcsolattípus**, jelölje be **pont-pont**.
4. A **helyi**, kattintson a **kötelező beállítások konfigurálása**. Ekkor megnyílik a hello **helyi** panelen.
5. A hello **helyi** panelen, hozzon létre egy név toorefer toohello erőforrás-kezelő virtuális hálózat. Például "RMVNetLocal."
6. Ha hello VPN-átjáró a hello erőforrás-kezelő virtuális hálózat már rendelkezik egy nyilvános IP-cím, hello értékét használni hello **VPN-átjáró IP-címet** mező. Ha ezek a lépések végrehajtása egy gyakorlat szerint, vagy még nem rendelkezik a virtuális hálózati átjáró az erőforrás-kezelő vnet, hogy a helyőrző IP-címeit. Győződjön meg arról, hogy használ-e a hello helyőrző IP-cím formátuma érvénytelen. Később hello helyőrző IP-cím cserélje hello hello erőforrás-kezelő virtuális hálózati átjáró nyilvános IP-címét.
7. A **ügyfél Címterület**, hello virtuális hálózati IP-címterek hello erőforrás-kezelő virtuális hálózat hello értékeit használja. A beállítás nem használt toospecify hello cím szóközöket tooroute toohello erőforrás-kezelő virtuális hálózat.
8. Kattintson a **OK** toosave hello értékeket, és térjen vissza a toohello **új VPN-kapcsolat** panelen.

### <a name="part-2---create-hello-virtual-network-gateway"></a>2. rész – hello virtuális hálózati átjáró létrehozása

1. A hello **új VPN-kapcsolat** panelen, jelölje be hello **átjáró létrehozása azonnal** jelölőnégyzetet és kattintson a **választható átjáró konfigurációs** tooopen hello  **Átjáró konfigurációs** panelen. 

    ![Nyissa meg az átjáró konfigurációs panel](./media/vpn-gateway-connect-different-deployment-models-portal/optionalgatewayconfiguration.png "nyitott átjáró konfigurációs panel")
2. Kattintson a **alhálózat - kötelező beállítások konfigurálása** tooopen hello **alhálózat hozzáadása** panelen. Hello **neve** már be van állítva szükséges hello értékű **GatewaySubnet**.
3. Hello **-címtartományt** toohello tartomány hivatkozik az hello átjáró alhálózatához. Bár létrehozhat egy átjáró-alhálózatot a /29-címtartományt (3 címeket), javasoljuk, hogy további IP-címeket létrehozható egy átjáró-alhálózatot, amely tartalmaz. Ez be tudja fogadni a jövőbeli konfigurációk, előfordulhat, hogy több elérhető IP-címeket. Ha lehetséges használjon /27 vagy /28. Ha ezeket a lépéseket egy gyakorlatot használ, olvassa el a toohello [példa](#values) értékeket. Kattintson a **OK** toocreate hello átjáró-alhálózatot.
4. A hello **átjáró konfigurációs** panelen **mérete** toohello gateway SKU hivatkozik. Válassza ki a hello gateway SKU a VPN-átjárót.
5. Ellenőrizze a hello **útválasztási típus** van **dinamikus**, kattintson a **OK** tooreturn toohello **új VPN-kapcsolat** panelen.
6. A hello **új VPN-kapcsolat** panelen kattintson a **OK** toobegin a VPN-átjáró létrehozásához. VPN-átjáró létrehozása akár is igénybe vehet too45 perc toocomplete.

### <a name="ip"></a>3. rész - másolási hello virtuális hálózati átjáró nyilvános IP-cím

Hello virtuális hálózati átjáró létrehozása után megtekintheti a hello átjáró IP-címe. 

1. Keresse meg a tooyour klasszikus virtuális hálózatot, majd kattintson **áttekintése**.
2. Kattintson a **VPN-kapcsolatok** tooopen hello VPN kapcsolatok panelt. Hello VPN-kapcsolatok paneljén megtekintheti hello nyilvános IP-cím. Ez az hello nyilvános IP-cím tooyour virtuális hálózati átjáró. 
3. Írja le, vagy másolja a hello IP-címet. Használhatja a későbbi lépésekben a Resource Manager helyi hálózati átjáró konfigurációs beállítások használatakor. Az átjáró-kapcsolatok hello állapotát is megtekintheti. Értesítés hello helyi hálózati telephely létrehozott "Csatlakozás" szerepel. a kapcsolatok létrehozása után hello állapota változik.
4. Hello átjáró IP-cím másolás után zárja be a hello panelt.

## <a name="rmvnet"></a>2. Hello Resource Manager virtuális hálózat beállításainak konfigurálása

Ebben a szakaszban hoz létre hello virtuális hálózati átjáró és a helyi hálózati átjáró hello az erőforrás-kezelő virtuális hálózat számára. Ha egy erőforrás-kezelő virtuális hálózat nem rendelkezik, és ezeket a lépéseket egy gyakorlatot fussanak, is létrehozhat egy virtuális hálózat használatával [Ez a cikk](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) és hello [példa](#values) beállítások értékeket felülről.

A képernyőképek csak példaként szolgálnak. Lehet, hogy tooreplace hello értékeket a saját, vagy használjon hello [példa](#values) értékeket.

### <a name="part-1---create-a-gateway-subnet"></a>1. rész – hozzon létre egy átjáró-alhálózatot

Virtuális hálózati átjáró létrehozása előtt először toocreate hello átjáró-alhálózatot. Hozzon létre egy átjáró-alhálózatot CIDR száma /28 vagy nagyobb. (/ 27- / 26, stb.)

[!INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

### <a name="part-2---create-a-virtual-network-gateway"></a>2. rész – a virtuális hálózati átjáró létrehozása

[!INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

### <a name="createlng"></a>3. rész – a helyi hálózati átjáró létrehozása

hello helyi hálózati átjáró hello címtartományt és a klasszikus virtuális hálózat és a virtuális hálózati átjáró társított hello nyilvános IP-cím megadása

Ha ezeket a lépéseket tennie, mint egy gyakorlatot, tekintse meg a toothese beállítások:

| Virtual Network | Címterület | Régió | Toolocal hálózati hellyel összeköti |Átjáró nyilvános IP-címe|
|:--- |:--- |:--- |:--- |:--- |
| ClassicVNet |(10.0.0.0/24) |USA nyugati régiója | RMVNetLocal (192.168.0.0/16) |hello hozzárendelt toohello ClassicVNet átjáró nyilvános IP-cím|
| RMVNet | (192.168.0.0/16) |USA keleti régiója |ClassicVNetLocal (10.0.0.0/24) |hello hozzárendelt toohello RMVNet átjáró nyilvános IP-cím.|

[!INCLUDE [vpn-gateway-add-lng-rm-portal](../../includes/vpn-gateway-add-lng-rm-portal-include.md)]

## <a name="modifylng"></a>3. Hello klasszikus virtuális hálózat helyi webhely beállításainak módosítása

Ebben a szakaszban cserélje le hello helyőrző IP-címet, amelyet használt hello helyi hely beállításait, az erőforrás-kezelő VPN átjáró IP-címet hello megadásakor. Ez a szakasz a hello klasszikus (SM) PowerShell-parancsmagok.

1. Hello Azure-portálon lépjen a toohello klasszikus virtuális hálózatot.
2. A virtuális hálózat hello paneljén kattintson **áttekintése**.
3. A hello **VPN-kapcsolatok** területen kattintson a helyi webhely hello ábrán hello nevét.

    ![VPN-kapcsolatok](./media/vpn-gateway-connect-different-deployment-models-portal/vpnconnections.png "VPN-kapcsolatok")
4. A hello **telephelyek közötti VPN-kapcsolatok** paneljén kattintson hello hello hely nevét.

    ![Site-name](./media/vpn-gateway-connect-different-deployment-models-portal/sitetosite3.png "helyi webhely neve")
5. A helyi webhely hello kapcsolat paneljén kattintson hello helyi tooopen hello hello neve **helyi** panelen.

    ![Nyissa meg helyi-webhelyeken](./media/vpn-gateway-connect-different-deployment-models-portal/openlocal.png "helyi webhely megnyitása")
6. A hello **helyi** panelen, a név felülírandó hello **VPN-átjáró IP-címet** hello erőforrás-kezelő átjáró hello IP-címmel.

    ![Ip-átjárócím](./media/vpn-gateway-connect-different-deployment-models-portal/gwipaddress.png "átjáró IP-címe")
7. Kattintson a **OK** tooupdate hello IP-címet.

## <a name="RMtoclassic"></a>4. Erőforrás-kezelő tooclassic kapcsolat létrehozása

Ezeket a lépéseket, konfigurálja az erőforrás-kezelő virtuális hálózat hello hello kapcsolat toohello klasszikus virtuális hálózat használatával hello Azure-portálon.

1. A **összes erőforrás**, keresse meg a helyi hálózati átjáró hello. A jelen példában hello helyi hálózati átjáró van **ClassicVNetLocal**.
2. Kattintson a **konfigurációs** és győződjön meg arról, hogy hello IP-cím érték hello VPN-átjáró hello a klasszikus virtuális hálózatot. Szükség esetén frissítse, majd kattintson az **mentése**. Bezárás hello panelen.
3. A **összes erőforrás**, kattintson a helyi hálózati átjáró hello.
4. Kattintson a **kapcsolatok** tooopen hello kapcsolatok panelt.
5. A hello **kapcsolatok** panelen kattintson a  **+**  tooadd kapcsolatot.
6. A hello **kapcsolat hozzáadása a** panelen, hello kapcsolat neve. Például "RMtoClassic."
7. **Pont-pont** már be van jelölve a panel.
8. Válassza ki, hogy szeretné-e a hellyel való tooassociate hello virtuális hálózati átjáró.
9. Hozzon létre egy **megosztott kulcs**. Ez a kulcs hello kapcsolatot hoz létre hello klasszikus virtuális hálózat toohello erőforrás-kezelő virtuális hálózat is használatban van. Hello kulcs létrehozása, vagy egy alkotják. A jelen példában használjuk "abc123", de lehet (és kell) használhat összetettebb.
10. Kattintson a **OK** toocreate hello kapcsolat.

##<a name="classictoRM"></a>5. Klasszikus tooResource Manager-kapcsolat létrehozása

Ezeket a lépéseket konfigurálja hello klasszikus virtuális hálózat toohello erőforrás-kezelő virtuális hálózat hello kapcsolatot. Ezeket a lépéseket igényelnek a PowerShell. Ez a kapcsolat nem hozható létre hello portálon. Győződjön meg arról, hogy már letöltötte és hello klasszikus (SM) és erőforrás-kezelő (RM) PowerShell-parancsmagok is telepítve.

### <a name="1-connect-tooyour-azure-account"></a>1. Csatlakozás Azure-fiók tooyour

Nyissa meg a hello PowerShell konzolt emelt szintű jogosultságokkal, és jelentkezzen be Azure-fiók tooyour. hello következő parancsmag kéri hello bejelentkezési hitelesítő adatokat az Azure-fiók. A bejelentkezés után a fiók beállításait, hogy-e elérhető tooAzure PowerShell lesznek letöltve.

```powershell
Login-AzureRmAccount
```
   
Az Azure-előfizetések listájának lekérdezése, ha egynél több előfizetéssel rendelkezik.

```powershell
Get-AzureRmSubscription
```

Adja meg, hogy szeretné-e toouse hello előfizetés. 

```powershell
Select-AzureRmSubscription -SubscriptionName "Name of subscription"
```

Adja hozzá az Azure-fiók toouse hello klasszikus PowerShell parancsmagokat (SM). toodo tehát hello a következő parancs használható:

```powershell
Add-AzureAccount
```

### <a name="2-view-hello-network-configuration-file-values"></a>2. Hello hálózati konfigurációs fájl értékeinek megtekintése

Hello Azure-portálon létrehoz egy Vnetet, teljes név hello Azure használó esetén nem látható hello Azure-portálon. Egy VNet toobe elnevezett "ClassicVNet" hello Azure-portálon megjelenő például lehet egy sokkal hosszabb név hello hálózati konfigurációs fájlban. hello neve lehet, hogy alábbihoz hasonló: "Csoport ClassicRG ClassicVNet". Ezeket a lépéseket akkor töltse le hello hálózati konfigurációs fájl és a nézet hello értékeket.

Hozzon létre egy könyvtárat a számítógépen, és exportálhatja a hálózati konfiguráció hello toohello könyvtára. Ebben a példában a hello hálózati konfigurációs fájl nem exportált tooC:\AzureNet.

```powershell
Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
```

Nyissa meg a hello fájlt szöveg-szerkesztő és a nézet hello nevére a klasszikus virtuális hálózat. Hello hálózati konfigurációs fájlban hello-neveket használja, amikor a PowerShell-parancsmagok futtatását.

- Virtuális hálózat nevének felsorolt **VirtualNetworkSite neve =**
- Helyneveket felsorolt **LocalNetworkSite neve =**

### <a name="3-create-hello-connection"></a>3. Hello kapcsolat létrehozása

Hello megosztott kulcsot, és hello kapcsolat hello klasszikus virtuális hálózat toohello erőforrás-kezelő virtuális hálózat létrehozása. Hello hello portálon megosztott kulcs nem állítható be. Ellenőrizze, hogy ezeket a lépéseket a klasszikus verziója hello hello PowerShell-parancsmagok használatával bejelentkezve futtatja. Igen, használjon toodo **Add-AzureAccount**. Ellenkező esetben nem fogja tudni tooset hello "-AzureVNetGatewayKey".

- Ebben a példában **- VNetName** hello név hello, mint a klasszikus virtuális hálózatot a hálózati konfigurációs fájlban található. 
- Hello **- LocalNetworkSiteName** hello a megadott néven hello helyi hely, mint a hálózati konfigurációs fájlban található.
- Hello **- SharedKey** érték, amely Ön hozza létre, és adja meg. Ehhez a példához használtuk *abc123*, de összetettebb hozhat létre. fontos dolog, hogy az itt megadott hello érték hello azonos érték a Resource Manager tooclassic kapcsolat létrehozásakor megadott hello kell lennie.

```powershell
Set-AzureVNetGatewayKey -VNetName "Group ClassicRG ClassicVNet" `
-LocalNetworkSiteName "172B9E16_RMVNetLocal" -SharedKey abc123
```

##<a name="verify"></a>6. Kapcsolatok ellenőrzése

A kapcsolatok hello Azure-portál vagy PowerShell ellenőrizheti. Amikor ellenőrzése, szükség lehet toowait egy-két percen hello kapcsolat létrehozását. Sikeres kapcsolat esetén hello kapcsolati állapota megosztottról "Csatlakozás" too'Connected ".

### <a name="tooverify-hello-connection-from-your-classic-vnet-tooyour-resource-manager-vnet"></a>a klasszikus virtuális hálózat tooyour erőforrás-kezelő virtuális hálózat tooverify hello kapcsolat

[!INCLUDE [vpn-gateway-verify-connection-azureportal-classic](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]

###<a name="tooverify-hello-connection-from-your-resource-manager-vnet-tooyour-classic-vnet"></a>az erőforrás-kezelő virtuális hálózat tooyour tooverify hello kapcsolatot klasszikus virtuális hálózaton

[!INCLUDE [vpn-gateway-verify-connection-portal-rm](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <a name="faq"></a>Virtuális hálózatok közötti kapcsolat – gyakori kérdések

[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]
