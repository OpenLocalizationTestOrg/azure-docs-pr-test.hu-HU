---
title: "aaaUsing Emulator Express toorun és hibakeresése az Azure felhőalapú szolgáltatás a helyi számítógépen |} Microsoft Docs"
description: "Helyi gépen toorun Emulator Express és a hibakeresési egy felhőalapú szolgáltatás használata"
services: visual-studio-online
documentationcenter: n/a
author: kraigb
manager: ghogen
editor: 
ms.assetid: 73108f98-a552-4817-b7a1-551367b71906
ms.service: visual-studio-online
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/06/2017
ms.author: kraigb
ms.openlocfilehash: d89a0fc2dce42b4a2d2f93f9c4ff0482af924ad4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-emulator-express-toorun-and-debug-an-azure-cloud-service-on-a-local-machine"></a>Emulator Express használatával toorun és hibakeresése az Azure felhőalapú szolgáltatás a helyi számítógépen
Emulator Express használatával tesztelése, és egy felhőalapú szolgáltatás hibakeresése a Visual Studio futtatása rendszergazdaként nélkül. Beállíthatja a projekt beállítások toouse vagy Emulator Express vagy hello teljes emulátor, attól függően, hogy a felhőszolgáltatás hello követelményeinek. Hello teljes emulátor kapcsolatos további információkért lásd: [egy Azure-alkalmazás futtatását a Compute Emulator hello](storage/common/storage-use-emulator.md).

## <a name="using-emulator-express-in-visual-studio"></a>A Visual Studio Emulator Express használatával
Az Azure SDK 2.3 vagy újabb Azure-projekt létrehozásakor automatikusan Emulator Express szolgál. Egy korábbi hello Azure SDK-val létrehozott meglévő projekteket használja a következő lépéseket tooselect Emulator Express hello:

1. Hozzon létre, vagy a Visual Studio Azure cloud service projekt megnyitása.

1. A **Megoldáskezelőben**, kattintson a jobb gombbal a projekt hello és, hello helyi menüből válassza ki a **tulajdonságok**.

1. Hello projektek tulajdonságai lapon válassza ki a hello **webes** fülre.

    ![Egy Azure-felhőszolgáltatás-projekt tulajdonságai](./media/vs-azure-tools-emulator-express-debug-run/web-properties.png)

1. A **helyi fejlesztési kiszolgáló**, jelölje be **beállítás használatát az IIS Express**.

1. A **emulátor**, jelölje be **használata Emulator Express**.
   
1. toolaunch hello Emulator Express, futtassa a következő parancsot a parancssorba hello: 

    ```
    csrun.exe /useemulatorexpress
    ```

## <a name="emulator-express-limitations"></a>Emulator Express korlátozásai
a következő problémák hello ismert korlátozásai Emulator Express: 

- Emulator Express nem kompatibilis az IIS-webkiszolgáló.
- A felhőalapú szolgáltatás több szerepkört is tartalmazhat, de minden egyes szerepkör korlátozott tooone példány.
- Portszámok 1000 alatt nem érhető el. Ha használ olyan hitelesítésszolgáltatója, amely általában az alábbi 1000 portot használ, szükség lehet toochange ezen érték tooa portszám, amely 1000 fent.
- Azure Compute Emulator toohello vonatkozó korlátozások is érvényesek tooEmulator Express. Például nem lehet üzemelő példányonként legfeljebb 50 szerepkörpéldányokat. Hello Azure Compute Emulator kapcsolatos további információkért lásd: [egy Azure-alkalmazás futtatását a Compute Emulator hello](http://go.microsoft.com/fwlink/p/?LinkId=623050).

## <a name="next-steps"></a>Következő lépések
[Hibakeresési Azure cloud services csomag](https://msdn.microsoft.com/library/azure/ee405479.aspx)
