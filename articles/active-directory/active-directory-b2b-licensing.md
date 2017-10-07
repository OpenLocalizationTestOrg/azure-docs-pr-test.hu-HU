---
title: "útmutatás licencelési aaaAzure Active Directory B2B együttműködés |} Microsoft Docs"
description: "Az Azure Active Directory B2B együttműködés nem igényel fizetős Azure AD-licenccel, de is is beolvasása fizetett funkciók a B2B vendégfelhasználók számára"
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
ms.date: 08/09/2017
ms.author: curtand
ms.reviewer: sasubram
ms.custom: it-pro
ms.openlocfilehash: 8e15d66209b090bff210674ecdacc88cd642dcc0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2b-collaboration-licensing-guidance"></a>Útmutató az Azure Active Directory vállalatközi együttműködés licenceléséhez

Is használhatja Azure AD B2B együttműködés képességek tooinvite vendégfelhasználók az Azure AD-bérlő tooallow azokat Azure AD szolgáltatásokba tooaccess és egyéb erőforrásokat a szervezetben. Ha azt szeretné, hogy tooprovide hozzáférési toopaid az Azure AD-funkciókat, a B2B együttműködés vendégfelhasználók licenccel kell rendelkezniük a megfelelő Azure AD licencet. 

Konkrétan:
* Az Azure AD ingyenes képességek érhetők el a vendégfelhasználók külön engedélye nélkül.
* Ha azt szeretné, hogy tooprovide hozzáférés toopaid az Azure AD szolgáltatások tooB2B felhasználók, a B2B vendég felhasználók elég licencek toosupport kell rendelkeznie.
* Az hívja fel egy licencet a fizetős Azure ad-bérlő tartozik B2B együttműködés használata jogok tooan további öt B2B Vendég meghívott felhasználók toohello bérlő.
* Bérlői meghívása hello birtokló hello ügyfél hello egy toodetermine B2B együttműködés hány felhasználó kell fizetni az Azure AD-lehetőségek kell lennie. Attól függően, hogy a fizetős Azure AD hello szolgáltatásokra van szükség a vendég felhasználók rendelkeznie kell ahhoz az Azure AD fizetett licencek toocover B2B együttműködés felhasználók hello azonos 5:1 arányban.

A B2B együttműködés Vendég felhasználó meg van adva egy felhasználó egy partnervállalatnak, nem egy alkalmazott a szervezet vagy egy alkalmazott a konglomerátum különböző üzleti. A B2B Vendég felhasználó is jelentkezzen be a külső hitelesítő adatokat vagy a hitelesítő adatokat a szervezet által birtokolt, ebben a cikkben leírtak szerint. 

Más szóval B2B licencelési értéke nem hello felhasználók hitelesítése hogyan történjen, hanem inkább hello kapcsolat hello felhasználói tooyour szervezet. Ha ezek a felhasználók nem partnerek, ezeket kell kezelni eltérően a licencelési időszakonként. Nem tekinthetők a B2B együttműködés felhasználó toobe licenckezelési célokból, akkor is, ha azok UserType attribútuma "Vendég". Ezek engedéllyel kell rendelkeznie általában, minden felhasználóhoz egy-egy licencet. Ezek a felhasználók a következők:
* Az alkalmazottak
* A személyzet külső identitásokkal bejelentkezés
* Egy alkalmazott a konglomerátum különböző üzleti


## <a name="licensing-examples"></a>Licencelési példák
- Az ügyfél szeretne tooinvite 100 B2B együttműködés felhasználók tooits Azure AD-bérlő. hello ügyfél rendeli hozzá a hozzáférés-kezelés és a kiépítés az összes felhasználó számára, de 50 felhasználóinak is kell-e többtényezős hitelesítés és a feltételes hozzáférés. Ez az ügyfél kell vásárolnia, 10 Azure AD alapvető licencek és 10 Azure AD Premium P1 licencek toocover ezen B2B felhasználók megfelelően. Hello ügyfél toouse Identity Protection-szolgáltatások tervek B2B felhasználóival, rendelkeznie kell Azure AD Premium P2 licencek toocover hello meghívott felhasználók hello az azonos 5:1 arányban.
- Az ügyfél 10 alkalmazottak, akik az Azure AD Premium P1 jelenleg licencét rendelkezik. Most szeretné tooinvite 60 B2B felhasználók számára többtényezős hitelesítést (MFA). Hello 5:1 engedélyezési szabály alapján hello ügyfél rendelkeznie kell legalább 12 Azure AD Premium P1 licencek toocover 60 B2B együttműködés felhasználók. Mert az már 10 Premium P1 licencek 10 az alkalmazottak, jogok tooinvite 50 B2B felhasználók Premium P1 szolgáltatások, mint az MFA rendelkeznek. Ebben a példában, majd azok kell 2 további Premium P1 licencek vásárlása toocover hello fennmaradó 10 B2B együttműködés felhasználók.

> [!NOTE]
> Még tooassign licencek közvetlenül toohello B2B felhasználók tooenable e B2B együttműködés jogokkal, nincs lehetőség.

Bérlői meghívása hello birtokló hello ügyfél hello egy toodetermine B2B együttműködés hány felhasználó kell fizetni az Azure AD-lehetőségek kell lennie. Attól függően, amelyek fizetős Azure AD-funkciókat a vendégfelhasználók használni szeretne elég az Azure AD licencet toocover B2B együttműködés felhasználók fizetett hello 5:1 arányban kell rendelkeznie. 

## <a name="additional-licensing-details"></a>További licencelési részletei
- Nincs szükség tooactually hozzárendelése licencek tooB2B felhasználói fiókokat. Hello 5:1 arányban alapján, licencelési automatikusan kiszámítása és jelenteni.
- Van-e nem fizetős Azure AD licencet hello bérlői, minden a meghívott felhasználó lekérdezi hello jogoktól hello Azure AD ingyenes kiadás ajánlatokat.
- Ha a B2B együttműködés a felhasználónak már van egy fizetős Azure AD a szervezet licencszerződés, nem használják ki a hello B2B együttműködés licencek hello hívja fel bérlő egyikét.

## <a name="advanced-discussion-what-are-hello-licensing-considerations-when-we-add-users-from-a-conglomerate-organization-as-members-using-your-apis"></a>Speciális vitafórum: hello kapcsolatos Mik amikor azt felhasználók hozzáadása az API-k "tagjai" konglomerátum szervezetből?
A B2B Vendég felhasználó egy, a a fiókpartner szervezet toowork hello állomás szervezettel felkérik. Általában minden más esetben nem minősül B2B még azt B2B funkciókat használja. Nézzük két esetben különösen:

1. Ha egy állomás meghívja az alkalmazott fogyasztói címet használ
  * Ez a forgatókönyv nem megfelel az engedélyezési házirendeket, és nem ajánlott.

2. Ha egy állomás szervezet hozzáad egy felhasználót egy másik konglomerátum szervezet
  1. Ebben az esetben hello felhasználói felkérik B2B API-k használatával, de ebben az esetben nincs hagyományosan B2B. Ideális esetben rendelkezik szervezetek számára a meghívás más szervezethez felhasználók hello (az API felületen lehetővé teszi, hogy) tagjai. Ebben az esetben a licencek toothese tagok számukra a szervezet meghívása hello tooaccess erőforrások hozzárendelt toobe rendelkezik.

  2. Egyes szervezetek egyéb szervezeti felhasználók toobe fel, mert "Vendég" házirend-érdemes tooadd hello. Két olyan eset létezik itt:
      * hello konglomerátum munkahelyén már működik a Azure AD és hello meghívott felhasználók licencét hello a másik szervezet: Ebben az esetben nem várhatóan meghívott felhasználók tooneed toofollow hello 1:5 képlet a jelen dokumentum korábbi leírva. 

      * hello konglomerátum szervezete nem használja az Azure AD, vagy nem rendelkezik megfelelő licencek: Ebben az esetben kövesse a jelen dokumentum korábbi elrendezésének 1:5 képlet hello.

## <a name="next-steps"></a>Következő lépések

Ismerje meg az Azure AD B2B együttműködés további cikkeit:

* [Mi az az Azure AD B2B együttműködés?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Hogyan rendszergazdák Azure Active Directory B2B együttműködés felhasználók hozzá?](active-directory-b2b-admin-add-users.md)
* [Hogyan hozzá az információkkal dolgozó szakemberek B2B együttműködés felhasználók?](active-directory-b2b-iw-add-users.md)
* [hello B2B együttműködés meghívó e-mail hello elemei](active-directory-b2b-invitation-email.md)
* [B2B együttműködés meghívó érvényesítési](active-directory-b2b-redemption-experience.md)
* [Hibaelhárítás az Azure Active Directory B2B együttműködés](active-directory-b2b-troubleshooting.md)
* [Az Azure Active Directory B2B együttműködés gyakori kérdések (GYIK)](active-directory-b2b-faq.md)
* [Az Azure Active Directory B2B együttműködés API és a Testreszabás](active-directory-b2b-api.md)
* [Többtényezős hitelesítés a B2B-együttműködés felhasználói számára](active-directory-b2b-mfa-instructions.md)
* [Adja hozzá a B2B együttműködés felhasználók nélkül](active-directory-b2b-add-user-without-invite.md)
* [Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke](active-directory-apps-index.md)
