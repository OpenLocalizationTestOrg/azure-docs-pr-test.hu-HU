---
title: "egy Azure Import/Export importálási feladat - v1 aaaRepairing |} Microsoft Docs"
description: "Ismerje meg, hogyan toorepair lett létrehozva, és futtassa az importálási feladat hello Azure Import/Export szolgáltatás."
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
ms.openlocfilehash: a9ed81f50cffd8ae6e0cb21b25a04815c2b51ee5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="repairing-an-import-job"></a>Importálási feladat javítása
Microsoft Azure Import/Export szolgáltatás hello sikertelenek lehetnek toocopy néhány, a fájlok vagy a fájl toohello Windows Azure Blob szolgáltatás részei. Néhány ok, amiért hibák a következők:  
  
-   A sérült fájlok  
  
-   Sérült meghajtók  
  
-   hello tárfiók kulcsa módosítható, amíg hello fájl átvitel során.  
  
Futtathatja Microsoft Azure Import/Export eszköz hello hello importálás feladat másolási naplófájlokat, és hello eszköz hello hiányzó fájlok (vagy egy fájl részeit) fel kell töltenie tooyour Windows Azure tárolási fiók toocomplete importálási feladat.  
  
## <a name="repairimport-parameters"></a>RepairImport paraméterek

hello következő paraméterek adható **RepairImport**: 
  
|||  
|-|-|  
|**r:**< RepairFile\>|**Szükséges.** Elérési út toohello javítási fájl, amely hello javítási hello előrehaladását követi nyomon, és lehetővé teszi a tooresume az megszakított javítását. Minden olyan meghajtó meg kell adni egy javítási fájlt. Amikor elindít egy adott meghajtó javítása, át lesz fájlban hello elérési tooa javítás még nem létezik. tooresume az megszakított javítását, meg kell adjon át egy meglévő javítási fájl hello neve. hello javítási fájl megfelelő toohello célmeghajtó mindig meg kell adni.|  
|**/ logdir:**< LogDirectory\>|**Nem kötelező.** hello naplókönyvtár. Részletes naplófájlok toothis directory lesz írva. Ha nincs naplókönyvtár meg van adva, hello aktuális könyvtárhoz hello naplókönyvtár lesz.|  
|**/ d:**< TargetDirectories\>|**Szükséges.** Legalább egy pontosvesszővel elválasztott tartalmazó könyvtárak hello importált eredeti fájlokat. hello importálási meghajtó is használható, de nincs szükség esetén érhetők el az eredeti fájlokat más helyekre.|  
|**/BK:**< BitLockerKey\>|**Nem kötelező.** Hello BitLocker kulcsot kell megadnia, ha azt szeretné, hogy hello eszköz toounlock egy titkosított meghajtó, ahol hello eredeti fájl áll rendelkezésre.|  
|**/sn:**< StorageAccountName\>|**Szükséges.** hello hello hello a tárfiók nevét importálási feladat.|  
|**/SK:**< StorageAccountKey\>|**Szükséges** csak, ha a tároló SAS nincs megadva. hello fiók kulcsának hello hello az importálási feladat.|  
|**/csas:**< ContainerSas\>|**Szükséges** csak, ha nincs megadva hello tárfiók kulcsa. hello tároló SAS hello importálási feladathoz hozzárendelt hello blobok eléréséhez.|  
|**/ CopyLogFile:**< DriveCopyLogFile\>|**Szükséges.** Elérési út toohello meghajtó másolása naplófájl (vagy a részletes naplózás, vagy a hibanapló). hello fájl hello Windows Azure Import/Export szolgáltatás által generált és tölthető le: hello hello feladattal társított blob-tároló. hello napló-fájl másolása sikertelen blobokkal vagy a fájlokat, amelyek javítani toobe információkat tartalmaz.|  
|**/ PathMapFile:**< DrivePathMapFile\>|**Nem kötelező.** Elérési út tooa szövegfájl, amely használható tooresolve kétértelműséget, ha azonos nevet, hogy a importálna hello több fájlok hello ugyanazt a feladatot. hello első alkalommal hello eszköz fut, ez a fájl összes hello nem egyértelmű nevek töltheti fel. Hello eszköz utólagosan a fájl tooresolve hello kétértelműséget fogja használni.|  
  
## <a name="using-hello-repairimport-command"></a>Hello RepairImport parancs használatával  
toorepair importálási adatok hello adatok folyamatos hello hálózaton keresztül, meg kell adnia hello importálna használatával eredeti fájlokat tartalmazó hello könyvtárak hello `/d` paraméter. Hello másolási naplófájl importáljon a tárfiókba letöltött is meg kell adnia. Egy tipikus parancssori toorepair az importálási feladat részleges kapcsolatos problémákkal néz ki:  
  
```  
WAImportExport.exe RepairImport /r:C:\WAImportExport\9WM35C2V.rep /d:C:\Users\bob\Pictures;X:\BobBackup\photos /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\WAImportExport\9WM35C2V.log  
```  
  
