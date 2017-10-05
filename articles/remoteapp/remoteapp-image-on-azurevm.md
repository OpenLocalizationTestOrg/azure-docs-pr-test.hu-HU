---
title: "Hozzon létre egy Azure virtuális Gépen alapuló rendszerképet az Azure RemoteApp |} Microsoft Docs"
description: "Lemezkép létrehozása az Azure RemoteApp kezdve az Azure virtuális géphez tudnivalók."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: d41583ef-6cd8-4115-8dcb-b2cd5b3d301a
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: ee64b86835af8e6237cddcd8acc779fc6dac8214
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-azure-remoteapp-image-based-on-an-azure-virtual-machine"></a>Egy Azure RemoteApp-rendszerképet az Azure virtuális gép létrehozása
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. A részletekért olvassa el a [bejelentést](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Azure RemoteApp-lemezképek (amely a gyűjteményben lévő megosztott alkalmazások tárolására) hozhat létre egy Azure virtuális gépről. Egy virtuálisgép-lemezkép az Azure RemoteApp kép összes követelménynek megfelelő Azure virtuális gép lemezképének gyűjteményébe hozzáadott használatára is kiválaszthatják,-használhatja, hogy a Virtuálisgép-lemezkép kiindulási pontként a saját virtuális gép, ha azt szeretné. Keresse a "Windows Server távoli asztali munkamenetgazda" kép a könyvtárban.

Hozzon létre egy saját egy Azure virtuális Gépen alapuló rendszerképet - lemezkép létrehozásához, és töltse fel azt az Azure virtuális gép könyvtárból Azure RemoteApp két lépésből áll.

## <a name="create-a-custom-image-based-on-an-azure-vm"></a>Egy Azure virtuális Gépen alapuló egyéni lemezkép létrehozása
Ezek a lépések segítségével hozzon létre egy Azure virtuális Gépen alapuló rendszerképet.

1. Hozzon létre egy Azure virtuális gépen. Használhatja a "Windows Server távoli asztali munkamenetgazda" vagy a "Windows Server távoli asztali munkamenet állomást a Microsoft Office 365 ProPlus" lemezkép az Azure virtuális gép lemezképének gyűjteményből. Ez a rendszerkép megfelel az összes Azure RemoteApp sablon kép.
   
    További információkért lásd: [Windows rendszerű virtuális gép létrehozása](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
2. Csatlakoztassa a virtuális Gépet, és telepítse, és konfigurálja a Remoteappen keresztül megosztani kívánt alkalmazásokat. Ellenőrizze, hogy minden további Windows-konfigurációt, az alkalmazások által megkövetelt végrehajtásához.
   
    További információkért lásd: [jelentkezzen be a virtuális gép futó Windows Server hogyan](../virtual-machines/windows/classic/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
3. A Windows Server távoli asztali munkamenetgazda lemezképeit egyik használ, nincs-e egy befoglalt érvényesítési parancsfájlt, amely biztosítja a virtuális gép megfelel-e a RemoteApp előtti reqs. Parancsfájl futtatásához kattintson duplán a **ValidateRemoteAppImage** az asztalon. Győződjön meg arról, hogy a szkript által jelzett összes hibát a következő lépés a folytatás előtt rögzítettek.
4. A SYSPREP generalize, és a lemezképének rögzítése. Lásd: [sablon Windows virtuális gép rögzítése](../virtual-machines/windows/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) utasításokat.

## <a name="import-the-image-into-the-azure-remoteapp-image-library"></a>A kép importálása az Azure RemoteApp kép könyvtárába.
Az új lemezkép az Azure Remoteappba importálásához tegye a következőket:

1. Az a **Sablonrendszerképek** lapon:
   
   * Ha nincs meglévő lemezképet, kattintson a **feltölteni, vagy importáljon egy sablon rendszerképet**.
   * Ha már legalább egy kép, kattintson a  **+**  hozzáadása egy új lemezképet.
2. Válassza ki **kép importálása a virtuális gépek** könyvtárban, és kattintson **következő**.
3. A következő oldalon válassza ki az egyéni lemezképet a listából, és győződjön meg arról, hogy követték-e a lemezkép létrehozásakor ismertetett lépéseket. Kattintson a **Tovább** gombra.
4. Adjon meg egy nevet az új RemoteApp-lemezkép, és mentse a helyre, majd kattintson a pipa jelre az importálási folyamat elindításához.

> [!NOTE]
> Lemezképek Azure bárhonnan, Azure virtuális gépek által támogatott bármely Azure RemoteApp által támogatott Azure helyre importálhatja. Attól függően, hogy a helyek az importálás legfeljebb 25 percig is tarthat.
> 
> 

Most már készen áll az új gyűjtemény, vagy hozzon létre egy [felhő](remoteapp-create-cloud-deployment.md) gyűjtemény vagy [hibrid](remoteapp-create-hybrid-deployment.md), attól függően, az igényeknek megfelelően.

