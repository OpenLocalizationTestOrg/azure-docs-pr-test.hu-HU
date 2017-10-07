---
title: "az Azure Import/Export aaaPreparing merevlemezek importálási feladat |} Microsoft Docs"
description: "Ismerje meg, hogyan tooprepare merevlemez-meghajtókat használ hello WAImportExport eszköz toocreate hello Azure Import/Export szolgáltatás egy importálási feladat."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: muralikk
ms.openlocfilehash: 3f247a9efee29da2d18140353edc9dd7103a0761
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="preparing-hard-drives-for-an-import-job"></a>Merevlemez-meghajtók előkészítése az importálási feladat

hello WAImportExport eszköze hello meghajtó előkészítése és lemezjavító eszköz, amely hello használata [Microsoft Azure Import/Export szolgáltatás](storage-import-export-service.md). Az eszköz toocopy toohello rögzített adatmeghajtókon tooship tooan Azure-adatközpontban fog is használhatja. Az importálási feladat befejezése után is használhatja az eszköz toorepair megsérült, hiányoztak vagy ütköző blobot a más BLOB. Miután hello meghajtók kapott egy befejezett exportálási feladat, használhatja az eszköz toorepair, azokat a fájlokat, sérült vagy hiányzó hello meghajtókon. Ez a cikk azt ismerteti hello használata az eszköz is.

## <a name="prerequisites"></a>Előfeltételek

### <a name="requirements-for-waimportexportexe"></a>WAImportExport.exe követelményei

