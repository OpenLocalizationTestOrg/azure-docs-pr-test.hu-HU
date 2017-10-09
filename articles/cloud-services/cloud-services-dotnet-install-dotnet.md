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
# <a name="install-net-on-azure-cloud-services-roles"></a>Telepítse a .NET szerepkörök Azure Cloud Services csomag
Ez a cikk ismerteti, hogyan tooinstall verzióit, amelyek nem rendelkeznek .NET-keretrendszer hello Azure vendég operációs rendszer. Használhatja a .NET hello vendég operációs rendszer tooconfigure a felhőalapú szolgáltatás webes és feldolgozói szerepköröket.

Például telepíthető .NET 4.6.1 hello Vendég operációsrendszer-család 4, amelyhez nem tartozik a .NET 4.6 bármelyik verzióját. (hello Vendég operációsrendszer-család 5 .NET 4.6 kapható.) Hello legfrissebb információk hello Azure vendég operációs rendszer feloldja, lásd: hello [Azure vendég operációs rendszer kiadási hírek](cloud-services-guestos-update-matrix.md). 

>[!IMPORTANT]
>hello Azure SDK 2.9 tartalmazza a .NET 4.6 telepítése hello Vendég operációsrendszer-család 4-es vagy korábbi korlátozását. Egy javítást alkalmaztunk hello korlátozás érhető el hello [Microsoft Docs](https://github.com/MicrosoftDocs/azure-cloud-services-files/tree/master/Azure%20Targets%20SDK%202.9) hely.

a webes és feldolgozói szerepkörök .NET tooinstall hello .NET webes telepítőjét a felhőszolgáltatás-projekt részeként tartalmazza. Indítsa el a hello telepítő hello szerepkör indítása feladatok részeként. 

## <a name="add-hello-net-installer-tooyour-project"></a>Hello .NET telepítő tooyour projekt hozzáadása
toodownload hello webes telepítőjének hello .NET-keretrendszer, válassza ki a megjeleníteni kívánt tooinstall hello verziót:

* [.NET 4.7 telepítő](http://go.microsoft.com/fwlink/?LinkId=825298)
* [.NET 4.6.1 webalkalmazás-telepítő](http://go.microsoft.com/fwlink/?LinkId=671729)

tooadd hello telepítőjéhez egy *webes* szerepkör:
  1. A **Megoldáskezelőben**a **szerepkörök** a felhőszolgáltatás-projekt, kattintson a jobb egérgombbal a *webes* szerepkör, és válassza **Hozzáadás**  >  **Új mappa**. Hozza létre a **bin**.
  2. Kattintson a jobb gombbal a hello bin mappát, és válassza ki **Hozzáadás** > **meglévő cikk**. Válassza ki a hello .NET telepítő, és vegye fel toohello bin mappájában.
  
tooadd hello telepítőjéhez egy *munkavégző* szerepkör:
* Kattintson a jobb gombbal a *munkavégző* szerepkör, és válassza **Hozzáadás** > **meglévő cikk**. Válassza ki a hello .NET telepítő, és vegye fel toohello szerepkör. 

Fájlok kerülnek a módon toohello szerepkör tartalom mappában, ha azok még automatikusan tooyour cloud service-csomag. hello fájlok vannak, majd a telepített tooa konzisztens hely hello virtuális gépen. Ismételje meg az eljárást minden webes és feldolgozói szerepkör esetén a felhőalapú szolgáltatás, hogy az összes szerepkör hello telepítő másolatát.

> [!NOTE]
> .NET 4.6.1 telepítsen a felhőalapú szolgáltatás szerepkör, akkor is, ha az alkalmazáshoz .NET 4.6 célozza. Vendég operációs rendszer hello tartalmaz hello Tudásbázis [3098779 frissítése](https://support.microsoft.com/kb/3098779) és [3097997 frissítése](https://support.microsoft.com/kb/3097997). Probléma akkor fordulhat elő, ha .NET 4.6 hello Tudásbázis frissítések felett van telepítve a .NET-alkalmazások futtatásakor. Ezek a problémák tooavoid 4.6-os verzió, hanem .NET 4.6.1 telepítése. További információkért lásd: hello [Tudásbázis 3118750](https://support.microsoft.com/kb/3118750).
> 
> 

![Szerepkör tartalmak installer-fájlok][1]

## <a name="define-startup-tasks-for-your-roles"></a>Adja meg a szerepkörök feladatainak indítása
Használhatja az indítási feladatok tooperform műveletek szerepkör indítása előtt. Hello .NET-keretrendszer telepítése, mert hello indítási tevékenységhez részét biztosítja, hogy hello keretrendszer telepítve van, bármely alkalmazás kód futtatása előtt. Indítási feladatok további információkért lásd: [indítási feladatok futtatása az Azure-ban](cloud-services-startup-tasks.md). 

1. Adja hozzá a következő tartalom toohello ServiceDefinition.csdef fájl alapján hello hello **webrole típusról** vagy **WorkerRole** csomópont összes szerepkörök:
   
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
   
    hello előző konfigurációs hello konzol parancs futtatása `install.cmd` rendelkező rendszergazdai jogosultságokkal tooinstall hello a .NET-keretrendszer. hello konfigurációs is létrehoz egy **LocalStorage** nevű elem **NETFXInstall**. hello indítási parancsfájl hello ideiglenes mappa toouse a helyi tároló-erőforrás állítja be. 
    
    > [!IMPORTANT]
    > tooensure kijavíthatja telepítési hello keretrendszer, az ezen erőforrás tooat hello mérete legalább 1024 MB.
    
    Indítási feladatokkal kapcsolatos további információkért lásd: [közös Azure Felhőszolgáltatások indítási feladatok](cloud-services-startup-tasks-common.md).

2. Hozzon létre egy fájlt **install.cmd** , és adja hozzá a hello következő toohello parancsfájl telepítése.

    hello szkript ellenőrzi, hogy hello megadott hello .NET-keretrendszer verziója már telepítve van a hello gépen hello beállításjegyzék lekérdezésével. Ha hello .NET verziója nincs telepítve, hello .NET webes telepítő van megnyitva. toohelp fellépő hibák elhárítására, hello parancsfájl naplózza az összes tevékenység toohello fájl startuptasklog-(jelenlegi dátum és idő) .txt tárolt **InstallLogs** helyi tárterület.

    > [!IMPORTANT]
    > Egy egyszerű szövegszerkesztőben használja, mint a Windows Jegyzettömb toocreate hello install.cmd fájl. Ha használja a Visual Studio toocreate egy szövegfájlt, és hello bővítmény too.cmd módosítása, hello fájl továbbra is tartalmazhat a UTF-8 bájtos rendelés be van jelölve. Ez a jel hibát okozhat, ha a hello első sor hello parancsfájl futtatása. tooavoid Ez a hiba, hogy hello első sor hello a parancsfájl egy el utasítást, amely hello bájt rendelés feldolgozása által figyelmen kívül hagyja. 
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
   > Ez a parancsfájl bemutatja, hogyan tooinstall .NET 4.5.2 vagy folytonosságát, még ha a .NET 4.5.2 már használható 4.6 verziójú hello Azure vendég operációs rendszer. Közvetlenül telepíteni kell verziója 4.6, helyett .NET 4.6.1 hello leírtak [Tudásbázis 3118750](https://support.microsoft.com/kb/3118750).
   > 
   > 

3. Hozzáadás hello install.cmd fájl tooeach szerepkör használatával **hozzáadása** > **a létező elemet** a **Megoldáskezelőben** Ez a témakör a korábban ismertetett módon. 

    Ez a lépés befejezése után az összes szerepkör hello .NET telepítő és hello install.cmd fájlt kell rendelkeznie.

   ![Összes fájlját szerepkör tartalma][2]

## <a name="configure-diagnostics-tootransfer-startup-logs-tooblob-storage"></a>Diagnosztika tootransfer indítási naplók tooBlob tároló konfigurálása
toosimplify a telepítési problémák elhárításához, az Azure Diagnostics tootransfer hello indítási által generált naplófájlokat parancsfájl vagy .NET telepítő tooAzure Blob-tároló hello is konfigurálhat. Ezt a módszert használva tekintheti hello naplók hello napló fájlok letöltését a Blob storage ahelyett, hogy tooremote asztali hello szerepkörhöz.

tooconfigure diagnosztika, nyissa meg hello diagnostics.wadcfgx fájlt, és adja hozzá a következő hello alatt látható tartalmat hello **könyvtárak** csomópont: 

```xml 
<DataSources>
 <DirectoryConfiguration containerName="netfx-install">
  <LocalResource name="NETFXInstall" relativePath="log"/>
 </DirectoryConfiguration>
</DataSources>
```

Az XML-kód diagnosztika tootransfer hello fájlok azt állítja be a hello hello naplókönyvtár **NETFXInstall** erőforrás toohello diagnosztikai tárfiók a hello **netfx-telepítés** blob tároló.

## <a name="deploy-your-cloud-service"></a>A felhőalapú szolgáltatás üzembe helyezése
A felhőalapú szolgáltatás telepítésekor hello indítási feladatok hello .NET-keretrendszer telepítése, ha még nincs telepítve. A felhőszolgáltatás szerepköreit szerepelnek hello *foglalt* állapot hello keretrendszer telepítése közben. Ha hello keretrendszer telepítési újraindítást igényel, hello szolgáltatás szerepkörök is újraindulhat. 

## <a name="additional-resources"></a>További források
* [Hello .NET-keretrendszer telepítése][Installing hello .NET Framework]
* [Határozza meg, melyik .NET-keretrendszer verziója telepítve van][How to: Determine Which .NET Framework Versions Are Installed]
* [Hibaelhárítás a .NET-keretrendszer telepítése][Troubleshooting .NET Framework Installations]

[How to: Determine Which .NET Framework Versions Are Installed]: https://msdn.microsoft.com/library/hh925568.aspx
[Installing hello .NET Framework]: https://msdn.microsoft.com/library/5a4x27ek.aspx
[Troubleshooting .NET Framework Installations]: https://msdn.microsoft.com/library/hh925569.aspx

<!--Image references-->
[1]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithinstallerfiles.png
[2]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithallfiles.png
