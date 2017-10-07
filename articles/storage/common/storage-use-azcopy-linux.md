---
title: "aaaCopy, vagy helyezze adatok tooAzure Linux AzCopy szolgáltatással |} Microsoft Docs"
description: "Hello AzCopy használata Linux segédprogram toomove vagy másolat adatok tooor a blob és a fájl tartalmát. Adatok tooAzure tárolási átmásolhatja a helyi fájlokból, vagy adatok belül vagy tárfiókok között. Az adatok tooAzure Storage könnyen át."
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
ms.date: 05/11/2017
ms.author: seguler
ms.openlocfilehash: ee39c311d996a046999b7fd4a4eb873f25b4eb86
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="transfer-data-with-azcopy-on-linux"></a>Adatátvitel az AzCopy Linux rendszeren
AzCopy Linux egy parancssori segédprogram, az adatok tooand másolását a Microsoft Azure-Blob és a fájl tárolási egyszerű parancsokkal optimális teljesítménnyel. Adatokat másolhat egy objektum tooanother a tárfiókon belül vagy tárfiókok között.

AzCopy, letöltheti a két verziója van. AzCopy Linux ajánlat POSIX-stílusú parancssori kapcsolók Linux platformon célozza .NET Core keretrendszerrel épül. [A Windows AzCopy](../storage-use-azcopy.md) a .NET-keretrendszer épül, és ez biztosítja a Windows stílus parancssori kapcsolókat. Ez a cikk a Linux AzCopy ismerteti.

## <a name="download-and-install-azcopy"></a>Töltse le és telepítse az AzCopy
### <a name="installation-on-linux"></a>Linux-telepítés

AzCopy Linux szüksége van a .NET Core keretrendszer hello platformon. Hello telepítési utasításait lásd a hello [.NET Core](https://www.microsoft.com/net/core#linuxubuntu) lap.

Tegyük fel most telepítse a .NET Core Ubuntu 16.10. Hello legújabb telepítési útmutató a Microsoft [.NET Core Linux](https://www.microsoft.com/net/core#linuxubuntu) telepítési oldal.


```bash
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ yakkety main" > /etc/apt/sources.list.d/dotnetdev.list' 
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
sudo apt-get update
sudo apt-get install dotnet-dev-1.0.3
```

A .NET Core telepítése után töltse le és telepítse az AzCopy.

```bash
wget -O azcopy.tar.gz https://aka.ms/downloadazcopyprlinux
tar -xf azcopy.tar.gz
sudo ./install.sh
```

Hello kibontott fájlokat is távolítható el, ha az AzCopy Linux rendszeren telepítve van. Másik lehetőségként nem jogosult felügyelő, ha is futtathatja hello parancsfájl használatával AzCopy "azcopy" hello kibontott mappában. 

### <a name="alternative-installation-on-ubuntu"></a>Ubuntu alternatív telepítése

**Ubuntu 14.04**

Apt-forrás hozzáadása a .net Core:

```bash
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ trusty main" > /etc/apt/sources.list.d/dotnetdev.list' 
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
```

Microsoft Linux termék tárház apt forrás hozzáadása, és telepítse az AzCopy:

```bash
curl https://packages.microsoft.com/config/ubuntu/14.04/prod.list > ./microsoft-prod.list
sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
```

```bash
sudo apt-get update
sudo apt-get install azcopy
```

**Ubuntu 16.04**

Apt-forrás hozzáadása a .net Core:

```bash
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ xenial main" > /etc/apt/sources.list.d/dotnetdev.list' 
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
```

Microsoft Linux termék tárház apt forrás hozzáadása, és telepítse az AzCopy:

```bash
curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > ./microsoft-prod.list
sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
```

```bash
sudo apt-get update
sudo apt-get install azcopy
```

**Ubuntu 16.10**

Apt-forrás hozzáadása a .net Core:

```bash
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ yakkety main" > /etc/apt/sources.list.d/dotnetdev.list' 
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
```

Microsoft Linux termék tárház apt forrás hozzáadása, és telepítse az AzCopy:

```bash
curl https://packages.microsoft.com/config/ubuntu/16.10/prod.list > ./microsoft-prod.list
sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
```

```bash
sudo apt-get update
sudo apt-get install azcopy
```

## <a name="writing-your-first-azcopy-command"></a>Az első AzCopy parancs írása
hello alapvető AzCopy parancs szintaxisa a következő:

```azcopy
azcopy --source <source> --destination <destination> [Options]
```

hello a következő példák bemutatják, a Microsoft Azure-BLOB és a fájlok másolását adatok tooand különböző lehetőségeket. Tekintse meg a toohello `azcopy --help` menü használt minden egyes minta hello paraméterek részletes leírását.

## <a name="blob-download"></a>BLOB: letöltése
### <a name="download-single-blob"></a>Egy blob letöltése

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --include "abc.txt"
```

Ha hello mappa `/mnt/myfiles` nem létezik, AzCopy létrehozza, és letölti `abc.txt ` hello új mappába.

### <a name="download-single-blob-from-secondary-region"></a>A másodlagos régióba egyetlen blob letöltése

```azcopy
azcopy \
    --source https://myaccount-secondary.blob.core.windows.net/mynewcontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --include "abc.txt"
```

Vegye figyelembe, hogy írásvédett georedundáns tárolás engedélyezve kell rendelkeznie.

### <a name="download-all-blobs"></a>Töltse le az összes BLOB

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --recursive
```

Hello következő feltételezik hello megadott tárolóban található blobok:  

```
abc.txt
abc1.txt
abc2.txt
vd1/a.txt
vd1/abcd.txt
```

Hello letöltési művelet után hello directory `/mnt/myfiles` tartalmazza a következő fájlok hello:

```
/mnt/myfiles/abc.txt
/mnt/myfiles/abc1.txt
/mnt/myfiles/abc2.txt
/mnt/myfiles/vd1/a.txt
/mnt/myfiles/vd1/abcd.txt
```

Ha nem adja meg a beállítás `--recursive`, nincs blob le lesznek töltve.

### <a name="download-blobs-with-specified-prefix"></a>A megadott előtag blobok letöltése

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --include "a" \
    --recursive
```

Hello következő feltételezik hello megadott tárolóban található blobok. Hello előtaggal kezdődő összes BLOB `a` letöltődnek.

```
abc.txt
abc1.txt
abc2.txt
xyz.txt
vd1\a.txt
vd1\abcd.txt
```

Hello letöltési művelet után hello mappa `/mnt/myfiles` tartalmazza a következő fájlok hello:

```
/mnt/myfiles/abc.txt
/mnt/myfiles/abc1.txt
/mnt/myfiles/abc2.txt
```

hello előtag toohello virtuális könyvtár, hello hello blob neve az első részét képező vonatkozik. Hello a fenti példában hello virtuális könyvtár nem azonos hello megadott előtag, így nincs blob letöltődik. Emellett, ha hello lehetőséget `--recursive` nincs megadva, az AzCopy nem tölti le a blobokat.

### <a name="set-hello-last-modified-time-of-exported-files-toobe-same-as-hello-source-blobs"></a>Állítsa be az exportált fájlokat toobe hello utolsó módosításának időpontja szerint forrás blobok hello

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination "/mnt/myfiles" \
    --source-key <key> \
    --preserve-last-modified-time
```

Blobok emellett kizárja hello letöltési művelet az utolsó módosításának ideje alapján. Például, ha azt szeretné, hogy a tooexclude blobot, amelynek utolsó módosítási időpontjának hello azonos vagy újabb, mint hello célfájl, adja meg hello `--exclude-newer` lehetőséget:

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --preserve-last-modified-time \
    --exclude-newer
```

Vagy ha azt szeretné, hogy tooexclude blobot, amelynek utolsó módosítási időpontjának hello azonos vagy régebbi, mint hello célfájl, adja hozzá a hello `--exclude-older` lehetőséget:

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --preserve-last-modified-time \
    --exclude-older
```

## <a name="blob-upload"></a>BLOB: feltöltése
### <a name="upload-single-file"></a>Töltse fel egy fájlból

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --include "abc.txt"
```

Ha hello megadott tároló nem létezik, AzCopy létrehozza, és feltöltések fájl hello bele.

### <a name="upload-single-file-toovirtual-directory"></a>Egy fájlból toovirtual directory feltöltése

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --include "abc.txt"
```

Ha hello megadott virtuális könyvtár nem létezik, AzCopy feltölt hello tooinclude hello virtuális könyvtárának a hello blob neve (*pl.*, `vd/abc.txt` a fenti példában hello).

### <a name="upload-all-files"></a>Minden fájl feltöltése

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --recursive
```

Beállítás megadása `--recursive` hello feltöltések hello tartalmát a megadott könyvtár tooBlob tárolási rekurzív módon, ami azt jelenti, hogy minden almappa és a fájljaikat, valamint feltöltése. Például azt feltételezik hello következő mappában található fájlok `/mnt/myfiles`:

```
/mnt/myfiles/abc.txt
/mnt/myfiles/abc1.txt
/mnt/myfiles/abc2.txt
/mnt/myfiles/subfolder/a.txt
/mnt/myfiles/subfolder/abcd.txt
```

Hello feltöltési művelet után hello tároló tartalmazza a következő fájlok hello:

```
abc.txt
abc1.txt
abc2.txt
subfolder/a.txt
subfolder/abcd.txt
```

Ha a beállítás hello `--recursive` nincs megadva, csak hello következő három fájlok feltöltése:

```
abc.txt
abc1.txt
abc2.txt
```

### <a name="upload-files-matching-specified-pattern"></a>Töltse fel a megadott mintának megfelelő fájlok

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --include "a*" \
    --recursive
```

Hello következő feltételezik mappában található fájlok `/mnt/myfiles`:

```
/mnt/myfiles/abc.txt
/mnt/myfiles/abc1.txt
/mnt/myfiles/abc2.txt
/mnt/myfiles/xyz.txt
/mnt/myfiles/subfolder/a.txt
/mnt/myfiles/subfolder/abcd.txt
```

Hello feltöltési művelet után hello tároló tartalmazza a következő fájlok hello:

```
abc.txt
abc1.txt
abc2.txt
subfolder/a.txt
subfolder/abcd.txt
```

Ha a beállítás hello `--recursive` nincs megadva, az AzCopy kihagyja alkönyvtár lévő fájlok:

```
abc.txt
abc1.txt
abc2.txt
```

### <a name="specify-hello-mime-content-type-of-a-destination-blob"></a>Adjon meg egy cél BLOB hello MIME tartalomtípus
Alapértelmezés szerint AzCopy beállítja hello tartalomtípusa cél blob túl`application/octet-stream`. Azonban, explicit módon megadhat hello tartalomtípus keresztül hello beállítás `--set-content-type [content-type]`. Ez a szintaxis hello content-type összes BLOB a feltöltési művelet állítja be.

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/myContainer/ \
    --dest-key <key> \
    --include "ab" \
    --set-content-type "video/mp4"
```

Ha hello lehetőséget `--set-content-type` egy érték nélkül van megadva, majd az AzCopy állítja be, minden egyes blob vagy a fájl CODEPAGE tartalomtípus szerint tooits fájl kiterjesztése.

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/myContainer/ \
    --dest-key <key> \
    --include "ab" \
    --set-content-type
```

## <a name="blob-copy"></a>BLOB: másolása
### <a name="copy-single-blob-within-storage-account"></a>Másolja át egy blob Storage-fiókban

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer1 \
    --destination https://myaccount.blob.core.windows.net/mycontainer2 \
    --source-key <key> \
    --dest-key <key> \
    --include "abc.txt"
```

Ha--másolatának szinkronizálása beállítás nélkül egy blobot másol egy [kiszolgálóoldali másolatot](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) művelet.

### <a name="copy-single-blob-across-storage-accounts"></a>Másolja át egy blob Storage-fiókok között

```azcopy
azcopy \
    --source https://sourceaccount.blob.core.windows.net/mycontainer1 \
    --destination https://destaccount.blob.core.windows.net/mycontainer2 \
    --source-key <key1> \
    --dest-key <key2> \
    --include "abc.txt"
```

Ha--másolatának szinkronizálása beállítás nélkül egy blobot másol egy [kiszolgálóoldali másolatot](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) művelet.

### <a name="copy-single-blob-from-secondary-region-tooprimary-region"></a>Egy blob másolása másodlagos régióba tooprimary régió

```azcopy
azcopy \
    --source https://myaccount1-secondary.blob.core.windows.net/mynewcontainer1 \
    --destination https://myaccount2.blob.core.windows.net/mynewcontainer2 \
    --source-key <key1> \
    --dest-key <key2> \
    --include "abc.txt"
```

Vegye figyelembe, hogy írásvédett georedundáns tárolás engedélyezve kell rendelkeznie.

### <a name="copy-single-blob-and-its-snapshots-across-storage-accounts"></a>Egy blob másolási és a pillanatképek tárfiókok között

```azcopy
azcopy \
    --source https://sourceaccount.blob.core.windows.net/mycontainer1 \
    --destination https://destaccount.blob.core.windows.net/mycontainer2 \
    --source-key <key1> \
    --dest-key <key2> \
    --include "abc.txt" \
    --include-snapshot
```

Hello másolási művelet után a hello céltároló hello blob és a pillanatképek tartalmazza. hello tároló tartalmazza hello következő blob és a pillanatképek:

```
abc.txt
abc (2013-02-25 080757).txt
abc (2014-02-21 150331).txt
```

### <a name="synchronously-copy-blobs-across-storage-accounts"></a>Szinkron módon másolni BLOB Storage-fiókok
AzCopy alapértelmezés szerint aszinkron módon másolja az adatokat két tárolási végpontok közötti. Ezért hello másolási művelet futtatása hello háttérben tartalékolt sávszélesség-kapacitása, amelynek nincs szolgáltatásiszint-szerződés szempontjából hogyan gyors blob használatával kell másolni. 

Hello `--sync-copy` lehetőség biztosítja, hogy hello másolási művelet lekérdezi az egységes sebesség. AzCopy elkészíti hello szinkron hello blobok letöltésével toocopy hello a megadott forrás toolocal memória, és majd feltöltheti toohello Blob célhelyet.

```azcopy
azcopy \
    --source https://myaccount1.blob.core.windows.net/myContainer/ \
    --destination https://myaccount2.blob.core.windows.net/myContainer/ \
    --source-key <key1> \
    --dest-key <key2> \
    --include "ab" \
    --sync-copy
```

`--sync-copy`További kilépő képest költség tooasynchronous másolási hozhat létre. az ajánlott megközelítést alkalmazva hello toouse van ez a beállítás egy Azure virtuális gép, hello lévő és a forrás tárolási fiók tooavoid kilépő költségű ugyanabban a régióban.

## <a name="file-download"></a>Fájl: letöltése
### <a name="download-single-file"></a>Töltse le egy fájlból

```azcopy
azcopy \
    --source https://myaccount.file.core.windows.net/myfileshare/myfolder1/ \
    --destination /mnt/myfiles \
    --source-key <key> \
    --include "abc.txt"
```

Hello adott forrása az Azure fájlmegosztások, akkor meg kell adnia hello pontosan a fájl nevét, ha (*pl.* `abc.txt`) toodownload egyetlen fájl, vagy adja meg a beállítás `--recursive` összes toodownload hello megosztáson található fájlok rekurzív módon. Kísérlet toospecify egy fájl és egyaránt lehetőség `--recursive` hiba együtt eredményez.

### <a name="download-all-files"></a>Minden fájl letöltése

```azcopy
azcopy \
    --source https://myaccount.file.core.windows.net/myfileshare/ \
    --destination /mnt/myfiles \
    --source-key <key> \
    --recursive
```

Vegye figyelembe, hogy a rendszer nem tölti le üres mappák.

## <a name="file-upload"></a>Fájl: feltöltése
### <a name="upload-single-file"></a>Töltse fel egy fájlból

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.file.core.windows.net/myfileshare/ \
    --dest-key <key> \
    --include abc.txt
```

### <a name="upload-all-files"></a>Minden fájl feltöltése

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.file.core.windows.net/myfileshare/ \
    --dest-key <key> \
    --recursive
```

Vegye figyelembe, hogy az üres mappák nem töltődött fel.

### <a name="upload-files-matching-specified-pattern"></a>Töltse fel a megadott mintának megfelelő fájlok

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.file.core.windows.net/myfileshare/ \
    --dest-key <key> \
    --include "ab*" \
    --recursive
```

## <a name="file-copy"></a>Fájl: másolása
### <a name="copy-across-file-shares"></a>Fájlmegosztások másolni

```azcopy
azcopy \
    --source https://myaccount1.file.core.windows.net/myfileshare1/ \
    --destination https://myaccount2.file.core.windows.net/myfileshare2/ \
    --source-key <key1> \
    --dest-key <key2> \
    --recursive
```
Amikor fájlmegosztások között másolhat egy fájlt egy [kiszolgálóoldali másolatot](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) művelet.

### <a name="copy-from-file-share-tooblob"></a>Megosztás tooblob fájl másolása

```azcopy
azcopy \ 
    --source https://myaccount1.file.core.windows.net/myfileshare/ \
    --destination https://myaccount2.blob.core.windows.net/mycontainer/ \
    --source-key <key1> \
    --dest-key <key2> \
    --recursive
```
Ha fájlt átmásolni fájl megosztási tooblob, egy [kiszolgálóoldali másolatot](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) művelet.

### <a name="copy-from-blob-toofile-share"></a>A blob toofile megosztásból másolása

```azcopy
azcopy \
    --source https://myaccount1.blob.core.windows.net/mycontainer/ \
    --destination https://myaccount2.file.core.windows.net/myfileshare/ \
    --source-key <key1> \
    --dest-key <key2> \
    --recursive
```
Amikor blob toofile megosztásból másolhat egy fájlt egy [kiszolgálóoldali másolatot](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) művelet.

### <a name="synchronously-copy-files"></a>Szinkron módon történik a fájlok másolása
Megadhatja a hello `--sync-copy` toocopy adatok File Storage tooFile tároló, a File Storage tooBlob tároló és a tárolási Blob Storage tooFile szinkron módon lehetőséget. AzCopy művelet hello forrás adatok toolocal memória letöltés és toodestination feltöltése szerint fut. Ebben az esetben a szabványos kilépő költség érvényes.

```azcopy
azcopy \
    --source https://myaccount1.file.core.windows.net/myfileshare1/ \
    --destination https://myaccount2.file.core.windows.net/myfileshare2/ \
    --source-key <key1> \
    --dest-key <key2> \
    --recursive \
    --sync-copy
```

Amikor a File Storage tooBlob tárolási másol, hello alapértelmezett blob típushoz blokkblob, felhasználói beállítást adja meg `/BlobType:page` toochange hello céltípus blob.

Vegye figyelembe, hogy `--sync-copy` hozhat létre további kilépő összehasonlító tooasynchronous másolási költsége. az ajánlott megközelítést alkalmazva hello toouse van ez a beállítás egy Azure virtuális gép, hello lévő és a forrás tárolási fiók tooavoid kilépő költségű ugyanabban a régióban.

## <a name="other-azcopy-features"></a>Más AzCopy szolgáltatások
### <a name="only-copy-data-that-doesnt-exist-in-hello-destination"></a>Csak másolja az adatokat, amely a célként megadott hello nem létezik
Hello `--exclude-older` és `--exclude-newer` paraméterek tooexclude régebbi vagy újabb forrás erőforrások másolását, illetve lehetővé teszik. Ha csak toocopy forrás erőforrásokat, amelyek nem léteznek a hello cél, mindkét paraméter hello AzCopy parancs adhat meg:

    --source http://myaccount.blob.core.windows.net/mycontainer --destination /mnt/myfiles --source-key <sourcekey> --recursive --exclude-older --exclude-newer

    --source /mnt/myfiles --destination http://myaccount.file.core.windows.net/myfileshare --dest-key <destkey> --recursive --exclude-older --exclude-newer

    --source http://myaccount.blob.core.windows.net/mycontainer --destination http://myaccount.blob.core.windows.net/mycontainer1 --source-key <sourcekey> --dest-key <destkey> --recursive --exclude-older --exclude-newer

### <a name="use-a-configuration-file-toospecify-command-line-parameters"></a>Egy konfigurációs fájl toospecify parancssori paraméterek használata

```azcopy
azcopy --config-file "azcopy-config.ini"
```

AzCopy parancssori paramétereket is megadhat a konfigurációs fájlt. AzCopy folyamatok hello hello fájlban paramétereket, mintha hello parancssorban közvetlen helyettesítés hello tartalma hello fájl végrehajtása meg lett.

Tegyük fel, a konfigurációs fájl nevű `copyoperation`, amely tartalmazza a következő sorokat hello. Minden egyes AzCopy paraméter egy sorba adható meg.

    --source http://myaccount.blob.core.windows.net/mycontainer --destination /mnt/myfiles --source-key <sourcekey> --recursive --quiet

vagy külön sorok:

    --source http://myaccount.blob.core.windows.net/mycontainer
    --destination /mnt/myfiles
    --source-key<sourcekey>
    --recursive
    --quiet

AzCopy sikertelen lesz, ha hello paraméter osztani két sort, ahogy az itt látható a hello `--source-key` paraméter:

    http://myaccount.blob.core.windows.net/mycontainer
    /mnt/myfiles
    --sourcekey
    <sourcekey>
    --recursive
    --quiet

### <a name="specify-a-shared-access-signature-sas"></a>Adja meg a közös hozzáférésű jogosultságkód (SAS)

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer1 \
    --destination https://myaccount.blob.core.windows.net/mycontainer2 \
    --source-sas <SAS1> \
    --dest-sas <SAS2> \
    --include abc.txt
```

Azt is megadhatja a SAS hello tárolóra URI:

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer1/?SourceSASToken \
    --destination /mnt/myfiles \
    --recursive
```

AzCopy jelenleg csak támogatja a hello [fiók SAS](https://docs.microsoft.com/en-us/azure/storage/storage-dotnet-shared-access-signature-part-1).

### <a name="journal-file-folder"></a>Napló fájlmappa
Minden alkalommal, amikor egy parancs tooAzCopy kiadása ellenőrzi, hogy a napló fájl megtalálható-e hello alapértelmezett mappát, vagy hogy keresztül ez a beállítás a megadott mappa létezik. Hello napló fájl nem létezik egyik helyen sem, ha AzCopy hello művelet új kezeli, és létrehoz egy új naplófájl.

Hello napló fájl létezik, az AzCopy ellenőrzi, hogy megadott hello parancssori megegyezik-e parancssori hello hello napló fájlban. Hello két parancssorokat felel meg, ha az AzCopy hello hiányos művelet folytatása. Ha nem egyeznek, az AzCopy felhasználói tooeither felülírása hello napló fájl toostart új művelet, vagy toocancel hello aktuális művelet megadását kéri.

Ha azt szeretné, hogy toouse hello hello napló fájl alapértelmezett helye:

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --resume
```

Ha a beállítás nincs megadva `--resume`, vagy adja meg a beállítás `--resume` nélkül hello mappa elérési útja, a fent látható AzCopy fájlt hoz létre hello napló hello alapértelmezett helyen, amely `~\Microsoft\Azure\AzCopy`. Ha hello napló fájl már létezik, majd AzCopy folytatja hello művelet hello napló fájl alapján.

Ha azt szeretné, hogy egy egyéni helyet hello napló fájl toospecify:

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key key \
    --resume "/mnt/myjournal"
```

Ez a példa hello napló fájlt hoz létre, ha még nem létezik. Ha létezik, majd AzCopy folytatja hello művelet hello napló fájl alapján.

Ha azt szeretné, hogy az AzCopy művelet tooresume, ismételje meg a hello ugyanezt a parancsot. AzCopy Linux majd megerősítést kér fogja kérni:

```azcopy
Incomplete operation with same command line detected at hello journal directory "/home/myaccount/Microsoft/Azure/AzCopy", do you want tooresume hello operation? Choose Yes tooresume, choose No toooverwrite hello journal toostart a new operation. (Yes/No)
```

### <a name="output-verbose-logs"></a>Kimeneti részletes naplókat

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --verbose
```

### <a name="specify-hello-number-of-concurrent-operations-toostart"></a>Adja meg a hello száma párhuzamos műveletek toostart
A beállítás `--parallel-level` hello egyidejű másolási műveletek számát adja meg. Alapértelmezés szerint az AzCopy elindul egy bizonyos száma párhuzamos műveletek átviteli tooincrease hello adatátvitelt. hello száma párhuzamos műveletek nyolc alkalommal hello processzorral rendelkezik. Ha AzCopy kis sávszélességű hálózaton keresztül futtatja, megadhatja a--párhuzamos szintű tooavoid hibáját okozta. erőforrás verseny kevesebb.

[!TIP]
>tooview hello teljes listáját az AzCopy paraméterek, tekintse meg a "azcopy – súgó" menü.

## <a name="known-issues-and-best-practices"></a>Ismert problémák és ajánlott eljárások
### <a name="error-net-core-is-not-found-in-hello-system"></a>Hiba: A .NET Core nem található a hello rendszer.
Ha arról, hogy a .NET Core nincs telepítve a hello rendszer hibát észlel, elérési út toohello .NET Core bináris hello `dotnet` lehet, hogy hiányzik.

A rendezés tooaddress probléma, hello .NET Core bináris hello rendszerben található:
```bash
sudo find / -name dotnet
```

Ez visszaad hello elérési toohello dotnet bináris. 

    /opt/rh/rh-dotnetcore11/root/usr/bin/dotnet
    /opt/rh/rh-dotnetcore11/root/usr/lib64/dotnetcore/dotnet
    /opt/rh/rh-dotnetcore11/root/usr/lib64/dotnetcore/shared/Microsoft.NETCore.App/1.1.2/dotnet

Az elérési út toohello ELÉRÉSI változó most hozzáadása. A sudo secure_path toocontain hello elérési toohello dotnet bináris szerkesztése:
```bash 
sudo visudo
### Append hello path found in hello preceding example too'secure_path' variable
```

Ebben a példában secure_path változó olvassa be, mint:

```
secure_path = /sbin:/bin:/usr/sbin:/usr/bin:/opt/rh/rh-dotnetcore11/root/usr/bin/
```

Hello aktuális felhasználó esetében.bash_profile/.profile tooinclude hello elérési toohello dotnet bináris PATH változóban szerkesztése 
```bash
vi ~/.bash_profile
### Append hello path found in hello preceding example too'PATH' variable
```

Győződjön meg arról, hogy a .NET Core most az elérési út:
```bash
which dotnet
sudo which dotnet
```

### <a name="error-installing-azcopy"></a>Hiba történt az AzCopy telepítése
Ha hibát tapasztal az AzCopy telepítési, megpróbálhatja újból hello bash parancsfájlok használatát hello AzCopy kibontott toorun `azcopy` mappa.

```bash
cd azcopy
./azcopy
```

### <a name="limit-concurrent-writes-while-copying-data"></a>Adatok másolása egyidejű írási műveletek korlátozása
AzCopy rendelkező fájlokat vagy a BLOB másolása esetén vegye figyelembe, hogy egy másik alkalmazás módosítja hello adatokat másol, amíg. Ha lehetséges győződjön meg arról, hogy hello adatok másolása nem áll módosítás alatt hello másolási művelet során. Például amikor egy Azure virtuális géphez társított virtuális merevlemez másolása, győződjön meg arról, hogy más alkalmazás nem jelenleg írás toohello VHD-t. Egy jó módszer toodo ezen nem lízing hello erőforrás toobe másolva. Alternatív megoldásként pillanatkép létrehozása a virtuális merevlemez hello először, és másolja hello pillanatkép.

Ha más alkalmazásokat tooblobs vagy fájlok írásában közben másolni, akkor ne feledje, hogy hello idő hello feladat befejezése nem megelőzése érdekében hello másolt erőforrások már nincs teljes paritásos hello forrás erőforrásokkal.

### <a name="run-one-azcopy-instance-on-one-machine"></a>Futtassa az AzCopy egy példány egy számítógépen.
AzCopy tervezett toomaximize hello kihasználtságát a gép erőforrás tooaccelerate hello adatátviteli, azt javasoljuk, hogy csak egy AzCopy példány egy számítógépen futtatja, és hello beállítást adja meg `--parallel-level` Ha több egyidejű műveletek van szüksége. További tudnivalókért írja be a `AzCopy --help parallel-level` hello parancssorból.

## <a name="next-steps"></a>Következő lépések
Azure Storage és AzCopy kapcsolatos további információkért tekintse meg a következő erőforrások hello:

### <a name="azure-storage-documentation"></a>Az Azure Storage-dokumentáció:
* [Bevezetés tooAzure tároló](../storage-introduction.md)
* [Tárfiók létrehozása](../storage-create-storage-account.md)
* [A Tártallózó alkalmazással blobok kezelése](https://docs.microsoft.com/en-us/azure/vs-azure-tools-storage-explorer-blobs)
* [Az Azure Storage hello Azure CLI 2.0 használatával](../storage-azure-cli.md)
* [Hogyan toouse Blob storage-ának C++](../blobs/storage-c-plus-plus-how-to-use-blobs.md)
* [Hogyan toouse Blob storage-ának Java](../blobs/storage-java-how-to-use-blob-storage.md)
* [Hogyan toouse Blob-tároló Node.js-ből](../blobs/storage-nodejs-how-to-use-blob-storage.md)
* [Hogyan toouse Blob storage-ának Python](../blobs/storage-python-how-to-use-blob-storage.md)

### <a name="azure-storage-blog-posts"></a>Az Azure Storage blogbejegyzések:
* [AzCopy lévő Linux előzetes bejelentése](https://azure.microsoft.com/en-in/blog/announcing-azcopy-on-linux-preview/)
* [Introducing Azure Storage adatátviteli könyvtár megtekintés](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/)
* [AzCopy: Bevezetéséről szinkron másolatot és testreszabott tartalom típusa](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/01/13/azcopy-introducing-synchronous-copy-and-customized-content-type.aspx)
* [AzCopy: Általános rendelkezésre állási az AzCopy 3.0 és a tábla és a fájl-támogatással rendelkező az AzCopy 4.0 előzetes bejelentése](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/10/29/azcopy-announcing-general-availability-of-azcopy-3-0-plus-preview-release-of-azcopy-4-0-with-table-and-file-support.aspx)
* [AzCopy: A felügyeleti teendők központjaként másolással optimalizált](http://go.microsoft.com/fwlink/?LinkId=507682)
* [AzCopy: Írásvédett georedundáns tárolás támogatása](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/04/07/azcopy-support-for-read-access-geo-redundant-account.aspx)
* [AzCopy: Adatátvitelt újraindítható móddal és SAS-jogkivonat](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/09/07/azcopy-transfer-data-with-re-startable-mode-and-sas-token.aspx)
* [AzCopy: Kereszt-fiók másolási Blob használatával](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/04/01/azcopy-using-cross-account-copy-blob.aspx)
* [AzCopy: Feltöltése/fájlok letöltése Azure Blobokra vonatkozó](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/12/03/azcopy-uploading-downloading-files-for-windows-azure-blobs.aspx)

