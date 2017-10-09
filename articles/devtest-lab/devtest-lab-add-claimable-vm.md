---
title: "aaaAdd egy claimable VM tooa, amikor a Azure DevTest Labs szolgáltatásban |} Microsoft Docs"
description: "Megtudhatja, hogyan tooadd egy claimable virtuális gép tooa, amikor a Azure DevTest Labs szolgáltatásban"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: f671e66e-9630-4e30-a131-a6bad9ed9c11
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/17/2017
ms.author: tarcher
ms.openlocfilehash: fe6385ae2e59b9636b82aec250dc3a1f8a40ba5d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-claimable-vm-tooa-lab-in-azure-devtest-labs"></a>Az Azure DevTest Labs claimable VM tooa labor hozzáadása
Egy virtuális gép claimable tooa, amikor hozzáadja egy hasonló módon toohow meg [szabványos virtuális gép hozzáadása](devtest-lab-add-vm.md) – a egy *alap* , amely vagy egy [egyéni lemezkép](devtest-lab-create-template.md), [képlet](devtest-lab-manage-formulas.md), vagy [Piactéri lemezképhez](devtest-lab-configure-marketplace-images.md). Ez az oktatóanyag végigvezeti az Azure portál tooadd hello segítségével a DevTest Labs szolgáltatásban claimable VM tooa labor, és a felhasználó a következő virtuális gép tooclaim hello hello menetét mutatja.

> [!NOTE]
> Ha a labor virtuális gépeken keresztül telepít [Azure Resource Manager-sablonok](devtest-lab-create-environment-from-arm.md), claimable virtuális gépek létrehozásához hello beállítása **allowClaim** tulajdonság tootrue hello tulajdonságok szakaszban.
>
>

## <a name="steps-tooadd-a-claimable-vm-tooa-lab-in-azure-devtest-labs"></a>Lépéseket tooadd egy claimable VM tooa, amikor a Azure DevTest Labs szolgáltatásban
1. Jelentkezzen be toohello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Válassza ki **több szolgáltatások**, majd válassza ki **DevTest Labs** hello listából.
1. Labs hello listában jelölje ki hello labor kívánja a toocreate hello claimable virtuális gép.  
1. A hello labor **áttekintése** panelen válassza **+ Hozzáadás**.  

    ![Adja hozzá a virtuális gép gomb](./media/devtest-lab-add-vm/devtestlab-home-blade-add-vm.png)

1. A hello **base válasszon** panelen válassza ki a megfelelő virtuális gép hello alapja.
1. A hello **virtuális gép** panelen hello hello új virtuális gép nevét adja meg **virtuálisgép-nevet** szövegmezőben.

    ![Labor VM panel](./media/devtest-lab-add-vm/devtestlab-lab-vm-blade.png)

1. Adjon meg egy **felhasználónév** kap, amely rendszergazdai jogosultságokkal hello virtuális gépen.  
1. Ha azt szeretné, hogy toouse jelszó tárolja a [titkos tároló](https://azure.microsoft.com/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store), jelölje be **mentett titkos kulcs használata**, és adja meg a kulcs értéke, amely megfelel a tooyour titkos (jelszó). Ellenkező esetben feliratú hello szövegmezőbe írja be a jelszót **írjon be egy értéket**.
1. Hello **virtuális gép lemeztípus** határozza meg, hogy milyen típusú hello virtuális gépek hello tesztkörnyezetben.
1. Válassza ki **virtuálisgép-méret** hello közül előre meg hello Processzormagok, a RAM memória méretét és a merevlemez méretének hello hello VM toocreate elemekre.
1. Válassza ki **összetevők** és összetevők hello listából válassza ki és konfigurálja, amelyet az tooadd toohello alapjául szolgáló lemezképhez hello összetevők. Ha új tooDevTest Labs vagy konfigurálása összetevők, tekintse meg a toohello [hozzáadása egy meglévő virtuális gép összetevő tooa](devtest-lab-add-vm.md#add-an-existing-artifact-to-a-vm) szakaszt, és térjen vissza ide befejezésekor.
1. Válassza ki **speciális beállítások** tooconfigure hello Virtuálisgép-hálózati beállítások és a lejárati beállítások. A **beállítások jogcím**, válassza a **Igen** toomake hello gép claimable.

  ![Válassza ki a toomake hello VM claimable.](./media/devtest-lab-add-vm/devtestlab-claim-VM-option.png)

1. Ha szeretné, hogy tooview vagy hello Azure Resource Manager sablon másolása, tekintse meg a toohello [mentése Azure Resource Manager-sablon](devtest-lab-add-vm.md#save-azure-resource-manager-template) szakaszt, és térjen vissza ide, ha befejeződött.
1. Válassza ki **létrehozása** tooadd hello megadott virtuális gép toohello labor.
1. hello labor panel állapotát jeleníti meg az hello hello virtuális gép létrehozás - először mint **létrehozása**, majd, mint a **futtató** hello virtuális gép elindítása után.


## <a name="using-a-claimable-vm"></a>A claimable virtuális gépek használata

A felhasználó igényelhet a virtuális gép "Claimable virtual machines" hello listája lépések egyikének végrehajtásával:

* Hello listájából "Claimable virtual machines" hello hello tesztlabor áttekintése panel alsó részén, kattintson a jobb gombbal a virtuális gépek hello hello lista egyik, és válassza a **jogcím gép**.

 ![Egy adott claimable VM kérelmet.](./media/devtest-lab-add-vm/devtestlab-claim-VM.png)


* Hello hello tetején **áttekintése** paneljén válassza **minden jogcím**. Egy véletlenszerű virtuális géphez claimable virtuális gépek listájából hello van hozzárendelve.

 ![Bármely claimable VM kérelmet.](./media/devtest-lab-add-vm/devtestlab-claim-any.png)


Miután a felhasználó a jogcímek egy virtuális Gépet, a átkerül az "A virtuális gépnek" tartalmazó, és már nem claimable, amelyet semmilyen más felhasználó.

## <a name="next-steps"></a>Következő lépések
* Egyszer hello a virtuális gép létrehozása, csatlakoztathatja toohello virtuális gép kiválasztásával **Connect** hello VM panelen.
* Fedezze fel hello [DevTest Labs Azure Resource Manager gyorsindítási sablonok gyűjteménye](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates)
