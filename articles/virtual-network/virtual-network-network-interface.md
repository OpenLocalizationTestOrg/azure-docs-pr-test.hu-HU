---
title: "aaaCreate, módosítsa vagy törölje az Azure hálózati illesztő |} Microsoft Docs"
description: "Ismerje meg, mi a hálózati adaptert, és hogyan toocreate, módosítsa a beállításokat, majd törölje valamelyik."
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
ms.date: 07/24/2017
ms.author: jdial
ms.openlocfilehash: dcb6cdbd73bc0d0ca4efb9d972d370cf2d54abb6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-change-or-delete-a-network-interface"></a>Létrehozása, módosítása vagy a hálózati illesztő törlése

Ismerje meg, hogyan toocreate, beállításainak módosítása és törlése a hálózati adaptert. Egy adott hálózati csatoló lehetővé teszi, hogy egy Azure virtuális gép toocommunicate Internet, Azure és a helyszíni erőforrások. Virtuális gép hello Azure-portálon történő létrehozásakor hello portal több hálózati adapter alapértelmezett beállításokat hoz létre. Ehelyett válassza ki a toocreate hálózati illesztők egyéni beállításokkal, és adja hozzá egy vagy több hálózati illesztők tooa virtuális gép létrehozásakor. Érdemes lehet is toochange alapértelmezett hálózati kapcsolati beállítások egy meglévő hálózati illesztő. Ez a cikk azt ismerteti, hogyan toocreate egy adott hálózati csatoló egyéni beállításokkal módosítsa a meglévő beállítások, például hálózati szűrő (hálózati biztonsági csoport) hozzárendelés alhálózat-hozzárendelés, DNS-kiszolgáló beállításai vagy IP-továbbítást, és a hálózati illesztő törlése.

Ha tooadd van szüksége, módosítása vagy eltávolítása a hálózati illesztő IP-címek olvasási hello [kezelése IP-címek](virtual-network-network-interface-addresses.md) cikk. Ha tooadd hálózati illesztőre van szükség, vagy távolítsa el a hálózati adapterek virtuális gépekről, olvassa el a hello [hozzáadása vagy eltávolítása a hálózati adapterek](virtual-network-network-interface-vm.md) cikk.


## <a name="before-you-begin"></a>Előkészületek

Teljes hello feladatok bármelyik befejezése előtt a következő szakasz ebben a cikkben ismertetett visszaállítási lépésekkel:

