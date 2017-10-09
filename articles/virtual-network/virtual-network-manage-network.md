---
title: "aaaCreate, módosítsa vagy törölje az Azure virtuális hálózat |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate, módosítsa vagy törölje a virtuális hálózat az Azure-ban."
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
ms.openlocfilehash: 7dfe6632753182eae2a13bb0327f03f75e03d057
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-change-or-delete-a-virtual-network"></a>Létrehozása, módosítása vagy a virtuális hálózat törlése

Ismerje meg, hogyan toocreate, törölje a virtuális hálózati és a módosítási beállítások, például a DNS-kiszolgálók és IP-cím szóközt tartalmaz, a meglévő virtuális hálózat.

Egy virtuális hálózat a saját hálózati hello felhőben megjelenítése. A virtuális hálózat hello Azure felhőben, amely dedikált tooyour Azure-előfizetés logikai elkülönítése. Minden virtuális hálózathoz, az Ön által létrehozott a következő műveletek végezhetők el:
- Válasszon egy cím terület tooassign. Egy vagy több címtartományai Classless Inter-Domain Routing (CIDR) jelölésrendszer, például a 10.0.0.0/16 által meghatározott egy áll.
- Toouse hello Azure által biztosított DNS-kiszolgálót válasszon, vagy a saját DNS-kiszolgáló használatára. Csatlakoztatott toohello virtuális hálózati erőforrásait a DNS-kiszolgáló tooresolve nevek hello virtuális hálózaton belül vannak hozzárendelve.
- Szegmens hello virtuális hálózati alhálózatokra, mindegyiket a saját címtartomány hello címtartomány a virtuális hálózat hello belül. Hogyan toocreate, módosítása és törlése alhálózatok: toolearn [hozzáadása, módosítása vagy törlése alhálózatok](virtual-network-manage-subnet.md).

Ez a cikk azt ismerteti, hogyan toocreate, módosítsa, majd törölje a virtuális hálózatok hello Azure Resource Manager telepítési modell használatával.

## <a name="before"></a>Előkészületek

Ebben a cikkben ismertetett hello feladatok megkezdése előtt hajtsa végre a következő előfeltételek hello:

