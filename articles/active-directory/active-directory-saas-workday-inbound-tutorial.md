---
title: "Oktatóanyag: A helyszíni Active Directory és az Azure Active Directory-kiépítés automatikus felhasználói Workday konfigurálása |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse Workday Active Directory és az Azure Active Directory azonosító adatok forrásaként."
services: active-directory
author: asmalser-msft
documentationcenter: na
manager: femila
ms.assetid: 1a2c375a-1bb1-4a61-8115-5a69972c6ad6
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/26/2017
ms.author: asmalser
ms.openlocfilehash: d4eb3237b8fe7614606c58b39fbefcb44f4060fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configure-workday-for-automatic-user-provisioning-with-on-premises-active-directory-and-azure-active-directory"></a>Oktatóanyag: A helyszíni Active Directory és az Azure Active Directory-kiépítés automatikus felhasználói Workday konfigurálása
hello Ez az oktatóanyag célja meg a szükséges tooperform tooimport személyek a WORKDAY-ből az Active Directory és az Azure Active Directoryban, néhány attribútumok tooWorkday választható visszaírása lépéseket hello tooshow. 



## <a name="overview"></a>Áttekintés

Hello [kiépítése szolgáltatáshoz Azure Active Directory-felhasználó](active-directory-saas-app-provisioning.md) hello integrálható [Workday emberi erőforrások API](https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Get_Workers.html) a rendezés tooprovision felhasználói fiókokat. Az Azure AD a következő üzembe helyezési munkafolyamatok felhasználói kapcsolat tooenable hello használja:

* **Kiépítés felhasználók tooActive Directory** -kijelölt felhasználócsoportokhoz a WORKDAY-ből egy vagy több Active Directory-erdővel történő szinkronizálása. 

* **Csak felhőalapú felhasználók tooAzure Active Directory-kiépítés** -megtalálhatók az Active Directory és az Azure Active Directory hibrid felhasználók létesíthetők hello ez utóbbi használatával történő [AAD-csatlakozás](connect/active-directory-aadconnect.md). Azonban a felhasználók, akik csak felhőalapú telepíthető közvetlenül a WORKDAY-ből tooAzure Active Directory használatával hello szolgáltatás kiépítését az Azure AD-felhasználó.

* **Az e-mailek visszaírási címek tooWorkday** -szolgáltatás kiépítését hello Azure AD-felhasználó lehet írni a kijelölt Azure AD felhasználói attribútumok hátsó tooWorkday, például az üdvözlő e-mail címet.

### <a name="scenarios-covered"></a>Ismertetett forgatókönyvek

hello Workday felhasználói szolgáltatás kiépítését hello Azure AD-felhasználó által támogatott munkafolyamatok kiépítése lehetővé teszi, hogy az emberi erőforrások és identitás életciklus-kezelési forgatókönyveket a következő hello automatizálását:

* **Új alkalmazottak felvétele** – Ha egy új alkalmazott tooWorkday, megjelenik egy felhasználói fiókot automatikusan létrejönnek az Active Directory, Azure Active Directoryban, és opcionálisan Office 365 és [más Azure AD által támogatott SaaS-alkalmazásokhoz ](active-directory-saas-app-provisioning.md), hátsó hello e-mail cím tooWorkday írható.

* **Alkalmazott attribútum és profil frissítések** – amikor egy alkalmazott bejegyzés frissül a Workday (például a nevét, címét, vagy manager), a felhasználói fiók automatikusan frissül az Active Directory, Azure Active Directoryban, és opcionálisan Office 365 és [más SaaS-alkalmazásokhoz az Azure AD által támogatott](active-directory-saas-app-provisioning.md).

* **Alkalmazott végződnek** – Ha egy alkalmazott a rendszer megszakítja a munkanapok, a felhasználói fiók automatikusan le van tiltva az Active Directory, Azure Active Directoryban, és opcionálisan Office 365 és [más SaaS-alkalmazásokhoz az Azure AD által támogatott](active-directory-saas-app-provisioning.md).

* **Alkalmazott újra bízza** – Ha egy alkalmazott van rehired a munkanapok, a régi fiókot automatikusan aktiválhatók vagy újra kiosztott (rendelkezésére) tooActive Directory, Azure Active Directoryban, és opcionálisan az Office 365 és [más SaaS-alkalmazásokhoz az Azure AD által támogatott](active-directory-saas-app-provisioning.md).


## <a name="planning-your-solution"></a>A megoldás tervezése

A Workday-integrációs megkezdése előtt ellenőrizze az alábbi és olvasási hello következő útmutatás hello Előfeltételek az aktuális Active Directory-architektúra és a felhasználói és a kiépítés hello Azure Active által biztosított solution(s) toomatch Könyvtár.

### <a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:

