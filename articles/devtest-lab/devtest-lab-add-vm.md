---
title: "aaaAdd egy virtuális gép tooa, amikor a Azure DevTest Labs szolgáltatásban |} Microsoft Docs"
description: "Megtudhatja, hogyan tooadd egy virtuális gép tooa, amikor a Azure DevTest Labs szolgáltatásban"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/24/2017
ms.author: tarcher
ms.openlocfilehash: 82838e4349550db56de311264c188140b9556b24
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-vm-tooa-lab-in-azure-devtest-labs"></a>A Azure DevTest Labs szolgáltatásban virtuális gép tooa labor hozzáadása
Ha már rendelkezik [az első virtuális gép létrehozása](devtest-lab-create-first-vm.md), akkor valószínűleg sor került az előre betöltött [Piactéri lemezképhez](devtest-lab-configure-marketplace-images.md). Mostantól, ha azt szeretné, hogy további virtuális gépek tooyour labor tooadd, dönthet úgy is egy *alap* , amely vagy egy [egyéni lemezkép](devtest-lab-create-template.md) vagy egy [képlet](devtest-lab-manage-formulas.md). Ez az oktatóanyag bemutatja, hogyan hello Azure portál tooadd VM tooa labor használata a DevTest Labs szolgáltatásban.

Ez a cikk azt is bemutatja, hogyan toomanage hello összetevők egy virtuális Gépet a tesztkörnyezetben.

## <a name="steps-tooadd-a-vm-tooa-lab-in-azure-devtest-labs"></a>Lépéseket tooadd egy virtuális gép tooa, amikor a Azure DevTest Labs szolgáltatásban
1. Jelentkezzen be toohello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Válassza ki **több szolgáltatások**, majd válassza ki **DevTest Labs** hello listából.
1. Labs hello listáról válassza ki a használni kívánt virtuális gép toocreate hello hello labor.  
1. A hello labor **áttekintése** panelen válassza **+ Hozzáadás**.  

    ![Adja hozzá a virtuális gép gomb](./media/devtest-lab-add-vm/devtestlab-home-blade-add-vm.png)

1. A hello **base válasszon** panelen válassza ki a megfelelő virtuális gép hello alapja.
1. A hello **virtuális gép** panelen hello hello új virtuális gép nevét adja meg **virtuálisgép-nevet** szövegmezőben.

    ![Labor VM panel](./media/devtest-lab-add-vm/devtestlab-lab-vm-blade.png)

