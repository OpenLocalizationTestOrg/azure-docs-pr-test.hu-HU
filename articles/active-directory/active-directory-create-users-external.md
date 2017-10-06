---
title: "más címtárakban vagy partnervállalatokban az Azure Active Directoryban aaaAdd felhasználók |} Microsoft Docs"
description: "Azt ismerteti, hogyan tooadd felhasználók vagy felhasználói adatokat az Azure Active Directoryban, beleértve a külső és vendégfelhasználókat is módosíthatja."
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
ms.date: 07/25/2017
ms.author: curtand
ms.custom: oldportal;it-pro;
robots: NOINDEX
ms.openlocfilehash: 92099e5792365c307b0f3d4f2dff5dd8424aeab4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-users-from-other-directories-or-partner-companies-in-azure-active-directory"></a>Más címtárakban vagy partnervállalatokban lévő felhasználók felvétele az Azure Active Directoryba

Ez a cikk azt ismerteti, hogyan tooadd felhasználókat más címtárakból az Azure Active Directoryban, vagy adja hozzá a felhasználókat a partnervállalatokból. Új felhasználók hozzáadása a szervezetében, és a Microsoft-fiókkal rendelkező felhasználók hozzáadásával kapcsolatos további információkért lásd: [adja hozzá az új felhasználók tooAzure Active Directory](active-directory-create-users.md). 

> [!IMPORTANT]
> A Microsoft azt javasolja, hogy a hello használata az Azure AD kezelése [az Azure AD felügyeleti központban](https://aad.portal.azure.com) hello az Azure portál használata helyett hello hivatkozott ebben a cikkben a klasszikus Azure portálon. Hogyan tooadd B2B együttműködés vendégfelhasználók hello Azure AD felügyeleti központ, lásd: [Mi az az Azure AD B2B együttműködés?](active-directory-b2b-what-is-azure-ad-b2b.md)

A hozzáadott felhasználók alapértelmezés szerint nem rendelkeznek rendszergazdai engedélyekkel, de bármikor hozzárendelheti a szerepkörökhöz toothem.

## <a name="add-a-user"></a>Felhasználó hozzáadása
1. Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com) egy olyan fiókkal, amely hello címtár globális rendszergazdája.
2. Válassza az **Active Directory** lehetőséget, majd nyissa meg a címtárat.
3. Jelölje be hello **felhasználók** lapot, és ezt követően hello parancssávon válassza **felhasználó hozzáadása**.
4. A hello **adja meg azt a felhasználó** lap **felhasználó típusa**, a következők közül választhat:

   * **Egy másik Azure AD-címtár felhasználó** – felhasználói fiók tooyour könyvtár megosztásban hozzáadja egy másik Azure AD-címtárból. Csak akkor választhatja ki egy másik címtár felhasználóját, ha azon címtárnak is a tagja.
   * **Felhasználók partnervállalatokban** -tooinvite és erőforráspartner vállalati felhasználók tooyour directory engedélyezéséhez (lásd: [Azure Active Directory B2B együttműködés](active-directory-b2b-what-is-azure-ad-b2b.md)). Szüksége lesz túl[megadása az e-mail címek CSV-fájl feltöltése](active-directory-b2b-references-csv-file-format.md).
5. Hello felhasználó **profil** lapján adja meg az első és utolsó nevét, egy felhasználóbarát nevet és egy felhasználói szerepkört hello **szerepkörök** listája. A felhasználói és rendszergazdai szerepkörökről további információért lásd: [Rendszergazdai szerepkörök hozzárendelése az Azure AD-ben](active-directory-assign-admin-roles.md). Adja meg, hogy túl**többtényezős hitelesítés engedélyezése** hello felhasználó számára.
6. A hello **ideiglenes jelszó beszerzése** lapon jelölje be **létrehozása**.

> [!IMPORTANT]
> Ha szervezete több tartományt használ, a felhasználói fiók hozzáadásakor a következő problémák hello kell tudnia:
>
> * felhasználói fiókok tooadd hello azonos egyszerű felhasználónév (UPN) tartományokban, **első** hozzáadni, például geoffgrisso@contoso.onmicrosoft.com, **követ** geoffgrisso@contoso.com.
> * **Ne** adja hozzá a geoffgrisso@contoso.com címet a geoffgrisso@contoso.onmicrosoft.com hozzáadása előtt.
>