* Egy érvényes Azure AD Premium P1 előfizetés globális rendszergazdai jogosultságokkal
* A Workday megvalósítási bérlő tesztelése és integrációs célokra
* Workday toocreate a rendszer integrációs felhasználó rendszergazdai jogosultságokkal, és ellenőrizze módosítások tootest alkalmazott adatok tesztelési célra
* A felhasználók tooActive Directory átadásához, 2012 vagy újabb Windows-szolgáltatást futtató, tartományhoz csatlakoztatott kiszolgálóra szükség toohost hello [helyszíni szinkronizálási ügynök](https://go.microsoft.com/fwlink/?linkid=847801)
* [Az Azure AD Connect](connect/active-directory-aadconnect.md) szinkronizálásához Active Directory és az Azure AD között

> [!NOTE]
> Ha az Azure AD-bérlő található Európa, tekintse át hello [ismert problémák](#known-issues) az alábbi szakasz.


### <a name="solution-architecture"></a>Megoldás architektúrája

Az Azure AD széles választéka segítségével megoldani, telepítését és az identitások életciklus-felügyeletének a Workday tooActive Directory, az Azure AD SaaS-alkalmazásokhoz, vagy azon kívül összekötők toohelp kiépítés biztosít. Milyen funkciókkal fogja használni, és a szervezet környezet és hogyan lehet beállítani hello megoldás függenek. Első lépésként hány hello következő jelen, és a szervezetben telepített készlete érvénybe:

* Hány Active Directory-erdők használatban vannak?
* Hány Active Directory-tartományok használatban vannak?
* Hány Active Directory szervezeti egységben (OU) használatban vannak?
* Használatban vannak, hogy hány Azure Active Directory-bérlő?
* Vannak-e felhasználók, akik kiépített toobe tooboth Active Directory és az Azure Active Directory (pl. "hibrid" felhasználók)?
* Vannak-e felhasználók, akik kiépített toobe tooAzure Active Directory, de nem Active Directory (pl. "csak felhőalapú" felhasználók)?
* Szükség van visszaírását tooWorkday toobe felhasználó e-mail címéhez?

Válaszok toothese kérdése van, ha a központi telepítés alábbi hello útmutató az alábbi általi kiosztás Workday is tervezhet.

#### <a name="using-provisioning-connector-apps"></a>Üzembe helyezési összekötő-alkalmazások használata

Az Azure Active Directory előzetesen beépített létesítési összekötők támogatja a Workday és sok más SaaS-alkalmazásokhoz. 

Üzembe helyezési egyetlen összekötőt kapcsolódási pontok hello API egyetlen forrásai rendszert, és lehetővé teszi a kiépítési adatokat tooa egyetlen célrendszeren. A legtöbb létesítési összekötők, amely támogatja az Azure AD a egyetlen forrás és cél rendszer (pl. az Azure AD tooServiceNow), majd származhatnak telepítő hello alkalmazás felvételével a szóban forgó hello Azure AD-alkalmazásgyűjtemény (pl. ServiceNow). 

Üzembe helyezési connector-példány és az app-példányok közötti-az-egyhez kapcsolat áll fenn az Azure ad-ben:

| A forráskiszolgálón rendszer | Célrendszer |
| ---------- | ---------- | 
| Az Azure AD-bérlő | SaaS-alkalmazáshoz |


Munkanapok és Active Directory használata, ha van azonban több forrás és cél rendszerek toobe figyelembe venni:

| A forráskiszolgálón rendszer | Célrendszer | Megjegyzések |
| ---------- | ---------- | ---------- |
| Munkanapok | Active Directory-erdőben | Minden olyan erdőben, a rendszer egy különálló célrendszerben |
| Munkanapok | Az Azure AD-bérlő | A felhasználó esetében kötelező, csak felhőalapú |
| Active Directory-erdőben | Az Azure AD-bérlő | Ez a folyamat végzi el az AAD-csatlakozás még ma |
| Az Azure AD-bérlő | Munkanapok | Az e-mail címek visszaírásához. |

toofacilitate ezek több munkafolyamatok toomultiple forrása és célja a rendszerek, az Azure AD biztosít több üzembe helyezési összekötő alkalmazások hello Azure AD-alkalmazásgyűjtemény is hozzáadhat:

![Az AAD-Alkalmazásgyűjtemény](./media/active-directory-saas-workday-inbound-tutorial/WD_Gallery.PNG)

* **Directory kiépítés WORKDAY tooActive** – az alkalmazás lehetővé teszi felhasználói fiók kiépítése a Workday tooa egyetlen Active Directory-erdőben. Ha több erdővel rendelkezik, az alkalmazás egy példánya hello Azure AD-alkalmazásgyűjtemény minden Active Directory-erdőben való tooprovision van szüksége a is hozzáadhat.

* **WORKDAY tooAzure AD kiépítési** -közben AAD-csatlakozás hello eszközt, hogy által használt toosynchronize Active Directory felhasználók tooAzure Active Directory, az alkalmazás lehet használt toofacilitate csak felhőalapú felhasználók Workday tooa egyetlen kiépítése Az Azure Active Directory-bérlő.

* **WORKDAY visszaírási** – az alkalmazás lehetővé teszi a felhasználó e-mail címét az Azure Active Directory tooWorkday visszaírása.

> [!TIP]
> hello rendszeres "Workday" alkalmazás beállítása az egyszeri bejelentkezés Workday és az Azure Active Directory közötti szolgál. 

Hogyan tooset össze, és speciális konfigurálásukkal összekötő alkalmazások ebben az oktatóanyagban szakasza fennmaradó hello hello tárgya. Mely alkalmazásokat úgy dönt, hogy a tooconfigure függ, hogy melyik rendszereken kell tooprovision, és hogy hány Active Directory-erdők és az Azure AD bérlők vannak a környezetében.

![Áttekintés](./media/active-directory-saas-workday-inbound-tutorial/WD_Overview.PNG)

## <a name="configure-a-system-integration-user-in-workday"></a>A rendszer integrációs felhasználó Workday konfigurálása
Üzembe helyezési összekötők összes hello Workday közös követelményt, ezek a hitelesítő adatok megkövetelése a Workday rendszer integrációs fiók tooconnect toohello Workday emberi erőforrások API. Ez a szakasz ismerteti, hogyan a rendszer integráló toocreate Workday fiókként.

> [!NOTE]
> Lehetséges toobypass ezt az eljárást, és inkább rendszerfiókként hello integráció a Workday globális rendszergazdai fiókját. Előfordulhat, hogy jól működnek az bemutatók, de az üzemi környezetek nem ajánlott.

### <a name="create-an-integration-system-user"></a>Integrációs rendszer felhasználó létrehozása

**toocreate integrációs rendszer felhasználó:**

1. Jelentkezzen be arra a Workday-bérlői rendszergazdai fiók használatával. A hello **Workday munkaterület**, adja meg létrehozni a felhasználót hello keresési mezőbe, és kattintson a **integrációs rendszer felhasználó létrehozása**. 
   
    ![A felhasználó létrehozása](./media/active-directory-saas-workday-inbound-tutorial/IC750979.png "a felhasználó létrehozása")
2. Teljes hello **integrációs rendszer felhasználó létrehozása** feladatot úgy, hogy megadja a felhasználónevet és jelszót egy új integrációs rendszer felhasználó.  
 * Hagyja hello **új jelszó szükséges a következő bejelentkezési a** beállítás nincs bejelölve, mert a felhasználó fog naplózása programozott módon. 
 * Hagyja hello **munkamenet időkorlátja percben** az alapértelmezett érték 0, amely megakadályozza, hogy hello felhasználói munkamenet időtúllépés miatt idő előtt. 
   
    ![Hozzon létre rendszer-felhasználói integrációs](./media/active-directory-saas-workday-inbound-tutorial/IC750980.png "integrációs rendszer felhasználó létrehozása")

### <a name="create-a-security-group"></a>Biztonsági csoport létrehozása
Egy korlátozás nélküli integrációs rendszerbiztonsági csoport toocreate kell, és rendelje hozzá a hello felhasználói tooit.

**a biztonsági csoport toocreate:**

1. Adjon meg biztonsági csoport létrehozása hello keresési mezőbe, és kattintson a **biztonsági csoport létrehozása**. 
   
    ![CreateSecurity csoport](./media/active-directory-saas-workday-inbound-tutorial/IC750981.png "CreateSecurity csoport")
2. Teljes hello **biztonsági csoport létrehozása** feladat.  
3. Integráció rendszerbiztonsági csoport kiválasztása – nem korlátozott a hello **típus a központjaként biztonsági csoport** legördülő menüből.
4. Hozzon létre egy biztonsági csoport toowhich tagokat a rendszer explicit módon hozzáadja. 
   
    ![CreateSecurity csoport](./media/active-directory-saas-workday-inbound-tutorial/IC750982.png "CreateSecurity csoport")

### <a name="assign-hello-integration-system-user-toohello-security-group"></a>Hello integrációs felhasználói toohello rendszerbiztonsági csoport hozzárendelése

**tooassign hello integrációs rendszer felhasználó:**

1. Adja meg a biztonsági csoport szerkesztése hello keresési mezőbe, és kattintson **biztonsági csoport szerkesztése**. 
   
    ![Biztonsági csoport szerkesztése](./media/active-directory-saas-workday-inbound-tutorial/IC750983.png "biztonsági csoport szerkesztése")
2. Keresse meg, és válassza ki a hello új integrációs biztonsági csoport neve. 
   
    ![Biztonsági csoport szerkesztése](./media/active-directory-saas-workday-inbound-tutorial/IC750984.png "biztonsági csoport szerkesztése")
3. Hello új integrációs rendszer felhasználói toohello új biztonsági csoport hozzáadása. 
   
    ![Rendszerbiztonsági csoport](./media/active-directory-saas-workday-inbound-tutorial/IC750985.png "rendszerbiztonsági csoport")  

### <a name="configure-security-group-options"></a>Biztonsági csoport beállítások konfigurálása
Ebben a lépésben engedélyt kell adnia toohello új biztonsági csoport a **beolvasása** és **Put** műveleteket a következő tartomány biztonsági házirendek hello által biztonságossá tett hello objektumok:

* Külső fiók
* Adatok: Nyilvános dolgozó jelentések
* Adatok: Minden helyzetben
* Adatok: Aktuális személyzeti információk
* Adatok: Üzleti cím munkavégző profil

**tooconfigure biztonsági csoport beállításai:**

1. Adja meg a tartomány biztonsági házirendek hello keresési mezőbe, és kattintson a hivatkozásra hello **tartományi biztonsági házirendek funkcionális terület**.  
   
    ![Tartományi biztonsági házirendek](./media/active-directory-saas-workday-inbound-tutorial/IC750986.png "tartományi biztonsági házirendek")  
2. Keresse meg a rendszer, és jelölje be hello **rendszer** funkcionális területét.  Kattintson az **OK** gombra.  
   
    ![Tartományi biztonsági házirendek](./media/active-directory-saas-workday-inbound-tutorial/IC750987.png "tartományi biztonsági házirendek")  
3. Biztonsági házirendeket a rendszer funkcionális területen hello hello listájában bontsa ki **biztonsági felügyelet** válassza ki a hello tartomány biztonsági házirendje **külső fiók létrehozását**.  
   
    ![Tartományi biztonsági házirendek](./media/active-directory-saas-workday-inbound-tutorial/IC750988.png "tartományi biztonsági házirendek")  
4. Kattintson a **engedélyek módosítása**, majd a hello **engedélyek módosítása**párbeszédpanel lapján hello új biztonsági csoport toohello listát vesznek fel a biztonsági csoportok **beolvasása** és **Put** integrációs engedélyeket. 
   
    ![Engedélyek szerkesztése](./media/active-directory-saas-workday-inbound-tutorial/IC750989.png "engedélyek szerkesztése")  
5. Ismételje meg a működési területek kiválasztásának tooreturn toohello képernyője felett 1. lépés:, és ezúttal, keressen a személyzettel, válassza ki a hello **funkcionális területen személyzeti** kattintson **OK**.
   
    ![Tartományi biztonsági házirendek](./media/active-directory-saas-workday-inbound-tutorial/IC750990.png "tartományi biztonsági házirendek")  
6. Biztonsági házirendek hello személyzeti funkcionális területen hello listájában bontsa ki **adatok: személyzeti** és minden egyes biztonsági házirendek fennmaradó megismétli a fenti 4:

   * Adatok: Nyilvános dolgozó jelentések
   * Adatok: Minden helyzetben
   * Adatok: Aktuális személyzeti információk
   * Adatok: Üzleti cím munkavégző profil
   
7. Ismételje meg az 1, tooreturn toohello képernyője kiválasztásával működési területek felett, és ezúttal keresve **elérhetőségi adatait**, hello személyzeti funkcionális területen válassza ki, és kattintson a **OK**.

8.  Hello hello személyzeti funkcionális területen biztonsági házirendek listájában bontsa ki **adatok: munkahelyi elérhetőségi adatait**, és ismételje meg a fenti 4 hello biztonsági házirendek az alábbi:

    * Adatok: A munkahelyi E-mail

    ![Tartományi biztonsági házirendek](./media/active-directory-saas-workday-inbound-tutorial/IC750991.png "tartományi biztonsági házirendek")  
    
### <a name="activate-security-policy-changes"></a>Aktiválja a biztonsági házirend módosításai

**tooactivate biztonságiszabályzat-változásokat:**

1. Adja meg aktiválása hello keresési mezőbe, és kattintson a hivatkozásra hello **aktiválása függő biztonsági házirend módosításának**. 
   
    ![Aktiválása](./media/active-directory-saas-workday-inbound-tutorial/IC750992.png "aktiválása") 
2. Függőben lévő biztonsági módosítások aktiválása adjon meg egy megjegyzést a naplózási célokra feladat, és kattintson a kezdő hello **OK**. 
   
    ![Függőben lévő biztonsági aktiválása](./media/active-directory-saas-workday-inbound-tutorial/IC750993.png "függőben lévő biztonsági aktiválása")   
3. A következő képernyőn hello hello jelölőnégyzet bejelölésével teljes hello feladat **megerősítése**, és kattintson a **OK**. 
   
    ![Függőben lévő biztonsági aktiválása](./media/active-directory-saas-workday-inbound-tutorial/IC750994.png "függőben lévő biztonsági aktiválása")  

## <a name="configuring-user-provisioning-from-workday-tooactive-directory"></a>A felhasználók átadása a Workday tooActive Directory konfigurálása
Hajtsa végre az ezen utasításokat tooconfigure felhasználói fiók történő átadása Workday tooeach történő igénylő Active Directory-erdőben.

### <a name="part-1-adding-hello-provisioning-connector-app-and-creating-hello-connection-tooworkday"></a>1. rész: Hello létesítési összekötő alkalmazás hozzáadása és hello kapcsolat tooWorkday létrehozása

**tooconfigure Workday tooActive Directory kiépítés:**

1.  Nyissa meg túl<https://portal.azure.com>

2.  Hello bal oldali navigációs sávon, válassza ki a **Azure Active Directoryban**

3.  Válassza ki **vállalati alkalmazások**, majd **összes alkalmazás**.

4.  Válassza ki **alkalmazás hozzáadása**, és jelölje be hello **összes** kategóriát.

5.  Keresse meg **Workday kiépítés tooActive Directory**, és adja hozzá az alkalmazás hello gyűjteményből.

6.  Alkalmazás hozzáadása után hello és hello alkalmazás részletei képernyőn megjelenik, jelölje be **kiépítés**

7.  Változás hello **kiépítési** **mód** túl**automatikus**

8.  Teljes hello **rendszergazdai hitelesítő adataival** szakasz az alábbiak szerint:

   * **Rendszergazda felhasználóneve** – adja meg a hello Workday-integrációs rendszerfiók, hello felhasználóneve hello bérlői tartománynévvel lesz hozzáfűzve. **Hasonlóan kell kinéznie:username@contoso4**

   * **Rendszergazdai jelszó –** hello jelszó megadni a Workday-integrációs rendszerfiók hello

   * **A bérlői URL-cím –** hello URL-cím toohello Workday web services végpontjánál megadása a bérlő. Hasonlóan kell kinéznie: https://wd3-impl-services1.workday.com/ccx/service/contoso4, ahol contoso4 helyére a megfelelő bérlő neve és wd3-impl hello megfelelő környezet karakterlánc helyére.

   * **Az Active Directory Erdőfelderítési -** az Active Directory-erdőben, a "Name" hello, amelyet a Get-ADForest hello powershell-parancsmag segítségével. Ez általában egy olyan karakterlánc, például: *contoso.com*

   * **Active Directory-tároló -** megadása hello tároló, amely tartalmazza az összes felhasználót az Active Directory-erdőben. Példa: *OU általános jogú felhasználók, OU = = Users, DC = contoso, DC = test*

   * **E-mailben értesítést –** meg e-mail címét, és jelölje be az "e-mail küldési hiba esetén" jelölőnégyzetet.

   * Kattintson a hello **kapcsolat tesztelése** gombra. Ha hello kapcsolat ellenőrzése sikeres, kattintson a hello **mentése** hello felső gombra. Ha nem sikerül, ellenőrizze, hogy hello Workday hitelesítő adatok érvényesek-e a Workday. 

![Azure Portal](./media/active-directory-saas-workday-inbound-tutorial/WD_1.PNG)

### <a name="part-2-configure-attribute-mappings"></a>2. lépés: Konfigurálja a attribútum-leképezésekhez 

Ebben a szakaszban konfigurál, hogy felhasználói adatáramlás a WORKDAY-ből az Active Directory.

1.  A hello kiépítés lapon a **hozzárendelések**, kattintson a **szinkronizálása Workday munkavállalók tooOnPremises**.

2.  A hello **forrás objektum hatóköre** mezőjét, kiválaszthatja, mely a Workday felhasználócsoportokhoz tooAD, kialakítási Attribútumalapú szűrők csoportja meghatározásával hatókörében kell lennie. hello alapértelmezett hatóköre "minden felhasználó a Workday". Példa szűrők:

   * Példa: Hatókör toousers Worker-azonosítók 1000000 és 2000000 között

      * Attribútum: WorkerID

      * Operátor: Reguláris KIFEJEZÉSSEL egyező

      * Érték: (1[0-9][0-9][0-9][0-9][0-9][0-9])

   * Példa: Csak az alkalmazottak és a nem függő munkavállalók 

      * Attribútum: EmployeeID

      * Operátort: Nem NULL

3.  A hello **cél objektum műveletek** mezőjét, globálisan végezhet milyen műveletek végre Active Directory toobe engedélyezettek. **Hozzon létre** és **frissítés** leggyakoribb.

4.  A hello **attribútum-hozzárendelések** szakaszban attribútumok hozzárendelését tooActive könyvtárattribútumokat egyes Workday definiálhat.

5. Kattintson a egy meglévő attribútum leképezési tooupdate vagy **új leképezés hozzáadása** hello képernyő tooadd új leképezéseket hello alján. Az egyes címtárattribútum-leképezésben támogatja ezeket a tulajdonságokat:

      * **Leképezés típusa**

         * **Közvetlen** – hello hello Workday attribútum toohello AD attribútum értékének, módosítások nélküli írási műveletek

         * **Állandó** -statikus, állandó karakterlánc-érték írni hello AD attribútum

         * **Kifejezés** – lehetővé teszi a toowrite egy egyéni értéket hello AD attribútum egy vagy több Workday-attribútumok alapján. [További információk: Ez a cikk a kifejezések](active-directory-saas-writing-expressions-for-attribute-mappings.md).

      * **Adatforrás-attribútum** -hello felhasználói attribútum a WORKDAY-ből.

      * **Alapértelmezett érték** – nem kötelező. Hello adatforrás-attribútum értéke üres, ha hello leképezési fog kiírni, ez az érték helyett.
            Leggyakoribb beállítás tooleave ebben üres.

      * **Cél attribútumának** – hello felhasználói attribútum az Active Directoryban.

      * **Egyezik ezzel az attribútummal objektumok** – függetlenül attól, ez a leképezés használandó toouniquely azonosítsa azokat a felhasználókat a Workday és az Active Directory között. Ez általában be van állítva az munkavégző Azonosítót tartalmazó mezőt a Workday, amely általában van hozzárendelve egy hello Alkalmazottazonosító attribútumok az Active Directoryban.

      * **Megfelelő sorrend** – több egyező attribútumok állítható be. Ha több, kiértékelésük, ez a mező által megadott sorrendben. Amint a program egyezést talál, további megfelelő attribútumok kiértékelése.

      * **Ez a leképezés alkalmazása**
       
         * **Mindig** – Ez a leképezés alkalmazni mind a felhasználó létrehozása és a műveletek frissítése

         * **Csak létrehozásakor** – Ez a leképezés csak a felhasználó létrehozási műveletek alkalmazása

6. toosave a leképezései kattintson **mentése** attribútum leképezési szakasz hello tetején.

![Azure Portal](./media/active-directory-saas-workday-inbound-tutorial/WD_2.PNG)

**Az alábbiakban néhány példa néhány gyakori kifejezésekkel a Workday és az Active Directory közötti attribútum-leképezésekhez**

-   hello kifejezés, amely leképezhető toohello parentDistinguishedName AD attribútum használt tooprovision egy adott szervezeti egység alapján egy vagy több Workday forrás attribútum felhasználói tooa lehet. Ebben a példában a város adataikat függően különböző szervezeti felhasználók helyezi a Workday.

-   hello kifejezés, amely leképezhető toohello userPrincipalName AD attribútum hozzon létre egy egyszerű firstName.LastName@contoso.com. A váltja fel érvénytelen különleges karaktereket is.

-   [Nincs a dokumentáció itt kifejezések írása](active-directory-saas-writing-expressions-for-attribute-mappings.md)

  
| WORKDAY ATTRIBÚTUM | AZ ACTIVE DIRECTORY-ATTRIBÚTUM |  EGYEZŐ AZONOSÍTÓ? | LÉTREHOZÁSA / FRISSÍTÉSE |
| ---------- | ---------- | ---------- | ---------- |
|  **WorkerID**  |  EmployeeID | **Igen** | Írt csak létrehozásakor | 
|  **Település**   |   l csomag   |     | Hozzon létre + frissítése |
|  **Vállalati**         | Vállalati   |     |  Hozzon létre + frissítése |
|  **CountryReferenceTwoLetter**      |   CO |     |   Hozzon létre + frissítése |
| **CountryReferenceTwoLetter**    |  C  |     |         Hozzon létre + frissítése |
| **SupervisoryOrganization**  | Szervezeti egység  |     |  Hozzon létre + frissítése |
|  **PreferredNameData**  |  displayName |     |   Hozzon létre + frissítése |
| **EmployeeID**    |  CN    |   |   Írt csak létrehozásakor |
| **Faxkiszolgáló**      | facsimileTelephoneNumber     |     |    Hozzon létre + frissítése |
| **Utónév**   | givenName       |     |    Hozzon létre + frissítése |
| **Kapcsoló (\[aktív\],, "0", "True", "1")** |  AccountDisabled      |     | Hozzon létre + frissítése |
| **Mobileszköz**  |    Mobileszköz       |     |       Írt csak létrehozásakor |
| **E-mail cím**    | mail    |     |     Hozzon létre + frissítése |
| **ManagerReference**   | Manager  |     |  Hozzon létre + frissítése |
| **WorkSpaceReference** | physicalDeliveryOfficeName    |     |  Hozzon létre + frissítése |
| **Irányítószám**  |   Irányítószám  |     | Hozzon létre + frissítése |
| **LocalReference** |  preferredLanguage  |     |  Hozzon létre + frissítése |
| ** Cseréje (Left (cseréje (\[EmployeeID\],, "(\[\\\\/\\\\\\\\\\\\\[\\\\\]\\\\:\\\\;\\\\|\\\\=\\\\,\\\\+\\\\\*\\\\?\\ \\ &lt; \\ \\ &gt; \]) "," ",), 1, 20)," ([\\\\.) \* \$] (file:///\\.) *$)", , "", , )**      |    sAMAccountName            |     |         Írt csak létrehozásakor |
| **Vezetéknév**   |   sorozatszám   |     |  Hozzon létre + frissítése |
| **CountryRegionReference** |  St     |     | Hozzon létre + frissítése |
| **AddressLineData**    |  StreetAddress  |     |   Hozzon létre + frissítése |
| **PrimaryWorkTelephone**  |  TelephoneNumber   |     | Írt csak létrehozásakor |
| **BusinessTitle**   |  Cím     |     |  Hozzon létre + frissítése |
| **Join("@",Replace(Replace(Replace(Replace(Replace(Replace(Replace( Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace( Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Join(".", [FirstName], [LastName]), , "([Øø])", , "oe", , ), , "[Ææ]", , "ae", , ), , "([äãàâãåáąÄÃÀÂÃÅÁĄA])", , "a", , ), , "([B])", , "b", , ), , "([CçčćÇČĆ])", , "c", , ), , "([ďĎD])", , "d", , ), , "([ëèéêęěËÈÉÊĘĚE])", , "e", , ), , "([F])", , "f", , ), , "([G])", , "g", , ), , "([H])", , "h", , ), , "([ïîìíÏÎÌÍI])", , "i", , ), , "([J])", , "j", , ), , "([K])", , "k", , ), , "([ľłŁĽL])", , "l", , ), , "([M])" ,, "m",), "([ñńňÑŃŇN])", "n",), "([öòőõôóÖÒŐÕÔÓO])", "o",), "([P])", "p",), "([Q])", "q",), "([řŘR])", "r",), "([ßšśŠŚS])", "s",), "([TŤť])", "t",), "([üùûúůűÜÙÛÚŮŰU])", "u",), "([V])", "v",), "([w" karakter]), "w",), "([ýÿýŸÝY])", "y",), "([źžżŹŽŻZ])", "z",), "",,, "",), "contoso.com")**   | UserPrincipalName     |     | Hozzon létre + frissítése                                                   
| **Kapcsoló (\[település\], "OU általános jogú felhasználók, OU = felhasználók, OU = alapértelmezett, OU = helyek, DC = contoso, DC = = com", "Dallas", "OU általános jogú felhasználók, OU = felhasználók, OU = Dallas, OU = helyek, DC = contoso, DC = = com", "Austin", "OU általános jogú felhasználók, OU = felhasználók, OU = Austin, OU = helyek, DC = contoso, DC = = com", "Seattle", "OU általános jogú felhasználók, OU = felhasználók, OU = budapesti, OU = helyek, DC = contoso, DC = = com", "Londoni", "OU = általános jogú felhasználók Szervezeti egység felhasználók, OU = London, OU = helyek, DC = contoso, DC = = com ")**  | parentDistinguishedName     |     |  Hozzon létre + frissítése |
  
