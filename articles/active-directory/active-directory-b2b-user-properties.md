---
title: "egy Azure Active Directory B2B együttműködés felhasználó aaaProperties |} Microsoft Docs"
description: "Az Azure Active Directory B2B együttműködés felhasználó tulajdonságainak konfigurálható"
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 05/25/2017
ms.author: sasubram
ms.openlocfilehash: 78709f64430ed4c14eadf4dc257f175c24698c5e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="properties-of-an-azure-active-directory-b2b-collaboration-user"></a>Egy Azure Active Directory B2B együttműködés felhasználó tulajdonságai

Az Azure Active Directory (Azure AD) üzleti vállalatközi (B2B) együttműködés felhasználó tulajdonképpen UserType rendelkező felhasználó Vendég =. A Vendég felhasználói általában egy fiókpartner-szervezet származik, és csak korlátozott jogosultságokkal hello meghívása könyvtár, alapértelmezés szerint a.

Attól függően, hogy a szervezet igényeinek megfelelően meghívása hello az Azure AD B2B együttműködés felhasználói lehet a következő fiók állapotok hello egyikében:

- 1. állapot: Az Azure AD külső példányának a következő helyen, és felkéri a szervezet hello Vendég felhasználói képviseli. Ebben az esetben hello B2B felhasználó jelentkezik be egy Azure AD fiókkal, amely a meghívott toohello bérlőhöz tartozik. Hello fiókpartner-szervezet nem használ az Azure AD, ha az Azure AD hello Vendég felhasználó továbbra is létrejön. hello követelményei, hogy azok beváltani a meghívót, és az Azure AD ellenőrzi az e-mail címet. Ezzel az elrendezéssel egy közvetlenül az igény (szerinti JIT) bérlős és a "ugrásszerű" bérlőhöz is nevezik.

- 2. állapot: A Microsoft-fiókkal a következő helyen, és a Vendég felhasználó hello állomás szervezet-ként. Ebben az esetben hello Vendég felhasználó egy Microsoft-fiókkal jelentkezik be. hello meghívott felhasználó közösségi identitása (google.com vagy hasonló), amely nincs Microsoft-fiókja, amely a Microsoft-fiók során létrejön ajánlat érvényesítési.

- 3. állapot: Hello állomás szervezet helyi Active Directoryban a következő helyen, és szinkronizálja a hello állomás szervezet Azure AD. Ebben a kiadásban során hello felhőben PowerShell toomanually módosítás hello az ilyen felhasználók UserType kell használnia.

- 4. állapot: Az állomás szervezet Azure-ban a következő helyen UserType az AD = Vendég és a hitelesítő adatokat, amelyek hello állomás szervezetem kezeli.

  ![Hello meghívó monogramja megjelenítése](media/active-directory-b2b-user-properties/redemption-diagram.png)


Most nézzük meg, mi az Azure AD B2B együttműködés felhasználói állapot az 1. a következőképpen néz Azure AD-ben.

### <a name="before-invitation-redemption"></a>Meghívót visszaváltás előtt

![Az ajánlat visszaváltás előtt](media/active-directory-b2b-user-properties/before-redemption.png)

### <a name="after-invitation-redemption"></a>Meghívót érvényesítési után

![Az ajánlat érvényesítési után](media/active-directory-b2b-user-properties/after-redemption.png)

## <a name="key-properties-of-hello-azure-ad-b2b-collaboration-user"></a>Kulcstulajdonságok hello Azure AD B2B együttműködés felhasználó
### <a name="usertype"></a>UserType
Ez a tulajdonság azt jelzi, hogy hello felhasználói toohello állomás bérleti hello kapcsolata. Ez a tulajdonság két értékeket veheti fel:
- Tag: Ez az érték azt jelzi, egy alkalmazott hello állomás szervezet és a hello szervezet Bérlista-felhasználóhoz. Ez a felhasználó például toohave hozzáférés csak toointernal helyek vár. Ez a felhasználó nem tekinthető egy külső közreműködő.

- Vendég: Ez az érték azt jelzi, a felhasználó, aki belső toohello vállalati, például egy külső közreműködő, partner, ügyfél vagy hasonló felhasználói nem számít. Ilyen felhasználói nem várt tooreceive a Vezérigazgató belső emlékeztető, vagy vállalati előnyeit, például kap.

  > [!NOTE]
  > hello UserType rendelkezik nincs kapcsolat toohow hello felhasználó jelentkezik be, a hello directory szerepkör hello felhasználó, és így tovább. Ez a tulajdonság egyszerűen hello felhasználói kapcsolat toohello állomás szervezet azt jelzi, és lehetővé teszi, hogy a szervezet hello tooenforce házirendek, amelyek ennek a tulajdonságnak függenek.

### <a name="source"></a>Forrás
Ez a tulajdonság azt jelzi, hogyan hello felhasználó bejelentkezik.

- A meghívott felhasználó: Ez a felhasználó kérték, de még nem beváltott meghívót.

