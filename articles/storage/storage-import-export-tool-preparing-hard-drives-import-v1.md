---
title: "az Azure Import/Export aaaPreparing merevlemezek importálási feladat - 1-es verzió |} Microsoft Docs"
description: "Ismerje meg, hogyan tooprepare merevlemez-meghajtókat használ hello WAImportExport V1-es eszköz toocreate az importálási feladat hello Azure Import/Export szolgáltatás."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 3d818245-8b1b-4435-a41f-8e5ec1f194b2
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2017
ms.author: muralikk
ms.openlocfilehash: 8803ac01b7c7a2ec2e3199231d7b4c04ceafd5a7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="preparing-hard-drives-for-an-import-job"></a>Merevlemezek előkészítése importálási feladatokhoz
tooprepare egy vagy több merevlemezeit, az importálási feladat, kövesse az alábbi lépéseket:

-   A Blob szolgáltatás hello hello adatok tooimport azonosítása

-   Azonosítsa a cél virtuális könyvtárak és blobot, amely hello Blob szolgáltatás

-   Határozza meg, hány meghajtót kell

-   Másolja a hello adatok tooeach merevlemezen

 Lásd: a minta-munkafolyamat [minta munkafolyamat tooPrepare, hogy az importálás merevlemezek](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow-v1.md).

## <a name="identify-hello-data-toobe-imported"></a>Hello adatok toobe importált azonosítása
 hello első lépés toocreating az importálási feladat toodetermine mely könyvtárak és fájlok tooimport fog. Ez lehet könyvtárak listája, egyedi fájlok listáját, vagy ezeket a kettő kombinációja. Része egy könyvtárat, ha akkor hello könyvtárban és annak alkönyvtáraiban található összes fájl hello importálási feladat része lesz.

> [!NOTE]
>  Mivel alkönyvtárak belefoglalt rekurzív módon ha egy szülőkönyvtárában megtalálható, adja meg, csak a hello szülőkönyvtárat. Nem is megadhatja alkönyvtáraiban.
>
>  Jelenleg a Microsoft Azure Import/Export eszköz hello rendelkezik hello korlátozása a következő: Ha egy könyvtárat a merevlemez-meghajtóról tartalmazhat több adatot tartalmaz, hello directory kell toobe kisebb könyvtárak lebontva. Például ha egy könyvtárban található 2,5 TB adatok és hello merevlemez kapacitása csak 2TB, akkor toobreak hello 2,5 TB directory kisebb könyvtárak be kell. Ez a korlátozás kiiktatása a hello eszköz újabb verziója.

## <a name="identify-hello-destination-locations-in-hello-blob-service"></a>Hello rendeltetési helyeket hello blob szolgáltatásban azonosítása
 Minden fájl vagy könyvtár importálni kívánt meg kell tooidentify egy cél virtuális könyvtárat vagy hello Azure Blob szolgáltatást a blob. Ezeken a célokon bemenetek toohello Azure Import/Export eszköz adattárként fog használni. Vegye figyelembe, hogy a könyvtárak hello perjel karakterrel kell tagolva "/".

 a következő táblázat hello blob tárolók néhány példát látható:

|Forrás-fájl vagy könyvtár|Cél blob vagy virtuális könyvtár|
|------------------------------|-------------------------------------------|
|H:\Video|https://mystorageaccount.BLOB.Core.Windows.NET/video|
|H:\Photo|https://mystorageaccount.BLOB.Core.Windows.NET/Photo|
|K:\Temp\FavoriteVideo.ISO|https://mystorageaccount.BLOB.Core.Windows.NET/Favorite/FavoriteVideo.ISO|
|\\\myshare\john\music|https://mystorageaccount.BLOB.Core.Windows.NET/Music|

## <a name="determine-how-many-drives-are-needed"></a>Határozza meg, hány meghajtót van szükség
 A következő lépésben toodetermine:

-   a szükséges toostore hello adatok merevlemezek hello száma.

