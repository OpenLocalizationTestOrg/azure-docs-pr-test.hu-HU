---
title: "aaaOptimize az Active Directory-környezet az Azure Naplóelemzés |} Microsoft Docs"
description: "Active Directory-értékelési megoldás tooassess hello kockázat és a környezetek állapotának rendszeres időközönkénti hello is használhatja."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 81eb41b8-eb62-4eb2-9f7b-fde5c89c9b47
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 63290d95302a9e1d243cd993ac50556ed42b97bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-your-active-directory-environment-with-hello-active-directory-assessment-solution-in-log-analytics"></a>Az Active Directory-környezet, az Active Directory-értékelési megoldás Naplóelemzési hello optimalizálása

![AD Assessment szimbólum](./media/log-analytics-ad-assessment/ad-assessment-symbol.png)

Active Directory-értékelési megoldás tooassess hello kockázat és a környezetek állapotának rendszeres időközönkénti hello is használhatja. Ez a cikk segít telepítéséhez és használatához hello megoldás, hogy a potenciális problémákat korrekciós műveletek hajthatók végre.

Ez a megoldás a javaslatok adott tooyour telepített kiszolgálói infrastruktúra rangsorolt listáját tartalmazza. hello javaslatok szerint vannak kategóriába között négy fókusz területek, amely gyorsan megértéséhez hello kockázat, és hajtsa végre a műveletet.

hello javaslatok hello Tudásbázis és a Microsoft szakemberei ügyfél látogatások ezer tapasztalatai alapulnak. Minden ajánlást ismerteti, miért probléma előfordulhat, hogy számít, tooyou, és hogyan tooimplement hello javasolt módosításokat.

Kiválaszthatja a legfontosabb tooyour szervezet és a nyomon követni a kockázat szabad és megfelelő környezet futtató felé összpontosít.

Után, hello megoldás felvett értékelését fejezhető be, összefoglaló adatait fókuszterületek jelenik meg hello **AD Assessment** irányítópult hello infrastruktúra a környezetben. hello alábbi szakaszok azt ismertetik, hogyan toouse hello hello információk **AD Assessment** irányítópultot, ahol megtekintheti, és ezután javasolt műveletek a Active Directory-kiszolgálói infrastruktúrában.

![SQL Assessment csempe képe](./media/log-analytics-ad-assessment/ad-tile.png)

![SQL Assessment irányítópult képe](./media/log-analytics-ad-assessment/ad-assessment.png)

## <a name="installing-and-configuring-hello-solution"></a>Telepítése és konfigurálása hello megoldás
A következő információk tooinstall hello valamint hello megoldások konfigurálhatja.

* Ügynökök telepítenie kell a tartományvezérlőkön, amelyek tagjai hello tartomány toobe értékeli ki.
* Active Directory értékelési megoldás a .NET-keretrendszer 4 támogatott verzióját igényli hello (4.5.2 vagy újabb) valamennyi számítógépen, amelyen OMS-ügynököt.
* Adja hozzá a hello Active Directory-értékelési megoldás tooyour OMS-munkaterület a [Azure piactér](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ADAssessmentOMS?tab=Overview) vagy ismertetett folyamatot hello segítségével [hozzáadni a Naplóelemzési megoldások hello megoldások gyűjteménye a](log-analytics-add-solutions.md).  Nincs szükség további konfigurációra.

  > [!NOTE]
  > Hello megoldás hozzáadása után hello AdvisorAssessment.exe nincs hozzáadva fájl tooservers ügynökkel. Konfigurációs adatok olvasása és küldi el a rendszer toohello OMS szolgáltatás hello felhőben feldolgozásra. Logikai alkalmazott toohello fogadott adatok és hello felhőszolgáltatás hello adatait rögzíti.
  >
  >

## <a name="active-directory-assessment-data-collection-details"></a>Active Directory Assessment az gyűjtemény adatait

Active Directory Assessment adatokat gyűjti össze a következő források használatával, amelyeken engedélyezve hello ügynökök hello:

- Beállításjegyzék gyűjtő használatát
- LDAP-gyűjtő
- .NET-keretrendszer
- Esemény naplógyűjtő
- Active Directory Service interfaces (ADSI)
- Windows PowerShell
- Fájl adatgyűjtők
- A Windows Management Instrumentation (WMI)
- A DCDIAG eszköz API
- Fájlreplikációs szolgáltatás (NTFRS) tartozó API
- Egyéni C# kód


hello következő táblázatban a adatgyűjtési módszerek ügynökök, az Operations Manager (SCOM) szükséges-e, és milyen gyakran adatgyűjtés ügynök által.

| Platform | Közvetlen ügynök | SCOM-ügynököt | Azure Storage | SCOM szükséges? | Felügyeleti csoport keresztül küldött SCOM ügynök adatok | Gyűjtemény gyakorisága |
| --- | --- | --- | --- | --- | --- | --- |
| Windows |&#8226; |&#8226; |  |  |&#8226; |7 nap |

## <a name="understanding-how-recommendations-are-prioritized"></a>Hogyan kerülnek előrébb a javaslatok megértése
Minden javaslat végrehajtott, amely azonosítja a hello javaslat relatív fontosságát hello súlyozási értéket kap. Egyetlen hello 10 legfontosabb javaslatok láthatók.

### <a name="how-weights-are-calculated"></a>Hogyan súlyok kiszámítása
Súlyozás alapján három kulcsfontosságú szerepet játszik az összesített értékek:

* Hello *valószínűség* hibát azonosított problémákat okoz. Nagyobb a valószínűsége annak csatlakozás tooa nagyobb összesített pontszám hello javaslat.
* Hello *hatás* hello probléma szervezetfüggő, ha azt okozzák a problémát. Egy magasabb hatása megfelel tooa nagyobb összesített pontszám hello javaslat.
* Hello *elérhető* tooimplement hello javaslat szükséges. Egy újabb elérhető csatlakozás tooa kisebb összesített pontszám hello javaslat.

minden ajánlást a súlyozási hello arányában hello összesített pontszám minden fókusz területen érhető el. Például ha egy javaslatot hello biztonsági és megfelelőségi fókusz területen 5 %-os pontszámot, amelyekre érvényes végrehajtási növeli a teljes biztonsági és megfelelőségi pontszám által 5 %.

### <a name="focus-areas"></a>Fókuszterületek
**Biztonsági és megfelelőségi** -e fókuszba területen látható esetleges biztonsági fenyegetéseket jelezhetnek és a behatolás, a vállalati házirendeket és a technikai, jogszabályi és szabályozásoknak megfelelőségi előírásokra vonatkozó javaslatok.

**Rendelkezésre állás és üzleti folytonosság** -fókusz itt megtekinthető a szolgáltatás rendelkezésre állása, a rugalmasság, az infrastruktúra és az üzleti védelmét javaslatok.

**Teljesítmény és méretezhetőség** -e fókuszba területen látható javaslatok toohelp a szervezet informatikai infrastruktúrájának nő, győződjön meg arról, hogy az informatikai környezet megfelel az aktuális teljesítménykövetelményeknek, és képes toorespond toochanging a szükséges infrastruktúrát.

**Áttelepítés és a telepítés frissítéséhez** - fókusz itt megtekinthető javaslatok toohelp frissít, telepítse át, és Active Directory tooyour meglévő infrastruktúra központi telepítéséhez.

### <a name="should-you-aim-tooscore-100-in-every-focus-area"></a>Akkor érhető el, tooscore minden fókusz területen 100 %-os?
Nem feltétlenül. hello javaslatok hello Tudásbázis és a Microsoft szakemberei által ügyfél látogatások ezer keresztül szerzett tapasztalatok alapulnak. Azonban nem két kiszolgáló infrastruktúrák hello azonos, és konkrét javaslatokkal lehet több vagy kevesebb vonatkozó tooyou is. Például bizonyos biztonsági javaslatok kevesebb megfelelő, ha a virtuális gépek nincsenek kitett toohello Internet lehet. Egyes rendelkezésre állási javaslatok lehet kevésbé alacsony prioritást ad hoc adatgyűjtés és a reporting Services. Problémákra, amelyek fontos tooa érett üzleti a kevésbé fontos tooa kezdeti lehet. Előfordulhat, hogy szeretné, hogy mely fókusz területek a következők a prioritások tooidentify, és tekintse hogyan a pontszámokat változnak az idők.