- **Számítógép-konfiguráció**
  - Windows 7, Windows Server 2008 R2 vagy újabb Windows operációs rendszert
  - .NET framework 4 telepítve kell lennie. Lásd: [gyakran ismételt kérdések](#faq) hogyan toocheck, ha a .net-keretrendszer verziója hello számítógépen telepítve.
- **Tárfiók kulcsa** -hello kulcsait közül legalább egy hello storage-fiók szükséges.

### <a name="preparing-disk-for-import-job"></a>Lemez előkészítése az importálási feladat

- **A BitLocker -** BitLocker hello gépen futó hello WAImportExport eszköz engedélyezve kell lennie. Lásd: hello [gyakran ismételt kérdések](#faq) arról, hogyan tooenable BitLocker.
- **Lemezek** érhető el a gépen, amelyen WAImportExport eszközt futtatja. Lásd: [gyakran ismételt kérdések](#faq) lemez megadását.
- **Forrásfájlokat** -tooimport tervezi hello fájlok hello másolási számítógépről elérhetőnek kell lennie, hogy egy hálózati megosztásra vagy egy helyi merevlemezen lévő.

### <a name="repairing-a-partially-failed-import-job"></a>A részlegesen sikertelen importálási feladat javítása

- **Másolás naplófájl** , amely jön létre, amikor az Azure Import/Export szolgáltatás közötti Tárfiók és a lemez adatainak másolása. A céloldali tárfiók található.

### <a name="repairing-a-partially-failed-export-job"></a>A részlegesen sikertelen exportálási feladat javítása

- **Másolás naplófájl** , amely jön létre, amikor az Azure Import/Export szolgáltatás közötti Tárfiók és a lemez adatainak másolása. A forrás tárolási fiókjában található.
- **A jegyzékfájlnak** -exportált meghajtón, a Microsoft által visszaadott [opcionális] Located.

## <a name="download-and-install-waimportexport"></a>Töltse le és telepítse a WAImportExport

Töltse le a hello [WAImportExport.exe legújabb verziójának](https://www.microsoft.com/download/details.aspx?id=55280). Bontsa ki a hello tömörített tartalom tooa könyvtárat a számítógépen.

A következő feladathoz toocreate CSV-fájlok.

## <a name="prepare-hello-dataset-csv-file"></a>Hello dataset CSV-fájl előkészítése

### <a name="what-is-dataset-csv"></a>Mi az az adatkészlet CSV

A DataSet CSV-fájl hello /dataset jelző értéke könyvtárak listája és/vagy a fájlok másolása toobe tootarget meghajtók listáját tartalmazó CSV-fájlból. hello első lépés toocreating az importálási feladat toodetermine mely könyvtárak és fájlok tooimport fog. Ez lehet könyvtárak listája, egyedi fájlok listáját, vagy ezeket a kettő kombinációja. Része egy könyvtárat, ha akkor hello könyvtárban és annak alkönyvtáraiban található összes fájl hello importálási feladat része lesz.

Minden egyes importált fájl vagy könyvtár toobe meg kell adni a cél virtuális könyvtárat vagy hello Azure Blob szolgáltatást a blob. Ezeken a célokon bemenetek toohello WAImportExport eszközként fogja használni. Könyvtárak hello perjel karakterrel kell lennie elválasztott "/".

a következő táblázat hello blob tárolók néhány példát látható:

| Forrás-fájl vagy könyvtár | Cél blob vagy virtuális könyvtár |
| --- | --- |
| H:\Video | https://mystorageaccount.BLOB.Core.Windows.NET/video |
| H:\Photo | https://mystorageaccount.BLOB.Core.Windows.NET/Photo |
| K:\Temp\FavoriteVideo.ISO | https://mystorageaccount.BLOB.Core.Windows.NET/Favorite/FavoriteVideo.ISO |
| \\myshare\john\music | https://mystorageaccount.BLOB.Core.Windows.NET/Music |

### <a name="sample-datasetcsv"></a>A minta dataset.csv

```
BasePath,DstBlobPathOrPrefix,BlobType,Disposition,MetadataFile,PropertiesFile
"F:\50M_original\100M_1.csv.txt","containername/100M_1.csv.txt",BlockBlob,rename,"None",None
"F:\50M_original\","containername/",BlockBlob,rename,"None",None
```

### <a name="dataset-csv-file-fields"></a>Adatkészlet CSV-fájl mezők

| Mező | Leírás |
| --- | --- |
| BasePath | **[Szükséges]**<br/>Ez a paraméter értékének hello ahol hello adatok toobe importálva hello forrás jelöli. hello eszköz lesz rekurzív módon másolása az elérési út alatt található összes adatot.<br><br/>**Megengedett értékek**: Ez toobe nem rendelkezik a helyi számítógépen érvényes elérési utat vagy egy érvényes elérési utat, és elérhető-e hello felhasználó kell lennie. hello könyvtár elérési útjának abszolút elérési útnak (nem relatív elérési utat) kell lennie. Ha hello elérési útja végződik "\\", a befejezési nélkül elérési utat képvisel más könyvtár"\\" fájlt jelöli.<br/>Ez a mező nem regex engedélyezett. Ha hello elérési út szóközöket tartalmaz, tegye "".<br><br/>**Példa**: "c:\Directory\c\Directory\File.txt"<br>"\\\\FBaseFilesharePath.domain.net\sharename\directory\"  |
| DstBlobPathOrPrefix | **[Szükséges]**<br/> hello elérési toohello cél virtuális könyvtárának a Windows Azure storage-fiókot. hello virtuális könyvtárat is, vagy előfordulhat, hogy már nem létezik. Ha még nem létezik, Import/Export szolgáltatás egyet fog létrehozni.<br/><br/>Lehet, hogy toouse érvényes tárolónévnek cél virtuális címtárak vagy blobot megadásakor. Ne feledje, hogy a tároló nevének kisbetűnek kell lennie. A tároló elnevezési szabályait, lásd: [elnevezési és hivatkozó tárolók, Blobok és metaadatok](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata). Ha csak a legfelső szintű meg van adva, hello könyvtárstruktúrát hello forrás replikálódik hello cél blob a tárolóban. Ha egy másik könyvtárstruktúrát van szükség, mint egy forrás, leképezés CSV-ben több sort a hello<br/><br/>Megadhat egy tárolót, vagy egy blob előtagja például zene/70s /. hello célkönyvtár a következővel kell kezdődnie hello Tárolónév esetén törtvonallal követ "/", és előfordulhat, hogy választhatóan blob virtuális könyvtár nem végződik "/".<br/><br/>Ha hello céltárolója hello legfelső szintű tárolója, explicit módon meg kell hello legfelső szintű tároló, beleértve a hello törtvonallal, mint a $root /. Mivel a blobok hello gyökérszintű tárolóban nem tartalmazhatnak "/" a név alkönyvtáraiban hello forráskönyvtárban nem másolódnak hello célkönyvtáron hello legfelső szintű tároló esetén.<br/><br/>**Példa**<br/>Ha hello blob célútvonalat https://mystorageaccount.blob.core.windows.net/video, ez a mező értékének hello videó lehet /  |
| BlobType | **[Választható]**  blokk &#124; a lap<br/>Jelenleg Import/Export szolgáltatás használatát támogatja 2 blobokat. Blobok és blokk BlobsBy alapértelmezett összes fájlokat importálni szeretné blokkblobként lapon. És \*.vhd és \*.vhdx importálódnak, mert a lap BlobsThere korlátozást hello blokkblob és lapblobterület megengedett méretet. Lásd: [Storage méretezhetőségi célok](storage-scalability-targets.md#scalability-targets-for-blobs-queues-tables-and-files) további információt.  |
| Törlése | **[Választható]**  átnevezése &#124; nem írható felül &#124; felülírása <br/> Ebben a mezőben hello másolási-viselkedését az importálási Egytényezős során Ha folyamatban van-e az adatok feltöltése toohello tárfiók hello lemezről. Lehetőségek a következők: átnevezése &#124; felülírja &#124; nem írható felül. Alapértelmezett értéke túl "átnevezése", ha nincs megadva. <br/><br/>**Nevezze át**: Ha azonos névvel rendelkező objektum, a rendszer létrehoz egy másolatot a cél.<br/>Írja felül: felülírja a hello fájlt újabb fájllal. hello fájl utolsó módosításának wins.<br/>**Nem írható felül**: hello írása átugrása fájlt, ha már van ilyen.|
| MetadataFile | **[Választható]** <br/>hello érték toothis mezője hello metaadatfájl, amelyen megadható, ha egy hello toopreserve hello metaadatok hello objektumok, vagy adjon meg egyéni metaadat. Elérési út toohello tartozó metaadatfájl hello cél blobokat. Lásd: [Import/Export service metaadatok és a Tulajdonságok fájlformátum](storage-import-export-file-format-metadata-and-properties.md) további információ |
| PropertiesFile | **[Választható]** <br/>Elérési út toohello tulajdonság fájl hello cél blobokat. Lásd: [Import/Export service metaadatok és a Tulajdonságok fájlformátum](storage-import-export-file-format-metadata-and-properties.md) további információt. |

## <a name="prepare-initialdriveset-or-additionaldriveset-csv-file"></a>Készítse elő a InitialDriveSet vagy AdditionalDriveSet CSV-fájl

### <a name="what-is-driveset-csv"></a>Mi az a fürt megosztott kötetei szolgáltatás driveset

hello hello /InitialDriveSet vagy /AdditionalDriveSet jelző értéke hello toowhich hello meghajtóbetűjelek van leképezve, így hello eszköz is megfelelően válasszon előkészített lemezek toobe hello listája lemezek listáját tartalmazó CSV-fájlból. Ha hello adatok mérete nagyobb, mint a egyetlen lemez méretét, hello WAImportExport eszköz több olyan lemezt, a CSV-fájlban található optimalizált módon bejegyezve hello adatokat fogja kiosztani.

Nincs a lemezek hello adatok hello száma korlátozva tooin egyetlen munkamenet is beírhatók. hello eszköz kiosztja az adatokat a lemez mérete és a mappa mérete alapján. A legtöbb hello lemezt kiválaszt hello objektum méretű optimalizálva. Amikor feltöltött adatok hello toohello tárfiók lesz átszervezett hátsó toohello könyvtárstruktúrát dataset fájlban megadott. Rendelés toocreate egy fürt megosztott kötetei szolgáltatás driveset kövesse az alábbi hello lépéseket.

### <a name="create-basic-volume-and-assign-drive-letter"></a>Egyszerű kötet létrehozása és a meghajtóbetűjelet rendelje

Érdekében toocreate alaplemezek és meghajtóbetűjelet, a "Egyszerűbb partíció létrehozása" regisztrációkor hello utasításokat követve [Lemezkezelés áttekintése](https://technet.microsoft.com/library/cc754936.aspx).

### <a name="sample-initialdriveset-and-additionaldriveset-csv-file"></a>A minta InitialDriveSet és AdditionalDriveSet CSV-fájl

```
DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
G,AlreadyFormatted,SilentMode,AlreadyEncrypted,060456-014509-132033-080300-252615-584177-672089-411631
H,Format,SilentMode,Encrypt,
```

### <a name="driveset-csv-file-fields"></a>Driveset CSV-fájl mezők

| Mezők | Érték |
| --- | --- |
| Meghajtóbetűjel | **[Szükséges]**<br/> Minden olyan meghajtó toohello eszköz hello célként megadott kell egy egyszerű NTFS-köteten van, és egy meghajtó-betűjellel tooit hozzárendelve.<br/> <br/>**Példa**: R vagy r |
| FormatOption | **[Szükséges]**  Formátum &#124; AlreadyFormatted<br/><br/> **Formátum**: Adja meg a formázza a hello lemezen lévő összes hello adatokat. <br/>**AlreadyFormatted**: hello eszköz kihagyja, ha ezt az értéket. |
| SilentOrPromptOnFormat | **[Szükséges]**  SilentMode &#124; PromptOnFormat<br/><br/>**SilentMode**: Ez az érték megadása lehetővé teszi felhasználói toorun hello eszköz csendes módban. <br/>**PromptOnFormat**: hello eszköz hello felhasználói tooconfirm felkéri, hogy hello művelet valóban szánt minden formátum.<br/><br/>Ha nincs megadva, parancs fog megszakítása és hibaüzenet megjelenítése: "beállítás értéke helytelen SilentOrPromptOnFormat: nincs" |
| Titkosítás | **[Szükséges]**  Titkosítása &#124; AlreadyEncrypted<br/> Ez a mező értékének hello úgy dönt, mely lemezre tooencrypt, és amely nem arra. <br/><br/>**Titkosítani**: eszköz formázza hello meghajtó. Ha "FormatOption" mező értéke "Formátum" Ez az érték akkor szükséges toobe "Titkosítás". "AlreadyEncrypted" meg van adva ebben az esetben, ha okoz hiba történt "Formátum megadása esetén titkosítása is meg kell adni".<br/>**AlreadyEncrypted**: eszköz használata "ExistingBitLockerKey" mezőben megadott BitLockerKey hello hello meghajtó fog visszafejtéséhez. Ha a "FormatOption" mező értéke "AlreadyFormatted", majd ez az érték lehet "Titkosítás" vagy "AlreadyEncrypted" |
| ExistingBitLockerKey | **[Szükséges]**  Ha "Titkosítás" mező értéke "AlreadyEncrypted"<br/> hello Ez a mező értéke hello adott lemezt társított hello BitLocker-kulcsot. <br/><br/>Ez a mező "Titkosítás" hello "Titkosítás" mező értékének esetén üresen kell hagyni.  A BitLocker kulcs van megadva ebben az esetben, ha okoz hiba történt "Bitlocker kulcs nem adható meg".<br/>  **Példa**: 060456-014509-132033-080300-252615-584177-672089-411631|

##  <a name="preparing-disk-for-import-job"></a>Lemez előkészítése az importálási feladat

tooprepare meghajtók, hogy az importálás, hívja hello WAImportExport eszközt a hello **PrepImport** parancsot. Is paramétereket az attól függ, hogy ez hello, első másolás munkamenet, vagy egy későbbi másolási munkamenet.

### <a name="first-session"></a>Első munkamenet

Első másolás munkamenet tooCopy egy Single/Multiple Directory tooa egy vagy több lemez (attól függően, hogy mi a CSV-fájlban van megadva) WAImportExport eszköz hello PrepImport parancsot kell másolnia munkamenet toocopy könyvtárak és/vagy a fájlokat egy új munkamenet-példány:

```
WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> [/logdir:<LogDirectory>] [/sk:<StorageAccountKey>] [/silentmode] [/InitialDriveSet:<driveset.csv>] DataSet:<dataset.csv>
```

**Példa**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#1  /sk:\*\*\*\*\*\*\*\*\*\*\*\*\* /InitialDriveSet:driveset-1.csv /DataSet:dataset-1.csv /logdir:F:\logs
```

### <a name="add-data-in-subsequent-session"></a>Adatok hozzáadása a következő munkamenet

A következő másolási-munkamenetekben nem kell toospecify hello kezdeti paramétereket. Toouse kell hello azonos naplófájl ahhoz, hogy hello eszköz tooremember ahol azt korábban hello az előző munkamenet. hello másolási munkamenet hello állapotát toohello napló fájl írása. Íme egy későbbi másolatot hello szintaxisának munkamenet toocopy további könyvtárak és, vagy fájlokat:

```
WAImportExport.exe PrepImport /j:<SameJournalFile> /id:<DifferentSessionId>  [DataSet:<differentdataset.csv>]
```

**Példa**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2  /DataSet:dataset-2.csv
```

### <a name="add-drives-toolatest-session"></a>Adja hozzá a meghajtók toolatest munkamenet

Ha hello adatok nem fértek InitialDriveset a meghajtók, egy hello eszköz tooadd további meghajtók toosame másolási munkamenet használja. 

>[!NOTE] 
>hello munkamenet-azonosítót meg kell felelnie hello előző munkamenet-azonosító. Napló fájlt meg kell felelnie egy előző munkamenetben megadott hello.
>
```
WAImportExport.exe PrepImport /j:<SameJournalFile> /id:<SameSessionId> /AdditionalDriveSet:<newdriveset.csv>
```

**Példa**

```
WAImportExport.exe PrepImport /j:SameJournalTest.jrn /id:session#2  /AdditionalDriveSet:driveset-2.csv
```

### <a name="abort-hello-latest-session"></a>Hello legújabb munkamenet megszakítása:

Ha egy másolás munkamenet megszakad, és nem lehetséges tooresume (például ha forráskönyvtárat bizonyult nem érhető el), meg kell megszakítás hello aktuális munkamenet, hogy akkor állítható vissza és az új példány munkamenetek lehet indítani:

```
WAImportExport.exe PrepImport /j:<SameJournalFile> /id:<SameSessionId> /AbortSession
```

**Példa**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2  /AbortSession
```

Csak hello utolsó másolás munkamenet, ha a rendellenesen, is megszakítja. Vegye figyelembe, hogy Ön megszakítási hello első másolás munkamenet meghajtóhoz. Ehelyett újra kell indítani a hello másolási munkamenet egy új napló-fájllal.

### <a name="resume-a-latest-interrupted-session"></a>A legújabb megszakított munkamenet folytatása

Ha a Másolás munkamenet bármilyen okból megszakad, csak a megadott hello naplófájl hello eszköz futtatásával folytathatja:

```
WAImportExport.exe PrepImport /j:<SameJournalFile> /id:<SameSessionId> /ResumeSession
```

**Példa**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2 /ResumeSession
```

> [!IMPORTANT] 
> A másolási munkamenet visszatértekor ne módosítsa hello forrás adatok fájlok és könyvtárak hozzáadásával vagy eltávolításával a fájlokat.

## <a name="waimportexport-parameters"></a>WAImportExport paraméterek

| Paraméterek | Leírás |
| --- | --- |
|     /j:&lt;JournalFile&gt;  | **Szükséges**<br/> Toohello naplófájl elérési útja. A naplófájl meghajtók készlete nyomon követi, és rekordok hello folyamatban van, ezek a meghajtók előkészítésében. hello naplófájl mindig meg kell adni.  |
|     / logdir:&lt;LogDirectory&gt;  | **Választható**. hello naplókönyvtár.<br/> Részletes naplófájlok, valamint bizonyos fájlokat toothis directory kerülnek. Ha nem, hello naplókönyvtár megadott, a jelenlegi könyvtárat fogja használni. hello naplókönyvtár is csak egyszer adható meg a hello ugyanahhoz a napló-fájlhoz.  |
|     /ID:&lt;munkamenet-azonosító&gt;  | **Szükséges**<br/> hello munkamenet azonosítójának használt tooidentify másolási-munkamenet. Használt tooensure pontos helyreállítási egy megszakított másolási munkamenet is.  |
|     / ResumeSession  | Választható. Ha hello utolsó másolás munkamenet rendellenesen megszakadt, ennek a paraméternek megadott tooresume hello munkamenet lehet.   |
|     / AbortSession  | Választható. Ha hello utolsó másolás munkamenet rendellenesen megszakadt, ennek a paraméternek megadott tooabort hello munkamenet lehet.  |
|     /sn:&lt;StorageAccountName&gt;  | **Szükséges**<br/> Csak a RepairImport és RepairExport alkalmazható. hello hello storage-fiók neve.  |
|     /SK:&lt;StorageAccountKey&gt;  | **Szükséges**<br/> hello hello tárfiók kulcsa. |
|     / InitialDriveSet:&lt;driveset.csv&gt;  | **Szükséges** első másolás munkamenet hello futtatásakor<br/> A CSV-fájl meghajtók tooprepare listáját tartalmazza.  |
|     / AdditionalDriveSet:&lt;driveset.csv&gt; | **Szükséges**. Meghajtók toocurrent másolási munkamenet hozzáadásakor. <br/> A CSV-fájl fel további meghajtók toobe listáját tartalmazza.  |
|      r:&lt;RepairFile&gt; | **Szükséges** RepairImport és RepairExport csak érvényes.<br/> Elérési út toohello fájl nyomon követése javítása folyamatban van. Minden olyan meghajtó meg kell adni egy javítási fájlt.  |
|     / d:&lt;TargetDirectories&gt; | **Szükséges**. Csak a RepairImport és RepairExport alkalmazható. A RepairImport egy vagy több könyvtárakat pontosvesszővel elválasztott toorepair; A RepairExport, egy címtárban toorepair, pl. gyökérkönyvtár hello meghajtó.  |
|     / CopyLogFile:&lt;DriveCopyLogFile&gt; | **Szükséges** RepairImport és RepairExport csak érvényes. Elérési út toohello meghajtó másolása naplófájl (részletes vagy hiba).  |
|     / ManifestFile:&lt;DriveManifestFile&gt; | **Szükséges** RepairExport csak alkalmazható.<br/> Elérési út toohello meghajtó jegyzékfájlt.  |
|     / PathMapFile:&lt;DrivePathMapFile&gt; | **Választható**. Csak a RepairImport alkalmazható.<br/> Elérési út toohello fájlt tartalmazó fájl elérési utak relatív toohello meghajtó legfelső szintű toolocations (tabulátorral tagolt) tényleges fájlok leképezéseit. Először adja meg, ha azt tölti fel elérési utat a üres célkitűzések, ami azt jelenti, vagy nem található TargetDirectories, a hozzáférés megtagadva a érvénytelen a neve, vagy több könyvtárban vannak. hello elérési leképezés lehet manuálisan szerkesztett tooinclude hello helyes cél elérési, és újra a hello eszköz tooresolve hello elérési utat helyesen megadva.  |
|     / ExportBlobListFile:&lt;ExportBlobListFile&gt; | **Szükséges**. Csak a PreviewExport alkalmazható.<br/> Elérési út toohello XML blob elérési utak listája tartalmazó fájlt, vagy blob elérési út előtagokat hello blobok toobe exportált. hello fájlformátum hello azonos hello blob lista blob formátumban a hello feladat Put művelet hello Import/Export szolgáltatás REST API számára.  |
|     / DriveSize:&lt;DriveSize&gt; | **Szükséges**. Csak a PreviewExport alkalmazható.<br/>  Az Exportálás használt meghajtók toobe mérete. Ha például 500 GB 1,5 TB. Megjegyzés: 1 GB 1 000 hívás = 000 000 bytes1 TB 1,000,000,000,000 bájt =  |
|     / Adatkészlet:&lt;dataset.csv&gt; | **Szükséges**<br/> Könyvtárak listája és/vagy a fájlok toobe listáját tartalmazó CSV-fájl tootarget meghajtókra másolva.  |
|     /silentmode  | **Választható**.<br/> Nincs megadva, a meghajtó követelmény hello és a megerősítő toocontinue kell emlékezteti.  |

## <a name="tool-output"></a>Eszköz kimeneti

### <a name="sample-drive-manifest-file"></a>Minta meghajtó jegyzékfájl

```xml
<?xml version="1.0" encoding="UTF-8"?>
<DriveManifest Version="2011-MM-DD">
   <Drive>
      <DriveId>drive-id</DriveId>
      <StorageAccountKey>storage-account-key</StorageAccountKey>
      <ClientCreator>client-creator</ClientCreator>
      <!-- First Blob List -->
      <BlobList Id="session#1-0">
         <!-- Global properties and metadata that applies tooall blobs -->
         <MetadataPath Hash="md5-hash">global-metadata-file-path</MetadataPath>
         <PropertiesPath Hash="md5-hash">global-properties-file-path</PropertiesPath>
         <!-- First Blob -->
         <Blob>
            <BlobPath>blob-path-relative-to-account</BlobPath>
            <FilePath>file-path-relative-to-transfer-disk</FilePath>
            <ClientData>client-data</ClientData>
            <Length>content-length</Length>
            <ImportDisposition>import-disposition</ImportDisposition>
            <!-- page-range-list-or-block-list -->
            <!-- page-range-list -->
            <PageRangeList>
               <PageRange Offset="1073741824" Length="512" Hash="md5-hash" />
               <PageRange Offset="1073741824" Length="512" Hash="md5-hash" />
            </PageRangeList>
            <!-- block-list -->
            <BlockList>
               <Block Offset="1073741824" Length="4194304" Id="block-id" Hash="md5-hash" />
               <Block Offset="1073741824" Length="4194304" Id="block-id" Hash="md5-hash" />
            </BlockList>
            <MetadataPath Hash="md5-hash">metadata-file-path</MetadataPath>
            <PropertiesPath Hash="md5-hash">properties-file-path</PropertiesPath>
         </Blob>
      </BlobList>
   </Drive>
</DriveManifest>
```

### <a name="sample-journal-file-xml-for-each-drive"></a>Minden olyan meghajtó minta napló fájl (XML)

```xml
[BeginUpdateRecord][2016/11/01 21:22:25.379][Type:ActivityRecord]
ActivityId: DriveInfo
DriveState: [BeginValue]
<?xml version="1.0" encoding="UTF-8"?>
<Drive>
   <DriveId>drive-id</DriveId>
   <BitLockerKey>*******</BitLockerKey>
   <ManifestFile>\DriveManifest.xml</ManifestFile>
   <ManifestHash>D863FE44F861AE0DA4DCEAEEFFCCCE68</ManifestHash> </Drive>
[EndValue]
SaveCommandOutput: Completed
[EndUpdateRecord]
```

### <a name="sample-journal-file-jrn-for-session-that-records-hello-trail-of-sessions"></a>Minta munkamenetet, amely rögzíti a munkamenetek hello ellenőrzési napló fájlt (JRN)

```
[BeginUpdateRecord][2016/11/02 18:24:14.735][Type:NewJournalFile]
VocabularyVersion: 2013-02-01
[EndUpdateRecord]
[BeginUpdateRecord][2016/11/02 18:24:14.749][Type:ActivityRecord]
ActivityId: PrepImportDriveCommandContext
LogDirectory: F:\logs
[EndUpdateRecord]
[BeginUpdateRecord][2016/11/02 18:24:14.754][Type:ActivityRecord]
ActivityId: PrepImportDriveCommandContext
StorageAccountKey: *******
[EndUpdateRecord]
```

## <a name="faq"></a>GYIK

### <a name="general"></a>Általános kérdések

#### <a name="what-is-waimportexport-tool"></a>Mi az WAImportExport eszköz?

hello WAImportExport eszköze hello meghajtó előkészítése és lemezjavító eszköz, amely hello Microsoft Azure Import/Export szolgáltatás használata. Az eszköz toocopy toohello rögzített adatmeghajtókon tooship tooan Azure-adatközpontban fog is használhatja. Az importálási feladat befejezése után is használhatja az eszköz toorepair megsérült, hiányoztak vagy ütköző blobot a más BLOB. Miután hello meghajtók kapott egy befejezett exportálási feladat, használhatja az eszköz toorepair, azokat a fájlokat, sérült vagy hiányzó hello meghajtókon.

#### <a name="how-does-hello-waimportexport-tool-work-on-multiple-source-dir-and-disks"></a>Hogyan segített hello WAImportExport eszköz több forrás dir és lemezek működnek?

Hello adatok mérete nagyobb, mint hello lemezméretet, ha hello WAImportExport eszköz optimalizált módon fog terjesztése hello adatok hello lemezek között. hello adatlemezek másolási toomultiple párhuzamosan vagy egymás után végezhető. Nincs a lemezek hello adatok hello száma korlátozva toosimultaneously lehet írni. hello eszköz kiosztja az adatokat a lemez mérete és a mappa mérete alapján. A legtöbb hello lemezt kiválaszt hello objektum méretű optimalizálva. feltöltött adatok hello toohello tárfiók lesz vissza összevont toohello megadott könyvtárstruktúrát.

#### <a name="where-can-i-find-previous-version-of-waimportexport-tool"></a>Hol találok WAImportExport eszköz korábbi verzióját?

WAImportExport az eszköz összes funkciói láthatók, amelyekről WAImportExport V1 eszköz rendelkezik. WAImportExport eszköz lehetővé teszi a felhasználók toospecify több források és írási toomultiple meghajtók. Emellett egy, könnyedén kezelhető, amelyből hello adatoknak kell toobe átmásolja a egy CSV-fájl több forráshelyet. Azonban esetben meg kell SAS támogatja, vagy szeretné, hogy toocopy egyetlen toosingle lemezt, akkor [letöltheti WAImportExport V1-es eszköz] (http://go.microsoft.com/fwlink/? LinkID = 301900&amp;clcid = 0x409), majd tekintse át a túl[WAImportExport V1 hivatkozás](storage-import-export-tool-how-to-v1.md) WAImportExport V1 felhasználású segítségét.

#### <a name="what-is-a-session-id"></a>Mi az a munkamenet-azonosító?

hello eszköz hello másolási munkamenet vár (/ azonosítója) paraméter toobe azonos hello, ha hello célja toospread hello adatok több lemezre. Karbantartása hello azonos hello másolási munkamenet neve lehetővé teszi egy vagy több adatforrás helyének be egy vagy több cél lemezek/könyvtárak felhasználói toocopy adatait. Azonos munkamenet-azonosító karbantartása lehetővé teszi, hogy a hello eszköz toopick, ahol azt volt korábban hello legutóbbi fájlok hello másolatát.

Egy példány munkamenet azonban nem lehet használt tooimport adatok toodifferent storage-fiókok.

Munkamenet-példány neve nem azonos hello eszköz több fut, amikor hello logfile (/ logdir) és a tárfiók kulcsa (/ sk) van is várt toobe hello azonos.

Munkamenet-azonosító tartalmazhat betűket, 0 ~ 9, understore (\_), kötőjelet (-) vagy a kivonatoló (#), és a hossza lehet 3 ~ 30.

például a munkamenet-1 vagy a munkamenet #1 vagy a munkamenet\_1

#### <a name="what-is-a-journal-file"></a>Mi az, hogy a napló-fájl?

Hello WAImportExport eszköz toocopy fájlok toohello merevlemez-meghajtóról, minden egyes futtatásakor hello eszköz létrehoz egy másolatot munkamenet. hello másolási munkamenet hello állapotát toohello napló fájl írása. Ha a Másolás munkamenet megszakad (például miatt tooa rendszer áramellátás megszakadása miatt), akkor hello eszköz újra és hello napló fájl hello parancssorban megadhatja folytathatók.

Mindegyik merevlemez, amely az Azure Import/Export eszköz hello készíteni, hello eszköz létre fog hozni egy egyetlen naplófájl nevű "&lt;meghajtót&gt;.xml" meghajtó azonosítója társított sorozatszám hello az eszköz hello toohello tartalmazó meghajtótól olvassa be az hello lemez. Az összes, a meghajtók toocreate hello importálási feladat kell hello Adatbázisnapló-fájlok. hello napló fájl is használt tooresume meghajtó előkészítése, ha hello eszköz megszakad.

#### <a name="what-is-a-log-directory"></a>Mi az az naplókönyvtár?

hello naplókönyvtár megadja egy könyvtár toobe toostore részletes naplókat, valamint az ideiglenes fájlok. Ha nincs megadva, hello aktuális könyvtárhoz hello naplókönyvtár lesz. nincsenek hello naplói. a részletes naplókat.

### <a name="prerequisites"></a>Előfeltételek

#### <a name="what-are-hello-specifications-of-my-disk"></a>Mik a lemez hello specifikációk?

Egy vagy több üres 2.5-es vagy 3,5 hüvelykes SATAII vagy III vagy rögzített SSD meghajtók csatlakoztatott toohello másolási gép.

#### <a name="how-can-i-enable-bitlocker-on-my-machine"></a>Hogyan engedélyezhetem a BitLocker a gépen?

Egyszerűen toocheck kattintson a jobb gombbal a rendszermeghajtón van. Megjelenik a BitLocker beállítások Ha hello funkció be van kapcsolva. Ha ki van kapcsolva, akkor nem látható.

![A BitLocker ellenőrzése](./media/storage-import-export-tool-preparing-hard-drives-import/BitLocker.png)

Íme egy cikk a [hogyan tooenable BitLocker](https://technet.microsoft.com/library/cc766295.aspx)

Akkor lehet, hogy a számítógép nem rendelkezik TPM lapka használata. Ha nem jelenik meg egy kimeneti TPM.msc parancsot használja, tekintse meg hello következő gyakran ismételt kérdések.

#### <a name="how-toodisable-trusted-platform-module-tpm-in-bitlocker"></a>Hogyan toodisable megbízható platformmegbízhatósági modul (TPM) a BitLocker?
> [!NOTE]
> Csak akkor, ha nincs TPM szerepel a kiszolgálókra, toodisable TPM házirend szüksége. Nincs szükség toodisable TPM Ha felhasználó kiszolgáló megbízható TPM-mel. 
> 

Rendelés toodisable BitLocker TPM nyissa meg a lépéseket követve hello keresztül:<br/>
1. Indítsa el **Helyicsoportházirend-szerkesztő** gpedit.msc parancsot a parancssorba beírásával. Ha **Helyicsoportházirend-szerkesztő** toobe nem érhető el, a BitLocker engedélyezéséhez először jelenik meg. Lásd az előző gyakran ismételt kérdések.
2. Nyissa meg **helyi számítógép-házirend &gt; számítógép konfigurációja &gt; felügyeleti sablonok &gt; Windows-összetevők&gt; a BitLocker meghajtótitkosítás &gt; operációsrendszer-meghajtók**.
3. Szerkesztés **indításkor további hitelesítést** házirend.
4. Hello házirendjének túl**engedélyezve** , és győződjön meg arról, hogy **BitLocker engedélyezése kompatibilis TPM nélküli** be van jelölve.

####  <a name="how-toocheck-if-net-4-or-higher-version-is-installed-on-my-machine"></a>Hogyan toocheck, ha a .NET 4-es vagy újabb verziója telepítve van a számítógépen?

A következő könyvtár telepített összes Microsoft .NET-keretrendszer verziója: %windir%\Microsoft.NET\Framework\

Keresse meg a fent említett része a célszámítógépen, amikor az eszköz hello kell toorun toohello. Keresse meg "v4" kezdetű mappa nevét. Nincs ilyen könyvtár azt jelenti, hogy a .NET 4 nincs telepítve a számítógépre. A gép használatával is letöltheti a .net-4 [Microsoft .NET-keretrendszer 4 (webes telepítő)](https://www.microsoft.com/download/details.aspx?id=17851).

### <a name="limits"></a>Korlátok

#### <a name="how-many-drives-can-i-preparesend-at-hello-same-time"></a>Hány meghajtók is szeretnék előkészítése/küldési: hello ugyanannyi időt vesz igénybe?

Előkészítheti eszköz hello lemezek számát hello korlátozva van. Azonban hello eszköz meghajtóbetűjelek bemeneteként vár. Amely korlátozza az too25 lemez egyidejű előkészítése. Egyetlen feladat egyszerre legfeljebb 10 lemezre kezelésére képes. Ha több mint 10 lemezek célcsoport-kezelési hello ugyanazt a tárfiókot, hello lemezek több feladat legyen elosztva.

#### <a name="can-i-target-more-than-one-storage-account"></a>Célozhatok egynél több tárfiókot?

Csak egy tárfiók küldheti el feladat és egy eredeti munkamenetben.

### <a name="capabilities"></a>Funkciók

#### <a name="does-waimportexportexe-encrypt-my-data"></a>Nem WAImportExport.exe az adatok titkosítása?

Igen. A bitlockernek engedélyezve van, és ez a folyamat szükséges.

#### <a name="what-will-be-hello-hierarchy-of-my-data-when-it-appears-in-hello-storage-account"></a>Mely hello tárfiókban megjelenésekor kell hello hierarchiáját adataimat?

Bár az adatok lemezek között van elosztva, hello feltöltött adatok toohello tárfiók lesz átszervezett hello dataset CSV-fájlban megadott toohello könyvtárstruktúrát biztonsági.

#### <a name="how-many-of-hello-input-disks-will-have-active-io-in-parallel-when-copy-is-in-progress"></a>Hány hello bemeneti lemezek aktív IO párhuzamosan, akkor lesz másolása folyamatban van?

hello eszköz adatok elosztja a lemezek bemeneti hello hello hello bemeneti fájlok mérete alapján. Említett, hello száma párhuzamos aktív lemezek teljesen delends hello jellegéről hello bemeneti adatok. Attól függően, hogy az hello bemeneti adatkészlet egyes fájlok mérete hello egy vagy több lemezt is megjelenítése a aktív IO párhuzamosan. Következő kérdés további részletekért tekintse meg.

#### <a name="how-does-hello-tool-distribute-hello-files-across-hello-disks"></a>Hogyan nem hello eszköz szét hello fájlok hello lemezek?

WAImportExport eszköz beolvassa és kötegelt által kötegelt fájlok egy kötegelt maximális értékének 100000 fájlokat tartalmazza. Ez azt jelenti, hogy maximális 100000 fájlok írhatók párhuzamos. Több lemez írt toosimultaneously, ha a 100000 fájlok elosztott toomulti meghajtókat. Azonban e hello eszköz ír toomultiple lemezek egyszerre, vagy egyetlen lemez hello összesített mérete hello kötegelt függ. Például esetén a kisebb fájlok, ha 10,0000 fájlok összes képes toofit egyetlen meghajtóval, az eszköz ír tooonly egy lemez hello feldolgozása során.

### <a name="waimportexport-output"></a>WAImportExport kimeneti

#### <a name="there-are-two-journal-files-which-one-should-i-upload-tooazure-portal"></a>Két napló fájlt, melyiket kell tooAzure portal tölthetők fel?

**.xml** -mindegyik merevlemez, amely a hello WAImportExport eszközével előkészítheti, hello eszköz létre fog hozni egy egyetlen naplófájl nevű `<DriveID>.xml` meghajtót az társított sorozatszám hello eszköz hello toohello tartalmazó meghajtótól hello lemez olvassa be. Az összes, a meghajtók toocreate hello importálási feladat a hello Azure-portálon kell hello Adatbázisnapló-fájlok. Ez a napló fájl is használt tooresume meghajtó előkészítése, ha hello eszköz megszakad.

**.jrn** -hello napló fájl utótaggal rendelkező `.jrn` merevlemez-meghajtóra vonatkozó összes másolási munkamenet hello állapotát tartalmazza. Információkat is tartalmaz hello szükséges toocreate hello importálási feladat. Mindig meg kell adnia egy naplófájl, amikor a futó hello WAImportExport eszköz, valamint a Másolás munkamenet-azonosító.

## <a name="next-steps"></a>Következő lépések

* [Hello Azure Import/Export eszköz beállítása](storage-import-export-tool-setup.md)
* [A tulajdonságok és metaadatok során hello importálási folyamat](storage-import-export-tool-setting-properties-metadata-import.md)
* [Minta munkafolyamat tooprepare merevlemezeit, az importálási feladat](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow.md)
* [A gyakran használt parancsok rövid összefoglalása](storage-import-export-tool-quick-reference.md) 
* [Feladatok állapotának áttekintése a másolási naplófájlok segítségével](storage-import-export-tool-reviewing-job-status-v1.md)
* [Importálási feladat javítása](storage-import-export-tool-repairing-an-import-job-v1.md)
* [Exportálási feladat javítása](storage-import-export-tool-repairing-an-export-job-v1.md)
* [Hibaelhárítási hello Azure Import/Export eszköz](storage-import-export-tool-troubleshooting-v1.md)
