---
title: "aaaUpload egyéni lemezképként az Azure Remoteappban |} Microsoft Docs"
description: "Ismerje meg, hogyan tooupload egyéni rendszerképet az Azure RemoteApp"
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
ms.openlocfilehash: 6ad40fe58795ece37f4c7900be01bc713938da87
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upload-a-custom-image-for-azure-remoteapp"></a>Az Azure RemoteApp egyéni lemezkép feltöltése
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.
> 
> 

Most, hogy létrehozta az egyéni sablon rendszerképet, vagy frissítette a változások, kell tooupload adott kép tooyour Azure RemoteApp Képtár. Kövesse az alábbi lépéseket.

## <a name="before-you-start"></a>Előkészületek
1. Ellenőrizze az egyéni lemezképet megfelel-e hello [képre vonatkozó követelmények](remoteapp-imagereqs.md) és [alkalmazáskövetelményeket](remoteapp-appreqs.md).
2. Telepítse a hello [Azure PowerShell modul](/powershell/azure/overview).

## <a name="step-by-step-on-how-tooupload-custom-image"></a>Lépésről lépésre arról, hogy hogyan tooupload egyéni kép
1. Nyissa meg az Azure felügyeleti portálon, és keresse meg a toohello RemoteApp lap.
2. A hello **sablonrendszerképek** lapra, majd **feltöltése** hello lap hello alján.
3. Adjon egy rövid nevet a lemezkép számára, és adja meg a hello tárfiókhely. Győződjön meg arról, hello helye hello és a RemoteApp-gyűjtemény vagy egy helyre, ahol egy toocreate ugyanazon a helyen.
4. Amikor a rendszer kéri, töltse le a hello parancsfájl tooyour helyi számítógépen.
5. Hello parancsparaméterek hello szöveg mezőben tooyour vágólapra másolása.
6. Nyisson meg egy rendszergazda jogú Windows PowerShell-ablakban.
7. A hello emelt szintű Windows PowerShell-ablakot, keresse meg a toohello azonos könyvtárat, amelybe letöltötte hello parancsfájl.
8. Beillesztés hello másolt parancs, és nyomja le az **Enter**.
   
   hello feltöltési folyamat akkor kezdődik, és időtartama függhet számos tényező közé tartoznak többek között a hálózati sebesség és a hello lemezkép mérete
9. Ha a feltöltés nem sikerül miatt hálózati megszakítás és dolgot hasonlóan, mindig folytathatja hello feltöltési folyamat megkezdése. tooresume feltöltés, újra hello parancsprogrammal hello azonos parancssorban.

> [!WARNING]
> Soha ne módosítsa a hello feltöltés parancsfájlt. Egyes ellenőrzéséről, hogy a kép megfelel-e hello kép követelményeknek és alkalmazáskövetelmények hello megvalósított tooensure törölték.
> 
> 

## <a name="common-problems"></a>Gyakori problémák
* Győződjön meg arról, hogy a Windows PowerShell, nem az Azure PowerShell használata. Meg kell tooinstall hello Azure PowerShell modul, mivel egyes modulok hello feltöltési folyamat során van szükség.
* Soha ne módosítsa a hello parancsfájl, érvényesítést vannak-e a felhasználók kényelme érdekében.
* Ha hello vhd-fájl feltöltése során lekérdezi zárolva, hello másolás, vagy helyezze tooa új helyre, és megpróbál feltöltése újra. Előfordulhat, hogy egy Windows-folyamat, amely megakadályozza az feltöltése.  

