---
title: "Konfigurálja az Azure AD Privileged Identity Management |} Microsoft Docs"
description: "Ez a témakör azt ismerteti, mi az Azure AD Privileged Identity Management és a PIM használatát a felhő biztonság növelése érdekében."
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
ms.openlocfilehash: eb7059368cb80be7dd625f9dc6ad2aab1bad709a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="what-is-azure-ad-privileged-identity-management"></a>Mi az Azure AD Privileged Identity Management?
Az Azure Active Directory (AD) Privileged Identity Management segítségével kezelheti, irányíthatja és felügyelheti a szervezeten belüli hozzáféréseket. Ebbe beletartozik az Azure AD és más online Microsoft-szolgáltatások, például az Office 365 vagy a Microsoft Intune erőforrásaihoz való hozzáférés.  

> [!NOTE]
> A privileged Identity Management a rendszergazdák az Azure Active Directory Premium P2 kiadásával licenceli esetén érhető el a teljes szervezet számára. További információk: [Azure Active Directory editions](active-directory-editions.md) (Azure Active Directory-kiadások).

A szervezetek szeretne, amely csökkenti az esélye, hogy a hozzáférést egy rosszindulatú felhasználó, mert a személyeket, akik hozzáférhetnek a bizalmas adatok vagy az erőforrások számának csökkentése érdekében. Felhasználók azonban továbbra is kell Azure, az Office 365 vagy az SaaS-alkalmazások kiemelt műveletek végrehajtására. A szervezetek hozzáférést felhasználók kiemelt az Azure AD nélkül figyelését, ezek a felhasználók tevékenységeit a rendszergazda jogosultságokkal. Az Azure AD Privileged Identity Management segít a kockázat megoldásához.  

Az Azure AD Privileged Identity Management nyújt segítséget:  

* Mely felhasználók vannak-e az Azure AD-rendszergazdák
* Engedélyezze az igény, "igény szerint" a Microsoft Online Services rendszergazdai hozzáféréssel, például Office 365 és az Intune-ban
* Rendszergazda-hozzárendelések beolvasása a rendszergazdai hozzáférési műveleteiről és a változások
* Egy kiemelt szerepkörhöz való hozzáféréssel kapcsolatos riasztásokat kaphat
* (Előzetes verzió) aktiválása jóváhagyás szükséges

Az Azure AD Privileged Identity Management a beépített kezelheti az Azure AD szervezeti szerepkörök, például (de nem kizárólagosan):  

* Globális rendszergazda
* Számlázási rendszergazda
* Szolgáltatás-rendszergazda  
* Felhasználó rendszergazda
* Jelszókezelő

## <a name="just-in-time-administrator-access"></a>Csak a rendszergazdai hozzáférés időpontja
Hagyományosan felhasználó sikerült hozzárendelése egy rendszergazdai szerepkört, a klasszikus Azure portálon vagy a Windows Powershellen keresztül. Ennek eredményeképpen, ő lesz annak egy **állandó rendszergazdai**, mindig a hozzárendelt szerepkör aktív. Az Azure AD Privileged Identity Management jelenik meg az egy **jogosult felügyeleti**. Jogosult rendszergazdák most majd, de nem minden nap privilegizált hozzáférést igénylő felhasználók kell lennie. A szerepkör nem aktív, amíg a felhasználó kell elérnie, akkor az aktiválási folyamat befejezéséhez, és egy előre meghatározott időn egy aktív felügyeleti válik.