Minden javaslat arról, hogy miért fontos útmutatást tartalmazza. Ez az útmutató tooevaluate kell használnia, hogy hello javaslat végrehajtási, az informatikai szolgáltatások és hello üzleti a szervezet igényeinek hello jellegéből.

## <a name="use-assessment-focus-area-recommendations"></a>Kiértékelés fókusz terület ajánlásai használata
Egy értékelési megoldás az OMS használata előtt telepített hello megoldást kell rendelkeznie. tooread több megoldások telepítéséről lásd: [hozzáadni a Naplóelemzési megoldásokat az hello megoldások gyűjtemény](log-analytics-add-solutions.md). Azt követően, megtekintheti a javaslatok hello összegzése az OMS-hello áttekintése oldalon hello Assessment csempe használatával.

Nézet hello megfelelőségi értékelése az infrastruktúrát, és a-feltárás javaslatokat foglalja össze.

### <a name="tooview-recommendations-for-a-focus-area-and-take-corrective-action"></a>a fókusz területről, és hajtsa végre a megfelelő javítási műveletek tooview javaslatok
1. A hello **áttekintése** hello kattintson **Assessment** a kiszolgálói infrastruktúra csempéjén.
2. A hello **Assessment** lapon ellenőrizze a hello összefoglaló információkat valamelyik hello fókusz terület paneleken és majd kattintson egy adott fókusz területre tooview javaslatok.
3. Bármely hello fókusz terület lapok tekintheti meg a környezetnek előrébb hello ajánlásokat. Kattintson az ajánlás **érintett objektumok** tooview ezért hello javaslatokkal kapcsolatos információk.  
    ![Kiértékelés ajánlásai képe](./media/log-analytics-ad-assessment/ad-focus.png)
4. Az ajánlott javítási műveletek hajthatók végre **javasolt műveletek**. Hello elem intéztek, ha újabb értékelések azt jelzi, hogy a javasolt műveletek vették, és a megfelelőségi pontszám növeli. Javított elemek jelennek meg **átadott objektumok**.

## <a name="ignore-recommendations"></a>Figyelmen kívül hagyja a javaslatok
Ha rendelkezik, amelyet az tooignore ajánlásokat, létrehozhat egy szövegfájlt, amely OMS tooprevent javaslatok jelennek meg az értékelési eredmények fogja használni.

### <a name="tooidentify-recommendations-that-you-will-ignore"></a>figyelmen kívül hagyja majd tooidentify javaslatok
1. Jelentkezzen be tooyour munkaterület, és nyissa meg a keresési napló. A következő lekérdezés toolist javaslatok, amelyek nem tudták a környezetében az hello használata.

   ```
   Type=ADAssessmentRecommendation RecommendationResult=Failed | select  Computer, RecommendationId, Recommendation | sort  Computer
   ```
>[!NOTE]
> Ha a munkaterületet frissített toohello [új Log Analytics lekérdezési nyelv](log-analytics-log-search-upgrade.md), majd a fenti lekérdezés hello megváltozna toohello következő.
>
> `ADAssessmentRecommendation | where RecommendationResult == "Failed" | sort by Computer asc | project Computer, RecommendationId, Recommendation`

   Itt található napló keresési lekérdezés ábrázoló képernyőkép hello: ![nem lehetett ajánlásait](./media/log-analytics-ad-assessment/ad-failed-recommendations.png)
2. Válassza ki a megjeleníteni kívánt tooignore javaslatok. A következő eljárással hello hello értékeket szeretné használni a RecommendationId.

