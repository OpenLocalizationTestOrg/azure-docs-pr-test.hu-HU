---
title: Azure AD Privileged Identity Management aaaConfigure |} Microsoft Docs
description: "Ez a témakör ismerteti az Azure AD Privileged Identity Management, és hogyan toouse PIM tooimprove a felhő biztonsági."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: c548ed2e-06e3-4eaf-a63d-0f02ee72da25
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: billmath
ms.custom: pim ; H1Hack27Feb2017
ms.openlocfilehash: dbe49fe4a0f6e5b46ed5a17fc7e8dcdacafe3846
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-ad-privileged-identity-management"></a>Mi az Azure AD Privileged Identity Management?
Az Azure Active Directory (AD) Privileged Identity Management segítségével kezelheti, irányíthatja és felügyelheti a szervezeten belüli hozzáféréseket. Ez magában foglalja az Azure AD hozzáférési tooresources és más Microsoft online szolgáltatások, például az Office 365-öt vagy a Microsoft Intune.  

> [!NOTE]
> A privileged Identity Management teljes szervezet számára elérhető tooyour esetén a rendszergazdák az Azure Active Directory Premium P2 hello kiadásával licenceli. További információk: [Azure Active Directory editions](active-directory-editions.md) (Azure Active Directory-kiadások).

A szervezetek szeretné, hogy toominimize hello száma, akik toosecure adatok eléréséhez, vagy az erőforrások, mert, amely csökkenti a hello esélyét, hogy a hozzáférést egy rosszindulatú felhasználó. Felhasználók azonban továbbra is kell toocarry kimenő Azure, az Office 365 vagy az SaaS-alkalmazásokban privilegizált műveleteket. A szervezetek hozzáférést felhasználók kiemelt az Azure AD nélkül figyelését, ezek a felhasználók tevékenységeit a rendszergazda jogosultságokkal. Az Azure AD Privileged Identity Management segít tooresolve a kockázat.  

Az Azure AD Privileged Identity Management nyújt segítséget:  

* Mely felhasználók vannak-e az Azure AD-rendszergazdák
* Engedélyezze az igény, "csak az időben" rendszergazdai hozzáférés tooMicrosoft Online szolgáltatások, például Office 365 és az Intune-ban
* Rendszergazda-hozzárendelések beolvasása a rendszergazdai hozzáférési műveleteiről és a változások
* Értesítéskérés a hozzáférés tooa kiemelt szerepkörű
* Jóváhagyási tooactivate (előzetes verzió) megkövetelése

Az Azure AD Privileged Identity Management hello beépített az Azure AD szervezeti szerepkörök, például (de nem kizárólagosan) képes kezelni:  

* Globális rendszergazda
* Számlázási rendszergazda
* Szolgáltatás-rendszergazda  
* Felhasználó rendszergazda
* Jelszókezelő

## <a name="just-in-time-administrator-access"></a>Csak a rendszergazdai hozzáférés időpontja
Hagyományosan rendelheti olyan tooan rendszergazdai szerepkörhöz hello a klasszikus Azure portálon keresztül vagy a Windows PowerShell. Ennek eredményeképpen, ő lesz annak egy **állandó rendszergazdai**, mindig aktív hello hozzárendelve szerepkör. Az Azure AD Privileged Identity Management bemutatja hello egy **jogosult felügyeleti**. Jogosult rendszergazdák most majd, de nem minden nap privilegizált hozzáférést igénylő felhasználók kell lennie. hello szerepkör nem aktív, amíg hello felhasználói kell elérnie, akkor az aktiválási folyamat befejezéséhez, és egy aktív felügyeleti válik a előre meghatározott időn.

## <a name="enable-privileged-identity-management-for-your-directory"></a>Privileged Identity Management engedélyezéséhez a címtáron
Megkezdheti az Azure AD Privileged Identity Management a hello [Azure-portálon](https://portal.azure.com/).

> [!NOTE]
> Egy olyan szervezeti fiókkal egy globális rendszergazdának kell lennie (például @yourdomain.com), nem Microsoft-fiókkal (például @outlook.com), Azure AD Privileged Identity Management könyvtár tooenable.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/) a címtár globális rendszergazdaként.
2. Ha a szervezet több címtárral rendelkezik, válassza ki a felhasználónév hello jobb felső sarokban a hello Azure-portálon. Válassza ki a hello directory, ahol az Azure AD Privileged Identity Management szüksége lesz.
3. Válassza ki **további szolgáltatások** és hello szűrő szövegmező toosearch a **Azure AD Privileged Identity Management**.
4. Ellenőrizze **PIN-kód toodashboard** majd **létrehozása**. Megnyílik a Privileged Identity Management alkalmazás hello.

Ha az első, aki toouse Azure AD Privileged Identity Management most hello a könyvtárban, majd hello [biztonsági varázsló](active-directory-privileged-identity-management-security-wizard.md) bemutatja, hogyan hello hozzárendelés kezdeti lépésein. Ezt követően automatikusan lesz hello először **biztonsági rendszergazda** és **kiemelt szerepkörű rendszergazda** hello könyvtár.

Csak a kiemelt szerepkörű rendszergazda kezelheti a hozzáférést más rendszergazdák számára. Is [más felhasználók hello képességét toomanage ad PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).

