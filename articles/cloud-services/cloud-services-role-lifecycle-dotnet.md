---
title: "aaaHandle Felhőszolgáltatás életciklus-események |} Microsoft Docs"
description: "Ismerje meg, hogyan hello életciklusának egy felhőalapú szolgáltatás szerepkör metódus használható a .NET"
services: cloud-services
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: 39b30acd-57b9-48b7-a7c4-40ea3430e451
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: adegeo
ms.openlocfilehash: cc0ccc5f055b965202b6e081a6ab72ad5d39b034
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="customize-hello-lifecycle-of-a-web-or-worker-role-in-net"></a>A .NET webes vagy feldolgozói szerepkör életciklus hello testreszabása
A feldolgozói szerepkör létrehozásakor kiterjesztheti a hello [RoleEntryPoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx) osztály, amely metódusokat biztosít az Ön toooverride, amelyek lehetővé teszik események toolifecycle válaszol. A webes szerepkörök Ez az osztály nem kötelező, így toorespond toolifecycle események kell használni.

## <a name="extend-hello-roleentrypoint-class"></a>Hello RoleEntryPoint osztály
Hello [RoleEntryPoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx) osztály tartalmazza a módszereket, amelyek az Azure által meghívott amikor azt **elindul**, **fut**, vagy **leállítja** egy webes vagy feldolgozói szerepkörrel. Ezen módszerek toomanage szerepkör inicializálása, szerepkör leállítási sorozatok vagy hello végrehajtási szállal hello szerepkör szükség lehet felülbírálni. 

Amikor bővíti **RoleEntryPoint**, akkor érdemes figyelembe a következő viselkedés hello módszerek hello:

* Hello [OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) és [OnStop](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx) módszerek egy logikai értéket adja vissza, ezért lehetséges tooreturn **hamis** az ezen módszerek.
  
   Ha a kódot adja vissza **hamis**, hello szerepkör folyamat váratlanul megszakadt, bármely leállítási folyamat futtatása nélkül előfordulhat, hogy teljesülnek. Általában kerülendő visszaadó **hamis** a hello **OnStart** metódust.
