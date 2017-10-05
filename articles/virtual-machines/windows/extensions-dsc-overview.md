---
title: "Célállapotkonfiguráció Azure áttekintés |} Microsoft Docs"
description: "A Microsoft Azure-kiterjesztés kezelője segítségével a PowerShell célállapot-konfiguráció áttekintése. Többek között az Előfeltételek, architektúra, parancsmagok..."
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
ms.openlocfilehash: c05c2d541a5f526f362f9cd72fe6d878374112b6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="introduction-to-the-azure-desired-state-configuration-extension-handler"></a><span data-ttu-id="9907c-104">Az Azure célállapot-konfiguráció kiterjesztés kezelőjének bemutatása</span><span class="sxs-lookup"><span data-stu-id="9907c-104">Introduction to the Azure Desired State Configuration extension handler</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="9907c-105">Az Azure Virtuálisgép-ügynök és a kapcsolódó bővítmények a Microsoft Azure infrastruktúra-szolgáltatásokat a részét képezik.</span><span class="sxs-lookup"><span data-stu-id="9907c-105">The Azure VM Agent and associated Extensions are part of the Microsoft Azure Infrastructure Services.</span></span> <span data-ttu-id="9907c-106">Virtuálisgép-bővítmények olyan szoftverösszetevők, amelyek a virtuális gép bővíthetők, és egyszerűbbé tehető a virtuális gép különböző felügyeleti műveleteket.</span><span class="sxs-lookup"><span data-stu-id="9907c-106">VM Extensions are software components that extend the VM functionality and simplify various VM management operations.</span></span> <span data-ttu-id="9907c-107">Például a VMAccess bővítmény segítségével a rendszergazda jelszavát, vagy az egyéni parancsprogramok futtatására szolgáló bővítmény használható parancsfájl végrehajtása a virtuális Gépen.</span><span class="sxs-lookup"><span data-stu-id="9907c-107">For example, the VMAccess extension can be used to reset an administrator's password, or the Custom Script extension can be used to execute a script on the VM.</span></span>

<span data-ttu-id="9907c-108">Ez a cikk a PowerShell kívánt állapot konfigurációs szolgáltatása (DSC) bővítmény be Azure virtuális gépek az Azure PowerShell SDK részeként.</span><span class="sxs-lookup"><span data-stu-id="9907c-108">This article introduces the PowerShell Desired State Configuration (DSC) Extension for Azure VMs as part of the Azure PowerShell SDK.</span></span> <span data-ttu-id="9907c-109">Új parancsmagok segítségével töltse fel, és a PowerShell DSC-konfiguráció alkalmazása egy Azure virtuális gépen engedélyezve van a PowerShell DSC-bővítménnyel.</span><span class="sxs-lookup"><span data-stu-id="9907c-109">You can use new cmdlets to upload and apply a PowerShell DSC configuration on an Azure VM enabled with the PowerShell DSC extension.</span></span> <span data-ttu-id="9907c-110">A PowerShell DSC bővítmény hívások a PowerShell DSC kihirdeti a fogadott DSC-konfiguráció a virtuális Gépen.</span><span class="sxs-lookup"><span data-stu-id="9907c-110">The PowerShell DSC extension calls into PowerShell DSC to enact the received DSC configuration on the VM.</span></span> <span data-ttu-id="9907c-111">Ez a funkció az Azure portálon keresztül is érhető el.</span><span class="sxs-lookup"><span data-stu-id="9907c-111">This functionality is also available through the Azure portal.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9907c-112">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="9907c-112">Prerequisites</span></span>
<span data-ttu-id="9907c-113">**Helyi számítógép** kommunikál az Azure Virtuálisgép-bővítmény, az Azure-portálon vagy az Azure PowerShell SDK használatával szüksége.</span><span class="sxs-lookup"><span data-stu-id="9907c-113">**Local machine** To interact with the Azure VM extension, you need to use either the Azure portal or the Azure PowerShell SDK.</span></span> 

