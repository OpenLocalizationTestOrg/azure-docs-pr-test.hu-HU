---
title: "aaaAzure Active Directory hibrid identitáskezelési elrendezésével kapcsolatos szempontok - áttekintése |} Microsoft Docs"
description: "Áttekintés és a hibrid Identitáskezelés – kialakítási szempontokat útmutató tartalmak térképét"
documentationcenter: 
services: active-directory
author: billmath
manager: femila
editor: 
ms.assetid: 100509c4-0b83-4207-90c8-549ba8372cf7
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 10aacb04c90abd100eb56d7c44d590946b052f18
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-hybrid-identity-design-considerations"></a>Azure Active Directory hibrid identitáskezelés – kialakítási szempontok
Ügyfél-alapú eszközök vannak proliferáló vállalati hello world, és felhő alapú szoftver-,--szolgáltatás (SaaS) alkalmazások könnyen tooadopt. Ennek eredményeképpen az adatközpontok és felhőszolgáltatások platformon, belső alkalmazás hozzáférést irányítás megőrzéséhez kihívást.  

A Microsoft identity megoldások span a helyszíni és felhőalapú képességek, létrehozása egy felhasználói azonosítót a hitelesítéshez és engedélyezéshez tooall erőforrások helyétől függetlenül. A hibrid identitás nevezzük. Nincsenek a különböző kialakítási és konfigurációs lehetőség hibrid identitás Microsoft megoldások segítségével, és néhány esetben előfordulhat, hogy melyik kombináció felel meg a szervezet hello igényeinek legjobban nehéz toodetermine. 

A hibrid Identitáskezelés – kialakítási szempontokat útmutató segítségével toounderstand hogyan toodesign hibrid identitáskezelési megoldás, amely legjobban megfelel a hello üzleti és informatikai kell a szervezet számára.  Ez az útmutató részletesen ismerteti azokat a lépéseket és feladatokat, hogy a szervezet egyedi igényeinek megfelelő hibrid identitáskezelési megoldás tervezésekor toohelp követheti. Hello lépések és feladatok során hello útmutató bemutatja a hello releváns technológiákat és szolgáltatás beállítások elérhető tooorganizations toomeet funkcionális és szolgáltatásminőségi (például a rendelkezésre állási, a méretezhetőséget, a teljesítmény, a kezelhetőségi és a biztonsági) szintre vonatkozó követelményeknek. 

Pontosabban hello hibrid identitás kialakítási szempontok útmutató céljai tooanswer hello a következő kérdéseket: 

* Kérdések mire I tooask kell, és válaszolja meg a hibrid identitás-specifikus terv a technológiai vagy problématerületnek igényeimnek leginkább megfelelő toodrive?
* Milyen tevékenységek sorozatát kell elvégzése toodesign hello technológiai vagy problématerületnek a hibrid identitáskezelési megoldás? 
* Milyen hibrid identitás technológiai és konfigurációs lehetőségek me követelmények teljesítéséhez használható toohelp? Mik a hello rendelkezésemre-előnyökkel és hátrányokkal járnak ezek a lehetőségek, hogy kiválaszthassam hello legjobb lehetőség a vállalati?

## <a name="who-is-this-guide-intended-for"></a>Kinek szól a Ez az útmutató?
 CIO, CITO, fő identitás fejlesztők, vállalati fejlesztők és informatikai fejlesztők felelősek közepes vagy nagy szervezetek hibrid identitáskezelési megoldás.

## <a name="how-can-this-guide-help-you"></a>Hogyan Ez az útmutató segítségével?
Ez az útmutató toounderstand használható hogyan toodesign hibrid identitáskezelési megoldás, amely képes toointegrate felhő alapú identitás felügyeleti rendszerében a jelenlegi helyszíni identitás-megoldás. 

a következő ábra hello példáját mutatja be, amely lehetővé teszi a rendszergazdáknak toomanage toointegrate a Windows Server Active Directory megoldással a helyszínen aktuális Microsoft Azure Active Directory tooenable felhasználók toouse egyetlen a hibrid identitáskezelési megoldás Bejelentkezés (SSO) hello felhőalapú és helyszíni alkalmazások között.