## <a name="privileged-identity-management-admin-dashboard"></a>Privileged Identity Management felügyeleti Irányítópult
Az Azure AD Privileged Identity Manager biztosít egy felügyeleti irányítópultot, amely fontos információkat, például:

* Amely felhívja a lehetőségek tooimprove biztonsági riasztások
* hello tooeach kiemelt szerepkörhöz hozzárendelt felhasználók száma  
* jogosult és állandó rendszergazdák hello száma
* Egy grafikonon kiemelt szerepkörű tartanak a könyvtárban

![A PIM irányítópult – képernyőkép][2]

## <a name="privileged-role-management"></a>Kiemelt szerepkörű kezelése
Az Azure AD Privileged Identity Management kezelheti hello rendszergazdák hozzáadásával vagy eltávolításával a rendszergazdák állandó vagy jogosult tooeach szerepkör.

![A PIM hozzáadása rendszergazdák – képernyőkép][3]

## <a name="configure-hello-role-activation-settings"></a>Hello szerepkör aktiválási beállításainak konfigurálása
Hello segítségével [szerepkör beállításainak](active-directory-privileged-identity-management-how-to-change-default-settings.md) hello jogosult szerepkör aktiválása tulajdonságait többek között konfigurálhatja:

* hello szerepkör aktiválási időszak hello időtartama
* hello szerepkör-aktiválási értesítés
* hello információk egy felhasználó tooprovide hello szerepkör-aktiválási folyamat során van szükség.
* Szolgáltatás-jegy vagy incidens száma
* [Jóváhagyási munkafolyamat követelmények – előzetes](./privileged-identity-management/azure-ad-pim-approval-workflow.md)

![Képernyőfelvétel a PIM - rendszergazdai aktiválás - beállítások][4]

Vegye figyelembe, hogy hello képen hello szolgáló gombok **multi-factor Authentication** le vannak tiltva. Egyes, nagy kiemelt szerepköröket, a megnöveli védelemhez szükséges többtényezős Hitelesítést.

## <a name="role-activation"></a>Szerepkör aktiválása
túl[szerepkört aktiváló](active-directory-privileged-identity-management-how-to-activate-role.md), egy erre jogosult rendszergazda kér egy időhöz kötött "aktiválás" hello szerepkörhöz. hello aktiválási hello is meg kell adniuk **a szerepkör aktiválásához** Azure AD Privileged Identity Management beállítása.

Egy rendszergazda szerepkör tooactivate kívánó tooinitialize hello Azure-portálon az Azure AD Privileged Identity Management kell.

Szerepkör aktiválása testre szabható. Hello PIM beállításaiban azt is meghatározhatja hello aktiválás és milyen információkat Üdvözöljük a rendszergazdákat kell tooprovide tooactivate hello szerepkör hello hosszát.

![PIM rendszergazda kérelem szerepkör aktiválása – képernyőkép][5]

## <a name="review-role-activity"></a>Felülvizsgálati szerepkör tevékenység
Két módon tootrack van az alkalmazottak és a rendszergazdák hogyan használják a kiemelt szerepköröket. hello első lehetőség által használt [Directory szerepkörök naplózási előzmények](active-directory-privileged-identity-management-how-to-use-audit-log.md). hello naplózási előzmények változások követése kiemelt szerepkör-hozzárendelések és a szerepkör aktiválása előzmények naplózza.

![A PIM aktiválási előzmények – képernyőkép][6]

hello második lehetőség mentése rendszeres tooset [értékelést hozzáférési](active-directory-privileged-identity-management-how-to-start-security-review.md). Ezek hozzáférés értékelést végzi, és hozzárendelt felülvizsgáló (például egy csoport manager) vagy hello alkalmazottak tekintheti át magukat. Ez a hello legjobb módja toomonitor akik továbbra is hozzáférést igényel, és ki nem tesz.

## <a name="azure-ad-pim-at-subscription-expiration"></a>Az Azure AD PIM előfizetés lejárati:
Előzetes tooreaching általános rendelkezésre állási Azure AD PIM preview, és engedélyezve volt a bérlő toopreview Azure AD PIM keresi.  Most, hogy az Azure AD PIM elérte az általánosan rendelkezésre álló, próbaverziós vagy fizetős licencek toohello rendszergazdái hello bérlői toocontinue PIM használatával kell hozzárendelni.  Ha a szervezet nem vásárol Azure AD Premium P2, vagy a próbaidőszak lejárata, többnyire az összes hello Azure AD PIM szolgáltatás már nem lesz elérhető az Ön bérelt szolgáltatásának.  További a hello [Azure AD PIM előfizetés követelményeinek](./privileged-identity-management/subscription-requirements.md)

## <a name="next-steps"></a>Következő lépések
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png
[2]: ./media/active-directory-privileged-identity-management-configure/PIM_Admin_Overview.png
[3]: ./media/active-directory-privileged-identity-management-configure/PIM_AddRemove.png
[4]: ./media/active-directory-privileged-identity-management-configure/PIM_Settings_w_Approval_Disabled.png
[5]: ./media/active-directory-privileged-identity-management-configure/PIM_RequestActivation.png
[6]: ./media/active-directory-privileged-identity-management-configure/PIM_ActivationHistory.png
