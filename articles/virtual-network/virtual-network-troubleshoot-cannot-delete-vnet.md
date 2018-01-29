---
title: "Nem lehet törölni a virtuális hálózat az Azure-ban |} Microsoft Docs"
description: "Útmutató az Azure virtuális hálózat nem törölhető a probléma elhárítása érdekében."
services: virtual-network
documentationcenter: na
author: chadmath
manager: cshepard
editor: 
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/12/2017
ms.author: genli
ms.openlocfilehash: 60e4a0dbbc15b0414bf1b81e1fd7885e47aa962d
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshooting-failed-to-delete-a-virtual-network-in-azure"></a>Hibaelhárítás: Nem sikerült törölni a virtuális hálózat az Azure-ban

Hiba fordulhat elő, a Microsoft Azure virtuális hálózat törlése közben. Ez a cikk nyújt hibaelhárítási segítséget nyújtanak a probléma megoldásához. 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="troubleshooting-guidance"></a>Hibaelhárítási útmutató 

1. [Ellenőrizze, hogy a virtuális hálózati átjáró fut-e a virtuális hálózat](#check-whether-a-virtual-network-gateway-is-running-in-the-virtual-network).
2. [Ellenőrizze, hogy a virtuális hálózaton fut-e a meglévő Alkalmazásátjáró](#check-whether-an-application-gateway-is-running-in-the-virtual-network).
3. [Ellenőrizze, hogy az Azure Active Directory tartományi szolgáltatások engedélyezve van-e a virtuális hálózat](#check-whether-azure-active-directory-domain-service-is-enabled-in-the-virtual-network).
4. [Ellenőrizze, hogy a virtuális hálózathoz kapcsolódik-e más erőforrás](#check-whether-the-virtual-network-is-connected-to-other-resource).
5. [Ellenőrizze, hogy a virtuális gépek továbbra is fut a virtuális hálózat](#check-whether-a-virtual-machine-is-still-running-in-the-virtual-network).
6. [Ellenőrizze, hogy a virtuális hálózati áttelepítés Beragadt](#check-whether-the-virtual-network-is-stuck-in-migration).

## <a name="troubleshooting-steps"></a>hibaelhárítási útmutatóját

### <a name="check-whether-a-virtual-network-gateway-is-running-in-the-virtual-network"></a>Ellenőrizze, hogy a virtuális hálózati átjáró fut-e a virtuális hálózat

Távolítsa el a virtuális hálózatot, akkor először el kell távolítani a virtuális hálózati átjáró.

Klasszikus virtuális hálózatok, látogasson el a **áttekintése** oldalán a klasszikus virtuális hálózatot az Azure portálon. Az a **VPN-kapcsolatok** szakaszban, ha a virtuális hálózatot az átjáró fut, jelenik meg az átjáró IP-címét. 

![Ellenőrizze, hogy az átjáró fut-e](media/virtual-network-troubleshoot-cannot-delete-vnet/classic-gateway.png)

Virtuális hálózatok, látogasson el a **áttekintése** a virtuális hálózat lap. Ellenőrizze **csatlakoztatott eszközök** virtuális hálózati átjáró.

![Ellenőrizze a csatlakoztatott eszköz](media/virtual-network-troubleshoot-cannot-delete-vnet/vnet-gateway.png)

Mielőtt eltávolíthatná az átjáró, először távolítsa el **kapcsolat** az átjáró objektumokat. 

### <a name="check-whether-an-application-gateway-is-running-in-the-virtual-network"></a>Ellenőrizze, hogy fut-e olyan átjárót a virtuális hálózat

Lépjen a **áttekintése** a virtuális hálózat lap. Ellenőrizze a **csatlakoztatott eszközök** az az Alkalmazásátjáró.

![Ellenőrizze a csatlakoztatott eszköz](media/virtual-network-troubleshoot-cannot-delete-vnet/app-gateway.png)

Ha olyan átjárót, el kell távolítania azt a virtuális hálózat törlése előtt.

### <a name="check-whether-azure-active-directory-domain-service-is-enabled-in-the-virtual-network"></a>Ellenőrizze, hogy az Azure Active Directory tartományi szolgáltatások engedélyezve van-e a virtuális hálózat

Ha az Active Directory tartományi szolgáltatások engedélyezve van, és a virtuális hálózathoz csatlakoztatva van, ez a virtuális hálózat nem törölhető. 

![Ellenőrizze a csatlakoztatott eszköz](media/virtual-network-troubleshoot-cannot-delete-vnet/enable-domain-services.png)

A szolgáltatás letiltása, lásd: [letiltása Azure Active Directory tartományi szolgáltatások az Azure portál használatával](../active-directory-domain-services/active-directory-ds-disable-aadds.md).

### <a name="check-whether-the-virtual-network-is-connected-to-other-resource"></a>Ellenőrizze, hogy a virtuális hálózathoz kapcsolódik-e más erőforrás

Ellenőrizze a kapcsolatcsoport hivatkozások, a kapcsolatok és a virtuális hálózati társviszony. Ezek egyikét sem okozhat a virtuális hálózat törlése sikertelen. 

Az ajánlott Törlés sorrendje a következőképpen:

1. Átjáró-kapcsolatok
2. Átjárók
3. IP-címek
4. Virtuális hálózati társviszony
5. App Service Environment-környezet (ASE)

### <a name="check-whether-a-virtual-machine-is-still-running-in-the-virtual-network"></a>Ellenőrizze, hogy a virtuális gépek továbbra is fut a virtuális hálózat

Győződjön meg arról, hogy egyetlen virtuális gép a virtuális hálózat.

### <a name="check-whether-the-virtual-network-is-stuck-in-migration"></a>Ellenőrizze, hogy a virtuális hálózati áttelepítés Beragadt

Ha a virtuális hálózati Beragadt a migrálási állapota, nem lehet törölni. Az áttelepítés megszakítja a következő parancsot, és törölje a virtuális hálózat.

    Move-AzureVirtualNetwork -VirtualNetworkName "Name" -Abort

## <a name="next-steps"></a>Következő lépések

- [Azure Virtual Network](virtual-networks-overview.md)
- [Azure Virtual Network – Gyakori kérdések (GYIK)](virtual-networks-faq.md)
