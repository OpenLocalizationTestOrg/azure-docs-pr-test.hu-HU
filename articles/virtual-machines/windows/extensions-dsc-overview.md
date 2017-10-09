---
title: "Állapotkonfiguráció Azure áttekintés aaaDesired |} Microsoft Docs"
description: "Hello Microsoft Azure-kiterjesztés kezelője segítségével a PowerShell célállapot-konfiguráció áttekintése. Többek között az Előfeltételek, architektúra, parancsmagok..."
services: virtual-machines-windows
documentationcenter: 
author: zjalexander
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
keywords: 
ms.assetid: bbacbc93-1e7b-4611-a3ec-e3320641f9ba
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 01/09/2017
ms.author: zachal
ms.openlocfilehash: b0337a2f1124f35e5e40c1478bd7530427e59d44
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toohello-azure-desired-state-configuration-extension-handler"></a><span data-ttu-id="65b8b-104">Bevezetés toohello Azure célállapot-konfiguráció bővítmény kezelő</span><span class="sxs-lookup"><span data-stu-id="65b8b-104">Introduction toohello Azure Desired State Configuration extension handler</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="65b8b-105">hello Azure virtuális gép ügynökének és kapcsolódó bővítmények a Microsoft Azure infrastruktúra-szolgáltatásokat hello részét képezik.</span><span class="sxs-lookup"><span data-stu-id="65b8b-105">hello Azure VM Agent and associated Extensions are part of hello Microsoft Azure Infrastructure Services.</span></span> <span data-ttu-id="65b8b-106">Virtuálisgép-bővítmények olyan szoftverösszetevők, amelyek hello VM bővíthetők, és egyszerűbbé tehető a virtuális gép különböző felügyeleti műveleteket.</span><span class="sxs-lookup"><span data-stu-id="65b8b-106">VM Extensions are software components that extend hello VM functionality and simplify various VM management operations.</span></span> <span data-ttu-id="65b8b-107">Például hello VMAccess bővítmény használt tooreset a rendszergazda jelszavát, és egyéni parancsfájl hello kiterjesztése használt tooexecute egy parancsfájlt a virtuális gép hello lehet.</span><span class="sxs-lookup"><span data-stu-id="65b8b-107">For example, hello VMAccess extension can be used tooreset an administrator's password, or hello Custom Script extension can be used tooexecute a script on hello VM.</span></span>

<span data-ttu-id="65b8b-108">Ez a cikk hello PowerShell kívánt állapot konfigurációs szolgáltatása (DSC) bővítmény be Azure virtuális gépek hello Azure PowerShell SDK részeként.</span><span class="sxs-lookup"><span data-stu-id="65b8b-108">This article introduces hello PowerShell Desired State Configuration (DSC) Extension for Azure VMs as part of hello Azure PowerShell SDK.</span></span> <span data-ttu-id="65b8b-109">Új parancsmagok tooupload használja, és a PowerShell DSC-konfiguráció alkalmazása egy Azure virtuális gépen engedélyezve van a PowerShell DSC-bővítményt hello.</span><span class="sxs-lookup"><span data-stu-id="65b8b-109">You can use new cmdlets tooupload and apply a PowerShell DSC configuration on an Azure VM enabled with hello PowerShell DSC extension.</span></span> <span data-ttu-id="65b8b-110">hello PowerShell DSC bővítmény hívások a PowerShell DSC tooenact hello DSC-konfiguráció a virtuális gép hello kapott.</span><span class="sxs-lookup"><span data-stu-id="65b8b-110">hello PowerShell DSC extension calls into PowerShell DSC tooenact hello received DSC configuration on hello VM.</span></span> <span data-ttu-id="65b8b-111">Ez a funkció hello Azure-portálon keresztül is érhető el.</span><span class="sxs-lookup"><span data-stu-id="65b8b-111">This functionality is also available through hello Azure portal.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="65b8b-112">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="65b8b-112">Prerequisites</span></span>
<span data-ttu-id="65b8b-113">**Helyi számítógép** toointeract a hello Azure Virtuálisgép-bővítmény, Azure PowerShell SDK hello vagy szükséges toouse vagy hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="65b8b-113">**Local machine** toointeract with hello Azure VM extension, you need toouse either hello Azure portal or hello Azure PowerShell SDK.</span></span> 

<span data-ttu-id="65b8b-114">**A Vendégügynök** hello Azure virtuális gép hello DSC-konfiguráció által konfigurált toobe egy operációs rendszer, amely támogatja a Windows Management Framework (WMF) 4.0-s vagy 5.0 kell.</span><span class="sxs-lookup"><span data-stu-id="65b8b-114">**Guest Agent** hello Azure VM that is configured by hello DSC configuration needs toobe an OS that supports either Windows Management Framework (WMF) 4.0 or 5.0.</span></span> <span data-ttu-id="65b8b-115">hello támogatott operációsrendszer-verziók teljes listája megtalálható hello [DSC-bővítmény verzióelőzmények](https://blogs.msdn.microsoft.com/powershell/2014/11/20/release-history-for-the-azure-dsc-extension/).</span><span class="sxs-lookup"><span data-stu-id="65b8b-115">hello full list of supported OS versions can be found at hello [DSC Extension Version History](https://blogs.msdn.microsoft.com/powershell/2014/11/20/release-history-for-the-azure-dsc-extension/).</span></span>

## <a name="terms-and-concepts"></a><span data-ttu-id="65b8b-116">Kifejezések és fogalmak</span><span class="sxs-lookup"><span data-stu-id="65b8b-116">Terms and concepts</span></span>
<span data-ttu-id="65b8b-117">Ez az útmutató a következő fogalmak hello ismeretét feltételezi:</span><span class="sxs-lookup"><span data-stu-id="65b8b-117">This guide presumes familiarity with hello following concepts:</span></span>

<span data-ttu-id="65b8b-118">Konfiguráció - DSC konfigurációs dokumentum.</span><span class="sxs-lookup"><span data-stu-id="65b8b-118">Configuration - A DSC configuration document.</span></span> 

<span data-ttu-id="65b8b-119">Csomópont - céljának DSC-konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="65b8b-119">Node - A target for a DSC configuration.</span></span> <span data-ttu-id="65b8b-120">Ebben a dokumentumban a "csomópont" mindig tooan Azure virtuális gép hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="65b8b-120">In this document, "node" always refers tooan Azure VM.</span></span>

<span data-ttu-id="65b8b-121">Konfigurációs adatok - egy .psd1 fájlt a környezeti adatokat tartalmazó konfiguráció</span><span class="sxs-lookup"><span data-stu-id="65b8b-121">Configuration Data - A .psd1 file containing environmental data for a configuration</span></span>

## <a name="architectural-overview"></a><span data-ttu-id="65b8b-122">Az architektúra áttekintése</span><span class="sxs-lookup"><span data-stu-id="65b8b-122">Architectural overview</span></span>
<span data-ttu-id="65b8b-123">hello Azure DSC-bővítményt használ hello Azure Virtuálisgép-ügynök keretrendszer toodeliver, kihirdeti, és a jelentés az Azure virtuális gépeken futó DSC-konfigurációk.</span><span class="sxs-lookup"><span data-stu-id="65b8b-123">hello Azure DSC extension uses hello Azure VM Agent framework toodeliver, enact, and report on DSC configurations running on Azure VMs.</span></span> <span data-ttu-id="65b8b-124">hello DSC-bővítményt vár legalább egy konfigurációs dokumentumot tartalmazó .zip fájlt, és a paraméterek megadott hello Azure PowerShell SDK vagy keresztül hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="65b8b-124">hello DSC extension expects a .zip file containing at least a configuration document, and a set of parameters provided either through hello Azure PowerShell SDK or through hello Azure portal.</span></span>

<span data-ttu-id="65b8b-125">Meghívásakor hello bővítmény hello az első alkalommal fut a telepítési művelet.</span><span class="sxs-lookup"><span data-stu-id="65b8b-125">When hello extension is called for hello first time, it runs an installation process.</span></span> <span data-ttu-id="65b8b-126">Ez a folyamat a következő logika hello segítségével hello Windows Management Framework (WMF) verzióját telepíti:</span><span class="sxs-lookup"><span data-stu-id="65b8b-126">This process installs a version of hello Windows Management Framework (WMF) using hello following logic:</span></span>

1. <span data-ttu-id="65b8b-127">Ha hello Azure virtuális gép operációs rendszere Windows Server 2016-os, semmilyen művelet nem lesz végrehajtva.</span><span class="sxs-lookup"><span data-stu-id="65b8b-127">If hello Azure VM OS is Windows Server 2016, no action is taken.</span></span> <span data-ttu-id="65b8b-128">Windows Server 2016 már telepített PowerShell hello legújabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="65b8b-128">Windows Server 2016 already has hello latest version of PowerShell installed.</span></span>
2. <span data-ttu-id="65b8b-129">Ha hello `wmfVersion` tulajdonság van megadva, a hello WMF verziója van telepítve, kivéve, ha hello virtuális gép operációs rendszere kompatibilis.</span><span class="sxs-lookup"><span data-stu-id="65b8b-129">If hello `wmfVersion` property is specified, that version of hello WMF is installed unless it is incompatible with hello VM's OS.</span></span>
3. <span data-ttu-id="65b8b-130">Ha nincs `wmfVersion` tulajdonság van megadva, a hello legújabb megfelelő verziója hello WMF telepítve van.</span><span class="sxs-lookup"><span data-stu-id="65b8b-130">If no `wmfVersion` property is specified, hello latest applicable version of hello WMF is installed.</span></span>

<span data-ttu-id="65b8b-131">Hello WMF telepítése újraindítást igényel.</span><span class="sxs-lookup"><span data-stu-id="65b8b-131">Installation of hello WMF requires a reboot.</span></span> <span data-ttu-id="65b8b-132">Az újraindítást követően hello bővítmény letölti a hello hello .zip fájl `modulesUrl` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="65b8b-132">After reboot, hello extension downloads hello .zip file specified in hello `modulesUrl` property.</span></span> <span data-ttu-id="65b8b-133">Ha ezen a helyen az Azure blob Storage tárolóban, SAS-jogkivonat adható hello `sasToken` tulajdonság tooaccess hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="65b8b-133">If this location is in Azure blob storage, a SAS token can be specified in hello `sasToken` property tooaccess hello file.</span></span> <span data-ttu-id="65b8b-134">Hello .zip letöltése és kicsomagolása, után hello definiált konfigurációs függvény `configurationFunction` toogenerate hello MOF-fájl futtatása.</span><span class="sxs-lookup"><span data-stu-id="65b8b-134">After hello .zip is downloaded and unpacked, hello configuration function defined in `configurationFunction` is run toogenerate hello MOF file.</span></span> <span data-ttu-id="65b8b-135">hello bővítmény majd futtatja az `Start-DscConfiguration -Force` hello jön létre a MOF-fájlt.</span><span class="sxs-lookup"><span data-stu-id="65b8b-135">hello extension then runs `Start-DscConfiguration -Force` on hello generated MOF file.</span></span> <span data-ttu-id="65b8b-136">hello bővítmény kimeneti rögzíti, és vissza kimenő toohello Azure állapot csatorna írja azt.</span><span class="sxs-lookup"><span data-stu-id="65b8b-136">hello extension captures output and writes it back out toohello Azure Status Channel.</span></span> <span data-ttu-id="65b8b-137">Ettől kezdve, hello DSC LCM kezeli a figyelés és a szokásos módon javítása.</span><span class="sxs-lookup"><span data-stu-id="65b8b-137">From this point on, hello DSC LCM handles monitoring and correction as normal.</span></span> 

## <a name="powershell-cmdlets"></a><span data-ttu-id="65b8b-138">PowerShell-parancsmagok</span><span class="sxs-lookup"><span data-stu-id="65b8b-138">PowerShell cmdlets</span></span>
<span data-ttu-id="65b8b-139">PowerShell-parancsmagok használhatók az Azure Resource Manager vagy a klasszikus telepítési modell toopackage hello, közzétételét és DSC-bővítmény telepítésének figyelése.</span><span class="sxs-lookup"><span data-stu-id="65b8b-139">PowerShell cmdlets can be used with Azure Resource Manager or hello classic deployment model toopackage, publish, and monitor DSC extension deployments.</span></span> <span data-ttu-id="65b8b-140">hello szerepel a következő parancsmagok olyan hello klasszikus üzembe helyezési modulok, de "Azure" "AzureRm" toouse hello Azure Resource Manager modellt lehet cserélni.</span><span class="sxs-lookup"><span data-stu-id="65b8b-140">hello following cmdlets listed are hello classic deployment modules, but "Azure" can be replaced with "AzureRm" toouse hello Azure Resource Manager model.</span></span> <span data-ttu-id="65b8b-141">Például `Publish-AzureVMDscConfiguration` használ hello klasszikus üzembe helyezési modellel, ahol `Publish-AzureRmVMDscConfiguration` Azure Resource Managert használja.</span><span class="sxs-lookup"><span data-stu-id="65b8b-141">For example,  `Publish-AzureVMDscConfiguration` uses hello classic deployment model, where `Publish-AzureRmVMDscConfiguration` uses Azure Resource Manager.</span></span> 

<span data-ttu-id="65b8b-142">`Publish-AzureVMDscConfiguration`időt vesz igénybe, a konfigurációs fájlban, keres a DSC tőle függő erőforrások, és létrehoz egy hello mind DSC erőforrások szükséges tooenact hello konfigurációs tartalmazó .zip fájlt.</span><span class="sxs-lookup"><span data-stu-id="65b8b-142">`Publish-AzureVMDscConfiguration` takes in a configuration file, scans it for dependent DSC resources, and creates a .zip file containing hello configuration and DSC resources needed tooenact hello configuration.</span></span> <span data-ttu-id="65b8b-143">Azt is létrehozhat hello csomagot helyben hello `-ConfigurationArchivePath` paraméter.</span><span class="sxs-lookup"><span data-stu-id="65b8b-143">It can also create hello package locally using hello `-ConfigurationArchivePath` parameter.</span></span> <span data-ttu-id="65b8b-144">Ellenkező esetben hello .zip fájl tooAzure blob-tároló tesz közzé, és biztosítja a az SAS-jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="65b8b-144">Otherwise, it publishes hello .zip file tooAzure blob storage and secures it with a SAS token.</span></span>

<span data-ttu-id="65b8b-145">Ez a parancsmag által létrehozott hello .zip fájl hello .ps1 konfigurációs parancsfájl hello archív mappa gyökeréhez hello rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="65b8b-145">hello .zip file created by this cmdlet has hello .ps1 configuration script at hello root of hello archive folder.</span></span> <span data-ttu-id="65b8b-146">Erőforrások van hello modul mappára hello archív mappába helyezi.</span><span class="sxs-lookup"><span data-stu-id="65b8b-146">Resources have hello module folder placed in hello archive folder.</span></span> 

<span data-ttu-id="65b8b-147">`Set-AzureVMDscExtension`hello beállításokat, amelyek használatával hello PowerShell DSC-bővítményt a Virtuálisgép-konfigurációs objektum esetében.</span><span class="sxs-lookup"><span data-stu-id="65b8b-147">`Set-AzureVMDscExtension` injects hello settings needed by hello PowerShell DSC extension into a VM configuration object.</span></span> <span data-ttu-id="65b8b-148">Hello klasszikus üzembe helyezési modellel, hello VM módosítások alkalmazott tooan Azure virtuális Gépen kell lennie a `Update-AzureVM`.</span><span class="sxs-lookup"><span data-stu-id="65b8b-148">In hello classic deployment model, hello VM changes must be applied tooan Azure VM with `Update-AzureVM`.</span></span> 

<span data-ttu-id="65b8b-149">`Get-AzureVMDscExtension`Lekéri egy adott virtuális gép hello DSC-bővítmény állapotának.</span><span class="sxs-lookup"><span data-stu-id="65b8b-149">`Get-AzureVMDscExtension` retrieves hello DSC extension status of a particular VM.</span></span> 

<span data-ttu-id="65b8b-150">`Get-AzureVMDscExtensionStatus`lekéri a hello állapotának hello DSC-konfiguráció hello DSC-bővítmény kezelő helyeztek.</span><span class="sxs-lookup"><span data-stu-id="65b8b-150">`Get-AzureVMDscExtensionStatus` retrieves hello status of hello DSC configuration enacted by hello DSC extension handler.</span></span> <span data-ttu-id="65b8b-151">Ez a művelet végrehajtható egyetlen virtuális gép vagy csoportján.</span><span class="sxs-lookup"><span data-stu-id="65b8b-151">This action can be performed on a single VM, or group of VMs.</span></span>

<span data-ttu-id="65b8b-152">`Remove-AzureVMDscExtension`hello-kiterjesztés kezelőjének eltávolítása a megadott virtuális géppel.</span><span class="sxs-lookup"><span data-stu-id="65b8b-152">`Remove-AzureVMDscExtension` removes hello extension handler from a given virtual machine.</span></span> <span data-ttu-id="65b8b-153">Ez a parancsmag does **nem** hello konfiguráció eltávolítása, távolítsa el a hello WMF vagy hello virtuális gépen hello alkalmazott beállítások módosítása.</span><span class="sxs-lookup"><span data-stu-id="65b8b-153">This cmdlet does **not** remove hello configuration, uninstall hello WMF, or change hello applied settings on hello virtual machine.</span></span> <span data-ttu-id="65b8b-154">Csak hello bővítmény kezelő távolítja el.</span><span class="sxs-lookup"><span data-stu-id="65b8b-154">It only removes hello extension handler.</span></span> 

<span data-ttu-id="65b8b-155">**A címterület-kezelés és az Azure Resource Manager parancsmagok főbb változásai**</span><span class="sxs-lookup"><span data-stu-id="65b8b-155">**Key differences in ASM and Azure Resource Manager cmdlets**</span></span>

* <span data-ttu-id="65b8b-156">Az Azure Resource Manager parancsmagjainak szinkronizáltak.</span><span class="sxs-lookup"><span data-stu-id="65b8b-156">Azure Resource Manager cmdlets are synchronous.</span></span> <span data-ttu-id="65b8b-157">Címterület-kezelési parancsmagok aszinkron jellegűek.</span><span class="sxs-lookup"><span data-stu-id="65b8b-157">ASM cmdlets are asynchronous.</span></span>
* <span data-ttu-id="65b8b-158">Erőforráscsoport-név, a VMName, a ArchiveStorageAccountName, a verziót és a hely összes kötelező paraméterek az Azure Resource Manager tartoznak.</span><span class="sxs-lookup"><span data-stu-id="65b8b-158">ResourceGroupName, VMName, ArchiveStorageAccountName, Version, and Location are all required parameters in Azure Resource Manager.</span></span>
* <span data-ttu-id="65b8b-159">ArchiveResourceGroupName egy új Azure Resource Manager nem kötelező paraméter.</span><span class="sxs-lookup"><span data-stu-id="65b8b-159">ArchiveResourceGroupName is a new optional parameter for Azure Resource Manager.</span></span> <span data-ttu-id="65b8b-160">Ez a paraméter is megadhat, ha a tárfiók tartozik tooa másik erőforráscsoportban található, mint egy hello ahol hello a virtuális gép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="65b8b-160">You can specify this parameter when your storage account belongs tooa different resource group than hello one where hello virtual machine is created.</span></span>
* <span data-ttu-id="65b8b-161">ConfigurationArchive nevezik ArchiveBlobName az Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="65b8b-161">ConfigurationArchive is called ArchiveBlobName in Azure Resource Manager</span></span>
* <span data-ttu-id="65b8b-162">ContainerName nevezik ArchiveContainerName az Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="65b8b-162">ContainerName is called ArchiveContainerName in Azure Resource Manager</span></span>
* <span data-ttu-id="65b8b-163">StorageEndpointSuffix nevezik ArchiveStorageEndpointSuffix az Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="65b8b-163">StorageEndpointSuffix is called ArchiveStorageEndpointSuffix in Azure Resource Manager</span></span>
* <span data-ttu-id="65b8b-164">hello AutoUpdate kapcsoló tooAzure erőforrás-kezelő tooenable automatikus frissítés hello kiterjesztés kezelő toohello legújabb verziója, és a rendelkezésre álló bővült.</span><span class="sxs-lookup"><span data-stu-id="65b8b-164">hello AutoUpdate switch has been added tooAzure Resource Manager tooenable automatic updating of hello extension handler toohello latest version as and when it is available.</span></span> <span data-ttu-id="65b8b-165">Vegye figyelembe ennek a paraméternek hello lehetséges toocause újraindul a virtuális gép, amikor egy új verziója WMF felszabadul hello hello.</span><span class="sxs-lookup"><span data-stu-id="65b8b-165">Note this parameter has hello potential toocause reboots on hello VM when a new version of hello WMF is released.</span></span> 

## <a name="azure-portal-functionality"></a><span data-ttu-id="65b8b-166">Az Azure portál funkció</span><span class="sxs-lookup"><span data-stu-id="65b8b-166">Azure portal functionality</span></span>
<span data-ttu-id="65b8b-167">Keresse meg a virtuális gép tooa.</span><span class="sxs-lookup"><span data-stu-id="65b8b-167">Browse tooa VM.</span></span> <span data-ttu-id="65b8b-168">A beállítások -> Általános kattintson "Kiterjesztésekkel."</span><span class="sxs-lookup"><span data-stu-id="65b8b-168">Under Settings -> General click "Extensions."</span></span> <span data-ttu-id="65b8b-169">Egy új ablak jön létre.</span><span class="sxs-lookup"><span data-stu-id="65b8b-169">A new pane is created.</span></span> <span data-ttu-id="65b8b-170">Kattintson a "Hozzáadás" gombra, és válassza ki a PowerShell DSC.</span><span class="sxs-lookup"><span data-stu-id="65b8b-170">Click "Add" and select PowerShell DSC.</span></span>

<span data-ttu-id="65b8b-171">hello portál van szüksége.</span><span class="sxs-lookup"><span data-stu-id="65b8b-171">hello portal needs input.</span></span>
<span data-ttu-id="65b8b-172">**Modulok vagy parancsfájl**: A mező kitöltése kötelező.</span><span class="sxs-lookup"><span data-stu-id="65b8b-172">**Configuration Modules or Script**: This field is mandatory.</span></span> <span data-ttu-id="65b8b-173">Egy konfigurációs parancsfájl tartalmazó .ps1 fájlt vagy egy .ps1 konfigurációs parancsfájl hello gyökerében és a tőle függő összes erőforrásról, a modul mappákban belül hello .zip .zip fájl szükséges.</span><span class="sxs-lookup"><span data-stu-id="65b8b-173">Requires a .ps1 file containing a configuration script, or a .zip file with a .ps1 configuration script at hello root, and all dependent resources in module folders within hello .zip.</span></span> <span data-ttu-id="65b8b-174">A hello is létrehozható `Publish-AzureVMDscConfiguration -ConfigurationArchivePath` parancsmag hello Azure PowerShell SDK tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="65b8b-174">It can be created with hello `Publish-AzureVMDscConfiguration -ConfigurationArchivePath` cmdlet included in hello Azure PowerShell SDK.</span></span> <span data-ttu-id="65b8b-175">hello .zip fájl van töltve a SAS-jogkivonat által védett felhasználói blob-tároló.</span><span class="sxs-lookup"><span data-stu-id="65b8b-175">hello .zip file is uploaded into your user blob storage secured by a SAS token.</span></span> 

<span data-ttu-id="65b8b-176">**Konfigurációs adatok psd1 kiterjesztésű fájl**: Ez a mező nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="65b8b-176">**Configuration Data PSD1 File**: This field is optional.</span></span> <span data-ttu-id="65b8b-177">Ha a konfiguráció szükséges .psd1 a konfigurációs adatfájlt, használja az ebben a mezőben tooselect azt, és töltse fel tooyour felhasználói blob-tároló, SAS-jogkivonat által védett ahol.</span><span class="sxs-lookup"><span data-stu-id="65b8b-177">If your configuration requires a configuration data file in .psd1, use this field tooselect it and upload it tooyour user blob storage, where it is secured by a SAS token.</span></span> 

<span data-ttu-id="65b8b-178">**Konfiguráció neve modulhoz minősített**: .ps1 fájlok rendelkezhet több konfigurációs funkciók.</span><span class="sxs-lookup"><span data-stu-id="65b8b-178">**Module-Qualified Name of Configuration**: .ps1 files can have multiple configuration functions.</span></span> <span data-ttu-id="65b8b-179">Hello konfigurációs .ps1 parancsfájl követ hello nevét adja meg a "\' és hello hello konfigurációs függvény neve.</span><span class="sxs-lookup"><span data-stu-id="65b8b-179">Enter hello name of hello configuration .ps1 script followed by a  '\' and hello name of hello configuration function.</span></span> <span data-ttu-id="65b8b-180">Például ha a .ps1 parancsfájl hello neve "configuration.ps1", és hello konfigurációs "IisInstall", írja be:`configuration.ps1\IisInstall`</span><span class="sxs-lookup"><span data-stu-id="65b8b-180">For example, if your .ps1 script has hello name "configuration.ps1", and hello configuration is "IisInstall", you would enter: `configuration.ps1\IisInstall`</span></span>

<span data-ttu-id="65b8b-181">**Konfigurációs argumentumok**: Ha hello konfigurációs függvény argumentummal, adja meg őket itt hello formátumban `argumentName1=value1,argumentName2=value2`.</span><span class="sxs-lookup"><span data-stu-id="65b8b-181">**Configuration Arguments**: If hello configuration function takes arguments, enter them in here in hello format `argumentName1=value1,argumentName2=value2`.</span></span> <span data-ttu-id="65b8b-182">Megjegyzés: ezt a formátumot más formátumú, mint hogyan konfigurációs argumentumot fogad PowerShell-parancsmagok és a Resource Manager-sablonok használatával.</span><span class="sxs-lookup"><span data-stu-id="65b8b-182">Note this format is a different format than how configuration arguments are accepted through PowerShell cmdlets or Resource Manager templates.</span></span> 

## <a name="getting-started"></a><span data-ttu-id="65b8b-183">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="65b8b-183">Getting started</span></span>
<span data-ttu-id="65b8b-184">hello Azure DSC-bővítményt vesz igénybe a DSC-konfiguráció dokumentumok és ír elő őket az Azure virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="65b8b-184">hello Azure DSC extension takes in DSC configuration documents and enacts them on Azure VMs.</span></span> <span data-ttu-id="65b8b-185">A következő konfiguráció egy egyszerű példa.</span><span class="sxs-lookup"><span data-stu-id="65b8b-185">A simple example of a configuration follows.</span></span> <span data-ttu-id="65b8b-186">Mentse helyileg "IisInstall.ps1":</span><span class="sxs-lookup"><span data-stu-id="65b8b-186">Save it locally as "IisInstall.ps1":</span></span>

```powershell
configuration IISInstall 
{ 
    node "localhost"
    { 
        WindowsFeature IIS 
        { 
            Ensure = "Present" 
            Name = "Web-Server"                       
        } 
    } 
}
```

<span data-ttu-id="65b8b-187">hello követő lépéseket hely hello IisInstall.ps1 parancsfájl hello a megadott virtuális gép, hello konfigurációs hajtható végre, és jelentéseket küldhetnek vissza állapota.</span><span class="sxs-lookup"><span data-stu-id="65b8b-187">hello following steps place hello IisInstall.ps1 script on hello specified VM, execute hello configuration, and report back on status.</span></span>
###<a name="classic-model"></a><span data-ttu-id="65b8b-188">Klasszikus modell</span><span class="sxs-lookup"><span data-stu-id="65b8b-188">Classic model</span></span>
```powershell
#Azure PowerShell cmdlets are required
Import-Module Azure

#Use an existing Azure Virtual Machine, 'DscDemo1'
$demoVM = Get-AzureVM DscDemo1

#Publish hello configuration script into user storage.
Publish-AzureVMDscConfiguration -ConfigurationPath ".\IisInstall.ps1" -StorageContext $storageContext -Verbose -Force

#Set hello VM toorun hello DSC configuration
Set-AzureVMDscExtension -VM $demoVM -ConfigurationArchive "IisInstall.ps1.zip" -StorageContext $storageContext -ConfigurationName "IisInstall" -Verbose

#Update hello configuration of an Azure Virtual Machine
$demoVM | Update-AzureVM -Verbose

#check on status
Get-AzureVMDscExtensionStatus -VM $demovm -Verbose
```
###<a name="azure-resource-manager-model"></a><span data-ttu-id="65b8b-189">Az Azure Resource Manager modellt</span><span class="sxs-lookup"><span data-stu-id="65b8b-189">Azure Resource Manager model</span></span>

```powershell
$resourceGroup = "dscVmDemo"
$location = "westus"
$vmName = "myVM"
$storageName = "demostorage"
#Publish hello configuration script into user storage
Publish-AzureRmVMDscConfiguration -ConfigurationPath .\iisInstall.ps1 -ResourceGroupName $resourceGroup -StorageAccountName $storageName -force
#Set hello VM toorun hello DSC configuration
Set-AzureRmVmDscExtension -Version 2.21 -ResourceGroupName $resourceGroup -VMName $vmName -ArchiveStorageAccountName $storageName -ArchiveBlobName iisInstall.ps1.zip -AutoUpdate:$true -ConfigurationName "IISInstall"

```

## <a name="logging"></a><span data-ttu-id="65b8b-190">Naplózás</span><span class="sxs-lookup"><span data-stu-id="65b8b-190">Logging</span></span>
<span data-ttu-id="65b8b-191">Naplók kerülnek:</span><span class="sxs-lookup"><span data-stu-id="65b8b-191">Logs are placed in:</span></span>

<span data-ttu-id="65b8b-192">C:\WindowsAzure\Logs\Plugins\Microsoft.PowerShell.DSC\[verziószám]</span><span class="sxs-lookup"><span data-stu-id="65b8b-192">C:\WindowsAzure\Logs\Plugins\Microsoft.Powershell.DSC\[Version Number]</span></span>

## <a name="next-steps"></a><span data-ttu-id="65b8b-193">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="65b8b-193">Next steps</span></span>
<span data-ttu-id="65b8b-194">További információ a PowerShell DSC [látogasson el a hello PowerShell dokumentációs központban](https://msdn.microsoft.com/powershell/dsc/overview).</span><span class="sxs-lookup"><span data-stu-id="65b8b-194">For more information about PowerShell DSC, [visit hello PowerShell documentation center](https://msdn.microsoft.com/powershell/dsc/overview).</span></span> 

<span data-ttu-id="65b8b-195">Vizsgálja meg a hello [Azure Resource Manager sablon hello DSC-bővítmény](extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="65b8b-195">Examine hello [Azure Resource Manager template for hello DSC extension](extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 

<span data-ttu-id="65b8b-196">a PowerShell DSC kezelhető toofind további funkciók [keresse meg a PowerShell-galériában hello](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) további DSC-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="65b8b-196">toofind additional functionality you can manage with PowerShell DSC, [browse hello PowerShell gallery](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) for more DSC resources.</span></span>

<span data-ttu-id="65b8b-197">Bizalmas paraméterek átadása a konfigurációk a részletekért lásd: [biztonságosan hello DSC-bővítmény kezelő a hitelesítő adatok kezelése](extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="65b8b-197">For details on passing sensitive parameters into configurations, see [Manage credentials securely with hello DSC extension handler](extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

