---
title: "aaaCreate az első virtuális gép egy tesztkörnyezetben, a Azure DevTest Labs szolgáltatásban |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate az első virtuális gép egy tesztkörnyezetben, a Azure DevTest Labs szolgáltatásban"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: fbc5a438-6e02-4952-b654-b8fa7322ae5f
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/24/2017
ms.author: tarcher
ms.openlocfilehash: 4c3257efca9be6fdd190eaac1db731464e07fcfd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-vm-in-a-lab-in-azure-devtest-labs"></a>Az első virtuális gép létrehozása az Azure DevTest Labs szolgáltatásban egy tesztkörnyezetben

Kezdetben DevTest Labs eléréséhez, és szeretné, hogy az első virtuális gép toocreate, akkor lesz valószínűleg megteheti segítségével előre betöltött [alap Piactéri lemezképhez](devtest-lab-configure-marketplace-images.md). Később a is lesz a képes toochoose egy [egyéni lemezképet, és egy képletet](devtest-lab-add-vm.md) további virtuális gépek létrehozásakor. 

Ez az oktatóanyag bemutatja, hogyan hello Azure portál tooadd használatával a virtuális gép első tooa labor a DevTest Labs szolgáltatásban.

## <a name="steps-tooadd-your-first-vm-tooa-lab-in-azure-devtest-labs"></a>Lépések tooadd az első virtuális gép tooa tesztlabor a Azure DevTest Labs szolgáltatásban
1. Jelentkezzen be toohello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Válassza ki **több szolgáltatások**, majd válassza ki **DevTest Labs** hello listából.
1. Labs hello listáról válassza ki a használni kívánt virtuális gép toocreate hello hello labor.  
1. A hello labor **áttekintése** panelen válassza **+ Hozzáadás**.  

    ![Adja hozzá a virtuális gép gomb](./media/devtest-lab-add-vm/devtestlab-home-blade-add-vm.png)

1. A hello **base válasszon** panelen válassza a Piactéri lemezképet a virtuális gép hello.
1. A hello **virtuális gép** panelen hello hello új virtuális gép nevét adja meg **virtuálisgép-nevet** szövegmezőben.

    ![Labor VM panel](./media/devtest-lab-add-vm/devtestlab-lab-add-first-vm.png)

1. Adjon meg egy **felhasználónév** kap, amely rendszergazdai jogosultságokkal hello virtuális gépen.  
1. Címkével hello szövegmezőbe írja be a jelszót **írjon be egy értéket**.
1. Hello **virtuális gép lemeztípus** határozza meg, hogy milyen típusú hello virtuális gépek hello tesztkörnyezetben.
1. Válassza ki **virtuálisgép-méret** hello közül előre meg hello Processzormagok, a RAM memória méretét és a merevlemez méretének hello hello VM toocreate elemekre.
1. Válassza ki **összetevők** és - műtermék - hello listából válassza ki és konfigurálja, amelyet az tooadd toohello alapjául szolgáló lemezképhez hello összetevők.
    **Megjegyzés:** Ha új tooDevTest Labs vagy konfigurálása összetevők, tekintse meg a toohello [hozzáadása egy meglévő virtuális gép összetevő tooa](./devtest-lab-add-vm.md#add-an-existing-artifact-to-a-vm) szakaszt, és térjen vissza ide befejezésekor.
1. Válassza ki **létrehozása** tooadd hello megadott virtuális gép toohello labor.

   hello labor panel állapotát jeleníti meg az hello hello virtuális gép létrehozás - először mint **létrehozása**, majd, mint a **futtató** hello virtuális gép elindítása után.

## <a name="next-steps"></a>Következő lépések
* Egyszer hello a virtuális gép létrehozása, csatlakoztathatja toohello virtuális gép kiválasztásával **Connect** hello VM panelen.
* Tekintse meg [VM tooa labor hozzáadása](devtest-lab-add-vm.md) bővebb információt a további virtuális gépek hozzáadása a tesztkörnyezetben.
* Fedezze fel hello [Office DevTest Labs Azure Resource Manager gyorsindítási sablonok](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates).
