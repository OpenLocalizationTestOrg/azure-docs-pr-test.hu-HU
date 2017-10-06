---
title: "aaaAzure Active Directory hibrid identitáskezelési elrendezésével kapcsolatos szempontok - incidens rResponse követelmények meghatározása |} Microsoft Docs"
description: "Hello hibrid identitáskezelési megoldás, amely szerint informatikai tootake műveletek tooidentify kihasználható és csökkentik a potenciális fenyegetéseket a figyelési és jelentéskészítési képességekkel meghatározása"
documentationcenter: 
services: active-directory
author: billmath
manager: femila
editor: 
ms.assetid: a3d2a459-599b-4b67-8e51-7369ee25082d
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 7084096f318ef461e8331fb6edde1b77d4108466
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="determine-incident-response-requirements-for-your-hybrid-identity-solution"></a>A hibrid identitáskezelési megoldás incidensválasz-követelmények meghatározása
Közepes vagy nagy szervezetek valószínűleg fog rendelkezni a [biztonsági incidensekre adott reakciók](https://technet.microsoft.com/library/cc700825.aspx) a hely toohelp informatikai műveletek ennek megfelelően incidens toohello szintjét. hello identity management rendszer hello incidensválasz folyamat fontos összetevője oka végző felhasználók listáját egy bizonyos művelet hello célon azonosítására használt toohelp lehet. hello hibrid identitáskezelési megoldás képes tooprovide figyelési és jelentéskészítési funkciókat nyújtanak, amelyek informatikai tootake műveletek tooidentify által is javítható, és a potenciális fenyegetések mérséklésére kell lennie. Egy tipikus incidensválasz tervben meg kell hello fázisok hello terv részeként a következő:

1. Kezdeti értékelése.
2. Incidens kommunikációt.
3. Kárelhárítási és a kockázat csökkentése érdekében.
4. Mi azonosítása biztonsági sérülés és a súlyosság volt.
5. Bizonyító adatok megőrzését.
6. Értesítési tooappropriate felek.
7. Rendszer-helyreállítás.
8. Dokumentációját.
9. Sérülés és a költséghatékonyság értékelése.
10. Folyamat és a csomag verziójának.

Alatt hello azonosítását, amit a rendszergazda volt a biztonsági sérülés és a súlyosság-fázis, a szükséges tooidentify hello rendszerek biztonsága sérült, hozzáfért lett, és határozza meg azokat a fájlokat hello érzékenységének fájlok lesz. A hibrid identitáskezelési rendszer kell tudni toofulfill ezen követelmények tooassist azonosításának hello módosítást végrehajtó felhasználó, ezeket a módosításokat. 

## <a name="monitoring-and-reporting"></a>Figyelés és jelentéskészítés
Sokszor hello identitásrendszere is segíthet kezdeti felmérési fázisban, főleg, ha a naplózási és jelentéskészítési képességek hello rendszer létrehozta. Hello kezdeti értékelés során rendszergazdának kell tudni tooidentify gyanús tevékenységet, vagy hello rendszernek kell lennie a feladat egy előre konfigurált automatikusan alapján képes tootrigger. Számos tevékenység a lehetséges támadások jelezhet, azonban más esetekben a rosszul konfigurált rendszer vakriasztások tooa száma vezethet egy behatolás-észlelés rendszerben. 

hello identity management rendszer segítséget nyújt az informatikai rendszergazdák tooidentify kell, és a jelentés azokat a gyanús tevékenységek. Ezek a technikai követelmények általában minden rendszerek figyelése és a jelentéskészítési képességet is jelölje ki a lehetséges veszélyforrásokkal szemben, amelyek teljesíthetők. Hello kérdések alatt toohelp alakítson ki az incidensválasz-követelmények figyelembe vételével a hibrid identitáskezelési megoldás:

* Nem a vállalat rendelkezik egy biztonsági incidensekre adott reakciók?
  * Ha igen, hello identitás jelenlegi felügyeleti rendszerben szerepel hello folyamat részeként?
* A vállalatának meg kell tooidentify gyanús bejelentkezési kísérletet a felhasználók különböző eszközökön?
* A vállalatának meg kell toodetect lehetséges feltört felhasználói hitelesítő adatokat?
* A vállalatának meg kell tooaudit felhasználói hozzáférés és a művelet?
* A vállalatának meg kell tooknow amikor egy felhasználó a jelszó alaphelyzetbe állítása?

## <a name="policy-enforcement"></a>Házirendek érvényesítése
Kárelhárítási és a kockázatot csökkentő-fázis során fontos tooquickly hello tényleges és lehetséges hatásai a támadás csökkentése. Adott elvégzendő lesz ezen a ponton tehet egy kisebb és nagyobb egy hello különbsége. a szervezet és a hello támadások ellen, amik akkor szembesülhetnek hello jellege hello pontos válasz függ. Ha hello kezdeti assessment arra a következtetésre jutott, hogy a fiók biztonsági szempontból sérült, akkor ez a fiók tooenforce házirend tooblock. Ez mindössze egy példa, ahol hello identity management rendszer elkészítéséhez használja. A hibrid identitáskezelési megoldás tervezése során figyelembe véve, hogyan lesz házirendek kényszerítése tooreact tooan folyamatban lévő incidens toohelp alatt hello kérdések használata:

* Nem rendelkezik a vállalata házirendekkel a hely tooblock felhasználók hozzáférési hello hálózatról szükség?
  * Ha igen, nem hello aktuális megoldás címtárszolgáltatással hello hibrid identitás felügyeleti rendszer, hogy-e folyamatban tooadopt?
* A vállalatának meg kell tooenforce feltételes hozzáférés a felhasználók számára, amely karanténba? 

> [!NOTE]
> Győződjön meg arról, hogy tootake megjegyzések minden válaszról és hello válasz hello logikája ismertetése. [Data protection stratégia meghatározása](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) hello lehetőségeit és az egyes lehetőségek előnyeit és hátrányait ismerteti.  Ezen kérdések mely leginkább megfelelő lehetőséget az üzleti megválaszolása szükséges.
> 
> 

## <a name="next-steps"></a>Következő lépések
[Data protection stratégia meghatározása](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md)

## <a name="see-also"></a>Lásd még:
[Kialakítási szempontok áttekintése](active-directory-hybrid-identity-design-considerations-overview.md)