### <a name="part-3-configure-hello-on-premises-synchronization-agent"></a>3. rész: Hello helyszíni szinkronizálási ügynök konfigurálása

A sorrend tooprovision tooActive címtár a helyszínen az ügynök hello desire Active Directory-erdőt egy tartományhoz csatlakozó kiszolgálón telepítenie kell. Hitelesítő adatok szükségesek a hello eljárással tartományi rendszergazda (vagy vállalati rendszergazdai).

**[Letöltheti a hello helyszíni szinkronizálási ügynök Itt](https://go.microsoft.com/fwlink/?linkid=847801)**

Ügynök telepítése után futtassa az alábbi tooconfigure hello ügynök a környezetnek hello Powershell parancsokat.

**A parancs #1**

> CD C:\\programfájlok\\Microsoft Azure Active Directory szinkronizálási ügynök\\modulok\\AADSyncAgent

> import-module AADSyncAgent.psd1

**A parancs #2**

> Adja hozzá ADSyncAgentActiveDirectoryConfiguration

* Bemenet: "Könyvtárnév", adja meg hello AD-erdő neve, részben megadott \#2
* Bemenet: Rendszergazda felhasználónevét és jelszavát, az Active Directory-erdő

**A parancs #3**

> Adja hozzá ADSyncAgentAzureActiveDirectoryConfiguration

* Bemenet: Globális rendszergazda felhasználónevét és jelszavát az Azure AD-bérlő

**A parancs #4**

> Get-AdSyncAgentProvisioningTasks

* Művelet: Ellenőrizze, hogy adatokat ad vissza. Ez a parancs automatikusan észleli a Workday kiépítés alkalmazások az Azure AD-bérlőben. Példa a kimenetre:

> Name: Az Active Directory-erdőben
>
> Engedélyezve: igaz
>
> Könyvtárnév: mydomain.contoso.com
>
> Credentialed: hamis
>
> Azonosító: WDAYdnAppDelta.c2ef8d247a61499ba8af0a29208fb853.4725aa7b-1103-41e6-8929-75a5471a5203

**A parancs #5**

> Start-AdSyncAgentSynchronization-automatikus

**A parancs #6**

> net stop aadsyncagent

**A parancs #7**

> a net start aadsyncagent

### <a name="part-4-start-hello-service"></a>4. lépés: Start hello szolgáltatás
1-3 részből befejezése után újra hello Azure felügyeleti portálon a szolgáltatás kiépítését hello is elindítható.

1.  A hello **kiépítési** lap, a set hello **kiépítési állapot** való **a**.

2. Kattintson a **Save** (Mentés) gombra.

3. Hello kezdeti szinkronizálás, ami eltarthat, attól függően, hogy hány felhasználó szerepelnek a Workday óra változó számú elindítja.

4. Egyes szinkronizálási események például a felhasználók Workday kívül olvas, és később hozzáadott vagy frissített tooActive Directory, majd lehet megtekinteni a hello **naplók** fülre. **[Lásd: hello kiépítés részletes információkra van szüksége a hogyan tooread hello auditnaplókat jelentéskészítési útmutató](active-directory-saas-provisioning-reporting.md)**

5.  hello Windows alkalmazásnaplót a hello ügynökszámítógépet hello ügynök keresztül végrehajtani minden műveletnél jeleníti meg.

6. Fejeződött be, akkor fog kiírni, összefoglaló jelentést a **kiépítési** lapon, a lent látható módon.

![Azure Portal](./media/active-directory-saas-workday-inbound-tutorial/WD_3.PNG)


## <a name="configuring-user-provisioning-tooazure-active-directory"></a>Felhasználók átadásához tooAzure Active Directory konfigurálása
Hogyan konfigurálja az Active Directory létesítési tooAzure hello az alábbi táblázat a kiépítési követelményektől függ.

| Forgatókönyv | Megoldás |
| -------- | -------- |
| **Kiépített toobe tooActive Directory és az Azure AD a felhasználóknak kell** | Használjon  **[AAD-csatlakozás](connect/active-directory-aadconnect.md)** |
| **Felhasználók kell kiépített toobe tooActive Directory csak** | Használjon  **[AAD-csatlakozás](connect/active-directory-aadconnect.md)** |
| **A felhasználóknak kell kiépített toobe tooAzure AD csak (csak felhő)** | Használjon hello **Workday tooAzure Active Directory-kiépítés** hello alkalmazásgyűjtemény alkalmazás |

Az Azure AD Connect beállításával kapcsolatos útmutatásért lásd: hello [Azure AD Connect dokumentációját](connect/active-directory-aadconnect.md).

a következő szakaszok hello tooprovision csak felhőalapú felhasználók Workday és az Azure AD közötti kapcsolat beállításának ismertetik.

> [!IMPORTANT]
> Csak akkor hajtsa végre hello kövesse az alábbi eljárást, ha kizárólag felhőalapú felhasználót kell kiépített toobe tooAzure AD, és nem a helyszíni Active Directory.

### <a name="part-1-adding-hello-azure-ad-provisioning-connector-app-and-creating-hello-connection-tooworkday"></a>1. rész: Hello Azure AD-létesítési összekötő alkalmazás hozzáadása és hello kapcsolat tooWorkday létrehozása

**tooconfigure Workday tooAzure Active Directory-kiépítés csak felhőalapú felhasználók számára:**

1.  Nyissa meg túl<https://portal.azure.com>.

2.  Hello bal oldali navigációs sávon, válassza ki a **Azure Active Directoryban**

3.  Válassza ki **vállalati alkalmazások**, majd **összes alkalmazás**.

4.  Válassza ki **alkalmazás hozzáadása**, majd válassza ki a hello **összes** kategóriát.

5.  Keresse meg **Workday tooAzure AD kiépítés**, és adja hozzá az alkalmazás hello gyűjteményből.

6.  Alkalmazás hozzáadása után hello és hello alkalmazás részletei képernyőn megjelenik, jelölje be **kiépítés**

7.  Változás hello **kiépítési** **mód** túl**automatikus**

8.  Teljes hello **rendszergazdai hitelesítő adataival** szakasz az alábbiak szerint:

   * **Rendszergazda felhasználóneve** – adja meg a hello Workday-integrációs rendszerfiók, hello felhasználóneve hello bérlői tartománynévvel lesz hozzáfűzve. Hasonlóan kell kinéznie:username@contoso4

   * **Rendszergazdai jelszó –** hello jelszó megadni a Workday-integrációs rendszerfiók hello

   * **A bérlői URL-cím –** hello URL-cím toohello Workday web services végpontjánál megadása a bérlő. Hasonlóan kell kinéznie: https://wd3-impl-services1.workday.com/ccx/service/contoso4, ahol contoso4 helyére a megfelelő bérlő neve és wd3-impl cseréli hello megfelelő környezet karakterlánc (ha szükséges).

   * **E-mailben értesítést –** meg e-mail címét, és jelölje be az "e-mail küldési hiba esetén" jelölőnégyzetet.

   * Kattintson a hello **kapcsolat tesztelése** gombra.

   * Ha hello kapcsolat ellenőrzése sikeres, kattintson a hello **mentése** hello felső gombra. Ha nem sikerül, ellenőrizze, hogy hello Workday URL-címet, és hitelesítő adatok érvényesek a Workday.


### <a name="part-2-configure-attribute-mappings"></a>2. lépés: Konfigurálja a attribútum-leképezésekhez 

Ebben a szakaszban konfigurál, hogyan felhasználói adatáramlás a WORKDAY-ből az Azure Active Directory csak felhőalapú felhasználók számára.

1.  A hello kiépítés lapon a **hozzárendelések**, kattintson a **szinkronizálása munkavállalók tooAzure AD**.

2.   A hello **forrás objektum hatóköre** mezőjét, kiválaszthatja, mely a Workday felhasználócsoportokhoz tooAzure AD, kialakítási Attribútumalapú szűrők csoportja meghatározásával hatókörében kell lennie. hello alapértelmezett hatóköre "minden felhasználó a Workday". Példa szűrők:

   * Példa: Hatókör toousers Worker-azonosítók 1000000 és 2000000 között

      * Attribútum: WorkerID

      * Operátor: Reguláris KIFEJEZÉSSEL egyező

      * Érték: (1[0-9][0-9][0-9][0-9][0-9][0-9])

   * Példa: Csak függő dolgozó munkatársak és nem rendszeres alkalmazottak

      * Attribútum: ContingentID

      * Operátort: Nem NULL

3.  A hello **cél objektum műveletek** globálisan végezhet milyen műveletek használata engedélyezett az Azure AD végre toobe mezőjét. **Hozzon létre** és **frissítés** leggyakoribb.

4.  A hello **attribútum-hozzárendelések** szakaszban attribútumok hozzárendelését tooActive könyvtárattribútumokat egyes Workday definiálhat.

5. Kattintson a egy meglévő attribútum leképezési tooupdate vagy **új leképezés hozzáadása** hello képernyő tooadd új leképezéseket hello alján. Az egyes címtárattribútum-leképezésben támogatja ezeket a tulajdonságokat:

   * **Leképezés típusa**

      * **Közvetlen** – hello hello Workday attribútum toohello AD attribútum értékének, módosítások nélküli írási műveletek

      * **Állandó** -statikus, állandó karakterlánc-érték írni hello AD attribútum

      * **Kifejezés** – lehetővé teszi a toowrite egy egyéni értéket hello AD attribútum egy vagy több Workday-attribútumok alapján. [További információk: Ez a cikk a kifejezések](active-directory-saas-writing-expressions-for-attribute-mappings.md).

   * **Adatforrás-attribútum** -hello felhasználói attribútum a WORKDAY-ből.

   * **Alapértelmezett érték** – nem kötelező. Hello adatforrás-attribútum értéke üres, ha hello leképezési fog kiírni, ez az érték helyett.
            Leggyakoribb beállítás tooleave ebben üres.

   * **Cél attribútumának** – hello felhasználói attribútum az Azure ad-ben.

   * **Egyezik ezzel az attribútummal objektumok** – függetlenül attól, ez a leképezés használandó toouniquely azonosítsa azokat a felhasználókat a Workday és az Azure AD között. Ez általában be van állítva az munkavégző Azonosítót tartalmazó mezőt a Workday, amely általában hozzárendelve hello alkalmazott ID attribútum (új), vagy a bővítmény attribútum az Azure ad-ben.

   * **Megfelelő sorrend** – több egyező attribútumok állítható be. Ha több, kiértékelésük, ez a mező által megadott sorrendben. Amint a program egyezést talál, további megfelelő attribútumok kiértékelése.

   * **Ez a leképezés alkalmazása**

     * **Mindig** – Ez a leképezés alkalmazni mind a felhasználó létrehozása és a műveletek frissítése

     * **Csak létrehozásakor** – Ez a leképezés csak a felhasználó létrehozási műveletek alkalmazása

6. toosave a leképezései kattintson **mentése** attribútum leképezési szakasz hello tetején.

### <a name="part-3-start-hello-service"></a>3. rész: Start hello szolgáltatás
1-2 részek elvégzése után, megkezdheti a hello szolgáltatás kiépítését.

1.  A hello **kiépítési** lap, a set hello **kiépítési állapot** való **a**.

2. Kattintson a **Save** (Mentés) gombra.

3. Hello kezdeti szinkronizálás, ami eltarthat, attól függően, hogy hány felhasználó szerepelnek a Workday óra változó számú elindítja.

4. Egyes szinkronizálási események tekinthető hello **naplók** fülre. **[Lásd: hello kiépítés részletes információkra van szüksége a hogyan tooread hello auditnaplókat jelentéskészítési útmutató](active-directory-saas-provisioning-reporting.md)**

5. Fejeződött be, akkor fog kiírni, összefoglaló jelentést a **kiépítési** lapon, a lent látható módon.


## <a name="configuring-writeback-of-email-addresses-tooworkday"></a>Az e-mail címek tooWorkday visszaírási konfigurálásával
Hajtsa végre a felhasználó e-mail címéhez utasításokat tooconfigure visszaírása az Azure Active Directory tooWorkday.

### <a name="part-1-adding-hello-provisioning-connector-app-and-creating-hello-connection-tooworkday"></a>1. rész: Hello létesítési összekötő alkalmazás hozzáadása és hello kapcsolat tooWorkday létrehozása

**tooconfigure Workday tooActive Directory kiépítés:**

1.  Nyissa meg túl<https://portal.azure.com>

2.  Hello bal oldali navigációs sávon, válassza ki a **Azure Active Directoryban**

3.  Válassza ki **vállalati alkalmazások**, majd **összes alkalmazás**.

4.  Válassza ki **alkalmazás hozzáadása**, majd jelölje be hello **összes** kategóriát.

5.  Keresse meg **Workday visszaírási**, és adja hozzá az alkalmazás hello gyűjteményből.

6.  Alkalmazás hozzáadása után hello és hello alkalmazás részletei képernyőn megjelenik, jelölje be **kiépítés**

7.  Változás hello **kiépítési** **mód** túl**automatikus**

8.  Teljes hello **rendszergazdai hitelesítő adataival** szakasz az alábbiak szerint:

   * **Rendszergazda felhasználóneve** – adja meg a hello Workday-integrációs rendszerfiók, hello felhasználóneve hello bérlői tartománynévvel lesz hozzáfűzve. Hasonlóan kell kinéznie:username@contoso4

   * **Rendszergazdai jelszó –** hello jelszó megadni a Workday-integrációs rendszerfiók hello

   * **A bérlői URL-cím –** hello URL-cím toohello Workday web services végpontjánál megadása a bérlő. Hasonlóan kell kinéznie: https://wd3-impl-services1.workday.com/ccx/service/contoso4, ahol contoso4 helyére a megfelelő bérlő neve és wd3-impl cseréli hello megfelelő környezet karakterlánc (ha szükséges).

   * **E-mailben értesítést –** meg e-mail címét, és jelölje be az "e-mail küldési hiba esetén" jelölőnégyzetet.

   * Kattintson a hello **kapcsolat tesztelése** gombra. Ha hello kapcsolat ellenőrzése sikeres, kattintson a hello **mentése** hello felső gombra. Ha nem sikerül, ellenőrizze, hogy hello Workday URL-címet, és hitelesítő adatok érvényesek a Workday.


### <a name="part-2-configure-attribute-mappings"></a>2. lépés: Konfigurálja a attribútum-leképezésekhez 


Ebben a szakaszban konfigurál, hogy felhasználói adatáramlás a WORKDAY-ből az Active Directory.

1.  A hello kiépítés lapon a **hozzárendelések**, kattintson a **szinkronizálása Azure Active Directory-felhasználók tooWorkday**.

2.  A hello **forrás objektum hatóköre** mezőjét, igény szerint szűrheti az Azure Active Directory mely felhasználócsoportokhoz kell rendelkeznie az e-mail címmel visszaírását tooWorkday. hello alapértelmezett hatóköre "az Azure AD-minden felhasználó". 

3.  A hello **attribútum-hozzárendelések** szakaszban attribútumok hozzárendelését tooActive könyvtárattribútumokat egyes Workday definiálhat. Nincs hello e-mail cím alapértelmezés szerint a leképezéseket. Azonban az Azonosítóval egyező hello kell lennie az frissített toomatch felhasználók a Workday a megfelelő bejegyzések az Azure AD-ben. Egy népszerű megfelelő metódus toosynchronize hello Workday dolgozó azonosítója vagy alkalmazott tooextensionAttribute1 – 15 azonosító az Azure ad-ben, és használja ezt az attribútumot az Azure AD toomatch felhasználók újra a Workday.

4.  toosave a leképezései kattintson **mentése** hello attribútum leképezési szakasz hello tetején.

### <a name="part-3-start-hello-service"></a>3. rész: Start hello szolgáltatás
1-2 részek elvégzése után, megkezdheti a hello szolgáltatás kiépítését.

1.  A hello **kiépítési** lap, a set hello **kiépítési állapot** való **a**.

2. Kattintson a **Save** (Mentés) gombra.

3. Hello kezdeti szinkronizálás, ami eltarthat, attól függően, hogy hány felhasználó szerepelnek a Workday óra változó számú elindítja.

4. Egyes szinkronizálási események tekinthető hello **naplók** fülre. **[Lásd: hello kiépítés részletes információkra van szüksége a hogyan tooread hello auditnaplókat jelentéskészítési útmutató](active-directory-saas-provisioning-reporting.md)**

5. Fejeződött be, akkor fog kiírni, összefoglaló jelentést a **kiépítési** lapon, a lent látható módon.

## <a name="known-issues"></a>Ismert problémák

* **A naplók az Európai nyelv** – Ez a technical preview kiadásában hello, nem egy ismert probléma az hello [naplók](active-directory-saas-provisioning-reporting.md) hello Workday összekötő alkalmazások nem jelennek meg hello [Azure-portálon](https://portal.azure.com) Ha hello Azure AD bérlő Európai adatközpontban található. Egy javítást alkalmaztunk a probléma azonnali. Ellenőrizze, hogy ezen a helyen, újra a jövőbeli frissítések közelében hello. 

## <a name="additional-resources"></a>További források
* [Oktatóanyag: Az egyszeri bejelentkezés közötti Workday és az Azure Active Directory konfigurálása](active-directory-saas-workday-tutorial.md)
* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a>Következő lépések

* [Ismerje meg, hogy miként naplózza az tooreview és jelentések készítése a kiépítés tevékenység](https://docs.microsoft.com/azure/active-directory/active-directory-saas-provisioning-reporting)
