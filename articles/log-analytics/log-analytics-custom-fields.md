---
title: "Naplóelemzési aaaCustom mezőinek |} Microsoft Docs"
description: "Naplóelemzési hello egyéni mezők jellemzője toocreate lehetővé teszi a saját kereshető mezők toohello tulajdonságok összegyűjtött rekord hozzáadása OMS adatokból.  Ez a cikk ismerteti hello folyamat toocreate egyéni mezők és részletes útmutató egy minta eseményhez."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 31572b51-6b57-4945-8208-ecfc3b5304fc
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/18/2016
ms.author: bwren
ms.openlocfilehash: 1aa0e497d5b1d7898b0da6a5ef40f568e63bc589
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="custom-fields-in-log-analytics"></a>A Naplóelemzési egyéni mezők
Hello **egyéni mezők** Naplóelemzési funkciója lehetővé teszi meglévő rekordjainak tooextend hello OMS-tárház saját kereshető mezők hozzáadásával.  Egyéni mező automatikusan feltöltődik értékkel más tulajdonságokat a hello kinyert adatokból ugyanazt a rekordot.

![Egyéni mezők – áttekintés](media/log-analytics-custom-fields/overview.png)

Például hello minta az alábbi rekordnak a hasznos adatok megbúvó hello esemény leírása.  Csomagolja ki ezeket az adatokat külön tulajdonságok lehetővé teszi az olyan műveleteket, mint a rendezési és szűrési.

![Napló Keresés gomb](media/log-analytics-custom-fields/sample-extract.png)

> [!NOTE]
> Hello Preview, a korlátozott too100 egyéni mezőket a munkaterületen.  Ezt a határt kibontásra váró, amikor ez a szolgáltatás általánosan rendelkezésre álló eléri.
> 
> 

## <a name="creating-a-custom-field"></a>Egyéni mező létrehozása
Amikor létrehoz egy egyéni mező, Log Analyticshez meg kell érteniük mely adatok toouse toopopulate értéke.  Technológiát használja, akkor a Microsoft Research nevű FlashExtract tooquickly azonosíthatja ezeket az adatokat.  Ahelyett, hogy nincs szükség tooprovide utasításokat, Log Analyticshez értesül hello adatok kívánt tooextract az Ön által biztosított példák.

a következő szakaszok hello hello eljárás egyéni mező létrehozásához adjon meg.  Ez a cikk alján hello egy minta kivonása bemutatóért.

> [!NOTE]
> egyéni mező hello egyező hello megadott feltételek lettek hozzáadva toohello OMS-tárolót, így azt csak akkor jelenik meg az összegyűjtött hello egyéni mező létrehozása után rekordban bejegyzésként fel van töltve.  hello egyéni mező nem kerül toorecords, amelyek már hello adattár létrehozásakor.
> 
> 

### <a name="step-1--identify-records-that-will-have-hello-custom-field"></a>Az 1 – hello egyéni mező lesz rekordok azonosítása
hello első lépéseként tooidentify hello azt jelzi, hogy megkapja a hello egyéni mező értéke.  A kiindulási pont a [szabványos naplófájl-keresési](log-analytics-log-searches.md) majd válassza ki a rekord tooact hello modell, amely a Naplóelemzési bemutatja, hogyan.  Ha megadja, hogy az egyéni mező tooextract adatokat fog, hello **mező a varázsló** meg van nyitva, amelyen érvényesítése és pontosítsa hello feltételeket.

1. Nyissa meg túl**naplófájl-keresési** , és egy [tooretrieve hello rekordok lekérdezése](log-analytics-log-searches.md) , amely lesz hello egyéni mező.
2. Jelölje ki a bejegyzést, hogy a Naplóelemzési használja tooact modellként kibontott adatok toopopulate hello egyéni mező.  Azt szeretné, hogy a rekordból tooextract, és Naplóelemzési ezen információk toodetermine hello logika toopopulate hello egyéni mező fogja használni az összes hasonló rekordok hello adatok határozható meg.
3. Hello gomb toohello balra hello rekord, és válassza a text tulajdonságához kattintson **bontsa ki a mezőket**.
4. Hello **mező a varázsló már meg van nyitva**, és a kiválasztott hello bejegyzés megjelenik hello **fő példa** oszlop.  hello egyéni mező határozza meg a kijelölt hello tulajdonságok értékei azonos hello bejegyzéseket.  
5. Ha hello beállítás nem pontosan mit szeretne, válassza ki további mezőket toonarrow hello feltételeket.  Rendelés toochange hello mezőértékeket hello feltételek szakítsa meg és jelölje ki a kívánt hello feltételeknek megfelelő másik bejegyzést.

