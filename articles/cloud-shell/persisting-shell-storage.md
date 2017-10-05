---
title: "Azure Cloud rendszerhéj (előzetes verzió) fájlok maradnak |} Microsoft Docs"
description: "A forgatókönyv a hogyan Azure Cloud rendszerhéj fájlokat továbbra is fennáll."
services: 
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: juluk
ms.openlocfilehash: 61a8bfcf3704f361432400771d8fcc8b81927b53
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="persist-files-in-azure-cloud-shell"></a>Azure Cloud rendszerhéj fájlok megőrzése
Felhő rendszerhéj kihasználja az Azure File storage fájlok megőrizni a munkamenetek között.

## <a name="set-up-a-clouddrive-file-share"></a>Állítson be egy `clouddrive` fájlmegosztás
A kezdeti kezdőlapon a felhő rendszerhéj kéri, hogy társítson egy új vagy meglévő fájlmegosztást fájlok megőrizni a munkamenetek között.

### <a name="create-new-storage"></a>Új tároló létrehozása

Ha alapszintű beállításokat alkalmazza, és válassza ki az előfizetés csak, felhőalapú rendszerhéj az Ön nevében, akkor a legközelebb esik a támogatott régióban hoz létre három erőforrásokat:
* Erőforráscsoport:`cloud-shell-storage-<region>`
* Storage-fiók:`cs<uniqueGuid>`
* Fájlmegosztás:`cs-<user>-<domain>-com-<uniqueGuid>`

![Az előfizetési beállítás](media/basic-storage.png)

A fájlmegosztás csatlakoztatja `clouddrive` a a `$Home` könyvtár. A fájlmegosztás is tárolására szolgál egy 5 GB-os lemezképet, amely jön létre, és, amely automatikusan frissíti, és továbbra is fennáll a `$Home` könyvtár. Ez egy egyszeri művelet, és a további munkamenetekhez automatikusan csatlakoztatja a fájlmegosztást.

### <a name="use-existing-resources"></a>Létező erőforrásokból

A speciális beállítás használatával társíthatja a meglévő erőforrásokat. Ha a tárolási telepítő parancssor jelenik meg, válassza ki a **megjelenítése speciális beállítások** további beállítások. Meglévő fájlmegosztások kap egy 5 GB-os felhasználói lemezképet a `$Home` könyvtár. A legördülő menükben helyi redundáns & georedundáns storage-fiókok és a felhő rendszerhéj régió szűrve.

![Az erőforrás-csoport beállítás](media/advanced-storage.png)

### <a name="restrict-resource-creation-with-an-azure-resource-policy"></a>Erőforrás létrehozása az Azure-erőforrás házirendjével korlátozása
A felhő rendszerhéj létrehozott tárfiókok vannak látva `ms-resource-usage:azure-cloud-shell`. Ha azt szeretné, hogy a storage-fiókok létrehozása a felhő rendszerhéj letilthatja, hozzon létre egy [Azure-erőforrás házirend címkék](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-policy-tags) , amely váltja ki ezt a címkét.

## <a name="how-cloud-shell-works"></a>Felhő rendszerhéj működése
Felhő rendszerhéj továbbra is fennáll, az alábbi eljárások közül mindkettő keresztül fájlokat:
* A lemez lemezkép létrehozása a `$Home` directory megőrizni a címtárban található összes tartalmat. Az alapkonfiguráció lemezképét mentette: a megadott fájlmegosztással `acc_<User>.img` : `fileshare.storage.windows.net/fileshare/.cloudconsole/acc_<User>.img`, és automatikusan szinkronizálja a módosításokat.

* A megadott fájlmegosztással csatlakoztatása `clouddrive` a a `$Home` való közvetlen fájl megosztási interakció könyvtárába. `/Home/<User>/clouddrive`van leképezve `fileshare.storage.windows.net/fileshare`.
 
> [!NOTE]
> Az összes fájlt a `$Home` címtár, például az SSH-kulcsokat, a lemez a csatlakoztatott fájl megosztáson található felhasználói lemezkép megmaradnak. Gyakorlati tanácsok alkalmazza, ha az adatokat továbbra is fennáll a `$Home` directory és a csatlakoztatott fájlmegosztás.

## <a name="use-the-clouddrive-command"></a>Használja a `clouddrive` parancs
Felhő rendszerhéj is futtathatja egy olyan parancsot `clouddrive`, amely lehetővé teszi, hogy manuálisan frissítse a felhő rendszerhéj a fájlmegosztást, amely csatlakoztatva van.
![A "clouddrive" parancs futtatása](media/clouddrive-h.png)

## <a name="mount-a-new-clouddrive"></a>Egy új csatlakoztatása`clouddrive`

### <a name="prerequisites-for-manual-mounting"></a>Manuális csatlakoztatásának előfeltételei
A fájlmegosztás tartozó felhő rendszerhéj használatával frissítheti a `clouddrive mount` parancsot.

Ha egy meglévő fájlmegosztást csatlakoztatja, a tárfiókok kell lennie:
* Helyileg redundáns tárolás vagy georedundáns tárolás fájlmegosztások támogatásához.
* A hozzárendelt régióban található. Amikor bevezetése, a régiót hozzá van rendelve, az erőforráscsoport neve szerepel `cloud-shell-storage-<region>`.

