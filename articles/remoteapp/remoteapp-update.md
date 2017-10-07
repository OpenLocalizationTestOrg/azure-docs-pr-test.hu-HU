---
title: "aaaUpdate az Azure RemoteApp-gyűjteménnyel |} Microsoft Docs"
description: "Megtudhatja, hogyan tooupdate Azure RemoteApp-gyűjteménye"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
editor: 
ms.assetid: e553d432-e581-48fe-b996-c432357eb64a
ms.service: remoteapp
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: compute
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 849d7abfdfad4dbe6a235d2a28c71f7943eb812c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="update-a-collection-in-azure-remoteapp"></a>Az Azure Remoteappban egy gyűjtemény frissítéséhez
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.
> 
> 

Egy időpontot, mindenképpen, ha van szükség tooupdate hello alkalmazások vagy lemezkép az Azure RemoteApp-gyűjteménnyel fog érkezni. Használata az Azure RemoteApp előfizetés felhőalapú vagy hibrid gyűjtemény hello rendszerképeket egyik minden frissítések kezeli Azure RemoteApp magát, így könnyen hagyhatja.

Azonban ha egy egyéni lemezképet (vagy teljesen új beépített, vagy a lemezképek módosításával létrehozott) használ, is feladata hello rendszerkép és az alkalmazások karbantartása. Ha a lemezkép vagy azon belül hello alkalmazások bármelyikét tooupdate kell, kell toocreate hello lemezképet, majd cserélje le hello meglévő lemezkép új, frissített verziójának új frissített lemezképpel a gyűjteményben.

Igen hogyan továbblépne: a gyűjtemény frissítése? Meglehetősen egyszerű:

1. A gyűjteményben használt lemezképet hello frissítése. Bármely javítások vagy a szükséges frissítéseket alkalmazza, és mentse az új nevet.
2. [Töltse fel](remoteapp-uploadimage.md) vagy [importálása](remoteapp-image-on-azurevm.md) adott kép tooRemoteApp.
3. Most hello gyűjtemény oldalon kattintson **frissítés**.
4. Új kép hello választhat hello **sablonlemezkép** listája.
5. Ide tartozik hello legbonyolultabb - toodecide hogyan kell azokat a felhasználókat, hello gyűjtemény jelenleg használ egy alkalmazást a toodeal. Hello a következő lehetőségek közül választhat:
   
   * **Hello frissítés utáni a 60 perc haladék a felhasználóknak**. Amint hello frissítése befejeződött, az Azure RemoteApp egy üzenet tooany aktív felhasználók kapnak a munkahelyi és a napló ki, és jelentkezzen be oda toosave jeleníti meg. 60 perc után nem jelentkezik ki, aktív felhasználók automatikusan ki lesz léptetve. Felhasználók közvetlenül jelentkezhetnek be újra.
   * **Felhasználók azonnali kijelentkeztetése**. Amint hello frissítése befejeződött, jelentkezzen ki minden felhasználó figyelmeztetés nélkül automatikusan. Ha ezt a lehetőséget választja, a felhasználók adatok elveszhetnek. Azonban ezeket újra csatlakozhat toohello alkalmazás azonnal.
6. Kattintson a hello pipa toostart hello frissítés.

