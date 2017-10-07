---
title: "aaaAzure Active Directory hibrid identitáskezelési elrendezésével kapcsolatos szempontok - tartalomkezelési követelmények meghatározása |} Microsoft Docs"
description: "Hogyan toodetermine hello a vállalat követelményeinek tartalomkezelési betekintést nyújt. Általában amikor egy felhasználó rendelkezik-e a saját eszköz ő lehet is több hitelesítő adatokat, amelyek függően, hogy használó toohello alkalmazás váltakozó lesz. Fontos toodifferentiate is, hogy mely tartalmak lett létrehozva, és a vállalati hitelesítő adatok használatával létrehozott ők hello személyes hitelesítő adataival. A megoldást kell a felhőalapú szolgáltatások tooprovide képes toointeract egy zökkenőmentes élményt toohello végfelhasználói közben az adatvédelem biztosításához és az adatszivárgás elleni hello növelhető."
documentationcenter: 
services: active-directory
author: billmath
manager: femila
editor: 
ms.assetid: dd1ef776-db4d-4ab8-9761-2adaa5a4f004
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 607d366633c37b65ec5cf8ae5c64d73ca1cc96b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="determine-content-management-requirements-for-your-hybrid-identity-solution"></a>A hibrid identitáskezelési megoldás tartalomkezelési követelmények meghatározása
Hello tartalomkezelési követelményeinek ismertetése számára az üzleti előfordulhat, hogy közvetlen befolyásolhatják, mely hibrid identitáskezelési megoldás toouse meg. A hello elterjedése több eszközök és felhasználók toobring hello képességét a saját eszközeiket ([BYOD](http://aka.ms/byodcg)), hello vállalati védelmét kell beállítani, hogy hozzáadják saját adataikat, de azt is kell módosulna felhasználók adatait. Általában amikor egy felhasználó rendelkezik-e a saját eszköz ő lehet is több hitelesítő adatokat, amelyek függően, hogy használó toohello alkalmazás váltakozó lesz. Fontos toodifferentiate is, hogy mely tartalmak lett létrehozva, és a vállalati hitelesítő adatok használatával létrehozott ők hello személyes hitelesítő adataival. A megoldást kell a felhőalapú szolgáltatások tooprovide képes toointeract egy zökkenőmentes élményt toohello végfelhasználói közben az adatvédelem biztosításához és az adatszivárgás elleni hello növelhető. 

A megoldást fog javítható a rendelés tooprovide tartalomkezelési különböző technikai vezérlőkkel, az alábbi hello ábrán látható módon:

![](./media/hybrid-id-design-considerations/securitycontrols.png)

**Az identitás-kezelő rendszer használni fogja biztonsági vezérlők**

Tartalomkezelési követelményeit fogja használni, általában az identitás felügyeleti rendszer hello a következő területeken:

* Adatvédelem: olyan erőforráshoz és alkalmazása hello megfelelő vezérlők toomaintain integritási birtokló hello felhasználói azonosító.
* Adatok besorolása: azonosítsa a hello felhasználót vagy csoportot és szintű hozzáférés tooan objektum tooits besorolás alapján történik. 
* Az Adatszivárgás adatvédelem: biztonsági vezérlők tooavoid adatszivárgás védelmét kell toointeract hello identitás rendszer toovalidate hello felhasználó identitásával. Ez fontos is ellenőrzési célból naplózását.

> [!NOTE]
> Olvasási [felhő készítve az adatok besorolását](http://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) további információt az ajánlott eljárásokról és útmutatást az adatok besorolása érdekében.
> 
> 

Ha a hibrid identitáskezelési megoldás tervezésének részeként győződjön meg arról, hogy a következő hello a kérdések és válaszok tooyour szervezet igényeinek megfelelően:

* Nem rendelkezik a vállalata biztonsági vezérlők hely tooenforce adatvédelem?
  * Ha igen, azokat a vezérlőelemeket lesz a hello hibrid identitáskezelési megoldás, hogy-e folyamatban tooadopt képes toointegrate?
* A vállalat használ az adatok besorolásával?
  * Ha igen, van hello aktuális megoldás képes toointegrate hello hibrid identitáskezelési megoldás, hogy-e folyamatban tooadopt?
* Rendelkezik a vállalata jelenleg bármely, a megoldás az adatok kiszivárgásának? 
  * Ha igen, van hello aktuális megoldás képes toointegrate hello hibrid identitáskezelési megoldás, hogy-e folyamatban tooadopt?
* A vállalatának meg kell tooaudit hozzáférés tooresources?
  * Ha igen, milyen típusú erőforrásokat?
  * Ha igen, milyen szintű adatokat szükség?
  * Ha igen, ahol hello napló kell lennie? A helyszíni vagy felhőben hello?
* A vállalatának meg kell tooencrypt a bizalmas adatok (taj számok esetében, hitelkártyaszámok stb) tartalmazó e-mailek?
* A vállalatának meg kell tooencrypt összes dokumentumok/tartalma a külső üzleti partnerek megosztott?
* A vállalatának meg kell tooenforce vállalati házirendek-e-mailek bizonyos típusú (nincs válasz mindenkinek végezni, ne továbbítsa)?

> [!NOTE]
> Győződjön meg arról, hogy tootake megjegyzések minden válaszról és hello válasz hello logikája ismertetése. [Data Protection stratégia meghatározása](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) hello lehetőségeit és az egyes lehetőségek előnyeit és hátrányait ismerteti.  Ezen kérdések mely leginkább megfelelő lehetőséget az üzleti megválaszolása szükséges.
> 
> 

## <a name="next-steps"></a>Következő lépések
[Hozzáférés-vezérlési követelményeinek meghatározása](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)

## <a name="see-also"></a>Lásd még:
[Kialakítási szempontok áttekintése](active-directory-hybrid-identity-design-considerations-overview.md)

