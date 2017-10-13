---
title: "Más címtárakban vagy partnervállalatokban lévő felhasználók felvétele az Azure Active Directoryba | Microsoft Docs"
description: "Ez a cikk azt ismerteti, hogyan vehet fel felhasználókat, vagy hogyan módosíthatja a felhasználói adatokat az Azure Active Directoryban, beleértve a külső és vendégfelhasználókat is."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 564a04ec-53c1-470b-9ab9-f3db57da0a89
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/10/2017
ms.author: curtand
ms.custom: oldportal;it-pro;
robots: NOINDEX
ms.openlocfilehash: 30dbd21c6d21aa55c2c7e84c315fba098674ddd8
ms.sourcegitcommit: 51ea178c8205726e8772f8c6f53637b0d43259c6
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/11/2017
---
# <a name="add-users-from-other-directories-or-partner-companies-in-azure-active-directory"></a>Más címtárakban vagy partnervállalatokban lévő felhasználók felvétele az Azure Active Directoryba

Ez a cikk azt ismerteti, hogyan vehet fel felhasználókat más címtárakból az Azure Active Directoryba, illetve hogyan vehet fel felhasználókat a partnervállalatokból. A szervezetben új felhasználók hozzáadásáról és a Microsoft-fiókokkal rendelkező felhasználók hozzáadásáról további információért lásd: [Új felhasználók felvétele az Azure Active Directoryba](active-directory-create-users.md). 

