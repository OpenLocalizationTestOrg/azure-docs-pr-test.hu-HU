---
title: "egyéni aaaCollect naplózza az OMS szolgáltatáshoz |} Microsoft Docs"
description: "A Naplóelemzési képes eseményeket gyűjteni szöveges fájlt a Windows és Linux rendszerű számítógépeken.  Ez a cikk ismerteti, hogyan toodefine egy új egyéni napló és a részletek a hello rekordok hoznak létre a hello OMS-tárházban."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: aca7f6bb-6f53-4fd4-a45c-93f12ead4ae1
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/15/2017
ms.author: bwren
ms.openlocfilehash: a75d058c371fecf7c43690a797d4e650c1570317
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="custom-logs-in-log-analytics"></a>A Naplóelemzési egyéni naplókat
hello egyéni naplói adatforrás Naplóelemzési lehetővé teszi a Windows és Linux számítógépeken egyaránt szövegfájlból toocollect események. Számos alkalmazás naplófájlok információk tootext szabványos naplózási szolgáltatások, például a Windows Eseménynapló vagy a Syslog helyett.  Összegyűjtését követően elemezni a rekordokban lévő hello hello segítségével tooindividual mezők [egyéni mezők](log-analytics-custom-fields.md) Naplóelemzési szolgáltatása.

![Egyéni naplógyűjtést](media/log-analytics-data-sources-custom-logs/overview.png)

hello napló fájlok toobe gyűjtött egyeznie kell a következő feltételek hello.

- hello napló kell telepítse egy soronként egy bejegyzést, vagy használja a timestamp megfelelő hello következő formázza a leírásokban hello elején.

    ÉÉÉÉ-HH-NN ÓÓ: PP:<br>H/ÉÉÉÉ ÓÓ: PP: MP DE/DU <br>F nn, éééé óó: pp:

- hello naplófájl nem engedélyezheti a körkörös frissítések ahol hello fájlt felülírja a rendszer az új bejegyzések.
- hello naplófájl ASCII vagy UTF-8 kódolást kell használnia.  Más például az UTF-16 formátum nem támogatott.

>[!NOTE]
>Ha ne legyenek ismétlődő bejegyzések hello naplófájlban, Log Analyticshez gyűjt őket.  Azonban hello keresési eredmények lesz inkonzisztens ahol hello szűrő eredményben hello eredményszám-nál több esemény.  Szüksége lesz hello napló toodetermine ellenőrzi, hogy azt létrehozó hello alkalmazást okozza ezt a viselkedést, és ha lehetséges cím hello egyéni gyűjtemény naplódefiníciója létrehozása előtt.  
>
  
## <a name="defining-a-custom-log"></a>Egy egyéni napló meghatározása
A következő eljárás toodefine egyéni naplófájl hello használata.  Ez a cikk útmutatást a minta egy egyéni napló hozzáadásának görgetési toohello végét.

### <a name="step-1-open-hello-custom-log-wizard"></a>1. lépés Nyissa meg a hello egyéni napló varázsló
Egyéni napló varázsló hello hello OMS-portálon fut, és lehetővé teszi egy új egyéni napló toocollect toodefine.

1. Hello OMS-portálon lépjen túl**beállítások**.
2. Kattintson a **adatok** , majd **egyéni naplói**.
3. Alapértelmezés szerint az összes konfigurációs módosításhoz automatikusan vannak leküldött tooall ügynökök.  Linux-ügynökök, a konfigurációs fájl toohello Fluentd adatgyűjtő küldött.  Ha a toomodify ezt a fájlt manuálisan minden egyes Linux-ügynök, majd törölje a jelet hello mezőben *alábbi konfigurációs toomy Linux számítógépek alkalmaz*.
4. Kattintson a **Add +** tooopen hello egyéni napló varázsló.

### <a name="step-2-upload-and-parse-a-sample-log"></a>2. lépés Töltse fel, és elemezni egy mintanaplót
Hello egyéni napló minta feltöltésével megkezdése.  hello varázsló elemzése és a fájlban, az Ön toovalidate hello bejegyzések megjeleníteni.  A Naplóelemzési meg kell adnia tooidentify rekordokban hello elválasztó fogja használni.

