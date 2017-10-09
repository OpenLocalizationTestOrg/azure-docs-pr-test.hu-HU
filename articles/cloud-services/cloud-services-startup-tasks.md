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
# <a name="how-tooconfigure-and-run-startup-tasks-for-a-cloud-service"></a><span data-ttu-id="7fa66-104">Hogyan tooconfigure, és futtassa indítási feladatokat egy felhőalapú szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="7fa66-104">How tooconfigure and run startup tasks for a cloud service</span></span>
<span data-ttu-id="7fa66-105">Használhatja az indítási feladatok tooperform műveletek szerepkör indítása előtt.</span><span class="sxs-lookup"><span data-stu-id="7fa66-105">You can use startup tasks tooperform operations before a role starts.</span></span> <span data-ttu-id="7fa66-106">Előfordulhat, hogy kívánt tooperform műveletek közé tartozik, egy összetevő telepítésével, COM-összetevők regisztrálása, beállításkulcsok vagy hosszú ideig futó folyamat elindítása.</span><span class="sxs-lookup"><span data-stu-id="7fa66-106">Operations that you might want tooperform include installing a component, registering COM components, setting registry keys, or starting a long running process.</span></span>

> [!NOTE]
> <span data-ttu-id="7fa66-107">Indítási feladatokat nem alkalmazható tooVirtual gépek, csak tooCloud szolgáltatás webes és feldolgozói szerepköröket is.</span><span class="sxs-lookup"><span data-stu-id="7fa66-107">Startup tasks are not applicable tooVirtual Machines, only tooCloud Service Web and Worker roles.</span></span>
> 
> 

## <a name="how-startup-tasks-work"></a><span data-ttu-id="7fa66-108">Indítási feladatok működése</span><span class="sxs-lookup"><span data-stu-id="7fa66-108">How startup tasks work</span></span>
<span data-ttu-id="7fa66-109">Indítási feladatokat is a szerepkörök kezdődik, és meghatározott hello előtt végzett műveleteket [ServiceDefinition.csdef] hello segítségével fájlt [feladat] hello elemének [indítási]elemet.</span><span class="sxs-lookup"><span data-stu-id="7fa66-109">Startup tasks are actions that are taken before your roles begin and are defined in hello [ServiceDefinition.csdef] file by using hello [Task] element within hello [Startup] element.</span></span> <span data-ttu-id="7fa66-110">Indítási feladatok gyakran parancsfájlokat, de ezek is lehetnek konzol alkalmazások, vagy indítsa el a PowerShell-parancsfájlok, kötegelt fájlok.</span><span class="sxs-lookup"><span data-stu-id="7fa66-110">Frequently startup tasks are batch files, but they can also be console applications, or batch files that start PowerShell scripts.</span></span>

<span data-ttu-id="7fa66-111">Környezeti változók adhatók át információk egy indítási tevékenységhez, és a helyi tároló lehet egy indítási tevékenységhez kívül használt toopass információkat.</span><span class="sxs-lookup"><span data-stu-id="7fa66-111">Environment variables pass information into a startup task, and local storage can be used toopass information out of a startup task.</span></span> <span data-ttu-id="7fa66-112">Egy környezeti változó megadhatja például, hello elérési tooa program tooinstall szeretne, valamint a fájlok írhatók tudja majd olvasni később a szerepkörök toolocal tárhelyet.</span><span class="sxs-lookup"><span data-stu-id="7fa66-112">For example, an environment variable can specify hello path tooa program you want tooinstall, and files can be written toolocal storage that can then be read later by your roles.</span></span>

<span data-ttu-id="7fa66-113">Az indítási tevékenységhez jelentkezhetnek, adatokat és hello által megadott könyvtárban található hibák toohello **TEMP** környezeti változó.</span><span class="sxs-lookup"><span data-stu-id="7fa66-113">Your startup task can log information and errors toohello directory specified by hello **TEMP** environment variable.</span></span> <span data-ttu-id="7fa66-114">Hello indítási feladat során hello **TEMP** környezeti változó, oldja fel toohello *C:\\erőforrások\\temp\\[guid]. [ rolename]\\RoleTemp* directory hello felhő futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="7fa66-114">During hello startup task, hello **TEMP** environment variable resolves toohello *C:\\Resources\\temp\\[guid].[rolename]\\RoleTemp* directory when running on hello cloud.</span></span>

