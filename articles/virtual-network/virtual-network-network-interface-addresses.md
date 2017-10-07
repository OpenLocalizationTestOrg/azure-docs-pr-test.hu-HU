---
title: "egy Azure hálózati illesztő IP-címek aaaConfigure |} Microsoft Docs"
description: "Ismerje meg, hogyan tooadd, módosítsa, majd távolítsa el a privát és nyilvános IP-címek a hálózati illesztő."
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
ms.openlocfilehash: 1e5ea6c65d93be9b1fda5d807500a0823c94c89c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-change-or-remove-ip-addresses-for-an-azure-network-interface"></a>Módosítsa, vagy távolítsa el az Azure hálózati illesztő IP-címek

Ismerje meg, hogyan tooadd, módosítsa, majd távolítsa el a nyilvános és magánhálózati IP-címek a hálózati illesztő. Magán IP-címek hozzárendelve tooa hálózati illesztő a virtuális gép toocommunicate más erőforrásokat egy Azure virtuális hálózatra és csatlakoztatott hálózatok engedélyezése. Magánhálózati IP-címnek azt is lehetővé teszi, hogy a kimenő kommunikáció toohello internetkapcsolat, és előre nem látható IP-címet a. A [nyilvános IP-cím](virtual-network-public-ip-address.md) tooa hozzárendelt hálózati illesztő lehetővé teszi a bejövő kommunikáció tooa a virtuális gép hello Internet. hello cím is lehetővé teszi, hogy a kimenő kommunikációt hello virtuális gép toohello Internet egy előre jelezhető IP-címet használja. További információkért lásd: [ismertetése az Azure-ban kimenő kapcsolatok](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json). 

Ha toocreate van szüksége, módosítsa vagy törölje a hálózati adaptert, olvassa el a hello [kezelheti egy adott hálózati csatoló](virtual-network-network-interface.md) cikk. Ha tooadd hálózati adapterek tooor eltávolítása hálózati adapterek virtuális gépről, olvassa el a hello [hozzáadása vagy eltávolítása a hálózati adapterek](virtual-network-network-interface-vm.md) cikk. 


## <a name="before-you-begin"></a>Előkészületek

Teljes hello feladatok bármelyik befejezése előtt a következő szakasz ebben a cikkben ismertetett visszaállítási lépésekkel:

