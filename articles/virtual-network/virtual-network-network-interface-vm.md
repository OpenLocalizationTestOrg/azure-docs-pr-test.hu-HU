---
title: "az Azure virtuális gépek eltávolítása aaaAdd hálózati illesztők tooor |} Microsoft Docs"
description: "Ismerje meg, hogyan tooadd hálózati illesztők tooor el a virtuális gépek hálózati adapterrel."
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
ms.date: 07/25/2017
ms.author: jdial
ms.openlocfilehash: eb564b94932b9242f29bc23234b08b0c76ac23a7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-network-interfaces-tooor-remove-from-virtual-machines"></a>Hálózati illesztők tooor távolítsa el a virtuális gépek hozzáadása

Ismerje meg, hogyan tooadd egy meglévő hálózati illesztő a virtuális gép létrehozásakor vagy hozzáadása vagy eltávolítása a hálózati adapterek hello a meglévő virtuális leállt (felszabadított) állapotát. Egy adott hálózati csatoló lehetővé teszi, hogy egy Azure virtuális gép (VM) toocommunicate Internet, Azure és a helyszíni erőforrások. A virtuális gépek is egy vagy több hálózati illesztőre van szükség. 

Ha tooadd van szüksége, módosítása vagy eltávolítása a hálózati illesztő IP-címek olvasási hello [hálózati illesztő IP-címeinek kezelése](virtual-network-network-interface-addresses.md) cikk. Ha toocreate van szüksége, módosítsa vagy törölje a hálózati adapter áll rendelkezésükre, olvassa el a hello [kezelése a hálózati adapterek](virtual-network-network-interface.md) cikk.

## <a name="before"></a>Előkészületek

Teljes hello feladatok bármelyik befejezése előtt a következő szakasz ebben a cikkben ismertetett visszaállítási lépésekkel:

