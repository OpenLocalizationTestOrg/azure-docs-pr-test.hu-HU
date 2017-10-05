---
title: "Egyéni lemezkép feltöltése az Azure Remoteappban |} Microsoft Docs"
description: "Útmutató: az Azure RemoteApp egyéni lemezkép feltöltése"
services: remoteapp
documentationcenter: 
author: ericorman
manager: mbaldwin
ms.assetid: 299e0510-1a6b-4fdf-914a-3631b061a360
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: ericor
ms.openlocfilehash: 5a235fac88d6e95ea294bda197641108acb4a09f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="upload-a-custom-image-for-azure-remoteapp"></a>Az Azure RemoteApp egyéni lemezkép feltöltése
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. A részletekért olvassa el a [bejelentést](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Most, hogy létrehozta az egyéni sablon rendszerképet, vagy frissítette a változások, kell, hogy a Rendszerkép feltöltése az Azure RemoteApp Képtár. Kövesse az alábbi lépéseket.

## <a name="before-you-start"></a>Előkészületek
1. Ellenőrizze az egyéni lemezképet megfelel-e a [képre vonatkozó követelmények](remoteapp-imagereqs.md) és [alkalmazáskövetelményeket](remoteapp-appreqs.md).
2. Telepítse a [Azure PowerShell modul](/powershell/azure/overview).

## <a name="step-by-step-on-how-to-upload-custom-image"></a>Lépésről lépésre kapcsolatos egyéni lemezkép feltöltése
1. Nyissa meg az Azure felügyeleti portálon, és nyissa meg a RemoteApp lapot.
2. Az a **sablonrendszerképek** lapra, majd **feltöltése** az oldal alján.
3. Adjon egy rövid nevet a lemezkép számára, és adja meg a tárfiók helyének. Győződjön meg arról a hely és a RemoteApp-gyűjteménnyel ugyanazon a helyen vagy egy helyet, ahová hozzon létre egyet.
4. Amikor a rendszer kéri, töltse le a parancsfájlt a helyi számítógépen.
5. A parancs paraméterei a szövegmezőben másolása a vágólapra.
6. Nyisson meg egy rendszergazda jogú Windows PowerShell-ablakban.
7. Emelt szintű Windows PowerShell ablakában keresse meg az azonos könyvtárat, amelybe letöltötte a parancsfájl.
8. Illessze be a másolt parancs, és nyomja le az **Enter**.
   
   A feltöltési folyamat akkor kezdődik, és időtartama függhet számos tényező közé tartoznak többek között a hálózati sebesség és a lemezkép mérete
9. Ha a feltöltés nem sikerül miatt hálózati megszakítás és dolgot hasonlóan, mindig folytathatja a feltöltési folyamat megkezdése. Feltöltés folytatásához futtassa a parancsfájlt, az azonos parancssorban segítségével újból.

> [!WARNING]
> Soha ne módosítsa a feltöltési parancsfájlt. Annak érdekében, hogy a kép megfelel-e a lemezkép-követelmények és alkalmazáskövetelmények valósul ellenőrzésekkel.
> 
> 

## <a name="common-problems"></a>Gyakori problémák
* Győződjön meg arról, hogy a Windows PowerShell, nem az Azure PowerShell használata. Szeretné telepíteni az Azure PowerShell-modult, mert bizonyos modulok van szükség a feltöltési folyamat során.
* Soha ne módosítsa a parancsprogram, érvényesítést vannak-e a felhasználók kényelme érdekében.
* Ha a vhd-fájl feltöltése során lekérdezi zárolva, másolja a fájlt, vagy újra áthelyezése egy új helyre, és megpróbál feltöltése. Előfordulhat, hogy egy Windows-folyamat, amely megakadályozza az feltöltése.  