## <a name="enable-privileged-identity-management-for-your-directory"></a>Privileged Identity Management engedélyezéséhez a címtáron
Az Azure AD Privileged Identity Management indíthatja a [Azure-portálon](https://portal.azure.com/).

> [!NOTE]
> Egy olyan szervezeti fiókkal egy globális rendszergazdának kell lennie (például @yourdomain.com), nem Microsoft-fiókkal (például @outlook.com), a címtár az Azure AD Privileged Identity Management engedélyezéséhez.

1. Jelentkezzen be az [Azure Portalra](https://portal.azure.com/) a címtára globális rendszergazdájaként.
2. Ha a szervezet több címtárral rendelkezik, akkor kattintson a felhasználónevére az Azure Portal jobb felső sarkában. Válassza ki a könyvtárat, ahol az Azure AD Privileged Identity Management fogja használni.
3. Válassza a **További szolgáltatások** lehetőséget, és a Szűrők szövegmezővel keresse meg az **Azure AD Privileged Identity Management** alkalmazást.
4. Jelölje be a **Rögzítés az irányítópulton** jelölőnégyzetet, majd kattintson a **Létrehozás** gombra. Megnyílik a Privileged Identity Management alkalmazás.

Ha Ön az első, aki a címtárban az Azure AD Privileged Identity Management alkalmazást használja, akkor a [biztonság varázsló](active-directory-privileged-identity-management-security-wizard.md) végigvezeti a hozzárendelés kezdeti lépésein. Ezt követően automatikusan vált az első **biztonsági rendszergazda** és **kiemelt szerepkörű rendszergazda** a könyvtár.

Csak a kiemelt szerepkörű rendszergazda kezelheti a hozzáférést más rendszergazdák számára. Is [ismerkedő más felhasználók kezelhetik a PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).

## <a name="privileged-identity-management-admin-dashboard"></a>Privileged Identity Management felügyeleti Irányítópult
Az Azure AD Privileged Identity Manager biztosít egy felügyeleti irányítópultot, amely fontos információkat, például:

* Riasztások, amely felhívja a biztonság növelése érdekében lehetőségek
* Minden kiemelt szerepkörhöz rendelt felhasználók száma  
* A jogosult és állandó rendszergazdák száma
* Egy grafikonon kiemelt szerepkörű tartanak a könyvtárban

![A PIM irányítópult – képernyőkép][2]

## <a name="privileged-role-management"></a>Kiemelt szerepkörű kezelése
Az Azure AD Privileged Identity Management kezelheti a rendszergazdák hozzáadásával vagy eltávolításával szerepkörönként rendszergazdák állandó sem jogosultak.

![A PIM hozzáadása rendszergazdák – képernyőkép][3]

## <a name="configure-the-role-activation-settings"></a>A szerepkör-aktiválási beállításainak konfigurálása
Használja a [szerepkör beállításainak](active-directory-privileged-identity-management-how-to-change-default-settings.md) állíthatja be a megfelelő szerepkör aktiválási tulajdonságait, beleértve:

* A szerepkör-aktiválási időszak
* A szerepkör-aktiválási értesítés
* A felhasználó adatokat kell megadni. a szerepkör-aktiválási folyamat során
* Szolgáltatás-jegy vagy incidens száma
* [Jóváhagyási munkafolyamat követelmények – előzetes](./privileged-identity-management/azure-ad-pim-approval-workflow.md)

![Képernyőfelvétel a PIM - rendszergazdai aktiválás - beállítások][4]

Vegye figyelembe, hogy a kép gombjai **multi-factor Authentication** le vannak tiltva. Egyes, nagy kiemelt szerepköröket, a megnöveli védelemhez szükséges többtényezős Hitelesítést.

## <a name="role-activation"></a>Szerepkör aktiválása
A [szerepkört aktiváló](active-directory-privileged-identity-management-how-to-activate-role.md), egy erre jogosult rendszergazda kér egy időhöz kötött "aktiválás" a szerepkörhöz. Az aktiválás használatával kell adniuk a **a szerepkör aktiválásához** Azure AD Privileged Identity Management beállítása.

Egy rendszergazda, aki szerepkört aktiváló szeretne kell inicializálni az Azure AD Privileged Identity Management az Azure portálon.

Szerepkör aktiválása testre szabható. A PIM beállítások segítségével meghatározhatja az aktiválás és milyen adatokat a rendszergazdának kell megadni. a szerepkör aktiválásához.

![PIM rendszergazda kérelem szerepkör aktiválása – képernyőkép][5]

## <a name="review-role-activity"></a>Felülvizsgálati szerepkör tevékenység
Két módon lehet kísérje figyelemmel, hogy az alkalmazottak és a rendszergazdák kiemelt szerepköröket használ. Az első lehetőség által használt [Directory szerepkörök naplózási előzmények](active-directory-privileged-identity-management-how-to-use-audit-log.md). A naplózási előzmények változások követése kiemelt szerepkör-hozzárendelések és a szerepkör aktiválása előzmények naplózza.

![A PIM aktiválási előzmények – képernyőkép][6]

A második beállítás lesz rendszeres beállítása [értékelést hozzáférési](active-directory-privileged-identity-management-how-to-start-security-review.md). Ezek hozzáférés értékelést végzi, és hozzárendelt felülvizsgáló (például egy csoport manager) vagy az alkalmazottak saját magukat tudja ellenőrizni. Ez az a legjobb módszer a figyelheti, akik továbbra is hozzáférést igényel, és ki nem tesz.

## <a name="azure-ad-pim-at-subscription-expiration"></a>Az Azure AD PIM előfizetés lejárati:
Általános rendelkezésre állás elérése előtt Azure AD PIM preview, és nem licenc ellenőrzi a bérlő az Azure AD PIM előzetes történt.  Most, hogy az Azure AD PIM elérte az általánosan rendelkezésre álló, próbaverziós vagy fizetős licencet hozzá kell rendelni a bérlő rendszergazdák továbbra is használja a PIM.  Ha a szervezet nem vásárol Azure AD Premium P2, vagy a próbaidőszak lejárata, többnyire az összes Azure AD PIM szolgáltatás már nem lesz elérhető az Ön bérelt szolgáltatásának.  A további a [Azure AD PIM előfizetés követelményeinek](./privileged-identity-management/subscription-requirements.md)

## <a name="next-steps"></a>Következő lépések
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png
[2]: ./media/active-directory-privileged-identity-management-configure/PIM_Admin_Overview.png
[3]: ./media/active-directory-privileged-identity-management-configure/PIM_AddRemove.png
[4]: ./media/active-directory-privileged-identity-management-configure/PIM_Settings_w_Approval_Disabled.png
[5]: ./media/active-directory-privileged-identity-management-configure/PIM_RequestActivation.png
[6]: ./media/active-directory-privileged-identity-management-configure/PIM_ActivationHistory.png
