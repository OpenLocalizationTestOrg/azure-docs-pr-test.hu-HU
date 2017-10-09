---
title: "személyes adatok aaaProtect Azure identitások és hozzáférések vezérlőkkel |} Microsoft Docs"
description: "Azure identitás- és hozzáférés-vezérlők toohelp használatával, a személyes adatok védelme"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2017
ms.author: barclayn
ms.custom: 
ms.openlocfilehash: 3132c2af25f86662668e5b555eab1d81de7f2e6a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-and-multi-factor-authentication-protect-personal-data-with-identity-and-access-controls"></a>Az Azure Active Directory és a multi-factor Authentication: identitások és hozzáférések vezérlőkkel személyes adatok védelme

Ez a cikk ismerteti, használhatja a tooprotect személyes adatokat az Azure Active Directory és a többtényezős hitelesítési biztonsági szolgáltatások és szolgáltatások használatával.

## <a name="scenario"></a>Forgatókönyv

A nagy körutazás vállalati telephelyének hello az Amerikai Egyesült Államokban, a műveletek toooffer útvonalak hello mediterrán, Adriai, és Balti tengerek, valamint hello Brit-szigetekre növekszik. toosupport e erőfeszítéseket szerzett több kisebb körutazás sorok Olaszország, német, Dánia és hello brit 

hello vállalati hello felhő Microsoft Azure toostore vállalati adatait használja. Ez magában foglalja a személyes azonosításra alkalmas adatokat, például neveket, címeket, telefonszámokat, és a globális felhasználói bázis hitelkártya adatait. Minden helyen hagyományos emberi erőforrások adatokat, például a címet, telefonszámot, azonosító számokat és a vállalat alkalmazottai orvosi információt is tartalmaz. hello körutazás sor is fenntartja, nagy adatbázis ellenszolgáltatás és hűség program tagok, amely tartalmazza az aktuális és korábbi ügyfelek tootrack kapcsolatokkal személyes adatokat.

Vállalati alkalmazottak hozzáférési hello hálózati hello vállalati távoli iroda és utazás ügynökök található hello világ rendelkezik toosome vállalati erőforrások eléréséhez.

## <a name="problem-statement"></a>Probléma leírása

hello vállalati kell védelmében hello ügyfelek és az alkalmazottak a személyes adatok a támadóktól sérült toouse identitások toogain hozzáférést kér. Is biztosítaniuk kell, hogy jogosult felhasználók adatait csak azok, akik szükség lenne rá toodo korlátozott hozzáférés toopersonal a munkájukat.

## <a name="company-goal"></a>Vállalati cél

