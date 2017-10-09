---
title: "aaaAdd, vagy módosítsa az Azure rendszergazdai előfizetés szerepkörök |} Microsoft Docs"
description: "Ismerteti, hogyan tooadd Azure Társadminisztrátoraként, a szolgáltatás-rendszergazda és a fiók rendszergazdájához vagy módosítása"
services: 
documentationcenter: 
author: genlin
manager: jlian
editor: 
tags: billing
ms.assetid: 13a72d76-e043-4212-bcac-a35f4a27ee26
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: genli
ms.openlocfilehash: 14eaecf2dbfd7152775ac3552bf3a7ae3db596b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-or-change-azure-administrator-roles-that-manage-hello-subscription-or-services"></a>Hozzáadása vagy módosítása az Azure-rendszergazdai szerepkörök, amelyek hello előfizetés vagy a szolgáltatások kezelése
Módosíthatja, amely kezeli az Azure-előfizetéshez és nem is kezeli az Azure rendszergazdai hello hello Azure-szolgáltatásokhoz használt az előfizetésben. tooview Azure számlázási adatokat és -előfizetések kezelése, be kell jelentkeznie a toohello [Account Center](https://account.windowsazure.com/Home/Index) , hello fiók rendszergazdájához. 

## <a name="add-an-admin-for-a-subscription"></a>Adja hozzá a rendszergazda az előfizetéshez
Egy Azure rendszergazdát adhat hozzá, hello Azure-portálon vagy a klasszikus Azure portálon hello.

**Azure Portal**

tooadd valaki az előfizetésre hello Azure portálon rendszergazdaként, hogy számukra hello tulajdonosi szerepkört. hello tulajdonosi szerepkör csak kezelhetik az hello erőforrásokat rendelt hello előfizetésben. Nem rendelkezik hozzáférési jogosultsággal tooother előfizetések. hello keresztül hozzáadhat tulajdonosok hello [Azure-portálon](https://portal.azure.com) nem tudja kezelni a hello erőforrás [a klasszikus Azure portálon](https://manage.windowsazure.com).

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Hello központ menüben válassza ki a **előfizetés** > *előfizetést, amelyet az hello admin tooaccess hello*.

    ![Képernyőkép a kijelölt előfizetés](./media/billing-add-change-azure-subscription-administrator/newselectsub.png)

3. Hello előfizetés panelen válassza ki a **hozzáférés-vezérlés (IAM)**.
4. Válassza ki **hozzáadása** > **szerepkör** > **tulajdonos**. Írja be a hello felhasználó e-mail címe az hello tooadd szeretné tulajdonosa, jelölje be hello felhasználó, és válassza ki **mentése**.

    ![Képernyőkép a kiválasztott hello tulajdonosi szerepkör](./media/billing-add-change-azure-subscription-administrator/add-role.png)

5. Ha azt szeretné, tooadd hello tulajdonos fiók közös rendszergazdaként az hello **hozzáférés-vezérlés (IAM)** lapon, kattintson a jobb gombbal a hello felhasználó, és válassza ki **közös rendszergazdaként Hozzáadás**. Ez a funkció már elérhető a [Azure betekintő portál](https://preview.portal.azure.com/). 

     ![Képernyőkép a társadminisztrátoraként hozzáadása](./media/billing-add-change-azure-subscription-administrator/add-coadmin.png)

    >[!TIP]
    >Szüksége lesz tooadd hello "Owner" felhasználói párhuzamos rendszergazdaként Ha hello felhasználónak toomanage van szüksége az Azure-szolgáltatások hello [a klasszikus Azure portálon](https://manage.windowsazure.com/).

    tooremove hello közös rendszergazdai jogosultsággal, kattintson a jobb gombbal a hello "közös rendszergazda" felhasználói, és válassza ki **közös rendszergazda eltávolítása**.

    ![Képernyőkép a társadminisztrátoraként eltávolítása](./media/billing-add-change-azure-subscription-administrator/remove-coadmin.png)


**klasszikus Azure portál**

1. Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com/).
2. Hello navigációs ablaktábláján válassza ki **beállítások**> **rendszergazdák**> **Hozzáadás**. </br>

    ![Képernyőkép a hogyan tooget toohello hozzáadása gomb](./media/billing-add-change-azure-subscription-administrator/addcoadmin.png)
3. Típus hello hello felvenni kívánt személy e-mail tooadd közös rendszergazdaként, és válassza ki, hogy szeretné-e hello társadminisztrátoraként tooaccess hello előfizetés.</br>

    ![A kijelölt előfizetésben képernyőkép ](./media/billing-add-change-azure-subscription-administrator/addcoadmin2.png)</br>

a következő e-mail cím hello felveheti társadminisztrátoraként:

* **Microsoft Account** (korábbi nevén Windows Live ID Azonosítóval) </br>
  A Microsoft Account toosign használhatja tooall kereskedelmi célú Microsoft-termékek és a felhőalapú szolgáltatások, például az Outlook (Hotmail), a Skype (MSN), a onedrive vállalati verzió, a Windows Phone és a Xbox LIVE.
* **Szervezeti fiókkal**</br>
  Egy szervezeti egy Azure Active Directory alatt létrehozott fiók. hello szervezeti fiók címe van ebben a formátumban:

    @ felhasználó&lt;a tartomány&gt;. onmicrosoft.com

## <a name="change-service-administrator-for-a-subscription"></a>Módosítsa az előfizetés szolgáltatás-rendszergazda
Csak a Fiókadminisztrátor hello módosíthatja hello szolgáltatás-rendszergazdát az előfizetéshez.

1. Jelentkezzen be a túl[Azure Account Center](https://account.windowsazure.com/subscriptions) hello rendszergazdai fiók használatával.
2. Válassza ki a kívánt toochange hello előfizetést.
3. Hello jobb oldalon válassza ki a **előfizetés szerkesztése** részleteit. </br>

    ![editsub](./media/billing-add-change-azure-subscription-administrator/editsub.png)
4. A hello **szolgáltatás-rendszergazda** hello hello e-mail címét adja meg új szolgáltatás-rendszergazda. </br>

    ![changeSA](./media/billing-add-change-azure-subscription-administrator/changeSA.png)

## <a name="change-hello-account-administrator"></a>Hello Fiókadminisztrátort módosítása
tootransfer tulajdonjogát hello Azure-fiók tooanother fiók, lásd: [Azure-előfizetés átvitele tulajdonjogát](billing-subscription-transfer.md).

Határozottan javasoljuk, hogy nem törli vagy nevezze át a hello fiók rendszergazdai e-mail címet. Nem várt és nemkívánatos működését hello Azure-fiók jelenhet meg. Nem lehet képes bejelentkezési tooAzure azzal a fiókkal, módosításokat toohello fiókot, és kezelheti az erőforrásokat, azzal a fiókkal. 

## <a name="check-hello-account-administrator-of-hello-subscription"></a>Hello Fiókadminisztrátort hello előfizetés ellenőrzése
Ha nem biztos, aki hello fiókadminisztrátort az előfizetéséhez, használja a következő lépéseket toofind kimenő hello.

  1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
  2. Hello központ menüben válassza ki a **előfizetés**.
  3. Jelölje ki azt szeretné, hogy toocheck, majd az hello előfizetést **beállítások**.
  4. Válassza ki **tulajdonságok**. hello fiókadminisztrátort hello előfizetés hello megjelenő **Fiókadminisztrátor** mezőbe.  

## <a name="types-of-azure-admin-accounts"></a>Az Azure rendszergazdai fiókok típusai
 Fiók rendszergazdájához, szolgáltatás-rendszergazda és társadminisztrátoraként háromféle hello a Microsoft Azure-ban rendszergazdai szerepkörrel. hello következő táblázat ismerteti ezeket három rendszergazdai szerepkörei hello különbsége.

| Rendszergazdai szerepkör | Korlát | Leírás |
| --- | --- | --- |
| A Fiókadminisztrátor (AA) |1 / Azure-fiók |Ez az hello személy, aki regisztrált a vagy vásárolt Azure-előfizetést, és jogosult tooaccess hello [Account Center](https://account.windowsazure.com/Home/Index) és különféle felügyeleti feladatok elvégzésére. Ezek például képes toocreate előfizetések, előfizetések megszakítja, módosítsa az előfizetéshez hello számlázási és hello szolgáltatás-rendszergazda módosíthatja. |
| Szolgáltatás-rendszergazdai (SA) |1 / Azure-előfizetés |A szerepkör engedélyezett toomanage szolgáltatások hello [Azure-portálon](https://portal.azure.com). Alapértelmezés szerint egy új előfizetés hello Fiókadminisztrátort egyben hello szolgáltatás-rendszergazdát. |
| A hello társadminisztrátoraként (CA) [a klasszikus Azure portálon](https://manage.windowsazure.com) |200 előfizetésenként |Ez a szerepkör rendelkezik hello ugyanaz, mint hello szolgáltatás-rendszergazda hozzáférési jogosultsággal, de nem módosíthatják előfizetések tooAzure könyvtárak hello hozzárendelését. |

Az Azure Active Directory szerepköralapú hozzáférés-vezérlés (RBAC) lehetővé teszi, hogy a felhasználók toobe hozzáadott toomultiple szerepkörök. További információkért lásd: [Azure Active Directory szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md).

## <a name="limitations-and-restrictions-for-admin-accounts"></a>Korlátozások és rendszergazdai fiókok korlátozásai
* Minden előfizetés társítva az Azure AD-címtár (más néven hello alapértelmezett címtárat). toofind hello alapértelmezett címtárat hello előfizetés társítva, nyissa meg toohello [a klasszikus Azure portálon](https://manage.windowsazure.com/), jelölje be **beállítások** > **előfizetések**. Ellenőrizze a hello előfizetési azonosító toofind hello alapértelmezett címtárat.
* Ha Microsoft Account van bejelentkezve, csak hozzáadhat más Microsoft Accounts vagy hello alapértelmezett címtárat belüli felhasználók közös rendszergazdaként.
* Ha szervezeti fiókkal van bejelentkezve, közös rendszergazdaként a szervezet más szervezeti fiókot is hozzáadhat. Például abby@contoso.com adhat hozzá bob@contoso.com , szolgáltatás-rendszergazdai vagy társadminisztrátori, nem adható hozzá, de john@notcontoso.com kivéve, ha john@notcontoso.com az alapértelmezett könyvtár. Szolgáltatás rendszergazdájának vagy Társadminisztrátorának tooadd Microsoft Account felhasználók a munkahelyi és iskolai fiókok bejelentkezve felhasználók továbbra is.
* Most, hogy a szervezeti fiókkal tooAzure lehetséges toosign, az alábbiakban hello módosítások tooService rendszergazda és a közös rendszergazdai fiókra vonatkozó követelmények:

  | Bejelentkezés módszer | Adja hozzá a Microsoft Account vagy alapértelmezett könyvtárban lévő felhasználók vagy a rendszergazdai (SA)? | Szervezeti fiók hozzáadása a hello ugyanazon a szervezeten belül, vagy a rendszergazdai (SA)? | Szervezeti fiók hozzáadása a másik szervezet Hitelesítésszolgáltatói vagy a rendszergazdai (SA)? |
  | --- | --- | --- | --- |
  |  Microsoft-fiók |Igen |Nem |Nem |
  |  Szervezeti fiók |Igen |Igen |Nem |

## <a name="learn-more-about-resource-access-control-and-active-directory"></a>További információ az erőforrás hozzáférés-vezérlés és az Active Directory
* toolearn hogyan szabályozott erőforrások elérése a Microsoft Azure-ban kapcsolatos további információkért lásd: [az az Azure-erőforrások hozzáférésének megismerése](../active-directory/active-directory-understanding-resource-access.md).
* Azure Active Directoryval kapcsolatos további információkért lásd: [kapcsolódnak hogyan Azure-előfizetések az Azure Active Directory](../active-directory/active-directory-how-subscriptions-associated-directory.md) és [rendszergazdai szerepkörök hozzárendelése az Azure Active Directory](../active-directory/active-directory-assign-admin-roles.md).

## <a name="need-help-contact-support"></a>Segítség Forduljon a támogatási szolgálathoz.
Ha további segítségre van, [forduljon a támogatási szolgálathoz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) a probléma megoldódik gyorsan tooget.
