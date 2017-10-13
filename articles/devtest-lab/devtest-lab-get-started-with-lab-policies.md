---
title: "Az Azure DevTest Labs alapvető tesztlabor-házirendek kezeléséhez |} Microsoft Docs"
description: "Útmutató az alapvető házirendeket (beállítások) labor némelyike a DevTest Labs szolgáltatásban"
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
ms.date: 06/29/2017
ms.author: tarcher
ms.openlocfilehash: ed35d081b191ec41ed9e5970515057a4715c0d59
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="manage-basic-policies-for-a-lab-in-azure-devtest-labs"></a>Az Azure DevTest Labs szolgáltatásban a labor alapvető házirendjeinek kezelése

Azure DevTest Labs segítségével a labs a pazarlás minimalizálásához által az egyes labor (beállítások) házirendek kezelése és költségek szabályozásához. Ebben a cikkben megkezdése házirendek beállítása két legfontosabb házirendek hogyan - virtuális gépek (VM) létrehozott vagy egy felhasználó által igényelt számának korlátozása, és az automatikus rendszerleállítást konfigurálásával. Minden tesztkörnyezeti házirend tájékoztatásért tekintse meg a cikk megtekintéséhez [labor házirendeket definiálhat az Azure DevTest Labs](devtest-lab-set-lab-policy.md).  

## <a name="accessing-a-labs-policies-in-azure-devtest-labs"></a>Egy tesztlabor házirendek az Azure DevTest Labs elérése
A következő lépések végigvezetik a Azure DevTest Labs szolgáltatásban labor házirendek beállítása:

Megtekintése (és módosítása) a házirendek egy tesztkörnyezetet, kövesse az alábbi lépéseket:

1. Jelentkezzen be az [Azure Portalra](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Válassza a **További szolgáltatások**, majd a **DevTest Labs** elemet a listából.

1. Válassza ki a kívánt labor labs listájának megtekintéséhez.   

1. Válassza ki **konfigurációs és házirendek**.

    ![Szabályzat-beállítások panelen](./media/devtest-lab-set-lab-policy/policies-menu.png)

1. A **konfigurációs és házirendek** panel megadható beállítások menüt tartalmaz. Ez a cikk foglalkozik beállításait csak **virtuális gépek száma felhasználónként** és **automatikus leállítási**. A fennmaradó beállításaival kapcsolatos további tudnivalókért lásd: [egy Azure DevTest Labs szolgáltatásban található, amikor az összes házirend kezeléséhez](./devtest-lab-set-lab-policy.md). 
   
## <a name="set-virtual-machines-per-user"></a>Beállítása virtuális gépek száma felhasználónként
A házirend **virtuális gépek száma felhasználónként** megadhatja az egyes felhasználók által létrehozott virtuális gépek maximális számát. Ha egy felhasználó megpróbál létrehozásához vagy a virtuális gépek jogcímek a megadott felhasználói korlátot teljesülésekor, egy hibaüzenet arra utalnak, hogy a virtuális gép nem létrehozott/igényt. 

1. A tesztlabor a **konfigurációs és házirendek** menüjében válassza **virtuális gépek száma felhasználónként**.
   
    ![Virtuális gépek száma felhasználónként](./media/devtest-lab-set-lab-policy/max-vms-per-user.png)

1. Válassza ki **Igen** korlátozza a virtuális gépek száma felhasználónként. Ha nem szeretné, hogy a virtuális gépek száma felhasználónként korlátozni, válassza ki a **nem**. Ha **Igen**, adjon meg egy numerikus érték, amely a létrehozott vagy a felhasználó által igényelt virtuális gépek maximális számát. 

1. Válassza ki **Igen** SSD (SSD lemezt) használó virtuális gépek számát. Ha nem szeretné, hogy a virtuális gépek, amelyek is SSD használni, válassza a számának korlátozásához **nem**. Ha **Igen**, adjon meg egy értéket, amely SSD segítségével hozhatók létre virtuális gépek maximális száma. 

1. Kattintson a **Mentés** gombra.

## <a name="set-auto-shutdown"></a>Készlet automatikus rendszerleállítást
Az automatikus rendszerleállítást a szabályzattal meggyőződhetnek labor pazarlás minimalizálásához azáltal, hogy adja meg a labor virtuális gépek leállítása idejét.

1. A tesztlabor a **konfigurációs és házirendek** panelen válassza **automatikus leállítási**.
   
    ![Automatikus leállítási](./media/devtest-lab-set-lab-policy/auto-shutdown.png)

1. Válassza ki **a** ahhoz, hogy ezt a házirendet, és **ki** le kell tiltani.

1. Ha engedélyezi ezt a házirendet, adja meg az időt (és időzóna) leállítása az összes virtuális gép az aktuális tesztkörnyezetben.

1. Adja meg **Igen** vagy **nem** beállítás értesítés küldése előtt a megadott automatikus rendszerleállítást idő 15 perc. Ha megad **Igen**, írja be a webhook URL-végpontjának az értesítések fogadásához. További információ a webhookok: [webhook vagy API Azure-függvény létrehozása](../azure-functions/functions-create-a-web-hook-or-api-function.md). 

1. Kattintson a **Mentés** gombra.

    Alapértelmezés szerint engedélyezve van, ez a házirend vonatkozik az összes virtuális gépen az aktuális tesztkörnyezetben. Távolítsa el ezt a beállítást egy adott virtuális géptől, nyissa meg a virtuális gép panelt, és módosítsa a **automatikus leállítási** beállítás 

## <a name="set-auto-start"></a>Készlet automatikus indítás
Az automatikus indítási házirend lehetővé teszi, hogy adja meg, amikor el kell indítani a virtuális gépeket az aktuális tesztkörnyezetben.  

1. A tesztlabor a **konfigurációs és házirendek** panelen válassza **automatikusan elinduló**.
   
    ![Automatikus indítás](./media/devtest-lab-set-lab-policy/auto-start.png)

2. Válassza ki **a** ahhoz, hogy ezt a házirendet, és **ki** le kell tiltani.

3. Ha engedélyezi ezt a házirendet, adja meg az ütemezett kezdési időpontban, időzóna és a napokat a hét, amelyre a idő vonatkozik. 

4. Kattintson a **Mentés** gombra.

    Az engedélyezést követően a házirend nem automatikusan érvényben van a virtuális gépek az aktuális tesztkörnyezetben. Alkalmazza ezt a beállítást egy adott virtuális géphez, nyissa meg a virtuális gép panelt, és módosítsa a **automatikusan elinduló** beállítás 

## <a name="next-steps"></a>Következő lépések

- [Labor házirendeket definiálhat az Azure DevTest Labs](devtest-lab-set-lab-policy.md) -megtudhatja, hogyan egyéb tesztkörnyezeti házirendek módosítása 
