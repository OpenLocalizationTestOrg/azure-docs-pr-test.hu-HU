---
title: "Azure RemoteApp aaaApp követelményei |} Microsoft Docs"
description: "Hello az alkalmazások követelményeit, amelyet az Azure Remoteappban toouse megismerése"
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
ms.openlocfilehash: 3fa2bcdaab457a6fbee8ac52a81d1c4154bbdce1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="app-requirements"></a>Alkalmazáskövetelmények
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.
> 
> 

Az Azure RemoteApp támogatja az adatfolyam-továbbítási 32 bites vagy 64 bites Windows-alapú alkalmazások a Windows Server 2012 R2-lemezkép. A legtöbb meglévő 32 bites vagy 64 bites Windows-alapú alkalmazások futnak, "adott állapotban" az Azure RemoteApp (távoli asztali szolgáltatások vagy a korábban a Terminálszolgáltatások néven ismert) környezetben. Azonban fut, és működését között eltérés van – egyes alkalmazások megfelelően működni, és hajtsa végre, míg mások azonban nem. a következő információk hello nyújt útmutatást a távoli asztali szolgáltatások környezetben futó alkalmazások fejlesztéséhez és teszteléséhez tooensure kompatibilitási.

Tipp: Dolgozunk, működő néhány olyan alkalmazások létrehozásával meg. Új témakörök ismertetik, Microsoft Access QuickBooks és App-V használatával RemoteApp láthatja.

## <a name="requirements"></a>Követelmények
Három követelménynek, ha követi, segítséget az alkalmazás jól RemoteApp fut:

1. Alkalmazások, amelyek minden [Windows asztali alkalmazások esetén a Hardvertanúsítvány követelményeit](https://msdn.microsoft.com/library/windows/desktop/hh749939.aspx) és követi túl[irányelvek programozási távoli asztali szolgáltatások](https://msdn.microsoft.com/library/aa383490.aspx) RemoteApp teljes kompatibilitás fog rendelkezni.
2. Alkalmazások kell soha nem tárolnak adatokat helyileg hello lemezkép vagy a RemoteApp-példányok elveszhet.  A RemoteApp-gyűjtemény létrehozása után hello példányok klónozva vannak állapot nélküli és alkalmazások csak tartalmaznia kell. A külső forrásból vagy hello felhasználó profiljában adatok tárolására.
3. Egyéni lemezképek soha nem kell tartalmaznia, amely akkor szakadhat meg adatok.  

## <a name="testing-your-apps"></a>Az alkalmazások tesztelése
Ezen lépések tootesting alkalmazásokat használnak:

1. Windows Server 2012 R2 és az alkalmazás telepítése
2. A Távoli asztal engedélyezése
3. Hozzon létre két felhasználói fiókokat, a "a" felhasználó és a "b" felhasználó, mindkét felhasználói fiókok toohello távoli asztal biztonsági csoport hozzáadása.
4. Több munkamenet kompatibilitás-ellenőrzés létrehozásával a két egyidejű távoli asztali munkamenetek toohello PC hello alkalmazás indítása közben.
5. Ellenőrizze az alkalmazás viselkedését

## <a name="application-development-guidelines"></a>Alkalmazás fejlesztői útmutató
A következő útmutatást a RemoteApp-alkalmazások fejlesztésével hello használata.

### <a name="multiple-users"></a>Több felhasználó
* Telepítés egy [alkalmazás egy felhasználó ](https://msdn.microsoft.com/library/aa380661.aspx)problémákat okozhat az többfelhasználós környezetben.
* Alkalmazások kell [kapcsolatos információkat tárolja](https://msdn.microsoft.com/library/aa383452.aspx) felhasználóspecifikus helyen, külön-külön a globális adatokat, amelyek érvényes tooall felhasználók.
* RemoteApp használja több [kernel objektumok névterek](https://msdn.microsoft.com/library/aa382954.aspx); a globális névtér elsősorban az ügyfél/kiszolgáló alkalmazások services használja.
* Nincs biztonságos tooassume, amely a számítógép nevét vagy hello hello [IP-cím](https://msdn.microsoft.com/library/aa382942.aspx) hozzárendelt toohello számítógép nincsenek társítva van egy-egy felhasználóhoz, mert több felhasználó egyidejűleg tooa távoli asztali munkamenetgazda (távoli asztali munkamenet kell bejelentkeznie Gazdagép)-kiszolgálón.

### <a name="performance"></a>Teljesítmény
* Tiltsa le a [grafikus hatások](https://msdn.microsoft.com/library/aa380822.aspx) az alkalmazás tooRemoteApp hozzáadása előtt.
* vagy tiltsa le a toomaximize CPU rendelkezésre állása, a felhasználók [feladatok háttérben ](https://msdn.microsoft.com/library/aa380665.aspx) , vagy hozzon létre, amelyek nincsenek erőforrás-igényes feladatok hatékony háttér.
* Hangolja és alkalmazás egyensúlyba kell [használati szál](https://msdn.microsoft.com/library/aa383520.aspx) többfelhasználós, többprocesszoros környezethez.
* toooptimize teljesítmény érdekében ajánlott az alkalmazások túl[észlelése](https://msdn.microsoft.com/library/aa380798.aspx) e futtatnak, az ügyfél.