1. Adjon meg egy **felhasználónév** kap, amely rendszergazdai jogosultságokkal hello virtuális gépen.  
1. Ha azt szeretné, hogy toouse jelszó tárolja a [titkos tároló](https://azure.microsoft.com/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store), jelölje be **mentett titkos kulcs használata**, és adja meg a kulcs értéke, amely megfelel a tooyour titkos (jelszó). Ellenkező esetben feliratú hello szövegmezőbe írja be a jelszót **írjon be egy értéket**.
1. Hello **virtuális gép lemeztípus** határozza meg, hogy milyen típusú hello virtuális gépek hello tesztkörnyezetben.
1. Válassza ki **virtuálisgép-méret** hello közül előre meg hello Processzormagok, a RAM memória méretét és a merevlemez méretének hello hello VM toocreate elemekre.
1. Válassza ki **összetevők** és - műtermék - hello listából válassza ki és konfigurálja, amelyet az tooadd toohello alapjául szolgáló lemezképhez hello összetevők.
    **Megjegyzés:** Ha új tooDevTest Labs vagy konfigurálása összetevők, tekintse meg a toohello [hozzáadása egy meglévő virtuális gép összetevő tooa](#add-an-existing-artifact-to-a-vm) szakaszt, és térjen vissza ide befejezésekor.
1. Válassza ki **speciális beállítások** tooconfigure hello Virtuálisgép-hálózati beállítások és a lejárati beállítások. 

   tooset lejárati lehetőség kiválasztása hello naptár ikon toospecify a dátum, amelyen hello a virtuális gép automatikusan törölve lesznek.  Alapértelmezés szerint a virtuális gép hello nem jár. 
1. Ha szeretné, hogy tooview vagy hello Azure Resource Manager sablon másolása, tekintse meg a toohello [mentése Azure Resource Manager-sablon](#save-azure-resource-manager-template) szakaszt, és térjen vissza ide, ha befejeződött.
1. Válassza ki **létrehozása** tooadd hello megadott virtuális gép toohello labor.
1. hello labor panel állapotát jeleníti meg az hello hello virtuális gép létrehozás - először mint **létrehozása**, majd, mint a **futtató** hello virtuális gép elindítása után.

> [!NOTE]
> [Adja hozzá a claimable virtuális gépek](devtest-lab-add-claimable-vm.md) bemutatja, hogyan toomake hello-e a virtuális gép claimable, hogy bármely felhasználója hello laborban érhető el.
>
>

## <a name="add-an-existing-artifact-tooa-vm"></a>Adja hozzá egy meglévő összetevő tooa méretű VM
A virtuális gépek létrehozásakor a meglévő összetevőket is hozzáadhat. Minden tesztkörnyezeti hello nyilvános DevTest Labs összetevő-adattárban lévő összetevők, valamint, hogy a létrehozott és a hozzáadott tooyour saját Összetevőtárban összetevőket tartalmazza.

* Az Azure DevTest Labs *összetevők* lehetővé teszik, hogy adja meg *műveletek* , amelyek végrehajtása közben hello virtuális gép ki van építve, például a Windows PowerShell-parancsfájlok futtatásakor, Bash parancsok futtatása, és a szoftver telepítése.
* Összetevő *paraméterek* lehetővé teszik, hogy az adott forgatókönyv szerint hello összetevő testreszabása

Hogyan toocreate összetevőihez: toodiscover hello cikk [megtudhatja, hogyan tooauthor a saját összetevők használata DevTest Labs](devtest-lab-artifact-author.md).

1. Jelentkezzen be toohello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Válassza ki **több szolgáltatások**, majd válassza ki **DevTest Labs** hello listából.
1. Labs hello listában jelölje ki kívánja toowork hello VM tartalmazó hello labor.  
1. Válassza ki **a virtuális gépek**.
1. Hello jelölje be a keresett virtuális gép.
1. Válassza ki **összetevők**. 
1. Válassza ki **összetevők alkalmazása**.
1. A hello **összetevők alkalmazása** panelen, jelölje be hello összetevő tooadd toohello VM kívánja.
1. A hello **Hozzáadás összetevő** panelen adja meg a szükséges hello paraméterértékek és a választható paramétereket, amelyekre szüksége van.  
1. Válassza ki **Hozzáadás** tooadd hello összetevő, és térjen vissza toohello **összetevők alkalmazása** panelen.
1. Folytassa az összetevők csak a virtuális gép szükséges.
1. Miután az összetevők adott hozzá, [összetevők vannak futtassa mely hello hello sorrendjének módosítása](#change-the-order-in-which-artifacts-are-run). Is visszatérhet túl[megtekintése vagy módosítása egy közbülső](#view-or-modify-an-artifact).
1. Amikor befejezte az összetevők hozzáadása, válassza ki a **alkalmaz**

## <a name="change-hello-order-in-which-artifacts-are-run"></a>Az összetevők futnak, amelyben hello sorrend módosítása
Alapértelmezés szerint hello hello műtermék végrehajtás, amelyben hozzáadásuk után toohello VM hello sorrendben. hello következő lépések bemutatják, hogyan toochange hello mely hello összetevők futásának sorrendje.

1. Hello hello tetején **összetevők alkalmazása** panelen, jelölje be hello hivatkozás toohello VM hozzáadott összetevők hello számát.
   
    ![A hozzáadott tooVM összetevők száma](./media/devtest-lab-add-vm-with-artifacts/devtestlab-add-artifacts-blade-selected-artifacts.png)
1. A hello **kiválasztott összetevők** panelen, a fogd és vidd hello összetevők hello a kívánt sorrendben. **Megjegyzés:** Ha gondja támad hello összetevő húzással, ellenőrizze, hogy bal oldalán található hello összetevő hello húz. 
1. Kattintson az **OK** gombra, amikor végzett.  

## <a name="view-or-modify-an-artifact"></a>Megtekintése vagy módosítása egy összetevő
hello következő lépések bemutatják, hogyan tooview vagy módosítsa az összetevő hello paramétereket:

1. Hello hello tetején **összetevők alkalmazása** panelen, jelölje be hello hivatkozás toohello VM hozzáadott összetevők hello számát.
   
    ![A hozzáadott tooVM összetevők száma](./media/devtest-lab-add-vm-with-artifacts/devtestlab-add-artifacts-blade-selected-artifacts.png)
1. A hello **kiválasztott összetevők** panelen, jelölje be hello összetevő tooview szeretné, vagy szerkesztheti.  
1. A hello **Hozzáadás összetevő** panelen, ellenőrizze a szükséges módosításokat, és válassza a **OK** tooclose hello **Hozzáadás összetevő** panelen.
1. Válassza ki **OK** tooclose hello **kiválasztott összetevők** panelen.

## <a name="save-azure-resource-manager-template"></a>Azure Resource Manager sablon mentése
Az Azure Resource Manager sablon lehetővé teszi egy deklaratív módon toodefine egy ismételhető központi telepítést. hello lépések azt ismertetik, hogyan toosave hello hello virtuális gép létrehozása Azure Resource Manager sablon.
Ha mentett, használhatja hello Azure Resource Manager sablon túl[új virtuális gépeket az Azure PowerShell telepítése](../azure-resource-manager/resource-group-overview.md#template-deployment).

1. A hello **virtuális gép** panelen válassza **ARM-sablon megtekintése**.
2. A hello **nézet Azure Resource Manager-sablon** panelen, jelölje be hello sablon szövegét.
3. Hello kijelölt szöveg toohello vágólapra másolása.
4. Válassza ki **OK** tooclose hello **Azure Resource Manager sablon megtekintése panel**.
5. Nyisson meg egy szövegszerkesztőt.
6. Illessze be a hello sablon hello vágólapra másolt szöveget.
7. Mentse a fájlt hello későbbi használatra.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

### <a name="next-steps"></a>Következő lépések
* Egyszer hello a virtuális gép létrehozása, csatlakoztathatja toohello virtuális gép kiválasztásával **Connect** hello VM panelen.
* Ismerje meg, hogyan túl[egyéni összetevők létrehozása a DevTest Labs szolgáltatásban virtuális gép](devtest-lab-artifact-author.md).
* Fedezze fel hello [Office DevTest Labs Azure Resource Manager gyorsindítási sablonok](https://github.com/Azure/azure-devtestlab/tree/master/Samples).
