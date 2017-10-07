---
title: "a Felhőszolgáltatások aaaCommon indítási feladatok |} Microsoft Docs"
description: "Néhány példa biztosít közös indítási feladatok érdemes lehet a felhőalapú szolgáltatások webes szerepkör vagy a feldolgozói szerepkör tooperform."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: a7095dad-1ee7-4141-bc6a-ef19a6e570f1
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: adegeo
ms.openlocfilehash: c80fac4079439410dfc3795e4bce0fbc07dbbfab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="common-cloud-service-startup-tasks"></a>A felhőalapú szolgáltatás indítási gyakori feladatok
Ez a cikk ismerteti a következőkben néhány példát láthat a gyakori indítása műveletek érdemes lehet tooperform a felhőszolgáltatásban. Használhatja az indítási feladatok tooperform műveletek szerepkör indítása előtt. Előfordulhat, hogy kívánt tooperform műveletek közé tartozik, egy összetevő telepítésével, COM-összetevők regisztrálása, beállításkulcsok vagy hosszú ideig futó folyamat elindítása. 

Lásd: [Ez a cikk](cloud-services-startup-tasks.md) toounderstand indítási feladatok működése, és hogyan kifejezetten toocreate hello bejegyzéseket, amelyek meghatározzák egy indítási tevékenységhez.

> [!NOTE]
> Indítási feladatokat nem alkalmazható tooVirtual gépek, csak tooCloud szolgáltatás webes és feldolgozói szerepköröket is.
> 

## <a name="define-environment-variables-before-a-role-starts"></a>Adja meg a környezeti változókat szerepkör indítása előtt
Ha a környezeti változók egy adott feladat definiálva van szüksége, használja a hello [környezet] hello elemének [feladat] elemet.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
    <WorkerRole name="WorkerRole1">
        ...
        <Startup>
            <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple">
                <Environment>
                    <Variable name="MyEnvironmentVariable" value="MyVariableValue" />
                </Environment>
            </Task>
        </Startup>
    </WorkerRole>
</ServiceDefinition>
```

Változók is használhatja a [érvényes Azure XPath értékét](cloud-services-role-config-xpath.md) valami hello telepítésére vonatkozó tooreference. Hello használata helyett `value` attribútumot, adja meg a [RoleInstanceValue] gyermekelemet.

```xml
<Variable name="PathToStartupStorage">
    <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='StartupLocalStorage']/@path" />
</Variable>
```


## <a name="configure-iis-startup-with-appcmdexe"></a>IIS indítási AppCmd.exe konfigurálása
Hello [AppCmd.exe](https://technet.microsoft.com/library/jj635852.aspx) parancssori eszközzel használt toomanage IIS beállításait az Azure-on indításkor lehet. *AppCmd.exe* indítási feladatok használható a kényelmes, a parancssori hozzáférést tooconfiguration beállításokat tartalmazza, az Azure-on. Használatával *AppCmd.exe*, webhely beállításainál fel, módosíthatók, vagy az alkalmazások és a hely eltávolítása.

Van azonban a kimenő néhány dolgot toowatch hello igénybe veszik a *AppCmd.exe* az indítási feladatok:

* Indítási feladatok egynél többször újraindításnál is futtatható. Például ha egy szerepkör újraindul.
* Ha egy *AppCmd.exe* művelet csak egyszer történik, akkor hozhat létre hiba. Például kísérlet túl tooadd szakasz*Web.config* kétszer tudta előállítani hiba.
* Indítási feladat sikertelen, ha azok egy nem nulla kilépési kóddal tér vissza vagy **errorlevel**. Például ha *AppCmd.exe* hibát generál.

Egy jó gyakorlat toocheck hello **errorlevel** hívása után *AppCmd.exe*, ha ez utóbbi érték könnyen toodo hello hívás túl burkolása*AppCmd.exe* rendelkező egy *.cmd*  fájlt. Ha egy ismert **errorlevel** választ, amelyet figyelmen kívül vagy adja át azt vissza.

hello által visszaadott errorlevel *AppCmd.exe* hello winerror.h fájlban találhatók, és meg is látható [MSDN](https://msdn.microsoft.com/library/windows/desktop/ms681382.aspx).

### <a name="example-of-managing-hello-error-level"></a>Hello hibaszintet kezelése – példa
Ebben a példában a tömörítési szakasz és egy JSON toohello tömörítési bejegyzést ad *Web.config* fájl, a hiba- és naplózás.

hello vonatkozó részeit hello [ServiceDefinition.csdef] fájl itt látható, amelyek magukban foglalják hello beállítása [executionContext](https://msdn.microsoft.com/library/azure/gg557552.aspx#Task) túl attribútum`elevated` toogive *AppCmd.exe*  megfelelő engedélyek toochange hello beállításai a hello *Web.config* fájlt:

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
    <WorkerRole name="WorkerRole1">
        ...
        <Startup>
            <Task commandLine="Startup.cmd" executionContext="elevated" taskType="simple" />
        </Startup>
    </WorkerRole>
</ServiceDefinition>
```