<span data-ttu-id="7fa66-115">Indítási feladatok is hajtható végre több alkalommal újraindításnál.</span><span class="sxs-lookup"><span data-stu-id="7fa66-115">Startup tasks can also be executed several times between reboots.</span></span> <span data-ttu-id="7fa66-116">Például hello indítási lesz futtatható a feladat minden alkalommal, amikor hello szerepkör újraindul, és a szerepkör újrahasznosítja azt mindig nem tartalmazhatnak újraindítás.</span><span class="sxs-lookup"><span data-stu-id="7fa66-116">For example, hello startup task will be run each time hello role recycles, and role recycles may not always include a reboot.</span></span> <span data-ttu-id="7fa66-117">Indítási feladatok oly módon, amely lehetővé tenné toorun problémamentesen többször kell írni.</span><span class="sxs-lookup"><span data-stu-id="7fa66-117">Startup tasks should be written in a way that allows them toorun several times without problems.</span></span>

<span data-ttu-id="7fa66-118">Indítási feladatok végén szerepelnie kell egy **errorlevel** (vagy a kilépési kód) hello indítási folyamat toocomplete nulla.</span><span class="sxs-lookup"><span data-stu-id="7fa66-118">Startup tasks must end with an **errorlevel** (or exit code) of zero for hello startup process toocomplete.</span></span> <span data-ttu-id="7fa66-119">Ha egy indítási tevékenységhez végződik-e egy nem nulla **errorlevel**, hello szerepkör nem indul el.</span><span class="sxs-lookup"><span data-stu-id="7fa66-119">If a startup task ends with a non-zero **errorlevel**, hello role will not start.</span></span>

## <a name="role-startup-order"></a><span data-ttu-id="7fa66-120">Szerepkör indítási sorrend</span><span class="sxs-lookup"><span data-stu-id="7fa66-120">Role startup order</span></span>
<span data-ttu-id="7fa66-121">a következő listák hello szerepkör indítási eljárás az Azure-ban hello:</span><span class="sxs-lookup"><span data-stu-id="7fa66-121">hello following lists hello role startup procedure in Azure:</span></span>

1. <span data-ttu-id="7fa66-122">hello példány van megjelölve, **indítása** és nem kap a forgalmat.</span><span class="sxs-lookup"><span data-stu-id="7fa66-122">hello instance is marked as **Starting** and does not receive traffic.</span></span>
2. <span data-ttu-id="7fa66-123">Minden indítási feladatok végrehajtásának tootheir szerint **taskType** attribútum.</span><span class="sxs-lookup"><span data-stu-id="7fa66-123">All startup tasks are executed according tootheir **taskType** attribute.</span></span>
   
   * <span data-ttu-id="7fa66-124">Hello **egyszerű** feladatok végrehajtásának szinkron módon történik, egyenként.</span><span class="sxs-lookup"><span data-stu-id="7fa66-124">hello **simple** tasks are executed synchronously, one at a time.</span></span>
   * <span data-ttu-id="7fa66-125">Hello **háttér** és **előtér** feladatokat is aszinkron módon elindítva, párhuzamos toohello indítási tevékenységhez.</span><span class="sxs-lookup"><span data-stu-id="7fa66-125">hello **background** and **foreground** tasks are started asynchronously, parallel toohello startup task.</span></span>  
     
     > [!WARNING]
     > <span data-ttu-id="7fa66-126">Az IIS nem teljesen konfigurálható hello indítási tevékenységhez szakasza hello indítási folyamat során, a szerepkör-specifikus adatok nem érhetők el.</span><span class="sxs-lookup"><span data-stu-id="7fa66-126">IIS may not be fully configured during hello startup task stage in hello startup process, so role-specific data may not be available.</span></span> <span data-ttu-id="7fa66-127">Indítási feladatokhoz szükséges szerepkör-specifikus adatok használandó [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx).</span><span class="sxs-lookup"><span data-stu-id="7fa66-127">Startup tasks that require role-specific data should use [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx).</span></span>
     > 
     > 
