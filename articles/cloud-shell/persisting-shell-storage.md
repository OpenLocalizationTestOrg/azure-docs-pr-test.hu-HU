---
title: "Azure Cloud rendszerhéj (előzetes verzió) aaaPersist-fájlok |} Microsoft Docs"
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
ms.openlocfilehash: b230453d5551c545553d63559741950206e4f1b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="persist-files-in-azure-cloud-shell"></a>Azure Cloud rendszerhéj fájlok megőrzése
Felhő rendszerhéj kihasználja az Azure File storage toopersist fájlok munkamenetei között.

## <a name="set-up-a-clouddrive-file-share"></a>Állítson be egy `clouddrive` fájlmegosztás
A kezdeti kezdőlapon felhő rendszerhéj kéri egy új vagy meglévő fájlmegosztás toopersist fájlok között munkamenetek tooassociate.

### <a name="create-new-storage"></a>Új tároló létrehozása

Ha alapszintű beállításokat alkalmazza, és válassza ki az előfizetés csak, felhő rendszerhéj az Ön nevében hello támogatott régióban legközelebbi tooyou hoz létre három erőforrásokat:
* Erőforráscsoport:`cloud-shell-storage-<region>`
* Storage-fiók:`cs<uniqueGuid>`
* Fájlmegosztás:`cs-<user>-<domain>-com-<uniqueGuid>`

![hello előfizetési beállítás](media/basic-storage.png)

hello fájlmegosztás csatlakoztatja `clouddrive` a a `$Home` könyvtár. hello fájlmegosztás egyben használt toostore egy 5 GB-os lemezképnek, amely automatikusan jön létre, és frissíti, és továbbra is fennáll a `$Home` könyvtár. Ez egy egyszeri művelet, és hello fájlmegosztás automatikusan csatlakoztatja a további munkamenetekhez.

### <a name="use-existing-resources"></a>Létező erőforrásokból

Speciális beállítás hello segítségével társíthatja a meglévő erőforrásokat. Amikor hello tárolási telepítő parancssor jelenik meg, válasszon **megjelenítése speciális beállítások** tooview további beállításokat. Meglévő fájlmegosztások kap egy 5 GB-os felhasználói lemezkép toopersist a `$Home` könyvtár. hello legördülő menükben helyi redundáns & georedundáns storage-fiókok és a felhő rendszerhéj régió szűrve.

![hello erőforrás tárolócsoport-beállítás](media/advanced-storage.png)

### <a name="restrict-resource-creation-with-an-azure-resource-policy"></a>Erőforrás létrehozása az Azure-erőforrás házirendjével korlátozása
A felhő rendszerhéj létrehozott tárfiókok vannak látva `ms-resource-usage:azure-cloud-shell`. Ha azt szeretné, hogy a storage-fiókok létrehozása a felhő rendszerhéj toodisallow felhasználóit, hozzon létre egy [Azure-erőforrás házirend címkék](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-policy-tags) , amely váltja ki ezt a címkét.

## <a name="how-cloud-shell-works"></a>Felhő rendszerhéj működése
Felhő rendszerhéj továbbra is fennáll, mind a következő módszerek hello keresztül fájlokat:
* A lemez lemezkép létrehozása a `$Home` directory toopersist összes hello könyvtár tartalmát. hello lemezképét mentette: a megadott fájlmegosztással `acc_<User>.img` : `fileshare.storage.windows.net/fileshare/.cloudconsole/acc_<User>.img`, és automatikusan szinkronizálja a módosításokat.

* A megadott fájlmegosztással csatlakoztatása `clouddrive` a a `$Home` való közvetlen fájl megosztási interakció könyvtárába. `/Home/<User>/clouddrive`túl van leképezve`fileshare.storage.windows.net/fileshare`.
 
> [!NOTE]
> Az összes fájlt a `$Home` címtár, például az SSH-kulcsokat, a lemez a csatlakoztatott fájl megosztáson található felhasználói lemezkép megmaradnak. Gyakorlati tanácsok alkalmazza, ha az adatokat továbbra is fennáll a `$Home` directory és a csatlakoztatott fájlmegosztás.

## <a name="use-hello-clouddrive-command"></a>Használjon hello `clouddrive` parancs
Felhő rendszerhéj is futtathatja egy olyan parancsot `clouddrive`, amely lehetővé teszi, hogy toomanually frissítés hello fájlmegosztás meg csatlakoztatott tooCloud rendszerhéj.
![Hello "clouddrive" parancs futtatása](media/clouddrive-h.png)

## <a name="mount-a-new-clouddrive"></a>Egy új csatlakoztatása`clouddrive`

### <a name="prerequisites-for-manual-mounting"></a>Manuális csatlakoztatásának előfeltételei
Frissítheti az hello fájlmegosztás hello segítségével felhő rendszerhéj társított `clouddrive mount` parancsot.

