---
title: "aaaCreate, módosítsa vagy törölje az Azure nyilvános IP-cím |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate, módosítsa vagy törölje a nyilvános IP-cím."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: bb71abaf-b2d9-4147-b607-38067a10caf6
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/24/2017
ms.author: jdial
ms.openlocfilehash: 0f2578e8661c8f33419b896debcfa0ba1b41fc5c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-change-or-delete-a-public-ip-address"></a>Létrehozása, módosítása vagy a nyilvános IP-cím törlése

Tudnivalók a nyilvános IP-cím, cím, és hogyan toocreate, módosítása és törlése egy. A nyilvános IP-cím és a saját konfigurálható beállítások erőforrás. Lehetővé teszi egy nyilvános IP-cím tooother Azure-erőforrások hozzárendelése:
- Bejövő Internet kapcsolat tooresources például Azure virtuális gépek (VM), Azure virtuálisgép-méretezési csoportok, Azure VPN Gateway, alkalmazásátjárót és az Internet felé néző Azure Load Balancer Terheléselosztók. Azure-erőforrások nem tud fogadni bejövő kommunikáció hello Internet nélkül hozzárendelt nyilvános IP-címet. Míg bizonyos Azure-erőforrások eredendően nyilvános IP-címek keresztül érhető el, további erőforrások rendelkeznie kell nyilvános IP-címtartományból toothem toobe hello Internet elérhető.
- Kimenő kapcsolódás toohello Internet egy előre jelezhető IP-címet használja. Például a virtuális gépek kommunikálhatnak kimenő toohello Internet egy nyilvános IP-cím tooit nélkül, de a cím az Azure tooan előre nem látható nyilvános cím fordította hálózati cím. A nyilvános IP-cím tooa erőforrás hozzárendelése lehetővé teszi tooknow hello kimenő kapcsolathoz használt melyik IP-címet. Bár a előre jelezhető, hello cím változhat, attól függően, hogy a kiválasztott hello hozzárendelési módszert. További információkért lásd: [hozzon létre egy nyilvános IP-cím](#create-a-public-ip-address). További információk az Azure-erőforrások, olvassa el a hello kimenő kapcsolatok toolearn [kimenő kapcsolatok megértéséhez](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json) cikk.

## <a name="before-you-begin"></a>Előkészületek

Teljes hello feladatok bármelyik befejezése előtt a következő szakasz ebben a cikkben ismertetett visszaállítási lépésekkel:

- Felülvizsgálati hello [Azure korlátozza](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) cikk toolearn kapcsolatos korlátozásokat a nyilvános IP-címeket.
- Jelentkezzen be toohello Azure [portal](https://portal.azure.com), az Azure parancssori felület (CLI), vagy az Azure PowerShell használata az Azure-fiók. Ha még nem rendelkezik Azure-fiókja, regisztráljon egy [ingyenes próbafiók](https://azure.microsoft.com/free).
- Ha a PowerShell használatával parancsok toocomplete feladatok ebben a cikkben [Azure PowerShell telepítése és konfigurálása](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Ellenőrizze, hogy hello hello Azure PowerShell parancsmagjait telepített legújabb verziója. Írja be a PowerShell-parancsok, példákkal tooget súgóját `get-help <command> -full`.
- Ha az Azure parancssori felület (CLI) használatával parancsok toocomplete feladatok ebben a cikkben [telepítése és konfigurálása az Azure parancssori felület hello](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Ellenőrizze, hogy a legújabb verziójának hello hello Azure parancssori felület telepítve. Írja be a parancssori felület parancsait tooget súgóját `az <command> --help`. Ahelyett, hogy a parancssori felület telepítése hello és a szükséges előfeltételek hello Azure Cloud rendszerhéj is használhatja. hello Azure Cloud rendszerhéj a szabad rendszerhéjakba futtatható közvetlenül hello Azure-portálon belül. Hello Azure CLI előtelepített és konfigurált toouse-fiókjához van. toouse hello felhő rendszerhéj, kattintson a felhő rendszerhéj hello **> _** hello hello tetején gomb [portal](https://portal.azure.com).

Nyilvános IP-címek rendelkezik egy névleges kell fizetni. az árképzés hello tooview, olvassa el a hello [IP-cím árképzési](https://azure.microsoft.com/pricing/details/ip-addresses) lap. 

## <a name="create-a-public-ip-address"></a>Hozzon létre egy nyilvános IP-címet

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely hozzárendelt (minimum) engedélyeinek hello hálózat közreműködő szerepkört az előfizetés. Olvasási hello [Azure szerepköralapú hozzáférés-vezérlés beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) cikk toolearn további szerepköröket és engedélyeket tooaccounts rendelése.
2. Hello mezőben hello szöveget tartalmazó *keresési erőforrások* tetején hello hello Azure-portálon, írja be a *nyilvános IP-cím*. Ha **nyilvános IP-címek** jelenik meg a hello találatok, kattintson rá.
3. Kattintson a **+ Hozzáadás** a hello **nyilvános IP-cím** panel, amely akkor jelenik meg.
4. Adja meg vagy válassza ki a következő beállítások hello hello értékeit **nyilvános IP-cím létrehozása** panel, amely akkor jelenik meg, majd kattintson **létrehozása**:

    |Beállítás|Kötelező?|Részletek|
    |---|---|---|
    |Név|Igen|hello nevének választja hello erőforráscsoporton belül egyedinek kell lennie.|
    |IP-verziója|Igen| Válassza ki az IPv4- vagy IPv6. Nyilvános IPv4-címek lehetnek hozzárendelve tooseveral Azure-erőforrások, míg IPv6 nyilvános IP-cím csak rendelhetők hozzá tooan internetre terheléselosztóhoz. hello terheléselosztót is IPv6-alapú forgalom tooAzure virtuális gépek terhelést elosztani. További információ [terheléselosztási IPv6-forgalom toovirtual gépek](../load-balancer/load-balancer-ipv6-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
    |IP-cím hozzárendelése|Igen|**Dinamikus:** dinamikus címek hozzárendelésének csak után hello nyilvános IP-cím tartozik tooa NIC csatolt tooa VM és hello VM indítja el hello első alkalommal. Dinamikus címek is módosítható, ha hello VM hello NIC csatolt toois leállítva (felszabadítva). hello cím marad hello azonos Ha hello virtuális gép újraindítása vagy leállítása (de nem felszabadítása. lehetséges). **Statikus:** statikus címek hozzárendelésének hello nyilvános IP-cím létrehozásakor. Statikus címeket nem még akkor is, kerül, ha hello VM hello leállítva (felszabadítva) állapotváltoztatási tegye. hello cím csak kiadott, hálózati hello törlésekor. Hello hozzárendelési módszert hello hálózati adapter létrehozása után módosíthatja. Ha IPv6-alapú hello választ **verziójú IP**, hello csak elérhető hozzárendelési módszer **dinamikus**.|
    |Üresjárati időkorlátja (perc)|Nem|Hány percig tookeep TCP- vagy HTTP-kapcsolatot nyissa meg a függő ügyfelek toosend életben tartási üzenetek nélkül. Ha az IPv6 **verziójú IP**, ez az érték nem módosítható. |
    |DNS-névcímke|Nem|Hello (között az összes előfizetést és az összes ügyfél számára) hozzon létre hello neve az Azure-beli hely belül egyedinek kell lennie. hello Azure nyilvános DNS-szolgáltatás automatikusan regisztrálja hello nevét és IP-cím így tooa erőforrás hello nevű is elérheti. Azure hozzáfűzi *location.cloudapp.azure.com* (amennyiben helye hello választja) toohello-nevet toocreate hello teljesen minősített DNS-neve. Ha úgy dönt, toocreate mindkét cím verziók, hello azonos DNS-név hozzá van rendelve tooboth hello IPv4 és IPv6-címeket. hello Azure DNS-szolgáltatás mind az IPv4 és IPv6 AAAA neve rekordok tartalmazza, és mindkét rekordok válaszol, amikor hello DNS-név keresése. hello ügyfél úgy dönt, hogy mely cím (IPv4 vagy IPv6) toocommunicate rendelkező.|
    |Egy IPv6-(vagy IPv4-) cím létrehozása|Nem| Válassza a függő IPv6 és IPv4-alapú megjelenítését **verziójú IP**. Ha például **IPv4** a **verziójú IP**, **IPv6** itt jelenik meg.
    |Name (csak akkor látható, ha bejelölte a hello **hozzon létre egy IPv6-(vagy IPv4-alapú) címet** jelölőnégyzet)|Igen, ha hello **hozzon létre egy IPv6-alapú** (vagy IPv4-alapú) jelölőnégyzetet.|hello neve nem lehet azonos hello nevet hello először **neve** ezen a listán. Ha toocreate IPv4- és IPv6-cím, a hello portál két külön nyilvános IP-cím erőforrás, egyet mindegyik tooit hozzárendelt IP-cím verziót hoz létre.|
    |IP-cím hozzárendelése (csak akkor látható, ha bejelölte a hello **hozzon létre egy IPv6-(vagy IPv4-alapú) címet** jelölőnégyzet)|Igen, ha hello **hozzon létre egy IPv6-alapú** (vagy IPv4-alapú) jelölőnégyzetet.|Ha hello jelölőnégyzet felirat **IPv4-cím létrehozása**, egy hozzárendelési módszert. Ha hello jelölőnégyzet felirat **IPv6-cím létrehozása**, nem egy hozzárendelési módszert választja, azt kell **dinamikus**.|
    |Előfizetés|Igen|Már léteznie kell hello azonos [előfizetés](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) hello erőforrásként szeretne tooassociate hello nyilvános IP-címet.|
    |Erőforráscsoport|Igen|Azonos vagy eltérő, hello létezhet [erőforráscsoport](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group) hello erőforrásként szeretne tooassociate hello nyilvános IP-címet.|
    |Hely|Igen|Már léteznie kell hello azonos [hely](https://azure.microsoft.com/regions), tooas régió, más néven hello erőforráson tooassociate hello nyilvános IP-címet.|

**Parancsok**

Bár hello portal biztosít hello beállítás toocreate két nyilvános IP-cím erőforrás (egy IPv4- és egy IPv6), a következő parancssori felület és a PowerShell-parancsok hello egy erőforrás létrehozása egy IP-verziót vagy hello címmel más. Ha azt szeretné, hogy két nyilvános IP-cím erőforrás, egy az egyes IP-verziót, futtatnia kell hello parancs kétszer, adja meg a különböző neveket és hello nyilvános IP-cím erőforrás-verziók. 

|Eszköz|Parancs|
|---|---|
|parancssori felület|[az hálózati nyilvános ip-létrehozása](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[Új AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress)|

## <a name="view-change-settings-for-or-delete-a-public-ip-address"></a>Megtekintése, módosítsa a beállításokat, vagy egy nyilvános IP-cím törlése

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely hozzárendelt (minimum) engedélyeinek hello hálózat közreműködő szerepkört az előfizetés. Olvasási hello [Azure szerepköralapú hozzáférés-vezérlés beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) cikk toolearn további szerepköröket és engedélyeket tooaccounts rendelése.
2. Hello mezőben hello szöveget tartalmazó *keresési erőforrások* tetején hello hello Azure-portálon, írja be a *nyilvános IP-cím*. Ha **nyilvános IP-címek** jelenik meg a hello találatok, kattintson rá.
3. A hello **nyilvános IP-címek** panel, amelyen megjelenik, kattintson a hello neve hello nyilvános IP-címet szeretné, hogy tooview, beállításainak módosítása vagy törlése.
4. Hello panelen megjelenő hello nyilvános IP-cím, az alábbi beállítások attól függően, hogy tooview, hello teljes egyik törlése, vagy hello nyilvános IP-címének módosítása.
    - **Nézet**: hello **áttekintése** hello panel részén látható hello nyilvános IP-cím-beállításait, például a hello hálózati adapter kapcsolódik túl (ha hello cím tooa társított felülettel). hello portál nem jelenik meg a hello verziójának (IPv4 vagy IPv6) hello cím. tooview hello fájlverzió-információkat, használjon hello PowerShell vagy a CLI parancs tooview hello nyilvános IP-cím. Ha hello IP-cím verziót IPv6, címmel hello nem hello portálon, a PowerShell vagy a hello CLI jelenik meg. 
    - **Törlése**: toodelete hello nyilvános IP-cím, kattintson a **törlése** a hello **áttekintése** hello panel szakasza. Ha hello cím jelenleg társított tooan IP-konfigurációja, nem lehet törölni. Ha hello cím jelenleg társítva van egy konfigurációhoz, kattintson a **szüntesse** toodissociate hello cím a hello IP-konfigurációt.
    - **Változás**: kattintson a **konfigurációs**. Módosítsa a beállításokat a hello 4 hello információk alapján [hozzon létre egy nyilvános IP-cím](#create-a-public-ip-address) című szakaszát. egy IPv4-címet a statikus toodynamic toochange hello hozzárendelését, kell először megszünteti hello nyilvános IPv4-cím a hello IP-konfiguráció azt hozzá van rendelve. Hello hozzárendelési módszer toodynamic módosítsa, majd kattintson a **társítása** tooassociate hello IP-cím toohello azonos IP-konfigurációs, egy másik konfigurációt, vagy meg is hagyhatja azt leválasztása. a nyilvános IP-címet, hello toodissociate **áttekintése** kattintson **szüntesse**.

>[!WARNING]
>Amikor hello hozzárendelés metódus statikus toodynamic vált, elvesznek a hello IP-címet, amely toohello nyilvános IP-cím lett rendelve. Hello Azure nyilvános DNS-kiszolgálók kezelése statikus vagy dinamikus címek és a DNS-Névcímke (Ha egy meghatározott) közötti leképezést, amíg egy dinamikus IP-címet módosíthatja, ha a hello VM egy után indítják el a hello leállására (felszabadított) állapot. tooprevent hello cím módosítsák, rendelje hozzá egy statikus IP-címet.

**Parancsok**

|Eszköz|Parancs|
|---|---|
|parancssori felület|[az nyilvános ip-azon hálózati](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#list) toolist nyilvános IP-címek, [az nyilvános ip-megjelenítése hálózati](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#show) tooshow beállítások; [az hálózati nyilvános ip-frissítés](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#update) tooupdate; [az hálózati nyilvános ip-törlése](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#delete) toodelete|
|PowerShell|[Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress?toc=%2fazure%2fvirtual-network%2ftoc.json) tooretrieve egy nyilvános IP-cím objektum cím és a beállítások megtekintéséhez [Set-AzureRmPublicIpAddress](/powershell/resourcemanager/azurerm.network/set-azurermpublicipaddress?toc=%2fazure%2fvirtual-network%2ftoc.json) tooupdate beállítások; [Remove-AzureRmPublicIpAddress](/powershell/module/azurerm.network/remove-azurermpublicipaddress) toodelete|

## <a name="next-steps"></a>Következő lépések
Nyilvános IP-címek hozzárendelése a hello Azure-erőforrások a következő létrehozásakor:

- [Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-network%2ftoc.json) vagy [Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) virtuális gépek
- [Az Internet felé néző Azure terheléselosztó](../load-balancer/load-balancer-get-started-internet-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Az Azure alkalmazás átjáró](../application-gateway/application-gateway-create-gateway-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Pont-pont kapcsolat az Azure VPN Gateway használatával](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Az Azure virtuálisgép-méretezési csoport](../virtual-machine-scale-sets/virtual-machine-scale-sets-portal-create.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