**Új sor** hello alapértelmezett elválasztó és a naplófájlokat, amelyek egy-egy bejegyzésnek soronként használt.  Ha hello sor kezdődik-e a dátum és idő hello formátumok egyikében, akkor megadhatja egy **időbélyeg** szövegelválasztó, amely támogatja a bejegyzéseket, amelyek több egynél több sort.

Ha időbélyeg elválasztó szolgál, majd az OMS tárolt rekordokat hello TimeGenerated tulajdonsága hello dátum/idő az adott bejegyzés hello naplófájlban megadott tölti fel.  Ha egy új sor elválasztó használja, majd TimeGenerated a telepítéskor dátum és idő, hogy a Naplóelemzési gyűjtött hello bejegyzés.

> [!NOTE]
> A Naplóelemzési jelenleg hello dátum/idő a napló egy Timestamp típusú elválasztó használatával időobjektumot UTC időzóna szerint gyűjtött kezeli.  Ez hamarosan módosított toouse hello időzóna hello ügynökön.
>
>

1. Kattintson a **Tallózás** , és keresse meg a tooa mintafájl.  Vegye figyelembe, hogy ez lehetséges, hogy gomb neve lehet **Choose File** egyes böngészőkben.
2. Kattintson a **Tovább** gombra.
3. hello egyéni napló varázslóban fel kell töltenie hello fájl- és hello azt jelzi, hogy azonosítja.
4. Hello szövegelválasztó, amely egy új rekordot használt tooidentify és legjobb azonosító hello rögzíti a naplófájl válassza hello elválasztó módosítása.
5. Kattintson a **Tovább** gombra.

### <a name="step-3-add-log-collection-paths"></a>3. lépés Adja hozzá a Naplógyűjtemények elérési útját
Meg kell adnia egy vagy több elérési utak hello ügynökön, ahol megtalálja a hello egyéni naplót.  Vagy megadhatja egy adott elérési útját és hello naplófájl nevét, vagy egy elérési utat is megadhat a hello neve helyettesítő karakter.  Ez a funkció támogatja alkalmazásokat, amelyek minden nap, vagy ha egy fájl elér egy adott méretet, hozzon létre egy új fájlt.  Több útvonal egy naplófájlt is biztosítható.

Például egy alkalmazás előfordulhat, hogy dátummal hozza létre a naplófájl minden nap hello hello neve, ahogy log20100316.txt tartalmazza. Előfordulhat, hogy az ilyen naplók minta *napló\*.txt* akkor vonatkozik, amely tooany naplófájl hello alkalmazás a következő tartozó elnevezési sémát.

hello következő táblázat érvényes minták toospecify különböző naplófájlokat.

| Leírás | Elérési út |
|:--- |:--- |
| Az összes fájl *C:\Logs* .txt kiterjesztésű, a Windows-ügynök |C:\Logs\\\*.txt |
| Az összes fájl *C:\Logs* napló és a Windows-ügynök a .txt kiterjesztésű neve |C:\Logs\log\*.txt |
| Az összes fájl */var/log/audit* .txt kiterjesztésű, a Linux-ügynök |/var/log/audit/*.txt |
| Az összes fájl */var/log/audit* napló és a Linux-ügynök a .txt kiterjesztésű neve |/var/log/audit/log\*.txt |

1. Válassza ki a Windows vagy Linux toospecify elérési út formátumot ad hozzá.
2. Írja be a hello elérési útja, és kattintson a hello  **+**  gombra.
3. Ismételje meg a hello folyamat egyik további elérési úton.

### <a name="step-4-provide-a-name-and-description-for-hello-log"></a>4. lépés Adjon nevet és leírást hello napló
hello a megadott használandó hello típussal fent leírt módon.  Azt mindig végződik _CL toodistinguish, mint egy egyéni naplót.

1. Adjon meg egy nevet az hello naplók.  Hello  **\_CL** utótag automatikusan elérhető.
2. Adja hozzá egy nem kötelező **leírás**.
3. Kattintson a **következő** toosave hello egyéni naplódefiníciója.

### <a name="step-5-validate-that-hello-custom-logs-are-being-collected"></a>5. lépés Hello egyéni naplókat gyűjtik ellenőrzése
Egy új egyéni napló tooappear Naplóelemzési a kezdeti adatok hello tooan órát igénybe vehet.  Bejegyzések begyűjtése hello fog elindulni hello elérési úton található napló a megadott hello pontról meghatározott hello egyéni bejelentkeznie.  Nem megtartja a hello bejegyzések feltöltött hello egyéni napló létrehozása során, de azt hello naplófájlokat, amely azt a már meglévő bejegyzéseket gyűjt.

