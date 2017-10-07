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
# <a name="automate-mobility-service-installation-by-using-software-deployment-tools"></a>Szoftvertelepítési eszközök segítségével automatizálhatja a mobilitási szolgáltatás telepítési

>[!IMPORTANT]
Jelen dokumentum céljából feltételezzük, hogy verzióját használja **9.9.4510.1** vagy újabb verzióját.

Ez a cikk azt mutatja, hogyan használhatók a System Center Configuration Manager toodeploy hello Azure Site Recovery mobilitási szolgáltatás az adatközpontban található. A következő előnyöket hello egy szoftverfrissítési központi telepítési eszközzel például a Configuration Manager rendelkezik:
* A friss telepítések és frissítések, a tervezett karbantartási időszakban, a szoftverfrissítések központi telepítési ütemezés
* A kiszolgálók telepítési toohundreds egyidejűleg skálázás


> [!NOTE]
> Ebben a cikkben az System Center Configuration Manager 2012 R2 toodemonstrate hello telepítési tevékenységet. Sikerült is automatizálni a mobilitási szolgáltatás telepítési [Azure Automation és célállapot-konfiguráció](site-recovery-automate-mobility-service-install.md).

## <a name="prerequisites"></a>Előfeltételek
1. Szoftverfrissítési központi telepítési eszköz, például a Configuration Manager, már üzembe van helyezve a környezetben.
  Hozzon létre két [eszközgyűjtemények](https://technet.microsoft.com/library/gg682169.aspx), egy, az összes **Windows kiszolgálók**, és egy másik összes **Linux kiszolgálók**, tooprotect kívánt Site Recovery segítségével.
3. A konfigurációs kiszolgáló, amely már regisztrálva van a Site Recovery.
4. A biztonságos hálózati fájlmegosztást (Server Message Block megosztás), amelyet hello Configuration Manager-kiszolgáló elérhető legyen.

## <a name="deploy-mobility-service-on-computers-running-windows"></a>Windows rendszerű számítógépekre telepíteni a mobilitási szolgáltatás
> [!NOTE]
> Ez a cikk feltételezi, hogy hello konfigurációs kiszolgáló IP-címének hello 192.168.3.121, és hello biztonságos hálózati fájlmegosztás \\\ContosoSecureFS\MobilityServiceInstallers.

### <a name="step-1-prepare-for-deployment"></a>1. lépés: Felkészülés a központi telepítés
1. Hozzon létre egy mappát a hello hálózati megosztást, és adjon neki nevet **MobSvcWindows**.
2. Jelentkezzen be tooyour konfigurációs kiszolgáló, és nyisson meg egy rendszergazdai parancssort.
3. Futtassa a következő parancsok toogenerate egy hozzáférési kódot fájl hello:

    `cd %ProgramData%\ASR\home\svsystems\bin`

    `genpassphrase.exe -v > MobSvc.passphrase`
4. Másolás hello **MobSvc.passphrase** hello fájlból **MobSvcWindows** mappa a hálózati megosztáson.
5. Keresse meg a toohello telepítő tárház hello konfigurációs kiszolgálón hello a következő parancs futtatásával:

   `cd %ProgramData%\ASR\home\svsystems\puhsinstallsvc\repository`

6. Másolás hello  **Microsoft-ASR\_EE\_*verzió*\_Windows\_GA\_*dátum* \_ Release.exe** toohello **MobSvcWindows** mappa a hálózati megosztáson.
7. A következő kód hello másolja, és mentse a fájt **install.bat** történő hello **MobSvcWindows** mappa.

   > [!NOTE]
   > A parancsfájl hello [CSIP] helyőrzőket cserélje le tényleges értékek hello hello IP-cím a konfigurációs kiszolgáló.

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

### <a name="step-2-create-a-package"></a>2. lépés: Csomag létrehozása

1. Jelentkezzen be tooyour Configuration Manager konzolon.
2. Keresse meg a túl**szoftverkönyvtár** > **Alkalmazáskezelés** > **csomagok**.
3. Kattintson a jobb gombbal **csomagok**, és válassza ki **csomag létrehozása**.
4. Adja meg hello neve, a leírás, a gyártó, a nyelvet és a verzió.
5. Jelölje be hello **Ez a csomag forrásfájlokat tartalmaz** jelölőnégyzetet.
6. Kattintson a **Tallózás**, és válassza hello hello telepítő tároló hálózati megosztást (\\\ContosoSecureFS\MobilityServiceInstaller\MobSvcWindows).

  ![Képernyőfelvétel a csomag és Program létrehozása varázsló](./media/site-recovery-install-mobility-service-using-sccm/create_sccm_package.png)

7. A hello **válasszon hello program típusát, amelyet az toocreate** lapon jelölje be **normál Program**, és kattintson a **következő**.

  ![Képernyőfelvétel a csomag és Program létrehozása varázsló](./media/site-recovery-install-mobility-service-using-sccm/sccm-standard-program.png)

8. A hello **adja meg a normál programra vonatkozó információk** lapon adja meg a következő bemenet hello, és kattintson a **következő**. (hello más bemeneti használhatja az alapértelmezett értékekre.)

  | **A paraméter neve** | **Érték** |
  |--|--|
  | Név | Telepítse a Microsoft Azure mobilitási szolgáltatás (Windows) |
  | Parancssor | Install.bat |
  | A program futtatható | -E a bejelentkezett felhasználó |

  ![Képernyőfelvétel a csomag és Program létrehozása varázsló](./media/site-recovery-install-mobility-service-using-sccm/sccm-program-properties.png)

9. Hello következő lapon válassza ki a hello cél operációs rendszereket. Csak a Windows Server 2012 R2, Windows Server 2012 és Windows Server 2008 R2 telepítheti a mobilitási szolgáltatást.

  ![Képernyőfelvétel a csomag és Program létrehozása varázsló](./media/site-recovery-install-mobility-service-using-sccm/sccm-program-properties-page2.png)

10. toocomplete hello varázsló, kattintson a **következő** kétszer.


> [!NOTE]
> hello parancsfájl mindkét új ügynök telepítése, Mobilitásiszolgáltatás támogatja, és frissíti a már telepített tooagents.

### <a name="step-3-deploy-hello-package"></a>3. lépés: Hello csomag központi telepítése
1. Hello Configuration Manager konzolon, kattintson jobb gombbal a csomagra, és válassza ki **tartalom terjesztése**.
  ![Képernyőfelvétel a Configuration Manager konzol](./media/site-recovery-install-mobility-service-using-sccm/sccm_distribute.png)
2. Jelölje be hello  **[terjesztési pontok](https://technet.microsoft.com/library/gg712321.aspx#BKMK_PlanForDistributionPoints)**  toowhich a hello csomagok kell másolni.
3. Teljes hello varázsló. hello csomag majd toohello replikálása elindul a megadott terjesztési pontokra.
4. A csomag terjesztését a hello után kattintson a jobb gombbal a hello csomagot, és válassza ki **telepítés**.
  ![Képernyőfelvétel a Configuration Manager konzol](./media/site-recovery-install-mobility-service-using-sccm/sccm_deploy.png)
5. Jelölje ki azokat a hello hello célgyűjteményt központi telepítéséhez, hello Előfeltételek szakaszában létrehozott Windows Server-eszközgyűjteményt.

  ![Képernyőfelvétel a szoftver központi telepítése varázsló](./media/site-recovery-install-mobility-service-using-sccm/sccm-select-target-collection.png)

6. A hello **hello tartalom célhely megadása** lapon jelölje be a **terjesztési pontok**.
7. A hello **adja meg a beállítások toocontrol hogyan a szoftver központi telepítése** lapon, győződjön meg arról, hogy hello célja **szükséges**.

  ![Képernyőfelvétel a szoftver központi telepítése varázsló](./media/site-recovery-install-mobility-service-using-sccm/sccm-deploy-select-purpose.png)

8. A hello **adja meg a központi telepítés hello ütemezése** lapján adjon meg egy ütemezést. További információkért lásd: [csomagok ütemezés](https://technet.microsoft.com/library/gg682178.aspx).
9. A hello **terjesztési pontok** lapon, az Adatközpont toohello igényeinek megfelelően hello tulajdonságainak konfigurálása. Fejezze be hello varázslót.

> [!TIP]
> felesleges tooavoid újraindul, ütemezés hello csomag telepítése során a havi karbantartási vagy szoftver frissítések ablakot.

Hello központi telepítésének állapotáról hello Configuration Manager konzol használatával figyelheti. Nyissa meg túl**figyelés** > **központi telepítések** > *[a csomag neve]*.

  ![Képernyőfelvétel a Configuration Manager beállítás toomonitor központi telepítések](./media/site-recovery-install-mobility-service-using-sccm/report.PNG)

## <a name="deploy-mobility-service-on-computers-running-linux"></a>Linux rendszerű számítógépeken a mobilitási szolgáltatás üzembe helyezése
> [!NOTE]
> Ez a cikk feltételezi, hogy hello konfigurációs kiszolgáló IP-címének hello 192.168.3.121, és hello biztonságos hálózati fájlmegosztás \\\ContosoSecureFS\MobilityServiceInstallers.

### <a name="step-1-prepare-for-deployment"></a>1. lépés: Felkészülés a központi telepítés
1. Hozzon létre egy mappát a hello hálózati megosztást, és nevezze el, **MobSvcLinux**.
2. Jelentkezzen be tooyour konfigurációs kiszolgáló, és nyisson meg egy rendszergazdai parancssort.
3. Futtassa a következő parancsok toogenerate egy hozzáférési kódot fájl hello:

    `cd %ProgramData%\ASR\home\svsystems\bin`

    `genpassphrase.exe -v > MobSvc.passphrase`
4. Másolás hello **MobSvc.passphrase** hello fájlból **MobSvcLinux** mappa a hálózati megosztáson.
5. Keresse meg a toohello telepítő tárház hello konfigurációs kiszolgálón hello parancs futtatásával:

   `cd %ProgramData%\ASR\home\svsystems\puhsinstallsvc\repository`

6. Másolás hello következő fájlok toohello **MobSvcLinux** a hálózati megosztáson mappába:
   * Microsoft-ASR\_EE\*bites RHEL6-64*release.tar.gz
   * Microsoft-ASR\_EE\*RHEL7-64\*release.tar.gz
   * Microsoft-ASR\_EE\*SLES11-SP3-64\*release.tar.gz
   * Microsoft-ASR\_EE\*SLES11-SP4-64\*release.tar.gz
   * Microsoft-ASR\_EE\*OL6-64\*release.tar.gz
   * Microsoft-ASR\_EE\*UBUNTU 14.04-64\*release.tar.gz


7. A következő kód hello másolja, és mentse a fájt **install_linux.sh** történő hello **MobSvcLinux** mappa.
   > [!NOTE]
   > A parancsfájl hello [CSIP] helyőrzőket cserélje le tényleges értékek hello hello IP-cím a konfigurációs kiszolgáló.

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

### <a name="step-2-create-a-package"></a>2. lépés: Csomag létrehozása

1. Jelentkezzen be tooyour Configuration Manager konzolon.
2. Keresse meg a túl**szoftverkönyvtár** > **Alkalmazáskezelés** > **csomagok**.
3. Kattintson a jobb gombbal **csomagok**, és válassza ki **csomag létrehozása**.
4. Adja meg hello neve, a leírás, a gyártó, a nyelvet és a verzió.
5. Jelölje be hello **Ez a csomag forrásfájlokat tartalmaz** jelölőnégyzetet.
6. Kattintson a **Tallózás**, és válassza hello hello telepítő tároló hálózati megosztást (\\\ContosoSecureFS\MobilityServiceInstaller\MobSvcLinux).

  ![Képernyőfelvétel a csomag és Program létrehozása varázsló](./media/site-recovery-install-mobility-service-using-sccm/create_sccm_package-linux.png)

7. A hello **válasszon hello program típusát, amelyet az toocreate** lapon jelölje be **normál Program**, és kattintson a **következő**.

  ![Képernyőfelvétel a csomag és Program létrehozása varázsló](./media/site-recovery-install-mobility-service-using-sccm/sccm-standard-program.png)

8. A hello **adja meg a normál programra vonatkozó információk** lapon adja meg a következő bemenet hello, és kattintson a **következő**. (hello más bemeneti használhatja az alapértelmezett értékekre.)

    | **A paraméter neve** | **Érték** |
  |--|--|
  | Név | Telepítse a Microsoft Azure mobilitási szolgáltatás (Linux) |
  | Parancssor | ./install_linux.SH |
  | A program futtatható | -E a bejelentkezett felhasználó |

  ![Képernyőfelvétel a csomag és Program létrehozása varázsló](./media/site-recovery-install-mobility-service-using-sccm/sccm-program-properties-linux.png)

9. Hello következő lapon válassza ki a **Ez a program bármilyen platformon futtatható**.
  ![Képernyőfelvétel a csomag és Program létrehozása varázsló](./media/site-recovery-install-mobility-service-using-sccm/sccm-program-properties-page2-linux.png)

10. toocomplete hello varázsló, kattintson a **következő** kétszer.

> [!NOTE]
> hello parancsfájl mindkét új ügynök telepítése, Mobilitásiszolgáltatás támogatja, és frissíti a már telepített tooagents.

### <a name="step-3-deploy-hello-package"></a>3. lépés: Hello csomag központi telepítése
1. Hello Configuration Manager konzolon, kattintson jobb gombbal a csomagra, és válassza ki **tartalom terjesztése**.
  ![Képernyőfelvétel a Configuration Manager konzol](./media/site-recovery-install-mobility-service-using-sccm/sccm_distribute.png)
2. Jelölje be hello  **[terjesztési pontok](https://technet.microsoft.com/library/gg712321.aspx#BKMK_PlanForDistributionPoints)**  toowhich a hello csomagok kell másolni.
3. Teljes hello varázsló. hello csomag majd toohello replikálása elindul a megadott terjesztési pontokra.
4. A csomag terjesztését a hello után kattintson a jobb gombbal a hello csomagot, és válassza ki **telepítés**.
  ![Képernyőfelvétel a Configuration Manager konzol](./media/site-recovery-install-mobility-service-using-sccm/sccm_deploy.png)
5. Válassza ki a hello hello célgyűjteményt központi telepítéséhez, hello Előfeltételek szakaszában létrehozott Linux Server eszközgyűjteményt.

  ![Képernyőfelvétel a szoftver központi telepítése varázsló](./media/site-recovery-install-mobility-service-using-sccm/sccm-select-target-collection-linux.png)

6. A hello **hello tartalom célhely megadása** lapon jelölje be a **terjesztési pontok**.
7. A hello **adja meg a beállítások toocontrol hogyan a szoftver központi telepítése** lapon, győződjön meg arról, hogy hello célja **szükséges**.

  ![Képernyőfelvétel a szoftver központi telepítése varázsló](./media/site-recovery-install-mobility-service-using-sccm/sccm-deploy-select-purpose.png)

8. A hello **adja meg a központi telepítés hello ütemezése** lapján adjon meg egy ütemezést. További információkért lásd: [csomagok ütemezés](https://technet.microsoft.com/library/gg682178.aspx).
9. A hello **terjesztési pontok** lapon, az Adatközpont toohello igényeinek megfelelően hello tulajdonságainak konfigurálása. Fejezze be hello varázslót.

Lekérdezi hello Linux Server eszközgyűjteményt, konfigurált toohello ütemezés szerint telepíteni a mobilitási szolgáltatást.

## <a name="other-methods-tooinstall-mobility-service"></a>Egyéb módszerek tooinstall mobilitási szolgáltatás
Az alábbiakban néhány más beállításokat a mobilitási szolgáltatás telepítése:
* [Grafikus felhasználói felülettel manuális telepítés](http://aka.ms/mobsvcmanualinstall)
* [Manuális telepítés parancssori használata](http://aka.ms/mobsvcmanualinstallcli)
* [Ügyfélleküldéses telepítés konfigurációs kiszolgáló használatával](http://aka.ms/pushinstall)
* [Azure Automation és célállapot-konfiguráció automatikus telepítés](http://aka.ms/mobsvcdscinstall)

## <a name="uninstall-mobility-service"></a>Távolítsa el a mobilitási szolgáltatás
A Configuration Manager csomagok toouninstall Mobilitásiszolgáltatás hozhat létre. Ezért a következő parancsfájl toodo hello használata:

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

## <a name="next-steps"></a>Következő lépések
Most már készen áll túl[engedélyezni a védelmet](https://docs.microsoft.com/en-us/azure/site-recovery/site-recovery-vmware-to-azure#step-6-replicate-applications) a virtuális gépek számára.
