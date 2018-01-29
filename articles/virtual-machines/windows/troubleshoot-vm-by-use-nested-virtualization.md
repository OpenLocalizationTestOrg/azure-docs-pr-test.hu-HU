---
title: "Azure virtuális gép kapcsolatos probléma megoldása beágyazott virtualizálási használatával az Azure-ban |} Microsoft Docs"
description: "Azure virtuális gép probléma elhárítása beágyazott virtualizálási használatával az Azure-ban"
services: virtual-machines-windows
documentationcenter: 
author: glimoli
manager: timlt
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 11/06/2017
ms.author: genli
ms.openlocfilehash: b50d139f62ef773fb5b9bb2638e40f8154e80979
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/03/2018
---
# <a name="troubleshoot-a-problem-azure-vm-by-using-nested-virtualization-in-azure"></a>Azure virtuális gép kapcsolatos probléma megoldása beágyazott virtualizálási használatával az Azure-ban

Ez a cikk bemutatja, hogyan hozhat létre egy beágyazott virtualizálási környezet a Microsoft Azure-ban, így akkor is csatlakoztathatja a lemezt a virtuális gép hibát a Hyper-V állomás (a helyreállítási virtuális gép) hibaelhárítási célból.

## <a name="prerequisite"></a>Előfeltétel

Csatlakoztassa a virtuális gép problémát, a helyreállítási virtuális gép a következő feltételnek kell megfelelnie:

-   A helyreállítási virtuális Gépet, a problémát a virtuális gép ugyanazon a helyen kell lennie.

-   A helyreállítási virtuális Gépet, a problémát a virtuális gép ugyanabban az erőforráscsoportban kell lennie.

-   A helyreállítási virtuális Gépet kell használnia a Tárfiók (Standard vagy prémium) azonos típusú, a problémát a virtuális gép.

## <a name="step-1-create-a-recovery-vm-and-install-hyper-v-role"></a>1. lépés: A helyreállítási virtuális gép létrehozása és Hyper-V szerepkör telepítése

1.  Hozzon létre egy új helyreállítási virtuális Gépet:

    -  Operációs rendszer: Windows Server 2016 Datacenter

    -  Méret: Bármely V3 adatsorozat és legalább két, hogy a beágyazott támogatási virtualizálási processzormag. További információkért lásd: [vezet be, az új Dv3 és Ev3 VM-méretek](https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/).

    -  Azonos helyen Tárfiók és erőforráscsoporthoz tartozik, mint a virtuális gép probléma.

    -  Jelöljön ki azonos tárolási módot, a problémát a virtuális gép (Standard vagy prémium).

2.  A helyreállítási virtuális gép után jön létre, a helyreállítási virtuális gép távoli asztal.

3.  A Kiszolgálókezelő ablakában válassza **kezelése** > **szerepkörök és szolgáltatások hozzáadása**.

4.  Az a **telepítési típus** szakaszban jelölje be **szerepköralapú vagy szolgáltatásalapú telepítés**.

5.  Az a **jelölje be a célkiszolgáló** területen győződjön meg arról, hogy a helyreállítási virtuális gép el van-e kiválasztva.

6.  Válassza ki a **Hyper-V szerepkör** > **szolgáltatások hozzáadása**.

7.  Válassza ki **következő** a a **szolgáltatások** szakasz.

8.  Ha egy virtuális hálózathoz nem érhető el, válassza ki azt. Máskülönben válassza **következő**.

9.  Az a **áttelepítési** szakaszban jelölje be **tovább**

10. Az a **alapértelmezett tárak** szakaszban jelölje be **következő**.

11. Jelölje be a jelölőnégyzetet, hogy a kiszolgáló automatikusan újraindul, ha szükséges.

12. Válassza ki **telepítése**.

13. Lehetővé teszi a kiszolgáló a Hyper-V szerepkör telepítése. Ez néhány percet vesz igénybe, és a kiszolgáló automatikusan újraindul.

## <a name="step-2-create-the-problem-vm-on-the-recovery-vms-hyper-v-server"></a>2. lépés: A problémát a virtuális gép létrehozása a helyreállítási virtuális Gépet a Hyper-V kiszolgálón

1.  Jegyezze fel a nevét, a lemez a virtuális gép probléma, és törölje a virtuális gép probléma. Győződjön meg arról, hogy őrizze meg az összes csatlakoztatott lemezek. 

