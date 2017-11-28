---
title: "Szoftvertelepítési eszközök használatával az Azure Site Recovery mobilitási szolgáltatás telepítési aaaAutomate |} Microsoft Docs"
description: "Ez a cikk segítséget nyújt a szoftvertelepítési eszközök például a System Center Configuration Manager használatával automatizálhatja a mobilitási szolgáltatás telepítését."
services: site-recovery
documentationcenter: 
author: AnoopVasudavan
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: anoopkv
ms.openlocfilehash: 6c883c6d5308dcec6e0628b0c2196b3a12e08ebe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="automate-mobility-service-installation-by-using-software-deployment-tools"></a><span data-ttu-id="ed380-103">Szoftvertelepítési eszközök segítségével automatizálhatja a mobilitási szolgáltatás telepítési</span><span class="sxs-lookup"><span data-stu-id="ed380-103">Automate Mobility Service installation by using software deployment tools</span></span>

>[!IMPORTANT]
<span data-ttu-id="ed380-104">Jelen dokumentum céljából feltételezzük, hogy verzióját használja **9.9.4510.1** vagy újabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="ed380-104">This document assumes you are using version **9.9.4510.1** or higher.</span></span>

<span data-ttu-id="ed380-105">Ez a cikk azt mutatja, hogyan használhatók a System Center Configuration Manager toodeploy hello Azure Site Recovery mobilitási szolgáltatás az adatközpontban található.</span><span class="sxs-lookup"><span data-stu-id="ed380-105">This article provides you an example of how you can use System Center Configuration Manager toodeploy hello Azure Site Recovery Mobility Service in your datacenter.</span></span> <span data-ttu-id="ed380-106">A következő előnyöket hello egy szoftverfrissítési központi telepítési eszközzel például a Configuration Manager rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="ed380-106">Using a software deployment tool like Configuration Manager has hello following advantages:</span></span>
* <span data-ttu-id="ed380-107">A friss telepítések és frissítések, a tervezett karbantartási időszakban, a szoftverfrissítések központi telepítési ütemezés</span><span class="sxs-lookup"><span data-stu-id="ed380-107">Scheduling deployment of fresh installations and upgrades, during your planned maintenance window for software updates</span></span>
* <span data-ttu-id="ed380-108">A kiszolgálók telepítési toohundreds egyidejűleg skálázás</span><span class="sxs-lookup"><span data-stu-id="ed380-108">Scaling deployment toohundreds of servers simultaneously</span></span>


