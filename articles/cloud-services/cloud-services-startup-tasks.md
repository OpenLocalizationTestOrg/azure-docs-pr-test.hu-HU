---
title: "Azure Cloud Services indítási feladatokat aaaRun |} Microsoft Docs"
description: "Indítási feladatok segítségével, az alkalmazás a cloud service-környezet előkészítéséhez. Ez útmutatást ad, hogy hogyan indítási munkahelyi feladatokat, és hogyan toomake őket"
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 886939be-4b5b-49cc-9a6e-2172e3c133e9
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: adegeo
ms.openlocfilehash: 3391a5d7434164f59972b8e497e5c34e33409543
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-and-run-startup-tasks-for-a-cloud-service"></a>Hogyan tooconfigure, és futtassa indítási feladatokat egy felhőalapú szolgáltatás
Használhatja az indítási feladatok tooperform műveletek szerepkör indítása előtt. Előfordulhat, hogy kívánt tooperform műveletek közé tartozik, egy összetevő telepítésével, COM-összetevők regisztrálása, beállításkulcsok vagy hosszú ideig futó folyamat elindítása.

> [!NOTE]
> Indítási feladatokat nem alkalmazható tooVirtual gépek, csak tooCloud szolgáltatás webes és feldolgozói szerepköröket is.
> 
> 

## <a name="how-startup-tasks-work"></a>Indítási feladatok működése
Indítási feladatokat is a szerepkörök kezdődik, és meghatározott hello előtt végzett műveleteket [ServiceDefinition.csdef] hello segítségével fájlt [feladat] hello elemének [indítási]elemet. Indítási feladatok gyakran parancsfájlokat, de ezek is lehetnek konzol alkalmazások, vagy indítsa el a PowerShell-parancsfájlok, kötegelt fájlok.

Környezeti változók adhatók át információk egy indítási tevékenységhez, és a helyi tároló lehet egy indítási tevékenységhez kívül használt toopass információkat. Egy környezeti változó megadhatja például, hello elérési tooa program tooinstall szeretne, valamint a fájlok írhatók tudja majd olvasni később a szerepkörök toolocal tárhelyet.

Az indítási tevékenységhez jelentkezhetnek, adatokat és hello által megadott könyvtárban található hibák toohello **TEMP** környezeti változó. Hello indítási feladat során hello **TEMP** környezeti változó, oldja fel toohello *C:\\erőforrások\\temp\\[guid]. [ rolename]\\RoleTemp* directory hello felhő futtatásakor.

Indítási feladatok is hajtható végre több alkalommal újraindításnál. Például hello indítási lesz futtatható a feladat minden alkalommal, amikor hello szerepkör újraindul, és a szerepkör újrahasznosítja azt mindig nem tartalmazhatnak újraindítás. Indítási feladatok oly módon, amely lehetővé tenné toorun problémamentesen többször kell írni.

Indítási feladatok végén szerepelnie kell egy **errorlevel** (vagy a kilépési kód) hello indítási folyamat toocomplete nulla. Ha egy indítási tevékenységhez végződik-e egy nem nulla **errorlevel**, hello szerepkör nem indul el.

## <a name="role-startup-order"></a>Szerepkör indítási sorrend
a következő listák hello szerepkör indítási eljárás az Azure-ban hello:

1. hello példány van megjelölve, **indítása** és nem kap a forgalmat.
2. Minden indítási feladatok végrehajtásának tootheir szerint **taskType** attribútum.
   
   * Hello **egyszerű** feladatok végrehajtásának szinkron módon történik, egyenként.
   * Hello **háttér** és **előtér** feladatokat is aszinkron módon elindítva, párhuzamos toohello indítási tevékenységhez.  
     
     > [!WARNING]
     > Az IIS nem teljesen konfigurálható hello indítási tevékenységhez szakasza hello indítási folyamat során, a szerepkör-specifikus adatok nem érhetők el. Indítási feladatokhoz szükséges szerepkör-specifikus adatok használandó [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx).
     > 
     > 
