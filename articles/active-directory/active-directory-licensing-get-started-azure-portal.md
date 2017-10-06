---
title: "a licencelés az Azure Active Directory használatába aaaGet |} Microsoft Docs"
description: "Azure Active Directory licencelése működik, hogyan tooget indításának az ajánlott eljárásokkal"
services: active-directory
keywords: "Az Azure AD-licencelés"
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/26/2017
ms.author: curtand
ms.reviewer: piotrci
ms.custom: H1Hack27Feb2017;it-pro
ms.openlocfilehash: 268dab806b8b959790341d630a0355c6a43871d0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="license-yourself-and-your-users-in-azure-active-directory"></a>Licenc saját maga és a felhasználók az Azure Active Directoryban

> [!div class="op_single_selector"]
> * [Az Azure portál utasításokat](active-directory-licensing-get-started-azure-portal.md)
> * [Az Azure klasszikus portál adatok beolvasása](active-directory-licensing-what-is.md)
>
>

Azure Active Directory (Azure AD) identitás, mint a szolgáltatás (IDaaS) megoldás és a Microsoft platform. Az Azure AD különböző kiadásai tartományregisztráció:

* Az Azure Active Directory szabad, amelyik elérhető a Microsoft szolgáltatással, például az Office 365, Dynamics, a Microsoft Intune vagy Azure. Az Azure AD nem hoz létre a fogyasztás díjakat ebben a módban.

* Kiadás, például a fizetős Azure AD:
  - Enterprise Mobility + Security 
  - Prémium szintű Azure AD (P1 és P2)
  - Az Azure AD alapszintű
  - Azure Multi-Factor Authentication

Sok Microsoft online szolgáltatások, például az Azure AD fizetős verziója azonnal felhasználói jogosultságok keresztül Office 365, a Microsoft Intune és az Azure AD vannak. Ezekben az esetekben egy vagy több előfizetés által képviselt hello szolgáltatás beszerzési, és minden előfizetés tartalmaz néhány prepurchased licencek az Ön bérelt szolgáltatásának. Felhasználói jogosultságok által érhetők el:

* A licenc hozzárendelése. 
* Hello felhasználói és hello termék közötti kapcsolat létrehozásához.
* Hello szolgáltatás-összetevők hello felhasználói engedélyezése.
* Licencek fel hello egyik előre.

[Kipróbálom most az Azure AD prémium.](https://portal.office.com/Signup/Signup.aspx?OfferId=01824d11-5ad8-447f-8523-666b0848b381&ali=1#0)

Az Azure AD szolgáltatás képességek az átfogó megismerésében, lásd: [Mi az Azure AD?](active-directory-whatis.md). További információkért lásd: a [szolgáltatói szerződések lap](https://azure.microsoft.com/support/legal/sla/active-directory/v1_0/).

> [!NOTE]
> Azure-előfizetést használatalapú engedélyezte az Azure-erőforrások létrehozását, és képezze le őket tooyour fizetési módot. Nincsenek nem hello előfizetéshez társított licencek száma. Ha egy felhasználónak engedélyt toooperate az Azure-erőforrások leképezése toohello előfizetést engedélyez, ezeket hello előfizetéshez és kezelheti az előfizetéshez kapcsolódó erőforrásokat.

## <a name="how-does-azure-active-directory-licensing-work"></a>Azure Active Directory licencelésének működése

Az Azure AD licencet-alapú munkahelyi szolgáltatások az Azure AD szolgáltatás bérlő előfizetés aktiválása. Miután hello előfizetése aktív, hello szolgáltatás képességek az Azure AD-rendszergazdák által felügyelt, és a licenccel rendelkező felhasználók által használt.

### <a name="manage-subscription-information"></a>-Előfizetés adatainak kezelése

Nagyvállalati mobilitási + biztonsági, prémium szintű Azure AD vagy Azure AD alapvető, a bérlő vásárol hello előfizetéssel, beleértve az érvényességi frissül, és előre fizetett licenccel. Az előfizetési adatai, beleértve rendelt vagy rendelkezésre álló licencek számát hello hello Azure-portálon keresztül érhető el: A **Azure Active Directory**, nyissa meg hello **licencek** hello csempéjén adott címtárhoz. Hello **licencek** panel van is hello legjobb helyen toomanage a licenc-hozzárendelések.

Minden előfizetés áll egy vagy több service-csomagokról, például Azure AD, a multi-factor Authentication, Intune, az Exchange Online vagy SharePoint online-hoz.  Az Azure AD-Licenckezelés does *nem* igényelnek szolgáltatásiszint-terv-kezelést. Az Office 365 eltér, mivel azt a speciális konfigurációs mód toomanage tooincluded szolgáltatást alapul. Az Azure AD üzemelő konfigurációs tooenable szolgáltatások alapul, és kezeli az egyéni engedélyeket.

> [!IMPORTANT]
> Az Azure AD Premium, Azure AD alapvető és nagyvállalati mobilitási + biztonsági előfizetések kiépítve zárt tootheir directory-bérlő. Előfizetések könyvtárak vagy más címtárakban használt tooentitle felhasználók között nem osztható meg. Egy előfizetés közötti könyvtárak, lehetséges, de kell egy támogatási jegy elküldése, vagy törlését és az visszavásárlási közvetlen vásárlások.
>
> Ha az Azure AD vagy Enterprise Mobility + Security mennyiségi licencelés előfizetéssel értékesítik, és hello szerződés magában foglalja a Microsoft Online más szolgáltatásaihoz (például Office 365), ha az aktiválás automatikusan megtörténik. 

### <a name="assign-licenses"></a>Licencek hozzárendelése

Bár egy előfizetés beszerzése minden szükséges képességek fizetett tooconfigure, továbbra is kell osztania licenceket szolgáltatások toousers fizetős Azure ad. Bármely felhasználó, aki keresztül a szolgáltatás fizetős Azure AD hozzáférési tooor rendelkező licencet kell hozzárendelni. A licenc-hozzárendelést a felhasználó és a megvásárolt szolgáltatás, például az Azure AD Premium, a Basic, vagy a nagyvállalati mobilitási + biztonsági közötti társítás.


Kezelése, a címtárban mely felhasználók rendelkezzenek a licenccel érhető el: 

* Licencek hozzárendelése, a hello toogroups [Azure-portálon](active-directory-licensing-whatis-azure-portal.md).
* Hozzárendelés licencek közvetlenül toousers hello portálon, a PowerShell vagy az API-k. 

Licencek tooa csoport hozzárendelésekor még csoport minden tagjára kapott licencet. Ha a felhasználók hozzáadásakor vagy eltávolításakor hello csoportból, hello megfelelő licenc hozzárendelt vagy eltávolítani. Csoport-hozzárendelés minden csoport felügyeleti elérhető tooyou használhatja, amely biztonságicsoport-alapú hozzárendelés tooapplications konzisztens.

Használhat [Csoportalapú licenc-hozzárendelést](active-directory-licensing-whatis-azure-portal.md) tooset például hello következő szabályokat:
* A címtárban szereplő összes felhasználó automatikusan licenc beszerzése
* Mindenki hello megfelelő feladat címmel lekérdezi a licenc
* Hello döntési tooother kezelők hello szervezet delegálhat (használatával [az önkiszolgáló csoportok](active-directory-accessmanagement-self-service-group-management.md))

A licenc hozzárendelése toogroups részletes leírását, beleértve a speciális forgatókönyvek és az Office 365 licencelési forgatókönyvek, lásd: [hozzárendelése az Azure Active Directory csoport tagsága toousers licencek](active-directory-licensing-group-assignment-azure-portal.md).

## <a name="get-started-with-azure-ad-licensing"></a>Ismerkedés az Azure AD-licenceléshez

Ismerkedés az Azure ad-vel is könnyen. A címtár regisztrációt részeként bármikor létrehozhat egy [Azure ingyenes próbát](https://azure.microsoft.com/offers/ms-azr-0044p/).

hello következő bevált gyakorlatát révén biztos lehet abban, hogy a bérlő igazítva van-e, előfordulhat, hogy fel más Microsoft-szolgáltatásokkal és a célokat hello szolgáltatás:

- Ha már használja a Microsoft hello a szervezeti szolgáltatások, az Azure AD-bérlő már rendelkezik. Hasznos toouse hello azonos bérlői más szolgáltatásaihoz, így hello szolgáltatásban core Identitáskezelés, beleértve a kiépítés és hibrid egyszeri bejelentkezés (SSO) is használható legyen. Az egyszeri bejelentkezés a felhasználók előnyt hello sokoldalú képességeit hello szolgáltatásban. Így ha úgy dönt, hogy a dolgozók számára egy fizetős Azure AD szolgáltatás toobuy, azt javasoljuk, hogy ismét ugyanazt a bérlői hello használja.

- Javasoljuk, hogy egy új bérlőt az Azure-portálon hello használjon el, ha azt tervezi, hogy:
  - Használhatja az Azure Active Directory különböző állítja be a felhasználók számára (például a partnerek vagy az ügyfelek).
  - Értékelje ki a termelési service elszigetelt Azure AD szolgáltatásokba.
  - A szolgáltatások egy védőfal mögötti környezet beállítása.

  hello új könyvtár hozza létre a fiók globális rendszergazdai jogosultságokkal rendelkező külső felhasználóként. Amikor bejelentkezik toohello Azure portál, amely ezt a fiókot, tekintse meg ezt a bérlőt, és elérni az összes felügyeleti feladatok.

> [!NOTE]
> Az Azure AD "vendégfelhasználók," felhasználói fiókok Azure AD-bérlő a Microsoft-fiókkal vagy egy Azure AD identity egy másik bérlőhöz keresztül létrehozott képező támogatja. hello Office 365 felügyeleti portál jelenleg nem támogatja ezeket a felhasználókat. Microsoft-fiókkal rendelkező vendégfelhasználók addig nem képes tooaccess hello Office 365 felügyeleti portál, amíg más Azure AD bérlő vendégfelhasználók figyelmen kívül lesznek hagyva. Hello utóbbi esetben csak a felhasználó helyi fiók, az Azure AD hello hello vagy ahol eredetileg hello felhasználó, az Office 365-bérlő érhető el.

### <a name="select-one-or-more-license-trials"></a>Válasszon egy vagy több licenc próbaverzió

Aktiválhatja az Azure AD Premium vagy nagyvállalati mobilitási + biztonsági próba-előfizetést a **Azure Active Directory** &gt; **gyors üzembe helyezési**.

![Válassza ki a licencszerződés próbaverziójának](media/active-directory-licensing-get-started-azure-portal/select-a-license-trial.png)

Próbaverziós licenc érhetők el a hello **licencek** panelen.

### <a name="assign-licenses-toousers-and-groups"></a>Toousers licencek és csoportok hozzárendelése

Miután hello előfizetése aktív, hogy rendeljen egy licencet tooyourself. Ezt követően frissítse, hogy Ön összes hello szolgáltatás hello böngésző tooensure. következő lépés hello tooassign licencek toohello felhasználók, akik hozzáférést toopaid Azure AD-funkciókat. A [licencek hozzárendelése](#assign-licenses), tooassign licencek tooidentify hello csoport hello képviselő célközönség szükséges, és rendelje hozzá a hello licenc tooit egy egyszerű módot. Ezzel a módszerrel hozzáadásakor vagy keresztül életciklus hello csoportból eltávolított felhasználók rendelt vagy hello licenccel, illetve távolítva.

> [!NOTE]
> Egyes Microsoft-szolgáltatások nem érhetők el az összes helyen. Licenc tooa felhasználói rendelhető, mielőtt hello rendszergazdának meg kell adnia hello **felhasználási hely** hello felhasználó tulajdonság. Beállíthatja, hogy ez a tulajdonság a **felhasználói** &gt; **profil** &gt; **beállítások** a hello Azure-portálon. Licenc-hozzárendelést használatakor bármely felhasználó, akinek felhasználási hely nincs megadva örökli hello könyvtár hello helye.

licenc tooassign a **Azure Active Directory** &gt; **licencek** &gt; **összes**, egy vagy több terméket, majd válassza ki és **Hozzárendelése** hello parancssávon.

![Válassza ki a licencszerződés tooassign](media/active-directory-licensing-get-started-azure-portal/select-license-to-assign.png)

Használhatja a hello **felhasználók és csoportok** panel toochoose több felhasználót vagy csoportot vagy toodisable service plans hello termékben. A felhasználó- és csoportnevek akkor használja a felső toosearch hello keresőmezőt.

![Válasszon egy felhasználót vagy csoportot a licenc-hozzárendelést](media/active-directory-licensing-get-started-azure-portal/select-user-for-license-assignment.png)

Licenccsoport tooa hozzárendelésekor most is igénybe vehet egy ideig, amíg az összes felhasználó öröklése hello licenc, attól függően, hogy hello hello csoportba tartozó felhasználók száma. Ellenőrizheti a hello feldolgozási állapot hello **csoport** panel alatt hello **licencek** csempére.

![Licenc-hozzárendelés állapota](media/active-directory-licensing-get-started-azure-portal/license-assignment-status.png)

Hozzárendelési hibák az Azure AD a licenc-hozzárendelést során fordulhatnak elő, de viszonylag ritkán kezelése az Azure AD és a nagyvállalati mobilitási + biztonsági termékeket. A lehetséges hozzárendelési hibák korlátozva:
- Hozzárendelés ütközés: amikor a felhasználó a korábban hozzárendelt a nem kompatibilis a jelenlegi licenc hello licenchez. Ebben az esetben hello új licenc hozzárendelése el kell távolítania jelenlegi hello.
- Túllépte a rendelkezésre álló licencek: hozzárendelt csoportokban lévő felhasználók hello száma meghaladja a rendelkezésre álló licencek hello, amikor a felhasználó-hozzárendelés állapotát tükrözi egy hiba tooassign toomissing licencek miatt.

#### <a name="azure-ad-b2b-collaboration-licensing"></a>Az Azure AD B2B együttműködés licencelés

B2B együttműködés lehetővé teszi tooinvite vendégfelhasználók be az Azure AD bérlői tooprovide hozzáférés tooAzure AD szolgáltatásokba, és bármely Azure-erőforrások elérhetővé tenni kívánt.  

Nincs a B2B felhasználókat fióknevet és az Azure AD tooan alkalmazás hozzárendelése nem kell fizetni. Vendég / too10 alkalmazások fel felhasználói és 3 kapcsolatos alapszintű jelentések szabadon is B2B együttműködés felhasználók számára. Ha a Vendég felhasználó az Azure AD-bérlő hello partner bármely megfelelő licenccel rendelkezik, akkor lesz licenccel kell rendelkezniük a saját is.

Nem kötelező, de ha azt szeretné, hogy tooprovide hozzáférési toopaid az Azure AD-funkciókat, e B2B vendégfelhasználók licenccel kell rendelkezniük az Azure AD licencet megfelelő. Az hívja fel egy licencet a fizetős Azure ad-bérlő B2B együttműködés felhasználói jogosultságokat rendelhet meghívott tooan további öt vendégfelhasználók toohello bérlő. A forgatókönyvek és a vonatkozó adatokat, lásd: [útmutatást licencelési B2B együttműködés](active-directory-b2b-licensing.md).

### <a name="view-assigned-licenses"></a>Licencek hozzárendelése megtekintése

A hozzárendelt és rendelkezésre álló licencek összefoglaló áttekintést jelenik meg **Azure Active Directory** &gt; **licencek** &gt; **összes**.

![Licenc összefoglaló megtekintése](media/active-directory-licensing-get-started-azure-portal/view-license-summary.png)

Hozzárendelt felhasználók és csoportok részletes listáját a termék kiválasztása esetén érhető el. Hello **licenccel rendelkező felhasználók** lista mutatja azokat az összes felhasználó jelenleg fel a licencet, és hogy hello licenc lett hozzárendelve közvetlenül toohello felhasználó, vagy ha öröklés útján egy csoporttól származik.

![Licenc részleteinek megtekintése](media/active-directory-licensing-get-started-azure-portal/view-license-detail.png)

Ehhez hasonlóan hello **licenccel rendelkező csoportok** lista mutatja azokat az összes csoport toowhich licencek van rendelve. Válassza ki a felhasználó vagy csoport tooopen hello **licencek** panel, amelyen látható az összes licenc hozzárendelése toothat objektum.

### <a name="remove-a-license"></a>Távolítsa el a licenc

tooremove licencet, nyissa meg toohello felhasználót vagy csoportot, és nyissa meg a hello **licencek** csempére. Válassza ki a hello licenc, és kattintson a **eltávolítása**.

![Távolítsa el a licenc](media/active-directory-licensing-get-started-azure-portal/remove-license.png)

A csoportból hello felhasználó által örökölt licencek közvetlenül nem távolítható el. Ehelyett távolítsa el a hello felhasználót, ahol hello licenc történő öröklés hello csoportból.

### <a name="extend-trials"></a>Próbaverzió kiterjesztése

Az ügyfelek próba bővítmények érhetők el, önkiszolgáló regisztrációs hello Office 365 portálon keresztül. Egy ügyfél rendszergazdai toohello Office-portálon lépjen (hello Office portál engedélyei függ access), és válassza ki a hello Azure AD prémium csomag próbaidőszaka. Gombra kattintva hello **hosszabbítani próbaidőszakot** hivatkozás hello folyamat elindul. Hitelkártya szükséges, de nem kerül sor.

![Az Azure-portálon hello próbaverzió meghosszabbítása](media/active-directory-licensing-get-started-azure-portal/extend-trial-beginning.png)

## <a name="next-steps"></a>Következő lépések

További részletek toolearn speciális forgatókönyvek a Licenckezelés keresztül csoportok, olvassa el a hello cikk [hozzárendelése licencek tooa csoport](active-directory-licensing-group-assignment-azure-portal.md).

Itt található információ a tooconfigure és más szolgáltatások fizetős Azure AD használatára:

* [Önkiszolgáló jelszóváltoztatás](active-directory-manage-passwords.md)
* [Önkiszolgáló csoportkezelés](active-directory-accessmanagement-self-service-group-management.md)
* [Az Azure AD Connect health](active-directory-aadconnect-health.md)
* [Az Azure többtényezős hitelesítés](../multi-factor-authentication/multi-factor-authentication.md)
* [Prémium szintű Azure AD licencet közvetlen beszerzési](http://aka.ms/buyaadp)