### <a name="step-2---perform-initial-extract"></a>2. lépés – hajtsa végre a kezdeti kivonatot.
Hello azt jelzi, hogy lesz hello egyéni mező állapította meg, Miután azonosította, amelyet az tooextract hello adatokat.  A Naplóelemzési hasonló rekordok ezen információk tooidentify hasonló minták használni fog.  Hello lépés ezt követően tudja toovalidate hello eredmények lehet, és nyújt további részleteket a elemzésben Naplóelemzési toouse.

1. Jelölje ki a megjeleníteni kívánt toopopulate hello egyéni mező hello minta rekordban hello szöveget.  Majd választhat a párbeszédpanel bezárásához tooprovide a hello mező és tooperform hello kezdeti kivonat nevét.  hello karakterek  **\_CF** automatikusan lesz hozzáfűzve.
2. Kattintson a **kibontása** tooperform összegyűjtött rekordok elemzését.  
3. Hello **összegzés** és **keresési eredmények** szakaszok hello kivonat hello eredmények megjelenítéséhez, így pontosságát vizsgálhatja meg.  **Összefoglalás** minden azonosított hello adatértékek hello feltételeket a tooidentify rekordok és a számát jeleníti meg.  **A keresési eredményekben** hello feltételeknek megfelelő rekordok részletes listáját tartalmazza.

### <a name="step-3--verify-accuracy-of-hello-extract-and-create-custom-field"></a>3. lépés – ellenőrizze hello kivonat pontosságát, és hozzon létre egyéni mező
Miután elvégezte a hello kezdeti kinyerési, Naplóelemzési megjeleníti az eredményeket már gyűjtött adatok alapján.  Ha hello eredmények keresse meg a pontos majd hozhat létre egyéni mező hello rendelkező nincs további feladata.  Ha nem, majd pontosíthatja hello eredményeit, hogy a Naplóelemzési javíthatja a logikája.

1. Ha minden egyes érték hello kezdeti kivonat nem megfelelő, majd kattintson a hello **szerkesztése** ikon következő tooan pontatlan rekord, és válassza **módosítása a kiemelési** rendelés toomodify hello kijelölés.
2. hello bejegyzés másolt toohello **további példákat** szakasz alatt hello **fő példa**.  Módosíthatja a hello kiemelési itt toohelp Naplóelemzési tisztában kell végzett hello kijelölés.
3. Kattintson a **kibontása** toouse az új adatokat tooevaluate összes hello meglévő rögzíti.  hello eredmények módosíthat egy csak módosító alapján ez az új eszközintelligencia hello eltérő rekordok.
4. Továbbra is tooadd javításokat, amíg minden rekordot hello kibontása megfelelően azonosíthatja hello adatok toopopulate hello új egyéni mező.
5. Kattintson a **mentése kibontása** Ha elégedett hello eredmények.  hello egyéni mező már meg van adva, de az nem adható hozzá tooany rekordok még.
6. Várjon, amíg az új rekordok megfelelő hello megadott feltételek toobe gyűjt, és futtassa újból a hello napló keresése. Új rekordok hello egyéni mező kell rendelkeznie.
7. Hello egyéni mező, mint bármely más rekord tulajdonság használható.  Adatok tooaggregate és csoportosítása használja, és még tooproduce új insights használják.

## <a name="viewing-custom-fields"></a>Egyéni mezők megtekintése
Minden egyéni mezők listáját megtekintheti a felügyeleti csoport hello a **beállítások** hello OMS irányítópult csempe.  Válassza ki **adatok** , majd **egyéni mezők** a munkaterületen lévő összes egyéni mezők listája.  

![Egyéni mezők](media/log-analytics-custom-fields/list.png)

## <a name="removing-a-custom-field"></a>Egyéni mező eltávolítása
Nincsenek két módon tooremove egyéni mező.  hello első az hello **eltávolítása** lehetőséget az egyes mezők, amikor megtekintik hello teljes listáját a fent leírt módon.  hello lesz egy rekord, és kattintson hello gomb toohello hello mező a bal oldali tooretrieve.  hello menü tooremove hello egyéni mező fog rendelkezni.

## <a name="sample-walkthrough"></a>A minta forgatókönyv
hello következő szakasz végigvezeti az egyéni mezők létrehozása átfogó példát.  Ez a példa kibontja hello szolgáltatás nevét az állapotváltás szolgáltatás jelző Windows-eseményeket.  Ez a Windows rendszerű számítógépeken lévő hello szolgáltatásvezérlő által létrehozott események támaszkodik.  Ha azt szeretné, toofollow ebben a példában, kell lennie [hello rendszernapló információk események gyűjtése](log-analytics-data-sources-windows-events.md).