hello az alábbiakban látható egy példa egy másolás naplófájl. Ebben az esetben az egyik fájl 64 K adat megsérült hello importálási feladatnak teljesített hello meghajtón. Mivel ez hello csak hibát jelzett, hello részeinek hello blobok hello feladat sem sikerült importálni.  
  
```xml
<?xml version="1.0" encoding="utf-8"?>  
<DriveLog>  
 <DriveId>9WM35C2V</DriveId>  
 <Blob Status="CompletedWithErrors">  
 <BlobPath>pictures/animals/koala.jpg</BlobPath>  
 <FilePath>\animals\koala.jpg</FilePath>  
 <Length>163840</Length>  
 <ImportDisposition Status="Overwritten">overwrite</ImportDisposition>  
 <PageRangeList>  
  <PageRange Offset="65536" Length="65536" Hash="AA2585F6F6FD01C4AD4256E018240CD4" Status="Corrupted" />  
 </PageRangeList>  
 </Blob>  
 <Status>CompletedWithErrors</Status>  
</DriveLog>  
```
  
Ha ez a példány napló toohello Azure Import/Export eszköz, a hello eszköz próbálkozik toofinish hello importálási fájl hello hiányzó tartalom másolása hello hálózaton keresztül. Következő hello a fenti példában hello eszköz hello eredeti fájlt fog keresni `\animals\koala.jpg` belül két hello könyvtárak `C:\Users\bob\Pictures` és `X:\BobBackup\photos`. Ha hello fájl `C:\Users\bob\Pictures\animals\koala.jpg` létezik, hello Azure Import/Export eszköz másolatot készít az adatok toohello megfelelő blob hiányzó számos hello `http://bobmediaaccount.blob.core.windows.net/pictures/animals/koala.jpg`.  
  
## <a name="resolving-conflicts-when-using-repairimport"></a>Az ütközések feloldása RepairImport használatakor  
Bizonyos esetekben hello eszköz valószínűleg nem fogja tudni toofind vagy hello nyissa meg a szükséges fájl hello a következő okok valamelyike miatt: hello fájl nem található, hello fájl nem érhető el, hello fájl neve nem egyértelmű vagy hello hello fájl tartalma nem megfelelő.  
  
Nem egyértelmű hiba akkor fordulhat elő, ha hello eszköz próbál toolocate `\animals\koala.jpg` , és mindkettő adott nevű fájl `C:\Users\bob\pictures` és `X:\BobBackup\photos`. Ez azt jelenti, hogy mindkét `C:\Users\bob\pictures\animals\koala.jpg` és `X:\BobBackup\photos\animals\koala.jpg` hello importálási feladat meghajtón található.  
  
Hello `/PathMapFile` beállítás lehetővé teszi a tooresolve ezeket a hibákat. Megadhatja, hogy nem volt képes lesz a fájlokat, az eszköz hello hello listáját tartalmazó hello fájl hello neve toocorrectly azonosításához. hello az alábbiakban látható egy példa parancssori volna feltöltő `9WM35C2V_pathmap.txt`:  
  
```
WAImportExport.exe RepairImport /r:C:\WAImportExport\9WM35C2V.rep /d:C:\Users\bob\Pictures;X:\BobBackup\photos /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\WAImportExport\9WM35C2V.log /PathMapFile:C:\WAImportExport\9WM35C2V_pathmap.txt  
```
  
hello eszköz majd ír túl hello problematikus Fájlelérési utak`9WM35C2V_pathmap.txt`, soronként egy. Például a hello fájl tartalmazhat hello bejegyzések hello parancs futtatása után a következő:  
 
```
\animals\koala.jpg  
\animals\kangaroo.jpg  
```
  
 Hello listában lévő összes fájlhoz kell toolocate kísérlet, és nyissa meg a rendelkezésre álló toohello eszköz hello fájl tooensure. Tootell hello eszköz explicit módon, ahol módosíthatja a fájl toofind, hello elérési képezze le a fájlt és hello elérési tooeach fájl hello hozzáadása ugyanabban a sorban, karakterrel elválasztó karakterláncként lapon:  
  
```
\animals\koala.jpg           C:\Users\bob\Pictures\animals\koala.jpg  
\animals\kangaroo.jpg        X:\BobBackup\photos\animals\kangaroo.jpg  
```
  
Rendelkezésre álló toohello eszköz felhasználásának hello szükséges fájlokat, vagy frissítési hello elérési leképezés, után futtassa újra hello eszköz toocomplete hello az importálási folyamat.  
  
## <a name="next-steps"></a>Következő lépések
 
* [Hello Azure Import/Export eszköz beállítása](storage-import-export-tool-setup-v1.md)   
* [Merevlemezek előkészítése importálási feladatokhoz](storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [Feladatok állapotának áttekintése a másolási naplófájlok segítségével](storage-import-export-tool-reviewing-job-status-v1.md)   
* [Exportálási feladat javítása](storage-import-export-tool-repairing-an-export-job-v1.md)   
* [Hibaelhárítási hello Azure Import/Export eszköz](storage-import-export-tool-troubleshooting-v1.md)
