---
title: "Azure Active Directory-architektúra aaaUnderstand |} Microsoft Docs"
description: "Ismerteti, mi az Azure AD-bérlő van, és hogyan toomanage Azure Azure Active Directoryn keresztül"
services: active-directory
documentationcenter: 
author: MarkusVi
writer: v-lorisc
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/02/2017
ms.author: markvi
ms.openlocfilehash: 799943c012dcc309907ed3c36372038a0aad222a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="understand-azure-active-directory-architecture"></a>Az Azure Active Directory architektúrájának ismertetése
Az Azure Active Directory (Azure AD) lehetővé teszi, hogy Ön toosecurely kezelése elérési tooAzure szolgáltatások és erőforrások a felhasználók számára. Az Azure AD-ben megtalálható az identitáskezelési megoldások teljes palettája. Az Azure AD-funkciókkal kapcsolatos információért lásd: [Mi az az Azure Active Directory?](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-whatis)

Az Azure ad-vel hozzon létre és kezelheti a felhasználókat és csoportokat, és engedélyezheti engedélyek tooallow és tooenterprise erőforrások megtagadja a hozzáférést. Identitás-kezeléssel kapcsolatos további információkért lásd: [hello Azure Identitáskezelés alapjai](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals-identity).

## <a name="azure-ad-architecture"></a>Azure AD-architektúra
Az Azure AD földrajzilag elosztott architektúra egyesíti a széles körű figyelést, automatikus átirányítását, a feladatátvételi és helyreállítási funkciók lehetővé teszik számunkra toodeliver vállalati szintű rendelkezésre állásának és teljesítményének tooour ügyfelek.

hello architektúra elemek a következő cikkben ismertetett:
 *  Szolgáltatásarchitektúra kialakítása
 *  Méretezhetőség 
 *  Folyamatos rendelkezésre állás
 *  Adatközpontok

### <a name="service-architecture-design"></a>Szolgáltatásarchitektúra kialakítása
hello leggyakoribb módja toobuild egy méretezhető, magas rendelkezésre állású, adatok gazdag rendszer független építőelemeket vagy hello Azure AD adatrétegbeli a méretezési egységek használatával, a méretezési egységek nevezzük *partíciók*. 

hello adatrétegbeli rendelkezik írási és olvasási képességet több előtér-szolgáltatás. az alábbi ábrán hello bemutatja, hogyan egyetlen címtárpartíció hello összetevők teljes földrajzilag distrubuted adatközpontok küld. 

  ![Egy címtárból álló partíciók](./media/active-directory-architecture/active-directory-architecture.png)

az Azure AD-architektúra hello összetevők közé tartoznak, egy elsődleges replika és a másodlagos replikákon.

**Elsődleges replika**

Hello *elsődleges replika* megkapja az összes *ír* hello partíció tartozik. Bármely az írási művelet azonnal replikált tooa másodlagos másodpéldány egy ugyanabban az adatközpontban sikeres toohello hívó, ami biztosítja az írások georedundáns tartóssági visszatérése előtt.

**Másodlagos replikák**

Minden *címtárolvasás* *másodlagos replikákból* van szolgáltatva, amelyek különböző földrajzi helyeken lévő adatközpontokban találhatók. Sok másodlagos replika van, mivel az adatok replikálása aszinkron módon történik. Directory olvasási műveletek, például a hitelesítési kérelmeket, a rendszer szervizelt adatközpontokban, amelyek Bezárás tooour ügyfelek. hello másodlagos replikák olvasási méretezhetőség felelősek.

### <a name="scalability"></a>Méretezhetőség

A méretezhetőség azt hello azon képessége, egy szolgáltatás tooexpand toomeet teljesítményigénye növelését. Az írási méretezhetőség megvalósítani hello adatok particionálása. Olvasási méretezhetőségi adatok replikálásához több partíció toomultiple másodlagos replikák hello world belül terjesztett használatával érhető el.

A könyvtár alkalmazások kérések használata általában irányított toohello datacenter, amely fizikailag közelebb. Írás transzparens módon átirányított toohello elsődleges replika tooprovide írható-olvasható konzisztencia. Mivel hello könyvtárak általában szolgál olvasások hello legtöbbször ennek a másodlagos replikák jelentősen hello méretezési partíciók terjed ki.

