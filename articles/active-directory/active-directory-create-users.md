---
title: "aaaAdd új felhasználók tooAzure Active Directory |} Microsoft Docs"
description: "Azt ismerteti, hogyan tooadd új felhasználók, vagy módosítsa a felhasználói adatokat az Azure Active Directoryban."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: e3673727-6bec-4fdc-87a4-d65b213c4c3c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: oldportal;it-pro;
robots: NOINDEX
ms.openlocfilehash: 72f67ad41022fd19fd94c8e1301943b0db1e57bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-new-users-or-users-with-microsoft-accounts-tooazure-active-directory"></a>Új felhasználók vagy a Microsoft-fiókok tooAzure Active Directory felhasználók hozzáadása
Adja hozzá a felhasználók toopopulate a címtárban. Ez a cikk azt ismerteti, hogyan tooadd új felhasználókat a szervezetben, és hogyan tooadd Microsoft-fiókkal rendelkező felhasználók. Az Azure Active Directoryban a más címtárakban lévő felhasználók felvételéről vagy a partnervállalatokban lévő felhasználók felvételéről további információért lásd: [Más címtárakban vagy partnervállalatokban lévő felhasználók felvétele az Azure Active Directoryba](active-directory-create-users-external.md). A hozzáadott felhasználók alapértelmezés szerint nem rendelkeznek rendszergazdai engedélyekkel, de bármikor hozzárendelheti a szerepkörökhöz toothem.

> [!IMPORTANT]
> A Microsoft azt javasolja, hogy a hello használata az Azure AD kezelése [az Azure AD felügyeleti központban](https://aad.portal.azure.com) hello az Azure portál használata helyett hello hivatkozott ebben a cikkben a klasszikus Azure portálon. Hogyan tooadd hello Azure AD felügyeleti központban, felhasználó: a [adja hozzá az új felhasználók tooAzure Active Directory](active-directory-users-create-azure-portal.md).

## <a name="add-a-user"></a>Felhasználó hozzáadása
1. Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com) egy olyan fiókkal, amely hello címtár globális rendszergazdája.
2. Válassza ki **Active Directory**, majd válassza ki a szervezete címtárának nevét hello.
3. Jelölje be hello **felhasználók** lapot, és ezt követően hello parancssávon válassza **felhasználó hozzáadása**.
4. A hello **adja meg azt a felhasználó** lap **felhasználó típusa**, a következők közül választhat:

   * **Új felhasználó a szervezetben** – egy új felhasználói fiókot ad a címtárhoz.
   * **Egy meglévő Microsoft-fiókkal rendelkező felhasználó** – ad hozzá egy meglévő Microsoft fogyasztói fiók tooyour könyvtár (például egy Outlook-fiókot)
5. A **Felhasználó típusától** függően írjon be egy felhasználónevet (új felhasználóhoz) vagy e-mail-címet (Microsoft-fiókkal rendelkező felhasználóhoz).
6. Hello felhasználó **profil** lapján adja meg az első és utolsó nevét, egy felhasználóbarát nevet és egy felhasználói szerepkört hello **szerepkörök** listája. A felhasználói és rendszergazdai szerepkörökről további információért lásd: [Rendszergazdai szerepkörök hozzárendelése az Azure AD-ben](active-directory-assign-admin-roles.md). Adja meg, hogy túl**többtényezős hitelesítés engedélyezése** hello felhasználó számára.
7. A hello **ideiglenes jelszó beszerzése** lapon jelölje be **létrehozása**.

> [!IMPORTANT]
> Ha szervezete több tartományt használ, a felhasználói fiók hozzáadásakor a következő problémák hello kell tudnia:
>
> * felhasználói fiókok tooadd hello azonos egyszerű felhasználónév (UPN) tartományokban, **első** hozzáadni, például geoffgrisso@contoso.onmicrosoft.com, **követ** geoffgrisso@contoso.com.
> * **Ne** adja hozzá a geoffgrisso@contoso.com címet a geoffgrisso@contoso.onmicrosoft.com hozzáadása előtt. Ez a sorrend fontos, és nehézkes tooundo lehet.
>
>

