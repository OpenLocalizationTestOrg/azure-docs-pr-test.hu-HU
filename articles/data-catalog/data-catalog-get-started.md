---
title: "a Data Catalog használatába aaaGet |} Microsoft Docs"
description: "Végpont oktatóanyag hello forgatókönyvek és az Azure Data Catalog képességeinek."
documentationcenter: 
services: data-catalog
author: steelanddata
manager: jhubbard
editor: 
tags: 
ms.assetid: 03332872-8d84-44a0-8a78-04fd30e14b18
ms.service: data-catalog
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/03/2017
ms.author: spelluru
ms.openlocfilehash: 7652918b5a8254f5cff9e32d77b1fd3e1bf59d59
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-catalog"></a>Ismerkedés az Azure Data Catalog szolgáltatással
Az Azure Data Catalog teljes körűen felügyelt felhőszolgáltatás, amely vállalati adategységek regisztrációs és felderítőrendszereként szolgál. A szolgáltatás részletes bemutatásáért olvassa el a [Mi az az Azure Data Catalog?](data-catalog-what-is-data-catalog.md) című cikket.

Ez az oktatóanyag az Azure Data Catalog használatának megkezdésébe vezeti be a felhasználót. Ez az oktatóanyag az alábbi eljárásokat hello elvégezhető:

| Eljárás | Leírás |
|:--- |:--- |
| [A Data Catalog kiépítése](#provision-data-catalog) |Az eljárás keretében el fogja végezni az Azure Data Catalog kiépítését vagy beállítását. Ez a lépés csak akkor, ha hello katalógus nem beállítása előtt hajtsa végre. Szervezetenként (Microsoft Azure Active Directory-tartományonként) csupán egyetlen adatkatalógussal rendelkezhet, még akkor is, ha Azure-fiókjához több előfizetés is tartozik. |
| [Adategységek regisztrálása](#register-data-assets) |Ezzel az eljárással regisztrált adategységeket hello AdventureWorks2014 minta adatbázisból hello a data catalog. Regisztráció az például nevek, típusok és helyek hello adatforrás és a metaadatok toohello katalógus másolása kibontása fő szerkezeti metaadatok hello folyamatán. hello adatforrás és az adategységek maradnak. Ha vannak, de hello metaadatok hello katalógus toomake használatával könnyebben feltárhatóvá és értelmezhetővé őket. |
| [Adategységek felderítése](#discover-data-assets) |Ez az eljárás használhatja az hello Azure Data Catalog portál toodiscover adategységeket hello előző lépésben regisztrált. Egy adatforrás regisztrálva van az Azure Data Catalog szolgáltatással, miután a metaadatait indexelik hello szolgáltatást, hogy a felhasználók könnyen kereshet hello adatok van szükségük. |
| [Adategységek ellátása dekorációkkal](#annotate-data-assets) |Ezzel az eljárással, jegyzetek (például a leírásokat, címkéket, dokumentáció vagy szakértők adatait) hello az adategységeket. Ezt az információt kiegészíti hello adatforrás hello metaadatokat, és toomake hello adatforrás többen érthető toomore. |
| [Csatlakozás toodata eszközök](#connect-to-data-assets) |Ebben az eljárásban adategységeket fog megnyitni integrált ügyféleszközökkel (például az Excellel és az SQL Server Data Tools eszközzel), valamint egy nem integrált eszközzel (SQL Server Management Studio). |
| [Adategységek felügyelete](#manage-data-assets) |Ebben az eljárásban fogja elvégezni az adategységek biztonságának beállítását. A Data Catalog nem nyújt a felhasználóknak hozzáférést toohello adatokat mozgatná. hello adatforrás hello tulajdonosának adatelérési szabályozza. <br/><br/> A Data Catalog felderíthetők az adatforrások és a nézet hello **metaadatok** hello katalógusban regisztrált toohello adatforrások kapcsolatos. Előfordulhatnak olyan esetekben, azonban az adatforrások kell látható csak toospecific felhasználók vagy az adott csoportok toomembers. Ezek a forgatókönyvek regisztrált adategységeket belül hello katalógus és vezérlés hello látható-e saját hello eszközök tulajdonjogát a Data Catalog tootake is használhatja. |
| [Adategységek eltávolítása](#remove-data-assets) |Ebben az eljárásban megismerheti, hogyan tooremove adategységeket a hello a data catalog. |

## <a name="tutorial-prerequisites"></a>Az oktatóanyag előfeltételei
### <a name="azure-subscription"></a>Azure-előfizetés
tooset be Azure Data Catalog hello tulajdonosa vagy egy Azure-előfizetés társtulajdonos kell lennie.

Azure-előfizetések megkönnyítik toocloud hasonlóan az Azure Data Catalog szolgáltatás erőforrások eléréséhez. Ezenfelül az előfizetés révén azt is megszabhatja, hogy hogyan szeretne jelentést készíteni az erőforrások használatáról, hogy hogyan számlázzák ki azt Önnek, illetve, hogy hogyan szeretne fizetni érte. Az egyes előfizetésekhez eltérő számlázási és fizetési beállítások tartozhatnak, így osztályonként, projektenként, területi képviseletenként stb. különböző előfizetési és fizetési megoldásokat használhat. Minden felhőalapú szolgáltatás tooa előfizetéshez tartozik, és meg kell, hogy toohave előfizetésem az Azure Data Catalog beállítása előtt. több, lásd: toolearn [kezelhetők a fiókok, előfizetések és rendszergazdai szerepkörök](../active-directory/active-directory-how-subscriptions-associated-directory.md).

Ha nem rendelkezik előfizetéssel, mindössze néhány perc alatt létrehozhat egy ingyenes próbafiókot. A részletekért lásd: [Ingyenes próbafiók](https://azure.microsoft.com/pricing/free-trial/).

### <a name="azure-active-directory"></a>Azure Active Directory
tooset be Azure Data Catalog, be kell jelentkeznie be egy Azure Active Directory (Azure AD) felhasználói fiókkal. Hello tulajdonosa vagy egy Azure-előfizetés társtulajdonos kell lennie.  

Az Azure AD az üzleti toomanage identitás és hozzáférés, egyszerre hello felhőben és helyszíni egyszerű módszert biztosít. Is használhatja a egyetlen munkahelyi vagy iskolai fiók toosign tooany felhőben, vagy a helyszíni webalkalmazást. Az Azure Data Catalog használja az Azure AD tooauthenticate bejelentkezés. több, lásd: toolearn [Mi az Azure Active Directory](../active-directory/active-directory-whatis.md).

### <a name="azure-active-directory-policy-configuration"></a>Azure Active Directory-szabályzat konfigurálása
Olyan helyzet állhat elő, ahol bejelentkezhet toohello Azure Data Catalog-portált, de az adatforrás-regisztráló eszköz toohello toosign tett kísérlet során, olyan hibaüzenetet, amely megakadályozza, hogy aláírási észlel. Ez a hiba akkor fordulhat elő, amikor hello vállalati hálózaton, vagy ha külső hello vállalati hálózathoz csatlakozik.

hello regisztrációs eszköz használja *űrlapos hitelesítés* toovalidate felhasználói bejelentkezések Azure Active Directoryban. Sikeres bejelentkezés, az Azure Active Directory-rendszergazda engedélyeznie kell a hello az űrlapos hitelesítés *globális hitelesítési házirend*.

Hello globális hitelesítési házirend engedélyezheti külön-külön az intranetes és extranetes kapcsolatok hitelesítési hello kép a következő ábrán. Bejelentkezési hiba akkor fordulhat elő, ha a hello hálózati kapcsolat, amelyből nincs engedélyezve az űrlapos hitelesítés.

 ![Az Azure Active Directory globális hitelesítési szabályzata](./media/data-catalog-prerequisites/global-auth-policy.png)

További információkért lásd a [Hitelesítési házirendek konfigurálása](https://technet.microsoft.com/library/dn486781.aspx) című cikket.

## <a name="provision-data-catalog"></a>Adatkatalógus létrehozása
Szervezetenként (Azure Active Directory-tartományonként) mindössze egy adatkatalógust hozhat létre. Ezért hello tulajdonosa vagy a társtulajdonos az Azure-előfizetés toothis Azure Active Directory-tartományba tartozó már létrehozott egy katalógust, ha csak akkor tudja toocreate katalógus újra még akkor is, ha több Azure-előfizetéssel rendelkezik. hogy egy adatkatalógus az Azure Active Directory-tartományban, a felhasználó hozott létre Ugrás toohello tootest [Azure Data Catalog-kezdőlap](http://azuredatacatalog.com) , és ellenőrizze, hogy megjelenik-e hello katalógus. Ha már létrehoztak egy katalógust meg, hagyja ki a következő eljárást, majd a toohello következő szakasz hello.    

1. Nyissa meg toohello [Data Catalog szolgáltatás lapján](https://azure.microsoft.com/services/data-catalog) kattintson **Ismerkedés**.
   
    ![Azure Data Catalog – marketinges kezdőlap](media/data-catalog-get-started/data-catalog-marketing-landing-page.png)
2. Jelentkezzen be egy felhasználói fiókot, amely hello tulajdonosa, vagy egy Azure-előfizetés társtulajdonos. A bejelentkezés után a lap következő hello láthatja.
   
    ![Azure Data Catalog – adatkatalógus létrehozása](media/data-catalog-get-started/data-catalog-create-azure-data-catalog.png)
3. Adjon meg egy **neve** hello adatkatalógus hello **előfizetés** toouse szeretne, és hello **hely** hello katalógus.
4. Bontsa ki az **Árképzés** csomópontot, és válassza ki az Azure Data Catalog **kiadását** (Ingyenes vagy Standard).
    ![Azure Data Catalog – kiadás kiválasztása](media/data-catalog-get-started/data-catalog-create-catalog-select-edition.png)
5. Bontsa ki a **katalógus felhasználói** kattintson **Hozzáadás** a hello data catalog tooadd felhasználók. Toothis csoport automatikusan tagjává válnak.
    ![Azure Data Catalog – felhasználók](media/data-catalog-get-started/data-catalog-add-catalog-user.png)
6. Bontsa ki a **katalógus-rendszergazdák** kattintson **Hozzáadás** tooadd további rendszergazdák a hello data catalog. Toothis csoport automatikusan tagjává válnak.
    ![Azure Data Catalog – rendszergazdák](media/data-catalog-get-started/data-catalog-add-catalog-admins.png)
7. Kattintson a **katalógus létrehozása** toocreate hello adatkatalógus a szervezet számára. Hello kezdőlapja a hello data catalog létrehozása után megjelenik.
    ![Azure Data Catalog – létrehozva](media/data-catalog-get-started/data-catalog-created.png)    

### <a name="find-a-data-catalog-in-hello-azure-portal"></a>A data catalog található hello Azure-portálon
1. Egy külön lapján hello webböngészőben, vagy egy külön webkiszolgáló böngészőablakban nyissa meg toohello [Azure-portálon](https://portal.azure.com) , és jelentkezzen be az hello ugyanazt a fiókot, hogy Ön használt toocreate hello adatkatalógus hello előző lépésben.
2. Válassza a **Tallózás** lehetőséget, majd kattintson a **Data Catalog** elemre.
   
    ![Az Azure Data Catalog – keresse meg az Azure](media/data-catalog-get-started/data-catalog-browse-azure-portal.png) katalógus létrehozott hello adatokat látja.
   
    ![Azure Data Catalog – katalóguslista megtekintése](media/data-catalog-get-started/data-catalog-azure-portal-show-catalog.png)
3. Kattintson a létrehozott hello katalógus. Megjelenik a hello **Data Catalog** hello portál panel.
   
   ![Azure Data Catalog – panel a portálon ](media/data-catalog-get-started/data-catalog-blade-azure-portal.png)
4. A data catalog hello tulajdonságainak megtekintése, és frissítse azokat. Kattintson például **tarifacsomag** és hello kiadását módosítani.
   
    ![Azure Data Catalog – tarifacsomag](media/data-catalog-get-started/data-catalog-change-pricing-tier.png)

### <a name="adventure-works-sample-database"></a>Az Adventure Works példaadatbázisa
Ebben az oktatóanyagban adategységek (táblák) regisztrálja az SQL Server adatbázismotor hello hello AdventureWorks2014 adatbázist, de bármilyen támogatott adatforrást használhat, ha szeretne ismerős és megfelelő tooyour szerepkört adatokkal toowork. A támogatott adatforrások listájáért lásd: [Supported data sources](data-catalog-dsr.md) (Támogatott adatforrások).

### <a name="install-hello-adventure-works-2014-oltp-database"></a>Hello Adventure Works 2014 OLTP adatbázis telepítése
hello Adventure Works adatbázisa szabványos online tranzakció-feldolgozási forgatókönyvek egy fiktív kerékpárgyártó (Adventure Works Cycles), beleértve a termékek, értékesítési, és a beszerzési támogatja. Ebben az oktatóanyagban termékekre vonatkozó információkat fog regisztrálni az Azure Data Catalogba.

tooinstall hello Adventure Works példaadatbázisának:

1. Töltse le az [Adventure Works 2014 Full Database Backup.zip](https://msftdbprodsamples.codeplex.com/downloads/get/880661) fájlt a CodePlex oldalról.
2. a számítógépre, toorestore hello adatbázis hello utasításokat követve [egy adatbázis biztonsági másolatának visszaállítása az SQL Server Management Studio használatával](http://msdn.microsoft.com/library/ms177429.aspx), vagy a következő lépések végrehajtásával:
   1. Nyissa meg az SQL Server Management Studio eszközt, és csatlakozzon az SQL Server adatbázismotor toohello.
   2. Kattintson jobb gombbal a **Databases** (Adatbázisok) lehetőségre, majd a **Restore Database** (Adatbázis visszaállítása) elemre.
   3. A **Restore Database**, hello kattintson **eszköz** választás, **forrás** kattintson **Tallózás**.
   4. A **Select backup devices** (Biztonsági mentési eszközök kiválasztása) lehetőségnél kattintson az **Add** (Hozzáadás) lehetőségre.
   5. Nyissa meg toohello mappa hello esetében **AdventureWorks2014.bak** fájlt, jelölje be hello fájlt, majd kattintson **OK** tooclose hello **keresse meg a biztonságimásolat-fájl** párbeszédpanel megnyitásához.
   6. Kattintson a **OK** tooclose hello **válassza ki a biztonsági mentési eszközöket** párbeszédpanel megnyitásához.    
   7. Kattintson a **OK** tooclose hello **Restore Database** párbeszédpanel megnyitásához.

Ezután már regisztrálhatnak Azure Data Catalog használatával az hello Adventure Works példaadatbázisát az adategységeket.

## <a name="register-data-assets"></a>Adategységek regisztrálása
Ebben a gyakorlatban hello regisztrációs eszköz tooregister adategységeket hello Adventure Works adatbázisból hello katalógussal együtt használja. Regisztráció az hello folyamat például nevek, típusok és helyek fő szerkezeti metaadatok beolvasása a hello adatforrás és hello eszközöket tartalmaz, és a metaadatok toohello katalógus másolása. hello adatforrás és az adategységek maradnak. Ha vannak, de hello metaadatok hello katalógus toomake használatával könnyebben feltárhatóvá és értelmezhetővé őket.

### <a name="register-a-data-source"></a>Adatforrás regisztrálása
1. Nyissa meg toohello [Azure Data Catalog-kezdőlap](http://azuredatacatalog.com) kattintson **adatok közzététele**.
   
   ![Azure Data Catalog – Adatok közzététele gomb](media/data-catalog-get-started/data-catalog-publish-data.png)
2. Kattintson a **alkalmazás indítása** toodownload, telepítése és futtatása hello frissítésregisztráló eszköz a számítógépen.
   
   ![Azure Data Catalog – Indítás gomb](media/data-catalog-get-started/data-catalog-launch-application.png)
3. A hello **üdvözli** kattintson **jelentkezzen be a** hitelesítő adataival.     
   
    ![Azure Data Catalog – Kezdőlap](media/data-catalog-get-started/data-catalog-welcome-dialog.png)
4. A hello **Microsoft Azure Data Catalog** kattintson **SQL Server** és **következő**.
   
    ![Azure Data Catalog – adatforrások](media/data-catalog-get-started/data-catalog-data-sources.png)
5. Adja meg a hello SQL Server csatlakozási tulajdonságokat az **AdventureWorks2014** (lásd a következő példa hello), majd **CONNECT**.
   
   ![Azure Data Catalog – SQL Server-kapcsolati tulajdonságok](media/data-catalog-get-started/data-catalog-sql-server-connection.png)
6. Regisztrálja a adategységet hello metaadatait. Ebben a példában regisztrálása **Production/Product** objektumok hello AdventureWorks Production névtérből:
   
   1. A hello **kiszolgálói hierarchia** fán, bontsa ki a **AdventureWorks2014** kattintson **éles**.
   2. A Ctrl+kattintás módszerrel jelölje ki egyszerre a következőket: **Product**, **ProductCategory**, **ProductDescription** és **ProductPhoto**.
   3. Kattintson a hello **helyezze át a kiválasztott nyíl** (**>**). Ez a művelet az összes kijelölt objektumot áthelyezi hello **regisztrált objektumok toobe** listája.
      
      ![Azure Data Catalog oktatóanyag – objektumok megkeresése és kiválasztása](media/data-catalog-get-started/data-catalog-server-hierarchy.png)
   4. Válassza ki **előképet** tooinclude hello adatok pillanatkép előnézetét. hello pillanatkép too20 rekordok egyes tartalmazza, és hello katalógusba való másolás.
   5. Válassza ki **adatok profillal együtt** tooinclude hello objektum statisztikai adatok hello-profil pillanatképe (például: minimális, maximális és átlagos értéket vár egy oszlophoz, a sorok számát).
   6. A hello **címkéket** mezőbe írja be **adventure works, ciklusok**. Ez a művelet hozzáadja az adategységekhez a keresési címkéket. Címkék nagyszerű módját toohelp felhasználók keresése egy bizonyos regisztrált adatforrást.
   7. Hello nevét adja meg egy **szakértői** ezeken az adatokon (nem kötelező).
      
      ![Azure Data Catalog útmutató – objektumok toobe regisztrálva](media/data-catalog-get-started/data-catalog-objects-register.png)
   8. Kattintson a **REGISTER** (REGISZTRÁLÁS) gombra. Az Azure Data Catalog regisztrálja a kiválasztott objektumokat. Ebben a gyakorlatban az Adventure Works kiválasztott hello objektumok van regisztrálva. hello frissítésregisztráló eszköz metaadatait a hello adategységet, és másolja át az adatokat hello Azure Data Catalog szolgáltatásba. hello adatok marad, ahol jelenleg van, és hello irányítását hello rendszergazdák és a házirendek hello aktuális rendszer alatt marad.
      
      ![Azure Data Catalog – regisztrált objektumok](media/data-catalog-get-started/data-catalog-registered-objects.png)
   9. toosee a regisztrált adatforrás-objektumok, kattintson a **View Portal**. Hello Azure Data Catalog-portált győződjön meg arról, hogy látni minden négy táblák és hello adatbázis hello táblázatos nézetben.
      
      ![Az objektumok hello Azure Data Catalog-portál ](media/data-catalog-get-started/data-catalog-view-portal.png)

Ebben a gyakorlatban regisztrálta az Adventure Works példaadatbázisának hello objektumok, hogy azok könnyen megtalálhatók legyenek felhasználók a szervezeten belüli. Hello következő gyakorlatból elsajátíthatja, hogyan toodiscover regisztrált adategységeket.

## <a name="discover-data-assets"></a>Adategységek felderítése
Az Azure Data Catalog felderítési funkciója elsődlegesen két mechanizmust használ: keresés és szűrés.

Tervezett toobe elsajátítható és hatékony keresést. Alapértelmezés szerint a keresési feltételek elleni hello katalógusban, beleértve a felhasználó által megadott jegyzetek egyik tulajdonságnak sem teljesül.

Szűrés úgy van kialakítva, toocomplement keresése. Kiválaszthatja, hogy adott jellemzőiket, például a szakértők, adatforrástípust, objektumtípus, és címkék tooview megfelelő adategységeket és tooconstrain keresési eredmények toomatching eszközök.

A Keresés és szűrés együttes használatával gyorsan navigálhat hello adatforrások regisztrált az Azure Data Catalog toodiscover hello adategységeket van szüksége.

Ebben a gyakorlatban hello Azure Data Catalog portál toodiscover adategységeket hello előző gyakorlat regisztrált használja. A keresési szintaxissal kapcsolatban lásd: [Data Catalog Search syntax reference](https://msdn.microsoft.com/library/azure/mt267594.aspx) (A Data Catalog keresési szintaxisának leírása).

Az alábbiakban néhány példa az adategységek hello katalógusban.  

### <a name="discover-data-assets-with-basic-search"></a>Adategységek felderítése az alapszintű kereséssel
Az alapszintű kereséssel egy vagy több keresőkifejezést megadva végezhet keresést a katalógusban. A eredményei bármely eszközök, amelyek bármely tulajdonság egy vagy több megadott hello feltételek egyeznek.

1. Kattintson a **Home** hello Azure Data Catalog-portál. Hello böngésző bezárása után, nyissa meg toohello [Azure Data Catalog-kezdőlap](https://www.azuredatacatalog.com).
2. Hello keresési mezőbe, írja be a `cycles` nyomja le az ENTER **ENTER**.
   
    ![Azure Data Catalog – alapszintű szöveges keresés](media/data-catalog-get-started/data-catalog-basic-text-search.png)
3. Ellenőrizze, hogy az összes négy táblák és hello adatbázis (AdventureWorks2014) hello eredmények látható-e. Válthat **rácsnézethez** és **listanézet** gombjaival hello eszköztár hello kép a következő ábrán. Figyelje meg, hogy hello keresési kulcsszót a találatok között hello van jelölve, mert hello **kiemelése** lehetőség **ON**. Azt is megadhatja, hány hello **laponként eredmények** a találatok között.
   
    ![Azure Data Catalog – alapszintű szöveges keresés, találatok](media/data-catalog-get-started/data-catalog-basic-text-search-results.png)
   
    Hello **keresések** panel bal oldali hello és hello egy **tulajdonságok** panel jobb hello megtalálható. A hello **keresések** panelen, módosítsa a keresési feltételt, és eredmények szűrésére. Hello **tulajdonságok** panel hello rács vagy listáját jeleníti meg a kiválasztott objektum tulajdonságait.
4. Kattintson a **termék** hello keresési eredmények között. Hello kattintson **előzetes**, **oszlopok**, **adatok profil**, és **dokumentáció** lapokon, vagy kattintson a hello nyíl tooexpand hello alsó ablaktáblán láthatóvá válnak.  
   
    ![Azure Data Catalog – alsó panel](media/data-catalog-get-started/data-catalog-data-asset-preview.png)
   
    A hello **előzetes** lapon levő hello hello adatok előnézetének megtekintéséhez **termék** tábla.  
5. Hello kattintson **oszlopok** lapon oszlopok toofind adatait (például **neve** és **adattípus**) a hello adategységet.
6. Hello kattintson **adatok profil** lapon toosee hello profilkészítési adatokat (például: a sorok, adatokat vagy egy oszlop legkisebb érték méretének száma) a hello adategységet.
7. Hello eredmények szűrése **szűrők** hello bal oldalon. Kattintson például **tábla** a **objektumtípus**, és tekintse meg a csak négy hello táblák, nem az adatbázis hello.
   
    ![Azure Data Catalog – találatok szűrése](media/data-catalog-get-started/data-catalog-filter-search-results.png)

### <a name="discover-data-assets-with-property-scoping"></a>Adategységek felderítése tulajdonságértékekben való kereséssel
A megadott tulajdonságérték azt tapasztalja, ahol hello keresési kifejezés egyeztetve van-e hello adategységeket segít hatókörének tulajdonság.

1. Törölje a jelet hello **tábla** alapján szűrni **objektumtípus** a **szűrők**.  
2. Hello keresési mezőbe, írja be a `tags:cycles` nyomja le az ENTER **ENTER**. Lásd: [Data Catalog keresési szintaxist hivatkozás](https://msdn.microsoft.com/library/azure/mt267594.aspx) az összes hello is használhatja a Keresés a data catalog hello tulajdonságait.
3. Ellenőrizze, hogy az összes négy táblák és hello adatbázis (AdventureWorks2014) hello eredmények látható-e.  
   
    ![Data Catalog – tulajdonságértékekben való keresés, találatok](media/data-catalog-get-started/data-catalog-property-scoping-results.png)

### <a name="save-hello-search"></a>Hello Keresés mentése
1. A hello **keresések** hello ablaktábláján **aktuális keresés** szakaszt, adjon meg egy nevet a hello keresés, és kattintson **mentése**.
   
    ![Azure Data Catalog – keresés mentése](media/data-catalog-get-started/data-catalog-save-search.png)
2. Győződjön meg róla, alapján jeleníti meg, hogy a Keresés mentése hello **mentett keresések**.
   
    ![Azure Data Catalog – mentett keresések](media/data-catalog-get-started/data-catalog-saved-search.png)
3. Válasszon ki egy hello elvégezhető műveletekhez nyújtanak a hello mentett keresés (**átnevezése**, **törlése**, **mentése alapértelmezettként** keresési).
   
    ![Azure Data Catalog – mentett kereséseken elvégezhető műveletek](media/data-catalog-get-started/data-catalog-saved-search-options.png)

### <a name="boolean-operators"></a>Logikai operátorok
A keresést logikai operátorok segítségével bővítheti vagy szűkítheti.

1. Hello keresési mezőbe, írja be a `tags:cycles AND objectType:table`, és nyomja le az ENTER **ENTER**.
2. Győződjön meg arról, hogy csak olyan táblákra (nem hello adatbázis) hello eredmények látni.  
   
    ![Azure Data Catalog – keresés logikai operátorokkal](media/data-catalog-get-started/data-catalog-search-boolean-operator.png)

### <a name="grouping-with-parentheses"></a>Csoportosítás zárójelekkel
Csoportosítási kifejezést, amelyet csoportosíthatja hello lekérdezés tooachieve logikai elkülönítés érdekében, különösen a logikai operátorokkal együtt részeit.

1. Hello keresési mezőbe, írja be a `name:product AND (tags:cycles AND objectType:table)` nyomja le az ENTER **ENTER**.
2. Ellenőrizze, hogy látható-e csak hello **termék** tábla hello keresési eredmények között.
   
    ![Azure Data Catalog – keresések csoportosítása](media/data-catalog-get-started/data-catalog-grouping-search.png)   

### <a name="comparison-operators"></a>Összehasonlító operátorok
Az összehasonlító operátorok segítségével a szám és adat adattípusú tulajdonságok esetében a nem egyenlő összehasonlítások is használhatók.

1. Hello keresési mezőbe, írja be a `lastRegisteredTime:>"06/09/2016"`.
2. Törölje a jelet hello **tábla** alapján szűrni **objektumtípus**.
3. Nyomja le az **ENTER** billentyűt.
4. Ellenőrizze, hogy látható-e hello **termék**, **ProductCategory**, **ProductDescription**, és **ProductPhoto** táblák és hello AdventureWorks2014 adatbázis regisztrálta a találatok között.
   
    ![Azure Data Catalog – összehasonlító találatok](media/data-catalog-get-started/data-catalog-comparison-operator-results.png)

Lásd: [hogyan toodiscover adategységek](data-catalog-how-to-discover.md) adategységek kapcsolatos részletes információkat és [Data Catalog keresési szintaxist hivatkozás](https://msdn.microsoft.com/library/azure/mt267594.aspx) keresési szintaxis.

## <a name="annotate-data-assets"></a>Adategységek ellátása dekorációkkal
Ebben a gyakorlatban hello Azure Data Catalog-portál tooannotate használ (például a leírásokat, címkéket és szakértők hozzáadása) hello katalógusban korábban regisztrált adategységeket. hello jegyzetek kiegészíti az és javítása érdekében hello kinyert metaadatokat hello adatforrás regisztráció során és teszi hello adategységeket sokkal könnyebb toodiscover és megérteni.

Ebben a gyakorlatban egyetlen adategységet (ProductPhoto) fogunk dekorációkkal ellátni. Hozzáadhat egy felhasználóbarát név és leírás toohello ProductPhoto adategységet.  

1. Nyissa meg toohello [Azure Data Catalog-kezdőlap](https://www.azuredatacatalog.com) és a keresési `tags:cycles` toofind hello regisztrált adategységeket.  
2. Kattintson a találatok között a **ProductPhoto** elemre.  
3. Adja meg **Termékképek** a **rövid név** és **termékfényképek marketingcélú** a hello **leírás**.
   
    ![Azure Data Catalog – ProductPhoto leírása](media/data-catalog-get-started/data-catalog-productphoto-description.png)
   
    Hello **leírás** segít másoknak feltárni és értelmezni, miért és hogyan toouse hello kiválasztott adategységet. Lehetősége van további címkék hozzáadására és oszlopok megtekintésére is. Most megpróbálhatja keresési és szűrési toodiscover adategységeket hello leíró metaadatok használatával felvett toohello katalógus.

Azt is megteheti hello ezen a lapon a következő:

* Adja hozzá a hello adategységet szakértői. Kattintson a **Hozzáadás** a hello **szakértők** területen.
* Címkék hozzáadása hello dataset szinten. Kattintson a **Hozzáadás** a hello **címkék** területen. A címke lehet felhasználói vagy szószedetcímke. hello a Data Catalog Standard kiadása egy üzleti szószedetet, amelyek segítségével a katalógus-rendszergazdák egy központi üzleti elnevezési rendszert megadása tartalmaz. A katalógus felhasználói ezután a szószedet kifejezéseivel jelölhetik meg az adategységeket. További információkért lásd: [hogyan mentése tooset hello szabályozott címkézésre üzleti szószedet](data-catalog-how-to-business-glossary.md)
* Címkék hozzáadása hello szinten. Kattintson a **Hozzáadás** alatt **címkék** hello oszlop tooannotate keresi.
* Adjon meg leírást hello szinten. Töltse ki az oszlop **Leírás** mezőjét. Hello adatforrás hello leírás metaadatokat is megtekintheti.
* Adja hozzá **hozzáférés kérése** információt, amely mutatja a felhasználók hogyan férhetnek hozzá a toorequest toohello adategységet.
  
    ![Azure Data Catalog – címkék és leírások hozzáadása](media/data-catalog-get-started/data-catalog-add-tags-experts-descriptions.png)
* Válassza ki a hello **dokumentáció** lapra, és adja meg a hello adategységet dokumentációját. Az Azure Data Catalog dokumentációja az adatkatalógus akár egy tartalomtárházban toocreate egy teljes leírását az adategységet is használhatja.
  
    ![Azure Data Catalog – Dokumentáció lap](media/data-catalog-get-started/data-catalog-documentation.png)

Azt is megteheti egy jegyzet toomultiple adategységeket. Például minden hello adatok eszközök regisztrálása, és adja meg számukra szakértő.

![Azure Data Catalog – dekoráció készítése több adategységhez](media/data-catalog-get-started/data-catalog-multi-select-annotate.png)

Az Azure Data Catalog egy tömeg irányítására forrás megközelítés tooannotations támogatja. A Data Catalog felhasználók hozzáadhatnak címkék (felhasználó vagy szószedet), leírások és egyéb metaadatokat, hogy bármely felhasználó, akinek meglátásai vannak egy adategységet és használatát is, hogy rögzített szempontjából, illetve a rendelkezésre álló tooother felhasználók.

Lásd: [hogyan tooannotate adategységek](data-catalog-how-to-annotate.md) ellátása megjegyzésekkel adategységeket kapcsolatos részletes információkat.

## <a name="connect-toodata-assets"></a>Csatlakozás toodata eszközök
Ebben a gyakorlatban a kapcsolatadatok segítségével az adategységeket egy integrált ügyféleszközben (Excel) és egy nem integrált eszközben (SQL Server Management Studio) is meg fogja nyitni.

> [!NOTE]
> Fontos, hogy az Azure Data Catalog nem biztosítanak tooremember adatforrásának toohello tényleges – egyszerűen csak megkönnyíti az Ön toodiscover és megértette. Sikeres csatlakozás tooa adatforrás esetén hello ügyfélalkalmazás úgy dönt, hogy a Windows-felhasználó hitelesítő adatait, és kéri a hitelesítő adatokat, szükség esetén használja. Ha Ön nem korábban kapott hozzáférési toohello adatforrás, hozzáférést kap, mielőtt az csatlakozna toobe kell.
> 
> 

### <a name="connect-tooa-data-asset-from-excel"></a>Csatlakozás tooa adategységet az Excelből
1. A találatok közül válassza ki a **Product** elemet. Kattintson a **Megnyitás a következőben** elemre, majd hello eszköztár **Excel**.
   
    ![Az Azure Data Catalog – kapcsolódás toodata eszköz](media/data-catalog-get-started/data-catalog-connect1.png)
2. Kattintson a **nyitott** hello letöltési előugró ablakban. Ez a felület hello böngésző függően változhat.
   
    ![Azure Data Catalog – exceles csatlakozási fájl letöltése](media/data-catalog-get-started/data-catalog-download-open.png)
3. A hello **Microsoft Excel biztonsági figyelmeztetés** ablak, kattintson a **engedélyezése**.
   
    ![Azure Data Catalog – Excel biztonsági előugró ablak](media/data-catalog-get-started/data-catalog-excel-security-popup.png)
4. Ne hello alapértelmezett hello **és adatokat importálhat** párbeszédpanel megnyitásához, és kattintson **OK**.
   
    ![Azure Data Catalog – Excel, adatok beolvasása](media/data-catalog-get-started/data-catalog-excel-import-data.png)
5. Nézet hello az adatforrás az Excel programban.
   
    ![Azure Data Catalog – terméktábla az Excelben](media/data-catalog-get-started/data-catalog-connect2.png)

Ebben a gyakorlatban Azure Data Catalog módszerével észlelt toodata eszközök csatlakoztatva. Hello Azure Data Catalog-portált, és kapcsolódhatnak közvetlenül a hello hello integrált ügyfélalkalmazások használatával **Megnyitás** menü. Bármilyen alkalmazással, ha úgy dönt, hello kapcsolat helyére vonatkozó információkat hello eszköz metaadatok szereplő használatával is kapcsolódhatnak. Például használhatja az SQL Server Management Studio tooconnect toohello AdventureWorks2014 adatbázis tooaccess hello adatok hello ebben az oktatóanyagban regisztrált adategységeket.

1. Nyissa meg az **SQL Server Management Studiót**.
2. Hello a **tooServer csatlakozás** párbeszédpanelen adja meg a kiszolgálónév hello hello a **tulajdonságok** hello Azure Data Catalog-portál ablaktábláján.
3. Használja a megfelelő hitelesítési és a hitelesítő adatok tooaccess hello adategységet. Nem rendelkezik hozzáféréssel, az információt használja a hello **hozzáférés kérése** mező tooget azt.
   
    ![Azure Data Catalog – hozzáférés kérése](media/data-catalog-get-started/data-catalog-request-access.png)

Kattintson a **nézet kapcsolati karakterláncok** tooview, és másolja ODBC, OLEDB és az ADF.NET kapcsolati karakterláncok toohello vágólapra az alkalmazás számára.

## <a name="manage-data-assets"></a>Adategységek felügyelete
Ebben a lépésben lásd: hogyan tooset biztonsági az adategységeket. A Data Catalog nem nyújt a felhasználóknak hozzáférést toohello adatokat mozgatná. hello adatforrás hello tulajdonosának adatelérési szabályozza.

Használhatja a Data Catalog toodiscover adatforrások és tooview hello metaadatok kapcsolódó hello katalógusban regisztrált toohello adatforrások. Előfordulhatnak olyan helyzetek, azonban ha adatforrások használatuk csak akkor látható toospecific felhasználók vagy az adott csoportok toomembers. Ezek a forgatókönyvek használhatja regisztrált adategységeket hello katalógus belül a Data Catalog tootake tulajdonjogát, és toothen hello láthatóságának hello eszközök Ön a tulajdonosa.

> [!NOTE]
> Ebben a gyakorlatban leírt hello felügyeleti lehetőségek csak a Standard Edition Azure Data Catalog hello, nem a hello ingyenes kiadásban.
> Az Azure Data Catalog az adategységek saját tulajdonba, társtulajdonosok toodata eszközök hozzáadása és hello adategységek láthatóságának beállítása.
> 
> 

### <a name="take-ownership-of-data-assets-and-restrict-visibility"></a>Adategységek birtokbavétele és láthatóságának korlátozása
1. Nyissa meg toohello [Azure Data Catalog-kezdőlap](https://www.azuredatacatalog.com). A hello **keresési** szöveget adja meg a `tags:cycles` nyomja le az ENTER **ENTER**.
2. Hello eredménylista bármely elemére, és kattintson a **tulajdonba** hello eszköztáron.
3. A hello **felügyeleti** hello szakasza **tulajdonságok** panelen, kattintson a **tulajdonba**.
   
    ![Azure Data Catalog – saját tulajdonba vétel](media/data-catalog-get-started/data-catalog-take-ownership.png)
4. toorestrict látható, válassza a **tulajdonosok és ezek felhasználók** a hello **látható** szakaszt, és kattintson **Hozzáadás**. Adja meg a felhasználó e-mail címéhez hello szövegmezőbe, majd nyomja le az **ENTER**.
   
    ![Azure Data Catalog – hozzáférés korlátozása](media/data-catalog-get-started/data-catalog-ownership.png)

## <a name="remove-data-assets"></a>Adategységek eltávolítása
Ebben a gyakorlatban hello Azure Data Catalog portál tooremove preview regisztrált adategységeket adatait használja, és törölje az adategységet hello katalógusból.

Az Azure Data Catalogban az adategységek egyesével és csoportosan is törölhetők.

1. Nyissa meg toohello [Azure Data Catalog-kezdőlap](https://www.azuredatacatalog.com).
2. A hello **keresési** szöveg adja meg a `tags:cycles` kattintson **ENTER**.
3. Hello eredménylista kiválaszt egy elemet, és kattintson a **törlése** hello eszköztáron látható hello kép a következő módon:
   
    ![Azure Data Catalog – rácselem törlése](media/data-catalog-get-started/data-catalog-delete-grid-item.png)
   
    Hello listanézet használ, hello jelölőnégyzet be hello elem toohello balra látható hello kép a következő módon:
   
    ![Azure Data Catalog – listaelem törlése](media/data-catalog-get-started/data-catalog-delete-list-item.png)
   
    Válassza ki egyszerre több adategységet is, és törölje azokat, ahogy az a következő kép hello:
   
    ![Azure Data Catalog – több adategység törlése](media/data-catalog-get-started/data-catalog-delete-assets.png)

> [!NOTE]
> hello katalógus alapértelmezett viselkedése hello van tooallow bármely felhasználó tooregister minden adatforrás és tooallow bármely felhasználó toodelete bármely regisztrált adategységet. Standard Edition Azure Data Catalog hello szereplő hello felügyeleti képességek saját tulajdonba az eszközök, akik feltérképezésére is korlátozása, és korlátozhatók a törölhetik az eszközök számára további lehetőségeket biztosít.
> 
> 

## <a name="summary"></a>Összefoglalás
Ebben az oktatóanyagban bemutattuk az Azure Data Catalog alapvető funkcióit, például a regisztrálást, a dekorálást, a felderítést és a vállalati adategységek felügyeletét. Most, hogy végrehajtotta hello oktatóanyag, a rendszer idő tooget elindult. A munkatársai figyelmét Ön és csapata támaszkodnak hello adatforrások nyilvántartására, és a fióknevet, munkatársakat toouse hello katalógus.

## <a name="references"></a>Referencia
* [Hogyan tooregister adategységeket](data-catalog-how-to-register.md)
* [Hogyan toodiscover adategységeket](data-catalog-how-to-discover.md)
* [Hogyan tooannotate adategységeket](data-catalog-how-to-annotate.md)
* [Hogyan toodocument adategységeket](data-catalog-how-to-documentation.md)
* [Hogyan tooconnect toodata eszközök](data-catalog-how-to-connect.md)
* [Hogyan toomanage adategységeket](data-catalog-how-to-manage.md)

