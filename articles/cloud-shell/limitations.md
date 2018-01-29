---
title: "Azure Cloud rendszerhéj-korlátozások |} Microsoft Docs"
description: "Azure Cloud rendszerhéj vonatkozó korlátozások áttekintése"
services: azure
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
ms.date: 01/17/2018
ms.author: juluk
ms.openlocfilehash: 7e498582d78d2807070c943dfd838dd9efeb4ed2
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/18/2018
---
# <a name="limitations-of-azure-cloud-shell"></a>Azure-felhőbe rendszerhéj korlátozásai

Azure Cloud rendszerhéj ismert korlátai a következők:

## <a name="general-limitations"></a>Általános korlátozások

### <a name="system-state-and-persistence"></a>Rendszerállapot- és adatmegőrzési

A felhő rendszerhéj munkamenet biztosító a gép csak átmenetileg létezik, és újrahasznosítása után a munkamenet a 20 percig inaktív. Felhő rendszerhéj csatlakoztatni kell egy Azure fájlmegosztás szükséges. Ennek eredményeképpen az előfizetés kell tudni tárolási erőforrások beállítása felhő rendszerhéj eléréséhez. Egyéb szempontok a következők:

* Csatlakoztatott tárolóval, csak azokat a módosításokat, belül a `clouddrive` directory megmaradnak. A Bash a `$Home` directory is megőrződjenek.
* Azure fájlmegosztásokat csak belülről lehet csatlakoztatni az [hozzárendelve régió](persisting-shell-storage.md#mount-a-new-clouddrive).
  * A Bash, futtassa az `env` állítja be a régióban található `ACC_LOCATION`.
* Az Azure Files csak a helyileg redundáns tárolás és a georedundáns tárolás fiókokat támogatja.

### <a name="browser-support"></a>Webböngésző támogatása

Felhő rendszerhéj a Microsoft Edge, a Microsoft Internet Explorer, a Google Chrome, a Mozilla Firefox és az Apple Safari legújabb verzióit támogatja. Safari privát üzemmódban nem támogatott.

### <a name="copy-and-paste"></a>Másolás és beillesztés

[!include [copy-paste](../../includes/cloud-shell-copy-paste.md)]

### <a name="for-a-given-user-only-one-shell-can-be-active"></a>A megadott felhasználó csak egy rendszerhéj lehet aktív

Felhasználók is csak elindíthatja egy adott típusú rendszerhéj egyszerre, vagy **Bash** vagy **PowerShell**. Azonban előfordulhat, hogy Bash vagy a PowerShell fut egyszerre több példányát. Csere Bash vagy a PowerShell okok felhő rendszerhéj újraindítására, amely megszakítja a meglévő munkamenetek között.

### <a name="usage-limits"></a>Használati korlátozások

Felhő rendszerhéj készült interaktív használati eseteket. Ennek eredményeképpen a hosszan futó nem interaktív munkamenet befejeződik figyelmeztetés nélkül.

## <a name="bash-limitations"></a>Bash korlátozásai

### <a name="user-permissions"></a>Felhasználói engedélyek

Engedélyek vannak beállítva, normál felhasználóként sudo hozzáférés nélkül. Bármely telepítési kívül a `$Home` könyvtár nem őrzi meg.

### <a name="clouddrive-smb-limited-permissions"></a>Clouddrive SMB korlátozott engedélyekkel
Bizonyos parancsok belül a `clouddrive` könyvtárába, például `git clone`, nem rendelkezik megfelelő engedélyekkel bizonyos fájlok írható/olvasható. Ha elérte ezt a problémát, próbálja újra a a `$Home` könyvtárat, amelyhez nem tartozik az SMB-korlátozások.

### <a name="editing-bashrc"></a>.Bashrc szerkesztése

Szerkesztés .bashrc, így váratlan hibákat okozhat felhő rendszerhéj elvégzendő járjon el.

### <a name="bashhistory"></a>.bash_history

Lehet, hogy az előzmények bash parancsok felhő rendszerhéj munkamenet megszakítása vagy egyidejű munkamenetek miatt inkonzisztens.

## <a name="powershell-limitations"></a>PowerShell-korlátozások

### <a name="slow-startup-time"></a>Lassú indítási idő

Azure Cloud rendszerhéj (előzetes verzió) PowerShell előzetes inicializálása 60 másodperc is beletelhet.

### <a name="no-home-directory-persistence"></a>No $Home directory adatmegőrzési

Írt adatok `$Home` bármely alkalmazás (például: git, vim, megint mások) nem maradnak PowerShell-munkamenetek között. Megkerülő megoldásért [talál](troubleshooting.md#powershell-resolutions).

### <a name="default-file-location-when-created-from-azure-drive"></a>Alapértelmezett helye az Azure-meghajtóról létrehozásakor:

PowerShell-parancsmagok használatával felhasználók nem hozható létre az Azure meghajtón található fájl. Amikor a felhasználók más eszközökkel, például vim vagy nano, új fájlok létrehozása a fájlok kerülnek C:\Users mappa alapértelmezés szerint. 

### <a name="gui-applications-are-not-supported"></a>Grafikus felhasználói Felülettel alkalmazások nem támogatottak.

Ha a felhasználó hozna létre például egy Windows párbeszédpanelen parancsot futtatja `Connect-AzureAD` vagy `Login-AzureRMAccount`, egy üzenetet egy hiba például: `Unable to load DLL 'IEFRAME.dll': The specified module could not be found. (Exception from HRESULT: 0x8007007E)`.

## <a name="next-steps"></a>További lépések

[Hibaelhárítási felhő rendszerhéj](troubleshooting.md) <br>
[Rövid útmutató a Bash-hez](quickstart.md) <br>
[Rövid útmutató a PowerShellhez](quickstart-powershell.md)
