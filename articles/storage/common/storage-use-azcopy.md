---
title: "aaaCopy vagy move data tooAzure tároló a Windows AzCopy |} Microsoft Docs"
description: "Hello AzCopy használata Windows segédprogram toomove vagy másolat adatok tooor blob, table és a fájl. Adatok tooAzure tárolási átmásolhatja a helyi fájlokból, vagy adatok belül vagy tárfiókok között. Az adatok tooAzure Storage könnyen át."
services: storage
documentationcenter: 
author: seguler
manager: jahogg
editor: tysonn
ms.assetid: aa155738-7c69-4a83-94f8-b97af4461274
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/14/2017
ms.author: seguler
ms.openlocfilehash: a063a0380b7b8e6b212d0cec276f7d0f421936ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="transfer-data-with-hello-azcopy-on-windows"></a>A Windows hello AzCopy az adatátvitel
AzCopy adatok tooand másolására az egyszerű parancsokkal optimális teljesítménnyel Microsoft Azure Blob, a fájl és a Table storage egy parancssori segédprogram. Adatokat másolhat egy objektum tooanother a tárfiókon belül vagy tárfiókok között.

AzCopy, letöltheti a két verziója van. AzCopy a Windows a .NET-keretrendszer épül, és ez biztosítja a Windows stílus parancssori kapcsolókat. [AzCopy Linux](storage-use-azcopy-linux.md) épül, a .NET Core keretprogram, amelyik ajánlat POSIX-stílusú parancssori kapcsolók Linux platformon célozza. Ez a cikk a Windows AzCopy ismerteti.