![](./media/hybrid-id-design-considerations/hybridID-example.png)

a fenti ábrán hello felhő befolyásol hibrid identitáskezelési megoldás például services és a helyszíni szolgáltatásokkal a rendelés tooprovide toointegrate, egy egységes élmény toohello végfelhasználói hitelesítési folyamat és a informatikai toofacilitate kezelése Ezeket az erőforrásokat. Ez lehet egy nagyon gyakori forgatókönyv, de minden szervezet hibrid identitás Tervező valószínűleg toobe toodifferent követelmények miatt az 1. ábrán bemutatott hello példa eltér. 

Ez az útmutató azokat a lépéseket és feladatokat, hogy kövesse a szervezet egyedi igényeinek megfelelő hibrid identitáskezelési megoldás toodesign. Teljes hello következő lépések és feladatok, hello útmutató megadja hello releváns technológiákat és szolgáltatás lehetőségek elérhető tooyou toomeet funkcionális és szolgáltatásminőségi szintre vonatkozó követelményeinek. a szervezet.

**Előfeltételek**: rendelkezik némi tapasztalattal a Windows Server, az Active Directory tartományi szolgáltatások és az Azure Active Directory. Ebben a dokumentumban feltételezzük, hogy olyan eszközökre vonatkozóan, hogy a ezek a megoldások hogyan felelnek meg az üzleti igényeinek önállóan vagy egy integrált megoldás részeként.

## <a name="design-considerations-overview"></a>Kialakítási szempontok áttekintése
Ez a dokumentum azon lépéseket és teendőket, hogy kövesse a vonatkozó követelményeknek legjobban megfelelő hibrid identitáskezelési megoldás toodesign biztosít. hello lépéseket egy rendezett sorozata jelenjenek meg. Kialakítási szempontok a későbbi lépésekben megismert igényelheti toochange során meghozott döntések a korábbi lépések azonban miatt tooconflicting tervezési döntések ütköznek azokkal. Emiatt mindent megtettünk tooalert meg hello dokumentum toopotential tervezési ütközésekre. 

Meg fog megérkezni hello tervezési, hogy igényeknek leginkább megfelelő hello lépéseken, ahányszor szükséges tooincorporate iteráció után csak a összes hello szempontok hello dokumentumban. 

| Hibrid Identity szakasz | A témakör listája |
| --- | --- |
| Identitás-követelmények meghatározása |[Az üzleti igények meghatározása](active-directory-hybrid-identity-design-considerations-business-needs.md)<br> [Címtár-szinkronizálás követelmények meghatározása](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)<br> [A multi-factor authentication követelmények meghatározása](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)<br> [A hibrid identitás bevezetési stratégia meghatározása](active-directory-hybrid-identity-design-considerations-identity-adoption-strategy.md) |
| Adatok biztonsági erős identitáskezelési megoldással továbbfejlesztésének tervezése |[Adatvédelmi követelményeinek meghatározása](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md) <br> [A Tartalomkezelés követelmények meghatározása](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)<br> [Hozzáférés-vezérlési követelményeinek meghatározása](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)<br> [Incidensválasz-követelmények meghatározása](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) <br> [Data protection stratégia meghatározása](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) |
| Hibrid identitás életciklusának tervezése |[Hibrid identitás felügyeleti feladatok meghatározása](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md) <br> [Szinkronizálás kezelése](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)<br> [Hibrid identitás kezelés bevezetési stratégiájának meghatározása](active-directory-hybrid-identity-design-considerations-lifecycle-adoption-strategy.md) |

## <a name="download-this-guide"></a>Ez az útmutató letöltése
A pdf-verziót hello hibrid identitáskezelési elrendezésével kapcsolatos szempontok az útmutató letölthető hello [Technet gallery](https://gallery.technet.microsoft.com/Azure-Hybrid-Identity-b06c8288). 

