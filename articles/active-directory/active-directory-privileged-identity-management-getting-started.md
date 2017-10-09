---
title: "az Azure AD Privileged Identity Management lépései aaaGet |} Microsoft Docs"
description: "Ismerje meg, hogyan toomanage kiemelt identitásokat az Azure-portálon hello Azure Active Directory Privileged Identity Management alkalmazás."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 2299db7d-bee7-40d0-b3c6-8d628ac61071
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/27/2017
ms.author: billmath
ms.custom: pim ; H1Hack27Feb2017
ms.openlocfilehash: a89205023a8dbcc3649fa732735ca927e64736ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="start-using-azure-ad-privileged-identity-management"></a>Az Azure AD Privileged Identity Management használatba vétele
Az Azure Active Directory (AD) Privileged Identity Management segítségével kezelheti, irányíthatja és felügyelheti a szervezeten belüli hozzáféréseket. Ebben a hatókörben tartalmazza az Azure AD hozzáférési tooresources és más Microsoft online szolgáltatások, például az Office 365-öt vagy a Microsoft Intune.

Ez a cikk azt ismerteti, hogyan tooadd hello Azure AD PIM alkalmazást tooyour Azure portál Irányítópultjára.

## <a name="add-hello-privileged-identity-management-application"></a>Hello Privileged Identity Management alkalmazás felvétele
Azure AD Privileged Identity Management használata előtt kell tooadd hello alkalmazás tooyour Azure portál Irányítópultjára.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/) a címtár globális rendszergazdaként.
2. Ha a szervezet több címtárral rendelkezik, válassza ki a felhasználónév hello jobb felső sarokban a hello Azure-portálon. Válassza ki a kívánt toouse PIM hello könyvtár.
3. Válassza ki **további szolgáltatások** és hello szűrő szövegmező toosearch a **Azure AD Privileged Identity Management**.
4. Ellenőrizze **PIN-kód toodashboard** majd **létrehozása**. Megnyílik a Privileged Identity Management alkalmazás hello.

Első, aki toouse Azure AD Privileged Identity Management a könyvtárban van hello, ha automatikusan kapott hello **biztonsági rendszergazda** és **kiemelt szerepkörű rendszergazda** szerepkörök hello könyvtárban. Csak a kiemelt szerepkörű rendszergazdák kezelhetik a felhasználók szerepkör-hozzárendeléseit. Emellett úgy is dönthet, toorun hello [biztonsági varázsló.](active-directory-privileged-identity-management-security-wizard.md) amely bemutatja, hogyan hello kezdeti felderítés és a hozzárendelés környezetet.

## <a name="navigate-tooyour-tasks"></a>Keresse meg a tooyour feladatok
Azure AD Privileged Identity Management beállítása után hello navigációs panelen láthatja hello alkalmazás minden indításakor. A panel tooaccomplish használja az identitás-kezelési feladatok.

![Felső szintű PIM-tevékenységek – képernyőkép](./media/active-directory-privileged-identity-management-getting-started/PIM_Tasks_New.png)

* **A szerepkörök** viszi tooa tooyou rendelt szerepkörök listáját. Ebben a szakaszban aktiválhatja az Ön számára elérhető szerepköröket.
* A **Kérések jóváhagyása (Előnézet)** a címtárban található felhasználók függőben lévő aktiválási kéréseit jeleníti meg. [Részletek](./privileged-identity-management/azure-ad-pim-approval-workflow.md)
* **Függőben lévő kérelmek (előzetes verzió)** bármely aktuális kérelmek végrehajtott toohave tooactivate jeleníti meg.
* **Tekintse át a hozzáférési** tooany hozzáférés függőben lévő ellenőrzi az, hogy kell-e toocomplete, hogy hozzáférési véleményezése saját magának vagy valaki más vesz igénybe.
* **Az Azure Active Directory szerepkörök** hello irányítópultot kiemelt szerepkörű rendszergazda toomanage szerepkör-hozzárendelések, szerepkör-aktiválási beállításainak módosítása, kezdő hozzáférés értékelést és több hello "Kezelése" szakasz alatt van. az irányítópulton hello-beállítások le vannak tiltva, bárki, aki nem kiemelt szerepkörű rendszergazda.

## <a name="next-steps"></a>Következő lépések
Hello [Azure AD Privileged Identity Management áttekintése](active-directory-privileged-identity-management-configure.md) hogyan kezelheti a rendszergazdai hozzáférést a szervezet további részleteket tartalmaz.

[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png
