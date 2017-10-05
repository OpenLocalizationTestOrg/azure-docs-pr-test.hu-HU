---
title: "A klasszikus Azure portál Azure Active Directory-felhasználók licenc |} Microsoft Docs"
description: "A Microsoft Azure Active Directory licencelésének ismertetése, hogyan működik, első lépések és ajánlott eljárások, beleértve az Office 365, a Microsoft Intune, és Azure Active Directory prémium és alapszintű kiadásai"
services: active-directory
keywords: "Az Azure AD-licencelés"
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: d769e8c6-7581-43f5-a3b4-de4b1dca2344
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/24/2017
ms.author: curtand
ms.custom: oldportal;it-pro;H1Hack27Feb2017
ms.openlocfilehash: 9da5bb6987a9eb3398abe0d6066f1945620df9a4
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="what-is-microsoft-azure-active-directory-licensing-in-the-azure-classic-portal"></a>Mi van a Microsoft Azure Active Directory licencelésének a klasszikus Azure portálon?

> [!div class="op_single_selector"]
> * [Az Azure portál útmutatást](active-directory-licensing-get-started-azure-portal.md)
> * [Az Azure klasszikus portál adatai](active-directory-licensing-what-is.md)
>
>

Azure Active Directory (Azure AD) a Microsoft Identity Service (IDaaS) megoldás és a platform áll. Az Azure AD az Azure AD ingyenes, amely megtalálható a Microsoft-szolgáltatás, például az Office 365, Dynamics, a Microsoft Intune és az Azure közötti funkcionális és műszaki verziók számos tartományregisztráció (az Azure AD nem hoz létre a fogyasztás díjakat ebben a módban), az Azure AD fizetős például nagyvállalati mobilitási csomag (EMS), a prémium szintű Azure AD és a Basic, valamint Azure multi-factor Authentication (MFA) verzióit. Sok Microsoft online szolgáltatások, például az Azure AD fizetős verziója azonnal felhasználói jogosultságok keresztül Office 365, a Microsoft Intune és az Azure AD vannak. Ezekben az esetekben a szolgáltatás beszerzési szerepel egy vagy több előfizetésekkel, és minden előfizetés licencek előzetes beszerzés számos elérhető az Ön bérelt szolgáltatásának. Felhasználói jogosultságok a licenc-hozzárendelést, a felhasználó és a termék engedélyezése a felhasználó számára a szolgáltatás-összetevők, és fel egyet, előre csatolása keresztül érhetők el.