<span data-ttu-id="9907c-114">**A Vendégügynök** az Azure virtuális gép a DSC-konfiguráció által konfigurált kell lennie, amely támogatja a Windows Management Framework (WMF) 4.0-s vagy 5.0 operációs.</span><span class="sxs-lookup"><span data-stu-id="9907c-114">**Guest Agent** The Azure VM that is configured by the DSC configuration needs to be an OS that supports either Windows Management Framework (WMF) 4.0 or 5.0.</span></span> <span data-ttu-id="9907c-115">A támogatott operációsrendszer-verziók teljes listája megtalálható a [DSC-bővítmény verzióelőzmények](https://blogs.msdn.microsoft.com/powershell/2014/11/20/release-history-for-the-azure-dsc-extension/).</span><span class="sxs-lookup"><span data-stu-id="9907c-115">The full list of supported OS versions can be found at the [DSC Extension Version History](https://blogs.msdn.microsoft.com/powershell/2014/11/20/release-history-for-the-azure-dsc-extension/).</span></span>

## <a name="terms-and-concepts"></a><span data-ttu-id="9907c-116">Kifejezések és fogalmak</span><span class="sxs-lookup"><span data-stu-id="9907c-116">Terms and concepts</span></span>
<span data-ttu-id="9907c-117">Ez az útmutató a következő fogalmakat ismeretét feltételezi:</span><span class="sxs-lookup"><span data-stu-id="9907c-117">This guide presumes familiarity with the following concepts:</span></span>

<span data-ttu-id="9907c-118">Konfiguráció - DSC konfigurációs dokumentum.</span><span class="sxs-lookup"><span data-stu-id="9907c-118">Configuration - A DSC configuration document.</span></span> 

<span data-ttu-id="9907c-119">Csomópont - céljának DSC-konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="9907c-119">Node - A target for a DSC configuration.</span></span> <span data-ttu-id="9907c-120">A jelen dokumentum "csomópont" mindig hivatkozik egy Azure virtuális Gépen.</span><span class="sxs-lookup"><span data-stu-id="9907c-120">In this document, "node" always refers to an Azure VM.</span></span>

<span data-ttu-id="9907c-121">Konfigurációs adatok - egy .psd1 fájlt a környezeti adatokat tartalmazó konfiguráció</span><span class="sxs-lookup"><span data-stu-id="9907c-121">Configuration Data - A .psd1 file containing environmental data for a configuration</span></span>

## <a name="architectural-overview"></a><span data-ttu-id="9907c-122">Az architektúra áttekintése</span><span class="sxs-lookup"><span data-stu-id="9907c-122">Architectural overview</span></span>
<span data-ttu-id="9907c-123">Az Azure DSC-bővítmény az Azure Virtuálisgép-ügynök keretrendszer használatával kézbesíti, kihirdeti, és a jelentés az Azure virtuális gépeken futó DSC-konfigurációk.</span><span class="sxs-lookup"><span data-stu-id="9907c-123">The Azure DSC extension uses the Azure VM Agent framework to deliver, enact, and report on DSC configurations running on Azure VMs.</span></span> <span data-ttu-id="9907c-124">A DSC-bővítményt vár legalább egy konfigurációs dokumentum, és az Azure PowerShell SDK vagy az Azure-portálon keresztül megadott paraméterek tartalmazó .zip fájlt.</span><span class="sxs-lookup"><span data-stu-id="9907c-124">The DSC extension expects a .zip file containing at least a configuration document, and a set of parameters provided either through the Azure PowerShell SDK or through the Azure portal.</span></span>

<span data-ttu-id="9907c-125">A bővítmény hívásakor először a telepítési folyamat futása.</span><span class="sxs-lookup"><span data-stu-id="9907c-125">When the extension is called for the first time, it runs an installation process.</span></span> <span data-ttu-id="9907c-126">Ez a folyamat egy olyan verzióját, a Windows Management Framework (WMF) használatával a következő programot telepíti:</span><span class="sxs-lookup"><span data-stu-id="9907c-126">This process installs a version of the Windows Management Framework (WMF) using the following logic:</span></span>

1. <span data-ttu-id="9907c-127">Ha az Azure virtuális gép operációs rendszere Windows Server 2016-os, semmilyen művelet nem lesz végrehajtva.</span><span class="sxs-lookup"><span data-stu-id="9907c-127">If the Azure VM OS is Windows Server 2016, no action is taken.</span></span> <span data-ttu-id="9907c-128">Windows Server 2016 már telepített PowerShell legújabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="9907c-128">Windows Server 2016 already has the latest version of PowerShell installed.</span></span>
2. <span data-ttu-id="9907c-129">Ha a `wmfVersion` tulajdonság van megadva, a WMF adott verziója van telepítve, kivéve, ha a virtuális gép operációs rendszere kompatibilis.</span><span class="sxs-lookup"><span data-stu-id="9907c-129">If the `wmfVersion` property is specified, that version of the WMF is installed unless it is incompatible with the VM's OS.</span></span>
3. <span data-ttu-id="9907c-130">Ha nincs `wmfVersion` tulajdonság van megadva, a WMF legújabb megfelelő verziója telepítve van.</span><span class="sxs-lookup"><span data-stu-id="9907c-130">If no `wmfVersion` property is specified, the latest applicable version of the WMF is installed.</span></span>

<span data-ttu-id="9907c-131">A WMF telepítése újraindítást igényel.</span><span class="sxs-lookup"><span data-stu-id="9907c-131">Installation of the WMF requires a reboot.</span></span> <span data-ttu-id="9907c-132">Az újraindítást követően a bővítmény megadott .zip fájl letölti a `modulesUrl` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="9907c-132">After reboot, the extension downloads the .zip file specified in the `modulesUrl` property.</span></span> <span data-ttu-id="9907c-133">Ha ezen a helyen az Azure blob Storage tárolóban, SAS-tokennel is megadhatók a `sasToken` tulajdonság a fájl elérésére.</span><span class="sxs-lookup"><span data-stu-id="9907c-133">If this location is in Azure blob storage, a SAS token can be specified in the `sasToken` property to access the file.</span></span> <span data-ttu-id="9907c-134">A .zip letöltése és kicsomagolása, után a konfigurációs függvény meghatározott `configurationFunction` fut a MOF-fájl létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="9907c-134">After the .zip is downloaded and unpacked, the configuration function defined in `configurationFunction` is run to generate the MOF file.</span></span> <span data-ttu-id="9907c-135">A bővítmény majd futtatja az `Start-DscConfiguration -Force` meg a létrehozott MOF-fájlt.</span><span class="sxs-lookup"><span data-stu-id="9907c-135">The extension then runs `Start-DscConfiguration -Force` on the generated MOF file.</span></span> <span data-ttu-id="9907c-136">A bővítmény kimeneti, mind az írás vissza azt az Azure állapot csatorna rögzíti.</span><span class="sxs-lookup"><span data-stu-id="9907c-136">The extension captures output and writes it back out to the Azure Status Channel.</span></span> <span data-ttu-id="9907c-137">A DSC LCM ettől kezdve, kezeli a figyelés és a szokásos módon javítása.</span><span class="sxs-lookup"><span data-stu-id="9907c-137">From this point on, the DSC LCM handles monitoring and correction as normal.</span></span> 

## <a name="powershell-cmdlets"></a><span data-ttu-id="9907c-138">PowerShell-parancsmagok</span><span class="sxs-lookup"><span data-stu-id="9907c-138">PowerShell cmdlets</span></span>
<span data-ttu-id="9907c-139">PowerShell-parancsmagok segítségével az Azure Resource Manager vagy a klasszikus üzembe helyezési modellel csomag közzététele és DSC-bővítmény telepítésének figyelése.</span><span class="sxs-lookup"><span data-stu-id="9907c-139">PowerShell cmdlets can be used with Azure Resource Manager or the classic deployment model to package, publish, and monitor DSC extension deployments.</span></span> <span data-ttu-id="9907c-140">A következő táblázatban szereplő parancsmagok a klasszikus üzembe helyezési modulok, de "Azure" lecserélhető a "AzureRm" az Azure Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="9907c-140">The following cmdlets listed are the classic deployment modules, but "Azure" can be replaced with "AzureRm" to use the Azure Resource Manager model.</span></span> <span data-ttu-id="9907c-141">Például `Publish-AzureVMDscConfiguration` a klasszikus üzembe helyezési modellt használ, ahol `Publish-AzureRmVMDscConfiguration` Azure Resource Managert használja.</span><span class="sxs-lookup"><span data-stu-id="9907c-141">For example,  `Publish-AzureVMDscConfiguration` uses the classic deployment model, where `Publish-AzureRmVMDscConfiguration` uses Azure Resource Manager.</span></span> 

<span data-ttu-id="9907c-142">`Publish-AzureVMDscConfiguration`időt vesz igénybe, a konfigurációs fájlban, keres a DSC tőle függő erőforrások és a konfiguráció és a konfigurációs kihirdeti szükséges DSC erőforrásokat tartalmazó .zip fájlt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="9907c-142">`Publish-AzureVMDscConfiguration` takes in a configuration file, scans it for dependent DSC resources, and creates a .zip file containing the configuration and DSC resources needed to enact the configuration.</span></span> <span data-ttu-id="9907c-143">A csomag helyileg is létrehozhat a `-ConfigurationArchivePath` paraméter.</span><span class="sxs-lookup"><span data-stu-id="9907c-143">It can also create the package locally using the `-ConfigurationArchivePath` parameter.</span></span> <span data-ttu-id="9907c-144">Ellenkező esetben a .zip-fájlt az Azure blob storage közzéteszi, és biztosítja a SAS-tokennel rendelkező.</span><span class="sxs-lookup"><span data-stu-id="9907c-144">Otherwise, it publishes the .zip file to Azure blob storage and secures it with a SAS token.</span></span>

<span data-ttu-id="9907c-145">A zip-fájlt, a parancsmag által létrehozott a .ps1 konfigurációs parancsfájl az archív mappa gyökerében van.</span><span class="sxs-lookup"><span data-stu-id="9907c-145">The .zip file created by this cmdlet has the .ps1 configuration script at the root of the archive folder.</span></span> <span data-ttu-id="9907c-146">Erőforrások, hogy a modul mappát az archív mappába helyezi.</span><span class="sxs-lookup"><span data-stu-id="9907c-146">Resources have the module folder placed in the archive folder.</span></span> 

<span data-ttu-id="9907c-147">`Set-AzureVMDscExtension`a beállításokat, amelyek használatával a PowerShell DSC-bővítményt a Virtuálisgép-konfigurációs objektum esetében.</span><span class="sxs-lookup"><span data-stu-id="9907c-147">`Set-AzureVMDscExtension` injects the settings needed by the PowerShell DSC extension into a VM configuration object.</span></span> <span data-ttu-id="9907c-148">A klasszikus üzembe helyezési modellel, a virtuális gép alkalmazni kell a változtatásokat egy Azure virtuális gépre a `Update-AzureVM`.</span><span class="sxs-lookup"><span data-stu-id="9907c-148">In the classic deployment model, the VM changes must be applied to an Azure VM with `Update-AzureVM`.</span></span> 

<span data-ttu-id="9907c-149">`Get-AzureVMDscExtension`Lekéri egy adott virtuális gép DSC-bővítmény állapotának.</span><span class="sxs-lookup"><span data-stu-id="9907c-149">`Get-AzureVMDscExtension` retrieves the DSC extension status of a particular VM.</span></span> 

<span data-ttu-id="9907c-150">`Get-AzureVMDscExtensionStatus`a DSC-konfiguráció a DSC-bővítmény kezelő helyeztek állapotát olvassa be.</span><span class="sxs-lookup"><span data-stu-id="9907c-150">`Get-AzureVMDscExtensionStatus` retrieves the status of the DSC configuration enacted by the DSC extension handler.</span></span> <span data-ttu-id="9907c-151">Ez a művelet végrehajtható egyetlen virtuális gép vagy csoportján.</span><span class="sxs-lookup"><span data-stu-id="9907c-151">This action can be performed on a single VM, or group of VMs.</span></span>

<span data-ttu-id="9907c-152">`Remove-AzureVMDscExtension`a bővítmény kezelő eltávolítja a megadott virtuális géppel.</span><span class="sxs-lookup"><span data-stu-id="9907c-152">`Remove-AzureVMDscExtension` removes the extension handler from a given virtual machine.</span></span> <span data-ttu-id="9907c-153">Ez a parancsmag does **nem** távolítsa el a konfigurációt, távolítsa el a WMF, vagy módosítsa a virtuális gépen a alkalmazott beállításokat.</span><span class="sxs-lookup"><span data-stu-id="9907c-153">This cmdlet does **not** remove the configuration, uninstall the WMF, or change the applied settings on the virtual machine.</span></span> <span data-ttu-id="9907c-154">Csak a bővítmény kezelő távolítja el.</span><span class="sxs-lookup"><span data-stu-id="9907c-154">It only removes the extension handler.</span></span> 

<span data-ttu-id="9907c-155">**A címterület-kezelés és az Azure Resource Manager parancsmagok főbb változásai**</span><span class="sxs-lookup"><span data-stu-id="9907c-155">**Key differences in ASM and Azure Resource Manager cmdlets**</span></span>

* <span data-ttu-id="9907c-156">Az Azure Resource Manager parancsmagjainak szinkronizáltak.</span><span class="sxs-lookup"><span data-stu-id="9907c-156">Azure Resource Manager cmdlets are synchronous.</span></span> <span data-ttu-id="9907c-157">Címterület-kezelési parancsmagok aszinkron jellegűek.</span><span class="sxs-lookup"><span data-stu-id="9907c-157">ASM cmdlets are asynchronous.</span></span>
* <span data-ttu-id="9907c-158">Erőforráscsoport-név, a VMName, a ArchiveStorageAccountName, a verziót és a hely összes kötelező paraméterek az Azure Resource Manager tartoznak.</span><span class="sxs-lookup"><span data-stu-id="9907c-158">ResourceGroupName, VMName, ArchiveStorageAccountName, Version, and Location are all required parameters in Azure Resource Manager.</span></span>
* <span data-ttu-id="9907c-159">ArchiveResourceGroupName egy új Azure Resource Manager nem kötelező paraméter.</span><span class="sxs-lookup"><span data-stu-id="9907c-159">ArchiveResourceGroupName is a new optional parameter for Azure Resource Manager.</span></span> <span data-ttu-id="9907c-160">Ez a paraméter is megadhat, ha a tárfiók tartozik egy másik erőforráscsoportban található, mint ahol a virtuális gép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="9907c-160">You can specify this parameter when your storage account belongs to a different resource group than the one where the virtual machine is created.</span></span>
* <span data-ttu-id="9907c-161">ConfigurationArchive nevezik ArchiveBlobName az Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="9907c-161">ConfigurationArchive is called ArchiveBlobName in Azure Resource Manager</span></span>
* <span data-ttu-id="9907c-162">ContainerName nevezik ArchiveContainerName az Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="9907c-162">ContainerName is called ArchiveContainerName in Azure Resource Manager</span></span>
* <span data-ttu-id="9907c-163">StorageEndpointSuffix nevezik ArchiveStorageEndpointSuffix az Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="9907c-163">StorageEndpointSuffix is called ArchiveStorageEndpointSuffix in Azure Resource Manager</span></span>
* <span data-ttu-id="9907c-164">Az automatikus frissítési kapcsoló vált elérhetővé, az Azure Resource Manager a kiterjesztés kezelője a legfrissebb verzióra, és a rendelkezésre álló automatikus frissítésének engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="9907c-164">The AutoUpdate switch has been added to Azure Resource Manager to enable automatic updating of the extension handler to the latest version as and when it is available.</span></span> <span data-ttu-id="9907c-165">Megjegyzés: Ez a paraméter van okozhat újraindítja a virtuális Gépre, a WMF egy új verzióinak kiadásakor.</span><span class="sxs-lookup"><span data-stu-id="9907c-165">Note this parameter has the potential to cause reboots on the VM when a new version of the WMF is released.</span></span> 

## <a name="azure-portal-functionality"></a><span data-ttu-id="9907c-166">Az Azure portál funkció</span><span class="sxs-lookup"><span data-stu-id="9907c-166">Azure portal functionality</span></span>
<span data-ttu-id="9907c-167">Tallózással keresse meg a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="9907c-167">Browse to a VM.</span></span> <span data-ttu-id="9907c-168">A beállítások -> Általános kattintson "Kiterjesztésekkel."</span><span class="sxs-lookup"><span data-stu-id="9907c-168">Under Settings -> General click "Extensions."</span></span> <span data-ttu-id="9907c-169">Egy új ablak jön létre.</span><span class="sxs-lookup"><span data-stu-id="9907c-169">A new pane is created.</span></span> <span data-ttu-id="9907c-170">Kattintson a "Hozzáadás" gombra, és válassza ki a PowerShell DSC.</span><span class="sxs-lookup"><span data-stu-id="9907c-170">Click "Add" and select PowerShell DSC.</span></span>

<span data-ttu-id="9907c-171">A portál van szüksége.</span><span class="sxs-lookup"><span data-stu-id="9907c-171">The portal needs input.</span></span>
<span data-ttu-id="9907c-172">**Modulok vagy parancsfájl**: A mező kitöltése kötelező.</span><span class="sxs-lookup"><span data-stu-id="9907c-172">**Configuration Modules or Script**: This field is mandatory.</span></span> <span data-ttu-id="9907c-173">Egy konfigurációs parancsfájl tartalmazó .ps1 fájlt, vagy a legfelső szintű és a tőle függő összes erőforrásról, a modul mappákban belül a .zip .ps1 konfigurációs parancsfájl .zip fájl szükséges.</span><span class="sxs-lookup"><span data-stu-id="9907c-173">Requires a .ps1 file containing a configuration script, or a .zip file with a .ps1 configuration script at the root, and all dependent resources in module folders within the .zip.</span></span> <span data-ttu-id="9907c-174">A létrehozás a `Publish-AzureVMDscConfiguration -ConfigurationArchivePath` parancsmag az Azure PowerShell SDK tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="9907c-174">It can be created with the `Publish-AzureVMDscConfiguration -ConfigurationArchivePath` cmdlet included in the Azure PowerShell SDK.</span></span> <span data-ttu-id="9907c-175">A zip-fájlt a SAS-jogkivonat által védett felhasználói blob-tároló van töltve.</span><span class="sxs-lookup"><span data-stu-id="9907c-175">The .zip file is uploaded into your user blob storage secured by a SAS token.</span></span> 

<span data-ttu-id="9907c-176">**Konfigurációs adatok psd1 kiterjesztésű fájl**: Ez a mező nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="9907c-176">**Configuration Data PSD1 File**: This field is optional.</span></span> <span data-ttu-id="9907c-177">Ha a konfiguráció szükséges .psd1 a konfigurációs adatfájlt, használja ebben a mezőben jelölje ki, majd töltse fel a felhasználó blob-tároló, a SAS-jogkivonat által védett ahol.</span><span class="sxs-lookup"><span data-stu-id="9907c-177">If your configuration requires a configuration data file in .psd1, use this field to select it and upload it to your user blob storage, where it is secured by a SAS token.</span></span> 

<span data-ttu-id="9907c-178">**Konfiguráció neve modulhoz minősített**: .ps1 fájlok rendelkezhet több konfigurációs funkciók.</span><span class="sxs-lookup"><span data-stu-id="9907c-178">**Module-Qualified Name of Configuration**: .ps1 files can have multiple configuration functions.</span></span> <span data-ttu-id="9907c-179">Adja meg a konfigurációs .ps1 parancsfájl követ egy "\' és a konfigurációs függvény neve.</span><span class="sxs-lookup"><span data-stu-id="9907c-179">Enter the name of the configuration .ps1 script followed by a  '\' and the name of the configuration function.</span></span> <span data-ttu-id="9907c-180">Például ha a .ps1 parancsfájl "configuration.ps1" nevet, és a konfiguráció "IisInstall", írja be:`configuration.ps1\IisInstall`</span><span class="sxs-lookup"><span data-stu-id="9907c-180">For example, if your .ps1 script has the name "configuration.ps1", and the configuration is "IisInstall", you would enter: `configuration.ps1\IisInstall`</span></span>

<span data-ttu-id="9907c-181">**Konfigurációs argumentumok**: Ha a konfigurációs függvény argumentummal, adja meg őket itt formátumú `argumentName1=value1,argumentName2=value2`.</span><span class="sxs-lookup"><span data-stu-id="9907c-181">**Configuration Arguments**: If the configuration function takes arguments, enter them in here in the format `argumentName1=value1,argumentName2=value2`.</span></span> <span data-ttu-id="9907c-182">Megjegyzés: ezt a formátumot más formátumú, mint hogyan konfigurációs argumentumot fogad PowerShell-parancsmagok és a Resource Manager-sablonok használatával.</span><span class="sxs-lookup"><span data-stu-id="9907c-182">Note this format is a different format than how configuration arguments are accepted through PowerShell cmdlets or Resource Manager templates.</span></span> 

## <a name="getting-started"></a><span data-ttu-id="9907c-183">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="9907c-183">Getting started</span></span>
<span data-ttu-id="9907c-184">Az Azure DSC-bővítmény veszi a DSC-konfiguráció dokumentumok és ír elő őket az Azure virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="9907c-184">The Azure DSC extension takes in DSC configuration documents and enacts them on Azure VMs.</span></span> <span data-ttu-id="9907c-185">A következő konfiguráció egy egyszerű példa.</span><span class="sxs-lookup"><span data-stu-id="9907c-185">A simple example of a configuration follows.</span></span> <span data-ttu-id="9907c-186">Mentse helyileg "IisInstall.ps1":</span><span class="sxs-lookup"><span data-stu-id="9907c-186">Save it locally as "IisInstall.ps1":</span></span>

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

<span data-ttu-id="9907c-187">Helyezze a IisInstall.ps1 parancsfájl a megadott virtuális gép, végrehajtani a konfigurációs és jelentéseket küldhetnek vissza állapota az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="9907c-187">The following steps place the IisInstall.ps1 script on the specified VM, execute the configuration, and report back on status.</span></span>
###<a name="classic-model"></a><span data-ttu-id="9907c-188">Klasszikus modell</span><span class="sxs-lookup"><span data-stu-id="9907c-188">Classic model</span></span>
```powershell
#Azure PowerShell cmdlets are required
Import-Module Azure

#Use an existing Azure Virtual Machine, 'DscDemo1'
$demoVM = Get-AzureVM DscDemo1

#Publish the configuration script into user storage.
Publish-AzureVMDscConfiguration -ConfigurationPath ".\IisInstall.ps1" -StorageContext $storageContext -Verbose -Force

#Set the VM to run the DSC configuration
Set-AzureVMDscExtension -VM $demoVM -ConfigurationArchive "IisInstall.ps1.zip" -StorageContext $storageContext -ConfigurationName "IisInstall" -Verbose

#Update the configuration of an Azure Virtual Machine
$demoVM | Update-AzureVM -Verbose

#check on status
Get-AzureVMDscExtensionStatus -VM $demovm -Verbose
```
###<a name="azure-resource-manager-model"></a><span data-ttu-id="9907c-189">Az Azure Resource Manager modellt</span><span class="sxs-lookup"><span data-stu-id="9907c-189">Azure Resource Manager model</span></span>

```powershell
$resourceGroup = "dscVmDemo"
$location = "westus"
$vmName = "myVM"
$storageName = "demostorage"
#Publish the configuration script into user storage
Publish-AzureRmVMDscConfiguration -ConfigurationPath .\iisInstall.ps1 -ResourceGroupName $resourceGroup -StorageAccountName $storageName -force
#Set the VM to run the DSC configuration
Set-AzureRmVmDscExtension -Version 2.21 -ResourceGroupName $resourceGroup -VMName $vmName -ArchiveStorageAccountName $storageName -ArchiveBlobName iisInstall.ps1.zip -AutoUpdate:$true -ConfigurationName "IISInstall"

```

## <a name="logging"></a><span data-ttu-id="9907c-190">Naplózás</span><span class="sxs-lookup"><span data-stu-id="9907c-190">Logging</span></span>
<span data-ttu-id="9907c-191">Naplók kerülnek:</span><span class="sxs-lookup"><span data-stu-id="9907c-191">Logs are placed in:</span></span>

<span data-ttu-id="9907c-192">C:\WindowsAzure\Logs\Plugins\Microsoft.PowerShell.DSC\[verziószám]</span><span class="sxs-lookup"><span data-stu-id="9907c-192">C:\WindowsAzure\Logs\Plugins\Microsoft.Powershell.DSC\[Version Number]</span></span>

## <a name="next-steps"></a><span data-ttu-id="9907c-193">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9907c-193">Next steps</span></span>
<span data-ttu-id="9907c-194">További információ a PowerShell DSC [látogasson el a PowerShell dokumentációs központban](https://msdn.microsoft.com/powershell/dsc/overview).</span><span class="sxs-lookup"><span data-stu-id="9907c-194">For more information about PowerShell DSC, [visit the PowerShell documentation center](https://msdn.microsoft.com/powershell/dsc/overview).</span></span> 

<span data-ttu-id="9907c-195">Vizsgálja meg a [Azure Resource Manager sablon a DSC-bővítmény](extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="9907c-195">Examine the [Azure Resource Manager template for the DSC extension](extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 

<span data-ttu-id="9907c-196">A PowerShell DSC kezelhető további funkciók kereséséhez [keresse meg a PowerShell-galériában](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) további DSC-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="9907c-196">To find additional functionality you can manage with PowerShell DSC, [browse the PowerShell gallery](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) for more DSC resources.</span></span>

<span data-ttu-id="9907c-197">Bizalmas paraméterek átadása a konfigurációk a részletekért lásd: [biztonságosan a DSC-bővítmény kezelő a hitelesítő adatok kezelése](extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="9907c-197">For details on passing sensitive parameters into configurations, see [Manage credentials securely with the DSC extension handler](extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