Directory alkalmazások legközelebbi adatközpont toohello csatlakozzon. Ez javítja a teljesítményt, és így lehetséges a felskálázás. Egy lehetnek sok másodlagos replika, mivel a másodlagos replikák szorosabb toohello directory ügyfelek helyezhetők. Csak belső könyvtár szolgáltatás-összetevők, amelyek közvetlenül írási igényű cél hello aktív elsődleges replika.

### <a name="continuous-availability"></a>Folyamatos rendelkezésre állás

Rendelkezésre állás (vagy hasznos üzemidő) határozza meg a hello azon képessége, a rendszer tooperform megszakítás nélkül. hello kulcs tooAzure AD a magas rendelkezésre állású, hogy a szolgáltatások is gyorsan az eltolás mértékét megadó forgalom között több földrajzilag elosztott adatközpontokban. Mindegyik adatközpont független, ami lehetővé teszi a nem összefüggő hibaállapotokat.

Az Azure AD partíció kialakítása egyszerűsített összehasonlított toohello vállalati AD-kialakításához, ami létfontosságú a vertikális felskálázásával hello rendszer esetén. Egyetlen főkialakítást alkalmaztunk, amely egy körültekintően összehangolt és determinisztikus feladatátvételi folyamatot is magában foglal az elsődleges replika számára.

**Hibatűrés**

A rendszer több elérhető, hibatűrő toohardware, szoftverek és hálózati hibák esetén. Mindegyik partíció hello könyvtár, egy magas rendelkezésre állású fő replika létezik: hello elsődleges másodpéldány. Csak az írási műveletek toohello partíció megy végbe a replika. A replika szorosan és folyamatosan figyeli, és az írás lehet azonnal eltolt tooanother replika (amely hello válik új elsődleges) Ha hiba lép fel. Feladatátvétel esetén általában 1-2 percig megszűnhet az írás rendelkezésre állása. Ez nincs hatással az olvasás rendelkezésre állására.

Olvasási műveletek (amely írások outnumber sok nagyságrenddel szerint) csak Ugrás toosecondary replikákat. Mivel a másodlagos replikákat az idempotent, bármely egy másodpéldány egy adott partíció megszűnését könnyen kompenzálva által általában arra utasíthatja a hello olvasások tooanother replika hello ugyanabban az adatközpontban.

**Adatok tartóssága**

Az írási véglegesítésére tooat legalább két adatok központok előzetes tooit nyugtázása folyamatban. Ez akkor fordul elő először véglegesítése az elsődleges hello hello az írási és majd azonnal replikálása hello írási tooat legalább egy másik adatközpont. Ez biztosítja, hogy egy lehetséges hello data center üzemeltetési hello elsődleges végzetes adatvesztés adatvesztést eredményez.

