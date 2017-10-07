---
title: "használatába aaaGet előre konfigurált megoldások |} Microsoft Docs"
description: "Hajtsa végre az ezen oktatóanyag toolearn hogyan toodeploy egy Azure IoT Suite előre konfigurált megoldás."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 6ab38d1a-b564-469e-8a87-e597aa51d0f7
ms.service: iot-suite
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: dobett
ms.openlocfilehash: a7f46023d26b08de2e8ed48c34c5066a43e3fa38
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-preconfigured-solutions"></a>Ismerkedés az előre konfigurált hello megoldások

Az Azure IoT Suite [előre konfigurált megoldások] [ lnk-preconfigured-solutions] több Azure IoT szolgáltatások toodeliver végpontok közötti megoldások, amelyek megvalósítják az IoT elterjedt üzleti forgatókönyvek kombinálni. Hello *távoli megfigyelési* előkonfigurált megoldás összekapcsolja tooand figyelők az eszközöket. Azáltal, hogy válaszoljon automatikusan toothat adatfolyamba való folyamatok hello megoldás tooanalyze hello adatfolyam az eszközök és tooimprove üzleti tevékenységét származó adatok is használhatja.

Az oktatóanyag bemutatja, hogyan tooprovision hello távoli megfigyelési előre konfigurált a megoldás. Azt is bemutatja, hogyan hello alapvető szolgáltatások előre konfigurált hello megoldás. Érheti el ezeket a funkciókat számos hello megoldás *irányítópult* , előre konfigurált hello megoldás részeként telepíti:

![Az előre konfigurált távoli figyelési megoldás irányítópultja][img-dashboard]

toocomplete ebben az oktatóanyagban aktív Azure-előfizetés szükséges.

> [!NOTE]
> Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot. További információ: [Ingyenes Azure-fiók létrehozása][lnk_free_trial].

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

## <a name="scenario-overview"></a>Forgatókönyv áttekintése

Távoli felügyeleti előkonfigurált megoldás hello központi telepítésekor az erőforrásokat, amelyek lehetővé teszik a toostep keresztül távoli figyelési forgatókönyve előre feltöltve. Ebben a forgatókönyvben számos eszközök csatlakoztatott toohello megoldás jelentik a váratlan hőmérséklet-értékek. hello következő szakaszok bemutatják, hogyan számára:

* Váratlan hőmérséklet értékek küldése hello eszközök azonosítása.
* Ezek az eszközök konfigurálásához toosend részletesebb telemetriai adatokat.
* Az eszközön a hello belső vezérlőprogram frissítése a hello probléma megoldásához.
* Győződjön meg arról, hogy a művelet megoldotta az hello probléma.

Ebben a forgatókönyvben alapfunkciója, hogy távolról is végrehajtható, ezek a műveletek hello megoldás irányítópulton. Nem kell fizikai hozzáférés toohello eszközök.

## <a name="view-hello-solution-dashboard"></a>Nézet hello megoldás irányítópultja

hello megoldás irányítópultja toomanage telepített hello megoldás lehetővé teszi. Megtekintheti például a telemetriát, eszközöket adhat hozzá és szabályokat konfigurálhat.

1. Hello kiépítés befejeztével, miután az előkonfigurált megoldás hello csempe jelzi **készen**, válassza a **indítsa el** tooopen a távoli felügyeleti megoldás portál új lapon.

    ![Indítsa el az előre konfigurált hello megoldás][img-launch-solution]

1. Alapértelmezés szerint hello megoldás portal mutatja hello *irányítópult*. Navigálhat tooother területek hello megoldás portál hello bal oldalán található hello hello menü segítségével.

    ![Az előre konfigurált távoli figyelési megoldás irányítópultja][img-menu]

hello irányítópult a következő információ hello jeleníti meg:

* Minden eszköz hello helye megjelenítő térkép toohello megoldás csatlakoztatva. Hello megoldás első futtatásakor nincsenek 25 szimulált eszközök. hello szimulált eszköz megvalósítása Azure webjobs-feladatok, és hello megoldás a hello térképen hello a Bing térképek API tooplot információkat használja. Lásd: hello [gyakran ismételt kérdések] [ lnk-faq] toolearn hogyan toomake hello térkép dinamikus.
* A **Telemetria előzményei** panelt, amely a páratartalommal és hőmérséklettel kapcsolatos telemetriát jelenít meg a kiválasztott eszközről közel valós időben, és összesített adatokat tartalmaz, például a maximális, minimális és átlagos páratartalmat.
* A **Riasztások előzményei** panelt, amely közelmúltbeli riasztási eseményeket jelenít meg, amikor egy telemetriaérték túllépett egy küszöbértéket. Saját riasztások hozzáadása toohello példákban előre konfigurált hello megoldás által létrehozott adhat meg.
* A **Feladatok** panelt, amely az ütemezett feladatok információit jeleníti meg. A **Felügyeleti feladatok** lapon ütemezheti a saját feladatait.

## <a name="view-alarms"></a>Riasztások megtekintése

hello riasztási Előzmények panelen láthatók öt eszköz nagyobbnak, mint a várt telemetriai értékek jelentik.

![TEENDŐ riasztás előzmények hello megoldás irányítópultja][img-alarms]

