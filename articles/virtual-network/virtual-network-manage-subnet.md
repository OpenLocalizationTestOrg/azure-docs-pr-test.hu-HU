---
title: "aaaAdd, módosítása vagy egy Azure virtuális hálózati alhálózat törlése |} Microsoft Docs"
description: "Ismerje meg, hogyan tooadd, módosítsa vagy törölje a virtuális hálózati alhálózat az Azure-ban."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/10/2017
ms.author: jdial
ms.openlocfilehash: 0d6d813b7e2fc52a00a87f6c6714ab5b7ca589ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-change-or-delete-a-virtual-network-subnet"></a>Hozzáadása, módosítása vagy virtuális hálózati alhálózat törlése

Ismerje meg, hogyan tooadd, módosítsa vagy törölje a virtuális hálózati alhálózat. 

Ha még nem ismeri a virtuális hálózatok hozzáadása, módosítása vagy alhálózat törlése előtt, azt javasoljuk, hogy olvassa el [Azure Virtual Network áttekintése](virtual-networks-overview.md) és [létrehozása, módosítása vagy törlése a virtuális hálózati](virtual-network-manage-network.md). Egy virtuális hálózatot helyezett összes Azure-erőforrások egy alhálózatba a virtuális hálózaton belül vannak telepítve. Általában több alhálózatot a virtuális hálózaton belüli jönnek létre:
- **Alhálózatok közötti forgalmat**. Alkalmazhat a hálózati biztonsági csoportok toosubnets toofilter bejövő és kimenő hálózati forgalmat az összes erőforrás (például virtuális gépek) hello virtuális hálózatban. További részletek toolearn, hogyan toocreate a hálózati biztonsági csoportok: [hálózati biztonsági csoportok létrehozása a](virtual-networks-create-nsg-arm-pportal.md).
- **Szabályozhatja az alhálózatok közötti útválasztást**. Azure alapértelmezett útvonalak hoz létre, így automatikusan továbbítódik alhálózatok között. Felhasználó által definiált útvonalak létrehozásával felülbírálhatja az Azure alapértelmezett útvonalak. További információ a felhasználó által definiált útvonalak toolearn lásd [hozhat létre útvonalakat felhasználói](virtual-network-create-udr-arm-ps.md). 

Ez a cikk azt ismerteti, hogyan tooadd, módosítása és törlése hello Azure Resource Manager üzembe helyezési modellben használatával létrehozott virtuális hálózatok alhálózatot.
 
## <a name="before"></a>Előkészületek

Ebben a cikkben ismertetett hello feladatok megkezdése előtt hajtsa végre a következő előfeltételek hello:

