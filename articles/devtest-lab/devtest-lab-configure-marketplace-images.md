---
title: "Azure piactér kép beállításainak konfigurálása az Azure DevTest Labs szolgáltatásban |} Microsoft Docs"
description: "Mely Azure piactéren elérhető rendszerkép is használható, ha a virtuális gép létrehozása az Azure DevTest Labs konfigurálása"
services: devtest-lab,virtual-machines
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: 804c6af2-17e9-4320-af3a-f454bd398379
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/25/2016
ms.author: v-craic
ms.openlocfilehash: a3b52bb8db0bcd46badb15d4bc65b85977faaadc
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/02/2018
---
# <a name="configure-azure-marketplace-image-settings-in-azure-devtest-labs"></a>Az Azure DevTest Labs Azure piactér kép beállításainak konfigurálása
DevTest Labs létrehozása virtuális gépek Azure piactéren elérhető rendszerkép attól függően, hogy hogyan konfigurálta az Azure piactéren elérhető rendszerkép alapján használhatók a labor támogatja. Ez a cikk bemutatja, hogyan határozhatja meg, ha bármely, az Azure piactéren elérhető rendszerkép lehet egy, amikor a virtuális gépek létrehozásakor használt. Ez biztosítja, hogy a csoport csak fér hozzá a piactéren elérhető rendszerkép van szükségük. 

## <a name="select-which-azure-marketplace-images-are-allowed-when-creating-a-vm"></a>Válassza ki, melyik Azure piactéren elérhető rendszerkép vannak engedélyezve, ha a virtuális gép létrehozása
1. Jelentkezzen be az [Azure Portalra](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Válassza ki **több szolgáltatások**, majd válassza ki **DevTest Labs** a listából.
3. Válassza ki a kívánt labor labs listájának megtekintéséhez. 
4. A labor paneljén válassza **konfigurációs és házirendek**.
5. A tesztlabor a **konfigurációs és házirendek** részen **virtuális gép körrel**, jelölje be **piactéren elérhető rendszerkép**.
6. Adja meg, hogy minden a minősített Azure piactéren elérhető rendszerkép alapjaként egy új virtuális gép számára elérhető legyen. Ha **Igen**, majd az Azure piactéren lemezképeket, hogy megfelel a következő feltételek engedélyezettek a laborban:
   
   * A kép létrehoz egy virtuális, **és**
   * A kép Azure Resource Managert használja a virtuális gépek, a rendelkezésre **és**
   * A kép nincs szükség, egy extra licenccsomagban megvásárlásáról
     
    Ha azt szeretné, hogy a kép nem számára engedélyezett, vagy meg szeretné határozni mely képek is használható, jelölje be **nem**.
     
     ![A beállítást, minden piactéren elérhető rendszerkép virtuális gépek portbesorolása alap képként](./media/devtest-lab-configure-marketplace-images/allow-all-marketplace-images.png)
7. Ha **nem** az előző lépésben a **engedélyezett képek/válassza ki az összes** jelölőnégyzet engedélyezve van. 
   Segítségével ezt a beállítást a keresőmezőbe együtt gyorsan válassza ki, vagy kapcsolja ki a listában megjelenített összes elemet.
   * Válassza ki az Azure piactéren elérhető rendszerkép szeretné engedélyezni a virtuális gép létrehozásához egyenként mindegyik lemezkép operációs rendszerének megfelelő jelölőnégyzet bejelölésével.
   * Válassza ki semmi a listából, ha nem kívánja a laborban használandó bármely Azure piactéren elérhető rendszerkép engedélyezése.
   
    ![Megadhatja a virtuális gépek mely Azure piactéren elérhető rendszerkép használható alap képként](./media/devtest-lab-configure-marketplace-images/select-marketplace-images.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>További lépések
Miután konfigurálta, hogy Azure piactéren elérhető rendszerkép engedélyezettek, ha a virtuális gép létrehozása, a következő lépés, hogy [a virtuális gépek hozzáadása a labor](devtest-lab-add-vm.md).

