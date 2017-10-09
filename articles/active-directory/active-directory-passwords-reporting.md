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
ms.openlocfilehash: af65b9be1e00c2819431694a5b0064b97b9e1867
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="reporting-options-for-azure-ad-password-management"></a>Jelentéskészítési lehetőségek az Azure AD-jelszókezelés

Központi telepítés a számos szervezet hogyan szeretné tooknow, vagy ha SSPR valóban használja fel. Az Azure AD jelentéskészítési funkciókat, amelyek segítségével tooanswer kérdések segítenek konzerv jelentéseket biztosít, és ha megfelelően rendelkezik licenccel, lehetővé teszi az toocreate egyéni lekérdezéseket.

hello következő tud válaszolni kérdéseket jelentésekben szereplő hello [Azure-portálon] (https://portal.azure.com/).

> [!NOTE]
> Kell [egy globális rendszergazda](active-directory-assign-admin-roles.md#assign-or-remove-administrator-roles) és kell részt az ezzel a szervezet nevében összegyűjtött adatok toobe által meghívott hello lapon vagy a napló reporting naplózza legalább egyszer. Így, amíg adatok nem lesznek összegyűjtve a szervezet számára

* Jelszó alaphelyzetbe állítása hányan regisztrált?
* Aki regisztrálva van a jelszó alaphelyzetbe állítása?
* Milyen adatokat próbál regisztrálni személyek?
* Hányan az elmúlt hét napban hello jelszavak alaphelyzetbe?
* Mik azok a hello leggyakrabban használt módszerek felhasználók vagy rendszergazdák használja tooreset jelszavukat?
* Mik a közös állít felhasználók vagy rendszergazdák arc, toouse jelszó alaphelyzetbe állítása közben?
* Milyen a rendszergazdák gyakran alaphelyzetbe állítja saját jelszavukat?
* A gyanús tevékenységeket, a jelszó alaphelyzetbe állítása folyamatban van?

## <a name="how-tooview-password-management-reports-in-hello-azure-portal"></a>Hogyan tooview jelszókezelés jelentései hello Azure-portálon

Hello Azure portál felhasználói élmény, a továbbfejlesztett módon tooview jelszó alaphelyzetbe állítása és a jelszó alaphelyzetbe állítása regisztrációs tevékenység van.  Kövesse az alábbi toofind hello jelszó alaphelyzetbe állítása és a jelszó alaphelyzetbe állítása regisztrációs események hello lépéseket:

1. Keresse meg a túl[**portal.azure.com**](https://portal.azure.com)
2. Kattintson a hello **további szolgáltatások** hello fő az Azure portál bal oldali navigációs menü
3. Keresse meg **Azure Active Directory** a hello szolgáltatások listájában, és jelölje ki
4. Kattintson a **felhasználók és csoportok** hello Azure Active Directory navigációs menü
5. Kattintson a hello **naplók** navigációs elem hello felhasználók és csoportok navigációs menüjében. Megjelenik az összes hello naplózási eseményekből a címtárban szereplő összes hello felhasználó ellen. A nézet toosee összes hello jelszó-események, is végezhet.
6. toofilter a nézet tooonly hello jelszó-átállítási kapcsolódó események, kattintson a hello **szűrő** hello panel felső hello gombra.
7. A hello **szűrő** menü, jelölje be hello **kategória** legördülő menüből, és módosítsa úgy toohello **önkiszolgáló jelszókezelés** kategória típusa.
8. Nem kötelező további hello szűrőlista hello specifikus kiválasztásával **tevékenység** érdekli

## <a name="how-tooretrieve-password-management-events-from-hello-azure-ad-reports-and-events-api"></a>Hogyan tooretrieve jelszó felügyeleti események hello Azure AD-jelentéseket és események API

hello Azure AD-jelentéseket és események API támogatja a jelszó alaphelyzetbe állítása és a jelszó alaphelyzetbe állítása szóregisztrációs jelentéseket szereplő összes hello adatok beolvasása. Ez az API használatával letöltheti az egyes jelszó alaphelyzetbe állítása és jelszó-változtatási regisztrációs események az integráció a technológia a kiválasztott jelentési hello.

### <a name="how-tooget-started-with-hello-reporting-api"></a>Hogyan a tooget hello reporting API használatába

tooaccess ezeket az adatokat kell toowrite egy kis alkalmazást vagy parancsfájl-tooretrieve azt a kiszolgálóink. [Ismerje meg, hogyan tooget használatába hello Azure AD jelentéskészítési API](active-directory-reporting-api-getting-started.md).

Ha már van egy működő parancsfájl, érdemes tooexamine hello jelszó alaphelyzetbe állítása és nyilvántartási események olvashatók be toomeet a forgatókönyvek mellett.

* [SsprActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent): hello elérhető oszlopok listája, a jelszó-átállítási események
* [SsprRegistrationActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprRegistrationActivityEvent): hello elérhető oszlopok listája, a jelszó-átállítási regisztráció események

### <a name="reporting-api-data-retrieval-limitations"></a>Jelentéskészítési API adatok lekérését korlátozásai

Jelenleg az Azure AD-jelentések hello és események API túl veszi fel**75,000 események** a hello [SsprActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent) és [SsprRegistrationActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprRegistrationActivityEvent) típusok , hello átfedés **utolsó 30 nap**.

Ha tooretrieve kell, vagy ez az ablak adatok tárolására, javasoljuk, hogy külső adatbázis megőrzése, és hello API tooquery hello eltérések eredményező használatával. Azt javasoljuk, ezek az adatok beolvasása SSPR indíthatja a szervezetében, megőrizni a külsőleg, és folytassa előre tootrack hello eltérések ettől toobegin.

## <a name="how-toodownload-password-reset-registration-events-quickly-with-powershell"></a>Hogyan toodownload jelszó-változtatási regisztrációs események gyorsan a PowerShell használatával

Továbbá toousing hello Azure AD-jelentéseket és események API közvetlenül, a PowerShell parancsfájl toorecent regisztrációs események alatt hello is használhatja a címtárban. Ez akkor hasznos, ha azt szeretné, hogy toosee, akik mostanában regisztrálva van, vagy szeretné például, hogy a jelszó-átállítási bevezetés tooensure várt folyamatban van.

* [Az Azure AD SSPR regisztrációs tevékenység PowerShell-parancsfájl](https://gallery.technet.microsoft.com/scriptcenter/azure-ad-self-service-e31b8aee)

### <a name="description-of-report-columns-in-azure-portal"></a>Azure-portálon jelentés oszlopok leírása

hello alábbi lista ismerteti részletesen hello jelentés oszlopainak mindegyike:

* **Felhasználói** – hello felhasználó, aki kísérlet történt a jelszó alaphelyzetbe állítása a regisztrációs műveletet.
* **Szerepkör** – hello szerepkör hello felhasználó hello könyvtárban.
* **Dátum és idő** – hello dátum és idő hello kísérlet.
* **Adatok regisztrált** – milyen hitelesítési adatok hello felhasználó által megadott során jelszó-átállítási regisztrációk.

### <a name="description-of-report-values-in-azure-portal"></a>Azure-portálon jelentés értékek leírása

hello következő táblázat minden egyes oszlophoz engedélyezett hello különböző értékeket:

| Oszlop | Megengedett értékek és azok jelentését |
| --- | --- |
| Regisztrált adatok |**Másodlagos e-mail-címet** – használt felhasználó másodlagos e-mail vagy hitelesítési e-mail tooauthenticate<p><p>**Irodai telefon**– a felhasználó használt office phone tooauthenticate<p>**Mobiltelefon** -felhasználó használt mobiltelefonját vagy hitelesítési phone tooauthenticate<p>**Biztonsági kérdések** – felhasználói használt biztonsági kérdések tooauthenticate<p>**Hello fölött (például a másodlagos E-mail + a mobiltelefon) kombinációja** – 2 kapu házirend van megadva, és bemutatja, melyik két módszer hello használt felhasználói tooauthentication következik be a jelszó-változtatási kérelmet. |

## <a name="view-password-reset-activity-in-hello-classic-portal"></a>Nézet jelszó-visszaállítási tevékenység hello klasszikus portál

Ez a jelentés tartalmazza az összes jelszó alaphelyzetbe állítása, amely a szervezet történt kísérlet.

* **Maximális időtartomány**: 30 napban
* **A sorok maximális számát**: 75 000
* **Letölthető**: Igen, a CSV-fájl használatával

### <a name="description-of-report-columns-in-azure-classic-portal"></a>A klasszikus Azure portálon jelentés oszlopok leírása

hello alábbi lista ismerteti részletesen hello jelentés oszlopainak mindegyike:

1. **Felhasználói** – hello felhasználó, aki kísérlet történt a jelszó alaphelyzetbe állítása (ha hello felhasználói jelszó tooreset megadott hello felhasználói azonosító mező alapján) műveletet.
2. **Szerepkör** – hello szerepkör hello felhasználó hello könyvtárban.
3. **Dátum és idő** – hello dátum és idő hello kísérlet.
4. **Módszerek használható** – hitelesítési módszerek hello használt felhasználói a visszaállítási műveletet.
5. **Eredmény** – hello eredményét hello jelszó alaphelyzetbe állítása a műveletet.
6. **Részletek** – hello részleteit miért hello jelszó-átállítási ugyanúgy hello értéket eredményezett.  Is tartalmaz a megoldás lépéseket vehet tooresolve, akkor nem várt hiba történt.

### <a name="description-of-report-values-in-azure-classic-portal"></a>A jelentés értékeket a klasszikus Azure portálon leírása

hello következő táblázat minden egyes oszlophoz engedélyezett hello különböző értékeket:

| Oszlop | Megengedett értékek és azok jelentését |
| --- | --- |
| Használt módszerek |**Másodlagos e-mail-címet** – használt felhasználó másodlagos e-mail vagy hitelesítési e-mail tooauthenticate<p>**Irodai telefon** – a felhasználó használt office phone tooauthenticate<p>**Mobiltelefon** – a felhasználó használt mobiltelefonját vagy hitelesítési phone tooauthenticate<p>**Biztonsági kérdések** – felhasználói használt biztonsági kérdések tooauthenticate<p>**Hello fölött (például a másodlagos E-mail + a mobiltelefon) kombinációja** – 2 kapu házirend van megadva, és bemutatja, melyik két módszer hello használt felhasználói tooauthentication következik be a jelszó-változtatási kérelmet. |
| eredménye |**Elhagyott** – felhasználói jelszó-visszaállítás elindult, de le félúton helyezkedjenek el keresztül nélkül befejezése<p>**Blokkolt** – felhasználói fiók volt megakadályozta abban toouse jelszó-visszaállítás miatt tooattempting toouse hello jelszó alaphelyzetbe állítása lap, vagy egyetlen jelszó kapu visszaállítása egy 24 órás időtartamon belül túl sokszor<p>**Visszavont** – felhasználói jelszó-visszaállítás elindult, de rákattintva hello Mégse gomb toocancel hello munkamenet részben módon keresztül <p>**Rendszergazda-e kommunikálni** – felhasználói probléma volt a munkamenetben, hogy nem oldható fel, így hello felhasználó helyett befejezési hello "Forduljon a rendszergazdához" hivatkozásra kattint hello jelszó-átállítási folyamata<p>**Nem sikerült** – felhasználó nem volt képes tooreset jelszót, valószínűleg hello felhasználói nem kellett konfigurált toouse hello szolgáltatást (például nem licenc, hiányzó hitelesítési adatokat, jelszó által kezelt helyszíni, de visszaírás le van tiltva).<p>**Sikeres** – jelszó-visszaállítás sikeres volt. |
| Részletek |Az alábbi táblázatban találja |

### <a name="allowed-values-for-details-column"></a>Engedélyezett részletek oszlop értékei

Az alábbiakban olvashat tevékenységgel kapcsolatos jelentés használatával hello jelszó alaphelyzetbe állítása során előfordulhat, hogy várt eredménytípusai hello listát:

| Részletek | Eredménytípus |
| --- | --- |
| A felhasználó próbálkozás után hello e-mail ellenőrzési módszert befejezése |Elhagyott |
| A felhasználó próbálkozás után hello mobil SMS ellenőrzési módszert befejezése |Elhagyott |
| A felhasználó próbálkozás után hello mobileszköz beszédfelismerési hívás ellenőrzési módszert befejezése |Elhagyott |
| A felhasználó próbálkozás után hello office hang hívás ellenőrzési módszert befejezése |Elhagyott |
| A felhasználó próbálkozás hello biztonsági kérdések beállítás befejezése után |Elhagyott |
| A felhasználó próbálkozás után a felhasználói azonosító megadása |Elhagyott |
| A felhasználó próbálkozás után hello e-mail ellenőrzési módszert indítása |Elhagyott |
| A felhasználó próbálkozás után hello mobil SMS ellenőrzési módszert indítása |Elhagyott |
| A felhasználó próbálkozás után hello mobileszköz beszédfelismerési hívás ellenőrzési módszert indítása |Elhagyott |
| A felhasználó próbálkozás után hello office hang hívás ellenőrzési módszert indítása |Elhagyott |
| A felhasználó próbálkozás után hello biztonsági kérdések beállítás indítása |Elhagyott |
| A felhasználó egy új jelszót kiválasztása előtt félbehagyná |Elhagyott |
| A felhasználó egy új jelszót kijelölés közben félbehagyná |Elhagyott |
| Felhasználói megadott túl sok érvénytelen SMS ellenőrző kódok kezelésére, és blokkolva van, 24 óra |Letiltva |
| Felhasználó túl sokszor próbált meg mobiltelefonszám ellenőrzése a hang, és blokkolva van, 24 óra |Letiltva |
| Felhasználó túl sokszor próbált meg office Telefonszám ellenőrzése a hang, és blokkolva van, 24 óra |Letiltva |
| Felhasználó túl sokszor próbált meg tooanswer biztonsági kérdéseit, és 24 órán keresztül le van tiltva |Letiltva |
| Felhasználó túl sokszor próbált meg tooverify egy telefonszámot, és 24 órán keresztül le van tiltva |Letiltva |
| A felhasználó megszakította, mielőtt átadná hello szükséges hitelesítési módszerek |Törölve |
| A felhasználó megszakította az új jelszó elküldése előtt |Törölve |
| Felhasználói rendszergazda után próbálkozzon hello e-mail ellenőrzési lehetőség érhető el. |Kapcsolat rendszergazda |
| Felhasználó rendszergazda után próbálkozzon hello mobil SMS ellenőrzési lehetőség érhető el. |Kapcsolat rendszergazda |
| Felhasználó rendszergazda után próbálkozzon hello mobileszköz beszédfelismerési hívás ellenőrzési lehetőség érhető el. |Kapcsolat rendszergazda |
| Felhasználó rendszergazda után próbálkozzon hello office hang hívás ellenőrzési lehetőség érhető el. |Kapcsolat rendszergazda |
| Felhasználó rendszergazda próbált hello biztonsági kérdés ellenőrzése beállítás után érhető el. |Kapcsolat rendszergazda |
| Jelszó alaphelyzetbe állítása nem engedélyezett a felhasználó számára. Jelszó alaphelyzetbe állítása a hello engedélyezése beállítás lapon tooresolve |Nem sikerült |
| Felhasználó nem rendelkezik licenccel. Ez a licenc toohello felhasználói tooresolve adhat hozzá |Nem sikerült |
| A felhasználó egy eszközről tooreset próbált nélkül cookie-k engedélyezve |Nem sikerült |
| Felhasználói fiók rendelkezik definiált megfelelő hitelesítési módszereket. Hitelesítési adatai tooresolve felvétele |Nem sikerült |
| Jelszó a helyszínen felügyelt. Engedélyezheti a Jelszóvisszaírást tooresolve Ez |Nem sikerült |
| Nem sikerült elérni a helyszíni jelszó alaphelyzetbe állítása szolgáltatással. A szinkronizálási számítógépen az eseménynaplóban |Nem sikerült |
| Hello helyszíni jelszó alaphelyzetbe állítása során hiba történt. A szinkronizálási számítógépen az eseménynaplóban |Nem sikerült |
| A felhasználó nincs hello jelszó alaphelyzetbe állítása felhasználók csoport tagja. Adja hozzá a felhasználó toothat csoport tooresolve ezt. |Nem sikerült |
| Jelszó alaphelyzetbe állítása le van tiltva ennél a bérlőnél a teljes mértékben. Lásd: [Itt](http://aka.ms/ssprtroubleshoot) tooresolve ez. |Nem sikerült |
| Felhasználó sikeresen a jelszó alaphelyzetbe állítása |Sikeres |

## <a name="self-service-password-management-activity-types"></a>Önkiszolgáló jelszókezelés tevékenységtípusok

a következő típusú tevékenységek hello jelennek meg hello **önkiszolgáló jelszókezelés** naplózási esemény kategória.  Az alábbiak szerint egyes leírását.

* [**Önkiszolgáló jelszóváltoztatás blokkolva** ](#activity-type-blocked-from-self-service-password-reset) -azt jelzi, hogy egy felhasználó próbált tooreset jelszót, egy adott kapu használja, vagy egy telefonszám ellenőrzése 24 órában teljes 5-nél több alkalommal.
* [**Módosítsa jelszavát (önkiszolgáló)** ](#activity-type-change-password-self-service) -azt jelzi, a felhasználó önkéntes végre, vagy a kényszerített (esedékes tooexpiry) a jelszó módosítása.
* [**Jelszó-átállítási (amelyeket a rendszergazda)** ](#activity-type-reset-password-by-admin) -azt jelzi, hogy a rendszergazda végre az Azure-portálon hello egy felhasználó nevében új jelszót.
* [**Jelszó-átállítási (önkiszolgáló)** ](#activity-type-reset-password-self-service) -azt jelzi, hogy a felhasználó sikeresen hello a jelszó visszaállítása [Azure AD-jelszó-változtatási portál](https://passwordreset.microsoftonline.com).
* [**Az önkiszolgáló jelszó-átállítási folyamata tevékenység folyamatban** ](#activity-type-self-serve-password-reset-flow-activity-progress) -minden felhasználó végighalad (például a hitelesítési kapu átadja a jelszó alaphelyzetbe állítása) meghatározott lépés azt jelzi, része hello jelszó alaphelyzetbe állítási eljárás szerint.
* [**Felhasználói fiók (önkiszolgáló) feloldása** ](#activity-type-unlock-user-account-self-service) -azt jelzi, hogy a felhasználó sikeresen feloldotta az Active Directory-fiókjában hello a jelszó alaphelyzetbe állításával [Azure AD-jelszó-változtatási portál](https://passwordreset.microsoftonline.com) használatával hello AD-fiókot zárolásának feloldása nélkül alaphelyzetbe állítása lehetőséget.
* [**A felhasználó önkiszolgáló jelszó-visszaállításhoz regisztrálva** ](#activity-type-user-registered-for-self-service-password-reset) -azt jelzi, hogy a felhasználó regisztrálva van-e minden szükséges hello információk toobe képes tooreset megfelelően hello jelszavukat aktuálisan megadott bérlői jelszó-visszaállítási házirend.

### <a name="activity-type-blocked-from-self-service-password-reset"></a>Tevékenységtípus: az önkiszolgáló jelszó-visszaállítás blokkolva

hello a következő lista ismerteti ennek a tevékenységnek a részletei:

* **Tevékenység leírásában** – jelzi a felhasználó próbált tooreset jelszót, vagy egy adott kapu használni, egy telefonszám ellenőrzése 24 órában teljes 5-nél több alkalommal.
* **Tevékenység Aktor** -végrehajtása további lett halmozódni hello felhasználó visszaállítási műveletről. A felhasználó vagy egy rendszergazda is lehet.
* **Tevékenység cél** -végrehajtása további lett halmozódni hello felhasználó visszaállítási műveletről. A felhasználó vagy egy rendszergazda is lehet.
* **Engedélyezett tevékenység állapotai**
  * _Sikeres_ -azt jelzi, hogy a felhasználó lett halmozódni hajt végre semmilyen további alaphelyzetbe állítását, próbálja meg semmilyen további hitelesítési módszerek, vagy ellenőrizze minden további telefonszámainak hello 24 órán át.
* **Tevékenység állapota a hiba oka** – nem alkalmazható

### <a name="activity-type-change-password-self-service"></a>Tevékenységtípus: jelszó módosítása (önkiszolgáló)

hello a következő lista ismerteti ennek a tevékenységnek a részletei:

* **Tevékenység leírásában** – azt jelzi, a felhasználó önkéntes végre, vagy a kényszerített (esedékes tooexpiry) a jelszó módosítása.
* **Tevékenység Aktor** -hello felhasználó, aki módosítani a jelszavát. A felhasználó vagy egy rendszergazda is lehet.
* **Tevékenység cél** -hello felhasználó, aki módosítani a jelszavát. A felhasználó vagy egy rendszergazda is lehet.
* **Engedélyezett tevékenység állapotai**
  * _Sikeres_ -azt jelzi, hogy a felhasználó sikeresen módosította a jelszavát
  * _Hiba_ -azt jelzi, hogy a felhasználó nem tudta toochange a jelszavát. Gombra kattintva hello sor lehetővé teszi a toosee hello **tevékenység állapotának oka** kategória toolearn többet miért hello hiba történt.
* **Tevékenység állapota a hiba oka** - 
  * _FuzzyPolicyViolationInvalidPassword_ -hello felhasználó megadott jelszó, amely automatikusan lett-e tiltani tooMicrosoft által tiltott jelszó észlelés találja toobe túl köznapi vagy különösen gyenge miatt.

### <a name="activity-type-reset-password-by-admin"></a>Tevékenységtípus: jelszó-átállítási (amelyeket a rendszergazda)

hello a következő lista ismerteti ennek a tevékenységnek a részletei:

* **Tevékenység leírásában** – azt jelzi, hogy a rendszergazda végre az Azure-portálon hello egy felhasználó nevében új jelszót.
* **Tevékenység Aktor** -hello jelszó alaphelyzetbe állítása, egy másik felhasználó vagy rendszergazda nevében végző hello rendszergazda. Kell lennie, vagy egy globális rendszergazda, jelszókezelő, felhasználó, vagy segélyszolgálat rendszergazdája.
* **Tevékenység cél** -hello felhasználó, amelynek a jelszó alaphelyzetbe állítása. Felhasználó vagy egy másik, rendszergazdai lehet.
* **Engedélyezett tevékenység állapotai**
  * _Sikeres_ -azt jelzi, hogy egy rendszergazda alaphelyzetbe állítása sikeresen megtörtént a jelszó
  * _Hiba_ -azt jelzi, hogy egy rendszergazda nem tudta toochange egy felhasználó jelszavát. Gombra kattintva hello sor lehetővé teszi a toosee hello **tevékenység állapotának oka** kategória toolearn többet miért hello hiba történt.

### <a name="activity-type-reset-password-self-service"></a>Tevékenységtípus: jelszó-átállítási (önkiszolgáló)

hello a következő lista ismerteti ennek a tevékenységnek a részletei:

* **Tevékenység leírásában** – azt jelzi, hogy a felhasználó sikeresen hello a jelszó visszaállítása [Azure AD-jelszó-változtatási portál](https://passwordreset.microsoftonline.com).
* **Tevékenység Aktor** -jelszó visszaállítása hello felhasználó. A felhasználó vagy egy rendszergazda is lehet.
* **Tevékenység cél** -jelszó visszaállítása hello felhasználó. A felhasználó vagy egy rendszergazda is lehet.
* **Engedélyezett tevékenység állapotai**
  * _Sikeres_ -azt jelzi, hogy a felhasználó alaphelyzetbe állítása sikeresen megtörtént a saját jelszavát
  * _Hiba_ -azt jelzi, hogy a felhasználó nem tudta tooreset a saját jelszavát. Gombra kattintva hello sor lehetővé teszi a toosee hello **tevékenység állapotának oka** kategória toolearn többet miért hello hiba történt.
* **Tevékenység állapota a hiba oka** -
  * _FuzzyPolicyViolationInvalidPassword_ -Üdvözöljük a rendszergazdákat kijelölt jelszó, amely automatikusan lett-e tiltani tooMicrosoft által tiltott jelszó észlelés találja toobe túl köznapi vagy különösen gyenge miatt.

### <a name="activity-type-self-serve-password-reset-flow-activity-progress"></a>Tevékenységtípus: önkiszolgáló szolgálnak a jelszó alaphelyzetbe állítása folyamata tevékenység folyamatban

hello a következő lista ismerteti ennek a tevékenységnek a részletei:

* **Tevékenység leírásában** – azt jelzi, a felhasználó végighalad (például a hitelesítési kapu átadja a jelszó alaphelyzetbe állítása), része hello jelszó alaphelyzetbe állítási eljárás adott lépésre.
* **Tevékenység Aktor** -hello jelszó részét hello felhasználó visszaállítása folyamata. A felhasználó vagy egy rendszergazda is lehet.
* **Tevékenység cél** -hello jelszó részét hello felhasználó visszaállítása folyamata. A felhasználó vagy egy rendszergazda is lehet.
* **Engedélyezett tevékenység állapotai**
  * _Sikeres_ -azt jelzi, hogy a felhasználó sikeresen befejezte egy adott lépésre a hello jelszóvisszaállítási folyamatot.
  * _Hiba_ -azt jelzi, hogy egy adott lépésre hello jelszó alaphelyzetbe állítása nem sikerült folyamat. Gombra kattintva hello sor lehetővé teszi a toosee hello **tevékenység állapotának oka** kategória toolearn többet miért hello hiba történt.
* **Engedélyezett tevékenység állapotának oka**
  * Lásd az alábbi táblázatban [minden engedélyezett visszaállítási tevékenység állapotának oka](#allowed-values-for-details-column)

### <a name="activity-type-unlock-user-account-self-service"></a>Tevékenységtípus: felhasználói fiók (önkiszolgáló) feloldása

hello a következő lista ismerteti ennek a tevékenységnek a részletei:

* **Tevékenység leírásában** – azt jelzi, hogy a felhasználó sikeresen feloldotta az Active Directory-fiókjában hello a jelszó alaphelyzetbe állításával [Azure AD-jelszó-változtatási portál](https://passwordreset.microsoftonline.com) visszaállítása nélkül is feloldhatják hello AD fiók használatával a szolgáltatás.
* **Tevékenység Aktor** -hello felhasználó, aki a fiók zárolását nélkül a jelszó alaphelyzetbe állításával. A felhasználó vagy egy rendszergazda is lehet.
* **Tevékenység cél** -hello felhasználó, aki a fiók zárolását nélkül a jelszó alaphelyzetbe állításával. A felhasználó vagy egy rendszergazda is lehet.
* **Engedélyezett tevékenység állapotai**
  * _Sikeres_ -azt jelzi, hogy a felhasználók a saját fiókjuk nyitása sikeresen megtörtént
  * _Hiba_ -azt jelzi, hogy a felhasználó nem tudta toounlock fiókjuk. Gombra kattintva hello sor lehetővé teszi a toosee hello **tevékenység állapotának oka** kategória toolearn többet miért hello hiba történt.

### <a name="activity-type-user-registered-for-self-service-password-reset"></a>Tevékenységtípus: felhasználó regisztrált az önkiszolgáló jelszóváltoztatáshoz

hello a következő lista ismerteti ennek a tevékenységnek a részletei:

* **Tevékenység leírásában** – azt jelzi, hogy a felhasználó regisztrálva van-e minden szükséges hello információk toobe képes tooreset megfelelően hello jelszavukat aktuálisan megadott bérlői jelszó-visszaállítási házirend. 
* **Tevékenység Aktor** -hello regisztrált felhasználó folyamatos jelszó-visszaállításhoz. A felhasználó vagy egy rendszergazda is lehet.
* **Tevékenység cél** -hello regisztrált felhasználó folyamatos jelszó-visszaállításhoz. A felhasználó vagy egy rendszergazda is lehet.
* **Engedélyezett tevékenység állapotai**
  * _Sikeres_ -azt jelzi, a felhasználók jelszó-változtatási hello aktuális nyilatkozatnak regisztrálása sikeres volt. 
  * _Hiba_ -azt jelzi, a felhasználó nem sikerült tooregister jelszó-visszaállításhoz. Gombra kattintva hello sor lehetővé teszi a toosee hello **tevékenység állapotának oka** kategória toolearn többet miért hello hiba történt. Megjegyzés: Ez nem jelenti a felhasználó nem tooreset van a saját jelszavát, csak, hogy azok nem fejeződött be hello regisztrációs folyamat során. Ha nem ellenőrzött adatok a fiókra, amely a megfelelő (például egy telefonszámot, amelyet a rendszer nem érvényesíti), annak ellenére, hogy nem ellenőrizte, hogy ez a telefonszám, továbbra is használhatják azt tooreset a jelszavát. További információkért lásd: [mi történik, ha a felhasználó regisztrál?](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-learn-more#what-happens-when-a-user-registers)

## <a name="next-steps"></a>Következő lépések

a következő hivatkozások hello adja meg a jelszó alaphelyzetbe állítása, az Azure AD használatával kapcsolatos további információk

* [Helyi toouser felügyeleti naplók](https://portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/Audit) - lépjen közvetlenül tooyour bérlői felhasználói felügyeleti naplók
* [**Gyors üzembe helyezés**](active-directory-passwords-getting-started.md) – Percek alatt üzembe helyezheti az Azure AD önkiszolgáló jelszókezelőjét. 
* [**Licencelés**](active-directory-passwords-licensing.md) – Az Azure AD licencelésének konfigurálása.
* [**Adatok** ](active-directory-passwords-data.md) - szükséges hello adatok megismeréséhez, és hogyan használja fel azokat a jelszókezelés
* [**Bevezetés** ](active-directory-passwords-best-practices.md) -megtervezése és telepítése az önkiszolgáló jelszó-Változtatási tooyour felhasználók hello útmutatást itt talál
* [**Testre szabhatja** ](active-directory-passwords-customize.md) -testreszabása, önkiszolgáló jelszó-Változtatási élményt a vállalata hello hello megjelenését és működését.
* [**Műszaki mélyreható** ](active-directory-passwords-how-it-works.md) -mögött hello függöny toounderstand nyissa meg annak működéséről
* [**Gyakori kérdések**](active-directory-passwords-faq.md) – Hogyan? Hogy miért? Mi? Hová? Ki? Mikor? -Válaszok mindig kívánta tooask tooquestions
* [**Hibaelhárítás** ](active-directory-passwords-troubleshoot.md) -megtudhatja, hogyan tooresolve közös állít ki, hogy az önkiszolgáló jelszó-Változtatási látható
* [**Szabályzat**](active-directory-passwords-policy.md) – Megismerheti és beállíthatja az Azure AD jelszószabályzatait.
