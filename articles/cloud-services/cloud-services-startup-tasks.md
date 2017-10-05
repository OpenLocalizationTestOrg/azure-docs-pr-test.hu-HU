---
title: "Indítási feladatok futtatása az Azure Felhőszolgáltatások |} Microsoft Docs"
description: "Indítási feladatok segítségével, az alkalmazás a cloud service-környezet előkészítéséhez. Ez útmutatást ad, hogy indítási feladatok működése, és hogyan végezheti el őket"
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
ms.openlocfilehash: 1c1b3aa86dc8211de0c07c9fb68da5685c86f551
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-configure-and-run-startup-tasks-for-a-cloud-service"></a><span data-ttu-id="5cf8b-104">Hogyan lehet konfigurálni és egy felhőalapú szolgáltatás indítási feladatok futtatása</span><span class="sxs-lookup"><span data-stu-id="5cf8b-104">How to configure and run startup tasks for a cloud service</span></span>
<span data-ttu-id="5cf8b-105">Használhatja az indítási feladatok műveletek végrehajtásához, a szerepkör indítása előtt.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-105">You can use startup tasks to perform operations before a role starts.</span></span> <span data-ttu-id="5cf8b-106">Végrehajtani kívánt műveletek közé tartozik egy összetevő telepítésével, COM-összetevők regisztrálása, beállításkulcsok vagy hosszú ideig futó folyamat elindítása.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-106">Operations that you might want to perform include installing a component, registering COM components, setting registry keys, or starting a long running process.</span></span>

> [!NOTE]
> <span data-ttu-id="5cf8b-107">Indítási feladatok nem alkalmazhatók, a virtuális gépek, csak a felhőalapú szolgáltatás webes és feldolgozói szerepköröket.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-107">Startup tasks are not applicable to Virtual Machines, only to Cloud Service Web and Worker roles.</span></span>
> 
> 

## <a name="how-startup-tasks-work"></a><span data-ttu-id="5cf8b-108">Indítási feladatok működése</span><span class="sxs-lookup"><span data-stu-id="5cf8b-108">How startup tasks work</span></span>
<span data-ttu-id="5cf8b-109">Indítási feladatokat is a szerepkörök kezdődik, és meghatározott előtt végzett műveleteket a [ServiceDefinition.csdef] fájl használatával a [feladat] elemet a [indítási] elem.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-109">Startup tasks are actions that are taken before your roles begin and are defined in the [ServiceDefinition.csdef] file by using the [Task] element within the [Startup] element.</span></span> <span data-ttu-id="5cf8b-110">Indítási feladatok gyakran parancsfájlokat, de ezek is lehetnek konzol alkalmazások, vagy indítsa el a PowerShell-parancsfájlok, kötegelt fájlok.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-110">Frequently startup tasks are batch files, but they can also be console applications, or batch files that start PowerShell scripts.</span></span>

<span data-ttu-id="5cf8b-111">Környezeti változók adhatók át információk egy indítási tevékenységhez, és helyi tárterület adhatók át információk egy indítási tevékenységhez kívül is használható.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-111">Environment variables pass information into a startup task, and local storage can be used to pass information out of a startup task.</span></span> <span data-ttu-id="5cf8b-112">Például egy környezeti változó adhatja meg a telepíteni kívánt program elérési útja, és a fájlok csak írható helyi tárhelyet tudja majd olvasni később a szerepkörök.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-112">For example, an environment variable can specify the path to a program you want to install, and files can be written to local storage that can then be read later by your roles.</span></span>

<span data-ttu-id="5cf8b-113">Az indítási tevékenységhez bejelentkezhetnek adatok és a hibák által megadott könyvtárban a **TEMP** környezeti változó.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-113">Your startup task can log information and errors to the directory specified by the **TEMP** environment variable.</span></span> <span data-ttu-id="5cf8b-114">Az indítási feladat során a **TEMP** környezeti változó feloldása egy olyan a *C:\\erőforrások\\temp\\[guid]. [ rolename]\\RoleTemp* directory, a felhő futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-114">During the startup task, the **TEMP** environment variable resolves to the *C:\\Resources\\temp\\[guid].[rolename]\\RoleTemp* directory when running on the cloud.</span></span>

