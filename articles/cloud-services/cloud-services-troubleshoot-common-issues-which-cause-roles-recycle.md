---
title: "aaaCommon okai Felhőszolgáltatási szerepkörök újrahasznosítása |} Microsoft Docs"
description: "A felhőalapú szolgáltatás egyik szerepköre, hirtelen újraindul jelentős állásidőt okozhat. Az alábbiakban néhány gyakori problémákat okozhat a szerepkörök toobe felhasználását, amelyek segítségével csökkentheti az állásidőt."
services: cloud-services
documentationcenter: 
author: simonxjx
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: 533930d1-8035-4402-b16a-cf887b2c4f85
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 7/26/2017
ms.author: v-six
ms.openlocfilehash: 8fa152b33d2b22a8a02f834d4bc38519b4272f7b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="common-issues-that-cause-roles-toorecycle"></a>Gyakori hibák, melyek a szerepkörök toorecycle
Ez a cikk ismerteti az egyes hello jellemző okai az alkalmazástelepítéssel kapcsolatos problémák és toohelp hibaelhárítási tippek a probléma megoldásához. Arra utal, hogy probléma van egy alkalmazás esetén hello szerepkörpéldányt toostart sikertelen lesz, vagy azt ciklusok hello inicializálása, foglalt és leállítása állapotok között.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="missing-runtime-dependencies"></a>Hiányzó futásidejű függőségek
Ha az alkalmazás szerepkör támaszkodik, amely nem része a hello szerelvények .NET-keretrendszer vagy hello Azure felügyelt kódtárára, explicit módon szerepelnie kell, hogy a szerelvény hello alkalmazáscsomagot. Ne feledje, hogy más Microsoft-keretrendszer nem érhető el az Azure-on alapértelmezés szerint. Ha a szerepkör ilyen keretrendszer támaszkodik, hozzá kell adni ezeket szerelvények toohello alkalmazáscsomagot.

Mielőtt felépítéséhez és az alkalmazás becsomagolása, ellenőrizze a hello következőket:

* Ha a Visual studio segítségével, győződjön meg arról, hogy hello **másolása helyi** tulajdonsága túl**igaz** minden hivatkozott szerelvény, amely nem része a hello Azure SDK célverzióját, vagy hello .NET-keretrendszer.
* Győződjön meg arról, hogy a web.config fájl hello nem hivatkozik hello fordítási elemben nem használt szerelvényeket.
* Hello **Build művelet** minden .cshtml a fájl túl van beállítva**tartalom**. Ez biztosítja, hogy hello fájlok hello csomag megfelelően jelenik-e, és lehetővé teszi, hogy más fájlok hivatkozott tooappear hello csomagban.

## <a name="assembly-targets-wrong-platform"></a>Szerelvény célok nem megfelelő platformon
Azure egy 64 bites környezetben. Emiatt Azure .NET-szerelvényt a 32 bites cél számára nem fog működni.

## <a name="role-throws-unhandled-exceptions-while-initializing-or-stopping"></a>Szerepkör nem kezelt kivételek inicializálása vagy leállítása közben jelez.
Hello hello módszerek által okozott kivételek [RoleEntryPoint] osztály, amely tartalmazza a hello [OnStart], [OnStop], és [futtatása]módszereket, nem kezelt kivételek. Ha nem kezelt kivétel lép fel, ezek közül, hello szerepkör indul újra. Ha ismételten újrahasznosítása hello szerepkör, előfordulhat, hogy kell egy nem kezelt kivétel kiváltása minden alkalommal, amikor megkísérli toostart.

## <a name="role-returns-from-run-method"></a>Szerepkör vissza Run metódus
Hello [futtatása] metódus tervezett toorun határozatlan ideig. Ha a kód felülírja hello [futtatása] metódust, akkor kell alvó határozatlan ideig. Ha hello [futtatása] metódus ad vissza, hello szerepkör újraindul.

## <a name="incorrect-diagnosticsconnectionstring-setting"></a>Helytelen DiagnosticsConnectionString beállítása
Ha az alkalmazás Azure Diagnostics használja, a szolgáltatás konfigurációs fájlja hello kell megadnia `DiagnosticsConnectionString` konfigurációs beállítás. Ez a beállítás adjon meg egy HTTPS-kapcsolat tooyour storage-fiókot az Azure-ban.

tooensure, amely a `DiagnosticsConnectionString` érték helyes-e az alkalmazás csomag tooAzure központi telepítése előtt, ellenőrizze a következőket hello:  

* Hello `DiagnosticsConnectionString` tooa érvényes tárfiókot pontok beállítása az Azure-ban.  
  Alapértelmezés szerint ez a beállítás, explicit módon kell változtatni ezt a beállítást, mielőtt a alkalmazáscsomag telepítése mutat emulált toohello storage-fiók esetén. Ha nem módosítja ezt a beállítást, a rendszer kivételt hoz létre, amikor hello szerepkörpéldányt próbál toostart hello diagnosztikai figyelő. Emiatt a hello szerepkör példány toorecycle határozatlan ideig.
* hello kapcsolati karakterláncban megadott hello következő [formátum](../storage/common/storage-configure-connection-string.md). (protokoll hello meg kell adni, HTTPS.) Cserélje le *MyAccountName* hello nevet a tárfiók, és *MyAccountKey* a hozzáférési kulccsal:    

        DefaultEndpointsProtocol=https;AccountName=MyAccountName;AccountKey=MyAccountKey

  Ha az alkalmazás Azure-eszközök a Microsoft Visual Studio használatával fejleszt, használható hello tulajdonság lapok tooset ezt az értéket.

## <a name="exported-certificate-does-not-include-private-key"></a>Exportált tanúsítvány nem tartalmazza a titkos kulcs
a webes szerepkör alatt SSL toorun, győződjön meg arról, hogy saját exportált felügyeleti tanúsítványát tartalmazza-e a titkos kulcs hello. Hello használatakor *Windows tanúsítványkezelő* tooexport hello tanúsítvány, lehet, hogy tooselect **Igen** a hello **hello titkos kulcs exportálása** lehetőséget. hello PFX formátumban hello csak formátum jelenleg támogatott hello tanúsítványt kell exportálni.

## <a name="next-steps"></a>Következő lépések
További [cikkek hibaelhárítási](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) felhőszolgáltatásai számára.

Megtekintheti a további szerepkörök újrahasznosítása a következő forgatókönyvek [Kevin Williamson blog adatsorozat](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).

[RoleEntryPoint]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx
[OnStart]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx
[OnStop]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx
[futtatása]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx
