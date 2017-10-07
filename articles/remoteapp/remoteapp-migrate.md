---
title: "aaaMigrate felhasználói adatokat az Azure RemoteApp |} Microsoft Docs"
description: "Megtudhatja, hogyan toomigrate mindkét Azure RemoteApp felhasználói adatokat."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: d7e4fbf1-cb42-4430-94a0-ed6d4676fc86
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: aefc6ccc2c6173754acf6cad06102f27c8cb1d26
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomigrate-data-into-and-out-of-azure-remoteapp"></a>Hogyan toomigrate adatok esetében bejövő és kimenő Azure RemoteApp
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.
> 
> 

Számos különböző eszközöket és módszerek tootransfer [felhasználói adatok](remoteapp-upd.md) esetében bejövő és kimenő Azure RemoteApp. Az alábbiakban néhány módszert:

* Másolja és illessze be a használatával a vágólap megosztása
* Másolja a fájlokat és adatokat tooa fájlkiszolgáló
* Másolja a fájlokat tooOneDrive vállalati böngészőn keresztül
* Másolja a fájlokat a átirányítása

> [!NOTE]
> Nem engedélyezte a onedrive vállalati verzió hello üzleti vagy fogyasztói sync-ügynök - azok [használata nem támogatott](remoteapp-onedrive.md) az Azure Remoteappban.
> 
> 

## <a name="use-copy-and-paste-in-file-explorer"></a>Másolás és beillesztés a Fájlkezelőben
A RemoteApp-telepítések engedélyezve van a másolás és beillesztés hello vágólap segítségével [alapértelmezés szerint](remoteapp-redirection.md). Ez lehetővé teszi, hogy a felhasználók a helyi számítógép és a RemoteApp-alkalmazások közti fájlokat másolni. Gyakran hello normális működés során a RemoteApp-alkalmazások használata, a felhasználók mentett fájlokat tootheir felhasználóiprofil - áthelyezését, hogy könnyen-e RemoteApp adatokat:

1. [A Fájlkezelőben egy alkalmazás közzététele](remoteapp-publish.md) egy RemoteApp-gyűjteményben. (Vegye figyelembe, hogy ez a felügyeleti feladatot.)
2. Közvetlen a felhasználók toolaunch hello fájlkezelő alkalmazást közzétett és toouse adott toocopy és a Beillesztés fájlok azok UPD be- és belőle.

## <a name="upload-files-and-data-tooa-file-server-by-using-standard-network-file-copy"></a>Töltse fel a fájlokat és adatokat tooa fájlkiszolgáló szabványos hálózati fájlmásolat segítségével
Gyakran szervezetek fájl kiszolgálók toostore általános adatok felhasználásával. Ha tudja, hello kiszolgáló nevét vagy helyét, a felhasználók navigálhat hello helyi hálózati hello kiszolgáló és a fájlokat, majd másolja, sokkal megszokott módon fent. Újra lesz toopublish Fájlkezelőben tooRemoteApp szeretné és megosztása a felhasználók.

> [!NOTE]
> hello fájlkiszolgáló RemoteApp a telepített hello az irányítható hálózaton kell lennie.
> 
> 

## <a name="copy-files-tooonedrive-for-business"></a>A vállalati fájlok tooOneDrive másolása
Bár hello OneDrive vállalati szinkronizálási ügynök a RemoteApp nem engedélyezi, akkor továbbra is fájljait átmásolhatja a UPD tooOneDrive vállalati böngészőn keresztül. 

1. A Fájlkezelőben tooRemoteApp közzététele, és majd kérje meg a felhasználók tooaccess hello fájlok számára, hogy alkalmazáson keresztül. 
2. Legegyszerűbb tootransfer fájlokat, ha azok tömörített, így a felhasználók hozzon létre egy .zip fájlt, amely tartalmazza az összes hello fájlok toomove tooOneDrive vállalati.
3. Kérje meg a felhasználók toogo toohello Office 365 portálra, és folytassa a tooOneDrive és hello .zip fájl feltöltése.

## <a name="copy-files-by-using-drive-redirection"></a>Másolja a fájlokat átirányítás használatával
Ha engedélyezte a [meghajtó átirányítása](remoteapp-redirection.md), már létrehozott egy leképzett meghajtóról a felhasználók számára. Ebben az esetben azok a zip-fájljaikat a átirányítva hello meghajtó és mentse őket tootheir helyi számítógépen.

## <a name="how-administrators-can-export-data"></a>Hogyan rendszergazdák adatok exportálása

Az Azure RemoteApp exportálhatja az összes felhasználói profil lemezeket (UPD) felügyeli a összes gyűjteményt belül egy előfizetés tooAzure tárolást biztosít az Azure PowerShell-parancsmag Export-AzureRemoteAppUserDisk.  Nincs nem képes tooselect egyedi UPD meg.  Hello PowerShell-parancs végrehajtásakor minden felhasználó lemez fog egy rögzített méretű lemez mérete 50 GB-os és exportált tooAzure tároló.  Az Azure storage költségei azonnal tájékozódnia a tárolására.  Ellenőrizze a parancs futtatása hiányoznak nem munkamenetek, egyébként pedig hello exportálás sikertelen lesz.

UPD a tartományhoz csatlakoztatott Azure RemoteApp-telepítésekhez csak akkor használható újra egy távoli asztali szolgáltatások telepítésben, a tartományhoz nem csatlakoztatott központi telepítés nem használható.  Ha ezek a lemezek használ a távoli asztali szolgáltatások-telepítés toouse javasoljuk a [parancsfájlok automatikus](https://github.com/arcadiahlyy/aramigration) , exportálása, konvertálja, majd hello UPD meg importálni egy távoli asztali szolgáltatások környezetbe.

