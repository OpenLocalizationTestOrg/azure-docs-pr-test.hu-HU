---
title: "Létrehozásához és kezeléséhez claimable virtuális gépeket egy, amikor a Azure DevTest Labs szolgáltatásban |} Microsoft Docs"
description: "Megtudhatja, hogyan claimable virtuális gép hozzáadása egy laborhoz a Azure DevTest Labs szolgáltatásban"
services: devtest-lab,virtual-machines
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: f671e66e-9630-4e30-a131-a6bad9ed9c11
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2017
ms.author: v-craic
ms.openlocfilehash: a27423a75cb2b5063156109ea9ee3a45fa036c07
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/02/2018
---
# <a name="create-and-manage-claimable-vms-in-azure-devtest-labs"></a>Hozzon létre és claimable Azure DevTest Labs szolgáltatásban virtuális gépeinek kezelése
Ad hozzá egy claimable virtuális labor be, hogyan hasonló módon, [szabványos virtuális gép hozzáadása](devtest-lab-add-vm.md) – a egy *alap* , amely vagy egy [egyéni lemezkép](devtest-lab-create-template.md), [képlet](devtest-lab-manage-formulas.md) , vagy [Piactéri lemezképhez](devtest-lab-configure-marketplace-images.md). Ez az oktatóanyag végigvezeti az Azure portál használata claimable virtuális gép hozzáadása egy laborhoz a DevTest Labs szolgáltatásban, és a folyamatok jeleníti meg a felhasználói jogcímek és a virtuális gép unclaim követi.