> [!IMPORTANT]
> A Microsoft javasolja, hogy az Azure Portalon található [Azure AD felügyeleti központból](https://aad.portal.azure.com) kezelje az Azure AD-t az ebben a cikkben javasolt klasszikus Azure portál helyett. Arról, hogyan rendelhet hozzá rendszergazdai szerepköröket az Azure AD felügyeleti központban című [az Azure AD Ön és a felhasználók licenc](active-directory-licensing-get-started-azure-portal.md).

[Kipróbálom most az Azure AD premium.](https://portal.office.com/Signup/Signup.aspx?OfferId=01824d11-5ad8-447f-8523-666b0848b381&ali=1#0)

> [!NOTE]
> Az Azure AD felügyeleti portál a klasszikus Azure portálon része. Pedig bármely Azure vásárlás nem igényelnek, az Azure AD, a portál eléréséhez szükséges egy aktív Azure-előfizetés vagy egy [Azure próba-előfizetés](https://azure.microsoft.com/pricing/free-trial/).
>
>

Az Azure AD szolgáltatás képességek az átfogó megismerésében, lásd: [Mi az Azure AD](active-directory-whatis.md).
[További tudnivalók az Azure AD szolgáltatási szintek](https://azure.microsoft.com/support/legal/sla/)

> [!NOTE]
> Az Azure használatalapú előfizetések eltérőek: közben is képviselt a címtárban, ezeket az előfizetéseket engedélyezte az Azure-erőforrások létrehozását, és hozzárendelheti a fizetési módot. Ebben az esetben nincsenek nincsenek az előfizetéshez társított licencek száma. Az előfizetés kezelése az előfizetéshez kapcsolódó erőforrásokat, a felhasználók elérését a felhasználói társítást megvalósítani jogosultságokat őket az Azure-erőforrások rendelve az előfizetéshez való működésre.
>
>

## <a name="how-does-azure-ad-licensing-work"></a>Az Azure AD-licencelés működése
Az Azure AD (jogosultság alapján) licenc-alapú munkahelyi szolgáltatásainak a címtárszolgáltatás vagy Azure AD-bérlő előfizetés aktiválása. Amint az előfizetése aktív a szolgáltatási lehetőségeket címtárszolgáltatás vagy a rendszergazdák által kezelhető és licenccel rendelkező felhasználók által használt.

Vásároljon, vagy a nagyvállalati mobilitási csomag, a prémium szintű Azure AD és a Azure AD alapvető aktiválása, a címtár frissül az előfizetést, beleértve a érvényességi időtartam és a fizetett licenccel. Az előfizetési adatok állapot, a következő életciklus esemény és a hozzárendelt vagy elérhető licencek száma érhető el az adott könyvtár licencek lapon a klasszikus Azure portálon keresztül. Ez egyben ajánlott helyre, a licenc-hozzárendelések kezeléséhez.

Minden előfizetés áll egy vagy több service-csomagokról, minden típusú; szolgáltatás részét működési szintje leképezése például az Azure Active Directory, az Azure MFA, a Microsoft Intune, az Exchange Online vagy SharePoint online-hoz. Az Azure AD-Licenckezelés szolgáltatás terv szolgáltatásiszint-kezelés nincs szükség. Ez eltér az Office 365 szolgáltatásból a belefoglalt szolgáltatásokhoz való hozzáférés kezelése a speciális konfigurációs mód. Az Azure AD szolgáltatás konfigurációjában szolgáltatások engedélyezése és kezeli az egyéni engedélyeket támaszkodik.

Az Azure AD-előfizetés adatainak általában a klasszikus Azure portálra, az adott könyvtár licencek lapon kezeli. Azure AD-előfizetések Azure AD Premium számára, kivéve az Office portálon nem jelennek meg.

> [!IMPORTANT]
> Az Azure AD prémium és alapszintű, valamint nagyvállalati mobilitási csomag előfizetések korlátozódnak a kiépített directory bérlője. Előfizetések nem kell könyvtárak elosztva vagy más címtárakban lévő felhasználók jogosíthatók szolgál. Előfizetés könyvtárak közötti áthelyezése is lehetséges, de elküldése egy támogatási jegy vagy megszakítása és újbóli beszerzési közvetlen vásárlás esetén szükséges.
>
> Az Azure AD beszerzésekor vagy a nagyvállalati mobilitási csomag előfizetés aktiválással mennyiségi licencelés művelet automatikusan megtörténik, amikor a szerződés tartalmazza-e a Microsoft Online más szolgáltatásaihoz, például az Office 365.
>
>

A fizetős Azure AD-funkciókat span annak a könyvtárnak a hardverekről. Példák erre vonatkozóan:

* Biztonságicsoport-alapú hozzárendelés alkalmazásokhoz, amelyek engedélyezve vannak az adott alkalmazás kezeli a.
* Speciális és az önkiszolgáló csoportkezelési képességeinek érhetők el a címtár konfigurációja vagy az adott csoport.
* Prémium szintű biztonsági jelentések vannak a jelentés lapon
* A cloud discovery-alkalmazás az Azure portálon identitás jeleníti meg.

### <a name="assigning-licenses"></a>Licencek hozzárendelése
Míg megszerezni egy előfizetés fizetett képességek konfigurálnia kell az összes, az Azure AD-szolgáltatások fizetős használatához a megfelelő személyeket szükséges licencek kiosztása. Általában minden olyan felhasználó, akik hozzáféréssel rendelkezhetnek vagy akik kezeli az Azure AD a szolgáltatás fizetős licencet kell hozzárendelni. A licenc-hozzárendelést a felhasználó és a megvásárolt szolgáltatás, például az Azure AD prémium, alapszintű vagy a nagyvállalati mobilitási csomag közötti társítás.

A címtárban mely felhasználók rendelkezzenek a licenc kezelése felettébb egyszerű. Egy csoport hozzárendelése az Azure AD felügyeleti portál használatával a hozzárendelési szabályok létrehozása vagy licencek hozzárendelése közvetlenül a megfelelő személyeket a portál, a PowerShell vagy az API-k is elvégezhető. Amikor licencet rendel egy csoportot, csoport minden tagjára kioszt egy licencet. Ha a felhasználók hozzáadásakor vagy eltávolításakor hozzá lesz rendelve, vagy a megfelelő licenc eltávolítása a csoportból. Csoport-hozzárendelés nyújthatnak a rendelkezésre álló csoportkezelés, és egységes alkalmazásokhoz történő Csoportalapú hozzárendelése. Ezzel a megközelítéssel szabályok beállítása úgy, hogy a címtárban szereplő összes felhasználó automatikusan hozzárendelt, győződjön meg arról, hogy a megfelelő beosztás rendelkező összes felhasználó rendelkezik-e a licenc vagy a szervezet más kezelői a döntést még delegálása.

Csoportalapú licenc hozzárendelése minden olyan felhasználó, hiányzik a felhasználási hely hozzárendelés során örökli a könyvtár azok pontos helye. Ezen a helyen a rendszergazda bármikor módosíthatja. Azokban az esetekben, ahol az automatikus hozzárendelés hiba miatt sikertelen volt a felhasználó adatai, hogy licenctípus állapotban fogja tartalmazni.

## <a name="getting-started-with-azure-ad-licensing"></a>Alapvető tudnivalók az Azure AD licenceléséről
Ismerkedés az Azure ad-vel könnyen; bármikor létrehozhat egy ingyenes Azure-próbaverzióra regisztráljanak részeként a címtárban. [További információk a szervezeti regisztrál](sign-up-organization.md). A következő segítségével győződjön meg arról, hogy a címtárban legjobb igazodik más Microsoft-szolgáltatásokban is fel kell, vagy tervezi használni, és a kitűzött célokat a szolgáltatás megszerezni.

Az alábbiakban néhány gyakorlati tanácsokat:

* Ha már használja a Microsoft szervezeti szolgáltatások, Azure AD-címtár már rendelkezik. Ebben az esetben kell továbbra is használhatja könyvtárába más szolgáltatásaihoz, hogy az Identitáskezelés mag, kiosztás és a hibrid SSO, beleértve a szolgáltatások között állítható be. A felhasználók egy egyszeri bejelentkezési élmény lesz, és szolgáltatásban előnyösek, gazdagabb képességeit. Ennek eredményeképpen ha úgy dönt, hogy a dolgozók számára a szolgáltatás fizetős Azure AD vásárolni, ajánlott, hogy ugyanaz a könyvtár ehhez.
* Ha azt tervezi, az Azure AD használatára különböző állítja be a felhasználók (partnerek, ügyfelek és így tovább), vagy ha szeretné kiértékelni az Azure AD szolgáltatásokba, és szeretné, hogy az éles szolgáltatás önmagában, vagy ha egy védőfal mögötti környezet, a szolgáltatások beállítása, azt javasoljuk, hogy először létrehoz egy új könyvtárat a klasszikus Azure-Azure-portálon keresztül. [További tudnivalók: létrehozása egy új Azure AD-címtárát a klasszikus Azure portálon](active-directory-licensing-directory-independence.md). Az új könyvtár globális rendszergazdai jogosultságokkal rendelkező külső felhasználóként a fiók jön létre. Amikor bejelentkezik a klasszikus Azure-portálon ezzel a fiókkal, lesz, ez a könyvtár megtekintéséhez és eléréséhez minden rendszergazdai feladatokat a címtáron. Azt javasoljuk, hogy hozzon létre egy helyi fiók megfelelő jogosultságokkal más Microsoft-szolgáltatások (amelyek a klasszikus Azure portálon keresztül nem érhető el) kezelésére. [További tudnivalók a felhasználói fiókok létrehozása az Azure AD](active-directory-create-users.md).

> [!NOTE]
> Az Azure AD támogatja a "külső felhasználók,", amely egy példányát az Azure AD egy Microsoft fiók (msa-t) vagy egy másik címtárban szereplő Azure AD identity használatával létrehozott felhasználói fiókok találhatók. Amíg azt nem foglalt kiterjesztése az összes Microsoft szervezeti szolgáltatást, ez a funkció most ezeket fiókok nem támogatottak az egyes a szolgáltatások lép; például az Office 365 felügyeleti portál jelenleg nem támogatja ezeket a felhasználókat. Ennek eredményeképpen a Microsoft-fiókokkal külső felhasználók csak akkor érhessék el az Office 365 felügyeleti portál, amíg a külső felhasználók más Azure AD-címtártól a rendszer figyelmen kívül hagyja. Az utóbbi esetben csak a felhasználó helyi fiók, az Azure AD vagy az Office 365 könyvtár, ahol a felhasználó eredetileg, elérhető, a felhasználói élmény mellett.
>
>

Jelöli, az Azure AD is rendelkezik különböző fizetős verziót. Ezen verziói között van néhány kisebb különbség a beszerzési rendelkezésre állási:

| Product | EA/VL | Nyílt | Felhőszolgáltató (CSP) | MPN használati jogosultságok | Közvetlen beszerzési | Próbaverzió |
| --- | --- | --- | --- | --- | --- | --- |
| A nagyvállalati mobilitási csomag |X |X |X |X | |X |
| Azure AD Premium |X |X |X | |X |X |
| Az Azure AD alapszintű |X |X |X |X |<br /> |<br /> |

### <a name="select-one-or-more-license-trials"></a>Válasszon egy vagy több licenc próbaverzió
 Minden esetben aktiválhatja a kívánt licencek lapján a címtár adott próbaverzió kiválasztásával egy Azure AD Premium vagy a nagyvállalati mobilitási csomag próba-előfizetést. Vagy a próbaidőszak 100 licencből egy 30 napos előfizetés tartalmazza.

![Az Azure Active Directory próba licenccsomagok](./media/active-directory-licensing-what-is/trial_plans.png)

![A nagyvállalati mobilitási csomag próba licenccsomagok](./media/active-directory-licensing-what-is/EMS_trial_plan.png)

![Aktív próba licenccsomagok](./media/active-directory-licensing-what-is/active_license_trials.png)

### <a name="assign-licenses"></a>Licencek hozzárendelése
Amint az előfizetése aktív, a licenc kiosztani saját magának, és frissítse a böngészőt annak érdekében, hogy a szolgáltatások Ön. A következő lépés a szükséges licencek kiosztása a felhasználók számára, amelyek eléréséhez, vagy szerepelnie kell a fizetős Azure AD-funkciókat. Mint azt korábban említettük, a "Hozzárendelése licenses", a legjobb módja a kívánt célközönség képviselő csoportnak, és rendelje hozzá a licenc;-e Ezzel a módszerrel hozzáadott és életciklus keresztül a csoportból eltávolított felhasználók számára kell rendelt vagy lesz távolítva a licencfeltételeket.

Licenc hozzárendelése egy vagy az egyéni felhasználók számára, jelölje ki a licencet szeretne hozzárendelni, és kattintson a **hozzárendelése** a parancssávon.

![Aktív próba licenccsomagok](./media/active-directory-licensing-what-is/assign_licenses.png)

Ha a kiválasztott terv hozzárendelés párbeszédpanelen kiválaszthatja a felhasználók, és hozzáadják őket a **hozzárendelése** a jobb oldali oszlopban. A Felhasználólista között, vagy keresse meg a keresése használatával konkrét személyek üveg felső sarkában található a felhasználó rács. Rendelje hozzá a csoportokat, válassza a "Csoport" a **megjelenítése** menü és kattintson az ellenőrzés gombra kattint, a jobb oldalon jelennek meg a hozzárendeléseket frissítéséhez.

![Licencek hozzárendelése csoportokhoz](./media/active-directory-licensing-what-is/assign_licenses_to_groups.png)

Rákereshet vagy csoportok között, és hozzáadhatja a **hozzárendelése** oszlop azonos módon. Felhasználók és csoportok egyetlen művelettel hozzárendelni azt is használhatja. A hozzárendelési folyamat befejezéséhez kattintson a pipa gombra az oldal jobb alsó sarokban.

![Licenc hozzárendelése állapotüzenetet](./media/active-directory-licensing-what-is/license_assignment_progress_message.png)

Egy csoport hozzá van rendelve, a tagok örökli a licencek 30 percen belül, de általában 1-2 percen belül.

Hozzárendelési hibák az Azure AD a licenc-hozzárendelést során fordulhatnak elő, de viszonylag ritkán fordul elő. A lehetséges hozzárendelési hibák korlátozva:

* Hozzárendelés ütközés - Ha a felhasználó korábban hozzárendelt licenccel, amely nem kompatibilis a jelenlegi licenc. Ebben az esetben a új licenc szükséges az előző példány eltávolítása.
* Túllépte a rendelkezésre álló licenc - hozzárendelt csoportokban lévő felhasználók száma meghaladja a rendelkezésre álló licenc, amikor a felhasználók hozzárendelési állapota fogja tartalmazni egy hiba miatt hiányzó licencek hozzárendelése.

### <a name="view-assigned-licenses"></a>Licencek hozzárendelése megtekintése
Hozzárendelt licencekkel, beleértve az elérhető, a hozzárendelt és a következő előfizetés életciklus esemény összefoglaló áttekintést jelenik meg a **licencek** fülre.

![A hozzárendelt licencek számának megtekintése](./media/active-directory-licensing-what-is/view_assigned_licenses.png)

Hozzárendelt felhasználók és csoportok részletes listáját, beleértve hozzárendelési állapotáról és elérési útja (közvetlen vagy egy vagy több csoportokból örökölt) akkor használható, ha olyan licenccsomag történő navigáció.

![Részletek megjelenítéséhez használatos olyan licenccsomag a hozzárendelt licencek](./media/active-directory-licensing-what-is/assigned_licenses_detail.png)

Licencek eltávolítása az imént lehető legkönnyebben hozzárendelése. Ha a felhasználó közvetlenül hozzá rendelt vagy egy hozzárendelt csoport, távolítsa el a licencfeltételeket jelölje ki a licencet, kiválasztásával **eltávolítása**, a felhasználó vagy csoport hozzáadása a remove listához, és erősítse meg a műveletet. Másik lehetőségként megnyitásához írja be a licenc, az adott felhasználó vagy csoport kiválasztása és koppintson **eltávolítása** a parancssávon. Egy csoportból licencet a felhasználó örökségének befejezéséhez egyszerűen eltávolítja a felhasználót a csoporthoz.

### <a name="extending-trials"></a>Próbaverzió kiterjesztése
Az ügyfelek próba bővítmények érhetők el az önkiszolgáló az Office 365 portálon keresztül. Egy ügyfél rendszergazdai lépjen a [Office-portál](https://portal.office.com/#Billing) (az Office portál engedélyei függ access), és válassza ki az Azure AD Premium próbaverzió. Kattintson a **hosszabbítani próbaidőszakot** hivatkozásra, és kövesse az utasításokat. Adjon meg egy hitelkártyának kell, de azt nem kell fizetnie.

![Az Office portálon licenc próbaverzió kiterjesztése](./media/active-directory-licensing-what-is/extend_license_trial.png)

Az ügyfelek által a támogatási kérelem elküldése is kérheti a próbaidőszak meghosszabbítását. Egy ügyfél rendszergazdai lépjen az Office 365 portál [támogatási oldalára](http://aka.ms/extendAADtrial) (hozzáférés az Office-támogatás lap engedélyeinek függően). Ezen a lapon válassza a "Előfizetések és kísérletek" a szolgáltatások és a "Próbaverzió kérdések" jelenség alatt. Végül írja be az adatokat a körülmények között

![A licenc próbaverziójának egy támogatási kérést használatával kiterjesztése](./media/active-directory-licensing-what-is/alternate_office_aad_trial_extension.png)

## <a name="next-steps"></a>Következő lépések
Valószínűleg most már készen áll a konfigurálására, és néhány Azure AD prémium szolgáltatások használatáért.

* [Önkiszolgáló jelszóváltoztatás](active-directory-manage-passwords.md)
* [Önkiszolgáló csoportkezelés](active-directory-accessmanagement-self-service-group-management.md)
* [Az Azure AD Connect heath](active-directory-aadconnect-health.md)
* [Csoport-hozzárendelés alkalmazásokhoz](active-directory-manage-groups.md)
* [Az Azure többtényezős hitelesítés](../multi-factor-authentication/multi-factor-authentication.md)
* [Prémium szintű Azure AD licencet közvetlen beszerzési](http://aka.ms/buyaadp)