> [!IMPORTANT]
> A Microsoft javasolja, hogy az Azure Portalon található [Azure AD felügyeleti központból](https://aad.portal.azure.com) kezelje az Azure AD-t az ebben a cikkben javasolt klasszikus Azure portál helyett. Az Azure AD felügyeleti központban B2B együttműködési vendégfelhasználók hozzáadásáról további információkért lásd: [Mi az az Azure AD B2B együttműködés?](active-directory-b2b-what-is-azure-ad-b2b.md)

A hozzáadott felhasználók alapértelmezés szerint nem rendelkeznek rendszergazdai engedélyekkel, de bármikor hozzájuk rendelhet szerepköröket.

## <a name="add-a-user"></a>Felhasználó hozzáadása
1. Jelentkezzen be a [klasszikus Azure portálra](https://manage.windowsazure.com) egy olyan fiókkal, amely a címtár globális rendszergazdája.
2. Válassza az **Active Directory** lehetőséget, majd nyissa meg a címtárat.
3. Válassza a **Felhasználók** lapot, majd a parancssávon válassza a **Felhasználó hozzáadása** parancsot.
4. A **Felhasználó bemutatása** lap **Felhasználó típusa** területén válassza a következők egyikét:

   * **Felhasználó egy másik Azure AD-címtárban** – olyan felhasználói fiókot ad a címtárhoz, amely másik Azure AD-címtárból származik. Csak akkor választhatja ki egy másik címtár felhasználóját, ha azon címtárnak is a tagja.
   * **Felhasználók partnervállalatokban** – a partnervállalatok felhasználóinak meghívásához és engedélyezéséhez (lásd: [Azure Active Directory B2B együttműködés](active-directory-b2b-what-is-azure-ad-b2b.md)). [Fel kell töltenie egy CSV-fájlt, amely megadja az e-mail-címeket](active-directory-b2b-references-csv-file-format.md).
5. A felhasználói **Profil** oldalon adjon meg egy kereszt- és egy vezetéknevet, egy rövid nevet és egy felhasználói szerepkört a **Szerepkörök** listából. A felhasználói és rendszergazdai szerepkörökről további információért lásd: [Rendszergazdai szerepkörök hozzárendelése az Azure AD-ben](active-directory-assign-admin-roles.md). Adja meg, hogy **engedélyezi-e a többtényezős hitelesítést** a felhasználó számára.
6. Az **Ideiglenes jelszó beszerzése** oldalon válassza a **Létrehozás** lehetőséget.

> [!IMPORTANT]
> Ha a szervezete több tartományt használ, a következő problémákról kell tudnia a felhasználói fiók hozzáadásakor:
>
> * Ha azonos egyszerű felhasználónévvel (UPN) szeretne felhasználói fiókokat felvenni több tartományba, **először** adja hozzá például a geoffgrisso@contoso.onmicrosoft.com,  **címet, majd a**  geoffgrisso@contoso.com címet.
> * **Ne** adja hozzá a geoffgrisso@contoso.com címet a geoffgrisso@contoso.onmicrosoft.com hozzáadása előtt.
>

Ha módosítja olyan felhasználó adatait, akinek identitása szinkronizálva van a helyszíni Active Directory szolgáltatással, nem módosíthatja a felhasználó adatait a klasszikus Azure portálon. A felhasználói adatok módosításához használja a helyszíni Active Directory-felügyeleti eszközöket.

## <a name="add-external-users"></a>Külső felhasználók felvétele
Másik olyan Azure AD-címtárból is felvehet felhasználókat, amelyeknek a tagja, vagy megteheti ugyanezt egy CSV-fájl feltöltésével, ha a partnervállalatokból vesz fel felhasználókat. Külső felhasználó hozzáadásához a **Felhasználó típusa** alatt adja meg a **Felhasználó egy másik Microsoft Azure AD-címtárban** vagy a **Felhasználók partnervállalatokban** beállítást.

Mindkét felhasználótípust egy másik címtárból adja hozzá **külső felhasználóként**. A külső felhasználók együttműködhetnek más címtár-felhasználókkal anélkül, hogy új fiókokat és hitelesítő adatokat kellene felvenni. A külső felhasználók a saját címtárukkal végezhetnek hitelesítést amikor bejelentkeznek, és ez a hitelesítés bármely olyan címtárnál működik, amelyhez hozzá lettek adva.

## <a name="external-user-management-and-limitations"></a>Külső felhasználók kezelése és korlátozásai
Amikor egy másik címtárból ad hozzá felhasználót a címtárához, ez a felhasználó külső felhasználónak számít a címtárában. A megjelenített nevet és a felhasználónevet a rendszer kimásolja a saját címtárukból, és a külső felhasználóhoz adja a címtárában. Ettől kezdve a külső felhasználói fiók tulajdonságai teljesen függetlenek. Ha a saját címtárban módosulnak a felhasználó tulajdonságai, ezek a módosítások nem tükröződnek majd a címtárában lévő külső felhasználói fiókban.

A két fiók között az egyetlen kapcsolat, hogy a felhasználó mindig a saját címtárával vagy a Microsoft-fiókjával végzi a hitelesítést. Ezért nem lát olyan lehetőséget, amellyel alaphelyzetbe állíthatná a jelszót, vagy engedélyezhetné a többtényezős hitelesítést a külső felhasználók számára. Jelenleg a saját címtár vagy a Microsoft-fiók hitelesítési házirendje az egyetlen, amelyet a rendszer kiértékel a felhasználó bejelentkezésekor.

> [!NOTE]
> Továbbra is letilthatja a külső felhasználót a címtárban, amivel blokkolja a hozzáférését a címtárhoz.
>
>

Ha a felhasználót törlik a saját címtárában, vagy ha törlik a Microsoft-fiókját, a külső felhasználó továbbra is létezik a címtárában. De a címtárában lévő felhasználó nem éri el az erőforrásokat, mert nem tudja hitelesíteni magát a saját címtárával vagy Microsoft-fiókjával.

### <a name="services-that-currently-support-access-by-azure-ad-external-users"></a>Az Azure AD külső felhasználóinak hozzáférését jelenleg támogató szolgáltatások
* **Klasszikus Azure portál:** lehetővé teszi, hogy egy több címtár rendszergazdájaként eljáró külső felhasználó kezelje ezeket a címtárakat.
* **SharePoint Online:** ha a külső megosztás engedélyezett, lehetővé teszi, hogy a külső felhasználók elérjék a SharePoint Online engedélyezett erőforrásait.
* **Dynamics CRM:** ha a felhasználó PowerShellen keresztül kapott licencet, lehetővé teszi, hogy a külső felhasználó elérje az engedélyezett erőforrásokat a Dynamics CRM-ben.
* **Dynamics AX:** ha a felhasználó PowerShellen keresztül kapott licencet, lehetővé teszi, hogy a külső felhasználó elérje az engedélyezett erőforrásokat a Dynamics AX-ben. Az [Azure AD külső felhasználóira](#known-limitations-of-azure-ad-external-users) vonatkozó korlátozások a Dynamics AX külső felhasználóira is érvényesek.

## <a name="next-steps"></a>Következő lépések
* [Új felhasználók hozzáadása az Azure Active Directoryhoz](active-directory-create-users.md)
* [Az Azure AD felügyelete](active-directory-administer.md)
* [Jelszavak kezelése az Azure AD-ben](active-directory-manage-passwords.md)
* [Csoportok kezelése az Azure AD-ben](active-directory-manage-groups.md)
