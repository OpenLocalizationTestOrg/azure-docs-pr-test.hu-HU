---
title: "az Azure Media Indexer médiafájlok aaaIndexing"
description: "Az Azure Media Indexer lehetővé teszi a kereshető médiafájlok toomake tartalmát, és a teljes szöveges Beszélgetés szövegének toogenerate a feliratok és kulcsszavak lezárni. Ez a témakör bemutatja, hogyan toouse Media Indexer."
services: media-services
documentationcenter: 
author: Asolanki
manager: cfowler
editor: 
ms.assetid: 827a56b2-58a5-4044-8d5c-3e5356488271
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/20/2017
ms.author: adsolank;juliako;johndeu
ms.openlocfilehash: c1bed774e302e61ca3510668645dc2015b434a0c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="indexing-media-files-with-azure-media-indexer"></a>Az Azure Media Indexer médiafájlok indexelő
Az Azure Media Indexer lehetővé teszi a kereshető médiafájlok toomake tartalmát, és a teljes szöveges Beszélgetés szövegének toogenerate a feliratok és kulcsszavak lezárni. Feldolgozható egyetlen médiafájl is, vagy egy kötegben több médiafájl is.  

> [!IMPORTANT]
> Amikor indexelő tartalmat, győződjön meg arról, hogy toouse világossá beszéd (nélkül háttér zene, zaj, hatások vagy mikrofon hiss) rendelkező médiafájlok. Néhány példa a megfelelő tartalom: értekezletek, előadások vagy bemutatók rögzíti. hello következő tartalom nem feltétlenül alkalmas indexelő: filmek, tévéműsorok, semmi a vegyes hang- és hatást, rosszul rögzített háttérzaj (hiss) tartalmát.
> 
> 

Az indexelő feladat hozhat létre a következő kimenetek hello:

* Felirat fájlok a következő formátumok hello zárva: **SZÁMI**, **TTML**, és **WebVTT**.
  
    Feliratfájlokat-fájlok közé tartoznak a címke neve Recognizability, mely egy indexelő feladat alapján hogyan ismerhető hello beszéd hello forrás videó pontszámok van.  Használhatóság Recognizability tooscreen kimeneti fájlok hello érték használható. Alacsony pontszámmal jelentené rossz indexelési eredmények tooaudio minőségi miatt.
* Kulcsszó fájl (XML).
* Hang való használathoz az SQL server az indexelés (AIB) fájlját.
  
    További információkért lásd: [AIB fájlok használata az Azure Media Indexer és az SQL Server](https://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/).

Ez a témakör bemutatja, hogyan túl feladatok toocreate indexelő**egy eszköz Index** és **több fájl Index**.

Lásd: hello Azure Media Indexer legújabb [Media Services blogok](#preset).

## <a name="using-configuration-and-manifest-files-for-indexing-tasks"></a>Konfigurációs és a jegyzékfájl-fájlokat használ a indexelési feladatok
Az indexelő tevékenységek további részleteket adhat meg a feladat konfigurációja segítségével. Például a médiafájl mely metaadatok toouse is megadhat. A metaadatok a szóhasználatának hello nyelvi motor tooexpand használják, és nagy mértékben csökkenti a hello beszédfelismerés pontosságát.  Ön a kívánt kimeneti fájlokat képes toospecify is vannak.

Több médiafájlok egyszerre is feldolgozhat jegyzékfájlt segítségével.

További információkért lásd: [feladat az adott néven beállítás az Azure Media Indexer](https://msdn.microsoft.com/library/dn783454.aspx).

## <a name="index-an-asset"></a>Egy eszköz index
hello következő metódus feltölt egy médiafájlt eszközként, és létrehoz egy feladatot tooindex hello eszköz.

Vegye figyelembe, hogy ha nincs konfigurációs fájl meg van adva, hello médiafájl lesz indexelve alapértelmezett beállításokkal.

    static bool RunIndexingJob(string inputMediaFilePath, string outputFolder, string configurationFile = "")
    {
        // Create an asset and upload hello input media file toostorage.
        IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
            "My Indexing Input Asset",
            AssetCreationOptions.None);

        // Declare a new job.
        IJob job = _context.Jobs.Create("My Indexing Job");

        // Get a reference toohello Azure Media Indexer.
        string MediaProcessorName = "Azure Media Indexer";
        IMediaProcessor processor = GetLatestMediaProcessorByName(MediaProcessorName);

        // Read configuration from file if specified.
        string configuration = string.IsNullOrEmpty(configurationFile) ? "" : File.ReadAllText(configurationFile);

        // Create a task with hello encoding details, using a string preset.
        ITask task = job.Tasks.AddNew("My Indexing Task",
            processor,
            configuration,
            TaskOptions.None);

        // Specify hello input asset toobe indexed.
        task.InputAssets.Add(asset);

        // Add an output asset toocontain hello results of hello job.
        task.OutputAssets.AddNew("My Indexing Output Asset", AssetCreationOptions.None);

        // Use hello following event handler toocheck job progress.  
        job.StateChanged += new EventHandler<JobStateChangedEventArgs>(StateChanged);

        // Launch hello job.
        job.Submit();

        // Check job execution and wait for job toofinish.
        Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
        progressJobTask.Wait();

        // If job state is Error, hello event handling
        // method for job progress should log errors.  Here we check
        // for error state and exit if needed.
        if (job.State == JobState.Error)
        {
            Console.WriteLine("Exiting method due toojob error.");
            return false;
        }

        // Download hello job outputs.
        DownloadAsset(task.OutputAssets.First(), outputFolder);

        return true;
    }

    static IAsset CreateAssetAndUploadSingleFile(string filePath, string assetName, AssetCreationOptions options)
    {
        IAsset asset = _context.Assets.Create(assetName, options);

        var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
        assetFile.Upload(filePath);

        return asset;
    }

    static void DownloadAsset(IAsset asset, string outputDirectory)
    {
        foreach (IAssetFile file in asset.AssetFiles)
        {
            file.Download(Path.Combine(outputDirectory, file.Name));
        }
    }

    static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors
        .Where(p => p.Name == mediaProcessorName)
        .ToList()
        .OrderBy(p => new Version(p.Version))
        .LastOrDefault();

        if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor",
                                                       mediaProcessorName));

        return processor;
    }  