Ha egy meglévő fájlmegosztást csatlakoztatja, hello tárfiókok kell lennie:
* Helyileg redundáns tárolás vagy georedundáns toosupport tárolófájl-megosztások.
* A hozzárendelt régióban található. Amikor bevezetése, hello régió rendelt hello erőforráscsoport-név szerepel toois `cloud-shell-storage-<region>`.

### <a name="supported-storage-regions"></a>Támogatott tárolási régiók
hello az Azure files kell lennie, hello azonos régióban gépként hello felhő rendszerhéj, hogy van-e csatlakoztatni őket. Felhő rendszerhéj fürtök jelenleg szerepel a következő régiókban hello:
|Terület|Régió|
|---|---|
|Amerika|USA keleti régiója, USA déli középső RÉGIÓJA, USA nyugati régiója|
|Európa|Észak-Európa, Nyugat-Európa|
|Ázsia és a Csendes-óceáni térség|India központi, Délkelet-Ázsia|

### <a name="hello-clouddrive-mount-command"></a>Hello `clouddrive mount` parancs

> [!NOTE]
> Ha egy új fájlmegosztást most csatlakoztatni, új felhasználói lemezkép hoz létre a `$Home` könyvtár, mert az előző `$Home` kép tartják hello előző fájlmegosztásban.

Futtassa a hello `clouddrive mount` hello paraméterek a következő parancsot:

```
clouddrive mount -s mySubscription -g myRG -n storageAccountName -f fileShareName
```

tooview további információkért futtassa `clouddrive mount -h`, ahogy az itt látható:

![Futó hello "clouddrive mount'command](media/mount-h.png)

## <a name="unmount-clouddrive"></a>Válassza le a lemezképet`clouddrive`
Egy fájlmegosztás meg csatlakoztatott tooCloud rendszerhéj bármikor is leválasztása. Ha a fájlmegosztás nem csatlakoztatott, fogja felszólító toomount egy új fájl megosztási előzetes tooyour következő munkamenet.

tooremove fájl megosztása a felhőalapú rendszerhéjból:
1. Futtassa az `clouddrive unmount` parancsot.
2. Megerősíti, és erősítse meg a hello megjelenő utasításokat.

A fájlmegosztás tooexist folytatódik, kivéve, ha manuálisan törölni. Felhő rendszerhéj program már nem keres a fájlmegosztást a további munkamenetekhez.

tooview további információkért futtassa `clouddrive unmount -h`, ahogy az itt látható:

![Futó hello "clouddrive unmount'command](media/unmount-h.png)

> [!WARNING]
> A parancs futtatása minden olyan erőforrásnál nem törli. Egy erőforráscsoport, a tárfiókhoz vagy a fájlmegosztással rendelkezik, amely törlésével leképezve tooCloud rendszerhéj véglegesen törli a `$Home` directory lemezképet és egyéb fájlokat a fájlmegosztásban. Ez a művelet nem vonható vissza.

## <a name="list-clouddrive-file-shares"></a>Lista `clouddrive` fájlmegosztások
mely fájlmegosztás csatlakoztatva van, mint a toodiscover `clouddrive`, futtassa a következő hello `df` parancsot. 

hello fájl elérési útja tooclouddrive jeleníti meg a tárfiók nevét és fájlmegosztás hello URL-címben. Például: `//storageaccountname.file.core.windows.net/filesharename`

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

## <a name="transfer-local-files-toocloud-shell"></a>Átviteli helyi fájlok tooCloud rendszerhéj
Hello `clouddrive` hello Azure portál tárolás panel directory szinkronizál. Ezen a panelen tootransfer helyi fájlok tooor a fájlmegosztásból. Felhő rendszerhéj található fájlokat frissítése tükrözi hello a file storage grafikus felhasználói Felülettel hello panel frissítésekor.

### <a name="download-files"></a>Fájlok letöltése

![A helyi fájlok listája](media/download.png)
1. A hello Azure-portálon válassza a toohello csatlakoztatott fájlmegosztást.
2. Válassza ki a hello cél fájlt.
3. Jelölje be hello **letöltése** gombra.

### <a name="upload-files"></a>Fájlok feltöltése

![Helyi fájlok toobe feltöltve.](media/upload.png)
1. Nyissa meg tooyour csatlakoztatott fájlmegosztást.
2. Jelölje be hello **feltöltése** gombra.
3. Jelölje be hello vagy fájl, amelyet az tooupload.
4. Erősítse meg a hello feltöltése.

Most látnia kell, hogy elérhető-e hello fájlok a `clouddrive` felhő rendszerhéj könyvtárába.

## <a name="next-steps"></a>Következő lépések
[Felhő rendszerhéj gyors üzembe helyezés](quickstart.md) <br>
[További tudnivalók az Azure File storage](https://docs.microsoft.com/azure/storage/storage-introduction#file-storage) <br>
[További információk a tárolási címkék](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) <br>