2.  A problémát a virtuális gép operációsrendszer-lemez csatolása a helyreállítási virtuális gép adatainak lemezként.

    1.  Nyissa meg a problémát a virtuális gép törlődik, miután a helyreállítási virtuális gép.

    2.  Válassza ki **lemezek**, majd **Hozzáadás adatlemez**.

    3.  A probléma virtuális lemezt, majd válassza ki és **mentése**.

3.  Miután a lemez sikeresen csatolt, a távoli asztal a helyreállítási virtuális gép.

4.  Nyissa meg a Lemezkezelés (Diskmgmt.msc beépülő modult). Győződjön meg arról, hogy a probléma a lemez virtuális gép beállításai **Offline**.

5.  Nyissa meg a Hyper-V kezelőjével: A **Kiszolgálókezelő**, jelölje be a **Hyper-V szerepkör**. Kattintson a jobb gombbal a kiszolgáló, és válassza ki a **Hyper-V kezelője**.

6.  A Hyper-V kezelőjében kattintson a jobb gombbal a helyreállítási virtuális Gépet, majd válassza ki **új** > **virtuális gép** > **következő**.

7.  Adjon meg egy nevet a virtuális géphez, majd válassza ki **következő**.

8.  Válassza ki **1. generációs**.

9.  Állítsa be az indítási memória: legalább 1024 MB.

10. Ha van ilyen válassza ki a Hyper-V hálózati kapcsoló, amely hozták létre. Ellenkező esetben helyezze át a következő oldalra.

11. Válassza ki **virtuális merevlemez csatlakoztatása később**.

    ![a kép kapcsolatos a csatolás egy újabb virtuális merevlemez beállítása](./media/troubleshoot-vm-by-use-nested-virtualization/attach-disk-later.png)

12. Válassza ki **Befejezés** a virtuális gép létrehozásakor.

13. Kattintson a jobb gombbal a létrehozott, és válassza ki a virtuális **beállítások**.

14. Válassza ki **IDE-vezérlő 0**, jelölje be **merevlemez**, és kattintson a **Hozzáadás**.

    ![a kép kapcsolatos hozzáadja az új merevlemez-meghajtóról](./media/troubleshoot-vm-by-use-nested-virtualization/create-new-drive.png)    

15. A **fizikai merevlemez**, válassza ki a lemezt a virtuális Gépet, amely az Azure virtuális géphez csatolt probléma. Ha nem látja a lemezeken, ellenőrizze, ha a lemez értéke offline a Lemezkezelés eszköz használatával.

    ![a kép kapcsolatos csatlakoztatja a lemezt](./media/troubleshoot-vm-by-use-nested-virtualization/mount-disk.png)  


17. Válassza ki **alkalmaz**, majd válassza ki **OK**.

18. Kattintson duplán a virtuális Gépre, és indítsa el.

19. Most már használhatja a virtuális Gépen a helyszíni virtuális gépként. Hibaelhárítási lépéseket kell követhető.

## <a name="step-3-re-create-your-azure-vm-in-azure"></a>3. lépés: Hozza létre az Azure virtuális gép az Azure-ban

1.  Miután a virtuális gép ismét online elérhetővé, állítsa le a virtuális Gépet a Hyper-V kezelőjében.

2.  Lépjen a [Azure-portálon](https://portal.azure.com) válassza ki a helyreállítási virtuális gép > lemezek, másolja a lemez neve. A név a következő lépésben fogja használni. Válassza le a rögzített méretű lemezt a helyreállítási virtuális Gépet.

3.  Ugrás a **összes erőforrás**, és keresse meg a lemez neve, majd válassza ki a lemezt.

     ![a kép kapcsolatos keres](./media/troubleshoot-vm-by-use-nested-virtualization/search-disk.png)     

4. Kattintson a **hozzon létre virtuális Gépet**.

     ![a kép kapcsolatos hoz létre a virtuális gép a lemezről](./media/troubleshoot-vm-by-use-nested-virtualization/create-vm-from-vhd.png) 

Azure PowerShell használatával a virtuális gép létrehozása a lemezen. További információkért lásd: [a új virtuális gép létrehozása meglévő lemez a PowerShell használatával](create-vm-specialized.md#create-the-new-vm). 

## <a name="next-steps"></a>További lépések

Ha a virtuális Géphez való kapcsolódás problémát tapasztal, tekintse meg [hibáinak elhárítása az RDP-kapcsolatok egy Azure virtuális gépre](troubleshoot-rdp-connection.md). A virtuális gépen futó alkalmazások elérésével problémákkal kapcsolatban lásd: [alkalmazás csatlakozási problémák a Windows virtuális gép](troubleshoot-app-connection.md).
