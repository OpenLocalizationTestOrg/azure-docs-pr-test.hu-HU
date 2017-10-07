---
title: "aaaHow tooplan a virtuális hálózat az Azure RemoteApp-gyűjteményt |} Microsoft Docs"
description: "Megtudhatja, hogyan tooplan a virtuális hálózat az Azure RemoteApp-gyűjteményt."
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
ms.openlocfilehash: d7eeefc3c66815b18f9338e2e428585e6f81a12a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooplan-your-virtual-network-for-azure-remoteapp"></a>Hogyan tooplan a virtuális hálózat az Azure RemoteApp
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.
> 
> 

Ez a dokumentum ismerteti, hogyan tooset mentése az Azure virtuális hálózathoz (VNET) és az Azure RemoteApp hello alhálózatot. Ha ismeri az Azure virtuális hálózataihoz, ami egy olyan képességet, amellyel Ön toovirtualize a hálózati infrastruktúra toohello felhő- és toocreate hibrid megoldások Azure és a helyszíni erőforrások. További információk [itt](../virtual-network/virtual-networks-overview.md).

Ha használni szeretne toodefine biztonsági házirendek (bejövő és kimenő) forgalmat a virtuális hálózat az Azure RemoteApp telepítéséhez, erősen ajánlott külön alhálózathoz létrehozása az Azure RemoteApp el a központi hello Azure hello többi virtuális hálózat. Hogyan toodefine biztonsági házirendek az Azure virtuális hálózati alhálózat további információkért olvassa el az [Mi az a hálózati biztonsági csoport (NSG)?](../virtual-network/virtual-networks-nsg.md).

## <a name="types-of-azure-remoteapp-collections-with-azure-virtual-networks"></a>Egy Azure virtuális hálózatot az Azure RemoteApp-gyűjtemény típusai
hello következő grafikus megjelenítése hello két másik adatgyűjtési beállítások Ha azt szeretné, hogy a virtuális hálózati toouse.

### <a name="azure-remoteapp-cloud-collection-with-vnet"></a>Az Azure RemoteApp felhőalapú gyűjtemény virtuális hálózaton
 ![Az Azure RemoteApp - felhőalapú gyűjtemény egy virtuális hálózaton](./media/remoteapp-planvpn/ra-cloudvpn.png)

Egy Azure RemoteApp-gyűjteményben, amelyen üzembe van helyezve, hello RemoteApp munkamenet állomások kell tooaccess összes hello erőforrást az Azure-ban jelképez. Ezek lehetnek a hello ugyanazon virtuális hello RemoteApp virtuális Hálózatot, vagy egy másik virtuális Hálózatot az Azure-ban.

### <a name="azure-remoteapp-hybrid-collection-with-vnet"></a>Az Azure RemoteApp hibrid gyűjtemény virtuális hálózaton
![Az Azure RemoteApp - hibrid gyűjtemény egy virtuális hálózaton](./media/remoteapp-planvpn/ra-hybridvpn.png)

Egy Azure RemoteApp-gyűjteménye, ahol hello erőforrásokhoz, hogy hello RemoteApp munkamenet állomások tooaccess telepített helyszíni jelképez. hello RemoteApp virtuális Hálózatot az Azure hibrid technológiákat, például a telephelyek közötti VPN vagy Express Route csatolt toohello a helyi hálózaton.

## <a name="how-hello-system-works"></a>Hello rendszer működése
Hello színfalak Azure RemoteApp telepíti az Azure virtuális gépek (a feltöltött lemezkép) toohello virtuális hálózati alhálózat kiépítése során megadottal. Hibrid gyűjtemény választotta, ha azt próbálja tooresolve hello hello munkafolyamat létrehozását hello DNS-kiszolgáló a megadott virtuális hálózat hello hello megadott tartományvezérlő teljes Tartománynevét.  
Ha tooan meglévő virtuális hálózathoz kapcsolódik, győződjön meg arról, hogy tooexpose hello szükséges portok a hálózati biztonsági csoportok az Azure RemoteApp alhálózatban. 

Ajánlott egy [elég nagy alhálózat az Azure RemoteApp](remoteapp-vnetsizing.md). Azure virtuális hálózat által támogatott legnagyobb hello /8 (CIDR alhálózati definíciók használatával). Az alhálózat legyen elég nagy tooaccommodate hello Azure RemoteApp virtuális gépeinek során méretezett amikor több felhasználó hello alkalmazásokhoz fér hozzá. 

Az alábbiakban tooenable lesz szükség a virtuális hálózati alhálózat hello dolgot: 

1. Kimenő forgalom hello alhálózatból engedélyezni kell a port tartomány 10101-10175 toocommunicate hello belső Azure RemoteApp szolgáltatások egyike.
2. Az alhálózati tooconnect tooAzure tárolót a 443-as porton engedélyezni kell a kimenő forgalom
3. Ha Azure-ban üzemeltetett Active Directory, győződjön meg arról a virtuális gép belül hello virtuális hálózati alhálózat az Azure RemoteApp képes tooconnect toothat tartományvezérlő. hello DNS-beli virtuális hálózaton hello képes tooresolve hello a tartományvezérlő teljes Tartománynevét kell lennie.

## <a name="virtual-network-with-forced-tunneling"></a>A kényszerített bújtatás virtuális hálózat
[A kényszerített bújtatás](../vpn-gateway/vpn-gateway-about-forced-tunneling.md) támogatása az összes új Azure RemoteApp-gyűjteményt. Jelenleg nem támogatjuk hello áttelepítését egy meglévő gyűjteményt toosupport kényszerített bújtatást.  Toodelete lesz a meglévő gyűjtemények hello tooAzure RemoteApp kapcsolódik, és hozzon létre egy új, a kényszerített bújtatás, a gyűjtemények engedélyezve egy tooget virtuális hálózat használatával. 

