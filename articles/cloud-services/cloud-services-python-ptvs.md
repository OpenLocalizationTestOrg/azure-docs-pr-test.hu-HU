---
title: "aaaGet a Python és az Azure Cloud Services használatába |} Microsoft Docs"
description: "Python Tools for Visual Studio toocreate Azure felhőszolgáltatások, például webes és feldolgozói szerepkörök használatával áttekintése."
services: cloud-services
documentationcenter: python
author: thraka
manager: timlt
editor: 
ms.assetid: 5489405d-6fa9-4b11-a161-609103cbdc18
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: hero-article
ms.date: 07/18/2017
ms.author: adegeo
ms.openlocfilehash: f5fd85e754839f146abe912351c59dc4a148c990
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="python-web-and-worker-roles-with-python-tools-for-visual-studio"></a><span data-ttu-id="0a673-103">Python webes és feldolgozói szerepkörök a Visual Studio eszközzel</span><span class="sxs-lookup"><span data-stu-id="0a673-103">Python web and worker roles with Python Tools for Visual Studio</span></span>

<span data-ttu-id="0a673-104">Ez a cikk a Python webes és feldolgozói szerepkörök [Python Tools for Visual Studio][Python Tools for Visual Studio] eszközben történő használatát ismerteti.</span><span class="sxs-lookup"><span data-stu-id="0a673-104">This article provides an overview of using Python web and worker roles using [Python Tools for Visual Studio][Python Tools for Visual Studio].</span></span> <span data-ttu-id="0a673-105">Megtudhatja, hogyan toouse Visual Studio toocreate és központi telepítése egy alapszintű felhőalapú szolgáltatás, amely a Pythont használja.</span><span class="sxs-lookup"><span data-stu-id="0a673-105">Learn how toouse Visual Studio toocreate and deploy a basic Cloud Service that uses Python.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0a673-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="0a673-106">Prerequisites</span></span>
* [<span data-ttu-id="0a673-107">Visual Studio 2013, 2015 vagy 2017</span><span class="sxs-lookup"><span data-stu-id="0a673-107">Visual Studio 2013, 2015, or 2017</span></span>](https://www.visualstudio.com/)
* <span data-ttu-id="0a673-108">[Python Tools for Visual Studio][Python Tools for Visual Studio] (PTVS)</span><span class="sxs-lookup"><span data-stu-id="0a673-108">[Python Tools for Visual Studio][Python Tools for Visual Studio] (PTVS)</span></span>
* <span data-ttu-id="0a673-109">[Azure SDK Tools for VS 2013][Azure SDK Tools for VS 2013] vagy</span><span class="sxs-lookup"><span data-stu-id="0a673-109">[Azure SDK Tools for VS 2013][Azure SDK Tools for VS 2013] or</span></span>  
<span data-ttu-id="0a673-110">[Azure SDK Tools for VS 2015][Azure SDK Tools for VS 2015] vagy</span><span class="sxs-lookup"><span data-stu-id="0a673-110">[Azure SDK Tools for VS 2015][Azure SDK Tools for VS 2015] or</span></span>  
<span data-ttu-id="0a673-111">[Azure SDK Tools for VS 2017][Azure SDK Tools for VS 2017]</span><span class="sxs-lookup"><span data-stu-id="0a673-111">[Azure SDK Tools for VS 2017][Azure SDK Tools for VS 2017]</span></span>
* <span data-ttu-id="0a673-112">[Python 2.7 32 bites][Python 2.7 32-bit] vagy [Python 3.5 32 bites][Python 3.5 32-bit]</span><span class="sxs-lookup"><span data-stu-id="0a673-112">[Python 2.7 32-bit][Python 2.7 32-bit] or [Python 3.5 32-bit][Python 3.5 32-bit]</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

## <a name="what-are-python-web-and-worker-roles"></a><span data-ttu-id="0a673-113">Mik a Python webes és feldolgozói szerepkörök?</span><span class="sxs-lookup"><span data-stu-id="0a673-113">What are Python web and worker roles?</span></span>
<span data-ttu-id="0a673-114">Az Azure három számítási modellt biztosít az alkalmazások futtatásához: [Web Apps szolgáltatás az Azure App Service-ben][execution model-web sites], [Azure Virtual Machines][execution model-vms] és [Azure Cloud Services][execution model-cloud services].</span><span class="sxs-lookup"><span data-stu-id="0a673-114">Azure provides three compute models for running applications: [Web Apps feature in Azure App Service][execution model-web sites], [Azure Virtual Machines][execution model-vms], and [Azure Cloud Services][execution model-cloud services].</span></span> <span data-ttu-id="0a673-115">Mindhárom modell támogatja a Python eszközt.</span><span class="sxs-lookup"><span data-stu-id="0a673-115">All three models support Python.</span></span> <span data-ttu-id="0a673-116">A webes és feldolgozói szerepköröket is tartalmazó Cloud Services *platformszolgáltatást (PaaS)* kínál.</span><span class="sxs-lookup"><span data-stu-id="0a673-116">Cloud Services, which include web and worker roles, provide *Platform as a Service (PaaS)*.</span></span> <span data-ttu-id="0a673-117">Belül egy felhőalapú szolgáltatás a webes szerepkör lehetővé teszi egy dedikált Internet Information Services (IIS) webes server toohost előtérbeli webes, míg a feldolgozói szerepkör aszinkron, hosszan futó vagy folyamatos feladatokat futtat függetlenül a felhasználói interakcióktól vagy bemenettől futtatható.</span><span class="sxs-lookup"><span data-stu-id="0a673-117">Within a cloud service, a web role provides a dedicated Internet Information Services (IIS) web server toohost front end web applications, while a worker role can run asynchronous, long-running, or perpetual tasks independent of user interaction or input.</span></span>

<span data-ttu-id="0a673-118">További információ: [Mi az a Cloud Service?].</span><span class="sxs-lookup"><span data-stu-id="0a673-118">For more information, see [What is a Cloud Service?].</span></span>

> [!NOTE]
> <span data-ttu-id="0a673-119">*Egy egyszerű webhely toobuild szüksége?*</span><span class="sxs-lookup"><span data-stu-id="0a673-119">*Looking toobuild a simple website?*</span></span>
> <span data-ttu-id="0a673-120">Ha csak egy egyszerű webhely előtér, fontolja meg az Azure App Service hello egyszerűsített Web Apps szolgáltatásának használatát.</span><span class="sxs-lookup"><span data-stu-id="0a673-120">If your scenario involves just a simple website front end, consider using hello lightweight Web Apps feature in Azure App Service.</span></span> <span data-ttu-id="0a673-121">A webhely növekszik és a követelmények változnak könnyedén frissíthet Cloud Service tooa.</span><span class="sxs-lookup"><span data-stu-id="0a673-121">You can easily upgrade tooa Cloud Service as your website grows and your requirements change.</span></span> <span data-ttu-id="0a673-122">Lásd: hello <a href="/develop/python/">Python fejlesztői központ</a> az Azure App Service-ben hello Web Apps szolgáltatásának fejlesztését ismertető cikkeket.</span><span class="sxs-lookup"><span data-stu-id="0a673-122">See hello <a href="/develop/python/">Python Developer Center</a> for articles that cover development of hello Web Apps feature in Azure App Service.</span></span>
> <br />
> 
> 

## <a name="project-creation"></a><span data-ttu-id="0a673-123">Projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="0a673-123">Project creation</span></span>
<span data-ttu-id="0a673-124">A Visual Studio kiválaszthatja **Azure Cloud Service** a hello **új projekt** párbeszédpanel **Python**.</span><span class="sxs-lookup"><span data-stu-id="0a673-124">In Visual Studio, you can select **Azure Cloud Service** in hello **New Project** dialog box, under **Python**.</span></span>

![Új projekt párbeszédpanel](./media/cloud-services-python-ptvs/new-project-cloud-service.png)

<span data-ttu-id="0a673-126">Hello Azure Cloud Service varázslóban létrehozhat új webes és feldolgozói szerepköröket.</span><span class="sxs-lookup"><span data-stu-id="0a673-126">In hello Azure Cloud Service wizard, you can create new web and worker roles.</span></span>

![Azure Cloud Service párbeszédpanel](./media/cloud-services-python-ptvs/new-service-wizard.png)

<span data-ttu-id="0a673-128">hello feldolgozói szerepkör sablonok bolierplate kóddal tooconnect tooan Azure storage-fiók vagy Azure Service Bus tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="0a673-128">hello worker role template comes with boilerplate code tooconnect tooan Azure storage account or Azure Service Bus.</span></span>

![Cloud Service megoldás](./media/cloud-services-python-ptvs/worker.png)

<span data-ttu-id="0a673-130">Bármikor hozzáadhat webes vagy feldolgozói szerepkörök tooan létező felhőalapú szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="0a673-130">You can add web or worker roles tooan existing cloud service at any time.</span></span>  <span data-ttu-id="0a673-131">Válassza ki a tooadd meglévő projekteket a megoldásban, vagy hozzon létre újakat.</span><span class="sxs-lookup"><span data-stu-id="0a673-131">You can choose tooadd existing projects in your solution, or create new ones.</span></span>

![Szerepkör hozzáadása parancs](./media/cloud-services-python-ptvs/add-new-or-existing-role.png)

<span data-ttu-id="0a673-133">A felhőszolgáltatás tartalmazhat különböző nyelveken kialakított szerepköröket.</span><span class="sxs-lookup"><span data-stu-id="0a673-133">Your cloud service can contain roles implemented in different languages.</span></span>  <span data-ttu-id="0a673-134">Például kialakíthat egy Python webes szerepkört Django, Python vagy C# feldolgozói szerepkör használatával.</span><span class="sxs-lookup"><span data-stu-id="0a673-134">For example, you can have a Python web role implemented using Django, with Python, or with C# worker roles.</span></span>  <span data-ttu-id="0a673-135">A szerepkörök között egyszerűen kommunikálhat Service Bus üzenetsorok vagy tárolási sorok használatával.</span><span class="sxs-lookup"><span data-stu-id="0a673-135">You can easily communicate between your roles using Service Bus queues or storage queues.</span></span>

## <a name="install-python-on-hello-cloud-service"></a><span data-ttu-id="0a673-136">Python hello felhőalapú szolgáltatás telepítése</span><span class="sxs-lookup"><span data-stu-id="0a673-136">Install Python on hello cloud service</span></span>
> [!WARNING]
> <span data-ttu-id="0a673-137">(időben hello Ez a cikk utolsó frissítés), a Visual Studio telepített hello beállítása parancsfájlok nem működnek.</span><span class="sxs-lookup"><span data-stu-id="0a673-137">hello setup scripts that are installed with Visual Studio (at hello time this article was last updated) do not work.</span></span> <span data-ttu-id="0a673-138">A megoldást ez a szakasz ismerteti.</span><span class="sxs-lookup"><span data-stu-id="0a673-138">This section describes a workaround.</span></span>
> 
> 

<span data-ttu-id="0a673-139">hello hello beállítása parancsfájlok fő problémája, hogy azok ne telepítse a python.</span><span class="sxs-lookup"><span data-stu-id="0a673-139">hello main problem with hello setup scripts is that they do not install python.</span></span> <span data-ttu-id="0a673-140">Először határozza meg két [indítási feladatok](cloud-services-startup-tasks.md) a hello [ServiceDefinition.csdef](cloud-services-model-and-package.md#servicedefinitioncsdef) fájlt.</span><span class="sxs-lookup"><span data-stu-id="0a673-140">First, define two [startup tasks](cloud-services-startup-tasks.md) in hello [ServiceDefinition.csdef](cloud-services-model-and-package.md#servicedefinitioncsdef) file.</span></span> <span data-ttu-id="0a673-141">a feladat első hello (**PrepPython.ps1**) letölti és telepíti a Python-futtatókörnyezet hello.</span><span class="sxs-lookup"><span data-stu-id="0a673-141">hello first task (**PrepPython.ps1**) downloads and installs hello Python runtime.</span></span> <span data-ttu-id="0a673-142">hello második feladata (**PipInstaller.ps1**) pip tooinstall összes függőséget, előfordulhat, hogy futtatja.</span><span class="sxs-lookup"><span data-stu-id="0a673-142">hello second task (**PipInstaller.ps1**) runs pip tooinstall any dependencies you may have.</span></span>

<span data-ttu-id="0a673-143">a következő parancsfájlok hello készültek, célzás Python 3.5-ös verzióját.</span><span class="sxs-lookup"><span data-stu-id="0a673-143">hello following scripts were written targeting Python 3.5.</span></span> <span data-ttu-id="0a673-144">Ha azt szeretné, hogy toouse hello verzió a python, set hello 2.x **PYTHON2** változó fájl túl**a** hello két indítási feladatok és hello futásidejű feladat: `<Variable name="PYTHON2" value="<mark>on</mark>" />`.</span><span class="sxs-lookup"><span data-stu-id="0a673-144">If you want toouse hello version 2.x of python, set hello **PYTHON2** variable file too**on** for hello two startup tasks and hello runtime task: `<Variable name="PYTHON2" value="<mark>on</mark>" />`.</span></span>

```xml
<Startup>

  <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PrepPython.ps1">
    <Environment>
      <Variable name="EMULATED">
        <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
      </Variable>
      <Variable name="PYTHON2" value="off" />
    </Environment>
  </Task>

  <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PipInstaller.ps1">
    <Environment>
      <Variable name="EMULATED">
        <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
      </Variable>
      <Variable name="PYTHON2" value="off" />
    </Environment>

  </Task>

</Startup>
```

<span data-ttu-id="0a673-145">Hello **PYTHON2** és **PYPATH** változók toohello munkavégző indítási tevékenységhez hozzá kell adni.</span><span class="sxs-lookup"><span data-stu-id="0a673-145">hello **PYTHON2** and **PYPATH** variables must be added toohello worker startup task.</span></span> <span data-ttu-id="0a673-146">Hello **PYPATH** változó csak akkor használatos, ha hello **PYTHON2** változó értéke túl**a**.</span><span class="sxs-lookup"><span data-stu-id="0a673-146">hello **PYPATH** variable is only used if hello **PYTHON2** variable is set too**on**.</span></span>

```xml
<Runtime>
  <Environment>
    <Variable name="EMULATED">
      <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
    </Variable>
    <Variable name="PYTHON2" value="off" />
    <Variable name="PYPATH" value="%SystemDrive%\Python27" />
  </Environment>
  <EntryPoint>
    <ProgramEntryPoint commandLine="bin\ps.cmd LaunchWorker.ps1" setReadyOnProcessStart="true" />
  </EntryPoint>
</Runtime>
```

#### <a name="sample-servicedefinitioncsdef"></a><span data-ttu-id="0a673-147">Mintául szolgáló ServiceDefinition.csdef</span><span class="sxs-lookup"><span data-stu-id="0a673-147">Sample ServiceDefinition.csdef</span></span>
```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceDefinition name="AzureCloudServicePython" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2015-04.2.6">
  <WorkerRole name="WorkerRole1" vmsize="Small">
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" />
      <Setting name="Python2" />
    </ConfigurationSettings>
    <Startup>
      <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PrepPython.ps1">
        <Environment>
          <Variable name="EMULATED">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
          <Variable name="PYTHON2" value="off" />
        </Environment>
      </Task>
      <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PipInstaller.ps1">
        <Environment>
          <Variable name="EMULATED">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
          <Variable name="PYTHON2" value="off" />
        </Environment>
      </Task>
    </Startup>
    <Runtime>
      <Environment>
        <Variable name="EMULATED">
          <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
        </Variable>
        <Variable name="PYTHON2" value="off" />
        <Variable name="PYPATH" value="%SystemDrive%\Python27" />
      </Environment>
      <EntryPoint>
        <ProgramEntryPoint commandLine="bin\ps.cmd LaunchWorker.ps1" setReadyOnProcessStart="true" />
      </EntryPoint>
    </Runtime>
    <Imports>
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
  </WorkerRole>
</ServiceDefinition>
```



<span data-ttu-id="0a673-148">Ezután hozzon létre hello **PrepPython.ps1** és **PipInstaller.ps1** hello fájlok **. / bin** mappában található a szerepkör.</span><span class="sxs-lookup"><span data-stu-id="0a673-148">Next, create hello **PrepPython.ps1** and **PipInstaller.ps1** files in hello **./bin** folder of your role.</span></span>

#### <a name="preppythonps1"></a><span data-ttu-id="0a673-149">PrepPython.ps1</span><span class="sxs-lookup"><span data-stu-id="0a673-149">PrepPython.ps1</span></span>
<span data-ttu-id="0a673-150">Ez a parancsfájl telepíti a Pythont.</span><span class="sxs-lookup"><span data-stu-id="0a673-150">This script installs python.</span></span> <span data-ttu-id="0a673-151">Ha hello **PYTHON2** környezeti változó értéke túl**a**, majd a Python 2.7 telepítve van, ellenkező esetben a Python 3.5 telepítve van.</span><span class="sxs-lookup"><span data-stu-id="0a673-151">If hello **PYTHON2** environment variable is set too**on**, then Python 2.7 is installed, otherwise Python 3.5 is installed.</span></span>

```powershell
$is_emulated = $env:EMULATED -eq "true"
$is_python2 = $env:PYTHON2 -eq "on"
$nl = [Environment]::NewLine

if (-not $is_emulated){
    Write-Output "Checking if python is installed...$nl"
    if ($is_python2) {
        & "${env:SystemDrive}\Python27\python.exe"  -V | Out-Null
    }
    else {
        py -V | Out-Null
    }

    if (-not $?) {

        $url = "https://www.python.org/ftp/python/3.5.2/python-3.5.2-amd64.exe"
        $outFile = "${env:TEMP}\python-3.5.2-amd64.exe"

        if ($is_python2) {
            $url = "https://www.python.org/ftp/python/2.7.12/python-2.7.12.amd64.msi"
            $outFile = "${env:TEMP}\python-2.7.12.amd64.msi"
        }

        Write-Output "Not found, downloading $url too$outFile$nl"
        Invoke-WebRequest $url -OutFile $outFile
        Write-Output "Installing$nl"

        if ($is_python2) {
            Start-Process msiexec.exe -ArgumentList "/q", "/i", "$outFile", "ALLUSERS=1" -Wait
        }
        else {
            Start-Process "$outFile" -ArgumentList "/quiet", "InstallAllUsers=1" -Wait
        }

        Write-Output "Done$nl"
    }
    else {
        Write-Output "Already installed"
    }
}
```

#### <a name="pipinstallerps1"></a><span data-ttu-id="0a673-152">PipInstaller.ps1</span><span class="sxs-lookup"><span data-stu-id="0a673-152">PipInstaller.ps1</span></span>
<span data-ttu-id="0a673-153">Ez a parancsfájl meghívja a pip, és telepíti a teljes hello függőségek hello **requirements.txt** fájlt.</span><span class="sxs-lookup"><span data-stu-id="0a673-153">This script calls up pip and installs all of hello dependencies in hello **requirements.txt** file.</span></span> <span data-ttu-id="0a673-154">Ha hello **PYTHON2** környezeti változó értéke túl**a**, majd a Python 2.7 szolgál, ellenkező esetben a Python 3.5 szolgál.</span><span class="sxs-lookup"><span data-stu-id="0a673-154">If hello **PYTHON2** environment variable is set too**on**, then Python 2.7 is used, otherwise Python 3.5 is used.</span></span>

```powershell
$is_emulated = $env:EMULATED -eq "true"
$is_python2 = $env:PYTHON2 -eq "on"
$nl = [Environment]::NewLine

if (-not $is_emulated){
    Write-Output "Checking if requirements.txt exists$nl"
    if (Test-Path ..\requirements.txt) {
        Write-Output "Found. Processing pip$nl"

        if ($is_python2) {
            & "${env:SystemDrive}\Python27\python.exe" -m pip install -r ..\requirements.txt
        }
        else {
            py -m pip install -r ..\requirements.txt
        }

        Write-Output "Done$nl"
    }
    else {
        Write-Output "Not found$nl"
    }
}
```

#### <a name="modify-launchworkerps1"></a><span data-ttu-id="0a673-155">A LaunchWorker.ps1 módosítása</span><span class="sxs-lookup"><span data-stu-id="0a673-155">Modify LaunchWorker.ps1</span></span>
> [!NOTE]
> <span data-ttu-id="0a673-156">Hello esetében egy **feldolgozói szerepkör** projektben **LauncherWorker.ps1** szükséges tooexecute hello indítási fájl.</span><span class="sxs-lookup"><span data-stu-id="0a673-156">In hello case of a **worker role** project, **LauncherWorker.ps1** file is required tooexecute hello startup file.</span></span> <span data-ttu-id="0a673-157">Az egy **webes szerepkör** projektre, hello indítási fájl helyette meghatározott hello projekt tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="0a673-157">In a **web role** project, hello startup file is instead defined in hello project properties.</span></span>
> 
> 

<span data-ttu-id="0a673-158">Hello **bin\LaunchWorker.ps1** eredetileg toodo nagy mennyiségű előkészítő munka, de valójában nem működik.</span><span class="sxs-lookup"><span data-stu-id="0a673-158">hello **bin\LaunchWorker.ps1** was originally created toodo a lot of prep work but it doesn't really work.</span></span> <span data-ttu-id="0a673-159">Cserélje ki a következő parancsfájl hello hello tartalmát, az adott fájlban.</span><span class="sxs-lookup"><span data-stu-id="0a673-159">Replace hello contents in that file with hello following script.</span></span>

<span data-ttu-id="0a673-160">Ez a parancsfájl meghívja a hello **worker.py** fájl a python projektből.</span><span class="sxs-lookup"><span data-stu-id="0a673-160">This script calls hello **worker.py** file from your python project.</span></span> <span data-ttu-id="0a673-161">Ha hello **PYTHON2** környezeti változó értéke túl**a**, majd a Python 2.7 szolgál, ellenkező esetben a Python 3.5 szolgál.</span><span class="sxs-lookup"><span data-stu-id="0a673-161">If hello **PYTHON2** environment variable is set too**on**, then Python 2.7 is used, otherwise Python 3.5 is used.</span></span>

```powershell
$is_emulated = $env:EMULATED -eq "true"
$is_python2 = $env:PYTHON2 -eq "on"
$nl = [Environment]::NewLine

if (-not $is_emulated)
{
    Write-Output "Running worker.py$nl"

    if ($is_python2) {
        cd..
        iex "$env:PYPATH\python.exe worker.py"
    }
    else {
        cd..
        iex "py worker.py"
    }
}
else
{
    Write-Output "Running (EMULATED) worker.py$nl"

    # Customize tooyour local dev environment

    if ($is_python2) {
        cd..
        iex "$env:PYPATH\python.exe worker.py"
    }
    else {
        cd..
        iex "py worker.py"
    }
}
```

#### <a name="pscmd"></a><span data-ttu-id="0a673-162">ps.cmd</span><span class="sxs-lookup"><span data-stu-id="0a673-162">ps.cmd</span></span>
<span data-ttu-id="0a673-163">hozza létre a Visual Studio sablonok hello egy **ps.cmd** hello fájlban **. / bin** mappát.</span><span class="sxs-lookup"><span data-stu-id="0a673-163">hello Visual Studio templates should have created a **ps.cmd** file in hello **./bin** folder.</span></span> <span data-ttu-id="0a673-164">A parancsfájl kimenő hello PowerShell meghívja a fenti burkoló parancsfájlok és hello PowerShell burkoló nevű hello neve alapján-naplózást végez.</span><span class="sxs-lookup"><span data-stu-id="0a673-164">This shell script calls out hello PowerShell wrapper scripts above and provides logging based on hello name of hello PowerShell wrapper called.</span></span> <span data-ttu-id="0a673-165">Ha a fájl nem jött létre, itt láthatja, minek kéne benne lennie.</span><span class="sxs-lookup"><span data-stu-id="0a673-165">If this file wasn't created, here is what should be in it.</span></span> 

```bat
@echo off

cd /D %~dp0

if not exist "%DiagnosticStore%\LogFiles" mkdir "%DiagnosticStore%\LogFiles"
%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe -ExecutionPolicy Unrestricted -File %* >> "%DiagnosticStore%\LogFiles\%~n1.txt" 2>> "%DiagnosticStore%\LogFiles\%~n1.err.txt"
```



## <a name="run-locally"></a><span data-ttu-id="0a673-166">Helyi futtatás</span><span class="sxs-lookup"><span data-stu-id="0a673-166">Run locally</span></span>
<span data-ttu-id="0a673-167">Ha a felhőszolgáltatási projektet állítja be hello kezdőprojektként, és nyomja le az F5 billentyűt, hello felhőszolgáltatás hello helyi Azure emulátorban futtatja.</span><span class="sxs-lookup"><span data-stu-id="0a673-167">If you set your cloud service project as hello startup project and press F5, hello cloud service runs in hello local Azure emulator.</span></span>

<span data-ttu-id="0a673-168">Bár a PVTS támogatja indítást hello emulátorban, a hibakeresés (például töréspontok keresése) nem működik.</span><span class="sxs-lookup"><span data-stu-id="0a673-168">Although PTVS supports launching in hello emulator, debugging (for example, breakpoints) does not work.</span></span>

<span data-ttu-id="0a673-169">toodebug a webes és feldolgozói szerepkörök állíthat hello szerepkör projekt hello indítási projektként, és helyette, amely debug.</span><span class="sxs-lookup"><span data-stu-id="0a673-169">toodebug your web and worker roles, you can set hello role project as hello startup project and debug that instead.</span></span>  <span data-ttu-id="0a673-170">Beállíthat több kiindulási projektet is.</span><span class="sxs-lookup"><span data-stu-id="0a673-170">You can also set multiple startup projects.</span></span>  <span data-ttu-id="0a673-171">Kattintson a jobb gombbal a hello megoldás, és válassza ki **indítási projektek beállítása**.</span><span class="sxs-lookup"><span data-stu-id="0a673-171">Right-click hello solution and then select **Set StartUp Projects**.</span></span>

![A megoldás kiindulási projektjének tulajdonságai](./media/cloud-services-python-ptvs/startup.png)

## <a name="publish-tooazure"></a><span data-ttu-id="0a673-173">TooAzure közzététele</span><span class="sxs-lookup"><span data-stu-id="0a673-173">Publish tooAzure</span></span>
<span data-ttu-id="0a673-174">toopublish, kattintson a jobb gombbal a hello megoldás hello felhőszolgáltatási projektre, és válassza ki **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="0a673-174">toopublish, right-click hello cloud service project in hello solution and then select **Publish**.</span></span>

![Bejelentkezés a Microsoft Azure közzétételhez](./media/cloud-services-python-ptvs/publish-sign-in.png)

<span data-ttu-id="0a673-176">Hajtsa végre a hello varázsló.</span><span class="sxs-lookup"><span data-stu-id="0a673-176">Follow hello wizard.</span></span> <span data-ttu-id="0a673-177">Szükség esetén engedélyezze a távoli asztal használatát.</span><span class="sxs-lookup"><span data-stu-id="0a673-177">If you need to, enable remote desktop.</span></span> <span data-ttu-id="0a673-178">A távoli asztal akkor hasznos, ha szüksége toodebug valamit.</span><span class="sxs-lookup"><span data-stu-id="0a673-178">Remote desktop is helpful when you need toodebug something.</span></span>

<span data-ttu-id="0a673-179">Ha elkészült a beállítások konfigurálásával, kattintson a **Közzététel** gombra.</span><span class="sxs-lookup"><span data-stu-id="0a673-179">When you are done configuring settings, click **Publish**.</span></span>

<span data-ttu-id="0a673-180">Hello kimeneti ablakban egy folyamatjelző jelenik meg, majd hello Microsoft Azure tevékenységnapló ablak látható.</span><span class="sxs-lookup"><span data-stu-id="0a673-180">Some progress appears in hello output window, then you'll see hello Microsoft Azure Activity Log window.</span></span>

![Microsoft Azure tevékenységnapló ablak](./media/cloud-services-python-ptvs/publish-activity-log.png)

<span data-ttu-id="0a673-182">Központi telepítés több percet toocomplete, majd a webes vesz igénybe, és/vagy feldolgozói szerepkörök futtatásához Azure!</span><span class="sxs-lookup"><span data-stu-id="0a673-182">Deployment takes several minutes toocomplete, then your web and/or worker roles run on Azure!</span></span>

### <a name="investigate-logs"></a><span data-ttu-id="0a673-183">Naplók vizsgálata</span><span class="sxs-lookup"><span data-stu-id="0a673-183">Investigate logs</span></span>
<span data-ttu-id="0a673-184">Miután hello felhőalapú szolgáltatás virtuális gép elindul, és telepíti a Python, vessen egy pillantást hello naplók toofind esetleges hibaüzenetek.</span><span class="sxs-lookup"><span data-stu-id="0a673-184">After hello cloud service virtual machine starts up and installs Python, you can look at hello logs toofind any failure messages.</span></span> <span data-ttu-id="0a673-185">Ezek a naplók találhatók hello **C:\Resources\Directory\\{szerepkör} \LogFiles** mappa.</span><span class="sxs-lookup"><span data-stu-id="0a673-185">These logs are located in hello **C:\Resources\Directory\\{role}\LogFiles** folder.</span></span> <span data-ttu-id="0a673-186">**PrepPython.err.txt** a amikor hello parancsfájl kísérli meg toodetect, ha telepítve van a Python rendelkezik legalább egy hibát és **PipInstaller.err.txt** panaszkodnak előfordulhat, hogy a pip elavult verziójára.</span><span class="sxs-lookup"><span data-stu-id="0a673-186">**PrepPython.err.txt** has at least one error in it from when hello script tries toodetect if Python is installed and **PipInstaller.err.txt** may complain about an outdated version of pip.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0a673-187">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0a673-187">Next steps</span></span>
<span data-ttu-id="0a673-188">Webes és feldolgozói szerepkörök Python Tools for Visual Studio használatával kapcsolatos részletes információkért lásd: hello PVTS dokumentációban:</span><span class="sxs-lookup"><span data-stu-id="0a673-188">For more detailed information about working with web and worker roles in Python Tools for Visual Studio, see hello PTVS documentation:</span></span>

* <span data-ttu-id="0a673-189">[Cloud Service-projektek][Cloud Service Projects]</span><span class="sxs-lookup"><span data-stu-id="0a673-189">[Cloud Service Projects][Cloud Service Projects]</span></span>

<span data-ttu-id="0a673-190">A webes és feldolgozói szerepköröket, például az Azure Storage vagy a Service Bus, az Azure-szolgáltatások használatával kapcsolatos további részletekért tekintse meg a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="0a673-190">For more details about using Azure services from your web and worker roles, such as using Azure Storage or Service Bus, see hello following articles:</span></span>

* <span data-ttu-id="0a673-191">[Blob Service][Blob Service]</span><span class="sxs-lookup"><span data-stu-id="0a673-191">[Blob Service][Blob Service]</span></span>
* <span data-ttu-id="0a673-192">[Table Service][Table Service]</span><span class="sxs-lookup"><span data-stu-id="0a673-192">[Table Service][Table Service]</span></span>
* <span data-ttu-id="0a673-193">[Queue szolgáltatás][Queue Service]</span><span class="sxs-lookup"><span data-stu-id="0a673-193">[Queue Service][Queue Service]</span></span>
* <span data-ttu-id="0a673-194">[Service Bus-üzenetsorok][Service Bus Queues]</span><span class="sxs-lookup"><span data-stu-id="0a673-194">[Service Bus Queues][Service Bus Queues]</span></span>
* <span data-ttu-id="0a673-195">[Service Bus-témakörök][Service Bus Topics]</span><span class="sxs-lookup"><span data-stu-id="0a673-195">[Service Bus Topics][Service Bus Topics]</span></span>

<!--Link references-->

[Mi az a Cloud Service?]: cloud-services-choose-me.md
[execution model-web sites]: ../app-service-web/app-service-web-overview.md
[execution model-vms]:../virtual-machines/windows/overview.md
[execution model-cloud services]: cloud-services-choose-me.md
[Python Developer Center]: /develop/python/

[Blob Service]:../storage/blobs/storage-python-how-to-use-blob-storage.md
[Queue Service]: ../storage/queues/storage-python-how-to-use-queue-storage.md
[Table Service]:../cosmos-db/table-storage-how-to-use-python.md
[Service Bus Queues]: ../service-bus-messaging/service-bus-python-how-to-use-queues.md
[Service Bus Topics]: ../service-bus-messaging/service-bus-python-how-to-use-topics-subscriptions.md


<!--External Link references-->

[Python Tools for Visual Studio]: http://aka.ms/ptvs
[Python Tools for Visual Studio Documentation]: http://aka.ms/ptvsdocs
[Cloud Service Projects]: http://go.microsoft.com/fwlink/?LinkId=624028
[Azure SDK Tools for VS 2013]: http://go.microsoft.com/fwlink/?LinkId=746482
[Azure SDK Tools for VS 2015]: http://go.microsoft.com/fwlink/?LinkId=746481
[Azure SDK Tools for VS 2017]: http://go.microsoft.com/fwlink/?LinkId=746483
[Python 2.7 32-bit]: https://www.python.org/downloads/
[Python 3.5 32-bit]: https://www.python.org/downloads/
