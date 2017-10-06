---
title: "aaaAzure Active Directory hibrid identitáskezelési elrendezésével kapcsolatos szempontok - hibrid identitás életciklus bevezetési stratégia meghatározása |} Microsoft Docs"
description: "Segít a hello hibrid identitás felügyeleti feladatok toohello lehetőségeit minden egyes életciklus szakasz szerint határozza meg."
documentationcenter: 
services: active-directory
author: billmath
manager: femila
editor: 
ms.assetid: 420b6046-bd9b-4fce-83b0-72625878ae71
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 86ec0a9896f069bc93e49e06006954848f8e4d48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="determine-hybrid-identity-lifecycle-adoption-strategy"></a>Hibrid identitás életciklus bevezetési stratégia meghatározása
Ebben a feladatban fogja definiálni, identitás-kezelési stratégia hello hibrid identitáskezelési megoldás toomeet hello üzleti igényeinek, amelyet a megadott [határozza meg a hibrid identitáskezelési felügyeleti feladatok](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md).

toodefine hello hibrid identitás felügyeleti feladatok toohello végpont identitás életciklus korábbi ebben a lépésben bemutatott szerint, akkor elérhető egyes életciklus lépésekhez tooconsider hello-beállítások.

## <a name="access-management-and-provisioning"></a>Kezelési és üzembe helyezését
Jó fiók hozzáférés-kezelési megoldás a szervezet nyomon követheti, pontosan rendelkező hello szervezet toowhat adatok eléréséhez.

Hozzáférés-vezérlés egy központosított, egyetlen pont létesítési rendszer kritikus függvény. Mellett a bizalmas adatok védelmével, hozzáférés-vezérlést a meglévő fiókokat, amelyeket elérhetővé tenni jóvá nem hagyott engedélyeket, vagy már nincs szükség. elavult fiókok toocontrol, hello létesítési hivatkozások együttesen Rendszerinformáció hello felhasználók, akik saját hello fiókok mérvadó információkat. Mérvadó felhasználói azonosító adatok általában hello adatbázisok és az emberi erőforrások könyvtárak megőrződik.

Fiókok a kifinomult informatikai nagyvállalatok több száz hello hatóságok meghatározó paraméterek közé tartoznak, és ezeket a részleteket az üzembe helyezési rendszer szabályozhatók. Új felhasználók azonosítható hello mérvadó forrásból nyújtó hello adatokkal. jóváhagyási kérelem hello hozzáférés hello folyamatok, amelyek jóváhagyása (vagy elutasítása) létesítési őket az erőforrás kezdeményezi.

| Életciklus-kezelési fázis | A helyszíni | Felhő | Hibrid |
| --- | --- | --- | --- |
| Fiókok kezelése és üzembe helyezését |Hello Active Directory® tartományi szolgáltatások (AD DS) kiszolgálói szerepkör segítségével létrehozhat egy méretezhető, biztonságos és kezelhető infrastruktúrát a felhasználó- és erőforrás-kezelést, és támogatást nyújthat olyan címtárhasználatra képes alkalmazásokhoz, például a Microsoft® Exchange Server. <br><br> [Active Directory tartományi szolgáltatásokban, az Identity manager keresztül létesíthet](https://technet.microsoft.com/library/ff686261.aspx) <br>[Az Active Directory Tartományi felhasználók oszthat](https://technet.microsoft.com/library/ff686263.aspx) <br><br> A rendszergazdák biztonsági okokból használhatják access control toomanage felhasználói tooshared erőforrások eléréséhez. Az Active Directoryban, hozzáférés-vezérlés hello objektum szintű hozzáféréssel vagy jogosultságokkal tooobjects különböző szintű beállítás által felügyelt például teljes hozzáférés, írási, olvassa el, vagy nem férhető hozzá. Hozzáférés-vezérlés az Active Directory határozza meg a különböző felhasználók hogyan használhatják az Active Directory-objektumokat. Az Active Directory-objektumokra vonatkozó engedélyek beállítása alapértelmezetten az, toohello legbiztonságosabb beállítás. |Toocreate minden felhasználó érik el a Microsoft felhőszolgáltatás-fiókkal rendelkeznie. Felhasználói fiókok módosítsa vagy törölje azokat, ha azok már nincs szükség is. Alapértelmezés szerint a felhasználók rendszergazdai jogosultságokkal nem rendelkező, de is rendelhet őket. További információkért lásd: [felhasználók kezelése az Azure AD](active-directory-create-users.md). <br><br> Az Azure Active Directoryban hello fő szolgáltatásainak egyik hello képességét toomanage hozzáférés tooresources. Ezeket az erőforrásokat hello könyvtár, mint hello engedélyek toomanage objektumok keresztül hello könyvtárban, vagy külső toohello címtár, például az SaaS-alkalmazásokhoz, Azure-szolgáltatásokat, és a SharePoint-webhelyek vagy a helyszíni erőforrásokhoz szerepkörök része lehet. erőforrások. <br><br> Hello center az Azure Active Directory hozzáférés-kezelési megoldás hello biztonsági csoport. hello erőforrás tulajdonosa (vagy hello címtár rendszergazdájának hello) rendelhet hozzá egy csoporthoz tooprovide egy bizonyos jobb toohello erőforrások eléréséhez saját. hello hello csoport tagjai a rendszer hello hozzáférést, és hello erőforrás tulajdonosa hello jobb toomanage hello tagok listája egy csoport toosomeone más – például a részleg vezetője vagy a segélyszolgálat rendszergazdák számára delegálhatják<br> <br> hello kezelése csoportok az Azure AD témakör nyújt részletesebb információt hozzáférés a csoportok kezelése. |Active Directory identitások kiterjeszti a szinkronizálás és az összevonási hello felhőben |

## <a name="role-based-access-control"></a>Szerepköralapú hozzáférés-vezérlés
Szerepköralapú hozzáférés-vezérlést (RBAC) használja a szerepkörök és üzembe helyezési házirendek tooevaluate, tesztelheti, és az üzleti folyamatokat és hozzáférési toousers szabályainak. Kulcs rendszergazdák létesítési házirendeket hozhat létre, és rendelje hozzá a felhasználók tooroles, és ezek a szerepkörök jogosultságainak tooresources készleteinek definiáló. Szerepalapú hello identity management megoldást toouse szoftveres folyamatok bővíti, és csökkentheti a felhasználói kézi beavatkozás a hello létesítésének folyamatát kell használnia.
Az Azure AD RBAC lehetővé teszi, hogy a hello vállalati toorestrict hello mennyisége, amely egy adott teheti, miután hozzáférést tooAzure kezelési portál rendelkezik műveletek. Szerepalapú toocontrol hozzáférés toohello portál használatával megközelíti a rendszergazdáknak hitelesítésszolgáltató delegált hozzáférés hello hozzáférés-kezelés a következő használatával:

* **Csoportalapú szerepkör-hozzárendelés**: hozzáférés tooAzure AD csoportokat rendelhet, hogy tud-e szinkronizálva a helyi Active Directoryból. Ez lehetővé teszi tooleverage hello használó megoldásokkal, amelyek a szervezet által végrehajtott tooling és-folyamatok csoportok kezelése. Használhatja az Azure AD Premium hello delegált csoport felügyeleti szolgáltatása.
* **Használja ki a beépített szerepkörök az Azure-ban**: használhatja három szerepkörök – tulajdonos, közreműködő, és ahhoz való olvasóra, tooensure, felhasználók és csoportok rendelkezik-e engedéllyel toodo csak hello feladatok toodo a feladatokat a van szükségük.
* **A részletes hozzáférés tooresources**: toousers szerepkörök és csoportok adott előfizetés, erőforráscsoport vagy egy egyéni Azure-erőforrás például egy webhely vagy az adatbázis rendelhet. Így biztosítható, hogy a felhasználók rendelkeznek tooall hello erőforrások eléréséhez szükséges és nem hozzáférés tooresources, hogy nincs szükségük toomanage.

## <a name="provisioning-and-other-customization-options"></a>Üzembe helyezési és egyéb testreszabási lehetőségek
A csoport használhat üzleti terveket és követelmények toodecide mennyi toocustomize hello identitáskezelési megoldás. Például a nagyvállalatok szükség lehet egy szakaszos bevezetési terv a munkafolyamatok és fokozatosan történő különböző földrajzi széles körben használt alkalmazások üzembe helyezéséhez egy idősorán alapuló egyéni adaptereket. Egy másik testreszabási terv két vagy több alkalmazások toobe kiépítve a teljes szervezeten belül, a sikeres vizsgálat után előfordulhat, hogy adja meg. Felhasználó-alkalmazás beavatkozás testre szabható, és erőforrások kiépítése eljárásai módosított tooaccommodate automatikus kiépítés.

Egy szolgáltatás vagy összetevő tooremove is kiosztásának megszüntetése. Például egy fiók megszüntetés azt jelenti, hogy az erőforrás hello fiókot törölték.

hello hibrid modelljét erőforrások kiépítése egyesíti a kérelem és a szerepkör-alapú megoldások, mindkettő által támogatott az Azure AD. Az alkalmazottak vagy a felügyelt rendszerekről egy részéhez egy üzleti érdemes tooautomate hozzáférés a szerepkör-alapú hozzárendelés. Üzleti is előfordulhat, hogy kezeli, más hozzáférési kérelmek vagy kivételek kérelemalapú modell használatával. Egyes vállalatok előfordulhat, hogy kézi hozzárendelés kezdődnie, és fejleszteni hibrid modell, a teljes szerepkör-alapú központi telepítés egy későbbi időpontban szándéka felé.

Más vállalatokkal, előfordulhat, hogy megtalálja praktikus üzleti okokból tooachieve teljes szerepkör-alapú üzembe helyezés és a cél egy hibrid megközelítés kívánt célként. Továbbra is más vállalatokkal, előfordulhat, hogy megfelelőnek csak kérelem-alapú üzembe helyezés, és nem szeretné, hogy tooinvest további műveleteket toodefine és szerepkör-alapú, az automatikus létesítési házirendjeinek kezelése.

## <a name="license-management"></a>Licenckezelés
Csoportalapú Licenckezelés az Azure AD lehetővé teszi, hogy a rendszergazdák rendelje felhasználók tooa biztonsági csoportot, és az Azure AD automatikusan hozzárendeli licencek hello csoport tagjai tooall hello. Ha a felhasználók ezt követően hozzá, vagy hello csoportból eltávolított, licenc lesz automatikusan hozzárendelve vagy megfelelően eltávolítani.

Directoryból szinkronizált csoportok használhatók a helyszíni AD vagy az Azure AD kezelése. Párosítás ennek az az Azure AD premium önkiszolgáló csoportkezelési könnyen delegálhatja licenc hozzárendelése toohello megfelelő döntéshozók. Akkor biztos lehet abban, hogy a problémák, mint például a licenc ütközések és a hiányzó helyadatok automatikusan rendezett.

## <a name="self-regulating-user-administration"></a>Önálló szabályozó felhasználókezelés
Amikor a szervezet összes belső szervezet tooprovision erőforrások elindul, meg kell valósítani hello önálló szabályozó felhasználói felügyeleti funkció. Szervezet határain túlnyúló hello előnyei és üzembe helyezési felhasználók előnyeit is megvalósítható. Ebben a környezetben a felhasználó állapotának változása automatikusan megjelenik a hozzáférési jogosultsága legyen a szervezet határain és földrajzi. Üzembe helyezési költségek csökkentése és a egyszerűsítésére hello hozzáférés és a jóváhagyási folyamatot. hello megvalósítási hello teljes lehetőségeket kínál a végpont hozzáférés kezelése szerepköralapú hozzáférés-vezérlés implementálása a szervezet valósít meg. Keresztül szabályozására, a felhasználók átadása automatizált eljárások a felügyeleti költségek csökkentése érdekében. A biztonság fokozása érdekében automatizálása a biztonsági házirendek betartását, és egyszerűsítésére és felhasználói életciklusának kezelését és az erőforrás-kiépítés nagy felhasználói csoportok számára.

> [!NOTE]
> További információkért lásd: Azure ad-val önkiszolgáló alkalmazáshozzáférés-kezeléshez beállítása
> 
> 

Az Azure AD (jogosultság alapján) licenc-alapú munkahelyi szolgáltatásainak a címtárszolgáltatás vagy Azure AD-bérlő előfizetés aktiválása. Aktív hello előfizetés, miután hello szolgáltatás jellemzőinek címtárszolgáltatás vagy a rendszergazdák által felügyelt, és a licenccel rendelkező felhasználók által használt. További információkért lásd: hogyan működik a licencelés munka az Azure AD?
Integráció más 3. fél szolgáltatók

Az Azure Active Directory egyszeri bejelentkezést biztosít, és a bővített alkalmazás hozzáférés biztonsági toothousands SaaS-alkalmazásokhoz és a helyszíni webalkalmazások. Támogatott SaaS-alkalmazásokhoz az Azure Active Directory alkalmazáskatalógusában részletes listájáért lásd: az Azure Active Directory összevonási kompatibilitási lista: harmadik fél Identitásszolgáltatók, amelyek lehetnek használt tooimplement egyszeri bejelentkezés

## <a name="define-synchronization-management"></a>Adja meg a szinkronizálási felügyeleti
A helyszíni címtárak és az Azure AD integrálása révén a felhasználók munkája hatékonyabbá válik, mivel a felhőalapú és a helyszíni erőforrások hozzáféréséhez közös identitás áll a rendelkezésükre. Ez az integráció felhasználók és a szervezetek kihasználhatják a hello következő:

* A szervezetek biztosíthatnak a felhasználóknak hibrid közös identitással a helyszíni vagy felhőalapú szolgáltatásokat kihasználva a Windows Server Active Directory, majd csatlakozzon az Active Directory tooAzure között.
* A rendszergazdák alkalmazás erőforrás, a eszköz és a felhasználói identitást, a hálózati hely és a többtényezős hitelesítés alapján feltételes hozzáférést nyújthatnak.
* A közös identitás keresztül fiókok Azure AD tooOffice 365, Intune, SaaS-alkalmazások és a harmadik féltől származó alkalmazások a felhasználók használhatják fel.
* A fejlesztők is használó alkalmazások létrehozását hello közös identitás modell alkalmazások integrálása a helyszíni Active Directory vagy az Azure felhőalapú alkalmazásokhoz

hello. a következő ábra is rendelkezik identitás szinkronizálási folyamat áttekintése látható.

![](./media/hybrid-id-design-considerations/identitysync.png)

Identitás szinkronizálási folyamat

Tekintse át a következő tábla toocompare hello szinkronizálási beállítások hello:

| Szinkronizálás lehetőséget | Előnyei | Hátrányok |
| --- | --- | --- |
| (A DirSync vagy az AADConnect) szinkronizálásra épülő |Felhasználók és csoportok szinkronizálja a helyszíni és felhő <br>  **A házirend-szabályozás**: fiók házirendeken keresztül állíthatók be, és több, anélkül, hogy további tooperform hello rendszergazda hello képességét toomanage jelszóházirendeket, munkaállomás, korlátozások, zárolás kibővített vezérlőket biztosít, amely Active Directoryn keresztül feladatok hello felhőben.  <br>  **Hozzáférés-vezérlés**: toohello felhőalapú szolgáltatás korlátozhatja, hogy a hello szolgáltatások is elérhetőek hello vállalati környezetben, az online kiszolgálók vagy mindkettőt. <br>  Támogatási hívások csökkenteni: Ha a felhasználók kevesebb jelszavak tooremember rendelkeznek, azok kevésbé valószínű tooforget őket. <br>  Biztonsági: Felhasználói identitások és információk védve vannak, mivel az összes hello kiszolgálók és az egyszeri bejelentkezés, a használt szolgáltatások értékűre, és a helyszíni szabályozott. <br>  Erős hitelesítés támogatása: hello felhőalapú szolgáltatás erős hitelesítést (más néven kéttényezős hitelesítést) használható. Azonban ha erős hitelesítés használatához kell használnia egyszeri bejelentkezést. | |
| Összevonáson alapuló (Active Directory összevonási Szolgáltatásokban) |Biztonságijogkivonat-szolgáltatás (STS) által engedélyezett. Ha egy STS tooprovide egyszeri bejelentkezéses hozzáférést konfigurál egy Microsoft felhőszolgáltatásra, létrehozni egy összevont megbízhatósági kapcsolat a helyszíni STS és hello összevont tartományt az Azure AD-bérlő megadott között. <br> Lehetővé teszi, hogy a végfelhasználók toouse hello azonos hitelesítő adatok tooobtain toomultiple erőforrások csoportja <br>a végfelhasználók nem rendelkeznek toomaintain több hitelesítőadat-készletek. Még, hello felhasználóknál tooprovide a hitelesítő adatok tooeach egy hello részt vevő erőforrások., B2B és B2C-forgatókönyvek esetében támogatott. |Speciális személyzet igényel dedikált helyszíni telepítésének és az AD FS-kiszolgáló. Ha azt tervezi, AD FS toouse az STS, nincsenek hello használata erős hitelesítés vonatkozó korlátozás. További információkért lásd: [speciális beállítások konfigurálása az AD FS 2.0](http://go.microsoft.com/fwlink/?linkid=235649). |

> [!NOTE]
> További információ: [a helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).
> 
> 

## <a name="see-also"></a>Lásd még:
[Kialakítási szempontok áttekintése](active-directory-hybrid-identity-design-considerations-overview.md)

