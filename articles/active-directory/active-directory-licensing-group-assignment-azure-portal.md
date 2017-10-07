---
title: Azure Active Directoryban aaaAssign licencek tooa csoport |} Microsoft Docs
description: "Hogyan tooassign licencek segítségével az Azure Active Directory csoport licencelési toousers"
services: active-directory
keywords: "Az Azure AD-licencelés"
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/05/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 148fe1bdd6c7f477a00c1f76bd8fa7d29c7b1f2c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="assign-licenses-toousers-by-group-membership-in-azure-active-directory"></a>Licencek toousers által az Azure Active Directoryban csoporttagság hozzárendelése

Ez a cikk útmutatást nyújt a hozzárendelése a termék licencek tooa csoport számára az Azure Active Directory (Azure AD), és győződjön meg arról, hogy azok még licencelt megfelelően.

Ebben a példában hello bérlő tartalmazza nevű biztonsági csoport **HR-részleg**. Ez a csoport összes tagja hello emberi erőforrások részleg (körülbelül 1000 felhasználó) tartalmazza. Office 365 nagyvállalati E3 csomag tooassign licencek toohello teljes részleg szeretné. Vállalati Yammer-szolgáltatás, amely megtalálható a hello termék hello kell ideiglenesen tiltott, amíg hello részleg kész toostart használja azt. Azt is szeretné toodeploy nagyvállalati mobilitási + biztonsági licencek toohello ugyanazt a felhasználói csoportot.

> [!NOTE]
> Egyes Microsoft-szolgáltatások nem érhetők el az összes helyen. Licenc tooa felhasználói rendelhető, mielőtt hello rendszergazda hello felhasználói rendelkezik toospecify hello használati helyet jelölő tulajdonsághoz.

> A licenc-hozzárendelést nélkül a megadott felhasználási hely összes felhasználója örökli hello könyvtár hello helye. Amennyiben több helyen is rendelkezik felhasználókkal, azt javasoljuk, hogy mindig állítsa használat helye a felhasználó-létrehozási folyamat részeként az Azure AD (pl. keresztül AAD Connect konfigurációjának) – Ez biztosítja a hello licenc-hozzárendelést eredménye mindig megfelelő, és a felhasználók nem kapják meg szolgáltatások a helyeken, amelyek nem engedélyezettek.

## <a name="step-1-assign-hello-required-licenses"></a>1. lépés: Hello szükséges licencek kiosztása

