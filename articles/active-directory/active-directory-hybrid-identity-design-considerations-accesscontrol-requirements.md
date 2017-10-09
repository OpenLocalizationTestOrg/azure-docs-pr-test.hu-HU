---
title: "aaaAzure Active Directory hibrid identitáskezelési elrendezésével kapcsolatos szempontok - határozza meg a hozzáférés-vezérlési követelményeinek |} Microsoft Docs"
description: "Magában foglalja az hello identitás és erőforrások a felhasználók hibrid környezetben azonosító való hozzáférés feltételeit."
documentationcenter: 
services: active-directory
author: billmath
manager: femila
editor: 
ms.assetid: e3b3b984-0d15-4654-93be-a396324b9f5e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: f0c22629f732a4c13ee7a24456651bec7637c387
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="determine-access-control-requirements-for-your-hybrid-identity-solution"></a>A hibrid identitáskezelési megoldás hozzáférés-vezérlési követelményeinek meghatározása
Egy szervezet tervezésekor a hibrid identitáskezelési megoldás szolgáltatást is alkalmazhatja a lehetőség tooreview elérni, hogy azok tervezési toomake hello erőforrásokkal kapcsolatos követelmények, a felhasználók. hello adatelérési közötti összes négy oszlopok identitását, amelyek:

* Adminisztráció
* Authentication
* Engedélyezés
* Naplózás

a következő szakaszok hello foglalkozunk hitelesítési és engedélyezési további részleteket, felügyeleti és a naplózás hello hibrid identitás életciklusának. Olvasási [határozza meg a hibrid identitáskezelési felügyeleti feladatok](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md) ezek képességeivel kapcsolatos további információk.

> [!NOTE]
> Olvasási [hello négy oszlopok az identitás - Identity Management szolgáltatásra hello hibrid informatikai kora](http://social.technet.microsoft.com/wiki/contents/articles/15530.the-four-pillars-of-identity-identity-management-in-the-age-of-hybrid-it.aspx) ezen oszlopok előtti mindegyiknél további információt.
> 
> 

## <a name="authentication-and-authorization"></a>Hitelesítés és engedélyezés
A hitelesítéshez és engedélyezéshez különböző forgatókönyv, ezek a forgatókönyvek, meg kell felelnie hello hibrid identitáskezelési megoldás, hogy hello vállalati-e a további folyamatos tooadopt meghatározott követelményekkel rendelkezik. Üzleti tooBusiness (B2B) érintő forgatókönyvek kommunikációs adhat hozzá egy extra kihívás az IT-rendszergazdák óta, amely hello hello szervezet által használt hitelesítési és engedélyezési módszer képes kommunikálni az üzleti partnerek tooensure van szükségük. Során hello hitelesítési és engedélyezési követelményeinek folyamatának kialakítása győződjön meg arról, a következő kérdések hello válaszok:

* Lesz a szervezet felhasználók hitelesítéséhez és engedélyezéséhez csak az identity management rendszerének helyen?
  * Vannak-e bármilyen tervek B2B forgatókönyvek esetén?
  * Ha igen, már ismeri mely protokollok (SAML, OAuth, a Kerberos, jogkivonatok vagy tanúsítványok) fog kell használt tooconnect mindkét vállalatok számára?
* Hello hibrid identitáskezelési megoldás, hogy tooadopt fog támogatja ezeket a protokollokat?

Egy másik fontos pont tooconsider, amelyben felhasználók és a partnerei által használt hello hitelesítési tárházban található és hello felügyeleti modell toobe használt. Vegye figyelembe az alábbi két alapvető beállítások hello:

* Központosított: a modell hello a felhasználói hitelesítő adatok, házirendek és felügyeleti lehet helyszínen központosított vagy hello felhőben.
* Hibrid: a modell hello felhasználói hitelesítő adatok, házirendek és felügyeleti lesz helyszínen központosított és a replikált hello felhőben.

A szervezet alkalmazott modellt fajtájától függően tootheir üzleti követelmények azt szeretné, a következő kérdések tooidentify, ahol hello identity management rendszer találhatók és felügyeleti mód toouse hello tooanswer hello:

* A szervezet jelenleg rendelkezik egy identitáskezelést a helyszínen?
  * Ha igen, akkor fogja a tookeep azt?
  * Vannak-e valamilyen szabályozás vagy megfelelőségi követelményeknek, hogy a szervezet hajtsa végre az adott megkövetel hello identitás felügyeleti rendszerre tároló kell?
* Használ a szervezete egyszeri bejelentkezésre található alkalmazásokhoz a helyszínen vagy a hello felhő?
  * Ha igen, a hibrid identitáskezelési modell hello elfogadását befolyásolja ez a folyamat?

## <a name="access-control"></a>Access Control
Hitelesítési és engedélyezési core elemek tooenable access toocorporate adatok felhasználó érvényesítési keresztül, de napjainkban is fontos toocontrol hello hozzáférési szintet, amelyet ezek a felhasználók lesz, és a rendszergazdák hello szintű hello keresztül kell ezek által kezelt erőforrások. A hibrid identitáskezelési megoldás képes tooprovide részletes hozzáférés tooresources, delegálással és a szerepköralapú hozzáférés-vezérlést kell lennie. Győződjön meg arról, hogy a következő kérdés hello válaszok vonatkozó hozzáférés-vezérlés:

* Rendelkezik a vállalata több mint egy emelt jogosultsági szintű toomanage rendelkező felhasználó az azonosítási rendszer?
  * Ha igen, nem minden felhasználónak kell hello ugyanazt a hozzáférési szint?
* A vállalati kell toodelegate hozzáférés toousers toomanage adott erőforrásokhoz volna?
  * Ha igen, milyen gyakran ez történik?
* A helyszíni és a felhő között toointegrate hozzáférés-vezérlés képességeinek kell a vállalati erőforrásokat?
* A vállalat toolimit hozzáférés tooresources toosome feltételek szerint kell?
* A vállalat rendelkezik bármely alkalmazás, amelyet a egyéni vezérlő toosome-erőforrások eléréséhez?
  * Ha igen, az alkalmazások helyét (helyszíni vagy felhőben hello)?
  * Ha igen, ahol vannak a cél lévő erőforrások (helyszíni vagy felhőben hello)?

> [!NOTE]
> Győződjön meg arról, hogy tootake megjegyzések minden válaszról és hello válasz hello logikája ismertetése. [Data Protection stratégia meghatározása](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) hello lehetőségeit és az egyes lehetőségek előnyeit és hátrányait ismerteti.  Ezen kérdések megválaszolásával kiválaszthatja az üzleti igényeinek megfelelő legjobb lehetőség.
> 
> 

## <a name="next-steps"></a>Következő lépések
[Incidensválasz-követelmények meghatározása](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md)

## <a name="see-also"></a>Lásd még:
[Kialakítási szempontok áttekintése](active-directory-hybrid-identity-design-considerations-overview.md)

