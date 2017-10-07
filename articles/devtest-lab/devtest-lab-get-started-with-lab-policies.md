---
title: "aaaManage alapvető tesztlabor házirendek az Azure DevTest Labs szolgáltatásban |} Microsoft Docs"
description: "Megtudhatja, hogyan tooset néhány hello alapvető házirendek (beállítások) egy, amikor a DevTest Labs szolgáltatásban"
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
ms.openlocfilehash: 792c0d19cfe73964be9c03d3de7751a868fd86e8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-basic-policies-for-a-lab-in-azure-devtest-labs"></a>Az Azure DevTest Labs szolgáltatásban a labor alapvető házirendjeinek kezelése

Az Azure DevTest Labs lehetővé teszi toocontrol költség, és a fejlesztőlaborokban lévő pazarlás minimalizálásához (beállítások) házirendek kezelése által az egyes labor. Ebben a cikkben megkezdése házirendek hogyan tooset két hello legfontosabb házirendek - korlátozása hello száma tanulás által létrehozott vagy egyetlen felhasználói fiókot, és konfigurálása automatikus rendszerleállítást által igényelt virtuális gépek (VM). tooview hogyan tooset minden tesztkörnyezeti házirend hello cikke [labor házirendeket definiálhat az Azure DevTest Labs](devtest-lab-set-lab-policy.md).  

## <a name="accessing-a-labs-policies-in-azure-devtest-labs"></a>Egy tesztlabor házirendek az Azure DevTest Labs elérése
hello következő lépések végigvezetik a Azure DevTest Labs szolgáltatásban labor házirendek beállítása:

egy tesztlabor tooview (és módosítása) hello szabályzatok kövesse az alábbi lépéseket:

1. Jelentkezzen be toohello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Válassza ki **további szolgáltatások**, majd válassza ki **DevTest Labs** hello listából.

1. Labs hello listában jelölje ki hello kívánt labor.   

1. Válassza ki **konfigurációs és házirendek**.

    ![Szabályzat-beállítások panelen](./media/devtest-lab-set-lab-policy/policies-menu.png)

1. Hello **konfigurációs és házirendek** panel megadható beállítások menüt tartalmaz. Ez a cikk foglalkozik csak hello beállításainak **virtuális gépek száma felhasználónként** és **automatikus leállítási**. toolearn fennmaradó beállításokat, hello kapcsolatban lásd: [egy Azure DevTest Labs szolgáltatásban található, amikor az összes házirend kezeléséhez](./devtest-lab-set-lab-policy.md). 
   
## <a name="set-virtual-machines-per-user"></a>Beállítása virtuális gépek száma felhasználónként
a házirend hello **virtuális gépek száma felhasználónként** lehetővé teszi a toospecify hello maximális számát egy adott felhasználó által létrehozott virtuális gépek. Akkor a toocreate vagy a jogcím egy virtuális gép hello megadott felhasználói korlátot teljesülésekor, hibaüzenet jelzi, hogy virtuális gép nem hozható létre /, hello. 

1. A hello labor **konfigurációs és házirendek** menüjében válassza **virtuális gépek száma felhasználónként**.
   
    ![Virtuális gépek száma felhasználónként](./media/devtest-lab-set-lab-policy/max-vms-per-user.png)

1. Válassza ki **Igen** toolimit hello virtuális gépek száma felhasználónként. Ha nem szeretné, hogy toolimit hello virtuális gépek száma felhasználónként, válassza ki a **nem**. Ha **Igen**, adjon meg egy numerikus érték, amely hello létrehozott vagy a felhasználó által igényelt virtuális gépek maximális számát. 

1. Válassza ki **Igen** SSD (SSD lemezt) használó virtuális gépek toolimit hello száma. Ha nem szeretné, hogy a virtuális gépek által használható SSD toolimit hello száma, válassza ki a **nem**. Ha **Igen**, adjon meg egy értéket hello SSD segítségével hozhatók létre virtuális gépek maximális számát. 

1. Kattintson a **Mentés** gombra.

## <a name="set-auto-shutdown"></a>Készlet automatikus rendszerleállítást
hello automatikus leállítási házirendet lehetővé teszik, hogy a labor virtuális gépek leállítása toospecify hello idő hulladék toominimize tesztkörnyezet segítségével.

1. A hello labor **konfigurációs és házirendek** panelen válassza **automatikus leállítási**.
   
    ![Automatikus leállítási](./media/devtest-lab-set-lab-policy/auto-shutdown.png)

1. Válassza ki **a** tooenable ezt a házirendet, és **ki** toodisable azt.

1. Ha engedélyezi ezt a házirendet, adja meg a hello idő (és időzóna) tooshut minden virtuális gép le hello aktuális tesztkörnyezetben.

1. Adja meg **Igen** vagy **nem** hello beállítás toosend egy értesítési 15 perccel korábbi toohello megadva automatikus leállítási ideje. Ha megad **Igen**, írja be a webhook URL-cím végpont tooreceive hello értesítést. További információ a webhookok: [webhook vagy API Azure-függvény létrehozása](../azure-functions/functions-create-a-web-hook-or-api-function.md). 

1. Kattintson a **Mentés** gombra.

    Alapértelmezés szerint egyszer engedélyezve van ez a házirend érvényes tooall virtuális gépek hello aktuális tesztkörnyezetben. tooremove ezt a beállítást egy adott virtuális gépről, nyissa meg a hello VM panelt, és módosítsa a **automatikus leállítási** beállítás 

## <a name="set-auto-start"></a>Készlet automatikus indítás
hello automatikusan elinduló házirend lehetővé teszi toospecify amikor hello aktuális tesztkörnyezetben lévő virtuális gépek hello el kell indítani.  

1. A hello labor **konfigurációs és házirendek** panelen válassza **automatikusan elinduló**.
   
    ![Automatikus indítás](./media/devtest-lab-set-lab-policy/auto-start.png)

2. Válassza ki **a** tooenable ezt a házirendet, és **ki** toodisable azt.

3. Ha engedélyezi ezt a házirendet, adja meg a hello ütemezett kezdési ideje, időzóna és hello napot hello mely hello idő vonatkozik. 

4. Kattintson a **Mentés** gombra.

    Engedélyezve van, ez a házirend nincs automatikusan alkalmazott tooany virtuális gépek hello aktuális tesztkörnyezetben. tooapply Ez a beállítás tooa speciális virtuális Gépet, a nyitott hello VM panel és a módosítás a **automatikusan elinduló** beállítás 

## <a name="next-steps"></a>Következő lépések

- [Labor házirendeket definiálhat az Azure DevTest Labs](devtest-lab-set-lab-policy.md) -megtudhatja, hogyan toomodify egyéb tesztkörnyezeti házirendek 
