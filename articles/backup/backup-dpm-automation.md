---
title: "biztonsági mentés – mentése a DPM-munkaterhelések használja a Powershellt tooback aaaAzure |} Microsoft Docs"
description: "Megtudhatja, hogyan toodeploy és kezelése az Azure Backup a Data Protection Manager (DPM) PowerShell használatával"
services: backup
documentationcenter: 
author: NKolli1
manager: shreeshd
editor: 
ms.assetid: e9bd223c-2398-4eb1-9bf3-50e08970fea7
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/23/2017
ms.author: adigan;anuragm;trinadhk;markgal
ms.openlocfilehash: 27d2b4b3127b68c9da564697eb61dc3ccbc34b3d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-manage-backup-tooazure-for-data-protection-manager-dpm-servers-using-powershell"></a><span data-ttu-id="18749-103">Központi telepítése és kezelése a PowerShell használatával a Data Protection Manager (DPM) kiszolgálók biztonsági mentési tooAzure</span><span class="sxs-lookup"><span data-stu-id="18749-103">Deploy and manage backup tooAzure for Data Protection Manager (DPM) servers using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="18749-104">ARM</span><span class="sxs-lookup"><span data-stu-id="18749-104">ARM</span></span>](backup-dpm-automation.md)
> * [<span data-ttu-id="18749-105">Klasszikus</span><span class="sxs-lookup"><span data-stu-id="18749-105">Classic</span></span>](backup-dpm-automation-classic.md)
>
>

<span data-ttu-id="18749-106">Ez a cikk bemutatja, hogyan toouse PowerShell toosetup Azure biztonsági mentés egy DPM-kiszolgálón, és toomanage biztonsági mentési és helyreállítási.</span><span class="sxs-lookup"><span data-stu-id="18749-106">This article shows you how toouse PowerShell toosetup Azure Backup on a DPM server, and toomanage backup and recovery.</span></span>

## <a name="setting-up-hello-powershell-environment"></a><span data-ttu-id="18749-107">Hello PowerShell környezet létrehozása</span><span class="sxs-lookup"><span data-stu-id="18749-107">Setting up hello PowerShell environment</span></span>
[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-include.md)]

<span data-ttu-id="18749-108">PowerShell toomanage készített biztonsági másolat a Data Protection Manager tooAzure használata előtt kell toohave hello megfelelő környezet a PowerShellben.</span><span class="sxs-lookup"><span data-stu-id="18749-108">Before you can use PowerShell toomanage backups from Data Protection Manager tooAzure, you need toohave hello right environment in PowerShell.</span></span> <span data-ttu-id="18749-109">Elején hello hello PowerShell-munkamenetet győződjön meg arról, hogy futtassa a következő parancs tooimport hello jobb modulok hello, és lehetővé teszik toocorrectly hivatkozás hello DPM-parancsmagokat:</span><span class="sxs-lookup"><span data-stu-id="18749-109">At hello start of hello PowerShell session, ensure that you run hello following command tooimport hello right modules and allow you toocorrectly reference hello DPM cmdlets:</span></span>

```
PS C:> & "C:\Program Files\Microsoft System Center 2012 R2\DPM\DPM\bin\DpmCliInitScript.ps1"

Welcome toohello DPM Management Shell!

Full list of cmdlets: Get-Command
Only DPM cmdlets: Get-DPMCommand
Get general help: help
Get help for a cmdlet: help <cmdlet-name> or <cmdlet-name> -?
Get definition of a cmdlet: Get-Command <cmdlet-name> -Syntax
Sample DPM scripts: Get-DPMSampleScript
```

## <a name="setup-and-registration"></a><span data-ttu-id="18749-110">Telepítését és regisztrálását</span><span class="sxs-lookup"><span data-stu-id="18749-110">Setup and Registration</span></span>
<span data-ttu-id="18749-111">toobegin:</span><span class="sxs-lookup"><span data-stu-id="18749-111">toobegin:</span></span>