3. hello szerepkör gazdagép-folyamat elindul, és hello helyen jön létre az IIS-ben.
4. Hello [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) metódust.
5. hello példány van megjelölve, **készen** és forgalma irányított toohello példány.
6. Hello [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.Run](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metódust.

## <a name="example-of-a-startup-task"></a>Példa egy indítási tevékenységhez
Indítási feladatok meghatározott hello [ServiceDefinition.csdef] hello-fájlban **feladat** elemet. Hello **commandLine** attribútum hello nevét adja meg, és paraméterek hello indítási köteg fájlt, vagy konzol parancs, hello **executionContext** attribútum hello indítási hello jogosultság szintjét határozza meg. a feladat, és hello **taskType** attribútum meghatározza, hogy hello feladat végrehajtásának módját.

Ebben a példában, egy környezeti változó **MyVersionNumber**létrehozott hello indítási tevékenységhez, amely toohello érték "**1.0.0.0**".

**ServiceDefinition.csdef**:

```xml
<Startup>
    <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" >
        <Environment>
            <Variable name="MyVersionNumber" value="1.0.0.0" />
        </Environment>
    </Task>
</Startup>
```

A következő példa hello, hello **Startup.cmd** parancsfájlt ír hello sor toohello StartupLog.txt fájl "hello jelenlegi verzió: 1.0.0.0" hello TEMP környezeti változóban megadott hello könyvtárban. Hello `EXIT /B 0` sor biztosítja, hogy hello indítási tevékenységhez végződik egy **errorlevel** nulla.

```cmd
ECHO hello current version is %MyVersionNumber% >> "%TEMP%\StartupLog.txt" 2>&1
EXIT /B 0
```

> [!NOTE]
> A Visual Studio hello **tooOutput Directory másolása** tulajdonságot a indítási kötegelt fájl kell beállítani, túl**másolási mindig** toobe meg arról, hogy az indítási kötegelt fájl megfelelően van-e telepítve tooyour projektet az Azure-on (**approot\\bin** webes szerepkör, és **approot** feldolgozói szerepkörök).
> 
> 

## <a name="description-of-task-attributes"></a>A feladat attribútumok leírása
hello következő szakasz ismerteti a hello hello attribútumait **feladat** hello elemének [ServiceDefinition.csdef] fájlt:

**commandLine** -megadja a parancssor hello hello indítási tevékenységhez:

* Hello parancsot, opcionális parancssori paramétereket, amely megkezdi a hello indítási tevékenységhez.
* Ez gyakran a .cmd és .bat kötegfájl hello fájlneve.
* hello feladata relatív toohello AppRoot\\hello telepítési mappáját. Környezeti változók nem bonthatók ki hello elérési útja és fájlneve hello feladat meghatározásában. A környezeti bővítésének szükség, ha egy kis .cmd parancsfájl, amely behívja a indítási tevékenységhez is létrehozhat.
* A Konzolalkalmazás vagy egy parancsfájlt, amely elindítja a [PowerShell-parancsfájl](cloud-services-startup-tasks-common.md#create-a-powershell-startup-task).

**executionContext** -hello indítási tevékenységhez hello jogosultsági szint megadja. hello jogosultsági szint korlátozza, vagy magasabb szintű:

* **korlátozott**  
  azonos jogosultsággal hello szerepkörként hello hello indítási feladat fut. Ha hello **executionContext** hello attribútuma [futásidejű] elem is **korlátozott**, akkor használja a felhasználói jogosultságok.
* **emelt szintű**  
  hello indítási tevékenységhez rendszergazdai jogosultságokkal futtatja. Ez lehetővé teszi, hogy indítási feladatok tooinstall programok, IIS-konfigurációs módosításokat, hajtsa végre a beállításjegyzék változásainak és egyéb rendszergazdai szintű tevékenységek hello jogosultsági szint hello szerepkör maga növelése nélkül.  

> [!NOTE]
> hello indítása tevékenység jogosultsági szint nem kell toobe hello azonos hello szerepkörként magát.
> 
> 

**taskType** -megadja a hello módon indítási feladat végrehajtása.

* **egyszerű**  
  Feladatok végrehajtásának szinkron módon történik, egyenként, hello sorrendben hello megadott [ServiceDefinition.csdef] fájlt. Ha egy **egyszerű** indítási tevékenységhez végződik egy **errorlevel** nulla, hello mellett **egyszerű** indítási feladat végrehajtása. Ha nem látható több **egyszerű** indítási tooexecute, feladatok, majd hello szerepkör indul el.   
  
  > [!NOTE]
  > Ha hello **egyszerű** feladat végződik. egy nem nulla **errorlevel**, hello példány le lesz tiltva. További **egyszerű** indítási feladatokat és hello szerepkör, nem fog elindulni.
  > 
  > 
  
    a kötegfájl végződő tooensure egy **errorlevel** nulla, hajtsa végre a hello parancsot `EXIT /B 0` hello a kötegelt fájl folyamat végén.
* **háttér**  
  Feladatok aszinkron módon végrehajtásának hello indítási hello szerepkör párhuzamosan.
* **előtérben**  
  Feladatok aszinkron módon végrehajtásának hello indítási hello szerepkör párhuzamosan. hello közötti fő különbség a **előtér** és egy **háttér** feladat, hogy egy **előtér** feladat megakadályozza, hogy a hello szerepkör újrahasznosítási, vagy amíg hello feladat leállítása véget ért. Hello **háttér** feladatok nincs ezt a korlátozást.

## <a name="environment-variables"></a>Környezeti változók
Környezeti változók olyan módon toopass információk tooa indítási feladat. Például helyezhetik hello elérési tooa blob, amely egy program tooinstall tartalmaz, vagy a szerepkör által használt portszámok, illetve beállítások toocontrol funkciója az indítási tevékenységhez.

Nincsenek kétféle környezeti változók indítási feladatokhoz; statikus környezeti változókat és a környezeti változók alapján hello tagjai [roleenvironment-et] osztály. Mindkettő a hello [környezet] hello szakasza [ServiceDefinition.csdef] fájlt, és mindkét használata hello [változó] elem és **neve** az attribútum.

Statikus környezeti változókat használ hello **érték** hello attribútumának [változó] elemet. a fenti hello példa hello környezeti változót hoz **MyVersionNumber** statikus értékre van, amely "**1.0.0.0**". Egy másik példa lehet toocreate egy **StagingOrProduction** környezeti változó, amely kézzel állítható be a toovalues "**átmeneti**"vagy"**éles**" tooperform különböző indítási műveletek hello az alapján hello **StagingOrProduction** környezeti változó.

Környezeti változók alapján hello roleenvironment-et osztály tagjai ne használjon hello **érték** hello attribútumának [változó] elemet. Ehelyett hello [RoleInstanceValue] gyermekelem, a megfelelő hello **XPath** attribútum értéke, a használt toocreate hello adott tagja alapján környezeti változó [roleenvironment-et] osztály. Hello értékeinek **XPath** attribútum tooaccess különböző [roleenvironment-et] értékek található [Itt](cloud-services-role-config-xpath.md).

Például olyan környezeti változó, amely toocreate "**igaz**" hello compute emulator, hello példány futtatásakor és "**hamis**" hello felhőben használatakor hello következő [változó] és [RoleInstanceValue] elemei:

```xml
<Startup>
    <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple">
        <Environment>

            <!-- Create hello environment variable that informs hello startup task whether it is running
                in hello Compute Emulator or in hello cloud. "%ComputeEmulatorRunning%"=="true" when
                running in hello Compute Emulator, "%ComputeEmulatorRunning%"=="false" when running
                in hello cloud. -->

            <Variable name="ComputeEmulatorRunning">
                <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
            </Variable>

        </Environment>
    </Task>
</Startup>
```

## <a name="next-steps"></a>Következő lépések
Megtudhatja, hogyan tooperform néhány [közös indítási feladatok](cloud-services-startup-tasks-common.md) a felhőalapú szolgáltatással.

[Csomag](cloud-services-model-and-package.md) a felhőalapú szolgáltatás.  

[ServiceDefinition.csdef]: cloud-services-model-and-package.md#csdef
[feladat]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Task
[indítási]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Startup
[futásidejű]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Runtime
[környezet]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Environment
[változó]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Variable
[RoleInstanceValue]: https://msdn.microsoft.com/library/azure/gg557552.aspx#RoleInstanceValue
[roleenvironment-et]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx
