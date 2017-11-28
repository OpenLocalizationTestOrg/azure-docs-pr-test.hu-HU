---
title: "A PowerShell és az Azure Resource Manager a Hyper-V virtuális gépek replikálása |} Microsoft Docs"
description: "Automatizálhatja a replikálást a Hyper-V virtuális gépek Azure-bA az Azure Site Recovery PowerShell és az Azure Resource Manager használatával."
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
ms.openlocfilehash: dbd562b73b0caecd0feb993bd6fb796dddb0438c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="replicate-between-on-premises-hyper-v-virtual-machines-and-azure-by-using-powershell-and-azure-resource-manager"></a><span data-ttu-id="b4de2-103">PowerShell és az Azure Resource Manager használatával a helyszíni Hyper-V virtuális gépek és az Azure közötti replikáció</span><span class="sxs-lookup"><span data-stu-id="b4de2-103">Replicate between on-premises Hyper-V virtual machines and Azure by using PowerShell and Azure Resource Manager</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="b4de2-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b4de2-104">Azure Portal</span></span>](site-recovery-hyper-v-site-to-azure.md)
> * [<span data-ttu-id="b4de2-105">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="b4de2-105">PowerShell - Resource Manager</span></span>](site-recovery-deploy-with-powershell-resource-manager.md)
> * [<span data-ttu-id="b4de2-106">Klasszikus portál</span><span class="sxs-lookup"><span data-stu-id="b4de2-106">Classic Portal</span></span>](site-recovery-hyper-v-site-to-azure-classic.md)
>
>

