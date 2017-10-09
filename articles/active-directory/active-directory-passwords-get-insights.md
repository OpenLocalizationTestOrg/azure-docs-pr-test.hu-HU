---
title: "Get Insights: Az Azure ad jelentést |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toouse jelentéseket a szervezetében jelszó felügyeleti műveletek tooget betekintést."
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
ms.openlocfilehash: 90e0b8e621cdfe3e3a2f15df7b98115008855500
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooget-operational-insights-with-password-management-reports"></a>Hogyan tooget az operational insights jelszókezelés jelentések
> [!IMPORTANT]
> **Azért van itt, mert problémák merültek fel a bejelentkezéssel kapcsolatban?** Ha igen, [így módosíthatja vagy állíthatja alaphelyzetbe a jelszavát](active-directory-passwords-update-your-own-password.md#reset-or-unlock-my-password-for-a-work-or-school-account).
>
>

Ez a szakasz ismerteti, hogyan használhatja az Azure Active Directory jelszókezelés jelentések tooview, hogy a felhasználók miként használnak a jelszó alaphelyzetbe állítása és módosítása a szervezetében.

* [**Jelszó felügyeleti jelentések áttekintése**](#overview-of-password-management-reports)
* [**Hogyan tooview jelszókezelés jelentések hello új Azure-portálon**](#how-to-view-password-management-reports)
 * [Directory szerepkörök engedélyezett tooread jelentések](#directory-roles-allowed-to-read-reports)
* [**Önkiszolgáló jelszókezelés típusú tevékenységek a hello új Azure-portálon**](#self-service-password-management-activity-types)
 * [Önkiszolgáló jelszóváltoztatás blokkolva](#activity-type-blocked-from-self-service-password-reset)
 * [Jelszó módosítása (önkiszolgáló)](#activity-type-change-password-self-service)
 * [Jelszó alaphelyzetbe állítása (amelyeket a rendszergazda)](#activity-type-reset-password-by-admin)
 * [Jelszó (önkiszolgáló)](#activity-type-reset-password-self-service)
 * [Szolgálnak az önkiszolgáló jelszó-átállítási folyamata tevékenység folyamatban](#activity-type-self-serve-password-reset-flow-activity-progress)
 * [Felhasználói fiók (önkiszolgáló) feloldása](#activity-type-unlock-user-account-self-service)
 * [Felhasználó által regisztrált az önkiszolgáló jelszóváltoztatáshoz](#activity-type-user-registered-for-self-service-password-reset)
* [**Hogyan tooretrieve jelszó felügyeleti események hello Azure AD-jelentéseket és események API**](#how-to-retrieve-password-management-events-from-the-azure-ad-reports-and-events-api)
 * [Jelentéskészítési API adatok lekérését korlátozásai](#reporting-api-data-retrieval-limitations)
* [**Hogyan toodownload jelszó-változtatási regisztrációs események gyorsan a PowerShell használatával**](#how-to-download-password-reset-registration-events-quickly-with-powershell)
* [**Hogyan tooview jelszókezelés jelentései hello klasszikus portál**](#how-to-view-password-management-reports-in-the-classic-portal)
* [**Nézet jelszó-visszaállítási regisztrációs tevékenység a szervezet hello klasszikus portál**](#view-password-reset-registration-activity-in-the-classic-portal)
* [**Nézet jelszó-visszaállítási tevékenység a szervezet hello klasszikus portál**](#view-password-reset-activity-in-the-classic-portal)


## <a name="overview-of-password-management-reports"></a>Jelszó jelentések áttekintése
Miután telepíti a jelszó alaphelyzetbe állítása, hello leggyakoribb lépések egyik toosee azt használatáról a szervezetében.  Például az tooget insight érdemes lehet a felhasználók hogyan regisztrálja a jelszó alaphelyzetbe állítása vagy hány jelszó alaphelyzetbe állítását kell végezni a hello utolsó néhány nap múlva.  Az alábbiakban néhány hello gyakori kérdésekre, amely akkor lesz képes tooanswer hello jelszó felügyeleti jelentések hello meglévő [Azure felügyeleti portálon](https://manage.windowsazure.com) ma:

* Jelszó alaphelyzetbe állítása hányan regisztrált?
* Aki regisztrálva van a jelszó alaphelyzetbe állítása?
* Milyen adatokat próbál regisztrálni személyek?
* Hányan legutóbbi 7 nap hello jelszavak alaphelyzetbe állítása?
* Mik azok a hello leggyakrabban használt módszerek felhasználók vagy rendszergazdák használja tooreset jelszavukat?
* Mik a közös állít felhasználók vagy rendszergazdák arc, toouse jelszó alaphelyzetbe állítása közben?
* Milyen a rendszergazdák gyakran alaphelyzetbe állítja saját jelszavukat?
* A gyanús tevékenységeket, a jelszó alaphelyzetbe állítása folyamatban van?

## <a name="how-tooview-password-management-reports"></a>Hogyan tooview jelszókezelés jelentések
Az új hello [Azure Portal](https://portal.azure.com) észlel, jobb módon tooview jelszó alaphelyzetbe állítása és a jelszó alaphelyzetbe állítása regisztrációs tevékenységgel kell.  Hajtsa végre hello lépéseket alatti toofind hello jelszó alaphelyzetbe állítása és a jelszó alaphelyzetbe állítása hello új regisztrációs események [Azure Portal](https://portal.azure.com):

1. Keresse meg a túl[**portal.azure.com**](https://portal.azure.com)
2. Kattintson a hello **további szolgáltatások** hello fő Azure portál bal oldali navigációs menü
3. Keresse meg **Azure Active Directory** a hello szolgáltatások listájában, és jelölje ki
4. Kattintson a **felhasználók és csoportok** hello Azure Active Directory navigációs menü
5. Kattintson a hello **naplók** navigációs elem hello felhasználók és csoportok navigációs menüjében. Ez megtudhatja, az összes hello naplózási események előforduló összes hello felhasználókkal szemben a címtárban. A nézet toosee összes hello jelszó-események, is végezhet.
6. a nézet tooonly hello jelszó felügyelettel kapcsolatos toofilter események, kattintson a hello **szűrő** hello panel felső hello gombra.
7. A hello **szűrő** menü, jelölje be hello **kategória** legördülő menüből, és módosítsa úgy toohello **önkiszolgáló jelszókezelés** kategória típusa.
8. Nem kötelező további hello szűrőlista hello specifikus kiválasztásával **tevékenység** érdekli
### <a name="direct-link-toouser-audit-blade"></a>A közvetlen hivatkozás tooUser naplózási panel
Ha be van jelentkezve tooyour portal, ez egy közvetlen hivatkozást toohello felhasználói naplózási panel ahol láthatja ezeket az eseményeket:

* [Nyissa meg toouser felügyeleti közvetlenül naplózási nézet](https://portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/Audit)

### <a name="directory-roles-allowed-tooread-reports"></a>Directory szerepkörök engedélyezett tooread jelentések
Jelenleg hello következő directory szerepkörök lehet olvasni a klasszikus Azure portálon hello Azure AD-jelszókezelés jelentések:

* Globális rendszergazda

Képes tooread ezekre a jelentésekre előtt, hello vállalat globális rendszergazdája kell rendelkezik léptetett be az e lapon vagy a napló naplók legalább egyszer reporting hello felkeresésével hello szervezeti beolvasott adatok toobe. Így, amíg nem lehet adatgyűjtés a szervezet számára.

többet directory szerepkörök és azok is, lásd: tooread [rendszergazdai szerepkörök hozzárendelése az Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-assign-admin-roles).

## <a name="self-service-password-management-activity-types"></a>Önkiszolgáló jelszókezelés tevékenységtípusok
a következő típusú tevékenységek hello jelennek meg hello **önkiszolgáló jelszókezelés** naplózási esemény kategória.  Minden egyes leírása az alábbi gombra.

* [**Önkiszolgáló jelszóváltoztatás blokkolva** ](#activity-type-blocked-from-self-service-password-reset) -azt jelzi, hogy egy felhasználó próbált tooreset jelszót, egy adott kapu használja, vagy egy telefonszám ellenőrzése 24 órában teljes 5-nél több alkalommal.
* [**Módosítsa jelszavát (önkiszolgáló)** ](#activity-type-change-password-self-service) -azt jelzi, a felhasználó önkéntes végre, vagy a kényszerített (esedékes tooexpiry) a jelszó módosítása.
* [**Jelszó-átállítási (amelyeket a rendszergazda)** ](#activity-type-reset-password-by-admin) -azt jelzi, hogy a rendszergazda végre a hello Azure portál egy felhasználó nevében új jelszót.
* [**Jelszó-átállítási (önkiszolgáló)** ](#activity-type-reset-password-self-service) -azt jelzi, hogy a felhasználó sikeresen visszaállítva hello jelszó visszaállításának [Azure AD-jelszó-változtatási portál](https://passwordreset.microsoftonline.com).
* [**Szolgálnak az önkiszolgáló jelszó-átállítási folyamata tevékenység folyamatban** ](#activity-type-self-serve-password-reset-flow-activity-progress) -minden felhasználó végighalad (például a hitelesítési kapu átadja a jelszó alaphelyzetbe állítása) meghatározott lépés azt jelzi, része hello jelszó alaphelyzetbe állítási eljárás szerint.
* [**Felhasználói fiók (önkiszolgáló) feloldása** ](#activity-type-unlock-user-account-self-service) -azt jelzi, hogy a felhasználó sikeresen feloldotta saját Active Directory-fiók jelszavát a hello alaphelyzetbe állításával [Azure AD-jelszó-változtatási portál](https://passwordreset.microsoftonline.com) hello segítségével [AD-fiókok zárolásának feloldása nélkül alaphelyzetbe állítása](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-passwords-customize#allow-users-to-unlock-accounts-without-resetting-their-password) szolgáltatás.
* [**A felhasználó önkiszolgáló jelszó-visszaállításhoz regisztrálva** ](#activity-type-user-registered-for-self-service-password-reset) -azt jelzi, hogy a felhasználó regisztrálva van-e minden szükséges hello információk toobe képes tooreset jelszavát hello bérlő jelenleg a megadott jelszó-visszaállítási házirend megfelelően.

### <a name="activity-type-blocked-from-self-service-password-reset"></a>Tevékenységtípus: az önkiszolgáló jelszó-visszaállítás blokkolva
hello a következő lista ismerteti ennek a tevékenységnek a részletei:

* **Tevékenység leírásában** – jelzi a felhasználó próbált tooreset jelszót, vagy egy adott kapu használni, egy telefonszám ellenőrzése 24 órában teljes 5-nél több alkalommal.
* **Tevékenység Aktor** -végrehajtása további lett halmozódni hello felhasználó visszaállítási műveletről. Felhasználó vagy egy rendszergazda is lehet.
* **Tevékenység cél** -végrehajtása további lett halmozódni hello felhasználó visszaállítási műveletről. Felhasználó vagy egy rendszergazda is lehet.
* **Engedélyezett tevékenység állapotai**
 * _Sikeres_ -azt jelzi, hogy a felhasználó lett halmozódni hajt végre semmilyen további alaphelyzetbe állítását, próbálja meg semmilyen további hitelesítési módszerek, vagy ellenőrizze minden további telefonszámainak hello 24 órán át.
* **Tevékenység állapota a hiba oka** – nem alkalmazható

### <a name="activity-type-change-password-self-service"></a>Tevékenységtípus: jelszó módosítása (önkiszolgáló)
hello a következő lista ismerteti ennek a tevékenységnek a részletei:

* **Tevékenység leírásában** – azt jelzi, a felhasználó önkéntes végre, vagy a kényszerített (esedékes tooexpiry) a jelszó módosítása.
* **Tevékenység Aktor** -hello felhasználó módosította jelszavát. Felhasználó vagy egy rendszergazda is lehet.
* **Tevékenység cél** -hello felhasználó módosította jelszavát. Felhasználó vagy egy rendszergazda is lehet.
* **Engedélyezett tevékenység állapotai**
 * _Sikeres_ -azt jelzi, hogy a felhasználó sikeresen módosította jelszavát
 * _Hiba_ -azt jelzi, hogy a felhasználó nem tudta toochange jelszavát. Kattintson a hello sor lehetővé teszi a toosee hello **tevékenység állapotának oka** kategória toolearn többet miért hello hiba történt.
* **Tevékenység állapota a hiba oka** -
 * _FuzzyPolicyViolationInvalidPassword_ -hello felhasználó kiválasztott olyan jelszót, amely automatikusan lett-e tiltani tooMicrosoft által tiltott jelszó észlelés találja toobe túl köznapi vagy különösen gyenge miatt.

### <a name="activity-type-reset-password-by-admin"></a>Tevékenységtípus: jelszó-átállítási (amelyeket a rendszergazda)
hello a következő lista ismerteti ennek a tevékenységnek a részletei:

* **Tevékenység leírásában** – azt jelzi, hogy a rendszergazda végre a hello Azure portál egy felhasználó nevében új jelszót.
* **Tevékenység Aktor** -hello jelszó alaphelyzetbe állítása, egy másik végfelhasználói vagy rendszergazda nevében végző hello rendszergazda. Kell lennie, vagy egy globális rendszergazda, jelszókezelő, felhasználó, vagy segélyszolgálat rendszergazdája.
* **Tevékenység cél** -hello felhasználó, amelynek a jelszó alaphelyzetbe állítása. Felhasználó vagy egy másik, rendszergazdai lehet.
* **Engedélyezett tevékenység állapotai**
 * _Sikeres_ -azt jelzi, hogy egy rendszergazda alaphelyzetbe állítása sikeresen megtörtént a jelszó
 * _Hiba_ -azt jelzi, hogy egy rendszergazda nem tudta toochange egy felhasználó jelszavát. Kattintson a hello sor lehetővé teszi a toosee hello **tevékenység állapotának oka** kategória toolearn többet miért hello hiba történt.

### <a name="activity-type-reset-password-self-service"></a>Tevékenységtípus: jelszó-átállítási (önkiszolgáló)
hello a következő lista ismerteti ennek a tevékenységnek a részletei:

* **Tevékenység leírásában** – azt jelzi, hogy a felhasználó sikeresen visszaállítva hello jelszó visszaállításának [Azure AD-jelszó-változtatási portál](https://passwordreset.microsoftonline.com).
* **Tevékenység Aktor** -hello felhasználó, aki a saját jelszavát. Felhasználó vagy egy rendszergazda is lehet.
* **Tevékenység cél** -hello felhasználó, aki a saját jelszavát. Felhasználó vagy egy rendszergazda is lehet.
* **Engedélyezett tevékenység állapotai**
 * _Sikeres_ -azt jelzi, hogy egy felhasználó a saját jelszó sikeresen visszaállítva
 * _Hiba_ -azt jelzi, hogy egy felhasználó nem tudta tooreset saját jelszó. Kattintson a hello sor lehetővé teszi a toosee hello **tevékenység állapotának oka** kategória toolearn többet miért hello hiba történt.
* **Tevékenység állapota a hiba oka** -
 * _FuzzyPolicyViolationInvalidPassword_ -Üdvözöljük a rendszergazdákat kijelölt olyan jelszót, amely automatikusan lett-e tiltani tooMicrosoft által tiltott jelszó észlelés találja toobe túl köznapi vagy különösen gyenge miatt.

### <a name="activity-type-self-serve-password-reset-flow-activity-progress"></a>Tevékenységtípus: önkiszolgáló szolgálnak a jelszó alaphelyzetbe állítása folyamata tevékenység folyamatban
hello a következő lista ismerteti ennek a tevékenységnek a részletei:

* **Tevékenység leírásában** – azt jelzi, a felhasználó végighalad (például a hitelesítési kapu átadja a jelszó alaphelyzetbe állítása), része hello jelszó alaphelyzetbe állítási eljárás adott lépésre.
* **Tevékenység Aktor** -hello jelszó részét hello felhasználó visszaállítása folyamata. Felhasználó vagy egy rendszergazda is lehet.
* **Tevékenység cél** -hello jelszó részét hello felhasználó visszaállítása folyamata. Felhasználó vagy egy rendszergazda is lehet.
* **Engedélyezett tevékenység állapotai**
 * _Sikeres_ -azt jelzi, hogy a felhasználó sikeresen befejezte egy adott lépésre a hello jelszóvisszaállítási folyamatot.
 * _Hiba_ -azt jelzi, hogy egy adott lépésre hello jelszó alaphelyzetbe állítása nem sikerült folyamat. Kattintson a hello sor lehetővé teszi a toosee hello **tevékenység állapotának oka** kategória toolearn többet miért hello hiba történt.
* **Engedélyezett tevékenység állapotának oka**
 * Lásd az alábbi táblázatban [minden engedélyezett visszaállítási tevékenység állapotának oka](#allowed-values-for-details-column)

### <a name="activity-type-unlock-user-account-self-service"></a>Tevékenységtípus: felhasználói fiók (önkiszolgáló) feloldása
hello a következő lista ismerteti ennek a tevékenységnek a részletei:

* **Tevékenység leírásában** – azt jelzi, hogy a felhasználó sikeresen feloldotta saját Active Directory-fiók jelszavát a hello alaphelyzetbe állításával [Azure AD-jelszó-változtatási portál](https://passwordreset.microsoftonline.com) hello segítségével [AD fiók zárolásának feloldása nélkül alaphelyzetbe állítása](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-passwords-customize#allow-users-to-unlock-accounts-without-resetting-their-password) szolgáltatás.
* **Tevékenység Aktor** -hello felhasználó, aki saját fiók nyitása nélkül a jelszó alaphelyzetbe állításával. Felhasználó vagy egy rendszergazda is lehet.
* **Tevékenység cél** -hello felhasználó, aki saját fiók nyitása nélkül a jelszó alaphelyzetbe állításával. Felhasználó vagy egy rendszergazda is lehet.
* **Engedélyezett tevékenység állapotai**
 * _Sikeres_ -azt jelzi, hogy egy felhasználó a saját fiók nyitása sikeresen megtörtént
 * _Hiba_ -azt jelzi, hogy egy felhasználó nem tudta toounlock fiókját. Kattintson a hello sor lehetővé teszi a toosee hello **tevékenység állapotának oka** kategória toolearn többet miért hello hiba történt.

### <a name="activity-type-user-registered-for-self-service-password-reset"></a>Tevékenységtípus: felhasználó regisztrált az önkiszolgáló jelszóváltoztatáshoz
hello a következő lista ismerteti ennek a tevékenységnek a részletei:

* **Tevékenység leírásában** – azt jelzi, hogy a felhasználó regisztrálva van-e minden szükséges hello információk toobe képes tooreset jelszavát hello bérlő jelenleg a megadott jelszó-visszaállítási házirend megfelelően.
* **Tevékenység Aktor** -hello regisztrált felhasználó folyamatos jelszó-visszaállításhoz. Felhasználó vagy egy rendszergazda is lehet.
* **Tevékenység cél** -hello regisztrált felhasználó folyamatos jelszó-visszaállításhoz. Felhasználó vagy egy rendszergazda is lehet.
* **Engedélyezett tevékenység állapotai**
 * _Sikeres_ -azt jelzi, egy felhasználói jelszó-változtatási hello aktuális nyilatkozatnak regisztrálása sikeres volt.
 * _Hiba_ -azt jelzi, a felhasználó nem sikerült tooregister jelszó-visszaállításhoz. Kattintson a hello sor lehetővé teszi a toosee hello **tevékenység állapotának oka** kategória toolearn többet miért hello hiba történt. Megjegyzés: Ez nem jelenti a felhasználó nem tud tooreset van saját jelszót, csak hogy ő nem fejeződött be hello regisztrációs folyamat során. Ha nem ellenőrzött adatok a fiókra, amely a megfelelő (például egy telefonszámot, amelyet a rendszer nem érvényesíti), annak ellenére, hogy nem ellenőrizte, hogy ez a telefonszám, továbbra is használhatják azt tooreset a jelszavát. További információkért lásd: [mi történik, ha a felhasználó regisztrál?](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-learn-more#what-happens-when-a-user-registers)

## <a name="how-tooretrieve-password-management-events-from-hello-azure-ad-reports-and-events-api"></a>Hogyan tooretrieve jelszó felügyeleti események hello Azure AD-jelentéseket és események API
2015. augusztus frissítésétől hello Azure AD-jelentéseket és események API mostantól támogatja a hello jelszó alaphelyzetbe állítása és a jelszó alaphelyzetbe állítása szóregisztrációs jelentéseket szereplő hello információk lekérése során. Ez az API használatával letöltheti az egyes jelszó alaphelyzetbe állítása és jelszó-változtatási regisztrációs események az integráció a choce technológia reporting hello a.

### <a name="how-tooget-started-with-hello-reporting-api"></a>Hogyan a tooget hello reporting API használatába
tooaccess ezeket az adatokat fogjuk toowrite egy kis alkalmazást kell vagy parancsfájl-tooretrieve azt a kiszolgálóink. [Ismerje meg, hogyan tooget használatába hello Azure AD jelentéskészítési API](active-directory-reporting-api-getting-started.md).

Ha már van egy működő parancsfájl, érdemes tooexamine hello jelszó alaphelyzetbe állítása és nyilvántartási események olvashatók be toomeet a forgatókönyvek mellett.

* [SsprActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent): hello elérhető oszlopok listája, a jelszó-átállítási események
* [SsprRegistrationActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprRegistrationActivityEvent): hello elérhető oszlopok listája, a jelszó-átállítási regisztráció események

### <a name="reporting-api-data-retrieval-limitations"></a>Jelentéskészítési API adatok lekérését korlátozásai
Jelenleg az Azure AD-jelentések hello és események API túl veszi fel**75,000 események** a hello [SsprActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent) és [SsprRegistrationActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprRegistrationActivityEvent) típusok , hello átfedés **utolsó 30 nap**.

Ha tooretrieve kell, vagy ez az ablak adatok tárolására, javasoljuk, hogy külső adatbázis megőrzése, és hello API tooquery hello eltérések eredményező használatával. Az ajánlott eljárás toobegin beolvasása ezeket az adatokat, amikor hozzákezd a jelszó alaphelyzetbe állítása a regisztrációt a szervezet, megőrizni a külsőleg, és folytassa előre tootrack hello eltérések ettől.

## <a name="how-toodownload-password-reset-registration-events-quickly-with-powershell"></a>Hogyan toodownload jelszó-változtatási regisztrációs események gyorsan a PowerShell használatával
Továbbá toousing hello Azure AD-jelentéseket és események API közvetlenül, a PowerShell parancsfájl toorecent regisztrációs események alatt hello is használhatja a címtárban. Ez akkor hasznos, ha azt szeretné, hogy toosee, akik mostanában regisztrálva van, vagy szeretné például, hogy a jelszó-átállítási bevezetés tooensure várt folyamatban van.

* [Az Azure AD SSPR regisztrációs tevékenység PowerShell-parancsfájl](https://gallery.technet.microsoft.com/scriptcenter/azure-ad-self-service-e31b8aee)

## <a name="how-tooview-password-management-reports-in-hello-classic-portal"></a>Hogyan tooview jelszókezelés jelentései hello klasszikus portál
toofind hello jelszó jelentések, kövesse az alábbi hello lépéseket:

1. Kattintson a hello **Active Directory** hello a bővítmény [a klasszikus Azure portálon](https://manage.windowsazure.com).
2. Hello portálon megjelenő hello listából válassza ki a címtárát.
3. Kattintson a hello **jelentések** fülre.
4. Keresse meg a hello **tevékenységi naplóit** szakasz.
5. Válassza ki bármelyik hello **jelszó-visszaállítási tevékenység** jelentés vagy hello **jelszó-visszaállítási regisztrációs tevékenység** jelentés.

## <a name="view-password-reset-registration-activity-in-hello-classic-portal"></a>Nézet jelszó-visszaállítási regisztrációs tevékenység hello klasszikus portál
hello jelszó visszaállítási regisztrációs Tevékenységjelentés látható, minden jelszó-átállítási regisztrációk a szervezet történt.  A jelszóátállítás regisztrációját megjelenik ez a jelentés sikeresen regisztrálta hello jelszó a hitelesítési adatokat bármely felhasználó visszaállítási regisztrációs portál ([http://aka.ms/ssprsetup](http://aka.ms/ssprsetup)).

* **Maximális időtartomány**: 30 napban
* **A sorok maximális számát**: 75 000
* **Letölthető**: Igen, a CSV-fájl használatával

### <a name="description-of-report-columns"></a>A jelentés oszlopok leírása
hello alábbi lista ismerteti részletesen hello jelentés oszlopainak mindegyike:

* **Felhasználói** – hello felhasználó, aki kísérlet történt a jelszó alaphelyzetbe állítása a regisztrációs műveletet.
* **Szerepkör** – hello szerepkör hello felhasználó hello könyvtárban.
* **Dátum és idő** – hello dátum és idő hello kísérlet.
* **Adatok regisztrált** – milyen hitelesítési adatok hello felhasználó által megadott során jelszó-átállítási regisztrációk.

### <a name="description-of-report-values"></a>A jelentés értékek leírása
hello következő táblázat minden egyes oszlophoz engedélyezett hello különböző értékeket:

| Oszlop | Megengedett értékek és azok jelentését |
| --- | --- |
| Regisztrált adatok |**Másodlagos e-mail-címet** – használt felhasználó másodlagos e-mail vagy hitelesítési e-mail tooauthenticate<p><p>**Irodai telefon**– a felhasználó használt office phone tooauthenticate<p>**Mobiltelefon** -felhasználó használt mobiltelefonját vagy hitelesítési phone tooauthenticate<p>**Biztonsági kérdések** – felhasználói használt biztonsági kérdések tooauthenticate<p>**Hello fent (pl. másodlagos E-mail + mobiltelefon) kombinációja** – 2 kapu házirend van megadva, és bemutatja, melyik két módszer hello használt felhasználói tooauthentication következik be a jelszó-változtatási kérelmet. |

## <a name="view-password-reset-activity-in-hello-classic-portal"></a>Nézet jelszó-visszaállítási tevékenység hello klasszikus portál
Ez a jelentés tartalmazza az összes jelszó alaphelyzetbe állítása, amely a szervezet történt kísérlet.

* **Maximális időtartomány**: 30 napban
* **A sorok maximális számát**: 75 000
* **Letölthető**: Igen, a CSV-fájl használatával

### <a name="description-of-report-columns"></a>A jelentés oszlopok leírása
hello alábbi lista ismerteti részletesen hello jelentés oszlopainak mindegyike:

1. **Felhasználói** – hello felhasználó, aki kísérlet történt a jelszó alaphelyzetbe állítása (ha hello felhasználói jelszó tooreset megadott hello felhasználói azonosító mező alapján) műveletet.
2. **Szerepkör** – hello szerepkör hello felhasználó hello könyvtárban.
3. **Dátum és idő** – hello dátum és idő hello kísérlet.
4. **Metódussal használt** – hitelesítési módszerek hello használt felhasználói a visszaállítási műveletet.
5. **Eredmény** – hello végeredményét hello jelszó alaphelyzetbe állítása a műveletet.
6. **Részletek** – hello részleteit miért hello jelszó-átállítási ugyanúgy hello értéket eredményezett.  Is tartalmaz a megoldás lépéseket vehet tooresolve, akkor nem várt hiba történt.

### <a name="description-of-report-values"></a>A jelentés értékek leírása
hello következő táblázat minden egyes oszlophoz engedélyezett hello különböző értékeket:

| Oszlop | Megengedett értékek és azok jelentését |
| --- | --- |
| Használt módszerek |**Másodlagos e-mail-címet** – használt felhasználó másodlagos e-mail vagy hitelesítési e-mail tooauthenticate<p>**Irodai telefon** – a felhasználó használt office phone tooauthenticate<p>**Mobiltelefon** – a felhasználó használt mobiltelefonját vagy hitelesítési phone tooauthenticate<p>**Biztonsági kérdések** – felhasználói használt biztonsági kérdések tooauthenticate<p>**Hello fent (pl. másodlagos E-mail + mobiltelefon) kombinációja** – 2 kapu házirend van megadva, és bemutatja, melyik két módszer hello használt felhasználói tooauthentication következik be a jelszó-változtatási kérelmet. |
| eredménye |**Elhagyott** – felhasználói jelszó-visszaállítás elindult, de le félúton helyezkedjenek el keresztül nélkül befejezése<p>**Blokkolt** – felhasználói fiók volt megakadályozta abban toouse jelszó-visszaállítás miatt tooattempting toouse hello jelszó alaphelyzetbe állítása lap, vagy egyetlen jelszó kapu visszaállítása egy 24 órás időtartamon belül túl sokszor<p>**Megszakítva** – felhasználói jelszó-visszaállítás elindult, de rákattintva hello Mégse gomb toocancel hello munkamenet részben módon keresztül <p>**Rendszergazda-e kommunikálni** – felhasználói probléma volt a munkamenetben, hogy nem oldható fel, így hello felhasználó helyett befejezési hello "Forduljon a rendszergazdához" hivatkozásra kattint hello jelszó-átállítási folyamata<p>**Nem sikerült** – felhasználó nem volt képes tooreset jelszót, valószínűleg hello felhasználói nem kellett konfigurált toouse hello szolgáltatást (pl. nincs engedélye, hiányzik a hitelesítési adatokat, felügyelt jelszó a helyszínen, de visszaírási le van tiltva).<p>**Sikeres** – jelszó-visszaállítás sikeres volt. |
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
| A felhasználó megszakította, mielőtt átadná hello szükséges hitelesítési módszerek |Megszakítva |
| A felhasználó megszakította az új jelszó elküldése előtt |Megszakítva |
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

## <a name="next-steps"></a>Következő lépések
Az alábbiakban hivatkozások tooall hello az Azure AD-Jelszóvisszaállítási dokumentációs oldal hivatkozását:

* **Azért van itt, mert problémák merültek fel a bejelentkezéssel kapcsolatban?** Ha igen, [így módosíthatja vagy állíthatja alaphelyzetbe a jelszavát](active-directory-passwords-update-your-own-password.md#reset-or-unlock-my-password-for-a-work-or-school-account).
* [**Hogyan működik** ](active-directory-passwords-how-it-works.md) -megismerése hello hello szolgáltatást, és milyen hat különböző összetevőjét valók
* [**Első lépések** ](active-directory-passwords-getting-started.md) -megtudhatja, hogyan tooallow felhasználók tooreset és a felhőbeli vagy helyszíni jelszavukat
* [**Testre szabhatja** ](active-directory-passwords-customize.md) – megtudhatja, hogyan toocustomize hello meg & működését és viselkedésének hello szolgáltatás tooyour szervezet igényeinek megfelelően
* [**Ajánlott eljárások** ](active-directory-passwords-best-practices.md) -megtudhatja, hogyan tooquickly és kezelhet hatékonyan jelszavakat a szervezetben
* [**Gyakran ismételt kérdések** ](active-directory-passwords-faq.md) -válaszok toofrequently kérdések
* [**Hibaelhárítási** ](active-directory-passwords-troubleshoot.md) -megtudhatja, hogyan tooquickly hello szolgáltatás kapcsolatos problémák elhárítása
* [**További** ](active-directory-passwords-learn-more.md) – elmerülhet hello szolgáltatás működésének műszaki részleteiben hello