## <a name="download-and-install-azcopy"></a>Töltse le és telepítse az AzCopy
### <a name="azcopy-on-windows"></a>AzCopy Windowson
Töltse le a hello [Windows AzCopy legújabb verzióját](http://aka.ms/downloadazcopy).

#### <a name="installation-on-windows"></a>Windows-telepítés
Miután telepítette a AzCopy a Windows hello installer használatával, nyisson meg egy parancsablakot, és keresse meg a toohello AzCopy telepítési könyvtárára a számítógép -, ahol hello `AzCopy.exe` végrehajtható található. Ha szükséges, hello AzCopy telepítési tooyour rendszer elérési útjának is hozzáadhat. Alapértelmezés szerint AzCopy túl van telepítve`%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy` vagy `%ProgramFiles%\Microsoft SDKs\Azure\AzCopy`.

## <a name="writing-your-first-azcopy-command"></a>Az első AzCopy parancs írása
hello alapvető AzCopy parancs szintaxisa a következő:

```azcopy
AzCopy /Source:<source> /Dest:<destination> [Options]
```

hello a következő példák bemutatják, különféle forgatókönyvekhez, amik a Microsoft Azure-Blobokkal, fájlok és táblák adatok tooand másolására. Tekintse meg a toohello [AzCopy paraméterek](#azcopy-parameters) részletes leírását az egyes mintában használt hello paraméterek szakaszban.

## <a name="blob-download"></a>BLOB: letöltése
### <a name="download-single-blob"></a>Egy blob letöltése

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:"abc.txt"
```

Vegye figyelembe, hogy ha hello mappa `C:\myfolder` nem létezik, AzCopy hoz létre, és a letöltési `abc.txt ` hello új mappába.

### <a name="download-single-blob-from-secondary-region"></a>A másodlagos régióba egyetlen blob letöltése

```azcopy
AzCopy /Source:https://myaccount-secondary.blob.core.windows.net/mynewcontainer /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt
```

Vegye figyelembe, hogy írásvédett georedundáns tárolás engedélyezve kell rendelkeznie.

### <a name="download-all-blobs"></a>Töltse le az összes BLOB

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /S
```

Hello következő feltételezik hello megadott tárolóban található blobok:  

    abc.txt
    abc1.txt
    abc2.txt
    vd1\a.txt
    vd1\abcd.txt

Hello letöltési művelet után hello directory `C:\myfolder` tartalmazza a következő fájlok hello:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\vd1\a.txt
    C:\myfolder\vd1\abcd.txt

Ha nem adja meg a beállítás `/S`, nincs BLOB letöltődnek.

### <a name="download-blobs-with-specified-prefix"></a>A megadott előtag blobok letöltése

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:a /S
```

Hello következő feltételezik hello megadott tárolóban található blobok. Hello előtaggal kezdődő összes BLOB `a` lesznek letöltve:

    abc.txt
    abc1.txt
    abc2.txt
    xyz.txt
    vd1\a.txt
    vd1\abcd.txt

Hello letöltési művelet után hello mappa `C:\myfolder` tartalmazza a következő fájlok hello:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt

hello előtag toohello virtuális könyvtár, hello hello blob neve az első részét képező vonatkozik. Hello a fenti példában hello virtuális könyvtár nem azonos hello megadott előtag, így azt nem töltődik le. Emellett, ha hello lehetőséget `\S` nincs megadva, az AzCopy nem tölti le a blobokat.

### <a name="set-hello-last-modified-time-of-exported-files-toobe-same-as-hello-source-blobs"></a>Állítsa be az exportált fájlokat toobe hello utolsó módosításának időpontja szerint forrás blobok hello

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT
```

Blobok emellett kizárja hello letöltési művelet az utolsó módosításának ideje alapján. Például, ha azt szeretné, hogy a tooexclude blobot, amelynek utolsó módosítási időpontjának hello azonos vagy újabb, mint hello célfájl, adja meg hello `/XN` lehetőséget:

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XN
```

Vagy ha azt szeretné, hogy tooexclude blobot, amelynek utolsó módosítási időpontjának hello azonos vagy régebbi, mint hello célfájl, adja hozzá a hello `/XO` lehetőséget:

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XO
```

## <a name="blob-upload"></a>BLOB: feltöltése
### <a name="upload-single-file"></a>Töltse fel egy fájlból

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:"abc.txt"
```

Ha hello megadott tároló nem létezik, AzCopy létrehozza, és feltöltések fájl hello bele.

### <a name="upload-single-file-toovirtual-directory"></a>Egy fájlból toovirtual directory feltöltése

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer/vd /DestKey:key /Pattern:abc.txt
```

Ha hello megadott virtuális könyvtár nem létezik, AzCopy feltölt hello tooinclude hello virtuális könyvtárának nevében (*pl.*, `vd/abc.txt` a fenti példában hello).

### <a name="upload-all-files"></a>Minden fájl feltöltése

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /S
```

Beállítás megadása `/S` hello feltöltések hello tartalmát a megadott könyvtár tooBlob tárolási rekurzív módon, ami azt jelenti, hogy minden almappa és a fájljaikat, valamint feltöltése. Például azt feltételezik hello következő mappában található fájlok `C:\myfolder`:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

Hello feltöltési művelet után hello tároló tartalmazza a következő fájlok hello:

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

Ha nem adja meg a beállítás `/S`, AzCopy töltse fel a rekurzív módon. Hello feltöltési művelet után hello tároló tartalmazza a következő fájlok hello:

    abc.txt
    abc1.txt
    abc2.txt

### <a name="upload-files-matching-specified-pattern"></a>Töltse fel a megadott mintának megfelelő fájlok

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:a* /S
```

Hello következő feltételezik mappában található fájlok `C:\myfolder`:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\xyz.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

Hello feltöltési művelet után hello tároló tartalmazza a következő fájlok hello:

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

Ha nem adja meg a beállítás `/S`, AzCopy csak feltölti a blobok, amelyek nem találhatók a virtuális könyvtárban:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt

### <a name="specify-hello-mime-content-type-of-a-destination-blob"></a>Adjon meg egy cél BLOB hello MIME tartalomtípus
Alapértelmezés szerint AzCopy beállítja hello tartalomtípusa cél blob túl`application/octet-stream`. 3.1.0 verziójával kezdve explicit módon megadhatja hello tartalomtípus keresztül hello beállítás `/SetContentType:[content-type]`. Ez a szintaxis hello content-type összes BLOB a feltöltési művelet állítja be.

```azcopy
AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType:video/mp4
```

Ha megad `/SetContentType` , érték nélküli AzCopy állítja be, minden egyes blob vagy a fájl tartalomtípusa szerint tooits fájl kiterjesztése.

```azcopy
AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType
```

## <a name="blob-copy"></a>BLOB: másolása
### <a name="copy-single-blob-within-storage-account"></a>Másolja át egy blob Storage-fiókban

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceKey:key /DestKey:key /Pattern:abc.txt
```

Ha egy blobba egy tárfiókon belül másol egy [kiszolgálóoldali másolatot](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) művelet.

### <a name="copy-single-blob-across-storage-accounts"></a>Másolja át egy blob Storage-fiókok között

```azcopy
AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt
```

A blob Storage-fiókok között másolásakor egy [kiszolgálóoldali másolatot](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) művelet.

### <a name="copy-single-blob-from-secondary-region-tooprimary-region"></a>Egy blob másolása másodlagos régióba tooprimary régió

```azcopy
AzCopy /Source:https://myaccount1-secondary.blob.core.windows.net/mynewcontainer1 /Dest:https://myaccount2.blob.core.windows.net/mynewcontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt
```

Vegye figyelembe, hogy írásvédett georedundáns tárolás engedélyezve kell rendelkeznie.

### <a name="copy-single-blob-and-its-snapshots-across-storage-accounts"></a>Egy blob másolási és a pillanatképek tárfiókok között

```azcopy
    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt /Snapshot
```

Hello másolási művelet után a hello céltároló hello blob és a pillanatképek tartalmazza. Ha a fenti példában hello hello blob két pillanatképekkel rendelkezik, hello tároló hello következőket tartalmazza blob és a pillanatképeket:

    abc.txt
    abc (2013-02-25 080757).txt
    abc (2014-02-21 150331).txt

### <a name="synchronously-copy-blobs-across-storage-accounts"></a>Szinkron módon másolni BLOB Storage-fiókok
AzCopy alapértelmezés szerint aszinkron módon másolja az adatokat két tárolási végpontok közötti. Ezért hello másolási művelet futtatása hello háttérben tartalékolt sávszélesség-kapacitása, amelynek nincs szolgáltatásiszint-szerződés szempontjából hogyan gyors blob használatával másolja, és AzCopy rendszeres időközönként ellenőrzi hello példány állapotát, amíg nem fejeződött be, vagy hello másolása nem sikerült.

Hello `/SyncCopy` lehetőség biztosítja, hogy hello másolási művelet lekérdezi az egységes sebesség. AzCopy elkészíti hello szinkron hello blobok letöltésével toocopy hello a megadott forrás toolocal memória, és majd feltöltheti toohello Blob célhelyet.

```azcopy
AzCopy /Source:https://myaccount1.blob.core.windows.net/myContainer/ /Dest:https://myaccount2.blob.core.windows.net/myContainer/ /SourceKey:key1 /DestKey:key2 /Pattern:ab /SyncCopy
```

`/SyncCopy`hozhat létre további költségeket összehasonlított tooasynchronous másolási hello kilépő ajánlott megközelítést alkalmazva van toouse ezt a beállítást egy Azure virtuális gép, amely hello és a forrás tárolási fiók tooavoid kilépő költségű ugyanabban a régióban.

## <a name="file-download"></a>Fájl: letöltése
### <a name="download-single-file"></a>Töltse le egy fájlból

```azcopy
AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/myfolder1/ /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt
```

Hello adott forrása az Azure fájlmegosztások, akkor meg kell adnia hello pontosan a fájl nevét, ha (*pl.* `abc.txt`) toodownload egyetlen fájl, vagy adja meg a beállítás `/S` összes toodownload hello megosztáson található fájlok rekurzív módon. Kísérlet toospecify egy fájl és egyaránt lehetőség `/S` hiba együtt eredményez.

### <a name="download-all-files"></a>Minden fájl letöltése

```azcopy
AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/ /Dest:C:\myfolder /SourceKey:key /S
```

Vegye figyelembe, hogy a rendszer nem tölti le üres mappák.

## <a name="file-upload"></a>Fájl: feltöltése
### <a name="upload-single-file"></a>Töltse fel egy fájlból

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /Pattern:abc.txt
```

### <a name="upload-all-files"></a>Minden fájl feltöltése

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /S
```

Vegye figyelembe, hogy az üres mappák nem töltődött fel.

### <a name="upload-files-matching-specified-pattern"></a>Töltse fel a megadott mintának megfelelő fájlok

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /Pattern:ab* /S
```

## <a name="file-copy"></a>Fájl: másolása
### <a name="copy-across-file-shares"></a>Fájlmegosztások másolni

```azcopy
AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S
```
Amikor fájlmegosztások között másolhat egy fájlt egy [kiszolgálóoldali másolatot](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) művelet.

### <a name="copy-from-file-share-tooblob"></a>Megosztás tooblob fájl másolása

```azcopy
AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare/ /Dest:https://myaccount2.blob.core.windows.net/mycontainer/ /SourceKey:key1 /DestKey:key2 /S
```
Ha fájlt átmásolni fájl megosztási tooblob, egy [kiszolgálóoldali másolatot](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) művelet.


### <a name="copy-from-blob-toofile-share"></a>A blob toofile megosztásból másolása

```azcopy
AzCopy /Source:https://myaccount1.blob.core.windows.net/mycontainer/ /Dest:https://myaccount2.file.core.windows.net/myfileshare/ /SourceKey:key1 /DestKey:key2 /S
```
Amikor blob toofile megosztásból másolhat egy fájlt egy [kiszolgálóoldali másolatot](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) művelet.

### <a name="synchronously-copy-files"></a>Szinkron módon történik a fájlok másolása
Megadhatja a hello `/SyncCopy` toocopy adatait a File Storage tooFile tároló, a File Storage tooBlob tárolási lehetőséget, és a tárolási Blob Storage tooFile szinkron módon történik, AzCopy ennek hello forrás adatok toolocal memória töltése és töltse fel az újra toodestination. Standard kilépő költség érvényes.

```azcopy
AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S /SyncCopy
```

Amikor a File Storage tooBlob tárolási másol, hello alapértelmezett blob típushoz blokkblob, felhasználói beállítást adja meg `/BlobType:page` toochange hello céltípus blob.

Vegye figyelembe, hogy `/SyncCopy` összehasonlító tooasynchronous másolási költség további kilépő hozhat létre, hello ajánlott megoldás, ezt a beállítást, az Azure virtuális Gépen, amely hello hello toouse és a forrás tárolási fiók tooavoid kilépő költségű ugyanabban a régióban.

## <a name="table-export"></a>Tábla: exportálása
### <a name="export-table"></a>Tábla exportálása

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key
```

AzCopy ír a jegyzékfájl toohello megadott célmappát. hello jegyzékfájl hello importálási folyamat toolocate hello szükséges adatok fájlokat használnak, és az adatellenőrzése. hello jegyzékfájl hello elnevezési alapértelmezés szerint a következő használja:

    <account name>_<table name>_<timestamp>.manifest

Felhasználói is megadhat hello beállítás `/Manifest:<manifest file name>` tooset hello jegyzékfájl neve.

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /Manifest:abc.manifest
```

### <a name="split-export-into-multiple-files"></a>Több fájlra vegyes exportálása

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/mytable/ /Dest:C:\myfolder /SourceKey:key /S /SplitSize:100
```

AzCopy használ egy *kötet index* adatok hello a fájlnevek toodistinguish több fájlt. hello kötet index két részből áll egy *partíciós kulcs tartományindexszel* és egy *vegyes fájl index*. Mindkét indexek nulla alapú.

hello partíciós kulcs tartományindexszel értéke 0, ha hello felhasználó nem adja meg a beállítás `/PKRS`.

Tegyük fel például, AzCopy két adatfájlok állít elő, miután hello felhasználó határozza meg a beállítás `/SplitSize`. adatok fájlnevek eredő hello lehet:

    myaccount_mytable_20140903T051850.8128447Z_0_0_C3040FE8.json
    myaccount_mytable_20140903T051850.8128447Z_0_1_0AB9AC20.json

Vegye figyelembe, hogy hello lehetséges minimális értékének a beállításhoz tartozó `/SplitSize` 32 MB-nál. Ha a megadott hello célobjektuma Blob-tároló, AzCopy hello adatfájl felosztja a után annak mérete eléri a hello blob mérete korlátozás (200 GB-os), függetlenül attól, hogy lehetőséget `/SplitSize` hello felhasználó által meghatározott.

### <a name="export-table-toojson-or-csv-data-file-format"></a>Tábla tooJSON vagy CSV-fájlformátumot adatok exportálása
Alapértelmezés szerint AzCopy táblák tooJSON adatfájlok exportálja. Hello beállítást adhat meg `/PayloadFormat:JSON|CSV` tooexport hello táblák JSON vagy a fürt megosztott kötetei szolgáltatás.

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PayloadFormat:CSV
```

Hello CSV az adattartalom formátuma megadásakor AzCopy is, ami egy séma kiterjesztésű fájlt `.schema.csv` minden olyan adatfájl esetében.

### <a name="export-table-entities-concurrently"></a>Táblaentitásokat egyidejűleg exportálása

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PKRS:"aa#bb"
```

AzCopy párhuzamos műveletek tooexport entitások kezdődik, amikor a felhasználó hello határozza meg a beállítás `/PKRS`. Egyes műveletek egy kulcs partíciótartomány exportálja.

Vegye figyelembe, hogy hello száma párhuzamos műveletek is beállítás által vezérelt `/NC`. AzCopy Processzormagok száma hello használja, mint a hello alapértelmezett értékének `/NC` táblaentitásokat, másolásakor akkor is, ha `/NC` nincs megadva. Ha a hello felhasználói beállítás megad `/PKRS`, AzCopy hello kisebb hello két értékek - partíció kulcstartományokkal és implicit vagy explicit módon megadott párhuzamos műveletek - toodetermine hello száma párhuzamos műveletek toostart használja. További tudnivalókért írja be a `AzCopy /?:NC` hello parancssorból.

### <a name="export-table-tooblob"></a>Exportálási tábla tooblob

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:https://myaccount.blob.core.windows.net/mycontainer/ /SourceKey:key1 /Destkey:key2
```

AzCopy JSON adatfájlt a következő elnevezési konvenció hello blob tárolóba állít elő:

    <account name>_<table name>_<timestamp>_<volume index>_<CRC>.json

hello létrehozott JSON-adatfájlt a következő hello az adattartalom formátuma a szükséges metaadatokat. Ez az adattartalom formátuma a részletekért lásd: [az adattartalom formátuma Table szolgáltatási műveletek](http://msdn.microsoft.com/library/azure/dn535600.aspx).

Ne feledje, hogy táblák tooblobs exportálásakor AzCopy hello tábla entitások toolocal ideiglenes fájlokat tölti le majd feltölti az adott entitások toohello blob. Ezek ideiglenes fájlokat kerüljenek mappába hello napló fájl hello alapértelmezett elérési úttal rendelkező "<code>%LocalAppData%\Microsoft\Azure\AzCopy</code>", lehetőséget adhat meg/Z: [napló-fájlok és mappák] toochange hello napló fájlok mappáját, és így a hello ideiglenes fájlok helyének módosításához. hello ideiglenes fájlok mérete, amelyekről a táblaentitásokat és hello mérete a megadott hello beállítás /SplitSize, bár ideiglenes adatfájlját hello helyi lemez törlése azonnal azt követően adatok feltöltése toohello blob, győződjön meg arról, hogy elég helyi lemezre terület toostore ezek ideiglenes fájlokat törlés előtt.

## <a name="table-import"></a>Tábla: importálása
### <a name="import-table"></a>Tábla importálása

```azcopy
AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.table.core.windows.net/mytable1/ /DestKey:key /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:InsertOrReplace
```

a beállítás hello `/EntityOperation` azt jelzi, hogyan tooinsert entitások be hello tábla. Lehetséges értékek:

* `InsertOrSkip`: A meglévő entitás kihagyja vagy szúr be egy új entitást, ha hello tábla nem létezik.
* `InsertOrMerge`: Egyesíti a meglévő entitás vagy szúr be egy új entitást, ha hello tábla nem létezik.
* `InsertOrReplace`: A felváltja meglévő entitás vagy szúr be egy új entitást, ha hello tábla nem létezik.

Vegye figyelembe, hogy a beállítás nem adható meg `/PKRS` hello importálás esetén. Hello exportálási forgatókönyv, amelyben a kapcsolót kell megadnia eltérően `/PKRS` toostart párhuzamos műveletek, AzCopy párhuzamos műveletek alapértelmezés szerint elindul egy tábla importálásakor. hello alapértelmezett száma párhuzamos műveletek indítása processzormagokkal; egyenlő toohello száma azonban megadhat egyidejű lehetőséggel különböző számú `/NC`. További tudnivalókért írja be a `AzCopy /?:NC` hello parancssorból.

Ügyeljen arra, hogy AzCopy csak támogatja a JSON, CSV nem importálása. AzCopy nem támogatja a felhasználó által létrehozott JSON tábla származó és fájlok manifest. Az AzCopy tábla exportálása mindkét fájl kell származnia. tooavoid hibák, ne módosítsa hello exportált JSON vagy jegyzékfájlt.

### <a name="import-entities-tootable-using-blobs"></a>Importálás entitások tootable blobs használata
Tegyük fel, a Blob-tároló tartalmazza hello alábbi: az Azure-tábla és a hozzá tartozó jegyzékfájl jelölő A JSON-fájlt.

    myaccount_mytable_20140103T112020.manifest
    myaccount_mytable_20140103T112020_0_0_0AF395F1DC42E952.json

Futtathatja a következő parancs tooimport entitások egy táblába a blob tárolóban hello jegyzékfájl hello:

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:https://myaccount.table.core.windows.net/mytable /SourceKey:key1 /DestKey:key2 /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:"InsertOrReplace"
```

## <a name="other-azcopy-features"></a>Más AzCopy szolgáltatások
### <a name="only-copy-data-that-doesnt-exist-in-hello-destination"></a>Csak másolja az adatokat, amely a célként megadott hello nem létezik
Hello `/XO` és `/XN` paraméterek tooexclude régebbi vagy újabb forrás erőforrások másolását, illetve lehetővé teszik. Ha csak toocopy forrás erőforrásokat, amelyek nem léteznek a hello cél, mindkét paraméter hello AzCopy parancs adhat meg:

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /XO /XN

    /Source:C:\myfolder /Dest:http://myaccount.file.core.windows.net/myfileshare /DestKey:<destkey> /S /XO /XN

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:http://myaccount.blob.core.windows.net/mycontainer1 /SourceKey:<sourcekey> /DestKey:<destkey> /S /XO /XN

Vegye figyelembe, hogy ez nem támogatott esetén hello cél- és egy tábla.

### <a name="use-a-response-file-toospecify-command-line-parameters"></a>A válasz fájl toospecify parancssori paraméterekkel

```azcopy
AzCopy /@:"C:\responsefiles\copyoperation.txt"
```

AzCopy parancssori paramétereket is megadhat a egy válaszfájlt. AzCopy folyamatok hello hello fájlban paramétereket, mintha hello parancssorban közvetlen helyettesítés hello tartalma hello fájl végrehajtása meg lett.

Egy válaszfájl nevű feltételezik `copyoperation.txt`, amely tartalmazza a következő sorokat hello. Minden egyes AzCopy paraméter adható meg egy sorba

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y

vagy külön sorok:

    /Source:http://myaccount.blob.core.windows.net/mycontainer
    /Dest:C:\myfolder
    /SourceKey:<sourcekey>
    /S
    /Y

AzCopy sikertelen lesz, ha hello paraméter osztani két sort, ahogy az itt látható a hello `/sourcekey` paraméter:

    http://myaccount.blob.core.windows.net/mycontainer
     C:\myfolder
    /sourcekey:
    <sourcekey>
    /S
    /Y

### <a name="use-multiple-response-files-toospecify-command-line-parameters"></a>Több válasz fájlok toospecify parancssori paraméterekkel
Egy válaszfájl nevű feltételezik `source.txt` , amely megadja, hogy a forrás-tároló:

    /Source:http://myaccount.blob.core.windows.net/mycontainer

És egy válaszfájlt nevű `dest.txt` hello fájlrendszer, amely egy célmappát határozza meg:

    /Dest:C:\myfolder

És egy válaszfájlt nevű `options.txt` , amely megadja, hogy az AzCopy beállítások:

    /S /Y

AzCopy toocall a válasz fájlok, amelyek találhatók a könyvtárban található `C:\responsefiles`, használja ezt a parancsot:

```azcopy
AzCopy /@:"C:\responsefiles\source.txt" /@:"C:\responsefiles\dest.txt" /SourceKey:<sourcekey> /@:"C:\responsefiles\options.txt"   
```

AzCopy végrehajtja ezt a parancsot, ha minden egyes paraméterek hello hello parancssorban szerepeltetett volna:

```azcopy
AzCopy /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y
```

### <a name="specify-a-shared-access-signature-sas"></a>Adja meg a közös hozzáférésű jogosultságkód (SAS)

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceSAS:SAS1 /DestSAS:SAS2 /Pattern:abc.txt
```

Azt is megadhatja a SAS hello tárolóra URI:

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1/?SourceSASToken /Dest:C:\myfolder /S
```

### <a name="journal-file-folder"></a>Napló fájlmappa
Minden alkalommal, amikor egy parancs tooAzCopy kiadása ellenőrzi, hogy a napló fájl megtalálható-e hello alapértelmezett mappát, vagy hogy keresztül ez a beállítás a megadott mappa létezik. Hello napló fájl nem létezik egyik helyen sem, ha AzCopy hello művelet új kezeli, és létrehoz egy új naplófájl.

Hello napló fájl létezik, az AzCopy ellenőrzi, hogy megadott hello parancssori megegyezik-e parancssori hello hello napló fájlban. Hello két parancssorokat felel meg, ha az AzCopy hello hiányos művelet folytatása. Ha nem egyeznek, rákérdezéses tooeither felülírása hello napló toostart új művelet, vagy toocancel hello aktuális művelete is.

Ha azt szeretné, hogy toouse hello hello napló fájl alapértelmezett helye:

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z
```

Ha a beállítás nincs megadva `/Z`, vagy adja meg a beállítás `/Z` nélkül hello mappa elérési útja, a fent látható AzCopy fájlt hoz létre hello napló hello alapértelmezett helyen, amely `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`. Ha hello napló fájl már létezik, majd AzCopy folytatja hello művelet hello napló fájl alapján.

Ha azt szeretné, hogy egy egyéni helyet hello napló fájl toospecify:

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z:C:\journalfolder\
```

Ez a példa hello napló fájlt hoz létre, ha még nem létezik. Ha létezik, majd AzCopy folytatja hello művelet hello napló fájl alapján.

Ha azt szeretné, hogy tooresume AzCopy művelet:

```azcopy
AzCopy /Z:C:\journalfolder\
```

Ez a példa hello utolsó művelet, amely talán nem sikerült toocomplete folytatja.

### <a name="generate-a-log-file"></a>A naplófájl létrehozása

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V
```

Ha a beállítás megadja `/V` anélkül, hogy a fájl elérési útja toohello részletes naplózás, majd AzCopy hello naplófájl hello alapértelmezett a helyen hozza létre, amely `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`.

Ellenkező esetben egy naplófájlt is létrehozhat egy egyéni helyen:

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V:C:\myfolder\azcopy1.log
```

Vegye figyelembe, hogy ha a beállítás a következő relatív elérési utat ad meg `/V`, például a `/V:test/azcopy1.log`, majd hello részletes napló jön létre az aktuális munkakönyvtárban hello belül egy almappát `test`.

### <a name="specify-hello-number-of-concurrent-operations-toostart"></a>Adja meg a hello száma párhuzamos műveletek toostart
A beállítás `/NC` hello egyidejű másolási műveletek számát adja meg. Alapértelmezés szerint az AzCopy elindul egy bizonyos száma párhuzamos műveletek átviteli tooincrease hello adatátvitelt. A tábla műveleteket hello száma párhuzamos műveletek rendelkezik processzorok száma egyenlő toohello. A Blob és a fájl műveleteket, hello száma párhuzamos műveletek egyenlő 8 alkalommal hello számú processzorral rendelkezik. Ha AzCopy kis sávszélességű hálózaton keresztül futtatja, megadhatja a /NC tooavoid hibáját okozta. erőforrás verseny kevesebb.

### <a name="run-azcopy-against-azure-storage-emulator"></a>AzCopy futtassa az Azure Storage emulatorban
AzCopy is futtathatók hello [Azure Storage Emulator](storage-use-emulator.md) a Blobok:

```azcopy
AzCopy /Source:https://127.0.0.1:10000/myaccount/mycontainer/ /Dest:C:\myfolder /SourceKey:key /SourceType:Blob /S
```

és táblák:

```azcopy
AzCopy /Source:https://127.0.0.1:10002/myaccount/mytable/ /Dest:C:\myfolder /SourceKey:key /SourceType:Table
```

## <a name="azcopy-parameters"></a>AzCopy paraméterek
AzCopy paramétereinek az alábbiakban található. Egy hello parancsok parancssorból hello AzCopy használatával kapcsolatos segítséget a következő beírásával:

* AzCopy részletes parancssori segítséget:`AzCopy /?`
* További információt az AzCopy paramétert:`AzCopy /?:SourceKey`
* A parancssori példák:`AzCopy /?:Samples`

### <a name="sourcesource"></a>/ Source: "forrás"
Határozza meg, melyik toocopy hello forrásfájlok adatait. hello forrás lehet egy fájl rendszer könyvtár, egy blob-tároló, blob virtuális könyvtár, egy tárolófájl-megosztás, tároló fájl könyvtár, vagy egy Azure-tábla.

**Alkalmazandó:** blobokat, fájlok, táblák

### <a name="destdestination"></a>/ Cél: "cél"
Megadja a hello cél toocopy számára. hello cél lehet egy fájl rendszer könyvtár, egy blob-tároló, blob virtuális könyvtár, egy tárolófájl-megosztás, tároló fájl könyvtár, vagy egy Azure-tábla.

**Alkalmazandó:** blobokat, fájlok, táblák

### <a name="patternfile-pattern"></a>/ Mintát: "fájl-minta"
Adja meg a fájl mintát, amely azt jelzi, mely fájlok toocopy. hello /Pattern paraméter hello viselkedését hello forrásadatok hello helyét, és hello jelenléte hello rekurzív mód beállítása határozza meg. Rekurzív mód keresztül /s beállítás van megadva.

Ha hello megadott forrás könyvtár hello fájlrendszer, akkor szokásos helyettesítő karakterek érvényesek, és hello fájl minta biztosította a rendszer megkeres hello könyvtárban lévő fájlokra vonatkozóan. Ha beállítás /s kapcsoló meg van adva, majd AzCopy is megegyezik a megadott minta hello hello könyvtár alatt almappákban található összes fájl ellen.

Ha hello adott forrásból származnak. a blob-tároló vagy a virtuális könyvtár, majd helyettesítő karakterek nem érvényesek. Ha a beállítás /s kapcsoló meg van adva, majd AzCopy hello megadott fájlminta blob előtagjaként értelmezi. Ha a beállítás /s kapcsoló nincs megadva, majd AzCopy megegyezik hello fájl mintával pontos blob nevekkel.

Ha hello adott forrása az Azure fájlmegosztások, akkor meg kell adnia hello pontosan a fájl nevét (pl. abc.txt) toocopy egyetlen fájlban, vagy adja meg a beállítás /S toocopy minden fájl hello megosztás rekurzív módon. Kísérlet történt egy toospecify mindkét egy fájl mintát és beállítás /S együtt hibát eredményez.

AzCopy használja a kis-és nagybetűket megfelelő, ha hello/Source a blob-tároló vagy a blob virtuális könyvtár, és használja, azonban nem minden egyező hello más esetekben.

hello alapértelmezett fájl használható, ha nincs fájl minta van megadva a mintája, *.* egy olyan fájlhelyre rendszer vagy egy Azure Storage helyének egy üres előtag. Több fájl minták megadása nem támogatott.

**Alkalmazandó:** Blobok, fájlok

### <a name="destkeystorage-key"></a>/ DestKey: "storage-kulcsot"
Megadja a hello tárfiók kulcsa hello cél erőforrás.

**Alkalmazandó:** blobokat, fájlok, táblák

### <a name="destsassas-token"></a>/ DestSAS: "sas-token"
Adja meg a közös hozzáférésű Jogosultságkód (SAS) hello cél OLVASÁSI és írási engedélyekkel (ha van ilyen). Az idézőjelek közé foglalt, SAS hello között legyen, tartalmaz, így előfordulhat, hogy speciális parancssori karaktereket.

Ha hello cél erőforrás a blob-tároló, a fájlmegosztás vagy a táblát, vagy adja meg ezt a beállítást hello SAS-jogkivonat követ, vagy hello SAS hello cél blob tároló, a fájlmegosztás vagy a tábla URI, anélkül, hogy ez a beállítás részeként is megadhat.

Ha hello forrás és cél mindkét blobokat, akkor hello cél blob kell lennie, hello belül ugyanaz, mint hello forrás blob storage-fiók.

**Alkalmazandó:** blobokat, fájlok, táblák

### <a name="sourcekeystorage-key"></a>/ SourceKey: "storage-kulcsot"
Megadja a hello tárfiók kulcsa hello forrás erőforrás.

**Alkalmazandó:** blobokat, fájlok, táblák

### <a name="sourcesassas-token"></a>/ SourceSAS: "sas-token"
Megadja egy közös hozzáférésű Jogosultságkód és lista engedélyt hello forrás (ha van ilyen). Az idézőjelek közé foglalt, SAS hello között legyen, tartalmaz, így előfordulhat, hogy speciális parancssori karaktereket.

Ha hello forráserőforrásnak egy blob-tároló, és nem egy kulcs, és SAS-kód nem áll, majd hello blob tároló írásvédett keresztül névtelen hozzáférés.

Ha hello forrás egy fájlmegosztás vagy a tábla, meg kell adni egy kulcs- vagy SAS-kód.

**Alkalmazandó:** blobokat, fájlok, táblák

### <a name="s"></a>/ S
Meghatározza a másolási műveletek rekurzív módját. Rekurzív módban AzCopy másolja át a blobokkal vagy a fájlokat, amelyek megfelelnek a megadott fájl hello mintát, almappák lévőket is beleértve.

**Alkalmazandó:** Blobok, fájlok

### <a name="blobtypeblock--page--append"></a>/ BlobType: "block" |} "lap" |} "append"
Megadja, hogy hello cél blob egy blokkblob, oldalakra vonatkozó blob vagy hozzáfűző blob. Ez a beállítás csak egy blob feltölteni kívánt esetén alkalmazható. Ellenkező esetben hiba történik. Ha hello cél blob, és ez a beállítás nincs megadva, alapértelmezés szerint az AzCopy egy blokkblob hoz létre.

**Alkalmazandó:** Blobok

### <a name="checkmd5"></a>/ CheckMD5
Kiszámítja az MD5 kivonatoló letöltött adatok, és ellenőrzi, hogy hello MD5 kivonatoló hello blob tárolja, vagy a fájl Content-MD5 tulajdonsága egyezést mutat a hello kiszámítása kivonatát. hello MD5 ellenőrzése ki van kapcsolva alapértelmezés szerint, az adatok letöltése során meg kell adnia ezt a beállítást tooperform hello MD5 az ellenőrzést.

Vegye figyelembe, hogy Azure Storage nem garantálja, hogy hello MD5 kivonatoló hello BLOB tárolja, vagy a fájl naprakész. Amikor hello blob vagy a fájl módosítása ügyfél felelősségi tooupdate hello MD5.

AzCopy mindig hello Content-MD5 tulajdonság beállítása az Azure blob vagy a fájl után ismét feltölteni a toohello szolgáltatás.  

**Alkalmazandó:** Blobok, fájlok

### <a name="snapshot"></a>Vagy pillanatkép
Azt jelzi, hogy tootransfer pillanatképeket. Ez a beállítás csak akkor érvényes, ha egy blob hello forrás.

hello átvitt blob pillanatképek átnevezi a következő formátumban: .extension blob-név (pillanatkép-time)

Alapértelmezés szerint a pillanatképek nem kerülnek.

**Alkalmazandó:** Blobok

### <a name="vverbose-log-file"></a>/ V: [részletes-naplófájl]
Kimeneti részletes üzenetek fájlba.

Alapértelmezés szerint hello részletes naplófájl neve a AzCopyVerbose.log `%LocalAppData%\Microsoft\Azure\AzCopy`. Ha megadja ezt a lehetőséget egy meglévő fájl helyét, a hello részletes napló egy hozzáfűzött toothat fájl.  

**Alkalmazandó:** blobokat, fájlok, táblák

### <a name="zjournal-file-folder"></a>/ Z: [napló-fájlok és mappák]
A művelet folytatása napló fájl mappáját adja meg.

AzCopy mindig támogatja a Folytatás, ha egy művelet megszakadt.

Ha ez a beállítás nincs megadva, vagy egy mappa elérési útját nélkül van megadva, majd AzCopy hello alapértelmezett helye a % LocalAppData%\Microsoft\Azure\AzCopy hello napló fájlt hoz létre.

Minden alkalommal, amikor egy parancs tooAzCopy kiadása ellenőrzi, hogy a napló fájl megtalálható-e hello alapértelmezett mappát, vagy hogy keresztül ez a beállítás a megadott mappa létezik. Hello napló fájl nem létezik egyik helyen sem, ha AzCopy hello művelet új kezeli, és létrehoz egy új naplófájl.

Hello napló fájl létezik, az AzCopy ellenőrzi, hogy megadott hello parancssori megegyezik-e parancssori hello hello napló fájlban. Hello két parancssorokat felel meg, ha az AzCopy hello hiányos művelet folytatása. Ha nem egyeznek, rákérdezéses tooeither felülírása hello napló toostart új művelet, vagy toocancel hello aktuális művelete is.

hello napló fájl hello művelet sikeres befejezését követően törlődik.

Vegye figyelembe, hogy a Folytatás, az AzCopy egy korábbi verziójával létrehozott napló fájlból egy művelet nem támogatott.

**Alkalmazandó:** blobokat, fájlok, táblák

### <a name="parameter-file"></a>/@:"Parameter-File"
Megadja a paramétereket tartalmazó fájlt. AzCopy folyamatok hello hello fájlban paramétereket, mintha csak hello parancssorban megadott lett.

A válaszfájlban több paramétert meg egyetlen vonal, vagy adja meg az egyes paramétereket külön sorban tüntettük. Vegye figyelembe, hogy az egyes paraméter nem terjedhetnek többsoros.

Válasz tartalmazhatnak hello # karakterrel kezdődő sorok megjegyzések.

Válasz több fájl is megadható. Vegye figyelembe azonban, hogy az AzCopy nem támogatja a beágyazott válaszfájlok.

**Alkalmazandó:** blobokat, fájlok, táblák

### <a name="y"></a>/Y
Letiltja az összes AzCopy megerősítést kér.

**Alkalmazandó:** blobokat, fájlok, táblák

### <a name="l"></a>/ L
Megadja a listázási művelet csak; adatot nem másolódik.

AzCopy értelmezi hello segítségével a ezt a beállítást a szimuláció futó hello parancssor ez nincs lehetőség/l és számát, hogy hány objektumok másolását követően is megadhat /V beállítást: hello azonos időben toocheck mely objektumok hello részletes naplóba kerülnek.

Ez a beállítás viselkedése hello is határozza meg hello forrásadatok hello helye és hello rekurzív mód beállítás/s és a fájl minta beállítás /Pattern hello jelenlétét.

AzCopy, ez a hely LISTÁJÁT, és OLVASÁSI engedélyre van szüksége, ez a beállítás használata esetén.

**Alkalmazandó:** Blobok, fájlok

### <a name="mt"></a>/MT
Beállítja a hello letöltött fájl utolsó módosításának ideje toobe hello azonos hello forrás blob vagy fájl.

**Alkalmazandó:** Blobok, fájlok

### <a name="xn"></a>/XN
Nem tartalmazza egy újabb forráserőforrásnak. hello erőforrás nem másolódik esetén hello hello forrás utolsó módosítási időpontjának hello azonos vagy újabb, mint a cél.

**Alkalmazandó:** Blobok, fájlok

### <a name="xo"></a>/XO
Nem tartalmazza egy régebbi forráserőforrásnak. hello erőforrás nem másolódik esetén hello hello forrás utolsó módosítási időpontjának hello azonos vagy régebbi, mint a cél.

**Alkalmazandó:** Blobok, fájlok

### <a name="a"></a>/A
Csak a hello Archiválandó fájlok feltöltését.

**Alkalmazandó:** Blobok, fájlok

### <a name="iarashcnetoi"></a>/ IA: [RASHCNETOI]
Feltöltések csak a fájlok hello bármelyike megadott attribútumok beállítása.

A rendelkezésre álló attribútumok a következők:

* R = csak olvasható fájlokat
* A = archiválásra kész
* S rendszerfájlok =
* H = Rejtett fájlok
* C = tömörített fájlok
* N = normál fájlok
* E titkosított fájlok =
* T ideiglenes fájlok =
* O = Offline fájlok
* I = nem indexelt fájlok

**Alkalmazandó:** Blobok, fájlok

### <a name="xarashcnetoi"></a>/ XA: [RASHCNETOI]
Nem tartalmazza a fájlra, amely bármelyik megadott hello attribútumok beállítása.

A rendelkezésre álló attribútumok a következők:

* R = csak olvasható fájlokat
* A = archiválásra kész
* S rendszerfájlok =
* H = Rejtett fájlok
* C = tömörített fájlok
* N = normál fájlok
* E titkosított fájlok =
* T ideiglenes fájlok =
* O = Offline fájlok
* I = nem indexelt fájlok

**Alkalmazandó:** Blobok, fájlok

### <a name="delimiterdelimiter"></a>/ Elválasztó karakter: "elválasztót".
Azt jelzi, hello elválasztó karakter egy blob neve toodelimit virtuális könyvtárak használni.

Alapértelmezés szerint az AzCopy által használt / hello elválasztó karakterként. Azonban AzCopy támogatja a közös karakter használatát (például a @, #, vagy %) elválasztó. Ha egy, a következő különleges karakterek hello parancssorban tooinclude van szüksége, tegye a hello fájlnevet idézőjelekkel együtt.

Ez a beállítás csak akkor alkalmazható blobok letöltése.

**Alkalmazandó:** Blobok

### <a name="ncnumber-of-concurrent-operations"></a>/ NC: "számot-az-egyidejű-műveletek"
Hello párhuzamos műveletek számát adja meg.

AzCopy alapértelmezés szerint elindul egy bizonyos száma párhuzamos műveletek átviteli tooincrease hello adatátvitelt. Ne feledje, hogy nagy száma párhuzamos műveletek alacsony sávszélességű környezetben előfordulhat, hogy ne terhelje tovább hello hálózati kapcsolat teljes befejezését hello műveletek tiltása. Párhuzamos műveletek alapján tényleges rendelkezésre álló hálózati sávszélesség szabályozását.

hello felső párhuzamos műveletek határa 512.

**Alkalmazandó:** blobokat, fájlok, táblák

### <a name="sourcetypeblob--table"></a>/ Forrástípus: "Blob" |} "Table"
Meghatározza, hogy hello `source` erőforrás áll rendelkezésre hello helyi fejlesztői környezetben futó hello storage emulator blob.

**Alkalmazandó:** Blobok, táblák

### <a name="desttypeblob--table"></a>/ DestType: "Blob" |} "Table"
Meghatározza, hogy hello `destination` erőforrás áll rendelkezésre hello helyi fejlesztői környezetben futó hello storage emulator blob.

**Alkalmazandó:** Blobok, táblák

### <a name="pkrskey1key2key3"></a>/ PKRS: "key&#1;key&#2;key&#3;..."
Elágazást hello partíciós kulcs tartomány tooenable tábla adatexportálási párhuzamosan, ami növeli a hello az exportálási művelet hello sebességét.

Ha ez a beállítás nincs megadva, az AzCopy egy egyetlen szálon tooexport táblaentitásokat használja. Például ha hello felhasználó határozza meg a /PKRS: "aa #bb", akkor az AzCopy három párhuzamos műveletek kezdődik.

Egyes műveletek exportálja egy három partíció kulcstartományokkal, alább látható módon:

  [első partíciókulcs, aa)

  [aa, bb)

  [bb, utolsó partíciókulcs]

**Alkalmazandó:** táblák

### <a name="splitsizefile-size"></a>/ SplitSize: "fájl mérete"
Megadja az exportált fájl mérete MB vágási hello, hello minimális engedélyezett értéke 32.

Ha ez a beállítás nincs megadva, az AzCopy tábla adatok tooa az egyfájlos exportálja.

Ha hello tábla adatai exportált tooa blob, és hello exportált fájl mérete eléri a következőt hello 200 GB-os korlát a blob-mérethez, majd AzCopy felosztja a hello exportált fájl, akkor is, ha ez a beállítás nincs megadva.

**Alkalmazandó:** táblák

### <a name="entityoperationinsertorskip--insertormerge--insertorreplace"></a>/ EntityOperation: "InsertOrSkip" |} "InsertOrMerge" |} "InsertOrReplace"
Megadja a hello tábla importálása működéshez.

* InsertOrSkip - kihagyja a meglévő entitás vagy szúr be egy új entitást, ha hello tábla nem létezik.
* InsertOrMerge - egyesíti a meglévő entitás vagy szúr be egy új entitást, ha hello tábla nem létezik.
* InsertOrReplace - lecseréli a meglévő entitás vagy szúr be egy új entitást, ha hello tábla nem létezik.

**Alkalmazandó:** táblák

### <a name="manifestmanifest-file"></a>/ Jegyzékfájl: "jegyzékfájl-file"
Adja meg a jegyzékfájl hello hello tábla exportálása, az importálási művelet.

Ez a beállítás akkor választható hello az exportálási művelet során, AzCopy előre definiált nevű jegyzékfájlt állít elő, ha ez a beállítás nincs megadva.

Ez a beállítás akkor szükséges hello adatfájlok helyének hello importálási művelet során.

**Alkalmazandó:** táblák

### <a name="synccopy"></a>/ SyncCopy
Azt jelzi, hogy toosynchronously másolása blobokkal vagy a fájlokat két Azure Storage-végpontok közötti.

Alapértelmezés szerint AzCopy kiszolgálóoldali aszinkron másolatot használja. Adja meg ezt a beállítást egy szinkron tooperform másolja, amely letölti a blobokat vagy toolocal memória fájlok és majd feltölti tooAzure tároló.

Ezt a beállítást is használhatja, amikor belül Blob-tároló, a File storage belül, vagy a Blob storage tooFile tároló- és fordítva fordítva a fájlok másolása.

**Alkalmazandó:** Blobok, fájlok

### <a name="setcontenttypecontent-type"></a>/ SetContentType: "content-type"
Megadja a hello MIME content-type cél blobokkal vagy a fájlokat.

AzCopy beállítása a blob tartalomtípusa hello, vagy a fájl tooapplication/octet-stream alapértelmezés szerint. Beállíthatja a hello content-type blobokkal vagy a fájlok explicit megadása ehhez a beállításhoz tartozó értéket.

Ha megadja ezt a beállítást érték nélküli, majd AzCopy beállítja minden egyes blob vagy a fájl tartalomtípusa szerint tooits fájl kiterjesztése.

**Alkalmazandó:** Blobok, fájlok

### <a name="payloadformatjson--csv"></a>/ PayloadFormat: "JSON" |} "CSV"
Meghatározza a hello tábla exportált fájlt hello formátumot.

Ha ez a beállítás nincs megadva, alapértelmezés szerint AzCopy exportálja tábla adatfájl JSON formátumban.

**Alkalmazandó:** táblák

## <a name="known-issues-and-best-practices"></a>Ismert problémák és ajánlott eljárások
### <a name="limit-concurrent-writes-while-copying-data"></a>Adatok másolása egyidejű írási műveletek korlátozása
AzCopy rendelkező fájlokat vagy a BLOB másolása esetén vegye figyelembe, hogy egy másik alkalmazás módosítja hello adatokat másol, amíg. Ha lehetséges győződjön meg arról, hogy hello adatok másolása nem áll módosítás alatt hello másolási művelet során. Például amikor egy Azure virtuális géphez társított virtuális merevlemez másolása, győződjön meg arról, hogy más alkalmazás nem jelenleg írás toohello VHD-t. Egy jó módszer toodo ezen nem lízing hello erőforrás toobe másolva. Alternatív megoldásként pillanatkép létrehozása a virtuális merevlemez hello először, és másolja hello pillanatkép.

Ha más alkalmazásokat tooblobs vagy fájlok írásában közben másolni, akkor ne feledje, hogy hello idő hello feladat befejezése nem megelőzése érdekében hello másolt erőforrások már nincs teljes paritásos hello forrás erőforrásokkal.

### <a name="run-one-azcopy-instance-on-one-machine"></a>Futtassa az AzCopy egy példány egy számítógépen.
AzCopy tervezett toomaximize hello kihasználtságát a gép erőforrás tooaccelerate hello adatátviteli, azt javasoljuk, hogy csak egy AzCopy példány egy számítógépen futtatja, és hello beállítást adja meg `/NC` Ha több egyidejű műveletek van szüksége. További tudnivalókért írja be a `AzCopy /?:NC` hello parancssorból.

### <a name="enable-fips-compliant-md5-algorithms-for-azcopy-when-you-use-fips-compliant-algorithms-for-encryption-hashing-and-signing"></a>FIPS szabványnak megfelelő MD5 algoritmusok engedélyezése az AzCopy amikor meg "használata FIPS szabványnak megfelelő algoritmusok használata titkosításhoz, kivonatoláshoz és aláíráshoz".
AzCopy alapértelmezés szerint a .NET MD5 megvalósítási toocalculate hello MD5 használja az objektumok másolásakor, de néhány biztonsági követelmények, amelyeket AzCopy tooenable FIPS szabványnak megfelelő MD5-beállítás.

Létrehozhat egy app.config fájl `AzCopy.exe.config` tulajdonsággal `AzureStorageUseV1MD5` , tegye a AzCopy.exe tartalékoljon.

    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
      <appSettings>
        <add key="AzureStorageUseV1MD5" value="false"/>
      </appSettings>
    </configuration>

"AzureStorageUseV1MD5" • tulajdonság igaz - hello alapértelmezett érték, AzCopy használja a .NET MD5 végrehajtására.
• Hamis – AzCopy FIPS-kompatibilis MD5 algoritmust használ.

Vegye figyelembe, hogy FIPS szabványnak megfelelő algoritmus a Windows-számítógépen alapértelmezés szerint le van tiltva, a Futtatás ablakba írja be a secpol.msc, és ellenőrizze, hogy ezt a kapcsolót, a biztonsági beállítások -> helyi házirend -> biztonsági beállítások -> rendszer-kriptográfia: használata FIPS-kompatibilis algoritmusok használata titkosításhoz, kivonatoláshoz és aláíráshoz.

## <a name="next-steps"></a>Következő lépések
Azure Storage és AzCopy kapcsolatos további információkért tekintse meg a következő erőforrások hello:

### <a name="azure-storage-documentation"></a>Az Azure Storage-dokumentáció:
* [Bevezetés tooAzure tároló](../storage-introduction.md)
* [Hogyan toouse a .NET-Blob-tároló](../blobs/storage-dotnet-how-to-use-blobs.md)
* [Hogyan toouse File storage .NET](../storage-dotnet-how-to-use-files.md)
* [Hogyan toouse a Table storage a .NET használatával](../../cosmos-db/table-storage-how-to-use-dotnet.md)
* [Hogyan toocreate, kezelése vagy törlése](../storage-create-storage-account.md)
* [Adatátvitel az AzCopy Linux rendszeren](storage-use-azcopy-linux.md)

### <a name="azure-storage-blog-posts"></a>Az Azure Storage blogbejegyzések:
* [Introducing Azure Storage adatátviteli könyvtár megtekintés](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/)
* [AzCopy: Bevezetéséről szinkron másolatot és testreszabott tartalom típusa](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/01/13/azcopy-introducing-synchronous-copy-and-customized-content-type.aspx)
* [AzCopy: Általános rendelkezésre állási az AzCopy 3.0 és a tábla és a fájl-támogatással rendelkező az AzCopy 4.0 előzetes bejelentése](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/10/29/azcopy-announcing-general-availability-of-azcopy-3-0-plus-preview-release-of-azcopy-4-0-with-table-and-file-support.aspx)
* [AzCopy: A felügyeleti teendők központjaként másolással optimalizált](http://go.microsoft.com/fwlink/?LinkId=507682)
* [AzCopy: Írásvédett georedundáns tárolás támogatása](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/04/07/azcopy-support-for-read-access-geo-redundant-account.aspx)
* [AzCopy: Adatátvitelt újra elindítható móddal és SAS-jogkivonat](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/09/07/azcopy-transfer-data-with-re-startable-mode-and-sas-token.aspx)
* [AzCopy: Kereszt-fiók másolási Blob használatával](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/04/01/azcopy-using-cross-account-copy-blob.aspx)
* [AzCopy: Feltöltése/fájlok letöltése Azure Blobokra vonatkozó](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/12/03/azcopy-uploading-downloading-files-for-windows-azure-blobs.aspx)

