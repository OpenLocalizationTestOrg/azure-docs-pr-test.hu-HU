---
title: "aaaReplicate Hyper-V virtuális gépek tooAzure hello klasszikus portál a PowerShell-lel |} Microsoft Docs"
description: "Automatizálhatja a hello replikálást a Hyper-V virtuális gépek VMM-felhőkben hello klasszikus portál helyreállítás és a PowerShell használatával"
services: site-recovery
documentationcenter: 
author: bsiva
manager: abhiag
editor: tysonn
ms.assetid: 9011f567-e0b4-4306-951a-b30da19f5db6
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: bsiva
ms.openlocfilehash: d6847b46ac227209e6890de4ab603b23f827360f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-vms-tooazure-with-powershell-in-hello-classic-portal"></a><span data-ttu-id="c0a07-103">A klasszikus portálon hello a PowerShell segítségével a Hyper-V virtuális gépek tooAzure replikálása</span><span class="sxs-lookup"><span data-stu-id="c0a07-103">Replicate Hyper-V VMs tooAzure with PowerShell in hello classic portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c0a07-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c0a07-104">Azure Portal</span></span>](site-recovery-vmm-to-azure.md)
> * [<span data-ttu-id="c0a07-105">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="c0a07-105">PowerShell - Resource Manager</span></span>](site-recovery-vmm-to-azure-powershell-resource-manager.md)
> * [<span data-ttu-id="c0a07-106">Klasszikus portál</span><span class="sxs-lookup"><span data-stu-id="c0a07-106">Classic Portal</span></span>](site-recovery-vmm-to-azure-classic.md)
> * [<span data-ttu-id="c0a07-107">PowerShell – Klasszikus</span><span class="sxs-lookup"><span data-stu-id="c0a07-107">PowerShell - Classic</span></span>](site-recovery-deploy-with-powershell.md)
>
>

## <a name="overview"></a><span data-ttu-id="c0a07-108">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="c0a07-108">Overview</span></span>
<span data-ttu-id="c0a07-109">Az Azure Site Recovery hozzájárul tooyour üzleti folytonossági és vészhelyreállítási (BCDR) helyreállítási stratégia megvalósításában replikációjának, feladatátvételének és helyreállítási virtuális gép a különböző telepítési forgatókönyvek esetén.</span><span class="sxs-lookup"><span data-stu-id="c0a07-109">Azure Site Recovery contributes tooyour business continuity and disaster recovery (BCDR) strategy by orchestrating replication, failover and recovery of virtual machines in a number of deployment scenarios.</span></span> <span data-ttu-id="c0a07-110">Egy teljes listája központi telepítési forgatókönyvek: hello [Azure Site Recovery áttekintését](site-recovery-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c0a07-110">For a full list of deployment scenarios see hello [Azure Site Recovery overview](site-recovery-overview.md).</span></span>

<span data-ttu-id="c0a07-111">Ez a cikk bemutatja, hogyan toouse PowerShell tooautomate gyakori feladatokat kell tooperform Azure Site Recovery tooreplicate Hyper-V virtuális gépek a System Center VMM-felhők tooAzure tárolási beállításakor.</span><span class="sxs-lookup"><span data-stu-id="c0a07-111">This article shows you how toouse PowerShell tooautomate common tasks you need tooperform when you set up Azure Site Recovery tooreplicate Hyper-V virtual machines in System Center VMM clouds tooAzure storage.</span></span>

<span data-ttu-id="c0a07-112">hello cikkből hello forgatókönyvet, és bemutatja, hogyan telepítheti a tooset Site Recovery-tároló, be Azure Site Recovery Provider hello hello forrás VMM-kiszolgálón előfeltételeit, regisztrálja hello kiszolgálót hello tárolóban, Azure-tárfiók hozzáadása, hello Azure telepítéséhez Recovery Services Agent ügynököt a Hyper-V gazdakiszolgálók, adja meg, amely alkalmazott tooall védett virtuális gépek fog, majd engedélyezze a védelmet azon virtuális gépek VMM-felhő védelmi beállításait.</span><span class="sxs-lookup"><span data-stu-id="c0a07-112">hello article includes prerequisites for hello scenario, and shows you how tooset up a Site Recovery vault, install hello Azure Site Recovery Provider on hello source VMM server, register hello server in hello vault, add an Azure storage account, install hello Azure Recovery Services agent on Hyper-V host servers, configure protection settings for VMM clouds that will be applied tooall protected virtual machines, and then enable protection for those virtual machines.</span></span> <span data-ttu-id="c0a07-113">Végül tesztelési hello feladatátvételi toomake meg arról, hogy minden a várt módon működik.</span><span class="sxs-lookup"><span data-stu-id="c0a07-113">Finish up by testing hello failover toomake sure everything's working as expected.</span></span>

<span data-ttu-id="c0a07-114">Ha ez a forgatókönyv beállítása problémát tapasztal, kérdéseit a hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="c0a07-114">If you run into problems setting up this scenario, post your questions on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

> [!NOTE]
> <span data-ttu-id="c0a07-115">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="c0a07-115">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="c0a07-116">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="c0a07-116">This article covers using hello Classic deployment model.</span></span>
>
>

## <a name="before-you-start"></a><span data-ttu-id="c0a07-117">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="c0a07-117">Before you start</span></span>
<span data-ttu-id="c0a07-118">Győződjön meg arról, hogy az előfeltételek teljesülnek:</span><span class="sxs-lookup"><span data-stu-id="c0a07-118">Make sure you have these prerequisites in place:</span></span>

### <a name="azure-prerequisites"></a><span data-ttu-id="c0a07-119">Azure-előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c0a07-119">Azure prerequisites</span></span>
* <span data-ttu-id="c0a07-120">Szüksége lesz egy [Microsoft Azure](https://azure.microsoft.com/)-fiókra.</span><span class="sxs-lookup"><span data-stu-id="c0a07-120">You'll need a [Microsoft Azure](https://azure.microsoft.com/) account.</span></span> <span data-ttu-id="c0a07-121">Kezdésként használhatja az [ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/) is.</span><span class="sxs-lookup"><span data-stu-id="c0a07-121">You can start with a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="c0a07-122">Szüksége lesz egy Azure storage fiók toostore replikált adatokat.</span><span class="sxs-lookup"><span data-stu-id="c0a07-122">You'll need an Azure storage account toostore replicated data.</span></span> <span data-ttu-id="c0a07-123">hello fiókot kell georeplikáció engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="c0a07-123">hello account needs geo-replication enabled.</span></span> <span data-ttu-id="c0a07-124">A hello és hello Azure Site Recovery-tárolónak ugyanabban a régióban, és társítani kell hello ugyanahhoz az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="c0a07-124">It should be in hello same region as hello Azure Site Recovery vault and be associated with hello same subscription.</span></span> <span data-ttu-id="c0a07-125">[További tudnivalók az Azure storage](../storage/common/storage-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="c0a07-125">[Learn more about Azure storage](../storage/common/storage-introduction.md).</span></span>
* <span data-ttu-id="c0a07-126">Szüksége lesz a toomake meg arról, hogy tooprotect kívánt virtuális gépek ahhoz [Azure virtuális gép Előfeltételek](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).</span><span class="sxs-lookup"><span data-stu-id="c0a07-126">You'll need toomake sure that virtual machines you want tooprotect comply with [Azure virtual machine prerequisites](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).</span></span>

### <a name="vmm-prerequisites"></a><span data-ttu-id="c0a07-127">A VMM-Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c0a07-127">VMM prerequisites</span></span>
* <span data-ttu-id="c0a07-128">A System Center 2012 R2 rendszeren futó VMM-kiszolgáló lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="c0a07-128">You'll need  VMM server running on System Center 2012 R2.</span></span>
* <span data-ttu-id="c0a07-129">A VMM-kiszolgálónak tooprotect hello legalább egy felhő lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="c0a07-129">You'll need at least one cloud on hello VMM server you want tooprotect.</span></span> <span data-ttu-id="c0a07-130">hello felhők tartalmazzák:</span><span class="sxs-lookup"><span data-stu-id="c0a07-130">hello cloud should contain:</span></span>
  * <span data-ttu-id="c0a07-131">Legalább egy VMM-gazdagépcsoport.</span><span class="sxs-lookup"><span data-stu-id="c0a07-131">One or more VMM host groups.</span></span>
  * <span data-ttu-id="c0a07-132">Egy vagy több Hyper-V gazdakiszolgálók vagy fürtök minden gazdagépcsoportban.</span><span class="sxs-lookup"><span data-stu-id="c0a07-132">One or more Hyper-V host servers or clusters in each host group .</span></span>
  * <span data-ttu-id="c0a07-133">Egy vagy több virtuális gépek hello forrás Hyper-V kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="c0a07-133">One or more virtual machines on hello source Hyper-V server.</span></span>

### <a name="hyper-v-prerequisites"></a><span data-ttu-id="c0a07-134">Hyper-V előfeltételei</span><span class="sxs-lookup"><span data-stu-id="c0a07-134">Hyper-V prerequisites</span></span>
* <span data-ttu-id="c0a07-135">hello Hyper-V gazdakiszolgálók futnia kell az legalább **Windows Server 2012** Hyper-V szerepkörrel vagy **Microsoft Hyper-V Server 2012** és a legújabb frissítések telepítve rendelkezik hello.</span><span class="sxs-lookup"><span data-stu-id="c0a07-135">hello host Hyper-V servers must be running at least **Windows Server 2012** with Hyper-V role or **Microsoft Hyper-V Server 2012** and have hello latest updates installed.</span></span>
* <span data-ttu-id="c0a07-136">Ha fürtben futtatja a Hyper-V-t, ne feledje, hogy ha statikus IP-címen alapuló fürtöt használ, a rendszer nem hozza létre automatikusan a fürtszervezőt.</span><span class="sxs-lookup"><span data-stu-id="c0a07-136">If you're running Hyper-V in a cluster note that cluster broker isn't created automatically if you have a static IP address-based cluster.</span></span> <span data-ttu-id="c0a07-137">Manuálisan kell tooconfigure hello fürtszervező.</span><span class="sxs-lookup"><span data-stu-id="c0a07-137">You'll need tooconfigure hello cluster broker manually.</span></span> <span data-ttu-id="c0a07-138">toodo ezt, a Kiszolgálókezelő > Feladatátvevőfürt-kezelőt, csatlakoztassa toohello fürtöt, kattintson a **szerepkör konfigurálása** válassza ki **Hyper-V Replikaszervező** a hello **Szerepkörválasztás**hello magas rendelkezésre állás varázsló képernyőjén.</span><span class="sxs-lookup"><span data-stu-id="c0a07-138">toodo this, in Server Manager > Failover Cluster Manager, connect toohello cluster, click **Configure Role** and select **Hyper-V Replica Broker** in hello **Select Role** screen of hello High Availability wizard.</span></span>
* <span data-ttu-id="c0a07-139">Bármely Hyper-V gazdakiszolgáló vagy fürt legyen toomanage védelmi szerepelnie kell egy VMM-felhőben.</span><span class="sxs-lookup"><span data-stu-id="c0a07-139">Any Hyper-V host server or cluster for which you want toomanage protection must be included in a VMM cloud.</span></span>

### <a name="network-mapping-prerequisites"></a><span data-ttu-id="c0a07-140">Hálózatleképezési előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c0a07-140">Network mapping prerequisites</span></span>
<span data-ttu-id="c0a07-141">Ha virtuális gépet az Azure-hálózat hozzárendelés véd felel meg a Virtuálisgép-hálózatok hello forrás VMM-kiszolgálón és a cél Azure-hálózatok tooenable hello következő között:</span><span class="sxs-lookup"><span data-stu-id="c0a07-141">When you protect virtual machines in Azure network mapping maps between VM networks on hello source VMM server and target Azure networks tooenable hello following:</span></span>

* <span data-ttu-id="c0a07-142">Feladatok átadása a hello azonos gépeire hálózati kapcsolódhatnak tooeach más, függetlenül a helyreállítási terv.</span><span class="sxs-lookup"><span data-stu-id="c0a07-142">All machines which fail over on hello same network can connect tooeach other, irrespective of which recovery plan they are in.</span></span>
* <span data-ttu-id="c0a07-143">Ha egy hálózati átjáró működik hello cél Azure-hálózatot, virtuális gépek tooother a helyszíni virtuális gépek is elérheti.</span><span class="sxs-lookup"><span data-stu-id="c0a07-143">If a network gateway is setup on hello target Azure network, virtual machines can connect tooother on-premises virtual machines.</span></span>
* <span data-ttu-id="c0a07-144">Ha nem adja meg a hálózati leképezése csak virtuális gépek feladatátvételt hello azonos helyreállítási terv feladatátvételi tooAzure után lesznek képesek tooconnect tooeach más.</span><span class="sxs-lookup"><span data-stu-id="c0a07-144">If you don’t configure network mapping only virtual machines that fail over in hello same recovery plan will be able tooconnect tooeach other after failover tooAzure.</span></span>

<span data-ttu-id="c0a07-145">Ha azt szeretné, hogy toodeploy hálózatra való leképezés, hello következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="c0a07-145">If you want toodeploy network mapping you'll need hello following:</span></span>

* <span data-ttu-id="c0a07-146">azt szeretné, hogy a forrás VMM-kiszolgálón hello tooprotect hello virtuális gépek csatlakoztatott tooa Virtuálisgép-hálózathoz kell lennie.</span><span class="sxs-lookup"><span data-stu-id="c0a07-146">hello virtual machines you want tooprotect on hello source VMM server should be connected tooa VM network.</span></span> <span data-ttu-id="c0a07-147">Erre a hálózatra kell csatolt tooa hello felhő társított logikai hálózat.</span><span class="sxs-lookup"><span data-stu-id="c0a07-147">That network should be linked tooa logical network that is associated with hello cloud.</span></span>
* <span data-ttu-id="c0a07-148">Egy Azure-hálózat toowhich replikált virtuális gépek a feladatátvételt követően csatlakozni tudnak.</span><span class="sxs-lookup"><span data-stu-id="c0a07-148">An Azure network toowhich replicated virtual machines can connect after failover.</span></span> <span data-ttu-id="c0a07-149">Ezt a hálózatot a feladatátvételi hello időpontjában válasszon ki.</span><span class="sxs-lookup"><span data-stu-id="c0a07-149">You'll select this network at hello time of failover.</span></span> <span data-ttu-id="c0a07-150">hello hello hálózat legyen ugyanabban a régióban, az Azure Site Recovery-előfizetést.</span><span class="sxs-lookup"><span data-stu-id="c0a07-150">hello network should be in hello same region as your Azure Site Recovery subscription.</span></span>

### <a name="powershell-prerequisites"></a><span data-ttu-id="c0a07-151">PowerShell-Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c0a07-151">PowerShell prerequisites</span></span>
<span data-ttu-id="c0a07-152">Győződjön meg arról, hogy készen áll az Azure PowerShell toogo.</span><span class="sxs-lookup"><span data-stu-id="c0a07-152">Make sure you have Azure PowerShell ready toogo.</span></span> <span data-ttu-id="c0a07-153">Ha már használ PowerShell, szüksége lesz a tooupgrade tooversion 0.8.10 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="c0a07-153">If you are already using PowerShell, you'll need tooupgrade tooversion 0.8.10 or later.</span></span> <span data-ttu-id="c0a07-154">PowerShell beállításával kapcsolatos információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azureps-cmdlets-docs).</span><span class="sxs-lookup"><span data-stu-id="c0a07-154">For information about setting up PowerShell, see [How tooinstall and configure Azure PowerShell](/powershell/azureps-cmdlets-docs).</span></span> <span data-ttu-id="c0a07-155">Miután beállítása és PowerShell konfigurálva, megtekintheti az összes rendelkezésre álló parancsmagok hello hello szolgáltatás [Itt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="c0a07-155">Once you have set up and configured PowerShell, you can view all of hello available cmdlets for hello service [here](/powershell/azure/overview).</span></span>

<span data-ttu-id="c0a07-156">toolearn tippeket, amelyik segíthet a hello parancsmag például paraméterértékeket, bemenetekhez és kimenetekhez általában kezelésének módja az Azure PowerShell használatával kapcsolatban lásd: [Ismerkedés az Azure-parancsmagok](/powershell/azure/get-started-azureps).</span><span class="sxs-lookup"><span data-stu-id="c0a07-156">toolearn about tips that can help you use hello cmdlets, such as how parameter values, inputs, and outputs are typically handled in Azure PowerShell, see [Get Started with Azure Cmdlets](/powershell/azure/get-started-azureps).</span></span>

## <a name="step-1-set-hello-subscription"></a><span data-ttu-id="c0a07-157">1. lépés: Hello előfizetés beállítása</span><span class="sxs-lookup"><span data-stu-id="c0a07-157">Step 1: Set hello subscription</span></span>
<span data-ttu-id="c0a07-158">A PowerShellben futtassa a parancsmagokat:</span><span class="sxs-lookup"><span data-stu-id="c0a07-158">In PowerShell, run these cmdlets:</span></span>

```
$UserName = "<user@live.com>"
$Password = "<password>"
$AzureSubscriptionName = "prod_sub1"

$SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
$Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $securePassword
Add-AzureAccount -Credential $Cred;
$AzureSubscription = Select-AzureSubscription -SubscriptionName $AzureSubscriptionName

```

<span data-ttu-id="c0a07-159">Hello elemek hello "< >" belül cserélje le a kért adatokat.</span><span class="sxs-lookup"><span data-stu-id="c0a07-159">Replace hello elements within hello "< >" with your specific information.</span></span>

## <a name="step-2-create-a-site-recovery-vault"></a><span data-ttu-id="c0a07-160">2. lépés: A Site Recovery-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="c0a07-160">Step 2: Create a Site Recovery vault</span></span>
<span data-ttu-id="c0a07-161">A PowerShellben hello elemek hello "< >" belül cserélje le a kért adatokat, és futtassa az alábbi parancsokat:</span><span class="sxs-lookup"><span data-stu-id="c0a07-161">In PowerShell, replace hello elements within hello "< >" with your specific information, and run these commands:</span></span>

```

$VaultName = "<testvault123>"
$VaultGeo  = "<Southeast Asia>"
$OutputPathForSettingsFile = "<c:\>"

```


```
New-AzureSiteRecoveryVault -Location $VaultGeo -Name $VaultName;
$vault = Get-AzureSiteRecoveryVault -Name $VaultName;

```

## <a name="step-3-generate-a-vault-registration-key"></a><span data-ttu-id="c0a07-162">3. lépés: A tároló regisztrációs kulcsának létrehozása</span><span class="sxs-lookup"><span data-stu-id="c0a07-162">Step 3: Generate a vault registration key</span></span>
<span data-ttu-id="c0a07-163">Hozzon létre egy regisztrációs kulcsot hello tárolóban lévő állapottal.</span><span class="sxs-lookup"><span data-stu-id="c0a07-163">Generate a registration key in hello vault.</span></span> <span data-ttu-id="c0a07-164">Miután hello Azure Site Recovery Provider letöltése, és telepítse a VMM-kiszolgálón hello, a kulcs tooregister hello VMM-kiszolgáló hello tárolóban fogja használni.</span><span class="sxs-lookup"><span data-stu-id="c0a07-164">After you download hello Azure Site Recovery Provider and install it on hello VMM server, you'll use this key tooregister hello VMM server in hello vault.</span></span>

1. <span data-ttu-id="c0a07-165">Hello tároló beállítási fájl beolvasása és hello környezet beállítása:</span><span class="sxs-lookup"><span data-stu-id="c0a07-165">Get hello vault setting file and set hello context:</span></span>

   ```

   $VaultName = "<testvault123>"
   $VaultGeo  = "<Southeast Asia>"
   $OutputPathForSettingsFile = "<c:\>"

   $VaultSetingsFile = Get-AzureSiteRecoveryVaultSettingsFile -Location $VaultGeo -Name $VaultName -Path $OutputPathForSettingsFile;

   ```
2. <span data-ttu-id="c0a07-166">Hello tároló környezet beállítása hello a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="c0a07-166">Set hello vault context by running hello following commands:</span></span>

   ```

   $VaultSettingFilePath = $vaultSetingsFile.FilePath
   $VaultContext = Import-AzureSiteRecoveryVaultSettingsFile -Path $VaultSettingFilePath -ErrorAction Stop

   ```

## <a name="step-4-install-hello-azure-site-recovery-provider"></a><span data-ttu-id="c0a07-167">4. lépés: Hello Azure Site Recovery Provider telepítése</span><span class="sxs-lookup"><span data-stu-id="c0a07-167">Step 4: Install hello Azure Site Recovery Provider</span></span>
1. <span data-ttu-id="c0a07-168">Hello VMM-gépen hozzon létre egy könyvtárat hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="c0a07-168">On hello VMM machine, create a directory by running hello following command:</span></span>

   ```

   pushd C:\ASR\

   ```
2. <span data-ttu-id="c0a07-169">Bontsa ki a letöltött hello szolgáltató használatával hello a következő parancs futtatásával hello fájlt</span><span class="sxs-lookup"><span data-stu-id="c0a07-169">Extract hello files using hello downloaded provider by running hello following command</span></span>

   ```

   AzureSiteRecoveryProvider.exe /x:. /q

   ```
3. <span data-ttu-id="c0a07-170">Hello szolgáltató a következő parancsok hello segítségével telepíti:</span><span class="sxs-lookup"><span data-stu-id="c0a07-170">Install hello provider using hello following commands:</span></span>

   ```

   .\SetupDr.exe /i

   ```

   ```

   $installationRegPath = "hklm:\software\Microsoft\Microsoft System Center Virtual Machine Manager Server\DRAdapter"
   do
   {
     $isNotInstalled = $true;
     if(Test-Path $installationRegPath)
     {
         $isNotInstalled = $false;
     }
   }While($isNotInstalled)

   ```

   <span data-ttu-id="c0a07-171">Várjon, amíg hello telepítési toofinish.</span><span class="sxs-lookup"><span data-stu-id="c0a07-171">Wait for hello installation toofinish.</span></span>
4. <span data-ttu-id="c0a07-172">Hello kiszolgáló regisztrálása hello tárolónál hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="c0a07-172">Register hello server in hello vault using hello following command:</span></span>

   ```

   $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
   pushd $BinPath
   $encryptionFilePath = "C:\temp\"
   .\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice

   ```

## <a name="step-5-create-an-azure-storage-account"></a><span data-ttu-id="c0a07-173">5. lépés: Az Azure storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="c0a07-173">Step 5: Create an Azure storage account</span></span>
<span data-ttu-id="c0a07-174">Ha nem rendelkezik Azure-tárfiók, hozzon létre egy engedélyezett a georeplikáció fiókot hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="c0a07-174">If you don't have an Azure storage account, create a geo-replication enabled account by running hello following command:</span></span>

```

$StorageAccountName = "teststorageacc1"
$StorageAccountGeo  = "Southeast Asia"

New-AzureStorageAccount -StorageAccountName $StorageAccountName -Label $StorageAccountName -Location $StorageAccountGeo;

```

<span data-ttu-id="c0a07-175">Vegye figyelembe, hogy hello tárfiókot kell lennie a hello és hello Azure Site Recovery szolgáltatásnak ugyanabban a régióban, és társítva hello ugyanahhoz az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="c0a07-175">Note that hello storage account must be in hello same region as hello Azure Site Recovery service, and be associated with hello same subscription.</span></span>

## <a name="step-6-install-hello-azure-recovery-services-agent"></a><span data-ttu-id="c0a07-176">6. lépés: Hello Azure Recovery Services Agent telepítése</span><span class="sxs-lookup"><span data-stu-id="c0a07-176">Step 6: Install hello Azure Recovery Services Agent</span></span>
<span data-ttu-id="c0a07-177">Hello Azure-portálon, minden Hyper-V gazdagép-kiszolgálón található VMM-felhőkben hello telepítés hello Azure Recovery Services Agent ügynököt, tooprotect szeretné.</span><span class="sxs-lookup"><span data-stu-id="c0a07-177">From hello Azure portal, install hello Azure Recovery Services agent on each Hyper-V host server located in hello VMM clouds you want tooprotect.</span></span>

<span data-ttu-id="c0a07-178">Futtassa az alábbi a parancs minden VMM-gazdagép hello:</span><span class="sxs-lookup"><span data-stu-id="c0a07-178">Run hello following command on all VMM hosts:</span></span>

```

marsagentinstaller.exe /q /nu

```


## <a name="step-7-configure-cloud-protection-settings"></a><span data-ttu-id="c0a07-179">7. lépés: Konfigurálja a felhő védelmi beállításait</span><span class="sxs-lookup"><span data-stu-id="c0a07-179">Step 7: Configure cloud protection settings</span></span>
1. <span data-ttu-id="c0a07-180">Hozza létre a felhő védelmi profil tooAzure hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="c0a07-180">Create a cloud protection profile tooAzure by running hello following command:</span></span>

   ```

   $ReplicationFrequencyInSeconds = "300";
   $ProfileResult = New-AzureSiteRecoveryProtectionProfileObject -ReplicationProvider HyperVReplica -RecoveryAzureSubscription $AzureSubscriptionName -RecoveryAzureStorageAccount $StorageAccountName -ReplicationFrequencyInSeconds     $ReplicationFrequencyInSeconds;

   ```
2. <span data-ttu-id="c0a07-181">Töltse le a védelmi tároló hello a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="c0a07-181">Get a protection container by running hello following commands:</span></span>

   ```

   $PrimaryCloud = "testcloud"
   $protectionContainer = Get-AzureSiteRecoveryProtectionContainer -Name $PrimaryCloud;

   ```
3. <span data-ttu-id="c0a07-182">Indítsa el a hello társítás hello védelmi tároló hello felhő:</span><span class="sxs-lookup"><span data-stu-id="c0a07-182">Start hello association of hello protection container with hello cloud:</span></span>

   ```

   $associationJob = Start-AzureSiteRecoveryProtectionProfileAssociationJob -ProtectionProfile $profileResult -PrimaryProtectionContainer $protectionContainer;        

   ```
4. <span data-ttu-id="c0a07-183">Hello feladat befejeződését követően futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="c0a07-183">After hello job has finished, run hello following command:</span></span>

        $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
        if($job -eq $null -or $job.StateDescription -ne "Completed")
        {
        $isJobLeftForProcessing = $true;
        }


1. <span data-ttu-id="c0a07-184">Hello feladat feldolgozása után futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="c0a07-184">After hello job has finished processing, run hello following command:</span></span>

        Do
        {
        $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
        Write-Host "Job State:{0}, StateDescription:{1}" -f Job.State, $job.StateDescription;
        if($job -eq $null -or $job.StateDescription -ne "Completed")
        {
            $isJobLeftForProcessing = $true;
        }

        if($isJobLeftForProcessing)
        {
            Start-Sleep -Seconds 60
        }
        }While($isJobLeftForProcessing)



<span data-ttu-id="c0a07-185">hello művelet, toocheck hello befejezése kövesse hello [figyelési tevékenység](#monitor).</span><span class="sxs-lookup"><span data-stu-id="c0a07-185">toocheck hello completion of hello operation, follow hello steps in [Monitor Activity](#monitor).</span></span>

## <a name="step-8-configure-network-mapping"></a><span data-ttu-id="c0a07-186">8. lépés: Hálózatleképezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c0a07-186">Step 8: Configure network mapping</span></span>
<span data-ttu-id="c0a07-187">Mielőtt elkezdené hálózatleképezés ellenőrizze, hogy hello forrás VMM-kiszolgálón található virtuális gépek csatlakoztatott tooa Virtuálisgép-hálózatot.</span><span class="sxs-lookup"><span data-stu-id="c0a07-187">Before you begin network mapping verify that virtual machines on hello source VMM server are connected tooa VM network.</span></span> <span data-ttu-id="c0a07-188">Továbbá hozzon létre legalább egy Azure virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="c0a07-188">In addition, create one or more Azure virtual networks.</span></span> <span data-ttu-id="c0a07-189">Vegye figyelembe, hogy több Virtuálisgép-hálózatot is csatlakoztatott tooa egyetlen Azure-hálózatra.</span><span class="sxs-lookup"><span data-stu-id="c0a07-189">Note that multiple VM networks can be mapped tooa single Azure network.</span></span>

<span data-ttu-id="c0a07-190">Vegye figyelembe, hogy ha hello célhálózatban már több alhálózat működik, és ezek egyikének rendelkezik hello neve megegyezik a virtual machine hello forrás alhálózati található, akkor hello replika virtuális gép lesz a cél alhálózathoz csatlakoztatott toothat feladatátvételt követően.</span><span class="sxs-lookup"><span data-stu-id="c0a07-190">Note that if hello target network has multiple subnets and one of those subnets has hello same name as subnet on which hello source virtual machine is located, then hello replica virtual machine will be connected toothat target subnet after failover.</span></span> <span data-ttu-id="c0a07-191">Ha nincs egyező nevű cél alhálózat, hello virtuális gép lesz a csatlakoztatott toohello hello hálózat első alhálózatát.</span><span class="sxs-lookup"><span data-stu-id="c0a07-191">If there is not a target subnet with a matching name, hello virtual machine will be connected toohello first subnet in hello network.</span></span>

<span data-ttu-id="c0a07-192">hello első parancs beolvasása kiszolgálók hello aktuális Azure Site Recovery-tárolóban.</span><span class="sxs-lookup"><span data-stu-id="c0a07-192">hello first command gets servers for hello current Azure Site Recovery vault.</span></span> <span data-ttu-id="c0a07-193">hello parancs hello Microsoft Azure Site Recovery kiszolgálók hello $Servers tömbváltozó tárolja.</span><span class="sxs-lookup"><span data-stu-id="c0a07-193">hello command stores hello Microsoft Azure Site Recovery servers in hello $Servers array variable.</span></span>

    $Servers = Get-AzureSiteRecoveryServer


<span data-ttu-id="c0a07-194">hello második parancs hello hely helyreállítási hálózati hello első kiszolgáló kéri le a hello $Servers tömbben.</span><span class="sxs-lookup"><span data-stu-id="c0a07-194">hello second command gets hello site recovery network for hello first server in hello $Servers array.</span></span> <span data-ttu-id="c0a07-195">hello parancs hello hálózatok hello $Networks változó tárolja.</span><span class="sxs-lookup"><span data-stu-id="c0a07-195">hello command stores hello networks in hello $Networks variable.</span></span>

    $Networks = Get-AzureSiteRecoveryNetwork -Server $Servers[0]

<span data-ttu-id="c0a07-196">hello harmadik parancs az Azure-előfizetések lekérdezi a Get-AzureSubscription hello parancsmag használatával, és majd hello $Subscriptions változó tárolja ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="c0a07-196">hello third command gets your Azure subscriptions by using hello Get-AzureSubscription cmdlet, and then stores that value in hello $Subscriptions variable.</span></span>

    $Subscriptions = Get-AzureSubscription



<span data-ttu-id="c0a07-197">hello negyedik parancs hello $AzureVmNetworks változó a Get-AzureVNetSite hello parancsmagot, és ezt az értéket segítségével kéri le egy Azure virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="c0a07-197">hello fourth command gets Azure virtual networks by using hello Get-AzureVNetSite cmdlet, and then that value in hello $AzureVmNetworks variable.</span></span>

    $AzureVmNetworks = Get-AzureVNetSite



<span data-ttu-id="c0a07-198">hello végső parancsmag hello elsődleges hálózati és hello Azure virtuális gép hálózati közötti leképezést hoz létre.</span><span class="sxs-lookup"><span data-stu-id="c0a07-198">hello final cmdlet creates a mapping between hello primary network and hello Azure virtual machine network.</span></span> <span data-ttu-id="c0a07-199">hello parancsmag hello elsődleges hálózati $Networks hello első eleme határozza meg.</span><span class="sxs-lookup"><span data-stu-id="c0a07-199">hello cmdlet specifies hello primary network as hello first element of $Networks.</span></span> <span data-ttu-id="c0a07-200">hello parancsmag határozza meg a virtuálisgép-hálózat első elemének hello $AzureVmNetworks azonosítójú használatával</span><span class="sxs-lookup"><span data-stu-id="c0a07-200">hello cmdlet specifies a virtual machine network as hello first element of $AzureVmNetworks by using its ID.</span></span> <span data-ttu-id="c0a07-201">hello parancs tartalmazza az Azure-előfizetés-azonosítójával.</span><span class="sxs-lookup"><span data-stu-id="c0a07-201">hello command includes your Azure subscription ID.</span></span>

    New-AzureSiteRecoveryNetworkMapping -PrimaryNetwork $Networks[0] -AzureSubscriptionId $Subscriptions[0].SubscriptionId -AzureVMNetworkId $AzureVmNetworks[0].Id


## <a name="step-9-enable-protection-for-virtual-machines"></a><span data-ttu-id="c0a07-202">9. lépés: Virtuális gépek védelmének engedélyezése</span><span class="sxs-lookup"><span data-stu-id="c0a07-202">Step 9: Enable protection for virtual machines</span></span>
<span data-ttu-id="c0a07-203">Kiszolgálók, felhők és hálózatok megfelelő konfigurálását követően engedélyezheti a hello felhő virtuális gépek védelmét.</span><span class="sxs-lookup"><span data-stu-id="c0a07-203">After servers, clouds, and networks are configured correctly, you can enable protection for virtual machines in hello cloud.</span></span> <span data-ttu-id="c0a07-204">Vegye figyelembe a következőket hello:</span><span class="sxs-lookup"><span data-stu-id="c0a07-204">Note hello following:</span></span>

<span data-ttu-id="c0a07-205">A virtuális gépeknek meg kell felelniük [Azure virtuális gép Előfeltételek](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).</span><span class="sxs-lookup"><span data-stu-id="c0a07-205">Virtual machines must meet [Azure virtual machine prerequisites](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).</span></span>

<span data-ttu-id="c0a07-206">hello virtuális gép tooenable védelmi hello operációsrendszer- és operációsrendszer-lemez tulajdonságait kell állítani.</span><span class="sxs-lookup"><span data-stu-id="c0a07-206">tooenable protection hello operating system and operating system disk properties must be set for hello virtual machine.</span></span> <span data-ttu-id="c0a07-207">Ha egy virtuális gép létrehozása a VMM-virtuálisgép-sablon használatával beállíthatja hello tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="c0a07-207">When you create a virtual machine in VMM using a virtual machine template you can set hello property.</span></span> <span data-ttu-id="c0a07-208">Ezeket a tulajdonságokat a meglévő virtuális gépek állítsa be a hello **általános** és **hardverkonfiguráció** hello virtuális gép tulajdonságok lapján.</span><span class="sxs-lookup"><span data-stu-id="c0a07-208">You can also set these properties for existing virtual machines on hello **General** and **Hardware Configuration** tabs of hello virtual machine properties.</span></span> <span data-ttu-id="c0a07-209">Ha ezeket a tulajdonságokat a VMM-ben nem állít be fog tudni tooconfigure őket hello Azure Site Recovery portálon.</span><span class="sxs-lookup"><span data-stu-id="c0a07-209">If you don't set these properties in VMM you'll be able tooconfigure them in hello Azure Site Recovery portal.</span></span>

1. <span data-ttu-id="c0a07-210">Futtassa a következő parancs tooget hello védelmi tároló hello tooenable védelem:</span><span class="sxs-lookup"><span data-stu-id="c0a07-210">tooenable protection, run hello following command tooget hello protection container:</span></span>

     <span data-ttu-id="c0a07-211">$ProtectionContainer = get-AzureSiteRecoveryProtectionContainer-$CloudName neve</span><span class="sxs-lookup"><span data-stu-id="c0a07-211">$ProtectionContainer = Get-AzureSiteRecoveryProtectionContainer -Name $CloudName</span></span>
2. <span data-ttu-id="c0a07-212">Töltse le a hello védelmi entitás (VM) hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="c0a07-212">Get hello protection entity (VM) by running hello following command:</span></span>

        $protectionEntity = Get-AzureSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer



1. <span data-ttu-id="c0a07-213">A virtuális gép hello vész-Helyreállítási hello engedélyezése hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="c0a07-213">Enable hello DR for hello VM by running hello following command:</span></span>

        $jobResult = Set-AzureSiteRecoveryProtectionEntity -ProtectionEntity $protectionEntity     -Protection Enable -Force



## <a name="test-your-deployment"></a><span data-ttu-id="c0a07-214">A központi telepítés tesztelése</span><span class="sxs-lookup"><span data-stu-id="c0a07-214">Test your deployment</span></span>
<span data-ttu-id="c0a07-215">Tervezze meg a központi telepítés futtathat egyetlen virtuális gép feladatátvételi tesztjét, vagy több virtuális gép álló és hello a feladatátvételi teszt futtatása helyreállítási terv létrehozása tootest.</span><span class="sxs-lookup"><span data-stu-id="c0a07-215">tootest your deployment you can run a test failover for a single virtual machine, or create a recovery plan consisting of multiple virtual machines and run a test failover for hello plan.</span></span> <span data-ttu-id="c0a07-216">A feladatátvételi teszt segítségével elkülönített hálózatban próbálhatja ki a feladatátvételi és helyreállítási mechanizmusokat.</span><span class="sxs-lookup"><span data-stu-id="c0a07-216">Test failover simulates your failover and recovery mechanism in an isolated network.</span></span> <span data-ttu-id="c0a07-217">Vegye figyelembe:</span><span class="sxs-lookup"><span data-stu-id="c0a07-217">Note that:</span></span>

* <span data-ttu-id="c0a07-218">Ha tooconnect toohello virtuális gépet az Azure-ban a távoli asztal hello feladatátvétel után, engedélyezze a távoli asztali kapcsolat hello virtuális gépen, hello a feladatátvételi teszt futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="c0a07-218">If you want tooconnect toohello virtual machine in Azure using Remote Desktop after hello failover, enable Remote Desktop Connection on hello virtual machine before you run hello test failover.</span></span>
* <span data-ttu-id="c0a07-219">A feladatátvétel után a nyilvános IP cím tooconnect toohello virtuális gépek fogja használni a távoli asztali kapcsolat segítségével.</span><span class="sxs-lookup"><span data-stu-id="c0a07-219">After failover, you'll use a public IP address tooconnect toohello virtual machine in Azure using Remote Desktop.</span></span> <span data-ttu-id="c0a07-220">Ha meg akarja toodo, győződjön meg arról, nincs érvényben tartományi szabályzatok, amelyek meggátolják, kapcsolódó tooa virtuális gép nyilvános cím segítségével.</span><span class="sxs-lookup"><span data-stu-id="c0a07-220">If you want toodo this, ensure you don't have any domain policies that prevent you from connecting tooa virtual machine using a public address.</span></span>

<span data-ttu-id="c0a07-221">hello művelet, toocheck hello befejezése kövesse hello [figyelési tevékenység](#monitor).</span><span class="sxs-lookup"><span data-stu-id="c0a07-221">toocheck hello completion of hello operation, follow hello steps in [Monitor Activity](#monitor).</span></span>

### <a name="create-a-recovery-plan"></a><span data-ttu-id="c0a07-222">Helyreállítási terv létrehozása</span><span class="sxs-lookup"><span data-stu-id="c0a07-222">Create a recovery plan</span></span>
1. <span data-ttu-id="c0a07-223">Hozzon létre egy .xml fájlt a helyreállítási terv hello adatok az alábbi sablont, és elmentse "C:\RPTemplatePath.xml".</span><span class="sxs-lookup"><span data-stu-id="c0a07-223">Create an .xml file as a template for your recovery plan using hello data below, and then save it as "C:\RPTemplatePath.xml".</span></span>
2. <span data-ttu-id="c0a07-224">Hello RecoveryPlan csomópont azonosító, név, PrimaryServerId és SecondaryServerId módosítása.</span><span class="sxs-lookup"><span data-stu-id="c0a07-224">Change hello RecoveryPlan node Id, Name, PrimaryServerId, and SecondaryServerId.</span></span>
3. <span data-ttu-id="c0a07-225">Hello ProtectionEntity csomópont PrimaryProtectionEntityId (a VMM-ből vmid) módosítása.</span><span class="sxs-lookup"><span data-stu-id="c0a07-225">Change hello ProtectionEntity node PrimaryProtectionEntityId (vmid from VMM).</span></span>
4. <span data-ttu-id="c0a07-226">További virtuális gépeket adhat hozzá további ProtectionEntity csomópontok hozzáadásával.</span><span class="sxs-lookup"><span data-stu-id="c0a07-226">You can add more VMs by adding more ProtectionEntity nodes.</span></span>

        <#
        <?xml version="1.0" encoding="utf-16"?>
        <RecoveryPlan Id="d0323b26-5be2-471b-addc-0a8742796610" Name="rp-test"     PrimaryServerId="9350a530-d5af-435b-9f2b-b941b5d9fcd5"     SecondaryServerId="21a9403c-6ec1-44f2-b744-b4e50b792387" Description=""     Version="V2014_07">
          <Actions />
          <ActionGroups>
            <ShutdownAllActionGroup Id="ShutdownAllActionGroup">
              <PreActionSequence />
              <PostActionSequence />
            </ShutdownAllActionGroup>
            <FailoverAllActionGroup Id="FailoverAllActionGroup">
              <PreActionSequence />
              <PostActionSequence />
            </FailoverAllActionGroup>
            <BootActionGroup Id="DefaultActionGroup">
              <PreActionSequence />
              <PostActionSequence />
              <ProtectionEntity PrimaryProtectionEntityId="d4c8ce92-a613-4c63-9b03-    cf163cc36ef8" />
            </BootActionGroup>
          </ActionGroups>
          <ActionGroupSequence>
            <ActionGroup Id="ShutdownAllActionGroup" ActionId="ShutdownAllActionGroup"     Before="FailoverAllActionGroup" />
            <ActionGroup Id="FailoverAllActionGroup" ActionId="FailoverAllActionGroup"     After="ShutdownAllActionGroup" Before="DefaultActionGroup" />
            <ActionGroup Id="DefaultActionGroup" ActionId="DefaultActionGroup" After="FailoverAllActionGroup"/>
          </ActionGroupSequence>
        </RecoveryPlan>
        #>



1. <span data-ttu-id="c0a07-227">Töltse ki hello sablon hello adatokat:</span><span class="sxs-lookup"><span data-stu-id="c0a07-227">Fill in hello data in hello template:</span></span>

        $TemplatePath = "C:\RPTemplatePath.xml";



1. <span data-ttu-id="c0a07-228">Hello RecoveryPlan létrehozása:</span><span class="sxs-lookup"><span data-stu-id="c0a07-228">Create hello RecoveryPlan:</span></span>

        $RPCreationJob = New-AzureSiteRecoveryRecoveryPlan -File $TemplatePath -WaitForCompletion;

### <a name="run-a-test-failover"></a><span data-ttu-id="c0a07-229">Feladatátvételi teszt futtatása</span><span class="sxs-lookup"><span data-stu-id="c0a07-229">Run a test failover</span></span>
1. <span data-ttu-id="c0a07-230">Hello RecoveryPlan objektum beolvasása hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="c0a07-230">Get hello RecoveryPlan object by running hello following command:</span></span>

     <span data-ttu-id="c0a07-231">$RPObject = get-AzureSiteRecoveryRecoveryPlan-Name $RPName;</span><span class="sxs-lookup"><span data-stu-id="c0a07-231">$RPObject = Get-AzureSiteRecoveryRecoveryPlan -Name $RPName;</span></span>
2. <span data-ttu-id="c0a07-232">Indítsa el a hello feladatátvételi teszthez hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="c0a07-232">Start hello test failover by running hello following command:</span></span>

        $jobIDResult = Start-AzureSiteRecoveryTestFailoverJob -RecoveryPlan $RPObject -Direction PrimaryToRecovery;


## <span data-ttu-id="c0a07-233"><a name=monitor></a>Figyelési tevékenység</span><span class="sxs-lookup"><span data-stu-id="c0a07-233"><a name=monitor></a> Monitor Activity</span></span>
<span data-ttu-id="c0a07-234">A következő parancsok toomonitor hello tevékenység hello használata.</span><span class="sxs-lookup"><span data-stu-id="c0a07-234">Use hello following commands toomonitor hello activity.</span></span> <span data-ttu-id="c0a07-235">Vegye figyelembe, hogy rendelkezik-e toowait Between hello feldolgozási toofinish feladataihoz.</span><span class="sxs-lookup"><span data-stu-id="c0a07-235">Note that you have toowait in between jobs for hello processing toofinish.</span></span>

    Do
    {
            $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
            Write-Host "Job State:{0}, StateDescription:{1}" -f Job.State, $job.StateDescription;
            if($job -eq $null -or $job.StateDescription -ne "Completed")
            {
                $isJobLeftForProcessing = $true;
            }

        if($isJobLeftForProcessing)
            {
                Start-Sleep -Seconds 60
            }
    }While($isJobLeftForProcessing)



## <a name="next-steps"></a><span data-ttu-id="c0a07-236">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c0a07-236">Next steps</span></span>
<span data-ttu-id="c0a07-237">[További](/powershell/azure/overview) Azure Site Recovery PowerShell-parancsmagokkal.</span><span class="sxs-lookup"><span data-stu-id="c0a07-237">[Read more](/powershell/azure/overview) about Azure Site Recovery PowerShell cmdlets.</span></span> <span data-ttu-id="c0a07-238"></a>.</span><span class="sxs-lookup"><span data-stu-id="c0a07-238"></a>.</span></span>
