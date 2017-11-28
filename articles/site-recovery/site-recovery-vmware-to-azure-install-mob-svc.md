---
title: "Telepítse a mobilitási szolgáltatás (VMware vagy fizikai az Azure-bA) |} Microsoft Docs"
description: "Ismerje meg, hogyan kell telepíteni a mobilitási szolgáltatás ügynököt a helyi számítógép védelme érdekében."
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
ms.openlocfilehash: 848284f37ae2470a169d8f8a8c9c0bb5b926abe3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="install-mobility-service-vmware-or-physical-to-azure"></a><span data-ttu-id="094c9-103">Telepítse a mobilitási szolgáltatás (VMware vagy fizikai az Azure-bA)</span><span class="sxs-lookup"><span data-stu-id="094c9-103">Install Mobility Service (VMware or physical to Azure)</span></span>
<span data-ttu-id="094c9-104">Az Azure Site Recovery mobilitási szolgáltatás egy számítógépen végbemenő adatírásokat, és ezután továbbítja őket a folyamatkiszolgálónak.</span><span class="sxs-lookup"><span data-stu-id="094c9-104">Azure Site Recovery Mobility Service captures data writes on a computer, and then forwards them to the process server.</span></span> <span data-ttu-id="094c9-105">Minden számítógépre (VMware virtuális gép vagy fizikai kiszolgálón), amely az Azure-bA replikálni kívánt telepíteni a mobilitási szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="094c9-105">Deploy Mobility Service to every computer (VMware VM or physical server) that you want to replicate to Azure.</span></span> <span data-ttu-id="094c9-106">Az alábbi módszerek használatával védeni kívánt kiszolgálók mobilitási szolgáltatás telepítése:</span><span class="sxs-lookup"><span data-stu-id="094c9-106">You can deploy Mobility Service to the servers that you want to protect by using the following methods:</span></span>


* [<span data-ttu-id="094c9-107">Telepítse a mobilitási szolgáltatás szoftvertelepítési eszközök például a System Center Configuration Manager használatával</span><span class="sxs-lookup"><span data-stu-id="094c9-107">Install Mobility Service by using software deployment tools like System Center Configuration Manager</span></span>](site-recovery-install-mobility-service-using-sccm.md)
* [<span data-ttu-id="094c9-108">Mobilitási szolgáltatás telepítése Azure Automation és célállapot-konfiguráció (Automation DSC)</span><span class="sxs-lookup"><span data-stu-id="094c9-108">Install Mobility Service by using Azure Automation and Desired State Configuration (Automation DSC)</span></span>](site-recovery-automate-mobility-service-install.md)
* [<span data-ttu-id="094c9-109">Telepítse manuálisan a mobilitási szolgáltatást a grafikus felhasználói felületen (GUI)</span><span class="sxs-lookup"><span data-stu-id="094c9-109">Install Mobility Service manually by using the graphical user interface (GUI)</span></span>](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui)
* [<span data-ttu-id="094c9-110">Telepítse manuálisan a mobilitási szolgáltatást a parancssorba</span><span class="sxs-lookup"><span data-stu-id="094c9-110">Install Mobility Service manually at a command prompt</span></span>](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-at-a-command-prompt)
* [<span data-ttu-id="094c9-111">Telepítse a mobilitási szolgáltatás leküldéses telepítése az Azure Site Recovery által</span><span class="sxs-lookup"><span data-stu-id="094c9-111">Install Mobility Service by push installation from Azure Site Recovery</span></span>](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery)