<span data-ttu-id="5cf8b-115">Indítási feladatok is hajtható végre több alkalommal újraindításnál.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-115">Startup tasks can also be executed several times between reboots.</span></span> <span data-ttu-id="5cf8b-116">Például az indítási lesz futtatható a feladat minden alkalommal, amikor a szerepkör újraindul, és a szerepkör újrahasznosítja azt mindig nem tartalmazhatnak újraindítás.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-116">For example, the startup task will be run each time the role recycles, and role recycles may not always include a reboot.</span></span> <span data-ttu-id="5cf8b-117">Indítási feladatok oly módon, amely lehetővé teszi több alkalommal problémamentesen kell írni.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-117">Startup tasks should be written in a way that allows them to run several times without problems.</span></span>

<span data-ttu-id="5cf8b-118">Indítási feladatok végén szerepelnie kell egy **errorlevel** (vagy a kilépési kód) nulla a rendszerindítási folyamat befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-118">Startup tasks must end with an **errorlevel** (or exit code) of zero for the startup process to complete.</span></span> <span data-ttu-id="5cf8b-119">Ha egy indítási tevékenységhez végződik-e egy nem nulla **errorlevel**, a szerepkör nem indul el.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-119">If a startup task ends with a non-zero **errorlevel**, the role will not start.</span></span>

## <a name="role-startup-order"></a><span data-ttu-id="5cf8b-120">Szerepkör indítási sorrend</span><span class="sxs-lookup"><span data-stu-id="5cf8b-120">Role startup order</span></span>
<span data-ttu-id="5cf8b-121">Az alábbi lista tartalmazza a szerepkör indítási eljárás az Azure-ban:</span><span class="sxs-lookup"><span data-stu-id="5cf8b-121">The following lists the role startup procedure in Azure:</span></span>

1. <span data-ttu-id="5cf8b-122">A példány van megjelölve, **indítása** és nem kap a forgalmat.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-122">The instance is marked as **Starting** and does not receive traffic.</span></span>
2. <span data-ttu-id="5cf8b-123">Minden indítási feladatok végrehajtásának megfelelően a **taskType** attribútum.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-123">All startup tasks are executed according to their **taskType** attribute.</span></span>
   
   * <span data-ttu-id="5cf8b-124">A **egyszerű** feladatok végrehajtásának szinkron módon történik, egyenként.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-124">The **simple** tasks are executed synchronously, one at a time.</span></span>
   * <span data-ttu-id="5cf8b-125">A **háttér** és **előtér** feladatok indított aszinkron módon történik, párhuzamosan a indítási tevékenységhez.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-125">The **background** and **foreground** tasks are started asynchronously, parallel to the startup task.</span></span>  
     
     > [!WARNING]
     > <span data-ttu-id="5cf8b-126">Az IIS nem teljesen konfigurálható a rendszerindítási folyamat indítási tevékenységhez szakaszában, a szerepkör-specifikus adatok nem érhetők el.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-126">IIS may not be fully configured during the startup task stage in the startup process, so role-specific data may not be available.</span></span> <span data-ttu-id="5cf8b-127">Indítási feladatokhoz szükséges szerepkör-specifikus adatok használandó [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx).</span><span class="sxs-lookup"><span data-stu-id="5cf8b-127">Startup tasks that require role-specific data should use [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx).</span></span>
     > 
     > 
