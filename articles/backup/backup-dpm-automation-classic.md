---
title: "Az Azure Backup: A PowerShell szolgáltatás használatával a DPM-munkaterhelések biztonsági mentése |} Microsoft Docs"
description: "Megtudhatja, hogyan telepíthetnek és kezelhetnek az Azure Backup a Data Protection Manager (DPM) PowerShell használatával"
services: backup
documentationcenter: 
author: Nkolli1
manager: shreeshd
editor: 
ms.assetid: bcbcef79-9d33-4e84-a558-9866614f2cae
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: nkolli;trinadhk;anuragm;markgal
ms.openlocfilehash: 943a12dcba49a114d206b9dab968da332ea99926
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="deploy-and-manage-backup-to-azure-for-data-protection-manager-dpm-servers-using-powershell"></a><span data-ttu-id="aba34-103">Az Azure-ba történő biztonsági mentés üzembe helyezése és kezelése DPM-kiszolgálókon a PowerShell-lel</span><span class="sxs-lookup"><span data-stu-id="aba34-103">Deploy and manage backup to Azure for Data Protection Manager (DPM) servers using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="aba34-104">ARM</span><span class="sxs-lookup"><span data-stu-id="aba34-104">ARM</span></span>](backup-dpm-automation.md)
> * [<span data-ttu-id="aba34-105">Klasszikus</span><span class="sxs-lookup"><span data-stu-id="aba34-105">Classic</span></span>](backup-dpm-automation-classic.md)
>
>

<span data-ttu-id="aba34-106">Ez a cikk ismerteti, hogyan PowerShell biztonsági mentése és helyreállítása a DPM-adatok a biztonsági mentési tárolóból.</span><span class="sxs-lookup"><span data-stu-id="aba34-106">This article explains how to use PowerShell to back up and recover DPM data from a backup vault.</span></span> <span data-ttu-id="aba34-107">A Microsoft azt javasolja, hogy az összes új központi telepítéseknél Recovery Services-tárolók használatával.</span><span class="sxs-lookup"><span data-stu-id="aba34-107">Microsoft recommends using Recovery Services vaults for all new deployments.</span></span> <span data-ttu-id="aba34-108">Ha egy új Azure Backup-felhasználó, használja a cikk [központi telepítése és kezelése az Azure PowerShell használatával a Data Protection Manager adatok](backup-dpm-automation.md), így az adatok tárolása a Recovery Services-tároló.</span><span class="sxs-lookup"><span data-stu-id="aba34-108">If you are a new Azure Backup user, use the article, [Deploy and manage Data Protection Manager data to Azure using PowerShell](backup-dpm-automation.md), so you store your data in a Recovery Services vault.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="aba34-109">A biztonsági mentési tárolókról mostantól lehetőség van Recovery Services-tárolókra váltani.</span><span class="sxs-lookup"><span data-stu-id="aba34-109">You can now upgrade your Backup vaults to Recovery Services vaults.</span></span> <span data-ttu-id="aba34-110">A részletekről bővebben az [Váltás biztonsági mentési tárolóról Recovery Services-tárolóra](backup-azure-upgrade-backup-to-recovery-services.md) című cikkben olvashat.</span><span class="sxs-lookup"><span data-stu-id="aba34-110">For details, see the article [Upgrade a Backup vault to a Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md).</span></span> <span data-ttu-id="aba34-111">A Microsoft azt javasolja, hogy a biztonsági mentési tárolóról váltson Recovery Services-tárolóra.</span><span class="sxs-lookup"><span data-stu-id="aba34-111">Microsoft encourages you to upgrade your Backup vaults to Recovery Services vaults.</span></span> <span data-ttu-id="aba34-112">2017. október 15-től a PowerShell nem használható Backup-tárolók létrehozására.</span><span class="sxs-lookup"><span data-stu-id="aba34-112">After October 15, 2017, you can’t use PowerShell to create Backup vaults.</span></span> <span data-ttu-id="aba34-113">**2017. november 1-től**:</span><span class="sxs-lookup"><span data-stu-id="aba34-113">**By November 1, 2017**:</span></span>
>- <span data-ttu-id="aba34-114">Minden fennmaradó Backup-tároló automatikusan Recovery Services-tárolóra frissül.</span><span class="sxs-lookup"><span data-stu-id="aba34-114">All remaining Backup vaults will be automatically upgraded to Recovery Services vaults.</span></span>
>- <span data-ttu-id="aba34-115">A klasszikus portálon nem lehet majd hozzáférni a biztonsági másolati adatokhoz.</span><span class="sxs-lookup"><span data-stu-id="aba34-115">You won't be able to access your backup data in the classic portal.</span></span> <span data-ttu-id="aba34-116">Helyette az Azure Portal segítségével férhet hozzá a Recovery Services-tárolókban található biztonsági mentési adatokhoz.</span><span class="sxs-lookup"><span data-stu-id="aba34-116">Instead, use the Azure portal to access your backup data in Recovery Services vaults.</span></span>
>

## <a name="setting-up-the-powershell-environment"></a><span data-ttu-id="aba34-117">A PowerShell-környezet létrehozása</span><span class="sxs-lookup"><span data-stu-id="aba34-117">Setting up the PowerShell environment</span></span>
[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-include.md)]

<span data-ttu-id="aba34-118">Mielőtt a PowerShell használatával a Data Protection Manager biztonsági mentések kezelése az Azure-ba, szüksége lesz a megfelelő környezete van, a PowerShell.</span><span class="sxs-lookup"><span data-stu-id="aba34-118">Before you can use PowerShell to manage backups from Data Protection Manager to Azure, you will need to have the right environment in PowerShell.</span></span> <span data-ttu-id="aba34-119">A PowerShell-munkamenet elején győződjön meg arról, hogy futtatja a következő parancsot a megfelelő modul importálása és engedélyezi, hogy helyesen hivatkozik a DPM-parancsmagokat:</span><span class="sxs-lookup"><span data-stu-id="aba34-119">At the start of the PowerShell session, ensure that you run the following command to import the right modules and allow you to correctly reference the DPM cmdlets:</span></span>

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

## <a name="setup-and-registration"></a><span data-ttu-id="aba34-120">Telepítését és regisztrálását</span><span class="sxs-lookup"><span data-stu-id="aba34-120">Setup and Registration</span></span>
<span data-ttu-id="aba34-121">Megkezdéséhez:</span><span class="sxs-lookup"><span data-stu-id="aba34-121">To begin:</span></span>