- Ha új tooworking virtuális hálózatokat, azt javasoljuk, hogy tekintse át a hello gyakorlat [az első Azure virtuális hálózat létrehozása](virtual-network-get-started-vnet-subnet.md). Ebben a gyakorlatban segítségével Megismerkedés a virtuális hálózatok.
- virtuális hálózatok, tekintse át az határértékeit kapcsolatos toolearn [Azure korlátozását](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).
- Jelentkezzen be Azure-portálon toohello, hello Azure parancssori eszköz (Azure CLI), vagy az Azure PowerShell használatával az Azure-fiókjával. Ha nem rendelkezik Azure-fiókja, regisztráljon egy [ingyenes próbafiók](https://azure.microsoft.com/free).
- Ha azt tervezi, hogy a PowerShell-parancsok ebben a cikkben ismertetett toocomplete hello feladatok toouse, először [Azure PowerShell telepítése és konfigurálása](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Győződjön meg arról, hogy rendelkezik-e hello legújabb verziója található hello Azure PowerShell-parancsmagjai telepítve vannak-e. Adja meg a PowerShell-parancsok hello példák tooget súgóját `get-help <command> -full`.
- Ha azt tervezi, hogy az Azure parancssori felület parancsai ebben a cikkben ismertetett toocomplete hello feladatok toouse, először:
    - [Telepítse és konfigurálja az Azure parancssori felület hello](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Győződjön meg arról, hogy rendelkezik-e telepítve az Azure parancssori felület legújabb verziójának hello.
    - Azure Cloud rendszerhéj hello használja. Hello CLI és függőségeinek telepítése, helyett hello Azure Cloud rendszerhéj is használhatja. hello Azure Cloud rendszerhéj a szabad rendszerhéjakba futtatható közvetlenül hello Azure-portálon belül. Hello Azure CLI előtelepített és konfigurált toouse-fiókjához van. toouse hello felhő rendszerhéj, kattintson a hello felhő rendszerhéj (**> _**) ikonra hello Azure-portálon hello tetején. 

  Azure parancssori felület parancsait tooget segítségre meg `az <command> --help`.

## <a name="create-subnet"></a>Adjon hozzá egy alhálózatot

egy alhálózat tooadd:

1. Jelentkezzen be toohello [portal](https://portal.azure.com) olyan fiókkal, amely hozzá van rendelve a hello hálózati közreműködői szerepkör (minimum) az előfizetéshez tartozó engedélyeit. toolearn szerepköröket és engedélyeket tooaccounts hozzárendelése bővebben lásd: [Azure szerepköralapú hozzáférés-vezérlés beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Hello portál keresési mezőbe, írja be a **virtuális hálózatok**. Hello keresési eredmények között kattintson **virtuális hálózatok**.
3. A hello **virtuális hálózatok** panelen kattintson a hello virtuális hálózati alhálózat tooadd kívánt.
4. Hello virtuális hálózati paneljén kattintson **alhálózatok**.
5. Kattintson a **+ alhálózati**.
6. A hello **alhálózat hozzáadása** panelen adja meg a következő paraméterek hello:
    - **Név**: hello nevének hello virtuális hálózaton belül egyedinek kell lennie.
    - **Címtartomány**: hello tartomány hello címtartomány hello virtuális hálózaton belül egyedinek kell lennie. hello tartományon belül hello virtuális hálózat más alhálózati címtartományt nem lehet átfedésben. hello címterület Classless Inter-Domain Routing (CIDR) jelölésrendszer szerint kell adni. Például cím terület 10.0.0.0/16 rendelkező virtuális hálózatban, megadhat egy alhálózat címtartománya a 10.0.0.0/24. hello legkisebb tartományt is megadhat, amely lehetővé teszi nyolc IP-címek hello alhálózati /29,. Azure tartalék hello először, és minden alhálózatban protokoll megfelelési legutóbbi címek. Három további címek az Azure szolgáltatás használati számára vannak fenntartva. Ennek eredményeképpen meghatározása egy /29 alhálózat tartomány eredményezi, hogy három használható IP-címet a hello alhálózati cím. Ha azt tervezi, hogy a virtuális hálózati tooa VPN-átjáró tooconnect, létre kell hoznia egy átjáró-alhálózatot. További információ [adott cím tartomány szempontjai átjáró alhálózatok](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#gwsub). Hello címtartomány meghatározott feltételek hello alhálózat hozzáadása után módosíthatja. Hogyan toochange a alhálózati címtartományt: toolearn [alhálózati beállítások módosítása](#change-subnet) ebben a cikkben.
    - **Hálózati biztonsági csoport**: opcionálisan társíthatja a meglévő hálózati biztonsági csoportot hello alhálózati toocontrol hello alhálózat szűrés bejövő és kimenő hálózati forgalmat. hello hálózati biztonsági csoport már léteznie kell hello azonos előfizetésben és helyen hello virtuális hálózatként. Azt is kell használatával hozható létre hello Resource Manager üzembe helyezési modellben. További részletek toolearn, hogyan toocreate a hálózati biztonsági csoportok: [hálózati biztonsági csoportok](virtual-networks-create-nsg-arm-pportal.md).
    - **Az útvonaltábla**: opcionálisan társíthatja egy meglévő útvonaltábla hello alhálózati toocontrol hálózati forgalom-útválasztási tooother hálózatok. hello útvonal táblázatnak már léteznie kell a hello azonos előfizetésben és helyen hello virtuális hálózatként. Azt is kell használatával hozható létre hello Resource Manager üzembe helyezési modellben. További részletek toolearn, hogyan toocreate útvonaltábláit, lásd: [felhasználó által definiált útvonalak](virtual-network-create-udr-arm-ps.md).
    - **Felhasználók**: beépített szerepkörök vagy a saját egyéni szerepkörök használatával hozzáférést toohello alhálózati szabályozhatja. toolearn hozzárendelése a szerepkörök és a felhasználók tooaccess hello alhálózati, bővebben lásd: [használja a szerepkör hozzárendelése toomanage hozzáférés tooyour Azure erőforrások](../active-directory/role-based-access-control-configure.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-access).
7. tooadd hello alhálózati toohello virtuális hálózatot választotta, kattintson a **OK**.

**Parancsok**

|Eszköz|Parancs|
|---|---|
|Azure CLI|[az alhálózaton virtuális hálózat létrehozása](/cli/azure/network/vnet/subnet?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[Új AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig?toc=%2fazure%2fvirtual-network%2ftoc.json), [hozzáadása AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/add-azurermvirtualnetworksubnetconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="change-subnet"></a>Alhálózati beállítások módosítása

Hálózati biztonsági csoportok, útvonaltábláit és felhasználói hozzáférés tooa alhálózati módosíthatja egy alhálózaton lévő erőforrások kezelése. a beállításokkal kapcsolatban toolearn a [adjon hozzá egy alhálózatot](#create-subnet), lásd a 6. lépést. Ha azt szeretné, hogy toochange hello címterület alhálózaton, először törölnie kell minden olyan erőforrásnál, amely hello alhálózaton. hello lépései toodelete erőforrás hello erőforrás függenek. Hogyan alhálózatok, az egyes erőforrások dokumentációjában olvasható hello toodelete-erőforrást írja be a megjeleníteni kívánt toodelete toolearn. hello-címtartomány toochange alhálózathoz:

1. Jelentkezzen be toohello [portal](https://portal.azure.com) olyan fiókkal, amely hozzá van rendelve a hello hálózati közreműködői szerepkör (minimum) az előfizetéshez tartozó engedélyeit. toolearn szerepköröket és engedélyeket tooaccounts hozzárendelése bővebben lásd: [Azure szerepköralapú hozzáférés-vezérlés beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Hello portál keresési mezőbe, írja be a **virtuális hálózatok**. Hello keresési eredmények között kattintson **virtuális hálózatok**.
3. A hello **virtuális hálózatok** panelen kattintson a kívánt toochange egy alhálózati címtartományt hello virtuális hálózat.
4. Kattintson a kívánt toochange hello címtartomány hello alhálózatra.
5. Hello alhálózati panelen, a hello **-címtartományt** hello új címtartományt adjon meg. hello tartomány hello címtartomány hello virtuális hálózaton belül egyedinek kell lennie. hello tartományon belül hello virtuális hálózat más alhálózati címtartományt nem lehet átfedésben. hello címterület CIDR jelölésrendszer szerint kell adni. Például cím terület 10.0.0.0/16 rendelkező virtuális hálózatban, megadhat egy alhálózat címtartománya a 10.0.0.0/24. hello legkisebb tartományt is megadhat, amely lehetővé teszi nyolc IP-címek hello alhálózati /29,. Azure tartalék hello először, és minden alhálózatban protokoll megfelelési legutóbbi címek. Három további címek az Azure szolgáltatás használati számára vannak fenntartva. Ennek eredményeképpen a /29 alhálózat címtartománya három használható IP-címmel rendelkezik. Ha azt tervezi, hogy a virtuális hálózati tooa VPN-átjáró tooconnect, létre kell hoznia egy átjáró-alhálózatot. További információ [adott cím tartomány szempontjai átjáró alhálózatok](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#gwsub). Hello címtartomány meghatározott feltételek hello alhálózati létrehozása után módosíthatja. Hogyan toochange a alhálózati címtartományt: toolearn [alhálózati beállítások módosítása](#change-subnet) ebben a cikkben.
6. Kattintson a **Save** (Mentés) gombra.

**Parancsok**

|Eszköz|Parancs|
|---|---|
|Azure CLI|[az hálózat vnet alhálózat frissítése](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|


## <a name="delete-subnet"></a>Alhálózat törlése

Egy alhálózat csak akkor, ha nincsenek erőforrások hello alhálózat törölheti. Ha nincsenek erőforrások hello az alhálózaton, törölnie kell a hello-erőforrást hello alhálózati hello alhálózati törlése előtt. hello lépései toodelete erőforrás hello erőforrás függenek. Hogyan alhálózatok, az egyes erőforrások dokumentációjában olvasható hello toodelete-erőforrást írja be a megjeleníteni kívánt toodelete toolearn. egy alhálózat toodelete:

1. Jelentkezzen be toohello [portal](https://portal.azure.com) olyan fiókkal, amely hozzá van rendelve a hello hálózati közreműködői szerepkör (minimum) az előfizetéshez tartozó engedélyeit. toolearn szerepköröket és engedélyeket tooaccounts hozzárendelése bővebben lásd: [Azure szerepköralapú hozzáférés-vezérlés beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Hello portál keresési mezőbe, írja be a **virtuális hálózatok**. Hello keresési eredmények között kattintson **virtuális hálózatok**.
3. A hello **virtuális hálózatok** panelen kattintson hello virtuális hálózatra, amelyre toodelete az alhálózatot.
4. Hello virtuális hálózati panelen a **beállítások**, kattintson a **alhálózatok**.
5. Hello alhálózatok listájának megjelenő hello (alhálózatok) panel, kattintson a jobb gombbal hello alhálózati meg szeretné toodelete, kattintson a **törlése**, és kattintson a **Igen** toodelete hello alhálózat.

**Parancsok**

|Eszköz|Parancs|
|---|---|
|Azure CLI|[az hálózati virtuális hálózat törlése](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#delete)|
|PowerShell|[Remove-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/remove-azurermvirtualnetworksubnetconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="next-steps"></a>Következő lépések

az alhálózat, a virtuális gép toocreate lásd [virtuális hálózat létrehozása és központi telepítése a virtuális gépek hello alhálózat](virtual-network-get-started-vnet-subnet.md#create-vms).