> [!NOTE]
> <span data-ttu-id="ed380-109">Ebben a cikkben az System Center Configuration Manager 2012 R2 toodemonstrate hello telepítési tevékenységet.</span><span class="sxs-lookup"><span data-stu-id="ed380-109">This article uses System Center Configuration Manager 2012 R2 toodemonstrate hello deployment activity.</span></span> <span data-ttu-id="ed380-110">Sikerült is automatizálni a mobilitási szolgáltatás telepítési [Azure Automation és célállapot-konfiguráció](site-recovery-automate-mobility-service-install.md).</span><span class="sxs-lookup"><span data-stu-id="ed380-110">You could also automate Mobility Service installation by using [Azure Automation and Desired State Configuration](site-recovery-automate-mobility-service-install.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ed380-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ed380-111">Prerequisites</span></span>
1. <span data-ttu-id="ed380-112">Szoftverfrissítési központi telepítési eszköz, például a Configuration Manager, már üzembe van helyezve a környezetben.</span><span class="sxs-lookup"><span data-stu-id="ed380-112">A software deployment tool, like Configuration Manager, that is already deployed in your environment.</span></span>
  <span data-ttu-id="ed380-113">Hozzon létre két [eszközgyűjtemények](https://technet.microsoft.com/library/gg682169.aspx), egy, az összes **Windows kiszolgálók**, és egy másik összes **Linux kiszolgálók**, tooprotect kívánt Site Recovery segítségével.</span><span class="sxs-lookup"><span data-stu-id="ed380-113">Create two [device collections](https://technet.microsoft.com/library/gg682169.aspx), one for all **Windows servers**, and another for all **Linux servers**, that you want tooprotect by using Site Recovery.</span></span>
3. <span data-ttu-id="ed380-114">A konfigurációs kiszolgáló, amely már regisztrálva van a Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="ed380-114">A configuration server that is already registered with Site Recovery.</span></span>
4. <span data-ttu-id="ed380-115">A biztonságos hálózati fájlmegosztást (Server Message Block megosztás), amelyet hello Configuration Manager-kiszolgáló elérhető legyen.</span><span class="sxs-lookup"><span data-stu-id="ed380-115">A secure network file share (Server Message Block share) that can be accessed by hello Configuration Manager server.</span></span>

## <a name="deploy-mobility-service-on-computers-running-windows"></a><span data-ttu-id="ed380-116">Windows rendszerű számítógépekre telepíteni a mobilitási szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="ed380-116">Deploy Mobility Service on computers running Windows</span></span>
> [!NOTE]
> <span data-ttu-id="ed380-117">Ez a cikk feltételezi, hogy hello konfigurációs kiszolgáló IP-címének hello 192.168.3.121, és hello biztonságos hálózati fájlmegosztás \\\ContosoSecureFS\MobilityServiceInstallers.</span><span class="sxs-lookup"><span data-stu-id="ed380-117">This article assumes that hello IP address of hello configuration server is 192.168.3.121, and that hello secure network file share is \\\ContosoSecureFS\MobilityServiceInstallers.</span></span>

### <a name="step-1-prepare-for-deployment"></a><span data-ttu-id="ed380-118">1. lépés: Felkészülés a központi telepítés</span><span class="sxs-lookup"><span data-stu-id="ed380-118">Step 1: Prepare for deployment</span></span>
1. <span data-ttu-id="ed380-119">Hozzon létre egy mappát a hello hálózati megosztást, és adjon neki nevet **MobSvcWindows**.</span><span class="sxs-lookup"><span data-stu-id="ed380-119">Create a folder on hello network share, and name it **MobSvcWindows**.</span></span>
2. <span data-ttu-id="ed380-120">Jelentkezzen be tooyour konfigurációs kiszolgáló, és nyisson meg egy rendszergazdai parancssort.</span><span class="sxs-lookup"><span data-stu-id="ed380-120">Sign in tooyour configuration server, and open an administrative command prompt.</span></span>
3. <span data-ttu-id="ed380-121">Futtassa a következő parancsok toogenerate egy hozzáférési kódot fájl hello:</span><span class="sxs-lookup"><span data-stu-id="ed380-121">Run hello following commands toogenerate a passphrase file:</span></span>

    `cd %ProgramData%\ASR\home\svsystems\bin`

    `genpassphrase.exe -v > MobSvc.passphrase`
4. <span data-ttu-id="ed380-122">Másolás hello **MobSvc.passphrase** hello fájlból **MobSvcWindows** mappa a hálózati megosztáson.</span><span class="sxs-lookup"><span data-stu-id="ed380-122">Copy hello **MobSvc.passphrase** file into hello **MobSvcWindows** folder on your network share.</span></span>
5. <span data-ttu-id="ed380-123">Keresse meg a toohello telepítő tárház hello konfigurációs kiszolgálón hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="ed380-123">Browse toohello installer repository on hello configuration server by running hello following command:</span></span>

   `cd %ProgramData%\ASR\home\svsystems\puhsinstallsvc\repository`

6. <span data-ttu-id="ed380-124">Másolás hello  **Microsoft-ASR\_EE\_*verzió*\_Windows\_GA\_*dátum* \_ Release.exe** toohello **MobSvcWindows** mappa a hálózati megosztáson.</span><span class="sxs-lookup"><span data-stu-id="ed380-124">Copy hello **Microsoft-ASR\_UA\_*version*\_Windows\_GA\_*date*\_Release.exe** toohello **MobSvcWindows** folder on your network share.</span></span>
7. <span data-ttu-id="ed380-125">A következő kód hello másolja, és mentse a fájt **install.bat** történő hello **MobSvcWindows** mappa.</span><span class="sxs-lookup"><span data-stu-id="ed380-125">Copy hello following code, and save it as **install.bat** into hello **MobSvcWindows** folder.</span></span>

   > [!NOTE]
   > <span data-ttu-id="ed380-126">A parancsfájl hello [CSIP] helyőrzőket cserélje le tényleges értékek hello hello IP-cím a konfigurációs kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="ed380-126">Replace hello [CSIP] placeholders in this script with hello actual values of hello IP address of your configuration server.</span></span>

```DOS
Time /t >> C:\Temp\logfile.log
REM ==================================================
REM ==== Clean up hello folders ========================
RMDIR /S /q %temp%\MobSvc
MKDIR %Temp%\MobSvc
MKDIR C:\Temp
REM ==================================================

REM ==== Copy new files ==============================
COPY M*.* %Temp%\MobSvc
CD %Temp%\MobSvc
REN Micro*.exe MobSvcInstaller.exe
REM ==================================================

REM ==== Extract hello installer =======================
MobSvcInstaller.exe /q /x:%Temp%\MobSvc\Extracted
REM ==== Wait 10s for extraction toocomplete =========
TIMEOUT /t 10
REM =================================================

REM ==== Perform installation =======================
REM =================================================

CD %Temp%\MobSvc\Extracted
whoami >> C:\Temp\logfile.log
SET PRODKEY=HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall
REG QUERY %PRODKEY%\{275197FC-14FD-4560-A5EB-38217F80CBD1}
IF NOT %ERRORLEVEL% EQU 0 (
    echo "Product is not installed. Goto INSTALL." >> C:\Temp\logfile.log
    GOTO :INSTALL
) ELSE (
    echo "Product is installed." >> C:\Temp\logfile.log

    echo "Checking for Post-install action status." >> C:\Temp\logfile.log
    GOTO :POSTINSTALLCHECK
)

:POSTINSTALLCHECK
    REG QUERY "HKLM\SOFTWARE\Wow6432Node\InMage Systems\Installed Products\5" /v "PostInstallActions" | Find "Succeeded"
    If %ERRORLEVEL% EQU 0 (
        echo "Post-install actions succeeded. Checking for Configuration status." >> C:\Temp\logfile.log
        GOTO :CONFIGURATIONCHECK
    ) ELSE (
        echo "Post-install actions didn't succeed. Goto INSTALL." >> C:\Temp\logfile.log
        GOTO :INSTALL
    )

:CONFIGURATIONCHECK
    REG QUERY "HKLM\SOFTWARE\Wow6432Node\InMage Systems\Installed Products\5" /v "AgentConfigurationStatus" | Find "Succeeded"
    If %ERRORLEVEL% EQU 0 (
        echo "Configuration has succeeded. Goto UPGRADE." >> C:\Temp\logfile.log
        GOTO :UPGRADE
    ) ELSE (
        echo "Configuration didn't succeed. Goto CONFIGURE." >> C:\Temp\logfile.log
        GOTO :CONFIGURE
    )


:INSTALL
    echo "Perform installation." >> C:\Temp\logfile.log
    UnifiedAgent.exe /Role MS /InstallLocation "C:\Program Files (x86)\Microsoft Azure Site Recovery" /Platform "VmWare" /Silent
    IF %ERRORLEVEL% EQU 0 (
        echo "Installation has succeeded." >> C:\Temp\logfile.log
        (GOTO :CONFIGURE)
    ) ELSE (
        echo "Installation has failed." >> C:\Temp\logfile.log
        GOTO :ENDSCRIPT
    )

:CONFIGURE
    echo "Perform configuration." >> C:\Temp\logfile.log
    cd "C:\Program Files (x86)\Microsoft Azure Site Recovery\agent"
    UnifiedAgentConfigurator.exe  /CSEndPoint "[CSIP]" /PassphraseFilePath %Temp%\MobSvc\MobSvc.passphrase
    IF %ERRORLEVEL% EQU 0 (
        echo "Configuration has succeeded." >> C:\Temp\logfile.log
    ) ELSE (
        echo "Configuration has failed." >> C:\Temp\logfile.log
    )
    GOTO :ENDSCRIPT

:UPGRADE
    echo "Perform upgrade." >> C:\Temp\logfile.log
    UnifiedAgent.exe /Platform "VmWare" /Silent
    IF %ERRORLEVEL% EQU 0 (
        echo "Upgrade has succeeded." >> C:\Temp\logfile.log
    ) ELSE (
        echo "Upgrade has failed." >> C:\Temp\logfile.log
    )
    GOTO :ENDSCRIPT

:ENDSCRIPT
    echo "End of script." >> C:\Temp\logfile.log


```

### <a name="step-2-create-a-package"></a><span data-ttu-id="ed380-127">2. lépés: Csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="ed380-127">Step 2: Create a package</span></span>

1. <span data-ttu-id="ed380-128">Jelentkezzen be tooyour Configuration Manager konzolon.</span><span class="sxs-lookup"><span data-stu-id="ed380-128">Sign in tooyour Configuration Manager console.</span></span>
2. <span data-ttu-id="ed380-129">Keresse meg a túl**szoftverkönyvtár** > **Alkalmazáskezelés** > **csomagok**.</span><span class="sxs-lookup"><span data-stu-id="ed380-129">Browse too**Software Library** > **Application Management** > **Packages**.</span></span>
3. <span data-ttu-id="ed380-130">Kattintson a jobb gombbal **csomagok**, és válassza ki **csomag létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="ed380-130">Right-click **Packages**, and select **Create Package**.</span></span>
4. <span data-ttu-id="ed380-131">Adja meg hello neve, a leírás, a gyártó, a nyelvet és a verzió.</span><span class="sxs-lookup"><span data-stu-id="ed380-131">Provide values for hello name, description, manufacturer, language, and version.</span></span>
5. <span data-ttu-id="ed380-132">Jelölje be hello **Ez a csomag forrásfájlokat tartalmaz** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="ed380-132">Select hello **This package contains source files** check box.</span></span>
6. <span data-ttu-id="ed380-133">Kattintson a **Tallózás**, és válassza hello hello telepítő tároló hálózati megosztást (\\\ContosoSecureFS\MobilityServiceInstaller\MobSvcWindows).</span><span class="sxs-lookup"><span data-stu-id="ed380-133">Click **Browse**, and select hello network share where hello installer is stored (\\\ContosoSecureFS\MobilityServiceInstaller\MobSvcWindows).</span></span>

  ![Képernyőfelvétel a csomag és Program létrehozása varázsló](./media/site-recovery-install-mobility-service-using-sccm/create_sccm_package.png)

7. <span data-ttu-id="ed380-135">A hello **válasszon hello program típusát, amelyet az toocreate** lapon jelölje be **normál Program**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="ed380-135">On hello **Choose hello program type that you want toocreate** page, select **Standard Program**, and click **Next**.</span></span>

  ![Képernyőfelvétel a csomag és Program létrehozása varázsló](./media/site-recovery-install-mobility-service-using-sccm/sccm-standard-program.png)

8. <span data-ttu-id="ed380-137">A hello **adja meg a normál programra vonatkozó információk** lapon adja meg a következő bemenet hello, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="ed380-137">On hello **Specify information about this standard program** page, provide hello following inputs, and click **Next**.</span></span> <span data-ttu-id="ed380-138">(hello más bemeneti használhatja az alapértelmezett értékekre.)</span><span class="sxs-lookup"><span data-stu-id="ed380-138">(hello other inputs can use their default values.)</span></span>

  | <span data-ttu-id="ed380-139">**A paraméter neve**</span><span class="sxs-lookup"><span data-stu-id="ed380-139">**Parameter name**</span></span> | <span data-ttu-id="ed380-140">**Érték**</span><span class="sxs-lookup"><span data-stu-id="ed380-140">**Value**</span></span> |
  |--|--|
  | <span data-ttu-id="ed380-141">Név</span><span class="sxs-lookup"><span data-stu-id="ed380-141">Name</span></span> | <span data-ttu-id="ed380-142">Telepítse a Microsoft Azure mobilitási szolgáltatás (Windows)</span><span class="sxs-lookup"><span data-stu-id="ed380-142">Install Microsoft Azure Mobility Service (Windows)</span></span> |
  | <span data-ttu-id="ed380-143">Parancssor</span><span class="sxs-lookup"><span data-stu-id="ed380-143">Command line</span></span> | <span data-ttu-id="ed380-144">Install.bat</span><span class="sxs-lookup"><span data-stu-id="ed380-144">install.bat</span></span> |
  | <span data-ttu-id="ed380-145">A program futtatható</span><span class="sxs-lookup"><span data-stu-id="ed380-145">Program can run</span></span> | <span data-ttu-id="ed380-146">-E a bejelentkezett felhasználó</span><span class="sxs-lookup"><span data-stu-id="ed380-146">Whether or not a user is logged on</span></span> |

  ![Képernyőfelvétel a csomag és Program létrehozása varázsló](./media/site-recovery-install-mobility-service-using-sccm/sccm-program-properties.png)

9. <span data-ttu-id="ed380-148">Hello következő lapon válassza ki a hello cél operációs rendszereket.</span><span class="sxs-lookup"><span data-stu-id="ed380-148">On hello next page, select hello target operating systems.</span></span> <span data-ttu-id="ed380-149">Csak a Windows Server 2012 R2, Windows Server 2012 és Windows Server 2008 R2 telepítheti a mobilitási szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="ed380-149">Mobility Service can be installed only on Windows Server 2012 R2, Windows Server 2012, and Windows Server 2008 R2.</span></span>

  ![Képernyőfelvétel a csomag és Program létrehozása varázsló](./media/site-recovery-install-mobility-service-using-sccm/sccm-program-properties-page2.png)

10. <span data-ttu-id="ed380-151">toocomplete hello varázsló, kattintson a **következő** kétszer.</span><span class="sxs-lookup"><span data-stu-id="ed380-151">toocomplete hello wizard, click **Next** twice.</span></span>


> [!NOTE]
> <span data-ttu-id="ed380-152">hello parancsfájl mindkét új ügynök telepítése, Mobilitásiszolgáltatás támogatja, és frissíti a már telepített tooagents.</span><span class="sxs-lookup"><span data-stu-id="ed380-152">hello script supports both new installations of Mobility Service agents and updates tooagents that are already installed.</span></span>

### <a name="step-3-deploy-hello-package"></a><span data-ttu-id="ed380-153">3. lépés: Hello csomag központi telepítése</span><span class="sxs-lookup"><span data-stu-id="ed380-153">Step 3: Deploy hello package</span></span>
1. <span data-ttu-id="ed380-154">Hello Configuration Manager konzolon, kattintson jobb gombbal a csomagra, és válassza ki **tartalom terjesztése**.</span><span class="sxs-lookup"><span data-stu-id="ed380-154">In hello Configuration Manager console, right-click your package, and select **Distribute Content**.</span></span>
  <span data-ttu-id="ed380-155">![Képernyőfelvétel a Configuration Manager konzol](./media/site-recovery-install-mobility-service-using-sccm/sccm_distribute.png)</span><span class="sxs-lookup"><span data-stu-id="ed380-155">![Screenshot of Configuration Manager console](./media/site-recovery-install-mobility-service-using-sccm/sccm_distribute.png)</span></span>
2. <span data-ttu-id="ed380-156">Jelölje be hello  **[terjesztési pontok](https://technet.microsoft.com/library/gg712321.aspx#BKMK_PlanForDistributionPoints)**  toowhich a hello csomagok kell másolni.</span><span class="sxs-lookup"><span data-stu-id="ed380-156">Select hello **[distribution points](https://technet.microsoft.com/library/gg712321.aspx#BKMK_PlanForDistributionPoints)** on toowhich hello packages should be copied.</span></span>
3. <span data-ttu-id="ed380-157">Teljes hello varázsló.</span><span class="sxs-lookup"><span data-stu-id="ed380-157">Complete hello wizard.</span></span> <span data-ttu-id="ed380-158">hello csomag majd toohello replikálása elindul a megadott terjesztési pontokra.</span><span class="sxs-lookup"><span data-stu-id="ed380-158">hello package then starts replicating toohello specified distribution points.</span></span>
4. <span data-ttu-id="ed380-159">A csomag terjesztését a hello után kattintson a jobb gombbal a hello csomagot, és válassza ki **telepítés**.</span><span class="sxs-lookup"><span data-stu-id="ed380-159">After hello package distribution is done, right-click hello package, and select **Deploy**.</span></span>
  <span data-ttu-id="ed380-160">![Képernyőfelvétel a Configuration Manager konzol](./media/site-recovery-install-mobility-service-using-sccm/sccm_deploy.png)</span><span class="sxs-lookup"><span data-stu-id="ed380-160">![Screenshot of Configuration Manager console](./media/site-recovery-install-mobility-service-using-sccm/sccm_deploy.png)</span></span>
5. <span data-ttu-id="ed380-161">Jelölje ki azokat a hello hello célgyűjteményt központi telepítéséhez, hello Előfeltételek szakaszában létrehozott Windows Server-eszközgyűjteményt.</span><span class="sxs-lookup"><span data-stu-id="ed380-161">Select hello Windows Server device collection you created in hello prerequisites section as hello target collection for deployment.</span></span>

  ![Képernyőfelvétel a szoftver központi telepítése varázsló](./media/site-recovery-install-mobility-service-using-sccm/sccm-select-target-collection.png)

6. <span data-ttu-id="ed380-163">A hello **hello tartalom célhely megadása** lapon jelölje be a **terjesztési pontok**.</span><span class="sxs-lookup"><span data-stu-id="ed380-163">On hello **Specify hello content destination** page, select your **Distribution Points**.</span></span>
7. <span data-ttu-id="ed380-164">A hello **adja meg a beállítások toocontrol hogyan a szoftver központi telepítése** lapon, győződjön meg arról, hogy hello célja **szükséges**.</span><span class="sxs-lookup"><span data-stu-id="ed380-164">On hello **Specify settings toocontrol how this software is deployed** page, ensure that hello purpose is **Required**.</span></span>

  ![Képernyőfelvétel a szoftver központi telepítése varázsló](./media/site-recovery-install-mobility-service-using-sccm/sccm-deploy-select-purpose.png)

8. <span data-ttu-id="ed380-166">A hello **adja meg a központi telepítés hello ütemezése** lapján adjon meg egy ütemezést.</span><span class="sxs-lookup"><span data-stu-id="ed380-166">On hello **Specify hello schedule for this deployment** page, specify a schedule.</span></span> <span data-ttu-id="ed380-167">További információkért lásd: [csomagok ütemezés](https://technet.microsoft.com/library/gg682178.aspx).</span><span class="sxs-lookup"><span data-stu-id="ed380-167">For more information, see [scheduling packages](https://technet.microsoft.com/library/gg682178.aspx).</span></span>
9. <span data-ttu-id="ed380-168">A hello **terjesztési pontok** lapon, az Adatközpont toohello igényeinek megfelelően hello tulajdonságainak konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="ed380-168">On hello **Distribution Points** page, configure hello properties according toohello needs of your datacenter.</span></span> <span data-ttu-id="ed380-169">Fejezze be hello varázslót.</span><span class="sxs-lookup"><span data-stu-id="ed380-169">Then complete hello wizard.</span></span>

> [!TIP]
> <span data-ttu-id="ed380-170">felesleges tooavoid újraindul, ütemezés hello csomag telepítése során a havi karbantartási vagy szoftver frissítések ablakot.</span><span class="sxs-lookup"><span data-stu-id="ed380-170">tooavoid unnecessary reboots, schedule hello package installation during your monthly maintenance window or software updates window.</span></span>

<span data-ttu-id="ed380-171">Hello központi telepítésének állapotáról hello Configuration Manager konzol használatával figyelheti.</span><span class="sxs-lookup"><span data-stu-id="ed380-171">You can monitor hello deployment progress by using hello Configuration Manager console.</span></span> <span data-ttu-id="ed380-172">Nyissa meg túl**figyelés** > **központi telepítések** > *[a csomag neve]*.</span><span class="sxs-lookup"><span data-stu-id="ed380-172">Go too**Monitoring** > **Deployments** > *[your package name]*.</span></span>

  ![Képernyőfelvétel a Configuration Manager beállítás toomonitor központi telepítések](./media/site-recovery-install-mobility-service-using-sccm/report.PNG)

## <a name="deploy-mobility-service-on-computers-running-linux"></a><span data-ttu-id="ed380-174">Linux rendszerű számítógépeken a mobilitási szolgáltatás üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="ed380-174">Deploy Mobility Service on computers running Linux</span></span>
> [!NOTE]
> <span data-ttu-id="ed380-175">Ez a cikk feltételezi, hogy hello konfigurációs kiszolgáló IP-címének hello 192.168.3.121, és hello biztonságos hálózati fájlmegosztás \\\ContosoSecureFS\MobilityServiceInstallers.</span><span class="sxs-lookup"><span data-stu-id="ed380-175">This article assumes that hello IP address of hello configuration server is 192.168.3.121, and that hello secure network file share is \\\ContosoSecureFS\MobilityServiceInstallers.</span></span>

### <a name="step-1-prepare-for-deployment"></a><span data-ttu-id="ed380-176">1. lépés: Felkészülés a központi telepítés</span><span class="sxs-lookup"><span data-stu-id="ed380-176">Step 1: Prepare for deployment</span></span>
1. <span data-ttu-id="ed380-177">Hozzon létre egy mappát a hello hálózati megosztást, és nevezze el, **MobSvcLinux**.</span><span class="sxs-lookup"><span data-stu-id="ed380-177">Create a folder on hello network share, and name it as **MobSvcLinux**.</span></span>
2. <span data-ttu-id="ed380-178">Jelentkezzen be tooyour konfigurációs kiszolgáló, és nyisson meg egy rendszergazdai parancssort.</span><span class="sxs-lookup"><span data-stu-id="ed380-178">Sign in tooyour configuration server, and open an administrative command prompt.</span></span>
3. <span data-ttu-id="ed380-179">Futtassa a következő parancsok toogenerate egy hozzáférési kódot fájl hello:</span><span class="sxs-lookup"><span data-stu-id="ed380-179">Run hello following commands toogenerate a passphrase file:</span></span>

    `cd %ProgramData%\ASR\home\svsystems\bin`

    `genpassphrase.exe -v > MobSvc.passphrase`
4. <span data-ttu-id="ed380-180">Másolás hello **MobSvc.passphrase** hello fájlból **MobSvcLinux** mappa a hálózati megosztáson.</span><span class="sxs-lookup"><span data-stu-id="ed380-180">Copy hello **MobSvc.passphrase** file into hello **MobSvcLinux** folder on your network share.</span></span>
5. <span data-ttu-id="ed380-181">Keresse meg a toohello telepítő tárház hello konfigurációs kiszolgálón hello parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="ed380-181">Browse toohello installer repository on hello configuration server by running hello command:</span></span>

   `cd %ProgramData%\ASR\home\svsystems\puhsinstallsvc\repository`

6. <span data-ttu-id="ed380-182">Másolás hello következő fájlok toohello **MobSvcLinux** a hálózati megosztáson mappába:</span><span class="sxs-lookup"><span data-stu-id="ed380-182">Copy hello following files toohello **MobSvcLinux** folder on your network share:</span></span>
   * <span data-ttu-id="ed380-183">Microsoft-ASR\_EE\*bites RHEL6-64*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="ed380-183">Microsoft-ASR\_UA\*RHEL6-64*release.tar.gz</span></span>
   * <span data-ttu-id="ed380-184">Microsoft-ASR\_EE\*RHEL7-64\*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="ed380-184">Microsoft-ASR\_UA\*RHEL7-64\*release.tar.gz</span></span>
   * <span data-ttu-id="ed380-185">Microsoft-ASR\_EE\*SLES11-SP3-64\*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="ed380-185">Microsoft-ASR\_UA\*SLES11-SP3-64\*release.tar.gz</span></span>
   * <span data-ttu-id="ed380-186">Microsoft-ASR\_EE\*SLES11-SP4-64\*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="ed380-186">Microsoft-ASR\_UA\*SLES11-SP4-64\*release.tar.gz</span></span>
   * <span data-ttu-id="ed380-187">Microsoft-ASR\_EE\*OL6-64\*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="ed380-187">Microsoft-ASR\_UA\*OL6-64\*release.tar.gz</span></span>
   * <span data-ttu-id="ed380-188">Microsoft-ASR\_EE\*UBUNTU 14.04-64\*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="ed380-188">Microsoft-ASR\_UA\*UBUNTU-14.04-64\*release.tar.gz</span></span>


7. <span data-ttu-id="ed380-189">A következő kód hello másolja, és mentse a fájt **install_linux.sh** történő hello **MobSvcLinux** mappa.</span><span class="sxs-lookup"><span data-stu-id="ed380-189">Copy hello following code, and save it as **install_linux.sh** into hello **MobSvcLinux** folder.</span></span>
   > [!NOTE]
   > <span data-ttu-id="ed380-190">A parancsfájl hello [CSIP] helyőrzőket cserélje le tényleges értékek hello hello IP-cím a konfigurációs kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="ed380-190">Replace hello [CSIP] placeholders in this script with hello actual values of hello IP address of your configuration server.</span></span>

```Bash
#!/usr/bin/env bash

rm -rf /tmp/MobSvc
mkdir -p /tmp/MobSvc
INSTALL_DIR='/usr/local/ASR'
VX_VERSION_FILE='/usr/local/.vx_version'

echo "=============================" >> /tmp/MobSvc/sccm.log
echo `date` >> /tmp/MobSvc/sccm.log
echo "=============================" >> /tmp/MobSvc/sccm.log

if [ -f /etc/oracle-release ] && [ -f /etc/redhat-release ]; then
    if grep -q 'Oracle Linux Server release 6.*' /etc/oracle-release; then
        if uname -a | grep -q x86_64; then
            OS="OL6-64"
            echo $OS >> /tmp/MobSvc/sccm.log
            cp *OL6*.tar.gz /tmp/MobSvc
        fi
    fi
elif [ -f /etc/redhat-release ]; then
    if grep -q 'Red Hat Enterprise Linux Server release 6.* (Santiago)' /etc/redhat-release || \
        grep -q 'CentOS Linux release 6.* (Final)' /etc/redhat-release || \
        grep -q 'CentOS release 6.* (Final)' /etc/redhat-release; then
        if uname -a | grep -q x86_64; then
            OS="RHEL6-64"
            echo $OS >> /tmp/MobSvc/sccm.log
            cp *RHEL6*.tar.gz /tmp/MobSvc
        fi
    elif grep -q 'Red Hat Enterprise Linux Server release 7.* (Maipo)' /etc/redhat-release || \
        grep -q 'CentOS Linux release 7.* (Core)' /etc/redhat-release; then
        if uname -a | grep -q x86_64; then
            OS="RHEL7-64"
            echo $OS >> /tmp/MobSvc/sccm.log
            cp *RHEL7*.tar.gz /tmp/MobSvc
                fi
    fi
elif [ -f /etc/SuSE-release ] && grep -q 'VERSION = 11' /etc/SuSE-release; then
    if grep -q "SUSE Linux Enterprise Server 11" /etc/SuSE-release && grep -q 'PATCHLEVEL = 3' /etc/SuSE-release; then
        if uname -a | grep -q x86_64; then
            OS="SLES11-SP3-64"
            echo $OS >> /tmp/MobSvc/sccm.log
            cp *SLES11-SP3*.tar.gz /tmp/MobSvc
        fi
    elif grep -q "SUSE Linux Enterprise Server 11" /etc/SuSE-release && grep -q 'PATCHLEVEL = 4' /etc/SuSE-release; then
        if uname -a | grep -q x86_64; then
            OS="SLES11-SP4-64"
            echo $OS >> /tmp/MobSvc/sccm.log
            cp *SLES11-SP4*.tar.gz /tmp/MobSvc
        fi
    fi
elif [ -f /etc/lsb-release ] ; then
    if grep -q 'DISTRIB_RELEASE=14.04' /etc/lsb-release ; then
       if uname -a | grep -q x86_64; then
           OS="UBUNTU-14.04-64"
           echo $OS >> /tmp/MobSvc/sccm.log
           cp *UBUNTU-14*.tar.gz /tmp/MobSvc
       fi
    fi
else
    exit 1
fi

if [ -z "$OS" ]; then
    exit 1
fi

Install()
{
    echo "Perform Installation." >> /tmp/MobSvc/sccm.log
    ./install -q -d ${INSTALL_DIR} -r MS -v VmWare
    RET_VAL=$?
    echo "Installation Returncode: $RET_VAL" >> /tmp/MobSvc/sccm.log
    if [ $RET_VAL -eq 0 ]; then
        echo "Installation has succeeded. Proceed tooconfiguration." >> /tmp/MobSvc/sccm.log
        Configure
    else
        echo "Installation has failed." >> /tmp/MobSvc/sccm.log
        exit $RET_VAL
    fi
}

Configure()
{
    echo "Perform configuration." >> /tmp/MobSvc/sccm.log
    ${INSTALL_DIR}/Vx/bin/UnifiedAgentConfigurator.sh -i [CSIP] -P MobSvc.passphrase
    RET_VAL=$?
    echo "Configuration Returncode: $RET_VAL" >> /tmp/MobSvc/sccm.log
    if [ $RET_VAL -eq 0 ]; then
        echo "Configuration has succeeded." >> /tmp/MobSvc/sccm.log
    else
        echo "Configuration has failed." >> /tmp/MobSvc/sccm.log
        exit $RET_VAL
    fi
}

Upgrade()
{
    echo "Perform Upgrade." >> /tmp/MobSvc/sccm.log
    ./install -q -v VmWare
    RET_VAL=$?
    echo "Upgrade Returncode: $RET_VAL" >> /tmp/MobSvc/sccm.log
    if [ $RET_VAL -eq 0 ]; then
        echo "Upgrade has succeeded." >> /tmp/MobSvc/sccm.log
    else
        echo "Upgrade has failed." >> /tmp/MobSvc/sccm.log
        exit $RET_VAL
    fi
}

cp MobSvc.passphrase /tmp/MobSvc
cd /tmp/MobSvc

tar -zxvf *.tar.gz

if [ -e ${VX_VERSION_FILE} ]; then
    echo "${VX_VERSION_FILE} exists. Checking for configuration status." >> /tmp/MobSvc/sccm.log
    agent_configuration=$(grep ^AGENT_CONFIGURATION_STATUS "${VX_VERSION_FILE}" | cut -d"=" -f2 | tr -d " ")
    echo "agent_configuration=$agent_configuration" >> /tmp/MobSvc/sccm.log
     if [ "$agent_configuration" == "Succeeded" ]; then
        echo "Agent is already configured. Proceed tooUpgrade." >> /tmp/MobSvc/sccm.log
        Upgrade
    else
        echo "Agent is not configured. Proceed tooConfigure." >> /tmp/MobSvc/sccm.log
        Configure
    fi
else
    Install
fi

cd /tmp

```

### <a name="step-2-create-a-package"></a><span data-ttu-id="ed380-191">2. lépés: Csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="ed380-191">Step 2: Create a package</span></span>

1. <span data-ttu-id="ed380-192">Jelentkezzen be tooyour Configuration Manager konzolon.</span><span class="sxs-lookup"><span data-stu-id="ed380-192">Sign in  tooyour Configuration Manager console.</span></span>
2. <span data-ttu-id="ed380-193">Keresse meg a túl**szoftverkönyvtár** > **Alkalmazáskezelés** > **csomagok**.</span><span class="sxs-lookup"><span data-stu-id="ed380-193">Browse too**Software Library** > **Application Management** > **Packages**.</span></span>
3. <span data-ttu-id="ed380-194">Kattintson a jobb gombbal **csomagok**, és válassza ki **csomag létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="ed380-194">Right-click **Packages**, and select **Create Package**.</span></span>
4. <span data-ttu-id="ed380-195">Adja meg hello neve, a leírás, a gyártó, a nyelvet és a verzió.</span><span class="sxs-lookup"><span data-stu-id="ed380-195">Provide values for hello name, description, manufacturer, language, and version.</span></span>
5. <span data-ttu-id="ed380-196">Jelölje be hello **Ez a csomag forrásfájlokat tartalmaz** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="ed380-196">Select hello **This package contains source files** check box.</span></span>
6. <span data-ttu-id="ed380-197">Kattintson a **Tallózás**, és válassza hello hello telepítő tároló hálózati megosztást (\\\ContosoSecureFS\MobilityServiceInstaller\MobSvcLinux).</span><span class="sxs-lookup"><span data-stu-id="ed380-197">Click **Browse**, and select hello network share where hello installer is stored (\\\ContosoSecureFS\MobilityServiceInstaller\MobSvcLinux).</span></span>

  ![Képernyőfelvétel a csomag és Program létrehozása varázsló](./media/site-recovery-install-mobility-service-using-sccm/create_sccm_package-linux.png)

7. <span data-ttu-id="ed380-199">A hello **válasszon hello program típusát, amelyet az toocreate** lapon jelölje be **normál Program**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="ed380-199">On hello **Choose hello program type that you want toocreate** page, select **Standard Program**, and click **Next**.</span></span>

  ![Képernyőfelvétel a csomag és Program létrehozása varázsló](./media/site-recovery-install-mobility-service-using-sccm/sccm-standard-program.png)

8. <span data-ttu-id="ed380-201">A hello **adja meg a normál programra vonatkozó információk** lapon adja meg a következő bemenet hello, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="ed380-201">On hello **Specify information about this standard program** page, provide hello following inputs, and click **Next**.</span></span> <span data-ttu-id="ed380-202">(hello más bemeneti használhatja az alapértelmezett értékekre.)</span><span class="sxs-lookup"><span data-stu-id="ed380-202">(hello other inputs can use their default values.)</span></span>

    | <span data-ttu-id="ed380-203">**A paraméter neve**</span><span class="sxs-lookup"><span data-stu-id="ed380-203">**Parameter name**</span></span> | <span data-ttu-id="ed380-204">**Érték**</span><span class="sxs-lookup"><span data-stu-id="ed380-204">**Value**</span></span> |
  |--|--|
  | <span data-ttu-id="ed380-205">Név</span><span class="sxs-lookup"><span data-stu-id="ed380-205">Name</span></span> | <span data-ttu-id="ed380-206">Telepítse a Microsoft Azure mobilitási szolgáltatás (Linux)</span><span class="sxs-lookup"><span data-stu-id="ed380-206">Install Microsoft Azure Mobility Service (Linux)</span></span> |
  | <span data-ttu-id="ed380-207">Parancssor</span><span class="sxs-lookup"><span data-stu-id="ed380-207">Command line</span></span> | <span data-ttu-id="ed380-208">./install_linux.SH</span><span class="sxs-lookup"><span data-stu-id="ed380-208">./install_linux.sh</span></span> |
  | <span data-ttu-id="ed380-209">A program futtatható</span><span class="sxs-lookup"><span data-stu-id="ed380-209">Program can run</span></span> | <span data-ttu-id="ed380-210">-E a bejelentkezett felhasználó</span><span class="sxs-lookup"><span data-stu-id="ed380-210">Whether or not a user is logged on</span></span> |

  ![Képernyőfelvétel a csomag és Program létrehozása varázsló](./media/site-recovery-install-mobility-service-using-sccm/sccm-program-properties-linux.png)

9. <span data-ttu-id="ed380-212">Hello következő lapon válassza ki a **Ez a program bármilyen platformon futtatható**.</span><span class="sxs-lookup"><span data-stu-id="ed380-212">On hello next page, select **This program can run on any platform**.</span></span>
  <span data-ttu-id="ed380-213">![Képernyőfelvétel a csomag és Program létrehozása varázsló](./media/site-recovery-install-mobility-service-using-sccm/sccm-program-properties-page2-linux.png)</span><span class="sxs-lookup"><span data-stu-id="ed380-213">![Screenshot of Create Package and Program wizard](./media/site-recovery-install-mobility-service-using-sccm/sccm-program-properties-page2-linux.png)</span></span>

10. <span data-ttu-id="ed380-214">toocomplete hello varázsló, kattintson a **következő** kétszer.</span><span class="sxs-lookup"><span data-stu-id="ed380-214">toocomplete hello wizard, click **Next** twice.</span></span>

> [!NOTE]
> <span data-ttu-id="ed380-215">hello parancsfájl mindkét új ügynök telepítése, Mobilitásiszolgáltatás támogatja, és frissíti a már telepített tooagents.</span><span class="sxs-lookup"><span data-stu-id="ed380-215">hello script supports both new installations of Mobility Service agents and updates tooagents that are already installed.</span></span>

### <a name="step-3-deploy-hello-package"></a><span data-ttu-id="ed380-216">3. lépés: Hello csomag központi telepítése</span><span class="sxs-lookup"><span data-stu-id="ed380-216">Step 3: Deploy hello package</span></span>
1. <span data-ttu-id="ed380-217">Hello Configuration Manager konzolon, kattintson jobb gombbal a csomagra, és válassza ki **tartalom terjesztése**.</span><span class="sxs-lookup"><span data-stu-id="ed380-217">In hello Configuration Manager console, right-click your package, and select **Distribute Content**.</span></span>
  <span data-ttu-id="ed380-218">![Képernyőfelvétel a Configuration Manager konzol](./media/site-recovery-install-mobility-service-using-sccm/sccm_distribute.png)</span><span class="sxs-lookup"><span data-stu-id="ed380-218">![Screenshot of Configuration Manager console](./media/site-recovery-install-mobility-service-using-sccm/sccm_distribute.png)</span></span>
2. <span data-ttu-id="ed380-219">Jelölje be hello  **[terjesztési pontok](https://technet.microsoft.com/library/gg712321.aspx#BKMK_PlanForDistributionPoints)**  toowhich a hello csomagok kell másolni.</span><span class="sxs-lookup"><span data-stu-id="ed380-219">Select hello **[distribution points](https://technet.microsoft.com/library/gg712321.aspx#BKMK_PlanForDistributionPoints)** on toowhich hello packages should be copied.</span></span>
3. <span data-ttu-id="ed380-220">Teljes hello varázsló.</span><span class="sxs-lookup"><span data-stu-id="ed380-220">Complete hello wizard.</span></span> <span data-ttu-id="ed380-221">hello csomag majd toohello replikálása elindul a megadott terjesztési pontokra.</span><span class="sxs-lookup"><span data-stu-id="ed380-221">hello package then starts replicating toohello specified distribution points.</span></span>
4. <span data-ttu-id="ed380-222">A csomag terjesztését a hello után kattintson a jobb gombbal a hello csomagot, és válassza ki **telepítés**.</span><span class="sxs-lookup"><span data-stu-id="ed380-222">After hello package distribution is done, right-click hello package, and select **Deploy**.</span></span>
  <span data-ttu-id="ed380-223">![Képernyőfelvétel a Configuration Manager konzol](./media/site-recovery-install-mobility-service-using-sccm/sccm_deploy.png)</span><span class="sxs-lookup"><span data-stu-id="ed380-223">![Screenshot of Configuration Manager console](./media/site-recovery-install-mobility-service-using-sccm/sccm_deploy.png)</span></span>
5. <span data-ttu-id="ed380-224">Válassza ki a hello hello célgyűjteményt központi telepítéséhez, hello Előfeltételek szakaszában létrehozott Linux Server eszközgyűjteményt.</span><span class="sxs-lookup"><span data-stu-id="ed380-224">Select hello Linux Server device collection you created in hello prerequisites section as hello target collection for deployment.</span></span>

  ![Képernyőfelvétel a szoftver központi telepítése varázsló](./media/site-recovery-install-mobility-service-using-sccm/sccm-select-target-collection-linux.png)

6. <span data-ttu-id="ed380-226">A hello **hello tartalom célhely megadása** lapon jelölje be a **terjesztési pontok**.</span><span class="sxs-lookup"><span data-stu-id="ed380-226">On hello **Specify hello content destination** page, select your **Distribution Points**.</span></span>
7. <span data-ttu-id="ed380-227">A hello **adja meg a beállítások toocontrol hogyan a szoftver központi telepítése** lapon, győződjön meg arról, hogy hello célja **szükséges**.</span><span class="sxs-lookup"><span data-stu-id="ed380-227">On hello **Specify settings toocontrol how this software is deployed** page, ensure that hello purpose is **Required**.</span></span>

  ![Képernyőfelvétel a szoftver központi telepítése varázsló](./media/site-recovery-install-mobility-service-using-sccm/sccm-deploy-select-purpose.png)

8. <span data-ttu-id="ed380-229">A hello **adja meg a központi telepítés hello ütemezése** lapján adjon meg egy ütemezést.</span><span class="sxs-lookup"><span data-stu-id="ed380-229">On hello **Specify hello schedule for this deployment** page, specify a schedule.</span></span> <span data-ttu-id="ed380-230">További információkért lásd: [csomagok ütemezés](https://technet.microsoft.com/library/gg682178.aspx).</span><span class="sxs-lookup"><span data-stu-id="ed380-230">For more information, see [scheduling packages](https://technet.microsoft.com/library/gg682178.aspx).</span></span>
9. <span data-ttu-id="ed380-231">A hello **terjesztési pontok** lapon, az Adatközpont toohello igényeinek megfelelően hello tulajdonságainak konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="ed380-231">On hello **Distribution Points** page, configure hello properties according toohello needs of your datacenter.</span></span> <span data-ttu-id="ed380-232">Fejezze be hello varázslót.</span><span class="sxs-lookup"><span data-stu-id="ed380-232">Then complete hello wizard.</span></span>

<span data-ttu-id="ed380-233">Lekérdezi hello Linux Server eszközgyűjteményt, konfigurált toohello ütemezés szerint telepíteni a mobilitási szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="ed380-233">Mobility Service gets installed on hello Linux Server Device Collection, according toohello schedule you configured.</span></span>

## <a name="other-methods-tooinstall-mobility-service"></a><span data-ttu-id="ed380-234">Egyéb módszerek tooinstall mobilitási szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="ed380-234">Other methods tooinstall Mobility Service</span></span>
<span data-ttu-id="ed380-235">Az alábbiakban néhány más beállításokat a mobilitási szolgáltatás telepítése:</span><span class="sxs-lookup"><span data-stu-id="ed380-235">Here are some other options for installing Mobility Service:</span></span>
* [<span data-ttu-id="ed380-236">Grafikus felhasználói felülettel manuális telepítés</span><span class="sxs-lookup"><span data-stu-id="ed380-236">Manual Installation using GUI</span></span>](http://aka.ms/mobsvcmanualinstall)
* [<span data-ttu-id="ed380-237">Manuális telepítés parancssori használata</span><span class="sxs-lookup"><span data-stu-id="ed380-237">Manual Installation using command-line</span></span>](http://aka.ms/mobsvcmanualinstallcli)
* [<span data-ttu-id="ed380-238">Ügyfélleküldéses telepítés konfigurációs kiszolgáló használatával</span><span class="sxs-lookup"><span data-stu-id="ed380-238">Push Installation using configuration server </span></span>](http://aka.ms/pushinstall)
* [<span data-ttu-id="ed380-239">Azure Automation és célállapot-konfiguráció automatikus telepítés</span><span class="sxs-lookup"><span data-stu-id="ed380-239">Automated Installation using Azure Automation & Desired State Configuration </span></span>](http://aka.ms/mobsvcdscinstall)

## <a name="uninstall-mobility-service"></a><span data-ttu-id="ed380-240">Távolítsa el a mobilitási szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="ed380-240">Uninstall Mobility Service</span></span>
<span data-ttu-id="ed380-241">A Configuration Manager csomagok toouninstall Mobilitásiszolgáltatás hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="ed380-241">You can create Configuration Manager packages toouninstall Mobility Service.</span></span> <span data-ttu-id="ed380-242">Ezért a következő parancsfájl toodo hello használata:</span><span class="sxs-lookup"><span data-stu-id="ed380-242">Use hello following script toodo so:</span></span>

```
Time /t >> C:\logfile.log
REM ==================================================
REM ==== Check if Mob Svc is already installed =======
REM ==== If not installed no operation required ========
REM ==== Else run uninstall command =====================
REM ==== {275197FC-14FD-4560-A5EB-38217F80CBD1} is ====
REM ==== guid for Mob Svc Installer ====================
whoami >> C:\logfile.log
NET START | FIND "InMage Scout Application Service"
IF  %ERRORLEVEL% EQU 1 (GOTO :INSTALL) ELSE GOTO :UNINSTALL
:NOOPERATION
                echo "No Operation Required." >> c:\logfile.log
                GOTO :ENDSCRIPT
:UNINSTALL
                echo "Uninstall" >> C:\logfile.log
                MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
:ENDSCRIPT

```

## <a name="next-steps"></a><span data-ttu-id="ed380-243">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ed380-243">Next steps</span></span>
<span data-ttu-id="ed380-244">Most már készen áll túl[engedélyezni a védelmet](https://docs.microsoft.com/en-us/azure/site-recovery/site-recovery-vmware-to-azure#step-6-replicate-applications) a virtuális gépek számára.</span><span class="sxs-lookup"><span data-stu-id="ed380-244">You are now ready too[enable protection](https://docs.microsoft.com/en-us/azure/site-recovery/site-recovery-vmware-to-azure#step-6-replicate-applications) for your virtual machines.</span></span>
