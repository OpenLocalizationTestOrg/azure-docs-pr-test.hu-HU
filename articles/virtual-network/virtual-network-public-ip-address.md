---
title: "Létrehozása, módosítása vagy törlése az Azure nyilvános IP-cím |} Microsoft Docs"
description: "Megtudhatja, hogyan létrehozása, módosítása vagy a nyilvános IP-cím törlése."
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
ms.openlocfilehash: 9a51cd703266e330550a2a2f4e6a4d46722fec09
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="create-change-or-delete-a-public-ip-address"></a>Létrehozása, módosítása vagy a nyilvános IP-cím törlése

Információ a nyilvános IP-cím és létrehozása, módosítása és törlése egy. A nyilvános IP-cím és a saját konfigurálható beállítások erőforrás. Lehetővé teszi egy nyilvános IP-cím hozzárendelése más Azure-erőforrások:
- Erőforrások, például az Azure virtuális gépek (VM), Azure virtuálisgép-méretezési csoportok, Azure VPN Gateway, alkalmazásátjárót és az Internet felé néző Azure Load Balancer Terheléselosztók bejövő Internet kapcsolat. Azure-erőforrások tud fogadni bejövő kommunikáció nélkül hozzárendelt nyilvános IP-címet az internetről. Egyes Azure-erőforrások eredendően keresztül hozzáférhetők nyilvános IP-címeket, amíg más erőforrások rendelkeznie kell nyilvános IP-címeket az internetről hozzáférhető legyen rendelve.
- Kimenő kapcsolódás az internethez, egy előre jelezhető IP-címet használja. Például a virtuális gépek kommunikálhatnak kimenő internetkapcsolat nélkül egy nyilvános IP-címet kap, de a cím az Azure fordította egy előre nem látható a nyilvános cím hálózati cím. Egy erőforrást egy nyilvános IP-cím hozzárendelése lehetővé teszi, hogy tudja, melyik IP-címet a kimenő kapcsolathoz használt. Bár a előre jelezhető, a cím változhat, attól függően, hogy a választott hozzárendelési módszert. További információkért lásd: [hozzon létre egy nyilvános IP-cím](#create-a-public-ip-address). Az Azure-erőforrások kimenő kapcsolatok kapcsolatos további tudnivalókért olvassa el a [kimenő kapcsolatok megértéséhez](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json) cikk.

## <a name="before-you-begin"></a>Előkészületek

Esetlegesen szakasz ebben a cikkben szereplő lépésekkel befejezése előtt hajtsa végre a következő feladatokat:

- Tekintse át a [Azure korlátozza](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) cikkben tájékozódhat az korlátozhatja a nyilvános IP-címeket.
- Jelentkezzen be a Azure [portal](https://portal.azure.com), az Azure parancssori felület (CLI), vagy az Azure PowerShell használata az Azure-fiók. Ha még nem rendelkezik Azure-fiókja, regisztráljon egy [ingyenes próbafiók](https://azure.microsoft.com/free).
- Ha a feladat-ebben a cikkben a PowerShell-parancsokkal [Azure PowerShell telepítése és konfigurálása](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Ellenőrizze, hogy a legfrissebb telepítve az Azure PowerShell-parancsmagjaival. Ha segítséget szeretne kérni a PowerShell-parancsaihoz, valamint példákkal, írja be a `get-help <command> -full`.
- Ha ebben a cikkben a feladatokat az Azure parancssori felület (CLI) parancsokkal [telepítése és konfigurálása az Azure parancssori felület](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Ellenőrizze, hogy a telepített Azure CLI legújabb verziója. Segítség kérése parancssori felület parancsait, írja be a következőt `az <command> --help`. Ahelyett, hogy a parancssori felület és a szükséges előfeltételek telepítése, az Azure-felhő rendszerhéj is használhatja. Az Azure Cloud Shell olyan ingyenes Bash-felület, amelyet közvetlenül futtathat az Azure Portalon. A fiókjával való használat érdekében az Azure CLI már előre telepítve és konfigurálva van rajta. A felhő rendszerhéj használatához kattintson a felhő rendszerhéj **> _** gomb tetején a [portal](https://portal.azure.com).

Nyilvános IP-címek rendelkezik egy névleges kell fizetni. Az árképzés megtekintéséhez olvassa el a [IP-cím árképzési](https://azure.microsoft.com/pricing/details/ip-addresses) lap. 

## <a name="create-a-public-ip-address"></a>Hozzon létre egy nyilvános IP-címet

1. Jelentkezzen be a [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely a hálózat közreműködő szerepkört az előfizetés (minimum) hozzárendelt engedélyeit. Olvassa el a [Azure szerepköralapú hozzáférés-vezérlés beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) cikk tudhat meg többet a szerepköröket és engedélyeket hozzárendelése a fiókokhoz.
2. A mezőbe a szöveget tartalmazó *keresési erőforrások* az Azure portál felső részén írja be a *nyilvános IP-cím*. Ha **nyilvános IP-címek** jelenik meg a keresési eredmények között kattintson rá.
3. Kattintson a **+ Hozzáadás** a a **nyilvános IP-cím** panel, amely akkor jelenik meg.
4. Adja meg vagy válassza ki a következő beállítások értékei a **nyilvános IP-cím létrehozása** panel, amely akkor jelenik meg, majd kattintson **létrehozása**:

    |Beállítás|Kötelező megadni?|Részletek|
    |---|---|---|
    |Név|Igen|A nevét, válassza ki az erőforráscsoporton belül egyedinek kell lennie.|
    |IP-verziója|Igen| Válassza ki az IPv4- vagy IPv6. Nyilvános IPv4-címek hozzárendelhetők legyenek több Azure-erőforrások, míg IPv6 nyilvános IP-cím csak egy internetre irányuló terheléselosztót rendelhető. A load balancer is terheléselosztásához IPv6-forgalom Azure virtuális gépekhez. További információ [terheléselosztási IPv6-forgalom a virtuális gépek](../load-balancer/load-balancer-ipv6-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
    |IP-cím hozzárendelése|Igen|**Dinamikus:** dinamikus címek hozzárendelésének csak után a nyilvános IP-cím társítva egy hálózati Adaptert egy virtuális gép és a virtuális gép csatlakoztatva első alkalommal elindul. A dinamikus címek megváltozhatnak, ha a virtuális gép, amelyhez a hálózati adapter csatlakoztatva van, leáll (felszabadul). A cím változatlan marad, ha a virtuális gép újraindítása vagy leállítása (de nem felszabadítása. lehetséges) is. **Statikus:** statikus címek hozzárendelésének a nyilvános IP-cím létrehozásakor. A statikus címek akkor sem változnak, ha a virtuális gép leállított (felszabadított) állapotba kerül. A cím csak a hálózati adapter törlése esetén oldódik fel. A hozzárendelési módszer a hálózati adapter létrehozása után módosítható. Ha az IPv6 a **verziójú IP**, az csak a rendelkezésre álló hozzárendelési módszer **dinamikus**.|
    |Üresjárati időkorlátja (perc)|Nem|Hány perc megnyitva, a TCP- vagy HTTP-kapcsolat ügyfelek által küldött életben tartási üzenetek nélkül. Ha az IPv6 **verziójú IP**, ez az érték nem módosítható. |
    |DNS-névcímke|Nem|Az Azure-hoz létre a neve (között az összes előfizetést és az összes ügyfél számára) hely belül egyedinek kell lennie. Az Azure nyilvános DNS-szolgáltatás automatikusan regisztrálja nevét és IP-címe úgy kapcsolódhat a nevű erőforrás. Azure hozzáfűzi *location.cloudapp.azure.com* (amennyiben helye az választja) neve megadnia a teljesen minősített DNS-név létrehozásához. Ha mindkét cím verzió létrehozása mellett dönt, a DNS-névvel az IPv4 és IPv6-cím van hozzárendelve. Az Azure DNS szolgáltatást tartalmaz, mind az IPv4 és IPv6 AAAA neve rekordok, és mindkét rekordok válaszol, amikor a DNS-név keresése. Az ügyfél úgy dönt, hogy mely címet (IPv4 vagy IPv6) való kommunikációhoz.|
    |Egy IPv6-(vagy IPv4-) cím létrehozása|Nem| Válassza a függő IPv6 és IPv4-alapú megjelenítését **verziójú IP**. Ha például **IPv4** a **verziójú IP**, **IPv6** itt jelenik meg.
    |Name (csak akkor látható, ha bejelölte a **hozzon létre egy IPv6-(vagy IPv4-alapú) címet** jelölőnégyzet)|Igen, ha bejelöli a **hozzon létre egy IPv6-alapú** (vagy IPv4-alapú) jelölőnégyzetet.|A neve eltér a nevet kell lennie az első **neve** ezen a listán. Ha IPv4- és IPv6-cím létrehozása mellett dönt, a portál két külön nyilvános IP-cím erőforrás, egyet a minden egyes hozzárendelt IP-cím verziót hoz létre.|
    |IP-cím hozzárendelése (csak akkor látható, ha bejelölte a **hozzon létre egy IPv6-(vagy IPv4-alapú) címet** jelölőnégyzet)|Igen, ha bejelöli a **hozzon létre egy IPv6-alapú** (vagy IPv4-alapú) jelölőnégyzetet.|Ha a jelölőnégyzet felirat **IPv4-cím létrehozása**, egy hozzárendelési módszert. Ha a jelölőnégyzet felirat **IPv6-cím létrehozása**, nem egy hozzárendelési módszert választja, azt kell **dinamikus**.|
    |Előfizetés|Igen|Léteznie kell az azonos [előfizetés](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) a nyilvános IP-címet hozzárendelni kívánt erőforrásként.|
    |Erőforráscsoport|Igen|A azonos vagy eltérő, létezhet [erőforráscsoport](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group) a nyilvános IP-címet hozzárendelni kívánt erőforrásként.|
    |Hely|Igen|Léteznie kell az azonos [hely](https://azure.microsoft.com/regions), régió, mint a nyilvános IP-címet hozzárendelni kívánt erőforrásként cím is hivatkozott.|

**Parancsok**

Bár a portálon hozzon létre két nyilvános IP-cím erőforrás (egy IPv4- és egy IPv6-) lehetőséget biztosít, a következő parancssori felület és a PowerShell-parancsok egy IP-verziót, vagy a másik címmel hozzon létre egy erőforrást. Ha azt szeretné, hogy két nyilvános IP-cím erőforrás, egy az egyes IP-verziót, futtatnia kell a parancs kétszer, különböző neveket és a nyilvános IP-cím erőforrás-verziók. 

|Eszköz|Parancs|
|---|---|
|parancssori felület|[az hálózati nyilvános ip-létrehozása](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[Új AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress)|

## <a name="view-change-settings-for-or-delete-a-public-ip-address"></a>Megtekintése, módosítsa a beállításokat, vagy egy nyilvános IP-cím törlése

1. Jelentkezzen be a [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely a hálózat közreműködő szerepkört az előfizetés (minimum) hozzárendelt engedélyeit. Olvassa el a [Azure szerepköralapú hozzáférés-vezérlés beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) cikk tudhat meg többet a szerepköröket és engedélyeket hozzárendelése a fiókokhoz.
2. A mezőbe a szöveget tartalmazó *keresési erőforrások* az Azure portál felső részén írja be a *nyilvános IP-cím*. Ha **nyilvános IP-címek** jelenik meg a keresési eredmények között kattintson rá.
3. Az a **nyilvános IP-címek** panel, amelyen megjelenik, kattintson a nevére, a nyilvános IP-cím szeretné megtekinteni, beállításainak módosítása vagy törlése.
4. A panelen megjelenő a nyilvános IP-cím végezze el, attól függően, hogy megtekintéséhez, törölni vagy módosítani a nyilvános IP-cím a következő lehetőségek közül.
    - **Nézet**: A **áttekintése** a panel részén látható beállításait a nyilvános IP-címet, például a hálózati illesztő azt hozzá van rendelve (Ha egy hálózati adapter társítva hozzá a cím). A portál nem jelenik meg a címet (IPv4 vagy IPv6) verziója. Szeretné megtekinteni a fájlverzió-információkat, a PowerShell vagy a CLI parancs segítségével megtekintheti a nyilvános IP-cím. Ha az IP-cím verziót IPv6, a hozzárendelt címet nem jelenik meg a portálon, PowerShell vagy a parancssori felület. 
    - **Törlés**: a nyilvános IP-cím törléséhez kattintson **törlése** a a **áttekintése** részében találhatja. Ha a cím jelenleg társított IP-konfigurációt, nem lehet törölni. Ha a cím jelenleg társítva van egy konfigurációhoz, kattintson a **szüntesse** leválasztja a címet az IP-konfigurációt.
    - **Változás**: kattintson a **konfigurációs**. Az információk alapján a 4. lépésben a beállítások módosításához a [hozzon létre egy nyilvános IP-cím](#create-a-public-ip-address) című szakaszát. Ha módosítani szeretné egy IPv4-cím hozzárendelés statikus dinamikus, meg kell szüntetnie a nyilvános IPv4-cím, amelyekhez társítva vannak az IP-konfigurációja a. Majd módosítsa a hozzárendelési módszert dinamikus, majd kattintson az **társítása** hozzárendelni az IP cím az ugyanazon IP-konfiguráció, egy másik konfigurációt, vagy is hagyhatja leválasztása. Leválasztja a nyilvános IP-cím, az a **áttekintése** kattintson **szüntesse**.

>[!WARNING]
>Amikor a hozzárendelés metódus statikus dinamikus módosítjuk, az IP-cím, amely a nyilvános IP-cím lett rendelve elvesznek. Az Azure nyilvános DNS-kiszolgálók statikus vagy dinamikus címek és a DNS-Névcímke (Ha egy meghatározott) közötti leképezést karbantartása, amíg egy dinamikus IP-címet módosíthatja, ha a virtuális gép leállított (felszabadított) állapotában állapota után elindul. A cím megváltoztatása érdekében rendelje hozzá egy statikus IP-címet.

**Parancsok**

|Eszköz|Parancs|
|---|---|
|parancssori felület|[az nyilvános ip-azon hálózati](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#list) nyilvános IP-címeinek listáját, hogy [az hálózati nyilvános ip-megjelenítése](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#show) ; beállítások megjelenítése [az hálózati nyilvános ip-frissítés](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#update) frissítése; [az hálózati nyilvános ip-törlése](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#delete) törlése|
|PowerShell|[Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress?toc=%2fazure%2fvirtual-network%2ftoc.json) egy nyilvános IP-cím objektum beolvasása és a beállítások megtekintéséhez [Set-AzureRmPublicIpAddress](/powershell/resourcemanager/azurerm.network/set-azurermpublicipaddress?toc=%2fazure%2fvirtual-network%2ftoc.json) frissíteni a beállításait; [Remove-AzureRmPublicIpAddress](/powershell/module/azurerm.network/remove-azurermpublicipaddress) törlése|

## <a name="next-steps"></a>Következő lépések
Rendelje hozzá a nyilvános IP-címek, a következő Azure-erőforrások létrehozásakor:

- [Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-network%2ftoc.json) vagy [Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) virtuális gépek
- [Az Internet felé néző Azure terheléselosztó](../load-balancer/load-balancer-get-started-internet-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Az Azure alkalmazás átjáró](../application-gateway/application-gateway-create-gateway-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Pont-pont kapcsolat az Azure VPN Gateway használatával](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Az Azure virtuálisgép-méretezési csoport](../virtual-machine-scale-sets/virtual-machine-scale-sets-portal-create.md?toc=%2fazure%2fvirtual-network%2ftoc.json)