---
title: "aaaMount Azure fájlmegosztás SMB-n keresztül macOS |} Microsoft Docs"
description: "Ismerje meg, hogyan toomount egy Azure-fájl megosztása SMB-n keresztül macOS."
services: storage
documentationcenter: 
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/27/2017
ms.author: renash
ms.openlocfilehash: 7b4924cb42247470521c1ae8b9d03ab1756996e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="mount-azure-file-share-over-smb-with-macos"></a>Azure-fájlmegosztás csatlakoztatása SMB protokoll segítségével macOS rendszeren
[Az Azure File storage](storage-dotnet-how-to-use-files.md) hello iparági szabvány használja a Microsoft-szolgáltatás, amely lehetővé teszi a hello Azure toocreate és -felhasználási hálózati fájlmegosztások. Az Azure-fájlmegosztások a macOS rendszer Sierra (10.12) és El Capitan (10.11) verzióra csatlakoztathatók. Ez a cikk mutatja be két különböző módon toomount egy Azure fájlmegosztás macOS hello kereső felhasználói felület és a Terminálszolgáltatások hello használatával.

> [!Note]  
> Az Azure-fájlmegosztás SMB protokollon keresztül történő csatlakoztatása előtt javasoljuk, hogy tiltsa le az SMB-csomagaláírást. Nem így előfordulhat, hogy yield gyenge teljesítményt, amikor macOS hello Azure fájlmegosztás eléréséhez. Az SMB-kapcsolatot lesz titkosítva, így ez nincs hatással a hello biztonsági a kapcsolat. A Terminálszolgáltatások hello hello következő parancsok letiltja a SMB-csomagokat, ez szerint [Apple támogatási cikk a letiltása az SMB-csomagokat](https://support.apple.com/HT205926):  
>    ```
>    sudo -s
>    echo "[default]" >> /etc/nsmb.conf
>    echo "signing_required=no" >> /etc/nsmb.conf
>    exit
>    ```

## <a name="prerequisites-for-mounting-an-azure-file-share-on-macos"></a>Az Azure-fájlmegosztások macOS rendszerre történő csatlakoztatásának előfeltételei
* **A tárfiók neve**: toomount egy Azure fájlmegosztás, akkor lesz szüksége hello hello tárfiókja nevére.

* **Tárfiók kulcsa**: toomount egy Azure fájlmegosztás, akkor lesz szüksége hello elsődleges (vagy másodlagos) storage-kulcs. Az SAS-kulcsokkal való csatlakoztatás jelenleg nem támogatott.

* **Győződjön meg arról, hogy a 445-ös port nyitva legyen**: Az SMB protokoll a 445-ös TCP porton keresztül kommunikál. Az ügyfélszámítógép (hello Mac) ellenőrizze, hogy a tűzfal nem blokkolja a 445-ös TCP-port toomake.

## <a name="mount-an-azure-file-share-via-finder"></a>Azure-fájlmegosztás csatlakoztatása a Finder segítségével
1. **Nyissa meg a keresési**: kereső macOS alapértelmezés szerint nyitva, de gondoskodhat hello jelenleg kiválasztott alkalmazás hello "macOS szembesülhetnek ikon" gombra kattintva hello dock:  
    ![hello macOS szembesülhetnek ikon](media/storage-file-how-to-use-files-mac/mount-via-finder-1.png)

2. **Válassza ki a "Csatlakozás tooServer" hello "Ugrás" menü a**: hello hello UNC-útvonalat használ [Előfeltételek](#preq), átalakítása hello kezdete dupla fordított perjel (`\\`) túl`smb://` és egyéb fordított (`\`) tooforwards perjeleket (`/`). A hivatkozás hello hasonlóan kell kinéznie: ![hello "Csatlakozás tooServer" párbeszédpanel](./media/storage-file-how-to-use-files-mac/mount-via-finder-2.png)

3. **Hello megosztás nevét és a tárolási fiók kulcs használata a felhasználónevet és jelszót kérő**: kattintson a "Csatlakozás" hello "Csatlakozás tooServer" párbeszédpanelen, amikor kérni fogja hello felhasználóneve és jelszava (Ez lesz a macOS rendelkező autopopulated username). Lehetősége van hello a helyezi el a macOS kulcslánc hello megosztás neve/tárfiók kulcsa.

4. **Használjon hello Azure fájlmegosztás tetszés szerint**: után és hello megosztás nevét és a tárolási fiók kulcsot a hello felhasználóneve és jelszava, hello megosztást is csatlakoztatva. Használhat ez történik, mint általában egy helyi fájl/mappa megosztáshoz, többek között a húzása hello fájlmegosztás be:

    ![Pillanatfelvétel egy csatlakoztatott Azure-fájlmegosztásról](./media/storage-file-how-to-use-files-mac/mount-via-finder-3.png)

## <a name="mount-an-azure-file-share-via-terminal"></a>Azure-fájlmegosztás csatlakoztatása a Terminál segítségével
1. Cserélje le `<storage-account-name>` hello a tárfiók nevére. Adja meg a tárfiók kulcsát, amikor a rendszer felkéri erre. 

    ```
    mount_smbfs //<storage-account-name>@<storage-account-name>.file.core.windows.net/<share-name> <desired-mount-point>
    ```

2. **Használjon hello Azure fájlmegosztás tetszés szerint**: hello Azure fájlmegosztás hello előző parancs által meghatározott hello csatlakozási ponton is csatlakoztatva.  

    ![Pillanatkép készítése a csatlakoztatott hello Azure fájlmegosztás](./media/storage-file-how-to-use-files-mac/mount-via-terminal-1.png)

## <a name="next-steps"></a>Következő lépések
Az alábbi hivatkozások további információkat tartalmaznak az Azure File Storage-ról.

* [Apple-támogatási cikk - hogyan tooconnect fájlmegosztás a Mac gépen](https://support.apple.com/HT204445)
* [Gyakori kérdések](storage-files-faq.md)
* [hibaelhárítással](storage-troubleshoot-file-connection-problems.md)