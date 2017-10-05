---
title: "Azure RemoteApp alkalmazás követelményei |} Microsoft Docs"
description: "Tudnivalók az Azure Remoteappban használni kívánt alkalmazások követelményeit:"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 4427eef6-288a-49e1-97eb-fee67d99f26a
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 13d42df97ea2b090180f5865a4eac25945f9f34c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="app-requirements"></a>Alkalmazáskövetelmények
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. A részletekért olvassa el a [bejelentést](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Az Azure RemoteApp támogatja az adatfolyam-továbbítási 32 bites vagy 64 bites Windows-alapú alkalmazások a Windows Server 2012 R2-lemezkép. A legtöbb meglévő 32 bites vagy 64 bites Windows-alapú alkalmazások futnak, "adott állapotban" az Azure RemoteApp (távoli asztali szolgáltatások vagy a korábban a Terminálszolgáltatások néven ismert) környezetben. Azonban fut, és működését között eltérés van – egyes alkalmazások megfelelően működni, és hajtsa végre, míg mások azonban nem. A következő információkat nyújt útmutatást a távoli asztali szolgáltatások környezetben futó alkalmazások fejlesztéséhez és teszteléséhez kompatibilitás-ellenőrzése.

Tipp: Dolgozunk, működő néhány olyan alkalmazások létrehozásával meg. Új témakörök ismertetik, Microsoft Access QuickBooks és App-V használatával RemoteApp láthatja.

## <a name="requirements"></a>Követelmények
Három követelménynek, ha követi, segítséget az alkalmazás jól RemoteApp fut:

1. Alkalmazások, amelyek minden [Windows asztali alkalmazások esetén a Hardvertanúsítvány követelményeit](https://msdn.microsoft.com/library/windows/desktop/hh749939.aspx) és követi a [irányelvek programozási távoli asztali szolgáltatások](https://msdn.microsoft.com/library/aa383490.aspx) RemoteApp teljes kompatibilitás fog rendelkezni.
2. Alkalmazások kell soha nem tárolnak adatokat helyileg a lemezkép vagy elvesznek, RemoteApp-példány.  A RemoteApp-gyűjtemény létrehozása után a példányok klónozva vannak állapot nélküli és alkalmazások csak tartalmaznia kell. Külső forrás vagy a felhasználó profiljában adatok tárolására.
3. Egyéni lemezképek soha nem kell tartalmaznia, amely akkor szakadhat meg adatok.  

## <a name="testing-your-apps"></a>Az alkalmazások tesztelése
Használja az alkalmazások tesztelése az alábbi lépéseket:

1. Windows Server 2012 R2 és az alkalmazás telepítése
2. A Távoli asztal engedélyezése
3. Hozzon létre két felhasználói fiókokat, a "a" felhasználó és a "b" felhasználó, a távoli asztal biztonsági csoportba való felvételekor mindkét felhasználói fiókot.
4. Több munkamenet kompatibilitás-ellenőrzés létrehozásával a két egyidejű távoli asztali szolgáltatások munkamenet támadóknak a Számítógéphez az alkalmazás indítása közben.
5. Ellenőrizze az alkalmazás viselkedését

## <a name="application-development-guidelines"></a>Alkalmazás fejlesztői útmutató
Használja az alábbi útmutatást a RemoteApp-alkalmazások fejlesztésével.

### <a name="multiple-users"></a>Több felhasználó
* Telepítés egy [alkalmazás egy felhasználó ](https://msdn.microsoft.com/library/aa380661.aspx)problémákat okozhat az többfelhasználós környezetben.
* Alkalmazások kell [kapcsolatos információkat tárolja](https://msdn.microsoft.com/library/aa383452.aspx) felhasználóspecifikus helyen, elkülönítve a globális adatokat az összes felhasználóra érvényes.
* RemoteApp használja több [kernel objektumok névterek](https://msdn.microsoft.com/library/aa382954.aspx); a globális névtér elsősorban az ügyfél/kiszolgáló alkalmazások services használja.
* Már nem biztonságos feltételeztük, hogy a számítógép nevét vagy a [IP-cím](https://msdn.microsoft.com/library/aa382942.aspx) a számítógéphez rendelt társítva egy-egy felhasználóhoz, mert több felhasználó is kell bejelentkeznie egyszerre egy távoli asztali munkamenetgazda (távoli asztali munkamenetgazda) kiszolgálóhoz.

### <a name="performance"></a>Teljesítmény
* Tiltsa le a [grafikus hatások](https://msdn.microsoft.com/library/aa380822.aspx) csak az alkalmazás hozzáadni a RemoteApp.
* Minden felhasználó CPU rendelkezésre állásának maximalizálását, vagy tiltsa le a [feladatok háttérben ](https://msdn.microsoft.com/library/aa380665.aspx) , vagy hozzon létre, amelyek nincsenek erőforrás-igényes feladatok hatékony háttérben.
* Hangolja és alkalmazás egyensúlyba kell [használati szál](https://msdn.microsoft.com/library/aa383520.aspx) többfelhasználós, többprocesszoros környezethez.
* A teljesítmény optimalizálása érdekében ajánlott az alkalmazások [észlelése](https://msdn.microsoft.com/library/aa380798.aspx) e futtatnak, az ügyfél.

