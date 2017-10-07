---
title: "aaaAzure Active Directory hibrid identitáskezelési elrendezésével kapcsolatos szempontok - identitás követelmények meghatározása |} Microsoft Docs"
description: "Hello a vállalat üzleti igényei vezet, hello hibrid identitás kialakításának követelményei toodefine hello azonosításához."
documentationcenter: 
services: active-directory
author: billmath
manager: femila
editor: 
ms.assetid: de690978-84ef-41ad-9dfe-785722d343a1
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: b2f1cad923b0f08ededa0d8f9a4ea8e799956e54
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="determine-identity-requirements-for-your-hybrid-identity-solution"></a>A hibrid identitáskezelési megoldás az identitás-követelmények meghatározása
hibrid identitáskezelési megoldás tervezésének első lépése hello toodetermine hello követelményei hello üzleti szervezet, amely ehhez a megoldáshoz használni fog.  Hibrid identitás (támogatott minden más felhőalapú megoldás, adja meg a hitelesítés) támogató szerepkörként elindul, és tooprovide új és érdekes képességekkel, amelyek a felhasználók új munkaterhelések feloldásához állapotba.  A munkaterhelések vagy a szolgáltatások tooadopt kívánja a felhasználók számára hello hibrid identitás kialakításának követelményei hello szabja meg.  Ezeket a szolgáltatásokat és munkaterhelések kell tooleverage hibrid identitás mind a helyszíni és felhőben hello.  

Szüksége toogo kulcsfontosságú elemeit annak hello üzleti toounderstand mi most követelmény, és milyen hello vállalat jövőbeli hello tervez. Ha nem rendelkezik hello látható-e hibrid identitás Tervező hello hosszú távú stratégiát, valószínűleg, hogy a megoldás nem lesz méretezhető hello üzleti igények növekedésének és változásának megfelelően.   T, hogy az alábbi ábrán egy példa egy hibrid identitás-architektúra és hello munkaterhelések vannak zárja nyitás alatt a felhasználók számára. Ez az csak egy példa, amely oldhatja fel, és fel egy teli hibrid identitás stratégia minden hello új lehetőségeket. 

Néhány összetevőt hello hibrid identitás architektúrájának részét képező![](./media/hybrid-id-design-considerations/hybrid-identity-architechture.png)

## <a name="determine-business-needs"></a>Az üzleti igények meghatározása
Minden vállalat különböző követelményekkel rendelkezik akkor is, ha a vállalatok hello része azonos iparágba hello valódi üzleti követelmények függően változhat. Továbbra is kihasználhatják a hello iparág ajánlott eljárásait, de végső soron vezet, hello hibrid identitás kialakításának követelményei toodefine hello hello a vállalat üzleti igényei. 

Ellenőrizze, hogy tooanswer hello következő kérdésekre tooidentify az üzleti kell:

* A vállalat informatikai működési költségek toocut keresi?
* A vállalat toosecure felhőalapú eszközök (SaaS-alkalmazásokhoz, infrastruktúra) keresi?
* A vállalat toomodernize keresése az ALKALMAZÁST?
  * Azok a felhasználók több mobil- és kibővített informatikai toocreate kivételek a DMZ tooallow különböző típusú forgalom tooaccess különböző erőforrások?
  * A vállalata rendelkezik közzétett toobe szükséges örökölt alkalmazások toothese modern felhasználók de azok nem egyszerű toorewrite?
  * Nem a vállalat tooaccomplish kell ezeket a feladatokat és hello ellenőrzés alá vonásához ugyanannyi időt vesz igénybe?
* A vállalat toosecure felhasználók identitását keresése és kockázatok csökkentése érdekében, hogy kihasználja a Microsoft Azure biztonsági szakértői helyszínen hello szakértői új eszközök?
* A vállalat távolíthatja el hello tooget próbált dreaded "external" fiókok a helyszíni, és helyezze azokat, ha azok már nem a helyszíni környezeten belül inaktív fenyegetést toohello felhő?

## <a name="analyze-on-premises-identity-infrastructure"></a>A helyszíni identitás-infrastruktúra elemzése
Most, hogy egy ötletet kapcsolatban a vállalat üzleti igényei, kell tooevaluate a helyszíni identitás-infrastruktúra. Ez a kiértékelés meghatározásához hello műszaki követelményeiben toointegrate a jelenlegi identitás megoldás toohello cloud identity felügyeleti rendszer fontos. Győződjön meg arról, hogy tooanswer hello a következő kérdéseket:

* Milyen hitelesítési és engedélyezési megoldás does a vállalat helyszíni használni? 
* Rendelkezik a vállalata jelenleg helyszíni szinkronizálási szolgáltatások?
* A vállalat használ bármely harmadik fél Identitásszolgáltatók (IdP)?

Szükség toobe tudomást hello felhőszolgáltatások, amelyek a vállalati lehet. Egy értékelési toounderstand hello aktuális integrálása SaaS hajtja végre, az adott környezetben IaaS és PaaS modellek nagyon fontos. Győződjön meg arról, hogy tooanswer hello kérdéseket az értékelés során a következő:

* Rendelkezik a vállalata a felhőbeli szolgáltatás szolgáltatója együtt?
* Ha igen, mely szolgáltatásokat használják?
* Ez az integráció jelenleg éles vagy a próbaüzem?

