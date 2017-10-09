---
title: "Azure Cloud Services szerepkörök .NET aaaInstall |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toomanually a felhőalapú szolgáltatás webes és feldolgozói szerepkörök hello .NET-keretrendszer telepítése"
services: cloud-services
documentationcenter: .net
author: thraka
manager: timlt
editor: 
ms.assetid: 8d1243dc-879c-4d1f-9ed0-eecd1f6a6653
ms.service: cloud-services
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/24/2017
ms.author: adegeo
ms.openlocfilehash: 45f0f30221292f98c591511b091b02ebe1c1272c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-net-on-azure-cloud-services-roles"></a><span data-ttu-id="7a3b9-103">Telepítse a .NET szerepkörök Azure Cloud Services csomag</span><span class="sxs-lookup"><span data-stu-id="7a3b9-103">Install .NET on Azure Cloud Services roles</span></span>
<span data-ttu-id="7a3b9-104">Ez a cikk ismerteti, hogyan tooinstall verzióit, amelyek nem rendelkeznek .NET-keretrendszer hello Azure vendég operációs rendszer.</span><span class="sxs-lookup"><span data-stu-id="7a3b9-104">This article describes how tooinstall versions of .NET Framework that don't come with hello Azure Guest OS.</span></span> <span data-ttu-id="7a3b9-105">Használhatja a .NET hello vendég operációs rendszer tooconfigure a felhőalapú szolgáltatás webes és feldolgozói szerepköröket.</span><span class="sxs-lookup"><span data-stu-id="7a3b9-105">You can use .NET on hello Guest OS tooconfigure your cloud service web and worker roles.</span></span>

<span data-ttu-id="7a3b9-106">Például telepíthető .NET 4.6.1 hello Vendég operációsrendszer-család 4, amelyhez nem tartozik a .NET 4.6 bármelyik verzióját.</span><span class="sxs-lookup"><span data-stu-id="7a3b9-106">For example, you can install .NET 4.6.1 on hello Guest OS family 4, which doesn't come with any release of .NET 4.6.</span></span> <span data-ttu-id="7a3b9-107">(hello Vendég operációsrendszer-család 5 .NET 4.6 kapható.) Hello legfrissebb információk hello Azure vendég operációs rendszer feloldja, lásd: hello [Azure vendég operációs rendszer kiadási hírek](cloud-services-guestos-update-matrix.md).</span><span class="sxs-lookup"><span data-stu-id="7a3b9-107">(hello Guest OS family 5 does come with .NET 4.6.) For hello latest information on hello Azure Guest OS releases, see hello [Azure Guest OS release news](cloud-services-guestos-update-matrix.md).</span></span> 

