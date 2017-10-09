---
title: "Az Azure AD Connect: További lépések és hogyan toomanage az Azure AD Connect |} Microsoft Docs"
description: "Ismerje meg, hogyan tooextend hello alapértelmezett konfiguráció és a működtetési feladatok az Azure AD Connect."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: curtand
ms.assetid: c18bee36-aebf-4281-b8fc-3fe14116f1a5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 4404aaff24d54d76b83baca3b331a6a250ba4c03
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="next-steps-and-how-toomanage-azure-ad-connect"></a>További lépések és hogyan toomanage az Azure AD Connect
Eljárásokkal hello működési Ez a cikk toocustomize Azure Active Directory (Azure AD) Connect toomeet a szervezete igényét.  

## <a name="add-additional-sync-admins"></a>További szinkronizálási rendszergazdák hozzáadása
Alapértelmezés szerint csak hello felhasználó adott hello telepítési és a helyi rendszergazdák képesek toomanage telepítve hello szinkronizálási motor található. A további személyek toobe képes tooaccess hello szinkronizálási motor kezelése, majd keresse meg az ADSyncAdmins nevű a helyi kiszolgálón hello hello csoport és a toothis csoporthoz adja hozzá.

## <a name="assign-licenses-tooazure-ad-premium-and-enterprise-mobility-suite-users"></a>Licencek tooAzure AD Premium és nagyvállalati mobilitási csomag felhasználók hozzárendelése
Most, hogy a felhasználók lettek toohello felhő szinkronizálva tooassign kell őket, a felhőalapú alkalmazások, például az Office 365 használatbavétele licenccel.

### <a name="tooassign-an-azure-ad-premium-or-enterprise-mobility-suite-license"></a>az Azure AD Premium vagy Enterprise Mobility Suite licenc tooassign

1. Jelentkezzen be toohello Azure-portálon, rendszergazdaként
2. Hello bal oldalon válassza ki a **Active Directory**.
3. A hello **Active Directory** lapon, kattintson duplán a megegyező tooset akarja hello felhasználók hello könyvtárat.
4. Hello hello directory oldal tetején, válassza ki a **licencek**.
5. A hello **licencek** lapon jelölje be **Active Directory Premium** vagy **nagyvállalati mobilitási csomag**, és kattintson a **hozzárendelése**.
6. A hello párbeszédpanelen válassza ki a hello felhasználók tooassign licenceket szeretne, és kattintson a hello pipa ikon toosave hello módosításokat.

## <a name="verify-hello-scheduled-synchronization-task"></a>Hello ütemezett szinkronizálás task ellenőrzése.
A szinkronizálás hello Azure portál toocheck hello állapotát használja.

### <a name="tooverify-hello-scheduled-synchronization-task"></a>tooverify hello beütemezett szinkronizálási feladat
1. Jelentkezzen be toohello Azure-portálon, rendszergazdaként
2. Hello bal oldalon válassza ki a **Active Directory**.
3. A hello **Active Directory** lapon, kattintson duplán a megegyező tooset akarja hello felhasználók hello könyvtárat.
4. Hello hello directory oldal tetején, válassza ki a **címtár-integráció**.
5. A **helyi active directory integrációja a**, Megjegyzés hello utolsó szinkronizálás.

<center>![Címtár-szinkronizálás ideje](./media/active-directory-aadconnect-whats-next/verify.png)</center>

## <a name="start-a-scheduled-synchronization-task"></a>Egy beütemezett szinkronizálási feladat indítása
Ha egy szinkronizálási feladat toorun van szüksége, ehhez keresztül hello Azure AD Connect varázsló ismételt futtatásával.  Azure AD hitelesítő adatait kell tooprovide.  Hello varázslóban válassza hello **testre szabhatja a szinkronizálási beállítások** feladat, és kattintson a **következő** toomove hello varázsló használatával. Hello végén győződjön meg arról, hogy hello **hello szinkronizálási folyamat indítása, amint hello kezdeti konfigurálás befejeződik** be van jelölve.

<center>![A szinkronizálás megkezdéséhez](./media/active-directory-aadconnect-whats-next/startsynch.png)</center>

Hello Azure AD Connect szinkronizálási szolgáltatás Feladatütemező további információkért lásd: [az Azure AD Connect Feladatütemező](active-directory-aadconnectsync-feature-scheduler.md).

## <a name="additional-tasks-available-in-azure-ad-connect"></a>Az Azure AD Connectben elérhető további feladatok
Az Azure AD Connect a kezdeti telepítés után is mindig hello varázsló ismételt futtatása az Azure AD Connect start lap vagy az asztal helyi hello.  Megfigyelheti, hogy újra áthaladás hello varázsló lehetőségeket is kínál néhány új további feladatok hello formájában.  

hello következő táblázat a feladatok összefoglalása, és minden feladat rövid leírása.

![További feladatok listája](./media/active-directory-aadconnect-whats-next/addtasks.png)

| További feladatok | Leírás |
| --- | --- |
| **Nézet hello választott forgatókönyv** |A jelenleg az Azure AD Connect megoldás megtekintéséhez.  Ez magában foglalja az általános beállítások szinkronizálása könyvtárak, és a szinkronizálási beállítások. |
| **Szinkronizálási beállítások testreszabása** |Hello aktuális konfigurációjának módosítása például további Active Directory-erdők toohello konfiguráció hozzáadása, illetve a szinkronizálási beállítások, például felhasználó, csoport, eszköz vagy a jelszóvisszaírás engedélyezése. |
| **Az átmeneti környezetű üzemmód engedélyezése** |Szakasz információkat, amelyek nem azonnal szinkronizálja, és nem tooAzure AD vagy a helyszíni Active Directory exportált.  Ezzel a szolgáltatással mielőtt bekövetkeznének megtekintheti hello szinkronizálások. |

## <a name="next-steps"></a>Következő lépések
További információ [a helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).