Ha módosítja olyan felhasználó, akinek identitása szinkronizálva van a helyszíni Active Directory szolgáltatással adatait, nem módosíthatja a klasszikus Azure portálon hello hello felhasználó adatait. toochange hello felhasználói adatokat, használja a helyszíni Active Directory kezelőeszközöket.

## <a name="add-external-users"></a>Külső felhasználók felvétele
Is hozzáadhat felhasználókat egy másik Azure AD directory toowhich Ön is a tagja, vagy a partnervállalatokban CSV-fájl feltöltésével. a külső felhasználó tooadd a **felhasználó típusa**, adja meg **felhasználó egy másik Microsoft Azure AD-címtárban** vagy **felhasználók partnervállalatokban**.

Mindkét felhasználótípust egy másik címtárból adja hozzá **külső felhasználóként**. Külső felhasználók együttműködhetnek más követelmény tooadd új fiókokat és hitelesítő adatok nélkül címtár-felhasználókkal. Külső felhasználók hitelesítéséhez a saját címtárukkal, amikor bejelentkeznek, és más címtárak toowhich vannak adva működik, ha a hitelesítés.

## <a name="external-user-management-and-limitations"></a>Külső felhasználók kezelése és korlátozásai
Directory tooyour másik címtárban szereplő hozzáadni egy felhasználót, amikor a felhasználó egy külső felhasználónak számít a címtárában. hello megjelenítési nevet és egy felhasználónév másolta a saját címtárukból, és használt hello külső felhasználónak számít a címtárában. Ettől hello külső felhasználói fiók tulajdonságai teljesen függetlenek. Ha tulajdonság módosításai toohello felhasználói a saját címtárában, ezek a módosítások nem kerülnek toohello külső felhasználói fiókot a címtárban.

hello egyetlen kapcsolat hello két fiók között, hogy hello felhasználó mindig hitelesíti magát a saját címtárával vagy a Microsoft-fiókkal. Ezért ne tekintse meg a beállítás tooreset hello jelszavát, vagy külső felhasználó többtényezős hitelesítés engedélyezése. Hello saját címtárával vagy Microsoft-fiók hitelesítési házirendje hello jelenleg csak az egyik hello felhasználó bejelentkezésekor kiértékelt hello.

> [!NOTE]
> Külső felhasználói hello hello könyvtárban, amely megakadályozza a hozzáférés a címtárhoz tooyour továbbra is letilthatja.
>
>

Ha a felhasználót törlik a saját címtárában, vagy törlik a saját Microsoft-fiókjával, hello külső felhasználó továbbra is létezik a címtárában. Azonban a címtárában lévő hello felhasználó nem tud hozzáférni erőforrások, mert nem tudja hitelesíteni magát a saját címtárával vagy Microsoft-fiókkal.

### <a name="services-that-currently-support-access-by-azure-ad-external-users"></a>Az Azure AD külső felhasználóinak hozzáférését jelenleg támogató szolgáltatások
* **A klasszikus Azure portálon**: egy külső felhasználó, aki rendszergazdai jogosultságokkal rendelkező több könyvtárak toomanage minden ezeket a címtárakat.
* **SharePoint Online**: Ha a külső megosztás engedélyezve van, lehetővé teszi, hogy egy külső felhasználó tooaccess SharePoint Online engedélyezett erőforrásait.
* **Dynamics CRM**: hello felhasználói PowerShell licencét szerezte meg, ha lehetővé teszi, hogy egy külső felhasználó tooaccess engedélyezett erőforrásokat a Dynamics CRM-ben.
* **Dynamics AX**: hello felhasználói PowerShell licencét szerezte meg, ha lehetővé teszi, hogy egy külső felhasználó tooaccess engedélyezett erőforrásokat a Dynamics AX. hello vonatkozó korlátozások [az Azure AD külső felhasználóinak](#known-limitations-of-azure-ad-external-users) tooexternal felhasználókat a Dynamics AX is vonatkoznak.

## <a name="next-steps"></a>Következő lépések
* [Adja hozzá az új felhasználók tooAzure Active Directory](active-directory-create-users.md)
* [Az Azure AD felügyelete](active-directory-administer.md)
* [Jelszavak kezelése az Azure AD-ben](active-directory-manage-passwords.md)
* [Csoportok kezelése az Azure AD-ben](active-directory-manage-groups.md)