-   hello könyvtárak és/vagy az önálló fájlok, amelyek a merevlemez másolt tooeach lesznek.

 Győződjön meg arról, hogy rendelkezik-e szüksége lesz a toostore hello adatok viszi merevlemezek hello száma.

## <a name="copy-data-tooyour-hard-drive"></a>Másolja az adatokat tooyour merevlemez-meghajtó
 Ez a szakasz ismerteti, hogyan toocall hello-e Azure Import/Export eszköz toocopy az adatok tooone vagy további merevlemez-meghajtókat. Minden alkalommal meghívja a hello Azure Import/Export eszköz, hozzon létre egy új *munkamenet másolása*. Legalább egy másolási munkamenetet létrehozni az egyes meghajtó toowhich a Másolás; egyes esetekben szükség lehet a egynél több példány munkamenet toocopy összes toosingle meghajtóval. Az alábbiakban néhány ok, amiért az, hogy szükség lehet a több példány munkamenetet:

-   Létre kell hoznia egy külön példányát munkamenet minden meghajtó, amelyet másol.

-   A másolási munkamenet másolhatja, vagy egy könyvtár, vagy egy egyetlen blob toohello meghajtó. Ha több címtárral, több blobok vagy mindkettőt másolni, szüksége lesz toocreate több példány munkamenetet.

-   Tulajdonságok és a metaadatok importálása az importálási feladat részeként hello blobok beállított is megadhat. hello tulajdonságok vagy a metaadatokat, amelyek másolási munkamenet megadása tooall BLOB másolási munkamenet által meghatározott fog vonatkozni. Néhány BLOB toospecify más tulajdonságokkal vagy metaadatok használni szeretne, ha egy külön másolása munkamenet toocreate lesz szüksége. Lásd: [a tulajdonságok és metaadatok hello az importálási folyamat során](storage-import-export-tool-setting-properties-metadata-import-v1.md)további információt.

> [!NOTE]
>  Ha rendelkezik-e több gép leírt hello követelményeknek [beállítása hello Azure Import/Export eszköz](storage-import-export-tool-setup-v1.md), a másoláshoz adatok toomultiple merevlemez-meghajtók párhuzamosan az eszköz egy példányát futtató minden számítógépen.

 Az egyes merevlemez-meghajtóról, amely az Azure Import/Export eszköz hello készíteni hello eszköz létrehoz egy egyetlen napló fájlt. Az összes, a meghajtók toocreate hello importálási feladat kell hello Adatbázisnapló-fájlok. hello napló fájl is használt tooresume meghajtó előkészítése, ha hello eszköz megszakad.

### <a name="azure-importexport-tool-syntax-for-an-import-job"></a>Hogy az importálás az Azure Import/Export eszköz szintaxisa
 tooprepare meghajtók, hogy az importálás, hívja az hello Azure Import/Export eszköz hello **PrepImport** parancsot. Is paramétereket az attól függ, hogy ez hello, első másolás munkamenet, vagy egy későbbi másolási munkamenet.

 hello első másolás munkamenet meghajtó szükséges néhány további paraméterek toospecify hello tárfiók kulcsa; hello cél meghajtóbetűjelet; e hello formázni kell; e hello meghajtó titkosítva kell lennie, és ha igen, hello BitLocker-kulcsot. és hello naplókönyvtár. Íme egy kezdeti másolatot munkamenet toocopy hello szintaxisának egy könyvtárat vagy akár egyetlen fájlt:

 **Először másolja az munkamenet toocopy egyetlen könyvtár**

 `WAImportExport PrepImport /sk:<StorageAccountKey> /csas:<ContainerSas> /t: <TargetDriveLetter> [/format] [/silentmode] [/encrypt] [/bk:<BitLockerKey>] [/logdir:<LogDirectory>] /j:<JournalFile> /id:<SessionId> /srcdir:<SourceDirectory> /dstdir:<DestinationBlobVirtualDirectory> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]`

 **Először a munkamenet toocopy egyetlen fájl másolása**

 `WAImportExport PrepImport /sk:<StorageAccountKey> /csas:<ContainerSas> /t: <TargetDriveLetter> [/format] [/silentmode] [/encrypt] [/bk:<BitLockerKey>] [/logdir:<LogDirectory>] /j:<JournalFile> /id:<SessionId> /srcfile:<SourceFile> /dstblob:<DestinationBlobPath> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]`

 A következő másolási-munkamenetekben nem kell toospecify hello kezdeti paramétereket. Íme egy későbbi másolási munkamenet toocopy hello szintaxisának egy könyvtárat vagy akár egyetlen fájlt:

 **További másolási munkamenetek toocopy egyetlen könyvtár**

 `WAImportExport PrepImport /j:<JournalFile> /id:<SessionId> /srcdir:<SourceDirectory> /dstdir:<DestinationBlobVirtualDirectory> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]`

 **További másolási munkamenetek toocopy egyetlen fájl**

 `WAImportExport PrepImport /j:<JournalFile> /id:<SessionId> /srcfile:<SourceFile> /dstblob:<DestinationBlobPath> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]`