1. <span data-ttu-id="aba34-122">[Töltse le a legfrissebb PowerShell](https://github.com/Azure/azure-powershell/releases) (szükséges minimális verziója: 1.0.0)</span><span class="sxs-lookup"><span data-stu-id="aba34-122">[Download latest PowerShell](https://github.com/Azure/azure-powershell/releases) (minimum version required is : 1.0.0)</span></span>
2. <span data-ttu-id="aba34-123">Engedélyezze az Azure Backup szolgáltatás parancsmagjaival átváltás *AzureResourceManager* mód használatával a **Switch-AzureMode** parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="aba34-123">Enable the Azure Backup commandlets by switching to *AzureResourceManager* mode by using the **Switch-AzureMode** commandlet:</span></span>

```
PS C:\> Switch-AzureMode AzureResourceManager
```

<span data-ttu-id="aba34-124">A következő telepítését és regisztrálását feladatok automatizálhatók a PowerShell használatával:</span><span class="sxs-lookup"><span data-stu-id="aba34-124">The following setup and registration tasks can be automated with PowerShell:</span></span>

* <span data-ttu-id="aba34-125">Backup-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="aba34-125">Create a backup vault</span></span>
* <span data-ttu-id="aba34-126">Az Azure Backup szolgáltatás ügynökének telepítése</span><span class="sxs-lookup"><span data-stu-id="aba34-126">Installing the Azure Backup agent</span></span>
* <span data-ttu-id="aba34-127">Az Azure Backup szolgáltatás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="aba34-127">Registering with the Azure Backup service</span></span>
* <span data-ttu-id="aba34-128">Hálózati beállítások</span><span class="sxs-lookup"><span data-stu-id="aba34-128">Networking settings</span></span>
* <span data-ttu-id="aba34-129">Titkosítási beállítások</span><span class="sxs-lookup"><span data-stu-id="aba34-129">Encryption settings</span></span>

### <a name="create-a-backup-vault"></a><span data-ttu-id="aba34-130">Backup-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="aba34-130">Create a backup vault</span></span>
> [!WARNING]
> <span data-ttu-id="aba34-131">Az ügyfelek először az Azure Backup segítségével az Azure Backup-szolgáltató az előfizetéshez használandó regisztrálnia kell.</span><span class="sxs-lookup"><span data-stu-id="aba34-131">For customers using Azure Backup for the first time, you need to register the Azure Backup provider to be used with your subscription.</span></span> <span data-ttu-id="aba34-132">Ezt megteheti a következő parancs futtatásával: Register-AzureProvider - ProviderNamespace "Microsoft.Backup"</span><span class="sxs-lookup"><span data-stu-id="aba34-132">This can be done by running the following command: Register-AzureProvider -ProviderNamespace "Microsoft.Backup"</span></span>
>
>

<span data-ttu-id="aba34-133">Létrehozhat egy új mentési tárolót használ a **New-AzureRMBackupVault** parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="aba34-133">You can create a new backup vault using the **New-AzureRMBackupVault** commandlet.</span></span> <span data-ttu-id="aba34-134">A mentési tároló egy ARM-erőforrás, ezért el kell helyezni az erőforráscsoporton belül.</span><span class="sxs-lookup"><span data-stu-id="aba34-134">The backup vault is an ARM resource, so you need to place it within a Resource Group.</span></span> <span data-ttu-id="aba34-135">Egy rendszergazda jogú Azure PowerShell-konzolban a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="aba34-135">In an elevated Azure PowerShell console, run the following commands:</span></span>

```
PS C:\> New-AzureResourceGroup –Name “test-rg” -Region “West US”
PS C:\> $backupvault = New-AzureRMBackupVault –ResourceGroupName “test-rg” –Name “test-vault” –Region “West US” –Storage GRS
```

<span data-ttu-id="aba34-136">Kaphat be egy adott előfizetés összes mentési tárolók listája az **Get-AzureRMBackupVault** parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="aba34-136">You can get a list of all the backup vaults in a given subscription using the **Get-AzureRMBackupVault** commandlet.</span></span>

### <a name="installing-the-azure-backup-agent-on-a-dpm-server"></a><span data-ttu-id="aba34-137">Az Azure Backup szolgáltatás ügynökének telepítése a DPM-kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="aba34-137">Installing the Azure Backup agent on a DPM Server</span></span>
<span data-ttu-id="aba34-138">Az Azure Backup szolgáltatás ügynökének telepítése előtt kell rendelkeznie a telepítő letöltött és található a Windows Server.</span><span class="sxs-lookup"><span data-stu-id="aba34-138">Before you install the Azure Backup agent, you need to have the installer downloaded and present on the Windows Server.</span></span> <span data-ttu-id="aba34-139">Kaphat, hogy a telepítő a legújabb verzióját a [Microsoft Download Center](http://aka.ms/azurebackup_agent) vagy a mentési tároló irányítópult-oldalon.</span><span class="sxs-lookup"><span data-stu-id="aba34-139">You can get the latest version of the installer from the [Microsoft Download Center](http://aka.ms/azurebackup_agent) or from the backup vault's Dashboard page.</span></span> <span data-ttu-id="aba34-140">A telepítő menteni egy könnyen elérhető helyre, például a * C:\Downloads\*.</span><span class="sxs-lookup"><span data-stu-id="aba34-140">Save the installer to an easily accessible location like *C:\Downloads\*.</span></span>

<span data-ttu-id="aba34-141">Az ügynök telepítéséhez futtassa a következő parancsot egy rendszergazda jogú PowerShell-konzolban **a DPM-kiszolgálón**:</span><span class="sxs-lookup"><span data-stu-id="aba34-141">To install the agent, run the following command in an elevated PowerShell console **on the DPM server**:</span></span>

```
PS C:\> MARSAgentInstaller.exe /q
```

<span data-ttu-id="aba34-142">Ezzel telepíti az ügynököt az összes alapértelmezett beállítást.</span><span class="sxs-lookup"><span data-stu-id="aba34-142">This installs the agent with all the default options.</span></span> <span data-ttu-id="aba34-143">A telepítés néhány percet vesz igénybe a háttérben.</span><span class="sxs-lookup"><span data-stu-id="aba34-143">The installation takes a few minutes in the background.</span></span> <span data-ttu-id="aba34-144">Ha nem adja meg a */nu* lehetőséget a **Windows Update** ablak nyílik meg a frissítések keresését a telepítés végén.</span><span class="sxs-lookup"><span data-stu-id="aba34-144">If you do not specify the */nu* option the **Windows Update** window will open at the end of the installation to check for any updates.</span></span>

<span data-ttu-id="aba34-145">Az ügynök a telepített programok listájában jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="aba34-145">The agent will show in the list of installed programs.</span></span> <span data-ttu-id="aba34-146">A telepített programok listájának megtekintéséhez keresse fel **Vezérlőpult** > **programok** > **programok és szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="aba34-146">To see the list of installed programs, go to **Control Panel** > **Programs** > **Programs and Features**.</span></span>

![Az ügynök telepítve](./media/backup-dpm-automation/installed-agent-listing.png)

#### <a name="installation-options"></a><span data-ttu-id="aba34-148">Telepítési beállítások</span><span class="sxs-lookup"><span data-stu-id="aba34-148">Installation options</span></span>
<span data-ttu-id="aba34-149">A parancssori keresztül elérhető lehetőségekről, használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="aba34-149">To see all the options available via the command-line, use the following command:</span></span>

```
PS C:\> MARSAgentInstaller.exe /?
```

<span data-ttu-id="aba34-150">Az elérhető lehetőségek a következők:</span><span class="sxs-lookup"><span data-stu-id="aba34-150">The available options include:</span></span>

| <span data-ttu-id="aba34-151">Beállítás</span><span class="sxs-lookup"><span data-stu-id="aba34-151">Option</span></span> | <span data-ttu-id="aba34-152">Részletek</span><span class="sxs-lookup"><span data-stu-id="aba34-152">Details</span></span> | <span data-ttu-id="aba34-153">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="aba34-153">Default</span></span> |
| --- | --- | --- |
| <span data-ttu-id="aba34-154">/q</span><span class="sxs-lookup"><span data-stu-id="aba34-154">/q</span></span> |<span data-ttu-id="aba34-155">Csendes telepítés</span><span class="sxs-lookup"><span data-stu-id="aba34-155">Quiet installation</span></span> |- |
| <span data-ttu-id="aba34-156">/ p: "hely"</span><span class="sxs-lookup"><span data-stu-id="aba34-156">/p:"location"</span></span> |<span data-ttu-id="aba34-157">Az Azure Backup szolgáltatás ügynökének a telepítési mappa elérési útja.</span><span class="sxs-lookup"><span data-stu-id="aba34-157">Path to the installation folder for the Azure Backup agent.</span></span> |<span data-ttu-id="aba34-158">C:\Program Files\Microsoft Azure Recovery Services Agent ügynök</span><span class="sxs-lookup"><span data-stu-id="aba34-158">C:\Program Files\Microsoft Azure Recovery Services Agent</span></span> |
| <span data-ttu-id="aba34-159">/ s: "hely"</span><span class="sxs-lookup"><span data-stu-id="aba34-159">/s:"location"</span></span> |<span data-ttu-id="aba34-160">Az Azure Backup szolgáltatás ügynökének a gyorsítótár mappájának elérési útja.</span><span class="sxs-lookup"><span data-stu-id="aba34-160">Path to the cache folder for the Azure Backup agent.</span></span> |<span data-ttu-id="aba34-161">C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch</span><span class="sxs-lookup"><span data-stu-id="aba34-161">C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch</span></span> |
| <span data-ttu-id="aba34-162">/m</span><span class="sxs-lookup"><span data-stu-id="aba34-162">/m</span></span> |<span data-ttu-id="aba34-163">Részvétel a Microsoft Update</span><span class="sxs-lookup"><span data-stu-id="aba34-163">Opt-in to Microsoft Update</span></span> |- |
| <span data-ttu-id="aba34-164">/Nu</span><span class="sxs-lookup"><span data-stu-id="aba34-164">/nu</span></span> |<span data-ttu-id="aba34-165">Ne keressen frissítéseket telepítésének befejezése után</span><span class="sxs-lookup"><span data-stu-id="aba34-165">Do not Check for updates after installation is complete</span></span> |- |
| <span data-ttu-id="aba34-166">/d</span><span class="sxs-lookup"><span data-stu-id="aba34-166">/d</span></span> |<span data-ttu-id="aba34-167">Eltávolítja a Microsoft Azure Recovery Services Agent ügynök</span><span class="sxs-lookup"><span data-stu-id="aba34-167">Uninstalls Microsoft Azure Recovery Services Agent</span></span> |- |
| <span data-ttu-id="aba34-168">/pH</span><span class="sxs-lookup"><span data-stu-id="aba34-168">/ph</span></span> |<span data-ttu-id="aba34-169">Állomás proxycím</span><span class="sxs-lookup"><span data-stu-id="aba34-169">Proxy Host Address</span></span> |- |
| <span data-ttu-id="aba34-170">/po</span><span class="sxs-lookup"><span data-stu-id="aba34-170">/po</span></span> |<span data-ttu-id="aba34-171">Proxy Host Port száma</span><span class="sxs-lookup"><span data-stu-id="aba34-171">Proxy Host Port Number</span></span> |- |
| <span data-ttu-id="aba34-172">/Pu</span><span class="sxs-lookup"><span data-stu-id="aba34-172">/pu</span></span> |<span data-ttu-id="aba34-173">Proxy-állomás felhasználónév</span><span class="sxs-lookup"><span data-stu-id="aba34-173">Proxy Host UserName</span></span> |- |
| <span data-ttu-id="aba34-174">/pW</span><span class="sxs-lookup"><span data-stu-id="aba34-174">/pw</span></span> |<span data-ttu-id="aba34-175">Proxy jelszava</span><span class="sxs-lookup"><span data-stu-id="aba34-175">Proxy Password</span></span> |- |

### <a name="registering-with-the-azure-backup-service"></a><span data-ttu-id="aba34-176">Az Azure Backup szolgáltatás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="aba34-176">Registering with the Azure Backup service</span></span>
<span data-ttu-id="aba34-177">Az Azure Backup szolgáltatással regisztrálhatja, mielőtt kell ahhoz, hogy a [Előfeltételek](backup-azure-dpm-introduction.md) teljesülnek.</span><span class="sxs-lookup"><span data-stu-id="aba34-177">Before you can register with the Azure Backup service, you need to ensure that the [prerequisites](backup-azure-dpm-introduction.md) are met.</span></span> <span data-ttu-id="aba34-178">A következők szükségesek:</span><span class="sxs-lookup"><span data-stu-id="aba34-178">You must:</span></span>

* <span data-ttu-id="aba34-179">Egy érvényes Azure-előfizetése</span><span class="sxs-lookup"><span data-stu-id="aba34-179">Have a valid Azure subscription</span></span>
* <span data-ttu-id="aba34-180">Rendelkezik egy biztonsági mentési tárolóval</span><span class="sxs-lookup"><span data-stu-id="aba34-180">Have a backup vault</span></span>

<span data-ttu-id="aba34-181">Töltse le a tárolói hitelesítő adatokat, futtassa a **Get-AzureBackupVaultCredentials** parancsmag egy Azure PowerShell-konzolban és a tároló azt egy tetszőleges helyen, például a * C:\Downloads\*.</span><span class="sxs-lookup"><span data-stu-id="aba34-181">To download the vault credentials, run the **Get-AzureBackupVaultCredentials** commandlet in an Azure PowerShell console and store it in a convenient location like *C:\Downloads\*.</span></span>

```
PS C:\> $credspath = "C:\"
PS C:\> $credsfilename = Get-AzureRMBackupVaultCredentials -Vault $backupvault -TargetLocation $credspath
PS C:\> $credsfilename
f5303a0b-fae4-4cdb-b44d-0e4c032dde26_backuprg_backuprn_2015-08-11--06-22-35.VaultCredentials
```

<span data-ttu-id="aba34-182">A számítógép regisztrálása a tárolóban történik használatával a [Start-DPMCloudRegistration](https://technet.microsoft.com/library/jj612787) parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="aba34-182">Registering the machine with the vault is done using the [Start-DPMCloudRegistration](https://technet.microsoft.com/library/jj612787) cmdlet:</span></span>

```
PS C:\> $cred = $credspath + $credsfilename
PS C:\> Start-DPMCloudRegistration -DPMServerName "TestingServer" -VaultCredentialsFilePath $cred
```

<span data-ttu-id="aba34-183">Ez a DPM-kiszolgáló Microsoft Azure-tárolóban megadott tárolói hitelesítő adatokat használja a "Ezután" nevű regisztrálja.</span><span class="sxs-lookup"><span data-stu-id="aba34-183">This will register the DPM Server named “TestingServer” with Microsoft Azure Vault using the specified vault credentials.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="aba34-184">Ne használjon relatív elérési utak adhatja meg a tároló hitelesítési adatait tartalmazó fájlt.</span><span class="sxs-lookup"><span data-stu-id="aba34-184">Do not use relative paths to specify the vault credentials file.</span></span> <span data-ttu-id="aba34-185">A parancsmag bemenetként abszolút elérési utat kell megadnia.</span><span class="sxs-lookup"><span data-stu-id="aba34-185">You must provide an absolute path as an input to the cmdlet.</span></span>
>
>

### <a name="initial-configuration-settings"></a><span data-ttu-id="aba34-186">Kezdeti konfigurációs beállításai</span><span class="sxs-lookup"><span data-stu-id="aba34-186">Initial configuration settings</span></span>
<span data-ttu-id="aba34-187">Miután a DPM-kiszolgáló regisztrálva van az Azure Backup-tárolóban, az alapértelmezett előfizetési beállítások fog elindulni.</span><span class="sxs-lookup"><span data-stu-id="aba34-187">Once the DPM Server is registered with the Azure Backup vault, it will start with default subscription settings.</span></span> <span data-ttu-id="aba34-188">Ezek előfizetési beállítások közé tartozik a hálózatkezelés, a titkosítás és az átmeneti területre.</span><span class="sxs-lookup"><span data-stu-id="aba34-188">These subscription settings include Networking, Encryption and the Staging area.</span></span> <span data-ttu-id="aba34-189">Az előfizetési beállítások először kapott egy leíró használatával meglévő (alapértelmezett) beállításainak módosítása a kezdéshez a [Get-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612793) parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="aba34-189">To begin changing the subscription settings you need to first get a handle on the existing (default) settings using the [Get-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612793) cmdlet:</span></span>

```
$setting = Get-DPMCloudSubscriptionSetting -DPMServerName "TestingServer"
```

<span data-ttu-id="aba34-190">Minden módosítás a helyi PowerShell objektumhoz ```$setting``` majd a teljes objektum elkötelezte magát a DPM és az Azure Backup mentse őket, és használja a [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="aba34-190">All modifications are made to this local PowerShell object ```$setting```  and then the full object is committed to DPM and Azure Backup to save them using the [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet.</span></span> <span data-ttu-id="aba34-191">Kell használnia a ```–Commit``` beállítás segítségével győződjön meg arról, hogy a változtatások megmaradnak.</span><span class="sxs-lookup"><span data-stu-id="aba34-191">You need to use the ```–Commit``` flag to ensure that the changes are persisted.</span></span> <span data-ttu-id="aba34-192">A beállítások nem is alkalmazott és Azure Backup szolgáltatás csak a véglegesített használni.</span><span class="sxs-lookup"><span data-stu-id="aba34-192">The settings will not be applied and used by Azure Backup unless committed.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -Commit
```

### <a name="networking"></a><span data-ttu-id="aba34-193">Hálózat</span><span class="sxs-lookup"><span data-stu-id="aba34-193">Networking</span></span>
<span data-ttu-id="aba34-194">Ha a DPM-számítógépen az Azure Backup szolgáltatás az interneten való csatlakoztatásához proxykiszolgálón keresztül, majd a proxykiszolgáló beállításait kell megadni a biztonsági mentések sikeres végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="aba34-194">If the connectivity of the DPM machine to the Azure Backup service on the internet is through a proxy server, then the proxy server settings should be provided for backups to succeed.</span></span> <span data-ttu-id="aba34-195">Ehhez használja a ```-ProxyServer```, ```-ProxyPort```, ```-ProxyUsername``` és a ```ProxyPassword``` paramétereket a [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="aba34-195">This is done by using the ```-ProxyServer```, ```-ProxyPort```, ```-ProxyUsername``` and the ```ProxyPassword``` parameters with the [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet.</span></span> <span data-ttu-id="aba34-196">Ebben a példában nincs proxy kiszolgáló, explicit módon azt vannak törlésével bármely proxy kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="aba34-196">In this example, there is no proxy server so we are explicitly clearing any proxy-related information.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -NoProxy
```

<span data-ttu-id="aba34-197">Sávszélesség is szabályozható a beállítások a ```-WorkHourBandwidth``` és ```-NonWorkHourBandwidth``` napokat a hét adott számú.</span><span class="sxs-lookup"><span data-stu-id="aba34-197">Bandwidth usage can also be controlled with options of ```-WorkHourBandwidth``` and ```-NonWorkHourBandwidth``` for a given set of days of the week.</span></span> <span data-ttu-id="aba34-198">Ebben a példában a Microsoft nem állítja a sávszélesség-szabályozás.</span><span class="sxs-lookup"><span data-stu-id="aba34-198">In this example we are not setting any throttling.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -NoThrottle
```

### <a name="configuring-the-staging-area"></a><span data-ttu-id="aba34-199">Az átmeneti területre konfigurálása</span><span class="sxs-lookup"><span data-stu-id="aba34-199">Configuring the staging Area</span></span>
<span data-ttu-id="aba34-200">A DPM-kiszolgálón futó Azure Backup ügynöknek szüksége van ideiglenes tárhelyre, (helyi átmeneti terület) a felhőből helyreállított adatok számára.</span><span class="sxs-lookup"><span data-stu-id="aba34-200">The Azure Backup agent running on the DPM server needs temporary storage for data restored from the cloud (local staging area).</span></span> <span data-ttu-id="aba34-201">Konfigurálhatja az átmeneti területre segítségével a [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) parancsmag és a ```-StagingAreaPath``` paraméter.</span><span class="sxs-lookup"><span data-stu-id="aba34-201">Configure the staging area using the [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet and the ```-StagingAreaPath``` parameter.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -StagingAreaPath "C:\StagingArea"
```

<span data-ttu-id="aba34-202">A fenti példában az átmeneti területre úgy lesz beállítva, *C:\StagingArea* a PowerShell objektumban ```$setting```.</span><span class="sxs-lookup"><span data-stu-id="aba34-202">In the example above, the staging area will be set to *C:\StagingArea* in the PowerShell object ```$setting```.</span></span> <span data-ttu-id="aba34-203">Győződjön meg arról, hogy a megadott mappa már létezik, vagy pedig az előfizetési beállítások végső véglegesítése sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="aba34-203">Ensure that the specified folder already exists, or else the final commit of the subscription settings will fail.</span></span>

### <a name="encryption-settings"></a><span data-ttu-id="aba34-204">Titkosítási beállítások</span><span class="sxs-lookup"><span data-stu-id="aba34-204">Encryption settings</span></span>
<span data-ttu-id="aba34-205">A biztonsági mentési Azure biztonsági mentési küldött adatok védelmét az adatok bizalmas mivoltát titkosított.</span><span class="sxs-lookup"><span data-stu-id="aba34-205">The backup data sent to Azure Backup is encrypted to protect the confidentiality of the data.</span></span> <span data-ttu-id="aba34-206">A titkosítás jelszava a "password" vissza kell fejtenie az adatokat a visszaállítás időpontjában.</span><span class="sxs-lookup"><span data-stu-id="aba34-206">The encryption passphrase is the "password" to decrypt the data at the time of restore.</span></span> <span data-ttu-id="aba34-207">Fontos tartani ezeket az információkat megbízható és biztonságos be van állítva.</span><span class="sxs-lookup"><span data-stu-id="aba34-207">It is important to keep this information safe and secure once it is set.</span></span>

<span data-ttu-id="aba34-208">Az alábbi példában a karakterlánc az első parancs konvertálása ```passphrase123456789``` egy biztonságos karakterláncot, és a biztonságos karakterláncot kell megadnia a változóhoz nevű rendel ```$Passphrase```.</span><span class="sxs-lookup"><span data-stu-id="aba34-208">In the example below, the first command converts the string ```passphrase123456789``` to a secure string and assigns the secure string to the variable named ```$Passphrase```.</span></span> <span data-ttu-id="aba34-209">a második parancs beállítja a biztonságos karakterláncot kell megadnia ```$Passphrase``` és a biztonsági másolatok titkosításához.</span><span class="sxs-lookup"><span data-stu-id="aba34-209">the second command sets the secure string in ```$Passphrase``` as the password for encrypting backups.</span></span>

```
PS C:\> $Passphrase = ConvertTo-SecureString -string "passphrase123456789" -AsPlainText -Force

PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -EncryptionPassphrase $Passphrase
```

> [!IMPORTANT]
> <span data-ttu-id="aba34-210">Jelszó információinak megtartása biztonságos be van állítva.</span><span class="sxs-lookup"><span data-stu-id="aba34-210">Keep the passphrase information safe and secure once it is set.</span></span> <span data-ttu-id="aba34-211">Nem lehet visszaállítani az adatokat az Azure-ból nélkül ezt a jelszót.</span><span class="sxs-lookup"><span data-stu-id="aba34-211">You will not be able to restore data from Azure without this passphrase.</span></span>
>
>

<span data-ttu-id="aba34-212">Ezen a ponton a szükséges módosításokat, hogy el kell a ```$setting``` objektum.</span><span class="sxs-lookup"><span data-stu-id="aba34-212">At this point, you should have made all the required changes to the ```$setting``` object.</span></span> <span data-ttu-id="aba34-213">Ne felejtse el a változtatások véglegesítése a határidő.</span><span class="sxs-lookup"><span data-stu-id="aba34-213">Remember to commit the changes.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -Commit
```

## <a name="protect-data-to-azure-backup"></a><span data-ttu-id="aba34-214">Az Azure Backup szolgáltatásba adatok védelme</span><span class="sxs-lookup"><span data-stu-id="aba34-214">Protect data to Azure Backup</span></span>
<span data-ttu-id="aba34-215">Ez a szakasz az üzemi kiszolgáló hozzáadása a DPM, és majd adatvédelemben a helyi DPM-tároló, majd az Azure Backup szolgáltatásba.</span><span class="sxs-lookup"><span data-stu-id="aba34-215">In this section, you will add a production server to DPM and then protect the data to local DPM storage and then to Azure Backup.</span></span> <span data-ttu-id="aba34-216">A példákban a fájlok és mappák biztonsági bemutatjuk.</span><span class="sxs-lookup"><span data-stu-id="aba34-216">In the examples we will demonstrate how to back up files and folders.</span></span> <span data-ttu-id="aba34-217">A logikai könnyen bővíthető minden DPM által támogatott adatforrás biztonsági mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="aba34-217">The logic can easily be extended to backup any DPM-supported data source.</span></span> <span data-ttu-id="aba34-218">A DPM biztonsági mentések által a védelmi csoport (PG) négy alkotórészek vonatkoznak:</span><span class="sxs-lookup"><span data-stu-id="aba34-218">All your DPM backups are governed by a Protection Group (PG) with four parts:</span></span>

1. <span data-ttu-id="aba34-219">**Csoport tagjai** a védhető objektumok listáját (más néven *adatforrások* a DPM-ben), amelyek ugyanabba a védelmi csoportba a védeni kívánt.</span><span class="sxs-lookup"><span data-stu-id="aba34-219">**Group members** is a list of all the protectable objects (also known as *Datasources* in DPM) that you want to protect in the same protection group.</span></span> <span data-ttu-id="aba34-220">Például előfordulhat, hogy védeni kívánt virtuális gépek éles egy védelmi csoport és egy másik védelmi csoportban található SQL Server-adatbázisok különböző biztonsági követelményeket is rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="aba34-220">For example, you may want to protect production VMs in one protection group and SQL Server databases in another protection group as they may have different backup requirements.</span></span> <span data-ttu-id="aba34-221">Előtt biztonsági másolatot készíthet a datasource egy üzemi kiszolgálón meg kell győződnie, hogy a DPM-ügynök telepítve van a kiszolgálón, és a DPM által kezelt.</span><span class="sxs-lookup"><span data-stu-id="aba34-221">Before you can back up any datasource on a production server you need to make sure the DPM Agent is installed on the server and is managed by DPM.</span></span> <span data-ttu-id="aba34-222">Kövesse a lépéseket [a DPM-ügynök telepítéséhez](https://technet.microsoft.com/library/bb870935.aspx) és kapcsolásával végezze el a megfelelő DPM-kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="aba34-222">Follow the steps for [installing the DPM Agent](https://technet.microsoft.com/library/bb870935.aspx) and linking it to the appropriate DPM Server.</span></span>
2. <span data-ttu-id="aba34-223">**Adatvédelmi módszer** határozza meg a biztonsági mentési célhelyek - szalag, a lemez és a felhőben.</span><span class="sxs-lookup"><span data-stu-id="aba34-223">**Data protection method** specifies the target backup locations - tape, disk, and cloud.</span></span> <span data-ttu-id="aba34-224">Ebben a példában azt fogja védeni az adatokat a helyi lemezek és a felhő.</span><span class="sxs-lookup"><span data-stu-id="aba34-224">In our example we will protect data to the local disk and to the cloud.</span></span>
3. <span data-ttu-id="aba34-225">A **biztonsági mentés ütemezése** , amely megadja, hogy ha a biztonsági mentést kell tenni, és milyen gyakran a szinkronizálja az adatokat a DPM-kiszolgálón és az üzemi kiszolgáló között.</span><span class="sxs-lookup"><span data-stu-id="aba34-225">A **backup schedule** that specifies when backups need to be taken and how often the data should be synchronized between the DPM Server and the production server.</span></span>
4. <span data-ttu-id="aba34-226">A **adatmegőrzési ütemterv** , amely megadja, mennyi ideig szeretné megőrizni a helyreállítási pontok az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="aba34-226">A **retention schedule** that specifies how long to retain the recovery points in Azure.</span></span>

### <a name="creating-a-protection-group"></a><span data-ttu-id="aba34-227">Védelmi csoport létrehozásakor</span><span class="sxs-lookup"><span data-stu-id="aba34-227">Creating a protection group</span></span>
<span data-ttu-id="aba34-228">Először hozzon létre egy új védelmi csoport használatával a [New-DPMProtectionGroup](https://technet.microsoft.com/library/hh881722) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="aba34-228">Start by creating a new Protection Group using the [New-DPMProtectionGroup](https://technet.microsoft.com/library/hh881722) cmdlet.</span></span>

```
PS C:\> $PG = New-DPMProtectionGroup -DPMServerName " TestingServer " -Name "ProtectGroup01"
```

<span data-ttu-id="aba34-229">A fenti parancsmag hozza létre a védelmi csoport nevű *ProtectGroup01*.</span><span class="sxs-lookup"><span data-stu-id="aba34-229">The above cmdlet will create a Protection Group named *ProtectGroup01*.</span></span> <span data-ttu-id="aba34-230">Egy meglévő védelmi csoport is módosíthatja később a biztonsági mentés vegye fel az Azure felhőbe.</span><span class="sxs-lookup"><span data-stu-id="aba34-230">An existing protection group can also be modified later to add backup to the Azure cloud.</span></span> <span data-ttu-id="aba34-231">Azonban ne módosítsa a védelmi csoport - új vagy meglévő - kell a leírót szerezni a *módosíthatóvá* objektumba a [Get-DPMModifiableProtectionGroup](https://technet.microsoft.com/library/hh881713) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="aba34-231">However, to make any changes to the Protection Group - new or existing - we need to get a handle on a *modifiable* object using the [Get-DPMModifiableProtectionGroup](https://technet.microsoft.com/library/hh881713) cmdlet.</span></span>

```
PS C:\> $MPG = Get-ModifiableProtectionGroup $PG
```

### <a name="adding-group-members-to-the-protection-group"></a><span data-ttu-id="aba34-232">Csoport tagok hozzáadása védelmi csoporthoz</span><span class="sxs-lookup"><span data-stu-id="aba34-232">Adding group members to the Protection Group</span></span>
<span data-ttu-id="aba34-233">Minden DPM-ügynök tudja azon a kiszolgálón, amelyre telepítve van, az adatforrások listája.</span><span class="sxs-lookup"><span data-stu-id="aba34-233">Each DPM Agent knows the list of datasources on the server that it is installed on.</span></span> <span data-ttu-id="aba34-234">Egy adatforrás hozzáadása a védelmi csoport, a DPM-ügynököt kell először elküldi a DPM-kiszolgálón az adatforrások listáját.</span><span class="sxs-lookup"><span data-stu-id="aba34-234">To add a datasource to the Protection Group, the DPM Agent needs to first send a list of the datasources back to the DPM server.</span></span> <span data-ttu-id="aba34-235">Egy vagy több adatforrás majd kiválasztva, és hozzáad a védelmi csoporthoz.</span><span class="sxs-lookup"><span data-stu-id="aba34-235">One or more datasources are then selected and added to the Protection Group.</span></span> <span data-ttu-id="aba34-236">A PowerShell lépéseken, amelyekkel az beszerzése elérése ennek a következők:</span><span class="sxs-lookup"><span data-stu-id="aba34-236">The PowerShell steps needed to get achieve this are:</span></span>

1. <span data-ttu-id="aba34-237">A DPM-ügynököt a DPM által kezelt összes kiszolgálók listájának beolvasása.</span><span class="sxs-lookup"><span data-stu-id="aba34-237">Fetch a list of all servers managed by DPM through the DPM Agent.</span></span>
2. <span data-ttu-id="aba34-238">Válasszon egy adott kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="aba34-238">Choose a specific server.</span></span>
3. <span data-ttu-id="aba34-239">Az összes adatforrás listájának beolvasása a kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="aba34-239">Fetch a list of all datasources on the server.</span></span>
4. <span data-ttu-id="aba34-240">Válasszon egy vagy több adatforrást, majd adja hozzá a védelmi csoport</span><span class="sxs-lookup"><span data-stu-id="aba34-240">Choose one or more datasources and add them to the Protection Group</span></span>

<span data-ttu-id="aba34-241">A DPM-ügynök telepítve van és a DPM-kiszolgáló által kezelt kiszolgálók listája szerzett a [Get-DPMProductionServer](https://technet.microsoft.com/library/hh881600) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="aba34-241">The list of servers on which the DPM Agent is installed and is being managed by the DPM Server is acquired with the [Get-DPMProductionServer](https://technet.microsoft.com/library/hh881600) cmdlet.</span></span> <span data-ttu-id="aba34-242">Ebben a példában azt szűrésére és konfigurálását elvégzi, csak PS nevű *productionserver01* a biztonsági mentéshez.</span><span class="sxs-lookup"><span data-stu-id="aba34-242">In this example we will filter and only configure PS with name *productionserver01* for backup.</span></span>

```
PS C:\> $server = Get-ProductionServer -DPMServerName "TestingServer" | where {($_.servername) –contains “productionserver01”
```

<span data-ttu-id="aba34-243">Most beolvasni az adatforrások listája ```$server``` használatával a [Get-DPMDatasource](https://technet.microsoft.com/library/hh881605) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="aba34-243">Now fetch the list of datasources on ```$server``` using the [Get-DPMDatasource](https://technet.microsoft.com/library/hh881605) cmdlet.</span></span> <span data-ttu-id="aba34-244">Ebben a példában a Microsoft jelenleg korlátozza a kötetek * D:\* amely szeretnénk konfigurálni a biztonsági mentéshez.</span><span class="sxs-lookup"><span data-stu-id="aba34-244">In this example we are filtering for the volume *D:\* which we want to configure for backup.</span></span> <span data-ttu-id="aba34-245">Ez az adatforrás kerül a védelmi csoport használatával a [Add-DPMChildDatasource](https://technet.microsoft.com/library/hh881732) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="aba34-245">This datasource is then added to the Protection Group using the [Add-DPMChildDatasource](https://technet.microsoft.com/library/hh881732) cmdlet.</span></span> <span data-ttu-id="aba34-246">Fontos, hogy a *modifable* védelmi csoport objektum ```$MPG``` , hogy a kiegészítsék.</span><span class="sxs-lookup"><span data-stu-id="aba34-246">Remember to use the *modifable* protection group object ```$MPG``` to make the additions.</span></span>

```
PS C:\> $DS = Get-Datasource -ProductionServer $server -Inquire | where { $_.Name -contains “D:\” }

PS C:\> Add-DPMChildDatasource -ProtectionGroup $MPG -ChildDatasource $DS
```

<span data-ttu-id="aba34-247">Ismételje meg ezt a lépést, ha szükséges, ahányszor, amíg az összes kiválasztott adatforrás a védelmi csoporthoz hozzáadott.</span><span class="sxs-lookup"><span data-stu-id="aba34-247">Repeat this step as many times as required, until you have added all the chosen datasources to the protection group.</span></span> <span data-ttu-id="aba34-248">Start van egy DataSource elemmel, és végezze el a védelmi csoport létrehozása a munkafolyamatot, majd később adja további adatforrások védelmi csoporthoz.</span><span class="sxs-lookup"><span data-stu-id="aba34-248">You can also start with just one datasource, and complete the workflow for creating the Protection Group, and at a later point add more datasources to the Protection Group.</span></span>

### <a name="selecting-the-data-protection-method"></a><span data-ttu-id="aba34-249">Az adatvédelmi módszer kiválasztása</span><span class="sxs-lookup"><span data-stu-id="aba34-249">Selecting the data protection method</span></span>
<span data-ttu-id="aba34-250">Miután az adatforrások érhetőek el a védelmi csoport, a következő lépéssel adhatja meg a védelmi módszer használatával-e a [Set-DPMProtectionType](https://technet.microsoft.com/library/hh881725) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="aba34-250">Once the datasources have been added to the Protection Group, the next step is to specify the protection method using the [Set-DPMProtectionType](https://technet.microsoft.com/library/hh881725) cmdlet.</span></span> <span data-ttu-id="aba34-251">Ebben a példában a védelmi csoport lesz a telepítőt a helyi lemezek és felhőbeli biztonsági mentését.</span><span class="sxs-lookup"><span data-stu-id="aba34-251">In this example, the Protection Group will be setup for local disk and cloud backup.</span></span> <span data-ttu-id="aba34-252">Is meg kell adnia a felhőbe használatával védeni kívánt adatforrást a [Add-DPMChildDatasource](https://technet.microsoft.com/library/hh881732.aspx) jelölővel - Online parancsmag.</span><span class="sxs-lookup"><span data-stu-id="aba34-252">You also need to specify the datasource that you want to protect to cloud using the [Add-DPMChildDatasource](https://technet.microsoft.com/library/hh881732.aspx) cmdlet with -Online flag.</span></span>

```
PS C:\> Set-DPMProtectionType -ProtectionGroup $MPG -ShortTerm Disk –LongTerm Online
PS C:\> Add-DPMChildDatasource -ProtectionGroup $MPG -ChildDatasource $DS –Online
```

### <a name="setting-the-retention-range"></a><span data-ttu-id="aba34-253">A megőrzési tartomány beállítása</span><span class="sxs-lookup"><span data-stu-id="aba34-253">Setting the retention range</span></span>
<span data-ttu-id="aba34-254">A biztonsági mentési pontok használatával, állítsa be a megőrzési a [Set-DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="aba34-254">Set the retention for the backup points using the [Set-DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) cmdlet.</span></span> <span data-ttu-id="aba34-255">Amíg tűnhet páratlan beállítani a megőrzési előtt a biztonsági mentés ütemezése definiálva van, használja a ```Set-DPMPolicyObjective``` parancsmag automatikusan úgy állítja be a alapértelmezett biztonsági mentés ütemezését, majd módosítható.</span><span class="sxs-lookup"><span data-stu-id="aba34-255">While it might seem odd to set the retention before the backup schedule has been defined, using the ```Set-DPMPolicyObjective``` cmdlet automatically sets a default backup schedule that can then be modified.</span></span> <span data-ttu-id="aba34-256">Mindig lehetőség a biztonsági mentés ütemezése először megadása és az adatmegőrzési után.</span><span class="sxs-lookup"><span data-stu-id="aba34-256">It is always possible to set the backup schedule first and the retention policy after.</span></span>

<span data-ttu-id="aba34-257">Az alábbi példában a parancsmag a lemezes biztonsági mentések megőrzési paramétereinek beállítása.</span><span class="sxs-lookup"><span data-stu-id="aba34-257">In the example below, the cmdlet sets the retention parameters for disk backups.</span></span> <span data-ttu-id="aba34-258">Ez fogja megőrizni biztonsági másolatokat 10 nap, és szinkronizálja az adatokat az üzemi kiszolgálón és a DPM-kiszolgáló közötti 6 óránként.</span><span class="sxs-lookup"><span data-stu-id="aba34-258">This will retain backups for 10 days, and sync data every 6 hours between the production server and the DPM server.</span></span> <span data-ttu-id="aba34-259">A ```SynchronizationFrequencyMinutes``` nem adja meg, milyen gyakran egy biztonsági mentési pont jön létre, de milyen gyakran adatokat másolja a DPM-kiszolgáló; ezzel megakadályozza, hogy a biztonsági mentések túl nagy.</span><span class="sxs-lookup"><span data-stu-id="aba34-259">The ```SynchronizationFrequencyMinutes``` doesn't define how often a backup point is created, but how often data is copied to the DPM server; this prevents backups from becoming too large.</span></span>

```
PS C:\> Set-DPMPolicyObjective –ProtectionGroup $MPG -RetentionRangeInDays 10 -SynchronizationFrequencyMinutes 360
```

<span data-ttu-id="aba34-260">Ugrás az Azure biztonsági mentés (DPM hivatkozik ezeket az Online biztonsági mentés másként) állítható be a megőrzési időtartamokat [hosszú távon szerzett-Édesapja-fia megjelenítve (GFS) megőrzési](backup-azure-backup-cloud-as-tape.md).</span><span class="sxs-lookup"><span data-stu-id="aba34-260">For backups going to Azure (DPM refers to these as Online backups) the retention ranges can be configured for [long term retention using a Grandfather-Father-Son scheme (GFS)](backup-azure-backup-cloud-as-tape.md).</span></span> <span data-ttu-id="aba34-261">Ez azt jelenti, hogy napi, heti, havi és éves adatmegőrzési szabályoknál érintő kombinált adatmegőrzési adhat meg.</span><span class="sxs-lookup"><span data-stu-id="aba34-261">That is, you can define a combined retention policy involving daily, weekly, monthly and yearly retention policies.</span></span> <span data-ttu-id="aba34-262">Ebben a példában azt az összetett megőrzési séma, amely azt szeretnénk, ha képviselő tömböt létrehozása, és adja meg a megőrzési tartomány használata a [Set-DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="aba34-262">In this example, we create an array representing the complex retention scheme that we want, and then configure the retention range using the [Set-DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) cmdlet.</span></span>

```
PS C:\> $RRlist = @()
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 180, Days)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 104, Weeks)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 60, Month)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 10, Years)
PS C:\> Set-DPMPolicyObjective –ProtectionGroup $MPG -OnlineRetentionRangeList $RRlist
```

### <a name="set-the-backup-schedule"></a><span data-ttu-id="aba34-263">A biztonsági mentési ütemezést beállítani.</span><span class="sxs-lookup"><span data-stu-id="aba34-263">Set the backup schedule</span></span>
<span data-ttu-id="aba34-264">A DPM automatikusan beállítja, egy alapértelmezett biztonsági mentés ütemezése ha megadja, hogy a védelmi cél használata a ```Set-DPMPolicyObjective``` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="aba34-264">DPM sets a default backup schedule automatically if you specify the protection objective using the ```Set-DPMPolicyObjective``` cmdlet.</span></span> <span data-ttu-id="aba34-265">Az alapértelmezett ütemezés módosításához használja a [Get-DPMPolicySchedule](https://technet.microsoft.com/library/hh881749) parancsmag követi a [Set-DPMPolicySchedule](https://technet.microsoft.com/library/hh881723) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="aba34-265">To change the default schedules, use the [Get-DPMPolicySchedule](https://technet.microsoft.com/library/hh881749) cmdlet followed by the [Set-DPMPolicySchedule](https://technet.microsoft.com/library/hh881723) cmdlet.</span></span>

```
PS C:\> $onlineSch = Get-DPMPolicySchedule -ProtectionGroup $mpg -LongTerm Online
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[0] -TimesOfDay 02:00
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[1] -TimesOfDay 02:00 -DaysOfWeek Sa,Su –Interval 1
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[2] -TimesOfDay 02:00 -RelativeIntervals First,Third –DaysOfWeek Sa
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[3] -TimesOfDay 02:00 -DaysOfMonth 2,5,8,9 -Months Jan,Jul
PS C:\> Set-DPMProtectionGroup -ProtectionGroup $MPG
```

<span data-ttu-id="aba34-266">A példában ```$onlineSch``` tömb négy elemekkel, amely tartalmazza a meglévő online védelmi ütemterv a védelmi csoport GFS sémában:</span><span class="sxs-lookup"><span data-stu-id="aba34-266">In the example above, ```$onlineSch``` is an array with four elements that contains the existing online protection schedule for the Protection Group in the GFS scheme:</span></span>

1. <span data-ttu-id="aba34-267">```$onlineSch[0]```a napi ütemezés fogja tartalmazni.</span><span class="sxs-lookup"><span data-stu-id="aba34-267">```$onlineSch[0]``` will contain the daily schedule</span></span>
2. <span data-ttu-id="aba34-268">```$onlineSch[1]```a heti ütemezés fogja tartalmazni.</span><span class="sxs-lookup"><span data-stu-id="aba34-268">```$onlineSch[1]``` will contain the weekly schedule</span></span>
3. <span data-ttu-id="aba34-269">```$onlineSch[2]```a havi ütemezések fogja tartalmazni.</span><span class="sxs-lookup"><span data-stu-id="aba34-269">```$onlineSch[2]``` will contain the monthly schedule</span></span>
4. <span data-ttu-id="aba34-270">```$onlineSch[3]```az éves ütemezés fogja tartalmazni.</span><span class="sxs-lookup"><span data-stu-id="aba34-270">```$onlineSch[3]``` will contain the yearly schedule</span></span>

<span data-ttu-id="aba34-271">Így ha módosítania kell a heti ütemezés, akkor tekintse meg a ```$onlineSch[1]```.</span><span class="sxs-lookup"><span data-stu-id="aba34-271">So if you need to modify the weekly schedule, you need to refer to the ```$onlineSch[1]```.</span></span>

### <a name="initial-backup"></a><span data-ttu-id="aba34-272">Kezdeti biztonsági mentés</span><span class="sxs-lookup"><span data-stu-id="aba34-272">Initial backup</span></span>
<span data-ttu-id="aba34-273">Amikor első alkalommal a biztonsági másolatot a datasource, DPM-nek egy kezdeti replika, amelyet létre fog hozni a védeni kívánt adatforrás másolatát a DPM-replikakötet létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="aba34-273">When backing up a datasource for the first time, DPM needs to create an initial replica which will create a copy of the datasource to be protected on DPM replica volume.</span></span> <span data-ttu-id="aba34-274">Ez a tevékenység vagy egy adott időpont ütemezhetők, vagy manuálisan is elindítható, használja a [Set-DPMReplicaCreationMethod](https://technet.microsoft.com/library/hh881715) parancsmag és paraméter ```-NOW```.</span><span class="sxs-lookup"><span data-stu-id="aba34-274">This activity can either be scheduled for a specific time, or can be triggered manually, using the [Set-DPMReplicaCreationMethod](https://technet.microsoft.com/library/hh881715) cmdlet with the parameter ```-NOW```.</span></span>

```
PS C:\> Set-DPMReplicaCreationMethod -ProtectionGroup $MPG -NOW
```
### <a name="changing-the-size-of-dpm-replica--recovery-point-volume"></a><span data-ttu-id="aba34-275">DPM-replika & a helyreállításipont-kötet méretének módosítása</span><span class="sxs-lookup"><span data-stu-id="aba34-275">Changing the size of DPM Replica & recovery point volume</span></span>
<span data-ttu-id="aba34-276">Is módosíthatja a méretét a DPM-replikakötet, valamint árnyékmásolat-kötet használatával [Set-DPMDatasourceDiskAllocation](https://technet.microsoft.com/library/hh881618.aspx) és a parancsmag az alábbi példában: Get-DatasourceDiskAllocation - Datasource $DS Set-datasourcediskallocation segédprogram - Datasource $DS - ProtectionGroup $MPG-manuális - ReplicaArea (2 gb) - ShadowCopyArea (2 gb)</span><span class="sxs-lookup"><span data-stu-id="aba34-276">You can also change the size of DPM Replica volume as well as Shadow Copy volume using [Set-DPMDatasourceDiskAllocation](https://technet.microsoft.com/library/hh881618.aspx) cmdlet as in the below example: Get-DatasourceDiskAllocation -Datasource $DS Set-DatasourceDiskAllocation -Datasource $DS -ProtectionGroup $MPG -manual -ReplicaArea (2gb) -ShadowCopyArea (2gb)</span></span>

### <a name="committing-the-changes-to-the-protection-group"></a><span data-ttu-id="aba34-277">A módosítások végrehajtása a védelmi csoporthoz</span><span class="sxs-lookup"><span data-stu-id="aba34-277">Committing the changes to the Protection Group</span></span>
<span data-ttu-id="aba34-278">Végül a módosításokat kell hajtható végre, a DPM a biztonsági mentés az új védelmi csoport konfigurációja / megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="aba34-278">Finally, the changes need to be committed before DPM can take the backup per the new Protection Group configuration.</span></span> <span data-ttu-id="aba34-279">Ebben az esetben használja a [Set-DPMProtectionGroup](https://technet.microsoft.com/library/hh881758) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="aba34-279">This is done using the [Set-DPMProtectionGroup](https://technet.microsoft.com/library/hh881758) cmdlet.</span></span>

```
PS C:\> Set-DPMProtectionGroup -ProtectionGroup $MPG
```
## <a name="view-the-backup-points"></a><span data-ttu-id="aba34-280">A biztonsági mentési pontok megtekintése</span><span class="sxs-lookup"><span data-stu-id="aba34-280">View the backup points</span></span>
<span data-ttu-id="aba34-281">Használhatja a [Get-DPMRecoveryPoint](https://technet.microsoft.com/library/hh881746) parancsmag listájának összes helyreállítási pont egy adatforrás.</span><span class="sxs-lookup"><span data-stu-id="aba34-281">You can use the [Get-DPMRecoveryPoint](https://technet.microsoft.com/library/hh881746) cmdlet to get a list of all recovery points for a datasource.</span></span> <span data-ttu-id="aba34-282">Ebben a példában a következő történik:</span><span class="sxs-lookup"><span data-stu-id="aba34-282">In this example, we will:</span></span>

* <span data-ttu-id="aba34-283">a DPM-kiszolgálón tárolni a tömbben a PGs beolvasása```$PG```</span><span class="sxs-lookup"><span data-stu-id="aba34-283">fetch all the PGs on the DPM server which will be stored in an array ```$PG```</span></span>
* <span data-ttu-id="aba34-284">a megfelelő adatforrások beolvasása az```$PG[0]```</span><span class="sxs-lookup"><span data-stu-id="aba34-284">get the datasources corresponding to the ```$PG[0]```</span></span>
* <span data-ttu-id="aba34-285">a helyreállítási pontok lekérése egy adatforrás.</span><span class="sxs-lookup"><span data-stu-id="aba34-285">get all the recovery points for a datasource.</span></span>

```
PS C:\> $PG = Get-DPMProtectionGroup –DPMServerName "TestingServer"
PS C:\> $DS = Get-DPMDatasource -ProtectionGroup $PG[0]
PS C:\> $RecoveryPoints = Get-DPMRecoverypoint -Datasource $DS[0] -Online
```

## <a name="restore-data-protected-on-azure"></a><span data-ttu-id="aba34-286">Az Azure-on védett adatok visszaállítása</span><span class="sxs-lookup"><span data-stu-id="aba34-286">Restore data protected on Azure</span></span>
<span data-ttu-id="aba34-287">Adat-visszaállítást a rendszer kombinációja egy ```RecoverableItem``` objektum és a ```RecoveryOption``` objektum.</span><span class="sxs-lookup"><span data-stu-id="aba34-287">Restoring data is a combination of a ```RecoverableItem``` object and a ```RecoveryOption``` object.</span></span> <span data-ttu-id="aba34-288">Az előző szakaszban egy adatforrás azt kapott a biztonsági mentési pontok listáját.</span><span class="sxs-lookup"><span data-stu-id="aba34-288">In the previous section, we got a list of the backup points for a datasource.</span></span>

<span data-ttu-id="aba34-289">Az alábbi példában bemutatjuk, hogyan visszaállíthatók Hyper-V rendszerű virtuális gép Azure Backup szolgáltatás biztonsági mentési pontok kombinálásával a Helyreállítás célhelye.</span><span class="sxs-lookup"><span data-stu-id="aba34-289">In the example below, we demonstrate how to restore a Hyper-V virtual machine from Azure Backup by combining backup points with the target for recovery.</span></span> <span data-ttu-id="aba34-290">Ehhez a következőket:</span><span class="sxs-lookup"><span data-stu-id="aba34-290">This includes:</span></span>

* <span data-ttu-id="aba34-291">Egy helyreállítási lehetőség segítségével létrehozása a [New-DPMRecoveryOption](https://technet.microsoft.com/library/hh881592) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="aba34-291">Creating a recovery option using the  [New-DPMRecoveryOption](https://technet.microsoft.com/library/hh881592) cmdlet.</span></span>
* <span data-ttu-id="aba34-292">Segítségével biztonsági mentési pontok tömbjének beolvasása a ```Get-DPMRecoveryPoint``` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="aba34-292">Fetching the array of backup points using the ```Get-DPMRecoveryPoint``` cmdlet.</span></span>
* <span data-ttu-id="aba34-293">A visszaállítandó biztonsági mentési pontok kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="aba34-293">Choosing a backup point to restore from.</span></span>

```
PS C:\> $RecoveryOption = New-DPMRecoveryOption -HyperVDatasource -TargetServer "HVDCenter02" -RecoveryLocation AlternateHyperVServer -RecoveryType Recover -TargetLocation “C:\VMRecovery”

PS C:\> $PG = Get-DPMProtectionGroup –DPMServerName "TestingServer"
PS C:\> $DS = Get-DPMDatasource -ProtectionGroup $PG[0]
PS C:\> $RecoveryPoints = Get-DPMRecoverypoint -Datasource $DS[0] -Online

PS C:\> Restore-DPMRecoverableItem -RecoverableItem $RecoveryPoints[0] -RecoveryOption $RecoveryOption
```

<span data-ttu-id="aba34-294">A parancsok könnyen bővíthető bármely adatforrástípusnál.</span><span class="sxs-lookup"><span data-stu-id="aba34-294">The commands can easily be extended for any datasource type.</span></span>

## <a name="next-steps"></a><span data-ttu-id="aba34-295">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="aba34-295">Next steps</span></span>
* <span data-ttu-id="aba34-296">További információ a DPM Azure biztonsági mentés: [Bevezetés a DPM biztonsági mentése](backup-azure-dpm-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="aba34-296">For more information about Azure Backup for DPM see [Introduction to DPM Backup](backup-azure-dpm-introduction.md)</span></span>