Azt adja meg a következő lekérdezés tooreturn hello összes esemény szolgáltatásvezérlőtől 7036 egy Eseményazonosítót hello esemény, amely jelzi a szolgáltatás indítása és leállítása, amely rendelkező.

![Lekérdezés](media/log-analytics-custom-fields/query.png)

Azt bármely rekord event ID 7036, majd válassza ki.

![Forrás-rekord](media/log-analytics-custom-fields/source-record.png)

Azt szeretnénk, ha hello szolgáltatás hello megjelenő név **RenderedDescription** tulajdonságot, jelölje be hello gomb következő toothis tulajdonságot.

![Bontsa ki a mezők](media/log-analytics-custom-fields/extract-fields.png)

Hello **mező a varázsló** már meg van nyitva, és hello **EventLog** és **EventID** mezők vannak kijelölve a hello **fő példa** oszlop.  Ez azt jelzi, hogy hello egyéni mező határozza meg a rendszernaplóba hello 7036 Eseménynapló azonosítójú események.  Ez is használhatók, így nem szükséges tooselect más mezőket.

![Fő – példa](media/log-analytics-custom-fields/main-example.png)

A Microsoft hello hello társított szolgáltatás neve a hello jelöljön ki **RenderedDescription** tulajdonság és -felhasználási **szolgáltatás** tooidentify hello szolgáltatás neve.  hello egyéni mező neve **Service_CF**.

![Mezőnév](media/log-analytics-custom-fields/field-title.png)

Hello szolgáltatás neve, amelynél megfelelően az egyes rekordok, de mások számára nem látható.   Hello **keresési eredmények** hello hello nevét része megjelenítése **WMI Teljesítmény Adapter** nincs kiválasztva.  Hello **összegzés** mutat be, amely négy rögzíti a **DPRMA** szolgáltatás helytelenül része egy extra word és az azonosított két rekordok **modulok telepítője** helyett **Windows Installer-modulok**.  

![Keresési eredmények](media/log-analytics-custom-fields/search-results-01.png)

Először hello **WMI Teljesítmény Adapter** rekord.  A Szerkesztés ikonra elemre kattint, majd **módosítása a kiemelési**.  

![Kiemelési módosítása](media/log-analytics-custom-fields/modify-highlight.png)

A Microsoft hello kiemelési tooinclude hello word növelése **WMI** , majd futtassa újra a hello kivonatot.  

![További – példa](media/log-analytics-custom-fields/additional-example-01.png)

Láthatja, hogy hello bejegyzéseket **WMI Teljesítmény Adapter** javítását, és Naplóelemzési is, hogy információkat toocorrect hello rekordok **Windows Installer-modul**.  Láthatja a hello **összegzés** szakasz azonban, hogy **DPMRA** van még nem azonosított megfelelően.

![Keresési eredmények](media/log-analytics-custom-fields/search-results-02.png)

Görgessen tooa rekord hello DPMRA-szolgáltatást, azt, és ugyanezt a folyamatot, amely rögzíti toocorrect hello használata.

![További – példa](media/log-analytics-custom-fields/additional-example-02.png)

 Hello kibontási futtatásakor azt mutatják, hogy az eredmények összes most pontos.

![Keresési eredmények](media/log-analytics-custom-fields/search-results-03.png)

Láthatja, hogy **Service_CF** létrejött, de még nincs felvéve tooany rögzíti.

![Kezdeti száma](media/log-analytics-custom-fields/initial-count.png)

Után egy ideig, az új eseményeket a rendszer gyűjti, láthatja, hogy hello **Service_CF** mező most adja hozzá a feltételeknek eleget tevő toorecords.

![Végső eredmények](media/log-analytics-custom-fields/final-results.png)

Most már használhatja a Microsoft hello egyéni mező, mint bármely más rekord tulajdonság.  tooillustrate, nem csoportosítása hello új lekérdezést létrehozni **Service_CF** mező tooinspect szolgáltatások legtöbb aktív hello.

![Lekérdezés csoportosítás](media/log-analytics-custom-fields/query-group.png)

## <a name="next-steps"></a>Következő lépések
* További tudnivalók [keresések jelentkezzen](log-analytics-log-searches.md) toobuild lekérdezések egyéni mezők feltétel használatával.
* A figyelő [egyéni naplófájlok](log-analytics-data-sources-custom-logs.md) , amely elemezni a egyéni mezőkkel.