3. <span data-ttu-id="5cf8b-128">A szerepkör gazdagép-folyamat elindul, és a webhely létrehozása IIS-ben.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-128">The role host process is started and the site is created in IIS.</span></span>
4. <span data-ttu-id="5cf8b-129">A [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) metódust.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-129">The [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) method is called.</span></span>
5. <span data-ttu-id="5cf8b-130">A példány van megjelölve, **készen** és a példányra továbbítódik.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-130">The instance is marked as **Ready** and traffic is routed to the instance.</span></span>
6. <span data-ttu-id="5cf8b-131">A [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.Run](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metódust.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-131">The [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.Run](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) method is called.</span></span>

## <a name="example-of-a-startup-task"></a><span data-ttu-id="5cf8b-132">Példa egy indítási tevékenységhez</span><span class="sxs-lookup"><span data-stu-id="5cf8b-132">Example of a startup task</span></span>
<span data-ttu-id="5cf8b-133">Indítási feladatok vannak definiálva a [ServiceDefinition.csdef] fájlban, az a **feladat** elemet.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-133">Startup tasks are defined in the [ServiceDefinition.csdef] file, in the **Task** element.</span></span> <span data-ttu-id="5cf8b-134">A **commandLine** attribútum határozza meg, a név és a paraméterek az indítási kötegelt fájl vagy a konzol parancs, a **executionContext** attribútum meghatározza, hogy az indítási feladat a jogosultsági szint és a **taskType** attribútum határozza meg a feladat végrehajtásának módját.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-134">The **commandLine** attribute specifies the name and parameters of the startup batch file or console command, the **executionContext** attribute specifies the privilege level of the startup task, and the **taskType** attribute specifies how the task will be executed.</span></span>

<span data-ttu-id="5cf8b-135">Ebben a példában, egy környezeti változó **MyVersionNumber**, az indítási tevékenységhez hoz létre, az értéke "**1.0.0.0**".</span><span class="sxs-lookup"><span data-stu-id="5cf8b-135">In this example, an environment variable, **MyVersionNumber**, is created for the startup task and set to the value "**1.0.0.0**".</span></span>

<span data-ttu-id="5cf8b-136">**ServiceDefinition.csdef**:</span><span class="sxs-lookup"><span data-stu-id="5cf8b-136">**ServiceDefinition.csdef**:</span></span>

```xml
<Startup>
    <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" >
        <Environment>
            <Variable name="MyVersionNumber" value="1.0.0.0" />
        </Environment>
    </Task>
</Startup>
```

<span data-ttu-id="5cf8b-137">A következő példában a **Startup.cmd** parancsfájlt ír a sor "a jelenlegi verzió: 1.0.0.0" StartupLog.txt fájl a TEMP környezeti változóban megadott könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-137">In the following example, the **Startup.cmd** batch file writes the line "The current version is 1.0.0.0" to the StartupLog.txt file in the directory specified by the TEMP environment variable.</span></span> <span data-ttu-id="5cf8b-138">A `EXIT /B 0` sor biztosítja, hogy az indítási tevékenységhez végződik egy **errorlevel** nulla.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-138">The `EXIT /B 0` line ensures that the startup task ends with an **errorlevel** of zero.</span></span>

```cmd
ECHO The current version is %MyVersionNumber% >> "%TEMP%\StartupLog.txt" 2>&1
EXIT /B 0
```

> [!NOTE]
> <span data-ttu-id="5cf8b-139">A Visual Studio a **másolása a kimeneti könyvtárba** kell megadni a indítási kötegfájl tulajdonsága **másolási mindig** győződjön meg arról, hogy az indítási kötegfájl megfelelően a rendszer a projekthez az Azure (**approot\\bin** webes szerepkör, és **approot** feldolgozói szerepkörök).</span><span class="sxs-lookup"><span data-stu-id="5cf8b-139">In Visual Studio, the **Copy to Output Directory** property for your startup batch file should be set to **Copy Always** to be sure that your startup batch file is properly deployed to your project on Azure (**approot\\bin** for Web roles, and **approot** for worker roles).</span></span>
> 
> 

## <a name="description-of-task-attributes"></a><span data-ttu-id="5cf8b-140">A feladat attribútumok leírása</span><span class="sxs-lookup"><span data-stu-id="5cf8b-140">Description of task attributes</span></span>
<span data-ttu-id="5cf8b-141">A következő szakasz ismerteti a attribútumait a **feladat** eleme a [ServiceDefinition.csdef] fájlt:</span><span class="sxs-lookup"><span data-stu-id="5cf8b-141">The following describes the attributes of the **Task** element in the [ServiceDefinition.csdef] file:</span></span>

<span data-ttu-id="5cf8b-142">**commandLine** -az indítási tevékenységhez tartozó parancssort határozza meg:</span><span class="sxs-lookup"><span data-stu-id="5cf8b-142">**commandLine** - Specifies the command line for the startup task:</span></span>

* <span data-ttu-id="5cf8b-143">A parancs, opcionális parancssori paraméterek, amely megkezdi az indítási tevékenységhez.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-143">The command, with optional command line parameters, which begins the startup task.</span></span>
* <span data-ttu-id="5cf8b-144">Ez gyakran a .cmd és .bat kötegelt fájl nevét.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-144">Frequently this is the filename of a .cmd or .bat batch file.</span></span>
* <span data-ttu-id="5cf8b-145">A feladata a AppRoot viszonyítva\\a központi telepítési mappáját.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-145">The task is relative to the AppRoot\\Bin folder for the deployment.</span></span> <span data-ttu-id="5cf8b-146">Környezeti változók meghatározni, hogy az elérési útját és a feladat nem bonthatók ki.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-146">Environment variables are not expanded in determining the path and file of the task.</span></span> <span data-ttu-id="5cf8b-147">A környezeti bővítésének szükség, ha egy kis .cmd parancsfájl, amely behívja a indítási tevékenységhez is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-147">If environment expansion is required, you can create a small .cmd script that calls your startup task.</span></span>
* <span data-ttu-id="5cf8b-148">A Konzolalkalmazás vagy egy parancsfájlt, amely elindítja a [PowerShell-parancsfájl](cloud-services-startup-tasks-common.md#create-a-powershell-startup-task).</span><span class="sxs-lookup"><span data-stu-id="5cf8b-148">Can be a console application or a batch file that starts a [PowerShell script](cloud-services-startup-tasks-common.md#create-a-powershell-startup-task).</span></span>

<span data-ttu-id="5cf8b-149">**executionContext** -adja meg a jogosultsági szint az indítási tevékenységhez.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-149">**executionContext** - Specifies the privilege level for the startup task.</span></span> <span data-ttu-id="5cf8b-150">A jogosultsági szint korlátozza, vagy magasabb szintű:</span><span class="sxs-lookup"><span data-stu-id="5cf8b-150">The privilege level can be limited or elevated:</span></span>

* <span data-ttu-id="5cf8b-151">**korlátozott**</span><span class="sxs-lookup"><span data-stu-id="5cf8b-151">**limited**</span></span>  
  <span data-ttu-id="5cf8b-152">Az indítási feladat fut, a szerepkör jogosultságainak.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-152">The startup task runs with the same privileges as the role.</span></span> <span data-ttu-id="5cf8b-153">Ha a **executionContext** az attribútum a [futásidejű] elem is **korlátozott**, akkor használja a felhasználói jogosultságok.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-153">When the **executionContext** attribute for the [Runtime] element is also **limited**, then user privileges are used.</span></span>
* <span data-ttu-id="5cf8b-154">**emelt szintű**</span><span class="sxs-lookup"><span data-stu-id="5cf8b-154">**elevated**</span></span>  
  <span data-ttu-id="5cf8b-155">Az indítási tevékenységhez rendszergazdai jogosultságokkal futtatja.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-155">The startup task runs with administrator privileges.</span></span> <span data-ttu-id="5cf8b-156">Ez lehetővé teszi, hogy indítási programok telepítése, az IIS-konfigurációs módosításokat, hajtsa végre a beállításjegyzék módosításait, és más rendszergazdai szintű feladatok, a szerepkör magát a jogosultsági szint növelése nélkül.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-156">This allows startup tasks to install programs, make IIS configuration changes, perform registry changes, and other administrator level tasks, without increasing the privilege level of the role itself.</span></span>  

> [!NOTE]
> <span data-ttu-id="5cf8b-157">A jogosultsági szint indítása feladat nem lehet azonos a szerepkör kell.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-157">The privilege level of a startup task does not need to be the same as the role itself.</span></span>
> 
> 

<span data-ttu-id="5cf8b-158">**taskType** -indítási feladat végrehajtásának módját határozza meg.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-158">**taskType** - Specifies the way a startup task is executed.</span></span>

* <span data-ttu-id="5cf8b-159">**egyszerű**</span><span class="sxs-lookup"><span data-stu-id="5cf8b-159">**simple**</span></span>  
  <span data-ttu-id="5cf8b-160">Feladatok végrehajtásának szinkron módon történik, egyenként, a megadott sorrendben a [ServiceDefinition.csdef] fájlt.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-160">Tasks are executed synchronously, one at a time, in the order specified in the [ServiceDefinition.csdef] file.</span></span> <span data-ttu-id="5cf8b-161">Ha egy **egyszerű** indítási tevékenységhez végződik egy **errorlevel** nulla, a következő **egyszerű** indítási feladat végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-161">When one **simple** startup task ends with an **errorlevel** of zero, the next **simple** startup task is executed.</span></span> <span data-ttu-id="5cf8b-162">Ha nem látható több **egyszerű** indítási feladatok végrehajtásához, majd a szerepkör indul el.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-162">If there are no more **simple** startup tasks to execute, then the role itself will be started.</span></span>   
  
  > [!NOTE]
  > <span data-ttu-id="5cf8b-163">Ha a **egyszerű** feladat végződik. egy nem nulla **errorlevel**, a példány le lesz tiltva.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-163">If the **simple** task ends with a non-zero **errorlevel**, the instance will be blocked.</span></span> <span data-ttu-id="5cf8b-164">További **egyszerű** indítási feladatokat, és a szerepkör nem fog elindulni.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-164">Subsequent **simple** startup tasks, and the role itself, will not start.</span></span>
  > 
  > 
  
    <span data-ttu-id="5cf8b-165">Annak érdekében, hogy a kötegfájl végződik egy **errorlevel** nulla, a parancs végrehajtása `EXIT /B 0` a kötegelt fájl folyamat végén.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-165">To ensure that your batch file ends with an **errorlevel** of zero, execute the command `EXIT /B 0` at the end of your batch file process.</span></span>
* <span data-ttu-id="5cf8b-166">**háttér**</span><span class="sxs-lookup"><span data-stu-id="5cf8b-166">**background**</span></span>  
  <span data-ttu-id="5cf8b-167">Feladatok aszinkron módon történik, az indítási szerepkör párhuzamosan végrehajtásának.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-167">Tasks are executed asynchronously, in parallel with the startup of the role.</span></span>
* <span data-ttu-id="5cf8b-168">**előtérben**</span><span class="sxs-lookup"><span data-stu-id="5cf8b-168">**foreground**</span></span>  
  <span data-ttu-id="5cf8b-169">Feladatok aszinkron módon történik, az indítási szerepkör párhuzamosan végrehajtásának.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-169">Tasks are executed asynchronously, in parallel with the startup of the role.</span></span> <span data-ttu-id="5cf8b-170">A fő különbség a **előtér** és egy **háttér** feladat, hogy egy **előtér** feladat megakadályozza, hogy a szerepkör újrahasznosítása vagy leállítása, amíg a feladat befejeződött.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-170">The key difference between a **foreground** and a **background** task is that a **foreground** task prevents the role from recycling or shutting down until the task has ended.</span></span> <span data-ttu-id="5cf8b-171">A **háttér** feladatok nincs ezt a korlátozást.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-171">The **background** tasks do not have this restriction.</span></span>

## <a name="environment-variables"></a><span data-ttu-id="5cf8b-172">Környezeti változók</span><span class="sxs-lookup"><span data-stu-id="5cf8b-172">Environment variables</span></span>
<span data-ttu-id="5cf8b-173">Környezeti változókat, amelyek olyan információkat adnak át egy indítási tevékenységhez.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-173">Environment variables are a way to pass information to a startup task.</span></span> <span data-ttu-id="5cf8b-174">Az elérési út helyezhetik például blob, amely tartalmazza a program telepítéséhez, vagy a szerepkör által használt portszámok vagy elérhető szolgáltatások vezérléséhez a indítási feladat beállításait.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-174">For example, you can put the path to a blob that contains a program to install, or port numbers that your role will use, or settings to control features of your startup task.</span></span>

<span data-ttu-id="5cf8b-175">Nincsenek kétféle környezeti változók indítási feladatokhoz; statikus környezeti változókat és a környezeti változók alapján tagjai a [roleenvironment-et] osztály.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-175">There are two kinds of environment variables for startup tasks; static environment variables and environment variables based on members of the [RoleEnvironment] class.</span></span> <span data-ttu-id="5cf8b-176">A mindkettő a [környezet] szakasza a [ServiceDefinition.csdef] fájlt, és mindkét használja a [változó] elem és **neve** az attribútum.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-176">Both are in the [Environment] section of the [ServiceDefinition.csdef] file, and both use the [Variable] element and **name** attribute.</span></span>

<span data-ttu-id="5cf8b-177">Statikus környezeti változókat használ a **érték** attribútuma a [változó] elemet.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-177">Static environment variables uses the **value** attribute of the [Variable] element.</span></span> <span data-ttu-id="5cf8b-178">A fenti példában a környezeti változót hoz **MyVersionNumber** statikus értékre van, amely "**1.0.0.0**".</span><span class="sxs-lookup"><span data-stu-id="5cf8b-178">The example above creates the environment variable **MyVersionNumber** which has a static value of "**1.0.0.0**".</span></span> <span data-ttu-id="5cf8b-179">Egy másik példa lehet létrehozni egy **StagingOrProduction** környezeti változó, amely kézzel állítható értékeire "**átmeneti**"vagy"**éles**" végrehajtásához különböző indítási műveletek alapján értékének a **StagingOrProduction** környezeti változó.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-179">Another example would be to create a **StagingOrProduction** environment variable which you can manually set to values of "**staging**" or "**production**" to perform different startup actions based on the value of the **StagingOrProduction** environment variable.</span></span>

<span data-ttu-id="5cf8b-180">Ne használjon környezeti változókat a roleenvironment-et osztály tagjai alapján a **érték** attribútuma a [változó] elemet.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-180">Environment variables based on members of the RoleEnvironment class do not use the **value** attribute of the [Variable] element.</span></span> <span data-ttu-id="5cf8b-181">Ehelyett a [RoleInstanceValue] gyermekelem, a megfelelő **XPath** attribútum-érték, egy környezeti változó, egy adott tagjához alapú létrehozásához használt a [roleenvironment-et] osztály.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-181">Instead, the [RoleInstanceValue] child element, with the appropriate **XPath** attribute value, are used to create an environment variable based on a specific member of the [RoleEnvironment] class.</span></span> <span data-ttu-id="5cf8b-182">Értékei a **XPath** attribútum különböző eléréséhez [roleenvironment-et] értékek található [Itt](cloud-services-role-config-xpath.md).</span><span class="sxs-lookup"><span data-stu-id="5cf8b-182">Values for the **XPath** attribute to access various [RoleEnvironment] values can be found [here](cloud-services-role-config-xpath.md).</span></span>

<span data-ttu-id="5cf8b-183">Ahhoz például, hogy hozzon létre egy környezeti változó, amely "**igaz**" Ha a példány fut. a compute emulator és "**hamis**" Ha fut a felhőben, használja a következő [változó] és [RoleInstanceValue] elemei:</span><span class="sxs-lookup"><span data-stu-id="5cf8b-183">For example, to create an environment variable that is "**true**" when the instance is running in the compute emulator, and "**false**" when running in the cloud, use the following [Variable] and [RoleInstanceValue] elements:</span></span>

```xml
<Startup>
    <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple">
        <Environment>

            <!-- Create the environment variable that informs the startup task whether it is running
                in the Compute Emulator or in the cloud. "%ComputeEmulatorRunning%"=="true" when
                running in the Compute Emulator, "%ComputeEmulatorRunning%"=="false" when running
                in the cloud. -->

            <Variable name="ComputeEmulatorRunning">
                <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
            </Variable>

        </Environment>
    </Task>
</Startup>
```

## <a name="next-steps"></a><span data-ttu-id="5cf8b-184">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5cf8b-184">Next steps</span></span>
<span data-ttu-id="5cf8b-185">Ismerje meg, hogyan végezhető el [közös indítási feladatok](cloud-services-startup-tasks-common.md) a felhőalapú szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-185">Learn how to perform some [common startup tasks](cloud-services-startup-tasks-common.md) with your Cloud Service.</span></span>

<span data-ttu-id="5cf8b-186">[Csomag](cloud-services-model-and-package.md) a felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="5cf8b-186">[Package](cloud-services-model-and-package.md) your Cloud Service.</span></span>  

<span data-ttu-id="5cf8b-187">[ServiceDefinition.csdef]: cloud-services-model-and-package.md#csdef</span><span class="sxs-lookup"><span data-stu-id="5cf8b-187">[ServiceDefinition.csdef]: cloud-services-model-and-package.md#csdef</span></span>
<span data-ttu-id="5cf8b-188">[feladat]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Task</span><span class="sxs-lookup"><span data-stu-id="5cf8b-188">[Task]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Task</span></span>
<span data-ttu-id="5cf8b-189">[indítási]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Startup</span><span class="sxs-lookup"><span data-stu-id="5cf8b-189">[Startup]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Startup</span></span>
<span data-ttu-id="5cf8b-190">[futásidejű]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Runtime</span><span class="sxs-lookup"><span data-stu-id="5cf8b-190">[Runtime]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Runtime</span></span>
<span data-ttu-id="5cf8b-191">[környezet]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Environment</span><span class="sxs-lookup"><span data-stu-id="5cf8b-191">[Environment]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Environment</span></span>
<span data-ttu-id="5cf8b-192">[változó]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Variable</span><span class="sxs-lookup"><span data-stu-id="5cf8b-192">[Variable]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Variable</span></span>
<span data-ttu-id="5cf8b-193">[RoleInstanceValue]: https://msdn.microsoft.com/library/azure/gg557552.aspx#RoleInstanceValue</span><span class="sxs-lookup"><span data-stu-id="5cf8b-193">[RoleInstanceValue]: https://msdn.microsoft.com/library/azure/gg557552.aspx#RoleInstanceValue</span></span>
<span data-ttu-id="5cf8b-194">[roleenvironment-et]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx</span><span class="sxs-lookup"><span data-stu-id="5cf8b-194">[RoleEnvironment]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx</span></span>
