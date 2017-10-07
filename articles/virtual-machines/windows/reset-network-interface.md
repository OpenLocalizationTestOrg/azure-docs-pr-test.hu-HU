---
title: "az Azure virtuális gépekhez Windows aaaHow tooreset hálózati illesztő |} Microsoft Docs"
description: "Bemutatja, hogyan tooreset a hálózati kapcsolat az Azure virtuális gépekhez Windows"
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
ms.openlocfilehash: 1b653820927ef4c3bb8f384a7e752846a8be3da9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooreset-network-interface-for-azure-windows-vm"></a>Hogyan tooreset a hálózati kapcsolat az Azure virtuális gépekhez Windows 

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Hello alapértelmezett hálózati illesztő (NIC) letiltása után a tooMicrosoft Azure Windows virtuális gép (VM) nem tud csatlakozni, vagy manuálisan egy statikus IP-cím beállítása hello hálózati adaptert. Ez a cikk bemutatja, hogyan tooreset hello hello távoli kapcsolati probléma elhárítható Azure Windows virtuális hálózati adapter.

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]
## <a name="reset-network-interface"></a>Hálózati adapter alaphelyzetbe állítása

### <a name="for-classic-vms"></a>Klasszikus virtuális gépekhez

tooreset hálózati csatoló, kövesse az alábbi lépéseket:

1.  Nyissa meg toohello [Azure-portálon]( https://ms.portal.azure.com).
2.  Válassza ki **virtuális gépek (klasszikus)**.
3.  Jelölje be hello hatással a virtuális gép.
4.  Válassza ki **IP-címek**.
5.  Ha hello **privát IP-hozzárendelés** nem **statikus**, túl megváltoztatnia**statikus**.
6.  Változás hello **IP-cím** hello alhálózat elérhető IP-cím tooanother.
7.  A mentés kiválasztása.
8.  virtuális gép hello tooinitialize hello új hálózati toohello rendszer újraindul.
9.  Próbálja meg tooRDP tooyour gép. Ha sikeres, hello magán IP cím hátsó toohello eredeti módosíthatja, ha azt szeretné. Ellenkező esetben beállíthatja, hogy azt. 

### <a name="for-vms-deployed-in-resource-group-model"></a>Erőforrás csoport modellben telepített virtuális gépek

1.  Nyissa meg toohello [Azure-portálon]( https://ms.portal.azure.com).
2.  Jelölje be hello hatással a virtuális gép.
3.  Válassza ki **hálózati illesztőt**.
4.  Válassza ki a géphez tartozó hálózati illesztőt hello
5.  Válassza ki **IP-konfigurációk**.
6.  Jelölje be hello IP-címet. 
7.  Ha hello **privát IP-hozzárendelés** nem **statikus**, túl megváltoztatnia**statikus**.
8.  Változás hello **IP-cím** hello alhálózat elérhető IP-cím tooanother.
9. virtuális gép hello tooinitialize hello új hálózati toohello rendszer újraindul.
10. Próbálja meg tooRDP tooyour gép. Ha sikeres, hello magán IP cím hátsó toohello eredeti módosíthatja, ha azt szeretné. Ellenkező esetben beállíthatja, hogy azt. 

## <a name="delete-hello-unavailable-nics"></a>Törlés hello nem érhető el a hálózati adapterek
Után is távoli asztali toohello géphez, törölnie kell az hello régi hálózati adapterek tooavoid hello lehetséges problémát:

1.  Nyissa meg az Eszközkezelőben.
2.  Válassza ki **nézet** > **rejtett eszközök megjelenítése**.
3.  Válassza ki **hálózati adapterek**. 
4.  Ellenőrizze, mint a "Microsoft Hyper-V hálózati Adapter" nevű hello adapterek.
5.  Láthatja, hogy az nem érhető el adapter, amely szürkén jelenik meg. Kattintson a jobb gombbal a hello adapter, és válassza az eltávolítás.

    ![a hálózati adapter hello hello képe](media/reset-network-interface/nicpage.png)

    > [!NOTE]
    > Csak eltávolítása nem érhető el adapterek hello hello neve "Microsoft Hyper-V hálózati Adapter" használatát. Ha eltávolítja a hello bármely más rejtett adapterek, akkor további problémák.
    >
    >

6.  Most már nem érhető el az összes adapter jelölését törölje a rendszerből.