<!-- __ -->
### <a id="output_files"></a>Kimeneti fájlok
Alapértelmezés szerint egy indexelő feladat a következő kimeneti fájlok hello állít elő. hello első kimeneti adategységen hello fájlokat fogja tárolni.

Ha egynél több bemeneti médiafájlt, indexelő hello feladatot kimenetek "JobResult.txt" nevű jegyzékfájlokat hoz létre. Minden egyes bemeneti médiafájl hello eredményül kapott AIB, SZÁMI, TTML, WebVTT kulcsszó fájlok egymás után számozott és nevű, "Alias" hello segítségével.

| Fájlnév | Leírás |
| --- | --- |
| **InputFileName.aib** |Az indexelő blob hangfájl. <br/><br/> Hangfájl indexelő Blob (AIB) egy olyan bináris fájl, a Microsoft SQL server teljes szöveges keresés kereshető.  hello AIB fájl sokkal hatékonyabb, mint hello egyszerű felirat fájlok, mert minden egyes szó, így jóval szélesebb keresésekhez alternatívák tartalmaz. <br/> <br/>A számítógépen futó Microsoft SQL server 2008 vagy újabb hello indexelő SQL bővítmény hello telepítése szükséges hozzá. Keresés hello AIB használatával a Microsoft SQL server teljes szöveges keresés biztosít, mint a keresés hello lezárt WAMI által létrehozott felirat fájlok pontosabb keresési eredmények. Ennek az az oka hello AIB word alternatívák hasonló, mivel hello lezárt felirat fájlok hangkártya tartalmazhat hello legmagasabb abban, hogy word hello hang minden szakasza tartalmazza. Ha upmost fontos az szóbeli szavak keresése, majd ajánlott toouse hello AIB a együtt a Microsoft SQL Server.<br/><br/> toodownload hello bővítményt, kattintson a <a href="http://aka.ms/indexersql">Azure Media Indexer SQL bővítmény</a>. <br/><br/>Azt is lehetséges tooutilize más keresőprogramok, mint például a lezárt hello felirat és kulcsszó alapján Apache Lucene/Solr toosimply index hello videó XML-fájlokat, de ez kevésbé pontos keresés eredménye eredményez. |
| **InputFileName.smi**<br/>**InputFileName.ttml**<br/>**InputFileName.vtt** |Bezárt felirat (CC) fájlok SZÁMI TTML és WebVTT formátumban.<br/><br/>Használt toomake hang el, és a videó elérhető toopeople nyújtanak segítséget fogyatékkal fájlokat.<br/><br/>Lezárt felirat-fájlok közé tartoznak a címke neve <b>Recognizability</b> amely pontszámaihoz, hogyan ismerhető hello beszéd hello forrás videó alapján egy indexelő feladat van.  Használhatja a hello értékének <b>Recognizability</b> tooscreen kimeneti fájlok a használhatóság. Alacsony pontszámmal jelentené rossz indexelési eredmények tooaudio minőségi miatt. |
| **InputFileName.kw.xml<br/>InputFileName.info** |Kulcsszó és az adatok fájlokat. <br/><br/>A fájl kulcsszó gyakorisággal és oszlopeltolási információ hello beszéd tartalom, kinyert kulcsszavak tartalmazó XML-fájl. <br/><br/>A fájl adatait egy egyszerű szöveges fájl, amely minden kifejezéshez ismeri fel a részletes információkat tartalmaz. első sor hello különleges és hello Recognizability pontszám tartalmazza. Minden egyes őket követő sorban a következő adatok hello tabulátorral tagolt listáját: Indítsa el az idő, befejezési idő, szó vagy kifejezés, benne. hello időtartamok másodpercben és a hello abban, hogy egy számot a 0-1. <br/><br/>Példa sor: "1,20 1.45 word 0.67" <br/><br/>Számos célra, mint például a beszéd analytics tooperform, ezeket a fájlokat is használható, vagy például a Bing, a Google vagy a Microsoft SharePoint toomake hello médiafájlok felfedezését, vagy akár használt toodeliver motorok kitett toosearch több megfelelő ads. |
| **JobResult.txt** |Kimeneti jegyzék, jelenik meg, csak akkor, ha több fájl, a következő információk hello tartalmazó indexelő:<br/><br/><table border="1"><tr><th>InputFile</th><th>Alias</th><th>MediaLength</th><th>Hiba</th></tr><tr><td>a.mp4</td><td>Media_1</td><td>300</td><td>0</td></tr><tr><td>b.mp4</td><td>Media_2</td><td>0</td><td>3000</td></tr><tr><td>c.mp4</td><td>Media_3</td><td>600</td><td>0</td></tr></table><br/> |