* A nem kezelt kivétel belül egy túlterhelése egy **RoleEntryPoint** metódus nem kezelt kivételt a rendszer.
  
   Ha kivétel hello életciklus módszerek belül előfordul, a Azure emeli hello [UnhandledException](https://msdn.microsoft.com/library/system.appdomain.unhandledexception.aspx) esemény, és ezután hello folyamat megszakadt. A szerepkör offline állapotban van, után újraindul az Azure-ban. Ha nem kezelt kivétel történik, hello [leállítása](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.stopping.aspx) esemény nem következik be, és hello **OnStop** metódus nem lett meghívva.

Ha a szerepkör nem indul el, vagy az újrahasznosítás hello inicializálása, foglalt, és leállítása állapotok közötti lehet, hogy a kód kiváltása hello életciklus-események idő hello szerepkörönként újraindul egy nem kezelt kivétel. Ebben az esetben használja a hello [UnhandledException](https://msdn.microsoft.com/library/system.appdomain.unhandledexception.aspx) esemény toodetermine hello hello kivétel okát, és megfelelően kezelje. A szerepkör is előfordulhat, hogy visszatérése hello [futtatása](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metódus, amely azt eredményezi, hello szerepkör toorestart. További információ a központi telepítési állapotok: [gyakori problémák amely ok szerepkörök tooRecycle](cloud-services-troubleshoot-common-issues-which-cause-roles-recycle.md).

> [!NOTE]
> Hello használata **Azure eszközök a Microsoft Visual Studio** toodevelop az alkalmazás a hello szerepkör projekt sablonok automatikus kiterjesztése hello **RoleEntryPoint** osztály, a hello *WebRole.cs* és *WorkerRole.cs* fájlokat.
> 
> 

## <a name="onstart-method"></a>OnStart metódus
Hello **OnStart** metódus lehívásra kerül, amikor a szerepkör példánya online állapotba az Azure-ban. Hello OnStart kód végrehajtása folyik, amíg hello szerepkörpéldányt jelölésű **foglalt** és nem a külső forgalom lesz hello terheléselosztó által vezérelt tooit. Felülírhatja a metódus tooperform inicializálási munka, például eseménykezelők végrehajtási és elindítása [Azure Diagnostics](cloud-services-how-to-monitor.md).

Ha **OnStart** adja vissza **igaz**, hello példány sikeresen inicializálva, és meghívja a Azure hello **RoleEntryPoint.Run** metódust. Ha **OnStart** adja vissza **hamis**, hello szerepkör leállítása azonnal, minden tervezett leállás sorozatok végrehajtása nélkül.

a következő kód példa azt mutatja meg hogyan hello toooverride hello **OnStart** metódust. Ez a módszer konfigurálja, és egy diagnosztikai figyelő akkor kezdődik, amikor elindul és naplózási adatok tooa tárfiók átruházása beállítása a hello szerepkör példánya:

```csharp
public override bool OnStart()
{
    var config = DiagnosticMonitor.GetDefaultInitialConfiguration();

    config.DiagnosticInfrastructureLogs.ScheduledTransferLogLevelFilter = LogLevel.Error;
    config.DiagnosticInfrastructureLogs.ScheduledTransferPeriod = TimeSpan.FromMinutes(5);

    DiagnosticMonitor.Start("DiagnosticsConnectionString", config);

    return true;
}
```

## <a name="onstop-method"></a>OnStop módszer
Hello **OnStop** módszer neve után a szerepkörpéldány offline állapotban van az Azure-ban, és mielőtt hello folyamat kilép. Ez a szerepkör-példány toocleanly, állítsa le a szükséges mód toocall kódot felülbírálható.

> [!IMPORTANT]
> Hello futó **OnStop** metódus egy korlátozott ideig toofinish rendelkezik, ennek oka nem egy felhasználó által kezdeményezett leállítást meghívásakor. Ezen idő eltelte után hello folyamat megszakadt, ezért meg kell győződnie arról, hogy a kód a hello **OnStop** metódus nem fut toocompletion eltűr vagy gyorsan futtathatók. Hello **OnStop** módszer neve után hello **leállítása** egy esemény jelenik meg.
> 
> 

## <a name="run-method"></a>Run metódus
Felülbírálhatja hello **futtatása** metódus tooimplement a hosszan futó szál a szerepkör-példányhoz.

Hello felülbírálása **futtatása** metódus nincs szükség; hello alapértelmezett implementációja elindítja a szál, amelyek örökre alvó állapotban van. Ha hello felülbírálása **futtatása** metódus, a kódot kell blokkolása határozatlan ideig. Ha hello **futtatása** metódus ad vissza, hello szerepkör automatikusan szabályosan újrahasznosított; más szóval Azure riasztást hello **leállítása** esemény és hívások hello **OnStop** módszer, a leállítási sorozatok hello szerepkör előtt is végrehajthat offline állapotba kerül.

### <a name="implementing-hello-aspnet-lifecycle-methods-for-a-web-role"></a>A webes szerepkör végrehajtási hello ASP.NET életciklus módszerek
Módszerek használhatók a hello ASP.NET életciklusát, továbbá a toothose hello által biztosított **RoleEntryPoint** osztály, toomanage inicializálása és leállítási sorozatok egy webes szerepkör. Ez kompatibilitási célokra hasznos lehet, ha meg van egy meglévő ASP.NET alkalmazás tooAzure eljárás. hello ASP.NET életciklus módszerek is meghívhatók hello **RoleEntryPoint** módszerek. Hello **alkalmazás\_Start** módszer neve után hello **RoleEntryPoint.OnStart** metódus befejeződik. Hello **alkalmazás\_End** módszer neve előtt hello **RoleEntryPoint.OnStop** metódust.

## <a name="next-steps"></a>Következő lépések
Ismerje meg, hogyan túl[cloud service-csomag létrehozása](cloud-services-model-and-package.md).