### <a name="toocreate-and-use-an-ignorerecommendationstxt-text-file"></a>toocreate és -felhasználási egy IgnoreRecommendations.txt szövegfájl
1. Hozzon létre egy IgnoreRecommendations.txt nevű fájlt.
2. Illessze be, vagy adjon meg minden RecommendationId minden javaslat, hogy azt szeretné, hogy külön sorban Naplóelemzési tooignore és majd mentse és zárja be hello fájl számára.
3. A következő mappa minden olyan számítógépen OMS tooignore ajánlásokat, ahová hello hello fájl helyezze el.
   * Azokon a számítógépeken a Microsoft Monitoring Agent (közvetlenül vagy az Operations Manager keresztül csatlakozik) – hello *SystemDrive*: \Program Files\Microsoft figyelés Agent\Agent
   * Hello Operations Manager felügyeleti kiszolgálón - *SystemDrive*: System Center 2012 R2\Operations Manager\Server \Program Files\Microsoft

### <a name="tooverify-that-recommendations-are-ignored"></a>tooverify, hogy javaslatokat figyelmen kívül lesznek hagyva
Alapértelmezett gyakoriság 7 nap által a következő ütemezett értékelési hello futtatása után hello megadott vannak megjelölve az ajánlások *figyelmen kívül hagyott* és nem jelenik meg a hello assessment irányítópulton.

1. Használhatja a következő naplófájl-keresési lekérdezések toolist hello ajánlások hello figyelmen kívül hagyja.

    ```
    Type=ADAssessmentRecommendation RecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```
>[!NOTE]
> Ha a munkaterületet frissített toohello [új Log Analytics lekérdezési nyelv](log-analytics-log-search-upgrade.md), majd a fenti lekérdezés hello megváltozna toohello következő.
>
> `ADAssessmentRecommendation | where RecommendationResult == "Ignored" | sort by Computer asc | project Computer, RecommendationId, Recommendation`

2. Ha később úgy dönt, hogy szeretné-e véve toosee javaslatok, távolítsa el a IgnoreRecommendations.txt fájlokat, vagy RecommendationIDs eltávolíthatja őket.

## <a name="ad-assessment-solutions-faq"></a>AD Assessment megoldások – gyakori kérdések
*Milyen gyakran fut a felmérés elvégzéséhez?*

* hello assessment 7 naponként futtatható.

*Van olyan módon tooconfigure milyen gyakran hello assessment fut?*

* Jelenleg nem.

*Ha a másik kiszolgáló fel van derítve, miután felvett egy értékelési megoldás, az értékelési?*

* Igen, ha kiderül, hogy megfelelőségét ellenőrizni kell, majd a gyakoriság 7 nap.

*Ha egy kiszolgáló le van szerelve, ha azt eltávolítja a szolgáltatásból hello assessment?*

* Ha a kiszolgáló nem időközönként adatokat küldenek a 3 hét, a rendszer eltávolítja.

*Mi az az adatgyűjtés hello hello folyamat hello neve?*

* AdvisorAssessment.exe

*Mennyi időt vesz igénybe a gyűjtött adatok toobe?*

* hello tényleges adatgyűjtés hello kiszolgálón körülbelül 1 órát vesz igénybe. Azt is tovább tarthat a kiszolgálókon, amelyek nagyszámú Active Directory-kiszolgálók.

*Van olyan módon tooconfigure adatainak a gyűjtése?*

* Jelenleg nem.

*Miért meg csak hello első 10 javaslatok?*

* Helyett felkínálva feladatok túlságosan teljesnek, javasoljuk, összpontosítani előrébb hello javaslatok először címzést. Miután foglalkozni velük, további javaslatokat is elérhető lesz. Ha jobban szeret toosee hello részletes listáját, megtekintheti az összes naplófájl-keresési javaslatok.

*Van olyan módon tooignore ajánlást?*

* Igen, tekintse meg [figyelmen kívül hagyja a javaslatok](#ignore-recommendations) fenti szakaszban.

## <a name="next-steps"></a>Következő lépések
* Használjon [Log Analytics-e jelentkezni a keresések](log-analytics-log-searches.md) tooview részletes AD megfelelőségvizsgálati adatai és a javaslatok.