Hello *Startup.cmd* batch-fájl által használt *AppCmd.exe* tooadd tömörítés szakaszt és egy JSON toohello tömörítési bejegyzést *Web.config* fájlt. várt hello **errorlevel** 183 van állítva toozero hello ELLENŐRIZZE használatával. A következő EXE parancssori program. Váratlan errorlevels naplózott tooStartupErrorLog.txt.

```cmd
REM   *** Add a compression section toohello Web.config file. ***
%windir%\system32\inetsrv\appcmd set config /section:urlCompression /doDynamicCompression:True /commit:apphost >> "%TEMP%\StartupLog.txt" 2>&1

REM   ERRORLEVEL 183 occurs when trying tooadd a section that already exists. This error is expected if this
REM   batch file were executed twice. This can occur and must be accounted for in a Azure startup
REM   task. toohandle this situation, set hello ERRORLEVEL toozero by using hello Verify command. hello Verify
REM   command will safely set hello ERRORLEVEL toozero.
IF %ERRORLEVEL% EQU 183 DO VERIFY > NUL

REM   If hello ERRORLEVEL is not zero at this point, some other error occurred.
IF %ERRORLEVEL% NEQ 0 (
    ECHO Error adding a compression section toohello Web.config file. >> "%TEMP%\StartupLog.txt" 2>&1
    GOTO ErrorExit
)

REM   *** Add compression for json. ***
%windir%\system32\inetsrv\appcmd set config  -section:system.webServer/httpCompression /+"dynamicTypes.[mimeType='application/json; charset=utf-8',enabled='True']" /commit:apphost >> "%TEMP%\StartupLog.txt" 2>&1
IF %ERRORLEVEL% EQU 183 VERIFY > NUL
IF %ERRORLEVEL% NEQ 0 (
    ECHO Error adding hello JSON compression type toohello Web.config file. >> "%TEMP%\StartupLog.txt" 2>&1
    GOTO ErrorExit
)

REM   *** Exit batch file. ***
EXIT /b 0

REM   *** Log error and exit ***
:ErrorExit
REM   Report hello date, time, and ERRORLEVEL of hello error.
DATE /T >> "%TEMP%\StartupLog.txt" 2>&1
TIME /T >> "%TEMP%\StartupLog.txt" 2>&1
ECHO An error occurred during startup. ERRORLEVEL = %ERRORLEVEL% >> "%TEMP%\StartupLog.txt" 2>&1
EXIT %ERRORLEVEL%
```

## <a name="add-firewall-rules"></a>A tűzfalszabályok hozzáadása
Nincsenek hatékonyan két tűzfal Azure-ban. hello első tűzfal hello virtuális gép és a hello world kívül közötti kapcsolatok szabályozza. Ez a tűzfal hello vezérli [végpontok] hello elemének [ServiceDefinition.csdef] fájlt.

hello második tűzfal hello virtuális gép és a virtuális gépen belül hello folyamatok közötti kapcsolatok szabályozza. Ez a tűzfal szabályozhatók hello `netsh advfirewall firewall` parancssori eszközt.

Elindul a szerepkörök belül hello folyamatok vonatkozó tűzfalszabályok az Azure létrehoz. Például egy szolgáltatás vagy program indításakor Azure automatikusan hoz létre hello tűzfal szükséges szabályok tooallow adott szolgáltatás toocommunicate hello Internet. Azonban ha egy szolgáltatás, a szerepkör (például egy COM + szolgáltatást, vagy a Windows ütemezett feladat) kívül a folyamat által elindított hoz létre, meg kell toomanually hozzon létre egy tűzfal szabály tooallow toothat szolgáltatás. A tűzfalszabályok is létrehozható egy indítási tevékenységhez használatával.

Rendelkeznie kell egy olyan tűzfalszabályt hoz indítási tevékenységhez egy [executionContext][feladat] a **emelt szintű**. Adja hozzá a következő indítási feladat toohello hello [ServiceDefinition.csdef] fájlt.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
    <WorkerRole name="WorkerRole1">
        ...
        <Startup>
            <Task commandLine="AddFirewallRules.cmd" executionContext="elevated" taskType="simple" />
        </Startup>
    </WorkerRole>
</ServiceDefinition>
```

tooadd hello tűzfalszabályt, használnia kell a megfelelő hello `netsh advfirewall firewall` parancsok a indítási parancsfájlba. Ebben a példában hello indítási feladathoz biztonsági és a titkosítás a 80-as TCP-porton.

```cmd
REM   Add a firewall rule in a startup task.

REM   Add an inbound rule requiring security and encryption for TCP port 80 traffic.
netsh advfirewall firewall add rule name="Require Encryption for Inbound TCP/80" protocol=TCP dir=in localport=80 security=authdynenc action=allow >> "%TEMP%\StartupLog.txt" 2>&1

REM   If an error occurred, return hello errorlevel.
EXIT /B %errorlevel%
```

## <a name="block-a-specific-ip-address"></a>Egy adott IP-cím letiltása
Egy Azure webes szerepkör hozzáférés tooa megadott IP-címek készletét korlátozhatja az IIS módosításával **web.config** fájlt. Emellett szükség van egy parancsfájlt, amely feloldja hello toouse **ipSecurity** hello szakasza **ApplicationHost.config** fájlt.

toodo feloldásához hello **ipSecurity** hello szakasza **ApplicationHost.config** fájlt, hozzon létre egy parancsfájlt, amely a szerepkör indítása. Hozzon létre egy mappát a webes szerepkör nevű hello gyökér szintű **indítási** és az ebben a mappában hozzon létre egy kötegfájlt nevű **startup.cmd**. Adja hozzá a fájl tooyour Visual Studio-projektet, és állítsa be a hello tulajdonságokat túl**másolási mindig** tooensure, hogy a csomagban található.

Adja hozzá a következő indítási feladat toohello hello [ServiceDefinition.csdef] fájlt.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
    <WebRole name="WebRole1">
        ...
        <Startup>
            <Task commandLine="startup.cmd" executionContext="elevated" />
        </Startup>
    </WebRole>
</ServiceDefinition>
```

Adja hozzá a parancs toohello **startup.cmd** fájlt:

```cmd
@echo off
@echo Installing "IPv4 Address and Domain Restrictions" feature 
powershell -ExecutionPolicy Unrestricted -command "Install-WindowsFeature Web-IP-Security"
@echo Unlocking configuration for "IPv4 Address and Domain Restrictions" feature 
%windir%\system32\inetsrv\AppCmd.exe unlock config -section:system.webServer/security/ipSecurity
```

E feladat futtatásakor hello **startup.cmd** batch-fájl toobe minden alkalommal fut hello webes szerepkör inicializálva van, győződjön meg arról, hogy szükséges hello **ipSecurity** szakasz meg oldva.

