---
title: "aaaAzure importálási/exportálási formátumát |} Microsoft Docs"
description: "További információk a lépéseket az Import/Export szolgáltatás feladat végrehajtásakor létrehozott hello naplófájlok hello formátuma."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 38cc16bd-ad55-4625-9a85-e1726c35fd1b
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 15a652455aa947922af0aa39ccefe68811a3db19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-importexport-service-log-file-format"></a>Az Azure Import/Export szolgáltatás naplófájlformátum
Amikor hello Microsoft Azure Import/Export szolgáltatás hajt végre, az importálási feladat vagy exportálási feladat részeként egy műveletet a meghajtón, írja a naplókat tooblock blobok adott feladattal társított hello tárfiókban.  
  
Hello Import/Export szolgáltatás által írható két naplók van:  
  
-   hello hibanapló mindig egy hiba hello esemény jön létre.  
  
-   hello részletes naplózás alapértelmezés szerint nincs engedélyezve, de előfordulhat, hogy engedélyezni lehessen hello beállítása `EnableVerboseLog` tulajdonságának egy [Put feladat](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) vagy [frissítés Feladattulajdonság](/rest/api/storageimportexport/jobs#Jobs_Update) műveletet.  
  
## <a name="log-file-location"></a>Naplófájl helye  
hello írja a naplókat blobok tooblock hello tároló vagy hello által megadott virtuális könyvtár `ImportExportStatesPath` beállítás, amely állíthat be egy `Put Job` műveletet. Írja a hello hely toowhich hello naplókat attól függ, hogyan hitelesítési hello feladat együtt megadott hello értéke meg van adva `ImportExportStatesPath`. A tárfiók kulcsa, vagy a tároló SAS (közös hozzáférésű jogosultságkód) hitelesítési hello feladat adható meg.  
  
hello hello tároló vagy a virtuális könyvtár nevét is vagy hello alapértelmezett neve `waimportexport`, vagy egy másik tárolóhoz vagy a megadott virtuális könyvtár neve.  
  
az alábbi táblázat hello hello lehetséges lehetőségeket mutatja be:  
  
|Hitelesítési módszer|Az érték `ImportExportStatesPath`elem|Blobok napló helye|  
|---------------------------|----------------------------------------------|---------------------------|  
|Tárfiók kulcsa|Alapértelmezett érték|Nevű tárolót `waimportexport`, ez az alapértelmezett tároló hello. Példa:<br /><br /> `https://myaccount.blob.core.windows.net/waimportexport`|  
|Tárfiók kulcsa|A felhasználó által megadott érték|Hello felhasználó nevű tárolót. Példa:<br /><br /> `https://myaccount.blob.core.windows.net/mylogcontainer`|  
|A tároló SAS|Alapértelmezett érték|Nevű virtuális könyvtár `waimportexport`, amely hello alapértelmezett nevét, hello SAS megadott hello tároló alatt.<br /><br /> Például, ha a megadott hello SAS hello folyamat `https://myaccount.blob.core.windows.net/mylogcontainer?sv=2012-02-12&se=2015-05-22T06%3A54%3A55Z&sr=c&sp=wl&sig=sigvalue`, majd hello napló helyét lenne.`https://myaccount.blob.core.windows.net/mylogcontainer/waimportexport`|  
|A tároló SAS|A felhasználó által megadott érték|Hello felhasználó alatt megadott hello SAS hello tároló nevű virtuális könyvtárat.<br /><br /> Például, ha a megadott hello SAS hello folyamat `https://myaccount.blob.core.windows.net/mylogcontainer?sv=2012-02-12&se=2015-05-22T06%3A54%3A55Z&sr=c&sp=wl&sig=sigvalue`, és hello megadott virtuális könyvtár neve `mylogblobs`, majd hello napló helyét lenne `https://myaccount.blob.core.windows.net/mylogcontainer/waimportexport/mylogblobs`.|  
  
Hello URL-címe hello hiba és a részletes naplókat hívó hello olvashatók be [Get Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) műveletet. hello érhetők el naplók hello meghajtó feldolgozás befejezése után.  
  
## <a name="log-file-format"></a>Naplófájlformátum  
hello mindkét naplók formátumban van hello ugyanaz: hello az eseményeket, amelyek a blobok hello merevlemezre és hello felhasználói fiókhoz közötti másolás közben történt az XML-leírását tartalmazó blob.  
  
hello részletes teljes vonatkozó információkat tartalmaz hello állapot hello másolási művelet minden egyes blob (hogy az importálás) vagy a fájlt (exportálási feladat), mivel hello hibanapló csak hello adatait blobok vagy hibát közben hello fájlokat tartalmazza importálhatja és exportálhatja a feladatot.  
  
hello részletes napló formátuma alább látható. hello hibanapló hello azonos struktúra, de sikeres műveletek kiszűri rendelkezik.  

```xml
<DriveLog Version="2014-11-01">  
  <DriveId>drive-id</DriveId>  
  [<Blob Status="blob-status">  
   <BlobPath>blob-path</BlobPath>  
   <FilePath>file-path</FilePath>  
   [<Snapshot>snapshot</Snapshot>]  
   <Length>length</Length>  
   [<LastModified>last-modified</LastModified>]  
   [<ImportDisposition Status="import-disposition-status">import-disposition</ImportDisposition>]  
   [page-range-list-or-block-list]  
   [metadata-status]  
   [properties-status]  
  </Blob>]  
  [<Blob>  
    . . .  
  </Blob>]  
  <Status>drive-status</Status>  
</DriveLog>  
  
page-range-list-or-block-list ::= 
  page-range-list | block-list  
  
page-range-list ::=   
<PageRangeList>  
      [<PageRange Offset="page-range-offset" Length="page-range-length"   
       [Hash="md5-hash"] Status="page-range-status"/>]  
      [<PageRange Offset="page-range-offset" Length="page-range-length"   
       [Hash="md5-hash"] Status="page-range-status"/>]  
</PageRangeList>  
  
block-list ::=  
<BlockList>  
      [<Block Offset="block-offset" Length="block-length" [Id="block-id"]  
       [Hash="md5-hash"] Status="block-status"/>]  
      [<Block Offset="block-offset" Length="block-length" [Id="block-id"]   
       [Hash="md5-hash"] Status="block-status"/>]  
</BlockList>  
  
metadata-status ::=  
<Metadata Status="metadata-status">  
   [<GlobalPath Hash="md5-hash">global-metadata-file-path</GlobalPath>]  
   [<Path Hash="md5-hash">metadata-file-path</Path>]  
</Metadata>  
  
properties-status ::=  
<Properties Status="properties-status">  
   [<GlobalPath Hash="md5-hash">global-properties-file-path</GlobalPath>]  
   [<Path Hash="md5-hash">properties-file-path</Path>]  
</Properties>  
```

hello következő táblázatban hello elemek hello naplófájl.  
  
|XML-elem.|Típus|Leírás|  
|-----------------|----------|-----------------|  
|`DriveLog`|XML-elem.|A meghajtó napló jelöli.|  
|`Version`|Attribútum, karakterlánc|hello naplóformátumban hello verzióját.|  
|`DriveId`|Karakterlánc|hello meghajtó hardver sorozatszámát.|  
|`Status`|Karakterlánc|Hello meghajtó feldolgozási állapotát. Lásd: hello `Drive Status Codes` tábla alább olvashat.|  
|`Blob`|Beágyazott XML-elem.|Egy blob jelöli.|  
|`Blob/BlobPath`|Karakterlánc|hello hello blob URI Azonosítóját.|  
|`Blob/FilePath`|Karakterlánc|hello relatív elérési út toohello fájl hello meghajtón.|  
|`Blob/Snapshot`|Dátum és idő|hello blob, csak exportálási feladat hello pillanatkép verzióját.|  
|`Blob/Length`|Egész szám|hello teljes hossza hello blob bájtban.|  
|`Blob/LastModified`|Dátum és idő|hello dátum/idő, hogy a hello blob voltak utoljára módosítva, csak az exportálási feladat.|  
|`Blob/ImportDisposition`|Karakterlánc|hello importálása hello BLOB, hogy az importálás csak törlése.|  
|`Blob/ImportDisposition/@Status`|Attribútum, karakterlánc|hello hello állapotának importálása törlése.|  
|`PageRangeList`|Beágyazott XML-elem.|Az oldalakra vonatkozó blob tartományokat listáját jelöli.|  
|`PageRange`|XML-elem.|Egy laptartomány jelöli.|  
|`PageRange/@Offset`|Attribútum, egész szám|Hello blob eltolása hello laptartomány kezdve.|  
|`PageRange/@Length`|Attribútum, egész szám|Hello laptartomány hossza (bájt).|  
|`PageRange/@Hash`|Attribútum, karakterlánc|Base16 kódolású MD5 kivonatoló hello lap tartomány.|  
|`PageRange/@Status`|Attribútum, karakterlánc|Hello laptartomány feldolgozási állapotát.|  
|`BlockList`|Beágyazott XML-elem.|Egy blokkblobhoz tartoznak blokkok listája jelöli.|  
|`Block`|XML-elem.|A blokk jelöli.|  
|`Block/@Offset`|Attribútum, egész szám|Kezdő eltolása hello blob hello blokkja.|  
|`Block/@Length`|Attribútum, egész szám|Hello blokk hossza bájtban.|  
|`Block/@Id`|Attribútum, karakterlánc|hello blokk-azonosító.|  
|`Block/@Hash`|Attribútum, karakterlánc|Base16 kódolású MD5 kivonatoló hello blokk.|  
|`Block/@Status`|Attribútum, karakterlánc|Hello blokk feldolgozási állapotát.|  
|`Metadata`|Beágyazott XML-elem.|Hello blob metaadatai jelöli.|  
|`Metadata/@Status`|Attribútum, karakterlánc|Hello blob metaadatok feldolgozási állapotát.|  
|`Metadata/GlobalPath`|Karakterlánc|Relatív elérési út toohello globális metaadatait tartalmazó fájl.|  
|`Metadata/GlobalPath/@Hash`|Attribútum, karakterlánc|Base16 kódolású MD5 kivonatoló hello globális metaadatfájl.|  
|`Metadata/Path`|Karakterlánc|Relatív elérési út toohello metaadatait tartalmazó fájl.|  
|`Metadata/Path/@Hash`|Attribútum, karakterlánc|Base16 kódolású MD5 kivonatoló hello metaadatfájl.|  
|`Properties`|Beágyazott XML-elem.|Hello blob tulajdonságait adja meg.|  
|`Properties/@Status`|Attribútum, karakterlánc|Feldolgozási hello blob tulajdonságai, például a fájl nem található, állapotának befejeződött.|  
|`Properties/GlobalPath`|Karakterlánc|Toohello globális tulajdonságok fájl relatív elérési út.|  
|`Properties/GlobalPath/@Hash`|Attribútum, karakterlánc|Base16 kódolású MD5 kivonatoló hello globális tulajdonságok fájl.|  
|`Properties/Path`|Karakterlánc|Toohello tulajdonságok fájl relatív elérési út.|  
|`Properties/Path/@Hash`|Attribútum, karakterlánc|Base16 kódolású MD5 kivonatoló hello tulajdonságok fájl.|  
|`Blob/Status`|Karakterlánc|Hello blob feldolgozási állapotát.|  
  
# <a name="drive-status-codes"></a>Meghajtó állapotkódok  
hello következő táblázatban hello állapotkódok egy meghajtó feldolgozásához.  
  
|Állapotkód|Leírás|  
|-----------------|-----------------|  
|`Completed`|hello meghajtó feldolgozási hiba nélkül befejeződött.|  
|`CompletedWithWarnings`|hello meghajtó figyelmeztetésekkel fejeződött be a / hello importálási dispositions hello blobok megadott egy vagy több blobot feldolgozása befejeződött.|  
|`CompletedWithErrors`|egy vagy több blobot, vagy adattömbök hello meghajtó befejeződött.|  
|`DiskNotFound`|Nincs lemez hello meghajtón található.|  
|`VolumeNotNtfs`|hello első adatok lemezen levő kötetet a hello nem nem NTFS formátumú.|  
|`DiskOperationFailed`|Ismeretlen hiba történt, amikor hello meghajtón műveletet hajt végre.|  
|`BitLockerVolumeNotFound`|A BitLocker encryptable kötet található.|  
|`BitLockerNotActivated`|A BitLocker nincs engedélyezve a hello kötet.|  
|`BitLockerProtectorNotFound`|hello numerikus jelszavas kötetvédő nem létezik hello köteten.|  
|`BitLockerKeyInvalid`|hello numerikus jelszót adott meg, nem oldhatja fel hello kötet.|  
|`BitLockerUnlockVolumeFailed`|Ismeretlen hiba történt a toounlock hello kötet tett kísérlet során.|  
|`BitLockerFailed`|Ismeretlen hiba történt a BitLocker műveletet hajt végre.|  
|`ManifestNameInvalid`|hello jegyzékfájl neve érvénytelen.|  
|`ManifestNameTooLong`|hello jegyzékfájl neve túl hosszú.|  
|`ManifestNotFound`|hello jegyzékfájl nem található.|  
|`ManifestAccessDenied`|Toohello jegyzékfájl-hozzáférés megtagadva.|  
|`ManifestCorrupted`|hello Alkalmazásjegyzék-fájl sérült (hello tartalom nem felel meg a kivonatoló).|  
|`ManifestFormatInvalid`|hello jegyzék tartalom nem felel meg a toohello megkívánt formátumban.|  
|`ManifestDriveIdMismatch`|hello meghajtó hello jegyzékfájl-azonosító nem egyezik meg a hello egy hello meghajtóról olvasni.|  
|`ReadManifestFailed`|A lemez i/o-hiba történt a következő jegyzékből hello olvasás közben.|  
|`BlobListFormatInvalid`|hello exportálási blob lista blob nem felel meg a toohello megkívánt formátumban.|  
|`BlobRequestForbidden`|Blobok elérése az toohello hello tárfiókban tiltott. Előfordulhat, hogy ennek lehet tooinvalid tárfiók kulcsa vagy a tároló SAS.|  
|`InternalError`|És hello meghajtó feldolgozása közben belső hiba történt.|  
  
## <a name="blob-status-codes"></a>A BLOB állapotkódok  
hello következő táblázatban hello állapotkódok blob feldolgozásához.  
  
|Állapotkód|Leírás|  
|-----------------|-----------------|  
|`Completed`|hello blob feldolgozási hiba nélkül befejeződött.|  
|`CompletedWithErrors`|hello blob hibák egy vagy több tartományokat vagy blokkok, metaadatok vagy-tulajdonságok feldolgozása befejeződött.|  
|`FileNameInvalid`|hello neve érvénytelen.|  
|`FileNameTooLong`|hello fájl neve túl hosszú.|  
|`FileNotFound`|hello fájl nem található.|  
|`FileAccessDenied`|Toohello fájl elérése megtagadva.|  
|`BlobRequestFailed`|tooaccess hello blob hello Blob szolgáltatási kérelem sikertelen volt.|  
|`BlobRequestForbidden`|hello Blob szolgáltatási kérelem tooaccess hello blob tiltott. Előfordulhat, hogy ennek lehet tooinvalid tárfiók kulcsa vagy a tároló SAS.|  
|`RenameFailed`|Nem sikerült toorename hello blob (hogy az importálás) vagy hello fájlt (exportálási feladat).|  
|`BlobUnexpectedChange`|(Az exportálási feladat) hello blob egy váratlan módosítás történt.|  
|`LeasePresent`|Nincs a címbérlet hello blob megtalálható.|  
|`IOFailed`|A lemez- vagy hálózati i/o-hiba történt a hello blob feldolgozásakor.|  
|`Failed`|Ismeretlen hiba történt a hello blob feldolgozása során.|  
  
## <a name="import-disposition-status-codes"></a>Importálás törlése állapotkódok  
hello következő táblázatban hello állapotkódjai feloldása egy importálási törlése.  
  
|Állapotkód|Leírás|  
|-----------------|-----------------|  
|`Created`|hello blob létrehozását.|  
|`Renamed`|hello blob egy átnevezési importálási törlése át lett nevezve. Hello `Blob/BlobPath` elem átnevezése hello BLOB URI hello tartalmazza.|  
|`Skipped`|hello blob ki lett hagyva `no-overwrite` importálása törlése.|  
|`Overwritten`|hello blob felülírta egy meglévő blob / `overwrite` importálása törlése.|  
|`Cancelled`|Egy korábbi hiba hello importálási témakör további feldolgozása leállt.|  
  
## <a name="page-rangeblock-status-codes"></a>Tartomány/blokk állapotkódok lap  
hello következő táblázatban hello állapotkódok tartományt vagy egy tiltólista feldolgozásra.  
  
|Állapotkód|Leírás|  
|-----------------|-----------------|  
|`Completed`|hello lap tartomány vagy blokk feldolgozási hiba nélkül befejeződött.|  
|`Committed`|hello blokk véglegesítve, de nem a hello teljes tiltólista, mert más blokkok sikertelen, vagy helyezze magát teljes tiltólista sikertelen volt.|  
|`Uncommitted`|hello blokk feltölteni, de a nem véglegesített.|  
|`Corrupted`|hello lap tartomány vagy blokk sérült (hello tartalom nem felel meg a kivonatoló).|  
|`FileUnexpectedEnd`|Egy nem várt fájlvégjel történt.|  
|`BlobUnexpectedEnd`|Egy blob váratlan vége történt.|  
|`BlobRequestFailed`|Blob szolgáltatási kérelem tooaccess hello laptartomány hello, vagy blokk nem sikerült.|  
|`IOFailed`|A lemez- vagy hálózati i/o-hiba történt hello lap tartomány vagy blokk feldolgozása közben.|  
|`Failed`|Ismeretlen hiba történt a hello lap tartomány vagy blokk feldolgozása során.|  
|`Cancelled`|Egy korábbi hiba hello lap tartomány vagy blokk további feldolgozás leállt.|  
  
## <a name="metadata-status-codes"></a>Metaadatok állapotkódok  
hello következő táblázatban hello állapotkódjai feldolgozási blob metaadatait.  
  
|Állapotkód|Leírás|  
|-----------------|-----------------|  
|`Completed`|hello metaadatok feldolgozási hiba nélkül befejeződött.|  
|`FileNameInvalid`|hello metaadat neve érvénytelen.|  
|`FileNameTooLong`|hello metaadatok fájl neve túl hosszú.|  
|`FileNotFound`|hello metaadatait tartalmazó fájl nem található.|  
|`FileAccessDenied`|Toohello metaadatfájl hozzáférés megtagadva.|  
|`Corrupted`|hello metaadatait tartalmazó fájl sérült (hello tartalom nem felel meg a kivonatoló).|  
|`XmlReadFailed`|hello metaadatok tartalom nem felel meg a toohello megkívánt formátumban.|  
|`XmlWriteFailed`|Írás hello metaadatok XML sikertelen volt.|  
|`BlobRequestFailed`|tooaccess hello metaadatok hello Blob szolgáltatási kérelem sikertelen volt.|  
|`IOFailed`|Lemez- vagy i/o-hiba történt a hello metaadatok feldolgozásakor a program.|  
|`Failed`|Ismeretlen hiba történt a hello metaadatok feldolgozásakor a program.|  
|`Cancelled`|Egy korábbi hiba hello metaadatok további feldolgozás leállt.|  
  
## <a name="properties-status-codes"></a>Tulajdonságok állapotkódok  
hello következő táblázatban hello állapotkódjai blob tulajdonságok feldolgozása.  
  
|Állapotkód|Leírás|  
|-----------------|-----------------|  
|`Completed`|hello tulajdonságok feldolgozása hiba nélkül befejeződött.|  
|`FileNameInvalid`|hello tulajdonságok neve érvénytelen.|  
|`FileNameTooLong`|hello tulajdonságok fájl neve túl hosszú.|  
|`FileNotFound`|hello tulajdonságok fájl nem található.|  
|`FileAccessDenied`|Toohello tulajdonságok fájl elérése megtagadva.|  
|`Corrupted`|hello tulajdonságok fájl sérült (hello tartalom nem felel meg a kivonatoló).|  
|`XmlReadFailed`|hello tulajdonságok tartalom nem felel meg a toohello megkívánt formátumban.|  
|`XmlWriteFailed`|A tulajdonságoknak a hello XML sikertelen volt.|  
|`BlobRequestFailed`|tooaccess hello tulajdonságok hello Blob szolgáltatási kérelem sikertelen volt.|  
|`IOFailed`|A lemez- vagy hálózati i/o-hiba történt a hello tulajdonságok feldolgozása során.|  
|`Failed`|Ismeretlen hiba történt a hello tulajdonságok feldolgozása során.|  
|`Cancelled`|Egy korábbi hiba hello tulajdonságok további feldolgozás leállt.|  
  
## <a name="sample-logs"></a>A minta-naplók  
hello az alábbiakban látható egy példa részletes naplózás.  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<DriveLog Version="2014-11-01">  
    <DriveId>WD-WMATV123456</DriveId>  
    <Blob Status="Completed">  
       <BlobPath>pictures/bob/wild/desert.jpg</BlobPath>  
       <FilePath>\Users\bob\Pictures\wild\desert.jpg</FilePath>  
       <Length>98304</Length>  
       <ImportDisposition Status="Created">overwrite</ImportDisposition>  
       <BlockList>  
          <Block Offset="0" Length="65536" Id="AAAAAA==" Hash=" 9C8AE14A55241F98533C4D80D85CDC68" Status="Completed"/>  
          <Block Offset="65536" Length="32768" Id="AQAAAA==" Hash=" DF54C531C9B3CA2570FDDDB3BCD0E27D" Status="Completed"/>  
       </BlockList>  
       <Metadata Status="Completed">  
          <GlobalPath Hash=" E34F54B7086BCF4EC1601D056F4C7E37">\Users\bob\Pictures\wild\metadata.xml</GlobalPath>  
       </Metadata>  
    </Blob>  
    <Blob Status="CompletedWithErrors">  
       <BlobPath>pictures/bob/animals/koala.jpg</BlobPath>  
       <FilePath>\Users\bob\Pictures\animals\koala.jpg</FilePath>  
       <Length>163840</Length>  
       <ImportDisposition Status="Overwritten">overwrite</ImportDisposition>  
       <PageRangeList>  
          <PageRange Offset="0" Length="65536" Hash="19701B8877418393CB3CB567F53EE225" Status="Completed"/>  
          <PageRange Offset="65536" Length="65536" Hash="AA2585F6F6FD01C4AD4256E018240CD4" Status="Corrupted"/>  
          <PageRange Offset="131072" Length="4096" Hash="9BA552E1C3EEAFFC91B42B979900A996" Status="Completed"/>  
       </PageRangeList>  
       <Properties Status="Completed">  
          <Path Hash="38D7AE80653F47F63C0222FEE90EC4E7">\Users\bob\Pictures\animals\koala.jpg.properties</Path>  
       </Properties>  
    </Blob>  
    <Status>CompletedWithErrors</Status>  
</DriveLog>  
```  
  
hello megfelelő hibanapló alább láthatók.  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<DriveLog Version="2014-11-01">  
    <DriveId>WD-WMATV6965824</DriveId>  
    <Blob Status="CompletedWithErrors">  
       <BlobPath>pictures/bob/animals/koala.jpg</BlobPath>  
       <FilePath>\Users\bob\Pictures\animals\koala.jpg</FilePath>  
       <Length>163840</Length>  
       <ImportDisposition Status="Overwritten">overwrite</ImportDisposition>  
       <PageRangeList>  
          <PageRange Offset="65536" Length="65536" Hash="AA2585F6F6FD01C4AD4256E018240CD4" Status="Corrupted"/>  
       </PageRangeList>  
    </Blob>  
    <Status>CompletedWithErrors</Status>  
</DriveLog>  
```

 hello kövesse hibanaplóját, hogy az importálás hello importálási meghajtón nem található fájlokról hibát tartalmaz. Vegye figyelembe, hogy további összetevők hello állapotának `Cancelled`.  
  
```xml
<?xml version="1.0" encoding="utf-8"?>  
<DriveLog Version="2014-11-01">  
  <DriveId>9WM35C2V</DriveId>  
  <Blob Status="FileNotFound">  
    <BlobPath>pictures/animals/koala.jpg</BlobPath>  
    <FilePath>\animals\koala.jpg</FilePath>  
    <Length>30310</Length>  
    <ImportDisposition Status="Cancelled">rename</ImportDisposition>  
    <BlockList>  
      <Block Offset="0" Length="6062" Id="MD5/cAzn4h7VVSWXf696qp5Uaw==" Hash="700CE7E21ED55525977FAF7AAA9E546B" Status="Cancelled" />  
      <Block Offset="6062" Length="6062" Id="MD5/PEnGwYOI8LPLNYdfKr7kAg==" Hash="3C49C6C18388F0B3CB35875F2ABEE402" Status="Cancelled" />  
      <Block Offset="12124" Length="6062" Id="MD5/FG4WxqfZKuUWZ2nGTU2qVA==" Hash="146E16C6A7D92AE5166769C64D4DAA54" Status="Cancelled" />  
      <Block Offset="18186" Length="6062" Id="MD5/ZzibNDzr3IRBQENRyegeXQ==" Hash="67389B343CEBDC8441404351C9E81E5D" Status="Cancelled" />  
      <Block Offset="24248" Length="6062" Id="MD5/ZzibNDzr3IRBQENRyegeXQ==" Hash="67389B343CEBDC8441404351C9E81E5D" Status="Cancelled" />  
    </BlockList>  
  </Blob>  
  <Status>CompletedWithErrors</Status>  
</DriveLog>  
```

hello exportálási feladat következő hibanaplóban azt jelzi, hogy hello blob tartalma sikeresen írt toohello meghajtó, azonban, hogy hiba történt a hello blob tulajdonságai exportálása során.  
  
```xml
<?xml version="1.0" encoding="utf-8"?>  
<DriveLog Version="2014-11-01">  
  <DriveId>9WM35C3U</DriveId>  
  <Blob Status="CompletedWithErrors">  
    <BlobPath>pictures/wild/canyon.jpg</BlobPath>  
    <FilePath>\pictures\wild\canyon.jpg</FilePath>  
    <LastModified>2012-09-18T23:47:08Z</LastModified>  
    <Length>163840</Length>  
    <BlockList />  
    <Properties Status="Failed" />  
  </Blob>  
  <Status>CompletedWithErrors</Status>  
</DriveLog>  
```
  
## <a name="next-steps"></a>Következő lépések
 
* [Storage Import/Export REST API felülete](/rest/api/storageimportexport/)
