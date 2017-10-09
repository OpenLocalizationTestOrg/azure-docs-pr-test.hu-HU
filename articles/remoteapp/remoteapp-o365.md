---
title: az Azure RemoteApp Office aaaUsing |} Microsoft Docs
description: "Ismerje meg, hogy Office és az Azure RemoteApp együttműködni"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: f1773baf-8aa1-423c-a2f9-e4679e0463d3
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: d065c1a0a2191c9e2e405264a835cecf791d6ab7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-office-with-azure-remoteapp"></a>Az Office és az Azure RemoteApp használata
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.
> 
> 

Az Office-alkalmazások az Azure Remoteappban üzemeltetési két lehetősége van: Office 365 ProPlus vagy Office 2013 Professional Plus próbaverziója.

**Hát tudja vezetünk be egy új, cikket, amely hamarosan lecseréli a jobb? Tekintse meg [hogyan toouse az Office 365-előfizetéshez az Azure RemoteApp](remoteapp-officesubscription.md). Az Office 365 + az Azure RemoteApp használata szükséges összes hello információ magában foglalja.**

## <a name="office-365-proplus"></a>Az Office 365 ProPlus
Létrehozhat egy RemoteApp-gyűjtemény hello Office 365 ProPlus sablon rendszerképet használja. Ez a beállítás lehetővé teszi tooextend az Office 365 szolgáltatás tooRemoteApp. Rendelkeznie kell egy meglévő előfizetési csomagot, és a felhasználóknak pedig licenccel kell rendelkezniük az Office 365 ProPlus szolgáltatás, vagy önálló hello vagy hello Office 365 service-csomagokról.

RemoteApp támogatja az Office 365 megosztott számítógép aktiválása. Ha megosztott számítógépet aktiválási engedélyezi, és hello használata [Office-telepítő eszköz](http://www.microsoft.com/download/details.aspx?id=36778) nélkül aktiválandó telepíti Office 365 ProPlus, a telepítéshez. Amikor egy felhasználó jelentkezik be egy gyűjteményt, amely tartalmazza az Office 365, az Office ellenőrzi, hogy hello felhasználói az Office 365 ProPlus van kiépítve toosee. Ha igen, Office ideiglenesen aktiválja Office 365 ProPlus - a az aktiválás továbbra is fennáll, amíg a felhasználók jeleket kívüli hello szolgáltatás meg.

Office 365 megosztott számítógép aktiválási toouse, kell toocreate egy [egyéni sablon](remoteapp-create-custom-image.md) és az Office 365 ProPlus telepítése, a következő [ezt az útmutatót](https://technet.microsoft.com/library/dn782858.aspx).

Kezelheti a felhasználók Office 365 licencek: hello [Office 365 felügyeleti portál](https://portal.office365.com/). További információt olvashat [Office 365 service-csomagokról](http://technet.microsoft.com/library/office-365-plan-options.aspx).  

## <a name="office-2013-professional-plus-trial"></a>Office 2013 Professional Plus próbaverzió
A RemoteApp 30 napos próbaverziót, során hello Office 2013 Professional Plus (próbaverzió) sablon kép toocreate egy RemoteApp-gyűjtemény is használhatja. Felhasználók toothis próba adatgyűjtést az Azure Active Directory munkahelyi fiókok vagy a Microsoft-fiókok rendelhet hozzá. Nincsenek további előfizetési szükség.

Ez egy nagyszerű lehetőséget tookick hello gumikat, és egy jó érzelmek felkeltésére szolgálnak az Office a RemoteApp első. Ez a beállítás azonban szánt értékelési és tesztelési csak. RemoteApp-gyűjtemények hello Office 2013 Professional Plus (próbaverzió) sablon rendszerképet használatával létrehozott állapotra váltott tooproduction módban nem lehet, és hello próbaidőszak végén hello le lesz tiltva.

## <a name="switching-from-trial-tooproduction"></a>Váltás a próba tooproduction
A 30 napos ingyenes próbaverzióval indításakor hello hello portal RemoteApp szakasza a Megjegyzés megtudhatja, mennyi ideig kilépett hello kipróbált előtt fiók fizetett tootransition tooa van szüksége. Ez a Megjegyzés hello hivatkozással a fiók és a kapcsoló tooproduction mód aktiválhatja.

A fiók aktiválása, ez hatással lesz a összes hello RemoteApp-gyűjteményére fiókját.

* Gyűjtemények, amelyeken a Windows Server 2012 R2 hello vagy Office 365 ProPlus sablonrendszerképek hello zökkenőmentesen ismerhetik tooproduction. Minden felhasználói adatokat és beállításokat, beleértve a folyamatban lévő munkamenetek változatlanok maradnak.
* Ha egyéni sablon rendszerképek feltöltött, ezeket a képeket használatával gyűjtemények is fog zökkenőmentesen átmenet.
* csak tesztelési készült hello Office 2013 Professional Plus (próbaverzió) sablon rendszerképet. A sablon lemezképe futtató gyűjtemények nem lehet tooproduction állapotra váltott. A "Letiltva" állapotba kerülnek.

Ha nem az tooproduction üzemmód átmenet a a próbaidőszak lejárta hello által, a RemoteApp-gyűjtemények letiltásra kerül. Ne aggódjon, mert a beállítások és a felhasználói adatok menti egy másik 90 nap, így továbbra is aktiválhatja a szolgáltatás és a kapcsoló tooproduction módot, az adatvesztés nélkül.

