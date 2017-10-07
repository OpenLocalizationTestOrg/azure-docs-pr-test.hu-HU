---
title: "aaaOptimize az Azure Naplóelemzés a System Center Operations Manager-környezettel |} Microsoft Docs"
description: "A System Center Operations Manager értékelési megoldás tooassess hello kockázat és a környezetek állapotának rendszeres időközönkénti hello is használhatja."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: tysonn
ms.assetid: 49aad8b1-3e05-4588-956c-6fdd7715cda1
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/11/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c024e53826e91524c120bdb98ae7d96d6dc37d15
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-your-environment-with-hello-system-center-operations-manager-assessment-preview-solution"></a>A System Center Operations Manager értékelési (előzetes verzió) megoldás hello környezetében optimalizálása

![A System Center Operations Manager Assessment szimbólum](./media/log-analytics-scom-assessment/scom-assessment-symbol.png)

A System Center Operations Manager értékelési megoldás tooassess hello kockázat és a System Center Operations Manager server-környezetek állapotának rendszeres időközönkénti hello is használhatja. Ez a cikk segítséget nyújt a telepítését, konfigurálását és hello megoldást használni, hogy a potenciális problémákat korrekciós műveletek hajthatók végre.

Ez a megoldás a javaslatok adott tooyour telepített kiszolgálói infrastruktúra rangsorolt listáját tartalmazza. hello javaslatok szerint vannak kategóriába között négy fókusz területek, amely gyorsan megértéséhez hello kockázat, és hajtsa végre a javítási műveletet.

hello ajánlásokat hello Tudásbázis és a Microsoft szakemberei ügyfél látogatások ezer tapasztalatai alapulnak. Minden ajánlást ismerteti, miért probléma előfordulhat, hogy számít, tooyou, és hogyan tooimplement hello javasolt módosításokat.

Kiválaszthatja a legfontosabb tooyour szervezet és a nyomon követni a kockázat szabad és megfelelő környezet futtató felé összpontosít.

Után, hello megoldás felvett értékelését fejezhető be, összefoglaló adatait fókuszterületek jelenik meg hello **System Center Operations Manager Assessment** irányítópult a infrastruktúra. hello alábbi szakaszok azt ismertetik, hogyan toouse hello hello információk **System Center Operations Manager Assessment** irányítópultot, ahol megtekintheti, és ezután javasolt műveletek a SCOM-infrastruktúrában.

![A System Center Operations Manager megoldás csempe](./media/log-analytics-scom-assessment/scom-tile.png)

![A System Center Operations Manager Assessment irányítópult](./media/log-analytics-scom-assessment/scom-dashboard01.png)

## <a name="installing-and-configuring-hello-solution"></a>Telepítése és konfigurálása hello megoldás

hello megoldása a Microsoft System Operations Manager 2012 R2 és a 2012 SP1 használható.

A következő információk tooinstall hello használja, és hello megoldás konfigurálása.

 - Egy értékelési megoldás az OMS használata előtt telepített hello megoldást kell rendelkeznie. Hello megoldást telepítése [Azure piactér](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.SCOMAssessmentOMS?tab=Overview) vagy hello utasításait követve [hozzáadni a Naplóelemzési megoldásokat az hello megoldások gyűjtemény](log-analytics-add-solutions.md).

 - A felvett hello megoldás toohello munkaterületen, a hello irányítópult hello System Center Operations Manager Assessment csempéjén hello további konfigurációs szükséges jelenik meg. Kattintson a hello csempe, és hajtsa végre az hello konfigurációs lépéseket hello oldalon szerepel

 ![A System Center Operations Manager irányítópult-csempe](./media/log-analytics-scom-assessment/scom-configrequired-tile.png)

 A System Center Operations Manager hello konfigurációs hello konfigurálása lapon hello megoldás az OMS említett hello lépéseket követve végezhető el hello parancsfájl.

 Ehelyett tooconfigure hello assessment SCOM-konzolról, hajtsa végre hello az alábbi lépések hello azonos sorrendje
1. [A System Center Operations Manager Assessment hello futtató fiók beállítása](#operations-manager-run-as-accounts-for-oms)  
2. [Hello System Center Operations Manager Assessment szabály konfigurálása](#configure-the-assessment-rule)

## <a name="system-center-operations-manager-assessment-data-collection-details"></a>A System Center Operations Manager assessment az gyűjtemény adatait

System Center Operations Manager assessment hello gyűjti a WMI-adatok, beállításjegyzék-adatok, az eseménynaplóban adatok, az Operations Manager adatainak a Windows PowerShell, az SQL-lekérdezések, a fájl információkat gyűjtő hello kiszolgálóval, amelyeken engedélyezve.

hello következő táblázatban a adatgyűjtési módszerek a System Center Operations Manager értékelést, és milyen gyakran egy ügynök által gyűjtött adatok.

| Platform | Közvetlen ügynök | SCOM-ügynököt | Azure Storage | SCOM szükséges? | Felügyeleti csoport keresztül küldött SCOM ügynök adatok | Gyűjtemény gyakorisága |
| --- | --- | --- | --- | --- | --- | --- |
| Windows | | | | &#8226; | | hét napja |

## <a name="operations-manager-run-as-accounts-for-oms"></a>Az Operations Manager futtató fiókot a következő OMS

Felügyeleti csomagok a munkaterhelések tooprovide épít OMS-hozzáadott értéket szolgáltatások. Minden számítási feladat által igényelt, munkaterhelés-specifikus jogosultságokkal toorun felügyeleti csomagok eltérő biztonsági környezetben, például egy olyan tartományi fiók. Konfigurálja az Operations Manager futtató fiókja tooprovide hitelesítő adatokat.

Használja a következő információ tooset hello Operations Manager futtató fiókja a System Center Operations Manager Assessment hello.

### <a name="set-hello-run-as-account"></a>Set hello futtató fiók

1. Hello Operations Manager konzolt, keresse meg toohello **felügyeleti** fülre.
2. A hello **futtató konfiguráció**, kattintson a **fiókok**.
3. Hozzon létre hello futtató fiókot, a következő hello egy Windows-fiók létrehozása varázsló segítségével. hello fiók toouse egy meghatározott hello és a hello minden, az alábbi előfeltételek:

    >[!NOTE]
    Futtató fiók hello következő követelményeknek kell megfelelnie:
    - Tartományi fiók tagja hello helyi Rendszergazdák csoport összes kiszolgálójára hello környezetben (az összes Operations Manager szerepkör - felügyeleti kiszolgáló, az OpsMgr-adatbázis, adatraktár, jelentéskészítési, Webkonzol, átjáró)
    - A művelet Manager rendszergazdai szerepköréhez kiszabott hello felügyeleti csoport
    - Hello végrehajtása [parancsfájl](#sql-script-to-grant-granular-permissions-to-the-run-as-account) toogrant részletes engedélyeket toohello fiókot az Operations Manager által használt SQL-példány.
      Megjegyzés: Ha ez a fiók már rendelkezik rendszergazdai jogosultságokkal, majd hagyja ki hello parancsfájl végrehajtása.

4. A **terjesztési biztonsági**, jelölje be **biztonságosabb**.
5. Hello felügyeleti kiszolgáló hello fiók terjesztési helyének megadása.
3. Lépjen vissza toohello futtató konfiguráció, és kattintson a **profilok**.
4. Keresse meg a hello *SCOM Assessment profil*.
5. hello-profil nevének kell lennie: *Microsoft System Center Advisor SCOM Assessment futtató profil*.
6. Kattintson a jobb gombbal, és a tulajdonságok frissítése, és adja hozzá a nemrég létrehozott futtató fiók a 3. lépésben létrehozott hello.

### <a name="sql-script-toogrant-granular-permissions-toohello-run-as-account"></a>SQL parancsfájl toogrant részletes engedélyeket toohello futtató fiók

A következő SQL parancsfájl toogrant szükséges engedélyek toohello futtató fiókot az Operations Manager által használt SQL-példányában hello hello hajtható végre.

```
-- Replace <UserName> with hello actual user name being used as Run As Account.
USE master

-- Create login for hello user, comment this line if login is already created.
CREATE LOGIN [UserName] FROM WINDOWS


--GRANT permissions toouser.
GRANT VIEW SERVER STATE too[UserName]
GRANT VIEW ANY DEFINITION too[UserName]
GRANT VIEW ANY DATABASE too[UserName]

-- Add database user for all hello databases on SQL Server Instance, this is required for connecting tooindividual databases.
-- NOTE: This command must be run anytime new databases are added tooSQL Server instances.
EXEC sp_msforeachdb N'USE [?]; CREATE USER [UserName] FOR LOGIN [UserName];'

Use msdb
GRANT SELECT too[UserName]
Go

--Give SELECT permission on all Operations Manager related Databases

--Replace hello Operations Manager database name with hello one in your environment
Use [OperationsManager];
GRANT SELECT too[UserName]
GO

--Replace hello Operations Manager DatawareHouse database name with hello one in your environment
Use [OperationsManagerDW];
GRANT SELECT too[UserName]
GO

--Replace hello Operations Manager Audit Collection database name with hello one in your environment
Use [OperationsManagerAC];
GRANT SELECT too[UserName]
GO

--Give db_owner on [OperationsManager] DB
--Replace hello Operations Manager database name with hello one in your environment
USE [OperationsManager]
GO
ALTER ROLE [db_owner] ADD MEMBER [UserName]

```


### <a name="configure-hello-assessment-rule"></a>Hello assessment szabály konfigurálása

a System Center Operations Manager értékelési megoldás felügyeleti csomag magában foglalja egy nevű szabályt hello *Microsoft System Center Advisor SCOM Assessment futtatása Assessment szabály*. Ez a szabály felelős hello felmérés futtatása. tooenable szabály hello és hello gyakoriságának, használja az alábbi hello eljárások konfigurálása.

Microsoft System Center Advisor SCOM Assessment futtatása Assessment szabály hello alapértelmezés szerint le van tiltva. toorun hello assessment, engedélyeznie kell a hello szabály egy felügyeleti kiszolgálón. A lépéseket követve hello használata.

#### <a name="enable-hello-rule-for-a-specific-management-server"></a>Egy adott felügyeleti kiszolgáló hello szabály engedélyezése

1. A hello **szerzői műveletek** hello Operations Manager konzolt, keresse meg a szabály hello munkaterületén *Microsoft System Center Advisor SCOM Assessment futtatása Assessment szabály* a hello **szabályok** ablaktáblán.
2. Hello keresési eredmények között, válassza ki valamelyik hello szöveget tartalmazó hello *típusa: felügyeleti kiszolgáló*.
3. Kattintson a jobb gombbal a hello szabályt, és kattintson a **felülbírálások** > **a következő osztály egy adott objektumához: felügyeleti kiszolgáló**.
4.  Hello elérhető felügyeleti kiszolgálók listában válassza ki a hello felügyeleti kiszolgálót, amelyen hello szabály futtasson-e.
5.  Győződjön meg arról, hogy módosítsa felülbírálás értéke túl**igaz** a hello **engedélyezve** paraméter értékét.  
    ![bírálja felül a paraméter](./media/log-analytics-scom-assessment/rule.png)

Ebben az ablakban a továbbra is konfigurálásához futtassa a következő eljárással hello hello hello gyakoriságát.

#### <a name="configure-hello-run-frequency"></a>Futtatás hello gyakoriságának konfigurálása

hello assessment minden 10 080 perc (vagy a hét napja), beállított toorun hello alapértelmezett gyakoriság. Ha szeretné felülbírálni az hello érték tooa minimális érték 1440 perc (vagy egy nap). hello érték azt jelenti, hogy hello minimális kihagyást közötti egymást követő assessment futtatása szükséges. toooverride hello időköz, használjon hello alábbi lépéseket.

1. A hello **szerzői műveletek** hello Operations Manager konzolt, keresse meg a szabály hello munkaterületén *Microsoft System Center Advisor SCOM Assessment futtatása Assessment szabály* a hello **szabályok** ablaktáblán.
2. Hello keresési eredmények között, válassza ki valamelyik hello szöveget tartalmazó hello *típusa: felügyeleti kiszolgáló*.
3. Kattintson a jobb gombbal a hello szabályt, és kattintson a **szabály felülbírálása hello** > **a következő osztály összes objektumához: felügyeleti kiszolgáló**.
4. Változás hello **időköz** paraméter értéke tooyour szükséges intervallumértéket. Hello az alábbi példában a hello értéke too1440 perc (egy nap).  
    ![időköz paraméter](./media/log-analytics-scom-assessment/interval.png)  
    Ha hello értéke tooless mint 1440 perc, majd hello szabály lefut egy nap időközönként. Ebben a példában a hello szabály figyelmen kívül hagyja a hello időköz értéke, és egy nap gyakorisággal futtatja.


## <a name="understanding-how-recommendations-are-prioritized"></a>Hogyan kerülnek előrébb a javaslatok megértése

Minden javaslat végrehajtott, amely azonosítja a hello javaslat relatív fontosságát hello súlyozási értéket kap. Egyetlen hello 10 legfontosabb javaslatok láthatók.

### <a name="how-weights-are-calculated"></a>Hogyan súlyok kiszámítása

Súlyozás alapján három kulcsfontosságú szerepet játszik az összesített értékek:

- Hello *valószínűség* , hogy azonosított problémát okoz problémát. Nagyobb a valószínűsége annak csatlakozás tooa nagyobb összesített pontszám hello javaslat.
- Hello *hatás* hello probléma szervezetfüggő, ha azt okozzák a problémát. Egy magasabb hatása megfelel tooa nagyobb összesített pontszám hello javaslat.
- Hello *elérhető* tooimplement hello javaslat szükséges. Egy újabb elérhető csatlakozás tooa kisebb összesített pontszám hello javaslat.

minden ajánlást a súlyozási hello arányában hello összesített pontszám minden fókusz területen érhető el. Például ha a rendelkezésre állás és üzleti folytonosság fókusz terület hello ajánlást 5 %-os pontszámot, amelyekre érvényes végrehajtási növeli a teljes rendelkezésre állás és üzleti folytonosság pontszám által 5 %.

### <a name="focus-areas"></a>Fókuszterületek

**Rendelkezésre állás és üzleti folytonosság** -fókusz itt megtekinthető a szolgáltatás rendelkezésre állása, a rugalmasság, az infrastruktúra és az üzleti védelmét javaslatok.

**Teljesítmény és méretezhetőség** -e fókuszba területen látható javaslatok toohelp a szervezet informatikai infrastruktúrájának nő, győződjön meg arról, hogy az informatikai környezet megfelel az aktuális teljesítménykövetelményeknek, és képes toorespond toochanging a szükséges infrastruktúrát.

**Frissítés, az áttelepítés és a központi telepítés** - fókusz itt megtekinthető javaslatok toohelp frissít, telepítse át, és SQL Server tooyour meglévő infrastruktúra központi telepítéséhez.

**Műveletek és -figyelő** - e fókuszba területen látható javaslatok toohelp érdekében az IT-üzemeltetők valósíthatja meg megelőző karbantartási és teljesítmény maximalizálása érdekében.

### <a name="should-you-aim-tooscore-100-in-every-focus-area"></a>Akkor érhető el, tooscore minden fókusz területen 100 %-os?

Nem feltétlenül. hello javaslatok hello Tudásbázis és a Microsoft szakemberei által ügyfél látogatások ezer keresztül szerzett tapasztalatok alapulnak. Azonban nem két kiszolgáló infrastruktúrák hello azonos, és konkrét javaslatokkal lehet több vagy kevesebb vonatkozó tooyou is. Például bizonyos biztonsági javaslatok kevesebb megfelelő, ha a virtuális gépek nincsenek kitett toohello Internet lehet. Egyes rendelkezésre állási javaslatok lehet kevésbé alacsony prioritást ad hoc adatgyűjtés és a reporting Services. Problémákra, amelyek fontos tooa érett üzleti a kevésbé fontos tooa kezdeti lehet. Előfordulhat, hogy szeretné, hogy mely fókusz területek a következők a prioritások tooidentify, és tekintse hogyan a pontszámokat változnak az idők.

Minden javaslat arról, hogy miért fontos útmutatást tartalmazza. Az útmutató tooevaluate használhatja, végrehajtási hello javaslat, az informatikai szolgáltatások és hello üzleti a szervezet igényeinek hello jellege miatt.

## <a name="use-assessment-focus-area-recommendations"></a>Kiértékelés fókusz terület ajánlásai használata

Egy értékelési megoldás az OMS használata előtt telepített hello megoldást kell rendelkeznie. tooread több megoldások telepítéséről lásd: [hozzáadni a Naplóelemzési megoldásokat az hello megoldások gyűjtemény](log-analytics-add-solutions.md). Azt követően, megtekintheti a javaslatok hello összegzése az OMS-hello áttekintése oldalon hello System Center Operations Manager Assessment csempe használatával.

Nézet hello megfelelőségi értékelése az infrastruktúrát, és a-feltárás javaslatokat foglalja össze.

### <a name="tooview-recommendations-for-a-focus-area-and-take-corrective-action"></a>a fókusz területről, és hajtsa végre a megfelelő javítási műveletek tooview javaslatok

1. A hello **áttekintése** hello kattintson **System Center Operations Manager Assessment** csempére.
2. A hello **System Center Operations Manager Assessment** lapon ellenőrizze a hello összefoglaló információkat valamelyik hello fókusz terület paneleken és majd kattintson egy adott fókusz területre tooview javaslatok.
3. Bármely hello fókusz terület lapok tekintheti meg a környezetnek előrébb hello ajánlásokat. Kattintson az ajánlás **érintett objektumok** tooview ezért hello javaslatokkal kapcsolatos információk.  
    ![fókusz terület](./media/log-analytics-scom-assessment/focus-area.png)
4. Az ajánlott javítási műveletek hajthatók végre **javasolt műveletek**. Hello elem intéztek, újabb értékelések fog jegyezze fel, az elvégzett műveletek, és a megfelelőségi pontszám növeli. Javított elemek jelennek meg **átadott objektumok**.

## <a name="ignore-recommendations"></a>Figyelmen kívül hagyja a javaslatok

Ha állnak, amelyet az tooignore, létrehozhat egy szövegfájlt, amely OMS tooprevent javaslatok jelennek meg az értékelési eredmények használja.

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

### <a name="tooidentify-recommendations-that-you-want-tooignore"></a>megjeleníteni kívánt tooignore tooidentify javaslatok

1. Jelentkezzen be tooyour munkaterület, és nyissa meg a keresési napló. A következő lekérdezés toolist javaslatok, amelyek nem tudták a környezetében az hello használata.

    ```
    Type=SCOMAssessmentRecommendationRecommendationResult=Failed | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```

    Naplófájl-keresési lekérdezés ábrázoló képernyőfelvétel hello itt található:  
    ![naplóbeli keresés](./media/log-analytics-scom-assessment/scom-log-search.png)

2. Válassza ki a megjeleníteni kívánt tooignore javaslatok. A következő eljárással hello hello értékeket szeretné használni a RecommendationId.

### <a name="toocreate-and-use-an-ignorerecommendationstxt-text-file"></a>toocreate és -felhasználási egy IgnoreRecommendations.txt szövegfájl

1. Hozzon létre egy IgnoreRecommendations.txt nevű fájlt.
2. Illessze be, vagy adjon meg minden RecommendationId minden javaslat, hogy azt szeretné, hogy külön sorban OMS tooignore és majd mentse és zárja be hello fájl számára.
3. A következő mappa minden olyan számítógépen OMS tooignore ajánlásokat, ahová hello hello fájl helyezze el.
4. Hello Operations Manager felügyeleti kiszolgálón - *SystemDrive*: System Center 2012 R2\Operations Manager\Server \Program Files\Microsoft.

### <a name="tooverify-that-recommendations-are-ignored"></a>tooverify, hogy javaslatokat figyelmen kívül lesznek hagyva

1. Hét naponta alapértelmezés szerint a következő ütemezett értékelési hello futtatása után hello megadott javaslatok figyelmen kívül hagyott megjelölve, és nem jelenik meg a hello assessment irányítópulton.
2. Használhatja a következő naplófájl-keresési lekérdezések toolist hello ajánlások hello figyelmen kívül hagyja.

    ```
    Type=SCOMAssessmentRecommendationRecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```

3. Ha később úgy dönt, hogy szeretné-e véve toosee javaslatok, távolítsa el a IgnoreRecommendations.txt fájlokat, vagy RecommendationIDs eltávolíthatja őket.

## <a name="system-center-operations-manager-assessment-solution-faq"></a>A System Center Operations Manager értékelési megoldás – gyakori kérdések

*Hello értékelési megoldás toomy OMS-munkaterület felvétele. Azonban nem szerepel az hello javaslatokat. miért ne?* A felvett hello megoldás, lépéseket nézet hello javaslatok hello OMS-irányítópulton a következő hello használja.  

- [A System Center Operations Manager Assessment hello futtató fiók beállítása](#operations-manager-run-as-accounts-for-oms)  
- [Hello System Center Operations Manager Assessment szabály konfigurálása](#configure-the-assessment-rule)


*Van olyan módon tooconfigure milyen gyakran hello assessment fut?* Igen. Lásd: [Futtatás gyakorisága konfigurálása hello](#configure-the-run-frequency).

*Ha egy másik kiszolgáló észlelhető után hello System Center Operations Manager értékelési megoldás felvett, az értékelési?* Igen, felderítése, után úgy értékelik ettől majd a--alapértelmezés szerint hét naponta.

*Mi az az adatgyűjtés hello hello folyamat hello neve?* AdvisorAssessment.exe

*Ha fut a hello AdvisorAssessment.exe folyamat?* AdvisorAssessment.exe fut a hello hello felügyeleti kiszolgáló Állapotfigyelő szolgáltatás, amelyben engedélyezve van a hello assessment szabály. A folyamat használja, a teljes környezet felderítési sorrendekben távoli adatgyűjtés.

*Mennyi időt vesz igénybe az adatok gyűjtését?* Adatgyűjtés hello kiszolgálón körülbelül egy órát vesz igénybe. Azt is tovább tarthat olyan környezetekben, ahol sok Operations Manager-példányok vagy adatbázisok.

*Mi történik, ha állítható be hello assessment tooless mint 1440 perc hello időköz?*  hello assessment naponta egyszer legfeljebb előre konfigurált toorun. Ha hello időköz érték tooa érték kisebb, mint 1440 perc felülbírálása, majd hello assessment használ hello időköz értékének 1440 perc.

*Hogyan tooknow előfeltételt hibák esetén?* Ha hello assessment futott, és az eredmények nem jelenik meg, akkor valószínű, hogy néhány szükséges hello Előfeltételek hello értékelése sikertelen volt. Ön is végrehajthatja a lekérdezések: `Type=Operation Solution=SCOMAssessment` és `Type=SCOMAssessmentRecommendation FocusArea=Prerequisites` a keresési napló toosee hello nem sikerült a szükséges előfeltételek.

*Van egy `Failed tooconnect toohello SQL Instance (….).` üzenet előfeltételt sikertelen. Mi az a hello problémát?* AdvisorAssessment.exe, gyűjti az adatokat, hello folyamat alatt hello hello felügyeleti kiszolgáló Állapotfigyelő szolgáltatás fut. Hello assessment részeként hello folyamat megpróbál tooconnect toohello SQL Server jelen hello Operations Manager-adatbázis esetén. Ez a hiba akkor fordulhat elő, ha a tűzfalszabályok blokkolása hello kapcsolat toohello SQL Server-példány.

*Milyen típusú adatok gyűjtése?*  gyűjtése történt a következő típusú adatok hello: - WMI-adatok - beállításjegyzék - adatok eseménynapló - adatok az Operations Manager adatainak Windows PowerShell, az SQL-lekérdezések és a fájl információkat gyűjtő keresztül.

*Miért kell tooconfigure egy futtató fiókot?* Az Operations Manager-kiszolgáló esetén a különböző SQL-lekérdezések futnak. Ahhoz, hogy azok toorun, szükséges engedélyekkel rendelkező futtató fiókot kell használnia. Emellett a helyi rendszergazdai hitelesítő adatokat is szükséges tooquery WMI.

*Miért meg csak hello első 10 javaslatok?* Helyett felkínálva feladatok teljes körű, túlságosan listáját, ajánlott összpontosítani előrébb hello javaslatok először címzést. Miután foglalkozni velük, további javaslatokat is elérhető lesz. Ha jobban szeret toosee hello részletes listáját, megtekintheti az összes naplófájl-keresési javaslatok.

*Van olyan módon tooignore ajánlást?* Igen, tekintse meg a hello [figyelmen kívül hagyja a javaslatok](#Ignore-recommendations).


## <a name="next-steps"></a>Következő lépések

- [Naplók keresése](log-analytics-log-searches.md) tooview részletes adatok a System Center Operations Manager Assessment és javaslatokat.