hello vállalati célja toopersonal adatokat elérő tooensure szigorú vezérlését. Fontos, hogy a felhasználók hozzáférést toopersonal adatokkal azonosítói erős hitelesítés védi. A házirend a [minimális jogosultság] (https://en.wikipedia.org/wiki/Principle_of_least_privilege), hogy jogos felhasználója csak hello szintű hozzáférést a szükséges, és nem több kényszerítettnek kell lennie.

## <a name="solutions"></a>Megoldások

A Microsoft Azure access tooresources személyes adatokat tartalmazó rendelkező toohelp vállalatok felügyeletét biztosítja, identitás- és hozzáférés-felügyeleti eszközök.

### <a name="azure-active-directory"></a>Azure Active Directory

[Az Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) (AAD) identitások kezeli, és szabályozza a hozzáférést tooAzure, valamint egyéb a helyszíni és más felhőalapú erőforrásokat, adatokhoz és alkalmazásokhoz. [Az Azure Active Directory Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/active-directory-securing-privileged-access) segít az Azure rendszergazdák toominimize hello személyek, akiknek toocertain adatok eléréséhez például a személyes adatok száma. Lehetővé teszi őket toodiscover korlátozása és figyelheti a kiemelt jogosultságú identitások és azok hozzáférés tooresources, és ideiglenes, tooassign fordítója (JIT) rendszergazdai jogosultságokkal tooeligible felhasználók. Emellett biztosítja az AAD-rendszergazdai jogosultságokkal rendelkező személyek betekintést.

az AAD PIM használatával járó hello tevékenységei közé tartoznak:

- A Privileged Identity Management engedélyezése a címtáron

- A Privileged Identity Management felügyeleti Irányítópult toosee fontos információk segítségével egy pillanat alatt

- Hello emelt szintű identitások (rendszergazdák) hozzáadásával vagy eltávolításával a rendszergazdák állandó vagy jogosult tooeach szerepkör kezelése

- Hello szerepkör aktiválási beállításainak konfigurálása

- Szerepkörök aktiválása

- Szerepkör tevékenység áttekintése

#### <a name="how-do-i-enable-aad-pim"></a>Hogyan engedélyezhető az aad-ben PIM?

a PIM használatát a címtár toostart hello a következő:

1. Jelentkezzen be toohello Azure-portálon a címtár globális rendszergazdaként.

2. Ha a szervezet több címtárral rendelkezik, válassza ki a felhasználónév hello jobb felső sarokban a hello Azure-portálon. Válassza ki a hello directory, ahol az Azure AD Privileged Identity Management szüksége lesz.

3. Válassza ki **további szolgáltatások** és hello **szűrő** szövegmező toosearch az Azure AD Privileged Identity Management.

4. Ellenőrizze **PIN-kód toodashboard** majd **létrehozása**. Megnyílik a Privileged Identity Management alkalmazás hello.

Azure AD Privileged Identity Management beállítása után hello navigációs panelen láthatja hello alkalmazás minden indításakor.

![](media/protect-personal-data-identity-access-controls/azure-pim.png)

További információkat és utasításokat Ismerkedés az aad-ben PIM [Start használata az Azure AD Privileged Identity Management.](https://docs.microsoft.com/active-directory/active-directory-privileged-identity-management-getting-started)

### <a name="azure-role-based-access-control"></a>Azure szerepköralapú hozzáférés-vezérlés

[Szerepköralapú hozzáférés-vezérlés az Azure](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) (RBAC) hozzáférési tooAzure erőforrások kezelése hello nyújtása hello felhasználó hozzárendelt szerepkörön alapuló hozzáférés engedélyezésével az Azure rendszergazdáit segíti. Elkülönítse a csapaton belüli feladatokat, és adja meg csak hello mennyisége hozzáférés toousers, csoportok és alkalmazások, hogy be kell tooperform a munkájukat.

Szerepköralapú hozzáférés-toousers hello Azure-portálon, az Azure parancssori eszközöket vagy Azure szolgáltatásfelügyeleti API használatával engedélyezhetők.

Azure RBAC alapokkal kapcsolatos további információkért lásd: [Ismerkedés a szerepköralapú hozzáférés-vezérlés a hello Azure portálon.](https://docs.microsoft.com/active-directory/role-based-access-control-what-is)

#### <a name="how-do-i-manage-azure-rbac-with-powershell"></a>Hogyan kezelhető az Azure RBAC a PowerShell segítségével?

Használhatja a PowerShell-parancsmagok toomanage Azure RBAC, beleértve a következő felügyeleti feladatok hello:

- Lista szerepkörök

- Lásd a kinek van hozzáférése:

- Hozzáférés biztosítása

- Megszünteti a hozzáférést

- Egyéni szerepkör létrehozása

- Egy erőforrás-szolgáltató műveleteinek beolvasása

- Egyéni szerepkör módosítása

- Egyéni szerepkör törléséhez

- Egyéni szerepkörök listája

Hogyan toomanage a PowerShell használatával, az Azure RBAC: kapcsolatos utasításokat [kezelése szerepköralapú hozzáférés az Azure PowerShell](https://docs.microsoft.com/azure/active-directory/role-based-access-control-manage-access-powershell).

### <a name="azure-multi-factor-authentication"></a>Azure Multi-Factor Authentication

[Az Azure multi-factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/) (MFA) egy kétlépéses ellenőrzés megoldás, amely segít a biztonságos működés érdekében hozzáférés toodata és az alkalmazások, egyszerű bejelentkezési folyamatot a felhasználó igény szerint betartása mellett. Számos (például telefonos megerősítést, szöveges üzenetet vagy mobilalkalmazást használó) ellenőrzési módszerének köszönhetően erős hitelesítést biztosít.

toofirst szüksége toodeploy MFA hello Azure felhőben, az engedélyezéshez, és kapcsolja be a kétlépéses ellenőrzést, a felhasználók számára.

#### <a name="how-do-i-enable-azure-toouse-mfa"></a>Hogyan engedélyezhető az Azure toouse MFA?

Ha a felhasználók, amely tartalmazza az Azure multi-factor Authentication licencek, nincs szükség, hogy kell-e az Azure MFA toodo tooturn. Ha nem, a multi-factor Auth provider toocreate a könyvtárban van szüksége. toodo, kövesse az alábbi lépéseket:

1. Válassza ki **Active Directory** a hello (bejelentkezve rendszergazdaként) a klasszikus Azure portálon.

2. Válassza ki **többtényezős hitelesítési szolgáltatók.**

3. Válassza ki **új** majd a **alkalmazásszolgáltatások** kiválasztása **többtényezős hitelesítésszolgáltató.**

4. Válassza ki **Gyorslétrehozás.**

5. Hello neve mezőbe, és válassza ki a használati modell (engedélyezett felhasználónkénti vagy hitelesítésenkénti).

6. Kijelöl egy könyvtárat, mely hello MFA-szolgáltatóra társítva.

7. Kattintson a hello **létrehozása** gombra.

![](media/protect-personal-data-identity-access-controls/quick-create.png)

További útmutatást toomanage a többtényezős hitelesítésszolgáltató, lásd: [első lépések az Azure multi-factor Auth Provider.](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-auth-provider)

#### <a name="how-do-i-turn-on-two-step-verification-for-users"></a>Hogyan kapcsolja a kétlépéses ellenőrzést, a felhasználók számára?

Kényszerítheti a kétlépéses ellenőrzést az összes bejelentkezéseket, vagy hozhat létre feltételes hozzáférési házirendek toorequire kétlépéses ellenőrzést, csak akkor, ha a megadott feltétel teljesül.

Azure MFA engedélyezése a felhasználói állapotok módosításával egy hagyományos megközelítés hello megkövetelő a kétlépéses ellenőrzést. Minden hello felhasználónak, amely engedélyezi a hello azonos követelmény tooperform kétlépéses ellenőrzést, minden alkalommal, amikor bejelentkeznek az. A feltételes hozzáférési szabályzatok, amelyek befolyásolhatják, hogy a felhasználó engedélyezése egy felhasználó felülbírálja.

Azure MFA engedélyezésével a feltételes hozzáférési házirenddel egy olyan rugalmasabb megközelítés megkövetelő a kétlépéses ellenőrzést. Toogroups, valamint az egyes felhasználókra vonatkozó feltételes hozzáférési házirendeket is létrehozhat. Magas kockázatú csoportok adható meg több korlátozás mint alacsony kockázat csoportok, vagy a kétlépéses ellenőrzés csak magas kockázatú felhőalkalmazások szükséges, és kihagyja a alacsony kockázat néhányat a meglévők közül. Feltételes hozzáférés azonban egy fizetős az Azure Active Directory szolgáltatás.

tooenable MFA felhasználói állapot módosításával hello a következő:

1. Jelentkezzen be toohello Azure portálra rendszergazdaként.
2. Nyissa meg túl**Azure Active Directory \> felhasználók és csoportok \> minden felhasználó**.
3. Válassza ki **a multi-factor Authentication**.
4. Keresés hello felhasználói tooenable szeretné az Azure MFA számára. Előfordulhat, hogy toochange hello nézet hello tetején.
5. Ellenőrizze a hello mezőben következő toohello felhasználó nevét.
6. A jobb oldali Gyorsműveletek a hello válassza **engedélyezése**.

   ![](media/protect-personal-data-identity-access-controls/quick-create.png)

7. Erősítse meg választását a hello előugró ablakban.  Felhasználók, akik számára engedélyezve van az MFA kell adnia tooregister hello következő bejelentkezéskor.

a feltételes hozzáférési házirend, az Azure MFA tooenable hello a következő:

1. Jelentkezzen be toohello Azure portálra rendszergazdaként.

2. Nyissa meg túl**Azure Active Directory \> feltételes hozzáférés**.

3. Válassza ki **új házirend**.

4. A **hozzárendelések**, jelölje be **felhasználók és csoportok**. Használjon hello **Include** és **kizárása** lapokon toospecify mely felhasználók és csoportok hello házirend fogja kezelni.

5. A **hozzárendelések**, jelölje be **felhőalapú alkalmazásokba.** Válassza ki a túl**tartoznak azok az összes felhő alkalmazások**.
6.  A **hozzáférés-szabályozási**, jelölje be **Grant**. Válasszon **többtényezős hitelesítést**.
7.  Kapcsolja be **házirend engedélyezése** túl**a** majd **mentése**.

Információk hogyan tooconfigure Azure MFA beállítások tooset csalás figyelmeztetéseket, az egyszeri Mellőzés létrehozni, használjon egyedi hangüzenetek, gyorsítótár konfigurálása, adja meg a megbízható IP-címek, alkalmazásjelszavak létrehozásának, jelszóelőzmények eszközök, felhasználók megbízik, és válassza ki az MFA engedélyezése az ellenőrzési módszereket, tekintse meg [Azure multi-factor Authentication beállításainak konfigurálása.](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-whats-next)

## <a name="next-steps"></a>Következő lépések

- [Az Azure Active Directory biztonságossá tétele a privilegizált hozzáférési jogosultsága.](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/active-directory-securing-privileged-access)

- [Azure Multi-Factor Authentication – gyakran ismételt kérdések](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-faq)

- [Szerepköralapú hozzáférés-vezérlés hibaelhárítása](https://docs.microsoft.com/azure/active-directory/role-based-access-control-troubleshooting)

- [Az Azure Active Directory azonosító adatok védelmét](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection)
