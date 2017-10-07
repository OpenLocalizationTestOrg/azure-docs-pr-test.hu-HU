---
title: "a klasszikus Azure portálon hello Azure Active Directory-felhasználók aaaLicense |} Microsoft Docs"
description: "A Microsoft Azure Active Directory licencelésének ismertetése, hogyan működik, tooget indításának és ajánlott eljárások, beleértve az Office 365, a Microsoft Intune, és Azure Active Directory prémium és alapszintű kiadásai"
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
ms.openlocfilehash: 4d5f244cbee2ae37a30976f70b5d4f21c3516d90
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-microsoft-azure-active-directory-licensing-in-hello-azure-classic-portal"></a>Mi van a Microsoft Azure Active Directory licencelése a klasszikus Azure portálon hello?

> [!div class="op_single_selector"]
> * [Az Azure portál útmutatást](active-directory-licensing-get-started-azure-portal.md)
> * [Az Azure klasszikus portál adatai](active-directory-licensing-what-is.md)
>
>

Azure Active Directory (Azure AD) a Microsoft Identity Service (IDaaS) megoldás és a platform áll. Az Azure AD az Azure AD ingyenes, amely megtalálható a Microsoft-szolgáltatás, például az Office 365, Dynamics, a Microsoft Intune és az Azure közötti funkcionális és műszaki verziók számos tartományregisztráció (az Azure AD nem hoz létre a fogyasztás díjakat ebben a módban), tooAzure AD fizetős, például a nagyvállalati mobilitási csomag (EMS), a prémium szintű Azure AD és az alapszintű verzió, valamint az Azure multi-factor Authentication (MFA). Sok Microsoft online szolgáltatások, például az Azure AD fizetős verziója azonnal felhasználói jogosultságok keresztül Office 365, a Microsoft Intune és az Azure AD vannak. Ezekben az esetekben hello szolgáltatás beszerzési szerepel egy vagy több előfizetésekkel, és minden előfizetés licencek előzetes beszerzés számos elérhető az Ön bérelt szolgáltatásának. Felhasználói jogosultságok licenc-hozzárendelést, hello felhasználói és hello termék, hello felhasználó hello szolgáltatás-összetevők közötti kapcsolat létrehozása keresztül elérhető, és fel hello egyik előre fizetett licenccel.

