---
title: "aaaCreate, módosítsa vagy törölje az Azure-beli virtuális hálózat társviszony-létesítés |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate, módosítsa vagy törölje a virtuális hálózati társviszony-létesítés."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/26/2017
ms.author: jdial
ms.openlocfilehash: 70f85364ab23e23533ad276aeec0156cebe89be5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-change-or-delete-a-virtual-network-peering"></a>Létrehozása, módosítása vagy törlése a virtuális hálózati társviszony-létesítés

Ismerje meg, hogyan toocreate, módosítsa vagy törölje a virtuális hálózati társviszony-létesítés. Virtuális hálózati társviszony-létesítési lehetővé teszi, hogy Ön tooconnect két virtuális hálózatok hello keresztül azonos Azure-beli hely hello Azure hálózat. Ha nincsenek társviszonyban, hello két virtuális hálózatok továbbra is felügyelt külön erőforrásként. Bármelyik virtuális hálózatán lévő erőforrásokat kommunikálni hello azonos késleltetés és a sávszélesség, mintha hello erőforrások a hello azonos virtuális hálózaton. Ha még nem ismeri a virtuális hálózati társviszony-létesítést, ajánlott hello olvasása [virtuális hálózati társviszony-létesítési áttekintése](virtual-network-peering-overview.md) és hello befejezése [hozzon létre egy virtuális hálózati társviszony-létesítési oktatóanyag](virtual-network-create-peering.md), még a befejeződése előtt Ez a cikk hello feladatokat.

## <a name="before-you-begin"></a>Előkészületek

Teljes hello feladatok végrehajtása előtt a következő szakasz ebben a cikkben ismertetett visszaállítási lépésekkel:

