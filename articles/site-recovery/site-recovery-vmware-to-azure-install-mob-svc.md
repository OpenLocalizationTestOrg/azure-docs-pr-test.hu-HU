---
title: "aaaInstall mobilitási szolgáltatás (VMware vagy fizikai tooAzure) |} Microsoft Docs"
description: "Ismerje meg, hogyan tooinstall hello Mobilitásiszolgáltatás ügynök tooprotect a helyszíni számítógépek."
services: site-recovery
documentationcenter: 
author: AnoopVasudavan
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: backup-recovery
ms.date: 06/29/2017
ms.author: anoopkv
ms.openlocfilehash: f7836e6b35d3838bae1eff927838ce4b245b9f56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-mobility-service-vmware-or-physical-tooazure"></a>Telepítse a mobilitási szolgáltatás (VMware vagy fizikai tooAzure)
Az Azure Site Recovery mobilitási szolgáltatás egy számítógépen végbemenő adatírásokat, és ezután továbbítja azokat toohello folyamatkiszolgáló. Telepítse a mobilitási szolgáltatás tooevery számítógép (VMware virtuális gép vagy fizikai kiszolgálón), amelyet az tooreplicate tooAzure. Megjeleníteni kívánt tooprotect a következő módszerek hello segítségével a mobilitási szolgáltatás toohello kiszolgálók telepíthetők:


* [Telepítse a mobilitási szolgáltatás szoftvertelepítési eszközök például a System Center Configuration Manager használatával](site-recovery-install-mobility-service-using-sccm.md)
* [Mobilitási szolgáltatás telepítése Azure Automation és célállapot-konfiguráció (Automation DSC)](site-recovery-automate-mobility-service-install.md)
* [Telepítse manuálisan a mobilitási szolgáltatás hello grafikus felhasználói felületen (GUI) használatával](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui)
* [Telepítse manuálisan a mobilitási szolgáltatást a parancssorba](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-at-a-command-prompt)
* [Telepítse a mobilitási szolgáltatás leküldéses telepítése az Azure Site Recovery által](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery)