### <a name="parameters-for-hello-first-copy-session-for-a-hard-drive"></a>Hello paramétereinek másolnia a merevlemez-meghajtóról munkamenet
 Minden egyes futtatásakor hello Azure Import/Export eszköz toocopy fájlok toohello merevlemez-meghajtóról, hello eszköz létrehoz egy másolatot munkamenet. Minden példány munkamenet egyetlen könyvtárat vagy egyetlen fájlt tooa merevlemez-meghajtóra másolja. hello másolási munkamenet hello állapotát toohello napló fájl írása. Ha a Másolás munkamenet megszakad (például miatt tooa rendszer áramellátás megszakadása miatt), akkor hello eszköz újra és hello napló fájl hello parancssorban megadhatja folytathatók.

> [!WARNING]
>  Ha megadja a hello **/formázása** hello első másolás munkamenet paramétere, hello meghajtó formázva lesz, és hello meghajtón lévő összes adat törlődik. Javasoljuk, használjon üres meghajtók csak a Másolás munkamenet.

 minden meghajtó első másolás munkamenet hello használt hello parancs paraméterei különböző mint hello parancsok későbbi másolási munkamenetek. hello következő táblázat hello további paraméterek, amelyek elérhetők a hello első másolás munkamenet.

|Parancssori paraméter|Leírás|
|-----------------------------|-----------------|
|**/SK:**< StorageAccountKey\>|`Optional.`hello tárfiók kulcsa hello tárolási fiók toowhich hello adatok fog kiterjedni. Meg kell adni vagy **/sk:**< StorageAccountKey\> vagy **/csas:**< ContainerSas\> hello parancsban.|
|**/csas:**< ContainerSas\>|`Optional`. hello tároló SAS toouse tooimport adatok toohello tárfiók. Meg kell adni vagy **/sk:**< StorageAccountKey\> vagy **/csas:**< ContainerSas\> hello parancsban.<br /><br /> a paraméter értéke hello hello tároló neve követi a kérdőjel (?) és a hello SAS-jogkivonatot kell kezdődnie. Példa:<br /><br /> `mycontainer?sv=2014-02-14&sr=c&si=abcde&sig=LiqEmV%2Fs1LF4loC%2FJs9ZM91%2FkqfqHKhnz0JM6bqIqN0%3D&se=2014-11-20T23%3A54%3A14Z&sp=rwdl`<br /><br /> hello engedélyek, hogy hello URL-címen, vagy egy tárolt hozzáférési házirendben megadott, tartalmaznia kell olvasási, írási és törlési importálási feladatok, és olvasási, írási és lista exportálási feladatok.<br /><br /> Ha ez a paraméter meg van adva, az összes BLOB toobe importálása vagy exportálása hello közös hozzáférésű jogosultságkódot megadott hello tároló tartományba kell esnie.|
|**/ t:**< TargetDriveLetter\>|`Required.`hello hello cél merevlemez-meghajtó betűjele hello aktuális másolási munkamenet, hello záró kettőspont nélkül.|
|**Format**|`Optional.`Ez a paraméter megadásakor hello meghajtót kell toobe formázva. Ellenkező esetben hagyja ki ezt az. Mielőtt hello eszköz hello meghajtó formátumú, akkor arra fogja kérni a konzolról megerősítést. toosuppress hello jóváhagyás lapon adja meg a hello /silentmode paramétert.|
|**/silentmode**|`Optional.`Adja meg a paraméter toosuppress hello megerősítő hello targert meghajtó formázásához.|
|**/ titkosítása**|`Optional.`Ez a paraméter megadott hello meghajtó nem még titkosított hello eszköz titkosítása a BitLocker és a vonatkozó igényeket toobe rendelkező. Ha a BitLocker már titkosított hello meghajtót, majd kihagyja ezt a paramétert, és adja meg a hello `/bk` paraméter hello meglévő BitLocker kulcs megadása.<br /><br /> Ha megadja a hello `/format` paraméter, akkor is meg kell adnia hello `/encrypt` paraméter.|
|**/BK:**< BitLockerKey\>|`Optional.`Ha `/encrypt` van megadva, hagyja ki ezt a paramétert. Ha `/encrypt` van szüksége a nincs megadva, toohave már titkosított hello meghajtón a BitLocker szolgáltatással. A paraméter toospecify hello BitLocker-kulcsot használ. BitLocker-titkosítást az importálási feladatok minden merevlemezét szükség.|
|**/ logdir:**< LogDirectory\>|`Optional.`hello naplókönyvtár megadja egy könyvtár toobe toostore részletes naplókat, valamint az ideiglenes fájlok. Ha nincs megadva, hello aktuális könyvtárhoz hello naplókönyvtár lesz.|

