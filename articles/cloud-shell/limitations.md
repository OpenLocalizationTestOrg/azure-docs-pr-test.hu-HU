---
title: "aaaAzure felhő rendszerhéj (előzetes verzió) korlátozások |} Microsoft Docs"
description: "Azure Cloud rendszerhéj vonatkozó korlátozások áttekintése"
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
ms.openlocfilehash: 8462b0b9850fcde790a386433009439bbab52c0f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="limitations-of-azure-cloud-shell"></a>Azure-felhőbe rendszerhéj korlátozásai
Azure Cloud rendszerhéj rendelkezik hello következő ismert korlátozásai:

## <a name="system-state-and-persistence"></a>Rendszerállapot- és adatmegőrzési
a felhő rendszerhéj munkamenet biztosító hello gép csak átmenetileg létezik, és újrahasznosítása után a munkamenet a 20 percig inaktív. Felhő rendszerhéj egy fájl megosztási toobe csatlakoztatva van szükség. Ennek eredményeképpen az előfizetés mentése a tárolási erőforrások tooaccess felhő rendszerhéj képes tooset kell lennie. Egyéb szempontok a következők:
* Csatlakoztatott tárolóval, csak azokat a módosításokat, belül a `$Home` könyvtár vagy `clouddrive` directory megmaradnak.
* Fájlmegosztások csak belülről lehet csatlakoztatni az [hozzárendelve régió](persisting-shell-storage.md#mount-a-new-clouddrive).
* Az Azure Files csak a helyileg redundáns tárolás és a georedundáns tárolás fiókokat támogatja.

## <a name="user-permissions"></a>Felhasználói engedélyek
Engedélyek vannak beállítva, normál felhasználóként sudo hozzáférés nélkül. Bármely telepítési kívül a `$Home` könyvtár nem fogja megőrizni.
Bár az egyes parancsok belül hello `clouddrive` könyvtárába, például `git clone`, nem rendelkezik megfelelő engedélyekkel a `$Home` directory engedélye.

## <a name="browser-support"></a>Webböngésző támogatása
Felhő rendszerhéj támogatja a Microsoft Edge, a Microsoft Internet Explorer, a Google Chrome, a Mozilla Firefox és az Apple Safari legújabb verziói hello. Safari privát üzemmódban nem támogatott.

## <a name="copy-and-paste"></a>Másolás és beillesztés
CTRL + C és a Ctrl + V, a másolás/beillesztés parancsikonok felhő rendszerhéj Windows gépeken, használja a Ctrl + Insert és a Shift + Insert toocopy és beillesztés, illetve nem működnek.

Kattintson a jobb gombbal a másolás és beillesztés beállítások, de kattintson a jobb gombbal függvény tulajdonos toobrowser-specifikus vágólap hozzáférésének.

## <a name="editing-bashrc"></a>.Bashrc szerkesztése
Szerkesztés .bashrc, így váratlan hibákat okozhat felhő rendszerhéj elvégzendő járjon el.

## <a name="bashhistory"></a>.bash_history
Lehet, hogy az előzmények bash parancsok felhő rendszerhéj munkamenet megszakítása vagy egyidejű munkamenetek miatt inkonzisztens.

## <a name="usage-limits"></a>Használati korlátok
Felhő rendszerhéj készült interaktív használati eseteket. Ennek eredményeképpen a hosszan futó nem interaktív munkamenet befejeződik figyelmeztetés nélkül.

## <a name="network-connectivity"></a>Hálózati kapcsolat
Felhő rendszerhéj bármely késés tulajdonos toolocal internetkapcsolattal, a felhő rendszerhéj bármely küldött utasítások végrehajtásával kimenő tooattempt toocarry továbbra is.

## <a name="next-steps"></a>Következő lépések
[Felhő rendszerhéj gyors üzembe helyezés](quickstart.md)
