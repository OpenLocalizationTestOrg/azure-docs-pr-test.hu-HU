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
# <a name="python-web-and-worker-roles-with-python-tools-for-visual-studio"></a>Python webes és feldolgozói szerepkörök a Visual Studio eszközzel

Ez a cikk a Python webes és feldolgozói szerepkörök [Python Tools for Visual Studio][Python Tools for Visual Studio] eszközben történő használatát ismerteti. Megtudhatja, hogyan toouse Visual Studio toocreate és központi telepítése egy alapszintű felhőalapú szolgáltatás, amely a Pythont használja.

## <a name="prerequisites"></a>Előfeltételek
* [Visual Studio 2013, 2015 vagy 2017](https://www.visualstudio.com/)
* [Python Tools for Visual Studio][Python Tools for Visual Studio] (PTVS)
* [Azure SDK Tools for VS 2013][Azure SDK Tools for VS 2013] vagy  
[Azure SDK Tools for VS 2015][Azure SDK Tools for VS 2015] vagy  
[Azure SDK Tools for VS 2017][Azure SDK Tools for VS 2017]
* [Python 2.7 32 bites][Python 2.7 32-bit] vagy [Python 3.5 32 bites][Python 3.5 32-bit]

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

## <a name="what-are-python-web-and-worker-roles"></a>Mik a Python webes és feldolgozói szerepkörök?
Az Azure három számítási modellt biztosít az alkalmazások futtatásához: [Web Apps szolgáltatás az Azure App Service-ben][execution model-web sites], [Azure Virtual Machines][execution model-vms] és [Azure Cloud Services][execution model-cloud services]. Mindhárom modell támogatja a Python eszközt. A webes és feldolgozói szerepköröket is tartalmazó Cloud Services *platformszolgáltatást (PaaS)* kínál. Belül egy felhőalapú szolgáltatás a webes szerepkör lehetővé teszi egy dedikált Internet Information Services (IIS) webes server toohost előtérbeli webes, míg a feldolgozói szerepkör aszinkron, hosszan futó vagy folyamatos feladatokat futtat függetlenül a felhasználói interakcióktól vagy bemenettől futtatható.

További információ: [Mi az a Cloud Service?].

> [!NOTE]
> *Egy egyszerű webhely toobuild szüksége?*
> Ha csak egy egyszerű webhely előtér, fontolja meg az Azure App Service hello egyszerűsített Web Apps szolgáltatásának használatát. A webhely növekszik és a követelmények változnak könnyedén frissíthet Cloud Service tooa. Lásd: hello <a href="/develop/python/">Python fejlesztői központ</a> az Azure App Service-ben hello Web Apps szolgáltatásának fejlesztését ismertető cikkeket.
> <br />
> 
> 

## <a name="project-creation"></a>Projekt létrehozása
A Visual Studio kiválaszthatja **Azure Cloud Service** a hello **új projekt** párbeszédpanel **Python**.

![Új projekt párbeszédpanel](./media/cloud-services-python-ptvs/new-project-cloud-service.png)

Hello Azure Cloud Service varázslóban létrehozhat új webes és feldolgozói szerepköröket.

![Azure Cloud Service párbeszédpanel](./media/cloud-services-python-ptvs/new-service-wizard.png)

hello feldolgozói szerepkör sablonok bolierplate kóddal tooconnect tooan Azure storage-fiók vagy Azure Service Bus tartalmaz.

![Cloud Service megoldás](./media/cloud-services-python-ptvs/worker.png)

Bármikor hozzáadhat webes vagy feldolgozói szerepkörök tooan létező felhőalapú szolgáltatást.  Válassza ki a tooadd meglévő projekteket a megoldásban, vagy hozzon létre újakat.

![Szerepkör hozzáadása parancs](./media/cloud-services-python-ptvs/add-new-or-existing-role.png)

A felhőszolgáltatás tartalmazhat különböző nyelveken kialakított szerepköröket.  Például kialakíthat egy Python webes szerepkört Django, Python vagy C# feldolgozói szerepkör használatával.  A szerepkörök között egyszerűen kommunikálhat Service Bus üzenetsorok vagy tárolási sorok használatával.

## <a name="install-python-on-hello-cloud-service"></a>Python hello felhőalapú szolgáltatás telepítése
> [!WARNING]
> (időben hello Ez a cikk utolsó frissítés), a Visual Studio telepített hello beállítása parancsfájlok nem működnek. A megoldást ez a szakasz ismerteti.
> 
> 

hello hello beállítása parancsfájlok fő problémája, hogy azok ne telepítse a python. Először határozza meg két [indítási feladatok](cloud-services-startup-tasks.md) a hello [ServiceDefinition.csdef](cloud-services-model-and-package.md#servicedefinitioncsdef) fájlt. a feladat első hello (**PrepPython.ps1**) letölti és telepíti a Python-futtatókörnyezet hello. hello második feladata (**PipInstaller.ps1**) pip tooinstall összes függőséget, előfordulhat, hogy futtatja.

a következő parancsfájlok hello készültek, célzás Python 3.5-ös verzióját. Ha azt szeretné, hogy toouse hello verzió a python, set hello 2.x **PYTHON2** változó fájl túl**a** hello két indítási feladatok és hello futásidejű feladat: `<Variable name="PYTHON2" value="<mark>on</mark>" />`.

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

Hello **PYTHON2** és **PYPATH** változók toohello munkavégző indítási tevékenységhez hozzá kell adni. Hello **PYPATH** változó csak akkor használatos, ha hello **PYTHON2** változó értéke túl**a**.

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

#### <a name="sample-servicedefinitioncsdef"></a>Mintául szolgáló ServiceDefinition.csdef
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



Ezután hozzon létre hello **PrepPython.ps1** és **PipInstaller.ps1** hello fájlok **. / bin** mappában található a szerepkör.

#### <a name="preppythonps1"></a>PrepPython.ps1
Ez a parancsfájl telepíti a Pythont. Ha hello **PYTHON2** környezeti változó értéke túl**a**, majd a Python 2.7 telepítve van, ellenkező esetben a Python 3.5 telepítve van.

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

#### <a name="pipinstallerps1"></a>PipInstaller.ps1
Ez a parancsfájl meghívja a pip, és telepíti a teljes hello függőségek hello **requirements.txt** fájlt. Ha hello **PYTHON2** környezeti változó értéke túl**a**, majd a Python 2.7 szolgál, ellenkező esetben a Python 3.5 szolgál.

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

#### <a name="modify-launchworkerps1"></a>A LaunchWorker.ps1 módosítása
> [!NOTE]
> Hello esetében egy **feldolgozói szerepkör** projektben **LauncherWorker.ps1** szükséges tooexecute hello indítási fájl. Az egy **webes szerepkör** projektre, hello indítási fájl helyette meghatározott hello projekt tulajdonságait.
> 
> 

Hello **bin\LaunchWorker.ps1** eredetileg toodo nagy mennyiségű előkészítő munka, de valójában nem működik. Cserélje ki a következő parancsfájl hello hello tartalmát, az adott fájlban.

Ez a parancsfájl meghívja a hello **worker.py** fájl a python projektből. Ha hello **PYTHON2** környezeti változó értéke túl**a**, majd a Python 2.7 szolgál, ellenkező esetben a Python 3.5 szolgál.

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

#### <a name="pscmd"></a>ps.cmd
hozza létre a Visual Studio sablonok hello egy **ps.cmd** hello fájlban **. / bin** mappát. A parancsfájl kimenő hello PowerShell meghívja a fenti burkoló parancsfájlok és hello PowerShell burkoló nevű hello neve alapján-naplózást végez. Ha a fájl nem jött létre, itt láthatja, minek kéne benne lennie. 

```bat
@echo off

cd /D %~dp0

if not exist "%DiagnosticStore%\LogFiles" mkdir "%DiagnosticStore%\LogFiles"
%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe -ExecutionPolicy Unrestricted -File %* >> "%DiagnosticStore%\LogFiles\%~n1.txt" 2>> "%DiagnosticStore%\LogFiles\%~n1.err.txt"
```



## <a name="run-locally"></a>Helyi futtatás
Ha a felhőszolgáltatási projektet állítja be hello kezdőprojektként, és nyomja le az F5 billentyűt, hello felhőszolgáltatás hello helyi Azure emulátorban futtatja.

Bár a PVTS támogatja indítást hello emulátorban, a hibakeresés (például töréspontok keresése) nem működik.

toodebug a webes és feldolgozói szerepkörök állíthat hello szerepkör projekt hello indítási projektként, és helyette, amely debug.  Beállíthat több kiindulási projektet is.  Kattintson a jobb gombbal a hello megoldás, és válassza ki **indítási projektek beállítása**.

![A megoldás kiindulási projektjének tulajdonságai](./media/cloud-services-python-ptvs/startup.png)

## <a name="publish-tooazure"></a>TooAzure közzététele
toopublish, kattintson a jobb gombbal a hello megoldás hello felhőszolgáltatási projektre, és válassza ki **közzététel**.

![Bejelentkezés a Microsoft Azure közzétételhez](./media/cloud-services-python-ptvs/publish-sign-in.png)

Hajtsa végre a hello varázsló. Szükség esetén engedélyezze a távoli asztal használatát. A távoli asztal akkor hasznos, ha szüksége toodebug valamit.

Ha elkészült a beállítások konfigurálásával, kattintson a **Közzététel** gombra.

Hello kimeneti ablakban egy folyamatjelző jelenik meg, majd hello Microsoft Azure tevékenységnapló ablak látható.

![Microsoft Azure tevékenységnapló ablak](./media/cloud-services-python-ptvs/publish-activity-log.png)

Központi telepítés több percet toocomplete, majd a webes vesz igénybe, és/vagy feldolgozói szerepkörök futtatásához Azure!

### <a name="investigate-logs"></a>Naplók vizsgálata
Miután hello felhőalapú szolgáltatás virtuális gép elindul, és telepíti a Python, vessen egy pillantást hello naplók toofind esetleges hibaüzenetek. Ezek a naplók találhatók hello **C:\Resources\Directory\\{szerepkör} \LogFiles** mappa. **PrepPython.err.txt** a amikor hello parancsfájl kísérli meg toodetect, ha telepítve van a Python rendelkezik legalább egy hibát és **PipInstaller.err.txt** panaszkodnak előfordulhat, hogy a pip elavult verziójára.

## <a name="next-steps"></a>Következő lépések
Webes és feldolgozói szerepkörök Python Tools for Visual Studio használatával kapcsolatos részletes információkért lásd: hello PVTS dokumentációban:

* [Cloud Service-projektek][Cloud Service Projects]

A webes és feldolgozói szerepköröket, például az Azure Storage vagy a Service Bus, az Azure-szolgáltatások használatával kapcsolatos további részletekért tekintse meg a következő cikkek hello:

* [Blob Service][Blob Service]
* [Table Service][Table Service]
* [Queue szolgáltatás][Queue Service]
* [Service Bus-üzenetsorok][Service Bus Queues]
* [Service Bus-témakörök][Service Bus Topics]

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