- Felülvizsgálati hello [Azure korlátozza](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) cikk toolearn kapcsolatos korlátozásokat a hálózati adaptereken.
- Jelentkezzen be toohello Azure [portal](https://portal.azure.com), az Azure parancssori felület (CLI), vagy az Azure PowerShell használata az Azure-fiók. Ha még nem rendelkezik Azure-fiókja, regisztráljon egy [ingyenes próbafiók](https://azure.microsoft.com/free).
- Ha a PowerShell használatával parancsok toocomplete feladatok ebben a cikkben [Azure PowerShell telepítése és konfigurálása](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Ellenőrizze, hogy hello hello Azure PowerShell parancsmagjait telepített legújabb verziója. Írja be a PowerShell-parancsok, példákkal tooget súgóját `get-help <command> -full`.
- Ha az Azure parancssori felület (CLI) használatával parancsok toocomplete feladatok ebben a cikkben [telepítése és konfigurálása az Azure parancssori felület hello](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Ellenőrizze, hogy a legújabb verziójának hello hello Azure parancssori felület telepítve. Írja be a parancssori felület parancsait tooget súgóját `az <command> --help`. Ahelyett, hogy a parancssori felület telepítése hello és a szükséges előfeltételek hello Azure Cloud rendszerhéj is használhatja. hello Azure Cloud rendszerhéj a szabad rendszerhéjakba futtatható közvetlenül hello Azure-portálon belül. Hello Azure CLI előtelepített és konfigurált toouse-fiókjához van. toouse hello felhő rendszerhéj, kattintson a felhő rendszerhéj hello **> _** hello hello tetején gomb [portal](https://portal.azure.com).

## <a name="create-a-network-interface"></a>A hálózati illesztő létrehozása

Virtuális gép hello Azure-portálon történő létrehozásakor hello portál egy adott hálózati csatoló alapértelmezett beállításokat hoz létre. A hálózati kapcsolati beállítások ahelyett, hogy meg kellene megadnia, ha egyéni beállításokkal hozza létre a hálózati adaptert, és csatolja hello hálózati illesztő tooa virtuális gép (a PowerShell vagy hello Azure parancssori felület használatával) hello virtuális gép létrehozásakor. Hozzon létre egy adott hálózati csatoló is, és tooan (a PowerShell vagy hello Azure parancssori felület használatával) meglévő virtuális gép felvétele. toolearn hogyan toocreate a már létező virtuális gép hálózati kapcsolat vagy a tooadd, vagy távolítsa el a hálózati adapterek a meglévő virtuális gépekről, olvassa el a hello [hozzáadása vagy eltávolítása a hálózati adapterek](virtual-network-network-interface-vm.md) cikk. Mielőtt létrehozna egy adott hálózati csatoló, rendelkeznie kell egy meglévő [virtuális hálózati](virtual-networks-create-vnet-arm-pportal.md) a hello azonos helyen és előfizetést hoz létre a hálózati illesztő.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely hozzárendelt (minimum) engedélyeinek hello hálózat közreműködő szerepkört az előfizetés. Olvasási hello [Azure szerepköralapú hozzáférés-vezérlés beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) cikk toolearn további szerepköröket és engedélyeket tooaccounts rendelése.
2. Hello mezőben hello szöveget tartalmazó *keresési erőforrások* tetején hello hello Azure-portálon, írja be a *hálózati illesztőt*. Ha **hálózati illesztőt** jelenik meg a hello találatok, kattintson rá.
3. A hello **hálózati illesztőt** panel, amelyen megjelenik, kattintson a **+ Hozzáadás**.
4. A hello **létrehozás hálózati illesztő** panelen megjelenő, adja meg, vagy válassza ki a következő beállítások hello értékeit, majd kattintson **létrehozása**:

    |Beállítás|Kötelező?|Részletek|
    |---|---|---|
    |Név|Igen|hello nevének választja hello erőforráscsoporton belül egyedinek kell lennie. Adott idő alatt akkor valószínűleg kell több hálózati adapterrel az Azure-előfizetésben. Olvasási hello [elnevezési konvenciói](/azure/architecture/best-practices/naming-conventions?toc=%2fazure%2fvirtual-network%2ftoc.json#naming-rules-and-restrictions) az javaslatok egy elnevezési konvenció toomake kezelése több létrehozásakor hálózati illesztők könnyebb a következő cikket. hello neve nem változtatható meg, hello hálózati illesztő létrehozása után.|
    |Virtuális hálózat|Igen|Válassza ki a hello virtuális hálózatot hello hálózati illesztő. Csak hozzárendelheti a hálózati illesztő tooa virtuális hálózat létezik a hello azonos előfizetésben és helyen hello hálózati adapterként. A hálózati illesztő létrehozása után nem módosítható hello virtuális hálózathoz van hozzárendelve. hello hello hálózati illesztő toomust hozzáadhat virtuális gép is létezik a hello azonos helyen és az előfizetés hello hálózati adapterként.|
    |Alhálózat|Igen|Válassza ki egy alhálózatot hello kiválasztott virtuális hálózaton belül. Módosíthatja a hello alhálózati hello hálózati illesztő létrehozása tooafter van hozzárendelve.|
    |Privát IP-cím hozzárendelése|Igen| Ebben a beállításban hello hozzárendelési módszer hello IPv4-cím van kiválasztása. Hello hozzárendelési módszert a következő lehetőségek közül választhat: **dinamikus:** Ha ezt a lehetőséget választja, Azure automatikusan rendeli hozzá egy címet hello címterület hello alhálózat választotta. Azure előfordulhat, hogy rendeljen egy másik címet tooa hálózati adapteren, amikor az hello virtuális gép indításánál a hello lett leállította (felszabadított) állapotát. hello cím marad hello azonos Ha hello virtuális gép újraindítása nélkül a hello lett leállítva (felszabadított) állapotát. **Statikus:** Ha ezt a lehetőséget választja, kézzel kell rendelnie egy szabad IP-cím hello címterület kiválasztott hello alhálózat belül. Statikus címeket nem megváltoztatni, amíg meg nem módosítja őket, vagy hello hálózati illesztő törlése. Hello hozzárendelési módszert hello hálózati illesztő létrehozása után módosíthatja. hello Azure DHCP-kiszolgáló címe toohello hálózati illesztő belül hello virtuális gép operációs rendszere hello rendeli hozzá.|
    |Hálózati biztonsági csoport|Nem| Hagyja beállítása túl**nincs**, válasszon ki egy létező [hálózati biztonsági csoport](virtual-networks-nsg.md), vagy [hálózati biztonsági csoport létrehozása](virtual-networks-create-nsg-arm-pportal.md). Hálózati biztonsági csoportok lehetővé teszik a toofilter hálózati forgalom mindkét egy adott hálózati csatoló. Nulla vagy egy hálózati biztonsági csoport tooa hálózati adapter is alkalmazhatja. Nulla vagy egy hálózati biztonsági csoport is alkalmazhatók toohello alhálózati hello hálózati adapter van hozzárendelve. Ha a hálózati biztonsági csoport-e a alkalmazott tooa hálózati adapter és a hello alhálózati hello hálózati adapter van hozzárendelve, néha váratlan eredményekhez. tootroubleshoot hálózati biztonsági csoportok alkalmazott toonetwork felületek és -alhálózatok, olvassa el a hello [hibaelhárítása a hálózati biztonsági csoportok](virtual-network-nsg-troubleshoot-portal.md#nsg) cikk.|
    |Előfizetés|Igen|Válasszon egyet az Azure [előfizetések](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription). hello virtuális gép csatlakoztatása a hálózati illesztő tooand hello virtuális hálózati csatlakoztatja toomust szerepel hello azonos előfizetéssel.|
    |Magánhálózati IP-cím (IPv6)|Nem| Ha bejelöli ezt a jelölőnégyzetet, az IPv6-címek toohello hozzárendelt hálózati illesztő, továbbá toohello IPv4-cím hozzárendelése toohello hálózati illesztőt. Lásd: hello [IPv6](#IPv6) című szakaszban a fontos adatokat IPv6 hálózati adapterrel együtt. Nem választhat ki olyan hello IPv6-cím hozzárendelés módszert. Ha úgy dönt, tooassign IPv6-címet, dinamikus módszerrel hello van hozzárendelve.
    |IPv6-név (csak akkor jelenik meg, amikor hello **magánhálózati IP-cím (IPv6)** jelölőnégyzet be van jelölve) |Igen, ha hello **magánhálózati IP-cím (IPv6)** jelölőnégyzet be van jelölve.| Ez a név tooa másodlagos IP-konfiguráció hello hálózati adapter van hozzárendelve. További információ a hello IP-konfigurációk [hálózati kapcsolati beállítások megtekintése](#view-network-interface-settings) című szakaszát.|
    |Erőforráscsoport|Igen|Válasszon ki egy létező [erőforráscsoport](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group) vagy hozzon létre egyet. A hálózati adaptert is létezik hello azonos, vagy másik erőforráscsoportban található, mint hello virtuális gépnek csatlakoztassa, vagy hello virtuális hálózati kösse össze.|
    |Hely|Igen|hello virtuális gép csatlakoztatása a hálózati illesztő tooand hello virtuális hálózati csatlakoztatja toomust szerepel hello azonos [hely](https://azure.microsoft.com/regions), más néven tooas egy régiót.|

hello portál nem biztosít hello beállítás tooassign egy nyilvános IP-cím toohello hálózati kapcsolat létrehozásakor, bár a hello portálon hozzon létre egy nyilvános IP-címet, és rendelje hozzá tooa hálózati illesztő hello portál virtuális gép létrehozásakor. Hogyan tooadd nyilvános IP-cím toohello hálózati csatoló a létrehozást követően olvasni hello toolearn [kezelése IP-címek](virtual-network-network-interface-addresses.md) cikk. Ha azt szeretné, hogy a nyilvános IP-címek a hálózati illesztő toocreate, hello parancssori felületen vagy a PowerShell toocreate hello hálózati illesztő kell használnia.

>[!Note]
> A MAC cím toohello hálózati adaptert csak után hello hálózati illesztő virtuális géphez csatolt tooa és hello virtuális gép Azure rendel az első alkalommal hello elindult. Nem adható meg, hogy Azure hozzárendel toohello hálózati illesztő hello MAC-címet. MAC cím rendelve marad toohello hálózati illesztő hello hello hálózati illesztő nem törlik, vagy hello hozzárendelt elsődleges IP-konfiguráció toohello hello elsődleges hálózati adapter magánhálózati IP-cím módosul. További információk az IP-címek és IP-konfigurációk, olvassa el a hello toolearn [kezelése IP-címek](virtual-network-network-interface-addresses.md) cikk.

**Parancsok**

|Eszköz|Parancs|
|---|---|
|parancssori felület|[az hálózati hálózati adapter létrehozása](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[Új AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|

## <a name="view-network-interface-settings"></a>Hálózati kapcsolat beállításainak megjelenítése

Megtekintheti és módosíthatja a legtöbb beállítást egy adott hálózati csatoló létrehozása után.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely hozzárendelt (minimum) engedélyeinek hello hálózat közreműködő szerepkört az előfizetés. Olvasási hello [Azure szerepköralapú hozzáférés-vezérlés beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) cikk toolearn további szerepköröket és engedélyeket tooaccounts rendelése.
2. Hello mezőben hello szöveget tartalmazó *keresési erőforrások* tetején hello hello Azure-portálon, írja be a *hálózati illesztőt*. Ha **hálózati illesztőt** jelenik meg a hello találatok, kattintson rá.
3. A hello **hálózati illesztőt** panel, amelyen megjelenik, kattintson a hello hálózati illesztő tooview szeretné, vagy módosítsa a beállításokat.
4. hello alábbi beállítások találhatók hello panelen megjelenő kiválasztott hello hálózati adapter:
    - **Áttekintés:** hello hálózati kapcsolat információkat nyújt, például a hello IP-címek hozzárendelve tooit, hello virtuális hálózatot/alhálózatot hello hálózati adapter van hozzárendelve, és hello virtuális gép hello hálózati illesztő túl csatlakozik (Ha kapcsolódik tooone). hello alábbi képen látható hello áttekintése beállításait egy adott hálózati csatoló nevű **mywebserver256**: ![hálózati illesztő – áttekintés](./media/virtual-network-network-interface/nic-overview.png) áthelyezheti a hálózati illesztő tooa eltérő erőforráscsoportban vagy előfizetés kattintva (**módosítása**) következő toohello **erőforráscsoport** vagy **előfizetés neve**. Hello hálózati illesztő helyezi át, ha minden erőforrások kapcsolódó toohello hálózati illesztő – azt kell áthelyeznie. Ha hello hálózati illesztő virtuális géphez csatolt tooa, például akkor is át kell helyezni hello virtuális gép és az egyéb kapcsolódó virtuális gép erőforrásokhoz. olvassa el a hello toomove egy adott hálózati csatoló [erőforrás tooa új erőforráscsoportba vagy előfizetésbe áthelyezése](../azure-resource-manager/resource-group-move-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json#use-portal) cikk. hello cikk Előfeltételek, és hogyan toomove-erőforrások hello Azure-portálon PowerShell, és az Azure parancssori felület hello sorolja fel.
    - **IP-konfiguráció:** nyilvános és magánhálózati IPv4 és IPv6 típusú címek tooIP konfigurációk hozzárendelt az itt felsorolt. Ha IPv6-cím hozzá van rendelve tooan IP-konfiguráció, hello cím nem jelenik meg. További információk IP-konfigurációk, és hogyan tooadd és eltávolítás IP-címekre hello [konfigurálja az IP-címeket az Azure hálózati illesztő](virtual-network-network-interface-addresses.md) cikk. IP-továbbítás és alhálózat-hozzárendelés ebben a szakaszban is vannak konfigurálva. További információk ezeket a beállításokat, olvassa el a hello toolearn [engedélyezi vagy letiltja az IP-továbbítás](#enable-or-disable-ip-forwarding) és [alhálózat-hozzárendelés módosítása](#change-subnet-assignment) a cikk szakaszaiban.
    - **DNS-kiszolgálók:** megadhatja, hogy melyik DNS-kiszolgálót egy adott hálózati csatoló által hozzárendelt hello Azure DHCP-kiszolgálók. hello hálózati adapter is hello beállítás öröklése a hello virtuális hálózati hello hálózati illesztő hozzá van rendelve, vagy hello beállítás hello virtuális hálózat társítva a felülbíráló egyéni beállításokkal rendelkeznek. Mi jelenjen meg toomodify, teljes hello szükséges lépések hello [módosítás DNS-kiszolgálók](#change-dns-servers) című szakaszát.
    - **Hálózati biztonsági csoport (NSG):** jeleníti meg, amely NSG társított toohello hálózati illesztőt (ha van ilyen). Az NSG bejövő és kimenő szabályok toofilter hálózati forgalmat hello hálózati illesztő tartalmazza. Ha egy NSG társított toohello hálózati kapcsolat nevét hello hello társított NSG jelenik meg. Mi jelenjen meg toomodify, teljes hello szükséges lépések hello [kezelése a hálózati biztonsági csoport társítását](virtual-network-manage-nsg-arm-portal.md#manage-associations) cikk.
    - **Tulajdonságok:** kulcs hello hálózati felületén, beleértve a MAC-címét (ha hello hálózati adapter nem virtuális géphez csatolt tooa üres) vonatkozó beállítások, és megtalálható a előfizetés hello jeleníti meg.
    - **Hatékony biztonsági szabályokat:** biztonsági szabályok láthatók, ha hello hálózati illesztő csatolt tooa virtuális gépet futtat, és egy NSG toohello társított felülettel, hello alhálózati hozzá van rendelve, vagy mindkettőt. toolearn megjelenített, kapcsolatos további információkért olvassa el a hello [hibaelhárítása a hálózati biztonsági csoportok](virtual-network-nsg-troubleshoot-portal.md#nsg) cikk. További információk az NSG-k, olvassa el a hello toolearn [hálózati biztonsági csoportok](virtual-networks-nsg.md) cikk.
    - **Hatékony útvonalak:** útvonalak hello hálózati adapter esetén is fut a virtuális géphez csatolt tooa találhatók. hello útvonalak hello Azure alapértelmezett útvonalak, bármely felhasználó által definiált útvonalak (UDR) és a BGP-útvonalakat, amelyek a hello alhálózati hello hálózati illesztő hozzá van rendelve. Előfordulhat, hogy létezik olyan. toolearn megjelenített, kapcsolatos további információkért olvassa el a hello [útvonalak hibaelhárítása](virtual-network-routes-troubleshoot-portal.md#view-effective-routes-for-a-network-interface) cikk. További információk az Azure alapértelmezett és udr-EK, olvassa el a hello toolearn [felhasználó által definiált útvonalak](virtual-networks-udr-overview.md) cikk.
    - **Közös Azure Resource Manager-beállítások:** további információk a közös Azure Resource Manager-beállítások, olvassa el a hello toolearn [tevékenységnapló](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#activity-logs), [hozzáférés-vezérlés (IAM)](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#access-control), [címkék ](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#tags), [Zárolja](../azure-resource-manager/resource-group-lock-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json), és [automatizálási parancsfájl](../azure-resource-manager/resource-manager-export-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json#export-the-template-from-resource-group) cikkeket.

**Parancsok**

IPv6-cím van hozzárendelve, tooa hálózati illesztő, hello PowerShell kimeneti hello tényt, hogy hello cím hozzá van rendelve, de nem vissza hozzárendelt hello címet adja vissza. Hasonlóképpen, a CLI-t adja vissza, hello tényt, hogy hello cím hozzá van rendelve, de ad vissza hello *null* hello cím kimenete.

|Eszköz|Parancs|
|---|---|
|parancssori felület|[az a hálózati adapter lista](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#list) tooview hálózati illesztők hello előfizetés; [az hálózati nic megjelenítése](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#show) tooview beállításait egy adott hálózati csatoló|
|PowerShell|[Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json) hello előfizetés vagy nézet beállításait egy adott hálózati csatoló tooview hálózati illesztők|

## <a name="change-dns-servers"></a>Módosítsa a DNS-kiszolgálók

hello DNS-kiszolgáló hozzá van rendelve, által hello Azure-DHCP server toohello hálózati csatoló hello virtuális gép operációs rendszerében. hozzárendelt hello DNS-kiszolgáló bármilyen hello DNS-kiszolgáló beállítása a hálózati illesztő. toolearn egy adott hálózati csatoló név feloldása beállításokkal kapcsolatos további információkért lásd: [névfeloldás a virtuális gépek](virtual-networks-name-resolution-for-vms-and-role-instances.md). hello hálózati illesztő hello-beállítások örökléséhez hello virtuális hálózatból, vagy használja a saját egyedi beállításokat, amelyek a virtuális hálózat hello hello beállításának felülbírálása.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely hozzárendelt (minimum) engedélyeinek hello hálózat közreműködő szerepkört az előfizetés. Olvasási hello [Azure szerepköralapú hozzáférés-vezérlés beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) cikk toolearn további szerepköröket és engedélyeket tooaccounts rendelése.
2. Hello mezőben hello szöveget tartalmazó *keresési erőforrások* tetején hello hello Azure-portálon, írja be a *hálózati illesztőt*. Ha **hálózati illesztőt** jelenik meg a hello találatok, kattintson rá.
3. A hello **hálózati illesztőt** panel, amelyen megjelenik, kattintson a hello hálózati illesztő tooview szeretné, vagy módosítsa a beállításokat.
4. Hello hálózati illesztő kiválasztott hello paneljén kattintson **DNS-kiszolgálók** alatt **beállítások**.
5. Jelölje be az:
    - **Virtuális hálózat (alapértelmezett) örökölhet**: válassza ezt a beállítást tooinherit hello DNS-kiszolgáló beállítása meg van adva hello virtuális hálózati hello hálózati adapter van hozzárendelve. Egy egyéni DNS-kiszolgáló vagy a hello Azure által biztosított DNS-kiszolgáló hello virtuális hálózat szintjén van meghatározva. hello Azure által biztosított DNS-kiszolgáló fel tudja oldani a hozzárendelt erőforrások toohello állomásnevek ugyanazt a virtuális hálózatot. Teljes Tartománynevének kell lennie az erőforrásokhoz hozzárendelt toodifferent virtuális hálózatok használt tooresolve.
    - **Egyéni**: a saját DNS-kiszolgáló tooresolve nevek több virtuális hálózat is konfigurálhatók. Adja meg az IP-cím hello hello kiszolgáló DNS-kiszolgálóként toouse szeretné. hello DNS-kiszolgáló címét adja meg, hogy csak a toothis hálózati adapter és a felülbírálások hello virtuális hálózati hello hálózati illesztő DNS beállítás be van-e rendelve van hozzárendelve.
6. Kattintson a **Save** (Mentés) gombra.

**Parancsok**

|Eszköz|Parancs|
|---|---|
|parancssori felület|[az hálózat hálózati adapter frissítése](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Set-AzureRmNetworkInterface](/powershell/module/azurerm.network/set-azurermnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="enable-or-disable-ip-forwarding"></a>Engedélyezi vagy letiltja az IP-továbbítás

IP-továbbítás lehetővé teszi, hogy a hello virtuális gép egy hálózati adapter csatlakozik:
- Nem hozzárendelt az hello IP-konfigurációjának hozzárendelt hálózati illesztő toohello tooany hello IP-címek egyikére szánt hálózati forgalom fogadására.
- Mint az egy adott hálózati csatoló IP-konfigurációjának hozzárendelt tooone egy hello eltérő forrás IP-címek a hálózati forgalom elküldése.

minden hálózati adapter, amely csatolt toohello, hogy a virtuális gép igényeinek tooforward hello forgalmat fogadó virtuális gép hello beállítást engedélyezni kell. A virtuális gépek is továbbítsa a forgalmat, hogy több hálózati adapter vagy egy egyetlen hálózati kapcsolatát tooit rendelkezik. Egy Azure beállítás pedig a IP-továbbítás hello virtuális gép is futtatnia kell az alkalmazás képes tooforward hello forgalom, például tűzfalat, WAN-optimalizálást és terheléselosztást végző alkalmazások. A virtuális gép hálózati alkalmazásokat futtató, hello virtuális gép esetén gyakran hivatkozott tooas egy virtuális hálózati berendezések. Készen áll a toodeploy hálózati virtuális készülékek listáját megtekintheti a hello [Azure piactér](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances). IP-továbbítás jellemzően a felhasználó által definiált útvonalak. További információk a felhasználó által definiált útvonalak, olvassa el a hello toolearn [felhasználó által definiált útvonalak](virtual-networks-udr-overview.md) cikk.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely hozzárendelt (minimum) engedélyeinek hello hálózat közreműködő szerepkört az előfizetés. Olvasási hello [Azure szerepköralapú hozzáférés-vezérlés beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) cikk toolearn további szerepköröket és engedélyeket tooaccounts rendelése.
2. Hello mezőben hello szöveget tartalmazó *keresési erőforrások* tetején hello hello Azure-portálon, írja be a *hálózati illesztőt*. Ha **hálózati illesztőt** jelenik meg a hello találatok, kattintson rá.
3. A hello **hálózati illesztőt** panel, amelyen megjelenik, kattintson a hello hálózati illesztő tooenable szeretné, vagy tiltsa le a továbbítási IP.
4. Hello hálózati illesztő kiválasztott hello paneljén kattintson **IP-konfigurációk** a hello **beállítások** szakasz.
5. Kattintson a **engedélyezve** vagy **letiltott** (alapértelmezett beállítás) toochange hello beállítást.
6. Kattintson a **Save** (Mentés) gombra.

**Parancsok**

|Eszköz|Parancs|
|---|---|
|parancssori felület|[az hálózat hálózati adapter frissítése](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Set-AzureRmNetworkInterface](/powershell/module/azurerm.network/set-azurermnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="change-subnet-assignment"></a>Alhálózat-hozzárendelés módosítása

Hello alhálózati, de nem hello virtuális hálózaton, egy adott hálózati csatoló rendelt módosíthatja.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely hozzárendelt (minimum) engedélyeinek hello hálózat közreműködő szerepkört az előfizetés. Olvasási hello [Azure szerepköralapú hozzáférés-vezérlés beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) cikk toolearn további szerepköröket és engedélyeket tooaccounts rendelése.
2. Hello mezőben hello szöveget tartalmazó *keresési erőforrások* tetején hello hello Azure-portálon, írja be a *hálózati illesztőt*. Ha **hálózati illesztőt** jelenik meg a hello találatok, kattintson rá.
3. A hello **hálózati illesztőt** panel, amelyen megjelenik, kattintson a hello hálózati illesztő tooview szeretné, vagy módosítsa a beállításokat.
4. Kattintson a **IP-konfigurációk** alatt **beállítások** hello panelen kiválasztott hello hálózati illesztő. Ha az összes IP-konfiguráció magánhálózati IP-címek felsorolt **(statikus)** következő toothem, meg kell változtatnia hello IP cím hozzárendelés metódus toodynamic hello következő lépések végrehajtásával. Összes magán IP-címet kell rendelni, amelyben hello dinamikus hozzárendelési módszer toochange hello alhálózat-hozzárendelés hello hálózati illesztő. Hello címek hozzárendelésének hello dinamikus metódussal, ha továbbra is toostep öt. Ha minden IPv4-címek hello statikus hozzárendelési módszer van hozzárendelve, hajtsa végre a következő lépéseket toochange hello hozzárendelési módszer toodynamic hello:
    - Kattintson a kívánt toochange hello IPv4 cím hozzárendelési módszert az IP-konfigurációk közül hello hello IP-konfigurációt.
    - Hello panelen megjelenő hello IP-konfiguráció kattintson **dinamikus** a hello **hozzárendelés** metódust. Nem rendelhető hozzá az IPv6-címek hello statikus hozzárendelés metódussal.
    - Kattintson a **Save** (Mentés) gombra.
5. Válassza ki a kívánt tooconnect hello hálózati illesztő toofrom hello hello alhálózati **alhálózati** legördülő listából.
6. Kattintson a **Save** (Mentés) gombra. Új dinamikus címek hello alhálózati címtartományt hello új alhálózat rendeli. Hello hálózati illesztő tooa új alhálózat hozzárendelése, után hozzárendelheti egy statikus IPv4-címet az új alhálózati címtartományt hello Ha úgy dönt. További információk hozzáadása, módosítása és eltávolítása az IP-címek a hálózati illesztő, olvassa el a hello toolearn [kezelése IP-címek](virtual-network-network-interface-addresses.md) cikk.

**Parancsok**

|Eszköz|Parancs|
|---|---|
|parancssori felület|[az hálózat hálózati adapter ip-konfiguráció frissítése](/cli/azure/network/nic/ip-config?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Set-AzureRmNetworkInterfaceIpConfig](/powershell/module/azurerm.network/set-azurermnetworkinterfaceipconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|


## <a name="delete-a-network-interface"></a>A hálózati illesztő törlése

Egy adott hálózati csatoló törölheti, amíg nincs virtuális géphez csatolt tooa. Ha csatlakoztatott tooa virtuális gép, kell-e virtuális gép hello leállítva (felszabadítva) állapota első hely hello, hello hálózati illesztőt a hello virtuális gépről, majd leválasztani a hello hálózati illesztő törlése előtt. toodetach a hálózati adaptert egy virtuális gépről, teljes hello szükséges lépések hello [leválasztani a hálózati adaptert egy virtuális gép](virtual-network-network-interface-vm.md#vm-remove-nic) hello szakasza [hozzáadása vagy eltávolítása a hálózati adapterek](virtual-network-network-interface-vm.md) cikk. Egy virtuális gép törlése leválasztja az összes hálózati illesztők csatolt tooit, de nem törli a hello hálózati adapterrel.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely hozzárendelt (minimum) engedélyeinek hello hálózat közreműködő szerepkört az előfizetés. Olvasási hello [Azure szerepköralapú hozzáférés-vezérlés beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) cikk toolearn további szerepköröket és engedélyeket tooaccounts rendelése.
2. Hello mezőben hello szöveget tartalmazó *keresési erőforrások* tetején hello hello Azure-portálon, írja be a *hálózati illesztőt*. Ha **hálózati illesztőt** jelenik meg a hello találatok, kattintson rá.
3. Kattintson a jobb gombbal hello hálózati illesztő toodelete ki, majd kattintson **törlése**.
4. Kattintson a **Igen** hello hálózati illesztő tooconfirm törlését.

Ha töröl egy adott hálózati csatoló bármely MAC vagy IP-címek hozzárendelve tooit kiadásakor.

**Parancsok**

|Eszköz|Parancs|
|---|---|
|parancssori felület|[az hálózati hálózati delete](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#delete)|
|PowerShell|[Remove-AzureRmNetworkInterface](/powershell/module/azurerm.network/remove-azurermnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="next-steps"></a>Következő lépések
a következő cikkek hello toocreate egy virtuális gép több hálózati adapter vagy az IP-címek, olvassa el:

**Parancsok**

|Tevékenység|Eszköz|
|---|---|
|Több hálózati adapterrel rendelkező virtuális gép létrehozása|[Parancssori felület](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
|Hozzon létre egy hálózati adapter virtuális több IPv4-címekkel|[Parancssori felület](virtual-network-multiple-ip-addresses-cli.md), [PowerShell](virtual-network-multiple-ip-addresses-powershell.md)|
|Hozzon létre egy hálózati adapter virtuális magánhálózati IPv6-cím (mögött egy Azure Load Balancer)|[Parancssori felület](../load-balancer/load-balancer-ipv6-internet-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../load-balancer/load-balancer-ipv6-internet-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [Azure Resource Manager-sablon](../load-balancer/load-balancer-ipv6-internet-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