> [!NOTE]
> Ezek a riasztások akkor jönnek létre, előre konfigurált hello megoldás szereplő szabály által. Ez a szabály riasztást küld, ha egy eszköz által küldött hello hőmérséklet értéke meghaladja a 60. Kiválasztásával megadhatja a saját szabályok és a műveletek [szabályok](#add-a-rule) és [műveletek](#add-an-action) hello bal oldali menüben.

## <a name="view-devices"></a>Eszközök megtekintése

Hello *eszközök* jeleníti meg az összes regisztrált hello eszközök hello megoldás. Hello eszköz listából, megtekintheti és szerkesztheti az eszköz metaadatait adja hozzá vagy távolítson el eszközöket, és metódusok eszközökön. Szűrés és a rendezés hello eszközlistában eszközök hello listája. Testre szabhatja hello oszlopok hello eszközök listája látható.

1. Válasszon **eszközök** tooshow hello eszközök listája ebben a megoldásban.

   ![Hello hello megoldás portálon eszköz lista megtekintése][img-devicelist]

1. hello eszközlista kezdetben jeleníti 25 szimulált hozta létre hello létesítésének folyamatát kell használnia. Hozzáadhat további szimulált és a fizikai eszközök toohello megoldás.

1. tooview hello részletei az eszköz hello eszköz listában válassza ki egy eszközt.

   ![Hello hello megoldás portálon eszköz részleteinek megtekintése][img-devicedetails]

Hello **eszközadatok** panel hat szakaszokat tartalmazza:

* Hivatkozások gyűjteményét is tartalmazza, amelyek engedélyezik, toocustomize hello eszköz ikon, hello eszköz letiltása, szabály hozzáadása, a kíván hívni egy metódust vagy a parancsot. Ezen parancsok (egy eszközről a felhőbe irányuló üzenetek) összehasonlításáért lásd a [felhőből egy eszközre irányuló kommunikáció útmutatóját][lnk-c2d-guidance].
* Hello **eszköz iker - címkék** szakasz tooedit előfizetéscímkék értékeit hello eszköz segítségével. Címke értékek megjelenítése hello eszközök listáját, és címke értékek toofilter hello eszközre a listában.
* Hello **eszköz iker - tulajdonságok szükséges** szakasz tooset tulajdonság értékek küldött toobe toohello eszköz segítségével.
* Hello **eszköz iker - tulajdonságok jelentett** szakasz hello eszközről küldött tulajdonságértékek jeleníti meg.
* Hello **eszköztulajdonságok** szakasz hello identitásjegyzékhez hello eszköz például az azonosító és a hitelesítő kulcs információval is.
* Hello **legutóbbi feladatok** szakasz információkat az eszköz nemrég célzott feladatokat jeleníti meg.

## <a name="filter-hello-device-list"></a>Szűrő hello eszközök listája

A szűrő toodisplay csak a nem várt hőmérséklet értékek küldenek eszközöket használhatja. távoli felügyeleti előkonfigurált megoldás hello tartalmaz hello **nem megfelelő eszközök** átlagos hőmérséklet érték nagyobb, mint 60 tooshow eszközök szűréséhez. Emellett [saját szűrőket is létrehozhat](#add-a-filter).

1. Válasszon **mentett szűrő megnyitása** toodisplay rendelkezésre álló szűrők listája. Válassza a **nem megfelelő eszközök** tooapply hello szűrő:

    ![Szűrők hello listájának megjelenítése][img-unhealthy-filter]

1. hello eszközlista most jeleníti csak átlagos hőmérséklet érték nagyobb, mint 60.

    ![Nézet hello szűrt eszköz listáját jeleníti meg a nem megfelelő eszközt][img-filtered-unhealthy-list]

## <a name="update-desired-properties"></a>Eszköz kívánt tulajdonságainak frissítése

Sikerült tehát azonosítani néhány eszközt, amelyek javításra szorulhatnak. Azonban úgy dönt, hogy hello Adatok gyakoriságot 15 másodperc nincs-e elegendő hello probléma egyértelmű diagnosztikai. Hello telemetriai gyakoriság toofive másodperc tooprovide módosításával kapcsolatos további adatok pontok toobetter hello probléma diagnosztizálásához. A konfigurációs módosítás tooyour távoli eszközök hello megoldás portálról tolható ki. Egyszer, hello hatás értékelje ki, és majd bérlőként hello eredmények hello módosítása, hogy.

Kövesse ezeket a lépéseket toorun egy feladatot, amely módosítja a hello **TelemetryInterval** hatással hello eszközök tulajdonság szükséges. Ha hello eszköz megkapja-e új hello **TelemetryInterval** tulajdonság értéke, módosítsa a konfigurációs toosend telemetriai minden ötödik helyett 15 másodpercenként másodperc:

1. Válassza a nem megfelelő eszközök listája hello megjelennek a hello eszközök listáját, amíg **Feladatütemező**, majd **szerkesztése eszköz iker**.

1. Hello feladat hívás **módosításának telemetriai időköze**.

1. Hello hello értékének módosítása **kívánt tulajdonság** neve **kívánt. Config.TelemetryInterval** toofive másodperc.

1. Válassza az **Ütemezés** elemet.

    ![Hello TelemetryInterval tulajdonság toofive másodperc módosítása][img-change-interval]

1. Kísérheti hello hello feladat a hello **felügyeleti feladatok** hello lapjára.

> [!NOTE]
> Toochange kívánt tulajdonság értéke egy különálló eszköz, használja hello **kívánt tulajdonságokkal** hello szakasz **eszközadatok** panel egy feladat futtatása helyett.

Ez a feladat beállítja hello hello értékének **TelemetryInterval** hello eszköz iker tulajdonság szükséges, az összes kijelölt hello szűrő eszközök hello. hello eszközök lekérje ezt az értéket a hello eszköz iker, és azok viselkedését frissítése. Amikor egy eszköz kéri le és dolgozza fel a kívánt tulajdonságot egy eszközt a két, hello megfelelő érték tulajdonság állítja be.

## <a name="invoke-methods"></a>Metódusok meghívása

Amíg hello feladat fut, bizonyára észrevette, a nem megfelelő eszközök hello listájában, hogy ezek az eszközök rendelkeznek-e a régi (kevesebb mint 1.6-os verzióra) belső vezérlőprogram verziója.

![Nézet hello jelentett belsővezérlőprogram-verziónként hello nem megfelelő eszközök][img-old-firmware]

Lehet, hogy a belső vezérlőprogram verziójának hello okának váratlan hello hőmérséklet értékei, mivel tudható, hogy a más kifogástalan állapotú eszközök nemrég frissített tooversion 2.0 volt. Használhatja a hello beépített **régi belső vezérlőprogram eszközök** szűrése tooidentify azokat az eszközöket, a régi belső vezérlőprogramjának következő verziójával. Hello portálról távolról frissítheti az összes hello eszközök továbbra is fut a régi belsővezérlőprogram-verziók:

1. Válasszon **mentett szűrő megnyitása** toodisplay rendelkezésre álló szűrők listája. Válassza a **régi belső vezérlőprogram eszközök** tooapply hello szűrő:

    ![Szűrők hello listájának megjelenítése][img-old-filter]

1. hello eszközök listája most csak régi belső vezérlőprogramjának következő verziójával rendelkező eszközöket jeleníti meg. Ez a lista tartalmazza a hello öt eszköz hello által azonosított **nem megfelelő eszközök** szűrő és három további eszközöket:

    ![Nézet hello szűrt eszköz listáját jeleníti meg régi eszközt][img-filtered-old-list]

1. Válassza a **Feladatütemező**, majd a **Metódus meghívása** elemet.

1. Állítsa be **feladatnév** túl**belső vezérlőprogram frissítési tooversion 2.0**.

1. Válasszon **InitiateFirmwareUpdate** , hello **metódus**.

1. Set hello **FwPackageUri** paraméter túl**https://iotrmassets.blob.core.windows.net/firmwares/FW20.bin**.

1. Válassza az **Ütemezés** elemet. hello alapértelmezett most hello feladat toorun szolgál.

    ![Feladat tooupdate hello az kijelölt hello eszközök belső vezérlőprogramjai létrehozása][img-method-update]

> [!NOTE]
> Ha azt szeretné, hogy egy adott eszközre vonatkozó metódus tooinvoke, **módszerek** a hello **eszközadatok** panel egy feladat futtatása helyett.

Ez a feladat hív meg hello **InitiateFirmwareUpdate** közvetlen módszer hello szűrő kijelölt összes hello eszközön. Eszközök azonnal tooIoT Hub válaszol, és majd aszinkron módon az hello belső vezérlőprogram frissítési folyamat elindítása. hello eszközök ismertetik állapot hello belső vezérlőprogram frissítési folyamat keresztül jelentett tulajdonságértékek, ahogy az alábbi képernyőképek hello. Válassza ki a hello **frissítése** ikon tooupdate hello információk hello eszköz és a feladat azokat:

![A feladatlista hello belső vezérlőprogram frissítési lista futó megjelenítő][img-update-1]
![eszközök listáját ábrázoló belső vezérlőprogram frissítési állapot][img-update-2]
![listáját ábrázoló hello belső vezérlőprogram frissítési feladatlista teljes][img-update-3]

> [!NOTE]
> Éles környezetben feladatokat toorun kijelölt karbantartási időszak alatt ütemezhető.

## <a name="scenario-review"></a>Forgatókönyv áttekintése

Ebben a forgatókönyvben egy potenciális problémát egy, a távoli eszközök hello riasztási előzmények hello irányítópult és a szűrő használata azonosított. Újból használt hello szűrési és egy feladat tooremotely konfigurálása hello eszközök tooprovide további információt toohelp hello probléma diagnosztizálásához. Végezetül használt szűrő, ezért tooschedule karbantartási feladat az érintett hello eszközökön. Ha toohello irányítópult ad vissza, ellenőrizheti, hogy nincsenek-e már nem bármely adatforrásból származó eszközöket a megoldás a riasztások. Használhatja a szűrőt, amely a belső vezérlőprogram hello tooverify naprakész a megoldás összes hello eszközön, és hogy nincsenek több nem megfelelő eszközök:

![Szűrő, amely kimutatja, hogy az összes eszköz belső vezérlőprogramja naprakész][img-updated]

![Szűrő, amely kimutatja, hogy az összes eszköz kifogástalan állapotú][img-healthy]

## <a name="other-features"></a>Egyéb jellemzők

hello következő szakaszok ismertetik a távoli felügyeleti előkonfigurált megoldás hello néhány további szolgáltatásokat, nem ismerteti a hello előző forgatókönyv részeként.

### <a name="customize-columns"></a>Oszlopok testreszabása

Testre szabhatja kiválasztásával hello eszközök listája látható hello információk **oszlop szerkesztő**. Hozzáadhat és eltávolíthat jelentett tulajdonságot és címkeértékeket megjelenítő oszlopokat. Átrendezheti és át is nevezheti az oszlopokat:

   ![Oszlop szerkesztő adatmegőrzési hello eszközök listája][img-columneditor]

### <a name="customize-hello-device-icon"></a>Hello eszköz ikon testreszabása

Testre szabhatja a hello eszköz listáját a következőtől: hello hello eszköz ikon **eszközadatok** panel az alábbiak szerint:

1. Válassza ki a hello ceruza ikonra tooopen hello **Szerkesztés kép** eszköz panel:

   ![Eszközkép szerkesztőjének megnyitása][img-startimageedit]

1. Töltse fel az új képet vagy egy meglévő lemezképet hello használja, és válassza a **mentése**:

   ![Eszközkép szerkesztőjének szerkesztési folyamata][img-imageedit]

1. hello most kijelölt képet jeleníti meg hello **ikon** hello eszköz oszlop.

> [!NOTE]
> hello lemezképe a blob Storage tárolóban. Hello eszköz iker egy címkét tartalmaz egy hivatkozást toohello lemezképet a blob Storage tárolóban.

### <a name="add-a-device"></a>Eszköz hozzáadása

Előre konfigurált hello megoldás telepítésekor automatikusan létesítsen 25 minta eszközök hello eszközök listája látható. Ezek az eszközök Azure WebJobs-feladatban futó, *szimulált eszközök*. Szimulált eszközök megkönnyítik az Ön tooexperiment előre konfigurált hello megoldással hello kell toodeploy valódi, fizikai eszközök nélkül. Ha szeretné, hogy a valós toohello megoldás tooconnect, tekintse meg a hello [csatlakozzon a távoli felügyeleti előkonfigurált megoldás eszköz toohello] [ lnk-connect-rm] oktatóanyag.

hello következő lépések bemutatják, hogyan tooadd szimulált eszköz toohello megoldást:

1. Lépjen vissza toohello eszközök listáját.

1. tooadd egy eszközt, válassza a **+ A eszköz hozzáadása** hello bal alsó sarokban található.

   ![Egy előre konfigurált toohello megoldás hozzáadása][img-adddevice]

1. Válasszon **új hozzáadása** a hello **szimulált eszköz** csempére.

   ![Az új eszköz adatainak megadása az irányítópulton][img-addnew]

   Továbbá toocreating új szimulált eszköz, is hozzáadhat egy fizikai eszköz Ha úgy dönt, hogy toocreate egy **egyéni eszköz**. toolearn fizikai eszközök toohello megoldás, bővebben lásd: [csatlakozni az eszköz toohello IoT Suite távoli felügyeleti előkonfigurált megoldás][lnk-connect-rm].

1. Válassza a **Meghatározom a saját eszközazonosítómat** elemet, és írjon be egy egyéni eszközazonosító nevet, például a **mydevice_01** nevet.

1. Válassza a **Létrehozás** elemet.

   ![Új eszköz mentése][img-definedevice]

1. A 3. lépésében **szimulált eszköz hozzáadása**, válassza a **végzett** tooreturn toohello eszközök listáját.

1. Megtekintheti az eszköz **futtató** hello eszköz listában.

    ![Új eszköz megtekintése az eszközlistában][img-runningnew]

1. Megtekintheti továbbá hello szimulált telemetriai hello Irányítópulton az új eszközről:

    ![Az új eszköz telemetriájának megtekintése][img-runningnew-2]

### <a name="disable-and-delete-a-device"></a>Eszköz letiltása és törlése

Letilthatja az eszközöket, és a letiltásuk után eltávolíthatja őket:

![Eszköz letiltása és eltávolítása][img-disable]

### <a name="add-a-rule"></a>Szabály hozzáadása

Nincsenek az előzőekben adott hozzá az új eszköz hello szabályok. Ebben a szakaszban hozzáadhat egy szabályt, amely egy riasztás akkor váltja ki, ha új hello által jelentett hello hőmérséklet eszköz meghaladja 47 fokban megadva. Mielőtt elkezdené, figyelje meg, hogy hello telemetriai előzmények hello új eszköz hello irányítópulton látható hello eszköz hőmérséklete soha nem meghaladja 45 fokban megadva.

1. Lépjen vissza toohello eszközök listáját.

1. tooadd szabály hello eszközt, jelölje ki az új eszköz hello **eszközök listában**, és válassza a **Hozzáadás szabály**.

1. Hozzon létre egy szabályt, amely használja **hőmérséklet** hello adatmező és felhasználási **AlarmTemp** hello kimeneti, amikor hello elérte 47 fok:

    ![Eszközszabály hozzáadása][img-adddevicerule]

1. toosave a módosításokat, válassza a **mentése és a szabályok megtekintése**.

1. Válasszon **parancsok** hello eszköz részletező ablaktábláján hello új eszköz.

    ![Eszközszabály hozzáadása][img-adddevicerule2]

1. Válassza ki **ChangeSetPointTemp** hello parancs lista és a set **SetPointTemp** too45. Ezután válassza a **Parancs küldése** elemet:

    ![Eszközszabály hozzáadása][img-adddevicerule3]

1. Lépjen vissza toohello irányítópult. Rövid idő múlva megjelenik egy új bejegyzést hello **riasztási előzmények** ablaktáblán, ha az új eszköz által jelentett hello hőmérséklet hello 47 fokos küszöbértéket meghaladó:

    ![Eszközszabály hozzáadása][img-adddevicerule4]

1. Megtekintheti és szerkesztheti a hello a szabályok **szabályok** hello irányítópult oldalán:

    ![Eszközszabályok listázása][img-rules]

1. Megtekintheti és szerkesztheti is lehet képernyőfelvételt készíteni a hello válasz tooa szabályban lévő hello műveleteket **műveletek** hello irányítópult oldalán:

    ![Eszközműveletek listázása][img-actions]

> [!NOTE]
> E-mailt küldhet lehetséges toodefine műveleteket, vagy a válasz tooa SMS szabály, vagy egy üzleti rendszer integrálása egy [logikai alkalmazás][lnk-logic-apps]. További információkért lásd: hello [Azure IoT Suite távoli megfigyelési csatlakozás logikai alkalmazás tooyour előre konfigurált megoldás][lnk-logicapptutorial].

### <a name="manage-filters"></a>Szűrők kezelése

Hello eszközlistában hozzon létre, mentse, és töltse be újra a szűrők toodisplay eszközök csatlakoztatott tooyour hub testreszabott listáját. toocreate szűrő:

1. Válassza ki a hello Szerkesztés szűrő ikonja felett hello eszközök listája:

    ![Nyissa meg hello szűrő szerkesztő][img-editfiltericon]

1. A hello **szűrő szerkesztő**, vegye fel a hello mezők, operátorok és értékek toofilter hello eszközök listáját. A szűrő több záradékok toorefine adhat hozzá. Válasszon **szűrő** tooapply hello szűrő:

    ![Szűrő létrehozása][img-filtereditor]

1. Ebben a példában hello alapján szűri a program gyártó és a modell száma:

    ![Szűrt lista][img-filterelist]

1. toosave egy egyéni nevet, a szűrő válasszon hello **Mentés másként** ikon:

    ![Szűrő mentése][img-savefilter]

1. egy korábban mentett szűrő tooreapply válasszon hello **mentett szűrő megnyitása** ikon:

    ![Szűrő megnyitása][img-openfilter]

Eszközazonosító, eszközállapot, kívánt tulajdonságok, jelentett tulajdonságok és címkék alapján hozhat létre szűrőket. Hozzáadja a saját egyéni címkék tooa eszköz hello **címkék** hello szakasza **eszközadatok** panelen, vagy futtassa egy feladat tooupdate címkék több eszközön.

> [!NOTE]
> A hello **szűrő szerkesztő**, használhatja a hello **nézet speciális** tooedit hello lekérdezés szövegének közvetlenül.

### <a name="commands"></a>Parancsok

A hello **eszközadatok** panelen parancsok toohello eszköz küldhet. Indításakor egy eszközt, küldi hello információ parancsok toohello megoldás támogatja. Parancsok és módszerek hello különbségei tárgyalását lásd: [Azure IoT Hub felhő eszközre beállítások][lnk-c2d-guidance].

1. Válasszon **parancsok** a hello **. eszköz részletei** hello kiválasztott eszköz panel:

   ![Eszközparancsok az irányítópulton][img-devicecommands]

1. Válassza ki **PingDevice** hello listából.

1. Válassza a **Parancs küldése** elemet.

1. Az előző hello hello parancs hello állapotát tekintheti meg.

   ![Parancsállapot az irányítópulton][img-pingcommand]

hello megoldás minden parancs küldi hello állapotát követi nyomon. Hello eredménye kezdetben **függőben lévő**. Amikor hello eszköz jelzi, hogy teljesítette-e hello parancs, hello eredménye túl van-e állítva**sikeres**.

## <a name="behind-hello-scenes"></a>Hello háttérben

Amikor telepít egy előre beállított megoldás, hello központi telepítési folyamat több erőforrást hello kijelölt Azure-előfizetés hoz létre. Megtekintheti ezeket az erőforrásokat a hello Azure [portal][lnk-portal]. hello központi telepítési folyamat létrehoz egy **erőforráscsoport** előkonfigurált megoldást választja hello neve alapján néven:

![Hello Azure-portálon az előkonfigurált megoldás][img-portal]

Megtekintheti a hello-beállítások az egyes erőforrások hello az erőforrások listájához a hello erőforráscsoport lehetőséget.

Előre konfigurált hello megoldás forráskódját hello is megtekintheti. távoli előkonfigurált megoldás forráskódját figyelési hello van hello [azure iot-távoli-ellenőrző] [ lnk-rmgithub] GitHub-tárházban:

* Hello **DeviceAdministration** mappa hello forráskódja hello irányítópult tartalmazza.
* Hello **szimulátor** mappa tartalmazza hello hello szimulált eszköz.
* Hello **EventProcessor** mappa hello forráskódja kezelő Bejövő hello telemetriai hello háttér-folyamat tartalmazza.

Amikor elkészült, hogy törli előre konfigurált hello megoldás az Azure-előfizetéshez hello [azureiotsuite.com] [ lnk-azureiotsuite] hely. Ez a hely összes hello előre konfigurált hello megoldás létrehozása után kiépített erőforrások tooeasily törlése lehetővé teszi.

> [!NOTE]
> tooensure törlése minden kapcsolódó előre konfigurált toohello megoldás, törölje a hello [azureiotsuite.com] [ lnk-azureiotsuite] helyről, és ne törölje az erőforráscsoportot hello hello portálon.

## <a name="next-steps"></a>Következő lépések

Most, hogy egy előre konfigurált működő megoldást telepítése után, továbbra is a következő cikkek hello olvasásával Ismerkedés az IoT Suite:

* [A távoli figyelési előre konfigurált megoldás bemutatója][lnk-rm-walkthrough]
* [Csatlakozzon a távoli felügyeleti előkonfigurált megoldás eszköz toohello][lnk-connect-rm]
* [Engedélyek hello azureiotsuite.com webhelyen][lnk-permissions]

[img-launch-solution]: media/iot-suite-getstarted-preconfigured-solutions/launch.png
[img-dashboard]: media/iot-suite-getstarted-preconfigured-solutions/dashboard.png
[img-menu]: media/iot-suite-getstarted-preconfigured-solutions/menu.png
[img-devicelist]: media/iot-suite-getstarted-preconfigured-solutions/devicelist.png
[img-alarms]: media/iot-suite-getstarted-preconfigured-solutions/alarms.png
[img-devicedetails]: media/iot-suite-getstarted-preconfigured-solutions/devicedetails.png
[img-devicecommands]: media/iot-suite-getstarted-preconfigured-solutions/devicecommands.png
[img-pingcommand]: media/iot-suite-getstarted-preconfigured-solutions/pingcommand.png
[img-adddevice]: media/iot-suite-getstarted-preconfigured-solutions/adddevice.png
[img-addnew]: media/iot-suite-getstarted-preconfigured-solutions/addnew.png
[img-definedevice]: media/iot-suite-getstarted-preconfigured-solutions/definedevice.png
[img-runningnew]: media/iot-suite-getstarted-preconfigured-solutions/runningnew.png
[img-runningnew-2]: media/iot-suite-getstarted-preconfigured-solutions/runningnew2.png
[img-rules]: media/iot-suite-getstarted-preconfigured-solutions/rules.png
[img-adddevicerule]: media/iot-suite-getstarted-preconfigured-solutions/addrule.png
[img-adddevicerule2]: media/iot-suite-getstarted-preconfigured-solutions/addrule2.png
[img-adddevicerule3]: media/iot-suite-getstarted-preconfigured-solutions/addrule3.png
[img-adddevicerule4]: media/iot-suite-getstarted-preconfigured-solutions/addrule4.png
[img-actions]: media/iot-suite-getstarted-preconfigured-solutions/actions.png
[img-portal]: media/iot-suite-getstarted-preconfigured-solutions/portal.png
[img-disable]: media/iot-suite-getstarted-preconfigured-solutions/solutionportal_08.png
[img-columneditor]: media/iot-suite-getstarted-preconfigured-solutions/columneditor.png
[img-startimageedit]: media/iot-suite-getstarted-preconfigured-solutions/imagedit1.png
[img-imageedit]: media/iot-suite-getstarted-preconfigured-solutions/imagedit2.png
[img-editfiltericon]: media/iot-suite-getstarted-preconfigured-solutions/editfiltericon.png
[img-filtereditor]: media/iot-suite-getstarted-preconfigured-solutions/filtereditor.png
[img-filterelist]: media/iot-suite-getstarted-preconfigured-solutions/filteredlist.png
[img-savefilter]: media/iot-suite-getstarted-preconfigured-solutions/savefilter.png
[img-openfilter]:  media/iot-suite-getstarted-preconfigured-solutions/openfilter.png
[img-unhealthy-filter]: media/iot-suite-getstarted-preconfigured-solutions/unhealthyfilter.png
[img-filtered-unhealthy-list]: media/iot-suite-getstarted-preconfigured-solutions/unhealthylist.png
[img-change-interval]: media/iot-suite-getstarted-preconfigured-solutions/changeinterval.png
[img-old-firmware]: media/iot-suite-getstarted-preconfigured-solutions/noticeold.png
[img-old-filter]: media/iot-suite-getstarted-preconfigured-solutions/oldfilter.png
[img-filtered-old-list]: media/iot-suite-getstarted-preconfigured-solutions/oldlist.png
[img-method-update]: media/iot-suite-getstarted-preconfigured-solutions/methodupdate.png
[img-update-1]: media/iot-suite-getstarted-preconfigured-solutions/jobupdate1.png
[img-update-2]: media/iot-suite-getstarted-preconfigured-solutions/jobupdate2.png
[img-update-3]: media/iot-suite-getstarted-preconfigured-solutions/jobupdate3.png
[img-updated]: media/iot-suite-getstarted-preconfigured-solutions/updated.png
[img-healthy]: media/iot-suite-getstarted-preconfigured-solutions/healthy.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com
[lnk-logic-apps]: https://azure.microsoft.com/documentation/services/app-service/logic/
[lnk-portal]: http://portal.azure.com/
[lnk-rmgithub]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-logicapptutorial]: iot-suite-logic-apps-tutorial.md
[lnk-rm-walkthrough]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-connect-rm]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md
[lnk-c2d-guidance]: ../iot-hub/iot-hub-devguide-c2d-guidance.md
[lnk-faq]: iot-suite-faq.md