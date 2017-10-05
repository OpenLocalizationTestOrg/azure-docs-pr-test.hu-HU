---
title: "Az Azure RemoteApp-gyűjteményt a virtuális hálózat tervezése |} Microsoft Docs"
description: "Megtudhatja, hogyan tervezze meg a virtuális hálózat az Azure RemoteApp-gyűjteményt."
services: remoteapp
documentationcenter: 
author: mghosh1616
manager: mbaldwin
ms.assetid: ad9aff0e-f374-49c0-951d-4a7be1c36de0
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
ms.openlocfilehash: 1eb8115b13fb18074b4c4726b69e3d9faf387c32
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-plan-your-virtual-network-for-azure-remoteapp"></a>Az Azure RemoteApp a virtuális hálózat tervezése
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. A részletekért olvassa el a [bejelentést](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Ez a dokumentum ismerteti, hogyan állíthat be az Azure virtuális hálózathoz (VNET) és az alhálózat az Azure RemoteApp. Ha ismeri az Azure virtuális hálózataihoz, ez a funkció lehetővé teszi a hálózati infrastruktúra a felhő virtualizálása és hibrid megoldások létrehozásához Azure és a helyszíni erőforrások az. További információk [itt](../virtual-network/virtual-networks-overview.md).

Ha szeretne forgalom (bejövő és kimenő) biztonsági házirendeket definiálhat az Azure RemoteApp telepítéséhez a virtuális hálózaton, erősen ajánlott külön alhálózathoz létrehozása az Azure RemoteApp többi részétől a telepítések az Azure-ban virtuális hálózat. További információ a biztonsági házirendek definiálása az Azure virtuális hálózati alhálózat, olvassa el [Mi az a hálózati biztonsági csoport (NSG)?](../virtual-network/virtual-networks-nsg.md).

## <a name="types-of-azure-remoteapp-collections-with-azure-virtual-networks"></a>Egy Azure virtuális hálózatot az Azure RemoteApp-gyűjtemény típusai
A következő grafikus megjelenítése a két másik adatgyűjtési beállítások, ha egy virtuális hálózatot szeretne.

### <a name="azure-remoteapp-cloud-collection-with-vnet"></a>Az Azure RemoteApp felhőalapú gyűjtemény virtuális hálózaton
 ![Az Azure RemoteApp - felhőalapú gyűjtemény egy virtuális hálózaton](./media/remoteapp-planvpn/ra-cloudvpn.png)

Egy Azure RemoteApp-gyűjteményben, amelyen üzembe van helyezve a RemoteApp-munkamenetben gazdagépeket elérését igénylő összes erőforrást az Azure-ban jelképez. Ezek lehetnek, ugyanazt a virtuális Hálózatot, a RemoteApp virtuális Hálózatot, vagy egy másik virtuális Hálózatot az Azure-ban.

### <a name="azure-remoteapp-hybrid-collection-with-vnet"></a>Az Azure RemoteApp hibrid gyűjtemény virtuális hálózaton
![Az Azure RemoteApp - hibrid gyűjtemény egy virtuális hálózaton](./media/remoteapp-planvpn/ra-hybridvpn.png)

Egy Azure RemoteApp-gyűjteményt, amelyben a forrásokhoz való hozzáférést igénylő a RemoteApp-munkamenet gazdagépek is telepített a helyszíni jelképez. A RemoteApp virtuális Hálózatot használó Azure hibrid technológiák, például a pont-pont VPN vagy Express Route a helyszíni hálózathoz kapcsolódik.

## <a name="how-the-system-works"></a>A rendszer működése
A színfalak Azure RemoteApp telepíti az Azure virtuális gépek (a feltöltött lemezkép) a virtuális hálózati alhálózat kiépítése során megadottal. Hibrid gyűjtemény választotta, ha azt megpróbálja feloldani a DNS-kiszolgálóval, a virtuális hálózat szerepel a telepítési munkafolyamat megadott tartományvezérlő teljes Tartománynevét.  
Ha egy meglévő virtuális hálózathoz csatlakozik, győződjön meg arról, teszi közzé a szükséges portokat a hálózati biztonsági csoportok az Azure RemoteApp alhálózatban. 

Ajánlott egy [elég nagy alhálózat az Azure RemoteApp](remoteapp-vnetsizing.md). A legnagyobb Azure virtuális hálózat által támogatott /8 (CIDR alhálózati definíciók használatával). Az alhálózat elég nagynak kell lennie az Azure RemoteApp VMs olyan méretezett során, amikor több felhasználó érnek el alkalmazásokat. 

Az alábbiakban a dolgokhoz, szüksége lesz ahhoz, hogy a virtuális hálózati alhálózat: 

1. Az alhálózat kimenő forgalmát engedélyezni kell a porttartomány 10101-10175 egy belső Azure RemoteApp szolgáltatás kommunikálni.
2. Kimenő forgalom engedélyezni kell a alhálózatból csatlakozni az Azure Storage a 443-as port
3. Ha Azure-ban üzemeltetett Active Directory, győződjön meg arról e virtuális Gépet a virtuális hálózati alhálózat az Azure RemoteApp belül tartományvezérlőhöz csatlakozni. A DNS-beli virtuális hálózaton kell tudnia oldani a tartományvezérlő teljes Tartománynevét.

## <a name="virtual-network-with-forced-tunneling"></a>A kényszerített bújtatás virtuális hálózat
[A kényszerített bújtatás](../vpn-gateway/vpn-gateway-about-forced-tunneling.md) támogatása az összes új Azure RemoteApp-gyűjteményt. Jelenleg nem támogatjuk a kényszerített bújtatás támogatásához egy meglevő gyűjteményhez áttelepítését.  Hogy törölje a meglévő gyűjtemények a VNET, amely kapcsolódik az Azure RemoteApp segítségével, és hozzon létre egy újat az beszerzése a kényszerített bújtatás, a gyűjtemények engedélyezve. 

