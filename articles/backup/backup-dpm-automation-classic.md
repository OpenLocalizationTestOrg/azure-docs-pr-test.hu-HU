---
title: "Az Azure Backup: Használja a Powershellt tooback mentése a DPM-munkaterhelések |} Microsoft Docs"
description: "Megtudhatja, hogyan toodeploy és kezelése az Azure Backup a Data Protection Manager (DPM) PowerShell használatával"
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
ms.openlocfilehash: 48ebe6b520857836e89749ffb6fe83d1f14c5597
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-manage-backup-tooazure-for-data-protection-manager-dpm-servers-using-powershell"></a><span data-ttu-id="0baa7-103">Központi telepítése és kezelése a PowerShell használatával a Data Protection Manager (DPM) kiszolgálók biztonsági mentési tooAzure</span><span class="sxs-lookup"><span data-stu-id="0baa7-103">Deploy and manage backup tooAzure for Data Protection Manager (DPM) servers using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="0baa7-104">ARM</span><span class="sxs-lookup"><span data-stu-id="0baa7-104">ARM</span></span>](backup-dpm-automation.md)
> * [<span data-ttu-id="0baa7-105">Klasszikus</span><span class="sxs-lookup"><span data-stu-id="0baa7-105">Classic</span></span>](backup-dpm-automation-classic.md)
>
>

<span data-ttu-id="0baa7-106">Ez a cikk azt ismerteti, hogyan toouse PowerShell tooback össze, és a DPM-adatok helyreállítása a biztonsági mentési tárolóból.</span><span class="sxs-lookup"><span data-stu-id="0baa7-106">This article explains how toouse PowerShell tooback up and recover DPM data from a backup vault.</span></span> <span data-ttu-id="0baa7-107">A Microsoft azt javasolja, hogy az összes új központi telepítéseknél Recovery Services-tárolók használatával.</span><span class="sxs-lookup"><span data-stu-id="0baa7-107">Microsoft recommends using Recovery Services vaults for all new deployments.</span></span> <span data-ttu-id="0baa7-108">Ha egy új Azure Backup-felhasználó, használja a hello a cikkben [központi telepítése és kezelése a PowerShell használatával a Data Protection Manager-adatok tooAzure](backup-dpm-automation.md), így az adatok tárolása a Recovery Services-tároló.</span><span class="sxs-lookup"><span data-stu-id="0baa7-108">If you are a new Azure Backup user, use hello article, [Deploy and manage Data Protection Manager data tooAzure using PowerShell](backup-dpm-automation.md), so you store your data in a Recovery Services vault.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0baa7-109">Most már frissítheti a mentési tárolók tooRecovery szolgáltatások tárolókban.</span><span class="sxs-lookup"><span data-stu-id="0baa7-109">You can now upgrade your Backup vaults tooRecovery Services vaults.</span></span> <span data-ttu-id="0baa7-110">További információkért lásd: hello cikk [frissíteni a biztonsági mentési tároló tooa Recovery Services-tároló](backup-azure-upgrade-backup-to-recovery-services.md).</span><span class="sxs-lookup"><span data-stu-id="0baa7-110">For details, see hello article [Upgrade a Backup vault tooa Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md).</span></span> <span data-ttu-id="0baa7-111">A Microsoft tooupgrade támogatja a mentési tárolókat tooRecovery szolgáltatások tárolók.</span><span class="sxs-lookup"><span data-stu-id="0baa7-111">Microsoft encourages you tooupgrade your Backup vaults tooRecovery Services vaults.</span></span> <span data-ttu-id="0baa7-112">2017. október 15. után PowerShell toocreate mentési tárolókban nem használható.</span><span class="sxs-lookup"><span data-stu-id="0baa7-112">After October 15, 2017, you can’t use PowerShell toocreate Backup vaults.</span></span> <span data-ttu-id="0baa7-113">**2017. november 1-től**:</span><span class="sxs-lookup"><span data-stu-id="0baa7-113">**By November 1, 2017**:</span></span>
>- <span data-ttu-id="0baa7-114">Az összes többi biztonsági mentési tárolók lesz automatikusan frissített tooRecovery szolgáltatások tárolók.</span><span class="sxs-lookup"><span data-stu-id="0baa7-114">All remaining Backup vaults will be automatically upgraded tooRecovery Services vaults.</span></span>
>- <span data-ttu-id="0baa7-115">Ön nem fogja tudni tooaccess a biztonsági mentési adatok hello a klasszikus portálon.</span><span class="sxs-lookup"><span data-stu-id="0baa7-115">You won't be able tooaccess your backup data in hello classic portal.</span></span> <span data-ttu-id="0baa7-116">Ehelyett használjon hello Azure portál tooaccess a biztonsági mentési adatok a Recovery Services-tárolók.</span><span class="sxs-lookup"><span data-stu-id="0baa7-116">Instead, use hello Azure portal tooaccess your backup data in Recovery Services vaults.</span></span>
>

## <a name="setting-up-hello-powershell-environment"></a><span data-ttu-id="0baa7-117">Hello PowerShell környezet létrehozása</span><span class="sxs-lookup"><span data-stu-id="0baa7-117">Setting up hello PowerShell environment</span></span>
[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-include.md)]

<span data-ttu-id="0baa7-118">PowerShell toomanage készített biztonsági másolat a Data Protection Manager tooAzure használata előtt kell toohave hello megfelelő környezet a PowerShellben.</span><span class="sxs-lookup"><span data-stu-id="0baa7-118">Before you can use PowerShell toomanage backups from Data Protection Manager tooAzure, you will need toohave hello right environment in PowerShell.</span></span> <span data-ttu-id="0baa7-119">Elején hello hello PowerShell-munkamenetet győződjön meg arról, hogy futtassa a következő parancs tooimport hello jobb modulok hello, és lehetővé teszik toocorrectly hivatkozás hello DPM-parancsmagokat:</span><span class="sxs-lookup"><span data-stu-id="0baa7-119">At hello start of hello PowerShell session, ensure that you run hello following command tooimport hello right modules and allow you toocorrectly reference hello DPM cmdlets:</span></span>

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

## <a name="setup-and-registration"></a><span data-ttu-id="0baa7-120">Telepítését és regisztrálását</span><span class="sxs-lookup"><span data-stu-id="0baa7-120">Setup and Registration</span></span>
<span data-ttu-id="0baa7-121">toobegin:</span><span class="sxs-lookup"><span data-stu-id="0baa7-121">toobegin:</span></span>