Naplóelemzési elindul hello egyéni napló begyűjtése, a rekordok állnak rendelkezésre a napló keresés.  Használja a hello neve, mint hello egyéni naplót, hello **típus** a lekérdezésben.

> [!NOTE]
> Ha hello keresési hello RawData tulajdonság hiányzik, tooclose kell, és nyissa meg újra a böngészőt.
>
>

### <a name="step-6-parse-hello-custom-log-entries"></a>6. lépés Hello egyéni naplóbejegyzések elemzése
hello teljes naplóbejegyzés fogja tárolni egy tulajdonságot, **RawData**.  Valószínűleg érdemes tooseparate hello különböző adatra hello rekordban tárolt egyes tulajdonságokat az egyes bejegyzések.  Ehhez használja a hello [egyéni mezők](log-analytics-custom-fields.md) Naplóelemzési szolgáltatása.

Nincsenek megadva itt részletes, lépésenkénti leírását hello egyéni naplóbejegyzés elemzésekor.  Tekintse meg a toohello [egyéni mezők](log-analytics-custom-fields.md) ezt az információt dokumentációját.

## <a name="disabling-a-custom-log"></a>Egy egyéni napló letiltása
Egy egyéni naplódefiníciója nem távolítható el, ha van létrejött, de letilthatja eltávolítja az összes a Naplógyűjtemények elérési útját.

1. Hello OMS-portálon lépjen túl**beállítások**.
2. Kattintson a **adatok** , majd **egyéni naplói**.
3. Kattintson a **részletek** definition toodisable következő toohello egyéni naplót.
4. Távolítsa el az egyes hello egyéni naplódefiníciója hello Naplógyűjtemények elérési útját.

## <a name="data-collection"></a>Adatgyűjtés
A Naplóelemzési összegyűjti az új bejegyzések minden egyéni naplóból körülbelül 5 percenként.  hello ügynök minden az összegyűjtő naplófájlban rögzítik a helyére.  Ha hello ügynök egy ideig offline állapotba kerül, majd Naplóelemzési bejegyzések az gyűjtenek ahol utolsó abbamaradtak, akkor is, ha a tételekhez hozta létre, miközben hello ügynök offline állapotban volt.

hello teljes tartalma hello naplóbejegyzés írt tooa egyetlen tulajdonság nevű **RawData**.  Ez az elemzett és meghatározásával külön keresés több tulajdonságok tudja értelmezni [egyéni mezők](log-analytics-custom-fields.md) hello egyéni napló létrehozása után.

## <a name="custom-log-record-properties"></a>Egyéni napló rekord tulajdonságai
Egyéni naplórekordokat tartozhat hello napló nevű, adja meg, és a következő táblázat hello tulajdonságok hello.

| Tulajdonság | Leírás |
|:--- |:--- |
| TimeGenerated |Dátum és idő rekord hello Naplóelemzési által összegyűjtött.  Ha hello napló időalapú elválasztó használ majd ez az összegyűjtött hello bejegyzésből hello idő. |
| SourceSystem |Ügynök hello rekord típusa történt gyűjtött. <br> OpsManager – Windows-ügynök, vagy közvetlen kapcsolódás vagy a System Center Operations Manager <br> Linux – az összes Linux-ügynökök |
| RawData |Hello teljes szövege gyűjtött bejegyzés. |
| ManagementGroupName |A System Center Operations kezelése ügynökök hello felügyeleti csoport neve.  Más ügynökök, ez pedig AOI -\<munkaterület azonosítója\> |

## <a name="log-searches-with-custom-log-records"></a>Napló keresést egyéni naplórekordokat:
Az egyéni naplókat rekordok hello OMS tárház csakúgy, mint bármely más adatforrás bejegyzéseit tárolódnak.  A típus, amely megadja, hogy használhassa a keresési tooretrieve bejegyzések egy adott napló gyűjtött hello Type tulajdonság hello napló definiálásakor hello névnek megfelelő rendelkeznek.

hello következő táblázat példákat különböző napló kereséseket rekordok lekérése az egyéni naplókat.