1. <span data-ttu-id="18749-112">[Töltse le a legfrissebb PowerShell](https://github.com/Azure/azure-powershell/releases) (szükséges minimális verziója: 1.0.0)</span><span class="sxs-lookup"><span data-stu-id="18749-112">[Download latest PowerShell](https://github.com/Azure/azure-powershell/releases) (minimum version required is: 1.0.0)</span></span>
2. <span data-ttu-id="18749-113">Engedélyezze a hello Azure Backup szolgáltatás parancsmagjaival váltás túl*AzureResourceManager* módból hello **Switch-AzureMode** parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="18749-113">Enable hello Azure Backup commandlets by switching too*AzureResourceManager* mode by using hello **Switch-AzureMode** commandlet:</span></span>

```
PS C:\> Switch-AzureMode AzureResourceManager
```

<span data-ttu-id="18749-114">hello a következő és nyilvántartási feladatok automatizálhatók a PowerShell használatával:</span><span class="sxs-lookup"><span data-stu-id="18749-114">hello following setup and registration tasks can be automated with PowerShell:</span></span>

* <span data-ttu-id="18749-115">Recovery Services-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="18749-115">Create a Recovery Services vault</span></span>
* <span data-ttu-id="18749-116">Hello Azure Backup szolgáltatás ügynökének telepítése</span><span class="sxs-lookup"><span data-stu-id="18749-116">Installing hello Azure Backup agent</span></span>
* <span data-ttu-id="18749-117">Hello Azure Backup szolgáltatás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="18749-117">Registering with hello Azure Backup service</span></span>
* <span data-ttu-id="18749-118">Hálózati beállítások</span><span class="sxs-lookup"><span data-stu-id="18749-118">Networking settings</span></span>
* <span data-ttu-id="18749-119">Titkosítási beállítások</span><span class="sxs-lookup"><span data-stu-id="18749-119">Encryption settings</span></span>

## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="18749-120">Recovery Services-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="18749-120">Create a recovery services vault</span></span>
<span data-ttu-id="18749-121">a lépéseket követve hello vezethet a Recovery Services-tároló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="18749-121">hello following steps lead you through creating a Recovery Services vault.</span></span> <span data-ttu-id="18749-122">Recovery Services-tároló nem egyezik egy biztonsági mentési tárolót.</span><span class="sxs-lookup"><span data-stu-id="18749-122">A Recovery Services vault is different than a Backup vault.</span></span>

1. <span data-ttu-id="18749-123">Ha használ Azure Backup a hello először, használnia kell a hello **Register-AzureRMResourceProvider** parancsmag tooregister hello Azure helyreállítási szolgáltató az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="18749-123">If you are using Azure Backup for hello first time, you must use hello **Register-AzureRMResourceProvider** cmdlet tooregister hello Azure Recovery Service provider with your subscription.</span></span>

    ```
    PS C:\> Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```
2. <span data-ttu-id="18749-124">hello Recovery Services-tároló egy ARM-erőforrás, ezért meg kell tooplace az erőforráscsoporton belül.</span><span class="sxs-lookup"><span data-stu-id="18749-124">hello Recovery Services vault is an ARM resource, so you need tooplace it within a Resource Group.</span></span> <span data-ttu-id="18749-125">Használjon egy meglévő erőforráscsoportot, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="18749-125">You can use an existing resource group, or create a new one.</span></span> <span data-ttu-id="18749-126">Új erőforráscsoport létrehozása esetén adja meg a hello és hello erőforrásnak helyét.</span><span class="sxs-lookup"><span data-stu-id="18749-126">When creating a new resource group, specify hello name and location for hello resource group.</span></span>  

    ```
    PS C:\> New-AzureRmResourceGroup –Name "test-rg" –Location "West US"
    ```
3. <span data-ttu-id="18749-127">Használjon hello **New-AzureRmRecoveryServicesVault** parancsmag toocreate egy új tárolót.</span><span class="sxs-lookup"><span data-stu-id="18749-127">Use hello **New-AzureRmRecoveryServicesVault** cmdlet toocreate a new vault.</span></span> <span data-ttu-id="18749-128">Győződjön meg arról, hogy toospecify hello hello tároló ugyanazon a helyen, mint az erőforráscsoport hello használt.</span><span class="sxs-lookup"><span data-stu-id="18749-128">Be sure toospecify hello same location for hello vault as was used for hello resource group.</span></span>

    ```
    PS C:\> New-AzureRmRecoveryServicesVault -Name "testvault" -ResourceGroupName " test-rg" -Location "West US"
    ```
4. <span data-ttu-id="18749-129">Adja meg a tárolási redundancia toouse; hello típusa használhat [helyileg redundáns tárolás (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) vagy [földrajzi redundáns tárolás (GRS)](../storage/common/storage-redundancy.md#geo-redundant-storage).</span><span class="sxs-lookup"><span data-stu-id="18749-129">Specify hello type of storage redundancy toouse; you can use [Locally Redundant Storage (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) or [Geo Redundant Storage (GRS)](../storage/common/storage-redundancy.md#geo-redundant-storage).</span></span> <span data-ttu-id="18749-130">hello következő példa bemutatja hello - BackupStorageRedundancy testVault vonatkozó beállítás tooGeoRedundant.</span><span class="sxs-lookup"><span data-stu-id="18749-130">hello following example shows hello -BackupStorageRedundancy option for testVault is set tooGeoRedundant.</span></span>

   > [!TIP]
   > <span data-ttu-id="18749-131">Sok Azure biztonsági mentést készítő parancsmagok bemenetként hello Recovery Services-tároló objektum szükséges.</span><span class="sxs-lookup"><span data-stu-id="18749-131">Many Azure Backup cmdlets require hello Recovery Services vault object as an input.</span></span> <span data-ttu-id="18749-132">Emiatt egy kényelmes toostore hello biztonsági mentést a Recovery Services tároló objektum egy változóban.</span><span class="sxs-lookup"><span data-stu-id="18749-132">For this reason, it is convenient toostore hello Backup Recovery Services vault object in a variable.</span></span>
   >
   >

    ```
    PS C:\> $vault1 = Get-AzureRmRecoveryServicesVault –Name "testVault"
    PS C:\> Set-AzureRmRecoveryServicesBackupProperties  -vault $vault1 -BackupStorageRedundancy GeoRedundant
    ```

## <a name="view-hello-vaults-in-a-subscription"></a><span data-ttu-id="18749-133">Nézet hello tárolók az előfizetés</span><span class="sxs-lookup"><span data-stu-id="18749-133">View hello vaults in a subscription</span></span>
<span data-ttu-id="18749-134">Használjon **Get-AzureRmRecoveryServicesVault** ebben az előfizetésben hello összes tárolók tooview hello listája.</span><span class="sxs-lookup"><span data-stu-id="18749-134">Use **Get-AzureRmRecoveryServicesVault** tooview hello list of all vaults in hello current subscription.</span></span> <span data-ttu-id="18749-135">Ez a parancs toocheck, hogy létrejött-e egy új tárolót vagy toosee milyen tárolók hello előfizetésben érhetők el.</span><span class="sxs-lookup"><span data-stu-id="18749-135">You can use this command toocheck that a new  vault was created, or toosee what vaults are available in hello subscription.</span></span>

<span data-ttu-id="18749-136">Hello parancs, a Get-AzureRmRecoveryServicesVault, és minden tárolók hello az előfizetéshez vannak felsorolva.</span><span class="sxs-lookup"><span data-stu-id="18749-136">Run hello command, Get-AzureRmRecoveryServicesVault, and all vaults in hello subscription are listed.</span></span>

```
PS C:\> Get-AzureRmRecoveryServicesVault
Name              : Contoso-vault
ID                : /subscriptions/1234
Type              : Microsoft.RecoveryServices/vaults
Location          : WestUS
ResourceGroupName : Contoso-docs-rg
SubscriptionId    : 1234-567f-8910-abc
Properties        : Microsoft.Azure.Commands.RecoveryServices.ARSVaultProperties
```


## <a name="installing-hello-azure-backup-agent-on-a-dpm-server"></a><span data-ttu-id="18749-137">Hello Azure Backup szolgáltatás ügynökének telepítése a DPM-kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="18749-137">Installing hello Azure Backup agent on a DPM Server</span></span>
<span data-ttu-id="18749-138">Hello Azure Backup szolgáltatás ügynökének telepítése előtt kell toohave hello telepítő letöltött és a Windows Server hello megtalálható.</span><span class="sxs-lookup"><span data-stu-id="18749-138">Before you install hello Azure Backup agent, you need toohave hello installer downloaded and present on hello Windows Server.</span></span> <span data-ttu-id="18749-139">Hello installer legújabb verzióját hello letölthető hello [Microsoft Download Center](http://aka.ms/azurebackup_agent) vagy hello Recovery Services tároló irányítópult-oldalon.</span><span class="sxs-lookup"><span data-stu-id="18749-139">You can get hello latest version of hello installer from hello [Microsoft Download Center](http://aka.ms/azurebackup_agent) or from hello Recovery Services vault's Dashboard page.</span></span> <span data-ttu-id="18749-140">Mentés hello telepítő tooan könnyen hozzáférhető helyen, például * C:\Downloads\*.</span><span class="sxs-lookup"><span data-stu-id="18749-140">Save hello installer tooan easily accessible location like *C:\Downloads\*.</span></span>

<span data-ttu-id="18749-141">tooinstall hello ügynök, futtassa a következő parancsot egy rendszergazda jogú PowerShell-konzolban hello **hello DPM-kiszolgálón**:</span><span class="sxs-lookup"><span data-stu-id="18749-141">tooinstall hello agent, run hello following command in an elevated PowerShell console **on hello DPM server**:</span></span>

```
PS C:\> MARSAgentInstaller.exe /q
```

<span data-ttu-id="18749-142">Ezzel telepít hello ügynök minden hello alapértelmezett beállításokkal.</span><span class="sxs-lookup"><span data-stu-id="18749-142">This installs hello agent with all hello default options.</span></span> <span data-ttu-id="18749-143">hello telepítési hello háttérben néhány percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="18749-143">hello installation takes a few minutes in hello background.</span></span> <span data-ttu-id="18749-144">Ha nem adja meg a hello */nu* beállítás hello **Windows Update** ablak hello telepítési toocheck bármely frissítések hello végén.</span><span class="sxs-lookup"><span data-stu-id="18749-144">If you do not specify hello */nu* option hello **Windows Update** window opens at hello end of hello installation toocheck for any updates.</span></span>

<span data-ttu-id="18749-145">hello ügynök telepített programok listájában hello mutatja.</span><span class="sxs-lookup"><span data-stu-id="18749-145">hello agent shows up in hello list of installed programs.</span></span> <span data-ttu-id="18749-146">a telepített programok toosee hello listája, nyissa meg túl**Vezérlőpult** > **programok** > **programok és szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="18749-146">toosee hello list of installed programs, go too**Control Panel** > **Programs** > **Programs and Features**.</span></span>

![Az ügynök telepítve](./media/backup-dpm-automation/installed-agent-listing.png)

### <a name="installation-options"></a><span data-ttu-id="18749-148">Telepítési beállítások</span><span class="sxs-lookup"><span data-stu-id="18749-148">Installation options</span></span>
<span data-ttu-id="18749-149">toosee hello commandline, a következő használatát hello keresztül elérhető hello beállítások parancsot:</span><span class="sxs-lookup"><span data-stu-id="18749-149">toosee all hello options available via hello commandline, use hello following command:</span></span>

```
PS C:\> MARSAgentInstaller.exe /?
```

<span data-ttu-id="18749-150">hello elérhető lehetőségek a következők:</span><span class="sxs-lookup"><span data-stu-id="18749-150">hello available options include:</span></span>

| <span data-ttu-id="18749-151">Beállítás</span><span class="sxs-lookup"><span data-stu-id="18749-151">Option</span></span> | <span data-ttu-id="18749-152">Részletek</span><span class="sxs-lookup"><span data-stu-id="18749-152">Details</span></span> | <span data-ttu-id="18749-153">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="18749-153">Default</span></span> |
| --- | --- | --- |
| <span data-ttu-id="18749-154">/q</span><span class="sxs-lookup"><span data-stu-id="18749-154">/q</span></span> |<span data-ttu-id="18749-155">Csendes telepítés</span><span class="sxs-lookup"><span data-stu-id="18749-155">Quiet installation</span></span> |- |
| <span data-ttu-id="18749-156">/ p: "hely"</span><span class="sxs-lookup"><span data-stu-id="18749-156">/p:"location"</span></span> |<span data-ttu-id="18749-157">Elérési út toohello telepítési mappáját hello Azure Backup szolgáltatás ügynöke.</span><span class="sxs-lookup"><span data-stu-id="18749-157">Path toohello installation folder for hello Azure Backup agent.</span></span> |<span data-ttu-id="18749-158">C:\Program Files\Microsoft Azure Recovery Services Agent ügynök</span><span class="sxs-lookup"><span data-stu-id="18749-158">C:\Program Files\Microsoft Azure Recovery Services Agent</span></span> |
| <span data-ttu-id="18749-159">/ s: "hely"</span><span class="sxs-lookup"><span data-stu-id="18749-159">/s:"location"</span></span> |<span data-ttu-id="18749-160">Elérési út toohello gyorsítótármappája hello Azure Backup szolgáltatás ügynöke.</span><span class="sxs-lookup"><span data-stu-id="18749-160">Path toohello cache folder for hello Azure Backup agent.</span></span> |<span data-ttu-id="18749-161">C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch</span><span class="sxs-lookup"><span data-stu-id="18749-161">C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch</span></span> |
| <span data-ttu-id="18749-162">/m</span><span class="sxs-lookup"><span data-stu-id="18749-162">/m</span></span> |<span data-ttu-id="18749-163">Részt tooMicrosoft frissítés</span><span class="sxs-lookup"><span data-stu-id="18749-163">Opt-in tooMicrosoft Update</span></span> |- |
| <span data-ttu-id="18749-164">/Nu</span><span class="sxs-lookup"><span data-stu-id="18749-164">/nu</span></span> |<span data-ttu-id="18749-165">Ne keressen frissítéseket telepítésének befejezése után</span><span class="sxs-lookup"><span data-stu-id="18749-165">Do not Check for updates after installation is complete</span></span> |- |
| <span data-ttu-id="18749-166">/d</span><span class="sxs-lookup"><span data-stu-id="18749-166">/d</span></span> |<span data-ttu-id="18749-167">Eltávolítja a Microsoft Azure Recovery Services Agent ügynök</span><span class="sxs-lookup"><span data-stu-id="18749-167">Uninstalls Microsoft Azure Recovery Services Agent</span></span> |- |
| <span data-ttu-id="18749-168">/pH</span><span class="sxs-lookup"><span data-stu-id="18749-168">/ph</span></span> |<span data-ttu-id="18749-169">Állomás proxycím</span><span class="sxs-lookup"><span data-stu-id="18749-169">Proxy Host Address</span></span> |- |
| <span data-ttu-id="18749-170">/po</span><span class="sxs-lookup"><span data-stu-id="18749-170">/po</span></span> |<span data-ttu-id="18749-171">Proxy Host Port száma</span><span class="sxs-lookup"><span data-stu-id="18749-171">Proxy Host Port Number</span></span> |- |
| <span data-ttu-id="18749-172">/Pu</span><span class="sxs-lookup"><span data-stu-id="18749-172">/pu</span></span> |<span data-ttu-id="18749-173">Proxy-állomás felhasználónév</span><span class="sxs-lookup"><span data-stu-id="18749-173">Proxy Host UserName</span></span> |- |
| <span data-ttu-id="18749-174">/pW</span><span class="sxs-lookup"><span data-stu-id="18749-174">/pw</span></span> |<span data-ttu-id="18749-175">Proxy jelszava</span><span class="sxs-lookup"><span data-stu-id="18749-175">Proxy Password</span></span> |- |

## <a name="registering-dpm-tooa-recovery-services-vault"></a><span data-ttu-id="18749-176">A DPM tooa Recovery Services-tároló regisztrálása</span><span class="sxs-lookup"><span data-stu-id="18749-176">Registering DPM tooa Recovery Services Vault</span></span>
<span data-ttu-id="18749-177">Hello Recovery Services-tároló létrehozását követően töltse le a legújabb ügynököt hello és hello tárolói hitelesítő adatokat, és C:\Downloads például egy tetszőleges helyen tárolja.</span><span class="sxs-lookup"><span data-stu-id="18749-177">After you created hello Recovery Services vault, download hello latest agent and hello vault credentials and store it in a convenient location like C:\Downloads.</span></span>

```
PS C:\> $credspath = "C:\downloads"
PS C:\> $credsfilename = Get-AzureRmRecoveryServicesVaultSettingsFile -Backup -Vault $vault1 -Path  $credspath
PS C:\> $credsfilename
C:\downloads\testvault\_Sun Apr 10 2016.VaultCredentials
```

<span data-ttu-id="18749-178">Hello DPM-kiszolgálón, futtassa a hello [Start-OBRegistration](https://technet.microsoft.com/library/hh770398%28v=wps.630%29.aspx) parancsmag tooregister hello gép hello tárolóban.</span><span class="sxs-lookup"><span data-stu-id="18749-178">On hello DPM server, run hello [Start-OBRegistration](https://technet.microsoft.com/library/hh770398%28v=wps.630%29.aspx) cmdlet tooregister hello machine with hello vault.</span></span>

```
PS C:\> $cred = $credspath + $credsfilename
PS C:\> Start-OBRegistration-VaultCredentials $cred -Confirm:$false
CertThumbprint      :7a2ef2caa2e74b6ed1222a5e89288ddad438df2
SubscriptionID      : ef4ab577-c2c0-43e4-af80-af49f485f3d1
ServiceResourceName: testvault
Region              :West US
Machine registration succeeded.
```

### <a name="initial-configuration-settings"></a><span data-ttu-id="18749-179">Kezdeti konfigurációs beállításai</span><span class="sxs-lookup"><span data-stu-id="18749-179">Initial configuration settings</span></span>
<span data-ttu-id="18749-180">Miután hello DPM-kiszolgáló regisztrálva van az hello Recovery Services-tároló, alapértelmezett előfizetési beállítások kezdődik.</span><span class="sxs-lookup"><span data-stu-id="18749-180">Once hello DPM Server is registered with hello Recovery Services vault, it starts with default subscription settings.</span></span> <span data-ttu-id="18749-181">Ezek előfizetési beállítások közé tartozik a hálózatkezelés, a titkosítás és a hello az átmeneti területről.</span><span class="sxs-lookup"><span data-stu-id="18749-181">These subscription settings include Networking, Encryption and hello Staging area.</span></span> <span data-ttu-id="18749-182">toochange előfizetési beállítások toofirst kell leírót szerezni hello meglévő (alapértelmezett) a beállításokat a hello [Get-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612793) parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="18749-182">toochange subscription settings you need toofirst get a handle on hello existing (default) settings using hello [Get-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612793) cmdlet:</span></span>

```
$setting = Get-DPMCloudSubscriptionSetting -DPMServerName "TestingServer"
```

<span data-ttu-id="18749-183">Minden módosítások készült toothis helyi PowerShell objektum ```$setting``` , és ezután hello teljes objektum véglegesített tooDPM és az Azure Backup toosave őket hello segítségével [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="18749-183">All modifications are made toothis local PowerShell object ```$setting```  and then hello full object is committed tooDPM and Azure Backup toosave them using hello [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet.</span></span> <span data-ttu-id="18749-184">Toouse hello kell ```–Commit``` jelző tooensure, amely hello módosítások megmaradnak.</span><span class="sxs-lookup"><span data-stu-id="18749-184">You need toouse hello ```–Commit``` flag tooensure that hello changes are persisted.</span></span> <span data-ttu-id="18749-185">hello-beállítások nem is alkalmazott és Azure Backup szolgáltatás csak a véglegesített használni.</span><span class="sxs-lookup"><span data-stu-id="18749-185">hello settings will not be applied and used by Azure Backup unless committed.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -Commit
```

## <a name="networking"></a><span data-ttu-id="18749-186">Hálózat</span><span class="sxs-lookup"><span data-stu-id="18749-186">Networking</span></span>
<span data-ttu-id="18749-187">Ha hello DPM gép toohello hello Azure biztonsági mentési szolgáltatás hello kapcsolatát internet proxykiszolgálón keresztül, majd hello proxykiszolgálót kell rendelkezni a sikeres biztonsági mentések.</span><span class="sxs-lookup"><span data-stu-id="18749-187">If hello connectivity of hello DPM machine toohello Azure Backup service on hello internet is through a proxy server, then hello proxy server settings should be provided for successful backups.</span></span> <span data-ttu-id="18749-188">Hello használata ehhez ```-ProxyServer```és ```-ProxyPort```, ```-ProxyUsername``` és hello ```ProxyPassword``` hello paramétereket [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="18749-188">This is done by using hello ```-ProxyServer```and ```-ProxyPort```, ```-ProxyUsername``` and hello ```ProxyPassword``` parameters with hello [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet.</span></span> <span data-ttu-id="18749-189">Ebben a példában nincs proxy kiszolgáló, explicit módon azt vannak törlésével bármely proxy kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="18749-189">In this example, there is no proxy server so we are explicitly clearing any proxy-related information.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -NoProxy
```

<span data-ttu-id="18749-190">Sávszélesség is szabályozható a beállítások a ```-WorkHourBandwidth``` és ```-NonWorkHourBandwidth``` egy adott objektumcsoporthoz hello hét nap.</span><span class="sxs-lookup"><span data-stu-id="18749-190">Bandwidth usage can also be controlled with options of ```-WorkHourBandwidth``` and ```-NonWorkHourBandwidth``` for a given set of days of hello week.</span></span> <span data-ttu-id="18749-191">Ebben a példában a Microsoft nem állítja a sávszélesség-szabályozás.</span><span class="sxs-lookup"><span data-stu-id="18749-191">In this example, we are not setting any throttling.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -NoThrottle
```

## <a name="configuring-hello-staging-area"></a><span data-ttu-id="18749-192">Az átmeneti területről hello konfigurálása</span><span class="sxs-lookup"><span data-stu-id="18749-192">Configuring hello staging Area</span></span>
<span data-ttu-id="18749-193">hello DPM-kiszolgálón futó hello Azure Backup ügynöknek szüksége van ideiglenes tárhelyre, (helyi átmeneti terület) hello felhőből helyreállított adatok számára.</span><span class="sxs-lookup"><span data-stu-id="18749-193">hello Azure Backup agent running on hello DPM server needs temporary storage for data restored from hello cloud (local staging area).</span></span> <span data-ttu-id="18749-194">Hello átmeneti terület használatával hello konfigurálása [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) parancsmag és hello ```-StagingAreaPath``` paraméter.</span><span class="sxs-lookup"><span data-stu-id="18749-194">Configure hello staging area using hello [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet and hello ```-StagingAreaPath``` parameter.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -StagingAreaPath "C:\StagingArea"
```

<span data-ttu-id="18749-195">Hello a fenti példában a hello átmeneti terület lesz beállítva, túl*C:\StagingArea* hello PowerShell objektumban ```$setting```.</span><span class="sxs-lookup"><span data-stu-id="18749-195">In hello example above, hello staging area will be set too*C:\StagingArea* in hello PowerShell object ```$setting```.</span></span> <span data-ttu-id="18749-196">Győződjön meg arról, hogy hello megadott mappa már létezik, vagy pedig hello előfizetési beállítások hello végső véglegesítése sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="18749-196">Ensure that hello specified folder already exists, or else hello final commit of hello subscription settings will fail.</span></span>

### <a name="encryption-settings"></a><span data-ttu-id="18749-197">Titkosítási beállítások</span><span class="sxs-lookup"><span data-stu-id="18749-197">Encryption settings</span></span>
<span data-ttu-id="18749-198">hello küldött biztonsági mentési adatok tooAzure biztonsági mentés titkosított tooprotect hello adatok bizalmas mivoltát hello.</span><span class="sxs-lookup"><span data-stu-id="18749-198">hello backup data sent tooAzure Backup is encrypted tooprotect hello confidentiality of hello data.</span></span> <span data-ttu-id="18749-199">hello titkosítási jelszó visszaállítása a hello időpontjában az hello "password" toodecrypt hello adatai.</span><span class="sxs-lookup"><span data-stu-id="18749-199">hello encryption passphrase is hello "password" toodecrypt hello data at hello time of restore.</span></span> <span data-ttu-id="18749-200">Fontos tookeep ezen információk biztonságos és biztonságos be van állítva.</span><span class="sxs-lookup"><span data-stu-id="18749-200">It is important tookeep this information safe and secure once it is set.</span></span>

<span data-ttu-id="18749-201">Hello az alábbi példában az első parancs hello alakít át hello ```passphrase123456789``` tooa biztonságos karakterlánc és rendel hello biztonságos karakterlánc toohello nevű változó ```$Passphrase```.</span><span class="sxs-lookup"><span data-stu-id="18749-201">In hello example below, hello first command converts hello string ```passphrase123456789``` tooa secure string and assigns hello secure string toohello variable named ```$Passphrase```.</span></span> <span data-ttu-id="18749-202">hello második parancs beállítja hello biztonságos karakterlánc ```$Passphrase``` hello jelszóként biztonsági mentések titkosításához.</span><span class="sxs-lookup"><span data-stu-id="18749-202">hello second command sets hello secure string in ```$Passphrase``` as hello password for encrypting backups.</span></span>

```
PS C:\> $Passphrase = ConvertTo-SecureString -string "passphrase123456789" -AsPlainText -Force

PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -EncryptionPassphrase $Passphrase
```

> [!IMPORTANT]
> <span data-ttu-id="18749-203">Biztosíthatja hello jelszót adatok biztonságos be van állítva.</span><span class="sxs-lookup"><span data-stu-id="18749-203">Keep hello passphrase information safe and secure once it is set.</span></span> <span data-ttu-id="18749-204">Csak akkor tudja toorestore adatokat az Azure-ból nélkül ezt a jelszót.</span><span class="sxs-lookup"><span data-stu-id="18749-204">You will not be able toorestore data from Azure without this passphrase.</span></span>
>
>

<span data-ttu-id="18749-205">Ezen a ponton az összes szükséges hello módosítások toohello el kell ```$setting``` objektum.</span><span class="sxs-lookup"><span data-stu-id="18749-205">At this point, you should have made all hello required changes toohello ```$setting``` object.</span></span> <span data-ttu-id="18749-206">Ne felejtse el toocommit hello módosításokat.</span><span class="sxs-lookup"><span data-stu-id="18749-206">Remember toocommit hello changes.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -Commit
```

## <a name="protect-data-tooazure-backup"></a><span data-ttu-id="18749-207">Biztonsági mentési adatok tooAzure védelme</span><span class="sxs-lookup"><span data-stu-id="18749-207">Protect data tooAzure Backup</span></span>
<span data-ttu-id="18749-208">Ebben a szakaszban egy üzemi kiszolgáló tooDPM hozzáadása, és majd védelme a DPM toolocal hello adattárolás és tooAzure biztonsági mentés.</span><span class="sxs-lookup"><span data-stu-id="18749-208">In this section, you will add a production server tooDPM and then protect hello data toolocal DPM storage and then tooAzure Backup.</span></span> <span data-ttu-id="18749-209">Hello példákban fogjuk mutatni, hogyan tooback fájlokat és mappákat.</span><span class="sxs-lookup"><span data-stu-id="18749-209">In hello examples, we will demonstrate how tooback up files and folders.</span></span> <span data-ttu-id="18749-210">hello logikát is egyszerű kiterjesztett toobackup minden DPM által támogatott adatforrás használni.</span><span class="sxs-lookup"><span data-stu-id="18749-210">hello logic can easily be extended toobackup any DPM-supported data source.</span></span> <span data-ttu-id="18749-211">A DPM biztonsági mentések által a védelmi csoport (PG) négy alkotórészek vonatkoznak:</span><span class="sxs-lookup"><span data-stu-id="18749-211">All your DPM backups are governed by a Protection Group (PG) with four parts:</span></span>

1. <span data-ttu-id="18749-212">**Csoport tagjai** összes hello védhető objektum listája (más néven *adatforrások* a DPM-ben), amelyet a hello tooprotect ugyanahhoz a védelmi csoporthoz.</span><span class="sxs-lookup"><span data-stu-id="18749-212">**Group members** is a list of all hello protectable objects (also known as *Datasources* in DPM) that you want tooprotect in hello same protection group.</span></span> <span data-ttu-id="18749-213">Például érdemes lehet tooprotect üzemi virtuális gépek egy védelmi csoport és egy másik védelmi csoportban található SQL Server-adatbázisok különböző biztonsági követelményeket is rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="18749-213">For example, you may want tooprotect production VMs in one protection group and SQL Server databases in another protection group as they may have different backup requirements.</span></span> <span data-ttu-id="18749-214">Biztonsági másolatot készíthet a datasource egy üzemi kiszolgálón meg kell, hogy hello toomake a DPM-ügynök hello kiszolgálóra van telepítve, és a DPM által kezelt.</span><span class="sxs-lookup"><span data-stu-id="18749-214">Before you can back up any datasource on a production server you need toomake sure hello DPM Agent is installed on hello server and is managed by DPM.</span></span> <span data-ttu-id="18749-215">Hello utasításai [a DPM-ügynök telepítése hello](https://technet.microsoft.com/library/bb870935.aspx) és toohello kapcsolásával végezze el a megfelelő DPM-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="18749-215">Follow hello steps for [installing hello DPM Agent](https://technet.microsoft.com/library/bb870935.aspx) and linking it toohello appropriate DPM Server.</span></span>
2. <span data-ttu-id="18749-216">**Adatvédelmi módszer** hello biztonsági mentési célhelyek - szalag, a lemez és a felhő határozza meg.</span><span class="sxs-lookup"><span data-stu-id="18749-216">**Data protection method** specifies hello target backup locations - tape, disk, and cloud.</span></span> <span data-ttu-id="18749-217">Ebben a példában a Microsoft által védendő adatok toohello helyi lemez- és toohello-alapú.</span><span class="sxs-lookup"><span data-stu-id="18749-217">In our example we will protect data toohello local disk and toohello cloud.</span></span>
3. <span data-ttu-id="18749-218">A **biztonsági mentés ütemezése** , amely megadja, hogy ha a biztonsági mentések kell venni toobe, és milyen gyakran hello szinkronizálja az adatokat a DPM-kiszolgáló hello és hello az üzemi kiszolgáló között.</span><span class="sxs-lookup"><span data-stu-id="18749-218">A **backup schedule** that specifies when backups need toobe taken and how often hello data should be synchronized between hello DPM Server and hello production server.</span></span>
4. <span data-ttu-id="18749-219">A **adatmegőrzési ütemterv** , amely meghatározza, mennyi ideig tooretain hello helyreállítási pontok az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="18749-219">A **retention schedule** that specifies how long tooretain hello recovery points in Azure.</span></span>

### <a name="creating-a-protection-group"></a><span data-ttu-id="18749-220">Védelmi csoport létrehozásakor</span><span class="sxs-lookup"><span data-stu-id="18749-220">Creating a protection group</span></span>
<span data-ttu-id="18749-221">Először hozzon létre egy új védelmi csoportot a hello [New-DPMProtectionGroup](https://technet.microsoft.com/library/hh881722) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="18749-221">Start by creating a new Protection Group using hello [New-DPMProtectionGroup](https://technet.microsoft.com/library/hh881722) cmdlet.</span></span>

```
PS C:\> $PG = New-DPMProtectionGroup -DPMServerName " TestingServer " -Name "ProtectGroup01"
```

<span data-ttu-id="18749-222">hello fent parancsmag létrehoz egy védelmi csoport neve *ProtectGroup01*.</span><span class="sxs-lookup"><span data-stu-id="18749-222">hello above cmdlet will create a Protection Group named *ProtectGroup01*.</span></span> <span data-ttu-id="18749-223">Egy meglévő védelmi csoport is módosíthatja újabb tooadd biztonsági mentési toohello Azure felhőben.</span><span class="sxs-lookup"><span data-stu-id="18749-223">An existing protection group can also be modified later tooadd backup toohello Azure cloud.</span></span> <span data-ttu-id="18749-224">Azonban toomake módosításokat toohello új védelmi csoport - vagy a meglévő - igazolnia kell a tooget leírót egy *módosíthatóvá* hello használó [Get-DPMModifiableProtectionGroup](https://technet.microsoft.com/library/hh881713) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="18749-224">However, toomake any changes toohello Protection Group - new or existing - we need tooget a handle on a *modifiable* object using hello [Get-DPMModifiableProtectionGroup](https://technet.microsoft.com/library/hh881713) cmdlet.</span></span>

```
PS C:\> $MPG = Get-ModifiableProtectionGroup $PG
```

### <a name="adding-group-members-toohello-protection-group"></a><span data-ttu-id="18749-225">Csoport tagjainak toohello védelmi csoport hozzáadása</span><span class="sxs-lookup"><span data-stu-id="18749-225">Adding group members toohello Protection Group</span></span>
<span data-ttu-id="18749-226">Minden DPM-ügynök tudja hello listája adatforrások hello kiszolgálón, amelyre telepítve van.</span><span class="sxs-lookup"><span data-stu-id="18749-226">Each DPM Agent knows hello list of datasources on hello server that it is installed on.</span></span> <span data-ttu-id="18749-227">tooadd egy adatforrás toohello védelmi csoport, a DPM-ügynök igények toofirst hello hello adatforrások hátsó toohello DPM-kiszolgáló listájának küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="18749-227">tooadd a datasource toohello Protection Group, hello DPM Agent needs toofirst send a list of hello datasources back toohello DPM server.</span></span> <span data-ttu-id="18749-228">Egy vagy több adatforrás nem, akkor a kiválasztott és toohello védelmi csoporthoz hozzáadott.</span><span class="sxs-lookup"><span data-stu-id="18749-228">One or more datasources are then selected and added toohello Protection Group.</span></span> <span data-ttu-id="18749-229">hello PowerShell lépésekre tooachieve ez van szükség:</span><span class="sxs-lookup"><span data-stu-id="18749-229">hello PowerShell steps needed tooachieve this are:</span></span>

1. <span data-ttu-id="18749-230">Keresztül hello DPM-ügynököt a DPM által kezelt összes kiszolgálók listájának beolvasása.</span><span class="sxs-lookup"><span data-stu-id="18749-230">Fetch a list of all servers managed by DPM through hello DPM Agent.</span></span>
2. <span data-ttu-id="18749-231">Válasszon egy adott kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="18749-231">Choose a specific server.</span></span>
3. <span data-ttu-id="18749-232">Hello kiszolgálón az összes adatforrás listájának beolvasása.</span><span class="sxs-lookup"><span data-stu-id="18749-232">Fetch a list of all datasources on hello server.</span></span>
4. <span data-ttu-id="18749-233">Válasszon egy vagy több adatforrást, és vegye fel őket a védelmi csoport toohello</span><span class="sxs-lookup"><span data-stu-id="18749-233">Choose one or more datasources and add them toohello Protection Group</span></span>

<span data-ttu-id="18749-234">mely hello a DPM-ügynök telepítve van, és hello DPM-kiszolgáló által kezelt kiszolgálók listájában hello hello van megszerezve [Get-DPMProductionServer](https://technet.microsoft.com/library/hh881600) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="18749-234">hello list of servers on which hello DPM Agent is installed and is being managed by hello DPM Server is acquired with hello [Get-DPMProductionServer](https://technet.microsoft.com/library/hh881600) cmdlet.</span></span> <span data-ttu-id="18749-235">Ebben a példában azt szűrésére és konfigurálását elvégzi, csak PS nevű *productionserver01* a biztonsági mentéshez.</span><span class="sxs-lookup"><span data-stu-id="18749-235">In this example we will filter and only configure PS with name *productionserver01* for backup.</span></span>

```
PS C:\> $server = Get-ProductionServer -DPMServerName "TestingServer" | where {($_.servername) –contains “productionserver01”
```

<span data-ttu-id="18749-236">Most beolvasni adatforrások listája hello ```$server``` hello segítségével [Get-DPMDatasource](https://technet.microsoft.com/library/hh881605) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="18749-236">Now fetch hello list of datasources on ```$server``` using hello [Get-DPMDatasource](https://technet.microsoft.com/library/hh881605) cmdlet.</span></span> <span data-ttu-id="18749-237">Ebben a példában a Microsoft jelenleg korlátozza a hello kötet * D:\* tooconfigure szeretnénk a biztonsági mentéshez.</span><span class="sxs-lookup"><span data-stu-id="18749-237">In this example we are filtering for hello volume *D:\* that we want tooconfigure for backup.</span></span> <span data-ttu-id="18749-238">Ez az adatforrás kerül toohello védelmi csoport használatával hello [Add-DPMChildDatasource](https://technet.microsoft.com/library/hh881732) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="18749-238">This datasource is then added toohello Protection Group using hello [Add-DPMChildDatasource](https://technet.microsoft.com/library/hh881732) cmdlet.</span></span> <span data-ttu-id="18749-239">Ne feledje toouse hello *módosíthatóvá* védelmi csoport objektum ```$MPG``` toomake hello kiegészítéseit.</span><span class="sxs-lookup"><span data-stu-id="18749-239">Remember toouse hello *modifiable* protection group object ```$MPG``` toomake hello additions.</span></span>

```
PS C:\> $DS = Get-Datasource -ProductionServer $server -Inquire | where { $_.Name -contains “D:\” }

PS C:\> Add-DPMChildDatasource -ProtectionGroup $MPG -ChildDatasource $DS
```

<span data-ttu-id="18749-240">Ismételje meg ezt a lépést, ha szükséges, ahányszor, amíg a kiválasztott adatforrások toohello védelmi csoport összes hello hozzáadását.</span><span class="sxs-lookup"><span data-stu-id="18749-240">Repeat this step as many times as required, until you have added all hello chosen datasources toohello protection group.</span></span> <span data-ttu-id="18749-241">Is csak egy adatforrást, és teljes hello munkafolyamat hello védelmi csoport létrehozásához kezdődnie, és később adja hozzá a további adatforrások toohello védelmi csoportot.</span><span class="sxs-lookup"><span data-stu-id="18749-241">You can also start with just one datasource, and complete hello workflow for creating hello Protection Group, and at a later point add more datasources toohello Protection Group.</span></span>

### <a name="selecting-hello-data-protection-method"></a><span data-ttu-id="18749-242">Hello adatvédelmi módszer kiválasztása</span><span class="sxs-lookup"><span data-stu-id="18749-242">Selecting hello data protection method</span></span>
<span data-ttu-id="18749-243">Hello adatforrások toohello védelmi csoporthoz lettek hozzáadva, miután hello következő lépésre-e toospecify hello védelmi módszer használatával hello [Set-DPMProtectionType](https://technet.microsoft.com/library/hh881725) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="18749-243">Once hello datasources have been added toohello Protection Group, hello next step is toospecify hello protection method using hello [Set-DPMProtectionType](https://technet.microsoft.com/library/hh881725) cmdlet.</span></span> <span data-ttu-id="18749-244">Ebben a példában a védelmi csoport hello beállítva helyi lemezek és felhőbeli biztonsági mentését.</span><span class="sxs-lookup"><span data-stu-id="18749-244">In this example, hello Protection Group is setup for local disk and cloud backup.</span></span> <span data-ttu-id="18749-245">Emellett szükség van, amelyet az hello segítségével tooprotect toocloud toospecify hello datasource [Add-DPMChildDatasource](https://technet.microsoft.com/library/hh881732.aspx) jelölővel - Online parancsmag.</span><span class="sxs-lookup"><span data-stu-id="18749-245">You also need toospecify hello datasource that you want tooprotect toocloud using hello [Add-DPMChildDatasource](https://technet.microsoft.com/library/hh881732.aspx) cmdlet with -Online flag.</span></span>

```
PS C:\> Set-DPMProtectionType -ProtectionGroup $MPG -ShortTerm Disk –LongTerm Online
PS C:\> Add-DPMChildDatasource -ProtectionGroup $MPG -ChildDatasource $DS –Online
```

### <a name="setting-hello-retention-range"></a><span data-ttu-id="18749-246">Hello megőrzési tartomány beállítása</span><span class="sxs-lookup"><span data-stu-id="18749-246">Setting hello retention range</span></span>
<span data-ttu-id="18749-247">Hello megőrzési hello biztonsági mentési pontok használatával hello beállítása [Set-DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="18749-247">Set hello retention for hello backup points using hello [Set-DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) cmdlet.</span></span> <span data-ttu-id="18749-248">Amíg páratlan tooset hello megőrzési tűnhet, mielőtt hello biztonsági mentési ütemezés definiálása hello segítségével ```Set-DPMPolicyObjective``` parancsmag automatikusan úgy állítja be a alapértelmezett biztonsági mentés ütemezését, majd módosítható.</span><span class="sxs-lookup"><span data-stu-id="18749-248">While it might seem odd tooset hello retention before hello backup schedule has been defined, using hello ```Set-DPMPolicyObjective``` cmdlet automatically sets a default backup schedule that can then be modified.</span></span> <span data-ttu-id="18749-249">Mindig lehetséges tooset hello biztonsági mentés ütemezése először és hello adatmegőrzési után.</span><span class="sxs-lookup"><span data-stu-id="18749-249">It is always possible tooset hello backup schedule first and hello retention policy after.</span></span>

<span data-ttu-id="18749-250">Hello az alábbi példában a hello parancsmag hello megőrzési paramétereinek beállítása lemezes biztonsági mentések.</span><span class="sxs-lookup"><span data-stu-id="18749-250">In hello example below, hello cmdlet sets hello retention parameters for disk backups.</span></span> <span data-ttu-id="18749-251">Ez megőrzi biztonsági mentések 10 nap, és szinkronizálja az adatokat az üzemi kiszolgáló hello és hello DPM-kiszolgáló közötti 6 óránként.</span><span class="sxs-lookup"><span data-stu-id="18749-251">This will retain backups for 10 days, and sync data every 6 hours between hello production server and hello DPM server.</span></span> <span data-ttu-id="18749-252">Hello ```SynchronizationFrequencyMinutes``` nem adja meg, milyen gyakran egy biztonsági mentési pont jön létre, de milyen gyakran adata másolt toohello DPM-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="18749-252">hello ```SynchronizationFrequencyMinutes``` doesn't define how often a backup point is created, but how often data is copied toohello DPM server.</span></span>  <span data-ttu-id="18749-253">Ez a beállítás megakadályozza, hogy a biztonsági mentések túl nagy.</span><span class="sxs-lookup"><span data-stu-id="18749-253">This setting prevents backups from becoming too large.</span></span>

```
PS C:\> Set-DPMPolicyObjective –ProtectionGroup $MPG -RetentionRangeInDays 10 -SynchronizationFrequencyMinutes 360
```

<span data-ttu-id="18749-254">A biztonsági mentésekhez tooAzure (DPM hivatkozik toothem az Online biztonsági mentés másként) fog hello megőrzési időtartamokat konfigurálhatja a [hosszú távon szerzett-Édesapja-fia megjelenítve (GFS) megőrzési](backup-azure-backup-cloud-as-tape.md).</span><span class="sxs-lookup"><span data-stu-id="18749-254">For backups going tooAzure (DPM refers toothem as Online backups) hello retention ranges can be configured for [long term retention using a Grandfather-Father-Son scheme (GFS)](backup-azure-backup-cloud-as-tape.md).</span></span> <span data-ttu-id="18749-255">Ez azt jelenti, hogy napi, heti, havi és éves adatmegőrzési szabályoknál érintő kombinált adatmegőrzési adhat meg.</span><span class="sxs-lookup"><span data-stu-id="18749-255">That is, you can define a combined retention policy involving daily, weekly, monthly and yearly retention policies.</span></span> <span data-ttu-id="18749-256">Ebben a példában azt szeretnénk hello összetett megőrzési séma képviselő tömböt létrehozása, és adja meg hello megőrzési tartományt hello [Set-DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="18749-256">In this example, we create an array representing hello complex retention scheme that we want, and then configure hello retention range using hello [Set-DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) cmdlet.</span></span>

```
PS C:\> $RRlist = @()
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 180, Days)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 104, Weeks)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 60, Month)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 10, Years)
PS C:\> Set-DPMPolicyObjective –ProtectionGroup $MPG -OnlineRetentionRangeList $RRlist
```

### <a name="set-hello-backup-schedule"></a><span data-ttu-id="18749-257">Hello biztonsági mentési ütemezés szerint</span><span class="sxs-lookup"><span data-stu-id="18749-257">Set hello backup schedule</span></span>
<span data-ttu-id="18749-258">A DPM automatikusan beállítja, egy alapértelmezett biztonsági mentés ütemezése hello védelmi célok hello segítségével noconnection ```Set-DPMPolicyObjective``` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="18749-258">DPM sets a default backup schedule automatically if you specify hello protection objective using hello ```Set-DPMPolicyObjective``` cmdlet.</span></span> <span data-ttu-id="18749-259">toochange hello alapértelmezett ütemtervét, használja a hello [Get-DPMPolicySchedule](https://technet.microsoft.com/library/hh881749) parancsmag követ hello [Set-DPMPolicySchedule](https://technet.microsoft.com/library/hh881723) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="18749-259">toochange hello default schedules, use hello [Get-DPMPolicySchedule](https://technet.microsoft.com/library/hh881749) cmdlet followed by hello [Set-DPMPolicySchedule](https://technet.microsoft.com/library/hh881723) cmdlet.</span></span>

```
PS C:\> $onlineSch = Get-DPMPolicySchedule -ProtectionGroup $mpg -LongTerm Online
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[0] -TimesOfDay 02:00
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[1] -TimesOfDay 02:00 -DaysOfWeek Sa,Su –Interval 1
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[2] -TimesOfDay 02:00 -RelativeIntervals First,Third –DaysOfWeek Sa
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[3] -TimesOfDay 02:00 -DaysOfMonth 2,5,8,9 -Months Jan,Jul
PS C:\> Set-DPMProtectionGroup -ProtectionGroup $MPG
```

<span data-ttu-id="18749-260">A fenti példában hello ```$onlineSch``` tömb négy elemekkel, amely tartalmazza a meglévő online védelmi ütemterv hello hello védelmi csoport hello GFS rendszerben:</span><span class="sxs-lookup"><span data-stu-id="18749-260">In hello above example, ```$onlineSch``` is an array with four elements that contains hello existing online protection schedule for hello Protection Group in hello GFS scheme:</span></span>

1. <span data-ttu-id="18749-261">```$onlineSch[0]```napi ütemezés hello tartalmazza</span><span class="sxs-lookup"><span data-stu-id="18749-261">```$onlineSch[0]``` contains hello daily schedule</span></span>
2. <span data-ttu-id="18749-262">```$onlineSch[1]```hello heti ütemezés tartalmazza</span><span class="sxs-lookup"><span data-stu-id="18749-262">```$onlineSch[1]``` contains hello weekly schedule</span></span>
3. <span data-ttu-id="18749-263">```$onlineSch[2]```hello havi ütemezés tartalmazza</span><span class="sxs-lookup"><span data-stu-id="18749-263">```$onlineSch[2]``` contains hello monthly schedule</span></span>
4. <span data-ttu-id="18749-264">```$onlineSch[3]```hello éves ütemezés tartalmazza</span><span class="sxs-lookup"><span data-stu-id="18749-264">```$onlineSch[3]``` contains hello yearly schedule</span></span>

<span data-ttu-id="18749-265">Így ha toomodify hello heti ütemezés van szüksége, toorefer toohello ```$onlineSch[1]```.</span><span class="sxs-lookup"><span data-stu-id="18749-265">So if you need toomodify hello weekly schedule, you need toorefer toohello ```$onlineSch[1]```.</span></span>

### <a name="initial-backup"></a><span data-ttu-id="18749-266">Kezdeti biztonsági mentés</span><span class="sxs-lookup"><span data-stu-id="18749-266">Initial backup</span></span>
<span data-ttu-id="18749-267">Ha biztonsági mentése a hello datasource először, a DPM igényeinek hoz létre a kezdeti replika, amely létrehozza a hello datasource toobe védett DPM-replikakötet teljes másolatát.</span><span class="sxs-lookup"><span data-stu-id="18749-267">When backing up a datasource for hello first time, DPM needs creates initial replica that creates a full copy of hello datasource toobe protected on DPM replica volume.</span></span> <span data-ttu-id="18749-268">Ez a tevékenység vagy egy adott időpont ütemezhetők, vagy manuálisan is elindítható, hello segítségével [Set-DPMReplicaCreationMethod](https://technet.microsoft.com/library/hh881715) hello paraméterrel parancsmag ```-NOW```.</span><span class="sxs-lookup"><span data-stu-id="18749-268">This activity can either be scheduled for a specific time, or can be triggered manually, using hello [Set-DPMReplicaCreationMethod](https://technet.microsoft.com/library/hh881715) cmdlet with hello parameter ```-NOW```.</span></span>

```
PS C:\> Set-DPMReplicaCreationMethod -ProtectionGroup $MPG -NOW
```
### <a name="changing-hello-size-of-dpm-replica--recovery-point-volume"></a><span data-ttu-id="18749-269">DPM-replika & a helyreállításipont-kötet hello méretének módosítása</span><span class="sxs-lookup"><span data-stu-id="18749-269">Changing hello size of DPM Replica & recovery point volume</span></span>
<span data-ttu-id="18749-270">Hello méretét a DPM-replikakötet és árnyékmásolat-kötet használatával is módosíthatja [Set-DPMDatasourceDiskAllocation](https://technet.microsoft.com/library/hh881618.aspx) parancsmag, mint például a következő hello: Get-DatasourceDiskAllocation - Datasource $DS Set-datasourcediskallocation segédprogram - Datasource $DS - ProtectionGroup $MPG-manuális - ReplicaArea (2 gb) – ShadowCopyArea (2 gb)</span><span class="sxs-lookup"><span data-stu-id="18749-270">You can also change hello size of DPM Replica volume and Shadow Copy volume using [Set-DPMDatasourceDiskAllocation](https://technet.microsoft.com/library/hh881618.aspx) cmdlet as in hello following example: Get-DatasourceDiskAllocation -Datasource $DS Set-DatasourceDiskAllocation -Datasource $DS -ProtectionGroup $MPG -manual -ReplicaArea (2gb) -ShadowCopyArea (2gb)</span></span>

### <a name="committing-hello-changes-toohello-protection-group"></a><span data-ttu-id="18749-271">Hello véglegesítése toohello védelmi csoport módosítása</span><span class="sxs-lookup"><span data-stu-id="18749-271">Committing hello changes toohello Protection Group</span></span>
<span data-ttu-id="18749-272">Végezetül hello módosításokat kell a DPM hello biztonsági mentés / hello új védelmi csoport konfigurációs megkezdése előtt véglegesítése toobe.</span><span class="sxs-lookup"><span data-stu-id="18749-272">Finally, hello changes need toobe committed before DPM can take hello backup per hello new Protection Group configuration.</span></span> <span data-ttu-id="18749-273">Ez megvalósítható hello segítségével [Set-DPMProtectionGroup](https://technet.microsoft.com/library/hh881758) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="18749-273">This can be achieved using hello [Set-DPMProtectionGroup](https://technet.microsoft.com/library/hh881758) cmdlet.</span></span>

```
PS C:\> Set-DPMProtectionGroup -ProtectionGroup $MPG
```
## <a name="view-hello-backup-points"></a><span data-ttu-id="18749-274">Nézet hello biztonsági mentési pontok</span><span class="sxs-lookup"><span data-stu-id="18749-274">View hello backup points</span></span>
<span data-ttu-id="18749-275">Használhatja a hello [Get-DPMRecoveryPoint](https://technet.microsoft.com/library/hh881746) parancsmag tooget egy adatforrás összes helyreállítási pont listáját.</span><span class="sxs-lookup"><span data-stu-id="18749-275">You can use hello [Get-DPMRecoveryPoint](https://technet.microsoft.com/library/hh881746) cmdlet tooget a list of all recovery points for a datasource.</span></span> <span data-ttu-id="18749-276">Ebben a példában a következő történik:</span><span class="sxs-lookup"><span data-stu-id="18749-276">In this example, we will:</span></span>

* <span data-ttu-id="18749-277">lehívási összes PGs hello hello DPM-kiszolgálón, és egy tömbben található```$PG```</span><span class="sxs-lookup"><span data-stu-id="18749-277">fetch all hello PGs on hello DPM server and stored in an array ```$PG```</span></span>
* <span data-ttu-id="18749-278">hello adatforrások megfelelő toohello beolvasása```$PG[0]```</span><span class="sxs-lookup"><span data-stu-id="18749-278">get hello datasources corresponding toohello ```$PG[0]```</span></span>
* <span data-ttu-id="18749-279">egy adatforrás összes hello helyreállítási pont beolvasása.</span><span class="sxs-lookup"><span data-stu-id="18749-279">get all hello recovery points for a datasource.</span></span>

```
PS C:\> $PG = Get-DPMProtectionGroup –DPMServerName "TestingServer"
PS C:\> $DS = Get-DPMDatasource -ProtectionGroup $PG[0]
PS C:\> $RecoveryPoints = Get-DPMRecoverypoint -Datasource $DS[0] -Online
```

## <a name="restore-data-protected-on-azure"></a><span data-ttu-id="18749-280">Az Azure-on védett adatok visszaállítása</span><span class="sxs-lookup"><span data-stu-id="18749-280">Restore data protected on Azure</span></span>
<span data-ttu-id="18749-281">Adat-visszaállítást a rendszer kombinációja egy ```RecoverableItem``` objektum és a ```RecoveryOption``` objektum.</span><span class="sxs-lookup"><span data-stu-id="18749-281">Restoring data is a combination of a ```RecoverableItem``` object and a ```RecoveryOption``` object.</span></span> <span data-ttu-id="18749-282">Hello előző szakaszban egy adatforrás azt kapott hello biztonsági mentési pontok listáját.</span><span class="sxs-lookup"><span data-stu-id="18749-282">In hello previous section, we got a list of hello backup points for a datasource.</span></span>

<span data-ttu-id="18749-283">A hello az alábbi példában bemutatjuk, hogyan toorestore egy Hyper-V virtuális gép az Azure Backup szolgáltatás biztonsági mentési pontok egyesítő hello helyreállítási célként.</span><span class="sxs-lookup"><span data-stu-id="18749-283">In hello example below, we demonstrate how toorestore a Hyper-V virtual machine from Azure Backup by combining backup points with hello target for recovery.</span></span> <span data-ttu-id="18749-284">Ebben a példában a következőket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="18749-284">This example includes:</span></span>

* <span data-ttu-id="18749-285">A helyreállítási lehetőséget a hello segítségével létrehozása [New-DPMRecoveryOption](https://technet.microsoft.com/library/hh881592) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="18749-285">Creating a recovery option using hello  [New-DPMRecoveryOption](https://technet.microsoft.com/library/hh881592) cmdlet.</span></span>
* <span data-ttu-id="18749-286">Hello segítségével biztonsági mentési pontok tömbjének lekérdezésekor hello ```Get-DPMRecoveryPoint``` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="18749-286">Fetching hello array of backup points using hello ```Get-DPMRecoveryPoint``` cmdlet.</span></span>
* <span data-ttu-id="18749-287">A biztonsági mentési pont toorestore a kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="18749-287">Choosing a backup point toorestore from.</span></span>

```
PS C:\> $RecoveryOption = New-DPMRecoveryOption -HyperVDatasource -TargetServer "HVDCenter02" -RecoveryLocation AlternateHyperVServer -RecoveryType Recover -TargetLocation “C:\VMRecovery”

PS C:\> $PG = Get-DPMProtectionGroup –DPMServerName "TestingServer"
PS C:\> $DS = Get-DPMDatasource -ProtectionGroup $PG[0]
PS C:\> $RecoveryPoints = Get-DPMRecoverypoint -Datasource $DS[0] -Online

PS C:\> Restore-DPMRecoverableItem -RecoverableItem $RecoveryPoints[0] -RecoveryOption $RecoveryOption
```

<span data-ttu-id="18749-288">hello parancsok könnyen bővíthető bármely adatforrástípusnál.</span><span class="sxs-lookup"><span data-stu-id="18749-288">hello commands can easily be extended for any datasource type.</span></span>

## <a name="next-steps"></a><span data-ttu-id="18749-289">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="18749-289">Next steps</span></span>
* <span data-ttu-id="18749-290">További információ a DPM biztonsági mentés tooAzure lásd [bemutatása tooDPM biztonsági mentése](backup-azure-dpm-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="18749-290">For more information about DPM tooAzure Backup see [Introduction tooDPM Backup](backup-azure-dpm-introduction.md)</span></span>
