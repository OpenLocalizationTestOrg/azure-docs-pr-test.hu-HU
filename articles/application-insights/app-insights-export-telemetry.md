---
title: "az Application Insights telemetria aaaContinuous exportálása |} Microsoft Docs"
description: "Exportálja a diagnosztikai és használati adatok toostorage a Microsoft Azure-ban, és töltse le innen."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 5b859200-b484-4c98-9d9f-929713f1030c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 02/23/2017
ms.author: bwren
ms.openlocfilehash: be9ed7e05922c1c8186df9ca4e642862adaa5fd0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="export-telemetry-from-application-insights"></a>Az Application Insights telemetria exportálása
Szeretné, hogy tookeep a telemetriai adatok hello szabványos megőrzési időszaknál hosszabb ideig? Vagy valamilyen speciális módon dolgozza fel? A folyamatos exportálás ideális ehhez. megjelenik a hello Application Insights portál hello események JSON formátumban a Microsoft Azure exportált toostorage lehet. Ott töltse le az adatok és írási függetlenül kódját, tooprocess kell azt.  

Használja a folyamatos exportálás járhat egy kell külön fizetni. Ellenőrizze a [fizetési modell](http://azure.microsoft.com/pricing/details/application-insights/).

A folyamatos exportálás beállítása előtt van néhány más-érdemes tooconsider:

* egy metrikák vagy keresési panel felső hello hello Exportálás gomb lehetővé teszi átviteli táblázatokat és diagramokat tooan Excel-táblázat.

* [Elemzés](app-insights-analytics.md) hatékony lekérdezésnyelvet biztosít a telemetriai adatokat. Exportálhatja eredmények.
* Ha túl[az adatokba a Power BI](app-insights-export-power-bi.md), azt teheti a folyamatos exportálás használata nélkül.
* Hello [adatelérési REST API](https://dev.applicationinsights.io/) programozott hozzáférést a telemetriai adatok.

A folyamatos exportálás nem másolta az adatokat toostorage (amennyiben azt is marad mindaddig, amíg tetszés), akkor továbbra is használható az Application Insightsban szokásos hello [megőrzési időszak](app-insights-data-retention-privacy.md).

## <a name="setup"></a>A folyamatos exportálás létrehozása
1. A hello Application Insights-erőforrást az alkalmazáshoz, nyissa meg a folyamatos exportálás, és válassza **Hozzáadás**:

    ![Görgessen le, majd kattintson a folyamatos exportálás](./media/app-insights-export-telemetry/01-export.png)

2. Válassza ki a hello telemetriai adattípusok tooexport szeretné.

3. Hozzon létre vagy válasszon egy [Azure storage-fiók](../storage/common/storage-introduction.md) ahová toostore hello adatokat.

    > [!Warning]
    > Alapértelmezés szerint hello tárolási helye lesz beállítva, toohello és az Application Insights-erőforrás ugyanabban a földrajzi régióban. Ha egy másik régióban vannak tárolva, az átviteli további költségekkel járhat.

    ![Kattintson a Hozzáadás gombra, exportálás cél tárfiók, és majd létrehoz egy új vagy egy meglévő tároló kiválasztása](./media/app-insights-export-telemetry/02-add.png)

4. Hozzon létre, vagy válasszon egy tároló hello tárolóban:

    ![Válasszon eseménytípust kattintson](./media/app-insights-export-telemetry/create-container.png)

Ha létrehozta az exportálás, fog kezdődik. Csak get hello exportálási létrehozása után érkező adatokat.

Körülbelül egy óra alatt jelenik meg adat hello tárolási előtt késéssel is járhat.

### <a name="tooedit-continuous-export"></a>a folyamatos exportálás tooedit

Ha azt szeretné, hogy toochange hello eseménytípusok később, hello exportálás módosításával:

![Válasszon eseménytípust kattintson](./media/app-insights-export-telemetry/05-edit.png)

### <a name="toostop-continuous-export"></a>a folyamatos exportálás toostop

toostop hello exportálás kattintson letiltása. Engedélyezze újra gombra, a rendszer újraindítja a hello exportálási új adatokkal. Hello adatok érkező hello portálon exportálás le lett tiltva, amíg nem jelenik meg.

toostop hello exportálási véglegesen, törölje a parancsikont. Így nem törlése az adatok tárolására.

### <a name="cant-add-or-change-an-export"></a>Nem adható hozzá, vagy módosítsa az export?
* tooadd, vagy módosítsa azokat exportálja, tulajdonos, közreműködő vagy Application Insights közreműködői hozzáférési jogosultságok szükségesek. [További tudnivalók a szerepkörök][roles].

## <a name="analyze"></a>Milyen eseményeket kapott?
hello exportált adatok hello nyers telemetriaadatok az alkalmazásból érkező azzal a különbséggel, hogy a jelenleg a helyadatok, amelyek Microsoft hello ügyfél IP-címről érkező felvenni.

Adatok, amelyek szerint el lett vetve [mintavételi](app-insights-sampling.md) hello exportált adatok nem szerepel.

Más számított metrikák nem érhetők el. Például azt átlagos CPU-felhasználása ne exportálja, de exportálása hello nyers telemetriaadatok, amelyből hello átlagának kiszámítása történik.

hello adatokat is tartalmaz hello eredmények bármely [webteszt rendelkezésre állási](app-insights-monitor-web-app-availability.md) , amely meg van adva.

> [!NOTE]
> **Mintavételi.** Az alkalmazás nagy mennyiségű adatot küld, ha hello mintavételi szolgáltatás működik, és csak töredéke alatt létrehozott hello telemetriai adatot küldeni. [További tudnivalók a mintavételezésről.](app-insights-sampling.md)
>
>

## <a name="get"></a>Hello adatok vizsgálata
Közvetlenül a hello portál hello tárolási vizsgálhatja meg. Kattintson a **Tallózás**, válassza ki a tárfiók, és nyisson **tárolók**.

Nyissa meg a Visual studióban, az Azure storage tooinspect **nézet**, **Cloud Explorer**. (Még nem rendelkezik az adott parancs, ha szüksége van-e tooinstall hello Azure SDK: nyitott hello **új projekt** párbeszédpanelen bontsa ki a Visual C# / felhőalapú, és válassza **Microsoft Azure SDK for .NET**.)

A blob-tároló megnyitásakor látni fogja a blob-fájlokat a tárolóban. az egyes fájlok URI hello az Application Insights-erőforrás nevét, a instrumentation kulcs, a telemetria-típus vagy dátum/idő származik. (hello erőforrásnév kisbetű szerepelhet, és hello instrumentation kulcs kihagyja a kötőjel.)

![Vizsgálja meg a megfelelő eszköz hello blob tároló](./media/app-insights-export-telemetry/04-data.png)

hello dátuma és időpontja (UTC) és amikor hello telemetriai lett letétbe hello tárolójában – hello idő nem hozta létre. Így ha kód toodownload hello adatok írása, áthelyezheti azt lineárisan hello adatok között.

Hello űrlap hello elérési út a következő:

    $"{applicationName}_{instrumentationKey}/{type}/{blobDeliveryTimeUtc:yyyy-MM-dd}/{ blobDeliveryTimeUtc:HH}/{blobId}_{blobCreationTimeUtc:yyyyMMdd_HHmmss}.blob"

Ha

* `blobCreationTimeUtc`van blob létrehozásának időpontja a hello belső átmeneti tárolási
* `blobDeliveryTimeUtc`hello időpontot, amikor blob másolt toohello exportálási destination tároló van

## <a name="format"></a>Az adatformátum
* Minden egyes blob egy szövegfájlt, amely több tartalmazza az "\n'-separated sorokat. Körülbelül fél perc időszakra feldolgozott hello telemetriai adatokat tartalmaz.
* Minden egyes sorára például egy kérelem vagy a lap nézet telemetriai adatpont jelöli.
* Minden egyes sorára nem formázott JSON-dokumentumhoz. Ha toosit és, hogy stare, nyissa meg a Visual Studio és az válassza szerkesztése, a speciális formátumú fájlba:

![Nézet hello telemetriai megfelelő eszközzel](./media/app-insights-export-telemetry/06-json.png)

Idő időtartamok vannak ütés, ahol a 10 000-re ölések = 1 MS. Például ezek az értékek megjelenítése a idő 1 MS toosend hello böngészőből 3ms kérelmet tooreceive és 1.8s tooprocess hello lapon hello böngészőben:

    "sendRequest": {"value": 10000.0},
    "receiveRequest": {"value": 30000.0},
    "clientProcess": {"value": 17970000.0}

[Részletes adatok modellhez tartozó referencia hello Tulajdonságtípusok és értékeket.](app-insights-export-data-model.md)

## <a name="processing-hello-data"></a>Hello adatainak feldolgozása
Kis méretű az adatok írása az egyes kód toopull egymástól, olvassa el, egy számolótáblába, és így tovább. Példa:

    private IEnumerable<T> DeserializeMany<T>(string folderName)
    {
      var files = Directory.EnumerateFiles(folderName, "*.blob", SearchOption.AllDirectories);
      foreach (var file in files)
      {
         using (var fileReader = File.OpenText(file))
         {
            string fileContent = fileReader.ReadToEnd();
            IEnumerable<string> entities = fileContent.Split('\n').Where(s => !string.IsNullOrWhiteSpace(s));
            foreach (var entity in entities)
            {
                yield return JsonConvert.DeserializeObject<T>(entity);
            }
         }
      }
    }

A nagyobb kódminta, lásd: [feldolgozói szerepkörök használatával][exportasa].

## <a name="delete"></a>A régi adatok törlése
Vegye figyelembe, hogy Ön felelősséggel tartozik a tárolási kapacitás kezelése és hello régi adatok törlése, ha szükséges.

## <a name="if-you-regenerate-your-storage-key"></a>Ha a tárolási kulcs újragenerálása...
Hello kulcs tooyour tárolási módosítása esetén a folyamatos exportálás nem fog tovább működni. Az Azure-fiókjába, megjelenik egy értesítés.

Hello a folyamatos exportálás panel megnyitásához, és az Exportálás szerkesztése. Hello exportálása cél szerkesztése, de ne változtassa meg ugyanazt a tárhelyet kijelölt hello. Kattintson az OK tooconfirm.

![Szerkesztés hello folyamatos exportálása, nyissa meg, majd zárja be, ezekkel az exportálási cél.](./media/app-insights-export-telemetry/07-resetstore.png)

a folyamatos exportálás hello újraindul.

## <a name="export-samples"></a>Exportálás minták

* [Használja a Stream Analytics tooSQL exportálása][exportasa]
* [Stream Analytics minta 2](app-insights-export-stream-analytics.md)

Nagyobb skálán, érdemes lehet [HDInsight](https://azure.microsoft.com/services/hdinsight/) -Hadoop-fürtök hello felhőben. HDInsight számos technológiák kezelése, és a big Data típusú adatok elemzésére, és használhatja azt az Application Insights exportált tooprocess adatokat.

## <a name="q--a"></a>Kérdések és válaszok
* *Azonban az összes kívánt egyszeri letölteni egy diagram.*  

    Igen, akkor teheti meg. Hello hello panel felső részén kattintson **adatok exportálása**.
* *Exportálás beállítva, de nincs adat a saját tárolóban.*

    Az Application Insights kapta meg összes telemetriai adat az alkalmazás óta hello exportálás beállítása? Csak kap új adatokat.
* *I tooset Exportálás mentése próbált, de a rendszer megtagadta a hozzáférést*

    Ha hello fiókot a szervezete tulajdonában van, akkor toobe hello tulajdonos vagy közreműködő szerepkörrel rendelkező személyek csoport tagja.
* *Exportálhatja a saját helyszíni tárolási egyenes toomy?*

    Nem, sajnos. Az Exportálás motor jelenleg csak akkor működik az Azure storage most.  
* *Bármely korlát toohello mennyiségű adat kerüljön a saját tárolóban van?*

    Nem. Azt fogja tartani kérdez le adatokat hello exportálási törléséig. Ha azt találati hello külső korlátozhatja a blob Storage tárolóban, de ez közérthető hatalmas lesz leállítása. Az működik tooyou toocontrol mennyi tárhelyre használja.  
* *Hány blobok kell lásd hello Storage?*

  * Az összes adattípus kijelölt tooexport, egy új blob létrejön percenként (ha érhetők el adatok).
  * Emellett nagy forgalmú alkalmazások esetén további partíció egységek foglal le. Ebben az esetben tárolóegységekhez hoz létre egy blobot percenként.
* *I hello kulcs toomy tárolási újragenerálása vagy hello hello tároló neve megváltozott, és hello exportálás nem működik.*

    Hello exportálás módosításával és hello exportálási cél panel megnyitásához. Ugyanazt a tárhelyet választott azt korábban hello hagyja, és kattintson az OK tooconfirm. Exportálás újraindul. Ha hello módosítása néhány napok hello belül volt, adatok nem vesznek el.
* *Szüneteltetheti hello exportálás is?*

    Igen. Kattintson a letiltása.

## <a name="code-samples"></a>Kódminták

* [Stream Analytics-minta](app-insights-export-stream-analytics.md)
* [Használja a Stream Analytics tooSQL exportálása][exportasa]
* [Részletes adatok modellhez tartozó referencia hello Tulajdonságtípusok és értékeket.](app-insights-export-data-model.md)

<!--Link references-->

[exportasa]: app-insights-code-sample-export-sql-stream-analytics.md
[roles]: app-insights-resources-roles-access-control.md
