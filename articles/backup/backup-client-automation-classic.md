---
title: "Windows Server biztonsági mentéseit, az Azure PowerShell használatával |} Microsoft Docs"
description: "Telepítése és kezelése a PowerShell használatával Windows Server biztonsági mentés."
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
ms.assetid: e7e269e2-1f11-41a9-957b-a2155de6a1e0
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: saurse;markgal;nkolli;trinadhk
ms.openlocfilehash: a8e20356ae383ee4fa2158ea544d5d0905028124
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="deploy-and-manage-backup-to-azure-for-windows-serverwindows-client-using-powershell"></a><span data-ttu-id="814bc-103">Az Azure-ba történő biztonsági mentés üzembe helyezése és kezelése Windows Server vagy Windows-ügyfél rendszereken a PowerShell-lel</span><span class="sxs-lookup"><span data-stu-id="814bc-103">Deploy and manage backup to Azure for Windows Server/Windows Client using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="814bc-104">ARM</span><span class="sxs-lookup"><span data-stu-id="814bc-104">ARM</span></span>](backup-client-automation.md)
> * [<span data-ttu-id="814bc-105">Klasszikus</span><span class="sxs-lookup"><span data-stu-id="814bc-105">Classic</span></span>](backup-client-automation-classic.md)
>
>

<span data-ttu-id="814bc-106">Ez a cikk ismerteti, hogyan PowerShell biztonsági mentése a Windows Server vagy Windows munkaállomás-adatok a mentési tárolóban.</span><span class="sxs-lookup"><span data-stu-id="814bc-106">This article explains how to use PowerShell to back up Windows Server or Windows workstation data to a backup vault.</span></span> <span data-ttu-id="814bc-107">A Microsoft azt javasolja, hogy az összes új központi telepítéseknél Recovery Services-tárolók használatával.</span><span class="sxs-lookup"><span data-stu-id="814bc-107">Microsoft recommends using Recovery Services vaults for all new deployments.</span></span> <span data-ttu-id="814bc-108">Ha egy új Azure biztonságimásolat-felhasználó, és nem hozott létre egy mentési tárolót az előfizetéshez, használja a cikk [központi telepítése és kezelése az Azure PowerShell használatával a Data Protection Manager adatok](backup-client-automation.md) , az adatok tárolása a Recovery Services-tároló.</span><span class="sxs-lookup"><span data-stu-id="814bc-108">If you are a new Azure Backup user and have not created a backup vault in your subscription, use the article, [Deploy and manage Data Protection Manager data to Azure using PowerShell](backup-client-automation.md) so you store your data in a Recovery Services vault.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="814bc-109">A biztonsági mentési tárolókról mostantól lehetőség van Recovery Services-tárolókra váltani.</span><span class="sxs-lookup"><span data-stu-id="814bc-109">You can now upgrade your Backup vaults to Recovery Services vaults.</span></span> <span data-ttu-id="814bc-110">A részletekről bővebben az [Váltás biztonsági mentési tárolóról Recovery Services-tárolóra](backup-azure-upgrade-backup-to-recovery-services.md) című cikkben olvashat.</span><span class="sxs-lookup"><span data-stu-id="814bc-110">For details, see the article [Upgrade a Backup vault to a Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md).</span></span> <span data-ttu-id="814bc-111">A Microsoft azt javasolja, hogy a biztonsági mentési tárolóról váltson Recovery Services-tárolóra.</span><span class="sxs-lookup"><span data-stu-id="814bc-111">Microsoft encourages you to upgrade your Backup vaults to Recovery Services vaults.</span></span><br/> <span data-ttu-id="814bc-112">2017. október 15-től a PowerShell nem használható Backup-tárolók létrehozására.</span><span class="sxs-lookup"><span data-stu-id="814bc-112">After October 15, 2017, you can’t use PowerShell to create Backup vaults.</span></span> <span data-ttu-id="814bc-113">**2017. november 1-től**:</span><span class="sxs-lookup"><span data-stu-id="814bc-113">**By November 1, 2017**:</span></span>
>- <span data-ttu-id="814bc-114">Minden fennmaradó Backup-tároló automatikusan Recovery Services-tárolóra frissül.</span><span class="sxs-lookup"><span data-stu-id="814bc-114">All remaining Backup vaults will be automatically upgraded to Recovery Services vaults.</span></span>
>- <span data-ttu-id="814bc-115">A klasszikus portálon nem lehet majd hozzáférni a biztonsági másolati adatokhoz.</span><span class="sxs-lookup"><span data-stu-id="814bc-115">You won't be able to access your backup data in the classic portal.</span></span> <span data-ttu-id="814bc-116">Helyette az Azure Portal segítségével férhet hozzá a Recovery Services-tárolókban található biztonsági mentési adatokhoz.</span><span class="sxs-lookup"><span data-stu-id="814bc-116">Instead, use the Azure portal to access your backup data in Recovery Services vaults.</span></span>
>

## <a name="install-azure-powershell"></a><span data-ttu-id="814bc-117">Az Azure PowerShell telepítése</span><span class="sxs-lookup"><span data-stu-id="814bc-117">Install Azure PowerShell</span></span>
[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-include.md)]

<span data-ttu-id="814bc-118">2015 október, az Azure PowerShell 1.0-s verziójában jelent meg.</span><span class="sxs-lookup"><span data-stu-id="814bc-118">In October 2015, Azure PowerShell 1.0 was released.</span></span> <span data-ttu-id="814bc-119">Ebben a kiadásban a 0.9.8-as sikeres szabadítsa fel, és néhány jelentős módosításokat, különösen a parancsmagok elnevezési mintát állapotba.</span><span class="sxs-lookup"><span data-stu-id="814bc-119">This release succeeded the 0.9.8 release and brought about some significant changes, especially in the naming pattern of the cmdlets.</span></span> <span data-ttu-id="814bc-120">Az 1.0-ás parancsmagok az {ige}-AzureRm{főnév} elnevezési mintát használják; a 0.9.8-as nevek viszont nem tartalmazzák az **Rm** tagot (például New-AzureRmResourceGroup New-AzureResourceGroup helyett).</span><span class="sxs-lookup"><span data-stu-id="814bc-120">1.0 cmdlets follow the naming pattern {verb}-AzureRm{noun}; whereas, the 0.9.8 names do not include **Rm** (for example, New-AzureRmResourceGroup instead of New-AzureResourceGroup).</span></span> <span data-ttu-id="814bc-121">Ha az Azure PowerShell 0.9.8-as verzióját használja, először engedélyeznie kell az erőforrás-kezelői módot a **Switch-AzureMode AzureResourceManager** parancs futtatásával.</span><span class="sxs-lookup"><span data-stu-id="814bc-121">When using Azure PowerShell 0.9.8, you must first enable the Resource Manager mode by running the **Switch-AzureMode AzureResourceManager** command.</span></span> <span data-ttu-id="814bc-122">Ez a parancs nincs szükség az 1.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="814bc-122">This command is not necessary in 1.0 or later.</span></span>

<span data-ttu-id="814bc-123">Ha a parancsfájlok a 0.9.8-as írt használni kívánt környezet, 1.0-s vagy újabb környezetben körültekintően tesztelje a parancsfájlokat egy üzem előtti környezetben váratlan hatás elkerülése érdekében az éles használat előtt.</span><span class="sxs-lookup"><span data-stu-id="814bc-123">If you want to use your scripts written for the 0.9.8 environment, in the 1.0 or later environment, you should carefully test the scripts in a pre-production environment before using them in production to avoid unexpected impact.</span></span>

<span data-ttu-id="814bc-124">[Töltse le a legfrissebb verzióját a PowerShell](https://github.com/Azure/azure-powershell/releases) (szükséges minimális verziója: 1.0.0)</span><span class="sxs-lookup"><span data-stu-id="814bc-124">[Download the latest PowerShell release](https://github.com/Azure/azure-powershell/releases) (minimum version required is : 1.0.0)</span></span>

[!INCLUDE [arm-getting-setup-powershell](../../includes/arm-getting-setup-powershell.md)]

## <a name="create-a-backup-vault"></a><span data-ttu-id="814bc-125">Backup-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="814bc-125">Create a backup vault</span></span>
> [!WARNING]
> <span data-ttu-id="814bc-126">Az ügyfelek először az Azure Backup segítségével az Azure Backup-szolgáltató az előfizetéshez használandó regisztrálnia kell.</span><span class="sxs-lookup"><span data-stu-id="814bc-126">For customers using Azure Backup for the first time, you need to register the Azure Backup provider to be used with your subscription.</span></span> <span data-ttu-id="814bc-127">Ezt megteheti a következő parancs futtatásával: Register-AzureProvider - ProviderNamespace "Microsoft.Backup"</span><span class="sxs-lookup"><span data-stu-id="814bc-127">This can be done by running the following command: Register-AzureProvider -ProviderNamespace "Microsoft.Backup"</span></span>
>
>

<span data-ttu-id="814bc-128">Létrehozhat egy új mentési tárolót használ a **New-AzureRMBackupVault** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="814bc-128">You can create a new backup vault using the **New-AzureRMBackupVault** cmdlet.</span></span> <span data-ttu-id="814bc-129">A mentési tároló egy ARM-erőforrás, ezért el kell helyezni az erőforráscsoporton belül.</span><span class="sxs-lookup"><span data-stu-id="814bc-129">The backup vault is an ARM resource, so you need to place it within a Resource Group.</span></span> <span data-ttu-id="814bc-130">Egy rendszergazda jogú Azure PowerShell-konzolban a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="814bc-130">In an elevated Azure PowerShell console, run the following commands:</span></span>

```
PS C:\> New-AzureResourceGroup –Name “test-rg” -Region “West US”
PS C:\> $backupvault = New-AzureRMBackupVault –ResourceGroupName “test-rg” –Name “test-vault” –Region “West US” –Storage GeoRedundant
```

<span data-ttu-id="814bc-131">Használja a **Get-AzureRMBackupVault** parancsmagot, hogy a mentési tárolók az előfizetés listában.</span><span class="sxs-lookup"><span data-stu-id="814bc-131">Use the **Get-AzureRMBackupVault** cmdlet to list the backup vaults in a subscription.</span></span>

## <a name="installing-the-azure-backup-agent"></a><span data-ttu-id="814bc-132">Az Azure Backup szolgáltatás ügynökének telepítése</span><span class="sxs-lookup"><span data-stu-id="814bc-132">Installing the Azure Backup agent</span></span>
<span data-ttu-id="814bc-133">Az Azure Backup szolgáltatás ügynökének telepítése előtt kell rendelkeznie a telepítő letöltött és található a Windows Server.</span><span class="sxs-lookup"><span data-stu-id="814bc-133">Before you install the Azure Backup agent, you need to have the installer downloaded and present on the Windows Server.</span></span> <span data-ttu-id="814bc-134">Kaphat, hogy a telepítő a legújabb verzióját a [Microsoft Download Center](http://aka.ms/azurebackup_agent) vagy a mentési tároló irányítópult-oldalon.</span><span class="sxs-lookup"><span data-stu-id="814bc-134">You can get the latest version of the installer from the [Microsoft Download Center](http://aka.ms/azurebackup_agent) or from the backup vault's Dashboard page.</span></span> <span data-ttu-id="814bc-135">A telepítő menteni egy könnyen elérhető helyre, például a * C:\Downloads\*.</span><span class="sxs-lookup"><span data-stu-id="814bc-135">Save the installer to an easily accessible location like *C:\Downloads\*.</span></span>

<span data-ttu-id="814bc-136">Az ügynök telepítéséhez futtassa a következő parancsot egy emelt szintű PowerShell-konzolon:</span><span class="sxs-lookup"><span data-stu-id="814bc-136">To install the agent, run the following command in an elevated PowerShell console:</span></span>

```
PS C:\> MARSAgentInstaller.exe /q
```

<span data-ttu-id="814bc-137">Ezzel telepíti az ügynököt az összes alapértelmezett beállítást.</span><span class="sxs-lookup"><span data-stu-id="814bc-137">This installs the agent with all the default options.</span></span> <span data-ttu-id="814bc-138">A telepítés néhány percet vesz igénybe a háttérben.</span><span class="sxs-lookup"><span data-stu-id="814bc-138">The installation takes a few minutes in the background.</span></span> <span data-ttu-id="814bc-139">Ha nem adja meg a */nu* lehetőséget, majd a **Windows Update** ablak nyílik meg a frissítések keresését a telepítés végén.</span><span class="sxs-lookup"><span data-stu-id="814bc-139">If you do not specify the */nu* option then the **Windows Update** window will open at the end of the installation to check for any updates.</span></span> <span data-ttu-id="814bc-140">A telepítést követően az ügynök a telepített programok listájában jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="814bc-140">Once installed, the agent will show in the list of installed programs.</span></span>

<span data-ttu-id="814bc-141">A telepített programok listájának megtekintéséhez keresse fel **Vezérlőpult** > **programok** > **programok és szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="814bc-141">To see the list of installed programs, go to **Control Panel** > **Programs** > **Programs and Features**.</span></span>

![Az ügynök telepítve](./media/backup-client-automation/installed-agent-listing.png)

### <a name="installation-options"></a><span data-ttu-id="814bc-143">Telepítési beállítások</span><span class="sxs-lookup"><span data-stu-id="814bc-143">Installation options</span></span>
<span data-ttu-id="814bc-144">A parancssori keresztül elérhető lehetőségekről, használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="814bc-144">To see all the options available via the command-line, use the following command:</span></span>

```
PS C:\> MARSAgentInstaller.exe /?
```

<span data-ttu-id="814bc-145">Az elérhető lehetőségek a következők:</span><span class="sxs-lookup"><span data-stu-id="814bc-145">The available options include:</span></span>

| <span data-ttu-id="814bc-146">Beállítás</span><span class="sxs-lookup"><span data-stu-id="814bc-146">Option</span></span> | <span data-ttu-id="814bc-147">Részletek</span><span class="sxs-lookup"><span data-stu-id="814bc-147">Details</span></span> | <span data-ttu-id="814bc-148">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="814bc-148">Default</span></span> |
| --- | --- | --- |
| <span data-ttu-id="814bc-149">/q</span><span class="sxs-lookup"><span data-stu-id="814bc-149">/q</span></span> |<span data-ttu-id="814bc-150">Csendes telepítés</span><span class="sxs-lookup"><span data-stu-id="814bc-150">Quiet installation</span></span> |- |
| <span data-ttu-id="814bc-151">/ p: "hely"</span><span class="sxs-lookup"><span data-stu-id="814bc-151">/p:"location"</span></span> |<span data-ttu-id="814bc-152">Az Azure Backup szolgáltatás ügynökének a telepítési mappa elérési útja.</span><span class="sxs-lookup"><span data-stu-id="814bc-152">Path to the installation folder for the Azure Backup agent.</span></span> |<span data-ttu-id="814bc-153">C:\Program Files\Microsoft Azure Recovery Services Agent ügynök</span><span class="sxs-lookup"><span data-stu-id="814bc-153">C:\Program Files\Microsoft Azure Recovery Services Agent</span></span> |
| <span data-ttu-id="814bc-154">/ s: "hely"</span><span class="sxs-lookup"><span data-stu-id="814bc-154">/s:"location"</span></span> |<span data-ttu-id="814bc-155">Az Azure Backup szolgáltatás ügynökének a gyorsítótár mappájának elérési útja.</span><span class="sxs-lookup"><span data-stu-id="814bc-155">Path to the cache folder for the Azure Backup agent.</span></span> |<span data-ttu-id="814bc-156">C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch</span><span class="sxs-lookup"><span data-stu-id="814bc-156">C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch</span></span> |
| <span data-ttu-id="814bc-157">/m</span><span class="sxs-lookup"><span data-stu-id="814bc-157">/m</span></span> |<span data-ttu-id="814bc-158">Részvétel a Microsoft Update</span><span class="sxs-lookup"><span data-stu-id="814bc-158">Opt-in to Microsoft Update</span></span> |- |
| <span data-ttu-id="814bc-159">/Nu</span><span class="sxs-lookup"><span data-stu-id="814bc-159">/nu</span></span> |<span data-ttu-id="814bc-160">Ne keressen frissítéseket telepítésének befejezése után</span><span class="sxs-lookup"><span data-stu-id="814bc-160">Do not Check for updates after installation is complete</span></span> |- |
| <span data-ttu-id="814bc-161">/d</span><span class="sxs-lookup"><span data-stu-id="814bc-161">/d</span></span> |<span data-ttu-id="814bc-162">Eltávolítja a Microsoft Azure Recovery Services Agent ügynök</span><span class="sxs-lookup"><span data-stu-id="814bc-162">Uninstalls Microsoft Azure Recovery Services Agent</span></span> |- |
| <span data-ttu-id="814bc-163">/pH</span><span class="sxs-lookup"><span data-stu-id="814bc-163">/ph</span></span> |<span data-ttu-id="814bc-164">Állomás proxycím</span><span class="sxs-lookup"><span data-stu-id="814bc-164">Proxy Host Address</span></span> |- |
| <span data-ttu-id="814bc-165">/po</span><span class="sxs-lookup"><span data-stu-id="814bc-165">/po</span></span> |<span data-ttu-id="814bc-166">Proxy Host Port száma</span><span class="sxs-lookup"><span data-stu-id="814bc-166">Proxy Host Port Number</span></span> |- |
| <span data-ttu-id="814bc-167">/Pu</span><span class="sxs-lookup"><span data-stu-id="814bc-167">/pu</span></span> |<span data-ttu-id="814bc-168">Proxy-állomás felhasználónév</span><span class="sxs-lookup"><span data-stu-id="814bc-168">Proxy Host UserName</span></span> |- |
| <span data-ttu-id="814bc-169">/pW</span><span class="sxs-lookup"><span data-stu-id="814bc-169">/pw</span></span> |<span data-ttu-id="814bc-170">Proxy jelszava</span><span class="sxs-lookup"><span data-stu-id="814bc-170">Proxy Password</span></span> |- |

## <a name="registering-with-the-azure-backup-service"></a><span data-ttu-id="814bc-171">Az Azure Backup szolgáltatás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="814bc-171">Registering with the Azure Backup service</span></span>
<span data-ttu-id="814bc-172">Az Azure Backup szolgáltatással regisztrálhatja, mielőtt kell ahhoz, hogy a [Előfeltételek](backup-configure-vault.md) teljesülnek.</span><span class="sxs-lookup"><span data-stu-id="814bc-172">Before you can register with the Azure Backup service, you need to ensure that the [prerequisites](backup-configure-vault.md) are met.</span></span> <span data-ttu-id="814bc-173">A következők szükségesek:</span><span class="sxs-lookup"><span data-stu-id="814bc-173">You must:</span></span>

* <span data-ttu-id="814bc-174">Egy érvényes Azure-előfizetése</span><span class="sxs-lookup"><span data-stu-id="814bc-174">Have a valid Azure subscription</span></span>
* <span data-ttu-id="814bc-175">Rendelkezik egy biztonsági mentési tárolóval</span><span class="sxs-lookup"><span data-stu-id="814bc-175">Have a backup vault</span></span>

<span data-ttu-id="814bc-176">Töltse le a tárolói hitelesítő adatokat, futtassa a **Get-AzureRMBackupVaultCredentials** parancsmag egy Azure PowerShell-konzolban és a tároló azt egy tetszőleges helyen, például a * C:\Downloads\*.</span><span class="sxs-lookup"><span data-stu-id="814bc-176">To download the vault credentials, run the **Get-AzureRMBackupVaultCredentials** cmdlet in an Azure PowerShell console and store it in a convenient location like *C:\Downloads\*.</span></span>

```
PS C:\> $credspath = "C:\"
PS C:\> $credsfilename = Get-AzureRMBackupVaultCredentials -Vault $backupvault -TargetLocation $credspath
PS C:\> $credsfilename
f5303a0b-fae4-4cdb-b44d-0e4c032dde26_backuprg_backuprn_2015-08-11--06-22-35.VaultCredentials
```

<span data-ttu-id="814bc-177">A számítógép regisztrálása a tárolóban történik használatával a [Start-OBRegistration](https://technet.microsoft.com/library/hh770398%28v=wps.630%29.aspx) parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="814bc-177">Registering the machine with the vault is done using the [Start-OBRegistration](https://technet.microsoft.com/library/hh770398%28v=wps.630%29.aspx) cmdlet:</span></span>

```
PS C:\> $cred = $credspath + $credsfilename
PS C:\> Start-OBRegistration -VaultCredentials $cred -Confirm:$false

CertThumbprint      : 7a2ef2caa2e74b6ed1222a5e89288ddad438df2
SubscriptionID      : ef4ab577-c2c0-43e4-af80-af49f485f3d1
ServiceResourceName : test-vault
Region              : West US

Machine registration succeeded.
```

> [!IMPORTANT]
> <span data-ttu-id="814bc-178">Ne használjon relatív elérési utak adhatja meg a tároló hitelesítési adatait tartalmazó fájlt.</span><span class="sxs-lookup"><span data-stu-id="814bc-178">Do not use relative paths to specify the vault credentials file.</span></span> <span data-ttu-id="814bc-179">A parancsmag bemenetként abszolút elérési utat kell megadnia.</span><span class="sxs-lookup"><span data-stu-id="814bc-179">You must provide an absolute path as an input to the cmdlet.</span></span>
>
>

## <a name="networking-settings"></a><span data-ttu-id="814bc-180">Hálózati beállítások</span><span class="sxs-lookup"><span data-stu-id="814bc-180">Networking settings</span></span>
<span data-ttu-id="814bc-181">Ha a Windows számítógép az internethez csatlakoztatásához proxykiszolgálón keresztül, a proxykiszolgáló beállításait is megadható az ügynököt.</span><span class="sxs-lookup"><span data-stu-id="814bc-181">When the connectivity of the Windows machine to the internet is through a proxy server, the proxy settings can also be provided to the agent.</span></span> <span data-ttu-id="814bc-182">Ebben a példában nincs nincs proxykiszolgáló, explicit módon azt vannak törlésével bármely proxy kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="814bc-182">In this example, there is no proxy server, so we are explicitly clearing any proxy-related information.</span></span>

<span data-ttu-id="814bc-183">Sávszélesség is szabályozható lehetőségét ```work hour bandwidth``` és ```non-work hour bandwidth``` napokat a hét adott számú.</span><span class="sxs-lookup"><span data-stu-id="814bc-183">Bandwidth usage can also be controlled with the options of ```work hour bandwidth``` and ```non-work hour bandwidth``` for a given set of days of the week.</span></span>

<span data-ttu-id="814bc-184">Beállítás a proxy- és sávszélesség részletei történik használatával a [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409%28v=wps.630%29.aspx) parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="814bc-184">Setting the proxy and bandwidth details is done using the [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409%28v=wps.630%29.aspx) cmdlet:</span></span>

```
PS C:\> Set-OBMachineSetting -NoProxy
Server properties updated successfully.

PS C:\> Set-OBMachineSetting -NoThrottle
Server properties updated successfully.
```

## <a name="encryption-settings"></a><span data-ttu-id="814bc-185">Titkosítási beállítások</span><span class="sxs-lookup"><span data-stu-id="814bc-185">Encryption settings</span></span>
<span data-ttu-id="814bc-186">A biztonsági mentési Azure biztonsági mentési küldött adatok védelmét az adatok bizalmas mivoltát titkosított.</span><span class="sxs-lookup"><span data-stu-id="814bc-186">The backup data sent to Azure Backup is encrypted to protect the confidentiality of the data.</span></span> <span data-ttu-id="814bc-187">A titkosítás jelszava a "password" vissza kell fejtenie az adatokat a visszaállítás időpontjában.</span><span class="sxs-lookup"><span data-stu-id="814bc-187">The encryption passphrase is the "password" to decrypt the data at the time of restore.</span></span>

```
PS C:\> ConvertTo-SecureString -String "Complex!123_STRING" -AsPlainText -Force | Set-OBMachineSetting
Server properties updated successfully
```

> [!IMPORTANT]
> <span data-ttu-id="814bc-188">Jelszó információinak megtartása biztonságos be van állítva.</span><span class="sxs-lookup"><span data-stu-id="814bc-188">Keep the passphrase information safe and secure once it is set.</span></span> <span data-ttu-id="814bc-189">Nem lehet visszaállítani az adatokat az Azure-ból nélkül ezt a jelszót.</span><span class="sxs-lookup"><span data-stu-id="814bc-189">You will not be able to restore data from Azure without this passphrase.</span></span>
>
>

## <a name="back-up-files-and-folders"></a><span data-ttu-id="814bc-190">Fájlok és mappák biztonsági mentése</span><span class="sxs-lookup"><span data-stu-id="814bc-190">Back up files and folders</span></span>
<span data-ttu-id="814bc-191">Összes a biztonsági mentés a Windows-kiszolgálók és ügyfelek az Azure Backup szolgáltatásba a házirend vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="814bc-191">All your backups from Windows Servers and clients to Azure Backup are governed by a policy.</span></span> <span data-ttu-id="814bc-192">A házirend három részből áll:</span><span class="sxs-lookup"><span data-stu-id="814bc-192">The policy comprises three parts:</span></span>

1. <span data-ttu-id="814bc-193">A **biztonsági mentés ütemezése** , amely megadja, amikor biztonsági mentést kell venni és szinkronizálása a szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="814bc-193">A **backup schedule** that specifies when backups need to be taken and synchronized with the service.</span></span>
2. <span data-ttu-id="814bc-194">A **adatmegőrzési ütemterv** , amely megadja, mennyi ideig szeretné megőrizni a helyreállítási pontok az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="814bc-194">A **retention schedule** that specifies how long to retain the recovery points in Azure.</span></span>
3. <span data-ttu-id="814bc-195">A **belefoglalási/kizárási megadására** , amely határozzák meg, hogy mi készüljön biztonsági másolat.</span><span class="sxs-lookup"><span data-stu-id="814bc-195">A **file inclusion/exclusion specification** that dictates what should be backed up.</span></span>

<span data-ttu-id="814bc-196">Ebben a dokumentumban mivel azt még automatizálása a biztonsági mentés, feltételezzük, semmi nem konfiguráltak.</span><span class="sxs-lookup"><span data-stu-id="814bc-196">In this document, since we're automating backup, we'll assume nothing has been configured.</span></span> <span data-ttu-id="814bc-197">Hozzon létre egy új biztonsági mentési házirend használatával megkezdjük a [New-OBPolicy](https://technet.microsoft.com/library/hh770416.aspx) parancsmag és használja azt.</span><span class="sxs-lookup"><span data-stu-id="814bc-197">We begin by creating a new backup policy using the [New-OBPolicy](https://technet.microsoft.com/library/hh770416.aspx) cmdlet and using it.</span></span>

```
PS C:\> $newpolicy = New-OBPolicy
```

<span data-ttu-id="814bc-198">Jelenleg a házirend üres, és más parancsmagok olyan szükséges meghatározásához, hogy mely elemeket kell befoglalt, sem a kizárt, amikor biztonsági mentést fog futni, és egyes esetekben a biztonsági mentések tárolódik.</span><span class="sxs-lookup"><span data-stu-id="814bc-198">At this time the policy is empty and other cmdlets are needed to define what items will be included or excluded, when backups will run, and where the backups will be stored.</span></span>

### <a name="configuring-the-backup-schedule"></a><span data-ttu-id="814bc-199">A biztonsági mentési ütemezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="814bc-199">Configuring the backup schedule</span></span>
<span data-ttu-id="814bc-200">Az első 3 részből álló házirend a biztonsági mentési ütemezést, és létre kell hozni a [New-OBSchedule](https://technet.microsoft.com/library/hh770401) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="814bc-200">The first of the 3 parts of a policy is the backup schedule, which is created using the [New-OBSchedule](https://technet.microsoft.com/library/hh770401) cmdlet.</span></span> <span data-ttu-id="814bc-201">A biztonsági mentés ütemezése határozza meg, amikor biztonsági mentést kell tenni.</span><span class="sxs-lookup"><span data-stu-id="814bc-201">The backup schedule defines when backups need to be taken.</span></span> <span data-ttu-id="814bc-202">Ütemezés létrehozásakor meg kell adnia 2 bemeneti paraméterek:</span><span class="sxs-lookup"><span data-stu-id="814bc-202">When creating a schedule you need to specify 2 input parameters:</span></span>

* <span data-ttu-id="814bc-203">**A hét napjai** , amely a biztonsági mentést kell futtatnia.</span><span class="sxs-lookup"><span data-stu-id="814bc-203">**Days of the week** that the backup should run.</span></span> <span data-ttu-id="814bc-204">A biztonsági mentési feladat csak egy napon, vagy a hét minden napján, vagy tetszőleges kombinációját a közötti futtatása.</span><span class="sxs-lookup"><span data-stu-id="814bc-204">You can run the backup job on just one day, or every day of the week, or any combination in between.</span></span>
* <span data-ttu-id="814bc-205">**A nap** mikor fusson a biztonsági mentés.</span><span class="sxs-lookup"><span data-stu-id="814bc-205">**Times of the day** when the backup should run.</span></span> <span data-ttu-id="814bc-206">Ha a biztonsági mentés akkor is kiváltódik 3 különböző napszakokban adhat meg.</span><span class="sxs-lookup"><span data-stu-id="814bc-206">You can define up to 3 different times of the day when the backup will be triggered.</span></span>

<span data-ttu-id="814bc-207">Például konfigurálhatja a biztonsági mentési házirend du. 4: futtat minden szombat és vasárnap.</span><span class="sxs-lookup"><span data-stu-id="814bc-207">For instance, you could configure a backup policy that runs at 4PM every Saturday and Sunday.</span></span>

```
PS C:\> $sched = New-OBSchedule -DaysofWeek Saturday, Sunday -TimesofDay 16:00
```

<span data-ttu-id="814bc-208">A biztonsági mentés ütemezése kell lennie a házirendhez társított, és ez használatával érhető el a [Set-OBSchedule](https://technet.microsoft.com/library/hh770407) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="814bc-208">The backup schedule needs to be associated with a policy, and this can be achieved by using the [Set-OBSchedule](https://technet.microsoft.com/library/hh770407) cmdlet.</span></span>

```
PS C:> Set-OBSchedule -Policy $newpolicy -Schedule $sched
BackupSchedule : 4:00 PM Saturday, Sunday, Every 1 week(s) DsList : PolicyName : RetentionPolicy : State : New PolicyState : Valid
```
### <a name="configuring-a-retention-policy"></a><span data-ttu-id="814bc-209">Egy megőrzési házirend konfigurálása</span><span class="sxs-lookup"><span data-stu-id="814bc-209">Configuring a retention policy</span></span>
<span data-ttu-id="814bc-210">A megőrzési házirend határozza meg, mennyi ideig megőrzi a biztonsági mentési feladatok létrehozott helyreállítási pontok.</span><span class="sxs-lookup"><span data-stu-id="814bc-210">The retention policy defines how long recovery points created from backup jobs are retained.</span></span> <span data-ttu-id="814bc-211">Egy új megőrzési házirend használatával létrehozásakor a [New-OBRetentionPolicy](https://technet.microsoft.com/library/hh770425) parancsmaggal, megadhatja az, hogy hány napig őrzi meg az Azure biztonsági mentési kell a biztonsági mentések helyreállítási pontjait.</span><span class="sxs-lookup"><span data-stu-id="814bc-211">When creating a new retention policy using the [New-OBRetentionPolicy](https://technet.microsoft.com/library/hh770425) cmdlet, you can specify the number of days that the backup recovery points need to be retained with Azure Backup.</span></span> <span data-ttu-id="814bc-212">Az alábbi példában 7 napos adatmegőrzési állítja be.</span><span class="sxs-lookup"><span data-stu-id="814bc-212">The example below sets a retention policy of 7 days.</span></span>

```
PS C:\> $retentionpolicy = New-OBRetentionPolicy -RetentionDays 7
```

<span data-ttu-id="814bc-213">Az adatmegőrzési hozzá kell rendelni a fő házirend-parancsmag segítségével [Set-OBRetentionPolicy](https://technet.microsoft.com/library/hh770405):</span><span class="sxs-lookup"><span data-stu-id="814bc-213">The retention policy must be associated with the main policy using the cmdlet [Set-OBRetentionPolicy](https://technet.microsoft.com/library/hh770405):</span></span>

```
PS C:\> Set-OBRetentionPolicy -Policy $newpolicy -RetentionPolicy $retentionpolicy

BackupSchedule  : 4:00 PM
                  Saturday, Sunday,
                  Every 1 week(s)
DsList          :
PolicyName      :
RetentionPolicy : Retention Days : 7

                  WeeklyLTRSchedule :
                  Weekly schedule is not set

                  MonthlyLTRSchedule :
                  Monthly schedule is not set

                  YearlyLTRSchedule :
                  Yearly schedule is not set

State           : New
PolicyState     : Valid
```
### <a name="including-and-excluding-files-to-be-backed-up"></a><span data-ttu-id="814bc-214">Többek között, és a fájlok biztonsági mentése nélkül</span><span class="sxs-lookup"><span data-stu-id="814bc-214">Including and excluding files to be backed up</span></span>
<span data-ttu-id="814bc-215">Egy ```OBFileSpec``` objektum meghatározása és a biztonsági másolat nem lehet a fájlokat.</span><span class="sxs-lookup"><span data-stu-id="814bc-215">An ```OBFileSpec``` object defines the files to be included and excluded in a backup.</span></span> <span data-ttu-id="814bc-216">Ez a szabálykészlet, amely a védett fájlok és mappák gépen kimenő hatókör.</span><span class="sxs-lookup"><span data-stu-id="814bc-216">This is a set of rules that scope out the protected files and folders on a machine.</span></span> <span data-ttu-id="814bc-217">Akkor is, számos befoglalási vagy kizárási szabály szükség szerint fájlt, és rendelje hozzá őket egy házirendet.</span><span class="sxs-lookup"><span data-stu-id="814bc-217">You can have as many file inclusion or exclusion rules as required, and associate them with a policy.</span></span> <span data-ttu-id="814bc-218">Új OBFileSpec objektum létrehozásakor meg a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="814bc-218">When creating a new OBFileSpec object, you can:</span></span>

* <span data-ttu-id="814bc-219">Adja meg a fájlok és mappák része</span><span class="sxs-lookup"><span data-stu-id="814bc-219">Specify the files and folders to be included</span></span>
* <span data-ttu-id="814bc-220">Adja meg a fájlok és mappák ki lesznek zárva</span><span class="sxs-lookup"><span data-stu-id="814bc-220">Specify the files and folders to be excluded</span></span>
* <span data-ttu-id="814bc-221">Adja meg a rekurzív az adatok biztonsági mentése egy mappában (vagy) e a megadott mappában csak a legfelső szintű fájlok biztonsági másolatot kell fel.</span><span class="sxs-lookup"><span data-stu-id="814bc-221">Specify recursive backup of data in a folder (or) whether only the top-level files in the specified folder should be backed up.</span></span>

<span data-ttu-id="814bc-222">Ez utóbbi használatával érhető el a nem rekurzív - jelzőt a New-OBFileSpec parancsban.</span><span class="sxs-lookup"><span data-stu-id="814bc-222">The latter is achieved by using the -NonRecursive flag in the New-OBFileSpec command.</span></span>

<span data-ttu-id="814bc-223">Az alábbi példa azt fogja biztonsági mentése kötet C: és D: és a az operációs rendszer bináris ideiglenes mappákat a Windows mappában található fájlok kizárása.</span><span class="sxs-lookup"><span data-stu-id="814bc-223">In the example below, we'll back up volume C: and D: and exclude the OS binaries in the Windows folder and any temporary folders.</span></span> <span data-ttu-id="814bc-224">Ennek érdekében hozzon létre két fájl használatával specifikációk a [New-OBFileSpec](https://technet.microsoft.com/library/hh770408) parancsmag - befoglalási és kizárási egyet.</span><span class="sxs-lookup"><span data-stu-id="814bc-224">To do so we'll create two file specifications using the [New-OBFileSpec](https://technet.microsoft.com/library/hh770408) cmdlet - one for inclusion and one for exclusion.</span></span> <span data-ttu-id="814bc-225">A fájl specifikációk létrehozása után, fontosságúak a szabályzat használatához a [Add-OBFileSpec](https://technet.microsoft.com/library/hh770424) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="814bc-225">Once the file specifications have been created, they're associated with the policy using the [Add-OBFileSpec](https://technet.microsoft.com/library/hh770424) cmdlet.</span></span>

```
PS C:\> $inclusions = New-OBFileSpec -FileSpec @("C:\", "D:\")

PS C:\> $exclusions = New-OBFileSpec -FileSpec @("C:\windows", "C:\temp") -Exclude

PS C:\> Add-OBFileSpec -Policy $newpolicy -FileSpec $inclusions

BackupSchedule  : 4:00 PM
                  Saturday, Sunday,
                  Every 1 week(s)
DsList          : {DataSource
                  DatasourceId:0
                  Name:C:\
                  FileSpec:FileSpec
                  FileSpec:C:\
                  IsExclude:False
                  IsRecursive:True

                  , DataSource
                  DatasourceId:0
                  Name:D:\
                  FileSpec:FileSpec
                  FileSpec:D:\
                  IsExclude:False
                  IsRecursive:True

                  }
PolicyName      :
RetentionPolicy : Retention Days : 7

                  WeeklyLTRSchedule :
                  Weekly schedule is not set

                  MonthlyLTRSchedule :
                  Monthly schedule is not set

                  YearlyLTRSchedule :
                  Yearly schedule is not set

State           : New
PolicyState     : Valid


PS C:\> Add-OBFileSpec -Policy $newpolicy -FileSpec $exclusions

BackupSchedule  : 4:00 PM
                  Saturday, Sunday,
                  Every 1 week(s)
DsList          : {DataSource
                  DatasourceId:0
                  Name:C:\
                  FileSpec:FileSpec
                  FileSpec:C:\
                  IsExclude:False
                  IsRecursive:True
                  ,FileSpec
                  FileSpec:C:\windows
                  IsExclude:True
                  IsRecursive:True
                  ,FileSpec
                  FileSpec:C:\temp
                  IsExclude:True
                  IsRecursive:True

                  , DataSource
                  DatasourceId:0
                  Name:D:\
                  FileSpec:FileSpec
                  FileSpec:D:\
                  IsExclude:False
                  IsRecursive:True

                  }
PolicyName      :
RetentionPolicy : Retention Days : 7

                  WeeklyLTRSchedule :
                  Weekly schedule is not set

                  MonthlyLTRSchedule :
                  Monthly schedule is not set

                  YearlyLTRSchedule :
                  Yearly schedule is not set

State           : New
PolicyState     : Valid
```

### <a name="applying-the-policy"></a><span data-ttu-id="814bc-226">A házirend alkalmazása</span><span class="sxs-lookup"><span data-stu-id="814bc-226">Applying the policy</span></span>
<span data-ttu-id="814bc-227">A csoportházirend-objektum most már befejeződött, és egy társított biztonsági mentés ütemezése, a megőrzési házirend és a fájlok belefoglalási/kizárási lista.</span><span class="sxs-lookup"><span data-stu-id="814bc-227">Now the policy object is complete and has an associated backup schedule, retention policy, and an inclusion/exclusion list of files.</span></span> <span data-ttu-id="814bc-228">Ez a házirend most lehet véglegesíteni az Azure Backup használatához.</span><span class="sxs-lookup"><span data-stu-id="814bc-228">This policy can now be committed for Azure Backup to use.</span></span> <span data-ttu-id="814bc-229">Alkalmazása előtt az újonnan létrehozott házirend gondoskodjon arról, hogy nincsenek-e a kiszolgálóhoz társított használatával meglévő biztonsági mentési házirendek a [Remove-OBPolicy](https://technet.microsoft.com/library/hh770415) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="814bc-229">Before you apply the newly created policy ensure that there are no existing backup policies associated with the server by using the [Remove-OBPolicy](https://technet.microsoft.com/library/hh770415) cmdlet.</span></span> <span data-ttu-id="814bc-230">A házirend eltávolítása megerősítő fogja kérni.</span><span class="sxs-lookup"><span data-stu-id="814bc-230">Removing the policy will prompt for confirmation.</span></span> <span data-ttu-id="814bc-231">Kihagyja a megerősítő használja a ```-Confirm:$false``` jelzőt mellékel a parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="814bc-231">To skip the confirmation use the ```-Confirm:$false``` flag with the cmdlet.</span></span>

```
PS C:> Get-OBPolicy | Remove-OBPolicy
Microsoft Azure Backup Are you sure you want to remove this backup policy? This will delete all the backed up data. [Y] Yes [A] Yes to All [N] No [L] No to All [S] Suspend [?] Help (default is "Y"):
```

<span data-ttu-id="814bc-232">A csoportházirend-objektum életbe léptetése történik használatával a [Set-OBPolicy](https://technet.microsoft.com/library/hh770421) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="814bc-232">Committing the policy object is done using the [Set-OBPolicy](https://technet.microsoft.com/library/hh770421) cmdlet.</span></span> <span data-ttu-id="814bc-233">Ez is megerősítést kér.</span><span class="sxs-lookup"><span data-stu-id="814bc-233">This will also ask for confirmation.</span></span> <span data-ttu-id="814bc-234">Kihagyja a megerősítő használja a ```-Confirm:$false``` jelzőt mellékel a parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="814bc-234">To skip the confirmation use the ```-Confirm:$false``` flag with the cmdlet.</span></span>

```
PS C:> Set-OBPolicy -Policy $newpolicy
Microsoft Azure Backup Do you want to save this backup policy ? [Y] Yes [A] Yes to All [N] No [L] No to All [S] Suspend [?] Help (default is "Y"):
BackupSchedule : 4:00 PM Saturday, Sunday, Every 1 week(s)
DsList : {DataSource
         DatasourceId:4508156004108672185
         Name:C:\
         FileSpec:FileSpec
         FileSpec:C:\
         IsExclude:False
         IsRecursive:True,

         FileSpec
         FileSpec:C:\windows
         IsExclude:True
         IsRecursive:True,

         FileSpec
         FileSpec:C:\temp
         IsExclude:True
         IsRecursive:True,

         DataSource
         DatasourceId:4508156005178868542
         Name:D:\
         FileSpec:FileSpec
         FileSpec:D:\
         IsExclude:False
         IsRecursive:True
    }
PolicyName : c2eb6568-8a06-49f4-a20e-3019ae411bac
RetentionPolicy : Retention Days : 7
              WeeklyLTRSchedule :
              Weekly schedule is not set

              MonthlyLTRSchedule :
              Monthly schedule is not set

              YearlyLTRSchedule :
              Yearly schedule is not set
State : Existing PolicyState : Valid
```

<span data-ttu-id="814bc-235">A részleteket a meglévő biztonsági mentési házirend használatával megtekintheti a [Get-OBPolicy](https://technet.microsoft.com/library/hh770406) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="814bc-235">You can view the details of the existing backup policy using the [Get-OBPolicy](https://technet.microsoft.com/library/hh770406) cmdlet.</span></span> <span data-ttu-id="814bc-236">Akkor is leásási használatával a [Get-OBSchedule](https://technet.microsoft.com/library/hh770423) parancsmag a biztonsági mentési ütemezés és a [Get-OBRetentionPolicy](https://technet.microsoft.com/library/hh770427) parancsmag az adatmegőrzési házirendek</span><span class="sxs-lookup"><span data-stu-id="814bc-236">You can drill-down further using the [Get-OBSchedule](https://technet.microsoft.com/library/hh770423) cmdlet for the backup schedule and the [Get-OBRetentionPolicy](https://technet.microsoft.com/library/hh770427) cmdlet for the retention policies</span></span>

```
PS C:> Get-OBPolicy | Get-OBSchedule
SchedulePolicyName : 71944081-9950-4f7e-841d-32f0a0a1359a
ScheduleRunDays : {Saturday, Sunday}
ScheduleRunTimes : {16:00:00}
State : Existing

PS C:> Get-OBPolicy | Get-OBRetentionPolicy
RetentionDays : 7
RetentionPolicyName : ca3574ec-8331-46fd-a605-c01743a5265e
State : Existing

PS C:> Get-OBPolicy | Get-OBFileSpec
FileName : *
FilePath : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\
FileSpec : D:\
IsExclude : False
IsRecursive : True

FileName : *
FilePath : \?\Volume{cdd41007-a22f-11e2-be6c-806e6f6e6963}\
FileSpec : C:\
IsExclude : False
IsRecursive : True

FileName : *
FilePath : \?\Volume{cdd41007-a22f-11e2-be6c-806e6f6e6963}\windows
FileSpec : C:\windows
IsExclude : True
IsRecursive : True

FileName : *
FilePath : \?\Volume{cdd41007-a22f-11e2-be6c-806e6f6e6963}\temp
FileSpec : C:\temp
IsExclude : True
IsRecursive : True
```

### <a name="performing-an-ad-hoc-backup"></a><span data-ttu-id="814bc-237">Az ad hoc biztonsági mentés</span><span class="sxs-lookup"><span data-stu-id="814bc-237">Performing an ad-hoc backup</span></span>
<span data-ttu-id="814bc-238">Miután a biztonsági mentési házirend van beállítva a biztonsági mentések száma az ütemezésben fog létrejönni.</span><span class="sxs-lookup"><span data-stu-id="814bc-238">Once a backup policy has been set the backups will occur per the schedule.</span></span> <span data-ttu-id="814bc-239">Az ad hoc biztonsági mentés indítására az is lehetséges használatával a [Start-OBBackup](https://technet.microsoft.com/library/hh770426) parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="814bc-239">Triggering an ad-hoc backup is also possible using the [Start-OBBackup](https://technet.microsoft.com/library/hh770426) cmdlet:</span></span>

```
PS C:> Get-OBPolicy | Start-OBBackup
Taking snapshot of volumes...
Preparing storage...
Estimating size of backup items...
Estimating size of backup items...
Transferring data...
Verifying backup...
Job completed.
The backup operation completed successfully.
```

## <a name="restore-data-from-azure-backup"></a><span data-ttu-id="814bc-240">Állítsa vissza az adatokat az Azure Backup</span><span class="sxs-lookup"><span data-stu-id="814bc-240">Restore data from Azure Backup</span></span>
<span data-ttu-id="814bc-241">Ez a szakasz végigvezeti az Azure biztonsági mentés az adatok helyreállítás automatizálása lépéseit.</span><span class="sxs-lookup"><span data-stu-id="814bc-241">This section will guide you through the steps for automating recovery of data from Azure Backup.</span></span> <span data-ttu-id="814bc-242">Ennek során a következő lépésekből áll:</span><span class="sxs-lookup"><span data-stu-id="814bc-242">Doing so involves the following steps:</span></span>

1. <span data-ttu-id="814bc-243">Válassza ki a forráskötet</span><span class="sxs-lookup"><span data-stu-id="814bc-243">Pick the source volume</span></span>
2. <span data-ttu-id="814bc-244">Válassza ki a visszaállítandó biztonsági mentési pontok</span><span class="sxs-lookup"><span data-stu-id="814bc-244">Choose a backup point to restore</span></span>
3. <span data-ttu-id="814bc-245">Válasszon ki egy elemet visszaállítása</span><span class="sxs-lookup"><span data-stu-id="814bc-245">Choose an item to restore</span></span>
4. <span data-ttu-id="814bc-246">A visszaállítási folyamat eseményindító</span><span class="sxs-lookup"><span data-stu-id="814bc-246">Trigger the restore process</span></span>

### <a name="picking-the-source-volume"></a><span data-ttu-id="814bc-247">A forráskötet válassza háttérszínnek.</span><span class="sxs-lookup"><span data-stu-id="814bc-247">Picking the source volume</span></span>
<span data-ttu-id="814bc-248">Ahhoz, hogy az Azure Backup elem visszaállításához először kell azonosítani a cikk forrását.</span><span class="sxs-lookup"><span data-stu-id="814bc-248">In order to restore an item from Azure Backup, you first need to identify the source of the item.</span></span> <span data-ttu-id="814bc-249">Azt éppen végrehajtás alatt a parancsokat egy Windows Server vagy egy Windows ügyfél környezetben, mert a gép már azonosítja.</span><span class="sxs-lookup"><span data-stu-id="814bc-249">Since we're executing the commands in the context of a Windows Server or a Windows client, the machine is already identified.</span></span> <span data-ttu-id="814bc-250">A következő lépése a forrás azonosítása, hogy az azt tartalmazó kötet azonosításához.</span><span class="sxs-lookup"><span data-stu-id="814bc-250">The next step in identifying the source is to identify the volume containing it.</span></span> <span data-ttu-id="814bc-251">Erről a gépről biztonsági mentés alatt lekérhetők végrehajtása adatforrásokat, illetve kötetek listáját a [Get-OBRecoverableSource](https://technet.microsoft.com/library/hh770410) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="814bc-251">A list of volumes or sources being backed up from this machine can be retrieved by executing the [Get-OBRecoverableSource](https://technet.microsoft.com/library/hh770410) cmdlet.</span></span> <span data-ttu-id="814bc-252">Ez a parancs minden, a biztonsági másolatot készített a kiszolgáló ügyfél források tömbjét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="814bc-252">This command returns an array of all the sources backed up from this server/client.</span></span>

```
PS C:> $source = Get-OBRecoverableSource
PS C:> $source
FriendlyName : C:\
RecoverySourceName : C:\
ServerName : myserver.microsoft.com

FriendlyName : D:\
RecoverySourceName : D:\
ServerName : myserver.microsoft.com
```

### <a name="choosing-a-backup-point-to-restore"></a><span data-ttu-id="814bc-253">A visszaállítandó biztonsági mentési pont kiválasztása</span><span class="sxs-lookup"><span data-stu-id="814bc-253">Choosing a backup point to restore</span></span>
<span data-ttu-id="814bc-254">A biztonsági mentési pontok listáját a következő futtatásával lehet beolvasni a [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) parancsmag megfelelő paraméterekkel.</span><span class="sxs-lookup"><span data-stu-id="814bc-254">The list of backup points can be retrieved by executing the [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) cmdlet with appropriate parameters.</span></span> <span data-ttu-id="814bc-255">A jelen példában mutatjuk be a legújabb biztonsági mentési pont a forráskötet *D:* , és visszaállít egy adott fájlt.</span><span class="sxs-lookup"><span data-stu-id="814bc-255">In our example, we’ll choose the latest backup point for the source volume *D:* and use it to recover a specific file.</span></span>

```
PS C:> $rps = Get-OBRecoverableItem -Source $source[1]
IsDir : False
ItemNameFriendly : D:\
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\
LocalMountPoint : D:\
MountPointName : D:\
Name : D:\
PointInTime : 18-Jun-15 6:41:52 AM
ServerName : myserver.microsoft.com
ItemSize :
ItemLastModifiedTime :

IsDir : False
ItemNameFriendly : D:\
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\
LocalMountPoint : D:\
MountPointName : D:\
Name : D:\
PointInTime : 17-Jun-15 6:31:31 AM
ServerName : myserver.microsoft.com
ItemSize :
ItemLastModifiedTime :
```
<span data-ttu-id="814bc-256">Az objektum ```$rps``` biztonsági mentési pontok tömbje.</span><span class="sxs-lookup"><span data-stu-id="814bc-256">The object ```$rps``` is an array of backup points.</span></span> <span data-ttu-id="814bc-257">Az első elem a legutóbbi pontnak, az n-edik elemének pedig a legrégebbi pont.</span><span class="sxs-lookup"><span data-stu-id="814bc-257">The first element is the latest point and the Nth element is the oldest point.</span></span> <span data-ttu-id="814bc-258">Válassza ki a legutóbbi pontnak, hogy használjuk ```$rps[0]```.</span><span class="sxs-lookup"><span data-stu-id="814bc-258">To choose the latest point, we will use ```$rps[0]```.</span></span>

### <a name="choosing-an-item-to-restore"></a><span data-ttu-id="814bc-259">Visszaállítás elem kiválasztása</span><span class="sxs-lookup"><span data-stu-id="814bc-259">Choosing an item to restore</span></span>
<span data-ttu-id="814bc-260">A pontos fájl vagy mappa visszaállításához rekurzív módon használja azonosítására a [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="814bc-260">To identify the exact file or folder to restore, recursively use the [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) cmdlet.</span></span> <span data-ttu-id="814bc-261">A mappahierarchia kizárólag használatával tallózható így a ```Get-OBRecoverableItem```.</span><span class="sxs-lookup"><span data-stu-id="814bc-261">That way the folder hierarchy can be browsed solely using the ```Get-OBRecoverableItem```.</span></span>

<span data-ttu-id="814bc-262">Ebben a példában, ha azt szeretné, hogy állítsa vissza a fájlt *finances.xls* azt hivatkozhat, amely az objektum ```$filesFolders[1]```.</span><span class="sxs-lookup"><span data-stu-id="814bc-262">In this example, if we want to restore the file *finances.xls* we can reference that using the object ```$filesFolders[1]```.</span></span>

```
PS C:> $filesFolders = Get-OBRecoverableItem $rps[0]
PS C:> $filesFolders
IsDir : True
ItemNameFriendly : D:\MyData\
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\MyData\
LocalMountPoint : D:\
MountPointName : D:\
Name : MyData
PointInTime : 18-Jun-15 6:41:52 AM
ServerName : myserver.microsoft.com
ItemSize :
ItemLastModifiedTime : 15-Jun-15 8:49:29 AM

PS C:> $filesFolders = Get-OBRecoverableItem $filesFolders[0]
PS C:> $filesFolders
IsDir : False
ItemNameFriendly : D:\MyData\screenshot.oxps
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\MyData\screenshot.oxps
LocalMountPoint : D:\
MountPointName : D:\
Name : screenshot.oxps
PointInTime : 18-Jun-15 6:41:52 AM
ServerName : myserver.microsoft.com
ItemSize : 228313
ItemLastModifiedTime : 21-Jun-14 6:45:09 AM

IsDir : False
ItemNameFriendly : D:\MyData\finances.xls
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\MyData\finances.xls
LocalMountPoint : D:\
MountPointName : D:\
Name : finances.xls
PointInTime : 18-Jun-15 6:41:52 AM
ServerName : myserver.microsoft.com
ItemSize : 96256
ItemLastModifiedTime : 21-Jun-14 6:43:02 AM
```

<span data-ttu-id="814bc-263">Az elemek visszaállítás segítségével is kereshet a ```Get-OBRecoverableItem``` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="814bc-263">You can also search for items to restore using the ```Get-OBRecoverableItem``` cmdlet.</span></span> <span data-ttu-id="814bc-264">A jelen példában keressen rá a *finances.xls* azt sikerült leírót a fájl a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="814bc-264">In our example, to search for *finances.xls* we could get a handle on the file by running this command:</span></span>

```
PS C:\> $item = Get-OBRecoverableItem -RecoveryPoint $rps[0] -Location "D:\MyData" -SearchString "finance*"
```

### <a name="triggering-the-restore-process"></a><span data-ttu-id="814bc-265">A visszaállítási folyamat időt.</span><span class="sxs-lookup"><span data-stu-id="814bc-265">Triggering the restore process</span></span>
<span data-ttu-id="814bc-266">A visszaállítási folyamat indításához először kell adja meg a helyreállítási beállításokat.</span><span class="sxs-lookup"><span data-stu-id="814bc-266">To trigger the restore process, we first need to specify the recovery options.</span></span> <span data-ttu-id="814bc-267">Ezt megteheti a a [New-OBRecoveryOption](https://technet.microsoft.com/library/hh770417.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="814bc-267">This can be done by using the [New-OBRecoveryOption](https://technet.microsoft.com/library/hh770417.aspx) cmdlet.</span></span> <span data-ttu-id="814bc-268">Ehhez a példához tegyük fel, hogy a fájlok visszaállítására szeretnénk *C:\temp*.</span><span class="sxs-lookup"><span data-stu-id="814bc-268">For this example, let's assume that we want to restore the files to *C:\temp*.</span></span> <span data-ttu-id="814bc-269">Tegyük is fel, hogy szeretnénk már megtalálható fájlok kihagyása a rendeltetési mappára *C:\temp*.</span><span class="sxs-lookup"><span data-stu-id="814bc-269">Let's also assume that we want to skip files that already exist on the destination folder *C:\temp*.</span></span> <span data-ttu-id="814bc-270">Hozzon létre egy helyreállítási lehetőség, használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="814bc-270">To create such a recovery option, use the following command:</span></span>

```
PS C:\> $recovery_option = New-OBRecoveryOption -DestinationPath "C:\temp" -OverwriteType Skip
```

<span data-ttu-id="814bc-271">Most már a visszaállítás elindítása használatával a [Start-OBRecovery](https://technet.microsoft.com/library/hh770402.aspx) parancs a kiválasztott ```$item``` kimenetében a ```Get-OBRecoverableItem``` parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="814bc-271">Now trigger restore by using the [Start-OBRecovery](https://technet.microsoft.com/library/hh770402.aspx) command on the selected ```$item``` from the output of the ```Get-OBRecoverableItem``` cmdlet:</span></span>

```
PS C:\> Start-OBRecovery -RecoverableItem $item -RecoveryOption $recover_option
Estimating size of backup items...
Estimating size of backup items...
Estimating size of backup items...
Estimating size of backup items...
Job completed.
The recovery operation completed successfully.
```


## <a name="uninstalling-the-azure-backup-agent"></a><span data-ttu-id="814bc-272">Az Azure Backup szolgáltatás ügynökének eltávolítása</span><span class="sxs-lookup"><span data-stu-id="814bc-272">Uninstalling the Azure Backup agent</span></span>
<span data-ttu-id="814bc-273">Az Azure Backup-ügynök eltávolítása végezhető a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="814bc-273">Uninstalling the Azure Backup agent can be done by using the following command:</span></span>

```
PS C:\> .\MARSAgentInstaller.exe /d /q
```

<span data-ttu-id="814bc-274">Az ügynök bináris fájlok eltávolítása a számítógépről következménnyel néhány kell figyelembe venni:</span><span class="sxs-lookup"><span data-stu-id="814bc-274">Uninstalling the agent binaries from the machine has some consequences to consider:</span></span>

* <span data-ttu-id="814bc-275">A fájl-szűrő eltávolítja a gépet, és a változások követését le van állítva.</span><span class="sxs-lookup"><span data-stu-id="814bc-275">It removes the file-filter from the machine, and tracking of changes is stopped.</span></span>
* <span data-ttu-id="814bc-276">A gép eltávolítja az összes házirend az adatokat, de a házirenddel kapcsolatos információk továbbra is a szolgáltatásban kell tárolni.</span><span class="sxs-lookup"><span data-stu-id="814bc-276">All policy information is removed from the machine, but the policy information continues to be stored in the service.</span></span>
* <span data-ttu-id="814bc-277">A mentési ütemezések törlődnek, és nincs további biztonsági mentés készül.</span><span class="sxs-lookup"><span data-stu-id="814bc-277">All backup schedules are removed, and no further backups are taken.</span></span>

<span data-ttu-id="814bc-278">Azonban az adatok Azure marad tárolja, és megőrzi a megőrzési házirend beállítása adott meg.</span><span class="sxs-lookup"><span data-stu-id="814bc-278">However, the data stored in Azure remains and is retained as per the retention policy setup by you.</span></span> <span data-ttu-id="814bc-279">Régebbi pontok automatikusan van elavult.</span><span class="sxs-lookup"><span data-stu-id="814bc-279">Older points are automatically aged out.</span></span>

## <a name="remote-management"></a><span data-ttu-id="814bc-280">Távfelügyelet</span><span class="sxs-lookup"><span data-stu-id="814bc-280">Remote management</span></span>
<span data-ttu-id="814bc-281">Az Azure Backup agent, a házirendek és a adatforrások minden felügyeleti távolról végezhető el a PowerShell.</span><span class="sxs-lookup"><span data-stu-id="814bc-281">All the management around the Azure Backup agent, policies, and data sources can be done remotely through PowerShell.</span></span> <span data-ttu-id="814bc-282">A távolról felügyelt számítógép kell megfelelően elő kell készíteni.</span><span class="sxs-lookup"><span data-stu-id="814bc-282">The machine that will be managed remotely needs to be prepared correctly.</span></span>

<span data-ttu-id="814bc-283">Alapértelmezés szerint a WinRM szolgáltatás kézi indításra van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="814bc-283">By default, the WinRM service is configured for manual startup.</span></span> <span data-ttu-id="814bc-284">Az indítási típust kell beállítani *automatikus* és kell indítani a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="814bc-284">The startup type must be set to *Automatic* and the service should be started.</span></span> <span data-ttu-id="814bc-285">Győződjön meg arról, hogy a WinRM szolgáltatás fut, az állapot tulajdonság értékének kell *futtató*.</span><span class="sxs-lookup"><span data-stu-id="814bc-285">To verify that the WinRM service is running, the value of the Status property should be *Running*.</span></span>

```
PS C:\> Get-Service WinRM

Status   Name               DisplayName
------   ----               -----------
Running  winrm              Windows Remote Management (WS-Manag...
```

<span data-ttu-id="814bc-286">PowerShell távoli eljáráshívás kell konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="814bc-286">PowerShell should be configured for remoting.</span></span>

```
PS C:\> Enable-PSRemoting -force
WinRM is already set up to receive requests on this computer.
WinRM has been updated for remote management.
WinRM firewall exception enabled.

PS C:\> Set-ExecutionPolicy unrestricted -force
```

<span data-ttu-id="814bc-287">A számítógép most távolról felügyelhetők - az ügynök telepítésének indítása.</span><span class="sxs-lookup"><span data-stu-id="814bc-287">The machine can now be managed remotely - starting from the installation of the agent.</span></span> <span data-ttu-id="814bc-288">A következő parancsfájl például másolja át az ügynököt a távoli számítógépre, és telepíti azt.</span><span class="sxs-lookup"><span data-stu-id="814bc-288">For example, the following script copies the agent to the remote machine and installs it.</span></span>

```
PS C:\> $dloc = "\\REMOTESERVER01\c$\Windows\Temp"
PS C:\> $agent = "\\REMOTESERVER01\c$\Windows\Temp\MARSAgentInstaller.exe"
PS C:\> $args = "/q"
PS C:\> Copy-Item "C:\Downloads\MARSAgentInstaller.exe" -Destination $dloc - force

PS C:\> $s = New-PSSession -ComputerName REMOTESERVER01
PS C:\> Invoke-Command -Session $s -Script { param($d, $a) Start-Process -FilePath $d $a -Wait } -ArgumentList $agent $args
```

## <a name="next-steps"></a><span data-ttu-id="814bc-289">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="814bc-289">Next steps</span></span>
<span data-ttu-id="814bc-290">További információ az Azure biztonsági mentés a Windows Server vagy Windows-ügyfélen lásd:</span><span class="sxs-lookup"><span data-stu-id="814bc-290">For more information about Azure Backup for Windows Server/Client see</span></span>

* [<span data-ttu-id="814bc-291">Az Azure Backup bemutatása</span><span class="sxs-lookup"><span data-stu-id="814bc-291">Introduction to Azure Backup</span></span>](backup-introduction-to-azure-backup.md)
* [<span data-ttu-id="814bc-292">Windows-kiszolgálók biztonsági mentését</span><span class="sxs-lookup"><span data-stu-id="814bc-292">Back up Windows Servers</span></span>](backup-configure-vault.md)