## <a name="overview"></a><span data-ttu-id="b4de2-107">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="b4de2-107">Overview</span></span>
<span data-ttu-id="b4de2-108">Az Azure Site Recovery hozzájárul az üzleti folytonossági és vészhelyreállítási helyreállítási stratégia megvalósításában a replikáció, feladatátvétel és a különböző telepítési forgatókönyvek esetén a virtuális gépek helyreállítása.</span><span class="sxs-lookup"><span data-stu-id="b4de2-108">Azure Site Recovery contributes to your business continuity and disaster recovery strategy by orchestrating replication, failover, and recovery of virtual machines in a number of deployment scenarios.</span></span> <span data-ttu-id="b4de2-109">Központi telepítési forgatókönyvek teljes listáját lásd: a [Azure Site Recovery áttekintését](site-recovery-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b4de2-109">For a full list of deployment scenarios, see the [Azure Site Recovery overview](site-recovery-overview.md).</span></span>

<span data-ttu-id="b4de2-110">Az Azure PowerShell modul, amely kezelése a Windows PowerShell segítségével Azure-parancsmagokat kínál.</span><span class="sxs-lookup"><span data-stu-id="b4de2-110">Azure PowerShell is a module that provides cmdlets to manage Azure through Windows PowerShell.</span></span> <span data-ttu-id="b4de2-111">Együtt tudjon működni az kétféle modulok: az Azure-profil modul, vagy az Azure Resource Manager modul.</span><span class="sxs-lookup"><span data-stu-id="b4de2-111">It can work with two types of modules: the Azure Profile module, or the Azure Resource Manager module.</span></span>

<span data-ttu-id="b4de2-112">Site Recovery PowerShell parancsmagokat, elérhető az Azure PowerShell az Azure Resource Manager segítségével védelme és helyreállítása a kiszolgálók az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="b4de2-112">Site Recovery PowerShell cmdlets, available with Azure PowerShell for Azure Resource Manager, help you protect and recover your servers in Azure.</span></span>

<span data-ttu-id="b4de2-113">A cikkből megtudhatja, hogyan használhatja a Windows PowerShell, együtt Azure Resource Manager konfigurálását és levezényléséhez server védelme az Azure Site Recovery üzembe.</span><span class="sxs-lookup"><span data-stu-id="b4de2-113">This article describes how to use Windows PowerShell, together with Azure Resource Manager, to deploy Site Recovery to configure and orchestrate server protection to Azure.</span></span> <span data-ttu-id="b4de2-114">A példában ez a cikk bemutatja, hogyan védelme, a feladatátvétel, és az Azure-ba, a Hyper-V gazdagépen található virtuális gépek helyreállítása Azure PowerShell használatával az Azure Resource Manager eszközzel.</span><span class="sxs-lookup"><span data-stu-id="b4de2-114">The example used in this article shows you how to protect, fail over, and recover virtual machines on a Hyper-V host to Azure, by using Azure PowerShell with Azure Resource Manager.</span></span>

> [!NOTE]
> <span data-ttu-id="b4de2-115">A Site Recovery PowerShell parancsmagokat jelenleg lehetővé teszik a következő konfigurálását: egy a Virtual Machine Manager-helyet, amely egy másik, a Virtual Machine Manager helyet az Azure és a Hyper-V helyet az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="b4de2-115">The Site Recovery PowerShell cmdlets currently allow you to configure the following: one Virtual Machine Manager site to another, a Virtual Machine Manager site to Azure, and a Hyper-V site to Azure.</span></span>
>
>

<span data-ttu-id="b4de2-116">Nem kell lennie a PowerShell használatával Ez a cikk szakértői, de meg kell ismernie az alapszintű fogalmakkal, mint a modulok, a parancsmagok és a munkamenetek.</span><span class="sxs-lookup"><span data-stu-id="b4de2-116">You don't need to be a PowerShell expert to use this article, but you do need to understand the basic concepts, such as modules, cmdlets, and sessions.</span></span> <span data-ttu-id="b4de2-117">További információ a Windows PowerShellről: [Ismerkedés a Windows PowerShell-lel](http://technet.microsoft.com/library/hh857337.aspx).</span><span class="sxs-lookup"><span data-stu-id="b4de2-117">For more information about Windows PowerShell, see [Getting Started with Windows PowerShell](http://technet.microsoft.com/library/hh857337.aspx).</span></span>

<span data-ttu-id="b4de2-118">Akkor is olvashat további tudnivalókat [az Azure PowerShell használata Azure Resource Managerrel](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="b4de2-118">You can also read more about [Using Azure PowerShell with Azure Resource Manager](../powershell-azure-resource-manager.md).</span></span>

> [!NOTE]
> <span data-ttu-id="b4de2-119">A Cloud Solution Provider (CSP) program részét képező Microsoft-partnerek konfigurálható és kezelhető az ügyfelek megfelelő CSP előfizetések (bérlői előfizetések) az ügyfelek kiszolgálók védelmét.</span><span class="sxs-lookup"><span data-stu-id="b4de2-119">Microsoft partners that are part of the Cloud Solution Provider (CSP) program can configure and manage protection of their customers' servers to their customers' respective CSP subscriptions (tenant subscriptions).</span></span>
>
>

## <a name="before-you-start"></a><span data-ttu-id="b4de2-120">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="b4de2-120">Before you start</span></span>
<span data-ttu-id="b4de2-121">Győződjön meg arról, hogy az előfeltételek teljesülnek:</span><span class="sxs-lookup"><span data-stu-id="b4de2-121">Make sure you have these prerequisites in place:</span></span>

* <span data-ttu-id="b4de2-122">A [Microsoft Azure](https://azure.microsoft.com/) fiók.</span><span class="sxs-lookup"><span data-stu-id="b4de2-122">A [Microsoft Azure](https://azure.microsoft.com/) account.</span></span> <span data-ttu-id="b4de2-123">Kezdésként használhatja az [ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/) is.</span><span class="sxs-lookup"><span data-stu-id="b4de2-123">You can start with a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> <span data-ttu-id="b4de2-124">Emellett áttekintheti az [Azure Site Recovery Manager árképzési](https://azure.microsoft.com/pricing/details/site-recovery/).</span><span class="sxs-lookup"><span data-stu-id="b4de2-124">In addition, you can read about [Azure Site Recovery Manager pricing](https://azure.microsoft.com/pricing/details/site-recovery/).</span></span>
* <span data-ttu-id="b4de2-125">Azure PowerShell 1.0.</span><span class="sxs-lookup"><span data-stu-id="b4de2-125">Azure PowerShell 1.0.</span></span> <span data-ttu-id="b4de2-126">Ebben a kiadásban és telepítéséről kapcsolatos információkért lásd: [Azure PowerShell 1.0.](https://azure.microsoft.com/)</span><span class="sxs-lookup"><span data-stu-id="b4de2-126">For information about this release and how to install it, see [Azure PowerShell 1.0.](https://azure.microsoft.com/)</span></span>
* <span data-ttu-id="b4de2-127">A [AzureRM.SiteRecovery](https://www.powershellgallery.com/packages/AzureRM.SiteRecovery/) és [AzureRM.RecoveryServices](https://www.powershellgallery.com/packages/AzureRM.RecoveryServices/) modulok.</span><span class="sxs-lookup"><span data-stu-id="b4de2-127">The [AzureRM.SiteRecovery](https://www.powershellgallery.com/packages/AzureRM.SiteRecovery/) and [AzureRM.RecoveryServices](https://www.powershellgallery.com/packages/AzureRM.RecoveryServices/) modules.</span></span> <span data-ttu-id="b4de2-128">Ezek a modulok a legújabb verziói kaphat a [PowerShell-galériában](https://www.powershellgallery.com/)</span><span class="sxs-lookup"><span data-stu-id="b4de2-128">You can get the latest versions of these modules from the [PowerShell gallery](https://www.powershellgallery.com/)</span></span>

<span data-ttu-id="b4de2-129">Ez a cikk bemutatja, hogyan használhatja az Azure Powershellt Azure Resource Manager konfigurálását és kezelését a kiszolgálók védelmét.</span><span class="sxs-lookup"><span data-stu-id="b4de2-129">This article illustrates how to use Azure Powershell with Azure Resource Manager to configure and manage protection of your servers.</span></span> <span data-ttu-id="b4de2-130">A példában ez a cikk bemutatja, hogyan védi meg egy olyan virtuális géphez, a Hyper-V gazdagépen, az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="b4de2-130">The example used in this article shows you how to protect a virtual machine, running on a Hyper-V host, to Azure.</span></span> <span data-ttu-id="b4de2-131">Ebben a példában a következő előfeltételek vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="b4de2-131">The following prerequisites are specific to this example.</span></span> <span data-ttu-id="b4de2-132">Átfogóbb bizonyos követelmények a Site Recovery több, különböző esetekre tekintse meg a forgatókönyv vonatkozó dokumentációt.</span><span class="sxs-lookup"><span data-stu-id="b4de2-132">For a more comprehensive set of requirements for the various Site Recovery scenarios, refer to the documentation pertaining to that scenario.</span></span>

* <span data-ttu-id="b4de2-133">A Hyper-V gazdagépen fut a Windows Server 2012 R2 vagy Microsoft Hyper-V Server 2012 R2 tartalmazó egy vagy több virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="b4de2-133">A Hyper-V host running Windows Server 2012 R2 or Microsoft Hyper-V Server 2012 R2 containing one or more virtual machines.</span></span>
* <span data-ttu-id="b4de2-134">Hyper-V kiszolgálók csatlakozik az internethez, közvetlen vagy proxyn keresztül.</span><span class="sxs-lookup"><span data-stu-id="b4de2-134">Hyper-V servers connected to the Internet, either directly or through a proxy.</span></span>
* <span data-ttu-id="b4de2-135">A védeni kívánt virtuális gépek meg kell felelnie az [a virtuális gép előfeltételei](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).</span><span class="sxs-lookup"><span data-stu-id="b4de2-135">The virtual machines you want to protect should conform with [Virtual Machine prerequisites](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).</span></span>

## <a name="step-1-sign-in-to-your-azure-account"></a><span data-ttu-id="b4de2-136">1. lépés: Bejelentkezés az Azure-fiókjával</span><span class="sxs-lookup"><span data-stu-id="b4de2-136">Step 1: Sign in to your Azure account</span></span>
1. <span data-ttu-id="b4de2-137">Nyisson meg egy PowerShell-konzolt, és futtassa ezt a parancsot az Azure-fiókjával bejelentkezhet.</span><span class="sxs-lookup"><span data-stu-id="b4de2-137">Open a PowerShell console and run this command to sign in to your Azure account.</span></span> <span data-ttu-id="b4de2-138">A parancsmag megnyitása egy weblap, amelyen kérni fogja a hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="b4de2-138">The cmdlet brings up a web page that will prompt you for your account credentials.</span></span>

        Login-AzureRmAccount

    <span data-ttu-id="b4de2-139">Alternatív megoldásként akkor is lehetnek a fiók hitelesítő adataival paramétereként a `Login-AzureRmAccount` parancsmag használatával a `-Credential` paraméter.</span><span class="sxs-lookup"><span data-stu-id="b4de2-139">Alternately, you could also include your account credentials as a parameter to the `Login-AzureRmAccount` cmdlet, by using the `-Credential` parameter.</span></span>

    <span data-ttu-id="b4de2-140">Ha működik a bérlő nevében CSP partner, adja meg az ügyfél a bérlők a tenantID vagy bérlői elsődleges tartománynév használatával.</span><span class="sxs-lookup"><span data-stu-id="b4de2-140">If you are CSP partner working on behalf of a tenant, specify the customer as a tenant, by using their tenantID or tenant primary domain name.</span></span>

        Login-AzureRmAccount -Tenant "fabrikam.com"
2. <span data-ttu-id="b4de2-141">Egy fiók több előfizetések van így társítania kell a fiókhoz használni kívánt előfizetést.</span><span class="sxs-lookup"><span data-stu-id="b4de2-141">An account can have several subscriptions, so you should associate the subscription you want to use with the account.</span></span>

        Select-AzureRmSubscription -SubscriptionName $SubscriptionName
3. <span data-ttu-id="b4de2-142">Győződjön meg arról, hogy az előfizetés regisztrálva van-e az Azure szolgáltatók használatához a Recovery Services és a hely helyreállítását követően a következő parancsokkal:</span><span class="sxs-lookup"><span data-stu-id="b4de2-142">Verify that your subscription is registered to use the Azure providers for Recovery Services and Site Recovery, by using the following commands:</span></span>

   * `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices`
   * `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`

   <span data-ttu-id="b4de2-143">A kimenet parancsok Ha a **RegistrationState** értéke **regisztrált**, folytathatja a 2. lépés.</span><span class="sxs-lookup"><span data-stu-id="b4de2-143">In the output of these commands, if the **RegistrationState** is set to **Registered**, you can proceed to Step 2.</span></span> <span data-ttu-id="b4de2-144">Ha nem, a hiányzó szolgáltató regisztrálnia kell az előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="b4de2-144">If not, you should register the missing provider in your subscription.</span></span>

   <span data-ttu-id="b4de2-145">Az Azure-szolgáltató regisztrálása a Site Recovery és helyreállítási szolgáltatásokhoz, a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="b4de2-145">To register the Azure provider for Site Recovery and Recovery Services, run the following commands:</span></span>

       Register-AzureRmResourceProvider -ProviderNamespace Microsoft.SiteRecovery
       Register-AzureRmResourceProvider -ProviderNamespace Microsoft.RecoveryServices

   <span data-ttu-id="b4de2-146">Győződjön meg arról, hogy a szolgáltatók regisztrálása sikeresen befejeződött a következő parancsokkal: `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices` és `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`.</span><span class="sxs-lookup"><span data-stu-id="b4de2-146">Verify that the providers registered successfully by using the following commands: `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices` and `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`.</span></span>

## <a name="step-2-set-up-the-recovery-services-vault"></a><span data-ttu-id="b4de2-147">2. lépés: A Recovery Services-tároló beállítása</span><span class="sxs-lookup"><span data-stu-id="b4de2-147">Step 2: Set up the Recovery Services vault</span></span>
1. <span data-ttu-id="b4de2-148">Hozzon létre egy Azure Resource Manager-csoportot, amelyben meg kell hozzon létre a tárolót, vagy használjon egy meglévő erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="b4de2-148">Create an Azure Resource Manager resource group in which you'll create the vault, or use an existing resource group.</span></span> <span data-ttu-id="b4de2-149">Új erőforráscsoport létrehozásához a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="b4de2-149">You can create a new resource group by using the following command:</span></span>

        New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Geo  

    <span data-ttu-id="b4de2-150">ahol a $ResourceGroupName változó szeretne létrehozni az erőforráscsoport nevét tartalmazza, és a $Geo változó tartalmazza az Azure-régió, amihez létre kívánja hozni az erőforráscsoportot (például "Dél-Brazília") a.</span><span class="sxs-lookup"><span data-stu-id="b4de2-150">where the $ResourceGroupName variable contains the name of the resource group you want to create, and the $Geo variable contains the Azure region in which to create the resource group (for example, "Brazil South").</span></span>

    <span data-ttu-id="b4de2-151">Ezt úgy szerezheti be az erőforráscsoportok listáját az előfizetésében használatával a `Get-AzureRmResourceGroup` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="b4de2-151">You can obtain a list of resource groups in your subscription by using the `Get-AzureRmResourceGroup` cmdlet.</span></span>
2. <span data-ttu-id="b4de2-152">Hozzon létre egy új Azure Recovery Services-tároló az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="b4de2-152">Create a new Azure Recovery Services vault as follows:</span></span>

        $vault = New-AzureRmRecoveryServicesVault -Name <string> -ResourceGroupName <string> -Location <string>

    <span data-ttu-id="b4de2-153">Meglévő tárolók listája használatával kérheti le a `Get-AzureRmRecoveryServicesVault` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="b4de2-153">You can retrieve a list of existing vaults by using the `Get-AzureRmRecoveryServicesVault` cmdlet.</span></span>

> [!NOTE]
> <span data-ttu-id="b4de2-154">Ha a klasszikus portálon vagy az Azure Service Management PowerShell-modul segítségével létrehozni a Site Recovery-tárolóhoz a műveletek végrehajtásához, használatával kérheti le az ilyen tárolók listája az `Get-AzureRmSiteRecoveryVault` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="b4de2-154">If you wish to perform operations on Site Recovery vaults created using the classic portal or the Azure Service Management PowerShell module, you can retrieve a list of such vaults by using the `Get-AzureRmSiteRecoveryVault` cmdlet.</span></span> <span data-ttu-id="b4de2-155">Hozzon létre egy új Recovery Services-tároló minden új műveletnél.</span><span class="sxs-lookup"><span data-stu-id="b4de2-155">You should create a new Recovery Services vault for all new operations.</span></span> <span data-ttu-id="b4de2-156">A Site Recovery-tárolóhoz, korábban létrehozott támogatottak, de nem rendelkezik a legújabb funkciókat.</span><span class="sxs-lookup"><span data-stu-id="b4de2-156">The Site Recovery vaults you've created earlier are supported, but don't have the latest features.</span></span>
>
>

## <a name="step-3-set-the-recovery-services-vault-context"></a><span data-ttu-id="b4de2-157">3. lépés: A Recovery Services-tároló környezet beállítása</span><span class="sxs-lookup"><span data-stu-id="b4de2-157">Step 3: Set the Recovery Services vault context</span></span>
1. <span data-ttu-id="b4de2-158">A tároló környezet beállítása a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="b4de2-158">Set the vault context by running the following command:</span></span>

       Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="step-4-create-a-hyper-v-site-and-generate-a-new-vault-registration-key-for-the-site"></a><span data-ttu-id="b4de2-159">4. lépés: Hyper-V-hely létrehozása, és hozzon létre egy új tároló regisztrációs kulcsot a hely.</span><span class="sxs-lookup"><span data-stu-id="b4de2-159">Step 4: Create a Hyper-V site and generate a new vault registration key for the site.</span></span>
1. <span data-ttu-id="b4de2-160">Hozzon létre egy új Hyper-V hely az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="b4de2-160">Create a new Hyper-V site as follows:</span></span>

        $sitename = "MySite"                #Specify site friendly name
        New-AzureRmSiteRecoverySite -Name $sitename

    <span data-ttu-id="b4de2-161">Ez a parancsmag elindítja a Site Recovery feladatot, a webhely létrehozása, és a Site Recovery feladat objektum beállítása/beolvasása.</span><span class="sxs-lookup"><span data-stu-id="b4de2-161">This cmdlet starts a Site Recovery job to create the site, and returns a Site Recovery job object.</span></span> <span data-ttu-id="b4de2-162">Várjon, amíg a feladat befejeződését, és ellenőrizze, hogy a feladat sikeresen befejeződött.</span><span class="sxs-lookup"><span data-stu-id="b4de2-162">Wait for the job to complete and verify that the job completed successfully.</span></span>

    <span data-ttu-id="b4de2-163">Kérje le a feladat objektumot, és ezáltal a feladat aktuális állapotának ellenőrzése a Get-AzureRmSiteRecoveryJob parancsmag használatával.</span><span class="sxs-lookup"><span data-stu-id="b4de2-163">You can retrieve the job object, and thereby check the current status of the job, by using the Get-AzureRmSiteRecoveryJob cmdlet.</span></span>
2. <span data-ttu-id="b4de2-164">Létrehozni, és töltse le a hely egy regisztrációs kulcsot az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="b4de2-164">Generate and download a registration key for the site, as follows:</span></span>

        $SiteIdentifier = Get-AzureRmSiteRecoverySite -Name $sitename | Select -ExpandProperty SiteIdentifier
        Get-AzureRmRecoveryServicesVaultSettingsFile -Vault $vault -SiteIdentifier $SiteIdentifier -SiteFriendlyName $sitename -Path $Path

    <span data-ttu-id="b4de2-165">Másolja a letöltött kulcs a Hyper-V gazdagépen.</span><span class="sxs-lookup"><span data-stu-id="b4de2-165">Copy the downloaded key to the Hyper-V host.</span></span> <span data-ttu-id="b4de2-166">A Hyper-V gazdagép, a hely regisztrálja a kulccsal van szüksége.</span><span class="sxs-lookup"><span data-stu-id="b4de2-166">You need the key to register the Hyper-V host to the site.</span></span>

## <a name="step-5-install-the-azure-site-recovery-provider-and-azure-recovery-services-agent-on-your-hyper-v-host"></a><span data-ttu-id="b4de2-167">5. lépés: Az Azure Site Recovery provider és az Azure Recovery Services Agent telepítését a Hyper-V gazdagépen</span><span class="sxs-lookup"><span data-stu-id="b4de2-167">Step 5: Install the Azure Site Recovery provider and Azure Recovery Services Agent on your Hyper-V host</span></span>
1. <span data-ttu-id="b4de2-168">A telepítő a szolgáltató legújabb verziójának letöltése [Microsoft](https://aka.ms/downloaddra).</span><span class="sxs-lookup"><span data-stu-id="b4de2-168">Download the installer for the latest version of the provider from [Microsoft](https://aka.ms/downloaddra).</span></span>
2. <span data-ttu-id="b4de2-169">A regisztrációs lépésében továbbra is futtatható a telepítő a Hyper-V gazdagépen, és a telepítés végén.</span><span class="sxs-lookup"><span data-stu-id="b4de2-169">Run the installer on your Hyper-V host, and at the end of the installation continue to the registration step.</span></span>
3. <span data-ttu-id="b4de2-170">Amikor a rendszer kéri, adja meg a letöltött hely regisztrációs kulcsot, és a Hyper-V gazdagép a hely teljes regisztrálását.</span><span class="sxs-lookup"><span data-stu-id="b4de2-170">When prompted, provide the downloaded site registration key, and complete registration of the Hyper-V host to the site.</span></span>
4. <span data-ttu-id="b4de2-171">Győződjön meg arról, hogy a Hyper-V gazdagép regisztrálva van a helyhez a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="b4de2-171">Verify that the Hyper-V host is registered to the site by using the following command:</span></span>

        $server =  Get-AzureRmSiteRecoveryServer -FriendlyName $server-friendlyname

## <a name="step-6-create-a-replication-policy-and-associate-it-with-the-protection-container"></a><span data-ttu-id="b4de2-172">6. lépés: Hozzon létre egy replikációs házirendet, és rendelje hozzá azt a védelmi tároló</span><span class="sxs-lookup"><span data-stu-id="b4de2-172">Step 6: Create a replication policy and associate it with the protection container</span></span>
1. <span data-ttu-id="b4de2-173">Egy replikációs házirend létrehozásához az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="b4de2-173">Create a replication policy as follows:</span></span>

        $ReplicationFrequencyInSeconds = "300";        #options are 30,300,900
        $PolicyName = “replicapolicy”
        $Recoverypoints = 6                    #specify the number of recovery points
        $storageaccountID = Get-AzureRmStorageAccount -Name "mystorea" -ResourceGroupName "MyRG" | Select -ExpandProperty Id

        $PolicyResult = New-AzureRmSiteRecoveryPolicy -Name $PolicyName -ReplicationProvider “HyperVReplicaAzure” -ReplicationFrequencyInSeconds $ReplicationFrequencyInSeconds  -RecoveryPoints $Recoverypoints -ApplicationConsistentSnapshotFrequencyInHours 1 -RecoveryAzureStorageAccountId $storageaccountID

    <span data-ttu-id="b4de2-174">Ellenőrizze a visszaadott feladatot annak érdekében, hogy a replikációs házirend létrehozása sikeres.</span><span class="sxs-lookup"><span data-stu-id="b4de2-174">Check the returned job to ensure that the replication policy creation succeeds.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="b4de2-175">A megadott tárfiók ugyanabban a régióban Azure és a Recovery Services-tárolónak kell lennie, és rendelkeznie kell a georeplikáció engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="b4de2-175">The storage account specified should be in the same Azure region as your Recovery Services vault, and should have geo-replication enabled.</span></span>
   >
   > * <span data-ttu-id="b4de2-176">Ha a megadott helyreállítási tárfiók típusú Azure Storage (klasszikus), a feladatátvétel a védett gépek Azure IaaS (klasszikus) a gép állítható helyre.</span><span class="sxs-lookup"><span data-stu-id="b4de2-176">If the specified Recovery storage account is of type Azure Storage (Classic), failover of the protected machines recover the machine to Azure IaaS (Classic).</span></span>
   > * <span data-ttu-id="b4de2-177">Ha a megadott helyreállítási tárfiók típusú Azure Storage (az Azure Resource Manager), a védett gépek feladatátvétele Azure IaaS (Azure Resource Manager) a gép állítható helyre.</span><span class="sxs-lookup"><span data-stu-id="b4de2-177">If the specified Recovery storage account is of type Azure Storage (Azure Resource Manager), failover of the protected machines recover the machine to Azure IaaS (Azure Resource Manager).</span></span>
   >
   >
2. <span data-ttu-id="b4de2-178">Töltse le a védelmi tároló megfelelő a helyhez, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="b4de2-178">Get the protection container corresponding to the site, as follows:</span></span>

        $protectionContainer = Get-AzureRmSiteRecoveryProtectionContainer
3. <span data-ttu-id="b4de2-179">Indítsa el a társítást a védelmi tároló a replikációs házirend a következőképpen:</span><span class="sxs-lookup"><span data-stu-id="b4de2-179">Start the association of the protection container with the replication policy, as follows:</span></span>

     <span data-ttu-id="b4de2-180">$Policy = get-AzureRmSiteRecoveryPolicy - FriendlyName $PolicyName $associationJob = a Start-AzureRmSiteRecoveryPolicyAssociationJob-házirend $Policy - PrimaryProtectionContainer $protectionContainer</span><span class="sxs-lookup"><span data-stu-id="b4de2-180">$Policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $PolicyName   $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy $Policy -PrimaryProtectionContainer $protectionContainer</span></span>

   <span data-ttu-id="b4de2-181">Várjon, amíg a társítás feladat befejeződik, és győződjön meg arról, hogy sikeresen befejeződött-e.</span><span class="sxs-lookup"><span data-stu-id="b4de2-181">Wait for the association job to complete, and ensure that it completed successfully.</span></span>

## <a name="step-7-enable-protection-for-virtual-machines"></a><span data-ttu-id="b4de2-182">7. lépés: Virtuális gépek védelmének engedélyezése</span><span class="sxs-lookup"><span data-stu-id="b4de2-182">Step 7: Enable protection for virtual machines</span></span>
1. <span data-ttu-id="b4de2-183">Töltse le a megfelelő módon védeni kívánt virtuális gép védelmi entitás:</span><span class="sxs-lookup"><span data-stu-id="b4de2-183">Get the protection entity corresponding to the VM you want to protect, as follows:</span></span>

        $VMFriendlyName = "Fabrikam-app"                    #Name of the VM
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName
2. <span data-ttu-id="b4de2-184">Indítsa el a virtuális gép védelmét az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="b4de2-184">Start protecting the virtual machine, as follows:</span></span>

        $Ostype = "Windows"                                 # "Windows" or "Linux"
        $DRjob = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionEntity -Policy $Policy -Protection Enable -RecoveryAzureStorageAccountId $storageaccountID  -OS $OStype -OSDiskName $protectionEntity.Disks[0].Name

   > [!IMPORTANT]
   > <span data-ttu-id="b4de2-185">A megadott tárfiók ugyanabban a régióban Azure és a Recovery Services-tárolónak kell lennie, és rendelkeznie kell a georeplikáció engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="b4de2-185">The storage account specified should be in the same Azure region as your Recovery Services vault, and should have geo-replication enabled.</span></span>
   >
   > * <span data-ttu-id="b4de2-186">Ha a megadott helyreállítási tárfiók típusú Azure Storage (klasszikus), a feladatátvétel a védett gépek Azure IaaS (klasszikus) a gép állítható helyre.</span><span class="sxs-lookup"><span data-stu-id="b4de2-186">If the specified Recovery storage account is of type Azure Storage (Classic), failover of the protected machines recover the machine to Azure IaaS (Classic).</span></span>
   > * <span data-ttu-id="b4de2-187">Ha a megadott helyreállítási tárfiók típusú Azure Storage (az Azure Resource Manager), a védett gépek feladatátvétele Azure IaaS (Azure Resource Manager) a gép állítható helyre.</span><span class="sxs-lookup"><span data-stu-id="b4de2-187">If the specified Recovery storage account is of type Azure Storage (Azure Resource Manager), failover of the protected machines recover the machine to Azure IaaS (Azure Resource Manager).</span></span>
   >
   > <span data-ttu-id="b4de2-188">Ha a védett virtuális gép nem csatlakoztatható egynél több lemez, adja meg az operációsrendszer-lemez használatával a *OSDiskName* paraméter.</span><span class="sxs-lookup"><span data-stu-id="b4de2-188">If the VM you are protecting has more than one disk attached to it, specify the operating system disk by using the *OSDiskName* parameter.</span></span>
   >
   >
3. <span data-ttu-id="b4de2-189">Várjon, amíg a kezdeti replikálás után a védett állapotot elérni a virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="b4de2-189">Wait for the virtual machines to reach a protected state after the initial replication.</span></span> <span data-ttu-id="b4de2-190">Ez eltarthat egy ideig, például replikálni adatok mennyisége és az Azure-bA fölérendelt rendelkezésre álló sávszélességet tényezőktől függően.</span><span class="sxs-lookup"><span data-stu-id="b4de2-190">This can take a while, depending on factors such as the amount of data to be replicated and the available upstream bandwidth to Azure.</span></span> <span data-ttu-id="b4de2-191">A feladat állapotát és StateDescription frissítve lett, az alábbiak szerint a virtuális gép védett állapotban elérése után.</span><span class="sxs-lookup"><span data-stu-id="b4de2-191">The job State and StateDescription are updated as follows, upon the VM reaching a protected state.</span></span>

        PS C:\> $DRjob = Get-AzureRmSiteRecoveryJob -Job $DRjob

        PS C:\> $DRjob | Select-Object -ExpandProperty State
        Succeeded

        PS C:\> $DRjob | Select-Object -ExpandProperty StateDescription
        Completed
4. <span data-ttu-id="b4de2-192">Frissítse a helyreállítási tulajdonságait, például a Virtuálisgép-szerepkör méretéhez és csatolni a virtuális gép hálózati kártya feladatátvétel esetén az Azure-hálózatot.</span><span class="sxs-lookup"><span data-stu-id="b4de2-192">Update recovery properties, such as the VM role size, and the Azure network to attach the virtual machine's network interface cards to upon failover.</span></span>

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
        DisplayName      : Update the virtual machine
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



## <a name="step-8-run-a-test-failover"></a><span data-ttu-id="b4de2-193">8. lépés: Feladatátvételi teszt futtatása</span><span class="sxs-lookup"><span data-stu-id="b4de2-193">Step 8: Run a test failover</span></span>
1. <span data-ttu-id="b4de2-194">A tesztfeladat feladatátvételt futtassa a következőképpen:</span><span class="sxs-lookup"><span data-stu-id="b4de2-194">Run a test failover job, as follows:</span></span>

        $nw = Get-AzureRmVirtualNetwork -Name "TestFailoverNw" -ResourceGroupName "MyRG" #Specify Azure vnet name and resource group

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMFriendlyName -ProtectionContainer $protectionContainer

        $TFjob = Start-AzureRmSiteRecoveryTestFailoverJob -ProtectionEntity $protectionEntity -Direction PrimaryToRecovery -AzureVMNetworkId $nw.Id
2. <span data-ttu-id="b4de2-195">Győződjön meg arról, hogy a teszt virtuális gép létrehozása az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="b4de2-195">Verify that the test VM is created in Azure.</span></span> <span data-ttu-id="b4de2-196">(A teszt feladatátvételi feladatot fel van függesztve, az Azure-ban a teszteléshez használt virtuális gép létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="b4de2-196">(The test failover job is suspended, after creating the test VM in Azure.</span></span> <span data-ttu-id="b4de2-197">A feladat be visszakapcsolását a feladat létrehozott vakpróbát törléséről a következő lépésben ismertetett módon.)</span><span class="sxs-lookup"><span data-stu-id="b4de2-197">The job completes by cleaning up the created artefacts upon resuming the job, as illustrated in the next step.)</span></span>
3. <span data-ttu-id="b4de2-198">Végezze el a feladatátvételi tesztet, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="b4de2-198">Complete the test failover, as follows:</span></span>

        $TFjob = Resume-AzureRmSiteRecoveryJob -Job $TFjob

## <a name="next-steps"></a><span data-ttu-id="b4de2-199">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b4de2-199">Next Steps</span></span>
<span data-ttu-id="b4de2-200">[További](https://msdn.microsoft.com/library/azure/mt637930.aspx) Azure Site Recovery Azure Resource Manager PowerShell-parancsmagokkal kapcsolatos.</span><span class="sxs-lookup"><span data-stu-id="b4de2-200">[Read more](https://msdn.microsoft.com/library/azure/mt637930.aspx) about Azure Site Recovery with Azure Resource Manager PowerShell cmdlets.</span></span>