3. <span data-ttu-id="7fa66-128">hello szerepkör gazdagép-folyamat elindul, és hello helyen jön létre az IIS-ben.</span><span class="sxs-lookup"><span data-stu-id="7fa66-128">hello role host process is started and hello site is created in IIS.</span></span>
4. <span data-ttu-id="7fa66-129">Hello [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) metódust.</span><span class="sxs-lookup"><span data-stu-id="7fa66-129">hello [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) method is called.</span></span>
5. <span data-ttu-id="7fa66-130">hello példány van megjelölve, **készen** és forgalma irányított toohello példány.</span><span class="sxs-lookup"><span data-stu-id="7fa66-130">hello instance is marked as **Ready** and traffic is routed toohello instance.</span></span>
6. <span data-ttu-id="7fa66-131">Hello [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.Run](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metódust.</span><span class="sxs-lookup"><span data-stu-id="7fa66-131">hello [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.Run](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) method is called.</span></span>

## <a name="example-of-a-startup-task"></a><span data-ttu-id="7fa66-132">Példa egy indítási tevékenységhez</span><span class="sxs-lookup"><span data-stu-id="7fa66-132">Example of a startup task</span></span>
<span data-ttu-id="7fa66-133">Indítási feladatok meghatározott hello [ServiceDefinition.csdef] hello-fájlban **feladat** elemet.</span><span class="sxs-lookup"><span data-stu-id="7fa66-133">Startup tasks are defined in hello [ServiceDefinition.csdef] file, in hello **Task** element.</span></span> <span data-ttu-id="7fa66-134">Hello **commandLine** attribútum hello nevét adja meg, és paraméterek hello indítási köteg fájlt, vagy konzol parancs, hello **executionContext** attribútum hello indítási hello jogosultság szintjét határozza meg. a feladat, és hello **taskType** attribútum meghatározza, hogy hello feladat végrehajtásának módját.</span><span class="sxs-lookup"><span data-stu-id="7fa66-134">hello **commandLine** attribute specifies hello name and parameters of hello startup batch file or console command, hello **executionContext** attribute specifies hello privilege level of hello startup task, and hello **taskType** attribute specifies how hello task will be executed.</span></span>

<span data-ttu-id="7fa66-135">Ebben a példában, egy környezeti változó **MyVersionNumber**létrehozott hello indítási tevékenységhez, amely toohello érték "**1.0.0.0**".</span><span class="sxs-lookup"><span data-stu-id="7fa66-135">In this example, an environment variable, **MyVersionNumber**, is created for hello startup task and set toohello value "**1.0.0.0**".</span></span>

<span data-ttu-id="7fa66-136">**ServiceDefinition.csdef**:</span><span class="sxs-lookup"><span data-stu-id="7fa66-136">**ServiceDefinition.csdef**:</span></span>

```xml
<Startup>
    <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" >
        <Environment>
            <Variable name="MyVersionNumber" value="1.0.0.0" />
        </Environment>
    </Task>
</Startup>
```

<span data-ttu-id="7fa66-137">A következő példa hello, hello **Startup.cmd** parancsfájlt ír hello sor toohello StartupLog.txt fájl "hello jelenlegi verzió: 1.0.0.0" hello TEMP környezeti változóban megadott hello könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="7fa66-137">In hello following example, hello **Startup.cmd** batch file writes hello line "hello current version is 1.0.0.0" toohello StartupLog.txt file in hello directory specified by hello TEMP environment variable.</span></span> <span data-ttu-id="7fa66-138">Hello `EXIT /B 0` sor biztosítja, hogy hello indítási tevékenységhez végződik egy **errorlevel** nulla.</span><span class="sxs-lookup"><span data-stu-id="7fa66-138">hello `EXIT /B 0` line ensures that hello startup task ends with an **errorlevel** of zero.</span></span>

```cmd
ECHO hello current version is %MyVersionNumber% >> "%TEMP%\StartupLog.txt" 2>&1
EXIT /B 0
```

> [!NOTE]
> <span data-ttu-id="7fa66-139">A Visual Studio hello **tooOutput Directory másolása** tulajdonságot a indítási kötegelt fájl kell beállítani, túl**másolási mindig** toobe meg arról, hogy az indítási kötegelt fájl megfelelően van-e telepítve tooyour projektet az Azure-on (**approot\\bin** webes szerepkör, és **approot** feldolgozói szerepkörök).</span><span class="sxs-lookup"><span data-stu-id="7fa66-139">In Visual Studio, hello **Copy tooOutput Directory** property for your startup batch file should be set too**Copy Always** toobe sure that your startup batch file is properly deployed tooyour project on Azure (**approot\\bin** for Web roles, and **approot** for worker roles).</span></span>
> 
> 

## <a name="description-of-task-attributes"></a><span data-ttu-id="7fa66-140">A feladat attribútumok leírása</span><span class="sxs-lookup"><span data-stu-id="7fa66-140">Description of task attributes</span></span>
<span data-ttu-id="7fa66-141">hello következő szakasz ismerteti a hello hello attribútumait **feladat** hello elemének [ServiceDefinition.csdef] fájlt:</span><span class="sxs-lookup"><span data-stu-id="7fa66-141">hello following describes hello attributes of hello **Task** element in hello [ServiceDefinition.csdef] file:</span></span>

<span data-ttu-id="7fa66-142">**commandLine** -megadja a parancssor hello hello indítási tevékenységhez:</span><span class="sxs-lookup"><span data-stu-id="7fa66-142">**commandLine** - Specifies hello command line for hello startup task:</span></span>

* <span data-ttu-id="7fa66-143">Hello parancsot, opcionális parancssori paramétereket, amely megkezdi a hello indítási tevékenységhez.</span><span class="sxs-lookup"><span data-stu-id="7fa66-143">hello command, with optional command line parameters, which begins hello startup task.</span></span>
* <span data-ttu-id="7fa66-144">Ez gyakran a .cmd és .bat kötegfájl hello fájlneve.</span><span class="sxs-lookup"><span data-stu-id="7fa66-144">Frequently this is hello filename of a .cmd or .bat batch file.</span></span>
* <span data-ttu-id="7fa66-145">hello feladata relatív toohello AppRoot\\hello telepítési mappáját.</span><span class="sxs-lookup"><span data-stu-id="7fa66-145">hello task is relative toohello AppRoot\\Bin folder for hello deployment.</span></span> <span data-ttu-id="7fa66-146">Környezeti változók nem bonthatók ki hello elérési útja és fájlneve hello feladat meghatározásában.</span><span class="sxs-lookup"><span data-stu-id="7fa66-146">Environment variables are not expanded in determining hello path and file of hello task.</span></span> <span data-ttu-id="7fa66-147">A környezeti bővítésének szükség, ha egy kis .cmd parancsfájl, amely behívja a indítási tevékenységhez is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="7fa66-147">If environment expansion is required, you can create a small .cmd script that calls your startup task.</span></span>
* <span data-ttu-id="7fa66-148">A Konzolalkalmazás vagy egy parancsfájlt, amely elindítja a [PowerShell-parancsfájl](cloud-services-startup-tasks-common.md#create-a-powershell-startup-task).</span><span class="sxs-lookup"><span data-stu-id="7fa66-148">Can be a console application or a batch file that starts a [PowerShell script](cloud-services-startup-tasks-common.md#create-a-powershell-startup-task).</span></span>

<span data-ttu-id="7fa66-149">**executionContext** -hello indítási tevékenységhez hello jogosultsági szint megadja.</span><span class="sxs-lookup"><span data-stu-id="7fa66-149">**executionContext** - Specifies hello privilege level for hello startup task.</span></span> <span data-ttu-id="7fa66-150">hello jogosultsági szint korlátozza, vagy magasabb szintű:</span><span class="sxs-lookup"><span data-stu-id="7fa66-150">hello privilege level can be limited or elevated:</span></span>

* <span data-ttu-id="7fa66-151">**korlátozott**</span><span class="sxs-lookup"><span data-stu-id="7fa66-151">**limited**</span></span>  
  <span data-ttu-id="7fa66-152">azonos jogosultsággal hello szerepkörként hello hello indítási feladat fut.</span><span class="sxs-lookup"><span data-stu-id="7fa66-152">hello startup task runs with hello same privileges as hello role.</span></span> <span data-ttu-id="7fa66-153">Ha hello **executionContext** hello attribútuma [futásidejű] elem is **korlátozott**, akkor használja a felhasználói jogosultságok.</span><span class="sxs-lookup"><span data-stu-id="7fa66-153">When hello **executionContext** attribute for hello [Runtime] element is also **limited**, then user privileges are used.</span></span>
* <span data-ttu-id="7fa66-154">**emelt szintű**</span><span class="sxs-lookup"><span data-stu-id="7fa66-154">**elevated**</span></span>  
  <span data-ttu-id="7fa66-155">hello indítási tevékenységhez rendszergazdai jogosultságokkal futtatja.</span><span class="sxs-lookup"><span data-stu-id="7fa66-155">hello startup task runs with administrator privileges.</span></span> <span data-ttu-id="7fa66-156">Ez lehetővé teszi, hogy indítási feladatok tooinstall programok, IIS-konfigurációs módosításokat, hajtsa végre a beállításjegyzék változásainak és egyéb rendszergazdai szintű tevékenységek hello jogosultsági szint hello szerepkör maga növelése nélkül.</span><span class="sxs-lookup"><span data-stu-id="7fa66-156">This allows startup tasks tooinstall programs, make IIS configuration changes, perform registry changes, and other administrator level tasks, without increasing hello privilege level of hello role itself.</span></span>  

> [!NOTE]
> <span data-ttu-id="7fa66-157">hello indítása tevékenység jogosultsági szint nem kell toobe hello azonos hello szerepkörként magát.</span><span class="sxs-lookup"><span data-stu-id="7fa66-157">hello privilege level of a startup task does not need toobe hello same as hello role itself.</span></span>
> 
> 

<span data-ttu-id="7fa66-158">**taskType** -megadja a hello módon indítási feladat végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="7fa66-158">**taskType** - Specifies hello way a startup task is executed.</span></span>

* <span data-ttu-id="7fa66-159">**egyszerű**</span><span class="sxs-lookup"><span data-stu-id="7fa66-159">**simple**</span></span>  
  <span data-ttu-id="7fa66-160">Feladatok végrehajtásának szinkron módon történik, egyenként, hello sorrendben hello megadott [ServiceDefinition.csdef] fájlt.</span><span class="sxs-lookup"><span data-stu-id="7fa66-160">Tasks are executed synchronously, one at a time, in hello order specified in hello [ServiceDefinition.csdef] file.</span></span> <span data-ttu-id="7fa66-161">Ha egy **egyszerű** indítási tevékenységhez végződik egy **errorlevel** nulla, hello mellett **egyszerű** indítási feladat végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="7fa66-161">When one **simple** startup task ends with an **errorlevel** of zero, hello next **simple** startup task is executed.</span></span> <span data-ttu-id="7fa66-162">Ha nem látható több **egyszerű** indítási tooexecute, feladatok, majd hello szerepkör indul el.</span><span class="sxs-lookup"><span data-stu-id="7fa66-162">If there are no more **simple** startup tasks tooexecute, then hello role itself will be started.</span></span>   
  
  > [!NOTE]
  > <span data-ttu-id="7fa66-163">Ha hello **egyszerű** feladat végződik. egy nem nulla **errorlevel**, hello példány le lesz tiltva.</span><span class="sxs-lookup"><span data-stu-id="7fa66-163">If hello **simple** task ends with a non-zero **errorlevel**, hello instance will be blocked.</span></span> <span data-ttu-id="7fa66-164">További **egyszerű** indítási feladatokat és hello szerepkör, nem fog elindulni.</span><span class="sxs-lookup"><span data-stu-id="7fa66-164">Subsequent **simple** startup tasks, and hello role itself, will not start.</span></span>
  > 
  > 
  
    <span data-ttu-id="7fa66-165">a kötegfájl végződő tooensure egy **errorlevel** nulla, hajtsa végre a hello parancsot `EXIT /B 0` hello a kötegelt fájl folyamat végén.</span><span class="sxs-lookup"><span data-stu-id="7fa66-165">tooensure that your batch file ends with an **errorlevel** of zero, execute hello command `EXIT /B 0` at hello end of your batch file process.</span></span>
* <span data-ttu-id="7fa66-166">**háttér**</span><span class="sxs-lookup"><span data-stu-id="7fa66-166">**background**</span></span>  
  <span data-ttu-id="7fa66-167">Feladatok aszinkron módon végrehajtásának hello indítási hello szerepkör párhuzamosan.</span><span class="sxs-lookup"><span data-stu-id="7fa66-167">Tasks are executed asynchronously, in parallel with hello startup of hello role.</span></span>
* <span data-ttu-id="7fa66-168">**előtérben**</span><span class="sxs-lookup"><span data-stu-id="7fa66-168">**foreground**</span></span>  
  <span data-ttu-id="7fa66-169">Feladatok aszinkron módon végrehajtásának hello indítási hello szerepkör párhuzamosan.</span><span class="sxs-lookup"><span data-stu-id="7fa66-169">Tasks are executed asynchronously, in parallel with hello startup of hello role.</span></span> <span data-ttu-id="7fa66-170">hello közötti fő különbség a **előtér** és egy **háttér** feladat, hogy egy **előtér** feladat megakadályozza, hogy a hello szerepkör újrahasznosítási, vagy amíg hello feladat leállítása véget ért.</span><span class="sxs-lookup"><span data-stu-id="7fa66-170">hello key difference between a **foreground** and a **background** task is that a **foreground** task prevents hello role from recycling or shutting down until hello task has ended.</span></span> <span data-ttu-id="7fa66-171">Hello **háttér** feladatok nincs ezt a korlátozást.</span><span class="sxs-lookup"><span data-stu-id="7fa66-171">hello **background** tasks do not have this restriction.</span></span>

## <a name="environment-variables"></a><span data-ttu-id="7fa66-172">Környezeti változók</span><span class="sxs-lookup"><span data-stu-id="7fa66-172">Environment variables</span></span>
<span data-ttu-id="7fa66-173">Környezeti változók olyan módon toopass információk tooa indítási feladat.</span><span class="sxs-lookup"><span data-stu-id="7fa66-173">Environment variables are a way toopass information tooa startup task.</span></span> <span data-ttu-id="7fa66-174">Például helyezhetik hello elérési tooa blob, amely egy program tooinstall tartalmaz, vagy a szerepkör által használt portszámok, illetve beállítások toocontrol funkciója az indítási tevékenységhez.</span><span class="sxs-lookup"><span data-stu-id="7fa66-174">For example, you can put hello path tooa blob that contains a program tooinstall, or port numbers that your role will use, or settings toocontrol features of your startup task.</span></span>

<span data-ttu-id="7fa66-175">Nincsenek kétféle környezeti változók indítási feladatokhoz; statikus környezeti változókat és a környezeti változók alapján hello tagjai [roleenvironment-et] osztály.</span><span class="sxs-lookup"><span data-stu-id="7fa66-175">There are two kinds of environment variables for startup tasks; static environment variables and environment variables based on members of hello [RoleEnvironment] class.</span></span> <span data-ttu-id="7fa66-176">Mindkettő a hello [környezet] hello szakasza [ServiceDefinition.csdef] fájlt, és mindkét használata hello [változó] elem és **neve** az attribútum.</span><span class="sxs-lookup"><span data-stu-id="7fa66-176">Both are in hello [Environment] section of hello [ServiceDefinition.csdef] file, and both use hello [Variable] element and **name** attribute.</span></span>

<span data-ttu-id="7fa66-177">Statikus környezeti változókat használ hello **érték** hello attribútumának [változó] elemet.</span><span class="sxs-lookup"><span data-stu-id="7fa66-177">Static environment variables uses hello **value** attribute of hello [Variable] element.</span></span> <span data-ttu-id="7fa66-178">a fenti hello példa hello környezeti változót hoz **MyVersionNumber** statikus értékre van, amely "**1.0.0.0**".</span><span class="sxs-lookup"><span data-stu-id="7fa66-178">hello example above creates hello environment variable **MyVersionNumber** which has a static value of "**1.0.0.0**".</span></span> <span data-ttu-id="7fa66-179">Egy másik példa lehet toocreate egy **StagingOrProduction** környezeti változó, amely kézzel állítható be a toovalues "**átmeneti**"vagy"**éles**" tooperform különböző indítási műveletek hello az alapján hello **StagingOrProduction** környezeti változó.</span><span class="sxs-lookup"><span data-stu-id="7fa66-179">Another example would be toocreate a **StagingOrProduction** environment variable which you can manually set toovalues of "**staging**" or "**production**" tooperform different startup actions based on hello value of hello **StagingOrProduction** environment variable.</span></span>

<span data-ttu-id="7fa66-180">Környezeti változók alapján hello roleenvironment-et osztály tagjai ne használjon hello **érték** hello attribútumának [változó] elemet.</span><span class="sxs-lookup"><span data-stu-id="7fa66-180">Environment variables based on members of hello RoleEnvironment class do not use hello **value** attribute of hello [Variable] element.</span></span> <span data-ttu-id="7fa66-181">Ehelyett hello [RoleInstanceValue] gyermekelem, a megfelelő hello **XPath** attribútum értéke, a használt toocreate hello adott tagja alapján környezeti változó [roleenvironment-et] osztály.</span><span class="sxs-lookup"><span data-stu-id="7fa66-181">Instead, hello [RoleInstanceValue] child element, with hello appropriate **XPath** attribute value, are used toocreate an environment variable based on a specific member of hello [RoleEnvironment] class.</span></span> <span data-ttu-id="7fa66-182">Hello értékeinek **XPath** attribútum tooaccess különböző [roleenvironment-et] értékek található [Itt](cloud-services-role-config-xpath.md).</span><span class="sxs-lookup"><span data-stu-id="7fa66-182">Values for hello **XPath** attribute tooaccess various [RoleEnvironment] values can be found [here](cloud-services-role-config-xpath.md).</span></span>

<span data-ttu-id="7fa66-183">Például olyan környezeti változó, amely toocreate "**igaz**" hello compute emulator, hello példány futtatásakor és "**hamis**" hello felhőben használatakor hello következő [változó] és [RoleInstanceValue] elemei:</span><span class="sxs-lookup"><span data-stu-id="7fa66-183">For example, toocreate an environment variable that is "**true**" when hello instance is running in hello compute emulator, and "**false**" when running in hello cloud, use hello following [Variable] and [RoleInstanceValue] elements:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="7fa66-184">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7fa66-184">Next steps</span></span>
<span data-ttu-id="7fa66-185">Megtudhatja, hogyan tooperform néhány [közös indítási feladatok](cloud-services-startup-tasks-common.md) a felhőalapú szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="7fa66-185">Learn how tooperform some [common startup tasks](cloud-services-startup-tasks-common.md) with your Cloud Service.</span></span>

<span data-ttu-id="7fa66-186">[Csomag](cloud-services-model-and-package.md) a felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="7fa66-186">[Package](cloud-services-model-and-package.md) your Cloud Service.</span></span>  

[ServiceDefinition.csdef]: cloud-services-model-and-package.md#csdef
[feladat]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Task
[indítási]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Startup
[futásidejű]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Runtime
[környezet]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Environment
[változó]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Variable
[RoleInstanceValue]: https://msdn.microsoft.com/library/azure/gg557552.aspx#RoleInstanceValue
[roleenvironment-et]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx
