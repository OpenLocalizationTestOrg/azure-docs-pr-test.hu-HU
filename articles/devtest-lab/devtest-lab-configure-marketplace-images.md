---
title: "aaaConfigure Azure piactér lemezkép beállításainak a Azure DevTest Labs szolgáltatásban |} Microsoft Docs"
description: "Mely Azure piactéren elérhető rendszerkép is használható, ha a virtuális gép létrehozása az Azure DevTest Labs konfigurálása"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 804c6af2-17e9-4320-af3a-f454bd398379
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/25/2016
ms.author: tarcher
ms.openlocfilehash: bb4b7f1c0cbe967bee724f7ee20f64f8c4ea58ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-azure-marketplace-image-settings-in-azure-devtest-labs"></a>Az Azure DevTest Labs Azure piactér kép beállításainak konfigurálása
DevTest Labs alapján attól függően, hogy hogyan konfigurálta az Azure piactér képek toobe a tesztkörnyezetben használt Azure piactéren elérhető rendszerkép létrehozása virtuális gépek támogatja. Ez a cikk bemutatja, hogyan toospecify, amely, ha bármely, az Azure piactéren elérhető rendszerkép lehet egy, amikor a virtuális gépek létrehozásakor használt. Ez biztosítja, hogy csak rendelkezik hozzáférési toohello piactéren elérhető rendszerkép van szükségük. 

## <a name="select-which-azure-marketplace-images-are-allowed-when-creating-a-vm"></a>Válassza ki, melyik Azure piactéren elérhető rendszerkép vannak engedélyezve, ha a virtuális gép létrehozása
1. Jelentkezzen be toohello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Válassza ki **több szolgáltatások**, majd válassza ki **DevTest Labs** hello listából.
3. Labs hello listában jelölje ki hello kívánt labor. 
4. Hello labor paneljén válassza **konfigurációs és házirendek**.
5. A tesztlabor a **konfigurációs és házirendek** részen **virtuális gép körrel**, jelölje be **piactéren elérhető rendszerkép**.
6. Adja meg, hogy az összes hello minősített Azure piactér képek toobe használható egy új virtuális gép alapjaként. Ha **Igen**, majd az összes hello Azure piactéren elérhető rendszerkép összes hello következő feltételeknek megfelelő engedélyezettek hello laborban:
   
   * hello lemezképet hoz létre egy virtuális, **és**
   * hello lemezképet használja az Azure Resource Manager tooprovision virtuális gépek, **és**
   * hello lemezkép nincs szükség, egy extra licenccsomagban megvásárlásáról
     
    Ha azt szeretné, hogy nincsenek képek toobe engedélyezett, vagy azt szeretné, hogy mely képek is használható, jelölje be toospecify **nem**.
     
     ![Beállítás tooallow összes Piactéri lemezképet toobe használt alap képként virtuális gépekhez](./media/devtest-lab-configure-marketplace-images/allow-all-marketplace-images.png)
7. Ha **nem** toohello előző lépést, hello **engedélyezett képek/válassza ki az összes** jelölőnégyzet engedélyezve van. 
   Ezzel a kapcsolóval együtt hello Keresés mezőbe tooquickly válassza ki, vagy hello listán megjelenített összes hello elemek kijelölésének megszüntetése.
   * Válassza ki a kívánt tooallow virtuális gépek létrehozására egyenként mindegyik lemezkép operációs rendszerének megfelelő jelölőnégyzet bejelölésével hello Azure piactéren elérhető rendszerkép.
   * Válassza ki semmi a hello lista Ha nem szeretné, hogy tooallow bármely Azure piactér képek toobe hello a tesztkörnyezetben használt.
   
    ![Megadhatja a virtuális gépek mely Azure piactéren elérhető rendszerkép használható alap képként](./media/devtest-lab-configure-marketplace-images/select-marketplace-images.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Következő lépések
Miután konfigurálta, hogy Azure piactéren elérhető rendszerkép engedélyezettek, ha a virtuális gép létrehozása, hello tovább túl van-e[VM tooyour labor hozzáadása](devtest-lab-add-vm-with-artifacts.md).