Ha nem bemeneti médiafájljait sikeresen indexelt, hello indexelő feladat 4000 hibakóddal meghiúsul. További információkért lásd: [hibakódok](#error_codes).

## <a name="index-multiple-files"></a>Több fájl index
a következő metódus hello több médiafájlok feltölti eszközként, és létrehoz egy feladatot tooindex ezeket a fájlokat egy kötegben.

A jegyzékfájl hello .lst kiterjesztésű létrehozása és feltöltése hello objektumba. hello jegyzékfájl összes hello adategység-fájloknak hello listáját tartalmazza. További információkért lásd: [feladat az adott néven beállítás az Azure Media Indexer](https://msdn.microsoft.com/library/dn783454.aspx).

    static bool RunBatchIndexingJob(string[] inputMediaFiles, string outputFolder)
    {
        // Create an asset and upload toostorage.
        IAsset asset = CreateAssetAndUploadMultipleFiles(inputMediaFiles,
            "My Indexing Input Asset - Batch Mode",
            AssetCreationOptions.None);

        // Create a manifest file that contains all hello asset file names and upload toostorage.
        string manifestFile = "input.lst";            
        File.WriteAllLines(manifestFile, asset.AssetFiles.Select(f => f.Name).ToArray());
        var assetFile = asset.AssetFiles.Create(Path.GetFileName(manifestFile));
        assetFile.Upload(manifestFile);

        // Declare a new job.
        IJob job = _context.Jobs.Create("My Indexing Job - Batch Mode");

        // Get a reference toohello Azure Media Indexer.
        string MediaProcessorName = "Azure Media Indexer";
        IMediaProcessor processor = GetLatestMediaProcessorByName(MediaProcessorName);

        // Read configuration.
        string configuration = File.ReadAllText("batch.config");

        // Create a task with hello encoding details, using a string preset.
        ITask task = job.Tasks.AddNew("My Indexing Task - Batch Mode",
            processor,
            configuration,
            TaskOptions.None);

        // Specify hello input asset toobe indexed.
        task.InputAssets.Add(asset);

        // Add an output asset toocontain hello results of hello job.
        task.OutputAssets.AddNew("My Indexing Output Asset - Batch Mode", AssetCreationOptions.None);

        // Use hello following event handler toocheck job progress.  
        job.StateChanged += new EventHandler<JobStateChangedEventArgs>(StateChanged);

        // Launch hello job.
        job.Submit();

        // Check job execution and wait for job toofinish.
        Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
        progressJobTask.Wait();

        // If job state is Error, hello event handling
        // method for job progress should log errors.  Here we check
        // for error state and exit if needed.
        if (job.State == JobState.Error)
        {
            Console.WriteLine("Exiting method due toojob error.");
            return false;
        }

        // Download hello job outputs.
        DownloadAsset(task.OutputAssets.First(), outputFolder);

        return true;
    }

    private static IAsset CreateAssetAndUploadMultipleFiles(string[] filePaths, string assetName, AssetCreationOptions options)
    {
        IAsset asset = _context.Assets.Create(assetName, options);

        foreach (string filePath in filePaths)
        {
            var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
            assetFile.Upload(filePath);
        }

        return asset;
    }

### <a name="partially-succeeded-job"></a>Részlegesen sikeres feladat
Ha nem bemeneti médiafájljait sikeresen indexelt, hello indexelő feladat 4000 hibakóddal meghiúsul. További információkért lásd: [hibakódok](#error_codes).

(a sikeres feladat) azonos kimenetek hello akkor jönnek létre. Olvassa el a toohello kimeneti jegyzékfájl toofind, amely a bemeneti fájlokat nem sikerült, toohello hibaértékek oszlop szerint. Nem sikerült a bemeneti fájlok esetében eredményül kapott AIB, SZÁMI, TTML, WebVTT hello és kulcsszó fájlok nem lesz létrehozva.

### <a id="preset"></a>Az Azure Media indexelő feladat készlet
az Azure Media Indexer feldolgozása hello egy választható feladat hello feladat mellett az adott néven beállítás megadásával testreszabható.  hello következő szakasz ismerteti a konfigurációs xml hello formátumát.

| Név | Kötelező | Leírás |
| --- | --- | --- |
| **bemeneti** |hamis |Eszköz-fájl, amelyet az tooindex.</p><p>Az Azure Media Indexer támogatja a következő fájlformátumokat media hello: MP4, WMV, MP3, M4A, WMA, AAC, WAV.</p><p>Hello fájl neve (s) adhat meg hello **neve** vagy **lista** hello attribútumának **bemeneti** eleme (lásd alább). Ha nem adja meg a mely eszköz fájl tooindex, hello elsődleges fájl nek. Ha nem elsődleges eszköz fájl be van állítva, indexelt hello hello bemeneti eszköz első fájlt.</p><p>tooexplicitly hello eszköz nevét adja meg, tegye:<br/>`<input name="TestFile.wmv">`<br/><br/>Több eszköz fájl egyszerre (fájlokról too10) is elvégezheti az indexelést. toodo ezt:<br/><br/><ol class="ordered"><li><p>Hozzon létre egy szövegfájlt (jegyzékfájl), és adjon neki egy .lst bővítmény. </p></li><li><p>Adja hozzá a bemeneti eszköz toothis jegyzékfájl összes hello eszköz fájlnevek listáját. </p></li><li><p>Adja hozzá a (feltöltés) thanifest fájl toohello eszköz.  </p></li><li><p>Hello bemeneti attribútum hello jegyzékfájl hello nevét adja meg.<br/>`<input list="input.lst">`</li></ol><br/><br/>Megjegyzés: Ha több mint 10 fájlok toohello jegyzékfájl, hello indexelő feladat sikertelen lesz, és hello 2006 hibakód. |
| **metaadatok** |hamis |Hello metaadatai megadott objektum (ok) ban szóhasználatának kiigazítása használatos.  Hasznos tooprepare indexelő toorecognize nem szabványos szóhasználatának szavak például tulajdonnevek.<br/>`<metadata key="..." value="..."/>` <br/><br/>Megadhat olyan **értékek** az előre definiált **kulcsok**. Jelenleg a következő kulcsok hello támogatottak:<br/><br/>"title" és "description" - szóhasználatának kiigazítása tootweak hello nyelvvel használt modell a feladatot, és javíthatja a beszédfelismerés pontosságát.  hello értékek Internet keresések toofind összefüggéseikben való szöveget dokumentumok, hello tartalma tooaugment hello belső szótár hello ideje alatt a indexelő feladat magtípusú leképezést.<br/>`<metadata key="title" value="[Title of hello media file]" />`<br/>`<metadata key="description" value="[Description of hello media file] />"` |
| **szolgáltatások** <br/><br/> A hozzáadott 1.2-es verzióját. Csak a támogatott hello szolgáltatás jelenleg Beszédfelismerés ("automatikus"). |hamis |hello beszédfelismerés szolgáltatás a következő beállítások kulcsok hello rendelkezik:<table><tr><th><p>Kulcs</p></th>        <th><p>Leírás</p></th><th><p>Példaérték</p></th></tr><tr><td><p>Nyelv</p></td><td><p>hello természetes nyelvű toobe hello multimédiás fájlban ismerhető fel.</p></td><td><p>Angol, spanyol</p></td></tr><tr><td><p>CaptionFormats</p></td><td><p>egy listában pontosvesszővel elválasztva szükséges hello felirat formátumokban (ha van ilyen)</p></td><td><p>ttml; Számi; webvtt</p></td></tr><tr><td><p>GenerateAIB</p></td><td><p>Egy logikai jelző, adja meg, hogy-e egy AIB fájl szükséges (az SQL Server és hello ügyfél indexelő IFilter való használatra).  További információkért lásd: <a href="http://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/">AIB fájlok használata az Azure Media Indexer és az SQL Server</a>.</p></td><td><p>Igaz; Hamis</p></td></tr><tr><td><p>GenerateKeywords</p></td><td><p>Egy logikai jelző, adja meg, hogy-e a kulcsszó XML-fájl szükséges.</p></td><td><p>Igaz; Hamis. </p></td></tr><tr><td><p>ForceFullCaption</p></td><td><p>Egy logikai jelző, függetlenül attól, teljes tooforce feliratai (függetlenül a megbízhatósági szint) megadása.  </p><p>Alapértelmezett érték hamis, ebben az esetben szavak és kifejezések, amelyhez tartozik egy kisebb, mint 50 %-os megbízhatósági szintet kimarad a hello utolsó felirat kimenetek és szerepét a három pontot ("...").  hello folytatást jelző pontokra felirat minőség-ellenőrzési és a naplózási hasznosak.</p></td><td><p>Igaz; Hamis. </p></td></tr></table> |

### <a id="error_codes"></a>Hibakódok
Hiba hello esetben jelenteniük kell az Azure Media Indexer biztonsági hello a következő hibakódok egyikét:

| Kód | Név | Lehetséges okok |
| --- | --- | --- |
| 2000 |Érvénytelen konfiguráció |Érvénytelen konfiguráció |
| 2001 |Érvénytelen bemeneti eszközök |Hiányzik a bemeneti eszközök vagy üres eszköz. |
| 2002 |Érvénytelen jegyzék |Jegyzékfájl üres vagy jegyzék érvénytelen elemet tartalmaz. |
| 2003 |Nem sikerült toodownload médiafájl |URL-cím érvénytelen a jegyzékfájl. |
| 2004 |A protokoll nem támogatott |Media URL-címének protokoll nem támogatott. |
| 2005 |Nem támogatott fájltípus |Bemeneti media fájl típusa nem támogatott. |
| 2006 |Túl sok bemeneti fájl |10-nél több fájl van hello bemeneti jegyzékben. |
| 3000 |Nem sikerült toodecode médiafájl |Nem támogatott adathordozó kodek <br/>vagy<br/> Sérült médiafájl <br/>vagy<br/> Nincs hangadatfolyam a bemeneti adathordozóján található. |
| 4000 |Kötegelt indexelő részlegesen sikeres volt. |Néhány hello bemeneti adathordozó-fájlok nem indexelt toobe. További információkért lásd: <a href="#output_files">kimeneti fájlok</a>. |
| egyéb |Belső hiba |Lépjen kapcsolatba a terméktámogatási csapathoz. indexer@microsoft.com |

## <a id="supported_languages"></a>Támogatott nyelvek
Jelenleg hello angol és spanyol nyelven használható. További információkért lásd: [1.2-es verzió kiadás blogbejegyzés hello](https://azure.microsoft.com/blog/2015/04/13/azure-media-indexer-spanish-v1-2/).

## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>Kapcsolódó hivatkozások
[Azure Media Services elemző áttekintése](media-services-analytics-overview.md)

[Az Azure Media Indexer és az SQL Server AIB fájlok használata](https://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/)

[Az Azure Media Indexer 2 előzetes verziójú médiafájlok indexelő](media-services-process-content-with-indexer2.md)