- Ha új tooworking virtuális hálózatokat, azt javasoljuk, hogy tekintse át a hello gyakorlat [az első Azure virtuális hálózat létrehozása](virtual-network-get-started-vnet-subnet.md). Ebben a gyakorlatban segítségével Megismerkedés a virtuális hálózatok.
- virtuális hálózatok, tekintse át az határértékeit kapcsolatos toolearn [Azure korlátozását](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).
- Jelentkezzen be Azure-portálon toohello, hello Azure parancssori eszköz (Azure CLI), vagy az Azure PowerShell használatával az Azure-fiókjával. Ha nem rendelkezik Azure-fiókja, regisztráljon egy [ingyenes próbafiók](https://azure.microsoft.com/free).
- Ha azt tervezi, hogy a PowerShell-parancsok ebben a cikkben ismertetett toocomplete hello feladatok toouse, először [Azure PowerShell telepítése és konfigurálása](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Győződjön meg arról, hogy rendelkezik-e hello legújabb verziója található hello Azure PowerShell-parancsmagjai telepítve vannak-e. Adja meg a PowerShell-parancsok hello példák tooget súgóját `get-help <command> -full`.
- Ha azt tervezi, hogy az Azure parancssori felület parancsai ebben a cikkben ismertetett toocomplete hello feladatok toouse, először [telepítése és konfigurálása az Azure parancssori felület](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Győződjön meg arról, hogy rendelkezik-e telepítve az Azure parancssori felület legújabb verziójának hello. Azure parancssori felület parancsait tooget segítségre meg `az <command> --help`.


## <a name="create-vnet"></a>Virtuális hálózat létrehozása

a virtuális hálózati toocreate:

1. Jelentkezzen be toohello [portal](https://portal.azure.com) olyan fiókkal, amely hozzá van rendelve a hello hálózati közreműködői szerepkör (minimum) az előfizetéshez tartozó engedélyeit. toolearn szerepköröket és engedélyeket tooaccounts hozzárendelése bővebben lásd: [Azure szerepköralapú hozzáférés-vezérlés beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Kattintson a **új** > **hálózati** > **virtuális hálózati**.
3. A hello **virtuális hálózati** paneljén, hello **telepítési modell kiválasztása** mezőben hagyja **erőforrás-kezelő** kiválasztva, és kattintson **létrehozása**.
4. A hello **virtuális hálózat létrehozása** panelen adja meg vagy válassza ki a következő beállítások hello értékeit, majd kattintson **létrehozása**:
    - **Név**: hello egyedinek kell lennie a hello [erőforráscsoport](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group) toocreate hello virtuális hálózatot a választ. Hello neve nem módosítható, hello virtuális hálózat létrejötte után. Több virtuális hálózat adott idő alatt is létrehozhat. A elnevezésére vonatkozó javaslatokat, lásd: [elnevezési konvenciói](/azure/architecture/best-practices/naming-conventions.md?toc=%2fazure%2fvirtual-network%2ftoc.json#naming-rules-and-restrictions). A következő elnevezési segíthet az egyszerűbb toomanage több virtuális hálózat.
    - **Címtér**: hello címterület CIDR-formátumban adja meg. Megadhatja hello címterület public vagy private (az RFC 1918) lehet. Hello címterület nyilvánosként vagy magánként határozza meg, hogy hello címterület érhető el csak hello virtuális hálózathoz csatlakozó virtuális hálózatot, és a helyszíni hálózatokhoz, hogy csatlakozott-e toohello virtuális hálózati belül. Nem adható hozzá a következő címterek hello:
        - 224.0.0.0/4 (csoportos küldés)
        - 255.255.255.255/32 (közvetítés)
        - 127.0.0.0/8 (visszacsatolás)
        - 169.254.0.0/16 (kapcsolatszintű)
        - 168.63.129.16/32 (belső DNS)

      Bár csak egy címterület hello virtuális hálózat létrehozásakor definiálhat, további címterületeket hello virtuális hálózat létrejötte után adhat hozzá. toolearn hogyan tooadd egy címet a lemezterület-tooan már meglévő virtuális hálózatot, lásd: [hozzáadása vagy eltávolítása egy](#add-address-spaces) ebben a cikkben.

      >[!WARNING]
      >Ha egy virtuális hálózati cím szóközt tartalmaz, amelyek átfedik a másik virtuális hálózati vagy a helyszíni hálózattal, a két hello hálózatok nem lehet csatlakozni. Definiálása előtt célszerű egy címtartománnyal, vegye figyelembe, hogy érdemes tooconnect hello virtuális hálózati tooother virtuális hálózatok és a helyszíni hálózatokhoz hello a jövőbeli.
      >
      >

    - **Alhálózati név**: hello alhálózati név hello virtuális hálózaton belül egyedinek kell lennie. Hello alhálózat neve nem módosítható, miután hello alhálózat létre van hozva. hello portálhoz szükséges, érdemes megadni egy alhálózatot egy virtuális hálózatban, létrehozásakor annak ellenére, hogy a virtuális hálózat nem szükséges toohave alhálózatokkal. Hello portál csak egy alhálózat segítségével megadhatja, ha a virtuális hálózat létrehozása. Hello virtuális hálózat létrejötte után később adhat hozzá a virtuális hálózati további alhálózatokat toohello. tooadd alhálózati tooa virtuális hálózat, lásd: [hozzon létre egy alhálózatot](virtual-network-manage-subnet.md#create-subnet) a [létrehozása, módosítása vagy törlése alhálózatok](virtual-network-manage-subnet.md). Virtuális hálózat már több alhálózat működik az Azure parancssori felület vagy a PowerShell használatával is létrehozhat.

      >[!TIP]
      >Egyes esetekben rendszergazdák hozzon létre külön alhálózatokon toofilter vagy vezérlési forgalom-útválasztási hello alhálózatok között. Definiálása előtt célszerű alhálózatok, fontolja meg, hogyan lehet toofilter szeretne, és irányíthatja a forgalmat az alhálózatok között. toolearn alhálózatok között, a forgalom kapcsolatos további információkért lásd: [hálózati biztonsági csoportok](virtual-networks-nsg.md). Az Azure automatikusan útvonalak forgalom között az alhálózatokat, de szeretné felülbírálni az Azure alapértelmezett útvonalak. toolearn hogyan toooverride Azure alapértelmezett alhálózati a forgalom útválasztásához, lásd: [felhasználó által definiált útvonalak](virtual-networks-udr-overview.md).
      >

    - **Alhálózati címtartományt**: hello hello virtuális hálózathoz megadott hello címterület belül kell lennie. hello legkisebb tartományt is megadhat, amely lehetővé teszi nyolc IP-címek hello alhálózati /29,. Azure tartalék hello először, és minden alhálózatban protokoll megfelelési legutóbbi címek. Három további címek az Azure szolgáltatás használati számára vannak fenntartva. Ennek eredményeképpen a virtuális hálózat /29 alhálózati cím tartománnyal csak három használható IP-címmel rendelkezik. Ha azt tervezi, hogy a virtuális hálózati tooa VPN-átjáró tooconnect, létre kell hoznia egy átjáró-alhálózatot. További információ [adott cím tartomány szempontjai átjáró alhálózatok](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#gwsub). Hello címtartomány meghatározott feltételek hello alhálózati létrehozása után módosíthatja. Hogyan toochange a alhálózati címtartományt: toolearn [alhálózati beállítások módosítása](#change-subnet) a [hozzáadása, módosítása vagy törlése alhálózatok](virtual-network-manage-subnet.md).
    - **Előfizetés**: Válasszon egy [előfizetés](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription). Hello nem használhatja ugyanazt a virtuális hálózatot egynél több Azure-előfizetésben. Azonban egy előfizetés másik előfizetés toovirtual hálózatokhoz a virtuális hálózat is elérheti. virtuális hálózatok tooconnect különböző előfizetésekhez használja az Azure VPN Gateway vagy a virtuális hálózati társviszony-létesítés. Az Azure-erőforrásokkal, hogy a virtuális hálózati toohello csatlakozni kell hello hello virtuális hálózatnak ugyanahhoz az előfizetéshez.
    - **Erőforráscsoport**: Válasszon ki egy létező [erőforráscsoport](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-groups) vagy hozzon létre egy újat. Egy Azure-erőforrás, hogy a virtuális hálózati toohello csatlakozni lehet a hello ugyanabban az erőforráscsoportban hello virtuális hálózatot, vagy egy másik erőforráscsoportban található.
    - **Hely**: válassza ki az Azure [hely](https://azure.microsoft.com/regions/), más néven egy régiót. Virtuális hálózat csak egy Azure-beli hely lehet. Egy hely tooa virtuális hálózat egy másik helyre a virtuális hálózat azonban VPN-átjáró használatával is elérheti. Az Azure-erőforrásokkal, hogy a virtuális hálózati toohello csatlakozni kell hello hello virtuális hálózat ugyanazon a helyen.

**Parancsok**

|Eszköz|Parancs|
|---|---|
|Azure CLI|[az hálózati virtuális hálózat létrehozása](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[Új-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name = "view-vnet"></a>Virtuális hálózatok megjelenítése és beállítások

tooview virtuális hálózatok és a beállítások:

1. Jelentkezzen be toohello [portal](https://portal.azure.com) olyan fiókkal, amely hozzá van rendelve a hello hálózati közreműködői szerepkör (minimum) az előfizetéshez tartozó engedélyeit. toolearn szerepköröket és engedélyeket tooaccounts hozzárendelése bővebben lásd: [Azure szerepköralapú hozzáférés-vezérlés beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Hello portál keresési mezőbe, írja be a **virtuális hálózatok**. Hello keresési eredmények között kattintson **virtuális hálózatok**.
3. A hello **virtuális hálózatok** paneljén kattintson hello virtuális hálózathoz, amelyet tooview beállításait.
4. hello következő beállításai láthatók a kiválasztott virtuális hálózat hello hello panelen:
    - **Áttekintés**: hello virtuális hálózat, beleértve a címterület és a DNS-kiszolgálók információt nyújt. hello alábbi képernyőfelvételen látható a virtuális hálózat nevű hello áttekintése beállításainak **MyVNet**:

        ![Hálózati illesztő – áttekintés](./media/virtual-network-manage-network/vnet-overview.png)

      A hello **áttekintése** panelen áthelyezheti egy virtuális hálózati tooa másik előfizetés vagy az erőforrás csoporthoz. Hogyan toomove egy virtuális hálózat: toolearn [erőforrások tooa másik erőforráscsoportba vagy előfizetésbe áthelyezése](../azure-resource-manager/resource-group-move-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json). hello cikk Előfeltételek, és hogyan toomove erőforrásokat a hello Azure-portálon, PowerShell és az Azure parancssori felület sorolja fel. Csatlakoztatott toohello virtuális hálózati erőforrásait hello virtuális hálózathoz kell áthelyeznie.
    - **Címtér**: toohello virtuális hálózati hozzárendelt címterek hello vannak felsorolva. Hogyan tooadd és eltávolítása egy címtartománnyal kész toolearn hello lépéseit [hozzáadása vagy eltávolítása egy](#address-spaces) ebben a cikkben.
    - **Csatlakoztatott eszközök**: találhatók, amelyek csatlakoztatott toohello virtuális hálózati erőforrásokat. A fenti képernyőfelvételen hello három hálózati adapterrel és egy terheléselosztó olyan csatlakoztatott toohello virtuális hálózati. Bármely új erőforrások létrehozása, és a virtuális hálózat toohello vannak felsorolva. Ha töröl egy erőforrást, de a virtuális hálózathoz csatlakoztatott toohello, akkor nem fog többé megjelenni hello lista.
    - **Alhálózatok**: hello virtuális hálózaton belül található alhálózatok listája látható. Hogyan tooadd és eltávolítása egy alhálózaton: toolearn [hozzon létre egy alhálózatot](virtual-network-manage-subnet.md#create-subnet) és [alhálózat törlése](virtual-network-manage-subnet.md#delete-subnet) a [hozzáadása, módosítása vagy törlése alhálózatok](virtual-network-manage-subnet.md).
    - **DNS-kiszolgálók**: megadhatja, hogy hello Azure belső DNS-kiszolgáló vagy egy egyéni DNS-kiszolgálót biztosít névfeloldás csatlakoztatott toohello virtuális hálózati eszközöket. Hello Azure-portál használatával egy virtuális hálózatot hoz létre, amikor Azure DNS-kiszolgálók névfeloldásra a virtuális hálózaton belül által használt alapértelmezett. lépések teljes hello toomodify hello DNS-kiszolgálók, [hozzáadása, módosítása vagy eltávolítása a DNS-kiszolgáló](#dns-servers) ebben a cikkben.
    - **Társviszony**: Ha nincsenek meglévő társviszony hello az előfizetést, akkor az itt felsorolt. Meglévő társviszony beállítások megtekintése vagy létrehozása, módosítása vagy törlése esetében. toolearn esetében, kapcsolatos további információkért lásd: [virtuális hálózati társviszony-létesítés](virtual-network-peering-overview.md).
    - **Tulajdonságok**: beállításainak megjelenítése készül hello virtuális hálózat, beleértve a hello virtuális hálózati erőforrás-azonosító és az Azure-előfizetés hello.
    - **Diagram**: hello diagram csatlakoztatott toohello virtuális hálózat összes eszköz vizuális ábrázolását biztosítja. hello diagram néhány hello eszközökkel kapcsolatos legfontosabb tudnivalókat tartalmaz. Ebben a nézetben hello diagramon eszköz toomanage kattintson hello eszközre.
    - **Közös Azure beállításai**: toolearn közös Azure beállításokkal, kapcsolatos további információkért tekintse meg a következő információ hello:
        *   [Tevékenységnapló](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#activity-logs)
        *   [Hozzáférés-vezérlés (IAM)](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#access-control)
        *   [Címkék](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#tags)
        *   [Zárolások feloldása](../azure-resource-manager/resource-group-lock-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
        *   [Automatizálási parancsfájl](../azure-resource-manager/resource-manager-export-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json#export-the-template-from-resource-group)

**Parancsok**

|Eszköz|Parancs|
|---|---|
|Azure CLI|[az hálózati vnet megjelenítése](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#show)|
|PowerShell|[Get-AzureRmVirtualNetwork](/powershell/module/azurerm.network/get-azurermvirtualnetwork/?toc=%2fazure%2fvirtual-network%2ftoc.json)|


## <a name="add-address-spaces"></a>Hozzáadni vagy eltávolítani egy címtartománnyal

Adja hozzá, és távolítsa el a virtuális hálózat-címterét. A címterület CIDR-formátumban kell megadni, és nem lehetnek átfedésben más címterek hello belül az azonos virtuális hálózaton. Megadhatja hello címterek public vagy private (az RFC 1918) lehet. Hello címterület nyilvánosként vagy magánként határozza meg, hogy hello címterület érhető el csak hello virtuális hálózathoz csatlakozó virtuális hálózatot, és a helyszíni hálózatokhoz, hogy csatlakozott-e toohello virtuális hálózati belül. Nem adható hozzá a következő címterek hello:

- 224.0.0.0/4 (csoportos küldés)
- 255.255.255.255/32 (közvetítés)
- 127.0.0.0/8 (visszacsatolás)
- 169.254.0.0/16 (kapcsolatszintű)
- 168.63.129.16/32 (belső DNS)

tooadd, vagy távolítsa el az-címtér:

1. Jelentkezzen be toohello [portal](https://portal.azure.com) olyan fiókkal, amely hozzá van rendelve a hello hálózati közreműködői szerepkör (minimum) az előfizetéshez tartozó engedélyeit. toolearn szerepköröket és engedélyeket tooaccounts hozzárendelése bővebben lásd: [Azure szerepköralapú hozzáférés-vezérlés beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Hello portál keresési mezőbe, írja be a **virtuális hálózatok**. Hello keresési eredmények között, válassza ki a **virtuális hálózatok**.
3. A hello **virtuális hálózatok** panelen kattintson a virtuális hálózat hello, amelynek szeretné, hogy tooadd, vagy távolítsa el a címteret.
4. Hello virtuális hálózati panelen a **beállítások**, kattintson a **Címtéren**.
5. A hello címterület hello panelen végezze el hello alábbi beállítások egyikét:
    - **Egy címtartomány felvétele**: hello új címtartományt adjon meg. meglévő címterület hello virtuális hálózathoz definiált hello címterület nem lehet átfedésben.
    - **Távolítsa el az címtéren**: kattintson a jobb gombbal a címteret, és kattintson **eltávolítása**. Ha alhálózat létezik-e hello címtérben, hello címterület nem távolítható el. egy tooremove, először törölnie kell alhálózatok (és minden olyan erőforrásnál, amelyek csatlakoztatott toohello alhálózatok), amely létezik hello címterületen belülre.
6. Kattintson a **Save** (Mentés) gombra.

**Parancsok**

|Eszköz|Parancs|
|---|---|
|Azure CLI|Csak az erőforrás-kezelő|[az hálózat virtuális hálózat frissítése](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="dns-servers"></a>Hozzáadása, módosítása vagy eltávolítása a DNS-kiszolgáló

Minden virtuális gép csatlakoztatott toohello virtuális hálózatokon hello hello virtuális hálózathoz megadott DNS-kiszolgálók regisztrálása. Használják hello megadott DNS-kiszolgáló a névfeloldáshoz. Mindegyik hálózati interfész (NIC) a virtuális gép lehet a saját DNS-kiszolgáló beállításai. Ha egy hálózati Adaptert a saját DNS-kiszolgáló beállításait, azok felülírják hello hello virtuális hálózat DNS-kiszolgáló beállításait. toolearn hálózati adapter DNS beállításokkal kapcsolatos további információkért lásd: [hálózati illesztő feladatok és a beállítások](virtual-network-network-interface.md#change-dns-servers). toolearn névfeloldás virtuális gépek és az Azure Felhőszolgáltatások, szerepkörpéldányok kapcsolatos további információkért lásd: [névfeloldását virtuális gépek és a szerepkörpéldányok](virtual-networks-name-resolution-for-vms-and-role-instances.md). tooadd, módosítása vagy a DNS-kiszolgáló eltávolítása:

1. Jelentkezzen be toohello [portal](https://portal.azure.com) olyan fiókkal, amely hozzá van rendelve a hello hálózati közreműködői szerepkör (minimum) az előfizetéshez tartozó engedélyeit. toolearn szerepköröket és engedélyeket tooaccounts hozzárendelése bővebben lásd: [Azure szerepköralapú hozzáférés-vezérlés beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Hello portál keresési mezőbe, írja be a **virtuális hálózatok**. Hello keresési eredmények között, válassza ki a **virtuális hálózatok**.
3. A hello **virtuális hálózatok** panelen kattintson a hello toochange a DNS beállításokat a virtuális hálózat.
4. Hello virtuális hálózati panelen a **beállítások**, kattintson a **DNS-kiszolgálók**.
5. Válasszon egyet az alábbi beállítások a DNS-kiszolgálók felsoroló hello panelen hello:
    - **Alapértelmezett (Azure által biztosított)**: összes erőforrás nevét és privát IP-címek olyan automatikusan regisztrált toohello Azure DNS-kiszolgálók. Minden olyan erőforrásnál, amelyek csatlakoztatott toohello között oldhatja ugyanazt a virtuális hálózatot. Nem használhatja a beállításnevek tooresolve virtuális hálózatok között. virtuális hálózatok közötti tooresolve nevét, egy egyéni DNS-kiszolgálót kell használnia.
    - **Egyéni**: egy vagy több kiszolgáló hozzáadása is be toohello Azure korlátozza a virtuális hálózat. toolearn DNS-kiszolgálókra vonatkozó korlátok, kapcsolatos további információkért lásd: [Azure korlátozását](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#virtual-networking-limits-classic). Hello alábbi beállítások közül választhat:
        - **Adjon hozzá egy címet**: hello server tooyour virtuális hálózat DNS-kiszolgálók listája hozzáadja. Ez a beállítás hello DNS-kiszolgáló is regisztrálja az Azure-ral. Ha már egy DNS-kiszolgáló már regisztrálva van az Azure, kiválaszthatja, hogy a DNS-kiszolgáló hello listában.
        - **Távolítsa el az címet**: kattintson a következő toohello kiszolgálóra, amelyet az tooremove, **X**. Törlése hello kiszolgáló eltávolítása hello server csak a virtuális hálózatok listája. hello DNS-kiszolgáló továbbra is az Azure-ban a többi virtuális hálózatok toouse regisztrált.
        - **DNS-kiszolgálócímek átrendezése**: fontos, hogy a DNS-kiszolgálók hello felsorolja tooverify javítsa ki a ahhoz, hogy a környezetében. DNS-kiszolgálók listáját használt hello ahhoz, hogy azokat. A ciklikus multiplexelési telepítési nem működik. Hello lista hello első DNS-kiszolgáló elérhető, ha a hello ügyfél használja a DNS-kiszolgáló, függetlenül attól, hogy hello DNS-kiszolgáló megfelelően működik-e. Távolítsa el az összes felsorolt hello DNS-kiszolgáló, és adja őket újra a hello megfelelő.
        - **Módosítsa a címet**: hello DNS-kiszolgáló hello listában jelölje ki, és írja be hello új nevét.
6. Kattintson a **Save** (Mentés) gombra.
7. Indítsa újra a hello virtuális gépek, amelyek csatlakoztatott toohello virtuális hálózaton, így rendelt hello új DNS-kiszolgáló beállításai. Virtuális gépek továbbra is toouse az aktuális DNS-beállítások azok újraindításáig.

**Parancsok**

|Eszköz|Parancs|
|---|---|
|Azure CLI|[az hálózat virtuális hálózat frissítése](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="delete-vnet"></a>A virtuális hálózat törlése

Nincsenek erőforrások csatlakoztatott tooit esetén csak egy virtuális hálózati törölheti. Ha nincsenek erőforrások csatlakoztatott tooany alhálózati hello virtuális hálózaton belül, először törölnie kell hello erőforrásokat, amelyek csatlakoztatott tooall alhálózatok hello virtuális hálózaton belül. hello lépései toodelete erőforrás hello erőforrás függenek. Hogyan csatlakoztatott toosubnets toodelete erőforráshoz olvasási toolearn hello dokumentáció minden erőforrástípus toodelete keresi. a virtuális hálózati toodelete:

1. Jelentkezzen be toohello [portal](https://portal.azure.com) egy olyan fiókkal, amely hozzárendelt (minimum) engedélyeinek hello hálózat közreműködő szerepkört az előfizetés. toolearn szerepköröket és engedélyeket tooaccounts hozzárendelése bővebben lásd: [Azure szerepköralapú hozzáférés-vezérlés beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Hello portál keresési mezőbe, írja be a **virtuális hálózatok**. Hello keresési eredmények között kattintson **virtuális hálózatok**.
3. A hello **virtuális hálózatok** panelen, jelölje be hello virtuális hálózatot toodelete szeretné.
4. Hello virtuális hálózati paneljén, hogy nincsenek-e eszközök tooconfirm csatlakoztatott toohello virtuális hálózati, a **beállítások**, kattintson a **csatlakoztatott eszközök**. Ha csatlakoztatott eszközön, törölnie kell azokat hello virtuális hálózat törlése előtt. Ha nincsenek csatlakoztatott eszközök, kattintson a **áttekintése**.
5. Hello hello panel felső részén kattintson hello **törlése** ikonra.
6. tooconfirm hello törlésének hello virtuális hálózatot, kattintson a **Igen**.


**Parancsok**

|Eszköz|Parancs|
|---|---|
|Azure CLI|[Azure-hálózat virtuális hálózat törlése](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#delete)|
|PowerShell|[Remove-AzureRmVirtualNetwork](/powershell/module/azurerm.network/remove-azurermvirtualnetwork?toc=%2fazure%2fvirtual-network%2ftoc.json)|


## <a name="next-steps"></a>Következő lépések

- a virtuális gépek toocreate és tooa virtuális hálózathoz csatlakozzon, lásd: [hozzon létre egy virtuális hálózatot, és csatlakozzon a virtuális gépek](virtual-network-get-started-vnet-subnet.md#create-vms).
- Tekintse meg a toofilter hálózati forgalmat a virtuális hálózaton belül alhálózatok között [hálózati biztonsági csoportok létrehozása a](virtual-networks-create-nsg-arm-pportal.md).
- Tekintse meg a virtuális hálózat tooanother virtuális hálózat, toopeer [hozzon létre egy virtuális hálózati társviszony-létesítés](virtual-network-create-peering.md#portal).
- Csatlakozás a virtuális hálózat tooan a helyszíni hálózati beállításokkal kapcsolatos toolearn lásd [VPN-átjáró](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#diagrams).
