---
title: "aaaRepairing Azure Import/Export exportálási feladat - 1-es verzió |} Microsoft Docs"
description: "Ismerje meg, hogyan toorepair lett létrehozva, és futtassa az exportálási feladatot hello Azure Import/Export szolgáltatás."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 728e2a42-04ce-4be8-9375-e9e2bc6827a5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 96c674fc7c697c37882fb2980c340303896ac6c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="repairing-an-export-job"></a>Exportálási feladat javítása
Exportálási feladat befejezése után a Microsoft Azure Import/Export eszköz helyszíni hello is futtathatja:  
  
1.  Töltse le a fájlokat, hogy hello Azure Import/Export szolgáltatás nem tooexport volt-e.  
  
2.  Ellenőrizze, hogy helyesen lettek-e exportálva hello hello meghajtón.  
  
Ez a funkció kapcsolat tooAzure tárolási toouse kell rendelkeznie.  
  
az importálási feladat javítási hello parancs **RepairExport**.

## <a name="repairexport-parameters"></a>RepairExport paraméterek

hello következő paraméterek adható **RepairExport**:  
  
|Paraméter|Leírás|  
|---------------|-----------------|  
|**r: < RepairFile\>**|Kötelező. Elérési út toohello javítási fájl, amely hello javítási hello előrehaladását követi nyomon, és lehetővé teszi a tooresume az megszakított javítását. Minden olyan meghajtó meg kell adni egy javítási fájlt. Amikor elindít egy adott meghajtó javítása, át lesz fájlban hello elérési tooa javítás még nem létezik. tooresume az megszakított javítását, meg kell adjon át egy meglévő javítási fájl hello neve. hello javítási fájl megfelelő toohello célmeghajtó mindig meg kell adni.|  
|**/ logdir: < LogDirectory\>**|Választható. hello naplókönyvtár. Részletes naplófájlok toothis directory lesz írva. Ha nincs naplókönyvtár meg van adva, hello aktuális könyvtárhoz hello naplókönyvtár lesz.|  
|**/ d: < TargetDirectory\>**|Kötelező. hello directory toovalidate és javítása. Ez általában hello hello exportálási meghajtó gyökérkönyvtárába, de sikerült is kell egy hálózati fájlmegosztásra tartalmazó hello exportált fájlokat egy példányát.|  
|**/BK: < BitLockerKey\>**|Választható. Hello BitLocker kulcsot kell megadnia, ha azt szeretné, hogy hello eszköz toounlock egy titkosított amikor hello exportált fájlokat tárolja.|  
|**/sn: < StorageAccountName\>**|Kötelező. hello hello hello storage-fiók nevét a feladat exportálja.|  
|**/SK: < StorageAccountKey\>**|**Szükséges** csak, ha a tároló SAS nincs megadva. hello fiók kulcsának hello hello a feladat exportálja.|  
|**/csas: < ContainerSas\>**|**Szükséges** csak, ha nincs megadva hello tárfiók kulcsa. hello tároló SAS hello exportálási feladat társított hello blobok eléréséhez.|  
|**/ CopyLogFile: < DriveCopyLogFile\>**|Kötelező. hello elérési toohello meghajtó másolása naplófájlt. hello fájl hello Windows Azure Import/Export szolgáltatás által generált és tölthető le: hello hello feladattal társított blob-tároló. hello napló-fájl másolása sikertelen blobokkal vagy a fájlokat, amelyek javítani toobe információkat tartalmaz.|  
|**/ ManifestFile: < DriveManifestFile\>**|Választható. elérési út toohello exportálási meghajtó jegyzékfájl hello. Ez a fájl hello Windows Azure Import/Export szolgáltatás által létrehozott és hello exportálási meghajtón, és szükség esetén egy blobba hello feladattal társított hello tárfiókban tárolja.<br /><br /> hello hello exportálási meghajtón lévő fájlokat a rendszer ellenőrzi az ebben a fájlban található hello MD5 kivonatok hello tartalmát. Sérült meghatározott toobe fájlokon letöltött és egy átírt toohello cél könyvtárak lesz.|  
  