>[!IMPORTANT]
> <span data-ttu-id="094c9-112">Kezdve verziója 9.7.0.0, a Windows virtuális gépek (VM), a mobilitási szolgáltatás telepítési is telepíti a legújabb elérhető [Azure Virtuálisgép-ügynök](../virtual-machines/windows/extensions-features.md#azure-vm-agent).</span><span class="sxs-lookup"><span data-stu-id="094c9-112">Beginning with version 9.7.0.0, on Windows virtual machines (VMs), the Mobility Service installer also installs the latest available [Azure VM agent](../virtual-machines/windows/extensions-features.md#azure-vm-agent).</span></span> <span data-ttu-id="094c9-113">Ha a számítógép nem keresztül az Azure-ba, a számítógép megfelel-e a Virtuálisgép-bővítmény használatára vonatkozó előfeltételek telepítése.</span><span class="sxs-lookup"><span data-stu-id="094c9-113">When a computer fails over to Azure, the computer meets the agent installation prerequisite for using any VM extension.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="094c9-114">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="094c9-114">Prerequisites</span></span>
<span data-ttu-id="094c9-115">Előfeltételként szükséges lépések végrehajtása előtt manuálisan telepítenie mobilitási szolgáltatás a kiszolgálón:</span><span class="sxs-lookup"><span data-stu-id="094c9-115">Complete these prerequisite steps before you manually install Mobility Service on your server:</span></span>
1. <span data-ttu-id="094c9-116">Jelentkezzen be a konfigurációs kiszolgáló, és nyissa meg egy parancssori ablakot rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="094c9-116">Sign in to your configuration server, and then open a Command Prompt window as an administrator.</span></span>
2. <span data-ttu-id="094c9-117">Módosítsa a könyvtárat a bin mappát, és hozza létre jelszót fájlt:</span><span class="sxs-lookup"><span data-stu-id="094c9-117">Change the directory to the bin folder, and then create a passphrase file:</span></span>

    ```
    cd %ProgramData%\ASR\home\svsystems\bin
    genpassphrase.exe -v > MobSvc.passphrase
    ```
3. <span data-ttu-id="094c9-118">A jelszó fájlt tárolja biztonságos helyen.</span><span class="sxs-lookup"><span data-stu-id="094c9-118">Store the passphrase file in a secure location.</span></span> <span data-ttu-id="094c9-119">A fájlt a mobilitási szolgáltatás telepítése során használja.</span><span class="sxs-lookup"><span data-stu-id="094c9-119">You use the file during the Mobility Service installation.</span></span>
4. <span data-ttu-id="094c9-120">Minden támogatott operációs rendszerek a mobilitási szolgáltatás telepítők a %ProgramData%\ASR\home\svsystems\pushinstallsvc\repository mappában találhatók.</span><span class="sxs-lookup"><span data-stu-id="094c9-120">Mobility Service installers for all supported operating systems are in the %ProgramData%\ASR\home\svsystems\pushinstallsvc\repository folder.</span></span>

### <a name="mobility-service-installer-to-operating-system-mapping"></a><span data-ttu-id="094c9-121">Mobilitási szolgáltatás installer-operációs rendszer leképezése</span><span class="sxs-lookup"><span data-stu-id="094c9-121">Mobility Service installer-to-operating system mapping</span></span>

| <span data-ttu-id="094c9-122">Telepítési fájl neve</span><span class="sxs-lookup"><span data-stu-id="094c9-122">Installer file template name</span></span>| <span data-ttu-id="094c9-123">Operációs rendszer</span><span class="sxs-lookup"><span data-stu-id="094c9-123">Operating system</span></span> |
|---|--|
|<span data-ttu-id="094c9-124">Microsoft-ASR\_EE\*Windows\*release.exe</span><span class="sxs-lookup"><span data-stu-id="094c9-124">Microsoft-ASR\_UA\*Windows\*release.exe</span></span> | <span data-ttu-id="094c9-125">Windows Server 2008 R2 SP1 (64 bites)</span><span class="sxs-lookup"><span data-stu-id="094c9-125">Windows Server 2008 R2 SP1 (64-bit)</span></span> </br> <span data-ttu-id="094c9-126">A Windows Server 2012 (64 bites)</span><span class="sxs-lookup"><span data-stu-id="094c9-126">Windows Server 2012 (64-bit)</span></span> </br> <span data-ttu-id="094c9-127">Windows Server 2012 R2 (64 bites)</span><span class="sxs-lookup"><span data-stu-id="094c9-127">Windows Server 2012 R2 (64-bit)</span></span> |
|<span data-ttu-id="094c9-128">Microsoft-ASR\_EE\*bites RHEL6-64*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="094c9-128">Microsoft-ASR\_UA\*RHEL6-64*release.tar.gz</span></span>| <span data-ttu-id="094c9-129">Red Hat Enterprise Linux (RHEL) 6.4 6.5, 6.6, 6.7, 6.8 (csak 64 bites verzió esetén)</span><span class="sxs-lookup"><span data-stu-id="094c9-129">Red Hat Enterprise Linux (RHEL) 6.4, 6.5, 6.6, 6.7, 6.8 (64-bit only)</span></span> </br> <span data-ttu-id="094c9-130">CentOS 6.4, 6.5, 6.6, 6.7, 6.8 (csak 64 bites verzió esetén)</span><span class="sxs-lookup"><span data-stu-id="094c9-130">CentOS 6.4, 6.5, 6.6, 6.7, 6.8 (64-bit only)</span></span> |
|<span data-ttu-id="094c9-131">Microsoft-ASR\_EE\*RHEL7-64\*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="094c9-131">Microsoft-ASR\_UA\*RHEL7-64\*release.tar.gz</span></span> | <span data-ttu-id="094c9-132">Red Hat Enterprise Linux (RHEL) 7.1, 7.2 (csak 64 bites verzió esetén)</span><span class="sxs-lookup"><span data-stu-id="094c9-132">Red Hat Enterprise Linux (RHEL) 7.1, 7.2 (64-bit only)</span></span> </br> <span data-ttu-id="094c9-133">CentOS 7.0, 7.1, 7.2 (csak 64 bites verzió esetén)</span><span class="sxs-lookup"><span data-stu-id="094c9-133">CentOS 7.0, 7.1, 7.2 (64-bit only)</span></span></br> <span data-ttu-id="094c9-134">7.3 (csak az áttelepítés esetén) – centOs</span><span class="sxs-lookup"><span data-stu-id="094c9-134">CentOs 7.3 (migration only)</span></span> |
|<span data-ttu-id="094c9-135">Microsoft-ASR\_EE\*SLES11-SP3-64\*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="094c9-135">Microsoft-ASR\_UA\*SLES11-SP3-64\*release.tar.gz</span></span>| <span data-ttu-id="094c9-136">SUSE Linux Enterprise Server 11 SP3 (csak 64 bites verzió esetén)</span><span class="sxs-lookup"><span data-stu-id="094c9-136">SUSE Linux Enterprise Server 11 SP3 (64-bit only)</span></span>|
|<span data-ttu-id="094c9-137">Microsoft-ASR\_EE\*SLES11-SP4-64\*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="094c9-137">Microsoft-ASR\_UA\*SLES11-SP4-64\*release.tar.gz</span></span>| <span data-ttu-id="094c9-138">SUSE Linux Enterprise Server 11 SP4 (csak 64 bites verzió esetén)</span><span class="sxs-lookup"><span data-stu-id="094c9-138">SUSE Linux Enterprise Server 11 SP4 (64-bit only)</span></span>|
|<span data-ttu-id="094c9-139">Microsoft-ASR\_EE\*OL6-64\*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="094c9-139">Microsoft-ASR\_UA\*OL6-64\*release.tar.gz</span></span> | <span data-ttu-id="094c9-140">Oracle Enterprise Linux 6.4 6.5 (csak 64 bites verzió esetén)</span><span class="sxs-lookup"><span data-stu-id="094c9-140">Oracle Enterprise Linux 6.4, 6.5 (64-bit only)</span></span>|
|<span data-ttu-id="094c9-141">Microsoft-ASR\_EE\*UBUNTU 14.04-64\*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="094c9-141">Microsoft-ASR\_UA\*UBUNTU-14.04-64\*release.tar.gz</span></span> | <span data-ttu-id="094c9-142">Ubuntu Linux 14.04 (csak 64 bites verzió esetén)</span><span class="sxs-lookup"><span data-stu-id="094c9-142">Ubuntu Linux 14.04 (64-bit only)</span></span>|


## <a name="install-mobility-service-manually-by-using-the-gui"></a><span data-ttu-id="094c9-143">Telepítse manuálisan a mobilitási szolgáltatást a grafikus felhasználói felület használatával</span><span class="sxs-lookup"><span data-stu-id="094c9-143">Install Mobility Service manually by using the GUI</span></span>

>[!IMPORTANT]
> <span data-ttu-id="094c9-144">Használatakor a **konfigurációs kiszolgáló** replikálásához **Azure IaaS virtuális gépek** a egy Azure előfizetés vagy régió másik majd **használhatja a parancssori-alapú telepítési** módszer</span><span class="sxs-lookup"><span data-stu-id="094c9-144">If you are using a **Configuration Server** to replicate **Azure IaaS virtual machines** from one Azure Subscription/Region to another then **use the Command-line based installation** method</span></span>

[!INCLUDE [site-recovery-install-mob-svc-gui](../../includes/site-recovery-install-mob-svc-gui.md)]

## <a name="install-mobility-service-manually-at-a-command-prompt"></a><span data-ttu-id="094c9-145">Telepítse manuálisan a mobilitási szolgáltatást a parancssorba</span><span class="sxs-lookup"><span data-stu-id="094c9-145">Install Mobility Service manually at a command prompt</span></span>

### <a name="command-line-installation-on-a-windows-computer"></a><span data-ttu-id="094c9-146">Parancssori telepítés Windows-számítógépen</span><span class="sxs-lookup"><span data-stu-id="094c9-146">Command-line installation on a Windows computer</span></span>
[!INCLUDE [site-recovery-install-mob-svc-win-cmd](../../includes/site-recovery-install-mob-svc-win-cmd.md)]

### <a name="command-line-installation-on-a-linux-computer"></a><span data-ttu-id="094c9-147">Parancssori telepítés, a Linux számítógépre</span><span class="sxs-lookup"><span data-stu-id="094c9-147">Command-line installation on a Linux computer</span></span>
[!INCLUDE [site-recovery-install-mob-svc-lin-cmd](../../includes/site-recovery-install-mob-svc-lin-cmd.md)]


## <a name="install-mobility-service-by-push-installation-from-azure-site-recovery"></a><span data-ttu-id="094c9-148">Telepítse a mobilitási szolgáltatás leküldéses telepítése az Azure Site Recovery által</span><span class="sxs-lookup"><span data-stu-id="094c9-148">Install Mobility Service by push installation from Azure Site Recovery</span></span>
<span data-ttu-id="094c9-149">A mobilitási szolgáltatás leküldéses telepítéséhez tegye a Site Recovery segítségével, minden célszámítógépen a következő előfeltételeknek kell megfelelnie.</span><span class="sxs-lookup"><span data-stu-id="094c9-149">To do a push installation of Mobility Service by using Site Recovery, all target computers must meet the following prerequisites.</span></span>

[!INCLUDE [site-recovery-prepare-push-install-mob-svc-win](../../includes/site-recovery-prepare-push-install-mob-svc-win.md)]

[!INCLUDE [site-recovery-prepare-push-install-mob-svc-lin](../../includes/site-recovery-prepare-push-install-mob-svc-lin.md)]


> [!NOTE]
<span data-ttu-id="094c9-150">Az Azure-portálon a mobilitási szolgáltatás telepítése után válassza ki a **replikálása** gombra kattintva indítsa el a virtuális gépek védelmét.</span><span class="sxs-lookup"><span data-stu-id="094c9-150">After Mobility Service is installed, in the Azure portal, select the **Replicate** button to start protecting these VMs.</span></span>

## <a name="uninstall-mobility-service-on-a-windows-server-computer"></a><span data-ttu-id="094c9-151">Távolítsa el a mobilitási szolgáltatást a Windows Server rendszerben</span><span class="sxs-lookup"><span data-stu-id="094c9-151">Uninstall Mobility Service on a Windows Server computer</span></span>
<span data-ttu-id="094c9-152">Az alábbi módszerek valamelyikével távolítsa el a mobilitási szolgáltatás egy Windows Server rendszeren.</span><span class="sxs-lookup"><span data-stu-id="094c9-152">Use one of the following methods to uninstall Mobility Service on a Windows Server computer.</span></span>

### <a name="uninstall-by-using-the-gui"></a><span data-ttu-id="094c9-153">Távolítsa el a grafikus felhasználói felület használatával</span><span class="sxs-lookup"><span data-stu-id="094c9-153">Uninstall by using the GUI</span></span>
1. <span data-ttu-id="094c9-154">A Vezérlőpulton válassza **programok**.</span><span class="sxs-lookup"><span data-stu-id="094c9-154">In Control Panel, select **Programs**.</span></span>
2. <span data-ttu-id="094c9-155">Válassza ki **Microsoft Azure Site helyreállítási mobilitási szolgáltatás/fő célkiszolgáló**, majd válassza ki **Eltávolítás**.</span><span class="sxs-lookup"><span data-stu-id="094c9-155">Select **Microsoft Azure Site Recovery Mobility Service/Master Target server**, and then select **Uninstall**.</span></span>

### <a name="uninstall-at-a-command-prompt"></a><span data-ttu-id="094c9-156">Távolítsa el a parancssorba</span><span class="sxs-lookup"><span data-stu-id="094c9-156">Uninstall at a command prompt</span></span>
1. <span data-ttu-id="094c9-157">Nyisson meg egy parancssori ablakot rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="094c9-157">Open a Command Prompt window as an administrator.</span></span>
2. <span data-ttu-id="094c9-158">Távolítsa el a mobilitási szolgáltatást, futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="094c9-158">To uninstall Mobility Service, run the following command:</span></span>

```
MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
```

## <a name="uninstall-mobility-service-on-a-linux-computer"></a><span data-ttu-id="094c9-159">Távolítsa el a mobilitási szolgáltatás egy Linux-számítógép</span><span class="sxs-lookup"><span data-stu-id="094c9-159">Uninstall Mobility Service on a Linux computer</span></span>
1. <span data-ttu-id="094c9-160">A Linux-kiszolgálón jelentkezzen be egy **legfelső szintű** felhasználó.</span><span class="sxs-lookup"><span data-stu-id="094c9-160">On your Linux server, sign in as a **root** user.</span></span>
2. <span data-ttu-id="094c9-161">A terminálban keresse /user/local/ASR.</span><span class="sxs-lookup"><span data-stu-id="094c9-161">In a terminal, go to /user/local/ASR.</span></span>
3. <span data-ttu-id="094c9-162">Távolítsa el a mobilitási szolgáltatást, futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="094c9-162">To uninstall Mobility Service, run the following command:</span></span>

```
uninstall.sh -Y
```