Végül módosítsa hello [system.webServer szakasz](http://www.iis.net/configreference/system.webserver/security/ipsecurity#005) a webes szerepkör **web.config** fájl tooadd hozzáféréssel, IP-címek listáját, ahogy az alábbi példa hello:

Ez a minta config **lehetővé teszi, hogy** összes IP-címek tooaccess hello kiszolgáló kivételével a két meghatározott hello

```xml
<system.webServer>
    <security>
    <!--Unlisted IP addresses are granted access-->
    <ipSecurity>
        <!--hello following IP addresses are denied access-->
        <add allowed="false" ipAddress="192.168.100.1" subnetMask="255.255.0.0" />
        <add allowed="false" ipAddress="192.168.100.2" subnetMask="255.255.0.0" />
    </ipSecurity>
    </security>
</system.webServer>
```

Ez a minta config **megtagadja** összes IP-címek hello kiszolgáló kivételével a két meghatározott hello hozzáférését.

```xml
<system.webServer>
    <security>
    <!--Unlisted IP addresses are denied access-->
    <ipSecurity allowUnlisted="false">
        <!--hello following IP addresses are granted access-->
        <add allowed="true" ipAddress="192.168.100.1" subnetMask="255.255.0.0" />
        <add allowed="true" ipAddress="192.168.100.2" subnetMask="255.255.0.0" />
    </ipSecurity>
    </security>
</system.webServer>
```

## <a name="create-a-powershell-startup-task"></a>PowerShell indítási feladat létrehozása
A Windows PowerShell-parancsfájlok nem hívható közvetlenül a hello [ServiceDefinition.csdef] fájlt, de ezek is elindítható az indítási parancsfájlban.

PowerShell (alapértelmezés) nem fut az aláíratlan parancsfájlok. Ha nem jelentkezik a parancsfájlt, tooconfigure PowerShell kell toorun az aláíratlan parancsfájlok. toorun az aláíratlan parancsfájlok, hello **ExecutionPolicy** be kell állítani túl**nem korlátozott**. Hello **ExecutionPolicy** beállítás használata hello verzión alapul a Windows PowerShell.

```cmd
REM   Run an unsigned PowerShell script and log hello output
PowerShell -ExecutionPolicy Unrestricted .\startup.ps1 >> "%TEMP%\StartupLog.txt" 2>&1

REM   If an error occurred, return hello errorlevel.
EXIT /B %errorlevel%
```

Ha használata esetén a vendég operációs rendszer, amely futtatja a PowerShell 2.0 vagy 1.0-ás kényszerítheti toorun 2-es verzióját, és nem érhető el, használja az 1-es verziójával.

```cmd
REM   Attempt tooset hello execution policy by using PowerShell version 2.0 syntax.
PowerShell -Version 2.0 -ExecutionPolicy Unrestricted .\startup.ps1 >> "%TEMP%\StartupLog.txt" 2>&1

REM   If PowerShell version 2.0 isn't available. Set hello execution policy by using hello PowerShell
IF %ERRORLEVEL% EQU -393216 (
   PowerShell -Command "Set-ExecutionPolicy Unrestricted" >> "%TEMP%\StartupLog.txt" 2>&1
   PowerShell .\startup.ps1 >> "%TEMP%\StartupLog.txt" 2>&1
)

REM   If an error occurred, return hello errorlevel.
EXIT /B %errorlevel%
```

## <a name="create-files-in-local-storage-from-a-startup-task"></a>Az indítási feladat a helyi tárterület-fájlok létrehozása
A helyi tároló-erőforrás toostore is használhatja az indítási feladat, amelyhez a később az alkalmazás által létrehozott fájlokat.

toocreate hello helyi tároló-erőforrás, adjon hozzá egy [LocalResources] szakasz toohello [ServiceDefinition.csdef] fájlt, és hozzáadja a hello [LocalStorage] gyermekelemet. Hello helyi tároló-erőforrás adjon egyedi nevet és egy megfelelő méretű az indítási tevékenységhez.

az indítási feladat a helyi tároló egyik erőforrásához toouse, meg kell toocreate egy környezeti változó tooreference hello helyi tároló-erőforrás helye. Majd hello indítási tevékenységhez és hello alkalmazás képes tooread és írási fájlok toohello helyi tároló-erőforrás.

hello vonatkozó részeit hello **ServiceDefinition.csdef** fájl itt látható:

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <WorkerRole name="WorkerRole1">
    ...

    <LocalResources>
      <LocalStorage name="StartupLocalStorage" sizeInMB="5"/>
    </LocalResources>

    <Startup>
      <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple">
        <Environment>
          <Variable name="PathToStartupStorage">
            <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='StartupLocalStorage']/@path" />
          </Variable>
        </Environment>
      </Task>
    </Startup>
  </WorkerRole>