## <a name="using-repairexport-mode-toocorrect-failed-exports"></a>RepairExport használatával mód toocorrect kivitel sikertelen  
Nem sikerült tooexport hello Azure Import/Export eszköz toodownload fájlok is használhatja. hello másolási naplófájlt fogja tartalmazni, melyeknél nem sikerült tooexport fájlok listáját.  
  
hello exportálási hiba okai hello a következő lehetőségeket:  
  
-   Sérült meghajtók  
  
-   tárfiók kulcsa hello hello átviteli folyamat során megváltozott  
  
toorun hello eszköz **RepairExport** mód, először tooconnect hello meghajtóján hello exportált fájlokat tooyour számítógép. Ezután futtassa az Azure Import/Export eszköz hello elérési toothat meghajtó megadása a hello hello `/d` paraméter. Meg kell toospecify hello elérési toohello meghajtó másolása naplófájl letöltött is. hello következő parancssor például az alábbi futtató hello eszköz toorepair azokat a fájlokat, tooexport sikertelen:  
  
```  
WAImportExport.exe RepairExport /r:C:\WAImportExport\9WM35C3U.rep /d:G:\ /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\WAImportExport\9WM35C3U.log  
```  
  
hello következő másolási naplófájl jeleníti meg, hogy egy blokk hello meghiúsult tooexport példája:  
  
```xml
<?xml version="1.0" encoding="utf-8"?>  
<DriveLog>  
  <DriveId>9WM35C2V</DriveId>  
  <Blob Status="CompletedWithErrors">  
    <BlobPath>pictures/wild/desert.jpg</BlobPath>  
    <FilePath>\pictures\wild\desert.jpg</FilePath>  
    <LastModified>2012-09-18T23:47:08Z</LastModified>  
    <Length>163840</Length>  
    <BlockList>  
      <Block Offset="65536" Length="65536" Id="AQAAAA==" Status="Failed" />  
    </BlockList>  
  </Blob>  
  <Status>CompletedWithErrors</Status>  
</DriveLog>  
```  
  
hello másolási naplófájl azt jelzi, hogy hiba történt a Windows Azure Import/Export szolgáltatás hello lett letöltése egyik hello blob blokkok toohello hello exportálási meghajtón. hello más összetevők hello fájl letöltése sikeresen befejeződött, és hello fájl hosszát helyesen be lett állítva. Ebben az esetben hello eszköz hello meghajtón hello fájl megnyitása, letölthesse hello blokk hello tárfiókból, és írja a 65536 értékű eltolás hosszúságú 65536-től kezdődő toohello fájl tartományon.  
  
## <a name="using-repairexport-toovalidate-drive-contents"></a>RepairExport toovalidate meghajtó-tartalmak segítségével  
Azure Import/Export is használható hello **RepairExport** beállítás toovalidate hello tartalma hello meghajtón helyesek. minden exportálási meghajtón hello jegyzékfájl MD5s hello meghajtó hello tartalmát tartalmazza.  
  
hello Azure Import/Export szolgáltatás is mentheti hello jegyzékfájlt tooa tárfiók hello exportálási folyamat során. hello hello fájlok helye elérhető hello [Get Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) műveletet, amikor a hello feladat befejeződött. Lásd: [Import/Export service Manifest fájlformátum](storage-import-export-file-format-metadata-and-properties.md) meghajtó jegyzékfájl hello formátumban olvashat.  
  
hello következő példa bemutatja, hogyan toorun hello-Azure Import/Export eszköz hello **/ManifestFile** és **/CopyLogFile** paraméterek:  
  
```  
WAImportExport.exe RepairExport /r:C:\WAImportExport\9WM35C3U.rep /d:G:\ /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\WAImportExport\9WM35C3U.log /ManifestFile:G:\9WM35C3U.manifest  
```  
  
hello az alábbiakban látható egy példa a jegyzékfájlt:  
  
