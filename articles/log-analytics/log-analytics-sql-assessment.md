---
title: "aaaOptimize az SQL Server-környezet az Azure Naplóelemzés |} Microsoft Docs"
description: "Az Azure Naplóelemzés hello SQL értékelési megoldás tooassess hello kockázat és az SQL server-környezetek állapotának rendszeres időközönkénti is használhatja."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: e297eb57-1718-4cfe-a241-b9e84b2c42ac
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f31326d8cdad3ef5d5a190614d1a18c1dac826ed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-your-sql-server-environment-with-hello-sql-assessment-solution-in-log-analytics"></a>Az SQL-értékelési megoldás a Log Analyticshez hello a SQL Server-környezettel optimalizálása

![SQL Assessment szimbólum](./media/log-analytics-sql-assessment/sql-assessment-symbol.png)

SQL-értékelési megoldás tooassess hello kockázat és a környezetek állapotának rendszeres időközönkénti hello is használhatja. Ez a cikk segít telepíteni hello megoldás, hogy a potenciális problémákat korrekciós műveletek hajthatók végre.

Ez a megoldás a javaslatok adott tooyour telepített kiszolgálói infrastruktúra rangsorolt listáját tartalmazza. hello javaslatok szerint vannak kategóriába között hat fókusz gyorsan területeket megértéséhez hello kockázat, és hajtsa végre a javítási műveletet.

hello ajánlásokat hello Tudásbázis és a Microsoft szakemberei ügyfél látogatások ezer tapasztalatai alapulnak. Minden ajánlást ismerteti, miért probléma előfordulhat, hogy számít, tooyou, és hogyan tooimplement hello javasolt módosításokat.

Kiválaszthatja a legfontosabb tooyour szervezet és a nyomon követni a kockázat szabad és megfelelő környezet futtató felé összpontosít.

Után, hello megoldás felvett értékelését fejezhető be, összefoglaló adatait fókuszterületek jelenik meg hello **SQL Assessment** irányítópult hello infrastruktúra a környezetben. hello alábbi szakaszok azt ismertetik, hogyan toouse hello hello információk **SQL Assessment** irányítópultot, ahol megtekintheti, és ezután javasolt műveletek a SQL server-infrastruktúrában.

![SQL Assessment csempe képe](./media/log-analytics-sql-assessment/sql-assess-tile.png)

![SQL Assessment irányítópult képe](./media/log-analytics-sql-assessment/sql-assess-dash.png)

## <a name="installing-and-configuring-hello-solution"></a>Telepítése és konfigurálása hello megoldás
SQL Assessment összes jelenleg támogatott verziója az SQL Server Standard, Developer és Enterprise kiadás hello működik.

A következő információk tooinstall hello használja, és hello megoldás konfigurálása.

* Ügynökök telepítenie kell a kiszolgálókat, az SQL Server telepítve van.
* SQL-értékelési megoldás hello minden számítógépen, amelyen OMS-ügynököt telepített .NET-keretrendszer 4 támogatott verziója szükséges.
* A sorrend tooinstall hello megoldás hello felhasználói kell rendszergazda vagy közreműködői toohello Azure-előfizetés használata hello Azure-portálon. Ezenkívül hello felhasználói hello OMS munkaterület közreműködő vagy a rendszergazda szerepkör tagjának az OMS-portálon hello kell lennie.
* Operations Manager-ügynök hello SQL értékelésével használatakor toouse Operations Manager Run-As fiók lesz szüksége. Lásd: [Operations Manager futtató fiókok az OMS](#operations-manager-run-as-accounts-for-oms) alább olvashat.

  > [!NOTE]
  > hello MMA ügynök nem támogatja az Operations Manager Run-As fiókokat.
  >
  >
* Adja hozzá a hello SQL értékelési megoldás tooyour ismertetett eljárással hello OMS-munkaterület [hozzáadni a Naplóelemzési megoldásokat az hello megoldások gyűjtemény](log-analytics-add-solutions.md). Nincs szükség további konfigurációra.

> [!NOTE]
> Hello megoldás hozzáadása után hello AdvisorAssessment.exe nincs hozzáadva fájl tooservers ügynökkel. Konfigurációs adatok olvasása és küldi el a rendszer toohello OMS szolgáltatás hello felhőben feldolgozásra. Logikai alkalmazott toohello fogadott adatok és hello felhőszolgáltatás hello adatait rögzíti.

## <a name="sql-assessment-data-collection-details"></a>SQL Assessment az gyűjtemény adatait
SQL Assessment gyűjti a WMI-adatok, a beállításjegyzék-adatok, a teljesítményadatokat és az SQL Server dinamikus felügyeleti nézetben eredményeket hello ügynökök, amelyeken engedélyezve.

hello következő táblázatban a adatgyűjtési módszerek ügynökök, az Operations Manager (SCOM) szükséges-e, és milyen gyakran adatgyűjtés ügynök által.

| Platform | Közvetlen ügynök | SCOM-ügynököt | Azure Storage | SCOM szükséges? | Felügyeleti csoport keresztül küldött SCOM ügynök adatok | Gyűjtemény gyakorisága |
| --- | --- | --- | --- | --- | --- | --- |
| Windows | &#8226; | &#8226; |  |  | &#8226; |7 nap |

## <a name="operations-manager-run-as-accounts-for-oms"></a>Az Operations Manager futtató fiókot a következő OMS
Az OMS szolgáltatáshoz hello Operations Manager-ügynök és a felügyeleti csoport toocollect használ, és küldjön adatokat toohello OMS szolgáltatáshoz. Felügyeleti csomagokat a munkaterhelések tooprovide után OMS-buildek érték-szolgáltatások hozzáadása. Minden számítási feladat által igényelt, munkaterhelés-specifikus jogosultságokkal toorun felügyeleti csomagok eltérő biztonsági környezetben, például egy olyan tartományi fiók. Tooprovide hitelesítő adatok konfigurálása egy Operations Manager futtató fiókja szükséges.

Információ tooset hello Operations Manager futtató fiókja a SQL ellenőrzéséhez a következő hello használata.

### <a name="set-hello-run-as-account-for-sql-assessment"></a>Hello futtató fiókot az SQL-értékelés beállítása
 Ha már használja az SQL Server felügyeleti csomag hello, a futtató fiókot kell használnia.

#### <a name="tooconfigure-hello-sql-run-as-account-in-hello-operations-console"></a>tooconfigure hello SQL Futtatás mint fiók hello operatív konzolon
> [!NOTE]
> Használata hello OMS közvetlen ügynök, nem pedig hello SCOM-ügynököt, hello felügyeleti csomag mindig fut a helyi rendszer fiók hello hello biztonsági környezetében. Kihagyás lépéseket 1-5, az alábbi, majd futtassa a T-SQL vagy a Powershell sample NT AUTHORITY\SYSTEM hello felhasználónév megadása vagy hello.
>
>

1. Az Operations Manager programban nyissa meg hello operatív konzolt, és kattintson **felügyeleti**.
2. A **futtató konfiguráció**, kattintson a **profilok**, és nyissa meg a **OMS SQL Assessment futtató profil**.
3. A hello **futtató fiókok** kattintson **Hozzáadás**.
4. Válassza ki a Windows futtató fiókhoz, amely tartalmazza az SQL Server szükséges hello hitelesítő adatokat, vagy kattintson a **új** toocreate egyet.

   > [!NOTE]
   > Futtató fiók típusú hello Windows kell lennie. hello futtató fiókot is a helyi Rendszergazdák csoport összes SQL Server-példányokat futtató Windows-kiszolgálók részének kell lennie.
   >
   >
5. Kattintson a **Save** (Mentés) gombra.
6. Módosíthatja, és hajthat végre a következő T-SQL minta az egyes SQL Server-példány toogrant a minimálisan szükséges tooRun fiók tooperform SQL Assessment hello. Azonban nem kell toodo a futtató fiók része már hello SysAdmin (rendszergazda) kiszolgálói szerepkört az SQL Server-példányokat.

```
---
    -- Replace <UserName> with hello actual user name being used as Run As Account.
    USE master

    -- Create login for hello user, comment this line if login is already created.
    CREATE LOGIN [<UserName>] FROM WINDOWS

    -- Grant permissions toouser.
    GRANT VIEW SERVER STATE too[<UserName>]
    GRANT VIEW ANY DEFINITION too[<UserName>]
    GRANT VIEW ANY DATABASE too[<UserName>]

    -- Add database user for all hello databases on SQL Server Instance, this is required for connecting tooindividual databases.
    -- NOTE: This command must be run anytime new databases are added tooSQL Server instances.
    EXEC sp_msforeachdb N'USE [?]; CREATE USER [<UserName>] FOR LOGIN [<UserName>];'

```
#### <a name="tooconfigure-hello-sql-run-as-account-using-windows-powershell"></a>tooconfigure hello SQL futtató fiókot a Windows PowerShell használatával
Nyisson meg egy PowerShell-ablakot, és futtassa az információkkal már frissítése után a következő parancsfájl hello:

```

    import-module OperationsManager
    New-SCOMManagementGroupConnection "<your management group name>"

    $profile = Get-SCOMRunAsProfile -DisplayName "OMS SQL Assessment Run As Profile"
    $account = Get-SCOMrunAsAccount | Where-Object {$_.Name -eq "<your run as account name>"}
    Set-SCOMRunAsProfile -Action "Add" -Profile $Profile -Account $Account
```

## <a name="understanding-how-recommendations-are-prioritized"></a>Hogyan kerülnek előrébb a javaslatok megértése
Minden javaslat végrehajtott, amely azonosítja a hello javaslat relatív fontosságát hello súlyozási értéket kap. Csak hello tíz legfontosabb javaslatok láthatók.

### <a name="how-weights-are-calculated"></a>Hogyan súlyok kiszámítása
Súlyozás alapján három kulcsfontosságú szerepet játszik az összesített értékek:

* Hello *valószínűség* , hogy azonosított problémát okoz problémát. Nagyobb a valószínűsége annak csatlakozás tooa nagyobb összesített pontszám hello javaslat.
* Hello *hatás* hello probléma szervezetfüggő, ha azt okozzák a problémát. Egy magasabb hatása megfelel tooa nagyobb összesített pontszám hello javaslat.
* Hello *elérhető* tooimplement hello javaslat szükséges. Egy újabb elérhető csatlakozás tooa kisebb összesített pontszám hello javaslat.

minden ajánlást a súlyozási hello arányában hello összesített pontszám minden fókusz területen érhető el. Például ha hello biztonsági és megfelelőségi fókusz területen ajánlást 5 %-os pontszámot, amelyekre érvényes végrehajtási növeli a teljes biztonsági és megfelelőségi pontszám által 5 %.

### <a name="focus-areas"></a>Fókuszterületek
**Biztonsági és megfelelőségi** -e fókuszba területen látható esetleges biztonsági fenyegetéseket jelezhetnek és a behatolás, a vállalati házirendeket és a technikai, jogszabályi és szabályozásoknak megfelelőségi előírásokra vonatkozó javaslatok.

**Rendelkezésre állás és üzleti folytonosság** -fókusz itt megtekinthető a szolgáltatás rendelkezésre állása, a rugalmasság, az infrastruktúra és az üzleti védelmét javaslatok.

**Teljesítmény és méretezhetőség** -e fókuszba területen látható javaslatok toohelp a szervezet informatikai infrastruktúrájának nő, győződjön meg arról, hogy az informatikai környezet megfelel az aktuális teljesítménykövetelményeknek, és képes toorespond toochanging a szükséges infrastruktúrát.

**Áttelepítés és a telepítés frissítéséhez** - fókusz itt megtekinthető javaslatok toohelp frissít, telepítse át, és SQL Server tooyour meglévő infrastruktúra központi telepítéséhez.

**Műveletek és -figyelő** - e fókuszba területen látható javaslatok toohelp érdekében az IT-üzemeltetők valósíthatja meg megelőző karbantartási és teljesítmény maximalizálása érdekében.

**Változás- és konfigurációkezelés** -e fókuszba területen látható javaslatok toohelp védelme a napi tevékenységek, győződjön meg arról, hogy módosításokat nem negatív hatással vannak az infrastruktúra, Változáskezelő eljárások és tootrack naplózása rendszer-konfigurációkat.

### <a name="should-you-aim-tooscore-100-in-every-focus-area"></a>Akkor érhető el, tooscore minden fókusz területen 100 %-os?
Nem feltétlenül. hello javaslatok hello Tudásbázis és a Microsoft szakemberei által ügyfél látogatások ezer keresztül szerzett tapasztalatok alapulnak. Azonban nem két kiszolgáló infrastruktúrák hello azonos, és konkrét javaslatokkal lehet több vagy kevesebb vonatkozó tooyou is. Például bizonyos biztonsági javaslatok kevesebb megfelelő, ha a virtuális gépek nincsenek kitett toohello Internet lehet. Egyes rendelkezésre állási javaslatok lehet kevésbé alacsony prioritást ad hoc adatgyűjtés és a reporting Services. Problémákra, amelyek fontos tooa érett üzleti a kevésbé fontos tooa kezdeti lehet. Előfordulhat, hogy szeretné, hogy mely fókusz területek a következők a prioritások tooidentify, és tekintse hogyan a pontszámokat változnak az idők.

Minden javaslat arról, hogy miért fontos útmutatást tartalmazza. Ez az útmutató tooevaluate kell használnia, hogy hello javaslat végrehajtási, az informatikai szolgáltatások és hello üzleti a szervezet igényeinek hello jellegéből.

## <a name="use-assessment-focus-area-recommendations"></a>Kiértékelés fókusz terület ajánlásai használata
Egy értékelési megoldás az OMS használata előtt telepített hello megoldást kell rendelkeznie. tooread több megoldások telepítéséről lásd: [hozzáadni a Naplóelemzési megoldásokat az hello megoldások gyűjtemény](log-analytics-add-solutions.md). Azt követően, megtekintheti a javaslatok hello összegzése az OMS-hello áttekintése oldalon hello SQL Assessment csempe használatával.

Nézet hello megfelelőségi értékelése az infrastruktúrát, és a-feltárás javaslatokat foglalja össze.

### <a name="tooview-recommendations-for-a-focus-area-and-take-corrective-action"></a>a fókusz területről, és hajtsa végre a megfelelő javítási műveletek tooview javaslatok
1. A hello **áttekintése** hello kattintson **SQL Assessment** csempére.
2. A hello **SQL Assessment** lapon ellenőrizze a hello összefoglaló információkat valamelyik hello fókusz terület paneleken és majd kattintson egy adott fókusz területre tooview javaslatok.
3. Bármely hello fókusz terület lapok tekintheti meg a környezetnek előrébb hello ajánlásokat. Kattintson az ajánlás **érintett objektumok** tooview ezért hello javaslatokkal kapcsolatos információk.  
    ![SQL-kiértékelés ajánlásai képe](./media/log-analytics-sql-assessment/sql-assess-focus.png)
4. Az ajánlott javítási műveletek hajthatók végre **javasolt műveletek**. Hello elem intéztek, újabb értékelések fog jegyezze fel, az elvégzett műveletek, és a megfelelőségi pontszám növeli. Javított elemek jelennek meg **átadott objektumok**.

## <a name="ignore-recommendations"></a>Figyelmen kívül hagyja a javaslatok
Ha rendelkezik, amelyet az tooignore ajánlásokat, létrehozhat egy szövegfájlt, amely OMS tooprevent javaslatok jelennek meg az értékelési eredmények fogja használni.

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

### <a name="tooidentify-recommendations-that-you-will-ignore"></a>figyelmen kívül hagyja majd tooidentify javaslatok
1. Jelentkezzen be tooyour munkaterület, és nyissa meg a keresési napló. A következő lekérdezés toolist javaslatok, amelyek nem tudták a környezetében az hello használata.

   ```
   Type=SQLAssessmentRecommendation RecommendationResult=Failed | select  Computer, RecommendationId, Recommendation | sort  Computer
   ```

   Itt található napló keresési lekérdezés ábrázoló képernyőkép hello: ![nem lehetett ajánlásait](./media/log-analytics-sql-assessment/sql-assess-failed-recommendations.png)
2. Válassza ki a megjeleníteni kívánt tooignore javaslatok. A következő eljárással hello hello értékeket szeretné használni a RecommendationId.

### <a name="toocreate-and-use-an-ignorerecommendationstxt-text-file"></a>toocreate és -felhasználási egy IgnoreRecommendations.txt szövegfájl
1. Hozzon létre egy IgnoreRecommendations.txt nevű fájlt.
2. Illessze be, vagy adjon meg minden RecommendationId minden javaslat, hogy azt szeretné, hogy külön sorban OMS tooignore és majd mentse és zárja be hello fájl számára.
3. A következő mappa minden olyan számítógépen OMS tooignore ajánlásokat, ahová hello hello fájl helyezze el.
   * Azokon a számítógépeken a Microsoft Monitoring Agent (közvetlenül vagy az Operations Manager keresztül csatlakozik) – hello *SystemDrive*: \Program Files\Microsoft figyelés Agent\Agent
   * Hello Operations Manager felügyeleti kiszolgálón - *SystemDrive*: System Center 2012 R2\Operations Manager\Server \Program Files\Microsoft

### <a name="tooverify-that-recommendations-are-ignored"></a>tooverify, hogy javaslatokat figyelmen kívül lesznek hagyva
1. Alapértelmezett gyakoriság 7 nap által a következő ütemezett értékelési hello futtatása után hello megadott javaslatok figyelmen kívül hagyott megjelölve, és nem jelenik meg a hello assessment irányítópulton.
2. Használhatja a következő naplófájl-keresési lekérdezések toolist hello ajánlások hello figyelmen kívül hagyja.

   ```
   Type=SQLAssessmentRecommendation RecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
   ```
3. Ha később úgy dönt, hogy szeretné-e véve toosee javaslatok, távolítsa el a IgnoreRecommendations.txt fájlokat, vagy RecommendationIDs eltávolíthatja őket.

## <a name="sql-assessment-solution-faq"></a>SQL-értékelési megoldás – gyakori kérdések
*Milyen gyakran fut a felmérés elvégzéséhez?*

* hello assessment 7 naponként futtatható.

*Van olyan módon tooconfigure milyen gyakran hello assessment fut?*

* Jelenleg nem.

*Ha egy másik kiszolgáló észlelhető után hello SQL értékelési megoldás felvett, az értékelési?*

* Igen, ha kiderül, hogy megfelelőségét ellenőrizni kell, majd a gyakoriság 7 nap.

*Ha egy kiszolgáló le van szerelve, ha azt eltávolítja a szolgáltatásból hello assessment?*

* Ha a kiszolgáló nem időközönként adatokat küldenek a 3 hét, a rendszer eltávolítja.

*Mi az az adatgyűjtés hello hello folyamat hello neve?*

* AdvisorAssessment.exe

*Mennyi időt vesz igénybe a gyűjtött adatok toobe?*

* hello tényleges adatgyűjtés hello kiszolgálón körülbelül 1 órát vesz igénybe. Azt is tovább tarthat, amely az SQL Server-példányok vagy adatbázisok sok kiszolgálókon.

*Milyen típusú adatok gyűjtése?*

* a következő típusú adatok hello gyűjtik:
  * WMI
  * Beállításkulcs
  * Teljesítményszámlálók
  * Az SQL-dinamikus felügyeleti nézetek (DMV).

*Van olyan módon tooconfigure adatainak a gyűjtése?*

* Jelenleg nem.

*Miért kell tooconfigure egy futtató fiókot?*

* Az SQL Server az SQL-lekérdezések kis számú futnak. Ahhoz, hogy azok toorun, egy futtató fiókot a VIEW SERVER STATE engedélyek tooSQL kell használni.  Emellett a rendelés tooquery WMI, a helyi rendszergazdai hitelesítő adatokra szükség.

*Miért meg csak hello első 10 javaslatok?*

* Helyett felkínálva feladatok túlságosan teljesnek, javasoljuk, összpontosítani előrébb hello javaslatok először címzést. Miután foglalkozni velük, további javaslatokat is elérhető lesz. Ha jobban szeret toosee hello részletes listáját, hello OMS naplófájl-keresési ajánlások tekintheti meg.

*Van olyan módon tooignore ajánlást?*

* Igen, tekintse meg [figyelmen kívül hagyja a javaslatok](#ignore-recommendations) fenti szakaszban.

## <a name="next-steps"></a>Következő lépések
* [Naplók keresése](log-analytics-log-searches.md) tooview részletes SQL megfelelőségvizsgálati adatai és a javaslatok.