- Felülvizsgálati hello [Azure korlátozza](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) cikk toolearn kapcsolatos korlátozásokat a társviszony-létesítéshez.
- Jelentkezzen be a toohello Azure-portálon, az Azure parancssori felület (CLI) vagy az Azure PowerShell és az Azure-fiók. Ha még nem rendelkezik Azure-fiókja, regisztráljon egy [ingyenes próbafiók](https://azure.microsoft.com/free).
- Ha a PowerShell használatával parancsok toocomplete feladatok ebben a cikkben [Azure PowerShell telepítése és konfigurálása](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Ellenőrizze, hogy a legfrissebb verzióra hello hello Azure PowerShell-parancsmagjai telepítve vannak. Írja be a PowerShell-parancsok, példákkal tooget súgóját `get-help <command> -full`.
- Ha az Azure parancssori felület (CLI) használatával parancsok toocomplete feladatok ebben a cikkben [telepítése és konfigurálása az Azure parancssori felület hello](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Ellenőrizze, hogy a legújabb verziójának hello hello Azure parancssori felület telepítve. Írja be a parancssori felület parancsait tooget súgóját `az <command> --help`. Ahelyett, hogy a parancssori felület telepítése hello és a szükséges előfeltételek hello Azure Cloud rendszerhéj is használhatja. hello Azure Cloud rendszerhéj a szabad rendszerhéjakba futtatható közvetlenül hello Azure-portálon belül. hello felhő rendszerhéj hello Azure CLI előtelepített és konfigurált toouse-fiókjához tartozik. toouse hello felhő rendszerhéj, kattintson a felhő rendszerhéj hello **> _** hello hello tetején gomb [portal](https://portal.azure.com). 

## <a name="create-a-peering"></a>A társviszony-létesítés létrehozása

>[!NOTE]
>Néhány követelményt, korlátozásokat, és szempontok toosuccessfully hozzon létre egy virtuális hálózati társviszony-létesítés. Létrehozása társviszony-létesítés, előtt győződjön meg a saját kezűleg a hello már familiarized [követelményeket és korlátokat](#requirements-and-constraints) és [szükséges engedélyek](#permissions).
>

1. Jelentkezzen be toohello [portal](https://portal.azure.com) egy olyan fiókkal, amely hozzá van rendelve a hello szükséges [szerepkör vagy engedélyek](#permissions).
2. Hello mezőben hello szöveget tartalmazó *keresési erőforrások* tetején hello hello Azure-portálon, írja be a *virtuális hálózatok*. Ha **virtuális hálózatok** jelenik meg a hello találatok, kattintson rá. Ne válassza **virtuális hálózatok (klasszikus)** Ha hello listájában látható, nem hozzon létre egy hello klasszikus üzembe helyezési modellben telepített virtuális hálózati társviszony.
3. A hello **virtuális hálózatok** panel, amelyen megjelenik, kattintson a hello virtuális hálózati társviszony-létesítés a toocreate szeretné.
4. A kiválasztott virtuális hálózat hello megjelenő hello ablaktáblán kattintson **Társviszony** a hello **beállítások** szakasz.
5. Kattintson a **+ Hozzáadás**. 
6. <a name="add-peering"></a>A hello **hozzáadása a társviszony-létesítés** panelen adja meg vagy válassza ki a következő beállítások hello értékeit:
    - **Name:** hello nevét hello társviszony-létesítés hello virtuális hálózaton belül egyedinek kell lennie.
    - **Virtuális hálózat üzembe helyezési modellt:** válassza ki, melyik központi telepítési modell hello virtuális hálózati meg szeretné, hogy a toopeer keresztül telepítve lett.
    - **Tudható, hogy az erőforrás-azonosító:** Ha rendelkezik olvasási hozzáféréssel toohello virtuális hálózatot a toopeer szeretné, hagyja a jelölőnégyzet nincs bejelölve. Ha nem rendelkezik olvasási jogosultsággal toohello virtuális hálózat vagy a toopeer kívánt előfizetést, ezt a négyzetet. Adja meg a teljes erőforrás-azonosító hello hello toopeer a hello a kívánt virtuális hálózat **erőforrás-azonosító** jelölőnégyzet, amelynek jelent meg, ha hello mezőben be van jelölve. hello erőforrás-azonosító kell lennie egy virtuális hálózat már szerepel hello azonos Azure [hely](https://azure.microsoft.com/regions) , ez a virtuális hálózat. hello teljes erőforrás-azonosító a következőhöz hasonlótúl/előfizetések/<Id>/resourceGroups/ < erőforráscsoport--neve > /providers/Microsoft.Network/virtualNetworks/ < virtuális-hálózati-neve >. Hello erőforrás-azonosító kaphat a virtuális hálózat a virtuális hálózat hello tulajdonságainak megtekintése. Hogyan tooview hello tulajdonságai egy virtuális hálózat: toolearn [virtuális hálózatok kezeléséhez](virtual-network-manage-network.md#view-vnet).
    - **Előfizetés:** válassza hello [előfizetés](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) toopeer a kívánt hello virtuális hálózat. Egy vagy több előfizetés is szerepelnek, attól függően, hogy hány előfizetések olvasási hozzáféréssel rendelkezik fiókjába. Ha bejelölte a hello **erőforrás-azonosító** jelölőnégyzetét, ez a beállítás nem érhető el. Virtuális hálózatok különböző előfizetésekhez is partnert, mindaddig, amíg mindkét virtuális hálózat Resource Manageren keresztül lettek létrehozva. előzetes hello képességét toopeer keresztül különböző üzembe helyezési modellel létrehozott előfizetések között van. Regisztrálja a hello preview társviszony-létesítés különböző üzembe helyezési modellel különböző előfizetésekhez létező telepített virtuális hálózatok közötti létrehozása előtt. További tudnivalók hello Preview tooregister és [keresztül különböző előfizetésekhez különböző üzembe helyezési modellel létrehozott virtuális hálózatokat partnert](create-peering-different-deployment-models-subscriptions.md).
    - **Virtuális hálózat:** válassza ki a kívánt toopeer a virtuális hálózati hello. Kiválaszthatja, hogy vagy az Azure-telepítés modell használatával létrehozott virtuális hálózatban, de hello virtuális hálózat ugyanazon a helyen éppen kezdeményezése hello virtuális hálózatként hello a társviszony hello kell. Rendelkeznie kell olvasási hozzáférési toohello virtuális hálózat az toobe hello listája látható. Ha egy virtuális hálózat szerepel a listában, szürke, de ez lehet, hogy hello virtuális hálózati hello címtartomány átfedésben hello ehhez a virtuális hálózathoz. Virtuális hálózati címterük hozzon létre, ha azok nem társítottak. Ha bejelölte a hello **erőforrás-azonosító** jelölőnégyzetét, ez a beállítás nem érhető el.
    - **Virtuális hálózati hozzáférés engedélyezése:** válasszon **engedélyezve** (alapértelmezett), ha azt szeretné, hogy tooenable kommunikációs hello két virtuális hálózatok között. Virtuális hálózatok közötti kommunikáció lehetővé teszi, hogy csatlakoztatott tooeither virtuális hálózati toocommunicate egymással a hello azonos sávszélesség és a késés, mintha csatlakoztatott toohello ugyanazt a virtuális hálózatot. Erőforrások hello két virtuális hálózatok közötti összes kommunikáció hello Azure magánhálózati felett van. Hello **VirtualNetwork** magában foglalja a hálózati biztonsági csoportok az alapértelmezett címke hello virtuális hálózat és a virtuális hálózat társviszonyban. További részletek toolearn hálózati biztonsági csoport alapértelmezett címkéket, olvassa el a hello [hálózati biztonsági csoportok – áttekintés](virtual-networks-nsg.md#default-tags) cikk.  Válassza ki **letiltott** Ha nem szeretné, hogy forgalom tooflow toohello társviszonyban virtuális hálózat. Kiválaszthatja **letiltott** Ha egy másik virtuális hálózatot a virtuális hálózat már társviszonyban, de időnként szeretné, hogy toodisable hello két virtuális hálózatok közötti adatforgalom. Előfordulhat, hogy törli, majd újra létrehozza a társviszony mint kényelmesebb engedélyezése vagy tiltása. Ha ez a beállítás le van tiltva, a forgalom között nem folyik hello társítottak, virtuális hálózatok.
    - **Engedélyezi a továbbított forgalmat:** ellenőrizze, hogy a mezőben tooallow továbbított forgalom toohello virtuális hálózat (hello társviszonyban virtuális hálózat nem származó forgalmat) nincsenek társviszonyban tooflow toothis virtuális hálózat. Forgalom továbbítását a hálózati virtuális készülék hello virtuális hálózat most társviszony-létesítés, és létre felhasználó által definiált útvonalak tooforward forgalom hello hálózati virtuális készülék telepítése esetén közös. Ha nem adja meg ezt a jelölőnégyzetet bejelölve (alapértelmezett), a virtuális hálózati hello társviszonyban érkező forgalom nem flow toothis virtuális hálózati. Ez a funkció lehetővé teszi, hogy hello továbbított forgalmat keresztül hello társviszony-létesítést, amíg nem hozhat létre bármely felhasználó által definiált útvonalakat és virtuális készülékekre. Felhasználó által definiált útvonalak és hálózati virtuális készülékek külön hoz létre. További tudnivalók [felhasználó által definiált útvonalak](virtual-networks-udr-overview.md).
    - **Átjáró átvitel engedélyezése:** ellenőrizze ezt a jelölőnégyzetet, ha olyan virtuális hálózati átjáró csatolt toothis virtuális hálózattal rendelkezik, és a kívánt tooallow forgalom hello társítottak, virtuális hálózati tooflow hello átjárón keresztül. Például ezt a virtuális hálózatot lehet csatolt tooan a helyi hálózaton keresztül a virtuális hálózati átjáró. Ez a mező lehetővé teszi, hogy a forgalom hello ellenőrzése társítottak, virtuális hálózati tooflow hello átjáró csatolt toothis virtuális hálózaton keresztül. Ha bejelöli a jelölőnégyzetet, virtuális hálózati hello társviszonyban konfigurált átjárók nem lehet. hello társítottak, virtuális hálózathoz kell hello **átjáró használata a távoli** jelölőnégyzet be van jelölve, amikor a társviszony hello beállítása hello más virtuális hálózat toothis virtuális hálózat. Ha nem adja meg ezt a jelölőnégyzetet bejelölve (alapértelmezett), akkor a hello társítottak, virtuális hálózati forgalmát továbbra is toothis virtuális hálózati forgalmat, de virtuális hálózati átjáró csatolt toothis virtuális hálózat nem áramlása. További információ [virtuális hálózati átjárók](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#s2smulti). 
    
    Nem engedélyezi ezt a beállítást, ha egy virtuális hálózat (klasszikus) rendelkező virtuális hálózatban (erőforrás-kezelő) még társviszony. Bár hello forgalom hello virtuális hálózatok között, a nem áramlása hello virtuális hálózat (klasszikus) forgalom hálózati átjáró csatolt toohello virtuális hálózat (Resource Manager).
    - **Távoli átjárókat használ:** ellenőrizze a virtuális hálózati átjáró csatolt toohello virtuális hálózaton keresztül, hogy társviszony-létesítés a virtuális hálózati tooflow mezőben tooallow forgalmát. Például, hogy társviszony-létesítés hello virtuális hálózati rendelkezik csatolt VPN-átjáró, amely lehetővé teszi a kommunikációt tooan a helyi hálózaton.  A jelölőnégyzet bejelölésével lehetővé teszi, hogy a virtuális hálózati tooflow hello VPN gateway csatolt toohello társviszonyban virtuális hálózaton keresztül érkező forgalmat. Ha bejelöli a jelölőnégyzetet, hello társítottak, virtuális hálózati rendelkeznie kell egy virtuális hálózati átjáró csatolt tooit, és rendelkeznie kell hello **átjáró átvitel engedélyezése** jelölőnégyzet be van jelölve. Ha nem adja meg ezt a jelölőnégyzetet bejelölve (alapértelmezett), a virtuális hálózati hello társviszonyban továbbra is áramolhasson az adatforgalom toothis virtuális hálózat, de nem folyik a virtuális hálózati átjáró csatolt toothis virtuális hálózaton keresztül. 
    
    Nem engedélyezi ezt a beállítást, ha egy virtuális hálózat (klasszikus) rendelkező virtuális hálózatban (erőforrás-kezelő) még társviszony. Bár hello forgalom hello virtuális hálózatok között, a nem áramlása hello (erőforrás-kezelő) virtuális hálózati forgalom hálózati átjáró csatolt toohello virtuális hálózat (klasszikus).
7. Kattintson a hello **OK** gomb tooadd hello alhálózati toohello virtuális hálózati választotta.

### <a name="commands"></a>Parancsok

|Eszköz|Parancs|
|---|---|
|parancssori felület|[az hálózati vnetben társviszony-létesítés létrehozása](/cli/azure/network/vnet/peering#create?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[Adja hozzá AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/add-azurermvirtualnetworkpeering?toc=%2fazure%2fvirtual-network%2ftoc.json)|


### <a name="scenarios"></a>Forgatókönyvek

Virtuális hálózati társviszony-létesítés jön létre, hello létre azonos virtuális hálózatok között, vagy meglévő különböző üzembe helyezési modellel hello ugyanazon vagy másik előfizetést. Hajtsa végre a következő forgatókönyvek hello egyik részletes oktatóanyaga:
 
|Azure üzembehelyezési modell  | Előfizetés  |
|---------|---------|
|Mindkét Resource Manager |[Ugyanaz](virtual-network-create-peering.md)|
| |[Különböző](create-peering-different-subscriptions.md)|
|Egy Resource Manager, egy klasszikus     |[Ugyanaz](create-peering-different-deployment-models.md)|
| |[Különböző](create-peering-different-deployment-models-subscriptions.md)|

## <a name="view-or-change-peering-settings"></a>Társviszony-létesítési beállításainak megtekintése vagy módosítása

1. Jelentkezzen be toohello [portal](https://portal.azure.com) egy olyan fiókkal, amely hozzá van rendelve a hello szükséges [szerepkör vagy engedélyek](#permissions).
2. Hello mezőben hello szöveget tartalmazó *keresési erőforrások* tetején hello hello Azure-portálon, írja be a *virtuális hálózatok*. Ha **virtuális hálózatok** jelenik meg a hello találatok, kattintson rá.
3. A hello **virtuális hálózatok** panel, amelyen megjelenik, kattintson a hello virtuális hálózati társviszony-létesítés a toocreate szeretné.
4. A kiválasztott virtuális hálózat hello megjelenő hello ablaktáblán kattintson **Társviszony** a hello **beállítások** szakasz.
5. Kattintson a társviszony-létesítés hello tooview szeretné, vagy módosítsa a beállításokat.
6. Hello megfelelő beállítás módosítása. Olvassa el a mindegyikéhez hello-beállítások [6. lépés](#add-peering) hello, hozzon létre egy társviszony-létesítési című szakaszát. 

    >[!NOTE]
    >Néhány követelményt, korlátozásokat, és szempontok toosuccessfully hozzon létre egy virtuális hálózati társviszony-létesítés. Létrehozása társviszony-létesítés, előtt győződjön meg a saját kezűleg a hello már familiarized [követelményeket és korlátokat](#requirements-and-constraints) és [szükséges engedélyek](#permissions).
    >

7. Kattintson a **Save** (Mentés) gombra.

**Parancsok**

|Eszköz|Parancs|
|---|---|
|parancssori felület|[az hálózati vnetben társviszony-létesítési lista](/cli/azure/network/vnet/peering?toc=%2fazure%2fvirtual-network%2ftoc.json#list) toolist esetében a virtuális hálózat [az hálózati vnetben társviszony-létesítési megjelenítése](/cli/azure/network/vnet/peering?toc=%2fazure%2fvirtual-network%2ftoc.json#show) tooshow beállításait egy adott társviszony-létesítést, és [az hálózati társviszony-létesítési frissítés vnetet](/cli/azure/network/vnet/peering?toc=%2fazure%2fvirtual-network%2ftoc.json#update) toochange társviszony-létesítési beállításokat.|
|PowerShell|[Get-AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/get-azurermvirtualnetworkpeering?toc=%2fazure%2fvirtual-network%2ftoc.json) tooretrieve társviszony-létesítési beállításainak megtekintése és [Set-AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/set-azurermvirtualnetworkpeering?toc=%2fazure%2fvirtual-network%2ftoc.json) toochange beállításait.|

## <a name="delete-a-peering"></a>Törölje a társviszony-létesítés
Társviszony törlődik, ha a virtuális hálózat már nem a forgalom toohello társítottak, virtuális hálózati. Resource Manager használatával telepített virtuális hálózatok társviszonyban van, minden egyes virtuális hálózati rendelkezik egy társviszony-létesítési toohello más virtuális hálózaton. Abban az esetben, ha egy virtuális hálózati társviszony hello törlése letiltja a hello kommunikációt hello virtuális hálózatok között, nem törli a hello társviszony hello más virtuális hálózaton. hello társviszony-létesítési állapot hello társviszony-létesítést, amely létezik a hello más virtuális hálózat **Disconnected**. Nem tudja újra létrehozni hello társviszony-létesítés csak akkor hozza létre újból a hello első virtuális hálózati társviszony hello és hello társviszony-létesítési állapot mind a virtuális hálózatok módosítások túl*csatlakoztatva*. 

Ha azt szeretné, virtuális hálózatok toocommunicate néha, de nem mindig társviszony-létesítés törléssel beállítható hello **virtuális hálózati hozzáférés engedélyezése** beállítás túl**letiltott** helyette. toolearn, hogy olvassa a hello 6. lépés [létrehozása a társviszony-létesítés](#create-peering) című szakaszát. Előfordulhat, hogy letiltásával és egyszerűbb, mint törlése és újbóli létrehozása esetében a hálózati hozzáférés engedélyezéséhez.

1. Jelentkezzen be toohello [portal](https://portal.azure.com) egy olyan fiókkal, amely hozzá van rendelve a hello szükséges [szerepkör vagy engedélyek](#permissions).
2. Hello mezőben hello szöveget tartalmazó *keresési erőforrások* tetején hello hello Azure-portálon, írja be a *virtuális hálózatok*. Ha **virtuális hálózatok** jelenik meg a hello találatok, kattintson rá.
3. A hello **virtuális hálózatok** panel, amelyen megjelenik, kattintson a hello virtuális hálózati társviszony-létesítés a toodelete szeretné.
4. Hello panelen megjelenő hello kiválasztott virtuális hálózat kattintson **Társviszony** alatt **beállítások**.
5. A társviszony megjelenő hello társviszony panelen, kattintson a jobb gombbal hello társviszony-létesítés meg akarja toodelete hello listájában kattintson **törlése**, majd **Igen** toodelete hello hello első virtuális hálózati társviszony.
6. Teljes hello előző lépéseket toodelete hello társviszony-létesítés a többi virtuális hálózati társviszony-létesítés hello hello.

**Parancsok**

|Eszköz|Parancs|
|---|---|
|parancssori felület|[az hálózati vnetben társviszony törlése](/cli/azure/network/vnet/peering?toc=%2fazure%2fvirtual-network%2ftoc.json#delete)|
|PowerShell|[Remove-AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/remove-azurermvirtualnetworkpeering?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="requirements-and-constraints"></a>Követelmények és korlátozások 

- hello virtuális hálózatok partnert, rendelkeznie kell egymást nem átfedő IP-címterületeken.
- Nem adja hozzá a-címtereket, és nem törölhető címterek virtuális hálózatról, miután a virtuális hálózaton nincsenek társviszonyban, egy másik virtuális hálózathoz. tooadd, vagy távolítsa el címterekhez, törlési hello társviszony-létesítést, vegye fel vagy távolítsa el a hello címterek, majd hozza létre a hello társviszony-létesítés. tooadd-címtereket való, vagy távolítsa el a címterületeket a virtuális hálózatok, olvassa el a hello [létrehozása, módosítása vagy törlése a virtuális hálózatok](virtual-network-manage-network.md#add-address-spaces) cikk. 
- Erőforrás-kezelő vagy a Resource Manager használatával telepített hello klasszikus üzembe helyezési modellben telepített virtuális hálózat virtuális hálózaton keresztül telepített két virtuális hálózat is partnert. Két, hello klasszikus telepítési modell használatával létrehozott virtuális hálózatok nem partnert. Ha nem ismeri az Azure üzembe helyezési modellel, olvassa el a hello [megértéséhez Azure üzembe helyezési modellel](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) cikk. Használhatja a [VPN-átjáró](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V) tooconnect két hello klasszikus telepítési modell használatával létrehozott virtuális hálózatokat.
- Két, a Resource Manager használatával létrehozott virtuális hálózatokat társviszony, amikor társviszony-létesítés be kell állítani az egyes virtuális hálózati társviszony-létesítés hello. Hello társviszony-létesítési toohello második virtuális hálózati hello első virtuális hálózat létrehozásakor hello társviszony-létesítési állapot: *kezdeményezett*.  Hello hello második virtuális hálózati toohello első virtuális hálózati társviszony létrehozásakor társviszony-létesítési állapotú-e *csatlakoztatva*. Ha hello hello első virtuális hálózati társviszony-létesítési állapot megtekintéséhez állapotának változása látható *kezdeményezett* túl*csatlakoztatva*. hello társviszony-létesítés nincs sikeresen létrehozva csak két virtuális hálózati társviszony hello társviszony-létesítési állapot *csatlakoztatva*. 
- Ha társviszony-létesítés hello klasszikus telepítési modell használatával létrehozott virtuális hálózatban a Resource Manager használatával létrehozott virtuális hálózatban, csak konfigurálnia a Resource Manager használatával telepített hello virtuális hálózati társviszony. A virtuális hálózat (klasszikus) vagy hello klasszikus üzembe helyezési modellben telepített két virtuális hálózatok közötti társviszony nem konfigurálható. Hello társviszony-létesítés hello virtuális hálózat (erőforrás-kezelő) toohello virtuális hálózat (klasszikus) létrehozásakor hello társviszony-létesítési állapot: *Frissítéskísérleti*, hamarosan megváltozik túl*csatlakoztatva*.   
- Társviszony-létesítés létrejön a virtuális hálózatok között. Nincsenek tranzitív esetében. Ha létre közötti esetében:
    - VirtualNetwork1 & VirtualNetwork2
    - VirtualNetwork2 & VirtualNetwork3

  Nincs nincs társviszony-létesítés VirtualNetwork1 és VirtualNetwork3 keresztül VirtualNetwork2 között. Ha toocreate egy virtuális hálózati társviszony-létesítés VirtualNetwork1 és VirtualNetwork3 között, hogy toocreate társviszony-létesítés VirtualNetwork1 és VirtualNetwork3 között.
- Alapértelmezett Azure névfeloldás használó társítottak, virtuális hálózatok neveinek nem oldható fel. más virtuális hálózatok nevei tooresolve, egy egyéni DNS-kiszolgálót kell használnia. hogyan olvassa el a saját DNS-kiszolgáló fel tooset toolearn hello [névfeloldáshoz a saját DNS-kiszolgáló](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) cikk.
- Mindkét virtuális hálózat hello társviszony-létesítés erőforrások kommunikálhatnak egymással hello azonos sávszélesség és a késleltetés, mintha hello azonos virtuális hálózaton. Minden virtuális gép méretét azonban rendelkezik saját maximális hálózati sávszélesség. További információk a különböző virtuális gépek méretét, olvassa el a hello maximális hálózati sávszélességének toolearn [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) vagy [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) virtuális gép méretének cikkeket.
- Virtuális hálózatok Resource Manager használatával telepített vannak a hello azonos, akkor is partnert másik előfizetést.
- Akkor is partnert különböző telepítési modellt a rendszer a hello azonos, a telepített virtuális hálózatot, vagy másik előfizetések (előzetes verzió). 
- hello előfizetések, amelyek mindkét virtuális hálózat kell társított toohello azonos Azure Active Directory-bérlő. Ha még nem rendelkezik az AD-bérlő, akkor gyorsan [hozzon létre egyet](../active-directory/develop/active-directory-howto-tenant.md?toc=%2fazure%2fvirtual-network%2ftoc.json#start-from-scratch). Használhatja a [VPN-átjáró](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V) tooconnect két virtuális hálózatokat különböző előfizetésekhez tartozó toodifferent Active Directory-bérlő.
- A virtuális hálózati társviszonyban tooanother virtuális hálózatot, és csatlakoztatott tooanother virtuális hálózatot az Azure-beli virtuális hálózat átjáróval. Ha a virtuális hálózatok társviszony-létesítés és egy átjárón keresztül csatlakoznak, a hello virtuális hálózatok közötti forgalom áthaladó hello társviszony-létesítési konfiguráció, nem pedig hello átjáró.
- Egy névleges díj vonatkozik a társhálózati viszonyt használó bejövő és kimenő forgalomra. További információkért lásd: hello [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/virtual-network).


## <a name="permissions"></a>Engedélyek

hello fiókjai virtuális hálózati társviszony-létesítés toocreate hello szükséges szerepkör- vagy hozzáférési engedélyekkel kell rendelkeznie. Például, ha két virtuális hálózatok myVnetA és myVnetB nevű volt társviszony, a fiókjához társítva kell lenni a következő minimális szerepkör vagy minden egyes virtuális hálózati engedélyeinek hello:
    
|Virtuális hálózat|Üzemi modell|Szerepkör|Engedélyek|
|---|---|---|---|
|myVnetA|Resource Manager|[Hálózati közreműködő](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write|
| |Klasszikus|[Klasszikus hálózati közreműködő](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|N/A|
|myVnetB|Resource Manager|[Hálózati közreműködő](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/peer|
||Klasszikus|[Klasszikus hálózati közreműködő](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|Microsoft.ClassicNetwork/virtualNetworks/peer|

További információ [beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) és konkrét engedélyeket túl[egyéni szerepkörök](../active-directory/role-based-access-control-custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (csak Resource Manager).

## <a name="next-steps"></a>Következő lépések

Megtudhatja, hogyan toocreate egy [küllős hálózati topológia](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) 
