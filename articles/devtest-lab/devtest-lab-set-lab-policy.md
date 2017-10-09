---
title: "aaaManage labor házirendek az Azure DevTest Labs szolgáltatásban |} Microsoft Docs"
description: "Megtudhatja, hogyan toodefine labor házirendek, például a virtuális gép méretét, maximális virtuális gépek minden felhasználóhoz, és a Leállítás automation."
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 7756aa64-49ca-45a0-9f90-0fd101c7be85
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/13/2017
ms.author: tarcher
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 351b3645a1fd729455884e5d177877c2986bd853
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-all-policies-for-a-lab-in-azure-devtest-labs"></a>Egy Azure DevTest Labs szolgáltatásban található, amikor az összes házirend kezeléséhez

Az Azure DevTest Labs lehetővé teszi a költségek szabályozásához, és a fejlesztőlaborokban lévő pazarlás minimalizálásához (beállítások) házirendek kezelése által az egyes labor. Ez a cikk részletes részletesen ismerteti hogyan tooset minden házirend.  

## <a name="set-allowed-virtual-machine-sizes"></a>Virtuális gépek méretét engedélyezett beállítása
hello beállítás hello házirend engedélyezett Virtuálisgép-méretek segít toominimize labor hulladék azáltal, hogy mely Virtuálisgép-méretek engedélyezettek hello labor toospecify engedélyezi. Ha ez a házirend aktív, csak Virtuálisgép-méretek a listából lehet használt toocreate virtuális gépeket.

1. A hello labor **konfigurációs és házirendek** panelen válassza **engedélyezett virtuális gépek méretét**.
   
    ![Engedélyezett virtuális gépek méretét](./media/devtest-lab-set-lab-policy/allowed-vm-sizes.png)

1. Válassza ki **a** tooenable ezt a házirendet, és **ki** toodisable azt.

1. Ha engedélyezi ezt a házirendet, válassza ki a tesztkörnyezetben hozható létre egy vagy több Virtuálisgép-méretek.

1. Kattintson a **Mentés** gombra.

## <a name="set-virtual-machines-per-user"></a>Beállítása virtuális gépek száma felhasználónként
a házirend hello **virtuális gépek száma felhasználónként** lehetővé teszi a toospecify hello maximális számát egy adott felhasználó által létrehozott virtuális gépek. Akkor a toocreate vagy a jogcím egy virtuális gép hello megadott felhasználói korlátot teljesülésekor, hibaüzenet jelzi, hogy virtuális gép nem hozható létre /, hello. 

1. A hello labor **konfigurációs és házirendek** menüjében válassza **virtuális gépek száma felhasználónként**.
   
    ![Virtuális gépek száma felhasználónként](./media/devtest-lab-set-lab-policy/max-vms-per-user.png)

1. Válassza ki **Igen** toolimit hello virtuális gépek száma felhasználónként. Ha nem szeretné, hogy toolimit hello virtuális gépek száma felhasználónként, válassza ki a **nem**. Ha **Igen**, adjon meg egy numerikus érték, amely hello létrehozott vagy a felhasználó által igényelt virtuális gépek maximális számát. 

1. Válassza ki **Igen** SSD (SSD lemezt) használó virtuális gépek toolimit hello száma. Ha nem szeretné, hogy a virtuális gépek által használható SSD toolimit hello száma, válassza ki a **nem**. Ha **Igen**, adjon meg egy értéket hello SSD segítségével hozhatók létre virtuális gépek maximális számát. 

1. Kattintson a **Mentés** gombra.

## <a name="set-virtual-machines-per-lab"></a>Beállítása virtuális gépek száma tesztkörnyezetenként
a házirend hello **virtuális gépek száma tesztkörnyezetenként** lehetővé teszi az aktuális tesztkörnyezetben hello toospecify hello maximális számát a virtuális gépek hozható létre. Akkor a virtuális gép toocreate hello labor korlát teljesülésekor, hibaüzenet jelzi, hogy hello virtuális gép nem hozható létre. 

1. A hello labor **konfigurációs és házirendek** menüjében válassza **virtuális gépek száma tesztkörnyezetenként**.
   
    ![Virtuális gépek száma tesztkörnyezetenként](./media/devtest-lab-set-lab-policy/max-vms-per-lab.png)

1. Válassza ki **Igen** toolimit hello számát a virtuális gépek száma tesztkörnyezetenként. Ha nem szeretné, hogy toolimit hello számát a virtuális gépek száma tesztkörnyezetenként, válassza ki a **nem**. Ha **Igen**, adjon meg egy numerikus érték, amely hello létrehozott vagy a felhasználó által igényelt virtuális gépek maximális számát. 

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

## <a name="set-expiration-date"></a>Lejárat dátuma
Megadhat egy lejárati dátum, [hello virtuális gép létrehozása](devtest-lab-add-vm.md). A **speciális beállítások**, válassza ki a hello naptár ikon toospecify a dátum, amelyen hello a virtuális gép automatikusan törölve lesznek.  Alapértelmezés szerint a virtuális gép hello nem jár.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Következő lépések
Amennyiben már definiálva, és alkalmazza hello különböző VM házirend-beállítások a tesztkörnyezet, az alábbiakban néhány dolgot tootry mellett:

* [Megosztott IP-címek megértéséhez](devtest-lab-shared-ip.md) -ismerteti, hogyan megosztott IP címeket használják a DevTest Labs toominimize hello számot nyilvános IP címek szükséges tooconnect tooyour labor virtuális gépeken.
* [Költség felügyeletének konfigurálásához](devtest-lab-configure-cost-management.md) -bemutatja, hogyan toouse hello **havi becsült Költségtrend** diagram  
  tooview hello az aktuális hónap becsült költség-date és tervezett hello hónap végi költség.
* [Hozzon létre egyéni lemezkép](devtest-lab-create-template.md) – a virtuális gépek létrehozásakor megad egy talál, amely lehet, vagy egy egyéni lemezképet, vagy egy Piactéri lemezképhez. Ez a cikk bemutatja, hogyan toocreate egyéni kép VHD-fájl.
* [Konfigurálja a piactéren elérhető rendszerkép](devtest-lab-configure-marketplace-images.md) – Azure DevTest Labs támogatja az Azure piactéren elérhető rendszerkép alapján virtuális gépek létrehozását. Ez a cikk bemutatja, hogyan toospecify, amely, ha bármely, az Azure piactéren elérhető rendszerkép lehet egy, amikor a virtuális gépek létrehozásakor használt.
* [Hozzon létre egy virtuális Gépet egy tesztkörnyezetben](devtest-lab-add-vm-with-artifacts.md) -bemutatja, hogyan toocreate alapjául szolgáló lemezképhez a virtuális gép (vagy egyéni vagy piactér), és hogyan toowork együtt a virtuális gépen.