</ServiceDefinition>
```

Tegyük fel ez **Startup.cmd** szkript a hello **PathToStartupStorage** környezeti változó toocreate hello fájl **MyTest.txt** hello helyi tárolóban hely.

```cmd
REM   Create a simple text file.

ECHO This text will go into hello MyTest.txt file which will be in hello    >  "%PathToStartupStorage%\MyTest.txt"
ECHO path pointed tooby hello PathToStartupStorage environment variable.  >> "%PathToStartupStorage%\MyTest.txt"
ECHO hello contents of hello PathToStartupStorage environment variable is   >> "%PathToStartupStorage%\MyTest.txt"
ECHO "%PathToStartupStorage%".                                          >> "%PathToStartupStorage%\MyTest.txt"

REM   Exit hello batch file with ERRORLEVEL 0.

EXIT /b 0
```

Elérhető helyi tároló mappa hello Azure SDK hello segítségével [GetLocalResource](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getlocalresource.aspx) metódust.

```csharp
string localStoragePath = Microsoft.WindowsAzure.ServiceRuntime.RoleEnvironment.GetLocalResource("StartupLocalStorage").RootPath;

string fileContent = System.IO.File.ReadAllText(System.IO.Path.Combine(localStoragePath, "MyTestFile.txt"));
```

## <a name="run-in-hello-emulator-or-cloud"></a>Hello emulátor vagy a felhőben futtatható
A különböző lépésekkel, ha működik a hello képest felhő toowhen hello compute emulator az indítási feladat lehet. Például érdemes lehet toouse friss másolati példányának SQL-adatok csak hello emulátorban futtatásakor. Vagy előfordulhat, hogy toodo néhány teljesítményoptimalizálás hello felhő nem szükséges toodo hello emulátorban futtatásakor.

A különböző hello műveletek compute emulator és hello felhő érhető el egy környezeti változó létrehozása a hello képességét tooperform [ServiceDefinition.csdef] fájlt. Az indítási tevékenységhez érték környezeti változó tesztelje.

toocreate hello környezeti változó, vegye fel a hello [változó]/[RoleInstanceValue] elemet, és hozzon létre egy XPath értékét `/RoleEnvironment/Deployment/@emulated`. hello értékének hello **ComputeEmulatorRunning %** környezeti változó `true` futtatásakor hello compute emulator, és `false` hello felhő futtatásakor.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <WorkerRole name="WorkerRole1">

    ...

    <Startup>
      <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple">
        <Environment>
          <Variable name="ComputeEmulatorRunning">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
        </Environment>
      </Task>
    </Startup>

  </WorkerRole>
</ServiceDefinition>
```

hello feladat most ellenőrizheti a hello **ComputeEmulatorRunning %** környezeti változó tooperform különféle műveletek hello szerepkör fut-e a hello alapján a felhő, vagy az emulátor hello. Íme egy .cmd héjparancsfájlt, amely ellenőrzi, hogy a környezeti változó.

```cmd
REM   Check if this task is running on hello compute emulator.

IF "%ComputeEmulatorRunning%" == "true" (
    REM   This task is running on hello compute emulator. Perform tasks that must be run only in hello compute emulator.
) ELSE (
    REM   This task is running on hello cloud. Perform tasks that must be run only in hello cloud.
)
```


## <a name="detect-that-your-task-has-already-run"></a>Észleli, hogy a feladat már futott
hello szerepkör előfordulhat, hogy indítsa újra az indítási feladatok toorun újra okozó újraindítás nélkül. Nincs futó feladat rendelkezik már hello Virtuálisgép üzemeltetési jelző tooindicate van. Előfordulhat, hogy egyes feladatokat hol nem számít, hogy futnak-e több alkalommal. Azonban előfordulhat, hogy az olyan helyzet, amikor tooprevent kell olyan feladatot futtat egynél többször futtatását.