>[!IMPORTANT]
> Verziójától 9.7.0.0, a Windows virtuális gépek (VM) hello Mobilitásiszolgáltatás is telepítője hello legújabb elérhető [Azure Virtuálisgép-ügynök](../virtual-machines/windows/extensions-features.md#azure-vm-agent). Ha a számítógép nem tooAzure keresztül, hello számítógép megfelel-e a Virtuálisgép-bővítmény használatára vonatkozó Előfeltételek hello az ügynök telepítése.

## <a name="prerequisites"></a>Előfeltételek
Előfeltételként szükséges lépések végrehajtása előtt manuálisan telepítenie mobilitási szolgáltatás a kiszolgálón:
1. Jelentkezzen be tooyour konfigurációs kiszolgáló, és nyissa meg egy parancssori ablakot rendszergazdaként.
2. Módosítsa a hello directory toohello bin mappát, majd hozza létre jelszót fájlt:

    ```
    cd %ProgramData%\ASR\home\svsystems\bin
    genpassphrase.exe -v > MobSvc.passphrase
    ```
3. Hello jelszót fájlt tárolja biztonságos helyen. Hello fájlt hello mobilitási szolgáltatás telepítése során használja.
4. Minden támogatott operációs rendszerek a mobilitási szolgáltatás telepítők hello %ProgramData%\ASR\home\svsystems\pushinstallsvc\repository mappában találhatók.

### <a name="mobility-service-installer-to-operating-system-mapping"></a>Mobilitási szolgáltatás installer-operációs rendszer leképezése

| Telepítési fájl neve| Operációs rendszer |
|---|--|
|Microsoft-ASR\_EE\*Windows\*release.exe | Windows Server 2008 R2 SP1 (64 bites) </br> A Windows Server 2012 (64 bites) </br> Windows Server 2012 R2 (64 bites) |
|Microsoft-ASR\_EE\*bites RHEL6-64*release.tar.gz| Red Hat Enterprise Linux (RHEL) 6.4 6.5, 6.6, 6.7, 6.8 (csak 64 bites verzió esetén) </br> CentOS 6.4, 6.5, 6.6, 6.7, 6.8 (csak 64 bites verzió esetén) |
|Microsoft-ASR\_EE\*RHEL7-64\*release.tar.gz | Red Hat Enterprise Linux (RHEL) 7.1, 7.2 (csak 64 bites verzió esetén) </br> CentOS 7.0, 7.1, 7.2 (csak 64 bites verzió esetén)</br> 7.3 (csak az áttelepítés esetén) – centOs |
|Microsoft-ASR\_EE\*SLES11-SP3-64\*release.tar.gz| SUSE Linux Enterprise Server 11 SP3 (csak 64 bites verzió esetén)|
|Microsoft-ASR\_EE\*SLES11-SP4-64\*release.tar.gz| SUSE Linux Enterprise Server 11 SP4 (csak 64 bites verzió esetén)|
|Microsoft-ASR\_EE\*OL6-64\*release.tar.gz | Oracle Enterprise Linux 6.4 6.5 (csak 64 bites verzió esetén)|
|Microsoft-ASR\_EE\*UBUNTU 14.04-64\*release.tar.gz | Ubuntu Linux 14.04 (csak 64 bites verzió esetén)|


## <a name="install-mobility-service-manually-by-using-hello-gui"></a>Telepítse manuálisan a mobilitási szolgáltatás hello grafikus felhasználói felület használatával

>[!IMPORTANT]
> Ha használ egy **konfigurációs kiszolgáló** tooreplicate **Azure IaaS virtuális gépek** egy Azure-előfizetés vagy régió tooanother majd a **hello parancssori alapú telepítés**  módszer

[!INCLUDE [site-recovery-install-mob-svc-gui](../../includes/site-recovery-install-mob-svc-gui.md)]

## <a name="install-mobility-service-manually-at-a-command-prompt"></a>Telepítse manuálisan a mobilitási szolgáltatást a parancssorba

### <a name="command-line-installation-on-a-windows-computer"></a>Parancssori telepítés Windows-számítógépen
[!INCLUDE [site-recovery-install-mob-svc-win-cmd](../../includes/site-recovery-install-mob-svc-win-cmd.md)]

### <a name="command-line-installation-on-a-linux-computer"></a>Parancssori telepítés, a Linux számítógépre
[!INCLUDE [site-recovery-install-mob-svc-lin-cmd](../../includes/site-recovery-install-mob-svc-lin-cmd.md)]


## <a name="install-mobility-service-by-push-installation-from-azure-site-recovery"></a>Telepítse a mobilitási szolgáltatás leküldéses telepítése az Azure Site Recovery által
a Site Recovery mobilitási szolgáltatás leküldéses telepítéséhez toodo, minden célszámítógépen meg kell felelnie a következő előfeltételek hello.

[!INCLUDE [site-recovery-prepare-push-install-mob-svc-win](../../includes/site-recovery-prepare-push-install-mob-svc-win.md)]

[!INCLUDE [site-recovery-prepare-push-install-mob-svc-lin](../../includes/site-recovery-prepare-push-install-mob-svc-lin.md)]


> [!NOTE]
Hello Azure-portálon a mobilitási szolgáltatás telepítése után válasszon hello **replikálása** gomb toostart védi a virtuális gépeken.

## <a name="uninstall-mobility-service-on-a-windows-server-computer"></a>Távolítsa el a mobilitási szolgáltatást a Windows Server rendszerben
Hello módszerek toouninstall mobilitási szolgáltatás egy Windows Server-számítógépen a következő egyikét használhatja.

### <a name="uninstall-by-using-hello-gui"></a>Távolítsa el a hello grafikus felhasználói felület használatával
1. A Vezérlőpulton válassza **programok**.
2. Válassza ki **Microsoft Azure Site helyreállítási mobilitási szolgáltatás/fő célkiszolgáló**, majd válassza ki **Eltávolítás**.

### <a name="uninstall-at-a-command-prompt"></a>Távolítsa el a parancssorba
1. Nyisson meg egy parancssori ablakot rendszergazdaként.
2. toouninstall mobilitási szolgáltatást, futtassa a következő parancs hello:

```
MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
```

## <a name="uninstall-mobility-service-on-a-linux-computer"></a>Távolítsa el a mobilitási szolgáltatás egy Linux-számítógép
1. A Linux-kiszolgálón jelentkezzen be egy **legfelső szintű** felhasználó.
2. A Terminálszolgáltatások lépjen túl/felhasználó/helyi/automatikus rendszer-Helyreállítás.
3. toouninstall mobilitási szolgáltatást, futtassa a következő parancs hello:

```
uninstall.sh -Y
```
