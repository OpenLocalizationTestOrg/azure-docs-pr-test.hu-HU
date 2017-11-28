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
# <a name="common-cloud-service-startup-tasks"></a><span data-ttu-id="6871c-103">A felhőalapú szolgáltatás indítási gyakori feladatok</span><span class="sxs-lookup"><span data-stu-id="6871c-103">Common Cloud Service startup tasks</span></span>
<span data-ttu-id="6871c-104">Ez a cikk ismerteti a következőkben néhány példát láthat a gyakori indítása műveletek érdemes lehet tooperform a felhőszolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="6871c-104">This article provides some examples of common startup tasks you may want tooperform in your cloud service.</span></span> <span data-ttu-id="6871c-105">Használhatja az indítási feladatok tooperform műveletek szerepkör indítása előtt.</span><span class="sxs-lookup"><span data-stu-id="6871c-105">You can use startup tasks tooperform operations before a role starts.</span></span> <span data-ttu-id="6871c-106">Előfordulhat, hogy kívánt tooperform műveletek közé tartozik, egy összetevő telepítésével, COM-összetevők regisztrálása, beállításkulcsok vagy hosszú ideig futó folyamat elindítása.</span><span class="sxs-lookup"><span data-stu-id="6871c-106">Operations that you might want tooperform include installing a component, registering COM components, setting registry keys, or starting a long running process.</span></span> 

<span data-ttu-id="6871c-107">Lásd: [Ez a cikk](cloud-services-startup-tasks.md) toounderstand indítási feladatok működése, és hogyan kifejezetten toocreate hello bejegyzéseket, amelyek meghatározzák egy indítási tevékenységhez.</span><span class="sxs-lookup"><span data-stu-id="6871c-107">See [this article](cloud-services-startup-tasks.md) toounderstand how startup tasks work, and specifically how toocreate hello entries that define a startup task.</span></span>

> [!NOTE]
> <span data-ttu-id="6871c-108">Indítási feladatokat nem alkalmazható tooVirtual gépek, csak tooCloud szolgáltatás webes és feldolgozói szerepköröket is.</span><span class="sxs-lookup"><span data-stu-id="6871c-108">Startup tasks are not applicable tooVirtual Machines, only tooCloud Service Web and Worker roles.</span></span>
> 

## <a name="define-environment-variables-before-a-role-starts"></a><span data-ttu-id="6871c-109">Adja meg a környezeti változókat szerepkör indítása előtt</span><span class="sxs-lookup"><span data-stu-id="6871c-109">Define environment variables before a role starts</span></span>
<span data-ttu-id="6871c-110">Ha a környezeti változók egy adott feladat definiálva van szüksége, használja a hello [környezet] hello elemének [feladat] elemet.</span><span class="sxs-lookup"><span data-stu-id="6871c-110">If you need environment variables defined for a specific task, use hello [Environment] element inside hello [Task] element.</span></span>

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

<span data-ttu-id="6871c-111">Változók is használhatja a [érvényes Azure XPath értékét](cloud-services-role-config-xpath.md) valami hello telepítésére vonatkozó tooreference.</span><span class="sxs-lookup"><span data-stu-id="6871c-111">Variables can also use a [valid Azure XPath value](cloud-services-role-config-xpath.md) tooreference something about hello deployment.</span></span> <span data-ttu-id="6871c-112">Hello használata helyett `value` attribútumot, adja meg a [RoleInstanceValue] gyermekelemet.</span><span class="sxs-lookup"><span data-stu-id="6871c-112">Instead of using hello `value` attribute, define a [RoleInstanceValue] child element.</span></span>

```xml
<Variable name="PathToStartupStorage">
    <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='StartupLocalStorage']/@path" />
</Variable>
```