- Külső Active Directory: Ez a felhasználó a külső szervezetek van a következő helyen, és az Azure AD-fiókot toohello másik szervezethez tartozó használatával. Ez a fajta bejelentkezési tooState 1 felel meg.

- Microsoft-fiók: Ez a felhasználó van a Microsoft-fiókkal a következő helyen, és a Microsoft-fiók használatával. Ez a fajta bejelentkezési tooState 2 felel meg.

- Windows Server Active Directory: A bejelentkezett felhasználó a helyi Active Directoryból, amely toothis szervezet tartozik. Ez a fajta bejelentkezési tooState 3 felel meg.

- Az Azure Active Directory: A felhasználó hitelesíti az Azure AD-fiókot, amelyhez tartozik toothis szervezet számára. Ez a fajta bejelentkezési tooState 4 felel meg.
  > [!NOTE]
  > Forrás- és UserType is független tulajdonságait. A forrás érték nem feltétlenül jelenti azt egy adott értéket a UserType.

## <a name="can-azure-ad-b2b-users-be-added-as-members-instead-of-guests"></a>Azure AD B2B felhasználók felveheti tagként vendégek helyett?
Általában az Azure AD B2B felhasználót és a Vendég felhasználó is megfelel. Ezért az Azure AD B2B együttműködés felhasználói meg van adva egy UserType rendelkező felhasználó Vendég = alapértelmezés szerint. Néhány esetben azonban hello fiókpartner-szervezet egy nagyobb szervezet toowhich hello állomás szervezete egy másik is tartozik. Ha igen, a hello állomás szervezet szeretnénk tootreat felhasználó hello fiókpartner-szervezet tagként vendégek helyett. Hello Azure AD B2B meghívó Manager API-k tooadd használja, vagy egy felhasználó hello partner szervezet toohello állomás szervezet tagként meghívása.

## <a name="filter-for-guest-users-in-hello-directory"></a>A vendégfelhasználók hello könyvtárban szűrő

![Szűrő vendégfelhasználók számára](media/active-directory-b2b-user-properties/filter-guest-users.png)

## <a name="convert-usertype"></a>Alakítsa át UserType
Jelenleg akkor lehet felhasználók tooconvert UserType tag tooGuest és viszont a PowerShell használatával. Hello UserType tulajdonság azonban toorepresent hello felhasználói kapcsolat toohello szervezet kellene. Ezért a tulajdonság értékének hello kell módosítani, csak akkor, ha hello felhasználói toohello szervezet hello kapcsolat megváltozik. Hello kapcsolat hello felhasználó módosítja, ha a probléma, például hogy hello egyszerű felhasználónév (UPN) kell módosítani, foglalkozzon? Hello felhasználó továbbra is toohave hozzáférés toohello ugyanazokat az erőforrásokat? Egy postaláda rendelhető? Hello UserType módosítása a powershellel atomi tevékenységként, ezért nem javasoljuk. Ezenkívül abban az esetben, ha ezt a tulajdonságot a PowerShell használatával válik nem módosítható, nem ajánlott véve egy függőséget meg ezt az értéket.

## <a name="remove-guest-user-limitations"></a>Távolítsa el a Vendég felhasználói korlátozásai
Előfordulhat, ahová toogive a Vendég magasabb szintű felhasználói jogosultságokkal. Vegye fel a Vendég felhasználói tooany szerepkört, és távolíthatnak el hello alapértelmezett Vendég felhasználói korlátozások a hello directory toogive egy felhasználó hello azonos jogosultsággal tagként.

Már lehetséges tooturn hello alapértelmezett Vendég felhasználó korlátozások ki, hogy a vállalati címtárban hello Vendég felhasználó kap hello ugyanazokkal az engedélyekkel tag felhasználóként.

![Távolítsa el a Vendég felhasználói korlátozásai](media/active-directory-b2b-user-properties/remove-guest-limitations.png)

## <a name="next-steps"></a>Következő lépések

Ismerje meg az Azure AD B2B együttműködés további cikkeit:

* [Mi az az Azure AD B2B együttműködés?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [B2B együttműködés felhasználói tooa szerepkör hozzáadása](active-directory-b2b-add-guest-to-role.md)
* [B2B együttműködés meghívókat delegálása](active-directory-b2b-delegate-invitations.md)
* [Naplózási és jelentéskészítési B2B együttműködés felhasználó](active-directory-b2b-auditing-and-reporting.md)
* [Dinamikus csoportok és a B2B együttműködés](active-directory-b2b-dynamic-groups.md)
* [B2B együttműködés kód és a PowerShell-példák](active-directory-b2b-code-samples.md)
* [B2B együttműködés SaaS-alkalmazások konfigurálása](active-directory-b2b-configure-saas-apps.md)
* [B2B együttműködés felhasználói jogkivonatokhoz](active-directory-b2b-user-token.md)
* [B2B együttműködés felhasználói jogcímek leképezése](active-directory-b2b-claims-mapping.md)
* [Külső Office 365-megosztás](active-directory-b2b-o365-external-user.md)
* [B2B együttműködés aktuális korlátozásai](active-directory-b2b-current-limitations.md)
