---
title: "aaaCannot törli a virtuális hálózatot az Azure-ban |} Microsoft Docs"
description: "Ismerje meg, hogyan tootroubleshoot hello probléma az Azure virtuális hálózat nem törölhető."
services: virtual-network
documentationcenter: na
author: chadmath
manager: cshepard
editor: 
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/17/2017
ms.author: genli
ms.openlocfilehash: a9050ab238ccb0380fd46130430222efb8f42388
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-failed-toodelete-a-virtual-network-in-azure"></a>Hibáinak elhárítása: Egy virtuális hálózatot az Azure-ban toodelete nem sikerült.

Hibák akkor fordulhat elő, amikor megpróbál toodelete a Microsoft Azure virtuális hálózat. Ez a cikk ismerteti a hibaelhárítási lépéseket toohelp a probléma megoldásához. 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="troubleshooting-guidance"></a>Hibaelhárítási útmutató 

1. [Ellenőrizze, hogy a virtuális hálózati átjáró fut-e a virtuális hálózati hello](#check-whether-a-virtual-network-gateway-is-running-in-the-virtual-network).
2. [Ellenőrizze, hogy fut-e olyan átjárót hello virtuális hálózat](#check-whether-an-application-gateway-is-running-in-the-virtual-network).
3. [Ellenőrizze, hogy engedélyezve van-e Azure Active Directory tartományi szolgáltatások a virtuális hálózati hello](#check-whether-azure-active-directory-domain-service-is-enabled-in-the-virtual-network).
4. [Ellenőrizze, hogy a virtuális hálózati hello csatlakoztatott tooother erőforrás](#check-whether-the-virtual-network-is-connected-to-other-resource).
5. [Ellenőrizze, hogy egy virtuális gép továbbra is fut a virtuális hálózati hello](#check-whether-a-virtual-machine-is-still-running-in-the-virtual-network).
6. [Ellenőrizze, hogy hello virtuális hálózati áttelepítés Beragadt](#check-whether-the-virtual-network-is-stuck-in-migration).

## <a name="troubleshooting-steps"></a>Hibaelhárítási lépések

### <a name="check-whether-a-virtual-network-gateway-is-running-in-hello-virtual-network"></a>Ellenőrizze, hogy fut-e a virtuális hálózati átjáró hello virtuális hálózatban

tooremove hello virtuális hálózaton, akkor először el kell távolítania hello virtuális hálózati átjáró.

Klasszikus virtuális hálózatot, nyissa meg a toohello **áttekintése** hello klasszikus virtuális hálózatot az Azure-portálon hello oldalán. A hello **VPN-kapcsolatok** szakaszban, ha hello virtuális hálózati átjáró hello fut, látni fogja a hello IP hello átjáró címét. 

![Ellenőrizze, hogy az átjáró fut-e](media/virtual-network-troubleshoot-cannot-delete-vnet/classic-gateway.png)

Virtuális hálózatok, nyissa meg a toohello **áttekintése** hello virtuális hálózati oldalán. Ellenőrizze **csatlakoztatott eszközök** hello virtuális hálózati átjáró.

![Ellenőrizze a hello csatlakoztatott eszközön](media/virtual-network-troubleshoot-cannot-delete-vnet/vnet-gateway.png)

Mielőtt eltávolíthatná hello átjáró, először távolítsa el **kapcsolat** hello átjáró objektumokat. 

### <a name="check-whether-an-application-gateway-is-running-in-hello-virtual-network"></a>Ellenőrizze, hogy fut-e olyan átjárót hello virtuális hálózatban

Nyissa meg toohello **áttekintése** hello virtuális hálózati oldalán. Ellenőrizze a hello **csatlakoztatott eszközök** az hello Alkalmazásátjáró.

![Ellenőrizze a hello csatlakoztatott eszközön](media/virtual-network-troubleshoot-cannot-delete-vnet/app-gateway.png)

Ha olyan átjárót, el kell távolítania azt hello virtuális hálózat törlése előtt.

### <a name="check-whether-azure-active-directory-domain-service-is-enabled-in-hello-virtual-network"></a>Ellenőrizze, hogy engedélyezve van-e Azure Active Directory tartományi szolgáltatások hello virtuális hálózatban

Ha Active Directory tartományi szolgáltatások hello engedélyezett és a csatlakoztatott toohello virtuális hálózat, a virtuális hálózat nem törölhető. 

![Ellenőrizze a hello csatlakoztatott eszközön](media/virtual-network-troubleshoot-cannot-delete-vnet/enable-domain-services.png)

toodisable hello szolgáltatást, kövesse az alábbi lépéseket:

1. Nyissa meg toohello [a klasszikus Azure portálon](https://manage.windowsazure.com).
2. Hello bal oldali ablaktáblában jelöljön ki **Active Directory**.
3. Válassza ki, amelyen engedélyezve van az Active Directory tartományi szolgáltatások hello Azure Active Directory (Azure AD) címtár.
4. Jelölje be hello **konfigurálása** fülre.
5. A **tartományi szolgáltatások**, hello módosítása **engedélyezése tartományi szolgáltatásokat a címtárhoz** beállítás túl**nem**.  

### <a name="check-whether-hello-virtual-network-is-connected-tooother-resource"></a>Ellenőrizze, hogy a virtuális hálózati hello csatlakoztatott tooother erőforrás

Ellenőrizze a kapcsolatcsoport hivatkozások, a kapcsolatok és a virtuális hálózati társviszony. Ezek a virtuális hálózat törlése toofail okozhat. 

hello ajánlott Törlés sorrendje a következőképpen történik:

1. Átjáró-kapcsolatok
2. Átjárók
3. IP-címek
4. Virtuális hálózati társviszony
5. App Service Environment-környezet (ASE)

### <a name="check-whether-a-virtual-machine-is-still-running-in-hello-virtual-network"></a>Ellenőrizze, hogy egy virtuális gép továbbra is fut a virtuális hálózati hello

Győződjön meg arról, hogy egyetlen virtuális gép hello virtuális hálózat.

### <a name="check-whether-hello-virtual-network-is-stuck-in-migration"></a>Ellenőrizze, hogy hello virtuális hálózati áttelepítés Beragadt

Ha a virtuális hálózati hello Beragadt a migrálási állapota, nem lehet törölni. Futtassa a következő parancs tooabort hello áttelepítési hello, és törölje a virtuális hálózati hello.

    Move-AzureVirtualNetwork -VirtualNetworkName "Name" -Abort

## <a name="next-steps"></a>Következő lépések

- [Azure Virtual Network](virtual-networks-overview.md)
- [Azure Virtual Network – Gyakori kérdések (GYIK)](virtual-networks-faq.md)