Az Azure AD fenntartja a nulla [helyreállítási idő célkitűzés (RTO)](https://en.wikipedia.org/wiki/Recovery_time_objective) jogkivonat kibocsátási és directory beolvassa és perc (~ 5 perc) RTO könyvtár hello sorrendben. A [helyreállítási időkorlát (RPO)](https://en.wikipedia.org/wiki/Recovery_point_objective) szintén nulla, így nem veszítünk adatokat a feladatátvételek során.

### <a name="data-centers"></a>Adatközpontok

Az Azure AD-replikák adatközpontok hello world belül vannak tárolva. További információk: [Azure-adatközpontok](https://azure.microsoft.com/en-us/overview/datacenters).

Az Azure AD, a következő jellemzőkkel hello működik adatközpontok között:

 * Hitelesítés, a Graph és az egyéb AD szolgáltatások hello átjárószolgáltatás mögött található. Átjáró hello kezeli terheléselosztása, ezeket a szolgáltatásokat. Automatikus feladatátvételt hajt végre, ha a rendszer a tranzakciós állapottesztek során nem megfelelő állapotú kiszolgálókat észlel. Ezek állapotfigyelő mintavételt alapján, hello átjáró dinamikusan irányítja a forgalmat toohealthy adatközpontokban.
 * A *beolvassa*, hello directory van több különböző adatközponthoz működő aktív-aktív konfigurációban másodlagos replikák és a megfelelő előtér-szolgáltatásokat. Ha egy teljes adatközpont leáll a forgalom lesz automatikusan irányított tooa ugyanabban az adatközpontban.
 *  A *ír*, hello directory lesz feladatátvevő (fő) replikáról különböző adatközpontokban keresztül tervezett (új elsődleges kiszolgáló elsődleges szinkronizált tooold) vagy vészhelyzeti feladatátvételi eljárásokat. Adatok tartóssága megvalósítani replikálásához bármely véglegesítési tooat legalább két adatközpontokban.

**Adatkonzisztencia**

hello directory modell a végleges konzisztencia egyike. Elosztott, aszinkron módon replikációs rendszerek egy tipikus probléma, hogy a "adott" a replika által visszaadott hello adatokat nem lehet toodate fel. 

Az Azure AD célcsoport-kezelési egy másodlagos másodpéldány által az írási műveletek toohello elsődleges replika útválasztási alkalmazások írható-olvasható konzisztenciát biztosít, és vissza toohello másodlagos replika szinkron módon húzza hello írja.

Alkalmazás írja a Graph API-t az Azure AD azért affinitás tooa directory replika írható-olvasható konzisztencia tartsanak hello segítségével. Azure AD Graph szolgáltatás hello fenntartja a logikai munkamenetet, amelynek affinitás tooa másodlagos replika használt olvasása; kapcsolat bekerül az "a replika tokenre" hello graph szolgáltatás gyorsítótárát egy elosztott gyorsítótár használatával. Ez a token hello a későbbi műveletek használja ugyanazt a logikai munkamenet. 

 >[!NOTE]
 >Írási műveleteket azonnal replikálódnak toohello másodlagos replika toowhich hello logikai munkamenet olvasások állítottak ki.
 >

**Biztonsági másolatok védelme**

hello directory véglegesen törli, rögzített törli, a felhasználók és a bérlők könnyen esetén történő helyreállításra vonatkozóan az ügyfél által véletlen törlések helyett valósítja meg. A bérlői rendszergazda véletlenül törli a felhasználókat, ha azok könnyen visszavonása és hello törölt felhasználók visszaállítása. 

Az Azure AD naponta biztonsági másolatot készít az összes adatról, ezért képes az adatok mérvadó visszaállítására bármilyen logikai törlés vagy sérülés esetében. Az adatrétegünk hibajavító kódokat alkalmaz, így ellenőrizni tudja a hibákat, és képes automatikusan kijavítani bizonyos típusú lemezhibákat.

**Mérőszámok és figyelők**

Egy magas rendelkezésre állású szolgáltatás futtatásához világszínvonalú metrikára és monitorozási képességekre van szükség. Az Azure AD folyamatosan elemzi a szolgáltatások állapotával kapcsolatos legfontosabb mérőszámokat és az egyes szolgáltatások sikerességi feltételeit, illetve jelentést készít azokról. Folyamatosan fejlesztjük és hangoljuk az egyes forgatókönyvekhez tartozó mérőszámokat, figyelést és riasztást az egyes Azure AD szolgáltatásokban, illetve az összes szolgáltatásban.

Minden Azure AD szolgáltatás nem a várt módon működik, ha azt azonnal érvénybe művelet toorestore funkció minél gyorsabban. hello legfontosabb mérőszám az Azure AD nyomon követi az milyen gyorsan azt is észleli és csökkenthető az ügyfél vagy élő hely probléma. Azt fektetnek fokozottan a figyelés és riasztás toominimize idő toodetect (TTD cél: < 5 perc) és a működőképesség toominimize idő toomitigate (TTM cél: < 30 perc).

**Biztonságos műveletek**

Műveleti vezérlőket, például többtényezős hitelesítést (MFA) alkalmazunk minden művelet esetében, valamint naplózzuk az összes műveletet. Emellett azt egy just-in-time jogosultságszint-emelési rendszer toogrant szükséges ideiglenes hozzáférést használhat bármely működési feladat igény helyezni. További információkért lásd: [megbízható felhő hello](https://azure.microsoft.com/en-us/support/trust-center).

## <a name="next-steps"></a>Következő lépések
[Az Azure Active Directory fejlesztői útmutatója](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-developers-guide)