> [!IMPORTANT]
> A Microsoft azt javasolja, hogy a hello használata az Azure AD kezelése [az Azure AD felügyeleti központban](https://aad.portal.azure.com) hello az Azure portál használata helyett hello hivatkozott ebben a cikkben a klasszikus Azure portálon. Hogyan center tooassign rendszergazdai szerepkörök az Azure AD Üdvözöljük a rendszergazdákat, lásd: [az Azure AD Ön és a felhasználók licenc](active-directory-licensing-get-started-azure-portal.md).

[Kipróbálom most az Azure AD premium.](https://portal.office.com/Signup/Signup.aspx?OfferId=01824d11-5ad8-447f-8523-666b0848b381&ali=1#0)

> [!NOTE]
> Az Azure AD felügyeleti portál hello a klasszikus Azure portálon része. Pedig bármely Azure vásárlás nem igényelnek, az Azure AD, a portál eléréséhez szükséges egy aktív Azure-előfizetés vagy egy [Azure próba-előfizetés](https://azure.microsoft.com/pricing/free-trial/).
>
>

Az Azure AD szolgáltatás képességek az átfogó megismerésében, lásd: [Mi az Azure AD](active-directory-whatis.md).
[További tudnivalók az Azure AD szolgáltatási szintek](https://azure.microsoft.com/support/legal/sla/)

> [!NOTE]
> Az Azure használatalapú előfizetések eltérőek: közben is képviselt a címtárban, ezeket az előfizetéseket engedélyezte az Azure-erőforrások létrehozását, és oldalváltozókhoz tooyour fizetési módot. Ebben az esetben nincsenek nem hello előfizetéshez társított licencek száma. Felhasználók társítása hello előfizetéssel, hello felhasználók toomanaging előfizetési erőforrások eléréséhez, megvalósítani a leképezett Azure-erőforrások toohello előfizetés engedélyek toooperate határozott megadása.
>
>

## <a name="how-does-azure-ad-licensing-work"></a>Az Azure AD-licencelés működése
Az Azure AD (jogosultság alapján) licenc-alapú munkahelyi szolgáltatásainak a címtárszolgáltatás vagy Azure AD-bérlő előfizetés aktiválása. Aktív hello előfizetés, miután hello szolgáltatás jellemzőinek címtárszolgáltatás vagy a rendszergazdák által felügyelt, és a licenccel rendelkező felhasználók által használt.

Ha és a nagyvállalati mobilitási csomag, a prémium szintű Azure AD és a Azure AD alapvető, a címtár aktiválása hello előfizetéssel, beleértve az érvényességi frissül, és előre fizetett licenccel. Az előfizetési adatok állapot, a következő életciklus esemény és a hello rendelt vagy rendelkezésre álló licencek számát érhető hello hello licencek lapon hello meghatározott könyvtárhoz a klasszikus Azure portálon keresztül. Ez a licenc-hozzárendelések toobest hely toomanage is van.

Minden előfizetés egy vagy több service-csomagokról áll, mindegyik leképezési hello szereplő hello szolgáltatástípus; működési szintje például az Azure Active Directory, az Azure MFA, a Microsoft Intune, az Exchange Online vagy SharePoint online-hoz. Az Azure AD-Licenckezelés szolgáltatás terv szolgáltatásiszint-kezelés nincs szükség. Ez eltér a speciális konfigurációs mód toomanage tooincluded szolgáltatást a Office 365. Az Azure AD szolgáltatás konfigurációjában, tooenable szolgáltatások alapul, és kezeli az egyéni engedélyeket.

Az Azure AD-előfizetés adatainak általában hello hello licencek lapon hello adott címtárhoz a klasszikus Azure portál kezeli. Azure AD-előfizetések az Azure AD Premium hello kivétel miatt nem jelennek meg az Office-portál hello.

> [!IMPORTANT]
> Az Azure AD Premium és a Basic, a nagyvállalati mobilitási csomag előfizetés, valamint vannak kiépítve zárt tootheir directory-bérlő. Előfizetések könyvtárak vagy más címtárakban használt tooentitle felhasználók között nem osztható meg. Előfizetés könyvtárak közötti áthelyezése lehetséges, de egy támogatási jegy vagy megszakítása és újbóli beszerzési hello esetben közvetlen beszerzési elküldése igényel.
>
> Az Azure AD beszerzésekor vagy a nagyvállalati mobilitási csomag előfizetés aktiválással mennyiségi licencelés művelet automatikusan megtörténik, amikor a hello megállapodás tartalmazza a Microsoft Online más szolgáltatásaihoz, például az Office 365.
>
>

A fizetős Azure AD-funkciókat hello directory span hello szélessége. Példák erre vonatkozóan:

* Csoport-alapú hozzárendelés tooapplications, amelyek engedélyezve vannak a kezelt hello adott alkalmazást.
* Speciális és az önkiszolgáló csoportkezelési képességeinek hello directory konfigurációja vagy hello adott csoporton belül.
* Prémium szintű biztonsági jelentések megtalálhatók a hello jelentéskészítési lap
* A cloud discovery-alkalmazás megjelennek hello Azure-portálon identitás.

### <a name="assigning-licenses"></a>Licencek hozzárendelése
Míg egy előfizetés beszerzése minden szükséges képességek fizetett tooconfigure, fizetős szolgáltatások az Azure AD használatához terjesztése licencek toohello jobb egyéni felhasználók számára. Általában bármely felhasználó, aki keresztül a szolgáltatás fizetős Azure AD hozzáférési tooor rendelkező licencet kell hozzárendelni. A licenc-hozzárendelést a felhasználó és a megvásárolt szolgáltatás, például az Azure AD prémium, alapszintű vagy a nagyvállalati mobilitási csomag közötti társítás.

A címtárban mely felhasználók rendelkezzenek a licenc kezelése felettébb egyszerű. Tooa csoport toocreate hozzárendelése a hozzárendelési szabályok hello Azure AD felügyeleti portál használatával, vagy közvetlenül toohello jobb egyéni felhasználók számára a portálon, a PowerShell vagy a API-k licencek hozzárendelésével is elvégezhető. Licencek tooa csoport hozzárendelésekor csoport minden tagjára kioszt egy licencet. Ha a felhasználók hozzáadásakor és hello csoportból eltávolított fogják hozzárendelt vagy eltávolított hello megfelelő licenccel. Csoport-hozzárendelés minden csoport felügyeleti elérhető tooyou nyújthatnak, és a biztonságicsoport-alapú hozzárendelés tooapplications konzisztens. Ezzel a megközelítéssel szabályok beállítása úgy, hogy a címtárban szereplő összes felhasználó automatikusan hozzárendelt, győződjön meg arról, hogy mindenki hello megfelelő feladat címmel rendelkezik-e a licenc vagy még delegált hello döntési tooother kezelők hello szervezetében.

A csoport-alapú licenc-hozzárendelést minden olyan felhasználó, hiányzik a felhasználási hely hozzárendelés során öröklik hello könyvtár azok pontos helye. Ezen a helyen hello rendszergazda bármikor módosíthatja. Azokban az esetekben, ahol a hello hozzárendelése miatt meghiúsult automatikus tooerror, hello a felhasználó adatai, hogy a licenctípus fogja tartalmazni az állapotban.

## <a name="getting-started-with-azure-ad-licensing"></a>Alapvető tudnivalók az Azure AD licenceléséről
Ismerkedés az Azure ad-vel könnyen; a címtár tooa ingyenes Azure-fiók létrehozása igénybe részeként bármikor létrehozhat. [További információk a szervezeti regisztrál](sign-up-organization.md). hello következő segítségével győződjön meg arról, hogy a címtárban legjobb igazodik más Microsoft-szolgáltatásokkal, előfordulhat, hogy fel kell, vagy tervezi, tooconsume, és a célokat, az beszerzése hello szolgáltatást.

Az alábbiakban néhány gyakorlati tanácsokat:

* Ha már használja a Microsoft szervezeti szolgáltatások, Azure AD-címtár már rendelkezik. Ebben az esetben, kell folytatni toouse hello más szolgáltatásaihoz könyvtárába, hogy az Identitáskezelés mag, kiosztás és a hibrid SSO, beleértve hello szolgáltatásban állítható be. A felhasználók egy egyszeri bejelentkezési élmény lesz, és hello szolgáltatásban előnyösek, gazdagabb képességeit. Ennek eredményeképpen ha úgy dönt, hogy az Azure AD toobuy fizetett a munkaerő szolgáltatást, azt javasoljuk, hogy használjon hello azonos directory toodo ez.
* Ha azt tervezi, toouse Azure AD-hez a felhasználók (partnerek, ügyfelek és így tovább) egy másik csoportja, vagy ha szeretné tooevaluate az Azure AD-szolgáltatások, és szeretné toodo, elkülönítve az éles szolgáltatás, vagy ha a toosetup egy védőfal mögötti környezet a szolgáltatások azt javasoljuk, hogy Ön először létre kell hoznia egy új könyvtárat klasszikus hello Azure Azure portálon keresztül. [További tudnivalók: létrehozása egy új Azure AD-címtárát a klasszikus Azure portálon hello](active-directory-licensing-directory-independence.md). hello új címtár globális rendszergazdai jogosultságokkal rendelkező külső felhasználóként a fiók jön létre. Amikor bejelentkezik toohello klasszikus Azure portál ezzel a fiókkal fog lenniük képes toosee ebben a könyvtárban, és minden rendszergazdai feladatokat a címtáron eléréséhez. Azt javasoljuk, hogy hozzon létre egy helyi fiók a megfelelő jogosultságokkal toomanage más Microsoft-szolgáltatásokkal (amelyek hello a klasszikus Azure portálon keresztül nem érhető el). [További tudnivalók a felhasználói fiókok létrehozása az Azure AD](active-directory-create-users.md).

> [!NOTE]
> Az Azure AD támogatja a "külső felhasználók,", amely egy példányát az Azure AD egy Microsoft fiók (msa-t) vagy egy másik címtárban szereplő Azure AD identity használatával létrehozott felhasználói fiókok találhatók. Amíg azt nem foglalt kiterjesztése az összes Microsoft szervezeti szolgáltatást, ez a funkció most ezeket fiókok nem támogatottak az egyes hello szolgáltatások lép; például hello Office 365 felügyeleti portál jelenleg nem támogatja ezeket a felhasználókat. Emiatt külső felhasználók Microsoft-fiókkal rendelkező nem lesz képes tooaccess hello Office 365 felügyeleti portál, amíg más Azure AD-címtártól külső felhasználók figyelmen kívül. Ez utóbbi esetben hello, csak hello felhasználói helyi fiók, hello Azure AD vagy az Office 365 könyvtárban, ahol eredetileg hello felhasználói, lenne érhető el, a felhasználói élmény mellett.
>
>

Jelöli, az Azure AD is rendelkezik különböző fizetős verziót. Ezen verziói között van néhány kisebb különbség a beszerzési rendelkezésre állási:

| Product | EA/VL | Nyílt | Felhőszolgáltató (CSP) | MPN használati jogosultságok | Közvetlen beszerzési | Próbaverzió |
| --- | --- | --- | --- | --- | --- | --- |
| A nagyvállalati mobilitási csomag |X |X |X |X | |X |
| Azure AD Premium |X |X |X | |X |X |
| Az Azure AD alapszintű |X |X |X |X |<br /> |<br /> |

### <a name="select-one-or-more-license-trials"></a>Válasszon egy vagy több licenc próbaverzió
 Minden esetben az Azure AD Premium vagy a nagyvállalati mobilitási csomag próba-előfizetés hello kívánt hello licencek lapon a címtár adott próbaverzió választásával aktiválhatja. Vagy a próbaidőszak 100 licencből egy 30 napos előfizetés tartalmazza.

![Az Azure Active Directory próba licenccsomagok](./media/active-directory-licensing-what-is/trial_plans.png)

![A nagyvállalati mobilitási csomag próba licenccsomagok](./media/active-directory-licensing-what-is/EMS_trial_plan.png)

![Aktív próba licenccsomagok](./media/active-directory-licensing-what-is/active_license_trials.png)

### <a name="assign-licenses"></a>Licencek hozzárendelése
Ha hello előfizetése aktív, rendeljen egy licencet tooyourself, és frissítse a szolgáltatások Ön hello böngésző tooensure. hello következő lépésre tooassign licencek toohello felhasználók, amelyek tooaccess kell, vagy nem szerepelnek a fizetős Azure AD-funkciókat. Leírtaknak azt "Hozzárendelése licenses," hello legjobb módja toodo ez tooidentify hello csoport szükséges hello célközönség képviselő, és azt toohello használatára jogosító licenc hozzárendeléséhez; Ezzel a módszerrel hozzáadásakor vagy keresztül életciklus hello csoportból eltávolított felhasználók rendeli tooor hello licenc távolítva.

tooassign tooa licenccsoport vagy bizonyos felhasználók számára, jelölje be a licenccsomag kívánja tooassign, majd kattintson a hello **hozzárendelése** hello parancssávon.

![Aktív próba licenccsomagok](./media/active-directory-licensing-what-is/assign_licenses.png)

Miután hello kiválasztott terv hello hozzárendelés párbeszédpanelen kiválaszthatja a felhasználók, és hozzáadja őket toohello **hozzárendelése** hello jobb oldali oszlopban. Hello Felhasználólista között, vagy keresse meg a hello üveg konkrét személyek hello keresése használatával jobb felhasználói rács hello. tooassign csoportok válasszon "Csoportok" hello **megjelenítése** menü majd hello jobb toorefresh hello hozzárendelések megjelenített hello ellenőrzése gombra.

![Toogroups licencek hozzárendelése](./media/active-directory-licensing-what-is/assign_licenses_to_groups.png)

Rákereshet vagy csoportok között, és hozzá toohello **hozzárendelése** hello oszlopa azonos módon. A felhasználók és csoportok kombinációját tooassign egyetlen művelettel is használhatja. toocomplete hello hozzárendelési folyamat, hello jobb alsó sarkában hello lap hello ellenőrzése gombra.

![Licenc hozzárendelése állapotüzenetet](./media/active-directory-licensing-what-is/license_assignment_progress_message.png)

Egy csoport hozzá van rendelve, a tagok örökli hello licencek 30 percen belül, de általában 1-2 percen belül.

Hozzárendelési hibák az Azure AD a licenc-hozzárendelést során fordulhatnak elő, de viszonylag ritkán fordul elő. A lehetséges hozzárendelési hibák korlátozva:

* Hozzárendelés ütközés - Ha a felhasználó korábban volt rendelve, amely nem kompatibilis a jelenlegi licenc hello licenc. Ebben az esetben hello új licenc hozzárendelése szükséges hello előzőre eltávolítása.
* Túllépte a rendelkezésre álló licenc - hozzárendelt csoportokban lévő felhasználók hello száma meghaladja a rendelkezésre álló licenc, amikor hello felhasználói hozzárendelés állapota fogja tartalmazni egy hiba tooassign toomissing licencek miatt.

### <a name="view-assigned-licenses"></a>Licencek hozzárendelése megtekintése
Hozzárendelt licencekkel, beleértve az elérhető, a hozzárendelt és a következő előfizetés életciklus esemény összefoglaló áttekintést hello megjelenő **licencek** fülre.

![Hozzárendelt licencekkel hello számának megjelenítése](./media/active-directory-licensing-what-is/view_assigned_licenses.png)

Hozzárendelt felhasználók és csoportok részletes listáját, beleértve hozzárendelési állapotáról és elérési útja (közvetlen vagy egy vagy több csoportokból örökölt) akkor használható, ha olyan licenccsomag történő navigáció.

![Részletek megjelenítéséhez használatos olyan licenccsomag a hozzárendelt licencek](./media/active-directory-licensing-what-is/assigned_licenses_detail.png)

Licencek eltávolítása az imént lehető legkönnyebben hozzárendelése. Ha hello felhasználói közvetlenül hozzá van rendelve, vagy egy hozzárendelt csoport eltávolíthatja hello licenc hello licenctípus kiválasztásával, kiválasztásával **eltávolítása**, hello felhasználó vagy csoport toohello eltávolítása lista hozzáadása és erősítse meg hello művelet. Azt is megteheti, nyissa meg a licenc típusa, jelölje be hello adott felhasználó vagy csoport, és koppintson **eltávolítása** hello parancssávon. tooend licencet egy csoportból, a felhasználó örökségének egyszerűen csoportból hello felhasználói hello.

### <a name="extending-trials"></a>Próbaverzió kiterjesztése
Az ügyfelek próba bővítmények érhetők el az önkiszolgáló hello Office 365 portálon keresztül. Egy ügyfél rendszergazdai navigálhatnak toohello [Office-portál](https://portal.office.com/#Billing) (hello Office portál engedélyei függ access), és válassza ki az Azure AD Premium próbaverzió. Kattintson a hello **hosszabbítani próbaidőszakot** hivatkozásra és hello útmutatást. Szüksége lesz a hitelkártya tooenter, de azt nem kell fizetnie.

![A licenc próbaverziójának hello Office-portál bővítése](./media/active-directory-licensing-what-is/extend_license_trial.png)

Az ügyfelek által a támogatási kérelem elküldése is kérheti a próbaidőszak meghosszabbítását. Egy ügyfél rendszergazdai navigálhatnak toohello Office 365 portál [támogatási oldalára](http://aka.ms/extendAADtrial) (hozzáférés hello Office támogatási lap engedélyeinek függően). Ezen a lapon válassza a "Előfizetések és kísérletek" a szolgáltatások és a "Próbaverzió kérdések" jelenség alatt. Végül adja meg az adatokat a hello körülmények között

![A licenc próbaverziójának egy támogatási kérést használatával kiterjesztése](./media/active-directory-licensing-what-is/alternate_office_aad_trial_extension.png)

## <a name="next-steps"></a>Következő lépések
Most előfordulhat, hogy készen áll a tooconfigure kell, és néhány Azure AD prémium szolgáltatások használatáért.

* [Önkiszolgáló jelszóváltoztatás](active-directory-manage-passwords.md)
* [Önkiszolgáló csoportkezelés](active-directory-accessmanagement-self-service-group-management.md)
* [Az Azure AD Connect heath](active-directory-aadconnect-health.md)
* [Csoport-hozzárendelés tooapplications](active-directory-manage-groups.md)
* [Az Azure többtényezős hitelesítés](../multi-factor-authentication/multi-factor-authentication.md)
* [Prémium szintű Azure AD licencet közvetlen beszerzési](http://aka.ms/buyaadp)