1. Jelentkezzen be toohello [ **Azure-portálon** ](https://portal.azure.com) rendszergazdai fiókkal. toomanage licencek, hello fiók globális rendszergazdai szerepkör vagy felhasználó fiók rendszergazdai jogosultsággal kell rendelkeznie.

2. Válassza ki **további szolgáltatások** a hello bal oldali navigációs panelen, majd válassza ki **Azure Active Directory**. A panel tooFavorites hozzáadhat és rögzítheti toohello portál Irányítópultjára.

3. A hello **Azure Active Directory** panelen válassza **licencek**. Ekkor megnyílik egy panel, ahol látható, és minden licencelhető termékek hello bérlői felügyelhető.

4. A **összes**, válassza ki az Office 365 nagyvállalati E3 csomag és a nagyvállalati mobilitási + biztonsági hello terméknevek kiválasztásával. toostart hello hozzárendelés, jelölje be **hozzárendelése** hello panel hello tetején.

   ![Minden termékek használatára jogosító licenc hozzárendeléséhez](media/active-directory-licensing-group-assignment-azure-portal/all-products-assign.png)

5. A hello **licenc hozzárendelése** panelen kattintson a **felhasználók és csoportok** tooopen hello **felhasználók és csoportok** panelen. Keressen a hello csoportnév *HR-részleg*, válasszon hello csoportot, és akkor lesz kattintva meg arról, hogy tooconfirm **válassza** hello hello panel alsó részén.

   ![Válasszon ki egy csoportot](media/active-directory-licensing-group-assignment-azure-portal/select-a-group.png)

6. A hello **licenc hozzárendelése** panelen kattintson a **hozzárendelés beállításai (választható)**, megjeleníti az összes service csomagokban szereplő hello két termék, amely azt a korábban kiválasztott. Található **Yammer vállalati** kapcsolja **ki** toodisable, amelyeket a szolgáltatás a hello termék licencét. Gombra kattintva erősítse meg **OK** hello aljához **hozzárendelés beállításai**.

   ![Hozzárendelés beállításai](media/active-directory-licensing-group-assignment-azure-portal/assignment-options.png)

7. toocomplete hello hozzárendelés hello **licenc hozzárendelése** panelen kattintson a **hozzárendelése** hello hello panel alsó részén.

8. Megjelenik egy értesítés hello jobb felső sarkában található, amely hello állapotát és hello folyamat eredményeit jeleníti meg. Hello hozzárendelés toohello csoport nem végezhető el (például azért, mert már meglévő licencek hello csoport), kattintson a hello értesítési tooview hello hiba részleteit.

A Microsoft most megadott hello HR-részleg csoport licenc sablonját. Az Azure AD háttérfolyamatként lett elindítva tooprocess csoport összes meglévő tagjára. A kezdeti művelet eltarthat egy ideig, attól függően, hogy hello hello csoport jelenlegi méretét. Hello a következő lépésben azt kell ismertetik, hogyan tooverify hello folyamat befejeződött, és annak megállapítása, hogy további figyelmet szükséges tooresolve problémákat.

> [!NOTE]
> Megkezdheti hello azonos hozzárendelés másik elérési utat: **felhasználók és csoportok** Azure AD-ben. Nyissa meg túl**Azure Active Directory** > **felhasználók és csoportok** > **összes csoport**. Majd keresse meg hello csoport, válassza ki azt, majd keresse meg toohello **licencek** külön-külön hello **hozzárendelése** gomb felett hello panel hello licenc hozzárendelése paneljének megnyitása.

## <a name="step-2-verify-that-hello-initial-assignment-has-finished"></a>2. lépés: Győződjön meg arról, hogy a hozzárendelés kezdeti hello befejeződött

1. Nyissa meg túl**Azure Active Directory** > **felhasználók és csoportok** > **összes csoport**. Ezután megkeresi a hello **HR-részleg** csoport, a hozzárendelt licenceket.

2. A hello **HR-részleg** csoport panelen válassza **licencek**. Ez lehetővé teszi gyorsan erősítse meg, ha licencek teljes hozzárendelt toousers és, amelyekre szüksége van a toolook hiba történik. hello a következő információ áll rendelkezésre:

   - Terméklicencek, amelyek jelenleg listája toohello csoport társítva. Válassza ki egy bejegyzést tooshow hello meghatározott szolgáltatásokat, amelyeken engedélyezve van, és a toomake módosításait.

   - Hello legújabb licenc elvégzett módosítások toohello csoport (ha hello módosítások feldolgozott vagy feldolgozása befejeződik az összes felhasználó tag számára) állapotát.

   - Felhasználók, akik nincsenek hibás állapotú, mert a licenc nem sikerült hozzárendelni a toothem kapcsolatos információkat.

   ![Hozzárendelés beállításai](media/active-directory-licensing-group-assignment-azure-portal/assignment-errors.png)

3. Részletes információ a feldolgozása a licenc **Azure Active Directory** > **felhasználók és csoportok** > *csoportnév*  >  **Naplók**. Vegye figyelembe a következő tevékenységek hello:

   - Tevékenység: **indítsa el a csoport licenc toousers alkalmazása**. Ez naplóz, amikor hello rendszer szerzi be hello licenc-hozzárendelés módosítása hello csoport és telepítené azt tooall felhasználói tagjai elindul. Hello változás, amely történt információt tartalmaz.

   - Tevékenység: **Befejezés csoport licenc toousers alkalmazása**. Ez naplóz, amikor a rendszer hello befejezi hello csoport minden tagjára feldolgozását. Hány felhasználó feldolgozása sikeresen megtörtént, és hány felhasználó nem sikerült hozzárendelni a csoport licencek összegzését tartalmazza.

   [Ebben a szakaszban olvasható](./active-directory-licensing-group-advanced.md#use-audit-logs-to-monitor-group-based-licensing-activity) toolearn további naplók hogyan lehet használt tooanalyze módosítások Csoportalapú licencelésével kapcsolatban.

## <a name="step-3-check-for-license-problems-and-resolve-them"></a>3. lépés: Ellenőrizze a licenc problémákat, és a megoldásukkal együtt

1. Nyissa meg túl**Azure Active Directory** > **felhasználók és csoportok** > **összes csoport**, és hello található **HR-részleg**csoport, a hozzárendelt licenceket.
2. A hello **HR-részleg** csoport panelen válassza **licencek**. hello értesítési fölött hello panelen látható, hogy vannak-e 10 olyan felhasználót, hogy a licencek nem sikerült hozzárendelni. Kattintson rá megnyitja az összes olyan felhasználó listáját egy licencelési-hibaállapot ehhez a csoporthoz.
3. Hello **nem sikerült a hozzárendelés** oszlopból kiderül, hogy mindkét terméklicencek nem sikerült hozzárendelni toohello felhasználók. Hello **hiba okának felső** oszlop tartalmaz hello hello hiba okát. Ebben az esetben van **ütköző service-csomagokról**.

   ![Nem sikerült hozzárendelések](media/active-directory-licensing-group-assignment-azure-portal/failed-assignments.png)

4. Válassza ki a felhasználói tooopen hello **licencek** panelen. Ezen a panelen látható már hozzá vannak rendelve toohello felhasználó összes licencet. Ebben a példában hello felhasználó rendelkezik-e hello öröklődött hello Office 365 nagyvállalati E1 csomag licenchez **teljes képernyős felhasználók** csoport. Ez nem ütközik az hello a rendszer megkísérelte tooapply hello hello E3 licenchez **HR-részleg** csoport. Ennek eredményeképpen a csoport hello licencek egyike rendelték toohello felhasználó.

   ![Egy felhasználó licencek megtekintése](media/active-directory-licensing-group-assignment-azure-portal/user-license-view.png)

5. toosolve az ütközést, eltávolítás hello felhasználót hello **teljes képernyős felhasználók** csoport. Az Azure AD folyamatok hello módosítása, után hello **HR-részleg** licencek megfelelően vannak hozzárendelve.

   ![-Licenccel megfelelően](media/active-directory-licensing-group-assignment-azure-portal/license-correctly-assigned.png)

## <a name="next-steps"></a>Következő lépések

hello funkcióval kapcsolatban további csoportok, a licenc-kezelő beállítása toolearn tekintse meg a következő cikkek hello:

* [Mi az a csoport-alapú licencelése az Azure Active Directoryban?](active-directory-licensing-whatis-azure-portal.md)
* [Majd azonosítani és megoldani az Azure Active Directory csoport licenc problémák](active-directory-licensing-group-problem-resolution-azure-portal.md)
* [Hogyan toomigrate személy licenccel rendelkező felhasználók toogroup alapú licencelése az Azure Active Directoryban](active-directory-licensing-group-migration-azure-portal.md)
* [Az Azure Active Directory biztonságicsoport-alapú licencelési további helyzeteket is](active-directory-licensing-group-advanced.md)