## <a name="steps-to-add-a-claimable-vm-to-a-lab-in-azure-devtest-labs"></a>Ismerteti a végrehajtás lépéseit claimable virtuális gép hozzáadása egy laborhoz a Azure DevTest Labs szolgáltatásban
1. Jelentkezzen be az [Azure Portalra](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Válassza ki **több szolgáltatások**, majd válassza ki **DevTest Labs** a listából.
1. Labs listában jelölje ki a labor kívánja claimable virtuális gép létrehozása.  
1. A tesztlabor a **áttekintése** ablaktáblán válassza előbb **+ Hozzáadás**.  

    ![Adja hozzá a virtuális gép gomb](./media/devtest-lab-add-vm/devtestlab-home-blade-add-vm.png)

1. Az a **base válasszon** területen válassza ki a megfelelő alapja a virtuális gép számára.
1. A a **virtuális gép** panelen adjon meg egy nevet az új virtuális gép a **virtuálisgép-nevet** szövegmezőben.

    ![Labor VM ablaktábla](./media/devtest-lab-add-vm/devtestlab-lab-vm-blade.png)

1. Adjon meg egy **felhasználónév** kap, amely rendszergazdai jogosultságokkal a virtuális gépen.  
1. Ha azt szeretné tárolni a jelszó használatát a [titkos tároló](https://azure.microsoft.com/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store), jelölje be **mentett titkos kulcs használata**, és adja meg a kulcs értéke, amely megfelel a titkos kulcs (jelszó). Ellenkező esetben a feliratú szövegmezőbe írja be a jelszót **írjon be egy értéket**.
1. A **virtuális gép lemeztípus** határozza meg, hogy milyen típusú a virtuális gépek a tesztkörnyezetben.
1. Válassza ki **virtuálisgép-méret** , és válassza ki a Processzormagok RAM memória méretét és a merevlemez mérete a virtuális gép létrehozásához adjon meg előre meghatározott elemek.
1. Válassza ki **összetevők** és az összetevők listájában válassza ki és konfigurálja az alapjául szolgáló lemezképhez hozzáadni kívánt összetevők. Ha ismerkedik a DevTest Labs szolgáltatásban, vagy tekintse meg az összetevők, konfigurálása a [meglévő összetevő felvétele a virtuális gépek](devtest-lab-add-vm.md#add-an-existing-artifact-to-a-vm) szakaszt, és térjen vissza ide befejezésekor.
1. Válassza ki **speciális beállítások** a virtuális gép hálózati beállításai és a lejárati beállítások konfigurálása. A **jogcím-beállítások**, válassza a **Igen** claimable ellenőrizze a gép számára.

  ![Válassza a virtuális gép claimable legyen.](./media/devtest-lab-add-vm/devtestlab-claim-VM-option.png)

1. Ha szeretné megtekinteni, illetve az Azure Resource Manager sablon másolása, tekintse meg a [mentése Azure Resource Manager-sablon](devtest-lab-add-vm.md#save-azure-resource-manager-template) szakaszt, és térjen vissza ide, ha befejeződött.
1. Válassza ki **létrehozása** a megadott virtuális gép hozzáadása a labor.

   A virtuális gép létrehozása a állapotjelzéssel jelenik meg, mint első **létrehozása**, majd **futtató** a virtuális gép elindítása után.

> [!NOTE]
> Ha a labor virtuális gépeken keresztül telepít [Azure Resource Manager-sablonok](devtest-lab-create-environment-from-arm.md), claimable virtuális gépeket hozhat létre úgy, hogy a **allowClaim** tulajdonság igaz értékű a Tulajdonságok szakaszának.
>
>

## <a name="using-a-claimable-vm"></a>A claimable virtuális gépek használata

A felhasználó igényelhet a virtuális gép "Claimable virtual machines" közül lépések egyikének végrehajtásával:

* "Claimable virtuális gépnek" a labor "Overview" ablaktábla alján a listából, kattintson a jobb gombbal a virtuális gépeket, a lista egyik, és válassza a **jogcím gép**.

 ![Egy adott claimable VM kérelmet.](./media/devtest-lab-add-vm/devtestlab-claim-VM.png)


* A "Overview" ablak tetején válassza **minden jogcím**. Egy véletlenszerű virtuális géphez claimable virtuális gépek közül van hozzárendelve.

 ![Bármely claimable VM kérelmet.](./media/devtest-lab-add-vm/devtestlab-claim-any.png)


Miután a felhasználó a jogcímek egy virtuális Gépet, a átkerül az "A virtuális gépnek" tartalmazó, és már nem claimable, amelyet semmilyen más felhasználó.

## <a name="unclaim-a-vm"></a>A virtuális gépek unclaim

Amikor egy felhasználó a hivatkozott virtuális gépek használatának befejezése után, és azt szeretné, ha valaki más szeretné elérhetővé tenni, akkor visszatérhet az igényelt VM claimable virtuális gépek listájának lépések egyikének végrehajtásával:

- "A virtuális gépnek" a listából, kattintson a jobb gombbal a lista – a virtuális gépek egyik vagy a három ponttal (…) – válasszon, majd válassza a **Unclaim**.

  ![A listán szereplő virtuális gép virtuális gép unclaim.](./media/devtest-lab-add-vm/devtestlab-unclaim-VM2.png)

- A "A virtuális gépnek", jelölje ki a felügyelet ablaktábla megnyitása, majd válassza ki a virtuális gépek **Unclaim** felső menü.

  ![A virtuális gép felügyeleti ablaktábláján egy virtuális gép unclaim.](./media/devtest-lab-add-vm/devtestlab-unclaim-VM.png)

Amikor egy felhasználó egy virtuális gép unclaims, azok többé nem kell azokat az engedélyeket az adott VM adott labor.

### <a name="transferring-the-data-disk"></a>Az adatlemez átvitele
Claimable VM-e, és a felhasználó kapcsolódik adatlemezt unclaims azt, a adatlemez marad a virtuális gép. Ha egy másik felhasználó majd jogcímeket, hogy virtuális Gépet, hogy új felhasználói jogcímeket a adatlemez, valamint a virtuális gép.

Ez az úgynevezett "átvitele a adatlemez". Az adatlemez majd válik elérhetővé az új felhasználó partnereinek listájához **az adatlemezek** számukra.

![Az adatlemezek unclaim.](./media/devtest-lab-add-vm/devtestlab-unclaim-datadisks.png)



## <a name="next-steps"></a>További lépések
* A már létrehozott, keresztül csatlakozhat a virtuális gép kiválasztásával **Connect** a a felügyeleti ablaktábla.
* Megismerkedhet a [DevTest Labs Azure Resource Manager gyorsindítási sablonok galéria](https://github.com/Azure/azure-devtestlab/tree/master/Samples).