hello legegyszerűbb módja toodetect, amely a feladat már futott toocreate hello fájlban **% TEMP %** mappát, ha hello feladat sikeres, és nézze meg az hello hello feladat elindítása. Íme egy minta cmd héjparancsfájlt, amelyet, amely meg.

```cmd
REM   If Task1_Success.txt exists, then Application 1 is already installed.
IF EXIST "%RoleRoot%\Task1_Success.txt" (
  ECHO Application 1 is already installed. Exiting. >> "%TEMP%\StartupLog.txt" 2>&1
  GOTO Finish
)

REM   Run your real exe task
ECHO Running XYZ >> "%TEMP%\StartupLog.txt" 2>&1
"%PathToApp1Install%\setup.exe" >> "%TEMP%\StartupLog.txt" 2>&1

IF %ERRORLEVEL% EQU 0 (
  REM   hello application installed without error. Create a file tooindicate that hello task
  REM   does not need toobe run again.

  ECHO This line will create a file tooindicate that Application 1 installed correctly. > "%RoleRoot%\Task1_Success.txt"

) ELSE (
  REM   An error occurred. Log hello error and exit with hello error code.

  DATE /T >> "%TEMP%\StartupLog.txt" 2>&1
  TIME /T >> "%TEMP%\StartupLog.txt" 2>&1
  ECHO  An error occurred running task 1. Errorlevel = %ERRORLEVEL%. >> "%TEMP%\StartupLog.txt" 2>&1

  EXIT %ERRORLEVEL%
)

:Finish

REM   Exit normally.
EXIT /B 0
```

## <a name="task-best-practices"></a>A feladat gyakorlati tanácsok
Az alábbiakban néhány gyakorlati tanácsok a feladatot a webes vagy feldolgozói szerepkör konfigurálásakor kell követnie.

### <a name="always-log-startup-activities"></a>Mindig a napló indítási tevékenységek
A Visual Studio nem ad egy hibakereső toostep keresztül parancsfájlokat, hogy helyes tooget adatmennyiség lehető hello parancsfájlokat működését. Hello kimeneti parancsfájlokat, a naplózás is **stdout** és **stderr**, is Önnek fontos információk toodebug közben, és javítsa ki a parancsfájlokat. mindkét toolog **stdout** és **stderr** toohello StartupLog.txt fájl hello directory hegyes tooby hello **% TEMP %** környezeti változó hello szöveget `>>  "%TEMP%\\StartupLog.txt" 2>&1`specifikus toohello végét sor azt szeretné, hogy toolog. A hello például tooexecute setup.exe **PathToApp1Install %** könyvtár:

    "%PathToApp1Install%\setup.exe" >> "%TEMP%\StartupLog.txt" 2>&1

toosimplify az XML-kódot, létrehozhat egy burkoló *cmd* , amely behívja a indítási összes fájl és naplózás feladatokkal, és minden gyermek-tevékenység megosztások hello azonos környezeti változók biztosítja.

Azt tapasztalhatja, ami esetleg csak bosszantó azonban toouse `>> "%TEMP%\StartupLog.txt" 2>&1` minden indítási tevékenységhez hello végén. Hozzon létre egy burkoló, amely kezeli az Ön naplózási kényszerítheti a feladat naplózás. A burkoló meghívja a kívánt toorun hello valós kötegfájlt futtat. Bármely olyan kimenete hello kötegelt célfájl lesz átirányított toohello *Startuplog.txt* fájlt.

hello a következő példa bemutatja, hogyan tooredirect összes eredményét indítási parancsfájlt. Ebben a példában hello ServerDefinition.csdef hoz létre, amely behívja indítási feladat *logwrap.cmd*. *logwrap.cmd* hívások *Startup2.cmd*, minden kimenet átirányítása túl**% TEMP %\\StartupLog.txt**.

ServiceDefinition.cmd:

```xml
<Startup>
    <Task commandLine="logwrap.cmd startup2.cmd" executionContext="limited" taskType="simple" />
</Startup>
```

**logwrap.cmd:**

