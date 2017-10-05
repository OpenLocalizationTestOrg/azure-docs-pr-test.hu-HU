---
title: "Hálózati kapcsolat visszaállítása az Azure virtuális gépekhez Windows |} Microsoft Docs"
description: "Bemutatja, hogyan Azure Windows virtuális hálózati adapter alaphelyzetbe"
services: virtual-machines-windows, azure-resource-manager
documentationcenter: 
author: genlin
manager: willchen
editor: 
tags: top-support-issue, azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: genli
ms.openlocfilehash: 220e426be20086841854d89831f6c9d67529867f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-reset-network-interface-for-azure-windows-vm"></a>Az Azure virtuális gépekhez Windows hálózati kapcsolat visszaállítása 

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Nem lehet csatlakoztatni a Microsoft Azure Windows virtuális gép (VM) után tiltsa le az alapértelmezett hálózati illesztő (NIC), vagy manuálisan egy statikus IP-cím beállítása a hálózati adaptert. Ez a cikk bemutatja, hogyan alaphelyzetbe állítani a hálózati illesztő Azure Windows virtuális gép, amely a távoli kapcsolati probléma elhárítható.

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]
## <a name="reset-network-interface"></a>Hálózati adapter alaphelyzetbe állítása

### <a name="for-classic-vms"></a>Klasszikus virtuális gépekhez

Hálózati illesztő visszaállításához kövesse az alábbi lépéseket:

1.  Nyissa meg az [Azure Portal]( https://ms.portal.azure.com).
2.  Válassza ki **virtuális gépek (klasszikus)**.
3.  Válassza ki az érintett virtuális gépet.
4.  Válassza ki **IP-címek**.
5.  Ha a **privát IP-hozzárendelés** nem **statikus**, módosítsa úgy, hogy **statikus**.
6.  Módosítsa a **IP-cím** egy másik, elérhető az alhálózat IP-címre.
7.  A mentés kiválasztása.
8.  A virtuális gép újraindul, a rendszer az új hálózati inicializálása.
9.  Próbálja meg az RDP a számítógépen. Ha sikeres, módosíthatja a magánhálózati IP-cím vissza az eredeti Ha szeretné. Ellenkező esetben beállíthatja, hogy azt. 

### <a name="for-vms-deployed-in-resource-group-model"></a>Erőforrás csoport modellben telepített virtuális gépek

1.  Nyissa meg az [Azure Portal]( https://ms.portal.azure.com).
2.  Válassza ki az érintett virtuális gépet.
3.  Válassza ki **hálózati illesztőt**.
4.  Válassza ki a géphez társított hálózati illesztőt
5.  Válassza ki **IP-konfigurációk**.
6.  Válassza ki az IP-cím. 
7.  Ha a **privát IP-hozzárendelés** nem **statikus**, módosítsa úgy, hogy **statikus**.
8.  Módosítsa a **IP-cím** egy másik, elérhető az alhálózat IP-címre.
9. A virtuális gép újraindul, a rendszer az új hálózati inicializálása.
10. Próbálja meg az RDP a számítógépen. Ha sikeres, módosíthatja a magánhálózati IP-cím vissza az eredeti Ha szeretné. Ellenkező esetben beállíthatja, hogy azt. 

## <a name="delete-the-unavailable-nics"></a>Az elérhető hálózati adapterek törlése
Után is a távoli asztal a géphez, törölnie kell a régi hálózati adaptert a potenciális problémák elkerülése érdekében:

1.  Nyissa meg az Eszközkezelőben.
2.  Válassza ki **nézet** > **rejtett eszközök megjelenítése**.
3.  Válassza ki **hálózati adapterek**. 
4.  Ellenőrizze, hogy az adapterek, mint a "Microsoft Hyper-V hálózati Adapter" nevű.
5.  Láthatja, hogy az nem érhető el adapter, amely szürkén jelenik meg. Kattintson a jobb gombbal az adaptert, és válassza az eltávolítás.

    ![a hálózati adapter képe](media/reset-network-interface/nicpage.png)

    > [!NOTE]
    > Csak távolítsa el a "Microsoft Hyper-V hálózati Adapter" nevű nem érhető el adaptereket. Ha bármelyik más rejtett adapter távolítja el, további problémákat okozhat.
    >
    >

6.  Most már nem érhető el az összes adapter jelölését törölje a rendszerből.