### <a name="parameters-required-for-all-copy-sessions"></a>Az összes másolási munkamenetek szükséges paraméterek
 hello napló fájlt egy merevlemezen összes másolási munkamenetére hello állapotát tartalmazza. Információkat is tartalmaz hello szükséges toocreate hello importálási feladat. A naplófájl mindig meg kell adnia, amikor fut hello Azure Import/Export eszköz, valamint egy másolatot munkamenet-azonosító:

|||
|-|-|
|Parancssori paraméter|Leírás|
|**/j:**< JournalFile\>|`Required.`hello elérési toohello naplófájl. Minden olyan meghajtó meg kell adni pontosan egy napló fájlt. Vegye figyelembe, hogy hello napló fájl nem hello cél meghajtón kell lennie. hello napló fájl kiterjesztése `.jrn`.|
|**/ID:**< munkamenet-azonosító\>|`Required.`hello munkamenet-azonosító egy másolás munkamenet azonosítja. Használt tooensure pontos helyreállítási egy megszakított másolási munkamenet is. Másolás munkamenetben másolt fájlok vannak tárolva egy munkamenet-Azonosítót hello hello célmeghajtó után nevű könyvtár.|

### <a name="parameters-for-copying-a-single-directory"></a>Egyetlen könyvtár másolására paraméterek
 Egyetlen könyvtár másolásakor alkalmazni hello következő szükséges és választható paraméterek:

|Parancssori paraméter|Leírás|
|----------------------------|-----------------|
|**/srcdir:**< SourceDirectory\>|`Required.`fájlok toobe tartalmazó hello forráskönyvtár toohello célmeghajtó másolva. hello könyvtár elérési útjának abszolút elérési útnak (nem relatív elérési utat) kell lennie.|
|**/dstdir:**< DestinationBlobVirtualDirectory\>|`Required.`hello elérési toohello cél virtuális könyvtárának a Windows Azure storage-fiókot. hello virtuális könyvtárat is, vagy előfordulhat, hogy már nem létezik.<br /><br /> Megadhatja, hogy a tároló, vagy egy blob előtagja, például `music/70s/`. hello célkönyvtár a következővel kell kezdődnie hello Tárolónév esetén törtvonallal követ "/", és előfordulhat, hogy választhatóan blob virtuális könyvtár nem végződik "/".<br /><br /> Ha hello céltárolója hello legfelső szintű tárolója, explicit módon meg kell hello legfelső szintű tároló, beleértve hello törtvonallal, mint `$root/`. Mivel a blobok hello gyökérszintű tárolóban nem tartalmazhatnak "/" a név alkönyvtáraiban hello forráskönyvtárban nem másolódnak hello célkönyvtáron hello legfelső szintű tároló esetén.<br /><br /> Lehet, hogy toouse érvényes tárolónévnek cél virtuális címtárak vagy blobot megadásakor. Ne feledje, hogy a tároló nevének kisbetűnek kell lennie. A tároló elnevezési szabályait, lásd: [elnevezési és hivatkozó tárolók, Blobok és metaadatok](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).|
|**/ Szabályozó:**< átnevezése &#124; nem írható felül &#124; felülírása >|`Optional.`Meghatározza a hello viselkedését, amikor egy blobot hello a megadott cím már létezik. Ez a paraméter érvényes értékei: `rename`, `no-overwrite` és `overwrite`. Ne feledje, hogy ezek az értékek kis-és nagybetűket. Ha nincs érték megadva hello alapértelmezett érték `rename`.<br /><br /> Ez a paraméter hatással van a megadott hello hello könyvtárban található összes hello fájl számára megadott érték hello `/srcdir` paraméter.|
|**/ BlobType:**< BlockBlob &#124; PageBlob >|`Optional.`Hello cél blobok hello blob típusát határozza meg. Érvényes értékek a következők: `BlockBlob` és `PageBlob`. Ne feledje, hogy ezek az értékek kis-és nagybetűket. Ha nincs érték megadva hello alapértelmezett érték `BlockBlob`.<br /><br /> A legtöbb esetben `BlockBlob` ajánlott. Ha megad `PageBlob`, minden fájl hello hello hosszát 512, hello méretű lapblobokra lap többszörösének kell lennie.|
|**/ PropertyFile:**< PropertyFile\>|`Optional.`Elérési út toohello tulajdonság fájl hello cél blobokat. Lásd: [Import/Export service metaadatok és a Tulajdonságok fájlformátum](storage-import-export-file-format-metadata-and-properties.md) további információt.|
|**/ MetadataFile:**< MetadataFile\>|`Optional.`Elérési út toohello tartozó metaadatfájl hello cél blobokat. Lásd: [Import/Export service metaadatok és a Tulajdonságok fájlformátum](storage-import-export-file-format-metadata-and-properties.md) további információt.|

### <a name="parameters-for-copying-a-single-file"></a>Paraméterek egy fájl másolása
 Ha egy fájl másolása, hello következő szükséges és választható paramétereit alkalmazza:

|Parancssori paraméter|Leírás|
|----------------------------|-----------------|
|**/srcfile:**< SourceFile\>|`Required.`hello teljes elérési útja toohello fájl toobe másolva. hello könyvtár elérési útjának abszolút elérési útnak (nem relatív elérési utat) kell lennie.|
|**/dstblob:**< DestinationBlobPath\>|`Required.`hello elérési toohello cél blob Windows Azure-tárfiókba. hello blob is, vagy már nem létezik.<br /><br /> Hello blob neve elején hello tároló neve adható meg. hello blob neve nem kezdődhet "/" vagy hello tárfióknevet. A blob elnevezési szabályait, lásd: [elnevezési és hivatkozó tárolók, Blobok és metaadatok](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).<br /><br /> Ha hello céltárolója hello legfelső szintű tárolója, explicit módon meg kell `$root` hello a tárolóhoz, például a módon `$root/sample.txt`. Vegye figyelembe, hogy a blobok hello gyökérszintű tárolóban nem tartalmazhatnak "/" a név.|
|**/ Szabályozó:**< átnevezése &#124; nem írható felül &#124; felülírása >|`Optional.`Meghatározza a hello viselkedését, amikor egy blobot hello a megadott cím már létezik. Ez a paraméter érvényes értékei: `rename`, `no-overwrite` és `overwrite`. Ne feledje, hogy ezek az értékek kis-és nagybetűket. Ha nincs érték megadva hello alapértelmezett érték `rename`.|
|**/ BlobType:**< BlockBlob &#124; PageBlob >|`Optional.`Hello cél blobok hello blob típusát határozza meg. Érvényes értékek a következők: `BlockBlob` és `PageBlob`. Ne feledje, hogy ezek az értékek kis-és nagybetűket. Ha nincs érték megadva hello alapértelmezett érték `BlockBlob`.<br /><br /> A legtöbb esetben `BlockBlob` ajánlott. Ha megad `PageBlob`, minden fájl hello hello hosszát 512, hello méretű lapblobokra lap többszörösének kell lennie.|
|**/ PropertyFile:**< PropertyFile\>|`Optional.`Elérési út toohello tulajdonság fájl hello cél blobokat. Lásd: [Import/Export service metaadatok és a Tulajdonságok fájlformátum](storage-import-export-file-format-metadata-and-properties.md) további információt.|
|**/ MetadataFile:**< MetadataFile\>|`Optional.`Elérési út toohello tartozó metaadatfájl hello cél blobokat. Lásd: [Import/Export service metaadatok és a Tulajdonságok fájlformátum](storage-import-export-file-format-metadata-and-properties.md) további információt.|

### <a name="resuming-an-interrupted-copy-session"></a>Egy megszakított másolási munkamenet folytatása
 Ha a Másolás munkamenet bármilyen okból megszakad, csak a megadott hello naplófájl hello eszköz futtatásával folytathatja:

```
WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> /ResumeSession
```

 Csak hello legutóbbi másolási munkamenet, ha rendellenesen, folytathatók.

> [!IMPORTANT]
>  A másolási munkamenet visszatértekor ne módosítsa hello forrás adatok fájlok és könyvtárak hozzáadásával vagy eltávolításával a fájlokat.

### <a name="aborting-an-interrupted-copy-session"></a>Egy megszakított másolási munkamenet megszakítása
 Ha egy másolás munkamenet megszakad, és nem lehetséges tooresume (például ha forráskönyvtárat bizonyult nem érhető el), meg kell megszakítás hello aktuális munkamenet, hogy akkor állítható vissza és az új példány munkamenetek lehet indítani:

```
WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> /AbortSession
```

 Csak hello utolsó másolás munkamenet, ha a rendellenesen, is megszakítja. Vegye figyelembe, hogy Ön megszakítási hello első másolás munkamenet meghajtóhoz. Ehelyett újra kell indítani a hello másolási munkamenet egy új napló-fájllal.

## <a name="next-steps"></a>Következő lépések

* [Hello Azure Import/Export eszköz beállítása](storage-import-export-tool-setup-v1.md)
* [A tulajdonságok és metaadatok során hello importálási folyamat](storage-import-export-tool-setting-properties-metadata-import-v1.md)
* [Minta munkafolyamat tooprepare merevlemezeit, az importálási feladat](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow-v1.md)
* [A gyakran használt parancsok rövid összefoglalása](storage-import-export-tool-quick-reference-v1.md) 
* [Feladatok állapotának áttekintése a másolási naplófájlok segítségével](storage-import-export-tool-reviewing-job-status-v1.md)
* [Importálási feladat javítása](storage-import-export-tool-repairing-an-import-job-v1.md)
* [Exportálási feladat javítása](storage-import-export-tool-repairing-an-export-job-v1.md)
* [Hibaelhárítási hello Azure Import/Export eszköz](storage-import-export-tool-troubleshooting-v1.md)
