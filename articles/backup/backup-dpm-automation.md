---
title: "Az Azure Backup - a PowerShell szolgáltatás használatával a DPM-munkaterhelések biztonsági mentése |} Microsoft Docs"
description: "Megtudhatja, hogyan telepíthetnek és kezelhetnek az Azure Backup a Data Protection Manager (DPM) PowerShell használatával"
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
ms.openlocfilehash: 2e3b4a094511a59cfa02917efc2e3e053840af0c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-and-manage-backup-to-azure-for-data-protection-manager-dpm-servers-using-powershell"></a><span data-ttu-id="96d89-103">Az Azure-ba történő biztonsági mentés üzembe helyezése és kezelése DPM-kiszolgálókon a PowerShell-lel</span><span class="sxs-lookup"><span data-stu-id="96d89-103">Deploy and manage backup to Azure for Data Protection Manager (DPM) servers using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="96d89-104">ARM</span><span class="sxs-lookup"><span data-stu-id="96d89-104">ARM</span></span>](backup-dpm-automation.md)
> * [<span data-ttu-id="96d89-105">Klasszikus</span><span class="sxs-lookup"><span data-stu-id="96d89-105">Classic</span></span>](backup-dpm-automation-classic.md)
>
>

<span data-ttu-id="96d89-106">Ez a cikk bemutatja, hogyan PowerShell használatával történő telepítés Azure biztonsági mentés DPM-kiszolgálón, és kezelheti a biztonsági mentés és helyreállítás.</span><span class="sxs-lookup"><span data-stu-id="96d89-106">This article shows you how to use PowerShell to setup Azure Backup on a DPM server, and to manage backup and recovery.</span></span>

## <a name="setting-up-the-powershell-environment"></a><span data-ttu-id="96d89-107">A PowerShell-környezet létrehozása</span><span class="sxs-lookup"><span data-stu-id="96d89-107">Setting up the PowerShell environment</span></span>
[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-include.md)]

<span data-ttu-id="96d89-108">Mielőtt a PowerShell használatával a Data Protection Manager biztonsági mentések kezelése az Azure-ba, szüksége a megfelelő környezete van, a PowerShell.</span><span class="sxs-lookup"><span data-stu-id="96d89-108">Before you can use PowerShell to manage backups from Data Protection Manager to Azure, you need to have the right environment in PowerShell.</span></span> <span data-ttu-id="96d89-109">A PowerShell-munkamenet elején győződjön meg arról, hogy futtatja a következő parancsot a megfelelő modul importálása és engedélyezi, hogy helyesen hivatkozik a DPM-parancsmagokat:</span><span class="sxs-lookup"><span data-stu-id="96d89-109">At the start of the PowerShell session, ensure that you run the following command to import the right modules and allow you to correctly reference the DPM cmdlets:</span></span>

```
PS C:> & "C:\Program Files\Microsoft System Center 2012 R2\DPM\DPM\bin\DpmCliInitScript.ps1"

Welcome to the DPM Management Shell!

Full list of cmdlets: Get-Command
Only DPM cmdlets: Get-DPMCommand
Get general help: help
Get help for a cmdlet: help <cmdlet-name> or <cmdlet-name> -?
Get definition of a cmdlet: Get-Command <cmdlet-name> -Syntax
Sample DPM scripts: Get-DPMSampleScript
```

## <a name="setup-and-registration"></a><span data-ttu-id="96d89-110">Telepítését és regisztrálását</span><span class="sxs-lookup"><span data-stu-id="96d89-110">Setup and Registration</span></span>
<span data-ttu-id="96d89-111">Megkezdéséhez:</span><span class="sxs-lookup"><span data-stu-id="96d89-111">To begin:</span></span>