### <a name="supported-storage-regions"></a>Támogatott tárolási régiók
Az Azure files és a felhő rendszerhéj gépet, hogy van-e csatlakoztatni őket ugyanabban a régióban kell lennie. Felhő rendszerhéj fürtök jelenleg a következő régiókban található:
|Terület|Régió|
|---|---|
|Amerika|USA keleti régiója, USA déli középső RÉGIÓJA, USA nyugati régiója|
|Európa|Észak-Európa, Nyugat-Európa|
|Ázsia és a Csendes-óceáni térség|India központi, Délkelet-Ázsia|

### <a name="the-clouddrive-mount-command"></a>A `clouddrive mount` parancs

> [!NOTE]
> Ha egy új fájlmegosztást most csatlakoztatni, új felhasználói lemezkép hoz létre a `$Home` könyvtár, mert az előző `$Home` kép tartják a korábbi fájlmegosztásban.

Futtassa a `clouddrive mount` parancsot a következő paraméterekkel:

```
clouddrive mount -s mySubscription -g myRG -n storageAccountName -f fileShareName
```

További részletek megtekintéséhez futtassa `clouddrive mount -h`, ahogy az itt látható:

![Fut a "clouddrive mount'command](media/mount-h.png)

## <a name="unmount-clouddrive"></a>Válassza le a lemezképet`clouddrive`
Egy fájlmegosztást, hogy bármikor a felhő rendszerhéj csatlakoztatott is leválasztása. Ha a fájlmegosztás nem csatlakoztatott, kérni fogja a következő munkamenet előtt egy új fájlmegosztást csatlakoztatni.

Fájlmegosztás eltávolítása felhő rendszerhéj:
1. Futtassa az `clouddrive unmount` parancsot.
2. Igazolja, és erősítse meg a megjelenő utasításokat.

A fájlmegosztás továbbra is létezik, kivéve, ha manuálisan törölni. Felhő rendszerhéj program már nem keres a fájlmegosztást a további munkamenetekhez.

További részletek megtekintéséhez futtassa `clouddrive unmount -h`, ahogy az itt látható:

![Fut a "clouddrive unmount'command](media/unmount-h.png)

> [!WARNING]
> A parancs futtatása minden olyan erőforrásnál nem törli. Egy erőforráscsoport, a tárfiókhoz vagy a felhő rendszerhéj leképezett fájlmegosztás törlésével véglegesen törli a `$Home` directory lemezképet és egyéb fájlokat a fájlmegosztásban. Ez a művelet nem vonható vissza.

## <a name="list-clouddrive-file-shares"></a>Lista `clouddrive` fájlmegosztások
Derítsen fel, melyik fájlmegosztás csatlakoztatva van, mint `clouddrive`, futtassa a következő `df` parancsot. 

A fájl elérési útját clouddrive jeleníti meg a tárfiók nevét és az URL-címben fájlmegosztás. Például: `//storageaccountname.file.core.windows.net/filesharename`

```
justin@Azure:~$ df
Filesystem                                               1K-blocks     Used Available Use% Mounted on
overlay                                                   30428648 15585636  14826628  52% /
tmpfs                                                       986704        0    986704   0% /dev
tmpfs                                                       986704        0    986704   0% /sys/fs/cgroup
/dev/sda1                                                 30428648 15585636  14826628  52% /etc/hosts
shm                                                          65536        0     65536   0% /dev/shm
//mystoragename.file.core.windows.net/fileshareName        6291456  5242944   1048512  84% /usr/justin/clouddrive
/dev/loop0                                                 5160576   601652   4296780  13% /home/justin
```

## <a name="transfer-local-files-to-cloud-shell"></a>Helyi fájlok átvitele a felhő rendszerhéj
A `clouddrive` directory szinkronizál az Azure portál tárolás panel. Ezen a panelen vihet át a helyi fájlokat vagy a fájlmegosztásból. Felhő rendszerhéj található fájlokat frissítése is megjelenik a file storage grafikus felhasználói Felülettel amikor frissíteni a panelt.

### <a name="download-files"></a>Fájlok letöltése

![A helyi fájlok listája](media/download.png)
1. Az Azure-portálon lépjen a csatlakoztatott fájlmegosztáshoz.
2. Válassza ki a cél-fájlt.
3. Válassza ki a **letöltése** gombra.

### <a name="upload-files"></a>Fájlok feltöltése

![Fel kell tölteni a helyi fájlok](media/upload.png)
1. Nyissa meg a csatlakoztatott fájlmegosztáshoz.
2. Válassza ki a **feltöltése** gombra.
3. Válassza ki a fájl vagy a feltölteni kívánt fájlokat.
4. Erősítse meg a feltöltést.

Most látnia kell, hogy elérhető-e a fájlokat a `clouddrive` felhő rendszerhéj könyvtárába.

## <a name="next-steps"></a>Következő lépések
[Felhő rendszerhéj gyors üzembe helyezés](quickstart.md) <br>
[További tudnivalók az Azure File storage](https://docs.microsoft.com/azure/storage/storage-introduction#file-storage) <br>
[További információk a tárolási címkék](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) <br>
