---
title: "Hyper-V virtuális gépek PowerShell és az Azure Resource Manager aaaReplicate |} Microsoft Docs"
description: "A Hyper-V virtuális gépek tooAzure hello replikációs automatizálhatja az Azure Site Recovery PowerShell és az Azure Resource Manager használatával."
services: site-recovery
documentationcenter: 
author: bsiva
manager: abhiag
editor: 
ms.assetid: 05e0d76e-c3f5-4845-8052-094019b6d102
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/31/2017
ms.author: bsiva
ms.openlocfilehash: 4fb15ce2e9ad54f1dd6f54ff769eb912aa4b0272
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-between-on-premises-hyper-v-virtual-machines-and-azure-by-using-powershell-and-azure-resource-manager"></a><span data-ttu-id="c39d8-103">PowerShell és az Azure Resource Manager használatával a helyszíni Hyper-V virtuális gépek és az Azure közötti replikáció</span><span class="sxs-lookup"><span data-stu-id="c39d8-103">Replicate between on-premises Hyper-V virtual machines and Azure by using PowerShell and Azure Resource Manager</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c39d8-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c39d8-104">Azure Portal</span></span>](site-recovery-hyper-v-site-to-azure.md)
> * [<span data-ttu-id="c39d8-105">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="c39d8-105">PowerShell - Resource Manager</span></span>](site-recovery-deploy-with-powershell-resource-manager.md)
> * [<span data-ttu-id="c39d8-106">Klasszikus portál</span><span class="sxs-lookup"><span data-stu-id="c39d8-106">Classic Portal</span></span>](site-recovery-hyper-v-site-to-azure-classic.md)
>
>

## <a name="overview"></a><span data-ttu-id="c39d8-107">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="c39d8-107">Overview</span></span>
<span data-ttu-id="c39d8-108">Az Azure Site Recovery hozzájárul tooyour üzleti folytonossági és vészhelyreállítási helyreállítási stratégia megvalósításában a replikáció, feladatátvétel és a különböző telepítési forgatókönyvek esetén a virtuális gépek helyreállítása.</span><span class="sxs-lookup"><span data-stu-id="c39d8-108">Azure Site Recovery contributes tooyour business continuity and disaster recovery strategy by orchestrating replication, failover, and recovery of virtual machines in a number of deployment scenarios.</span></span> <span data-ttu-id="c39d8-109">Központi telepítési forgatókönyvek teljes listáját lásd: hello [Azure Site Recovery áttekintését](site-recovery-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c39d8-109">For a full list of deployment scenarios, see hello [Azure Site Recovery overview](site-recovery-overview.md).</span></span>

<span data-ttu-id="c39d8-110">Az Azure PowerShell egy modul, amely a parancsmagok toomanage Azure, a Windows PowerShell segítségével.</span><span class="sxs-lookup"><span data-stu-id="c39d8-110">Azure PowerShell is a module that provides cmdlets toomanage Azure through Windows PowerShell.</span></span> <span data-ttu-id="c39d8-111">Együtt tudjon működni az kétféle modulok: hello Azure profil modul vagy hello Azure Resource Manager modul.</span><span class="sxs-lookup"><span data-stu-id="c39d8-111">It can work with two types of modules: hello Azure Profile module, or hello Azure Resource Manager module.</span></span>

<span data-ttu-id="c39d8-112">Site Recovery PowerShell parancsmagokat, elérhető az Azure PowerShell az Azure Resource Manager segítségével védelme és helyreállítása a kiszolgálók az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="c39d8-112">Site Recovery PowerShell cmdlets, available with Azure PowerShell for Azure Resource Manager, help you protect and recover your servers in Azure.</span></span>

<span data-ttu-id="c39d8-113">Ez a cikk ismerteti, hogyan toouse Windows PowerShell, együtt Azure Resource Manager, a Site Recovery tooconfigure toodeploy és levezényléséhez server védelmi tooAzure.</span><span class="sxs-lookup"><span data-stu-id="c39d8-113">This article describes how toouse Windows PowerShell, together with Azure Resource Manager, toodeploy Site Recovery tooconfigure and orchestrate server protection tooAzure.</span></span> <span data-ttu-id="c39d8-114">a cikk ezt használja hello példa bemutatja, hogyan tooprotect, a feladatátvétel, és helyreállítani a virtuális gépeket a Hyper-V-gazdagép tooAzure, az Azure Resource Manager Azure PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="c39d8-114">hello example used in this article shows you how tooprotect, fail over, and recover virtual machines on a Hyper-V host tooAzure, by using Azure PowerShell with Azure Resource Manager.</span></span>

> [!NOTE]
> <span data-ttu-id="c39d8-115">hello Site Recovery PowerShell parancsmagokat jelenleg lehetővé teszik a következő tooconfigure hello: egy a Virtual Machine Manager-hely tooanother, egy a Virtual Machine Manager-hely tooAzure és a Hyper-V hely tooAzure.</span><span class="sxs-lookup"><span data-stu-id="c39d8-115">hello Site Recovery PowerShell cmdlets currently allow you tooconfigure hello following: one Virtual Machine Manager site tooanother, a Virtual Machine Manager site tooAzure, and a Hyper-V site tooAzure.</span></span>
>
>

<span data-ttu-id="c39d8-116">Nem kell a PowerShell szakértői toouse toobe ebben a cikkben, de toounderstand hello alapszintű fogalmakkal, mint a modulok, a parancsmagok és a munkamenetek van szüksége.</span><span class="sxs-lookup"><span data-stu-id="c39d8-116">You don't need toobe a PowerShell expert toouse this article, but you do need toounderstand hello basic concepts, such as modules, cmdlets, and sessions.</span></span> <span data-ttu-id="c39d8-117">További információ a Windows PowerShellről: [Ismerkedés a Windows PowerShell-lel](http://technet.microsoft.com/library/hh857337.aspx).</span><span class="sxs-lookup"><span data-stu-id="c39d8-117">For more information about Windows PowerShell, see [Getting Started with Windows PowerShell](http://technet.microsoft.com/library/hh857337.aspx).</span></span>

<span data-ttu-id="c39d8-118">Akkor is olvashat további tudnivalókat [az Azure PowerShell használata Azure Resource Managerrel](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="c39d8-118">You can also read more about [Using Azure PowerShell with Azure Resource Manager](../powershell-azure-resource-manager.md).</span></span>

> [!NOTE]
> <span data-ttu-id="c39d8-119">Microsoft-partnerek hello Cloud Solution Provider (CSP) program részét képező konfigurálható és kezelhető az ügyfelek kiszolgálók védelmét tootheir ügyfelek megfelelő CSP előfizetések (bérlői előfizetések).</span><span class="sxs-lookup"><span data-stu-id="c39d8-119">Microsoft partners that are part of hello Cloud Solution Provider (CSP) program can configure and manage protection of their customers' servers tootheir customers' respective CSP subscriptions (tenant subscriptions).</span></span>
>
>

## <a name="before-you-start"></a><span data-ttu-id="c39d8-120">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="c39d8-120">Before you start</span></span>
<span data-ttu-id="c39d8-121">Győződjön meg arról, hogy az előfeltételek teljesülnek:</span><span class="sxs-lookup"><span data-stu-id="c39d8-121">Make sure you have these prerequisites in place:</span></span>

* <span data-ttu-id="c39d8-122">A [Microsoft Azure](https://azure.microsoft.com/) fiók.</span><span class="sxs-lookup"><span data-stu-id="c39d8-122">A [Microsoft Azure](https://azure.microsoft.com/) account.</span></span> <span data-ttu-id="c39d8-123">Kezdésként használhatja az [ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/) is.</span><span class="sxs-lookup"><span data-stu-id="c39d8-123">You can start with a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> <span data-ttu-id="c39d8-124">Emellett áttekintheti az [Azure Site Recovery Manager árképzési](https://azure.microsoft.com/pricing/details/site-recovery/).</span><span class="sxs-lookup"><span data-stu-id="c39d8-124">In addition, you can read about [Azure Site Recovery Manager pricing](https://azure.microsoft.com/pricing/details/site-recovery/).</span></span>
* <span data-ttu-id="c39d8-125">Azure PowerShell 1.0.</span><span class="sxs-lookup"><span data-stu-id="c39d8-125">Azure PowerShell 1.0.</span></span> <span data-ttu-id="c39d8-126">Ebben a kiadásban információt, és hogyan tooinstall, lásd: [Azure PowerShell 1.0.](https://azure.microsoft.com/)</span><span class="sxs-lookup"><span data-stu-id="c39d8-126">For information about this release and how tooinstall it, see [Azure PowerShell 1.0.](https://azure.microsoft.com/)</span></span>
* <span data-ttu-id="c39d8-127">Hello [AzureRM.SiteRecovery](https://www.powershellgallery.com/packages/AzureRM.SiteRecovery/) és [AzureRM.RecoveryServices](https://www.powershellgallery.com/packages/AzureRM.RecoveryServices/) modulok.</span><span class="sxs-lookup"><span data-stu-id="c39d8-127">hello [AzureRM.SiteRecovery](https://www.powershellgallery.com/packages/AzureRM.SiteRecovery/) and [AzureRM.RecoveryServices](https://www.powershellgallery.com/packages/AzureRM.RecoveryServices/) modules.</span></span> <span data-ttu-id="c39d8-128">Ezek a modulok hello legújabb verziója letölthető hello [PowerShell-galériában](https://www.powershellgallery.com/)</span><span class="sxs-lookup"><span data-stu-id="c39d8-128">You can get hello latest versions of these modules from hello [PowerShell gallery](https://www.powershellgallery.com/)</span></span>

<span data-ttu-id="c39d8-129">Ez a cikk bemutatja, hogyan toouse Azure PowerShell-lel rendelkező Azure Resource Manager tooconfigure és a kiszolgálók a védelem kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="c39d8-129">This article illustrates how toouse Azure Powershell with Azure Resource Manager tooconfigure and manage protection of your servers.</span></span> <span data-ttu-id="c39d8-130">a cikk ezt használja hello példa bemutatja, hogyan tooprotect egy olyan virtuális géphez, a Hyper-V gazdagépen tooAzure.</span><span class="sxs-lookup"><span data-stu-id="c39d8-130">hello example used in this article shows you how tooprotect a virtual machine, running on a Hyper-V host, tooAzure.</span></span> <span data-ttu-id="c39d8-131">a következő előfeltételek hello adott toothis példa.</span><span class="sxs-lookup"><span data-stu-id="c39d8-131">hello following prerequisites are specific toothis example.</span></span> <span data-ttu-id="c39d8-132">Átfogóbb állítja be a hello követelményei a Site Recovery különböző lehetőségeket, tekintse meg a vonatkozó toothat forgatókönyv toohello dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="c39d8-132">For a more comprehensive set of requirements for hello various Site Recovery scenarios, refer toohello documentation pertaining toothat scenario.</span></span>

* <span data-ttu-id="c39d8-133">A Hyper-V gazdagépen fut a Windows Server 2012 R2 vagy Microsoft Hyper-V Server 2012 R2 tartalmazó egy vagy több virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="c39d8-133">A Hyper-V host running Windows Server 2012 R2 or Microsoft Hyper-V Server 2012 R2 containing one or more virtual machines.</span></span>
* <span data-ttu-id="c39d8-134">Hyper-V kiszolgálók toohello Internet, csatlakoztatva, közvetlen vagy proxyn keresztül.</span><span class="sxs-lookup"><span data-stu-id="c39d8-134">Hyper-V servers connected toohello Internet, either directly or through a proxy.</span></span>
* <span data-ttu-id="c39d8-135">hello tooprotect kívánt virtuális gépek meg kell felelnie az [a virtuális gép előfeltételei](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).</span><span class="sxs-lookup"><span data-stu-id="c39d8-135">hello virtual machines you want tooprotect should conform with [Virtual Machine prerequisites](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).</span></span>

## <a name="step-1-sign-in-tooyour-azure-account"></a><span data-ttu-id="c39d8-136">1. lépés: Bejelentkezés tooyour Azure-fiók</span><span class="sxs-lookup"><span data-stu-id="c39d8-136">Step 1: Sign in tooyour Azure account</span></span>
1. <span data-ttu-id="c39d8-137">Nyisson meg egy PowerShell-konzolt, és futtassa a parancsot toosign tooyour Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="c39d8-137">Open a PowerShell console and run this command toosign in tooyour Azure account.</span></span> <span data-ttu-id="c39d8-138">hello parancsmag megnyitása egy weblap, amelyen kérni fogja a hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="c39d8-138">hello cmdlet brings up a web page that will prompt you for your account credentials.</span></span>

        Login-AzureRmAccount

    <span data-ttu-id="c39d8-139">Alternatív megoldásként akkor is lehetnek a fiók hitelesítő adataival mint egy paraméterrel toohello `Login-AzureRmAccount` parancsmag hello segítségével `-Credential` paraméter.</span><span class="sxs-lookup"><span data-stu-id="c39d8-139">Alternately, you could also include your account credentials as a parameter toohello `Login-AzureRmAccount` cmdlet, by using hello `-Credential` parameter.</span></span>

    <span data-ttu-id="c39d8-140">Ha működik a bérlő nevében CSP partner, adja meg hello ügyfél a bérlők a tenantID vagy bérlői elsődleges tartománynév használatával.</span><span class="sxs-lookup"><span data-stu-id="c39d8-140">If you are CSP partner working on behalf of a tenant, specify hello customer as a tenant, by using their tenantID or tenant primary domain name.</span></span>

        Login-AzureRmAccount -Tenant "fabrikam.com"
2. <span data-ttu-id="c39d8-141">Egy fiók több előfizetések van így toouse hello fiókkal szeretne hello előfizetés társítania kell.</span><span class="sxs-lookup"><span data-stu-id="c39d8-141">An account can have several subscriptions, so you should associate hello subscription you want toouse with hello account.</span></span>

        Select-AzureRmSubscription -SubscriptionName $SubscriptionName
3. <span data-ttu-id="c39d8-142">Győződjön meg arról, hogy az előfizetéshez regisztrált toouse hello szolgáltatók az Azure Recovery Services és a hely helyreállítását követően hello a következő parancsok használatával:</span><span class="sxs-lookup"><span data-stu-id="c39d8-142">Verify that your subscription is registered toouse hello Azure providers for Recovery Services and Site Recovery, by using hello following commands:</span></span>

   * `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices`
   * `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`

   <span data-ttu-id="c39d8-143">A parancsok, ha hello hello kimenetéből **RegistrationState** értéke túl**regisztrált**, folytathatja a tooStep 2.</span><span class="sxs-lookup"><span data-stu-id="c39d8-143">In hello output of these commands, if hello **RegistrationState** is set too**Registered**, you can proceed tooStep 2.</span></span> <span data-ttu-id="c39d8-144">Ha nem, hello hiányzó szolgáltató regisztrálnia kell az előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="c39d8-144">If not, you should register hello missing provider in your subscription.</span></span>

   <span data-ttu-id="c39d8-145">tooregister hello Azure provider a Site Recovery és helyreállítási szolgáltatásokhoz, futtassa a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="c39d8-145">tooregister hello Azure provider for Site Recovery and Recovery Services, run hello following commands:</span></span>

       Register-AzureRmResourceProvider -ProviderNamespace Microsoft.SiteRecovery
       Register-AzureRmResourceProvider -ProviderNamespace Microsoft.RecoveryServices

   <span data-ttu-id="c39d8-146">Győződjön meg arról, hogy hello szolgáltatók regisztrálása sikeresen hello a következő parancsok használatával: `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices` és `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`.</span><span class="sxs-lookup"><span data-stu-id="c39d8-146">Verify that hello providers registered successfully by using hello following commands: `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices` and `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`.</span></span>

## <a name="step-2-set-up-hello-recovery-services-vault"></a><span data-ttu-id="c39d8-147">2. lépés: A Recovery Services-tároló hello beállítása</span><span class="sxs-lookup"><span data-stu-id="c39d8-147">Step 2: Set up hello Recovery Services vault</span></span>
1. <span data-ttu-id="c39d8-148">Hozzon létre egy Azure Resource Manager-csoportot, amelyben meg kell hello tároló létrehozása, vagy használjon egy meglévő erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="c39d8-148">Create an Azure Resource Manager resource group in which you'll create hello vault, or use an existing resource group.</span></span> <span data-ttu-id="c39d8-149">Létrehozhat egy új erőforráscsoportot hello a következő parancs használatával:</span><span class="sxs-lookup"><span data-stu-id="c39d8-149">You can create a new resource group by using hello following command:</span></span>

        New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Geo  

    <span data-ttu-id="c39d8-150">hello $ResourceGroupName változó hello erőforráscsoport hello nevét tartalmazza, ahol azt szeretné, hogy toocreate, és hello $Geo változót tartalmaz hello Azure régióban mely toocreate hello erőforráscsoportban (például "Dél-Brazília").</span><span class="sxs-lookup"><span data-stu-id="c39d8-150">where hello $ResourceGroupName variable contains hello name of hello resource group you want toocreate, and hello $Geo variable contains hello Azure region in which toocreate hello resource group (for example, "Brazil South").</span></span>

    <span data-ttu-id="c39d8-151">Ezt úgy szerezheti be az erőforráscsoportok listáját az előfizetésében hello segítségével `Get-AzureRmResourceGroup` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="c39d8-151">You can obtain a list of resource groups in your subscription by using hello `Get-AzureRmResourceGroup` cmdlet.</span></span>
2. <span data-ttu-id="c39d8-152">Hozzon létre egy új Azure Recovery Services-tároló az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="c39d8-152">Create a new Azure Recovery Services vault as follows:</span></span>

        $vault = New-AzureRmRecoveryServicesVault -Name <string> -ResourceGroupName <string> -Location <string>

    <span data-ttu-id="c39d8-153">Hello segítségével kérheti le a meglévő tárolók listája `Get-AzureRmRecoveryServicesVault` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="c39d8-153">You can retrieve a list of existing vaults by using hello `Get-AzureRmRecoveryServicesVault` cmdlet.</span></span>

> [!NOTE]
> <span data-ttu-id="c39d8-154">Ha a klasszikus portál hello vagy hello Azure Service Management PowerShell-modul segítségével létrehozni a Site Recovery-tárolók tooperform műveleteket, hello segítségével kérheti le az ilyen tárolók listája `Get-AzureRmSiteRecoveryVault` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="c39d8-154">If you wish tooperform operations on Site Recovery vaults created using hello classic portal or hello Azure Service Management PowerShell module, you can retrieve a list of such vaults by using hello `Get-AzureRmSiteRecoveryVault` cmdlet.</span></span> <span data-ttu-id="c39d8-155">Hozzon létre egy új Recovery Services-tároló minden új műveletnél.</span><span class="sxs-lookup"><span data-stu-id="c39d8-155">You should create a new Recovery Services vault for all new operations.</span></span> <span data-ttu-id="c39d8-156">korábban létrehozott hello Site Recovery tárolók támogatottak, de nem rendelkezik a legújabb funkciókat hello.</span><span class="sxs-lookup"><span data-stu-id="c39d8-156">hello Site Recovery vaults you've created earlier are supported, but don't have hello latest features.</span></span>
>
>

## <a name="step-3-set-hello-recovery-services-vault-context"></a><span data-ttu-id="c39d8-157">3. lépés: Hello Recovery Services-tároló környezet beállítása</span><span class="sxs-lookup"><span data-stu-id="c39d8-157">Step 3: Set hello Recovery Services vault context</span></span>
1. <span data-ttu-id="c39d8-158">Hello tároló környezet beállítása hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="c39d8-158">Set hello vault context by running hello following command:</span></span>

       Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="step-4-create-a-hyper-v-site-and-generate-a-new-vault-registration-key-for-hello-site"></a><span data-ttu-id="c39d8-159">4. lépés: Hyper-V-hely létrehozása, és hozzon létre egy új tároló regisztrációs kulcsot hello helyhez.</span><span class="sxs-lookup"><span data-stu-id="c39d8-159">Step 4: Create a Hyper-V site and generate a new vault registration key for hello site.</span></span>
1. <span data-ttu-id="c39d8-160">Hozzon létre egy új Hyper-V hely az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="c39d8-160">Create a new Hyper-V site as follows:</span></span>

        $sitename = "MySite"                #Specify site friendly name
        New-AzureRmSiteRecoverySite -Name $sitename

    <span data-ttu-id="c39d8-161">Ez a parancsmag elindítja a Site Recovery feladat toocreate hello helyet, és a Site Recovery feladat objektum beállítása/beolvasása.</span><span class="sxs-lookup"><span data-stu-id="c39d8-161">This cmdlet starts a Site Recovery job toocreate hello site, and returns a Site Recovery job object.</span></span> <span data-ttu-id="c39d8-162">Hello feladat toocomplete várja meg, és győződjön meg arról, hogy hello feladat sikeresen befejeződött.</span><span class="sxs-lookup"><span data-stu-id="c39d8-162">Wait for hello job toocomplete and verify that hello job completed successfully.</span></span>

    <span data-ttu-id="c39d8-163">Hello feladatobjektum beolvashatja, és ezáltal állapotának hello aktuális hello feladat hello Get-AzureRmSiteRecoveryJob parancsmag használatával.</span><span class="sxs-lookup"><span data-stu-id="c39d8-163">You can retrieve hello job object, and thereby check hello current status of hello job, by using hello Get-AzureRmSiteRecoveryJob cmdlet.</span></span>
2. <span data-ttu-id="c39d8-164">Létrehozni, és töltse le a regisztrációs kulcs hello helyhez, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="c39d8-164">Generate and download a registration key for hello site, as follows:</span></span>

        $SiteIdentifier = Get-AzureRmSiteRecoverySite -Name $sitename | Select -ExpandProperty SiteIdentifier
        Get-AzureRmRecoveryServicesVaultSettingsFile -Vault $vault -SiteIdentifier $SiteIdentifier -SiteFriendlyName $sitename -Path $Path

    <span data-ttu-id="c39d8-165">Másolás hello letöltött kulcs toohello Hyper-V gazdagépen.</span><span class="sxs-lookup"><span data-stu-id="c39d8-165">Copy hello downloaded key toohello Hyper-V host.</span></span> <span data-ttu-id="c39d8-166">Hello kulcs tooregister hello Hyper-V gazdagép toohello hely van szüksége.</span><span class="sxs-lookup"><span data-stu-id="c39d8-166">You need hello key tooregister hello Hyper-V host toohello site.</span></span>

## <a name="step-5-install-hello-azure-site-recovery-provider-and-azure-recovery-services-agent-on-your-hyper-v-host"></a><span data-ttu-id="c39d8-167">5. lépés: Hello Azure Site Recovery provider és az Azure Recovery Services Agent telepítése a Hyper-V gazdagépen</span><span class="sxs-lookup"><span data-stu-id="c39d8-167">Step 5: Install hello Azure Site Recovery provider and Azure Recovery Services Agent on your Hyper-V host</span></span>
1. <span data-ttu-id="c39d8-168">Hello telepítő hello hello szolgáltató legújabb verziójának letöltése [Microsoft](https://aka.ms/downloaddra).</span><span class="sxs-lookup"><span data-stu-id="c39d8-168">Download hello installer for hello latest version of hello provider from [Microsoft](https://aka.ms/downloaddra).</span></span>
2. <span data-ttu-id="c39d8-169">A Hyper-V gazdagépen, és hello végén lévő hello telepítés futtatási hello telepítő toohello regisztrációs lépésében továbbra is.</span><span class="sxs-lookup"><span data-stu-id="c39d8-169">Run hello installer on your Hyper-V host, and at hello end of hello installation continue toohello registration step.</span></span>
3. <span data-ttu-id="c39d8-170">Amikor a rendszer kéri, adja meg a hello letöltött hely regisztrációs kulcsot, és hello Hyper-V gazdagép toohello hely regisztráció elvégzésére.</span><span class="sxs-lookup"><span data-stu-id="c39d8-170">When prompted, provide hello downloaded site registration key, and complete registration of hello Hyper-V host toohello site.</span></span>
4. <span data-ttu-id="c39d8-171">Győződjön meg arról, hogy hello Hyper-V gazdagép regisztrált toohello hely hello a következő parancs használatával:</span><span class="sxs-lookup"><span data-stu-id="c39d8-171">Verify that hello Hyper-V host is registered toohello site by using hello following command:</span></span>

        $server =  Get-AzureRmSiteRecoveryServer -FriendlyName $server-friendlyname

## <a name="step-6-create-a-replication-policy-and-associate-it-with-hello-protection-container"></a><span data-ttu-id="c39d8-172">6. lépés: Hozzon létre egy replikációs házirendet, és társítsa azt az üdvözlő védelmi tároló</span><span class="sxs-lookup"><span data-stu-id="c39d8-172">Step 6: Create a replication policy and associate it with hello protection container</span></span>
1. <span data-ttu-id="c39d8-173">Egy replikációs házirend létrehozásához az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="c39d8-173">Create a replication policy as follows:</span></span>

        $ReplicationFrequencyInSeconds = "300";        #options are 30,300,900
        $PolicyName = “replicapolicy”
        $Recoverypoints = 6                    #specify hello number of recovery points
        $storageaccountID = Get-AzureRmStorageAccount -Name "mystorea" -ResourceGroupName "MyRG" | Select -ExpandProperty Id

        $PolicyResult = New-AzureRmSiteRecoveryPolicy -Name $PolicyName -ReplicationProvider “HyperVReplicaAzure” -ReplicationFrequencyInSeconds $ReplicationFrequencyInSeconds  -RecoveryPoints $Recoverypoints -ApplicationConsistentSnapshotFrequencyInHours 1 -RecoveryAzureStorageAccountId $storageaccountID

    <span data-ttu-id="c39d8-174">Jelölőnégyzet hello visszaadott feladat tooensure hello replikációs házirend létrehozása sikeres.</span><span class="sxs-lookup"><span data-stu-id="c39d8-174">Check hello returned job tooensure that hello replication policy creation succeeds.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="c39d8-175">hello megadott tárfiók kell lennie a hello és a Recovery Services-tárolónak ugyanazon Azure-régiót, és rendelkeznie kell a georeplikáció engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="c39d8-175">hello storage account specified should be in hello same Azure region as your Recovery Services vault, and should have geo-replication enabled.</span></span>
   >
   > * <span data-ttu-id="c39d8-176">Hello tárolási fiók típusa (klasszikus) Azure Storage nem helyreállítási megadása esetén a hello feladatainak átvétele védett gépek helyreállítás hello gép tooAzure IaaS (klasszikus).</span><span class="sxs-lookup"><span data-stu-id="c39d8-176">If hello specified Recovery storage account is of type Azure Storage (Classic), failover of hello protected machines recover hello machine tooAzure IaaS (Classic).</span></span>
   > * <span data-ttu-id="c39d8-177">Hello megadott helyreállítási tárfiók típusú Azure Storage (az Azure Resource Manager), ha a hello feladatainak átvétele védett gépek helyreállítás hello gép tooAzure IaaS (Azure Resource Manager).</span><span class="sxs-lookup"><span data-stu-id="c39d8-177">If hello specified Recovery storage account is of type Azure Storage (Azure Resource Manager), failover of hello protected machines recover hello machine tooAzure IaaS (Azure Resource Manager).</span></span>
   >
   >
2. <span data-ttu-id="c39d8-178">Hello védelmi tároló megfelelő toohello helyen, töltse le a következőképpen:</span><span class="sxs-lookup"><span data-stu-id="c39d8-178">Get hello protection container corresponding toohello site, as follows:</span></span>

        $protectionContainer = Get-AzureRmSiteRecoveryProtectionContainer
3. <span data-ttu-id="c39d8-179">Kezdje hello társítás hello védelmi tároló hello replikációs házirend, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="c39d8-179">Start hello association of hello protection container with hello replication policy, as follows:</span></span>

     <span data-ttu-id="c39d8-180">$Policy = get-AzureRmSiteRecoveryPolicy - FriendlyName $PolicyName $associationJob = a Start-AzureRmSiteRecoveryPolicyAssociationJob-házirend $Policy - PrimaryProtectionContainer $protectionContainer</span><span class="sxs-lookup"><span data-stu-id="c39d8-180">$Policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $PolicyName   $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy $Policy -PrimaryProtectionContainer $protectionContainer</span></span>

   <span data-ttu-id="c39d8-181">Hello társítás feladat toocomplete várja meg, és győződjön meg arról, hogy sikeresen befejeződött-e.</span><span class="sxs-lookup"><span data-stu-id="c39d8-181">Wait for hello association job toocomplete, and ensure that it completed successfully.</span></span>

## <a name="step-7-enable-protection-for-virtual-machines"></a><span data-ttu-id="c39d8-182">7. lépés: Virtuális gépek védelmének engedélyezése</span><span class="sxs-lookup"><span data-stu-id="c39d8-182">Step 7: Enable protection for virtual machines</span></span>
1. <span data-ttu-id="c39d8-183">Hello védelmi entitás megfelelő toohello tooprotect, az alábbiak szerint kívánt virtuális gép beolvasása:</span><span class="sxs-lookup"><span data-stu-id="c39d8-183">Get hello protection entity corresponding toohello VM you want tooprotect, as follows:</span></span>

        $VMFriendlyName = "Fabrikam-app"                    #Name of hello VM
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName
2. <span data-ttu-id="c39d8-184">Hello virtuális gépet, az alábbiak szerint védelmének megkezdéséhez:</span><span class="sxs-lookup"><span data-stu-id="c39d8-184">Start protecting hello virtual machine, as follows:</span></span>

        $Ostype = "Windows"                                 # "Windows" or "Linux"
        $DRjob = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionEntity -Policy $Policy -Protection Enable -RecoveryAzureStorageAccountId $storageaccountID  -OS $OStype -OSDiskName $protectionEntity.Disks[0].Name

   > [!IMPORTANT]
   > <span data-ttu-id="c39d8-185">hello megadott tárfiók kell lennie a hello és a Recovery Services-tárolónak ugyanazon Azure-régiót, és rendelkeznie kell a georeplikáció engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="c39d8-185">hello storage account specified should be in hello same Azure region as your Recovery Services vault, and should have geo-replication enabled.</span></span>
   >
   > * <span data-ttu-id="c39d8-186">Hello tárolási fiók típusa (klasszikus) Azure Storage nem helyreállítási megadása esetén a hello feladatainak átvétele védett gépek helyreállítás hello gép tooAzure IaaS (klasszikus).</span><span class="sxs-lookup"><span data-stu-id="c39d8-186">If hello specified Recovery storage account is of type Azure Storage (Classic), failover of hello protected machines recover hello machine tooAzure IaaS (Classic).</span></span>
   > * <span data-ttu-id="c39d8-187">Hello megadott helyreállítási tárfiók típusú Azure Storage (az Azure Resource Manager), ha a hello feladatainak átvétele védett gépek helyreállítás hello gép tooAzure IaaS (Azure Resource Manager).</span><span class="sxs-lookup"><span data-stu-id="c39d8-187">If hello specified Recovery storage account is of type Azure Storage (Azure Resource Manager), failover of hello protected machines recover hello machine tooAzure IaaS (Azure Resource Manager).</span></span>
   >
   > <span data-ttu-id="c39d8-188">Ha egynél több tooit csatlakoztatott lemez, adja meg a hello operációsrendszer-lemez hello használatával védett virtuális gép hello tartalmaz *OSDiskName* paraméter.</span><span class="sxs-lookup"><span data-stu-id="c39d8-188">If hello VM you are protecting has more than one disk attached tooit, specify hello operating system disk by using hello *OSDiskName* parameter.</span></span>
   >
   >
3. <span data-ttu-id="c39d8-189">Várja meg a virtuális gépek tooreach hello védett állapotban hello kezdeti replikálás után.</span><span class="sxs-lookup"><span data-stu-id="c39d8-189">Wait for hello virtual machines tooreach a protected state after hello initial replication.</span></span> <span data-ttu-id="c39d8-190">Ez eltarthat egy ideig, például a replikált adatok toobe hello mennyisége tényezőktől függően és felsőbb rétegbeli sávszélességgel tooAzure hello.</span><span class="sxs-lookup"><span data-stu-id="c39d8-190">This can take a while, depending on factors such as hello amount of data toobe replicated and hello available upstream bandwidth tooAzure.</span></span> <span data-ttu-id="c39d8-191">hello feladatállapotot és StateDescription frissítve lett, az alábbiak szerint hello virtuális gép védett állapotban elérése során.</span><span class="sxs-lookup"><span data-stu-id="c39d8-191">hello job State and StateDescription are updated as follows, upon hello VM reaching a protected state.</span></span>

        PS C:\> $DRjob = Get-AzureRmSiteRecoveryJob -Job $DRjob

        PS C:\> $DRjob | Select-Object -ExpandProperty State
        Succeeded

        PS C:\> $DRjob | Select-Object -ExpandProperty StateDescription
        Completed
4. <span data-ttu-id="c39d8-192">Helyreállítási tulajdonságait, például Virtuálisgép-szerepkör méretéhez hello és hello Azure hálózati tooattach hello virtuális gép hálózati illesztő kártyák tooupon feladatátvételi frissítése.</span><span class="sxs-lookup"><span data-stu-id="c39d8-192">Update recovery properties, such as hello VM role size, and hello Azure network tooattach hello virtual machine's network interface cards tooupon failover.</span></span>

        PS C:\> $nw1 = Get-AzureRmVirtualNetwork -Name "FailoverNw" -ResourceGroupName "MyRG"

        PS C:\> $VMFriendlyName = "Fabrikam-App"

        PS C:\> $VM = Get-AzureRmSiteRecoveryVM -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName

        PS C:\> $UpdateJob = Set-AzureRmSiteRecoveryVM -VirtualMachine $VM -PrimaryNic $VM.NicDetailsList[0].NicId -RecoveryNetworkId $nw1.Id -RecoveryNicSubnetName $nw1.Subnets[0].Name

        PS C:\> $UpdateJob = Get-AzureRmSiteRecoveryJob -Job $UpdateJob

        PS C:\> $UpdateJob

        Name             : b8a647e0-2cb9-40d1-84c4-d0169919e2c5
        ID               : /Subscriptions/a731825f-4bf2-4f81-a611-c331b272206e/resourceGroups/MyRG/providers/Microsoft.RecoveryServices/vault
                           s/MyVault/replicationJobs/b8a647e0-2cb9-40d1-84c4-d0169919e2c5
        Type             : Microsoft.RecoveryServices/vaults/replicationJobs
        JobType          : UpdateVmProperties
        DisplayName      : Update hello virtual machine
        ClientRequestId  : 805a22a3-be86-441c-9da8-f32685673112-2015-12-10 17:55:51Z-P
        State            : Succeeded
        StateDescription : Completed
        StartTime        : 10-12-2015 17:55:53 +00:00
        EndTime          : 10-12-2015 17:55:54 +00:00
        TargetObjectId   : 289682c6-c5e6-42dc-a1d2-5f9621f78ae6
        TargetObjectType : ProtectionEntity
        TargetObjectName : Fabrikam-App
        AllowedActions   : {Restart}
        Tasks            : {UpdateVmPropertiesTask}
        Errors           : {}



## <a name="step-8-run-a-test-failover"></a><span data-ttu-id="c39d8-193">8. lépés: Feladatátvételi teszt futtatása</span><span class="sxs-lookup"><span data-stu-id="c39d8-193">Step 8: Run a test failover</span></span>
1. <span data-ttu-id="c39d8-194">A tesztfeladat feladatátvételt futtassa a következőképpen:</span><span class="sxs-lookup"><span data-stu-id="c39d8-194">Run a test failover job, as follows:</span></span>

        $nw = Get-AzureRmVirtualNetwork -Name "TestFailoverNw" -ResourceGroupName "MyRG" #Specify Azure vnet name and resource group

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMFriendlyName -ProtectionContainer $protectionContainer

        $TFjob = Start-AzureRmSiteRecoveryTestFailoverJob -ProtectionEntity $protectionEntity -Direction PrimaryToRecovery -AzureVMNetworkId $nw.Id
2. <span data-ttu-id="c39d8-195">Győződjön meg arról, hogy hello teszt virtuális gép létrehozása az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="c39d8-195">Verify that hello test VM is created in Azure.</span></span> <span data-ttu-id="c39d8-196">(hello tesztfeladat feladatátvételt fel van függesztve, az Azure-ban hello teszteléshez használt virtuális gép létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="c39d8-196">(hello test failover job is suspended, after creating hello test VM in Azure.</span></span> <span data-ttu-id="c39d8-197">hello feladat befejeződik által létrehozott hello vakpróbát hello feladat folytatása után törölje a hello következő lépésben ismertetett módon.)</span><span class="sxs-lookup"><span data-stu-id="c39d8-197">hello job completes by cleaning up hello created artefacts upon resuming hello job, as illustrated in hello next step.)</span></span>
3. <span data-ttu-id="c39d8-198">Teljes hello tesztelni, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="c39d8-198">Complete hello test failover, as follows:</span></span>

        $TFjob = Resume-AzureRmSiteRecoveryJob -Job $TFjob

## <a name="next-steps"></a><span data-ttu-id="c39d8-199">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c39d8-199">Next Steps</span></span>
<span data-ttu-id="c39d8-200">[További](https://msdn.microsoft.com/library/azure/mt637930.aspx) Azure Site Recovery Azure Resource Manager PowerShell-parancsmagokkal kapcsolatos.</span><span class="sxs-lookup"><span data-stu-id="c39d8-200">[Read more](https://msdn.microsoft.com/library/azure/mt637930.aspx) about Azure Site Recovery with Azure Resource Manager PowerShell cmdlets.</span></span>