## <a name="configure-iis-startup-with-appcmdexe"></a><span data-ttu-id="6871c-113">IIS indítási AppCmd.exe konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6871c-113">Configure IIS startup with AppCmd.exe</span></span>
<span data-ttu-id="6871c-114">Hello [AppCmd.exe](https://technet.microsoft.com/library/jj635852.aspx) parancssori eszközzel használt toomanage IIS beállításait az Azure-on indításkor lehet.</span><span class="sxs-lookup"><span data-stu-id="6871c-114">hello [AppCmd.exe](https://technet.microsoft.com/library/jj635852.aspx) command-line tool can be used toomanage IIS settings at startup on Azure.</span></span> <span data-ttu-id="6871c-115">*AppCmd.exe* indítási feladatok használható a kényelmes, a parancssori hozzáférést tooconfiguration beállításokat tartalmazza, az Azure-on.</span><span class="sxs-lookup"><span data-stu-id="6871c-115">*AppCmd.exe* provides convenient, command-line access tooconfiguration settings for use in startup tasks on Azure.</span></span> <span data-ttu-id="6871c-116">Használatával *AppCmd.exe*, webhely beállításainál fel, módosíthatók, vagy az alkalmazások és a hely eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="6871c-116">Using *AppCmd.exe*, Website settings can be added, modified, or removed for applications and sites.</span></span>

<span data-ttu-id="6871c-117">Van azonban a kimenő néhány dolgot toowatch hello igénybe veszik a *AppCmd.exe* az indítási feladatok:</span><span class="sxs-lookup"><span data-stu-id="6871c-117">However, there are a few things toowatch out for in hello use of *AppCmd.exe* as a startup task:</span></span>

* <span data-ttu-id="6871c-118">Indítási feladatok egynél többször újraindításnál is futtatható.</span><span class="sxs-lookup"><span data-stu-id="6871c-118">Startup tasks can be run more than once between reboots.</span></span> <span data-ttu-id="6871c-119">Például ha egy szerepkör újraindul.</span><span class="sxs-lookup"><span data-stu-id="6871c-119">For instance, when a role recycles.</span></span>
* <span data-ttu-id="6871c-120">Ha egy *AppCmd.exe* művelet csak egyszer történik, akkor hozhat létre hiba.</span><span class="sxs-lookup"><span data-stu-id="6871c-120">If a *AppCmd.exe* action is performed more than once, it may generate an error.</span></span> <span data-ttu-id="6871c-121">Például kísérlet túl tooadd szakasz*Web.config* kétszer tudta előállítani hiba.</span><span class="sxs-lookup"><span data-stu-id="6871c-121">For example, attempting tooadd a section too*Web.config* twice could generate an error.</span></span>
* <span data-ttu-id="6871c-122">Indítási feladat sikertelen, ha azok egy nem nulla kilépési kóddal tér vissza vagy **errorlevel**.</span><span class="sxs-lookup"><span data-stu-id="6871c-122">Startup tasks fail if they return a non-zero exit code or **errorlevel**.</span></span> <span data-ttu-id="6871c-123">Például ha *AppCmd.exe* hibát generál.</span><span class="sxs-lookup"><span data-stu-id="6871c-123">For example, when *AppCmd.exe* generates an error.</span></span>

<span data-ttu-id="6871c-124">Egy jó gyakorlat toocheck hello **errorlevel** hívása után *AppCmd.exe*, ha ez utóbbi érték könnyen toodo hello hívás túl burkolása*AppCmd.exe* rendelkező egy *.cmd*  fájlt.</span><span class="sxs-lookup"><span data-stu-id="6871c-124">It is a good practice toocheck hello **errorlevel** after calling *AppCmd.exe*, which is easy toodo if you wrap hello call too*AppCmd.exe* with a *.cmd* file.</span></span> <span data-ttu-id="6871c-125">Ha egy ismert **errorlevel** választ, amelyet figyelmen kívül vagy adja át azt vissza.</span><span class="sxs-lookup"><span data-stu-id="6871c-125">If you detect a known **errorlevel** response, you can ignore it, or pass it back.</span></span>

<span data-ttu-id="6871c-126">hello által visszaadott errorlevel *AppCmd.exe* hello winerror.h fájlban találhatók, és meg is látható [MSDN](https://msdn.microsoft.com/library/windows/desktop/ms681382.aspx).</span><span class="sxs-lookup"><span data-stu-id="6871c-126">hello errorlevel returned by *AppCmd.exe* are listed in hello winerror.h file, and can also be seen on [MSDN](https://msdn.microsoft.com/library/windows/desktop/ms681382.aspx).</span></span>

### <a name="example-of-managing-hello-error-level"></a><span data-ttu-id="6871c-127">Hello hibaszintet kezelése – példa</span><span class="sxs-lookup"><span data-stu-id="6871c-127">Example of managing hello error level</span></span>
<span data-ttu-id="6871c-128">Ebben a példában a tömörítési szakasz és egy JSON toohello tömörítési bejegyzést ad *Web.config* fájl, a hiba- és naplózás.</span><span class="sxs-lookup"><span data-stu-id="6871c-128">This example adds a compression section and a compression entry for JSON toohello *Web.config* file, with error handling and logging.</span></span>

<span data-ttu-id="6871c-129">hello vonatkozó részeit hello [ServiceDefinition.csdef] fájl itt látható, amelyek magukban foglalják hello beállítása [executionContext](https://msdn.microsoft.com/library/azure/gg557552.aspx#Task) túl attribútum`elevated` toogive *AppCmd.exe*  megfelelő engedélyek toochange hello beállításai a hello *Web.config* fájlt:</span><span class="sxs-lookup"><span data-stu-id="6871c-129">hello relevant sections of hello [ServiceDefinition.csdef] file are shown here, which include setting hello [executionContext](https://msdn.microsoft.com/library/azure/gg557552.aspx#Task) attribute too`elevated` toogive *AppCmd.exe* sufficient permissions toochange hello settings in hello *Web.config* file:</span></span>

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

<span data-ttu-id="6871c-130">Hello *Startup.cmd* batch-fájl által használt *AppCmd.exe* tooadd tömörítés szakaszt és egy JSON toohello tömörítési bejegyzést *Web.config* fájlt.</span><span class="sxs-lookup"><span data-stu-id="6871c-130">hello *Startup.cmd* batch file uses *AppCmd.exe* tooadd a compression section and a compression entry for JSON toohello *Web.config* file.</span></span> <span data-ttu-id="6871c-131">várt hello **errorlevel** 183 van állítva toozero hello ELLENŐRIZZE használatával. A következő EXE parancssori program.</span><span class="sxs-lookup"><span data-stu-id="6871c-131">hello expected **errorlevel** of 183 is set toozero using hello VERIFY.EXE command-line program.</span></span> <span data-ttu-id="6871c-132">Váratlan errorlevels naplózott tooStartupErrorLog.txt.</span><span class="sxs-lookup"><span data-stu-id="6871c-132">Unexpected errorlevels are logged tooStartupErrorLog.txt.</span></span>

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

## <a name="add-firewall-rules"></a><span data-ttu-id="6871c-133">A tűzfalszabályok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="6871c-133">Add firewall rules</span></span>
<span data-ttu-id="6871c-134">Nincsenek hatékonyan két tűzfal Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="6871c-134">In Azure, there are effectively two firewalls.</span></span> <span data-ttu-id="6871c-135">hello első tűzfal hello virtuális gép és a hello world kívül közötti kapcsolatok szabályozza.</span><span class="sxs-lookup"><span data-stu-id="6871c-135">hello first firewall controls connections between hello virtual machine and hello outside world.</span></span> <span data-ttu-id="6871c-136">Ez a tűzfal hello vezérli [végpontok] hello elemének [ServiceDefinition.csdef] fájlt.</span><span class="sxs-lookup"><span data-stu-id="6871c-136">This firewall is controlled by hello [EndPoints] element in hello [ServiceDefinition.csdef] file.</span></span>

<span data-ttu-id="6871c-137">hello második tűzfal hello virtuális gép és a virtuális gépen belül hello folyamatok közötti kapcsolatok szabályozza.</span><span class="sxs-lookup"><span data-stu-id="6871c-137">hello second firewall controls connections between hello virtual machine and hello processes within that virtual machine.</span></span> <span data-ttu-id="6871c-138">Ez a tűzfal szabályozhatók hello `netsh advfirewall firewall` parancssori eszközt.</span><span class="sxs-lookup"><span data-stu-id="6871c-138">This firewall can be controlled by hello `netsh advfirewall firewall` command-line tool.</span></span>

<span data-ttu-id="6871c-139">Elindul a szerepkörök belül hello folyamatok vonatkozó tűzfalszabályok az Azure létrehoz.</span><span class="sxs-lookup"><span data-stu-id="6871c-139">Azure creates firewall rules for hello processes started within your roles.</span></span> <span data-ttu-id="6871c-140">Például egy szolgáltatás vagy program indításakor Azure automatikusan hoz létre hello tűzfal szükséges szabályok tooallow adott szolgáltatás toocommunicate hello Internet.</span><span class="sxs-lookup"><span data-stu-id="6871c-140">For example, when you start a service or program, Azure automatically creates hello necessary firewall rules tooallow that service toocommunicate with hello Internet.</span></span> <span data-ttu-id="6871c-141">Azonban ha egy szolgáltatás, a szerepkör (például egy COM + szolgáltatást, vagy a Windows ütemezett feladat) kívül a folyamat által elindított hoz létre, meg kell toomanually hozzon létre egy tűzfal szabály tooallow toothat szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="6871c-141">However, if you create a service that is started by a process outside your role (like a COM+ service or a Windows Scheduled Task), you need toomanually create a firewall rule tooallow access toothat service.</span></span> <span data-ttu-id="6871c-142">A tűzfalszabályok is létrehozható egy indítási tevékenységhez használatával.</span><span class="sxs-lookup"><span data-stu-id="6871c-142">These firewall rules can be created by using a startup task.</span></span>

<span data-ttu-id="6871c-143">Rendelkeznie kell egy olyan tűzfalszabályt hoz indítási tevékenységhez egy [executionContext][feladat] a **emelt szintű**.</span><span class="sxs-lookup"><span data-stu-id="6871c-143">A startup task that creates a firewall rule must have an [executionContext][Task] of **elevated**.</span></span> <span data-ttu-id="6871c-144">Adja hozzá a következő indítási feladat toohello hello [ServiceDefinition.csdef] fájlt.</span><span class="sxs-lookup"><span data-stu-id="6871c-144">Add hello following startup task toohello [ServiceDefinition.csdef] file.</span></span>

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

<span data-ttu-id="6871c-145">tooadd hello tűzfalszabályt, használnia kell a megfelelő hello `netsh advfirewall firewall` parancsok a indítási parancsfájlba.</span><span class="sxs-lookup"><span data-stu-id="6871c-145">tooadd hello firewall rule, you must use hello appropriate `netsh advfirewall firewall` commands in your startup batch file.</span></span> <span data-ttu-id="6871c-146">Ebben a példában hello indítási feladathoz biztonsági és a titkosítás a 80-as TCP-porton.</span><span class="sxs-lookup"><span data-stu-id="6871c-146">In this example, hello startup task requires security and encryption for TCP port 80.</span></span>

```cmd
REM   Add a firewall rule in a startup task.

REM   Add an inbound rule requiring security and encryption for TCP port 80 traffic.
netsh advfirewall firewall add rule name="Require Encryption for Inbound TCP/80" protocol=TCP dir=in localport=80 security=authdynenc action=allow >> "%TEMP%\StartupLog.txt" 2>&1

REM   If an error occurred, return hello errorlevel.
EXIT /B %errorlevel%
```

## <a name="block-a-specific-ip-address"></a><span data-ttu-id="6871c-147">Egy adott IP-cím letiltása</span><span class="sxs-lookup"><span data-stu-id="6871c-147">Block a specific IP address</span></span>
<span data-ttu-id="6871c-148">Egy Azure webes szerepkör hozzáférés tooa megadott IP-címek készletét korlátozhatja az IIS módosításával **web.config** fájlt.</span><span class="sxs-lookup"><span data-stu-id="6871c-148">You can restrict an Azure web role access tooa set of specified IP addresses by modifying your IIS **web.config** file.</span></span> <span data-ttu-id="6871c-149">Emellett szükség van egy parancsfájlt, amely feloldja hello toouse **ipSecurity** hello szakasza **ApplicationHost.config** fájlt.</span><span class="sxs-lookup"><span data-stu-id="6871c-149">You also need toouse a command file which unlocks hello **ipSecurity** section of hello **ApplicationHost.config** file.</span></span>

<span data-ttu-id="6871c-150">toodo feloldásához hello **ipSecurity** hello szakasza **ApplicationHost.config** fájlt, hozzon létre egy parancsfájlt, amely a szerepkör indítása.</span><span class="sxs-lookup"><span data-stu-id="6871c-150">toodo unlock hello **ipSecurity** section of hello **ApplicationHost.config** file, create a command file that runs at role start.</span></span> <span data-ttu-id="6871c-151">Hozzon létre egy mappát a webes szerepkör nevű hello gyökér szintű **indítási** és az ebben a mappában hozzon létre egy kötegfájlt nevű **startup.cmd**.</span><span class="sxs-lookup"><span data-stu-id="6871c-151">Create a folder at hello root level of your web role called **startup** and, within this folder, create a batch file called **startup.cmd**.</span></span> <span data-ttu-id="6871c-152">Adja hozzá a fájl tooyour Visual Studio-projektet, és állítsa be a hello tulajdonságokat túl**másolási mindig** tooensure, hogy a csomagban található.</span><span class="sxs-lookup"><span data-stu-id="6871c-152">Add this file tooyour Visual Studio project and set hello properties too**Copy Always** tooensure that it is included in your package.</span></span>

<span data-ttu-id="6871c-153">Adja hozzá a következő indítási feladat toohello hello [ServiceDefinition.csdef] fájlt.</span><span class="sxs-lookup"><span data-stu-id="6871c-153">Add hello following startup task toohello [ServiceDefinition.csdef] file.</span></span>

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

<span data-ttu-id="6871c-154">Adja hozzá a parancs toohello **startup.cmd** fájlt:</span><span class="sxs-lookup"><span data-stu-id="6871c-154">Add this command toohello **startup.cmd** file:</span></span>

```cmd
@echo off
@echo Installing "IPv4 Address and Domain Restrictions" feature 
powershell -ExecutionPolicy Unrestricted -command "Install-WindowsFeature Web-IP-Security"
@echo Unlocking configuration for "IPv4 Address and Domain Restrictions" feature 
%windir%\system32\inetsrv\AppCmd.exe unlock config -section:system.webServer/security/ipSecurity
```

<span data-ttu-id="6871c-155">E feladat futtatásakor hello **startup.cmd** batch-fájl toobe minden alkalommal fut hello webes szerepkör inicializálva van, győződjön meg arról, hogy szükséges hello **ipSecurity** szakasz meg oldva.</span><span class="sxs-lookup"><span data-stu-id="6871c-155">This task causes hello **startup.cmd** batch file toobe run every time hello web role is initialized, ensuring that hello required **ipSecurity** section is unlocked.</span></span>

<span data-ttu-id="6871c-156">Végül módosítsa hello [system.webServer szakasz](http://www.iis.net/configreference/system.webserver/security/ipsecurity#005) a webes szerepkör **web.config** fájl tooadd hozzáféréssel, IP-címek listáját, ahogy az alábbi példa hello:</span><span class="sxs-lookup"><span data-stu-id="6871c-156">Finally, modify hello [system.webServer section](http://www.iis.net/configreference/system.webserver/security/ipsecurity#005) your web role’s **web.config** file tooadd a list of IP addresses that are granted access, as shown in hello following example:</span></span>

<span data-ttu-id="6871c-157">Ez a minta config **lehetővé teszi, hogy** összes IP-címek tooaccess hello kiszolgáló kivételével a két meghatározott hello</span><span class="sxs-lookup"><span data-stu-id="6871c-157">This sample config **allows** all IPs tooaccess hello server except hello two defined</span></span>

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

<span data-ttu-id="6871c-158">Ez a minta config **megtagadja** összes IP-címek hello kiszolgáló kivételével a két meghatározott hello hozzáférését.</span><span class="sxs-lookup"><span data-stu-id="6871c-158">This sample config **denies** all IPs from accessing hello server except for hello two defined.</span></span>

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

## <a name="create-a-powershell-startup-task"></a><span data-ttu-id="6871c-159">PowerShell indítási feladat létrehozása</span><span class="sxs-lookup"><span data-stu-id="6871c-159">Create a PowerShell startup task</span></span>
<span data-ttu-id="6871c-160">A Windows PowerShell-parancsfájlok nem hívható közvetlenül a hello [ServiceDefinition.csdef] fájlt, de ezek is elindítható az indítási parancsfájlban.</span><span class="sxs-lookup"><span data-stu-id="6871c-160">Windows PowerShell scripts cannot be called directly from hello [ServiceDefinition.csdef] file, but they can be invoked from within a startup batch file.</span></span>

<span data-ttu-id="6871c-161">PowerShell (alapértelmezés) nem fut az aláíratlan parancsfájlok.</span><span class="sxs-lookup"><span data-stu-id="6871c-161">PowerShell (by default) does not run unsigned scripts.</span></span> <span data-ttu-id="6871c-162">Ha nem jelentkezik a parancsfájlt, tooconfigure PowerShell kell toorun az aláíratlan parancsfájlok.</span><span class="sxs-lookup"><span data-stu-id="6871c-162">Unless you sign your script, you need tooconfigure PowerShell toorun unsigned scripts.</span></span> <span data-ttu-id="6871c-163">toorun az aláíratlan parancsfájlok, hello **ExecutionPolicy** be kell állítani túl**nem korlátozott**.</span><span class="sxs-lookup"><span data-stu-id="6871c-163">toorun unsigned scripts, hello **ExecutionPolicy** must be set too**Unrestricted**.</span></span> <span data-ttu-id="6871c-164">Hello **ExecutionPolicy** beállítás használata hello verzión alapul a Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6871c-164">hello **ExecutionPolicy** setting that you use is based on hello version of Windows PowerShell.</span></span>

```cmd
REM   Run an unsigned PowerShell script and log hello output
PowerShell -ExecutionPolicy Unrestricted .\startup.ps1 >> "%TEMP%\StartupLog.txt" 2>&1

REM   If an error occurred, return hello errorlevel.
EXIT /B %errorlevel%
```

<span data-ttu-id="6871c-165">Ha használata esetén a vendég operációs rendszer, amely futtatja a PowerShell 2.0 vagy 1.0-ás kényszerítheti toorun 2-es verzióját, és nem érhető el, használja az 1-es verziójával.</span><span class="sxs-lookup"><span data-stu-id="6871c-165">If you're using a Guest OS that is runs PowerShell 2.0 or 1.0 you can force version 2 toorun, and if unavailable, use version 1.</span></span>

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

## <a name="create-files-in-local-storage-from-a-startup-task"></a><span data-ttu-id="6871c-166">Az indítási feladat a helyi tárterület-fájlok létrehozása</span><span class="sxs-lookup"><span data-stu-id="6871c-166">Create files in local storage from a startup task</span></span>
<span data-ttu-id="6871c-167">A helyi tároló-erőforrás toostore is használhatja az indítási feladat, amelyhez a később az alkalmazás által létrehozott fájlokat.</span><span class="sxs-lookup"><span data-stu-id="6871c-167">You can use a local storage resource toostore files created by your startup task that is accessed later by your application.</span></span>

<span data-ttu-id="6871c-168">toocreate hello helyi tároló-erőforrás, adjon hozzá egy [LocalResources] szakasz toohello [ServiceDefinition.csdef] fájlt, és hozzáadja a hello [LocalStorage] gyermekelemet.</span><span class="sxs-lookup"><span data-stu-id="6871c-168">toocreate hello local storage resource, add a [LocalResources] section toohello [ServiceDefinition.csdef] file and then add hello [LocalStorage] child element.</span></span> <span data-ttu-id="6871c-169">Hello helyi tároló-erőforrás adjon egyedi nevet és egy megfelelő méretű az indítási tevékenységhez.</span><span class="sxs-lookup"><span data-stu-id="6871c-169">Give hello local storage resource a unique name and an appropriate size for your startup task.</span></span>

<span data-ttu-id="6871c-170">az indítási feladat a helyi tároló egyik erőforrásához toouse, meg kell toocreate egy környezeti változó tooreference hello helyi tároló-erőforrás helye.</span><span class="sxs-lookup"><span data-stu-id="6871c-170">toouse a local storage resource in your startup task, you need toocreate an environment variable tooreference hello local storage resource location.</span></span> <span data-ttu-id="6871c-171">Majd hello indítási tevékenységhez és hello alkalmazás képes tooread és írási fájlok toohello helyi tároló-erőforrás.</span><span class="sxs-lookup"><span data-stu-id="6871c-171">Then hello startup task and hello application are able tooread and write files toohello local storage resource.</span></span>

<span data-ttu-id="6871c-172">hello vonatkozó részeit hello **ServiceDefinition.csdef** fájl itt látható:</span><span class="sxs-lookup"><span data-stu-id="6871c-172">hello relevant sections of hello **ServiceDefinition.csdef** file are shown here:</span></span>

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

<span data-ttu-id="6871c-173">Tegyük fel ez **Startup.cmd** szkript a hello **PathToStartupStorage** környezeti változó toocreate hello fájl **MyTest.txt** hello helyi tárolóban hely.</span><span class="sxs-lookup"><span data-stu-id="6871c-173">As an example, this **Startup.cmd** batch file uses hello **PathToStartupStorage** environment variable toocreate hello file **MyTest.txt** on hello local storage location.</span></span>

```cmd
REM   Create a simple text file.

ECHO This text will go into hello MyTest.txt file which will be in hello    >  "%PathToStartupStorage%\MyTest.txt"
ECHO path pointed tooby hello PathToStartupStorage environment variable.  >> "%PathToStartupStorage%\MyTest.txt"
ECHO hello contents of hello PathToStartupStorage environment variable is   >> "%PathToStartupStorage%\MyTest.txt"
ECHO "%PathToStartupStorage%".                                          >> "%PathToStartupStorage%\MyTest.txt"

REM   Exit hello batch file with ERRORLEVEL 0.

EXIT /b 0
```

<span data-ttu-id="6871c-174">Elérhető helyi tároló mappa hello Azure SDK hello segítségével [GetLocalResource](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getlocalresource.aspx) metódust.</span><span class="sxs-lookup"><span data-stu-id="6871c-174">You can access local storage folder from hello Azure SDK by using hello [GetLocalResource](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getlocalresource.aspx) method.</span></span>

```csharp
string localStoragePath = Microsoft.WindowsAzure.ServiceRuntime.RoleEnvironment.GetLocalResource("StartupLocalStorage").RootPath;

string fileContent = System.IO.File.ReadAllText(System.IO.Path.Combine(localStoragePath, "MyTestFile.txt"));
```

## <a name="run-in-hello-emulator-or-cloud"></a><span data-ttu-id="6871c-175">Hello emulátor vagy a felhőben futtatható</span><span class="sxs-lookup"><span data-stu-id="6871c-175">Run in hello emulator or cloud</span></span>
<span data-ttu-id="6871c-176">A különböző lépésekkel, ha működik a hello képest felhő toowhen hello compute emulator az indítási feladat lehet.</span><span class="sxs-lookup"><span data-stu-id="6871c-176">You can have your startup task perform different steps when it is operating in hello cloud compared toowhen it is in hello compute emulator.</span></span> <span data-ttu-id="6871c-177">Például érdemes lehet toouse friss másolati példányának SQL-adatok csak hello emulátorban futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="6871c-177">For example, you may want toouse a fresh copy of your SQL data only when running in hello emulator.</span></span> <span data-ttu-id="6871c-178">Vagy előfordulhat, hogy toodo néhány teljesítményoptimalizálás hello felhő nem szükséges toodo hello emulátorban futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="6871c-178">Or you may want toodo some performance optimizations for hello cloud that you don't need toodo when running in hello emulator.</span></span>

<span data-ttu-id="6871c-179">A különböző hello műveletek compute emulator és hello felhő érhető el egy környezeti változó létrehozása a hello képességét tooperform [ServiceDefinition.csdef] fájlt.</span><span class="sxs-lookup"><span data-stu-id="6871c-179">This ability tooperform different actions on hello compute emulator and hello cloud can be accomplished by creating an environment variable in hello [ServiceDefinition.csdef] file.</span></span> <span data-ttu-id="6871c-180">Az indítási tevékenységhez érték környezeti változó tesztelje.</span><span class="sxs-lookup"><span data-stu-id="6871c-180">You then test that environment variable for a value in your startup task.</span></span>

<span data-ttu-id="6871c-181">toocreate hello környezeti változó, vegye fel a hello [változó]/[RoleInstanceValue] elemet, és hozzon létre egy XPath értékét `/RoleEnvironment/Deployment/@emulated`.</span><span class="sxs-lookup"><span data-stu-id="6871c-181">toocreate hello environment variable, add hello [Variable]/[RoleInstanceValue] element and create an XPath value of `/RoleEnvironment/Deployment/@emulated`.</span></span> <span data-ttu-id="6871c-182">hello értékének hello **ComputeEmulatorRunning %** környezeti változó `true` futtatásakor hello compute emulator, és `false` hello felhő futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="6871c-182">hello value of hello **%ComputeEmulatorRunning%** environment variable is `true` when running on hello compute emulator, and `false` when running on hello cloud.</span></span>

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

<span data-ttu-id="6871c-183">hello feladat most ellenőrizheti a hello **ComputeEmulatorRunning %** környezeti változó tooperform különféle műveletek hello szerepkör fut-e a hello alapján a felhő, vagy az emulátor hello.</span><span class="sxs-lookup"><span data-stu-id="6871c-183">hello task can now check hello **%ComputeEmulatorRunning%** environment variable tooperform different actions based on whether hello role is running in hello cloud or hello emulator.</span></span> <span data-ttu-id="6871c-184">Íme egy .cmd héjparancsfájlt, amely ellenőrzi, hogy a környezeti változó.</span><span class="sxs-lookup"><span data-stu-id="6871c-184">Here is a .cmd shell script that checks for that environment variable.</span></span>

```cmd
REM   Check if this task is running on hello compute emulator.

IF "%ComputeEmulatorRunning%" == "true" (
    REM   This task is running on hello compute emulator. Perform tasks that must be run only in hello compute emulator.
) ELSE (
    REM   This task is running on hello cloud. Perform tasks that must be run only in hello cloud.
)
```


## <a name="detect-that-your-task-has-already-run"></a><span data-ttu-id="6871c-185">Észleli, hogy a feladat már futott</span><span class="sxs-lookup"><span data-stu-id="6871c-185">Detect that your task has already run</span></span>
<span data-ttu-id="6871c-186">hello szerepkör előfordulhat, hogy indítsa újra az indítási feladatok toorun újra okozó újraindítás nélkül.</span><span class="sxs-lookup"><span data-stu-id="6871c-186">hello role may recycle without a reboot causing your startup tasks toorun again.</span></span> <span data-ttu-id="6871c-187">Nincs futó feladat rendelkezik már hello Virtuálisgép üzemeltetési jelző tooindicate van.</span><span class="sxs-lookup"><span data-stu-id="6871c-187">There is no flag tooindicate that a task has already run on hello hosting VM.</span></span> <span data-ttu-id="6871c-188">Előfordulhat, hogy egyes feladatokat hol nem számít, hogy futnak-e több alkalommal.</span><span class="sxs-lookup"><span data-stu-id="6871c-188">You may have some tasks where it doesn't matter that they run multiple times.</span></span> <span data-ttu-id="6871c-189">Azonban előfordulhat, hogy az olyan helyzet, amikor tooprevent kell olyan feladatot futtat egynél többször futtatását.</span><span class="sxs-lookup"><span data-stu-id="6871c-189">However, you may run into a situation where you need tooprevent a task from running more than once.</span></span>

<span data-ttu-id="6871c-190">hello legegyszerűbb módja toodetect, amely a feladat már futott toocreate hello fájlban **% TEMP %** mappát, ha hello feladat sikeres, és nézze meg az hello hello feladat elindítása.</span><span class="sxs-lookup"><span data-stu-id="6871c-190">hello simplest way toodetect that a task has already run is toocreate a file in hello **%TEMP%** folder when hello task is successful and look for it at hello start of hello task.</span></span> <span data-ttu-id="6871c-191">Íme egy minta cmd héjparancsfájlt, amelyet, amely meg.</span><span class="sxs-lookup"><span data-stu-id="6871c-191">Here is a sample cmd shell script that does that for you.</span></span>

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

## <a name="task-best-practices"></a><span data-ttu-id="6871c-192">A feladat gyakorlati tanácsok</span><span class="sxs-lookup"><span data-stu-id="6871c-192">Task best practices</span></span>
<span data-ttu-id="6871c-193">Az alábbiakban néhány gyakorlati tanácsok a feladatot a webes vagy feldolgozói szerepkör konfigurálásakor kell követnie.</span><span class="sxs-lookup"><span data-stu-id="6871c-193">Here are some best practices you should follow when configuring task for your web or worker role.</span></span>

### <a name="always-log-startup-activities"></a><span data-ttu-id="6871c-194">Mindig a napló indítási tevékenységek</span><span class="sxs-lookup"><span data-stu-id="6871c-194">Always log startup activities</span></span>
<span data-ttu-id="6871c-195">A Visual Studio nem ad egy hibakereső toostep keresztül parancsfájlokat, hogy helyes tooget adatmennyiség lehető hello parancsfájlokat működését.</span><span class="sxs-lookup"><span data-stu-id="6871c-195">Visual Studio does not provide a debugger toostep through batch files, so it's good tooget as much data on hello operation of batch files as possible.</span></span> <span data-ttu-id="6871c-196">Hello kimeneti parancsfájlokat, a naplózás is **stdout** és **stderr**, is Önnek fontos információk toodebug közben, és javítsa ki a parancsfájlokat.</span><span class="sxs-lookup"><span data-stu-id="6871c-196">Logging hello output of batch files, both **stdout** and **stderr**, can give you important information when trying toodebug and fix batch files.</span></span> <span data-ttu-id="6871c-197">mindkét toolog **stdout** és **stderr** toohello StartupLog.txt fájl hello directory hegyes tooby hello **% TEMP %** környezeti változó hello szöveget `>>  "%TEMP%\\StartupLog.txt" 2>&1`specifikus toohello végét sor azt szeretné, hogy toolog.</span><span class="sxs-lookup"><span data-stu-id="6871c-197">toolog both **stdout** and **stderr** toohello StartupLog.txt file in hello directory pointed tooby hello **%TEMP%** environment variable, add hello text `>>  "%TEMP%\\StartupLog.txt" 2>&1` toohello end of specific lines you want toolog.</span></span> <span data-ttu-id="6871c-198">A hello például tooexecute setup.exe **PathToApp1Install %** könyvtár:</span><span class="sxs-lookup"><span data-stu-id="6871c-198">For example, tooexecute setup.exe in hello **%PathToApp1Install%** directory:</span></span>

    "%PathToApp1Install%\setup.exe" >> "%TEMP%\StartupLog.txt" 2>&1

<span data-ttu-id="6871c-199">toosimplify az XML-kódot, létrehozhat egy burkoló *cmd* , amely behívja a indítási összes fájl és naplózás feladatokkal, és minden gyermek-tevékenység megosztások hello azonos környezeti változók biztosítja.</span><span class="sxs-lookup"><span data-stu-id="6871c-199">toosimplify your xml, you can create a wrapper *cmd* file that calls all of your startup tasks along with logging and ensures each child-task shares hello same environment variables.</span></span>

<span data-ttu-id="6871c-200">Azt tapasztalhatja, ami esetleg csak bosszantó azonban toouse `>> "%TEMP%\StartupLog.txt" 2>&1` minden indítási tevékenységhez hello végén.</span><span class="sxs-lookup"><span data-stu-id="6871c-200">You may find it annoying though toouse `>> "%TEMP%\StartupLog.txt" 2>&1` on hello end of each startup task.</span></span> <span data-ttu-id="6871c-201">Hozzon létre egy burkoló, amely kezeli az Ön naplózási kényszerítheti a feladat naplózás.</span><span class="sxs-lookup"><span data-stu-id="6871c-201">You can enforce task logging by creating a wrapper that handles logging for you.</span></span> <span data-ttu-id="6871c-202">A burkoló meghívja a kívánt toorun hello valós kötegfájlt futtat.</span><span class="sxs-lookup"><span data-stu-id="6871c-202">This wrapper calls hello real batch file you want toorun.</span></span> <span data-ttu-id="6871c-203">Bármely olyan kimenete hello kötegelt célfájl lesz átirányított toohello *Startuplog.txt* fájlt.</span><span class="sxs-lookup"><span data-stu-id="6871c-203">Any output from hello target batch file will be redirected toohello *Startuplog.txt* file.</span></span>

<span data-ttu-id="6871c-204">hello a következő példa bemutatja, hogyan tooredirect összes eredményét indítási parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="6871c-204">hello following example shows how tooredirect all output from a startup batch file.</span></span> <span data-ttu-id="6871c-205">Ebben a példában hello ServerDefinition.csdef hoz létre, amely behívja indítási feladat *logwrap.cmd*.</span><span class="sxs-lookup"><span data-stu-id="6871c-205">In this example, hello ServerDefinition.csdef file creates a startup task that calls *logwrap.cmd*.</span></span> <span data-ttu-id="6871c-206">*logwrap.cmd* hívások *Startup2.cmd*, minden kimenet átirányítása túl**% TEMP %\\StartupLog.txt**.</span><span class="sxs-lookup"><span data-stu-id="6871c-206">*logwrap.cmd* calls *Startup2.cmd*, redirecting all output too**%TEMP%\\StartupLog.txt**.</span></span>

<span data-ttu-id="6871c-207">ServiceDefinition.cmd:</span><span class="sxs-lookup"><span data-stu-id="6871c-207">ServiceDefinition.cmd:</span></span>

```xml
<Startup>
    <Task commandLine="logwrap.cmd startup2.cmd" executionContext="limited" taskType="simple" />
</Startup>
```

<span data-ttu-id="6871c-208">**logwrap.cmd:**</span><span class="sxs-lookup"><span data-stu-id="6871c-208">**logwrap.cmd:**</span></span>

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

<span data-ttu-id="6871c-209">**Startup2.cmd:**</span><span class="sxs-lookup"><span data-stu-id="6871c-209">**Startup2.cmd:**</span></span>

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

<span data-ttu-id="6871c-210">Minta kimenet a hello **StartupLog.txt** fájlt:</span><span class="sxs-lookup"><span data-stu-id="6871c-210">Sample output in hello **StartupLog.txt** file:</span></span>

```txt
[Mon 10/17/2016 20:24:46.75] == START logwrap.cmd ============================================== 
[Mon 10/17/2016 20:24:46.75] Running command1.cmd 
[Mon 10/17/2016 20:24:46.77] Some log information about this task
[Mon 10/17/2016 20:24:46.77] Some more log information about this task
[Mon 10/17/2016 20:24:46.77] Done 
[Mon 10/17/2016 20:24:46.77] == END logwrap.cmd ================================================ 
```

> [!TIP]
> <span data-ttu-id="6871c-211">Hello **StartupLog.txt** fájl található hello *C:\Resources\temp\\{szerepkör-azonosító} \RoleTemp* mappa.</span><span class="sxs-lookup"><span data-stu-id="6871c-211">hello **StartupLog.txt** file is located in hello *C:\Resources\temp\\{role identifier}\RoleTemp* folder.</span></span>
> 
> 

### <a name="set-executioncontext-appropriately-for-startup-tasks"></a><span data-ttu-id="6871c-212">Állítsa be megfelelően az indítási feladatok executionContext</span><span class="sxs-lookup"><span data-stu-id="6871c-212">Set executionContext appropriately for startup tasks</span></span>
<span data-ttu-id="6871c-213">Állítsa be megfelelően hello indítási tevékenységhez tartozó jogosultságok.</span><span class="sxs-lookup"><span data-stu-id="6871c-213">Set privileges appropriately for hello startup task.</span></span> <span data-ttu-id="6871c-214">Néha indítási feladatok futtatásához emelt szintű jogosultságokkal annak ellenére, hogy a normál jogosultságokkal fut hello szerepkör.</span><span class="sxs-lookup"><span data-stu-id="6871c-214">Sometimes startup tasks must run with elevated privileges even though hello role runs with normal privileges.</span></span>

<span data-ttu-id="6871c-215">Hello [executionContext][feladat] attribútum hello indítási tevékenységhez hello jogosultsági szintjének beállítása.</span><span class="sxs-lookup"><span data-stu-id="6871c-215">hello [executionContext][Task] attribute sets hello privilege level of hello startup task.</span></span> <span data-ttu-id="6871c-216">Használatával `executionContext="limited"` azt jelenti, hogy hello indítási tevékenységhez hello hello szerepkör azonos jogosultsági szinten.</span><span class="sxs-lookup"><span data-stu-id="6871c-216">Using `executionContext="limited"` means hello startup task has hello same privilege level as hello role.</span></span> <span data-ttu-id="6871c-217">Használatával `executionContext="elevated"` azt jelenti, hogy hello indítási tevékenységhez rendszergazdai jogosultságokkal rendelkezik, amely lehetővé teszi, hogy hello indítása tevékenység tooperform rendszergazdai feladatok rendszergazdai jogosultságokkal tooyour szerepkör kiadása nélkül.</span><span class="sxs-lookup"><span data-stu-id="6871c-217">Using `executionContext="elevated"` means hello startup task has administrator privileges, which allows hello startup task tooperform administrator tasks without giving administrator privileges tooyour role.</span></span>

<span data-ttu-id="6871c-218">Emelt szintű jogosultság szükséges indítási tevékenységhez példa, hogy egy indítási feladat által használt **AppCmd.exe** tooconfigure IIS.</span><span class="sxs-lookup"><span data-stu-id="6871c-218">An example of a startup task that requires elevated privileges is a startup task that uses **AppCmd.exe** tooconfigure IIS.</span></span> <span data-ttu-id="6871c-219">**AppCmd.exe** szükséges `executionContext="elevated"`.</span><span class="sxs-lookup"><span data-stu-id="6871c-219">**AppCmd.exe** requires `executionContext="elevated"`.</span></span>

### <a name="use-hello-appropriate-tasktype"></a><span data-ttu-id="6871c-220">Hello megfelelő taskType használata</span><span class="sxs-lookup"><span data-stu-id="6871c-220">Use hello appropriate taskType</span></span>
<span data-ttu-id="6871c-221">Hello [taskType][feladat] attribútum meghatározza, hogy hello módon hello indítási feladat végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="6871c-221">hello [taskType][Task] attribute determines hello way hello startup task is executed.</span></span> <span data-ttu-id="6871c-222">Három értékei: **egyszerű**, **háttér**, és **előtér**.</span><span class="sxs-lookup"><span data-stu-id="6871c-222">There are three values: **simple**, **background**, and **foreground**.</span></span> <span data-ttu-id="6871c-223">hello háttér és előtér feladatok elindultak aszinkron módon történik, és majd hello egyszerű feladatok végre párhuzamosan egyenként.</span><span class="sxs-lookup"><span data-stu-id="6871c-223">hello background and foreground tasks are started asynchronously, and then hello simple tasks are executed synchronously one at a time.</span></span>

<span data-ttu-id="6871c-224">A **egyszerű** indítási feladatok hello hello feladatok futásának sorrendje a mely hello feladatok hello ServiceDefinition.csdef fájlban felsorolt hello sorrend szerint beállíthatja.</span><span class="sxs-lookup"><span data-stu-id="6871c-224">With **simple** startup tasks, you can set hello order in which hello tasks run by hello order in which hello tasks are listed in hello ServiceDefinition.csdef file.</span></span> <span data-ttu-id="6871c-225">Ha egy **egyszerű** egy nem nulla feladat multiplicitású kilépési kód, majd hello leáll az indítási és hello szerepkör nem indul el.</span><span class="sxs-lookup"><span data-stu-id="6871c-225">If a **simple** task ends with a non-zero exit code, then hello startup procedure stops and hello role does not start.</span></span>

<span data-ttu-id="6871c-226">hello közötti különbség **háttér** indítási feladatok és **előtér** indítási feladatok, hogy **előtér** feladatok megőrzése hello szerepkör futnak, amíg hello  **előtérben** feladat befejeződik.</span><span class="sxs-lookup"><span data-stu-id="6871c-226">hello difference between **background** startup tasks and **foreground** startup tasks is that **foreground** tasks keep hello role running until hello **foreground** task ends.</span></span> <span data-ttu-id="6871c-227">Ez azt is jelenti, hogy ha hello **előtér** feladat válaszol vagy összeomlik, hello szerepkör nem indul újra amíg hello **előtér** feladat kényszeríti lezárva.</span><span class="sxs-lookup"><span data-stu-id="6871c-227">This also means that if hello **foreground** task hangs or crashes, hello role will not recycle until hello **foreground** task is forced closed.</span></span> <span data-ttu-id="6871c-228">Emiatt **háttér** feladatok ajánlott aszinkron indítási feladatok, ha nem az adott hello szolgáltatást kell **előtér** feladat.</span><span class="sxs-lookup"><span data-stu-id="6871c-228">For this reason, **background** tasks are recommended for asynchronous startup tasks unless you need that feature of hello **foreground** task.</span></span>

### <a name="end-batch-files-with-exit-b-0"></a><span data-ttu-id="6871c-229">A 0 kilépési /B End parancsfájlokat</span><span class="sxs-lookup"><span data-stu-id="6871c-229">End batch files with EXIT /B 0</span></span>
<span data-ttu-id="6871c-230">hello szerepkör csak akkor kezdi meg ha hello **errorlevel** minden a egyszerű indítási tevékenységhez értéke nulla.</span><span class="sxs-lookup"><span data-stu-id="6871c-230">hello role will only start if hello **errorlevel** from each of your simple startup task is zero.</span></span> <span data-ttu-id="6871c-231">Nem minden program hello beállítása **errorlevel** (kilépési kód) megfelelően, így hello kötegfájl kell végződnie egy `EXIT /B 0` Ha minden megfelelően már futott.</span><span class="sxs-lookup"><span data-stu-id="6871c-231">Not all programs set hello **errorlevel** (exit code) correctly, so hello batch file should end with an `EXIT /B 0` if everything ran correctly.</span></span>

<span data-ttu-id="6871c-232">Egy hiányzó `EXIT /B 0` célból egy indítási parancsfájl hello szerepköröket, amelyek nem indulnak el az oka leggyakrabban.</span><span class="sxs-lookup"><span data-stu-id="6871c-232">A missing `EXIT /B 0` at hello end of a startup batch file is a common cause of roles that do not start.</span></span>

> [!NOTE]
> <span data-ttu-id="6871c-233">I észrevette, hogy a beágyazott kötegelt fájlok néha lefagy hello használatakor `/B` paraméter.</span><span class="sxs-lookup"><span data-stu-id="6871c-233">I've noticed that nested batch files sometimes hang when using hello `/B` parameter.</span></span> <span data-ttu-id="6871c-234">Érdemes lehet arra, hogy lefagy a probléma nem fordulhat elő, ha egy másik köteg fájl az aktuális köteg fájl, például ha hello toomake [napló burkoló](#always-log-startup-activities).</span><span class="sxs-lookup"><span data-stu-id="6871c-234">You may want toomake sure that this hang problem does not happen if another batch file calls your current batch file, like if you use hello [log wrapper](#always-log-startup-activities).</span></span> <span data-ttu-id="6871c-235">Elhagyható hello `/B` ebben az esetben paraméter.</span><span class="sxs-lookup"><span data-stu-id="6871c-235">You can omit hello `/B` parameter in this case.</span></span>
> 
> 

### <a name="expect-startup-tasks-toorun-more-than-once"></a><span data-ttu-id="6871c-236">Indítási feladatok toorun várhatóan csak egyszer</span><span class="sxs-lookup"><span data-stu-id="6871c-236">Expect startup tasks toorun more than once</span></span>
<span data-ttu-id="6871c-237">Nem minden szerepkör újrahasznosítja azt újraindítás többek között az összes szerepkör újrahasznosítja azt közé tartoznak az éppen futó összes indítási feladatok.</span><span class="sxs-lookup"><span data-stu-id="6871c-237">Not all role recycles include a reboot, but all role recycles include running all startup tasks.</span></span> <span data-ttu-id="6871c-238">Ez azt jelenti, hogy az indítási feladatok többször újraindításnál gond nélkül képes toorun kell lennie.</span><span class="sxs-lookup"><span data-stu-id="6871c-238">This means that startup tasks must be able toorun multiple times between reboots without any problems.</span></span> <span data-ttu-id="6871c-239">Ez hello ismertet [szakasz megelőző](#detect-that-your-task-has-already-run).</span><span class="sxs-lookup"><span data-stu-id="6871c-239">This is discussed in hello [preceding section](#detect-that-your-task-has-already-run).</span></span>

### <a name="use-local-storage-toostore-files-that-must-be-accessed-in-hello-role"></a><span data-ttu-id="6871c-240">Használhatják a helyi tároló toostore hello szerepkör érhető el</span><span class="sxs-lookup"><span data-stu-id="6871c-240">Use local storage toostore files that must be accessed in hello role</span></span>
<span data-ttu-id="6871c-241">Ha szeretné, hogy toocopy, vagy hozzon létre egy fájlt a indítási feladat során, amely majd elérhető tooyour szerepkör, akkor a fájlt helyi tárhelyet kell helyezni.</span><span class="sxs-lookup"><span data-stu-id="6871c-241">If you want toocopy or create a file during your startup task that is then accessible tooyour role, then that file must be placed in local storage.</span></span> <span data-ttu-id="6871c-242">Lásd: hello [szakasz megelőző](#create-files-in-local-storage-from-a-startup-task).</span><span class="sxs-lookup"><span data-stu-id="6871c-242">See hello [preceding section](#create-files-in-local-storage-from-a-startup-task).</span></span>

## <a name="next-steps"></a><span data-ttu-id="6871c-243">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6871c-243">Next steps</span></span>
<span data-ttu-id="6871c-244">Tekintse át a hello felhő [service modell és a csomag](cloud-services-model-and-package.md)</span><span class="sxs-lookup"><span data-stu-id="6871c-244">Review hello cloud [service model and package](cloud-services-model-and-package.md)</span></span>

<span data-ttu-id="6871c-245">További tudnivalók [feladatok](cloud-services-startup-tasks.md) működik.</span><span class="sxs-lookup"><span data-stu-id="6871c-245">Learn more about how [Tasks](cloud-services-startup-tasks.md) work.</span></span>

<span data-ttu-id="6871c-246">[Létrehozhat és telepíthet](cloud-services-how-to-create-deploy-portal.md) a cloud service-csomag.</span><span class="sxs-lookup"><span data-stu-id="6871c-246">[Create and deploy](cloud-services-how-to-create-deploy-portal.md) your cloud service package.</span></span>

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
