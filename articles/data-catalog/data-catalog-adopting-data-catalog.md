---
title: "aaaApproach és feldolgozása az Azure Data Catalog bevezetése |} Microsoft Docs"
description: "Ez a cikk az Azure Data Catalog bevezetését megfontoló szervezetek számára mutat be egy megközelítést és egy folyamatot, beleértve a stratégiai célok kitűzését, a fő üzleti alkalmazási esetek azonosítását, valamint egy kísérleti projekt kiválasztását."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 0c771e7a-6fcd-417f-9247-897177719567
ms.service: data-catalog
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 06/15/2017
ms.author: maroche
ms.openlocfilehash: ffa6993b63c8a6066f48727fb046d0334a6df541
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="approach-and-process-for-adopting-azure-data-catalog"></a>Az Azure Data Catalog bevezetésének módszere és folyamata
Ez a cikk segít bevezetni az **Azure Data Catalog** szolgáltatást a szervezetében. toosuccessfully elfogadják **Azure Data Catalog**, három kulcsfontosságú dologra koncentrálhat: a stratégiai határozza meg, a szervezeten belül a fő üzleti alkalmazási esetek azonosítására és egy kísérleti projekt kiválasztására.

## <a name="introducing-hello-azure-data-catalog"></a>Hello Azure Data Catalog bemutatása
Hello world munka, belül Népi azzal kapcsolatos elvárások, hogyan kell tudni toofind adategységekre vonatkozó szakmai információkhoz megváltoztak. Ma a hello médiaeszközök munkahelyi használatának közösségi médiaeszközök Yammer, mára elvárás toobe képes tooquickly get segítségnyújtáshoz és tanácsadáshoz a témakörök széles skáláját. Az **Azure Data Catalog** segítségével a cégek és a munkacsoportok egyetlen központi adattárban vonhatják össze a céges adategységekre vonatkozó információkat. Az adatfelhasználók hozzájuthatnak az adategységekhez, és birtokba vehetik a szakértők által közzétett tudásanyagot.

Ez a cikk bemutatja egy használatának megközelítés toogetting **Azure Data Catalog**. hello cikk egy tipikus Data Catalog-bevezetési terv hello fiktív cég, az Adventure Works.

**Mi az az Azure Data Catalog?**

