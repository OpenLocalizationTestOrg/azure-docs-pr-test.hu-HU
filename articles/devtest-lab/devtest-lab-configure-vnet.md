---
title: "a Azure DevTest Labs szolgáltatásban virtuális hálózat aaaConfigure |} Microsoft Docs"
description: "Megtudhatja, hogyan tooconfigure egy meglévő virtuális hálózat és az alhálózatot, és felhasználja az Azure DevTest Labs szolgáltatásban virtuális gépen"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 6cda99c2-b87e-4047-90a0-5df10d8e9e14
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/16/2017
ms.author: tarcher
ms.openlocfilehash: a11ce8315e3c540e44aeacc9c5ee3dde014d4621
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-virtual-network-in-azure-devtest-labs"></a>A Azure DevTest Labs szolgáltatásban virtuális hálózat konfigurálása
Hello a cikkben leírtak szerint [adja hozzá a virtuális gép és az összetevők tooa labor](devtest-lab-add-vm-with-artifacts.md), egy tesztkörnyezetben, a virtuális gépek létrehozásakor megadhatja, hogy egy konfigurált virtuális hálózati. Mindezt egy például az is, ha tooaccess van szüksége a virtuális gépek használata a vállalati hálózat erőforrások hello virtuális hálózati telephelyek közötti VPN és ExpressRoute konfigurált. hello következő szakaszok bemutatják, hogyan tooadd be a labor virtuális hálózati beállításait, hogy elérhető toochoose virtuális gépek létrehozásakor a meglévő virtuális hálózatot.

## <a name="configure-a-virtual-network-for-a-lab-using-hello-azure-portal"></a>Virtuális hálózat hello Azure-portál használatával labor beállítása
hello következő lépések végigvezetik hozzáadása egy meglévő virtuális hálózat (és alhálózati) tooa labor, hogy a virtuális gép létrehozásakor a hello használható azonos labor. 

1. Jelentkezzen be toohello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Válassza ki **több szolgáltatások**, majd válassza ki **DevTest Labs** hello listából.
3. Labs hello listában jelölje ki hello kívánt labor. 
4. Hello labor paneljén válassza **konfigurációs**.
5. A hello labor **konfigurációs** panelen válassza **virtuális hálózatok**.
6. A hello **virtuális hálózatok** panelen konfigurált hello aktuális tesztkörnyezetben, valamint a hello alapértelmezett virtuális hálózat a tesztkörnyezethez létrehozott virtuális hálózatok listájának megtekintéséhez. 
7. Válassza ki **+ Hozzáadás**.
   
    ![Egy meglévő virtuális hálózat tooyour labor hozzáadása](./media/devtest-lab-configure-vnet/lab-settings-vnet-add.png)
8. A hello **virtuális hálózati** panelen válassza **[Select virtuális hálózati]**.
   
    ![Válassza ki a meglévő virtuális hálózat](./media/devtest-lab-configure-vnet/lab-settings-vnets-vnet1.png)
9. A hello **válasszon virtuális hálózati** panelen, jelölje be hello kívánt virtuális hálózat. hello panelen látható minden hello virtuális hálózaton vannak a hello azonos hello előfizetés hello lab-régiót.  
10. Miután kiválasztott egy virtuális hálózatot, amelyről toohello **virtuális hálózati** hello alhálózati hello listában hello hello panel alsó részén kattintson.

    ![Alhálózati listája](./media/devtest-lab-configure-vnet/lab-settings-vnets-vnet2.png)
    
    hello labor alhálózati panel jelenik meg.

    ![Labor alhálózati panel](./media/devtest-lab-configure-vnet/lab-subnet.png)

11. Adjon meg egy **tesztkörnyezet alhálózatának nevét**.
12. Válasszon egy virtuális gép létrehozása, a tesztkörnyezetben használt alhálózati toobe tooallow **használja a virtuális gépek létrehozásához**.
13. tooenable egy [nyilvános IP-cím megosztott](devtest-lab-shared-ip.md), jelölje be **engedélyezése megosztott nyilvános IP-cím**.
14. Válassza ki a nyilvános IP-címek tooallow az alhálózat, **nyilvános IP-létrehozásának engedélyezése**.
15. A hello **virtuális gépek maximális száma felhasználónként** mezőben adja meg az egyes alhálózatokon felhasználónkénti maximális virtuális gépek hello. Ha azt szeretné, hogy a virtuális gépek korlátlan számú, hagyja üresen ezt a mezőt.
16. Válassza ki **OK** tooclose hello labor alhálózati panelen.
17. Válassza ki **mentése** tooclose hello virtuális hálózat panel.
18. Most, hogy hello virtuális hálózat van konfigurálva, akkor jelölhető ki a virtuális gépek létrehozásakor. 
    toosee hogyan toocreate a virtuális gépek és adjon meg egy virtuális hálózatot, tekintse meg a toohello cikk [adja hozzá a virtuális gép és az összetevők tooa labor](devtest-lab-add-vm-with-artifacts.md). 

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Következő lépések
Hello szükséges virtuális hálózati tooyour labor hozzáadása után hello tovább túl van-e[VM tooyour labor hozzáadása](devtest-lab-add-vm-with-artifacts.md).

