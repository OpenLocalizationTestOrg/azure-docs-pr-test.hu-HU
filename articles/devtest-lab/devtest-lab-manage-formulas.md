---
title: "a virtuális gépek Azure DevTest Labs toocreate aaaManage képletek |} Microsoft Docs"
description: "Megtudhatja, hogyan tooupdate és eltávolítás Azure DevTest Labs képlet"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 841dd95a-657f-4d80-ba26-59a9b5104fe4
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/07/2017
ms.author: tarcher
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 855debe46f3b70cc45ea5d55869663b64e225124
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-devtest-labs-formulas"></a>Azure DevTest Labs képletek kezelése

[!INCLUDE [devtest-lab-formula-definition](../../includes/devtest-lab-formula-definition.md)]

Ez a cikk bemutatja, hogyan toocreate base (egyéni lemezképet, Piactéri lemezképhez vagy egy másik képlet) vagy a meglévő virtuális képletet. Ez a cikk is végigvezeti Önt meglévő képletek kezelése.

## <a name="create-a-formula"></a>Létrehozhat egy képletet
Bárki, aki DevTest Labs *felhasználók* engedélyek képes toocreate virtuális gépek képlettel alapjaként. Két módon toocreate képletek: 

* A base - használni, ha szeretné toodefine hello képlet összes hello jellemzőit.
* Az egy meglévő Virtuálisgép - tesztkörnyezet használja, ha azt szeretné, hogy toocreate képlet beállításai alapján a hello egy meglévő virtuális gép.

Felhasználók és engedélyek hozzáadásával kapcsolatos további információkért lásd: [tulajdonosa és a felhasználók hozzáadása az Azure DevTest Labs](./devtest-lab-add-devtest-user.md).

### <a name="create-a-formula-from-a-base"></a>A base létrehozhat egy képletet
a lépéseket követve hello hello folyamatot, amely egy egyéni lemezképet, Piactéri lemezképhez vagy egy másik képlet képletek létrehozását ismerteti.

1. Jelentkezzen be toohello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).

2. Válassza ki **több szolgáltatások**, majd válassza ki **DevTest Labs** hello listából.

3. Labs hello listában jelölje ki hello kívánt labor.  

4. Hello labor paneljén válassza **képletek (újrafelhasználható körrel)**.
   
    ![Képletadat menü](./media/devtest-lab-create-formulas/lab-settings-formulas.png)

5. A hello **képletek** panelen válassza **+ Hozzáadás**.
   
    ![A képlet hozzáadása](./media/devtest-lab-create-formulas/add-formula.png)

6. A hello **base válasszon** panelen, jelölje be hello alapja (egyéni lemezképet, Piactéri lemezképhez vagy képlet), amelyből el kívánja toocreate hello képlet.
   
    ![Kiinduló lista](./media/devtest-lab-create-formulas/base-list.png)