1. <span data-ttu-id="96d89-112">[Töltse le a legfrissebb PowerShell](https://github.com/Azure/azure-powershell/releases) (szükséges minimális verziója: 1.0.0)</span><span class="sxs-lookup"><span data-stu-id="96d89-112">[Download latest PowerShell](https://github.com/Azure/azure-powershell/releases) (minimum version required is: 1.0.0)</span></span>
2. <span data-ttu-id="96d89-113">Engedélyezze az Azure Backup szolgáltatás parancsmagjaival átváltás *AzureResourceManager* mód használatával a **Switch-AzureMode** parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="96d89-113">Enable the Azure Backup commandlets by switching to *AzureResourceManager* mode by using the **Switch-AzureMode** commandlet:</span></span>

```
PS C:\> Switch-AzureMode AzureResourceManager
```

<span data-ttu-id="96d89-114">A következő telepítését és regisztrálását feladatok automatizálhatók a PowerShell használatával:</span><span class="sxs-lookup"><span data-stu-id="96d89-114">The following setup and registration tasks can be automated with PowerShell:</span></span>

* <span data-ttu-id="96d89-115">Recovery Services-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="96d89-115">Create a Recovery Services vault</span></span>
* <span data-ttu-id="96d89-116">Az Azure Backup szolgáltatás ügynökének telepítése</span><span class="sxs-lookup"><span data-stu-id="96d89-116">Installing the Azure Backup agent</span></span>
* <span data-ttu-id="96d89-117">Az Azure Backup szolgáltatás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="96d89-117">Registering with the Azure Backup service</span></span>
* <span data-ttu-id="96d89-118">Hálózati beállítások</span><span class="sxs-lookup"><span data-stu-id="96d89-118">Networking settings</span></span>
* <span data-ttu-id="96d89-119">Titkosítási beállítások</span><span class="sxs-lookup"><span data-stu-id="96d89-119">Encryption settings</span></span>

## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="96d89-120">Recovery Services-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="96d89-120">Create a recovery services vault</span></span>
<span data-ttu-id="96d89-121">A következő lépések alapján a Recovery Services-tároló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="96d89-121">The following steps lead you through creating a Recovery Services vault.</span></span> <span data-ttu-id="96d89-122">Recovery Services-tároló nem egyezik egy biztonsági mentési tárolót.</span><span class="sxs-lookup"><span data-stu-id="96d89-122">A Recovery Services vault is different than a Backup vault.</span></span>

1. <span data-ttu-id="96d89-123">Ha az Azure biztonsági mentés először használ, kell használnia a **Register-AzureRMResourceProvider** parancsmag futtatásával regisztrálja az Azure Recovery szolgáltató az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="96d89-123">If you are using Azure Backup for the first time, you must use the **Register-AzureRMResourceProvider** cmdlet to register the Azure Recovery Service provider with your subscription.</span></span>

    ```
    PS C:\> Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```
2. <span data-ttu-id="96d89-124">A Recovery Services-tároló egy ARM-erőforrás, ezért el kell helyezni az erőforráscsoporton belül.</span><span class="sxs-lookup"><span data-stu-id="96d89-124">The Recovery Services vault is an ARM resource, so you need to place it within a Resource Group.</span></span> <span data-ttu-id="96d89-125">Használjon egy meglévő erőforráscsoportot, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="96d89-125">You can use an existing resource group, or create a new one.</span></span> <span data-ttu-id="96d89-126">Új erőforráscsoport létrehozása esetén adja meg a nevét és helyét, ahhoz az erőforráscsoporthoz.</span><span class="sxs-lookup"><span data-stu-id="96d89-126">When creating a new resource group, specify the name and location for the resource group.</span></span>  

    ```
    PS C:\> New-AzureRmResourceGroup –Name "test-rg" –Location "West US"
    ```
3. <span data-ttu-id="96d89-127">Használja a **New-AzureRmRecoveryServicesVault** parancsmag segítségével hozzon létre egy új tárolót.</span><span class="sxs-lookup"><span data-stu-id="96d89-127">Use the **New-AzureRmRecoveryServicesVault** cmdlet to create a new vault.</span></span> <span data-ttu-id="96d89-128">Ne felejtse el ugyanazon a helyen, a tároló adja meg, mint az erőforráscsoport használt.</span><span class="sxs-lookup"><span data-stu-id="96d89-128">Be sure to specify the same location for the vault as was used for the resource group.</span></span>

    ```
    PS C:\> New-AzureRmRecoveryServicesVault -Name "testvault" -ResourceGroupName " test-rg" -Location "West US"
    ```
4. <span data-ttu-id="96d89-129">Megadhatja a használandó; adattároló redundanciája, amely használhat [helyileg redundáns tárolás (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) vagy [földrajzi redundáns tárolás (GRS)](../storage/common/storage-redundancy.md#geo-redundant-storage).</span><span class="sxs-lookup"><span data-stu-id="96d89-129">Specify the type of storage redundancy to use; you can use [Locally Redundant Storage (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) or [Geo Redundant Storage (GRS)](../storage/common/storage-redundancy.md#geo-redundant-storage).</span></span> <span data-ttu-id="96d89-130">A következő példa bemutatja a - BackupStorageRedundancy beállítás a testVault GeoRedundant értékre van állítva.</span><span class="sxs-lookup"><span data-stu-id="96d89-130">The following example shows the -BackupStorageRedundancy option for testVault is set to GeoRedundant.</span></span>

   > [!TIP]
   > <span data-ttu-id="96d89-131">Sok Azure biztonsági mentést készítő parancsmagok bemeneti adatokként a Recovery Services-tároló objektum szükséges.</span><span class="sxs-lookup"><span data-stu-id="96d89-131">Many Azure Backup cmdlets require the Recovery Services vault object as an input.</span></span> <span data-ttu-id="96d89-132">Emiatt célszerű a Recovery Services biztonsági másolat tároló objektum tárolható egy változóban.</span><span class="sxs-lookup"><span data-stu-id="96d89-132">For this reason, it is convenient to store the Backup Recovery Services vault object in a variable.</span></span>
   >
   >

    ```
    PS C:\> $vault1 = Get-AzureRmRecoveryServicesVault –Name "testVault"
    PS C:\> Set-AzureRmRecoveryServicesBackupProperties  -vault $vault1 -BackupStorageRedundancy GeoRedundant
    ```

## <a name="view-the-vaults-in-a-subscription"></a><span data-ttu-id="96d89-133">A tárolók előfizetés megtekintése</span><span class="sxs-lookup"><span data-stu-id="96d89-133">View the vaults in a subscription</span></span>
<span data-ttu-id="96d89-134">Használjon **Get-AzureRmRecoveryServicesVault** megtekintéséhez az összes tárolók listája az aktuális előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="96d89-134">Use **Get-AzureRmRecoveryServicesVault** to view the list of all vaults in the current subscription.</span></span> <span data-ttu-id="96d89-135">Ezt a parancsot használhatja, ellenőrizze, hogy létrejött-e egy új tárolót, vagy, hogy milyen tárolók-előfizetésben elérhető.</span><span class="sxs-lookup"><span data-stu-id="96d89-135">You can use this command to check that a new  vault was created, or to see what vaults are available in the subscription.</span></span>

<span data-ttu-id="96d89-136">Futtassa a parancsot, a Get-AzureRmRecoveryServicesVault, és az előfizetés összes tárolók találhatók.</span><span class="sxs-lookup"><span data-stu-id="96d89-136">Run the command, Get-AzureRmRecoveryServicesVault, and all vaults in the subscription are listed.</span></span>

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


## <a name="installing-the-azure-backup-agent-on-a-dpm-server"></a><span data-ttu-id="96d89-137">Az Azure Backup szolgáltatás ügynökének telepítése a DPM-kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="96d89-137">Installing the Azure Backup agent on a DPM Server</span></span>
<span data-ttu-id="96d89-138">Az Azure Backup szolgáltatás ügynökének telepítése előtt kell rendelkeznie a telepítő letöltött és található a Windows Server.</span><span class="sxs-lookup"><span data-stu-id="96d89-138">Before you install the Azure Backup agent, you need to have the installer downloaded and present on the Windows Server.</span></span> <span data-ttu-id="96d89-139">Kaphat, hogy a telepítő a legújabb verzióját a [Microsoft Download Center](http://aka.ms/azurebackup_agent) vagy a Recovery Services-tároló irányítópult-oldalon.</span><span class="sxs-lookup"><span data-stu-id="96d89-139">You can get the latest version of the installer from the [Microsoft Download Center](http://aka.ms/azurebackup_agent) or from the Recovery Services vault's Dashboard page.</span></span> <span data-ttu-id="96d89-140">A telepítő menteni egy könnyen elérhető helyre, például a * C:\Downloads\*.</span><span class="sxs-lookup"><span data-stu-id="96d89-140">Save the installer to an easily accessible location like *C:\Downloads\*.</span></span>

<span data-ttu-id="96d89-141">Az ügynök telepítéséhez futtassa a következő parancsot egy rendszergazda jogú PowerShell-konzolban **a DPM-kiszolgálón**:</span><span class="sxs-lookup"><span data-stu-id="96d89-141">To install the agent, run the following command in an elevated PowerShell console **on the DPM server**:</span></span>

```
PS C:\> MARSAgentInstaller.exe /q
```

<span data-ttu-id="96d89-142">Ezzel telepíti az ügynököt az összes alapértelmezett beállítást.</span><span class="sxs-lookup"><span data-stu-id="96d89-142">This installs the agent with all the default options.</span></span> <span data-ttu-id="96d89-143">A telepítés néhány percet vesz igénybe a háttérben.</span><span class="sxs-lookup"><span data-stu-id="96d89-143">The installation takes a few minutes in the background.</span></span> <span data-ttu-id="96d89-144">Ha nem adja meg a */nu* lehetőséget a **Windows Update** ablak nyílik meg a frissítések keresését a telepítés végén.</span><span class="sxs-lookup"><span data-stu-id="96d89-144">If you do not specify the */nu* option the **Windows Update** window opens at the end of the installation to check for any updates.</span></span>

<span data-ttu-id="96d89-145">Az ügynök megjelennek a telepített programok listájában.</span><span class="sxs-lookup"><span data-stu-id="96d89-145">The agent shows up in the list of installed programs.</span></span> <span data-ttu-id="96d89-146">A telepített programok listájának megtekintéséhez keresse fel **Vezérlőpult** > **programok** > **programok és szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="96d89-146">To see the list of installed programs, go to **Control Panel** > **Programs** > **Programs and Features**.</span></span>

![Az ügynök telepítve](./media/backup-dpm-automation/installed-agent-listing.png)

### <a name="installation-options"></a><span data-ttu-id="96d89-148">Telepítési beállítások</span><span class="sxs-lookup"><span data-stu-id="96d89-148">Installation options</span></span>
<span data-ttu-id="96d89-149">A parancssor keresztül elérhető lehetőségekről, használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="96d89-149">To see all the options available via the commandline, use the following command:</span></span>

```
PS C:\> MARSAgentInstaller.exe /?
```

<span data-ttu-id="96d89-150">Az elérhető lehetőségek a következők:</span><span class="sxs-lookup"><span data-stu-id="96d89-150">The available options include:</span></span>

| <span data-ttu-id="96d89-151">Beállítás</span><span class="sxs-lookup"><span data-stu-id="96d89-151">Option</span></span> | <span data-ttu-id="96d89-152">Részletek</span><span class="sxs-lookup"><span data-stu-id="96d89-152">Details</span></span> | <span data-ttu-id="96d89-153">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="96d89-153">Default</span></span> |
| --- | --- | --- |
| <span data-ttu-id="96d89-154">/q</span><span class="sxs-lookup"><span data-stu-id="96d89-154">/q</span></span> |<span data-ttu-id="96d89-155">Csendes telepítés</span><span class="sxs-lookup"><span data-stu-id="96d89-155">Quiet installation</span></span> |- |
| <span data-ttu-id="96d89-156">/ p: "hely"</span><span class="sxs-lookup"><span data-stu-id="96d89-156">/p:"location"</span></span> |<span data-ttu-id="96d89-157">Az Azure Backup szolgáltatás ügynökének a telepítési mappa elérési útja.</span><span class="sxs-lookup"><span data-stu-id="96d89-157">Path to the installation folder for the Azure Backup agent.</span></span> |<span data-ttu-id="96d89-158">C:\Program Files\Microsoft Azure Recovery Services Agent ügynök</span><span class="sxs-lookup"><span data-stu-id="96d89-158">C:\Program Files\Microsoft Azure Recovery Services Agent</span></span> |
| <span data-ttu-id="96d89-159">/ s: "hely"</span><span class="sxs-lookup"><span data-stu-id="96d89-159">/s:"location"</span></span> |<span data-ttu-id="96d89-160">Az Azure Backup szolgáltatás ügynökének a gyorsítótár mappájának elérési útja.</span><span class="sxs-lookup"><span data-stu-id="96d89-160">Path to the cache folder for the Azure Backup agent.</span></span> |<span data-ttu-id="96d89-161">C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch</span><span class="sxs-lookup"><span data-stu-id="96d89-161">C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch</span></span> |
| <span data-ttu-id="96d89-162">/m</span><span class="sxs-lookup"><span data-stu-id="96d89-162">/m</span></span> |<span data-ttu-id="96d89-163">Részvétel a Microsoft Update</span><span class="sxs-lookup"><span data-stu-id="96d89-163">Opt-in to Microsoft Update</span></span> |- |
| <span data-ttu-id="96d89-164">/Nu</span><span class="sxs-lookup"><span data-stu-id="96d89-164">/nu</span></span> |<span data-ttu-id="96d89-165">Ne keressen frissítéseket telepítésének befejezése után</span><span class="sxs-lookup"><span data-stu-id="96d89-165">Do not Check for updates after installation is complete</span></span> |- |
| <span data-ttu-id="96d89-166">/d</span><span class="sxs-lookup"><span data-stu-id="96d89-166">/d</span></span> |<span data-ttu-id="96d89-167">Eltávolítja a Microsoft Azure Recovery Services Agent ügynök</span><span class="sxs-lookup"><span data-stu-id="96d89-167">Uninstalls Microsoft Azure Recovery Services Agent</span></span> |- |
| <span data-ttu-id="96d89-168">/pH</span><span class="sxs-lookup"><span data-stu-id="96d89-168">/ph</span></span> |<span data-ttu-id="96d89-169">Állomás proxycím</span><span class="sxs-lookup"><span data-stu-id="96d89-169">Proxy Host Address</span></span> |- |
| <span data-ttu-id="96d89-170">/po</span><span class="sxs-lookup"><span data-stu-id="96d89-170">/po</span></span> |<span data-ttu-id="96d89-171">Proxy Host Port száma</span><span class="sxs-lookup"><span data-stu-id="96d89-171">Proxy Host Port Number</span></span> |- |
| <span data-ttu-id="96d89-172">/Pu</span><span class="sxs-lookup"><span data-stu-id="96d89-172">/pu</span></span> |<span data-ttu-id="96d89-173">Proxy-állomás felhasználónév</span><span class="sxs-lookup"><span data-stu-id="96d89-173">Proxy Host UserName</span></span> |- |
| <span data-ttu-id="96d89-174">/pW</span><span class="sxs-lookup"><span data-stu-id="96d89-174">/pw</span></span> |<span data-ttu-id="96d89-175">Proxy jelszava</span><span class="sxs-lookup"><span data-stu-id="96d89-175">Proxy Password</span></span> |- |

## <a name="registering-dpm-to-a-recovery-services-vault"></a><span data-ttu-id="96d89-176">Egy helyreállítási regisztrálásakor DPM szolgáltatások tároló</span><span class="sxs-lookup"><span data-stu-id="96d89-176">Registering DPM to a Recovery Services Vault</span></span>
<span data-ttu-id="96d89-177">A Recovery Services-tároló létrehozását követően töltse le a legújabb ügynököt és a tárolói hitelesítő adatokat, és C:\Downloads például egy tetszőleges helyen tárolja.</span><span class="sxs-lookup"><span data-stu-id="96d89-177">After you created the Recovery Services vault, download the latest agent and the vault credentials and store it in a convenient location like C:\Downloads.</span></span>

```
PS C:\> $credspath = "C:\downloads"
PS C:\> $credsfilename = Get-AzureRmRecoveryServicesVaultSettingsFile -Backup -Vault $vault1 -Path  $credspath
PS C:\> $credsfilename
C:\downloads\testvault\_Sun Apr 10 2016.VaultCredentials
```

<span data-ttu-id="96d89-178">A DPM-kiszolgálón futtassa a [Start-OBRegistration](https://technet.microsoft.com/library/hh770398%28v=wps.630%29.aspx) parancsmag regisztrálni a gépet a tárolóban.</span><span class="sxs-lookup"><span data-stu-id="96d89-178">On the DPM server, run the [Start-OBRegistration](https://technet.microsoft.com/library/hh770398%28v=wps.630%29.aspx) cmdlet to register the machine with the vault.</span></span>

```
PS C:\> $cred = $credspath + $credsfilename
PS C:\> Start-OBRegistration-VaultCredentials $cred -Confirm:$false
CertThumbprint      :7a2ef2caa2e74b6ed1222a5e89288ddad438df2
SubscriptionID      : ef4ab577-c2c0-43e4-af80-af49f485f3d1
ServiceResourceName: testvault
Region              :West US
Machine registration succeeded.
```

### <a name="initial-configuration-settings"></a><span data-ttu-id="96d89-179">Kezdeti konfigurációs beállításai</span><span class="sxs-lookup"><span data-stu-id="96d89-179">Initial configuration settings</span></span>
<span data-ttu-id="96d89-180">Miután a DPM-kiszolgáló regisztrálva van a Recovery Services-tároló, alapértelmezett előfizetési beállítások kezdődik.</span><span class="sxs-lookup"><span data-stu-id="96d89-180">Once the DPM Server is registered with the Recovery Services vault, it starts with default subscription settings.</span></span> <span data-ttu-id="96d89-181">Ezek előfizetési beállítások közé tartozik a hálózatkezelés, a titkosítás és az átmeneti területre.</span><span class="sxs-lookup"><span data-stu-id="96d89-181">These subscription settings include Networking, Encryption and the Staging area.</span></span> <span data-ttu-id="96d89-182">Először kapott egy leíró használata (alapértelmezett) beállítást az előfizetési beállítások módosítása a [Get-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612793) parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="96d89-182">To change subscription settings you need to first get a handle on the existing (default) settings using the [Get-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612793) cmdlet:</span></span>

```
$setting = Get-DPMCloudSubscriptionSetting -DPMServerName "TestingServer"
```

<span data-ttu-id="96d89-183">Minden módosítás a helyi PowerShell objektumhoz ```$setting``` majd a teljes objektum elkötelezte magát a DPM és az Azure Backup mentse őket, és használja a [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="96d89-183">All modifications are made to this local PowerShell object ```$setting```  and then the full object is committed to DPM and Azure Backup to save them using the [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet.</span></span> <span data-ttu-id="96d89-184">Kell használnia a ```–Commit``` beállítás segítségével győződjön meg arról, hogy a változtatások megmaradnak.</span><span class="sxs-lookup"><span data-stu-id="96d89-184">You need to use the ```–Commit``` flag to ensure that the changes are persisted.</span></span> <span data-ttu-id="96d89-185">A beállítások nem is alkalmazott és Azure Backup szolgáltatás csak a véglegesített használni.</span><span class="sxs-lookup"><span data-stu-id="96d89-185">The settings will not be applied and used by Azure Backup unless committed.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -Commit
```

## <a name="networking"></a><span data-ttu-id="96d89-186">Hálózat</span><span class="sxs-lookup"><span data-stu-id="96d89-186">Networking</span></span>
<span data-ttu-id="96d89-187">Ha a DPM-számítógépen az Azure Backup szolgáltatás az interneten való csatlakoztatásához proxykiszolgálón keresztül, majd a proxykiszolgáló beállításait kell rendelkezni a sikeres biztonsági mentések.</span><span class="sxs-lookup"><span data-stu-id="96d89-187">If the connectivity of the DPM machine to the Azure Backup service on the internet is through a proxy server, then the proxy server settings should be provided for successful backups.</span></span> <span data-ttu-id="96d89-188">Ehhez használja a ```-ProxyServer```és ```-ProxyPort```, ```-ProxyUsername``` és a ```ProxyPassword``` paramétereket a [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="96d89-188">This is done by using the ```-ProxyServer```and ```-ProxyPort```, ```-ProxyUsername``` and the ```ProxyPassword``` parameters with the [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet.</span></span> <span data-ttu-id="96d89-189">Ebben a példában nincs proxy kiszolgáló, explicit módon azt vannak törlésével bármely proxy kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="96d89-189">In this example, there is no proxy server so we are explicitly clearing any proxy-related information.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -NoProxy
```

<span data-ttu-id="96d89-190">Sávszélesség is szabályozható a beállítások a ```-WorkHourBandwidth``` és ```-NonWorkHourBandwidth``` napokat a hét adott számú.</span><span class="sxs-lookup"><span data-stu-id="96d89-190">Bandwidth usage can also be controlled with options of ```-WorkHourBandwidth``` and ```-NonWorkHourBandwidth``` for a given set of days of the week.</span></span> <span data-ttu-id="96d89-191">Ebben a példában a Microsoft nem állítja a sávszélesség-szabályozás.</span><span class="sxs-lookup"><span data-stu-id="96d89-191">In this example, we are not setting any throttling.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -NoThrottle
```

## <a name="configuring-the-staging-area"></a><span data-ttu-id="96d89-192">Az átmeneti területre konfigurálása</span><span class="sxs-lookup"><span data-stu-id="96d89-192">Configuring the staging Area</span></span>
<span data-ttu-id="96d89-193">A DPM-kiszolgálón futó Azure Backup ügynöknek szüksége van ideiglenes tárhelyre, (helyi átmeneti terület) a felhőből helyreállított adatok számára.</span><span class="sxs-lookup"><span data-stu-id="96d89-193">The Azure Backup agent running on the DPM server needs temporary storage for data restored from the cloud (local staging area).</span></span> <span data-ttu-id="96d89-194">Konfigurálhatja az átmeneti területre segítségével a [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) parancsmag és a ```-StagingAreaPath``` paraméter.</span><span class="sxs-lookup"><span data-stu-id="96d89-194">Configure the staging area using the [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet and the ```-StagingAreaPath``` parameter.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -StagingAreaPath "C:\StagingArea"
```

<span data-ttu-id="96d89-195">A fenti példában az átmeneti területre úgy lesz beállítva, *C:\StagingArea* a PowerShell objektumban ```$setting```.</span><span class="sxs-lookup"><span data-stu-id="96d89-195">In the example above, the staging area will be set to *C:\StagingArea* in the PowerShell object ```$setting```.</span></span> <span data-ttu-id="96d89-196">Győződjön meg arról, hogy a megadott mappa már létezik, vagy pedig az előfizetési beállítások végső véglegesítése sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="96d89-196">Ensure that the specified folder already exists, or else the final commit of the subscription settings will fail.</span></span>

### <a name="encryption-settings"></a><span data-ttu-id="96d89-197">Titkosítási beállítások</span><span class="sxs-lookup"><span data-stu-id="96d89-197">Encryption settings</span></span>
<span data-ttu-id="96d89-198">A biztonsági mentési Azure biztonsági mentési küldött adatok védelmét az adatok bizalmas mivoltát titkosított.</span><span class="sxs-lookup"><span data-stu-id="96d89-198">The backup data sent to Azure Backup is encrypted to protect the confidentiality of the data.</span></span> <span data-ttu-id="96d89-199">A titkosítás jelszava a "password" vissza kell fejtenie az adatokat a visszaállítás időpontjában.</span><span class="sxs-lookup"><span data-stu-id="96d89-199">The encryption passphrase is the "password" to decrypt the data at the time of restore.</span></span> <span data-ttu-id="96d89-200">Fontos tartani ezeket az információkat megbízható és biztonságos be van állítva.</span><span class="sxs-lookup"><span data-stu-id="96d89-200">It is important to keep this information safe and secure once it is set.</span></span>

<span data-ttu-id="96d89-201">Az alábbi példában a karakterlánc az első parancs konvertálása ```passphrase123456789``` egy biztonságos karakterláncot, és a biztonságos karakterláncot kell megadnia a változóhoz nevű rendel ```$Passphrase```.</span><span class="sxs-lookup"><span data-stu-id="96d89-201">In the example below, the first command converts the string ```passphrase123456789``` to a secure string and assigns the secure string to the variable named ```$Passphrase```.</span></span> <span data-ttu-id="96d89-202">a második parancs beállítja a biztonságos karakterláncot kell megadnia ```$Passphrase``` és a biztonsági másolatok titkosításához.</span><span class="sxs-lookup"><span data-stu-id="96d89-202">the second command sets the secure string in ```$Passphrase``` as the password for encrypting backups.</span></span>

```
PS C:\> $Passphrase = ConvertTo-SecureString -string "passphrase123456789" -AsPlainText -Force

PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -EncryptionPassphrase $Passphrase
```

> [!IMPORTANT]
> <span data-ttu-id="96d89-203">Jelszó információinak megtartása biztonságos be van állítva.</span><span class="sxs-lookup"><span data-stu-id="96d89-203">Keep the passphrase information safe and secure once it is set.</span></span> <span data-ttu-id="96d89-204">Nem lehet visszaállítani az adatokat az Azure-ból nélkül ezt a jelszót.</span><span class="sxs-lookup"><span data-stu-id="96d89-204">You will not be able to restore data from Azure without this passphrase.</span></span>
>
>

<span data-ttu-id="96d89-205">Ezen a ponton a szükséges módosításokat, hogy el kell a ```$setting``` objektum.</span><span class="sxs-lookup"><span data-stu-id="96d89-205">At this point, you should have made all the required changes to the ```$setting``` object.</span></span> <span data-ttu-id="96d89-206">Ne felejtse el a változtatások véglegesítése a határidő.</span><span class="sxs-lookup"><span data-stu-id="96d89-206">Remember to commit the changes.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -Commit
```

## <a name="protect-data-to-azure-backup"></a><span data-ttu-id="96d89-207">Az Azure Backup szolgáltatásba adatok védelme</span><span class="sxs-lookup"><span data-stu-id="96d89-207">Protect data to Azure Backup</span></span>
<span data-ttu-id="96d89-208">Ez a szakasz az üzemi kiszolgáló hozzáadása a DPM, és majd adatvédelemben a helyi DPM-tároló, majd az Azure Backup szolgáltatásba.</span><span class="sxs-lookup"><span data-stu-id="96d89-208">In this section, you will add a production server to DPM and then protect the data to local DPM storage and then to Azure Backup.</span></span> <span data-ttu-id="96d89-209">A példákban a fájlok és mappák biztonsági bemutatjuk.</span><span class="sxs-lookup"><span data-stu-id="96d89-209">In the examples, we will demonstrate how to back up files and folders.</span></span> <span data-ttu-id="96d89-210">A logikai könnyen bővíthető minden DPM által támogatott adatforrás biztonsági mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="96d89-210">The logic can easily be extended to backup any DPM-supported data source.</span></span> <span data-ttu-id="96d89-211">A DPM biztonsági mentések által a védelmi csoport (PG) négy alkotórészek vonatkoznak:</span><span class="sxs-lookup"><span data-stu-id="96d89-211">All your DPM backups are governed by a Protection Group (PG) with four parts:</span></span>

1. <span data-ttu-id="96d89-212">**Csoport tagjai** a védhető objektumok listáját (más néven *adatforrások* a DPM-ben), amelyek ugyanabba a védelmi csoportba a védeni kívánt.</span><span class="sxs-lookup"><span data-stu-id="96d89-212">**Group members** is a list of all the protectable objects (also known as *Datasources* in DPM) that you want to protect in the same protection group.</span></span> <span data-ttu-id="96d89-213">Például előfordulhat, hogy védeni kívánt virtuális gépek éles egy védelmi csoport és egy másik védelmi csoportban található SQL Server-adatbázisok különböző biztonsági követelményeket is rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="96d89-213">For example, you may want to protect production VMs in one protection group and SQL Server databases in another protection group as they may have different backup requirements.</span></span> <span data-ttu-id="96d89-214">Előtt biztonsági másolatot készíthet a datasource egy üzemi kiszolgálón meg kell győződnie, hogy a DPM-ügynök telepítve van a kiszolgálón, és a DPM által kezelt.</span><span class="sxs-lookup"><span data-stu-id="96d89-214">Before you can back up any datasource on a production server you need to make sure the DPM Agent is installed on the server and is managed by DPM.</span></span> <span data-ttu-id="96d89-215">Kövesse a lépéseket [a DPM-ügynök telepítéséhez](https://technet.microsoft.com/library/bb870935.aspx) és kapcsolásával végezze el a megfelelő DPM-kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="96d89-215">Follow the steps for [installing the DPM Agent](https://technet.microsoft.com/library/bb870935.aspx) and linking it to the appropriate DPM Server.</span></span>
2. <span data-ttu-id="96d89-216">**Adatvédelmi módszer** határozza meg a biztonsági mentési célhelyek - szalag, a lemez és a felhőben.</span><span class="sxs-lookup"><span data-stu-id="96d89-216">**Data protection method** specifies the target backup locations - tape, disk, and cloud.</span></span> <span data-ttu-id="96d89-217">Ebben a példában azt fogja védeni az adatokat a helyi lemezek és a felhő.</span><span class="sxs-lookup"><span data-stu-id="96d89-217">In our example we will protect data to the local disk and to the cloud.</span></span>
3. <span data-ttu-id="96d89-218">A **biztonsági mentés ütemezése** , amely megadja, hogy ha a biztonsági mentést kell tenni, és milyen gyakran a szinkronizálja az adatokat a DPM-kiszolgálón és az üzemi kiszolgáló között.</span><span class="sxs-lookup"><span data-stu-id="96d89-218">A **backup schedule** that specifies when backups need to be taken and how often the data should be synchronized between the DPM Server and the production server.</span></span>
4. <span data-ttu-id="96d89-219">A **adatmegőrzési ütemterv** , amely megadja, mennyi ideig szeretné megőrizni a helyreállítási pontok az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="96d89-219">A **retention schedule** that specifies how long to retain the recovery points in Azure.</span></span>

### <a name="creating-a-protection-group"></a><span data-ttu-id="96d89-220">Védelmi csoport létrehozásakor</span><span class="sxs-lookup"><span data-stu-id="96d89-220">Creating a protection group</span></span>
<span data-ttu-id="96d89-221">Először hozzon létre egy új védelmi csoport használatával a [New-DPMProtectionGroup](https://technet.microsoft.com/library/hh881722) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="96d89-221">Start by creating a new Protection Group using the [New-DPMProtectionGroup](https://technet.microsoft.com/library/hh881722) cmdlet.</span></span>

```
PS C:\> $PG = New-DPMProtectionGroup -DPMServerName " TestingServer " -Name "ProtectGroup01"
```

<span data-ttu-id="96d89-222">A fenti parancsmag hozza létre a védelmi csoport nevű *ProtectGroup01*.</span><span class="sxs-lookup"><span data-stu-id="96d89-222">The above cmdlet will create a Protection Group named *ProtectGroup01*.</span></span> <span data-ttu-id="96d89-223">Egy meglévő védelmi csoport is módosíthatja később a biztonsági mentés vegye fel az Azure felhőbe.</span><span class="sxs-lookup"><span data-stu-id="96d89-223">An existing protection group can also be modified later to add backup to the Azure cloud.</span></span> <span data-ttu-id="96d89-224">Azonban ne módosítsa a védelmi csoport - új vagy meglévő - kell a leírót szerezni a *módosíthatóvá* objektumba a [Get-DPMModifiableProtectionGroup](https://technet.microsoft.com/library/hh881713) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="96d89-224">However, to make any changes to the Protection Group - new or existing - we need to get a handle on a *modifiable* object using the [Get-DPMModifiableProtectionGroup](https://technet.microsoft.com/library/hh881713) cmdlet.</span></span>

```
PS C:\> $MPG = Get-ModifiableProtectionGroup $PG
```

### <a name="adding-group-members-to-the-protection-group"></a><span data-ttu-id="96d89-225">Csoport tagok hozzáadása védelmi csoporthoz</span><span class="sxs-lookup"><span data-stu-id="96d89-225">Adding group members to the Protection Group</span></span>
<span data-ttu-id="96d89-226">Minden DPM-ügynök tudja azon a kiszolgálón, amelyre telepítve van, az adatforrások listája.</span><span class="sxs-lookup"><span data-stu-id="96d89-226">Each DPM Agent knows the list of datasources on the server that it is installed on.</span></span> <span data-ttu-id="96d89-227">Egy adatforrás hozzáadása a védelmi csoport, a DPM-ügynököt kell először elküldi a DPM-kiszolgálón az adatforrások listáját.</span><span class="sxs-lookup"><span data-stu-id="96d89-227">To add a datasource to the Protection Group, the DPM Agent needs to first send a list of the datasources back to the DPM server.</span></span> <span data-ttu-id="96d89-228">Egy vagy több adatforrás majd kiválasztva, és hozzáad a védelmi csoporthoz.</span><span class="sxs-lookup"><span data-stu-id="96d89-228">One or more datasources are then selected and added to the Protection Group.</span></span> <span data-ttu-id="96d89-229">Ennek eléréséhez szükséges PowerShell lépéseket a következők:</span><span class="sxs-lookup"><span data-stu-id="96d89-229">The PowerShell steps needed to achieve this are:</span></span>

1. <span data-ttu-id="96d89-230">A DPM-ügynököt a DPM által kezelt összes kiszolgálók listájának beolvasása.</span><span class="sxs-lookup"><span data-stu-id="96d89-230">Fetch a list of all servers managed by DPM through the DPM Agent.</span></span>
2. <span data-ttu-id="96d89-231">Válasszon egy adott kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="96d89-231">Choose a specific server.</span></span>
3. <span data-ttu-id="96d89-232">Az összes adatforrás listájának beolvasása a kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="96d89-232">Fetch a list of all datasources on the server.</span></span>
4. <span data-ttu-id="96d89-233">Válasszon egy vagy több adatforrást, majd adja hozzá a védelmi csoport</span><span class="sxs-lookup"><span data-stu-id="96d89-233">Choose one or more datasources and add them to the Protection Group</span></span>

<span data-ttu-id="96d89-234">A DPM-ügynök telepítve van és a DPM-kiszolgáló által kezelt kiszolgálók listája szerzett a [Get-DPMProductionServer](https://technet.microsoft.com/library/hh881600) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="96d89-234">The list of servers on which the DPM Agent is installed and is being managed by the DPM Server is acquired with the [Get-DPMProductionServer](https://technet.microsoft.com/library/hh881600) cmdlet.</span></span> <span data-ttu-id="96d89-235">Ebben a példában azt szűrésére és konfigurálását elvégzi, csak PS nevű *productionserver01* a biztonsági mentéshez.</span><span class="sxs-lookup"><span data-stu-id="96d89-235">In this example we will filter and only configure PS with name *productionserver01* for backup.</span></span>

```
PS C:\> $server = Get-ProductionServer -DPMServerName "TestingServer" | where {($_.servername) –contains “productionserver01”
```

<span data-ttu-id="96d89-236">Most beolvasni az adatforrások listája ```$server``` használatával a [Get-DPMDatasource](https://technet.microsoft.com/library/hh881605) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="96d89-236">Now fetch the list of datasources on ```$server``` using the [Get-DPMDatasource](https://technet.microsoft.com/library/hh881605) cmdlet.</span></span> <span data-ttu-id="96d89-237">Ebben a példában a Microsoft jelenleg korlátozza a kötetek * D:\* kell konfigurálni a biztonsági mentéshez.</span><span class="sxs-lookup"><span data-stu-id="96d89-237">In this example we are filtering for the volume *D:\* that we want to configure for backup.</span></span> <span data-ttu-id="96d89-238">Ez az adatforrás kerül a védelmi csoport használatával a [Add-DPMChildDatasource](https://technet.microsoft.com/library/hh881732) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="96d89-238">This datasource is then added to the Protection Group using the [Add-DPMChildDatasource](https://technet.microsoft.com/library/hh881732) cmdlet.</span></span> <span data-ttu-id="96d89-239">Fontos, hogy a *módosíthatóvá* védelmi csoport objektum ```$MPG``` , hogy a kiegészítsék.</span><span class="sxs-lookup"><span data-stu-id="96d89-239">Remember to use the *modifiable* protection group object ```$MPG``` to make the additions.</span></span>

```
PS C:\> $DS = Get-Datasource -ProductionServer $server -Inquire | where { $_.Name -contains “D:\” }

PS C:\> Add-DPMChildDatasource -ProtectionGroup $MPG -ChildDatasource $DS
```

<span data-ttu-id="96d89-240">Ismételje meg ezt a lépést, ha szükséges, ahányszor, amíg az összes kiválasztott adatforrás a védelmi csoporthoz hozzáadott.</span><span class="sxs-lookup"><span data-stu-id="96d89-240">Repeat this step as many times as required, until you have added all the chosen datasources to the protection group.</span></span> <span data-ttu-id="96d89-241">Start van egy DataSource elemmel, és végezze el a védelmi csoport létrehozása a munkafolyamatot, majd később adja további adatforrások védelmi csoporthoz.</span><span class="sxs-lookup"><span data-stu-id="96d89-241">You can also start with just one datasource, and complete the workflow for creating the Protection Group, and at a later point add more datasources to the Protection Group.</span></span>

### <a name="selecting-the-data-protection-method"></a><span data-ttu-id="96d89-242">Az adatvédelmi módszer kiválasztása</span><span class="sxs-lookup"><span data-stu-id="96d89-242">Selecting the data protection method</span></span>
<span data-ttu-id="96d89-243">Miután az adatforrások érhetőek el a védelmi csoport, a következő lépéssel adhatja meg a védelmi módszer használatával-e a [Set-DPMProtectionType](https://technet.microsoft.com/library/hh881725) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="96d89-243">Once the datasources have been added to the Protection Group, the next step is to specify the protection method using the [Set-DPMProtectionType](https://technet.microsoft.com/library/hh881725) cmdlet.</span></span> <span data-ttu-id="96d89-244">Ebben a példában a védelmi csoport működik a helyi lemezek és felhőbeli biztonsági mentését.</span><span class="sxs-lookup"><span data-stu-id="96d89-244">In this example, the Protection Group is setup for local disk and cloud backup.</span></span> <span data-ttu-id="96d89-245">Is meg kell adnia a felhőbe használatával védeni kívánt adatforrást a [Add-DPMChildDatasource](https://technet.microsoft.com/library/hh881732.aspx) jelölővel - Online parancsmag.</span><span class="sxs-lookup"><span data-stu-id="96d89-245">You also need to specify the datasource that you want to protect to cloud using the [Add-DPMChildDatasource](https://technet.microsoft.com/library/hh881732.aspx) cmdlet with -Online flag.</span></span>

```
PS C:\> Set-DPMProtectionType -ProtectionGroup $MPG -ShortTerm Disk –LongTerm Online
PS C:\> Add-DPMChildDatasource -ProtectionGroup $MPG -ChildDatasource $DS –Online
```

### <a name="setting-the-retention-range"></a><span data-ttu-id="96d89-246">A megőrzési tartomány beállítása</span><span class="sxs-lookup"><span data-stu-id="96d89-246">Setting the retention range</span></span>
<span data-ttu-id="96d89-247">A biztonsági mentési pontok használatával, állítsa be a megőrzési a [Set-DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="96d89-247">Set the retention for the backup points using the [Set-DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) cmdlet.</span></span> <span data-ttu-id="96d89-248">Amíg tűnhet páratlan beállítani a megőrzési előtt a biztonsági mentés ütemezése definiálva van, használja a ```Set-DPMPolicyObjective``` parancsmag automatikusan úgy állítja be a alapértelmezett biztonsági mentés ütemezését, majd módosítható.</span><span class="sxs-lookup"><span data-stu-id="96d89-248">While it might seem odd to set the retention before the backup schedule has been defined, using the ```Set-DPMPolicyObjective``` cmdlet automatically sets a default backup schedule that can then be modified.</span></span> <span data-ttu-id="96d89-249">Mindig lehetőség a biztonsági mentés ütemezése először megadása és az adatmegőrzési után.</span><span class="sxs-lookup"><span data-stu-id="96d89-249">It is always possible to set the backup schedule first and the retention policy after.</span></span>

<span data-ttu-id="96d89-250">Az alábbi példában a parancsmag a lemezes biztonsági mentések megőrzési paramétereinek beállítása.</span><span class="sxs-lookup"><span data-stu-id="96d89-250">In the example below, the cmdlet sets the retention parameters for disk backups.</span></span> <span data-ttu-id="96d89-251">Ez fogja megőrizni biztonsági másolatokat 10 nap, és szinkronizálja az adatokat az üzemi kiszolgálón és a DPM-kiszolgáló közötti 6 óránként.</span><span class="sxs-lookup"><span data-stu-id="96d89-251">This will retain backups for 10 days, and sync data every 6 hours between the production server and the DPM server.</span></span> <span data-ttu-id="96d89-252">A ```SynchronizationFrequencyMinutes``` nem adja meg, milyen gyakran egy biztonsági mentési pont jön létre, de milyen gyakran adatokat a DPM-kiszolgálóra másolja.</span><span class="sxs-lookup"><span data-stu-id="96d89-252">The ```SynchronizationFrequencyMinutes``` doesn't define how often a backup point is created, but how often data is copied to the DPM server.</span></span>  <span data-ttu-id="96d89-253">Ez a beállítás megakadályozza, hogy a biztonsági mentések túl nagy.</span><span class="sxs-lookup"><span data-stu-id="96d89-253">This setting prevents backups from becoming too large.</span></span>

```
PS C:\> Set-DPMPolicyObjective –ProtectionGroup $MPG -RetentionRangeInDays 10 -SynchronizationFrequencyMinutes 360
```

<span data-ttu-id="96d89-254">Ugrás az Azure biztonsági mentés (DPM hivatkozik azokat az Online biztonsági mentés másként) állítható be a megőrzési időtartamokat [hosszú távon szerzett-Édesapja-fia megjelenítve (GFS) megőrzési](backup-azure-backup-cloud-as-tape.md).</span><span class="sxs-lookup"><span data-stu-id="96d89-254">For backups going to Azure (DPM refers to them as Online backups) the retention ranges can be configured for [long term retention using a Grandfather-Father-Son scheme (GFS)](backup-azure-backup-cloud-as-tape.md).</span></span> <span data-ttu-id="96d89-255">Ez azt jelenti, hogy napi, heti, havi és éves adatmegőrzési szabályoknál érintő kombinált adatmegőrzési adhat meg.</span><span class="sxs-lookup"><span data-stu-id="96d89-255">That is, you can define a combined retention policy involving daily, weekly, monthly and yearly retention policies.</span></span> <span data-ttu-id="96d89-256">Ebben a példában azt az összetett megőrzési séma, amely azt szeretnénk, ha képviselő tömböt létrehozása, és adja meg a megőrzési tartomány használata a [Set-DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="96d89-256">In this example, we create an array representing the complex retention scheme that we want, and then configure the retention range using the [Set-DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) cmdlet.</span></span>

```
PS C:\> $RRlist = @()
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 180, Days)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 104, Weeks)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 60, Month)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 10, Years)
PS C:\> Set-DPMPolicyObjective –ProtectionGroup $MPG -OnlineRetentionRangeList $RRlist
```

### <a name="set-the-backup-schedule"></a><span data-ttu-id="96d89-257">A biztonsági mentési ütemezést beállítani.</span><span class="sxs-lookup"><span data-stu-id="96d89-257">Set the backup schedule</span></span>
<span data-ttu-id="96d89-258">A DPM automatikusan beállítja, egy alapértelmezett biztonsági mentés ütemezése ha megadja, hogy a védelmi cél használata a ```Set-DPMPolicyObjective``` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="96d89-258">DPM sets a default backup schedule automatically if you specify the protection objective using the ```Set-DPMPolicyObjective``` cmdlet.</span></span> <span data-ttu-id="96d89-259">Az alapértelmezett ütemezés módosításához használja a [Get-DPMPolicySchedule](https://technet.microsoft.com/library/hh881749) parancsmag követi a [Set-DPMPolicySchedule](https://technet.microsoft.com/library/hh881723) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="96d89-259">To change the default schedules, use the [Get-DPMPolicySchedule](https://technet.microsoft.com/library/hh881749) cmdlet followed by the [Set-DPMPolicySchedule](https://technet.microsoft.com/library/hh881723) cmdlet.</span></span>

```
PS C:\> $onlineSch = Get-DPMPolicySchedule -ProtectionGroup $mpg -LongTerm Online
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[0] -TimesOfDay 02:00
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[1] -TimesOfDay 02:00 -DaysOfWeek Sa,Su –Interval 1
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[2] -TimesOfDay 02:00 -RelativeIntervals First,Third –DaysOfWeek Sa
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[3] -TimesOfDay 02:00 -DaysOfMonth 2,5,8,9 -Months Jan,Jul
PS C:\> Set-DPMProtectionGroup -ProtectionGroup $MPG
```

<span data-ttu-id="96d89-260">A fenti példában ```$onlineSch``` tömb négy elemekkel, amely tartalmazza a meglévő online védelmi ütemterv a védelmi csoport GFS sémában:</span><span class="sxs-lookup"><span data-stu-id="96d89-260">In the above example, ```$onlineSch``` is an array with four elements that contains the existing online protection schedule for the Protection Group in the GFS scheme:</span></span>

1. <span data-ttu-id="96d89-261">```$onlineSch[0]```napi ütemezés tartalmazza</span><span class="sxs-lookup"><span data-stu-id="96d89-261">```$onlineSch[0]``` contains the daily schedule</span></span>
2. <span data-ttu-id="96d89-262">```$onlineSch[1]```a heti ütemezés tartalmazza</span><span class="sxs-lookup"><span data-stu-id="96d89-262">```$onlineSch[1]``` contains the weekly schedule</span></span>
3. <span data-ttu-id="96d89-263">```$onlineSch[2]```a havi ütemezések tartalmazza</span><span class="sxs-lookup"><span data-stu-id="96d89-263">```$onlineSch[2]``` contains the monthly schedule</span></span>
4. <span data-ttu-id="96d89-264">```$onlineSch[3]```éves ütemezés tartalmazza</span><span class="sxs-lookup"><span data-stu-id="96d89-264">```$onlineSch[3]``` contains the yearly schedule</span></span>

<span data-ttu-id="96d89-265">Így ha módosítania kell a heti ütemezés, akkor tekintse meg a ```$onlineSch[1]```.</span><span class="sxs-lookup"><span data-stu-id="96d89-265">So if you need to modify the weekly schedule, you need to refer to the ```$onlineSch[1]```.</span></span>

### <a name="initial-backup"></a><span data-ttu-id="96d89-266">Kezdeti biztonsági mentés</span><span class="sxs-lookup"><span data-stu-id="96d89-266">Initial backup</span></span>
<span data-ttu-id="96d89-267">Ha egy adatforrás először, DPM rendszernek végig kell biztonsági mentéséről hoz létre a kezdeti replika, amely az adatforrás védelemre a DPM-replikakötet teljes másolatot készít.</span><span class="sxs-lookup"><span data-stu-id="96d89-267">When backing up a datasource for the first time, DPM needs creates initial replica that creates a full copy of the datasource to be protected on DPM replica volume.</span></span> <span data-ttu-id="96d89-268">Ez a tevékenység vagy egy adott időpont ütemezhetők, vagy manuálisan is elindítható, használja a [Set-DPMReplicaCreationMethod](https://technet.microsoft.com/library/hh881715) parancsmag és paraméter ```-NOW```.</span><span class="sxs-lookup"><span data-stu-id="96d89-268">This activity can either be scheduled for a specific time, or can be triggered manually, using the [Set-DPMReplicaCreationMethod](https://technet.microsoft.com/library/hh881715) cmdlet with the parameter ```-NOW```.</span></span>

```
PS C:\> Set-DPMReplicaCreationMethod -ProtectionGroup $MPG -NOW
```
### <a name="changing-the-size-of-dpm-replica--recovery-point-volume"></a><span data-ttu-id="96d89-269">DPM-replika & a helyreállításipont-kötet méretének módosítása</span><span class="sxs-lookup"><span data-stu-id="96d89-269">Changing the size of DPM Replica & recovery point volume</span></span>
<span data-ttu-id="96d89-270">Módosíthatja a méretét a DPM-replikakötet és árnyékmásolat-kötet használatával is [Set-DPMDatasourceDiskAllocation](https://technet.microsoft.com/library/hh881618.aspx) parancsmag az alábbi példában látható módon: Get-DatasourceDiskAllocation - Datasource $DS Set-datasourcediskallocation segédprogram - DataSource $DS - ProtectionGroup $MPG-manuális - ReplicaArea (2 gb) – ShadowCopyArea (2 gb)</span><span class="sxs-lookup"><span data-stu-id="96d89-270">You can also change the size of DPM Replica volume and Shadow Copy volume using [Set-DPMDatasourceDiskAllocation](https://technet.microsoft.com/library/hh881618.aspx) cmdlet as in the following example: Get-DatasourceDiskAllocation -Datasource $DS Set-DatasourceDiskAllocation -Datasource $DS -ProtectionGroup $MPG -manual -ReplicaArea (2gb) -ShadowCopyArea (2gb)</span></span>

### <a name="committing-the-changes-to-the-protection-group"></a><span data-ttu-id="96d89-271">A módosítások végrehajtása a védelmi csoporthoz</span><span class="sxs-lookup"><span data-stu-id="96d89-271">Committing the changes to the Protection Group</span></span>
<span data-ttu-id="96d89-272">Végül a módosításokat kell hajtható végre, a DPM a biztonsági mentés az új védelmi csoport konfigurációja / megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="96d89-272">Finally, the changes need to be committed before DPM can take the backup per the new Protection Group configuration.</span></span> <span data-ttu-id="96d89-273">Ez érhető el, használja a [Set-DPMProtectionGroup](https://technet.microsoft.com/library/hh881758) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="96d89-273">This can be achieved using the [Set-DPMProtectionGroup](https://technet.microsoft.com/library/hh881758) cmdlet.</span></span>

```
PS C:\> Set-DPMProtectionGroup -ProtectionGroup $MPG
```
## <a name="view-the-backup-points"></a><span data-ttu-id="96d89-274">A biztonsági mentési pontok megtekintése</span><span class="sxs-lookup"><span data-stu-id="96d89-274">View the backup points</span></span>
<span data-ttu-id="96d89-275">Használhatja a [Get-DPMRecoveryPoint](https://technet.microsoft.com/library/hh881746) parancsmag listájának összes helyreállítási pont egy adatforrás.</span><span class="sxs-lookup"><span data-stu-id="96d89-275">You can use the [Get-DPMRecoveryPoint](https://technet.microsoft.com/library/hh881746) cmdlet to get a list of all recovery points for a datasource.</span></span> <span data-ttu-id="96d89-276">Ebben a példában a következő történik:</span><span class="sxs-lookup"><span data-stu-id="96d89-276">In this example, we will:</span></span>

* <span data-ttu-id="96d89-277">a PGs beolvasni a DPM-kiszolgálón, és egy tömbben található```$PG```</span><span class="sxs-lookup"><span data-stu-id="96d89-277">fetch all the PGs on the DPM server and stored in an array ```$PG```</span></span>
* <span data-ttu-id="96d89-278">a megfelelő adatforrások beolvasása az```$PG[0]```</span><span class="sxs-lookup"><span data-stu-id="96d89-278">get the datasources corresponding to the ```$PG[0]```</span></span>
* <span data-ttu-id="96d89-279">a helyreállítási pontok lekérése egy adatforrás.</span><span class="sxs-lookup"><span data-stu-id="96d89-279">get all the recovery points for a datasource.</span></span>

```
PS C:\> $PG = Get-DPMProtectionGroup –DPMServerName "TestingServer"
PS C:\> $DS = Get-DPMDatasource -ProtectionGroup $PG[0]
PS C:\> $RecoveryPoints = Get-DPMRecoverypoint -Datasource $DS[0] -Online
```

## <a name="restore-data-protected-on-azure"></a><span data-ttu-id="96d89-280">Az Azure-on védett adatok visszaállítása</span><span class="sxs-lookup"><span data-stu-id="96d89-280">Restore data protected on Azure</span></span>
<span data-ttu-id="96d89-281">Adat-visszaállítást a rendszer kombinációja egy ```RecoverableItem``` objektum és a ```RecoveryOption``` objektum.</span><span class="sxs-lookup"><span data-stu-id="96d89-281">Restoring data is a combination of a ```RecoverableItem``` object and a ```RecoveryOption``` object.</span></span> <span data-ttu-id="96d89-282">Az előző szakaszban egy adatforrás azt kapott a biztonsági mentési pontok listáját.</span><span class="sxs-lookup"><span data-stu-id="96d89-282">In the previous section, we got a list of the backup points for a datasource.</span></span>

<span data-ttu-id="96d89-283">Az alábbi példában bemutatjuk, hogyan visszaállíthatók Hyper-V rendszerű virtuális gép Azure Backup szolgáltatás biztonsági mentési pontok kombinálásával a Helyreállítás célhelye.</span><span class="sxs-lookup"><span data-stu-id="96d89-283">In the example below, we demonstrate how to restore a Hyper-V virtual machine from Azure Backup by combining backup points with the target for recovery.</span></span> <span data-ttu-id="96d89-284">Ebben a példában a következőket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="96d89-284">This example includes:</span></span>

* <span data-ttu-id="96d89-285">Egy helyreállítási lehetőség segítségével létrehozása a [New-DPMRecoveryOption](https://technet.microsoft.com/library/hh881592) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="96d89-285">Creating a recovery option using the  [New-DPMRecoveryOption](https://technet.microsoft.com/library/hh881592) cmdlet.</span></span>
* <span data-ttu-id="96d89-286">Segítségével biztonsági mentési pontok tömbjének beolvasása a ```Get-DPMRecoveryPoint``` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="96d89-286">Fetching the array of backup points using the ```Get-DPMRecoveryPoint``` cmdlet.</span></span>
* <span data-ttu-id="96d89-287">A visszaállítandó biztonsági mentési pontok kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="96d89-287">Choosing a backup point to restore from.</span></span>

```
PS C:\> $RecoveryOption = New-DPMRecoveryOption -HyperVDatasource -TargetServer "HVDCenter02" -RecoveryLocation AlternateHyperVServer -RecoveryType Recover -TargetLocation “C:\VMRecovery”

PS C:\> $PG = Get-DPMProtectionGroup –DPMServerName "TestingServer"
PS C:\> $DS = Get-DPMDatasource -ProtectionGroup $PG[0]
PS C:\> $RecoveryPoints = Get-DPMRecoverypoint -Datasource $DS[0] -Online

PS C:\> Restore-DPMRecoverableItem -RecoverableItem $RecoveryPoints[0] -RecoveryOption $RecoveryOption
```

<span data-ttu-id="96d89-288">A parancsok könnyen bővíthető bármely adatforrástípusnál.</span><span class="sxs-lookup"><span data-stu-id="96d89-288">The commands can easily be extended for any datasource type.</span></span>

## <a name="next-steps"></a><span data-ttu-id="96d89-289">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="96d89-289">Next steps</span></span>
* <span data-ttu-id="96d89-290">További információ a DPM az Azure Backup szolgáltatásba: [Bevezetés a DPM biztonsági mentése](backup-azure-dpm-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="96d89-290">For more information about DPM to Azure Backup see [Introduction to DPM Backup](backup-azure-dpm-introduction.md)</span></span>
