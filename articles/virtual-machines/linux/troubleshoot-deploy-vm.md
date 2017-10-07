---
title: "Linux virtuális gépekkel kapcsolatos problémákat, az Azure-ban telepítése aaaTroubleshoot |} Microsoft Docs"
description: "Problémamegoldás telepítését Linux virtuális gép Azurethe Resource Manager üzembe helyezési modellben."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 4e383427-4aff-4bf3-a0f4-dbff5c6f0c81
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: genli
ms.openlocfilehash: d1092ca3d9d51af7510b19c8c9a79e6dda588697
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-deploying-linux-virtual-machine-issues-in-azure"></a>Problémamegoldás telepítését Linux virtuális gép az Azure-ban

tootroubleshoot virtuális gép (VM) telepítési problémákat Azure, tekintse át a hello [leggyakoribb problémák](#top-issues) a gyakori hibák és megoldására.

Ha ez a cikk bármely pontján további segítségre van szüksége, forduljon az Azure-szakértők hello [MSDN Azure és a Stack Overflow fórumok hello](https://azure.microsoft.com/support/forums/). Másik lehetőségként is fájl az Azure támogatási incidens. Nyissa meg toohello [az Azure támogatási webhelyén](https://azure.microsoft.com/support/options/) válassza ki **támogatja az beszerzése**.

## <a name="top-issues"></a>Problémák
[!INCLUDE [virtual-machines-linux-troubleshoot-deploy-vm-top](../../../includes/virtual-machines-linux-troubleshoot-deploy-vm-top.md)]

## <a name="hello-cluster-cannot-support-hello-requested-vm-size"></a>hello fürt nem támogatja a kért Virtuálisgép-méretet hello
<properties
supportTopicIds="123456789"
resourceTags="windows"
productPesIds="1234, 5678"
/>
- Próbálja megismételni a kérelmet hello kisebb Virtuálisgép-méretet.
- Ha hello a hello mérete nem lehet módosítani a virtuális gép kért:
    - Állítsa le a rendelkezésre állási csoport hello hello virtuális gépeinek. Kattintson a **erőforráscsoportok** > az erőforráscsoport > **erőforrások** > a rendelkezésre állási csoport > **virtuális gépek** > a virtuális gép > **leállítása**.
    - Amikor az összes virtuális gépek leállítási hello, és hello VM szükséges hello mérete.
    - Indítsa el először hello új virtuális Gépet, és jelölje hello virtuális gépek leállítása, majd válassza az Indítás parancsot.


## <a name="hello-cluster-does-not-have-free-resources"></a>hello fürtnek nincs szabad erőforrást
<properties
supportTopicIds="123456789"
resourceTags="windows"
productPesIds="1234, 5678"
/>
- Próbálkozzon újra később hello kéréssel.
- Ha hello új virtuális gép is része egy másik rendelkezésre állási beállítani
    - Hozzon létre egy virtuális Gépet egy másik rendelkezésre állási készlet (a hello ugyanabban a régióban).
    - Adja hozzá a hello új virtuális gép toohello ugyanazt a virtuális hálózatot.

## <a name="how-do-i-activate-my-monthly-credit-for-visual-studio-enterprise-bizspark"></a>Hogyan aktiválhatja a Visual Studio Enterprise (BizSpark) a havi keretet

tooactivate a havi jóváírása, ez [cikk](https://azure.microsoft.com/offers/ms-azr-0064p/).

## <a name="why-can-i-not-install-hello-gpu-driver-for-an-ubuntu-nv-vm"></a>Miért nem telepíthetők hello GPU illesztőprogramjának portok HV Ubuntu virtuális gép?

Linux GPU támogatja jelenleg csak Azure virtuális gépeken NC Ubuntu Server 16.04 LTS futtató érhető el. További információkért lásd: [N-sorozat linuxos virtuális gépek GPU illesztőprogramok beállítása](n-series-driver-setup.md).

## <a name="my-drivers-are-missing-for-my-linux-n-series-vm"></a>Az illesztőprogramok hiányoznak a Linux N sorozatú virtuális Gépemhez

Linux-alapú virtuális gépek illesztőprogramok találhatók [Itt](n-series-driver-setup.md). 

## <a name="i-cant-find-a-gpu-instance-within-my-n-series-vm"></a>A GPU példánya nem található a N sorozatú virtuális Gépen belül

tootake előny az Azure N sorozatú virtuális gépeket futtathatnak Windows Server 2016 hello GPU lehetőségeit vagy Windows Server 2012 R2, telepítenie kell NVIDIA video-illesztőprogramok a virtuális gépek telepítést követően. Illesztőprogram-telepítő információ [Windows virtuális gépek](../windows/n-series-driver-setup.md) és [Linux virtuális gépek](n-series-driver-setup.md).

## <a name="is-n-series-vms-available-in-my-region"></a>Érhető el N sorozatú virtuális gépek régiómban?

Ellenőrizheti a hello rendelkezésre biztosít hello [régió tábla által elérhető termékek](https://azure.microsoft.com/regions/services), és az árképzés terén [Itt](https://azure.microsoft.com/pricing/details/virtual-machines/series/#n-series).

## <a name="i-am-not-able-toosee-vm-size-family-that-i-want-when-resizing-my-vm"></a>A számítógépen nem tudja toosee Virtuálisgép-méretet termékcsalád, amelyek szeretnék, ha a virtuális gép átméretezésével.

Amikor egy virtuális gép fut, nem telepített tooa fizikai kiszolgálón. hello fizikai kiszolgálók Azure-régiók közös fizikai hardver fürtök vannak csoportosítva. A hello VM áthelyezése toobe toodifferent hardver fürtök igénylő virtuális gépek átméretezésével eltér attól függően, hogy melyik üzembe helyezési modellel használt toodeploy hello VM volt.

- Klasszikus üzembe helyezési modellel, hello felhő szolgáltatástelepítés telepített virtuális gépek el kell távolítani, és toochange hello virtuális gépek tooa mérete egy másik mérete termékcsalád újratelepíteni.

- Erőforrás-kezelő üzembe helyezési modellben telepített virtuális gépek, le kell állítania minden virtuális gép hello rendelkezésre állási csoportban, a virtuális gép hello rendelkezésre állási készlet hello méretének módosítása előtt.

## <a name="hello-listed-vm-size-is-not-supported-while-deploying-in-availability-set"></a>hello felsorolt Virtuálisgép-méret nem támogatott a rendelkezésre állási csoport központi telepítése során.

Hello rendelkezésre állási csoport fürtön támogatott méret kiválasztása. Ajánlott létrehozásakor rendelkezésre állási készlet toochoose hello legnagyobb Virtuálisgép-méretet úgy gondolja, hogy kell, és rendelkezik, amelyek az első központi telepítési toohello rendelkezésre állási csoportban.

## <a name="what-linux-distributionsversions-are-supported-on-azure"></a>Milyen Linux terjesztéseket/verziók támogatottak az Azure-on?

Hello listáján, a Linux található [Azure-Endorsed Terjesztéseket](endorsed-distros.md).

## <a name="can-i-add-an-existing-classic-vm-tooan-availability-set"></a>Adhat hozzá egy meglévő klasszikus virtuális gép tooan rendelkezésre állási csoportot?

Igen. Hozzáadhat egy meglévő klasszikus virtuális gép tooa új vagy meglévő rendelkezésre állási csoportban. További információ: [adja hozzá a virtuális gép tooan rendelkezésre állási készlet](../windows/classic/configure-availability.md#addmachine).


## <a name="next-steps"></a>Következő lépések
Ha ez a cikk bármely pontján további segítségre van szüksége, forduljon az Azure-szakértők hello [MSDN Azure és a Stack Overflow fórumok hello](https://azure.microsoft.com/support/forums/).

Másik lehetőségként is fájl az Azure támogatási incidens. Nyissa meg toohello [az Azure támogatási webhelyén](https://azure.microsoft.com/support/options/) válassza ki **támogatja az beszerzése**.