>[!IMPORTANT]
><span data-ttu-id="7a3b9-108">hello Azure SDK 2.9 tartalmazza a .NET 4.6 telepítése hello Vendég operációsrendszer-család 4-es vagy korábbi korlátozását.</span><span class="sxs-lookup"><span data-stu-id="7a3b9-108">hello Azure SDK 2.9 contains a restriction on deploying .NET 4.6 on hello Guest OS family 4 or earlier.</span></span> <span data-ttu-id="7a3b9-109">Egy javítást alkalmaztunk hello korlátozás érhető el hello [Microsoft Docs](https://github.com/MicrosoftDocs/azure-cloud-services-files/tree/master/Azure%20Targets%20SDK%202.9) hely.</span><span class="sxs-lookup"><span data-stu-id="7a3b9-109">A fix for hello restriction is available on hello [Microsoft Docs](https://github.com/MicrosoftDocs/azure-cloud-services-files/tree/master/Azure%20Targets%20SDK%202.9) site.</span></span>

<span data-ttu-id="7a3b9-110">a webes és feldolgozói szerepkörök .NET tooinstall hello .NET webes telepítőjét a felhőszolgáltatás-projekt részeként tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="7a3b9-110">tooinstall .NET on your web and worker roles, include hello .NET web installer as part of your cloud service project.</span></span> <span data-ttu-id="7a3b9-111">Indítsa el a hello telepítő hello szerepkör indítása feladatok részeként.</span><span class="sxs-lookup"><span data-stu-id="7a3b9-111">Start hello installer as part of hello role's startup tasks.</span></span> 

## <a name="add-hello-net-installer-tooyour-project"></a><span data-ttu-id="7a3b9-112">Hello .NET telepítő tooyour projekt hozzáadása</span><span class="sxs-lookup"><span data-stu-id="7a3b9-112">Add hello .NET installer tooyour project</span></span>
<span data-ttu-id="7a3b9-113">toodownload hello webes telepítőjének hello .NET-keretrendszer, válassza ki a megjeleníteni kívánt tooinstall hello verziót:</span><span class="sxs-lookup"><span data-stu-id="7a3b9-113">toodownload hello web installer for hello .NET Framework, choose hello version that you want tooinstall:</span></span>

* [<span data-ttu-id="7a3b9-114">.NET 4.7 telepítő</span><span class="sxs-lookup"><span data-stu-id="7a3b9-114">.NET 4.7 web installer</span></span>](http://go.microsoft.com/fwlink/?LinkId=825298)
* [<span data-ttu-id="7a3b9-115">.NET 4.6.1 webalkalmazás-telepítő</span><span class="sxs-lookup"><span data-stu-id="7a3b9-115">.NET 4.6.1 web installer</span></span>](http://go.microsoft.com/fwlink/?LinkId=671729)

<span data-ttu-id="7a3b9-116">tooadd hello telepítőjéhez egy *webes* szerepkör:</span><span class="sxs-lookup"><span data-stu-id="7a3b9-116">tooadd hello installer for a *web* role:</span></span>
  1. <span data-ttu-id="7a3b9-117">A **Megoldáskezelőben**a **szerepkörök** a felhőszolgáltatás-projekt, kattintson a jobb egérgombbal a *webes* szerepkör, és válassza **Hozzáadás**  >  **Új mappa**.</span><span class="sxs-lookup"><span data-stu-id="7a3b9-117">In **Solution Explorer**, under **Roles** in your cloud service project, right-click your *web* role and select **Add** > **New Folder**.</span></span> <span data-ttu-id="7a3b9-118">Hozza létre a **bin**.</span><span class="sxs-lookup"><span data-stu-id="7a3b9-118">Create a folder named **bin**.</span></span>
  2. <span data-ttu-id="7a3b9-119">Kattintson a jobb gombbal a hello bin mappát, és válassza ki **Hozzáadás** > **meglévő cikk**.</span><span class="sxs-lookup"><span data-stu-id="7a3b9-119">Right-click hello bin folder and select **Add** > **Existing Item**.</span></span> <span data-ttu-id="7a3b9-120">Válassza ki a hello .NET telepítő, és vegye fel toohello bin mappájában.</span><span class="sxs-lookup"><span data-stu-id="7a3b9-120">Select hello .NET installer and add it toohello bin folder.</span></span>
  
<span data-ttu-id="7a3b9-121">tooadd hello telepítőjéhez egy *munkavégző* szerepkör:</span><span class="sxs-lookup"><span data-stu-id="7a3b9-121">tooadd hello installer for a *worker* role:</span></span>
* <span data-ttu-id="7a3b9-122">Kattintson a jobb gombbal a *munkavégző* szerepkör, és válassza **Hozzáadás** > **meglévő cikk**.</span><span class="sxs-lookup"><span data-stu-id="7a3b9-122">Right-click your *worker* role and select **Add** > **Existing Item**.</span></span> <span data-ttu-id="7a3b9-123">Válassza ki a hello .NET telepítő, és vegye fel toohello szerepkör.</span><span class="sxs-lookup"><span data-stu-id="7a3b9-123">Select hello .NET installer and add it toohello role.</span></span> 

<span data-ttu-id="7a3b9-124">Fájlok kerülnek a módon toohello szerepkör tartalom mappában, ha azok még automatikusan tooyour cloud service-csomag.</span><span class="sxs-lookup"><span data-stu-id="7a3b9-124">When files are added in this way toohello role content folder, they're automatically added tooyour cloud service package.</span></span> <span data-ttu-id="7a3b9-125">hello fájlok vannak, majd a telepített tooa konzisztens hely hello virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="7a3b9-125">hello files are then deployed tooa consistent location on hello virtual machine.</span></span> <span data-ttu-id="7a3b9-126">Ismételje meg az eljárást minden webes és feldolgozói szerepkör esetén a felhőalapú szolgáltatás, hogy az összes szerepkör hello telepítő másolatát.</span><span class="sxs-lookup"><span data-stu-id="7a3b9-126">Repeat this process for each web and worker role in your cloud service so that all roles have a copy of hello installer.</span></span>

> [!NOTE]
> <span data-ttu-id="7a3b9-127">.NET 4.6.1 telepítsen a felhőalapú szolgáltatás szerepkör, akkor is, ha az alkalmazáshoz .NET 4.6 célozza.</span><span class="sxs-lookup"><span data-stu-id="7a3b9-127">You should install .NET 4.6.1 on your cloud service role even if your application targets .NET 4.6.</span></span> <span data-ttu-id="7a3b9-128">Vendég operációs rendszer hello tartalmaz hello Tudásbázis [3098779 frissítése](https://support.microsoft.com/kb/3098779) és [3097997 frissítése](https://support.microsoft.com/kb/3097997).</span><span class="sxs-lookup"><span data-stu-id="7a3b9-128">hello Guest OS includes hello Knowledge Base [update 3098779](https://support.microsoft.com/kb/3098779) and [update 3097997](https://support.microsoft.com/kb/3097997).</span></span> <span data-ttu-id="7a3b9-129">Probléma akkor fordulhat elő, ha .NET 4.6 hello Tudásbázis frissítések felett van telepítve a .NET-alkalmazások futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="7a3b9-129">Issues can occur when you run your .NET applications if .NET 4.6 is installed on top of hello Knowledge Base updates.</span></span> <span data-ttu-id="7a3b9-130">Ezek a problémák tooavoid 4.6-os verzió, hanem .NET 4.6.1 telepítése.</span><span class="sxs-lookup"><span data-stu-id="7a3b9-130">tooavoid these issues, install .NET 4.6.1 rather than version 4.6.</span></span> <span data-ttu-id="7a3b9-131">További információkért lásd: hello [Tudásbázis 3118750](https://support.microsoft.com/kb/3118750).</span><span class="sxs-lookup"><span data-stu-id="7a3b9-131">For more information, see hello [Knowledge Base article 3118750](https://support.microsoft.com/kb/3118750).</span></span>
> 
> 

![Szerepkör tartalmak installer-fájlok][1]

## <a name="define-startup-tasks-for-your-roles"></a><span data-ttu-id="7a3b9-133">Adja meg a szerepkörök feladatainak indítása</span><span class="sxs-lookup"><span data-stu-id="7a3b9-133">Define startup tasks for your roles</span></span>
<span data-ttu-id="7a3b9-134">Használhatja az indítási feladatok tooperform műveletek szerepkör indítása előtt.</span><span class="sxs-lookup"><span data-stu-id="7a3b9-134">You can use startup tasks tooperform operations before a role starts.</span></span> <span data-ttu-id="7a3b9-135">Hello .NET-keretrendszer telepítése, mert hello indítási tevékenységhez részét biztosítja, hogy hello keretrendszer telepítve van, bármely alkalmazás kód futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="7a3b9-135">Installing hello .NET Framework as part of hello startup task ensures that hello framework is installed before any application code is run.</span></span> <span data-ttu-id="7a3b9-136">Indítási feladatok további információkért lásd: [indítási feladatok futtatása az Azure-ban](cloud-services-startup-tasks.md).</span><span class="sxs-lookup"><span data-stu-id="7a3b9-136">For more information on startup tasks, see [Run startup tasks in Azure](cloud-services-startup-tasks.md).</span></span> 

1. <span data-ttu-id="7a3b9-137">Adja hozzá a következő tartalom toohello ServiceDefinition.csdef fájl alapján hello hello **webrole típusról** vagy **WorkerRole** csomópont összes szerepkörök:</span><span class="sxs-lookup"><span data-stu-id="7a3b9-137">Add hello following content toohello ServiceDefinition.csdef file under hello **WebRole** or **WorkerRole** node for all roles:</span></span>
   
    ```xml
    <LocalResources>
      <LocalStorage name="NETFXInstall" sizeInMB="1024" cleanOnRoleRecycle="false" />
    </LocalResources>    
    <Startup>
      <Task commandLine="install.cmd" executionContext="elevated" taskType="simple">
        <Environment>
          <Variable name="PathToNETFXInstall">
            <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='NETFXInstall']/@path" />
          </Variable>
          <Variable name="ComputeEmulatorRunning">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
        </Environment>
      </Task>
    </Startup>
    ```
   
    <span data-ttu-id="7a3b9-138">hello előző konfigurációs hello konzol parancs futtatása `install.cmd` rendelkező rendszergazdai jogosultságokkal tooinstall hello a .NET-keretrendszer.</span><span class="sxs-lookup"><span data-stu-id="7a3b9-138">hello preceding configuration runs hello console command `install.cmd` with administrator privileges tooinstall hello .NET Framework.</span></span> <span data-ttu-id="7a3b9-139">hello konfigurációs is létrehoz egy **LocalStorage** nevű elem **NETFXInstall**.</span><span class="sxs-lookup"><span data-stu-id="7a3b9-139">hello configuration also creates a **LocalStorage** element named **NETFXInstall**.</span></span> <span data-ttu-id="7a3b9-140">hello indítási parancsfájl hello ideiglenes mappa toouse a helyi tároló-erőforrás állítja be.</span><span class="sxs-lookup"><span data-stu-id="7a3b9-140">hello startup script sets hello temp folder toouse this local storage resource.</span></span> 
    
    > [!IMPORTANT]
    > <span data-ttu-id="7a3b9-141">tooensure kijavíthatja telepítési hello keretrendszer, az ezen erőforrás tooat hello mérete legalább 1024 MB.</span><span class="sxs-lookup"><span data-stu-id="7a3b9-141">tooensure correct installation of hello framework, set hello size of this resource tooat least 1,024 MB.</span></span>
    
    <span data-ttu-id="7a3b9-142">Indítási feladatokkal kapcsolatos további információkért lásd: [közös Azure Felhőszolgáltatások indítási feladatok](cloud-services-startup-tasks-common.md).</span><span class="sxs-lookup"><span data-stu-id="7a3b9-142">For more information about startup tasks, see [Common Azure Cloud Services startup tasks](cloud-services-startup-tasks-common.md).</span></span>

2. <span data-ttu-id="7a3b9-143">Hozzon létre egy fájlt **install.cmd** , és adja hozzá a hello következő toohello parancsfájl telepítése.</span><span class="sxs-lookup"><span data-stu-id="7a3b9-143">Create a file named **install.cmd** and add hello following install script toohello file.</span></span>

    <span data-ttu-id="7a3b9-144">hello szkript ellenőrzi, hogy hello megadott hello .NET-keretrendszer verziója már telepítve van a hello gépen hello beállításjegyzék lekérdezésével.</span><span class="sxs-lookup"><span data-stu-id="7a3b9-144">hello script checks whether hello specified version of hello .NET Framework is already installed on hello machine by querying hello registry.</span></span> <span data-ttu-id="7a3b9-145">Ha hello .NET verziója nincs telepítve, hello .NET webes telepítő van megnyitva.</span><span class="sxs-lookup"><span data-stu-id="7a3b9-145">If hello .NET version is not installed, then hello .NET web installer is opened.</span></span> <span data-ttu-id="7a3b9-146">toohelp fellépő hibák elhárítására, hello parancsfájl naplózza az összes tevékenység toohello fájl startuptasklog-(jelenlegi dátum és idő) .txt tárolt **InstallLogs** helyi tárterület.</span><span class="sxs-lookup"><span data-stu-id="7a3b9-146">toohelp troubleshoot any issues, hello script logs all activity toohello file startuptasklog-(current date and time).txt that is stored in **InstallLogs** local storage.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="7a3b9-147">Egy egyszerű szövegszerkesztőben használja, mint a Windows Jegyzettömb toocreate hello install.cmd fájl.</span><span class="sxs-lookup"><span data-stu-id="7a3b9-147">Use a basic text editor like Windows Notepad toocreate hello install.cmd file.</span></span> <span data-ttu-id="7a3b9-148">Ha használja a Visual Studio toocreate egy szövegfájlt, és hello bővítmény too.cmd módosítása, hello fájl továbbra is tartalmazhat a UTF-8 bájtos rendelés be van jelölve.</span><span class="sxs-lookup"><span data-stu-id="7a3b9-148">If you use Visual Studio toocreate a text file and change hello extension too.cmd, hello file might still contain a UTF-8 byte order mark.</span></span> <span data-ttu-id="7a3b9-149">Ez a jel hibát okozhat, ha a hello első sor hello parancsfájl futtatása.</span><span class="sxs-lookup"><span data-stu-id="7a3b9-149">This mark can cause an error when hello first line of hello script is run.</span></span> <span data-ttu-id="7a3b9-150">tooavoid Ez a hiba, hogy hello első sor hello a parancsfájl egy el utasítást, amely hello bájt rendelés feldolgozása által figyelmen kívül hagyja.</span><span class="sxs-lookup"><span data-stu-id="7a3b9-150">tooavoid this error, make hello first line of hello script a REM statement that can be skipped by hello byte order processing.</span></span> 
    > 
    >
   
    ```cmd
    REM Set hello value of netfx tooinstall appropriate .NET Framework. 
    REM ***** tooinstall .NET 4.5.2 set hello variable netfx too"NDP452" *****
    REM ***** tooinstall .NET 4.6 set hello variable netfx too"NDP46" *****
    REM ***** tooinstall .NET 4.6.1 set hello variable netfx too"NDP461" *****
    REM ***** tooinstall .NET 4.6.2 set hello variable netfx too"NDP462" *****
    REM ***** tooinstall .NET 4.7 set hello variable netfx too"NDP47" *****
    set netfx="NDP47"

    REM ***** Set script start timestamp *****
    set timehour=%time:~0,2%
    set timestamp=%date:~-4,4%%date:~-10,2%%date:~-7,2%-%timehour: =0%%time:~3,2%
    set "log=install.cmd started %timestamp%."

    REM ***** Exit script if running in Emulator *****
    if %ComputeEmulatorRunning%=="true" goto exit

    REM ***** Needed toocorrectly install .NET 4.6.1, otherwise you may see an out of disk space error *****
    set TMP=%PathToNETFXInstall%
    set TEMP=%PathToNETFXInstall%

    REM ***** Setup .NET filenames and registry keys *****
    if %netfx%=="NDP47" goto NDP47
    if %netfx%=="NDP462" goto NDP462
    if %netfx%=="NDP461" goto NDP461
    if %netfx%=="NDP46" goto NDP46
        set "netfxinstallfile=NDP452-KB2901954-Web.exe"
        set netfxregkey="0x5cbf5"
        goto logtimestamp

    :NDP46
    set "netfxinstallfile=NDP46-KB3045560-Web.exe"
    set netfxregkey="0x6004f"
    goto logtimestamp

    :NDP461
    set "netfxinstallfile=NDP461-KB3102438-Web.exe"
    set netfxregkey="0x6040e"
    goto logtimestamp

    :NDP462
    set "netfxinstallfile=NDP462-KB3151802-Web.exe"
    set netfxregkey="0x60632"

    :NDP47
    set "netfxinstallfile=NDP47-KB3186500-Web.exe"
    set netfxregkey="0x707FE"

    :logtimestamp
    REM ***** Setup LogFile with timestamp *****
    md "%PathToNETFXInstall%\log"
    set startuptasklog="%PathToNETFXInstall%log\startuptasklog-%timestamp%.txt"
    set netfxinstallerlog="%PathToNETFXInstall%log\NetFXInstallerLog-%timestamp%"
    echo %log% >> %startuptasklog%
    echo Logfile generated at: %startuptasklog% >> %startuptasklog%
    echo TMP set to: %TMP% >> %startuptasklog%
    echo TEMP set to: %TEMP% >> %startuptasklog%

    REM ***** Check if .NET is installed *****
    echo Checking if .NET (%netfx%) is installed >> %startuptasklog%
    set /A netfxregkeydecimal=%netfxregkey%
    set foundkey=0
    FOR /F "usebackq skip=2 tokens=1,2*" %%A in (`reg query "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full" /v Release 2^>nul`) do @set /A foundkey=%%C
    echo Minimum required key: %netfxregkeydecimal% -- found key: %foundkey% >> %startuptasklog%
    if %foundkey% GEQ %netfxregkeydecimal% goto installed

    REM ***** Installing .NET *****
    echo Installing .NET with commandline: start /wait %~dp0%netfxinstallfile% /q /serialdownload /log %netfxinstallerlog%  /chainingpackage "CloudService Startup Task" >> %startuptasklog%
    start /wait %~dp0%netfxinstallfile% /q /serialdownload /log %netfxinstallerlog% /chainingpackage "CloudService Startup Task" >> %startuptasklog% 2>>&1
    if %ERRORLEVEL%== 0 goto installed
        echo .NET installer exited with code %ERRORLEVEL% >> %startuptasklog%    
        if %ERRORLEVEL%== 3010 goto restart
        if %ERRORLEVEL%== 1641 goto restart
        echo .NET (%netfx%) install failed with Error Code %ERRORLEVEL%. Further logs can be found in %netfxinstallerlog% >> %startuptasklog%

    :restart
    echo Restarting toocomplete .NET (%netfx%) installation >> %startuptasklog%
    EXIT /B %ERRORLEVEL%

    :installed
    echo .NET (%netfx%) is installed >> %startuptasklog%

    :end
    echo install.cmd completed: %date:~-4,4%%date:~-10,2%%date:~-7,2%-%timehour: =0%%time:~3,2% >> %startuptasklog%

    :exit
    EXIT /B 0
    ```
   
   > [!NOTE]
   > <span data-ttu-id="7a3b9-151">Ez a parancsfájl bemutatja, hogyan tooinstall .NET 4.5.2 vagy folytonosságát, még ha a .NET 4.5.2 már használható 4.6 verziójú hello Azure vendég operációs rendszer.</span><span class="sxs-lookup"><span data-stu-id="7a3b9-151">This script shows how tooinstall .NET 4.5.2 or version 4.6 for continuity, even though .NET 4.5.2 is already available on hello Azure Guest OS.</span></span> <span data-ttu-id="7a3b9-152">Közvetlenül telepíteni kell verziója 4.6, helyett .NET 4.6.1 hello leírtak [Tudásbázis 3118750](https://support.microsoft.com/kb/3118750).</span><span class="sxs-lookup"><span data-stu-id="7a3b9-152">You should directly install .NET 4.6.1 rather than version 4.6, as described in hello [Knowledge Base article 3118750](https://support.microsoft.com/kb/3118750).</span></span>
   > 
   > 

3. <span data-ttu-id="7a3b9-153">Hozzáadás hello install.cmd fájl tooeach szerepkör használatával **hozzáadása** > **a létező elemet** a **Megoldáskezelőben** Ez a témakör a korábban ismertetett módon.</span><span class="sxs-lookup"><span data-stu-id="7a3b9-153">Add hello install.cmd file tooeach role by using **Add** > **Existing Item** in **Solution Explorer** as described earlier in this topic.</span></span> 

    <span data-ttu-id="7a3b9-154">Ez a lépés befejezése után az összes szerepkör hello .NET telepítő és hello install.cmd fájlt kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="7a3b9-154">After this step is complete, all roles should have hello .NET installer file and hello install.cmd file.</span></span>

   ![Összes fájlját szerepkör tartalma][2]

## <a name="configure-diagnostics-tootransfer-startup-logs-tooblob-storage"></a><span data-ttu-id="7a3b9-156">Diagnosztika tootransfer indítási naplók tooBlob tároló konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7a3b9-156">Configure Diagnostics tootransfer startup logs tooBlob storage</span></span>
<span data-ttu-id="7a3b9-157">toosimplify a telepítési problémák elhárításához, az Azure Diagnostics tootransfer hello indítási által generált naplófájlokat parancsfájl vagy .NET telepítő tooAzure Blob-tároló hello is konfigurálhat.</span><span class="sxs-lookup"><span data-stu-id="7a3b9-157">toosimplify troubleshooting installation issues, you can configure Azure Diagnostics tootransfer any log files generated by hello startup script or hello .NET installer tooAzure Blob storage.</span></span> <span data-ttu-id="7a3b9-158">Ezt a módszert használva tekintheti hello naplók hello napló fájlok letöltését a Blob storage ahelyett, hogy tooremote asztali hello szerepkörhöz.</span><span class="sxs-lookup"><span data-stu-id="7a3b9-158">By using this approach, you can view hello logs by downloading hello log files from Blob storage rather than having tooremote desktop into hello role.</span></span>

<span data-ttu-id="7a3b9-159">tooconfigure diagnosztika, nyissa meg hello diagnostics.wadcfgx fájlt, és adja hozzá a következő hello alatt látható tartalmat hello **könyvtárak** csomópont:</span><span class="sxs-lookup"><span data-stu-id="7a3b9-159">tooconfigure Diagnostics, open hello diagnostics.wadcfgx file and add hello following content under hello **Directories** node:</span></span> 

```xml 
<DataSources>
 <DirectoryConfiguration containerName="netfx-install">
  <LocalResource name="NETFXInstall" relativePath="log"/>
 </DirectoryConfiguration>
</DataSources>
```

<span data-ttu-id="7a3b9-160">Az XML-kód diagnosztika tootransfer hello fájlok azt állítja be a hello hello naplókönyvtár **NETFXInstall** erőforrás toohello diagnosztikai tárfiók a hello **netfx-telepítés** blob tároló.</span><span class="sxs-lookup"><span data-stu-id="7a3b9-160">This XML configures Diagnostics tootransfer hello files in hello log directory in hello **NETFXInstall** resource toohello Diagnostics storage account in hello **netfx-install** blob container.</span></span>

## <a name="deploy-your-cloud-service"></a><span data-ttu-id="7a3b9-161">A felhőalapú szolgáltatás üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="7a3b9-161">Deploy your cloud service</span></span>
<span data-ttu-id="7a3b9-162">A felhőalapú szolgáltatás telepítésekor hello indítási feladatok hello .NET-keretrendszer telepítése, ha még nincs telepítve.</span><span class="sxs-lookup"><span data-stu-id="7a3b9-162">When you deploy your cloud service, hello startup tasks install hello .NET Framework if it's not already installed.</span></span> <span data-ttu-id="7a3b9-163">A felhőszolgáltatás szerepköreit szerepelnek hello *foglalt* állapot hello keretrendszer telepítése közben.</span><span class="sxs-lookup"><span data-stu-id="7a3b9-163">Your cloud service roles are in hello *busy* state while hello framework is being installed.</span></span> <span data-ttu-id="7a3b9-164">Ha hello keretrendszer telepítési újraindítást igényel, hello szolgáltatás szerepkörök is újraindulhat.</span><span class="sxs-lookup"><span data-stu-id="7a3b9-164">If hello framework installation requires a restart, hello service roles might also restart.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="7a3b9-165">További források</span><span class="sxs-lookup"><span data-stu-id="7a3b9-165">Additional resources</span></span>
* <span data-ttu-id="7a3b9-166">[Hello .NET-keretrendszer telepítése][Installing hello .NET Framework]</span><span class="sxs-lookup"><span data-stu-id="7a3b9-166">[Installing hello .NET Framework][Installing hello .NET Framework]</span></span>
* <span data-ttu-id="7a3b9-167">[Határozza meg, melyik .NET-keretrendszer verziója telepítve van][How to: Determine Which .NET Framework Versions Are Installed]</span><span class="sxs-lookup"><span data-stu-id="7a3b9-167">[Determine which .NET Framework versions are installed][How to: Determine Which .NET Framework Versions Are Installed]</span></span>
* <span data-ttu-id="7a3b9-168">[Hibaelhárítás a .NET-keretrendszer telepítése][Troubleshooting .NET Framework Installations]</span><span class="sxs-lookup"><span data-stu-id="7a3b9-168">[Troubleshooting .NET Framework installations][Troubleshooting .NET Framework Installations]</span></span>

[How to: Determine Which .NET Framework Versions Are Installed]: https://msdn.microsoft.com/library/hh925568.aspx
[Installing hello .NET Framework]: https://msdn.microsoft.com/library/5a4x27ek.aspx
[Troubleshooting .NET Framework Installations]: https://msdn.microsoft.com/library/hh925569.aspx

<!--Image references-->
[1]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithinstallerfiles.png
[2]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithallfiles.png