7. A hello **képletet** panelen adja meg a következő értékek hello:
   
    * **Képlet neve** -adja meg a képlet nevét. Ez az érték alap képek hello listája jelenik meg a virtuális gépek létrehozásakor. Írja be azt, és nem érvényes, ha egy üzenet azt jelzi-e egy érvényes nevet hello követelményei hello nevének érvényességét.
    * **Leírás** -adja meg a képlet beszédes leírást. Ezt az értéket a virtuális gép létrehozása esetén hello képlet helyi menüjéből érhető el.
    * **Felhasználónév** -adjon meg egy felhasználónevet, amely rendszergazdai jogosultságokkal engedélyezett.
    * **Jelszó** – írja be - vagy hello legördülő menüből válassza - hello titkos kulcs (jelszó), amelyet az toouse hello megadott felhasználó társított egy értéket. Hello titkok kapcsolatos további információkért lásd: [Azure DevTest Labs: titkos tárolójának](https://azure.microsoft.com/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store/).
    * **Virtuális gép lemeztípus** : Adja meg vagy HDD (merevlemez-meghajtóra), vagy mely tárolási lemez típusa (SSD-meghajtóra) SSD tooindicate hello virtuális gépek üzembe helyezve az alapjául szolgáló lemezképhez használata esetén engedélyezett.
    * ** Virtuális gép mérete ** – hello processzormag, a RAM memória méretét és a merevlemez méretének hello hello VM toocreate meghatározó előre definiált hello elemek közül. 
    * **Az összetevők** -válassza tooopen hello **vegye fel az összetevők** , amelyben akkor válassza ki, és beállíthatja, amelyet az tooadd toohello alapjául szolgáló lemezképhez hello összetevők paneljén. Az összetevők kapcsolatos további információkért lásd: [kezelése Virtuálisgép-összetevők a Azure DevTest Labs szolgáltatásban](./devtest-lab-add-vm-with-artifacts.md).
    * **Speciális beállítások** -válassza tooopen hello **speciális** hello a következő beállítások konfigurálására szolgáló panel:
        * **Virtuális hálózati** -adja meg a hello virtuális hálózat szükséges.
        * **Alhálózati** -szükséges hello alhálózatot adjon meg.    
        * **IP-címkonfigurációt** -adja meg, ha hello Public, Private vagy megosztott IP-címeket. További információ a megosztott IP-címek: [megértése megosztott IP-címek az Azure DevTest Labs](./devtest-lab-shared-ip.md).
        * **Ellenőrizze a gép claimable** -és a gép "claimable" azt jelenti, hogy a rendszer nem hozzárendel tulajdonjoga hello létrehozása során. Ehelyett a labor felhasználók fognak képes tootake tulajdonosi ("jogcím") hello gép hello labor panelen.     
    * **Kép** – Ez a mező neve hello alapjául szolgáló lemezképhez kiválasztott hello előző panelen. 
     
       ![Képlet létrehozása](./media/devtest-lab-create-formulas/create-formula.png)

8. Válassza ki **létrehozása** toocreate hello képlet.

9. Hello képlet létrehozásakor hello hello listája megjeleníti **képletek** panelen.

### <a name="create-a-formula-from-a-vm"></a>Létrehozhat egy képletet a virtuális gépről
hello következő lépések végigvezetik hello egy meglévő virtuális Gépen alapuló képlet létrehozásának folyamatán. 

> [!NOTE]
> egy virtuális géptől képlet toocreate, hello VM kell létrehozni után, 2016. március 30.. 
> 
> 

1. Jelentkezzen be toohello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Válassza ki **több szolgáltatások**, majd válassza ki **DevTest Labs** hello listából.
3. Labs hello listában jelölje ki hello kívánt labor.  
4. A hello labor **áttekintése** panel, amelyen toocreate hello képlet kívánja válassza hello VM.
   
    ![Labs virtuális gépek](./media/devtest-lab-create-formulas/my-vms.png)
5. Hello virtuális gép paneljén válassza **képletet (újrafelhasználható alap)**.
   
    ![Képlet létrehozása](./media/devtest-lab-create-formulas/create-formula-menu.png)
6. A hello **képletet** panelen adjon meg egy **neve** és **leírása** az új képlet.
   
    ![Képletadat panel létrehozása](./media/devtest-lab-create-formulas/create-formula-blade.png)
7. Válassza ki **OK** toocreate hello képlet.

## <a name="modify-a-formula"></a>A képlet módosítása
toomodify képlet, kövesse az alábbi lépéseket:

1. Jelentkezzen be toohello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Válassza ki **több szolgáltatások**, majd válassza ki **DevTest Labs** hello listából.
3. Labs hello listában jelölje ki hello kívánt labor.  
4. Hello labor paneljén válassza **képletek (újrafelhasználható körrel)**.
   
    ![Képletadat menü](./media/devtest-lab-manage-formulas/lab-settings-formulas.png)
5. A hello **labor képletek** panelen, jelölje be hello képlet toomodify kívánja.
6. A hello **képlet frissítése** panelen szükséges hello Szerkesztés, és válassza ki **frissítés**.

## <a name="delete-a-formula"></a>Képlet törlése
toodelete képlet, kövesse az alábbi lépéseket:

1. Jelentkezzen be toohello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Válassza ki **több szolgáltatások**, majd válassza ki **DevTest Labs** hello listából.
3. Labs hello listában jelölje ki hello kívánt labor.  
4. A tesztkörnyezet hello **beállítások** panelen válassza **képletek**.
   
    ![Képletadat menü](./media/devtest-lab-manage-formulas/lab-settings-formulas.png)
5. A hello **labor képletek** panelen, jelölje be hello három pont toohello sarkában hello képlet toodelete kívánja.
   
    ![Képletadat menü](./media/devtest-lab-manage-formulas/lab-formulas-blade.png)
6. Hello képlet helyi menüben, válassza ki a **törlése**.
   
    ![Képletadat helyi menü](./media/devtest-lab-manage-formulas/formula-delete-context-menu.png)
7. Válassza ki **Igen** toohello törlési megerősítés.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Kapcsolódó blogbejegyzések
* [Egyéni lemezképek vagy képletek?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)

## <a name="next-steps"></a>Következő lépések
Miután létrehozta a képlet használható virtuális gép létrehozásakor, hello tovább túl van-e[VM tooyour labor hozzáadása](devtest-lab-add-vm-with-artifacts.md).

