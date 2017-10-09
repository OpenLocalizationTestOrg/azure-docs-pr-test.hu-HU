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
# <a name="install-mobility-service-vmware-or-physical-tooazure"></a><span data-ttu-id="f2326-103">Telepítse a mobilitási szolgáltatás (VMware vagy fizikai tooAzure)</span><span class="sxs-lookup"><span data-stu-id="f2326-103">Install Mobility Service (VMware or physical tooAzure)</span></span>
<span data-ttu-id="f2326-104">Az Azure Site Recovery mobilitási szolgáltatás egy számítógépen végbemenő adatírásokat, és ezután továbbítja azokat toohello folyamatkiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="f2326-104">Azure Site Recovery Mobility Service captures data writes on a computer, and then forwards them toohello process server.</span></span> <span data-ttu-id="f2326-105">Telepítse a mobilitási szolgáltatás tooevery számítógép (VMware virtuális gép vagy fizikai kiszolgálón), amelyet az tooreplicate tooAzure.</span><span class="sxs-lookup"><span data-stu-id="f2326-105">Deploy Mobility Service tooevery computer (VMware VM or physical server) that you want tooreplicate tooAzure.</span></span> <span data-ttu-id="f2326-106">Megjeleníteni kívánt tooprotect a következő módszerek hello segítségével a mobilitási szolgáltatás toohello kiszolgálók telepíthetők:</span><span class="sxs-lookup"><span data-stu-id="f2326-106">You can deploy Mobility Service toohello servers that you want tooprotect by using hello following methods:</span></span>


* [<span data-ttu-id="f2326-107">Telepítse a mobilitási szolgáltatás szoftvertelepítési eszközök például a System Center Configuration Manager használatával</span><span class="sxs-lookup"><span data-stu-id="f2326-107">Install Mobility Service by using software deployment tools like System Center Configuration Manager</span></span>](site-recovery-install-mobility-service-using-sccm.md)
* [<span data-ttu-id="f2326-108">Mobilitási szolgáltatás telepítése Azure Automation és célállapot-konfiguráció (Automation DSC)</span><span class="sxs-lookup"><span data-stu-id="f2326-108">Install Mobility Service by using Azure Automation and Desired State Configuration (Automation DSC)</span></span>](site-recovery-automate-mobility-service-install.md)
* [<span data-ttu-id="f2326-109">Telepítse manuálisan a mobilitási szolgáltatás hello grafikus felhasználói felületen (GUI) használatával</span><span class="sxs-lookup"><span data-stu-id="f2326-109">Install Mobility Service manually by using hello graphical user interface (GUI)</span></span>](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui)
* [<span data-ttu-id="f2326-110">Telepítse manuálisan a mobilitási szolgáltatást a parancssorba</span><span class="sxs-lookup"><span data-stu-id="f2326-110">Install Mobility Service manually at a command prompt</span></span>](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-at-a-command-prompt)
* [<span data-ttu-id="f2326-111">Telepítse a mobilitási szolgáltatás leküldéses telepítése az Azure Site Recovery által</span><span class="sxs-lookup"><span data-stu-id="f2326-111">Install Mobility Service by push installation from Azure Site Recovery</span></span>](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery)


>[!IMPORTANT]
> <span data-ttu-id="f2326-112">Verziójától 9.7.0.0, a Windows virtuális gépek (VM) hello Mobilitásiszolgáltatás is telepítője hello legújabb elérhető [Azure Virtuálisgép-ügynök](../virtual-machines/windows/extensions-features.md#azure-vm-agent).</span><span class="sxs-lookup"><span data-stu-id="f2326-112">Beginning with version 9.7.0.0, on Windows virtual machines (VMs), hello Mobility Service installer also installs hello latest available [Azure VM agent](../virtual-machines/windows/extensions-features.md#azure-vm-agent).</span></span> <span data-ttu-id="f2326-113">Ha a számítógép nem tooAzure keresztül, hello számítógép megfelel-e a Virtuálisgép-bővítmény használatára vonatkozó Előfeltételek hello az ügynök telepítése.</span><span class="sxs-lookup"><span data-stu-id="f2326-113">When a computer fails over tooAzure, hello computer meets hello agent installation prerequisite for using any VM extension.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f2326-114">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f2326-114">Prerequisites</span></span>
<span data-ttu-id="f2326-115">Előfeltételként szükséges lépések végrehajtása előtt manuálisan telepítenie mobilitási szolgáltatás a kiszolgálón:</span><span class="sxs-lookup"><span data-stu-id="f2326-115">Complete these prerequisite steps before you manually install Mobility Service on your server:</span></span>
1. <span data-ttu-id="f2326-116">Jelentkezzen be tooyour konfigurációs kiszolgáló, és nyissa meg egy parancssori ablakot rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="f2326-116">Sign in tooyour configuration server, and then open a Command Prompt window as an administrator.</span></span>
2. <span data-ttu-id="f2326-117">Módosítsa a hello directory toohello bin mappát, majd hozza létre jelszót fájlt:</span><span class="sxs-lookup"><span data-stu-id="f2326-117">Change hello directory toohello bin folder, and then create a passphrase file:</span></span>

    ```
    cd %ProgramData%\ASR\home\svsystems\bin
    genpassphrase.exe -v > MobSvc.passphrase
    ```
3. <span data-ttu-id="f2326-118">Hello jelszót fájlt tárolja biztonságos helyen.</span><span class="sxs-lookup"><span data-stu-id="f2326-118">Store hello passphrase file in a secure location.</span></span> <span data-ttu-id="f2326-119">Hello fájlt hello mobilitási szolgáltatás telepítése során használja.</span><span class="sxs-lookup"><span data-stu-id="f2326-119">You use hello file during hello Mobility Service installation.</span></span>
4. <span data-ttu-id="f2326-120">Minden támogatott operációs rendszerek a mobilitási szolgáltatás telepítők hello %ProgramData%\ASR\home\svsystems\pushinstallsvc\repository mappában találhatók.</span><span class="sxs-lookup"><span data-stu-id="f2326-120">Mobility Service installers for all supported operating systems are in hello %ProgramData%\ASR\home\svsystems\pushinstallsvc\repository folder.</span></span>

### <a name="mobility-service-installer-to-operating-system-mapping"></a><span data-ttu-id="f2326-121">Mobilitási szolgáltatás installer-operációs rendszer leképezése</span><span class="sxs-lookup"><span data-stu-id="f2326-121">Mobility Service installer-to-operating system mapping</span></span>

| <span data-ttu-id="f2326-122">Telepítési fájl neve</span><span class="sxs-lookup"><span data-stu-id="f2326-122">Installer file template name</span></span>| <span data-ttu-id="f2326-123">Operációs rendszer</span><span class="sxs-lookup"><span data-stu-id="f2326-123">Operating system</span></span> |
|---|--|
|<span data-ttu-id="f2326-124">Microsoft-ASR\_EE\*Windows\*release.exe</span><span class="sxs-lookup"><span data-stu-id="f2326-124">Microsoft-ASR\_UA\*Windows\*release.exe</span></span> | <span data-ttu-id="f2326-125">Windows Server 2008 R2 SP1 (64 bites)</span><span class="sxs-lookup"><span data-stu-id="f2326-125">Windows Server 2008 R2 SP1 (64-bit)</span></span> </br> <span data-ttu-id="f2326-126">A Windows Server 2012 (64 bites)</span><span class="sxs-lookup"><span data-stu-id="f2326-126">Windows Server 2012 (64-bit)</span></span> </br> <span data-ttu-id="f2326-127">Windows Server 2012 R2 (64 bites)</span><span class="sxs-lookup"><span data-stu-id="f2326-127">Windows Server 2012 R2 (64-bit)</span></span> |
|<span data-ttu-id="f2326-128">Microsoft-ASR\_EE\*bites RHEL6-64*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="f2326-128">Microsoft-ASR\_UA\*RHEL6-64*release.tar.gz</span></span>| <span data-ttu-id="f2326-129">Red Hat Enterprise Linux (RHEL) 6.4 6.5, 6.6, 6.7, 6.8 (csak 64 bites verzió esetén)</span><span class="sxs-lookup"><span data-stu-id="f2326-129">Red Hat Enterprise Linux (RHEL) 6.4, 6.5, 6.6, 6.7, 6.8 (64-bit only)</span></span> </br> <span data-ttu-id="f2326-130">CentOS 6.4, 6.5, 6.6, 6.7, 6.8 (csak 64 bites verzió esetén)</span><span class="sxs-lookup"><span data-stu-id="f2326-130">CentOS 6.4, 6.5, 6.6, 6.7, 6.8 (64-bit only)</span></span> |
|<span data-ttu-id="f2326-131">Microsoft-ASR\_EE\*RHEL7-64\*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="f2326-131">Microsoft-ASR\_UA\*RHEL7-64\*release.tar.gz</span></span> | <span data-ttu-id="f2326-132">Red Hat Enterprise Linux (RHEL) 7.1, 7.2 (csak 64 bites verzió esetén)</span><span class="sxs-lookup"><span data-stu-id="f2326-132">Red Hat Enterprise Linux (RHEL) 7.1, 7.2 (64-bit only)</span></span> </br> <span data-ttu-id="f2326-133">CentOS 7.0, 7.1, 7.2 (csak 64 bites verzió esetén)</span><span class="sxs-lookup"><span data-stu-id="f2326-133">CentOS 7.0, 7.1, 7.2 (64-bit only)</span></span></br> <span data-ttu-id="f2326-134">7.3 (csak az áttelepítés esetén) – centOs</span><span class="sxs-lookup"><span data-stu-id="f2326-134">CentOs 7.3 (migration only)</span></span> |
|<span data-ttu-id="f2326-135">Microsoft-ASR\_EE\*SLES11-SP3-64\*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="f2326-135">Microsoft-ASR\_UA\*SLES11-SP3-64\*release.tar.gz</span></span>| <span data-ttu-id="f2326-136">SUSE Linux Enterprise Server 11 SP3 (csak 64 bites verzió esetén)</span><span class="sxs-lookup"><span data-stu-id="f2326-136">SUSE Linux Enterprise Server 11 SP3 (64-bit only)</span></span>|
|<span data-ttu-id="f2326-137">Microsoft-ASR\_EE\*SLES11-SP4-64\*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="f2326-137">Microsoft-ASR\_UA\*SLES11-SP4-64\*release.tar.gz</span></span>| <span data-ttu-id="f2326-138">SUSE Linux Enterprise Server 11 SP4 (csak 64 bites verzió esetén)</span><span class="sxs-lookup"><span data-stu-id="f2326-138">SUSE Linux Enterprise Server 11 SP4 (64-bit only)</span></span>|
|<span data-ttu-id="f2326-139">Microsoft-ASR\_EE\*OL6-64\*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="f2326-139">Microsoft-ASR\_UA\*OL6-64\*release.tar.gz</span></span> | <span data-ttu-id="f2326-140">Oracle Enterprise Linux 6.4 6.5 (csak 64 bites verzió esetén)</span><span class="sxs-lookup"><span data-stu-id="f2326-140">Oracle Enterprise Linux 6.4, 6.5 (64-bit only)</span></span>|
|<span data-ttu-id="f2326-141">Microsoft-ASR\_EE\*UBUNTU 14.04-64\*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="f2326-141">Microsoft-ASR\_UA\*UBUNTU-14.04-64\*release.tar.gz</span></span> | <span data-ttu-id="f2326-142">Ubuntu Linux 14.04 (csak 64 bites verzió esetén)</span><span class="sxs-lookup"><span data-stu-id="f2326-142">Ubuntu Linux 14.04 (64-bit only)</span></span>|


## <a name="install-mobility-service-manually-by-using-hello-gui"></a><span data-ttu-id="f2326-143">Telepítse manuálisan a mobilitási szolgáltatás hello grafikus felhasználói felület használatával</span><span class="sxs-lookup"><span data-stu-id="f2326-143">Install Mobility Service manually by using hello GUI</span></span>

>[!IMPORTANT]
> <span data-ttu-id="f2326-144">Ha használ egy **konfigurációs kiszolgáló** tooreplicate **Azure IaaS virtuális gépek** egy Azure-előfizetés vagy régió tooanother majd a **hello parancssori alapú telepítés**  módszer</span><span class="sxs-lookup"><span data-stu-id="f2326-144">If you are using a **Configuration Server** tooreplicate **Azure IaaS virtual machines** from one Azure Subscription/Region tooanother then **use hello Command-line based installation** method</span></span>

[!INCLUDE [site-recovery-install-mob-svc-gui](../../includes/site-recovery-install-mob-svc-gui.md)]

## <a name="install-mobility-service-manually-at-a-command-prompt"></a><span data-ttu-id="f2326-145">Telepítse manuálisan a mobilitási szolgáltatást a parancssorba</span><span class="sxs-lookup"><span data-stu-id="f2326-145">Install Mobility Service manually at a command prompt</span></span>

### <a name="command-line-installation-on-a-windows-computer"></a><span data-ttu-id="f2326-146">Parancssori telepítés Windows-számítógépen</span><span class="sxs-lookup"><span data-stu-id="f2326-146">Command-line installation on a Windows computer</span></span>
[!INCLUDE [site-recovery-install-mob-svc-win-cmd](../../includes/site-recovery-install-mob-svc-win-cmd.md)]

### <a name="command-line-installation-on-a-linux-computer"></a><span data-ttu-id="f2326-147">Parancssori telepítés, a Linux számítógépre</span><span class="sxs-lookup"><span data-stu-id="f2326-147">Command-line installation on a Linux computer</span></span>
[!INCLUDE [site-recovery-install-mob-svc-lin-cmd](../../includes/site-recovery-install-mob-svc-lin-cmd.md)]


## <a name="install-mobility-service-by-push-installation-from-azure-site-recovery"></a><span data-ttu-id="f2326-148">Telepítse a mobilitási szolgáltatás leküldéses telepítése az Azure Site Recovery által</span><span class="sxs-lookup"><span data-stu-id="f2326-148">Install Mobility Service by push installation from Azure Site Recovery</span></span>
<span data-ttu-id="f2326-149">a Site Recovery mobilitási szolgáltatás leküldéses telepítéséhez toodo, minden célszámítógépen meg kell felelnie a következő előfeltételek hello.</span><span class="sxs-lookup"><span data-stu-id="f2326-149">toodo a push installation of Mobility Service by using Site Recovery, all target computers must meet hello following prerequisites.</span></span>

[!INCLUDE [site-recovery-prepare-push-install-mob-svc-win](../../includes/site-recovery-prepare-push-install-mob-svc-win.md)]

[!INCLUDE [site-recovery-prepare-push-install-mob-svc-lin](../../includes/site-recovery-prepare-push-install-mob-svc-lin.md)]


> [!NOTE]
<span data-ttu-id="f2326-150">Hello Azure-portálon a mobilitási szolgáltatás telepítése után válasszon hello **replikálása** gomb toostart védi a virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="f2326-150">After Mobility Service is installed, in hello Azure portal, select hello **Replicate** button toostart protecting these VMs.</span></span>

## <a name="uninstall-mobility-service-on-a-windows-server-computer"></a><span data-ttu-id="f2326-151">Távolítsa el a mobilitási szolgáltatást a Windows Server rendszerben</span><span class="sxs-lookup"><span data-stu-id="f2326-151">Uninstall Mobility Service on a Windows Server computer</span></span>
<span data-ttu-id="f2326-152">Hello módszerek toouninstall mobilitási szolgáltatás egy Windows Server-számítógépen a következő egyikét használhatja.</span><span class="sxs-lookup"><span data-stu-id="f2326-152">Use one of hello following methods toouninstall Mobility Service on a Windows Server computer.</span></span>

### <a name="uninstall-by-using-hello-gui"></a><span data-ttu-id="f2326-153">Távolítsa el a hello grafikus felhasználói felület használatával</span><span class="sxs-lookup"><span data-stu-id="f2326-153">Uninstall by using hello GUI</span></span>
1. <span data-ttu-id="f2326-154">A Vezérlőpulton válassza **programok**.</span><span class="sxs-lookup"><span data-stu-id="f2326-154">In Control Panel, select **Programs**.</span></span>
2. <span data-ttu-id="f2326-155">Válassza ki **Microsoft Azure Site helyreállítási mobilitási szolgáltatás/fő célkiszolgáló**, majd válassza ki **Eltávolítás**.</span><span class="sxs-lookup"><span data-stu-id="f2326-155">Select **Microsoft Azure Site Recovery Mobility Service/Master Target server**, and then select **Uninstall**.</span></span>

### <a name="uninstall-at-a-command-prompt"></a><span data-ttu-id="f2326-156">Távolítsa el a parancssorba</span><span class="sxs-lookup"><span data-stu-id="f2326-156">Uninstall at a command prompt</span></span>
1. <span data-ttu-id="f2326-157">Nyisson meg egy parancssori ablakot rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="f2326-157">Open a Command Prompt window as an administrator.</span></span>
2. <span data-ttu-id="f2326-158">toouninstall mobilitási szolgáltatást, futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="f2326-158">toouninstall Mobility Service, run hello following command:</span></span>

```
MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
```

## <a name="uninstall-mobility-service-on-a-linux-computer"></a><span data-ttu-id="f2326-159">Távolítsa el a mobilitási szolgáltatás egy Linux-számítógép</span><span class="sxs-lookup"><span data-stu-id="f2326-159">Uninstall Mobility Service on a Linux computer</span></span>
1. <span data-ttu-id="f2326-160">A Linux-kiszolgálón jelentkezzen be egy **legfelső szintű** felhasználó.</span><span class="sxs-lookup"><span data-stu-id="f2326-160">On your Linux server, sign in as a **root** user.</span></span>
2. <span data-ttu-id="f2326-161">A Terminálszolgáltatások lépjen túl/felhasználó/helyi/automatikus rendszer-Helyreállítás.</span><span class="sxs-lookup"><span data-stu-id="f2326-161">In a terminal, go too/user/local/ASR.</span></span>
3. <span data-ttu-id="f2326-162">toouninstall mobilitási szolgáltatást, futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="f2326-162">toouninstall Mobility Service, run hello following command:</span></span>

```
uninstall.sh -Y
```