- Ismerje meg, hány hálózati adapterekkel kapcsolatos minden egyes Linux és a Windows virtuális gép mérete hello megtekintésével támogatja [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) vagy [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) virtuális gép méretének cikkeket.
- Jelentkezzen be toohello Azure [portal](https://portal.azure.com), az Azure parancssori felület (CLI), vagy az Azure PowerShell használata az Azure-fiók. Ha még nem rendelkezik Azure-fiókja, regisztráljon egy [ingyenes próbafiók](https://azure.microsoft.com/free).
- Ha a PowerShell használatával parancsok toocomplete feladatok ebben a cikkben [Azure PowerShell telepítése és konfigurálása](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Ellenőrizze, hogy hello hello Azure PowerShell parancsmagjait telepített legújabb verziója. Írja be a PowerShell-parancsok, példákkal tooget súgóját `get-help <command> -full`.
- Ha az Azure parancssori felület (CLI) használatával parancsok toocomplete feladatok ebben a cikkben [telepítése és konfigurálása az Azure parancssori felület hello](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Ellenőrizze, hogy a legújabb verziójának hello hello Azure parancssori felület telepítve. Írja be a parancssori felület parancsait tooget súgóját `az <command> --help`. Ahelyett, hogy a parancssori felület telepítése hello és a szükséges előfeltételek hello Azure Cloud rendszerhéj is használhatja. hello Azure Cloud rendszerhéj a szabad rendszerhéjakba futtatható közvetlenül hello Azure-portálon belül. Hello Azure CLI előtelepített és konfigurált toouse-fiókjához van. toouse hello felhő rendszerhéj, kattintson a felhő rendszerhéj hello **> _** hello hello tetején gomb [portal](https://portal.azure.com).

## <a name="about"></a>Hálózati felületek és virtuális gépek

Hozzáadhat (csatolása) egy meglévő hálózati illesztő tooa VM hello virtuális gépek létrehozásakor megadott hello hálózati adapter jelenleg nem csatlakoztatott tooanother virtuális gép. A hálózati adaptert hozzáadása vagy eltávolítása (leválasztani) egy hálózati csatoló toofrom egy meglévő virtuális Gépen, feltéve hello VM a hello leállt (felszabadított) állapotát. Hello Azure-portál virtuális gép létrehozása, ha hello portálon létrehoz egy adott hálózati csatoló alapértelmezett beállításokkal. hello portál nem engedélyezi, hogy:

- Adjon meg egy meglévő hálózati illesztő tooadd hello virtuális gép létrehozásakor
- Több hálózati adapterrel rendelkező virtuális gép létrehozása
- Adjon meg egy nevet hello hálózati adapter (hello portálon hoz létre hello hálózati adapter egy alapértelmezett nevet)

Minden hello előző nem használható attribútumok hello portál Azure PowerShell vagy hello CLI toocreate a hálózati adaptert vagy virtuális gép használható. A következő részekben hello hello feladatok elvégzése előtt fontolja meg a következő hello korlátozások és viselkedéshez:

- Az összes Virtuálisgép-méretek legalább két hálózati adapterrel, azonban az egyes Virtuálisgép-méretek támogatja a több mint két hálózati adapterrel. A hello részhez, néhány virtuális gép mérete csak támogatja több hálózati adapter. toolearn támogatja az egyes Virtuálisgép-méretet, hogy hány hálózati illesztők olvasási hello [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) vagy [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) virtuális gép méretének cikkeket. 
- Elmúlt hello hálózati illesztők csak lehet, amely támogatja több hálózati adapterrel, és létre lettek hozva legalább két hálózati adapterrel rendelkező hozzáadott tooVMs. Nem sikerült hozzáadni a hálózati illesztő tooa virtuális Gépet, amely egy hálózati csatoló hozták létre, akkor is, ha a Virtuálisgép-méretet hello támogatott több hálózati adapterrel. Ezzel szemben csak eltávolítható hálózati adapterek legalább három hálózati adapterrel rendelkező virtuális gép alapján, mert mindig legalább két hálózati adapterrel létrehozott virtuális gépek toohave legalább két hálózati illesztőt. Ezek a megkötések egyike sem már érvényesek. Ön most hozzon létre egy virtuális Gépet tetszőleges számú, a hálózati adapterek (mentése hello virtuális gép mérete által támogatott toohello számot) és hozzáadhat és eltávolíthat tetszőleges számú (a virtuális gépek hello leállítva (felszabadítva) állapotú), a hálózati adapterek mindaddig hello virtuális gép mindig legalább egy hálózati adapterrel rendelkezik.
- Alapértelmezés szerint hello első hálózati adapter egy virtuális gépre van definiálva, hello *elsődleges* hálózati illesztőt. Virtuális gép hello más hálózati felületek *másodlagos* hálózati illesztőt.
- Elsődleges hálózati illesztők rendeli hozzá egy alapértelmezett átjáró hello Azure DHCP-kiszolgálók, a teljesség másodlagos hálózati adapterrel. Másodlagos hálózati adapter nem rendelkezik alapértelmezett átjárót, mivel azok nem kommunikálhatnak kívül az alhálózati erőforrások alapértelmezés szerint. tooenable másodlagos hálózati adapter van egy Windows virtuális gép toocommunicate erőforrásokkal kívül az alhálózati útvonalakat toohello operációs rendszer hello segítségével hozzáadása `route add` egy Windows parancssori parancsot. Linux virtuális gépekhez mivel hello alapértelmezett konfigurációját használja gyenge állomás útválasztási, érdemes másodlagos hálózati adapterek tooa egyetlen alhálózat forgalmának korlátozása. Ha másodlagos hálózati adapterrel, a csoportházirend-alapú útválasztási tooensure adott érkező engedélyezése szükséges hello alhálózati kívüli kapcsolatot és a kimenő forgalom hello használja ugyanazt a hálózati adapter.
- Alapértelmezés szerint minden kimenő forgalom hello virtuális gép által kiküldött hello IP-címet kap toohello elsődleges IP-konfiguráció hello elsődleges hálózati adapter. Szabályozhatja, hogy melyik IP-címet a kimenő forgalom hello virtuális gép operációs rendszerben használatos, de alapértelmezés szerint hello elsődleges hálózati kapcsolaton keresztül.
- Hello részhez, minden virtuális gép hello belül azonos rendelkezésre állási csoport egy vagy több, hálózati adapterek szükséges toohave voltak. Tetszőleges számú hálózati adapterek is léteznek a virtuális gépek hello azonos rendelkezésre állási készletbe, hello virtuális gép mérete által támogatott toohello telefonszámát. Csak egy virtuális gép tooan rendelkezésre állási csoportot, ha létrehozásakor adhat hozzá. További információk a rendelkezésre állási csoportok, olvassa el a hello toolearn [hello Azure virtuális gépek rendelkezésre állásának kezelése](../virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy) cikk.
- Amíg a hálózati adapterek hello azonos virtuális gép lehet egy Vneten belül csatlakoztatott toodifferent alhálózatok, hello hálózati illesztők csatlakoztatott toohello kell lennie ugyanazt a virtuális hálózatot.
- Bármely IP-konfiguráció bármely elsődleges vagy másodlagos hálózati illesztő tooan Azure Load Balancer háttér-címkészlet IP-címeket adhat hozzá. Az elmúlt hello csak hello elsődleges IP-cím hello elsődleges hálózati illesztő sikerült hozzáadni tooa háttér-készlet. több IP-címek és a konfigurációról, olvassa el a hello toolearn [hozzáadása, módosítása vagy eltávolítása IP-címek](virtual-network-network-interface-addresses.md) cikk.
- A virtuális gép törlése nem érinti használt csatolt tooit hello hálózati illesztőt. A virtuális gép törlésekor hello hálózati illesztők le vannak választva a virtuális gép hello. Hello hálózati illesztők toodifferent virtuális gépek hozzáadása, vagy törölje őket.
- Ha egy hálózati illesztőnek egy saját IPv6-cím tooit, csatolhatók tooa VM hello virtuális gép létrehozásakor. Hálózati illesztő – egy hozzárendelt IPv6-cím tooa virtuális gép nem lehet csatolni, hello virtuális gép létrehozása után. Hálózati illesztő – hozzárendelt saját IPv6-cím csatlakoztat egy virtuális gép létrehozásakor, ha csak csatolhat a hálózati illesztő toohello virtuális gép által támogatja a Virtuálisgép-méretet hello hány hálózati illesztők függetlenül. Lásd: [hálózati illesztő IP-címek](virtual-network-network-interface-addresses.md) toolearn bővebben hozzárendelés, IP-címek toonetwork felületek.

## <a name="vm-create"></a>Adja hozzá a meglévő hálózati adapterek tooa új virtuális gép

Hello portálon keresztül a virtuális gépek létrehozásakor hello portálon hoz létre egy adott hálózati csatoló alapértelmezett beállításokkal, és csatolja toohello VM meg. Nem vehető fel a meglévő hálózati adapterek tooa új virtuális Gépet, vagy hozzon létre egy virtuális gép több hálózati adapterrel hello Azure-portál használatával. Mindkét mindent parancssori felületen vagy a PowerShell használatával hello. Számos hálózati illesztők tooa VM hello hoz létre, támogatja a Virtuálisgép-méretet, adhat hozzá. hány hálózati kapcsolatos további információkért toolearn felületeihez minden virtuális gép mérete támogatja, olvassa el a hello [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) vagy [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) virtuális gép méretének cikkeket. hello hálózati illesztők ad hozzá a virtuális gép tooa jelenleg nem lehet csatolt tooanother virtuális gép. hálózati adapterek, olvassa el a hello létrehozásával kapcsolatos további toolearn [kezelése a hálózati adapterek](virtual-network-network-interface.md#create-a-network-interface) cikk.

> [!WARNING]
> Ha egy adott hálózati csatoló van rendelve a tooit saját IPv6-cím, csak adhat hello hálózati illesztő toohello virtuális gép hello virtuális gép létrehozásakor. Egynél több hálózati illesztő toohello virtuális gép hello virtuális gépet hoz létre, vagy hello után a virtuális gép jön létre, akkor nem lehet csatolni, mindaddig, amíg az IPv6-cím hozzá van rendelve a tooa hálózati kapcsolatát tooa virtuális gép. Lásd: [hálózati illesztő IP-címek](virtual-network-network-interface-addresses.md) toolearn bővebben hozzárendelés, IP-címek toonetwork felületek.

**Parancsok**

|Eszköz|Parancs|
|---|---|
|parancssori felület|[az virtuális gép létrehozása](/cli/azure/vm?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[Új AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="vm-add-nic"></a>Egy meglévő hálózati illesztő tooan meglévő virtuális gép hozzáadása

Számos hálózati illesztők tooa VM hello hálózati illesztők toosupports adja hozzá a virtuális gép méretét, adhat hozzá. toolearn támogatja az egyes Virtuálisgép-méretet, hogy hány hálózati illesztők olvasási hello [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) vagy [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) virtuális gép méretének cikkeket. azt szeretné, hogy a hálózati illesztő toomust támogatja a hálózati adapterek tooadd szeretné, és szerepelnie kell a hello leállt hello száma tooadd VM hello (felszabadítva) állapotát. azt szeretné, hogy tooadd hello hálózati illesztők jelenleg nem lehet csatolt tooanother virtuális gép. Meglévő Azure-portálon hello használó virtuális gépek hálózati illesztők tooan nem vehető fel. tooadd hálózati adapterek virtuális gép meglévő tooan, hello CLI vagy a PowerShell kell használnia. 

> [!WARNING]
> Ha egy adott hálózati csatoló van rendelve a tooit saját IPv6-cím, nem adható hozzá tooan meglévő virtuális gépet. Hálózati illesztő – egy hozzárendelt titkos IPv6 cím tooa virtuális gép csak egy virtuális gép létrehozásakor adhat hozzá. Lásd: [hálózati illesztő IP-címek](virtual-network-network-interface-addresses.md) toolearn bővebben hozzárendelés, IP-címek toonetwork felületek.

|Eszköz|Parancs|
|---|---|
|parancssori felület|[az vm hálózati adapter hozzáadása](/cli/azure/vm/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#add) (hivatkozás) vagy [részletes lépései](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-a-nic-to-a-vm)|
|PowerShell|[Adja hozzá AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json) (hivatkozás) vagy [részletes lépései](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-a-nic-to-an-existing-vm)|

## <a name="vm-view-nic"></a>A virtuális gépek nézet hálózati illesztők

Megtekintheti a hello hálózati illesztők jelenleg csatlakoztatott tooa VM toolearn mindegyik hálózati interfész konfigurálásával kapcsolatban, és hello IP-címek hozzárendelve tooeach hálózati illesztőt. 

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely hozzá van rendelve a hello tulajdonos, közreműködő vagy hálózat közreműködő szerepkört az előfizetés. További információ az szerepkörök tooaccounts hozzárendelése toolearn lásd: [Azure szerepköralapú hozzáférés-vezérlés beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Hello mezőben hello szöveget tartalmazó *keresési erőforrások* tetején hello hello Azure-portálon, írja be a *virtuális gépek*. Ha **virtuális gépek** jelenik meg a hello találatok, kattintson rá.
3. A hello **virtuális gépek** panel, amelyen megjelenik, kattintson a hello hello tooview hálózati adaptert használjon a kívánt virtuális gép nevét.
4. A hello **beállítások** szakasza hello virtuális gép paneljén megjelenő hello VM választotta, kattintson a **hálózati illesztőt**. hálózati kapcsolati beállítások kapcsolatos toolearn és hogyan toochange őket, olvassa el hello [kezelése a hálózati adapterek](virtual-network-network-interface.md) cikk. toolearn hozzáadása, módosítása, vagy távolítsa el IP-címtartományból tooa hálózati illesztő, lásd: [kezelése IP-címek](virtual-network-network-interface-addresses.md).

**Parancsok**

|Eszköz|Parancs|
|---|---|
|parancssori felület|[az vm megjelenítése](/cli/azure/vm?toc=%2fazure%2fvirtual-network%2ftoc.json#show)|
|PowerShell|[Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="vm-remove-nic"></a>Távolítsa el a hálózati adaptert egy virtuális gépből

hello azt szeretné tooremove (vagy leválasztani) a hálózati adaptert a virtuális gép és kell lennie a leállított (felszabadított) állapotot hello kell rendelkeznie legalább két hálózati adapter csatlakoztatva tooit. Eltávolíthatja a hálózati csatolóhoz, de a virtuális gép hello mindig rendelkeznie kell legalább egy hálózati kapcsolatát tooit. Ha eltávolítja az elsődleges hálózati illesztő, az Azure hello elsődleges attribútum toohello hálózati adapter, amelynek már csatlakoztatott toohello VM hello leghosszabb rendeli hozzá. 

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely hozzá van rendelve a hello tulajdonos, közreműködő vagy hálózat közreműködő szerepkört az előfizetés. További információ az szerepkörök tooaccounts hozzárendelése toolearn lásd: [Azure szerepköralapú hozzáférés-vezérlés beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Hello mezőben hello szöveget tartalmazó *keresési erőforrások* tetején hello hello Azure-portálon, írja be a *virtuális gépek*. Ha **virtuális gépek** jelenik meg a hello találatok, kattintson rá.
3. A hello **virtuális gépek** panel, amelyen megjelenik, kattintson egy hálózati csatoló a tooremove kívánt virtuális gép hello hello nevére.
4. A hello **beállítások** szakasza hello virtuális gép paneljén megjelenő hello VM választotta, kattintson a **hálózati illesztőt**. hálózati kapcsolati beállítások kapcsolatos toolearn és hogyan toochange őket, olvassa el hello [kezelése a hálózati adapterek](virtual-network-network-interface.md) cikk. toolearn hozzáadása, módosítása, vagy távolítsa el IP-címtartományból tooa hálózati illesztő, lásd: [kezelése IP-címek](virtual-network-network-interface-addresses.md).
5. A hello hálózati illesztők megjelenő panelen kattintson a hello **...**  toohello sarkában található, amelyet az toodetach hello hálózati illesztőt.
6. Kattintson a **leválasztani**. Ha csak egy hálózati kapcsolatát toohello virtuális gép, hello **leválasztási** lehetőség nem érhető el. Kattintson a **Igen** hello megerősítő be, amely akkor jelenik meg.

**Parancsok**

|Eszköz|Parancs|
|---|---|
|parancssori felület|[Távolítsa el a virtuális gép hálózati az](/cli/azure/vm/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#remove) (hivatkozás) vagy [részletes lépései](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#remove-a-nic-from-a-vm)|
|PowerShell|[Remove-AzureRMVMNetworkInterface](/powershell/module/azurerm.compute/remove-azurermvmnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json) (hivatkozás) vagy [részletes lépései](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#remove-a-nic-from-an-existing-vm)|

## <a name="next-steps"></a>Következő lépések
a következő cikkek hello toocreate több hálózati adapterrel rendelkező virtuális gépeknél és IP-címek, olvassa el:

**Parancsok**

|Tevékenység|Eszköz|
|---|---|
|Több hálózati adapterrel rendelkező virtuális gép létrehozása|[Parancssori felület](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
|Hozzon létre egy hálózati adapter virtuális több IPv4-címekkel|[Parancssori felület](virtual-network-multiple-ip-addresses-cli.md), [PowerShell](virtual-network-multiple-ip-addresses-powershell.md)|
|Hozzon létre egy hálózati adapter virtuális magánhálózati IPv6-cím (mögött egy Azure Load Balancer)|[Parancssori felület](../load-balancer/load-balancer-ipv6-internet-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../load-balancer/load-balancer-ipv6-internet-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [Azure Resource Manager-sablon](../load-balancer/load-balancer-ipv6-internet-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