- Felülvizsgálati hello [Azure korlátozza](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) cikk toolearn kapcsolatos korlátozásokat a nyilvános és magánhálózati IP-címeket.
- Jelentkezzen be toohello Azure [portal](https://portal.azure.com), az Azure parancssori felület (CLI), vagy az Azure PowerShell használata az Azure-fiók. Ha még nem rendelkezik Azure-fiókja, regisztráljon egy [ingyenes próbafiók](https://azure.microsoft.com/free).
- Ha a PowerShell használatával parancsok toocomplete feladatok ebben a cikkben [Azure PowerShell telepítése és konfigurálása](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Ellenőrizze, hogy hello hello Azure PowerShell parancsmagjait telepített legújabb verziója. Írja be a PowerShell-parancsok, példákkal tooget súgóját `get-help <command> -full`.
- Ha az Azure parancssori felület (CLI) használatával parancsok toocomplete feladatok ebben a cikkben [telepítése és konfigurálása az Azure parancssori felület hello](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Ellenőrizze, hogy a legújabb verziójának hello hello Azure parancssori felület telepítve. Írja be a parancssori felület parancsait tooget súgóját `az <command> --help`. Ahelyett, hogy a parancssori felület telepítése hello és a szükséges előfeltételek hello Azure Cloud rendszerhéj is használhatja. hello Azure Cloud rendszerhéj a szabad rendszerhéjakba futtatható közvetlenül hello Azure-portálon belül. Hello Azure CLI előtelepített és konfigurált toouse-fiókjához van. toouse hello felhő rendszerhéj, kattintson a felhő rendszerhéj hello **> _** hello hello tetején gomb [portal](https://portal.azure.com).

## <a name="add-ip-addresses"></a>IP-címek hozzáadása

Hozzáadhat annyi [titkos](#private) és [nyilvános](#public) [IPv4](#ipv4) címek szükséges tooa hálózati adapterként, hello határokon belül megjelennek a hello [Azure korlátozását ](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) cikk. Nem használhat hello portál tooadd IPv6 cím tooan meglévő hálózati illesztő (bár a hello portál tooadd egy titkos IPv6 cím tooa hálózati adapter használható hello hálózati kapcsolat létrehozásakor). Használhatja a PowerShell vagy parancssori felület tooadd titkos IPv6 cím tooone hello [másodlagos IP-konfiguráció](#secondary) (feltéve, nincsenek nincs meglévő másodlagos IP-konfigurációk) meglévő hálózat nem csatlakoztatott virtuális tooa gép. Minden eszköz tooadd egy nyilvános IPv6 cím tooa hálózati adapter nem használható. Lásd: [IPv6](#ipv6) IPv6-címek használatával. 

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely hozzárendelt (minimum) engedélyeinek hello hálózat közreműködő szerepkört az előfizetés. Olvasási hello [Azure szerepköralapú hozzáférés-vezérlés beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) cikk toolearn további szerepköröket és engedélyeket tooaccounts rendelése.
2. Hello mezőben hello szöveget tartalmazó *keresési erőforrások* tetején hello hello Azure-portálon, írja be a *hálózati illesztőt*. Ha **hálózati illesztőt** jelenik meg a hello találatok, kattintson rá.
3. A hello **hálózati illesztőt** panel, amelyen megjelenik, kattintson a hello hálózati kapcsolat tooadd egy IPv4-címe.
4. Kattintson a **IP-konfigurációk** a hello **beállítások** hello panelen kiválasztott hello hálózati illesztő szakasza.
5. Kattintson a **+ Hozzáadás** hello panelen, amely megnyitja az IP-konfigurációhoz.
6. Adja meg a következő hello beállításokat, majd kattintson a **OK** tooclose hello **hozzáadása IP-konfiguráció** panel:

    |Beállítás|Kötelező?|Részletek|
    |---|---|---|
    |Név|Igen|Hello hálózati illesztő egyedinek kell lennie|
    |Típus|Igen|Mivel adja hozzá egy IP-konfiguráció tooan meglévő hálózati kapcsolat, és mindegyik hálózati interfész rendelkeznie kell egy [elsődleges](#primary) IP-konfiguráció egyetlen választása marad: **másodlagos**.|
    |Privát IP-cím hozzárendelési módszert|Igen|[**Dinamikus** ](#dynamic) címet használva módosítható, ha hello virtuális gép újraindítása után a hello lett leállítva (felszabadított) állapotát. Azure rendel egy címet a hello címterület hello alhálózati hello hálózati adapter csatlakozik. [**Statikus** ](#static) címek nem kiadott, amíg nem hello hálózati illesztőt. Adja meg a hello terület címtartománya, amely jelenleg nem használja egy másik IP-konfigurációja IP-címeit.|
    |Nyilvános IP-cím|Nem|**Letiltva:** nincs nyilvános IP-cím erőforrás jelenleg társított toohello IP-konfigurációt. **Engedélyezve:** válasszon ki egy meglévő IPv4 nyilvános IP-címet, vagy hozzon létre egy újat. Hogyan toocreate egy nyilvános IP-címet, olvassa el toolearn hello [nyilvános IP-címek](virtual-network-public-ip-address.md#create-a-public-ip-address) cikk.|
7. Manuálisan adja hozzá a másodlagos privát IP-címek toohello virtuális gép operációs rendszere hello hello utasításait elvégzésével [több IP-cím hozzárendelése toovirtual gép operációs rendszerek](virtual-network-multiple-ip-addresses-portal.md#os-config) cikk. Lásd: [titkos](#private) IP-címek IP-címek tooa virtuális gép operációs rendszere manuális hozzáadása előtt különleges szempontjait. Bármely nyilvános IP-címek toohello virtuális gép operációs rendszere nem adja hozzá.

**Parancsok**

|Eszköz|Parancs|
|---|---|
|parancssori felület|[az hálózati hálózati adapter ip-konfiguráció létrehozása](/cli/azure/network/nic/ip-config?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[Adja hozzá AzureRmNetworkInterfaceIpConfig](/powershell/module/azurerm.network/add-azurermnetworkinterfaceipconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="change-ip-address-settings"></a>IP-cím beállításainak módosítása

Toochange hello hozzárendelési módszer egy IPv4-címet, a módosítás hello statikus IPv4-cím, szükség lehet, illetve módosítása hello nyilvános IP-cím hozzárendelése tooa hálózati illesztőt. Ha épp módosított hello privát IPv4-cím, egy másodlagos IP-konfiguráció, a másodlagos hálózati adaptert egy virtuális gép társított (További információ [elsődleges és másodlagos hálózati adapterek](virtual-network-network-interface-vm.md#about)), virtuális hely hello hello a gép leállított (felszabadított) állapotához hello lépések végrehajtása előtt: 

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely hozzárendelt (minimum) engedélyeinek hello hálózat közreműködő szerepkört az előfizetés. Olvasási hello [Azure szerepköralapú hozzáférés-vezérlés beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) cikk toolearn további szerepköröket és engedélyeket tooaccounts rendelése.
2. Hello mezőben hello szöveget tartalmazó *keresési erőforrások* tetején hello hello Azure-portálon, írja be a *hálózati illesztőt*. Ha **hálózati illesztőt** jelenik meg a hello találatok, kattintson rá.
3. A hello **hálózati illesztőt** panel, amelyen megjelenik, kattintson a hello hálózati illesztő szeretné, hogy tooview vagy IP-cím beállításainak módosítása.
4. Kattintson a **IP-konfigurációk** a hello **beállítások** hello panelen kiválasztott hello hálózati illesztő szakasza.
5. Kattintson a kívánt hello panel, amely megnyitja az IP-konfiguráció hello listájából toomodify hello IP-konfigurációja.
6. Hello beállításait, a kívánt módon működjenek, hello információk segítségével hello beállításaival kapcsolatos hello 6. lépésben módosítsa [adja hozzá egy IP-konfiguráció](#create-ip-config) című szakaszát. Kattintson a **mentése** tooclose hello panel hello IP-konfiguráció módosította.

>[!NOTE]
>Ha hello elsődleges hálózati adapter több IP-konfigurációk és magánhálózati IP-címe hello hello elsődleges IP-konfigurációja megváltoztatja, akkor manuálisan kell újra hozzárendelnie hello elsődleges és másodlagos IP-címek toohello hálózati kapcsolat a Windows (nem szükséges Linux). toomanually rendelje hozzá az IP-címek tooa hálózati kapcsolat az operációs rendszerben, olvassa el a hello [több IP-cím hozzárendelése toovirtual gépek](virtual-network-multiple-ip-addresses-portal.md#os-config) cikk. Lásd: [titkos](#private) IP-címek IP-címek tooa virtuális gép operációs rendszere manuális hozzáadása előtt különleges szempontjait. Bármely nyilvános IP-címek toohello virtuális gép operációs rendszere nem adja hozzá.

**Parancsok**

|Eszköz|Parancs|
|---|---|
|parancssori felület|[az hálózat hálózati adapter ip-konfiguráció frissítése](/cli/azure/network/nic/ip-config?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Set-AzureRMNetworkInterfaceIpConfig](/powershell/module/azurerm.network/set-azurermnetworkinterfaceipconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="remove-ip-addresses"></a>Távolítsa el az IP-címek

Eltávolíthatja [titkos](#private) és [nyilvános](#public) hálózati illesztő IP-címet, de egy adott hálózati csatoló mindig rendelkeznie kell legalább egy privát IPv4 cím tooit.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely hozzárendelt (minimum) engedélyeinek hello hálózat közreműködő szerepkört az előfizetés. Olvasási hello [Azure szerepköralapú hozzáférés-vezérlés beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) cikk toolearn további szerepköröket és engedélyeket tooaccounts rendelése.
2. Hello mezőben hello szöveget tartalmazó *keresési erőforrások* tetején hello hello Azure-portálon, írja be a *hálózati illesztőt*. Ha **hálózati illesztőt** jelenik meg a hello találatok, kattintson rá.
3. A hello **hálózati illesztőt** panel, amelyen megjelenik, kattintson a hello hálózati kapcsolat tooremove IP-címek.
4. Kattintson a **IP-konfigurációk** a hello **beállítások** hello panelen kiválasztott hello hálózati illesztő szakasza.
5. Kattintson a jobb gombbal egy [másodlagos](#secondary) IP-konfiguráció (hello nem törölhető [elsődleges](#primary) konfigurációs) toodelete szeretné, kattintson a **törlése**, majd kattintson a **Igen**  tooconfirm hello törlését. Ha hello-konfigurációban szerepelt egy nyilvános IP-cím erőforrás társított tooit, hello erőforrás van elválasztja hello IP-konfiguráció, de hello erőforrás nem törlődik.
6. Bezárás hello **IP-konfigurációk** panelen.

**Parancsok**

|Eszköz|Parancs|
|---|---|
|parancssori felület|[az hálózati hálózati adapter ip-konfiguráció törlése](/cli/azure/network/nic/ip-config?toc=%2fazure%2fvirtual-network%2ftoc.json#delete)|
|PowerShell|[Remove-AzureRmNetworkInterfaceIpConfig](/powershell/module/azurerm.network/remove-azurermnetworkinterfaceipconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="ip-configurations"></a>IP-konfigurációk

[Személyes](#private) és (opcionálisan) [nyilvános](#public) IP-címek hozzárendelésének tooone vagy több IP-konfiguráció hozzárendelése tooa hálózati illesztőt. Az IP-konfigurációjának két típusa van:

### <a name="primary"></a>Elsődleges

Mindegyik hálózati interfész hozzá van rendelve egy elsődleges IP-konfigurációval. Egy elsődleges IP-konfiguráció:

- Rendelkezik egy [titkos](#private) [IPv4](#ipv4) cím tooit. Nem rendelhet hozzá egy olyan magánhálózat [IPv6](#ipv6) cím tooa elsődleges IP-konfigurációja.
- Is egy [nyilvános](#public) IPv4-cím tooit. Nem rendelhető hozzá egy nyilvános IPv6 tooa elsődleges vagy másodlagos IP-címkonfigurációt. De a saját, rendelje hozzá egy nyilvános IPv6 cím tooan Azure terheléselosztó, amely be tudják tölteni egyenleg forgalom tooa virtuális gép saját IPv6-cím. További információkért lásd: [részleteit és az IPv6 korlátozások](../load-balancer/load-balancer-ipv6-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#details-and-limitations).

### <a name="secondary"></a>Másodlagos

Továbbá tooa elsődleges IP-konfiguráció, egy adott hálózati csatoló lehet nulla vagy több másodlagos IP-konfigurációk tooit rendelve. Egy másodlagos IP-konfiguráció:

- A privát IPv4 vagy IPv6-cím tooit kell rendelkeznie. Ha hello cím IPv6-alapú, hello hálózati illesztő csak van egy másodlagos IP-konfigurációval. Ha hello cím IPv4, hello hálózati adapter rendelkezhet hozzárendelt tooit több másodlagos IP-konfigurációk. toolearn hány privát és nyilvános IPv4-címek rendelhetők tooa hálózati kapcsolat kapcsolatos további információkért lásd: hello [Azure korlátozza](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) cikk.  
- Előfordulhat, hogy is egy nyilvános IPv4-cím tooit, ha hello magánhálózati IP-cím IPv4-alapú. Ha IPv6-alapú hello magánhálózati IP-cím, egy nyilvános IPv4- vagy IPv6 cím toohello IP-konfiguráció nem rendelhető hozzá. Hozzárendelése több IP-címek tooa hálózati kapcsolat a következő esetekben hasznos, mint:
    - Több webhely vagy szolgáltatás üzemeltetése különböző IP-címekkel és SSL-tanúsítványokkal egyetlen kiszolgálón.
    - Egy virtuális gépet, a hálózati virtuális készülék, például egy tűzfal vagy terheléselosztó szolgál.
    - hello képességét tooadd bármelyik hello privát IPv4-címek bármely hello hálózati illesztők tooan Azure Load Balancer háttér-készlet. Az elmúlt hello csak hello elsődleges IPv4-cím hello elsődleges hálózati illesztő sikerült hozzáadni tooa háttér-készlet. toolearn hogyan tooload elosztása több IPv4-alapú konfiguráció kapcsolatos további információkért lásd: hello [terheléselosztás több IP-konfigurációk](../load-balancer/load-balancer-multiple-ip.md?toc=%2fazure%2fvirtual-network%2ftoc.json) cikk. 
    - hello képességét tooload egyenleg egy IPv6 cím hozzárendelt tooa hálózati adapter. toolearn hogyan tooload egyenleg tooa saját IPv6-címet, kapcsolatos további információkért lásd: hello [IPv6-címek terheléselosztásához](../load-balancer/load-balancer-ipv6-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) cikk.


## <a name="address-types"></a>Cím típusa

A következő IP-címek tooan típusú hello rendelhet [IP-konfiguráció](#ip-configurations):

### <a name="private"></a>Saját

Személyes [IPv4](#ipv4) címek engedélyezése egy virtuális gép toocommunicate virtuális hálózat vagy más hálózatokhoz csatlakozó más erőforrásokat. Egy virtuális gépet nem lehetett továbbítani a bejövő, és nem hello virtuális gépek kommunikálhatnak egy olyan magánhálózat a kimenő [IPv6](#ipv6) cím, egy kivétellel. A virtuális gépek kommunikálhatnak hello Azure terheléselosztó IPv6-cím használatával. További információkért lásd: [részleteit és az IPv6 korlátozások](../load-balancer/load-balancer-ipv6-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#details-and-limitations). 

Alapértelmezés szerint hello Azure DHCP-kiszolgálók hozzárendelése hello privát IPv4-cím hello [elsődleges IP-konfiguráció](#primary) hello hálózati illesztő toohello hálózati adapter hello virtuális gép operációs rendszerében. Ha szükséges, soha nem manuálisan állítsa be hello hello virtuális gép operációs rendszerében a hálózati illesztő IP-címét. 

> [!WARNING]
> Ha hello IPv4-címet beállítani a hálózati adaptert egy virtuális gép operációs rendszerében hello elsődleges IP-címe eltér legalább egyszer hello privát IPv4-cím hozzárendelése toohello elsődleges IP-konfiguráció hello elsődleges hálózati adapter csatlakoztatva tooa virtuális gép Azure-ban, elvesznek a kapcsolat toohello virtuális gépet.

Nincsenek forgatókönyvekben, ahol azt szükséges toomanually hello IP-cím beállítása a hálózati adapter hello virtuális gép operációs rendszerében. Például meg kell adni manuálisan hello elsődleges és másodlagos IP-címeket a Windows operációs rendszer több IP-címek tooan Azure virtuális géphez való hozzáadásakor. A Linux virtuális gép is elegendő lehet toomanually set hello másodlagos IP-címeket. Lásd: [hozzáadása IP-címek tooa virtuális gép operációs rendszer](virtual-network-multiple-ip-addresses-portal.md#os-config) részleteiről. Ha manuálisan hello IP-cím hello operációs rendszerben, ajánlott mindig használjon hello címek toohello IP-konfigurációja egy adott hálózati csatoló hello statikus (helyett dinamikus)-hozzárendelési módszert használja. Rendelje hozzá hello statikus metódussal hello címet biztosítja, hogy hello cím nem változtatja meg az Azure. Ha valaha is kell toochange hello cím tooan IP-konfiguráció, javasoljuk, hogy:

1. tooensure hello virtuális gép egy címet fogad hello Azure DHCP-kiszolgálók, hello IP cím hátsó tooDHCP belül hello operációsrendszer- és újraindítási hello virtuális gép hello hozzárendelésének módosítása.
2. Állítsa le (felszabadítása) hello virtuális gépet.
3. Hello IP-konfiguráció Azure-ban hello IP-címének módosítása.
4. Hello virtuális gép elindításához.
5. [Manuálisan konfigurálnia a](virtual-network-multiple-ip-addresses-portal.md#os-config) hello másodlagos IP-címek hello operációs rendszer (és a Windows hello elsődleges IP-cím) belül toomatch be Azure-ban.
 
Hello előző lépéseket követve hello privát IP-cím hozzárendelése toohello hálózati összeköttetés Azure és a virtuális gép operációs rendszerének belül, továbbra is hello azonos. tookeep nyomon követése, amelyek az előfizetés, amely a manuálisan beállított IP-címeit, az operációs rendszer virtuális gépeit vegyen fel egy Azure [címke](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#tags) toohello virtuális gépek. Használhatja a "IP-cím hozzárendelése: statikus", például. Ezzel a módszerrel könnyedén megtalálhatja a hello virtuális gépeken belül az előfizetés, amely a manuálisan beállított hello IP-címet hello operációs rendszerben.

Egy virtuális gép toocommunicate egyéb erőforrásokkal belül azonos, vagy csatlakoztatott virtuális hálózatok, egy magánhálózati IP-címe is cím hello tooenabling továbbá lehetővé teszi, hogy a virtuális gép toocommunicate kimenő toohello Internet. Kifelé irányuló kapcsolatok olyan Azure tooan előre nem látható nyilvános IP-cím szerinti fordítással hálózati forráscím. További információk az Azure kimenő internetkapcsolattal, olvassa el a hello toolearn [Azure kimenő internetkapcsolat](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json) cikk. Bejövő tooa virtuális gépek magánhálózati IP-címet a hello Internet nem tud kommunikálni.

### <a name="public"></a>Nyilvános

Nyilvános IP-címek engedélyezése a bejövő kapcsolatot tooa a virtuális gép hello Internet. Kimenő kapcsolatok toohello Internet egy előre jelezhető IP-címet használja. Lásd: [ismertetése az Azure-ban kimenő kapcsolatok](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json) részleteiről. Előfordulhat, hogy rendelje hozzá egy nyilvános IP-címének tooan IP konfigurációja, de nem szükséges. Ha nem rendel hozzá egy nyilvános IP cím tooa virtuális gépet, továbbra is képes kommunikálni a kimenő toohello Internet privát IP-címére. További információ a nyilvános IP-címek, olvassa el a hello toolearn [nyilvános IP-cím](virtual-network-public-ip-address.md) cikk.

Számos korlátok toohello titkos és nyilvános IP-címeket az hozzárendelheti tooa hálózati adapter. További információkért olvassa el a hello [Azure korlátozza](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) cikk.

> [!NOTE]
> Azure fordítja le a virtuális gép magánhálózati IP cím tooa nyilvános IP-cím. Így nincs szükség tooever nincs manuálisan adjon meg egy nyilvános IP-cím hello operációs rendszerben, emiatt hello operációs rendszer nem észleli a nyilvános IP-címek hozzárendelve tooit.

## <a name="assignment-methods"></a>Hozzárendelési módszert

Nyilvános és magánhálózati IP-címek hozzá a következő hozzárendelési módszert hello használata:

### <a name="dynamic"></a>Dinamikus

Dinamikus privát IPv4 és IPv6-alapú (opcionális) címek alapértelmezés szerint vannak hozzárendelve. Dinamikus címet használva módosítható, ha hello helyezni, hello virtuális gép leállított (felszabadított) állapotához, majd elindítani. Ha IPv4-címek toochange hello során hello virtuális gép nem szeretné, rendelje hozzá a hello címek hello statikus metódus használatával. Csak egy titkos IPv6-címet, dinamikus hozzárendelése metódussal hello rendelhet hozzá. Nem rendelhető hozzá egy nyilvános IPv6 tooan IP-címkonfigurációt módszerek használatával.

### <a name="static"></a>Statikus

Hello statikus metódus használata hozzárendelt címek ne változtassa meg addig, amíg a virtuális gép törlődik. Manuálisan hozzá nem rendeli egy statikus privát IPv4 cím tooan IP-konfigurációt a hello címterület a hello alhálózati hello hálózati adaptert. (Opcionális) rendelhet hozzá a saját vagy nyilvános statikus IPv4 cím tooan IP-konfigurációt. Nem rendelhető hozzá egy statikus nyilvános vagy privát IPv6 tooan IP-címkonfigurációt. toolearn hogyan Azure rendel statikus nyilvános IPv4-címet, kapcsolatos további információkért lásd: hello [nyilvános IP-cím](virtual-network-public-ip-address.md) cikk.

## <a name="ip-address-versions"></a>IP-cím verziók

Cím hozzárendelésekor a következő verziók hello adhatja meg:

### <a name="ipv4"></a>IPv4-alapú

Mindegyik hálózati interfész rendelkeznie kell egy [elsődleges](#primary) egy hozzárendelt IP-beállítását [titkos](#private) [IPv4](#ipv4) cím. Hozzáadhat egy vagy több [másodlagos](#secondary) IP-konfiguráció magánhálózati IPv4- és (opcionálisan) IPv4 tartalmazó [nyilvános](#public) IP-címet.

### <a name="ipv6"></a>IPv6

Nulla vagy egy személyes rendelhet [IPv6](#ipv6) cím tooone másodlagos IP-konfigurációja, a hálózati adaptert. hello hálózati adapter nem rendelkezhet minden meglévő másodlagos IP-konfigurációt. Nem adható hozzá egy IP-konfiguráció hello portál használata IPv6-címmel. Használja a Powershellt vagy hello CLI tooadd a személyes IPv6 cím tooan meglévő hálózati illesztő IP-konfigurációt. hello hálózati illesztő nem lehet a meglévő virtuális gép csatlakoztatott tooan.

> [!NOTE]
> Bár létrehozhat egy adott hálózati csatoló hello portál használata IPv6-címmel, nem adhat hozzá egy meglévő hálózati illesztő tooa új vagy meglévő virtuális gépet, hello portál használatával. PowerShell vagy Azure CLI 2.0 toocreate egy adott hálózati csatoló hello használata egy saját IPv6-címet, majd hello hálózati adapter csatlakoztatása egy virtuális gép létrehozásakor. Hozzárendelt tooit tooan meglévő virtuális gép saját IPv6-címmel rendelkező hálózati illesztő nem lehet csatolni. Nem adható hozzá a titkos IPv6 cím tooan IP-konfiguráció bármely hálózati kapcsolatát tooa virtuális géphez olyan eszközöket (portál, CLI vagy PowerShell) segítségével.

Nem rendelhető hozzá egy nyilvános IPv6 tooa elsődleges vagy másodlagos IP-címkonfigurációt.

## <a name="next-steps"></a>Következő lépések
egy virtuális gépet a különböző IP-konfigurációk, olvassa el a következő cikkek hello toocreate:

|Tevékenység|Eszköz|
|---|---|
|Több hálózati adapterrel rendelkező virtuális gép létrehozása|[Parancssori felület](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
|Hozzon létre egy hálózati adapter virtuális több IPv4-címekkel|[Parancssori felület](virtual-network-multiple-ip-addresses-cli.md), [PowerShell](virtual-network-multiple-ip-addresses-powershell.md)|
|Hozzon létre egy hálózati adapter virtuális magánhálózati IPv6-cím (mögött egy Azure Load Balancer)|[Parancssori felület](../load-balancer/load-balancer-ipv6-internet-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../load-balancer/load-balancer-ipv6-internet-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [Azure Resource Manager-sablon](../load-balancer/load-balancer-ipv6-internet-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
