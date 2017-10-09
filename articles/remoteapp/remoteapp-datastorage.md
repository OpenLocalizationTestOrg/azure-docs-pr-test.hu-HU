---
title: "aaaNever tárolóban lévő érzékeny adatokat az Azure RemoteApp egyéni lemezképek |} Microsoft Docs"
description: "Tudnivalók az adatok tárolását az Azure Remoteappban egyéni lemezképek hello:"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 5a19903b-15f9-49d9-9bc1-ae80f2671aa1
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
ms.openlocfilehash: 86a0fea218f8826d6d25f50d3c4c36e368e26fb5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="never-store-sensitive-data-on-custom-images"></a>Soha ne tároljon bizalmas adatokat az egyéni lemezképek
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.
> 
> 

Ha az alkalmazását, az Azure Remoteappban, hello első lépése toocreate egy egyéni lemezképet. A kép: egyéni toocreate Virtuálisgép-példányok, amely az alkalmazások kiszolgálására tooyour felhasználók használjuk. hello egyéni lemezképet csak alkalmazások és a nem bizalmas adatok elvesznek, például az SQL-adatbázisok, személyzet fájlok vagy különleges adatfájlok QuickBooks vállalati fájlok például kell tartalmaznia. Az összes bizalmas adatokat kell tárolni a külső tooAzure RemoteApp egy fájlkiszolgálón, egy másik Azure virtuális Gépen, vagy az SQL Azure-ban. hello kép csak hello gazdaalkalmazást toohello adatforráshoz kapcsolódik, és megadja a hello adatok kell. Felülvizsgálati [Azure RemoteApp-lemezképek követelményei](remoteapp-imagereqs.md) további információt. 

Miért nem tárolja bizalmas adatok toounderstand Azure RemoteApp működése toounderstand van szüksége. Ha a gyűjtemény létrehozásakor vagy frissítésekor, hello háttérben több klónok vagy hello kép egy példánya jönnek létre. A Virtuálisgép-példányok másolatai pontos hello egyéni lemezkép; Amikor felhasználók alkalmazásokat a Virtuálisgép-példányok csatlakoztatott tooone. De hello példányt nem garantált, és nem kell számít, mivel azok nem állandó. hello Virtuálisgép-példányok üzemeltetési hello az alkalmazások nem állandó és lehet megsemmisítették, vagy törölték a alapú, például gyűjtemény frissítése közben. 

Miután hello gyűjtemény ki van építve, és elindíthatók csatlakozó toohello virtuális gépeket, a felhasználói adatok állandó és védelme, mert a Mentés belül tartalmazó virtuális merevlemez nevezzük különálló tárhelyet a egy [felhasználói profil lemezre (UPD)](remoteapp-upd.md), vagyis hello felhasználói profil a c:\users\<userprofile >. Az alkalmazás indításakor hello UPD csatlakozik és hasonlóan a helyi felhasználói profil hello operációs rendszer által kezelni. További információk [Azure RemoteApp menti a felhasználói adatok és beállítások](remoteapp-upd.md).

Példa adatokat kell hello kép nem található:

* A felhasználók tooaccess megosztott adatok
* SQL-adatbázis vagy a QuickBooks DB
* Egyetlen megadott adattal sem D:\

Példa adatok hello alapértelmezett profil toobe átmásolja az összes felhasználói UPD is található:

* Felhasználói szintű konfigurációs fájlok
* Olyan parancsfájlok, amelyek lenne szükség a felhasználók saját UPD megőrzi

Kulcs mutat:

* Soha nem tárolnak a bizalmas adatokat, amelyek egyéni lemezkép létrehozásakor hello rendszerképre elveszett.
* Bizalmas adatok mindig kell egy különálló fájlkiszolgálón található, Azure virtuális gép, hello felhőhöz, és az Azure RemoteApp alkalmazást üzemeltető mindig külső toohello Virtuálisgép-példány külön. 
* Felhasználói adatok menti, és továbbra is fennáll, a hello felhasználói profil lemezre (UPD)

