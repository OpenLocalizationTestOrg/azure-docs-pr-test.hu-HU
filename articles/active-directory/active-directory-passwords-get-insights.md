---
title: "Get Insights: Az Azure ad jelentést |} Microsoft Docs"
description: "Ez a cikk ismerteti a jelentések segítségével a szervezet jelszó felügyeleti műveletek webhelynaplókat."
services: active-directory
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: gahug
ms.assetid: 1472b51d-53f4-4b0f-b1be-57f6fa88fa65
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: ae83df618e3c392fe89878bcd1be0d6c6cb1edb4
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-get-operational-insights-with-password-management-reports"></a>Jelentések lekérése az operational insights jelszókezelés
> [!IMPORTANT]
> **Azért van itt, mert problémák merültek fel a bejelentkezéssel kapcsolatban?** Ha igen, [így módosíthatja vagy állíthatja alaphelyzetbe a jelszavát](active-directory-passwords-update-your-own-password.md#reset-or-unlock-my-password-for-a-work-or-school-account).
>
>

Ez a szakasz ismerteti, hogyan használhatja Azure Active Directory jelszó jelentések megtekintéséhez, hogy a felhasználók miként használnak a jelszó alaphelyzetbe állítása és módosítása a szervezetében.

* [**Jelszó felügyeleti jelentések áttekintése**](#overview-of-password-management-reports)
* [**Az új Azure-portálon jelszó jelentések megtekintése**](#how-to-view-password-management-reports)
 * [Jelentések olvasása engedélyezett Directory szerepkörök](#directory-roles-allowed-to-read-reports)
* [**Önkiszolgáló jelszókezelés típusú tevékenységek az új Azure-portálon**](#self-service-password-management-activity-types)
 * [Önkiszolgáló jelszóváltoztatás blokkolva](#activity-type-blocked-from-self-service-password-reset)
 * [Jelszó módosítása (önkiszolgáló)](#activity-type-change-password-self-service)
 * [Jelszó alaphelyzetbe állítása (amelyeket a rendszergazda)](#activity-type-reset-password-by-admin)
 * [Jelszó (önkiszolgáló)](#activity-type-reset-password-self-service)
 * [Szolgálnak az önkiszolgáló jelszó-átállítási folyamata tevékenység folyamatban](#activity-type-self-serve-password-reset-flow-activity-progress)
 * [Felhasználói fiók (önkiszolgáló) feloldása](#activity-type-unlock-user-account-self-service)
 * [Felhasználó által regisztrált az önkiszolgáló jelszóváltoztatáshoz](#activity-type-user-registered-for-self-service-password-reset)
* [**Hogyan lehet lekérni a jelszó felügyeleti események az Azure AD-jelentések és események API**](#how-to-retrieve-password-management-events-from-the-azure-ad-reports-and-events-api)
 * [Jelentéskészítési API adatok lekérését korlátozásai](#reporting-api-data-retrieval-limitations)
* [**Jelszó alaphelyzetbe állítása regisztrációs események gyorsan PowerShell letöltése**](#how-to-download-password-reset-registration-events-quickly-with-powershell)
* [**Jelszó jelentések megtekintése a klasszikus portál**](#how-to-view-password-management-reports-in-the-classic-portal)
* [**Nézet jelszó-visszaállítási regisztrációs tevékenység a szervezet a klasszikus portál**](#view-password-reset-registration-activity-in-the-classic-portal)
* [**Nézet jelszó-visszaállítási tevékenység a szervezet a klasszikus portál**](#view-password-reset-activity-in-the-classic-portal)


## <a name="overview-of-password-management-reports"></a>Jelszó jelentések áttekintése
Miután telepíti a jelszó alaphelyzetbe állítása, a leggyakrabban használt további lépéseket egyik azt használatáról a szervezet megjelenítéséhez.  Például előfordulhat, hogy kívánt felmérése a felhasználók hogyan regisztrálja a jelszó alaphelyzetbe állítása vagy hány jelszó alaphelyzetbe állítását kell végezni néhány napnál.  Íme néhány a gyakori kérdésekre, amely fogadja a hívást, amely szerepel jelszó felügyeleti jelentések lesz a [Azure felügyeleti portálon](https://manage.windowsazure.com) ma:

* Jelszó alaphelyzetbe állítása hányan regisztrált?
* Aki regisztrálva van a jelszó alaphelyzetbe állítása?
* Milyen adatokat próbál regisztrálni személyek?
* Hányan visszaállíthassák a jelszavukat, az elmúlt 7 napban?
* Mik a leggyakoribb módszerek felhasználók vagy rendszergazdák visszaállíthassák a jelszavukat használja?
* Mik azok a gyakori problémák felhasználók vagy rendszergazdák arcfelismerési a jelszó alaphelyzetbe állítása?
* Milyen a rendszergazdák gyakran alaphelyzetbe állítja saját jelszavukat?
* A gyanús tevékenységeket, a jelszó alaphelyzetbe állítása folyamatban van?

## <a name="how-to-view-password-management-reports"></a>Jelszó jelentések megtekintése
Az új [Azure Portal](https://portal.azure.com) élmény érdekében azt van megtekintéséhez a jelszó alaphelyzetbe állítása hatékonyabb módja, ezért a jelszó-regisztrálási tevékenységét.  Kövesse az alábbi lépéseket a jelszó-változtatási található, és a jelszó-változtatási regisztrációs események az új [Azure Portal](https://portal.azure.com):

1. Navigáljon a [ **portal.azure.com**](https://portal.azure.com)
2. Kattintson a **további szolgáltatások** a fő Azure portál bal oldali navigációs menü
3. Keresse meg **Azure Active Directory** a szolgáltatások listájában, és jelölje ki
4. Kattintson a **felhasználók és csoportok** az Azure Active Directory navigációs menü
5. Kattintson a **naplók** navigációs elem a felhasználók és csoportok navigációs menüjében. Ez megtudhatja, a naplózási események végrehajtását a felhasználókkal szemben mindegyikét a címtárban. Ez a nézet összes a jelszóval kapcsolatos eseményeket, is végezhet.
6. Ez a nézet csak a jelszó-felügyelettel kapcsolatos események szűréséhez kattintson a **szűrő** gomb a panel tetején.
7. Az a **szűrő** menüben válassza a **kategória** legördülő menüből, és módosítsa úgy, hogy a **önkiszolgáló jelszókezelés** kategória típusa.
8. Opcionálisan tovább szűkítheti a listában válassza ki az adott **tevékenység** érdekli
### <a name="direct-link-to-user-audit-blade"></a>A felhasználói naplózása panelen a közvetlen hivatkozás
Ha be van jelentkezve a portált, itt közvetlen kapcsolat van a felhasználó naplózási paneljét ahol láthatja ezeket az eseményeket:

* [Keresse fel a felhasználói felügyeleti naplózási nézet közvetlenül](https://portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/Audit)

### <a name="directory-roles-allowed-to-read-reports"></a>Jelentések olvasása engedélyezett Directory szerepkörök
Jelenleg a következő könyvtár szerepkörök lehet olvasni a klasszikus Azure portálra az Azure AD-jelszókezelés jelentések:

* Globális rendszergazda

Mielőtt olvassa el ezeket a jelentéseket, a vállalat globális rendszergazdája kell rendelkezik léptetett be ezeket az adatokat a jelentéskészítési vagy a vizsgálati naplók legalább egyszer felkeresésével beolvasni a szervezet nevében. Így, amíg nem lehet adatgyűjtés a szervezet számára.

További directory szerepkörökkel és mit tehet, kapcsolatban lásd: [rendszergazdai szerepkörök hozzárendelése az Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-assign-admin-roles).

## <a name="self-service-password-management-activity-types"></a>Önkiszolgáló jelszókezelés tevékenységtípusok
A következő típusú tevékenységek szerepelnek a **önkiszolgáló jelszókezelés** naplózási esemény kategóriát.  Minden egyes leírása az alábbi gombra.

* [**Önkiszolgáló jelszóváltoztatás blokkolva** ](#activity-type-blocked-from-self-service-password-reset) -azt jelzi, hogy egy felhasználó a jelszó alaphelyzetbe állítása, vagy egy adott kapu használni, egy telefonszám ellenőrzése 24 órában teljes 5-nél több alkalommal megpróbálta.
* [**Módosítsa jelszavát (önkiszolgáló)** ](#activity-type-change-password-self-service) -azt jelzi, a felhasználó elvégzett önkéntes, vagy (mert a lejárati) kényszerített a jelszó módosítása.
* [**Jelszó-átállítási (amelyeket a rendszergazda)** ](#activity-type-reset-password-by-admin) -azt jelzi, hogy a rendszergazda végre az Azure-portálról egy felhasználó nevében új jelszót.
* [**Jelszó-átállítási (önkiszolgáló)** ](#activity-type-reset-password-self-service) -azt jelzi, hogy a felhasználó alaphelyzetbe állítása sikeresen megtörtént a saját jelszavát a [Azure AD-jelszó-változtatási portál](https://passwordreset.microsoftonline.com).
* [**Szolgálnak az önkiszolgáló jelszó-átállítási folyamata tevékenység folyamatban** ](#activity-type-self-serve-password-reset-flow-activity-progress) -minden adott lépésre, a felhasználó végighalad (például a hitelesítési kapu átadja a jelszó alaphelyzetbe állítása) azt jelzi, mert része a jelszó alaphelyzetbe állítási eljárás.
* [**Felhasználói fiók (önkiszolgáló) feloldása** ](#activity-type-unlock-user-account-self-service) -azt jelzi, hogy a felhasználó zárolása sikeresen feloldva saját Active Directory-fiók jelszavát a alaphelyzetbe állításával a [Azure AD-jelszó-változtatási portál](https://passwordreset.microsoftonline.com) használatával a [AD-fiókok zárolásának feloldása nélkül alaphelyzetbe állítása](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-passwords-customize#allow-users-to-unlock-accounts-without-resetting-their-password) szolgáltatás.
* [**A felhasználó önkiszolgáló jelszó-visszaállításhoz regisztrálva** ](#activity-type-user-registered-for-self-service-password-reset) -jelzi a felhasználó regisztrálva van-e a szükséges adatokat állítható vissza a, vagy saját jelszó megfelel-e a bérlő jelenleg a megadott jelszó-visszaállítási házirend.

### <a name="activity-type-blocked-from-self-service-password-reset"></a>Tevékenységtípus: az önkiszolgáló jelszó-visszaállítás blokkolva
Az alábbi lista ismerteti ennek a tevékenységnek a részletei:

* **Tevékenység leírásában** – annak jelzése, egy rendszergazda felhasználó megpróbálta jelszó alaphelyzetbe állítása, vagy egy adott kapu használni, Telefonszám ellenőrzése a 24 órában teljes 5-nél több alkalommal.
* **Tevékenység Aktor** -lett halmozódni végrehajtása további felhasználó visszaállítási műveletről. Felhasználó vagy egy rendszergazda is lehet.
* **Tevékenység cél** -lett halmozódni végrehajtása további felhasználó visszaállítási műveletről. Felhasználó vagy egy rendszergazda is lehet.
* **Engedélyezett tevékenység állapotai**
 * _Sikeres_ -azt jelzi, hogy a felhasználó lett halmozódni hajt végre semmilyen további alaphelyzetbe állítását, próbálja meg semmilyen további hitelesítési módszerek, vagy minden további telefonszámok érvényesítése a következő 24 órában.
* **Tevékenység állapota a hiba oka** – nem alkalmazható

### <a name="activity-type-change-password-self-service"></a>Tevékenységtípus: jelszó módosítása (önkiszolgáló)
Az alábbi lista ismerteti ennek a tevékenységnek a részletei:

* **Tevékenység leírásában** – azt jelzi, a felhasználó elvégzett önkéntes, vagy (mert a lejárati) kényszerített a jelszó módosítása.
* **Tevékenység Aktor** -felhasználó módosította jelszavát. Felhasználó vagy egy rendszergazda is lehet.
* **Tevékenység cél** -felhasználó módosította jelszavát. Felhasználó vagy egy rendszergazda is lehet.
* **Engedélyezett tevékenység állapotai**
 * _Sikeres_ -azt jelzi, hogy a felhasználó sikeresen módosította jelszavát
 * _Hiba_ -azt jelzi, hogy a felhasználó nem sikerült módosítani a jelszavát. A sor kattintva lehetővé teszi a tekintse meg a **tevékenység állapotának oka** kategória további információt a hiba okát.
* **Tevékenység állapota a hiba oka** -
 * _FuzzyPolicyViolationInvalidPassword_ – a felhasználó olyan jelszót, amely automatikusan lett-e tiltani miatt a Microsoft tiltott jelszó észlelés találja túl köznapi vagy különösen gyenge választotta.

### <a name="activity-type-reset-password-by-admin"></a>Tevékenységtípus: jelszó-átállítási (amelyeket a rendszergazda)
Az alábbi lista ismerteti ennek a tevékenységnek a részletei:

* **Tevékenység leírásában** – azt jelzi, hogy a rendszergazda végre az Azure-portálról egy felhasználó nevében új jelszót.
* **Tevékenység Aktor** -végző a jelszó-változtatási nevében végfelhasználói vagy a rendszergazda egy másik rendszergazda. Kell lennie, vagy egy globális rendszergazda, jelszókezelő, felhasználó, vagy segélyszolgálat rendszergazdája.
* **Tevékenység cél** – a felhasználó, akinek a jelszó alaphelyzetbe állítása. Felhasználó vagy egy másik, rendszergazdai lehet.
* **Engedélyezett tevékenység állapotai**
 * _Sikeres_ -azt jelzi, hogy egy rendszergazda alaphelyzetbe állítása sikeresen megtörtént a jelszó
 * _Hiba_ -azt jelzi, hogy egy rendszergazda a jelszó módosítása sikertelen volt. A sor kattintva lehetővé teszi a tekintse meg a **tevékenység állapotának oka** kategória további információt a hiba okát.

### <a name="activity-type-reset-password-self-service"></a>Tevékenységtípus: jelszó-átállítási (önkiszolgáló)
Az alábbi lista ismerteti ennek a tevékenységnek a részletei:

* **Tevékenység leírásában** – annak jelzése, a felhasználó alaphelyzetbe állítása sikeresen megtörtént a saját jelszavát a [Azure AD-jelszó-változtatási portál](https://passwordreset.microsoftonline.com).
* **Tevékenység Aktor** – a felhasználóknak, akik a saját jelszavát. Felhasználó vagy egy rendszergazda is lehet.
* **Tevékenység cél** – a felhasználóknak, akik a saját jelszavát. Felhasználó vagy egy rendszergazda is lehet.
* **Engedélyezett tevékenység állapotai**
 * _Sikeres_ -azt jelzi, hogy egy felhasználó a saját jelszó sikeresen visszaállítva
 * _Hiba_ -azt jelzi, hogy egy felhasználó a saját jelszó visszaállítása sikertelen. A sor kattintva lehetővé teszi a tekintse meg a **tevékenység állapotának oka** kategória további információt a hiba okát.
* **Tevékenység állapota a hiba oka** -
 * _FuzzyPolicyViolationInvalidPassword_ – a rendszergazda olyan jelszót, amely automatikusan lett-e tiltani miatt a Microsoft tiltott jelszó észlelés találja túl köznapi vagy különösen gyenge kiválasztva.

### <a name="activity-type-self-serve-password-reset-flow-activity-progress"></a>Tevékenységtípus: önkiszolgáló szolgálnak a jelszó alaphelyzetbe állítása folyamata tevékenység folyamatban
Az alábbi lista ismerteti ennek a tevékenységnek a részletei:

* **Tevékenység leírásában** – azt jelzi, a felhasználó végighalad (például a hitelesítési kapu átadja a jelszó alaphelyzetbe állítása), mert része a jelszó alaphelyzetbe állítási eljárás adott lépésre.
* **Tevékenység Aktor** -jelszó részét végző felhasználó visszaállítása folyamata. Felhasználó vagy egy rendszergazda is lehet.
* **Tevékenység cél** -jelszó részét végző felhasználó visszaállítása folyamata. Felhasználó vagy egy rendszergazda is lehet.
* **Engedélyezett tevékenység állapotai**
 * _Sikeres_ -azt jelzi, hogy a felhasználó sikeresen befejezte a jelszóvisszaállítási folyamatot az adott lépésre.
 * _Hiba_ -azt jelzi, hogy egy adott lépésre a jelszó alaphelyzetbe állítása nem sikerült folyamat. A sor kattintva lehetővé teszi a tekintse meg a **tevékenység állapotának oka** kategória további információt a hiba okát.
* **Engedélyezett tevékenység állapotának oka**
 * Lásd az alábbi táblázatban [minden engedélyezett visszaállítási tevékenység állapotának oka](#allowed-values-for-details-column)

### <a name="activity-type-unlock-user-account-self-service"></a>Tevékenységtípus: felhasználói fiók (önkiszolgáló) feloldása
Az alábbi lista ismerteti ennek a tevékenységnek a részletei:

* **Tevékenység leírásában** – annak jelzése, a felhasználó zárolása sikeresen feloldva saját Active Directory-fiók jelszavát a alaphelyzetbe állításával a [Azure AD-jelszó-változtatási portál](https://passwordreset.microsoftonline.com) használatával a [AD-fiókot visszaállítás nélkül is feloldhatják](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-passwords-customize#allow-users-to-unlock-accounts-without-resetting-their-password) szolgáltatás.
* **Tevékenység Aktor** – a felhasználóknak, akik a saját fiókja zárolását nélkül a jelszó alaphelyzetbe állításával. Felhasználó vagy egy rendszergazda is lehet.
* **Tevékenység cél** – a felhasználóknak, akik a saját fiókja zárolását nélkül a jelszó alaphelyzetbe állításával. Felhasználó vagy egy rendszergazda is lehet.
* **Engedélyezett tevékenység állapotai**
 * _Sikeres_ -azt jelzi, hogy egy felhasználó a saját fiók nyitása sikeresen megtörtént
 * _Hiba_ -azt jelzi, hogy egy felhasználó saját fiók zárolásának feloldása nem sikerült. A sor kattintva lehetővé teszi a tekintse meg a **tevékenység állapotának oka** kategória további információt a hiba okát.

### <a name="activity-type-user-registered-for-self-service-password-reset"></a>Tevékenységtípus: felhasználó regisztrált az önkiszolgáló jelszóváltoztatáshoz
Az alábbi lista ismerteti ennek a tevékenységnek a részletei:

* **Tevékenység leírásában** – annak jelzése a felhasználó regisztrálva van-e a szükséges adatokat állítható vissza a, vagy saját jelszó megfelel-e a bérlő jelenleg a megadott jelszó-visszaállítási házirend.
* **Tevékenység Aktor** -jelszó alaphelyzetbe állítása regisztrált felhasználó. Felhasználó vagy egy rendszergazda is lehet.
* **Tevékenység cél** -jelszó alaphelyzetbe állítása regisztrált felhasználó. Felhasználó vagy egy rendszergazda is lehet.
* **Engedélyezett tevékenység állapotai**
 * _Sikeres_ -egy felhasználó sikerült regisztrálni a jelszó alaphelyzetbe állítása, az aktuális nyilatkozatnak jelzi.
 * _Hiba_ -azt jelzi, hogy a felhasználó nem tudta regisztrálni a jelszó-visszaállításhoz. A sor kattintva lehetővé teszi a tekintse meg a **tevékenység állapotának oka** kategória további információt a hiba okát. Megjegyzés: Ez nem jelenti azt a felhasználó nem tudja alaphelyzetbe állítani a, vagy a saját jelszavát, csak adott általa nem fejeződött be a regisztrációs folyamat során. Ha nem ellenőrzött adatok a fiókra, amely a megfelelő (például egy telefonszámot, amelyet a rendszer nem érvényesíti), annak ellenére, hogy nem ellenőrizte, hogy ez a telefonszám, továbbra is használhatják a jelszó alaphelyzetbe állításához. További információkért lásd: [mi történik, ha a felhasználó regisztrál?](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-learn-more#what-happens-when-a-user-registers)

## <a name="how-to-retrieve-password-management-events-from-the-azure-ad-reports-and-events-api"></a>Hogyan lehet lekérni a jelszó felügyeleti események az Azure AD-jelentések és események API
2015. augusztus frissítésétől az Azure AD-jelentések és események API mostantól támogatja a jelszó alaphelyzetbe állítása és a jelszó alaphelyzetbe állítása szóregisztrációs jelentéseket szereplő adatok beolvasása. Ez az API használatával letöltheti az egyes jelszó alaphelyzetbe állítása és jelszó-változtatási regisztrációs események a choce a jelentéskészítési technológiával integrációja.

### <a name="how-to-get-started-with-the-reporting-api"></a>Első lépések a reporting API-hoz
Ezek az adatok elérésére lesz szüksége beírni egy kis alkalmazás vagy a parancsfájl azt lekérése a kiszolgálóról. [Útmutató az Azure AD Reporting API használatába](active-directory-reporting-api-getting-started.md).

Ha már van egy működő parancsfájl, ezután érdemes vizsgálja meg az esetek teljesítéséhez kérheti le a jelszó alaphelyzetbe állítása és nyilvántartási események.

* [SsprActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent): az elérhető oszlopok listája, a jelszó-átállítási események
* [SsprRegistrationActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprRegistrationActivityEvent): az elérhető oszlopok listája, a jelszó-átállítási regisztráció események

### <a name="reporting-api-data-retrieval-limitations"></a>Jelentéskészítési API adatok lekérését korlátozásai
Jelenleg az Azure AD-jelentések és események API lekérdezi legfeljebb **75,000 események** , a [SsprActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent) és [SsprRegistrationActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprRegistrationActivityEvent) típusok átfedés a **utolsó 30 nap**.

Ha szeretné beolvasni, vagy ez az ablak adatok tárolására, javasoljuk, hogy külső adatbázis megőrzése, és a eltéréseit – eredményező lekérdezni az API-val. Ajánlott ezeket az adatokat, amikor hozzákezd a jelszó alaphelyzetbe állítása a regisztrációt a szervezet, megőrizni a külsőleg és majd továbbra is nyomon követheti az eltérések ettől kezdve a kezdéshez.

## <a name="how-to-download-password-reset-registration-events-quickly-with-powershell"></a>Jelszó alaphelyzetbe állítása regisztrációs események gyorsan PowerShell letöltése
Az Azure AD-jelentések és események API közvetlenül használata mellett is használhatja az alábbi PowerShell-parancsfájl használatával legutóbbi regisztrációs események a címtárban. Ez akkor hasznos, ha meg szeretné tekinteni, akik nemrég regisztrálva van, vagy győződjön meg arról, hogy a várt módon jelentkezett-e a jelszó alaphelyzetbe állítása bevezetést szeretné.

* [Az Azure AD SSPR regisztrációs tevékenység PowerShell-parancsfájl](https://gallery.technet.microsoft.com/scriptcenter/azure-ad-self-service-e31b8aee)

## <a name="how-to-view-password-management-reports-in-the-classic-portal"></a>Jelszó jelentések megtekintése a klasszikus portál
A jelszó jelentések megkereséséhez kövesse az alábbi lépéseket:

1. Kattintson a **Active Directory** a bővítmény a [a klasszikus Azure portálon](https://manage.windowsazure.com).
2. Válassza ki a címtárát a listából, amely megjelenik a portálon.
3. Kattintson a **jelentések** fülre.
4. Tekintse át a **tevékenységi naplóit** szakasz.
5. Válassza a **jelszó-visszaállítási tevékenység** jelentés vagy a **jelszó-visszaállítási regisztrációs tevékenység** jelentés.

## <a name="view-password-reset-registration-activity-in-the-classic-portal"></a>Nézet jelszó-visszaállítási regisztrációs tevékenység a klasszikus portál
A jelszó-visszaállítási regisztrációs Tevékenységjelentés látható, minden jelszó-átállítási regisztrációk a szervezet történt.  A jelszóátállítás regisztrációját megjelenik ez a jelentés bármely felhasználó, aki sikeresen regisztrálta hitelesítési adatokat, a jelszó-visszaállítási regisztrációs portál ([http://aka.ms/ssprsetup](http://aka.ms/ssprsetup)).

* **Maximális időtartomány**: 30 napban
* **A sorok maximális számát**: 75 000
* **Letölthető**: Igen, a CSV-fájl használatával

### <a name="description-of-report-columns"></a>A jelentés oszlopok leírása
Az alábbi lista ismerteti részletesen jelentés oszlopainak mindegyike:

* **Felhasználói** – kísérletet a jelszó alaphelyzetbe állítása regisztrációs műveletet.
* **Szerepkör** – a szerepkört a felhasználó a címtárban.
* **Dátum és idő** – a dátum és idő a kísérlet.
* **Adatok regisztrált** – milyen hitelesítési adatokat, a felhasználó által megadott során jelszó-átállítási regisztrációk.

### <a name="description-of-report-values"></a>A jelentés értékek leírása
A következő táblázat ismerteti a különböző minden egyes oszlophoz engedélyezett értékek:

| Oszlop | Megengedett értékek és azok jelentését |
| --- | --- |
| Regisztrált adatok |**Másodlagos e-mail-címet** – a felhasználó másodlagos e-mail vagy hitelesítési e-mail használt hitelesítéséhez<p><p>**Irodai telefon**– hitelesítéséhez használt felhasználói irodai telefon<p>**Mobiltelefon** -használt felhasználói mobiltelefonszám vagy a hitelesítéshez megadott telefonját hitelesítéséhez<p>**Biztonsági kérdések** – felhasználó hitelesítéséhez a biztonsági kérdések használt<p>**A fenti bármilyen kombinációját (pl. másodlagos E-mail + mobiltelefon)** – akkor fordul elő, amikor 2 kapu házirend van megadva, és melyik két módszert használja a felhasználó mutatja a hitelesítéshez a jelszó-változtatási kérelem. |

## <a name="view-password-reset-activity-in-the-classic-portal"></a>Nézet jelszó-visszaállítási tevékenység a klasszikus portál
Ez a jelentés tartalmazza az összes jelszó alaphelyzetbe állítása, amely a szervezet történt kísérlet.

* **Maximális időtartomány**: 30 napban
* **A sorok maximális számát**: 75 000
* **Letölthető**: Igen, a CSV-fájl használatával

### <a name="description-of-report-columns"></a>A jelentés oszlopok leírása
Az alábbi lista ismerteti részletesen jelentés oszlopainak mindegyike:

1. **Felhasználói** – kísérletet a jelszó alaphelyzetbe állítása (a felhasználói azonosító mező megadni, ha a felhasználó rendelkezik a jelszó alaphelyzetbe állítása alapján) műveletet.
2. **Szerepkör** – a szerepkört a felhasználó a címtárban.
3. **Dátum és idő** – a dátum és idő a kísérlet.
4. **Metódussal használt** – ez használt a felhasználói hitelesítési módszerek alaphelyzetbe állítja a műveletet.
5. **Eredmény** – a végeredménynek az a jelszó alaphelyzetbe állítása a műveletet.
6. **Részletek** – miért a jelszó-átállítási részleteit az értéket, ugyanúgy eredményezett.  Is tartalmaz a megoldás lépéseket hiba megoldásához is igénybe vehet.

### <a name="description-of-report-values"></a>A jelentés értékek leírása
A következő táblázat ismerteti a különböző minden egyes oszlophoz engedélyezett értékek:

| Oszlop | Megengedett értékek és azok jelentését |
| --- | --- |
| Használt módszerek |**Másodlagos e-mail-címet** – a felhasználó másodlagos e-mail vagy hitelesítési e-mail használt hitelesítéséhez<p>**Irodai telefon** – hitelesítéséhez használt felhasználói irodai telefon<p>**Mobiltelefon** – használt felhasználói mobiltelefonszám vagy a hitelesítéshez megadott telefonját hitelesítéséhez<p>**Biztonsági kérdések** – felhasználó hitelesítéséhez a biztonsági kérdések használt<p>**A fenti bármilyen kombinációját (pl. másodlagos E-mail + mobiltelefon)** – akkor fordul elő, amikor 2 kapu házirend van megadva, és melyik két módszert használja a felhasználó mutatja a hitelesítéshez a jelszó-változtatási kérelem. |
| eredménye |**Elhagyott** – felhasználói jelszó-visszaállítás elindult, de le félúton helyezkedjenek el keresztül nélkül befejezése<p>**Blokkolt** – felhasználói fiók segítségével, hogy a jelszó-visszaállítás miatt kísérlet a jelszó alaphelyzetbe állítása a lapon miatt sikertelen volt, vagy egyetlen jelszó kapu visszaállítása egy 24 órás időtartamon belül túl sokszor<p>**Megszakítva** – felhasználói jelszó-visszaállítás elindult, de majd kattintott a Mégse gombra a részben módon a munkamenet megszakítása <p>**Rendszergazda-e kommunikálni** – felhasználói probléma volt a munkamenetben, hogy nem oldható fel, így a felhasználó helyett befejezési "Forduljon a rendszergazdához" hivatkozásra kattint a jelszó-átállítási folyamata<p>**Nem sikerült** – felhasználó nem tudta állítani a jelszót, valószínűleg, mert a felhasználó nem lett konfigurálva ahhoz, hogy a szolgáltatás (pl. nincs engedélye, hiányzik a hitelesítési adatokat, felügyelt jelszó a helyszínen, de visszaírási le van tiltva).<p>**Sikeres** – jelszó-visszaállítás sikeres volt. |
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
| A felhasználó megszakította a szükséges hitelesítési módszerek előtt |Megszakítva |
| A felhasználó megszakította az új jelszó elküldése előtt |Megszakítva |
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

## <a name="next-steps"></a>Következő lépések
Az alábbiakban láthatja az összes Azure AD-jelszóvisszaállítási dokumentációs oldal hivatkozását:

* **Azért van itt, mert problémák merültek fel a bejelentkezéssel kapcsolatban?** Ha igen, [így módosíthatja vagy állíthatja alaphelyzetbe a jelszavát](active-directory-passwords-update-your-own-password.md#reset-or-unlock-my-password-for-a-work-or-school-account).
* [**Működés**](active-directory-passwords-how-it-works.md) – megismerheti a szolgáltatás hat különböző összetevőjét és azt, hogy ezek mire valók
* [**Első lépések** ](active-directory-passwords-getting-started.md) -megtudhatja, hogyan teszik lehetővé a felhasználók alaphelyzetbe állítása és a felhőbeli vagy helyszíni jelszavukat
* [**Testreszabás**](active-directory-passwords-customize.md) – megtudhatja, hogyan szabhatja testre a szervezet által igényelt szolgáltatás kezelőfelületét és működését
* [**Ajánlott eljárások**](active-directory-passwords-best-practices.md) – megtudhatja, hogyan helyezhet gyorsan üzembe és hogyan kezelhet hatékonyan jelszavakat a szervezetben
* [**Gyakori kérdések**](active-directory-passwords-faq.md) – választ kaphat a gyakori kérdésekre
* [**Hibaelhárítás**](active-directory-passwords-troubleshoot.md) – megtudhatja, hogyan háríthatja el a szolgáltatással kapcsolatos problémákat
* [**További információ**](active-directory-passwords-learn-more.md) – elmerülhet a szolgáltatás működésének műszaki részleteiben