> [!NOTE]
> Ha nem rendelkezik egy pontos leképezése az alkalmazások és a felhőalapú szolgáltatások, használhatja a hello a Cloud App Discovery eszközt. Ez az eszköz az informatikai részleg biztosíthat a szervezet üzleti és fogyasztói felhőalkalmazások láthatósága. Amely könnyebbé teszi legalább egyszer toodiscover árnyékmásolat informatikai a szervezetben, beleértve a használati minták és a felhasználók a felhőalapú alkalmazásokhoz való hozzáférés. lépések tooget lásd [a Cloud app discovery](active-directory-cloudappdiscovery-whatis.md).
> 
> 

## <a name="evaluate-identity-integration-requirements"></a>Identity integration követelmények felmérése
A következő lépésben tooevaluate hello identitáskövetelmények integráció. Ez a kiértékelés fontos toodefine hello műszaki követelményeitől felhasználók hogyan hitelesíti magát, hello felhőben hello szervezet jelenléte megjelenését, hogyan hello szervezet engedélyezi engedélyezési és milyen hello felhasználói élmény folyamatos toobe. Győződjön meg arról, hogy tooanswer hello a következő kérdéseket:

* A szervezet használni összevonási, szabványon alapuló hitelesítési vagy mindkettő?
* Ez a követelmény az összevonási?  Miatt hello következő:
  * Kerberos-alapú egyszeri bejelentkezés
  * A vállalat egy (vagy a belső fejlesztésű vagy 3. fél beépített) a helyszíni alkalmazások, amely SAML vagy hasonló összevonási képességekkel rendelkezik.
  * Többtényezős hitelesítés az intelligens kártyák keresztül. RSA securid-titkosítást, stb.
  * Ügyfelekre vonatkozó hozzáférési szabályok a címet az alábbi hello kérdésekre:
    1. Letilthatom az összes külső hozzáférés tooOffice 365 hello hello ügyfél IP-címe alapján?
    2. Letilthatom az összes külső hozzáférés tooOffice 365, az Exchange ActiveSync kivételével?
    3. Letilthatom az összes külső hozzáférés tooOffice 365, kivéve a böngészőalapú alkalmazások (OWA, SPO)
    4. Letilthatom az összes külső hozzáférés tooOffice 365 kijelölt AD-csoportok tagjai számára
* Biztonsági vagy naplózási vonatkozik
* Az összevont hitelesítés már meglévő befektetések
* Milyen nevet a szervezet használja a tartomány hello felhőben?
* Rendelkezik a hello szervezete egyéni tartománnyal?
  1. Az adott tartomány nyilvános és könnyen ellenőrizhető DNS-rendszerben?
  2. Ha nem, majd rendelkezik, amelyek az ad-ben használt tooregister egy alternatív UPN lehetnek nyilvános tartománybeli?
* Megegyeznek a hello felhasználói azonosítók a felhő ábrázolását? 
* Hello szervezet rendelkezik a felhőalapú szolgáltatásokkal való integrációt igénylő alkalmazások?
* Rendelkezik hello szervezete több tartományt, és azok minden hitelesítést fog használni standard vagy összevont?

## <a name="evaluate-applications-that-run-in-your-environment"></a>Értékelje ki a környezetben futó alkalmazások
Most, hogy rendelkezik egy ötletet kapcsolatban a helyszíni és felhőalapú infrastruktúra, kell tooevaluate hello ezekben a környezetekben futó alkalmazást. Ez a kiértékelés fontos toodefine hello műszaki követelményeiben toointegrate ezen alkalmazások toohello felhőalapú identitás felügyeleti rendszer. Győződjön meg arról, hogy tooanswer hello a következő kérdéseket:

* Lakhelyétől fog az alkalmazások?
* Felhasználók hozzáférhetnek a helyszíni alkalmazások?  Hello felhőben? Vagy mindkét?
* Vannak-e tervek tootake hello meglévő alkalmazások és szolgáltatások, és helyezze azokat toohello felhő?
* Tervezik toodevelop új alkalmazások találhatók a helyszínen vagy a hello a felhő, amelyek felhőalapú hitelesítést fog használni?

## <a name="evaluate-user-requirements"></a>Értékelje ki a felhasználói követelmények
Akkor is tooevaluate hello felhasználói követelmények. Ez a kiértékelés fontos toodefine hello előkészítésének és segítik a felhasználókat, mivel azok átmenet toohello felhő szükséges lépéseket. Győződjön meg arról, hogy tooanswer hello a következő kérdéseket:

* Felhasználók hozzáférhetnek alkalmazásokhoz a helyszínen?
* Felhasználók hozzáférhetnek a hello felhőalapú alkalmazások?
* Honnan a felhasználók általában bejelentkezési tootheir a helyszíni környezetben?
* Hogyan fogja a felhasználók bejelentkezési toohello felhő?

> [!NOTE]
> Győződjön meg arról, hogy tootake megjegyzések minden válaszról és hello válasz hello logikája ismertetése. [Incidensválasz-követelmények meghatározása](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) hello lehetőségeit és az egyes lehetőségek előnyeit/hátrányait ismerteti.  Ezen kérdések mely leginkább megfelelő lehetőséget az üzleti megválaszolása szükséges.
> 
> 

## <a name="next-steps"></a>Következő lépések
[Címtár-szinkronizálás követelmények meghatározása](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)

## <a name="see-also"></a>Lásd még:
[Kialakítási szempontok áttekintése](active-directory-hybrid-identity-design-considerations-overview.md)