```cmd
@ECHO OFF

REM   logwrap.cmd calls passed in batch file, redirecting all output toohello StartupLog.txt log file.

ECHO [%date% %time%] == START logwrap.cmd ============================================== >> "%TEMP%\StartupLog.txt" 2>&1
ECHO [%date% %time%] Running %1 >> "%TEMP%\StartupLog.txt" 2>&1

REM   Call hello child command batch file, redirecting all output toohello StartupLog.txt log file.
START /B /WAIT %1 >> "%TEMP%\StartupLog.txt" 2>&1

REM   Log hello completion of child command.
ECHO [%date% %time%] Done >> "%TEMP%\StartupLog.txt" 2>&1

IF %ERRORLEVEL% EQU 0 (

   REM   No errors occurred. Exit logwrap.cmd normally.
   ECHO [%date% %time%] == END logwrap.cmd ================================================ >> "%TEMP%\StartupLog.txt" 2>&1
   ECHO.  >> "%TEMP%\StartupLog.txt" 2>&1
   EXIT /B 0

) ELSE (

   REM   Log hello error.
   ECHO [%date% %time%] An error occurred. hello ERRORLEVEL = %ERRORLEVEL%.  >> "%TEMP%\StartupLog.txt" 2>&1
   ECHO [%date% %time%] == END logwrap.cmd ================================================ >> "%TEMP%\StartupLog.txt" 2>&1
   ECHO.  >> "%TEMP%\StartupLog.txt" 2>&1
   EXIT /B %ERRORLEVEL%

)
```

**Startup2.cmd:**

```cmd
@ECHO OFF

REM   This is hello batch file where hello startup steps should be performed. Because of the
REM   way Startup2.cmd was called, all commands and their outputs will be stored in the
REM   StartupLog.txt file in hello directory pointed tooby hello TEMP environment variable.

REM   If an error occurs, hello following command will pass hello ERRORLEVEL back toothe
REM   calling batch file.

ECHO [%date% %time%] Some log information about this task
ECHO [%date% %time%] Some more log information about this task

EXIT %ERRORLEVEL%
```

Minta kimenet a hello **StartupLog.txt** fájlt:

```txt
[Mon 10/17/2016 20:24:46.75] == START logwrap.cmd ============================================== 
[Mon 10/17/2016 20:24:46.75] Running command1.cmd 
[Mon 10/17/2016 20:24:46.77] Some log information about this task
[Mon 10/17/2016 20:24:46.77] Some more log information about this task
[Mon 10/17/2016 20:24:46.77] Done 
[Mon 10/17/2016 20:24:46.77] == END logwrap.cmd ================================================ 
```

> [!TIP]
> Hello **StartupLog.txt** fájl található hello *C:\Resources\temp\\{szerepkör-azonosító} \RoleTemp* mappa.
> 
> 

### <a name="set-executioncontext-appropriately-for-startup-tasks"></a>Állítsa be megfelelően az indítási feladatok executionContext
Állítsa be megfelelően hello indítási tevékenységhez tartozó jogosultságok. Néha indítási feladatok futtatásához emelt szintű jogosultságokkal annak ellenére, hogy a normál jogosultságokkal fut hello szerepkör.

Hello [executionContext][feladat] attribútum hello indítási tevékenységhez hello jogosultsági szintjének beállítása. Használatával `executionContext="limited"` azt jelenti, hogy hello indítási tevékenységhez hello hello szerepkör azonos jogosultsági szinten. Használatával `executionContext="elevated"` azt jelenti, hogy hello indítási tevékenységhez rendszergazdai jogosultságokkal rendelkezik, amely lehetővé teszi, hogy hello indítása tevékenység tooperform rendszergazdai feladatok rendszergazdai jogosultságokkal tooyour szerepkör kiadása nélkül.

Emelt szintű jogosultság szükséges indítási tevékenységhez példa, hogy egy indítási feladat által használt **AppCmd.exe** tooconfigure IIS. **AppCmd.exe** szükséges `executionContext="elevated"`.

### <a name="use-hello-appropriate-tasktype"></a>Hello megfelelő taskType használata
Hello [taskType][feladat] attribútum meghatározza, hogy hello módon hello indítási feladat végrehajtása. Három értékei: **egyszerű**, **háttér**, és **előtér**. hello háttér és előtér feladatok elindultak aszinkron módon történik, és majd hello egyszerű feladatok végre párhuzamosan egyenként.

