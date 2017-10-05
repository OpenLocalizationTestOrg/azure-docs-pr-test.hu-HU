---
title: 'Reporting: Az Azure AD SSPR |} Microsoft Docs'
description: "Események jelentés készítését az Azure AD az önkiszolgáló jelszó alaphelyzetbe állítása"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: gahug
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: ba7b36e654aa0bf3b74d42a2b0ae96ae2a9b6241
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="reporting-options-for-azure-ad-password-management"></a>Jelentéskészítési lehetőségek az Azure AD-jelszókezelés

Telepítés a számos szervezet szeretnék tudni, hogyan, vagy ha az önkiszolgáló jelszó-Változtatási valóban használatban van. Az Azure AD biztosít, amelyek segítenek használatával, előre összeállított jelentéseket, és ha megfelelően rendelkezik licenccel, kérdések megválaszolása jelentéskészítési szolgáltatások lehetővé teszik egyéni lekérdezések létrehozása.

Az alábbi kérdések is válaszolhatók meg az [Azure-portálon] jelentéseket (https://portal.azure.com/).

> [!NOTE]
> Kell [egy globális rendszergazda](active-directory-assign-admin-roles.md#assign-or-remove-administrator-roles) és kell hagyniuk az ezeket az adatokat a jelentéskészítési lapon vagy a napló naplók legalább egyszer felkeresésével összegyűjteni a szervezet nevében. Így, amíg adatok nem lesznek összegyűjtve a szervezet számára

* Jelszó alaphelyzetbe állítása hányan regisztrált?
* Aki regisztrálva van a jelszó alaphelyzetbe állítása?
* Milyen adatokat próbál regisztrálni személyek?
* Hányan visszaállíthassák a jelszavukat, az elmúlt hét napban?
* Mik a leggyakoribb módszerek felhasználók vagy rendszergazdák visszaállíthassák a jelszavukat használja?
* Mik azok a gyakori problémák felhasználók vagy rendszergazdák arcfelismerési a jelszó alaphelyzetbe állítása?
* Milyen a rendszergazdák gyakran alaphelyzetbe állítja saját jelszavukat?
* A gyanús tevékenységeket, a jelszó alaphelyzetbe állítása folyamatban van?

## <a name="how-to-view-password-management-reports-in-the-azure-portal"></a>Az Azure portálon jelszó jelentések megtekintése

Az Azure portál felhasználói élmény megtekintéséhez a jelszó alaphelyzetbe állítása egy továbbfejlesztett módja van, és a jelszó-visszaállítási regisztrációs tevékenység.  Kövesse az alábbi lépéseket a jelszó alaphelyzetbe állítása és a jelszó alaphelyzetbe állítása regisztrációs események kereséséhez:

1. Navigáljon a [ **portal.azure.com**](https://portal.azure.com)
2. Kattintson a **további szolgáltatások** fő az Azure portál bal oldali navigációs menü
3. Keresse meg **Azure Active Directory** a szolgáltatások listájában, és jelölje ki
4. Kattintson a **felhasználók és csoportok** az Azure Active Directory navigációs menü
5. Kattintson a **naplók** navigációs elem a felhasználók és csoportok navigációs menüjében. Megjelenik az összes szemben a címtárban szereplő összes felhasználó bekövetkező események naplózása. Ez a nézet összes a jelszóval kapcsolatos eseményeket, is végezhet.
6. Ez a nézet csak a jelszó-átállítási kapcsolódó események szűréséhez kattintson a **szűrő** gomb a panel tetején.
7. Az a **szűrő** menüben válassza a **kategória** legördülő menüből, és módosítsa úgy, hogy a **önkiszolgáló jelszókezelés** kategória típusa.
8. Opcionálisan tovább szűkítheti a listában válassza ki az adott **tevékenység** érdekli

## <a name="how-to-retrieve-password-management-events-from-the-azure-ad-reports-and-events-api"></a>Hogyan lehet lekérni a jelszó felügyeleti események az Azure AD-jelentések és események API

Az Azure AD-jelentések és események API támogatja az összes, a jelszó alaphelyzetbe állítása és a jelszó alaphelyzetbe állítása szóregisztrációs jelentéseket szereplő adatok beolvasása. Ez az API használatával letöltheti az egyes jelszó alaphelyzetbe állítása, és a jelszó-változtatási regisztrációs események az integráció a jelentéskészítő technológiával az Ön által választott.

### <a name="how-to-get-started-with-the-reporting-api"></a>Első lépések a reporting API-hoz

Ezek az adatok eléréséhez meg kell írnia egy kis alkalmazás vagy a parancsfájl azt lekérése a kiszolgálóról. [Útmutató az Azure AD Reporting API használatába](active-directory-reporting-api-getting-started.md).

Ha már van egy működő parancsfájl, ezután érdemes vizsgálja meg az esetek teljesítéséhez kérheti le a jelszó alaphelyzetbe állítása és nyilvántartási események.

* [SsprActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent): az elérhető oszlopok listája, a jelszó-átállítási események
* [SsprRegistrationActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprRegistrationActivityEvent): az elérhető oszlopok listája, a jelszó-átállítási regisztráció események

### <a name="reporting-api-data-retrieval-limitations"></a>Jelentéskészítési API adatok lekérését korlátozásai

Jelenleg az Azure AD-jelentések és események API lekérdezi legfeljebb **75,000 események** , a [SsprActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent) és [SsprRegistrationActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprRegistrationActivityEvent) típusok átfedés a **utolsó 30 nap**.

Ha szeretné beolvasni, vagy ez az ablak adatok tárolására, javasoljuk, hogy külső adatbázis megőrzése, és a eltéréseit – eredményező lekérdezni az API-val. Azt javasoljuk, hogy ezek az adatok beolvasása SSPR segítségével a szervezet indításakor megkezdéséhez, megőrizni a külsőleg, és majd továbbra is nyomon követheti az eltérések ettől kezdve.

## <a name="how-to-download-password-reset-registration-events-quickly-with-powershell"></a>Jelszó alaphelyzetbe állítása regisztrációs események gyorsan PowerShell letöltése

Az Azure AD-jelentések és események API közvetlenül használata mellett is használhatja az alábbi PowerShell-parancsfájl használatával legutóbbi regisztrációs események a címtárban. Ez akkor hasznos, ha meg szeretné tekinteni, akik nemrég regisztrálva van, vagy győződjön meg arról, hogy a várt módon jelentkezett-e a jelszó alaphelyzetbe állítása bevezetést szeretné.

* [Az Azure AD SSPR regisztrációs tevékenység PowerShell-parancsfájl](https://gallery.technet.microsoft.com/scriptcenter/azure-ad-self-service-e31b8aee)

### <a name="description-of-report-columns-in-azure-portal"></a>Azure-portálon jelentés oszlopok leírása

Az alábbi lista ismerteti részletesen jelentés oszlopainak mindegyike:

* **Felhasználói** – kísérletet a jelszó alaphelyzetbe állítása regisztrációs műveletet.
* **Szerepkör** – a szerepkört a felhasználó a címtárban.
* **Dátum és idő** – a dátum és idő a kísérlet.
* **Adatok regisztrált** – milyen hitelesítési adatokat, a felhasználó által megadott során jelszó-átállítási regisztrációk.

### <a name="description-of-report-values-in-azure-portal"></a>Azure-portálon jelentés értékek leírása

A következő táblázat ismerteti a különböző minden egyes oszlophoz engedélyezett értékek:

| Oszlop | Megengedett értékek és azok jelentését |
| --- | --- |
| Regisztrált adatok |**Másodlagos e-mail-címet** – a felhasználó másodlagos e-mail vagy hitelesítési e-mail használt hitelesítéséhez<p><p>**Irodai telefon**– hitelesítéséhez használt felhasználói irodai telefon<p>**Mobiltelefon** -használt felhasználói mobiltelefonszám vagy a hitelesítéshez megadott telefonját hitelesítéséhez<p>**Biztonsági kérdések** – felhasználó hitelesítéséhez a biztonsági kérdések használt<p>**A fenti (például másodlagos E-mail + mobiltelefon) kombinációja** – akkor fordul elő, amikor 2 kapu házirend van megadva, és melyik két módszert használja a felhasználó mutatja a hitelesítéshez a jelszó-változtatási kérelmet. |

## <a name="view-password-reset-activity-in-the-classic-portal"></a>Nézet jelszó-visszaállítási tevékenység a klasszikus portál

Ez a jelentés tartalmazza az összes jelszó alaphelyzetbe állítása, amely a szervezet történt kísérlet.

* **Maximális időtartomány**: 30 napban
* **A sorok maximális számát**: 75 000
* **Letölthető**: Igen, a CSV-fájl használatával

### <a name="description-of-report-columns-in-azure-classic-portal"></a>A klasszikus Azure portálon jelentés oszlopok leírása

Az alábbi lista ismerteti részletesen jelentés oszlopainak mindegyike:

1. **Felhasználói** – kísérletet a jelszó alaphelyzetbe állítása (a felhasználói azonosító mező megadni, ha a felhasználó rendelkezik a jelszó alaphelyzetbe állítása alapján) műveletet.
2. **Szerepkör** – a szerepkört a felhasználó a címtárban.
3. **Dátum és idő** – a dátum és idő a kísérlet.
4. **Módszerek használható** – ez használt a felhasználói hitelesítési módszerek alaphelyzetbe állítja a műveletet.
5. **Eredmény** – az eredmény a jelszó alaphelyzetbe állítása a műveletet.
6. **Részletek** – miért a jelszó-átállítási részleteit az értéket, ugyanúgy eredményezett.  Is tartalmaz a megoldás lépéseket hiba megoldásához is igénybe vehet.

### <a name="description-of-report-values-in-azure-classic-portal"></a>A jelentés értékeket a klasszikus Azure portálon leírása

A következő táblázat ismerteti a különböző minden egyes oszlophoz engedélyezett értékek:

| Oszlop | Megengedett értékek és azok jelentését |
| --- | --- |
| Használt módszerek |**Másodlagos e-mail-címet** – a felhasználó másodlagos e-mail vagy hitelesítési e-mail használt hitelesítéséhez<p>**Irodai telefon** – hitelesítéséhez használt felhasználói irodai telefon<p>**Mobiltelefon** – használt felhasználói mobiltelefonszám vagy a hitelesítéshez megadott telefonját hitelesítéséhez<p>**Biztonsági kérdések** – felhasználó hitelesítéséhez a biztonsági kérdések használt<p>**A fenti (például másodlagos E-mail + mobiltelefon) kombinációja** – akkor fordul elő, amikor 2 kapu házirend van megadva, és melyik két módszert használja a felhasználó mutatja a hitelesítéshez a jelszó-változtatási kérelmet. |
| eredménye |**Elhagyott** – felhasználói jelszó-visszaállítás elindult, de le félúton helyezkedjenek el keresztül nélkül befejezése<p>**Blokkolt** – felhasználói fiók segítségével, hogy a jelszó-visszaállítás miatt kísérlet a jelszó alaphelyzetbe állítása a lapon miatt sikertelen volt, vagy egyetlen jelszó kapu visszaállítása egy 24 órás időtartamon belül túl sokszor<p>**Visszavont** – felhasználói jelszó-visszaállítás elindult, de majd kattintott a Mégse gombra a részben módon a munkamenet megszakítása <p>**Rendszergazda-e kommunikálni** – felhasználói probléma volt a munkamenetben, hogy nem oldható fel, így a felhasználó helyett befejezési "Forduljon a rendszergazdához" hivatkozásra kattint a jelszó-átállítási folyamata<p>**Nem sikerült** – felhasználó nem tudta állítani a jelszót, valószínűleg, mert a felhasználó nem lett konfigurálva ahhoz, hogy a szolgáltatás (például nem licenc, hiányzó hitelesítési adatokat, jelszó által kezelt helyszíni, de visszaírás le van tiltva).<p>**Sikeres** – jelszó-visszaállítás sikeres volt. |
| Részletek |Az alábbi táblázatban találja |

### <a name="allowed-values-for-details-column"></a>Engedélyezett részletek oszlop értékei

Alább pedig akkor is várhatnak, amikor jelszó segítségével alaphelyzetbe állítja a tevékenységgel kapcsolatos jelentés eredménytípusai:

| Részletek | Eredménytípus |
| --- | --- |
| Az e-mail-hitelesítés kihagyásának lehetőségét befejezése után elhagyott felhasználó |Elhagyott |
| A mobileszköz SMS-hitelesítés kihagyásának lehetőségét befejezése után elhagyott felhasználó |Elhagyott |
| A mobileszköz beszédfelismerési hívás ellenőrzési lehetőséggel befejezése után elhagyott felhasználó |Elhagyott |
| Az office hang hívás ellenőrzési lehetőséggel befejezése után elhagyott felhasználó |Elhagyott |
| A felhasználó elhagyott után a beállítás befejezése a biztonsági kérdésekre. |Elhagyott |
| A felhasználó próbálkozás után a felhasználói azonosító megadása |Elhagyott |
| Az e-mail-hitelesítés kihagyásának lehetőségét elindítása után elhagyott felhasználó |Elhagyott |
| A mobileszköz SMS-hitelesítés kihagyásának lehetőségét elindítása után elhagyott felhasználó |Elhagyott |
| A mobileszköz beszédfelismerési hívás ellenőrzési lehetőséggel elindítása után elhagyott felhasználó |Elhagyott |
| A felhasználó próbálkozás után az office hang hívás ellenőrzési lehetőséggel indítása |Elhagyott |
| A felhasználó elhagyott után kezdve a biztonsági kérdések beállítás |Elhagyott |
| A felhasználó egy új jelszót kiválasztása előtt félbehagyná |Elhagyott |
| A felhasználó egy új jelszót kijelölés közben félbehagyná |Elhagyott |
| Felhasználói megadott túl sok érvénytelen SMS ellenőrző kódok kezelésére, és blokkolva van, 24 óra |Letiltva |
| Felhasználó túl sokszor próbált meg mobiltelefonszám ellenőrzése a hang, és blokkolva van, 24 óra |Letiltva |
| Felhasználó túl sokszor próbált meg office Telefonszám ellenőrzése a hang, és blokkolva van, 24 óra |Letiltva |
| Felhasználó biztonsági kérdések megválaszolása túl sokszor próbált és blokkolva van, 24 óra |Letiltva |
| Felhasználó Telefonszám ellenőrzése a túl sokszor próbált és blokkolva van, 24 óra |Letiltva |
| A felhasználó megszakította, mielőtt átadná szükséges hitelesítési módszerek |Törölve |
| A felhasználó megszakította az új jelszó elküldése előtt |Törölve |
| Felhasználó rendszergazda után próbálkozzon az e-mailek ellenőrzési módszerrel érhető el. |Kapcsolat rendszergazda |
| Felhasználó rendszergazda a mobileszköz SMS-hitelesítés kihagyásának lehetőségét tett kísérlet után érhető el. |Kapcsolat rendszergazda |
| Felhasználó rendszergazda után próbálkozzon a mobileszköz beszédfelismerési hívás ellenőrzési módszerrel érhető el. |Kapcsolat rendszergazda |
| Felhasználó rendszergazda után próbálkozzon az office hang hívás ellenőrzési módszerrel érhető el. |Kapcsolat rendszergazda |
| Felhasználói rendszergazda után próbálkozzon a biztonsági kérdés ellenőrzése beállítás érhető el. |Kapcsolat rendszergazda |
| Jelszó alaphelyzetbe állítása nem engedélyezett a felhasználó számára. Jelszó alaphelyzetbe állítása, az a probléma megoldásához konfigurálása lapon engedélyezése |Nem sikerült |
| Felhasználó nem rendelkezik licenccel. A felhasználó a probléma megoldásához hozzáadhat licenc |Nem sikerült |
| A felhasználó megpróbálta alaphelyzetbe állítani egy eszközről anélkül, hogy a cookie-k engedélyezve |Nem sikerült |
| Felhasználói fiók rendelkezik definiált megfelelő hitelesítési módszereket. A probléma megoldásához hitelesítési adatokkal |Nem sikerült |
| Jelszó a helyszínen felügyelt. Engedélyezheti a Jelszóvisszaírást a probléma megoldásához |Nem sikerült |
| Nem sikerült elérni a helyszíni jelszó alaphelyzetbe állítása szolgáltatással. A szinkronizálási számítógépen az eseménynaplóban |Nem sikerült |
| Hiba történt miközben a felhasználó alaphelyzetbe állítása a helyszíni jelszót. A szinkronizálási számítógépen az eseménynaplóban |Nem sikerült |
| A felhasználó nincs a jelszó alaphelyzetbe állítása felhasználók csoport tagja. Ez a felhasználó hozzáadása a csoporthoz, a probléma megoldásához. |Nem sikerült |
| Jelszó alaphelyzetbe állítása le van tiltva ennél a bérlőnél a teljes mértékben. Lásd: [Itt](http://aka.ms/ssprtroubleshoot) a probléma megoldásához. |Nem sikerült |
| Felhasználó sikeresen a jelszó alaphelyzetbe állítása |Sikeres |

## <a name="self-service-password-management-activity-types"></a>Önkiszolgáló jelszókezelés tevékenységtípusok

A következő típusú tevékenységek szerepelnek a **önkiszolgáló jelszókezelés** naplózási esemény kategóriát.  Az alábbiak szerint egyes leírását.

* [**Önkiszolgáló jelszóváltoztatás blokkolva** ](#activity-type-blocked-from-self-service-password-reset) -azt jelzi, hogy egy felhasználó a jelszó alaphelyzetbe állítása, vagy egy adott kapu használni, egy telefonszám ellenőrzése 24 órában teljes 5-nél több alkalommal megpróbálta.
* [**Módosítsa jelszavát (önkiszolgáló)** ](#activity-type-change-password-self-service) -azt jelzi, a felhasználó elvégzett önkéntes, vagy (mert a lejárati) kényszerített a jelszó módosítása.
* [**Jelszó-átállítási (amelyeket a rendszergazda)** ](#activity-type-reset-password-by-admin) -azt jelzi, hogy a rendszergazda végre az Azure-portálról egy felhasználó nevében új jelszót.
* [**Jelszó-átállítási (önkiszolgáló)** ](#activity-type-reset-password-self-service) -azt jelzi, hogy a felhasználó alaphelyzetbe állítása sikeresen megtörtént a jelszavukat a [Azure AD-jelszó-változtatási portál](https://passwordreset.microsoftonline.com).
* [**Az önkiszolgáló jelszó-átállítási folyamata tevékenység folyamatban** ](#activity-type-self-serve-password-reset-flow-activity-progress) -minden adott lépésre, a felhasználó végighalad (például a hitelesítési kapu átadja a jelszó alaphelyzetbe állítása) azt jelzi, mert része a jelszó alaphelyzetbe állítási eljárás.
* [**Felhasználói fiók (önkiszolgáló) feloldása** ](#activity-type-unlock-user-account-self-service) -azt jelzi, hogy a felhasználó sikeresen feloldotta az Active Directory-fiókjában a jelszó alaphelyzetbe állításával a [Azure AD-jelszó-változtatási portál](https://passwordreset.microsoftonline.com) alaphelyzetbe állítása funkció nélkül is feloldhatják az AD-fiókkal.
* [**A felhasználó önkiszolgáló jelszó-visszaállításhoz regisztrálva** ](#activity-type-user-registered-for-self-service-password-reset) -azt jelzi, hogy a felhasználó regisztrálva van-e a szükséges információkat szeretne a jelenleg megadott tenant jelszó-visszaállítási házirend megfelelően jelszó visszaállítása.

### <a name="activity-type-blocked-from-self-service-password-reset"></a>Tevékenységtípus: az önkiszolgáló jelszó-visszaállítás blokkolva

Az alábbi lista ismerteti ennek a tevékenységnek a részletei:

* **Tevékenység leírásában** – annak jelzése, egy rendszergazda felhasználó megpróbálta jelszó alaphelyzetbe állítása, vagy egy adott kapu használni, Telefonszám ellenőrzése a 24 órában teljes 5-nél több alkalommal.
* **Tevékenység Aktor** -lett halmozódni végrehajtása további felhasználó visszaállítási műveletről. A felhasználó vagy egy rendszergazda is lehet.
* **Tevékenység cél** -lett halmozódni végrehajtása további felhasználó visszaállítási műveletről. A felhasználó vagy egy rendszergazda is lehet.
* **Engedélyezett tevékenység állapotai**
  * _Sikeres_ -azt jelzi, hogy a felhasználó lett halmozódni hajt végre semmilyen további alaphelyzetbe állítását, próbálja meg semmilyen további hitelesítési módszerek, vagy minden további telefonszámok érvényesítése a következő 24 órában.
* **Tevékenység állapota a hiba oka** – nem alkalmazható

### <a name="activity-type-change-password-self-service"></a>Tevékenységtípus: jelszó módosítása (önkiszolgáló)

Az alábbi lista ismerteti ennek a tevékenységnek a részletei:

* **Tevékenység leírásában** – azt jelzi, a felhasználó elvégzett önkéntes, vagy (mert a lejárati) kényszerített a jelszó módosítása.
* **Tevékenység Aktor** -felhasználó módosítani a jelszavát. A felhasználó vagy egy rendszergazda is lehet.
* **Tevékenység cél** -felhasználó módosítani a jelszavát. A felhasználó vagy egy rendszergazda is lehet.
* **Engedélyezett tevékenység állapotai**
  * _Sikeres_ -azt jelzi, hogy a felhasználó sikeresen módosította a jelszavát
  * _Hiba_ -azt jelzi, hogy a felhasználó nem sikerült módosítani a jelszavát. Tulajdonsági lehetővé teszi, hogy a **tevékenység állapotának oka** kategória további információt a hiba okát.
* **Tevékenység állapota a hiba oka** - 
  * _FuzzyPolicyViolationInvalidPassword_ – a felhasználó jelszavát, amely automatikusan lett-e tiltani miatt a Microsoft tiltott jelszó észlelés találja túl köznapi vagy különösen gyenge választotta.

### <a name="activity-type-reset-password-by-admin"></a>Tevékenységtípus: jelszó-átállítási (amelyeket a rendszergazda)

Az alábbi lista ismerteti ennek a tevékenységnek a részletei:

* **Tevékenység leírásában** – azt jelzi, hogy a rendszergazda végre az Azure-portálról egy felhasználó nevében új jelszót.
* **Tevékenység Aktor** – a rendszergazda, aki önkiszolgáló a jelszó-változtatási egy másik felhasználó vagy rendszergazda nevében. Kell lennie, vagy egy globális rendszergazda, jelszókezelő, felhasználó, vagy segélyszolgálat rendszergazdája.
* **Tevékenység cél** – a felhasználó, akinek a jelszó alaphelyzetbe állítása. Felhasználó vagy egy másik, rendszergazdai lehet.
* **Engedélyezett tevékenység állapotai**
  * _Sikeres_ -azt jelzi, hogy egy rendszergazda alaphelyzetbe állítása sikeresen megtörtént a jelszó
  * _Hiba_ -azt jelzi, hogy egy rendszergazda a jelszó módosítása sikertelen volt. Tulajdonsági lehetővé teszi, hogy a **tevékenység állapotának oka** kategória további információt a hiba okát.

### <a name="activity-type-reset-password-self-service"></a>Tevékenységtípus: jelszó-átállítási (önkiszolgáló)

Az alábbi lista ismerteti ennek a tevékenységnek a részletei:

* **Tevékenység leírásában** – annak jelzése, a felhasználó alaphelyzetbe állítása sikeresen megtörtént a jelszavukat a [Azure AD-jelszó-változtatási portál](https://passwordreset.microsoftonline.com).
* **Tevékenység Aktor** -jelszó visszaállítása felhasználó. A felhasználó vagy egy rendszergazda is lehet.
* **Tevékenység cél** -jelszó visszaállítása felhasználó. A felhasználó vagy egy rendszergazda is lehet.
* **Engedélyezett tevékenység állapotai**
  * _Sikeres_ -azt jelzi, hogy a felhasználó alaphelyzetbe állítása sikeresen megtörtént a saját jelszavát
  * _Hiba_ -azt jelzi, hogy a felhasználók a saját jelszó visszaállítása sikertelen. Tulajdonsági lehetővé teszi, hogy a **tevékenység állapotának oka** kategória további információt a hiba okát.
* **Tevékenység állapota a hiba oka** -
  * _FuzzyPolicyViolationInvalidPassword_ – a rendszergazdai jelszó, amely automatikusan lett-e tiltani miatt a Microsoft tiltott jelszó észlelés találja túl köznapi vagy különösen gyenge kiválasztva.

### <a name="activity-type-self-serve-password-reset-flow-activity-progress"></a>Tevékenységtípus: önkiszolgáló szolgálnak a jelszó alaphelyzetbe állítása folyamata tevékenység folyamatban

Az alábbi lista ismerteti ennek a tevékenységnek a részletei:

* **Tevékenység leírásában** – azt jelzi, a felhasználó végighalad (például a hitelesítési kapu átadja a jelszó alaphelyzetbe állítása), mert része a jelszó alaphelyzetbe állítási eljárás adott lépésre.
* **Tevékenység Aktor** -jelszó részét végző felhasználó visszaállítása folyamata. A felhasználó vagy egy rendszergazda is lehet.
* **Tevékenység cél** -jelszó részét végző felhasználó visszaállítása folyamata. A felhasználó vagy egy rendszergazda is lehet.
* **Engedélyezett tevékenység állapotai**
  * _Sikeres_ -azt jelzi, hogy a felhasználó sikeresen befejezte a jelszóvisszaállítási folyamatot az adott lépésre.
  * _Hiba_ -azt jelzi, hogy egy adott lépésre a jelszó alaphelyzetbe állítása nem sikerült folyamat. Tulajdonsági lehetővé teszi, hogy a **tevékenység állapotának oka** kategória további információt a hiba okát.
* **Engedélyezett tevékenység állapotának oka**
  * Lásd az alábbi táblázatban [minden engedélyezett visszaállítási tevékenység állapotának oka](#allowed-values-for-details-column)

### <a name="activity-type-unlock-user-account-self-service"></a>Tevékenységtípus: felhasználói fiók (önkiszolgáló) feloldása

Az alábbi lista ismerteti ennek a tevékenységnek a részletei:

* **Tevékenység leírásában** – annak jelzése nyitása sikeresen megtörtént az Active Directory-fiókot a felhasználó a jelszó alaphelyzetbe állításával a [Azure AD-jelszó-változtatási portál](https://passwordreset.microsoftonline.com) alaphelyzetbe állítása funkció nélkül is feloldhatják az AD-fiókkal.
* **Tevékenység Aktor** -felhasználó nélkül a jelszó alaphelyzetbe állításával a fiók zárolását. A felhasználó vagy egy rendszergazda is lehet.
* **Tevékenység cél** -felhasználó nélkül a jelszó alaphelyzetbe állításával a fiók zárolását. A felhasználó vagy egy rendszergazda is lehet.
* **Engedélyezett tevékenység állapotai**
  * _Sikeres_ -azt jelzi, hogy a felhasználók a saját fiókjuk nyitása sikeresen megtörtént
  * _Hiba_ -azt jelzi, hogy a felhasználó nem tudta feloldhatja a fiók zárolását. Tulajdonsági lehetővé teszi, hogy a **tevékenység állapotának oka** kategória további információt a hiba okát.

### <a name="activity-type-user-registered-for-self-service-password-reset"></a>Tevékenységtípus: felhasználó regisztrált az önkiszolgáló jelszóváltoztatáshoz

Az alábbi lista ismerteti ennek a tevékenységnek a részletei:

* **Tevékenység leírásában** – azt jelzi, hogy a felhasználó regisztrálva van-e a szükséges információkat szeretne a jelenleg megadott tenant jelszó-visszaállítási házirend megfelelően jelszó visszaállítása. 
* **Tevékenység Aktor** -jelszó alaphelyzetbe állítása regisztrált felhasználó. A felhasználó vagy egy rendszergazda is lehet.
* **Tevékenység cél** -jelszó alaphelyzetbe állítása regisztrált felhasználó. A felhasználó vagy egy rendszergazda is lehet.
* **Engedélyezett tevékenység állapotai**
  * _Sikeres_ -azt jelzi, a felhasználó sikerült regisztrálni a jelszó alaphelyzetbe állítása, az aktuális nyilatkozatnak. 
  * _Hiba_ -azt jelzi, hogy a felhasználó nem tudta regisztrálni a jelszó-visszaállításhoz. Tulajdonsági lehetővé teszi, hogy a **tevékenység állapotának oka** kategória további információt a hiba okát. Megjegyzés: Ez nem jelenti a felhasználó számára, hogy azok nem fejeződött be a regisztrációs folyamat csak a saját jelszavát. Ha nem ellenőrzött adatok a fiókra, amely a megfelelő (például egy telefonszámot, amelyet a rendszer nem érvényesíti), annak ellenére, hogy nem ellenőrizte, hogy ez a telefonszám, továbbra is használhatják a jelszó alaphelyzetbe állításához. További információkért lásd: [mi történik, ha a felhasználó regisztrál?](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-learn-more#what-happens-when-a-user-registers)

## <a name="next-steps"></a>Következő lépések

Az alábbi hivatkozásokat követve az Azure AD jelszóátállításáról olvashat további információkat.

* [A felhasználó felügyeleti naplók helyi](https://portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/Audit) - lépjen közvetlenül a bérlő felhasználók kezelése a naplók
* [**Gyors üzembe helyezés**](active-directory-passwords-getting-started.md) – Percek alatt üzembe helyezheti az Azure AD önkiszolgáló jelszókezelőjét. 
* [**Licencelés**](active-directory-passwords-licensing.md) – Az Azure AD licencelésének konfigurálása.
* [**Adatok**](active-directory-passwords-data.md) – A szükséges adatok megismerése, és az adatok használata a rendszer jelszókezelésre.
* [**Bevezetés**](active-directory-passwords-best-practices.md) – Az SSPR funkció tervezése és üzembe helyezése a felhasználók számára az itt található útmutató segítségével.
* [**Testreszabás**](active-directory-passwords-customize.md) – Az SSPR-felület megjelenésének és működésének testre szabása a cége számára.
* [**Részletes műszaki bemutatás**](active-directory-passwords-how-it-works.md) – Egy pillantás a függöny mögé, hogy megértse, hogyan is működik.
* [**Gyakori kérdések**](active-directory-passwords-faq.md) – Hogyan? Hogy miért? Mi? Hová? Ki? Mikor? – Válaszok olyan kérdésekre, amiket mindig is fel akart tenni
* [**Hibaelhárítás**](active-directory-passwords-troubleshoot.md) – Ismerje meg, hogyan oldhat meg általános, az SSPR működése során jelentkező hibákat.
* [**Szabályzat**](active-directory-passwords-policy.md) – Megismerheti és beállíthatja az Azure AD jelszószabályzatait.