1. <span data-ttu-id="0baa7-122">[Töltse le a legfrissebb PowerShell](https://github.com/Azure/azure-powershell/releases) (szükséges minimális verziója: 1.0.0)</span><span class="sxs-lookup"><span data-stu-id="0baa7-122">[Download latest PowerShell](https://github.com/Azure/azure-powershell/releases) (minimum version required is : 1.0.0)</span></span>
2. <span data-ttu-id="0baa7-123">Engedélyezze a hello Azure Backup szolgáltatás parancsmagjaival váltás túl*AzureResourceManager* módból hello **Switch-AzureMode** parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="0baa7-123">Enable hello Azure Backup commandlets by switching too*AzureResourceManager* mode by using hello **Switch-AzureMode** commandlet:</span></span>

```
PS C:\> Switch-AzureMode AzureResourceManager
```

<span data-ttu-id="0baa7-124">hello a következő és nyilvántartási feladatok automatizálhatók a PowerShell használatával:</span><span class="sxs-lookup"><span data-stu-id="0baa7-124">hello following setup and registration tasks can be automated with PowerShell:</span></span>

* <span data-ttu-id="0baa7-125">Backup-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="0baa7-125">Create a backup vault</span></span>
* <span data-ttu-id="0baa7-126">Hello Azure Backup szolgáltatás ügynökének telepítése</span><span class="sxs-lookup"><span data-stu-id="0baa7-126">Installing hello Azure Backup agent</span></span>
* <span data-ttu-id="0baa7-127">Hello Azure Backup szolgáltatás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="0baa7-127">Registering with hello Azure Backup service</span></span>
* <span data-ttu-id="0baa7-128">Hálózati beállítások</span><span class="sxs-lookup"><span data-stu-id="0baa7-128">Networking settings</span></span>
* <span data-ttu-id="0baa7-129">Titkosítási beállítások</span><span class="sxs-lookup"><span data-stu-id="0baa7-129">Encryption settings</span></span>

### <a name="create-a-backup-vault"></a><span data-ttu-id="0baa7-130">Backup-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="0baa7-130">Create a backup vault</span></span>
> [!WARNING]
> <span data-ttu-id="0baa7-131">A felhasználó Azure Backup segítségével hello az első alkalommal esetén tooregister hello Azure biztonsági mentés szolgáltató toobe használja az előfizetéskor kell.</span><span class="sxs-lookup"><span data-stu-id="0baa7-131">For customers using Azure Backup for hello first time, you need tooregister hello Azure Backup provider toobe used with your subscription.</span></span> <span data-ttu-id="0baa7-132">Ezt megteheti hello a következő parancs futtatásával: Register-AzureProvider - ProviderNamespace "Microsoft.Backup"</span><span class="sxs-lookup"><span data-stu-id="0baa7-132">This can be done by running hello following command: Register-AzureProvider -ProviderNamespace "Microsoft.Backup"</span></span>
>
>

<span data-ttu-id="0baa7-133">Létrehozhat egy új mentési tárolót használó hello **New-AzureRMBackupVault** parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="0baa7-133">You can create a new backup vault using hello **New-AzureRMBackupVault** commandlet.</span></span> <span data-ttu-id="0baa7-134">hello mentési tároló egy ARM-erőforrás, ezért meg kell tooplace az erőforráscsoporton belül.</span><span class="sxs-lookup"><span data-stu-id="0baa7-134">hello backup vault is an ARM resource, so you need tooplace it within a Resource Group.</span></span> <span data-ttu-id="0baa7-135">Egy rendszergazda jogú Azure PowerShell-konzolban futtassa a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="0baa7-135">In an elevated Azure PowerShell console, run hello following commands:</span></span>

```
PS C:\> New-AzureResourceGroup –Name “test-rg” -Region “West US”
PS C:\> $backupvault = New-AzureRMBackupVault –ResourceGroupName “test-rg” –Name “test-vault” –Region “West US” –Storage GRS
```

<span data-ttu-id="0baa7-136">Minden hello mentési tárolók listája egy adott feliratkozás hello olvashatók be **Get-AzureRMBackupVault** parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="0baa7-136">You can get a list of all hello backup vaults in a given subscription using hello **Get-AzureRMBackupVault** commandlet.</span></span>

### <a name="installing-hello-azure-backup-agent-on-a-dpm-server"></a><span data-ttu-id="0baa7-137">Hello Azure Backup szolgáltatás ügynökének telepítése a DPM-kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="0baa7-137">Installing hello Azure Backup agent on a DPM Server</span></span>
<span data-ttu-id="0baa7-138">Hello Azure Backup szolgáltatás ügynökének telepítése előtt kell toohave hello telepítő letöltött és a Windows Server hello megtalálható.</span><span class="sxs-lookup"><span data-stu-id="0baa7-138">Before you install hello Azure Backup agent, you need toohave hello installer downloaded and present on hello Windows Server.</span></span> <span data-ttu-id="0baa7-139">Hello installer legújabb verzióját hello letölthető hello [Microsoft Download Center](http://aka.ms/azurebackup_agent) vagy hello mentési tároló irányítópult-oldalon.</span><span class="sxs-lookup"><span data-stu-id="0baa7-139">You can get hello latest version of hello installer from hello [Microsoft Download Center](http://aka.ms/azurebackup_agent) or from hello backup vault's Dashboard page.</span></span> <span data-ttu-id="0baa7-140">Mentés hello telepítő tooan könnyen hozzáférhető helyen, például * C:\Downloads\*.</span><span class="sxs-lookup"><span data-stu-id="0baa7-140">Save hello installer tooan easily accessible location like *C:\Downloads\*.</span></span>

<span data-ttu-id="0baa7-141">tooinstall hello ügynök, futtassa a következő parancsot egy rendszergazda jogú PowerShell-konzolban hello **hello DPM-kiszolgálón**:</span><span class="sxs-lookup"><span data-stu-id="0baa7-141">tooinstall hello agent, run hello following command in an elevated PowerShell console **on hello DPM server**:</span></span>

```
PS C:\> MARSAgentInstaller.exe /q
```

<span data-ttu-id="0baa7-142">Ezzel telepít hello ügynök minden hello alapértelmezett beállításokkal.</span><span class="sxs-lookup"><span data-stu-id="0baa7-142">This installs hello agent with all hello default options.</span></span> <span data-ttu-id="0baa7-143">hello telepítési hello háttérben néhány percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="0baa7-143">hello installation takes a few minutes in hello background.</span></span> <span data-ttu-id="0baa7-144">Ha nem adja meg a hello */nu* beállítás hello **Windows Update** ablak nyílik meg hello telepítési toocheck bármely frissítések hello végén.</span><span class="sxs-lookup"><span data-stu-id="0baa7-144">If you do not specify hello */nu* option hello **Windows Update** window will open at hello end of hello installation toocheck for any updates.</span></span>

<span data-ttu-id="0baa7-145">hello ügynök telepített programok listájában hello fogja megjeleníteni.</span><span class="sxs-lookup"><span data-stu-id="0baa7-145">hello agent will show in hello list of installed programs.</span></span> <span data-ttu-id="0baa7-146">a telepített programok toosee hello listája, nyissa meg túl**Vezérlőpult** > **programok** > **programok és szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="0baa7-146">toosee hello list of installed programs, go too**Control Panel** > **Programs** > **Programs and Features**.</span></span>

![Az ügynök telepítve](./media/backup-dpm-automation/installed-agent-listing.png)

#### <a name="installation-options"></a><span data-ttu-id="0baa7-148">Telepítési beállítások</span><span class="sxs-lookup"><span data-stu-id="0baa7-148">Installation options</span></span>
<span data-ttu-id="0baa7-149">az összes elérhető beállítások hello toosee hello parancssori, használja a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="0baa7-149">toosee all hello options available via hello command-line, use hello following command:</span></span>

```
PS C:\> MARSAgentInstaller.exe /?
```

<span data-ttu-id="0baa7-150">hello elérhető lehetőségek a következők:</span><span class="sxs-lookup"><span data-stu-id="0baa7-150">hello available options include:</span></span>

| <span data-ttu-id="0baa7-151">Beállítás</span><span class="sxs-lookup"><span data-stu-id="0baa7-151">Option</span></span> | <span data-ttu-id="0baa7-152">Részletek</span><span class="sxs-lookup"><span data-stu-id="0baa7-152">Details</span></span> | <span data-ttu-id="0baa7-153">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="0baa7-153">Default</span></span> |
| --- | --- | --- |
| <span data-ttu-id="0baa7-154">/q</span><span class="sxs-lookup"><span data-stu-id="0baa7-154">/q</span></span> |<span data-ttu-id="0baa7-155">Csendes telepítés</span><span class="sxs-lookup"><span data-stu-id="0baa7-155">Quiet installation</span></span> |- |
| <span data-ttu-id="0baa7-156">/ p: "hely"</span><span class="sxs-lookup"><span data-stu-id="0baa7-156">/p:"location"</span></span> |<span data-ttu-id="0baa7-157">Elérési út toohello telepítési mappáját hello Azure Backup szolgáltatás ügynöke.</span><span class="sxs-lookup"><span data-stu-id="0baa7-157">Path toohello installation folder for hello Azure Backup agent.</span></span> |<span data-ttu-id="0baa7-158">C:\Program Files\Microsoft Azure Recovery Services Agent ügynök</span><span class="sxs-lookup"><span data-stu-id="0baa7-158">C:\Program Files\Microsoft Azure Recovery Services Agent</span></span> |
| <span data-ttu-id="0baa7-159">/ s: "hely"</span><span class="sxs-lookup"><span data-stu-id="0baa7-159">/s:"location"</span></span> |<span data-ttu-id="0baa7-160">Elérési út toohello gyorsítótármappája hello Azure Backup szolgáltatás ügynöke.</span><span class="sxs-lookup"><span data-stu-id="0baa7-160">Path toohello cache folder for hello Azure Backup agent.</span></span> |<span data-ttu-id="0baa7-161">C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch</span><span class="sxs-lookup"><span data-stu-id="0baa7-161">C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch</span></span> |
| <span data-ttu-id="0baa7-162">/m</span><span class="sxs-lookup"><span data-stu-id="0baa7-162">/m</span></span> |<span data-ttu-id="0baa7-163">Részt tooMicrosoft frissítés</span><span class="sxs-lookup"><span data-stu-id="0baa7-163">Opt-in tooMicrosoft Update</span></span> |- |
| <span data-ttu-id="0baa7-164">/Nu</span><span class="sxs-lookup"><span data-stu-id="0baa7-164">/nu</span></span> |<span data-ttu-id="0baa7-165">Ne keressen frissítéseket telepítésének befejezése után</span><span class="sxs-lookup"><span data-stu-id="0baa7-165">Do not Check for updates after installation is complete</span></span> |- |
| <span data-ttu-id="0baa7-166">/d</span><span class="sxs-lookup"><span data-stu-id="0baa7-166">/d</span></span> |<span data-ttu-id="0baa7-167">Eltávolítja a Microsoft Azure Recovery Services Agent ügynök</span><span class="sxs-lookup"><span data-stu-id="0baa7-167">Uninstalls Microsoft Azure Recovery Services Agent</span></span> |- |
| <span data-ttu-id="0baa7-168">/pH</span><span class="sxs-lookup"><span data-stu-id="0baa7-168">/ph</span></span> |<span data-ttu-id="0baa7-169">Állomás proxycím</span><span class="sxs-lookup"><span data-stu-id="0baa7-169">Proxy Host Address</span></span> |- |
| <span data-ttu-id="0baa7-170">/po</span><span class="sxs-lookup"><span data-stu-id="0baa7-170">/po</span></span> |<span data-ttu-id="0baa7-171">Proxy Host Port száma</span><span class="sxs-lookup"><span data-stu-id="0baa7-171">Proxy Host Port Number</span></span> |- |
| <span data-ttu-id="0baa7-172">/Pu</span><span class="sxs-lookup"><span data-stu-id="0baa7-172">/pu</span></span> |<span data-ttu-id="0baa7-173">Proxy-állomás felhasználónév</span><span class="sxs-lookup"><span data-stu-id="0baa7-173">Proxy Host UserName</span></span> |- |
| <span data-ttu-id="0baa7-174">/pW</span><span class="sxs-lookup"><span data-stu-id="0baa7-174">/pw</span></span> |<span data-ttu-id="0baa7-175">Proxy jelszava</span><span class="sxs-lookup"><span data-stu-id="0baa7-175">Proxy Password</span></span> |- |

### <a name="registering-with-hello-azure-backup-service"></a><span data-ttu-id="0baa7-176">Hello Azure Backup szolgáltatás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="0baa7-176">Registering with hello Azure Backup service</span></span>
<span data-ttu-id="0baa7-177">Az Azure Backup szolgáltatás hello regisztrálhatja, jóvá kell tooensure adott hello [Előfeltételek](backup-azure-dpm-introduction.md) teljesülnek.</span><span class="sxs-lookup"><span data-stu-id="0baa7-177">Before you can register with hello Azure Backup service, you need tooensure that hello [prerequisites](backup-azure-dpm-introduction.md) are met.</span></span> <span data-ttu-id="0baa7-178">A következők szükségesek:</span><span class="sxs-lookup"><span data-stu-id="0baa7-178">You must:</span></span>

* <span data-ttu-id="0baa7-179">Egy érvényes Azure-előfizetése</span><span class="sxs-lookup"><span data-stu-id="0baa7-179">Have a valid Azure subscription</span></span>
* <span data-ttu-id="0baa7-180">Rendelkezik egy biztonsági mentési tárolóval</span><span class="sxs-lookup"><span data-stu-id="0baa7-180">Have a backup vault</span></span>

<span data-ttu-id="0baa7-181">toodownload hello tárolói hitelesítő adatokat, futtassa a hello **Get-AzureBackupVaultCredentials** parancsmag egy Azure PowerShell-konzolban és a tároló azt egy tetszőleges helyen, például a * C:\Downloads\*.</span><span class="sxs-lookup"><span data-stu-id="0baa7-181">toodownload hello vault credentials, run hello **Get-AzureBackupVaultCredentials** commandlet in an Azure PowerShell console and store it in a convenient location like *C:\Downloads\*.</span></span>

```
PS C:\> $credspath = "C:\"
PS C:\> $credsfilename = Get-AzureRMBackupVaultCredentials -Vault $backupvault -TargetLocation $credspath
PS C:\> $credsfilename
f5303a0b-fae4-4cdb-b44d-0e4c032dde26_backuprg_backuprn_2015-08-11--06-22-35.VaultCredentials
```

<span data-ttu-id="0baa7-182">Regisztrálásakor hello gép hello tárolóban történik hello segítségével [Start-DPMCloudRegistration](https://technet.microsoft.com/library/jj612787) parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="0baa7-182">Registering hello machine with hello vault is done using hello [Start-DPMCloudRegistration](https://technet.microsoft.com/library/jj612787) cmdlet:</span></span>

```
PS C:\> $cred = $credspath + $credsfilename
PS C:\> Start-DPMCloudRegistration -DPMServerName "TestingServer" -VaultCredentialsFilePath $cred
```

<span data-ttu-id="0baa7-183">Regisztrálja a hello "Ezután" nevű DPM-kiszolgáló Microsoft Azure-tárolóban megadott hello használata az tároló hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="0baa7-183">This will register hello DPM Server named “TestingServer” with Microsoft Azure Vault using hello specified vault credentials.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0baa7-184">Ne használjon relatív elérési utak toospecify hello tároló hitelesítési adatait tartalmazó fájlt.</span><span class="sxs-lookup"><span data-stu-id="0baa7-184">Do not use relative paths toospecify hello vault credentials file.</span></span> <span data-ttu-id="0baa7-185">Egy bemeneti toohello parancsmagot, abszolút elérési utat kell megadnia.</span><span class="sxs-lookup"><span data-stu-id="0baa7-185">You must provide an absolute path as an input toohello cmdlet.</span></span>
>
>

### <a name="initial-configuration-settings"></a><span data-ttu-id="0baa7-186">Kezdeti konfigurációs beállításai</span><span class="sxs-lookup"><span data-stu-id="0baa7-186">Initial configuration settings</span></span>
<span data-ttu-id="0baa7-187">Miután hello DPM-kiszolgáló regisztrálva hello Azure mentési tárolóval, alapértelmezett előfizetés beállításokkal fog elindulni.</span><span class="sxs-lookup"><span data-stu-id="0baa7-187">Once hello DPM Server is registered with hello Azure Backup vault, it will start with default subscription settings.</span></span> <span data-ttu-id="0baa7-188">Ezek előfizetési beállítások közé tartozik a hálózatkezelés, a titkosítás és a hello az átmeneti területről.</span><span class="sxs-lookup"><span data-stu-id="0baa7-188">These subscription settings include Networking, Encryption and hello Staging area.</span></span> <span data-ttu-id="0baa7-189">toobegin hello előfizetési beállítások módosításának toofirst kell leírót szerezni hello meglévő (alapértelmezett) a beállításokat a hello [Get-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612793) parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="0baa7-189">toobegin changing hello subscription settings you need toofirst get a handle on hello existing (default) settings using hello [Get-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612793) cmdlet:</span></span>

```
$setting = Get-DPMCloudSubscriptionSetting -DPMServerName "TestingServer"
```

<span data-ttu-id="0baa7-190">Minden módosítások készült toothis helyi PowerShell objektum ```$setting``` , és ezután hello teljes objektum véglegesített tooDPM és az Azure Backup toosave őket hello segítségével [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="0baa7-190">All modifications are made toothis local PowerShell object ```$setting```  and then hello full object is committed tooDPM and Azure Backup toosave them using hello [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet.</span></span> <span data-ttu-id="0baa7-191">Toouse hello kell ```–Commit``` jelző tooensure, amely hello módosítások megmaradnak.</span><span class="sxs-lookup"><span data-stu-id="0baa7-191">You need toouse hello ```–Commit``` flag tooensure that hello changes are persisted.</span></span> <span data-ttu-id="0baa7-192">hello-beállítások nem is alkalmazott és Azure Backup szolgáltatás csak a véglegesített használni.</span><span class="sxs-lookup"><span data-stu-id="0baa7-192">hello settings will not be applied and used by Azure Backup unless committed.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -Commit
```

### <a name="networking"></a><span data-ttu-id="0baa7-193">Hálózat</span><span class="sxs-lookup"><span data-stu-id="0baa7-193">Networking</span></span>
<span data-ttu-id="0baa7-194">Ha hello DPM gép toohello hello Azure biztonsági mentési szolgáltatás hello kapcsolatát internet proxykiszolgálón keresztül, majd hello proxykiszolgálót kell megadni a biztonsági mentések toosucceed.</span><span class="sxs-lookup"><span data-stu-id="0baa7-194">If hello connectivity of hello DPM machine toohello Azure Backup service on hello internet is through a proxy server, then hello proxy server settings should be provided for backups toosucceed.</span></span> <span data-ttu-id="0baa7-195">Hello használata ehhez ```-ProxyServer```, ```-ProxyPort```, ```-ProxyUsername``` és hello ```ProxyPassword``` hello paramétereket [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="0baa7-195">This is done by using hello ```-ProxyServer```, ```-ProxyPort```, ```-ProxyUsername``` and hello ```ProxyPassword``` parameters with hello [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet.</span></span> <span data-ttu-id="0baa7-196">Ebben a példában nincs proxy kiszolgáló, explicit módon azt vannak törlésével bármely proxy kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="0baa7-196">In this example, there is no proxy server so we are explicitly clearing any proxy-related information.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -NoProxy
```

<span data-ttu-id="0baa7-197">Sávszélesség is szabályozható a beállítások a ```-WorkHourBandwidth``` és ```-NonWorkHourBandwidth``` egy adott objektumcsoporthoz hello hét nap.</span><span class="sxs-lookup"><span data-stu-id="0baa7-197">Bandwidth usage can also be controlled with options of ```-WorkHourBandwidth``` and ```-NonWorkHourBandwidth``` for a given set of days of hello week.</span></span> <span data-ttu-id="0baa7-198">Ebben a példában a Microsoft nem állítja a sávszélesség-szabályozás.</span><span class="sxs-lookup"><span data-stu-id="0baa7-198">In this example we are not setting any throttling.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -NoThrottle
```

### <a name="configuring-hello-staging-area"></a><span data-ttu-id="0baa7-199">Az átmeneti területről hello konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0baa7-199">Configuring hello staging Area</span></span>
<span data-ttu-id="0baa7-200">hello DPM-kiszolgálón futó hello Azure Backup ügynöknek szüksége van ideiglenes tárhelyre, (helyi átmeneti terület) hello felhőből helyreállított adatok számára.</span><span class="sxs-lookup"><span data-stu-id="0baa7-200">hello Azure Backup agent running on hello DPM server needs temporary storage for data restored from hello cloud (local staging area).</span></span> <span data-ttu-id="0baa7-201">Hello átmeneti terület használatával hello konfigurálása [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) parancsmag és hello ```-StagingAreaPath``` paraméter.</span><span class="sxs-lookup"><span data-stu-id="0baa7-201">Configure hello staging area using hello [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet and hello ```-StagingAreaPath``` parameter.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -StagingAreaPath "C:\StagingArea"
```

<span data-ttu-id="0baa7-202">Hello a fenti példában a hello átmeneti terület lesz beállítva, túl*C:\StagingArea* hello PowerShell objektumban ```$setting```.</span><span class="sxs-lookup"><span data-stu-id="0baa7-202">In hello example above, hello staging area will be set too*C:\StagingArea* in hello PowerShell object ```$setting```.</span></span> <span data-ttu-id="0baa7-203">Győződjön meg arról, hogy hello megadott mappa már létezik, vagy pedig hello előfizetési beállítások hello végső véglegesítése sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="0baa7-203">Ensure that hello specified folder already exists, or else hello final commit of hello subscription settings will fail.</span></span>

### <a name="encryption-settings"></a><span data-ttu-id="0baa7-204">Titkosítási beállítások</span><span class="sxs-lookup"><span data-stu-id="0baa7-204">Encryption settings</span></span>
<span data-ttu-id="0baa7-205">hello küldött biztonsági mentési adatok tooAzure biztonsági mentés titkosított tooprotect hello adatok bizalmas mivoltát hello.</span><span class="sxs-lookup"><span data-stu-id="0baa7-205">hello backup data sent tooAzure Backup is encrypted tooprotect hello confidentiality of hello data.</span></span> <span data-ttu-id="0baa7-206">hello titkosítási jelszó visszaállítása a hello időpontjában az hello "password" toodecrypt hello adatai.</span><span class="sxs-lookup"><span data-stu-id="0baa7-206">hello encryption passphrase is hello "password" toodecrypt hello data at hello time of restore.</span></span> <span data-ttu-id="0baa7-207">Fontos tookeep ezen információk biztonságos és biztonságos be van állítva.</span><span class="sxs-lookup"><span data-stu-id="0baa7-207">It is important tookeep this information safe and secure once it is set.</span></span>

<span data-ttu-id="0baa7-208">Hello az alábbi példában az első parancs hello alakít át hello ```passphrase123456789``` tooa biztonságos karakterlánc és rendel hello biztonságos karakterlánc toohello nevű változó ```$Passphrase```.</span><span class="sxs-lookup"><span data-stu-id="0baa7-208">In hello example below, hello first command converts hello string ```passphrase123456789``` tooa secure string and assigns hello secure string toohello variable named ```$Passphrase```.</span></span> <span data-ttu-id="0baa7-209">hello második parancs beállítja hello biztonságos karakterlánc ```$Passphrase``` hello jelszóként biztonsági mentések titkosításához.</span><span class="sxs-lookup"><span data-stu-id="0baa7-209">hello second command sets hello secure string in ```$Passphrase``` as hello password for encrypting backups.</span></span>

```
PS C:\> $Passphrase = ConvertTo-SecureString -string "passphrase123456789" -AsPlainText -Force

PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -EncryptionPassphrase $Passphrase
```

> [!IMPORTANT]
> <span data-ttu-id="0baa7-210">Biztosíthatja hello jelszót adatok biztonságos be van állítva.</span><span class="sxs-lookup"><span data-stu-id="0baa7-210">Keep hello passphrase information safe and secure once it is set.</span></span> <span data-ttu-id="0baa7-211">Csak akkor tudja toorestore adatokat az Azure-ból nélkül ezt a jelszót.</span><span class="sxs-lookup"><span data-stu-id="0baa7-211">You will not be able toorestore data from Azure without this passphrase.</span></span>
>
>

<span data-ttu-id="0baa7-212">Ezen a ponton az összes szükséges hello módosítások toohello el kell ```$setting``` objektum.</span><span class="sxs-lookup"><span data-stu-id="0baa7-212">At this point, you should have made all hello required changes toohello ```$setting``` object.</span></span> <span data-ttu-id="0baa7-213">Ne felejtse el toocommit hello módosításokat.</span><span class="sxs-lookup"><span data-stu-id="0baa7-213">Remember toocommit hello changes.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -Commit
```

## <a name="protect-data-tooazure-backup"></a><span data-ttu-id="0baa7-214">Biztonsági mentési adatok tooAzure védelme</span><span class="sxs-lookup"><span data-stu-id="0baa7-214">Protect data tooAzure Backup</span></span>
<span data-ttu-id="0baa7-215">Ebben a szakaszban egy üzemi kiszolgáló tooDPM hozzáadása, és majd védelme a DPM toolocal hello adattárolás és tooAzure biztonsági mentés.</span><span class="sxs-lookup"><span data-stu-id="0baa7-215">In this section, you will add a production server tooDPM and then protect hello data toolocal DPM storage and then tooAzure Backup.</span></span> <span data-ttu-id="0baa7-216">Hello példákban fogjuk mutatni, hogyan tooback fájlokat és mappákat.</span><span class="sxs-lookup"><span data-stu-id="0baa7-216">In hello examples we will demonstrate how tooback up files and folders.</span></span> <span data-ttu-id="0baa7-217">hello logikát is egyszerű kiterjesztett toobackup minden DPM által támogatott adatforrás használni.</span><span class="sxs-lookup"><span data-stu-id="0baa7-217">hello logic can easily be extended toobackup any DPM-supported data source.</span></span> <span data-ttu-id="0baa7-218">A DPM biztonsági mentések által a védelmi csoport (PG) négy alkotórészek vonatkoznak:</span><span class="sxs-lookup"><span data-stu-id="0baa7-218">All your DPM backups are governed by a Protection Group (PG) with four parts:</span></span>

1. <span data-ttu-id="0baa7-219">**Csoport tagjai** összes hello védhető objektum listája (más néven *adatforrások* a DPM-ben), amelyet a hello tooprotect ugyanahhoz a védelmi csoporthoz.</span><span class="sxs-lookup"><span data-stu-id="0baa7-219">**Group members** is a list of all hello protectable objects (also known as *Datasources* in DPM) that you want tooprotect in hello same protection group.</span></span> <span data-ttu-id="0baa7-220">Például érdemes lehet tooprotect üzemi virtuális gépek egy védelmi csoport és egy másik védelmi csoportban található SQL Server-adatbázisok különböző biztonsági követelményeket is rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="0baa7-220">For example, you may want tooprotect production VMs in one protection group and SQL Server databases in another protection group as they may have different backup requirements.</span></span> <span data-ttu-id="0baa7-221">Biztonsági másolatot készíthet a datasource egy üzemi kiszolgálón meg kell, hogy hello toomake a DPM-ügynök hello kiszolgálóra van telepítve, és a DPM által kezelt.</span><span class="sxs-lookup"><span data-stu-id="0baa7-221">Before you can back up any datasource on a production server you need toomake sure hello DPM Agent is installed on hello server and is managed by DPM.</span></span> <span data-ttu-id="0baa7-222">Hello utasításai [a DPM-ügynök telepítése hello](https://technet.microsoft.com/library/bb870935.aspx) és toohello kapcsolásával végezze el a megfelelő DPM-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="0baa7-222">Follow hello steps for [installing hello DPM Agent](https://technet.microsoft.com/library/bb870935.aspx) and linking it toohello appropriate DPM Server.</span></span>
2. <span data-ttu-id="0baa7-223">**Adatvédelmi módszer** hello biztonsági mentési célhelyek - szalag, a lemez és a felhő határozza meg.</span><span class="sxs-lookup"><span data-stu-id="0baa7-223">**Data protection method** specifies hello target backup locations - tape, disk, and cloud.</span></span> <span data-ttu-id="0baa7-224">Ebben a példában a Microsoft által védendő adatok toohello helyi lemez- és toohello-alapú.</span><span class="sxs-lookup"><span data-stu-id="0baa7-224">In our example we will protect data toohello local disk and toohello cloud.</span></span>
3. <span data-ttu-id="0baa7-225">A **biztonsági mentés ütemezése** , amely megadja, hogy ha a biztonsági mentések kell venni toobe, és milyen gyakran hello szinkronizálja az adatokat a DPM-kiszolgáló hello és hello az üzemi kiszolgáló között.</span><span class="sxs-lookup"><span data-stu-id="0baa7-225">A **backup schedule** that specifies when backups need toobe taken and how often hello data should be synchronized between hello DPM Server and hello production server.</span></span>
4. <span data-ttu-id="0baa7-226">A **adatmegőrzési ütemterv** , amely meghatározza, mennyi ideig tooretain hello helyreállítási pontok az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="0baa7-226">A **retention schedule** that specifies how long tooretain hello recovery points in Azure.</span></span>

### <a name="creating-a-protection-group"></a><span data-ttu-id="0baa7-227">Védelmi csoport létrehozásakor</span><span class="sxs-lookup"><span data-stu-id="0baa7-227">Creating a protection group</span></span>
<span data-ttu-id="0baa7-228">Először hozzon létre egy új védelmi csoportot a hello [New-DPMProtectionGroup](https://technet.microsoft.com/library/hh881722) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="0baa7-228">Start by creating a new Protection Group using hello [New-DPMProtectionGroup](https://technet.microsoft.com/library/hh881722) cmdlet.</span></span>

```
PS C:\> $PG = New-DPMProtectionGroup -DPMServerName " TestingServer " -Name "ProtectGroup01"
```

<span data-ttu-id="0baa7-229">hello fent parancsmag létrehoz egy védelmi csoport neve *ProtectGroup01*.</span><span class="sxs-lookup"><span data-stu-id="0baa7-229">hello above cmdlet will create a Protection Group named *ProtectGroup01*.</span></span> <span data-ttu-id="0baa7-230">Egy meglévő védelmi csoport is módosíthatja újabb tooadd biztonsági mentési toohello Azure felhőben.</span><span class="sxs-lookup"><span data-stu-id="0baa7-230">An existing protection group can also be modified later tooadd backup toohello Azure cloud.</span></span> <span data-ttu-id="0baa7-231">Azonban toomake módosításokat toohello új védelmi csoport - vagy a meglévő - igazolnia kell a tooget leírót egy *módosíthatóvá* hello használó [Get-DPMModifiableProtectionGroup](https://technet.microsoft.com/library/hh881713) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="0baa7-231">However, toomake any changes toohello Protection Group - new or existing - we need tooget a handle on a *modifiable* object using hello [Get-DPMModifiableProtectionGroup](https://technet.microsoft.com/library/hh881713) cmdlet.</span></span>

```
PS C:\> $MPG = Get-ModifiableProtectionGroup $PG
```

### <a name="adding-group-members-toohello-protection-group"></a><span data-ttu-id="0baa7-232">Csoport tagjainak toohello védelmi csoport hozzáadása</span><span class="sxs-lookup"><span data-stu-id="0baa7-232">Adding group members toohello Protection Group</span></span>
<span data-ttu-id="0baa7-233">Minden DPM-ügynök tudja hello listája adatforrások hello kiszolgálón, amelyre telepítve van.</span><span class="sxs-lookup"><span data-stu-id="0baa7-233">Each DPM Agent knows hello list of datasources on hello server that it is installed on.</span></span> <span data-ttu-id="0baa7-234">tooadd egy adatforrás toohello védelmi csoport, a DPM-ügynök igények toofirst hello hello adatforrások hátsó toohello DPM-kiszolgáló listájának küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="0baa7-234">tooadd a datasource toohello Protection Group, hello DPM Agent needs toofirst send a list of hello datasources back toohello DPM server.</span></span> <span data-ttu-id="0baa7-235">Egy vagy több adatforrás nem, akkor a kiválasztott és toohello védelmi csoporthoz hozzáadott.</span><span class="sxs-lookup"><span data-stu-id="0baa7-235">One or more datasources are then selected and added toohello Protection Group.</span></span> <span data-ttu-id="0baa7-236">hello PowerShell szükséges lépéseket tooget elérése ennek a következők:</span><span class="sxs-lookup"><span data-stu-id="0baa7-236">hello PowerShell steps needed tooget achieve this are:</span></span>

1. <span data-ttu-id="0baa7-237">Keresztül hello DPM-ügynököt a DPM által kezelt összes kiszolgálók listájának beolvasása.</span><span class="sxs-lookup"><span data-stu-id="0baa7-237">Fetch a list of all servers managed by DPM through hello DPM Agent.</span></span>
2. <span data-ttu-id="0baa7-238">Válasszon egy adott kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="0baa7-238">Choose a specific server.</span></span>
3. <span data-ttu-id="0baa7-239">Hello kiszolgálón az összes adatforrás listájának beolvasása.</span><span class="sxs-lookup"><span data-stu-id="0baa7-239">Fetch a list of all datasources on hello server.</span></span>
4. <span data-ttu-id="0baa7-240">Válasszon egy vagy több adatforrást, és vegye fel őket a védelmi csoport toohello</span><span class="sxs-lookup"><span data-stu-id="0baa7-240">Choose one or more datasources and add them toohello Protection Group</span></span>

<span data-ttu-id="0baa7-241">mely hello a DPM-ügynök telepítve van, és hello DPM-kiszolgáló által kezelt kiszolgálók listájában hello hello van megszerezve [Get-DPMProductionServer](https://technet.microsoft.com/library/hh881600) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="0baa7-241">hello list of servers on which hello DPM Agent is installed and is being managed by hello DPM Server is acquired with hello [Get-DPMProductionServer](https://technet.microsoft.com/library/hh881600) cmdlet.</span></span> <span data-ttu-id="0baa7-242">Ebben a példában azt szűrésére és konfigurálását elvégzi, csak PS nevű *productionserver01* a biztonsági mentéshez.</span><span class="sxs-lookup"><span data-stu-id="0baa7-242">In this example we will filter and only configure PS with name *productionserver01* for backup.</span></span>

```
PS C:\> $server = Get-ProductionServer -DPMServerName "TestingServer" | where {($_.servername) –contains “productionserver01”
```

<span data-ttu-id="0baa7-243">Most beolvasni adatforrások listája hello ```$server``` hello segítségével [Get-DPMDatasource](https://technet.microsoft.com/library/hh881605) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="0baa7-243">Now fetch hello list of datasources on ```$server``` using hello [Get-DPMDatasource](https://technet.microsoft.com/library/hh881605) cmdlet.</span></span> <span data-ttu-id="0baa7-244">Ebben a példában a Microsoft jelenleg korlátozza a hello kötet * D:\* azt szeretnénk, amely a biztonsági mentéshez tooconfigure.</span><span class="sxs-lookup"><span data-stu-id="0baa7-244">In this example we are filtering for hello volume *D:\* which we want tooconfigure for backup.</span></span> <span data-ttu-id="0baa7-245">Ez az adatforrás kerül toohello védelmi csoport használatával hello [Add-DPMChildDatasource](https://technet.microsoft.com/library/hh881732) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="0baa7-245">This datasource is then added toohello Protection Group using hello [Add-DPMChildDatasource](https://technet.microsoft.com/library/hh881732) cmdlet.</span></span> <span data-ttu-id="0baa7-246">Ne feledje toouse hello *modifable* védelmi csoport objektum ```$MPG``` toomake hello kiegészítéseit.</span><span class="sxs-lookup"><span data-stu-id="0baa7-246">Remember toouse hello *modifable* protection group object ```$MPG``` toomake hello additions.</span></span>

```
PS C:\> $DS = Get-Datasource -ProductionServer $server -Inquire | where { $_.Name -contains “D:\” }

PS C:\> Add-DPMChildDatasource -ProtectionGroup $MPG -ChildDatasource $DS
```

<span data-ttu-id="0baa7-247">Ismételje meg ezt a lépést, ha szükséges, ahányszor, amíg a kiválasztott adatforrások toohello védelmi csoport összes hello hozzáadását.</span><span class="sxs-lookup"><span data-stu-id="0baa7-247">Repeat this step as many times as required, until you have added all hello chosen datasources toohello protection group.</span></span> <span data-ttu-id="0baa7-248">Is csak egy adatforrást, és teljes hello munkafolyamat hello védelmi csoport létrehozásához kezdődnie, és később adja hozzá a további adatforrások toohello védelmi csoportot.</span><span class="sxs-lookup"><span data-stu-id="0baa7-248">You can also start with just one datasource, and complete hello workflow for creating hello Protection Group, and at a later point add more datasources toohello Protection Group.</span></span>

### <a name="selecting-hello-data-protection-method"></a><span data-ttu-id="0baa7-249">Hello adatvédelmi módszer kiválasztása</span><span class="sxs-lookup"><span data-stu-id="0baa7-249">Selecting hello data protection method</span></span>
<span data-ttu-id="0baa7-250">Hello adatforrások toohello védelmi csoporthoz lettek hozzáadva, miután hello következő lépésre-e toospecify hello védelmi módszer használatával hello [Set-DPMProtectionType](https://technet.microsoft.com/library/hh881725) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="0baa7-250">Once hello datasources have been added toohello Protection Group, hello next step is toospecify hello protection method using hello [Set-DPMProtectionType](https://technet.microsoft.com/library/hh881725) cmdlet.</span></span> <span data-ttu-id="0baa7-251">Ebben a példában hello védelmi csoport lesz a telepítőt a helyi lemezek és felhőbeli biztonsági mentését.</span><span class="sxs-lookup"><span data-stu-id="0baa7-251">In this example, hello Protection Group will be setup for local disk and cloud backup.</span></span> <span data-ttu-id="0baa7-252">Emellett szükség van, amelyet az hello segítségével tooprotect toocloud toospecify hello datasource [Add-DPMChildDatasource](https://technet.microsoft.com/library/hh881732.aspx) jelölővel - Online parancsmag.</span><span class="sxs-lookup"><span data-stu-id="0baa7-252">You also need toospecify hello datasource that you want tooprotect toocloud using hello [Add-DPMChildDatasource](https://technet.microsoft.com/library/hh881732.aspx) cmdlet with -Online flag.</span></span>

```
PS C:\> Set-DPMProtectionType -ProtectionGroup $MPG -ShortTerm Disk –LongTerm Online
PS C:\> Add-DPMChildDatasource -ProtectionGroup $MPG -ChildDatasource $DS –Online
```

### <a name="setting-hello-retention-range"></a><span data-ttu-id="0baa7-253">Hello megőrzési tartomány beállítása</span><span class="sxs-lookup"><span data-stu-id="0baa7-253">Setting hello retention range</span></span>
<span data-ttu-id="0baa7-254">Hello megőrzési hello biztonsági mentési pontok használatával hello beállítása [Set-DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="0baa7-254">Set hello retention for hello backup points using hello [Set-DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) cmdlet.</span></span> <span data-ttu-id="0baa7-255">Amíg páratlan tooset hello megőrzési tűnhet, mielőtt hello biztonsági mentési ütemezés definiálása hello segítségével ```Set-DPMPolicyObjective``` parancsmag automatikusan úgy állítja be a alapértelmezett biztonsági mentés ütemezését, majd módosítható.</span><span class="sxs-lookup"><span data-stu-id="0baa7-255">While it might seem odd tooset hello retention before hello backup schedule has been defined, using hello ```Set-DPMPolicyObjective``` cmdlet automatically sets a default backup schedule that can then be modified.</span></span> <span data-ttu-id="0baa7-256">Mindig lehetséges tooset hello biztonsági mentés ütemezése először és hello adatmegőrzési után.</span><span class="sxs-lookup"><span data-stu-id="0baa7-256">It is always possible tooset hello backup schedule first and hello retention policy after.</span></span>

<span data-ttu-id="0baa7-257">Hello az alábbi példában a hello parancsmag hello megőrzési paramétereinek beállítása lemezes biztonsági mentések.</span><span class="sxs-lookup"><span data-stu-id="0baa7-257">In hello example below, hello cmdlet sets hello retention parameters for disk backups.</span></span> <span data-ttu-id="0baa7-258">Ez megőrzi biztonsági mentések 10 nap, és szinkronizálja az adatokat az üzemi kiszolgáló hello és hello DPM-kiszolgáló közötti 6 óránként.</span><span class="sxs-lookup"><span data-stu-id="0baa7-258">This will retain backups for 10 days, and sync data every 6 hours between hello production server and hello DPM server.</span></span> <span data-ttu-id="0baa7-259">Hello ```SynchronizationFrequencyMinutes``` nem adja meg, milyen gyakran egy biztonsági mentési pont jön létre, de milyen gyakran adatok DPM-kiszolgálóra másolt toohello; ezzel megakadályozza, hogy a biztonsági mentések túl nagy.</span><span class="sxs-lookup"><span data-stu-id="0baa7-259">hello ```SynchronizationFrequencyMinutes``` doesn't define how often a backup point is created, but how often data is copied toohello DPM server; this prevents backups from becoming too large.</span></span>

```
PS C:\> Set-DPMPolicyObjective –ProtectionGroup $MPG -RetentionRangeInDays 10 -SynchronizationFrequencyMinutes 360
```

<span data-ttu-id="0baa7-260">A biztonsági mentésekhez tooAzure (DPM hivatkozik toothese az Online biztonsági mentés másként) fog hello megőrzési időtartamokat konfigurálhatja a [hosszú távon szerzett-Édesapja-fia megjelenítve (GFS) megőrzési](backup-azure-backup-cloud-as-tape.md).</span><span class="sxs-lookup"><span data-stu-id="0baa7-260">For backups going tooAzure (DPM refers toothese as Online backups) hello retention ranges can be configured for [long term retention using a Grandfather-Father-Son scheme (GFS)](backup-azure-backup-cloud-as-tape.md).</span></span> <span data-ttu-id="0baa7-261">Ez azt jelenti, hogy napi, heti, havi és éves adatmegőrzési szabályoknál érintő kombinált adatmegőrzési adhat meg.</span><span class="sxs-lookup"><span data-stu-id="0baa7-261">That is, you can define a combined retention policy involving daily, weekly, monthly and yearly retention policies.</span></span> <span data-ttu-id="0baa7-262">Ebben a példában azt szeretnénk hello összetett megőrzési séma képviselő tömböt létrehozása, és adja meg hello megőrzési tartományt hello [Set-DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="0baa7-262">In this example, we create an array representing hello complex retention scheme that we want, and then configure hello retention range using hello [Set-DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) cmdlet.</span></span>

```
PS C:\> $RRlist = @()
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 180, Days)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 104, Weeks)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 60, Month)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 10, Years)
PS C:\> Set-DPMPolicyObjective –ProtectionGroup $MPG -OnlineRetentionRangeList $RRlist
```

### <a name="set-hello-backup-schedule"></a><span data-ttu-id="0baa7-263">Hello biztonsági mentési ütemezés szerint</span><span class="sxs-lookup"><span data-stu-id="0baa7-263">Set hello backup schedule</span></span>
<span data-ttu-id="0baa7-264">A DPM automatikusan beállítja, egy alapértelmezett biztonsági mentés ütemezése hello védelmi célok hello segítségével noconnection ```Set-DPMPolicyObjective``` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="0baa7-264">DPM sets a default backup schedule automatically if you specify hello protection objective using hello ```Set-DPMPolicyObjective``` cmdlet.</span></span> <span data-ttu-id="0baa7-265">toochange hello alapértelmezett ütemtervét, használja a hello [Get-DPMPolicySchedule](https://technet.microsoft.com/library/hh881749) parancsmag követ hello [Set-DPMPolicySchedule](https://technet.microsoft.com/library/hh881723) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="0baa7-265">toochange hello default schedules, use hello [Get-DPMPolicySchedule](https://technet.microsoft.com/library/hh881749) cmdlet followed by hello [Set-DPMPolicySchedule](https://technet.microsoft.com/library/hh881723) cmdlet.</span></span>

```
PS C:\> $onlineSch = Get-DPMPolicySchedule -ProtectionGroup $mpg -LongTerm Online
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[0] -TimesOfDay 02:00
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[1] -TimesOfDay 02:00 -DaysOfWeek Sa,Su –Interval 1
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[2] -TimesOfDay 02:00 -RelativeIntervals First,Third –DaysOfWeek Sa
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[3] -TimesOfDay 02:00 -DaysOfMonth 2,5,8,9 -Months Jan,Jul
PS C:\> Set-DPMProtectionGroup -ProtectionGroup $MPG
```

<span data-ttu-id="0baa7-266">A fenti, hello példa ```$onlineSch``` tömb négy elemekkel, amely tartalmazza a meglévő online védelmi ütemterv hello hello védelmi csoport hello GFS rendszerben:</span><span class="sxs-lookup"><span data-stu-id="0baa7-266">In hello example above, ```$onlineSch``` is an array with four elements that contains hello existing online protection schedule for hello Protection Group in hello GFS scheme:</span></span>

1. <span data-ttu-id="0baa7-267">```$onlineSch[0]```napi ütemezés hello fogja tartalmazni.</span><span class="sxs-lookup"><span data-stu-id="0baa7-267">```$onlineSch[0]``` will contain hello daily schedule</span></span>
2. <span data-ttu-id="0baa7-268">```$onlineSch[1]```hello heti ütemezés fogja tartalmazni.</span><span class="sxs-lookup"><span data-stu-id="0baa7-268">```$onlineSch[1]``` will contain hello weekly schedule</span></span>
3. <span data-ttu-id="0baa7-269">```$onlineSch[2]```hello havi ütemezés fogja tartalmazni.</span><span class="sxs-lookup"><span data-stu-id="0baa7-269">```$onlineSch[2]``` will contain hello monthly schedule</span></span>
4. <span data-ttu-id="0baa7-270">```$onlineSch[3]```hello éves ütemezés fogja tartalmazni.</span><span class="sxs-lookup"><span data-stu-id="0baa7-270">```$onlineSch[3]``` will contain hello yearly schedule</span></span>

<span data-ttu-id="0baa7-271">Így ha toomodify hello heti ütemezés van szüksége, toorefer toohello ```$onlineSch[1]```.</span><span class="sxs-lookup"><span data-stu-id="0baa7-271">So if you need toomodify hello weekly schedule, you need toorefer toohello ```$onlineSch[1]```.</span></span>

### <a name="initial-backup"></a><span data-ttu-id="0baa7-272">Kezdeti biztonsági mentés</span><span class="sxs-lookup"><span data-stu-id="0baa7-272">Initial backup</span></span>
<span data-ttu-id="0baa7-273">Amikor első alkalommal biztonsági mentését a hello datasource, DPM-nek egy kezdeti replika, amelyet létre fog hozni hello datasource toobe védett másolatát a DPM-replikakötet toocreate.</span><span class="sxs-lookup"><span data-stu-id="0baa7-273">When backing up a datasource for hello first time, DPM needs toocreate an initial replica which will create a copy of hello datasource toobe protected on DPM replica volume.</span></span> <span data-ttu-id="0baa7-274">Ez a tevékenység vagy egy adott időpont ütemezhetők, vagy manuálisan is elindítható, hello segítségével [Set-DPMReplicaCreationMethod](https://technet.microsoft.com/library/hh881715) hello paraméterrel parancsmag ```-NOW```.</span><span class="sxs-lookup"><span data-stu-id="0baa7-274">This activity can either be scheduled for a specific time, or can be triggered manually, using hello [Set-DPMReplicaCreationMethod](https://technet.microsoft.com/library/hh881715) cmdlet with hello parameter ```-NOW```.</span></span>

```
PS C:\> Set-DPMReplicaCreationMethod -ProtectionGroup $MPG -NOW
```
### <a name="changing-hello-size-of-dpm-replica--recovery-point-volume"></a><span data-ttu-id="0baa7-275">DPM-replika & a helyreállításipont-kötet hello méretének módosítása</span><span class="sxs-lookup"><span data-stu-id="0baa7-275">Changing hello size of DPM Replica & recovery point volume</span></span>
<span data-ttu-id="0baa7-276">Hello méretét a DPM-replikakötet, valamint árnyékmásolat-kötet használatával is módosíthatja [Set-DPMDatasourceDiskAllocation](https://technet.microsoft.com/library/hh881618.aspx) parancsmag, ahogy az alábbi példában hello: Get-DatasourceDiskAllocation - Datasource $DS Set-datasourcediskallocation segédprogram - Datasource $DS - ProtectionGroup $MPG-manuális - ReplicaArea (2 gb) – ShadowCopyArea (2 gb)</span><span class="sxs-lookup"><span data-stu-id="0baa7-276">You can also change hello size of DPM Replica volume as well as Shadow Copy volume using [Set-DPMDatasourceDiskAllocation](https://technet.microsoft.com/library/hh881618.aspx) cmdlet as in hello below example: Get-DatasourceDiskAllocation -Datasource $DS Set-DatasourceDiskAllocation -Datasource $DS -ProtectionGroup $MPG -manual -ReplicaArea (2gb) -ShadowCopyArea (2gb)</span></span>

### <a name="committing-hello-changes-toohello-protection-group"></a><span data-ttu-id="0baa7-277">Hello véglegesítése toohello védelmi csoport módosítása</span><span class="sxs-lookup"><span data-stu-id="0baa7-277">Committing hello changes toohello Protection Group</span></span>
<span data-ttu-id="0baa7-278">Végezetül hello módosításokat kell a DPM hello biztonsági mentés / hello új védelmi csoport konfigurációs megkezdése előtt véglegesítése toobe.</span><span class="sxs-lookup"><span data-stu-id="0baa7-278">Finally, hello changes need toobe committed before DPM can take hello backup per hello new Protection Group configuration.</span></span> <span data-ttu-id="0baa7-279">Ebben az esetben hello segítségével [Set-DPMProtectionGroup](https://technet.microsoft.com/library/hh881758) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="0baa7-279">This is done using hello [Set-DPMProtectionGroup](https://technet.microsoft.com/library/hh881758) cmdlet.</span></span>

```
PS C:\> Set-DPMProtectionGroup -ProtectionGroup $MPG
```
## <a name="view-hello-backup-points"></a><span data-ttu-id="0baa7-280">Nézet hello biztonsági mentési pontok</span><span class="sxs-lookup"><span data-stu-id="0baa7-280">View hello backup points</span></span>
<span data-ttu-id="0baa7-281">Használhatja a hello [Get-DPMRecoveryPoint](https://technet.microsoft.com/library/hh881746) parancsmag tooget egy adatforrás összes helyreállítási pont listáját.</span><span class="sxs-lookup"><span data-stu-id="0baa7-281">You can use hello [Get-DPMRecoveryPoint](https://technet.microsoft.com/library/hh881746) cmdlet tooget a list of all recovery points for a datasource.</span></span> <span data-ttu-id="0baa7-282">Ebben a példában a következő történik:</span><span class="sxs-lookup"><span data-stu-id="0baa7-282">In this example, we will:</span></span>

* <span data-ttu-id="0baa7-283">DPM-kiszolgálón hello tömbben tárolni összes hello PGs beolvasása```$PG```</span><span class="sxs-lookup"><span data-stu-id="0baa7-283">fetch all hello PGs on hello DPM server which will be stored in an array ```$PG```</span></span>
* <span data-ttu-id="0baa7-284">hello adatforrások megfelelő toohello beolvasása```$PG[0]```</span><span class="sxs-lookup"><span data-stu-id="0baa7-284">get hello datasources corresponding toohello ```$PG[0]```</span></span>
* <span data-ttu-id="0baa7-285">egy adatforrás összes hello helyreállítási pont beolvasása.</span><span class="sxs-lookup"><span data-stu-id="0baa7-285">get all hello recovery points for a datasource.</span></span>

```
PS C:\> $PG = Get-DPMProtectionGroup –DPMServerName "TestingServer"
PS C:\> $DS = Get-DPMDatasource -ProtectionGroup $PG[0]
PS C:\> $RecoveryPoints = Get-DPMRecoverypoint -Datasource $DS[0] -Online
```

## <a name="restore-data-protected-on-azure"></a><span data-ttu-id="0baa7-286">Az Azure-on védett adatok visszaállítása</span><span class="sxs-lookup"><span data-stu-id="0baa7-286">Restore data protected on Azure</span></span>
<span data-ttu-id="0baa7-287">Adat-visszaállítást a rendszer kombinációja egy ```RecoverableItem``` objektum és a ```RecoveryOption``` objektum.</span><span class="sxs-lookup"><span data-stu-id="0baa7-287">Restoring data is a combination of a ```RecoverableItem``` object and a ```RecoveryOption``` object.</span></span> <span data-ttu-id="0baa7-288">Hello előző szakaszban egy adatforrás azt kapott hello biztonsági mentési pontok listáját.</span><span class="sxs-lookup"><span data-stu-id="0baa7-288">In hello previous section, we got a list of hello backup points for a datasource.</span></span>

<span data-ttu-id="0baa7-289">A hello az alábbi példában bemutatjuk, hogyan toorestore egy Hyper-V virtuális gép az Azure Backup szolgáltatás biztonsági mentési pontok egyesítő hello helyreállítási célként.</span><span class="sxs-lookup"><span data-stu-id="0baa7-289">In hello example below, we demonstrate how toorestore a Hyper-V virtual machine from Azure Backup by combining backup points with hello target for recovery.</span></span> <span data-ttu-id="0baa7-290">Ehhez a következőket:</span><span class="sxs-lookup"><span data-stu-id="0baa7-290">This includes:</span></span>

* <span data-ttu-id="0baa7-291">A helyreállítási lehetőséget a hello segítségével létrehozása [New-DPMRecoveryOption](https://technet.microsoft.com/library/hh881592) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="0baa7-291">Creating a recovery option using hello  [New-DPMRecoveryOption](https://technet.microsoft.com/library/hh881592) cmdlet.</span></span>
* <span data-ttu-id="0baa7-292">Hello segítségével biztonsági mentési pontok tömbjének lekérdezésekor hello ```Get-DPMRecoveryPoint``` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="0baa7-292">Fetching hello array of backup points using hello ```Get-DPMRecoveryPoint``` cmdlet.</span></span>
* <span data-ttu-id="0baa7-293">A biztonsági mentési pont toorestore a kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="0baa7-293">Choosing a backup point toorestore from.</span></span>

```
PS C:\> $RecoveryOption = New-DPMRecoveryOption -HyperVDatasource -TargetServer "HVDCenter02" -RecoveryLocation AlternateHyperVServer -RecoveryType Recover -TargetLocation “C:\VMRecovery”

PS C:\> $PG = Get-DPMProtectionGroup –DPMServerName "TestingServer"
PS C:\> $DS = Get-DPMDatasource -ProtectionGroup $PG[0]
PS C:\> $RecoveryPoints = Get-DPMRecoverypoint -Datasource $DS[0] -Online

PS C:\> Restore-DPMRecoverableItem -RecoverableItem $RecoveryPoints[0] -RecoveryOption $RecoveryOption
```

<span data-ttu-id="0baa7-294">hello parancsok könnyen bővíthető bármely adatforrástípusnál.</span><span class="sxs-lookup"><span data-stu-id="0baa7-294">hello commands can easily be extended for any datasource type.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0baa7-295">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0baa7-295">Next steps</span></span>
* <span data-ttu-id="0baa7-296">További információ a DPM Azure biztonsági mentés: [bemutatása tooDPM biztonsági mentése](backup-azure-dpm-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="0baa7-296">For more information about Azure Backup for DPM see [Introduction tooDPM Backup](backup-azure-dpm-introduction.md)</span></span>