A **egyszerű** indítási feladatok hello hello feladatok futásának sorrendje a mely hello feladatok hello ServiceDefinition.csdef fájlban felsorolt hello sorrend szerint beállíthatja. Ha egy **egyszerű** egy nem nulla feladat multiplicitású kilépési kód, majd hello leáll az indítási és hello szerepkör nem indul el.

hello közötti különbség **háttér** indítási feladatok és **előtér** indítási feladatok, hogy **előtér** feladatok megőrzése hello szerepkör futnak, amíg hello  **előtérben** feladat befejeződik. Ez azt is jelenti, hogy ha hello **előtér** feladat válaszol vagy összeomlik, hello szerepkör nem indul újra amíg hello **előtér** feladat kényszeríti lezárva. Emiatt **háttér** feladatok ajánlott aszinkron indítási feladatok, ha nem az adott hello szolgáltatást kell **előtér** feladat.

### <a name="end-batch-files-with-exit-b-0"></a>A 0 kilépési /B End parancsfájlokat
hello szerepkör csak akkor kezdi meg ha hello **errorlevel** minden a egyszerű indítási tevékenységhez értéke nulla. Nem minden program hello beállítása **errorlevel** (kilépési kód) megfelelően, így hello kötegfájl kell végződnie egy `EXIT /B 0` Ha minden megfelelően már futott.

Egy hiányzó `EXIT /B 0` célból egy indítási parancsfájl hello szerepköröket, amelyek nem indulnak el az oka leggyakrabban.

> [!NOTE]
> I észrevette, hogy a beágyazott kötegelt fájlok néha lefagy hello használatakor `/B` paraméter. Érdemes lehet arra, hogy lefagy a probléma nem fordulhat elő, ha egy másik köteg fájl az aktuális köteg fájl, például ha hello toomake [napló burkoló](#always-log-startup-activities). Elhagyható hello `/B` ebben az esetben paraméter.
> 
> 

### <a name="expect-startup-tasks-toorun-more-than-once"></a>Indítási feladatok toorun várhatóan csak egyszer
Nem minden szerepkör újrahasznosítja azt újraindítás többek között az összes szerepkör újrahasznosítja azt közé tartoznak az éppen futó összes indítási feladatok. Ez azt jelenti, hogy az indítási feladatok többször újraindításnál gond nélkül képes toorun kell lennie. Ez hello ismertet [szakasz megelőző](#detect-that-your-task-has-already-run).

### <a name="use-local-storage-toostore-files-that-must-be-accessed-in-hello-role"></a>Használhatják a helyi tároló toostore hello szerepkör érhető el
Ha szeretné, hogy toocopy, vagy hozzon létre egy fájlt a indítási feladat során, amely majd elérhető tooyour szerepkör, akkor a fájlt helyi tárhelyet kell helyezni. Lásd: hello [szakasz megelőző](#create-files-in-local-storage-from-a-startup-task).

## <a name="next-steps"></a>Következő lépések
Tekintse át a hello felhő [service modell és a csomag](cloud-services-model-and-package.md)

További tudnivalók [feladatok](cloud-services-startup-tasks.md) működik.

[Létrehozhat és telepíthet](cloud-services-how-to-create-deploy-portal.md) a cloud service-csomag.

[ServiceDefinition.csdef]: cloud-services-model-and-package.md#csdef
[feladat]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Task
[Startup]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Startup
[Runtime]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Runtime
[környezet]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Environment
[változó]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Variable
[RoleInstanceValue]: https://msdn.microsoft.com/library/azure/gg557552.aspx#RoleInstanceValue
[RoleEnvironment]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx
[Végpontok]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Endpoints
[LocalStorage]: https://msdn.microsoft.com/library/azure/gg557552.aspx#LocalStorage
[LocalResources]: https://msdn.microsoft.com/library/azure/gg557552.aspx#LocalResources
[RoleInstanceValue]: https://msdn.microsoft.com/library/azure/gg557552.aspx#RoleInstanceValue