## <a name="change-user-information"></a>Felhasználói adatok módosítása
Módosíthatja bármely felhasználói attribútumot, kivéve a hello objektum azonosítójával.

1. Nyissa meg a címtárat.
2. Jelölje be hello **felhasználók** fülre, majd jelölje ki hello megjelenített neve a hello toochange kívánt felhasználó.
3. Végezze el a módosításokat, majd kattintson a **Mentés** parancsra.

Ha hello épp módosított felhasználó szinkronizálva van a helyszíni Active Directory szolgáltatással, nem módosíthatja hello felhasználó adatait ezzel az eljárással. toochange hello felhasználói, használja a helyszíni Active Directory kezelőeszközöket.

## <a name="guest-user-management-and-limitations"></a>Vendégfelhasználók kezelése és korlátozásai
A Vendég felhasználókat más címtárakból, akik a meghívott tooyour directory tooaccess SharePoint-dokumentumok, alkalmazások vagy más Azure-erőforrások. A Vendég fiók a könyvtárban van, a mögöttes UserType attribútuma túl "Guest." Normál felhasználók (különösen a címtár tagjai) rendelkezik hello UserType attribútuma "Tag".

A vendégek rendelkeznek korlátozott jogok hello könyvtárban. Ezek a jogosultságok korlátozzák a vendégek toodiscover információt hello címtár más felhasználóinak hello képességét. Azonban vendégfelhasználók továbbra is kommunikálhatnak hello felhasználókhoz és csoportokhoz társított hello erőforrásokat, amelyeken dolgoznak. A vendégfelhasználók a következőkre képesek:

* Felhasználók és csoportok társított egy Azure-előfizetés toowhich hozzá vannak rendelve
* Tekintse meg a csoportok toowhich tartoznak hello tagjai
* Más felhasználók hello könyvtárban kereshet, ha ismerik hello hello felhasználó teljes e-mail címe
* Lásd: hello felhasználók megtekintése – korlátozott toodisplay nevét, e-mail címét, egyszerű felhasználónév (UPN) és a miniatűr fényképre attribútumok csak korlátozott számú
* Hello könyvtárban lévő ellenőrzött tartományok listájának beolvasása
* Hozzájárulás tooapplications számukra hello ugyanolyan szintű hozzáférése, amely a tagok rendelkeznek a címtárban

## <a name="set-guest-user-access-policies"></a>A vendégfelhasználók hozzáférési házirendjeinek beállítása
Hello **konfigurálása** címtár lapján található beállítások toocontrol a vendégfelhasználók hozzáférés. Ezek a lehetőségek csak a klasszikus Azure portálon módosíthatók a címtár globális rendszergazdája által. Jelenleg erre a célra nem létezik PowerShell- vagy API-módszer.

tooopen hello **konfigurálása** lapján hello Azure klasszikus portál, válassza ki **Active Directory**, majd válassza ki a hello hello könyvtár nevét.

![Konfigurálás lap az Azure Active Directoryban][1]

Ezután szerkesztheti a vendégfelhasználók hello beállítások toocontrol hozzáférés.

![hozzáférés-vezérlési lehetőségek a vendégfelhasználók számára][2]

## <a name="whats-next"></a>A következő lépések
* [Más címtárakban vagy partnervállalatokban lévő felhasználók felvétele az Azure Active Directoryba](active-directory-create-users-external.md)
* [Az Azure AD felügyelete](active-directory-administer.md)
* [Jelszavak kezelése az Azure AD-ben](active-directory-manage-passwords.md)
* [Csoportok kezelése az Azure AD-ben](active-directory-manage-groups.md)

<!--Image references-->
[1]: ./media/active-directory-create-users/RBACDirConfigTab.png
[2]: ./media/active-directory-create-users/RBACGuestAccessControls.png