| Lekérdezés | Leírás |
|:--- |:--- |
| Típus = MyApp_CL |Az egyéni összes eseménynaplózás elnevezett MyApp_CL. |
| Típus MyApp_CL Severity_CF = hiba = |Minden egyéni az eseménynaplózás elnevezett MyApp_CL értékkel rendelkező *hiba* nevű egyéni mező *Severity_CF*. |

>[!NOTE]
> Ha a munkaterületet frissített toohello [új Log Analytics lekérdezési nyelv](log-analytics-log-search-upgrade.md), majd a fenti lekérdezések hello megváltozna toohello következő.

> | Lekérdezés | Leírás |
|:--- |:--- |
| MyApp_CL |Az egyéni összes eseménynaplózás elnevezett MyApp_CL. |
| MyApp_CL &#124; Ha Severity_CF == "error" |Minden egyéni az eseménynaplózás elnevezett MyApp_CL értékkel rendelkező *hiba* nevű egyéni mező *Severity_CF*. |


## <a name="sample-walkthrough-of-adding-a-custom-log"></a>Egy egyéni napló hozzáadásának minta forgatókönyv
hello következő szakasz végigvezeti egy egyéni napló létrehozására láthat példát.  gyűjtenek hello mintanaplót soronként egy dátumot és időt, majd majd vesszővel tagolt mezők kódja, állapotát és üzenet kezdve egy-egy bejegyzésnek rendelkezik.  Több minta bejegyzések alább látható.

    2016-03-10 01:34:36 207,Success,Client 05a26a97-272a-4bc9-8f64-269d154b0e39 connected
    2016-03-10 01:33:33 208,Warning,Client ec53d95c-1c88-41ae-8174-92104212de5d disconnected
    2016-03-10 01:35:44 209,Success,Transaction 10d65890-b003-48f8-9cfc-9c74b51189c8 succeeded
    2016-03-10 01:38:22 302,Error,Application could not connect toodatabase
    2016-03-10 01:31:34 303,Error,Application lost connection toodatabase

### <a name="upload-and-parse-a-sample-log"></a>Töltse fel, és elemezni egy mintanaplót
A Microsoft hello naplófájlok egyikét adja meg, és hello események gyűjtése kell lesz látható.  Ebben az esetben az új sor elegendő elválasztó.  Ha egy-egy bejegyzésnek hello naplóba, ha sikerült át több sorra, timestamp elválasztó használt toobe kellene.

![Töltse fel, és elemezni egy mintanaplót](media/log-analytics-data-sources-custom-logs/delimiter.png)

### <a name="add-log-collection-paths"></a>Adja hozzá a Naplógyűjtemények elérési útját
hello naplófájlok találhatók *C:\MyApp\Logs*.  Egy új fájl, amely tartalmazza az hello dátum hello mintában néven naponta jön létre *appYYYYMMDD.log*.  Ez a napló az elegendő mintázatát lenne *C:\MyApp\Logs\\\*.log*.

![Naplófájl elérési útvonala](media/log-analytics-data-sources-custom-logs/collection-path.png)

### <a name="provide-a-name-and-description-for-hello-log"></a>Adjon nevet és leírást hello napló
Egy nevét használjuk *MyApp_CL* , és írja be a **leírás**.

![Napló neve](media/log-analytics-data-sources-custom-logs/log-name.png)

### <a name="validate-that-hello-custom-logs-are-being-collected"></a>Hello egyéni naplókat gyűjtik ellenőrzése
A lekérdezés használjuk *típus = MyApp_CL* összes tooreturn hello összegyűjtött eseménynapló rögzíti.

![Egyéni mezők napló lekérdezés](media/log-analytics-data-sources-custom-logs/query-01.png)

### <a name="parse-hello-custom-log-entries"></a>Hello egyéni naplóbejegyzések elemzése
Egyéni mezők toodefine hello használjuk *EventTime*, *kód*, *állapot*, és *üzenet* mezőket, és segítünk látható hello hello különbség hello lekérdezés által visszaadott rekordok.

![Napló lekérdezés egyéni mezőkkel](media/log-analytics-data-sources-custom-logs/query-02.png)

## <a name="next-steps"></a>Következő lépések
* Használjon [egyéni mezők](log-analytics-custom-fields.md) tooparse hello bejegyzések hello egyéni napló tooindividual mezőket.
* További tudnivalók [keresések jelentkezzen](log-analytics-log-searches.md) tooanalyze hello adatokat gyűjteni az adatforrások és megoldásokat.