Az **Azure Data Catalog** egy teljes mértékben felügyelt Azure-alapú szolgáltatás, és egyben egy vállalati szintű, önkiszolgáló adatforrás-feltárást lehetővé tevő információ- és metaadat-katalógus. A Data Catalog meg regisztrálni, ismerje meg, megjegyzésekkel és toodata eszközök csatlakozzon. A Data Catalog tervezett toomanage különböző információkat eszközök toomake azok könnyen toofind adategységek, ismerje meg őket, és csatlakozzon toothem. Hello idő toogain insights csökkenti a rendelkezésre álló adatok alapján, és növeli a hello érték tooorganizations. több, lásd: toolearn [Microsoft Azure Data Catalog](https://azure.microsoft.com/services/data-catalog/).

## <a name="azure-data-catalog-adoption-plan"></a>Az Azure Data Catalog bevezetési terve
Egy **Azure Data Catalog** bevezetési terve leírja, hogyan hello szolgáltatás használatának előnyei hello közölt toostakeholders és a felhasználók, és milyen típusú betanítása, adja meg a toousers. A rendszer egy bevezetésének sikere illesztőprogram tooadopt Data Catalog milyen hatékonyan tudja kommunikálni hello szolgáltatás toousers és az érdekelt felek hello értéke. hello elsődleges célközönség a kezdeti bevezetési tervben olyan hello felhasználók hello szolgáltatást. Függetlenül attól, mennyi szállító-az érdekelt felek, kap Ha hello felhasználók, vagy a Data Catalog szolgáltatás ügyfelei nem térnek azok használatának, hello elfogadása nem lesz sikeres. Ennélfogva a jelen cikk feltételezi, hogy az érdekelt felek támogatása adott, és a Data Catalog felhasználói bevezetésére vonatkozó terv létrehozására összpontosít.
Egy hatékony bevezetési terv sikeresen felvázolja a felhasználók mit tesz lehetővé a Data Catalog, és megjeleníti őket a hello információkat és útmutatást tooachieve azt. Felhasználók toounderstand hello érték, amely a Data Catalog azokat a feladatokat a sikeres toohelp szükséges. Ha a felhasználók átlátják, hogy a Data Catalog segíthet nekik adatokkal jobb eredmények elérésében, törölje a jelet válik Data Catalog bevezetésének haszna hello értékének. Módosítása nehéz, így egy hatékony tervnek figyelembe tootake hello kihívásai kell.

A bevezetési terv segít kommunikálni a személyek toosucceed a kritikus és a kitűzött célokat. A tipikus tervek azt ismerteti, hogyan Data Catalog toomake felhasználók életét könnyebb lesz, és tartalmazza a következő részek hello:

* **Stratégiai célkitűzés** -segít röviden ismertetni hello bevezetési tervet a felhasználókkal és az érdekelt felek. Itt foglalhatja össze tömören a terv lényegét.
* **Próbacsapat és Véleményvezérek** - tanulási kísérleti és néhány véleményvezér súgó pontosítása hogyan toointroduce csoportok és felhasználók tooData katalógus. A véleményvezérek maguk is segíthetik a felhasználókat a tudnivalók elsajátításában. Emellett segít bevezetés hátráltató és előremozdító tooadoption azonosítását.
* **Kommunikációs és Lelkesítő terv** -felhasználók toounderstand segít hogyan Data Catalog is segítséget nyújthatnak, és elősegíti az organikus bevezetést csapatok belül, és végső soron a teljes szervezet hello.
* **Képzési terv** - képzési általában érdeklődők tooadoption sikeres bevezetéshez és kedvező eredményekhez átfogó.

Az alábbiakban néhány tippek toodefine egy **Azure Data Catalog** bevezetési tervet.

## <a name="define-your-data-catalog-project-vision"></a>Tűzze ki a Data Catalog-projekttel kapcsolatos stratégiai céljait
első lépés toodefine hello egy **Azure Data Catalog** bevezetési terv toowrite állítson össze egy leírást a próbált tooaccomplish. Ajánlott tookeep hello stratégiai utasítás viszonylag tágan még de elég tömören toodefine konkrét rövid és hosszú távú célokat is.

Az alábbiakban néhány tippek toohelp stratégiai célok meghatározásához:

* **Azonosítsa az üzembe helyezés fő mozgatórugóját hello** – gondoljon bele hello konkrét adatforrás-kezelési szükségletei vannak hello vállalatnak, amelyek a Data Catalog használatával kielégíthetők. Segít hello használatának legfőbb előnyeit a Data Catalog állapot. Például előfordulhat közös adatforrások, amelyeket minden új alkalmazottnak toolearn kapcsolatos és használatban van szükség, vagy fontos és összetett adatforrások, hogy csak néhány kulcsfontosságú személy ért alaposabban. **Az Azure Data Catalog** segítségével ellenőrizze ezen adatok adatforrásokat könnyen toodiscover és megérteni, hogy a jól ismert problémás pontok is kell közvetlenül elsimíthatók a hello szolgáltatás hello elfogadását.
* **Tisztán és egyértelműen kell** – egy tiszta hello stratégiai megértése már mindenki hello a Data Catalog hello érték azonos nézőpontra számos lehetőséget kínál toohello szervezet, és hogyan hello stratégiai támogatja-e a szervezeti célok.
* **Inspirálja személyek toowant toouse Data Catalog** – a stratégiai, és a kommunikációs tervet kell ösztönözze segítsen toorecognize, hogy a Data Catalog toofind hasznot húzhatnak, és csatlakozzon toodata források tooachieve további adatokkal.
* **Szögezze le a konkrét célokat és határidőket** – Így biztosíthatja, hogy a bevezetési tervnek konkrét és elérhető céljai legyenek. Ütemterv segít a célokra koncentrálni, és lehetővé teszi az ellenőrzőpontok toomeasure sikeres.

Íme egy példa stratégiai célokat a Data Catalog-bevezetési terv hello fiktív cég, az Adventure Works esetében:

**Az Azure Data Catalog** hello Adventure Works pénzügyi csapata toocollaborate a fő adatforrások lehetővé teszi, így minden csapattag könnyedén megtalálhatja és hello adatokkal egyenként kell, és a hello csapat egészével megoszthatja saját tudását.

Ha kitűzte a pontos stratégiai célokat, azok alapján kiválaszthatja a megfelelő próbaprojektet a Data Catalog bevezetéséhez. Általában nincsenek a Data Catalog számos forgatókönyv, a következő szakasz hello biztosít néhány tippek tooidentify releváns használati esetek.

## <a name="identify-data-catalog-business-use-cases"></a>A Data Catalog üzleti alkalmazási helyzeteinek azonosítása
tooidentify használati esetek, amelyek megfelelő tooData katalógus, szakértőivel a különböző üzleti egységek tooidentify releváns alkalmazási helyzetek és üzleti problémák toosolve. Vizsgálja meg, jelenleg milyen kihívásokat rejt a felhasználók számára az adategységek azonosítása és értelmezése. Például tegye csapatok megismerése adategységeket csak után több ember hello szervezet, aki rendelkezik releváns adatforrásokkal kérése?

Legyen a legjobb toochoose használati esetek, amelyek az alacsonyan lógó gyümölcsöt jelképezik: esetek, amelyek fontosak még van siker nagyon valószínű, ha a Data Catalog lehet megoldani.

Az alábbiakban néhány tipp tooidentify használati esetek:

* **Hello csoport hello célok meghatározása** – hogyan segített hello csapat a kitűzött célokat? A Data Catalog helyezi a hangsúlyt még nem hiszen toobe cél ebben a szakaszban. Ne feledje, hello üzleti eredmények számítanak, nem hello technológiájával kapcsolatban.
* **Adja meg a hello üzleti probléma** -Mik azok a hello problémákkal néz szembe hello csapat az adategységek megtalálását és megismerését illetően? Például fontos adatforrásokra vonatkozó információk találhatók egy hálózati mappában tárolt Excel-munkafüzetekben, és hello team sok hello munkafüzetek keresése ideig tarthat.
* **Team kulturális környezet kapcsolódó toochange megértéséhez** – bevezetés támasztotta kihívások sokszor tooresistance toochange vonatkoznak, hanem egy új eszköz végrehajtásának hello. A csapat változásokhoz toochange hozzáállása fontos szerepet játszik használata esetben, mivel hello jelenlegi folyamat csak helyben mivel"hogyan azt mindig megtörtént" vagy "Ha lehetséges, hogy a romlott minek megjavítani?". Egy új eszköz vagy folyamat bevezetése esetén mindig legegyszerűbb hello érintettek hello érték toobe is tudjuk hello módosítása a megismeréséhez, és elismerik hello problémák toobe orvosolhatók hello fontosságát.
* **Összpontosítson toodata eszközök kapcsolódó** – a csapat előtt álló hello üzleti problémák ismertetésekor kell túl "vágás hello – bozóton", és a vállalati adategységekre vonatkozó tooleveraging Újdonságok további érvényességi.

Íme néhány példa használata esetekben kapcsolódó tooData katalógus:

### <a name="example-use-cases"></a>Példák az alkalmazási helyzetekre
* **Központi nagy értékű adatforrások regisztrálása** -informatikai szervezet hello használt adatforrásokat felügyel. A Data Catalog használatával végzett informatikai felügyelet kezdő lépéseiként regisztrálhatók és megjegyzésekkel láthatók el a gyakran használt vállalati adatforrások.
* **Csapatszintű adatforrások regisztrálása** – A különböző csapatok különböző hasznos, üzletági adatforrásokkal rendelkeznek. Ismerkedés a **Azure Data Catalog** azonosítsa és regisztrálja, és a rögzítési hello csapatok kollektív ismereteit a különböző csapatok által használt fő adatforrások **Azure Data Catalog** jegyzetek.
* **Önkiszolgáló üzleti intelligencia** – A csapatoknak sokáig tart összefésülni a különböző forrásokból származó adatokat. Regisztrálja, és egy központi helyen tooeliminate kézi adatforrás-keresési folyamatok az adatforrások ellátása megjegyzésekkel.

További információk a Data Catalog-forgatókönyvekkel tooread lásd [Azure Data Catalog gyakori forgatókönyvei](data-catalog-common-scenarios.md).

Ha azonosította a Data Catalog néhány alkalmazási helyzetét, világossá válnak a gyakori forgatókönyvek. hello a következő szakasz ismerteti, hogyan tooidentify az első próbaprojekt alapján használati eset.

## <a name="choose-a-data-catalog-pilot-project"></a>Data Catalog-próbaprojekt kiválasztása
A kulcs sikeres tényező toosimplify, és úgy indítsa kis. Egy jól meghatározott próbaüzem egy korlátozott léptékű segítségével garantálják hello a projekt áthelyezése előre anélkül, hogy leragadnánk egy túl összetett, vagy amely túl sok résztvevővel rendelkező projektet. De azt is fontos tooinclude a felhasználók, a korai támogatók tooskeptics kombinációját. Hello megoldást támogató felhasználók segítséget finomíthatja a jövőbeli kommunikációs és lelkesítő terv. A szkeptikusok pedig segítenek a hátráltató tényezők azonosításában és kezelésében. Amint szkeptikusok alapján válnak, használhatja a visszajelzés tooidentify siker mozgatórugói.

A próbatervben érdemes olyan üzleti célokat kitűzni, amelyet a Data Catalog tooachieve. Hello kezdeti Próbaterv tanulságainak, a felhasználói bázis bővítheti. A kezdeti Próbaterv jó tooestablish mérhetővé válik a siker, de hello végső cél a természetes vagy ugrásszerű növekedés. A Data Catalog természetes tartásához, a felhasználók maguk irányíthatják a saját adatfelhasználásukat, és hatással és ösztönözzék a mások tooadopt és toohello katalógus bővítésére.

### <a name="target-hello-right-team"></a>Cél hello megfelelő csoport
A próbaprojekt kiválasztásakor válassza ki a hello legkedvezőbb a csoportot, amelyik egy létező üzleti probléma megoldására. Ilyen például egy üzleti elemző, aki egy SQL Server-adatbázisból készít jelentéseket. hello probléma, hogy ő tudomást hello adatforrás után csak tooseveral, munkatársakat van szó. Végül Miután jelentős időt közben toofind melyik adatforrások toouse, megtudta egy Excel-munkafüzetben, amely tartalmazza minden adatforrás leírása. Bár hello Excel-munkafüzet megfelelően leírja hello-táblázatot, amely az elemzőnek szüksége van, akkor megtalálhatta ezeket az adatforrásokat, ha regisztrálva és jegyzetelve vannak **Azure Data Catalog**.

### <a name="identify-data-heroes"></a>Adatkezelők kiválasztása
Az első próbaprojekt tartalmazzon néhány adatalkotó adatok előállításához és fogyasztásához adatokat, így hello csapat kiegyensúlyozottan képviseltesse magát.

Az **adatalkotók** az adatforrások szakértői. David például egy másik csapatban már huzamosabb ideig dolgozott az Adventure Works néhány fő adatforrásával. Előzetes toohello elfogadását **Azure Data Catalog**, David létrehozott egy Excel-munkafüzet toocapture Adventure Works adatforrásait kapcsolatos információkat.

**Az adatfelhasználók** hello adatok toosolve hello használata szakértői üzleti problémák vannak. Nancy például, egy üzleti elemző Adventure Works SQL Server data források tooanalyze adatokat használ.

Hello üzleti problémák egyike, amelyek **Azure Data Catalog** megoldja tooconnect van **Adatalkotók** túl**az adatfelhasználók**. Ezt úgy valósítja meg, hogy központi adattárként szolgál a céges adatforrásokra vonatkozó információk számára. David a Data Catalog használatával nyilvántartásba veszi az Adventure Works és az SQL Server adatforrásait. Közösségi bármely felhasználó, aki megtalálja ezt az adatforrást használó hello adatokon véleményt megoszthatja, továbbá toousing hello adatok ő észlelte. Nancy például hello adatforrások felderíti hello katalógus keresve, és közösen használja az hello adatokra vonatkozó szaktudását.  Most mások hello szervezet kiszolgálóterhelések használják ki a megosztott tudásnak hello a data catalog keresve.

* További információ az adatforrások regisztrálása toolearn lásd: [adatforrások regisztrálása](data-catalog-get-started.md).
* További információ az adatforrások felfedezése toolearn lásd [adatforrások keresése](data-catalog-get-started.md).

### <a name="start-small-and-focused"></a>Apró kezdeti lépések és összpontosítás
A legtöbb vállalati próbaprojekt esetében érdemes feltöltheti a hello katalógust nagy értékű adatforrásokkal rendelkező, hogy az üzleti felhasználók számára hamar láthatóvá hello Data Catalog értéke. Informatikai egy remek toostart érdeklődési tooyour kísérleti csapat lenne közös adatforrások azonosítására. A támogatott adatforrások, például az SQL Server, azt javasoljuk, hello **Azure Data Catalog** adatforrás-regisztráló eszköz. Hello adatforrás-regisztráló eszköz beleértve az SQL Server és Oracle-adatbázisok adatforrások széles skáláját regisztrálhatja, és SQL Server Reporting Services-jelentések. A jelenlegi adatforrások teljes listájáért lásd: [Azure Data Catalog supported data sources](data-catalog-dsr.md) (Az Azure Data Catalog által támogatott adatforrások).

Miután azonosította és fő adatforrások regisztrálva, már lehetséges tooalso importálási tárolt adatforrás-leírások más helyeken. hello Data Catalog API lehetővé teszi a fejlesztők tooload leírásokat és megjegyzéseket töltsenek be egy másik helyről, például a David létrehozott és fenntartott Excel-munkafüzet hello.

hello következő szakasz egy példaprojektet az Adventure Works vállalat hello írja le.

### <a name="an-example-project"></a>Egy példaprojekt
Az ebben a példában Nancy hello üzleti elemző, jelentéseket a csapata, SQL Server-adatbázis adatait használó számára hoz létre. hello probléma, hogy ő tudomást hello adatforrás után csak tooseveral, munkatársakat van szó. Sokkal hamarabb is megtalálhatta volna az adatforrást, ha regisztrálva és jegyzetelve vannak egy központi helyen, például az **Azure Data Catalog** szolgáltatásban.

tooillustrate milyen könnyen Nancy és csapata is értékes adatok kereséséhez, hello adatok forrás regisztrációs eszköz toopopulate hello katalógus használata információkkal (metaadatokkal) hello az adatforrásokról. Ez úgy hello hello adatbázis adatai elérhető toohello csoportját, és hello vállalati, nem csupán néhány egyéni felhasználók számára. Miután az adatforrások regisztrálva lettek a Data Catalog szolgáltatásban, Nancy és csapata könnyedén hozzáférnek az általuk használt forrásokhoz. hello eredmény egy átfogóbb és relevánsabb adatkatalógus a csapat és hello vállalati. Több csapat vezeti be a Data Catalog, üzleti adatforrások lesz-e a könnyebb toofind és -felhasználási; így a további adatok-központú kulturális környezet tooachieve további az adatait.

toolearn hello adatforrás-regisztráló eszköz, bővebben lásd: [Ismerkedés az Azure Data Catalog](data-catalog-get-started.md).

Hello kísérleti projekt részeként Nancy csapata is az adatforrás egy Excel-munkafüzet című a David és a kollégái. Mivel hello vállalat további csapatai is használja az Excel-munkafüzetek toodescribe adatforrások, hello informatikai csapat úgy dönt, toocreate eszköz toomigrate hello Excel munkafüzet tooData katalógus. Hello Data Catalog REST API tooimport meglévő megjegyzéseknek használatával hello kísérleti projekt csapata egy teljes adatkatalógus hello adatforrásokból hello adatforrás-regisztráló eszköz, kész, de adatokat kinyert metaadatokat álló lehet által korábban dokumentált adatalkotók és a fogyasztóknak, nélkül hello ismételt kézi bevitelre lenne szükség. Hello vállalati adatkatalógus növekszik, hello szervezet használatával hello adatforrás-regisztráló eszköz közös adatforrások, és az egyéni források és ritka esetekben Data Catalog API hello.

> [!NOTE]
> Egy minta eszközt, amely hello megírt **Azure Data Catalog** API toomigrate egy Excel-munkafüzet tooData katalógus. toolearn hello Data Catalog API és hello példaeszközzel, [hello Ad Hoc munkafüzetkód-példát letöltése](https://azure.microsoft.com/documentation/samples/data-catalog-dotnet-excel-register-data-assets/), és tekintse meg a hello [Azure Data Catalog REST API](https://msdn.microsoft.com/library/azure/mt267593.aspx) dokumentációjában.
>
>

Miután hello kísérleti projekt helyére került, ideje tooexecute a Data Catalog bevezetési tervét.

### <a name="execute"></a>Végrehajtás
Mostanra azonosította a Data Catalog alkalmazási helyzeteit és az első projektet is. Ezenkívül regisztrált hello kulcs Adventure Works adatforrásait, és rendelkezik bővebb információ a Excel-munkafüzetből hello meglévő hello segítségével eszközzel, hogy a beépített azt. Most már idő toowork a hello próbacsapat toostart hello Data Catalog bevezetésének folyamatát.

Az alábbiakban néhány tippek tooget indítása:

* **Keltsen érdeklődést** – A céges felhasználók érdeklődőbbek lesznek, ha hisznek abban, hogy az **Azure Data Catalog** megkönnyíti a munkájukat. Próbálja meg toomake hello párbeszéd középpontjába hello megoldás és hello előnyt biztosít, nem hello technológia.
* **Könnyítse meg a változást** – kezdje kis lépésekkel, és hello terv toobusiness felhasználók kommunikációhoz. toobe sikeres, akkor az hello elején, hogy hello eredménye befolyásolják, és magukénak érezhessék hello megoldás kulcsfontosságú tooinvolve felhasználók.
* **Készítsen fel korai támogatókat** -korai támogatók azon üzleti felhasználók, akik lelkesen végzik a dolgukat, és készséggel tooevangelize hello előnyei **Azure Data Catalog** tootheir társakhoz.
* **Célzott képzéseket** -üzleti felhasználóknak nem kell tooknow mindent tudjanak a Data Catalog, így cél képzési tooaddress csapatokra vonatkozó célkitűzésekhez. Mit a felhasználók, és hogyan bizonyos feladataik lehet módosítani, tooincorporate összpontosítani **Azure Data Catalog** azokat a mindennapi munkarutinjuk részévé válhasson.
* **Hajlandó toofail kell** – Ha hello a próbaüzem a nem szükséges hello eredmények elérése érdekében újra kiértékelje és területek toochange - hello próbaüzem, mielőtt továbblép tooa nagyobb hatókör javítás problémákat azonosítása.

Mielőtt a próbacsapat egyszer a Data Catalog, ütemezhet egy megszervezni az indító értekezlet toodiscuss elvárásait hello a kísérleti projekt, és kezdeti oktatást.

### <a name="set-expectations"></a>Az elvárások felállítása
Az elvárások felállításával és célok kitűzésével az üzleti felhasználók könnyebben koncentrálnak megadott eredmények elérésére. tookeep hello projekt előrehaladásának, rendeljen regular (például: naponta vagy hetente alapján hello és időtartamától függően hello próbaüzem) házi feladatokat. Hello legértékesebb tulajdonsága a Data Catalog egyik közösségi adategységek, hogy az üzleti felhasználók is kihasználhatja a vállalati adatokat használhatnak. Remek házi feladatnak minden kísérleti csapat tag tooregister vagy megjegyzésekkel legalább egy, az általa használt adatforrás. Lásd: [adatforrást regisztrálni](data-catalog-get-started.md) és [hogyan tooannotate adatforrások](data-catalog-get-started.md).

Szervezzen egy rendszeres időközönként tooreview hello csapatához hello jegyzetek némelyike. Jó megjegyzések adatforrások jelenti, egy Data Catalog sikeres bevezetésének hello lelke, mert a forrás elemzések lekérdezhetik a fontos adatok egy központi helyen biztosítanak. Jó megjegyzések nélkül az adatforrások ismerete hello vállalati szinten szórványos marad. Lásd: [hogyan tooannotate adatforrások](data-catalog-get-started.md).

És hello hello projekt végső próbatétele hogy a felhasználók felderítésére és megérteni hello adatforrások toouse van szükségük. Próbaüzem felhasználóknak érdemes rendszeresen tesztelniük hello katalógus tooensure, hogy hello nap tooday munkájához használt adatforrások relevánsak. Ha egy szükséges adatforrás hiányzik vagy nincs megfelelő megjegyzésekkel ellátva, ez szolgáljon a felszólítás tooregister további adatforrások vagy tooprovide további megjegyzéseket. Ez az eljárás csak nem adja hozzá érték toohello próbaprojektet teszi sikeresebbé, de alapozza a szokásokat, amelyek tooother csapatok jelenik meg hello próbaüzem befejezése után is alkot.

### <a name="provide-training"></a>Az oktatás lebonyolítása
Képzési legyen elég tooget hello felhasználók, és igazodjon toohello konkrét céljaihoz és tapasztalati szintjéhez hello próbacsapat tagjainak. tooget képzési használatába, lépésekkel hello a hello [Ismerkedés az Azure Data Catalog](data-catalog-get-started.md) cikk. Emellett letöltheti a hello [Azure Data Catalog Próbaprojektjeiről szóló oktatóbemutatót](https://github.com/Azure-Samples/data-catalog-dotnet-get-started/blob/master/Azure%20Data%20Catalog%20Training.pptx?raw=true). A PowerPoint-bemutató segít első lépések introducing Data Catalog tooyour próbacsapat tagjaival.

## <a name="conclusion"></a>Összegzés
Ha a próbacsapat már viszonylag zökkenőmentesen dolgozik, és elérte az eredetileg kitűzött célokat, is kiterjesztheti a Data Catalog bevezetésének toomore csoportok. Vonatkoznak, és finomítsa tovább a próbaprojekt tooexpand Data Catalog a teljes szervezetben.

hello részt vevő kezdeti támogatók a hello próbaüzem lehet hasznos tooget hello word kimenő kapcsolatos Data Catalog bevezetése hello előnyeit. Megoszthatják más csapatokkal hogyan Data Catalog segített a csapatuknak az üzleti problémák megoldásához, könnyebben adatforrások felfedezése és használt adatforrások hello észrevételeket oszthatnak. Például a próbacsapat – hello Adventure Works próbacsapatának kezdeti sikerült jelzik, mások milyen egyszerűen azt toofind információkat, amelyeket nehéz toofind Adventure Works-adategységeket és megérteni.

Ez a cikk arról szólt, hogyan vezetheti be az **Azure Data Catalog** szolgáltatást a szervezeténél. Azt legyen képes toostart egy Data Catalog-próbaprojekt volt, és bontsa ki a Data Catalog a szervezetben.

## <a name="more-information-about-azure-data-catalog"></a>További információk az Azure Data Catalog szolgáltatásról
* [Az Azure Data Catalog termékoldala](https://azure.microsoft.com/services/data-catalog/)
* [Az Azure Data Catalog dokumentációja](https://azure.microsoft.com/documentation/services/data-catalog/)
* [Az Azure Data Catalog gyakori forgatókönyvei](data-catalog-common-scenarios.md)
* [Adatforrások regisztrálása](data-catalog-get-started.md)
* [Adatforrások keresése](data-catalog-get-started.md)
* [Adatforrások ellátása megjegyzésekkel](data-catalog-get-started.md)
* [Metaadatok közösségi hozzáadása](data-catalog-get-started.md)