```xml
<?xml version="1.0" encoding="utf-8"?>  
<DriveManifest Version="2011-10-01">  
  <Drive>  
    <DriveId>9WM35C3U</DriveId>  
    <ClientCreator>Windows Azure Import/Export service</ClientCreator>  
    <BlobList>
      <Blob>  
        <BlobPath>pictures/city/redmond.jpg</BlobPath>  
        <FilePath>\pictures\city\redmond.jpg</FilePath>  
        <Length>15360</Length>  
        <PageRangeList>  
          <PageRange Offset="0" Length="3584" Hash="72FC55ED9AFDD40A0C8D5C4193208416" />  
          <PageRange Offset="3584" Length="3584" Hash="68B28A561B73D1DA769D4C24AA427DB8" />  
          <PageRange Offset="7168" Length="512" Hash="F521DF2F50C46BC5F9EA9FB787A23EED" />  
        </PageRangeList>  
        <PropertiesPath Hash="E72A22EA959566066AD89E3B49020C0A">\pictures\city\redmond.jpg.properties</PropertiesPath>  
      </Blob>  
      <Blob>  
        <BlobPath>pictures/wild/canyon.jpg</BlobPath>  
        <FilePath>\pictures\wild\canyon.jpg</FilePath>  
        <Length>10884</Length>  
        <BlockList>  
          <Block Offset="0" Length="2721" Id="AAAAAA==" Hash="263DC9C4B99C2177769C5EBE04787037" />  
          <Block Offset="2721" Length="2721" Id="AQAAAA==" Hash="0C52BAE2CC20EFEC15CC1E3045517AA6" />  
          <Block Offset="5442" Length="2721" Id="AgAAAA==" Hash="73D1CB62CB426230C34C9F57B7148F10" />  
          <Block Offset="8163" Length="2721" Id="AwAAAA==" Hash="11210E665C5F8E7E4F136D053B243E6A" />  
        </BlockList>  
        <PropertiesPath Hash="81D7F81B2C29F10D6E123D386C3A4D5A">\pictures\wild\canyon.jpg.properties</PropertiesPath>  
      </Blob> 
    </BlobList>  
 </Drive>  
</DriveManifest>  
``` 
  
Hello javítási folyamat befejeződik, miután hello eszköz olvassa végig a minden fájl hello jegyzékfájl hivatkozik, és ellenőrizze a hello MD5 kivonatok hello fájl integritását. A fenti hello jegyzék hibaállapotba kerül, a következő összetevők hello keresztül.  

```  
G:\pictures\city\redmond.jpg, offset 0, length 3584  
  
G:\pictures\city\redmond.jpg, offset 3584, length 3584  
  
G:\pictures\city\redmond.jpg, offset 7168, length 3584  
  
G:\pictures\city\redmond.jpg.properties  
  
G:\pictures\wild\canyon.jpg, offset 0, length 2721  
  
G:\pictures\wild\canyon.jpg, offset 2721, length 2721  
  
G:\pictures\wild\canyon.jpg, offset 5442, length 2721  
  
G:\pictures\wild\canyon.jpg, offset 8163, length 2721  
  
G:\pictures\wild\canyon.jpg.properties  
```

Hello ellenőrzése sikertelen összetevők közül bármelyik hello eszköz letöltődik és írni toohello ugyanazon fájl hello meghajtón.  
  
## <a name="next-steps"></a>Következő lépések
 
* [Hello Azure Import/Export eszköz beállítása](storage-import-export-tool-setup-v1.md)   
* [Merevlemezek előkészítése importálási feladatokhoz](storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [Feladatok állapotának áttekintése a másolási naplófájlok segítségével](storage-import-export-tool-reviewing-job-status-v1.md)   
* [Importálási feladat javítása](storage-import-export-tool-repairing-an-import-job-v1.md)   
* [Hibaelhárítási hello Azure Import/Export eszköz](storage-import-export-tool-troubleshooting-v1.md)
